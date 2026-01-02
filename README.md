# 📚 TÀI LIỆU DỰ ÁN CALL CENTER SAAS PLATFORM

> **Dự án:** Call Center SaaS Platform - Hệ thống tổng đài điện thoại đám mây  
> **Tech Stack:** .NET 8 Backend + ReactJS Frontend + FreeSWITCH  
> **Mô hình:** Agile/Scrum - Multi-tenant SaaS  
> **Timeline:** 12 tuần (3 tháng) cho MVP

---

## 🎯 GIỚI THIỆU

Đây là bộ tài liệu đầy đủ cho dự án phát triển hệ thống Call Center SaaS Platform. Tài liệu được tổ chức theo chuẩn Agile/Scrum, bao gồm các tài liệu kỹ thuật, quản lý dự án, và hướng dẫn triển khai.

**Mục tiêu dự án:**
- Xây dựng hệ thống tổng đài SaaS phục vụ nhiều khách hàng
- Hỗ trợ 100-200 cuộc gọi đồng thời (MVP)
- Triển khai trong 3 tháng với team 9 người
- Ngân sách: 880M - 1,133M VND

---

## 📖 CẤU TRÚC TÀI LIỆU

### 🌟 Tài liệu chính (Core Documents)

#### 1️⃣ [Tổng quan Hệ thống](00_TONG_QUAN_HE_THONG.md)
**Mục đích:** Executive Summary & System Overview  
**Đối tượng:** Tất cả stakeholders (Lãnh đạo, PM, Tech Lead)

**Nội dung:**
- 📊 Executive Summary
- 🏢 Giới thiệu dự án và bối cảnh thị trường
- 🎯 Mục tiêu kinh doanh (ngắn hạn & dài hạn)
- 💡 Tổng quan giải pháp
- 💰 Mô hình kinh doanh & Pricing
- 🏗️ Kiến trúc hệ thống tổng quan
- ⚡ Tính năng chính
- 🛠️ Tech stack summary
- 🗺️ Lộ trình triển khai
- ⚠️ Rủi ro và giải pháp

---

#### 2️⃣ [Hạ tầng & Nhân sự](01_HA_TANG_VA_NHAN_SU.md)
**Mục đích:** Infrastructure & Team Structure  
**Đối tượng:** PM, DevOps, Management

**Nội dung:**
- 🖥️ **Hạ tầng kỹ thuật**
  - Server requirements (All-in-One vs Tách biệt)
  - Network requirements
  - Firewall rules
  - Development infrastructure
- 🛠️ **Tech Stack chi tiết**
  - Backend: .NET 8 + packages
  - Frontend: ReactJS + libraries
  - Telephony: FreeSWITCH modules
  - Database & Storage
- 👥 **Cấu trúc nhân sự**
  - 9 vị trí: PM, Tech Lead, Senior/Mid Dev, DevOps, QA
  - Nhiệm vụ chi tiết từng vị trí
  - Kỹ năng yêu cầu
- 💵 **Ngân sách dự kiến**
  - Chi phí nhân sự: 793M - 1,033M (3 tháng)
  - Chi phí hạ tầng: 12-27M
  - Chi phí SIP Trunk: 20-33M
  - Tổng: 880M - 1,133M

---

#### 3️⃣ [Tài liệu Yêu cầu Phần mềm - SRS](02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md)
**Mục đích:** Software Requirements Specification  
**Đối tượng:** Dev team, QA, Product Owner

**Nội dung:**
- 👤 **Actors:** SuperAdmin, TenantAdmin, Supervisor, Agent
- ⚙️ **Yêu cầu chức năng (Functional Requirements)**
  - Authentication & Authorization
  - Tenant Management
  - Extension Management
  - Call Handling (Inbound/Outbound/Internal)
  - IVR & Queue
  - Recording & CDR
  - Billing
  - Real-time Dashboard
  - WebRTC Softphone
- 🔒 **Yêu cầu phi chức năng (Non-Functional Requirements)**
  - Performance (API < 200ms, 200 concurrent calls)
  - Security (JWT, RBAC, Encryption)
  - Scalability & Availability
- 📝 **Use Cases chi tiết**
  - UC-01: Tenant Admin tạo Agent
  - UC-02: Agent thực hiện cuộc gọi outbound
  - UC-03: Supervisor nghe lén cuộc gọi
- 🔌 **API Requirements**
  - RESTful endpoints
  - SignalR real-time
  - FreeSWITCH integration

---

#### 4️⃣ [Tài liệu Thiết kế Hệ thống - SDS](03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md)
**Mục đích:** System Design Specification  
**Đối tượng:** Dev team, DevOps, Tech Lead

**Nội dung:**
- 🏛️ **Clean Architecture Design**
  - Solution structure
  - Dependency flow
  - CQRS Pattern với MediatR
- 🗄️ **Database Design**
  - ERD (Entity Relationship Diagram)
  - Table definitions (PostgreSQL)
  - Indexes strategy
  - Partitioning
