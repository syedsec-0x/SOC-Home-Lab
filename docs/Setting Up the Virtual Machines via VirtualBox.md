## Overview

This document explains how VirtualBox was installed and configured to host the virtual machines used in the home SOC lab.

The lab uses VirtualBox to isolate the vulnerable target machine from the host operating system while allowing secure communication between the systems.


## Lab Architecture

| Component | Purpose |
|----------|---------|
| VirtualBox | Virtualization Platform |
| Windows 11 Host | Runs Splunk Enterprise |
| Metasploitable3 | Vulnerable Linux Target |
|Windows 10 VM | Runs SplunkUniversalforwarder |


## Step 1 - Install VirtualBox

Download the latest version of Oracle VirtualBox.

link:- https://www.virtualbox.org/wiki/Downloads

Run the installer using the default installation options.

During installation, VirtualBox installs the required networking drivers.

<img width="1804" height="816" alt="Screenshot 2026-06-30 153906" src="https://github.com/user-attachments/assets/0203f29a-f3db-4bbf-a503-6dac83710eec" />


## Step 2 - Launch VirtualBox

After installation, open VirtualBox.

The VirtualBox Manager should display an empty machine list.

> **Screenshot:** VirtualBox Manager
> <img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/f100861c-55dc-4a2e-bc2a-92c65b54c3c2" />

## Step 3 - Download the Metasploitable 3 Virtualbox

Download the latest version of metasploitable3.
https://sourceforge.net/projects/metasploitable3-ub1404upgraded/

<img width="1920" height="1080" alt="Screenshot 2026-06-30 154949" src="https://github.com/user-attachments/assets/bf5a7504-1cef-49e7-a462-32c2472161db" />

## step 4
Now have a look at your Downloads folder and you can see that metasploitable3.ova file must have been downloaded ,
you can directly open that .ova file and it will take you to the virtual box .

<img width="1228" height="229" alt="Screenshot 2026-06-30 155121" src="https://github.com/user-attachments/assets/990a515e-e97b-44e1-bac8-c1f47e2e0bff" />

## Step 5
#Before starting your VM , setup your virtualbox with necessary requirements.

<img width="1920" height="1080" alt="Screenshot 2026-06-30 160237" src="https://github.com/user-attachments/assets/b06b24e0-855c-4a55-849f-394488b19904" />

NOTE:=You can manually add the vm by starting the virtual box and on top left you can see the new button so you can add and locate your .ova file and virtual box will start your
vm for you.

Power on Metasploitable3.

Allow Ubuntu to boot completely.

> **Screenshot:** Ubuntu Login Screen
> <img width="640" height="480" alt="VirtualBox_Metasploitable3-ub1404_30_06_2026_16_09_29" src="https://github.com/user-attachments/assets/39995128-6d66-4e05-8566-b12e09b92511" />


Default Login credentials for the Metasploitable 3 ; --    usernamer:password = vagrant:vagrant

 <img width="640" height="480" alt="VirtualBox_Metasploitable3-ub1404_30_06_2026_16_10_54" src="https://github.com/user-attachments/assets/999dd380-5fed-4c16-b282-986ff08032ca" />

---

## Step 6 - Verify Network Connectivity

Inside Metasploitable3, verify the assigned IP address.

```bash
ip addr
```

The host-only interface should receive an IP address similar to:

```text
192.168.56.x
```

Verify communication with the Windows host.

```bash
ping 192.168.56.1
```

A successful response confirms network connectivity between the host and the virtual machine.


---

Configuring rsyslog on Metasploitable3
# Configuring rsyslog to Forward Logs to Splunk Enterprise

## Overview

The latest version of Splunk Universal Forwarder is not compatible with Ubuntu 14.04 due to library compatibility requirements. Since Metasploitable3 is based on Ubuntu 14.04, rsyslog was used to forward system logs to the Windows host running Splunk Enterprise.

This configuration enables centralized log collection from the Linux target into Splunk for monitoring and analysis.

---

## Lab Environment

| Component | Details |
|----------|---------|
| Source System | Metasploitable3 (Ubuntu 14.04) |
| Log Forwarder | rsyslog |
| Destination | Splunk Enterprise |
| Host IP Address | 192.168.56.1 |
| Protocol | UDP |
| Port | 5140 |

---

## Network Verification

Before configuring rsyslog, verify that the Windows host is reachable.

```bash
ping 192.168.56.1
```

Successful replies confirm that the virtual machine can communicate with the Splunk server.

> **Screenshot:** Successful ping to Windows Host

---

## Editing the rsyslog Configuration

Open the rsyslog configuration file.

```bash
sudo nano /etc/rsyslog.conf
```

Scroll to the bottom of the file and add the following line.

```text
*.* @192.168.56.1:5140
```

This forwards all system logs to the Splunk Enterprise server using UDP on port 5140.

> **Screenshot:** rsyslog.conf Configuration

---

## Restart the rsyslog Service

Apply the configuration changes.

```bash
sudo service rsyslog restart
```

Verify that the service is running.

```bash
sudo service rsyslog status
```

> **Screenshot:** rsyslog Service Status

---

## Generate a Test Log

Generate a custom syslog message.

```bash
logger "Hello Splunk"
```

This creates a new log entry that should immediately be forwarded to Splunk.

> **Screenshot:** Logger Command

---

## Verify Log Collection in Splunk

Log in to Splunk Enterprise and search for the generated message.

```spl
index=main "Hello Splunk"
```

If the message appears in the search results, the configuration is working correctly.

> **Screenshot:** Log Successfully Received in Splunk







