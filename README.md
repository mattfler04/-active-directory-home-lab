# Active Directory Home Lab

A hands-on Windows Server 2025 Active Directory home lab built with UTM on macOS.

This project documents the complete setup process, configuration, troubleshooting, and Help Desk scenarios completed in the lab.

---

# Table of Contents

1. Lab Overview
2. Lab Environment
3. Create Windows Server VM
4. Install Windows Server 2025
5. Rename Server to DC01
6. Configure Static IP Address
7. Install Active Directory Domain Services
8. Promote Server to Domain Controller
9. Configure DNS
10. Test DNS
11. Create Organizational Units
12. Create Users
13. Create Security Groups
14. Add Users to Groups
15. Configure Group Policy
16. Create Windows 11 Client VM
17. Configure Client Network
18. Troubleshoot Client DNS
19. Discover the Domain Controller
20. Join Windows 11 Client to Domain
21. Log Into the Domain
22. Verify Domain Authentication
23. Active Directory Account Compromise Scenario
24. Account Lockout Scenario
25. Account Lockout Policy
26. User Login Troubleshooting
27. Internet Troubleshooting
28. DNS Troubleshooting
29. Domain Join Troubleshooting
30. Group Policy Troubleshooting
31. Active Directory User Troubleshooting
32. Event Viewer Investigation
33. PowerShell Troubleshooting
34. General Help Desk Troubleshooting Process
35. Skills Demonstrated
36. Future Labs

---

# 1. Lab Overview

I built a Windows Server 2025 Active Directory environment and connected a Windows 11 client computer to the domain.

The purpose of this project was to gain hands-on experience with:

- Windows Server
- Active Directory
- Active Directory Users and Computers
- Active Directory Domain Services
- DNS
- Group Policy
- Organizational Units
- Security Groups
- User Accounts
- Domain Controllers
- Windows 11 domain clients
- Domain authentication
- Account security
- Account lockout troubleshooting
- Event Viewer
- Network troubleshooting
- DNS troubleshooting
- Domain join troubleshooting
- Help Desk troubleshooting

---

# 2. Lab Environment

## Host Computer

- Mac mini
- 24 GB RAM
- macOS
- UTM virtualization software
- External SSD used for virtual machine storage

## Server

- Windows Server 2025
- Computer name: DC01
- Domain Controller
- Domain: matthias.local
- IP Address: 192.168.64.2
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.64.1
- Preferred DNS: 192.168.64.2

## Client

- Windows 11 Pro
- ARM64 virtual machine
- 6 GB RAM
- 4 CPU cores
- Approximately 64 GB virtual disk
- Joined to matthias.local

---

# 3. Create Windows Server VM

## Step 1

Opened UTM on the Mac.

Created a new virtual machine.

Selected the Windows Server 2025 installation ISO.

Configured the virtual machine with:

- CPU
- Memory
- Storage
- Network adapter

Started the virtual machine.

---

# 4. Install Windows Server 2025

## Step 1

Started the Windows Server virtual machine.

Booted from the Windows Server 2025 ISO.

## Step 2

Completed the Windows Server installation.

Created the initial Administrator account.

Logged into Windows Server.

---

# 5. Rename Server to DC01

After Windows Server was installed, renamed the computer to:

DC01

The server was restarted after changing the computer name.

The server was then used as the Domain Controller.

---

# 6. Configure Static IP Address

A static IP address was configured so that the Domain Controller would always use the same address.

## Step 1

Opened:

Control Panel

## Step 2

Selected:

Network and Internet

## Step 3

Selected:

Network and Sharing Center

## Step 4

Selected:

Change adapter settings

## Step 5

Right-clicked:

Ethernet

Selected:

Properties

## Step 6

Selected:

Internet Protocol Version 4 (TCP/IPv4)

Clicked:

Properties

## Step 7

Selected:

Use the following IP address.

Configured:

IP Address:

192.168.64.2

Subnet Mask:

255.255.255.0

Default Gateway:

192.168.64.1

Preferred DNS Server:

192.168.64.2

## Step 8

Clicked OK.

The Domain Controller now uses a static IP address.

---

# 7. Install Active Directory Domain Services

## Step 1

Opened:

Server Manager

## Step 2

Selected:

Manage

Then:

Add Roles and Features

## Step 3

Selected:

Role-based or feature-based installation

## Step 4

Selected the local server.

## Step 5

Selected:

Active Directory Domain Services

## Step 6

Accepted the required features.

Clicked:

Add Features

## Step 7

Continued through the installation wizard.

Clicked:

Install

Waited for the installation to finish.

---

# 8. Promote Server to Domain Controller

After installing Active Directory Domain Services, promoted the server to a Domain Controller.

## Step 1

Opened Server Manager.

Clicked the notification flag.

Selected:

Promote this server to a domain controller

## Step 2

Selected:

Add a new forest

## Step 3

Entered the domain name:

matthias.local

## Step 4

Configured the Domain Controller Options.

Enabled:

- Domain Name System (DNS) server
- Global Catalog (GC)

Left:

- Read-only domain controller (RODC)

disabled.

## Step 5

Created the Directory Services Restore Mode password.

## Step 6

Left the default database, log files, and SYSVOL paths.

## Step 7

Completed the prerequisite check.

Clicked:

Install

## Step 8

The server restarted.

The server was now the Domain Controller for:

matthias.local

---

# 9. Configure DNS

DNS was installed automatically as part of the Domain Controller setup.

The DNS zone was created for:

matthias.local

The Domain Controller was configured to use itself as the preferred DNS server.

DC01 DNS:

192.168.64.2

---

# 10. Test DNS

## Test 1

Opened Command Prompt.

Ran:

```cmd
nslookup matthias.local