# [INC-20231006-041] Suspicious Network Traffic & Credential Harvesting - RedLine Stealer Activity

### Executive Summary (Incident Write-up)
During our triage, we detected suspicious outbound network connections and unauthorized credential harvesting activity originating from Workstation-01. The malicious binary executed under the masqueraded name `Wextract.exe` and dynamically interacted with `ADVAPI32.dll` to query security descriptors, manipulate access tokens, and escalate privileges. Following execution, the process verified external internet connectivity via DNS resolution to `facebook.com` and established a persistent raw TCP socket connection to a remote C2 server at `77.91.124.55:19071` for staging and exfiltrating harvested credentials, browser session cookies, and system telemetry.

### Artifacts
- **Host Name:** Workstation-01
- **IP Address:** 10.0.2.15
- **User/S:** corporate\analyst
- **OS Type:** WINDOWS 10
- **Masqueraded Binary Name:** Wextract.exe
- **Target System Library:** ADVAPI32.dll (Token Manipulation & Privilege Escalation)
- **Egress Connectivity Check:** facebook.com
- **C2 Destination IP & Port:** 77.91.124.55:19071
- **Date/Time that the event was noticed:** 10/06/2023 04:41 UTC
- **Alert time stamp:** 10/06/2023 04:41 UTC
- **Number of systems identified:** 1 Endpoint (Workstation-01)

### Text explaining why below data is important to IOCs
The forensics indicate active host infection by the information-stealing Trojan RedLine Stealer (cataloged under the threat alias RECORDSTEALER). Masquerading as the legitimate Windows extraction tool (`Wextract.exe`) allows the malware to bypass basic process reputation checks, while interacting with `ADVAPI32.dll` facilitates access token theft and privilege escalation. The high-risk egress session over non-standard port 19071 confirms active staging and exfiltration of browser data, cryptocurrency wallet credentials, and host hardware telemetry directly to threat-actor-controlled infrastructure.

### Declaration
True Pos. - Issue, because of that was a dynamic incident.

### Recommendations
- **Isolation of the infected system:** Immediately isolate workstation Workstation-01 (10.0.2.15) from the network via EDR to prevent lateral movement and further exfiltration.
- **Blocking malicious network indicators:** Block the destination C2 IP address (77.91.124.55) and outbound traffic on non-standard port 19071 across perimeter firewalls and web gateways.
- **EDR Hash & Binary Blocking:** Block the file hash associated with the rogue `Wextract.exe` binary across all endpoints enterprise-wide.
- **Credential Revocation & Identity Reset:** Invalidate all active domain Kerberos tickets/tokens, force a password reset for compromised user `corporate\analyst`, and revoke all active corporate browser session cookies.
- **Endpoint Remediation:** Perform forensic triage on the host drive to identify the initial dropper path, terminate persistence mechanisms, and re-image the workstation prior to production redeployment.

### References
- https://cyberdefenders.org/
- https://www.virustotal.com/
- https://malpedia.caad.fkie.fraunhofer.de/
- https://bazaar.abuse.ch/

### OSINT Links
- **CyberDefenders Challenge Reference:** https://cyberdefenders.org/
- **VirusTotal Classification:** Categorized as Trojan (Initial submission: 2023-10-06 04:41 UTC)
- **Malpedia Threat Alias:** Confirmed as RedLine Stealer / RECORDSTEALER linked to C2 infrastructure: https://malpedia.caad.fkie.fraunhofer.de/details/win.recordstealer
- **MalwareBazaar Community Detection:** YARA detection rule `detect_Redline_Stealer` by author Varp0s: https://bazaar.abuse.ch/
