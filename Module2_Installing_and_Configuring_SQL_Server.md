# Module 2: **Installing and Configuring SQL Server**

> **Module Objective**
> This module provides a complete practical guide to installing, configuring, patching, and upgrading SQL Server, with explicit step-by-step instructions for every complex procedure, enabling learners to master installation, configuration, patching, and upgrade strategies.

## Learning Outcomes

By the end of this module, learners will be able to:

1.  Identify SQL Server system requirements and differentiate between various editions.
2.  Perform SQL Server installations in diverse environments (bare-metal, virtualized, containerized) using interactive and unattended methods.
3.  Configure essential SQL Server services and instance-level settings, including service accounts, collation, memory, parallelism, `tempdb`, `FILESTREAM`, and `PolyBase`.
4.  Utilize SQL Server Management Studio (SSMS) for effective database administration and connectivity.
5.  Implement and manage different SQL Server authentication modes, including Windows Authentication with Kerberos and SQL Server Authentication.
6.  Create and manage multiple SQL Server instances on a single server.
7.  Configure network protocols, ports, firewall rules, and TLS encryption for secure SQL Server connectivity.

## Reference Materials

  - **Books:**

      - *SQL Server 2019 Administration Inside Out* by William Assaf, Randolph West, and Sven Aelterman
      - *Pro SQL Server 2019 Administration* by Louis Davidson, Kevin Kline, and Brent Ozar
      - *Expert SQL Server 2019 Administration* by Tim Chapman and Grant Fritchey

  - **Online Resources:**

      - Microsoft SQL Server Documentation ([https://learn.microsoft.com/en-us/sql/sql-server/sql-server-technical-documentation](https://learn.microsoft.com/en-us/sql/sql-server/sql-server-technical-documentation))
      - SQL Server Installation Guide ([https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server](https://learn.microsoft.com/en-us/sql/database-engine/install-windows/install-sql-server))
      - Configure `tempdb` ([https://learn.microsoft.com/en-us/sql/relational-databases/databases/tempdb-database](https://learn.microsoft.com/en-us/sql/relational-databases/databases/tempdb-database))
      - Enable Encrypted Connections to the Database Engine ([https://learn.microsoft.com/en-us/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine](https://learn.microsoft.com/en-us/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine))
      - DISA STIGs for SQL Server ([https://public.cyber.mil/stigs/](https://public.cyber.mil/stigs/) - search for SQL Server STIGs)

## Key Concepts & Detailed Content

### 1\. **2.1 System Requirements and Editions**

  - **System Requirements**:
      - **Operating System**: Windows Server (recommended for production), Windows 10/11 (for development/testing). Specific versions depend on SQL Server version.
      - **Hardware**:
          - **Processor**: Minimum 1.4 GHz (x64 processor), 2 GHz or faster recommended.
          - **RAM**: Minimum 1 GB (Express Edition), 4 GB (other editions), 16 GB+ recommended for production.
          - **Disk Space**: Minimum 6 GB available hard-disk space. More for databases and logs.
  - **SQL Server Editions**:
      - **Developer Edition**: Free, full-featured edition for development and testing in non-production environments.
      - **Express Edition**: Free, entry-level database for small-scale applications, limited in CPU, memory, and database size.
      - **Standard Edition**: Provides core database capabilities for small to mid-tier applications, with limited high-availability features.
      - **Enterprise Edition**: Premium edition with comprehensive features for mission-critical applications, including advanced analytics, high availability (AlwaysOn), and unlimited scalability.

### 2\. **2.2 Installation Steps for SQL Server**

  - **Installation Environments**:
      - **Bare-metal**: Direct installation on physical hardware for maximum performance and control.
      - **Virtualized**: Installation within a virtual machine (e.g., VMware, Hyper-V) offering flexibility, snapshots, and resource management.
      - **Containerized (Docker)**: Deploying SQL Server as a Docker container for lightweight, portable, and isolated environments, ideal for development/testing.
  - **Installation Methods**:
      - **Interactive Setup**: Using the graphical SQL Server Installer.
          - **Run the Installer**: Double-click the downloaded `SQLServer*.exe`.
          - **Select "Basic" installation type**: Recommended for beginners or quick evaluations.
          - **Accept License Terms**: Check "I accept the license terms" and click **Install**.
          - **Wait for Installation**: Progress bar will show installation status (\~10-20 mins). When complete, click **Close**.
          - **Custom Installation**: Provides granular control over features, installation paths, service accounts, and collation settings. Recommended for production.
      - **Silent/Unattended Setup**: Using command-line parameters or configuration files (`.ini`) for automated installations, ideal for large deployments or scripting.
  - **Side-by-Side Installs**: Installing multiple SQL Server instances (different versions or editions) on the same server, each with its own resources and configurations. This allows for testing or running multiple applications requiring different SQL Server versions.

### 3\. **2.3 Configuring SQL Server Services**

  - **Service Accounts**:
      - **Definition**: User accounts under which SQL Server services (Database Engine, Agent, Browser, etc.) run.
      - **Best Practices**: Use low-privilege domain accounts (Managed Service Accounts or Group Managed Service Accounts if available), separate accounts for different services, avoid using built-in accounts (Local System, Network Service) in production.
  - **Collation**:
      - **Definition**: Rules that SQL Server uses to sort and compare data, including character set, sort order, and sensitivity (case, accent, kana, width).
      - **Types**: SQL Server collations (e.g., `SQL_Latin1_General_CP1_CI_AS`), Windows collations (e.g., `Latin1_General_CI_AS`).
      - **Unicode/Non-Unicode**: Unicode collations (e.g., those ending in `_UTF8` or `_100`) support a wider range of characters and are generally recommended for new applications.
  - **Memory Configuration**:
      - **`max server memory (MB)`**: Sets the maximum amount of RAM SQL Server's buffer pool can consume. Crucial for system stability and preventing resource starvation for the OS or other applications.
      - **`min server memory (MB)`**: Ensures SQL Server retains a minimum amount of RAM, preventing excessive paging.
  - **Parallelism (MAXDOP)**:
      - **Definition**: `MAXDOP` (Maximum Degree of Parallelism) controls the number of processors used for parallel query execution.
      - **Configuration**: Typically set to the number of logical processors or 8 (whichever is lower) on larger servers to prevent over-parallelization, which can sometimes hurt performance.
  - **`tempdb` Optimization**:
      - **Definition**: A system database used by SQL Server as a temporary workspace for various operations.
      - **Multiple Files**: Create multiple data files for `tempdb` to reduce contention on allocation structures (PFS, GAM, SGAM pages). Microsoft generally recommends one `tempdb` data file per CPU core, up to 8 files.
        ```sql
        -- Example: Add new tempdb files
        ALTER DATABASE tempdb
        ADD FILE (NAME = tempdev2, FILENAME = 'C:\tempdb\tempdb2.ndf', SIZE = 256MB);

        ALTER DATABASE tempdb
        ADD FILE (NAME = tempdev3, FILENAME = 'C:\tempdb\tempdb3.ndf', SIZE = 256MB);

        ALTER DATABASE tempdb
        ADD FILE (NAME = tempdev4, FILENAME = 'C:\tempdb\tempdb4.ndf', SIZE = 256MB);
        ```
      - **Sizing**: Initial size should be adequate to avoid frequent autogrowth.
      - **Location**: Ideally, `tempdb` files should be on a fast I/O subsystem separate from user databases.
  - **`FILESTREAM`**:
      - **Definition**: A feature that integrates SQL Server with the NTFS file system for storing large binary objects (BLOBs) directly on the file system while maintaining transactional consistency with the database.
      - **Configuration**: Enabled at the instance level via SQL Server Configuration Manager, then on specific databases and tables.
  - **PolyBase**:
      - **Definition**: Technology that allows SQL Server to process queries on external data sources (like Hadoop, Azure Blob Storage, Azure Data Lake Store, Amazon S3) as if they were local tables.
      - **Configuration**: Requires installation of the PolyBase feature during setup and configuring external data sources within SQL Server.
  - **Hardening**:
      - **Best Practices**: Implementing security configurations beyond defaults to minimize vulnerabilities.
      - **Surface Area Reduction**: Minimizing enabled features, components, and services to reduce potential attack vectors (e.g., disabling CLR, `xp_cmdshell`, Ole Automation Procedures if not required).
      - **DISA STIGs (Defense Information Systems Agency Security Technical Implementation Guides)**: A set of stringent configuration standards for securing IT systems. Adhering to SQL Server-specific STIGs provides a highly secure and compliant configuration.

### 4\. **2.4 SQL Server Management Studio (SSMS) Overview**

  - **Role of SSMS**: A powerful, graphical tool for configuring, managing, and administering all components within SQL Server. It provides a comprehensive environment for database developers and administrators.
  - **Connecting to Instances**:
      - **Verify Installation**: After installation, use SSMS to connect to the SQL Server instance.
      - **Open SSMS**: Search for "SQL Server Management Studio" in the Start Menu.
      - **Connect to**:
          - **Server name**: `localhost` (for default instance on the same machine) or `.\SQLEXPRESS` (for Express named instance) or `YourServerName\YourInstanceName` (for named instances).
          - **Authentication**: Typically **Windows Authentication** for initial connection.

### 5\. **2.5 Authentication Modes (Windows vs. SQL Server)**

  - **Windows Authentication (Recommended)**:
      - **Definition**: Uses Windows user and group accounts for authentication. Users are authenticated by Windows before SQL Server verifies their permissions.
      - **Integration**: Seamlessly integrates with Active Directory, allowing centralized user management.
      - **Kerberos/SPNs**:
          - **Kerberos**: A network authentication protocol that provides strong, mutual authentication.
          - **Service Principal Names (SPNs)**: Unique identifiers for services running on a server (e.g., `MSSQLSvc/YourServer.YourDomain.com:1433`). Proper SPN registration is crucial for Kerberos authentication to SQL Server and prevents fallback to less secure NTLM authentication.
  - **SQL Server Authentication**:
      - **Definition**: Uses usernames and passwords stored directly within SQL Server.
      - **Use Cases**: Necessary for applications that don't support Windows Authentication, or in non-domain environments. Less secure than Windows Authentication due to local password management.

### 6\. **2.6 Creating and Managing SQL Server Instances**

  - **Default Instance**:
      - **Definition**: The first instance installed on a server, typically identified by just the server name (e.g., `YourServerName`). Listens on TCP port 1433 by default.
  - **Named Instances**:
      - **Definition**: Additional instances installed on the same server, identified by `YourServerName\InstanceName` (e.g., `YourServerName\SQL2019`).
      - **Management**: Managed primarily through SQL Server Configuration Manager for service control, network configuration, and through SSMS for database and security management.

### 7\. **2.7 Enabling TCP/IP and Named Pipes Protocols**

  - **Network Protocols**:
      - **TCP/IP**: The most common and recommended network protocol for SQL Server client connections, especially over a network.
      - **Named Pipes**: Primarily used for local connections or connections over a Local Area Network (LAN).
      - **Shared Memory**: The fastest protocol for local connections on the same machine, as it bypasses network stack.
  - **Ports**:
      - **Default Instance**: Listens on TCP port **1433** by default.
      - **Named Instances**: Use dynamic ports by default, but can be configured to use specific static ports for easier firewall management.
  - **Firewall Configuration**:
      - **Task: Allow SQL Server Through Firewall**:
          - **Open Firewall Settings**: Search for "Windows Defender Firewall" in Start Menu → Select **Advanced Settings**.
          - **Create New Inbound Rule**: Click **Inbound Rules** → **New Rule**.
          - **Configure Port**: Select **Port** → Click **Next**. Select **TCP**, enter port **1433** (or the static port for a named instance) → Click **Next**.
          - **Allow Connection**: Select **Allow the connection** → Click **Next** → Name rule: **"SQL Server Access"** (or similar) → Click **Finish**.
  - **Network Packet Size**:
      - **Definition**: The size of the data packets used for network communication between client and server.
      - **Configuration**: Can be adjusted in SQL Server properties to optimize performance for specific network conditions, though the default (4096 bytes) is usually sufficient.
  - **Endpoints**:
      - **Definition**: SQL Server objects that allow specific network protocols, ports, and authentication methods for incoming connections. Used for features like Database Mirroring, AlwaysOn Availability Groups, and external connections.
  - **TLS Encryption**:
      - **Task: Secure Connections with TLS 1.2**:
          - **Open Configuration Manager**: Search for "SQL Server Configuration Manager" in Start Menu.
          - **Navigate to Protocols**: Expand **SQL Server Network Configuration** → Click **Protocols for [YourInstance]**.
          - **Enable TLS**: Right-click **Protocols for [YourInstance]** → Select **Properties**. On the **Certificates** tab, select an installed SSL/TLS certificate. On the **Flags** tab, set **Force Encryption** to **Yes**.
          - **Disable Older Protocols**: (Recommended) In the Registry Editor, navigate to `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols`. Here, you can disable older protocols (SSL 3.0, TLS 1.0, and TLS 1.1) by creating subkeys for them (e.g., `TLS 1.0\Client` and `TLS 1.0\Server`) and adding a `DWORD` value named `Enabled` with data `0`.
          - **Restart SQL Server Service**: In SQL Server Configuration Manager, right-click SQL Server service → **Restart**.

## Lab Exercises / Hands-On Practice

| \# | Task | Tool | Outcome |
|---|---|---|---|
| 1 | Install SQL Server 2019 Developer Edition (Basic) and verify connectivity via SSMS. | SQL Server Installer, SSMS | SQL Server Database Engine installed and accessible locally. |
| 2 | Configure `tempdb` on the installed instance to have 4 data files, each 256MB in size. | SSMS | `tempdb` configured with multiple data files. |
| 3 | Install a second, named instance of SQL Server 2019 Developer Edition using a custom installation, specifying a different collation and a dedicated service account. | SQL Server Installer | A new named SQL Server instance is installed with specified configurations. |
| 4 | Enable `FILESTREAM` on the named instance and create a database and table to store `FILESTREAM` data. | SQL Server Configuration Manager, SSMS | `FILESTREAM` is enabled and a table can store large binary objects. |
| 5 | Configure Windows Firewall to allow TCP port 1433 for the default instance and a static port (e.g., 1434) for the named instance. | Windows Defender Firewall with Advanced Security | Inbound firewall rules created for both instances. |
| 6 | Enable TLS 1.2 encryption for connections to the default SQL Server instance (using a self-signed certificate for lab purposes). | SQL Server Configuration Manager, SSMS | SQL Server connections are encrypted via TLS 1.2. |
| 7 | Connect to both the default and named instances from a remote machine using SSMS, verifying successful connectivity and encryption status. | SSMS (on client machine) | Remote connections are successful and encrypted where configured. |

### 1\. Install SQL Server 2019 Developer Edition (Basic) and verify connectivity via SSMS.

**Tool:** SQL Server Installer, SSMS

**Outcome:** SQL Server Database Engine installed and accessible locally.

**Steps:**

1.  **Download SQL Server 2019 Developer Edition:** Go to the official Microsoft SQL Server downloads page and download the Developer Edition.
2.  **Run the Installer:** Execute the downloaded file.
3.  **Choose Installation Type:**
      * On the "SQL Server Installation Center" screen, select "Installation" from the left-hand menu.
      * Choose "New SQL Server stand-alone installation or add features to an existing installation."
4.  **Edition Selection:** Ensure "Developer" is selected.
5.  **License Terms:** Accept the license terms.
6.  **Microsoft Update:** (Optional but recommended) Check "Use Microsoft Update to check for updates (recommended)".
7.  **Install Rules:** Let the installer check for any issues. Resolve any warnings or errors if they appear.
8.  **Feature Selection:** For a basic installation, you can typically leave the default selected features, ensuring "Database Engine Services" is checked.
9.  **Instance Configuration:**
      * Select "Default instance". This will name your instance `MSSQLSERVER`.
      * (Optional) You can change the Instance root directory if desired.
10. **Server Configuration:**
      * **Service Accounts:** For "SQL Server Database Engine," it's common to use "NT Service\\MSSQLSERVER" as the account name for the default instance or a dedicated domain account in a production environment. Set the "Startup Type" to "Automatic."
      * **Collation:** Note the default collation (usually `SQL_Latin1_General_CP1_CI_AS`). You'll change this in a later step for the named instance.
11. **Database Engine Configuration:**
      * **Authentication Mode:** Choose "Mixed Mode (SQL Server authentication and Windows authentication)".
          * **Enter a password for the `sa` account.** Remember this password.
      * **Specify SQL Server administrators:** Click "Add Current User" to add your current Windows user as a SQL Server administrator. You can add other users/groups as needed.
      * **Data Directories:** (Optional) You can specify custom directories for user databases, logs, and tempdb. For a basic lab, the defaults are usually fine.
12. **Ready to Install:** Review your selections and click "Install."
13. **Installation Progress:** Wait for the installation to complete.
14. **Verify Connectivity via SSMS:**
      * **Download SSMS:** If you don't have it, download and install SQL Server Management Studio (SSMS) from the Microsoft website.
      * **Launch SSMS:** Open SSMS.
      * **Connect to Server:** In the "Connect to Server" dialog:
          * **Server name:** Enter `.` (for local machine) or `localhost`.
          * **Authentication:** Select "Windows Authentication."
          * Click "Connect."
      * **Verification:** Once connected, in the Object Explorer, you should see your server instance and be able to expand "Databases," "Security," etc., confirming successful installation and connectivity.

### 2\. Configure `tempdb` on the installed instance to have 4 data files, each 256MB in size.

**Tool:** SSMS

**Outcome:** `tempdb` configured with multiple data files.

**Steps:**

1.  **Connect to SQL Server:** Open SSMS and connect to your default SQL Server instance (e.g., `.` or `localhost`) using Windows Authentication.

2.  **Open New Query:** Click "New Query" in SSMS.

3.  **Check Current `tempdb` Files (Optional but Recommended):**

    ```sql
    USE tempdb;
    SELECT name, physical_name, size/128.0 AS SizeMB, state_desc
    FROM sys.database_files;
    ```

    This query will show you the existing `tempdb` data and log files.

4.  **Add `tempdb` Data Files:** Execute the following T-SQL script. This script will add three additional data files, bringing the total to four. Remember to adjust the `FILENAME` path to match your SQL Server's data directory. You can find the default data directory by looking at the `physical_name` from the previous query.

    ```sql
    USE master;
    GO

    ALTER DATABASE tempdb
    ADD FILE
    (
        NAME = tempdev2,
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\tempdev2.ndf', -- Adjust path accordingly
        SIZE = 256MB,
        FILEGROWTH = 256MB
    );
    GO

    ALTER DATABASE tempdb
    ADD FILE
    (
        NAME = tempdev3,
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\tempdev3.ndf', -- Adjust path accordingly
        SIZE = 256MB,
        FILEGROWTH = 256MB
    );
    GO

    ALTER DATABASE tempdb
    ADD FILE
    (
        NAME = tempdev4,
        FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\DATA\tempdev4.ndf', -- Adjust path accordingly
        SIZE = 256MB,
        FILEGROWTH = 256MB
    );
    GO
    ```

    **Note:** It's generally recommended that the number of `tempdb` data files should be equal to or up to 8 logical processors (cores) on your server, or at least 1 for every 4 cores. For this lab, 4 files are specified.

5.  **Restart SQL Server Service:** For the changes to `tempdb` to take full effect, you need to restart the SQL Server service.

      * Open **SQL Server Configuration Manager** (Start Menu -\> Microsoft SQL Server 2019 -\> SQL Server 2019 Configuration Manager).
      * In the left pane, select "SQL Server Services."
      * In the right pane, right-click "SQL Server (MSSQLSERVER)" and select "Restart."

6.  **Verify `tempdb` Configuration:** After the restart, reconnect to SQL Server in SSMS and run the following query again to confirm the new files:

    ```sql
    USE tempdb;
    SELECT name, physical_name, size/128.0 AS SizeMB, state_desc
    FROM sys.database_files;
    ```

    You should now see four data files for `tempdb`, each with an initial size of 256MB.

### 3\. Install a second, named instance of SQL Server 2019 Developer Edition using a custom installation, specifying a different collation and a dedicated service account.

**Tool:** SQL Server Installer

**Outcome:** A new named SQL Server instance is installed with specified configurations.

**Steps:**

1.  **Run the SQL Server Installer:** Execute the SQL Server 2019 Developer Edition installer again.
2.  **Choose Installation Type:**
      * On the "SQL Server Installation Center" screen, select "Installation."
      * Choose "New SQL Server stand-alone installation or add features to an existing installation."
3.  **Edition Selection:** Ensure "Developer" is selected.
4.  **License Terms:** Accept the license terms.
5.  **Microsoft Update:** (Optional but recommended) Check "Use Microsoft Update to check for updates (recommended)".
6.  **Install Rules:** Let the installer check for any issues.
7.  **Feature Selection:** Select "Database Engine Services." You can deselect other features if you only need the database engine.
8.  **Instance Configuration:**
      * Select "Named instance."
      * **Instance ID:** Provide a unique name for your instance, e.g., `SQLDEV`. This will make your instance accessible as `YOUR_COMPUTER_NAME\SQLDEV`.
      * (Optional) You can change the Instance root directory.
9.  **Server Configuration:**
      * **Service Accounts:**
          * For "SQL Server Database Engine," you will create a dedicated service account. For lab purposes, you can create a local Windows user account and specify it here.
              * **To create a local Windows user:**
                  * Go to "Computer Management" (Right-click Start -\> Computer Management).
                  * Expand "Local Users and Groups" -\> "Users."
                  * Right-click "Users" -\> "New User."
                  * Create a user (e.g., `SQLServiceAcct`), set a password, and uncheck "User must change password at next logon."
              * Back in the SQL Server installer, for "Account Name," enter the newly created local user (e.g., `.\SQLServiceAcct` for a local account). Enter the password.
          * Set the "Startup Type" to "Automatic" for both SQL Server Database Engine and SQL Server Browser (SQL Server Browser is important for named instances).
      * **Collation:** This is where you specify a different collation.
          * Click the "Collation" tab.
          * Click "Customize..."
          * Choose a different collation from the default, for example, `Latin1_General_100_CI_AS_SC` or `Japanese_CI_AS`. Select one that is distinctly different from the default `SQL_Latin1_General_CP1_CI_AS`.
          * Click "OK."
10. **Database Engine Configuration:**
      * **Authentication Mode:** Choose "Mixed Mode (SQL Server authentication and Windows authentication)".
          * **Enter a password for the `sa` account.** Remember this password.
      * **Specify SQL Server administrators:** Click "Add Current User" to add your current Windows user as a SQL Server administrator.
      * **Data Directories:** (Optional) You can specify custom directories for user databases, logs, and tempdb. It's a good practice to put them in a different location than the default instance.
11. **Ready to Install:** Review your selections and click "Install."
12. **Installation Progress:** Wait for the installation to complete.
13. **Verify Connectivity:**
      * Open SSMS.
      * In the "Connect to Server" dialog:
          * **Server name:** Enter `.\SQLDEV` (replacing `SQLDEV` with your chosen instance name).
          * **Authentication:** Select "Windows Authentication."
          * Click "Connect."
      * **Verify Collation:** After connecting, open a new query and run:
        ```sql
        SELECT SERVERPROPERTY('Collation') AS ServerCollation;
        ```
        This should show the custom collation you selected.

### 4\. Enable `FILESTREAM` on the named instance and create a database and table to store `FILESTREAM` data.

**Tool:** SQL Server Configuration Manager, SSMS

**Outcome:** `FILESTREAM` is enabled and a table can store large binary objects.

**Steps:**

1.  **Enable `FILESTREAM` via SQL Server Configuration Manager:**

      * Open **SQL Server Configuration Manager**.
      * In the left pane, select "SQL Server Services."
      * In the right pane, right-click on your **named instance** (e.g., "SQL Server (SQLDEV)") and select "Properties."
      * Go to the "**FILESTREAM**" tab.
      * Check "Enable FILESTREAM for Transact-SQL access."
      * Check "Enable FILESTREAM for file I/O streaming access."
      * Check "Enable FILESTREAM for remote client access."
      * Click "OK."
      * **Restart the SQL Server service** for your named instance (e.g., "SQL Server (SQLDEV)") by right-clicking it and selecting "Restart."

2.  **Create a Database with `FILESTREAM` Filegroup in SSMS:**

      * Connect to your **named instance** (e.g., `.\SQLDEV`) in SSMS.
      * Open a new query window.
      * Execute the following T-SQL to create a database with a `FILESTREAM` filegroup:
        ```sql
        USE master;
        GO

        CREATE DATABASE FileStreamDB
        ON
        PRIMARY ( NAME = FileStreamDB_data,
                  FILENAME = 'C:\SQLData\FileStreamDB_data.mdf', -- Adjust path accordingly
                  SIZE = 10MB,
                  MAXSIZE = UNLIMITED,
                  FILEGROWTH = 5MB),
        FILEGROUP FileStreamGroup CONTAINS FILESTREAM (
            NAME = FileStreamDB_FS,
            FILENAME = 'C:\SQLData\FileStreamData' -- This is a DIRECTORY, not a file. Adjust path accordingly
        )
        LOG ON ( NAME = FileStreamDB_log,
                 FILENAME = 'C:\SQLData\FileStreamDB_log.ldf', -- Adjust path accordingly
                 SIZE = 5MB,
                 MAXSIZE = 25MB,
                 FILEGROWTH = 5MB);
        GO
        ```
        **Important:** Make sure the `FILENAME` for the `FILESTREAM` filegroup (`FileStreamData` in the example) points to an *existing and empty directory*. SQL Server will create the necessary subdirectories within it.

3.  **Create a Table to Store `FILESTREAM` Data:**

      * Still connected to your named instance in SSMS, ensure you are using `FileStreamDB`.
      * Execute the following T-SQL to create a table that can store `FILESTREAM` data:
        ```sql
        USE FileStreamDB;
        GO

        CREATE TABLE Documents (
            DocumentID UNIQUEIDENTIFIER ROWGUIDCOL NOT NULL UNIQUE,
            Title NVARCHAR(255),
            FileData VARBINARY(MAX) FILESTREAM NULL,
            ModifiedDate DATETIME
        );
        GO
        ```
      * **Verification:** In Object Explorer, expand `FileStreamDB` -\> "Tables." You should see the `Documents` table. You can also right-click the `Documents` table, select "Design," and verify that `FileData` column has the `FILESTREAM` property.

### 5\. Configure Windows Firewall to allow TCP port 1433 for the default instance and a static port (e.g., 1434) for the named instance.

**Tool:** Windows Defender Firewall with Advanced Security

**Outcome:** Inbound firewall rules created for both instances.

**Steps:**

1.  **Configure Default Instance (TCP 1433):**

      * Open **Windows Defender Firewall with Advanced Security** (Search "Windows Firewall" in Start Menu, then select "Advanced settings").
      * In the left pane, select "Inbound Rules."
      * In the right pane, click "New Rule..."
      * **Rule Type:** Select "Port" and click "Next."
      * **Protocol and Ports:**
          * Select "TCP."
          * Select "Specific local ports" and enter `1433`.
          * Click "Next."
      * **Action:** Select "Allow the connection" and click "Next."
      * **Profile:** Select all profiles (Domain, Private, Public) that apply to your network environment (typically all three for lab purposes) and click "Next."
      * **Name:** Give the rule a descriptive name, e.g., "SQL Server Default Instance (TCP 1433)".
      * Click "Finish."

2.  **Configure Named Instance (Static Port - e.g., 1434):**

      * **Set a Static Port for the Named Instance:** By default, named instances use dynamic ports. To enable a static port for firewall rules, you need to configure it in SQL Server Configuration Manager.

          * Open **SQL Server Configuration Manager**.
          * In the left pane, expand "SQL Server Network Configuration" and select "Protocols for [YourNamedInstance]" (e.g., "Protocols for SQLDEV").
          * In the right pane, double-click "TCP/IP."
          * Go to the "IP Addresses" tab.
          * Scroll down to the "IPAll" section.
          * Clear the "TCP Dynamic Ports" field.
          * In the "TCP Port" field, enter your desired static port, e.g., `1434`.
          * Click "OK."
          * **Restart the SQL Server service** for your named instance (e.g., "SQL Server (SQLDEV)") for this change to take effect.

      * **Create Firewall Rule for Named Instance:**

          * Go back to **Windows Defender Firewall with Advanced Security**.
          * In the left pane, select "Inbound Rules."
          * In the right pane, click "New Rule..."
          * **Rule Type:** Select "Port" and click "Next."
          * **Protocol and Ports:**
              * Select "TCP."
              * Select "Specific local ports" and enter `1434` (or your chosen static port).
              * Click "Next."
          * **Action:** Select "Allow the connection" and click "Next."
          * **Profile:** Select all applicable profiles and click "Next."
          * **Name:** Give the rule a descriptive name, e.g., "SQL Server Named Instance (TCP 1434)".
          * Click "Finish."

### 6\. Enable TLS 1.2 encryption for connections to the default SQL Server instance (using a self-signed certificate for lab purposes).

**Tool:** SQL Server Configuration Manager, SSMS

**Outcome:** SQL Server connections are encrypted via TLS 1.2.

**Steps:**

1.  **Generate a Self-Signed Certificate (for Lab Purposes):**

      * **Using PowerShell (Recommended for ease in a lab):**
          * Open PowerShell as an Administrator.
          * Execute the following command to create a self-signed certificate. Replace `YourComputerName` with the actual name of your SQL Server host.
            ```powershell
            New-SelfSignedCertificate -DnsName "YourComputerName" -CertStoreLocation "Cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(1)
            ```
            **Note:** You can find your computer name by right-clicking "This PC" or "My Computer" and selecting "Properties."
      * **Using `makecert.exe` (Older method, may require Windows SDK):**
          * This method is more involved and `makecert.exe` is deprecated. PowerShell is preferred.

2.  **Assign the Certificate to SQL Server:**

      * Open **SQL Server Configuration Manager**.
      * In the left pane, expand "SQL Server Network Configuration" and select "Protocols for MSSQLSERVER" (for your default instance).
      * In the right pane, double-click "Properties" (or right-click "TCP/IP" and select "Properties").
      * Go to the "**Certificates**" tab.
      * From the "Certificate" dropdown, select the self-signed certificate you just created (it should appear by its subject name, which is your computer name).
      * Click "OK."

3.  **Force Encryption for SQL Server:**

      * Still in "Protocols for MSSQLSERVER," go to the "**Flags**" tab.
      * Change "Force Encryption" to "Yes."
      * Click "OK."

4.  **Restart SQL Server Service:**

      * In SQL Server Configuration Manager, go to "SQL Server Services."
      * Right-click "SQL Server (MSSQLSERVER)" and select "Restart."

5.  **Verify Encryption Status in SSMS:**

      * Open SSMS.
      * **Connect to your default instance** (e.g., `.` or `localhost`).
      * Before clicking "Connect," go to the "Connection Properties" tab in the "Connect to Server" dialog.
      * Check the box "Encrypt connection."
      * Click "Connect."
      * **Verification:** After connecting, open a new query and run the following:
        ```sql
        SELECT net_transport, encrypt_option, auth_scheme
        FROM sys.dm_exec_connections
        WHERE session_id = @@SPID;
        ```
        The `encrypt_option` column should show `TRUE`, indicating an encrypted connection.

### 7\. Connect to both the default and named instances from a remote machine using SSMS, verifying successful connectivity and encryption status.

**Tool:** SSMS (on client machine)

**Outcome:** Remote connections are successful and encrypted where configured.

**Steps:**

1.  **On the Remote Client Machine:**

      * **Install SSMS:** Ensure SQL Server Management Studio (SSMS) is installed on the remote machine.
      * **Verify Network Connectivity:** Ping the SQL Server host from the remote machine using its hostname or IP address to confirm basic network reachability.
        ```cmd
        ping <SQL_SERVER_HOSTNAME_OR_IP>
        ```

2.  **Connect to the Default Instance (with Encryption):**

      * Open SSMS on the remote machine.
      * In the "Connect to Server" dialog:
          * **Server name:** Enter `<SQL_SERVER_HOSTNAME_OR_IP>` (e.g., `192.168.1.100` or `MySqlHost`).
          * **Authentication:** Select "Windows Authentication" (assuming your remote machine is on the same domain or has appropriate credentials configured). If not, use "SQL Server Authentication" with the `sa` account or another SQL login.
          * Go to the "Connection Properties" tab.
          * **Check "Encrypt connection."** (This is crucial to verify the TLS 1.2 encryption enabled in the previous step).
          * Click "Connect."
      * **Verify Encryption:** Once connected, open a new query and run:
        ```sql
        SELECT net_transport, encrypt_option, auth_scheme
        FROM sys.dm_exec_connections
        WHERE session_id = @@SPID;
        ```
        `encrypt_option` should be `TRUE`.

3.  **Connect to the Named Instance (Static Port):**

      * Open SSMS on the remote machine.
      * In the "Connect to Server" dialog:
          * **Server name:** Enter `<SQL_SERVER_HOSTNAME_OR_IP>,<STATIC_PORT>` (e.g., `192.168.1.100,1434` or `MySqlHost,1434`).
          * **Authentication:** Select "Windows Authentication" or "SQL Server Authentication" as appropriate.
          * **Leave "Encrypt connection" unchecked** unless you explicitly configured encryption for the named instance (which was not part of this specific lab exercise for the named instance).
          * Click "Connect."
      * **Verify Connectivity:** Once connected, you should be able to browse databases and objects on the named instance. Open a new query and run:
        ```sql
        SELECT @@SERVERNAME;
        ```
        This should return the full server name including the instance name (e.g., `YOUR_COMPUTER_NAME\SQLDEV`).

This comprehensive guide should help you perform each of the lab exercises successfully on your Microsoft SQL Server instances.

## Assessments

### Knowledge Checks (MCQs)

1.  **Which SQL Server Edition is best suited for a development and testing environment with full features, without licensing costs?**

      - A) Express Edition
      - B) Standard Edition
      - C) Enterprise Edition
      - D) Developer Edition
      - **Answer:** D
      - **Explanation:** Developer Edition is free and includes all features of Enterprise Edition, but is licensed only for development and testing.

2.  **What is the primary benefit of configuring multiple data files for `tempdb`?**

      - A) To increase the total storage capacity of `tempdb`.
      - B) To reduce contention on `tempdb` allocation pages (PFS, GAM, SGAM).
      - C) To enable `tempdb` to be backed up and restored.
      - D) To allow `tempdb` to be used by `FILESTREAM`.
      - **Answer:** B
      - **Explanation:** Multiple `tempdb` data files help distribute I/O operations and reduce contention on shared internal resources, improving performance, especially on multi-core systems.

3.  **Which of the following is the most secure authentication mode for SQL Server in a domain environment?**

      - A) SQL Server Authentication
      - B) Mixed Mode Authentication
      - C) Windows Authentication
      - D) Basic Authentication
      - **Answer:** C
      - **Explanation:** Windows Authentication leverages the robust security infrastructure of Active Directory, offering centralized user management, strong password policies, and Kerberos support, making it the most secure option in domain environments.

