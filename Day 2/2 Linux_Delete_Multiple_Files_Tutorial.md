<div align="center">

# 🟥🟧🟨 Linux Tutorial: Delete Multiple Files at Once

### 🟦 Ubuntu / WSL • Beginner Hands-on Lab

</div>

> **⚠️ Safety:** `rm` permanently removes files rather than sending them to the Windows Recycle Bin. Preview filenames and prefer `rm -i` while learning.

## 🟢 1. Create Sample Files

```bash
mkdir -p ~/Linuxtraining/DeleteLab
cd ~/Linuxtraining/DeleteLab
touch file1.txt file2.txt file3.txt file4.txt file5.txt
ls
```

**Expected output:**

```text
file1.txt  file2.txt  file3.txt  file4.txt  file5.txt
```

## 🔵 2. Delete Multiple Files by Name

```bash
rm file1.txt file2.txt file3.txt
ls
```

**Expected output:**

```text
file4.txt  file5.txt
```

**Explanation:** `rm` removes each named file. Here, three files are removed in one command.

## 🟣 3. Delete All `.txt` Files

```bash
rm -i -- *.txt
```

**Explanation:** `*` matches zero or more characters; `*.txt` selects all visible filenames ending in `.txt` in the current directory. `-i` asks for confirmation for each file. To delete without prompts, use `rm -- *.txt` after checking the matches.

## 🟠 4. Delete Files Matching a Prefix

```bash
touch log1.txt log2.txt log3.txt employee.txt salary.txt
ls -d -- log*.txt   # Preview
rm -i -- log*.txt   # Delete with confirmation
ls
```

**Expected remaining files:**

```text
employee.txt  salary.txt
```

**Explanation:** `log*.txt` matches files starting with `log` and ending with `.txt`.

## 🟡 5. Delete Selected Files Using Brace Expansion

```bash
touch file1.txt file2.txt file3.txt file4.txt file5.txt
rm -i -- file{1,3,5}.txt
ls
```

**Expected remaining numbered files:**

```text
file2.txt  file4.txt
```

**Explanation:** Bash expands `file{1,3,5}.txt` into `file1.txt file3.txt file5.txt` before running `rm`.

## 🔵 6. Delete with Confirmation

```bash
rm -i -- file2.txt file4.txt
```

**Sample prompts:**

```text
rm: remove regular empty file 'file2.txt'? y
rm: remove regular empty file 'file4.txt'? y
```

**Explanation:** `-i` enables interactive confirmation for each file.

## 🔴 7. Delete All Visible Regular Files in the Current Directory

```bash
ls -la                    # Inspect first
rm -i -- *                # Ask before removing matching files
```

**Explanation:** `*` expands to non-hidden entries. Plain `rm` removes regular files but will not remove directories without `-r`. Avoid running these commands from your home directory unless you have checked the contents carefully.

## 🟢 8. Delete Multiple File Extensions

```bash
touch employee.txt salary.csv application.log notes.md
ls -d -- *.txt *.log       # Preview
rm -i -- *.txt *.log       # Confirm each deletion
ls
```

**Expected remaining files** (assuming no other files were present):

```text
notes.md  salary.csv
```

**Explanation:** The two wildcard patterns select `.txt` and `.log` files in one command.

## 📘 Summary

| Command | Purpose |
|---|---|
| `rm file1.txt file2.txt` | Delete named files |
| `rm *.txt` | Delete matching `.txt` files |
| `rm log*` | Delete entries starting with `log` (files only without `-r`) |
| `rm file{1,3,5}.txt` | Delete selected numbered files |
| `rm file{1..5}.txt` | Delete numbered files 1 through 5 |
| `rm -i *.txt` | Ask before deleting each matching file |
| `rm *.txt *.csv` | Delete multiple file types |
| `rm *` | Delete matching non-hidden files, not directories |

## ⭐ Best Practice: Preview Before Deleting

```bash
ls -d -- *.txt
rm -i -- *.txt
```

> **Note:** If no files match a wildcard, Bash may pass the literal pattern to the command and `rm` can report `No such file or directory`. This is normal.
