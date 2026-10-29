# Linux Practice Notes

> **Environment:** Ubuntu Linux terminal  
> **Practice topics:** Directories, files, redirection, text editing, deletion, copying, and CSV formatting.

## 1. Creating directories

Create the `Networkinglab` directory:

```bash
mkdir Networkinglab
```

`mkdir` creates a new directory in the current location.

Return to the home directory and create a `Day1` directory:

```bash
cd
mkdir Day1
```

`cd` without a path returns to the current user's home directory. `mkdir Day1` creates `Day1` in that location.

Create nested directories using a full home-relative path:

```bash
mkdir -p ~/Networkinglab/Day1
```

`-p` creates missing parent directories and allows an existing directory to remain. `~` represents the current user's home directory (for the recorded session, `/home/geetha`). The resulting path is `/home/geetha/Networkinglab/Day1`.

## 2. Creating and viewing a text file

Create an empty employee file:

```bash
touch employee.txt
```

`touch` creates an empty file if the file does not already exist.

Write one employee record:

```bash
echo "Employee: Ishwa Bharath" >employee.txt
```

`echo` displays the supplied text; `>` writes it into `employee.txt`, replacing any existing content.

View the contents:

```bash
cat employee.txt
```

**Recorded output:**

```text
Employee: Ishwa Bharath
```

### Overwrite and append

```bash
echo "Third employee :Dhanush " > employee.txt
echo "second Employee: Neha" >> employee.txt
```

`>` overwrites the existing file contents. `>>` adds the new text at the end of the file without removing earlier content.

### A command error from the session

```bash
rm employee.txt
"Employee: Ishwa Bharath" >employee.txt
```

**Recorded error:**

```text
Employee: Ishwa Bharath: command not found
```

`rm` removes the file. The quoted text alone is not a shell command; the shell tries to run it as one. Write text with `echo` instead:

```bash
echo "Employee: Ishwa Bharath" >employee.txt
cat employee.txt
echo "second Employee: Neha" >> employee.txt
echo "Third Employee: Jatin" >> employee.txt
cat employee.txt
```

**Recorded output:**

```text
Employee: Ishwa Bharath
second Employee: Neha
Third Employee: Jatin
```

## 3. Deleting lines from a file using `sed`

### How to delete second line in the employee.txt

```bash
sed -i '2d' employee.txt
```

`sed` means *Stream Editor*. `-i` edits the original file directly; `2` selects the second line; `d` deletes it; `employee.txt` is the file being modified.

### Few more sed commands

| Command | Description |
|---|---|
| `sed '2d' employee.txt` | Display content without the second line (original file unchanged) |
| `sed -i '2d' employee.txt` | Permanently delete the second line |
| `sed -i '1d' employee.txt` | Delete the first line |
| `sed -i '$d' employee.txt` | Delete the last line |
| `sed -i '2,3d' employee.txt` | Delete lines 2 through 3 |
| `sed -i '/Neha/d' employee.txt` | Delete all lines containing Neha |

After deleting the second line:

```bash
cat employee.txt
```

**Recorded output:**

```text
Employee: Ishwa Bharath
Third Employee: Jatin
```

### Display a changed view without editing the file

The session recreates the employee file and uses `sed` without `-i`:

```bash
rm employee.txt
ls
echo "Employee: Ishwa Bharath" >employee.txt
ls
cat employee.txt
echo "second Employee: Neha" >> employee.txt
echo "Third Employee: Jatin" >> employee.txt
cat employee.txt
sed '2d' employee.txt
```

**Output from `sed '2d' employee.txt`:**

```text
Employee: Ishwa Bharath
Third Employee: Jatin
```

The second line is omitted from the command's displayed output; the source file remains unchanged.

### Delete all lines matching a name

```bash
echo "fourth Employee: Ravi" >> employee.txt
echo "fifth Employee: RaviKumar" >> employee.txt
cat employee.txt
```

**Recorded output:**

```text
Employee: Ishwa Bharath
second Employee: Neha
Third Employee: Jatin
fourth Employee: Ravi
fifth Employee: RaviKumar
```

```bash
sed -i '/Ravi/d' employee.txt
cat employee.txt
```

**Recorded output:**

```text
Employee: Ishwa Bharath
second Employee: Neha
Third Employee: Jatin
```

The search pattern `/Ravi/` matches both `Ravi` and `RaviKumar`, so both lines are deleted.

## 4. Removing directories

### How to remove the Directory

**If Directory is Empty:**

```bash
rmdir data
```

`rmdir` removes an empty directory. When the directory is not empty, the recorded message is:

```text
rmdir: failed to remove 'data': Directory not empty
```

**If Directory is not Empty**

```bash
rm -r data
ls
```

`rm -r` removes the directory and all files or subdirectories inside it. `ls` lists the remaining items. Use recursive deletion carefully because files may not be recoverable.

## 5. Different way of creating Files

### Prepare the practice directory

```bash
mkdir -p FileCreationLab
cd FileCreationLab/
pwd
```

**Recorded output of `pwd`:**

```text
/home/geetha/Linuxtraining/FileCreationLab
```

`pwd` displays the current directory. These commands were run after entering `/home/geetha/Linuxtraining`.

