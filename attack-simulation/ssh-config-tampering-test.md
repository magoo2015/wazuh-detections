# SSH Configuration Tampering Test

## Purpose
Simulate modification of SSH configuration.

## Command

```bash
sudo bash -c 'echo "# ssh tamper lab test" >> /etc/ssh/sshd_config'