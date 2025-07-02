# 🔐 SSH Failed Login Detector

**Author:** Shady Emad  
**Date:** 2/7/2024  
**Purpose:** Detect and report SSH failed login attempts from system logs.

---

## 📄 Description

This Bash script analyzes the `/var/log/secure` file (used in RHEL/CentOS/Fedora) to detect failed SSH login attempts. It generates a daily report showing:

- Date & time of each attempt  
- Username used  
- Source IP address  
- Summary of top offending IPs  

It also allows optional cleanup of old reports based on user input.

---

## 📁 Output

A file will be generated in the same directory with the following name format:

```
failed_ssh_report_YYYY-MM-DD.txt
```

### 🧾 Example Report

```
SSH Failed Login Attempts
Generated on: Tue Jul 2 10:22:11 2024
----------------------------------------
Jul 2 10:22:11 | User: shady | IP: 192.168.1.50
Jul 2 10:25:08 | User: root  | IP: 10.0.0.5

Top IPs by number of failed attempts:
----------------------------------------
5 192.168.1.50
2 10.0.0.5
```

---

## 🚀 Usage

```bash
sudo ./ssh_failed_login_detector.sh
```

> ❗ Must be run as root to read `/var/log/secure`.

---

## 🧹 Cleanup Feature

After generating the report, the script asks:

```
Do you want to delete reports older than a certain number of days? (y/n)
```

If you choose **yes**, you'll be prompted for the number of days (e.g., 7). Old reports matching the pattern `failed_ssh_report_*.txt` and older than that number of days will be deleted.

---

## 🔍 How It Works

- **Log filtering:** Uses `grep` to find lines with `Failed password`.
- **Data extraction:**  
  - **Date:** Extracted with `awk '{print $1, $2, $3}'`  
  - **Username:** Using `grep -oP 'for (invalid user )?\K\S+'`  
  - **IP:** Extracted using `grep -oE 'from ([0-9]+\.[0-9]+\.[0-9]+\.[0-9]+)' | awk '{print $2}'`  
- **Top IP summary:** Sorts and counts IPs with most failed attempts.

---

## ⚙️ Requirements

- Linux system with `/var/log/secure` (RHEL/CentOS/Fedora)  
- Bash shell  
- Tools: `grep`, `awk`, `sort`, `uniq`, `find`  
- Root privileges (`sudo`)

---

## 📌 Example Log Line

```
Jul  2 10:22:11 vm1 sshd[12345]: Failed password for invalid user shady from 192.168.1.50 port 54321 ssh2
```

Parsed as:
- **Date:** Jul 2 10:22:11  
- **User:** shady  
- **IP:** 192.168.1.50

---

## 🛡️ Disclaimer

This script is for educational and monitoring purposes only. Log file paths may vary on different distributions (e.g., `/var/log/auth.log` on Ubuntu/Debian).

---

## 📎 License

MIT License – use freely with attribution.

---

