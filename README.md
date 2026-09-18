# Linux

## 🧑‍🏫 Linux Software Management

### 1. Start with the problem

Tell students:

> "Suppose our company has a new Linux server. We need to install Nginx web server. How do we install software in Linux?"

Explain:

```text
Windows
   ↓
Download .exe
   ↓
Install Software

Linux
   ↓
Package Manager
   ↓
Download Package + Dependencies
   ↓
Install Software
```

---

# 2. What is a Package?

A **package** is a bundle containing software and the information required to install/manage it.

For example:

```text
nginx package
   │
   ├── nginx program
   ├── configuration files
   ├── dependencies
   └── package metadata
```

Simple analogy:

> **Package = Product box**
> It contains the application and everything required to install/manage it.

---

# 3. What is Package Management?

Package management means:

* Install software
* Remove software
* Update software
* Search software
* Get software information
* Check installed software

```text
             Package Management
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
    Install       Update       Remove
       ↓            ↓            ↓
    nginx         nginx        nginx
```

---

# 4. Package Managers

Teach this table:

| Linux Distribution | Package Manager                    |
| ------------------ | ---------------------------------- |
| Ubuntu/Debian      | `apt`                              |
| RHEL               | `dnf`                              |
| Rocky Linux        | `dnf`                              |
| AlmaLinux          | `dnf`                              |
| CentOS             | `dnf` / `yum`                      |
| Amazon Linux       | `dnf` / `yum` depending on version |

For your **Ubuntu lab**, use `apt`.

---

# 5. Hands-on: Install Nginx

### Step 1 — Update package information

```bash
sudo apt update
```

Explain:

> This does **not normally upgrade installed applications**. It refreshes the local package information from configured repositories.

### Step 2 — Install Nginx

```bash
sudo apt install nginx -y
```

### Step 3 — Verify

```bash
nginx -v
```

or:

```bash
dpkg -l | grep nginx
```

### Step 4 — Check service

```bash
systemctl status nginx
```

Now students can see:

```text
Package installed
      ↓
Application available
      ↓
Service running
```

---

# 6. Connect Software Management With `/etc/passwd`

This is exactly the example you were asking about earlier.

After installing Nginx:

```bash
grep www-data /etc/passwd
```

Students may see:

```text
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

Then:

```bash
id www-data
```

Explain:

> Nginx uses the `www-data` account for its worker processes on Ubuntu/Debian systems. This is a **service account**, not a normal human login account.

Then:

```bash
ps aux | grep nginx
```

They can connect all the concepts:

```text
Install Nginx
     ↓
Nginx package
     ↓
Nginx service
     ↓
www-data service account
     ↓
www-data entry in /etc/passwd
     ↓
Nginx worker process runs as www-data
```

This is an excellent real-world Linux example.

---

# 7. Installing Packages — Commands

## Ubuntu

```bash
sudo apt install nginx
```

Remove:

```bash
sudo apt remove nginx
```

Remove package + configuration files:

```bash
sudo apt purge nginx
```

---

## RHEL-based Linux

For modern RHEL-family systems:

```bash
sudo dnf install nginx
```

Remove:

```bash
sudo dnf remove nginx
```

Older training material may show:

```bash
sudo yum install nginx
sudo yum remove nginx
```

Tell students:

> "`yum` is common in older RHEL/CentOS material. Modern RHEL-family systems primarily use `dnf`."

---

# 8. Updating Packages

### Ubuntu

Update package information:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt upgrade
```

You can explain the difference very simply:

```text
apt update
     ↓
"Tell me what updates are available"

apt upgrade
     ↓
"Install those available updates"
```

---

### RHEL-based

```bash
sudo dnf check-update
```

Update packages:

```bash
sudo dnf update
```

`yum` equivalent commonly seen:

```bash
sudo yum update
```

---

# 9. Get Package Information

### Ubuntu

```bash
apt show nginx
```

Search:

```bash
apt search nginx
```

Check installed package:

```bash
dpkg -l | grep nginx
```

### RHEL-based

```bash
dnf info nginx
```

List packages:

```bash
dnf list
```

List installed packages:

```bash
dnf list installed
```

Search:

```bash
dnf search nginx
```

---

# 10. Very Important Concept: Repository

Freshers often ask:

> "Where does Linux get the software from?"

Explain:

```text
                Internet
                   │
                   ↓
            Package Repository
                   │
                   ↓
             Package Manager
                   │
                   ↓
               Linux Server
```

For example:

```bash
sudo apt install nginx
```

Behind the scenes, the package manager:

```text
1. Reads repository configuration
2. Contacts repository
3. Finds nginx package
4. Finds required dependencies
5. Downloads packages
6. Installs them
7. Configures the software
```

---

# 11. What is a Dependency?

Suppose:

```text
Application A
     ↓
needs
     ↓
Library B
     ↓
needs
     ↓
Library C
```

The package manager helps resolve these dependencies.

This is one of the biggest advantages of using a package manager.

---

# 12. A Good Classroom Practical

Give students this ticket:

### 🎫 Jira Ticket: Install and Manage Nginx

**Problem Statement**

> The application team requires an Nginx web server on the Linux server. Install Nginx, verify the installed package, check its version and service status, identify the service account used by Nginx, and then remove Nginx after testing.

### Hands-on

**1. Update repositories**

```bash
sudo apt update
```

**2. Install Nginx**

```bash
sudo apt install nginx -y
```

**3. Check version**

```bash
nginx -v
```

**4. Check package**

```bash
dpkg -l | grep nginx
```

**5. Check service**

```bash
systemctl status nginx
```

**6. Find service user**

```bash
ps aux | grep nginx
```

**7. Check `/etc/passwd`**

```bash
grep www-data /etc/passwd
```

**8. Check user details**

```bash
id www-data
```

**9. Remove Nginx**

```bash
sudo apt remove nginx -y
```

**10. Verify**

```bash
nginx -v
```

The command should indicate that Nginx is no longer available.

---

