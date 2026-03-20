## Wazuh Detection Engineering Lab

### Lab Objective

This lab was created to learn **Wazuh detection engineering fundamentals** using a minimal-cost cloud environment.
The focus is learning how logs flow through the Wazuh pipeline and how to build custom detection rules.

The environment is hosted on **DigitalOcean** and accessed from a **Windows desktop via PuTTY**.

---

# Architecture

Windows Desktop
│
│ SSH (PuTTY)
▼
Wazuh Manager (DigitalOcean)
│
│ Wazuh Agent Communication
▼
Ubuntu Agent Host (DigitalOcean)

---

# Infrastructure

## Wazuh Manager

Droplet Size
2 vCPU
4 GB RAM
Ubuntu 22.04

Installed using:

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

Services installed:

Wazuh Manager
Wazuh Indexer
Wazuh Dashboard
Filebeat

Dashboard accessible via:

```
https://MANAGER_PUBLIC_IP
```

---

## Wazuh Agent

Droplet Size
1 vCPU
1 GB RAM

Installed with:

```
sudo WAZUH_MANAGER="MANAGER_PRIVATE_IP" apt install wazuh-agent -y
```

Agent started with:

```
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

# Security Hardening

Root SSH login disabled
SSH key authentication enabled
Created non-root user:

```
sysadmin
```

Added sudo privileges.

SSH keys generated using **PuTTYgen** and stored in:

```
~/.ssh/authorized_keys
```

---

# Detection Engineering Progress

## 1. Log Ingestion Verification

Verified that logs from the agent reach the manager.

Example alert:

```
rule.id:5501
PAM: Login session opened
```

---

## 2. Custom Detection Rule (Privilege Escalation)

Created a custom rule to detect sudo activity.

File:

```
/var/ossec/etc/rules/local_rules.xml
```

Rule:

```xml
<group name="local,privilege_escalation">
  <rule id="100010" level="8">
    <if_sid>5501</if_sid>
    <match>sudo</match>
    <description>Sudo privilege escalation activity</description>
  </rule>
</group>
```

Restarted manager:

```
sudo systemctl restart wazuh-manager
```

Successful alert example:

```
rule.id:100010
Sudo privilege escalation activity
```

---

## 3. Rule Testing

Used:

```
/var/ossec/bin/wazuh-logtest
```

Confirmed decoder pipeline:

```
Decoder: PAM
Rule triggered: 5501
Custom rule triggered: 100010
```

---

## 4. Command Execution Detection

Observed alerts containing executed commands:

Example:

```
COMMAND=/usr/bin/rm /etc/wazuh-lab-file
```

Fields available for detection:

```
data.command
srcuser
dstuser
tty
pwd
```

---

## 5. File Integrity Monitoring (FIM)

Enabled via syscheck.

Configuration:

```
/var/ossec/etc/ossec.conf
```

```
<syscheck>
  <frequency>43200</frequency>
  <scan_on_start>yes</scan_on_start>
  <directories>/etc,/usr/bin,/usr/sbin</directories>
</syscheck>
```

Observation:
FIM operates on scheduled scans rather than real-time events.

FIM now operating on realtime monitoring.

<syscheck>
  <disabled>no</disabled>
  <frequency>43200</frequency>
  <scan_on_start>yes</scan_on_start>
  <directories realtime="yes">/etc</directories>
  <directories realtime="yes">/usr/bin</directories>
  <directories realtime="yes">/usr/sbin</directories>
  <directories realtime="yes">/bin</directories>
  <directories realtime="yes">/sbin</directories>
  <directories realtime="yes">/boot</directories>
</syscheck>

---

# Key Concepts Learned

Wazuh detection pipeline:

Log Source
↓
Decoder
↓
Base Rule
↓
Custom Rule
↓
Alert
↓
Indexed in OpenSearch
↓
Visible in Dashboard

# Key learning notes to add

wazuh-analysisd -t did not provide useful output in this lab, so service restart plus observed alerts was the practical validation method.

Simple XML mistakes in local_rules.xml can prevent the manager from starting.

Correlation rules are stricter than simple match rules.

Not every sudo-related event contains command text.

Different activities may be better detected through different telemetry:

auth logs

sudo logs

account management events

FIM/syscheck

For some detections, broad <match> worked more reliably than field-specific matching in this lab.

visudo produced better signal through FIM than command-line matching.

Realtime syscheck was required for immediate FIM learning feedback.

---

# Next Learning Goals

Build detections for:

Cron persistence detection

Target paths:

/etc/crontab

/etc/cron.d/

/var/spool/cron/

This fits perfectly with the FIM work you already completed.

After that:

/etc/passwd or /etc/shadow tampering

reverse shell detection

suspicious download tooling

/etc/sudoers.d/ monitoring




The goal of this project is **learning detection engineering rather than building a production SOC.**
