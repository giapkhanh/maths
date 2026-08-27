# Mô hình Tấn công & Độ an toàn Tính toán

> **Mục tiêu học tập**:
> - Nắm vững phân loại tấn công theo năng lực can thiệp (Passive vs Active).
> - Phân biệt rạch ròi 4 mô hình tấn công chuẩn trên hệ mã hóa: COA, KPA, CPA, CCA.
> - Phân biệt mô hình tấn công trên chữ ký số (KMA, CMA) và các mục tiêu phá vỡ (Total Break vs Existential Forgery).
> - Hiểu rõ khái niệm Độ an toàn tính toán (Computational Security).

---

## 1. Phân loại theo năng lực của Kẻ tấn công (Attack Capability)

Khi phân tích an toàn của một giao thức mật mã, ta phân loại kẻ thù ($\text{Adv}$ - Adversary) theo khả năng can thiệp vào kênh truyền:

```text
                             NĂNG LỰC KẺ TẤN CÔNG
                                      │
         ┌────────────────────────────┴────────────────────────────┐
         ▼                                                         ▼
┌──────────────────────────────┐          ┌──────────────────────────────┐
│  TẤN CÔNG THỤ ĐỘNG (PASSIVE) │          │  TẤN CÔNG CHỦ ĐỘNG (ACTIVE)  │
│  - Nghe lén (Eavesdropping)  │          │  - Chỉnh sửa dữ liệu         │
│  - Phân tích lưu lượng       │          │  - Chèn thông điệp giả mạo   │
│  - KHÔNG sửa đổi kênh truyền │          │  - Tấn công phát lại (Replay)│
│  => Đe dọa Confidentiality   │          │  - Man-in-the-Middle (MitM)  │
└──────────────────────────────┘          │  => Đe dọa Integrity & Auth  │
                                          └──────────────────────────────┘
```

---

## 2. Bốn mô hình tấn công kinh điển trên Hệ mã hóa (Encryption Attack Models)

Khả năng tiếp cận bản rõ và bản mã của kẻ tấn công được chuẩn hóa thành 4 cấp độ tăng dần:

```text
  COA (Yếu nhất)        KPA                 CPA                 CCA (Mạnh nhất)
 ┌──────────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────────┐
 │ Chỉ có tập   │───►│ Có một số    │───►│ Chọn Plaintext│──►│ Chọn Plaintext   │
 │ Ciphertext   │    │ cặp (P_i, C_i)│   │ nhận Cipher  │    │ VÀ Ciphertext    │
 └──────────────┘    └──────────────┘    └──────────────┘    └──────────────────┘
```

### 1. Tấn công chỉ có bản mã — Ciphertext-Only Attack (COA)
- **Năng lực kẻ tấn công**: Kẻ thù chỉ thu thập được một tập các bản mã $\{C_1, C_2, \dots, C_k\}$ truyền trên kênh. Kẻ thù không biết bản rõ tương ứng (ngoài việc có thể biết ngôn ngữ hoặc phân phối xác suất thống kê của bản rõ).
- **Mục tiêu**: Khôi phục lại bản rõ $M$ hoặc tìm khóa bí mật $K$.

### 2. Tấn công biết trước bản rõ — Known-Plaintext Attack (KPA)
- **Năng lực kẻ tấn công**: Kẻ thù sở hữu một tập các cặp bản rõ và bản mã tương ứng $\{(M_1, C_1), (M_2, C_2), \dots, (M_k, C_k)\}$ được mã hóa dưới cùng một khóa $K$.
- **Bối cảnh thực tế**: Trong các giao thức mạng, phần mở đầu (header) của gói tin thường có cấu trúc cố định và được biết trước.

### 3. Tấn công chọn bản rõ — Chosen-Plaintext Attack (CPA)
- **Năng lực kẻ tấn công**: Kẻ thù có quyền chọn một bản rõ bất kỳ $M$ và đưa vào một "Hộp đen mã hóa" (Encryption Oracle) để nhận về bản mã tương ứng $C = E_K(M)$.
- **Bối cảnh thực tế**: Trong **mật mã khóa công khai**, mọi kẻ tấn công đều tự động có quyền năng CPA vì khóa mã hóa $PK$ là công khai.

