# ONBOARDING GUIDE
## Call Center SaaS Platform - New Team Member Onboarding

> [!IMPORTANT]
> Chào mừng bạn đến với dự án Call Center SaaS Platform! Đây là bộ tài liệu onboarding để giúp bạn nhanh chóng hòa nhập và bắt đầu làm việc hiệu quả.

**Phiên bản:** 1.0  
**Ngày tạo:** 15/01/2026  
**Dự án:** Call Center SaaS Platform

---

## 📋 MỤC LỤC

1. [Chào mừng](#1-chào-mừng)
2. [Tài liệu theo vai trò](#2-tài-liệu-theo-vai-trò)
3. [Quy trình onboarding](#3-quy-trình-onboarding)
4. [Liên hệ & Hỗ trợ](#4-liên-hệ--hỗ-trợ)

---

## 1. CHÀO MỪNG

### 1.1. Về dự án

**Call Center SaaS Platform** là một nền tảng tổng đài đám mây (Cloud Call Center) đa khách hàng (Multi-tenant), được xây dựng với công nghệ hiện đại:

- **Backend:** .NET 10, PostgreSQL 16, Redis, RabbitMQ
- **Frontend:** Next.js 15, React 18, TypeScript
- **Telephony:** FreeSWITCH
- **Infrastructure:** 3-server architecture (App, Media, Storage)

### 1.2. Team Structure

| Vai trò | Số lượng | Trách nhiệm chính |
|---------|----------|-------------------|
| **Business Analyst** | 1 | User Stories, UAT, Product Backlog |
| **Backend Senior** | 1 | Architecture, FreeSWITCH, Core APIs |
| **Backend Mid** | 1 | CRUD APIs, Testing, Bug fixing |
| **Frontend Senior** | 1 | Architecture, WebRTC, Real-time |
| **Frontend Mid** | 1 | UI Components, Forms, Integration |
| **DevOps** | 1 | Infrastructure, CI/CD, Monitoring |

### 1.3. Phương pháp làm việc

- **Methodology:** Agile/Scrum
- **Sprint Duration:** 2 tuần
- **Daily Standup:** 9:00 AM mỗi ngày
- **Sprint Planning:** Monday tuần đầu
- **Sprint Review/Retro:** Friday tuần cuối

---

## 2. TÀI LIỆU THEO VAI TRÒ

### 📊 Business Analyst

**Tài liệu onboarding:**
- **[ONBOARDING_BA.md](./ONBOARDING_BA.md)** ⭐
  - Tuần đầu tiên làm gì
  - Tài liệu cần đọc
  - Tools cần setup
  - Người cần gặp
  - Deliverables đầu tiên

**Roadmap chi tiết:**
- [ROADMAP_CHI_TIET_BA.md](../Preparing_For_Project_Implementation/ROADMAP_CHI_TIET_BA.md)

---

### 💻 Backend Engineer

**Tài liệu onboarding:**
- **[ONBOARDING_BACKEND.md](./ONBOARDING_BACKEND.md)** ⭐
  - Tuần đầu tiên làm gì
  - Setup môi trường (.NET, PostgreSQL, FreeSWITCH)
  - Codebase walkthrough
  - First task assignment
  - Code review process

**Roadmap chi tiết:**
- [ROADMAP_CHI_TIET_BACKEND.md](../Preparing_For_Project_Implementation/ROADMAP_CHI_TIET_BACKEND.md)

---

### 🎨 Frontend Engineer

**Tài liệu onboarding:**
- **[ONBOARDING_FRONTEND.md](./ONBOARDING_FRONTEND.md)** ⭐
  - Tuần đầu tiên làm gì
  - Setup môi trường (Node.js, Next.js)
  - Project structure
  - Component library
  - First task assignment

**Roadmap chi tiết:**
- [ROADMAP_CHI_TIET_FRONTEND.md](../Preparing_For_Project_Implementation/ROADMAP_CHI_TIET_FRONTEND.md)

---

### 🔧 DevOps Engineer

**Tài liệu onboarding:**
- **[ONBOARDING_DEVOPS.md](./ONBOARDING_DEVOPS.md)** ⭐
  - Tuần đầu tiên làm gì
  - Infrastructure overview
  - Access & credentials
  - Monitoring dashboards
  - First tasks

**Roadmap chi tiết:**
- [ROADMAP_CHI_TIET_DEVOPS.md](../Preparing_For_Project_Implementation/ROADMAP_CHI_TIET_DEVOPS.md)

---

## 3. QUY TRÌNH ONBOARDING

### Week 0: Pre-boarding (Trước ngày đầu tiên)

**HR sẽ gửi cho bạn:**
- [ ] Contract & paperwork
- [ ] Company handbook
- [ ] IT equipment list
- [ ] Access requests form

**Bạn cần chuẩn bị:**
- [ ] Laptop cá nhân (nếu BYOD)
- [ ] Đọc tài liệu dự án cơ bản
- [ ] Chuẩn bị câu hỏi

---

### Week 1: Orientation & Setup

#### **Ngày 1: Welcome & Overview**

**Morning (9:00 AM - 12:00 PM):**
- [ ] 9:00 - Welcome meeting với Team Lead
- [ ] 9:30 - Tour văn phòng & giới thiệu team
- [ ] 10:00 - IT setup (laptop, accounts, access)
- [ ] 11:00 - Đọc tài liệu dự án

**Afternoon (1:00 PM - 5:00 PM):**
- [ ] 1:00 - Đọc tài liệu onboarding theo vai trò
- [ ] 3:00 - 1-on-1 với Manager
- [ ] 4:00 - Setup development environment

**Deliverables:**
- ✅ All accounts created
- ✅ Development environment ready
- ✅ Understand project overview

---

#### **Ngày 2-3: Deep Dive**

**Theo vai trò:**

**BA:**
- [ ] Đọc Product Backlog
- [ ] Review User Stories
- [ ] Shadow current BA
- [ ] Attend Sprint Planning

**Backend:**
- [ ] Clone repository
- [ ] Run project locally
- [ ] Code walkthrough với Senior
- [ ] Đọc Clean Architecture docs

**Frontend:**
- [ ] Clone repository
- [ ] Run project locally
- [ ] Component library walkthrough
- [ ] Đọc Next.js docs

**DevOps:**
- [ ] Access servers
- [ ] Review infrastructure
- [ ] Monitoring dashboards
- [ ] CI/CD pipeline walkthrough

---

#### **Ngày 4-5: First Tasks**

- [ ] Nhận first task assignment
- [ ] Pair programming với Senior
- [ ] Submit first PR
- [ ] Code review feedback

**Deliverables:**
- ✅ First PR merged
- ✅ Understand team workflow
- ✅ Know who to ask for help

---

### Week 2: Ramp Up

#### **Mục tiêu:**
- [ ] Hoàn thành 2-3 tasks độc lập
- [ ] Tham gia đầy đủ Scrum ceremonies
- [ ] Contribute to code review
- [ ] Ask questions proactively

#### **End of Week 2:**
- [ ] 1-on-1 feedback với Manager
- [ ] Self-assessment
- [ ] Set goals for Month 1

---

### Month 1: Productivity

#### **Mục tiêu:**
- [ ] Deliver features independently
- [ ] Mentor new team members
- [ ] Contribute to technical discussions
- [ ] Improve team processes

---

## 4. LIÊN HỆ & HỖ TRỢ

### 4.1. Key Contacts

| Vai trò | Tên | Email | Slack |
|---------|-----|-------|-------|
| **Product Owner** | [Tên] | po@company.com | @po |
| **Tech Lead** | [Tên] | techlead@company.com | @techlead |
| **DevOps Lead** | [Tên] | devops@company.com | @devops |
| **HR** | [Tên] | hr@company.com | @hr |

### 4.2. Communication Channels

**Slack Channels:**
- `#general` - Team announcements
- `#dev-backend` - Backend discussions
- `#dev-frontend` - Frontend discussions
- `#devops` - Infrastructure & deployment
- `#random` - Water cooler chat

**Tools:**
- **Jira:** Task tracking
- **GitHub:** Code repository
- **Confluence:** Documentation
- **Zoom:** Video calls

### 4.3. Escalation Path

**Nếu bạn gặp vấn đề:**
1. **Technical:** Ask in Slack channel → Tech Lead
2. **Process:** Ask Scrum Master → Manager
3. **HR/Admin:** Ask HR
4. **Urgent:** Call Manager directly

---

## 5. CHECKLIST TỔNG HỢP

### Pre-boarding
- [ ] Nhận contract & equipment
- [ ] Đọc company handbook
- [ ] Chuẩn bị laptop

### Day 1
- [ ] Welcome meeting
- [ ] IT setup complete
- [ ] All accounts created
- [ ] Đọc project overview

### Week 1
- [ ] Development environment ready
- [ ] Understand codebase structure
- [ ] Know team members
- [ ] Submit first PR

### Week 2
- [ ] Complete 2-3 tasks
- [ ] Participate in all ceremonies
- [ ] Receive feedback from Manager

### Month 1
- [ ] Deliver features independently
- [ ] Contribute to code reviews
- [ ] Help onboard next new member

---

## 6. TÀI LIỆU THAM KHẢO

### Bắt buộc đọc:
1. [KIEN_TRUC_VA_TECH_STACK.md](../Project_Documents/KIEN_TRUC_VA_TECH_STACK.md) ⭐
2. [00_TONG_QUAN_HE_THONG.md](../Project_Documents/00_TONG_QUAN_HE_THONG.md)
3. [00_CHUAN_BI_DEVELOP.md](../Preparing_For_Project_Implementation/00_CHUAN_BI_DEVELOP.md)

### Theo vai trò:
- **BA:** [PRODUCT_BACKLOG.md](../AGILE_DOCS/PRODUCT_BACKLOG.md)
- **Backend:** [03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md](../Project_Documents/03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md)
- **Frontend:** [02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md](../Project_Documents/02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md)
- **DevOps:** [01_HA_TANG_VA_NHAN_SU.md](../Project_Documents/01_HA_TANG_VA_NHAN_SU.md)

---

## 7. TIPS CHO NGƯỜI MỚI

### ✅ DO's
- ✅ Ask questions early and often
- ✅ Take notes during meetings
- ✅ Read code before asking
- ✅ Participate in code reviews
- ✅ Share your ideas
- ✅ Ask for feedback regularly

### ❌ DON'Ts
- ❌ Afraid to ask "dumb" questions
- ❌ Work in isolation
- ❌ Skip daily standups
- ❌ Commit without code review
- ❌ Ignore team conventions
- ❌ Hesitate to ask for help

---

**Ngày tạo:** 15/01/2026  
**Phiên bản:** 1.0  
**Next Review:** Monthly

> [!TIP]
> Bắt đầu với tài liệu onboarding theo vai trò của bạn, sau đó follow checklist từng tuần. Đừng ngại hỏi - team luôn sẵn sàng giúp đỡ!

> [!NOTE]
> Onboarding là quá trình 2 chiều. Chúng tôi muốn nghe feedback của bạn để cải thiện quy trình này. Hãy chia sẻ ý kiến của bạn sau Week 1 và Month 1!
