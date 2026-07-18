# Lab 02 – SSH Failed Login Detection using Splunk

## Objective

The objective of this lab is to detect failed SSH authentication attempts on a Linux server using Splunk Enterprise. Failed login attempts may indicate incorrect credentials, unauthorized access attempts, or the early stages of a brute-force attack.

---

## Lab Environment

| Machine | Operating System | Role |
|---------|------------------|------|
| Windows Host | Windows 11 | Splunk Enterprise |
| Kali Linux | Kali Linux | Attacker |
| Metasploitable 3 | Ubuntu | Target |

---

## Tools Used

- Splunk Enterprise
- Kali Linux
- Metasploitable 3
- VirtualBox
- rsyslog
- OpenSSH
- Nmap

---

## MITRE ATT&CK

| Tactic | Technique | ID |
|---------|-----------|----|
| Credential Access | Brute Force | T1110 |

---

## Attack Scenario

An attacker attempts to authenticate to the SSH service using an incorrect password.

The Linux authentication daemon records the failed login attempt in `/var/log/auth.log`.

The log is forwarded to Splunk using rsyslog.

---

## Lab Workflow

Kali Linux

↓

SSH Login (Wrong Password)

↓

Metasploitable 3

↓

auth.log

↓

rsyslog

↓

Splunk

↓

Detection Rule

↓

Real-Time Alert

---

## Step 1 – Verify SSH Service

```bash
nmap -sV 172.28.128.3
```

Screenshot:
`01_nmap_scan.png`<img width="976" height="442" alt="11" src="https://github.com/user-attachments/assets/1b11bae8-f536-47ae-a2a2-0a3b50b2f772" />


---

## Step 2 – Perform Failed Login

```bash
ssh vagrant@172.28.128.3
```

Enter an incorrect password three times.

Screenshot:
`02_failed_login.png`<img width="645" height="270" alt="ssh failed" src="https://github.com/user-attachments/assets/7a77cb65-5bcc-48b4-bd14-91c0a44ac993" />


---

## Step 3 – Verify Authentication Log

```bash
sudo tail -f /var/log/auth.log
```


Example:

```
Failed password for vagrant from 172.28.128.101 port 52564 ssh2
```

Screenshot:
`03_auth_log.png`
<img width="640" height="480" alt="VirtualBox_Metasploitable3-ub1404_16_07_2026_14_30_42" src="https://github.com/user-attachments/assets/50dcc795-fc2f-4c01-8185-0a5ef5abfb44" />


---



## Step 4 – Verify Log in Splunk

```spl
index=main sourcetype=syslog "Failed password"
```

Screenshot:
`04_splunk_event.png`
<img width="1268" height="518" alt="Screenshot 2026-07-16 143547" src="https://github.com/user-attachments/assets/78d4b90e-e8f2-4b35-91cf-755bcb78fac8" />

<img width="1354" height="629" alt="Screenshot 2026-07-16 144110" src="https://github.com/user-attachments/assets/29f2bc81-5a6f-4632-92c7-c0eec0b57f0b" />

---

## Detection Rule

```spl
index=main sourcetype=syslog "Failed password"
| rex "Failed password for (?<username>\S+)"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| table _time username src_ip host
```

### Detection Logic

- Detect failed SSH logins
- Extract username
- Extract attacker IP
- Display affected host

---

## Alert Configuration

| Setting | Value |
|---------|-------|
| Alert Type | Real-Time |
| Trigger | Per Result |
| Alert Name | SSH Failed Login Detection |

Screenshot:
`05_alert_configuration.png`
<img width="1339" height="590" alt="Screenshot 2026-07-16 145102" src="https://github.com/user-attachments/assets/4c058cd7-40e6-4891-ade6-7c0cade806ca" />

---

## Validation

Generate another failed login.

Verify the alert appears in Triggered Alerts.

Screenshot:
`06_triggered_alert.png`
<img width="1347" height="634" alt="Screenshot 2026-07-16 145343" src="https://github.com/user-attachments/assets/668bdaac-607d-4215-a0bd-7e14072628af" />

---

## Skills Demonstrated

- Linux Authentication Monitoring
- SSH Log Analysis
- SPL Query Development
- Splunk Alert Creation
- Security Event Investigation
- MITRE ATT&CK Mapping

---

## Outcome

The failed SSH login was successfully detected, forwarded to Splunk, and generated a real-time alert.