### Create an empty file

```bash
touch employee.txt
ls
ls -l
```

**Recorded output:**

```text
employee.txt

total 0
-rw-r--r-- 1 geetha geetha 0 Oct 28 07:13 employee.txt
```

`ls` lists file names; `ls -l` displays details such as permissions, owner, size, and modification time. A size of `0` indicates an empty file.

### Create several files in one command

```bash
touch student.txt employe2.txt department.txt
ls
```

**Recorded output:**

```text
department.txt  employe2.txt  employee.txt  student.txt
```

A single `touch` command can name multiple files. The session then uses `ls -l` to inspect their details.

### Create a file using `echo`

```bash
echo "Trainer Name: Geetha" > trainer.txt
ls
```

**Recorded output of `ls`:**

```text
department.txt  employe2.txt  employee.txt  student.txt  trainer.txt
```

This writes text into `trainer.txt`, creating the file if necessary.

### Create a file using `printf`

```bash
printf "Employee:%s\n Salary:%d\n" "Geetha" 50000 >salary.txt
cat salary.txt
```

**Recorded output:**

```text
Employee:Geetha
 Salary:50000
```

`printf` formats text: `%s` inserts a string, `%d` inserts a number, and `\n` adds a line break.

### create a file with interactive mode:

```bash
cat > company.txt
```

Enter the file content:

```text
Company : Birlasoft
Location: Noida
Employee :2500
```

Finish input using **Ctrl+D**. Then display the file:

```bash
cat company.txt
```

**Recorded output:**

```text
Company : Birlasoft
Location: Noida
Employee :2500
```

## 6. Simple Copy command

The practice session edits an existing file and copies it:

```bash
cat employee.txt
vi employee.txt
cat employee.txt
```

**Recorded content after editing:**

```text
Employee file original
```

`vi employee.txt` opens the file in the Vi text editor. `cat` shows the file's contents after it has been edited.

```bash
cp employee.txt employee_backup.txt
ls
cat employee_backup.txt
```

**Recorded backup content:**

```text
Employee file original
```

`cp` makes a copy of the source file under the new file name. The original remains in place.

## 7. How to create Multiline Fine:

Create a CSV file using a here-document:

```bash
cat >employee.csv <<'EOF'
ID,Name,Department
101,Kamil,IT
102,Abinaya,HR
103,Tina,Transport
104,harish,Admin
EOF
```

`cat >employee.csv` writes into the destination file. `<<'EOF'` means all following lines become input until a line containing only `EOF` is entered.

```bash
cat employee.csv
```

**Recorded output:**

```text
ID,Name,Department
101,Kamil,IT
102,Abinaya,HR
103,Tina,Transport
104,harish,Admin
```

## 8. Displaying CSV data using `column`

Check where the command is installed:

```bash
command -v column
```

**Recorded output:**

```text
/usr/bin/column
```

Attempt to format CSV rows:

```bash
column -s, -t employee.csv
```

`-s,` uses comma as the field separator, and `-t` aligns fields into a table.

The session also checked the utility's package:

```bash
sudo apt install bsdextrautils
```

**Recorded package message:**

```text
bsdextrautils is already the newest version (2.39.3-9ubuntu6.6).
0 upgraded, 0 newly installed, 0 to remove and 48 not upgraded.
```

The next check showed the CSV file was empty:

```bash
cat employee.csv
ls -lh employee.csv
wc -c employee.csv
```

**Recorded output:**

```text
-rw-r--r-- 1 geetha geetha 0 Oct 28 07:26 employee.csv
0 employee.csv
```

`ls -lh` shows the file details and size; `wc -c` counts bytes. Here both confirm that `employee.csv` contains zero bytes. The session subsequently recreates the data file.

### Recreate the CSV data

```bash
cat > employee.csv <<'EOF'
EmployeeID,EmployeeName,Department,Salary
101,Ishwa Bharath,IT,45000
102,Neha,HR,50000
103,Jatin,Finance,55000
104,Geetha,Training,60000
105,Ravi,IT,65000
EOF
```

Verify the content:

```bash
cat employee.csv
```

**Recorded output:**

```text
EmployeeID,EmployeeName,Department,Salary
101,Ishwa Bharath,IT,45000
102,Neha,HR,50000
103,Jatin,Finance,55000
104,Geetha,Training,60000
105,Ravi,IT,65000
```

### Display the CSV as aligned columns

```bash
column -s, -t employee.csv
```

**Recorded output:**

```text
EmployeeID  EmployeeName   Department  Salary
101         Ishwa Bharath  IT          45000
102         Neha           HR          50000
103         Jatin          Finance     55000
104         Geetha         Training    60000
105         Ravi           IT          65000
```

### Display columns separated by ` | `

```bash
column -s, -t -o ' | ' employee.csv
```

**Recorded output:**

```text
EmployeeID | EmployeeName  | Department | Salary
101        | Ishwa Bharath | IT         | 45000
102        | Neha          | HR         | 50000
103        | Jatin         | Finance    | 55000
104        | Geetha        | Training   | 60000
105        | Ravi          | IT         | 65000
```

`-o ' | '` sets the text used to separate the formatted output columns. It changes display formatting without modifying the CSV file.
