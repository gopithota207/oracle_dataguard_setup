Oracle Data Guard Automation Script is a production-ready shell script designed to automate the complete setup of an Oracle Physical Standby Database from a Primary database environment. The script performs end-to-end Oracle Data Guard configuration using RMAN Active Duplicate and Data Guard Broker, reducing manual effort and minimizing configuration errors.

The automation supports Oracle Database versions 12c, 18c, 19c, and 21c on Linux operating systems including RHEL, Oracle Linux, CentOS, and Ubuntu.

The script automatically performs all major Data Guard setup activities, including:

Pre-flight environment validation
ARCHIVELOG mode configuration
FORCE LOGGING enablement
Data Guard parameter configuration
Standby Redo Log creation
Password file synchronization
TNS and listener configuration
Standby server directory preparation
RMAN Active Database Duplicate
Managed Recovery Process (MRP) startup
Data Guard Broker configuration
Protection mode setup
Final Data Guard health verification

The solution is designed for production environments and helps DBAs deploy Oracle Physical Standby databases quickly and consistently using a single command execution.

Prerequisites include:

Oracle software installed on both primary and standby servers
Same Oracle version on both servers
Passwordless SSH connectivity between servers
Listener communication enabled between hosts
Primary database in OPEN state
Sufficient storage available on standby server

The script supports configurable parameters such as:

Primary and standby database SID
ORACLE_HOME paths
Hostnames and listener ports
Data Guard protection modes
Optional email notification reports

This automation is intended for Oracle DBAs and infrastructure teams looking to standardize and simplify Oracle Data Guard deployments across enterprise environments.




