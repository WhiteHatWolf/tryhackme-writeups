# Conti Ransomware Investigation – TryHackMe Writeup

## Room Link

https://tryhackme.com/room/contiransomwarehgh

---

# Overview

This room simulates a ransomware incident where employees cannot access Outlook and the Exchange Admin Center. During triage, suspicious **readme ransom notes** were discovered on the Exchange server.

The investigation is performed using **Splunk** logs that include **Sysmon** events.

The goal is to analyze the logs and identify attacker actions that led to the deployment of **Conti ransomware**.

---

# Splunk Access

```
URL: http://10.49.134.55:8000
Username: bellybear
Password: password!!!
```

Navigate to:

```
Apps → Search & Reporting
```

Set the **Time Range → All Time**.

---

# Investigation

## 1. Identify the Ransomware Location

Search for file creation events:

```
index=* EventCode=11
| table _time TargetFilename Image
```

The suspicious executable was found in the temporary directory.

### Answer

```
C:\Windows\SERVIC~1\NETWOR~1\AppData\Local\Temp\mpam-69a0ed8.exe
```

---

## 2. Sysmon Event ID for File Creation

From Sysmon documentation:

```
EventCode = 11
```

### Answer

```
11
```

---

## 3. MD5 Hash of the Ransomware

Search for the ransomware executable.

```
index=* mpam-69a0ed8.exe
```

Check the **Hashes** field.

### Answer

```
290C7DFB01E50CEA9E19DA81A781AF2C
```

---

## 4. File Saved to Multiple Folder Locations

Search for repeated file creation.

```
index=* EventCode=11
| stats count by TargetFilename
| sort -count
```

Multiple ransom notes were created across user directories.

### Answer

```
readme.txt
```

---

## 5. Command Used to Add a New User

Search for user creation commands.

```
index=* "net user"
| table CommandLine
```

### Answer

```
net user /add securityninja hardToHack123$
```

This indicates attacker persistence via a new local user account.

---

## 6. Process Migration

Search process relationships.

```
index=* EventCode=1
| table Image ParentImage CommandLine
```

### Answer

```
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe,C:\Windows\System32\wbem\unsecapp.exe
```

This shows the attacker migrating execution into PowerShell.

---

## 7. Process Used to Retrieve System Hashes

Credential dumping typically targets the **Local Security Authority Subsystem Service**.

### Answer

```
C:\Windows\System32\lsass.exe
```

---

## 8. Web Shell Deployed

Search for ASPX files created on the server.

```
index=* ".aspx"
| table TargetFilename
```

### Answer

```
i3gfPctK1c2x.aspx
```

This is the malicious web shell placed on the Exchange server.

---

## 9. Command Executing the Web Shell

Search events containing the web shell name.

```
index=* i3gfPctK1c2x.aspx
```

### Answer

```
attrib.exe -r \\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Sever\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx
```

This command removed the **read-only attribute** from the shell.

---

## 10. Exploited CVEs

The attacker leveraged the following vulnerabilities.

### Answer

```
CVE-2020-0796,CVE-2018-13374,CVE-2018-13379
```

These vulnerabilities allowed the attacker to gain access and eventually deploy ransomware.

---

# Attack Timeline

1. Attacker gains initial access through vulnerable services.
2. Web shell is deployed to the Exchange OWA directory.
3. PowerShell execution and process migration occur.
4. New local user account is created.
5. Credentials are dumped from LSASS.
6. Ransomware executable is deployed.
7. `readme.txt` ransom notes are dropped across multiple folders.

---

# MITRE ATT&CK Techniques

| Technique            | Description                    |
| -------------------- | ------------------------------ |
| Initial Access       | Exploiting vulnerable services |
| Persistence          | Creating new user account      |
| Privilege Escalation | Credential dumping             |
| Execution            | PowerShell command execution   |
| Defense Evasion      | Obfuscated scripts             |
| Impact               | Ransomware deployment          |

---

# Conclusion

This investigation demonstrated how attackers leveraged vulnerabilities to compromise an Exchange server and deploy **Conti ransomware**. By analyzing Sysmon logs in Splunk, we were able to reconstruct the attacker’s activity and identify key indicators of compromise.

---

# Author

Midhun V (MV) Cybersecurity Enthusiast | Ethical Hacking Learner | Aspiring Penetration Tester
TryHackMe Writeups Repository
