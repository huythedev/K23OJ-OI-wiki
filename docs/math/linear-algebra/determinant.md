Định thức là phép toán trên ma trận vuông. Với ma trận $A$, $\det A$ là định thức.

Bài này giới thiệu ba cách định nghĩa tương đương.

## Định nghĩa bằng hoán vị

Kiến thức trước: [hoán vị](../permutation.md), [nghịch thế](../permutation.md#逆序数).

Cách này tính tay cho bậc nhỏ, độ phức tạp giai thừa.

Ký hiệu $\pi(j_1j_2\cdots j_n)$ là số nghịch thế, $S_n$ là tập hoán vị độ dài $n$. Khi đó:

$$
\begin{aligned}
\det A &= \begin{vmatrix}
a_{11} & a_{12} & \cdots & a_{1n}\\
a_{21} & a_{22} & \cdots & a_{2n}\\
\vdots & \vdots &  & \vdots\\
a_{n1} & a_{n2} & \cdots & a_{nn}\\
\end{vmatrix} \\
&= \sum_{(j_1j_2\cdots j_n) \in S_n} (-1)^{\pi(j_1j_2\cdots j_n)} a_{1 j_1} a_{2 j_2}\dots a_{n j_n}
\end{aligned}
$$

Nghĩa là tổng đại số của $n!$ tích, mỗi tích lấy một phần tử ở mỗi hàng và mỗi cột. Dấu của tích phụ thuộc vào tính chẵn/lẻ của hoán vị.

Quy tắc đường chéo cho cấp 2 và 3 chính là cách này. Bậc ≥4 không dùng được. Định thức bậc 1 là chính phần tử đó.

Định lý: nếu lấy các phần tử $a_{i_1j_1}\cdots a_{i_nj_n}$ từ các hàng/cột theo hoán vị, thì dấu là ${(-1)}^{s+t}$ với $s=\pi(i_1\cdots i_n)$, $t=\pi(j_1\cdots j_n)$.

Định lý: $\det A = \det A^T$.

Định lý: nếu một hàng (hoặc cột) là tổng hai hàng (cột), thì định thức là tổng hai định thức tương ứng.

## Định nghĩa quy nạp

Chỉ mô tả tính chất đại số, không dùng để tính.

### Phần bù đại số

Trong định thức bậc $n$, lấy ma trận con bậc $k$ và định thức của nó gọi là **tiểu thức**. Với phần tử $a_{ij}$, ma trận con bỏ hàng $i$ cột $j$ gọi là phần bù, định thức của nó là **phần bù**.

**Phần bù đại số** của $a_{ij}$ là $A_{ij}=(-1)^{i+j}\det M_{ij}$.

Theo định nghĩa hoán vị: nếu hàng $i$ (hoặc cột $j$) ngoài $a_{ij}$ đều 0, thì $\det A=a_{ij}A_{ij}$.

### Khai triển định thức

Định thức là tổng các phần tử trên một hàng (hoặc cột) nhân với phần bù đại số tương ứng:

$$
\begin{aligned}
\det A &= \sum_{j = 1}^{n} a_{ij}A_{ij} \\
&= \sum_{j = 1}^{n} (-1)^{i + j} a_{ij} \det M_{ij}
\end{aligned}
$$

Tương tự theo cột:

$$
\det A=\sum_{i = 1}^{n} (-1)^{i + j} a_{ij} \det M_{ij}
$$

Cơ sở quy nạp là bậc 1.

Hệ quả: hàng $i$ nhân phần bù của hàng $j$ (với $i\ne j$) có tổng bằng 0; tương tự theo cột.

## Định nghĩa tiên đề hóa

Dựa trên các tính chất, phép toán thỏa bốn tính chất là định thức.

Kiến thức trước: [biến đổi sơ cấp](./elementary-operations.md).

Ký hiệu $D_i(k)$, $P_{ij}$, $T_{ij}(k)$ như trong phần trước.

Với ma trận $A$, phép toán $\det$ là định thức nếu:

-   Nhân một hàng/cột với $k$ làm định thức nhân $k$:

    $$
    \det(D_i(k)A) = \det(AD_i(k)) = k \det A
    $$

-   Đổi hai hàng/cột đổi dấu:

    $$
    \det(P_{ij}A) = \det(AP_{ij}) = -\det A
    $$

-   Cộng bội một hàng/cột vào hàng/cột khác không đổi định thức:

    $$
    \det(T_{ij}(k)A) = \det(AT_{ij}(k))= \det A
    $$

-   $\det I=1$.

Dùng tính chất này để tính định thức bằng biến đổi sơ cấp; [khử Gauss](../numerical/gauss.md#行列式计算) cũng dựa vào đây, độ phức tạp $O(n^3)$.

Một số hệ quả:

-   Có thể đưa ước chung của một hàng/cột ra ngoài.
-   Nếu một hàng/cột toàn 0 thì định thức bằng 0.
-   Nếu hai hàng/cột tỉ lệ thì định thức bằng 0.
-   Nếu hai hàng/cột giống nhau thì định thức bằng 0.
