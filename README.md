ACTIVE DIRECTORY HOME LAB — GITHUB STRUCTURE
==============================================

REPOSITORY NAME
----------------

active-directory-home-lab


REPOSITORY DESCRIPTION
----------------------

Windows Server 2025 Active Directory home lab demonstrating AD DS, DNS,
Group Policy, Windows 11 domain joining, user administration, networking,
security troubleshooting, and Help Desk scenarios.


REPOSITORY STRUCTURE
--------------------

active-directory-home-lab/
│
├── README.md
│
├── screenshots/
│   │
│   ├── 01-server-manager.png
│   ├── 02-dc01-static-ip.png
│   ├── 03-active-directory.png
│   ├── 04-dns-manager.png
│   ├── 05-organizational-units.png
│   ├── 06-users.png
│   ├── 07-security-groups.png
│   ├── 08-group-policy.png
│   ├── 09-windows-client.png
│   ├── 10-client-ipconfig.png
│   ├── 11-domain-join.png
│   ├── 12-domain-login.png
│   ├── 13-nltest.png
│   ├── 14-event-viewer.png
│   └── 15-help-desk-scenario.png
│
├── documentation/
│   │
│   ├── network-configuration.md
│   ├── active-directory-configuration.md
│   ├── group-policy.md
│   ├── dns-troubleshooting.md
│   ├── domain-join-troubleshooting.md
│   └── help-desk-scenarios.md
│
└── labs/
    │
    ├── compromised-account.md
    ├── locked-account.md
    ├── no-internet.md
    ├── dns-failure.md
    ├── domain-join-failure.md
    └── group-policy-failure.md


==================================================
README.md
==================================================

# Active Directory Home Lab

## Overview

I built a Windows Server 2025 Active Directory home lab using UTM on a
Mac mini.

The goal of this project is to gain hands-on experience with Windows
administration, Active Directory, DNS, Group Policy, networking, security,
and Help Desk troubleshooting.

This lab is designed to simulate common IT Support and Help Desk scenarios
that can occur in a business environment.

## Technologies Used

- Windows Server 2025
- Windows 11 Pro
- Active Directory Domain Services
- DNS
- Group Policy
- Active Directory Users and Computers
- Event Viewer
- Command Prompt
- PowerShell
- UTM
- macOS
- Virtual Machines

## Lab Environment

### Domain Controller

Computer Name:

DC01

Operating System:

Windows Server 2025

Domain:

matthias.local

IP Address:

192.168.64.2

Subnet Mask:

255.255.255.0

Gateway:

192.168.64.1

DNS:

192.168.64.2

### Windows Client

Operating System:

Windows 11 Pro

Domain:

matthias.local

Test User:

ajohnson

## Active Directory Structure

matthias.local

├── IT
│   └── Jordan Williams
│
└── Help Desk
    └── Alex Johnson

## Security Groups

- IT Support
- Help Desk

Jordan Williams:

- IT Support

Alex Johnson:

- Help Desk


==================================================
1. WINDOWS SERVER 2025 INSTALLATION
==================================================

### Steps

1. Open UTM.
2. Create a new virtual machine.
3. Select the Windows Server 2025 ISO.
4. Configure the virtual machine.
5. Configure the CPU and RAM.
6. Create the virtual disk.
7. Store the virtual machine on the external SSD.
8. Start the virtual machine.
9. Install Windows Server 2025.
10. Create the Administrator password.
11. Complete the installation.
12. Log into Windows Server.


==================================================
2. RENAME SERVER
==================================================

### Steps

1. Open Settings.
2. Select System.
3. Select About.
4. Select the option to rename the PC.
5. Enter:

DC01

6. Apply the change.
7. Restart the server.
8. Log back into Windows Server.

### Verification

1. Open Command Prompt.
2. Run:

hostname

3. Verify that the computer name is:

DC01


==================================================
3. STATIC IP CONFIGURATION
==================================================

### Steps

1. Open Control Panel.
2. Select Network and Internet.
3. Select Network and Sharing Center.
4. Select Change adapter settings.
5. Right-click Ethernet.
6. Select Properties.
7. Select Internet Protocol Version 4 (TCP/IPv4).
8. Click Properties.
9. Select Use the following IP address.
10. Enter:

