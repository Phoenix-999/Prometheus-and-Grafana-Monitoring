# **Installing Prometheus and Grafana Monitoring on Ubuntu Server**

__________________________________________________________________________________________
## **Step 01 - Installing Prometheus**


> [Prometheus Download Files Here](https://prometheus.io/download/)

At first, make sure to update your scripts in line with the latest version from the mentioned website. As of now, the most recent version is prometheus-2.48.0.darwin-amd64.tar.gz. Please replace it accordingly, and keep an eye out for any new version updates.

### **Download Prometheus:**

```
wget https://github.com/prometheus/prometheus/releases/download/v2.48.0/prometheus-2.48.0.linux-amd64.tar.gz
```

### **Extract Prometheus:**

```
tar xzf prometheus-2.48.0.linux-amd64.tar.gz
```

### **Move Prometheus files:**

```
mv prometheus-2.48.0.linux-amd64 /etc/prometheus
```

### **Edit systemd service file:**

```
nano /etc/systemd/system/prometheus.service
```

### **Copy & Past the Content of prometheus.service**

```console
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target
[Service]
ExecStart=/etc/prometheus/prometheus --config.file=/etc/prometheus/prometheus.yml
Restart=always
[Install]
WantedBy=multi-user.target
```
Press Ctrl + X.
Confirm saving by pressing Y.
Press Enter to accept the current filename.

### **Prometheus Systemd Commands (Reload, Restart, Enable, Status)**
Reloads systemd to pick up the changes made to unit files, then restarts the Prometheus service, enables Prometheus to start on boot, and finally, checks the status of the Prometheus service.
```
systemctl daemon-reload
systemctl restart prometheus
systemctl enable prometheus
systemctl status prometheus
```

### **Display Network Interfaces:**

```
ip a
```

Next, make sure the port 9090 is open on your VPS server:

```
ufw allow 9090/tcp
sudo ufw allow 9090
```
Now, access the Prometheus web interface by navigating to 

> http://your-server-ip-address:9090/. 

Make sure to replace '`Your-Server-IP-Address`' with the actual IP address of your server.


> [Node Exporter Download Files Here](https://prometheus.io/download/#node_exporter)"

Make sure to update your scripts in line with the latest version OF Node Exporter from the mentioned website. 
As of now, the most recent version is node_exporter-1.7.0.darwin-amd64.tar.gz. Please replace it accordingly, and keep an eye out for any new version updates.

### **Download Node Exporter:**

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
```

### **Extract Node Exporter:**

```
tar xzf node_exporter-1.7.0.linux-amd64.tar.gz
```

### **Move Node Exporter files:**

```
mv node_exporter-1.7.0.linux-amd64 /etc/node_exporter
```

### **Edit systemd service file:**

```
nano /etc/systemd/system/node_exporter.service
```

### **Copy & Past the Content of node_exporter.service**

```console
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/etc/node_exporter/node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
```

Press Ctrl + X.
Confirm saving by pressing Y.
Press Enter to accept the current filename.

### **Node Exporter Systemd Commands (Reload, Restart, Enable, Status)**
Reloads systemd to pick up the changes made to unit files, then restarts the Node Exporte service, enables Node Exporte to start on boot, and finally, checks the status of the Node Exporte service.

```
systemctl daemon-reload
systemctl restart node_exporter
systemctl enable node_exporter
systemctl status node_exporter
```

### **Display Network Interfaces:**

```
ip a
```

### **Remove Existing Prometheus Configuration:**
Deletes the existing Prometheus configuration file (prometheus.yml).

```
sudo rm -rf /etc/prometheus/prometheus.yml
```

### **Edit New Prometheus Configuration:**
Opens the nano text editor to create/edit the new Prometheus configuration file.

```
sudo nano /etc/prometheus/prometheus.yml
```

### **Copy & Past the Content of prometheus.yml:**
Replace "Your-Server-IP-Address" with the actual IP address of your server.

```console
global:
  scrape_interval: 15s

scrape_configs:
- job_name: node
  static_configs:
  - targets: ['Your-Server-IP-Address:9100']
```

### **Prometheus Systemd Commands (Restart, Enable, Status)**
Restart Prometheus to apply configuration changes, set it for automatic startup on system boot, and verify service status for errors.

```
systemctl daemon-reload
systemctl restart prometheus
systemctl enable prometheus
systemctl status prometheus
```

__________________________________________________________________________________________
## **Step 02 - Installing Grafana**

> [Grafan Commands & Script](https://grafana.com/grafana/download?platform=linux)

At first, ensure your scripts align with the latest version from the mentioned website. The current package version is 10.2.2 for AMD64. Please visit the website and obtain the latest scripts.

### **Install Dependencies:**
Installs required dependencies for Grafana.

```
sudo apt-get install -y adduser libfontconfig1 musl
```

### **Download Grafana Enterprise Package:**
Downloads the Grafana Enterprise package version 10.2.2 for AMD64 architecture

```
wget https://dl.grafana.com/enterprise/release/grafana-enterprise_10.2.2_amd64.deb
```

### **Install Grafana Enterprise Package:**
Installs Grafana Enterprise using the Debian package manager (dpkg).

```
sudo dpkg -i grafana-enterprise_10.2.2_amd64.deb
```

### **Grafana Systemd Commands (Restart, Enable, Status)**
Restart Grafana to apply configuration changes, set it for automatic startup on system boot, and verify service status for errors.

```
systemctl restart grafana-server
systemctl enable grafana-server
systemctl status grafana-server
```

Next, make sure the port 3000 is open on your VPS server:

```
ufw allow 3000/tcp
sudo ufw allow 3000
```
Now, access the Grafana web interface and control panel by navigating to http://your-server-ip-address:3000/.
Make sure to replace `'Your-Server-IP-Address'` with the actual IP address of your server.


__________________________________________________________________________________________

