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

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 1. Setup Resources in Azure
    
1.a Create the Domain Controller VM (Windows Server 2022) named “DC-1”
    
        1.1 Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
    
1.b Set Domain Controller’s NIC Private IP address to be static
    
1.c Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.1
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 2. Ensure Connectivity between the client and Domain Controller
    
2.a Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)

2.b Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall

2.c Check back at Client-1 to see the ping succeed
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 3. Install Active Directory
    
3.a Login to DC-1 and install Active Directory Domain Services

3.b Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)

3.c Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 4. Create an Admin and Normal User Account in AD
    
4.a In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

4.b Create a new OU named “_ADMINS”

4.c Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

4.d Add jane_admin to the “Domain Admins” Security Group

4.e Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”

4.f User jane_admin as your admin account from now on
</p>
<br />
    
    <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 5. Join Client-1 to your domain (mydomain.com)

5.a From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

5.b From the Azure Portal, restart Client-1

5.c Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)

5.d Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

5.e Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br />
    
    <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 6. Setup Remote Desktop for non-administrative users on Client-1
    
6.a Log into Client-1 as mydomain.com\jane_admin and open system properties

6.b Click “Remote Desktop”

6.c Allow “domain users” access to remote desktop

6.d You can now log into Client-1 as a normal, non-administrative user now

6.e Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)
</p>
<br />
    
    <p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 7. Create a bunch of additional users and attempt to log into client-1 with one of the users
    
7.a Login to DC-1 as jane_admin

7.b Open PowerShell_ise as an administrator

7.c Create a new File and paste the contents of the script into it
    
    # ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
# ------------------------------------------------------ #

Function generate-random-name() {{
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}}
    
(https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

7.d Run the script and observe the accounts being created

7.e When finished, open ADUC and observe the accounts in the appropriate OU

7.f attempt to log into Client-1 with one of the accounts (take note of the password in the script)

</p>
<br />