IP Address: 192.168.64.2
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.64.1
Preferred DNS: 192.168.64.2

10. Click OK.
11. Close the network settings.

### Verification

1. Open Command Prompt.
2. Run:

ipconfig /all

3. Verify the IP address.
4. Verify the gateway.
5. Verify the DNS server.


==================================================
4. ACTIVE DIRECTORY DOMAIN SERVICES
==================================================

### Steps

1. Open Server Manager.
2. Click Manage.
3. Select Add Roles and Features.
4. Select Role-based or feature-based installation.
5. Select DC01.
6. Select Active Directory Domain Services.
7. Click Add Features.
8. Continue through the wizard.
9. Click Install.
10. Wait for the installation to complete.


==================================================
5. DOMAIN CONTROLLER PROMOTION
==================================================

### Steps

1. Open Server Manager.
2. Click the notification flag.
3. Select Promote this server to a domain controller.
4. Select Add a new forest.
5. Enter:

matthias.local

6. Make sure DNS Server is selected.
7. Make sure Global Catalog is selected.
8. Leave Read-only domain controller unchecked.
9. Create the DSRM password.
10. Continue through the wizard.
11. Leave the default AD database paths.
12. Review the configuration.
13. Click Install.
14. Allow DC01 to restart.
15. Log back into the server.


==================================================
6. VERIFY ACTIVE DIRECTORY
==================================================

### Steps

1. Open Server Manager.
2. Select Tools.
3. Open Active Directory Users and Computers.
4. Expand:

matthias.local

5. Verify that the domain exists.


==================================================
7. DNS CONFIGURATION
==================================================

### Steps

1. Open Server Manager.
2. Select Tools.
3. Open DNS.
4. Expand Forward Lookup Zones.
5. Locate:

matthias.local

6. Verify that the DNS zone exists.

### DNS Test

1. Open Command Prompt.
2. Run:

nslookup matthias.local

3. Verify that the domain resolves to:

192.168.64.2


==================================================
8. DNS TROUBLESHOOTING
==================================================

### Problem

nslookup displays:

Server: Unknown

### Steps

1. Open Command Prompt.
2. Run:

nslookup matthias.local

3. Verify that the domain resolves to:

192.168.64.2

4. Test the Active Directory SRV record:

nslookup -type=SRV _ldap._tcp.dc._msdcs.matthias.local 192.168.64.2

5. Verify that the result identifies:

DC01.matthias.local

6. Verify that the IP address is:

192.168.64.2

### Result

The DNS service was working even though nslookup displayed the DNS server
as Unknown.


==================================================
9. ORGANIZATIONAL UNITS
==================================================

The following OUs were created:

- IT
- Help Desk

### Steps

1. Open Active Directory Users and Computers.
2. Expand matthias.local.
3. Right-click the domain.
4. Select New.
5. Select Organizational Unit.
6. Enter:

IT

7. Click OK.
8. Repeat the process.
9. Create:

Help Desk

### Final Structure

matthias.local

├── IT
└── Help Desk


==================================================
10. USER ACCOUNTS
==================================================

### Jordan Williams

Username:

jwill​​iams

### Steps

1. Open Active Directory Users and Computers.
2. Open the IT OU.
3. Right-click an empty area.
4. Select New.
5. Select User.
6. Enter Jordan Williams.
7. Set the username to:

jwill​​iams

8. Create the password.
9. Complete the user creation.
10. Verify that Jordan Williams exists in the IT OU.

### Alex Johnson

Username:

ajohnson

### Steps

1. Open Active Directory Users and Computers.
2. Open the Help Desk OU.
3. Right-click an empty area.
4. Select New.
5. Select User.
6. Enter Alex Johnson.
7. Set the username to:

ajohnson

8. Create the password.
9. Complete the user creation.
10. Verify that Alex Johnson exists in the Help Desk OU.


==================================================
11. SECURITY GROUPS
==================================================

The following security groups were created:

- IT Support
- Help Desk

### Steps

