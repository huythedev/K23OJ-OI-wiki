## Giới thiệu

> Bài toán “vật không biết số”: có vật không biết số lượng, chia 3 dư 2, chia 5 dư 3, chia 7 dư 2. Hỏi có bao nhiêu?

Tức tìm số nguyên thỏa: chia 3 dư 2, chia 5 dư 3, chia 7 dư 2.

Bài toán xuất hiện sớm trong 《孙子算经》, có lời giải cụ thể. Nhà toán học Tần Cửu Thiều thời Tống trong 《数书九章》 (1247) đã giải hệ thống bài này. Câu đố ghi nhớ lời giải do Trình Đại Vị thời Minh trong 《算法统宗》:

> 三人同行七十希，五树梅花廿一支，七子团圆正半月，除百零五便得知．

$2\times 70+3\times 21+2\times 15=233=2\times 105+23$ nên đáp án là $23$.

## Định nghĩa

Định lý Trung Quốc (Chinese Remainder Theorem, CRT) giải hệ phương trình đồng dư tuyến tính một ẩn (với $n_1,\dots,n_k$ đôi một nguyên tố cùng nhau):

$$
\begin{cases}
x &\equiv a_1 \pmod {n_1} \\
x &\equiv a_2 \pmod {n_2} \\
  &\vdots \\
x &\equiv a_k \pmod {n_k} \\
\end{cases}
$$

Bài toán “vật không biết số” là một ví dụ.

## Quy trình

1.  Tính tích các mô-đun $n$;
2.  Với phương trình thứ $i$:
    1.  Tính $m_i=\frac{n}{n_i}$;
    2.  Tính [nghịch đảo](./inverse.md) $m_i^{-1}$ modulo $n_i$;
    3.  Tính $c_i=m_im_i^{-1}$ (**không lấy mod $n_i$**) .
3.  Nghiệm duy nhất modulo $n$: $x=\sum_{i=1}^k a_ic_i \pmod n$.

## Hiện thực

=== "C++"
    ```cpp
    LL CRT(int k, LL* a, LL* r) {
      LL n = 1, ans = 0;
      for (int i = 1; i <= k; i++) n = n * r[i];
      for (int i = 1; i <= k; i++) {
        LL m = n / r[i], b, y;
        exgcd(m, r[i], b, y);  // b * m mod r[i] = 1
        ans = (ans + a[i] * m * b % n) % n;
      }
      return (ans % n + n) % n;
    }
    ```

=== "Python"
    ```python
    def CRT(k, a, r):
        n = 1
        ans = 0
        for i in range(1, k + 1):
            n = n * r[i]
        for i in range(1, k + 1):
            m = n // r[i]
            b = y = 0
            exgcd(m, r[i], b, y)  # b * m mod r[i] = 1
            ans = (ans + a[i] * m * b % n) % n
        return (ans % n + n) % n
    ```

## Chứng minh

Cần chứng minh $x$ thỏa $x\equiv a_i \pmod {n_i}$ với mọi $i$.

Với $i\neq j$, có $m_j \equiv 0 \pmod {n_i}$, nên $c_j \equiv 0 \pmod {n_i}$. Và $c_i \equiv 1 \pmod {n_i}$. Do đó:

$$
\begin{aligned}
x&\equiv \sum_{j=1}^k a_jc_j                      &\pmod {n_i} \\
 &\equiv a_ic_i                                   &\pmod {n_i} \\
 &\equiv a_i \cdot m_i \cdot (m^{-1}_i \bmod n_i) &\pmod {n_i} \\
 &\equiv a_i                                      &\pmod {n_i}
\end{aligned}
$$

Vì không hạn chế $a_i$, mọi bộ $\{a_i\}$ đều có nghiệm $x$. Nếu $x\neq y$ thì tồn tại $i$ sao cho $x\not\equiv y\pmod{n_i}$. Do đó nghiệm là duy nhất modulo $n$.

## Minh họa

Giải bài “vật không biết số”:

