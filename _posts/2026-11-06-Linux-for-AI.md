---
layout: post
title: "Linux for AI: Essential Commands Every Developer Should Know"
date: 2026-11-06
---

# Linux for AI Engineers: Essential Commands Every Developer Should Know

When developing AI applications, we often work locally on macOS or Windows. However, most AI workloads—model training, inference servers, data processing, and GPU workloads—commonly run on Linux servers.

When connecting to a remote GPU machine through SSH, the terminal is often your primary interface. Knowing a few essential Linux commands makes it much easier to navigate files, manage processes, monitor resources, and troubleshoot problems.

This blog covers the Linux commands I found most useful while learning Linux for AI engineering.

---

## 1. Understanding the Linux File System

Linux organises everything under a single root directory `/`.

Some important directories:

| Directory | Purpose |
|---|---|
| `/` | Root directory; starting point of the file system |
| `/home` | Home directories of users |
| `/tmp` | Temporary files |
| `/etc` | System configuration files |
| `/usr` | System programs and libraries |
| `/var/log` | System and application logs |
| `/mnt` | Common mount point for external drives and storage |

Your home directory is represented by `~`. This is generally where you keep your projects, repositories, datasets, and scripts.

## 2. Navigating Directories

These commands help you understand where you are and move around the file system.

```bash
pwd
```
Prints the current working directory.

```bash
ls
```
Lists files and directories in the current location.

```bash
ls -la
```
Lists all files, including hidden files, with detailed information such as permissions, owner, size, and modification date.

```bash
cd /path/to/directory
```
Moves into a particular directory.

```bash
cd ~
```
Moves to your home directory.

```bash
cd ..
```
Moves one directory level up.

**Example:**

```bash
cd ~
mkdir my-ai-project
cd my-ai-project
pwd
```

This creates a project directory inside your home directory and navigates into it.

## 3. Creating, Copying, Moving and Deleting Files

| Command | Purpose |
|---|---|
| `mkdir project` | Creates a directory |
| `mkdir -p a/b/c` | Creates nested directories |
| `touch file.txt` | Creates an empty file if it doesn't exist |
| `cp file.txt backup.txt` | Copies a file |
| `cp -r src/ backup/` | Copies a directory recursively |
| `mv old.txt new.txt` | Renames a file |
| `mv file.txt /tmp/` | Moves a file |
| `rm file.txt` | Deletes a file |
| `rm -r directory/` | Deletes a directory and its contents |

**Important:** Be careful with `rm -rf`. It deletes files and directories recursively without asking for confirmation. There is generally no recycle bin or easy undo.

## 4. Reading Files and Monitoring Logs

When training a machine learning model or running an API, logs help you understand what is happening.

```bash
cat config.yaml
```
Prints the entire file.

```bash
head -20 training.log
```
Displays the first 20 lines.

```bash
tail -20 training.log
```
Displays the last 20 lines.

```bash
less training.log
```
Opens a file in a scrollable viewer. Press `q` to exit.

```bash
tail -f training.log
```
Continuously displays new lines as they are added to the log. Press `Ctrl + C` to stop following.

**Practical example:** When training a model on a remote GPU machine, you can use `tail -f` to monitor training progress without repeatedly opening the log file.

## 5. Searching Files and Text

Searching becomes especially useful when working with large codebases, configuration files, and training logs.

### grep — Search for text

```bash
grep "error" training.log
```
Finds lines containing the word `error`.

```bash
grep -i "cuda" config.yaml
```
Searches for `cuda`, ignoring uppercase and lowercase differences.

```bash
grep -r "learning_rate" .
```
Searches recursively through files in the current directory.

### find — Search for files

```bash
find . -name "*.py"
```
Finds all Python files under the current directory.

```bash
find . -name "*.ckpt" -size +1G
```
Finds checkpoint files larger than 1 GB.

**When to use which?**

- `grep` searches inside file contents.
- `find` searches for files and directories based on their names, types, sizes, and other properties.

## 6. Understanding File Permissions

Linux uses permissions to control who can read, write, and execute files.

Run:

```bash
ls -l train.py
```

Example output:

```text
-rwxr-xr-- 1 user group 2048 Mar 19 10:00 train.py
```

The permission characters represent:

- `r` — Read
- `w` — Write
- `x` — Execute

Permissions are defined for three groups:

1. Owner
2. Group
3. Others

### chmod — Change permissions

```bash
chmod +x train.sh
```
Makes a script executable.

```bash
chmod 755 deploy.sh
```
Gives the owner read, write, and execute permissions. Group and others get read and execute permissions.

