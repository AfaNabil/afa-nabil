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








