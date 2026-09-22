# What is Process Management in Linux?

### Definition

> **Process Management in Linux means monitoring, controlling, and managing the programs that are currently running on a Linux system.**

Whenever you start a program, Linux creates a **process** to run it.

For example:

```text
You start Java application
        ↓
Linux creates a process
        ↓
Java application keeps running
        ↓
Linux assigns it a PID
```

**PID = Process ID**, a unique number assigned to a running process.

---

# Real-World Example

Let's continue with our **payment application** example.

Suppose the application is running on a Linux App Server:

```text
Customer
   ↓
Web Server
   ↓
App Server
   ↓
Java Payment Application
```

The Java application is running as a Linux process:

```text
java -jar payment-app.jar
          ↓
       Process
          ↓
       PID 2450
```

If users report:

> **"The payment application is very slow."**

As a Linux/DevOps engineer, you might check:

```text
Is the Java process running?
        ↓
How much CPU is it using?
        ↓
How much memory is it using?
        ↓
Is there a problem with the process?
        ↓
Do we need to stop/restart it?
```

That's **process management**.

---

# 1. What is a Process?

A **program** is a file/code stored on disk.

A **process** is that program **while it is running**.

For example:

```text
payment-app.jar
       ↓
   Start Java
       ↓
Running Java process
       ↓
PID = 2450
```

So:

```text
Program = Stored application
Process = Running application
```

---

# 2. What is PID?

PID means:

> **Process ID**

Linux assigns a unique number to each running process.

Run:

```bash
ps aux
```

You may see:

```text
USER       PID   %CPU  %MEM   COMMAND
root         1   0.0   0.5    systemd
ubuntu    2450   5.2   8.2    java -jar payment-app.jar
root      3120   0.1   1.0    nginx
```

Here:

```text
2450
```

is the PID of the Java process.

---

# 3. How to See Running Processes?

### Basic command

```bash
ps
```

More useful:

```bash
ps aux
```

This shows running processes.

For example:

```text
PID
2450
3120
3500
```

---

# 4. Find a Particular Process

Suppose you want to check whether Java is running:

```bash
ps aux | grep java
```

Example:

```text
ubuntu   2450  5.2  8.2  java -jar payment-app.jar
```

This tells you:

```text
Java application
       ↓
Running
       ↓
PID = 2450
```

---

# 5. `top` — Monitor Processes

Run:

```bash
top
```

You can see:

```text
PID
CPU usage
Memory usage
Process name
```

Example:

```text
PID     %CPU    %MEM    COMMAND
2450    85.5    20.2    java
3120     2.1     1.2    nginx
```

Now you might notice:

```text
java → 85.5% CPU
```

This could explain why the application is slow and would be a starting point for further investigation.

Exit:

```text
q
```

---

# 6. Stop a Process

Suppose Java process has:

```text
PID = 2450
```

You can request it to stop:

```bash
kill 2450
```

Then check:

```bash
ps aux | grep java
```

---

# 7. Force Kill a Process

Sometimes a process doesn't stop normally.

You can use:

```bash
kill -9 2450
```

But teach students:

> **Don't use `kill -9` as the first choice.**

Normally try:

```bash
kill 2450
```

first.

Use forceful termination only when appropriate.

---

# 8. Find Process by Name

You can also use:

```bash
pgrep java
```

Example:

```text
2450
```

It gives the PID of the Java process.

---

# 9. Process Management in Your Payment Application

Now create a practical scenario.

### Problem

> Users report that the payment application is very slow.

### Step 1 — Check Java process

```bash
ps aux | grep java
```

### Step 2 — Find PID

```text
2450
```

### Step 3 — Monitor it

```bash
top
```

You discover:

```text
java
CPU = 95%
```

### Step 4 — Investigate

Don't immediately kill the process.

First check:

```text
Application logs
CPU
Memory
Disk
Database connectivity
Recent deployment
```

If the process is confirmed to be stuck and a restart is approved, you can stop/restart the application appropriately.

---

# 10. Important Process Management Commands

| Requirement        | Command               |
| ------------------ | --------------------- |
| Show processes     | `ps`                  |
| Detailed processes | `ps aux`              |
| Monitor processes  | `top`                 |
| Find process       | `ps aux \| grep java` |
| Find PID           | `pgrep java`          |
| Kill process       | `kill <PID>`          |
| Force kill         | `kill -9 <PID>`       |
| Process tree       | `pstree`              |

---

# 11. Diagram

```text
             LINUX SERVER
                  |
                  ↓
        ┌─────────────────┐
        │ Running Programs│
        └────────┬────────┘
                 |
       ┌─────────┼─────────┐
       ↓         ↓         ↓
     Nginx      Java      MySQL
       |         |         |
     PID 100   PID 2450  PID 3000
       |         |         |
       └─────────┼─────────┘
                 ↓
          PROCESS MANAGEMENT
                 |
        ┌────────┼────────┐
        ↓        ↓        ↓
      View     Monitor   Control
      ps       top       kill
```



