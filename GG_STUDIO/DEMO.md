Bạn hoàn toàn đúng. Với tư duy **Lean Startup** và **MVP (Minimum Viable Product)**, việc bắt đầu với **1 Server All-in-One** là bước đi thông minh nhất để tiết kiệm chi phí và kiểm chứng thị trường (Proof of Concept).

Tôi đã thay đổi chiến lược để phù hợp với yêu cầu của bạn: **"Bắt đầu nhỏ - Nghĩ lớn"**. Chúng ta sẽ chạy All-in-one nhưng thiết kế kiến trúc để *sẵn sàng tách ra* khi cần mà không phải đập đi xây lại.

Dưới đây là bộ tài liệu **Roadmap, Timeline, SRS và SDS** rút gọn cho dự án SaaS VoIP Multi-tenant của bạn.

---

### PHẦN 1: GIẢI QUYẾT CÂU HỎI "TẠI SAO KHÔNG CHẠY ALL-IN-ONE?"

**Trả lời:** **Có thể chạy All-in-one**.
Mặc định bộ cài FusionPBX chính là All-in-one (FreeSWITCH + DB + Web + Fail2Ban nằm chung).

**Tuy nhiên, rủi ro bạn cần chấp nhận khi làm SaaS Multi-tenant:**
1.  **Single Point of Failure:** Server chết = Tất cả Tenant chết. (Chấp nhận được ở giai đoạn Beta/Test).
2.  **Hiệu năng Web vs Thoại:** Khi truy xuất báo cáo (Query DB nặng), tiếng có thể bị méo hoặc ngắt quãng do CPU bị chiếm dụng.
3.  **Khó mở rộng nóng:** Khi full tải, muốn thêm server sẽ phải cấu hình lại IP, DNS, ảnh hưởng đến khách hàng đang dùng.

**=> Giải pháp cho giai đoạn MVP:**
Dùng 1 VPS cấu hình mạnh (Ví dụ: 8 CPU, 16GB RAM). Cài đặt FusionPBX chuẩn. Sau này khi có khách hàng trả tiền, ta sẽ tách Database và Web ra trước, giữ lại Server đó làm Media Server.

---

### PHẦN 2: ROADMAP TỔNG THỂ VÀ TIMELINE (DỰ KIẾN 2 THÁNG CHO MVP)

Giả định team có: 1 Tech Lead (bạn), 1 Dev Backend, 1 Dev Frontend/Mobile (nếu cần Softphone riêng), 1 Tester/QA.

#### **Giai đoạn 1: Dựng Core & Lab (Tuần 1-2)**
*   **Tuần 1:**
    *   Thuê VPS/Server. Setup môi trường Linux (Debian 12 - FusionPBX chạy mượt nhất trên Debian).
    *   Cài đặt FusionPBX Script mặc định.
    *   Cấu hình cơ bản: Extension, SIP Trunk (kết nối thử 1 nhà mạng VN như FPT/CMC/Viettel).
*   **Tuần 2:**
    *   Test cuộc gọi nội bộ (LAN) và qua Internet (NAT).
    *   Tích hợp thử Kamailio làm Proxy phía trước (Cài trên cùng server nhưng listen port khác, ví dụ Kamailio 5060 -> FreeSWITCH 5080). *Nếu thấy khó quá có thể bỏ qua Kamailio ở giai đoạn này, dùng FreeSWITCH trực tiếp*.

#### **Giai đoạn 2: Phát triển tính năng SaaS (Tuần 3-5)**
*   **Tuần 3:** Custom giao diện FusionPBX (Rebranding) -> Đổi logo, màu sắc, ẩn các menu không cần thiết để khách hàng đỡ rối.
*   **Tuần 4:** Xây dựng module **Billing (Tính cước)**. Đây là SaaS nên phải tính tiền được.
    *   Logic: Tenant A nạp tiền -> Gọi trừ tiền theo block 6s/1s.
*   **Tuần 5:** API Wrapper. Viết API để frontend (hoặc trang Landing page đăng ký) có thể tự tạo Tenant, tự tạo Extension mà không cần chọc vào giao diện Admin của FusionPBX.

#### **Giai đoạn 3: Testing & Pilot (Tuần 6-8)**
*   **Tuần 6:** Stress Test. Dùng công cụ **SIPP** để giả lập 50-100 cuộc gọi đồng thời xem server chịu nổi không.
*   **Tuần 7:** Dogfooding. Cho chính công ty bạn (10-20 người) dùng hệ thống này thay cho hệ thống cũ.
*   **Tuần 8:** Fix bug & Go Live bản Beta.

---

### PHẦN 3: TÀI LIỆU YÊU CẦU PHẦN MỀM (SRS - RÚT GỌN)

**Tên dự án:** Cloud Call Center SaaS (MVP)

#### 1. Actor (Các đối tượng sử dụng)
*   **Super Admin (Bạn):** Quyền tối cao, tạo Tenant, định tuyến SIP Trunk nhà mạng, xem doanh thu tổng.
*   **Tenant Admin (Khách hàng):** Quản lý nhân viên của họ, mua đầu số, xem báo cáo cuộc gọi của công ty họ, nghe file ghi âm.
*   **Agent (Nhân viên của khách):** Đăng nhập để nghe gọi, xem lịch sử cuộc gọi cá nhân.

#### 2. Functional Requirements (Yêu cầu chức năng)
*   **REQ-01: Multi-tenancy Management**
    *   Hệ thống phải phân tách dữ liệu tuyệt đối giữa các Tenant (Domain based). Tenant A không thấy số của Tenant B.
