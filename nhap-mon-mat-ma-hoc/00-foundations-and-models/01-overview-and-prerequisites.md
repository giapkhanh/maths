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
