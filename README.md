# 🖳 Active Directory Lab

<h2>Description</h2>
<b>Installed and configured Windows Active Directory to allow a “client” (Virtual Machine) to connect to a domain controller via a private network / internal NIC (VPN). RAS and NAT are configured in a way which allows DHCP to locate the Active Directory Domain server through DNS all while handling IP Address Assignments for the host and IP Address leasing. </b>
<br />

<h2>Languages and Software Used</h2>

- <b>PowerShell</b> 
- <b>Oracle VM VirtualBox</b>

<h2>Virtual Environments Used </h2>

- <b>Windows 10 Pro</b>
- <b>Windows Server 2019 (GUI)<b>

<h2>Project walk-through:</h2>

<p align="center">
Network Topology<br/>
<p align="center">
Diagram of how the network was architected for this lab<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/2a3e708d-df18-4cfb-a48a-f19c5b14c6a3"/>
<br />

<p align="center">
VirtualBox Network Configuration for Windows Server 2019 VM<br/>
<p align="center">
Enabled two NIC's on the VM: One thats connected to the public internet (DHCP - IP Addressing from ISP) and one internal NIC that our "client" will use to communicate with our Domain Controller over the virtual network<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/901633c6-7ab8-40b6-94c6-a956306b8d03"/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/7b78930a-0a0d-4906-b3ab-c39eca8d588b"/>
<br />

After booting into Windows Server 2019 and going through the initial install I've identified the internal NIC and configured the IPv4 properties to assign a static IP of 172.16.0.1, Subnet Mask: 255.255.0.0 and a default Gateway: 127.0.0.1 in accordance with the network diagram which will be used later in the project to configure RAS/NAT, DNS and DHCP.
<br />
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/281a3085-dcb5-4beb-9c0b-fc5349359441"/>
<br />

<p align="center">
Installing and Configuring Active Directory Domain Services<br/>
<p align="center">
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/fd9b733e-1b1e-406c-a4c2-943ccb81c024"/>

After AD DS role was installed I then promoted the server to a Domain Controller with the following Configuration:

- <b>New AD Forest</b>
  - Target Server: DOMAIN-CONTROLLER 
  - ROOT NAME: mydomain.com
- <b>Domain Controller Options</b>
  - Forest Funtional Level: Windows Server 2016
  - Domain Functional Level: Windows Server 2016
 
  Domain Controller Capabilities:
  - Enabled: Domain Name System (DNS) Server
  - Enabled: Global Catalog (GC)
  - Disabled: Read only domain controller (RODC)

- <b>Set an DSRM Password</b>
- <b>PATHS left the as default</b>

<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/b96d8a4f-65d7-421f-a3c1-d3ee73ad30a4"/>

<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/619ca6f1-0eb6-4f27-bf9e-d092b8de4391"/>

After configuring AD DS, I've created 3 OU's. The IT OU will house IT servers + Help Desk Users OU which will house accounts with admin privileges.

So I created an new account for myself that I will promote to domain admin that I will login from now on.

<p align="center">
Creating a new account | Domain Admin member<br/>
<p align="center">
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/d4e44f49-b5c6-4e8b-a9bf-9725caddc8ed"/>








<h2>Hard Skills Demonstrated </h2>
- Windows Server 2019 
- Active Directory  
- Active Directory Domain Service (AD DC)

- Creating a Domain
- Setup/Configure RAS and NAT
- Setup/Configuring DNS & DHCP
- PowerShell Scripting
- Networking
