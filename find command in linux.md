# `find` Command in Linux


## 1. What is the `find` command?

The Linux `find` command is used to **search for files and directories** based on conditions such as:

* Name
* Type
* Size
* Location
* Permission
* Owner
* Modified time


> **`find` is used to search for files and directories in Linux.**

---

# 2. Why do we need `find`?

Imagine a server has thousands of files:

```text
/home/student/
       ├── notes.txt
       ├── test.txt
       ├── report.pdf
       ├── project/
       │    ├── app.txt
       │    └── data.txt
       └── backup/
            └── report.txt
```

You want to find:

```text
Where is report.txt?
```

Instead of manually checking every directory, use:

```bash
find /home/student -name "report.txt"
```

Linux searches and gives you the location.

---

# 3. Basic Syntax

```bash
find [location] [condition]
```

For example:

```bash
find /home -name "test.txt"
```

Meaning:

```text
find
 ↓
Search
 ↓
/home
 ↓
File name = test.txt
```

---

# 4. First Hands-on

Let's create some files.

```bash
mkdir linux-demo
cd linux-demo
```

Create files:

```bash
touch file1.txt
touch file2.txt
touch report.txt
touch notes.txt
```

Check:

```bash
ls
```

You should see:

```text
file1.txt
file2.txt
report.txt
notes.txt
```

---

# 5. Find a File by Name

Run:

```bash
find . -name "report.txt"
```

Output:

```text
./report.txt
```

Explain:

### `.` means current directory

So:

```bash
find . -name "report.txt"
```

means:

> Search for `report.txt` starting from the current directory.

---

# 6. Find All `.txt` Files

```bash
find . -name "*.txt"
```

Output:

```text
./file1.txt
./file2.txt
./report.txt
./notes.txt
```

Here:

```text
*.txt
```

means:

> Any filename ending with `.txt`

For example:

```text
file1.txt
report.txt
notes.txt
```

---

# 7. Find Directories

Create some directories:

```bash
mkdir dev
mkdir test
mkdir backup
```

Now:

```bash
find . -type d
```

Output:

```text
.
./dev
./test
./backup
```

Explain:

```text
-type d
```

means:

> Search only for directories.

---

# 8. Find Files Only

```bash
find . -type f
```

Output:

```text
./file1.txt
./file2.txt
./report.txt
./notes.txt
```

Explain:

```text
-type f
```

means:

> Search only for regular files.

So students should remember:

```text
-type f → file
-type d → directory
```

---

# 9. Search in a Specific Directory

Suppose you have:

```text
/home/student/projects
```

You can search:

```bash
find /home/student/projects -name "*.txt"
```

The important thing is:

```text
Where should I search?
        +
What am I looking for?
```

---

# 10. Case-Insensitive Search

Suppose the file is:

```text
Report.txt
```

But you search:

```bash
find . -name "report.txt"
```

It may not find it because Linux filenames are case-sensitive.

Use:

```bash
find . -iname "report.txt"
```

`-iname` ignores uppercase/lowercase differences.

---

# 11. Find Files by Size

Suppose you want to find files larger than 10 MB:

```bash
find . -type f -size +10M
```

Meaning:

```text
-type f
   ↓
Only files

-size +10M
   ↓
Larger than 10 MB
```

You can also find files smaller than 10 MB:

```bash
find . -type f -size -10M
```

---

# 12. Find Recently Modified Files

This is useful for Linux administration.

Find files modified within the last 1 day:

```bash
find . -type f -mtime -1
```

Explain:

```text
-mtime -1
    ↓
Modified within the last 1 day
```

This can be useful when troubleshooting:

> "Which files were recently changed?"

---

# 13. Real-World Problem Statement

This is a good beginner assignment.

### Problem

> A Linux server contains many files. The administrator needs to find all `.log` files under `/var/log`.

Command:

```bash
find /var/log -type f -name "*.log"
```

The flow is:

```text
Linux Server
     ↓
/var/log
     ↓
Search
     ↓
Only files
     ↓
Filename ending with .log
     ↓
Display results
```

---


> **`find` finds the files → `-exec` performs an action on those files.**

This is very useful for Linux administration.

# 1. First understand the problem

Suppose you have 100 `.txt` files:

```text
project/
├── file1.txt
├── file2.txt
├── file3.txt
├── file4.txt
└── ...
```

You want to change permissions of **all `.txt` files**.

Without `find`:

```bash
chmod 644 file1.txt
chmod 644 file2.txt
chmod 644 file3.txt
...
```

This is difficult when there are hundreds of files.

Instead:

```bash
find . -type f -name "*.txt" -exec chmod 644 {} \;
```

This means:

```text
find files
   ↓
.txt files
   ↓
For each file
   ↓
run chmod 644
```

---

# 2. Understand `-exec`

The basic syntax is:

```bash
find <location> <condition> -exec <command> {} \;
```

For example:

```bash
find . -type f -name "*.txt" -exec chmod 644 {} \;
```

Break it down:

| Part            | Meaning                      |
| --------------- | ---------------------------- |
| `find`          | Search                       |
| `.`             | Current directory            |
| `-type f`       | Find files only              |
| `-name "*.txt"` | Find `.txt` files            |
| `-exec`         | Execute a command            |
| `chmod 644`     | Command to execute           |
| `{}`            | Current file found by `find` |
| `\;`            | End of `-exec` command       |

The most important part for students:

```text
{} = the file found by find
```

---

# 3.  example

Create a practice directory:

