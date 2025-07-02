<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation via Microsoft Azure using their Cloud Computing Services</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket.<br />


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
    

<h2>Installation Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
