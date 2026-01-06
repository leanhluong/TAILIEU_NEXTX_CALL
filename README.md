# DANH SÁCH TÀI LIỆU DỰ ÁN
## Call Center SaaS Platform - Document Index

> [!NOTE]
> Tài liệu này liệt kê tất cả các tài liệu trong dự án và mục đích của từng tài liệu

**Phiên bản:** 2.0  
**Ngày cập nhật:** 06/01/2026  
**Kiến trúc:** 3-Server Architecture (.NET 10 + Next.js 15)

---

## 📚 TÀI LIỆU CHÍNH

### 1. Tài liệu Kiến trúc & Tech Stack

| File | Mô tả | Trạng thái |
|------|-------|------------|
| [KIEN_TRUC_VA_TECH_STACK.md](./Project_Documents/KIEN_TRUC_VA_TECH_STACK.md) | **MỚI** - Kiến trúc 3-server chi tiết, Tech stack analysis (.NET 10, Next.js, PostgreSQL, RabbitMQ), So sánh frameworks, Roadmap scale | ✅ Hoàn thành |
| [KIEN_TRUC_CHI_TIET_VA_LUONG_GOI.md](./Project_Documents/KIEN_TRUC_CHI_TIET_VA_LUONG_GOI.md) | Luồng gọi điện chi tiết, Tương tác giữa các components | ⚠️ Cần cập nhật |

### 2. Tài liệu Dự án

| File | Mô tả | Trạng thái |
|------|-------|------------|
| [00_TONG_QUAN_HE_THONG.md](./Project_Documents/00_TONG_QUAN_HE_THONG.md) | Tổng quan hệ thống, Mục tiêu, Phạm vi | ⚠️ Cần cập nhật |
| [01_HA_TANG_VA_NHAN_SU.md](./Project_Documents/01_HA_TANG_VA_NHAN_SU.md) | Hạ tầng server, Nhân sự, Chi phí | ⚠️ Cần cập nhật |
| [02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md](./Project_Documents/02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md) | Software Requirements Specification | ✅ OK |
| [03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md](./Project_Documents/03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md) | System Design Specification, Database schema, API design | ✅ Đã cập nhật |
| [04_ROADMAP_VA_TIMELINE.md](./Project_Documents/04_ROADMAP_VA_TIMELINE.md) | Roadmap 12 tuần, Timeline chi tiết | ⚠️ Cần cập nhật |

### 3. Tài liệu FreeSWITCH

| File | Mô tả | Trạng thái |
|------|-------|------------|
| [README.md](./FreeSwitchs/README.md) | Danh mục tài liệu học FreeSWITCH | ✅ Hoàn thành |
| [00_TONG_QUAN_FREESWITCH.md](./FreeSwitchs/00_TONG_QUAN_FREESWITCH.md) | Giới thiệu FreeSWITCH, Kiến trúc | ✅ Hoàn thành |
| [NGAY_01_02_CAI_DAT_FREESWITCH.md](./FreeSwitchs/NGAY_01_02_CAI_DAT_FREESWITCH.md) | Cài đặt FreeSWITCH (16h) | ✅ Hoàn thành |
| [NGAY_03_04_SIP_EXTENSIONS.md](./FreeSwitchs/NGAY_03_04_SIP_EXTENSIONS.md) | SIP & Extensions (16h) | ✅ Hoàn thành |
| [NGAY_05_DIALPLAN_NANG_CAO.md](./FreeSwitchs/NGAY_05_DIALPLAN_NANG_CAO.md) | Dialplan nâng cao (8h) | ✅ Hoàn thành |
| [NGAY_06_07_MOD_XML_CURL.md](./FreeSwitchs/NGAY_06_07_MOD_XML_CURL.md) | mod_xml_curl - Tích hợp Backend (16h) | ✅ Hoàn thành |
| [NGAY_08_09_EVENT_SOCKET_LAYER.md](./FreeSwitchs/NGAY_08_09_EVENT_SOCKET_LAYER.md) | ESL - CDR & Billing (16h) | ✅ Hoàn thành |
| [TAI_LIEU_THAM_KHAO.md](./FreeSwitchs/TAI_LIEU_THAM_KHAO.md) | Links, Resources, Tools | ✅ Hoàn thành |

---

## 🎯 TECH STACK OVERVIEW

