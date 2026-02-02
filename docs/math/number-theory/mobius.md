author: hydingsy, hyp1231, ranwen, 383494

Kiến thức tiền đề: [Phân khối số học](./sqrt-decomposition.md)、[Tích Dirichlet](./dirichlet.md#dirichlet-卷积)

Molbius inversion là nội dung quan trọng trong số học．Với một số hàm $f(n)$, nếu khó tính trực tiếp giá trị của nó mà lại dễ tính tổng theo bội hoặc tổng theo ước $g(n)$, thì có thể dùng phép đảo Möbius để đơn giản hóa tính toán và suy ra $f(n)$．

## Hàm Möbius

Hàm Möbius (Möbius function) được định nghĩa là

$$
\mu(n)=
\begin{cases}
1,&n=1,\\
0,&n\text{ chia hết cho một số chính phương }>1,\\
(-1)^k,&n\text{ là tích của }k\text{ số nguyên tố phân biệt}.\\
\end{cases}
$$

Cụ thể, giả sử số nguyên dương $n$ có phân tích thừa số nguyên tố $n=\prod_{i=1}^kp_i^{e_i}$, trong đó $p_i$ là số nguyên tố, $e_i$ là số nguyên dương．Thì ba trường hợp tương ứng là:

1.  $\mu(1) = 1$；
2.  Khi tồn tại $i$ sao cho $e_i > 1$, tức là tồn tại bất kỳ thừa số nguyên tố nào xuất hiện quá một lần, thì $\mu(n)=0$；
3.  Ngược lại, với mọi $i$ đều có $e_i = 1$, tức là mọi thừa số nguyên tố đều chỉ xuất hiện một lần, thì $\mu(n)=(-1)^k$, trong đó $k$ là số lượng thừa số nguyên tố phân biệt.

### Tính chất

Theo định nghĩa, hàm Möbius $\mu(n)$ là hàm tích tính, nhưng không phải là hàm tích hoàn toàn. Ngoài ra, tính chất quan trọng nhất là đẳng thức sau:

???+ note "Tính chất"
    Với số nguyên dương $n$,
    
    $$
    \sum_{d\mid n}\mu(d) = [n = 1] =
    \begin{cases}
    1,&n=1,\\
    0,&n\neq 1.\\
    \end{cases}
    $$
    
    Trong đó $[\cdot]$ là Iverson bracket.

??? note "Chứng minh"
    Giả sử $n=\prod_{i=1}^kp_i^{e_i}$, đặt $n' = \prod_{i=1}^kp_i$．Theo [hệ thức nhị thức](../combinatorics/combination.md#hệ-thức-nhị-thức), ta có
    
    $$
    \sum_{d\mid n}\mu(d) = \sum_{d\mid n'}\mu(d) = \sum_{i=0}^k\binom{k}{i}(-1)^i = (1 + (-1))^k = [k = 0] = [n = 1].
    $$

Dùng tích Dirichlet, biểu thức này có thể viết thành $\varepsilon = 1 * \mu$．Nói cách khác, hàm Möbius là Dirichlet inverse của hàm hằng $1$.

Điều này có một ứng dụng rất phổ biến:

$$
[i\perp j] = [\gcd(i,j) = 1] = \sum_{d\mid\gcd(i,j)} \mu(d) = \sum_{d}[d\mid i][d\mid j]\mu(d).
$$

Nó chuyển đổi điều kiện nguyên tố thành biểu thức tổng về hàm Möbius, thuận tiện cho việc suy luận tiếp theo.

### Cách tính

Nếu cần tính giá trị của hàm Möbius $\mu(n)$ cho một số $n$, có thể dùng phân tích thừa số nguyên tố. Ví dụ, khi $n$ không quá lớn, có thể tính được $\mu(n)$ trong $O(\sqrt{n})$ thời gian.

???+ example "Tham khảo thực hiện"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/mobius/mobius-func-1.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/mobius/mobius-func-1.py:core"
        ```

Nếu cần tính trước $n$ số nguyên dương, có thể dùng tính tích tính của hàm, bằng cách dùng [sieve](./sieve.md#sieve-method-for-mobius-function) trong $O(n)$ thời gian.

???+ example "Tham khảo thực hiện"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/mobius/mobius-func-2.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/mobius/mobius-func-2.py:core"
        ```

## Đảo Molbius

Molbius inversion là ứng dụng quan trọng nhất của hàm Möbius.

???+ note "Đảo Molbius"
    Giả sử $f(n),g(n)$ là hai hàm số học. Thì
    
    $$
    f(n) = \sum_{d\mid n}g(d) \iff g(n) = \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)f(d).
    $$

??? note "Chứng minh một"
    Trực tiếp chứng minh, ta có:
    
    $$
    \begin{aligned}
    \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)f(d)
    &= \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)\sum_{k\mid d}g(k)\\
    &= \sum_{k\mid n}g(k)\sum_d[k\mid d\mid n]\mu\left(\dfrac{n}{d}\right)\\
    &= \sum_{k\mid n}g(k)\sum_{d\mid n}\left[\frac{n}{d}\mid\frac{n}{k}\right]\mu\left(\dfrac{n}{d}\right)\\
    &= \sum_{k\mid n}g(k)\left[\frac{n}{k} = 1\right] \\
    &= g(n).
    \end{aligned}
    $$
    
    Chuyển đổi quan trọng nhất là đổi thứ tự tổng, và nhận thấy rằng $k\mid d\mid n$ tương đương với $\dfrac{n}{d}\mid\dfrac{n}{k}$．Điểm thứ hai tương đương với tổng các hàm Möbius tại vị trí $\dfrac{n}{d}$, nên bằng $\left[\dfrac{n}{k} = 1\right]$．Biểu thức này chỉ không bằng $0$ tại $n=k$ cuối cùng sẽ được $g(n)$.

??? note "Chứng minh hai"
    Dùng tích Dirichlet, mệnh đề tương đương với
    
    $$
    f = 1 * g \iff g = \mu * f.
    $$
    
    Dùng $1 * \mu = \varepsilon$ , thực hiện tích với $\mu$ ở cả hai bên của đẳng thức, ta được
    
    $$
    f * \mu = (1 * g) * \mu = (1 * \mu) * g = \varepsilon * g = g.
    $$

Trong các bài toán liên quan đến các mối quan hệ chia hết, Molbius inversion là công cụ mạnh mẽ.

