<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# 🛠️ osTicket - Prerequisites and Installation Guide for Windows/IIS

This tutorial outlines the prerequisites and complete installation of the open-source help desk ticketing system **osTicket v1.15.8** on a Windows Server environment using **Internet Information Services (IIS)** and **MySQL**.

---

## 💻 Environments and Technologies Used

* **Microsoft Azure** (Virtual Machines/Compute)
* **Remote Desktop**
* **Internet Information Services (IIS)**
* **MySQL** (Database Server)
* **PHP** (via CGI)

## 🗄️ Operating Systems Used

* **Windows 10** (21H2) or **Windows Server**

## ✅ List of Prerequisites

All installer files were sourced from the `osTicket-Installation-Files.zip` package:
* IIS (Internet Information Services)
* PHP Manager
* VC\_redistx86
* MySQL 5.5.62
* URL Rewrite Module
* Heidi SQL
* osTicket v1.15.8 files

---

# 🚀 Installation Steps

### 1. Create and Connect to Virtual Machine in Azure
* Create a new VM with **Windows Server** in the Azure Portal.
* Use the VM’s public IP address to connect via **Remote Desktop Connection (RDP)**.
* Log in using the administrative credentials created during VM setup.
* 

### 2. Download and Extract osTicket Installation Files
* Inside your VM, download the `osTicket-Installation-Files.zip`. If prompted with a virus scan warning, click **Download anyway**.
* Locate the downloaded `.zip` file, **right-click**, and select **Extract All...**
    *  Right-clicking the downloaded zip file to Extract All <img width="1246" height="475" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/290e148b-a705-47ae-8737-79352444db50" />
* Confirm that the files are extracted to **DESKTOP**

### 3. Enable IIS with CGI Feature
* Go to **Control Panel** > **Uninstall A Program**.
* Click **Turn Windows features on or off** on the left.
* Enable the following components under **Internet Information Services**:
    * **World Wide Web Services** > **Application Development Features** > enable **CGI**.
    * <img width="534" height="664" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/4e9f13ff-0223-4a93-bc2d-a6dbeaa564a3" />


### 4. Install IIS Modules
* Install **PHP Manager** (`PHPManagerForIIS_V1.5.0.msi`) from the extracted files.
* Install the **URL Rewrite Module** (`rewrite_amd64_en-US.msi`) from the extracted files.
* Install the **VC** (`VC_redist.x86`) form the extracted files
  * <img width="1036" height="346" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/7d40cbdf-7a9c-435b-86f0-c7e44c2d959d" />


### 5. Create PHP Directory
* Open **File Explorer** and navigate to your **C drive**.
* Create a new folder and name it **PHP** (i.e., `C:\PHP`).
    * <img width="1110" height="387" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/e1d46599-43b0-4744-b048-3df54a9fa59d" />
* Extract PHP into the C:\PHP Folder
* Locate the PHP zip file (`php-7.3.8-nts-Win32-VC15-x86.zip`).
* Right-click, select **Extract All...**, and change the destination folder to **`C:\PHP`**, then click **Extract**.



### 6. Install MySQL 5.5.62
* Install **MySQL 5.5.62** (`mysql-5.5.62-win32.msi`).
* Select **Typical setup** and proceed with the **Standard configuration**.
* Set and confirm a **New root password** for the MySQL server instance. The username and **PASSWORD** is **`root`**.
    * <img width="1157" height="896" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/05452741-15fa-40d5-9e18-99dc442f5cf7" />


### 7. Register PHP in IIS Manager
* Launch **IIS Manager** as administrator.
* Double-click **PHP Manager**.
* Click **Register new PHP version**.
* Browse to the `C:\PHP` folder and select the **`php-cgi.exe`** file.
    * <img width="1548" height="935" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/66116ef4-9dba-4728-a0b8-20a3baec7719" />


### 8. Install osTicket Files
* From the `osTicket-Installation-Files` folder, unzip the osTicket archive (`osTicket-v1.15.8.zip`).
* **Copy** the **`upload`** folder found inside the extracted osTicket files.
* Navigate to **`C:\inetpub\wwwroot`**.
* **Paste** the **`upload`** folder here.
* **Rename** the **`upload`** folder to **`osTicket`**.
    * <img width="797" height="223" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/8a0c8f3d-d402-4f43-8083-55e0101ea9e3" />


### 9. Reload IIS and Launch Setup
* In the right-hand **Actions** pane in IIS Manager, click **Stop** and then **Start** the server.
* In IIS Manager, navigate to **Sites** > **Default Web Site** > **osTicket**.
* In the **Actions** pane, click **Browse *:80 (http)**. This opens the setup page.
    <img width="1488" height="256" alt="Screenshot (14)" src="https://github.com/user-attachments/assets/a852382e-84d2-4297-9999-9b128d46d7a9" />

### 10. Enable Required PHP Extensions
* Go back to **IIS Manager** > **Sites** > **Default Web Site** > **osTicket**, and open **PHP Manager**.
* Click **Enable or disable an extension**.
* Enable the following extensions that are typically marked as missing or recommended:
    * `php_imap.dll` (Required for mail fetching)
    * `php_intl.dll` (Recommended for improved localization)
    * `php_opcache.dll` (Recommended for faster performance)
    * <img width="874" height="945" alt="Screenshot (16)" src="https://github.com/user-attachments/assets/cdcffbf6-6db6-49c4-a16d-8abc02881f57" />



### 11. Rename Configuration File
* Navigate to **`C:\inetpub\wwwroot\osTicket\include`**.
* Rename the file **`ost-sampleconfig.php`** to **`ost-config.php`**.
    * <img width="907" height="266" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/cc52415b-1ca7-4f83-accf-088aca526717" />



### 12. Set Permissions for ost-config.php
* **Right-click** **`ost-config.php`** and select **Properties** > **Security** tab > **Advanced**.
* Click **Disable inheritance**, and then select **Remove all inherited permissions from this object**.
* Click **Add** > **Select a principal** > type **`Everyone`** > click **Check Names**, then **OK**.
    * <img width="1455" height="743" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/2dcb1916-9d73-4672-ad8c-4b09c2077beb" />
* Grant **Full control** to **Everyone**, then click **OK** and **Apply**.

### 13. Install HeidiSQL and Create Database
* Install **HeidiSQL**.
* Open HeidiSQL and create a new session using:
    * **User:** `root`
    * **Password:** `root`
    * <img width="1061" height="637" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/576f5cc5-aff3-401d-85fd-3a2c70ce3a56" />
* **Right-click** on the `Unnamed` session, click **Create new** > **Database**.
* Name the database **`osTicket`** and click **OK**.
    * <img width="1177" height="630" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/1240b0c9-60e3-453d-a7ff-7ddeb3bedb08" />


### 14. Finalize osTicket Setup
* Return to the osTicket setup page in your browser and refresh it.
* Fill out the **Admin User** details and **Database Settings** (ensure they match the `root` user, password: `root` and `osTicket` database created).
    <img width="1074" height="880" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/03195e26-bab5-491c-a9f5-64c0f0aecd6c" />

* Click **Install Now**.

### 15. Post-Installation Confirmation
* Upon successful installation, you will see the **Congratulations!** page.
    * <img width="1084" height="832" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/77a3fd1f-8be5-4014-9f39-8f84e77ab626" />
* Click the link to navigate to your **Staff Control Panel** (e.g., `http://localhost/osTicket/scp/`) and log in.

---
