# 🔐 Network Protocol Vulnerability Lab Walkthrough

**Name:** Muhammad Afa'Nabil

**Target Machine:** Metasploitable 2 (`192.168.21.137`) 



## 🎯 Objective
Explore vulnerabilities in common protocols (FTP, TELNET, SSH, HTTP) via brute-force attacks and network sniffing. Analyze security flaws and propose mitigations.

---
First of all get the ip for our victim which is metasploitable 2.

![image](https://github.com/)

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

![image](https://github.com/)

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
enum4linux -U 192.168.100.131

![image](https://github.com/)

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

![image](https://github.com/)

![image](https://github.com/)

now we can use the command to bruteforce ftp port :)

![image](https://github.com/)

### ✅ TELNET
**Tool:** Hydra

**Command:**
```bash
hydra -l msfadmin -P passwords.txt 192.168.21.137 telnet
```

as Hydra is often more reliable with Telnet we will use it.

![image](https://github.com/)

we can see from the result that the valid password is "msfadmin" ><

### ✅ SSH
**Tool:** NetExec  
**Command:**
```bash
nxc ssh 192.168.21.137 -u username.txt -p passwords.txt
```
SSH Brute Force using NetExec
✅ Step 1: Create a new list for username and save it as username.txt

![image](https://github.com/)


now we can use the command to bruteforce SSH 


![image](https://github.com/)


![image](https://github.com/)

#### 2.2: HTTP

## 🎯 Objective

   1. Use Burp Intruder to automate brute force attacks against an HTTP login page.
   2. Configure Intruder to test a list of usernames and passwords.
   3. Analyze the results to identify successful logins.

   #### 🚶‍♂️‍➡️ The Process:

   1. Login to DVWA (D*** Vulnerable Web App!)
      - Login from attacker device:
         ![Image](assets/screenshot/)

   2. Our Testing Page
      - Send a placeholder in login form:
         ![Image](assets/screenshot//)
         > make sure you enable the intercept in Burp Suite 

   3. Intercept Request
      - Right click and send to Intruder for bruteforce
         ![Image]()

   4. Select Attack Type:
      - Choose Cluster Bomb Attack:
         ![Image]()

   5. Setting up Payload:
      - Select potential-username.txt as a first payload for username:
      - ![Image]()

      - Select password.txt as a second payload for password:
      - ![Image]()

   6. Analyze the results to identify successful logins:
      - Click the lenght column for sort it:
         ![Image]()
         > We can see here payload "admin" "password" with most lenght

      - Open the respone tab:
         ![Image]()
         > As we can see here, we have successful attempt

   7. Login as admin:
      - Go back to the login form and try login:
         ![Image]()
         > Nice job buddy!

   
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
            - ![Image]()

      6. Sort the lenght column.
      7. Right click to the most lenght value > Follow > TCP Stream

         - Result:
            - ![Image]()
               > Here we capture data that transmit using telnet, which is not encrypted.

---
