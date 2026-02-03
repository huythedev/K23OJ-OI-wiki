## Tịnh tiến đa thức

Tịnh tiến đa thức là một trường hợp đơn giản của phép hợp thành đa thức. Cho hệ số của $f(x)=\sum _ {i=0}^nf_ix^i$ và một hằng số $c$, hãy tìm hệ số của $f(x+c)$, tức $f(x)\mapsto f(x+c)$.

### Phương pháp chia để trị

Đặt

$$
f(x)=f_0(x)+x^{\left\lfloor n/2\right\rfloor}f_1(x)
$$

khi đó

$$
f(x+c)=f_0(x+c)+(x+c)^{\left\lfloor n/2\right\rfloor}f_1(x+c)
$$

Hệ số của $(x+c)^{\left\lfloor n/2\right\rfloor}$ là các hệ số nhị thức, do đó

$$
T(n)=2T(n/2)+O(n\log n)=O(n\log^2 n)
$$

trong đó $O(n\log n)$ là thời gian nhân đa thức.

### Phương pháp công thức Taylor

Áp dụng công thức Taylor của $f(x)$ tại $c$:

$$
f(x)=f(c)+\frac{f'(c)}{1!}(x-c)+\frac{f''(c)}{2!}(x-c)^2+\cdots +\frac{f^{(n)}(c)}{n!}(x-c)^n
$$

Suy ra

$$
f(x+c)=f(c)+\frac{f'(c)}{1!}x+\frac{f''(c)}{2!}x^2+\cdots +\frac{f^{(n)}(c)}{n!}x^n
$$

Nhận thấy với $t\geq 0$:

$$
\begin{aligned}
t!\lbrack x^t\rbrack f(x+c)&=f^{(t)}(c)\\
&=\sum _ {i=t}^nf_ii!\frac{c^{i-t}}{(i-t)!}\\
&=\sum _ {i=0}^{n-t}f _ {i+t}(i+t)!\frac{c^i}{i!}
\end{aligned}
$$

Đặt

$$
\begin{aligned}
A_0(x)&=\sum _ {i=0}^nf _ {n-i}(n-i)!x^i\\
B_0(x)&=\sum _ {i=0}^n\frac{c^i}{i!}x^i
\end{aligned}
$$

thì

$$
\begin{aligned}
\lbrack x^{n-t}\rbrack (A_0(x)B_0(x))&=\sum _ {i=0}^{n-t} (\lbrack x^{n-t-i}\rbrack A_0(x))(\lbrack x^i\rbrack B_0(x))\\
&=\sum _ {i=0}^{n-t}f _ {i+t}(i+t)!\frac{c^i}{i!}\\
&=t!\lbrack x^t\rbrack f(x+c)
\end{aligned}
$$

### Phương pháp định lý nhị thức

Xét định lý nhị thức $\displaystyle (a+b)^n=\sum _ {i=0}^n\binom{n}{i}a^ib^{n-i}$, ta có

$$
\begin{aligned}
f(x+c)&=\sum _ {i=0}^nf_i(x+c)^i\\
&=\sum _ {i=0}^nf_i\left(\sum _ {j=0}^i\binom{i}{j}x^jc^{i-j}\right)\\
&=\sum _ {i=0}^nf_ii!\left(\sum _ {j=0}^i\frac{x^j}{j!}\frac{c^{i-j}}{(i-j)!}\right)\\
&=\sum _ {i=0}^n\frac{x^i}{i!}\left(\sum _ {j=i}^{n}f_jj!\frac{c^{j-i}}{(j-i)!}\right)
\end{aligned}
$$

Kết quả trùng với các phương pháp trên.

## Tịnh tiến giá trị tại các điểm liên tiếp

???+ note "Ví dụ [LOJ 166 Nội suy Lagrange 2](https://loj.ac/p/166)"
    Cho các giá trị liên tiếp $f(0),f(1),\dots ,f(n)$ của đa thức $f$ bậc không vượt quá $n$. Tính $f(c),f(c+1),\dots ,f(c+n)$ theo modulo $998244353$, với $1\leq n\leq 10^5,n < m\leq 10^8$.

### Phương pháp công thức nội suy Lagrange

Xét [công thức nội suy Lagrange](../numerical/interp.md#lagrange-插值法):

$$
\begin{aligned}
f(x)&=\sum _ {0\leq i\leq n}f(i)\prod _ {0\leq j\leq n\,\land \,j\neq i}\frac{x-j}{i-j}\\
&=\sum _ {0\leq i\leq n}f(i)\frac{x!}{(x-n-1)!(x-i)}\frac{(-1)^{n-i}}{i!(n-i)!}\\
&=\frac{x!}{(x-n-1)!}\sum _ {0\leq i\leq n}\frac{f(i)}{(x-i)}\frac{(-1)^{n-i}}{i!(n-i)!}
\end{aligned}
$$

Dù là dạng tích chập nhưng không đảm bảo $x-i\neq 0$ ở mẫu, nên dưới đây chỉ xét $c > n$. Các trường hợp khác (ví dụ hệ số theo modulo số nguyên tố cần tránh mẫu của $B_0(x)$ bằng 0) có thể xử lý phân trường hợp. Đặt

$$
\begin{aligned}
A_0(x)&=\sum _ {0\leq i\leq n}\frac{f(i)(-1)^{n-i}}{i!(n-i)!}x^i\\
B_0(x)&=\sum _ {i\geq 0}\frac{1}{c-n+i}x^i
\end{aligned}
$$

khi đó với $t\geq 0$:

$$
\begin{aligned}
\lbrack x^{n+t}\rbrack (A_0(x)B_0(x))&=\sum _ {i=0}^{n+t}(\lbrack x^i\rbrack A_0(x))(\lbrack x^{n+t-i}\rbrack B_0(x))\\
&=\sum _ {i=0}^{n}\frac{f(i)(-1)^{n-i}}{i!(n-i)!}\frac{1}{c+t-i}\\
&=\frac{(c+t-n-1)!}{(c+t)!}f(c+t)
\end{aligned}
$$

Trong cài đặt có thể cắt bớt phần cần thiết của $B_0(x)$ để lấy nhiều điểm hơn, và có thể dùng tích chập vòng.

Sửa bài toán một chút: giả sử ta có các giá trị $f(d),f(d+k),\dots ,f(d+nk)$, thì có thể tính $f(c+d),f(c+d+k),\dots ,f(c+d+nk)$ bằng cách coi đó là tịnh tiến các giá trị của $g(x)=f(d+kx)$ từ $g(0),g(1),\dots ,g(n)$ sang $g(c/k),g(c/k+1),\dots ,g(c/k+n)$.

Công thức nội suy Lagrange cũng cho cách tính tuyến tính một điểm bằng cách duy trì các tích tiền tố/hậu tố.

## Ứng dụng

### Các số Stirling loại một không dấu trên cùng một hàng

???+ note "Ví dụ [P5408 Stirling loại một · hàng](https://www.luogu.com.cn/problem/P5408)"
    Tính $\displaystyle {n\brack 0},{n\brack 1},\dots ,{n\brack n}$ theo modulo số nguyên tố $167772161$, với $1\leq n< 262144$.

Xét

$$
x^{\overline{n}}=\sum _ {i=0}^n{n\brack i}x^i,\quad n\geq 0
$$

trong đó $x^{\overline{n}}=x\cdot (x+1)\cdots (x+n-1)$ là lũy thừa giai thừa tăng. Đặt $f_n(x)=x^{\overline{n}}$, ta có

$$
f_{2n}(x)=x^{\overline{n}}\cdot (x+n)^{\overline{n}}=f_n(x)f_n(x+n)
$$

Tịnh tiến đa thức cho phép tính $f_n(x+n)$ trong $O(n\log n)$, kích thước bài toán giảm một nửa, nên

$$
T(n)=T(n/2)+O(n\log n)=O(n\log n)
$$

### Giai thừa theo modulo số nguyên tố

???+ note "Ví dụ [P5282【Mẫu】Thuật toán giai thừa nhanh](https://www.luogu.com.cn/problem/P5282)"
    Tính $n!\bmod p$, với $p$ là số nguyên tố và $1\leq n< p\leq 2^{31}-1$.

Đặt $v=\lfloor\sqrt{n}\rfloor$ và $g(x)=\prod _ {i=1}^v(x+i)$, khi đó

$$
n!\equiv \left(\prod _ {i=0}^{v-1}g(iv)\right)\cdot \prod _ {i=v^2+1}^n i\pmod{p}
$$

Trong đó $\prod _ {i=v^2+1}^n i$ tính được trong $O(\sqrt{n})$, ta muốn tính nhanh phần đầu.

#### Đa điểm giá trị đa thức

Hệ số $g(x)$ có thể tính bằng thuật toán tịnh tiến đa thức trong $O(n\log n)$, nhưng tính $g(0),g(v),g(2v),\dots ,g(v^2-v)$ bằng đa điểm giá trị cần $O(\sqrt{n}\log ^2n)$.

#### Tịnh tiến giá trị tại các điểm liên tiếp

Đặt $g_d(x)=\prod _ {i=1}^d(x+i)$, ta có thể dùng $d+1$ điểm $g_d(0),g_d(v),\dots ,g_d(dv)$ để xác định duy nhất đa thức bậc $d$. Mặt khác

$$
g _ {2d}(x)=g_d(x)g_d(x+d)
$$

nên chỉ cần $2d+1$ điểm là xác định được $g _ {2d}(x)$. Khi đó dùng tịnh tiến điểm liên tiếp để tính $g_d((d+1)v),g_d((d+2)v),\dots ,g_d(2dv)$ (tức tịnh tiến điểm của $h(x)=g_d(vx)$ từ $h(0),h(1),\dots ,h(d)$ sang $h(d+1),h(d+2),\dots ,h(2d)$) và $g_d(d),g_d(v+d),\dots ,g_d(2dv+d)$ (tức tịnh tiến điểm của $h(x)=g_d(vx)$ từ $h(0),h(1),\dots ,h(d)$ sang $h(d/v),h(d/v+1),h(d/v+2),\dots ,h(d/v+2d)$). Nhân từng cặp điểm tương ứng sẽ được $g _ {2d}(0),g _ {2d}(v),\dots ,g _ {2d}(2dv)$.

Từ $g_d(0),g_d(v),\dots ,g_d(dv)$ tính $g _ {d+1}(0),g _ {d+1}(v),\dots ,g _ {d+1}(dv),g _ {d+1}((d+1)v)$, xét

$$
g _ {d+1}(x)=(x+d+1)\cdot g_d(x)
$$

Điểm bổ sung có thể tính tuyến tính. Khởi tạo $g_1(0)=1,g_1(v)=v+1$, rồi dùng tịnh tiến điểm liên tiếp để nhân đôi và duy trì các điểm, ta có

$$
T(n)=T(n/2)+O(n\log n)=O(n\log n)
$$

Ta chỉ cần khoảng $\sqrt{n}$ điểm, nên tổng thời gian $O(\sqrt{n}\log n)$.

### Tổng tiền tố hệ số nhị thức theo modulo số nguyên tố

???+ note "Ví dụ [LOJ 6386 Tổng tiền tố tổ hợp](https://loj.ac/p/6386)"
    Tính $\displaystyle \sum _ {i=0}^m\binom{n}{i}\bmod 998244353$, với $0\leq m\leq n\leq 9\times 10^8$.

Xét truy hồi $n!=n\cdot (n-1)!$ dưới dạng ma trận:

$$
\begin{bmatrix}
n!
\end{bmatrix}
=\left(
\prod _ {i=0}^{n-1}
\begin{bmatrix}i+1\end{bmatrix}
\right)
\begin{bmatrix}
1
\end{bmatrix}
$$

Tương tự, truy hồi của tổng tiền tố hệ số nhị thức:

$$
\begin{bmatrix}
\binom{n}{m+1}\\
\sum _ {i=0}^m\binom{n}{i}
\end{bmatrix}=
\begin{bmatrix}
(n-m)/(m+1)&0\\
1&1
\end{bmatrix}
\begin{bmatrix}
\binom{n}{m}\\
\sum _ {i=0}^{m-1}\binom{n}{i}
\end{bmatrix}
$$

Chú ý thứ tự nhân ma trận, ta có

$$
\begin{aligned}
\begin{bmatrix}
\binom{n}{m+1}\\
\sum _ {i=0}^m\binom{n}{i}
\end{bmatrix}
&=\left(
\prod _ {i=0}^{m}
\begin{bmatrix}
(n-i)/(i+1)&0\\1&1
\end{bmatrix}
\right)
\begin{bmatrix}
1\\0
\end{bmatrix}\\
&=
\frac{1}{(m+1)!}
\left(
\prod _ {i=0}^{m}
\begin{bmatrix}
n-i&0\\i+1&i+1
\end{bmatrix}
\right)
\begin{bmatrix}
1\\0
\end{bmatrix}
\end{aligned}
$$

Đặt $v=\lfloor\sqrt{m}\rfloor$, xét ma trận

$$
\begin{aligned}
M _ d(x)&=
\prod _ {i=1}^d
\begin{bmatrix}
-x+n+1-i&0\\
x+i&x+i
\end{bmatrix}\\
&=
\begin{bmatrix}
f_d(x)&0\\
g_d(x)&h_d(x)
\end{bmatrix}
\end{aligned}
$$

Ta duy trì các điểm $M _ d(0),M _ d(v),\dots ,M_d(dv)$, tức các điểm của $f_d,h_d,g_d$, và

$$
\begin{aligned}
M _ {2d}(x)&=
\prod _ {i=1}^{2d}
\begin{bmatrix}
-x+n+1-i&0\\
x+i&x+i
\end{bmatrix}\\
&=
\left(
\prod _ {i=1}^d
\begin{bmatrix}
-x-d+n+1-i&0\\
x+d+i&x+d+i
\end{bmatrix}
\right)
\left(
\prod _ {i=1}^d
\begin{bmatrix}
-x+n+1-i&0\\
x+i&x+i
\end{bmatrix}
\right) \\
&=
\begin{bmatrix}
f_d(x+d)&0\\
g_d(x+d)&h_d(x+d)
\end{bmatrix}
\begin{bmatrix}
f_d(x)&0\\
g_d(x)&h_d(x)
\end{bmatrix} \\
&=
\begin{bmatrix}
f_d(x+d)f_d(x)&0\\
g_d(x+d)f_d(x)+h_d(x+d)g_d(x)&h_d(x+d)h_d(x)
\end{bmatrix}
\end{aligned}
$$

Phần tử góc phải dưới chính là thứ cần duy trì trong bài giai thừa, nên

$$
\begin{aligned}
\prod _ {i=0}^{m}
\begin{bmatrix}
n-i&0\\i+1&i+1
\end{bmatrix}=
\left(
\prod _ {i=(k+1)v}^m
\begin{bmatrix}
n-i&0\\
i+1&i+1
\end{bmatrix}
\right)
\begin{bmatrix}
f_v(kv)&0\\
g_v(kv)&h_v(kv)
\end{bmatrix}
\cdots
\begin{bmatrix}
f_v(0)&0\\
g_v(0)&h_v(0)
\end{bmatrix}
\end{aligned}
$$

Có thể tính trong $O(\sqrt m\log m)$.

### Số điều hòa theo modulo số nguyên tố

???+ note "Ví dụ [P5702 Tổng cấp số điều hòa](https://www.luogu.com.cn/problem/P5702)"
    Tính $\sum _ {i=1}^ni^{-1}\bmod p$, với $p$ là số nguyên tố và $1\leq n< p< 2^{30}$.

Đặt $H_n=\sum _ {k=1}^nk^{-1}$, truy hồi một bước:

$$
\begin{bmatrix}
(n+1)!\\(n+1)!H _ {n+1}
\end{bmatrix}=
\begin{bmatrix}
n+1&0\\1&n+1
\end{bmatrix}
\begin{bmatrix}
n!\\n!H_n
\end{bmatrix}
$$

Suy ra

$$
\begin{bmatrix}
{n+1\brack 1}\\{n+1\brack 2}
\end{bmatrix}=
\begin{bmatrix}
n!\\n!H_n
\end{bmatrix}=
\left(
\prod _ {i=0}^{n-1}
\begin{bmatrix}
i+1&0\\1&i+1
\end{bmatrix}
\right)
\begin{bmatrix}
1\\0
\end{bmatrix}
$$

Ở đây $\displaystyle {n+1\brack 1}$ và $\displaystyle {n+1\brack 2}$ là số Stirling loại một không dấu. Cách duy trì ma trận điểm giống như trên.

## Truy hồi đa thức

Với trường hợp tổng quát hơn, tương tự bài giai thừa nhanh ở trên, ta cần một thuật toán như thế nào?

???+ note "Ví dụ [P6115【Mẫu】Truy hồi đa thức](https://www.luogu.com.cn/problem/P6115)"
    Dãy $a$ thỏa $\forall n\ge m,\sum_{k=0}^ma_{n-k}P_k(n)=0$, trong đó $P_k$ là đa thức bậc không vượt quá $d$.  
    Cho hệ số của các $P_k$ và $a_0,a_1,\dots,a_{m-1}$, tính $a_n$ theo modulo $998244353$. $n\le6\times10^8$，$1\le m,d\le7$，giới hạn thời gian $7s$.

Để mô tả hệ thống hóa cách dựng ma trận trong các ví dụ trên, ta dùng khái niệm [ma trận $\lambda$](../linear-algebra/jordan.md#lambda-%E7%9F%A9%E9%98%B5).

Trong thuật toán giai thừa nhanh, ta không duy trì $n!$ trực tiếp mà duy trì $\prod_{i=0}^{T-1}(aT+i)$, tức **quan hệ nhân giữa hai điểm**.

Với truy hồi đa thức bậc $m>1$, ta **không thể chỉ duy trì quan hệ nhân giữa hai số** nữa; thay vào đó cần **duy trì phép biến đổi tuyến tính giữa hai vector $m$ chiều**, tức một ma trận $m\times m$, **mỗi phần tử là một điểm của một đa thức**.

Dễ thấy với truy hồi đa thức tổng quát để tính hệ số xa, ta có thể dựng:

$$
-{\frac{1}{P_0(n)}}\begin{bmatrix}P_1(n)&P_2(n)&P_3(n)&\cdots&P_{m-1}(n)&P_m(n)\\-P_0(n)\\&-P_0(n)\\&&-P_0(n)\\&&&\ddots\\&&&&-P_0(n)\\\end{bmatrix}
\begin{bmatrix}a_{n-1}\\a_{n-2}\\a_{n-3}\\\vdots\\a_{n-m+1}\\a_{n-m}\end{bmatrix}
=\begin{bmatrix}a_n\\a_{n-1}\\a_{n-2}\\\vdots\\a_{n-m+2}\\a_{n-m+1}\end{bmatrix}
$$

Đặt

$$
B(\lambda)=\begin{bmatrix}
    P_1(\lambda)&P_2(\lambda)&P_3(\lambda)&\cdots&P_{m-1}(\lambda)&P_m(\lambda)\\
    -P_0(\lambda)\\
    &-P_0(\lambda)\\
    &&-P_0(\lambda)\\
    &&&\ddots\\
    &&&&-P_0(\lambda)\\
\end{bmatrix}
$$

Bỏ qua hệ số $-\frac1{P_0(n)}$, ta cần duy trì dạng $\prod_{i=0}^{T-1}B(aT+m+i)$, nhân từ phải sang trái.

Dễ thấy $B_T(\lambda)=\prod_{i=0}^{T-1}B(\lambda+i)$ là ma trận $\lambda$ có bậc không quá $dT$, chỉ cần $dT+1$ giá trị để duy trì.

Ta duy trì các **giá trị điểm** của $B_T(m)$, $B_T(m+T)$, $B_T(m+2T)$, $\dots$，$B_T(m+(dT-1)T)$, $B_T(m+dT^2)$, rồi dùng cách tương tự giai thừa nhanh để tịnh tiến điểm và nhân đôi.

Cụ thể, để tăng $t=\log_2T$ lên 1:

1.  Trong $O(m^2dT\log(dT))$ tính $B_T(p+dT^2)$, $B_T(p+(dT+1)T)$, $B_T(p+(dT+2)T)$, $\cdots$，$B_T(p+(2dT-1)T)$, $B_T(p+(2dT)dT)$.
2.  Trong $O(m^2dT\log(dT))$ tính $B_T(p+2dT^2)$, $B_T(p+(2dT+1)T)$, $B_T(p+(2dT+2)T)$, $\cdots$，$B_T(p+(3dT-1)T)$, $B_T(p+(3dT)dT)$.
3.  Trong $O(m^2dT\log(dT))$ tính $B_T(p+3dT^2)$, $B_T(p+(3dT+1)T)$, $B_T(p+(3dT+2)T)$, $\cdots$，$B_T(p+(4dT-1)T)$, $B_T(p+(4dT)dT)$.
4.  Tính $B_{2T}(v)=B_{T}(v+T)B_{T}(v)$.

Mỗi vòng tốn $O(m^2dT\log(dT))$ để tịnh tiến; đồng thời chỉ cần $\Theta(dT)$ phép nhân ma trận, nên coi như $O(m^3dT)$.

Cuối cùng chỉ cần $T\ge\sqrt{n/d}$.

Hệ số $-\frac1{P_0(n)}$ phía trước xử lý tương tự.

Độ phức tạp tiền xử lý là $\Theta(\sqrt{nd}(m^3+m^2\log(nd)))$.

Khi truy vấn, ta cần $\Theta(n/T)$ phép nhân vector–ma trận, và $O(T)$ bước chuyển thẳng.

Phần này không phải nút thắt chính.

Vì vậy tổng độ phức tạp là $\Theta(\sqrt{nd}(m^3+m^2\log(nd)))$.

Trong cài đặt có thể dùng tích chập vòng để giảm hằng số của NTT.

Trong ứng dụng thực tế, thường là trích hệ số xa của một hàm sinh có đạo hàm hữu hạn; khi đó $m,d$ là hằng số, đạt $\Theta(\sqrt n\log n)$.

## Tài liệu tham khảo

-   Alin Bostan, Pierrick Gaudry, and Eric Schost. Linear recurrences with polynomial coefficients and application to integer factorization and Cartier–Manin operator.
-   Blog của Min\_25
-   [Blog của ZZQ - giai thừa modulo số nguyên tố lớn](https://www.cnblogs.com/zzqsblog/p/8408691.html)
