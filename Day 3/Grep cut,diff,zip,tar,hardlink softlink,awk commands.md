Part 1 – Prepare the practical environment
Imagine you work for an IT company that maintains an employee management application. The company needs to archive reports, back up application files, manage configuration links, investigate logs, and monitor servers.
Step 1: Create the lab directory
Open Ubuntu and execute:
cd ~
mkdir -p linux-company-lab
cd linux-company-lab

mkdir -p reports config logs backups archive scripts


Explanation: mkdir -p creates the requested directories. It also creates missing parent directories when necessary.
Step 2: Create sample files
cat > reports/employees.csv <<'EOF'
ID,Name,Department,Salary
101,Geetha,Training,60000
102,Ravi,IT,65000
103,Neha,HR,50000
104,Arun,IT,58000
105,Priya,HR,52000
106,Karthik,IT,75000
EOF


cat > config/application.conf <<'EOF'
Application=EmployeePortal
Environment=Production
Database=EmployeeDB
Port=8080
Logging=Enabled
EOF


cat > logs/application.log <<'EOF'
INFO Application started
INFO Database connected
ERROR Database timeout
WARN High memory utilization
INFO Request processed
ERROR Login failed
EOF


cat > reports/monthly.txt <<'EOF'
October Payroll Report
Total Employees: 6
Report Status: Completed
EOF


Step 3: Verify the structure
ls -R


Expected directory structure:
linux-company-lab/

 archive/

 backups/

 config/

 application.conf

 logs/

 application.log

 reports/

 employees.csv

 monthly.txt

 scripts/



All commands in the initial exercises assume you are inside ~/linux-company-lab unless otherwise stated.
Part 2 – ZIP command
1. What is ZIP?
ZIP is an archive format that allows multiple files or directories to be packaged into one file, usually with compression.

For example, instead of sending an HR department ten individual files, you can create a single hr-reports.zip.
Multiple files packaged into a single ZIP archive.
Why companies use ZIP: Sharing project files, packaging reports, archiving documents, transferring files between Windows and Linux, and reducing storage or transfer size when data is compressible.
Step 1: Install the utilities
sudo apt update
sudo apt install zip unzip -y


Check:
zip -v
unzip -v


Step 2: ZIP a single file
zip backups/employee.zip reports/employees.csv


Expected output:
  adding: reports/employees.csv (deflated 8%)


The percentage is illustrative and depends on the data.

Verify:
ls -lh backups/


Explanation: zip creates the archive, backups/employee.zip is its destination, and reports/employees.csv is the source file. The original CSV remains unchanged.
Step 3: ZIP multiple files
zip backups/reports.zip reports/employees.csv reports/monthly.txt


Check the contents without extracting:
unzip -l backups/reports.zip


Expected archive entries:
reports/employees.csv
reports/monthly.txt


Step 4: ZIP an entire directory
zip -r backups/all-reports.zip reports/


-r means recursive. It includes files inside the directory and any nested subdirectories.
Step 5: Add another file to an existing ZIP
zip backups/all-reports.zip config/application.conf


ZIP updates the existing archive.
Step 6: Exclude a file while compressing
zip -r backups/company.zip reports/ config/ logs/ -x "*.log"


This includes the directories but excludes files ending with .log.

Useful when a deployment bundle should contain configuration and reports but no application logs.
Step 7: Compression levels
zip -0 backups/no-compression.zip reports/employees.csv
zip -9 backups/max-compression.zip reports/employees.csv


-0 stores the file without compression. -9 requests maximum ZIP compression, generally using more CPU time.
Step 8: Password-protected ZIP
zip -e backups/private.zip reports/employees.csv


You will be prompted for a password.

Important: Traditional ZIP encryption may be weak. For sensitive company data, use a modern encryption method and manage passwords separately.
ZIP command variants
Command	Purpose
zip result.zip file.txt	Archive one file
zip result.zip a.txt b.txt	Archive multiple files
zip -r result.zip folder/	Archive a complete directory
zip -9 result.zip file.txt	Maximum compression
zip -0 result.zip file.txt	No compression
zip -u result.zip file.txt	Update an existing archived entry if the source is newer
zip -d result.zip file.txt	Delete a named entry from an archive
unzip -l result.zip	List archive contents
unzip -t result.zip	Check archive integrity

