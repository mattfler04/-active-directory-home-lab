# Windows Server 2025 Active Directory Home Lab

## Project Overview

This project documents my hands-on Active Directory home lab built using
UTM on a Mac mini.

The goal was to build a small business-style Windows environment and
practice real-world IT support, help desk, system administration, Active
Directory, DNS, Group Policy, user management, authentication,
networking, and security troubleshooting.

## Lab Environment

### Host

-   **Hardware:** Mac mini
-   **Memory:** 24 GB RAM
-   **Virtualization:** UTM
-   **VM storage:** External SSD

### Domain Controller

-   **Operating system:** Windows Server 2025
-   **Hostname:** `DC01`
-   **Domain:** `matthias.local`
-   **IP address:** `192.168.64.2`
-   **Subnet mask:** `255.255.255.0`
-   **Default gateway:** `192.168.64.1`
-   **Preferred DNS:** `192.168.64.2`

### Windows Client

-   **Operating system:** Windows 11 Pro
-   **Domain:** `matthias.local`
-   **DNS server:** `192.168.64.2`

------------------------------------------------------------------------

## 1. Windows Server 2025 Installation

I created a Windows Server 2025 virtual machine using UTM and completed
the initial Windows configuration.

I renamed the server to `DC01` and restarted it to apply the hostname
change.

## 2. Configuring a Static IP Address

I configured the Domain Controller with a static IPv4 address:

  Setting           Value
  ----------------- -----------------
  IP address        `192.168.64.2`
  Subnet mask       `255.255.255.0`
  Default gateway   `192.168.64.1`
  Preferred DNS     `192.168.64.2`

The Domain Controller uses itself as its DNS server because Active
Directory relies on DNS for domain and service discovery.

## 3. Installing Active Directory Domain Services

In **Server Manager**, I selected **Add Roles and Features** and
installed **Active Directory Domain Services (AD DS)**.

After installation, I promoted the server to a Domain Controller and
selected **Add a new forest**.

-   **Domain:** `matthias.local`
-   **DNS Server:** Enabled
-   **Global Catalog:** Enabled
-   **Read-Only Domain Controller (RODC):** Disabled

I created a Directory Services Restore Mode (DSRM) password and kept the
default locations for the Active Directory database, log files, and
SYSVOL. The server restarted after promotion.

## 4. Verifying Active Directory

After the restart, I opened **Active Directory Users and Computers** and
verified that:

-   The domain `matthias.local` existed.
-   The Domain Controller hostname was `DC01`.

This confirmed that Active Directory Domain Services had been installed
and the domain had been created.

## 5. Configuring and Testing DNS

I opened DNS Manager and verified that the `matthias.local` DNS zone
existed.

I tested domain name resolution:

``` cmd
nslookup matthias.local
```

The domain resolved to `192.168.64.2`.

I also tested the Active Directory LDAP service record:

``` cmd
nslookup -type=SRV _ldap._tcp.dc._msdcs.matthias.local 192.168.64.2
```

The result identified `DC01` and its address, `192.168.64.2`. This
confirmed that the Domain Controller was advertising the required Active
Directory service record.

## 6. DNS Troubleshooting

During testing, `nslookup` displayed:

``` text
Server: Unknown
```

However, `matthias.local` resolved successfully to `192.168.64.2`, and
the Active Directory SRV record lookup worked.

The `Unknown` label was related to reverse DNS/name resolution for the
DNS server address. The successful forward lookup and SRV lookup
indicated that the core DNS functions needed for this lab were working.

## 7. Creating Organizational Units

In **Active Directory Users and Computers**, I created two
Organizational Units (OUs):

-   `IT`
-   `Help Desk`

The structure was:

``` text
matthias.local
├── IT
└── Help Desk
```

OUs help organize users and can be used to scope Group Policy settings.

## 8. Creating Test Users

