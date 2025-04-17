# 🔐 Network Protocol Vulnerability Lab Walkthrough

**Name:** Muhammad Afa'Nabil

**Target Machine:** Metasploitable 2 (`192.168.21.137`) 



## 🎯 Objective
Explore vulnerabilities in common protocols (FTP, TELNET, SSH, HTTP) via brute-force attacks and network sniffing. Analyze security flaws and propose mitigations.

---
First of all get the ip for our victim which is metasploitable 2.

![image](https://github.com/user-attachments/assets/639d5563-1a90-4169-a179-eef502a96d92)


Use ip a command.

So here our target ip is 192.168.21.137

## 🔮 1. Enumeration

### Tool Used:
- Nmap

### Command:
```bash
nmap -sS -sV 192.168.21.137
```
use the command in Kali 

![image](https://github.com/user-attachments/assets/f02c5fc4-c1b9-4f08-9e57-fee03e27a954)


### Discovered Open Ports & Services:
- FTP (21)
- SSH (22)
- TELNET (23)
- HTTP (80)
- Others (Samba, MySQL, etc.)


WHAT IS SAMBA PORT?
-----------------------------------------------------------
Samba is an open-source implementation of theSMB (Server Message Block) protocol
used for file and printer sharing between systems, especially between Linux and Windows.
Samba only can attack with other device if the device have Samba  too

HOW TO USE IT?
----------------------
enum4linux -U 192.168.21.137

![image](https://github.com/user-attachments/assets/18861261-ef22-4ca4-9a8b-36f91a5524cb)


### Usernames Found:
From enum4linux :
- msfadmin
- postgres
- root
- ftp
- telnetd
- user

---

## 🔧 2. Brute Force Attacks

### ✅ FTP
**Tool:** Medusa  
**Command:**
```bash
medusa -h 192.168.21.137 -u msfadmin -P passwords.txt -M ftp
```
we need to create password.txt which will be list of password we use to bruteforce.

![image](https://github.com/user-attachments/assets/e72a5d01-fb91-450a-8550-0ba0aab657c2)


![image](https://github.com/user-attachments/assets/f28d01af-de0f-412c-ac89-e42ffcbd71eb)


now we can use the command to bruteforce ftp port :)

![image](https://github.com/user-attachments/assets/c2fe7833-fe3b-4247-8e8d-3c7bf4d1b643)


### ✅ TELNET
**Tool:** Hydra

**Command:**
```bash
hydra -l msfadmin -P passwords.txt 192.168.21.137 telnet
```

as Hydra is often more reliable with Telnet we will use it.

![image](https://github.com/user-attachments/assets/a092aa26-8c09-4679-8cdc-4747246d26e7)


we can see from the result that the valid password is "msfadmin" ><

### ✅ SSH
**Tool:** NetExec  
**Command:**
```bash
nxc ssh 192.168.21.137 -u username.txt -p passwords.txt
```
SSH Brute Force using NetExec
✅ Step 1: Create a new list for username and save it as username.txt

![image](https://github.com/user-attachments/assets/d6a57cb2-f73a-44c0-a858-db59640bd26a)


now we can use the command to bruteforce SSH 


![image](https://github.com/user-attachments/assets/48454594-5d48-4a86-a6a1-54f138ed1c03)


![image](https://github.com/user-attachments/assets/9c54b258-c931-45d9-927d-7ce6f558e478)


#### 2.2: HTTP

## 🎯 Objective

   1. Use Burp Intruder to automate brute force attacks against an HTTP login page.
   2. Configure Intruder to test a list of usernames and passwords.
   3. Analyze the results to identify successful logins.

   #### 🚶‍♂️‍➡️ The Process:

   1. Login to DVWA (D*** Vulnerable Web App!)
      - Login from attacker device:
        
        ![Image](https://github.com/user-attachments/assets/c2ea8395-14be-4823-9a45-95516cbaeaeb)


   2. Our Testing Page
      - Send a Hmphhh in login form:
        
         ![Image](https://github.com/user-attachments/assets/ebf74b3c-a15b-473b-97a0-152a4b580635)

         > make sure you enable the intercept in Burp Suite 

   3. Intercept Request
      - Right click and send to Intruder for bruteforce
      ![Image](https://github.com/user-attachments/assets/4eeea6ac-d364-4ec8-acec-c283e74b1f4e)
)

   4. Select Attack Type:
      - Choose Cluster Bomb Attack:
      - ![Image](https://github.com/user-attachments/assets/2e4db5cb-375e-4802-b26c-fd041aee08e9)
)

   5. Setting up Payload:
      - Select potential-username.txt as a first payload for username:
      - ![Image](https://github.com/user-attachments/assets/32e901c0-2a53-4524-976a-59d25092b2b3)
)

      - Select password.txt as a second payload for password:
      - ![Image](https://github.com/user-attachments/assets/6d954322-8ad9-4d39-a23c-f8d18f5fdb8d)
)

   6. Analyze the results to identify successful logins:
      - Click the lenght column for sort it:
      ![Image](https://github.com/user-attachments/assets/53c8dc74-90dd-4f74-a0ef-a551bf66923c)
)
         > We can see here payload "admin" "password" with most lenght

      - Open the respone tab:
      ![Image](https://github.com/user-attachments/assets/12035d45-334b-4474-8349-5253f1c74c0d)
)
         > As we can see here, we have successful attempt

   7. Login as admin:
      - Go back to the login form and try login:
      ![Image](https://github.com/user-attachments/assets/d653531b-0a9a-435c-b520-3b4f17d09319)

)
         
---

###  Sniff Network Traffic

## 🎯 Objective

1. Use the recovered credentials to log in to the respective services.
2. Use Wireshark or tcpdump to capture and analyze network traffic during the session.
3. Identify which protocols transmit data in plaintext and which use encryption.
4. Provide evidence (e.g., screenshots) to prove which protocols are secure and which are not.

   ---

   #### 🚶‍♂️‍➡️ The Process:
      
   a. Identify Protocol trasnmit data in plaintext:

      1. Open Wireshark on Attack device (Kali Linux)
      2. Choose eth0 (or channel that connect with metasploitable network)
      3. Start the scan
      4. Simulate the login attempt using real username:password we got for TELNET on Attacker Machine
      5. Go back to Wireshark:

         - Result:
            - ![Image](https://github.com/user-attachments/assets/e4627e41-3106-4d3f-b4c7-002fca38e44b)
)

      6. Sort the lenght column.
      7. Right click to the most lenght value > Follow > TCP Stream

         - Result:
            - ![Image](https://github.com/user-attachments/assets/318e589d-36e3-4035-9851-6ed4e901547c)
)
               > Here we capture data that transmit using telnet, which is not encrypted.

---
