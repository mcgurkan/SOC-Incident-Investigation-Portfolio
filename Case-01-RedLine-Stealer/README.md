# Incident Ticket: [INC-20231006-041]

1. The program initiated post-execution network traffic to verify outbound internet connectivity by resolving the domain "facebook.com". Subsequently, it established a direct TCP socket connection from the endpoint to destination IP 77.91.124.55, port 19071.

2. The program executed under the binary name: "Wextract.exe" (masquerading as a legitimate Windows archive extraction utility to evade detection).

3. The executable dynamically interacted with "ADVAPI32.dll" to query security descriptors, manipulate access tokens, and escalate privileges for local credential harvesting.

### Artifacts:

* **Host name:** Workstation-01
* **IP Address:** 10.0.2.15
* **User/S:** corporate\analyst
* **OS Type:** WINDOWS 10
* **Date/Time that the event was noticed:** 10/06/2023 04:41 UTC
* **Alert time stamp:** 10/06/2023 04:41 UTC
* **Number of systems identified:** Endpoint

**Text explaining why below data is important to IOCs:**  
The forensics in the events indicate the execution of an information-stealing Trojan identified as RedLine Stealer (documented in Malpedia as RECORDSTEALER). A Trojan virus masquerades as legitimate software to infiltrate systems. Once active, it harvests stored credentials, browser session cookies, cryptocurrency wallets, and system hardware specifications before transmitting the data to remote Command and Control infrastructure over non-standard port 19071.

**Declaration:** True Pos. - Issue, because of that was a dynamic incident.

### Recommendations:
* Isolation of the infected system via EDR
* Blocking malicious destination IP (77.91.124.55) and port (19071) at boundary firewalls
* Blocking of the file hash in EDR across all endpoints
* Revocation and password resets for compromised user credentials and active browser sessions
* Have to remediate the infected system, remove persistence hooks, or rollback / re-image if needed

### References:
* https://www.virustotal.com/
* https://malpedia.caad.fkie.fraunhofer.de/
* https://bazaar.abuse.ch/
* https://cyberdefenders.org/

### OSINT Links:
* VirusTotal indicates initial submission was recorded at 2023-10-06 04:41 UTC and categorized as Trojan
* Malpedia confirms malware alias as RECORDSTEALER associated with IP 77.91.124.55: https://malpedia.caad.fkie.fraunhofer.de/details/win.recordstealer
* MalwareBazaar community YARA detection rule "detect_Redline_Stealer" by author Varp0s: https://bazaar.abuse.ch/
* C2 Destination IP and port: 77.91.124.55:19071
* Resolved egress check domain: facebook.com
* Target system library used for token manipulation: ADVAPI32.dll
