# DANH MỤC TÀI LIỆU HỌC FREESWITCH

> [!NOTE]
> Bộ tài liệu học FreeSWITCH từ cơ bản đến nâng cao trong 4 tuần

## 📚 Tổng quan

Bộ tài liệu này được thiết kế để giúp bạn học FreeSWITCH từ con số 0 đến có thể phát triển đầy đủ các chức năng cho dự án Call Center SaaS.

**Thời gian:** 4 tuần (160 giờ)  
**Phương pháp:** 75% thực hành, 25% lý thuyết

---

## 📖 Danh sách tài liệu

### Tuần 1: Cơ bản FreeSWITCH (40h)

| File | Nội dung | Thời gian |
|------|----------|-----------|
| [00_TONG_QUAN_FREESWITCH.md](./00_TONG_QUAN_FREESWITCH.md) | Giới thiệu FreeSWITCH, kiến trúc, khái niệm cơ bản | 2h |
| [NGAY_01_02_CAI_DAT_FREESWITCH.md](./NGAY_01_02_CAI_DAT_FREESWITCH.md) | Cài đặt từ source, cấu hình cơ bản | 16h |
| [NGAY_03_04_SIP_EXTENSIONS.md](./NGAY_03_04_SIP_EXTENSIONS.md) | SIP protocol, tạo extensions, gọi nội bộ | 16h |
| [NGAY_05_DIALPLAN_NANG_CAO.md](./NGAY_05_DIALPLAN_NANG_CAO.md) | Dialplan XML, regex, conditions, applications | 8h |

### Tuần 2: Tích hợp Backend (40h)

| File | Nội dung | Thời gian |
|------|----------|-----------|
| [NGAY_06_07_MOD_XML_CURL.md](./NGAY_06_07_MOD_XML_CURL.md) | mod_xml_curl, tích hợp .NET API, dynamic config | 16h |
| [NGAY_08_09_EVENT_SOCKET_LAYER.md](./NGAY_08_09_EVENT_SOCKET_LAYER.md) | ESL, lắng nghe events, lưu CDR, billing | 16h |
| NGAY_10_SIP_TRUNKING.md | Kết nối SIP Trunk, gọi ra ngoài | 8h |

### Tuần 3: Tính năng nâng cao (40h)

| File | Nội dung | Thời gian |
|------|----------|-----------|
| NGAY_11_12_IVR.md | IVR Builder, DTMF, audio files | 16h |
| NGAY_13_CALL_QUEUE.md | mod_callcenter, queue strategies | 8h |
| NGAY_14_CALL_RECORDING.md | Recording, convert WAV→MP3, MinIO | 8h |
| NGAY_15_WEBRTC.md | WebRTC, JsSIP, softphone trên browser | 8h |

### Tuần 4: Production (40h)

| File | Nội dung | Thời gian |
|------|----------|-----------|
| NGAY_16_17_PERFORMANCE.md | Performance tuning, load testing | 16h |
| NGAY_18_SECURITY.md | Security hardening, Fail2Ban, TLS | 8h |
| NGAY_19_20_MONITORING.md | Prometheus, Grafana, logging | 16h |

---

## 🎯 Lộ trình học

### Tuần 1: Nền tảng
```
Ngày 1-2: Cài đặt FreeSWITCH
    ↓
Ngày 3-4: Tạo extensions, gọi nội bộ
    ↓
Ngày 5: Viết dialplan
```

**Kết quả:** Hiểu cơ bản FreeSWITCH, gọi nội bộ thành công

### Tuần 2: Tích hợp
```
Ngày 6-7: mod_xml_curl (FS ↔ API)
    ↓
Ngày 8-9: ESL (Events, CDR, Billing)
    ↓
Ngày 10: SIP Trunk (Gọi ra ngoài)
```

**Kết quả:** Extensions đăng ký qua API, CDR tự động lưu

### Tuần 3: Tính năng
```
Ngày 11-12: IVR
    ↓
Ngày 13: Queue
    ↓
Ngày 14: Recording
    ↓
Ngày 15: WebRTC
```

**Kết quả:** Tất cả tính năng dự án hoạt động

### Tuần 4: Production
```
Ngày 16-17: Performance
    ↓
Ngày 18: Security
    ↓
Ngày 19-20: Monitoring
```

**Kết quả:** Sẵn sàng deploy production

---

## ✅ Checklist tổng hợp

### Tuần 1
- [ ] Cài đặt FreeSWITCH thành công
- [ ] Tạo được 10 extensions
- [ ] Gọi nội bộ thành công
- [ ] Viết được dialplan cơ bản

### Tuần 2
- [ ] mod_xml_curl hoạt động
- [ ] Extensions đăng ký qua API
- [ ] ESL Worker lưu CDR
- [ ] Billing tính toán chính xác
- [ ] Gọi ra ngoài qua SIP Trunk

### Tuần 3
- [ ] IVR 3 cấp hoạt động
- [ ] Queue phân phối cuộc gọi
- [ ] Recording tự động upload
- [ ] WebRTC gọi được trên browser

### Tuần 4
- [ ] Load test 100 concurrent calls
- [ ] Security hardening
- [ ] Monitoring dashboard
- [ ] Production ready

---

## 📝 Ghi chú quan trọng

### Trước khi bắt đầu

1. **Chuẩn bị server:**
   - VPS/Cloud: 4 vCPU, 8GB RAM minimum
   - OS: Debian 12 hoặc Ubuntu 22.04
   - Bandwidth: 100Mbps+

2. **Kiến thức cần có:**
   - Linux command line cơ bản
   - .NET/C# (cho Backend)
   - SQL cơ bản
   - Networking cơ bản (IP, Port, NAT)

3. **Tools cần cài:**
   - Zoiper hoặc Linphone (Softphone)
   - Postman (Test API)
   - Visual Studio Code
   - DBeaver (Database client)

### Cách sử dụng tài liệu

1. **Đọc theo thứ tự** - Mỗi tài liệu build trên kiến thức trước đó
2. **Làm hết bài tập** - Thực hành là quan trọng nhất
3. **Ghi chú lại** - Viết note riêng khi học
4. **Test ngay** - Mỗi phần học xong phải test

### Tips học hiệu quả

- ⏰ **Học đều đặn:** 2-3 giờ/ngày tốt hơn 10 giờ/ngày
- 🎯 **Focus:** Tắt notifications khi học
- 📝 **Ghi chú:** Viết lại bằng ngôn ngữ của bạn
- 🤝 **Hỏi đáp:** Join FreeSWITCH Slack community
- 🔄 **Lặp lại:** Review lại kiến thức cũ

---

## 🆘 Hỗ trợ

### Khi gặp lỗi

1. **Đọc logs:**
   ```bash
   tail -f /usr/local/freeswitch/log/freeswitch.log
   ```

2. **Google error message** - Thường có người gặp lỗi tương tự

3. **Check FreeSWITCH Wiki** - Tài liệu chính thức

4. **Hỏi trên Slack** - Community rất nhiệt tình

### Resources

- 📚 [FreeSWITCH Wiki](https://freeswitch.org/confluence/)
- 💬 [FreeSWITCH Slack](https://signalwire.community/)
- 🎥 [YouTube Tutorials](https://www.youtube.com/results?search_query=freeswitch+tutorial)
- 📖 [FreeSWITCH Book](https://www.packtpub.com/product/mastering-freeswitch/9781784398880)

---

## 🚀 Bắt đầu học

Hãy bắt đầu với:
👉 [00_TONG_QUAN_FREESWITCH.md](./00_TONG_QUAN_FREESWITCH.md)

**Chúc bạn học tốt!** 💪
