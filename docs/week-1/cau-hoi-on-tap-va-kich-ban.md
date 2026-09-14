# 🎯 BỘ CÂU HỎI ÔN TẬP, KỊCH BẢN THỰC CHIẾN & ĐÁP ÁN CHUYÊN SÂU - TUẦN 1
> **Chủ đề: Tối ưu Hạ tầng Mạng & Bảo mật AWS (AWS VPC & Defense-in-Depth)**

---

## MỤC LỤC
1. [Phần 1: Kịch bản Thực chiến (Scenario-based Interview Questions)](#phần-1-kịch-bản-thực-chiến)
2. [Phần 2: Câu hỏi Trắc nghiệm & Phân tích Đánh lừa (Tricky MCQs)](#phần-2-câu-hỏi-trắc-nghiệm--phân-tích-đánh-lừa)
3. [Phần 3: Bài tập Tính toán CIDR & Quy hoạch Subnet](#phần-3-bài-tập-tính-toán-cidr--quy-hoạch-subnet)
4. [Phần 4: Lời giải & Phân tích Kiến trúc Chi tiết](#phần-4-lời-giải--phân-tích-kiến-trúc-chi-tiết)

---

## Phần 1: Kịch bản Thực chiến

### 🚨 Kịch bản 1: "Chi phí NAT Gateway tăng vọt bất thường"
- **Bối cảnh:** Bạn triển khai hệ thống AI Recommendation Service và Product Service trên các worker đặt trong **Private Subnet**. Hàng ngày, worker đọc hàng triệu file ảnh sản phẩm từ **Amazon S3** và hàng chục triệu bản ghi từ **DynamoDB**. Cuối tháng, sếp gọi bạn lên vì hóa đơn AWS phát sinh **$1,500 chi phí NAT Gateway Data Processing**.
- **Câu hỏi kỹ thuật:**
  1. Tại sao đọc ghi S3 và DynamoDB lại làm phát sinh chi phí NAT Gateway?
  2. Hãy đề xuất giải pháp kiến trúc giải quyết triệt để chi phí này mà không cần di dời worker sang Public Subnet? Chi phí của giải pháp mới là bao nhiêu?

---

### 🚨 Kịch bản 2: "Trang thương mại điện tử bị sập một phần khi một Data Center gặp sự cố"
- **Bối cảnh:** Toàn bộ Public Subnet và Private Subnet hiện tại được bạn tạo trong Availability Zone `ap-southeast-1a`. Một cơn bão lớn làm mất điện toàn bộ Data Center `ap-southeast-1a` của AWS. Dù bạn có cấu hình Auto Scaling, hệ thống vẫn chết hoàn toàn.
- **Câu hỏi kỹ thuật:**
  1. Làm thế nào để tái cấu trúc VPC đạt chuẩn **Multi-AZ High Availability**?
  2. Số lượng Subnet tối thiểu cần tạo là bao nhiêu và bố trí Route Table như thế nào?
  3. Cần bao nhiêu NAT Gateway để hệ thống vẫn sống sót nếu 1 AZ mất mạng hoàn toàn?

---

### 🚨 Kịch bản 3: "Hacker quét cổng (Port Scanning) và tấn công Brute Force SSH"
- **Bối cảnh:** Bạn có một Bastion Host (Jumpbox) đặt trong Public Subnet để DevOps SSH vào quản trị. CloudWatch phát hiện IP `45.33.32.156` liên tục thử hàng chục nghìn mật khẩu SSH mỗi phút.
- **Câu hỏi kỹ thuật:**
  1. Bạn vào Security Group của Bastion Host để thêm rule chặn IP này nhưng nhận ra không thể làm được. Tại sao?
  2. Bạn phải cấu hình thành phần nào để chặn đứng IP này ngay tại biên giới mạng? Viết rõ rule cần cấu hình!
  3. Cách tốt nhất (Best Practice) theo tiêu chuẩn AWS để loại bỏ hoàn toàn nguy cơ này mà thậm chí không cần mở cổng SSH 22 ra Internet là gì?

---

### 🚨 Kịch bản 4: "Bí ẩn gói tin biến mất trên đường về (Ephemeral Port Trap)"
- **Bối cảnh:** Một Junior DevOps tạo mới một Custom Network ACL (NACL) cho Public Subnet. Bạn ấy cấu hình:
  - **Inbound Rules:** `Rule 100: Allow HTTP (Port 80) from 0.0.0.0/0`
  - **Outbound Rules:** `Rule 100: Allow HTTP (Port 80) to 0.0.0.0/0`
  Khi người dùng truy cập vào Web Server bằng trình duyệt, kết nối hoàn toàn bị treo (Timeout).
- **Câu hỏi kỹ thuật:**
  1. Tại sao gói tin đã vào được server mà người dùng vẫn bị Timeout?
  2. Trình duyệt client dùng cổng nào để nhận dữ liệu phản hồi? Sửa rule NACL như thế nào để web hoạt động ngay lập tức?

---

## Phần 2: Câu hỏi Trắc nghiệm & Phân tích Đánh lừa

### Câu 1: Số lượng IP khả dụng
Một công ty tạo một Subnet mới trong VPC với dải CIDR là `172.16.5.0/26`. Số lượng máy chủ EC2 tối đa có thể khởi chạy và nhận Private IP trong Subnet này là bao nhiêu?
- A. 64
- B. 62
- C. 59
- D. 58

### Câu 2: Đặc tính của Internet Gateway (IGW)
Phát biểu nào sau đây là **CHÍNH XÁC NHẤT** về Internet Gateway trong AWS VPC?
- A. IGW là một máy ảo EC2 chuyên dụng có băng thông tối đa 10 Gbps.
- B. Một VPC có thể gắn (attach) đồng thời 2 hoặc nhiều Internet Gateway để tăng băng thông.
- C. IGW là một thành phần phần mềm phân tán, co giãn tự động và không tạo ra nút thắt cổ chai (bottleneck).
- D. IGW chỉ cho phép chiều Outbound từ trong VPC ra ngoài Internet.

### Câu 3: Security Group Referencing
Trong kiến trúc 3 lớp (Load Balancer $\rightarrow$ Web App $\rightarrow$ Database RDS), cách cấu hình Security Group Inbound của Database nào sau đây là an toàn nhất?
- A. Mở Port 3306 với Source là CIDR của VPC (`10.0.0.0/16`).
- B. Mở Port 3306 với Source là CIDR của App Private Subnet (`10.0.10.0/24`).
- C. Mở Port 3306 với Source là ID của chính Security Group của Web App (`sg-webapp-id`).
- D. Mở Port 3306 với Source là `0.0.0.0/0` kèm mật khẩu database mạnh.

---

## Phần 3: Bài tập Tính toán CIDR & Quy hoạch Subnet

**Đề bài:** Bạn được giao quyền thiết kế mạng cho dự án AWS 4-Week Challenge với dải IP gốc của VPC là `10.0.0.0/16`.  
Hệ thống yêu cầu triển khai trên **2 Availability Zones** (`ap-southeast-1a` và `ap-southeast-1b`), gồm 3 phân vùng (Tier):
1. **Public Tier:** Dành cho ALB và NAT Gateway.
2. **Application Tier:** Dành cho Microservices (Product, Order, Payment, AI).
3. **Database Tier:** Dành cho RDS/Aurora và ElastiCache.

👉 **Yêu cầu:** Hãy lập bảng quy hoạch mạng chi tiết: Tên Subnet, Availability Zone, CIDR Block, Tổng số IP, và Số IP khả dụng.

---

## Phần 4: Lời giải & Phân tích Kiến trúc Chi tiết

### Lời giải Kịch bản 1 (Chi phí NAT Gateway):
1. **Nguyên nhân:** S3 và DynamoDB có endpoint công khai ngoài VPC. Khi máy chủ trong Private Subnet gọi đến chúng, mặc định lưu lượng đi qua NAT Gateway. NAT Gateway thu phí **$0.045 / GB dữ liệu truyền tải**. Với hàng Terabyte ảnh và dữ liệu đọc ghi, chi phí tăng vọt.
2. **Giải pháp:** Tạo **VPC Gateway Endpoints** cho S3 và DynamoDB:
   - AWS sẽ tự động thêm Route dạng: `Prefix-List-S3 -> vpce-xxxx` vào Route Table của Private Subnet.
   - Traffic sẽ chạy hoàn toàn qua đường truyền nội bộ của AWS Data Center.
   - **Chi phí:** Hoàn toàn **MIỄN PHÍ** ($0/tháng và $0/GB xử lý).

### Lời giải Kịch bản 3 (Chặn Hacker Brute Force):
1. **Tại sao SG không làm được:** Security Group là **Allow-only (Chỉ có cho phép)**, không hỗ trợ quy tắc DENY.
2. **Cấu hình NACL:**
   - Vào Network ACL gắn với Public Subnet.
   - Thêm Inbound Rule:  
     `Rule Number: 50` | `Type: ALL Traffic (hoặc SSH)` | `Source: 45.33.32.156/32` | `Action: DENY`.
   *(Lưu ý: Rule number phải nhỏ hơn Rule 100 Default Allow để được ưu tiên xử lý trước).*
3. **Best Practice hiện đại:** Sử dụng **AWS Systems Manager (SSM) Session Manager**:
   - Gỡ bỏ hoàn toàn việc mở cổng 22 ra ngoài Internet.
   - Cài SSM Agent trên máy chủ.
   - Kỹ sư đăng nhập thông qua AWS Console hoặc AWS CLI có xác thực IAM và MFA. Tuyệt đối an toàn trước mọi đợt scan port.

### Lời giải Bài tập Tính toán CIDR (Phần 3):
Một mẫu thiết kế chuẩn Enterprise 3-Tier Multi-AZ:

| Tên Subnet | AZ | CIDR Block | Tổng IP | IP khả dụng | Mục đích sử dụng |
| :--- | :--- | :--- | :---: | :---: | :--- |
| `ws-public-1a` | `ap-southeast-1a` | `10.0.1.0/24` | 256 | 251 | ALB Node A, NAT Gateway A |
| `ws-public-1b` | `ap-southeast-1b` | `10.0.2.0/24` | 256 | 251 | ALB Node B, NAT Gateway B |
| `ws-app-private-1a` | `ap-southeast-1a` | `10.0.10.0/24` | 256 | 251 | Product, Order Microservices |
| `ws-app-private-1b` | `ap-southeast-1b` | `10.0.11.0/24` | 256 | 251 | Payment, AI Microservices |
| `ws-db-private-1a` | `ap-southeast-1a` | `10.0.20.0/24` | 256 | 251 | RDS Primary Database |
| `ws-db-private-1b` | `ap-southeast-1b` | `10.0.21.0/24` | 256 | 251 | RDS Standby Read-Replica |

*(Dải IP từ `10.0.30.0/24` trở đi vẫn còn dư hàng trăm subnet để mở rộng quy mô dự án sau này).*
