author: billchenchina, c-forrest, CCXXXI, danielqfmai, Enter-tainer, Great-designer, HeRaNO, lychees, Menci, Nanarikom, ouuan, shuzhouliu, sshwy, Tiphereth-A

Bài viết giới thiệu tích Dirichlet và hàm sinh Dirichlet.

## Tích Dirichlet

Tích Dirichlet của hai hàm số học $f(n)$ và $g(n)$, ký hiệu $f \ast g$, là hàm số học

$$
(f \ast g)(n) = \sum_{k\mid n}f(k)g\left(\dfrac{n}{k}\right) = \sum_{k\ell=n}f(k)g(\ell).
$$

Tích Dirichlet là phép toán quan trọng trong số học. Nhiều tính chất của hàm số học được khai thác từ phép toán này.

???+ example "Ví dụ"
    1.  Hàm đơn vị $\varepsilon$ là tích Dirichlet của hàm Möbius $\mu$ và hàm hằng $1$:
    
    $$
    \varepsilon=\mu \ast 1 \iff\varepsilon(n)=\sum_{d\mid n}\mu(d).
    $$
    
    2.  Hàm số lượng ước $\tau$ là tích Dirichlet của hàm hằng $1$ với chính nó:
    
        $$
        \tau=1 \ast 1 \iff \tau(n)=\sum_{d\mid n}1.
        $$
    
    3.  Hàm tổng ước $\sigma$ là tích Dirichlet của hàm đồng nhất $\mathrm{id}$ và hàm hằng $1$:
    
        $$
        \sigma=\mathrm{id} \ast 1 \iff\sigma(n)=\sum_{d\mid n}d.
        $$
    
    4.  Hàm Euler $\varphi$ là tích Dirichlet của $\mathrm{id}$ và $\mu$:
    
        $$
        \varphi=\mathrm{id}\ast \mu \iff\varphi(n)=\sum_{d\mid n}d\cdot\mu\left(\frac{n}{d}\right).
        $$

[Phép đảo Möbius](./mobius.md) sử dụng $\varepsilon=\mu \ast 1$ để biến đổi đẳng thức hàm số học.

### Tính chất

Tích Dirichlet có các tính chất đại số.

???+ note "Định lý"
    Cho $f,g,h$ là các hàm số học. Khi đó:
    
    1.  **Giao hoán**: $f\ast g=g\ast f$.
    2.  **Kết hợp**: $(f\ast g)\ast h=f\ast(g\ast h)$.
    3.  **Phân phối**: $(f+g)\ast h = f\ast h + g\ast h$.
    4.  **Đơn vị**: $f\ast\varepsilon = \varepsilon \ast f = f$, trong đó $\varepsilon(n) = [n=1]$ là đơn vị, $[\cdot]$ là ngoặc Iverson.
    5.  **Nghịch đảo**: khi và chỉ khi $f(1)\neq 0$ thì tồn tại $g$ sao cho $f\ast g=g\ast f=\varepsilon$; $g$ gọi là **nghịch đảo Dirichlet** của $f$, ký hiệu $f^{-1}$. Hơn nữa
    
        $$
        g(n) = \dfrac{\varepsilon(n) - \sum_{k\ell = n,~k\neq 1}f(k)g(\ell)}{f(1)}.
        $$

