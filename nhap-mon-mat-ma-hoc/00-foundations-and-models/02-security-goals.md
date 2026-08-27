# Mục tiêu An toàn & Phân định Thuật ngữ Bảo mật

> **Mục tiêu học tập**:
> - Hiểu sâu sắc bộ ba mục tiêu CIA: Confidentiality, Integrity, Availability.
> - Phân biệt chính xác giữa Tính xác thực thực thể, Xác thực nguồn gốc dữ liệu và Tính toàn vẹn.
> - Hiểu đúng bản chất của Tính chống chối bỏ (Non-repudiation) dưới góc độ mật mã học và góc độ hệ thống/pháp lý.

---

## 1. Bộ ba mục tiêu bảo mật kinh điển (CIA Triad)

Mọi hệ thống an toàn thông tin đều xoay quanh ba trụ cột cốt lõi:

```text
                        ┌────────────────────────┐
                        │    BẢO MẬT HỆ THỐNG    │
                        └───────────┬────────────┘
                                    │
         ┌──────────────────────────┼──────────────────────────┐
         ▼                          ▼                          ▼
┌──────────────────┐       ┌──────────────────┐       ┌──────────────────┐
│  Confidentiality │       │    Integrity     │       │   Availability   │
│  (Tính bí mật)   │       │ (Tính toàn vẹn)  │       │  (Tính khả dụng) │
└──────────────────┘       └──────────────────┘       └──────────────────┘
```

1. **Tính bí mật (Confidentiality)**: Đảm bảo thông tin chỉ được truy cập và đọc hiểu bởi những thực thể được cấp quyền. Kẻ nghe lén trên kênh truyền không thể trích xuất được thông tin hữu ích từ bản mã.
   - *Công cụ mật mã*: Mã hóa đối xứng (DES, AES), Mã hóa khóa công khai (RSA, ElGamal).
2. **Tính toàn vẹn (Integrity)**: Đảm bảo dữ liệu không bị sửa đổi, chèn thêm, xóa bớt hoặc hoán đổi vị trí một cách trái phép trong quá trình truyền hoặc lưu trữ mà không bị phát hiện.
   - *Công cụ mật mã*: Hàm băm mật mã (SHA-256), Mã xác thực thông điệp (MAC).
3. **Tính khả dụng (Availability)**: Đảm bảo hệ thống và dữ liệu luôn sẵn sàng phục vụ người dùng hợp lệ khi có yêu cầu (phòng chống tấn công từ chối dịch vụ DoS/DDoS).

---

## 2. Các mục tiêu an toàn mở rộng trong Mật mã học

Ngoài CIA, mật mã học hiện đại giải quyết trực tiếp 3 bài toán an toàn thông tin then chốt:

### A. Xác thực nguồn gốc dữ liệu (Data Origin Authentication)
Đảm bảo rằng thông điệp thực sự xuất phát từ đúng thực thể tuyên bố gửi nó, không phải do kẻ tấn công mạo danh tạo ra.

### B. Tính toàn vẹn dữ liệu (Data Integrity)
Khẳng định nội dung nhận được hoàn toàn trùng khớp với nội dung đã được gửi đi.

> [!NOTE]
> Trong thực tế, *Xác thực nguồn gốc dữ liệu* và *Tính toàn vẹn dữ liệu* luôn gắn liền với nhau. Nếu thông điệp bị thay đổi trên đường truyền, nguồn gốc dữ liệu của phần bị thay đổi đó không còn được xác thực.

### C. Tính chống chối bỏ (Non-repudiation)
Ngăn chặn người gửi phủ nhận việc mình đã tạo ra và gửi thông điệp trong quá khứ, hoặc ngăn chặn người nhận phủ nhận việc đã nhận thông điệp.

---

## 3. Phân định chính xác: Thuộc tính Mật mã vs Thuộc tính Hệ thống

Một trong những nhầm lẫn phổ biến nhất là đánh đồng:

$$\text{Chữ ký số} \iff \text{Tính chống chối bỏ}$$

Để hiểu đúng bản chất môn học, ta phải phân định rạch ròi hai tầng:

```text
┌─────────────────────────────────────────────────────────────────────────┐
│ TẦNG HỆ THỐNG / PHÁP LÝ: TÍNH CHỐNG CHỐI BỎ (NON-REPUDIATION)           │
│ Đòi hỏi: Chứng cứ pháp lý + PKI + Đóng dấu thời gian + Bảo vệ khóa      │
└────────────────────────────────────▲────────────────────────────────────┘
                                     │ được xây dựng dựa trên
┌────────────────────────────────────┴────────────────────────────────────┐
│ TẦNG NGUYÊN THỦY MẬT MÃ (CRYPTOGRAPHIC PRIMITIVES)                      │
│ Cung cấp: Xác thực nguồn gốc (Origin Auth) + Tính toàn vẹn (Integrity)   │
└─────────────────────────────────────────────────────────────────────────┘
```

1. **Góc độ toán học & nguyên thủy mật mã**:
   - Chữ ký số (Digital Signature) về mặt toán học chỉ cung cấp: **Xác thực nguồn gốc** (thông điệp được tạo ra bởi khóa riêng $SK$) và **Toàn vẹn dữ liệu** (thông điệp không bị chỉnh sửa sau khi ký).
2. **Góc độ hệ thống và pháp lý**:
   - Tính chống chối bỏ không thể đạt được chỉ bằng một phép tính toán học. Nó là thuộc tính cấp hệ thống đòi hỏi:
     - **Bảo vệ khóa an toàn**: Khóa riêng không bị đánh cắp, sao chép hoặc chia sẻ.
     - **Hạ tầng khóa công khai (PKI) & Chứng thư số hợp lệ**: Định danh của chủ thể được xác thực bởi bên thứ ba tin cậy (CA).
     - **Mốc thời gian tin cậy (Trusted Timestamping)**: Chứng minh chữ ký được tạo ra trước khi chứng thư số hết hạn hoặc bị thu hồi.
     - **Quy trình vận hành & Pháp lý**: Luật giao dịch điện tử thừa nhận giá trị ràng buộc của chữ ký số.

---

## 4. Tóm tắt cốt lõi

| Mục tiêu bảo mật | Định nghĩa ngắn gọn | Công cụ mật mã giải quyết |
| :--- | :--- | :--- |
| **Confidentiality** | Ngăn chặn đọc lén | Mã hóa khóa đối xứng, Mã hóa khóa công khai |
| **Data Integrity** | Ngăn chặn sửa đổi dữ liệu | Hàm băm (Hash), Mã xác thực thông điệp (MAC) |
| **Origin Authentication** | Xác định danh tính nguồn gửi | MAC (giữa 2 bên có khóa chung), Chữ ký số |
| **Non-repudiation** | Không thể phủ nhận hành vi | Chữ ký số + PKI + Timestamping + Quản trị khóa |

---

## 5. Câu hỏi ôn tập

1. Phân biệt sự khác nhau cơ bản giữa Mã hóa (Encryption) và Hàm băm (Hash Function) về mặt mục tiêu an toàn.
2. Tại sao một hệ thống chỉ dùng mã hóa đối xứng (Symmetric Encryption) thì KHÔNG THỂ cung cấp tính chống chối bỏ giữa hai bên gửi và nhận?
3. Nếu một người ký số bị lộ khóa riêng (Private Key Compromise), thuộc tính an toàn nào bị phá vỡ đầu tiên? Tại sao điều này làm vô hiệu hóa tính chống chối bỏ?
