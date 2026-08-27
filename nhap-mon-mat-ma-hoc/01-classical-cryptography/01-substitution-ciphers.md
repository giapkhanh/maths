# Hệ mật mã thay thế (Substitution Ciphers)

> **Mục tiêu học tập**:
> - Nắm vững nguyên lý thay thế (Substitution): ánh xạ các ký tự/khối ký tự sang ký tự/khối ký tự khác.
> - Hiểu và thực hiện thành thạo thuật toán mã hóa, giải mã của Mã Caesar và Mã Affine (thay thế đơn ký tự - Monographic).
> - Nắm vững cơ chế toán học và tính toán của Mã Hill (thay thế đa ký tự - Polygraphic) trên đại số tuyến tính mod 26.
> - Phân tích lý do các hệ mật thay thế đơn bảng bị phá vỡ hoàn toàn bởi phân tích tần suất.

---

## 1. Nguyên lý chung của Mật mã thay thế

Trong **Mật mã thay thế (Substitution Cipher)**, mỗi đơn vị của bản rõ (từng chữ cái hoặc từng nhóm chữ cái) được thay thế bằng một đơn vị bản mã khác theo một quy tắc xác định.

Quy ước chuẩn biểu diễn bảng chữ cái tiếng Anh (26 ký tự) sang số nguyên trong $\mathbb{Z}_{26}$:

$$\begin{array}{|c|c|c|c|c|c|c|c|c|c|c|c|c|}\hline
\text{A} & \text{B} & \text{C} & \text{D} & \text{E} & \text{F} & \text{G} & \text{H} & \text{I} & \text{J} & \text{K} & \text{L} & \text{M} \\ 
\hline
0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12 \\ 
\hline\hline
\text{N} & \text{O} & \text{P} & \text{Q} & \text{R} & \text{S} & \text{T} & \text{U} & \text{V} & \text{W} & \text{X} & \text{Y} & \text{Z} \\ 
\hline
13 & 14 & 15 & 16 & 17 & 18 & 19 & 20 & 21 & 22 & 23 & 24 & 25 \\ 
\hline
\end{array}$$

---

## 2. Mã dịch vòng (Caesar Cipher)

Mã Caesar là dạng mật mã thay thế đơn bảng đơn giản nhất, trong đó mỗi ký tự được dịch chuyển một khoảng cố định $k$ vị trí trong bảng chữ cái.

### Công thức toán học
- **Không gian khóa**: $\mathcal{K} = \{0, 1, 2, \dots, 25\}$.
- **Mã hóa (Encryption)**:
  $$C = E_k(P) \equiv (P + k) \pmod{26}$$
- **Giải mã (Decryption)**:
  $$P = D_k(C) \equiv (C - k) \pmod{26}$$

Trong đó:
- $P \in \mathbb{Z}_{26}$: Giá trị số của chữ cái bản rõ.
- $C \in \mathbb{Z}_{26}$: Giá trị số của chữ cái bản mã.
- $k \in \mathbb{Z}_{26}$: Khóa dịch chuyển (Caesar cổ điển dùng $k = 3$).

### Ví dụ tính tay (Toy Example)
Mã hóa từ `HELLO` với khóa $k = 3$:
1. Chuyển sang số: $\text{H} \to 7, \text{E} \to 4, \text{L} \to 11, \text{L} \to 11, \text{O} \to 14$.
2. Áp dụng công thức $C \equiv (P + 3) \pmod{26}$:
   - $E_3(7) = (7 + 3) \bmod 26 = 10 \to \text{K}$
   - $E_3(4) = (4 + 3) \bmod 26 = 7 \to \text{H}$
   - $E_3(11) = (11 + 3) \bmod 26 = 14 \to \text{O}$
   - $E_3(11) = (11 + 3) \bmod 26 = 14 \to \text{O}$
   - $E_3(14) = (14 + 3) \bmod 26 = 17 \to \text{R}$
3. Bản mã thu được: `KHOOR`.

---

## 3. Mã Affine (Affine Cipher)

Mã Affine mở rộng Caesar bằng cách kết hợp cả phép nhân và phép cộng trong $\mathbb{Z}_{26}$.

### Công thức toán học
- **Khóa bí mật**: $K = (a, b)$ với $a, b \in \mathbb{Z}_{26}$.
- **Điều kiện tồn tại phép giải mã**: $\gcd(a, 26) = 1$ (tức là $a$ phải khả nghịch trong $\mathbb{Z}_{26}$).
- **Mã hóa (Encryption)**:
  $$C = E_{(a, b)}(P) \equiv (a \cdot P + b) \pmod{26}$$
- **Giải mã (Decryption)**:
  $$P = D_{(a, b)}(C) \equiv a^{-1} \cdot (C - b) \pmod{26}$$

Trong đó $a^{-1}$ là phần tử nghịch đảo modulo 26 của $a$, thỏa mãn $a \cdot a^{-1} \equiv 1 \pmod{26}$.

> [!IMPORTANT]
> Vì $26 = 2 \times 13$, các giá trị của $a$ phải không chia hết cho 2 và 13.
> Tập hợp các giá trị hợp lệ của $a$ là $\mathbb{Z}_{26}^* = \{1, 3, 5, 7, 9, 11, 15, 17, 19, 21, 23, 25\}$ (tổng cộng $\phi(26) = 12$ giá trị).
> Kích thước không gian khóa của Affine: $|\mathcal{K}| = 12 \times 26 = 312$.

### Bảng tra nhanh nghịch đảo modulo 26 của $a$
$$\begin{array}{|c|c|c|c|c|c|c|c|c|c|c|c|c|}\hline
a & 1 & 3 & 5 & 7 & 9 & 11 & 15 & 17 & 19 & 21 & 23 & 25 \\ 
\hline
a^{-1} \pmod{26} & 1 & 9 & 21 & 15 & 3 & 19 & 7 & 23 & 11 & 5 & 17 & 25 \\ 
\hline
\end{array}$$

