# Comprehensive AWS Data Protection & Encryption Topics

## Table of Contents
1. AWS Key Management Service (KMS)
2. Customer Managed Keys (CMKs) vs. AWS Managed Keys
3. AWS Secrets Manager vs. AWS Systems Manager Parameter Store
4. AWS Certificate Manager (ACM)
5. AWS CloudHSM for Hardware-Based Encryption
6. AWS Private Certificate Authority (CA)
7. Transparent Data Encryption (TDE) for RDS
8. Amazon S3 Encryption (SSE-S3, SSE-KMS, SSE-C)
9. S3 Object Lock and Bucket Policies
10. AWS Nitro Enclaves for Sensitive Workloads
11. AWS Macie for Sensitive Data Discovery
12. AWS Data Lifecycle and Retention Policies
13. AWS Backup and Encryption Considerations
14. AWS Transfer Family Security (SFTP, FTPS, FTP)
15. AWS Secure Storage with Amazon EBS Encryption

---

## 1. AWS Key Management Service (KMS)
**Theory:** AWS KMS provides encryption key management, allowing users to create, manage, and control cryptographic keys securely.

**UI Steps:**
- Navigate to AWS KMS Console.
- Click "Create Key" and select the key type.
- Define key policies and IAM permissions.
- Enable automatic rotation if needed.

**CLI Steps:**
```sh
aws kms create-key --description "MyEncryptionKey"
aws kms enable-key-rotation --key-id alias/MyKeyAlias
```

## 2. Customer Managed Keys (CMKs) vs. AWS Managed Keys
**Theory:** CMKs offer user-defined policies and more control, while AWS Managed Keys are automatically handled by AWS with limited configuration options.

**UI Steps:**
- Navigate to AWS KMS Console.
- Compare "Customer Managed Keys" and "AWS Managed Keys" in the key list.
- Define policies and access restrictions for CMKs.

**CLI Steps:**
```sh
aws kms list-keys
aws kms describe-key --key-id alias/MyCMK
```

## 3. AWS Secrets Manager vs. AWS Systems Manager Parameter Store
**Theory:** AWS Secrets Manager stores sensitive credentials with automatic rotation, whereas Parameter Store provides configuration management with encryption support.

**UI Steps:**
- Navigate to AWS Secrets Manager or Systems Manager Parameter Store.
- Click "Store a new secret" or "Create Parameter".
- Define key-value pairs and enable encryption.

**CLI Steps:**
```sh
aws secretsmanager create-secret --name MySecret --secret-string "MyPassword123"
aws ssm put-parameter --name "MyParameter" --value "MyConfigValue" --type SecureString
```

## 4. AWS Certificate Manager (ACM)
**Theory:** AWS ACM manages SSL/TLS certificates for securing network communication.

**UI Steps:**
- Navigate to AWS ACM Console.
- Click "Request a Certificate".
- Choose validation method (DNS or Email).

**CLI Steps:**
```sh
aws acm request-certificate --domain-name example.com --validation-method DNS
```

## 5. AWS CloudHSM for Hardware-Based Encryption
**Theory:** AWS CloudHSM provides dedicated cryptographic hardware for secure key storage.

**UI Steps:**
- Navigate to AWS CloudHSM Console.
- Click "Create Cluster" and configure the hardware security module.

**CLI Steps:**
```sh
aws cloudhsm create-cluster --hsm-type hsm1.medium
```

## 6. AWS Private Certificate Authority (CA)
**Theory:** AWS Private CA enables the creation of internal certificate authorities.

**UI Steps:**
- Navigate to AWS Private CA.
- Click "Create a CA" and define the certificate details.

**CLI Steps:**
```sh
aws acm-pca create-certificate-authority --certificate-authority-configuration file://ca-config.json
```

## 7. Transparent Data Encryption (TDE) for RDS
**Theory:** TDE encrypts data at rest in RDS databases for enhanced security.

**UI Steps:**
- Navigate to AWS RDS Console.
- Enable encryption during DB instance creation.

