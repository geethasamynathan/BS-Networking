<div align="center">

# 🐧 LINUX UBUNTU — COMPLETE COMMANDS TUTORIAL
### From File Management to System Administration
**Navigation · Text Processing · Permissions · Processes · Networking · Services · Backups · Automation**

</div>

> **Practice environment:** Ubuntu / WSL with Bash. Commands and example outputs are separated for easy reading. Bash code blocks receive syntax highlighting in supported Markdown viewers.

## 🗺️ 1. Learning Roadmap

| Stage | Topic | Outcome |
|---|---|---|
| 1 | Creating files and folders | Create project files and directories |
| 2 | Navigation and file operations | Navigate, copy, move and delete |
| 3 | Viewing and editing content | Read and update text files |
| 4 | Searching and processing | Search, sort and summarize records |
| 5 | Permissions and accounts | Protect files and manage access |
| 6 | Processes and background jobs | Run and control programs |
| 7 | System monitoring | Check disk, memory and CPU |
| 8 | Networking, SSH and SCP | Diagnose connections, access servers |
| 9 | Packages, services and logs | Install and manage applications |
| 10 | Archives and backups | Compress, restore and synchronize |
| 11 | Shell scripts and scheduling | Automate recurring jobs |

## 🏗️ 2. Prepare the Employee Management Project

**Scenario:** Store employee records, generate departmental reports, inspect application errors, and perform backups.

```bash
cd ~/Linuxtraining
mkdir -p EmployeeManagement/{Employees,Departments,Logs,Backups,Scripts,Reports,Archive}
cd EmployeeManagement
pwd
```

**Expected directory:** `/home/geetha/Linuxtraining/EmployeeManagement` (if the username is `geetha`).

Create data:

```bash
cat > Employees/employees.csv <<'EOF_DATA'
ID,Name,Department,Salary,City
101,Ishwa Bharath,IT,50000,Chennai
102,Geetha,HR,45000,Coimbatore
103,Ravi,IT,60000,Bangalore
104,Meena,Finance,55000,Chennai
105,Arun,IT,70000,Hyderabad
106,Priya,HR,48000,Chennai
EOF_DATA
```

Create application logs:

```bash
cat > Logs/application.log <<'EOF_LOG'
2026-10-28 09:00:00 INFO Application started
2026-10-28 09:01:00 INFO Employee data loaded
2026-10-28 09:02:00 ERROR Database connection failed
2026-10-28 09:03:00 WARN Retrying database connection
2026-10-28 09:04:00 INFO Database connected
2026-10-28 09:05:00 ERROR Employee record not found
EOF_LOG
```

**Project architecture:**

```text
EmployeeManagement/
├── Employees/
│   └── employees.csv
├── Departments/
├── Logs/
│   └── application.log
├── Backups/
├── Scripts/
├── Reports/
└── Archive/
```

> Unless otherwise stated, run commands from `~/Linuxtraining/EmployeeManagement`.

## 📁 3. Navigation and File Operations

### 3.1 `pwd` — Print working directory

```bash
pwd
```

**Example output:**

```text
/home/geetha/Linuxtraining/EmployeeManagement
```

### 3.2 `ls` — List directory contents

```bash
ls        # Names
ls -l     # Detailed listing
ls -la    # Detailed listing including hidden entries
ls -lh    # Human-readable sizes
ls -lt    # Newest-modified first
ls -R     # Recursive listing
```

### 3.3 `cd` — Change directory

```bash
cd Employees
pwd
cd ..
cd Logs
cd -       # Previous directory
cd ~       # Home directory
cd ~/Linuxtraining/EmployeeManagement
```

### 3.4 `cp` — Copy files and directories

```bash
cp Employees/employees.csv Backups/employees_backup.csv
ls Backups
```

**Output:**

```text
employees_backup.csv
```

Variants:

```bash
cp -i Employees/employees.csv Backups/   # Ask before overwriting
cp -v Employees/employees.csv Reports/   # Verbose copy
cp -r Employees Archive/                # Recursive directory copy
cp -a Employees Archive/EmployeesSnapshot # Preserve metadata where possible
```

### 3.5 `mv` — Rename or move

