**Linux project idea with full code and documentation** that is suitable for learning and showcasing practical Linux skills:

---

## 🔍 Project: **Linux Log Monitoring & Alert System**

### 📂 Repo Name:

`linux-log-monitoring-alert-system`

---

## 📘 Project Description:

A shell-script-based log monitoring tool for Linux systems that:

* Monitors system logs (`/var/log/syslog` or `/var/log/messages`)
* Detects specific keywords (like `error`, `fail`, `denied`)
* Sends alerts (email or terminal)
* Generates a simple HTML report

This is useful for DevOps monitoring, Linux automation, or system administration roles.

---

## 🛠️ Features:

* Monitors system logs in real time
* Configurable keyword alerts
* Sends alerts to email or screen
* Generates daily log summary reports

---

## 🖥️ Tech Stack:

* Bash Shell
* `cron` for automation
* `mail` (or `sendmail`) for email alerts
* HTML for reporting

---

## 📁 Project Structure:

```
linux-log-monitoring-alert-system/
│
├── monitor.sh             # Main monitoring script
├── config.sh              # Configuration file
├── report.sh              # Daily summary report generator
├── sample-log.txt         # Sample log file (for testing)
├── log_report.html        # Output report (generated)
└── README.md              # Documentation
```

---

## 🧾 Full Code:

### 1. `config.sh`

```bash
#!/bin/bash

# Path to log file
LOG_FILE="/var/log/syslog"

# Keywords to search
KEYWORDS=("error" "fail" "denied")

# Email for alerts
ALERT_EMAIL="admin@example.com"

# Alert method: "email" or "terminal"
ALERT_METHOD="terminal"
```

---

### 2. `monitor.sh`

```bash
#!/bin/bash
source config.sh

# Monitor logs and alert if keyword is found
tail -Fn0 "$LOG_FILE" | \
while read line; do
  for keyword in "${KEYWORDS[@]}"; do
    echo "$line" | grep -i "$keyword" &> /dev/null
    if [ $? = 0 ]; then
      message="ALERT: '$keyword' found in log: $line"
      if [ "$ALERT_METHOD" == "email" ]; then
        echo "$message" | mail -s "Log Alert: $keyword" "$ALERT_EMAIL"
      else
        echo "$message"
      fi
    fi
  done
done
```

---

### 3. `report.sh`

```bash
#!/bin/bash
source config.sh

echo "<html><body><h2>Daily Log Summary Report</h2><ul>" > log_report.html

for keyword in "${KEYWORDS[@]}"; do
  count=$(grep -i "$keyword" "$LOG_FILE" | wc -l)
  echo "<li>$keyword: $count entries</li>" >> log_report.html
done

echo "</ul></body></html>" >> log_report.html
```

---

### 4. `sample-log.txt` (for testing without real `/var/log/syslog`)

```txt
Jul  9 10:10:10 myhost sshd[123]: Failed password for invalid user root
Jul  9 10:10:11 myhost sudo: pam_unix(sudo:auth): authentication failure
Jul  9 10:10:15 myhost sshd[123]: Accepted password for user
```

---

## ⏲️ Cron Setup for Daily Reports

Add to crontab:

```bash
0 23 * * * /path/to/report.sh
```

---

## 📷 Screenshot (Sample Report Output)

```html
<html><body><h2>Daily Log Summary Report</h2><ul>
<li>error: 5 entries</li>
<li>fail: 3 entries</li>
<li>denied: 1 entries</li>
</ul></body></html>
```

---

## ✅ How to Run:

```bash
chmod +x *.sh
./monitor.sh   # For real-time monitoring
./report.sh    # To generate summary report
```

---

## 📌 To-Do:

* Add Slack integration for alerts
* Add timestamp filters
* Store logs in a database

---
