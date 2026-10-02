# Cloud SQL を定期起動・停止するサンプル

```mermaid
flowchart LR
  startJob["Cloud Scheduler<br/>sql-cycling-start<br/>平日 8:00<br/>0 8 * * 1-5 Asia/Tokyo<br/>引数 targetState: ALWAYS<br/>実行 SA: sa-schd-sql-cycling"]
  stopJob["Cloud Scheduler<br/>sql-cycling-stop<br/>毎日 20:00<br/>0 20 * * * Asia/Tokyo<br/>引数 targetState: NEVER<br/>実行 SA: sa-schd-sql-cycling"]
  workflow["Cloud Workflows<br/>sql-cycling<br/>実行 SA: sa-workflows-sql-cycling<br/>roles/cloudsql.admin"]
  sql["Cloud SQL<br/>プロジェクト: _SQL_PROJECT_ID<br/>インスタンス: _SQL_INSTANCE_ID<br/>Secret: workflows-sql-cycling-envyaml<br/>変更項目: activationPolicy"]

  startJob -->|"POST executions.create"| workflow
  stopJob -->|"POST executions.create"| workflow
  workflow -->|"sqladmin.instances.patch"| sql
```