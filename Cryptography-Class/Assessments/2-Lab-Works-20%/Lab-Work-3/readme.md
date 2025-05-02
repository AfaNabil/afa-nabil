# Lab 3: Hands-on Exploration of Cryptographic Tools with OpenSSL

## A. Objectives:

This lab introduces to OpenSSL, a widely used cryptography toolkit, for performing essential cryptographic operations such as:

1. Symmetric encryption (AES)
2. Asymmetric encryption (RSA)
3. Hashing (SHA-256)
4. Digital signatures (RSA with SHA-256)

By the end of this lab, we will be able to:

1. Encrypt and decrypt files using symmetric and asymmetric encryption.
2. Generate and verify hashes for data integrity.
3. Create and verify digital signatures.

---

## B. Lab Tasks:

**Tools Used:**
- `OpenSSL` (command line)
- Linux shell utilities (`echo`, `cat`, `diff`)
- `Google Drive` (for sending files to each other)

### Task 1: Symmetric Encryption and Decryption using AES-256-CBC
**Scenario:** Danish wants to send a confidential message to Raja using symmetric encryption.

**Goal:** Encrypt a message using a shared key (symmetric encryption) and then decrypt it with the same key.

---

### Step-by-step :

#### Step 1 :
I am a `reciever` and `Haziq` is a `sender`. He will create a plaintext and key then send to me the encrypted message.

#### Plaintext :
```bash
echo "This is a secret message for Afa from Haziq." > haziq_message.txt

Task 2


## Task 3 :Hashing and Message Integrity using SHA-256

For this task I'm do it personally.

---

### Step 1 :

Create a plaintext and hash it.

#### Command :
```bash
echo "This document must remain unchanged." > integrity_check.txt
```
![ image ](https://github.com/user-attachments/assets/2e7054d7-b1e4-42e6-af29-cc8a52395c28)

### Step 2 :
Hashing the hash usung SHA-256
```bash
openssl dgst -sha256 hash.txt
```
![ image ](https://github.com/user-attachments/assets/22397737-2776-4e78-b213-7d80263bd5d1)

- `openssl dgst`: Use OpenSSL to compute a digest (hash).

- `-sha256`: Specify the SHA-256 algorithm.

### Step 3 :
Modify the file.

#### Command :
```bash
echo "This document is now changed." > integrity_check.txt

![image](https://github.com/user-attachments/assets/5185c6a1-edaa-4050-bf68-62c86dbf9480)









