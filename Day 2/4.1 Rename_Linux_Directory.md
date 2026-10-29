# 🐧 Linux Tutorial: Rename a Directory from `Course` to `Source`

> **Environment:** Ubuntu Linux / Windows Subsystem for Linux (WSL)  
> **Topic:** Renaming directories using `mv`  
> **Level:** Beginner

## Step 1: Check the existing directory

Run the following command in the directory containing `Course`:

```bash
ls
```

**Example output:**

```text
Course  Backup  Archive  Reports
```

## Step 2: Rename the directory

```bash
mv Course Source
```

**Explanation:**

| Part | Meaning |
|---|---|
| `mv` | Moves or renames a file or directory |
| `Course` | Existing directory name |
| `Source` | New directory name |

The command renames `Course` to `Source` **when `Source` does not already exist**.

## Step 3: Verify the new directory name

```bash
ls
```

**Expected output:**

```text
Source  Backup  Archive  Reports
```

## Step 4: Confirm the directory contents are preserved

```bash
ls Source
```

Files and subdirectories previously inside `Course` remain inside `Source`.

---

## Additional examples

### 1. Rename using absolute paths

```bash
mv ~/Linuxtraining/Course ~/Linuxtraining/Source
```

This works from another working directory, provided `Course` exists at the specified location.

### 2. Rename with interactive confirmation

```bash
mv -i Course Source
```

The `-i` option asks for confirmation before overwriting a conflicting destination. **It does not prevent moving `Course` inside an existing `Source` directory.**

### 3. Display the operation using verbose mode

```bash
mv -v Course Source
```

**Sample output:**

```text
renamed 'Course' -> 'Source'
```

---

## ⚠️ Important: What if `Source` already exists?

If `Source` is already a directory, running:

```bash
mv Course Source
```

will usually move `Course` **inside** `Source`, producing `Source/Course`, instead of renaming it.

To ensure that the destination is not treated as a directory, GNU `mv` supports:

```bash
mv -T Course Source
```

Use this only after checking the destination. If `Source` already exists as a nonempty directory, this command will fail rather than merge its contents.

**Preview both paths before proceeding:**

```bash
ls -ld Course Source
```

An error for `Source` saying it does not exist is expected if you have not created it yet.

## ✅ Quick reference

| Command | Purpose |
|---|---|
| `ls` | List directory entries |
| `mv Course Source` | Rename `Course` to `Source` if `Source` does not exist |
| `ls Source` | Check contents after renaming |
| `mv -i Course Source` | Prompt on overwrite conflicts |
| `mv -v Course Source` | Show what was moved or renamed |
| `mv -T Course Source` | Treat `Source` as the destination itself, not as a containing directory |

**Recommended command:**

```bash
mv Course Source
```

Run it from the directory containing `Course`, after confirming `Source` does not already exist.
