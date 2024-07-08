<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity
- Install Active Directory
- Create User Accounts in AD
- Join Client-1 to Domain
- Setup Remote Desktop for Users
- Create Additional Users and Test

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="1440" alt="Screen Shot 2024-06-28 at 12 08 19 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/ab2968c9-746e-4160-94d6-12a12cdbb9ff">

</p>
<p>
Create 2 virtual machines in Azure, one with Windows Server 2022 and one with Windows 10. Name the one with Windows Server 2022 DC-1 and the other one Client 1. Make sure that they are both in the same resource group and virtual network. 
</p>
<br />

<p>
<img width="1440" alt="Screen Shot 2024-06-28 at 11 59 14 AM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/c337969c-d9a0-422c-9323-5ef625fa15bb">

</p>
<p>
Go into DC-1's Virtual NIC settings and set the IP Address to static. This will ensure the IP address does not change. 
</p>
<br />

<p>
<img width="1440" alt="Screen Shot 2024-06-28 at 12 33 14 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/d71ed208-14dc-47c2-aa00-41d185e496f0">

</p>
<br />
<img width="1440" alt="Screen Shot 2024-06-28 at 12 35 46 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/e1120463-9611-46dd-baaa-7e0f009dcd74">

</p>
<p>
Log-into DC-1 and on the server manager click "Add Roles and Features", and hit next until you reach server roles. Check the Active Directory Domain Services box and click "Add Features" and install it. Once done, hit the flag and click "promote this server to a domain controller. In the configuration wizard select "Add a new forest" and create your domain name. Click next and set your restore mode password. 
</p>
<br />
<p>
<img width="1440" alt="Screen Shot 2024-06-28 at 12 56 44 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/649d9958-6cfc-49e3-88de-ff203c24f5cf">

</p>
<p>
Restart and log back into DC-1 as a normal user. Make sure to put @yourdomain.com after the username since we just promoted it to a domain controller.  In server manager go to tools --> Active Directory Users and Computers and create a new organizational unit called _EMPLOYEES and one called _ADMINS
</p>
<br />
<p>

<img width="1440" alt="Screen Shot 2024-06-28 at 12 59 52 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/d23465fe-218f-4281-bad5-ff54b3047d97">
</p>
<br />
<img width="1440" alt="Screen Shot 2024-06-28 at 1 06 33 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/60f35f8e-8e6e-489a-a73c-0d7c45b665c5">

</p>
<p>
Create a new Admin user with your name and add them to the security group "Domain Admins". This isn't required but it's good practice to have an admin account attached to a human name. Log out and log back in as this user. 
</p>
<br />
<p>
  
<img width="1440" alt="Screen Shot 2024-06-28 at 1 24 51 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/ec6ab2a4-b1f4-4152-93c3-16e32f9fdd6b">
</p>
<br />

<img width="1440" alt="Screen Shot 2024-06-28 at 1 33 23 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/5fe1f29a-888a-40a1-99da-cdcd9f2318dd">
</p>


<p>
From the Azure portal, set the DNS server of client 1 to DC-1's private IP address. Then restart it within the portal. 
</p>
<br />
<p>
  
<img width="1440" alt="Screen Shot 2024-06-28 at 1 24 18 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/16211b4c-9898-46c3-a197-9c0dd78761d9">

</p>
<p>
  
Log into client one and right click the Windows start button on the bottom left, go to system --> Rename this PC (Advanced) --> click change --> click domain and enter the domain name that you created earlier. The PC will restart and you will need to log back in 
</p>
<br />
<p>
  
<img width="1440" alt="Screen Shot 2024-06-28 at 1 38 25 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/c22a8b84-42a3-428e-85ec-99da052e113e">


</p>
<p>
Right click the Windows button on the bottom left of the screen and go to system --> Remote Desktop --> Select users that can remotely access this PC --> Add and then enter the name "Domain Users" and click check names. Then click ok. This allows any of the users to remotely log onto client 1. 

</p>
<br />
<p>
  
<img width="1440" alt="Screen Shot 2024-06-28 at 1 42 32 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/20222fbf-43b6-4622-af36-a8e173eed6c3">
</p>
<br />
<img width="1440" alt="Screen Shot 2024-06-28 at 1 44 52 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/16c92e46-dc32-4e53-86b3-adabf9efa83f">


</p>
<p>
Login to DC-1 as admin account created earlier, open PowerShell_ise as an administrator, create a new file and paste the contents of this script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1), run the script and observe the accounts being created, when finished open Active Directory Users and Computers and observe the accounts in the _EMPLOYEES folder. 

</p>
<br />
<p>
  
<img width="1440" alt="Screen Shot 2024-06-28 at 1 46 13 PM" src="https://github.com/Bpeduru/configure-ad/assets/171273980/0182b878-c812-455f-9a18-cb8c3d268a61">


</p>
<p>
  
Attempt to log into Client-1 with one of the accounts (take note of the password in the script). 
</p>
<br />

