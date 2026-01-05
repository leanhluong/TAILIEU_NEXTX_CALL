# KIẾN TRÚC CHI TIẾT VÀ LUỒNG GỌI
## Call Center SaaS Platform - Architecture Deep Dive

> [!IMPORTANT]
> Tài liệu này giải thích chi tiết về kiến trúc hệ thống, luồng gọi điện, và cách các thành phần tương tác với nhau.

**Phiên bản:** 1.0  
**Ngày tạo:** 05/01/2026  
**Mục đích:** Làm rõ kiến trúc và luồng hoạt động

---

## MỤC LỤC

1. [Kiến trúc tổng quan chính xác](#1-kiến-trúc-tổng-quan-chính-xác)
2. [Database Architecture](#2-database-architecture)
3. [Luồng gọi điện chi tiết](#3-luồng-gọi-điện-chi-tiết)
4. [Tương tác giữa các thành phần](#4-tương-tác-giữa-các-thành-phần)
5. [So sánh các phương thức gọi](#5-so-sánh-các-phương-thức-gọi)

---

## 1. KIẾN TRÚC TỔNG QUAN CHÍNH XÁC

### 1.1. Sơ đồ kiến trúc đầy đủ

```mermaid
graph TB
    subgraph "Client Layer"
        C1[Web Browser<br/>React App]
        C2[Mobile App<br/>React Native]
        C3[IP Phone<br/>Physical/Softphone]
    end
    
    subgraph "Application Layer - Cloud Server"
        subgraph "Frontend"
            F1[React Web App<br/>Port 3000]
            F2[React Native App]
        end
        
        subgraph "Backend - .NET 8"
            B1[Web API<br/>Port 5000]
            B2[Worker Service<br/>ESL Listener]
            B3[SignalR Hub<br/>WebSocket]
            B4[mod_xml_curl Handler<br/>Port 5000/api/freeswitch]
        end
        
        subgraph "Telephony Engine"
            T1[FreeSWITCH<br/>Port 5060 SIP]
            T2[ESL Port 8021]
            T3[WebSocket Port 8082]
        end
    end
    
    subgraph "Data Layer - Same Server"
        D1[(PostgreSQL<br/>Port 5432<br/>SINGLE DATABASE)]
        D2[(Redis<br/>Port 6379<br/>Cache)]
        D3[MinIO S3<br/>Port 9000<br/>Recordings]
    end
    
    subgraph "External"
        E1[SIP Trunk Provider<br/>Viettel/FPT/VNPT]
        E2[PSTN/Mobile Network]
    end
    
    %% Client connections
    C1 -.HTTP/HTTPS.-> F1
    C2 -.HTTP/HTTPS.-> F2
    C3 -.SIP 5060.-> T1
    
    %% Frontend to Backend
    F1 -.REST API.-> B1
    F2 -.REST API.-> B1
    F1 -.WebSocket.-> B3
    
    %% FreeSWITCH to Backend
    T1 -.mod_xml_curl<br/>HTTP.-> B4
    T2 -.ESL Events.-> B2
    T3 -.WebRTC<br/>WebSocket.-> F1
    
    %% Backend to Data
    B1 --> D1
    B1 --> D2
    B2 --> D1
    B2 --> D3
    B4 --> D1
    B4 --> D2
    
    %% FreeSWITCH to External
    T1 -.SIP Trunk.-> E1
    E1 --> E2
    
    style D1 fill:#ff6b6b,color:#fff
    style T1 fill:#4ecdc4,color:#fff
    style B1 fill:#95e1d3,color:#000
```

### 1.2. Giải thích các thành phần

#### **Client Layer (Người dùng)**

**1. Web Browser (React App)**
- Truy cập qua HTTPS
- Giao diện quản lý (Dashboard, CDR, Reports)
- **WebRTC Softphone** (gọi điện trên browser)
- Kết nối SignalR để nhận real-time updates

**2. Mobile App (React Native)**
- Tương tự Web App
- Push notifications
- Mobile-optimized UI

**3. IP Phone (Physical/Softphone)**
- Đăng ký SIP trực tiếp vào FreeSWITCH
- Không qua Backend API
- VD: Yealink, Cisco, Zoiper, Linphone

---

#### **Application Layer (Cloud Server)**

**Backend - .NET 8:**

**1. Web API (Port 5000)**
- REST API cho Frontend
- CRUD operations (Tenant, User, Extension, etc.)
- Authentication & Authorization (JWT)
- Business logic

**2. Worker Service (Background Service)**
- Lắng nghe ESL events từ FreeSWITCH
- Xử lý CDR (lưu vào database)
- Xử lý Recording (convert, upload MinIO)
- Billing calculation

**3. SignalR Hub (WebSocket)**
- Real-time communication
- Push events đến Frontend
- Live call monitoring
- Agent status updates

**4. mod_xml_curl Handler**
- Endpoint đặc biệt cho FreeSWITCH
- FreeSWITCH gọi HTTP đến endpoint này
- Trả về XML configuration
- **2 loại request:**
  - `section=directory` → User authentication
  - `section=dialplan` → Call routing

---

**Telephony Engine:**

**FreeSWITCH:**
- **Port 5060:** SIP (IP Phone đăng ký vào đây)
- **Port 8021:** ESL (Event Socket Layer) - Backend lắng nghe events
- **Port 8082:** WebSocket (WebRTC cho browser)

**Modules quan trọng:**
- `mod_xml_curl` → Hỏi Backend API để lấy config
- `mod_event_socket` → Gửi events đến Backend
- `mod_sofia` → SIP stack
- `mod_callcenter` → Queue/ACD

---

#### **Data Layer (Cùng server)**

**1. PostgreSQL (Port 5432) - SINGLE DATABASE**
> [!IMPORTANT]
> **CHỈ CÓ 1 DATABASE DUY NHẤT** lưu TẤT CẢ dữ liệu:

**Tables chính:**
```
Tenants                 → Thông tin khách hàng
Users                   → Người dùng
Extensions              → Số máy nhánh (101, 102, ...)
Trunks                  → Kết nối SIP Trunk
DIDs                    → Đầu số (hotline)
IVRs                    → IVR flows
Queues                  → Hàng đợi
CDRs                    → Call Detail Records
Recordings              → Metadata ghi âm
Transactions            → Lịch sử giao dịch
RateTables              → Bảng giá cước
Balances                → Số dư tài khoản
```

**2. Redis (Port 6379)**
- Cache XML responses (mod_xml_curl)
- Session storage
- Pub/Sub cho SignalR
- Rate limiting

**3. MinIO S3 (Port 9000)**
- Lưu file ghi âm (MP3)
- Lưu audio files cho IVR
- Object storage

---

## 2. DATABASE ARCHITECTURE

### 2.1. Câu trả lời cho câu hỏi của bạn

> **Câu hỏi:** "Tổng kết là mình chỉ có 1 database server để lưu chữ thôi chứ đúng k? như kiểu tất cả mình lưu đầu số, truck .... hay 2 database ở server tổng đài và server cloud của mình là khác nhau?"

**Trả lời:**

✅ **ĐÚNG! Chỉ có 1 DATABASE duy nhất (PostgreSQL)**

**Lý do:**
1. **Đơn giản hóa:** Không cần sync data giữa 2 database
2. **Consistency:** Dữ liệu luôn đồng nhất
3. **Cost-effective:** Tiết kiệm chi phí
4. **Easier backup:** Chỉ cần backup 1 database

**FreeSWITCH KHÔNG CÓ DATABASE:**
- FreeSWITCH chỉ là **telephony engine** (xử lý cuộc gọi)
- Mọi thông tin đều lấy từ Backend API (qua mod_xml_curl)
- FreeSWITCH **stateless** (không lưu trữ gì)

### 2.2. Sơ đồ Data Flow

```mermaid
graph LR
    subgraph "Single PostgreSQL Database"
        DB[(PostgreSQL<br/>Port 5432)]
    end
    
    subgraph "Applications"
        API[.NET Web API]
        Worker[Worker Service]
        FS_Handler[mod_xml_curl Handler]
    end
    
    subgraph "FreeSWITCH"
        FS[FreeSWITCH Engine<br/>NO DATABASE]
    end
    
    API -->|Read/Write| DB
    Worker -->|Read/Write| DB
    FS_Handler -->|Read| DB
    
    FS -.HTTP Request.-> FS_Handler
    FS_Handler -.XML Response.-> FS
    
    FS -.ESL Events.-> Worker
    
    style DB fill:#ff6b6b,color:#fff
    style FS fill:#4ecdc4,color:#fff
```

**Giải thích:**
1. **Web API** → CRUD operations vào PostgreSQL
2. **Worker Service** → Lưu CDR, Billing vào PostgreSQL
3. **mod_xml_curl Handler** → ĐỌC từ PostgreSQL, trả XML cho FreeSWITCH
4. **FreeSWITCH** → KHÔNG lưu gì, chỉ hỏi Backend

---

## 3. LUỒNG GỌI ĐIỆN CHI TIẾT

### 3.1. Phương thức 1: IP Phone → FreeSWITCH (Traditional SIP)

```mermaid
sequenceDiagram
    participant IP as IP Phone<br/>(Zoiper, Yealink)
    participant FS as FreeSWITCH
    participant API as mod_xml_curl Handler
    participant DB as PostgreSQL
    participant Worker as Worker Service
    
    Note over IP,DB: 1. REGISTRATION (Đăng ký SIP)
    IP->>FS: REGISTER sip:101@domain.com
    FS->>API: HTTP GET /api/freeswitch/configuration?<br/>section=directory&user=101
    API->>DB: SELECT * FROM Extensions WHERE Number='101'
    DB-->>API: Extension data (password, etc.)
    API-->>FS: XML Response (user config)
    FS-->>IP: 200 OK (Registered)
    
    Note over IP,DB: 2. MAKE CALL (Gọi điện)
    IP->>FS: INVITE sip:0901234567@domain.com
    FS->>API: HTTP GET /api/freeswitch/configuration?<br/>section=dialplan&destination=0901234567
    API->>DB: SELECT * FROM Tenants, RateTables, Balances
    DB-->>API: Routing info, balance check
    API-->>FS: XML Response (dialplan)
    FS->>FS: Execute dialplan
    FS->>SIP_Trunk: INVITE sip:0901234567@trunk.provider.com
    SIP_Trunk-->>FS: 200 OK (Connected)
    FS-->>IP: 200 OK (Call connected)
    
    Note over IP,DB: 3. CALL ENDS (Kết thúc)
    IP->>FS: BYE
    FS->>Worker: ESL Event: CHANNEL_HANGUP_COMPLETE<br/>(billsec, duration, etc.)
    Worker->>DB: INSERT INTO CDRs (...)
    Worker->>DB: UPDATE Balances SET amount = amount - cost
    Worker->>DB: INSERT INTO Transactions (...)
```

**Giải thích từng bước:**

**Bước 1: Registration (Đăng ký)**
1. IP Phone gửi REGISTER đến FreeSWITCH
2. FreeSWITCH hỏi Backend: "User 101 có tồn tại không? Password là gì?"
3. Backend query PostgreSQL
4. Backend trả XML cho FreeSWITCH
5. FreeSWITCH xác thực và cho phép đăng ký

**Bước 2: Make Call (Gọi điện)**
1. IP Phone gửi INVITE (gọi số 0901234567)
2. FreeSWITCH hỏi Backend: "Số này route như thế nào? User có đủ tiền không?"
3. Backend check balance, rate table
4. Backend trả dialplan XML
5. FreeSWITCH thực hiện cuộc gọi qua SIP Trunk

**Bước 3: Call Ends (Kết thúc)**
1. Cuộc gọi kết thúc
2. FreeSWITCH gửi event qua ESL
3. Worker Service nhận event
4. Worker lưu CDR vào database
5. Worker trừ tiền từ balance
6. Worker lưu transaction history

---

### 3.2. Phương thức 2: Web/Mobile → Backend API → FreeSWITCH (Click-to-Call)

```mermaid
sequenceDiagram
    participant User as User<br/>(Web/Mobile)
    participant FE as Frontend<br/>(React)
    participant API as Backend API
    participant DB as PostgreSQL
    participant FS as FreeSWITCH
    participant Worker as Worker Service
    
    Note over User,Worker: 1. USER CLICKS "CALL" BUTTON
    User->>FE: Click "Call 0901234567"
    FE->>API: POST /api/calls/make-call<br/>{from: "101", to: "0901234567"}
    
    Note over User,Worker: 2. BACKEND VALIDATES & SENDS TO FREESWITCH
    API->>DB: Check balance, permissions
    DB-->>API: Balance OK
    API->>FS: ESL Command: originate<br/>{endpoint=user/101}<br/>{exten=0901234567}
    
    Note over User,Worker: 3. FREESWITCH EXECUTES CALL
    FS->>FS: Call Extension 101
    FS->>IP_Phone: INVITE (ring phone 101)
    IP_Phone-->>FS: 200 OK (user answers)
    FS->>SIP_Trunk: INVITE 0901234567
    SIP_Trunk-->>FS: 200 OK (connected)
    FS-->>API: ESL Response: Call UUID
    API-->>FE: 200 OK {callId: "uuid-123"}
    FE-->>User: "Calling..."
    
    Note over User,Worker: 4. REAL-TIME UPDATES (SignalR)
    FS->>Worker: ESL Event: CHANNEL_ANSWER
    Worker->>SignalR: Broadcast "CallAnswered"
    SignalR-->>FE: WebSocket: {event: "CallAnswered"}
    FE-->>User: "Connected!"
    
    Note over User,Worker: 5. CALL ENDS
    FS->>Worker: ESL Event: CHANNEL_HANGUP_COMPLETE
    Worker->>DB: Save CDR, deduct balance
    Worker->>SignalR: Broadcast "CallEnded"
    SignalR-->>FE: WebSocket: {event: "CallEnded"}
    FE-->>User: "Call ended. Duration: 5:30"
```

**Giải thích từng bước:**

**Bước 1: User clicks "Call"**
- User click button trên Web/Mobile
- Frontend gọi API `POST /api/calls/make-call`

**Bước 2: Backend validates**
- Backend check balance trong PostgreSQL
- Backend gửi lệnh ESL đến FreeSWITCH: "Gọi từ 101 đến 0901234567"

**Bước 3: FreeSWITCH executes**
- FreeSWITCH gọi điện thoại của Extension 101
- User nhấc máy
- FreeSWITCH gọi ra ngoài qua SIP Trunk
- Cuộc gọi kết nối

**Bước 4: Real-time updates**
- FreeSWITCH gửi events (ANSWER, HANGUP) qua ESL
- Worker Service nhận events
- Worker broadcast qua SignalR
- Frontend nhận WebSocket và update UI

**Bước 5: Call ends**
- Lưu CDR, trừ tiền, update UI

---

### 3.3. Phương thức 3: WebRTC Softphone (Browser)

```mermaid
sequenceDiagram
    participant Browser as Web Browser
    participant JsSIP as JsSIP Library
    participant FS_WS as FreeSWITCH<br/>WebSocket
    participant FS as FreeSWITCH Core
    participant API as mod_xml_curl
    participant DB as PostgreSQL
    
    Note over Browser,DB: 1. REGISTER via WebSocket
    Browser->>JsSIP: user.register()
    JsSIP->>FS_WS: WebSocket: REGISTER
    FS_WS->>FS: SIP REGISTER
    FS->>API: HTTP: section=directory
    API->>DB: SELECT Extension
    DB-->>API: Extension data
    API-->>FS: XML user config
    FS-->>FS_WS: 200 OK
    FS_WS-->>JsSIP: WebSocket: 200 OK
    JsSIP-->>Browser: Registered!
    
    Note over Browser,DB: 2. MAKE CALL
    Browser->>JsSIP: session.call("0901234567")
    JsSIP->>FS_WS: WebSocket: INVITE
    FS_WS->>FS: SIP INVITE
    FS->>API: HTTP: section=dialplan
    API->>DB: Check balance, routing
    DB-->>API: Dialplan
    API-->>FS: XML dialplan
    FS->>SIP_Trunk: INVITE 0901234567
    SIP_Trunk-->>FS: 200 OK
    FS-->>FS_WS: 200 OK + SDP
    FS_WS-->>JsSIP: WebSocket: 200 OK
    JsSIP-->>Browser: Call connected!
    Browser->>Browser: Play audio (WebRTC)
```

**Giải thích:**
- Browser sử dụng **JsSIP library** (WebRTC)
- Kết nối đến FreeSWITCH qua **WebSocket** (port 8082)
- Sau đó giống như IP Phone (FreeSWITCH hỏi Backend qua mod_xml_curl)
- Audio stream qua WebRTC (peer-to-peer)

---

## 4. TƯƠNG TÁC GIỮA CÁC THÀNH PHẦN

### 4.1. FreeSWITCH ↔ Backend API

**mod_xml_curl (FreeSWITCH → Backend):**

```http
GET /api/freeswitch/configuration?section=directory&tag_name=domain&key_name=name&key_value=default&user=101 HTTP/1.1
Host: localhost:5000
```

**Backend Response (XML):**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<document type="freeswitch/xml">
  <section name="directory">
    <domain name="default">
      <user id="101">
        <params>
          <param name="password" value="hashed_password"/>
          <param name="vm-password" value="101"/>
        </params>
        <variables>
          <variable name="tenant_id" value="123"/>
          <variable name="user_context" value="default"/>
        </variables>
      </user>
    </domain>
  </section>
</document>
```

---

**ESL Events (FreeSWITCH → Backend):**

```json
{
  "Event-Name": "CHANNEL_HANGUP_COMPLETE",
  "Caller-Caller-ID-Number": "101",
  "Caller-Destination-Number": "0901234567",
  "variable_billsec": "330",
  "variable_duration": "335",
  "variable_hangup_cause": "NORMAL_CLEARING",
  "variable_sip_hangup_disposition": "recv_bye"
}
```

Worker Service nhận event này và:
1. Parse event data
2. Calculate cost: `billsec * rate_per_second`
3. Save CDR to PostgreSQL
4. Deduct balance
5. Broadcast to SignalR

---

### 4.2. Frontend ↔ Backend API

**REST API Examples:**

```typescript
// 1. Login
POST /api/auth/login
{
  "email": "admin@company.com",
  "password": "password123"
}
Response: { "token": "jwt_token", "refreshToken": "..." }

// 2. Get Extensions
GET /api/extensions?tenantId=123
Response: [
  { "id": 1, "number": "101", "name": "John Doe", "status": "registered" },
  { "id": 2, "number": "102", "name": "Jane Smith", "status": "unregistered" }
]

// 3. Make Call (Click-to-Call)
POST /api/calls/make-call
{
  "fromExtension": "101",
  "toNumber": "0901234567"
}
Response: { "callId": "uuid-123", "status": "initiated" }

// 4. Get CDRs
GET /api/cdrs?startDate=2026-01-01&endDate=2026-01-31&direction=outbound
Response: [
  {
    "id": 1,
    "callerId": "101",
    "destination": "0901234567",
    "duration": 330,
    "cost": 2750,
    "recordingUrl": "https://minio.../recording.mp3"
  }
]
```

---

**SignalR (Real-time):**

```typescript
// Frontend connects
const connection = new HubConnectionBuilder()
  .withUrl("https://api.example.com/hubs/callmonitor")
  .build();

// Subscribe to events
connection.on("CallStarted", (data) => {
  console.log("New call:", data);
  // Update UI
});

connection.on("CallEnded", (data) => {
  console.log("Call ended:", data);
  // Update UI
});

connection.on("AgentStatusChanged", (data) => {
  console.log("Agent status:", data);
  // Update agent board
});
```

---

## 5. SO SÁNH CÁC PHƯƠNG THỨC GỌI

### 5.1. Bảng so sánh

| Tiêu chí | IP Phone → FS | Web/Mobile → API → FS | WebRTC (Browser) |
|----------|---------------|----------------------|------------------|
| **Client** | IP Phone (Zoiper, Yealink) | Web/Mobile App | Web Browser |
| **Protocol** | SIP (UDP/TCP) | HTTP API + SIP | WebSocket + WebRTC |
| **Kết nối** | Trực tiếp đến FS | Qua Backend API | WebSocket đến FS |
| **Use case** | Agent có IP Phone | Click-to-call, automation | Gọi trên browser |
| **Audio quality** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Setup** | Cần config IP Phone | Chỉ cần click button | Không cần cài đặt |
| **NAT issues** | Có thể có | Không | Có thể có (cần TURN) |
| **Latency** | Thấp | Thấp | Trung bình |

---

### 5.2. Khi nào dùng phương thức nào?

**IP Phone → FreeSWITCH:**
- ✅ Agent chuyên nghiệp (call center)
- ✅ Cần audio quality cao nhất
- ✅ Gọi nhiều (8 giờ/ngày)
- ✅ Có IP Phone sẵn

**Web/Mobile → API → FreeSWITCH:**
- ✅ Click-to-call từ CRM
- ✅ Automation (auto-dialer)
- ✅ Gọi theo lịch
- ✅ Integration với hệ thống khác

**WebRTC (Browser):**
- ✅ Agent làm remote
- ✅ Không muốn cài softphone
- ✅ Gọi tạm thời
- ✅ Demo, testing

---

## 6. TÓM TẮT KIẾN TRÚC

### 6.1. Các điểm chính

✅ **1 DATABASE duy nhất (PostgreSQL)**
- Lưu TẤT CẢ dữ liệu: tenant, user, extension, trunk, DID, CDR, billing, etc.
- FreeSWITCH KHÔNG có database riêng
- Backend API là single source of truth

✅ **FreeSWITCH là Telephony Engine**
- Chỉ xử lý cuộc gọi (SIP, RTP)
- Hỏi Backend để lấy config (mod_xml_curl)
- Gửi events về Backend (ESL)
- Stateless (không lưu trữ)

✅ **Backend API là trung tâm**
- Quản lý tất cả business logic
- Cung cấp REST API cho Frontend
- Cung cấp XML cho FreeSWITCH
- Lắng nghe events từ FreeSWITCH
- Lưu CDR, billing vào database

✅ **3 cách gọi điện:**
1. IP Phone → FreeSWITCH (traditional)
2. Web/Mobile → API → FreeSWITCH (click-to-call)
3. Browser → WebRTC → FreeSWITCH (softphone)

---

### 6.2. Sơ đồ tóm tắt luồng data

```mermaid
graph TB
    subgraph "Clients"
        C1[IP Phone]
        C2[Web/Mobile]
        C3[Browser WebRTC]
    end
    
    subgraph "Application"
        FS[FreeSWITCH<br/>Telephony Engine<br/>NO DATABASE]
        API[Backend API<br/>.NET 8]
    end
    
    subgraph "Data"
        DB[(PostgreSQL<br/>SINGLE DATABASE<br/>All Data Here)]
    end
    
    C1 -.SIP.-> FS
    C2 -.HTTP.-> API
    C3 -.WebSocket.-> FS
    
    FS -.mod_xml_curl<br/>Ask for config.-> API
    FS -.ESL Events<br/>Send call data.-> API
    
    API -.Read/Write<br/>All data.-> DB
    
    style DB fill:#ff6b6b,color:#fff
    style FS fill:#4ecdc4,color:#fff
    style API fill:#95e1d3,color:#000
```

---

## 7. FAQ

**Q1: Tại sao không dùng database cho FreeSWITCH?**
- A: FreeSWITCH hỗ trợ database (ODBC), nhưng chúng ta dùng mod_xml_curl để linh hoạt hơn
- Backend API có thể implement business logic phức tạp
- Dễ scale, dễ maintain

**Q2: FreeSWITCH có lưu CDR không?**
- A: FreeSWITCH có thể lưu CDR vào file CSV hoặc database
- Nhưng chúng ta dùng ESL events để Backend tự lưu
- Linh hoạt hơn, có thể tính billing real-time

**Q3: Nếu Backend API down thì sao?**
- A: FreeSWITCH sẽ không authenticate được user mới
- Nhưng user đã đăng ký vẫn gọi được (FreeSWITCH cache)
- Cần implement failover mechanism

**Q4: Có thể dùng nhiều FreeSWITCH servers không?**
- A: Có! Giai đoạn scale sẽ dùng nhiều FreeSWITCH
- Tất cả đều kết nối đến cùng 1 Backend API
- Cùng 1 PostgreSQL database

---

**Ngày cập nhật:** 05/01/2026  
**Phiên bản:** 1.0  

> [!NOTE]
> Tài liệu này giải thích chi tiết về kiến trúc. Nếu còn thắc mắc, vui lòng hỏi thêm!
