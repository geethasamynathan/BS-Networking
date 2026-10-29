<div align="center">

# 🌈 LINUX `ls` COMMANDS
### <span style="color:#1479C9">Professional Hands-on Training Guide</span>
**<span style="color:#0B8F71">Ubuntu • WSL • Beginner to Advanced • 24 Practical Examples</span>**

</div>

> 🟦 **LAB GUIDE** &nbsp; Create files and folders, add content, explore command options, and understand sample output.

---

The **`ls` (list)** command is used to display files and directories in Linux.

We will use your Ubuntu WSL environment and create sample files to understand the different `ls` options.

## 1. Create a Sample Lab

Open Ubuntu Terminal.

### Step 1: Create a directory

```bash
mkdir -p ~/Linuxtraining/LS_Lab
cd ~/Linuxtraining/LS_Lab
```

### Step 2: Create subdirectories

```bash
mkdir Documents Reports Backup Scripts
```

### Step 3: Create files with content

```bash
echo "Employee: Ishwa Bharath" > employee.txt
echo "Employee: Neha" >> employee.txt
echo "Employee: Jatin" >> employee.txt

echo "Department: IT" > department.txt
echo "Department: HR" >> department.txt

echo "Salary: 45000" > salary.txt
echo "Linux Training Notes" > notes.txt
```

### Step 4: Create hidden files

```bash
echo "Environment=Development" > .config
echo "Training configuration" > .settings
```

### Step 5: Create files inside subdirectories

```bash
echo "January Report" > Reports/january.txt
echo "February Report" > Reports/february.txt
echo "Project Documentation" > Documents/project.txt
```

### Directory structure

```text
LS_Lab/
├── Documents/
│   └── project.txt
├── Reports/
│   ├── january.txt
│   └── february.txt
├── Backup/
├── Scripts/
├── employee.txt
├── department.txt
├── salary.txt
├── notes.txt
├── .config
└── .settings
```

Now we can practice different `ls` commands.

---

# 2. Basic `ls` Commands

## Example 1: `ls` — List Files and Directories

**Command:**

```bash
ls
```

**Expected output:**

```text
Backup  department.txt  Documents  employee.txt  notes.txt  Reports  salary.txt  Scripts
```

**Explanation:**

The `ls` command displays the files and directories available in the current working directory.

It does not display hidden files by default.

**Real-world use case:** Check which files are available inside a Linux application directory.

---

## Example 2: `ls -l` — Long Listing Format

**Command:**

```bash
ls -l
```

**Sample output:**

```text
-rw-r--r-- 1 geetha geetha   30 Oct 28 10:00 department.txt
drwxr-xr-x 2 geetha geetha 4096 Oct 28 10:00 Documents
-rw-r--r-- 1 geetha geetha   63 Oct 28 10:00 employee.txt
```

**Explanation:**

The `-l` option stands for **long listing format**.

It displays:

| Column | Meaning |
|---|---|
| `-rw-r--r--` | File permissions |
| `1` | Hard link count |
| `geetha` | Owner |
| `geetha` | Group |
| `63` | File size in bytes |
| `Oct 28 10:00` | Last modification date and time |
| `employee.txt` | Filename |

**Real-world use case:** Check file permissions, ownership, and size.

---

## Example 3: `ls -a` — Display All Files

**Command:**

```bash
ls -a
```

**Expected output:**

```text
.  ..  .config  .settings  Backup  department.txt  Documents  employee.txt  notes.txt  Reports  salary.txt  Scripts
```

**Explanation:**

The `-a` option means **all**.

It displays hidden files and directories.

In Linux, filenames beginning with `.` are hidden from normal `ls` output.

Special entries:

- `.` — Current directory.
- `..` — Parent directory.

**Real-world use case:** Find hidden configuration files such as `.env` and `.gitignore`.

---

## Example 4: `ls -A` — Almost All Files

**Command:**

```bash
ls -A
```

**Expected output:**

```text
.config  .settings  Backup  department.txt  Documents  employee.txt  notes.txt  Reports  salary.txt  Scripts
```

**Explanation:**

The `-A` option displays hidden files but excludes the special entries `.` and `..`.

**Difference:**

| Command | Hidden files | `.` and `..` |
|---|---|---|
| `ls` | No | No |
| `ls -a` | Yes | Yes |
| `ls -A` | Yes | No |

---

## Example 5: `ls -la` — Detailed Listing Including Hidden Files

**Command:**

```bash
ls -la
```

**Sample output:**

```text
drwxr-xr-x 6 geetha geetha 4096 Oct 28 10:00 .
drwxr-xr-x 3 geetha geetha 4096 Oct 28 09:55 ..
-rw-r--r-- 1 geetha geetha   24 Oct 28 10:00 .config
-rw-r--r-- 1 geetha geetha   23 Oct 28 10:00 .settings
```

