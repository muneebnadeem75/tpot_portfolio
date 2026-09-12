# MITRE ATT&CK Mapping

The following mappings reproduce the technique-level analysis documented in the academic project.

| Activity | Technique | ID |
|---|---|---|
| SSH password guessing | Brute Force | T1110 |
| Post-login system discovery | System Information Discovery | T1082 |
| SSH authorised-key persistence attempt | SSH Authorized Keys | T1098.004 |
| Malicious APK transfer | Ingress Tool Transfer | T1105 |
| APK execution | User Execution: Malicious File | T1204.002 |
| Secondary payload execution | Command and Scripting Interpreter | T1059 |
| SMB probing | Network Service Scanning | T1046 |
| SMTP probing | Phishing: Email Delivery | T1598 |

The mappings are based on observed honeypot activity rather than inferred from generic attack patterns.
