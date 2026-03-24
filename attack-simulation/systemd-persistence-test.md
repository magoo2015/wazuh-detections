# Systemd Persistence Test

## Objective

Simulate persistence via creation of a malicious systemd service and validate Wazuh detections.

MITRE ATT&CK:
T1543.002 – Create or Modify System Process: Systemd Service

---

## Create test service

Create a new systemd service file.

```bash
sudo tee /etc/systemd/system/lab-persist.service > /dev/null <<'EOF'
[Unit]
Description=Lab Persistence Test Service

[Service]
Type=simple
ExecStart=/bin/bash -c 'echo systemd persistence test >> /tmp/systemd-persist.log; sleep 300'

[Install]
WantedBy=multi-user.target
EOF

Reload systemd configuration
sudo systemctl daemon-reload
Enable persistence
sudo systemctl enable lab-persist.service

Optional:

sudo systemctl start lab-persist.service
Modify service file to trigger FIM detection
echo "# persistence modification test" | sudo tee -a /etc/systemd/system/lab-persist.service
Expected alerts in Wazuh dashboard
Command telemetry detection

Rule ID:
100162

Description:
Systemd-related sudo command detected - possible persistence activity

Example commands detected:

systemctl daemon-reload
systemctl enable
systemctl start

File integrity monitoring detection

Rule ID:
100160

Description:
Systemd service persistence detected - system service path modified

Example telemetry:

syscheck.path = /etc/systemd/system/lab-persist.service

Validate alerts

Search queries:

rule.id:100160

rule.id:100162

lab-persist.service

Cleanup

Stop and disable the service.

sudo systemctl stop lab-persist.service
sudo systemctl disable lab-persist.service

Remove service file.

sudo rm -f /etc/systemd/system/lab-persist.service
sudo systemctl daemon-reload

Remove test artifact.

sudo rm -f /tmp/systemd-persist.log
Notes

Systemd persistence detection may not trigger FIM alert on first file creation if directory was not previously indexed.

Modifying the service file again reliably triggers FIM event.

Layered detection improves reliability:

Command telemetry:
detects systemctl activity

File integrity monitoring:
detects service file creation or modification