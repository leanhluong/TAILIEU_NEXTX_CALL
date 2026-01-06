# NGÀY 3-4: SIP & EXTENSIONS (16 giờ)

> [!IMPORTANT]
> Mục tiêu: Tạo extensions, đăng ký SIP, và thực hiện cuộc gọi nội bộ thành công

## Phần 1: Hiểu về User Directory (3 giờ)

### 1.1. User Directory là gì?

**User Directory** = Danh bạ người dùng trong FreeSWITCH
- Lưu thông tin extensions (101, 102, 103...)
- Chứa password để authentication
- Định nghĩa variables cho mỗi user

### 1.2. Cấu trúc Directory

```
/usr/local/freeswitch/conf/directory/
├── default.xml              # Domain default
└── default/                 # Thư mục chứa users
    ├── 1000.xml            # Extension 1000 (mặc định)
    ├── 1001.xml            # Extension 1001
    ├── 101.xml             # Extension 101 (tự tạo)
    └── 102.xml             # Extension 102 (tự tạo)
```

### 1.3. Anatomy của User XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<include>
  <user id="101">
    <!-- PARAMS: Tham số SIP -->
    <params>
      <param name="password" value="1234"/>
      <param name="vm-password" value="101"/>
    </params>
    
    <!-- VARIABLES: Biến custom -->
    <variables>
      <variable name="toll_allow" value="domestic,international"/>
      <variable name="accountcode" value="101"/>
      <variable name="user_context" value="default"/>
      <variable name="effective_caller_id_name" value="John Doe"/>
      <variable name="effective_caller_id_number" value="101"/>
    </variables>
  </user>
</include>
```

**Giải thích:**
- `id="101"` - Extension number
- `password` - Mật khẩu SIP authentication
- `vm-password` - Mật khẩu voicemail
- `user_context` - Dialplan context (default)
- `effective_caller_id_name` - Tên hiển thị khi gọi
- `accountcode` - Mã tài khoản (dùng cho billing)

---

## Phần 2: Tạo Extensions (4 giờ)

### 2.1. Tạo Extension 101

```bash
cd /usr/local/freeswitch/conf/directory/default
sudo vim 101.xml
```

**Nội dung:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<include>
  <user id="101">
    <params>
      <param name="password" value="password101"/>
      <param name="vm-password" value="101"/>
    </params>
    <variables>
      <variable name="toll_allow" value="domestic,international"/>
      <variable name="accountcode" value="101"/>
      <variable name="user_context" value="default"/>
      <variable name="effective_caller_id_name" value="Agent 101"/>
      <variable name="effective_caller_id_number" value="101"/>
      <variable name="outbound_caller_id_name" value="$${outbound_caller_name}"/>
      <variable name="outbound_caller_id_number" value="$${outbound_caller_id}"/>
      <variable name="callgroup" value="sales"/>
    </variables>
  </user>
</include>
```

### 2.2. Tạo nhiều Extensions nhanh

**Script tạo 10 extensions (101-110):**

```bash
#!/bin/bash
for i in {101..110}; do
cat > /usr/local/freeswitch/conf/directory/default/$i.xml << EOF
<?xml version="1.0" encoding="utf-8"?>
<include>
  <user id="$i">
    <params>
      <param name="password" value="password$i"/>
      <param name="vm-password" value="$i"/>
    </params>
    <variables>
      <variable name="toll_allow" value="domestic,international"/>
      <variable name="accountcode" value="$i"/>
      <variable name="user_context" value="default"/>
      <variable name="effective_caller_id_name" value="Agent $i"/>
      <variable name="effective_caller_id_number" value="$i"/>
      <variable name="callgroup" value="sales"/>
    </variables>
  </user>
</include>
EOF
done

# Reload XML
fs_cli -x "reloadxml"
```

### 2.3. Kiểm tra Extensions

```bash
# Trong fs_cli
fs_cli

# List tất cả users
list_users

# Xem chi tiết user 101
user_data 101@$${domain}
```

---

## Phần 3: Đăng ký SIP với Softphone (4 giờ)

### 3.1. Cài đặt Softphone

**Các lựa chọn:**
- **Zoiper** (Windows/Mac/Linux) - Recommended
- **Linphone** (Cross-platform)
- **MicroSIP** (Windows - lightweight)
- **Bria** (Professional)

**Download Zoiper:**
- https://www.zoiper.com/en/voip-softphone/download/current

### 3.2. Cấu hình Zoiper

**Bước 1: Tạo Account mới**
- Mở Zoiper → Settings → Accounts → Add Account
- Chọn "SIP"

