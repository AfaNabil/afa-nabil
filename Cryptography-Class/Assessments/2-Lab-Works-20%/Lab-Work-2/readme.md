# Lab 2: Cracking Weak Password Hashes & Exploiting Poor Authentication

**Time Allocated:** 3 Hours  
**Total Marks:** 15  

---

## A. Objectives

1. Find and exploit weak crypto in database login and password storage.
2. Do offline cracking of password hashes found in the database.
3. Learn from real-world crypto mistakes and suggest better ways.
4. Write a report in GitHub (Markdown) and show a short demo.

---

## B. Lab Tasks

### 1. Service Enumeration and Initial Access

**Nmap Scan on Target Machine**

To discover open services on the Metasploitable2 target machine (`192.168.21.137`), I used a version scan with `nmap`.  
Command Used:
```
nmap -sV 192.168.21.137
```
