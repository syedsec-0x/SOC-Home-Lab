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

## Conclusion

VirtualBox was successfully installed, and Metasploitable3 was imported and configured with dual network adapters.






