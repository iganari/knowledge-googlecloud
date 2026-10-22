# Private Service Connect | Network attachments

## 概要

TBD

## [ユースケース]

### 別のプロジェクトの Datastream から PSC に繋ぐ場合

```
### PSC 側
export _psc_attachment_gc_pj_id='PSC がある Google Cloud Project ID'
export _psc_attachment_region='asia-northeast1'
export _psc_attachment_name='PSC attachment の名前'

### Datastream 側
export _datastream_gc_pj_id='Datastream がある Google Cloud Project ID'
```

- Datastream のプロジェクトにて Datastream テナントプロジェクト ID の取得

```
gcloud beta datastream private-connections create my-private-conn \
  --display-name="my-private-conn" \
  --location=asia-northeast1 \
  --network-attachment="projects/${_psc_attachment_gc_pj_id}/regions/${_psc_attachment_region}/networkAttachments/${_psc_attachment_name}" \
  --validate-only \
  --project=${_datastream_gc_pj_id}
```

- 実行結果例

```
$ gcloud datastream private-connections create my-private-conn \
  --display-name="my-private-conn" \
  --location=asia-northeast1 \
  --network-attachment="projects/${_psc_gc_pj_id}/regions/asia-northeast1/networkAttachments/${_psc_name}" \
  --validate-only \
  --project=${_datastream_gc_pj_id}
done: true
metadata:
  '@type': type.googleapis.com/google.cloud.datastream.v1.OperationMetadata
  apiVersion: v1
  createTime: '2026-10-22T13:53:31.304724467Z'
  endTime: '2026-10-22T13:53:31.329135383Z'
  requestedCancellation: false
  target: projects/xxxxxxxx/locations/xxxxxxxx/privateConnections/xxxxxxxx
  validationResult:
    validations:
    - message:
      - message: Assigned tenant project. Add this to your Network Attachment's allowed
          producer list.
        metadata:
          tenant_project_id: qd7bb77138aa7faf2p-tp    ### <--- これ
      state: PASSED
  verb: create
name: projects/xxxxxxxx/locations/xxxxxxxx/operations/operation-xxxxxxxx
response:
  '@type': type.googleapis.com/google.cloud.datastream.v1.PrivateConnection
  createTime: '2026-10-22T13:53:31.302589424Z'
  name: projects/xxxxxxxx/locations/xxxxxxxx/privateConnections/xxxxxxxx
  updateTime: '2026-10-22T13:53:31.302589424Z'
```
