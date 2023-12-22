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

<h1> Powershell: </h1>

Used powershell to programmatically create 1,000 users in Active Directory by interating over a specified .txt file containing auto generated names to create test accounts to login to our "Client" virtual machine with.

```
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$USER_FIRST_LAST_LIST = Get-Content .\users.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name CREATED_USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Description 'Created via a Script' `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=CREATED_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}
```
Temporarily changed the default "Execution Policy" to unrestricted for the purpose of this project and added my own name to the "names.txt" file that will be used by the powershell script to programatically create a test account with my name. 

<p align="center">
name.txt - Adding my name to the file
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/6f3f8de6-206b-4316-a224-c5ff02942ad5"/>
<br />

<p align="center">
Running the Powershell Script - Console Output
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/f400b767-40a1-4e8c-859f-244c173ef199"/>
<br />

After confirming that the script sucessfully ran we can check ADUC to confirm that all of our users exist in their intended OU='CREATED_USERS'.

<p align="center">
ADUC - After running the script
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/7ff8b0b5-7f32-4471-b283-6613c613c17f"/>
<br />

<h1> Client VM: </h1>

Created a client VM in VirtualBox with the configuration shown below and created a local admin account to join the "Client" to our domain.

<p align="center">
VirtualBox - Client VM Configuration
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/8f367d9a-0ada-43a1-8909-b6e8097406f5"/>
<br />

By using the internal NIC this allows our "Client" to communicate with our domain controller in order to join the domain.

<p align="center">
Client VM - Installing Windows 10 Pro
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/8f367d9a-0ada-43a1-8909-b6e8097406f5"/>
<br />

NOTE: Windows 10 Pro was installed on the Client VM because it's provides us the option of joining a domain which is necessary to join the domain we've created.

<p align="center">
Client VM - Creating Local Admin Account
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/01a5ee48-8604-4d1c-8d01-d0355d452352"/>
<br />

After the initial setup of Windows we can navigate to advance system settings to allows us to rename the computers name and join the domain.

<p align="center">
Client VM - Renaming to 'Client-1' and joining the domain (mydomain.com)
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/4776d162-4148-4bb3-b18d-a61f4e52616f"/>
<br />

We can now authorize this change with the account: atanco_HDA that we created earlier who is a member of the "Domains Admin" group.

<p align="center">
Client VM - Successfully joined machine to mydomain.com
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/57f99d11-71f8-4338-aaf0-12bd0b1b3ddd"/>
<br />

We should now be able to view our newly joined machine in ADUC from our domain controller as confirmation.

<p align="center">
Domain Controller - Confirming "Client-1" successfully is apart of the domain
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/9c1d2318-b936-4497-8bf0-99cce752d9b7"/>
<br />

Now that we confirmed that the machine is successfully joined to the domain, we can now finally login as any of the users we programmatically created with our script to ensure everything is working as intended.

<p align="center">
Client VM - Login as acosey (generatered AD account from our script)
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/f62330da-52a6-4df6-8ea3-61bfba80b375"/>
<br />

<p align="center">
Client VM - Login as acosey (running CMD 'whoami' command)
<br/>
<img src="https://github.com/AntonioTanco/ActiveDirectoryLab/assets/43735570/d38e65a6-dedf-4d75-8179-cd27725f65b4"/>
<br />

We can successfully login as any of our auto generated AD accounts we created via our script and we can confirm at this machine is on the domain by running the CMD Command: whoami

<h2>Hard Skills Demonstrated </h2>
- Windows Server 2019 
- Active Directory  
- Active Directory Domain Service (AD DC)

- Creating a Domain
- Setup/Configure RAS and NAT
- Setup/Configuring DNS & DHCP
- PowerShell Scripting
- Networking
