# Cloud Honeypot & Threat Analysis

## Multi-Region T-Pot Honeynet on Google Cloud Platform

A month-long academic security research project investigating real-world automated attack activity captured by a distributed **T-Pot honeynet** deployed across four Google Cloud regions.

The project focused on collecting, filtering and analysing telemetry from multiple honeypots, identifying attacker behaviour, enriching indicators with external threat-intelligence services, and mapping observed activity to the **MITRE ATT&CK** framework.

> **Portfolio note:** This repository is a sanitised portfolio presentation of the project. It intentionally excludes cloud credentials, private keys, student identifiers, live infrastructure addresses and the original university submission document.

## Project Overview

The honeynet consisted of a central **T-Pot Hive in London** and three remote sensors in **Israel, the USA and Singapore**. The deployment was designed to expose multiple commonly targeted services and observe differences in attack behaviour across regions.

The T-Pot platform provided several specialised honeypots, including:

- **Cowrie** — SSH/Telnet interaction and brute-force activity
- **ADBHoney** — Android Debug Bridge (ADB) attack activity
- **Dionaea** — network-service and malware-related scanning activity
- **Mailoney** — SMTP/spam probing activity
- **Kibana / Elasticsearch** — centralised log indexing, filtering and analysis

The deployment ran for approximately one month and produced high-volume telemetry suitable for behavioural analysis.

## Objectives

- Deploy a distributed honeynet across multiple cloud regions.
- Capture automated scanning, brute-force, malware and spam-related activity.
- Analyse attacker sessions and post-authentication behaviour.
- Investigate suspicious files and malware indicators.
- Enrich IP indicators using **AbuseIPDB**, **VirusTotal** and **Cisco Talos**.
- Compare attack patterns across geographical regions.
- Map observed behaviours to MITRE ATT&CK techniques.

## Architecture

```text
                         Google Cloud Platform

                    +-------------------------+
                    |     London / Hive       |
                    |       T-Pot Hive        |
                    |  Kibana / Elasticsearch |
                    +-----------+-------------+
                                |
              +-----------------+-----------------+
              |                 |                 |
              v                 v                 v
       +-------------+   +-------------+   +-------------+
       |   Israel    |   |     USA     |   |  Singapore  |
       |   Sensor    |   |   Sensor    |   |   Sensor    |
       +-------------+   +-------------+   +-------------+
              |                 |                 |
              +-----------------+-----------------+
                                |
                       T-Pot telemetry
                                |
                                v
                    Centralised investigation
```

The original deployment used one Hive in the London region and three remote sensors in Israel, the USA and Singapore.

## Deployment

The infrastructure was built using Google Cloud Platform. Four virtual machines were used for the distributed T-Pot environment: one Hive and three remote sensors. The deployment intentionally exposed commonly targeted services including SSH, Telnet, RDP, SMB, MySQL and ADB, together with additional web/service ports. Sensor telemetry was forwarded to the central Hive for indexing and analysis.

## Key Findings

### 1. High-volume SSH brute force

The London Cowrie sensor recorded **864,262 attacks from 4,803 unique IPs**. Common authentication attempts used generic usernames and weak passwords, demonstrating highly automated SSH brute-force behaviour. Attackers also issued system-information commands such as `uname`, `/proc/cpuinfo` and `free` after gaining access.

### 2. SSH persistence attempt

A Singapore Cowrie session showed successful authentication followed by system discovery and an attempt to establish persistence through the SSH `authorized_keys` mechanism. The captured activity included manipulation of the `.ssh` directory and placement of a suspicious file. The associated file hash was subsequently checked with VirusTotal.

### 3. Android malware deployment through ADB

The Singapore ADBHoney investigation identified an attacker uploading `ufo.apk`, attempting to install it with `pm install`, and launching its main activity. VirusTotal analysis reported **39/64 security vendors** detecting the sample as malicious. The report linked the observed behaviour to Android botnet/crypto-mining activity.