1.  $n=3\times 5\times 7=105$;
2.  三人同行 **七十** 希: $n_1=3, m_1=35, m_1^{-1}\equiv 2\pmod 3$, nên $c_1=70$;
3.  五树梅花 **廿一** 支: $n_2=5, m_2=21, m_2^{-1}\equiv 1\pmod 5$, nên $c_2=21$;
4.  七子团圆正 **半月**: $n_3=7, m_3=15, m_3^{-1}\equiv 1\pmod 7$, nên $c_3=15$;
5.  Nghiệm: $x\equiv 2\times 70+3\times 21+2\times 15\equiv 233\equiv 23 \pmod {105}$ (除 **百零五** 便得知).

## Thuật toán Garner

CRT còn dùng để biểu diễn số lớn bằng nhiều mô-đun nhỏ.

Ví dụ, nếu $a$ thỏa

$$
\begin{cases}
a &\equiv a_1 \pmod {p_1} \\
a &\equiv a_2 \pmod {p_2} \\
  &\vdots \\
a &\equiv a_k \pmod {p_k} \\
\end{cases}
$$

và $a < \prod_{i=1}^k p_i$ (với $p_i$ nguyên tố), ta có biểu diễn cơ số hỗn hợp:

$$
a = x_1 + x_2 p_1 + x_3 p_1 p_2 + \ldots + x_k p_1 \ldots p_{k-1}
$$

**Thuật toán Garner** tính các $x_i$.

Đặt $r_{ij}$ là nghịch đảo của $p_i$ modulo $p_j$:

$$
p_i \cdot r_{i,j} \equiv 1 \pmod{p_j}
$$

Từ phương trình đầu:

$$
a_1 \equiv x_1 \pmod{p_1}
$$

Phương trình thứ hai:

$$
a_2 \equiv x_1 + x_2 p_1 \pmod{p_2}
$$

Trừ $x_1$, chia $p_1$:

$$
\begin{aligned}
    a_2 - x_1           &\equiv x_2 p_1             &\pmod{p_2} \\
    (a_2 - x_1) r_{1,2} &\equiv x_2                 &\pmod{p_2} \\
    x_2                 &\equiv (a_2 - x_1) r_{1,2} &\pmod{p_2}
\end{aligned}
$$

Tương tự:

$$
x_k=(\dots((a_k-x_1)r_{1,k}-x_2)r_{2,k})-\dots)r_{k-1,k} \bmod p_k
$$

??? note "Hiện thực"
    === "C++"
        ```cpp
        for (int i = 0; i < k; ++i) {
          x[i] = a[i];
          for (int j = 0; j < i; ++j) {
            x[i] = r[j][i] * (x[i] - x[j]);
            x[i] = x[i] % p[i];
            if (x[i] < 0) x[i] += p[i];
          }
        }
        ```
    
    === "Python"
        ```python
        for i in range(0, k):
            x[i] = a[i]
            for j in range(0, i):
                x[i] = r[j][i] * (x[i] - x[j])
                x[i] = x[i] % p[i]
                if x[i] < 0:
                    x[i] = x[i] + p[i]
        ```

Độ phức tạp $O(k^2)$. Garner không yêu cầu mô-đun là nguyên tố, chỉ cần đôi một nguyên tố cùng nhau. Mã giả:

$$
\begin{array}{ll}
&\textbf{Chinese Remainder Algorithm }\operatorname{cra}(\mathbf{v}, \mathbf{m})\text{:} \\
&\textbf{Input}\text{: }\mathbf{m}=(m_0,m_1,\dots ,m_{n-1})\text{, }m_i\in\mathbb{Z}^+\land\gcd(m_i,m_j)=1\text{ for all } i\neq j\text{,} \\
&\qquad \mathbf{v}=(v_0,\dots ,v_{n-1}) \text{ where }v_i=x\bmod m_i\text{.} \\
&\textbf{Output}\text{: }x\bmod{\prod_{i=0}^{n-1} m_i}\text{.} \\
1&\qquad \textbf{for }i\text{ from }1\text{ to }(n-1)\textbf{ do} \\
2&\qquad \qquad C_i\gets \left(\prod_{j=0}^{i-1}m_j\right)^{-1}\bmod{m_i} \\
3&\qquad x\gets v_0 \\
4&\qquad \textbf{for }i\text{ from }1\text{ to }(n-1)\textbf{ do} \\
5&\qquad \qquad u\gets (v_i-x)\cdot C_i\bmod{m_i} \\
6&\qquad \qquad x\gets x+u\prod_{j=0}^{i-1}m_j \\
7&\qquad \textbf{return }(x)
\end{array}
$$