??? note "Chứng minh"
    Giao hoán:
    
    $$
    (f\ast g)(n) = \sum_{k\ell=n}f(k)g(\ell) = (g\ast f)(n).
    $$
    
    Kết hợp:
    
    $$
    ((f\ast g)\ast h)(n) = \sum_{k\ell m = n}f(k)g(\ell)h(m) = (f\ast (g\ast h))(n).
    $$
    
    Phân phối:
    
    $$
    \begin{aligned}
    ((f+g)\ast h)(n) &= \sum_{k\ell = n}(f(k) + g(k))h(\ell) \\
    &= \sum_{k\ell=n}f(k)h(\ell) + \sum_{k\ell=n}g(k)h(\ell) = (f\ast h+g\ast h)(n).
    \end{aligned}
    $$
    
    Đơn vị:
    
    $$
    (f\ast\varepsilon)(n) = \sum_{k\ell = n}f(k)\varepsilon(\ell) = f(n).
    $$
    
    Do $\varepsilon(\ell)$ chỉ khác $0$ khi $\ell=1$.
    
    Nghịch đảo: giả sử tồn tại $g$ sao cho $f\ast g=\varepsilon$. Khi đó
    
    $$
    (f\ast g)(n) = \sum_{k\ell = n}f(k)g(\ell) = \varepsilon(n).
    $$
    
    Đây là hệ phương trình xác định $g(n)$. Đặc biệt $n=1$ cho $f(1)g(1)=1$, nên cần $f(1)\neq 0$. Khi $f(1)\neq 0$ thì
    
    $$
    g(n) = \dfrac{\varepsilon(n) - \sum_{k\ell = n,~k\neq 1}f(k)g(\ell)}{f(1)}
    $$
    
    cho phép tính $g(n)$ đệ quy. Do đó nghịch đảo tồn tại khi và chỉ khi $f(1)\neq 0$.

