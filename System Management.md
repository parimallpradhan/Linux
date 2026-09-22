# System Management in Linux

## 1. What is System Management in Linux?

**System Management** in Linux means **checking, controlling, and maintaining the Linux server/system** so that it works properly.

As a Linux administrator or DevOps engineer, we regularly need to:

* Check whether the server is running properly
* Check CPU and memory usage
* Check disk space
* Start, stop, and restart services
* Check running processes
* Check system uptime
* Shut down or reboot the server

### Simple definition for students

> **System Management = Monitoring and controlling the health and operation of a Linux system.**

---

# 2. Why Do We Need System Management?

Imagine we have a company server running a **Java application**.

One morning, users report:

> "The application is not opening."

As a Linux/DevOps engineer, we need to investigate.

We may need to check:

```text
Is the server running?
       ↓
Is CPU overloaded?
       ↓
Is memory available?
       ↓
Is disk full?
       ↓
Is the application service running?
       ↓
Are there any problematic processes?
       ↓
Restart service if required
```

So system management helps us **identify and fix server problems**.

---

# 3. Problem Statement

### Real-world scenario

You are working as a **Linux Support Engineer** for ABC Technologies.

Your company has a Linux server hosting a web application.

Users report:

> "The application is slow and sometimes unavailable."

Your responsibility is to:

1. Check server uptime
2. Check CPU and memory
3. Check disk space
4. Check running processes
5. Check application service
6. Restart the service if required
7. Reboot the server if necessary

This is a basic **Linux System Management** task.

---

# 4. Hands-On Lab

We will use simple Linux commands.

## Step 1: Check System Information

Run:

```bash
uname -a
```

Example:

```text
Linux server01 5.15.0-105-generic x86_64 GNU/Linux
```

### Why?

To understand the Linux kernel and system architecture.

---

# 5. Check Hostname

```bash
hostname
```

Example:

```text
server01
```

You can also use:

```bash
hostnamectl
```

This provides more system information.

Example:

```text
Static hostname: server01
Operating System: Ubuntu
Kernel: Linux 5.15...
Architecture: x86-64
```

### Real-world use

When you manage multiple servers:

```text
web-server
app-server
db-server
jenkins-server
```

You need to know which server you are connected to.

---

# 6. Check System Uptime

Run:

```bash
uptime
```

Example:

```text
22:30:10 up 15 days, 4:20, 2 users, load average: 0.25, 0.30, 0.28
```

It tells us:

* Current time
* How long the server has been running
* Number of logged-in users
* System load

### Simple explanation

```text
up 15 days
```

means the server has been running continuously for 15 days.

---

# 7. Check CPU and Memory

Use:

```bash
top
```

You will see something similar to:

```text
Tasks: 150 total
%Cpu(s): 12.5 us, 3.2 sy
MiB Mem :  7980 total
MiB Swap:  2048 total
```

Press:

```text
q
```

to exit.

### Why?

Suppose users complain:

> "Application is very slow."

You can check whether CPU or memory is overloaded.

---

# 8. Check Memory

Use:

```bash
free -h
```

Example:

```text
              total    used    free
Mem:           7.7Gi   3.2Gi   2.1Gi
Swap:          2.0Gi   200Mi   1.8Gi
```

The `-h` means **human-readable**.

Without `-h`:

```bash
free
```

With `-h`:

```bash
free -h
```

The second output is easier to understand.

---

# 9. Check Disk Space

