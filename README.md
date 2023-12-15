# **Installing Prometheus & Grafana Monitoring on Ubuntu VPS**

__________________________________________________________________________________________
## **Step 01 - Installing Prometheus**


> [Prometheus Download Files Here](https://prometheus.io/download/)

At first, make sure to update your scripts in line with the latest version from the mentioned website. As of now, the most recent version is `prometheus-2.49.0-rc.0.linux-amd64.tar.gz`. Please replace it accordingly, and keep an eye out for any new version updates.

### **Download Prometheus:**

```
wget https://github.com/prometheus/prometheus/releases/download/v2.49.0-rc.0/prometheus-2.49.0-rc.0.linux-amd64.tar.gz
```

### **Extract Prometheus:**

```
tar xzf prometheus-2.49.0-rc.0.linux-amd64.tar.gz
```

### **Move Prometheus files:**

```
mv prometheus-2.49.0-rc.0.linux-amd64 /etc/prometheus
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
"Press `Q` to exit status mode.

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

![01 copy](https://github.com/iranxray/hope/assets/127796122/6fa891ee-92c1-430b-9598-d7cf696af107)


Navigate to the top of the website, click on `Status` in the navigation bar, and then select `Targets`. You should see something similar to the image below.

![02](https://github.com/iranxray/hope/assets/127796122/f44ae611-690c-4f3b-98d7-31412e77079f)

Next **Node Exporter**

Ensure your scripts are updated to the latest version of Node Exporter from the website below. The current version is node_exporter-1.7.0.darwin-amd64.tar.gz. Please replace it accordingly and stay informed about any new version updates.

> [Node Exporter Download Files Here](https://prometheus.io/download/#node_exporter)"

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

"Press `Q` to exit status mode.

### **Display Network Interfaces:**

```
ip a
```

Now, refresh the Prometheus web interface at http://your-server-ip-address:9090/. Navigate to the top of the website, click on 'Status' in the navigation bar, and then select 'Targets.' Ensure the instance value reflects your server IP address, and the Job should change to 'node,' as illustrated in the image below.

![03](https://github.com/iranxray/hope/assets/127796122/c10d5519-25f0-4669-a67a-7c64142b54f3)


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

"Press `Q` to exit status mode.

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

"Press `Q` to exit status mode.

Next, make sure the port 3000 is open on your VPS server:

```
ufw allow 3000/tcp
sudo ufw allow 3000
```
Now, access the Grafana web interface and control panel by navigating to http://your-server-ip-address:3000/.
Make sure to replace `'Your-Server-IP-Address'` with the actual IP address of your server.
__________________________________________________________________________________________

## **Step 03 - Set up the Grafana Panel**
Please follow the image instructions below to set up your panel for correct monitoring of your server

To log in, use the default username and password, which are both`admin` Afterward, you can choose a new, strong password.

![Screenshot 2023-12-06 at 09 03 48 copy](https://github.com/iranxray/hope/assets/127796122/91d24a2d-a25e-4825-a296-15ba6d1177c9)

Next, go to the Home hamburger menu on the top left, open the dropdown menu, and choose `Connections`.

![Screenshot 2023-12-06 at 09 06 55 copy](https://github.com/iranxray/hope/assets/127796122/cba9ac37-4af3-4e3b-937b-feac85e69824)

Then to add a new connection, search for 'Prometheus,' select it, and double-click to add.

![Screenshot 2023-12-06 at 09 07 54 copy](https://github.com/iranxray/hope/assets/127796122/4ebbc872-397c-40a5-a0a5-c6b96ec43e44)

Next, click the blue button on the top right corner, `Add New Data Source.`'In the connection field, change `localhost:9090` to `Your-Server-IP-Address:9090` as illustrated in the image below.

![Screenshot 2023-12-06 at 09 08 13](https://github.com/iranxray/hope/assets/127796122/74fbf74e-77bc-4eaf-8b37-e74b2d48ab68)

Then, scroll down to the bottom of the page and click on the `Save and Test` button. You should see the message `Successfully queried the Prometheus API`


![Screenshot 2023-12-06 at 09 10 16 copy](https://github.com/iranxray/hope/assets/127796122/6a84714c-903b-4d8e-b9f6-cc9df8e1ca50)

Next, go back to the home menu, select `Dashboard` click on the 'New' button on the top right, and choose `Import`
You will be directed to the import dashboard section, as illustrated in the image below

![Screenshot 2023-12-06 at 09 11 10 copy](https://github.com/iranxray/hope/assets/127796122/f6a3b3b3-656f-4265-88e4-5e5b95132ad7)
![Screenshot 2023-12-06 at 09 11 29 copy](https://github.com/iranxray/hope/assets/127796122/b325971e-d13b-449a-834e-bec9fa4d4697)

In this section, go to the `Find and import dashboard` field, enter the number `1860` and press the `Load` button on the right

![Screenshot 2023-12-06 at 09 12 40 copy](https://github.com/iranxray/hope/assets/127796122/6a5c5e1e-b250-4417-98b5-2f2189fcba29)
![Screenshot 2023-12-06 at 09 12 54 copy](https://github.com/iranxray/hope/assets/127796122/8cc0860b-8664-455e-93b2-4fe693c372d1)

After loading the 1860 application, in the `Panel` section, find the `Prometheus` dropdown field at the bottom and choose `Prometheus` from the list. Then, import it.

![Screenshot 2023-12-06 at 09 13 34 copy](https://github.com/iranxray/hope/assets/127796122/2632fb79-858d-45cf-8cac-fc6332ce81d1)

As shown in the image below, the monitor controller will be uploaded. Please click on the `settings` icon on the top bar on the right and select the `link` from the settings menu on the left.

![Screenshot 2023-12-06 at 11 38 18 copy](https://github.com/iranxray/hope/assets/127796122/abaf7cea-70d4-48d7-8fc4-4ec008251ab7)
![Screenshot 2023-12-06 at 09 14 38 copy](https://github.com/iranxray/hope/assets/127796122/3eaca084-1f6b-40a5-bc0a-a38169af0023)

In the link section, click on the `X` to delete the default links from GitHub and Grafana, as illustrated in the image.
Finally, click on the `Save Dashboard` button on the top right, and you are almost done setting up your panel.

![Screenshot 2023-12-06 at 09 15 08 copy](https://github.com/iranxray/hope/assets/127796122/d87dd04c-5b8e-4e4e-9567-b37caaeef5e4)

Now, in the final step, go back to the Home hamburger menu on the top left, open the dropdown menu, and choose `Dashboard` You will see `Node Exporter Full` and by clicking on that, you should have full access to your Grafana monitor controller.

![Screenshot 2023-12-06 at 09 16 49 copy](https://github.com/iranxray/hope/assets/127796122/1cfeeb93-0861-4610-a112-f3676695f701)
![Screenshot 2023-12-06 at 09 17 04 copy](https://github.com/iranxray/hope/assets/127796122/e0d85cbd-8b05-4c13-8280-228753cccde9)

Now, the choice is yours. Explore the list and select what you'd like to monitor.

![Screenshot 2023-12-06 at 09 17 51 copy](https://github.com/iranxray/hope/assets/127796122/1a2ec654-b2f0-415f-a76c-bde1513e2fcb)

__________________________________________________________________________________________
