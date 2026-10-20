# Linux Commands: Concepts, Use Cases, and Step-by-Step Hands-On

This guide is designed for beginners learning Linux for DevOps and Linux Support Engineer roles. You can use it directly to teach students in a practical lab.

We will cover 11 commands:

`head`, `tail`, `less`, `wc`, `find`, `grep`, `sed`, `diff`, `df -h`, `du -sh`, and `lsblk`.

## 1. Understand what each command does

| Command  | Purpose                                   | Real-world use case                               |
| -------- | ----------------------------------------- | ------------------------------------------------- |
| `head`   | Displays the beginning of a file          | Check the first few lines of a configuration file |
| `tail`   | Displays the end of a file                | Check the latest application logs                 |
| `less`   | Reads a file page by page                 | Inspect a large log file                          |
| `wc`     | Counts lines, words, and characters/bytes | Count log lines or records                        |
| `find`   | Searches for files and directories        | Find old logs or large files                      |
| `grep`   | Searches for matching text                | Find errors in application logs                   |
| `sed`    | Processes or modifies text                | Replace text in configuration files               |
| `diff`   | Compares two files                        | Compare DEV and PROD configurations               |
| `df -h`  | Shows filesystem disk usage               | Check whether a server disk is full               |
| `du -sh` | Shows the total size of a directory       | Find how much disk space an application uses      |
| `lsblk`  | Lists disks and partitions                | Identify attached disks before mounting them      |

# 2. Prepare your hands-on lab

You can run these commands on Ubuntu, a Linux EC2 instance, or Ubuntu on WSL.

### Step 1: Create a practice directory

```
mkdir -p ~/linux-lab/logs
mkdir -p ~/linux-lab/config
cd ~/linux-lab
```

Explanation:

- `mkdir` creates directories.
- `-p` creates parent directories if required.
- `~` refers to your home directory.
- `cd` changes the current directory.

### Step 2: Create a sample application log

Run:

```
cat > logs/app.log <<'EOF'
2026-10-20 09:00:00 INFO Application started
2026-10-20 09:01:00 INFO Database connected
2026-10-20 09:02:00 WARNING High memory usage
2026-10-20 09:03:00 ERROR Database connection failed
2026-10-20 09:04:00 INFO Retrying connection
2026-10-20 09:05:00 ERROR Payment service unavailable
2026-10-20 09:06:00 INFO Application recovered
2026-10-20 09:07:00 WARNING High CPU usage
2026-10-20 09:08:00 INFO Health check successful
2026-10-20 09:09:00 INFO Application running
EOF
```

This creates a file containing 10 sample log entries.

Verify it:

```
cat logs/app.log
```

We will use this same file for several commands so students can understand how each command solves a different problem.

# 3. `head` command

## Concept

The `head` command displays the first 10 lines of a file by default.

Use case: An application support engineer wants to inspect the beginning of a log file without displaying the entire file.

### Syntax

```
head [options] filename
```

### Hands-on

Step 1: Display the first 10 lines

```
head logs/app.log
```

Since our file contains 10 lines, all 10 are displayed.

Step 2: Display only the first 3 lines

```
head -n 3 logs/app.log
```

Step 3: Display the first 5 lines

```
head -5 logs/app.log
```

Both `head -5` and `head -n 5` work on typical Linux systems.

### Real-time use case

Suppose a configuration file has 500 lines, and you want to check its first few settings:

```
head -n 5 /etc/ssh/sshd_config
```

This helps you inspect the beginning of the file quickly. It does not modify the file.

# 4. `tail` command

## Concept

The `tail` command displays the last 10 lines of a file by default.

Use case: A DevOps engineer wants to see the most recent errors in an application log.

### Hands-on

Step 1: Display the last 10 lines

```
tail logs/app.log
```

Step 2: Display only the last 3 lines

```
tail -n 3 logs/app.log
```

Step 3: Monitor a log continuously

```
tail -f logs/app.log
```

The terminal stays open and displays new lines when they are appended to the file.

Open a second terminal and run:

```
echo "2026-10-20 09:10:00 ERROR Login service failed" >> ~/linux-lab/logs/app.log
```

Return to the first terminal. The new log entry should appear automatically.

Press `Ctrl+C` to stop monitoring.

### Important distinction