```bash
chmod 644 config.yaml
```
Gives the owner read and write permissions. Group and others get read-only access.

### chown — Change ownership

```bash
sudo chown user:group file.txt
```
Changes the owner and group of a file. Usually requires administrator privileges.

These commands are useful when you encounter errors such as `Permission denied` while executing scripts or accessing files.

## 7. Installing Software on Ubuntu Using apt

Ubuntu uses `apt` to install and manage system-level packages.

```bash
sudo apt update
```
Refreshes the package lists.

```bash
sudo apt install -y htop
```
Installs `htop`. The `-y` flag automatically confirms installation prompts.

```bash
sudo apt install -y git curl wget tmux unzip
```
Installs several commonly used tools.

```bash
apt list --installed
```
Lists installed packages.

```bash
sudo apt remove htop
```
Removes a package.

**Important distinction:**

- `apt` installs system-level software.
- `pip` installs Python packages into a Python environment.

For example, `apt` can install system libraries and tools, while `pip install pandas` installs the Python pandas library.

## 8. Users and Administrator Access

```bash
whoami
```
Displays the currently logged-in user.

```bash
sudo command
```
Runs a command with administrator privileges.

Example:

```bash
sudo apt install htop
```

Use `sudo` only when necessary. Running every command as root is not recommended.

## 9. Monitoring Processes and Managing Services

When a model training job hangs, an API stops responding, or a machine becomes slow, process-monitoring commands become useful.

### Process monitoring

```bash
htop
```
Opens an interactive process monitor showing CPU, memory, and running processes. Press `q` to quit.

```bash
ps aux
```
Lists currently running processes.

```bash
ps aux | grep python
```
Searches for running Python processes.

```bash
kill 12345
```
Requests a process to terminate gracefully using its process ID (PID).

```bash
kill -9 12345
```
Forcefully terminates a process. Use this only if normal termination does not work.

### GPU monitoring

```bash
nvidia-smi
```
Displays NVIDIA GPU information, including memory usage, GPU utilisation, and processes using the GPU.

This is particularly useful when training deep learning models.

### Managing services with systemctl

Linux servers often run background services such as web servers and inference APIs.

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl status nginx
sudo systemctl enable nginx
```

These commands start, stop, restart, inspect, and configure a service to start automatically at boot.

## 10. Checking Disk Space

AI projects can consume significant storage because of datasets, model checkpoints, Docker images, and cached models.

### df — Check available disk space

```bash
df -h
```

Displays disk usage for mounted file systems in a human-readable format.

```bash
df -h /home
```

Checks disk usage for the file system containing `/home`.

### du — Check directory size

```bash
du -sh *
```
Displays the size of each item in the current directory.

```bash
du -sh ~/.cache
```
Checks the size of the cache directory.

```bash
du -sh /data/checkpoints/
```
Checks the total size of a checkpoints directory.

To find large directories:

```bash
du -h --max-depth=1 . | sort -hr | head -20
```

This lists the 20 largest entries directly under the current directory, ordered by size.

**Useful distinction:**

- `df` tells you how much disk space is available on a file system.
- `du` tells you how much space files or directories are consuming.

Be careful when deleting caches or checkpoints. Confirm that the files are no longer needed first.

## 11. Networking and File Transfers

When working on remote AI servers, you will often need to download models, access APIs, and transfer datasets or results.

### Downloading files

```bash
wget https://example.com/model.bin
```
Downloads a file using `wget`.

```bash
curl -O https://example.com/data.tar.gz
```
Downloads a file using `curl`, retaining the remote filename.

### Calling APIs

```bash
curl -s https://api.example.com/health
```
Makes an HTTP request and displays the response.

To format a JSON response:

```bash
curl -s https://api.example.com/health | python3 -m json.tool
```

### scp — Secure file transfer

Copy a file from your local machine to a remote server:

```bash
scp model.bin user@remote:/data/
```

Copy a file from the remote server to your local machine:

```bash
scp user@remote:/data/results.csv .
```

Copy an entire directory:

```bash
scp -r user@remote:/data/checkpoints/ ./local-dir/
```

### rsync — Synchronise files and directories

```bash
rsync -avz --progress ./data/ user@remote:/data/
```

Transfers and synchronises data with a remote machine.

Common flags:

- `-a` — Archive mode; preserves important file attributes.
- `-v` — Verbose output.
- `-z` — Compresses data during transfer.
- `--progress` — Displays transfer progress.

**scp vs rsync**

- `scp` is useful for straightforward file copying.
- `rsync` is useful for repeated transfers and directory synchronisation. It can avoid retransferring unchanged data and can resume interrupted transfers when used with appropriate options.

For large datasets and repeated transfers, `rsync` is often the more practical choice.

## 12. Using tmux for Long-Running AI Jobs

One of the most useful tools when working with remote GPU servers is `tmux`.

Imagine starting a model training job over SSH. If your SSH connection disconnects, the process may terminate if it is tied to that terminal session.

`tmux` allows you to keep a terminal session running independently of your SSH connection.

### Essential commands

```bash
tmux new -s train
```
Creates a new session named `train`.

Start your training script inside it:

```bash
python train.py
```

Detach from the session:

```text
Ctrl + B, then D
```

Your training process continues running.

List existing sessions:

```bash
tmux ls
```

Reconnect to the session:

```bash
tmux attach -t train
```

### Splitting terminal panes

Inside tmux:

- `Ctrl + B`, then `%` — Split vertically.
- `Ctrl + B`, then `"` — Split horizontally.
- `Ctrl + B`, then arrow keys — Switch between panes.

