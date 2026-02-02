author: iamtwz, aofall, CCXXXI, CoelacanthusHex, Great-designer, Marcythm, Persdre, shuzhouliu, Tiphereth-A, Xeonacid

## Định nghĩa

???+ abstract "Phương trình đồng dư"
    Với số nguyên dương $m$ và đa thức một biến hệ số nguyên $f(x)=\sum_{i=0}^n a_ix^i$, trong đó ẩn $x\in\mathbf{Z}_m$, ta gọi phương trình
    
    $$
    f(x)\equiv 0\pmod m\tag{1}
    $$
    
    là **phương trình đồng dư** một ẩn modulo $m$ (Congruence Equation).
    
    Nếu $a_n\not\equiv 0\pmod m$ thì gọi là phương trình đồng dư bậc $n$.
    
    Tương tự có thể định nghĩa hệ phương trình đồng dư.

Về phương trình đồng dư bậc nhất và hệ phương trình, xem [phương trình đồng dư tuyến tính](./linear-equation.md) và [định lý Trung Quốc](./crt.md).

Bài viết trước hết nghiên cứu tính khả giải và cấu trúc nghiệm, sau đó giới thiệu ngắn gọn cách giải phương trình đồng dư bậc cao.

Theo [định lý Trung Quốc](./crt.md), giải phương trình modulo hợp số $m$ có thể quy về modulo lũy thừa của số nguyên tố. Do đó, dưới đây chỉ trình bày lý thuyết cho modulo lũy thừa nguyên tố và modulo nguyên tố.

## Phương trình đồng dư modulo lũy thừa nguyên tố

Giả sử $m=p^e~(p\in\mathbf{P},~e\in\mathbf{Z}_{>1})$.

Nếu $x_0$ là nghiệm của

$$
f(x)\equiv 0\pmod{p^e}
$$

thì $x_0$ cũng là nghiệm của

$$
f(x)\equiv 0\pmod{p^{e-1}}
$$

Điều này gợi ý xây nghiệm modulo bậc cao từ nghiệm modulo bậc thấp. Ta có:

<a id="定理-1"></a>

???+ note "Định lý 1 (Bổ đề Hensel)"
    Với số nguyên tố $p$ và $e>1$, đa thức hệ số nguyên $f(x)=\sum_{i=0}^na_ix^i~(p^e\nmid a_n)$, đặt đạo hàm $f'(x)=\sum_{i=1}^nia_ix^{i-1}$. Cho $x_0$ là nghiệm của
    
    $$
    f(x)\equiv 0\pmod{p^{e-1}}\tag{2}
    $$
    
    thì:
    
    1.  Nếu $f'(x_0)\not\equiv 0\pmod p$ thì tồn tại $t$ sao cho
    
        $$
        x=x_0+p^{e-1}t \tag{3}
        $$
    
        là nghiệm của
    
        $$
        f(x)\equiv 0\pmod{p^e} \tag{4}
        $$
    
    2.  Nếu $f'(x_0)\equiv 0\pmod p$ và $f(x_0)\equiv 0\pmod{p^e}$ thì với mọi $t=0,1,\dots,p-1$, $x$ từ (3) đều là nghiệm của (4).
    3.  Nếu $f'(x_0)\equiv 0\pmod p$ và $f(x_0)\not\equiv 0\pmod{p^e}$ thì không thể dùng (3) để tạo nghiệm của (4).

???+ note "Chứng minh"
    Giả sử (3) là nghiệm của (4), tức
    
    $$
    f(x_0+p^{e-1}t)\equiv 0\pmod{p^e}
    $$
    
    Suy ra
    
    $$
    f(x_0)+p^{e-1}tf'(x_0)\equiv 0\pmod{p^e}
    $$
    
    nên
    
    $$
    tf'(x_0)\equiv -\frac{f(x_0)}{p^{e-1}}\pmod p\tag{5}
    $$
    
    1.  Nếu $f'(x_0)\not\equiv 0\pmod p$ thì (5) có nghiệm duy nhất $t_0$, thay vào (3) cho nghiệm của (4).
    2.  Nếu $f'(x_0)\equiv 0\pmod p$ và $f(x_0)\equiv 0\pmod{p^e}$ thì mọi $t$ đều thỏa (5), do đó mọi $x$ từ (3) đều là nghiệm.
    3.  Nếu $f'(x_0)\equiv 0\pmod p$ và $f(x_0)\not\equiv 0\pmod{p^e}$ thì (5) vô nghiệm nên không thể dựng nghiệm của (4).

Suy ra:

<a id="推论-1"></a>

