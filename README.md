<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket Installation on Azure Windows 10 VM

This is a walkthrough on installing **osTicket**, a popular open-source help desk ticketing system, ideally on a **Windows 10 virtual machine using Microsoft Azure Services**. (This guide may be harder to follow on Windows 11 unless you're familiar with its new interface.) This project demonstrates skills in cloud infrastructure, Windows server configuration, and web-based application deployment—relevant for Help Desk and Cloud-related roles.

---

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Protocol
- Internet Information Services (IIS)

---

## Operating Systems Used

- Windows 10 (21H2)

---

## List of Prerequisites

1. Microsoft Azure Account (Recommended: Pay-as-you-go plan)
2. Azure Virtual Machine (Windows 10, 4 vCPUs)
3. Azure Virtual Machine Details:
   - Name: `osticket-vm`
   - Username: `ticketuser`
   - Password: `osTicket`
4. Remote Desktop Client
5. osTicket Requirements:
   
   - **PHP Manager for IIS (v1.5.0)**  
   - **URL Rewrite Module 2.1 (x64)**  
   - **PHP 7.3.8 Non-Thread Safe (VC15, x86)**  
   - **Microsoft Visual C++ Redistributable 2015 (x86)**  
   - **MySQL Community Server 5.5.62 (Windows, 32‑bit MSI)**  
   - **osTicket v1.15.8**  
   - **HeidiSQL (Latest Stable Installer)**  

---

## Step-by-Step Setup Guide

### 1. Create an Azure Virtual Machine

- Subscription: I recommend pay-as-you-go or the monthly plan (Make sure you don't run the VM's 24/7 or else the cost will skyrocket)
- Resource Group: Create a new Resource group called "osTicket" (We're basically isolating our work in sections particularly a resource group)
- Virtual Machine Name: Name it as "osticket-vm"
- Region: I choose East US 2 (That totally depends where you are in the world pick ones that are the closest to your region obviously)
- Image: Windows 10 Pro, version 22H2 - x64 Gen2 (Please don't do Windows 11 since the tutorial mainly focuses on Windows 10)
- Size: Standard_D4s_v3 - 4 vcpus, 16 GiB memory ($140.16/month) ($70.08/month) (The monthly fee only charges you if you run it 24/7 as long as you turned it off after each use you're fine. Unless if you're working with corporations)

  
- Username: labuser
- Password: osTicketPassword1!
- Make sure to check the box for Licensing at the very bottom of the first section of virtual machine creation.


Now onto the next section skip the "Disk" of Virtual Machine creation goto "Networking" section 

-Create a new Virtual Network called "osticket-vm-vnet" it should automatically make it for you similarly for Public IP.

https://github.com/user-attachments/assets/43231d35-ea6c-4367-9a1e-463fdc2e57a9

Finally review and create the Virtual Machine!!!

---

### 2. Log in to Connect to the VM
- You need to check the notification section to check if it's actually deployed or not. (Sometimes it will keep telling you that the deployment is in progress even thou it was already deployed)


- Use Remote Desktop to log in using your Azure VM credentials.
- Navigate your way towards the Azure Dashboard -> Virtual Machine -> Selecting osticket-vm -> Select Connect



- Then download the RDP client file to connect to your Virtual machine
  

- Select the downloaded RDP client file from the download section of your browser
  

- You can select "Don't ask me again for connection to this computer" then connect and log in to your username and password
  

- Then select both the "Don't ask me again for connection to this computer" and "Yes"
- Then proceed to interact and navigate as you're starting out your new Virtual Machine

  

https://github.com/user-attachments/assets/864de83b-5fd4-4813-ad53-274c42427396


---

### 3. Enable IIS with CGI Support within your Virtual Machine

- Navigate towards the Control Panel and Select Programs


- Then select "Turn Windows features on or off"


- Scroll Through "Internet Information Services (IIS)"
- Enable:
  - Internet Information Services (IIS)
  - World Wide Web Services → Application Development Features → CGI



https://github.com/user-attachments/assets/d0fb6be3-b319-44de-b13d-97291f206b2d


---

### 4. Install the following Prerequisites in proper order within your Virtual Machine.

 - **PHP Manager for IIS (v1.5.0)**  
     [https://github.com/RonaldCarter/PHPManager/releases/tag/V1.5.0](https://github.com/RonaldCarter/PHPManager/releases/tag/V1.5.0)
   
     _Think about it as a BIOS interface for PHP where it let's you toggle the settings instead of scouring through the raw config files and manually editing them each which is a big hassle._
  
 - **URL Rewrite Module 2.1 (x64)**  
     [https://www.iis.net/downloads/microsoft/url-rewrite  ](https://www.iis.net/downloads/microsoft/url-rewrite  )
     _This is more like a GPS routing system for the website it will literally identify cleanly map URL's._

    Similarly with visual studio just follow through the prompts

 - **Microsoft Visual C++ Redistributable 2015 (x86)**  
     https://www.microsoft.com/en-us/download/details.aspx?id=48145  
     _This is a device driver for PHP and MySQL that will make everything run well together._

   

https://github.com/user-attachments/assets/a8469a73-b9ed-467b-ac72-791a8260f6ba



### 5. Install MySQL

- **MySQL Community Server 5.5.62 (Windows, 32‑bit MSI)**  
     https://cdn.mysql.com//Downloads/MySQL-5.5/mysql-5.5.62-win32.msi  
     _It's basically the storage and filing system where it stores all the tickets, users, logs, and settings._

1. Run the file that you just downloaded `mysql-5.5.62-win32.msi`
2. Choose Typical Setup
3. After install, run Configuration Wizard
4. Use Standard Configuration then Install as Window service
5. Credentials: (Reason we have similar user and pass is to avoid errors)
   - Username: `root`
   - Password: `root`
6. Then Execute
   

Uploading 2025-07-07 09-38-11.mp4…


---

### 6. Set Up PHP

1. Create a directory in C:\PHP in your virtual machine

   
2. Extract or copy and paste `php-7.3.8-nts-Win32-VC15-x86.zip` into `C:\PHP`

   - **PHP 7.3.8 Non-Thread Safe (VC15, x86)**  
     https://windows.php.net/downloads/releases/archives/php-7.3.8-nts-Win32-VC15-x64.zip
     _If osTicket is the software we're using then PHP is the language that runs the software._

 - After downloading, since the files are not compressed you can just copy and paste the contents of "PHP 7.3.8" into your "PHP" folder in your C: drive.


3. Open IIS Manager from your windows search bar by typing "IIS" (as Administrator)

4. Select PHP Manager


5. Use PHP Manager to register PHP by browsing the file "php-cgi.exe" the Path: `C:\PHP\php-cgi.exe`


6. Restart IIS once it is confirmed your php-cgi.exe is detected (Stop/Start the server)



https://github.com/user-attachments/assets/506ccc03-51ea-4e65-9b14-2ce0dcdf316c



---

### 7. Deploy osTicket Files

- **osTicket v1.15.8**  
     https://github.com/osTicket/osTicket/releases/tag/v1.15.8
     _The main software help desk web application that connects everything that you've installed._
  
1. Download and Extract `osTicket-v1.15.8.zip`
2. Copy the `upload` folder to `C:\inetpub\wwwroot` 
3. Rename `upload` to `osTicket` (Spelling is really important make sure it is exactly named similarly)
4. Restart IIS


https://github.com/user-attachments/assets/75304be1-1c77-45a5-92ac-724616513475


---

### 8. Open osTicket in Browser

1. In IIS Manager:
   - Navigate to Sites > Default Web Site > osTicket

   - Click "Browse *:80"


2. This opens osTicket at: `http://localhost/osTicket`


https://github.com/user-attachments/assets/dda0c04e-23a7-4749-839f-e30d6c0baa06


---

### 9. Enable PHP Extensions

1. In IIS, go to osTicket > PHP Manager
2. Click "Enable or disable an extension"
3. Enable: (Use the filter search bar to find them easily)
   - `php_imap.dll`


   - `php_intl.dll`

     
   - `php_opcache.dll`


3. Refresh osTicket in your browser (You should notice that some requirements have been checked)



Uploading 2025-07-07 09-51-27.mp4…



---

### 10. Rename and Secure Config File

1. Rename: (Fastest way to search is to use the top right search bar while you selected C:\ drive)
   - From: `C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php`
   - To: `C:\inetpub\wwwroot\osTicket\include\ost-config.php`
2. Set permissions:
3. Right click the file `ost-config.php` then select properties.
4. Then "Security" and select "Advance"
5. Disable inheritance, remove all users
6. Select Remove all
7. Select "Add" then on the top left select "Select a principal" then type within the box "everyone"
8. After adding `Everyone` check the box that says "Full Control"
- Then select "Apply" and "Ok"
9. You should be able to continue onto the osTicket browser and put in your credentials
10. Start filling out your information


https://github.com/user-attachments/assets/0b0fd8fb-160c-4178-a6bf-31922acc527b

    
---

### 11. Set Up the Database

  - **HeidiSQL (Latest Stable Installer)**  
     https://www.npackd.org/p/heidisql/12.3.0.6589  
     _Instead of using command-like SQL, it gives you a GUI to create databases and manage data it's basically similar to file explorer, but for MySQL._

    
1. Install HeidiSQL and after that we're making a setup for our osTicket Database
2. Open and create a new session: (There's a chance that even if you input the right USERNAME and PASSWORD that the virtual machine needs to be restarted since that database is not really established yet. Worse thing yet, you might need to redo the entire virtual machine process if you can't narrow down the problem. Especially if it's not PERMISSION related or ACCOUNT NAME related or PREREQUISITES related. You have no choice if it doesn't come from those I mentioned)
   - Username: `root`
   - Password: `root`
3. Connect and create a new database called: `osTicket`


https://github.com/user-attachments/assets/5b3436d7-40aa-4333-abad-bd3dbd426908


---

### 12. Complete osTicket Setup in Browser

1. Go to `http://localhost/osTicket`
2. Fill in:
   - Helpdesk name
   - Admin email and credentials
3. Database details:
   - DB Name: `osTicket`
   - DB User: `root`
   - DB Password: `root`
4. Click "Install Now" and make sure that the osTicket database actually pops out of the left side panel inside the osTicket
5. If you did everything properly or redo it. You should be golden and ready to go.
6. Log in URL as an admin for osTicket:
  
http://localhost/osTicket/scp/login.php
   
7. Log in for end users for osTicket:


http://localhost/osTicket/ 


https://github.com/user-attachments/assets/63954a09-6dce-46ed-ac16-f87d549a05c6



8. Clean up
Delete: C:\inetpub\wwwroot\osTicket\setup
Set Permissions to “Read” only: C:\inetpub\wwwroot\osTicket\include\ost-config.php

##You're done with the set up!!!##
