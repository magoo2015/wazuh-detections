# Objective

## Detect persistence via systemd service creation or modification.

MITRE ATT&CK

T1543.002 — Create or Modify System Process: Systemd Service

Paths monitored

/etc/systemd/system
/usr/lib/systemd/system

Rules

100160
100161
100162

Example telemetry observed

syscheck.path = /etc/systemd/system/lab-persist.service

Lessons learned

systemd persistence may not trigger FIM on first file creation
modifying file again produces reliable detection