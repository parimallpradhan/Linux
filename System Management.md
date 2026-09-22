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