Run:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use%
/dev/root        30G   18G   11G  63%
```

### Important column

```text
Use%
```

If you see:

```text
95%
```

or

```text
100%
```

there may be a disk-space problem.

### Real-world scenario

Application is not working.

You check:

```bash
df -h
```

and find:

```text
/dev/root   30G   30G   0G   100%
```

The disk is full.

You would then investigate logs, temporary files, old packages, etc.

---

# 10. Check Running Processes

Run:

```bash
ps
```

For more information:

```bash
ps aux
```

Example:

```text
root      1000  0.1  1.2  java
ubuntu    1200  0.0  0.5  nginx
```

### Why?

A Linux server can have hundreds of processes running.

For example:

```text
Java
Nginx
SSH
Docker
Jenkins
MySQL
```

We can use `ps` to identify running processes.

---

# 11. Search for a Specific Process

Suppose we want to check Java.

```bash
ps aux | grep java
```

Example:

```text
ubuntu   2450  2.5  8.2  java -jar employee-app.jar
```

This tells us that the Java application is running.

---

# 12. Check Services

Modern Linux systems commonly use **systemd** to manage services.

Check a service:

```bash
systemctl status nginx
```

Example:

```text
Active: active (running)
```

This means Nginx is running.

---

# 13. Start a Service

Suppose Nginx is stopped.

Run:

```bash
sudo systemctl start nginx
```

Then verify:

```bash
sudo systemctl status nginx
```

Expected:

```text
Active: active (running)
```

---

# 14. Stop a Service

To stop Nginx:

```bash
sudo systemctl stop nginx
```

Check:

```bash
sudo systemctl status nginx
```

Now it should show something similar to:

```text
Active: inactive (dead)
```

---

# 15. Restart a Service

This is very common in DevOps.

```bash
sudo systemctl restart nginx
```

Then:

```bash
sudo systemctl status nginx
```

### Real-world use

Suppose you deployed a new configuration:

```text
New configuration
       ↓
Restart Nginx
       ↓
Nginx loads new configuration
```

---

# 16. Enable Service at Boot

Suppose we want Nginx to automatically start whenever the server boots.

Run:

```bash
sudo systemctl enable nginx
```

Check:

```bash
sudo systemctl is-enabled nginx
```

Expected:

```text
enabled
```

### Important difference

```bash
start
```

Starts the service **now**.

```bash
enable
```

Configures the service to start **automatically after boot**.

---

# 17. Check Logged-in Users

Run:

```bash
who
```

Example:

```text
ubuntu   pts/0   2026-09-22 21:30
```

Another useful command:

```bash
w
```

It gives information about logged-in users and what they are doing.

---

# 18. Check Date and Time

```bash
date
```

Example:

```text
Tue Sep 22 22:40:10 IST 2026
```

Why is this useful?

For troubleshooting logs.

For example:

```text
Application error: 22:35
Server time: 22:40
```

Time synchronization is important when investigating production issues.

---

# 19. Reboot the Server

When a reboot is required:

```bash
sudo reboot
```

The server will restart.

### Important

Do **not** randomly reboot a production server.

First check:

* Is the server production?
* Are users connected?
* Is there a maintenance window?
* Is approval required?
* Is there another server handling traffic?

---

# 20. Shutdown the Server

To shut down:

```bash
sudo shutdown -h now
```

This completely shuts down the system.

Again, be careful on production systems.

---

# 21. Simple System Management Flow

```text
              LINUX SERVER
                   │
                   ↓
        ┌─────────────────────┐
        │ Check System Health │
        └──────────┬──────────┘
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
      CPU        Memory       Disk
       │           │           │
       └───────────┼───────────┘
                   ↓
           Check Processes
                   ↓
           Check Services
                   ↓
       ┌───────────┴───────────┐
       ↓                       ↓
 Service Running          Service Down
       │                       │
       ↓                       ↓
     Monitor              Start/Restart
