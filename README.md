This project provides a comprehensive guide to setting up an Active Directory (AD) home lab. This lab serves as a valuable environment for skill development and learning about Windows Server and Active Directory administration. The project involves installing AD, promoting the server to a Domain Controller, configuring DNS, DHCP, and RAS/NAT, populating AD with bulk users using PowerShell, and joining a client Windows 10 PC to the domain. This project is follows [Josh Madakor's](https://www.linkedin.com/in/joshmadakor/) YouTube [video:](https://www.youtube.com/watch?v=MHsI8hJmggI&t=2511s) "How to Setup a Basic Home Lab Running Active Directory (Oracle VirtualBox) | Add Users w/PowerShell"

First, we need to download VirtualBox, VirtualBox Extension pack, Microsoft Server 2022, and Microsoft Windows 10.

**Download and Install [VirtualBox](https://www.virtualbox.org/)**

**Download VirtualBox [Guest Additions](https://www.virtualbox.org/wiki/Downloads)**

**Download [Windows Server 2022 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) from Microsoft Evaluation Center**

**Download [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10)from Microsoft**

**Create a New Virtual Machine (VM)**

- Open VirtualBox and select "New".
    
- Name the VM (Server22).
    
- Select the Windows Server 2022 ISO.
![Create-New-VM](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/987a6815-819b-4acd-89c6-63682b1f2ee5)
