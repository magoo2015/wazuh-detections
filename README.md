# Wazuh Detection Engineering Lab

## Overview

This project documents a personal security engineering lab built on DigitalOcean using Wazuh.

The goal of the lab is to practice:

- Detection engineering
- Security monitoring
- Threat simulation
- Linux host hardening
- SOC alert triage

## Architecture

Manager:
- Ubuntu 22.04
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard

Agent:
- Ubuntu 22.04
- Wazuh Agent

## Network Architecture

Windows Desktop
     |
     | SSH
     v
DigitalOcean

wazuh-manager
   |
   | Wazuh agent communication
   |
wazuh-agent

## Detection Engineering Goals

- Detect sudo privilege escalation
- Detect SSH brute force
- Detect file integrity changes
- Create custom Wazuh detection rules

## Tools Used

- Wazuh
- DigitalOcean
- Linux (Ubuntu)
- GitHub