### Ví dụ tính tay (Toy Example)
Cho khóa $K = (a=7, b=3)$. Mã hóa ký tự `H` ($P = 7$) và giải mã ngược lại:
1. **Mã hóa**:
   $$C \equiv (7 \cdot 7 + 3) \bmod 26 = (49 + 3) \bmod 26 = 52 \bmod 26 = 0 \implies \text{A}$$
2. **Giải mã**: Tra bảng $a^{-1} = 7^{-1} \equiv 15 \pmod{26}$.
   $$P \equiv 15 \cdot (0 - 3) \pmod{26} \equiv 15 \cdot (-3) \pmod{26} = -45 \bmod 26 = 7 \implies \text{H}$$

---

## 4. Mã Hill (Hill Cipher — Polygraphic Substitution)

Được Lester S. Hill phát minh năm 1929, Mã Hill là hệ mật **thay thế đa ký tự** đầu tiên áp dụng đại số tuyến tính. Thay vì mã hóa từng ký tự đơn lẻ, Mã Hill chia bản rõ thành các khối $m$ ký tự và mã hóa đồng thời thông qua phép nhân ma trận.

### Định nghĩa toán học
- **Khóa bí mật**: Ma trận vuông cấp $m \times m$, ký hiệu là $K$:

$$K = \begin{pmatrix} k_{11} & k_{12} & \cdots & k_{1m} \\ 
k_{21} & k_{22} & \cdots & k_{2m} \\ 
\vdots & \vdots & \ddots & \vdots \\ 
k_{m1} & k_{m2} & \cdots & k_{mm} \end{pmatrix}, \quad k_{ij} \in \mathbb{Z}_{26}$$

- **Điều kiện khả nghịch của khóa**: Ma trận $K$ khả nghịch trong modulo 26 khi và chỉ khi:

$$\gcd(\det(K), 26) = 1$$

- **Mã hóa**: Bản rõ được chia thành các vector cột $P = (p_1, p_2, \dots, p_m)^T$:

$$C \equiv K \cdot P \pmod{26}$$

- **Giải mã**:

$$P \equiv K^{-1} \cdot C \pmod{26}$$

Công thức tìm ma trận nghịch đảo $K^{-1} \pmod{26}$ đối với ma trận cấp $2 \times 2$:

$$K = \begin{pmatrix} a & b \\ 
c & d \end{pmatrix} \implies K^{-1} \equiv (\det(K))^{-1} \cdot \begin{pmatrix} d & -b \\ 
-c & a \end{pmatrix} \pmod{26}$$

với $\det(K) = (ad - bc) \bmod 26$.

### Ví dụ tính tay từng bước (Toy Example)
Cho khóa ma trận cấp $2 \times 2$: 

$$K = \begin{pmatrix}
3 & 3 \\ 
2 & 5 
\end{pmatrix}$$

mã hóa bản rõ `HELP`.

**Bước 1: Chia khối và chuyển sang vector số**
- Khối 1: `HE`

$$\to P_1 = \begin{pmatrix} 7 \\ 
4 \end{pmatrix}$$

- Khối 2: `LP`

$$\to P_2 = \begin{pmatrix} 11 \\ 
15 \end{pmatrix}$$

**Bước 2: Mã hóa từng khối**
- Với $P_1$:

$$C_1 \equiv \begin{pmatrix} 3 & 3 \\ 
2 & 5 \end{pmatrix} \begin{pmatrix} 7 \\ 
4 \end{pmatrix} = \begin{pmatrix} 3(7) + 3(4) \\ 
2(7) + 5(4) \end{pmatrix} = \begin{pmatrix} 33 \\ 
34 \end{pmatrix} \equiv \begin{pmatrix} 7 \\ 
8 \end{pmatrix} \pmod{26} \implies \text{HI}$$

- Với $P_2$:

$$C_2 \equiv \begin{pmatrix} 3 & 3 \\ 
2 & 5 \end{pmatrix} \begin{pmatrix} 11 \\ 
15 \end{pmatrix} = \begin{pmatrix} 3(11) + 3(15) \\ 
2(11) + 5(15) \end{pmatrix} = \begin{pmatrix} 78 \\ 
97 \end{pmatrix} \equiv \begin{pmatrix} 0 \\ 
19 \end{pmatrix} \pmod{26} \implies \text{AT}$$

- Bản mã thu được: `HIAT`.

**Bước 3: Giải mã khối**

$$C_1 = \begin{pmatrix} 7 \\ 
8 \end{pmatrix}$$
 
để kiểm tra tính đúng đắn**
1. Tính định thức: $\det(K) = (3 \cdot 5 - 3 \cdot 2) = 15 - 6 = 9$.
2. Kiểm tra điều kiện: $\gcd(9, 26) = 1 \implies$ Hợp lệ.
3. Tìm nghịch đảo định thức: $9^{-1} \pmod{26} = 3$ (vì $9 \times 3 = 27 \equiv 1 \pmod{26}$).
4. Tính ma trận phụ hợp:

$$\begin{pmatrix} d & -b \\ 
-c & a \end{pmatrix} = 
\begin{pmatrix} 5 & -3 \\ 
-2 & 3 \end{pmatrix}
\equiv \begin{pmatrix} 5 & 23 \\ 
24 & 3 \end{pmatrix} \pmod{26}$$

6. Ma trận nghịch đảo:
   
$$K^{-1} \equiv 3 \cdot \begin{pmatrix} 5 & 23 \\ 
24 & 3 \end{pmatrix} =
\begin{pmatrix} 15 & 69 \\ 
72 & 9 \end{pmatrix} \equiv \begin{pmatrix} 15 & 17 \\ 
20 & 9 \end{pmatrix} \pmod{26}$$
   
8. Giải mã:
   
$$P_1 \equiv \begin{pmatrix} 15 & 17 \\ 
20 & 9 \end{pmatrix} \begin{pmatrix} 7 \\ 
8 \end{pmatrix} = \begin{pmatrix} 15(7) + 17(8) \\ 
20(7) + 9(8) \end{pmatrix} = \begin{pmatrix} 241 \\ 
212 \end{pmatrix} \equiv \begin{pmatrix} 7 \\ 
4 \end{pmatrix} \pmod{26} \implies \text{HE}$$

