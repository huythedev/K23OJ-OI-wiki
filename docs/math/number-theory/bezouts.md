Định lý Bézout揭示 mối liên hệ sâu sắc giữa ƯCLN và tổ hợp tuyến tính nguyên, là một trong những kết quả cơ bản và quan trọng nhất của số học. Dựa vào đó, bài viết tiếp tục thảo luận cách giải phương trình một ẩn bất định.

## Định lý Bézout

**Định lý Bézout** (Bézout's lemma), còn gọi là **đẳng thức Bézout** (Bézout's identity), cho điều kiện cần và đủ để một số nguyên có thể biểu diễn dưới dạng tổ hợp tuyến tính nguyên của hai số nguyên.

???+ note "Định lý Bézout"
    Cho $a,b$ không đồng thời bằng $0$. Với mọi $x,y$ nguyên, luôn có $\gcd(a,b)\mid ax+by$; hơn nữa tồn tại $x,y$ sao cho $ax+by=\gcd(a,b)$.

??? note "Chứng minh"
    Đặt $d=\gcd(a,b)$. Vì $d\mid a,b$ nên tồn tại $u,v$ sao cho $a=du,~b=dv$. Do đó
    
    $$
    ax + by = d(ux+vy).
    $$
    
    Suy ra $d\mid ax+by$.
    
    Ngược lại, cần chứng minh tồn tại $x,y$. Nếu một trong $a,b$ là $0$, giả sử $b=0$, thì $d=a$ và chọn $(x,y)=(1,0)$ là được. Xét trường hợp $a,b$ đều khác $0$. Do $\gcd(a,b)=\gcd(-a,b)=\gcd(a,-b)$, không mất tính tổng quát giả sử $a,b>0$.
    
    Xét thuật toán Euclid:
    
    $$
    \begin{aligned}
    a   &= q_1b   + r_1, && 0\le r_1 < b,\\
    b   &= q_2r_1 + r_2, && 0\le r_2 < r_1,\\
    r_1 &= q_3r_2 + r_3, && 0\le r_3 < r_2,\\
        & \cdots \\
    r_{n-3} &= q_{n-1}r_{n-2} + r_{n-1}, && 0\le r_{n-1} < r_{n-2},\\
    r_{n-2} &= q_nr_{n-1} + r_n,         && 0\le r_n     < r_{n-1},\\
    r_{n-1} &= q_{n+1}r_n.
    \end{aligned}
    $$
    
    Vì ƯCLN là $d$, nên $r_n=d$. Do đó
    
    $$
    d = r_n = r_{n-2} - q_nr_{n-1}.
    $$
    
    Từ phương trình trước đó:
    
    $$
    r_{n-1} = r_{n-3} - q_{n-1}r_{n-2}.
    $$
    
    Thế vào, được
    
    $$
    \begin{aligned}
    d &= r_{n-2} - q_n(r_{n-3} - q_{n-1}r_{n-2}) \\
    &= (1 + q_nq_{n-1})r_{n-2} - q_nr_{n-3}.
    \end{aligned}
    $$
    
    Tiếp tục khử $r_{n-2},r_{n-3},\cdots,r_2,r_1$ ta được
    
    $$
    d = xa + yb.
    $$
    
    Vậy tồn tại $x,y$ thỏa. Kết hợp với phần trên, mệnh đề được chứng minh.

