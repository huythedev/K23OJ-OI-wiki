## Hoán vị sai chỗ

### Định nghĩa

Hoán vị sai chỗ (derangement) là hoán vị không có phần tử nào ở đúng vị trí. Tức với hoán vị $P$ của $1\sim n$, nếu $P_i\neq i$ thì $P$ là hoán vị sai chỗ của $n$.

Ví dụ, hoán vị sai chỗ bậc 3 có $\{2,3,1\}$ và $\{3,1,2\}$. Bậc 4 có $\{2,1,4,3\}$、$\{2,3,4,1\}$、$\{2,4,1,3\}$、$\{3,1,4,2\}$、$\{3,4,1,2\}$、$\{3,4,2,1\}$、$\{4,1,2,3\}$、$\{4,3,1,2\}$ và $\{4,3,2,1\}$. Hoán vị sai chỗ là hoán vị không có điểm bất động, tức không có chu trình độ dài 1.

### Tính bằng bao hàm–loại trừ

Không gian mẫu $U$ là tất cả hoán vị của $1\sim n$, $|U|=n!$; đặt $S_i$ là tập hoán vị thỏa $P_i\neq i$. Dùng bù và [bao hàm–loại trừ](./inclusion-exclusion-principle.md), bài toán thành:

$$
\begin{aligned}
\left|\bigcap_{i=1}^n S_i\right|
&=|U|-\left|\bigcup_{i=1}^n\overline{S_i}\right|\\
&=n!-\sum_{k=1}^n(-1)^{k-1}\sum_{a_i<a_{i+1}}\left|\bigcap_{i=1}^{k}\overline{S_{a_i}}\right|
\end{aligned}
$$

Tổng trong là chọn $a_1, a_2, \cdots, a_k$ từ $1..n$ sao cho $a_i<a_{i+1}$. Khi đó

$$
\left|\bigcap_{i=1}^{k}\overline{S_{a_i}}\right|
$$

là số hoán vị có $k$ vị trí cố định $P_{a_i}=a_i$, còn lại $n-k$ vị trí tùy ý, nên:

$$
\left|\bigcap_{i=1}^{k}\overline{S_{a_i}}\right|=(n-k)!
$$

Có $\dbinom{n}{k}$ cách chọn $k$ vị trí, nên:

$$
\begin{aligned}
&\sum_{k=1}^n(-1)^{k-1}\sum_{a_i<a_{i+1}}\left|\bigcap_{i=1}^{k}\overline{S_{a_i}}\right|\\
=&\sum_{k=1}^n(-1)^{k-1}\dbinom{n}{k}(n-k)!\\
=&\sum_{k=1}^n(-1)^{k-1}\frac{n!}{k!}\\
=&n!\sum_{k=1}^n\frac{(-1)^{k-1} }{k!}
\end{aligned}
$$

Vậy số hoán vị sai chỗ:

$$
D_n=n!-n!\sum_{k=1}^n\frac{(-1)^{k-1} }{k!}=n!\sum_{k=0}^n\frac{(-1)^k}{k!}
$$

Dãy bắt đầu: $0,1,2,9,44,265$ ([OEIS A000166](http://oeis.org/A000166)).

### Tính bằng truy hồi

Xét bài toán đặt thư vào phong bì:

$n$ lá thư đánh số $1,2,3,4,5$ cần đặt vào phong bì $1..5$, yêu cầu số thư khác số phong bì. Hỏi số cách?

Xét phong bì $n$, ban đầu đặt thư $n$ vào phong bì $n$, rồi xét hai trường hợp:

-   $n-1$ phong bì trước đều sai chỗ;
-   $n-1$ phong bì trước có đúng một cái đúng chỗ, còn lại sai.

Trường hợp 1: $n-1$ phong bì trước đều sai, thì thư $n$ chỉ cần đổi chỗ với một vị trí trong $n-1$ vị trí, có $D_{n-1}\times (n-1)$ cách.

Trường hợp 2: có đúng một phong bì đúng chỗ, đổi thư ở đó với thư $n$ sẽ tạo thành một hoán vị sai chỗ bậc $n$.

Các trường hợp khác không thể thành hoán vị sai chỗ bậc $n$ chỉ bằng một phép đổi.

Do đó:

$$
D_n=(n-1)(D_{n-1}+D_{n-2})
$$

Ngoài ra còn có:

$$
D_n=nD_{n-1}+{(-1)}^n
$$

### Quan hệ khác

Số hoán vị sai chỗ có công thức làm tròn đơn giản, tốc độ tăng gần như $n!$:

$$
D_n=\begin{cases}
    \left\lceil\frac{n!}{\mathrm{e}}\right\rceil, & \text{n chẵn}, \\
    \left\lfloor\frac{n!}{\mathrm{e}}\right\rfloor,            & \text{n lẻ}.
\end{cases}
$$

Khi $n$ lớn, xác suất một hoán vị là sai chỗ:

$$
P=\lim_{n\to\infty}\frac{D_n}{n!}=\frac{1}{\mathrm{e}}
$$
