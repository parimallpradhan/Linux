# `sed` Command in Linux


> **`grep` finds text. `sed` finds text and can modify it.**

Think of it like this:

```text
grep
  ↓
Find something
  ↓
Show it
```

Whereas:

```text
sed
  ↓
Find something
  ↓
Change / delete / print it
```

---

# 1. What is `sed`?

`sed` stands for **Stream Editor**.

It is mainly used to **search, replace, delete, or modify text** in files.


> **`sed` is a Linux command used to search and modify text in files or command output.**

---

# 2. Why do we need `sed`?

Imagine you have a configuration file:

```text
app.conf
```

Inside:

```text
PORT=8080
ENV=DEV
DEBUG=true
```

You want to change:

```text
ENV=DEV
```

to:

```text
ENV=PROD
```

Instead of opening the file and manually editing it, you can use:

```bash
sed -i 's/ENV=DEV/ENV=PROD/' app.conf
```

This is especially useful in **DevOps automation**, where you may need to modify configuration files automatically.

---

# 3. First Hands-on

Create a file:

```bash
mkdir sed-demo
cd sed-demo
```

Create:

```bash
cat > app.conf
```

Enter:

```text
APP_NAME=MyApplication
PORT=8080
ENV=DEV
DEBUG=true
```

Press:

```text
Ctrl + D
```

Check it:

```bash
cat app.conf
```

---

# 4. Basic `sed` Syntax

The most common syntax is:

```bash
sed 's/old/new/' filename
```

For example:

```bash
sed 's/DEV/PROD/' app.conf
```

Output:

```text
APP_NAME=MyApplication
PORT=8080
ENV=PROD
DEBUG=true
```

### Important point

The original file is **not changed**.

`sed` simply displays the modified output.

Check:

```bash
cat app.conf
```

You will still see:

```text
ENV=DEV
```

This is a very important concept.

---

# 5. Understanding `s`

Look at:

```bash
sed 's/DEV/PROD/' app.conf
```

Break it down:

```text
s
↓
substitute

DEV
↓
old text

PROD
↓
new text
```

So:

```text
s/old/new/
```

means:

> Replace `old` with `new`.

---

# 6. Actually Change the File — `-i`

If you want to permanently modify the file:

```bash
sed -i 's/DEV/PROD/' app.conf
```

Now:

```bash
cat app.conf
```

Output:

```text
APP_NAME=MyApplication
PORT=8080
ENV=PROD
DEBUG=true
```

`-i` means:

> Modify the file in place.

---

# 7. Another Simple Example

Suppose:

```text
student.txt
```

contains:

```text
Hello Rahul
Hello Amit
Hello Rahul
```

Run:

```bash
sed 's/Rahul/Parimal/' student.txt
```

Output:

```text
Hello Parimal
Hello Amit
Hello Parimal
```

The original file remains unchanged unless you use `-i`.

---

# 8. Replace Only the First Occurrence

Suppose:

```text
Hello Rahul Rahul
```

Run:

```bash
sed 's/Rahul/Parimal/' file.txt
```

Output:

```text
Hello Parimal Rahul
```

By default, `sed` replaces the **first matching occurrence on each line**.

---

# 9. Replace All Occurrences — `g`

To replace all occurrences on a line:

```bash
sed 's/Rahul/Parimal/g' file.txt
```

`g` means:

> Global — replace all matching occurrences.

Example:

```text
Before:
Hello Rahul Rahul

After:
Hello Parimal Parimal
```

---

# 10. Delete a Line

`sed` can also delete lines.

Suppose:

```text
app.conf
```

contains:

```text
APP_NAME=MyApplication
PORT=8080
DEBUG=true
ENV=PROD
```

Delete line 3:

```bash
sed '3d' app.conf
```

Output:

```text
APP_NAME=MyApplication
PORT=8080
ENV=PROD
```

Here:

```text
3d
││
│└── delete
└── line 3
```

Again, without `-i`, the original file is not modified.

To actually delete line 3:

```bash
sed -i '3d' app.conf
```

---

# 11. Print Specific Lines

Suppose:

```text
app.conf
```

has:

```text
APP_NAME=MyApplication
PORT=8080
ENV=PROD
DEBUG=true
```

To display line 2:

```bash
sed -n '2p' app.conf
```

Output:

```text
PORT=8080
```

Meaning:

```text
-n → don't print everything
2p → print line 2
```

---

# 12. `grep` vs `sed`

This is very important for your students.

Suppose:

```text
ENV=DEV
```

### `grep`

```bash
grep "ENV" app.conf
```

Output:

```text
ENV=DEV
```

`grep` says:

> "I found it."

### `sed`

```bash
sed 's/DEV/PROD/' app.conf
```

Output:

```text
ENV=PROD
```

`sed` says:

> "I found it and changed it in the output."

So:

```text
grep
 ↓
SEARCH / FILTER

sed
 ↓
SEARCH / MODIFY
```

---

# 13. Real-World DevOps Example

This is a great example for your students.

### Problem Statement

> The same application is deployed in DEV, QA, and PROD. The configuration file contains an environment value. During deployment, the DevOps engineer needs to change the environment automatically.

Configuration:

```text
app.conf
```

Initially:

```text
ENV=DEV
PORT=8080
```

During PROD deployment:

```bash
sed -i 's/ENV=DEV/ENV=PROD/' app.conf
```

Now:

```bash
cat app.conf
```

Output:

```text
ENV=PROD
PORT=8080
```

This demonstrates why `sed` is useful in automation.

---

# 14. Another DevOps Example

Suppose a configuration file contains:

```text
SERVER=dev.example.com
PORT=8080
```

For production deployment, you need:

```text
SERVER=prod.example.com
```

You can run:

```bash
sed -i 's/dev.example.com/prod.example.com/' app.conf
```

Verify:

```bash
cat app.conf
```

---

# 15. Important Warning About `-i`

When you use:

```bash
sed -i
```

you are modifying the actual file.

So teach students this habit:

### First check

```bash
sed 's/DEV/PROD/' app.conf
```

### Verify the output

Then, if correct:

```bash
sed -i 's/DEV/PROD/' app.conf
```

This is safer for beginners.

---

# 16. Beginner Practice

Give students this file:

```bash
cat > employee.txt
```

Enter:

```text
Name=Rahul
Department=IT
Environment=DEV
Status=Active
Environment=DEV
```

### Task 1 — Replace DEV with PROD

```bash
sed 's/DEV/PROD/g' employee.txt
```

### Task 2 — Permanently replace it

```bash
sed -i 's/DEV/PROD/g' employee.txt
```

### Task 3 — Delete line 2

First test:

```bash
sed '2d' employee.txt
```

### Task 4 — Display only line 1

```bash
sed -n '1p' employee.txt
```

---

# 17. Commands I Recommend for Freshers

Don't teach too many `sed` features initially.

Start with these four:

### Search and replace

```bash
sed 's/old/new/' file.txt
```

### Replace all occurrences

```bash
sed 's/old/new/g' file.txt
```

### Modify actual file

```bash
sed -i 's/old/new/' file.txt
```

### Delete a line

```bash
sed '3d' file.txt
```

Then introduce:

```bash
sed -n '2p' file.txt
```

---

## Easy way for students to remember

```text
grep → Find

sed → Find + Change

awk → Find + Process/Report
```

For your Linux fresher course, I'd teach the sequence:

**`grep` → `sed` → `awk`**


