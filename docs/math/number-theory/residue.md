Kiến thức trước: [logarit rời rạc](./discrete-logarithm.md)

Bài viết bàn về thặng dư bậc cao và căn đơn vị theo môđun, đồng thời giới thiệu thuật toán khai căn theo môđun.

## Thặng dư bậc cao

Thặng dư bậc cao trong môđun có thể hiểu là khả năng khai căn bậc cao theo môđun. Đây là mở rộng của [thặng dư bậc hai](./quad-residue.md).

???+ abstract "Thặng dư bậc $k$"
    Cho số nguyên $k\geq 2$, số nguyên $a$ và số nguyên dương $m$ nguyên tố cùng nhau. Nếu tồn tại số nguyên $x$ sao cho
    
    $$
    x^k\equiv a\pmod m,
    $$
    
    thì $a$ gọi là **thặng dư bậc $k$** modulo $m$ (k-th residue), và $x$ là **căn bậc $k$** của $a$ modulo $m$; ngược lại gọi là **phi thặng dư bậc $k$** (k-th nonresidue).

Tức là căn bậc $k$ của $a$ modulo $m$ tồn tại khi và chỉ khi $a$ là thặng dư bậc $k$.

### Tính chất

Tương tự thặng dư bậc hai, có thể xét tiêu chuẩn, số lượng, và số lượng lớp thặng dư bậc $k$. Như các bài [phương trình đồng dư](./congruence-equation.md), có thể dùng [CRT](./crt.md) quy về môđun lũy thừa nguyên tố. Tùy việc có nguyên căn, chia thành trường hợp lũy thừa nguyên tố lẻ và lũy thừa của $2$.

Với môđun lẻ, trường hợp đơn giản hơn. Thực tế, trong mọi trường hợp có nguyên căn, có:

