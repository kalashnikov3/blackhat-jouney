# BadStore VM Walkthrough

## Objective

Explore and exploit the BadStore virtual machine to understand its vulnerabilities and practice penetration testing skills.

## Initial Setup

* Machine IP: `192.168.56.102`
* VM is reachable and running.
* ![BadStore VM Setup](screenshots/badstore1.png)


## Recon / Enumeration

**Goal:** Find out what services the machine is running and which ports are open. This tells us where to focus our attack attempts.

### Step 1: Check if the machine is alive

Command:

```bash
ping 192.168.56.102
```

**Explanation:**

* `ping` sends a small message to the machine and waits for a reply.
* If the host replies, it’s online and we can try to interact with its services.

**Result:** Machine responded, so it is reachable.

---

### Step 2: Scan for open ports and services

Command:

```bash
nmap -sC -sV 192.168.56.102
```

**Explanation:**

* `-sC` runs default scripts that gather basic info about each open port.
* `-sV` tries to identify the version of the software running on each port.
* Helps us know what kind of server we’re dealing with and potential vulnerabilities.

**Nmap Output:**

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-08-21 05:00 EDT
Nmap scan report for 192.168.56.102
Host is up (0.0051s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE  VERSION
80/tcp   open  http     Apache httpd 1.3.28 ((Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c)
|_http-server-header: Apache/1.3.28 (Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c
|_http-title: Welcome to BadStore.net v1.2.3s
| http-robots.txt: 5 disallowed entries 
|_/cgi-bin /scanbot /backup /supplier /upload
| http-methods: 
|_  Potentially risky methods: TRACE
443/tcp   open  ssl/http Apache httpd 1.3.28 ((Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c)
| ssl-cert: Subject: commonName=www.badstore.net/organizationName=BadStore.net/stateOrProvinceName=Illinois/countryName=US
| Subject Alternative Name: email:root@badstore.net
| Not valid before: 2006-05-10T12:52:53
|_Not valid after:  2009-02-02T12:52:53
| http-robots.txt: 5 disallowed entries 
|_/cgi-bin /scanbot /backup /supplier /upload
|_http-title: Welcome to BadStore.net v1.2.3s
|_http-server-header: Apache/1.3.28 (Unix) mod_ssl/2.8.15 OpenSSL/0.9.7c
|_ssl-date: 2025-08-21T09:01:55+00:00; +50s from scanner time.
| sslv2: 
|   SSL2_RC4_64_WITH_MD5
|   SSL2_RC2_128_CBC_WITH_MD5
|   SSL2_RC4_128_WITH_MD5
|   SSL2_IDEA_128_CBC_WITH_MD5
|   SSL2_DES_192_EDE3_CBC_WITH_MD5
|   SSL2_RC4_128_EXPORT40_WITH_MD5
|   SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|_  SSL2_DES_64_CBC_WITH_MD5
3306/tcp open  mysql    MySQL 4.1.7-standard
| mysql-info: 
|   Protocol: 10
|   Version: 4.1.7-standard
|   Thread ID: 6
|   Capabilities flags: 33324
|   Some Capabilities: LongColumnFlag, Support41Auth, ConnectWithDatabase, SupportsCompression, Speaks41ProtocolNew
|   Status: Autocommit
|_  Salt: ^zi~_Blv.~Uj:|'Hs/+*
MAC Address: 08:00:27:4A:39:79 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Host script results:
|_clock-skew: 49s

Nmap done: 1 IP address (1 host up) scanned in 13.82 seconds
```

**Interpretation:**

* **Port 80/tcp (HTTP)**: Apache web server, old version. Page title: *Welcome to BadStore.net v1.2.3s*.
* **Port 443/tcp (HTTPS)**: Apache with SSL. Certificate expired, SSLv2 supported (weak).
* **Port 3306/tcp (MySQL)**: Version 4.1.7, basic database server.

**Notes:**

* Web server (80/443) is first target.
* Disallowed directories in `robots.txt` may hide interesting files: `/cgi-bin`, `/scanbot`, `/backup`, `/supplier`, `/upload`.
* Risky HTTP methods like TRACE are noted.

**Next Step:**

* Explore the web app on port 80/443, check directories, forms, and pages for vulnerabilities.

---

*Markdown notes:*

* Use triple backticks for commands and code blocks.
* Use `**bold**` for important elements like ports and services.
* Use bullets for observations and notes.
* Separate steps with `---` for readability.