The remaining files and directories will also be listed.

**Explanation:**

- `-l` — Long listing.
- `-a` — Include hidden files.

**Real-world use case:** Inspect application configuration files and permissions.

---

# 3. File Size Commands

## Example 6: `ls -lh` — Human-Readable File Sizes

First, create files of different sizes:

```bash
truncate -s 1K small.log
truncate -s 10K medium.log
truncate -s 2M large.log
```

The `truncate` command creates or resizes files to the specified logical size.

Execute:

```bash
ls -lh *.log
```

**Expected output:**

```text
-rw-r--r-- 1 geetha geetha 2.0M Oct 28 10:00 large.log
-rw-r--r-- 1 geetha geetha  10K Oct 28 10:00 medium.log
-rw-r--r-- 1 geetha geetha 1.0K Oct 28 10:00 small.log
```

**Explanation:**

- `-l` — Long listing.
- `-h` — Human-readable file sizes.

Instead of displaying `2097152` bytes, Linux displays `2.0M`.

**Real-world use case:** Identify large log files.

---

## Example 7: `ls -lS` — Sort Files by Size

**Command:**

```bash
ls -lSh *.log
```

**Expected order:**

```text
large.log
medium.log
small.log
```

**Explanation:**

- `-l` — Detailed listing.
- `-S` — Sort by size, largest first.
- `-h` — Human-readable sizes.

**Real-world use case:** Identify files consuming the most disk space.

---

## Example 8: `ls -lShr` — Smallest Files First

**Command:**

```bash
ls -lShr *.log
```

**Expected order:**

```text
small.log
medium.log
large.log
```

**Explanation:**

The `-r` option reverses the sorting order.

---

# 4. Date and Time Commands

## Example 9: `ls -lt` — Newest Files First

Create files with different modification times:

```bash
touch -t 202610250900 old.txt
touch -t 202610261000 middle.txt
touch -t 202610271100 latest.txt
```

Execute:

```bash
ls -lt old.txt middle.txt latest.txt
```

**Expected order:**

```text
latest.txt
middle.txt
old.txt
```

**Explanation:**

- `-l` — Long listing.
- `-t` — Sort by modification time.

The most recently modified file appears first.

**Real-world use case:** Find recently updated application logs.

---

## Example 10: `ls -ltr` — Oldest Files First

**Command:**

```bash
ls -ltr old.txt middle.txt latest.txt
```

**Expected order:**

```text
old.txt
middle.txt
latest.txt
```

**Explanation:**

- `-l` — Long listing.
- `-t` — Sort by modification time.
- `-r` — Reverse the order.

**Real-world use case:** Identify older files for archiving.

---

## Example 11: `ls --full-time` — Display Complete Timestamps

**Command:**

```bash
ls --full-time employee.txt
```

**Sample output:**

```text
-rw-r--r-- 1 geetha geetha 63 2026-10-28 10:00:00.000000000 +0530 employee.txt
```

**Explanation:**

Displays the complete modification timestamp, including seconds, fractional seconds, and timezone.

**Real-world use case:** Investigate precisely when a file was modified.

---

# 5. Directory Listing Commands

## Example 12: `ls -d` — Display Directory Name

**Command:**

```bash
ls -d Documents
```

**Output:**

```text
Documents
```

**Explanation:**

The `-d` option displays the directory entry itself rather than its contents.

Compare:

```bash
ls Documents
```

Output:

```text
project.txt
```

**Real-world use case:** Check a directory name without displaying its contents.

---

## Example 13: `ls -ld` — Display Directory Details

**Command:**

```bash
ls -ld Documents
```

**Sample output:**

```text
drwxr-xr-x 2 geetha geetha 4096 Oct 28 10:00 Documents
```

**Explanation:**

- `-l` — Long listing.
- `-d` — Display the directory itself.

**Real-world use case:** Check directory ownership and permissions.

---

## Example 14: `ls -d */` — Display Only Directories

**Command:**

```bash
ls -d */
```

**Expected output:**

```text
Backup/  Documents/  Reports/  Scripts/
```

**Explanation:**

- `-d` — Display directory entries.
- `*/` — Match directory names in the current directory.

**Real-world use case:** List project folders without displaying files.

---

## Example 15: `ls -R` — Recursive Listing

**Command:**

```bash
ls -R
```

**Sample output:**

```text
.:
Backup  Documents  Reports  Scripts  employee.txt

./Backup:

./Documents:
project.txt

./Reports:
february.txt  january.txt

./Scripts:
```

The actual output will include all other visible files created in the lab.

**Explanation:**