Part 3 – UNZIP command
2. What is UNZIP?
unzip extracts files from ZIP archives.

Real-world situation: A development team delivers EmployeePortal.zip to a Linux administrator. The administrator extracts the package into a staging folder before deploying it.
Step 1: Extract an archive
unzip backups/all-reports.zip


This extracts the contents into the current directory and may prompt before overwriting existing files.
Step 2: Extract into a different folder
mkdir -p archive/extracted
unzip backups/all-reports.zip -d archive/extracted/


Expected result:
archive/
└── extracted/
    └── reports/
        ├── employees.csv
        └── monthly.txt


Step 3: View contents without extracting
unzip -l backups/all-reports.zip


Step 4: Extract a specific file
unzip backups/all-reports.zip reports/employees.csv -d archive/


Only the selected archive entry is extracted.
Step 5: Overwrite without asking
unzip -o backups/all-reports.zip -d archive/extracted/


-o overwrites existing destination files. Use it carefully.
Step 6: Do not overwrite existing files
unzip -n backups/all-reports.zip -d archive/extracted/


Step 7: Test archive integrity
unzip -t backups/all-reports.zip


Typical output:
No errors detected in compressed data of backups/all-reports.zip.


This checks ZIP integrity, not whether the files are trustworthy or safe.
Part 4 – TAR command
3. What is TAR?
TAR stands for Tape Archive. It combines files and directories into one archive. By itself, TAR does not necessarily compress data.

TAR is widely used for Linux backups, software distribution, server migration and log retention.
Difference between TAR and ZIP
Feature	ZIP	TAR
Primary function	Archiving and often compression	Archiving
Typical extension	.zip	.tar
Compression	Usually built into the archive	Can be added using gzip, bzip2 or xz
Linux metadata	Limited/format-dependent	Well suited to preserving Unix metadata
Common uses	Sharing files	Linux backups and deployment packages

A .tar.gz is a TAR archive compressed using gzip.
Step 1: Create a TAR archive
tar -cvf backups/reports.tar reports/


Arguments:
Option	Meaning
c	Create archive
v	Verbose: show processed files
f	Archive filename follows
x	Extract
t	List contents
z	Use gzip
j	Use bzip2
J	Use xz

Expected output:
reports/
reports/employees.csv
reports/monthly.txt


Step 2: List TAR contents
tar -tvf backups/reports.tar


This shows names and metadata without extracting files.
Step 3: Extract TAR files
mkdir -p archive/tar-extracted
tar -xvf backups/reports.tar -C archive/tar-extracted/


-C specifies where the extracted files should go.
Step 4: Create a compressed TAR.GZ
tar -czvf backups/reports.tar.gz reports/


z enables gzip compression.
Step 5: Extract a TAR.GZ
mkdir -p archive/gzip-extracted
tar -xzvf backups/reports.tar.gz -C archive/gzip-extracted/


Step 6: TAR.BZ2 and TAR.XZ
tar -cjvf backups/reports.tar.bz2 reports/
tar -cJvf backups/reports.tar.xz reports/


Extract:
tar -xjvf backups/reports.tar.bz2 -C archive/
tar -xJvf backups/reports.tar.xz -C archive/


Compression efficiency and speed vary with the content and algorithm.
Step 7: Add another file to an uncompressed TAR
tar -rvf backups/reports.tar config/application.conf


-r appends to a normal TAR archive. It cannot be used to append directly to a compressed .tar.gz archive.
Step 8: View or extract only one file
tar -tf backups/reports.tar
tar -xvf backups/reports.tar reports/employees.csv -C archive/


Industry example – Archive application logs
tar -czvf backups/application-logs.tar.gz logs/
tar -tzvf backups/application-logs.tar.gz


