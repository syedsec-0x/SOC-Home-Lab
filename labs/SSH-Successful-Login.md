# Lab 01 - SSH Successful Login Detection using Splunk

## Objective

The objective of this lab is to simulate a successful SSH login from Kali Linux to a Metasploitable 3 Linux server, collect the authentication logs using rsyslog, ingest them into Splunk, create a detection rule, and generate a real-time alert.

---

## Lab Environment

| Machine | Role |
|---------|------|
| Kali Linux | Attacker |
| Metasploitable 3 Ubuntu | Target Linux Server |
| Windows Host | Splunk Enterprise |
| rsyslog | Log Forwarding Service |

---

## Network Configuration

- Adapter 1 : NAT
- Adapter 2 : Host-Only Network
- Kali Linux and Metasploitable 3 were connected over the Host-Only network.
- Splunk Enterprise was running on the Windows host.

---

# Attack Workflow

## Step 1 - Start the Lab

Started the following virtual machines:

- Kali Linux
- Metasploitable 3

<img width="640" height="480" alt="meta3 1" src="https://github.com/user-attachments/assets/7ff0eca1-7ae8-4b2b-b82f-0e2ce1a3d952" />

kali Linux 

<img width="964" height="922" alt="kali connectivity 1" src="https://github.com/user-attachments/assets/077227bb-1357-4327-9caa-8f6bc330f1c3" />


---

## Step 2 - Verify Network Connectivity

Verified that Kali Linux could communicate with the target machine.

Example command:

```bash
ping <Meta3-IP>
```

Connectivity was confirmed before performing any attack.

---

## Step 3 - Service Enumeration

Performed service enumeration using Nmap to identify exposed services.

Command:

```bash
nmap -sV <Meta3-IP>
```

Result:

```
22/tcp open ssh
```

SSH service was confirmed to be running.
<img width="919" height="446" alt="33" src="https://github.com/user-attachments/assets/9cc4208c-9b29-4158-b95e-26b06e76bdc5" />


---

## Step 4 - Attack Simulation

Connected to the Linux server using SSH.

Command:

```bash
ssh vagrant@<Meta3-IP>
```

Authenticated successfully using valid credentials.

Exited the SSH session.

```bash
exit
```
<img width="919" height="483" alt="2" src="https://github.com/user-attachments/assets/aad6623e-35c9-46c8-ae78-d5d9e9937b3a" />


---

## Step 5 - Log Verification on Target

Verified that the authentication event was recorded on the target system.

Example command:

```bash
tail -f /var/log/auth.log
```

Observed log:

```
Accepted password for vagrant from <Kali-IP> port XXXXX ssh2
```
<img width="640" height="480" alt="meta 3 accepted password logs" src="https://github.com/user-attachments/assets/b78349a8-89e9-4e08-88d2-cf63ea9cb250" />

---

## Step 6 - Log Collection

Confirmed that rsyslog successfully forwarded the authentication log to Splunk Enterprise.

Verified the event inside Splunk Search.

Example Event:

```
Accepted password for vagrant from 172.xx.xx.xx
```
<img width="1586" height="796" alt="Screenshot 2026-07-14 151308" src="https://github.com/user-attachments/assets/a894557d-6ea0-4a3a-a95e-19ed5adde8bb" />

---

## Step 7 - Detection Rule

SPL Query

```spl
index=main sourcetype=syslog "Accepted password"
```

Purpose:

Detect successful SSH authentication events.

---

## Step 8 - Alert Creation

Created a Real-Time Splunk Alert.

Alert Name:

```
SSH Successful Login
```
<img width="1904" height="844" alt="Screenshot 2026-07-14 151243" src="https://github.com/user-attachments/assets/8dd0040f-8df0-4559-8402-8b3aac4bd4f7" />


Configuration:

- Alert Type: Real-Time
- Trigger: Per Result
- Action: Add to Triggered Alerts

---

## Step 9 - Alert Validation

Repeated the SSH login from Kali Linux.

Verified that:

- Authentication log was generated.
- Log reached Splunk.
- Detection rule matched the event.
- Splunk generated the alert successfully.
<img width="1913" height="916" alt="Screenshot 2026-07-14 153029" src="https://github.com/user-attachments/assets/17e9b4e1-6786-4a08-a2e0-e800a7f07491" />
<img width="1918" height="383" alt="Screenshot 2026-07-14 153144" src="https://github.com/user-attachments/assets/440ea894-61a1-4557-bd5b-bb36911aadaf" />

---

# MITRE ATT&CK Mapping

Technique

```
T1078
```

Technique Name

```
Valid Accounts
```

Reason

The attacker authenticated using legitimate credentials over SSH.

---

# Detection Logic

```
SSH Login

↓

Linux Authentication Log

↓

rsyslog

↓

Splunk

↓

Detection Rule

↓

Real-Time Alert
```

---

# Skills Demonstrated

- Linux Administration
- SSH Authentication
- Network Enumeration
- Nmap
- Linux Log Analysis
- rsyslog Configuration
- Splunk SIEM
- SPL Queries
- Alert Engineering
- SOC Investigation
- MITRE ATT&CK Mapping

---

# Lessons Learned

- Always verify connectivity before beginning an assessment.
- Enumerate exposed services before attempting access.
- Successful SSH logins generate authentication events in auth.log.
- rsyslog can forward Linux authentication logs into Splunk.
- Splunk can be used to detect and alert on successful SSH authentication events.
- Validated the complete detection pipeline from attack generation to SIEM alerting.
