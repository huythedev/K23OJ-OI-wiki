author: Peanut-Tang, Early0v0, Vxlimo, GHLinZhengyu, 1196131597

"Thuật toán Meissel–Lehmer" là một thuật toán có thể đếm số lượng số nguyên tố trong $1\sim n$ với độ phức tạp thời gian dưới tuyến tính．

## Các ký hiệu

$\left[x\right]$ biểu diễn phần nguyên của $x$．  
$p_k$ biểu diễn số nguyên tố thứ $k$，$p_1=2$．  
$\pi\left(x\right)$ biểu diễn số lượng số nguyên tố trong $1\sim x$．  
$\mu\left(x\right)$ biểu diễn hàm Möbius．  
Cho tập hợp $S$，$\# S$ biểu diễn số lượng phần tử của $S$．  
$\delta\left(x\right)$ biểu diễn số nguyên tố nhỏ nhất của $x$．  
$P^+\left(x\right)$ biểu diễn số nguyên tố lớn nhất của $x$．

## Thuật toán Meissel–Lehmer đếm $\pi(x)$

Định nghĩa $\phi\left(x,a\right)$ là số lượng các số nguyên dương nhỏ hơn $x$ mà tất cả các thừa số nguyên tố đều lớn hơn $p_a$，tức là:

$$
\phi\left(x,a\right)=\#\big\{n\le x\mid n\bmod p=0 \implies p>p_a\big\}\tag{1}
$$

Định nghĩa $P_k\left(x,a\right)$ là số lượng các số nguyên dương nhỏ hơn $x$ mà có đúng $k$ thừa số nguyên tố (tính lặp lại) và tất cả các thừa số nguyên tố đều lớn hơn $p_a$，tức là:

$$
P_k\left(x,a\right)=\#\big\{n\le x\mid n=q_1q_2\cdots q_k \implies \forall i,q_i>p_a\big\}\tag{2}
$$

Đặc biệt，ta định nghĩa: $P_0\left(x,a\right)=1$，vậy ta có:

$$
\phi\left(x,a\right)=P_0\left(x,a\right)+P_1\left(x,a\right)+\cdots+P_k\left(x,a\right)+\cdots
$$

Dãy vô hạn này thực tế có thể biểu diễn bằng một dãy hữu hạn vì khi $p_a^k>x$ thì $P_k\left(x,a\right)=0$．

Giả sử $y$ là một số nguyên thỏa mãn $x^{1/3}\le y\le x^{1/2}$，và $a=\pi\left(y\right)$．

Khi $k\ge 3$ thì $P_1\left(x,a\right)=\pi\left(x\right)-a$ và $P_k\left(x,a\right)=0$，vậy ta có:

$$
\pi\left(x\right)=\phi\left(x,a\right)+a-1-P_2\left(x,a\right)\tag{3}
$$

Vậy để tính $\pi\left(x\right)$ ta chỉ cần tính $\phi\left(x,a\right)$ và $P_2\left(x,a\right)$．

## Tính $P_2\left(x,a\right)$

Từ đẳng thức $\left(2\right)$ ta có $P_2\left(x,a\right)$ bằng số lượng các cặp số nguyên tố $\left(p,q\right)$ thỏa mãn $y<p\le q$ và $pq\le x$．

Trước hết ta nhận thấy $p\in \left[y+1,\sqrt{x}\right]$．Ngoài ra，với mỗi $p$ ta đều có $q\in\left[p,x/p\right]$．Vậy:

$$
P_2\left(x,a\right)=\sum_{y<p\le \sqrt{x}}{\left(\pi\left(\dfrac{x}{p}\right)-\pi\left(p\right)+1\right)}\tag{4}
$$

