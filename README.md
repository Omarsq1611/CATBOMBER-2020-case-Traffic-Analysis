# 🕵️‍♂️ CATBOMBER 2020: Network Traffic Analysis

## 📌 Project Overview
This repository contains a comprehensive investigation of the **2020-05-28 - TRAFFIC ANALYSIS EXERCISE - CATBOMBER** packet capture (PCAP). The objective of this exercise is to perform a deep-dive network traffic analysis to identify malicious activity, map the attacker's methodology, and uncover data exfiltration. 

## 🛠️ Tools Used
* **Wireshark:** For packet capture analysis, protocol filtering, and stream inspection.
* **VirusTotal:** For Open Source Intelligence (OSINT) and verifying malicious Indicators of Compromise (IOCs).

## 📋 Methodology & Investigation Steps

### 1. Initial Reconnaissance & Protocol Analysis
The investigation began with a broad overview of the network protocol hierarchy. A significant anomaly was immediately identified: the presence of **TLSv1** traffic. The use of this deprecated and insecure encryption protocol served as the initial indicator of a potentially compromised or poorly secured environment.

### 2. DNS Traffic Investigation
Focusing on DNS resolution, I analyzed the domain queries to identify command-and-control (C2) or staging indicators. A highly suspicious query for `whatismyip` was discovered. This behavior is commonly associated with malware scripts or threat actors attempting to verify their external routing and IP address during the initial stages of an infection.

### 3. HTTP Deep Dive & Unencrypted Traffic
Following the lack of proper encryption, the investigation pivoted to standard `HTTP` traffic. Filtering out HTTPS revealed a flood of unencrypted, outgoing `HTTP POST` requests directed toward an unknown, suspicious IP address.

### 4. Threat Intelligence Verification
To confirm the malicious nature of the destination IP, the address was extracted from the PCAP and queried against **VirusTotal**. Multiple security vendors flagged the IP as malicious, confirming an active threat and a successful compromise of the host.

### 5. Payload Inspection & Data Exfiltration
A deeper inspection of the `HTTP POST` streams revealed critical details about the attacker's infrastructure and objectives:
* **Server Header:** The malicious server was operating with the unusual header name **"cowboy"**.
* **Data Exfiltration:** The `POST` payloads contained highly sensitive Personally Identifiable Information (PII) and financial data being transmitted in plaintext. The exfiltrated data included:
  * Leaked Emails
  * Account Passwords
  * Credit Card Numbers
  * Banking PINs

## 💡 Key Takeaways & Mitigation
This incident highlights the critical importance of modern network security hygiene. Key recommendations based on this analysis include:
1. **Deprecate Legacy Protocols:** Strictly enforce TLS 1.2 or TLS 1.3 and disable TLSv1/1.1 across the network.
2. **Enforce HTTPS:** Ensure all sensitive data transmission occurs over encrypted channels to prevent plaintext packet sniffing.
3. **Monitor DNS Anomalies:** Implement alerting for automated or suspicious domain queries (like external IP checks).
4. **Endpoint Security:** Deploy robust EDR solutions to detect the malware responsible for harvesting and exfiltrating local system data.

---
*This analysis was conducted as part of a hands-on cybersecurity and network forensics training exercise.*
