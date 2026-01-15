# BỘ TÀI LIỆU CHUẨN BỊ PHÁT TRIỂN
## Call Center SaaS Platform - Development Preparation Package

> [!IMPORTANT]
> Đây là bộ tài liệu HOÀN CHỈNH để chuẩn bị bắt tay vào code cho dự án Call Center SaaS Platform. Bao gồm roadmap chi tiết cho TẤT CẢ các vị trí: BA, Backend, Frontend, DevOps.

**Phiên bản:** 1.0  
**Ngày tạo:** 15/01/2026  
**Dự án:** Call Center SaaS Platform  
**Thời gian:** 12 tuần (6 sprints × 2 tuần)

---

## 📚 DANH MỤC TÀI LIỆU

### 1. Tài liệu tổng quan

#### **[00_CHUAN_BI_BAT_TAY_VAO_CODE.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/00_CHUAN_BI_BAT_TAY_VAO_CODE.md)** ⭐ **ĐỌC ĐẦU TIÊN**

**Nội dung:**
- ✅ Tổng quan dự án
- ✅ Tài liệu cần đọc (theo vai trò)
- ✅ Môi trường phát triển
- ✅ Kiến thức cần có
- ✅ Checklist chuẩn bị (Tuần 0)
- ✅ Quy trình làm việc (Git, Sprint, Code Review)

**Dành cho:** TẤT CẢ thành viên team  
**Thời gian đọc:** 1-2 giờ  
**Mục đích:** Hiểu rõ dự án và chuẩn bị môi trường trước Sprint 1

---

### 2. Roadmap theo vai trò

#### **[ROADMAP_CHI_TIET_BA.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_BA.md)**

**Nội dung:**
- ✅ Trách nhiệm BA/Product Owner
- ✅ Kỹ năng cần có
- ✅ Roadmap từng tuần (12 tuần)
- ✅ User Story templates
- ✅ UAT test case templates
- ✅ Deliverables theo Sprint
- ✅ Best practices

**Dành cho:** Business Analyst / Product Owner  
**Thời gian đọc:** 2-3 giờ  
**Highlights:**
- Viết 45 User Stories
- UAT testing cho 6 sprints
- Sprint Planning & Review
- Documentation (Release Notes, User Manual)

---

#### **[ROADMAP_CHI_TIET_BACKEND.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_BACKEND.md)**

**Nội dung:**
- ✅ Phân chia công việc (Senior vs Mid)
- ✅ Tech Stack chi tiết (.NET 10, EF Core, MediatR)
- ✅ Roadmap từng tuần với code examples
- ✅ Clean Architecture setup
- ✅ mod_xml_curl integration
- ✅ ESL Worker Service
- ✅ SignalR Hub
- ✅ Best practices & patterns

**Dành cho:** Backend Engineers (Senior & Mid)  
**Thời gian đọc:** 3-4 giờ  
**Highlights:**
- Clean Architecture + CQRS
- FreeSWITCH integration (mod_xml_curl, ESL)
- Billing engine
- Real-time với SignalR
- WebRTC support

---

#### **[ROADMAP_CHI_TIET_FRONTEND.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_FRONTEND.md)**

**Nội dung:**
- ✅ Phân chia công việc (Senior vs Mid)
- ✅ Tech Stack chi tiết (Next.js 15, Redux Toolkit)
- ✅ Roadmap từng tuần với component examples
- ✅ Next.js App Router setup
- ✅ Redux state management
- ✅ WebRTC Softphone (JsSIP)
- ✅ SignalR real-time dashboard
- ✅ Best practices

**Dành cho:** Frontend Engineers (Senior & Mid)  
**Thời gian đọc:** 2-3 giờ  
**Highlights:**
- Next.js 15 with App Router
- Redux Toolkit state management
- IVR Builder (React Flow)
- WebRTC Softphone
- Real-time dashboard với SignalR

---

#### **[ROADMAP_CHI_TIET_DEVOPS.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_DEVOPS.md)**

**Nội dung:**
- ✅ Infrastructure overview (3-server architecture)
- ✅ Network configuration
- ✅ Roadmap từng tuần với commands
- ✅ FreeSWITCH installation & configuration
- ✅ CI/CD pipeline (GitHub Actions)
- ✅ Monitoring (Prometheus + Grafana)
- ✅ Security hardening
- ✅ Production deployment

