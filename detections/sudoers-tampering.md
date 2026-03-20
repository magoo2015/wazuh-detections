# Sudoers File Tampering Detection

## Objective
Detect modifications to /etc/sudoers.

## Detection Logic
Triggers when FIM detects modification of sudoers file.

## Rule

Rule ID: 100142  
Parent Rule: 550 (FIM modification)

Match:
/etc/sudoers

## Test

```bash
sudo visudo