1. Open Active Directory Users and Computers.
2. Right-click the appropriate location.
3. Select New.
4. Select Group.
5. Enter the group name.
6. Select Global for Group Scope.
7. Select Security for Group Type.
8. Click OK.
9. Repeat for the second group.


==================================================
12. ADD USERS TO SECURITY GROUPS
==================================================

### Jordan Williams

### Steps

1. Open IT Support Properties.
2. Select Members.
3. Click Add.
4. Enter:

jwill​​iams

5. Click Check Names.
6. Click OK.
7. Click Apply.
8. Click OK.

### Alex Johnson

### Steps

1. Open Help Desk Properties.
2. Select Members.
3. Click Add.
4. Enter:

ajohnson

5. Click Check Names.
6. Click OK.
7. Click Apply.
8. Click OK.


==================================================
13. GROUP POLICY
==================================================

GPO Name:

Help Desk Workstation Policy

### Steps

1. Open Server Manager.
2. Select Tools.
3. Open Group Policy Management.
4. Expand Forest.
5. Expand Domains.
6. Expand matthias.local.
7. Right-click the Help Desk OU.
8. Select Create a GPO in this domain, and Link it here.
9. Enter:

Help Desk Workstation Policy

10. Click OK.
11. Right-click the GPO.
12. Select Edit.


==================================================
14. GROUP POLICY CONFIGURATION
==================================================

### Disable Last Signed-In User

### Steps

1. Open the Help Desk Workstation Policy.
2. Navigate to:

Computer Configuration
Policies
Windows Settings
Security Settings
Local Policies
Security Options

3. Find:

Interactive logon: Do not display last signed-in

4. Open the setting.
5. Select Enabled.
6. Click Apply.
7. Click OK.


==================================================
15. PASSWORD POLICY
==================================================

### Steps

1. Open the appropriate Group Policy.
2. Navigate to:

Computer Configuration
Policies
Windows Settings
Security Settings
Account Policies
Password Policy

3. Find:

Minimum password length

4. Open the setting.
5. Set the value to:

10

6. Click Apply.
7. Click OK.


==================================================
16. WINDOWS 11 CLIENT
==================================================

### Steps

1. Open UTM.
2. Create a new virtual machine.
3. Select the Windows 11 ISO.
4. Configure the VM.
5. Allocate approximately 6 GB of RAM.
6. Allocate approximately 4 CPU cores.
7. Create approximately a 64 GB virtual disk.
8. Store the VM on the external SSD.
9. Start the VM.
10. Install Windows 11 Pro.
11. Create the initial local account.
12. Install UTM Guest Tools.
13. Restart the client if necessary.
14. Verify Windows 11 starts normally.


==================================================
17. WINDOWS CLIENT DNS
==================================================

### Steps

1. Open Control Panel.
2. Select Network and Internet.
3. Select Network and Sharing Center.
4. Select Change adapter settings.
5. Right-click Ethernet.
6. Select Properties.
7. Select Internet Protocol Version 4 (TCP/IPv4).
8. Click Properties.
9. Set Preferred DNS Server to:

192.168.64.2

10. Click OK.
11. Close the network settings.


==================================================
18. CLIENT NETWORK TESTING
==================================================

### IP Address

### Steps

1. Open Command Prompt.
2. Run:

ipconfig /all

3. Check the IPv4 address.
4. Check the subnet mask.
5. Check the default gateway.
6. Check the DNS server.

### Ping DC01

1. Run:

ping 192.168.64.2

2. Verify that the client receives replies.

### DNS Test

1. Run:

nslookup matthias.local

2. Verify that the domain resolves to:

192.168.64.2


==================================================
19. DOMAIN CONTROLLER DISCOVERY
==================================================

### Problem

The Windows 11 client initially had difficulty discovering the Domain
Controller.

### Steps

1. Open Command Prompt.
2. Run:

nltest /dsgetdc:matthias.local

3. If the command fails, check the client's DNS settings.
4. Verify DNS is:

192.168.64.2

5. Ping DC01.
6. Test DNS.
7. Check IPv6 configuration.
8. IPv6 was disabled on the client Ethernet adapter in this lab because
   IPv6 DNS was interfering with domain discovery.
9. Run the command again:

nltest /dsgetdc:matthias.local