```bash
cp Employees/employees.csv Reports/temp.csv
mv Reports/temp.csv Reports/employee_report.csv
mv Reports/employee_report.csv Archive/
ls Archive
```

### 3.6 `rm` and `rmdir` — Remove files and directories

```bash
touch Reports/unwanted.txt
rm -i Reports/unwanted.txt
mkdir Reports/Temporary
rmdir Reports/Temporary
mkdir -p Reports/TestFolder
touch Reports/TestFolder/test.txt
rm -ri Reports/TestFolder
```

> ⚠️ **Safety:** `rm` ordinarily does not use a Recycle Bin. Check paths carefully; `rm -ri` asks for confirmation during recursive deletion.

### 3.7 `stat` and `file` — Inspect files

```bash
stat Employees/employees.csv
file Employees/employees.csv
```

`stat` displays metadata such as size, permissions and timestamps. `file` examines data to infer its type.

### 3.8 `ln` — Create links

```bash
ln -s Employees/employees.csv employees-link.csv
readlink employees-link.csv
```

A symbolic link points to another path. The example works when the link is created from the project root.

## 📖 4. Displaying and Updating File Content

### 4.1 `cat`

```bash
cat Employees/employees.csv
```

**Output:**

```text
ID,Name,Department,Salary,City
101,Ishwa Bharath,IT,50000,Chennai
102,Geetha,HR,45000,Coimbatore
103,Ravi,IT,60000,Bangalore
104,Meena,Finance,55000,Chennai
105,Arun,IT,70000,Hyderabad
106,Priya,HR,48000,Chennai
```

### 4.2 `head` and `tail`

```bash
head -n 3 Employees/employees.csv
tail -n 2 Employees/employees.csv
tail -f Logs/application.log    # Follow new log entries; Ctrl+C to stop
```

**First three lines:**

```text
ID,Name,Department,Salary,City
101,Ishwa Bharath,IT,50000,Chennai
102,Geetha,HR,45000,Coimbatore
```

**Last two lines:**

```text
105,Arun,IT,70000,Hyderabad
106,Priya,HR,48000,Chennai
```

### 4.3 `less` and `nl`

```bash
less Employees/employees.csv   # Press Space, search /IT, exit q
nl -ba Employees/employees.csv # Number all lines
```

### 4.4 Overwrite versus append

```bash
echo "Employee Report" > Reports/summary.txt
echo "Department: IT" >> Reports/summary.txt
echo "Status: Active" >> Reports/summary.txt
cat Reports/summary.txt
```

**Output:**

```text
Employee Report
Department: IT
Status: Active
```

`>` creates or replaces content; `>>` creates or appends. These operators are part of shell redirection.

### 4.5 Append on the same line with `printf`

```bash
printf "Employee: Ishwa Bharath" > Reports/employee_details.txt
printf " | Department: IT" >> Reports/employee_details.txt
printf " | Salary: 50000\n" >> Reports/employee_details.txt
cat Reports/employee_details.txt
```

**Output:**

```text
Employee: Ishwa Bharath | Department: IT | Salary: 50000
```

`printf` does not add a newline unless requested. `>>` itself does not remove an existing newline.

## 🔎 5. Finding Files and Searching Text

### 5.1 `grep` — Find IT employees

```bash
grep ",IT," Employees/employees.csv
```

**Output:**

```text
101,Ishwa Bharath,IT,50000,Chennai
103,Ravi,IT,60000,Bangalore
105,Arun,IT,70000,Hyderabad
```

Useful forms:

```bash
grep -i "chennai" Employees/employees.csv  # Ignore case
grep -n "IT" Employees/employees.csv       # Line numbers
grep -c ",IT," Employees/employees.csv    # Count matching lines
grep -v ",IT," Employees/employees.csv    # Nonmatching lines
grep -w "ERROR" Logs/application.log      # Whole-word matching
grep -r "ERROR" Logs/                    # Recursive directory search
```

Search both error and warning messages:

```bash
grep -E "ERROR|WARN" Logs/application.log
```

**Output:**

```text
2026-10-28 09:02:00 ERROR Database connection failed
2026-10-28 09:03:00 WARN Retrying database connection
2026-10-28 09:05:00 ERROR Employee record not found
```

