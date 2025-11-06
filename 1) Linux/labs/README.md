# Linux Challenges — Bandit Wargame (OTW) & SadServers

**Summary:** Walkthrough / proof-of-work for the OverTheWire **Bandit** wargame levels I completed, plus a recommended next challenge (*SadServers*) to level-up Linux & pentest fundamentals. This README includes the challenge summary and a compact table of the most important / most-often-used Linux commands with short examples so you can copy them quickly.

## What I completed
### Main challenge: OverTheWire — **Bandit**
I completed Bandit up through level 13 as my primary learning challenge. This exercise is excellent for learning basic Linux navigation, file I/O, permissions, SSH, pipes, compression tools, and simple scripting.

**Proof (passwords you provided):**
- bandit5: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`  
- bandit6: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`  
- bandit7: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`  
- bandit8: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`  
- bandit9: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`  
- bandit10: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`  
- bandit11: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`  
- bandit12: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`  
- bandit13: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

## Next recommended challenge: **SadServers**
If you want to continue improving practical Linux and small-scale exploitation skills after Bandit, **SadServers** (or any beginner-safe CTF that focuses on web services, simple binary exploitation, and service misconfigurations) is an excellent next step. It builds on:

- network/service enumeration (nmap, netcat, ss)
- basic web exploitation (HTTP requests, directory traversal, misconfigured servers)
- reading server logs, automating tasks with shell scripts
- privilege escalation basics

## What I learned
- SSH basics: key-based and password logins; how challenges provide small isolated accounts.
- File & directory inspection: `ls`, `file`, `strings`, `less`, `head`, `tail`.
- Permissions & ownership: `chmod`, `chown`, SUID/SGID concepts.
- Data transformation: `grep`, `awk`, `sed`, `cut`, `tr`, `sort`, `uniq`.
- Remote interaction and debugging: `nc`, `curl`, `wget`.
- Archiving/compression: `tar`, `gzip`, `bzip2`, `zip`, `unzip`.
- Service inspection & systemd: `ps`, `ss`, `systemctl`, `journalctl`.

## Most important / most-often-used Linux commands (copyable table)

| Command | What it does (one line) | Common usage / example |
|---|---:|---|
| `ls` | List files & directories | `ls -lah` — human-readable long listing including hidden files |
| `cd` | Change directory | `cd /var/log` |
| `pwd` | Print working directory | `pwd` |
| `cat` | Concatenate & display file | `cat file.txt` |
| `less` | Paginate through a file (back/forward) | `less /var/log/syslog` |
| `head` | Show first lines of a file | `head -n 20 file.txt` |
| `tail` | Show last lines of a file (follow) | `tail -f /var/log/nginx/access.log` |
| `grep` | Search text with patterns | `grep -i "error" logfile | less` |
| `find` | Find files by name/attributes | `find . -type f -name "*.conf"` |
| `file` | Tell file type | `file secret.bin` |
| `stat` | Show file metadata | `stat myscript.sh` |
| `chmod` | Change permissions | `ch

