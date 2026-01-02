# DANH SÁCH TÀI LIỆU ĐẦY ĐỦ
## Cho Dự Án Phát Triển Phần Mềm Theo Mô Hình Agile

> [!IMPORTANT]
> Tài liệu này liệt kê đầy đủ các loại tài liệu cần thiết để phát triển một hệ thống phần mềm theo mô hình Agile/Scrum.

**Phiên bản:** 1.0  
**Ngày tạo:** 02/01/2026  
**Mục đích:** Checklist tài liệu cho Project Manager

---

## MỤC LỤC

1. [Tài liệu Agile cơ bản](#1-tài-liệu-agile-cơ-bản)
2. [Tài liệu kỹ thuật](#2-tài-liệu-kỹ-thuật)
3. [Tài liệu quản lý dự án](#3-tài-liệu-quản-lý-dự-án)
4. [Tài liệu vận hành](#4-tài-liệu-vận-hành)
5. [Tài liệu người dùng](#5-tài-liệu-người-dùng)
6. [Checklist cho dự án Call Center](#6-checklist-cho-dự-án-call-center)

---

## 1. TÀI LIỆU AGILE CƠ BẢN

### 1.1. Product Backlog
**Mục đích:** Danh sách tất cả tính năng, yêu cầu, cải tiến

**Nội dung:**
- User Stories (định dạng: As a [role], I want [feature], so that [benefit])
- Acceptance Criteria
- Story Points (ước lượng)
- Priority (MoSCoW: Must have, Should have, Could have, Won't have)
- Dependencies

**Tool:** Jira, Azure DevOps, Trello

**Ví dụ User Story:**
```
Title: Tạo Extension cho Agent
As a: Tenant Admin
I want: Tạo số máy nhánh (Extension) cho nhân viên
So that: Nhân viên có thể đăng nhập và gọi điện

Acceptance Criteria:
- Extension number phải unique trong Tenant
- Extension number: 3-5 chữ số
- Tự động generate SIP password
- Gửi email thông tin SIP cho Agent

Story Points: 5
Priority: Must Have
```

---

### 1.2. Sprint Backlog
**Mục đích:** Danh sách công việc cho Sprint hiện tại

**Nội dung:**
- User Stories được chọn cho Sprint
- Tasks breakdown (chia nhỏ thành tasks)
- Assigned to (người làm)
- Status (To Do, In Progress, Done)
- Burndown chart

**Cập nhật:** Daily

---

### 1.3. Definition of Done (DoD)
**Mục đích:** Tiêu chí để coi một task/story là hoàn thành

**Ví dụ DoD:**
```markdown
## Definition of Done

Story được coi là Done khi:
- [ ] Code implemented
- [ ] Unit tests written (coverage > 70%)
- [ ] Code reviewed và approved
- [ ] Integration tests passed
- [ ] QA tested và approved
- [ ] Documentation updated
- [ ] Deployed to staging
- [ ] Product Owner accepted
```

---

### 1.4. Sprint Planning Document
**Mục đích:** Kế hoạch cho Sprint

**Nội dung:**
- Sprint Goal
- Sprint Duration (thường 2 tuần)
- Team Capacity
- Selected User Stories
- Sprint Backlog
- Risks

**Tạo khi:** Đầu mỗi Sprint

---

### 1.5. Sprint Review Notes
**Mục đích:** Ghi chú buổi review cuối Sprint

**Nội dung:**
- Demo deliverables
- Completed stories
- Incomplete stories (và lý do)
- Feedback từ stakeholders
- Action items

**Tạo khi:** Cuối mỗi Sprint

---

### 1.6. Sprint Retrospective Notes
**Mục đích:** Cải tiến quy trình

**Nội dung:**
- What went well?
- What can be improved?
- Action items cho Sprint tiếp theo
- Metrics (velocity, bug count, etc.)

**Format:** Start-Stop-Continue

**Tạo khi:** Cuối mỗi Sprint

---

## 2. TÀI LIỆU KỸ THUẬT

### 2.1. System Overview Document ✅
**File:** `00_TONG_QUAN_HE_THONG.md`

**Nội dung:**
- Executive summary
- Business context
- High-level architecture
- Tech stack overview
- Deployment model

**Đối tượng:** Tất cả stakeholders

---

### 2.2. Software Requirements Specification (SRS) ✅
**File:** `02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md`

**Nội dung:**
- Functional requirements
- Non-functional requirements
- Use cases
- API requirements
- Data requirements

**Đối tượng:** Dev team, QA, Product Owner

---

### 2.3. System Design Specification (SDS) ✅
**File:** `03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md`

**Nội dung:**
- Architecture design
- Database design
- API design
- Sequence diagrams
- Security design
- Caching strategy

**Đối tượng:** Dev team, DevOps

---

### 2.4. Infrastructure & Team Document ✅
**File:** `01_HA_TANG_VA_NHAN_SU.md`

**Nội dung:**
- Server requirements
- Tech stack chi tiết
- Team structure
- Budget

**Đối tượng:** PM, DevOps, Management

---

### 2.5. API Documentation
**Tool:** Swagger/OpenAPI

**Nội dung:**
- Endpoints
- Request/Response format
- Authentication
- Error codes
- Examples

**Auto-generate:** Từ code annotations

---

### 2.6. Database Schema Documentation
**Tool:** dbdiagram.io, DBeaver

**Nội dung:**
- ERD (Entity Relationship Diagram)
- Table definitions
- Indexes
- Constraints
- Migration scripts

**Cập nhật:** Mỗi khi thay đổi schema

---

### 2.7. Code Documentation
**Nội dung:**
- README.md (mỗi repository)
- Code comments
- Architecture Decision Records (ADR)
- Coding standards

**Ví dụ README.md:**
```markdown
# Call Center SaaS - Backend API

## Tech Stack
- .NET 8
- PostgreSQL 15
- Redis 7

## Setup
1. Install .NET 8 SDK
2. Run `dotnet restore`
3. Update connection string in appsettings.json
4. Run `dotnet ef database update`
5. Run `dotnet run`

## Testing
- Unit tests: `dotnet test`
- Coverage: `dotnet test /p:CollectCoverage=true`
```

---

### 2.8. Architecture Decision Records (ADR)
**Mục đích:** Ghi lại các quyết định kiến trúc quan trọng

**Format:**
```markdown
# ADR-001: Chọn .NET thay vì Node.js cho Backend

## Status
Accepted

## Context
Cần chọn tech stack cho Backend API

## Decision
Chọn .NET 8 thay vì Node.js

## Consequences
**Pros:**
- Performance cao hơn
- Type safety (C#)
- Mature ecosystem

**Cons:**
- Team cần học .NET
- Ít developer .NET hơn Node.js
```

---

## 3. TÀI LIỆU QUẢN LÝ DỰ ÁN

### 3.1. Project Charter
**Mục đích:** Khởi động dự án chính thức

**Nội dung:**
- Project name & description
- Objectives
- Scope
- Stakeholders
- Budget
- Timeline
- Success criteria
- Approval signatures

---

### 3.2. Roadmap & Timeline ✅
**File:** `04_ROADMAP_VA_TIMELINE.md`

**Nội dung:**
- Timeline tổng thể
- Milestones
- Sprint planning
- Deliverables
- Dependencies

---

### 3.3. Risk Register
**Mục đích:** Quản lý rủi ro

**Format:**
| Risk ID | Description | Probability | Impact | Mitigation | Owner |
|---------|-------------|-------------|--------|------------|-------|
| R-001 | FreeSWITCH phức tạp | High | High | Thuê consultant | Tech Lead |

**Cập nhật:** Weekly

---

### 3.4. Resource Plan
**Nội dung:**
- Team members
- Roles & responsibilities
- Allocation (% time)
- Skills matrix
- Training needs

---

### 3.5. Communication Plan
**Nội dung:**
- Daily standup (9:00 AM)
- Sprint planning (Monday 9:00 AM)
- Sprint review (Friday 4:00 PM)
- Sprint retro (Friday 5:00 PM)
- Stakeholder updates (Bi-weekly)

---

### 3.6. Budget Tracking
**Nội dung:**
- Planned budget
- Actual spend
- Variance
- Forecast

**Tool:** Excel, Google Sheets

---

### 3.7. Status Reports
**Frequency:** Weekly hoặc Bi-weekly

**Nội dung:**
- Progress summary
- Completed items
- Upcoming items
- Blockers
- Risks
- Budget status

---

## 4. TÀI LIỆU VẬN HÀNH

### 4.1. Deployment Guide
**Nội dung:**
- Environment setup (Dev, Staging, Prod)
- Deployment steps
- Rollback procedure
- Configuration management
- Secrets management

---

### 4.2. Operations Manual
**Nội dung:**
- System monitoring
- Backup & restore
- Disaster recovery
- Scaling procedures
- Troubleshooting guide

---

### 4.3. Security Policy
**Nội dung:**
- Authentication & authorization
- Data encryption
- Network security
- Vulnerability management
- Incident response

---

### 4.4. SLA (Service Level Agreement)
**Nội dung:**
- Uptime commitment (99.5%, 99.9%)
- Response time
- Support hours
- Escalation process
- Penalties

---

### 4.5. Runbook
**Nội dung:**
- Common issues & solutions
- Emergency procedures
- Contact list
- Escalation matrix

---

## 5. TÀI LIỆU NGƯỜI DÙNG

### 5.1. User Manual
**Đối tượng:** End users

**Nội dung:**
- Getting started
- Features guide
- Screenshots
- FAQs
- Troubleshooting

---

### 5.2. Admin Guide
**Đối tượng:** System administrators

**Nội dung:**
- System configuration
- User management
- Monitoring
- Maintenance

---

### 5.3. Training Materials
**Nội dung:**
- Presentation slides
- Video tutorials
- Hands-on exercises
- Cheat sheets

---

### 5.4. Release Notes
**Frequency:** Mỗi release

**Nội dung:**
- Version number
- Release date
- New features
- Bug fixes
- Breaking changes
- Migration guide

---

## 6. CHECKLIST CHO DỰ ÁN CALL CENTER

### 6.1. Tài liệu đã có ✅

- [x] **00_TONG_QUAN_HE_THONG.md** - System Overview
- [x] **01_HA_TANG_VA_NHAN_SU.md** - Infrastructure & Team
- [x] **02_TAI_LIEU_YEU_CAU_PHAN_MEM_SRS.md** - Requirements
- [x] **03_TAI_LIEU_THIET_KE_HE_THONG_SDS.md** - System Design
- [x] **04_ROADMAP_VA_TIMELINE.md** - Roadmap & Timeline

---

### 6.2. Tài liệu cần tạo thêm

#### Agile Documents
- [ ] **Product Backlog** (Jira/Azure DevOps)
- [ ] **Sprint Backlog** (Jira/Azure DevOps)
- [ ] **Definition of Done** (Confluence/Wiki)
- [ ] **Sprint Planning Template** (Confluence)
- [ ] **Sprint Review Template** (Confluence)
- [ ] **Sprint Retrospective Template** (Confluence)

#### Technical Documents
- [ ] **API Documentation** (Swagger - auto-generate)
- [ ] **Database Schema Doc** (dbdiagram.io)
- [ ] **README.md** (Mỗi repository)
- [ ] **Architecture Decision Records** (ADR)
- [ ] **Code Style Guide** (Wiki)

#### Project Management
- [ ] **Project Charter** (Word/PDF)
- [ ] **Risk Register** (Excel)
- [ ] **Resource Plan** (Excel)
- [ ] **Communication Plan** (Wiki)
- [ ] **Budget Tracking** (Excel)
- [ ] **Weekly Status Report Template** (Word)

#### Operations
- [ ] **Deployment Guide** (Wiki)
- [ ] **Operations Manual** (Wiki)
- [ ] **Security Policy** (PDF)
- [ ] **SLA Document** (PDF)
- [ ] **Runbook** (Wiki)

#### User Documents
- [ ] **User Manual** (Wiki/PDF)
- [ ] **Admin Guide** (Wiki/PDF)
- [ ] **Training Slides** (PowerPoint)
- [ ] **Video Tutorials** (YouTube/Loom)
- [ ] **Release Notes Template** (Markdown)

---

### 6.3. Công cụ khuyến nghị

| Loại tài liệu | Công cụ | Lý do |
|---------------|---------|-------|
| **Agile/Scrum** | Jira hoặc Azure DevOps | Industry standard, tích hợp tốt |
| **Wiki/Knowledge Base** | Confluence hoặc Notion | Dễ dùng, collaboration tốt |
| **API Docs** | Swagger/OpenAPI | Auto-generate từ code |
| **Diagrams** | draw.io, Mermaid | Free, version control |
| **Database** | dbdiagram.io | Visual, export SQL |
| **Code Repo** | GitHub hoặc GitLab | Version control, CI/CD |
| **Communication** | Slack hoặc Teams | Real-time chat |
| **Video Call** | Zoom hoặc Google Meet | Stable, recording |

---

### 6.4. Quy trình tạo tài liệu theo Sprint

#### Sprint 0 (Pre-project)
- [x] System Overview
- [x] Infrastructure & Team
- [x] SRS
- [x] SDS
- [x] Roadmap
- [ ] Project Charter
- [ ] Product Backlog

#### Sprint 1-6 (Development)
- [ ] Sprint Planning (đầu Sprint)
- [ ] Daily Standup notes
- [ ] Sprint Review (cuối Sprint)
- [ ] Sprint Retrospective (cuối Sprint)
- [ ] Code documentation (ongoing)
- [ ] API documentation (ongoing)
- [ ] Weekly status report

#### Pre-launch
- [ ] Deployment Guide
- [ ] Operations Manual
- [ ] User Manual
- [ ] Training Materials
- [ ] SLA

#### Post-launch
- [ ] Release Notes (mỗi release)
- [ ] Incident Reports (khi có)
- [ ] Performance Reports (monthly)

---

## 7. MẪU TÀI LIỆU

### 7.1. Sprint Planning Template

```markdown
# Sprint Planning - Sprint [Number]

**Date:** [Date]
**Duration:** 2 weeks
**Sprint Goal:** [Goal statement]

## Team Capacity
- Total capacity: [X] story points
- Team members: [List]
- Holidays/Leave: [If any]

## Selected User Stories

### Story 1: [Title]
- Story Points: [X]
- Assigned to: [Name]
- Tasks:
  - [ ] Task 1
  - [ ] Task 2

### Story 2: [Title]
...

## Risks
- [Risk 1]
- [Risk 2]

## Dependencies
- [Dependency 1]
```

---

### 7.2. Weekly Status Report Template

```markdown
# Weekly Status Report - Week [Number]

**Date:** [Start Date] - [End Date]
**Project:** Call Center SaaS Platform

## Summary
[Brief overview of the week]

## Accomplishments
- [Achievement 1]
- [Achievement 2]

## Upcoming (Next Week)
- [Plan 1]
- [Plan 2]

## Blockers
- [Blocker 1] - Owner: [Name]

## Risks
- [Risk 1] - Mitigation: [Action]

## Metrics
- Velocity: [X] story points
- Bugs: [X] open, [Y] closed
- Code coverage: [X]%

## Budget Status
- Planned: [X]M VND
- Actual: [Y]M VND
- Variance: [Z]%
```

---

### 7.3. Release Notes Template

```markdown
# Release Notes - Version [X.Y.Z]

**Release Date:** [Date]
**Release Type:** Major / Minor / Patch

## What's New
- [Feature 1]
- [Feature 2]

## Improvements
- [Improvement 1]
- [Improvement 2]

## Bug Fixes
- Fixed [Bug 1]
- Fixed [Bug 2]

## Breaking Changes
> [!WARNING]
> [Breaking change description]

## Migration Guide
[Steps to migrate from previous version]

## Known Issues
- [Issue 1]

## Contributors
- [Name 1]
- [Name 2]
```

---

## 8. BEST PRACTICES

### 8.1. Nguyên tắc tài liệu hóa

1. **Keep it Simple** - Viết đơn giản, dễ hiểu
2. **Keep it Updated** - Cập nhật thường xuyên
3. **Version Control** - Lưu trong Git
4. **Accessible** - Dễ tìm, dễ truy cập
5. **Collaborative** - Nhiều người cùng edit
6. **Visual** - Dùng diagrams, screenshots
7. **Searchable** - Có thể search được

---

### 8.2. Checklist trước mỗi Sprint

- [ ] Product Backlog refined
- [ ] Sprint Goal defined
- [ ] User Stories estimated
- [ ] Team capacity calculated
- [ ] Dependencies identified
- [ ] Definition of Done reviewed

---

### 8.3. Checklist cuối mỗi Sprint

- [ ] Sprint Review conducted
- [ ] Demo recorded
- [ ] Retrospective completed
- [ ] Action items documented
- [ ] Velocity calculated
- [ ] Incomplete stories moved to next Sprint

---

## PHỤ LỤC

### A. Tài liệu tham khảo

- [Scrum Guide](https://scrumguides.org/)
- [Agile Manifesto](https://agilemanifesto.org/)
- [Atlassian Agile Coach](https://www.atlassian.com/agile)

---

### B. Templates Download

Tất cả templates có thể tải tại:
- [Confluence Templates](https://www.atlassian.com/software/confluence/templates)
- [Azure DevOps Templates](https://docs.microsoft.com/en-us/azure/devops/)

---

**Ngày cập nhật:** 02/01/2026  
**Phiên bản:** 1.0  
**Tác giả:** Project Manager

---

> [!TIP]
> **Khuyến nghị:** Bắt đầu với các tài liệu cơ bản (Product Backlog, Sprint Planning), sau đó bổ sung dần các tài liệu khác theo nhu cầu thực tế của dự án.
