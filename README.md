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
     *<img width="1424" height="437" alt="Screenshot 2025-11-20 151003" src="https://github.com/user-attachments/assets/da291bc7-1d99-43ee-b171-82a365fc1b21" />

### 2. Download and Extract osTicket Installation Files
* Inside your VM, download the `osTicket-Installation-Files.zip`. If prompted with a virus scan warning, click **Download anyway**.
* Locate the downloaded `.zip` file, **right-click**, and select **Extract All...**
    *  Right-clicking the downloaded zip file to Extract All <img width="808" height="254" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/41915c04-f979-4146-87b2-09be172fac4f" />

* Confirm that the files are extracted to **DESKTOP**

### 3. Enable IIS with CGI Feature
* Go to **Control Panel** > **Uninstall A Program**.
* Click **Turn Windows features on or off** on the left.
* Enable the following components under **Internet Information Services**:
    * **World Wide Web Services** > **Application Development Features** > enable **CGI**.
    * <img width="534" height="664" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/2b2f46da-27a0-4a4a-a41d-c577f6f92f5c" />


### 4. Install IIS Modules
* Install **PHP Manager** (`PHPManagerForIIS_V1.5.0.msi`) from the extracted files.
* Install the **URL Rewrite Module** (`rewrite_amd64_en-US.msi`) from the extracted files.
* Install the **VC** (`VC_redist.x86`) form the extracted files
  * <img width="1036" height="346" alt="Screenshot (5)" src="https://github.com/user-attachments/assets/0d7edbf8-c2ce-48a9-8b0e-0f6fb6e97a9b" />

### 5. Create PHP Directory
* Open **File Explorer** and navigate to your **C drive**.
* Create a new folder and name it **PHP** (i.e., `C:\PHP`).
* Extract PHP into the C:\PHP Folder
* Locate the PHP zip file (`php-7.3.8-nts-Win32-VC15-x86.zip`).
* Right-click, select **Extract All...**, and change the destination folder to **`C:\PHP`**, then click **Extract**.
    * <img width="1920" height="1080" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/05390c86-1ae3-4858-bef5-8df71ab9a012" />

### 6. Install MySQL 5.5.62
* Install **MySQL 5.5.62** (`mysql-5.5.62-win32.msi`).
* Select **Typical setup** and proceed with the **Standard configuration**.
* Set and confirm a **New root password** for the MySQL server instance. The username and **PASSWORD** is **`root`**.
    * <img width="1157" height="896" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/6726e00c-9888-487c-8146-bf091e973547" />

### 7. Register PHP in IIS Manager
* Launch **IIS Manager** as administrator.
* Double-click **PHP Manager**.
* Click **Register new PHP version**.
* Browse to the `C:\PHP` folder and select the **`php-cgi.exe`** file.
    * <img width="1548" height="935" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/34e0e734-e2fb-4dbd-b700-dd7ac8a31654" />


### 8. Install osTicket Files
* From the `osTicket-Installation-Files` folder, unzip the osTicket archive (`osTicket-v1.15.8.zip`).
* **Copy** the **`upload`** folder found inside the extracted osTicket files.
* Navigate to **`C:\inetpub\wwwroot`**.
* **Paste** the **`upload`** folder here.
* **Rename** the **`upload`** folder to **`osTicket`**.
    * <img width="797" height="223" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/4292c200-c314-41a9-b455-f4991e3b3025" />

### 9. Reload IIS and Launch Setup
* In the right-hand **Actions** pane in IIS Manager, click **Stop** and then **Start** the server.
* In IIS Manager, navigate to **Sites** > **Default Web Site** > **osTicket**.
* In the **Actions** pane, click **Browse *:80 (http)**. This opens the setup page.
    <img width="1488" height="256" alt="Screenshot (14)" src="https://github.com/user-attachments/assets/419ccb1b-6d14-48ec-973e-078735ba1a8a" />

### 10. Enable Required PHP Extensions
* Go back to **IIS Manager** > **Sites** > **Default Web Site** > **osTicket**, and open **PHP Manager**.
* Click **Enable or disable an extension**.
* Enable the following extensions that are typically marked as missing or recommended:
    * `php_imap.dll` (Required for mail fetching)
    * `php_intl.dll` (Recommended for improved localization)
    * `php_opcache.dll` (Recommended for faster performance)
    * <img width="874" height="945" alt="Screenshot (16)" src="https://github.com/user-attachments/assets/7f027bf2-facb-420d-ab80-b664c53fe404" />


### 11. Rename Configuration File
* Navigate to **`C:\inetpub\wwwroot\osTicket\include`**.
* Rename the file **`ost-sampleconfig.php`** to **`ost-config.php`**.
    * <img width="907" height="266" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/29ec4580-5769-44bd-941b-4788ace8ba4e" />



### 12. Set Permissions for ost-config.php
* **Right-click** **`ost-config.php`** and select **Properties** > **Security** tab > **Advanced**.
* Click **Disable inheritance**, and then select **Remove all inherited permissions from this object**.
* Click **Add** > **Select a principal** > type **`Everyone`** > click **Check Names**, then **OK**.
    * <img width="1455" height="743" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/8f9150f8-f8eb-409e-ba28-dac3fe97293b" />
* Grant **Full control** to **Everyone**, then click **OK** and **Apply**.

### 13. Install HeidiSQL and Create Database
* Install **HeidiSQL**.
* Open HeidiSQL and create a new session using:
    * **User:** `root`
    * **Password:** `root`
    * <img width="1061" height="637" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/41075c6c-8393-42a5-bce8-a512494d418b" />
* **Right-click** on the `Unnamed` session, click **Create new** > **Database**.
* Name the database **`osTicket`** and click **OK**.
    * <img width="1177" height="630" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/fc18a0ca-7e4b-4c94-b6ba-fac22eb1f1e5" />


### 14. Finalize osTicket Setup
* Return to the osTicket setup page in your browser and refresh it.
* Fill out the **Admin User** details and **Database Settings** (ensure they match the `root` user, password: `root` and `osTicket` database created).
    * <img width="1074" height="880" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/83f4b5c3-38f9-4887-96c3-5e259e93766c" />
* Click **Install Now**.

### 15. Post-Installation Confirmation
* Upon successful installation, you will see the **Congratulations!** page.
    * <img width="1084" height="832" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/3cfa4295-657d-47b9-88f0-1a2460830b32" />
* Click the link to navigate to your **Staff Control Panel** (e.g., `http://localhost/osTicket/scp/`) and log in.

---
