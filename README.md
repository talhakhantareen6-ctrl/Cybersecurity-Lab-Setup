# Cybersecurity Lab Environment Setup

Building an isolated virtual lab for ethical hacking practice using VirtualBox and Kali Linux.

**Network:** 10.0.0.0/24 | **Kali Linux:** 2026.3 | **Course:** NetworkWalks Cybersecurity (WK1-PM1)

---

## Project Overview

In this project I set up a virtual cybersecurity lab on my own laptop using VirtualBox and Kali Linux. Kali runs as the attacking machine inside a private NAT Network, so more target machines can be added later for authorized practice.

## Objectives

- Install VirtualBox and import Kali Linux
- Create a NAT Network in the 10.0.0.0/24 subnet
- Give Kali the static IP 10.0.0.2/24 with full internet access
- Enable clipboard and drag/drop (Bidirectional)
- Share the host Downloads folder with Kali
- Take a snapshot as a clean baseline

> **Important:** This lab must only be used on systems I own or have explicit permission to test.

## Lab Configuration

| Component | Configuration |
|---|---|
| Laptop | MECHREVO R14P Series |
| Host OS | Windows 11 Pro Education (Build 26300) |
| Processor | AMD Ryzen 5 7430U, 6 cores, 2.3 GHz |
| Host RAM | 8 GB |
| Hypervisor | Oracle VirtualBox (VERSION_HERE) |
| Security OS | Kali Linux 2026.3 (prebuilt VirtualBox amd64 image) |
| Kali RAM / CPU / Disk | 2048 MB / 2 processors / 80.09 GB |
| Virtual Network | NAT Network (`NatNetwork`) |
| Network Address | 10.0.0.0/24 |
| Kali IP | 10.0.0.2/24 |
| Gateway | 10.0.0.1 |
| DNS | 8.8.8.8 |
| Adapter Type | Intel PRO/1000 MT Desktop |

## Setup Steps

### Step 1. Install 7-Zip and VirtualBox
I installed 7-Zip (needed to extract the Kali archive) and VirtualBox on my Windows 11 laptop.

### Step 2. Create the NAT Network
In VirtualBox: File > Tools > Network > NAT Networks > Create.
Name: `NatNetwork`, IPv4 prefix: `10.0.0.0/24`, DHCP enabled.

![NAT Network](screenshots/1-nat-network.png)

### Step 3. Import Kali Linux
I downloaded the prebuilt VirtualBox image from kali.org, extracted it with 7-Zip and added it to VirtualBox. Network Adapter 1 is attached to NAT Network `NatNetwork`.

![Kali VM details](screenshots/2-kali-vm-details.png)

### Step 4. Clipboard, Drag and Drop, Shared Folder
- Shared Clipboard: Bidirectional
- Drag'n'Drop: Bidirectional
- Shared folder: my Windows Downloads folder, Access: Full, Auto Mount: Yes. It appears inside Kali as `sf_Downloads`, and files can be moved in both directions.

![Shared folders](screenshots/3-shared-folder.png)
![sf_Downloads on Kali desktop](screenshots/4-kali-desktop.png)

### Step 5. Static IP in Kali
Network icon > Edit Connections > Wired connection 1 > IPv4 Settings:
Method Manual, Address `10.0.0.2`, Netmask `24`, Gateway `10.0.0.1`, DNS `8.8.8.8`.

![Kali network settings](screenshots/5-kali-network.png)

### Step 6. Snapshot
I took a snapshot named **Fresh Kali 10.0.0.2** on 28/10/2026 as the clean baseline of the lab.

![Snapshot](screenshots/6-snapshot.png)

## Verification

| Test | Command | Result |
|---|---|---|
| IP address | `ip a` | Kali shows 10.0.0.2/24 |
| Gateway | `ping 10.0.0.1` | Replies received |
| Internet | `ping 8.8.8.8` | Replies received |
| DNS | `nslookup networkwalks.com` | Resolved through 8.8.8.8 |
| Shared folder | copy a file between Kali and Windows | Works both ways via `sf_Downloads` |
| Clipboard | copy text in Windows, paste in Kali | Works |

![nslookup](screenshots/7-nslookup.png)

## Problems Encountered and Solutions

**Problem 1. Installing and importing Kali was difficult at first.**
This was my first time working with a virtual machine. The Kali download is a large archive, so I had to extract it with 7-Zip first and then add the extracted machine to VirtualBox instead of installing from scratch. Once I understood that the prebuilt image is already installed, the import was easy.

**Problem 2. The shared folder was confusing.**
I did not know at first how the host folder would show up inside Kali. I learned that a shared folder needs the correct host path, Full access and Auto Mount enabled, and that it appears in Kali with an `sf_` prefix (`sf_Downloads`). If access is denied, the user can be added to the `vboxsf` group:

```
sudo usermod -aG vboxsf kali
```

**Small mistake.** My first `nslookup` returned `NXDOMAIN` only because I typed the domain name wrongly. After correcting the spelling, it worked. This taught me to read the command carefully before assuming a network problem.

## What I Learned

- How to build an isolated lab with VirtualBox and a custom NAT Network
- Why a static IP (10.0.0.2) makes a lab easier to document and use
- How shared folders, clipboard and drag/drop connect the host and the VM
- Why snapshots are useful: they give a clean state to restore after experiments
- How to verify a network step by step: IP, gateway, internet, DNS

## Author

Muhammad Talha Khan Tareen | Student, NetworkWalks Cybersecurity course
