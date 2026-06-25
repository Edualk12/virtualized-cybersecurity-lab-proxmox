
# MITRE ATT&CK Mapping
### T1046 - Network Service Scanning

### Objective
To test the cybersecurity lab's functionality and test different attack methodologies.

## Environment
- Attacker: Kali Linux (192.168.1.89)
- Target: Victim Network (192.168.35.0/24)
- Monitoring: Security Onion, Splunk
- Tools: Nmap, Zenmap


## Attack Execution

### 1.) Check for Live Systems
  For the very step of the nmap scan I need know how many host are available in the network that I am scanning.

 #### Commands used
  ```
      nmap -sn 192.168.35.0/24
  ```
 #### Output:
 This command performs a quick ping sweep across the subnet to identify which host machines are active and online without scanning their ports. Attackers use these initial discovery findings to map out the network structure and isolate live targets for future exploitatio
```
┌──(klaude㉿klaudekali)-[~]
└─$ nmap -sn 192.168.35.0/24 

Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 02:42 +0800
Nmap scan report for 192.168.35.1
Host is up (0.0010s latency).
Nmap scan report for 192.168.35.10
Host is up (0.0010s latency).
Nmap scan report for 192.168.35.11
Host is up (0.0028s latency).
Nmap scan report for 192.168.35.12
Host is up (0.00098s latency).
Nmap done: 256 IP addresses (4 hosts up) scanned in 4.47 seconds

```
 #### Findings
  The nmap scan was able to detect four active host in the 192.168.35.0/24 subnet network, with this I am able to go onto the second step.

  
### 2.) Check for Open Ports

#### Commands used
  ```
    nmap -sT 192.168.35.0/24 (port scan)
    nmap -sS 192.168.35.0/24 (stealth scan)
    nmap -p 22,80,443 192.168.35.0/24 (specific port scan)
  ```

#### Ouput (Nmap -sT)
 This command is used to see if there are any live host with open port that can be used as vulnerable entry points for attack.
```
┌──(klaude㉿klaudekali)-[~]
└─$ nmap -sT 192.168.35.0/24
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 02:49 +0800
Nmap scan report for 192.168.35.1
Host is up (0.0024s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE
53/tcp  open  domain
80/tcp  open  http
443/tcp open  https

Nmap scan report for 192.168.35.10
Host is up (0.0084s latency).
Not shown: 987 closed tcp ports (conn-refused)
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

Nmap scan report for 192.168.35.11
Host is up (0.015s latency).
All 1000 scanned ports on 192.168.35.11 are in ignored states.
Not shown: 1000 closed tcp ports (conn-refused)

Nmap scan report for 192.168.35.12
Host is up (0.0036s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 256 IP addresses (4 hosts up) scanned in 9.24 seconds
```
#### Findings (nmap -sT)
This findings shows that the TCP connect scan discovered two live hosts on the network with vulnerable open entry points. Host 192.168.35.12 is exposing web and remote shell capabilities via SSH, HTTP, and HTTPS, while host 192.168.35.50 is exposing a Remote Desktop (RDP) management port.



#### Output (nmap -sS)
This command functions similarly to the TCP connect scan by actively scanning the target subnet for open ports. However, it provides a more stealthy option by utilizing half-open connections that never fully complete the handshake process on the live hosts.
```
┌──(klaude㉿klaudekali)-[~]
└─$ nmap -sS 192.168.35.0/24
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 2:52 +0800
Nmap scan report for 192.168.35.1
Host is up (0.00068s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE
53/tcp  open  domain
80/tcp  open  http
443/tcp open  https

Nmap scan report for 192.168.35.10
Host is up (0.00084s latency).
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

Nmap scan report for 192.168.35.11
Host is up (0.00096s latency).
All 1000 scanned ports on 192.168.35.11 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap scan report for 192.168.35.12
Host is up (0.00074s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 256 IP addresses (4 hosts up) scanned in 8.88 seconds
```
#### Findings (nmap -sS)
The output is similar to the -sS but it was still be able to be detected by Security Onion due to security onion scanning for traffic signatures with the half fired tcp scan being a suspicious pheonomena.

#### Output (nmap -p)
This is another variation of the -sT command, but it gives you the flexibility to scan only the specific ports you actually care about. Instead of wasting time checking all 1,000 default ports, it zeroes in on your target list, making the scan finish much faster and keeping your network traffic to a minimum.
```
┌──(klaude㉿klaudekali)-[~]
└─$ nmap -p 22,80,443 192.168.35.0/24
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 02:53 +0800
Nmap scan report for 192.168.35.1
Host is up (0.0016s latency).

PORT    STATE    SERVICE
22/tcp  filtered ssh
80/tcp  open     http
443/tcp open     https

Nmap scan report for 192.168.35.10
Host is up (0.0015s latency).

PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.35.11
Host is up (0.00067s latency).

PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https

Nmap scan report for 192.168.35.12
Host is up (0.00089s latency).

PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https

Nmap done: 256 IP addresses (4 hosts up) scanned in 5.75 seconds

``` 
      
#### Findings (mmap -p)
Same as Before It was able to find the inputed ports (22,80,443) which for the majority of the hosts are closed. Except for pfsense default gateway.



 ### 3.) Perform Banner Grabbing
#### Commands used
  ```
  nmap -sV --script=banner 192.168.35.0/24 (for  service version detection)
  nmap -O 192.168.35.0/24 (for os detection )
  ```
  
#### Output (nmap -sV)

This command scans all 254 active network hosts in the 192.168.35.0/24 subnet to identify open ports and determine their exact application names and software versions. It also extracts the raw welcome text ("banners") broadcasted by those services.