> **Process Management = Find, monitor, and control running programs in Linux.**

And connect it to your previous **System Management** topic:

```text
System Management
       ↓
Overall server health
       ↓
CPU / Memory / Disk / Services

Process Management
       ↓
Individual running programs
       ↓
Java / Nginx / MySQL / Jenkins
```


## scenario


> **"We have a company website running on a Linux server. Users report that the website is not opening. Our job is to check whether the web-server process is running and fix it."**

```text
👤 USER
   |
   | HTTP Request
   ↓
🌐 WEB BROWSER
   |
   ↓
🖥️ LINUX SERVER
   |
   ↓
⚙️ NGINX PROCESS
   |
   ↓
📄 WEBSITE
```

---

# Hands-on from scratch

## Step 1 — Install Nginx

On Ubuntu:

```bash
sudo apt update
sudo apt install nginx -y
```

Check:

```bash
nginx -v
```

---

## Step 2 — Start the Web Server

```bash
sudo systemctl start nginx
```

Check:

```bash
sudo systemctl status nginx
```

You should see:

```text
Active: active (running)
```

---

## Step 3 — Test the Website

Run:

```bash
curl http://localhost
```

You should get HTML output.

Or, if this is an AWS EC2 server, open:

```text
http://<EC2-PUBLIC-IP>
```

in the browser.

Students can now **visually see the website**.

---

# Step 4 — Explain Process

Now ask:

> **"Nginx is running. But what is actually running inside Linux?"**

Show:

```bash
ps aux | grep nginx
```

Example:

```text
root      1200  0.0  ... nginx: master process
www-data  1201  0.0  ... nginx: worker process
www-data  1202  0.0  ... nginx: worker process
```

Explain:

> **Nginx is a service, and when it is running, Linux has Nginx processes associated with that service.**

This is a good opportunity to distinguish:

```text
Nginx
  ↓
Service

nginx processes
  ↓
Running processes
```

---

# Step 5 — Find the Nginx Process

Use:

```bash
pgrep nginx
```

Example:

```text
1200
1201
1202
```

Explain that Nginx can have **multiple processes**, so students shouldn't expect only one PID.

---

# Step 6 — Monitor Processes

Run:

```bash
top
```

Ask students to find:

```text
nginx
```

They can see:

```text
PID
CPU
Memory
Process
```

Exit:

```text
q
```

---

# Step 7 — Create the Problem

Now tell students:

> 🚨 **Production Alert: Users report that the company website is not opening.**

You intentionally stop Nginx:

```bash
sudo systemctl stop nginx
```

Now test:

```bash
curl http://localhost
```

It should fail.

---

# Step 8 — Students Troubleshoot

Ask:

> **"The website is not working. What should we check?"**

First:

```bash
ps aux | grep nginx
```

Then:

```bash
pgrep nginx
```

They should discover that the Nginx processes are not running.

Then check the service:

```bash
sudo systemctl status nginx
```

They should see:

```text
Active: inactive (dead)
```

---

# Step 9 — Fix the Problem

Start Nginx:

```bash
sudo systemctl start nginx
```

Verify:

```bash
sudo systemctl status nginx
```

Then:

```bash
pgrep nginx
```

Finally test:

```bash
curl http://localhost
```

Or refresh the browser.

Website should work again.

---

# Step 10 — Teach `kill` Carefully

Once students understand the relationship between service and process, you can demonstrate:

```bash
pgrep nginx
```

Suppose:

```text
1200
1201
1202
```

Explain:

> "These are Nginx processes. We normally manage Nginx using `systemctl`, not by randomly killing its processes."

For example:

```bash
sudo systemctl stop nginx
```

is preferred for stopping the service.

Then explain that `kill <PID>` is useful when **you specifically need to manage an individual process**.

---

# The Complete Story

This makes a very nice beginner lesson:

```text
             👤 USER
                |
                ↓
        "Website not working"
                |
                ↓
          🖥️ LINUX SERVER
                |
                ↓
        Check Nginx Service
                |
                ↓
      systemctl status nginx
                |
          ┌─────┴─────┐
          ↓           ↓
       Running       Down
          |           |
          ↓           ↓
    Check Process   Start Nginx
          |           |
          ↓           ↓
       ps / pgrep   Verify
          |           |
          └─────┬─────┘
                ↓
          Test Website
                |
                ↓
          curl localhost
                |
                ↓
             ✅ WORKING
```

### Process Management :

1. **What is a process?**
2. **Web server example — Nginx**
3. Install Nginx
4. Start Nginx
5. Test website
6. Find Nginx process — `ps`
7. Find PID — `pgrep`
8. Monitor process — `top`
9. Create "website down" problem
10. Troubleshoot the process/service
11. Start/restart Nginx
12. Verify website


