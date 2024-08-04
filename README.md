<p align="center">
<img src="https://i.imgur.com/MPvuCEH.png" alt="osTicket logo"/>
</p>

<h1>On-Premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within two Azure virtual machines.<br/>


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)



<h2>Deployment and Configuration Steps</h2>

<h3>Step 1: Setup Resources in Azure</h3>
A. Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
B. Set Domain Controller’s NIC Private IP address to be static
C. Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.A
D. Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

<img src="https://i.imgur.com/ZzmZhtZ.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/fnzjcB2.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/rB36Rt7.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/tc1jHKk.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/DbrsuNw.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/tW7330H.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/wUVM44a.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/ovi1OAf.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/FtjvqxD.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/okIFxei.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/ynHZ6I0.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/4OxFm0H.png" alt="osTicket logo"/>

<h3>Step 2: Ensure Connectivity between the client and Domain Controller</h3>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
Login to the Domain Controller and enable ICMPv4 in on the local Windows Firewall
Check back at Client-1 to see the ping succeed

<img src="https://i.imgur.com/VraeNHx.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/HAqbRok.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/sWoY7M9.png" alt="osTicket logo"/>

<h3>Step 3: Install Active Directory</h3>
Login to DC-1 and install Active Directory Domain Services 
Promote as a DC: Setup a new forest as mydomain.com (this can be anything just rememeber what it is)
Restart and then log back into DC-1 as user: mydomain.com\labuser

<img src="https://i.imgur.com/bA6H7SX.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/OGacTBQ.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/F05Ir0t.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/3jjOFg2.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/OgaBdZ4.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/VcnlNsU.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/BeS6YkL.png" alt="osTicket logo"/>

<h3>Step 4: Create an Admin and Normal User Account in AD</h3>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on

<img src="https://i.imgur.com/VapLLhG.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/IGIVerT.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/tjFLdam.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/xngFYT7.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/ErWYn1f.png" alt="osTicket logo"/>

<h3>Step 5: Join Client-1 to your domain (mydomain.com)</h3>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there 

<img src="https://i.imgur.com/Jqa8koK.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/8AyjLOf.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/EnNHE7q.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/7FDFHYv.png" alt="osTicket logo"/>

<h3>Step 6: Setup Remote Desktop for non-administrative users on Client-1</h3>
Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once

<img src="https://i.imgur.com/8zkHXi8.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/SFH6Anw.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/O2kLEC5.png" alt="osTicket logo"/>

<h3>Step 7: Create a bunch of additional users and attempt to log into client-1 with one of the users</h3>
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it 
Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)

<img src="https://i.imgur.com/x9rUt2Z.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/dT2o6XL.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/03G5AiD.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/VFx7Epi.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/mplTtup.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/IpuFZq7.png" alt="osTicket logo"/>
<img src="https://i.imgur.com/DTx14jm.png" alt="osTicket logo"/>
