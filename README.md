# savn.local — A Multi-VM Active Directory Domain Lab

**Built with Oracle VirtualBox | Windows Server 2025 Standard | Windows 11**

A self-directed home lab project completed for a university IT course, simulating a small enterprise network: a Windows Server domain controller, a joined member server, and a domain-joined Windows 11 client — all running as VMs on Oracle VirtualBox.

---

## Overview

This lab walks through standing up an Active Directory domain from nothing: installing server roles, promoting a domain controller, joining machines to the domain, configuring networking, creating organizational units and user accounts, and verifying everything actually works — including what happens when the domain controller isn't reachable.

**Objectives:**

- Install and configure Active Directory Domain Services (AD DS) and DNS on Windows Server  
- Join additional machines (server \+ client) to the domain  
- Configure static IP addressing and verify network connectivity between all VMs  
- Create and organize Active Directory objects (OUs, local users, domain users)  
- Test and compare local account vs. domain account authentication  
- Observe domain login behavior when the domain controller is unavailable

---

## Lab Topology

| Machine | Hostname | Role | OS | IP Address |
| :---- | :---- | :---- | :---- | :---- |
| DC1 | `FA-SU26-S25-S1` | Domain Controller (AD DS \+ DNS) | Windows Server 2025 Standard | `192.168.0.101` |
| S2 | `AF-SU26-S25-S2` | Domain Member Server | Windows Server 2025 Standard | `192.168.0.102` |
| C1 | `AF-SU26-W11-C1` | Domain-Joined Client | Windows 11 Education N (25H2) | `192.168.0.103` |

**Domain:** `savn.local`

                    ┌───────────────────────┐

                    │   DC1 (S25-S1)        │

                    │   AD DS \+ DNS         │

                    │   192.168.0.101       │

                    └───────────┬───────────┘

                                │

              ┌─────────────────┴─────────────────┐

              │                                    │

   ┌──────────┴──────────┐              ┌──────────┴──────────┐

   │   S2 (S25-S2)        │              │   C1 (W11-C1)        │

   │   Member Server       │              │   Domain Client       │

   │   192.168.0.102       │              │   192.168.0.103       │

   └───────────────────────┘              └────────────────────────┘

---

## Tools & Technologies

- Oracle VirtualBox (virtualization platform)  
- Windows Server 2025 Standard  
- Windows 11 Education N (25H2)  
- Active Directory Domain Services (AD DS)  
- DNS Server role  
- Active Directory Users and Computers (ADUC)  
- Windows Command Prompt (`ipconfig`, `ping`)

---

## Walkthrough

### 1\. Deploying the Domain Controller

Installed the AD DS and DNS server roles on `FA-SU26-S25-S1`, then promoted it to a domain controller, establishing a new forest and domain: `savn.local`.

`!FA-SU26-S25-S1 [Running] - Oracle VirtualBox 7_7_2026 8_28_30 PM.png` `![S1 in savn.local domain](screenshots/02-s1-in-savn-local-domain.png)` `![S1 set as domain controller](screenshots/03-s1-domain-controller.png)`

### 2\. Joining Machines to the Domain

Joined the second server (`AF-SU26-S25-S2`) and the Windows 11 client (`AF-SU26-W11-C1`) to `savn.local`. Verified both machines appear as computer objects in Active Directory Users and Computers.

`![S2 joins savn.local domain](screenshots/04-s2-joins-domain.png)` `![W11 joins savn.local domain](screenshots/05-w11-joins-domain.png)` `![Computers in savn.local](screenshots/06-computers-in-savn-local.png)`

### 3\. Network Configuration & Connectivity Verification

Configured static IP addressing on all three VMs, with each machine pointed to the domain controller (`192.168.0.101`) for DNS resolution. Confirmed configuration with `ipconfig /all` on each machine, then verified full connectivity by pinging between all three VMs in both directions.

`![DC1 ipconfig /all output](screenshots/07-dc1-ipconfig.png)` `![S2 ipconfig /all output](screenshots/08-s2-ipconfig.png)` `![C1 ipconfig /all output](screenshots/09-c1-ipconfig.png)` `![DC1 successful ping to other VMs](screenshots/10-dc1-ping-success.png)` `![S2 successful ping to other VMs](screenshots/11-s2-ping-success.png)` `![C1 successful ping to other VMs](screenshots/12-c1-ping-success.png)`

### 4\. Organizational Units & User Accounts

Created new Organizational Units within `savn.local` to reflect a basic departmental structure (Administration, Research, Sales). Created both a **local** user account (on S2) and a **domain** user account (`domainUser01`), and placed the domain user into the Administration OU.

`![New Organizational Units](screenshots/13-new-ous.png)` `![Local user account created](screenshots/14-local-user-created.png)` `![New domain user account created](screenshots/15-domain-user-created.png)` `![domainUser01 in Administration OU](screenshots/16-domainuser-in-ou.png)`

### 5\. Authentication Testing: Local vs. Domain

Logged in as the local account (`localUser01`) to confirm it authenticates independently of the domain. Then logged in as `domainUser01` on the Windows 11 client to confirm successful domain authentication against `savn.local`.

`![localUser01 logged in locally](screenshots/17-local-login.png)` `![Domain user logged into savn.local](screenshots/18-domain-login.png)`

### 6\. Failure Scenario: Domain Controller Unavailable

To understand the dependency between domain-joined machines and the DC, I took the domain controller offline and attempted a domain login from the client. Windows correctly surfaced an error indicating the domain wasn't reachable — a good demonstration of why DC availability (and things like cached credentials) matter in a real environment.

`![Attempted domain login when domain server is unavailable](screenshots/19-domain-unavailable.png)`

---

## Key Skills Demonstrated

- Active Directory Domain Services deployment and domain promotion  
- DNS server configuration  
- Domain joining for both servers and clients  
- Static IP configuration and network troubleshooting fundamentals  
- Organizational Unit design and Active Directory object management  
- User account provisioning (local vs. domain-level)  
- Diagnosing and interpreting authentication failure behavior

---

*Completed as part of university IT coursework, \[Summer/2026\]. All VMs run locally in Oracle VirtualBox.*  
