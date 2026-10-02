# Recommend Role

以下の Role を組織ノードにつけておくと、全体像が把握しやすくなる

Title = Role Name (ロールネーム) | ID | Used in | 公式ドキュメントのリンク
:- |:- |:- |:-
Organization Viewer (組織閲覧者) | **roles/resourcemanager.organizationViewer** | Resource Manager | [リンク](https://cloud.google.com/iam/docs/understanding-roles#resourcemanager.organizationViewer)
Organization Role Viewer (組織のロール閲覧者) | **roles/iam.organizationRoleViewer** | Roles | [リンク](https://cloud.google.com/iam/docs/understanding-roles#iam.organizationRoleViewer)
Cloud Asset Viewer (TBD) | **roles/cloudasset.viewer** | TBD | TBD
Org Policy Viewer (TBD) | **roles/orgpolicy.policyViewer** | TBD | TBD
Security Center Findings Viewer (TBD) | **roles/securitycenter.findingsViewer** | TBD | TBD
Private Log Viewer (TBD) | **roles/logging.privateLogViewer** | TBD | TBD
Compute Network Viewer (TBD) | **roles/compute.networkViewer** | TBD | TBD
Folder Viewer (フォルダ閲覧者) | **roles/resourcemanager.folderViewer** | Resource Manager | [リンク](https://cloud.google.com/iam/docs/understanding-roles#resourcemanager.folderViewer)
Security Reviewer (セキュリティ審査担当者) | **roles/iam.securityReviewer** | IAM | [リンク](https://cloud.google.com/iam/docs/understanding-roles#iam.securityReviewer)

Quota Viewer (割り当て閲覧者) | **roles/servicemanagement.quotaViewer** | Service Management | [リンク](https://cloud.google.com/iam/docs/understanding-roles#servicemanagement.quotaViewer)


Title = Role Name (ロールネーム) | ID | Used in | 役割・補足
:- | :- | :- | :-
Organization Viewer | roles/resourcemanager.organizationViewer | Resource Manager | 組織の属性閲覧
Folder Viewer | roles/resourcemanager.folderViewer | Resource Manager | フォルダ構造の閲覧
Browser (追加推奨) | roles/resourcemanager.browser | Resource Manager | コンソール上でのリソース階層一覧表示（ブラウズ）
Organization Role Viewer | roles/iam.organizationRoleViewer | IAM | カスタムロール等の参照
Security Reviewer | roles/iam.securityReviewer | IAM | 組織全体の IAM ポリシー・安全性の確認
Cloud Asset Viewer | roles/cloudasset.viewer | Cloud Asset Inventory | 全リソース・IAM の横断検索
Org Policy Viewer | roles/orgpolicy.policyViewer | Resource Manager (Org Policy) | 組織ポリシー（制約）の閲覧
Security Center Findings Viewer | roles/securitycenter.findingsViewer | Security Command Center | 脆弱性・セキュリティ検出事項の確認
Private Log Viewer | roles/logging.privateLogViewer | Cloud Logging | 監査ログを含むログの参照
Compute Network Viewer | roles/compute.networkViewer | Compute Engine | ネットワーク・VPC構成の確認
Quota Viewer | roles/servicemanagement.quotaViewer | Service Management | クォータ・割り当ての参照













```
以下は権限が強い
Viewer (閲覧者) | **roles/reader** | Project | [リンク](https://cloud.google.com/iam/docs/understanding-roles#viewer)
```
