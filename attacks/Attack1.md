
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

![nmapsv](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sv%20script.png)

```
  ┌──(klaude㉿klaudekali)-[~]
└─$ nmap -O 192.168.35.0/24
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 03:44 +0800
Nmap scan report for 192.168.35.1
Host is up (0.00072s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE
53/tcp  open  domain
80/tcp  open  http
443/tcp open  https
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): FreeBSD 11.X (97%)
OS CPE: cpe:/o:freebsd:freebsd:11.2
Aggressive OS guesses: FreeBSD 11.2-RELEASE (97%)
No exact OS matches for host (test conditions non-ideal).

Nmap scan report for 192.168.35.10
Host is up (0.0014s latency).
Not shown: 987 closed tcp ports (reset)
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
5357/tcp open  wsdapi
5985/tcp open  wsman
Device type: general purpose
Running: Microsoft Windows 2022
OS CPE: cpe:/o:microsoft:windows_server_2022
OS details: Microsoft Windows Server 2022
Network Distance: 3 hops

Nmap scan report for 192.168.35.11
Host is up (0.00091s latency).
All 1000 scanned ports on 192.168.35.11 are in ignored states.
Not shown: 1000 closed tcp ports (reset)
Too many fingerprints match this host to give specific OS details
Network Distance: 3 hops

Nmap scan report for 192.168.35.12
Host is up (0.00093s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
Device type: general purpose
Running: Microsoft Windows 10
OS CPE: cpe:/o:microsoft:windows_10
OS details: Microsoft Windows 10 1903 - 22H2
Network Distance: 3 hops

OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 256 IP addresses (4 hosts up) scanned in 15.77 seconds

```

   
 ### 4.) Scan for Vulnerabilties
- Commands used
  ```
    nmap -sV --script=vuln 192.168.35.0/24
  ```
  
- Findings
- Screenshot
   

```    
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 03:31 +0800
Nmap scan report for 192.168.35.1
Host is up (0.0012s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
53/tcp  open  domain   (generic dns response: REFUSED)
80/tcp  open  http     nginx
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
443/tcp open  ssl/http nginx
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-fileupload-exploiter: 
|   
|     Couldn't find a file-type field.
|   
|     Couldn't find a file-type field.
|   
|_    Couldn't find a file-type field.
|_http-sql-injection: ERROR: Script execution failed (use -d to debug)
| http-enum: 
|_  /manifest.json: Manifest JSON File
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.99%I=7%D=6/25%Time=6A3C30B0%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,E,"\0\x0c\0\x06\x81\x05\0\0\0\0\0\0\0\0");

Nmap scan report for 192.168.35.10
Host is up (0.00061s latency).
Not shown: 987 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-06-24 19:31:55Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
Service Info: Host: KLAUDE-DOMAIN-C; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR
|_samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR

Nmap scan report for 192.168.35.11
Host is up (0.00066s latency).
```



### Detection & Analysis
- Security Onion alerts

- nmap sn alert

 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sn%20alert.png)

- st alert
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20st%20alert.png)

- ss alert
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20ss%20alert.png)

- sv alert
  
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sv%20script%20alert.png)

 - o alert
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20o%20alert.png)


 ![alert]()
 ![alert]()



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
