# Ultimate Linux Interview Crash Course

*(Commands + Purpose + Q\&A + Best Practices)*

---

## 1️⃣ What is Linux?

* **Linux** is an open-source Unix-like operating system kernel.
* Used in servers, embedded systems, mobile devices (Android), cloud infrastructure.
* Popular distributions: Ubuntu, CentOS, Fedora, Debian, RedHat.

---

## 2️⃣ Linux Directory Structure

```
/
├── bin   → Essential binary executables
├── boot  → Boot loader files
├── dev   → Device files
├── etc   → Configuration files
├── home  → User home directories
├── lib   → Shared libraries
├── media → Mounted media
├── mnt   → Temporary mount points
├── opt   → Optional application software
├── proc  → Kernel & process info
├── root  → Root user home
├── sbin  → System binaries
├── tmp   → Temporary files
├── usr   → User programs
├── var   → Variable data (logs, spool)
```

---

## 3️⃣ File & Directory Management

| Command          | Purpose                      |
| ---------------- | ---------------------------- |
| `pwd`            | Show current directory path  |
| `ls`             | List files & directories     |
| `ls -l`          | Detailed list view           |
| `cd /path`       | Change directory             |
| `cd ..`          | Move one level up            |
| `mkdir dir_name` | Create directory             |
| `rmdir dir_name` | Remove empty directory       |
| `rm file`        | Delete file                  |
| `rm -r dir`      | Delete directory recursively |
| `cp src dest`    | Copy files                   |
| `mv src dest`    | Move or rename files         |

---

## 4️⃣ File Viewing & Editing

| Command           | Purpose                  |
| ----------------- | ------------------------ |
| `cat file`        | View file content        |
| `less file`       | View file with scroll    |
| `head -n 10 file` | First 10 lines           |
| `tail -n 10 file` | Last 10 lines            |
| `nano file`       | Edit file in Nano editor |
| `vi file`         | Edit file in Vi editor   |

---

## 5️⃣ File Permissions

| Command            | Purpose               |
| ------------------ | --------------------- |
| `ls -l`            | Show file permissions |
| `chmod 755 file`   | Change permissions    |
| `chown user file`  | Change file owner     |
| `chgrp group file` | Change file group     |

Permission Structure:

```
-rwxr-xr--
|   |  |
|   |  └── Others permissions
|   └──── Group permissions
└──────── Owner permissions
```

---

## 6️⃣ Searching

| Command                     | Purpose             |
| --------------------------- | ------------------- |
| `find /path -name file.txt` | Search file by name |
| `grep "pattern" file`       | Search inside files |
| `grep -r "pattern" /path`   | Recursive search    |

---

## 7️⃣ Process Management

| Command       | Purpose                      |
| ------------- | ---------------------------- |
| `ps`          | Show running processes       |
| `ps aux`      | Detailed process list        |
| `top`         | Real-time process monitoring |
| `htop`        | Interactive process viewer   |
| `kill PID`    | Kill process by PID          |
| `kill -9 PID` | Force kill process           |
| `jobs`        | Show background jobs         |
| `bg %1`       | Resume job in background     |
| `fg %1`       | Resume job in foreground     |

---

## 8️⃣ Disk Usage & System Info

| Command         | Purpose              |
| --------------- | -------------------- |
| `df -h`         | Disk space usage     |
| `du -sh folder` | Folder size          |
| `free -h`       | Memory usage         |
| `uptime`        | Show uptime & load   |
| `uname -a`      | Kernel info          |
| `whoami`        | Current user         |
| `hostname`      | Show system hostname |

---

## 9️⃣ User Management

| Command            | Purpose                  |
| ------------------ | ------------------------ |
| `who`              | Show logged-in users     |
| `w`                | Active sessions          |
| `id`               | User ID info             |
| `adduser username` | Create new user          |
| `passwd username`  | Set/change password      |
| `deluser username` | Delete user              |
| `su username`      | Switch user              |
| `sudo command`     | Run command as superuser |

---

## 🔟 Networking

| Command         | Purpose                      |
| --------------- | ---------------------------- |
| `ifconfig`      | Show IP & network interfaces |
| `ip a`          | Show network details         |
| `ping host`     | Test connectivity            |
| `curl url`      | Fetch data from URL          |
| `wget url`      | Download file                |
| `netstat -tuln` | Show open ports              |
| `ss -tuln`      | Show sockets & ports         |

---

## 1️⃣1️⃣ Archiving & Compression

| Command                     | Purpose                    |
| --------------------------- | -------------------------- |
| `tar -cvf file.tar dir`     | Create tar archive         |
| `tar -xvf file.tar`         | Extract tar archive        |
| `tar -czvf file.tar.gz dir` | Create compressed archive  |
| `tar -xzvf file.tar.gz`     | Extract compressed archive |
| `gzip file`                 | Compress file              |
| `gunzip file.gz`            | Decompress file            |

---

## 1️⃣2️⃣ Package Management

**Debian/Ubuntu**

```bash
apt update               # Update package index
apt upgrade              # Upgrade packages
apt install package      # Install package
apt remove package       # Remove package
```

**RedHat/CentOS**

```bash
yum install package      # Install package
yum remove package       # Remove package
yum update               # Update packages
```

---

## 1️⃣3️⃣ Common Interview Q\&A

**Q:** Difference between `>` and `>>`?
**A:** `>` overwrites file, `>>` appends to file.

**Q:** How to check running processes?
**A:** `ps aux` or `top`.

**Q:** How to find a file?
**A:** `find / -name filename`.

**Q:** How to check CPU and memory usage?
**A:** `top`, `htop`, `free -h`.

**Q:** Difference between `su` and `sudo`?
**A:** `su` switches user shell, `sudo` runs command as superuser without switching shell.

---

## 1️⃣4️⃣ Best Practices

* Use `sudo` instead of logging in as root.
* Always double-check `rm -rf` commands.
* Keep system updated.
* Use strong passwords.
* Backup important configs.

---

## 1️⃣5️⃣ Quick Cheat Sheet for Interview

```bash
# Navigation
pwd; ls -l; cd /path

# File Ops
cp src dest; mv src dest; rm file

# Search
find / -name file.txt; grep "text" file

# Processes
ps aux; kill -9 PID; top

# System Info
uname -a; df -h; free -h

# Networking
ping google.com; curl example.com

# Archive
tar -czvf file.tar.gz folder; tar -xzvf file.tar.gz
```

