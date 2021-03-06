---
title: "Encryption at Rest"
linkTitle: "Encryption at Rest"
weight: 10
slug: encryption-at-rest
---

Cortex supports data encryption at rest for some storage backends.

## Blocks storage

The [blocks storage](../blocks-storage/_index.md) supports encryption at rest only for S3. Other storage backends may support encryption at rest configuring it directly at the storage level.

### S3

The Cortex S3 client used by the blocks storage supports the following SSE types:

- [SSE-S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)
- [SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)

### `s3_sse_config`

The `s3_sse_config` configures the S3 server-side encryption.

```yaml
sse:
  # Enable AWS Server Side Encryption. Supported values: SSE-KMS, SSE-S3.
  # CLI flag: -s3.sse.type
  [type: <string> | default = ""]

  # KMS Key ID used to encrypt objects in S3
  # CLI flag: -s3.sse.kms-key-id
  [kms_key_id: <string> | default = ""]

  # KMS Encryption Context used for object encryption. It expects JSON formatted
  # string.
  # CLI flag: -s3.sse.kms-encryption-context
  [kms_encryption_context: <string> | default = ""]
```

#### S3 per-tenant overrides

The S3 client used by the blocks storage supports S3 SSE config overrides on a per-tenant basis, using the [runtime configuration file](../configuration/arguments.md#runtime-configuration-file).
The following settings can ben overridden for each tenant:

- **`s3_sse_type`**<br />
  S3 server-side encryption type. It must be set to enable the SSE config override for a given tenant.
- **`s3_sse_kms_key_id`**<br />
  S3 server-side encryption KMS Key ID. Ignored if the SSE type override is not set or the type is not `SSE-KMS`.
- **`s3_sse_kms_encryption_context`**<br />
  S3 server-side encryption KMS encryption context. If unset and the key ID override is set, the encryption context will not be provided to S3. Ignored if the SSE type override is not set or the type is not `SSE-KMS`.

## Chunks storage

The [chunks storage](../chunks-storage/_index.md) supports encryption at rest only for S3. Other storage backends may support encryption at rest configuring it directly at the storage level.

### S3

The Cortex S3 client used by the chunks storage supports the following SSE types:

- [SSE-S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)
- [SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html)

The server-side encryption in the S3 client used by the chunks storage can be configured similarly to the blocks storage, but **per-tenant overrides are not supported**.
