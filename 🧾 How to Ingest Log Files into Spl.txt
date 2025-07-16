🧾 How to Ingest Log Files into Splunk
✅ Step 1: Start Splunk
If Splunk is installed locally:

Run it from terminal or services

Open in browser: http://localhost:8000

Login with your credentials (default: admin / changeme)

✅ Step 2: Prepare Your Log File
Example logs (auth.log):Jul 16 12:10:01 kali sshd[1999]: Failed password for invalid user root from 192.168.1.101 port 55224 ssh2
Jul 16 12:10:10 kali sshd[2001]: Accepted password for admin from 192.168.1.105 port 55226 ssh2
Jul 16 12:11:00 kali nmap[2002]: Nmap scan report for 192.168.1.105
Jul 16 12:11:05 kali nikto[2003]: Nikto scan from 192.168.1.107 against http://192.168.1.10
Jul 16 12:10:01 kali sshd[1999]: Failed password for invalid user root from 192.168.1.101 port 55224 ssh2
Jul 16 12:10:03 kali sshd[1999]: Failed password for invalid user root from 192.168.1.101 port 55224 ssh2
Jul 16 12:10:05 kali sshd[1999]: Failed password for invalid user root from 192.168.1.101 port 55224 ssh2
Jul 16 12:10:10 kali sshd[2001]: Accepted password for admin from 192.168.1.105 port 55226 ssh2
Jul 16 12:11:00 kali nmap[2002]: Nmap scan report for 192.168.1.105
Jul 16 12:11:05 kali nikto[2003]: Nikto scan from 192.168.1.107 against http://192.168.1.10


✅ Step 3: Add Data in Splunk Web Interface
Go to:
Settings → Add Data

Choose "Upload" → Click Select File

Upload auth.log, syslog, or any other file

Click Next

Source type: Choose or create:

If SSH logs: linux_secure

If Web logs: access_combined or custom

Index: Select or create an index (e.g., attack_logs)

You can create one by going to:
Settings > Indexes > New Index

Review → Submit

✅ Your logs are now ingested and can be searched via Splunk.

✅ Step 4: Verify Ingestion
Run this SPL in Splunk’s Search:
  
another step

  index=attack_logs
If you see your log events listed, you’re good to go.

🔁 Alternative: Monitor Folder (for real-time logs)
Go to Settings > Add Data > Monitor

Choose "Files & Directories"

Enter folder path:

Windows: C:\Users\YourName\Documents\Logs

Linux: /var/log/ or custom directory

Follow same steps:

Set Source Type

Choose or create Index

Click Finish

📌 Splunk will now monitor and index any new logs in that folder automatically.

🔄 2. Upload Logs to Splunk
Go to Splunk Web:

URL: http://localhost:8000

Login and go to:
Settings > Add Data > Upload File

Upload auth.log

Set source type: linux_secure or custom

Set index: attack_logs

🔎 3. SPL Code for Log Analysis
3.1 Brute-force Detection (SSH Failed Passwords)

index=attack_logs sourcetype=linux_secure "Failed password"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip
| where count > 2
3.2 Successful Login Tracking

index=attack_logs sourcetype=linux_secure "Accepted password"
| rex "for (?<user>\w+) from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by user, src_ip
3.3 Nmap Port Scan Detection


index=attack_logs sourcetype=syslog "Nmap scan report"
| rex "for (?<target_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by target_ip
3.4 Nikto Web Scan Detection


index=attack_logs sourcetype=syslog "Nikto scan"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip


📊 4. Dashboard JSON (Auto-Create via Code in Splunk Dashboard Editor)
Paste this inside Dashboards > New Dashboard > Source:

<dashboard>
  <label>Attack Analysis Dashboard</label>
  <row>
    <panel>
      <title>Brute Force Attempts (SSH)</title>
      <chart>
        <search>
          <query>index=attack_logs sourcetype=linux_secure "Failed password" | rex "from (?&lt;src_ip&gt;\d+\.\d+\.\d+\.\d+)" | stats count by src_ip | where count &gt; 2</query>
        </search>
        <option name="charting.chart">bar</option>
      </chart>
    </panel>
    <panel>
      <title>Login Success Tracker</title>
      <chart>
        <search>
          <query>index=attack_logs sourcetype=linux_secure "Accepted password" | rex "from (?&lt;src_ip&gt;\d+\.\d+\.\d+\.\d+)" | stats count by src_ip</query>
        </search>
        <option name="charting.chart">pie</option>
      </chart>
    </panel>
  </row>
</dashboard>



