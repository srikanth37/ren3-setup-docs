# Storage Configuration

ReN3 supports multiple storage backends for file and data storage.
Configure the following flags in your service (server or ingestor) according to your chosen storage provider.

## Storage Location

Set the `storage_location` flag to specify which storage backend to use:

* `local`: Uses local disk storage on the host machine.
* `gcp`: Uses Google Cloud Storage.
* `s3`: Uses Amazon AWS S3.
* `az`: Uses Azure Blob Storage with account key authentication.
* `azclientid`: Uses Azure Blob Storage with service principal (App Registration) authentication.

---

## Local Storage (`local`)

Local storage can be configured differently depending on deployment type:

### 1. VM / Bare Metal Deployment

Use the `rn3bp_data_folder` flag to specify the path for ReN3 data.
If not set, it defaults to `/opt/rn3bp/rn3-data`.

| Flag Name           | Type   | Description                                                    |
| ------------------- | ------ | -------------------------------------------------------------- |
| `rn3bp_data_folder` | string | Path to the ReN3 data folder (default: `/opt/rn3bp/rn3-data`). |

**Example:**

```ini
storage_location=local
rn3bp_data_folder=/mnt/ren3-data
```

---

### 2. Kubernetes / Docker Deployment (Containers)

When running ReN3 as containers:

* **Do not set `rn3bp_data_folder`.**
* Instead, mount a **shared PVC (Kubernetes)** or **shared volume (Docker)** at `/opt/rn3bp/rn3-data`.
* The **same PVC/volume must be mounted in both the ReN3 server and ingestor** so both can read/write the same files.


**Kubernetes Example (shared PVC):**

```yaml
volumes:
  - name: ren3-data
    persistentVolumeClaim:
      claimName: ren3-data-pvc

# Server pod/container
volumeMounts:
  - name: ren3-data
    mountPath: /opt/rn3bp/rn3-data

# Ingestor pod/container
volumeMounts:
  - name: ren3-data
    mountPath: /opt/rn3bp/rn3-data
```

**Docker Compose Example (shared volume):**

```yaml
volumes:
  ren3-data:

services:
  server:
    image: ren3-server:latest
    volumes:
      - ren3-data:/opt/rn3bp/rn3-data

  ingestor:
    image: ren3-ingestor:latest
    volumes:
      - ren3-data:/opt/rn3bp/rn3-data
```

**PersistentVolumeClaim (PVC) Example**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ren3-data-pvc
spec:
  accessModes:
    - ReadWriteMany   # required so multiple pods (server + ingestor) can mount
  resources:
    requests:
      storage: 50Gi   # adjust size as needed
  storageClassName: nfs-client   # or your RWX-capable storage class
