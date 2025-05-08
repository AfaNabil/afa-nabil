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
LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBbmZ3NDlSazBoTm44ZlpjVFJCaXcxdnhjUS9PTTZnMFJucWhuNzh0MWovL2dPZE14CmRCOUR4cysrM3k0bytoSXllMlhuL01Tdk1pajBjYWhMeHhzYWUycUNqSnlVQTNKY0d6Tjg2UGRWV3BQY2RQTmoKUzROZ2ErSDZ1bXh5OHRvVVgrZVU4bEpFNDZBWVFFV2NuTFlKQSsvMmtnS0trMUJxQlVsQnBDbG9Zb1dpVUdsagpQeUlqTUlJaURMM1lLSVZLeUtBbWlHTEUyZ2tkRlQyVitGVTlwejZaYk50RE4vSFBpbDZ2UkVneStzSGN0cUcxCld5WVFuQ2VjRFRCaDJCN0pldVVURXRVTzRpbmhhUG5DQnA4WVFQZVVYTHNXenpZbnl1bElQekt0L3ozTzgxRkIKR204ZjZ5dFBrUUwyaTVBV1liK056T243dlVvd09oVW04MU5BQVFJREFRQUJBb0lCQUFGNWhmK1ZzMldORWx3Swp3Ym1JUUhoVlJUZzJLUW5UUXVlWCsxWmo4QTQrelhWRXVTaTBGUUloVk04SkE0Vm9ENVFTekxKUmxMQVRiVXExClR6WEYxVDZ6TFJKS2NPQkNYRVU5dXd3Q3FRZU9LMGZsTUxkVys5cXQ4cFQwWjdOSUlWb08wNWRhZUwybU5DdS8KYXBtVTRtc292WVM1NU5qQXJxaXJlU01pNXRCS21za0k2RjdJU0MzYi9PZ3NoZ3JscjlVcUlmck9kUnI2TWErUwpKd0E3RlVZU1NPbzdZcE4vWUJOSUFpR1pJTUFFL3AzVmN2UmlTYzZUNWxsK3ZMNVkyOXJNQ1VZbUlzSkRDV3g3CnFscWpXV21TRGZHT2I2a0wraHVRTTVRQm11ZlFCWXIyaGRGUUpwNVJKaGtjc3ZibVZyUEhUM0RDS0ZaSVNDbVIKR2JPVTJRRUNnWUVBdlJHUmx3NVVXWWp2UVZyUHhqN1NURk1Ea0kySEJDcXErQTZBRjM0Rks3OGJIeTNoWXZNNgpLMWYwSUJOZVk4TnRhRDFONzNLcEIyRWpTcXBVcjI5clJFU0wrUkx3UlZUK2N4aTNnaDIzR2VZS2tMUVMwTnJSCithNGxnWDZvZjB1UGVlZ3lKYnFUUkE1TlNBZHp6TVhoTUJoRVdCWWI1ZnlrQTZ6Q1R4OVlaNkVDZ1lFQTFlbTEKTlhZYUVmMjFmOXNTNk5USXcwN1FHaU5FYUU5cGxaYkRIUVhXZ0ljaUg0a0dUUjdDNk4ySlA3M1ViU0NoU3MyZgpMNzhxMU1TMlRhbjJSazJxd0t5M2Irc0NBNmcvSUVLZkFsOC9wR3JYZFFTcXNZSk00K3ZpS1RjWmk3M2NBWkNXCkpqZ0RPVXpOSGV2R2xDQ1M3SVBhVWdscGdka1VkdVRpS1hWaWZHRUNnWUVBaDFIZHZCbTdjV0c2ckRJTTU3enEKMDBuUEVWVGFQN2N5S3R4bC9XcHExUWF3cUw0enhKaUZGNlNaeTZOUk9XSVVHamxXWUh6V1VidktnSlFzakd2WQpnRUgzVk11alFGdzJ5YlgxRWFHbS9WaFNVNE11dkdFQlRBekNOMDZwMW9JRUxLSnQwZWNabytvQWtmOFRlSXBnClJBWDZWSGx1ZWtzNk1JOGplM0haMmtFQ2dZQU52Q1BXZXU3Um1PaStmT0tKOEoremFxd1NBWkd5NE9aMnBHZUMKeFkwd1UzenRhVzd1Skx0L0dEcG56VmlHYVVIeCs2SHMvdWxSSUJCVWpFVXozbWpJVms2dzhQWUFKaDFuaE4rUQoxMnpPc2MyRHhmanZ6TjREQkhKUkV5aWd3R0FQK2FpcWE0NUpiNVQ4QnRlSlMwNGp3Wk9URU9lbFhycmhwM0NlCkdOdHFBUUtCZ1FDSFdBcDc1YVgvazJZa1FCQlV3YzhVcUp4cEJaaDFVN2FzMFA4Tk1NaG1RNWdVTk1RWWVneW0KWHNyYmc0eU9VYTg2SXovdS9PN0dHcFEzMzN0ZTBVTXRFdVNKeGtMb2htTHBmS1JIY3Fpa0I5RWQrL0lTdUl6aApaekNWUmJtMis3U0tabUgvUzNaaGpqMzlOUk11K2RSdmM4UHJCYUxmZ1ZGc1JBVHYyUG52d2c9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQ==
```
Public key
```bash
LS0tLS1CRUdJTiBQVUJMSUMgS0VZLS0tLS0KTUlJQklqQU5CZ2txaGtpRzl3MEJBUUVGQUFPQ0FROEFNSUlCQ2dLQ0FRRUF2VGtkVUtBZnVhcEJFTDF3U3c0RApVVnA2MmxHQ3hRaDZuYkxud2lHajZpTTdsWUx5Q0RGVkZaUVBvRDNCaTl0aHBzRWJkM1R2TG11bHAveTVBN0NpClVNY3lQbERBaGhpMVdFdU9TSDNLN0dDVk5HbkNCNmpVcldDclFwTy9QRzlPaTVJUnZXN3I0SlhQenJYN3R1RHkKVFBvZk9iUEdoOFo1MEdRUnNUN25TeURuQ3EvMjgybEFSSHdkN2RkQWtjV1kwWFlRRjROSUNJaE5xM21sMmZtMwpnVzN0RlpyZTQra1FlOTh4RXIxekZGell5UWtUaDdPeUVnMHl4dzRuNGxhUXBnM2xWM2tEVGpsYWo1emp4MGtCCnczZ3BNSkQzSktsL0YyU3hYcGJnVXpyYUYrZjB6YUZnZit6WTk4Nk1iRTMyWXJZeCtGa21sNmJIL1NWa3ErUVAKZlFJREFRQUIKLS0tLS1FTkQgUFVCTElDIEtFWS0tLS0t
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
![image](https://github.com/user-attachments/assets/e07866de-3263-4517-8778-6e54d11778fc)

### Edit message :
Just add what ever you want 
### example ` -`
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

I will assign a `digital signature` using my `private key`*(TASK2)* to the message and send both to the Haziq, Haziq will `verify` the message with `digital signature`.


![image](https://github.com/user-attachments/assets/4655e0f2-7a31-41bb-b98a-d9c3f1ea6b8a)



















