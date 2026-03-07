# Lab Architecture

## Infrastructure

Cloud Provider: DigitalOcean

Nodes:

1. wazuh-manager
   - Wazuh Manager
   - Wazuh Indexer
   - Wazuh Dashboard

2. wazuh-agent
   - Linux endpoint monitored by Wazuh

## Communication

Agent → Manager on port 1514

Dashboard accessible on port 443