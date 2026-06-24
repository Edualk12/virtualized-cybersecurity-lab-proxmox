
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
  ```
  
- Findings
  
- Screenshot

  
  ![nmap](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sn.png)
  
### 2.) Check for Open Ports
- Commands used
  ```
    nmap -sT 192.168.35.0/24 (port scan)
    nmap -sS 192.168.35.0/24 (stealth scan)
    nmap -p 22,80,443 192.168.35.0/24 (specific port scan)
  ```

  
- Findings

  
- Screenshot


  ![nmap](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20st.png)

  ![nmap](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20ss.png)
  
  ![nmap](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20p.png)
    




 ### 3.) Perform Banner Grabbing
- Commands used
  ```
  nmap -O 192.168.35.0/24 (for os detection )
  nmap -sV --script=banner 192.168.35.0/24 (for  service version detection)
  ```
- Findings
- Screenshot


   
 ### 4.) Scan for Vulnerabilties
- Commands used
  ```
    nmap -sV --script=vuln 192.168.35.0/24
  ```
  
- Findings
- Screenshot
   
  ![nmap](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/tree/main/images
)



### Detection & Analysis
- Security Onion alerts

- nmap sn alert

 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sn%20alert.png)






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
