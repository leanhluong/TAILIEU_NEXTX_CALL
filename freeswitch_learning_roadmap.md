# LỘ TRÌNH HỌC FREESWITCH CHO DỰ ÁN CALL CENTER

> [!IMPORTANT]
> Lộ trình 4 tuần để thành thạo FreeSWITCH và phát triển đầy đủ các chức năng dự án Call Center SaaS

**Thời gian:** 4 tuần (160 giờ)  
**Mục tiêu:** Thành thạo FreeSWITCH để phát triển toàn bộ tính năng dự án

---

## TUẦN 1: CƠ BẢN FREESWITCH (40h)

### Ngày 1-2: Cài đặt & Cấu hình cơ bản (16h)

**Lý thuyết (4h):**
- VoIP & SIP protocol cơ bản
- FreeSWITCH architecture overview
- Modules system

**Thực hành (12h):**
```bash
# Cài đặt từ source
cd /usr/src
git clone https://github.com/signalwire/freeswitch.git
cd freeswitch
./bootstrap.sh
./configure
make && make install

# Test cơ bản
fs_cli
sofia status
```

**Deliverables:**
- [ ] FreeSWITCH chạy được
- [ ] Kết nối fs_cli thành công
- [ ] Hiểu cấu trúc thư mục /etc/freeswitch

### Ngày 3-4: SIP & Extensions (16h)

**Lý thuyết (4h):**
- SIP REGISTER, INVITE, BYE
- User directory structure
- Authentication flow

**Thực hành (12h):**
```xml
<!-- conf/directory/default/101.xml -->
<user id="101">
  <params>
    <param name="password" value="1234"/>
  </params>
  <variables>
    <param name="user_context" value="default"/>
  </variables>
</user>
```

**Test:**
- Đăng ký Zoiper/Linphone với extension 101
- Gọi nội bộ 101 → 102

**Deliverables:**
- [ ] Tạo được 10 extensions
- [ ] Gọi nội bộ thành công
- [ ] Hiểu SIP registration flow

### Ngày 5: Dialplan Basics (8h)

**Lý thuyết (2h):**
- Dialplan XML structure
- Conditions & Actions
- Variables

**Thực hành (6h):**
```xml
<!-- conf/dialplan/default.xml -->
<extension name="local_extension">
  <condition field="destination_number" expression="^(10[0-9])$">
    <action application="bridge" data="user/$1@$${domain}"/>
  </condition>
</extension>
```

**Deliverables:**
- [ ] Viết dialplan cho internal calls
- [ ] Hiểu regex trong dialplan
- [ ] Test với 5 scenarios khác nhau

---

## TUẦN 2: TÍCH HỢP VỚI BACKEND (40h)

### Ngày 1-2: mod_xml_curl (16h)

**Lý thuyết (4h):**
- mod_xml_curl workflow
- HTTP request/response
- XML generation

**Thực hành (12h):**
```xml
<!-- conf/autoload_configs/xml_curl.conf.xml -->
<configuration name="xml_curl.conf">
  <bindings>
    <binding name="directory">
      <param name="gateway-url" value="http://localhost:5000/api/freeswitch/configuration" bindings="directory"/>
    </binding>
    <binding name="dialplan">
      <param name="gateway-url" value="http://localhost:5000/api/freeswitch/configuration" bindings="dialplan"/>
    </binding>
  </bindings>
</configuration>
```

**Backend API (.NET):**
```csharp
[HttpGet("api/freeswitch/configuration")]
public IActionResult GetConfiguration([FromQuery] string section, [FromQuery] string user)
{
    if (section == "directory")
    {
        var xml = GenerateDirectoryXml(user);
        return Content(xml, "text/xml");
    }
    return NotFound();
}
```

**Deliverables:**
- [ ] mod_xml_curl hoạt động
- [ ] Backend trả XML cho directory
- [ ] Backend trả XML cho dialplan
- [ ] Extensions đăng ký qua API

### Ngày 3-4: Event Socket Layer (ESL) (16h)

**Lý thuyết (4h):**
- ESL inbound vs outbound
- Event types
- ESL commands

**Thực hành (12h):**
```csharp
// Worker Service
public class FreeSwitchEslWorker : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        var connection = new InboundSocket("localhost", 8021, "ClueCon");
        await connection.ConnectAsync();
        
        connection.SubscribeEvents(new[] { "CHANNEL_HANGUP_COMPLETE" });
        
        connection.OnEventReceived += async (sender, e) =>
        {
            var billsec = e.GetVariable("billsec");
            var caller = e.GetVariable("caller_id_number");
            // Save CDR to database
        };
    }
}
```

