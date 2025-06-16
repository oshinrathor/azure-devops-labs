===============================================================================
🧰  FULL SETUP: GRAFANA ON AMAZON LINUX EC2 WITH PROMETHEUS + NODE EXPORTER
===============================================================================

PREREQUISITES
-------------------------------------------------------------------------------
- Amazon EC2 instance running Amazon Linux 2
- Security Group allows inbound traffic on:
    • TCP 3000 (Grafana)
    • TCP 9090 (Prometheus)
    • TCP 9100 (Node Exporter)
- Node Exporter and Prometheus already running
- Sudo access on the instance


KEY TERMS
-------------------------------------------------------------------------------
Term             | Description
-----------------|------------------------------------------------------------
Grafana          | Web-based dashboard and visualization tool for metrics/logs
Prometheus       | Metrics monitoring system that scrapes metrics from exporters
Node Exporter    | Prometheus exporter exposing Linux system metrics (CPU, RAM)
EC2              | Amazon Elastic Compute Cloud — virtual machine on AWS


INSTALL AND CONFIGURE GRAFANA
-------------------------------------------------------------------------------

🔹 STEP 1: CONNECT TO EC2

    ssh -i /path/to/your-key.pem ec2-user@<your-ec2-public-ip>


🔹 STEP 2: ADD GRAFANA YUM REPOSITORY

    sudo tee /etc/yum.repos.d/grafana.repo <<EOF
    [grafana]
    name=grafana
    baseurl=https://packages.grafana.com/oss/rpm
    repo_gpgcheck=1
    enabled=1
    gpgcheck=1
    gpgkey=https://packages.grafana.com/gpg.key
    EOF


🔹 STEP 3: INSTALL GRAFANA

    sudo yum install -y grafana


🔹 STEP 4: START AND ENABLE GRAFANA

    sudo systemctl start grafana-server
    sudo systemctl enable grafana-server

    (This ensures Grafana starts automatically on reboot.)


🔹 STEP 5: OPEN PORT 3000 IN SECURITY GROUP

In the AWS Console:
    1. Go to EC2 → Instances → [Your Instance]
    2. Scroll to Security → Security Groups
    3. Click Edit Inbound Rules
    4. Add Rule:
        • Type: Custom TCP
        • Port: 3000
        • Source: 0.0.0.0/0 (or restrict to your IP for security)


🔹 STEP 6: ACCESS GRAFANA UI

    Open browser: http://<your-ec2-public-ip>:3000

    Default credentials:
        • Username: admin
        • Password: admin

    You will be prompted to change the password.


🔹 STEP 7: CONNECT PROMETHEUS AS A DATA SOURCE

In Grafana UI:
    1. Click ⚙️  (Gear icon) → Data Sources
    2. Click "Add data source"
    3. Choose "Prometheus"
    4. Set the URL to your Prometheus server:

        Example: http://<prometheus-ip>:9090
                 http://43.204.23.97:9090

    5. Click "Save & Test"
       (You should see: ✅ Data source is working)


🔹 STEP 8: IMPORT NODE EXPORTER DASHBOARD

    1. In Grafana, go to + → Import
    2. Enter dashboard ID: 1860
    3. Click "Load"
    4. Select your Prometheus data source
    5. Click "Import"

    ✔ You will now see full system metrics via Node Exporter


SUMMARY OF COMMANDS USED
-------------------------------------------------------------------------------

    # Connect to EC2
    ssh -i /path/to/key.pem ec2-user@<ec2-ip>

    # Add Grafana repo
    sudo tee /etc/yum.repos.d/grafana.repo <<EOF
    [grafana]
    name=grafana
    baseurl=https://packages.grafana.com/oss/rpm
    repo_gpgcheck=1
    enabled=1
    gpgcheck=1
    gpgkey=https://packages.grafana.com/gpg.key
    EOF

    # Install Grafana
    sudo yum install -y grafana

    # Start and enable Grafana
    sudo systemctl start grafana-server
    sudo systemctl enable grafana-server


OPTIONAL: MONITOR THE SERVICES
-------------------------------------------------------------------------------

To check if Grafana is running:

    sudo systemctl status grafana-server

To restart Grafana:

    sudo systemctl restart grafana-server


USEFUL LINKS
-------------------------------------------------------------------------------
- Grafana OSS RPM Docs     : https://grafana.com/docs/grafana/latest/setup-grafana/installation/rpm/
- Prometheus Downloads     : https://prometheus.io/download/
- Node Exporter GitHub     : https://github.com/prometheus/node_exporter