4.  **When configuring SQL Server network settings, which protocol is most commonly used for client connections over a network?**

      - A) Named Pipes
      - B) Shared Memory
      - C) TCP/IP
      - D) VIA
      - **Answer:** C
      - **Explanation:** TCP/IP is the standard and most widely used network protocol for connecting to SQL Server over a network.

### Short Answer Questions

1.  Explain the difference between a SQL Server "default instance" and a "named instance," including how they are identified for connectivity.
2.  Describe the steps involved in enabling TLS encryption for a SQL Server instance, and explain why disabling older TLS/SSL protocols is a critical security practice.
3.  Outline the importance of using low-privilege service accounts for SQL Server services and provide an example of a potential security risk if this best practice is ignored.

### Practical Task

  - **Description of practical assignment**: Design and implement a multi-instance SQL Server environment on a single Windows Server, including:
    1.  A default instance of SQL Server 2019 Developer Edition configured with Windows Authentication only, a specific collation (e.g., Latin1\_General\_CI\_AS), and `max server memory` set to 8GB.
    2.  A named instance of SQL Server 2019 Developer Edition with SQL Server Authentication enabled, a different collation (e.g., SQL\_Latin1\_General\_CP1\_CI\_AS), and `FILESTREAM` enabled.
    3.  Configure `tempdb` on *both* instances to have 8 data files, each 1GB in size, located on a separate drive (simulated if necessary).
    4.  Create inbound firewall rules for both instances to allow remote TCP/IP connections.
    5.  Perform an upgrade of a sample database from the default instance to the named instance using a backup/restore method, and update the compatibility level of the restored database.
    6.  Document all configuration changes and verify connectivity to both instances from a remote machine using SSMS, noting the authentication method used for each connection.
