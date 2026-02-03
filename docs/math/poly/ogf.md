author: sshwy

Hàm sinh thường (ordinary generating function, OGF) của dãy $a$ được định nghĩa là chuỗi lũy thừa hình thức:

$$
F(x)=\sum_{n}a_n x^n
$$

$a$ có thể là dãy hữu hạn hoặc vô hạn. Ví dụ thường gặp (giả sử $a$ bắt đầu từ $0$):

1.  Dãy $a=\langle 1,2,3\rangle$ có OGF là $1+2x+3x^2$.
2.  Dãy $a=\langle 1,1,1,\cdots\rangle$ có OGF là $\sum_{n\ge 0}x^n$.
3.  Dãy $a=\langle 1,2,4,8,16,\cdots\rangle$ có OGF là $\sum_{n\ge 0}2^nx^n$.
4.  Dãy $a=\langle 1,3,5,7,9,\cdots\rangle$ có OGF là $\sum_{n\ge 0}(2n+1)x^n$.

Nói cách khác, nếu dãy $a$ có công thức tổng quát, thì hệ số của OGF chính là công thức đó.

## Phép toán cơ bản

Xét hai dãy $a,b$ với OGF tương ứng là $F(x),G(x)$. Khi đó

$$
F(x)\pm G(x)=\sum_n (a_n\pm b_n)x^n
$$

Vì vậy $F(x)\pm G(x)$ là OGF của dãy $\langle a_n\pm b_n\rangle$.

Xét phép nhân, tức là tích chập:

$$
F(x)G(x)=\sum_n x^n \sum_{i=0}^na_ib_{n-i}
$$

Vì vậy $F(x)G(x)$ là OGF của dãy $\langle \sum_{i=0}^n a_ib_{n-i} \rangle$.

## Dạng đóng

Khi dùng hàm sinh, ta không luôn giữ dạng chuỗi lũy thừa hình thức mà sẽ chuyển sang dạng đóng để rút gọn.

Ví dụ dãy $\langle 1,1,1,\cdots\rangle$ có OGF $F(x)=\sum_{n\ge 0}x^n$, ta thấy

$$
F(x)x+1=F(x)
$$

Giải được

$$
F(x)=\frac{1}{1-x}
$$

Đó là dạng đóng của $\sum_{n\ge 0}x^n$.

Xét cấp số nhân $\langle 1,p,p^2,p^3,p^4,\cdots\rangle$ có OGF $F(x)=\sum_{n\ge 0}p^nx^n$, ta có

$$
\begin{aligned}F(x)px+1 &=F(x)\\F(x) &=\frac{1}{1-px}\end{aligned}
$$

Dạng đóng và dạng khai triển của cấp số nhân là phép biến đổi rất thường dùng.

???+ note "Bài tập nhỏ"
    Hãy tìm OGF (dạng chuỗi và dạng đóng) của các dãy sau. Độ khó tăng dần.
    
    1.  $a=\langle 0,1,1,1,1,\cdots\rangle$.
    2.  $a=\langle 1,0,1,0,1,\cdots \rangle$.
    3.  $a=\langle 1,2,3,4,\cdots \rangle$.
    4.  $a_n=\binom{m}{n}$ ($m$ là hằng số, $n\ge 0$).
    5.  $a_n=\binom{m+n}{n}$ ($m$ là hằng số, $n\ge 0$).

??? note "Đáp án"
    Câu 1:
    
    $$
    F(x)=\sum_{n\ge 1}x^n=\dfrac{x}{1-x}
    $$
    
    Câu 2:
    
    $$
    \begin{aligned}
    F(x)&=\sum_{n\ge 0}x^{2n}\\
    &=\sum_{n\ge 0}(x^2)^{n}\\
    &=\frac{1}{1-x^2}
    \end{aligned}
    $$
    
    Câu 3 (lấy đạo hàm):
    
    $$
    \begin{aligned}F(x)&=\sum_{n\ge 0}(n+1)x^n\\&=\sum_{n\ge 1}nx^{n-1}\\&=\sum_{n\ge 0}(x^n)'\\&=\left(\frac{1}{1-x}\right)'\\&=\frac{1}{(1-x)^2}\end{aligned}
    $$
    
    Câu 4 (nhị thức Newton):
    
    $$
    F(x)=\sum_{n\ge 0}\binom{m}{n}x^n=(1+x)^m
    $$
    
    Câu 5:
    
    $$
    F(x)=\sum_{n\ge 0}\binom{m+n}{n}x^n=\frac{1}{(1-x)^{m+1}}
    $$
    
    Có thể chứng minh bằng quy nạp.
    
    Trước hết khi $m=0$, ta có $F(x)=\dfrac{1}{1-x}$.
    
    Khi $m>0$:
    
    $$
    \begin{aligned}
    \frac{1}{(1-x)^{m+1}}
    &=\frac{1}{(1-x)^m}\frac{1}{1-x}\\
    &=\left(\sum_{n\ge 0}\binom{m+n-1}{n}x^n \right)\left(\sum_{n\ge 0}x^n \right)\\
    &=\sum_{n\ge 0} x^n\sum_{i=0}^n \binom{m+i-1}{i}\\
    &=\sum_{n\ge 0}\binom{m+n}{n}x^n
    \end{aligned}
    $$