This is a common first step before transferring old log files into backup storage. The archive still requires off-server storage, verification, and retention controls to qualify as a reliable backup.
Part 5 – Hard Links
4. What is a hard link?
A hard link is another filename pointing to the same underlying file data, identified by an inode on a filesystem.

Real-world use case: An administrator needs the same large data file to be accessible under two filenames on the same filesystem without creating a second full copy.
Step 1: Create an original file
echo "Production configuration" > config/original.txt


Step 2: Create the hard link
ln config/original.txt config/hardlink.txt


Step 3: Check inode numbers
ls -li config/original.txt config/hardlink.txt


Example output:
123456 -rw-r--r-- 2 user user 25 ... config/hardlink.txt
123456 -rw-r--r-- 2 user user 25 ... config/original.txt


The inode number 123456 is illustrative. Both entries will share an inode, and the link count is 2.
Step 4: Modify the linked data
echo "Updated configuration" >> config/hardlink.txt
cat config/original.txt


Expected output:
Production configuration
Updated configuration


Both names access the same data.
Step 5: Delete one filename
rm config/original.txt
cat config/hardlink.txt


Expected output:
Production configuration
Updated configuration


The data is still accessible because another hard link exists.

Important limitations: Hard links ordinarily cannot span separate filesystems and cannot normally be created for directories. Hard links are also not independent backups: changing the file through either name changes the same data.
Part 6 – Soft Links (Symbolic Links)
5. What is a soft link?
A symbolic link stores a path pointing to another file or directory. It works similarly to a shortcut, but Linux programs commonly follow the link as part of normal filesystem access.

Step 1: Create a symbolic link
ln -s application.conf config/current.conf


Here the link and its target are in the config directory. The relative target path is resolved relative to the link's location.
Step 2: Verify the link
ls -l config/current.conf


Expected output:
current.conf -> application.conf


Step 3: Read through the link
cat config/current.conf


Expected output:
Application=EmployeePortal
Environment=Production
Database=EmployeeDB
Port=8080
Logging=Enabled


Step 4: Check the target
readlink config/current.conf
readlink -f config/current.conf


readlink displays the stored target. readlink -f resolves it to a canonical absolute path if it can resolve the path.
Step 5: Demonstrate a broken symbolic link
mv config/application.conf config/application.old
cat config/current.conf


Expected result:
cat: config/current.conf: No such file or directory


The symbolic link still exists, but its target no longer exists under the expected filename.

Restore the target:
mv config/application.old config/application.conf
cat config/current.conf


The link works again.
Step 6: Symbolic link to a directory
ln -s reports report-shortcut
ls report-shortcut/


This provides another path to the reports directory.
Industry example – Switch deployment versions
A company deploys release 1 and release 2 and wants the application to use the current directory.
mkdir -p releases/v1 releases/v2

echo "Version 1" > releases/v1/version.txt
echo "Version 2" > releases/v2/version.txt

ln -s v1 releases/current

cat releases/current/version.txt


Output:
Version 1


Switch the link:
ln -sfn v2 releases/current
cat releases/current/version.txt


Output:
Version 2


For production deployment, use an atomic link replacement strategy rather than relying on ln -sfn to make a fully atomic switch.
Hard Link vs Soft Link
Property	Hard Link	Soft Link
Command	ln source link	ln -s target link
Inode	Shares file inode	Has its own inode
Target deleted	Remaining hard link works	Usually becomes broken
Directory linking	Normally prohibited	Supported
Across filesystems	No	Yes
Typical usage	Multiple names for the same file	Deployment pointers and shared configuration


Part 7 – Text Processing Commands (19–23)
These commands are useful when a company receives employee files, application logs, server reports or CSV exports.
19. awk – Process structured text
What is it? awk is a text-processing language for extracting, filtering, calculating and formatting data.

Why use it? Suppose HR exports a CSV file containing thousands of employee records. You want to print only employee names, identify employees in IT, or calculate total salaries.
Demo 1 – Display employee names
awk -F, 'NR>1 {print $2}' reports/employees.csv


Expected output:
Geetha
Ravi
Neha
Arun
Priya
Karthik


