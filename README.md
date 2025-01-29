
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>


- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (myadproject.com)
- Setup Remote Desktop for non-administrative users on Client-1
<h2>Deployment and Configuration Steps</h2>

Active Directory 

Create Resource Group
 ![Screenshot 2025-01-28 160347](https://github.com/user-attachments/assets/feaa24d2-c679-4496-856f-b7b81b42bb27)



In Azure 
Create Virtual Network 
![Screenshot 2025-01-28 160840](https://github.com/user-attachments/assets/5c635b6a-f626-4f45-a12b-660e458bc66e)

In Azure
Create two Virtual Machines (one server = dc-1 and one user= client-1 )
![Screenshot 2025-01-28 161804](https://github.com/user-attachments/assets/0f41f2df-d8a1-422a-b9ac-2c7fbe6794d9)
![Screenshot 2025-01-28 163322](https://github.com/user-attachments/assets/f1991979-31be-4107-a849-397dad8cab2e)
 
In Azure 
Turn dc-1 private ip address to static 
 - Dc-1 Network setting
![Screenshot 2025-01-28 165040](https://github.com/user-attachments/assets/16163ed6-f90c-447c-946f-4065bbffd0c2)

 
After creating VM, set Client-1 dns settings to dc-1 private IP address
 - Copy and paste dc-1 private address into client-1 dns server column 
  ![Screenshot 2025-01-28 220758](https://github.com/user-attachments/assets/5451e2e1-7f5f-4df9-a8dc-aae592c56cda)


Turn of firewall for dc-1 server
 - In windows defender firewall and advanced security
   ![Screenshot 2025-01-28 223900](https://github.com/user-attachments/assets/50492e1e-c798-4fe5-baf8-1187925f9ddf)

Log into client-1 using Remote Desktop 
 ![Screenshot 2025-01-28 221530](https://github.com/user-attachments/assets/840f29dd-fe69-4fe4-acc8-084cbb3b5409)
 



Attempt to ping dc-1 ip address 
 - Using powershell enter ping plus dc-1 private ip address
    ![Screenshot 2025-01-28 223810](https://github.com/user-attachments/assets/c3e0202f-02b2-475f-96ac-489f27c4b85a)

   
In client-1 powershell, Run ipconfig /all to show dc-1 private ip address as dns setting 

 ![Screenshot 2025-01-28 224327](https://github.com/user-attachments/assets/36d2ebb2-550d-4229-ab16-3cddeb39618c)



Log in to dc-1 and install Active Directory domain services  
  - Sever manager, dashboard, add roles and features, server roles, select active directory domain services, add features, install 
Promote dc-1 server to domain controller

  ![Screenshot 2025-01-28 230959](https://github.com/user-attachments/assets/1495974e-cd93-48b7-81b0-eac314d6c2c1)
  ![Screenshot 2025-01-28 231109](https://github.com/user-attachments/assets/59c1d187-6ef5-48cf-80e8-a25b3087d139)
  ![Screenshot 2025-01-28 231215](https://github.com/user-attachments/assets/c6cf86ce-b83c-4d94-9e17-7efdb1e53d50)


Clink the flag icon in server manager dashboard and select promote this server as a domain

  ![Screenshot 2025-01-28 233510](https://github.com/user-attachments/assets/8440baac-e1c4-47b3-9ce3-b773baf97a20)

Add a new forest with a root domain name 
  ![Screenshot 2025-01-28 233659](https://github.com/user-attachments/assets/f4f8a899-c09d-4b03-acf5-6a6d063d49b6)

 
next until install 

  ![Screenshot 2025-01-28 233924](https://github.com/user-attachments/assets/4f139ed3-54cb-45e6-84fc-7679f5e24889)

Log in as domain user and create a domain admin user within the domain 
 - In Active Directory, create an organizational unit (OU) called  _EMPLOYEES and another called _ADMINS

 ![Screenshot 2025-01-28 235641](https://github.com/user-attachments/assets/6a19d6c7-5acd-4ebf-bb91-2644dcaa8558) 
 ![Screenshot 2025-01-28 235826](https://github.com/user-attachments/assets/052e59b0-1acc-45a3-b582-23c86c28e01e)

 Create a new employee “Jane Doe”

  ![Screenshot 2025-01-29 000808](https://github.com/user-attachments/assets/156ea16a-017c-4e6a-bbaa-3cc408851c06)


In admin folder, right click, new, user 
Add Jane Doe to domain admins

  ![Screenshot 2025-01-29 001346](https://github.com/user-attachments/assets/7097ab3e-bb8e-4427-8714-f75b06550848)


Right click Jane doe, properties, member of, type domain admin and ok, apply and ok.

 ![Screenshot 2025-01-29 002422](https://github.com/user-attachments/assets/30557b5f-c137-4732-8cbb-faf9c8fb4047)
 ![Screenshot 2025-01-29 002630](https://github.com/user-attachments/assets/29fc40de-c1ed-4886-9bda-03ab35b3bb01)

 
Log in as mydomain.com/jane_admin
 - Back in Client-1 join client-1 to the domain 
 - Right click start menu, system, rename this PC(advanced), change, select domain. 
 - Allow domain users to access Remote Desktop

   ![Screenshot 2025-01-29 103328](https://github.com/user-attachments/assets/687cfdd3-0843-4d4d-b14a-d6b1bc8686fd)


You can now log in as none administrative user.

Also you can: Open system properties, remote desktop, under user account select users that can remotely access this PC, add other domain users.
And with that non administrative users added can access Remote Desktop!!!! 

Thanks for viewing! I hope you enjoyed it as much as I did!