---

## 5. Phân tích an toàn & Điểm yếu

1. **Caesar & Affine**: Không gian khóa cực nhỏ ($|\mathcal{K}| = 26$ và $|\mathcal{K}| = 312$), hoàn toàn bị phá hủy bằng phương pháp vét cạn (Brute-force) trong tích tắc. Ngoài ra, vì là thay thế đơn bảng, chúng giữ nguyên phân phối tần suất chữ cái của ngôn ngữ gốc $\to$ Bị phá vỡ bởi **Phân tích tần suất (Frequency Analysis)**.
2. **Hill Cipher**:
   - Kháng được phân tích tần suất đơn ký tự vì nó làm phẳng biểu đồ tần suất thông qua mã hóa khối.
   - **Điểm yếu chí mạng**: Tính chất tuyến tính hoàn toàn. Hill Cipher bị phá vỡ hoàn toàn dưới mô hình **Tấn công biết trước bản rõ (Known-Plaintext Attack - KPA)** bằng cách giải hệ phương trình ma trận tuyến tính $C = K \cdot P \iff K = C \cdot P^{-1} \pmod{26}$ khi biết đủ $m$ cặp vector độc lập tuyến tính.

---

## 6. Tóm tắt cốt lõi

| Hệ mật | Phân loại | Công thức mã hóa | Điều kiện an toàn khóa | Điểm yếu chính |
| :--- | :--- | :--- | :--- | :--- |
| **Caesar** | Monographic | $C \equiv P + k \pmod{26}$ | Không có | Vét cạn ($\|\mathcal{K}\|=26$), Phân tích tần suất |
| **Affine** | Monographic | $C \equiv aP + b \pmod{26}$ | $\gcd(a, 26) = 1$ | Vét cạn ($\|\mathcal{K}\|=312$), Phân tích tần suất |
| **Hill** | Polygraphic | $C \equiv K \cdot P \pmod{26}$ | $\gcd(\det(K), 26) = 1$ | KPA (Giải hệ phương trình ma trận) |

---

## 7. Bài tập tự luyện

1. Cho mã Affine với khóa $K = (9, 2)$.
   - a) Tìm hàm giải mã tương ứng.
   - b) Mã hóa thông điệp `MATH`.
2. Cho ma trận khóa Hill

$$K = \begin{pmatrix} 
2 & 3 \\
5 & 7 \\
\end{pmatrix}$$

Ma trận này có phải là khóa hợp lệ không? Nếu có, hãy tìm $K^{-1} \pmod{26}$$.

3. Giả sử với mã Hill cấp $2 \times 2$, kẻ thù thu được cặp bản rõ - bản mã: `FR` $\to$ `PQ` và `ED` $\to$ `XZ`. Hãy khôi phục lại ma trận khóa bí mật $K$.

```

---

# FILE 06/44: `01-classical-cryptography/02-polyalphabetic-ciphers.md`

```markdown
# Hệ mật mã thay thế đa bảng (Polyalphabetic Ciphers)

> **Mục tiêu học tập**:
> - Hiểu nguyên lý làm phẳng phân phối tần suất bằng kỹ thuật thay thế đa bảng.
> - Nắm vững cơ chế mã hóa và giải mã của Mã Vigenère.
> - Hiểu cách biểu diễn Vigenère dưới dạng chuỗi các phép dịch vòng Caesar có chu kỳ.

---

## 1. Trực giác & Vấn đề mà Mã đa bảng giải quyết

Trong các hệ mật thay thế đơn bảng (Monoalphabetic), một chữ cái bản rõ $P$ luôn luôn biến thành đúng một chữ cái bản mã $C$ duy nhất trong toàn bộ văn bản (ví dụ chữ `E` luôn biến thành `X`). Điều này khiến bức tranh thống kê tần suất ngôn ngữ bị rò rỉ nguyên vẹn.

**Mật mã thay thế đa bảng (Polyalphabetic Cipher)** giải quyết điểm yếu này bằng cách sử dụng **nhiều bảng chữ cái thay thế khác nhau** luân phiên trong quá trình mã hóa. Cùng một chữ cái `E` xuất hiện ở các vị trí khác nhau có thể được mã hóa thành các ký tự khác nhau (ví dụ lần 1 thành `K`, lần 2 thành `R`, lần 3 thành `B`).

---

## 2. Hệ mật Vigenère (Vigenère Cipher)

Phát minh bởi Blaise de Vigenère vào thế kỷ 16, từng được mệnh danh là *"Le Chiffre Indéchiffrable"* (Mật mã không thể phá vỡ) suốt hơn 300 năm.

### Cơ chế hoạt động
Mã Vigenère sử dụng một từ khóa (keyword) có độ dài $m$. Khóa này được lặp lại tuần hoàn cho đến khi có cùng độ dài với bản rõ.

- **Bản rõ**: $P = (p_0, p_1, p_2, \dots, p_{n-1})$
- **Từ khóa**: $K = (k_0, k_1, \dots, k_{m-1})$
- **Dòng khóa mở rộng**: $\tilde{K} = (\tilde{k}_0, \tilde{k}_1, \dots, \tilde{k}_{n-1})$ với $\tilde{k}_i = k_{i \bmod m}$

### Công thức toán học
- **Mã hóa (Encryption)**:
  $$c_i = E_{k_i}(p_i) \equiv (p_i + \tilde{k}_i) \pmod{26}$$
- **Giải mã (Decryption)**:
  $$p_i = D_{k_i}(c_i) \equiv (c_i - \tilde{k}_i) \pmod{26}$$

Trong đó $p_i, c_i, \tilde{k}_i \in \mathbb{Z}_{26}$.

---

## 3. Bảng Vigenère (Tabula Recta)

Bảng Vigenère gồm 26 dòng của mã Caesar với các giá trị dịch chuyển tăng dần từ $0$ đến $25$:

```text
      A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
    ┌────────────────────────────────────────────────────
  A │ A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
  B │ B C D E F G H I J K L M N O P Q R S T U V W X Y Z A
  C │ C D E F G H I J K L M N O P Q R S T U V W X Y Z A B
  D │ D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
  E │ E F G H I J K L M N O P Q R S T U V W X Y Z A B C D
  . │ ...
  Z │ Z A B C D E F G H I J K L M N O P Q R S T U V W X Y
```

*Quy tắc tra bảng*: Tìm ký tự bản rõ $p_i$ ở hàng ngang trên cùng, tìm ký tự khóa $\tilde{k}_i$ ở cột dọc ngoài cùng bên trái $\to$ Giao điểm giữa hàng và cột là ký tự bản mã $c_i$.

---

## 4. Ví dụ tính tay từng bước (Toy Example)

Mã hóa thông điệp $P =$ `ATTACKATDAWN` với từ khóa $K =$ `LEMON`.

**Bước 1: Thiết lập dòng khóa tuần hoàn**
```text
Vị trí i:    0  1  2  3  4  5  6  7  8  9 10 11
Bản rõ (P):  A  T  T  A  C  K  A  T  D  A  W  N
Khóa   (K):  L  E  M  O  N  L  E  M  O  N  L  E
```

**Bước 2: Tính toán modulo 26 từng vị trí**
- $i=0: \text{A}(0) + \text{L}(11) = 11 \bmod 26 \to \text{L}$
- $i=1: \text{T}(19) + \text{E}(4) = 23 \bmod 26 \to \text{X}$
- $i=2: \text{T}(19) + \text{M}(12) = 31 \bmod 26 = 5 \to \text{F}$
- $i=3: \text{A}(0) + \text{O}(14) = 14 \bmod 26 \to \text{O}$
- $i=4: \text{C}(2) + \text{N}(13) = 15 \bmod 26 \to \text{P}$
- $i=5: \text{K}(10) + \text{L}(11) = 21 \bmod 26 \to \text{V}$
- $i=6: \text{A}(0) + \text{E}(4) = 4 \bmod 26 \to \text{E}$
- $i=7: \text{T}(19) + \text{M}(12) = 31 \bmod 26 = 5 \to \text{F}$
- $i=8: \text{D}(3) + \text{O}(14) = 17 \bmod 26 \to \text{R}$
- $i=9: \text{A}(0) + \text{N}(13) = 13 \bmod 26 \to \text{N}$
- $i=10: \text{W}(22) + \text{L}(11) = 33 \bmod 26 = 7 \to \text{H}$
- $i=11: \text{N}(13) + \text{E}(4) = 17 \bmod 26 \to \text{R}$

**Bản mã thu được**: `LXFOPVEFRNHR`.

> [!NOTE]
> Nhận xét: Chữ `A` xuất hiện 3 lần ở các vị trí $0, 3, 6$ nhưng được mã hóa lần lượt thành 3 ký tự khác nhau: `L`, `O`, `E`. Ngược lại, ký tự bản mã `F` xuất hiện 2 lần ở vị trí $2$ và $7$ đều đại diện cho chữ `T`. Điều này làm mờ tính quy luật của thống kê đơn ký tự.

---

## 5. Phân tích an toàn & Bản chất điểm yếu

Dù Vigenère làm phẳng biểu đồ tần suất ký tự đơn lẻ, nó vẫn chứa điểm yếu cốt tử: **Tính chu kỳ (Periodicity)** của từ khóa.

Nếu độ dài từ khóa là $m$:
- Tập hợp tất cả các ký tự ở vị trí $0, m, 2m, 3m, \dots$ đều được mã hóa bằng cùng một khóa con $k_0$.
- Tập hợp tất cả các ký tự ở vị trí $1, m+1, 2m+1, \dots$ đều được mã hóa bằng cùng một khóa con $k_1$.

$\implies$ Một bản mã Vigenère với khóa độ dài $m$ thực chất chỉ là **$m$ bản mã Caesar độc lập xen kẽ nhau**. Nếu thám mã tìm ra được độ dài $m$, họ chỉ cần tách văn bản thành $m$ cột và áp dụng phân tích tần suất đơn bảng trên từng cột là phá vỡ hoàn toàn hệ thống.

*(Phương pháp thám mã Kasiski và Index of Coincidence để tìm độ dài $m$ được trình bày chi tiết tại `01-classical-cryptanalysis.md`)*.

---

## 6. Tóm tắt cốt lõi

1. Mã Vigenère là hệ mật thay thế đa bảng với cơ chế $c_i \equiv (p_i + k_{i \bmod m}) \pmod{26}$.
2. Vigenère triệt tiêu được phân tích tần suất trực tiếp trên từng ký tự đơn lẻ.
3. Điểm yếu chí mạng là tính tuần hoàn: Nếu biết độ dài khóa $m$, bài toán bị quy về việc giải $m$ bài toán mã Caesar đơn giản.

---

## 7. Bài tập tự luyện

1. Cho khóa $K =$ `CIPHER`. Mã hóa thông điệp `SECRET`.
2. Cho bản mã `MSX` được mã hóa bằng Vigenère với khóa `CAT`. Hãy giải mã để tìm bản rõ ban đầu.
3. Nếu một bức thư dài 1000 ký tự được mã hóa bằng Vigenère với từ khóa độ dài 5 ký tự, kẻ tấn công có thể phân tách bức thư thành bao nhiêu tập con đơn bảng? Mỗi tập con chứa bao nhiêu ký tự?
```

---

# FILE 07/44: `01-classical-cryptography/03-transposition-ciphers.md`

```markdown
# Hệ mật mã hoán vị (Transposition Ciphers)

> **Mục tiêu học tập**:
> - Hiểu nguyên lý hoán vị (Transposition / Permutation): giữ nguyên ký tự nhưng đảo lộn vị trí.
> - Thực hiện thành thạo các thuật toán: Gậy mật mã Scytale, Mã hàng rào (Rail Fence), Mã hoán vị dòng/cột (Row Transposition).
> - Nhận diện dấu hiệu đặc trưng của bản mã hoán vị thông qua kiểm tra phân phối tần suất chữ cái.