### Successful Result

The client discovered:

DC01.matthias.local

IP:

192.168.64.2

Domain:

matthias.local


==================================================
20. DOMAIN JOIN
==================================================

### Steps

1. Open Settings.
2. Select System.
3. Select About.
4. Open the advanced system or computer name settings.
5. Select Change.
6. Select Domain.
7. Enter:

matthias.local

8. Click OK.
9. Enter the Domain Administrator credentials.
10. Use:

MATTHIAS\Administrator

11. Enter the Administrator password.
12. Verify the message:

Welcome to the matthias.local domain

13. Click OK.
14. Restart the Windows 11 client.


==================================================
21. DOMAIN LOGIN
==================================================

### Steps

1. Restart Windows 11.
2. Select Other User if necessary.
3. Enter:

ajohnson

4. Enter the password.
5. Sign in.
6. Wait for the Windows desktop to load.


==================================================
22. VERIFY DOMAIN AUTHENTICATION
==================================================

### Steps

1. Open Command Prompt.
2. Run:

whoami

3. Verify that the result shows the domain and username.

4. Run:

echo %logonserver%

5. Verify that the result shows:

\\DC01

6. Verify that the Windows computer is joined to:

matthias.local


==================================================
23. COMPROMISED ACCOUNT SCENARIO
==================================================

### Scenario

A user's Active Directory account may have been compromised.

Test account:

ajohnson

### First Response

1. Disable or secure the account.
2. Reset the password.
3. Review suspicious activity.
4. Check Security Event Viewer.
5. Verify MFA requirements.
6. Document the incident.
7. Escalate when necessary.


==================================================
24. DISABLE COMPROMISED ACCOUNT
==================================================

### Steps

1. Log into DC01.
2. Open Active Directory Users and Computers.
3. Open the Help Desk OU.
4. Locate ajohnson.
5. Right-click the user.
6. Select Disable Account.
7. Confirm the action.
8. Verify the account is disabled.


==================================================
25. RESET COMPROMISED ACCOUNT PASSWORD
==================================================

### Steps

1. Open Active Directory Users and Computers.
2. Open the Help Desk OU.
3. Right-click ajohnson.
4. Select Reset Password.
5. Enter a temporary password.
6. Confirm the password.
7. Select User must change password at next logon when appropriate.
8. Click OK.
9. Document the password reset.


==================================================
26. SECURITY EVENT REVIEW
==================================================

### Steps

1. Open Event Viewer.
2. Expand Windows Logs.
3. Select Security.
4. Review recent events.
5. Look for successful logons.
6. Look for failed logons.
7. Look for account lockouts.
8. Look for account changes.
9. Record suspicious activity.
10. Document the relevant Event IDs.
11. Escalate suspicious activity when necessary.


==================================================
27. IMPORTANT SECURITY EVENT IDS
==================================================

Event ID 4624:

Successful logon.

Event ID 4625:

Failed logon.

Event ID 4740:

User account was locked out.

Event ID 4725:

User account was disabled.

Event ID 4724:

An attempt was made to reset an account password.


==================================================
28. MFA
==================================================

The on-premises Active Directory lab does not provide a complete MFA
solution by itself.

### Steps

1. Verify whether the organization uses MFA.
2. Check the organization's identity provider.
3. Verify the user's MFA enrollment.
4. Document the MFA status.
5. Do not claim MFA was configured if it was not.


==================================================
29. SECURITY INCIDENT DOCUMENTATION
==================================================

### Steps

1. Record the username.
2. Record the time the issue was discovered.
3. Record the suspicious activity.
4. Record whether the account was disabled.
5. Record whether the password was reset.
6. Record whether MFA was checked.
7. Record Security Event Viewer findings.
8. Record actions taken.
9. Record who the incident was escalated to.
10. Record the final resolution.


==================================================
30. LOCKED ACCOUNT SCENARIO
==================================================

### Scenario

A user reports that their Active Directory account is locked.

Test account:

ajohnson


==================================================
31. RE-ENABLE THE TEST ACCOUNT
==================================================

If the account was disabled during the compromised-account scenario,
enable it before testing account lockouts.

### Steps

