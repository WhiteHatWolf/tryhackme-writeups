# Cupid's Vault — TryHackMe Writeup

## Room Overview

**Platform:** TryHackMe
**Difficulty:** Easy
**Category:** Web Exploitation / Information Disclosure

Cupid designed a vault to hide secrets forever, but a misconfiguration exposed sensitive information. The objective of this challenge is to analyze the web application, identify vulnerabilities, and retrieve the hidden flag.

This room focuses on **information disclosure and poor security practices**, particularly the misuse of the `robots.txt` file.

---

# Enumeration

The first step was to access the web application provided by TryHackMe.

```
http://<TARGET_IP>:<PORT>
```

Since this is a web challenge, the next step was to enumerate common files and directories.

---

# Checking robots.txt

A common place to look for hidden directories is the `robots.txt` file.

```
http://<TARGET_IP>:<PORT>/robots.txt
```

The following content was discovered:

```
User-agent: *
Disallow: /cupids_secret_vault/*

# cupid_arrow_2026!!!
```

### Observations

Two important pieces of information were revealed:

1. A **hidden directory**

```
/cupids_secret_vault/
```

2. A **possible password**

```
cupid_arrow_2026!!!
```

The comment in `robots.txt` appeared to contain a credential.

---

# Discovering the Vault

Navigating to the hidden directory:

```
http://<TARGET_IP>:<PORT>/cupids_secret_vault/
```

Further enumeration revealed an **administrator panel**.

```
/cupids_secret_vault/administrator
```

Opening the page presented a **login form**.

---

# Authentication

The password discovered in `robots.txt` was tested on the login page.

**Credentials used:**

```
Password: cupid_arrow_2026!!!
```

After submitting the login form with the discovered password, authentication was successful and access to the administrator panel was granted.

---

# Flag Discovery

Once logged into the administrator panel, the flag was visible within the application interface.

The flag was successfully retrieved.

```
THM{REDACTED_FLAG}
```

---

# Vulnerability Explanation

The vulnerability in this challenge is **Information Disclosure**.

Sensitive information was exposed inside the `robots.txt` file:

```
# cupid_arrow_2026!!!
```

`robots.txt` is publicly accessible and should **never contain credentials, secrets, or sensitive data**.

Attackers commonly check this file during reconnaissance to discover hidden paths and leaked information.

---

# Attack Path Summary

1. Access the target web application
2. Check the `robots.txt` file
3. Discover hidden directory `/cupids_secret_vault/`
4. Find leaked password `cupid_arrow_2026!!!`
5. Locate the administrator login page
6. Authenticate using the discovered credential
7. Retrieve the flag from the admin panel

---

# Key Takeaways

* `robots.txt` is **not a security mechanism**
* Sensitive data should **never be stored in public files**
* Hidden directories can still be discovered through enumeration
* Information disclosure is a common web vulnerability

---

# Tools Used

* Web Browser
* Manual Enumeration

---

# Author

**Midhun V (MV)**
Cybersecurity Enthusiast | Ethical Hacking Learner | Aspiring Penetration Tester

---