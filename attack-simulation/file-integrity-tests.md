# File Integrity Monitoring Tests

## Purpose
Trigger file modification and deletion alerts.

## Commands

```bash
sudo touch /etc/wazuh-lab-file
sudo bash -c 'echo "test" >> /etc/wazuh-lab-file'
sudo rm /etc/wazuh-lab-file