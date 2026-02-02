## Mở đầu

Hoán vị và tổ hợp là nền tảng của tổ hợp học. **Hoán vị** là chọn một số phần tử từ tập đã cho và sắp thứ tự; **tổ hợp** là chọn nhưng không xét thứ tự. Vấn đề trung tâm là đếm số cách chọn thỏa điều kiện. Hoán vị–tổ hợp liên hệ chặt với xác suất cổ điển.

Trong toán phổ thông, thường dùng liệt kê, vét cạn, ... để giải.

## Nguyên lý cộng & nhân

### Nguyên lý cộng

Một công việc có $n$ loại cách, $a_i(1 \le i \le n)$ là số cách loại $i$. Tổng số cách là $S=a_1+a_2+\cdots +a_n$.

### Nguyên lý nhân

Một công việc cần $n$ bước, $a_i(1 \le i \le n)$ là số cách ở bước $i$. Tổng số cách là $S = a_1 \times a_2 \times \cdots \times a_n$.

## Cơ bản về hoán vị và tổ hợp

### Số hoán vị

Chọn $m$ phần tử từ $n$ phần tử khác nhau và sắp thứ tự, gọi là một hoán vị; số hoán vị gọi là $\mathrm A_n^m$ (hoặc $\mathrm P_n^m$).

Công thức:

$$
\mathrm A_n^m = n(n-1)(n-2) \cdots (n-m+1) = \frac{n!}{(n - m)!}
$$

$n!$ là giai thừa.

Có thể hiểu: chọn $m$ người từ $n$ người để xếp hàng. Vị trí đầu có $n$ chọn, tiếp theo $n-1$, ..., vị trí $m$ có $n-m+1$ chọn:

$$
\mathrm A_n^m = \frac{n!}{(n - m)!}
$$

Hoán vị toàn phần:

$$
\mathrm A_n^n = n! 
$$

### Số tổ hợp

Chọn $m\le n$ phần tử tạo thành tập (không xét thứ tự), gọi là một tổ hợp; số tổ hợp ký hiệu $\dbinom{n}{m}$, đọc là “$n$ chọn $m$”.

Công thức:

$$
\dbinom{n}{m} = \frac{\mathrm A_n^m}{m!} = \frac{n!}{m!(n - m)!}
$$

Giải thích: nếu xét thứ tự là $\mathrm A_n^m$, bỏ thứ tự thì chia cho $m!$ (hoán vị của $m$ phần tử).

$$
\begin{aligned}
\dbinom{n}{m} \times m! &= \mathrm A_n^m\\
\dbinom{n}{m} &= \frac{n!}{m!(n-m)!}
\end{aligned}
$$

Tổ hợp cũng hay ký hiệu $\mathrm C_n^m=\binom{n}{m}$, nhưng ký hiệu chuẩn là $\dbinom{n}{m}$.

Quy ước: nếu $m>n$ thì $\mathrm A_n^m=\dbinom{n}{m}=0$.

## Phương pháp sao và vạch (Stars and bars)

Dùng để đếm số cách chia các phần tử giống nhau, hoặc nghiệm phương trình tuyến tính.

### Số nghiệm nguyên dương

Bài toán 1: có $n$ phần tử **hoàn toàn giống nhau**, chia thành $k$ nhóm, mỗi nhóm ít nhất một phần tử. Có bao nhiêu cách?

Đặt $k-1$ vạch vào $n-1$ khoảng giữa $n$ phần tử. Do phần tử giống nhau, đáp án là $\dbinom{n - 1}{k - 1}$.

Tương đương số nghiệm nguyên dương của $x_1+x_2+\cdots+x_k=n$.

### Số nghiệm nguyên không âm

Bài toán 2: nếu cho phép nhóm rỗng?

Không thể áp dụng trực tiếp. Ta “mượn” $k$ phần tử để đảm bảo mỗi nhóm có ít nhất 1, rồi cắm vạch:

$$
\binom{n + k - 1}{k - 1} = \binom{n + k - 1}{n}
$$

Đây chính là đáp án thật: sau khi chia, trả lại $k$ phần tử đã mượn. Vì phần tử giống nhau, hai bài toán tương đương.

Do đó số nghiệm không âm của $x_1+\cdots+x_k=n$ là $\dbinom{n + k - 1}{n}$.

### Nghiệm có cận dưới khác nhau

Bài toán 3: mỗi nhóm $i$ cần ít nhất $a_i$, với $\sum a_i \le n$.

Tương đương $x_1+\cdots+x_k=n$, $x_i \ge a_i$.

Mượn $\sum a_i$ phần tử, đặt:

$$
x_i^{\prime}=x_i-a_i
$$

Ta có:

$$
\begin{aligned}
(x_1^{\prime}+a_1)+(x_2^{\prime}+a_2)+\cdots+(x_k^{\prime}+a_k)&=n\\
x_1^{\prime}+x_2^{\prime}+\cdots+x_k^{\prime}&=n-\sum a_i
\end{aligned}
$$

