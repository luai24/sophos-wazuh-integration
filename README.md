# Wazuh - Sophos Integration
This repository provides a complete integration between Wazuh (SIEM and XDR platform) and Sophos Firewall, supporting both the Central Reporting Format (CRF) and Standard Syslog Protocol. It includes custom decoders, rules, and MITRE ATT&CK mappings to detect and alert on Sophos security events such as:

➕ Web filtering violations

➕ IPS detections

➕ AV/ATP detections

➕ Application control

➕ SSL VPN login failures

➕ Firewall rule matches

➕ Email protection events

➕ Allowed and Denied Traffic

The integration enhances visibility into your Sophos security stack, allowing Wazuh to parse, correlate, and generate meaningful alerts from Sophos logs.

## Features
✅ Custom XML decoders for Sophos CRF

✅ Fine-grained rules with severity levels

✅ MITRE ATT&CK technique tagging

✅ Support for multiple Sophos log types

✅ Plug-and-play rule group
## Integration Guide
This guide walks you through integrating Sophos Firewall with Wazuh for advanced log monitoring and alerting using the Central Reporting Format (CRF) or Standard Syslog Protocol.

This guide walks you through integrating Sophos Firewall with Wazuh for advanced log monitoring and alerting using the Central Reporting Format (CRF) or Standard Syslog Protocol.

### 🧱 Prerequisites
🧠 A running Wazuh Manager (v4.x recommended)
🔥 Sophos Firewall (XG/XGS series) with Syslog capability
🔐 Root access to the Wazuh Manager
🌐 Network connectivity between Sophos and Wazuh (UDP/514)

### 1️⃣ Enable Syslog Forwarding on Sophos Firewall
🔑 Log in to the Sophos Admin Portal
🧭 Go to System Services → Log Settings → Syslog Server
➕ Add a new syslog server with the following:
🖥️ IP Address: Wazuh Manager IP
📦 Port: 514
🧾 Format: Central Reporting Format or Standard Syslog Format
🏷️ Facility: Local6
⚙️ Severity: Information or as needed
📂 Categories: Firewall, Web, IPS, SSL VPN, ATP, etc.
💾 Save and Apply

### 2️⃣ Allow Sophos IP in ossec.conf
📝 Edit the Wazuh config: `sudo nano /var/ossec/etc/ossec.conf`

🔐 Add your Sophos IP: `<remote>
  <connection>syslog</connection>
  <port>514</port>
  <protocol>udp</protocol>
  <allowed-ips>192.168.1.1</allowed-ips> <!-- Replace with actual Sophos IP -->
</remote>
`

♻️ Restart the wazuh manager: `systemctl restart wazuh-manager`

### 3️⃣ Verify Log Reception: 
🔍 Capture traffic with tcpdump: `sudo tcpdump -i any -nn -A udp port 514 and src host <sophos_ip_address>`

📥 Confirm logs like: `<30>device_name="SFW" timestamp="..." log_type="Firewall" log_subtype="Allowed" ...`

### 4️⃣ Deploy Decoders

### 5️⃣ Deploy Rules

### 6️⃣ Validate Configuration

🔄 Restart the manager: `sudo systemctl restart wazuh-manager`

🧪 Test it: `sudo /var/ossec/bin/wazuh-logtest`

Special thanks to JoernSchoenyan for providing the decoders!