I created two test accounts to simulate employees in a business
environment.

  Name              Username      OU
  ----------------- ------------- -----------
  Jordan Williams   `jwilliams`   IT
  Alex Johnson      `ajohnson`    Help Desk

## 9. Creating Security Groups

I created two **Global Security** groups:

-   **IT Support:** Added Jordan Williams (`jwilliams`).
-   **Help Desk:** Added Alex Johnson (`ajohnson`).

Security groups make it easier to manage permissions and assign access
based on users' roles.

## 10. Creating Group Policy

I created a Group Policy Object (GPO) named **Help Desk Workstation
Policy** and linked it to the **Help Desk** OU.

I configured the following setting:

**Computer Configuration → Policies → Windows Settings → Security
Settings → Local Policies → Security Options**

-   **Interactive logon: Do not display last signed-in:** Enabled

I also practiced configuring password policy settings, including a
minimum password length of 10 characters.

## 11. Creating the Windows 11 Client

I created a Windows 11 Pro virtual machine in UTM and placed it on the
same virtual network as the Domain Controller.

I configured the client to use `192.168.64.2` as its DNS server so it
could locate the Active Directory domain.

## 12. Testing Client Network Connectivity

Before joining the domain, I opened Command Prompt on the Windows 11
client and ran:

``` cmd
ping 192.168.64.2
```

The client received replies from the Domain Controller, confirming basic
network connectivity.

## 13. Checking the Client IP Configuration

I ran:

``` cmd
ipconfig
```

I checked the client's IPv4 address, subnet mask, default gateway, and
DNS server.

For this lab, the key setting was the DNS server: `192.168.64.2`.

## 14. Troubleshooting Domain Discovery

The client initially had trouble locating the Active Directory domain. I
tested domain discovery with:

``` cmd
nltest /dsgetdc:matthias.local
```

The client eventually returned the following Domain Controller
information:

-   **Domain Controller:** `DC01.matthias.local`
-   **Address:** `192.168.64.2`
-   **Domain:** `matthias.local`

This confirmed that the client could locate the Domain Controller.

## 15. IPv6 and DNS Troubleshooting

During troubleshooting, the client was using an IPv6 DNS configuration
that interfered with domain discovery in this lab setup.

I opened the Ethernet adapter properties through:

**Control Panel → Network and Internet → Network and Sharing Center →
Change adapter settings**

I reviewed the adapter's IPv4 DNS settings and set the preferred DNS
server to `192.168.64.2`. As a troubleshooting step in this lab, I also
disabled Internet Protocol Version 6 (TCP/IPv6) on the adapter.

I then ran:

``` cmd
nltest /dsgetdc:matthias.local
```

The client successfully found `DC01.matthias.local`.

**Note:** Disabling IPv6 is not a general recommendation for Windows or
Active Directory environments. In production, investigate the IPv4/IPv6
configuration and DNS settings before disabling a protocol.

## 16. Joining Windows 11 to the Domain

After DNS and domain discovery were working, I joined the Windows 11
client to `matthias.local`.

When prompted for domain credentials, I used the domain administrator
account:

``` text
MATTHIAS\Administrator
```

After Windows displayed **Welcome to the matthias.local domain**, I
restarted the client.

## 17. Troubleshooting Domain Login

After restarting, I signed in to the Windows 11 client using the
`ajohnson` domain account.

I ran:

``` cmd
whoami
```

Expected result:

``` text
matthias\ajohnson
```

I also ran:

``` cmd
echo %logonserver%
```

Expected result:

``` text
\\DC01
```

These results confirmed that the user session was using the domain
account and that `DC01` was the reported logon server.

## 18. Verifying Active Directory Authentication

The authentication flow in this lab was:

``` text
Windows 11 Client
       ↓
      DNS
       ↓
      DC01
       ↓
matthias.local
       ↓
 Active Directory
       ↓
    ajohnson
```

The successful domain login confirmed that the client could authenticate
with the Active Directory domain.

------------------------------------------------------------------------

## Help Desk and Security Scenarios