---

## 1. Trực giác & Nguyên lý Hoán vị

Khác với mật mã thay thế (thay đổi nhận dạng chữ cái nhưng giữ nguyên vị trí), **Mật mã hoán vị (Transposition / Permutation Cipher)** **giữ nguyên toàn bộ các chữ cái gốc** nhưng sắp xếp lại vị trí thứ tự của chúng trong văn bản.

```text
  Bản rõ:     H  E  L  L  O     (Tập hợp chữ cái: {E:1, H:1, L:2, O:1})
               │  │  │  │  │    
               ▼  ▼  ▼  ▼  ▼    (Xáo trộn vị trí)
  Bản mã:     L  H  O  E  L     (Tập hợp chữ cái: {E:1, H:1, L:2, O:1} KHÔNG ĐỔI)
```

> [!IMPORTANT]
> **Dấu hiệu nhận biết bản mã Hoán vị**:
> Tần suất xuất hiện của các chữ cái trong bản mã hoán vị giống hệt 100% tần suất xuất hiện trong bản rõ gốc (chữ `E`, `T`, `A` vẫn chiếm tỷ lệ cao nhất).

---

## 2. Gậy Mật mã Hy Lạp (Scytale)

Thiết bị mật mã quân sự cổ xưa nhất của người Sparta (khoảng 400 TCN). Một dải da được quấn quanh một trục gỗ hình trụ có đường kính xác định, thông điệp được viết dọc theo thân gậy.

- Khi tháo dải da ra: Thông điệp trở thành một chuỗi chữ cái rời rạc vô nghĩa.
- Khi người nhận quấn dải da vào một thanh gỗ có **cùng đường kính (khóa $k$)**: Thông điệp ban đầu hiện ra.
- *Bản chất toán học*: Bản rõ được ghi theo từng cột và đọc ra theo từng hàng (hoặc ngược lại) trên ma trận có $k$ dòng.

---

## 3. Mã hàng rào (Rail Fence Cipher)

Bản rõ được viết theo đường zíc-zắc (hình răng cưa) trên $k$ đường ray giả định, sau đó đọc tuần tự theo từng hàng ngang để tạo bản mã.

### Ví dụ tính tay (Toy Example)
Mã hóa thông điệp `WE ARE DISCOVERED FLEE AT ONCE` với số đường ray $k = 3$.

**Bước 1: Viết bản rõ theo hình zíc-zắc trên 3 hàng**
```text
Hàng 1: W . . . E . . . C . . . R . . . L . . . T . . . E
Hàng 2: . E . R . D . S . O . E . E . F . E . A . O . C .
Hàng 3: . . A . . . I . . . V . . . D . . . E . . . N . .
```

**Bước 2: Đọc tuần tự từng hàng từ trên xuống dưới**
- Hàng 1: `W E C R L T E`
- Hàng 2: `E R D S O E E F E A O C`
- Hàng 3: `A I V D E N`

**Bản mã thu được**: `WECRLTEERDSOEEFEAOCEAIVDEN`.

---

## 4. Mã hoán vị dòng/cột (Row/Column Transposition Cipher)

Bản rõ được ghi vào một ma trận theo từng dòng, sau đó các cột được tráo đổi vị trí theo thứ tự của một khóa hoán vị $\pi$, cuối cùng đọc bản mã ra theo từng cột.

### Thuật toán chi tiết
1. Chọn từ khóa có độ dài $m$. Xác định thứ tự ưu tiên của các cột bằng cách đánh số thứ tự bảng chữ cái của từng ký tự trong từ khóa.
2. Ghi bản rõ vào ma trận kích thước $r \times m$ theo thứ tự từ trái sang phải, từ trên xuống dưới. Nếu thiếu ký tự ở hàng cuối, điền thêm ký tự đệm (Padding - ví dụ `X`).
3. Đọc dữ liệu ra theo từng cột theo thứ tự số tăng dần của khóa.

### Ví dụ tính tay (Toy Example)
Mã hóa thông điệp `ATTACK POSTPONED UNTIL TWO AM` với từ khóa $K =$ `GERMAN`.

**Bước 1: Xác định thứ tự cột từ khóa `GERMAN`**
- Xếp theo thứ tự từ điển: `A(1), E(2), G(3), M(4), N(5), R(6)`.
- Thứ tự đọc cột:
```text
Khóa:      G  E  R  M  A  N
Thứ tự:    3  2  6  4  1  5
```

**Bước 2: Điền bản rõ vào ma trận**
```text
Thứ tự:    3  2  6  4  1  5
           ────────────────
Dòng 1:    A  T  T  A  C  K
Dòng 2:    P  O  S  T  P  O
Dòng 3:    N  E  D  U  N  T
Dòng 4:    I  L  T  W  O  A
Dòng 5:    M  X  X  X  X  X  (Điền thêm đệm 'X')
```

**Bước 3: Đọc ra theo thứ tự cột từ 1 đến 6**
- Cột 1 (A): `C P N O X`
- Cột 2 (E): `T O E L X`
- Cột 3 (G): `A P N I M`
- Cột 4 (M): `A T U W X`
- Cột 5 (N): `K O T A X`
- Cột 6 (R): `T S D T X`

**Bản mã thu được**: `CPNOX TOELX APNIM ATUWX KOTAX TSDTX`.

---

## 5. Phân tích an toàn & Thám mã

