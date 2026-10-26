# Class Notes: Linux System Commands, Paths & Starbucks Deployment on NGINX 🚀

**Date:** 05-01-2025  
**Topic:** Linux Fundamentals, Path Navigation, Package Management, Service Management, and Static Web Deployment

---

## 📌 1. Architecture & Overview

In this lecture, we explored how Linux servers are navigated, managed, and used to host real-world web applications. The hands-on project demonstrated deploying a **Starbucks static website** onto an **NGINX web server** running on Linux (WSL / Cloud VM / Killercoda environment).

```
   +--------------------+          +------------------------------------------------+
   | Developer Git Repo | -------> | Linux Machine (WSL / Cloud VM / Killercoda)   |
   +--------------------+  Fetch   |                                                |
                            Code   |  1. Install Nginx via apt package manager     |
                                   |  2. Systemctl start & enable nginx             |
                                   |  3. Deploy Starbucks code to /var/www/html     |
                                   |  4. Expose web server on localhost / Port 80   |
                                   +------------------------------------------------+
                                                         |
                                                         v
                                                 [ End User Browser ]
```

---

## 🧭 2. Absolute Path vs. Relative Path

Understanding file system hierarchy is crucial for DevOps automation and scripting.

| Path Type | Definition | Starting Point | Example |
| :--- | :--- | :--- | :--- |
| **Absolute Path** | Complete path from the root directory | Starts with root slash (`/`) | `cd /home/sakshi_bhabhi`<br>`cd /var/www/html` |
| **Relative Path** | Path relative to current working directory | Starts from where you currently are (`.` or `..`) | `cd ../sakshi_bhabhi`<br>`cd ./starbucks-code` |

* **Root Directory (`/`)**: The top of the Linux filesystem tree.
* **Current Directory (`.` or current folder)**: Where your shell is currently located (`pwd`).
* **Parent Directory (`..`)**: One level up in the directory tree.

---

## 🖥️ 3. Essential Linux System & Process Commands

Commands demonstrated on the whiteboard for system inspection and troubleshooting:

* **Hardware & Resource Inspection:**
  * `lscpu`: Displays detailed CPU architecture and core information.
  * `free -h`: Checks available and utilized system RAM in human-readable format (`-h` = GiB/MiB).
  * `df -h`: Displays disk space usage across mounted partitions.
  * `du -h <dir>`: Shows disk space usage of specific directories and files.
* **Operating System & Network Identity:**
  * `cat /etc/os-release`: Shows Linux OS distribution and release version (e.g., Ubuntu 22.04 LTS).
  * `hostname -I`: Displays system IP addresses.
  * `uptime`: Shows how long the machine has been running and load averages.
* **Process Management:**
  * `ps`: Displays currently running processes in the background.
  * `top`: Interactive real-time process viewer (Linux Task Manager equivalent).
  * `kill <PID>`: Terminates a running process by its Process ID.
* **Data Flow & Navigation:**
  * `|` (Pipe): Passes output of one command as input to another (e.g., `ps aux | grep nginx`).
  * `>` (Redirect / Overwrite): Writes output to a file, overwriting existing content.
  * `>>` (Append): Appends output to the end of a file.
  * `head`, `tail`, `less`: View file beginnings, ends, or paginated content.
* **User Management:**
  * `adduser <username>`: Interactive command to create a new user account.

---

## 📦 4. Linux Package Managers Across Operating Systems

Different Linux distributions utilize specific package managers to download, install, and update software:

* **Android:** Google Play Store
* **Apple:** App Store
* **Ubuntu / Debian:** `apt` (Advanced Package Tool)
* **Red Hat Enterprise Linux (RHEL):** `yum`
* **CentOS / Fedora:** `dnf`
* **Alpine Linux:** `apk`

### Working with `apt`
1. `apt update`: Updates the local package index cache from remote repository mirrors (`http://archive.ubuntu.com`).
2. `apt list`: Lists available packages.
3. `apt list --installed`: Lists packages currently installed on the host.
4. `apt search <package_name>`: Searches package index for keywords (e.g., `apt search nginx`).
5. `apt install -y <package_name>`: Downloads and installs software.

---

## ⚙️ 5. Service Management with `systemctl`

Control daemon services managed by Systemd:

```bash
# Check service state
sudo systemctl status nginx

# Start service
sudo systemctl start nginx

# Stop service
sudo systemctl stop nginx

# Restart service
sudo systemctl restart nginx

# Enable service to automatically start on boot
sudo systemctl enable nginx

# Disable service from boot startup
sudo systemctl disable nginx
```

---

## 🛠️ 6. End-to-End Starbucks Website Deployment on NGINX

### Step 1: Obtain a Linux Machine
* Use WSL2 locally, a cloud VM on Azure, or an ephemeral playground such as Killercoda.

### Step 2: Update Package Repositories & Install NGINX
```bash
sudo apt update -y
sudo apt install -y nginx
```

### Step 3: Verify and Start NGINX
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
* Test default landing page: `curl http://localhost` or open public IP in a browser.

### Step 4: Deploy Starbucks Web Application
* Default NGINX document root: `/var/www/html`
* Remove default files and clone/copy Starbucks frontend code:
```bash
cd /var/www/html
sudo rm -rf *
# Copy or git clone application artifacts
git clone https://github.com/<repo>/starbucks-landing-page.git .
```

### Step 5: Test & Validate
* Reload NGINX configuration:
```bash
sudo systemctl reload nginx
```
* Open the browser at `http://<IP_ADDRESS>` to verify the Starbucks website is serving live.

---

## 💡 Key Takeaways
1. Always run `apt update` before `apt install` so package indexes are fresh.
2. Web servers default to `/var/www/html` on Debian/Ubuntu systems.
3. Systemd commands (`systemctl`) are essential for managing long-running background daemons in production.