## 19. Security Scenario: Suspected Compromised Account

### Scenario

A user's Active Directory account is suspected of being compromised. The
priority is to contain the risk, investigate available evidence, and
document the response.

### Step 1: Disable the Account

On `DC01`, I opened **Active Directory Users and Computers**, navigated
to the **Help Desk** OU, located `ajohnson`, right-clicked the account,
and selected **Disable Account**.

Disabling the account prevents new sign-ins using that account while the
issue is investigated.

### Step 2: Reset the Password

I selected **Reset Password** for `ajohnson` and created a new temporary
password. I selected **User must change password at next logon** where
appropriate.

In a real incident, the reset should follow the organization's
incident-response procedure and account recovery policy.

### Step 3: Review Suspicious Activity

I opened **Event Viewer → Windows Logs → Security** and reviewed
relevant events.

  Event ID   Meaning
  ---------- -----------------------------------------
  `4624`     Successful logon
  `4625`     Failed logon
  `4724`     An attempt to reset an account password
  `4725`     User account disabled
  `4740`     User account locked out

These events can help investigate authentication activity. Event details
and surrounding activity should be reviewed before drawing conclusions.

### Step 4: Consider MFA

This on-premises Active Directory lab does not provide multifactor
authentication (MFA) by itself. In a real organization, MFA may be
implemented through an identity provider such as Microsoft Entra ID,
depending on the organization's architecture and licensing.

For this lab, I documented MFA as a security consideration rather than
claiming it was configured.

### Step 5: Document and Escalate

I documented the simulated incident:

-   **User:** Alex Johnson
-   **Username:** `ajohnson`
-   **Issue:** Suspected account compromise
-   **Actions:** Account disabled, password reset, security logs
    reviewed, MFA requirements considered, incident documented, and
    incident escalated

## 20. Help Desk Scenario: Locked User Account

### Scenario

A user reports: "I can't log into my computer."

My troubleshooting process is to locate the account, check its status,
unlock it if necessary, reset the password if appropriate, retest the
login, and document the result.

## 21. Checking Whether an Account Is Locked

On `DC01`, I opened **Active Directory Users and Computers**, navigated
to the **Help Desk** OU, located `ajohnson`, and opened **Properties →
Account**.

I checked the account status and whether the account was locked.
Depending on the tools and interface available, an **Unlock Account**
option may also be shown.

## 22. Unlocking an Account

If the account is locked, I use the available unlock option in the
user's account properties or the **Unlock Account** action.

After unlocking the account, I test the user's login again. If the
account immediately locks again, I would investigate the cause, such as
a saved old password on another device or repeated failed sign-in
attempts.

## 23. Resetting a Locked User's Password

If unlocking the account does not resolve the problem, I consider
resetting the password:

1.  Right-click `ajohnson`.
2.  Select **Reset Password**.
3.  Set a temporary password according to the lab's password
    requirements.
4.  Select **User must change password at next logon** when appropriate.
5.  Test the login again.

## 24. Configuring an Account Lockout Policy

To create a realistic account-lockout scenario, I can configure an
account lockout policy.

Example lab settings:

  Policy                                Example value
  ------------------------------------- --------------------------
  Account lockout threshold             3 invalid logon attempts
  Account lockout duration              15 minutes
  Reset account lockout counter after   15 minutes

The purpose of an account lockout policy is to limit repeated
password-guessing attempts. Production values should be chosen according
to organizational security requirements and the risk of
denial-of-service through intentional lockouts.

## 25. Testing Account Lockout

After configuring the policy, I can enter an incorrect password multiple
times on the Windows 11 client. Once the threshold is reached, the
account should lock according to the configured policy.

I then return to **Active Directory Users and Computers**, locate
`ajohnson`, check the account status, and unlock the account.

## 26. Verifying the Account After Unlocking

I return to the Windows 11 client and sign in with `ajohnson`. Then I
run:

``` cmd
whoami
```

