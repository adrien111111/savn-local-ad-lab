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

<img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 7_7_2026 8_28_30 PM" src="https://github.com/user-attachments/assets/2a5eb812-c93f-49dc-aadd-02df3be90361" />
<img width="1920" height="1111" alt="image" src="https://github.com/user-attachments/assets/b34c9870-cf5e-4b6c-87de-6ec7c1e6eead" />
<img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 7_7_2026 8_49_23 PM" src="https://github.com/user-attachments/assets/e596d33b-2c3e-4149-ae36-f4d07f679e62" />


### 2\. Joining Machines to the Domain

Joined the second server (`AF-SU26-S25-S2`) and the Windows 11 client (`AF-SU26-W11-C1`) to `savn.local`. Verified both machines appear as computer objects in Active Directory Users and Computers.

<img width="1920" height="1111" alt="AF-SU26-S25-S2  Running  - Oracle VirtualBox 7_7_2026 9_52_52 PM" src="https://github.com/user-attachments/assets/5fafd2b4-6571-436c-a66f-061a74e2b4ed" /> <img width="1920" height="1111" alt="AF-SU26-W110C1 (Snapshot 1)  Running  - Oracle VirtualBox 7_7_2026 10_09_34 PM" src="https://github.com/user-attachments/assets/e6abb171-a189-4c24-afbd-d49f85b1af74" /> <img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 7_7_2026 10_11_48 PM" src="https://github.com/user-attachments/assets/524e80e8-736f-406d-9f24-dc1673e6b2dc" />

### 3\. Network Configuration & Connectivity Verification

Configured static IP addressing on all three VMs, with each machine pointed to the domain controller (`192.168.0.101`) for DNS resolution. Confirmed configuration with `ipconfig /all` on each machine, then verified full connectivity by pinging between all three VMs in both directions.

<img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 6_28_2026 9_07_45 PM" src="https://github.com/user-attachments/assets/aea7f777-bcbc-446f-a9e6-219a857e914f" /> <img width="1920" height="1111" alt="AF-SU26-S25-S2  Running  - Oracle VirtualBox 6_28_2026 9_13_48 PM" src="https://github.com/user-attachments/assets/d4ef5c57-3812-4f31-a252-2caba001503f" /> <img width="1920" height="1111" alt="AF-SU26-W110C1 (Snapshot 1)  Running  - Oracle VirtualBox 6_28_2026 7_10_59 PM" src="https://github.com/user-attachments/assets/3e4c388c-13fa-40dd-9f33-0bd0f72159ca" /> <img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 6_28_2026 9_29_29 PM" src="https://github.com/user-attachments/assets/20434112-818a-4f57-8670-be4e571e4ff9" /> <img width="1920" height="1111" alt="AF-SU26-S25-S2  Running  - Oracle VirtualBox 6_28_2026 9_32_43 PM" src="https://github.com/user-attachments/assets/4e8e8333-01e6-413a-984a-609b9bdf2285" /> <img width="1920" height="1111" alt="AF-SU26-W110C1 (Snapshot 1)  Running  - Oracle VirtualBox 6_28_2026 9_34_43 PM" src="https://github.com/user-attachments/assets/b1297f88-18cf-454c-b7f0-ae3331e02e4f" />

### 4\. Organizational Units & User Accounts

Created new Organizational Units within `savn.local` to reflect a basic departmental structure (Administration, Research, Sales). Created both a **local** user account (on S2) and a **domain** user account (`domainUser01`), and placed the domain user into the Administration OU.

<img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 7_20_2026 8_00_15 PM" src="https://github.com/user-attachments/assets/042db4fb-6ddb-4b27-a2df-1331f443cc26" /> <img width="1920" height="1111" alt="AF-SU26-S25-S2  Running  - Oracle VirtualBox 7_20_2026 8_20_39 PM" src="https://github.com/user-attachments/assets/dbfbac30-7cb4-4d51-8d37-94adb39d2953" /> <img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 7_20_2026 9_18_44 PM" src="https://github.com/user-attachments/assets/8ec018cf-e7d3-4f61-bc96-7a06ec435418" /> <img width="1920" height="1111" alt="FA-SU26-S25-S1  Running  - Oracle VirtualBox 7_20_2026 10_14_11 PM" src="https://github.com/user-attachments/assets/40a96a39-0fce-4ecb-b944-86d309e9df26" />
### 5\. Authentication Testing: Local vs. Domain

Logged in as the local account (`localUser01`) to confirm it authenticates independently of the domain. Then logged in as `domainUser01` on the Windows 11 client to confirm successful domain authentication against `savn.local`.

<img width="1920" height="1111" alt="AF-SU26-S25-S2  Running  - Oracle VirtualBox 7_20_2026 10_24_25 PM" src="https://github.com/user-attachments/assets/d2d9a1a4-1537-4339-a7c8-88066ab844f4" /> <img width="1920" height="1111" alt="AF-SU26-W110C1 (Snapshot 1)  Running  - Oracle VirtualBox 7_20_2026 10_32_52 PM" src="https://github.com/user-attachments/assets/4fb5d051-4ab3-45bf-8aab-b93894fcb5ea" />
### 6\. Failure Scenario: Domain Controller Unavailable

To understand the dependency between domain-joined machines and the DC, I took the domain controller offline and attempted a domain login from the client. Windows correctly surfaced an error indicating the domain wasn't reachable — a good demonstration of why DC availability (and things like cached credentials) matter in a real environment.

<img width="1920" height="1111" alt="AF-SU26-S25-S2  Running  - Oracle VirtualBox 7_20_2026 11_01_07 PM" src="https://github.com/user-attachments/assets/ebadd5ec-9e3f-4769-b619-ac2d88c019b4" />

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
