# SSH Configuration Tampering Detection

## Objective
Detect privileged commands targeting /etc/ssh/sshd_config.

## Base Signal
5402 - Successful sudo to ROOT executed

## Custom Rule
100141 - SSH configuration targeted by sudo command

## Test
sudo bash -c 'echo "# ssh tamper lab test" >> /etc/ssh/sshd_config'

## Expected Result
Alert fires for rule.id 100141

## Learning Notes
This detection uses command-based matching against sudo activity to identify tampering with a high-value system configuration file.