???+ note "Định lý"
    Cho $k\geq 2$, $a$ nguyên, $m$ dương, $a\perp m$. Giả sử môđun $m$ có nguyên căn, và $g$ là một nguyên căn. Đặt $d=\gcd(k,\varphi(m))$, $d'=\dfrac{\varphi(m)}{d}$, với $\varphi(m)$ là [hàm Euler](./euler-totient.md). Khi đó:
    
    1.  $a$ là thặng dư bậc $k$ modulo $m$ khi và chỉ khi
    
        $$
        a^{d'} \equiv 1 \pmod m.
        $$
    2.  Nếu $a$ là thặng dư bậc $k$, thì có đúng $d$ căn bậc $k$ khác nhau modulo $m$, có dạng
    
        $$
        x \equiv g^{y_0+id'}\pmod{\varphi(m)},~0\le y_0 < d',~i=0,1,\cdots,d-1.
        $$
    3.  Số lớp thặng dư bậc $k$ modulo $m$ là $d'$, và toàn bộ là
    
        $$
        \{g^{di}\bmod m : 0 \le i < d'\}.
        $$

??? note "Chứng minh"
    Vì $a\perp m$ nên $x\perp m$. Do $g$ là nguyên căn, $x$ và $a$ đều đồng dư với một lũy thừa của $g$. Đặt $x\equiv g^y\pmod m$, thì $x^k\equiv a\pmod m$ tương đương
    
    $$
    g^{ky} \equiv g^{\operatorname{ind}_g a}\pmod m.
    $$
    
    Trong đó $\operatorname{ind}_g a$ là log rời rạc. Theo [tính chất bậc](./primitive-root.md#幂的循环结构) và $\delta_m(g)=\varphi(m)$, điều này tương đương
    
    $$
    ky \equiv \operatorname{ind}_g a \pmod{\varphi(m)}.
    $$
    
    Đây là [đồng dư tuyến tính](./linear-equation.md) theo $y$. Áp dụng phân tích nghiệm, ta biết phương trình có nghiệm khi và chỉ khi $d\mid\operatorname{ind}_g a$, và nghiệm có dạng
    
    $$
    y = y_0 + id' \pmod{\varphi(m)},~0\le y_0 < d',~i=0,1,\cdots,d-1.
    $$
    
    Từ đó suy ra nội dung định lý; còn điều kiện $a^{d'} \equiv 1 \pmod m$ là tiêu chuẩn tương đương. Theo [tính chất bậc 3](./primitive-root.md#ord-prop-3),
    
    $$
    \delta_m(a) = \delta_m(g^{\operatorname{ind}_g a}) = \dfrac{\varphi(m)}{\gcd(\varphi(m),\operatorname{ind}_g a)} = \dfrac{\varphi(m)}{\operatorname{ind}_g a}.
    $$
    
    Phương trình có nghiệm khi và chỉ khi $d\mid \operatorname{ind}_g a$, tức $\delta_m(a)\mid d'$. Theo [tính chất bậc 2](./primitive-root.md#ord-prop-2) điều này tương đương $a^{d'}\equiv 1\pmod m$.

Trường hợp môđun là lũy thừa của $2$ đặc biệt. Cần dùng kết luận về cấu trúc hệ thặng dư nguyên tố cùng nhau modulo $2^e$: mọi số lẻ $a$ đều đồng dư duy nhất với một số dạng $(-1)^s5^r\bmod 2^e$, với $s\in\{0,1\}$ và $0\le r < 2^{e-2}$. Từ đó có:

???+ note "Định lý"
    Với $k\ge 2$, $a$ lẻ và $m=2^e$ với $e \ge 2$. Khi $k$ lẻ:
    
    1.  $a$ luôn là thặng dư bậc $k$ modulo $m$.
    2.  $a$ có đúng một căn bậc $k$ modulo $m$.
    3.  Số lớp thặng dư bậc $k$ là $2^{e-1}$ và chính là toàn bộ các lớp nguyên tố cùng nhau.
    
    Khi $k$ chẵn, đặt $d=\gcd(k,2^{e-2})$, $d'=\dfrac{2^{e-2}}{d}$, có:
    
    1.  $a$ là thặng dư bậc $k$ modulo $m$ khi và chỉ khi $a\equiv 1\pmod 4$ và $a^{d'}\equiv 1\pmod m$.
    2.  Nếu $a$ là thặng dư bậc $k$, thì có đúng $2d$ căn bậc $k$ khác nhau modulo $m$, dạng
    
        $$
        x \equiv \pm 5^{y_0 + id'} \pmod{2^{e-1}},~ 0 \le y_0 < d',~i = 0, 1,\cdots,d-1. 
        $$
    3.  Số lớp thặng dư bậc $k$ là $d'$, và toàn bộ là
    
        $$
        \{5^{di}\bmod m : 0 \le i < d'\}.
        $$

??? note "Chứng minh"
    Vì $a\perp m$ nên $x\perp m$. Do $x,a$ lẻ, đặt $a\equiv (-1)^s5^r\pmod{2^e}$ và $x=(-1)^z5^{y}\pmod{2^e}$. Biểu diễn là duy nhất, nên $x^k\equiv a\pmod{2^e}$ tương đương hệ [đồng dư tuyến tính](./linear-equation.md):
    
    $$
    \begin{aligned}
    kz &\equiv s \pmod{2},\\
    ky &\equiv r \pmod{2^{e-2}}.
    \end{aligned}
    $$
    
    Dựa vào phân tích nghiệm, ta có:
    
    -   Khi $k$ lẻ, $\gcd(k,2)=\gcd(k,2^{e-2})=1$, nên hai phương trình luôn có nghiệm với mọi $s,r$, do đó phương trình gốc luôn có nghiệm với mọi $a$ lẻ.
    -   Khi $k$ chẵn, phương trình thứ nhất có nghiệm khi $2\mid s$, phương trình thứ hai có nghiệm khi $d\mid r$. Kết hợp cho ta điều kiện về thặng dư. Điều kiện đầu tương đương $a\equiv 1\pmod 4$; điều kiện thứ hai tương đương $a^{d'}=1$. Hai điều kiện cho tiêu chuẩn trong định lý. Nghiệm tổng quát của hệ là
    
        $$
        \begin{aligned}
        z &\equiv0,1\pmod 2, \\
        y &\equiv y_0 + id' \pmod{2^{e-2}},~ 0\le y_0 < 2^{e-2}.
        \end{aligned}
        $$
    
        Kết hợp lại suy ra nghiệm tổng quát của phương trình gốc.

Như vậy giải xong tiêu chuẩn thặng dư bậc $k$ cho mọi môđun. Các nội dung như ký hiệu Legendre và luật tương hỗ bậc hai có thể tổng quát hóa cho bậc cao, nhưng khó hơn, cần [trường phân chia](../algebra/field-theory.md#分圆域), và trong số học đại số được tổng quát bởi [định luật tương hỗ Artin](https://en.wikipedia.org/wiki/Artin_reciprocity).

## Căn đơn vị

Xét trường hợp đặc biệt là căn của $1$: **căn đơn vị** bậc $k$. Đây là tương ứng của căn đơn vị trong $\mathbf C$ với hệ thặng dư $\mathbf Z_m^*$ khi làm việc theo môđun. Khi môđun phù hợp, dùng căn đơn vị môđun thay cho $\omega_k$ có thể tăng tốc tính toán, ví dụ FFT.

Định nghĩa:

???+ abstract "Căn đơn vị bậc $k$ modulo $m$"
    Với môđun $m$, căn bậc $k$ của $1$ gọi là **căn đơn vị bậc $k$ modulo $m$**. Nếu $x$ là căn bậc $k$ và không là căn bậc $k'<k$, thì gọi là **căn đơn vị nguyên thủy bậc $k$** modulo $m$.

So với [nguyên căn](./primitive-root.md#原根), nguyên căn $g$ chính là căn đơn vị nguyên thủy bậc $\varphi(m)$.

Khi căn đơn vị nguyên thủy bậc $k$ tồn tại, tính chất đại số giống $\omega_k$, có thể thay thế trong tính toán. Ví dụ áp dụng vào [FFT](../poly/fft.md) cho ta [NTT](../poly/ntt.md) trên trường hữu hạn.

### Tính chất

Trong $\mathbf C$, mọi căn đơn vị đều tồn tại, nhưng trong số học thì không.

???+ note "Tính chất"
    Với môđun $m$, đặt $\lambda(m)$ là [hàm Carmichael](./primitive-root.md#carmichael-函数). Khi đó:
    
    1.  Mọi $a\perp m$ là căn đơn vị nguyên thủy bậc $\delta_m(a)$, trong đó $\delta_m(a)$ là [bậc](./primitive-root.md#阶).
    2.  Nếu $a$ là căn đơn vị bậc $k$ và $k'$ là bội của $k$, thì $a$ cũng là căn đơn vị bậc $k'$.
    3.  Nếu $a$ là căn đơn vị (nguyên thủy) bậc $k$, thì $a^{\ell}$ là căn đơn vị (nguyên thủy tương ứng) bậc $\dfrac{k}{\gcd(k,\ell)}$.
    4.  Khi $k'$ chạy qua các ước của $k$, tập căn đơn vị nguyên thủy bậc $k'$ tạo thành một phân hoạch của tập căn đơn vị bậc $k$. Với $\ell\perp k$, ánh xạ $x\mapsto x^\ell$ là song ánh giữa các căn đơn vị bậc $k$ và giữ phân hoạch, tức vẫn ánh xạ căn nguyên thủy bậc $k'$ sang căn nguyên thủy bậc $k'$.
    5.  Căn đơn vị nguyên thủy bậc $k$ tồn tại khi và chỉ khi $k\mid\lambda(m)$. Đặc biệt, căn đơn vị nguyên thủy bậc $\lambda(m)$ luôn tồn tại, gọi là **$\lambda$‑nguyên căn**.
    6.  $a$ là căn đơn vị bậc $k$ khi và chỉ khi $a^k\equiv 1\pmod{m}$ và với mọi ước nguyên tố $p\mid k$ đều có $a^{k/p}\not\equiv 1\pmod{m}$.

??? note "Chứng minh"
    Theo định nghĩa bậc, mọi $a\perp m$ là căn đơn vị nguyên thủy bậc $\delta_m(a)$; ngược lại nếu $a$ là căn đơn vị bậc $k$, thì $\gcd(a^k,m)=1$, suy ra $\gcd(a,m)=1$. Vậy $a$ là căn đơn vị (nguyên thủy) khi và chỉ khi $a\perp m$. Đây là tính chất 1.
    
    Nếu $k\mid k'$, từ $a^k\equiv 1\pmod m$ suy ra $a^{k'}\equiv 1\pmod m$, nên tính chất 2 đúng. Theo [tính chất bậc](./primitive-root.md#ord-prop-3),
    
    $$
    \delta(a^\ell) = \dfrac{\delta_m(a)}{\gcd(\delta_m(a),\ell)}.
    $$
    
    Nếu $a$ là căn đơn vị nguyên thủy bậc $k$ thì $\delta_m(a)=k$, suy ra $a^\ell$ là căn nguyên thủy bậc $\dfrac{k}{\gcd(k,\ell)}$. Nếu $a$ chỉ là căn bậc $k$, giả sử nó là căn nguyên thủy bậc $k'\mid k$, thì $a^\ell$ là căn nguyên thủy bậc $\dfrac{k'}{\gcd(k',\ell)}$. Do $k'\mid k$, ta có
    
    $$
    \dfrac{k'}{\gcd(k',\ell)} \mid \dfrac{k}{\gcd(k,\ell)},
    $$
    
    kết hợp tính chất 2 suy ra $a^\ell$ là căn bậc $\dfrac{k}{\gcd(k,\ell)}$. Đây là tính chất 3.
    
    Với $k'\mid k$, theo tính chất 2, căn nguyên thủy bậc $k'$ là căn bậc $k$. Các tập này rời nhau nên tạo phân hoạch. Với $\ell\perp k$, luôn có $\ell\perp k'$, nên căn nguyên thủy bậc $k'$ được ánh xạ tới căn nguyên thủy bậc $k'$. Lấy $\ell'=\ell^{-1}\bmod k$, ta có song ánh. Đây là tính chất 4.
    
    Theo định nghĩa Carmichael, tồn tại căn nguyên thủy bậc $\lambda(m)$. Với $k\mid\lambda(m)$, đặt $k'=\dfrac{\lambda(m)}{k}$ thì
    
    $$
    \delta_m(a^{k'}) = \dfrac{\lambda(m)}{(\lambda(m),k')} = \dfrac{\lambda(m)}{k'} = k,
    $$
    
    nên $a^{k'}$ là căn nguyên thủy bậc $k$. Ngược lại mọi bậc đều là ước của $\lambda(m)$. Đây là tính chất 5.
    
    Tính chất 6 chứng minh tương tự [tiêu chuẩn nguyên căn](./primitive-root.md#原根判定定理), thực chất là kiểm tra $\delta_m(a)=k$.

So với trường hợp có nguyên căn, $\lambda$‑nguyên căn đóng vai trò nền tảng. Khác với nguyên căn, lũy thừa của $\lambda$‑nguyên căn không sinh toàn bộ căn đơn vị. Dù vậy, mật độ $\lambda$‑nguyên căn không thấp[^lambda-density], nên có thể tìm ngẫu nhiên một $\lambda$‑nguyên căn, rồi lấy lũy thừa để có căn bậc $k$.

Nếu biết một căn bậc $k$ của $a$, có thể dùng toàn bộ căn đơn vị bậc $k$ để sinh ra tất cả căn bậc $k$.

???+ note "Định lý"
    Nếu $x$ là một căn bậc $k$ của $a$ modulo $m$, khi $r$ chạy qua toàn bộ căn đơn vị bậc $k$, thì $xr$ chạy qua toàn bộ căn bậc $k$ của $a$.

??? note "Chứng minh"
    Với hai căn $x,y$ của $a$, đặt $r=x^{-1}y\bmod m$, thì $r^k\equiv 1\pmod m$, nên $r$ là căn đơn vị bậc $k$. Ngược lại, nếu $r$ là căn đơn vị bậc $k$, thì $(xr)^k= x^kr^k\equiv a\pmod m$, nên $xr$ là căn bậc $k$.

Tương tự như nghiệm tổng quát của hệ tuyến tính không đồng nhất.

Trong trường hợp có nguyên căn, cấu trúc đơn giản hơn:

???+ note "Định lý"
    Với môđun $m$ có nguyên căn, $a$ là căn đơn vị nguyên thủy bậc $k$. Khi đó $b$ là căn đơn vị bậc $k$ khi và chỉ khi $b$ là lũy thừa của $a$.

??? note "Chứng minh"
    Với nguyên căn $g$, mọi phần tử nguyên tố cùng nhau đều là lũy thừa của $g$. Khi đó $a$ là căn nguyên thủy bậc $k$ khi và chỉ khi
    
    $$
    \delta_m(a) = \delta_m(g^{\operatorname{ind}_ga}) = \dfrac{\varphi(m)}{\gcd(\varphi(m),\operatorname{ind}_ga)} = k.
    $$
    
    Tương tự, $b$ là căn bậc $k$ khi và chỉ khi
    
    $$
    \delta_m(b) = \delta_m(g^{\operatorname{ind}_gb}) = \dfrac{\varphi(m)}{\gcd(\varphi(m),\operatorname{ind}_gb)} = k' \mid k.
    $$
    
    Do đó
    
    $$
    \gcd(\varphi(m),\operatorname{ind}_ga) \mid \gcd(\varphi(m),\operatorname{ind}_gb)\mid \operatorname{ind}_gb.
    $$
    
    Theo [phân tích đồng dư tuyến tính](./linear-equation.md), điều này tương đương phương trình
    
    $$
    (\operatorname{ind}_ga) x \equiv \operatorname{ind}_gb \pmod{\varphi(m)}
    $$
    
    có nghiệm. Lấy lũy thừa theo $g$, ta được $a^x\equiv b\pmod{m}$, tức $b$ là lũy thừa của $a$.

Định lý này cho thấy nếu có nguyên căn, tập căn đơn vị bậc $k$ là [nhóm cyclic](../algebra/group-theory.md#循环群), và căn nguyên thủy là sinh nhóm. Phần sau sẽ thấy Tonelli–Shanks lợi dụng điều này để tăng tốc phần log rời rạc.

## Khai căn theo môđun

Cuối cùng, bàn về cách tìm căn bậc $k$. Với $k=2$ có nhiều thuật toán nhanh, xem [khai căn bậc hai](./quad-residue.md#模意义下开平方). Nhưng với $k$ tổng quát, chưa có thuật toán đa thức. Phần này giới thiệu hai thuật toán: $O(m^{1/2})$ và $O(m^{1/4+\varepsilon})$. Dùng CRT có thể quy về môđun lũy thừa nguyên tố; ở đây tập trung trường hợp đó.

### Thuật toán cơ bản

Phân tích thặng dư bậc $k$ ở trên đã gợi ý cách tìm căn bậc $k$ modulo lũy thừa nguyên tố, giả sử $a\perp m$.

-   Với $m=p^e$ là lũy thừa nguyên tố lẻ, đặt $g$ là nguyên căn. Khi đó
    
    $$
    ky \equiv \operatorname{ind}_g a \pmod{\varphi(m)}.
    $$
    
    Tính $\operatorname{ind}_g a$ bằng [BSGS](./discrete-logarithm.md#大步小步算法), giải đồng dư tuyến tính, suy ra $y$, rồi $x\equiv g^y\pmod m$ là nghiệm. Có thể làm tương tự bằng cách chuyển sang log rời rạc của cơ số $g^k$.
    
    Với nguyên căn đã biết, độ phức tạp cho một nghiệm là $O(m^{1/2})$. Tìm nguyên căn có thể làm trong $o(m^{1/2})$, nên tổng vẫn $O(m^{1/2})$.

-   Với $m=2^e$, có thể viết $a\equiv (-1)^s5^r\pmod m$. $s$ xác định trong $O(1)$:
    
    $$
    s = \begin{cases}0, & a\equiv 1\pmod 4, \\ 1, & a\equiv 3\pmod 4.\end{cases}
    $$
    
    $r=\operatorname{ind}_5((-1)^sa)$ tính bằng BSGS trong $O(m^{1/2})$. Sau đó giải hệ:
    
    $$
    \begin{aligned}
    kz &\equiv s \pmod{2},\\
    ky &\equiv r \pmod{2^{e-2}}.
    \end{aligned}
    $$
    
    Nghiệm tổng quát $(z,y)$ dễ tìm, và $x=(-1)^z5^y$ là căn. Độ phức tạp vẫn $O(m^{1/2})$.

Với trường hợp vô nghiệm có thể kiểm tra bằng tiêu chuẩn ở trên trong $O(\log m)$.

Mã tham khảo (chỉ minh họa, do quá chậm):

??? example "Bài mẫu [Library Checker - Kth Root (Mod)](https://judge.yosupo.jp/problem/kth_root_mod) tham khảo"
    ```cpp
    --8<-- "docs/math/code/residue/bsgs-mod-p.cpp"
    ```

### Tonelli–Shanks cải tiến

Tổng quát hóa [Tonelli–Shanks](./quad-residue.md#tonellishanks-算法) để giải căn bậc $k$ trên lũy thừa nguyên tố. Một cách trực tiếp là Adleman–Manders–Miller[^amm], nhưng chưa đủ nhanh[^amm-comp]. Ở đây giới thiệu thuật toán cải tiến của sugarknri, Min\_25, 37zigen, có thể đạt $O(m^{1/4+\varepsilon})$.

Ý tưởng Tonelli–Shanks: đưa log rời rạc vào nhóm bậc $2^e$ để giảm độ phức tạp. Tương tự, log rời rạc trong nhóm bậc $p^e$ có thể giải nhanh hơn, nhưng vẫn $\Omega(\sqrt{p})$. Adleman–Manders–Miller tách bài toán thành nhiều log rời rạc bậc lũy thừa nguyên tố của $k$, nhưng bị chi phối bởi ước nguyên tố lớn nhất $p_\text{max}(k)$, nên vẫn $\Omega(\sqrt{p_\text{max}(k)})$. Thuật toán cải tiến tránh tính log với ước nguyên tố lớn, đưa tổng xuống $O(m^{1/4+\varepsilon})$.

#### Quy trình

Xét môđun $m$ (lũy thừa nguyên tố), giải

$$
x^k \equiv a \pmod m.
$$

Với $m=2^e$, cần $a\equiv 1\pmod{4}$, và có thể viết $a$ là lũy thừa của $g=5$. Khi xử lý $2^e$, mọi $\varphi(m)$ trong phần này thay bằng $\delta_m(5)=2^{e-2}$.

Bước 1: Quy về trường hợp số mũ chia $\varphi(m)$. Đặt $d=\gcd(k,\varphi(m))$. Nếu $a$ là thặng dư bậc $k$, thì $a$ là căn đơn vị bậc $\dfrac{\varphi(m)}{d}$. Với $\ell\perp\dfrac{\varphi(m)}{d}$, ánh xạ $x\mapsto x^\ell$ là song ánh. Lấy

$$
\ell = \left(\dfrac{k}{d}\right)^{-1}\bmod\dfrac{\varphi(m)}{d}.
$$

Nâng hai vế lên $\ell$:

$$
x^d\equiv x^{k\ell} \equiv a^{\ell} =: b \pmod{m}.
$$

Ở đây dùng

$$
k\ell = d\left(\frac{k}{d}\ell\right) = d\left(c\dfrac{\varphi(m)}{d}+1\right) \equiv d \pmod{\varphi(m)}.
$$

Bước 2: Phân tích $d$:

$$
d = \prod_{p\in\mathbf P}p^e.
$$

Từ $b=a^\ell$, lần lượt khai căn $p^e$ cho từng $p^e\neq 1$, cuối cùng nhận được căn bậc $d$ của $b$, tức căn bậc $k$ của $a$.

Bước 3: Giải

$$
x^{p^e} \equiv b \pmod m.
$$

Đặt $\varphi(m)=p^sr$, $p\perp r$. Tìm $q$ sao cho $qr\equiv -1\pmod{p^e}$. Vì $b$ là căn đơn vị bậc $rp^{s-e}$, nên $b^{qr}$ là căn đơn vị bậc $p^{s-e}$. Gọi $\zeta$ là căn đơn vị nguyên thủy bậc $p^s$. Khi đó $\zeta^{p^e}$ là căn nguyên thủy bậc $p^{s-e}$, nên tồn tại $h$ sao cho $b^{qr}\equiv \zeta^{hp^{e}}\pmod{m}$. Khi đó

$$
x\equiv b^{(qr+1)/p^e}\zeta^{-h} \pmod{m}
$$

là căn bậc $p^e$ của $b$.

Để tính $x$, cần tìm một $p$-th nonresidue $\eta$. Chỉ cần chọn ngẫu nhiên $\eta\perp m$ và kiểm tra $\eta^{\varphi(m)/p}\not\equiv 1\pmod{m}$. Mật độ là

$$
\dfrac{\varphi(m)}{m}\left(1-\dfrac{1}{p}\right) \ge \dfrac{1}{4},
$$

nên kỳ vọng thử không quá 4 số. Khi đó $\eta^{rp^{s-1}}\not\equiv 1\pmod m$ và $\eta^{rp^s}\equiv 1\pmod m$. Đặt $\zeta=\eta^r\bmod m$, $\xi=\eta^{rp^{s-1}}\bmod m$, thì $\zeta$ là căn nguyên thủy bậc $p^s$, $\xi$ là căn nguyên thủy bậc $p$.

Cuối cùng cần tính $h$. Ta có thể lấy $h < p^{s-e}$. Viết $h$ ở cơ số $p$:

$$
h = \sum_{j=0}^{s-e-1}h_jp^j = h_0 + h_1p + h_2p^2 +\cdots.
$$

Tính từng chữ số. Sau khi có $j$ chữ số đầu, ta có

$$
\left(b^{qr}\zeta^{-p^e(h_0+h_1p+\cdots + h_{j-1}p^{j-1})}\right)^{p^{s-e-j-1}} \equiv \zeta^{h_jp^{s-1}} \equiv \xi^{h_j} \pmod{m}.
$$

Do đó $h_j$ là log rời rạc theo cơ số $\xi$. Dùng BSGS. Nếu tiền xử lý $B$ lũy thừa của $\xi$, thì mỗi lần truy vấn là $O(p/B)$, tổng là

$$
O\left(B+(s-e)\dfrac{p}{B}\right).
$$

Chọn $B=\sqrt{(s-e)p}$ thì tối ưu: $O\left(\sqrt{(s-e)p}\right)$. Từ $h$ suy ra $x$.

#### Độ phức tạp

Độ phức tạp tổng là $O(m^{1/4+\varepsilon})$, giả sử nhân $O(1)$ và lũy thừa $O(\log m)$.

Xét một căn bậc $p^e$. Tìm nonresidue $O(\log m)$. Tính $s,r,\zeta,\eta,b^{qr}$ đều $O(\log m)$. Tính $h$ cần $(s-e)$ chữ số, mỗi chữ số cần $O(\log m)$ cho lũy thừa, tổng $O((s-e)\log m)$. Phần log rời rạc tổng $O\left(\sqrt{(s-e)p}\right)$. Vì $s-e=O(\log m)$, nên tổng là $O(p^{1/2+\varepsilon})$. Nếu $s=e$ thì còn $O(\log m)$.

Xét tổng toàn thuật toán: tính $\varphi(m),d,\ell$ là $O(\log m)$. Phân tích $d$ bằng [Pollard Rho](./pollard-rho.md#pollard-rho-算法) mất $O(m^{1/4})$. Cuối cùng tổng thời gian cho các $p^e$ là

$$
O\left(\sum_{e < s}p^{1/2+\varepsilon}\right).
$$

Vì với $e<s$ thì $p$ xuất hiện ít nhất 2 lần trong $\varphi(m)$, nên $p<m^{1/2}$, tổng là $O(m^{1/4+\varepsilon})$.

Thực tế không cần Pollard Rho. Chỉ cần thử chia $d$ tới $m^{1/4}$. Gọi phần còn lại là $z$. Với ước nguyên tố $p>m^{1/4}$ của $z$, ta có $\nu_p(\varphi(m))<4$. Vì chỉ cần xét

$$
1 \le e = \nu_p(d) < s = \nu_p(\varphi(m)) < 4
$$

nên $p$ lớn như vậy có nhiều nhất một. Để tách $p$, tính

$$
p^\star=\gcd\left(z,\dfrac{\varphi(m)}{z}\right) = \prod_{p : \nu_p(d) < \nu_p(\varphi(m))}p^{\min\{\nu_p(d),\nu_p(\varphi(m))-\nu_p(d)\}}.
$$

Xét các khả năng của $\nu_p(d),\nu_p(\varphi(m))$ cho thấy số mũ của $p$ là $1$, nên $p^\star$ là ước lớn duy nhất (nếu có). Phần còn lại $z/p^\star$ chỉ gồm các thừa số với $e=s$, nên không cần phân tích thêm.

Mã tham khảo:

??? example "Bài mẫu [Library Checker - Kth Root (Mod)](https://judge.yosupo.jp/problem/kth_root_mod) tham khảo"
    ```cpp
    --8<-- "docs/math/code/residue/tonelli-shanks-mod-p.cpp"
    ```

### Xử lý trường hợp tổng quát

Xét trường hợp tổng quát, môđun $m=p^e$ nhưng $\gcd(a,m)>1$. Nếu $a\equiv 0\pmod{m}$, thì

$$
x = p^{\lceil e/k \rceil}\ell\pmod{p^e},~\ell=0,1,\cdots,p^{e-\lceil e/k\rceil}-1
$$

đều là nghiệm. Xét $a\not\equiv 0\pmod{m}$. Viết $a = p^sa'$, $p\perp a'$. Đặt $x=p^zx'$, $p\perp x'$, ta có

$$
x^k = p^{kz}(x')^k\equiv p^sa'\pmod{p^e}.
$$

Vì $(x')^k\perp p$, điều này đúng khi và chỉ khi $kz = s$ và $(x')^k\equiv a'\pmod{p^{e-s}}$. Phương trình thứ nhất có nghiệm khi $k\mid s$, với $z=\dfrac{s}{k}$. Phương trình thứ hai đã giải. Lưu ý môđun khác nên mỗi nghiệm $x'$ sinh nhiều nghiệm $x$:

$$
x \equiv p^{s/k}(x' + \ell p^{e-s})\pmod{p^e},~\ell = 0,1,\cdots, p^{s-s/k}-1.
$$

Mã tham khảo tổng quát:

??? example "Bài mẫu [Luogu P5668【模板】N 次剩余](https://www.luogu.com.cn/problem/P5668) tham khảo"
    === "Thuật toán cơ bản"
        ```cpp
        --8<-- "docs/math/code/residue/bsgs.cpp"
        ```
    
    === "Tonelli–Shanks cải tiến"
        ```cpp
        --8<-- "docs/math/code/residue/tonelli-shanks.cpp"
        ```

## Tài liệu tham khảo và chú thích

-   冯克勤．初等数论及其应用．
-   [Root of unity modulo n - Wikipedia](https://en.wikipedia.org/wiki/Root_of_unity_modulo_n)
-   [No.981 一般冪乗根 解説 by 37zigen](https://yukicoder.me/problems/no/981/editorial)

[^fnnt]: Thực tế, $m$ không nhất thiết là nguyên tố. Chỉ cần $a$ là căn đơn vị nguyên thủy bậc $k=2^e$ modulo $m$ thì dùng được cho NTT. Nhưng thường $2^e$ lớn, nên mỗi thừa số nguyên tố của $m$ có dạng $c2^e+1$, dẫn đến $m$ lớn, nên ít dùng.

[^lambda-density]: Theo [kết luận về số lượng nguyên căn](./primitive-root.md#原根个数), số $\lambda$‑nguyên căn là $\varphi(\lambda(m))$. Vì với hầu hết $m$, $\lambda(m)/m = \exp(-(1+o(1))\log\log m\log\log\log m)$, và tồn tại $C>0$ sao cho $\varphi(m)/m = C / \log\log m$, nên với hầu hết $m$, $\varphi(\lambda(m))/m = \exp(-(1+o(1))\log\log m\log\log\log m)$. Hệ số $o(1)$ hấp thụ phần $\varphi(\lambda(m))/\lambda(m)$. Do đó, kỳ vọng cần $\exp((1+o(1))\log\log m\log\log\log m)$ lần thử để tìm $\lambda$‑nguyên căn. Tham khảo Rosser–Schoenfeld (1962) và Erdos–Pomerance–Schmutz (1991).

[^amm]: Bài gốc: Adleman, Leonard, Kenneth Manders, and Gary Miller. "On taking roots in finite fields." In 18th Annual Symposium on Foundations of Computer Science (sfcs 1977), pp. 175-178. IEEE Computer Society, 1977. Giới thiệu dễ đọc: Cao, Zhengjun, Qian Sha, and Xiao Fan. "Adleman-Manders-Miller root extraction method revisited." In International Conference on Information Security and Cryptology, pp. 77-85. Berlin, Heidelberg: Springer Berlin Heidelberg, 2011.

[^amm-comp]: Thuật toán yêu cầu $k$ là nguyên tố, nên trường hợp xấu nhất cần tính căn bậc $p$ với $p$ là ước nguyên tố lớn nhất của $\varphi(m)$. Khi đó cần log rời rạc trong nhóm bậc $p$, BSGS mất $O(\sqrt{p})$. Theo Fouvry (1985), tồn tại mật độ dương các nguyên tố $m$ sao cho $\varphi(m)=m-1$ có ước nguyên tố lớn nhất $p=\Omega(m^{2/3})$, nên độ phức tạp ít nhất $\Omega(m^{1/3})$, kém hơn thuật toán cải tiến.
