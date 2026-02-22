**Đề bài:** Trong không gian $Oxyz$, cho mặt phẳng $(P): 2x - y + z + 1 = 0$ và hai điểm $A(2; -1; 0)$, $B(3\sqrt{3}; 0; -1)$. Điểm $M(x; y; z)$ di động trên $(P)$, tìm giá trị nhỏ nhất của biểu thức $T = -MA + 7MB$. Tính giá trị $Q = 3a + 9b + 2025$ với $T_{min} = a\sqrt{b}$.

---

#### **Bước 1: Thiết lập hệ phương trình Lagrange**
Xét hàm Lagrange cho bài toán tối ưu có ràng buộc:
$$L = - \sqrt{(x-2)^2 + (y+1)^2 + z^2} + 7 \sqrt{(x-3\sqrt{3})^2 + y^2 + (z+1)^2} + \lambda(2x - y + z + 1)$$
Hệ phương trình đạo hàm riêng (điểm dừng):

$$
\left\{
\begin{aligned}
& -\frac{x - 2}{MA} + 7\frac{x - 3\sqrt{3}}{MB} + 2\lambda = 0 \\
& -\frac{y + 1}{MA} + 7\frac{y}{MB} - \lambda = 0 \\
& -\frac{z}{MA} + 7\frac{z + 1}{MB} + \lambda = 0 \\
& 2x - y + z + 1 = 0
\end{aligned}
\right.
$$

Nhìn vào hệ đạo hàm Lagrange, đổi 3 phương trình đầu về dạng Vector sẽ là:

$$\nabla f = -\frac{\vec{PA}}{PA} + 7\frac{\vec{PB}}{PB} + \lambda \vec{n} = \vec{0}$$

Suy ra P, A, B thuộc cùng 1 mặt phẳng.
#### **Bước 2: Triệt tiêu tham số $\lambda$ để tìm quan hệ tuyến tính**
Cộng phương trình (2) và (3) theo vế để khử $\lambda$:
$$\left( -\frac{y+1}{MA} + 7\frac{y}{MB} \right) + \left( -\frac{z}{MA} + 7\frac{z+1}{MB} \right) = 0$$
$$\iff -\frac{y+z+1}{MA} + \frac{7(y+z+1)}{MB} = 0 \iff (y+z+1) \left( \frac{7}{MB} - \frac{1}{MA} \right) = 0$$

**Trường hợp 1**: **$\frac{7}{MB} - \frac{1}{MA} = 0$**.

$\rightarrow MB = 7MA$

$\rightarrow$ $\frac{\vec{MA}}{MA} - 7\frac{\vec{MB}}{7MA} = \lambda \vec{n}$

$ \rightarrow \frac{\vec{MA} - \vec{MB}}{MA} = \lambda \vec{n}$

Vì $\vec{MA} - \vec{MB} = \vec{BA}$, phương trình trở thành: $\frac{\vec{BA}}{MA} = \lambda \vec{n}$

$\implies$Điều này có nghĩa là Vector **$\vec{BA}$** phải **cùng phương** với vector pháp tuyến $\vec{n}$ của mặt phẳng (tức là đường thẳng $AB$ phải vuông góc với mặt phẳng).

**Kiểm tra thực tế:** 
*   Vector $\vec{BA} = (2 - 3\sqrt{3}; -1; 1)$
*   Vector $\vec{n} = (2; -1; 1)$
Hai vector này **không cùng phương**. Ta loại trường hợp 1. 



**Trường hợp 2**: **$y + z + 1 = 0 \implies z = -1 - y$**.

Kết hợp với phương trình mặt phẳng $(P)$ mà đề đã cho:
$$
\begin{cases} 2x - y + z = -1 \\ y + z = -1 \end{cases} \implies \begin{cases} z = t \\ y = -t - 1 \\ 2x - (-t - 1) + t = -1 \end{cases} \implies \begin{cases} x = -t - 1 \\ y = -t - 1 \\ z = t \end{cases}
$$

$\implies M(t; t; -1-t)$

#### **Bước 3: Tối ưu hàm một biến bằng Casio**
Thay tọa độ $M$ vào biểu thức khoảng cách:
*   $MA = \sqrt{(t-2)^2 + (t+1)^2 + (-1-t)^2} = \sqrt{3t^2 + 6}$
*   $MB = \sqrt{(t-3\sqrt{3})^2 + t^2 + (-1-t+1)^2} = \sqrt{3t^2 - 6\sqrt{3}t + 27}$

Hàm mục tiêu: $T(t) = 7\sqrt{3t^2 - 6\sqrt{3}t + 27} - \sqrt{3t^2 + 6}$
Sử dụng tính năng **`SHIFT SOLVE`** cho phương trình $T'(t) = 0$:
Máy tính nhả nghiệm: $t = \frac{7\sqrt{3}}{6}$.

#### **Bước 4: Tính giá trị cực tiểu và kết luận**
Tại $t = \frac{7\sqrt{3}}{6}$, ta tính được:
*   $MA^2 = 3 \cdot \left(\frac{49}{12}\right) + 6 = \frac{73}{4} \implies MA = \frac{\sqrt{73}}{2}$
*   $MB^2 = 3 \cdot \left(\frac{49}{12}\right) - 6\sqrt{3} \cdot \left(\frac{7\sqrt{3}}{6}\right) + 27 = \frac{73}{4} \implies MB = \frac{\sqrt{73}}{2}$

Giá trị nhỏ nhất:
$$T_{min} = 7 \cdot \frac{\sqrt{73}}{2} - 1 \cdot \frac{\sqrt{73}}{2} = 6 \cdot \frac{\sqrt{73}}{2} = \mathbf{3\sqrt{73}}$$
$\implies a = 3, b = 73$.

Giá trị biểu thức $Q$:
$$Q = 3(3) + 9(73) + 2025 = 9 + 657 + 2025 = \mathbf{2691}$$

**Đáp số: 2691.**

---
