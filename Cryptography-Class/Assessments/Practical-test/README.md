# 🛡️ Practical Test – Cybersecurity Lab (Kali Linux)

**Student Name:** Muhammad Afa'Nabil Bin Afanizam  
**Student ID:** NWS22070268
**Course:** Cryptography / Cybersecurity  
**Platform:** Kali Linux  

---

## ✅ Task Overview

| Task | Title                          | Status |
|------|--------------------------------|--------|
| 1    | GPG Key Generation             | ✅ Completed |
| 2    | File Encryption and Decryption | ✅ Completed |
| 3    | Digital Signing and Verification | ✅ Completed |
| 4    | SSH Key Authentication         | ✅ Completed |
| 5    | Hash Cracking Challenge        | 🚧 In Progress |

# Start interactive GPG key generation

``gpg --full-generate-key``

![ Image 2025-05-16 at 19 48 02_09d58f12](https://github.com/user-attachments/assets/96fa7ecc-8d3b-40ce-ba9c-dd1499395c7f)

Selection :
- ``1``
- ``4096``
- ``1y``
- `` Muhammad Afa'Nabil Bin Afanizam(NWS22070268) ``
- `` mafanabil.afanizam@student.gmi.edu.my``
- ``none``


# After key is created, list your keys
gpg --list-keys

# Show fingerprint of the key (replace "Your Name" with actual name used)
gpg --fingerprint "Your Name"