### 5.2 `find` — Search filesystem entries

```bash
find . -name "*.csv"          # CSV files
find . -type f -name "*.log"  # Log files
find . -type d                # Directories
find . -type f -empty         # Empty files
find . -type f -mmin -1440    # Modified in past 1440 minutes
find . -type f -size +1M      # Files larger than 1 MiB approx.
```

### 5.3 Locate commands

```bash
which python3
type cd
whereis bash
```

`which` searches executables in `PATH`; `type` also identifies shell built-ins; `whereis` locates related binaries and manuals.

## 📊 6. Text Processing and Reports

### 6.1 `wc` — Count records

```bash
wc -l Employees/employees.csv
tail -n +2 Employees/employees.csv | wc -l
```

**Outputs:**

```text
7 Employees/employees.csv
6
```

The CSV has one header plus six employees.

### 6.2 `cut` — Select fields

```bash
cut -d',' -f2 Employees/employees.csv
cut -d',' -f2,4 Employees/employees.csv
```

`-d','` sets comma as delimiter, and `-f2,4` retrieves name and salary columns. These simple examples assume comma-free field values; real quoted CSVs need CSV-aware parsing.

### 6.3 `sort` — Sort by salary descending

```bash
tail -n +2 Employees/employees.csv | sort -t',' -k4,4nr
```

**Output:**

```text
105,Arun,IT,70000,Hyderabad
103,Ravi,IT,60000,Bangalore
104,Meena,Finance,55000,Chennai
101,Ishwa Bharath,IT,50000,Chennai
106,Priya,HR,48000,Chennai
102,Geetha,HR,45000,Coimbatore
```

`-t','` specifies delimiter, `-k4,4` the salary field, `n` numeric sorting and `r` reverse order.

### 6.4 `uniq` — Count departments

```bash
tail -n +2 Employees/employees.csv | cut -d',' -f3 | sort | uniq -c
```

**Output:**

```text
      1 Finance
      2 HR
      3 IT
```

`uniq` combines *adjacent* matching lines, so sorting first is common.

### 6.5 `awk` — Filter and summarize

Employees earning more than 50,000:

```bash
awk -F',' 'NR>1 && $4>50000 {print $2, $4}' Employees/employees.csv
```

**Output:**

```text
Ravi 60000
Meena 55000
Arun 70000
```

Total salary:

```bash
awk -F',' 'NR>1 {total+=$4} END {print "Total Salary:",total}' Employees/employees.csv
```

**Output:** `Total Salary: 328000`

Average salary:

```bash
awk -F',' 'NR>1 {total+=$4; count++} END {if(count>0) print "Average Salary:",total/count}' Employees/employees.csv
```

**Output:** `Average Salary: 54666.7`

### 6.6 `sed` — Replace text

```bash
sed 's/Bangalore/Bengaluru/' Employees/employees.csv         # Print transformed content
sed -i.bak 's/Bangalore/Bengaluru/' Employees/employees.csv  # Edit file; retain .bak
```

The in-place edit changes later outputs that reference the city of employee 103.

### 6.7 Pipes, redirection and `tee`

**Process flow:**

```text
Employees/employees.csv
          │
          ▼
   grep ",IT,"  → Filter department
          │
          ▼
   cut -d',' -f2 → Extract names
          │
          ▼
       sort       → Alphabetical names
          │
          ▼
  Reports/it_employees.txt
```

```bash
grep ",IT," Employees/employees.csv | cut -d',' -f2 | sort
```

**Output:**

```text
Arun
Ishwa Bharath
Ravi
```

Save the list:

```bash
grep ",IT," Employees/employees.csv | cut -d',' -f2 | sort > Reports/it_employees.txt
```

Save and show log errors simultaneously:

```bash
grep "ERROR" Logs/application.log | tee Reports/errors.txt
```

| Operator | Meaning |
|---|---|
| `>` | Redirect standard output; overwrite |
| `>>` | Redirect standard output; append |
| `<` | Redirect file into standard input |
| `2>` | Redirect standard error |
| `2>>` | Append standard error |
| `2>&1` | Send standard error to current standard-output destination |
| `\|` | Connect command output to next command |

