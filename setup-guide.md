# 🛠️ Active Directory Home Lab Setup Guide

## 📌 Overview
This guide walks through building a fully functional Active Directory home lab using VirtualBox, Windows Server 2022, and a Windows 10 client.

---

## 🧰 Requirements
- Oracle VirtualBox
- Windows Server 2022 ISO
- Windows 10 ISO
- At least 8GB RAM recommended

---

## 🖥️ Step 1: Create Virtual Machine

- Create new VM (Server22)
![Create VM](screenshots/create-vm.png)

- Allocate system resources (4GB+ RAM recommended)
![Resource Allocation](screenshots/resource-allocation.png)

- Set virtual disk size (32GB recommended)
![VM Disk Size](screenshots/vm-disk-size.png)

---

## 🌐 Step 2: Configure Networking

- Adapter 1: NAT (internet access)
![NAT Adapter](screenshots/network-nat-adapter.png)

- Adapter 2: Internal Network
![Internal Adapter](screenshots/network-internal-adapter.png)

---

## 💻 Step 3: Install Windows Server 2022

- Start installation
![Install Start](screenshots/server-install-start.png)

- Select Desktop Experience version
![Version Selection](screenshots/server-version-selection.png)

- Set Administrator password
![Password Setup](screenshots/server-password-setup.png)

---

## ⚙️ Step 4: Initial Configuration

- Install Guest Additions
![Guest Additions](screenshots/guest-additions-install.png)

- Configure static IP (172.16.0.1)
![IP Config](screenshots/ip-configuration.png)

- Rename server to DC
![Rename PC](screenshots/rename-pc-dc.png)

---

## 🏢 Step 5: Install Active Directory

- Add AD DS role
![Add Roles](screenshots/add-roles-features.png)

- Promote to Domain Controller (mydomain.com)
![Domain Setup](screenshots/domain-controller-promotion.png)

---

## 👤 Step 6: Create Admin User

- Open AD Users & Computers
![AD Console](screenshots/ad-users-console.png)

- Create Admin OU and user
![Create User](screenshots/create-admin-user.png)

- Add to Domain Admins
![Add Group](screenshots/add-to-domain-admins.png)

---

## 🌐 Step 7: Configure DHCP

- Install DHCP role
![DHCP Install](screenshots/dhcp-install.png)

- Create scope (172.16.0.100–200)
![DHCP Scope](screenshots/dhcp-scope-range.png)

---

## ⚡ Step 8: PowerShell Automation

```powershell
Import-Csv names.txt | ForEach-Object {
    New-ADUser -Name $_.Name -GivenName $_.FirstName -Surname $_.LastName
}
