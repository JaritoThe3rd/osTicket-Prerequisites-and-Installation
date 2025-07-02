<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# 🎫 osTicket Installation on Azure Windows 10 VM

This is a walkthrough on installing **osTicket**, a popular open-source help desk ticketing system, ideally on a **Windows 10 virtual machine using Microsoft Azure Services** (This guide will get really messy if you use Windows 11 unless if you're good at navigating in the new updated Windows 11). This will help you understand and demonstrate on your skills in cloud infrastructure, Windows server configuration, and web-based application deployment - all of which are relevant for Help Desk and Cloud roles.

---

<h2>Video Demonstration</h2>

- ### [YouTube: How To Install osTicket with Prerequisites](https://www.youtube.com)
  

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop Protocol
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

1. Microsoft Azure Account (I recommend using pay-as-you-go compared to monthly subscription)
2. Azure Virtual Machine (Ideally Windows 10 despite Windows 11's existence, Use 4 vCPU's)
3. Azure Virtual Machine Account (Name it as osticket-vm)
   - Name: `osticket-vm`
   - Username: `ticketuser`
   - Password: `osTicket`

4. Remote Desktop Client (Azure will provide a client file if you're connecting to the VM)
5. osTicket Requirements
   
  - **PHP Manager for IIS (v1.5.0)**
    
    -> https://www.iis.net/downloads/community/2018/05/php-manager-150-for-iis-10
    
    "Think about it as a BIOS interface for PHP where it let's you toggle the settings instead of scouring through the raw config files and manually editing them each which is a big hassle."
    
  - **URL Rewrite Module 2.1 (x64)**
    
    -> https://www.iis.net/downloads/microsoft/url-rewrite

    "This is more like a GPS routing system for the website it will literally identify cleanly map URL's."
    
  - **PHP 7.3.8 Non-Thread Safe (VC15, x86)**
    
    -> https://windows.php.net/downloads/releases/php-7.3.8-nts-Win32-VC15-x86.zip

    "If osTicket is the software we're using then PHP is the language that runs the software."
    
  - **MySQL Community Server 5.5.62 (Windows, 32‑bit MSI)**
    
    -> https://cdn.mysql.com//Downloads/MySQL-5.5/mysql-5.5.62-win32.msi

    "It's basically the storage and filing system where it stores all the tickets, users, logs, and settings."
    
  - **Microsoft Visual C++ Redistributable 2015 (x86)**
    
    -> https://www.microsoft.com/en-us/download/details.aspx?id=48145

    "This is a device driver for PHP and MySQL that will make everything run well together."
    
  - **osTicket v1.15.8**
    
    -> https://osticket.com/osticket-v1-15-8-v1-16-3-available/

    "Main software that will run everything"
    
  - **HeidiSQL (Latest Stable Installer)**
    
    -> https://www.heidisql.com/download.php

    "Instead of using command-like SQL, it gives you a GUI to create databases and manage data it's basically similar to file explorer, but for MySQL."
    

## ⚙️ Step-by-Step Setup Guide

- Make sure to create a Microsoft Azure Account (Please don't use your school email ideally work email related or personal email account)

### 1️⃣ Create an Azure Virtual Machine

- OS: **Windows 10**
- Size: **4 vCPUs** or higher (B2ms works well)
- Name: `osticket-vm`
- Username: `labuser`
- Password: `osTicketPassword1!`

> 🟢 Open port **3389** (for Remote Desktop) and **80** (for the osTicket website)

---

### 2️⃣ Connect to the VM

- Use Remote Desktop (RDP) to log in using the credentials above

---


### 4️⃣ Enable IIS with CGI Support

1. Open **“Turn Windows Features on or off”**
2. Enable:
   - ✅ Internet Information Services (IIS)
   - ✅ World Wide Web Services → Application Development Features → ✅ CGI

---

### 5️⃣ Install Prerequisites

From the `osTicket-Installation-Files` folder, run the following installers:

- 🟢 **PHP Manager for IIS** (`PHPManagerForIIS_V1.5.0.msi`)
- 🟢 **URL Rewrite Module** (`rewrite_amd64_en-US.msi`)
- 🟢 **VC++ Redistributable** (`VC_redist.x86.exe`)

---

### 6️⃣ Set Up PHP

1. Create a new folder: `C:\PHP`
2. Extract `php-7.3.8-nts-Win32-VC15-x86.zip` into `C:\PHP`
3. Open **IIS Manager as Administrator**
4. Use PHP Manager to **register PHP**:
   - Set executable path: `C:\PHP\php-cgi.exe`
5. Restart IIS (Stop and Start the server)

---

### 7️⃣ Install MySQL

1. Run `mysql-5.5.62-win32.msi`
2. Choose **Typical Setup**
3. After install, launch the **Configuration Wizard**
4. Use **Standard Configuration**
5. Set credentials:
   - Username: `root`
   - Password: `root`

---

### 8️⃣ Deploy osTicket Files

1. Extract `osTicket-v1.15.8.zip`
2. Copy the `upload` folder into: `C:\inetpub\wwwroot`
3. Rename `upload` to: `osTicket`
4. Restart IIS again

---

### 9️⃣ Open osTicket in Browser

1. In IIS Manager:
   - Go to **Sites > Default Web Site > osTicket**
   - On the right panel, click: **Browse *:80**
2. This will open osTicket in your browser (`http://localhost/osTicket`)

---

### 🔌 Enable PHP Extensions

1. In IIS > osTicket > **PHP Manager**
2. Click “Enable or disable an extension”
3. Enable these:
   - `php_imap.dll`
   - `php_intl.dll`
   - `php_opcache.dll`
4. Refresh the browser — the missing extension warnings should disappear

---

### 🔧 Rename and Secure Config File

1. Rename:
   - From: `C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php`
   - To: `C:\inetpub\wwwroot\osTicket\include\ost-config.php`
2. Set permissions:
   - Right-click `ost-config.php` → Properties → Security
   - Disable inheritance → Remove all existing
   - Add user: **Everyone** → Grant **Full Control**

---

### 🛠️ Set Up the Database

1. Install **HeidiSQL**
2. Open it and create a new session:
   - Username: `root`
   - Password: `root`
3. Connect and create a database named: `osTicket`

---

### 🌐 Finalize osTicket Setup in Browser

1. Go back to `http://localhost/osTicket`
2. Fill in:
   - Helpdesk name
   - Admin account
   - Default email address
3. For the database section:
   - DB Name: `osTicket`
   - DB User: `root`
   - DB Password: `root`
4. Click **Install Now**

> ✅ If everything worked, you’ll get a success message!

---

## 🔐 Post-Install Cleanup

1. Delete the folder:  
   `C:\inetpub\wwwroot\osTicket\setup`
2. Change permissions of `ost-config.php` to **Read-only**

---

## 🚪 Access Links

- 🛠️ Admin Portal: `http://localhost/osTicket/scp/login.php`  
- 🧾 User Portal: `http://localhost/osTicket/`

---

## ✅ Why This Project Matters

- ✅ Shows ability to deploy full-stack web applications
- ✅ Demonstrates cloud VM provisioning and access control
- ✅ Hands-on with IIS, PHP, MySQL — real tools in IT environments
- ✅ Aligns with Help Desk, SysAdmin, and Cloud Support roles

