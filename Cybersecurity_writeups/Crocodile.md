# HTB - Crocodile

**Difficulty:** Very Easy 
**Category:** [ Web / Ftp]  
**Date:** [10/03/2026]  
**Status:** ✅ Completed

---

## Summary
Crocodile is a very easy Linux machine which showcases the dangers of misconfigured authentication and sensitive data exposure. A vulnerable FTP server instance is misconfigured to allow anonymous authentication and upon enumerating the server, sensitive files can be found containing cleartext credentials. Enumerating and fuzzing the website will reveal a hidden login endpoint where the previously acquired credentials can be used to gain access to the admin panel.
---

## Enumeration

### Nmap Scan

```bash
nmap -sC -sV -p- --min-rate 5000 10.129.1.15
```

**Results:**
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.10.15.57
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Smash - Bootstrap Business Template
Service Info: OS: Unix
```


---

## Exploitation

### Step 1 

```bash
I use ftp -a 10.129.1.15 
to access the files with anonymous mode
```

**Output:**
```
Connected to 10.129.1.15.
220 (vsFTPd 3.0.3)
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```

### Step 2 

I decided to use GoBuster to find a .php file to give me admin access to the page. And I found the php file called /login.php
So I put the following on the host IP page: 10.129.14.14/login.php



```
gobuster dir -u http://10.129.14.14 -w /usr/share/wordlists/dirb/common.txt -x php,html

Output

|Gobuster v3.8
|by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)

|[+] Url:                     http://10.129.14.14
|[+] Method:                  GET
|[+] Threads:                 10
|[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
|[+] Negative Status codes:   404
|[+] User Agent:              gobuster/3.8
|[+] Extensions:              php,html
|[+] Timeout:                 10s
Starting gobuster in directory enumeration mode

/.htaccess            (Status: 403) [Size: 277]
/.hta.php             (Status: 403) [Size: 277]
/.hta.html            (Status: 403) [Size: 277]
/.hta                 (Status: 403) [Size: 277]
/.htaccess.php        (Status: 403) [Size: 277]
/.htaccess.html       (Status: 403) [Size: 277]
/.htpasswd.php        (Status: 403) [Size: 277]
/.htpasswd.html       (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/assets               (Status: 301) [Size: 313] [--> http://10.129.14.14/assets/]
/config.php           (Status: 200) [Size: 0]
/css                  (Status: 301) [Size: 310] [--> http://10.129.14.14/css/]
/dashboard            (Status: 301) [Size: 316] [--> http://10.129.14.14/dashboard/]
/fonts                (Status: 301) [Size: 312] [--> http://10.129.14.14/fonts/]
/index.html           (Status: 200) [Size: 58565]
/index.html           (Status: 200) [Size: 58565]
/js                   (Status: 301) [Size: 309] [--> http://10.129.14.14/js/]
/login.php            (Status: 200) [Size: 1577]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
```
---

## Flag

```
HTB{c7110277ac44d78b6a9fff2232434d16}
```

---

## What I Learned

- How to use gobuster and access a admin page with the /login.php + password that I find using the ftp
- Ftp i learned more how to use "get" and another commands


---

## References

- https://github.com/OJ/gobuster
- https://nmap.org/