## 🔐 7. Permissions, Ownership and Users

### 7.1 Inspect file permissions

```bash
ls -l Employees/employees.csv
```

**Illustrative output:**

```text
-rw-r--r-- 1 geetha geetha 260 Oct 28 09:30 employees.csv
```

**Permission diagram:**

```text
-rwxr-xr--
│└─┬┘└┬┘└┬┘
│  │  │  └── Others: read
│  │  └───── Group: read + execute
│  └──────── Owner: read + write + execute
└─────────── File type: regular file
```

### 7.2 `chmod` — Change permissions

```bash
echo 'echo "Employee system started"' > Scripts/start.sh
chmod u+x Scripts/start.sh
./Scripts/start.sh
```

**Output:**

```text
Employee system started
```

Examples:

```bash
chmod 700 Scripts/start.sh
chmod 640 Employees/employees.csv
```

| Number | Permission |
|---|---|
| 7 | Read + write + execute |
| 6 | Read + write |
| 5 | Read + execute |
| 4 | Read |
| 0 | No permissions |

### 7.3 `chown`, `chgrp`, `umask`, `sudo`

```bash
sudo chown geetha:geetha Scripts/start.sh
sudo chgrp geetha Scripts/start.sh
umask
sudo apt update
```

Replace `geetha` with an existing user/group on another installation. `sudo` runs commands with privileges allowed by system policy. A common `umask` result is `0022`, but this varies.

### 7.4 Users and groups

```bash
whoami
id
groups
who
w
```

With sufficient administrator privileges, create a sample user and group:

```bash
sudo adduser employee1
sudo groupadd projectteam
sudo usermod -aG projectteam employee1
id employee1
su - employee1
exit
```

New group membership generally takes effect for the user's next login session. `su - employee1` asks for the target account password when required.

## ⚙️ 8. Processes, Jobs and `nohup`

### 8.1 View processes

```bash
ps
ps -ef
ps aux
ps aux | grep python
top   # Press q to quit
```

### 8.2 Run jobs in background and foreground

```bash
sleep 120 &
jobs -l
fg %1
```

When a foreground command is running, `Ctrl+Z` suspends it. Resume the job with:

```bash
bg %1
```

`%1` is an example job number; use the number shown by `jobs`.

### 8.3 Terminate a process

```bash
sleep 300 &
jobs -l
kill PID
```

Replace `PID` with the actual process ID. `kill` normally sends SIGTERM for graceful termination. `kill -9` is a forceful last resort.

### 8.4 `nohup` — Background tasks surviving hangup

```bash
cat > Scripts/background_task.sh <<'EOF_SCRIPT'
#!/bin/bash
for i in {1..5}; do
    echo "Processing batch $i"
    sleep 5
done
EOF_SCRIPT

nohup bash Scripts/background_task.sh > Logs/background.log 2>&1 &
tail -f Logs/background.log
```

`nohup` ignores SIGHUP; production long-running services typically use a service manager such as systemd.

## 💻 9. System Monitoring and Storage

```bash
free -h                         # Memory and swap
df -h                           # Filesystem capacity
du -sh ~/Linuxtraining/EmployeeManagement
du -h --max-depth=1 ~/Linuxtraining/EmployeeManagement
uptime                          # Uptime and load
uname -a                        # Kernel details
hostnamectl                     # Host/system information
lsblk                           # Block devices
date                            # Date and time
```

**Real-world task:** If a server reports that its disk is nearly full, use `df -h` to find the affected filesystem, then `du` to identify large directories. WSL may expose virtualized storage rather than the entire Windows host.

## 🌐 10. Networking, SSH and SCP

**Connection diagram:**

```text
Windows / Linux client
      │  SSH, HTTPS, SCP
      ▼
 LAN / Internet / Firewall / DNS
      │
      ▼
 Ubuntu Linux server
      ├── SSH (commonly TCP 22)
      ├── Nginx (commonly HTTP 80 / HTTPS 443)
      └── Application services
```

### 10.1 Check interfaces and routing

```bash
ip addr
ip -br addr
ip route
```

### 10.2 Test connectivity and DNS