1. **Kháng phân tích tần suất đơn ký tự**: Mật mã hoán vị không làm thay đổi tần suất của từng ký tự. Do đó, thám mã nhận diện ngay đây là mã hoán vị khi thấy tỷ lệ xuất hiện của các chữ cái trùng khớp với tiếng Anh thông thường.
2. **Kỹ thuật thám mã**:
   - Sử dụng **Phân tích tần suất cặp ký tự (Digram/Trigram Analysis)**: Trong tiếng Anh, các cặp ký tự như `TH`, `HE`, `IN`, `ER` có tần suất xuất hiện rất cao, trong khi các cặp như `QZ`, `JX` gần như bằng 0.
   - Thám mã lập ma trận kích thước khả dĩ, thử tráo đổi các cột và tính điểm thống kê dựa trên tần suất xuất hiện của các cặp ký tự liền kề (*Anagramming method*).

---

## 6. Tóm tắt cốt lõi

1. Mật mã hoán vị thay đổi vị trí của các ký tự, giữ nguyên giá trị chữ cái gốc.
2. Bản mã luôn có phân phối tần suất ký tự đơn trùng khớp với bản rõ.
3. Phương pháp thám mã chính là sắp xếp lại các cột dựa trên tần suất của các cặp chữ cái đi liền nhau (Digrams).

---

## 7. Bài tập tự luyện

1. Giải mã bản mã `CTGEEKNITE` biết nó được tạo bởi Rail Fence với số hàng $k = 2$.
2. Cho từ khóa `MATH` (thứ tự cột: `3 1 4 2`), hãy mã hóa bản tin `GOOD MORNING`.
3. Làm thế nào để phân biệt một chuỗi bản mã được tạo ra bởi Mã Caesar hay Mã Hoán vị cột chỉ bằng mắt thường thông qua bảng thống kê tần suất?
```

---

# FILE 08/44: `01-classical-cryptography/04-one-time-pad-and-secrecy.md`

```markdown
# Hệ mật One-Time Pad & Độ an toàn tuyệt đối của Shannon

> **Mục tiêu học tập**:
> - Hiểu cơ chế hoạt động của Hệ mật One-Time Pad (Mã Vernam).
> - Nắm vững định nghĩa toán học về Độ an toàn tuyệt đối (Perfect Secrecy) của Claude Shannon.
> - Chứng minh tại sao OTP đạt độ an toàn tuyệt đối về mặt lý thuyết thông tin.
> - Hiểu lý do tại sao việc tái sử dụng khóa trong OTP dẫn đến sự sụp đổ hoàn toàn của hệ thống (Two-time Pad Attack).

---

## 1. Hệ mật One-Time Pad (Mã Vernam)

Được Gilbert Vernam phát minh năm 1917 và Joseph Mauborgne hoàn thiện, **One-Time Pad (OTP)** là hệ mật mã đầu tiên và duy nhất được chứng minh là **hoàn toàn không thể bị phá vỡ về mặt toán học** nếu tuân thủ đúng các nguyên tắc.

### Cơ chế hoạt động
Bản rõ $M$, Bản mã $C$ và Khóa bí mật $K$ đều là các chuỗi bit nhị phân có **cùng độ dài $n$**:

$$M, C, K \in \{0, 1\}^n$$

- **Mã hóa (Encryption)**: Dùng phép toán cộng XOR từng bit ($\oplus$):
  $$C = M \oplus K$$
- **Giải mã (Decryption)**: Áp dụng lại phép XOR với cùng khóa $K$:
  $$M = C \oplus K$$

### Tính đúng đắn toán học
Dựa trên 2 tính chất cơ bản của phép XOR: $A \oplus A = 0$ và $A \oplus 0 = A$:

$$C \oplus K = (M \oplus K) \oplus K = M \oplus (K \oplus K) = M \oplus 0 = M$$

### Ví dụ tính tay (Toy Example)
- Bản rõ: $M = 10011010_2$
- Khóa bí mật: $K = 01011100_2$
- **Mã hóa**:
  $$C = 10011010_2 \oplus 01011100_2 = 11000110_2$$
- **Giải mã**:
  $$M = 11000110_2 \oplus 01011100_2 = 10011010_2$$

---

## 2. Lý thuyết Độ an toàn tuyệt đối của Shannon (Perfect Secrecy)

Năm 1949, Claude Shannon đưa ra định nghĩa toán học chính xác cho một hệ mật an toàn tuyệt đối dựa trên Lý thuyết xác suất.

### Định nghĩa toán học
Một hệ mật $(\mathcal{M}, \mathcal{C}, \mathcal{K}, \mathcal{E}, \mathcal{D})$ đạt **Độ an toàn tuyệt đối (Perfect Secrecy)** nếu xác suất tiên nghiệm của bản rõ $M$ hoàn toàn bằng xác suất hậu nghiệm của nó sau khi kẻ tấn công đã quan sát được bản mã $C$:

$$P(M = m \mid C = c) = P(M = m), \quad \forall m \in \mathcal{M}, \forall c \in \mathcal{C}$$

*Ý nghĩa*: Việc biết được bản mã $C$ không cung cấp thêm bất kỳ một chút thông tin nào cho kẻ thù về bản rõ $M$. Mọi bản rõ $m$ có cùng độ dài đều có xác suất xảy ra như nhau.

### Định lý Shannon về Độ dài Khóa
Shannon đã chứng minh định lý nền tảng:
> **Định lý Shannon**: Để một hệ mật đạt được Perfect Secrecy, không gian khóa bắt buộc phải có kích thước tối thiểu bằng không gian bản rõ:
> $$|\mathcal{K}| \ge |\mathcal{M}|$$
> Trong mô hình chuỗi bit, điều này đồng nghĩa với việc: **Độ dài khóa bí mật phải lớn hơn hoặc bằng độ dài bản rõ ($|K| \ge |M|$)**.

---

## 3. Ba điều kiện bắt buộc của One-Time Pad

Để đạt được độ an toàn tuyệt đối trong thực tế, OTP bắt buộc phải thỏa mãn đồng thời 3 điều kiện:

1. **Khóa phải được sinh hoàn toàn ngẫu nhiên thực sự (Truly Random)**: Khóa không được sinh bởi các thuật toán giả ngẫu nhiên (PRNG), phân phối bit phải đồng đều $P(K=k) = \frac{1}{2^n}$.
2. **Độ dài khóa phải tối thiểu bằng độ dài thông điệp ($|K| \ge |M|$)**.
3. **Mỗi khóa chỉ được sử dụng duy nhất một lần và bị hủy ngay lập tức (Never Reuse Keys)**.

