
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
┌┌──(user㉿kali)-[~]
└─$ nmap -sV --script=vuln 192.168.35.0/24
Starting Nmap 7.99 ( https://nmap.org ) at 2026-06-26 17:01 +0800
Nmap scan report for 192.168.35.1
Host is up (0.0015s latency).
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
|_    Couldn't find a file-type field.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
| http-slowloris-check:
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|     Disclosure date: 2009-09-17
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-enum:
|_  /manifest.json: Manifest JSON File

Nmap scan report for 192.168.35.10
Host is up (0.00078s latency).
Not shown: 987 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-06-26 09:01:45Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: LAB.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: LAB.local, Site: Default-First-Site-Name)
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: LAB.local, Site: Default-First-Site-Name)
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: LAB.local, Site: Default-First-Site-Name)
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-dombased-xss: Couldn't find any DOM based XSS.
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS.
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-054: false
|_samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR

Nmap scan report for 192.168.35.11
Host is up (0.0013s latency).
Not shown: 979 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
| ftp-vsftpd-backdoor:
|   VULNERABLE:
|   vsFTPd version 2.3.4 backdoor
|     State: VULNERABLE (Exploitable)
|     IDs:  BID:48539  CVE:CVE-2011-2523
|     Disclosure date: 2011-07-03
|     Exploit results:
|       Shell command: id
|       Results: uid=0(root) gid=0(root)
|     References:
|       https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/unix/ftp/vsftpd_234_backdoor.rb
|       http://scarybeastsecurity.blogspot.com/2011/07/alert-vsftpd-download-backdoored.html
|_      https://www.securityfocus.com/bid/48539
| vulners:
|   vsftpd 2.3.4:
|       CVE-2011-2523   10.0    https://vulners.com/cve/CVE-2011-2523
|_      (additional duplicate exploit-DB / GitHub PoC entries trimmed)
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| vulners:
|   cpe:/a:openbsd:openssh:4.7p1:
|       CVE-2023-38408  9.8     https://vulners.com/cve/CVE-2023-38408
|       CVE-2016-1908   9.8     https://vulners.com/cve/CVE-2016-1908
|       CVE-2010-4478   9.8     https://vulners.com/cve/CVE-2010-4478
|       CVE-2015-5600   8.5     https://vulners.com/cve/CVE-2015-5600
|       CVE-2016-6515   7.8     https://vulners.com/cve/CVE-2016-6515
|_      (~130 additional lower-severity / duplicate PoC entries trimmed)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
| ssl-poodle:
|   VULNERABLE:
|   SSL POODLE information leak
|     State: VULNERABLE
|     IDs:  BID:70574  CVE:CVE-2014-3566
|     Disclosure date: 2014-10-14
|     Check results:
|       TLS_RSA_WITH_AES_128_CBC_SHA
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566
|_      https://www.openssl.org/~bodo/ssl-poodle.pdf
|_sslv2-drown: ERROR: Script execution failed (use -d to debug)
| smtp-vuln-cve2010-4344:
|_  The SMTP server is not Exim: NOT VULNERABLE
| ssl-dh-params:
|   VULNERABLE:
|   Anonymous Diffie-Hellman Key Exchange MitM Vulnerability
|     State: VULNERABLE
|     Check results:
|       ANONYMOUS DH GROUP 1
|             Cipher Suite: TLS_DH_anon_WITH_DES_CBC_SHA
|             Modulus Length: 1024
|
|   Transport Layer Security (TLS) Protocol DHE_EXPORT Ciphers Downgrade MitM (Logjam)
|     State: VULNERABLE
|     IDs:  BID:74733  CVE:CVE-2015-4000
|     Disclosure date: 2015-5-19
|     Check results:
|       EXPORT-GRADE DH GROUP 1
|             Cipher Suite: TLS_DHE_RSA_EXPORT_WITH_DES40_CBC_SHA
|             Modulus Length: 512
|     References:
|       https://weakdh.org
|
|   Diffie-Hellman Key Exchange Insufficient Group Strength
|     State: VULNERABLE
|     Check results:
|       WEAK DH GROUP 1
|             Cipher Suite: TLS_DHE_RSA_WITH_AES_128_CBC_SHA
|             Modulus Length: 1024
|_      https://weakdh.org
53/tcp   open  domain      ISC BIND 9.4.2
| vulners:
|   cpe:/a:isc:bind:9.4.2:
|       CVE-2008-0122   10.0    https://vulners.com/cve/CVE-2008-0122
|       CVE-2021-25216  9.8     https://vulners.com/cve/CVE-2021-25216
|       CVE-2020-8616   8.6     https://vulners.com/cve/CVE-2020-8616
|       CVE-2016-1286   8.6     https://vulners.com/cve/CVE-2016-1286
|       CVE-2012-1667   8.5     https://vulners.com/cve/CVE-2012-1667
|_      (~60 additional lower-severity entries trimmed)
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
| vulners:
|   cpe:/a:apache:http_server:2.2.8:
|       CVE-2010-0425   10.0    https://vulners.com/cve/CVE-2010-0425
|       CVE-2009-3555   9.8     https://vulners.com/cve/CVE-2009-3555
|       CVE-2021-44790  9.8     https://vulners.com/cve/CVE-2021-44790
|       CVE-2017-7679   9.8     https://vulners.com/cve/CVE-2017-7679
|       CVE-2011-3192   7.8     https://vulners.com/cve/CVE-2011-3192
|_      (~200 additional lower-severity / duplicate entries trimmed)
|_http-sql-injection: ERROR: Script execution failed (use -d to debug)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-server-header: Apache/2.2.8 (Ubuntu) DAV/2
| http-slowloris-check:
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-fileupload-exploiter:
|_    Couldn't find a file-type field.
|_http-trace: TRACE is enabled
| http-enum:
|   /tikiwiki/: Tikiwiki
|   /test/: Test page
|   /phpinfo.php: Possible information file
|   /phpMyAdmin/: phpMyAdmin
|   /doc/: Potentially interesting directory w/ listing on 'apache/2.2.8 (ubuntu) dav/2'
|   /icons/: Potentially interesting folder w/ directory listing
|_  /index/: Potentially interesting folder
| http-csrf:
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=192.168.35.11
|   Found the following possible CSRF vulnerabilities:
|     Path: http://192.168.35.11:80/dvwa/login.php
|     Form action: login.php
|_    (additional TWiki-related CSRF form matches trimmed)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp  open  exec        netkit-rsh rexecd
513/tcp  open  login       OpenBSD or Solaris rlogind
514/tcp  open  tcpwrapped
1099/tcp open  java-rmi    GNU Classpath grmiregistry
| rmi-vuln-classloader:
|   VULNERABLE:
|   RMI registry default configuration remote code execution vulnerability
|     State: VULNERABLE
|_      https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/multi/misc/java_rmi_server.rb
1524/tcp open  bindshell   Metasploitable root shell
2121/tcp open  ftp         ProFTPD 1.3.1
| vulners:
|   cpe:/a:proftpd:proftpd:1.3.1:
|       CVE-2019-12815  9.8     https://vulners.com/cve/CVE-2019-12815
|       CVE-2011-4130   9.0     https://vulners.com/cve/CVE-2011-4130
|       CVE-2020-9272   7.5     https://vulners.com/cve/CVE-2020-9272
|       CVE-2016-3125   7.5     https://vulners.com/cve/CVE-2016-3125
|_      (~15 additional lower-severity entries trimmed)
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
| vulners:
|   cpe:/a:mysql:mysql:5.0.51a-3ubuntu5:
|       CVE-2017-15945  7.8     https://vulners.com/cve/CVE-2017-15945
|       CVE-2009-4028   6.8     https://vulners.com/cve/CVE-2009-4028
|_      (4 additional lower-severity entries trimmed)
|_ssl-ccs-injection: No reply from server (TIMEOUT)
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
| ssl-ccs-injection:
|   VULNERABLE:
|   SSL/TLS MITM vulnerability (CCS Injection)
|     State: VULNERABLE
|     Risk factor: High
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0224
|_      http://www.openssl.org/news/secadv_20140605.txt
| ssl-dh-params:
|   VULNERABLE:
|   Diffie-Hellman Key Exchange Insufficient Group Strength
|     State: VULNERABLE
|     Check results:
|       WEAK DH GROUP 1
|             Cipher Suite: TLS_DHE_RSA_WITH_AES_128_CBC_SHA
|             Modulus Length: 1024
|_      https://weakdh.org
| vulners:
|   cpe:/a:postgresql:postgresql:8.3:
|       CVE-2013-1903   10.0    https://vulners.com/cve/CVE-2013-1903
|       CVE-2013-1902   10.0    https://vulners.com/cve/CVE-2013-1902
|       CVE-2019-10211  9.8     https://vulners.com/cve/CVE-2019-10211
|       CVE-2015-3166   9.8     https://vulners.com/cve/CVE-2015-3166
|       CVE-2015-0244   9.8     https://vulners.com/cve/CVE-2015-0244
|_      (~270 additional lower-severity / duplicate entries trimmed)
| ssl-poodle:
|   VULNERABLE:
|   SSL POODLE information leak
|     State: VULNERABLE
|     IDs:  BID:70574  CVE:CVE-2014-3566
|     Check results:
|       TLS_RSA_WITH_AES_128_CBC_SHA
|_      https://www.openssl.org/~bodo/ssl-poodle.pdf
5900/tcp open  vnc         VNC (protocol 3.3)
6000/tcp open  X11         (access denied)
6667/tcp open  irc         UnrealIRCd
|_irc-unrealircd-backdoor: Looks like trojaned version of unrealircd. See http://seclists.org/fulldisclosure/2010/Jun/277
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
|_http-server-header: Apache-Coyote/1.1
| http-enum:
|   /admin/: Possible admin folder
|   /manager/html: Apache Tomcat (401 Unauthorized)
|   /manager/html/upload: Apache Tomcat (401 Unauthorized)
|   /admin/view/javascript/fckeditor/editor/filemanager/connectors/test.html: OpenCart/FCKeditor File upload
|   /admin/includes/FCKeditor/editor/filemanager/upload/test.html: ASP Simple Blog / FCKeditor File Upload
|   /admin/jscript/upload.html: Lizard Cart/Remote File upload
|_  /webdav/: Potentially interesting folder
|   (additional duplicate /admin/*.html and *.jsp login-page guesses trimmed)
| http-csrf:
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=192.168.35.11
|   Found the following possible CSRF vulnerabilities:
|     Path: http://192.168.35.11:8180/servlets-examples/servlet/RequestParamExample
|_    Form action: RequestParamExample
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-sql-injection: ERROR: Script execution failed (use -d to debug)
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-cookie-flags:
|   /admin/:
|     JSESSIONID:
|_      httponly flag not set
|   (same finding repeats across all enumerated /admin/* paths — trimmed for brevity)
| http-slowloris-check:
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|_      http://ha.ckers.org/slowloris/
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-regsvc-dos: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-061: false

Nmap scan report for 192.168.35.12
Host is up (0.0011s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_smb-vuln-ms10-061: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR
|_smb-vuln-ms10-054: false
|_samba-vuln-cve-2012-1182: Could not negotiate a connection:SMB: Failed to receive bytes: ERROR

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 256 IP addresses (4 hosts up) scanned in 412.62 seconds

                                                                    
```
#### Findings



### Zenmap Network Map Output
Using Zenmap to able to be able to visualize the network being scanned.

![zenmap](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/zenmap%20scan.png)


## Detection & Analysis

### Security Onion alerts

#### Alert Tab Screenshot
![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/sample%20ss%20secu%20onion.png)



### Alert for nmap -sN 192.168.35.0/24



| Attribute | Value |
| :--- | :--- |
| **Timestamp** | 2026-06-24T18:42:22.902Z |
| **Alert Signature** | `GPL SCAN PING NMAP ` |
| **Category** | ` Attempted Information Leak ` |
| **Severity** | ` Medium ` |

---

### Network Data
* **Source IP / Port:** ` 192.168.1.89 ` : ` `
* **Destination IP / Port:** `192.168.36.10-12 ` : ` `
* **Protocol:** ` TCP `
* **Interface:** ` bond0 `

---

### Log & Payload Extract

### Decoded Network Data
```http

```

### Forensic Context
* **Log Path:** ` `
* **Capture File:** ` `
* **Flow ID:** ` `
* **Rule UUID:** ` `
* **Rule Reference:** ` `

Alert Screenshot
![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20sn%20alert.png)




## Alert for nmap -st 192.168.35.0/24

### Security Onion Incident Report

| Attribute | Value |
| :--- | :--- |
| **Timestamp** | ` ` |
| **Alert Signature** | ` ` |
| **Category** | ` ` |
| **Severity** | ` ` |

---

### Network Data
* **Source IP / Port:** ` ` : ` `
* **Destination IP / Port:** ` ` : ` `
* **Protocol:** ` `
* **Interface:** ` `

---

### Log & Payload Extract

### Decoded Network Data
```http

```

### Forensic Context
* **Log Path:** ` `
* **Capture File:** ` `
* **Flow ID:** ` `
* **Rule UUID:** ` `
* **Rule Reference:** ` `

Alert Screenshot
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20st%20alert.png)



## Alert for nmap -sS 192.168.35.0/24

### Security Onion Incident Report

| Attribute | Value |
| :--- | :--- |
| **Timestamp** | `2026-06-24T18:52:11.682Z ` |
| **Alert Signature** | ` ET SCAN NMAP -sS window 1024 ` |
| **Category** | ` Attempted Information Leak ` |
| **Severity** | ` medium ` |

---

### Network Data
* **Source IP / Port:** ` 192.168.1.89 ` : ` 59849 `
* **Destination IP / Port:** ` 192.168.35.10-12 ` : ` 25 `
* **Protocol:** ` TCP `
* **Interface:** `bond0 `

---

### Log & Payload Extract

### Decoded Network Data
```http

```

### Forensic Context
* **Log Path:** ` `
* **Capture File:** ` `
* **Flow ID:** ` `
* **Rule UUID:** ` `
* **Rule Reference:** ` `

Alert Screenshot
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20ss%20alert.png)




## Alert for nmap -sV 192.168.35.0/24
### Security Onion Incident Report

| Attribute | Value |
| :--- | :--- |
| **Timestamp 1** | `2026-06-25 03:26:27.042 +08:00 ` |
| **Timestamp 2** | ` 2026-06-25 03:26:26.888 +08:00` |
| **Timestamp 3** | `2026-06-25 03:26:26.812 +08:00 ` |
| **Timestamp 4** | `2026-06-25 03:26:26.812 +08:00 ` |
| **Timestamp 5** | `2026-06-25 03:26:23.577 +08:00 ` |
| **Alert Signature 1** | ` ET SCAN Suspicious inbound to MSSQL port 1433 ` |
| **Alert Signature 2** | ` ET SCAN Suspicious inbound to PostgreSQL port 5432 ` |
| **Alert Signature 3** | `	ET SCAN NMAP -sS window 1024 ` |
| **Alert Signature 4** | `ET SCAN NMAP -sS window 1024 ` |
| **Alert Signature 5** | ` GPL SCAN PING NMAP ` |
| **Category 1** | `Potentially Bad Traffic ` |
| **Category 2** | `Potentially Bad Traffic ` |
| **Category 3** | `Attempted Information Leak ` |
| **Category 4** | `Attempted Information Leak ` |
| **Category 5** | `Attempted Information Leak ` |
| **Severity** | ` medium ` |

---

### Network Data
* **Source IP / Port:** ` 192.168.1.89 ` : `  62278 `
* **Destination IP / Port:** ` 192.168.35.10-12 ` : `1433,5432,32776,	7435 `
* **Protocol:** `TCP `
* **Interface:** `bond0 `

---

### Log & Payload Extract

### Decoded Network Data
```http

```

### Forensic Context
* **Log Path:** ` `
* **Capture File:** ` `
* **Flow ID:** ` `
* **Rule UUID:** ` `
* **Rule Reference:** ` `

Alert Screenshot
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20alert%20sv.png)




## Alert for nmap -O 192.168.35.0/24
### Security Onion Incident Report
| Attribute | Value |
| :--- | :--- |
| **Timestamp 1** | `2026-06-26 17:38:14.046 +08:00` |
| **Timestamp 2** | `2026-06-26 17:38:16.613 +08:00` |
| **Timestamp 3** | `2026-06-26 17:38:16.994 +08:00` |
| **Timestamp 4** | `2026-06-26 17:38:17.302 +08:00` |
| **Alert Signature 1** | `GPL SCAN PING NMAP` |
| **Alert Signature 2** | `ET SCAN NMAP -sS window 1024` |
| **Alert Signature 3** | `ET SCAN Suspicious inbound to MSSQL port 1433` |
| **Alert Signature 4** | `ET SCAN NMAP -sS window 1024` |
| **Category 1** | `Attempted Information Leak` |
| **Category 2** | `Attempted Information Leak` |
| **Category 3** | `Potentially Bad Traffic` |
| **Category 4** | `Attempted Information Leak` |
| **Severity** | `medium` |

---

### Network Data
* **Source IP / Port:** `192.168.1.89` : `50498`
* **Destination IP / Port:** `192.168.35.11-12` : `993,1433,1658`
* **Protocol:** `TCP`
* **Interface:** ` bond0 `
 
---

### Log & Payload Extract

### Decoded Network Data
```http

```

### Forensic Context
* **Log Path:** ` `
* **Capture File:** ` `
* **Flow ID:** ` `
* **Rule UUID:** ` `
* **Rule Reference:** ` `

Alert Screenshot
 ![alert](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/nmap%20o%20alert.png)


## Alert for  nmap -sV --script=vuln 192.168.35.0/24



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