```bash
ping -c 4 8.8.8.8
getent hosts example.com
curl -I https://example.com
```

A successful `ping` does not prove a web service is reachable; ICMP might also be blocked.

### 10.3 Inspect listening sockets

```bash
ss -tuln
sudo ss -tlnp
```

### 10.4 SSH connection

```bash
ssh employee1@192.168.1.100
```

The server IP is illustrative. The destination needs an SSH server and suitable authentication and firewall configuration.

### 10.5 Secure file copy

```bash
scp Employees/employees.csv employee1@192.168.1.100:/home/employee1/
scp employee1@192.168.1.100:/home/employee1/employees.csv .
```

For secure automation, prefer SSH keys and limit server exposure.

## 📦 11. Package Installation, Services and Logs

### 11.1 APT package management

```bash
sudo apt update
apt search nginx
sudo apt install nginx
apt show nginx
sudo apt remove nginx   # When no longer needed
```

### 11.2 `systemctl` — Manage Nginx (on a systemd-enabled system)

```bash
sudo systemctl start nginx
sudo systemctl status nginx
sudo systemctl restart nginx
sudo systemctl stop nginx
sudo systemctl enable nginx
sudo systemctl disable nginx
```

### 11.3 `journalctl` — Examine service logs

```bash
sudo journalctl -u nginx -n 30
sudo journalctl -u nginx -f
```

> **WSL note:** Whether `systemctl` and system services work depends on the distribution's systemd configuration. Verify before using them.

## 🗜️ 12. Archives, Compression and Backup

### 12.1 Create archive

```bash
tar -cvf Archive/employees.tar Employees/
```

`c` creates, `v` prints file names, and `f` supplies the archive filename.

### 12.2 Create gzip-compressed archive

```bash
tar -czvf Archive/employees.tar.gz Employees/
```

### 12.3 Inspect and restore

```bash
tar -tzf Archive/employees.tar.gz
mkdir -p Reports/RestoreTest
tar -xzvf Archive/employees.tar.gz -C Reports/RestoreTest
find Reports/RestoreTest -type f
```

### 12.4 `gzip` / `gunzip`

```bash
gzip -c Logs/application.log > Archive/application.log.gz
gunzip -c Archive/application.log.gz > Reports/restored_application.log
```

### 12.5 `rsync` — Copy and synchronize

Install `rsync` if necessary, then:

```bash
rsync -av Employees/ Backups/EmployeeSync/
```

## 🤖 13. Bash Script: Daily Employee Report and Backup

Create the script:

```bash
nano Scripts/daily_report.sh
```

Paste:

```bash
#!/bin/bash
set -euo pipefail

BASE="$HOME/Linuxtraining/EmployeeManagement"

echo "Generating Employee Report"

{
    echo "EMPLOYEE MANAGEMENT REPORT"
    date
    echo "--------------------------"

    printf "Total Employees: "
    tail -n +2 "$BASE/Employees/employees.csv" | wc -l

    echo "Department Summary:"
    tail -n +2 "$BASE/Employees/employees.csv" |
        cut -d',' -f3 |
        sort |
        uniq -c

    echo "--------------------------"
    echo "Errors Found:"
    grep "ERROR" "$BASE/Logs/application.log" || true

} > "$BASE/Reports/daily_summary.txt"

cp "$BASE/Employees/employees.csv" \
   "$BASE/Backups/employees_daily.csv"

echo "Report and Backup Completed"
```

Save (`Ctrl+O`, Enter) and exit (`Ctrl+X`), then:

```bash
chmod +x Scripts/daily_report.sh
./Scripts/daily_report.sh
cat Reports/daily_summary.txt
```

**Console output:**

```text
Generating Employee Report
Report and Backup Completed
```

The report includes the date, total employee count, department summary, and ERROR messages. The backup contains a copy of the employee CSV.

## ⏰ 14. Schedule Tasks With Cron

```bash
crontab -l  # Inspect current entries
crontab -e  # Edit the user crontab
```

Run the daily report at 9:00 a.m. every day:

