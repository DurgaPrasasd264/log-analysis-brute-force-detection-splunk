# 🔐 Windows Log Analysis & Brute Force Detection using Splunk

## 📌 Overview
This project simulates a brute force attack on a Windows system and demonstrates how a SOC Analyst detects and investigates the attack using Splunk SIEM, RDP access, and network traffic analysis.

---

## 🧪 Lab Architecture

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

---

## ⚙️ Tools Used

- Splunk Enterprise (Ubuntu)
- Splunk Universal Forwarder (Windows)
- Kali Linux
- Hydra
- Nmap
- Remmina (GUI RDP)
- Wireshark

---

## 🔨 STEP 1: SPLUNK SETUP (UBUNTU)

### Install Splunk

wget -O splunk.tgz https://download.splunk.com/products/splunk/releases/9.x/linux/splunk.tgz

tar -xvzf splunk.tgz
cd splunk/bin
sudo ./splunk start --accept-license


### Enable Receiving Port

Settings → Forwarding and Receiving → Receive Data → Add Port → 9997


---

## 🔨 STEP 2: WINDOWS SETUP

### Create User

net user testuser Password123 /add
net localgroup "Remote Desktop Users" testuser /add


### Enable RDP

SystemPropertiesRemote

✔ Enable Remote Desktop

---

## 🔨 STEP 3: SPLUNK FORWARDER (WINDOWS)

### Install Universal Forwarder

### Configure outputs.conf

[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = <Ubuntu-IP>:9997


### Configure inputs.conf

[WinEventLog://Security]
disabled = 0


---

## 🔨 STEP 4: ATTACK (KALI LINUX)

### Scan Target

nmap -p 3389 192.168.213.130

![](https://github.com/DurgaPrasasd264/log-analysis-brute-force-detection-splunk/blob/1f68ca5436692d9cebbb5ad6ae5e20d8703f056d/SCREENSHOTS/image1.png)


### Brute Force (RDP)

hydra -l testuser -P passwords.txt rdp://192.168.213.130 -t 1 -V


✔ I saw:
- Failed attempts  
- Successful password

![](https://github.com/DurgaPrasasd264/log-analysis-brute-force-detection-splunk/blob/1f68ca5436692d9cebbb5ad6ae5e20d8703f056d/SCREENSHOTS/image2.png)


---

## 🔨 STEP 5: REMOTE ACCESS (REMmina GUI)

### Install Remmina

apt update
apt install remmina


### Open:
Applications → Internet → Remmina

### Configure:
- Protocol: RDP  
- Server: 192.168.213.130  
- Username: testuser  
- Password: Password123  

✔ Click **Connect**

✔ Remote Windows desktop opens

![](https://github.com/DurgaPrasasd264/log-analysis-brute-force-detection-splunk/blob/1f68ca5436692d9cebbb5ad6ae5e20d8703f056d/SCREENSHOTS/image3.png)

---

## 🔨 STEP 6: WIRESHARK ANALYSIS

### Start Capture
- Open Wireshark in Kali
- Select active interface

### Apply Filter

tcp.port == 3389


### Observe:
- RDP handshake packets  
- Multiple connection attempts  
- Traffic spike during brute force

![](https://github.com/DurgaPrasasd264/log-analysis-brute-force-detection-splunk/blob/1f68ca5436692d9cebbb5ad6ae5e20d8703f056d/SCREENSHOTS/image6.png)

---

## 🔨 STEP 7: SPLUNK DETECTION

### Failed Logins

index=* EventCode=4625
| stats count by Account_Name, src_ip


### Successful Login

index=* EventCode=4624


### Brute Force Detection

index=* (EventCode=4624 OR EventCode=4625)
| stats count by EventCode, Account_Name, src_ip

![](https://github.com/DurgaPrasasd264/log-analysis-brute-force-detection-splunk/blob/1f68ca5436692d9cebbb5ad6ae5e20d8703f056d/SCREENSHOTS/image4.png)

---

## 📊 DASHBOARD

Create panels:

- Failed login attempts  
- Successful logins  
- Source IP activity  
- Targeted usernames

![](https://github.com/DurgaPrasasd264/log-analysis-brute-force-detection-splunk/blob/1f68ca5436692d9cebbb5ad6ae5e20d8703f056d/SCREENSHOTS/image5.png)

---

## 🧠 MITRE ATT&CK

- T1110 – Brute Force  
- T1021 – Remote Services  
- T1078 – Valid Accounts  

---

## 🔍 RESULTS

- Multiple failed login attempts detected  
- Successful compromise identified  
- Attacker IP correlated  
- RDP traffic confirmed in network  

---

## 💼 SKILLS GAINED

- SIEM Monitoring  
- Log Analysis  
- Incident Detection  
- Network Analysis  
- SOC Workflow  


---

## 🚀 CONCLUSION

Successfully simulated a brute force attack and detected it using Splunk SIEM and network analysis tools, replicating a real-world SOC environment.
