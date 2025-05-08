# 🔐 Lab 4: Implementing Cryptography with Python

**Name** : Muhammad Afa'Nabil Bin Afanizam     **My partner** : Haziq

---

## 🧠 Objectives

This lab covers:

- Symmetric Encryption using AES
- Asymmetric Encryption using RSA
- Hashing using SHA-256
- Digital Signatures using RSA

---

## 🔨 Tools
Here I'm using `Visual Studio Code` for my platform to run python code.

> How to use `VS Code`?

#### Step 1 :
install python from extensions

#### Step 2 :
Open terminal in vscode and install `pycryptodome`.

```bash
pip install pycryptodome
```
### ✅ Task 1: Symmetric Encryption (AES)

Here is my python code.
AES_encrypted coding
```py
from Crypto.Cipher import AES
from Crypto.Hash import SHA256
from Crypto.Random import get_random_bytes
import base64

def get_key(password):
    """Derive a 32-byte AES key from the password using SHA-256."""
    hasher = SHA256.new()
    hasher.update(password.encode('utf-8'))
    return hasher.digest()

def pad(data):
    """Add PKCS7 padding."""
    pad_len = 16 - (len(data) % 16)
    return data + bytes([pad_len] * pad_len)

def encrypt(plaintext, password):
    try:
        key = get_key(password)
        iv = get_random_bytes(16)
        cipher = AES.new(key, AES.MODE_CBC, iv)
        padded_plaintext = pad(plaintext.encode('utf-8'))
        ciphertext = cipher.encrypt(padded_plaintext)
        encrypted = base64.b64encode(iv + ciphertext).decode('utf-8')
        return encrypted
    except Exception as e:
        return f"❌ Encryption failed: {e}"

if __name__ == "__main__":
    plaintext = input("Enter the plaintext message: ")
    password = input("Enter the password: ")
    result = encrypt(plaintext, password)
    print(f"\n🔐 Encrypted (Base64):\n{result}")
```
AES_decrypted coding
```py
from Crypto.Cipher import AES
from Crypto.Hash import SHA256
import base64

def get_key(password):
    """Derive a 32-byte AES key from the password using SHA-256."""
    hasher = SHA256.new()
    hasher.update(password.encode('utf-8'))
    return hasher.digest()

def unpad(data):
    """Remove PKCS7 padding."""
    pad_len = data[-1]
    return data[:-pad_len]

def decrypt(encrypted_b64, password):
    try:
        key = get_key(password)
        encrypted = base64.b64decode(encrypted_b64)
        iv = encrypted[:16]
        ciphertext = encrypted[16:]
        cipher = AES.new(key, AES.MODE_CBC, iv)
        padded_plaintext = cipher.decrypt(ciphertext)
        plaintext = unpad(padded_plaintext).decode('utf-8')
        return plaintext
    except Exception as e:
        return f"❌ Decryption failed: {e}"

if __name__ == "__main__":
    encrypted_b64 = input("Enter the Base64 ciphertext: ")
    password = input("Enter the password: ")
    result = decrypt(encrypted_b64, password)
    print(f"\n🔓 Decrypted Plaintext:\n{result}")
```
#### Encrypt the plaintext :

Haziq will encrypt a secret message using his key then give to me.

