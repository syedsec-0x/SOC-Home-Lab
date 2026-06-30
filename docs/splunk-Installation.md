# Splunk Enterprise Installation on Windows

## Overview

This document describes the installation of Splunk Enterprise on a Windows host machine. Splunk Enterprise serves as the Security Information and Event Management (SIEM) platform for my home SOC lab, where it is used to collect, index, search, and analyze log data.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Operating System | Windows 11 |
| SIEM Platform | Splunk Enterprise |
| Installation Type | Local Installation |
| Installation Method | Windows Installer (.msi) |

---


## Step 1 - Download Splunk Enterprise

Sign up by filling up your details scroll down and agree to terms and conditions and click on create account.
login >> on top right corner you will get an option start free trials click on that button and scroll and find splunk enterprise >>and click on start my free trial and download the latest version by seeing your device compatibility.

   A
> <img width="1920" height="1020" alt="Screenshot 2026-06-30 150943" src="https://github.com/user-attachments/assets/04fc8481-80bc-4ca8-a7fb-bd8aeb609054" />
<img width="1920" height="1020" alt="Screenshot 2026-06-30 151017" src="https://github.com/user-attachments/assets/64821aed-d5ac-4578-a502-e831377b71cb" />
<img width="1059" height="492" alt="Screenshot 2026-06-30 151118" src="https://github.com/user-attachments/assets/839b2310-08fd-436d-be7b-dd9ebe873428" />
<img width="1063" height="545" alt="Screenshot 2026-06-30 151140" src="https://github.com/user-attachments/assets/31037b59-851c-44bb-a3ec-0b0234ba29cf" />

---

## Step 2 - Install Splunk Enterprise

Follow the installation wizard and complete the installation.

During the installation:

- Accept the license agreement.
- Choose the installation directory (default location recommended).
- Create an administrator username and password.
- Continue until the installation is complete.

The default installation directory is:

```text
C:\Program Files\Splunk
```
proceed with the above instructions and if getting confused while configuring just keep the things on default and move and install the splunk.

> <img width="525" height="243" alt="Screenshot 2026-06-30 151313" src="https://github.com/user-attachments/assets/a09c5fe9-6d0a-4b88-89b5-8ee94ffc9dc6" />


---

## Step 3 - Launch Splunk Enterprise

After the installation is complete, Splunk Enterprise starts automatically.

If it does not start automatically, launch it from the Start Menu or start the **Splunkd** service manually.

> <img width="931" height="526" alt="Screenshot 2026-06-30 152259" src="https://github.com/user-attachments/assets/ec29a46a-bf24-4cdd-8244-508d2bb98d8f" />


---

## Step 4 - Access the Splunk Web Interface

Open a web browser and navigate to:

```text
https://localhost:8000
```

A browser security warning may appear because Splunk uses a self-signed certificate. Proceed to the website and log in using the administrator credentials created during installation.

> **Screenshot:** Splunk Login Page
> <img width="1249" height="619" alt="Screenshot 2026-06-30 152440" src="https://github.com/user-attachments/assets/561c8da1-7ed5-4f95-91b7-a010a85b449f" />


---

## Step 5 - Verify the Installation

After logging in successfully, the Splunk Home dashboard should be displayed.

This confirms that Splunk Enterprise has been installed successfully and is ready for configuration.

> **Screenshot:** Splunk Home Dashboard
> <img width="1887" height="848" alt="Screenshot 2026-06-30 152547" src="https://github.com/user-attachments/assets/0bceaf3f-1070-45b7-8688-da7ed57849c4" />


---

## Conclusion

Splunk Enterprise was successfully installed on a Windows 11 host machine and verified through the Splunk Web interface. The SIEM platform is now ready for further configuration, including log collection and monitoring in the home SOC lab.

