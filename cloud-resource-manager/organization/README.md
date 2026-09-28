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



Organization Viewerroles/resourcemanager.organizationViewerResource Manager組織の属性閲覧   Folder Viewerroles/resourcemanager.folderViewerResource Managerフォルダ構造の閲覧   Browser (追加推奨)roles/resourcemanager.browserResource Managerコンソール上でのリソース階層一覧表示（ブラウズ）Organization Role Viewerroles/iam.organizationRoleViewerIAMカスタムロール等の参照   Security Reviewerroles/iam.securityReviewerIAM組織全体の IAM ポリシー・安全性の確認   Cloud Asset Viewerroles/cloudasset.viewerCloud Asset Inventory全リソース・IAM の横断検索   Org Policy Viewerroles/orgpolicy.policyViewerResource Manager (Org Policy)組織ポリシー（制約）の閲覧   Security Center Findings Viewerroles/securitycenter.findingsViewerSecurity Command Center脆弱性・セキュリティ検出事項の確認   Private Log Viewerroles/logging.privateLogViewerCloud Logging監査ログを含むログの参照   Compute Network Viewerroles/compute.networkViewerCompute Engineネットワーク・VPC構成の確認   Quota Viewerroles/servicemanagement.quotaViewerService Managementクォータ・割り当ての参照   

















```
以下は権限が強い
Viewer (閲覧者) | **roles/reader** | Project | [リンク](https://cloud.google.com/iam/docs/understanding-roles#viewer)
```
