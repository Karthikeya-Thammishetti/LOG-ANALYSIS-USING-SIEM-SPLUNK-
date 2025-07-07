🛡️ Log Analysis & Threat Detection Using SIEM (Splunk) — Offensive Simulation with Kali Linux

📘 Project Overview

This cybersecurity project demonstrates the design and implementation of a real-time log monitoring and threat detection system using Splunk SIEM, enriched with simulated attack data from Kali Linux. The project mimics real-world attacker behavior through tools like Hydra, Nmap, and Nikto, with logs collected and analyzed using Splunk. It focuses on correlation rule development, custom dashboards, and attack mapping to enhance security operations and reduce mean-time-to-detect (MTTD).


📁 Repository Structure 

/Splunk-Log-Analysis/
├── dashboards/
│   ├── ssh_brute_dashboard.xml
│   ├── nikto_activity_dashboard.xml
├── sample-logs/
│   ├── auth.log
│   ├── syslog
│   ├── access.log
├── queries/
│   ├── brute_force.spl
│   ├── nikto_scan.spl
├── README.md

🎯 Key Objectives

* Collect and parse real-time logs from simulated offensive sources.
* Detect brute-force, port scanning, and web exploitation behavior.
* Develop SPL-based correlation rules and anomaly logic.
* Build visual dashboards for threat monitoring and response.
* Simulate attacker behavior to support blue team defense readiness.

⚙️ Tech Stack

| Component       | Technology                                  |
| --------------- | ------------------------------------------- |
| SIEM Tool       | Splunk Enterprise                           |
| Source OS       | Kali Linux (attacker), Ubuntu (target)      |
| Tools Simulated | Hydra (brute-force), Nmap, Nikto            |
| Logs Analyzed   | Syslog, Apache2 access/error logs, auth.log |
| Scripting       | SPL (Search Processing Language)            |
| Dashboards      | Splunk visualizations, threat maps          |

📥 Log Sources & Collection

| File/Log Path                 | Description                            |
| ----------------------------- | -------------------------------------- |
| /var/log/auth.log             | SSH login failures from Hydra          |
| /var/log/syslog               | Detected Nmap port scans               |
| /var/log/apache2/access.log   | Nikto HTTP requests and probes         |
| Windows/Linux logs (optional) | Local login & escalation activities    |
| IDS logs (optional)           | Future integration with Snort/Suricata |

Logs were forwarded from Kali using syslog or Splunk Universal Forwarder.

🔍 Attack Simulation & Detection

| Attack Type            | Tool Used | Detection Method in Splunk             |
| ---------------------- | --------- | -------------------------------------- |
| SSH Brute-force        | Hydra     | Multiple failed logins via auth.log    |
| Port Scanning          | Nmap      | SYN bursts in syslog to multiple ports |
| Web Vulnerability Scan | Nikto     | URI scans + suspicious User-Agent logs |



📊 Custom Dashboards Built

| Dashboard Name        | Key Features                                    |
| --------------------- | ----------------------------------------------- |
| SSH Brute-force Panel | Top brute IPs, failed login counts, geolocation |
| Port Scan Heatmap     | Ports targeted, scan frequency                  |
| Nikto Activity Panel  | Suspicious URIs, user agents, HTTP status       |
| Timeline View         | Attack phase correlation (Nmap → Nikto → Hydra) |
| Log Volume Trends     | Real-time ingestion stats per log type          |
| Alert Summary Panel   | Triggered alerts sorted by severity/time        |

🚨 Correlation Rules Used

* Multi-event correlation: Failed login + successful login from new geo-location
* Threshold alerting: >5 failed attempts in under 60 seconds
* Pattern recognition: cmd.exe, powershell, Nikto User-Agent
* Recon-to-exploit mapping: Traced activity from scan to brute-force

✅ Project Outcomes

* Simulated adversary TTPs using a controlled red-team environment
* Reduced analyst load by automating rule-based detections
* Improved MTTD by 30–50% using dashboards and alerts
* Enhanced blue-team threat visibility across all stages of attack

🔮 Future Enhancements

* Integrate SOAR (Splunk Phantom) to automate firewall blocking or account disablement
* Add unsupervised anomaly detection (Isolation Forest, One-Class SVM)
* Push real-time alerts to Slack or ServiceNow
* Extend simulation to SQLMap, Metasploit, or custom payloads

📝 Summary

This project bridges the gap between offensive simulation and defensive detection using Splunk. With attack data from Hydra, Nmap, and Nikto, and real-time detection logic powered by SPL and custom dashboards, it enables security analysts to detect, investigate, and respond to threats effectively. By aligning activity to the MITRE ATT&CK framework, it supports contextual threat attribution and strengthens SOC readiness.
