#  🐧 Linux Notes 🐧 

**Purpose:**  
This folder serves as my personal Linux cheat sheet. A reference of the most important and frequently used commands, collected while learning through **OverTheWire Bandit**, **SadServers**, and everyday Linux practice.

---

## ⚙️ Environment Setup
I’m running Linux on **Windows** via **WSL (Windows Subsystem for Linux)** with **Zsh** as my default shell and the **Powerlevel10k** theme for a fast and visually rich prompt.

| Tool | Description | Link |
|------|--------------|------|
| 🐧 **WSL 2** | Linux kernel integration on Windows 10/11 | [Install WSL](https://learn.microsoft.com/en-us/windows/wsl/install) |
| 💻 **Zsh** | Default shell replacing Bash | `sudo apt install zsh` |
| 🎨 **Powerlevel10k** | Beautiful Zsh prompt theme | [GitHub → romkatv/powerlevel10k](https://github.com/romkatv/powerlevel10k) |

---

## 🧰 Most Important / Most-Used Linux Commands (at least for me anyways)

| Command | What it does | Example |
|----------|---------------|----------|
| `ls` | List files & directories | `ls -lah` — human-readable listing including hidden files |
| `cd` | Change directory | `cd /var/log` |
| `pwd` | Show current directory | `pwd` |
| `cat` | View file contents | `cat file.txt` |
| `less` | Scroll through file (page view) | `less /var/log/syslog` |
| `head` | Show first lines | `head -n 20 file.txt` |
| `tail` | Show last lines or follow live logs | `tail -f /var/log/nginx/access.log` |
| `grep` | Search text using patterns | `grep -i "error" logfile` |
| `find` | Search files by name/permissions | `find / -type f -name "*.conf" 2>/dev/null` |
| `file` | Identify file type | `file secret.bin` |
| `stat` | Show detailed file info | `stat myscript.sh` |
| `chmod` | Change permissions | `chmod 755 script.sh` |
| `chown` | Change ownership | `chown root:root file.txt` |
| `sudo` | Run commands as root | `sudo apt update` |
| `su` | Switch user | `su -` |
| `ssh` | Remote login via SSH | `ssh user@host -p 22` |
| `scp` | Copy files over SSH | `scp file.txt user@host:/tmp/` |
| `rsync` | Efficient sync/copy tool | `rsync -avP src/ dest/` |
| `tar` | Archive/extract files | `tar -xzvf archive.tar.gz` |
| `gzip` / `gunzip` | Compress/decompress | `gzip file && gunzip file.gz` |
| `zip` / `unzip` | Zip utilities | `unzip archive.zip` |
| `nano` / `vi` | Text editors | `nano notes.txt` or `vi script.sh` |
| `awk` | Field-based text processing | `awk '{print $1,$3}' file` |
| `sed` | Stream text editing | `sed 's/foo/bar/g' file.txt` |
| `cut` | Extract columns | `cut -d: -f1 /etc/passwd` |
| `sort` | Sort lines | `sort file.txt` |
| `uniq` | Remove duplicates | `sort file | uniq -c` |
| `wc` | Count words, lines, chars | `wc -l file.txt` |
| `ps` | Show processes | `ps aux | grep ssh` |
| `top` / `htop` | View running processes interactively | `top` |
| `ss` / `netstat` | Show open ports & sockets | `ss -tulpn` |
| `curl` | Fetch URLs from CLI | `curl -I https://example.com` |
| `wget` | Download files | `wget http://example.com/file` |
| `ip` | Display network info | `ip addr show` |
| `systemctl` | Manage systemd services | `systemctl status sshd` |
| `journalctl` | View system logs | `journalctl -u sshd --since "1 hour ago"` |
| `crontab` | Schedule tasks | `crontab -l` |
| `ln` | Create symlinks | `ln -s /path/to/file ~/link` |
| `mkdir` | Make directories | `mkdir -p ~/projects/bandit` |
| `rm` | Delete files/directories | `rm -rf /tmp/test` (⚠️ be careful) |
| `touch` | Create file / update timestamp | `touch newfile` |
| `history` | Show command history | `history | grep ssh` |
| `strings` | Extract readable text from binaries | `strings binary | less` |
| `tr` | Translate or delete characters | `tr -d '\r' < dosfile > unixfile` |
| `nc` (netcat) | Read/write over network | `nc -lvnp 4444` |
| `openssl` | SSL/TLS and crypto utility | `openssl s_client -connect host:443` |

---

## 💡 Pro Tips
- Use `man <command>` to read a command’s manual.  
- Chain commands with `|` and redirect output with `>` or `>>`.  
- Use `grep`, `awk`, and `sed` together for fast text filtering.  
- When in doubt, `pwd`, `ls`, and `file` tell you what you’re looking at.  
- Keep a separate file like `cheats.md` for personal one-liners.

---

## 🧭 Related Notes
If you want to replicate my setup (and your on windows):
- [Powerlevel10k GitHub](https://github.com/romkatv/powerlevel10k)
- [Oh My Zsh](https://ohmyz.sh/)
- [Install WSL (Microsoft Docs)](https://learn.microsoft.com/en-us/windows/wsl/install)

---



















































































