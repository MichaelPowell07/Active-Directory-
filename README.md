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

- Preparing the AD Infrastructure in Azure 
- Deploying Active Directory
- Creating users with Powershell
- Group Policy and Managing Accounts

<h2>Deployment and Configuration Steps</h2>

Part 1: Preparing the AD infrastructure in Azure

Setup Domain controller in Azure 

1. Create a Resource Group
   -Navigate to the Azure Portal and create a new resource group for the lab enviornment.

   ![image](https://github.com/user-attachments/assets/e5bc6827-9528-4b1b-8724-cc46f6a8b4d2)

2. Create a Virtual Network and Subnet
   - Set up a Virtual Network with a subnet to host your VMs

![image](https://github.com/user-attachments/assets/19dc6913-b7ae-43eb-a5f4-8e470e0d651f)

3. Create the Domain Controller VM (Windows Server 2022)
   - Name the VM : DC-1
   - Ensure that the VM is on the Virtual Network created previously
  
     ![image](https://github.com/user-attachments/assets/3b690211-245a-492f-967a-ec9d62370f69)

     ![image](https://github.com/user-attachments/assets/aa2b7b0b-952e-4f65-a6d5-7d976372cbe7)

     ![image](https://github.com/user-attachments/assets/fdb390fd-4662-4249-8c52-9cf19c9bb339)

     4. Setup Client-1 VM in Azure
      1.)  - Create the client VM (Windows 10 22H2)
        - Name the VM:Client-1

          ![image](https://github.com/user-attachments/assets/7fddd15a-7d97-43e1-bd6d-2d37a05b3411)

          ![image](https://github.com/user-attachments/assets/e84ad204-4bf5-431c-b6cd-63a424e999bf)

          ![image](https://github.com/user-attachments/assets/52eecf47-7a92-40ac-897a-bef1db35a7f5)

         2.) Atatch the Client-1 to the same region and virtual network and subnet as DC-1

        ![image](https://github.com/user-attachments/assets/6454e18c-ec68-4f9b-9cdb-1522c264fdd4)

        5. Set static private IP address for DC-1
           - After the Vm is created, navigate to it's network interface card (NIC) settings and set the private IP address to static.
          
             ![image](https://github.com/user-attachments/assets/023bf8f0-2a39-4cce-a6f7-bfa0fd38961c)

       6. Disable Windows Firewall
          - Login to DC-1 and disable the windows firewall for testing connectivivty.

            ![image](https://github.com/user-attachments/assets/0c25fa1d-4fba-47f2-ae93-cbdcca20e403)

       7. Set DNS settings
           - Update client-1 DNS settings to point to DC-1 private IP address.

            ![image](https://github.com/user-attachments/assets/6a19af74-2d19-45f6-898f-ec0a6329a195)

      8. Test connectivity
         - Restart Client-1 from the Azure portal
         - log into Client-1 and use the ping command to test connectivity with DC-1

           ![image](https://github.com/user-attachments/assets/74c28aef-0754-42af-b6a8-7ee3d5249506)

    9. Verify DNS settings
        - Run IPconfig/all in PowerShell on client-1 to ensure DNS points to DC-1

          ![image](https://github.com/user-attachments/assets/202fb051-c5b7-49f5-83b9-ae789feca595)

      Part 2: Deploying Active Directory

Install Active Directory

1.) Log into DC-1
2.) Install Active Directory Domain Services (AD DS) 
3.) Promote DC-1 as a Domain Controller and setup a new forest (e.g., mydomain.com) 
4.) Restart DC-1 and login as mydomain.com/labuser.

![image](https://github.com/user-attachments/assets/bb690c74-3c9c-4736-ad46-9ce971916f02)

![image](https://github.com/user-attachments/assets/e475e289-1256-404f-8a7e-e0490ac42e11)

Create a Domain Admin User

1.) Open Active Directory Users and Computers (ACDUC)
2.) Create an Organizational Unit (OU) named _EMPLOYEES
3.) Create anither OU named _ADMINS
4.) Add new user 
- Name: Jane Doe
- Username: jane_admin
- Password: Cyberlab123!
5.) Add jane_admin to the Domanin Admins security group
6.) Log out and log back in as mydomain.com/jane_admin

![image](https://github.com/user-attachments/assets/7c052a07-5d4c-41af-b94e-f1cfd778a93a)

![image](https://github.com/user-attachments/assets/374934dd-02ee-4dbd-a933-f599886e6682)

![image](https://github.com/user-attachments/assets/61821faa-c73c-4be4-8983-1a001c0947cb)

Join Client-1 to the Domain 

1.) Log in as the local admin and join Client-1 to the Domain.
2.) Create a new OU titled _CLIENTS and add Client-1 in ADUC to _CLIENTS

![image](https://github.com/user-attachments/assets/ae5ecef0-fa8e-4664-973d-dde9e40707b6)

![image](https://github.com/user-attachments/assets/e3921af1-a911-45b4-bc6d-b5722eb59aea)

Part 3: Creating Users with PowerShell

Setup Remote Desktop for Domain Users

1.) Log into Client-1 as mydomain.com/jane_admin.
2.) Open Systems Properties and enable Remote Desktop.
3.) Allow "domain users" access to remote desktop.

![image](https://github.com/user-attachments/assets/078ffc41-dceb-4148-8537-540faccc6758)

Create Users with PowerShell 

1.) Log in to DC-1 as jane_admin.
2.) Open PowerShell ISE as an administrator.
3.) Create multiple new users using a script 
4.) Verify users appear in the _EMPLOYEES OU in ADUC.
5.) Attempt to log into Client-1 with one of the created accounts.

