# Windows Setup

## Create User
net user testuser Password123 /add
net localgroup "Remote Desktop Users" testuser /add

## Enable RDP
SystemPropertiesRemote

## Install Splunk Forwarder
- Install Universal Forwarder
- Configure outputs.conf
- Configure inputs.conf