**Bước 2: Điền thông tin:**
```
Account name: Extension 101
Domain: <IP_SERVER_FREESWITCH>
Username: 101
Password: password101
```

**Bước 3: Advanced Settings:**
```
Outbound Proxy: <IP_SERVER>:5060
Transport: UDP
STUN: Disable (nếu cùng mạng local)
```

**Bước 4: Save và đợi đăng ký**

### 3.3. Kiểm tra Registration

**Trong fs_cli:**

```bash
# Xem tất cả registrations
sofia status profile internal reg

# Output mẫu:
# Registrations:
# =================================================================================================
# Call-ID:        abc123@192.168.1.100
# User:           101@192.168.1.50
# Contact:        "Agent 101" <sip:101@192.168.1.100:5060>
# Agent:          Zoiper
# Status:         Registered(UDP)(unknown) EXP(2024-01-06 12:00:00)
```

**Xem chi tiết:**

```bash
sofia status profile internal user 101@$${domain}
```

### 3.4. Troubleshooting Registration

**Lỗi: "Registration Failed"**

```bash
# 1. Check SIP profile có chạy không
sofia status

# 2. Check firewall
sudo ufw status

# 3. Xem SIP trace
sofia global siptrace on

# 4. Xem logs
tail -f /usr/local/freeswitch/log/freeswitch.log | grep 101
```

**Lỗi: "Authentication Failed"**

```bash
# Check password trong user XML
cat /usr/local/freeswitch/conf/directory/default/101.xml

# Reload XML
reloadxml

# Thử đăng ký lại
```

---

## Phần 4: Thực hiện cuộc gọi nội bộ (5 giờ)

### 4.1. Hiểu Dialplan cơ bản

**Dialplan** = Kế hoạch quay số = "Khi gọi số X, làm gì?"

```xml
<extension name="tên_extension">
  <condition field="trường_kiểm_tra" expression="regex">
    <action application="ứng_dụng" data="tham_số"/>
  </condition>
</extension>
```

### 4.2. Dialplan cho Local Extension

```bash
sudo vim /usr/local/freeswitch/conf/dialplan/default.xml
```

**Tìm extension "Local_Extension":**

```xml
<extension name="Local_Extension">
  <condition field="destination_number" expression="^(10[0-9]{2})$">
    <action application="export" data="dialed_extension=$1"/>
    <action application="bind_meta_app" data="1 b s execute_extension::dx XML features"/>
    <action application="bind_meta_app" data="2 b s record_session::$${recordings_dir}/${caller_id_number}.${strftime(%Y-%m-%d-%H-%M-%S)}.wav"/>
    <action application="bind_meta_app" data="3 b s execute_extension::cf XML features"/>
    <action application="set" data="ringback=${us-ring}"/>
    <action application="set" data="transfer_ringback=$${hold_music}"/>
    <action application="set" data="call_timeout=30"/>
    <action application="set" data="hangup_after_bridge=true"/>
    <action application="set" data="continue_on_fail=true"/>
    <action application="hash" data="insert/${domain_name}-call_return/${dialed_extension}/${caller_id_number}"/>
    <action application="hash" data="insert/${domain_name}-last_dial_ext/${dialed_extension}/${uuid}"/>
    <action application="set" data="called_party_callgroup=${user_data(${dialed_extension}@${domain_name} var callgroup)}"/>
    <action application="hash" data="insert/${domain_name}-last_dial/${called_party_callgroup}/${uuid}"/>
    <action application="bridge" data="user/${dialed_extension}@${domain_name}"/>
    <action application="answer"/>
    <action application="sleep" data="1000"/>
    <action application="bridge" data="loopback/app=voicemail:default ${domain_name} ${dialed_extension}"/>
  </condition>
</extension>
```

**Giải thích đơn giản:**

```xml
<extension name="call_local_extension">
  <!-- Nếu gọi số 101-110 -->
  <condition field="destination_number" expression="^(10[0-9]|110)$">
    <!-- Đổ chuông 30 giây -->
    <action application="set" data="call_timeout=30"/>
    
    <!-- Gọi đến user đó -->
    <action application="bridge" data="user/$1@${domain_name}"/>
    
    <!-- Nếu không nhấc máy, chuyển voicemail -->
    <action application="answer"/>
    <action application="voicemail" data="default ${domain_name} $1"/>
  </condition>
</extension>
```

### 4.3. Test cuộc gọi

**Bước 1: Đăng ký 2 extensions**
- Zoiper 1: Extension 101
- Zoiper 2: Extension 102 (hoặc dùng softphone khác)