**Practical use case:** Run model training in one pane and monitor GPU usage or training logs in another.

Remember: `tmux` protects a process from ordinary terminal disconnection, but it does not protect against a server shutdown, reboot, or process crash.

## 13. Understanding Linux Pipes (`|`)

A pipe passes the output of one command as the input to another command.

For example:

```bash
tail -f train.log | grep "loss" | awk '{print $8}'
```

This command processes a continuously updating training log.

1. `tail -f train.log` — Continuously reads new log entries.
2. `grep "loss"` — Keeps only lines containing `loss`.
3. `awk '{print $8}'` — Extracts the eighth whitespace-separated field from each matching line.

The pipe operator `|` connects these commands, allowing you to process data without creating temporary files.

**In simple terms:** Read the training log, filter the loss-related lines, and extract the required column.

Note: The eighth field must actually contain the loss value in your log format. Always check your log structure before using a fixed column number.

---

## 14. macOS vs Linux: Important Differences

Since many developers use macOS locally and Linux on remote servers, it is useful to understand some common differences.

| Task | macOS | Ubuntu / Linux |
|---|---|---|
| Install packages | `brew install` | `sudo apt install` |
| Open a file | `open file.txt` | `xdg-open file.txt` (if GUI available) |
| Default shell config | `~/.zshrc` | Often `~/.bashrc` |
| Homebrew binaries | `/opt/homebrew/` (Apple Silicon) | Commonly `/usr/bin/` or `/usr/local/bin/` |
| Edit files in place with sed | `sed -i '' 's/a/b/' file` | `sed -i 's/a/b/' file` |
| File-system case sensitivity | Depends on volume; often case-insensitive by default | Usually case-sensitive |

The core commands such as `pwd`, `ls`, `cd`, `cp`, `mv`, `grep`, `find`, `df`, and `du` are available on both systems, although some command options differ.

---

## 15. My Essential Linux Command Cheat Sheet

| Category | Commands to remember |
|---|---|
| Navigation | `pwd`, `ls`, `cd` |
| File operations | `mkdir`, `touch`, `cp`, `mv`, `rm` |
| Reading files | `cat`, `head`, `tail`, `less` |
| Searching | `grep`, `find` |
| Permissions | `chmod`, `chown`, `sudo` |
| Package management | `apt update`, `apt install`, `apt remove` |
| Processes | `htop`, `ps`, `kill` |
| GPU monitoring | `nvidia-smi` |
| Services | `systemctl start`, `stop`, `restart`, `status` |
| Disk | `df -h`, `du -sh` |
| Networking | `curl`, `wget`, `scp`, `rsync` |
| Sessions | `tmux new`, `tmux ls`, `tmux attach` |
| Command chaining | `|` |

## Conclusion

Learning Linux does not mean memorising hundreds of commands. It means understanding the basic tools well enough to navigate a server, manage files, monitor processes, troubleshoot errors, and keep long-running workloads running.

For AI engineers, these skills become particularly important when working with remote GPU machines, model training pipelines, datasets, inference APIs, and production deployments.

I plan to keep practising these commands while building and deploying AI applications, because being comfortable with the terminal is an important part of becoming a capable AI engineer.

---

**Learning reference:** [Linux for AI — AI Engineering from Scratch](https://aiengineeringfromscratch.com/lesson?path=phases/00-setup-and-tooling/11-linux-for-ai)

