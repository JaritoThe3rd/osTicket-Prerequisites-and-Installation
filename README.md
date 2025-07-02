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
- Size: Standard_D2s_v3 - 2 vcpus, 8 GiB memory ($70.08/month) (The monthly fee only charges you if you run it 24/7 as long as you turned it off after each use you're fine. Unless if you're working with corporations)

  
- Username: labuser
- Password: osTicketPassword1!
- Make sure to check the box for Licensing at the very bottom of the first section of virtual machine creation.
  
  
![{47451417-C967-4242-AD56-A041AA736D31}](https://github.com/user-attachments/assets/4cc3def8-92be-4770-ae78-a7e3ecac6193)


![{88030348-784F-40BE-8F33-CDBC6AD5C14F}](https://github.com/user-attachments/assets/67574a01-0517-4c97-9d6d-6f9e2eb6f37c)


Now onto the next section skip the "Disk" of Virtual Machine creation goto "Networking" section 

-Create a new Virtual Network called "osticket-vm-vnet" it should automatically make it for you similarly for Public IP.


![{B1691694-33F8-4B1F-9B3D-B77213E20AF2}](https://github.com/user-attachments/assets/b54d1079-afcd-415e-acf6-75b4122503b2)


Finally review and create the Virtual Machine!!!

---

### 2. Log in to Connect to the VM
- You need to check the notification section to check if it's actually deployed or not. (Sometimes it will keep telling you that the deployment is in progress even thou it was already deployed)


  ![{CB9D9916-92B7-489D-AC06-60929E434CC4}](https://github.com/user-attachments/assets/191ec13b-2953-4906-8f36-86097206a5fe)


- Use Remote Desktop to log in using your Azure VM credentials.
- Navigate your way towards the Azure Dashboard -> Virtual Machine -> Selecting osticket-vm -> Select Connect

  
![{7EFED89A-A98D-4595-B9AB-B52133FEABE5}](https://github.com/user-attachments/assets/3aca1827-7c31-4292-a58b-8b4a814eecae)


- Then download the RDP client file to connect to your Virtual machine


  ![{2E1BA7B0-39D3-45CE-9D34-97458295025F}](https://github.com/user-attachments/assets/cc092821-de8f-49ee-8a97-06c712d1ab64)
  

- Select the downloaded RDP client file from the download section of your browser
  
  
  ![{435A76CA-6CD4-41D3-AC52-C8C9AB10F94E}](https://github.com/user-attachments/assets/8f84a109-1b23-4faa-b7c5-00c79830ea5b)
  

- You can select "Don't ask me again for connection to this computer" then connect and log in to your username and password
  

  ![{573B5319-0F9B-4C72-ABB7-30F0985852CE}](https://github.com/user-attachments/assets/0e953fa4-10a4-4dfe-8fb9-7159ad65cd40)

  
  ![{4C9AAE25-A6AA-47AE-AC25-604294503557}](https://github.com/user-attachments/assets/49e13879-c534-4f06-93df-ba5704958d52)
  

- Then select both the "Don't ask me again for connection to this computer" and "Yes"
- Then proceed to interact and navigate as you're starting out your new Virtual Machine

---

### 3. Enable IIS with CGI Support within your Virtual Machine

- Navigate towards the Control Panel and Select Programs

![image](https://github.com/user-attachments/assets/cb668e09-6d85-4a7d-80cd-ec0a35400e5f)


- Then select "Turn Windows features on or off"


  ![{6AF31233-70B6-42BB-9448-478B447B2334}](https://github.com/user-attachments/assets/ddc81e83-9de4-4b85-9465-b49f0831475c)


- Scroll Through "Internet Information Services (IIS)"
- Enable:
  - Internet Information Services (IIS)
  - World Wide Web Services → Application Development Features → CGI

    
![image](https://github.com/user-attachments/assets/4613881d-469b-4efb-b118-d7265b74bc0c)


![{B2D786BA-A7BF-41A0-9DEA-54F269BEB94F}](https://github.com/user-attachments/assets/090ce3b8-4011-442a-9fa9-30f414490d75)


---

### 4. Install the following Prerequisites in proper order within your Virtual Machine.

- **PHP Manager for IIS (v1.5.0)**  
     https://github.com/RonaldCarter/PHPManager/releases/tag/V1.5.0
     _Think about it as a BIOS interface for PHP where it let's you toggle the settings instead of scouring through the raw config files and manually editing them each which is a big hassle._
  
 - **URL Rewrite Module 2.1 (x64)**  
     https://www.iis.net/downloads/microsoft/url-rewrite  
     _This is more like a GPS routing system for the website it will literally identify cleanly map URL's._

 - Create a directory in C:\PHP of your virtual machine


   ![{778D91C9-C03A-4B88-A1FE-E5E7C1B7D5FC}](https://github.com/user-attachments/assets/3576a598-40df-477e-a6d3-b786d113cf3b)


### 5. Set Up PHP

1. Create a folder: `C:\PHP`
2. Download and extract the file to your "C:\PHP" virtual machine folder

   - **PHP 7.3.8 Non-Thread Safe (VC15, x86)**  
     https://windows.php.net/downloads/releases/archives/php-7.3.8-nts-Win32-VC15-x64.zip
     _If osTicket is the software we're using then PHP is the language that runs the software._

 - After downloading, since the files are not compressed you can just copy and paste the contents of "PHP 7.3.8" into your "PHP" folder in your C: drive.


   ![image](https://github.com/user-attachments/assets/8bb1f723-37a1-4972-ae31-7ed71c512a9a)


3. Extract or copy and paste `php-7.3.8-nts-Win32-VC15-x86.zip` into `C:\PHP`
4. Open IIS Manager from your windows search bar by typing "IIS" (as Administrator)

5. Select PHP Manager


![{79F09B67-629A-4793-90A2-9DA120DF4C22}](https://github.com/user-attachments/assets/9da57d71-df45-4f3f-995c-37bd80d7d7d5)


5. Use PHP Manager to register PHP by browsing the file "php-cgi.exe":

![image](https://github.com/user-attachments/assets/956cc205-b29b-4922-8cc4-9d0dc6948bef)


   - Path: `C:\PHP\php-cgi.exe`


7. Restart IIS (Stop/Start the server)


   ![image](https://github.com/user-attachments/assets/42f0b863-a582-4563-8f1e-9cbc4e8404fa)


   ![image](https://github.com/user-attachments/assets/995d6c51-ae55-476b-9bf6-470f5b56b37d)

- Similarly with visual studio just follow through the prompts

   - **Microsoft Visual C++ Redistributable 2015 (x86)**  
     https://www.microsoft.com/en-us/download/details.aspx?id=48145  
     _This is a device driver for PHP and MySQL that will make everything run well together._

---

### 6. Install MySQL

1. Run the file that you just downloaded `mysql-5.5.62-win32.msi`
2. Choose Typical Setup
3. After install, run Configuration Wizard
4. Use Standard Configuration then Install as Window service
5. Credentials: (Reason we have similar user and pass is to avoid errors)
   - Username: `tool`
   - Password: `tool`
6. Then Execute

- **MySQL Community Server 5.5.62 (Windows, 32‑bit MSI)**  
     https://cdn.mysql.com//Downloads/MySQL-5.5/mysql-5.5.62-win32.msi  
     _It's basically the storage and filing system where it stores all the tickets, users, logs, and settings._

---

### 7. Deploy osTicket Files

1. Extract `osTicket-v1.15.8.zip`
2. Copy the `upload` folder to `C:\inetpub\wwwroot`
3. Rename `upload` to `osTicket`
4. Restart IIS

---

### 8. Open osTicket in Browser

1. In IIS Manager:
   - Navigate to Sites > Default Web Site > osTicket
   - Click "Browse *:80"
2. This opens osTicket at: `http://localhost/osTicket`

---

### 9. Enable PHP Extensions

1. In IIS, go to osTicket > PHP Manager
2. Click "Enable or disable an extension"
3. Enable:
   - `php_imap.dll`
   - `php_intl.dll`
   - `php_opcache.dll`
4. Refresh osTicket in your browser

     - **osTicket v1.15.8**  
     https://osticket.com/osticket-v1-15-8-v1-16-3-available/  
     _The main software help desk web application that connects everything that you've installed._


---

### 10. Rename and Secure Config File

1. Rename:
   - From: `C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php`
   - To: `C:\inetpub\wwwroot\osTicket\include\ost-config.php`
2. Set permissions:
   - Disable inheritance, remove all users
   - Add `Everyone` with Full Control

---

### 11. Set Up the Database

1. Install HeidiSQL
2. Open and create a new session:
   - Username: `root`
   - Password: `root`
3. Connect and create a new database called: `osTicket`


   - **HeidiSQL (Latest Stable Installer)**  
     https://www.heidisql.com/download.php  
     _Instead of using command-like SQL, it gives you a GUI to create databases and manage data it's basically similar to file explorer, but for MySQL._


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
4. Click "Install Now"