???+ example "Ví dụ"
    1.  [Hàm Euler](./euler-totient.md) $\varphi(n)$ thỏa mãn mối quan hệ $n = \sum_{d\mid n}\varphi(d)$, tức là $\mathrm{id}=1*\varphi$．Đối nó thực hiện đảo, ta được $\varphi = \mu * \mathrm{id}$, tức là
    
        $$
        \varphi(n) = \sum_{d\mid n}d\mu\left(\dfrac{n}{d}\right).
        $$
    2.  Hàm chia số $\sigma_k(n) = \sum_{d\mid n}d^k$ , tức là $\sigma_k = 1 * \mathrm{id}_k$．Đối nó thực hiện đảo, ta được $\mathrm{id}_k = \mu * \sigma_k$ , tức là
    
        $$
        n^k = \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)\sigma_k(d).
        $$
    3.  Hàm số lượng thừa số nguyên tố phân biệt $\omega(n)=\sum_{d\mid n}[d\in\mathbf P]$ , tức là $\omega = 1* \mathbf{1}_{\mathbf P}$, trong đó $\mathbf{1}_{\mathbf P}$ là hàm chỉ thị tập số nguyên tố $\mathbf P$．Đối nó thực hiện đảo, ta được $\mathbf{1}_{\mathbf P} = \mu * \omega$ , tức là
    
        $$
        [n\in\mathbf P] = \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)\omega(d).
        $$
    4.  Xét hàm số thỏa mãn $\log n = \sum_{d\mid n}\Lambda(d)$, nó chính là hàm số học của logarit, còn gọi là von Mangoldt function:
    
        $$
        \Lambda(n) = \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)\log d = 
        \begin{cases}
        \log p, & n = p^e,~p\in\mathbf P,~e\in\mathbf N_+, \\
        0, &\text{otherwise}.
        \end{cases}
        $$

??? note "Phụ chú: $\Lambda(n)$ biểu thức chứng minh"
    Với số nguyên tố lũy thừa $n=p^e~(e\in\mathbf N_+)$, ta có
    
    $$
    \Lambda(n) = \sum_{i=0}^e\mu(p^{e-i})\log p^i = \log p^{e} - \log p^{e-1} = \log p.
    $$
    
    Với $n=1$, rõ ràng có $\Lambda(n)=\log 1=0$．Với các hợp số khác $n$, ta có
    
    $$
    \Lambda(n) = \sum_{d\mid n}\mu(d)(\log n-\log d) = \left(\sum_{d\mid n}\mu(d)\right)\log n-\sum_{d\mid n}\mu(d)\log d.
    $$
    
    Theo tính chất của hàm Möbius, hệ số của $\log n$ là $[n=1]=0$．Với phần sau, có thể phân tích $d$ thành tích các thừa số nguyên tố．Với bất kỳ số nguyên tố $p\mid n$, xét hệ số của $\log p$, đều có:
    
    $$
    -\sum_{p\mid d\mid n}\mu(d) = \sum_{(d/p)\mid(n/p)}\mu\left(\dfrac{d}{p}\right) = \left[\dfrac{n}{p}=1\right]=0.
    $$
    
    Như vậy, với các hợp số có nhiều hơn một thừa số nguyên tố, đều có $\Lambda(n)=0$.

### Dạng mở rộng

Ngoài dạng cơ bản, Molbius inversion còn có một số dạng mở rộng phổ biến. Trước hết, có thể xem xét dạng bội.

???+ note "Dạng mở rộng một"
    Giả sử $f(n),g(n)$ là hai hàm số học. Thì
    
    $$
    f(n) = \sum_{n\mid d}g(d) \iff g(n) = \sum_{n\mid d}\mu\left(\dfrac{d}{n}\right)f(d).
    $$

??? note "Chứng minh"
    Trực tiếp chứng minh, ta có:
    
    $$
    \begin{aligned}
    \sum_{n\mid d}\mu\left(\dfrac{d}{n}\right)f(d)
    &= \sum_{n\mid d}\mu\left(\dfrac{d}{n}\right)\sum_{d\mid k}g(k)\\
    &= \sum_{n\mid k}g(k)\sum_{d}[n\mid d\mid k]\mu\left(\dfrac{d}{n}\right)\\
    &= \sum_{n\mid k}g(k)\sum_{n\mid d}\left[\dfrac{d}{n}\mid\dfrac{k}{n}\right]\mu\left(\dfrac{d}{n}\right)\\
    &= \sum_{n\mid k}g(k)\left[\dfrac{k}{n}=1\right]\\
    &= g(n).
    \end{aligned}
    $$
    
    Đây là đối xứng hoàn toàn với dạng cơ bản.

Thứ hai, Molbius inversion không chỉ giới hạn ở phép cộng, nó thực ra đúng với mọi [Abel group](../algebra/basic.md#group) trong toán học. Ví dụ, nó có dạng nhân như sau:

???+ note "Dạng mở rộng hai"
    Giả sử $f(n),g(n)$ là hai hàm số học. Thì
    
    $$
    f(n) = \prod_{d\mid n}g(d) \iff g(n) = \prod_{d\mid n}f(d)^{\mu(n/d)}.
    $$

??? note "Chứng minh"
    Trực tiếp chứng minh, ta có:
    
    $$
    \begin{aligned}
    \prod_{d\mid n}f(d)^{\mu(n/d)}
    &= \prod_{d\mid n}\left(\prod_{k\mid d}g(k)\right)^{\mu(n/d)}\\
    &= \prod_{k\mid n}g(k)\uparrow\left(\sum_d[k\mid d\mid n]\mu\left(\dfrac{n}{d}\right)\right)\\
    &= \prod_{k\mid n}g(k)\uparrow\left(\sum_{d\mid n}\left[\frac{n}{d}\mid\frac{n}{k}\right]\mu\left(\dfrac{n}{d}\right)\right)\\
    &= \prod_{k\mid n}g(k)\uparrow\left[\frac{n}{k} = 1\right] \\
    &= g(n).
    \end{aligned}
    $$
    
    Trong đó, $a\uparrow b = a^b$ là Knuth arrow. So sánh với chứng minh dạng cơ bản, khác biệt duy nhất là cộng thay bằng nhân, và nhân thay bằng lấy lũy thừa.

Từ góc nhìn tích Dirichlet, Molbius inversion chỉ dùng đến việc "Möbius function is the Dirichlet inverse of the constant function $1$"．Dễ hình dung, mối quan hệ tương tự như vậy cũng thành lập với các hàm Dirichlet ngược.

???+ note "Dạng mở rộng ba"
    Giả sử $f(n),g(n),\alpha(n)$ đều là hàm số học, và $\alpha^{-1}(n)$ là Dirichlet inverse của $\alpha(n)$, tức là
    
    $$
    [n=1] = \sum_{d\mid n}\alpha\left(\dfrac{n}{d}\right)\alpha^{-1}(d).
    $$
    
    Thì
    
    $$
    f(n) = \sum_{d\mid n}\alpha\left(\dfrac{n}{d}\right)g(d) \iff g(n) = \sum_{d\mid n}\alpha^{-1}\left(\dfrac{n}{d}\right)f(d).
    $$

??? note "Chứng minh"
    Trực tiếp chứng minh, ta có:
    
    $$
    \begin{aligned}
    \sum_{d\mid n}\alpha^{-1}\left(\dfrac{n}{d}\right)f(d)
    &= \sum_{d\mid n}\alpha^{-1}\left(\dfrac{n}{d}\right)\sum_{k\mid d}\alpha\left(\dfrac{d}{k}\right)g(k)\\
    &= \sum_{k\mid n}g(k)\sum_d[k\mid d\mid n]\alpha\left(\dfrac{d}{k}\right)\alpha^{-1}\left(\dfrac{n}{d}\right)\\
    &= \sum_{k\mid n}g(k)\sum_{d\mid n}\left[\frac{n}{d}\mid\frac{n}{k}\right]\alpha\left(\dfrac{d}{k}\right)\alpha^{-1}\left(\dfrac{n/k}{d/k}\right)\\
    &= \sum_{k\mid n}g(k)\left[\frac{n}{k} = 1\right] \\
    &= g(n).
    \end{aligned}
    $$
    
    So sánh với chứng minh dạng cơ bản, chỉ cần thay đổi đẳng thức thứ hai.

???+ note "Định lý"
    Giả sử $f(n),g(n)$ là hàm số học, và $t(n)$ là hàm hoàn toàn tích tính. Thì
    
    $$
    f(n) = \sum_{d\mid n}t\left(\dfrac{n}{d}\right)g(d) \iff g(n) = \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)t\left(\dfrac{n}{d}\right)f(d).
    $$