---

## 4. Hiểm họa Tái sử dụng khóa (Two-Time Pad Attack)

Nếu cùng một khóa $K$ được dùng để mã hóa hai thông điệp khác nhau $M_1$ và $M_2$:

$$C_1 = M_1 \oplus K$$
$$C_2 = M_2 \oplus K$$

Kẻ tấn công nghe lén $C_1$ và $C_2$ chỉ cần thực hiện phép XOR hai bản mã với nhau:

$$C_1 \oplus C_2 = (M_1 \oplus K) \oplus (M_2 \oplus K) = M_1 \oplus M_2$$

```text
┌─────────────────────────────────────────────────────────────────────────┐
│                    HẬU QUẢ CỦA TWO-TIME PAD ATTACK                      │
│                                                                         │
│ 1. Khóa bí mật K bị triệt tiêu hoàn toàn.                               │
│ 2. Thu được chuỗi M1 ⊕ M2.                                              │
│ 3. Dựa vào tính dư thừa ngôn ngữ (khoảng trắng XOR chữ cái tiếng Anh),   │
│    thám mã có thể khôi phục lại cả M1 và M2 bằng kỹ thuật "Crib Dragging"│
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Tại sao OTP không được sử dụng rộng rãi trên thực tế?

Dù an toàn tuyệt đối về toán học, OTP có những rào cản ứng dụng không thể vượt qua trong hạ tầng mạng thực tế:
1. **Bài toán phân phối khóa (Key Distribution Problem)**: Để truyền một bức thư bí mật 1 GB qua OTP, hai bên phải gặp nhau trước để trao đổi an toàn một ổ cứng chứa đúng 1 GB khóa ngẫu nhiên. Nếu đã có kênh an toàn để chuyển 1 GB khóa thì họ có thể dùng chính kênh đó để chuyển 1 GB dữ liệu ban đầu.
2. **Lưu trữ và đồng bộ khóa**: Tốn kém tài nguyên lưu trữ và rủi ro mất đồng bộ dòng khóa giữa hai đầu truyền nhận.

---

## 6. Tóm tắt cốt lõi

1. OTP sử dụng phép toán $C = M \oplus K$ với khóa ngẫu nhiên có độ dài bằng bản rõ.
2. Đạt độ an toàn tuyệt đối theo Shannon: $P(M=m \mid C=c) = P(M=m)$.
3. Tái sử dụng khóa sẽ làm sụp đổ hệ thống vì $C_1 \oplus C_2 = M_1 \oplus M_2$.
4. OTP bị hạn chế ứng dụng thực tế bởi bài toán phân phối và lưu trữ khóa khổng lồ.

---

## 7. Bài tập tự luyện

1. Cho bản rõ $M = 11010011_2$ và khóa $K = 10110101_2$. Tính bản mã $C$.
2. Giả sử $C_1 \oplus C_2 = 00011011_2$. Biết bản rõ $M_1$ bắt đầu bằng ký tự mã ASCII là `A` ($01000001_2$). Hãy tìm ký tự bắt đầu của bản rõ $M_2$.
3. Chứng minh rằng nếu không gian khóa $|\mathcal{K}| < |\mathcal{M}|$, hệ mật không thể đạt được độ an toàn tuyệt đối (Perfect Secrecy).
```

---

# FILE 09/44: `01-classical-cryptography/05-classical-cryptanalysis.md`

```markdown
# Thám mã cổ điển: Tần suất, Kasiski & Chỉ số trùng khớp

> **Mục tiêu học tập**:
> - Nắm vững phương pháp Phân tích tần suất chữ cái đơn (Frequency Analysis) để phá mã thay thế đơn bảng.
> - Hiểu và thực hiện thành thạo Phương pháp kiểm tra Kasiski (Kasiski Examination) để tìm độ dài từ khóa của mã Vigenère.
> - Hiểu định nghĩa toán học và công thức tính Chỉ số trùng khớp (Index of Coincidence - $I_c$) để xác định độ dài khóa một cách tự động.

---

## 1. Phân tích tần suất đơn ký tự (Frequency Analysis)

Trong bất kỳ ngôn ngữ tự nhiên nào, các chữ cái không xuất hiện với xác suất bằng nhau. Trong tiếng Anh chuẩn, phân phối xác suất của 26 chữ cái có các đặc trưng rõ rệt:

```text
Tần suất (%)
 13 │     E
 12 │     █
  9 │     █       T
  8 │   A █   O   █
  7 │   █ █ I █ N █ S R H
  6 │   █ █ █ █ █ █ █ █ █ D L
  1 │ ...                       Q J Z
    └─────────────────────────────────► Ký tự