```

---

# 22. Important Commands for Freshers

| Requirement            | Command                   |
| ---------------------- | ------------------------- |
| System information     | `uname -a`                |
| Hostname               | `hostname`                |
| Detailed system info   | `hostnamectl`             |
| Uptime                 | `uptime`                  |
| CPU/process monitoring | `top`                     |
| Memory                 | `free -h`                 |
| Disk space             | `df -h`                   |
| Processes              | `ps aux`                  |
| Search process         | `ps aux \| grep java`     |
| Service status         | `systemctl status nginx`  |
| Start service          | `systemctl start nginx`   |
| Stop service           | `systemctl stop nginx`    |
| Restart service        | `systemctl restart nginx` |
| Enable at boot         | `systemctl enable nginx`  |
| Logged-in users        | `who`                     |
| Current activity       | `w`                       |
| Date/time              | `date`                    |
| Reboot                 | `reboot`                  |

---

## 23. One Complete Hands-On Scenario

Let's make this a **real DevOps support exercise**.

### Problem

> "The company website is not accessible."

### Step 1 — Check server

```bash
hostname
```

### Step 2 — Check uptime

```bash
uptime
```

### Step 3 — Check disk

```bash
df -h
```

### Step 4 — Check memory

```bash
free -h
```

### Step 5 — Check processes

```bash
ps aux
```

### Step 6 — Check Nginx

```bash
sudo systemctl status nginx
```

Suppose you find:

```text
Active: inactive (dead)
```

### Step 7 — Start Nginx

```bash
sudo systemctl start nginx
```

### Step 8 — Verify

```bash
sudo systemctl status nginx
```

Now:

```text
Active: active (running)
```

### Step 9 — Test

From the server:

```bash
curl http://localhost
```

If you receive HTML/application response, Nginx is responding.

---

## Interview Answer

If an interviewer asks:

**"What is system management in Linux?"**

You can say:

> **System management in Linux is the process of monitoring and controlling the Linux server to ensure it is running properly. I use commands such as `uptime`, `top`, `free`, `df`, `ps`, and `systemctl` to check system health, resource utilization, processes, disk space, and services. If a service is down, I investigate the issue and, if appropriate, restart or start the service.**

### For freshers, teach these 6 areas first:

1. **System information** — `uname`, `hostname`
2. **System health** — `uptime`, `top`
3. **Memory & disk** — `free`, `df`
4. **Process management** — `ps`
5. **Service management** — `systemctl`
6. **Reboot/shutdown** — `reboot`, `shutdown`

---


# Problem statement

## Users are reporting that the payment application is very slow, and some users are unable to complete payments.

<img width="612" height="408" alt="ChatGPT_Image_Sep_22__2026__11_51_46_PM-removebg-preview" src="https://github.com/user-attachments/assets/d98f55b0-cc30-4b4b-b95e-13738f440d42" />


<img width="612" height="408" alt="ChatGPT_Image_Sep_22__2026__11_51_31_PM-removebg-preview" src="https://github.com/user-attachments/assets/a9162e9f-5b52-403e-9d4f-a6d5d6d291a7" />


📱 Payment App

❌ "Unable to complete payment"
       OR
⏳ "Processing... Please wait"
       OR
🐌 "App is responding very slowly"

Yes. For freshers, I would teach **System Management as one complete hands-on lab**, starting from a fresh Linux server and then creating a simple problem to troubleshoot.

# Linux System Management

## 1. Lab Scenario

Imagine ABC Technologies has a Linux server running a web application.

Your job as a **Linux Support Engineer** is to:

```text
Create Linux Server
       ↓
Connect to Server
       ↓
Check System Information
       ↓
Check CPU / Memory / Disk
       ↓
Install Nginx
       ↓
Manage Nginx Service
       ↓
Create a problem
       ↓
Troubleshoot the problem
       ↓
Fix the problem
```



---

# 2. Step 1 — Create Linux Server

If using AWS:

Create an **Ubuntu EC2 instance**.

Example:

```text
OS          : Ubuntu
Instance    : t3.micro
Port 22     : SSH
Port 80     : HTTP
```

After the instance starts, connect:

```bash
ssh -i mykey.pem ubuntu@<PUBLIC-IP>
```

You should see:

```text
ubuntu@ip-10-0-1-25:~$
```

Now you are inside the Linux server.

---

# 3. Step 2 — First Check: Who Am I?

Run:

```bash
whoami
```

Expected:

```text
ubuntu
```

Explain:

> `whoami` tells us which Linux user we are currently logged in as.

---

# 4. Step 3 — Which Server Am I On?

Run:

```bash
hostname
```

Example:

```text
ip-10-0-1-25
```

Then:

```bash
hostnamectl
```

This gives more information about the server.

---

# 5. Step 4 — Check Linux Version

Run:

```bash
cat /etc/os-release
```

Example:

```text
NAME="Ubuntu"
VERSION="24.04..."
```

Then:

```bash
uname -a
```

Explain the difference simply:

```text
/etc/os-release → Linux distribution information

uname -a       → Kernel/system information
```

---

# 6. Step 5 — Check Server Uptime

Run:

```bash
uptime
```

Example:

```text
22:15:20 up 2 hours, 1 user, load average: 0.05, 0.03, 0.01
```



> "How long has this server been running?"



```text
up 2 hours
```

---

# 7. Step 6 — Check CPU and Processes

Run:

```bash
top
```



```text
CPU
Memory
Processes
Load
```



> Imagine the application is slow. `top` is one of the first commands we can use to check whether the server is overloaded.

Exit:

```text
q
```

---

# 8. Step 7 — Check Memory

Run:

```bash
free -h
```

Example:

```text
              total   used   free
