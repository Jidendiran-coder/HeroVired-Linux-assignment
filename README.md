# **Task 1: System Monitoring Setup** 🚀  

This plan ensures **continuous system monitoring**, **log collection**, and **reporting** to meet the task requirements.  

## **🛠️ Step 1: Install Monitoring Tools**  
Install `htop` and `nmon` for real-time monitoring:  
```bash
sudo apt update && sudo apt install -y htop nmon
```
- Run `htop` for interactive CPU & memory usage monitoring.  

---

## **📝 Step 2: Set Up Automated System Logging using cron jobs**  

## Installation Steps

### 1. Create Log Directories

The log files will be stored in their respective directories. Create the directories first:

```bash
sudo mkdir -p /var/log/disk_usage
sudo mkdir -p /var/log/process_usage
sudo mkdir -p /var/log/top_processes
sudo mkdir -p /var/log/memory_usage
```

### 2. Create the Logging Scripts

Next, create the four scripts to log system data.

#### a. **log_disk_usage.sh**

- Purpose: Logs disk usage to `/var/log/disk_usage/` every 5 minutes.
- The log file will be named with a timestamp.

- create file `/usr/local/bin//usr/local/bin/log_disk_usage.sh` with the below script

```bash
#!/bin/bash

# Directory for disk usage logs
LOG_DIR="/var/log/disk_usage"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/disk_usage_$current_time.log"

# Append disk usage to the log file
echo "🗂️ Disk Usage Report - $current_time" >> $LOG_FILE
df -h >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```

#### b. **log_process_usage.sh**

- Purpose: Logs the top 10 processes sorted by memory usage to `/var/log/process_usage/` every 10 minutes.

- create file `/usr/local/bin/log_process_usage.sh` with the below script
```bash
#!/bin/bash

# Directory for process usage logs
LOG_DIR="/var/log/process_usage"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/process_usage_$current_time.log"

# Append process usage to the log file
echo "⚙️ Process Usage Report - $current_time" >> $LOG_FILE
ps aux --sort=-%mem | head -10 >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```

#### c. **log_top_processes.sh**

- Purpose: Logs the top 15 CPU-consuming processes to `/var/log/top_processes/` every 15 minutes.

- create file `/usr/local/bin/log_top_processes.sh` with the below script
```bash
#!/bin/bash

# Directory for top processes logs
LOG_DIR="/var/log/top_processes"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/top_processes_$current_time.log"

# Append top processes to the log file
echo "🚨 High Resource Usage Report - $current_time" >> $LOG_FILE
top -b -o +%CPU | head -15 >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```
#### d. **log_memory_usage.sh**

- purpose: Logs memory usage statistics to /var/log/memory_usage/ every 1 minutes.

- create file `/usr/local/bin/log_memory_usage.sh` with the below script
```bash
#!/bin/bash

# Directory for memory usage logs
LOG_DIR="/var/log/memory_usage"

# Ensure the directory exists
mkdir -p $LOG_DIR

# Store the current time
current_time=$(date "+%Y-%m-%d %H:%M:%S")

# Define log file with timestamp
LOG_FILE="$LOG_DIR/memory_usage_$current_time.log"

# Append memory usage to the log file
echo "🧠 Memory Usage Report - $current_time" >> $LOG_FILE
free -h >> $LOG_FILE
echo "-----------------------------------" >> $LOG_FILE

# Remove entries older than 7 logs
find $LOG_DIR -type f -name "*.log" | sort | head -n -7 | xargs rm -f
```
### 3. Make Scripts Executable

Once the scripts are created, ensure they are executable:

```bash
sudo chmod +x /usr/local/bin/log_disk_usage.sh
sudo chmod +x /usr/local/bin/log_process_usage.sh
sudo chmod +x /usr/local/bin/log_top_processes.sh
sudo chmod +x /usr/local/bin/log_memory_usage.sh
```

### 4. Set Up Cron Jobs

To schedule the scripts to run automatically, we will configure cron jobs.

1. Open the crontab file:

   ```bash
   crontab -e
   ```

2. Add the following lines to the crontab file to run the scripts at the desired intervals:

```bash
# Disk Usage every 5 minutes
* * * * * /usr/local/bin/log_disk_usage.sh

# Process Usage every 10 minutes
* * * * * /usr/local/bin/log_process_usage.sh

# Top Processes every 15 minutes
* * * * * /usr/local/bin/log_top_processes.sh

# Memory Usage every 15 minutes
*/15 * * * * /usr/local/bin/log_memory_usage.sh
```
## crontab jobs Screenshot:
![image](https://github.com/user-attachments/assets/808f69d0-73c0-4fc2-8139-71bc973dfc36)

### 5. Test Cron Jobs

To verify that the cron jobs
## Disk Usage Logs:
![image](https://github.com/user-attachments/assets/12cc1348-c94b-4c6d-b06e-fc56c23b70ea)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/9769cc2e-a118-4272-af72-848e47859171)
-------------------------------------------------------------------------------------------
## Process usage
![image](https://github.com/user-attachments/assets/7b3eecb8-16f5-400b-bb17-06128df60548)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/365cb9b4-aa0c-4e45-99da-018c5407c541)
-------------------------------------------------------------------------------------------
## Top Processes Usage
![image](https://github.com/user-attachments/assets/4c499147-42d9-424b-9e8e-5060953d8e1d)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/8d7d9cb1-48f3-424a-8e8d-02c3a3dcc06a)
-------------------------------------------------------------------------------------------
## Memory Usage log
![image](https://github.com/user-attachments/assets/59bf5772-8378-47e1-9a76-183f6944ead7)
## 7 numbers retention:
![image](https://github.com/user-attachments/assets/0e71d058-9e83-4a3e-a6b1-4f829e12dc3c)
-------------------------------------------------------------------------------------------

