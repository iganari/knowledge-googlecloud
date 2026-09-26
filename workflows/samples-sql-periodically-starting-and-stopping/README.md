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

### Service Account に Role を付与

- Workflow に Cloud SQL 管理者権限を付与（起動・停止・ステータス確認に必要）

```
# Cloud SQL の操作・管理権限
gcloud projects add-iam-policy-binding ${_workflow_gc_pj_id} \
  --member="serviceAccount:${WORKFLOW_SA_EMAIL}" \
  --role="roles/cloudsql.admin"

# 実行ログ出力権限
gcloud projects add-iam-policy-binding ${_workflow_gc_pj_id} \
  --member="serviceAccount:${WORKFLOW_SA_EMAIL}" \
  --role="roles/logging.logWriter"
```

- Scheduler 用 SA への権限付与

```
# Workflows の実行を呼び出す権限のみを付与
gcloud projects add-iam-policy-binding ${_workflow_gc_pj_id} \
  --member="serviceAccount:${_scheduler_sa_email}" \
  --role="roles/workflows.invoker"
```

### Workflows のデプロイ

```
gcloud workflows deploy sql-periodically-starting-and-stopping \
  --source=sql-periodically-starting-and-stopping.yaml \
  --location=asia-northeast1 \
  --service-account=${_workflow_sa_email} \
  --project ${_workflow_gc_pj_id}
```