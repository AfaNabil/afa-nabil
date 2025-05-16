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

# ========== Task 1: Generate GPG Key Pair ========== #

- Start interactive GPG key generation

``gpg --full-generate-key``

![ Image 2025-05-16 at 19 48 02_09d58f12](https://github.com/user-attachments/assets/96fa7ecc-8d3b-40ce-ba9c-dd1499395c7f)

Selection :
- ``1``
- ``4096``
- ``1y``
- `` Muhammad Afa'Nabil Bin Afanizam(NWS22070268) ``
- `` mafanabil.afanizam@student.gmi.edu.my``
- ``none``

After key is created, list your keys

``gpg --list-keys``

![ Image 2025-05-16 at 19 51 04_f84cf75b](https://github.com/user-attachments/assets/d119963d-9662-4a38-9cc1-29fc07ad1b09)


- Show fingerprint of the key (replace "Your Name" with actual name used)

``gpg --fingerprint `` "Muhammad Afa'Nabil Bin Afanizam (NWS22070268)"

![ Image 2025-05-16 at 19 52 20_e802a036](https://github.com/user-attachments/assets/c4b41121-64ff-4192-97df-cb6317cdf2f5)


# ========== Task 2: Encrypt and Decrypt a File ==========

 - Create the message file

``echo "This file was encrypted by Your Name (YourStudentID)" > message.txt`` 

![Image 2025-05-16 at 19 54 08_ec7ade05](https://github.com/user-attachments/assets/1ed49a5f-0e3a-411f-8521-b4b8808790aa)

 ``Muhammad Afa'Nabil Bin Afanizam (NWS22070268)``

 - Encrypt the file with your own public key

``gpg --encrypt --recipient "Your Name" message.txt``

![Image 2025-05-16 at 19 58 13_49c9eaf8](https://github.com/user-attachments/assets/6f24be05-0c8a-4b4e-a931-b64e5a211dfb)

``Muhammad Afa'Nabil Bin Afanizam (NWS22070268)``

 - Decrypt the file and output to decrypted.txt

``gpg --output decrypted.txt --decrypt message.txt.gpg``

![Image 2025-05-16 at 19 59 30_ec0543dc](https://github.com/user-attachments/assets/9f138048-ada6-4abe-950e-745628528aea)

 - After this it will pop out like this image and it will required the password that i created just now which is RSA-key 

![Image 2025-05-16 at 19 58 45_45221c17](https://github.com/user-attachments/assets/6f4b9888-f80e-41c9-8d6b-addc9a7deaf2)


 - Verify the decrypted contents

``cat decrypted.txt``

![Image 2025-05-16 at 19 59 55_944273f5](https://github.com/user-attachments/assets/5d38d6f8-b2ab-497f-93ff-9fab021c616e)

# ========== Task 3: Sign and Verify a Message ==========

 - Create the message to be signed

``echo "I, Your Name, declare this is my work." > signed_message.txt``

![Image 2025-05-16 at 20 01 14_11620c1f](https://github.com/user-attachments/assets/e0732491-252c-446f-a70e-7e2bc9e97fde)


 - Sign the file using clear-sign format (ASCII readable)

``gpg --clearsign signed_message.txt``

![Image 2025-05-16 at 20 01 44_791d4929](https://github.com/user-attachments/assets/471c027a-ce57-4bb5-ae94-137b7996f1cf)


 - Verify the signature

``gpg --verify signed_message.txt.asc``

![Image 2025-05-16 at 20 02 24_8834875f](https://github.com/user-attachments/assets/23b21858-0d6b-44e3-9332-878b36458115)

# ========== Task 4: SSH Key Authentication ==========

For now im using Powershell

- Here are the step by step using PowerShell and Including Kali Linux

![WhatsApp Image 2025-05-16 at 21 48 15_3d2a3d1f](https://github.com/user-attachments/assets/52a06a45-4854-4b89-8928-c0ab4499ffdd)

![WhatsApp Image 2025-05-16 at 21 49 23_82a35cdf](https://github.com/user-attachments/assets/ddef6b8e-89d6-4b19-8aee-dce869caf321)

![WhatsApp Image 2025-05-16 at 21 50 07_36169caa](https://github.com/user-attachments/assets/bf2a05a7-c004-4979-a767-af6b4426de74)

![WhatsApp Image 2025-05-16 at 21 50 34_63af5964](https://github.com/user-attachments/assets/639410ee-aa44-4a16-b36c-f3b3aa35a683)

![WhatsApp Image 2025-05-16 at 21 50 50_e5a4b0ab](https://github.com/user-attachments/assets/9da64a99-def4-42c4-9d6a-822a20fda36e)

![WhatsApp Image 2025-05-16 at 21 51 11_99184a2b](https://github.com/user-attachments/assets/c82462da-c154-4de6-a33d-bebbed47fe29)

![WhatsApp Image 2025-05-16 at 21 51 25_481ffdf9](https://github.com/user-attachments/assets/05efba3b-8a95-4b25-9921-c7534ff99d9f)


