???+ note "Hệ quả 1"
    Với $p,e,f(x),x_0$ như [Định lý 1](#定理-1),
    
    1.  Nếu $s$ là nghiệm của $f(x)\equiv 0\pmod p$ và $f'(s)\not\equiv 0\pmod p$ thì tồn tại $x_s\in\mathbf{Z}_{p^e}$, $x_s\equiv s\pmod p$, sao cho $x_s$ là nghiệm của (4).
    2.  Nếu $f(x)\equiv 0\pmod p$ và $f'(x)\equiv 0\pmod p$ không có nghiệm chung thì số nghiệm của (4) bằng số nghiệm của $f(x)\equiv 0\pmod p$.

Vì vậy có thể quy về trường hợp modulo nguyên tố.

## Phương trình đồng dư modulo nguyên tố

Giả sử $p\in\mathbf{P}$, $f(x)=\sum_{i=0}^na_ix^i$ với $p\nmid a_n$, $x\in\mathbf{Z}_p$.

<a id="定理-2"></a>

???+ note "Định lý 2"
    Nếu phương trình
    
    $$
    f(x)\equiv 0\pmod p\tag{6}
    $$
    
    có $k$ nghiệm phân biệt $x_1,\dots,x_k~(k\leq n)$, thì
    
    $$
    f(x)\equiv g(x)\prod_{i=1}^k(x-x_i)\pmod p,
    $$
    
    với $\deg g=n-k$ và $[x^{n-k}]g(x)=a_n$.

???+ note "Chứng minh"
    Quy nạp theo $k$.
    
    -   $k=1$: chia đa thức, $f(x)=(x-x_1)g(x)+r$, $r\in\mathbf{Z}$. Vì $f(x_1)\equiv 0\pmod p$ nên $r\equiv 0\pmod p$, do đó $f(x)\equiv(x-x_1)g(x)\pmod p$.
    -   Giả sử đúng với $k-1$ ($k>1$). Nếu $f$ có $k$ nghiệm $x_1,\dots,x_k$ thì $f(x)\equiv(x-x_1)h(x)\pmod p$. Với $i\ge2$:
    
        $$
        0\equiv f(x_i)\equiv (x_i-x_1)h(x_i)\pmod p
        $$
    
        nên $h$ có $k-1$ nghiệm $x_2,\dots,x_k$. Theo giả thiết quy nạp:
    
        $$
        h(x)\equiv g(x)\prod_{i=2}^k(x-x_i)\pmod p,
        $$
    
        với $\deg g=n-k$ và $[x^{n-k}]g(x)=a_n$. Suy ra mệnh đề đúng.

<a id="推论-2"></a>

???+ note "Hệ quả 2"
    Với số nguyên tố $p$,
    
    -   $(\forall x\in\mathbf{Z}),~~x^{p-1}-1 \equiv \prod_{i=1}^{p-1}(x-i)\pmod p$.
    -   ([Định lý Wilson](./factorial.md#wilson-定理)) $(p-1)! \equiv -1 \pmod p$.

<a id="定理-3lagrange-定理"></a>

???+ note "Định lý 3 (Lagrange)"
    Phương trình (6) có nhiều nhất $n$ nghiệm phân biệt.

???+ note "Chứng minh"
    Giả sử có $n+1$ nghiệm $x_1,\dots,x_{n+1}$. Theo [Định lý 2](#定理-2), với $x_1,\dots,x_n$,
    
    $$
    f(x)\equiv a_n\prod_{i=1}^n(x-x_i)\pmod p
    $$
    
    Thế $x=x_{n+1}$:
    
    $$
    0\equiv f(x_{n+1})\equiv a_n\prod_{i=1}^n(x_{n+1}-x_i)\pmod p
    $$
    
    Vế phải không chia hết cho $p$, mâu thuẫn.

<a id="推论-3"></a>

???+ note "Hệ quả 3"
    Nếu phương trình $\sum_{i=0}^nb_ix^i\equiv 0\pmod p$ có nhiều hơn $n$ nghiệm thì
    
    $$
    (\forall i=0,1,\dots,n),~~p\mid b_i.
    $$

<a id="定理-4"></a>

???+ note "Định lý 4"
    Nếu (6) không có đủ $p$ nghiệm, thì tồn tại đa thức hệ số nguyên $r(x)$ với $\deg r<p$ sao cho nghiệm của $f(x)\equiv 0\pmod p$ và $r(x)\equiv 0\pmod p$ là như nhau.

???+ note "Chứng minh"
    Giả sử $n\ge p$, chia đa thức:
    
    $$
    f(x)=g(x)\left(x^p-x\right)+r(x)
    $$
    
    với $\deg r<p$.
    
    Theo [định lý Fermat nhỏ](./fermat.md), với mọi $x$ nguyên, $x^p\equiv x\pmod p$, do đó:
    
    -   Nếu $r(x)\equiv 0\pmod p$ thì theo [Hệ quả 2](#推论-2), $f(x)$ có $p$ nghiệm.
    -   Nếu $r(x)\not\equiv 0\pmod p$ thì $f(x)\equiv r(x)\pmod p$, hai phương trình có cùng nghiệm.

Định lý này dùng để hạ bậc.

<a id="定理-5"></a>

???+ note "Định lý 5"
    Với $n\leq p$, phương trình
    
    $$
    x^n+\sum_{i=0}^{n-1}a_ix^i\equiv 0\pmod p\tag{7}
    $$
    
    có $n$ nghiệm khi và chỉ khi tồn tại đa thức hệ số nguyên $q(x)$, $r(x)~(\deg r < n)$ sao cho
    
    $$
    x^p-x=f(x)q(x)+pr(x). \tag{8}
    $$

???+ note "Chứng minh"
    -   Cần: chia đa thức:
    
        $$
        x^p-x=f(x)q(x)+r_1(x).
        $$
    
        Nếu (7) có $n$ nghiệm, thì $r_1\equiv 0\pmod p$ cũng có $n$ nghiệm, theo [Hệ quả 3](#推论-3) suy ra $r_1(x)=pr(x)$. Do đó (8) đúng.
    -   Đủ: nếu (8) đúng, theo Fermat nhỏ:
    
        $$
        0\equiv x^p-x\equiv f(x)q(x)\pmod p.
        $$
    
        Suy ra $f(x)q(x)\equiv 0\pmod p$ có $p$ nghiệm. Gọi số nghiệm của (7) là $s$, theo [Lagrange](#定理-3lagrange-定理) có $s\le n$. Do $\deg q=p-n$, nên số nghiệm của $q(x)\equiv 0\pmod p$ không quá $p-n$. Vì tập nghiệm là hợp của hai tập nghiệm, ta có $s+(p-n)\ge p$, nên $s\ge n$. Vậy $s=n$.

Với đa thức không đơn vị, do $\mathbf{Z}_p$ là trường nên có thể chuẩn hóa về đơn vị để áp dụng.

<a id="定理-6"></a>

???+ note "Định lý 6"
    Nếu $n\mid p-1$, $p\nmid a$, thì
    
    $$
    x^n\equiv a\pmod p\tag{9}
    $$
    
    có nghiệm khi và chỉ khi
    
    $$
    a^{\frac{p-1}{n}}\equiv 1\pmod p.
    $$
    
    Và nếu có nghiệm thì có đúng $n$ nghiệm.

???+ note "Note"
    Cấu trúc nghiệm của (9) xem [phần dư bậc $k$](./residue.md).

???+ note "Chứng minh"
    -   Cần: nếu $x_0$ là nghiệm thì
    
        $$
        a^{\frac{p-1}{n}}\equiv {\left(x_0^n\right)}^{\frac{p-1}{n}}\equiv 1\pmod p
        $$
    -   Đủ: nếu $a^{\frac{p-1}{n}}\equiv 1\pmod p$, thì
    
        $$
        \begin{aligned}
            x^p-x&=x\left(x^{p-1}-1\right)\\
            &=x\left(\left(x^n\right)^{\frac{p-1}{n}}-a^{\frac{p-1}{n}}+a^{\frac{p-1}{n}}-1\right)\\
            &=\left(x^n-a\right)P(x)+x\left(a^{\frac{p-1}{n}}-1\right)\\
        \end{aligned}
        $$
    
        với $P(x)$ là đa thức hệ số nguyên. Theo [Định lý 5](#定理-5), (9) có $n$ nghiệm.

## Cách giải phương trình đồng dư bậc cao (và hệ)

Dùng [CRT](./crt.md) để chuyển **hệ phương trình đồng dư** thành **phương trình đồng dư**, và chuyển modulo **hợp số** $m$ thành modulo **lũy thừa nguyên tố**. Sau đó dùng [Định lý 1](#定理-1) để chuyển về modulo **nguyên tố**.

Khi đó chỉ cần xét

$$
x^n+\sum_{i=0}^{n-1}a_ix^i\equiv 0\pmod p
$$

với $p$ nguyên tố, $n<p$.

Có thể thay $x\mapsto x-\dfrac{a_{n-1}}{n}$ để khử hạng $x^{n-1}$, nên xét

$$
x^n+\sum_{i=0}^{n-2}a_ix^i\equiv 0\pmod p\tag{10}
$$

với $p$ nguyên tố, $n<p$.

-   $n=1$: xem [phương trình đồng dư tuyến tính](./linear-equation.md).
-   $n=2$: xem [phần dư bậc hai](./quad-residue.md).
-   Nếu (10) quy về
    
    $$
    x^n\equiv a\pmod p,
    $$
    
    thì xem [phần dư bậc $k$](./residue.md).

## Tài liệu tham khảo

1.  [Congruence Equation -- from Wolfram MathWorld](https://mathworld.wolfram.com/CongruenceEquation.html)
2.  [Lagrange's theorem (number theory) - Wikipedia](https://en.wikipedia.org/wiki/Lagrange%27s_theorem_%28number_theory%29)
3.  潘承洞，潘承彪．初等数论．
4.  冯克勤．初等数论及其应用．
5.  闵嗣鹤，严士健．初等数论．
