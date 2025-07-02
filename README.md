<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket Installation on Azure Windows 10 VM

This is a walkthrough on installing **osTicket**, a popular open-source help desk ticketing system, ideally on a **Windows 10 virtual machine using Microsoft Azure Services**. (This guide may be harder to follow on Windows 11 unless you're familiar with its new interface.) This project demonstrates skills in cloud infrastructure, Windows server configuration, and web-based application deployment—relevant for Help Desk and Cloud-related roles.

---

## Video Demonstration

- [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)

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
     https://www.iis.net/downloads/community/2018/05/php-manager-150-for-iis-10  
     _Acts like a BIOS interface for PHP—makes enabling/disabling extensions simple._

   - **URL Rewrite Module 2.1 (x64)**  
     https://www.iis.net/downloads/microsoft/url-rewrite  
     _Handles clean URLs—essential for page routing in osTicket._

   - **PHP 7.3.8 Non-Thread Safe (VC15, x86)**  
     https://windows.php.net/downloads/releases/php-7.3.8-nts-Win32-VC15-x86.zip  
     _The core language osTicket runs on—NTS version is ideal for IIS._

   - **MySQL Community Server 5.5.62 (Windows, 32‑bit MSI)**  
     https://cdn.mysql.com//Downloads/MySQL-5.5/mysql-5.5.62-win32.msi  
     _Database engine that stores tickets, users, logs, and settings._

   - **Microsoft Visual C++ Redistributable 2015 (x86)**  
     https://www.microsoft.com/en-us/download/details.aspx?id=48145  
     _Runtime components required by both PHP and MySQL._

   - **osTicket v1.15.8**  
     https://osticket.com/osticket-v1-15-8-v1-16-3-available/  
     _The main help desk web application._

   - **HeidiSQL (Latest Stable Installer)**  
     https://www.heidisql.com/download.php  
     _A GUI-based MySQL database manager._

---

## Step-by-Step Setup Guide

### 1. Create an Azure Virtual Machine

- OS: Windows 10
- Size: 4 vCPUs (e.g., B2ms)
- Name: `osticket-vm`
- Username: `labuser`
- Password: `osTicketPassword1!`
- Open ports: 3389 (RDP) and 80 (HTTP)

---

### 2. Connect to the VM

- Use Remote Desktop to log in using your Azure VM credentials.

---

### 3. Enable IIS with CGI Support

- Go to "Turn Windows features on or off"
- Enable:
  - Internet Information Services (IIS)
  - World Wide Web Services → Application Development Features → CGI

---

### 4. Install Prerequisites

From the `osTicket-Installation-Files` folder, run:

- PHP Manager for IIS (`PHPManagerForIIS_V1.5.0.msi`)
- URL Rewrite Module (`rewrite_amd64_en-US.msi`)
- VC++ Redistributable (`VC_redist.x86.exe`)

---

### 5. Set Up PHP

1. Create a folder: `C:\PHP`
2. Extract `php-7.3.8-nts-Win32-VC15-x86.zip` into `C:\PHP`
3. Open IIS Manager (as Administrator)
4. Use PHP Manager to register PHP:
   - Path: `C:\PHP\php-cgi.exe`
5. Restart IIS (Stop/Start the server)

---

### 6. Install MySQL

1. Run `mysql-5.5.62-win32.msi`
2. Choose Typical Setup
3. After install, run Configuration Wizard
4. Use Standard Configuration
5. Credentials:
   - Username: `root`
   - Password: `root`

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

---

## Post-Install Cleanup

1. Delete folder: `C:\inetpub\wwwroot\osTicket\setup`
2. Change permissions of `ost-config.php` to Read-only

---

## Access Links

- Admin Portal: `http://localhost/osTicket/scp/login.php`
- User Portal: `http://localhost/osTicket/`

---

## Why This Project Matters

- Demonstrates deployment of full-stack web apps
- Shows ability to configure virtual machines and services in Azure
- Familiarizes you with IIS, PHP, and MySQL
- Aligns with IT Support, Help Desk, SysAdmin, and Cloud Support roles
