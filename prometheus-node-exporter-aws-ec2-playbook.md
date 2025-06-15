# 🚀 Prometheus + Node Exporter on AWS EC2 — Quick Reference Playbook

> A fast and easy guide to monitor your AWS EC2 instances using Prometheus and Node Exporter.

---

## 1. 🖥️ EC2 Instances Overview

| Instance Role       | Purpose           | IP Address (replace with yours)    |  
|---------------------|-------------------|-----------------------------------|  
| 🔍 Monitoring Server | Runs Prometheus    | PROMETHEUS_PUBLIC_IP = 3.6.91.162 |  
| 📊 Monitored Node    | Runs Node Exporter | NODE_EXPORTER_PUBLIC_IP = 43.204.23.97 |  

---

## 2. 🔐 Connecting via SSH

ssh ec2-user@<INSTANCE_PUBLIC_IP>  
Replace <INSTANCE_PUBLIC_IP> with the instance's public IP.  
No .pem file needed if your setup works without it.

---

## 3. ⚙️ Node Exporter Setup (Monitored Node)

3.1 Check if Node Exporter service is running  
sudo systemctl status node_exporter  

3.2 Start Node Exporter service if stopped  
sudo systemctl start node_exporter  

3.3 Enable Node Exporter to start at boot (once only)  
sudo systemctl enable node_exporter  

---

## 4. ⚙️ Prometheus Setup (Monitoring Server)

4.1 Check Prometheus service status  
sudo systemctl status prometheus  

4.2 Start Prometheus service if stopped  
sudo systemctl start prometheus  

4.3 Enable Prometheus to start at boot (once only)  
sudo systemctl enable prometheus  

---

## 5. 📝 Prometheus Configuration File

Edit /opt/prometheus/prometheus.yml:

global:  
  scrape_interval: 15s  
  evaluation_interval: 15s  

scrape_configs:  
  - job_name: "prometheus"  
    static_configs:  
      - targets: ["localhost:9090"]  

  - job_name: "node_exporter"  
    static_configs:  
      - targets: ["43.204.23.97:9100"]   # Node Exporter instance IP  

After editing, restart Prometheus:  
sudo systemctl restart prometheus  

---

## 6. 🌐 Access Prometheus UI

Open in your browser:  
http://3.6.91.162:9090  

Go to Status > Targets to verify both Prometheus and Node Exporter are UP.

---

## 7. 🔒 Security Group Rules (AWS Console)

- Node Exporter Instance: Allow Inbound TCP 9100 from Prometheus instance's private IP.  
- Prometheus Instance: Allow Inbound TCP 9090 from your laptop's IP for web UI access.

---

## 8. ⚡ Quick Start/Restart Script (Optional)

Create a script `start-monitoring.sh` on each instance:  

#!/bin/bash  
sudo systemctl start node_exporter   # On Node Exporter instance  
sudo systemctl start prometheus      # On Prometheus instance  

Make it executable:  
chmod +x start-monitoring.sh  

Run anytime to quickly start services:  
./start-monitoring.sh  

---

## 9. 🛠️ How to Pick Up Tomorrow

1. Boot your laptop.  
2. SSH into Prometheus and Node Exporter instances.  
3. Run:  
   sudo systemctl status prometheus  
   sudo systemctl status node_exporter  
4. Start any stopped services:  
   sudo systemctl start <service>  
5. Access Prometheus UI at http://3.6.91.162:9090 and verify targets.

---

💡 **Tip:** Bookmark the Prometheus UI URL for quick access!

---

Thank you for using this playbook! 🎉 For more information, refer to official Prometheus and Node Exporter documentation.
