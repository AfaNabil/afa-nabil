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

**Commands:**
1. Haziq generate a strong random key
```bash
openssl rand -base64 32 > key.bin
```
![ Image ](https://github.com/user-attachments/assets/a4b37c10-93cc-4fe8-bfd2-1bfb1597b26f)

2. Create a plaintext message
```bash
echo "This is secret don tell anybody." > message.txt
```
![ image ](https://github.com/user-attachments/assets/9eb66937-8fdd-49b0-8d5b-02479a8b5e6a)

Haziq encrypt using AES-256-CBC
```bash
openssl enc -aes-256-cbc -salt -in message.txt -out encrypted_message.bin -pass file:./key.bin
```
> - `enc` : Encryption utility.

> - `aes-256-cbc` : Specifies AES with 256-bit key in CBC mode.

> - `salt` : Adds salt to prevent dictionary attacks.

> - `pass file` : ./key.bin: Reads key from file.

![image](https://github.com/user-attachments/assets/3ed602fb-512c-4833-b2e3-7b6ffebd4e0b)


