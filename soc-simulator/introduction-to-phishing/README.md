# TryHackMe – SOC Simulator: Introduction to Phishing

## Overview

This lab from TryHackMe introduces the workflow of a **Security Operations Center (SOC) Analyst** by simulating real security alerts related to phishing activity.

The objective of the scenario is to:

* Monitor incoming alerts
* Investigate suspicious emails and network activity
* Correlate events across different logs
* Determine whether alerts are **True Positives** or **False Positives**
* Create proper **incident reports**

The lab simulates a real SOC dashboard where analysts must triage alerts generated from **email security systems and firewall logs**.

---

# Scenario Objectives

* Monitor and analyze real-time alerts
* Identify suspicious emails and phishing attempts
* Investigate external URLs found in email messages
* Correlate firewall logs with email alerts
* Document findings in a structured SOC incident report

---

# Alerts Investigated

The scenario included **four alerts** that needed investigation.

| Alert ID | Alert Type                    | Severity | Final Classification |
| -------- | ----------------------------- | -------- | -------------------- |
| 8814     | Suspicious Email Link         | Medium   | False Positive       |
| 8815     | Phishing Email                | Medium   | True Positive        |
| 8816     | Blacklisted URL Access        | High     | True Positive        |
| 8817     | Microsoft Credential Phishing | Medium   | True Positive        |

---

# Alert Analysis

## Alert 8814 – HR Onboarding Email

### Email Details

**Sender:**
`onboarding@hrconnex.thm`

**Recipient:**
`j.garcia@thetrydaily.thm`

**Subject:**
Action Required: Finalize Your Onboarding Profile

**Link:**

```
https://hrconnex.thm/onboarding/15400654060/j.garcia
```

### Investigation

* Sender domain matched the link domain
* Content matched a normal **employee onboarding process**
* No phishing indicators such as urgency or credential theft
* No malicious domain detected

### Verdict

**False Positive**

This was a legitimate onboarding email.

---

# Alert 8815 – Amazon Delivery Phishing

### Email Details

**Sender**

```
urgents@amazon.biz
```

**Recipient**

```
h.harris@thetrydaily.thm
```

**Subject**

```
Your Amazon Package Couldn’t Be Delivered – Action Required
```

**Malicious URL**

```
http://bit.ly/3sHkX3da12340
```

### Phishing Indicators

* Sender domain impersonates Amazon
* Suspicious **.biz domain**
* Uses urgency to pressure the user
* Contains a **shortened URL (bit.ly)** hiding the real destination

### Verdict

**True Positive – Phishing Email**

---

# Alert 8816 – Firewall Blocked Malicious URL

### Firewall Log

| Field          | Value                       |
| -------------- | --------------------------- |
| Source IP      | 10.20.2.17                  |
| Destination IP | 67.199.248.11               |
| URL            | http://bit.ly/3sHkX3da12340 |
| Action         | Blocked                     |

### Investigation

The URL matches the phishing email found in **Alert 8815**.

Firewall logs show:

* An internal host attempted to access the malicious link
* The firewall successfully blocked the connection

### Verdict

**True Positive – Malicious URL Access Attempt**

This confirms that a user attempted to access the phishing link.

---

# Alert 8817 – Microsoft Credential Phishing

### Email Details

**Sender**

```
no-reply@m1crosoftsupport.co
```

**Recipient**

```
c.allen@thetrydaily.thm
```

**Phishing URL**

```
https://m1crosoftsupport.co/login
```

### Phishing Indicators

* Domain impersonates Microsoft
  (`m1crosoft` uses number **1 instead of i**)
* Fake login portal
* Claims suspicious login activity
* Attempts credential harvesting

### Verdict

**True Positive – Credential Phishing Attempt**

---

# Attack Chain Correlation

The alerts reveal a **multi-step phishing attack scenario**.

```
Phishing Email Delivered
        ↓
User Clicks Malicious Link
        ↓
Browser Attempts Connection
        ↓
Firewall Blocks Malicious Domain
        ↓
SOC Receives Alert
```

Alerts **8815 and 8816** are directly correlated.

---

# Indicators of Compromise (IOCs)

### Malicious Domains

```
m1crosoftsupport.co
```

### Suspicious Email Senders

```
urgents@amazon.biz
no-reply@m1crosoftsupport.co
```

### Malicious URLs

```
http://bit.ly/3sHkX3da12340
https://m1crosoftsupport.co/login
```

### Destination IP

```
67.199.248.11
```

---

# Recommended Remediation

* Block malicious domains across email and DNS filters
* Perform endpoint investigation on affected hosts
* Reset potentially exposed credentials
* Notify affected users
* Conduct phishing awareness training
* Monitor for further suspicious activity

---

# Key SOC Skills Practiced

This lab helped practice important **SOC Tier-1 analyst skills**:

* Email phishing detection
* Domain impersonation identification
* URL investigation
* Firewall log analysis
* Alert triage
* Incident documentation
* Threat correlation

---

# Conclusion

The SOC Simulator demonstrates how phishing attacks are detected and investigated within a real security monitoring environment.

By correlating **email alerts and firewall logs**, analysts can quickly identify phishing campaigns and prevent users from accessing malicious websites.

This exercise reinforces the importance of **log analysis, alert correlation, and incident reporting** in modern Security Operations Centers.

---

# Platform

Room completed on **TryHackMe**

https://tryhackme.com
