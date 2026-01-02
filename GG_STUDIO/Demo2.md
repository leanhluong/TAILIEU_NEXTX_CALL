Chào bạn, quyết định chuyển sang dùng **.NET (phiên bản mới nhất năm 2026 là .NET 10 hoặc .NET 9 LTS)** thay vì PHP là một bước đi cực kỳ táo bạo nhưng **rất mạnh mẽ** về mặt hiệu năng và khả năng bảo trì (Maintainability) cho hệ thống Enterprise sau này.

Tuy nhiên, bạn cần xác định rõ: **Bạn không thể dùng FusionPBX được nữa**. FusionPBX được viết cứng bằng PHP.
=> **Bạn sẽ phải tự viết một phần mềm quản lý (Softswitch Controller) bằng .NET Core để điều khiển FreeSWITCH.** Đây là việc xây dựng một "FusionPBX phiên bản .NET" của riêng bạn.

Dưới đây là bản kế hoạch chi tiết "tận răng" để team .NET của bạn bắt tay vào xây dựng hệ thống **SaaS VoIP Custom** này.

---

### PHẦN 1: TEAM BUILD (ĐỘI NGŨ TRIỂN KHAI)

Vì bạn tự build core logic, yêu cầu kỹ thuật sẽ cao hơn việc chỉ cài đặt tool có sẵn.

| Vị trí | Level | Hard Skills (Kỹ năng cứng) | Nhiệm vụ chi tiết |
| :--- | :---: | :--- | :--- |
| **1. Solution Architect / Tech Lead** (Có thể là bạn) | Senior | - **VoIP:** Deep understanding SIP, RTP, SDP, NAT. <br> - **Arch:** Microservices, Event-driven Architecture. <br> - **FreeSWITCH:** Cấu hình `mod_xml_curl`, `mod_event_socket`. | - Thiết kế luồng XML động trả về cho FreeSWITCH. <br> - Quyết định kiến trúc Database Sharding (nếu cần). <br> - Review code bảo mật. |
| **2. Senior Backend (.NET)** | Senior | - **Stack:** .NET 9/10, Entity Framework Core, LINQ. <br> - **Performance:** Redis Caching, Async/Await tối ưu. <br> - **Libs:** NFreeSwitch (hoặc ESL libs), gRPC. | - Viết API xử lý `mod_xml_curl` (FreeSWITCH gọi vào lấy config). <br> - Viết Worker Service lắng nghe sự kiện từ FreeSWITCH (ESL). <br> - Xử lý logic Billing thời gian thực. |
| **3. Frontend Developer** | Mid/Snr | - **Stack:** ReactJS / VueJS hoặc **Blazor WebAssembly** (nếu muốn full stack .NET). <br> - **Realtime:** SignalR (để hiển thị cuộc gọi đang diễn ra trên Web). | - Dựng Dashboard quản lý Tenant. <br> - Dựng Webphone (WebRTC) tích hợp trên trình duyệt. |
| **4. DevOps Engineer** | Mid | - **OS:** Debian 12/13. <br> - **Container:** Docker, Kubernetes (K8s) - cực hợp với .NET. <br> - **CI/CD:** Github Actions / Azure DevOps. | - Build Docker Image cho .NET API và FreeSWITCH. <br> - Cấu hình Nginx Reverse Proxy. <br> - Setup Monitoring (Prometheus + Grafana). |

---

### PHẦN 2: HẠ TẦNG KỸ THUẬT & TECH STACK

#### 1. Software Stack (Công nghệ sử dụng)
*   **Core Telephony:** **FreeSWITCH** (Biên dịch từ source trên Debian).
*   **Backend Logic:** **ASP.NET Core 9/10 Web API**.
*   **Realtime Communication:** **SignalR** (Websocket cho Frontend) + **Event Socket Library (ESL)** (Giao tiếp với FreeSWITCH).
*   **Database:**
    *   **PostgreSQL:** Lưu trữ dữ liệu tĩnh (User, Tenant, Domain, CDR).
    *   **Redis:** Cache cấu hình XML (Cực quan trọng để giảm tải DB khi FreeSWITCH query liên tục).
*   **Storage:** **MinIO** (S3 Compatible) - Tự host object storage để lưu file ghi âm.
*   **Proxy:** **Kamailio** (Có thể thêm vào sau, giai đoạn 1 dùng Nginx làm Gateway cho API).