Expected result:

``` text
matthias\ajohnson
```

I also run:

``` cmd
echo %logonserver%
```

Expected result:

``` text
\\DC01
```

These checks help confirm that the user can authenticate to the domain
again.

## 27. Troubleshooting a User Who Cannot Log In

When a user cannot sign in, I troubleshoot in this order:

1.  Verify the username.
2.  Verify the password and keyboard layout.
3.  Check whether the account is disabled.
4.  Check whether the account is locked.
5.  Check whether the password has expired.
6.  Check whether the user must change the password.
7.  Check the client's network connection.
8.  Confirm that the client can reach `DC01`.
9.  Check DNS configuration and resolution.
10. Verify that the computer is still joined to the domain.
11. Review relevant Event Viewer logs.
12. Reset the password if necessary and authorized.
13. Unlock or enable the account if appropriate and authorized.
14. Test the login again.
15. Document the resolution.

## 28. Troubleshooting No Internet Access

I practiced a basic Help Desk network troubleshooting workflow.

### Step 1: Check the Connection

Verify that Ethernet or Wi-Fi is connected.

### Step 2: Check IP Configuration

Run:

``` cmd
ipconfig
```

Check the IPv4 address. An address beginning with `169.254.x.x` can
indicate that the computer did not receive an address from DHCP.

### Step 3: Test the Gateway

``` cmd
ping 192.168.64.1
```

If this fails, investigate the local network and gateway configuration.
Note that some devices or networks block ping, so a failed ping alone is
not conclusive.

### Step 4: Test the Domain Controller

``` cmd
ping 192.168.64.2
```

If this fails, investigate connectivity between the client and `DC01`.

### Step 5: Test Internet Connectivity

``` cmd
ping 8.8.8.8
```

If this works but websites do not load, DNS may be a cause. Some
destinations block ICMP, so a failed ping alone does not prove Internet
access is unavailable.

### Step 6: Test DNS

``` cmd
nslookup google.com
```

If name resolution fails, investigate the DNS configuration and DNS
service.

## 29. DNS Troubleshooting Checklist

For Active Directory problems, DNS is one of the first things I check.

I run:

``` cmd
ipconfig /all
```

I verify that the client uses `192.168.64.2` as its DNS server, then
run:

``` cmd
nslookup matthias.local
```

I also test the Active Directory SRV record:

``` cmd
nslookup -type=SRV _ldap._tcp.dc._msdcs.matthias.local 192.168.64.2
```

If the expected records cannot be found, I investigate:

-   DNS service status
-   DNS zone and records
-   Domain Controller status
-   Network connectivity
-   Client DNS configuration

## 30. Domain Join Troubleshooting

If a Windows client cannot join the domain, I check:

1.  Is the client connected to the correct network?
2.  Can the client reach `DC01`?
3.  Is the client using `DC01` as its DNS server?
4.  Does `nslookup` resolve `matthias.local`?
5.  Can `nltest` locate the Domain Controller?
6.  Is `DC01` running?
7.  Are Active Directory Domain Services and DNS running?
8.  Is the domain name correct?
9.  Are the supplied domain credentials correct?
10. Is the client already joined to another domain?
11. Is the Windows edition capable of joining an Active Directory
    domain?

Commands used:

``` cmd
ipconfig /all
ping 192.168.64.2
nslookup matthias.local
nltest /dsgetdc:matthias.local
```

## 31. Group Policy Troubleshooting

If Group Policy is not applying, I check:

1.  Is the user in the correct OU?
2.  Is the computer in the correct OU?
3.  Is the GPO linked to the correct OU?
4.  Is the GPO link enabled, and is the GPO itself enabled?
5.  Is the setting under User Configuration or Computer Configuration?
6.  Is the client connected to the domain?
7.  Can the client reach `DC01`?

I can force a Group Policy update with:

``` cmd
gpupdate /force
```

I can review the applied policies with:

``` cmd
gpresult /r
```

