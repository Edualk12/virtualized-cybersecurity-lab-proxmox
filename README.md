# **Cybersecurity Testing and Monitoring Lab Using Proxmox VE**

![Proxmox VE](https://img.shields.io/badge/Proxmox_VE-E57000?style=for-the-badge&logo=proxmox&logoColor=white)
![pfSense](https://img.shields.io/badge/pfSense-212121?style=for-the-badge&logo=pfsense&logoColor=white)
![Security Onion](https://img.shields.io/badge/Security_Onion-4E944F?style=for-the-badge&logo=linux&logoColor=white)
![Splunk](https://img.shields.io/badge/Splunk-000000?style=for-the-badge&logo=splunk&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Windows Server](https://img.shields.io/badge/Windows_Server-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Windows_10-0078D6?style=for-the-badge&logo=windows&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Virtual Networking](https://img.shields.io/badge/Virtual_Networking-2496ED?style=for-the-badge&logo=cisco&logoColor=white)
![IDS/IPS](https://img.shields.io/badge/IDS%2FIPS-D72638?style=for-the-badge&logo=shieldsdotio&logoColor=white)
![SIEM](https://img.shields.io/badge/SIEM-2D3142?style=for-the-badge&logo=splunk&logoColor=white)
![Metasploitable 2](https://img.shields.io/badge/Metasploitable_2-black?style=for-the-badge&logo=linux&logoColor=white)
![Nmap](https://img.shields.io/badge/Nmap-0078D7?style=for-the-badge&logo=nmap&logoColor=white)

## Network / Security Architecture

![diagram](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/CYBER%20HOMELAB.png)


## Skills Demonstrated
- Firewall configuration
- Vlan configuration
- Hypervisor Configuration and Administration
- Network Architecture
  

## Tools and Technologies Used
- Proxmox Virtual Environemnt (Hypervisor)
- Virtualization
- PFsense (firewall)
- Windows 10
- Windows Server 2022
- Security Onion
- Splunk
- Vlans

## Dashboards

### Security Onion

![seconion](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/security%20onion%20ss.png)

### Proxmox Virtual Environment

![proxmox](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/proxmox%20ss.png)



# Sample Attack Scenarios and Detection Analysis

### Attack 1: Nmap Reconnaissance & Detection

- [Attack 1: Nmap Reconnaissance](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/attacks/Attack1.md)


## PC SPECS

### Dell Optiplex 3050 

Using a Mini-PC for my Proxmox VE is the most practical and common choice as it is power efficient and the specs you can get at a relatively cheaper price.


| CPU | Core # |
|-----|-----|
| Intel i5-8500t | 6 Cores |

| RAM | Capacity |
|-----|-----|
| Kingston DDR4 | 16 Gb |

| Storage Type | Capacity |
|-----|-----|
| Kingston M.2 SSD | 256 Gb |

## PROXMOX VE (VIRTUAL MACHINE RESOURCE ALLOCATION)

I've installed proxmox VM as my Hypervisor as it is free and open source and also multiple different features to configure and to learn more about hypervisors and setting up virtual machines.

I've configured different Virtual Machines for different purposes in the nextwork and also their specifications and optimized it to fit my limited resources as shown below

| Virtual Machine | Operating System | Role/Purpose | CPU | RAM | Storage |
|-----------------|------------------|--------------|------|-----|---------|
| Proxmox VE | Proxmox VE | Hypervisor hosting the virtual cybersecurity lab | 1 Core | 1 GB | - |
| pfSense | pfSense | Firewall, router, and network segmentation | 1 Core | 2 GB | 8 GB |
| AD Server | Windows Server 2022 | Active Directory, DNS, and enterprise services | 2 Cores | 2 GB | 35 GB |
| Security Onion | Security Onion | IDS/IPS, network monitoring, and threat detection | 4 Cores | 8 GB | 100 GB |
| PC-1 (Victim PC) | Windows 10 | User workstation and attack target | 2 Cores | 2 GB | 32 GB |
| Splunk Server | Ubuntu Server | SIEM, log collection, and security analysis | 2 Cores | 2 GB | 32 GB |
| Metasploitable 2 | Linux | Attack Target | 1 Core | 2 GB | 2 GB |

## Virtual Machines


### SECURITY ONION

 Using Security Onion is one of the most versatile and useful SIEM available due to the different tools and features present but the limited resources I have limited me to put the majority of my computing Resources to it.

### Splunk

Used for monitoring and logging the raw data coming from the Active Directory to allow real time detection of attacks.

### Active Directory 

Contains all the information of all its client PCs every user identity, password, and access privilege. So it is a primary target that is why it is included for Pentesting.

### Metasploitable 2

Metasploitable 2 is an intentionally vulnerable Linux virtual machine designed as a legal, safe training ground for security professionals to practice penetration testing and vulnerability exploitation.

## Network Segmentation and Firewall Configuration

### Router Static Route Config

Due to my Attacker Kali Linux machine being in a separate "network" , I need to cofigure a static router coming from my Kali Linux VM to the different VLANs in the virtual network.

```
Router> enable
Router# configure terminal
Router(config)# ip route [destination-network] [subnet-mask] [next-hop-ip]
```

## PFSense Vlan Configuration

I configured vlans for the different virtual machine for added security and manageability. Configuring pfsense firewall is straightforward after the initial setup using the Web interface.

![pfsense](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/PFsense.png)

| Interface | VLAN/Network Name | IP Address / Subnet | Purpose |
|-----------|-------------------|---------------------|---------|
| WAN | WAN | 192.168.1.97/24 | Internet access and external network connection |
| LAN | LAN | 192.168.15.1/24 | Internal management network |
| OPT1 | OPT1 | 192.168.25.1/24 | Additional internal network segment |
| OPT2 | VICTIM_NETWORK_LAN | 192.168.35.1/24 | Victim machines and attack target network |
| OPT3 | SPLUNK | 192.168.45.1/24 | SIEM server and log analysis network |

## Configuring Security Onion Monitoring Interface using Open V Switch

Based from my experience that i have encountered during creating the netwokr is that I was not able to use the built in span port configuration from pfsense so based on my research one of the most common setup is to use Open Vswitch for connection the monitoring interface of Security Onion to the Virtual Machines. So I created a separate virutal interface for my network (vmbr3) and for the sniffer interface is (vmbr2) so i will be mirroring the traffic from thr virtual bridge 3 (vmbr3) to the sniffer inteface.

![pfsense](https://github.com/Edualk12/virtualized-cybersecurity-lab-proxmox/blob/main/images/spanport.png)


## Port mirroring from vmbr3 to vmbr2

### Patching vmbr2 and 3
```
  # Connect vmbr3 to vmbr2
ovs-vsctl add-port vmbr3 patch-to-vmbr2 \
  -- set Interface patch-to-vmbr2 type=patch options:peer=patch-to-vmbr3

# Connect vmbr2 back to vmbr3
ovs-vsctl add-port vmbr2 patch-to-vmbr3 \
  -- set Interface patch-to-vmbr3 type=patch options:peer=patch-to-vmbr2
```

### Mirroring Vmbr3 to Vmbr2

```
  ovs-vsctl -- set bridge vmbr3 mirrors=@m \
  -- --id=@m create mirror name=bridge-span \
  select_all=true \
  output_port=$(ovs-vsctl get port patch-to-vmbr2 _uuid)
```

### Proxmox Network Interface Configuration
``` auto lo
iface lo inet loopback

auto enp1s0
iface enp1s0 inet manual

iface wlp2s0 inet manual

auto vmbr0
iface vmbr0 inet static
        address 192.168.1.12/24
        gateway 192.168.1.1
        bridge-ports enp1s0
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094

auto vmbr1
iface vmbr1 inet manual
        ovs_type OVSBridge

auto vmbr2
iface vmbr2 inet manual
        ovs_type OVSBridge
        ovs_ports patch-to-vmbr3 tap102i1


auto vmbr3
iface vmbr3 inet manual
        ovs_type OVSBridge
```




## Problems Encountered

- The Problem: I could ping outward from my VMs to my physical PC, and I could ping the pfSense WAN port from the outside, but I couldn't actually ping into the internal VM network from my physical PC.

- The Root Cause: My main home router was completely blind to the VM subnet. When my PC tried to talk to a VM, the router didn't even consider the pfSense WAN interface as an option—it just assumed the traffic was meant for the internet and dropped it.

- The Fix: I added a static route on my main home router telling it exactly how to find the VM subnet via the pfSense WAN IP, and opened up a firewall rule on pfSense to let the traffic cross over to the LAN.

- metapsplitable is not compatible with peroxmox

- Limited resources for the different virtual machines

## Lessons Learned

## Future Improvements