1. Open Active Directory Users and Computers.
2. Open the Help Desk OU.
3. Locate ajohnson.
4. Right-click the account.
5. Select Enable Account.
6. Confirm the account is enabled.


==================================================
32. ACCOUNT LOCKOUT POLICY
==================================================

### Steps

1. Open Group Policy Management.
2. Open the appropriate domain-level policy.
3. Navigate to:

Computer Configuration
Policies
Windows Settings
Security Settings
Account Policies
Account Lockout Policy

4. Configure Account lockout threshold.
5. Set the lab threshold to:

3 invalid logon attempts

6. Configure Account lockout duration.
7. Set the lab duration to:

15 minutes

8. Configure Reset account lockout counter after.
9. Set the reset period to:

15 minutes

10. Apply the settings.
11. Close the Group Policy editor.


==================================================
33. GENERATE AN ACCOUNT LOCKOUT
==================================================

### Steps

1. Make sure ajohnson is enabled.
2. Go to the Windows 11 client.
3. Attempt to log in as ajohnson.
4. Enter the wrong password.
5. Repeat the incorrect password until the lockout threshold is reached.
6. Verify the account becomes locked.
7. Return to DC01.
8. Open Active Directory Users and Computers.
9. Open the Help Desk OU.
10. Locate ajohnson.
11. Open Properties.
12. Select the Account tab.
13. Verify that the account is locked.


==================================================
34. UNLOCK USER ACCOUNT
==================================================

### Steps

1. Log into DC01.
2. Open Active Directory Users and Computers.
3. Open the Help Desk OU.
4. Right-click ajohnson.
5. Select Properties.
6. Select the Account tab.
7. Clear Account is locked out if it is selected.
8. Click Apply.
9. Click OK.
10. Return to the Windows 11 client.
11. Attempt to log in again.
12. Verify that the user can log in.


==================================================
35. PASSWORD RESET
==================================================

### Steps

1. Open Active Directory Users and Computers.
2. Open the Help Desk OU.
3. Right-click ajohnson.
4. Select Reset Password.
5. Enter a temporary password.
6. Confirm the password.
7. Select User must change password at next logon when appropriate.
8. Click OK.
9. Test the user's login.


==================================================
36. USER LOGIN TROUBLESHOOTING
==================================================

### Steps

1. Confirm the username.
2. Confirm the password.
3. Check whether the account is disabled.
4. Check whether the account is locked.
5. Check whether the password has expired.
6. Check the network connection.
7. Check DNS.
8. Check communication with DC01.
9. Check Event Viewer.
10. Reset the password if necessary.
11. Unlock the account if necessary.
12. Test the login.
13. Document the resolution.


==================================================
37. INTERNET TROUBLESHOOTING
==================================================

### Scenario

A user reports that their computer has no internet connection.

### Steps

1. Check whether Ethernet or Wi-Fi is connected.
2. Run:

ipconfig

3. Check the IP address.
4. Check the subnet mask.
5. Check the default gateway.
6. Check the DNS server.
7. Ping the gateway.
8. Run:

ping 8.8.8.8

9. Test DNS:

nslookup google.com

10. Determine whether the issue is network connectivity, routing, or DNS.
11. Apply the appropriate fix.
12. Test internet access again.
13. Document the issue.


==================================================
38. DNS TROUBLESHOOTING
==================================================

### Steps

1. Run:

ipconfig /all

2. Check the DNS server.
3. Run:

nslookup google.com

4. Test the internal domain:

nslookup matthias.local

5. Verify the DNS service on DC01.
6. Open DNS Manager.
7. Verify the matthias.local zone.
8. Check DNS records.
9. Test connectivity again.
10. Document the resolution.


==================================================
39. DOMAIN JOIN TROUBLESHOOTING
==================================================

### Steps

1. Check the client's network connection.
2. Run:

ipconfig /all

3. Verify the DNS server is:

192.168.64.2

4. Ping:

192.168.64.2

5. Run:

nslookup matthias.local

6. Run:

nltest /dsgetdc:matthias.local

