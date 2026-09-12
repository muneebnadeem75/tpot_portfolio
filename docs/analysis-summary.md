# Analysis Summary

## London — Cowrie

The London Cowrie sensor was selected for detailed SSH analysis because it recorded the highest Cowrie activity among the regions. The report recorded 864,262 attacks from 4,803 unique IPs. Common activity included repeated authentication attempts using weak credentials and post-login system reconnaissance.

Observed commands included:

- `uname -a`
- `cat /proc/cpuinfo`
- `free -m`

The analysis also recorded malware download attempts involving binaries associated with IoT botnet activity.

## Singapore — Cowrie

A Singapore Cowrie session was investigated for file-download activity and post-authentication behaviour. The attacker successfully authenticated using weak credentials, performed system discovery and attempted to establish persistence using SSH authorised keys.

The session included manipulation of the `.ssh` directory and placement of a suspicious `authorized_keys` file. The file was subsequently investigated through VirusTotal.

## Singapore — ADBHoney

ADBHoney generated a high volume of activity and was selected for deeper investigation. A filtered log search identified command activity associated with an attacker session.

The investigated sequence included:

1. Uploading `ufo.apk`.
2. Installing the APK with `pm install`.
3. Launching the APK using `am start`.
4. Checking for the `trinity` payload.
5. Changing execution permissions with `chmod 0755`.
6. Launching the secondary payload in the background.

VirusTotal reported 39/64 vendors detecting the uploaded APK as malicious.

## Singapore — Dionaea

Dionaea recorded more than 440,000 SMB/MySQL attempts. The analysis found repeated SMB connection attempts that completed the handshake without a payload, indicating widespread automated scanning.

## USA — Mailoney

Mailoney recorded 1,509 SMTP interactions from 112 sources. The activity consisted primarily of repetitive and malformed SMTP messages. The report interpreted the pattern as automated spam-delivery testing and probing for open-relay weaknesses.
