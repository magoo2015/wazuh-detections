# Account Management Detection

## Objective
Detect creation and deletion of user accounts.

## Detection Logic

### Account Deletion
Rule ID: 100120  
Based on built-in rule: 5903  

### Account Creation
Rule ID: 100121  
Based on built-in useradd events  

## Test

```bash
sudo useradd labuser1
sudo userdel labuser1