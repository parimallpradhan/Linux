# `diff` Command in Linux

> **`diff` is used to compare two files and identify the differences between them.**

A common Linux/DevOps use case is comparing **two configuration files** or checking what changed between two versions of a file.

---

## 1. SExample

Let's create two files:

```bash
echo "Hello" > file1.txt
echo "Hello" > file2.txt
```

Compare them:

```bash
diff file1.txt file2.txt
```

There will be **no output**.



> **No output means the two files have no differences.**

---

# 2. Create a Difference

Now change `file2.txt`:

```bash
echo "Hello Linux" > file2.txt
```

Compare again:

```bash
diff file1.txt file2.txt
```

You may see:

```text
1c1
< Hello
---
> Hello Linux
```

Don't make beginners memorize `1c1` initially.

Focus on:

```text
< Hello
> Hello Linux
```

Meaning:

```text
< → content from file1
> → content from file2
```

So:

```text
file1.txt
Hello

file2.txt
Hello Linux
```

---

# 3. Why Do We Need `diff`?

Imagine you have two configuration files:

```text
server-dev.conf
server-prod.conf
```

You want to know:

> "What is different between DEV and PROD?"

Instead of opening both files and checking line by line:

```bash
diff server-dev.conf server-prod.conf
```

Linux shows the differences.

---

# 4. Beginner Hands-on

Create a directory:

```bash
mkdir diff-demo
cd diff-demo
```

Create the first file:

```bash
cat > app-dev.conf
```

Enter:

```text
PORT=8080
ENV=DEV
DEBUG=true
```

Press:

```text
Ctrl + D
```

Now create the second:

```bash
cat > app-prod.conf
```

Enter:

```text
PORT=8080
ENV=PROD
DEBUG=false
```

Press:

```text
Ctrl + D
```

Now compare:

```bash
diff app-dev.conf app-prod.conf
```

You should see differences similar to:

```text
2c2
< ENV=DEV
---
> ENV=PROD
3c3
< DEBUG=true
---
> DEBUG=false
```

Explain:

```text
DEV configuration
       ↓
       diff
       ↑
PROD configuration
```

The port is the same:

```text
PORT=8080
```

but:

```text
ENV
DEBUG
```

are different.

---

# 5. What Does `1c1` Mean?

You can explain this later.

For example:

```text
2c2
```

means approximately:

```text
2 → line 2 of first file
c → change
2 → line 2 of second file
```

Common symbols:

| Symbol | Meaning |
| ------ | ------- |
| `c`    | Change  |
| `a`    | Add     |
| `d`    | Delete  |

For beginners, don't spend too much time on these symbols.

---

# 6. Very Useful: `diff -u`

In DevOps, I recommend teaching the **unified format** because it is easier to understand.

Run:

```bash
diff -u app-dev.conf app-prod.conf
```

You may see:

```text
--- app-dev.conf
+++ app-prod.conf
@@
-ENV=DEV
+ENV=PROD
-DEBUG=true
+DEBUG=false
```

This is much easier to read:

```text
- → line removed/different from first file
+ → line added/different in second file
```

So:

```text
-ENV=DEV
+ENV=PROD
```

means:

> DEV has `ENV=DEV`, while PROD has `ENV=PROD`.

---

# 7. Another Simple Example

Create:

```bash
echo "Linux" > student1.txt
echo "Linux" > student2.txt
```

Compare:

```bash
diff student1.txt student2.txt
```

No output.

Now:

```bash
echo "Linux Commands" > student2.txt
```

Compare:

```bash
diff student1.txt student2.txt
```

Output:

```text
1c1
< Linux
---
> Linux Commands
```

---

# 8. Real-World Problem Statement

### Problem

> A DevOps engineer has two configuration files: one from the DEV server and one from the PROD server. The application is behaving differently in the two environments. The engineer wants to identify configuration differences.

Files:

```text
dev.conf
prod.conf
```

Command:

```bash
diff -u dev.conf prod.conf
```

Example:

```text
-PORT=8080
+PORT=9090
```

Now the engineer knows that the port configuration differs.

This is a very good example because students can understand:

```text
Two files
   ↓
Compare
   ↓
Find differences
   ↓
Investigate configuration
```

---

# 9. `diff` vs `cmp`

You may eventually introduce `cmp`.

### `diff`

Shows **what is different** between text files.

```bash
diff file1.txt file2.txt
```

### `cmp`

Checks whether two files differ byte-by-byte.

```bash
cmp file1.txt file2.txt
```

For your beginners, **teach `diff` first**. `cmp` can be introduced later if needed.

---

# 10. Commands Students Should Remember

### Basic comparison

```bash
diff file1.txt file2.txt
```

### Easier-to-read comparison

```bash
diff -u file1.txt file2.txt
```

### Compare configuration files

```bash
diff -u dev.conf prod.conf
```

---

## Simple classroom definition

> **The `diff` command compares two files and shows the differences between them.**

### Easy memory trick

```text
diff
 ↓
DIFFerence
 ↓
Compare two files
 ↓
Find what changed
```

