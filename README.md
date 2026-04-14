# Phishing-Forensics-SOC-Workflow
A comprehensive Standard Operating Procedure (SOP) and investigation framework for phishing analysis, combining CFE investigative techniques with Azure Sentinel/KQL hunting.
# 🛡️ Phishing Investigation Workflow (2026)

This repository outlines a high-standard Operating Procedure (SOP) for investigating phishing threats, blending **Forensic Fraud Examination** with **SOC Analysis** within the Azure/Microsoft 365 ecosystem.

---

### ⚖️ The SOP "Golden Rules"
```diff
- NOTICE: Before the investigation begins, these mandates apply to all personnel:
```
> ### ⚠️ **MANDATORY: THE GOLDEN RULES**

> * **The "Capture First" Rule:** Digital evidence is volatile. You must take screenshots of the email, the landing page, and the source code immediately upon discovery. Phishing infrastructure often "goes dark" within minutes of detection.
> * **Legal Evidence Standards:**
>     * **Full Desktop Capture:** Capture the entire screen, including the system clock and the browser URL bar.
>     * **Metadata Preservation:** Use Fireshot or "Print to PDF" to preserve underlying links.
>     * **Hashing:** Immediately run saved files through **HashMyFiles** to generate a SHA-256 integrity hash.

---

## 🚀 Investigation Phases

### Phase 1: First Responder Triage
**Target:** Immediate identification and evidence freezing.
* **Stop & Hover:** Check the "Target URL" in the bottom corner of the browser. If the text says `maximus.com`, but the link goes to `usahire.site`, it is a threat.
* **The Tone Check:** Identify **Urgency** (Hiring now!), **Anomalies** (Bad grammar: "Apply on both role"), and **The Hook** (High-paying remote work).
* **Sender Verification:** Verify the actual domain. 
    * *Real:* `hr@maximus.com` | *Fake:* `recruit@usahire.site`

### Phase 2: Technical URL Investigation & Forensic Capture
**Target:** Safe extraction and permanent documentation.
* **Safe Extraction:** Right-click and "Copy Link Address." Do not click.
* **Immediate Forensic Capture:** Use **urlscan.io** or **Hunchly** to create a timestamped snapshot. 
* **Infrastructure Analysis:**
    * **VirusTotal:** Check for global blacklisting.
    * **DomainTools WHOIS:** Verify domain age and location (e.g., 3 days old in Lithuania).

### Phase 3: The Fraud Examination (CFE Focus)
**Target:** Intent, attribution, and PII risk assessment.
* **Identity Attribution:** Search LinkedIn for the sender. Validate the company via **OpenCorporates**.
* **Geographical Anomalies:** A US-based job hosted on a Lithuanian registrar is a high-probability scam.
* **Impact Assessment:** Determine if the goal is Identity Theft (SSN/DOB harvesting) or Financial Fraud (Wire transfer/Advance fee).

### Phase 4: SOC Advanced Hunting (Azure/Sentinel)
**Target:** Scoping the "blast radius" across the enterprise.
* **KQL Hunting:** Run the following in the **Microsoft Sentinel Portal**:
    ```kusto
    EmailEvents
    | where SenderMailFromDomain == "usahire.site" 
    | project Timestamp, RecipientEmailAddress, Subject
    ```
* **Header Forensics:** Use **Microsoft Message Header Analyzer** to verify SPF/DKIM/DMARC alignment.

### Phase 5: Remediation & Containment
**Target:** Neutralizing the threat and securing identities.
* **Purge:** Use Microsoft Defender to "Hard Delete" (ZAP) the email from all inboxes.
* **Block:** Add `usahire.site` to the Tenant Allow/Block List.
* **Identity Check:** Review **Microsoft Entra ID** logs for "Impossible Travel" alerts on any users who interacted with the link.

---

## 📑 Forensic Investigation Report Template
Standard layout for legally defensible documentation:

| Section | Content Requirements |
| :--- | :--- |
| **I. Executive Summary** | Incident ID, Date/Time, Investigator Name, and Summary Verdict. |
| **II. IOC Table** | Sender, Phishing URL, Source IP, Registrar, and SHA-256 File Hashes. |
| **III. Chronological Log** | Step-by-step timestamped actions (e.g., "21:05 - Performed WHOIS"). |
| **IV. Evidence** | Attached screenshots, raw headers, and Integrity Hashes. |
| **V. Remediation** | Actions taken (Domain blocked, ZAP triggered, User PW reset). |

---

## 🛠️ Tool Summary Checklist

### 🧪 Analysis & Detonation
* **[urlscan.io](https://urlscan.io/):** A "security camera" for the web. Visits the link and captures a screenshot safely.
* **[VirusTotal](https://www.virustotal.com/):** The "global database." Checks links/files against 70+ antivirus scanners.
* **[Any.Run](https://any.run/):** An "interactive sandbox" to watch malware behavior in real-time.

### 🕵️ Attribution (Unmasking the Attacker)
* **[DomainTools WHOIS](https://whois.domaintools.com/):** Background check for websites (verifies site age).
* **[MxToolbox](https://mxtoolbox.com/EmailHeaders.aspx):** Analyzes email headers to detect spoofing.
* **[OpenCorporates](https://opencorporates.com/):** The official registry to verify legal business entities.

### 🏢 Enterprise Operations (The Azure/SOC Stack)
* **[Microsoft Sentinel](https://azure.microsoft.com/en-us/products/microsoft-sentinel):** The "command center" for searching company-wide logs.
* **[Microsoft Defender](https://www.microsoft.com/en-us/security/business/threat-protection/microsoft-defender-office-365):** The "shield" used to purge (ZAP) malicious emails.
* **[Microsoft Entra ID](https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id):** Monitors for compromised account behaviors like "Impossible Travel."

### ⚖️ Evidence Integrity (Legal Chain of Custody)
* **[HashMyFiles](https://www.nirsoft.net/utils/hash_my_files.html):** Creates a SHA-256 "digital fingerprint" to prove evidence hasn't been tampered with.
* **[Hunchly](https://www.hunch.ly/):** Automatically captures and timestamps web pages for legal discovery.

---

**Final Note:** Treat your KQL queries and sandbox reports as Expert Witness Testimony. Save raw outputs as `.txt` files alongside your final PDF report to ensure full transparency during legal discovery.

---
© 2026 Dawn Marie Smith, CFE. All Rights Reserved. 
Unauthorized use and/or duplication of this material without express and written permission from this site’s author and/or owner is strictly prohibited.