??? note "Chứng minh"
    Theo tính chất của tích Dirichlet, với hàm hoàn toàn tích tính $t(n)$, Dirichlet inverse của nó là $\mu(n)t(n)$.

Cuối cùng, Molbius inversion có thể mở rộng đến các hàm phức trị trên $[1,+\infty)$, không chỉ giới hạn ở các hàm số học. Dạng cơ bản của Molbius inversion có thể xem là trường hợp đặc biệt của các hàm phức trị tại tất cả các điểm không nguyên.

???+ note "Dạng mở rộng bốn"
    Giả sử $F(x)$ và $G(x)$ đều là các hàm phức trị trên $[1,+\infty)$. Thì
    
    $$
    F(x) = \sum_{n = 1}^{\lfloor x\rfloor}G\left(\dfrac{x}{n}\right) \iff G(x) = \sum_{n = 1}^{\lfloor x\rfloor}\mu(n)F\left(\dfrac{x}{n}\right).
    $$

??? note "Chứng minh"
    Không mất tổng quát, bổ sung định nghĩa, giả sử khi $x < 1$ thì $F(x)=G(x)=0$．Thì mệnh đề tương đương với:
    
    $$
    F(x) = \sum_n G\left(\dfrac{x}{n}\right) \iff G(x) = \sum_n \mu(n)F\left(\dfrac{x}{n}\right).
    $$
    
    Những biểu thức này đều là tổng trên $n\in\mathbf N_+$.
    
    Trực tiếp chứng minh, ta có:
    
    $$
    \begin{aligned}
    \sum_n \mu(n)F\left(\dfrac{x}{n}\right)
    &= \sum_n\mu(n)\sum_d G\left(\dfrac{x/n}{d}\right)\\
    &= \sum_k G\left(\dfrac{x}{k}\right)\sum_{n\mid k}\mu(n)\\
    &= \sum_k G\left(\dfrac{x}{k}\right)[k=1]\\
    &= G(x).
    \end{aligned}
    $$
    
    Trong đó, để được đẳng thức thứ hai, cần đặt $k = nd$.

???+ note "Định lý"
    Giả sử $f(n),g(n)$ là hàm số học. Thì
    
    $$
    f(n) = \sum_{k=1}^ng\left(\left\lfloor\dfrac{n}{k}\right\rfloor\right) \iff g(n)=\sum_{k=1}^n\mu(k)f\left(\left\lfloor\dfrac{n}{k}\right\rfloor\right).
    $$

??? note "Chứng minh"
    Chỉ cần lấy $F(x)=f(\lfloor x\rfloor)$ và $G(x)=g(\lfloor x\rfloor)$.

Những dạng mở rộng này có thể kết hợp với nhau, tạo thành các mối quan hệ phức tạp hơn.

### Dirichlet prefix sum

Kiến thức tiền đề: [Prefix sum and difference](../../basic/prefix-sum.md)

Xét mối quan hệ cơ bản của Molbius inversion:

$$
f(n) = \sum_{d\mid n}g(d) \iff g(n) = \sum_{d\mid n}\mu\left(\dfrac{n}{d}\right)f(d).
$$

Phía trái, $f(n)$ là tổng các giá trị $g(n)$ tại các ước của $n$．Nếu hiểu $a\mid b$ là $a$ đứng trước $b$, thì $f(n)$ có thể hiểu là tổng theo nghĩa nào đó của $g(n)$．Do đó, trong cộng đồng thi đấu, việc tìm ra $\{f(k)\}_{k=1}^n$ từ $\{g(k)\}_{k=1}^n$ gọi là **Dirichlet prefix sum**, tương ứng là Dirichlet difference．Các phương pháp này thường xuất hiện trong các bài toán cần xử lý trước một số hàm số học tại các điểm đầu tiên.