*   **REQ-02: Call Handling (Cốt lõi)**
    *   Hỗ trợ Inbound (gọi vào), Outbound (gọi ra), Internal (gọi nội bộ).
    *   Tính năng: Transfer (chuyển máy), Hold, Mute.
*   **REQ-03: IVR & Routing**
    *   Tenant Admin có thể tự upload file lời chào.
    *   Cấu hình phím bấm (Bấm 1 gặp Sale, 2 gặp Kỹ thuật).
    *   Ring Group: Đổ chuông đồng loạt hoặc lần lượt.
*   **REQ-04: Recording & CDR**
    *   Ghi âm 100% cuộc gọi (hoặc tùy chọn).
    *   Lưu trữ file dạng `.wav` hoặc `.mp3`.
    *   Xem báo cáo CDR (Call Detail Record): Thời lượng, nguồn, đích, trạng thái (Answer/Cancel/Busy).

#### 3. Non-Functional Requirements (Phi chức năng)
*   **Codecs:** Hỗ trợ G.711 (PCMA/PCMU) và G.729 (tiết kiệm băng thông).
*   **Protocol:** SIP (UDP/TCP), WebRTC (để gọi trên trình duyệt).
*   **Latency:** Độ trễ cuộc gọi < 150ms.
*   **Security:** Chặn IP quốc tế, Fail2Ban (chặn đăng nhập sai quá 3 lần).

---

### PHẦN 4: TÀI LIỆU THIẾT KẾ HỆ THỐNG (SDS - MÔ HÌNH ALL-IN-ONE)

Dù là 1 Server vật lý, chúng ta vẫn thiết kế tách lớp Logic (Logical Separation).

#### 1. Kiến trúc tổng quan (Architecture Diagram)
*   **Server:** VPS Linux (Debian 12).
*   **Public IP:** 1 Static IP.

**Các tầng trong 1 Server:**
1.  **Ingress Layer (Cổng vào):**
    *   **Nginx (Port 80/443):** Phục vụ Web Interface, API.
    *   **Firewall (IPTables/UFW):** Chỉ mở port 80, 443, 5060 (SIP), 16384-32768 (RTP).
2.  **App Layer (Xử lý):**
    *   **FreeSWITCH:** Core xử lý thoại. Chạy port 5060 (Internal Profile) và 5080 (External Profile).
    *   **PHP-FPM:** Xử lý logic Web của FusionPBX.
3.  **Data Layer (Lưu trữ):**
    *   **PostgreSQL:** Database chính. Chứa bảng `v_domains`, `v_extensions`, `v_xml_cdr`.
    *   **File System:** Lưu file ghi âm tại `/var/lib/freeswitch/recordings/`.

#### 2. Database Design (Điểm nhấn Multi-tenant)
Trong FusionPBX, mọi bảng đều có cột `domain_uuid`. Đây là khóa để phân biệt Tenant.
*   Bảng `v_domains`: Danh sách khách hàng.
*   Bảng `v_extensions`:
    *   `extension_uuid` (PK)
    *   `domain_uuid` (FK - Thuộc về công ty nào)
    *   `extension` (Số máy nhánh, VD: 101)
    *   `password`
*   Bảng `v_xml_cdr`:
    *   `uuid` (ID cuộc gọi)
    *   `domain_uuid`
    *   `caller_id_number`
    *   `destination_number`
    *   `billsec` (Số giây đàm thoại thực tế - dùng để tính tiền).

#### 3. Luồng đi của một cuộc gọi (Sequence)
1.  Khách hàng (IP Phone) gửi `INVITE` đến IP Server.
2.  FreeSWITCH nhận bản tin, truy vấn PostgreSQL: "Số máy này thuộc Domain nào? Password đúng không?".
3.  Nếu đúng -> Chấp nhận đăng ký.
4.  Khi gọi ra: FreeSWITCH kiểm tra "Domain này còn tiền không?" (Thông qua script Lua hoặc check DB).
5.  Nếu còn tiền -> Đẩy cuộc gọi qua SIP Trunk nhà mạng -> Kết nối.

---

### PHẦN 5: CHECKLIST HÀNH ĐỘNG NGAY (ACTIONABLE ITEMS)

Đây là những việc bạn cần làm trong 3 ngày tới để khởi động dự án:

1.  **Mua Domain & VPS:**
    *   Domain: `crm-voice.com` (ví dụ).
    *   VPS: Tối thiểu 4 Core, 8GB RAM, SSD 100GB.
2.  **Cài đặt FusionPBX:**
    *   Tìm script cài đặt chính chủ (Official Install Script) trên Github của FusionPBX. Đừng cài tay từng món, rất dễ lỗi.
3.  **Đăng ký SIP Trunk test:**
    *   Liên hệ FPT/Viettel/CMC xin 1 đầu số SIP Trunk chạy thử (hoặc dùng các dịch vụ VoIP quốc tế như Twilio/SignalWire để test cho nhanh, đỡ thủ tục hợp đồng).
4.  **Mua 2 cái IP Phone hoặc tai nghe:**
    *   Để test chất lượng thoại thực tế. Đừng chỉ test bằng phần mềm trên máy tính.

**Lời khuyên cuối cùng:**
Đừng quá sa đà vào việc tùy biến Kamailio ngay lúc đầu nếu team bạn chưa mạnh về Telecom. FusionPBX (FreeSWITCH thuần) đã đủ mạnh để chạy ổn định cho < 500 cuộc gọi đồng thời. Hãy tập trung làm phần **Quản lý cước (Billing)** và **Báo cáo (Report)** thật tốt, vì đó là cái khách hàng SaaS quan tâm nhất để trả tiền cho bạn.