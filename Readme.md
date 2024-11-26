# Home Server 🐳

Welcome to the repository containing all the Docker Compose configurations for my Raspberry Pi 4-powered home server. This setup is designed to maximize utility, efficiency, and privacy using various self-hosted applications.

---

## 📋 System Overview

| **System Info**        | **Details**                   |
|-------------------------|-------------------------------|
| **OS**                 | Debian 12 (Bookworm)          |
| **Hardware**           | Raspberry Pi 4 Model B        |
| **Memory**             | 4GB RAM                       |
| **RAID Type**          | RAID1                         |


## 🔧 Hardware Overview

| **Component**           | **Model/Details**                                   |
|--------------------------|----------------------------------------------------|
| **Primary Disk**         | Samsung SSD 860 QVO 1TB                            |
| **Secondary Disk**       | Toshiba External USB 3.0 1TB                       |
| **OS Storage**           | SD Card (128GB)                                   |
| **Other Hardware**       | Case, charger, and USB hub info coming soon.       |

---

## 🛠️ Applications

Here is a list of the self-hosted applications running on this server:

| **Application**   | **Purpose**                                                                                   |
|--------------------|-----------------------------------------------------------------------------------------------|
| **Nextcloud**      | Personal cloud storage and file-sharing platform.                                            |
| **Immich**         | A self-hosted media backup solution, perfect for managing your photos and videos.            |
| **Cloudflare Tunnels** | Securely expose your local server to the internet using Cloudflare’s tunnel service.     |
| **PiVPN**          | VPN service using WireGuard, providing secure remote access to your home network.            |
| **Uptime Kuma**    | Monitoring service to track uptime and the health of applications and devices.               |
| **Cockpit**        | Web-based interface for server management, making it easy to monitor and control resources.  |

## 🖥️ Container Management

This server uses [`louislam/dockge`](https://github.com/louislam/dockge), a fancy, easy-to-use, and reactive self-hosted Docker Compose stack-oriented manager, to manage and deploy containers efficiently.

---

## 📚 Tutorials Followed

I followed several excellent guides to set up various components of this server. Here are the resources:

| **Setup**          | **Guide**                                                                                  |
|---------------------|--------------------------------------------------------------------------------------------|
| **Cockpit**         | [How to Install Cockpit](https://pimylifeup.com/raspberry-pi-cockpit/)                     |
| **WireGuard VPN**   | [WireGuard Setup Guide](https://pimylifeup.com/raspberry-pi-wireguard/)                    |
| **Static IP**       | [Assigning a Static IP Address](https://pimylifeup.com/raspberry-pi-static-ip-address/)    |
| **RAID Setup**      | [YouTube Tutorial](https://www.youtube.com/watch?v=Y_l1BCCqZSQ)                           |
| **RAID Alerts**     | Custom script for RAID monitoring and alerting (steps below).                             |

---

## 🚨 RAID Alert Setup

1. **Create the alert script**:
   ```bash
   sudo nano /usr/local/bin/raid_alert.sh
   ```

2. **Add the following content:**
    ```bash
    #!/bin/bash

    # Discord webhook URL
    WEBHOOK_URL="https://discord.com/api/webhooks/YOUR_WEBHOOK_URL"

    # Message content
    MESSAGE="🚨 **RAID Alert**: A drive has failed in your RAID array on $(hostname)! Check immediately. 🚨"

    # Send the message
    curl -H "Content-Type: application/json" -X POST -d "{\"content\": \"$MESSAGE\"}" $WEBHOOK_URL
    ```
       
3. **Save and exit. Make the script executable:**
    ```bash
    sudo chmod +x /usr/local/bin/raid_alert.sh
    ```

4. **Update the RAID monitoring configuration:**
    ```bash
    sudo nano /etc/mdadm/mdadm.conf
    ```

5. **Add the following line:**
    ```bash
    PROGRAM /usr/local/bin/raid_alert.sh
    ```

6. **Restart the RAID monitoring service:**
    ```bash
    sudo systemctl restart mdmonitor
    ```
---

# 🎉 Acknowledgments

A big thanks to the creators of the guides and tutorials listed above. Their work made this setup possible!
