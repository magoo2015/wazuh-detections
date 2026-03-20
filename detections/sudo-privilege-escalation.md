# Sudo Privilege Escalation Detection

## Objective
Detect when a user successfully elevates privileges using sudo.

## Detection Logic
Triggers when a sudo session is opened for root.

## Rule

Rule ID: 100010  
Description: Sudo privilege escalation activity  

## Base Event
Derived from PAM sudo session logs.

## Example Log
sudo: pam_unix(sudo:session): session opened for user root(uid=0) by sysadmin(uid=1000)

## Test

```bash
sudo ls /root