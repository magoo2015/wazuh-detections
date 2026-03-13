# SSH Brute Force Test

## Purpose
Generate repeated SSH authentication failures to trigger the brute force detection rule.

## Test Method
From a separate system, attempt repeated logins with a bad username or bad password.

Example:
```bash
ssh fakeuser@TARGET_IP

Repeat 5 or more times within 60 seconds.

Expected Wazuh Alert

Rule ID: 100100

Description: SSH brute force attack detected

# What you learned from this issue

This failure was actually a useful lab lesson.

## Lesson 1 — Wazuh is strict about rule syntax

A rule can be logically correct but still fail because of XML structure.

## Lesson 2 — correlation rules differ from simple match rules

Your sudo rule is a straightforward content match:

```xml
<if_sid>5501</if_sid>
<match>sudo</match>