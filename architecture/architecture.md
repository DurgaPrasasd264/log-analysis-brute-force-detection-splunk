# 🧪 Lab Architecture

    ┌──────────────────────┐
    │   Kali Linux         │
    │   (Attacker)         │
    │  - Nmap              │
    │  - Hydra             │
    │  - Remmina           │
    │  - Wireshark         │
    └─────────┬────────────┘
              │
              │  (RDP Brute Force - Port 3389)
              │
    ┌─────────▼────────────┐
    │   Windows 11         │
    │   (Target System)    │
    │  - RDP Enabled       │
    │  - Event Logs        │
    │  - Test User         │
    │  - Splunk Forwarder  │
    └─────────┬────────────┘
              │
              │  (Log Forwarding - Port 9997)
              │
    ┌─────────▼────────────┐
    │   Ubuntu Server      │
    │   (Splunk SIEM)      │
    │  - Log Collection    │
    │  - Log Analysis      │
    │  - Dashboards        │
    └──────────────────────┘ 

## Overview
This lab simulates a SOC environment with attacker, target, and SIEM components.

## Components

### Attacker (Kali Linux)
- Performs scanning (Nmap)
- Executes brute force (Hydra)
- Access via RDP (Remmina)
- Captures traffic (Wireshark)

### Target (Windows 11)
- RDP enabled (Port 3389)
- Generates security logs
- Runs Splunk Universal Forwarder

### SIEM (Ubuntu - Splunk)
- Receives logs on port 9997
- Indexes and analyzes logs
- Provides dashboards

## Flow

Kali → Attack → Windows → Logs → Splunk → Detection