```
┌──(klaude㉿klaudekali)-[~]
└─$ nmap -sV --script=banner 192.168.35.0/24
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 03:26 +0800
Nmap scan report for 192.168.35.1
Host is up (0.00072s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
53/tcp  open  domain   (generic dns response: REFUSED)
80/tcp  open  http     nginx
443/tcp open  ssl/http nginx
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.99%I=7%D=6/25%Time=6A3D03F0%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,E,"\0\x0c\0\x06\x81\x05\0\0\0\0\0\0\0\0");

Nmap scan report for 192.168.35.10
Host is up (0.00077s latency).
Not shown: 987 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-06-25 10:33:15Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
|_banner: ncacn_http/1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: KLAUDE.local, Site: Default-First-Site-Name)
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: KLAUDE-DOMAIN-C; OS: Windows; CPE: cpe:/o:microsoft:windows

Nmap scan report for 192.168.35.11
Host is up (0.00073s latency).
All 1000 scanned ports on 192.168.35.11 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap scan report for 192.168.35.12
Host is up (0.00067s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 256 IP addresses (4 hosts up) scanned in 74.03 seconds
```

#### Findings
This version detection scan uses Nmap Scripting Engine (NSE) scripts to dig deeper, specifically running the banner script to grab the raw welcome text from open services. These automated NSE scripts act like plugins that extend Nmap's capability beyond basic port checking, allowing it to accurately unmask an Nginx gateway, a Windows Active Directory Domain Controller, and a host on this subnet.


#### Output
This command is used specifically to see what type of Operating do the hosts use which can be later be utilized to what kind of exploit should be used against each host in the subnet.
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
#### Findings
This shows some of the open port and services that are available in the previous command and also the details of the operating system being used such as Windows 10, Linux Ubuntu, Windows 10 2022 Server, and Free BSD which matched the OS that is included in the network topology.

   
 ### 4.) Scan for Vulnerabilties
- Commands used
  ```
    nmap -sV --script=vuln 192.168.35.0/24
  ```
  
#### Output
   This is another example of a Nmap Script Engine which in this case automates to look for any vulnerabilities on the host.Th is command weaponizes Nmap to look for known security flaws, bugs, and misconfigurations.  
```    
┌──(klaude㉿klaudekali)-[~]
└─$ nmap -sV --script=vuln 192.168.35.0/24 

Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-25 03:31 +0800
Nmap scan report for 192.168.35.1
Host is up (0.00099s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
53/tcp  open  domain   (generic dns response: REFUSED)
80/tcp  open  http     nginx
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
443/tcp open  ssl/http nginx
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-sql-injection: ERROR: Script execution failed (use -d to debug)
| http-fileupload-exploiter: 
|   
|     Couldn't find a file-type field.
|   
|     Couldn't find a file-type field.
|   
|_    Couldn't find a file-type field.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-enum: 
|_  /manifest.json: Manifest JSON File
|_http-csrf: Couldn't find any CSRF vulnerabilities.
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.99%I=7%D=6/25%Time=6A3D2FF0%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,E,"\0\x0c\0\x06\x81\x05\0\0\0\0\0\0\0\0");

Nmap scan report for 192.168.35.10
Host is up (0.00062s latency).
Not shown: 987 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-06-25 13:40:59Z)
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
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
Service Info: Host: KLAUDE-DOMAIN-C; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR

Nmap scan report for 192.168.35.11
Host is up (0.00062s latency).
All 1000 scanned ports on 192.168.35.11 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap scan report for 192.168.35.12
Host is up (0.00079s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 256 IP addresses (4 hosts up) scanned in 217.00 seconds
                                                                    
```
#### Findings
This nmap scan script was the longest one to complete as it uses different components to see what vulnerabilities are present but the scan was not able to find any immediate high-severity exploits or unpatched system vulnerabilities as it indicates in the attack output being false and failed to see any XSS and CSRF vulnerabilities. But there are open ports in the Active Directory Host:

- Port 88 (Kerberos): This handles network authentication.
- Port 389 & 636 (LDAP/LDAPS): This holds the database of all users, groups, and computers. 
- Port 445 (SMB): This is used for file sharing and network communication. 
- Port 5985 (WinRM): This is the Windows Remote Management port. 


### Zenmap Network Map Output
Using Zenmap to able to be able to visualize the network being scanned.

![zenmap](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/zenmap%20scan.png)


## Detection & Analysis

### Security Onion alerts

#### Alert Tab Screenshot
![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/sample%20ss%20secu%20onion.png)



### Alert for nmap -sN 192.168.35.0/24
![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sn%20alert.png)




### Alert for nmap -st 192.168.35.0/24
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20st%20alert.png)



### Alert for nmap -sS 192.168.35.0/24
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20ss%20alert.png)




### Alert for nmap -sV 192.168.35.0/24
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sv%20script%20alert.png)




### Alert for nmap -O 192.168.35.0/24
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20o%20alert.png)


### Mitigation

- Restrict unnecessary open ports and services.
- Implement VLAN segmentation to limit network visibility.
- Apply firewall rules to control access between hosts.
- Restrict administrative services to trusted systems.
- Monitor reconnaissance activity using Security Onion and Suricata.

### Lessons Learned
This exercise showed how reconnaissance activity can be detected in a monitored network. It helped me understand how traffic flows through pfSense, Open vSwitch, and Security Onion, and how IDS alerts are generated from real attack activity.

-Validated end-to-end attack detection using Security Onion.
-Confirmed proper VLAN and SPAN configuration.
-Improved understanding of network monitoring and intrusion detection.
