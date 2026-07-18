# Lab 03 – SSH Brute Force Detection using Splunk

## Objective

The objective of this lab is to detect SSH brute-force attacks by identifying multiple failed authentication attempts 
originating from the same source IP address.

Unlike Lab 02, this lab focuses on detecting repeated authentication failures that indicate password guessing activity.

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
- Hydra
- rsyslog
- OpenSSH
- VirtualBox

---

## MITRE ATT&CK

| Tactic | Technique | ID |
|---------|-----------|----|
| Credential Access | Brute Force | T1110 |

---

## Attack Scenario

Hydra was used to perform an SSH password guessing attack against Metasploitable 3.

Each failed authentication generated a Linux authentication log which was forwarded to Splunk.

The detection identifies when a single IP address performs multiple failed SSH logins.

---

## Attack Command

```bash
hydra -l vagrant -P passwords.txt ssh://172.28.128.3
```

Screenshot:
`01_hydra_attack.png`
<img width="1834" height="497" alt="ssh brute force" src="https://github.com/user-attachments/assets/910cbb59-16b6-46ac-8bb5-b2ddff961bcc" />

---

## Verify Linux Logs

```bash
sudo tail -f /var/log/auth.log
```

Example:

```
Failed password for vagrant from 172.28.128.101 port 54120 ssh2
```

Screenshot:
`02_auth_log.png`
<img width="640" height="480" alt="VirtualBox_Metasploitable3-ub1404_16_07_2026_14_30_42" src="https://github.com/user-attachments/assets/39b94eff-6ad2-4f59-8f20-c982e9d0e6ab" />

---

## Verify Events in Splunk

```spl
index=main sourcetype=syslog "Failed password"
```


Screenshot:
`03_failed_events.png`
<img width="968" height="525" alt="Screenshot 2026-07-18 123900" src="https://github.com/user-attachments/assets/847beea1-fa92-4172-b468-d2d1d6ef88bf" />

---

## Detection Rule

```spl
index=main sourcetype=syslog "Failed password"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip
| where count >= 5
```

---

## Detection Logic

The search:

- Collects failed SSH authentication events.
- Extracts the source IP address.
- Counts the number of failed logins per IP.
- Generates an alert if an IP performs five or more failed authentication attempts.

---

## Alert Configuration

| Setting | Value |
|---------|-------|
| Alert Type | Real-Time |
| Trigger | Per Result |
| Alert Name | SSH Brute Force Detection |

Screenshot:
`04_alert_configuration.png`
<img width="1350" height="577" alt="Screenshot 2026-07-16 151832" src="https://github.com/user-attachments/assets/0753fd0e-4525-43f9-a677-cec4f92ad69e" />

---

## Validation

Run the Hydra attack again.

Verify that the alert triggers after the threshold is reached.
The alert has been triggered while monitoring in real time and successfully detected the brute force attack on SSH service.

---

## Security Impact

Repeated failed authentication attempts from a single source often indicate:

- Password guessing
- Credential stuffing
- Brute-force attacks
- Unauthorized access attempts

Early detection enables security teams to block malicious IP addresses before a successful compromise.

---

## Skills Demonstrated

- SSH Monitoring
- Hydra Attack Simulation
- Linux Log Analysis
- Splunk SPL
- Threshold-Based Detection
- Alert Engineering
- MITRE ATT&CK Mapping
- SOC Investigation

---

## Outcome

The SSH brute-force attack was successfully simulated using Hydra.

Multiple failed authentication attempts were detected in Splunk using a threshold-based detection rule,
and a real-time alert was generated for SOC monitoring.
