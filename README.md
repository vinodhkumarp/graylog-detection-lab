# Detection Engineering Lab with Graylog SIEM

## 📌 Project Overview
This project demonstrates **detection engineering** using a home lab SIEM (Graylog).  
I set up a Graylog instance, forwarded logs from an Ubuntu VM, simulated suspicious activity, and built detections for common attack techniques.

---

## 🛠️ Lab Setup
- **Platform:** Ubuntu 24.04 VM (Apple Silicon host)
- **SIEM:** Graylog Community Edition
- **Log Forwarding:** rsyslog
- **Data Sources:** `/var/log/auth.log`, `/var/log/syslog`

### Configuration
- rsyslog forwarding (`/etc/rsyslog.d/90-graylog.conf`):
  ```conf
  *.* @<Graylog-IP>:1514;RSYSLOG_SyslogProtocol23Format

### 🔒 Auditd Integration
- Configured Auditd to monitor:
    - User account changes (`/etc/passwd`)
    - Privilege escalation (`su`, `sudo`)
    - Authentication failures (`auth.log`)
- Forwarded Auditd logs to Graylog via rsyslog
- Verified detections:
    - New user creation → `user_changes`
    - Privilege escalation → `priv_escalation`
    - Failed logins → `auth_failures`

## 🧩 Detections as Code

In this project, I treat detections as **code artifacts**, not just UI clicks.  
Each detection rule created in Graylog is exported as a JSON file and stored in the `detections/` folder of this repository.

### Why Detections as Code?
- **Reusability** → Anyone can import these JSON files into their own Graylog instance.
- **Version Control** → Changes to detection logic are tracked in Git history.
- **Collaboration** → Other analysts/engineers can review, suggest improvements, or extend the rules.
- **Professional Practice** → Mirrors how modern SOCs and Detection Engineering teams manage rules (similar to Infrastructure as Code).

### Available Detection Rules
- `brute_force.json` → Detects >5 failed SSH logins in 1 minute
- `new_user.json` → Detects new user creation (`useradd`/`adduser`)
- `priv_escalation.json` → Detects privilege escalation attempts (`sudo`/`su`)

This approach demonstrates **Detection-as-Code (DaC)**, a growing best practice in cybersecurity engineering.

### Note on Event Exports
Graylog Community Edition does not support direct Event Definition export.  
The JSON files in `detections/` are manually documented representations of the detection logic.  
They serve as reusable templates for detection-as-code, even if not directly importable.