7. Verify that DC01 is discovered.
8. Check IPv6 if domain discovery fails.
9. Verify the Domain Controller is online.
10. Attempt the domain join again.
11. Enter valid domain credentials.
12. Restart the client.
13. Test domain authentication.
14. Document the resolution.


==================================================
40. GROUP POLICY TROUBLESHOOTING
==================================================

### Scenario

A Group Policy setting does not appear to be applying.

### Steps

1. Log into the Windows client.
2. Open Command Prompt.
3. Run:

gpupdate /force

4. Wait for the update to finish.
5. Run:

gpresult /r

6. Review the applied policies.
7. Verify Help Desk Workstation Policy is listed.
8. Verify the computer or user is in the correct OU.
9. Open Group Policy Management.
10. Verify the GPO is linked to the correct OU.
11. Verify the GPO is enabled.
12. Verify the policy setting is configured correctly.
13. Restart the client if necessary.
14. Run gpupdate /force again.
15. Test the policy.
16. Document the resolution.


==================================================
41. ACTIVE DIRECTORY USER TROUBLESHOOTING
==================================================

### Steps

1. Open Active Directory Users and Computers.
2. Locate the user.
3. Verify the username.
4. Verify the user's OU.
5. Verify the account is enabled.
6. Check whether the account is locked.
7. Check group memberships.
8. Check password settings.
9. Reset the password if necessary.
10. Unlock the account if necessary.
11. Test the user's login.
12. Document the resolution.


==================================================
42. GROUP MEMBERSHIP TROUBLESHOOTING
==================================================

### Steps

1. Open Active Directory Users and Computers.
2. Locate the user.
3. Open Properties.
4. Select Member Of.
5. Review the user's groups.
6. Verify that the user belongs to the correct security group.
7. Add the user to the correct group if necessary.
8. Click Apply.
9. Click OK.
10. Have the user sign out.
11. Have the user sign back in.
12. Test access.
13. Document the change.


==================================================
43. EVENT VIEWER TROUBLESHOOTING
==================================================

### Steps

1. Open Event Viewer.
2. Expand Windows Logs.
3. Select Application.
4. Review application errors.
5. Select System.
6. Review system errors.
7. Select Security.
8. Review authentication and account events.
9. Open suspicious events.
10. Record the Event ID.
11. Record the time.
12. Record the affected user or computer.
13. Investigate the cause.
14. Apply the appropriate fix.
15. Document the result.


==================================================
44. USEFUL COMMANDS
==================================================

### Check IP Configuration

ipconfig

### Detailed IP Configuration

ipconfig /all

### Test DC01 Connectivity

ping 192.168.64.2

### Test Internet Connectivity

ping 8.8.8.8

### Test DNS

nslookup matthias.local

### Test Domain Controller Discovery

nltest /dsgetdc:matthias.local

### Force Group Policy Update

gpupdate /force

### Check Applied Group Policies

gpresult /r

### Check Logged-In User

whoami

### Check Logon Server

echo %logonserver%

### Check Computer Name

hostname


==================================================
45. USEFUL POWERSHELL COMMANDS
==================================================

### Unlock a User

Unlock-ADAccount -Identity ajohnson

### Disable a User

Disable-ADAccount -Identity ajohnson

### Enable a User

Enable-ADAccount -Identity ajohnson

### Check a User

Get-ADUser ajohnson

### Check All User Properties

Get-ADUser ajohnson -Properties *

### Reset a User Password

Set-ADAccountPassword -Identity ajohnson -Reset -NewPassword (Read-Host -AsSecureString "New password")


==================================================
46. GENERAL HELP DESK TROUBLESHOOTING
==================================================

### Steps

1. Identify the problem.
2. Ask the user questions.
3. Determine when the problem started.
4. Determine whether anything recently changed.
5. Reproduce the issue.
6. Check the simplest possible cause.
7. Check hardware.
8. Check software.
9. Check network connectivity.
10. Check DNS.
11. Check user permissions.
12. Check Active Directory.
13. Check Group Policy.
14. Check Event Viewer.
15. Make one change at a time.
16. Test the result.
17. Confirm the issue is resolved.
18. Document everything.
19. Escalate if necessary.


==================================================
47. HELP DESK TICKET DOCUMENTATION
==================================================

