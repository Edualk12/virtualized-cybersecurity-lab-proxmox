
## Objective
To test the cybersecurity lab's functionality and test different attack methodologies.

## Environment
- Attacker: Kali Linux (192.168.1.89)
- Target: Victim Network (192.168.35.0/24)
- Monitoring: Security Onion, Splunk

## MITRE ATT&CK Mapping
### T1046 - Network Service Scanning

## Attack Execution

### 1.) Check for Live Systems
- Commands used
  ```
      nmap -sn 192.168.35.0/24
      nmap -PS 192.168.35.0/24
  ```
  
- Findings
- Screenshot

### 2.) Check for Open Ports
- Commands used
  ```
    nmap -sT 192.168.35.0/24
    nmap -sS 192.168.35.0/24
    nmap -p 22,80,443 192.168.35.0/24
  ```
  
- Findings
- Screenshot


 ### 3.) Perform Banner Grabbing
- Commands used
- Findings
- Screenshot
   
 ### 4.) Scan for Vulnerabilties
- Commands used
- Findings
- Screenshot
   

### Detection & Analysis
- Security Onion alerts


- Splunk logs



- Relevant Event IDs




- Network traffic evidence



### Indicators of Compromise (IoCs)
- Source IP
- Destination IP
- Suspicious processes/files
- Network signatures

### Mitigation
- Immediate containment
- Long-term security improvements

### Lessons Learned
- Key takeaways from the attack
