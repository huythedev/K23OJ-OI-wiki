author: iamtwz, Chrogeek, Enter-tainer, StudyingFather, aofall, CCXXXI, CoelacanthusHex, frank-xjh, Great-designer, greyqz, guodong2005, henrytbtrue, Ir1d, kZime, lihaoyu1234, Marcythm, MegaOwIer, Menci, nalemy, orzAtalod, ouuan, Persdre, segment-tree, ShaoChenHeng, shuzhouliu, sshwy, Struggler-q, Tiphereth-A, TrisolarisHD, Xeonacid, yuhuoji

## Định nghĩa

Hàm Euler (Euler's totient function), ký hiệu $\varphi(n)$, biểu thị số lượng các số nhỏ hơn hoặc bằng $n$ và nguyên tố cùng nhau với $n$.

Ví dụ $\varphi(1) = 1$.

Khi $n$ là số nguyên tố, hiển nhiên $\varphi(n) = n - 1$.

## Tính chất

-   Hàm Euler là [hàm nhân tính](./basic.md#积性函数).

    Tức với mọi số nguyên $a,b$ thỏa $\gcd(a, b) = 1$ thì có $\varphi(ab) = \varphi(a)\varphi(b)$.

    Đặc biệt, khi $n$ là số lẻ thì $\varphi(2n) = \varphi(n)$.

    Chứng minh xem tại [phép hợp của hệ dư](./basic.md#剩余系的复合).

-   $n = \sum_{d \mid n}{\varphi(d)}$.

    ???+ note "Chứng minh"
        Dựa vào kiến thức liên quan về [nghịch đảo Möbius](./mobius.md) có thể suy ra.

        Cũng có thể xét như sau: nếu $\gcd(k, n) = d$ thì $\gcd(\dfrac{k}{d},\dfrac{n}{d}) = 1, ( k < n )$.

        Nếu đặt $f(x)$ là số lượng các $k$ thỏa $\gcd(k, n) = x$ thì $n = \sum_{i = 1}^n{f(i)}$.

        Theo lập luận trên, ta có $f(x) = \varphi(\dfrac{n}{x})$, do đó $n = \sum_{d \mid n}\varphi(\dfrac{n}{d})$. Lưu ý rằng ước $d$ và $\dfrac{n}{d}$ đối xứng với nhau, nên công thức trở thành $n = \sum_{d \mid n}\varphi(d)$.

-   Nếu $n = p^k$ với $p$ là số nguyên tố thì $\varphi(n) = p^k - p^{k - 1}$.
    (Theo định nghĩa có thể suy ra)

-   Theo định lý phân tích duy nhất, đặt $n = \prod_{i=1}^{s}p_i^{k_i}$, trong đó $p_i$ là số nguyên tố, ta có $\varphi(n) = n \times \prod_{i = 1}^s{\dfrac{p_i - 1}{p_i}}$.

    ???+ note "Chứng minh"
        -   Bổ đề: với mọi số nguyên tố $p$ thì $\varphi(p^k)=p^{k-1}\times(p-1)$.

            Chứng minh: rõ ràng trong các số từ $1$ đến $p^k$, trừ $p^{k-1}$ bội số của $p$ thì các số còn lại đều nguyên tố cùng nhau với $p^k$, do đó $\varphi(p^k)=p^k-p^{k-1}=p^{k-1}\times(p-1)$, chứng minh xong.

        Tiếp theo chứng minh $\varphi(n) = n \times \prod_{i = 1}^s{\dfrac{p_i - 1}{p_i}}$. Từ định lý phân tích duy nhất và tính nhân của hàm $\varphi(x)$

        $$
        \begin{aligned}
            \varphi(n) &= \prod_{i=1}^{s} \varphi(p_i^{k_i}) \\
            &= \prod_{i=1}^{s} (p_i-1)\times {p_i}^{k_i-1}\\
            &=\prod_{i=1}^{s} {p_i}^{k_i} \times(1 - \frac{1}{p_i})\\
            &=n~ \prod_{i=1}^{s} (1- \frac{1}{p_i})
            &\square
        \end{aligned}
        $$

-   Với mọi số nguyên $m,n$ không đồng thời bằng $0$, $\varphi(mn)\varphi(\gcd(m,n))=\varphi(m)\varphi(n)\gcd(m,n)$.

    Có thể suy ra trực tiếp từ tính chất trước.

## Cài đặt

Nếu chỉ cần giá trị hàm Euler của một số, thì chỉ cần phân tích thừa số nguyên tố theo định nghĩa và đồng thời tính. Quá trình này có thể được tối ưu bằng thuật toán [Pollard Rho](./pollard-rho.md).

???+ note "Cài đặt tham khảo"
    === "C++"
        ```cpp
        #include <cmath>
        
        int euler_phi(int n) {
          int ans = n;
          for (int i = 2; i * i <= n; i++)
            if (n % i == 0) {
              ans = ans / i * (i - 1);
              while (n % i == 0) n /= i;
            }
          if (n > 1) ans = ans / n * (n - 1);
          return ans;
        }
        ```
    
    === "Python"
        ```python
        import math
        
        
        def euler_phi(n):
            ans = n
            for i in range(2, math.isqrt(n) + 1):
                if n % i == 0:
                    ans = ans // i * (i - 1)
                    while n % i == 0:
                        n = n // i
            if n > 1:
                ans = ans // n * (n - 1)
            return ans
        ```

Nếu cần giá trị hàm Euler của nhiều số, có thể dùng phương pháp sàng tuyến tính sẽ được nhắc ở phần sau để tính.

Xem thêm: [Sàng để tính hàm Euler](./sieve.md#筛法求欧拉函数)

## Ứng dụng

Hàm Euler thường được dùng để rút gọn tổng của một dãy ước chung lớn nhất. Một số bài viết trong nước gọi nó là **nghịch đảo Euler**[^1].

Trong hệ thức

$$
n=\sum_{d|n}\varphi(d)
$$

thay $n=\gcd(a,b)$, ta có

$$
\gcd(a,b) = \sum_{d|\gcd(a,b)}\varphi(d) = \sum_d [d|a][d|b]\varphi(d),
$$

trong đó $[\cdot]$ là ký hiệu Iverson. Lấy tổng hai vế, ta thu được

$$
\sum_{i=1}^n\gcd(i,n)=\sum_{d}\sum_{i=1}^n[d|i][d|n]\varphi(d)=\sum_d\left\lfloor\frac{n}{d}\right\rfloor[d|n]\varphi(d)=\sum_{d|n}\left\lfloor\frac{n}{d}\right\rfloor\varphi(d).
$$

Quan sát then chốt ở đây là $\sum_{i=1}^n[d|i]=\lfloor\frac{n}{d}\rfloor$, tức số lượng các $i$ giữa $1$ và $n$ chia hết cho $d$ là $\lfloor\frac{n}{d}\rfloor$.

Dựa vào công thức này, ta có thể duyệt các ước và cộng. Khi có nhiều truy vấn, có thể tiền xử lý prefix sum của hàm Euler, dùng phân khối số học để trả lời.

???+ note "[GCD SUM](https://www.luogu.com.cn/problem/P2398)"
    Cho $n\le 100000$, tính
    
    $$
    \sum_{i=1}^n\sum_{j=1}^n\gcd(i,j).
    $$
    
    ??? note "Ý tưởng"
        Tương tự phép biến đổi trên, ta có
        
        $$
        \sum_{i=1}^n\sum_{j=1}^n\gcd(i,j) = \sum_{d=1}^n\left\lfloor\frac{n}{d}\right\rfloor^2\varphi(d).
        $$
        
        Khi đó cần tính hàm Euler từ $1$ đến $n$; dùng sàng tuyến tính là có thể đạt $O(n)$ để ra đáp án.

## Định lý Euler

Một định lý liên hệ chặt chẽ với hàm Euler là định lý Euler, phát biểu như sau:

Nếu $\gcd(a, m) = 1$ thì $a^{\varphi(m)} \equiv 1 \pmod{m}$.

### Định lý Euler mở rộng

Cũng có định lý Euler mở rộng, dùng để xử lý trường hợp tổng quát của $a$ và $m$.

$$
a^b\equiv
\begin{cases}
a^{b\bmod\varphi(m)},\,&\gcd(a,\,m)=1\\
a^b,&\gcd(a,\,m)\ne1,\,b<\varphi(m)\\
a^{b\bmod\varphi(m)+\varphi(m)},&\gcd(a,\,m)\ne1,\,b\ge\varphi(m)
\end{cases}
\pmod m
$$

Chứng minh và bài tập xem tại [Định lý Euler](./fermat.md).

## Bài tập

-   [SPOJ ETF. Euler Totient Function](http://www.spoj.com/problems/ETF/)
-   [UVa 10179. Irreducible Basic Fractions](http://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1120)
-   [UVa 10299. Relatives](http://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1240)
-   [UVa 11327. Enumerating Rational Numbers](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2302)
-   [TIMUS 1673. Admission to Exam](http://acm.timus.ru/problem.aspx?space=1&num=1673)
-   [Luogu P1390 公约数的和](https://www.luogu.com.cn/problem/P1390)
-   [Luogu P2155 \[SDOI2008\] 沙拉公主的困惑](https://www.luogu.com.cn/problem/P2155)
-   [Luogu P2568 GCD](https://www.luogu.com.cn/problem/P2568)

## Tài liệu tham khảo và chú thích

[^1]: Cách gọi này không thấy trong các tạp chí học thuật hoặc diễn đàn nước ngoài; khi dùng cần lưu ý.
