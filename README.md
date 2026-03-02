# cme-kms-cloud-storage
Managing CMEK with cloud Storage and Cloud KMS

cme-kms-cloud-storage/
│
├── README.md
├── architecture-diagram.png
│
├── docs/
│   ├── project-overview.md
│   ├── cme-kms-best-practices.md
│   ├── troubleshooting-guide.md
│
├── scripts/
│   ├── create-kms-keyring.sh
│   ├── create-kms-key.sh
│   ├── create-cme-bucket.sh
│   ├── upload-encrypted-object.sh
│   ├── rotate-key.sh
│
├── exercises/
│   ├── exercise-1-upload-download.md
│   ├── exercise-2-key-rotation.md
│   ├── exercise-3-audit-log-check.md# 🔐 Customer-Managed Encryption Keys (CMEK) Lab – GCP

This project demonstrates **encrypting Cloud Storage data using Customer-Managed Encryption Keys (CMEK)** from Cloud Key Management Service (KMS).

## Objectives
- Create a KMS key ring and crypto key
- Create Cloud Storage buckets encrypted with CMEK
- Upload and download objects using the CMEK
- Rotate keys and verify encryption
- Audit access to keys and buckets

## Lab Architecture
- **Cloud KMS**: Hosts key ring & crypto keys
- **Cloud Storage Bucket**: Stores sensitive data
- **VM or Cloud Functions**: Optional, used for upload/download automation

## Benefits
- Data ownership control
- Meet compliance/regulatory requirements (e.g., HIPAA, PCI-DSS)
- Enables secure key rotation and audit
- Avoids reliance on Google-managed keys
# 🔐 CMEK Security Best Practices

1. **Least Privilege Access**
   - Only allow service accounts or users with necessary roles:
     - `roles/cloudkms.cryptoKeyEncrypterDecrypter`
     - `roles/storage.objectAdmin` (for CMEK buckets)

2. **Key Rotation**
   - Enable automatic key rotation or rotate manually periodically
   - Test rotation workflow in non-production first

3. **Audit & Monitoring**
   - Enable Cloud Audit Logs for KMS key usage
   - Monitor Cloud Storage logs for access to sensitive objects

4. **Regional Considerations**
   - Keep KMS key and bucket in the same region for performance
   - Plan multi-region replication if needed#!/bin/bash
# Usage: ./create-kms-keyring.sh PROJECT_ID LOCATION KEY_RING_NAME

PROJECT_ID=$1
LOCATION=$2
KEY_RING_NAME=$3

gcloud kms keyrings create $KEY_RING_NAME \
    --location $LOCATION \
    --project $PROJECT_ID

echo "KMS Key Ring $KEY_RING_NAME created in $LOCATION"#!/bin/bash
# Usage: ./create-kms-key.sh PROJECT_ID LOCATION KEY_RING_NAME KEY_NAME

PROJECT_ID=$1
LOCATION=$2
KEY_RING_NAME=$3
KEY_NAME=$4

gcloud kms keys create $KEY_NAME \
    --location $LOCATION \
    --keyring $KEY_RING_NAME \
    --purpose encryption \
    --rotation-period 30d \
    --project $PROJECT_ID
#!/bin/bash
# Usage: ./create-cme-bucket.sh PROJECT_ID BUCKET_NAME LOCATION KMS_KEY

PROJECT_ID=$1
BUCKET_NAME=$2
LOCATION=$3
KMS_KEY=$4  # Format: projects/PROJECT_ID/locations/LOCATION/keyRings/KEY_RING/cryptoKeys/KEY

gcloud storage buckets create $BUCKET_NAME \
    --location $LOCATION \
    --project $PROJECT_ID \
    --encryption-key $KMS_KEY

echo "CMEK Bucket $BUCKET_NAME created and encrypted with $KMS_KEY"#!/bin/bash
# Usage: ./upload-encrypted-object.sh BUCKET_NAME FILE_PATH

BUCKET_NAME=$1
FILE_PATH=$2

gsutil cp $FILE_PATH gs://$BUCKET_NAME/

echo "$FILE_PATH uploaded to gs://$BUCKET_NAME/ using CMEK"

echo "KMS Crypto Key $KEY_NAME created with 30-day rotation"






  
   - 
- 


