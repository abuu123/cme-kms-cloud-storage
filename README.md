# cme-kms-cloud-storage
Managing CMEK with cloud Storage and Cloud KMS
cme-kms-cloud-storage/
├── README.md
├── architecture-diagram.png
├── docs/
│   ├── project-overview.md
│   ├── cme-kms-best-practices.md
│   └── troubleshooting-guide.md
├── scripts/
│   ├── create-kms-keyring.sh
│   ├── create-kms-key.sh
│   ├── create-cme-bucket.sh
│   ├── upload-encrypted-object.sh
│   └── rotate-key.sh
├── exercises/
│   ├── exercise-1-upload-download.md
│   ├── exercise-2-key-rotation.md
│   └── exercise-3-audit-log-check.md

# Create Key Ring
./scripts/create-kms-keyring.sh my-project us-central1 my-keyring

# Create Crypto Key with rotation
./scripts/create-kms-key.sh my-project us-central1 my-keyring my-key

# Create CMEK-encrypted bucket
./scripts/create-cme-bucket.sh my-project my-bucket us-central1 \
projects/my-project/locations/us-central1/keyRings/my-keyring/cryptoKeys/my-key

# Upload object
./scripts/upload-encrypted-object.sh my-bucket ./sensitive-data.txt

## 🏗 Lab Architecture

```text
+-----------------+          +-----------------+
| Cloud Storage   | <------> | Cloud KMS       |
| Bucket (CMEK)   |          | Key Ring & Key  |
+-----------------+          +-----------------+
         ^
         |
     gsutil / Scripts
         |
     VM / Cloud Functions






  
   - 
- 