với $x_i^{\prime}\ge 0$.

Bài toán trở về trường hợp 2, đáp án:

$$
\binom{n - \sum a_i + k - 1}{n - \sum a_i}
$$

### Chọn không kề nhau

Chọn $k$ số từ $1..n$ sao cho không có hai số kề nhau, số cách là $\dbinom {n-k+1}{k}$.

## Định lý nhị thức

Định lý nhị thức cho hệ số của khai triển:

$$
(a+b)^n=\sum_{i=0}^n\binom{n}{i}a^{n-i}b^i
$$

Chứng minh bằng quy nạp, dùng $\dbinom{n}{k}+\dbinom{n}{k-1}=\dbinom{n+1}{k}$.

Mở rộng đa thức:

$$
(x_1 + x_2 + \cdots + x_t)^n = \sum_{满足 n_1 + \cdots + n_t=n 的非负整数解} \binom{n}{n_1,n_2,\cdots,n_t} x_1^{n_1}x_2^{n_2}\cdots x_t^{n_t}
$$

Hệ số đa thức:

$$
\sum{\binom{n}{n_1,n_2,\cdots,n_t}} = t^n
$$

## Hoán vị–tổ hợp nâng cao

### Hoán vị đa tập | Đa tổ hợp

Phải phân biệt **đa tổ hợp** và **tổ hợp của đa tập**.

Đa tập là tập có phần tử lặp. Gọi $S=\{n_1\cdot a_1,n_2\cdot a_2,\cdots,n_k\cdot a_k\}$ gồm $n_1$ phần tử $a_1$, ..., $n_k$ phần tử $a_k$. Số hoán vị toàn phần:

$$
\frac{n!}{\prod_{i=1}^kn_i!}=\frac{n!}{n_1!n_2!\cdots n_k!}
$$

Xem như có $k$ loại bóng khác nhau với số lượng $n_i$, tổng $n$. Số hoán vị chính là **hoán vị đa tập**, còn gọi **đa tổ hợp**. Ký hiệu:

$$
\binom{n}{n_1,n_2,\cdots,n_k}=\frac{n!}{\prod_{i=1}^kn_i!}
$$

Ta có $\dbinom{n}{m}=\dbinom{n}{m,n-m}$ nhưng thường không dùng vì rườm rà.

### Tổ hợp của đa tập 1

Với đa tập $S$ như trên, với số nguyên $r(r<n_i,\forall i\in[1,k])$, số cách chọn $r$ phần tử tạo đa tập là **tổ hợp của đa tập**. Bài toán tương đương số nghiệm không âm của $x_1+\cdots+x_k=r$, nên dùng sao–vạch:

$$
\binom{r+k-1}{k-1}
$$

### Tổ hợp của đa tập 2

Xét: với đa tập $S$ như trên, chọn $r$ phần tử với giới hạn số lượng từng loại. Ta có:

$$
\forall i\in [1,k],\ x_i\le n_i,\ \sum_{i=1}^kx_i=r
$$

Dùng bao hàm–loại trừ. Mô hình:

1.  Không gian mẫu: nghiệm không âm của $\sum x_i=r$.
2.  Thuộc tính: $x_i\le n_i$.

Gọi $S_i$ là tập thỏa thuộc tính $i$, $\overline{S_i}$ là tập không thỏa, tức $x_i\ge n_i+1$ (trở về bài sao–vạch có cận). Khi đó:

$$
\left|\bigcap_{i=1}^kS_i\right|=|U|-\left|\bigcup_{i=1}^k\overline{S_i}\right|
$$

Theo bao hàm–loại trừ:

$$
\begin{aligned}
\left|\bigcup_{i=1}^k\overline{S_i}\right|
=&\sum_i\left|\overline{S_i}\right|
-\sum_{i,j}\left|\overline{S_i}\cap\overline{S_j}\right|
+\sum_{i,j,k}\left|\overline{S_i}\cap\overline{S_j}\cap\overline{S_k}\right|
-\cdots\\
&+(-1)^{k-1}\left|\bigcap_{i=1}^k\overline{S_i}\right|\\
=&\sum_i\binom{k+r-n_i-2}{k-1}
-\sum_{i,j}\binom{k+r-n_i-n_j-3}{k-1}+\sum_{i,j,k}\binom{k+r-n_i-n_j-n_k-4}{k-1}
-\cdots\\
&+(-1)^{k-1}\binom{k+r-\sum_{i=1}^kn_i-k-1}{k-1}
\end{aligned}
$$

Lấy $|U|=\binom{k+r-1}{k-1}$ trừ đi ta được:

$$
Ans=\sum_{p=0}^k(-1)^p\sum_{A}\binom{k+r-1-\sum_{A} n_{A_i}-p}{k-1}
$$

trong đó $A$ là tập con kích thước $p$, thỏa $A_i<A_{i+1}$.

### Hoán vị vòng tròn

