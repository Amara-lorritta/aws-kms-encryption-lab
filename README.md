## **AWS KMS Encryption Lab**  
**Hands-on Project: Data Protection Using Encryption**

## **Overview**
This lab demonstrates how to protect sensitive data using **AWS Key Management Service (KMS)** and the **AWS Encryption CLI**.  
You’ll create an encryption key, encrypt plaintext files into ciphertext, and then decrypt them back into readable data, ensuring end-to-end confidentiality and integrity.


## **Objectives & Learning Outcomes**
After completing this lab, I was able to:
- Create and configure an **AWS KMS encryption key**
- Install and use the **AWS Encryption CLI**
- Encrypt plaintext files into ciphertext
- Decrypt ciphertext files back to plaintext securely
- Understand symmetric key encryption and decryption workflows


## **Architecture**

<img width="1536" height="400" alt="fcb06a53-6ce3-4834-b546-5d030a2cc6b9" src="https://github.com/user-attachments/assets/870d512c-6bcf-489b-8c88-ab47d7f7b432" />


**Architecture Summary:**
- **User** connects to the **AWS Console**  
- **KMS Key (MyKMSKey)** is created for symmetric encryption  
- **File Server EC2 instance** with IAM role accesses KMS securely  
- **AWS Encryption CLI** installed to encrypt and decrypt text files  
- Encrypted data stored and later decrypted on the same server


## **Commands and Steps**

```bash
# Step 1: Connect to the File Server
aws ssm start-session --target <instance-id>

# Step 2: Configure AWS CLI credentials
aws configure

# Step 3: Install AWS Encryption CLI
pip3 install aws-encryption-sdk-cli
export PATH=$PATH:/home/ssm-user/.local/bin

# Step 4: Create text files
touch secret1.txt secret2.txt secret3.txt
echo 'TOP SECRET 1!!!' > secret1.txt
cat secret1.txt

# Step 5: Encrypt file using KMS Key
keyArn=<YOUR_KMS_ARN>
aws-encryption-cli --encrypt \
  --input secret1.txt \
  --wrapping-keys key=$keyArn \
  --metadata-output ~/metadata \
  --encryption-context purpose=test \
  --commitment-policy require-encrypt-require-decrypt \
  --output ~/output/.

# Step 6: Verify encryption
ls output
cat output/secret1.txt.encrypted

# Step 7: Decrypt the file
aws-encryption-cli --decrypt \
  --input secret1.txt.encrypted \
  --wrapping-keys key=$keyArn \
  --commitment-policy require-encrypt-require-decrypt \
  --encryption-context purpose=test \
  --metadata-output ~/metadata \
  --max-encrypted-data-keys 1 \
  --buffer \
  --output .

cat secret1.txt.encrypted.decrypted
```

## **Screenshots**

**KMS Key Created**

<img width="1417" height="421" alt="Screenshot 2025-10-16 074445" src="https://github.com/user-attachments/assets/b91fae55-1158-4def-af54-7414546ad77d" />


**Encryption CLI Installation**
<img width="1131" height="302" alt="Screenshot 2025-10-16 080734" src="https://github.com/user-attachments/assets/89c185aa-6880-4b52-a6f4-46b5a74825e2" />


**Encrypted File**
<img width="1571" height="251" alt="Screenshot 2025-10-16 081643" src="https://github.com/user-attachments/assets/695522d8-c2bc-4f69-bd92-e4e25f30cb01" />



**Decrypted File**
<img width="1244" height="150" alt="Screenshot 2025-10-16 081825" src="https://github.com/user-attachments/assets/2d805e23-ebcd-43e0-8bcc-20f430e05f32" />


## **Tools Used**

AWS Key Management Service (KMS)

AWS Encryption CLI

AWS Identity and Access Management (IAM)

Amazon EC2 (Session Manager)

AWS CLI

## **What Actually Happened** 

Created a KMS symmetric key (MyKMSKey) to encrypt and decrypt files.

Configured AWS credentials on the EC2 File Server for secure access to KMS.

Installed the AWS Encryption CLI via pip to perform file encryption/decryption.

Created three text files with mock sensitive data.

Encrypted secret1.txt using the KMS key and verified it turned into ciphertext.

Decrypted the same file back into plaintext to confirm data integrity.

## **Key Takeaways**

Symmetric keys use one key for both encryption and decryption.

The AWS Encryption CLI provides simple, CLI-based encryption operations.

AWS KMS integrates securely with EC2 through IAM roles.

Using encryption context improves security metadata tracking and auditing.

## **Author**

Amarachi Emeziem

Cloud Security Engineer 

LinkedIn: https://www.linkedin.com/in/amarachilemeziem/
