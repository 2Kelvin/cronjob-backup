# Backup Logs Cronjob

A system-wide cronjob to automate log backups every night at 11:59PM. This job triggers the `backup-tool` to compress and archive `/var/log` content.

## 🛠 Configuration Details

The cronjob is designed with the following specifications:
* **Schedule:** Every day at **11:59 PM**.
* **User:** Root.
* **Target:** `/var/log` directory.
* **Logic:** Executes the `backup-tool` binary located in `/usr/local/bin`.

## 🚀 Installation & Setup

### 1. Install the Backup Tool
First, ensure you have the core backup utility installed on your system:

```bash
git clone https://github.com/2Kelvin/backup-logs-tool.git
cd backup-logs-tool
# Follow the installation instructions in that repository from the README
```

### 2. Configure the Cronjob
System-wide cronjobs reside in `/etc/cron.d/`. Create a new configuration file there (remember: do **not** use a file extension like `.sh` or `.cron`):

```bash
sudo vim /etc/cron.d/backup-logs
```

### 3. Add the Schedule
Paste the following line into the file. 

```bash
# /etc/cron.d/backup-logs: run everyday at 11:59 PM
59 23 * * * root backup-tool /var/log
```

**Note:** You must include an empty line at the very end of the file. Without this trailing newline, cron may fail to process the job.

## ⚠️ Important Requirements

To ensure `cron` picks up and executes this file, you must adhere to these strict security and formatting rules. Failure to follow these will result in the job being silently ignored.

| Requirement | Reason |
| :--- | :--- |
| **No File Extensions** | `cron` ignores files with dots (e.g., `backup.sh`). Use a simple name like `backup-logs`. |
| **Permissions** | Must be **644** (`-rw-r--r--`). `cron` will skip the file if it is group-writable for security. |
| **Ownership** | The file **must** be owned by `root`. |
| **Trailing Newline** | The file must end with a **blank line** to be considered valid by the parser. |