The uppercase `-R` option means **recursive**.

It displays files inside the current directory and its subdirectories.

**Real-world use case:** Examine a project's directory structure.

---

# 6. Sorting and Formatting Commands

## Example 16: `ls -r` — Reverse Alphabetical Order

**Command:**

```bash
ls -r
```

**Explanation:**

Reverses the default filename sorting order.

For example:

```text
Normal:
apple.txt
banana.txt
orange.txt

Reverse:
orange.txt
banana.txt
apple.txt
```

---

## Example 17: `ls -1` — One File Per Line

**Command:**

```bash
ls -1
```

**Sample output:**

```text
Backup
department.txt
Documents
employee.txt
Reports
salary.txt
Scripts
```

**Explanation:**

The option is the **number `1`**, not lowercase `l`.

It displays one entry per line.

---

## Example 18: `ls -F` — Identify File Types

**Command:**

```bash
ls -F
```

**Sample output:**

```text
Backup/
Documents/
Reports/
Scripts/
employee.txt
```

**Explanation:**

The `-F` option adds symbols to identify file types.

| Symbol | Meaning |
|---|---|
| `/` | Directory |
| `*` | Executable file |
| `@` | Symbolic link |
| `\|` | Named pipe |
| `=` | Socket |

---

## Example 19: `ls -i` — Display Inode Numbers

**Command:**

```bash
ls -i employee.txt
```

**Sample output:**

```text
123456 employee.txt
```

**Explanation:**

The `-i` option displays the inode number.

An inode is a filesystem data structure containing metadata such as permissions, ownership, timestamps, and references to file data.

**Real-world use case:** Investigate hard links and filesystem issues.

---

## Example 20: `ls -n` — Display Numeric User and Group IDs

**Command:**

```bash
ls -n employee.txt
```

**Sample output:**

```text
-rw-r--r-- 1 1000 1000 63 Oct 28 10:00 employee.txt
```

**Explanation:**

The `-n` option displays numeric user IDs and group IDs instead of names.

For example:

```text
geetha geetha
```

may appear as:

```text
1000 1000
```

**Real-world use case:** Troubleshoot ownership issues in Linux containers.

---

# 7. File Filtering Using `ls`

## Example 21: Display Only TXT Files

```bash
ls *.txt
```

**Explanation:**

The `*` wildcard matches zero or more characters.

`*.txt` matches filenames ending in `.txt`.

---

## Example 22: Display Only LOG Files

```bash
ls *.log
```

**Expected output:**

```text
large.log  medium.log  small.log
```

---

## Example 23: Display Files Starting with a Letter

```bash
ls e*
```

This displays entries beginning with `e`, such as:

```text
employee.txt
```

---

## Example 24: Display Files with a Specific Extension

```bash
ls -l *.log
```

Displays detailed information about matching `.log` files.

---

# 8. Summary of Important `ls` Commands

| Command | Meaning | Purpose |
|---|---|---|
| `ls` | List | Display files and directories |
| `ls -l` | Long | Detailed listing |
| `ls -a` | All | Include hidden files |
| `ls -A` | Almost all | Include hidden files except `.` and `..` |
| `ls -la` | Long + All | Detailed listing with hidden files |
| `ls -lh` | Long + Human-readable | Readable file sizes |
| `ls -lt` | Long + Time | Newest modified files first |
| `ls -ltr` | Long + Time + Reverse | Oldest modified files first |
| `ls -lS` | Long + Size | Largest files first |
| `ls -r` | Reverse | Reverse sorting order |
| `ls -R` | Recursive | List subdirectories recursively |
| `ls -d` | Directory | Display directory entry itself |
| `ls -ld` | Long + Directory | Display directory details |
| `ls -d */` | Directories only | List directories |
| `ls -F` | Classify | Add file type indicators |
| `ls -i` | Inode | Display inode numbers |
| `ls -n` | Numeric | Display numeric owner and group IDs |
| `ls -1` | One per line | Display one entry per line |
| `ls -X` | Extension | Sort by extension |
| `ls --full-time` | Full timestamp | Display complete modification time |
| `ls --color=auto` | Color | Color-code file types |

### Important Points for Trainees

**Linux commands are case-sensitive.**

For example:

```bash
ls -r
```

means reverse sorting, whereas:

```bash
ls -R
```

means recursive listing.

Similarly:

```bash
ls -s
```

displays allocated disk blocks, whereas:

```bash
ls -S
```

sorts by file size.

**Recommended commands for everyday Linux administration:** `ls -la`, `ls -lh`, `ls -ltr`, `ls -lSh`, `ls -ld`, and `ls -R`.

These commands are particularly useful when working with Linux servers, application deployments, log files, and middleware administration.