![image](https://github.com/user-attachments/assets/4c0897fe-0f81-4c2c-8247-b3f9c73e2a91)

![image](https://github.com/user-attachments/assets/55d746c6-59a5-44c0-a107-6d8f2a2c1597)

![image](https://github.com/user-attachments/assets/61e670d6-34f2-4996-8d67-7696ec1048b1)

Part 4: Group Policy and Managing Accounts

Account Lockout Configurations

1.) Log into DC-1.
2.) Open Group Policy Management.
3.) Edit the Default Domain Policy
- set account lockout thresehold to 5 invalid attempts
4.) Attempt to login with a user account using incorrect passwords observe the account lockout behavior.
5.) Unlock the account in ADUC and reset the password.

  ![image](https://github.com/user-attachments/assets/4544a000-2236-43b1-8f40-f27167f52f5b)

  ![image](https://github.com/user-attachments/assets/a0753b50-f83d-4ecd-bd83-40532648f0da)

  ![image](https://github.com/user-attachments/assets/258bcd83-c11d-4f47-a52e-6c3f16d05a16)

  ![image](https://github.com/user-attachments/assets/33ecd6b8-8748-4dc0-9d02-97b553c71de3)

  Enable and Disable Accounts

  1.) Disable a user account in ADUC.
  2.) Attempt to login with the disabled account and observe the error message.
  3.) Re-enable the account and login successfully.

  ![image](https://github.com/user-attachments/assets/424903b6-7360-4274-a5de-1d4bd87031bc)

  ![image](https://github.com/user-attachments/assets/e1437db9-5b35-4fe2-af72-93a6e4488336)

  ![image](https://github.com/user-attachments/assets/a82bb35a-2f0c-42f9-ae57-69096a98119e)

  Observing logs
  1.) Review authentication and account-related logs in event viewer
  - Log on DC-1 for domain-level events
  - Log on Client-1 for local events

    ![image](https://github.com/user-attachments/assets/d22ec0ba-9c62-482d-9717-445a13c2b206)

    Completion

    Congrats! You have successfully deployed and configured an on-premises Active Directory enviornment in Azure.

























          




