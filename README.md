# Load AdventureWorks in MySQL on Windows 🚀

This guide helps you set up the AdventureWorks database in MySQL on Windows. It's beginner-friendly and includes all necessary steps, from installation to backup. 😊

**Date**: April 05, 2025  
**Files Needed**:  
- `AdventureWorks2019.sql` (main database file)

---

## ⚙️ Prerequisites

- Windows 10 or 11  
- MySQL Server 8.0+  
- MySQL Workbench 8.0 CE (optional GUI)  
- SQL Server (needed if you're converting or referencing from the original format)

---

## 🛠️ Step 1: Install MySQL Server and Workbench

1. Download the MySQL Installer:  
   [https://dev.mysql.com/downloads/installer](https://dev.mysql.com/downloads/installer)  
   Save file as: `mysql-installer-web-community-8.0.36.0.msi`  
   Location: `E:\Download`

2. Run the installer:  
   - Choose **Custom Setup**  
   - Select:
     - MySQL Server 8.0.41  
     - MySQL Workbench 8.0 CE  
   - Install both.

3. During setup, set root password:  
   - Example: `mysqlnewpass`

---

## 🧹 Step 2: Fix MySQL Server Setup (if service fails)

1. Open PowerShell as Admin:  
   - `Win + S` → search "PowerShell" → right-click → **Run as Administrator**

2. Initialize MySQL data directory:

```powershell
& "C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqld.exe" --initialize-insecure
```

3. Start MySQL manually:

```powershell
& "C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqld.exe" --console
```

4. In another terminal, set root password:

```powershell
& "C:\Program Files\MySQL\MySQL Server 8.0\bin\mysql.exe" -u root
```

Inside MySQL:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'mysqlnewpass';
FLUSH PRIVILEGES;
exit
```

5. Stop MySQL:  
   - Use `Ctrl + C` in console window.

6. Register MySQL as a Windows service:

```powershell
& "C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqld.exe" --install MySQL80
Start-Service -Name "MySQL80"
```

---

## 🧾 Step 3: Load AdventureWorks Data

1. Open MySQL Workbench:

```powershell
& "C:\Program Files\MySQL\MySQL Workbench 8.0 CE\MySQLWorkbench.exe"
```

2. Connect to root using `mysqlnewpass`.

3. Run the following to create the database:

```sql
CREATE DATABASE adventureworks CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

4. Load `.sql` file using CMD (not PowerShell):  
   - Open CMD as Admin  
   - Navigate to the file location

```cmd
"C:\Program Files\MySQL\MySQL Server 8.0\bin\mysql.exe" -u root -p adventureworks < "E:\Download\AdventureWorks2019.sql"
```

Password: `mysqlnewpass`

---

## 🔍 Step 4: Verify Data in Workbench

Run the following queries:

```sql
USE adventureworks;
SHOW TABLES;

SELECT COUNT(*) FROM humanresources_department;
SELECT COUNT(*) FROM person_address;
SELECT COUNT(*) FROM sales_customer;

SELECT * FROM person_address a 
JOIN person_stateprovince sp ON a.StateProvinceID = sp.StateProvinceID 
LIMIT 5;

SELECT * FROM sales_customer LIMIT 5;
```

Expected results:  
- `humanresources_department`: 16 rows  
- `person_address`: ~19,614 rows  
- `sales_customer`: ~19,185 rows  

---

## 💾 Step 5: Back Up the Database

Run this in CMD:

```cmd
"C:\Program Files\MySQL\MySQL Server 8.0\bin\mysqldump.exe" -u root -p adventureworks > "E:\Download\adventureworks_backup.sql"
```

To restore backup later:

```cmd
"C:\Program Files\MySQL\MySQL Server 8.0\bin\mysql.exe" -u root -p adventureworks < "E:\Download\adventureworks_backup.sql"
```

---

## 📂 Explore Some Data

```sql
SELECT * FROM production_product LIMIT 5;
```

---

## 🧨 If You Want to Remove the Database

```sql
DROP DATABASE adventureworks;
```

---

## 📝 Note on SQL Server

If you're using the original AdventureWorks `.bak` file (for SQL Server):

- Install SQL Server (Express or Developer Edition)
- Restore `.bak` using SQL Server Management Studio (SSMS)
- Then export tables as `.sql` script for MySQL using tools like MySQL Migration Toolkit or manually through SSMS + Workbench

---

Enjoy working with AdventureWorks in MySQL! 🌟  
