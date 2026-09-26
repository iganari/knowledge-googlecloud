# Cloud SQL を定期起動・停止するサンプル

## 定義

1. 現状確認
2. すでに指定状態なら正常終了 (冪等性)
3. 状態変更 API 発行
4. 完了までポーリング待機
5. 失敗時は Slack 通知

## 手順

- 環境変数

```
export _workflow_gc_pj_id='Your Cloud Workflow of Google Cloud ID'
export _scheduler_gc_pj_id='Your Cloud Scheduler of Google Cloud ID'
```

- API の有効化

```
gcloud services enable workflows.googleapis.com \
  cloudscheduler.googleapis.com \
  sqladmin.googleapis.com \
  --project ${_workflow_gc_pj_id}
```

- Workflows/Scheduler 用の Service Account の作成と Role を付与

```
### Cloud Workflow
export _workflow_sa_name="sa-workflow-sql"
export _workflow_sa_email="${_workflow_sa_name}@${_workflow_gc_pj_id}.iam.gserviceaccount.com"

### Cloud Scheduler
export _scheduler_sa_name="sa-schd-workflow"
export _scheduler_sa_email="${_scheduler_sa_name}@${_scheduler_gc_pj_id}.iam.gserviceaccount.com"
```

### Service Account を作成

- Workflows 用 SA の作成

```
gcloud iam service-accounts create ${WORKFLOW_SA_NAME} \
  --display-name="Service Account for Cloud Workflows" \
  --project ${_workflow_gc_pj_id}
```

- Scheduler 用 SA の作成

```
gcloud iam service-accounts create ${SCHEDULER_SA_NAME} \
  --display-name="Service Account for Cloud Scheduler" \
  --project ${_scheduler_gc_pj_id}
```

### Service Account に Role 

- Workflows に Cloud SQL 管理者権限を付与（起動・停止・ステータス確認に必要）

```
gcloud projects add-iam-policy-binding ${_workflows_gc_pj_id} \
    --member="serviceAccount:${_workflow_sa_email}" \
    --role="roles/cloudsql.admin"

# Scheduler に Workflows の呼び出し権限を付与
gcloud projects add-iam-policy-binding ${_workflows_gc_pj_id} \
    --member="serviceAccount:${_workflow_sa_email}" \
    --role="roles/workflows.invoker"

# （推奨）Workflows の実行ログを出力するための権限
gcloud projects add-iam-policy-binding ${_workflows_gc_pj_id} \
    --member="serviceAccount:${_workflow_sa_email}" \
    --role="roles/logging.logWriter"
```



```
main:
  params: [args]
  steps:
    - init:
        assign:
          - projectId: ${sys.get_env("GOOGLE_CLOUD_PROJECT_ID")}
          - instanceId: "your-sql-instance-id" # 対象のCloud SQLインスタンス名
          - targetState: ${args.targetState}   # Schedulerから渡された引数を使う
          - slackWebhookUrl: "https://hooks.slack.com/services/XXX/YYY/ZZZ"

    # 1. 現在の状態を取得（事前チェック）
    - check_current_state:
        call: googleapis.sqladmin.v1.instances.get
        args:
          project: ${projectId}
          instance: ${instanceId}
        result: instanceInfo
        
    # 2. すでにその状態であれば何もせず終了（エラーにしない）
    - skip_if_already_in_state:
        switch:
          - condition: ${instanceInfo.settings.activationPolicy == targetState}
            return: "Already in target state. Skipped."

    # 3. 起動 / 停止の指示を発行
    - update_instance:
        try:
          call: googleapis.sqladmin.v1.instances.patch
          args:
            project: ${projectId}
            instance: ${instanceId}
            body:
              settings:
                activationPolicy: ${targetState}
          result: operation
        retry: ${http.default_retry}
        except:
          as: e
          steps:
            - notify_error:
                call: http.post
                args:
                  url: ${slackWebhookUrl}
                  body:
                    text: ${" Cloud SQL 変更API呼び出し失敗: " + e.message}
                return: "Failed to update instance"

    # 4. 処理が完了するまでループで待機（タイムアウト対策）
    - wait_for_completion:
        call: sys.sleep
        args:
          seconds: 10
    - check_operation_status:
        call: googleapis.sqladmin.v1.operations.get
        args:
          project: ${projectId}
          operation: ${operation.name}
        result: opStatus
    - evaluate_operation:
        switch:
          - condition: ${opStatus.status == "DONE"}
            next: check_final_result
          - condition: ${opStatus.status != "DONE"}
            next: wait_for_completion

    # 5. Operation の最終判定
    - check_final_result:
        switch:
          - condition: ${"error" in opStatus}
            steps:
              - notify_operation_failed:
                  call: http.post
                  args:
                    url: ${slackWebhookUrl}
                    body:
                      text: ${" Cloud SQL のステータス変更処理がエラー終了しました: " + instanceId}
                  return: "Operation Error"
        return: "Successfully updated Cloud SQL state."
```