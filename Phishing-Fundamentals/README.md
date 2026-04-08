# 🎣 Phishing Fundamentals – TryHackMe Write-up

## 📌 Overview
This repository documents my learning and hands-on experience from the **Phishing Fundamentals** room on TryHackMe.

The room focuses on how phishing attacks work, why they are effective, and how penetration testers simulate them to assess an organisation’s human security layer.

Phishing is a powerful **social engineering attack** that targets people instead of systems, making it one of the most effective techniques for gaining initial access during a penetration test.

---

## 🎯 Learning Objectives
- Understand what phishing is and its role in penetration testing  
- Learn the psychology behind phishing attacks  
- Identify different types of phishing (Phishing, Spear Phishing, Whaling)  
- Explore technical phishing techniques  
- Understand the lifecycle of a phishing campaign  
- Gain hands-on experience using phishing tools  

---

## 🧠 Key Concepts

### 🔹 What is Phishing?
Phishing is a cyber attack that uses **social engineering** to trick users into:
- Revealing sensitive information (credentials, financial data)
- Clicking malicious links
- Executing malware  

Common delivery methods:
- Email (Phishing)
- SMS (Smishing)
- Voice calls (Vishing)
- Fake websites

---

### 🔹 Types of Phishing
- **Phishing:** Broad, mass-targeted attacks  
- **Spear Phishing:** Targeted attack on a specific individual  
- **Whaling:** Targeted attack on high-level executives  

---

### 🔹 Psychology Behind Phishing
Phishing works by exploiting human behavior:

- **Urgency** → “Act now or lose access”  
- **Authority** → “Message from IT/Admin”  
- **Fear** → “Security breach detected”  
- **Curiosity** → “Confidential information”  
- **Scarcity** → “Limited offer”  
- **Trust** → Impersonating known entities  

---

### 🔹 Technical Phishing Techniques

#### 🌐 URL & Domain Manipulation
- Typosquatting (e.g., `tryhacme.com`)  
- Homograph attacks  
- URL masking & shorteners  

#### 📧 Email Spoofing
- Fake sender identity  
- Display name spoofing  
- Lookalike domains  

#### 🔐 Credential Harvesting
- Fake login pages capturing user input  
- Redirecting victims to real site after submission  

#### 📎 Payload Delivery
- Malicious macros in documents  
- Hidden scripts executed after enabling content  

---

### 🔹 Security Defenses
- SPF (Sender Policy Framework)  
- DKIM (DomainKeys Identified Mail)  
- DMARC (Domain-based Message Authentication, Reporting & Conformance)  

---

## 🔄 Phishing Campaign Lifecycle

1. **Planning & Scoping**  
2. **Reconnaissance (OSINT)**  
3. **Scenario & Payload Development**  
4. **Exploitation**  
5. **Reporting & Analysis**  

---

## 🛠️ Tools Used
- Social Engineering Toolkit (SET)  
- GoPhish  
- Evilginx  

---

## 💻 Practical: Credential Harvesting Attack

In this lab, I simulated a **spear phishing attack** targeting a finance employee.

### 🔧 Steps Performed:
1. Launched **SET (Social Engineering Toolkit)**  
2. Selected:
   - Social Engineering Attacks  
   - Website Attack Vectors  
   - Credential Harvester  
3. Imported custom phishing page  
4. Hosted fake login page on attacker machine  
5. Crafted phishing email using spoofed identity  
6. Delivered email via webmail client  
7. Captured credentials via POST request  

---

## 📊 Result
- Successfully executed a phishing attack  
- Captured user credentials through a fake login page  

**Captured Flag:** `THM{you_just_got_phished!}`

---

## 📈 Key Takeaways
- Humans are the weakest link in security  
- Phishing can bypass strong technical defenses  
- Realistic scenarios increase attack success rates  
- Security awareness training is critical  
- Simulated phishing helps measure organisational risk  

---

## ⚠️ Disclaimer
This project is for **educational purposes only**.  
All activities were performed in a controlled lab environment.

---

## 🚀 Author
**Midhun V**  
Cybersecurity Enthusiast | Ethical Hacking | Penetration Testing