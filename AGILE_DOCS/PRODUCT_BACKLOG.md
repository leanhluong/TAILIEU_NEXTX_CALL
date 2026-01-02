# PRODUCT BACKLOG
## Call Center SaaS Platform

> [!IMPORTANT]
> Đây là Product Backlog chính của dự án. Tất cả User Stories được ưu tiên theo MoSCoW (Must have, Should have, Could have, Won't have).

**Project:** Call Center SaaS Platform  
**Product Owner:** [Tên PO]  
**Last Updated:** 02/01/2026  
**Version:** 1.0

---

## 📊 TỔNG QUAN

**Tổng số Stories:** 45  
**Total Story Points:** 250  
**Sprint Duration:** 2 tuần  
**Team Velocity:** ~40-45 points/sprint  
**Estimated Sprints:** 6 sprints (12 tuần)

---

## 🎯 EPIC OVERVIEW

| Epic ID | Epic Name | Stories | Story Points | Priority | Status |
|---------|-----------|---------|--------------|----------|--------|
| EP-01 | Authentication & Authorization | 5 | 25 | Must Have | 🔴 Not Started |
| EP-02 | Tenant Management | 6 | 30 | Must Have | 🔴 Not Started |
| EP-03 | Extension Management | 5 | 25 | Must Have | 🔴 Not Started |
| EP-04 | Call Handling | 8 | 45 | Must Have | 🔴 Not Started |
| EP-05 | IVR & Queue | 6 | 35 | Must Have | 🔴 Not Started |
| EP-06 | CDR & Recording | 5 | 25 | Must Have | 🔴 Not Started |
| EP-07 | Billing System | 4 | 20 | Must Have | 🔴 Not Started |
| EP-08 | Real-time Dashboard | 4 | 25 | Should Have | 🔴 Not Started |
| EP-09 | WebRTC Softphone | 2 | 20 | Should Have | 🔴 Not Started |

---

## 📋 PRODUCT BACKLOG (Prioritized)

### EPIC-01: Authentication & Authorization (25 points)

#### US-001: User Registration
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 2

**User Story:**
```
As a: Tenant Admin
I want: Đăng ký tài khoản mới cho công ty
So that: Tôi có thể sử dụng hệ thống Call Center
```

**Acceptance Criteria:**
- [ ] Email phải unique trong hệ thống
- [ ] Password tối thiểu 8 ký tự, có chữ hoa, số, ký tự đặc biệt
- [ ] Tự động tạo Tenant với domain slug từ company name
- [ ] Gửi email xác nhận đăng ký
- [ ] Hiển thị thông báo thành công với thông tin đăng nhập

**Technical Notes:**
- Hash password bằng BCrypt (cost 12)
- Validate email format
- Generate unique domain slug

**Dependencies:** None

---

#### US-002: User Login
**Priority:** Must Have  
**Story Points:** 3  
**Sprint:** Sprint 2

**User Story:**
```
As a: User (bất kỳ role nào)
I want: Đăng nhập vào hệ thống
So that: Tôi có thể truy cập các tính năng theo quyền của mình
```

**Acceptance Criteria:**
- [ ] Login với email + password
- [ ] Trả về JWT access token (expire 24h)
- [ ] Trả về refresh token (expire 7 days)
- [ ] Sai password 5 lần → Lock account 15 phút
- [ ] Log login history (IP, device, timestamp)

**Technical Notes:**
- JWT payload: user_id, email, role, tenant_id
- Use HttpOnly cookie cho refresh token

**Dependencies:** US-001

---

#### US-003: Role-Based Access Control (RBAC)
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 2

**User Story:**
```
As a: System
I want: Phân quyền theo role
So that: Mỗi user chỉ truy cập được tính năng phù hợp với vai trò
```

**Acceptance Criteria:**
- [ ] Implement 4 roles: SuperAdmin, TenantAdmin, Supervisor, Agent
- [ ] SuperAdmin: Full access tất cả Tenant
- [ ] TenantAdmin: Quản lý Tenant của mình
- [ ] Supervisor: Xem báo cáo team, nghe lén
- [ ] Agent: Chỉ xem dữ liệu của bản thân
- [ ] API endpoints có @Authorize decorator với policy

**Technical Notes:**
- Policy-based authorization trong .NET
- Global query filter cho tenant isolation

**Dependencies:** US-002

---

#### US-004: Refresh Token
**Priority:** Should Have  
**Story Points:** 5  
**Sprint:** Sprint 2

**User Story:**
```
As a: User
I want: Tự động refresh token khi hết hạn
So that: Tôi không bị logout giữa chừng khi đang làm việc
```

**Acceptance Criteria:**
- [ ] Endpoint `/api/auth/refresh-token`
- [ ] Validate refresh token từ database
- [ ] Generate new access token
- [ ] Rotate refresh token (security best practice)
- [ ] Revoke old refresh token

**Dependencies:** US-002

---

#### US-005: Logout
**Priority:** Must Have  
**Story Points:** 2  
**Sprint:** Sprint 2

**User Story:**
```
As a: User
I want: Đăng xuất khỏi hệ thống
So that: Bảo mật tài khoản khi không sử dụng
```

**Acceptance Criteria:**
- [ ] Revoke refresh token
- [ ] Clear client-side tokens
- [ ] Redirect về login page

**Dependencies:** US-002

---

### EPIC-02: Tenant Management (30 points)

#### US-006: Create Tenant (SuperAdmin)
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 2

**User Story:**
```
As a: SuperAdmin
I want: Tạo Tenant mới cho khách hàng
So that: Khách hàng có thể bắt đầu sử dụng hệ thống
```

**Acceptance Criteria:**
- [ ] Input: Company name, domain, admin email, max_agents, max_concurrent_calls
- [ ] Domain phải unique
- [ ] Tự động tạo TenantAdmin user
- [ ] Gửi email welcome với temporary password
- [ ] Set initial balance = 0

**Technical Notes:**
- Generate strong temporary password
- Domain format: lowercase, no spaces, alphanumeric + hyphen

**Dependencies:** US-003

---

#### US-007: View Tenant List (SuperAdmin)
**Priority:** Must Have  
**Story Points:** 3  
**Sprint:** Sprint 2

**User Story:**
```
As a: SuperAdmin
I want: Xem danh sách tất cả Tenant
So that: Tôi có thể quản lý khách hàng
```

**Acceptance Criteria:**
- [ ] Hiển thị: Name, Domain, Balance, Max Agents, Status, Created Date
- [ ] Pagination (20 items/page)
- [ ] Search by name/domain
- [ ] Filter by status (Active/Inactive)
- [ ] Sort by created date, balance

**Dependencies:** US-006

---

#### US-008: Update Tenant Quota
**Priority:** Must Have  
**Story Points:** 3  
**Sprint:** Sprint 2

**User Story:**
```
As a: SuperAdmin
I want: Cập nhật quota cho Tenant
So that: Tôi có thể scale up/down theo nhu cầu khách hàng
```

**Acceptance Criteria:**
- [ ] Update max_agents
- [ ] Update max_concurrent_calls
- [ ] Validate: Không giảm quota xuống dưới số đang sử dụng
- [ ] Gửi email thông báo cho Tenant Admin

**Dependencies:** US-006

---

#### US-009: Suspend/Activate Tenant
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 2

**User Story:**
```
As a: SuperAdmin
I want: Tạm ngưng hoặc kích hoạt Tenant
So that: Tôi có thể xử lý khách hàng vi phạm hoặc nợ tiền
```

**Acceptance Criteria:**
- [ ] Suspend: Set is_active = false
- [ ] Khi Suspend: Reject tất cả cuộc gọi
- [ ] Khi Suspend: Không cho login
- [ ] Activate: Set is_active = true
- [ ] Log reason và timestamp
- [ ] Gửi email thông báo

**Dependencies:** US-006

---

#### US-010: View Tenant Dashboard (TenantAdmin)
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 3

**User Story:**
```
As a: Tenant Admin
I want: Xem dashboard tổng quan của công ty
So that: Tôi biết tình hình hoạt động
```

**Acceptance Criteria:**
- [ ] Hiển thị: Current balance, Total agents, Active calls
- [ ] Today's stats: Total calls, Answered, Missed, Total cost
- [ ] Chart: Calls by hour (today)
- [ ] Top 5 agents by call volume
- [ ] Recent CDRs (10 latest)

**Dependencies:** US-006

---

#### US-011: Update Tenant Profile
**Priority:** Should Have  
**Story Points:** 3  
**Sprint:** Sprint 3

**User Story:**
```
As a: Tenant Admin
I want: Cập nhật thông tin công ty
So that: Thông tin luôn chính xác
```

**Acceptance Criteria:**
- [ ] Update: Company name, Address, Phone, Email
- [ ] Không cho đổi domain (immutable)
- [ ] Validate email format, phone format

**Dependencies:** US-006

---

### EPIC-03: Extension Management (25 points)

#### US-012: Create Extension
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 2

**User Story:**
```
As a: Tenant Admin
I want: Tạo Extension (số máy nhánh) cho nhân viên
So that: Nhân viên có thể đăng nhập SIP và gọi điện
```

**Acceptance Criteria:**
- [ ] Input: Extension number (3-5 digits), Agent name, Email
- [ ] Extension unique trong Tenant
- [ ] Auto-generate SIP password (12 ký tự random)
- [ ] Return SIP credentials (username, password, domain, server)
- [ ] Gửi email cho Agent với SIP info

**Technical Notes:**
- SIP domain format: `{tenant_domain}.pbx.local`
- SIP server: `sip.callcenter-saas.com`

**Dependencies:** US-006

---

#### US-013: View Extension List
**Priority:** Must Have  
**Story Points:** 3  
**Sprint:** Sprint 2

**User Story:**
```
As a: Tenant Admin
I want: Xem danh sách Extension
So that: Tôi biết có bao nhiêu nhân viên
```

**Acceptance Criteria:**
- [ ] Hiển thị: Extension, Agent name, Email, Status, Created date
- [ ] Filter: Active/Inactive
- [ ] Search by extension/name/email
- [ ] Pagination

**Dependencies:** US-012

---

#### US-014: Update Extension
**Priority:** Must Have  
**Story Points:** 3  
**Sprint:** Sprint 2

**User Story:**
```
As a: Tenant Admin
I want: Cập nhật thông tin Extension
So that: Thông tin luôn chính xác
```

**Acceptance Criteria:**
- [ ] Update: Agent name, Email
- [ ] Không cho đổi Extension number (immutable)
- [ ] Reset SIP password (optional)
- [ ] Gửi email nếu đổi password

**Dependencies:** US-012

---

#### US-015: Delete Extension (Soft Delete)
**Priority:** Must Have  
**Story Points:** 3  
**Sprint:** Sprint 2

**User Story:**
```
As a: Tenant Admin
I want: Xóa Extension khi nhân viên nghỉ việc
So that: Họ không thể đăng nhập nữa
```

**Acceptance Criteria:**
- [ ] Soft delete: Set is_deleted = true
- [ ] Kick SIP registration (nếu đang online)
- [ ] Giữ lại CDR history
- [ ] Không cho phục hồi (business rule)

**Dependencies:** US-012

---

#### US-016: Extension SIP Registration
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 2

**User Story:**
```
As a: Agent
I want: Đăng ký SIP với Extension của mình
So that: Tôi có thể nhận và gọi điện thoại
```

**Acceptance Criteria:**
- [ ] FreeSWITCH gọi API `/api/freeswitch/configuration?section=directory`
- [ ] API trả về XML với user credentials
- [ ] Validate password
- [ ] Return user variables (tenant_id, extension_id)
- [ ] Response time < 200ms

**Technical Notes:**
- Implement mod_xml_curl handler
- Cache XML response trong Redis (TTL 10 phút)

**Dependencies:** US-012

---

### EPIC-04: Call Handling (45 points)

#### US-017: Internal Call (Extension to Extension)
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 2

**User Story:**
```
As a: Agent
I want: Gọi cho Extension khác trong công ty
So that: Tôi có thể liên lạc với đồng nghiệp
```

**Acceptance Criteria:**
- [ ] Agent gọi số Extension (VD: 102)
- [ ] FreeSWITCH query API để lấy dialplan
- [ ] API trả về XML bridge 2 Extension
- [ ] Cuộc gọi kết nối
- [ ] Không tính cước (cost = 0)
- [ ] Lưu CDR với direction = "internal"

**Technical Notes:**
- Dialplan: Bridge trực tiếp, không qua Trunk

**Dependencies:** US-016

---

#### US-018: Outbound Call
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 4

**User Story:**
```
As a: Agent
I want: Gọi ra số điện thoại bên ngoài
So that: Tôi có thể liên hệ khách hàng
```

**Acceptance Criteria:**
- [ ] Agent gọi số ngoài (VD: 0909123456)
- [ ] Check Balance > 0
- [ ] Check Concurrent calls < Quota
- [ ] Route qua SIP Trunk
- [ ] Cuộc gọi kết nối
- [ ] Start billing timer
- [ ] Lưu CDR với direction = "outbound"

**Technical Notes:**
- Dialplan: Bridge to `sofia/gateway/trunk_provider/$1`

**Dependencies:** US-016, US-030 (SIP Trunk)

---

#### US-019: Inbound Call
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 5

**User Story:**
```
As a: Customer
I want: Gọi vào số hotline của công ty
So that: Tôi được hỗ trợ
```

**Acceptance Criteria:**
- [ ] Caller gọi DID (VD: 1900xxxx)
- [ ] FreeSWITCH query API: Lookup DID → Tenant → Routing
- [ ] Nếu có IVR → Play IVR
- [ ] Nếu có Queue → Add to Queue
- [ ] Nếu Direct Extension → Bridge
- [ ] Lưu CDR với direction = "inbound"

**Dependencies:** US-016, US-024 (IVR), US-025 (Queue)

---

#### US-020: Call Transfer (Blind)
**Priority:** Should Have  
**Story Points:** 5  
**Sprint:** Sprint 3

**User Story:**
```
As a: Agent
I want: Chuyển cuộc gọi sang Extension khác
So that: Người phù hợp hơn xử lý
```

**Acceptance Criteria:**
- [ ] Agent bấm transfer + số Extension
- [ ] FreeSWITCH execute `uuid_transfer`
- [ ] Cuộc gọi chuyển sang Extension mới
- [ ] Update CDR: transfer_to field

**Dependencies:** US-017

---

#### US-021: Call Hold/Resume
**Priority:** Should Have  
**Story Points:** 3  
**Sprint:** Sprint 3

**User Story:**
```
As a: Agent
I want: Giữ máy tạm thời
So that: Tôi có thể tra cứu thông tin
```

**Acceptance Criteria:**
- [ ] Agent click Hold
- [ ] Caller nghe nhạc chờ
- [ ] Agent click Resume
- [ ] Cuộc gọi tiếp tục

**Technical Notes:**
- FreeSWITCH: `uuid_hold` / `uuid_unhold`

**Dependencies:** US-017

---

#### US-022: Call Recording
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 5

**User Story:**
```
As a: Tenant Admin
I want: Tự động ghi âm tất cả cuộc gọi
So that: Tôi có thể review chất lượng
```

**Acceptance Criteria:**
- [ ] Tenant setting: auto_record = true/false
- [ ] FreeSWITCH record to `/var/lib/freeswitch/recordings/{tenant_id}/{date}/{uuid}.wav`
- [ ] Worker Service quét file mỗi 1 phút
- [ ] Convert WAV → MP3 (ffmpeg)
- [ ] Upload lên MinIO
- [ ] Update CDR.recording_url
- [ ] Xóa file local

**Dependencies:** US-017, US-018, US-019

---

#### US-023: Conference Call (3-way)
**Priority:** Could Have  
**Story Points:** 8  
**Sprint:** Sprint 6

**User Story:**
```
As a: Agent
I want: Tạo cuộc gọi hội nghị 3 người
So that: Tôi, khách hàng và chuyên gia cùng bàn luận
```

**Acceptance Criteria:**
- [ ] Agent đang gọi với Caller A
- [ ] Agent gọi thêm Caller B
- [ ] Cả 3 người cùng nghe và nói
- [ ] Ghi âm cả 3 người

**Dependencies:** US-017, US-020

---

### EPIC-05: IVR & Queue (35 points)

#### US-024: IVR Builder
**Priority:** Must Have  
**Story Points:** 13  
**Sprint:** Sprint 6

**User Story:**
```
As a: Tenant Admin
I want: Tạo kịch bản IVR (bấm phím)
So that: Khách hàng tự chọn bộ phận cần gặp
```

**Acceptance Criteria:**
- [ ] UI: Drag & drop flow builder (React Flow)
- [ ] Nodes: Play Audio, Get Digits, Condition, Transfer, Hangup
- [ ] Upload audio files (MP3/WAV)
- [ ] Save flow as JSON
- [ ] Preview flow
- [ ] Assign IVR to DID

**Technical Notes:**
- Store flow in `ivrs.flow` (JSONB column)

**Dependencies:** US-006

---

#### US-025: Queue Management
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 6

**User Story:**
```
As a: Tenant Admin
I want: Tạo Queue (hàng đợi) cho từng bộ phận
So that: Cuộc gọi được phân phối đều cho Agent
```

**Acceptance Criteria:**
- [ ] Create Queue: Name, Strategy (ring-all/round-robin/longest-idle)
- [ ] Set max wait time
- [ ] Upload Music on Hold file
- [ ] Add/Remove Agents to Queue
- [ ] Set Agent priority

**Dependencies:** US-012

---

#### US-026: IVR Execution
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 6

**User Story:**
```
As a: System
I want: Thực thi IVR flow
So that: Caller được định tuyến đúng
```

**Acceptance Criteria:**
- [ ] Parse IVR JSON flow
- [ ] Generate FreeSWITCH XML dialplan
- [ ] Play audio files
- [ ] Capture DTMF input
- [ ] Execute conditions
- [ ] Transfer to Extension/Queue

**Dependencies:** US-024

---

#### US-027: Queue Call Distribution
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 6

**User Story:**
```
As a: System
I want: Phân phối cuộc gọi trong Queue
So that: Agent nhàn rỗi nhất nhận cuộc gọi
```

**Acceptance Criteria:**
- [ ] Caller vào Queue
- [ ] Play Music on Hold
- [ ] Ring Agent theo strategy
- [ ] Timeout mỗi Agent: 15s
- [ ] Nếu không có Agent available → Voicemail
- [ ] Max wait time: 300s

**Technical Notes:**
- Use FreeSWITCH mod_callcenter

**Dependencies:** US-025

---

### EPIC-06: CDR & Recording (25 points)

#### US-028: Generate CDR
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 5

**User Story:**
```
As a: System
I want: Tự động tạo CDR sau mỗi cuộc gọi
So that: Có dữ liệu để báo cáo và billing
```

**Acceptance Criteria:**
- [ ] Worker Service listen ESL event `CHANNEL_HANGUP_COMPLETE`
- [ ] Parse variables: uuid, caller, callee, start_time, answer_time, end_time, duration, billsec, hangup_cause
- [ ] Insert vào bảng `call_detail_records`
- [ ] Calculate cost (nếu outbound)

**Dependencies:** US-017, US-018, US-019

---

#### US-029: View CDR Report
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 5

**User Story:**
```
As a: Tenant Admin
I want: Xem báo cáo CDR
So that: Tôi biết tình hình cuộc gọi
```

**Acceptance Criteria:**
- [ ] Filters: Date range, Direction, Agent, Status
- [ ] Display: Caller, Callee, Start time, Duration, Cost, Status
- [ ] Pagination
- [ ] Export CSV/Excel
- [ ] Statistics: Total calls, Answered, Missed, Total cost

**Dependencies:** US-028

---

#### US-030: Play Recording
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 5

**User Story:**
```
As a: Tenant Admin/Supervisor
I want: Nghe lại file ghi âm
So that: Tôi có thể review chất lượng
```

**Acceptance Criteria:**
- [ ] Click "Play" button trong CDR
- [ ] Generate presigned URL từ MinIO (expire 1h)
- [ ] Play audio inline (HTML5 audio player)
- [ ] Download recording

**Dependencies:** US-022, US-028

---

#### US-031: CDR Charts
**Priority:** Should Have  
**Story Points:** 5  
**Sprint:** Sprint 10

**User Story:**
```
As a: Tenant Admin
I want: Xem biểu đồ thống kê cuộc gọi
So that: Tôi dễ hình dung xu hướng
```

**Acceptance Criteria:**
- [ ] Chart: Calls by hour (bar chart)
- [ ] Chart: Answer rate (pie chart)
- [ ] Chart: Duration distribution (histogram)
- [ ] Chart: Calls by agent (bar chart)

**Dependencies:** US-028

---

### EPIC-07: Billing System (20 points)

#### US-032: Rate Table Management
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 4

**User Story:**
```
As a: Tenant Admin
I want: Quản lý bảng giá cước
So that: Tôi biết mỗi cuộc gọi tốn bao nhiêu
```

**Acceptance Criteria:**
- [ ] Create rate: Prefix, Rate per minute, Billing increment
- [ ] View rate list
- [ ] Update rate
- [ ] Delete rate

**Dependencies:** US-006

---

#### US-033: Calculate Call Cost
**Priority:** Must Have  
**Story Points:** 8  
**Sprint:** Sprint 5

**User Story:**
```
As a: System
I want: Tính cước tự động sau cuộc gọi
So that: Trừ tiền từ balance
```

**Acceptance Criteria:**
- [ ] Lookup rate by prefix
- [ ] Formula: `cost = CEIL(billsec / increment) * increment / 60 * rate`
- [ ] Deduct from tenant balance
- [ ] Save to billing_transactions
- [ ] Update CDR.cost

**Dependencies:** US-028, US-032

---

#### US-034: Balance Management
**Priority:** Must Have  
**Story Points:** 5  
**Sprint:** Sprint 4

**User Story:**
```
As a: Tenant Admin
I want: Nạp tiền vào ví
So that: Tôi có thể gọi điện
```

**Acceptance Criteria:**
- [ ] View current balance
- [ ] Top-up (manual by SuperAdmin)
- [ ] View transaction history
- [ ] Low balance alert (< 100,000 VND)

**Dependencies:** US-006

---

#### US-035: Block Calls When Balance = 0
**Priority:** Must Have  
**Story Points:** 3  
**Sprint:** Sprint 4

**User Story:**
```
As a: System
I want: Chặn cuộc gọi outbound khi hết tiền
So that: Tenant không bị âm balance
```

**Acceptance Criteria:**
- [ ] Check balance trước khi bridge
- [ ] Nếu balance <= 0 → Play "Tài khoản hết tiền" → Hangup
- [ ] Gửi email alert cho Tenant Admin

**Dependencies:** US-034

---

### EPIC-08: Real-time Dashboard (25 points)

#### US-036: Live Call Monitor
**Priority:** Should Have  
**Story Points:** 8  
**Sprint:** Sprint 7

**User Story:**
```
As a: Tenant Admin/Supervisor
I want: Xem cuộc gọi đang diễn ra real-time
So that: Tôi biết ai đang bận
```

**Acceptance Criteria:**
- [ ] Display: Agent, Caller ID, Duration (counter), Status
- [ ] Auto-update mỗi 1s (SignalR)
- [ ] Color code: Green (Talking), Yellow (Ringing), Red (Hold)

**Dependencies:** US-017, US-018, US-019

---

#### US-037: Agent Status Board
**Priority:** Should Have  
**Story Points:** 8  
**Sprint:** Sprint 7

**User Story:**
```
As a: Supervisor
I want: Xem trạng thái tất cả Agent
So that: Tôi biết ai available, ai busy
```

**Acceptance Criteria:**
- [ ] Display: Agent grid với status
- [ ] Status: 🟢 Available, 🔴 Busy, 🟡 Break, ⚫ Offline
- [ ] Agent tự set status
- [ ] Auto-update real-time

**Dependencies:** US-012

---

#### US-038: Real-time Statistics
**Priority:** Should Have  
**Story Points:** 5  
**Sprint:** Sprint 7

**User Story:**
```
As a: Tenant Admin
I want: Xem thống kê real-time
So that: Tôi biết tình hình hiện tại
```

**Acceptance Criteria:**
- [ ] Active calls count
- [ ] Calls today (total, answered, missed)
- [ ] Average wait time
- [ ] Auto-update real-time

**Dependencies:** US-028

---

#### US-039: Call Notifications
**Priority:** Should Have  
**Story Points:** 5  
**Sprint:** Sprint 7

**User Story:**
```
As a: Supervisor
I want: Nhận thông báo khi có cuộc gọi mới
So that: Tôi không bỏ lỡ
```

**Acceptance Criteria:**
- [ ] Toast notification khi call started
- [ ] Sound alert (optional)
- [ ] Browser notification (optional)

**Dependencies:** US-036

---

### EPIC-09: WebRTC Softphone (20 points)

#### US-040: WebRTC Softphone
**Priority:** Should Have  
**Story Points:** 13  
**Sprint:** Sprint 9

**User Story:**
```
As a: Agent
I want: Gọi điện trực tiếp trên trình duyệt
So that: Tôi không cần cài IP Phone
```

**Acceptance Criteria:**
- [ ] Dial pad UI
- [ ] Make call
- [ ] Answer call
- [ ] Hangup
- [ ] Hold/Resume
- [ ] Mute/Unmute
- [ ] Call timer
- [ ] Audio level indicator

**Technical Notes:**
- Use JsSIP library
- FreeSWITCH WebSocket (wss://)
- Opus codec

**Dependencies:** US-016

---

#### US-041: Incoming Call Popup
**Priority:** Should Have  
**Story Points:** 5  
**Sprint:** Sprint 9

**User Story:**
```
As a: Agent
I want: Popup khi có cuộc gọi vào
So that: Tôi không bỏ lỡ
```

**Acceptance Criteria:**
- [ ] Popup hiển thị Caller ID
- [ ] Ringtone
- [ ] Answer/Reject buttons
- [ ] Auto-popup on top

**Dependencies:** US-040

---

## 📊 SPRINT ALLOCATION (Dự kiến)

| Sprint | Duration | Stories | Story Points | Focus |
|--------|----------|---------|--------------|-------|
| **Sprint 1** | 06-19/01 | Infrastructure | 0 | DevOps setup |
| **Sprint 2** | 20/01-02/02 | US-001 to US-016 | 45 | Auth, Tenant, Extension |
| **Sprint 3** | 03-16/02 | US-010, US-011, US-020, US-021 | 19 | Dashboard, Transfer |
| **Sprint 4** | 17/02-02/03 | US-018, US-030, US-032, US-034, US-035 | 24 | Outbound, Billing |
| **Sprint 5** | 03-16/03 | US-019, US-022, US-028, US-029, US-033 | 37 | Inbound, CDR, Recording |
| **Sprint 6** | 17/03-30/03 | US-023, US-024, US-025, US-026, US-027 | 37 | IVR, Queue, Conference |
| **Sprint 7** | (Future) | US-036 to US-039 | 26 | Real-time Dashboard |
| **Sprint 9** | (Future) | US-040, US-041 | 18 | WebRTC Softphone |

---

## 🎯 DEFINITION OF READY (DoR)

Story được coi là Ready khi:
- [ ] User story rõ ràng (As a, I want, So that)
- [ ] Acceptance criteria đầy đủ
- [ ] Story points đã estimate
- [ ] Dependencies đã identify
- [ ] Technical notes (nếu cần)
- [ ] Product Owner approved

---

## ✅ DEFINITION OF DONE (DoD)

Story được coi là Done khi:
- [ ] Code implemented
- [ ] Unit tests written (coverage > 70%)
- [ ] Code reviewed và approved
- [ ] Integration tests passed
- [ ] QA tested và approved
- [ ] Documentation updated
- [ ] Deployed to staging
- [ ] Product Owner accepted

---

## 📝 NOTES

### Prioritization Method: MoSCoW
- **Must Have:** Critical cho MVP
- **Should Have:** Quan trọng nhưng có thể defer
- **Could Have:** Nice to have
- **Won't Have:** Không làm trong MVP

### Story Points Scale: Fibonacci
- 1: Trivial (< 2 hours)
- 2: Simple (2-4 hours)
- 3: Medium (4-8 hours)
- 5: Complex (1-2 days)
- 8: Very Complex (2-3 days)
- 13: Epic (3-5 days)
- 20+: Too big, need to break down

---

**Last Updated:** 02/01/2026  
**Next Review:** Sprint Planning Sprint 2 (20/01/2026)  
**Product Owner:** [Tên PO]