### 4. Tấn công chọn bản mã — Chosen-Ciphertext Attack (CCA)
- **Năng lực kẻ tấn công**: Kẻ thù có toàn bộ quyền năng của CPA, đồng thời có thêm quyền chọn các bản mã tùy ý $C'$ và yêu cầu "Hộp đen giải mã" (Decryption Oracle) giải mã để trả về bản rõ $M' = D_K(C')$.
- **Quy tắc**: Kẻ thù không được phép yêu cầu giải mã chính bản mã mục tiêu $C^*$ đang cần phá vỡ.

---

## 3. Mô hình tấn công & Mục tiêu phá vỡ trên Chữ ký số (Digital Signatures)

Chữ ký số có cơ chế hoạt động khác với mã hóa, do đó mô hình an toàn được định nghĩa riêng biệt:

### A. Mô hình tấn công (Attack Models)
1. **Known-Message Attack (KMA)**: Kẻ tấn công có được một danh sách các thông điệp và chữ ký hợp lệ tương ứng $\{(m_1, s_1), (m_2, s_2), \dots\}$ do người ký hợp pháp tạo ra.
2. **Chosen-Message Attack (CMA)**: Kẻ tấn công có quyền chọn các thông điệp tùy ý $m_i$ và yêu cầu bộ máy ký (Signing Oracle) cấp chữ ký hợp lệ $s_i$.

### B. Mức độ phá vỡ hệ thống (Goals of Adversary)
1. **Phá vỡ hoàn toàn (Total Break)**: Kẻ tấn công tìm ra được chính xác khóa bí mật dùng để ký ($SK$). Khi đó, kẻ tấn công có thể ký bất kỳ văn bản nào dưới danh nghĩa nạn nhân.
2. **Giả mạo có chọn lọc (Selective Forgery)**: Kẻ tấn công có thể tạo ra chữ ký hợp lệ cho một thông điệp cụ thể $m^*$ được ấn định trước mà không cần biết khóa bí mật.
3. **Giả mạo bất kỳ (Existential Forgery)**: Kẻ tấn công tạo ra được ít nhất một cặp $(m, s)$ hợp lệ mới bất kỳ, trong đó $m$ có thể là một chuỗi vô nghĩa hoặc không do kẻ tấn công chủ động lựa chọn trước nội dung.

> [!IMPORTANT]
> **Chuẩn an toàn vàng cho Chữ ký số (EUF-CMA)**:
> Một hệ chữ ký số được coi là an toàn hiện đại nếu nó đạt tính chất: **Kháng giả mạo bất kỳ dưới mô hình tấn công chọn thông điệp (Existential Unforgeability under Chosen-Message Attack - EUF-CMA)**. Kẻ tấn công dù được cấp chữ ký cho nhiều thông điệp tùy ý vẫn không thể tự tạo ra chữ ký hợp lệ cho một thông điệp mới $m^*$.

---

## 4. Độ an toàn tính toán (Computational Security)

Trong thực tế, trừ hệ mật One-Time Pad đạt độ an toàn tuyệt đối (*Information-Theoretic Security*), hầu hết các thuật toán mật mã hiện đại đều dựa trên **Độ an toàn tính toán (Computational Security)**.

### Định nghĩa
Một hệ mật được coi là an toàn tính toán nếu việc phá vỡ nó đòi hỏi kẻ tấn công:
1. **Vượt quá giới hạn thời gian thực tế**: Thuật toán thám mã có độ phức tạp hàm mũ (ví dụ: $O(2^{128})$ thao tác), đòi hỏi các siêu máy tính hiện đại chạy trong hàng triệu năm.
2. **Vượt quá chi phí kinh tế**: Chi phí phần cứng và năng lượng để phá mã vượt xa giá trị của thông tin được bảo vệ.

```text
┌─────────────────────────────────────────────────────────────────────────┐
│               KHÔNG GIAN KHÓA VÀ ĐỘ PHỨC TẠP TÍNH TOÁN                  │
│                                                                         │
│  - Khóa 56-bit (DES cũ):    2^56  ≈ 7.2 x 10^16  (Phá vỡ trong vài giờ) │
│  - Khóa 128-bit (AES-128): 2^128 ≈ 3.4 x 10^38  (Bất khả thi tính toán)│
│  - Khóa 256-bit (AES-256): 2^256 ≈ 1.1 x 10^77  (An toàn tuyệt đối)    │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Tóm tắt cốt lõi

1. Kẻ tấn công thụ động nghe lén kênh truyền; kẻ tấn công chủ động can thiệp, sửa đổi và chèn dữ liệu.
2. Thứ tự sức mạnh mô hình tấn công trên hệ mã hóa: $\text{COA} \subset \text{KPA} \subset \text{CPA} \subset \text{CCA}$.
3. Hệ mật mã khóa công khai luôn phải đối mặt với tấn công chọn bản rõ (CPA).
4. Tiêu chuẩn an toàn của chữ ký số là ngăn chặn giả mạo bất kỳ (EUF-CMA).

---

## 6. Câu hỏi ôn tập

1. Tại sao trong hệ mật mã khóa công khai (Asymmetric Cryptosystem), kẻ tấn công mặc nhiên luôn thực hiện được tấn công Chọn bản rõ (CPA)?
2. Phân biệt sự khác nhau về mục tiêu của kẻ tấn công giữa *Total Break* và *Existential Forgery* trong lược đồ chữ ký số.
3. Một thuật toán mã hóa có không gian khóa $2^{40}$ có đạt độ an toàn tính toán ở thời điểm hiện tại không? Giải thích dựa trên năng lực tính toán hiện đại.