**CLI Steps:**
```sh
aws rds modify-db-instance --db-instance-identifier mydbinstance --storage-encrypted
```

## 8. Amazon S3 Encryption (SSE-S3, SSE-KMS, SSE-C)
**Theory:** S3 supports encryption using AWS KMS keys, S3-managed keys, and customer-provided keys.

**UI Steps:**
- Navigate to Amazon S3.
- Click "Bucket Properties" → "Default Encryption".

**CLI Steps:**
```sh
aws s3api put-bucket-encryption --bucket my-bucket --server-side-encryption-configuration file://encryption-config.json
```

## 9. S3 Object Lock and Bucket Policies
**Theory:** S3 Object Lock prevents accidental deletion by enforcing retention policies.

**UI Steps:**
- Navigate to S3.
- Enable "Object Lock" while creating the bucket.

**CLI Steps:**
```sh
aws s3api put-object-lock-configuration --bucket my-bucket --object-lock-configuration file://lock-config.json
```


#### **10. AWS Nitro Enclaves for Sensitive Workloads**
**Theory:** Nitro Enclaves provide isolated compute environments for secure data processing.

**UI Steps:**
1. Navigate to the AWS EC2 Console.
2. Click "Launch Instance" and select an instance type that supports Nitro Enclaves.
3. Configure instance settings and enable Nitro Enclaves under "Advanced Details".
4. Launch the instance and configure Enclave-aware applications.

**CLI Steps:**
```sh
aws ec2 modify-instance-attribute --instance-id i-xxxx --enclave-options Enabled=true
```

---

#### **11. AWS Macie for Sensitive Data Discovery**
**Theory:** Macie uses machine learning to identify and protect sensitive data in S3.

**UI Steps:**
1. Navigate to AWS Macie Console.
2. Click "Enable Macie" and define the scope of analysis.
3. Select S3 buckets to be monitored.
4. Review findings and adjust policies as needed.

**CLI Steps:**
```sh
aws macie2 create-classification-job --job-name MyJob --s3-job-definition file://macie-job.json
```

---

#### **12. AWS Data Lifecycle and Retention Policies**
**Theory:** Data lifecycle policies help manage data expiration and archiving.

**UI Steps:**
1. Navigate to Amazon S3 Console.
2. Select the bucket and open "Management" tab.
3. Click "Create lifecycle rule" and define the transition and expiration policies.
4. Save and apply the rule.

**CLI Steps:**
```sh
aws s3api put-bucket-lifecycle-configuration --bucket my-bucket --lifecycle-configuration file://lifecycle.json
```

---

#### **13. AWS Backup and Encryption Considerations**
**Theory:** AWS Backup automates backups while supporting encryption for data security.

**UI Steps:**
1. Navigate to AWS Backup Console.
2. Click "Create Backup Plan" and define backup rules.
3. Select the AWS resources to be backed up.
4. Configure encryption settings and save the plan.

**CLI Steps:**
```sh
aws backup create-backup-plan --backup-plan file://backup-plan.json
```

---

#### **14. AWS Transfer Family Security (SFTP, FTPS, FTP)**
**Theory:** Secure file transfer to AWS using Transfer Family services.

**UI Steps:**
1. Navigate to AWS Transfer Family Console.
2. Click "Create Server" and select the required protocol (SFTP, FTPS, FTP).
3. Configure authentication and security settings.
4. Review and launch the server.

**CLI Steps:**
```sh
aws transfer create-server --protocols SFTP
```

---

#### **15. AWS Secure Storage with Amazon EBS Encryption**
**Theory:** EBS encryption ensures secure data storage on Amazon EC2 instances.

**UI Steps:**
1. Navigate to AWS EC2 Console.
2. Click "Create Volume" and enable "Encrypt this volume".
3. Choose an encryption key from AWS KMS.
4. Attach the volume to an EC2 instance.

**CLI Steps:**
```sh
aws ec2 create-volume --encrypted --size 100 --availability-zone us-east-1a
```