Theo đại số trừu tượng, các hàm số học với phép cộng điểm và tích Dirichlet tạo thành [vành giao hoán](../algebra/basic.md#环), gọi là **vành Dirichlet**. Các phần tử khả nghịch là những hàm có giá trị khác $0$ tại $n=1$.

Hàm tích là lớp đặc biệt; đóng kín với tích Dirichlet và nghịch đảo.

???+ note "Định lý"
    Nếu $f,g$ là hàm tích thì $f\ast g$ cũng tích, và $f^{-1}$ tồn tại và cũng tích.

??? note "Chứng minh"
    Với $h=f\ast g$, nếu $n_1\perp n_2$ thì
    
    $$
    \begin{aligned}
    h(n_1)h(n_2) &= \left(\sum_{k_1\ell_1=n_1}f(k_1)g(\ell_1)\right)\left(\sum_{k_2\ell_2=n_2}f(k_2)g(\ell_2)\right)\\
    &= \sum_{k_1\ell_1=n_1,~k_2\ell_2=n_2}f(k_1)f(k_2)g(\ell_1)g(\ell_2)\\
    &= \sum_{k\ell = n_1n_2}f(k)g(\ell) \\
    &= h(n_1n_2).
    \end{aligned}
    $$
    
    Lập luận đổi thứ tự tổng: mỗi ước $k$ của $n_1n_2$ tách thành $k_1k_2$ với $k_1\mid n_1$, $k_2\mid n_2$, và ngược lại.
    
    Với $g=f^{-1}$, dùng quy nạp. $g(1)=1/f(1)=1$. Công thức nghịch đảo:
    
    $$
    g(n) = \varepsilon(n) - \sum_{k\ell = n,~k\neq 1} f(k)g(\ell).
    $$
    
    Với $n_1\perp n_2$ và $n_1n_2>1$:
    
    $$
    \begin{aligned}
    g(n_1n_2) &= -\sum_{k\ell=n_1n_2,~k\neq 1}f(k)g(\ell) \\
    &= -\sum_{k_1\ell_1=n_1,~k_2\ell_2=n_2,~k_1k_2\neq 1}f(k_1)f(k_2)g(\ell_1)g(\ell_2)\\
    &= f(1)f(1)g(n_1)g(n_2) - \sum_{k_1\ell_1=n_1,~k_2\ell_2=n_2}f(k_1)f(k_2)g(\ell_1)g(\ell_2)\\
    &= g(n_1)g(n_2) - \left(\sum_{k_1\ell_1=n_1}f(k_1)g(\ell_1)\right)\left(\sum_{k_2\ell_2=n_2}f(k_2)g(\ell_2)\right) \\
    &= g(n_1)g(n_2) - \varepsilon(n_1)\varepsilon(n_2)\\
    &= g(n_1)g(n_2).
    \end{aligned}
    $$
    
    Dùng giả thiết quy nạp ở bước hai.

Nhìn từ đại số trừu tượng, các hàm tích là một [nhóm con](../algebra/group-theory.md#子群) của nhóm nhân trong vành Dirichlet.

Đặc biệt hơn là hàm hoàn toàn tích.

???+ note "Định lý"
    Cho $\alpha$ là hàm hoàn toàn tích, $f,g$ là hàm số học:
    
    1.  Phân phối: $(\alpha f)\ast(\alpha g) = \alpha\cdot(f\ast g)$.
    2.  Nghịch đảo: $(\alpha f)^{-1}=\alpha f^{-1}$ nếu $f^{-1}$ tồn tại.
    3.  Hàm tích $f$ là hoàn toàn tích khi và chỉ khi $f^{-1}=\mu f$, trong đó $\mu$ là [hàm Möbius](./mobius.md#莫比乌斯函数).

??? note "Chứng minh"
    1.  
    $$
    \begin{aligned}
    ((\alpha f)\ast(\alpha g))(n) &= \sum_{k\ell = n}(\alpha f)(k)(\alpha g)(\ell) \\
    &= \sum_{k\ell = n}\alpha(k)f(k)\alpha(\ell)g(\ell) \\
    &= \sum_{k\ell = n}\alpha(n)f(k)g(\ell) \\
    &= \alpha(n)(f\ast g)(n).
    \end{aligned}
    $$
    
    2.  
    $$
    (\alpha f)\ast(\alpha f^{-1}) = \alpha(f\ast f^{-1}) = \alpha\varepsilon = \varepsilon.
    $$
    
    3.  Nếu $f$ hoàn toàn tích thì
    
    $$
    f^{-1} = (1f)^{-1} = 1^{-1}\cdot f = \mu f.
    $$
    
    Ngược lại, nếu $f$ tích và $f^{-1}=\mu f$, chỉ cần chứng minh $f(p^e)=f(p)^e$. Dùng quy nạp theo $e$. Với $e>1$,
    
    $$
    \begin{aligned}
    f^{-1}(p^e) &= -\sum_{i=1}^{e}f(p^i)f^{-1}(p^{e-i}) \\
    &= -\sum_{i=1}^{e}f(p^i)\mu(p^{e-i})f(p^{e-i})\\
    &= -f(p^e)f(1)\mu(1) - f(p^{e-1})\mu(p)f(p)\\
    &= -f(p^e) + f(p)^e.
    \end{aligned}
    $$
    
    Do $f^{-1}(p^e)=\mu(p^e)f(p^e)=0$, suy ra $f(p^e)=f(p)^e$.

Về đại số, với $\alpha$ hoàn toàn tích, ánh xạ $f\mapsto \alpha f$ là [tự đồng cấu](../algebra/ring-theory.md#理想) của vành Dirichlet.

## Hàm sinh Dirichlet

Gắn với tích Dirichlet là **hàm sinh Dirichlet** (Dirichlet series generating function, DGF). Với hàm số học $f(n)$ (dãy $\{f(n)\}$), định nghĩa chuỗi Dirichlet hình thức:

$$
F(s) = \sum_{n=1}^{\infty}\dfrac{f(n)}{n^s}.
$$

$s$ là biến hình thức. Trong thực tế, $s$ có thể là biến phức để nghiên cứu tính giải tích, nhưng nằm ngoài phạm vi lập trình thi đấu.

Tích của hàm sinh tương ứng tích Dirichlet:

???+ note "Định lý"
    Với $f,g$ và hàm sinh $F,G$, thì hàm sinh của $f\ast g$ là $F\cdot G$.

??? note "Chứng minh"
    Trực tiếp:
    
    $$
    \begin{aligned}
    F(s)G(s) &= \left(\sum_{k=1}^\infty\dfrac{f(k)}{k^s}\right)\left(\sum_{\ell=1}^\infty\dfrac{g(\ell)}{\ell^s}\right)= \sum_{k=1}^\infty\sum_{\ell=1}^\infty\dfrac{f(k)g(\ell)}{(k\ell)^s}\\
    &= \sum_{n=1}^{\infty}\dfrac{\sum_{k\ell = n}f(k)g(\ell)}{n^s} = \sum_{n=1}^\infty\dfrac{(f\ast g)(n)}{n^s}.
    \end{aligned}
    $$

Nhờ tương ứng này, các tính chất của tích Dirichlet có thể hiểu qua phép nhân chuỗi Dirichlet hình thức.

### Tích Euler

Tính chất của hàm tích phản ánh qua hàm sinh. Nhờ [phân tích duy nhất](./basic.md#算术基本定理):

$$
\begin{aligned}
F(s) &= \sum_{n=1}^{\infty}\dfrac{f(n)}{n^s} = \sum_{n=1}^{\infty}\prod_{p\in\mathbf P}\dfrac{f(p^{e})}{p^{es}} = \prod_{p\in\mathbf P}\sum_{e=0}^{\infty}\dfrac{f(p^e)}{p^{es}}\\
&= \prod_{p\in\mathbf P}\left(1 + \dfrac{f(p)}{p^s} + \dfrac{f(p^2)}{p^{2s}} + \dfrac{f(p^3)}{p^{3s}} + \cdots\right).
\end{aligned}
$$

Do đó $F(s)$ là tích vô hạn các $F_p(s)$, gọi là **tích Euler**. Điều này tương ứng với việc tích Dirichlet của các hàm tích vẫn là hàm tích.

Nếu $f$ hoàn toàn tích thì $f(p^e)=f(p)^e$ nên

$$
F(s) = \prod_{p\in\mathbf P}\sum_{e=0}^{\infty}\dfrac{f(p)^e}{p^{es}} = \prod_{p\in\mathbf P}\left(1-\dfrac{f(p)}{p^s}\right)^{-1}.
$$

Không như hàm tích, hàm hoàn toàn tích không đóng kín dưới tích Dirichlet và nghịch đảo, nhưng kết quả luôn là hàm tích.

???+ example "Ví dụ"
    1.  Hàm đơn vị $\varepsilon(n)$ là hoàn toàn tích. Hàm sinh:
    
        $$
        E(s) = \sum_{n=1}^{\infty}\dfrac{\varepsilon(n)}{n^s} = 1.
        $$
    
    2.  Hàm hằng $1(n)$ là hoàn toàn tích. Hàm sinh là zeta:
    
        $$
        I(s) = \sum_{n=1}^{\infty}\dfrac{1}{n^s} = \prod_{p\in\mathbf P}\dfrac{1}{1-p^{-s}} = \zeta(s).
        $$
    
    3.  Hàm Möbius $\mu(n)$ là nghịch đảo Dirichlet của $1$. Hàm sinh:
    
        $$
        M(s) = \sum_{n=1}^{\infty}\dfrac{\mu(n)}{n^s} = \prod_{p\in\mathbf P}(1-p^{-s}) = \dfrac{1}{\zeta(s)}.
        $$
    
    4.  Hàm lũy thừa $\operatorname{id}_k(n)=n^k$ là hoàn toàn tích. Hàm sinh:
    
        $$
        I_k(s) = \sum_{n=1}^{\infty}\dfrac{n^k}{n^s} = \prod_{p\in\mathbf P}\dfrac{1}{1-p^{k-s}} = \zeta(s-k).
        $$
    
    5.  Hàm Euler $\varphi(n)$ là tích. Hàm sinh:
    
        $$
        \begin{aligned}
        \Phi(s) &= \prod_{p\in\mathbf P}\left(1+\dfrac{p-1}{p^s} + \dfrac{p(p-1)}{p^{2s}} + \dfrac{p^2(p-1)}{p^{3s}}+\cdots\right)\\
        &= \prod_{p\in\mathbf P}\left(\dfrac{1}{1-p^{1-s}}-\dfrac{1}{p^s}\dfrac{1}{1-p^{1-s}}\right) = \prod_{p\in\mathbf P}\dfrac{1-p^{-s}}{1-p^{1-s}} = \dfrac{\zeta(s-1)}{\zeta(s)}.
        \end{aligned}
        $$
    
        Kết hợp với hàm lũy thừa, suy ra $\mathrm{id} = \varphi\ast 1$.
    
    6.  Hàm tổng ước $\sigma_k(n)=\sum_{d\mid n}d^k$ là tích. Hàm sinh:
    
        $$
        \begin{aligned}
        \Sigma_k(s) &= \prod_{p\in\mathbf P}\left(1+\dfrac{1+p^k}{p^s}+\dfrac{1+p^k+p^{2k}}{p^{2s}}+\dfrac{1+p^k+p^{2k}+p^{3k}}{p^{3s}}+\cdots\right) \\
        &= \prod_{p\in\mathbf P}\dfrac{1}{1-p^k}\left((1-p^k)+\dfrac{1-p^{2k}}{p^s}+\dfrac{1-p^{3k}}{p^{2s}}+\dfrac{1-p^{4k}}{p^{3k}}+\cdots\right)\\
        &= \prod_{p\in\mathbf P}\dfrac{1}{1-p^k}\left(\dfrac{1}{1-p^{-s}} - \dfrac{p^k}{1-p^{k-s}}\right)\\
        &= \prod_{p\in\mathbf P}\dfrac{1}{(1-p^{-s})(1-p^{k-s})} = \zeta(s-k)\zeta(s).
        \end{aligned}
        $$
    
        Suy ra $\sigma_k = \mathrm{id}_k\ast 1$.
    
    7.  Hàm chỉ thị số không có bình phương ước $u(n)=|\mu(n)|$ là tích. Hàm sinh:
    
        $$
        U(s) = \prod_{p\in\mathbf P}(1+p^{-s}) = \prod_{p\in\mathbf P}\dfrac{1-p^{-2s}}{1-p^{-s}} = \dfrac{\zeta(s)}{\zeta(2s)}.
        $$

### Ứng dụng

Hàm sinh Dirichlet giúp biểu diễn hàm tích dưới dạng tích Dirichlet.

Ví dụ trong sàng Du Jiao, để tính tổng tiền tố của hàm tích $f$, ta tìm $g$ sao cho $f\ast g$ và $g$ đều có thể tính tiền tố nhanh. Dùng hàm sinh để suy ra.

Trong ví dụ [Luogu P3768](../number-theory/du.md#问题二), cần $f(n)=n^2\varphi(n)$. Hàm sinh:

$$
F(s) = \prod_{p\in\mathbf P}\left(1 + \sum_{k=1}^{\infty}\dfrac{p^{3k-1}(p-1)}{p^{ks}}\right) = \prod_{p\in\mathbf P}\dfrac{1-p^{2-s}}{1-p^{3-s}} = \dfrac{\zeta(s-3)}{\zeta(s-2)}.
$$

So sánh với hàm lũy thừa, lấy $g = \mathrm{id}_2$ thì $f \ast g = \mathrm{id}_3$, đều tính tiền tố nhanh.

## Tính tích Dirichlet

Bài toán: cho $\{f(k)\}_{k=1}^n$, $\{g(k)\}_{k=1}^n$, tính $h=f\ast g$ cho $k\le n$. Tùy tính chất mà độ phức tạp khác nhau.

### Trường hợp chung

Không có tính chất đặc biệt thì dùng định nghĩa:

$$
h(n) = \sum_{k\ell = n}f(k)g(\ell).
$$

Duyệt $k,\ell$, cộng vào $h(k\ell)$. Độ phức tạp:

$$
O\left(\sum_{k=1}^{n}\dfrac{n}{k}\right) = O(n\log n).
$$

???+ example "Hiện thực tham khảo"
    ```cpp
    --8<-- "docs/math/code/dirichlet/dirichlet-1.cpp:core"
    ```

### Khi một hàm tích

Nếu $g$ là hàm tích, có thể dùng tích Euler để tăng tốc. Tính $h$ tương đương tính hệ số của $H(s)=F(s)G(s)$, với

$$
H(s) = F(s)G(s) = F(s)\prod_{p\in\mathbf P}G_p(s).
$$

Trong đó $G_p(s)$ chỉ chứa các hệ số tại lũy thừa của $p$:

$$
G_p(s) = \sum_{p^k\le n}\dfrac{f(p^k)}{p^{ks}} = 1 + \dfrac{f(p)}{p^s} + \dfrac{f(p^2)}{p^{2s}} + \cdots.
$$

Bắt đầu từ $F(s)$, duyệt mọi $p\le n$, nhân lần lượt $G_p(s)$ bằng cách dùng thuật toán chung. Tổng số phép duyệt:

$$
\sum_{p\in\mathbf P,~p\le n}\sum_{k=1}^{\infty}\left\lfloor\dfrac{n}{p^k}\right\rfloor \le \sum_{p\in\mathbf P,~p\le n}\dfrac{n}{p-1} \le \sum_{p\in\mathbf P,~p\le n}\dfrac{2n}{p} \in O(n\log\log n).
$$

Tương tự phân tích độ phức tạp của [sàng Eratosthenes](./sieve.md#埃拉托斯特尼筛法). Vậy tổng thể $O(n\log\log n)$.

???+ example "Hiện thực tham khảo"
    ```cpp
    --8<-- "docs/math/code/dirichlet/dirichlet-2.cpp:core"
    ```

Đặc biệt, nếu $g$ là hoàn toàn tích hoặc nghịch đảo Dirichlet của nó (như $g=1$ hoặc $g=\mu$), thuật toán có thể đơn giản hơn. Khi đó có thể dùng [tiền tố/hiệu Dirichlet](./mobius.md#dirichlet-前缀和), độ phức tạp vẫn $O(n\log\log n)$.

### Khi kết quả là hàm tích

Nếu $h$ là hàm tích, đặc biệt khi $f,g$ đều tích, thì chỉ cần tính $h$ tại các lũy thừa của số nguyên tố, rồi dùng [sàng tuyến tính](./sieve.md#线性筛法) để suy ra toàn bộ trong $O(n)$. Với $p^e$:

$$
h(p^e) = \sum_{i=0}^e f(p^i)g(p^{e-i}).
$$

Số phép duyệt:

$$
\begin{aligned}
\sum_{p\in\mathbf P,~p\le n}\sum_{e=1}^{\lfloor\log_p n\rfloor}(e+1) &\le \sum_{p\in\mathbf P,~p\le\sqrt{n}}\lfloor\log_p n\rfloor^2 + \sum_{p\in\mathbf P,~\sqrt{n} < p\le n}1 \\
&\le \sqrt{n}(\log_2 n)^2 + n \in O(n).
\end{aligned}
$$

Vậy tổng thời gian $O(n)$.

???+ example "Hiện thực tham khảo"
    ```cpp
    --8<-- "docs/math/code/dirichlet/dirichlet-3.cpp:core"
    ```

## Tài liệu tham khảo và chú thích

-   [Dirichlet convolution - Wikipedia](https://en.wikipedia.org/wiki/Dirichlet_convolution)
-   [Dirichlet series - Wikipedia](https://en.wikipedia.org/wiki/Dirichlet_series)
-   [Euler product - Wikipedia](https://en.wikipedia.org/wiki/Euler_product)
-   [Dirichlet 積と、数論関数の累積和 by maspy](https://maspypy.com/dirichlet-%e7%a9%8d%e3%81%a8%e3%80%81%e6%95%b0%e8%ab%96%e9%96%a2%e6%95%b0%e3%81%ae%e7%b4%af%e7%a9%8d%e5%92%8c)
