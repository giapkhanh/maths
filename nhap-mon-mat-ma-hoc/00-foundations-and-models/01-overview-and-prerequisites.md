# Tổng quan Mật mã học & Các mô hình bảo mật cơ bản

> **Mục tiêu học tập**:
> - Hiểu rõ sự khác biệt giữa Mật mã học (Cryptography), Thám mã học (Cryptanalysis) và Mật mã tổng thể (Cryptology).
> - Nắm vững Mô hình truyền tin an toàn của Claude Shannon.
> - Hiểu và vận dụng triệt để Nguyên lý Kerckhoffs trong thiết kế và đánh giá hệ mật.

---

## 1. Khái niệm cốt lõi & Trực giác

Trong thế giới truyền thông số, dữ liệu phải đi qua các kênh truyền công khai không an toàn (Internet, sóng vô tuyến). Bất kỳ ai có quyền truy cập vào hạ tầng mạng đều có thể nghe lén, chỉnh sửa hoặc giả mạo thông điệp.

**Mật mã học (Cryptography)** ra đời nhằm giải quyết bài toán: *Làm thế nào để hai bên có thể giao tiếp an toàn thông qua một kênh truyền không đáng tin cậy dưới sự hiện diện của kẻ thù (Adversary).*

```text
                 ┌──────────────────────────────────────────────────┐
                 │             Mật mã tổng thể (Cryptology)         │
                 └────────────────────────┬─────────────────────────┘
                                          │
                  ┌───────────────────────┴───────────────────────┐
                  ▼                                               ▼
    ┌───────────────────────────┐                   ┌───────────────────────────┐
    │   Mật mã học              │                   │   Thám mã học             │
    │   (Cryptography)          │                   │   (Cryptanalysis)         │
    │   (Nghệ thuật & Khoa học  │                   │   (Khoa học phá vỡ các    │
    │    xây dựng hệ mật an toàn│                   │    hệ thống mật mã)       │
    └───────────────────────────┘                   └───────────────────────────┘
```

---

## 2. Mô hình hệ mật mã tổng quát của Shannon

Năm 1949, Claude Shannon đã chuẩn hóa mô hình toán học cho việc truyền tin bí mật:

```text
  Bản rõ (M)                                                             Bản rõ (M)
 ───────────►┌────────────────┐       Bản mã (C)       ┌────────────────┐───────────►
             │  Thuật toán    │───────────────────────►│  Thuật toán    │
             │  Mã hóa (E)    │   Kênh không an toàn   │  Giải mã (D)   │
             └────────────────┘                        └────────────────┘
                     ▲                                         ▲
                     │ Khóa K                                  │ Khóa K
           ┌─────────┴─────────┐                     ┌─────────┴─────────┐
           │ Nguồn sinh khóa   │                     │ Nguồn sinh khóa   │
           └───────────────────┘                     └───────────────────┘
                                          ▲
                                          │ Nghe lén / Tấn công
                                   ┌──────┴──────┐
                                   │ Kẻ thù (Adv)│
                                   └─────────────┘
```

### Định nghĩa toán học

Một **Hệ mật mã (Cryptosystem)** là một bộ 5 thành phần:

$$\mathcal{S} = (\mathcal{M}, \mathcal{C}, \mathcal{K}, \mathcal{E}, \mathcal{D})$$

*Trong đó:*
- $\mathcal{M}$ (*Plaintext Space*): Không gian bản rõ chứa các thông điệp có nghĩa.
- $\mathcal{C}$ (*Ciphertext Space*): Không gian bản mã chứa các thông điệp đã được xáo trộn.
- $\mathcal{K}$ (*Key Space*): Không gian khóa chứa tập hợp tất cả các khóa hợp lệ.
- $\mathcal{E} = \{E_K : K \in \mathcal{K}\}$: Tập hợp các hàm mã hóa. Với mỗi $K \in \mathcal{K}$, hàm mã hóa ánh xạ:
  $$E_K: \mathcal{M} \to \mathcal{C}, \quad C = E_K(M)$$