Chứng minh tồn tại ở đây là xây dựng, đồng thời cung cấp cách tính hệ số, chính là [thuật toán Euclid mở rộng](./gcd.md#扩展欧几里得算法).

Xét trường hợp $\gcd(a,b)=1$ ta có hệ quả:

???+ note "Hệ quả"
    Hai số nguyên $a,b$ nguyên tố cùng nhau khi và chỉ khi tồn tại $x,y$ sao cho $ax+by=1$.

### Trường hợp nhiều số nguyên

Định lý Bézout có thể mở rộng cho nhiều số nguyên.

???+ note "Định lý"
    Cho $a_1,a_2,\cdots,a_n$ không đồng thời bằng $0$. Với mọi $x_1,\cdots,x_n$ nguyên, $\gcd(a_1,\cdots,a_n)\mid a_1x_1+\cdots+a_nx_n$; hơn nữa tồn tại $x_1,\cdots,x_n$ sao cho $\gcd(a_1,\cdots,a_n)=a_1x_1+\cdots+a_nx_n$.

??? note "Chứng minh"
    Dùng $\gcd(a_1,\cdots,a_n)=\gcd(\gcd(a_1,\cdots,a_{n-1}),a_n)$ và quy nạp theo $n$.

### Ví dụ

???+ example "[Codeforces 510 D. Fox And Jumping](https://codeforces.com/problemset/problem/510/D)"
    Cho $n\le 300$ thẻ, mỗi thẻ có $l_i$ và $c_i$. Trên một dải giấy vô hạn, bạn có thể trả $c_i$ để mua thẻ $i$, từ đó có thể nhảy trái/phải $l_i$ đơn vị bất kỳ lần nào. Hỏi chi phí tối thiểu để có thể đến mọi vị trí; nếu không được, in $-1$.

??? note "Lời giải"
    Để đến mọi ô, cần chọn $l_{i_1},\cdots,l_{i_k}$ sao cho có tổ hợp nguyên dương/âm bằng $1$, tức tồn tại $x_1,\cdots,x_k$ với $l_{i_1}x_1+\cdots+l_{i_k}x_k=1$. Theo Bézout nhiều biến, điều này tương đương chọn một tập $l_1,\cdots,l_n$ sao cho ƯCLN bằng $1$ và tổng chi phí nhỏ nhất.
    
    **Cách 1**: Xem bài toán là đường đi ngắn. Mỗi đỉnh là một giá trị ƯCLN hiện tại. Đỉnh xuất phát $0$, đích $1$. Mỗi bước từ $x$ sang $\gcd(x,l_i)$ với trọng số $c_i$. Dùng Dijkstra, độ phức tạp $O(n^2\log n)$.
    
    **Cách 2**: Xem như 0-1 knapsack. Đặt $f_{i,j}$ là chi phí nhỏ nhất khi xét $i$ số đầu và ƯCLN là $j$:
    
    $$
    f_{i, j} = \min_{\gcd(k, l_i) = j} f_{i - 1, k} + c_i.
    $$
    
    Kết quả là $f_{n,1}$. Dùng mảng cuộn; vì các ƯCLN có thể xuất hiện thưa, dùng hash để lưu.
    
    Thực ra, đồ thị ở cách 1 chính là đồ thị chuyển trạng thái của DP ở cách 2; cách 2 là tìm đường ngắn trên DAG, nên hai cách tương đương. Cách 2 không cần lưu toàn đồ thị và có độ phức tạp $O(n+m)$ nên tối ưu hơn.

## Phương trình bất định tuyến tính

**Phương trình bất định tuyến tính** (linear Diophantine equation) có dạng

$$
a_1x_1 + a_2x_2 + \cdots + a_nx_n = b
$$

trong đó $a_1,\cdots,a_n$ nguyên. Mục tiêu là tìm toàn bộ nghiệm nguyên.

### Hai biến

Xét

$$
a_1x_1 + a_2x_2 = b.
$$

Theo Bézout, có nghiệm khi và chỉ khi

$$
d = \gcd(a_1,a_2) \mid b.
$$

Giả sử điều kiện đúng. Dùng Euclid mở rộng tìm một nghiệm $(x_1^*,x_2^*)$ của $a_1x_1+a_2x_2=d$. Khi đó một nghiệm riêng là

$$
(x_1^\circ,x_2^\circ) = \left(\frac{b}{d}x_1^*,\frac{b}{d}x_2^*\right).
$$

Trừ đi phương trình $a_1x_1^\circ+a_2x_2^\circ=b$ ta được

$$
a_1(x_1 - x_1^\circ) + a_2(x_2 - x_2^\circ) = 0.
$$

Phương trình thuần nhất có nghiệm tổng quát

$$
(x_1-x_1^\circ,x_2-x_2^\circ) = \left(t\dfrac{a_2}{d},-t\dfrac{a_1}{d}\right).\quad(t\in\mathbf Z)
$$

Do đó nghiệm tổng quát:

$$
(x_1,x_2) = \left(x_1^\circ + t\dfrac{a_2}{d},x_2^\circ - t\dfrac{a_1}{d}\right).\quad(t\in\mathbf Z)
$$

Đây là các điểm nguyên cách đều trên đường thẳng $a_1x_1+a_2x_2=b$.

### Nhiều biến

Với $n$ biến ($n>3$):

$$
a_1x_1 + a_2x_2 + \cdots + a_nx_n = b
$$

Có nghiệm khi và chỉ khi

$$
\gcd(a_1,a_2,\cdots,a_n) \mid b.
$$

Nghiệm tổng quát có dạng

$$
(x_1^\circ,x_2^\circ,\cdots,x_n^\circ) + \sum_{k=1}^{n-1} t_k(x_1^{(k)},x_2^{(k)},\cdots,x_n^{(k)}).
$$

Để tìm cụ thể, đưa về $(n-1)$ biến. Đặt $d_1=\gcd(a_1,a_2)$, ta có mọi giá trị của $a_1x_1+a_2x_2$ chính là bội của $d_1$. Trước hết giải

$$
d_1y_1 + a_3x_3 + a_4x_4 + \cdots + a_nx_n = b.
$$

Giả sử nghiệm tổng quát:

$$
\begin{aligned}
y_1 &= y_1^\circ + \sum_{k=2}^{n-1}t_ky_1^{(k)}, \\
x_i &= x_i^\circ + \sum_{k=2}^{n-1}t_kx_i^{(k)},\quad i=3,\cdots,n.
\end{aligned}
$$

Gọi $(x_1^*,x_2^*)$ là nghiệm của $a_1x_1+a_2x_2=d_1$. Khi đó nghiệm tổng quát của $a_1x_1+a_2x_2=d_1y_1$ là

$$
x_1 = x_1^*y_1 + t_1\dfrac{a_2}{d_1},~x_2 = x_2^*y_1 - t_1\dfrac{a_1}{d_1}.
$$

Thế $y_1$ vào, ta được nghiệm tổng quát của bài toán gốc:

$$
\begin{aligned}
x_1 &= x_1^*y_1^\circ + t_1\dfrac{a_2}{d_1} + \sum_{k=2}^{n-1}t_kx_1^*y_1^{(k)}, \\
x_2 &= x_2^*y_1^\circ - t_1\dfrac{a_1}{d_1} + \sum_{k=2}^{n-1}t_kx_2^*y_1^{(k)}, \\
x_i &= x_i^\circ + \sum_{k=2}^{n-1}t_kx_i^{(k)},\quad i=3,\cdots,n.
\end{aligned}
$$

## Bài toán đồng xu Frobenius

Định lý Bézout cho điều kiện một số nguyên là tổ hợp tuyến tính nguyên. Liên quan chặt chẽ là **bài toán đồng xu Frobenius**:

-   Với các mệnh giá $a_1,\cdots,a_n$ nguyên dương, $\gcd(a_1,\cdots,a_n)=1$, số nguyên lớn nhất không thể biểu diễn bằng các mệnh giá đó là bao nhiêu?

Ở đây $x_i$ bị ràng buộc là số tự nhiên. Trường hợp $n=1$ là tầm thường, $n>2$ phức tạp; phần này chỉ xét $n=2$.

### Định lý Sylvester

Năm 1882, Sylvester giải trường hợp $n=2$:

???+ note "Định lý (Sylvester)"
    Với $a_1,a_2$ nguyên dương nguyên tố cùng nhau, số nguyên lớn nhất không thể biểu diễn dưới dạng $a_1x_1+a_2x_2~(x_1,x_2\in\mathbf N)$ là $C=a_1a_2-a_1-a_2$. Hơn nữa, với mọi $k\in\mathbf Z$, trong hai số $k$ và $C-k$ có đúng một số biểu diễn được.

Gọi số có thể biểu diễn dưới dạng $a_1x_1+a_2x_2$ (với $x_i\in\mathbf N$) là **có thể biểu diễn**.

??? note "Chứng minh 1"
    Vì $a_1,a_2$ nguyên tố cùng nhau, phương trình $a_1x_1+a_2x_2=k$ luôn có nghiệm nguyên và nghiệm tổng quát
    
    $$
    (x_1,x_2) = (x_1^\circ + ta_2, x_2^\circ - ta_1).\quad(t\in\mathbf Z)
    $$
    
    Chọn $t$ là thương của $x_2^\circ$ khi chia cho $a_1$, khi đó $x_2=x_2^\circ-ta_1$ nằm trong $[0,a_1-1]$. Vì $x_2$ là nhỏ nhất không âm, $k$ biểu diễn được khi và chỉ khi $x_1\ge 0$.
    
    **Bước 1**: Chứng minh mọi $k>C$ đều biểu diễn được.
    
    Với $k>C$:
    
    $$
    a_1x_1 = k - a_2x_2 > C - a_2(a_1-1) = -a_1.
    $$
    
    Suy ra $x_1>-1$ nên $x_1\ge 0$. Do đó $k$ biểu diễn được.
    
    **Bước 2**: Chứng minh $C$ không biểu diễn được, suy ra $C$ là lớn nhất, và $k$ và $C-k$ không thể cùng biểu diễn được.
    
    Giả sử $C$ biểu diễn được, tồn tại $x_1,x_2\in\mathbf N$ sao cho $a_1x_1+a_2x_2=C$. Thay $C$:
    
    $$
    a_1a_2 = a_1(x_1+1) + a_2(x_2+1).
    $$
    
    Suy ra $a_2\mid (x_1+1)$ và $a_1\mid (x_2+1)$, nên
    
    $$
    a_1a_2 \ge a_1a_2 + a_2a_1 = 2a_1a_2,
    $$
    
    mâu thuẫn. Vậy $C$ không biểu diễn được. Nếu $k$ và $C-k$ cùng biểu diễn được thì cộng hai biểu diễn sẽ cho biểu diễn của $C$, mâu thuẫn. Do đó tối đa một số biểu diễn được.
    
    **Bước 3**: Nếu $k$ không biểu diễn được thì $C-k$ biểu diễn được.
    
    Gọi $(x_1,x_2)$ là nghiệm nguyên của $a_1x_1+a_2x_2=k$. Khi $k$ không biểu diễn được thì $x_1<0$. Khi đó
    
    $$
    C - k = a_1a_2 - a_1 - a_2 - a_1x_1 - a_2x_2 = a_1(-1-x_1) + a_2(a_1-1-x_2).
    $$
    
    Với $-1-x_1$ và $a_1-1-x_2$ không âm, nên $C-k$ biểu diễn được.

??? note "Chứng minh 2"
    Ở đây chỉ chứng minh $C=a_1a_2-a_1-a_2$ là số tự nhiên lớn nhất không biểu diễn được; phần còn lại tương tự chứng minh 1.
    
    Xét modulo $a_2$: trong mỗi lớp dư, số tự nhiên nhỏ nhất có thể biểu diễn là duy nhất. Vì các số trong cùng lớp dư sai khác bội $a_2$, khi tìm số nhỏ nhất chỉ cần xét cộng/trừ $a_1$. Do $a_1$ và $a_2$ nguyên tố cùng nhau, số nhỏ nhất trong mỗi lớp dư là bội của $a_1$:
    
    $$
    0,~a_1,~2a_1,~\cdots,~(a_2-1)a_1.
    $$
    
    Vậy số lớn nhất không biểu diễn được là
    
    $$
    \max_{0\le i < a_2} ia_1 - a_2 = (a_2-1)a_1 - a_2 = C.
    $$

### Ý nghĩa hình học

Xem $a_1x_1+a_2x_2=k$ là một đường thẳng. Khi đó $k$ biểu diễn được khi và chỉ khi đường thẳng đi qua một điểm nguyên trong góc phần tư thứ nhất (kể cả trục). Khi $k<ab$, đường thẳng đi qua tối đa một điểm nguyên. Do đó với $0\le k<ab$, $k$ biểu diễn được khi và chỉ khi có đúng một điểm nguyên.

Vì thế, số lượng $k<ab$ có thể biểu diễn bằng số điểm nguyên dưới đường $a_1x_1+a_2x_2=k$ (kể cả biên). Con số này bằng

$$
\sum_{i=0}^{\lfloor k / a_1 \rfloor} \left\lfloor\dfrac{k-ia_1}{a_2}\right\rfloor.
$$

Đây là bài toán đếm điểm nguyên dưới đường thẳng, có thể giải bằng [thuật toán kiểu Euclid](./euclidean.md#类欧几里得算法) trong $O(\log\min\{a_1,a_2,k\})$.

### Bài tập

-   [Luogu P3951 NOIP2017 提高组 小凯的疑惑/蓝桥杯 2013 省 买不到的数目](https://www.luogu.com.cn/problem/P3951)