![image](https://github.com/user-attachments/assets/34a2391f-e694-4e0f-9fcf-0a9776bf750a)

#### Decrypt the ciphertext :

Now let's decrypt the ciphertext.
![image](https://github.com/user-attachments/assets/567a0e10-011c-4af6-ae8e-1e97dbecc4d7)

### 💡 Explanation

- `Generate key pair` – creates a private and public key.
- `Encrypt with public key` – only the private key can decrypt this.
- `Decrypt with private key` – gets the original message back.

### ✅ Task 2: Asymmetric Encryption (RSA)

### 🔐 What is RSA?

RSA is a method for encrypting and decrypting data using two keys:


Here is my python code.
- RSA_key_generator
```py
from Crypto.PublicKey import RSA
import base64

def generate_rsa_keys():
    # Generate RSA key pair (2048-bit key)
    key = RSA.generate(2048)
    
    # Extract the public and private keys
    private_key = key.export_key()
    public_key = key.publickey().export_key()
    
    # Convert keys to Base64
    private_key_b64 = base64.b64encode(private_key).decode('utf-8')
    public_key_b64 = base64.b64encode(public_key).decode('utf-8')
    
    return public_key_b64, private_key_b64

if __name__ == "__main__":
    public_key_b64, private_key_b64 = generate_rsa_keys()
    
    print("Public Key (Base64):")
    print(public_key_b64)
    
    print("\nPrivate Key (Base64):")
    print(private_key_b64)
```
- RSA_Encrypt
```py
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP
import base64

def encrypt_with_public_key(public_key_b64, plaintext):
    # Decode the Base64-encoded public key
    public_key_der = base64.b64decode(public_key_b64)
    public_key = RSA.import_key(public_key_der)
    
    # Create cipher with OAEP padding
    cipher = PKCS1_OAEP.new(public_key)
    
    # Encrypt the plaintext
    encrypted_data = cipher.encrypt(plaintext.encode('utf-8'))
    
    # Return Base64-encoded ciphertext
    return base64.b64encode(encrypted_data).decode('utf-8')

if __name__ == "__main__":
    # Get Base64 public key from user input
    print("\nEnter Base64-encoded RSA public key: ")
    public_key_b64 = input().strip()
    
    # Get plaintext to encrypt
    print("\nEnter plaintext to encrypt: ")
    plaintext = input().strip()
    
    # Encrypt and display result
    try:
        ciphertext_b64 = encrypt_with_public_key(public_key_b64, plaintext)
        print("\nEncrypted Data (Base64): ")
        print(ciphertext_b64)
    except Exception as e:
        print(f"\nError during encryption: {e}")
```
- RSA_Decrypt
```py
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP
import base64

def decrypt_rsa(ciphertext_b64, private_key_b64):
    try:
        # Decode the Base64 encoded private key and ciphertext
        private_key_bytes = base64.b64decode(private_key_b64)
        ciphertext_bytes = base64.b64decode(ciphertext_b64)
        
        # Import the private key from the decoded bytes
        private_key = RSA.import_key(private_key_bytes)
        
        # Create the cipher object using the private key and OAEP scheme
        cipher = PKCS1_OAEP.new(private_key)
        
        # Decrypt the ciphertext
        plaintext_bytes = cipher.decrypt(ciphertext_bytes)
        
        # Decode the plaintext bytes to string
        plaintext = plaintext_bytes.decode('utf-8')
        
        return plaintext
    except Exception as e:
        return f"❌ Decryption failed: {e}"

if __name__ == "__main__":
    # Input: ciphertext and private key in Base64 format
    ciphertext_b64 = input("Enter the Base64 ciphertext: ")
    private_key_b64 = input("\nEnter the Base64 private key: ")
    
    # Decrypt and print the result
    decrypted_message = decrypt_rsa(ciphertext_b64, private_key_b64)
    print(f"\n🔓 Decrypted Message:\n{decrypted_message}")
```
#### generate key pairs :
I will create a key which is Public key and Private key
![image](https://github.com/user-attachments/assets/9649044a-7ae8-4bd4-980d-21b9c3e42c56)

Private key
```bash
LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb3dJQkFBS0NBUUVBcjBiSndmU1UvR3BDYjdaVk0wTC9CcGlXZmdKUjFXa1JhclFha011dUxRY090RzJtCkVWUDE5dmhTbnNNNFRoSWhRVXRlRzZnSDlwVkVqVk56MUZpdHZURko1L09EUVROSm13aVlRTjRTTzM0Zk51a1UKMjZ4bFE4WVJxQU5jMkRjM0pYT2d4L3g2aFV2OFRSUC9BRXI3MG12Sy8yNzM5WnFrZmlKNjFJNldTaFJhajVNcgpNWDFTVm1VNkZlUWlFSGNSUW14YnBDV0FBK2N4SkpFNmFpWVJjTXFJemdTUGJ2S1J3bWJaa3ZSL3A3N1pQYVFLCjZvUHM0MDhZcHRXd3NybE92aGF3aVl6c0NEeHdxbXUvN2FBbDVFZU9RR1gxRi9yeHlpdktPVW12Wkk0RDVKbUwKUzZGSGdya2thSE9KRUw1RWRqWmg0YllhcUZ2WWJLWWhKdXpNM3dJREFRQUJBb0lCQUFENlBwYlAwK0xpZnROYgpxM2I1T1MzaXZQZ2Y4Qy9tQVp4Y084ZXg4eHM2TGxsUVFUeFlnUWNyRkY3aFROZVduMHZiYlQ5dlBQZ2hsZnZKCnl0d1UwSWM5RnFIdUNEM2RXTnc5L0lka0ZhcnlSWjlMUm53QkpxRnRvZHlUaERtS2VVckNyZE5mczVrNXRzTUIKU1F0SjM4U3BLRDByYzhPUlpMaDFYY1VBdEpMZUxzOFUxQ201ak9JNHhSSGVPSnpFeXFRNnNWS1pPVERLK3FzZwpSeElES2hheG9oSzNLb1I3WXF1Z2J1cjgyZ2R3Sm9ROWJ3WGtmMWF6MFJReDlOQmpYWUVuS3NsUGRsbXFNUUo4CmFuWjRzdUFvbjdpRmxSQitDY2MvSjBPK2RYL21TUzR1eitNMXNkdGx4ckJEelNSVXpTa2lobzczQzVsc1NLMHkKK2dYZFhGRUNnWUVBdWVRbkxmQUp4bnpuK2pWUU10clN6S0RrQXFXQ0ViUytZenpZcTFjR0UxVmZTOHcrbnVCcwpjR05uS2QrMUQza0hCV3VpN0RnMW1CaTExQU5QcEZyYzMwT2QzZG5qWDhxbXhmUlhXM2FVbE9aV2MxTnNnR1YxCmwvVVpRTzF1SjFSaFFDZ0Z4RXJrOGxZNVNPTDlDdE4xRlY4YS9kYTB6RzR1OHMxMHhlSTYvMjhDZ1lFQThXSEkKUmJYcmx6YzhtNGpTb29XT1JKM0w1cCt6Vm52ZDR6R202VGw3clBWeEFUcFd3dTBsdDVuQ3VDa1VBRU9heXNxdgpWb0EyV3ZTRHRXV0RQWHNiTW9lbzEvUjZnZjdlWXlVbGxwK09UTTFEZk9HT080L0hXYUg5aEIxNVRhMlJONjBzCmF1STF6cUZmN2JRTUdKWkhGcDdiS3ZkU0tHMWpHK3BoTGRwUVVaRUNnWUFraTJuNGFmeEIvVVJOdURWd0NmenYKSmFJdHN3UURIeGxld1JPSGU3QkNGTUtMVURmdlhBZDFnTlZETWl1U1F5VFNOL3JyblpGTHVXL2xvbEVCNnFzSgphVzJiczc0emtzOStreTZwV0lLVlUwTWpvUDd2cndLVFFTZ1p6NU9jUDhadXFtK1h5M0YvQXR4NEs4ZTdYUjdqCmVMTUJ2RGdIQ0pKS1FZSDNkU1pNR1FLQmdIT0N1Wll5Z25Vd3A3NCtjWjM0ajZaaVpPc09UWUg3Y0ttQ1h6K0kKbEppbU05ZkVsVFJlekFmYWV3WXFFam8ySCtNZnIxeVRxbTlJUjZjTTF3c2EyRjhMOEU2MzNkQkxxUnlFOEVIdQpST2lYNmMzZkduT0NVSUdpTlhGOXNVanM0R3RtWWJVY1gyZHNiajJRUmFHV2VKcG9sclV1K1pBREdNU0pHc3dZCmh3c2hBb0dCQUt2M3BXVjcxVWIrQlFwWmNKek02OXVQU1JXYmwzditJdWI5WEVhbGlwY05GeWwrUUI5UStadGkKaFpjWmhJWlU1SndDUEpuQytqQ3oxUk92WGJxeThPbkYvaW41WHJlbk9Oc1BWeHFnc1dBSzRKR1V5dlV4Z2VPdQpWbE1pZDcweUM0NlJEL05OV0tSckhTZzYyNmc2ZWxtc1UwSWxnc3pEYlN4Z3FJWFdNL09jCi0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0t
```
Public key
```bash
LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUFyMGJKd2ZTVS9HcENiN1pWTTBMLwpCcGlXZmdKUjFXa1JhclFha011dUxRY090RzJtRVZQMTl2aFNuc000VGhJaFFVdGVHNmdIOXBWRWpWTnoxRml0CnZURko1L09EUVROSm13aVlRTjRTTzM0Zk51a1UyNnhsUThZUnFBTmMyRGMzSlhPZ3gveDZoVXY4VFJQL0FFcjcKMG12Sy8yNzM5WnFrZmlKNjFJNldTaFJhajVNck1YMVNWbVU2RmVRaUVIY1JRbXhicENXQUErY3hKSkU2YWlZUgpjTXFJemdTUGJ2S1J3bWJaa3ZSL3A3N1pQYVFLNm9QczQwOFlwdFd3c3JsT3ZoYXdpWXpzQ0R4d3FtdS83YUFsCjVFZU9RR1gxRi9yeHlpdktPVW12Wkk0RDVKbUxTNkZIZ3Jra2FIT0pFTDVFZGpaaDRiWWFxRnZZYktZaEp1ek0KM3dJREFRQUIKLS0tLS1FTkQgUFVCTElDIEtFWS0tLS0t
```

#### Encrypt the plaintext :
Haziq will Encrypt the plain-text to cipher-text using my public key that i create and then send back to me 
![image](https://github.com/user-attachments/assets/874d250f-da07-42fa-8e9b-c88b0abccf30)

#### Decrypt the ciphertext :
I will Decrypt the cipher-text using my private key to see the plain-text
![image](https://github.com/user-attachments/assets/e9c2955b-2f7e-4def-8bc4-9752a6907d8e)

### ✅ Task 3: Hashing (SHA-256)

### 🔐 What is SHA-256?

SHA-256 is a one-way hashing algorithm that turns data into a fixed-size string. It's commonly used to verify data integrity (not for encryption/decryption).

Here is my python code.
```py
import hashlib

def sha256_hash(text):
    # Encode the text to bytes
    text_bytes = text.encode('utf-8')
    
    # Create SHA-256 hash object and update with bytes
    hash_object = hashlib.sha256()
    hash_object.update(text_bytes)
    
    # Get the hexadecimal digest of the hash
    hash_hex = hash_object.hexdigest()
    return hash_hex

if __name__ == "__main__":
    plaintext = input("Enter the text to hash with SHA-256: ")
    hashed = sha256_hash(plaintext)
    print(f"\n🔑 SHA-256 Hash:\n{hashed}")
```

### Hashing the message :
```bash
Cryptography Lab by Muhammad Afa'Nabil [bel]
```
![image](https://github.com/user-attachments/assets/e07866de-3263-4517-8778-6e54d11778fc)

### Edit message :
Just add what ever you want 
### example ` -`

```bash
Cryptography Lab by Muhammad Afa'Nabil [bel] -
```

![image](https://github.com/user-attachments/assets/87b1a651-2e73-4e22-8280-27926b7d0877)

### Result :
First Hash : Cryptography Lab by Muhammad Afa'Nabil [bel]
```bash
9d4b8878093d01c74a153720f0f4459d20aeb70c90b37fd04174df439864986e
```
Second Hash: Cryptography Lab by Muhammad Afa'Nabil [bel] -
```bash
a2c22949501f24026e14a8e8c899dc5ec1c5bca1d88f65df8fc7fdac67453d91
```
### 🌪️ What is the Avalanche Effect?
When you change even 1 character (or 1 bit) in the input, the entire hash output changes drastically — like a chain reaction.

### 🔍 Why Does It Happen?

Hash functions are designed so that:

- Tiny changes in input produce completely different hashes
- You can’t guess the input from the hash
- It’s impossible to predict how the output will change

This makes hash functions very secure and perfect for:

- `Password storage`
- `File integrity checking`
- `Digital signatures`

---

### ✅ Task 4: Digital Signatures (RSA)

### ✍️ What is a Digital Signature?

A digital signature ensures:

- `Integrity` – the message wasn't changed.
- `Authenticity` – it was really sent by the owner of the private key.

How it works:
- The sender signs the message using their private key.
- The receiver verifies the signature using the public key.

#### Generate and sign message with private key :

I will assign a digital signature using my private key `*(TASK2)*` to the message and send both to the Haziq, Haziq will `verify` the message with digital signature.

![image](https://github.com/user-attachments/assets/4655e0f2-7a31-41bb-b98a-d9c3f1ea6b8a)

### Digital Signature key
```bash
kH/N5LE4/qCjwHq+YJcGTogbCj5CcKeV3fPryMfcnN6GiVk5AdE95h26wnhP2BitP4xp4BAoAa3imvjCdFW3vZmMLLE/iyL4nXvh7xf59cJJva6xHgd486skas+5MQ9rd4lSv7ZncFdnXRhDoaNS6FD+yMEp4/h4riC27XTKnxwg4OjbNCxsuG9jBlBsiB3GeeSkCjtb9jRcAMaAETjq1GNjfBY575BB+ko4eio8rS8CKCs7rVv+WPKpQvkPq+aPrMSFBWrFl2HRU2ksxlDykAe6G1mY0Hl7WQkWZla/8FINqRhLqGFuMGB59YDbKm6H7rl90yg5um0ztU+Gz+lZxw==
```

- Haziq will verify the message using public key *(TASK2)* with digital signature.

### Original Text

```bash
Cryptography Lab by Haziq & Afa , NWS23010020 & NWS22070268
```

![image](https://github.com/user-attachments/assets/56ad3b0c-5d58-43f0-8ddd-f36353dc33bd)

#### Edit message :

change small thing in message to see the verify result.
Just add : `!!!`
```bash
Cryptography Lab by Haziq & Afa , NWS23010020 & NWS22070268!!!
```

![image](https://github.com/user-attachments/assets/13ab62ca-d859-47e4-b76f-5ccf9cbb7f89)























