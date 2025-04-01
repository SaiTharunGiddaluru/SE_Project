Software Architecture Document (SAD)
Project Name: System Health Monitoring & Automated Backup Solution
1. Introduction
Objective:
This project automates system health monitoring and backup processes in Linux. It:

Monitors CPU, memory, and disk usage, logging alerts if thresholds are exceeded.

Backs up important files to a remote server via rsync.

Logs activities to /var/log/system_health.log and /var/log/backup.log.

2. Software Design Principles
2.1 Abstraction
The system has two primary functionalities:

System Health Monitoring – Captures system resource usage and logs alerts.

Automated Backup – Transfers files securely using rsync.

2.2 Encapsulation
Each function is grouped into separate scripts:

monitor.sh – Monitors system health and logs alerts.

backup.sh – Automates file backup.

2.3 Modularity
The system consists of independent modules:

monitor.sh → Collects CPU, memory, and disk statistics.

backup.sh → Automates file backup to a remote server.

2.4 Cohesion & Coupling
High Cohesion: Each script is responsible for a single functionality.

Low Coupling: The scripts interact only through log files.

3. System Design
3.1 Data Flow Diagram (DFD)
System Monitoring (monitor.sh)

Fetches CPU, memory, and disk usage.

Logs alerts to /var/log/system_health.log.

Automated Backup (backup.sh)

Syncs files using rsync.

Logs success/failure in /var/log/backup.log.

3.2 Deployment Design
Installation & Execution
bash
Copy
Edit
# Clone repository
git clone https://github.com/your-repo/system-health-monitor.git

# Make scripts executable
chmod +x monitor.sh backup.sh

# Configure backup settings in backup.sh
nano backup.sh  

# Run system monitoring
./monitor.sh  

# Run backup script manually or via cron job
./backup.sh  

# (Optional) Automate execution via cron
crontab -e  
# Add:
*/5 * * * * /path/to/monitor.sh  
0 2 * * * /path/to/backup.sh  
4. Shell Script Implementation
4.1 monitor.sh (System Health Monitoring)
Checks CPU, memory, and disk usage.

Logs alerts if thresholds (80%) are exceeded.

Records running processes in /var/log/system_health.log.

Code Snippet:
bash
Copy
Edit
#!/bin/bash

CPU_THRESHOLD=80
MEMORY_THRESHOLD=80
DISK_THRESHOLD=80

CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print 100 - $1}')
if (( $(echo "$CPU_USAGE > $CPU_THRESHOLD" | bc -l) )); then
    echo "ALERT: CPU usage high: $CPU_USAGE%" >> /var/log/system_health.log
fi

MEMORY_USAGE=$(free | grep Mem | awk '{print $3/$2 * 100.0}')
if (( $(echo "$MEMORY_USAGE > $MEMORY_THRESHOLD" | bc -l) )); then
    echo "ALERT: Memory usage high: $MEMORY_USAGE%" >> /var/log/system_health.log
fi

DISK_USAGE=$(df / | grep / | awk '{print $5}' | sed 's/%//g')
if [ $DISK_USAGE -gt $DISK_THRESHOLD ]; then
    echo "ALERT: Disk space low: $DISK_USAGE%" >> /var/log/system_health.log
fi

ps aux >> /var/log/system_health.log
echo "Health check completed at $(date)" >> /var/log/system_health.log
4.2 backup.sh (Automated Backup Solution)
Uses rsync to sync files to a remote server.

Logs success/failure in /var/log/backup.log.

Code Snippet:
bash
Copy
Edit
#!/bin/bash

SOURCE_DIR="/path/to/source_directory"
REMOTE_USER="remote_user"
REMOTE_HOST="remote_host"
REMOTE_DIR="/path/to/remote_directory"
LOG_FILE="/var/log/backup.log"

rsync -avz --delete $SOURCE_DIR $REMOTE_USER@$REMOTE_HOST:$REMOTE_DIR

if [ $? -eq 0 ]; then
    echo "Backup succeeded at $(date)" >> $LOG_FILE
else
    echo "Backup failed at $(date)" >> $LOG_FILE
fi
5. Performance Testing & Risk Management
5.1 Performance Testing
Tools Used:

top, htop – Monitor CPU & memory usage.

time – Measure script execution time.

5.2 Risk Analysis
Risk	Type	Mitigation Strategy
Incorrect log parsing	Operational	Test grep and awk patterns before deployment.
Backup failure	Technical	Ensure correct rsync setup & SSH keys.
High CPU usage	Performance	Optimize loops & reduce log frequency.
6. Conclusion
This System Health Monitoring & Backup Automation project provides a robust, modular, and automated solution to track system performance and secure important files efficiently.
