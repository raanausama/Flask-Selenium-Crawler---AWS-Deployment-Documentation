# 🕷️ Flask Selenium Crawler - AWS Deployment

A production-ready **Flask + Selenium crawler** deployed on **AWS EC2** with **Nginx, Gunicorn, and systemd**.  
This project demonstrates how to run a scalable crawler that uses Selenium + Chrome in a headless environment, served via a REST API.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Directory Structure](#directory-structure)
- [Deployment & Service Management](#deployment--service-management)
- [Logs & Monitoring](#logs--monitoring)
- [API Usage](#api-usage)
- [Security & Backup](#security--backup)
- [Troubleshooting](#troubleshooting)
- [Maintenance](#maintenance)
- [Emergency Recovery](#emergency-recovery)
- [Support](#support)
- [Resources](#resources)

---

## 📖 Overview
This crawler is designed to extract product and material options from supported furniture and design websites.  
It is deployed on an **AWS EC2 instance (Ubuntu 22.04 LTS)** and made publicly available through **Nginx reverse proxy**.

---

## 🏗️ Architecture
<pre> 
AWS EC2 Instance (Ubuntu 22.04)
│
├── Nginx (Port 80) - Reverse Proxy
│
├── Gunicorn (Port 5000) - WSGI Server
│
├── Flask Application (app.py + selenium_utils.py)
│
└── Selenium WebDriver
├── Google Chrome 140.0.7339.207
└── ChromeDriver 140.0.7339.207
 </pre>


**Process Management:** systemd  
**Service Name:** `flask-crawler.service`

---

## ⚙️ Tech Stack
- **OS**: Ubuntu 22.04 LTS  
- **Framework**: Flask 3.1.2  
- **Web Server**: Nginx  
- **WSGI**: Gunicorn  
- **Automation**: Selenium 4.35.0  
- **Browser**: Chrome 140.0  
- **Driver**: ChromeDriver 140.0  
- **Python**: 3.12  

---

## 📂 Directory Structure

<pre>
/root/crawler/crawler-for-binnen/
├── app.py                  # Main Flask application
├── selenium_utils.py       # Selenium helper functions
├── requirements.txt        # Python dependencies
├── gunicorn_config.py      # Gunicorn configuration
├── venv/                   # Python virtual environment
│   ├── bin/
│   │   ├── python
│   │   ├── gunicorn
│   │   └── pip
│   └── lib/
├── *.json                  # Output data files
└── selenium_crawler.log    # Application logs </pre> 



---

## 🚀 Deployment & Service Management

### Systemd Service

# Check service status
<pre>
sudo systemctl status flask-crawler
</pre>

# Start/Stop/Restart service
<pre>
sudo systemctl start flask-crawler
sudo systemctl stop flask-crawler
sudo systemctl restart flask-crawler 
</pre>

## Nginx

Test config
<pre>
sudo nginx -t
 </pre>

# Restart service
<pre>
sudo systemctl restart nginx
 </pre>

📊 Logs & Monitoring

# Watch live logs
<pre>sudo journalctl -u flask-crawler -f
 </pre>


# Last 100 lines
<pre>
sudo journalctl -u flask-crawler -n 100
 </pre>

# Application log
<pre>
tail -f /root/crawler/crawler-for-binnen/selenium_crawler.log
</pre>

📡 API Usage

Endpoint: /extract-product-data
Method: POST
Content-Type: application/json

Example Request

<pre>
{
  "url": "https://www.jori.com/en/products/sofas",
  "headless": true,
  "output_file": "jori_sofas.json"
}
 </pre>

Example Response

<pre>

{
  "success": true,
  "url": "https://www.jori.com/en/products/sofas",
  "output_file": "jori_sofas.json",
  "upholstery_options": 15,
  "materials_options": 8,
  "data": {
    "upholstery": [...],
    "materials": [...]
  }
}
</pre>

✅ #Supports websites like:
artifort.com, csrugs.com, designonstock.com, bertplantagie.com, eyye.nl, jori.com, leolux.nl, montis.nl, pastoe.com

🔒 #Security & Backup

- Firewall (UFW)
<pre>
sudo ufw allow 22/tcp   # SSH
sudo ufw allow 80/tcp   # HTTP
sudo ufw allow 443/tcp  # HTTPS
</pre>

- SSL Setup (Certbot)
<pre>
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
</pre>
- Backup Script
<pre>
~/backup-crawler.sh
 </pre>
 
## 🛠️ Troubleshooting
- **Service won’t start** → `sudo systemctl restart flask-crawler`  
- **Chrome crash** → `sudo pkill -9 chrome && sudo rm -rf /tmp/chrome-*`  
- **Port in use** → `sudo netstat -tlnp | grep 5000`  
- **Version mismatch** → Update both Chrome + ChromeDriver  
- **Out of memory** → Restart service or upgrade EC2 instance  

---

## 🔄 Maintenance
- **Daily** → Check logs, monitor disk/memory  
- **Weekly** → Clean temp files, validate Chrome/Driver versions  
- **Monthly** → Update dependencies, rotate backups, check SSL expiry  


🆘 Emergency Recovery

If everything fails:

# Stop service & kill processes
sudo systemctl stop flask-crawler
sudo pkill -9 chrome chromedriver gunicorn

# Clean temp files
<pre>
sudo rm -rf /tmp/chrome-* /root/.config/google-chrome /root/.cache/google-chrome
 </pre>

# Recreate virtual environment
<pre>
cd /root/crawler/crawler-for-binnen
python3 -m venv venv
pip install -r requirements.txt
 </pre>

# Restart service
<pre>
sudo systemctl restart flask-crawler
 </pre>

## 📞 Support
- **Server**: AWS EC2 (Ubuntu 22.04)  
- **Service**: `flask-crawler.service`  
- **Public Port**: 80 (via Nginx)  
- **Internal Port**: 5000 (Gunicorn/Flask) 

## 📚 Resources
- [Flask Docs](https://flask.palletsprojects.com/)  
- [Selenium Docs](https://selenium-python.readthedocs.io/)  
- [Gunicorn Docs](https://docs.gunicorn.org/)  
- [Nginx Docs](https://nginx.org/en/docs/)  
- [Systemd Docs](https://www.freedesktop.org/wiki/Software/systemd/)  
- [Chrome for Testing](https://googlechromelabs.github.io/chrome-for-testing/)  

🏆 # Maintainer

Maintained by: System Administrator
Last Updated: October 1, 2025


---