Khi $p\in \left[y+1,\sqrt{x}\right]$ thì $\dfrac{x}{p}\in \left[1,\dfrac{x}{y}\right]$．Vậy ta có thể sàng khoảng $\left[1,\dfrac{x}{y}\right]$，rồi với mọi số nguyên tố $p\in \left[y+1,\sqrt{x}\right]$ tính $\pi\left(\dfrac{x}{p}\right)-\pi\left(p\right)+1$．Để giảm bớt độ phức tạp không gian của thuật toán này，ta có thể chia thành các khối，độ dài khối là $L$．Nếu độ dài khối $L=y$ thì ta có thể tính $P_2\left(x,a\right)$ trong $O\left(\dfrac{x}{y}\log{\log{x}}\right)$ thời gian và $O\left(y\right)$ không gian．

## Tính $\phi\left(x,a\right)$

Với $b\le a$，xem xét tất cả các số nguyên dương không quá $x$ mà tất cả các thừa số nguyên tố đều lớn hơn $p_{b-1}$．Các số này có thể chia thành hai nhóm:

1.  Có thể chia hết cho $p_b$；
2.  Không thể chia hết cho $p_b$．

Nhóm thứ $1$ có $\phi\left(\dfrac{x}{p_b},b-1\right)$ phần tử，nhóm thứ $2$ có $\phi\left(x,b\right)$ phần tử．

Vậy ta có kết luận:

> **Định lý $5.1$**: Hàm $\phi$ thỏa mãn các tính chất sau
>
> $$
> \phi\left(u,0\right)=\left[u\right]\tag{5}
> $$
>
> $$
> \phi\left(x,b\right)=\phi\left(x,b-1\right)-\phi\left(\dfrac{x}{p_b},b-1\right)\tag{6}
> $$

Cách tính $\phi\left(x,a\right)$ đơn giản có thể suy ra từ định lý này: ta lặp lại đẳng thức $\left(7\right)$ cho đến khi được $\phi\left(u,0\right)$．Quá trình này có thể xem như tạo ra một cây nhị phân có gốc $\phi\left(x,a\right)$，hình $1$ vẽ ra quá trình này．Từ phương pháp này ta có công thức:

$$
\phi\left(x,a\right)=\sum_{\substack{1\le n\le x\\ P^+\left(n\right)\le y}}{\mu\left(n\right)\left[x/n\right]}
$$

$$
\begin{gathered}
\begin{matrix}&&\phi\left(x,a\right)&&\\
&\swarrow&&\searrow&\\
&\phi\left(x,a-1\right)&&-\phi\left(\frac{x}{p_a},a-1\right)&\\
\swarrow&\downarrow&&\downarrow&\searrow\\
\phi\left(x,a-2\right)&\phi\left(\frac{x}{p_{a-1}},a-2\right)&&-\phi\left(\frac{x}{p_a},a-2\right)&\phi\left(\frac{x}{p_ap_{a-1}},a-2\right)\end{matrix}\\
\vdots\\
\end{gathered}
$$

Hình trên biểu diễn quá trình tính $\phi\left(x,a\right)$ như một cây nhị phân: tổng các trọng số của các lá là $\phi\left(x,a\right)$．

Nhưng như vậy cần tính quá nhiều thứ．Vì $y\geq x^{1/3}$，chỉ cần tính các tích của $3$ số nguyên tố không quá $y$，nếu theo phương pháp này thì sẽ có ít nhất $\dfrac{x}{\log^3 x}$ hạng mục，không thể đáp ứng nhu cầu về độ phức tạp．

Để hạn chế sự "trưởng thành" của cây nhị phân này，ta phải thay đổi điều kiện kết thúc．Điều kiện kết thúc ban đầu là:

> **Điều kiện kết thúc $1$**: Nếu $b=0$ thì không gọi lại đẳng thức $\left(6\right)$ cho nút $\mu\left(n\right)\phi\left(\dfrac xn,b\right)$．

Ta thay đổi thành điều kiện kết thúc mạnh hơn:

> **Điều kiện kết thúc $2$**: Nếu thỏa mãn một trong hai điều kiện sau thì không gọi lại đẳng thức $\left(6\right)$ cho nút $\mu\left(n\right)\phi\left(\dfrac xn,b\right)$:
>
> 1.  $b=0$ và $n\le y$；
> 2.  $n>y$．

Ta dựa vào **Điều kiện kết thúc $2$** để chia các lá của cây nhị phân thành hai loại:

1.  Nếu lá nút $\mu\left(n\right)\phi\left(\dfrac xn,b\right)$ thỏa mãn $n\le y$ thì gọi là **lá bình thường**；
2.  Nếu lá nút $\mu\left(n\right)\phi\left(\dfrac xn,b\right)$ thỏa mãn $n>y$ và $n=mp_b\left(m\le y\right)$ thì gọi là **lá đặc biệt**．

Vậy ta có:

> **Định lý $5.2$**: Ta có:
>
> $$
> \phi\left(x,a\right)=S_0+S\tag{7}
> $$
>
> Trong đó $S_0$ biểu diễn **lá bình thường**:
>
> $$
> S_0=\sum_{n\le y}{\mu\left(n\right)\left[\dfrac xn\right]}\tag{8}
> $$
>
> $S$ biểu diễn **lá đặc biệt**:
>
> $$
> S=\sum_{n/\delta\left(n\right)\le y\le n}{\mu\left(n\right)\phi\left(\dfrac{x}{n},\pi\left(\delta\left(n\right)\right)-1 \right)}\tag{9}
> $$

Tính $S_0$ rõ ràng có thể làm trong $O\left(y\log{\log x}\right)$ thời gian．Bây giờ ta cần tính $S$．

## Tính $S$

Ta có:

$$
S=-\sum_{p\le y}{\ \sum_{\substack{\delta\left(m\right)>p\\ m\le y<mp}}{\mu\left(m\right)\phi\left(\dfrac{x}{mp},\pi\left(p\right)-1\right)}}\tag{10}
$$

Ta viết lại:

$$
S=S_1+S_2+S_3
$$

Trong đó:

$$
S_1=-\sum_{x^{1/3}<p\le y}{\ \sum_{\substack{\delta\left(m\right)>p\\ m\le y<mp}}{\mu\left(m\right)\phi\left(\dfrac{x}{mp},\pi\left(p\right)-1\right)}}
$$

$$
S_2=-\sum_{x^{1/4}<p\le x^{1/3}}{\ \sum_{\substack{\delta\left(m\right)>p\\ m\le y<mp}}{\mu\left(m\right)\phi\left(\dfrac{x}{mp},\pi\left(p\right)-1\right)}}
$$

$$
S_3=-\sum_{p\le x^{1/4}}{\ \sum_{\substack{\delta\left(m\right)>p\\ m\le y<mp}}{\mu\left(m\right)\phi\left(\dfrac{x}{mp},\pi\left(p\right)-1\right)}}
$$

Chú ý rằng các tổng $S_1,S_2$ có chứa các $m$ đều là số nguyên tố，chứng minh như sau:

> Nếu không như vậy，vì có $\delta\left(m\right)>p>x^{1/4}$，vậy có $m>p^2>\sqrt{x}$，điều này mâu thuẫn với $m\le y$，vậy mệnh đề đúng．

Ngoài ra，khi $mp>x^{1/2}\ge y$ thì $y\le mp$．Vậy ta có:

$$
S_1=\sum_{x^{1/3}<p\le y}{\ \sum_{p<q\le y}{\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1\right)}}
$$

$$
S_2=\sum_{x^{1/4}<p\le x^{1/3}}{\ \sum_{p<q\le y}{\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1\right)}}
$$

### Tính $S_1$

Vì:

$$
\dfrac{x}{pq}<x^{1/3}<p
$$

Vậy:

$$
\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1\right)=1
$$

Vậy các hạng mục trong tổng $S_1$ đều bằng $1$．Vậy ta thực sự cần tính số lượng các cặp số nguyên tố $\left(p,q\right)$ thỏa mãn: $x^{1/3}<p<q\le y$．

Vậy:

$$
S_1=\dfrac{\left(\pi\left(y\right)-\pi\left(x^{1/3}\right)\right)\left(\pi\left(y\right)-\pi\left(x^{1/3}\right)-1\right)}{2}
$$

Vậy ta có thể tính $S_1$ trong $O\left(1\right)$ thời gian．

### Tính $S_2$

Ta có:

$$
S_2=\sum_{x^{1/4}<p\le x^{1/3}}{\ \sum_{p<q\le y}{\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1\right)}}
$$

Ta chia $S_2$ thành hai phần:

$$
S_2=U+V
$$

Trong đó:

$$
U=\sum_{x^{1/4}<p\le x^{1/3}}{\ \sum_{\substack{p<q<y\\q>x/p^2}}{\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1 \right)}}
$$

$$
V=\sum_{x^{1/4}<p\le x^{1/3}}{\ \sum_{\substack{p<q<y\\q\le x/p^2}}{\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1 \right)}}
$$

### Tính $U$

Từ $q>\dfrac x{p^2}$ ta có $p^2>\dfrac xq\le \dfrac xy,p>\sqrt{\dfrac xy}$，vậy:

$$
U=\sum_{\sqrt{x/y}<p\le x^{1/3}}{\ \sum_{\substack{p<q\le y\\q>x/p^2}}{\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1 \right)}}
$$

Vậy:

$$
U=\sum_{\sqrt{x/y}<p\le x^{1/3}}{\#\left\{q\mid \dfrac x{p^2}<q\le y \right\}}
$$

Vậy:

$$
U=\sum_{\sqrt{x/y}<p\le x^{1/3}}{\left(\pi\left(y\right)-\pi\left(\dfrac{x}{p^2} \right) \right)}
$$

Vì có $\dfrac x{p^2}<y$，vậy ta có thể pre-process tất cả $\pi\left(t\right)\left(t\le y\right)$，vậy ta có thể tính $U$ trong $O\left(y\right)$ thời gian．

### Tính $V$

Với mỗi hạng mục trong tổng $V$ ta đều có $p\le \dfrac{x}{pq}<x^{1/2}<p^2$．Vậy:

$$
\phi\left(\dfrac{x}{pq},\pi\left(p\right)-1 \right)=1+\pi\left(\dfrac{x}{pq} \right)-\left(\pi\left(p\right)-1\right)=2-\pi\left(p\right)+\pi\left(\dfrac{x}{pq} \right)
$$

Vậy $V$ có thể biểu diễn như:

$$
V=V_1+V_2
$$

Trong đó:

$$
V_1=\sum_{x^{1/4}<p\le x^{1/3}}{\ \sum_{p<q\le \min\left(x/p^2,y\right)}{\left(2-\pi\left(p\right)\right)}}
$$

$$
V_2=\sum_{x^{1/4}<p\le x^{1/3}}{\ \sum_{p<q\le \min\left(x/p^2,y\right)}{\pi\left(\dfrac{x}{pq} \right)}}
$$

Pre-process $\pi\left(t\right)\left(t\le y\right)$ ta có thể tính $V_1$ trong $O\left(x^{1/3}\right)$ thời gian．

Xem xét cách tính $V_2$．Ta có thể chia $q$ thành các đoạn mà mỗi đoạn có $\pi\left(\dfrac{x}{pq} \right)$ là hằng số，vậy chỉ cần tính độ dài mỗi đoạn và sự thay đổi của $\pi\left(\dfrac{x}{pq} \right)$ từ một đoạn sang đoạn tiếp theo．

Chính xác hơn，ta chia $V_2$ thành hai phần，rút gọn điều kiện phức tạp:

$$
V_2=\sum_{x^{1/4}<p\le \sqrt{x/y}}{\ \sum_{p<q\le y}{\pi\left(\dfrac{x}{pq} \right)}}+\sum_{\sqrt{x/y}<p\le x^{1/3}}{\ \sum_{p<q\le x/p^2}{\pi\left(\dfrac{x}{pq} \right)}}
$$

Tiếp theo ta viết lại:

$$
V_2=W_1+W_2+W_3+W_4+W_5
$$

Trong đó:

$$
W_1=\sum_{x^{1/4}<p\le x/y^2}{\ \sum_{p<q\le y}{\pi\left(\dfrac{x}{pq} \right)}}
$$

$$
W_2=\sum_{x/y^2<p\le \sqrt{x/y}}{\ \sum_{p<q\le \sqrt{x/p}}{\pi\left(\dfrac{x}{pq} \right)}}
$$

$$
W_3=\sum_{x/y^2<p\le \sqrt{x/y}}{\ \sum_{\sqrt{x/p}<q\le y}{\pi\left(\dfrac{x}{pq} \right)}}
$$

$$
W_4=\sum_{\sqrt{x/y}<p\le x^{1/3}}{\ \sum_{p<q\le \sqrt{x/p}}{\pi\left(\dfrac{x}{pq} \right)}}
$$

$$
W_5=\sum_{\sqrt{x/y}<p\le x^{1/3}}{\ \sum_{\sqrt{x/p}<q\le x/p^2}{\pi\left(\dfrac{x}{pq} \right)}}
$$

#### Tính $W_1$ và $W_2$

Tính hai giá trị này cần tính các giá trị $\pi\left(\dfrac{x}{pq} \right)$ thỏa mãn $y<\dfrac{x}{pq}<x^{1/2}$．Có thể sàng trong khoảng $[1,\sqrt x]$ phân khối．Trong mỗi khối ta cộng tất cả các $(p,q)$ thỏa mãn điều kiện．

#### Tính $W_3$

Với mỗi $p$ ta chia $q$ thành các đoạn，mỗi đoạn có $\pi\left(\dfrac x{pq}\right)$ là hằng số．Khi ta có một $q$ mới，ta dùng bảng $\pi(t)$ ($t\leq y$) để tính $\pi\left(\dfrac x{pq}\right)$．$y$ trong các số nguyên tố có thể cho ta giá trị $t$ sao cho $\pi(t)<\pi(t+1)=\pi\left(\dfrac x{pq}\right)$．Từ đó ta có thể tìm được giá trị $q$ tiếp theo khiến $\pi\left(\dfrac x{pq}\right)$ thay đổi．

#### Tính $W_4$

So với $W_3$，$W_4$ có $q$ nhỏ hơn，vậy $\pi\left(\dfrac x{pq}\right)$ thay đổi nhanh hơn．Vậy ta không có lợi thế nào khi dùng phương pháp tính $W_3$ để tính $W_4$．Vậy ta trực tiếp liệt kê các cặp $(p,q)$ để tính $W_4$．

#### Tính $W_5$

Ta tính $W_5$ như tính $W_3$．

## Tính $S_3$

Ta dùng tất cả các số nguyên tố nhỏ hơn $x^{1/4}$ để sàng khoảng $\left[1,\dfrac xy\right]$．Khi sàng đến $p_k$ thì ta tính được tất cả các $m$ thỏa mãn không có bình phương nhân tử và $\delta(m)>p_k$．Giá trị $-\mu(m)\phi\left(\dfrac{x}{mp_k},k-1 \right)$．Sàng pháp này là phân khối，ta duy trì một cây nhị phân để thực thời cập nhật kết quả sau mỗi lần sàng．Vậy ta chỉ cần $O(\log x)$ thời gian để tính được số lượng số chưa bị sàng．

## Độ phức tạp

Độ phức tạp bị ảnh hưởng bởi ba quá trình:

1.  Tính $P_2\left(x,a\right)$；
2.  Tính $W_1,W_2,W_3,W_4,W_5$；
3.  Tính $S_3$．

### Tính $P_2\left(x,y\right)$

Ta đã biết độ phức tạp thời gian là $O\left(\dfrac{x}{y}\log{\log x}\right)$，không gian là $O\left(y\right)$．

### Tính $W_1,W_2,W_3,W_4,W_5$

Tính $W_1,W_2$ thực hiện sàng với độ dài khối $y$，độ phức tạp thời gian là $O\left(\sqrt{x}\log{\log x}\right)$，không gian là $O\left(y\right)$．

Tính $W_1$ cần:

$$
\pi\left(\dfrac{x}{y^2} \right)\pi\left(y\right)=O\left(\dfrac{x}{y\log^2 x} \right)
$$

Tính $W_2$ cần:

$$
O\left(\sum_{x/y^2<p\le \sqrt{x/y}}{\pi\left(\sqrt{\dfrac xp}\right)} \right)=O\left(\dfrac{x^{3/4}}{y^{1/4}\log^2 x} \right)
$$

Vậy tính $W_3$ cần:

$$
O\left(\sum_{x/y^2<p\le \sqrt{x/y}}{\pi\left(\sqrt{\dfrac xp}\right)} \right)=O\left(\dfrac{x^{3/4}}{y^{1/4}\log^2 x} \right)
$$

Tính $W_4$ cần:

$$
O\left(\sum_{\sqrt{x/y}<p\le x^{1/3}}{\pi\left(\sqrt{\dfrac xp}\right)} \right)=O\left(\dfrac{x^{2/3}}{\log^2 x} \right)
$$

Tính $W_5$ cần:

$$
O\left(\sum_{\sqrt{x/y}<p\le x^{1/3}}{\pi\left(\sqrt{\dfrac xp}\right)} \right)=O\left(\dfrac{x^{2/3}}{\log^2 x} \right)
$$

### Tính $S_3$

Với pre-process：vì cần nhanh chóng truy vấn $\phi(u,b)$，ta không thể dùng sàng thông thường $O(1)$ để tính，mà phải duy trì một cấu trúc dữ liệu để mỗi lần truy vấn có độ phức tạp $O(\log x)$，vậy thời gian là $O\left(\dfrac{x}{y}\log x\log\log x\right)$．

Với tổng：với mỗi hạng mục trong tổng $S_3$ ta truy vấn cấu trúc dữ liệu một lần，tổng cộng $O\left(\log x\right)$ lần truy vấn．Ta cần tính số lượng hạng mục trong tổng，tức là số lượng lá trong cây nhị phân．Tất cả các lá đều có dạng $\pm\phi\left(\dfrac{x}{mp_b},b-1\right)$，với $m\le y,b<\pi(x^{1/4})$．Vậy số lượng lá là $O\left(y\pi\left(x^{1/4}\right)\right)$ cấp．Vậy tổng thời gian tính $S_3$ là:

$$
O\left(\dfrac{x}{y}\log x\log\log x+yx^{1/4}\right)
$$

### Tổng độ phức tạp

Thuật toán này có không gian phức tạp $O\left(y\right)$，thời gian phức tạp là:

$$
O\left(\dfrac{x}{y}\log{\log x}+\dfrac{x}{y}\log x\log{\log x}+x^{1/4}y+\dfrac{x^{2/3}}{\log^2{x}} \right)
$$

Ta lấy $y=x^{1/3}\log^3{x}\log{\log x}$，vậy thời gian tối ưu là $O\left(\dfrac{x^{2/3}}{\log^2 x}\right)$，không gian là $O\left(x^{1/3}\log^3{x}\log{\log x}\right)$．

## Một số cải tiến

Ta đưa ra một số cải tiến để giảm hằng số và tăng hiệu suất thực tế．

-   Trong **Điều kiện kết thúc $2$**，ta có thể dùng một $z$ thay cho $y$，với $z>y$．Ta có thể chứng minh rằng như vậy thời gian tính $S_3$ có thể tối ưu hóa đến:

    $$
    O\left(\dfrac{x}{z}\log x\log{\log x}+\dfrac{yx^{1/4}}{\log x}+z^{3/2} \right)
    $$

    Điều này cũng cho phép kiểm tra tính toán bằng cách thay đổi $z$．

-   Để rõ ràng hơn，ta chọn chia ở $x^{1/4}$ để tính tổng $S$，thực tế ta chỉ cần $p\le \dfrac{x}{pq}<p^2$ để tính．Ta có thể dùng điều này để giữ độ phức tạp cận trên．

-   Dùng các số nguyên tố đầu tiên $2,3,5$ để pre-process có thể tiết kiệm thêm thời gian．

## Tài liệu tham khảo

Bài viết này được dịch từ: [Computing $\pi(x)$: the Meissel, Lehmer, Lagarias, Miller, Odlyzko method](https://dl.acm.org/doi/abs/10.1090/s0025-5718-96-00674-6)
