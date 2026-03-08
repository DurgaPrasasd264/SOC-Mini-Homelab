# SOC Mini Homelab – Lab Setup Guide

This guide explains how to recreate the SOC Mini Homelab environment used in this project.

The lab simulates a small Security Operations Center (SOC) where multiple systems send logs to a centralized **Splunk SIEM server**.

---

# System Requirements

Recommended system specifications:

- RAM: 16 GB minimum
- Storage: 200 GB
- CPU: 4 cores or higher
- Virtualization Software: VMware Workstation or VirtualBox

---

# Virtual Machines Used

The lab environment includes the following machines:

| Machine | Operating System | Role |
|-------|-----------------|------|
| Kali Linux | Kali Linux | Attacker machine |
| Windows 11 | Windows 11 | Endpoint system |
| Windows Server | Windows Server 2022 | Server system |
| SIEM Server | Ubuntu Server | Splunk SIEM server |

---

# Software Downloads

## Kali Linux

Download Kali Linux:

https://www.kali.org/get-kali/

Recommended: Kali Linux VMware / VirtualBox VM image.

---

## Windows 11

Download Windows 11 ISO:

https://www.microsoft.com/software-download/windows11

---

## Windows Server 2022

Download Windows Server evaluation ISO:

https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022

---

## Ubuntu Server

Download Ubuntu Server:

https://ubuntu.com/download/server

Recommended version: Ubuntu Server 22.04 LTS

---

# Splunk Downloads

## Splunk Enterprise

Download Splunk Enterprise:

https://www.splunk.com/en_us/download/splunk-enterprise.html

---

## Splunk Universal Forwarder

Download Splunk Universal Forwarder:

https://www.splunk.com/en_us/download/universal-forwarder.html

---

# Network Configuration

All machines should be connected to the same virtual network.

Example configuration:

| Machine | IP Address |
|-------|-------------|
| Kali Linux | 192.168.1.10 |
| Windows 11 | 192.168.1.20 |
| Windows Server | 192.168.1.30 |
| Ubuntu Splunk Server | 192.168.1.40 |

---

# Installing Splunk on Ubuntu Server

Upload the Splunk `.deb` package to the Ubuntu server.

Install Splunk:
```
sudo dpkg -i splunk*.deb
```

Start Splunk:
```
sudo /opt/splunk/bin/splunk start
```
Enable Splunk to start automatically:
```
sudo /opt/splunk/bin/splunk enable boot-start
```
Access Splunk Web Interface:
```
http://<server-ip>:8000

Login using the admin account created during setup.

Enable Log Receiving

Login to Splunk Web
```
Go to:
```
Settings → Forwarding and Receiving
```
Click:
```
Configure Receiving
```
Add receiving port:
```
9997

This allows forwarders to send logs to the Splunk server.
```

# Install Splunk Universal Forwarder (Windows)

Install the Splunk Universal Forwarder on:

Windows 11

Windows Server

During installation configure:

```Receiver IP: <Splunk Server IP>
Port: 9997
Configure Windows Log Forwarding
```
Edit the configuration file:
```
C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
```
Add the following configuration:
```
[WinEventLog://Security]
disabled = 0

[WinEventLog://System]
disabled = 0

[WinEventLog://Application]
disabled = 0

Restart the Splunk Forwarder service after editing the file.

Verify Log Collection
```
Login to Splunk and run the following search:
```
index=*
```
To view logs by host:
```
index=* | stats count by host
```
If logs appear from Windows machines, the configuration is successful.

---

# Testing the Lab

Generate activity on the systems to produce logs:

Examples:
```
Login attempts on Windows machines

File access events

Network activity

System log events

These logs will appear in Splunk for monitoring and analysis.
```

---
# Result

After completing the setup:
```
Windows machines forward logs to Splunk

Splunk indexes and stores the logs

Security events can be monitored through the Splunk interface
```

This environment simulates a basic SOC monitoring lab used for learning SIEM operations.