- 🔌 **API Design**
  - RESTful conventions
  - Endpoints detail
  - Versioning strategy
  - Rate limiting
- 📞 **FreeSWITCH Integration**
  - mod_xml_curl configuration
  - Directory XML (User authentication)
  - Dialplan XML (Call routing)
  - ESL (Event Socket Library)
- 🔄 **Sequence Diagrams**
  - User registration flow
  - Outbound call flow
  - Inbound call with IVR
  - Recording processing
- 🔐 **Security Design**
  - JWT authentication
  - RBAC authorization
  - Multi-tenancy isolation
  - SIP security (Fail2Ban)
- ⚡ **Caching Strategy**
  - Redis cache layers
  - Cache keys design
  - Cache invalidation
- 📦 **File Storage Design**
  - MinIO bucket structure
  - Presigned URL generation
- 🔴 **Real-time Communication**
  - SignalR Hub design
  - Client-side integration

---

#### 5️⃣ [Roadmap & Timeline](04_ROADMAP_VA_TIMELINE.md)
**Mục đích:** Project Roadmap & Detailed Timeline  
**Đối tượng:** PM, Team leads, All team members

**Nội dung:**
- 🗺️ **Roadmap tổng thể**
  - Phase 1: Foundation (Tuần 1-4)
  - Phase 2: SaaS Features (Tuần 5-8)
  - Phase 3: Advanced & Launch (Tuần 9-12)
- 📅 **Timeline chi tiết từng tuần**
  - Tuần 1: Hạ tầng & Hello World
  - Tuần 2: Directory & Dialplan Handler
  - Tuần 3: Authentication & Tenant Management
  - Tuần 4: SIP Trunking & Outbound Calls
  - Tuần 5: CDR & Recording
  - Tuần 6: IVR & Queue
  - Tuần 7: Real-time Dashboard
  - Tuần 8: Mobile App Foundation
  - Tuần 9: WebRTC Softphone
  - Tuần 10: Advanced Reports
  - Tuần 11: Security & Performance
  - Tuần 12: Deployment & Go Live
- 👥 **Phân công công việc**
  - Backend Team (Senior + Mid)
  - Frontend Team (Senior + Mid)
  - DevOps Engineer
  - QA Team
  - Workload phân bổ từng tuần
- 🎯 **Milestones & Deliverables**
  - M1: Foundation Complete (02/02/2026)
  - M2: SaaS Features Complete (02/03/2026)
  - M3: Production Ready (30/03/2026)
- ⚠️ **Risk Management**
  - Rủi ro kỹ thuật
  - Rủi ro kinh doanh
  - Rủi ro nhân sự
- 📊 **Metrics & KPIs**
  - Development metrics
  - Quality metrics
  - Performance metrics

---

### 📋 Tài liệu bổ sung

#### 6️⃣ [Danh sách Tài liệu Agile](DANH_SACH_TAI_LIEU_AGILE.md)
**Mục đích:** Checklist đầy đủ tài liệu Agile  
**Đối tượng:** Project Manager, Scrum Master

**Nội dung:**
- 📝 **Tài liệu Agile cơ bản**
  - Product Backlog
  - Sprint Backlog
  - Definition of Done
  - Sprint Planning/Review/Retrospective
- 🛠️ **Tài liệu kỹ thuật**
  - API Documentation
  - Database Schema
  - Code Documentation
  - Architecture Decision Records (ADR)
- 📊 **Tài liệu quản lý dự án**
  - Project Charter
  - Risk Register
  - Resource Plan
  - Budget Tracking
  - Status Reports
- ⚙️ **Tài liệu vận hành**
  - Deployment Guide
  - Operations Manual
  - Security Policy
  - SLA
  - Runbook
- 👥 **Tài liệu người dùng**
  - User Manual
  - Admin Guide
  - Training Materials
  - Release Notes
- 📄 **Templates**
  - Sprint Planning Template
  - Weekly Status Report Template
  - Release Notes Template
- ✅ **Best Practices**
  - Nguyên tắc tài liệu hóa
  - Checklist trước/sau Sprint

---

## 🚀 HƯỚNG DẪN SỬ DỤNG

### Cho Stakeholders/Management
1. Đọc [Tổng quan Hệ thống](00_TONG_QUAN_HE_THONG.md) để hiểu tổng quan dự án
2. Xem [Roadmap & Timeline](04_ROADMAP_VA_TIMELINE.md) để theo dõi tiến độ
3. Tham khảo [Hạ tầng & Nhân sự](01_HA_TANG_VA_NHAN_SU.md) để hiểu về ngân sách và team

### Cho Product Owner
1. Đọc [SRS](02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md) để hiểu chi tiết yêu cầu
2. Sử dụng [Danh sách Tài liệu Agile](DANH_SACH_TAI_LIEU_AGILE.md) để quản lý backlog
3. Theo dõi [Roadmap](04_ROADMAP_VA_TIMELINE.md) để planning