#### 2. Hardware Sizing (Cho MVP - Chịu tải ~100-200 CCU)
*   **Server Logic (.NET API + DB):** 4 vCPU, 8GB RAM (NET Core rất nhẹ và nhanh).
*   **Server Media (FreeSWITCH):** 8 vCPU, 16GB RAM (Tập trung CPU để xử lý RTP).
*   **Network:** 1Gbps Port.

---

### PHẦN 3: TÀI LIỆU YÊU CẦU PHẦN MỀM (SRS) - CHI TIẾT

Đây là đề bài cho đội Dev .NET thực hiện.

#### 1. Module: Dynamic Configuration (Quan trọng nhất)
*   **Mô tả:** FreeSWITCH không lưu file XML tĩnh. Khi có request, nó gọi API .NET, API .NET query DB và trả về XML string.
*   **Functional Req:**
    *   **REQ-SYS-01:** API phải hứng request POST từ `mod_xml_curl` của FreeSWITCH.
    *   **REQ-SYS-02:** Xử lý `section=directory`: Trả về thông tin User (Password, Domain) để xác thực đăng ký (Register).
    *   **REQ-SYS-03:** Xử lý `section=dialplan`: Trả về logic định tuyến (gọi số A thì đi đâu, gặp IVR nào) dưới dạng XML.
    *   **Performance:** Thời gian phản hồi API phải < 200ms. Bắt buộc dùng Redis Cache layer.

#### 2. Module: Multi-tenancy (SaaS)
*   **REQ-SAAS-01:** Phân quyền dữ liệu theo `TenantID`. Mọi query Database đều phải có `Where(x => x.TenantId == ...`.
*   **REQ-SAAS-02:** Quản lý hạn mức (Quota). Mỗi Tenant có số lượng Agent tối đa, số kênh (Concurrent Calls) tối đa.

#### 3. Module: Billing & CDR (Tính tiền)
*   **REQ-BIL-01:** **ESL Listener Worker** phải bắt được sự kiện `CHANNEL_HANGUP_COMPLETE`.
*   **REQ-BIL-02:** Lấy thông tin: `variable_billsec` (số giây thực gọi), `variable_sip_to_user`, `variable_sip_from_user`.
*   **REQ-BIL-03:** Tính toán giá cước dựa trên bảng giá (Rate Table) của Tenant đó -> Trừ tiền vào ví (Wallet).
*   **REQ-BIL-04:** Nếu tài khoản < 0, API Dialplan phải trả về XML từ chối cuộc gọi (hoặc play file "Tài khoản quý khách đã hết").

#### 4. Module: Recording Management
*   **REQ-REC-01:** FreeSWITCH ghi âm ra file `.wav` tại local.
*   **REQ-REC-02:** .NET Background Service (Hosted Service) quét thư mục, convert sang `.mp3`, upload lên MinIO/S3.
*   **REQ-REC-03:** Xóa file gốc, lưu URL S3 vào Database.

---

### PHẦN 4: TÀI LIỆU THIẾT KẾ HỆ THỐNG (SDS)

#### 1. Architecture Diagram (Luồng dữ liệu)
```mermaid
[SIP Phone] <--> (UDP 5060/RTP) <--> [FreeSWITCH Server]
                                          |    ^
                               (mod_xml_curl)  | (ESL - TCP 8021)
                               HTTP POST       | TCP Socket
                                     |         v
[Redis] <--> [.NET Core Web API] <--> [.NET Worker Service]
                    |
              [PostgreSQL]
```

#### 2. Database Schema Design (Entity Framework Core)

*   **Table: Tenants**
    *   `Id` (Guid), `Name`, `Domain` (Unique), `Balance` (Decimal), `IsActive`.
*   **Table: SipProfiles** (Extension)
    *   `Id`, `TenantId` (FK), `Username` (101, 102...), `Password`, `AuthDomain`.
*   **Table: Dialplans**
    *   `Id`, `TenantId`, `MatchRule` (Regex), `Actions` (JSON - Lưu chuỗi hành động như Bridge, Playback).
*   **Table: CallDetailRecords** (CDRs)
    *   `Id`, `TenantId`, `CallUuid`, `Caller`, `Callee`, `StartTime`, `AnswerTime`, `EndTime`, `BillSec`, `RecordingUrl`, `Cost`.

#### 3. API Contract (Ví dụ endpoint phục vụ FreeSWITCH)
*   `POST /api/freeswitch/configuration`
    *   Input: Form-data (section, tag_name, key_name, key_value...)
    *   Output: `text/xml`
    *   Logic:
        ```csharp
        // Pseudo Code .NET
        if (request.Section == "directory") {
            var user = _userService.GetByExt(request.KeyValue);
            return _xmlBuilder.BuildUserXml(user);
        }
        ```