```bash
mkdir find-demo
cd find-demo
```

Create files:

```bash
touch file1.txt
touch file2.txt
touch file3.txt
touch report.txt
touch data.csv
```

Check:

```bash
ls -l
```

---

# 4. First use `find` WITHOUT `-exec`

Let's find `.txt` files:

```bash
find . -type f -name "*.txt"
```

Output:

```text
./file1.txt
./file2.txt
./file3.txt
./report.txt
```

Now we know which files we want to modify.

---

# 5. Change permissions using `-exec`

Suppose we want all `.txt` files to have:

```text
644
```

Run:

```bash
find . -type f -name "*.txt" -exec chmod 644 {} \;
```

Now verify:

```bash
ls -l
```

You should see something like:

```text
-rw-r--r-- file1.txt
-rw-r--r-- file2.txt
-rw-r--r-- file3.txt
-rw-r--r-- report.txt
```

### What happened?

For every file found, Linux effectively executed:

```bash
chmod 644 ./file1.txt
chmod 644 ./file2.txt
chmod 644 ./file3.txt
chmod 644 ./report.txt
```

You wrote only one command.

---

# 6. Why `{}` is important

Consider:

```bash
find . -type f -name "*.txt" -exec chmod 644 {} \;
```

When `find` finds:

```text
./file1.txt
```

`{}` becomes:

```text
./file1.txt
```

So Linux executes:

```bash
chmod 644 ./file1.txt
```

Then:

```text
{} → ./file2.txt
```

becomes:

```bash
chmod 644 ./file2.txt
```

So:

```text
{} = each matching file
```

---

# 7. Now deleting files

Suppose you want to delete all `.tmp` files.

First create some:

```bash
touch file1.tmp
touch file2.tmp
touch file3.tmp
```

Check:

```bash
find . -type f -name "*.tmp"
```

**Always check what will be affected before deleting.**

Then:

```bash
find . -type f -name "*.tmp" -exec rm {} \;
```

This means:

```text
Find .tmp files
      ↓
For each file
      ↓
Execute rm
      ↓
Delete the file
```

Verify:

```bash
find . -type f -name "*.tmp"
```

No output means the matching files are gone.

---

# 8. Very important safety lesson

Don't immediately teach students:

```bash
find / -type f -exec rm {} \;
```

This can attempt to delete **huge portions of the system**.

Instead, always start with a controlled directory:

```bash
find ./find-demo -type f -name "*.tmp"
```

First **find and verify**.

Then delete:

```bash
find ./find-demo -type f -name "*.tmp" -exec rm {} \;
```

### Golden rule

> **Before using `find -exec rm`, run the `find` command alone first and verify the files.**

---

# 9. Safer deletion with confirmation

You can ask for confirmation before deleting each file:

```bash
find . -type f -name "*.tmp" -exec rm -i {} \;
```

You may get:

```text
remove './file1.tmp'? 
```

Enter:

```text
y
```

to delete it.

Or:

```text
n
```

to keep it.

This is excellent for beginners because they can see exactly what is being deleted.

---

# 10. `-exec` can run many commands

It isn't limited to `chmod` and `rm`.

For example:

### Change ownership

```bash
find . -type f -name "*.txt" -exec chown student {} \;
```

### Change permissions

```bash
find . -type f -name "*.sh" -exec chmod 755 {} \;
```

### Display details

```bash
find . -type f -name "*.log" -exec ls -l {} \;
```

So the concept is:

```text
find
 ↓
Find matching files
 ↓
-exec
 ↓
Run a command on each matching file
```

---

# 11. A very useful real-world example

### Problem statement

> A Linux administrator finds that several `.sh` shell scripts in a deployment directory are not executable. The administrator needs to give execute permission to all `.sh` files.

First identify:

```bash
find /opt/scripts -type f -name "*.sh"
```

Then change permissions:

```bash
find /opt/scripts -type f -name "*.sh" -exec chmod 755 {} \;
```

Verify:

```bash
find /opt/scripts -type f -name "*.sh" -exec ls -l {} \;
```

This is a much better example for DevOps/Linux students than starting with Java applications.

---

# 12. One more useful example: delete old log files

Suppose you have temporary log files under:

```text
/var/tmp/myapp/
```

You want to find `.log` files older than 7 days.

First:

```bash
find /var/tmp/myapp -type f -name "*.log" -mtime +7
```

**Review the output.**

If the files are definitely safe to remove:

```bash
find /var/tmp/myapp -type f -name "*.log" -mtime +7 -exec rm {} \;
```

This combines:

```text
Location
   ↓
File type
   ↓
Filename
   ↓
Age
   ↓
Delete
```

---

# 13. diagram

```text
                 find
                  ↓
          Search for files
                  ↓
       ┌────────────────────┐
       │ Matching files     │
       │ file1.txt          │
       │ file2.txt          │
       │ file3.txt          │
       └────────────────────┘
                  ↓
               -exec
                  ↓
          Run command on
          each matching file
                  ↓
        ┌─────────┴─────────┐
        ↓                   ↓
     chmod                  rm
   change permission       delete
```

## The 3 commands students should remember

### Change permissions

```bash
find . -type f -name "*.txt" -exec chmod 644 {} \;
```

### Delete files

```bash
find . -type f -name "*.tmp" -exec rm {} \;
```

### Delete with confirmation

```bash
find . -type f -name "*.tmp" -exec rm -i {} \;
```

### One-line definition

> **`find -exec` allows us to search for files matching specific conditions and execute a Linux command on each matching file.**