**Deliverables:**
- [ ] ESL connection thành công
- [ ] Nhận được CHANNEL_HANGUP_COMPLETE
- [ ] Parse event variables
- [ ] Lưu CDR vào PostgreSQL

### Ngày 5: SIP Trunking (8h)

**Thực hành (8h):**
```xml
<!-- conf/sip_profiles/external/viettel.xml -->
<gateway name="viettel">
  <param name="username" value="your_username"/>
  <param name="password" value="your_password"/>
  <param name="proxy" value="sip.viettel.vn"/>
  <param name="register" value="true"/>
</gateway>
```

**Dialplan outbound:**
```xml
<extension name="outbound">
  <condition field="destination_number" expression="^(0[0-9]{9})$">
    <action application="bridge" data="sofia/gateway/viettel/$1"/>
  </condition>
</extension>
```

**Deliverables:**
- [ ] Kết nối SIP Trunk
- [ ] Gọi ra ngoài thành công
- [ ] Nhận cuộc gọi vào

---

## TUẦN 3: TÍNH NĂNG NÂNG CAO (40h)

### Ngày 1-2: IVR (Interactive Voice Response) (16h)

**Lý thuyết (4h):**
- IVR flow design
- DTMF detection
- Audio file formats

**Thực hành (12h):**
```xml
<extension name="ivr_demo">
  <condition field="destination_number" expression="^1900$">
    <action application="answer"/>
    <action application="sleep" data="1000"/>
    <action application="play_and_get_digits" 
            data="1 1 3 7000 # ivr/welcome.wav ivr/invalid.wav digit_pressed \d+"/>
    <action application="transfer" data="${digit_pressed} XML ivr_menu"/>
  </condition>
</extension>

<extension name="ivr_menu">
  <condition field="destination_number" expression="^1$">
    <action application="transfer" data="101 XML default"/>
  </condition>
  <condition field="destination_number" expression="^2$">
    <action application="transfer" data="102 XML default"/>
  </condition>
</extension>
```

**Deliverables:**
- [ ] IVR 3 cấp hoạt động
- [ ] Upload audio files
- [ ] DTMF detection
- [ ] Dynamic IVR từ database

### Ngày 3: Call Queue (mod_callcenter) (8h)

**Cấu hình (8h):**
```xml
<!-- conf/autoload_configs/callcenter.conf.xml -->
<configuration name="callcenter.conf">
  <queues>
    <queue name="support">
      <param name="strategy" value="longest-idle-agent"/>
      <param name="moh-sound" value="$${hold_music}"/>
      <param name="time-base-score" value="queue"/>
      <param name="max-wait-time" value="300"/>
      <param name="max-wait-time-with-no-agent" value="60"/>
    </queue>
  </queues>
  
  <agents>
    <agent name="1001@default" type="callback" contact="user/101"/>
    <agent name="1002@default" type="callback" contact="user/102"/>
  </agents>
  
  <tiers>
    <tier agent="1001@default" queue="support" level="1" position="1"/>
    <tier agent="1002@default" queue="support" level="1" position="2"/>
  </tiers>
</configuration>
```

**Deliverables:**
- [ ] Queue hoạt động
- [ ] Agent login/logout
- [ ] Queue strategies test
- [ ] API quản lý queue

### Ngày 4: Call Recording (8h)

**Cấu hình (8h):**
```xml
<action application="set" data="RECORD_STEREO=true"/>
<action application="set" data="recording_follow_transfer=true"/>
<action application="record_session" data="/var/recordings/${uuid}.wav"/>
```

**Recording Worker:**
```csharp
public async Task ProcessRecordingsAsync()
{
    var files = Directory.GetFiles("/var/recordings", "*.wav");
    foreach (var file in files)
    {
        // Convert WAV to MP3
        await ConvertToMp3Async(file);
        
        // Upload to MinIO
        await UploadToMinIOAsync(mp3File);
        
        // Update CDR
        await UpdateCdrRecordingUrlAsync(uuid, url);
        
        // Delete local file
        File.Delete(file);
    }
}
```

**Deliverables:**
- [ ] Recording tự động
- [ ] Convert WAV → MP3
- [ ] Upload MinIO
- [ ] Presigned URL

### Ngày 5: WebRTC (8h)