- `head` shows the beginning.
- `tail` shows the end.
- `tail -f` follows new log entries as they are written.

DevOps example: When a Jenkins deployment fails, an engineer may use `tail -f` on the relevant application log to watch startup and error messages.

# 5. `less` command

## Concept

The `less` command lets you read a file one screen at a time. It is especially useful for large files.

Use case: An application generates a log file with 100,000 lines. You need to search through it without dumping the entire file into the terminal.

### Hands-on

Step 1: Open the log file

```
less logs/app.log
```

Step 2: Learn the navigation keys

| Key      | Action                     |
| -------- | -------------------------- |
| `Space`  | Move forward one screen    |
| `b`      | Move backward one screen   |
| `Enter`  | Move down one line         |
| `g`      | Go to the beginning        |
| `G`      | Go to the end              |
| `/ERROR` | Search forward for `ERROR` |
| `n`      | Find the next match        |
| `N`      | Find the previous match    |
| `q`      | Exit                       |

Step 3: Search for errors

While inside `less`, type:

```
/ERROR
```

Press `Enter`. The command moves to a matching occurrence.

Then press `n` to find the next occurrence.

### Real-time use case

Read a large system log:

```
less /var/log/syslog
```

On some distributions, such as certain minimal Ubuntu installations or Amazon Linux configurations, this file may not exist. You can inspect the system journal instead:

```
sudo journalctl
```

Unlike `cat`, `less` does not print the entire file at once. It also does not modify the file.

# 6. `wc` command

## Concept

`wc` stands for word count. It counts lines, words, bytes, or characters in a file.

Use case: Count how many log entries exist, how many lines are in a configuration file, or how many records a text file contains.

### Syntax

```
wc [options] filename
```

### Hands-on

Step 1: Count lines, words, and bytes

```
wc logs/app.log
```

Example output:

```
10  41  444 logs/app.log
```

The exact word and byte counts depend on the file contents. The columns mean:

- First number: lines
- Second number: words
- Third number: bytes
- Last field: filename

Step 2: Count only lines

```
wc -l logs/app.log
```

Expected output:

```
10 logs/app.log
```

Step 3: Count words

```
wc -w logs/app.log
```

Step 4: Count bytes

```
wc -c logs/app.log
```

Step 5: Count characters

```
wc -m logs/app.log
```

### Real-time use case: Count application errors

```
grep -i "ERROR" logs/app.log | wc -l
```

This command searches for lines containing `ERROR` and counts the matching lines.

Our sample file contains 2 error entries.

Here, `|` is called a pipe. It passes the output of one command to another command.

# 7. `find` command

## Concept

The `find` command searches for files and directories based on criteria such as name, type, size, and modification time.

Use case: Find log files, locate a missing configuration file, or identify old files that may need cleanup.

### Syntax

```
find <starting-directory> <conditions>
```

### Hands-on

Step 1: Find all files and directories

```
find ~/linux-lab
```

Step 2: Find files by name

```
find ~/linux-lab -name "app.log"
```

Step 3: Find all `.log` files

First, create another log file:

```
touch logs/error.log
```

Now run:

```
find ~/linux-lab -type f -name "*.log"
```

Explanation:

- `-type f` searches for regular files.
- `-name "*.log"` matches names ending in `.log`.
- Quotes protect the wildcard from being expanded by the shell.

Step 4: Find directories only

```
find ~/linux-lab -type d
```

Step 5: Find files larger than 1 MB

```
find ~/linux-lab -type f -size +1M
```

This may return no output because our practice files are small.

Step 6: Find files modified within the last 24 hours

```
find ~/linux-lab -type f -mtime -1
```

Step 7: Find files and display their sizes

```
find ~/linux-lab -type f -exec ls -lh {} \;
```

Here, `{}` represents each matching file, and `\;` marks the end of the `-exec` command.

### Real-time DevOps use case

Find log files older than 7 days:

```
find /var/log/myapp -type f -name "*.log" -mtime +7
```

This only lists matching files. It does not delete them.

After reviewing the results, a cleanup command might be used, but do not add `-delete` until you have confirmed the correct directory and retention requirements.

# 8. `grep` command

## Concept

The `grep` command searches for text matching a pattern in files or command output.

Use case: Find errors, failed logins, HTTP error messages, or a specific configuration setting.

### Syntax

```
grep [options] "pattern" filename
```

