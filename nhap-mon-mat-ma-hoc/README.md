# Nhập môn Mật mã học (Introduction to Cryptography) — Khung Tri thức Chuẩn hóa

> Bộ tài liệu tự học có cấu trúc, chuẩn hóa theo giáo trình đại học, tập trung vào bản chất toán học, cơ chế giải thuật, phân tích an toàn và sổ tay tính tay phục vụ thi cử.

---

## 1. Triết lý thiết kế repository

Tài liệu này được xây dựng dựa trên các nguyên tắc sư phạm cốt lõi:

1. **Knowledge Dependency Graph**: Kiến thức được tổ chức theo mối quan hệ phụ thuộc logic tự nhiên (Toán học $\rightarrow$ Mật mã đối xứng $\rightarrow$ Mật mã bất đối xứng $\rightarrow$ Chữ ký số $\rightarrow$ Giao thức), không sao chép thứ tự tuyến tính rời rạc của các video bài giảng.
2. **Single Source of Truth (Một nguồn chân lý)**: Mỗi khái niệm nền tảng chỉ được định nghĩa chi tiết tại một vị trí duy nhất. Các module ứng dụng phía sau sẽ dẫn link tham chiếu thay vì lặp lại lý thuyết.
3. **Hiểu bản chất & Tính toán thực hành**: Không trình bày công thức suông. Mọi thuật toán trọng tâm đều đi kèm ví dụ số nhỏ (*Toy Example*) có thể bấm máy tính cầm tay từng bước.
4. **Phân định rạch ròi lý thuyết & ứng dụng**: Phân biệt rõ giữa nguyên thủy toán học (*Cryptographic Primitives*) và các lược đồ ứng dụng thực tế (*Engineering Constructions/Padding*).

---

## 2. Quy chuẩn ký hiệu & GitHub Alerts

Tài liệu sử dụng hệ thống GitHub Callouts chuẩn để làm nổi bật các loại thông tin:

> [!NOTE]
> **Kiến thức mở rộng / Ngữ cảnh thực tế**: Các tiêu chuẩn công nghiệp hiện đại (như AES-GCM, RSA-PSS, PKCS#1 v1.5, SHA-3) nhằm bổ trợ ngữ cảnh nhưng không nằm trong trọng tâm thi cử lý thuyết số học.

> [!TIP]
> **Mẹo giải bài & Bấm máy**: Kỹ thuật bấm máy tính Casio, lập bảng số học nhanh và phương pháp tính nhẩm trong phòng thi.

> [!IMPORTANT]
> **Điều kiện tiên quyết**: Các điều kiện toán học bắt buộc phải kiểm tra trước khi thực hiện thuật toán (ví dụ: $\gcd(a, m) = 1$, tính nguyên tố, cấp của nhóm).

> [!WARNING]
> **Lỗ hổng bảo mật chết người**: Các sai lầm khi triển khai dẫn đến việc hệ thống bị phá vỡ hoàn toàn (ví dụ: tái sử dụng nonce $k$ trong ElGamal/ECDSA, không kiểm tra padding).

> [!CAUTION]
> **Bẫy đề thi & Lỗi sai phổ biến**: Những nhầm lẫn thường gặp nhất giữa các định nghĩa dễ nhầm hoặc các lỗi tính toán modulo.

---

## 3. Mục lục tổng thể (Table of Contents)

- **[00. Mục tiêu an toàn & Mô hình tấn công](00-foundations-and-models/)**
  - `01-overview-and-prerequisites.md`: Tổng quan mật mã, mô hình Shannon, nguyên lý Kerckhoffs.
  - `02-security-goals.md`: CIA Triad, Origin Authentication vs Non-repudiation.
  - `03-threat-models.md`: Phân loại COA, KPA, CPA, CCA, CMA và Độ an toàn tính toán.
- **[01. Mật mã cổ điển & Thám mã](01-classical-cryptography/)**
  - `01-substitution-ciphers.md`: Caesar, Affine, Hill Cipher.
  - `02-polyalphabetic-ciphers.md`: Mã Vigenère & Khái niệm tuần hoàn khóa.
  - `03-transposition-ciphers.md`: Scytale, Rail Fence, Row Transposition.
  - `04-one-time-pad-and-secrecy.md`: OTP, Phép XOR & Shannon's Perfect Secrecy.
  - `05-classical-cryptanalysis.md`: Thám mã tần suất, Kasiski Test, Index of Coincidence.
- **[02. Nền tảng Toán học](02-mathematical-foundations/)**
  - `01-divisibility-and-gcd.md`: Phép chia hết, GCD, Thuật toán Euclid & Euclid mở rộng.
  - `02-modular-arithmetic-basics.md`: Đồng dư thức, Nghịch đảo modulo, Giải $ax \equiv b \pmod m$.
  - `03-primes-euler-fermat.md`: Số nguyên tố, Phi hàm Euler $\phi(n)$, Định lý Fermat/Euler, Lũy thừa nhanh.
  - `04-cyclic-groups-and-dlp.md`: Nhóm $\mathbb{Z}_p^*$, Cấp phần tử, Căn nguyên thủy, Bài toán DLP.
  - `05-chinese-remainder-theorem.md`: Định lý Thặng dư Trung Hoa (CRT) & Hệ phương trình đồng dư.
  - `06-quadratic-residues.md`: Thặng dư bình phương, Legendre, Jacobi, Khai căn mod $p$ và mod $pq$.
  - `07-finite-fields-gf2n.md`: Trường hữu hạn $\text{GF}(2^8)$, Đa thức bất khả quy, Nhân byte mod $m(x)$.
- **[03. Mật mã khóa đối xứng](03-symmetric-ciphers/)**
  - `01-symmetric-foundations.md`: Nguyên lý Shannon (Confusion/Diffusion), Feistel vs SPN.
  - `02-des-and-3des.md`: Chi tiết cấu trúc DES, S-Box, Key Schedule, 3DES, Tấn công MitM.
  - `03-aes.md`: Chi tiết AES (SubBytes, ShiftRows, MixColumns, AddRoundKey).
  - `04-block-cipher-modes.md`: Chế độ khối (ECB, CBC, CFB, OFB, CTR), Vector khởi tạo IV, Lan truyền lỗi.
  - `05-stream-ciphers.md`: Khái niệm mã dòng, PRNG, LFSR, Thuật toán RC4.
- **[04. Mật mã khóa công khai](04-asymmetric-ciphers/)**
  - `01-public-key-principles.md`: Khái niệm PKE, Trapdoor One-way Function.
  - `02-rsa-cryptosystem.md`: Hệ mật RSA (KeyGen, Enc/Dec, Chứng minh đúng đắn, Tấn công cơ bản).
  - `03-rabin-cryptosystem.md`: Hệ mật Rabin, Giải mã qua CRT, Bài toán 4 nghiệm căn bậc hai.
  - `04-elgamal-cryptosystem.md`: Hệ mật ElGamal trên $\mathbb{Z}_p^*$, Lỗ hổng tái sử dụng số ngẫu nhiên.
  - `05-merkle-hellman-knapsack.md`: Hệ mật Ba-lô Merkle-Hellman (Dãy siêu tăng & Phá vỡ).
  - `06-elliptic-curve-crypto.md`: Toán học ECC trên $\mathbb{F}_p$, Phép cộng điểm, Nhân vô hướng, ECDLP.
- **[05. Hàm băm & Mã xác thực thông điệp](05-hash-functions-and-mac/)**
  - `01-hash-functions-core.md`: Tính chất Preimage, Second Preimage, Collision Resistance, Birthday Attack.
  - `02-merkle-damgard-and-sha.md`: Cấu trúc lặp Merkle-Damgård, Cơ chế MD5, SHA-1, SHA-256.
  - `03-message-auth-codes.md`: Tính toàn vẹn và xác thực nguồn gốc, CBC-MAC, HMAC.
- **[06. Chữ ký số](06-digital-signatures/)**
  - `01-digital-signature-core.md`: Nguyên lý chữ ký số, Mô hình Hash-then-Sign, Origin Auth vs Non-repudiation.
  - `02-rsa-signatures.md`: Textbook RSA Signature, Tấn công Key-only & CMA, Lược đồ an toàn.
  - `03-elgamal-and-dsa.md`: Lược đồ ký ElGamal & Chuẩn chữ ký DSA, Hiểm họa lộ Nonce.
  - `04-ecdsa.md`: Chữ ký số trên đường cong Elliptic (ECDSA), Phân tích lỗ hổng Nonce $k$.
- **[07. Quản trị khóa & Giao thức](07-key-management-and-protocols/)**
  - `01-diffie-hellman.md`: Thỏa thuận khóa Diffie-Hellman & ECDH, Tấn công Man-in-the-Middle (MitM).
  - `02-blom-scheme.md`: Sơ đồ tiền phân phối khóa Blom (Ma trận đối xứng).
  - `03-certificates-and-pki.md`: Chứng thư số X.509, Tổ chức CA, Chuỗi tin cậy.
  - `04-kerberos-protocol.md`: Giao thức Kerberos (KDC, AS, TGS, Vé TGT, Authenticator).
- **[08. Sổ tay tính tay & Ôn thi](08-exam-prep-and-hand-calculations/)**
  - `01-hand-calc-euclid-and-inverse.md`: Bảng tính Euclid mở rộng & Nghịch đảo modulo.
  - `02-hand-calc-fast-exponentiation.md`: Bảng tính Lũy thừa nhanh (Square-and-Multiply).
  - `03-hand-calc-crt-and-roots.md`: Quy trình tính tay hệ CRT & Khai căn bậc hai mod $p$, mod $pq$.
  - `04-hand-calc-rsa-elgamal-rabin.md`: Bài tập tính tay hoàn chỉnh RSA, ElGamal, Rabin.
  - `05-hand-calc-ecc-point-addition.md`: Bảng tính tọa độ cộng điểm $(P + Q)$ và $(2P)$ trên $\mathbb{F}_p$.
  - `06-algorithm-comparison-cheat-sheet.md`: Bảng tổng hợp đối chuẩn: Độ dài khóa, Độ phức tạp, Tấn công kinh điển.

---

## 4. Bản đồ phụ thuộc tri thức (Dependency Graph)

```text
               ┌──────────────────────────────────────────────┐
               │ 00. Tổng quan & Các mô hình an toàn mật mã   │
               └──────────────────────┬───────────────────────┘
                                      │
       ┌──────────────────────────────┴──────────────────────────────┐
       ▼                                                             ▼
┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐
│ 01. Mật mã cổ điển (Caesar, Affine,      │    │ 02-01. Số học: Phép chia, GCD, Euclid MR │
│     Hill, Vigenère, Hoán vị, OTP)        │    └────────────────────┬─────────────────────┘
└──────────────────────────────────────────┘                         │
                                                                     ▼
                                                ┌──────────────────────────────────────────┐
                                                │ 02-02. Đồng dư, Nghịch đảo Modulo, ax≡b  │
                                                └─────┬───────┬──────────────────────┬─────┘
                                                      │       │                      │
       ┌──────────────────────────────────────────────┘       │                      └─────────────────────────────┐
       ▼                                                      ▼                                                    ▼
┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐
│ 02-07. Trường hữu hạn GF(2^8) & Đa thức  │    │ 02-03. Euler, Fermat, Lũy thừa nhanh     │    │ 02-05. Định lý Thặng dư Trung Hoa (CRT)  │
└──────────────────┬───────────────────────┘    └────────────────────┬─────────────────────┘    └────────────────────┬─────────────────────┘
                   │                                                 │                                               │
                   ▼                                                 ▼                                               ▼
┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐
│ 03. Mã đối xứng: DES, AES, Chế độ khối,  │    │ 04-02. Hệ mật RSA (KeyGen, Enc/Dec)      │    │ 02-06. Thặng dư bình phương & Khai căn   │
│     Mã dòng (LFSR, RC4)                  │    └────────────────────┬─────────────────────┘    └────────────────────┬─────────────────────┘
└──────────────────────────────────────────┘                         │                                               │
                                                ┌────────────────────┴─────────────────────┐                         ▼
                                                ▼                                          ▼    ┌──────────────────────────────────────────┐
                                ┌──────────────────────────────────────────┐               │    │ 04-03. Hệ mật Rabin                      │
                                │ 02-04. Nhóm Cyclic, Căn nguyên thủy, DLP │               │    └──────────────────────────────────────────┘
                                └────────────────────┬─────────────────────┘               │
                                                     │                                     ▼
                                                     ▼                          ┌──────────────────────────────────────────┐
                                ┌──────────────────────────────────────────┐    │ 04-05. Hệ mật Ba-lô Merkle-Hellman       │
                                │ 04-04. Hệ mật ElGamal                    │    └──────────────────────────────────────────┘
                                └────────────────────┬─────────────────────┘
                                                     │
                                                     ├─────────────────────────────────────┐
                                                     ▼                                     ▼
                                ┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐
                                │ 07-01. Giao thức Diffie-Hellman & ECDH   │    │ 04-06. Toán học Đường cong Elliptic ECC  │
                                └────────────────────┬─────────────────────┘    └────────────────────┬─────────────────────┘
                                                     │                                               │
                                                     ▼                                               ▼
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 05. Hàm băm mật mã (SHA) & Mã xác thực thông điệp (HMAC, CBC-MAC)                                                │
└────────────────────────────────────────────────────┬─────────────────────────────────────────────────────────────┘
                                                     │
       ┌─────────────────────────────────────────────┼─────────────────────────────────────────────┐
       ▼                                             ▼                                             ▼
┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐    ┌──────────────────────────────────────────┐
│ 06-02. Chữ ký số RSA (Hash-then-Sign)    │    │ 06-03. Chữ ký ElGamal & Chuẩn DSA        │    │ 06-04. Chữ ký số ECDSA                   │
└──────────────────────────────────────────┘    └──────────────────────────────────────────┘    └──────────────────────────────────────────┘
                                                     │
                                                     ▼
┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 07. Quản trị khóa & Giao thức: Sơ đồ Blom, PKI / Chứng thư X.509, Giao thức Kerberos                             │
└──────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
