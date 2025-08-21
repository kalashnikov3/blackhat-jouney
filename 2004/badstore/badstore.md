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

**Result / Interpretation:**

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