```

- **Nhóm tần suất cao nhất**: `E` (~12.7%), `T` (~9.1%), `A` (~8.2%), `O` (~7.5%), `I` (~7.0%), `N` (~6.7%).
- **Nhóm tần suất thấp nhất**: `Z`, `Q`, `X`, `J` (< 0.2%).
- **Nhóm cặp ký tự phổ biến (Digrams)**: `TH`, `HE`, `IN`, `ER`, `AN`, `RE`.

### Quy trình phá mã thay thế đơn bảng (Caesar, Affine, Hoán vị chữ cái)
1. Đếm tần suất xuất hiện của tất cả các ký tự trong bản mã.
2. Vẽ biểu đồ phân phối tần suất của bản mã.
3. So khớp hình dáng phân phối của bản mã với biểu đồ tiếng Anh chuẩn $\to$ Ký tự xuất hiện nhiều nhất trong bản mã thường tương ứng với chữ `E` hoặc `T`.
4. Thiết lập hệ phương trình để tìm khóa dịch chuyển $k$ hoặc cặp khóa Affine $(a, b)$.

---

## 2. Phương pháp kiểm tra Kasiski (Kasiski Examination)

Được Friedrich Kasiski công bố năm 1863, đây là kỹ thuật đầu tiên phá vỡ hoàn toàn tính bí mật của mã Vigenère bằng cách tìm ra **độ dài từ khóa $m$**.

### Nguyên lý cốt lõi
Nếu một chuỗi ký tự bản rõ (thường là các từ phổ biến như `THE`, `AND`, `ING`) vô tình được mã hóa bằng **cùng một đoạn của từ khóa**, chuỗi ký tự đó sẽ sinh ra **các cụm bản mã giống hệt nhau**.

Khoảng cách $d$ giữa các cụm bản mã trùng lặp này nhiều khả năng là một **bội số của độ dài từ khóa $m$**:

$$d \equiv 0 \pmod m \iff m \mid d$$

### Quy trình thực hiện
1. Quét bản mã để tìm tất cả các chuỗi ký tự trùng lặp có độ dài từ 3 ký tự trở lên (ví dụ: các đoạn 3-gram hoặc 4-gram lặp lại).
2. Ghi lại khoảng cách vị trí $d_1, d_2, \dots, d_k$ giữa các lần xuất hiện lặp lại.
3. Tính ước số chung lớn nhất (GCD) hoặc tìm các thừa số nguyên tố chung của tập hợp khoảng cách $\{d_1, d_2, \dots, d_k\}$.
4. Giá trị thừa số chung xuất hiện nhiều nhất chính là độ dài khóa $m$ khả dĩ nhất.

```text
Bản rõ:     ... T H E ... ... ... T H E ...
Dòng khóa:  ... K E Y ... ... ... K E Y ... (Vô tình trùng pha của từ khóa)
Bản mã:     ... D L C ... ... ... D L C ...
                ◄────── Khoảng cách d ──────► (d bắt buộc chia hết cho độ dài khóa m)
```

---

## 3. Chỉ số trùng khớp (Index of Coincidence — $I_c$)

Năm 1922, William F. Friedman đưa ra khái niệm **Chỉ số trùng khớp (Index of Coincidence)** nhằm cung cấp một công cụ toán học chính xác để đo độ "phẳng" của văn bản mà không cần tìm các cụm từ lặp lại.

### Định nghĩa toán học
Chỉ số trùng khớp $I_c$ của một văn bản là **xác suất để hai chữ cái được chọn ngẫu nhiên từ văn bản đó là giống hệt nhau**.

Cho văn bản có tổng số $N$ ký tự, trong đó chữ cái thứ $i$ trong bảng chữ cái ($i \in \{\text{A} \dots \text{Z}\}$) xuất hiện $f_i$ lần:

$$I_c = \frac{\sum_{i=0}^{25} f_i (f_i - 1)}{N (N - 1)}$$

### Các giá trị chuẩn của $I_c$
- **Văn bản tiếng Anh tự nhiên (chưa mã hóa hoặc mã thay thế đơn bảng)**:
  $$I_{\text{English}} \approx \sum_{i=0}^{25} p_i^2 \approx 0.065 \text{ đến } 0.068$$
- **Văn bản hoàn toàn ngẫu nhiên (hoặc mã hóa đa bảng với khóa rất dài)**:
  $$I_{\text{Random}} \approx \frac{1}{26} \approx 0.0385$$

### Ứng dụng ước lượng độ dài khóa Vigenère (Công thức Friedman)
Dựa trên giá trị $I_c$ tính được từ bản mã, độ dài từ khóa $m$ được ước lượng bằng công thức:

$$m \approx \frac{0.027 \cdot N}{(N - 1) \cdot I_c - 0.038 \cdot N + 0.065}$$

*Ý nghĩa sư phạm*: 
- Nếu $I_c \approx 0.065 \implies$ Văn bản được mã hóa bằng hệ đơn bảng ($m = 1$).
- Nếu $I_c \approx 0.038 \implies$ Bản mã rất ngẫu nhiên, độ dài từ khóa $m$ rất lớn.

---

## 4. Tóm tắt cốt lõi

```text
┌─────────────────────────────────────────────────────────────────────────┐
│              CHIẾN LƯỢC PHÁ MÃ CỔ ĐIỂN TỔNG QUÁT                        │
│                                                                         │
│ 1. Tính Ic của bản mã:                                                  │
│    - Nếu Ic ≈ 0.065: Mã đơn bảng (Caesar, Affine) hoặc Mã Hoán vị.      │
│    - Nếu Ic < 0.050: Mã đa bảng (Vigenère).                             │
│ 2. Nếu là Vigenère:                                                     │
│    - Dùng Kasiski Test + Công thức Friedman để tìm độ dài khóa m.       │
│    - Tách bản mã thành m cột độc lập.                                   │
│    - Áp dụng phân tích tần suất trên từng cột để tìm m chữ cái của khóa.│
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Câu hỏi & Bài tập ôn tập

1. Tại sao mã hóa Vigenère với từ khóa càng dài thì chỉ số trùng khớp $I_c$ của bản mã càng tiến gần đến giá trị $0.0385$?
2. Trong một bản mã Vigenère, ta tìm thấy chuỗi `VHO` xuất hiện tại các vị trí $32, 104, 176$. Hãy dự đoán các độ dài khả dĩ nhất của từ khóa.
3. Cho một đoạn văn bản ngắn gồm 10 ký tự với bảng tần suất: $\text{A}: 4, \text{B}: 3, \text{C}: 2, \text{D}: 1$. Hãy tính chỉ số trùng khớp $I_c$ của đoạn văn bản này.
```

---

### Báo cáo tiến độ hoàn thành:
- **Đã hoàn thành**: 9/44 files (`README.md`, Module 00 [3 files], Module 01 [5 files]).
- **Phạm vi hoàn tất**: Toàn bộ **Module 01: Mật mã cổ điển & Thám mã**.
- **Bước tiếp theo**: Module 02: Nền tảng Toán học (File 10/44 đến 16/44).

Bạn chỉ cần bấm **tiếp tục** để tôi biên soạn trọn vẹn Module 02!
