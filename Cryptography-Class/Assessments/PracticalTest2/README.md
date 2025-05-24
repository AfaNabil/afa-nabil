![image](https://github.com/user-attachments/assets/de779e6d-b460-4f10-8a45-bb7b72e552c3)

![image](https://github.com/user-attachments/assets/091b4545-51c3-4bee-9d41-ac930e722eda)

![image](https://github.com/user-attachments/assets/fa10b3a8-97c0-4a46-9b12-0a21bbf2270c)

![image](https://github.com/user-attachments/assets/d08d2fac-d8c4-4932-b260-459cc486abbc)

![image](https://github.com/user-attachments/assets/457fe05f-49d9-4035-94ee-6bfc07463443)

![image](https://github.com/user-attachments/assets/bc3c5df7-49c2-47ea-9c05-a43c3e7cbde9)

![image](https://github.com/user-attachments/assets/fed84d4f-1a3d-48ae-af57-573ebf4e883e)

![image](https://github.com/user-attachments/assets/bd6b6fdb-091c-4439-b3a3-3830c40cad13)

```py
from Crypto.Cipher import AES
from hashlib import sha256
import os

# Derive the same key as the ransomware
KEY_SUFFIX = "SecretHere"
KEY_STR = f"Bukan{KEY_SUFFIX}"
KEY = sha256(KEY_STR.encode()).digest()[:16]

def unpad(data):
    pad_len = data[-1]
    return data[:-pad_len]

def decrypt_file(filepath):
    with open(filepath, "rb") as f:
        ciphertext = f.read()
    cipher = AES.new(KEY, AES.MODE_ECB)
    padded_plaintext = cipher.decrypt(ciphertext)
    plaintext = unpad(padded_plaintext)
    output_path = filepath.replace(".enc", "_decrypted.txt")
    with open(output_path, "wb") as f:
        f.write(plaintext)
    print(f"Decrypted: {filepath} -> {output_path}")

def main():
    folder = "."
    for file in os.listdir(folder):
        if file.endswith(".enc"):
            decrypt_file(os.path.join(folder, file))

if __name__ == "__main__":
    main()
```

