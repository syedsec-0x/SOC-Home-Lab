# FTP Detection using Splunk

## Overview

This lab demonstrates how to detect FTP authentication events using Splunk by monitoring ProFTPD logs forwarded from a Metasploitable 3 server. The detections include successful logins, failed login attempts, and brute-force attacks.

---

## Lab Environment

| Component | Details |
|----------|---------|
| SIEM | Splunk Enterprise |
| FTP Server | ProFTPD |
| Target Machine | Metasploitable 3 |
| Attacker Machine | Kali Linux |
| Log Forwarding | rsyslog (UDP 5140) |

---

# Detection 1 - FTP Successful Login

## Objective

Detect successful FTP authentication events.

### Attack Simulation

```bash
ftp 172.28.128.3
```

Login using valid credentials.

```
Username: vagrant
Password: vagrant
```

<img width="622" height="330" alt="ftp" src="https://github.com/user-attachments/assets/39671c1d-f33d-439d-b2f1-1089b7c0754d" />


---

### Sample Log

```
Jul 19 10:15:42 metasploitable3-ub1404 proftpd[1984]:
USER vagrant: Login successful
```

---

### Splunk Detection Query

```spl
index=main sourcetype=syslog "proftpd" "Login successful"
```
<img width="1365" height="714" alt="Screenshot 2026-07-19 154551" src="https://github.com/user-attachments/assets/bdd03ff5-ed31-4203-b746-7714b33586f7" />

---

### Alert Configuration

| Setting | Value |
|---------|-------|
| Alert Name | FTP Successful Login |
| Trigger | Number of Results > 0 |
| Severity | Medium |

---

### MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Valid Accounts | T1078 |

---

# Detection 2 - FTP Failed Login

## Objective

Detect failed FTP authentication attempts.

### Attack Simulation

```bash
ftp 172.28.128.3
```

Use an incorrect password.

```
Username: vagrant
Password: wrongpassword
```

---

### Sample Log

```
Jul 19 10:20:54 metasploitable3-ub1404 proftpd[2032]:
USER vagrant (Login failed): Incorrect password
```

---

### Splunk Detection Query

```spl
index=main sourcetype=syslog "proftpd" "Login failed"
```
<img width="1003" height="299" alt="Screenshot 2026-07-19 155701" src="https://github.com/user-attachments/assets/240c1d7d-7b01-41d0-983a-0126816a43d2" />

---

### Alert Configuration

| Setting | Value |
|---------|-------|
| Alert Name | FTP Failed Login |
| Trigger | Number of Results > 0 |
| Severity | Medium |

---

### MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Brute Force | T1110 |

---

# Detection 3 - FTP Brute Force

## Objective

Detect multiple failed FTP login attempts originating from the same target server.

### Attack Simulation

Create a password list.

```bash
cat > passwords.txt <<EOF
123456
password
admin
vagrant123
letmein
EOF
```
<img width="1365" height="642" alt="ftp bf" src="https://github.com/user-attachments/assets/ffb25cba-23d5-4734-90e7-7fd3ab470e36" />

Launch Hydra.

```bash
hydra -l vagrant -P passwords.txt ftp://172.28.128.3
```

---

### Sample Logs

```
Jul 19 10:24:38 metasploitable3-ub1404 proftpd[2040]:
USER vagrant (Login failed): Incorrect password
```

```
Jul 19 10:24:38 metasploitable3-ub1404 proftpd[2041]:
USER vagrant (Login failed): Incorrect password
```

```
Jul 19 10:24:38 metasploitable3-ub1404 proftpd[2042]:
USER vagrant (Login failed): Incorrect password
```
<img width="1354" height="632" alt="Screenshot 2026-07-19 161006" src="https://github.com/user-attachments/assets/459983c6-2a62-40e2-9ba4-c20839fd1189" />

---

### Splunk Detection Query

```spl
index=main sourcetype=syslog "proftpd" "Login failed"
| stats count by host
| where count >= 5
```

<img width="1359" height="638" alt="Screenshot 2026-07-19 161457" src="https://github.com/user-attachments/assets/4a410905-951b-436d-b997-e04f833259b8" />

<img width="1351" height="568" alt="Screenshot 2026-07-19 161347" src="https://github.com/user-attachments/assets/7e252318-09ea-4edf-9a9d-3570a76526b1" />

---

### Query Explanation

- Search the **main** Splunk index.
- Filter **syslog** events.
- Search only **ProFTPD failed login** events.
- Count the number of failed authentication attempts.
- Trigger if the number of failures is **5 or greater**.

---

<img width="1353" height="635" alt="Screenshot 2026-07-19 161612" src="https://github.com/user-attachments/assets/155fada5-6b85-4557-bccc-b609c53f0d6d" />


### Alert Configuration

| Setting | Value |
|---------|-------|
| Alert Name | FTP Brute Force Detection |
| Trigger | Number of Results > 0 |
| Severity | High |

---

### MITRE ATT&CK

| Technique | ID |
|-----------|----|
| Brute Force | T1110 |

---

# Investigation Steps

When an alert is generated:

- Identify the affected FTP server.
- Review the number of failed authentication attempts.
- Determine whether the activity originated from an expected source.
- Check if a successful login occurred immediately after repeated failures.
- Investigate the source system for brute-force activity.

---

# Detection Summary

| Detection | SPL Query | MITRE |
|-----------|-----------|--------|
| FTP Successful Login | `index=main sourcetype=syslog "proftpd" "Login successful"` | T1078 |
| FTP Failed Login | `index=main sourcetype=syslog "proftpd" "Login failed"` | T1110 |
| FTP Brute Force | `index=main sourcetype=syslog "proftpd" "Login failed" \| stats count by host \| where count >= 5` | T1110 |

---

## Skills Demonstrated

- Splunk Search Processing Language (SPL)
- Log Analysis
- ProFTPD Authentication Monitoring
- Brute Force Detection
- Security Alert Creation
- MITRE ATT&CK Mapping
- SOC Investigation Workflow
