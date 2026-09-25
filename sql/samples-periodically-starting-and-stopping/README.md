# 定期起動・停止するサンプル

## 概要


## 比較表

構成パターン | 冪等性(事前状態チェック) | 完了待機・タイムアウト検知 | エラー時通知(Slack等) | 実装・保守コスト | 実行コスト | おすすめ度
:- | :- | :- | :- | :- | :- | :-
Cloud Scheduler + Cloud Workflows | 容易(APIで状態を取得しスキップ可能) | 容易(Workflowsの標準ループ/述語で完了まで待機) | 容易(http.post で Webhook 通知)低(YAML定義のみ・サーバーレス) | 極めて安い(無料枠大) | ★★★★★
Cloud Scheduler + Cloud Functions | 容易(コード内で get() して判定) | 可能(コード内で Sleep & Retry ポーリング) | 容易(Slack SDK や HTTP ライブラリ利用) | 中(コード・依存関係の管理が必要) | 極めて安い(無料枠大) | ★★★★☆
Cloud Scheduler + Cloud Build | 可能(gcloud CLI + bash で制御)やや煩雑(bash スクリプト内で loop 記述) | やや難(gcloud/curl でメッセージ生成) | 高(Docker 起動の遅延・Build 設定管理) | やや高い(ビルド時間課金) | ★★☆☆☆
Cloud Scheduler (API直叩き) | 不可(常に PATCH を発行) | 不可(リクエスト送信のみ。処理完了は見ない) | 難(Monitoring アラート経由のみ) | 極小(GUI/Terraform のみ) | 最安 | ★★☆☆☆