### 4. Multi-stage payload execution

After the primary APK activity, the attacker attempted to invoke a secondary payload named `trinity`, including checking whether it was running, changing execution permissions and launching it in the background. This provided evidence of a multi-stage infection sequence.

### 5. SMB scanning

The Singapore Dionaea sensor recorded more than **440,000 SMB/MySQL attempts**. The observed connections generally completed an SMB handshake without delivering a payload, consistent with automated reconnaissance rather than confirmed successful exploitation.

### 6. SMTP probing

The USA Mailoney honeypot recorded **1,509 SMTP interactions from 112 sources**. The activity was characterised as automated spam-delivery testing and probing for open-relay misconfigurations rather than targeted phishing delivery.

## Threat Intelligence Workflow

Indicators and samples were enriched using:

- **AbuseIPDB** — reputation and historical abuse reporting for IP indicators
- **VirusTotal** — malware/sample reputation and multi-vendor detection
- **Cisco Talos** — IP reputation and threat-intelligence context

The investigation combined honeypot telemetry with external intelligence rather than treating individual events in isolation.

## MITRE ATT&CK Mapping

| Honeypot activity | Observed behaviour | Technique | ID |
|---|---|---|---|
| Cowrie — SSH brute force | Repeated password guessing | Brute Force | T1110 |
| Cowrie — post-login behaviour | System information discovery | System Information Discovery | T1082 |
| Cowrie — malicious file placement | SSH authorised-key persistence attempt | SSH Authorized Keys | T1098.004 |
| ADBHoney — APK delivery | Upload and installation of malicious APK | Ingress Tool Transfer / User Execution | T1105 / T1204.002 |
| ADBHoney — botnet execution | Malware and secondary payload execution | Command and Scripting Interpreter | T1059 |
| Dionaea — SMB probing | Repeated network-service probing | Network Service Scanning | T1046 |
| Mailoney — SMTP probing | Repeated spoofed/invalid SMTP activity | Phishing: Email Delivery | T1598 |

These mappings are based on the MITRE ATT&CK mapping presented in the academic report.

## Evidence

The `screenshots/` directory contains selected, sanitised evidence from the original analysis. Personal identifiers and visible infrastructure IP addresses have been removed where applicable.

- `page-10-sanitised.png` — multi-region Kibana overview
- `page-12-sanitised.png` — Singapore dashboard and Cowrie attack volume
- `page-26-sanitised.png` — VirusTotal analysis and APK installation/execution evidence
- `page-27-sanitised.png` — multi-stage `trinity` payload execution
- `page-31-sanitised.png` — MITRE ATT&CK mapping

## Skills Demonstrated

- Cloud security experimentation with **Google Cloud Platform**
- Honeypot deployment and deception-based monitoring
- Security telemetry analysis with **Kibana / Elasticsearch**
- SSH brute-force and post-compromise analysis
- Malware and suspicious-file investigation
- Threat-intelligence enrichment
- Log filtering and session reconstruction
- IOC analysis
- MITRE ATT&CK mapping
- Technical security reporting

## Limitations

This was an academic research deployment rather than a production security monitoring environment. The results represent the traffic observed by the deployed honeypots during the study period and should not be interpreted as a complete measurement of global cyber-attack activity.

The repository does not contain the original cloud deployment credentials or live infrastructure configuration.

## Academic Context

**Course:** Digital Forensics and Cybersecurity (TU863)  
**Module:** Network Security Analytics  
**Project:** Assignment 1 — A Multi-Region Analysis of Cyber Attacks Using T-Pot Honeypots

The portfolio repository has been separated from the original university submission to keep the public project focused on technical work and to avoid publishing academic identifiers or sensitive infrastructure details.

## References

The original report referenced T-Pot, VirusTotal, AbuseIPDB, Cisco Talos, MITRE ATT&CK, Google Cloud Platform and supporting honeypot/security research. The original academic report is not included in this public repository.