---

### PHẦN 5: TIMELINE CHI TIẾT & CHECKLIST (12 TUẦN)

Xây dựng hệ thống custom tốn nhiều thời gian hơn cài sẵn.

#### **Giai đoạn 1: Core Foundation (Tuần 1 - 4)**
*   **Tuần 1: Hạ tầng & Hello World**
    *   [ ] Setup Server Debian, cài FreeSWITCH.
    *   [ ] Tạo Project .NET Solution (Clean Architecture).
    *   [ ] Cấu hình `mod_xml_curl` trong FreeSWITCH trỏ về `http://localhost:5000/api/fs`.
*   **Tuần 2: Directory Handler (Đăng ký User)**
    *   [ ] Thiết kế DB Schema User/Extension.
    *   [ ] Viết API trả về XML Directory.
    *   [ ] Test: Dùng Softphone đăng ký thành công vào hệ thống.
*   **Tuần 3: Dialplan Handler (Định tuyến)**
    *   [ ] Viết logic định tuyến nội bộ (Extension gọi Extension).
    *   [ ] Trả về XML Dialplan động.
    *   [ ] Test: Gọi nội bộ 2 máy thông nhau, có tiếng.
*   **Tuần 4: SIP Trunking (Gọi ra ngoài)**
    *   [ ] Cấu hình Gateway kết nối nhà mạng.
    *   [ ] Viết logic Dialplan gọi ra (Outbound).

#### **Giai đoạn 2: SaaS Features (Tuần 5 - 8)**
*   **Tuần 5: ESL & Event Processing**
    *   [ ] Viết .NET Worker Service kết nối tới FreeSWITCH port 8021.
    *   [ ] Bắt sự kiện `CHANNEL_CREATE`, `CHANNEL_ANSWER`, `CHANNEL_HANGUP`.
*   **Tuần 6: Billing System**
    *   [ ] Xử lý logic trừ tiền khi có sự kiện Hangup.
    *   [ ] Chặn cuộc gọi nếu Balance <= 0.
*   **Tuần 7: Recording & Storage**
    *   [ ] Cài MinIO.
    *   [ ] Viết Service upload file ghi âm background.
*   **Tuần 8: Frontend Dashboard (Admin CP)**
    *   [ ] Trang quản lý Tenant.
    *   [ ] Trang tạo Extension.
    *   [ ] Trang xem lịch sử cuộc gọi (CDR).

#### **Giai đoạn 3: Advanced & Optimization (Tuần 9 - 12)**
*   **Tuần 9: Realtime Dashboard (SignalR)**
    *   [ ] Hiển thị trạng thái Agent (Online/Offline/Busy) trên Web.
*   **Tuần 10: IVR Builder (Kịch bản)**
    *   [ ] Xây dựng giao diện kéo thả hoặc menu để tạo kịch bản IVR (Bấm phím).
*   **Tuần 11: Security & Stress Test**
    *   [ ] Cấu hình Firewall.
    *   [ ] Load Test với 200 cuộc gọi đồng thời.
*   **Tuần 12: Deployment & Pilot**
    *   [ ] Deploy lên môi trường Production.
    *   [ ] Cho khách hàng đầu tiên dùng thử.

---

### LỜI KHUYÊN DÀNH RIÊNG CHO ĐỘI DEV .NET CỦA BẠN

1.  **Thư viện nên dùng:** Hãy tìm hiểu thư viện **`NServiceBus`** hoặc các thư viện **ESL Client for .NET** trên GitHub. Đừng tự viết raw socket client trừ khi bạn quá giỏi, vì xử lý TCP stream rất dễ lỗi.
2.  **Debug:** Kỹ năng quan trọng nhất là đọc log FreeSWITCH (`fs_cli`) song song với Log của .NET (`Serilog`).
3.  **Caching:** Vì mỗi cuộc gọi FreeSWITCH đều gọi API để hỏi "Tao phải làm gì?", nếu API chậm -> Cuộc gọi bị delay. **Redis là bắt buộc**. Cache lại XML Dialplan của user trong 5-10 phút.

Đây là lộ trình chuyên nghiệp để build một Product SaaS "Make in Vietnam" xịn sò bằng công nghệ .NET mới nhất. Chúc team bạn code "bay" không bug!