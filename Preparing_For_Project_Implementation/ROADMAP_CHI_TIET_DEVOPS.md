# ROADMAP CHI TIẾT CHO DEVOPS ENGINEER
## Call Center SaaS Platform - DevOps Detailed Roadmap

> [!IMPORTANT]
> Roadmap chi tiết cho vị trí DevOps Engineer trong dự án Call Center SaaS Platform. Bao gồm công việc từng tuần, commands, và infrastructure best practices.

**Phiên bản:** 1.0  
**Ngày tạo:** 15/01/2026  
**Thời gian:** 12 tuần (6 sprints)  
**Tech Stack:** Debian 12, Docker, Nginx, PostgreSQL, Redis, FreeSWITCH, MinIO, Prometheus, Grafana

---

## 📋 MỤC LỤC

1. [Tổng quan vai trò DevOps](#1-tổng-quan-vai-trò-devops)
2. [Infrastructure Overview](#2-infrastructure-overview)
3. [Roadmap chi tiết từng tuần](#3-roadmap-chi-tiết-từng-tuần)
4. [Configuration Examples](#4-configuration-examples)
5. [Best Practices](#5-best-practices)
6. [Troubleshooting](#6-troubleshooting)

---

## 1. TỔNG QUAN VAI TRÒ DEVOPS

### 1.1. Trách nhiệm chính

- Infrastructure setup & maintenance
- FreeSWITCH installation & configuration
- CI/CD pipeline
- Monitoring & logging
- Security hardening
- Backup & disaster recovery
- Performance tuning
- Production deployment

### 1.2. Deliverables chính

| Deliverable | Deadline |
|-------------|----------|
| **3 Servers Setup** | Week 1 |
| **FreeSWITCH Operational** | Week 1 |
| **Database Setup** | Week 1 |
| **CI/CD Pipeline** | Week 2 |
| **SIP Trunk Configuration** | Week 4 |
| **Monitoring Dashboards** | Week 2 |
| **SSL/HTTPS** | Week 3 |
| **Load Testing** | Week 11 |
| **Production Deployment** | Week 12 |

---

## 2. INFRASTRUCTURE OVERVIEW

### 2.1. 3-Server Architecture

```
Server 1: Application Server (8 vCPU, 16GB RAM, 200GB SSD)
├── Next.js 15 (Frontend)
├── .NET 10 API (Backend)
├── PostgreSQL 16 (Database)
├── Redis 7 (Cache)
├── RabbitMQ (Message Queue)
└── Nginx (Reverse Proxy)

Server 2: Media Server (8 vCPU, 16GB RAM, 100GB SSD)
└── FreeSWITCH (Telephony Engine)

Server 3: Storage Server (4 vCPU, 8GB RAM, 1TB HDD)
└── MinIO (S3-compatible Storage)
```

### 2.2. Network Configuration

```
Public IPs:
- Server 1: 203.0.113.10 (Application)
- Server 2: 203.0.113.20 (FreeSWITCH)
- Server 3: 203.0.113.30 (Storage)

Private Network: 10.0.0.0/24
- Server 1: 10.0.0.10
- Server 2: 10.0.0.20
- Server 3: 10.0.0.30

Firewall Rules:
Server 1:
- 22 (SSH) - Restricted to admin IPs
- 80, 443 (HTTP/HTTPS) - Public
- 5432 (PostgreSQL) - Internal only
- 6379 (Redis) - Internal only

Server 2:
- 22 (SSH) - Restricted
- 5060 (SIP) - Public
- 8021 (ESL) - Internal only
- 8082 (WebSocket) - Public

Server 3:
- 22 (SSH) - Restricted
- 9000 (MinIO API) - Internal only
- 9001 (MinIO Console) - Restricted
```

---

## 3. ROADMAP CHI TIẾT TỪNG TUẦN

### **TUẦN 0: Preparation**

#### **Học FreeSWITCH (40h)**

**Ngày 1-2: FreeSWITCH Basics (16h)**
- [ ] Đọc [00_TONG_QUAN_FREESWITCH.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/FreeSwitchs/00_TONG_QUAN_FREESWITCH.md)
- [ ] Hiểu SIP protocol
- [ ] Hiểu FreeSWITCH architecture
- [ ] Hiểu modules system

**Ngày 3-4: Installation (16h)**
- [ ] Đọc [NGAY_01_02_CAI_DAT_FREESWITCH.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/FreeSwitchs/NGAY_01_02_CAI_DAT_FREESWITCH.md)
- [ ] Practice installation on test VM
- [ ] Test SIP registration
- [ ] Test basic call

**Ngày 5: Configuration (8h)**
- [ ] Đọc [NGAY_03_04_SIP_EXTENSIONS.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/FreeSwitchs/NGAY_03_04_SIP_EXTENSIONS.md)
- [ ] Understand SIP profiles
- [ ] Understand dialplan

---

### **SPRINT 1: Infrastructure Setup (Tuần 1-2)**

#### **Tuần 1: Server Setup & FreeSWITCH**

**Ngày 1: VPS Setup (8h)**

```bash
# Server 1: Application Server
# Login as root
ssh root@203.0.113.10

# Update system
apt update && apt upgrade -y

# Set hostname
hostnamectl set-hostname app-server

# Configure timezone
timedatectl set-timezone Asia/Ho_Chi_Minh

# Install basic tools
apt install -y curl wget git vim htop net-tools

# Create non-root user
adduser deploy
usermod -aG sudo deploy

# Configure SSH
nano /etc/ssh/sshd_config
# PermitRootLogin no
# PasswordAuthentication no
systemctl restart sshd

# Repeat for Server 2 & 3
```

**Ngày 2: Firewall Configuration (8h)**

```bash
# Install UFW
apt install -y ufw

# Server 1: Application Server
ufw default deny incoming
ufw default allow outgoing
ufw allow 22/tcp    # SSH
ufw allow 80/tcp    # HTTP
ufw allow 443/tcp   # HTTPS
ufw allow from 10.0.0.0/24 to any port 5432  # PostgreSQL internal
ufw allow from 10.0.0.0/24 to any port 6379  # Redis internal
ufw enable

# Server 2: FreeSWITCH
ufw allow 22/tcp
ufw allow 5060/udp  # SIP
ufw allow 5060/tcp  # SIP TCP
ufw allow 8082/tcp  # WebSocket
ufw allow from 10.0.0.0/24 to any port 8021  # ESL internal
ufw allow 16384:32768/udp  # RTP
ufw enable

# Server 3: MinIO
ufw allow 22/tcp
ufw allow from 10.0.0.0/24 to any port 9000  # MinIO API
ufw allow from YOUR_IP to any port 9001      # MinIO Console
ufw enable
```

**Ngày 3-4: FreeSWITCH Installation (16h)**

```bash
# Server 2: FreeSWITCH
ssh deploy@203.0.113.20

# Install dependencies
apt install -y build-essential cmake automake autoconf \
  libtool pkg-config libssl-dev zlib1g-dev libdb-dev \
  unixodbc-dev libncurses5-dev libexpat1-dev libgdbm-dev \
  bison erlang-dev libtpl-dev libtiff5-dev uuid-dev \
  libpcre3-dev libedit-dev libsqlite3-dev libcurl4-openssl-dev \
  nasm yasm libopus-dev libsndfile1-dev libldns-dev \
  libspeex-dev libspeexdsp-dev libedit-dev liblua5.2-dev \
  libavformat-dev libswscale-dev libavresample-dev

# Clone FreeSWITCH
cd /usr/src
git clone https://github.com/signalwire/freeswitch.git -b v1.10
cd freeswitch

# Bootstrap
./bootstrap.sh -j

# Configure modules
nano modules.conf
# Enable: mod_xml_curl, mod_callcenter, mod_verto, mod_sofia
# Disable: mod_signalwire (if not needed)

# Configure
./configure --enable-core-pgsql-support

# Compile (takes 30-60 minutes)
make -j$(nproc)

# Install
make install
make cd-sounds-install cd-moh-install

# Create systemd service
nano /etc/systemd/system/freeswitch.service
```

**FreeSWITCH systemd service:**

```ini
[Unit]
Description=FreeSWITCH
After=network.target

[Service]
Type=forking
PIDFile=/var/run/freeswitch/freeswitch.pid
Environment="DAEMON_OPTS=-nonat"
EnvironmentFile=-/etc/default/freeswitch
ExecStart=/usr/local/freeswitch/bin/freeswitch -u freeswitch -g freeswitch -ncwait $DAEMON_OPTS
TimeoutSec=45s
Restart=always
WorkingDirectory=/usr/local/freeswitch
User=freeswitch
Group=freeswitch

[Install]
WantedBy=multi-user.target
```

```bash
# Create freeswitch user
adduser --disabled-password --quiet --system --home /usr/local/freeswitch --gecos "FreeSWITCH" --ingroup daemon freeswitch
chown -R freeswitch:daemon /usr/local/freeswitch

# Enable and start
systemctl daemon-reload
systemctl enable freeswitch
systemctl start freeswitch
systemctl status freeswitch

# Test
fs_cli
# Should connect to FreeSWITCH console
```

**Ngày 5: Database Setup (8h)**

```bash
# Server 1: Application Server
ssh deploy@203.0.113.10

# Install PostgreSQL 16
apt install -y postgresql-16 postgresql-contrib-16

# Configure PostgreSQL
nano /etc/postgresql/16/main/postgresql.conf
# listen_addresses = '10.0.0.10'
# max_connections = 200

nano /etc/postgresql/16/main/pg_hba.conf
# host    all    all    10.0.0.0/24    md5

# Restart PostgreSQL
systemctl restart postgresql

# Create database and user
sudo -u postgres psql
CREATE DATABASE callcenter_saas;
CREATE USER callcenter WITH PASSWORD 'your_secure_password';
GRANT ALL PRIVILEGES ON DATABASE callcenter_saas TO callcenter;
\q

# Install Redis
apt install -y redis-server

# Configure Redis
nano /etc/redis/redis.conf
# bind 10.0.0.10
# requirepass your_redis_password

systemctl restart redis-server
```

#### **Tuần 2: CI/CD & Monitoring**

**Ngày 1-2: Docker Setup (16h)**

```bash
# Server 1: Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Install Docker Compose
curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

# Add deploy user to docker group
usermod -aG docker deploy

# Test
docker --version
docker-compose --version
```

**Ngày 3: CI/CD Pipeline (8h)**

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '10.0.x'
      
      - name: Build
        run: dotnet build --configuration Release
      
      - name: Test
        run: dotnet test
      
      - name: Publish
        run: dotnet publish -c Release -o ./publish
      
      - name: Deploy to Server
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          source: "./publish/*"
          target: "/var/www/api"
      
      - name: Restart Service
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: sudo systemctl restart callcenter-api

  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build
        run: npm run build
      
      - name: Deploy
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          source: ".next/*"
          target: "/var/www/frontend"
```

**Ngày 4-5: Monitoring Setup (16h)**

```bash
# Install Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.48.0/prometheus-2.48.0.linux-amd64.tar.gz
tar xvfz prometheus-*.tar.gz
cd prometheus-*
sudo mv prometheus promtool /usr/local/bin/
sudo mkdir /etc/prometheus /var/lib/prometheus
sudo mv consoles console_libraries prometheus.yml /etc/prometheus

# Configure Prometheus
sudo nano /etc/prometheus/prometheus.yml
```

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
  
  - job_name: 'postgres'
    static_configs:
      - targets: ['localhost:9187']
  
  - job_name: 'nginx'
    static_configs:
      - targets: ['localhost:9113']
```

```bash
# Create systemd service
sudo nano /etc/systemd/system/prometheus.service
```

```ini
[Unit]
Description=Prometheus
After=network.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/

[Install]
WantedBy=multi-user.target
```

```bash
# Install Grafana
sudo apt-get install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt-get update
sudo apt-get install grafana

# Start services
sudo systemctl enable prometheus grafana-server
sudo systemctl start prometheus grafana-server

# Access Grafana: http://server-ip:3000
# Default: admin/admin
```

**Deliverables Tuần 1-2:**
- ✅ 3 servers setup complete
- ✅ FreeSWITCH operational
- ✅ PostgreSQL + Redis running
- ✅ CI/CD pipeline configured
- ✅ Monitoring dashboards

---

### **SPRINT 2-6: Continued Operations**

**Sprint 2 (Tuần 3-4): SIP Trunk & SSL**
- SIP Trunk registration
- SSL certificate (Let's Encrypt)
- Nginx reverse proxy
- Network optimization

**Sprint 3 (Tuần 5-6): MinIO & Recording**
- MinIO installation
- Bucket configuration
- Recording path setup
- Lifecycle policies

**Sprint 4 (Tuần 7-8): Load Balancing**
- Nginx load balancer
- Health checks
- WebSocket proxy
- Redis pub/sub

**Sprint 5 (Tuần 9-10): WebRTC & TURN**
- STUN/TURN server
- WebSocket SSL
- Codec optimization
- NAT traversal

**Sprint 6 (Tuần 11-12): Production Launch**
- Load testing
- Security audit
- Backup automation
- Production deployment

---

## 4. CONFIGURATION EXAMPLES

### 4.1. Nginx Configuration

```nginx
# /etc/nginx/sites-available/callcenter-saas
upstream backend_api {
    server 127.0.0.1:5000;
}

upstream frontend {
    server 127.0.0.1:3000;
}

server {
    listen 80;
    server_name api.callcenter-saas.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name api.callcenter-saas.com;

    ssl_certificate /etc/letsencrypt/live/api.callcenter-saas.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/api.callcenter-saas.com/privkey.pem;

    location / {
        proxy_pass http://backend_api;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /hub {
        proxy_pass http://backend_api;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

### 4.2. FreeSWITCH mod_xml_curl

```xml
<!-- /usr/local/freeswitch/conf/autoload_configs/xml_curl.conf.xml -->
<configuration name="xml_curl.conf" description="cURL XML Gateway">
  <bindings>
    <binding name="directory">
      <param name="gateway-url" value="http://10.0.0.10:5000/api/freeswitch/configuration" bindings="directory"/>
      <param name="timeout" value="5"/>
    </binding>
    <binding name="dialplan">
      <param name="gateway-url" value="http://10.0.0.10:5000/api/freeswitch/configuration" bindings="dialplan"/>
      <param name="timeout" value="5"/>
    </binding>
  </bindings>
</configuration>
```

---

## 5. BEST PRACTICES

### 5.1. Security

- ✅ Use SSH keys, disable password auth
- ✅ Configure firewall (UFW)
- ✅ Use strong passwords
- ✅ Regular security updates
- ✅ Fail2Ban for SSH protection
- ✅ SSL/TLS everywhere

### 5.2. Monitoring

- ✅ Monitor CPU, RAM, Disk
- ✅ Monitor network traffic
- ✅ Monitor application logs
- ✅ Set up alerts
- ✅ Regular health checks

### 5.3. Backup

- ✅ Daily database backups
- ✅ Weekly full system backups
- ✅ Test restore procedures
- ✅ Off-site backup storage
- ✅ Backup retention policy

---

## 6. TROUBLESHOOTING

### 6.1. FreeSWITCH Issues

**Problem: SIP registration fails**
```bash
# Check FreeSWITCH logs
tail -f /usr/local/freeswitch/log/freeswitch.log

# Check SIP profile status
fs_cli -x "sofia status"

# Check if mod_xml_curl is responding
curl -X POST http://10.0.0.10:5000/api/freeswitch/configuration \
  -d "section=directory&user=101&domain=tenant.pbx.local"
```

**Problem: No audio in calls**
```bash
# Check RTP ports
netstat -tulpn | grep freeswitch

# Check firewall
ufw status

# Check codec negotiation
fs_cli -x "show codec"
```

### 6.2. Database Issues

**Problem: Connection refused**
```bash
# Check PostgreSQL status
systemctl status postgresql

# Check if listening on correct IP
netstat -tulpn | grep 5432

# Check pg_hba.conf
cat /etc/postgresql/16/main/pg_hba.conf
```

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0  
**Next Review:** Sprint 1 Planning