### Cho Tech Lead/Architects
1. Đọc [SDS](03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md) để hiểu kiến trúc chi tiết
2. Tham khảo [Hạ tầng & Nhân sự](01_HA_TANG_VA_NHAN_SU.md) cho tech stack
3. Review [SRS](02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md) để đảm bảo thiết kế đáp ứng yêu cầu

### Cho Developers
1. Đọc [SRS](02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md) để hiểu requirements
2. Đọc [SDS](03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md) để hiểu architecture và implement
3. Theo dõi [Roadmap](04_ROADMAP_VA_TIMELINE.md) để biết công việc từng tuần

### Cho DevOps
1. Đọc [Hạ tầng & Nhân sự](01_HA_TANG_VA_NHAN_SU.md) để setup infrastructure
2. Tham khảo [SDS](03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md) phần Deployment Architecture
3. Theo dõi [Roadmap](04_ROADMAP_VA_TIMELINE.md) để biết timeline deployment

### Cho QA/Testers
1. Đọc [SRS](02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md) để viết test cases
2. Tham khảo [Danh sách Tài liệu Agile](DANH_SACH_TAI_LIEU_AGILE.md) cho test planning
3. Theo dõi [Roadmap](04_ROADMAP_VA_TIMELINE.md) để biết timeline testing

---

## 📊 THÔNG TIN DỰ ÁN

### Thông số kỹ thuật
- **Backend:** .NET 8 + Clean Architecture + CQRS + MediatR
- **Frontend:** ReactJS 18 + TypeScript + Redux Toolkit
- **Mobile:** React Native
- **Telephony:** FreeSWITCH 1.10.9
- **Database:** PostgreSQL 15
- **Cache:** Redis 7
- **Storage:** MinIO (S3-compatible)
- **Real-time:** SignalR

### Team Structure
- 1 Project Manager
- 1 Tech Lead / Solution Architect
- 2 Backend Developers (.NET)
- 2 Frontend Developers (React)
- 1 DevOps Engineer
- 1 QA Lead
- 1 Manual Tester
- **Total:** 9 người

### Timeline
- **Duration:** 12 tuần (3 tháng)
- **Sprint:** 2 tuần/sprint (6 sprints)
- **Start:** 06/01/2026
- **MVP Launch:** 30/03/2026

### Budget
- **Nhân sự:** 793M - 1,033M VND
- **Hạ tầng:** 12-27M VND
- **SIP Trunk:** 20-33M VND
- **Phần mềm:** 19M VND
- **Khác:** 36M VND
- **TỔNG:** 880M - 1,133M VND

---

## 🎯 MILESTONES

| Milestone | Date | Status |
|-----------|------|--------|
| **M0:** Documentation Complete | 02/01/2026 | ✅ Done |
| **M1:** Foundation Complete | 02/02/2026 | ⏳ Pending |
| **M2:** SaaS Features Complete | 02/03/2026 | ⏳ Pending |
| **M3:** Production Ready | 30/03/2026 | ⏳ Pending |

---

## 📞 LIÊN HỆ

| Role | Name | Email | Phone |
|------|------|-------|-------|
| Project Manager | [TBD] | pm@company.com | [TBD] |
| Tech Lead | [TBD] | techlead@company.com | [TBD] |
| Product Owner | [TBD] | po@company.com | [TBD] |

---

## 📝 CHANGE LOG

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 02/01/2026 | PM & Tech Lead | Initial documentation complete |

---

## 📚 TÀI LIỆU THAM KHẢO

### VoIP & Telephony
- [FreeSWITCH Documentation](https://freeswitch.org/confluence/)
- [SIP RFC 3261](https://www.ietf.org/rfc/rfc3261.txt)
- [WebRTC Specification](https://www.w3.org/TR/webrtc/)

### .NET & Architecture
- [Clean Architecture by Jason Taylor](https://github.com/jasontaylordev/CleanArchitecture)
- [.NET 8 Documentation](https://docs.microsoft.com/en-us/dotnet/)
- [CQRS Pattern](https://martinfowler.com/bliki/CQRS.html)

### React & Frontend
- [React Documentation](https://react.dev/)
- [Material-UI](https://mui.com/)
- [JsSIP](https://jssip.net/)

### Agile/Scrum
- [Scrum Guide](https://scrumguides.org/)
- [Agile Manifesto](https://agilemanifesto.org/)
- [Atlassian Agile Coach](https://www.atlassian.com/agile)

---

## ⚖️ LICENSE

© 2026 [Company Name]. All rights reserved.

---

## 🙏 ACKNOWLEDGMENTS

Cảm ơn toàn bộ team đã đóng góp vào dự án này!

---

> [!NOTE]
> **Lưu ý:** Tài liệu này được cập nhật liên tục. Vui lòng kiểm tra version và ngày cập nhật trước khi sử dụng.

> [!TIP]
> **Khuyến nghị:** Bắt đầu đọc từ [Tổng quan Hệ thống](00_TONG_QUAN_HE_THONG.md) để có cái nhìn tổng quan trước khi đi vào chi tiết.

---

**Last Updated:** 02/01/2026  
**Version:** 1.0  
**Status:** ✅ Complete
