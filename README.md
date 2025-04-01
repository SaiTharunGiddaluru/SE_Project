# Software Architecture Document (SAD) for Linux-Based Automation System

## 1. Introduction

### 1.1 Objective
This project aims to automate system backups and health monitoring on Linux-based systems using shell scripting and system utilities. The automation system ensures data integrity by regularly synchronizing directories to a remote location and monitoring critical system resources such as CPU, memory, and disk usage.

### 1.2 Scope
The automation system consists of two primary scripts:
- *Backup Automation Script*: Utilizes rsync to sync files between local and remote directories and logs success or failure.
- *System Health Monitoring Script*: Monitors CPU, memory, and disk usage, logs running processes, and alerts the user if thresholds are exceeded.

## 2. Software Design Principles

### 2.1 Abstraction
- *Backup Functionality*: High-level backup operation abstracts the underlying rsync commands.
- *Monitoring Functionality*: Abstracts system health checks into distinct operations for CPU, memory, and disk.

### 2.2 Encapsulation
- Each script encapsulates related operations: backup tasks are handled by one script, and system monitoring is managed separately.
- Logs are stored in predefined locations to maintain integrity.

### 2.3 Modularity
- The backup script and monitoring script operate independently, making them reusable in different setups.
- Each script performs a single responsibility, ensuring maintainability.

### 2.4 Cohesion & Coupling
- *High Cohesion*: Each script performs a well-defined function.
- *Low Coupling*: The scripts do not depend on each other and can run independently.

## 3. System Design

### 3.1 Data Flow Diagram (DFD)
#### Level 0 - High-Level Data Flow
1. User initiates the backup script.
2. The script syncs files to the remote server.
3. The script logs success/failure messages.
4. The monitoring script checks CPU, memory, and disk usage periodically.
5. Alerts are logged if thresholds are exceeded.

### 3.2 Class Diagram (For OOP Implementation in Python - Optional)
- BackupManager: Manages file synchronization.
- SystemMonitor: Monitors system health parameters.
- Logger: Handles logging operations.

## 4. Deployment Design

### 4.1 Installation Instructions

#### Backup Script Deployment
- Place the script in /usr/local/bin/backup.sh
- Set execution permission:
  bash
  chmod +x /usr/local/bin/backup.sh
  
- Schedule with crontab -e:
  bash
  0 2 * * * /usr/local/bin/backup.sh
  

#### System Monitoring Script Deployment
- Place the script in /usr/local/bin/system_health.sh
- Set execution permission:
  bash
  chmod +x /usr/local/bin/system_health.sh
  
- Schedule with crontab -e:
  bash
  */10 * * * * /usr/local/bin/system_health.sh
  

## 5. Conclusion
This automation system provides an efficient way to manage backups and monitor system health. By adhering to software engineering design principles, it ensures modularity, maintainability, and reliability in a Linux environment.