## Hàm sinh của dãy Fibonacci

Tiếp theo ta suy ra hàm sinh của dãy Fibonacci.

Dãy Fibonacci: $a_0=0,a_1=1,a_n=a_{n-1}+a_{n-2}\;(n>1)$. Gọi OGF là $F(x)$, từ truy hồi ta lập phương trình:

$$
F(x)=xF(x)+x^2F(x)-a_0x+a_1x+a_0
$$

Giải được

$$
F(x)=\frac{x}{1-x-x^2}
$$

Câu hỏi tiếp theo: làm sao khai triển?

### Cách khai triển 1

Xem $x+x^2$ là một khối:

$$
\begin{aligned}
F(x) &= \dfrac{x}{1-(x+x^2)} \\
&= x\sum_{k=0}^{\infty}(x+x^2)^k \\
&= x\sum_{k=0}^{\infty}\sum_{i=0}^k\binom{k}{i}x^{k-i}(x^2)^i \\
&= \sum_{k=0}^{\infty}\sum_{i=0}^k\binom{k}{i}x^{k+i+1} \\
&= \sum_{n=1}^{\infty}\sum_{i=0}^{\lfloor(n-1)/2\rfloor}\binom{n-i-1}{i}x^n.
\end{aligned}
$$

Bước cuối đặt $n=k+i+1$ và đổi thứ tự tổng. Suy ra công thức:

$$
a_n = \sum_{i=0}^{\lfloor(n-1)/2\rfloor}\binom{n-i-1}{i}.
$$

Đây không phải dạng liên quan tỉ lệ vàng quen thuộc.

### Cách khai triển 2

Xét phương trình hệ số bất định:

$$
\frac{A}{1-ax}+\frac{B}{1-bx}= \frac{x}{1-x-x^2}
$$

Quy đồng:

$$
\frac{A-Abx+B-aBx}{(1-ax)(1-bx)} = \frac{x}{1-x-x^2}
$$

So sánh hệ số, được:

$$
\begin{cases}
A+B=0\\
-Ab-aB=1\\
a+b=1\\
ab=-1
\end{cases}
$$

Giải:

$$
\begin{cases}
A=\frac{1}{\sqrt{5}}\\
B=-\frac{1}{\sqrt{5}}\\
a=\frac{1+\sqrt{5}}{2}\\
b=\frac{1-\sqrt{5}}{2}
\end{cases}
$$

Từ khai triển cấp số nhân, ta thu được công thức:

$$
\frac{x}{1-x-x^2}=\sum_{n\ge 0}x^n
\frac{1}{\sqrt{5}}\left( \left(\frac{1+\sqrt{5}}{2}\right)^n-\left(\frac{1-\sqrt{5}}{2}\right)^n \right)
$$

Đây là dạng đóng khác của dãy Fibonacci.

Với đa thức $P(x),Q(x)$, khai triển $\dfrac{P(x)}{Q(x)}$ có thể dùng phương pháp trên. Thực tế thường tìm nghiệm của $Q(x)$, viết mẫu dưới dạng $\prod (1-p_ix)^{d_i}$ rồi xử lý tử.

Nếu mẫu có nghiệm bội, cần thêm phân thức. Ví dụ

$$
G(x)=\frac{1}{(1-x)(1-2x)^2}
$$

khi đó

$$
G(x)=\frac{c_0}{1-x}+\frac{c_1}{1-2x}+\frac{c_2}{(1-2x)^2}
$$

Giải được

$$
\begin{cases}
c_0&=1\\
c_1&=-2\\
c_2&=2
\end{cases}
$$

Suy ra

$$
[x^n]G(x)=1-2^{n+1}+(n+1)\cdot 2^{n+1}
$$

## Nhị thức Newton

Định nghĩa lại tổ hợp:

$$
\binom{r}{k}=\frac{r^{\underline{k}}}{k!}\quad(r\in\mathbf{C},k\in\mathbf{N})
$$

Lưu ý $r$ là số phức. Khi đó với $\alpha\in\mathbf{C}$:

$$
(1+x)^{\alpha}=\sum_{n\ge 0}\binom{\alpha}{n}x^n
$$

Nhị thức thường là trường hợp đặc biệt của nhị thức Newton.

## Hàm sinh của số Catalan

Xem [Diễn giải đại số của số Catalan](../combinatorics/catalan.md#代数推演).

## Ứng dụng

Dưới đây là một số bài mẫu về ứng dụng OGF trong OI.

### Food

???+ note "[Food](https://hydro.ac/p/bzoj-P3028)"
    Chọn $n$ món từ nhiều loại, mỗi loại có ràng buộc:
    
    1.  Bánh hamburger: số lượng chẵn
    2.  Coca: 0 hoặc 1
    3.  Đùi gà: 0,1 hoặc 2
    4.  Nước đào: số lượng lẻ
    5.  Gà viên: bội của 4
    6.  Bánh bao: 0,1,2 hoặc 3
    7.  Thịt xào khoai tây: không quá 1
    8.  Bánh mì: bội của 3
    
    Mỗi loại tính theo “cái”, tổng số là $n$ tính là một phương án. Với $n$ cho trước, hãy tính số phương án modulo $10007$.

Đây là bài kinh điển về hàm sinh. Với một loại, gọi $a_n$ là số cách chọn $n$ cái và lập hàm sinh. Hai loại thì số cách là tích chập, tương ứng với tích các hàm sinh. Nhiều loại thì tương tự.

Các hàm sinh (theo số thứ tự):

1.  $\displaystyle\sum_{n\ge 0}x^{2n}=\dfrac{1}{1-x^2}$.
2.  $1+x$.
3.  $1+x+x^2=\dfrac{1-x^3}{1-x}$.
4.  $\dfrac{x}{1-x^2}$.
5.  $\displaystyle \sum_{n\ge 0}x^{4n}=\dfrac{1}{1-x^4}$.
6.  $1+x+x^2+x^3=\dfrac{1-x^4}{1-x}$.
7.  $1+x$.
8.  $\dfrac{1}{1-x^3}$.

Nhân lại:

$$
F(x)=\frac{(1+x)(1-x^3)x(1-x^4)(1+x)}{(1-x^2)(1-x)(1-x^2)(1-x^4)(1-x)(1-x^3)}
=\frac{x}{(1-x)^4}
$$

Đổi sang dạng khai triển (dùng bài tập thứ 5):

$$
F(x)=\sum_{n\ge 1}\binom{n+2}{n-1}x^n
$$

Suy ra đáp án $\dbinom{n+2}{n-1}=\dbinom{n+2}{3}$.

### Sweet

???+ note "[「CEOI2004」Sweet](https://hydro.ac/p/bzoj-P3027)"
    Có $n$ đống kẹo. Các đống khác nhau có loại kẹo khác nhau (một đống chỉ một loại). Đống $i$ có $m_i$ cái. Ăn ít nhất $a$ và không quá $b$ cái. Hỏi số phương án.
    
    Hai phương án khác nhau nếu tổng số kẹo khác nhau, hoặc số kẹo của một loại nào đó khác nhau.
    
    $n\le 10,0\le a\le b\le 10^7,m_i\le 10^6$.

Trong đống $i$, ăn $j$ cái (duy nhất một cách) có hàm sinh:

$$
F_i(x)=\sum_{j=0}^{m_i}x^j=\frac{1-x^{m_i+1}}{1-x}
$$

Tổng số ăn $i$ cái có hàm sinh:

$$
G(x)=\prod_{i=1}^n F_i(x)=(1-x)^{-n}\prod_{i=1}^n(1-x^{m_i+1})
$$

Cần $\sum_{i=a}^b[x^i]G(x)$.

Vì $n\le 10$, ta có thể triển khai $\prod_{i=1}^n(1-x^{m_i+1})$ bằng vét cạn (tối đa $2^n$ hạng).

Sau đó với $(1-x)^{-n}$ dùng nhị thức Newton:

$$
\begin{aligned}
(1-x)^{-n}
&=\sum_{i\ge 0}\binom{-n}{i}(-x)^i\\
&=\sum_{i\ge 0}\binom{n-1+i}{i}x^i
\end{aligned}
$$

Giả sử hệ số $x^k$ trong $\prod_{i=1}^n(1-x^{m_i+1})$ là $c_k$. Nhân với $(1-x)^{-n}$, đóng góp vào đáp án:

$$
c_k\sum_{i=a-k}^{b-k}\binom{n-1+i}{i}=c_k\left(
\binom{n+b-k}{b-k}-
\binom{n+a-k-1}{a-k-1}
\right)
$$

Như vậy tính được trong $O(b)$.

Độ phức tạp $O(2^n+b)$.