**Bước 2: Từ 101, gọi 102**
- Nhấn dial pad: `102`
- Nhấn Call

**Bước 3: Kiểm tra trong fs_cli**

```bash
# Xem cuộc gọi đang hoạt động
show channels

# Output:
# uuid,direction,created,name,state,cid_name,cid_num,dest,application,application_data
# abc-123,inbound,2024-01-06 12:00:00,sofia/internal/101,CS_EXECUTE,Agent 101,101,102,bridge,user/102@domain.com
```

**Bước 4: Xem CDR**

```bash
# Sau khi kết thúc cuộc gọi
cat /usr/local/freeswitch/log/cdr-csv/Master.csv | tail -1
```

### 4.4. Dialplan Applications quan trọng

| Application | Mô tả | Ví dụ |
|-------------|-------|-------|
| **answer** | Trả lời cuộc gọi | `<action application="answer"/>` |
| **bridge** | Kết nối 2 channels | `<action application="bridge" data="user/102"/>` |
| **hangup** | Ngắt máy | `<action application="hangup"/>` |
| **playback** | Phát file audio | `<action application="playback" data="/tmp/welcome.wav"/>` |
| **sleep** | Đợi (ms) | `<action application="sleep" data="1000"/>` |
| **set** | Set variable | `<action application="set" data="var_name=value"/>` |
| **transfer** | Chuyển cuộc gọi | `<action application="transfer" data="102 XML default"/>` |
| **voicemail** | Voicemail | `<action application="voicemail" data="default ${domain} 101"/>` |

---

## Phần 5: SIP Debugging (2 giờ)

### 5.1. Enable SIP Trace

```bash
# Trong fs_cli
sofia global siptrace on

# Hoặc chỉ trace 1 profile
sofia profile internal siptrace on
```

### 5.2. Xem SIP Messages

**Khi gọi từ 101 → 102, bạn sẽ thấy:**

```
INVITE sip:102@192.168.1.50 SIP/2.0
Via: SIP/2.0/UDP 192.168.1.100:5060
From: "Agent 101" <sip:101@192.168.1.50>;tag=abc123
To: <sip:102@192.168.1.50>
Call-ID: xyz789@192.168.1.100
CSeq: 1 INVITE
Contact: <sip:101@192.168.1.100:5060>
Content-Type: application/sdp

v=0
o=FreeSWITCH 123456 123457 IN IP4 192.168.1.50
s=FreeSWITCH
c=IN IP4 192.168.1.50
t=0 0
m=audio 16384 RTP/AVP 0 8 101
a=rtpmap:0 PCMU/8000
a=rtpmap:8 PCMA/8000
a=rtpmap:101 telephone-event/8000
```

### 5.3. Analyze Call Flow

```bash
# Xem tất cả events của 1 cuộc gọi
uuid_dump <UUID>

# Xem variables
uuid_getvar <UUID> variable_name

# Kill cuộc gọi
uuid_kill <UUID>
```

---

## Bài tập thực hành

### ✅ Checklist

- [ ] Tạo được 10 extensions (101-110)
- [ ] Đăng ký Zoiper thành công
- [ ] Gọi nội bộ 101 → 102 thành công
- [ ] Xem được SIP trace
- [ ] Hiểu dialplan cơ bản

### 🎯 Bài tập

**Bài 1:** Tạo extension 201-210 với password khác

**Bài 2:** Viết dialplan để:
- Gọi 101-110 → ring 20 giây
- Gọi 201-210 → ring 40 giây

**Bài 3:** Tạo extension đặc biệt:
- Gọi 999 → Phát file audio "welcome.wav"

**Bài 4:** Debug:
- Enable SIP trace
- Gọi 101 → 102
- Capture SIP messages
- Phân tích INVITE, 200 OK, ACK, BYE

---

## Troubleshooting

### Lỗi: "User not found"

```bash
# Check user có tồn tại
list_users | grep 101

# Reload XML
reloadxml
```

### Lỗi: "No route to destination"

```bash
# Check dialplan
xml_locate dialplan

# Test dialplan
fsctl send_display :102@${domain}
```

### Lỗi: "One way audio"

```bash
# Check RTP ports
netstat -an | grep 16384

# Check NAT settings trong vars.xml
vim /usr/local/freeswitch/conf/vars.xml
```

---

## Bước tiếp theo

📄 [Ngày 5: Dialplan Nâng cao](./NGAY_05_DIALPLAN_NANG_CAO.md)
