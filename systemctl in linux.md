### What is `systemctl` in Linux?

`systemctl` is a **command used to manage services and system processes** in Linux systems that use **systemd**.

Think of it as a **remote control for Linux services**. You can use it to:

* Start a service
* Stop a service
* Restart a service
* Check service status
* Enable a service at boot
* Disable a service at boot

### Basic syntax

```bash
systemctl <action> <service-name>
```

### Common commands

| Command                      | Purpose                                      |
| ---------------------------- | -------------------------------------------- |
| `systemctl status nginx`     | Check Nginx status                           |
| `systemctl start nginx`      | Start Nginx                                  |
| `systemctl stop nginx`       | Stop Nginx                                   |
| `systemctl restart nginx`    | Restart Nginx                                |
| `systemctl enable nginx`     | Start Nginx automatically after reboot       |
| `systemctl disable nginx`    | Don't start Nginx automatically after reboot |
| `systemctl is-active nginx`  | Check if service is currently active         |
| `systemctl is-enabled nginx` | Check if service starts at boot              |

### 🧑‍💻 Simple hands-on

If Nginx is installed:

**1. Check status**

```bash
sudo systemctl status nginx
```

**2. Start Nginx**

```bash
sudo systemctl start nginx
```

**3. Check again**

```bash
sudo systemctl status nginx
```

You should see:

```text
Active: active (running)
```

**4. Stop Nginx**

```bash
sudo systemctl stop nginx
```

**5. Start automatically after server reboot**

```bash
sudo systemctl enable nginx
```

### Important difference

`systemctl` manages **services**, while commands such as `ps`, `top`, `kill`, and `pgrep` are commonly used to **view and control individual processes**.

For DevOps students, remember:

```text
systemctl
   ↓
Manage Services
   ↓
start / stop / restart / status / enable / disable
```

For example:

```bash
sudo systemctl restart jenkins
sudo systemctl status docker
sudo systemctl stop nginx
```

This is one of the most important Linux commands for DevOps because Jenkins, Docker, Nginx, SSH, and many other applications run as services.
