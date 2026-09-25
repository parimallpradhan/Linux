# `grep` Command in Linux


> **`grep` is used to search for specific text inside files or command output.**



```text
Large amount of text
        ↓
       grep
        ↓
Show only the lines I am interested in
```

---

## 1. Example

Create a file:

```bash
cat > students.txt
```

Enter:

```text
Rahul
Amit
Priya
Neha
Rahul
```

Press **Ctrl + D**.

Now search for `Rahul`:

```bash
grep "Rahul" students.txt
```

Output:

```text
Rahul
Rahul
```

So `grep` searched inside `students.txt` and displayed only the lines containing `Rahul`.

---

# 2. Why Do We Need `grep`?

Imagine a file contains 1,000 lines.

You want to find:

```text
ERROR
```

Instead of opening the file and manually searching, use:

```bash
grep "ERROR" application.log
```

Linux will show only the lines containing `ERROR`.

This is extremely common in Linux administration and DevOps.

---

# 3. Basic Syntax

```bash
grep "text" filename
```

Example:

```bash
grep "Linux" notes.txt
```

Meaning:

```text
grep
 ↓
Search for
 ↓
"Linux"
 ↓
inside notes.txt
```

---

# 4. Beginner Hands-on

Create a file:

```bash
cat > server.log
```

Enter:

```text
Server started
User login successful
Database connection successful
ERROR: Database connection failed
User login successful
ERROR: Disk space low
Server stopped
```

Press:

```text
Ctrl + D
```

Now:

```bash
cat server.log
```

This displays the entire file.

Instead, search only for errors:

```bash
grep "ERROR" server.log
```

Output:

```text
ERROR: Database connection failed
ERROR: Disk space low
```

### Explain to students:

Without `grep`:

```text
cat server.log
 ↓
Lots of lines
```

With `grep`:

```text
server.log
 ↓
grep "ERROR"
 ↓
Only ERROR lines
```

---

# 5. Search Without Case Sensitivity

Suppose the file contains:

```text
ERROR
Error
error
```

If you run:

```bash
grep "error" server.log
```

it normally searches case-sensitively.

Use:

```bash
grep -i "error" server.log
```

`-i` means:

> Ignore uppercase/lowercase differences.

---

# 6. Show Line Numbers

Use:

```bash
grep -n "ERROR" server.log
```

Output might be:

```text
4:ERROR: Database connection failed
6:ERROR: Disk space low
```

Here:

```text
4:
```

means the matching text is on **line 4**.

So:

```text
-n → show line number
```

---

# 7. Search Multiple Words

Suppose you want to search for `ERROR` in several log files:

```bash
grep "ERROR" app.log database.log server.log
```

It searches all three files.

---

# 8. Search All Files in a Directory

Suppose you have:

```text
logs/
├── app.log
├── server.log
└── database.log
```

You can search all files:

```bash
grep -r "ERROR" logs/
```

Here:

```text
-r
 ↓
recursive
 ↓
Search inside files under directories
```

This is very useful.

---

# 9. `grep` with `cat`

You may see beginners doing:

```bash
cat server.log | grep "ERROR"
```

This works.

But for a simple file search, this is cleaner:

```bash
grep "ERROR" server.log
```

Teach students the direct form first.

---

# 10. `grep` with Command Output

This is where `grep` becomes very useful.

Suppose:

```bash
ps aux
```

shows hundreds of processes.

You only want to find processes containing `ssh`.

```bash
ps aux | grep ssh
```

Flow:

```text
ps aux
   ↓
Lots of process information
   ↓
grep ssh
   ↓
Only lines containing ssh
```

This is a common Linux troubleshooting technique.

---

# 11. Another Example: Find a User

Suppose:

```bash
cat /etc/passwd
```

contains many users.

Instead of reading everything:

```bash
grep "student" /etc/passwd
```

It shows the matching user entry.

---

# 12. Search for Lines NOT Matching

Use:

```bash
grep -v "ERROR" server.log
```

`-v` means:

> Show lines that **do not** contain the search text.

For example:

```text
grep -v "ERROR" server.log
```

will show everything except lines containing `ERROR`.

---

# 13. Count Matches

Use:

```bash
grep -c "ERROR" server.log
```

Output:

```text
2
```

Meaning:

> There are 2 lines containing `ERROR`.

Important:

```text
-c → count matching lines
```

---

# 14. Search for a Whole Word

Suppose your file contains:

```text
user
username
users
```

If you run:

```bash
grep "user" file.txt
```

it can match all three because `user` appears inside them.

To search for the complete word:

```bash
grep -w "user" file.txt
```

`-w` means:

> Match the whole word.

---

# 15. Real-World Problem Statement

This is a very good example for your fresher class.

### Problem

> A Linux server has an application log file containing thousands of lines. The administrator wants to find all error messages.

File:

```text
application.log
```

Command:

```bash
grep "ERROR" application.log
```

If they also want line numbers:

```bash
grep -n "ERROR" application.log
```

If they want `ERROR`, `Error`, and `error`:

```bash
grep -in "error" application.log
```

---

# 16. Useful `grep` Options

For beginners, teach these first:

| Command            | Purpose                 |
| ------------------ | ----------------------- |
| `grep "text" file` | Search text             |
| `grep -i`          | Ignore case             |
| `grep -n`          | Show line number        |
| `grep -v`          | Show non-matching lines |
| `grep -c`          | Count matching lines    |
| `grep -r`          | Search recursively      |
| `grep -w`          | Match whole word        |

---

# 17. One Complete Hands-on

Give students this exercise.

### Create file

```bash
cat > application.log
```

Enter:

```text
INFO: Server started
INFO: User logged in
ERROR: Database connection failed
INFO: Request received
WARNING: Disk space is low
ERROR: Connection timeout
INFO: Server running
```

Press **Ctrl + D**.

### Task 1 — Find errors

```bash
grep "ERROR" application.log
```

### Task 2 — Show line numbers

```bash
grep -n "ERROR" application.log
```

### Task 3 — Count errors

```bash
grep -c "ERROR" application.log
```

### Task 4 — Find warnings

```bash
grep "WARNING" application.log
```

### Task 5 — Show everything except errors

```bash
grep -v "ERROR" application.log
```

---

# 18. The Most Important Concept

Make students remember this:

```text
                 grep
                  ↓
       Search inside text
                  ↓
        ┌─────────────────┐
        │ Large file      │
        │ 1000 lines      │
        │ 5000 lines      │
        └─────────────────┘
                  ↓
            "ERROR"
                  ↓
       Only matching lines
```

### One-line definition for notes

> **`grep` is a Linux command used to search and filter lines containing specific text from files or command output.**

And for your Linux fresher sequence, I would teach:

**`cat` → `grep` → `grep -i` → `grep -n` → `grep -v` → `grep -c` → `grep -r` → `grep` with `|` (pipe).**