These commands help identify which Group Policy settings are applying to
the user and computer.

## 32. Active Directory User Troubleshooting

When troubleshooting a user account, I check:

-   Username and password
-   Account status
-   Whether the account is disabled or locked
-   Password expiration and password-change requirements
-   Group membership
-   OU placement
-   Group Policy
-   Network connectivity
-   DNS
-   Domain Controller availability
-   Security event logs

I document each troubleshooting step and its result.

## 33. Event Viewer Troubleshooting

I used **Event Viewer → Windows Logs → Security** to review
authentication-related events.

  Event ID   Meaning
  ---------- -----------------------------------------
  `4624`     Successful logon
  `4625`     Failed logon
  `4724`     An attempt to reset an account password
  `4725`     User account disabled
  `4740`     User account locked out

These events can help investigate authentication issues and suspicious
activity. I review event details and surrounding context instead of
relying on an event ID alone.

## 34. PowerShell Active Directory Practice

I practiced PowerShell commands for common Active Directory
administration tasks.

Check a user:

``` powershell
Get-ADUser ajohnson
```

Unlock an account:

``` powershell
Unlock-ADAccount -Identity ajohnson
```

Disable an account:

``` powershell
Disable-ADAccount -Identity ajohnson
```

Enable an account:

``` powershell
Enable-ADAccount -Identity ajohnson
```

Check group membership:

``` powershell
Get-ADGroupMember "Help Desk"
```

These commands can help automate repetitive Active Directory
administration tasks. The Active Directory PowerShell module and
appropriate permissions are required.

## 35. General Help Desk Troubleshooting Process

My general workflow for an Active Directory issue is:

1.  Identify the user's problem.
2.  Ask what changed.
3.  Reproduce the problem if possible.
4.  Check the user's account.
5.  Check whether the account is enabled.
6.  Check whether the account is locked.
7.  Check the password and expiration status.
8.  Check group membership.
9.  Check OU placement.
10. Check Group Policy.
11. Check the client's IP address.
12. Check DNS.
13. Test connectivity to the Domain Controller.
14. Test domain discovery.
15. Review Event Viewer.
16. Make the appropriate change.
17. Test the solution.
18. Confirm the issue is resolved.
19. Document the ticket.
20. Escalate if necessary.

------------------------------------------------------------------------

## 36. What I Learned

This project gave me hands-on practice with:

-   Windows Server 2025
-   Active Directory Domain Services
-   Domain Controllers
-   DNS
-   Windows 11
-   Domain joining
-   User management
-   Security groups
-   Organizational Units
-   Group Policy
-   Password policies
-   Account lockout
-   Account security
-   Event Viewer and Windows Security logs
-   PowerShell
-   Network troubleshooting
-   Domain authentication
-   Help Desk troubleshooting
-   Security incident response

One of the biggest lessons was how closely connected IT services are.
DNS problems can prevent domain joining. Network problems can prevent
authentication. Active Directory account issues can prevent users from
signing in. Group Policy problems can affect workstation behavior. Event
Viewer can provide evidence that helps narrow down the cause.

Rather than only studying these concepts for certifications, I built and
troubleshot my own environment to practice them hands-on.

## 37. Future Lab Plans

I plan to expand the lab with:

-   Additional Windows clients
-   More users and departments
-   A file server
-   Shared folders and NTFS permissions
-   Printer management
-   DHCP
-   Advanced DNS troubleshooting
-   PowerShell automation
-   Additional security policies
-   SIEM integration
-   Microsoft Entra ID
-   MFA
-   Backup and recovery
-   Additional Help Desk ticket scenarios

## Conclusion

This Active Directory home lab is an ongoing project designed to build
practical IT support and system administration experience.

I am using the lab to practice building, troubleshooting, documenting,
and securing a Windows domain environment. The goal is to turn the
concepts I have studied into hands-on troubleshooting experience that
can be applied to real-world Help Desk and IT support environments.
