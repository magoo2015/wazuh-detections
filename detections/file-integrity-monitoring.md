# File Integrity Monitoring Detection

## Objective
Detect modifications or deletion of files in protected directories.

## Base Rules

550 - Integrity checksum changed  
553 - File deleted

## Custom Rules

100130 - File integrity modification detected  
100131 - Protected file deleted

## Test

sudo touch /etc/wazuh-lab-file
sudo bash -c 'echo test >> /etc/wazuh-lab-file'
sudo rm /etc/wazuh-lab-file

## Expected Result

Alerts triggered for file modification and deletion in monitored directories.