### Backend (.NET 10)
- **Framework:** ASP.NET Core Web API
- **Architecture:** Clean Architecture + CQRS
- **ORM:** Entity Framework Core
- **Patterns:** MediatR, FluentValidation, AutoMapper
- **Real-time:** SignalR
- **Message Queue:** RabbitMQ

### Frontend (Next.js 15)
- **Framework:** Next.js (App Router)
- **Language:** TypeScript
- **State:** Redux Toolkit
- **HTTP:** Axios
- **UI:** Tailwind CSS + Ant Design

### Database & Storage
- **Database:** PostgreSQL 16
- **Cache:** Redis 7
- **Storage:** MinIO S3

### Infrastructure
- **OS:** Debian 12
- **Containers:** Docker + Docker Compose
- **Proxy:** Nginx
- **Telephony:** FreeSWITCH

---

## 🏗️ KIẾN TRÚC 3-SERVER

```
Server 1: Application Server (8 vCPU, 16GB RAM)
├── Next.js 15 (Frontend)
├── .NET 10 API (Backend)
├── PostgreSQL 16 (Database)
├── Redis 7 (Cache)
└── RabbitMQ (Message Queue)

Server 2: Media Server (8 vCPU, 16GB RAM)
└── FreeSWITCH (Telephony Engine)

Server 3: Storage Server (4 vCPU, 8GB RAM)
└── MinIO (S3-compatible Storage)
```

**Chi phí:** ~$150-250/tháng  
**Capacity:** 100 agents, 200 concurrent calls

---

## 📋 CHECKLIST CẬP NHẬT TÀI LIỆU

### ✅ Đã hoàn thành
- [x] KIEN_TRUC_VA_TECH_STACK.md - Tài liệu mới chi tiết
- [x] 03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md - Cập nhật kiến trúc
- [x] FreeSwitchs/* - Tài liệu học FreeSWITCH (8 files)

### ⚠️ Cần cập nhật
- [ ] 00_TONG_QUAN_HE_THONG.md - Cập nhật tech stack
- [ ] 01_HA_TANG_VA_NHAN_SU.md - Cập nhật 3-server setup
- [ ] 04_ROADMAP_VA_TIMELINE.md - Cập nhật timeline với tech mới
- [ ] KIEN_TRUC_CHI_TIET_VA_LUONG_GOI.md - Cập nhật luồng với RabbitMQ

---

## 🔗 LIÊN KẾT NHANH

### Tài liệu quan trọng nhất
1. [KIEN_TRUC_VA_TECH_STACK.md](./Project_Documents/KIEN_TRUC_VA_TECH_STACK.md) - **BẮT ĐẦU TỪ ĐÂY**
2. [03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md](./Project_Documents/03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md) - Database & API design
3. [FreeSwitchs/README.md](./FreeSwitchs/README.md) - Học FreeSWITCH

### Theo vai trò

**Backend Developer:**
- KIEN_TRUC_VA_TECH_STACK.md (Section 2: Backend)
- 03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md
- FreeSwitchs/NGAY_06_07_MOD_XML_CURL.md
- FreeSwitchs/NGAY_08_09_EVENT_SOCKET_LAYER.md

**Frontend Developer:**
- KIEN_TRUC_VA_TECH_STACK.md (Section 3: Frontend)
- 02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md (UI/UX requirements)

**DevOps:**
- KIEN_TRUC_VA_TECH_STACK.md (Section 5: Infrastructure)
- 01_HA_TANG_VA_NHAN_SU.md
- FreeSwitchs/NGAY_01_02_CAI_DAT_FREESWITCH.md

**Project Manager:**
- 00_TONG_QUAN_HE_THONG.md
- 04_ROADMAP_VA_TIMELINE.md
- 02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md

---

## 📝 GHI CHÚ

### Thay đổi lớn (v2.0 - 06/01/2026)

**Backend:**
- ✅ .NET 8 → .NET 10
- ✅ Thêm RabbitMQ (Message Queue)
- ✅ Thêm SignalR (Real-time)

**Frontend:**
- ✅ React → Next.js 15
- ✅ Thêm TypeScript
- ✅ Thêm Redux Toolkit
- ✅ Material-UI → Tailwind CSS + Ant Design

**Infrastructure:**
- ✅ 1 server → 3 servers
- ✅ Tách FreeSWITCH ra server riêng
- ✅ Tách MinIO ra server riêng

**Database:**
- ✅ PostgreSQL 14 → PostgreSQL 16
- ✅ Redis 6 → Redis 7

---

**Cập nhật cuối:** 06/01/2026  
**Phiên bản:** 2.0