**Cấu hình (8h):**
```xml
<!-- conf/sip_profiles/internal.xml -->
<param name="ws-binding" value=":8082"/>
<param name="wss-binding" value=":8083"/>
```

**Frontend (JsSIP):**
```typescript
const socket = new JsSIP.WebSocketInterface('wss://domain.com:8083');
const configuration = {
  sockets: [socket],
  uri: 'sip:101@domain.com',
  password: '1234'
};

const ua = new JsSIP.UA(configuration);
ua.start();

// Make call
const session = ua.call('sip:0901234567@domain.com', {
  mediaConstraints: { audio: true, video: false }
});
```

**Deliverables:**
- [ ] WebSocket enabled
- [ ] Browser registration
- [ ] WebRTC call thành công
- [ ] STUN/TURN setup

---

## TUẦN 4: PRODUCTION & OPTIMIZATION (40h)

### Ngày 1-2: Performance Tuning (16h)

**Tối ưu hóa:**
```xml
<!-- conf/autoload_configs/switch.conf.xml -->
<param name="max-sessions" value="1000"/>
<param name="sessions-per-second" value="30"/>
<param name="rtp-start-port" value="16384"/>
<param name="rtp-end-port" value="32768"/>
```

**Database optimization:**
```sql
CREATE INDEX idx_cdrs_tenant_date ON cdrs(tenant_id, created_at);
CREATE INDEX idx_extensions_number ON extensions(number);
```

**Deliverables:**
- [ ] Load test 100 concurrent calls
- [ ] CPU/Memory monitoring
- [ ] Database query optimization
- [ ] Redis caching

### Ngày 3: Security Hardening (8h)

**Fail2Ban:**
```bash
# /etc/fail2ban/filter.d/freeswitch.conf
[Definition]
failregex = \[WARNING\] sofia_reg\.c:\d+ SIP auth challenge \(REGISTER\) on sofia profile \'[^']+\' for \[.*\] from ip <HOST>
```

**SIP ACL:**
```xml
<list name="domains" default="deny">
  <node type="allow" cidr="192.168.1.0/24"/>
  <node type="allow" cidr="10.0.0.0/8"/>
</list>
```

**Deliverables:**
- [ ] Fail2Ban active
- [ ] Firewall rules
- [ ] TLS/SRTP enabled
- [ ] Rate limiting

### Ngày 4-5: Monitoring & Troubleshooting (16h)

**Logging:**
```bash
# fs_cli
console loglevel DEBUG
sofia global siptrace on
sofia loglevel all 9
```

**Grafana Dashboard:**
- Active calls
- Calls per minute
- ASR (Answer Seizure Ratio)
- ACD (Average Call Duration)

**Deliverables:**
- [ ] Prometheus metrics
- [ ] Grafana dashboards
- [ ] Alert rules
- [ ] Log aggregation

---

## TÀI LIỆU THAM KHẢO

### Official Documentation
- [FreeSWITCH Wiki](https://freeswitch.org/confluence/)
- [mod_xml_curl](https://freeswitch.org/confluence/display/FREESWITCH/mod_xml_curl)
- [Event Socket Library](https://freeswitch.org/confluence/display/FREESWITCH/Event+Socket+Library)

### Books
- "FreeSWITCH 1.6 Cookbook" - Anthony Minessale
- "Mastering FreeSWITCH" - Anthony Minessale

### Video Courses
- YouTube: FreeSWITCH Basics
- Udemy: VoIP with FreeSWITCH

### Community
- FreeSWITCH Slack
- Stack Overflow #freeswitch

---

## CHECKLIST TỔNG HỢP

### Tuần 1: Cơ bản
- [ ] Cài đặt FreeSWITCH
- [ ] Tạo extensions
- [ ] Gọi nội bộ
- [ ] Viết dialplan

### Tuần 2: Tích hợp
- [ ] mod_xml_curl
- [ ] ESL Worker
- [ ] SIP Trunk
- [ ] CDR & Billing

### Tuần 3: Nâng cao
- [ ] IVR
- [ ] Queue
- [ ] Recording
- [ ] WebRTC

### Tuần 4: Production
- [ ] Performance tuning
- [ ] Security
- [ ] Monitoring
- [ ] Documentation

---

**Tổng thời gian:** 160 giờ (4 tuần × 40 giờ/tuần)  
**Kết quả:** Thành thạo FreeSWITCH, sẵn sàng phát triển dự án Call Center SaaS