- $\mathcal{D} = \{D_K : K \in \mathcal{K}\}$: Tập hợp các hàm giải mã. Với mỗi $K \in \mathcal{K}$, hàm giải mã ánh xạ:
  $$D_K: \mathcal{C} \to \mathcal{M}, \quad M = D_K(C)$$

### Điều kiện đúng đắn (Correctness Requirement)
Hệ mật phải đảm bảo rằng quá trình giải mã bằng đúng khóa hợp lệ luôn phục hồi chính xác bản rõ ban đầu:

$$\forall K \in \mathcal{K}, \quad \forall M \in \mathcal{M}: \quad D_K(E_K(M)) = M$$

---

## 3. Nguyên lý Kerckhoffs & Đánh giá an toàn

Năm 1883, nhà mật mã học Auguste Kerckhoffs đã phát biểu nguyên lý nền tảng của mật mã học hiện đại:

> **Nguyên lý Kerckhoffs (Kerckhoffs's Principle):**
> *Độ an toàn của một hệ mật mã không được dựa vào việc giữ bí mật thuật toán. Độ an toàn chỉ được phép phụ thuộc duy nhất vào việc giữ bí mật của chiếc chìa khóa (Key).*

### Tại sao nguyên lý này lại sống còn?
1. **Bảo mật qua sự che giấu (Security through Obscurity) luôn thất bại**: Một thuật toán sớm muộn sẽ bị đảo ngược kỹ thuật (reverse-engineered), bị rò rỉ bởi nhân viên nội bộ, hoặc bị phân tích từ mã máy.
2. **Chi phí thay đổi**: Nếu thuật toán bị lộ, việc thiết kế lại một thuật toán mới và cập nhật toàn bộ phần cứng/phần mềm là vô cùng đắt đỏ. Ngược lại, nếu khóa bị lộ, việc đổi một chuỗi khóa mới diễn ra trong vài mili-giây.
3. **Thẩm định công khai (Public Scrutiny)**: Các chuẩn mật mã mạnh nhất thế giới (như AES, RSA, SHA-3) đều được công khai hoàn toàn để hàng ngàn nhà toán học và chuyên gia thám mã toàn cầu tìm cách phá hủy trong nhiều năm trước khi được đưa vào ứng dụng.

> [!CAUTION]
> Tuyệt đối không tự sáng tạo thuật toán mã hóa riêng trong các bài toán thực tế rồi giữ bí mật thuật toán. Mọi thiết kế mật mã an toàn bắt buộc phải giả định kẻ tấn công biết toàn bộ mã nguồn thuật toán, chỉ trừ giá trị cụ thể của khóa bí mật $K$.

---

## 4. Tóm tắt cốt lõi

1. Hệ mật mã là một bộ 5 $(\mathcal{M}, \mathcal{C}, \mathcal{K}, \mathcal{E}, \mathcal{D})$ thỏa mãn tính khả nghịch $D_K(E_K(M)) = M$.
2. Toàn bộ độ an toàn của hệ thống tập trung tại không gian khóa $\mathcal{K}$ và việc giữ bí mật của $K$.
3. Kẻ tấn công luôn được giả định là biết rõ thuật toán mã hóa, cấu trúc dữ liệu và môi trường truyền tin.

---

## 5. Câu hỏi ôn tập

1. Phân biệt sự khác nhau giữa bài toán của một nhà thiết kế mật mã (Cryptographer) và một chuyên gia thám mã (Cryptanalyst).
2. Tại sao nguyên tắc "Bảo mật qua sự che giấu" (Security through obscurity) lại bị coi là phản mẫu thiết kế (anti-pattern) trong an toàn thông tin?
3. Giả sử một công ty tự phát triển thuật toán mã hóa độc quyền và từ chối công khai mã nguồn vì lý do bảo mật. Dưới góc nhìn của Nguyên lý Kerckhoffs, hãy nhận xét về độ tin cậy của hệ thống này.

# FILE 03/44: `00-foundations-and-models/02-security-goals.md`

```markdown
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
```

---

# FILE 04/44: `00-foundations-and-models/03-threat-models.md`

```markdown
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