```cron
0 9 * * * /bin/bash /home/geetha/Linuxtraining/EmployeeManagement/Scripts/daily_report.sh >> /home/geetha/Linuxtraining/EmployeeManagement/Logs/cron.log 2>&1
```

**Cron expression:**

```text
minute hour day-of-month month day-of-week
   0     9       *         *       *
```

A running cron service is required. The timezone is the cron environment's timezone. In WSL, shutdown or suspension can prevent tasks from running.

## 🧩 15. Complete Workflow Diagram

```text
EmployeeManagement/Employees/employees.csv
                    │
          ┌─────────┴──────────┐
          ▼                    ▼
   Search / reporting      File permissions
   grep, cut, sort, awk    chmod, chown
          │                    │
          └─────────┬──────────┘
                    ▼
           Bash report script
                    │
             ┌──────┴──────┐
             ▼             ▼
     Reports/daily_    Backups/employees_
     summary.txt       daily.csv
             │             │
             └──────┬──────┘
                    ▼
             Cron scheduling
```

## ✅ 16. Practical Lab Checklist

- [ ] Create the EmployeeManagement project directory tree.
- [ ] Use `pwd`, `cd`, `ls` and `tree` to explore directories.
- [ ] Copy, rename, move and remove demonstration files.
- [ ] View files with `cat`, `head`, `tail` and `less`.
- [ ] Find IT employee records and ERROR log entries using `grep`.
- [ ] Search filenames using `find`.
- [ ] Extract names using `cut` and sort salaries using `sort`.
- [ ] Count departments with `uniq -c`.
- [ ] Sum and average salaries using `awk`.
- [ ] Update a sample city using `sed` and inspect its backup.
- [ ] Create an executable script using `chmod`.
- [ ] Start and inspect a background job with `jobs` and `ps`.
- [ ] Inspect memory and storage using `free`, `df` and `du`.
- [ ] Test connectivity with `ping`, `getent`, `curl` and `ss`.
- [ ] If available, install and inspect Nginx with `apt` and `systemctl`.
- [ ] Archive and restore employee data with `tar`.
- [ ] Run the employee-report Bash script and verify its output.
- [ ] Configure cron in an environment that remains running.

## 🧰 17. Additional Useful Command Reference

| Category | Commands | Purpose |
|---|---|---|
| File links | `ln`, `ln -s`, `readlink` | Create and inspect links |
| Advanced permissions | `umask`, `getfacl`, `setfacl` | Permission masks and ACLs |
| Comparison | `diff`, `cmp`, `comm` | Compare files |
| Paths/metadata | `stat`, `file`, `basename`, `dirname` | Inspect paths and files |
| CPU priority | `nice`, `renice` | Adjust scheduling priority |
| Disks | `lsblk`, `mount`, `umount`, `blkid` | Block devices and mounts |
| Network diagnostics | `ss`, `ip`, `traceroute`, `dig` | Diagnose network issues |
| Environment | `env`, `printenv`, `export`, `source` | Manage shell environment |
| Command convenience | `history`, `alias`, `unalias` | History and aliases |
| Hardware | `lscpu`, `lsmem`, `uname` | System information |
| Transfers | `wget`, `curl`, `scp`, `rsync` | Download and transfer |
| Help | `man`, `info`, `whatis`, `--help` | Command documentation |

> ⚠️ Practice commands that modify users, permissions, mounts, network settings, or services on a disposable Linux environment rather than a production machine.

## 📅 18. Recommended Ten-Day Sequence

| Day | Subject | Practical outcome |
|---|---|---|
| 1 | Files, folders and navigation | Project tree |
| 2 | Copy, move, rename, delete and links | Organized records |
| 3 | File viewing and counting | Inspect CSV and logs |
| 4 | Searching and filtering | Extract matching employees |
| 5 | `awk`, `sed` and pipelines | Salary and department reports |
| 6 | Permissions, users and groups | Secure files |
| 7 | Processes and background jobs | Monitor running tasks |
| 8 | Networking, SSH, SCP | Diagnose server access |
| 9 | Services, logs, compression, backups | Restore data |
| 10 | Bash and cron | Scheduled reports |

---

**Next in depth:** File links, wildcards, advanced `find`, ownership and permissions, shell pipelines, and practical troubleshooting.