Tiếp theo, thảo luận về cách tính Dirichlet prefix sum. Nếu xem mỗi số nguyên tố là một chiều, đây là một dạng tổng nhiều chiều. Nhớ lại thuật toán [tổng theo chiều](../../basic/prefix-sum.md#tổng-theo-chiều): duyệt từng chiều, và cộng tất cả các giá trị tại mỗi vị trí vào vị trí tiếp theo. Với hàm số học, điều này tương đương với việc duyệt từ nhỏ đến lớn tất cả các số nguyên tố $p$ , và cộng giá trị tại $n$ vào $np$．Đây là thứ tự duyệt của [Eratosthenes sieve](./sieve.md#Eratosthenes sieve). Do đó, thuật toán này có thể tính được tổng Dirichlet của một dãy dài $n$ trong $O(n\log\log n)$ thời gian. Tương tự, dùng tổng theo chiều có thể tính được tổng Dirichlet difference trong cùng thời gian.

???+ example "Tham khảo thực hiện"
    === "Dirichlet prefix sum"
        ```cpp
        --8<-- "docs/math/code/mobius/mobius-func-3.cpp:presum"
        ```
    
    === "Dirichlet difference"
        ```cpp
        --8<-- "docs/math/code/mobius/mobius-func-3.cpp:diff"
        ```

Phương pháp này có thể mở rộng đến dạng bội (dạng mở rộng một), dạng nhân (dạng mở rộng hai), dạng dùng hàm hoàn toàn tích tính thay cho hàm hằng (dạng mở rộng ba), v.v.

## Bài toán

Phần này trình bày các bài toán minh họa cách sử dụng Molbius inversion và một số kỹ thuật biến đổi phổ biến. Trước hết, qua một bài toán quen thuộc để làm quen với kỹ thuật xử lý điều kiện $\gcd$.

???+ example "[Luogu P2522 \[HAOI 2011\] Problem b](https://www.luogu.com.cn/problem/P2522)"
    $T$ bộ dữ liệu. Với mỗi bộ dữ liệu, tính giá trị:
    
    $$
    \sum_{i=x}^{n}\sum_{j=y}^{m}[\gcd(i,j)=k].
    $$
    
    Phạm vi dữ liệu: $1\le T,x,y,n,m,k\le 5\times 10^4$.

??? note "Giải pháp"
    Theo nguyên lý bao hàm, biểu thức này có thể chia thành $4$ phần, mỗi phần có dạng
    
    $$
    f(n,m,k)=\sum_{i=1}^{n}\sum_{j=1}^{m}[\gcd(i,j)=k].
    $$
    
    Với dạng này, tiếp theo là một đoạn quy trình tiêu chuẩn: trích xuất thừa số, dùng tính chất của hàm Möbius, đổi thứ tự tổng.
    
    Trước hết, do $i,j$ chỉ có thể lấy các bội của $k$, có thể đưa ra một thừa số — tương đương với thay thế $i=ki'$ và $j=kj'$, ta được:
    
    $$
    f(n,m,k)=\sum_{i=1}^{\lfloor n/k\rfloor}\sum_{j=1}^{\lfloor m/k\rfloor}[\gcd(i,j)=1].
    $$
    
    Sau đó, dùng tính chất của hàm Möbius:
    
    $$
    [\gcd(i,j)=1] = \sum_{d\mid\gcd(i,j)}\mu(d) = \sum_d[d\mid i][d\mid j]\mu(d).
    $$
    
    Thay vào biểu thức, đổi thứ tự tổng, ta được:
    
    $$
    f(n,m,k)=\sum_d\mu(d)\left(\sum_{i=1}^{\lfloor n/k\rfloor}[d\mid i]\right)\left(\sum_{j=1}^{\lfloor m/k\rfloor}[d\mid j]\right).
    $$
    
    Một đoạn thao tác tốt là, cố định $d$ thì các tổng về $i$ và $j$ tách rời, có thể tính riêng. Tiếp theo, vì
    
    $$
    \sum_{i=1}^{\lfloor n/k\rfloor}[d\mid i] = \left\lfloor\dfrac{\lfloor n/k\rfloor}{d}\right\rfloor,~\sum_{j=1}^{\lfloor m/k\rfloor}[d\mid j]=\left\lfloor\dfrac{\lfloor m/k\rfloor}{d}\right\rfloor,
    $$
    
    nên có
    
    $$
    f(n,m,k)=\sum_d\mu(d)\left\lfloor\dfrac{\lfloor n/k\rfloor}{d}\right\rfloor\left\lfloor\dfrac{\lfloor m/k\rfloor}{d}\right\rfloor.
    $$
    
    Dùng sàng tuyến tính để tính trước $\mu(d)$, và tính trước tổng prefix, có thể dùng số học phân khối để giải. Tổng thời gian phức tạp là $O(N + T\sqrt{N})$ , trong đó $N$ là giới hạn trên của $n,m$, $T$ là số bộ dữ liệu.

??? note "Mã nguồn"
    ```cpp
    --8<-- "docs/math/code/mobius/mobius_1.cpp"
    ```

Tiếp theo, hai bài toán minh họa cách xử lý bằng cách liệt kê các thừa số chung, và dùng [sieve](./sieve.md#generalized-arithmetical-functions) để tính giá trị của các hàm tích tính.

???+ example "[SPOJ LCMSUM](https://www.spoj.com/problems/LCMSUM/)"
    $T$ bộ dữ liệu. Với mỗi bộ dữ liệu, tính giá trị:
    
    $$
    \sum_{i=1}^n \operatorname{lcm}(i,n).
    $$
    
    Phạm vi dữ liệu: $1\le T\le 3\times 10^5,~1\le n\le 10^6$.

??? note "Giải pháp một"
    Đề bài cho là số chia hết, nhưng thường thì số chia hết dễ xử lý hơn. Do đó, đầu tiên làm biến đổi:
    
    $$
    f(n)=\sum_{i=1}^n \operatorname{lcm}(i,n) = \sum_{i=1}^n \frac{i\cdot n}{\gcd(i,n)}.
    $$
    
    Lấy $n$ ra, liệt kê các thừa số chung $k$:
    
    $$
    f(n)=n\sum_{k\mid n}\sum_{i=1}^n\dfrac{i}{k}[\gcd(i,n)=k].
    $$
    
    Với lớp nội, đây là trường hợp phổ biến nhất có chứa điều kiện $\gcd$. Dùng quy trình tiêu chuẩn, ta có:
    
    $$
    \begin{aligned}
    f(n) &= n\sum_{k\mid n}\sum_{i=1}^{n/k}i\left[\gcd\left(i,\dfrac{n}{k}\right)=1\right]\\
    &= n\sum_{k\mid n}\sum_{i=1}^{n/k}i\sum_d\mu(d)[d\mid i]\left[d\mid \dfrac{n}{k}\right]\\
    &= n\sum_{k\mid n}\sum_d\mu(d)\left[d\mid \dfrac{n}{k}\right]\left(\sum_{i=1}^{n/k}i[d\mid i]\right).
    \end{aligned}
    $$
    
    Lại một lần nữa, tổng về $i$ tách rời với các phần khác. Cuối cùng, tổng về $i$ thực chất là một cấp số cộng: (lấy $i=di'$)
    
    $$
    \sum_{i=1}^{n/k}i[d\mid i] = d\frac{1}{2}\left(\dfrac{n}{kd}+1\right)\dfrac{n}{kd}=:dG\left(\dfrac{n}{kd}\right).
    $$
    
    Như vậy, ta có biểu thức:
    
    $$
    f(n) = n\sum_{k\mid n}\sum_d\mu(d)\left[d\mid \dfrac{n}{k}\right]dG\left(\dfrac{n}{kd}\right).
    $$
    
    Sau khi liệt kê các thừa số chung, dạng này rất phổ biến. Có thể xử lý như sau: đặt tích $l=kd$ , rồi đổi thứ tự tổng. Vì $d\mid(n/k)$ tương đương với $d\mid\ell\mid n$ , nên biểu thức biến thành:
    
    $$
    f(n) = n\sum_{\ell\mid n}G\left(\dfrac{n}{\ell}\right)\sum_{d\mid\ell}\mu(d)d.
    $$
    
    Đặt $F(\ell)=\sum_{d\mid\ell}\mu(d)d$ , thì biểu thức có dạng:
    
    $$
    f(n) = n\sum_{\ell\mid n}G\left(\dfrac{n}{\ell}\right)F(\ell).
    $$
    
    Vì $\mu(d)d$ là hàm tích tính, nên nó và hàm hằng $1$ có tích Dirichlet $F(n)$ cũng là hàm tích tính. Mặc dù biểu thức này có dạng tích Dirichlet, nhưng $G(n)$ không phải là hàm tích tính. Tuy nhiên, $G(n)$ là đa thức, nên nó thực chất là tổng của một số hàm hoàn toàn tích tính. Do đó, có
    
    $$
    f(n) = \dfrac{1}{2}n\left(\sum_{\ell}\left(\dfrac{n}{\ell}\right)^2F(\ell) + \sum_{\ell}\dfrac{n}{\ell}F(\ell)\right).
    $$
    
    Hai phần này (không kể hệ số) đều là hàm tích tính, có thể tính trước bằng sàng tuyến tính (hoặc có thể sàng tuyến tính để tính hàm trong lớp, rồi dùng tổng prefix trong $O(N\log\log N)$ thời gian). Cụ thể, đặt
    
    $$
    H_s(n) = \sum_{\ell}\left(\dfrac{n}{\ell}\right)^sF(\ell),~s=1,2.
    $$
    
    Để tìm biểu thức, chỉ cần xác định giá trị tại các số nguyên tố lũy thừa. Với số nguyên tố $p$ và số nguyên dương $e$,
    
    $$
    \begin{aligned}
    F(p^e) &= \mu(1) + \mu(p)p + \sum_{j=2}^e\mu(p^j)p^j = 1-p,\\
    H_s(p^e) &= (p^e)^{s}F(1) + \sum_{j=1}^e(p^{e-j})^sF(p^j) = p^{es} + (1-p)\dfrac{1-p^{es}}{1-p^s},~s=1,2.
    \end{aligned}
    $$
    
    Đặc biệt, $H_1(p^e)\equiv 1$ là hàm hằng, còn
    
    $$
    H_2(p^e) = p^{2e} + (1-p)\dfrac{1-p^{2e}}{1-p^2} = H_2(p^{e-1}) + p^{2e} - p^{2e-1}.
    $$
    
    Như vậy, dễ dàng dùng sàng tuyến tính để giải. Sau khi sàng tuyến tính ra $H_2(n)$, có thể dùng biểu thức $f(n)=(n/2)(H_2(n)+1)$ để tính trong $O(1)$ thời gian. Tổng thời gian phức tạp là $O(N+T)$, trong đó $N$ là giới hạn trên của $n$, $T$ là số bộ dữ liệu.
    
    Tham khảo thực hiện có thể dùng tính chất đặc biệt của biểu thức này, để làm rõ hơn. Chỉ cần xác định giá trị tại các số nguyên tố lũy thừa, vẫn có thể hoàn thành sàng tuyến tính trong $O(N)$ thời gian. Các bước làm rõ hơn được trình bày trong giải pháp hai.

??? note "Giải pháp hai"
    Với bài toán này, có cách xử lý linh hoạt hơn. Từ giải pháp một, ta có
    
    $$
    f(n) = n\sum_{k\mid n}\sum_{i=1}^{n/k}i\left[\gcd\left(i,\dfrac{n}{k}\right)=1\right] = n\sum_{k\mid n}F\left(\dfrac{n}{k}\right).
    $$
    
    Nếu ở bước này không tiếp tục làm Molbius inversion, mà quan sát tổng về $i$ là tổng các số nguyên nhỏ hơn hoặc bằng $d=n/k$ và nguyên tố với $d$. Với $d>1$, vì các số nguyên nguyên tố với $d$ xuất hiện thành cặp, tức là $i$ và $d-i$ đều nguyên tố với $d$, nên có
    
    $$
    F(d)=\sum_{i=1}^{n'}i[i\perp d] = \sum_{i=1}^{d}(d-i)[i\perp d] = \dfrac{1}{2}d\sum_{i=1}^{d}[i\perp d] = \dfrac{1}{2}d\varphi(d).
    $$
    
    Với $d=1$, thì có
    
    $$
    F(d)=1=\dfrac{1}{2}+\dfrac{1}{2}d\varphi(d).
    $$
    
    Tiếp theo, biểu thức có thể viết thành
    
    $$
    f(n) = \dfrac{1}{2}n\left(\sum_{d\mid n}d\varphi(d) + 1\right).
    $$
    
    Vì $G(n)=\sum_{d\mid n}d\varphi(d)$ là tích của hàm tích tính $n\varphi(n)$ và hàm hằng $1$, nên nó cũng là hàm tích tính, có thể dùng sàng tuyến tính để tính trước. Chỉ cần xác định giá trị tại các số nguyên tố lũy thừa. Với số nguyên tố $p$ và số nguyên dương $e$,
    
    $$
    G(p^e) = 1 + \sum_{i=1}^ep^e(p^e-1) = G(p^{e-1}) + p^{2e} - p^{2e-1}.
    $$
    
    Có thể thấy, biểu thức này và giải pháp một là nhất quán. Phương pháp này có tổng thời gian phức tạp vẫn là $O(N+T)$.

Cuối cùng, dùng biểu thức tích tính của hàm, có thể tối ưu hóa quá trình sàng tuyến tính. Với số nguyên tố $p$,
    
    $$
    G(p) = 1 - p + p^2.
    $$
    
    Khóa tuyến tính nằm ở việc tính $G(pn)$ cho các $n$. Điều này phân thành hai trường hợp. Khi $p\perp n$, vì $G$ là hàm tích tính, nên
    
    $$
    G(pn) = G(p)G(n).
    $$
    
    Ngược lại, khi $p\mid n$, đặt $n=p^em$ và $p\perp m$, thì
    
    $$
    \begin{aligned}
    G(pn) &= G(p^{e+1})G(m)\\
    &= G(p^e)G(m) + (p^{2e+2} - p^{2e+1})G(m)\\
    &= G(n) + (p^{2e+2} - p^{2e+1})G(m).
    \end{aligned}
    $$
    
    Kiểm tra trực tiếp thấy, biểu thức này cũng đúng với $p\perp n$. Do đó, có
    
    $$
    G(n) - G\left(\dfrac{n}{p}\right) = (p^{2e}-p^{2e-1})G(m).
    $$
    
    Thay vào, ta được
    
    $$
    G(pn) = G(n) + p^2\left(G(n) - G\left(\dfrac{n}{p}\right)\right).
    $$
    
    Điều này đơn giản hóa quá trình sàng tuyến tính. Dù không cần thiết, nhưng đối với các hàm tích tính không có tính chất đặc biệt, có thể dùng $G(pn)=G(p^{e+1})G(m)$ để hoàn thành sàng tuyến tính.

??? note "Mã nguồn"
    ```cpp
    --8<-- "docs/math/code/mobius/mobius_2.cpp"
    ```

???+ example "[BZOJ 2154 \[National Team\] Crash's Number Table](https://hydro.ac/p/bzoj-P2154)"
    Tính giá trị:
    
    $$
    \sum_{i=1}^n\sum_{j=1}^m\operatorname{lcm}(i,j)\mod{20101009}.
    $$
    
    Phạm vi dữ liệu: $1\le n,m\le 10^7$.

??? note "Giải pháp"
    Trong quá trình suy luận, bỏ qua modulo. Đặt
    
    $$
    f(n,m) = \sum_{i=1}^n\sum_{j=1}^m\operatorname{lcm}(i,j).
    $$
    
    Lại là chuyển đổi số chia hết thành số chia hết, liệt kê các thừa số chung, và dùng quy trình tiêu chuẩn, ta được
    
    $$
    \begin{aligned}
    f(n,m)
    &= \sum_k\sum_{i=1}^n\sum_{j=1}^m\dfrac{ij}{k}[\gcd(i,j)=k] \\
    &= \sum_k\sum_{i=1}^{\lfloor n/k\rfloor}\sum_{j=1}^{\lfloor m/k\rfloor} kij[\gcd(i,j)=1]\\
    &= \sum_k\sum_{i=1}^{\lfloor n/k\rfloor}\sum_{j=1}^{\lfloor m/k\rfloor} kij\sum_d\mu(d)[d\mid i][d\mid j]\\
    &= \sum_kk\sum_d\mu(d)\left(\sum_{i=1}^{\lfloor n/k\rfloor}i[d\mid i]\right)\left(\sum_{j=1}^{\lfloor m/k\rfloor}j[d\mid j]\right).
    \end{aligned}
    $$
    
    Lại một lần nữa, tổng về $i$ và $j$ tách rời. Trước hết, tính các tổng nội, trích xuất thừa số (tức là lấy $i=di'$), ta có
    
    $$
    \sum_{i=1}^{\lfloor n/k\rfloor}i[d\mid i] = d\sum_{i=1}^{\lfloor\lfloor n/k\rfloor/d\rfloor}i = dG\left(\left\lfloor\dfrac{\lfloor n/k\rfloor}{d}\right\rfloor\right) = dG\left(\left\lfloor\dfrac{n}{kd}\right\rfloor\right).
    $$
    
    Trong đó, $G(n)=\dfrac{1}{2}n(n+1)$ là tổng cấp số cộng, đẳng thức cuối cùng dùng tính chất của [hàm lấy phần nguyên](./basic.md#lấy-phần-nguyên).
    Tương tự, đối với tổng về $j$ có thể tính tương tự. Thay vào biểu thức, ta có
    
    $$
    f(n,m) = \sum_k k\sum_{d}\mu(d)d^2G\left(\left\lfloor\dfrac{n}{kd}\right\rfloor\right)G\left(\left\lfloor\dfrac{m}{kd}\right\rfloor\right).
    $$
    
    Như trước, với dạng liệt kê thừa số chung, cần liệt kê tích $l = kd$, đổi thứ tự tổng:
    
    $$
    f(n,m) = \sum_{\ell}\left(\sum_{d\mid\ell}\mu(d)d\ell\right)G\left(\left\lfloor\dfrac{n}{\ell}\right\rfloor\right)G\left(\left\lfloor\dfrac{m}{\ell}\right\rfloor\right).
    $$
    
    Đặt
    
    $$
    F(\ell) = \sum_{d\mid\ell}\mu(d)d\ell.
    $$
    
    Đây là tích của hàm tích tính $\ell$ và hàm tích tính $\sum_{d\mid\ell}\mu(d)d$ , nên cũng là hàm tích tính, có thể dùng sàng tuyến tính để tính trước, và tính trước tổng prefix. Sau đó, có thể dùng số học phân khối để tính $f(n,m)$ trong $O(\min\{n,m\})$.

??? note "Mã nguồn"
    ```cpp
    --8<-- "docs/math/code/mobius/mobius_3.cpp"
    ```

Tiếp theo, một bài toán đặc biệt, cần chuyển đổi hàm số lượng ước số.
    
???+ example "[LOJ 2185. \[SDOI2015\] Số lượng ước số và tổng](https://loj.ac/problem/2185)"
    $T$ bộ dữ liệu. Với mỗi bộ dữ liệu, tính giá trị:
    
    $$
    \sum_{i=1}^n\sum_{j=1}^m\sigma_0(ij).
    $$
    
    Trong đó, $\sigma_0(n)=\sum_{d \mid n}1$ là số lượng ước số của $n$.
    
    Phạm vi dữ liệu: $1\le n,m,T\le 5\times 10^4$.

??? note "Giải pháp"
    Đây là bài toán khó nhất, vì cần chuyển đổi $\sigma_0(ij)$ thành biểu thức về $\gcd$. Vì $\sigma_0$ là hàm tích tính, có thể đầu tiên xét trường hợp số nguyên tố. Với số nguyên tố $p$ và các số nguyên dương $e_1,e_2$,
    
    $$
    \sigma_0(ij) = 1 + e_1 + e_2 = \sum_{x\mid i}\sum_{y\mid j}[x\perp y].
    $$
    
    Với trường hợp tổng quát, giả sử $i=\prod_p i_p$ và $j=\prod_p j_p$ , trong đó $i_p,j_p$ là các mũ của $p$ trong phân tích thừa số nguyên tố của $i,j$. Tiếp theo, có
    
    $$
    \sigma_0(ij) = \prod_p\sigma_0(i_pj_p)= \prod_p\sum_{x_p\mid i_p}\sum_{y_p\mid j_p}[x_p\perp y_p].
    $$
    
    Nhận thấy, với mỗi thừa số nguyên tố $i_p$ của $i$, liệt kê các ước $x_p$ của nó, tương đương với liệt kê các ước $x$ của $i$ và phân tích thành các thừa số nguyên tố $x_p$; tương tự với $j$. Do đó, dùng phân phối, biểu thức này có thể viết thành
    
    $$
    \sigma_0(ij) = \sum_{x\mid i}\sum_{y\mid j}\prod_p[x_p\perp y_p] = \sum_{x\mid i}\sum_{y\mid j}[x\perp y].
    $$
    
    Bước cuối cùng dùng đến kết luận: $x\perp y$ khi và chỉ khi với mỗi thừa số nguyên tố $p$, đều có $x_p\perp y_p$.
    
    Được biểu thức này, có thể dùng quy trình tiêu chuẩn:
    
    $$
    \begin{aligned}
    \sigma_0(ij) 
    &= \sum_{x\mid i}\sum_{y\mid j}[x\perp y]\\
    &= \sum_{x\mid i}\sum_{y\mid j}\sum_d\mu(d)[d\mid x][d\mid y]\\
    &= \sum_d\mu(d)\left(\sum_{x}[d\mid x\mid i]\right)\left(\sum_{y}[d\mid y\mid j]\right)\\
    &= \sum_d\mu(d)[d\mid i][d\mid j]\sigma_0\left(\dfrac{i}{d}\right)\sigma_0\left(\dfrac{j}{d}\right).
    \end{aligned}
    $$
    
    Bước cuối cùng có nghĩa là: hàm chỉ không lấy giá trị khác $0$ khi $d\mid i$ và $d\mid j$, và khi đó, liệt kê các $x$ sao cho $d\mid x\mid i$ tương đương với liệt kê các $\dfrac{i}{d}$, liệt kê các $y$ tương tự.
    
    Thay biểu thức này vào biểu thức ban đầu, và đổi thứ tự tổng:
    
    $$
    \begin{aligned}
    f(n,m)
    &= \sum_{i=1}^n\sum_{j=1}^m\sigma_0(ij)\\
    &= \sum_{i=1}^n\sum_{j=1}^m\sum_d\mu(d)[d\mid i][d\mid j]\sigma_0\left(\dfrac{i}{d}\right)\sigma_0\left(\dfrac{j}{d}\right)\\
    &= \sum_d\mu(d)\left(\sum_{i=1}^n[d\mid i]\sigma_0\left(\dfrac{i}{d}\right)\right)\left(\sum_{j=1}^m[d\mid j]\sigma_0\left(\dfrac{j}{d}\right)\right)\\
    &= \sum_d\mu(d)\left(\sum_{i=1}^{\lfloor n/d\rfloor}\sigma_0(i)\right)\left(\sum_{j=1}^{\lfloor m/d\rfloor}\sigma_0(j)\right).
    \end{aligned}
    $$
    
    Đặt $G(n)=\sum_{i=1}^n\sigma_0(i)$, thì
    
    $$
    f(n,m)=\sum_{d}\mu(d)G\left(\left\lfloor\dfrac{n}{d}\right\rfloor\right)G\left(\left\lfloor\dfrac{m}{d}\right\rfloor\right).
    $$
    
    Có thể dùng số học phân khối để giải. Chỉ cần tính trước $\mu(n)$ và $\sigma_0(n)$ là được. Tổng thời gian phức tạp là $O(N+T\sqrt{N})$ , trong đó $N$ là giới hạn trên của $n,m$, $T$ là số bộ dữ liệu.

??? note "Mã nguồn"
    ```cpp
    --8<-- "docs/math/code/mobius/mobius_4.cpp"
    ```

Cuối cùng, một bài toán minh họa cách dùng dạng nhân của Molbius inversion.

???+ example "[Luogu P5221 Product](https://www.luogu.com.cn/problem/P5221)"
    Tính giá trị:
    
    $$
    \prod_{i=1}^n\prod_{j=1}^n\dfrac{\operatorname{lcm}(i,j)}{\gcd(i,j)}\pmod{104857601}.
    $$
    
    Phạm vi dữ liệu: $1\le n\le 1\times 10^6$.

??? note "Giải pháp một"
    Trong quá trình suy luận, bỏ qua modulo. Đặt
    
    $$
    f(n) = \prod_{i=1}^n\prod_{j=1}^n\dfrac{\operatorname{lcm}(i,j)}{\gcd(i,j)}.
    $$
    
    Lại là chuyển đổi số chia hết thành số chia hết:
    
    $$
    f(n) = \prod_{i=1}^n\prod_{j=1}^n\dfrac{ij}{(\gcd(i,j))^2}.
    $$
    
    Nhận thấy, các thừa số này là độc lập, có thể tính riêng. Đặt
    
    $$
    g(n) = \prod_{i=1}^n\prod_{j=1}^n\gcd(i,j).
    $$
    
    Thì biểu thức bằng:
    
    $$
    f(n) = \dfrac{(n!)^{2n}}{g(n)^2}.
    $$
    
    Trọng tâm là giải quyết $g(n)$. Cách xử lý tương tự như trước, nhưng cần chuyển sang dạng nhân. Trước hết, liệt kê và trích xuất thừa số:
    
    $$
    \begin{aligned}
    g(n) &= \prod_k\prod_{i=1}^n\prod_{j=1}^nk\uparrow[\gcd(i,j)=k]\\
    &= \prod_k\prod_{i=1}^{\lfloor n/k\rfloor}\prod_{j=1}^{\lfloor n/k\rfloor}k\uparrow[\gcd(i,j)=1].
    \end{aligned}
    $$
    
    Trong đó, $a\uparrow b=a^b$ là Knuth arrow. Sau đó, thay $[\gcd(i,j)=1]=\sum_d\mu(d)[d\mid i][d\mid j]$ và chuyển tổng về mũ, ta được:
    
    $$
    g(n) = \prod_k\prod_d\prod_{i=1}^{\lfloor n/k\rfloor}\prod_{j=1}^{\lfloor n/k\rfloor}k\uparrow(\mu(d)[d\mid i][d\mid j]).
    $$
    
    Lại một lần nữa, trích xuất thừa số (tức là lấy $i=di'$, $j=dj'$), và dùng tính chất của [hàm lấy phần nguyên](./basic.md#lấy-phần-nguyên), ta được:
    
    $$
    g(n) = \prod_k\prod_d\prod_{i=1}^{\lfloor n/(kd)\rfloor}\prod_{j=1}^{\lfloor n/(kd)\rfloor}k\uparrow\mu(d).
    $$
    
    Sau đó, tách rời về $i,j$, ta thấy rằng biểu thức không còn chứa $i,j$, nên nó tương đương với lấy mũ:
    
    $$
    g(n) = \prod_k\prod_d k\uparrow\left(\mu(d)\left\lfloor\dfrac{n}{kd}\right\rfloor^2\right).
    $$
    
    Vì đã liệt kê các thừa số chung, nên cần đổi thứ tự tổng. Đặt $\ell = kd$, ta có:
    
    $$
    \begin{aligned}
    g(n) &= \prod_{\ell}\prod_{d\mid\ell}\left(\dfrac{\ell}{d}\right)\uparrow\left(\mu(d)\left\lfloor\dfrac{n}{\ell}\right\rfloor^2\right)\\
    &= \prod_\ell\left(\prod_{d\mid\ell}\left(\dfrac{\ell}{d}\right)\uparrow\mu(d)\right)\uparrow\left\lfloor\dfrac{n}{\ell}\right\rfloor^2.
    \end{aligned}
    $$
    
    Đặt
    
    $$
    F(n) = \prod_{d\mid n}\left(\dfrac{n}{d}\right)\uparrow\mu(d).
    $$
    
    Dễ thấy đây là dạng tích của Molbius inversion. Dù không biết biểu thức, cũng có thể dùng [Dirichlet difference](#dirichlet-prefix-sum) để tính trước. Dù vậy, vì $\tilde F(n)=n$ rất đơn giản, nên biểu thức của $F(n)$ có thể tìm được:
    
    $$
    F(n) = 
    \begin{cases}
    p, & n = p^e,~p\in\mathbf P,~e\in\mathbf N_+, \\
    1, &\text{otherwise}.
    \end{cases}
    $$
    
    [von Mangoldt function](#Molbius inversion) chính là tự nhiên đối số. Được giá trị $F(n)$, có thể dùng tích phân khối để tính $g(n)$ trong $O(\sqrt{n})$ thời gian. Tổng thời gian phức tạp là $O(n)$.
    
    Cần lưu ý rằng, khi tính tích, thường dùng [Fermat's theorem](./fermat.md), nên chỉ số mũ cần lấy modulo với số modulo khác với số đã cho.

??? note "Giải pháp hai"
    Cách xử lý dạng tích khó nhất là đối với tích và mũ, nên có thể lấy log trước. Với bài toán này, chỉ xét $g(n)$. Lấy log, ta có:
    
    $$
    \log g(n) = \sum_{i=1}^n\sum_{j=1}^n\log\gcd(i,j).
    $$
    
    Với dạng này, dùng quy trình tiêu chuẩn, ta có:
    
    $$
    \begin{aligned}
    \log g(n) 
    &= \sum_k\log k\sum_{i=1}^n\sum_{j=1}^n[\gcd(i,j)=k]\\
    &= \sum_k\log k\sum_{i=1}^{\lfloor n/k\rfloor}\sum_{j=1}^{\lfloor n/k\rfloor}[\gcd(i,j)=1]\\
    &= \sum_k\log k\sum_d\mu(d)\left(\sum_{i=1}^{\lfloor n/k\rfloor}[i\mid d]\right)\left(\sum_{j=1}^{\lfloor n/k\rfloor}[j\mid d]\right)\\
    &= \sum_k\log k\sum_d\mu(d)\left\lfloor\dfrac{n}{kd}\right\rfloor^2\\
    &= \sum_{\ell}\left(\sum_d\mu(d)\log\dfrac{\ell}{d}\right)\left\lfloor\dfrac{n}{\ell}\right\rfloor^2\\
    &= \sum_{\ell}\Lambda(\ell)\left\lfloor\dfrac{n}{\ell}\right\rfloor^2.
    \end{aligned}
    $$
    
    Trong đó, $\Lambda(n)$ là [von Mangoldt function](#Molbius inversion). Lấy kết quả này, ta được giải pháp một.

??? note "Mã nguồn"
    ```cpp
    --8<-- "docs/math/code/mobius/mobius_5.cpp"
    ```

## Bài tập

-   [Luogu P3312 \[SDOI2014\] Số bảng](https://www.luogu.com.cn/problem/P3312)
-   [Luogu P3700 \[CQOI2017\] Bảng nhỏ của Q](https://www.luogu.com.cn/problem/P3700)
-   [Luogu P3704 \[SDOI2017\] Bảng số](https://www.luogu.com.cn/problem/P3704)
-   [Luogu P3768 Bài toán đơn giản](https://www.luogu.com.cn/problem/P3768)
-   [Luogu P4464 \[National Team\] JZPKIL](https://www.luogu.com.cn/problem/P4464)
-   [Luogu P4619 \[SDOI2018\] Bài toán cũ](https://www.luogu.com.cn/problem/P4619)
-   [Luogu P5518 \[MtOI2019\] Đoàn quỷ](https://www.luogu.com.cn/problem/P5518)
-   [Luogu P6222 Bài toán đơn giản](https://www.luogu.com.cn/problem/P6222)
-   [Luogu P6825「EZEC-4」Tổng](https://www.luogu.com.cn/problem/P6825)
-   [Luogu P7486「Stoi2031」Rainbow](https://www.luogu.com.cn/problem/P7486)
-   [AtCoder Grand Contest 038 C - LCMs](https://atcoder.jp/contests/agc038/tasks/agc038_c)
-   [Codeforces 1139 D. Steps to One](https://codeforces.com/problemset/problem/1139/D)

## Tài liệu tham khảo

-   [Möbius function - Wikipedia](https://en.wikipedia.org/wiki/M%C3%B6bius_function)
-   [Möbius inversion formula - Wikipedia](https://en.wikipedia.org/wiki/M%C3%B6bius_inversion_formula)
-   [Von Mangoldt function - Wikipedia](https://en.wikipedia.org/wiki/Von_Mangoldt_function)
-   [algocode 算法博客](https://web.archive.org/web/20190523150159/https://algocode.net/2018/04/18/20180418-KB-Mobius-Inversion-Formula/)
