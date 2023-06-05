<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: On premises Active Directory Deployed in the Cloud Azure](https://youtu.be/nLdh1Tb3jvc)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>


Step 1. Setup Resources in Azure
    
1.a Create the Domain Controller VM (Windows Server 2022) named “Random Name”

1.1 Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
    
1.b Set Domain Controller’s NIC Private IP address to be static
    
1.c Create the Client VM (Windows 10) named “Random Name”. Use the same Resource Group and Vnet that was created in Step 1.1
</p>
<br />

<p>
<img src="https://i.imgur.com/sBTFEXj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/X3gzNA0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/DhdLYay.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>

Step 2. Install Active Directory
    
2.a Login to DC-1 and install Active Directory Domain Services

2.b Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)

2.c Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://i.imgur.com/T5jiBQO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/UuqD9aL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ZJwara5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/WA9qQHW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/FTCv0O7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/xaRqoEv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>

Step 3. Create an Admin and Normal User Account in AD
    
3.a In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

3.b Create a new OU named “_ADMINS”

3.c Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

3.d Add jane_admin to the “Domain Admins” Security Group

3.e Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain  .com\jane_admin”

3.f User jane_admin as your admin account from now on
</p>
<br />  
    <p>
    
<img src="https://i.imgur.com/RfpJvhU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/FaNfk6r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ULZuBzh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/QmfBNQX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ZbKjLHk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/xkLryJO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/McUm7bI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Oyxk6Nz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Step 4. Join Client-1 to your domain (mydomain.com)

4.a From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

4.b From the Azure Portal, restart Client-1

4.c Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)

</p>
<br />
<img src="https://i.imgur.com/1wdmcA8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/FHIa3Je.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>    
<img src="https://i.imgur.com/KusrKcl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/umM96eP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


</p>
<p>
Step 5. Setup Remote Desktop for non-administrative users on Client-1
    
5.a Log into Client-1 as mydomain.com\jane_admin and open system properties

5.b Click “Remote Desktop”

5.c Allow “domain users” access to remote desktop

5.d You can now log into Client-1 as a normal, non-administrative user now

</p>
<br />
    
<img src="https://i.imgur.com/Oyxk6Nz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>    
<img src="https://i.imgur.com/DNjGbSM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/275hoZL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/BdA5ibU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>    
<img src="https://i.imgur.com/KvZA7ea.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 6. Create a bunch of additional users and attempt to log into client-1 with one of the users
    
6.a Login to DC-1 as jane_admin

6.b Open PowerShell_ise as an administrator

6.c Create a new File and paste the contents of the script into it
    
(https://github.com/ShalimRazzak/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

6.d Run the script and observe the accounts being created

6.e When finished, open ADUC and observe the accounts in the appropriate OU

6.f attempt to log into Client-1 with one of the accounts (take note of the password in the script)

<p>
<img src="https://i.imgur.com/BYHJOH3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/pjz7lPR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/uVtA3bu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/eMPa88q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/5IqFs2r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<br />
