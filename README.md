## This project provides a comprehensive guide to setting up an Active Directory (AD) home lab. This lab serves as a valuable environment for skill development and learning about Windows Server and Active Directory administration. The project involves installing AD, promoting the server to a Domain Controller, configuring DNS, DHCP, RAS/NAT, populating AD with bulk users using PowerShell, and joining a client Windows 10 PC to the domain. This project is follows [Josh Madakor's](https://www.linkedin.com/in/joshmadakor/) YouTube [video:](https://www.youtube.com/watch?v=MHsI8hJmggI&t=2511s) "How to Setup a Basic Home Lab Running Active Directory (Oracle VirtualBox) | Add Users w/PowerShell"

First, we need to download VirtualBox, VirtualBox Extension pack, Microsoft Server 2022, and Microsoft Windows 10.

**Download and Install [VirtualBox](https://www.virtualbox.org/)**

**Download VirtualBox [Guest Additions](https://www.virtualbox.org/wiki/Downloads)**

**Download [Windows Server 2022 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) from Microsoft Evaluation Center**

**Download [Windows 10 ISO](https://www.microsoft.com/en-us/software-download/windows10) from Microsoft**

**Create a New Virtual Machine (VM)**

- Open VirtualBox and select "New".
    
- Name the VM (Server22).
    
- Select the Windows Server 2022 ISO.
![Create-New-VM](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/987a6815-819b-4acd-89c6-63682b1f2ee5)

- Allocate resources. This will depend on how much RAM and CPU cores you have available. I suggest at least 4GB of RAM.
![Resource-Allocation](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/e3373a17-8bcd-4cc0-8336-1d08db4ffcbc)

- Selet size of Virtual Disk. I suggest at least 32GB.
![Select-VM-Disk-Size](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/c05a8d53-605d-463d-8954-e3ac48e1a783)

For this project, we are going to use two network adapters for the Server.
- Use NAT for adapter 1. This will allow the VM to connect to the internet through the host and automatically receive an IP address.
![1714403282713](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/25d67377-1edd-4720-8364-3c28f28a97d1)

- Add a second network adapter and select "Internal". This adapter will be given a static IP address and will allow our client machine to receive an IP address from a defined range of IP addresses.
![1714403781688](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/a0add659-83d9-4618-8a8c-8c24db06bffd)

**Start up the VM and Install Windows Server 2022**
- Follow the installation prompts.
![1714403810498](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/3ef85f99-28e1-49ce-b509-5b673477a0fd)

- Select Windows Server 2022 Standard Evaluation (Desktop Experience).
![1714403826022](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/08650fb0-5dbf-43fd-a0c7-32633391d74d)

- Set up a password for logging into Windows Server. Since this is just a lab, we will use "Password1" for logins.
![1714403848209](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/59a59cbc-94b4-453a-b3cd-aeec35f312ab)

**Install VirtualBox Guest Additions**
- After installation of Server 2022, insert the Guest Additions CD. Open file explorer, locate CD Drive (D:) VirtualBox Guest Additions and install Guest Additions.
![1714403876525](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/3ee913ef-47b3-4b2c-93b5-5c12f3e13cfd)

- Follow the prompts to install guest additions. Reboot.
![1714403896610](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/10e509dc-8817-448d-824d-2d1783ae4256)

Rename network adapters for clarity. Open up properties on each adapter to verify which one is the NAT adapter and which is the Internal adapter. Here we gave the NAT adapter the name "_Internet_" and the Internal adapter we named "X_Internal_X".
![1714404086706](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/80dd9ccd-faf6-48d4-980e-334509449c78)

Configure IPv4 settings, ensuring automatic IP for Internet (_Internet_) adapter and static IP 172.16.0.1. Set the subnet mask as 255.255.255.0 and leave the default gateway empty. For the DNS server, we can use the same static IP (172.16.0.1) or use the loopback address 127.0.0.1 for internal (X_Internal_X) adapter.
![1714405761439](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/4dde30ba-87a9-4b07-92a8-5062acc3fcbd)

Rename the PC as "DC" for easy identification  by going to Start > Settings > System > About > Rename This PC. Restart.
![1714404207962](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/bafc4f83-d8ca-46df-8496-2598b80c7dea)

**Install Active Directory Domain Services (ADDS)**
- Open Server Manager and click on Add Roles and Features.
![1714404234032](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/dc41957a-fc5a-445d-a8b3-856e23c0406f)

- Select Role-based, select the server and choose Active Directory Domain Services.
- Complete the installation process.
![1714404267549](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/3589d489-7cdd-4f47-84af-ec68450b4c96)

- Promote the server to a domain controller, specifying a new forest name mydomain.com.
![1714404289186](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/95695678-2528-4b0d-b854-e20fddd51e9a)
![1714404357473](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/f020a0cf-0fb1-4cac-90ea-7fb9e22a70fe)

- Create a password for Directory Service Restore Mode (DSRM). Since this is a lab, we can use the same password we used from earlier. Verify NetBIOS name. This should be the same as your domain name. (mydomain). Complete the installation and restart the server.
![1714404389723](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/564894a5-b880-4ce8-bfc6-f3d2a85fb3e4)

**Create Administrative User** - After restart, the login now shows MYDOMAIN\ADMINISTRATOR. Login with the password you created created earlier.
![1714404439555](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/a0321640-d36b-44e7-83bc-4dc24d9eadb1)

- Open Active Directory Users and Computers.
![1714404466597](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/0f982c32-2550-4e40-85f6-3d7287b8456f)

- Create a new Organizational Unit named "_Admins".
![1714404500688](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/7b20db08-916a-410d-9b50-c6ce1e413747)

- Add a new user to the _Admins OU.
![1714404530984](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/575614d3-5b19-4cf3-a628-d177d1b68824)

- Add user  to Domain Admins group.
![1714404552458](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/837b4f25-dda4-4799-97e1-fd6414ef303c)

- Logout out the built-in administrator account. Log back in with the new domain admin account.
![1714404586514](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/e3214d98-cb42-43e3-af1b-9698260d6cde)

**Install Routing and Remote Access (RAS/NAT)**
- Install RAS/NAT through "Add Roles and Features".
![1714404702636](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/23e20d46-7a05-4f88-bfb0-17448fb0bd32)

- Open Server Manager and go to Tools > Routing and Remote Access.
![1714404744346](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/f4765991-298d-474f-8a9a-5f94ba4900e3)

- Configure and enable routing and remote access, selecting NAT and specifying the public interface (_Internet_). Inside of "Routing and Remote Access", you may have to right-click on DC and click refresh to get the interfaces to show or close and re-open Routing and Remote Access.
![1714404792045](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/18af2de1-b02b-4206-8e96-d3458ed0a645)

**Set Up DHCP Server** - This will allow our client to receive an IP address in the scope that we are going to specify.
- Install DHCP Server role through "Add Roles and Features".
![1714404859730](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/09ec86ce-f16f-4e52-8bbd-875d15d03241)

- In Server Manager, go to Tools > DHCP. Click the drop down arrow next to dc.mydomain.com, right-click IPv4 and select "New Scope".
![1714404882028](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/e652a481-6dc7-4392-949a-9e4d1c8d9940)

Name the new scope "172.16.0.100-200" and specify the DHCP scope.
![1714404925026](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/9dafbfb2-aa70-4295-96a5-4aa40e364752)

- Skip IP Exclusions and leave the lease time as "8 days". Configure Router (Default Gateway) IP Address as 172.16.0.1. Click "Add".
![1714404967200](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/067ff732-f131-47c2-9c7e-5636b65af246)

- For DNS, leave the parent domain as mydomain.com.
![1714405021697](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/9b3858d1-f67b-4856-9706-9063d906b540)

- Leave WINS Server blank and click Next. Select "Yes, I want to activate this scope now". Authorize the DHCP server by right-clicking on dc.mydomain.com and select "Authorize". Refresh IPv4.
![1714405055831](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/79ffcd40-26eb-4ffc-bfdb-39c2cad9a682)

**Bulk User Creation with PowerShell**
- Open Microsoft Edge browser and navigate to this [URL](https://github.com/joshmadakor1/AD_PS).
- Download ZIP.
![1714405083859](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/dae4ec9e-b340-43e5-8afc-81dfbc942b0d)

- Open File Explorer and navigate to Downloads. Extract the ZIP file to the Desktop.
-  Run PowerShell ISE as administrator. Open the script. Use the CD command to to navigate to our extracted folder (cd C:/Users/a-jdoe/Desktop/AD_PS-master). Run the ls command command to display files in this folder. Should see "names.txt".
![1714405257091](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/90ac2d8c-297d-4b1e-b5b1-70b1b060e0bf)

- Run Set-ExecutionPolicy-Unrestricted and answer "yes".
![1714405291721](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/ed9b173c-d087-431e-855d-2c7007a09c35)

- Run the script and answer "Run Once" to the Security Warning prompt.
![1714405315678](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/5fad4efc-bd79-4e4d-aa25-098b2fed396b)

- When the script has finished running, exit out of PowerShell.
![1714405339180](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/e7f2c281-ff78-42e4-8f46-08689fb3466d)

- Verify user creation in Active Directory Users and Computers.
![1714405381945](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/f1fc7365-8716-4ceb-ac78-700b4d7367e5)

**Create Windows 10 Pro Client VM with the Windows 10 ISO**
- Create a new VM named "Client1" with appropriate settings.
![1714405419153](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/b6a3f37b-b083-497b-8f40-21bbc440d0ff)

- Change Client1 VM's network adapter to the "Internal" adapter we used for the server/ DC.
![1714405526362](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/3fd53604-c934-40c7-b0f2-403968807a88)

- Install Windows 10 Pro. Create a local account.
![1714405568853](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/4c2c88cd-05ed-4a4f-8de6-4de5099015c1)

- Install VirtualBox Guest Additions and restart the VM
- Change the client's DNS server to the IP address of the DC's X_Internal_X adapter.
![1714405977872](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/ffe35b27-0b97-4aec-9b3a-af78605522e8)

- Open the command prompt and run ipconfig. The client should have received an IP address from the DC.
![1714406011255](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/f32a047e-c576-49b2-8834-1222489a41d9)

- Rename the PC to "Client1" and join the domain (mydomain.com). Use the administrator credentials to authorize.
![1714406076482](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/af458bb9-f13f-42d8-93e0-5d118c613f7a)

- Restart the PC and log in with a domain user account.
![1714406117832](https://github.com/jdcarlyle1317/AD_PS/assets/88341973/3f11e4fd-9166-41a8-9f28-ebe4c79227d6)

 Starting with setting up VirtualBox and downloading essential software, we progressed through creating and configuring the virtual machine, installing Windows Server 2022, and adding Active Directory Domain Services (ADDS), Routing and Remote Access (RAS/NAT), and a DHCP Server.
We finished with bulk user creation using PowerShell and configuring a Windows 10 client machine to join the domain.
By following this guide, users can create a robust AD home lab environment, providing hands-on experience essential for mastering Windows Server and Active Directory administration.