### User

Record the affected user's name.

### Problem

Record a short description of the issue.

### Troubleshooting

Record the steps used to identify the cause.

### Resolution

Record the action taken to fix the issue.

### Verification

Record how the fix was tested.

### Escalation

Record who the issue was escalated to if necessary.


==================================================
48. EXAMPLE HELP DESK TICKET
==================================================

User:

Alex Johnson

Problem:

Unable to log into Windows.

Troubleshooting:

Checked the Active Directory account and discovered that the account
was locked.

Resolution:

Unlocked the account and reset the password.

Verification:

User successfully logged into the Windows 11 client.


==================================================
49. WHAT I LEARNED
==================================================

This project gave me hands-on experience with:

- Windows Server 2025
- Windows 11
- Active Directory
- Active Directory Domain Services
- Domain Controllers
- DNS
- Group Policy
- Organizational Units
- Security Groups
- User Account Management
- Password Management
- Account Lockout Management
- Domain Joining
- Domain Authentication
- Event Viewer
- Security Logs
- Command Prompt
- PowerShell
- Network Troubleshooting
- DNS Troubleshooting
- Help Desk Troubleshooting
- Security Incident Response
- Incident Documentation


==================================================
50. FUTURE LABS
==================================================

Future scenarios I plan to add:

- New employee onboarding
- Employee offboarding
- Password reset
- Account lockout
- Shared folder permissions
- NTFS permissions
- File server
- Network drive mapping
- Printer troubleshooting
- Remote Desktop
- Software installation
- Software troubleshooting
- DHCP
- Advanced DNS
- Advanced Group Policy
- BitLocker
- Windows security policies
- SIEM log analysis
- Security monitoring
- Endpoint security
- PowerShell automation
- Backup and recovery
- Disaster recovery
- Additional Active Directory troubleshooting


==================================================
51. SCREENSHOTS
==================================================

Screenshots should be stored in the screenshots folder.

Recommended screenshots:

1. Windows Server 2025 desktop
2. DC01 Server Manager
3. DC01 static IP configuration
4. Active Directory Users and Computers
5. DNS Manager
6. IT OU
7. Help Desk OU
8. Jordan Williams user
9. Alex Johnson user
10. IT Support group
11. Help Desk group
12. Group Policy Management
13. Help Desk Workstation Policy
14. Windows 11 client
15. ipconfig results
16. nslookup results
17. nltest results
18. Domain join confirmation
19. Domain login
20. Event Viewer Security logs
21. Account lockout scenario
22. Password reset scenario
23. Help Desk troubleshooting scenario


==================================================
52. SCREENSHOT NAMING
==================================================

Use simple names so the repository stays organized.

Example:

screenshots/01-server-manager.png

screenshots/02-dc01-static-ip.png

screenshots/03-active-directory.png

screenshots/04-dns-manager.png

screenshots/05-organizational-units.png

screenshots/06-users.png

screenshots/07-security-groups.png

screenshots/08-group-policy.png

screenshots/09-windows-client.png

screenshots/10-client-ipconfig.png

screenshots/11-domain-join.png

screenshots/12-domain-login.png

screenshots/13-nltest.png

screenshots/14-event-viewer.png

screenshots/15-help-desk-scenario.png


==================================================
53. GITHUB PROJECT GOAL
==================================================

The purpose of this repository is to demonstrate practical IT Support
skills through a hands-on Active Directory environment.

Instead of only listing Active Directory on my resume, this project
documents how I built the environment, configured it, troubleshot
problems, and practiced real Help Desk scenarios.


==================================================
54. RESUME PROJECT DESCRIPTION
==================================================

Active Directory Home Lab | Windows Server 2025

Built and managed a virtual Active Directory environment using Windows
Server 2025 and Windows 11. Configured AD DS, DNS, Organizational Units,
users, security groups, Group Policy, domain authentication, and network
connectivity. Troubleshot DNS, domain discovery, domain joining, account
lockouts, password issues, Group Policy, and security events using
Event Viewer, Command Prompt, and PowerShell.


==================================================
55. LINKEDIN PROJECT DESCRIPTION
==================================================

Built a Windows Server 2025 Active Directory home lab using UTM.