Dòng 6 chính là biểu diễn cơ số hỗn hợp.

## Ứng dụng

Một số bài toán cho mô-đun **không phải số nguyên tố**, nhưng phân tích nhân tử cho thấy không có bình phương, tức là tích các số nguyên tố khác nhau. Khi đó có thể giải riêng từng mô-đun rồi dùng CRT ghép.

Ví dụ:

???+ note "[洛谷 P2480 \[SDOI2010\] 古代猪文](https://www.luogu.com.cn/problem/P2480)"
    Cho $G,n$ ($1 \leq G,n \leq 10^9$), tính
    
    $$
    G^{\sum_{k\mid n}\binom{n}{k}} \bmod 999~911~659
    $$

Nếu $G=999~911~659$ thì kết quả là $0$.

Ngược lại, theo [định lý Euler](./fermat.md):

$$
G^{\sum_{k\mid n}\binom{n}{k} \bmod 999~911~658} \bmod 999~911~659
$$

Cần tính

$$
\sum_{k\mid n}\binom{n}{k} \bmod 999~911~658
$$

Vì $999~911~658$ không nguyên tố, không thể đảm bảo nghịch đảo cho mọi $x$. Ta có $999~911~658=2 \times 3 \times 4679 \times 35617$, mỗi thừa số xuất hiện bậc một. Vì vậy tính tổng theo từng mô-đun $2,3,4679,35617$ rồi ghép bằng CRT.

Tức giải hệ:

$$
\begin{cases}
x \equiv a_1 \pmod 2\\
x \equiv a_2 \pmod 3\\
x \equiv a_3 \pmod {4679}\\
x \equiv a_4 \pmod {35617}
\end{cases}
$$

Với mô-đun nguyên tố nhỏ, có thể tính $\binom{n}{k}$ bằng [định lý Lucas](./lucas.md).

## Mở rộng: mô-đun không nguyên tố cùng nhau

### Hai phương trình

Cho $x\equiv a_1 \pmod {m_1}$, $x\equiv a_2 \pmod {m_2}$.

Viết $x=m_1p+a_1=m_2q+a_2$ với $p,q$ nguyên, suy ra $m_1p-m_2q=a_2-a_1$.

Theo [Bézout](./bezouts.md), nếu $a_2-a_1$ không chia hết cho $\gcd(m_1,m_2)$ thì vô nghiệm.

Nếu có nghiệm, dùng [Euclid mở rộng](./gcd.md) tìm $(p,q)$, rồi suy ra nghiệm $x\equiv b\pmod M$ với $b=m_1p+a_1$, $M=\mathrm{lcm}(m_1,m_2)$.

### Nhiều phương trình

Gộp từng cặp theo cách trên.

## Bài tập

-   [【模板】中国剩余定理（CRT）/曹冲养猪](https://www.luogu.com.cn/problem/P1495)
-   [【模板】扩展中国剩余定理](https://www.luogu.com.cn/problem/P4777)
-   [「NOI2018」屠龙勇士](https://uoj.ac/problem/396)
-   [「TJOI2009」猜数字](https://www.luogu.com.cn/problem/P3868)

**Một phần nội dung dịch từ [Китайская теорема об остатках](http://e-maxx.ru/algo/chinese_theorem) và bản tiếng Anh [Chinese Remainder Theorem](https://cp-algorithms.com/algebra/chinese-remainder-theorem.html). Bản tiếng Nga: Public Domain + Leave a Link; bản tiếng Anh: CC-BY-SA 4.0.**
