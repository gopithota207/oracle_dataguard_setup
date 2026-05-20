Oracle Data Guard Automation Script - Primary to Standby Setup
A production-ready, end-to-end Oracle Data Guard automation script that sets up Physical Standby Database from scratch — covering archivelog mode, parameter configuration, RMAN duplicate, MRP startup, Broker setup, and full verification — all in a single command.
---
Supported Versions
Oracle 12c - Supported
Oracle 18c - Supported
Oracle 19c - Supported
Oracle 21c - Supported
OS: Linux (RHEL / OEL / CentOS / Ubuntu)
Standby Type: Physical Standby Database
Duplicate Method: RMAN Active Database Duplicate (network-based)
---
What It Does - Step by Step
Step 0  - Pre-flight Checks          - Validates root, SSH connectivity, disk space, DB status
Step 1  - Enable ARCHIVELOG Mode     - Switches primary to ARCHIVELOG if not already set
Step 2  - Enable FORCE LOGGING       - Enables FORCE LOGGING on primary for complete redo capture
Step 3  - Configure Primary Params   - Sets all Data Guard init parameters on primary
Step 4  - Add Standby Redo Logs      - Adds 4 Standby Redo Log groups on primary
Step 5  - Copy Password File         - Copies orapw file from primary to standby via SCP
Step 6  - Create Standby PFILE       - Generates and copies init.ora for standby database
Step 7  - Configure TNS              - Sets up tnsnames.ora and listener.ora on both nodes
Step 8  - Prepare Standby Dirs       - Creates all required directories on standby server
Step 9  - Start Standby in NOMOUNT  - Starts standby instance in NOMOUNT for duplication
Step 10 - RMAN Duplicate             - Clones primary to standby using active database duplicate
Step 11 - Standby Redo Logs         - Adds Standby Redo Log groups on standby database
Step 12 - Start MRP                  - Starts Managed Recovery Process for redo apply
Step 13 - Configure DG Broker        - Sets up DGMGRL broker configuration
Step 14 - Set Protection Mode        - Configures MaxProtection / MaxAvailability / MaxPerformance
Step 15 - Verify Data Guard          - Validates roles, MRP status, archive gap, applied logs
---
Repository Structure
oracle-dataguard-automation/
├── oracle_dataguard_setup.sh     # Main Data Guard setup script
└── README.md                     # This file
---
Prerequisites
Both servers must have Oracle software installed (same version)
Passwordless SSH must be configured from primary to standby for oracle user
Both servers must be able to reach each other on the listener port (default 1521)
Primary database must be in OPEN status before running the script
Sufficient disk space on standby (at least equal to primary DB size)
Setup passwordless SSH (run on primary as oracle user):
ssh-keygen -t rsa
ssh-copy-id oracle@standby-db-host
---
Configuration
Edit the configuration section at the top of oracle_dataguard_setup.sh:
PRIMARY_ORACLE_HOME="/u01/app/oracle/product/19.3.0/dbhome_1"
PRIMARY_ORACLE_BASE="/u01/app/oracle"
PRIMARY_SID="ORCL"
PRIMARY_DB_UNIQUE_NAME="ORCL_PRIMARY"
PRIMARY_HOST="primary-db-host"
PRIMARY_PORT="1521"
STANDBY_ORACLE_HOME="/u01/app/oracle/product/19.3.0/dbhome_1"
STANDBY_ORACLE_BASE="/u01/app/oracle"
STANDBY_SID="ORCLSTBY"
STANDBY_DB_UNIQUE_NAME="ORCL_STANDBY"
STANDBY_HOST="standby-db-host"
STANDBY_PORT="1521"
DG_PROTECTION_MODE="MaxPerformance"    # MaxProtection / MaxAvailability / MaxPerformance
MAIL_TO="dba@yourcompany.com"          # Leave blank to skip email
---
Usage
Basic Run:
sudo ./oracle_dataguard_setup.sh
With options:
sudo ./oracle_dataguard_setup.sh   
--primary-sid PROD   
--standby-sid PRODSTBY   
--primary-host db-primary   
--standby-host db-standby   
--protection-mode MaxAvailability   
--mail dba@example.com
All Available Options:
--primary-sid     SID     Primary database SID
--standby-sid     SID     Standby database SID
--primary-host    HOST    Primary server hostname
--standby-host    HOST    Standby server hostname
--primary-home    PATH    Primary ORACLE_HOME path
--standby-home    PATH    Standby ORACLE_HOME path
--protection-mode MODE    MaxProtection / MaxAvailability / MaxPerformance
--mail            EMAIL   Send report to this email
--help                    Show help
---

Sample Output
========================================================================
Oracle Data Guard Setup  |  PRIMARY: ORCL  to  STANDBY: ORCLSTBY
[INFO]  STEP 0 - Pre-flight Checks
[OK]    SSH connectivity to standby-host - OK
[OK]    Primary database status: OPEN
[OK]    Pre-flight checks complete.
[INFO]  STEP 1 - Enable ARCHIVELOG Mode
[OK]    Primary is already in ARCHIVELOG mode.
[INFO]  STEP 3 - Configure Primary Database Parameters
[OK]    Primary database parameters configured.
[INFO]  STEP 10 - RMAN Duplicate Primary to Standby
[OK]    RMAN Duplicate completed successfully.
[INFO]  STEP 12 - Start Managed Recovery Process (MRP)
[OK]    MRP0 is running: MRP0|APPLYING_LOG
[INFO]  STEP 15 - Verify Data Guard Setup
NAME        ROLE             OPEN_MODE   PROTECTION_MODE
----------- ---------------- ----------- --------------------