**Dành cho:** DevOps Engineer  
**Thời gian đọc:** 3-4 giờ  
**Highlights:**
- 3-server setup (App, Media, Storage)
- FreeSWITCH từ source
- CI/CD với GitHub Actions
- Monitoring dashboards
- Load testing & production launch

---

## 🎯 QUICK START GUIDE

### Bước 1: Đọc tài liệu tổng quan (TẤT CẢ)

1. ✅ Đọc [00_CHUAN_BI_BAT_TAY_VAO_CODE.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/00_CHUAN_BI_BAT_TAY_VAO_CODE.md)
2. ✅ Hiểu rõ kiến trúc 3-server
3. ✅ Hiểu rõ tech stack
4. ✅ Hoàn thành checklist chuẩn bị

### Bước 2: Đọc roadmap theo vai trò

**Business Analyst:**
- ✅ Đọc [ROADMAP_CHI_TIET_BA.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_BA.md)
- ✅ Đọc [PRODUCT_BACKLOG.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/AGILE_DOCS/PRODUCT_BACKLOG.md)
- ✅ Chuẩn bị Sprint 1 backlog

**Backend Engineer:**
- ✅ Đọc [ROADMAP_CHI_TIET_BACKEND.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_BACKEND.md)
- ✅ Học FreeSWITCH basics (40h)
- ✅ Setup .NET 10 environment

**Frontend Engineer:**
- ✅ Đọc [ROADMAP_CHI_TIET_FRONTEND.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_FRONTEND.md)
- ✅ Học Next.js 15 App Router (20h)
- ✅ Setup Node.js environment

**DevOps Engineer:**
- ✅ Đọc [ROADMAP_CHI_TIET_DEVOPS.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/ROADMAP_CHI_TIET_DEVOPS.md)
- ✅ Học FreeSWITCH installation (40h)
- ✅ Thuê 3 VPS servers

### Bước 3: Hoàn thành Tuần 0 Preparation

**TẤT CẢ thành viên:**
- [ ] Đọc tài liệu (Ngày 1-2)
- [ ] Setup môi trường (Ngày 3)
- [ ] Access & Accounts (Ngày 4)
- [ ] Team Meeting (Ngày 5)

**Theo vai trò:**
- [ ] BA: Review Product Backlog, Setup Jira
- [ ] Backend: Học FreeSWITCH, Setup .NET
- [ ] Frontend: Học Next.js, Setup Node.js
- [ ] DevOps: Học FreeSWITCH, Setup servers

### Bước 4: Sprint 1 Planning

- [ ] Tham gia Sprint Planning meeting
- [ ] Nhận task assignments
- [ ] Bắt đầu development

---

## 📊 TỔNG QUAN ROADMAP

### Timeline 12 tuần

```
Phase 1: Foundation (Tuần 1-4)
├── Sprint 1: Infrastructure & Auth (Tuần 1-2)
└── Sprint 2: Core Features & Calling (Tuần 3-4)

Phase 2: Telephony (Tuần 5-8)
├── Sprint 3: CDR & IVR (Tuần 5-6)
└── Sprint 4: Real-time & Queue (Tuần 7-8)

Phase 3: Advanced (Tuần 9-12)
├── Sprint 5: WebRTC & Mobile (Tuần 9-10)
└── Sprint 6: Polish & Launch (Tuần 11-12)
```

### Deliverables chính

| Sprint | BA | Backend | Frontend | DevOps |
|--------|----|---------| ---------|--------|
| **Sprint 1** | User Stories, UAT | Auth API, mod_xml_curl | Login UI, Dashboard | Servers, FreeSWITCH |
| **Sprint 2** | User Stories, UAT | Billing, ESL | Rate Table UI | SIP Trunk, SSL |
| **Sprint 3** | IVR Spec, UAT | IVR Engine, CDR | IVR Builder, CDR UI | MinIO, Recording |
| **Sprint 4** | Event Spec, UAT | SignalR Hub | Live Monitor | Load Balancer |
| **Sprint 5** | Mobile UX, UAT | WebRTC API | Softphone, Mobile | TURN Server |
| **Sprint 6** | Final UAT, Docs | Optimization | Polish | Production |

---

## 🔑 KEY SUCCESS FACTORS

### 1. Preparation (Tuần 0)

✅ **CRITICAL:** Hoàn thành TẤT CẢ checklist trong Tuần 0  
✅ Đọc kỹ tài liệu theo vai trò  
✅ Setup môi trường đầy đủ  
✅ Hiểu rõ kiến trúc và tech stack

### 2. Communication

