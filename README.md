# 🖳 Active Directory Lab

<h1>Description</h1>
<b>Installed and configured Windows Active Directory to allow a “client” (Virtual Machine) to connect to a domain controller via a private network / internal NIC (VPN). RAS and NAT are configured in a way which allows DHCP to locate the Active Directory Domain server through DNS all while handling IP Address Assignments for the host and IP Address leasing. </b>
<br />

<h1>Languages and Software Used</h1>

- <b>PowerShell</b> 
- <b>Oracle VM VirtualBox</b>

<h1>Virtual Environments Used </h1>

- <b>Windows 10 Pro</b>
- <b>Windows Server 2019 (GUI)<b>

<h1>Project walk-through:</h1>

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

<p align="center">
Logging as Atanco_HDA | Newly Created Domain Admin<br/>
<p align="center">
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/365ffac0-6b89-4af2-bc27-18d4c88a5eee"/>

<h1> Networking: </h1>

After configurating our domain controller we now need to setup and configure our networking to allow for our "clients" to communicate with our domain controller. 

- <b>Server Roles Installed/Configured:</b>
  - Remote Access
    - DirectAccess and VPN (RAS)
    - Routing

  - DHCP
      - DNS
      - IP Address Scope
      - IP Lease Duration
      - Router

<h2> Remote Access </h2>

<p align="center">
Installing Remote Access
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/1179dbb7-a43d-4ace-846f-cac97347e808"/>
<br />

<p align="center">
Installing Remote Access - Service Roles
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/2a2b201e-8621-44ba-8d6d-8ec15fa05068"/>
<br />

<p align="center">
Configurating Remote Access - Network Address Transalation (NAT)
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/6405a161-0d2f-49ee-9401-ee8494aa64ce"/>
<br />

<p align="center">
Configurating Remote Access - Network Address Transalation (NAT)
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/6405a161-0d2f-49ee-9401-ee8494aa64ce"/>
<br />

![Domain Controller - Configuring Remote Access Role - 3](https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/3194d547-daa0-419a-ab68-a8f0a8a1dbec)

Choosing the "Public" NIC will allow our "clients" to access the PUBLIC internet assuming we properly configure DHCP to use our domain controller as a router (default gateway) for all clients connected via the private network or internal NIC.

<h2> DHCP </h2>

<p align="center">
Installing DHCP - Server Role
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/2d8299e0-dda6-49dc-8229-69bda8c32693"/>
<br />

<p align="center">
Configurating DHCP - Defining an IP Address Scope
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/6554a6e5-aa30-4f3c-85eb-2da72a56ef8e"/>
<br />

<p align="center">
Configurating DHCP - Defining a lease duration
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/399e7ab6-f95c-466e-825c-e38b6368705c"/>
<br />

<p align="center">
Configurating DHCP - Specifying a router 
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/03c62b11-446b-467d-b232-1da3f219ebe2"/>
<br />

<p align="center">
Configurating DHCP - Specifying a DNS
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/71b88da3-e258-428d-a438-fad83ada88a2"/>
<br />

<p align="center">
Configurating DHCP - Authorizing Our DHCP Server
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/a53fa4c9-e7b2-43a6-8960-c6919e0990b2"/>
<br />


<h2>Hard Skills Demonstrated </h2>
- Windows Server 2019 
- Active Directory  
- Active Directory Domain Service (AD DC)

- Creating a Domain
- Setup/Configure RAS and NAT
- Setup/Configuring DNS & DHCP
- PowerShell Scripting
- Networking
