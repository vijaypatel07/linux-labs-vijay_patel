# Lab: Linux Standard Streams & I/O Redirection

## 📌 Objective
To master data stream manipulation by intercepting and redirecting Standard Input (`stdin`/`0`), Standard Output (`stdout`/`1`), and Standard Error (`stderr`/`2`) using operators and command pipelines.

---

## 🛠️ Concepts & Commands Executed

### 1. Output Redirection (`>` vs `>>`)
* **Overwriting Stream (`>`):** Diverts standard output to a file, replacing its contents entirely.
  ```bash
  echo "System baseline set" > tracking.log
  ```
* **Appending Stream (`>>`):** appends data to the end of a file without destroying existing records.
  ```bash
  echo "Security patch update deployed" >> tracking.log
  ```

### 2. Stream Separation & Error Handling
* **Isolating Errors (`2>`):** Redirects errors to a unique file while allowing valid output to display normally on the terminal.
  ```bash
  ls /root 2> access_denied.log
  ```
* **Merging Streams (`2>&1` or `&>`):** Directs both standard output and error output into a singular log file for consolidated analysis.
  ```bash
  tar -cvf backup.tar /var/log > backup_activity.log 2>&1
  ```

### 3. Piping Pipelines (`|`)
* Sends the standard output of the first process directly into the standard input of the next process to filter runtime information:
  ```bash
  ps aux | grep "systemd"
  ```

---

## 🔍 System Verification Output
I ran a test to search through the system history file while safely dumping any command errors into a standalone log file:

```bash
\$ history | grep "systemctl" 2> pipelines_errors.log
  102  sudo systemctl start nginx
  105  sudo systemctl status firewall
  110  history | grep "systemctl"
```
*Observation: The pipeline filtered my system service actions efficiently without generating error logs, proving successful execution of the stream pipeline framework.*