```

---

### Notes:

* **`ReadWriteMany (RWX)` is required** because both server and ingestor need simultaneous read/write access.
* The actual `storageClassName` depends on your Kubernetes cluster:

  * `nfs-client` (NFS dynamic provisioner)
  * `efs-sc` (AWS EFS)
  * `azurefile-csi` (Azure Files)
  * `filestore-csi` (GCP Filestore)
* If your cluster’s default storage class does not support RWX (common with standard block volumes), you’ll need an RWX-capable backend (NFS, EFS, Azure Files, etc.).


## Google Cloud Storage (`gcp`)

| Flag Name                 | Type   | Description                                                     |
| ------------------------- | ------ | --------------------------------------------------------------- |
| `gcs_project_id`          | string | Google Cloud project ID.                                        |
| `gcs_serviceaccount_file` | string | Path to the Google service account JSON credentials file.       |
| `gcs_bucket_name`         | string | Name of the Google Cloud Storage bucket.                        |
| `gcm_serviceaccount_file` | string | (Optional) Path to Google Cloud Messaging service account file. |

**Setup:**

1. Create a bucket in your GCP project.
2. Create a service account and download its JSON key file.
3. Provide `project_id`, `bucket_name`, and service account file in your config.

**Example:**

```ini
storage_location=gcp
gcs_project_id=your-gcp-project-id
gcs_serviceaccount_file=/path/to/service-account.json
gcs_bucket_name=your-bucket-name
```

---

## Azure Blob Storage (`az` and `azclientid`)

| Flag Name                         | Type   | Description                                   |
| --------------------------------- | ------ | --------------------------------------------- |
| `azureblob_account_name`          | string | Azure storage account name.                   |
| `azureblob_account_key`           | string | Account key (required for `az`).              |
| `azureblob_account_endpoint`      | string | Azure storage account endpoint URL.           |
| `azureblob_active_container_name` | string | Container name.                               |
| `azure_signedurl_expiration`      | int    | Expiration time (in seconds) for signed URLs. |
| `azureblob_client_id`             | string | Client ID (required for `azclientid`).        |
| `azureblob_client_secret`         | string | Client secret (required for `azclientid`).    |
| `azureblob_tenant_id`             | string | Tenant ID (required for `azclientid`).        |

**Setup:**

* For `az`: Use account key.
* For `azclientid`: Register an app in Azure AD, assign access, and use Client ID, Secret, and Tenant ID.

**Examples:**

Using **account key**:

```ini
storage_location=az
azureblob_account_name=your-storage-account
azureblob_account_key=your-account-key
azureblob_account_endpoint=https://your-storage-account.blob.core.windows.net/
azureblob_active_container_name=your-container
azure_signedurl_expiration=3600
```

Using **service principal**:

```ini
storage_location=azclientid
azureblob_account_name=your-storage-account
azureblob_account_endpoint=https://your-storage-account.blob.core.windows.net/
azureblob_active_container_name=your-container
azureblob_client_id=your-app-client-id
azureblob_client_secret=your-app-client-secret
azureblob_tenant_id=your-tenant-id
azure_signedurl_expiration=3600
```

---

## AWS S3 Storage (`s3`)

| Flag Name                     | Type   | Description                                   |
| ----------------------------- | ------ | --------------------------------------------- |
| `aws_access_key_id`           | string | AWS access key ID.                            |
| `aws_secret_access_key`       | string | AWS secret access key.                        |
| `aws_region`                  | string | AWS region where the bucket is located.       |
| `aws_s3_bucket`               | string | Bucket name.                                  |
| `aws_s3_signedurl_expiration` | int    | Expiration time (in seconds) for signed URLs. |

**Setup:**

1. Create an S3 bucket in AWS.
2. Create an IAM user with S3 access.
3. Provide `access_key_id`, `secret_access_key`, `region`, and `bucket` in config.

**Example:**

```ini
storage_location=s3
aws_access_key_id=your-access-key-id
aws_secret_access_key=your-secret-access-key
aws_region=us-west-2
aws_s3_bucket=your-bucket-name
aws_s3_signedurl_expiration=3600
```

---

### References

**Local Storage (VM / Containers)**

* Kubernetes PVC: [https://kubernetes.io/docs/concepts/storage/persistent-volumes/](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
* RWX Volumes / StorageClasses: [https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaims](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaims)
* Docker Volumes: [https://docs.docker.com/storage/volumes/](https://docs.docker.com/storage/volumes/)

**Google Cloud Storage (GCP)**

* [Creating Buckets](https://cloud.google.com/storage/docs/creating-buckets)
* [Service Account Keys](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)

**Azure Blob Storage**

* [Azure Storage Blobs](https://learn.microsoft.com/en-us/azure/storage/blobs/)
* [Service Principal Authentication](https://learn.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)

**AWS S3**

* [Creating Buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html)
* [IAM User for S3 Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html)

**Optional / Advanced**

* NFS / RWX storage for Kubernetes: [https://kubernetes.io/docs/concepts/storage/storage-classes/](https://kubernetes.io/docs/concepts/storage/storage-classes/)
* AWS EFS for RWX: [https://docs.aws.amazon.com/efs/latest/ug/gs-step-two-create-efs-resources.html](https://docs.aws.amazon.com/efs/latest/ug/gs-step-two-create-efs-resources.html)
* Azure Files for RWX: [https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share)

---
