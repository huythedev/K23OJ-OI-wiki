## Số Entringer

Số Entringer (Entringer number, [OEIS A008281](http://oeis.org/A008281)) $E(n,k)$ là số hoán vị của các số từ $0$ đến $n$ (tổng cộng $n+1$ số) thỏa:

-   Phần tử đầu là $k$;
-   Phần tử kế tiếp nhỏ hơn phần tử đầu, phần tử sau đó lớn hơn phần tử trước, rồi lại nhỏ hơn, ... quan hệ kề nhau luân phiên như vậy.

Giá trị khởi tạo:

$$
E(0,0)=1
$$

$$
E(n,0)=0
$$

Công thức truy hồi:

$$
E(n,k)=E(n,k-1)+E(n-1,n-k)
$$

## Tam giác Seidel–Entringer–Arnold

Một cách sắp xếp đặc biệt của số Entringer tạo thành tam giác Seidel–Entringer–Arnold (Seidel–Entringer–Arnold triangle, [OEIS A008280](http://oeis.org/A008280)). Tam giác này sắp theo “đường cày bò” (ox-plowing order):

$$
\begin{aligned}
& E(0,0) \\
& E(1,0) \rightarrow E(1,1) \\
& E(2,2) \leftarrow E(2,1) \leftarrow E(2,0) \\
& E(3,0) \rightarrow E(3,1) \rightarrow E(3,2) \rightarrow E(3,3) \\
& E(4,4) \leftarrow E(4,3) \leftarrow E(4,2) \leftarrow E(4,1) \leftarrow E(4,0)
\end{aligned}
$$

Tức là:

$$
\begin{aligned}
& 1 \\
& 0 \rightarrow 1 \\
& 1 \leftarrow 1 \leftarrow 0 \\
& 0 \rightarrow 1 \rightarrow 2 \rightarrow 2 \\
& 5 \leftarrow 5 \leftarrow 4 \leftarrow 2 \leftarrow 0
\end{aligned}
$$

Cách sắp xếp này phù hợp với truy hồi $E(n,k)=E(n,k-1)+E(n-1,n-k)$, dễ ghi nhớ và hiểu.

Số Entringer có một hàm sinh mũ:

$$
\sum_{m=0}^\infty\sum_{n=0}^\infty E\left(m+n,\frac{1}{2}\left(m+n+{(-1)}^{m+n}(n-m)\right)\right)\frac{x^m}{m!}\frac{x^n}{n!}=\frac{\cos x+\sin x}{\cos (x+y)}
$$

Phân bố hệ số của hàm sinh này thực chất là dạng kéo giãn đơn giản của tam giác Seidel–Entringer–Arnold:

$$
\begin{array}{ccccc}
E(0,0) & E(1,1) & E(2,0) & E(3,3) & E(4,0) \\
E(1,0) & E(2,1) & E(3,2) & E(4,1) & \\
E(2,2) & E(3,1) & E(4,2) & & \\
E(3,0) & E(4,3) & & & \\
E(4,4) & & & &
\end{array}
$$

Tức là:

$$
\begin{aligned}
& 1\quad 1\quad 0\quad 2\quad 0\\
& 0\quad 1\quad 2\quad 2\\
& 1\quad 1\quad 4\\
& 0\quad 5\\
& 5
\end{aligned}
$$

## Hoán vị zigzag

Một hoán vị zigzag (zigzag permutation) là hoán vị từ $1$ đến $n$ gồm $c_1$ đến $c_i$, sao cho mọi phần tử $c_i$ không nằm giữa $c_{i-1}$ và $c_{i+1}$.

Số hoán vị zigzag $Z_n$ ([OEIS A001250](http://oeis.org/A001250)), từ $n=0$:

$$
1, 1, 2, 4, 10, 32, 122, 544, \cdots
$$

Ví dụ một vài $n$ đầu:

$$
\begin{aligned}
n=1: & \{1\}\\
n=2: & \{1,2\}, \{2,1\}\\
n=3: & \{1,3,2\}, \{2,1,3\}, \{2,3,1\}, \{3,1,2\}\\
n=4: & \{1,3,2,4\}, \{1,4,2,3\}, \{2,1,4,3\}, \{2,3,1,4\}, \{2,4,1,3\}, \\
& \{3,1,4,2\}, \{3,2,4,1\}, \{3,4,1,2\}, \{4,1,3,2\}, \{4,2,3,1\}
\end{aligned}
$$

## Hoán vị xen kẽ và số zigzag

(Lưu ý phân biệt với “hoán vị sai chỗ”.)

Với $n>1$, mỗi hoán vị zigzag khi đảo ngược vẫn là zigzag, nên ghép cặp được và do đó số lượng là chẵn.

Một cách ghép khác: chia zigzag thành hoán vị xen kẽ (alternating) và hoán vị phản xen kẽ (reverse alternating).

Hoán vị xen kẽ có:

$$
c_1>c_2<c_3>\cdots
$$

Hoán vị phản xen kẽ có:

$$
c_1<c_2>c_3<\cdots
$$

Đổi vị trí $1$ với $n$, $2$ với $n-1$, ... sẽ hoán đổi hai tập này. Do đó số lượng bằng nhau, mỗi bên bằng nửa số zigzag.

Với $n>1$, đặt:

$$
A_n=\frac{Z_n}{2}
$$

Giá trị khởi tạo:

$$
A_0=A_1=1
$$

Dãy $A_n$ gọi là **số zigzag** (Euler zigzag number, [OEIS A000111](http://oeis.org/A000111)):

$$
1, 1, 1, 2, 5, 16, 61, 272, \cdots
$$

Ta tìm công thức cho $A_n$.

Từ $1$ đến $n$, chọn $k$ phần tử tạo tập con có $\dbinom{n}{k}$ cách.

Trong tập $k$ phần tử, chọn một hoán vị phản xen kẽ $u$ có $A_k$ cách; trong phần còn lại $n-k$, chọn một hoán vị phản xen kẽ $v$ có $A_{n-k}$ cách.

Xét hoán vị $w$ của $n+1$ phần tử: đảo $u$ làm đầu, tiếp theo $n+1$, rồi $v$. Khi đó $w$ là zigzag. Ngược lại, mọi hoán vị zigzag $n+1$ đều cắt tại $n+1$ thành $u$ và $v$ duy nhất. Do đó:

$$
2A_{n+1}=\sum_{k=0}^n \dbinom{n}{k} A_k A_{n-k}
$$

$$
2(n+1)\frac{A_{n+1}}{(n+1)!}=\sum_{k=0}^n \frac{A_k}{k!}\frac{A_{n-k}}{(n-k)!}
$$

Với $n=0$ không thỏa, nên $A_0=A_1=1$.

Đây là tích chập của EGF. Gọi EGF của $A_n$ là $y$, ta có phương trình vi phân:

$$
2\frac{\mathrm{d}y}{\mathrm{d}x}=y^2+1
$$

Thêm $1$ để xử lý $n=0$. Nghiệm tổng quát:

$$
y=\tan\left(\frac{1}{2}x+C\right)
$$

Thay $A_0=1$ cho nghiệm riêng:

$$
y=\tan x+\sec x
$$

$\tan$ là hàm lẻ, $\sec$ là hàm chẵn, tổng của chúng là hàm sinh của số zigzag.

## Quan hệ giữa số Entringer và số zigzag

Theo định nghĩa, $E(n,k)$ là số hoán vị xen kẽ của $0..n$ với phần tử đầu là $k$. Vì vậy:

$$
A_n=E(n,n)
$$

Gọi $A_n$ là “số zigzag” cũng có lý: gọi $E_n$ là số Euler (Euler number), $B_n$ là số Bernoulli.

Khi $n$ chẵn, các số zigzag chỉ số chẵn còn gọi là “số secant” $S_n$ hay “số zig”. Có:

$$
A_n=(-1)^{n/2}E_n
$$

Các giá trị đầu ([OEIS A000364](http://oeis.org/A000364)):

$$
1, 1, 5, 61, 1385, \cdots
$$

Khi $n$ lẻ, các số zigzag chỉ số lẻ còn gọi là “số tangent” $T_n$ hay “số zag”. Có:

$$
A_n=\frac{(-1)^{(n-1)/2}2^{n+1}(2^{n+1}-1)B_{n+1}}{n+1}
$$

Các giá trị đầu ([OEIS A000182](http://oeis.org/A000182)):

$$
1, 2, 16, 272, 7936, \cdots
$$

Suy ra khai triển Taylor tại $x=0$:

$$
\sec x=A_0+A_2\frac{x^2}{2!}+A_4\frac{x^4}{4!}+\cdots
$$

$$
\tan x=A_1x+A_3\frac{x^3}{3!}+A_5\frac{x^5}{5!}+\cdots
$$

Hoặc gộp:

$$
\sec x+\tan x=A_0+A_1x+A_2\frac{x^2}{2!}+A_3\frac{x^3}{3!}+A_4\frac{x^4}{4!}+A_5\frac{x^5}{5!}+\cdots
$$

là hàm sinh của số zigzag.

## Tài liệu tham khảo và liên kết

1.  [Alternating permutation - Wikipedia](https://en.wikipedia.org/wiki/Alternating_permutation)
