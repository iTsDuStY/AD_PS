# Active Directory Lab with PowerShell Automation

## 📌 Overview
This project demonstrates building and managing an Active Directory environment in a virtual lab, with a focus on automating user creation using PowerShell.

The lab simulates real-world IT administration tasks including domain setup, network services configuration, and bulk user provisioning.

---

## 🎯 Key Skills Demonstrated
- Active Directory Domain Services (AD DS)
- PowerShell scripting and automation
- DNS and DHCP configuration
- Domain user and group management
- Windows Server administration

---

## 🛠️ Environment
- Windows Server 2022 (Domain Controller)
- Windows 10 (Client Machine)
- Oracle VirtualBox

---

## ⚙️ What I Built
- Configured a Domain Controller (mydomain.com)
- Set up DNS and DHCP services for internal network
- Created Organizational Units and admin accounts
- Automated creation of 1,000+ users using PowerShell
- Joined Windows 10 client to the domain
- Verified authentication and domain functionality

---

## 🚀 PowerShell Automation
A script was used to bulk-create users from a text file.

### Example:
```powershell
# Create users from names.txt
Import-Csv names.txt | ForEach-Object {
    New-ADUser -Name $_.Name -GivenName $_.FirstName -Surname $_.LastName
}
```
By following this [guide](https://github.com/iTsDuStY/AD_PS/blob/main/setup-guide.md), users can create a robust AD home lab environment, providing hands-on experience essential for mastering Windows Server and Active Directory administration.