$n$ người xếp vòng tròn, số hoán vị là $\mathrm Q_n^n$. Cắt vòng tròn tại các vị trí khác nhau cho ra các hàng khác nhau, nên:

$$
\mathrm Q_n^n \times n = \mathrm A_n^n \Longrightarrow \mathrm Q_n = \frac{\mathrm A_n^n}{n} = (n-1)!
$$

Công thức hoán vị vòng tròn một phần:

$$
\mathrm Q_n^r = \frac{\mathrm A_n^r}{r} = \frac{n!}{r \times (n-r)!}
$$

## Tính chất tổ hợp | Hệ quả nhị thức

$$
\binom{n}{m}=\binom{n}{n-m}\tag{1}
$$

Tính đối xứng.

$$
\binom{n}{k} = \frac{n}{k} \binom{n-1}{k-1}\tag{2}
$$

Từ định nghĩa.

$$
\binom{n}{m}=\binom{n-1}{m}+\binom{n-1}{m-1}\tag{3}
$$

Công thức Pascal.

$$
\binom{n}{0}+\binom{n}{1}+\cdots+\binom{n}{n}=\sum_{i=0}^n\binom{n}{i}=2^n\tag{4}
$$

Từ nhị thức.

$$
\sum_{i=0}^n(-1)^i\binom{n}{i}=[n=0]\tag{5}
$$

Nhị thức với $a=1, b=-1$.

$$
\sum_{i=0}^k \binom{n}{i}\binom{m}{k-i} = \binom{m+n}{k}\tag{6}
$$

Đẳng thức Vandermonde.

$$
\sum_{i=0}^n\binom{n}{i}^2=\binom{2n}{n}\tag{7}
$$

Trường hợp đặc biệt của (6).

$$
\sum_{i=0}^ni\binom{n}{i}=n2^{n-1}\tag{8}
$$

Tổng có trọng số, suy ra từ đạo hàm.

$$
\sum_{i=0}^ni^2\binom{n}{i}=n(n+1)2^{n-2}\tag{9}
$$

Tương tự.

$$
\sum_{l=0}^n\binom{l}{k} = \binom{n+1}{k+1}\tag{10}
$$

Đẳng thức “gậy khúc” (Hockey-stick).

$$
\binom{n}{r}\binom{r}{k} = \binom{n}{k}\binom{n-k}{r-k}\tag{11}
$$

Từ định nghĩa.

$$
\sum_{i=0}^n\binom{n-i}{i}=F_{n+1}\tag{12}
$$

$F$ là Fibonacci.

$$
\binom{n+k}{k}^2=\sum_{j=0}^k\binom{k}{j}^2\binom{n+2k-j}{2k}\tag{13}
$$

Đẳng thức Li Shanlan.

## Phép nghịch đảo nhị thức

Gọi $f_n$ là số cách dùng đúng $n$ phần tử khác nhau tạo cấu trúc; $g_n$ là số cách chọn $i \geq 0$ phần tử từ $n$ phần tử để tạo cấu trúc.

Biết $f_n$ suy ra $g_n$:

$$
g_n = \sum_{i = 0}^{n} \binom{n}{i} f_i
$$

Biết $g_n$ suy ra $f_n$:

$$
f_n = \sum_{i = 0}^{n} \binom{n}{i} (-1)^{n-i} g_i
$$

Quá trình trên gọi là **nghịch đảo nhị thức**.

### Chứng minh

Khai triển $g_i$:

$$
\begin{aligned}
f_n &= \sum_{i = 0}^{n} \binom{n}{i} (-1)^{n-i} \left[\sum_{j = 0}^{i} \binom{i}{j} f_j\right] \\
&= \sum_{i = 0}^{n}\sum_{j = 0}^{i}\binom{n}{i}\binom{i}{j} (-1)^{n-i}f_j
\end{aligned}
$$

Đổi thứ tự tổng:

$$
\begin{aligned}
f_n &= \sum_{j = 0}^{n}\sum_{i = j}^{n}\binom{n}{i}\binom{i}{j} (-1)^{n-i}f_j \\
&= \sum_{j = 0}^{n}f_j\sum_{i = j}^{n}\binom{n}{i}\binom{i}{j} (-1)^{n-i}
\end{aligned}
$$

Dùng (11):

$$
\begin{aligned}
f_n &= \sum_{j = 0}^{n}f_j\sum_{i = j}^{n}\binom{n}{j}\binom{n - j}{i - j} (-1)^{n-i} \\
&= \sum_{j = 0}^{n}\binom{n}{j}f_j\sum_{i = j}^{n}\binom{n - j}{i - j} (-1)^{n-i}
\end{aligned}
$$

Đặt $k=i-j$:

$$
f_n = \sum_{j = 0}^{n}\binom{n}{j}f_j\sum_{k = 0}^{n - j}\binom{n - j}{k} (-1)^{n-j-k}1^{k}
$$

Dùng (5):

$$
f_n = \sum_{j = 0}^{n}\binom{n}{j}f_j[n = j] = f_n
$$

Chứng minh xong.
