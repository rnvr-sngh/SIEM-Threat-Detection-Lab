# 🛡️ Enterprise SIEM Threat Detection Lab

> Simulated a real-world brute-force attack and detected it using a centralized SIEM — built entirely from scratch on a virtualized network.

---

## 📌 Project Goal

To replicate the workflow of a SOC (Security Operations Center) analyst by:
1. Building an isolated attacker-victim network using VirtualBox
2. Deploying the ELK Stack (Elasticsearch, Logstash, Kibana) as a SIEM
3. Launching a brute-force attack using Kali Linux tools
4. Detecting and visualizing the attack in Kibana using KQL queries

---

## 🏗️ Architecture

```
+-------------------+          Internal Network (labnet)          +----------------------+
|   Kali Linux      |  ---------------------------------------->  |   Ubuntu Server      |
|   (Attacker)      |          192.168.10.20 → 192.168.10.10      |   (Victim + ELK)     |
|   Hydra + Nmap    |                                              |   Elasticsearch      |
+-------------------+                                             |   Kibana             |
                                                                  |   Filebeat           |
                                                                  +----------------------+
```

**Network Design:**
- **NAT Adapter** — Internet access for both VMs
- **Internal Network (labnet)** — Isolated attack traffic, never touches the real network
- **Static IPs** — Attacker: `192.168.10.20` | Victim: `192.168.10.10`

---

## 🔧 Technologies Used

| Tool | Role |
|------|------|
| VirtualBox | Virtualization platform |
| Kali Linux | Attacker machine |
| Ubuntu Server 24.04 | Victim / SIEM host |
| Elasticsearch | Log storage and indexing |
| Kibana | Visualization and threat hunting |
| Filebeat | Log shipper (monitors `/var/log/auth.log`) |
| Hydra | Brute-force attack tool |
| Nmap | Network reconnaissance |
| KQL | Kibana Query Language for log filtering |
| OpenSSH | Remote server management |

---

## 📋 Steps Performed

### Phase 1 — Infrastructure Setup
- Installed VirtualBox and created two VMs (Kali Linux + Ubuntu Server)
- Configured dual network adapters: NAT + Internal Network (`labnet`)
- Assigned static IPs to both machines
- Set up SSH port forwarding (Port `2222` → `22`) to manage Ubuntu remotely via Windows PowerShell

### Phase 2 — SIEM Deployment (ELK Stack)
- Installed and configured **Elasticsearch** as the central log database
- Deployed **Kibana** (resolved SSL and version compatibility issues in Ubuntu 24.04)
- Installed **Filebeat** and configured it to ship `/var/log/auth.log` to Elasticsearch in real-time

### Phase 3 — Attack Simulation
- Used **Nmap** to perform port scanning and confirm SSH was open
- Launched a brute-force attack with **Hydra** using the `rockyou.txt` wordlist against the root account

### Phase 4 — Threat Detection
- Used **KQL** in Kibana Discover to filter `Failed password` events
- Isolated the attacker's source IP: `192.168.10.20`
- Built a bar chart in Kibana to visualize the spike in failed login attempts

---

## 📸 Evidence

### Screenshot A — Network Configuration
*VirtualBox showing both VMs on the `labnet` internal network*

![Network Config](screenshots/A_network_config.png)

---

### Screenshot B — Brute-Force Attack
*Kali Linux terminal running Hydra attack against Ubuntu SSH*

![Hydra Attack](screenshots/B_hydra_attack.png)

---

### Screenshot C — Log Evidence in Kibana
*Kibana Discover tab showing Failed password events from attacker IP 192.168.10.20*

![Kibana Logs](screenshots/C_kibana_logs.png)

---

### Screenshot D — Visual Dashboard
*Bar chart showing the spike in failed login attempts during the attack window*

![Dashboard Graph](screenshots/D_kibana_graph.png)

---

## 🚧 Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Kibana SSL/HTTPS errors on Ubuntu 24.04 | Modified `kibana.yml` and `elasticsearch.yml` to handle certificate configuration |
| SSH "Connection Refused" via NAT | Configured VirtualBox port forwarding rule (2222 → 22) |
| Filebeat not shipping logs | Fixed file permission on `/var/log/auth.log` using `chown` |
| Static IP not persisting after reboot | Edited Netplan configuration (`/etc/netplan/`) |

---

## 🎯 Key Skills Demonstrated

- ✅ **Network Segmentation** — Isolated attack environment using NAT + Internal Network
- ✅ **Linux Administration** — systemctl, YAML configs, file permissions, Netplan
- ✅ **SIEM Deployment** — Full ELK stack from scratch
- ✅ **Threat Hunting** — KQL queries to find IOCs (Indicators of Compromise)
- ✅ **SOC Workflow** — Full Attack → Log → Detect → Visualize pipeline
- ✅ **Troubleshooting** — Resolved real-world SSL, networking, and permission issues

---

## 👤 Author

Ranveer singh — 2nd Year cybersecurity B.TECH Student  
🔗 [LinkedIn](www.linkedin.com/in/ranveer-singh-479a58383) | 📧 ranveersingh8823@gmail.com

---

*Built as part of personal cybersecurity upskilling. All attacks were performed in an isolated lab environment.*