Mem:           1.9Gi   500M   800M
Swap:          1.0Gi     0B   1.0G
```

Explain:

```text
total → total memory
used  → currently used memory
free  → available memory
```

---

# 9. Step 8 — Check Disk

Run:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use%
/dev/root        20G  5.0G   14G  27%
```

Explain:

> `df -h` tells us how much disk space is being used.

This is extremely important for production troubleshooting.

---

# 10. Step 9 — Install Nginx

Now we will create an actual service that we can manage.

First update package information:

```bash
sudo apt update
```

Install Nginx:

```bash
sudo apt install nginx -y
```

Check:

```bash
nginx -v
```

Example:

```text
nginx version: nginx/1.24...
```

---

# 11. Step 10 — Check Nginx Service

Run:

```bash
sudo systemctl status nginx
```

You should see:

```text
Active: active (running)
```

Explain:

> `systemctl` is used to manage services on modern Linux systems using systemd.

---

# 12. Step 11 — Start and Stop Service

Stop Nginx:

```bash
sudo systemctl stop nginx
```

Check:

```bash
sudo systemctl status nginx
```

Now you should see:

```text
Active: inactive (dead)
```

Start it again:

```bash
sudo systemctl start nginx
```

Check:

```bash
sudo systemctl status nginx
```

Expected:

```text
Active: active (running)
```

---

# 13. Step 12 — Restart Service

Run:

```bash
sudo systemctl restart nginx
```

Then:

```bash
sudo systemctl status nginx
```

Explain:

> Restart is commonly used after making configuration changes or when a service needs to be restarted.

---

# 14. Step 13 — Enable Service at Boot

Run:

```bash
sudo systemctl enable nginx
```

Check:

```bash
sudo systemctl is-enabled nginx
```

Expected:

```text
enabled
```

Explain:

```text
start
   ↓
Start service now

enable
   ↓
Start service automatically after server boot
```

---

# 15. Step 14 — Test the Web Server

Run:

```bash
curl http://localhost
```

You should receive HTML output.

For example:

```html
<!DOCTYPE html>
<html>
...
</html>
```

Now we have a real application/service running.

---

# 16. Real Problem


> "Users have reported that the website is not working."

You intentionally stop Nginx:

```bash
sudo systemctl stop nginx
```

Now the application is down.

---

# 17. Troubleshooting — Step by Step

### Step 1: Check server

```bash
hostname
```

### Step 2: Check uptime

```bash
uptime
```

### Step 3: Check disk

```bash
df -h
```

### Step 4: Check memory

```bash
free -h
```

### Step 5: Check processes

```bash
ps aux
```

### Step 6: Check Nginx

```bash
sudo systemctl status nginx
```



```text
Active: inactive (dead)
```

---

# 18. Fix the Problem

Start Nginx:

```bash
sudo systemctl start nginx
```

Verify:

```bash
sudo systemctl status nginx
```

Expected:

```text
Active: active (running)
```

Test again:

```bash
curl http://localhost
```

Website should respond.

---

# 19. Complete Troubleshooting Story

the **reason** behind the commands.

```text
USER
 │
 │ Website not working
 ↓
LINUX SERVER
 │
 ├── hostname
 │
 ├── uptime
 │
 ├── top
 │
 ├── free -h
 │
 ├── df -h
 │
 ├── ps aux
 │
 └── systemctl status nginx
             │
             ↓
       Nginx is DOWN
             │
             ↓
   systemctl start nginx
             │
             ↓
       Nginx is RUNNING
             │
             ↓
       curl localhost
             │
             ↓
          SUCCESS
```

---

# 20. Assignment: Linux Server Health Check

**Problem Statement:**

> You are a Linux Support Engineer. A web application is running on an Ubuntu server. Users report that the application is unavailable. Perform basic system health checks and restore the web service.

### should execute:

```bash
whoami
hostname
hostnamectl
cat /etc/os-release
uname -a
uptime
top
free -h
df -h
ps aux
sudo systemctl status nginx
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl enable nginx
curl http://localhost
```


```text
Linux Server
     ↓
System Information
     ↓
CPU / Memory / Disk
     ↓
Processes
     ↓
Services
     ↓
Troubleshooting
     ↓
Fix
```

