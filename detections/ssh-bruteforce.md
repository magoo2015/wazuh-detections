# SSH Brute Force Detection

## Objective
Learn how Wazuh correlation rules detect repeated authentication failures over time.

## Detection Logic
This rule triggers when multiple SSH authentication failure events occur within a short period.

## Rule
```xml
<group name="local,ssh,bruteforce,">

  <rule id="100100" level="10" frequency="5" timeframe="60">
    <if_matched_sid>5710</if_matched_sid>
    <description>SSH brute force attack detected</description>
    <group>authentication_failed,bruteforce,</group>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>

</group>

MITRE ATT&CK

T1110 - Brute Force

Simulation

Repeated failed SSH login attempts against the Ubuntu host.

Expected Result

Wazuh generates alert rule.id:100100 with description:

SSH brute force attack detected

Learning Notes

This detection demonstrates:

built-in SSH event decoding

correlation using frequency and timeframe

custom rule creation based on matched events