✅ Daily Standup: 9:00 AM mỗi ngày  
✅ Sprint Planning: Monday tuần đầu  
✅ Sprint Review: Friday tuần cuối  
✅ Sprint Retro: Friday tuần cuối

### 3. Quality

✅ Code review: Tất cả PR phải được review  
✅ Testing: Unit tests (>70% coverage)  
✅ UAT: BA test tất cả features  
✅ Documentation: Update liên tục

### 4. Collaboration

✅ Backend ↔ Frontend: API contract rõ ràng  
✅ Backend ↔ DevOps: Deployment process  
✅ BA ↔ All: User Stories chi tiết  
✅ All ↔ All: Blockers escalate ngay

---

## 📞 LIÊN HỆ & HỖ TRỢ

**Escalation Path:**
1. Tech Lead (technical questions)
2. Product Owner (business questions)
3. Scrum Master (process questions)

**Channels:**
- Slack/Discord: Daily communication
- Jira: Task tracking
- GitHub: Code repository
- Confluence: Documentation

---

## 🎓 LEARNING RESOURCES

### Tài liệu dự án

**Bắt buộc đọc:**
1. [KIEN_TRUC_VA_TECH_STACK.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/Project_Documents/KIEN_TRUC_VA_TECH_STACK.md) ⭐
2. [00_TONG_QUAN_HE_THONG.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/Project_Documents/00_TONG_QUAN_HE_THONG.md)
3. [03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/Project_Documents/03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md)

**Tham khảo:**
- [02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/Project_Documents/02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md)
- [04_ROADMAP_VA_TIMELINE.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/Project_Documents/04_ROADMAP_VA_TIMELINE.md)
- [PRODUCT_BACKLOG.md](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI/AGILE_DOCS/PRODUCT_BACKLOG.md)

### External Resources

**Backend:**
- [.NET 10 Documentation](https://docs.microsoft.com/en-us/dotnet/)
- [FreeSWITCH Wiki](https://freeswitch.org/confluence/)
- [Clean Architecture](https://github.com/jasontaylordev/CleanArchitecture)

**Frontend:**
- [Next.js 15 Docs](https://nextjs.org/docs)
- [Redux Toolkit](https://redux-toolkit.js.org/)
- [JsSIP](https://jssip.net/)

**DevOps:**
- [Docker Documentation](https://docs.docker.com/)
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Prometheus](https://prometheus.io/docs/)

---

## ✅ CHECKLIST TỔNG HỢP

### Tuần 0: Preparation

**Tất cả:**
- [ ] Đọc 00_CHUAN_BI_BAT_TAY_VAO_CODE.md
- [ ] Đọc roadmap theo vai trò
- [ ] Setup môi trường
- [ ] GitHub access
- [ ] Jira access

**BA:**
- [ ] Đọc PRODUCT_BACKLOG.md
- [ ] Setup Jira project
- [ ] Prepare Sprint 1 backlog

**Backend:**
- [ ] Học FreeSWITCH (40h)
- [ ] Setup .NET 10
- [ ] Setup PostgreSQL local

**Frontend:**
- [ ] Học Next.js 15 (20h)
- [ ] Setup Node.js 20
- [ ] Practice TypeScript

**DevOps:**
- [ ] Học FreeSWITCH (40h)
- [ ] Thuê 3 VPS
- [ ] Setup firewall

### Sprint 1: Ready to Code

- [ ] Sprint Planning completed
- [ ] Tasks assigned
- [ ] Development environment ready
- [ ] Git repository cloned
- [ ] Daily standup scheduled

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0  
**Status:** ✅ Ready for Sprint 1

> [!TIP]
> Bắt đầu với [00_CHUAN_BI_BAT_TAY_VAO_CODE.md](file:///C:/Users/LENOVO/.gemini/antigravity/brain/33284b68-a549-4606-86c5-47e1737f1906/00_CHUAN_BI_BAT_TAY_VAO_CODE.md), sau đó đọc roadmap theo vai trò của bạn. Hoàn thành checklist Tuần 0 trước khi bắt đầu Sprint 1.

> [!NOTE]
> Tất cả tài liệu này được tạo dựa trên tài liệu dự án hiện có. Nếu có thắc mắc, vui lòng tham khảo tài liệu gốc trong thư mục [TAI_LIEU_DU_AN_MOI](file:///c:/Users/LENOVO/Desktop/TTS-BE/Call/TAI_LIEU_DU_AN_MOI).
