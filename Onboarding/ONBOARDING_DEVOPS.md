# ONBOARDING - DEVOPS ENGINEER
## Call Center SaaS Platform - DevOps Onboarding Guide

> [!IMPORTANT]
> Chào mừng bạn đến với vị trí DevOps! Tài liệu này sẽ hướng dẫn bạn về infrastructure, deployment, và monitoring.

**Vai trò:** DevOps Engineer  
**Ngày bắt đầu:** [Điền ngày]  
**Mentor:** [Tên DevOps Lead]  
**Infrastructure:** 3-server architecture, Debian 12, Docker, FreeSWITCH

---

##  TUẦN ĐẦU TIÊN

### Ngày 1: Infrastructure Overview

#### **Morning: Access & Credentials**

**Get Access To:**
- [ ] Server 1 (Application) - SSH access
- [ ] Server 2 (FreeSWITCH) - SSH access
- [ ] Server 3 (MinIO) - SSH access
- [ ] GitHub repository - Admin access
- [ ] Domain registrar
- [ ] Cloud provider dashboard

**SSH Setup:**
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C 'your_email@example.com'

# Add to servers
ssh-copy-id deploy@server1-ip
ssh-copy-id deploy@server2-ip
ssh-copy-id deploy@server3-ip

# Test connection
ssh deploy@server1-ip
```

#### **Afternoon: Infrastructure Walkthrough**

**3-Server Architecture:**
```
Server 1: Application (203.0.113.10)
 Next.js Frontend (Port 3000)
 .NET API (Port 5000)
 PostgreSQL (Port 5432)
 Redis (Port 6379)
 Nginx (Port 80/443)

Server 2: FreeSWITCH (203.0.113.20)
 FreeSWITCH (Port 5060, 8021)
 WebSocket (Port 8082)

Server 3: MinIO (203.0.113.30)
 MinIO S3 (Port 9000, 9001)
```

**Review:**
- [ ] Network topology
- [ ] Firewall rules
- [ ] SSL certificates
- [ ] Backup strategy

---

### Ngày 2: FreeSWITCH Deep Dive

**Learn FreeSWITCH:**
- [ ] [00_TONG_QUAN_FREESWITCH.md](../FreeSwitchs/00_TONG_QUAN_FREESWITCH.md)
- [ ] [NGAY_01_02_CAI_DAT_FREESWITCH.md](../FreeSwitchs/NGAY_01_02_CAI_DAT_FREESWITCH.md)

**Check FreeSWITCH Status:**
```bash
# SSH to Server 2
ssh deploy@server2-ip

# Check FreeSWITCH status
systemctl status freeswitch

# Connect to fs_cli
fs_cli

# Check SIP registrations
sofia status

# Check active calls
show calls
```

---

### Ngày 3: Monitoring & Logging

**Prometheus & Grafana:**
```bash
# Access Grafana
http://server1-ip:3000
# Login: admin/admin

# Check dashboards:
# - System Metrics
# - PostgreSQL Metrics
# - Nginx Metrics
# - FreeSWITCH Metrics
```

**Logs:**
```bash
# Application logs
journalctl -u callcenter-api -f

# FreeSWITCH logs
tail -f /usr/local/freeswitch/log/freeswitch.log

# Nginx logs
tail -f /var/log/nginx/access.log
```

---

### Ngày 4-5: First Deployment

**Deploy Backend Update:**
```bash
# Pull latest code
cd /var/www/api
git pull origin main

# Build
dotnet publish -c Release -o ./publish

# Restart service
sudo systemctl restart callcenter-api

# Check status
sudo systemctl status callcenter-api

# Check logs
journalctl -u callcenter-api -n 50
```

**Deliverables Week 1:**
-  Access to all servers
-  Understand infrastructure
-  Can deploy updates
-  Can check logs

---

##  TÀI LIỆU HỌC TẬP

### Bắt buộc đọc
1. [01_HA_TANG_VA_NHAN_SU.md](../Project_Documents/01_HA_TANG_VA_NHAN_SU.md)
2. [ROADMAP_CHI_TIET_DEVOPS.md](../Preparing_For_Project_Implementation/ROADMAP_CHI_TIET_DEVOPS.md)
3. [FreeSWITCH Documentation](https://freeswitch.org/confluence/)

---

##  COMMON TASKS

### Restart Services
```bash
# API
sudo systemctl restart callcenter-api

# Frontend
sudo systemctl restart callcenter-frontend

# FreeSWITCH
sudo systemctl restart freeswitch

# Nginx
sudo systemctl restart nginx
```

### Check Disk Space
```bash
df -h
```

### Database Backup
```bash
pg_dump -U callcenter callcenter_prod > backup_.sql
```

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0

> [!TIP]
> Always test in staging before deploying to production!