Explanation:
- -F, specifies comma as the separator.
- NR>1 skips the header.
- $2 selects the second field.
Demo 2 – Display only IT employees
awk -F, '$3=="IT" {print $2, $4}' reports/employees.csv


Output:
Ravi 65000
Arun 58000
Karthik 75000


Demo 3 – Display salaries greater than ₹60,000
awk -F, 'NR>1 && $4>60000 {print $2, $4}' reports/employees.csv


Demo 4 – Calculate total salary
awk -F, 'NR>1 {sum+=$4} END {print "Total Salary:", sum}' reports/employees.csv


Output:
Total Salary: 360000


Demo 5 – Count employees by department
awk -F, 'NR>1 {count[$3]++} END {for(d in count) print d,count[d]}' reports/employees.csv


Expected results (order may vary):
Training 1
IT 3
HR 2


Other useful forms: awk '{print $1}' file, awk 'NR==3' file, awk '/ERROR/' file, and awk '{print NR,$0}' file.

Note: These examples use simple CSV data without quoted commas. Complex CSV requires a CSV-aware parser.
20. sort – Sort records
What is it? sort arranges lines alphabetically, numerically, or by selected columns.

Real-world use: Sorting salary reports, server names, IP addresses or error summaries.
Demo 1 – Sort names alphabetically
printf 'Ravi\nGeetha\nNeha\nArun\n' > reports/names.txt
sort reports/names.txt


Output:
Arun
Geetha
Neha
Ravi


Demo 2 – Reverse sorting
sort -r reports/names.txt


Demo 3 – Sort employees by salary
(head -n 1 reports/employees.csv; tail -n +2 reports/employees.csv | sort -t, -k4,4n)


-t, selects comma as the separator, -k4,4 selects salary, and n requests numeric sorting. The header remains first.
Demo 4 – Highest salary first
tail -n +2 reports/employees.csv | sort -t, -k4,4nr


Demo 5 – Unique values
printf 'IT\nHR\nIT\nTraining\nHR\n' | sort -u


Output:
HR
IT
Training


Additional options: sort -n (numeric), sort -r (reverse), sort -u (unique), sort -k2 (sort by second field), sort -o output.txt input.txt (save output).
21. cut – Extract selected columns
What is it? cut extracts fields or character ranges from text.

Why use it? You only need names and departments from a large employee export.
Demo 1 – Extract names
cut -d, -f2 reports/employees.csv


Output begins:
Name
Geetha
Ravi
Neha
Arun
Priya
Karthik


Demo 2 – Extract names and departments
cut -d, -f2,3 reports/employees.csv


Demo 3 – Extract first ten characters
cut -c1-10 reports/monthly.txt


Demo 4 – Extract user names from Linux account records
cut -d: -f1 /etc/passwd


Other forms: cut -f1, cut -c1-5, cut -d: -f1,3, cut -d, -f2-4.
22. diff – Compare files
What is it? diff identifies differences between two text files.

Real-world situation: A developer modifies application configuration, and the production support engineer wants to know what changed before deployment.
Step 1: Create two versions
cp config/application.conf config/application-v2.conf

sed -i 's/Port=8080/Port=9090/' config/application-v2.conf


Step 2: Compare
diff config/application.conf config/application-v2.conf


Expected output:
4c4
< Port=8080
---
> Port=9090


This means line 4 changed from Port=8080 to Port=9090.
Step 3: Unified format
diff -u config/application.conf config/application-v2.conf


This shows context with - for removed lines and + for added lines.
Step 4: Side-by-side comparison
diff -y config/application.conf config/application-v2.conf


Other forms: diff -q file1 file2 reports only whether files differ; diff -r dir1 dir2 compares directories recursively.
23. tee – Display and save output simultaneously
What is it? tee copies standard input to the terminal and to one or more files.

Why use it? An administrator needs to view a command's output while recording it for an audit.
Demo 1 – Capture command output
ls -lh reports/ | tee logs/report-list.txt


The listing appears on-screen and is saved to logs/report-list.txt.
Demo 2 – Append without replacing old content
date | tee -a logs/report-list.txt


-a means append.