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
