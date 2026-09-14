# BỘ CÂU HỎI ÔN TẬP & KỊCH BẢN THỰC CHIẾN - TUẦN 1
> **Chủ đề: Hạ tầng Mạng & Bảo mật AWS**

---

## 🎯 Phần 1: Tình huống thực chiến (Scenario-based Questions)

### Tình huống 1: "Ứng dụng gọi API bên ngoài bị timeout"
- **Bối cảnh:** Bạn có một Lambda/EC2 đặt trong **Private Subnet** chạy microservice `payment-service`. Khi service này thực hiện HTTP POST tới cổng thanh toán Stripe/VNPAY ở ngoài Internet thì nhận lỗi **Connection Timeout**.
- **Câu hỏi:** Bạn hãy kiểm tra những thành phần nào trong hạ tầng mạng VPC để khắc phục lỗi này?

---

### Tình huống 2: "Tại sao trang web không tải được dù đã mở Port 80 Inbound trên NACL?"
- **Bối cảnh:** Một Web Server đặt trong Public Subnet. Bạn cấu hình Network ACL (NACL) Inbound cho phép TCP port 80 từ `0.0.0.0/0`. Bạn cũng mở Port 80 trên Security Group. Tuy nhiên trình duyệt của người dùng vẫn quay tròn và báo lỗi timeout.
- **Câu hỏi:** Lỗi nằm ở đâu trong cấu hình NACL? Khái niệm nào giải thích hiện tượng này?

---

### Tình huống 3: "Chặn IP tấn công DDoS / Scan cổng"
- **Bối cảnh:** Bạn phát hiện địa chỉ IP `203.0.113.50` liên tục gửi các request tấn công vào hệ thống. Bạn muốn chặn hoàn toàn IP này.
- **Câu hỏi:** Bạn sẽ dùng **Security Group** hay **Network ACL**? Vì sao?

---

## 💡 Phần 2: Flashcards khái niệm cốt lõi

| Khái niệm | Ý nghĩa 1 câu ngắn gọn |
| :--- | :--- |
| **VPC** | "Nhà riêng" ảo của bạn trên Cloud, cô lập hoàn toàn với các tài khoản khác. |
| **CIDR** | Phương pháp định dải địa chỉ IP (ví dụ: `/16` là 65,536 IP, `/24` là 256 IP). |
| **Public Subnet** | Subnet có đường ra thẳng Internet qua Internet Gateway. |
| **Private Subnet** | Subnet kín, an toàn, không có route trực tiếp ra Internet. |
| **Internet Gateway (IGW)** | Chiếc cổng 2 chiều mở ra Internet toàn cầu. |
| **NAT Gateway** | Chiếc cửa xoay 1 chiều: Bên trong Private ra ngoài được, bên ngoài không thể chọc vào. |
| **Security Group** | Chốt bảo vệ ngay trước cửa phòng (ENI/Instance) - Có nhớ trạng thái (Stateful). |
| **Network ACL** | Trạm gác barrier ở cổng làng (Subnet) - Không nhớ trạng thái (Stateless). |
