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