### Hands-on

Step 1: Search for errors

```
grep "ERROR" logs/app.log
```

Expected matching entries:

```
2026-10-20 09:03:00 ERROR Database connection failed
2026-10-20 09:05:00 ERROR Payment service unavailable
```

Step 2: Ignore uppercase/lowercase differences

```
grep -i "error" logs/app.log
```

This can match `ERROR`, `Error`, or `error`.

Step 3: Display line numbers

```
grep -n "ERROR" logs/app.log
```

Step 4: Count matching lines

```
grep -c "ERROR" logs/app.log
```

Expected result:

```
2
```

Step 5: Search recursively inside a directory

```
grep -r "ERROR" ~/linux-lab
```

Step 6: Exclude matching lines

```
grep -v "INFO" logs/app.log
```

This displays lines that do not contain `INFO`.

Step 7: Search for multiple error types

```
grep -E "ERROR|WARNING" logs/app.log
```

This matches lines containing either `ERROR` or `WARNING`.

### Real-time DevOps use case

Check the latest application logs for failures:

```
tail -n 100 logs/app.log | grep -i "error"
```

This examines only the last 100 lines and filters them for errors.

On a server with a large log file, this is often more convenient than reading every line.

Remember: `grep` searches text; `find` searches for files and directories.

# 9. `sed` command

## Concept

`sed` stands for stream editor. It processes text and can perform operations such as substitution, deletion, and printing selected lines.

Use case: Replace a value in a configuration file, transform text, or remove unwanted lines from command output.

### Syntax

```
sed 's/old-text/new-text/' filename
```

The expression `s/old-text/new-text/` means substitute the old text with the new text.

### Hands-on

First, create a configuration file:

```
cat > config/app.conf <<'EOF'
APP_NAME=ShoppingApp
ENVIRONMENT=DEV
LOG_LEVEL=INFO
SERVER_PORT=8080
EOF
```

Verify it:

```
cat config/app.conf
```

Step 1: Replace DEV with QA

```
sed 's/DEV/QA/' config/app.conf
```

The output shows `ENVIRONMENT=QA`, but the original file remains unchanged.

Step 2: Replace every occurrence on each line

```
sed 's/INFO/DEBUG/g' config/app.conf
```

The `g` flag means replace all occurrences on each line, rather than only the first.

Step 3: Replace text and save changes

```
sed -i 's/DEV/QA/' config/app.conf
```

Now verify:

```
cat config/app.conf
```

The file has been modified.

Step 4: Delete lines containing a word from the output

```
sed '/LOG_LEVEL/d' config/app.conf
```

This displays the file without the `LOG_LEVEL` line. It does not delete that line from the original file.

Step 5: Display only a particular line

```
sed -n '2p' config/app.conf
```

- `-n` suppresses automatic printing.
- `2p` prints line 2.

### Real-time DevOps use case

Suppose a deployment requires changing an environment value:

```
sed -i 's/ENVIRONMENT=DEV/ENVIRONMENT=QA/' config/app.conf
```

This is useful in simple scripts, but for production deployments, prefer environment variables, templates, or configuration management when appropriate.

Important: Before using `sed -i` on a production configuration file, take a backup and verify the replacement pattern.

For example:

```
cp config/app.conf config/app.conf.bak
```

# 10. `diff` command

## Concept

The `diff` command compares two files and displays their differences.

Use case: Compare configuration files between DEV and PROD, review changes before deployment, or check whether two versions of a file are identical.

### Hands-on

Step 1: Create two configuration files

```
cat > config/dev.conf <<'EOF'
APP_NAME=ShoppingApp
ENVIRONMENT=DEV
SERVER_PORT=8080
EOF
```

```
cat > config/prod.conf <<'EOF'
APP_NAME=ShoppingApp
ENVIRONMENT=PROD
SERVER_PORT=8081
EOF
```

Step 2: Compare the files

```
diff config/dev.conf config/prod.conf
```

The output identifies lines that differ. Exact line markers and ordering may vary, but you should see differences in the environment and port values.

Step 3: Compare in a readable format

```
diff -u config/dev.conf config/prod.conf
```

The unified format uses:

- `-` for lines in the first file that differ or are removed.
- `+` for lines in the second file that differ or are added.
- Context lines to show the surrounding content.