Hands-on work included:

- Active Directory Domain Services
- DNS
- Organizational Units
- User and group management
- Group Policy
- Windows 11 domain joining
- Domain authentication
- Network troubleshooting
- Account lockouts
- Password resets
- Security Event Viewer
- Help Desk troubleshooting scenarios

I am using this lab to build practical IT Support and Help Desk skills
through hands-on troubleshooting rather than relying only on theory.


==================================================
56. FINAL PROJECT SUMMARY
==================================================

This Active Directory home lab demonstrates practical experience with
Windows Server administration, Active Directory, DNS, Group Policy,
Windows client management, account administration, networking, security
troubleshooting, and Help Desk workflows.

The environment was built from scratch using virtual machines.

The project will continue to grow as additional IT Support and
cybersecurity scenarios are added.


==================================================
GITHUB SETUP
==================================================

### Step 1 — Create the Repository

1. Sign into GitHub.
2. Create a new repository.
3. Name the repository:

active-directory-home-lab

4. Add the description:

Windows Server 2025 Active Directory home lab demonstrating AD DS, DNS,
Group Policy, Windows 11 domain joining, user administration, networking,
security troubleshooting, and Help Desk scenarios.

5. Set the repository to Public if you want recruiters to view it.
6. Create the repository.


==================================================
GITHUB FILES TO CREATE
==================================================

Create these folders:

screenshots

documentation

labs

Create these files:

README.md

documentation/network-configuration.md

documentation/active-directory-configuration.md

documentation/group-policy.md

documentation/dns-troubleshooting.md

documentation/domain-join-troubleshooting.md

documentation/help-desk-scenarios.md

labs/compromised-account.md

labs/locked-account.md

labs/no-internet.md

labs/dns-failure.md

labs/domain-join-failure.md

labs/group-policy-failure.md


==================================================
IMPORTANT
==================================================

Do not upload passwords, private keys, personal information, real
company information, real employee information, or sensitive credentials
to GitHub.

Use only your lab accounts and fictional test data.

The accounts used in this lab are test accounts created specifically
for the Active Directory environment.


==================================================
FINAL GITHUB PORTFOLIO
==================================================

The finished repository should look like this:

active-directory-home-lab/

├── README.md
│
├── screenshots/
│   ├── 01-server-manager.png
│   ├── 02-dc01-static-ip.png
│   ├── 03-active-directory.png
│   ├── 04-dns-manager.png
│   ├── 05-organizational-units.png
│   ├── 06-users.png
│   ├── 07-security-groups.png
│   ├── 08-group-policy.png
│   ├── 09-windows-client.png
│   ├── 10-client-ipconfig.png
│   ├── 11-domain-join.png
│   ├── 12-domain-login.png
│   ├── 13-nltest.png
│   ├── 14-event-viewer.png
│   └── 15-help-desk-scenario.png
│
├── documentation/
│   ├── network-configuration.md
│   ├── active-directory-configuration.md
│   ├── group-policy.md
│   ├── dns-troubleshooting.md
│   ├── domain-join-troubleshooting.md
│   └── help-desk-scenarios.md
│
└── labs/
    ├── compromised-account.md
    ├── locked-account.md
    ├── no-internet.md
    ├── dns-failure.md
    ├── domain-join-failure.md
    └── group-policy-failure.md


PROJECT STATUS
==============

Completed:

- Windows Server 2025 installed
- DC01 configured
- Static IP configured
- Active Directory installed
- matthias.local domain created
- DNS configured
- IT OU created
- Help Desk OU created
- Jordan Williams created
- Alex Johnson created
- IT Support group created
- Help Desk group created
- Group Policy created
- Windows 11 client created
- Windows 11 client joined to domain
- Domain authentication tested
- DNS troubleshooting completed
- Domain discovery troubleshooting completed
- Compromised account scenario completed
- Account management practiced
- Security Event Viewer practiced

Next:

- Complete additional Help Desk scenarios
- Add screenshots
- Add documentation
- Add more Group Policy labs
- Add file and folder permission labs
- Add DHCP labs
- Add PowerShell automation
- Add cybersecurity/SIEM labs