Step 4: Check whether files are identical

```
diff -s config/dev.conf config/prod.conf
```

If they are identical, `diff` reports that the files are identical.

### Real-time DevOps use case

Before deploying an updated configuration, compare the existing and proposed versions:

```
diff -u config/prod.conf config/prod-new.conf
```

Review the output before applying changes.

Remember: `diff` compares content; `grep` searches for text.

# 11. `df -h` command

## Concept

The `df` command stands for disk filesystem. It shows how much space is available and used on mounted filesystems.

The `-h` option displays sizes in a human-readable format such as GB and MB.

Use case: A Linux server is running out of disk space, and you need to identify which filesystem is full.

### Syntax

```
df -h
```

### Hands-on

Step 1: Check disk usage

```
df -h
```

Example output:

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        20G   15G  4.0G  79% /
tmpfs           1.0G     0  1.0G   0% /dev/shm
/dev/nvme1n1p1   50G   10G   40G  20% /data
```

Example values only; actual output depends on your machine.

| Column     | Meaning                                      |
| ---------- | -------------------------------------------- |
| Filesystem | Disk or filesystem name                      |
| Size       | Total filesystem size                        |
| Used       | Space already used                           |
| Avail      | Available space                              |
| Use%       | Percentage used                              |
| Mounted on | Directory where the filesystem is accessible |

Step 2: Check the root filesystem

```
df -h /
```

Step 3: Check a specific directory's filesystem

```
df -h /var/log
```

This tells you which mounted filesystem contains `/var/log` and how much space is available there.

### Real-time DevOps use case

Suppose a Jenkins server cannot build a project because its disk is full.

Run:

```
df -h
```

If the root filesystem is at 98%, investigate where the space is being consumed before removing anything.

For example:

```
sudo du -sh /var/lib/jenkins
sudo du -sh /var/log
```

Important distinction: `df -h` reports filesystem usage, not the size of one specific directory.

# 12. `du -sh` command

## Concept

The `du` command stands for disk usage. It reports the space consumed by files and directories.

- `-s` means summarize the total.
- `-h` means show human-readable sizes.

Use case: Identify which application, log directory, or backup directory is consuming disk space.

### Hands-on

Step 1: Check the size of your practice directory

```
du -sh ~/linux-lab
```

Example output:

```
16K    /home/user/linux-lab
```

The actual size will depend on your files and filesystem.

Step 2: Check the size of a specific directory

```
du -sh ~/linux-lab/logs
```

Step 3: Check the size of every subdirectory

```
du -h --max-depth=1 ~/linux-lab
```

This shows a summary for each immediate subdirectory.

On systems where `--max-depth` is unavailable, check the local `du` manual.

Step 4: Identify large directories under `/var`

```
sudo du -h --max-depth=1 /var
```

This can help identify whether logs, caches, or application data consume significant space.

### Real-time DevOps use case

A server's root disk is almost full. You run:

```
df -h
```

Then:

```
sudo du -sh /var/log
sudo du -sh /var/lib/jenkins
sudo du -sh /tmp
```

These commands help you investigate possible sources of disk usage.

If `/var/log` is large, inspect individual files before taking action.

### Difference between `df -h` and `du -sh`

| `df -h`                                  | `du -sh`                                   |
| ---------------------------------------- | ------------------------------------------ |
| Reports filesystem usage                 | Reports file/directory space usage         |
| Shows available space                    | Shows how much space a directory consumes  |
| Useful for identifying a full filesystem | Useful for investigating large directories |

For example, `df -h /` may show 95% disk usage, while `sudo du -sh /var/log` may show that the log directory uses 4 GB.

# 13. `lsblk` command

## Concept

The `lsblk` command stands for list block devices. It displays disks, partitions, and their relationships.

Use case: You have attached a new EBS volume to an AWS EC2 instance and need to identify the device before formatting and mounting it.

### Syntax

```
lsblk [options]
```

### Hands-on

Step 1: List available disks

```
lsblk
```

Example output:

```
NAME         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
nvme0n1      259:0    0   20G  0 disk
└─nvme0n1p1  259:1    0   20G  0 part /
nvme1n1      259:2    0   50G  0 disk
```

Illustrative output. Device names vary by Linux distribution and instance type.

Explanation:

- `nvme0n1`: a disk device.
- `nvme0n1p1`: a partition on that disk.
- `disk`: the whole block device.
- `part`: a partition.
- `/`: the mount point for the root filesystem.
- A blank mount point can indicate a device that is not mounted, but does not prove it is unused.

Step 2: Display filesystem information

```
lsblk -f
```

This includes filesystem types, labels, UUIDs, and mount points where available.

Step 3: Display sizes and mount points

```
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
```

### Real-time AWS EC2 use case

Suppose you attach a 10 GB EBS volume to an EC2 instance.

The workflow is:

1\. Attach EBS volume

AWS Console or CLI

2\. Identify the disk

`lsblk`

3\. Inspect filesystem

`lsblk -f`

4\. Prepare and mount

Only after confirming the correct device

Safety warning: Never format a disk just because it has no mount point. It may contain important data or an existing filesystem.

For a genuinely new, confirmed-empty volume, the general sequence is:

```
lsblk -f
sudo mkfs.ext4 /dev/DEVICE
sudo mkdir -p /data
sudo mount /dev/DEVICE /data
df -h /data
```

Replace `/dev/DEVICE` with the verified device path. Formatting destroys existing filesystem data, so do not run the example literally against an unidentified disk. For production systems, also plan persistent mounting through `/etc/fstab` using the filesystem UUID.

# 14. Combined DevOps hands-on exercise

Now combine the commands in a single troubleshooting scenario.

## Problem statement

Your team reports that a Linux application is slow. The application log may contain errors, the log directory may be consuming too much space, and the server's disk usage must be checked.

Run the following steps in order.

## Lab checklist

0 of 9

1\. Check filesystem usage

Identify filesystems with high disk utilization.

df -h

&#x20;Copy

2\. Check application directory size

Measure the total size of the practice directory.

du -sh \~/linux-lab

&#x20;Copy

3\. Find log files

Locate all matching log files.

find \~/linux-lab -type f -name '\*.log'

&#x20;Copy

4\. Read recent log entries

Inspect the last five log lines.

tail -n 5 \~/linux-lab/logs/app.log

&#x20;Copy

5\. Search for application errors

Find error messages and their line numbers.

grep -n 'ERROR' \~/linux-lab/logs/app.log

&#x20;Copy

6\. Count errors

Count matching error lines.

grep -c 'ERROR' \~/linux-lab/logs/app.log

&#x20;Copy

7\. Check log line count

Count total lines in the log.

wc -l \~/linux-lab/logs/app.log

&#x20;Copy

8\. Compare configurations

Identify differences between DEV and PROD.

diff -u \~/linux-lab/config/dev.conf \~/linux-lab/config/prod.conf

&#x20;Copy

9\. Inspect disks and partitions

Check block devices and filesystem information.

lsblk -f

&#x20;Copy

Reset checklist

## 15. Interview questions for students

1. What is the difference between `head` and `tail`?
2. How do you monitor a continuously growing log file?
3. What is the difference between `less` and `cat`?
4. How do you count lines in a file?
5. What is the difference between `find` and `grep`?
6. What is the purpose of `sed`? Does it always modify the original file?
7. How do you compare two configuration files?
8. What is the difference between `df -h` and `du -sh`?
9. What does `lsblk` display?
10. How would you investigate a Linux server whose disk utilization is 95%?

## 16. Quick revision sheet

| Requirement               | Command                       |
| ------------------------- | ----------------------------- |
| First 5 lines             | `head -n 5 file.txt`          |
| Last 5 lines              | `tail -n 5 file.txt`          |
| Follow a log              | `tail -f app.log`             |
| Read page by page         | `less file.txt`               |
| Count lines               | `wc -l file.txt`              |
| Find a file               | `find /path -name "file.txt"` |
| Search for errors         | `grep "ERROR" app.log`        |
| Replace text in output    | `sed 's/old/new/' file.txt`   |
| Compare files             | `diff -u file1 file2`         |
| Check filesystem space    | `df -h`                       |
| Check directory size      | `du -sh /path`                |
| List disks and partitions | `lsblk -f`                    |

Recommended teaching sequence: Start with `head`, `tail`, and `less`; then teach `wc`, `find`, and `grep`; follow with `sed` and `diff`; and finish with `df -h`, `du -sh`, and `lsblk`. This takes students from basic file inspection to practical Linux troubleshooting.
