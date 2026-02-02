Kiến thức trước: [Định lý nhỏ Fermat](./fermat.md#费马小定理)、[Định lý Euler](./fermat.md#欧拉定理)、[Định lý Lagrange](./congruence-equation.md#定理-3lagrange-定理)

Khái niệm **bậc** và **nguyên căn** là công cụ quan trọng để hiểu cấu trúc nhân của [hệ thặng dư tối giản](./basic.md#同余类与剩余系) $\mathbf Z_m^*$ theo modulo $m$. Từ đó có thể định nghĩa các khái niệm như [logarit rời rạc](./discrete-logarithm.md). Thảo luận tổng quát hơn có thể xem ở phần đại số trừu tượng: [lý thuyết nhóm](../algebra/group-theory.md#阶) và [lý thuyết vành](../algebra/ring-theory.md#应用整数同余类的乘法群), cùng các mục liên quan.

## Bậc

Trong mục này, luôn giả sử mô-đun $m\in\mathbf N_+$ và cơ số $a\in\mathbf Z$ nguyên tố cùng nhau, tức $(a,m)=1$, cũng ký hiệu $a\perp m$.

Với $n\in\mathbf Z$, các lũy thừa $a^n\bmod m$ có một cấu trúc tuần hoàn. Độ dài chu kỳ nhỏ nhất chính là bậc của $a$ modulo $m$. Bậc được định nghĩa là số mũ khi $a^n \bmod m$ quay về điểm xuất phát $a^0\bmod m = 1$ lần đầu:

???+ abstract "Bậc"
    Với $a\in\mathbf Z,m\in\mathbf N_+$ và $a\perp m$, số nguyên dương nhỏ nhất $n$ thỏa $a^n \equiv 1 \pmod m$ được gọi là **bậc của $a$ modulo $m$** (the order of $a$ modulo $m$), ký hiệu $\delta_m(a)$ hoặc $\operatorname{ord}_m(a)$.

???+ tip "Ghi chú"
    Trong [đại số trừu tượng](../algebra/group-theory.md#阶), “bậc” ở đây chính là bậc của phần tử $a$ trong nhóm nhân của hệ thặng dư tối giản modulo $m$. Ký hiệu $\delta$ chỉ dùng cho nhóm đặc biệt này. Nhiều tính chất dưới đây có thể trực tiếp tổng quát hóa cho bậc phần tử trong nhóm nói chung.

    Ngoài ra còn có khái niệm “bán bậc”, trong số học ký hiệu $\delta^-$. Đó là số nguyên dương nhỏ nhất thỏa $a^n \equiv -1 \pmod m$. Bán bậc không phải khái niệm trong lý thuyết nhóm. Bậc luôn tồn tại, bán bậc thì không nhất thiết.

### Cấu trúc tuần hoàn của lũy thừa

Nhờ bậc, ta có thể mô tả cấu trúc tuần hoàn của lũy thừa. Với $a^n\bmod m$, viết $n$ theo phép chia có dư bởi $\delta_m(a)$:

$$
n = \delta_m(a)q + r, ~ 0\le r < \delta_m(a). 
$$

Suy ra theo quy tắc lũy thừa:

$$
a^n = a^{\delta_m(a)q + r} = (a^{\delta_m(a)})^q \cdot a^r \equiv a^r \pmod m.
$$

Điều này cho thấy mọi lũy thừa có thể “tịnh tiến” về chu kỳ đầu tiên. Từ đó suy ra một loạt tính chất của bậc.

<a id="ord-prop-1"></a>

???+ note "Tính chất 1"
    Với $a\in\mathbf Z,m\in\mathbf N_+$ và $a\perp m$, các lũy thừa $a^0(=1),a,a^2,\cdots,a^{\delta_m(a)-1}$ modulo $m$ đôi một không đồng dư.

??? note "Chứng minh"
    Chứng minh phản chứng. Giả sử tồn tại $0\le i< j<\delta_m(a)$ sao cho $a^i\equiv a^j\pmod m$, khi đó $a^{j - i}\equiv 1\pmod m$. Nhưng $0 < j - i < \delta_m(a)$, mâu thuẫn với tính nhỏ nhất của bậc. Do đó mệnh đề đúng.

<a id="ord-prop-2"></a>

???+ note "Tính chất 2"
    Với $a,n\in\mathbf Z,m\in\mathbf N_+$ và $a\perp m$, quan hệ $a^n \equiv 1 \pmod m$ đúng khi và chỉ khi $\delta_m(a)\mid n$.

??? note "Chứng minh"
    Như trên, $a^{n}\equiv a^{n\bmod\delta_m(a)}\pmod m$. Theo [Tính chất 1](#ord-prop-1), trong $0\le r < \delta_m(a)$ chỉ có duy nhất $r=0$ khiến $a^r\equiv 1\pmod m$. Vì vậy $a^n \equiv 1 \pmod m$ khi và chỉ khi $n\bmod \delta_m(a) = 0$, tức $\delta_m(a)\mid n$.

Trong [định lý Euler](./fermat.md#欧拉定理), $a^{\varphi(m)}\equiv 1\pmod m$ đúng với mọi $a\perp m$. Kết hợp [Tính chất 2](#ord-prop-2) suy ra với mọi $a\perp m$ đều có $\delta_m(a)\mid\varphi(m)$. Nói cách khác, $\varphi(m)$ là một bội chung của mọi bậc $\delta_m(a)$. Với số nguyên dương $m$, bội chung nhỏ nhất của tất cả các bậc $\delta_m(a)$ (với $a\perp m$) ký hiệu $\lambda(m)$, chính là [hàm Carmichael](#carmichael-函数). Phần sau sẽ bàn kỹ.

Cũng như các cấu trúc tuần hoàn khác, có thể suy ra bậc của $a^k$ từ bậc của $a$.

<a id="ord-prop-3"></a>

???+ note "Tính chất 3"
    Với $k,a\in\mathbf Z,m\in\mathbf N_+$ và $a\perp m$, ta có

    $$
    \delta_m(a^k) = \dfrac{\delta_m(a)}{(\delta_m(a),k)}.
    $$

??? note "Chứng minh"
    Theo [Tính chất 2](#ord-prop-2), $(a^k)^n = a^{kn} \equiv 1\pmod m$ khi và chỉ khi $\delta_m(a) \mid kn$. Điều này tương đương

    $$
    \dfrac{\delta_m(a)}{\left(\delta_m(a),k\right)} \mid n.
    $$

    Số nguyên dương nhỏ nhất thỏa điều kiện là

    $$
    \delta_m(a^k)=\dfrac{\delta_m(a)}{\left(\delta_m(a),k\right)}.
    $$

### Bậc của tích

Giả sử $a,b$ là hai số nguyên khác nhau, đều nguyên tố cùng nhau với $m$. Nếu biết $\delta_m(a)$ và $\delta_m(b)$, ta có thể suy ra thông tin về bậc của tích $ab$, ký hiệu $\delta_{m}(ab)$.

<a id="ord-prop-4"></a>

???+ note "Tính chất 4"
    Với $a,b\in\mathbf Z,m\in\mathbf N_+$ và $a,b\perp m$, ta có

    $$
    \dfrac{[\delta_m(a),\delta_m(b)]}{(\delta_m(a),\delta_m(b))} \mid \delta_m(ab) \mid [\delta_m(a),\delta_m(b)].
    $$

??? note "Chứng minh"
    Vì $[\delta_m(a),\delta_m(b)]$ là bội của $\delta_m(a)$ và $\delta_m(b)$, theo [Tính chất 2](#ord-prop-2) suy ra

    $$
    (ab)^{[\delta_m(a),\delta_m(b)]} = a^{[\delta_m(a),\delta_m(b)]} b^{[\delta_m(a),\delta_m(b)]} \equiv 1 \pmod m.
    $$

    Áp dụng Tính chất 2 lần nữa, ta được

    $$
    \delta_m(ab) \mid [\delta_m(a),\delta_m(b)].
    $$

    Đây là quan hệ chia hết phía phải.

    Mặt khác, do

    $$
    1 \equiv (ab)^{\delta_m(ab)\delta_m(b)} \equiv a^{\delta_m(ab)\delta_m(b)} \pmod m,
    $$

    nên theo Tính chất 2 có $\delta_m(a)\mid\delta_m(ab)\delta_m(b)$. Khử $(\delta_m(a),\delta_m(b))$ hai vế, ta được

    $$
    \dfrac{\delta_m(a)}{(\delta_m(a),\delta_m(b))}\mid\delta_m(ab)\dfrac{\delta_m(b)}{(\delta_m(a),\delta_m(b))}.
    $$

    Hai phân số vế trái và phải nguyên tố cùng nhau, nên suy ra

    $$
    \dfrac{\delta_m(a)}{(\delta_m(a),\delta_m(b))}\mid\delta_m(ab).
    $$

    Tương tự,

    $$
    \dfrac{\delta_m(b)}{(\delta_m(a),\delta_m(b))}\mid\delta_m(ab).
    $$

    Vì hai số ở vế trái nguyên tố cùng nhau, nên

    $$
    \dfrac{[\delta_m(a),\delta_m(b)]}{(\delta_m(a),\delta_m(b))} =\dfrac{\delta_m(a)\delta_m(b)}{(\delta_m(a),\delta_m(b))^2}\mid\delta_m(ab).
    $$

    Đây là quan hệ chia hết phía trái.

Với trường hợp bậc của $a$ và $b$ nguyên tố cùng nhau, kết quả có dạng đơn giản hơn.

<a id="ord-prop-4p"></a>

???+ note "Tính chất 4'"
    Với $a,b\in\mathbf Z,m\in\mathbf N_+$ và $a,b\perp m$, ta có

    $$
    \delta_m(ab) = \delta_m(a)\delta_m(b) \iff \delta_m(a)\perp\delta_m(b).
    $$

??? note "Chứng minh"
    Nếu $\delta_m(a)\perp\delta_m(b)$ thì trong [Tính chất 4](#ord-prop-4), mọi quan hệ chia hết đều là đẳng thức, nên

    $$
    \delta_m(ab) = [\delta_m(a),\delta_m(b)] = \delta_m(a)\delta_m(b).
    $$

    Ngược lại, nếu $\delta_m(ab)=\delta_m(a)\delta_m(b)$, thì theo Tính chất 4,

    $$
    \delta_m(a)\delta_m(b) = \delta_m(ab) \mid [\delta_m(a),\delta_m(b)].
    $$

    Suy ra $(\delta_m(a),\delta_m(b))=1$, tức $\delta_m(a)\perp\delta_m(b)$.

Trong trường hợp tổng quát, các cận trong [Tính chất 4](#ord-prop-4) là chặt. Dễ dựng ví dụ khi bậc của tích đạt cận dưới: chẳng hạn $(a,b,m)=(3,5,7)$ thì $\delta_m(a)=\delta_m(b)=6$, nhưng $\delta_m(ab)=1$.

Dù trong trường hợp chung, bậc của tích $ab$ chưa chắc bằng bội chung nhỏ nhất của hai bậc, nhưng luôn tồn tại một phần tử có bậc đúng bằng bội chung nhỏ nhất đó.

<a id="ord-prop-5"></a>

???+ note "Tính chất 5"
    Với $a,b\in\mathbf Z,m\in\mathbf N_+$ và $a,b\perp m$, luôn tồn tại $c\in\mathbf Z$ với $c\perp m$ sao cho

    $$
    \delta_m(c) = [\delta_m(a),\delta_m(b)].
    $$

??? note "Chứng minh"
    Xét phân tích thừa số nguyên tố:

    $$
    \delta_m(a) = \prod_p p^{\alpha_p},~ \delta_m(b) = \prod_p p^{\beta_p}.
    $$

    Dựa trên so sánh $\alpha_p$ và $\beta_p$, chia các thừa số nguyên tố thành hai nhóm:

    $$
    A = \{p : \alpha_p \ge \beta_p\}, ~ B = \{p : \alpha_p < \beta_p\}.
    $$

    Từ đó đặt

    $$
    \gamma_A = \prod_{p\in A}p^{\alpha_p},~\gamma_B = \prod_{p\in B}p^{\alpha_p},~\eta_A = \prod_{p\in A}p^{\beta_p},~\eta_B = \prod_{p\in B}p^{\beta_p},
    $$

    suy ra $\delta_m(a) = \gamma_A\gamma_B$ và $\delta_m(b)=\eta_A\eta_B$. Theo [Tính chất 3](#ord-prop-3),

    $$
    \begin{aligned}
    \delta_m(a^{\gamma_B}) &= \dfrac{\delta_m(a)}{(\delta_m(a),\gamma_B)} = \dfrac{\delta_m(a)}{\gamma_B} = \gamma_A,\\
    \delta_m(b^{\eta_A}) &= \dfrac{\delta_m(b)}{(\delta_m(b),\eta_A)} = \dfrac{\delta_m(b)}{\eta_A} = \eta_B.
    \end{aligned}
    $$

    Vì $\gamma_A\perp\eta_B$, theo [Tính chất 4'](#ord-prop-4p),

    $$
    \delta_m(a^{\gamma_B}b^{\eta_A}) = \gamma_A\eta_B = \prod_p p^{\max\{\alpha_p,\beta_p\}} = [\delta_m(a),\delta_m(b)].
    $$

    Do đó $c=a^{\gamma_B}b^{\eta_A}$ có bậc đúng bằng $[\delta_m(a),\delta_m(b)]$.

Kết luận này thường dùng để dựng phần tử có bậc cho trước.

## Nguyên căn

Nguyên căn là những phần tử đặc biệt — bậc của nó bằng số phần tử của hệ thặng dư tối giản modulo $m$.

???+ abstract "Nguyên căn"
    Với $m\in\mathbf N_+$, nếu tồn tại $g\in\mathbf Z$ với $g\perp m$ sao cho $\delta_m(g)=|\mathbf Z_m^*|=\varphi(m)$, thì gọi $g$ là **nguyên căn modulo $m$** (primitive root modulo $m$). Ở đây $\varphi(m)$ là [hàm Euler](./euler-totient.md).

Không phải mọi số nguyên dương $m$ đều có nguyên căn. Theo [Tính chất 1](#ord-prop-1), nếu nguyên căn $g$ modulo $m$ tồn tại, thì các lớp đồng dư chứa $g,g^2,\cdots,g^{\varphi(m)}$ đôi một khác nhau, tạo thành hệ thặng dư tối giản modulo $m$. Đặc biệt, với số nguyên tố $p$, các số dư $g^i\bmod p$ với $i=1,2,\cdots,p-1$ đôi một khác nhau.

???+ tip "Ghi chú"
    Trong [đại số trừu tượng](../algebra/ring-theory.md#应用整数同余类的乘法群), nguyên căn chính là phần tử sinh của nhóm cyclic. Khái niệm “nguyên căn” chỉ dùng cho nhóm nhân của hệ thặng dư tối giản modulo $m$; trong các nhóm cyclic nói chung, thường gọi là “phần tử sinh”. Không phải mọi nhóm nhân modulo $m$ đều là cyclic; có nguyên căn nghĩa là nhóm đó đẳng cấu với nhóm cyclic, không có nguyên căn thì không đẳng cấu.

Khi mô-đun là $1$, nhóm nhân modulo $1$ là $\{0\}$. Đây là nhóm cyclic, nên nguyên căn là $0$.

### Định lý kiểm tra nguyên căn

Nếu biết phân tích thừa số nguyên tố của $\varphi(m)$, có thể dễ dàng kiểm tra $g$ có phải là nguyên căn modulo $m$ hay không.

???+ note "Định lý"
    Với số nguyên $m\ge 3$ và $g\perp m$, thì $g$ là nguyên căn modulo $m$ khi và chỉ khi với mỗi ước nguyên tố $p$ của $\varphi(m)$, ta có

    $$
    g^{\frac{\varphi(m)}{p}}\not\equiv 1 \pmod m.
    $$

??? note "Chứng minh"
    Tính cần thiết là hiển nhiên. Để chứng minh đủ, dùng phản chứng. Nếu $g$ không phải nguyên căn modulo $m$, thì $\delta_m(g)< \varphi(m)$. Theo [Tính chất 2](#ord-prop-2) và định lý Euler, $\delta_m(g)\mid\varphi(m)$. Lấy $p$ là một ước nguyên tố của $\dfrac{\varphi(m)}{\delta_m(g)}$, suy ra $\delta_m(g)\mid\dfrac{\varphi(m)}{p}$. Áp dụng Tính chất 2 lần nữa:

    $$
    g^{\frac{\varphi(m)}{p}} \equiv 1 \pmod m.
    $$

    Nhưng $p$ cũng là ước của $\varphi(m)$, mâu thuẫn với điều kiện giả thiết. Do đó, tính đủ được chứng minh.

### Số lượng nguyên căn

Khi nguyên căn tồn tại, nó không nhất thiết là duy nhất. Nói chung, về các bậc có thể và số phần tử có bậc cho trước trong hệ thặng dư tối giản, có kết luận sau:

???+ note "Định lý"
    Nếu số nguyên dương $m$ có nguyên căn $g$, thì phần tử bậc $d$ modulo $m$ tồn tại khi và chỉ khi $d\mid\varphi(m)$, và có đúng $\varphi(d)$ phần tử như vậy. Đặc biệt, số nguyên căn modulo $m$ là $\varphi(\varphi(m))$.

??? note "Chứng minh"
    Theo định nghĩa nguyên căn, mọi lớp đồng dư modulo $m$ đều có dạng $g^k\bmod m$, với $k$ là một trong $1,2,\cdots,\varphi(m)$. Theo [Tính chất 3](#ord-prop-3), bậc các phần tử này là

    $$
    \delta_m(g^k) = \dfrac{\varphi(m)}{(\varphi(m),k)}.
    $$

    Do đó, tồn tại phần tử bậc $d$ khi và chỉ khi $d\mid\varphi(m)$. Hơn nữa, với $d\mid\varphi(m)$, đặt $d'=\varphi(m)/d$, ta có tập các phần tử đó là

    $$
    \begin{aligned}
    A &= \{g^k : (\varphi(m),k)=d',~1\le k \le\varphi(m)\} \\
    &= \{g^k : d'\mid k,~ (d, k/d') = 1,~ 1 \le k/d' \le d\}.
    \end{aligned}
    $$

    Các $k'=k/d'$ đúng là những số nguyên dương không vượt quá $d$ và nguyên tố cùng nhau với $d$, số lượng là $\varphi(d)$.

### Định lý tồn tại nguyên căn

Trong mục này sẽ chứng minh định lý tồn tại nguyên căn sau:

???+ note "Định lý"
    Nguyên căn modulo $m$ tồn tại khi và chỉ khi $m=1,2,4,p^e,2p^e$, trong đó $p$ là số nguyên tố lẻ và $e\in\mathbf N_+$.

Để giải thích kết luận này, ta xét bốn trường hợp:

1.  $m=1,2,4$, nguyên căn lần lượt là $g=0,1,3$, hiển nhiên tồn tại.

2.  $m=p^{e}$ là lũy thừa của số nguyên tố lẻ $p$, với $e\in\mathbf N_+$.

    ???+ note "Bổ đề 1"
        Với số nguyên tố lẻ $p$, nguyên căn modulo $p$ tồn tại.

    ??? note "Chứng minh"
        Chứng minh gồm hai bước.

        **Bước 1**: Với $d\mid(p-1)$, phương trình đồng dư $x^d\equiv 1\pmod p$ có đúng $d$ nghiệm đôi một khác nhau.

        Đặt $p-1=kd$, xét đa thức

        $$
        f(x) = x^{d(k-1)} + x^{d(k-2)} + \cdots + x^d + 1. 
        $$

        Theo [định lý Euler](./fermat.md#欧拉定理), phương trình $(x^d-1)f(x)=x^{p-1}-1\equiv 0\pmod{p}$ có đúng $p-1$ nghiệm khác nhau. Các nghiệm này là nghiệm của $x^d-1$ hoặc $f(x)$. Theo [định lý Lagrange](./congruence-equation.md#定理-3lagrange-定理), chúng lần lượt có nhiều nhất $d$ và $d(k-1)$ nghiệm. Vì $d+d(k-1)=p-1$, nên $x^d\equiv 1\pmod p$ có đúng $d$ nghiệm.

        **Bước 2**: Với $d\mid(p-1)$, số phần tử bậc $d$ đúng bằng $\varphi(d)$.

        Sắp các ước của $\varphi(p)$ theo thứ tự và dùng quy nạp. Bậc $1$ chỉ có phần tử $1$, nên cơ sở quy nạp đúng. Với $d\mid(p-1)$, theo [Tính chất 2](#ord-prop-2), mọi nghiệm của $x^d\equiv 1\pmod p$ đều có bậc chia hết cho $d$. Do đó số phần tử bậc $d$ là

        $$
        N(d) = d - \sum_{e\mid d,~e\neq d} N(e) =  d - \sum_{e\mid d,~e\neq d} \varphi(e) = \varphi(d).
        $$

        Đẳng thức thứ hai dùng giả thiết quy nạp, đẳng thức thứ ba là tính chất của hàm Euler. Suy ra với mọi $d\mid(p-1)$ có đúng $\varphi(d)$ phần tử bậc $d$.

        Đặc biệt, với $d=p-1$ có đúng $\varphi(p-1)$ phần tử bậc $p-1$. Vậy nguyên căn modulo $p$ tồn tại.

    ???+ note "Bổ đề 2"
        Với số nguyên tố lẻ $p$ và $e \in \mathbf{N}_+$, nguyên căn modulo $p^e$ tồn tại.

    ??? note "Chứng minh"
        Chứng minh gồm ba bước.

        **Bước 1**: Tồn tại nguyên căn modulo $p$ là $g$, sao cho $g^{p-1}\not\equiv 1\pmod{p^2}$.

        Chọn bất kỳ nguyên căn modulo $p$ là $g$. Nếu $g^{p-1}\equiv 1\pmod{p^2}$, ta chứng minh $g+p$ thỏa điều kiện: $g+p$ cũng là nguyên căn modulo $p$ và

        $$
        \begin{aligned}
        (g+p)^{p-1} &\equiv \binom{p-1}{0}g^{p-1} + \binom{p-1}{1}g^{p-2}p \\
        &= g^{p-1} + g^{p-2}p(p-1) \\ 
        &\equiv 1 - pg^{p-2} \not\equiv 1 \pmod{p^2}.
        \end{aligned}
        $$

        **Bước 2**: Với $g$ đã chọn, với mọi $e\ge 1$ đều có $g^{\varphi(p^e)}\not\equiv 1\pmod{p^{e+1}}$.

        Khi $e=1$ điều này đúng theo Bước 1. Giả sử đúng với $e$, chứng minh cho $e+1$. Với $e \ge 1$, theo định lý Euler tồn tại $\lambda$ sao cho

        $$
        g^{\varphi(p^e)} = 1 + \lambda p^e
        $$

        đúng. Theo giả thiết quy nạp, $\lambda\perp p$. Vì $\varphi(p^{e+1})=p\varphi(p^e)$, nên

        $$
        g^{\varphi(p^{e+1})} = \left(g^{\varphi(p^{e})}\right)^p = (1 + \lambda p^e)^p \equiv 1 + \lambda p^{e+1} \pmod{p^{e+2}}.
        $$

        Với $\lambda\perp p$ suy ra $g^{\varphi(p^{e+1})}\not\equiv 1\pmod{p^{e+2}}$. Quy nạp hoàn tất.

        **Bước 3**: Với $g$ đã chọn, với mọi $e\ge 1$ thì $g$ là nguyên căn modulo $p^e$.

        Với $e=1$ hiển nhiên đúng. Giả sử đúng với $e$, chứng minh với $e+1$. Gọi $\delta=\delta_{p^{e+1}}(g)$. Vì $g^\delta\equiv 1\pmod{p^{e+1}}$ nên $g^\delta\equiv 1\pmod{p^e}$. Theo giả thiết quy nạp, $\delta_{p^e}(g) = \varphi(p^e)$, do đó theo [Tính chất 2](#ord-prop-2) suy ra $\varphi(p^e)\mid\delta$. Mặt khác, theo định lý Euler, $\delta\mid\varphi(p^{e+1})$, mà $\varphi(p^{e+1})=p\varphi(p^e)$. Vậy chỉ có hai khả năng: $\delta=\varphi(p^e)$ hoặc $\delta=\varphi(p^{e+1})$. Bước 2 cho thấy $g^{\varphi(p^e)}\not\equiv 1\pmod{p^{e+1}}$, nên không thể là $\delta=\varphi(p^e)$. Do đó $\delta=\varphi(p^{e+1})$, nghĩa là $g$ là nguyên căn modulo $p^{e+1}$. Quy nạp xong.

3.  $m=2p^{e}$, với $p$ là số nguyên tố lẻ, $e\in\mathbf N_+$.

    ???+ note "Bổ đề 3"
        Với số nguyên tố lẻ $p$ và $e \in \mathbf{N}_+$, nguyên căn modulo $2p^e$ tồn tại.

    ??? note "Chứng minh"
        Gọi $g$ là nguyên căn modulo $p^{e}$, thì $g+p^e$ cũng là nguyên căn modulo $p^{e}$. Trong hai số đó có một số lẻ; không mất tính tổng quát, giả sử $g$ là số lẻ. Khi đó $(g,2p^e)=1$. Đặt $\delta=\delta_{2p^e}(g)$, cần chứng minh $\delta=\varphi(2p^e)$. Theo định lý Euler, $\delta\mid\varphi(2p^e)$. Mặt khác, từ $g^\delta\equiv 1\pmod{2p^e}$ suy ra $g^\delta\equiv 1\pmod{p^e}$, nên theo [Tính chất 2](#ord-prop-2) và lựa chọn $g$ có $\delta_{p^e}(g)=\varphi(p^e)\mid \delta$. Do công thức của hàm Euler, $\varphi(2p^e) = \varphi(p^e)$. Vậy $\delta=\delta_{2p^e}(g)=\varphi(p^e)$. Suy ra $g$ là nguyên căn modulo $2p^e$.

4.  $m\ne 1,2,4,p^{e},2p^{e}$, với $p$ là số nguyên tố lẻ, $e\in\mathbf N_+$.

    <a id="prim-root-lem-4"></a>

    ???+ note "Bổ đề 4"
        Giả sử $m\neq 1,2,4$ và không tồn tại số nguyên tố lẻ $p$ cùng số nguyên dương $e$ sao cho $m=p^e$ hoặc $m=2p^e$. Khi đó nguyên căn modulo $m$ không tồn tại.

    ??? note "Chứng minh"
        Với $m=2^e$ và $e\ge 3$, giả sử nguyên căn $g$ modulo $m$ tồn tại. Vì $g\perp m$ nên $g$ là số lẻ. Đặt $g=2k+1$ với $k\in\mathbf N$, khi đó

        $$
        \begin{aligned}
        g^{2^{e-2}}
        &=(2k+1)^{2^{e-2}} \\
        &\equiv 1 + \binom{2^{e-2}}{1}(2k) + \binom{{2^{e-2}}}{2}(2k)^2 \\
        &= 1 + 2^{e-1}k + 2^{e-1}(2^{e-2}-1)k^2 \\
        &= 1 + 2^{e-1}(k + (2^{e-2}-1)k^2) \\
        &\equiv 1 \pmod{2^{e}}.
        \end{aligned}
        $$

        Ở dòng áp chót, vì $k$ và $(2^{e-2}-1)k^2$ cùng tính chẵn lẻ nên tổng của chúng là số chẵn. Theo định nghĩa bậc, $\delta_{2^{e}}(g)\le 2^{e-2}< \varphi(2^{e}) = 2^{e-1}$. Mâu thuẫn với giả thiết $g$ là nguyên căn. Do đó không tồn tại nguyên căn.

        Giả sử $m$ thỏa điều kiện nêu trên và không phải lũy thừa của $2$, khi đó tồn tại $2 < m_1 < m_2$ và $m_1\perp m_2$ sao cho $m=m_1m_2$. Giả sử nguyên căn $g$ modulo $m$ tồn tại. Vì $g\perp m$ nên với $i=1,2$ đều có $g\perp m_i$. Theo định lý Euler,

        $$
        g^{\varphi(m_i)} \equiv 1 \pmod{m_i}.
        $$

        Vì $m_i > 2$ nên $\varphi(m_i)$ là số chẵn, do đó với $i=1,2$,

        $$
        g^{\frac{1}{2}\varphi(m_1)\varphi(m_2)} \equiv 1 \pmod{m_i}.
        $$

        Theo [định lý phần dư Trung Hoa](./crt.md),

        $$
        g^{\frac{1}{2}\varphi(m_1)\varphi(m_2)} \equiv 1 \pmod{m}.
        $$

        Lại có $\varphi(m)=\varphi(m_1)\varphi(m_2)$, nên theo định nghĩa bậc

        $$
        \delta_m(g) \le \frac{1}{2}\varphi(m_1)\varphi(m_2) = \dfrac{1}{2}\varphi(m) < \varphi(m).
        $$

        Mâu thuẫn với giả thiết $g$ là nguyên căn modulo $m$. Do đó không tồn tại nguyên căn.

Tổng hợp bốn bổ đề trên, ta có điều kiện cần và đủ để một số tồn tại nguyên căn.

### Thuật toán tìm nguyên căn

Với mọi mô-đun $m$ có nguyên căn, để tìm nguyên căn $g$ chỉ cần duyệt các số nguyên dương khả dĩ và kiểm tra từng số. Có hai cách: duyệt tăng dần hoặc sinh ngẫu nhiên. Hai cách này có hiệu năng tương đương trong thực tế.

Khi duyệt tăng dần, ta thu được nguyên căn nhỏ nhất $g_m$. Vì vậy độ phức tạp phần duyệt phụ thuộc vào $g_m$. Có các ước lượng sau:

-   Cận trên: Wang Yuan[^yuan1959note] và Burgess[^burgess1962character] chứng minh với số nguyên tố $p$ thì nguyên căn nhỏ nhất $g_p=O\left(p^{0.25+\epsilon}\right)$, với $\epsilon>0$. Cohen, Odoni và Stothers[^cohen1974least] cùng Elliott và Murata[^elliott1998least] lần lượt chứng minh ước lượng này đúng với mô-đun $p^2$ và $2p^2$, trong đó $p$ là số nguyên tố lẻ. Vì với $e>2$, nguyên căn của $p^2$ (hoặc $2p^2$) cũng là nguyên căn của $p^e$ (hoặc $2p^e$), nên cận trên $O\left(p^{0.25+\epsilon}\right)$ đúng cho mọi trường hợp.
-   Cận dưới: Fridlander[^fridlender1949least] và Salié[^salie1949kleinsten] chứng minh tồn tại hằng số $C>0$ sao cho với vô hạn số nguyên tố $p$ có $g_p > C\log p$.
-   Trường hợp trung bình: Burgess và Elliott[^burgess1968average] chứng minh trung bình thì $g_p=O((\log p)^2(\log\log p)^4)$. Elliott và Murata[^elliott1997average] còn suy đoán giá trị trung bình của $g_p$ là một hằng số, được kiểm chứng số liệu[^more-evidence] khoảng $4.926$. Sau đó Elliott và Murata[^elliott1998least] mở rộng suy đoán này cho mô-đun $2p^2$.

Từ các phân tích trên, việc vét cạn tìm nguyên căn nhỏ nhất có độ phức tạp $O(g_m(\log m)^2)$ là chấp nhận được.

Ngoài việc duyệt tăng dần, ta có thể sinh ngẫu nhiên số nguyên và kiểm tra nguyên căn. Mật độ nguyên căn không nhỏ: [^density-prim-root]

$$
\dfrac{\varphi(\varphi(m))}{m} = \Omega\left(\dfrac{1}{\log\log m}\right).
$$

Do đó, kỳ vọng độ phức tạp phần duyệt là $O((\log m)^2\log\log m)$.

Cần lưu ý: kiểm tra nguyên căn cần biết phân tích thừa số nguyên tố của $\varphi(m)$. Trong các thuật toán phân tích thừa số thường dùng trong thi đấu (xem [Pollard Rho](./pollard-rho.md)), độ phức tạp tốt nhất vẫn là $O(m^{1/4+\varepsilon})$. Vì vậy khi chưa có phân tích thừa số của $\varphi(m)$, “nút thắt” về độ phức tạp nằm ở bước phân tích thừa số, không phải ở phần duyệt kiểm tra.

## Hàm Carmichael

So với khái niệm bậc của một phần tử modulo $m$, hàm Carmichael là khái niệm toàn cục: nó là chu kỳ chung nhỏ nhất của mọi lũy thừa các số nguyên tố cùng nhau với $m$.

???+ abstract "Hàm Carmichael"
    Với $m\in\mathbf N_+$, định nghĩa $\lambda(m)$ là số nguyên dương nhỏ nhất sao cho với mọi $a\perp m$ đều có $a^n\equiv 1\pmod m$. Hàm $\lambda:\mathbf N_+\to\mathbf N_+$ được gọi là **hàm Carmichael**.

Theo [Tính chất 2](#ord-prop-2), điều kiện $a^n\equiv 1\pmod m$ với mọi $a\perp m$ tương đương với $\delta_m(a)\mid n$ với mọi $a\perp m$. Nghĩa là $n$ phải là bội chung của tất cả $\delta_m(a)$. Do đó, số nhỏ nhất như vậy là bội chung nhỏ nhất:

$$
\lambda(m) = \operatorname{lcm}\{\delta_m(a) : a\perp m\}.
$$

Đây cũng là một định nghĩa tương đương thường dùng của hàm Carmichael.

Lặp lại [Tính chất 5](#ord-prop-5) nhiều lần cho thấy tồn tại $a\perp m$ sao cho $\delta_m(a)=\lambda(m)$. Vì vậy,

$$
\lambda(m) = \max\{\delta_m(a) : a\perp m\}.
$$

Phần tử đạt cực đại này gọi là **$\lambda$‑nguyên căn** modulo $m$. Nó tồn tại với mọi mô-đun $m$.

### Công thức truy hồi

Hàm Carmichael là một [hàm số học](./basic.md#数论函数). Mục này trình bày công thức truy hồi, và từ đó cho một chứng minh khác của định lý tồn tại nguyên căn.

Mặc dù không phải hàm nhân, khi tính $\lambda(m)$ vẫn có thể xử lý riêng các thừa số nguyên tố cùng nhau.

???+ note "Bổ đề"
    Với $m_1,m_2$ nguyên tố cùng nhau, có $\lambda(m_1m_2)=[\lambda(m_1),\lambda(m_2)]$.

??? note "Chứng minh"
    Gọi $a_1,a_2$ lần lượt là $\lambda$‑nguyên căn modulo $m_1$ và $m_2$. Đặt $m=m_1m_2$. Theo [định lý phần dư Trung Hoa](./crt.md), tồn tại $a\perp m$ sao cho $a\equiv a_i\pmod{m_i}$ với $i=1,2$. Vì $a^{\lambda(m)}\equiv 1\pmod m$, nên với $i=1,2$ đều có $a_i^{\lambda(m)} \equiv 1\pmod{m_i}$, suy ra theo [Tính chất 2](#ord-prop-2) và lựa chọn $a_i$ thì $\lambda(m_i)=\delta_{m_i}(a_i)\mid \lambda(m)$. Do đó $[\lambda(m_1),\lambda(m_2)]\mid\lambda(m)$.

    Ngược lại, với mọi $a\perp m$ và $i=1,2$, ta có $a^{[\lambda(m_1),\lambda(m_2)]} \equiv 1 \pmod{m_i}$. Áp dụng định lý phần dư Trung Hoa suy ra $a^{[\lambda(m_1),\lambda(m_2)]} \equiv 1 \pmod{m}$ với mọi $a\perp m$. Theo định nghĩa hàm Carmichael, $\lambda(m)\mid [\lambda(m_1),\lambda(m_2)]$.

    Vậy đẳng thức trong bổ đề đúng.

Vì vậy, chỉ cần tính $\lambda$ tại các lũy thừa nguyên tố. Trước hết là trường hợp lũy thừa của $2$.

???+ note "Bổ đề"
    Với $m=2^e$ và $e\in\mathbf N_+$, ta có $\lambda(2)=1$, $\lambda(4)=2$, và với $e\ge 3$ thì $\lambda(m)=2^{e-2}$.

??? note "Chứng minh"
    Với $m=2,4$ thì kiểm tra trực tiếp. Với $m=2^e$ và $e\ge 3$, lặp lại phần đầu chứng minh của [Bổ đề 4](#prim-root-lem-4) suy ra $\lambda(m)\le 2^{e-2}$. Sau đó chỉ cần chứng minh tồn tại phần tử bậc $2^{e-2}$. Ta có

    $$
    5^{2^{e-3}} = (1 + 2^2)^{2^{e-3}} = 1 + 2^2\times 2^{e-3} = 1 + 2^{e-1} \not\equiv 1 \pmod{2^e}.
    $$

    Do đó $\delta_m(5)\nmid 2^{e-3}$, mà $\delta_m(5) \mid 2^{e-2}$, nên $5$ phải có bậc $2^{e-2}$. Suy ra $\lambda(m)=2^{e-2}$.

Trong quá trình chứng minh bổ đề này, ta cũng thu được mô tả cấu trúc của hệ thặng dư tối giản modulo $2^e$:

<a id="mod-pow-2"></a>

???+ note "Hệ quả"
    Với mô-đun $2^e$ và $e \ge 2$, mọi số lẻ đồng dư với duy nhất một số dạng $\pm 5^k$, với $k\in\mathbf N$ và $k < 2^{e-2}$. Nghĩa là $\pm 1,\pm 5,\cdots,\pm 5^{2^{e-2}-1}$ đôi một không đồng dư và tạo thành một hệ thặng dư tối giản.

??? note "Chứng minh"
    Dễ kiểm tra trường hợp $e=2$. Với $e \ge 3$, vì $5$ có bậc $2^{e-2}$ modulo $2^e$, nên $1,5,\cdots,5^{2^{e-2}-1}$ đôi một khác nhau. Các số này đều $\equiv 1 \pmod 4$, còn các số đối của chúng đều $\equiv 3 \pmod 4$, nên $\pm 1,\pm 5,\cdots,\pm 5^{2^{e-2}-1}$ đôi một khác nhau modulo $2^e$. Tổng cộng có $2^{e-1}$ số, đúng bằng kích thước hệ thặng dư tối giản modulo $2^e$, nên chúng tạo thành hệ thặng dư tối giản.

Tiếp theo xét lũy thừa của số nguyên tố lẻ.

???+ note "Bổ đề"
    Với $m=p^e$, trong đó $p$ là số nguyên tố lẻ và $e\in\mathbf N_+$, có $\lambda(m)=p^{e-1}(p-1)$.

??? note "Chứng minh"
    Trước hết xét $e=1$, tức $m=p$ là số nguyên tố lẻ. Theo định nghĩa hàm Carmichael, mọi $a\perp p$ đều là nghiệm của $x^{\lambda(p)}\equiv 1\pmod{p}$. Trong modulo $p$, phương trình này có $p-1$ nghiệm khác nhau. Theo [định lý Lagrange](./congruence-equation.md#定理-3lagrange-定理) suy ra $p-1\le\lambda(p)$. Đồng thời theo định lý Euler, $\lambda(p)\mid\varphi(p)=p-1$. Vậy $\lambda(p)=p-1$.

    Với $m=p^e$ và $e>1$, bắt đầu bằng việc chứng minh $1+p$ có bậc $p^{e-1}$. Ta có

    $$
    (1+p)^{p^{e-1}} \equiv 1,\quad (1+p)^{p^{e-2}} \equiv 1 + p^{e-1} \not\equiv 1 \pmod{p^e}.
    $$

    Do đó $\delta_m(1+p)=p^{e-1}$. Mặt khác, gọi $g$ là nguyên căn modulo $p$, thì vì $g^{\delta_m(g)}\equiv 1 \pmod{p}$, nên theo [Tính chất 2](#ord-prop-2) suy ra $p-1\mid\delta_m(p)$. Theo định nghĩa hàm Carmichael và định lý Euler,

    $$
    p^{e-1}(p-1) = [\delta_m(p),p^{e-1}]\mid\lambda(m) \mid \varphi(m) = p^{e-1}(p-1).
    $$

    Do đó $\lambda(m)=p^{e-1}(p-1)$.

Tóm tắt lại, ta có công thức truy hồi của hàm Carmichael:

???+ note "Định lý"
    Với mọi số nguyên dương $m$, ta có

    $$
    \lambda(m) = \begin{cases}
    \varphi(m), & \text{nếu }m=1,2,4,p^e\text{ với }p\text{ lẻ và }e \ge 1,\\
    \frac{1}{2}\varphi(m), &\text{nếu }m=2^e,~e\ge 3,\\
    \operatorname{lcm}\{\lambda(p_1^{e_1}),\lambda(p_2^{e_2}),\cdots,\lambda(p_s^{e_s})\}, &\text{nếu }m = p_1^{e_1}p_2^{e_2}\cdots p_s^{e_s}\text{ với }p_1,p_2,\cdots,p_s\text{ đôi một khác nhau}.
    \end{cases}
    $$

Từ công thức truy hồi này có thể suy ra:

???+ note "Hệ quả"
    Với $m_1,m_2$ là số nguyên dương, có $\lambda([m_1,m_2])=[\lambda(m_1),\lambda(m_2)]$.

So sánh định nghĩa nguyên căn và hàm Carmichael, ta thấy nguyên căn modulo $m$ tồn tại khi và chỉ khi $\lambda(m)=\varphi(m)$. Từ công thức truy hồi, dễ dàng suy ra:

???+ note "Hệ quả"
    Nguyên căn modulo $m$ tồn tại khi và chỉ khi $m=1,2,4,p^e,2p^e$, trong đó $p$ là số nguyên tố lẻ và $e\in\mathbf N_+$.

Vì chứng minh công thức truy hồi ở trên không dùng định lý tồn tại nguyên căn, điều này cung cấp một chứng minh khác cho định lý.

### Số Carmichael

Nhờ hàm Carmichael, ta có thể nghiên cứu các tính chất và phân bố của số Carmichael (OEIS:[A002997](https://oeis.org/A002997)). Đây là các hợp số mà phép kiểm tra nguyên tố Fermat [Fermat test](./prime.md#fermat-素性测试) chắc chắn thất bại.

???+ abstract "Số Carmichael"
    Với hợp số $n$, nếu với mọi $a\perp n$ đều có $a^{n-1} \equiv 1 \pmod n$ thì gọi $n$ là **số Carmichael**.

Số Carmichael nhỏ nhất là $561 = 3 \times 11 \times 17$.

Theo định nghĩa hàm Carmichael, hợp số $n$ là Carmichael khi và chỉ khi $\lambda(n)\mid n-1$, trong đó $\lambda(n)$ là hàm Carmichael. Hơn nữa, ta có tiêu chuẩn sau:

???+ note "Tiêu chuẩn Korselt[^korselt1899probleme]"
    Hợp số $n$ là số Carmichael khi và chỉ khi $n$ không có thừa số bình phương và với mọi ước nguyên tố $p$ của $n$ đều có $(p-1) \mid (n-1)$.

??? note "Chứng minh"
    Trước hết chứng minh tính cần thiết. Giả sử $\lambda(n)\mid (n-1)$. Kiểm tra công thức truy hồi của hàm Carmichael thấy rằng nếu $n$ có thừa số bình phương $p$, thì $p\mid \lambda(n)$, nhưng $p\nmid (n-1)$, mâu thuẫn. Tương tự, công thức truy hồi cho thấy $(p-1)\mid \lambda(n)$, nên $(p-1)\mid (n-1)$.

    Tiếp theo chứng minh tính đủ. Vì $n$ là hợp số nên có ước nguyên tố lẻ $p$, do đó $n-1$ là chẵn và $n$ là số lẻ. Với hợp số lẻ không có thừa số bình phương, công thức truy hồi của hàm Carmichael cho biết $\lambda(n)=\operatorname{lcm}\{p-1:p\mid n\}$. Do đó chỉ cần $(p-1)\mid (n-1)$ với mọi ước nguyên tố $p$ thì $\lambda(n)\mid (n-1)$.

Từ tiêu chuẩn này, suy ra các tính chất đơn giản của số Carmichael:

???+ note "Hệ quả"
    Số Carmichael là số lẻ, không có thừa số bình phương, và có ít nhất $3$ thừa số nguyên tố khác nhau.

??? note "Chứng minh"
    Hai tính chất đầu suy ra trực tiếp từ tiêu chuẩn Korselt và chứng minh của nó. Tính chất thứ ba: chứng minh rằng tích của hai số nguyên tố phân biệt $p_1,p_2$ không thể là số Carmichael. Giả sử $n=p_1p_2$ là số Carmichael. Theo tiêu chuẩn Korselt, $(p_i-1)\mid (n-1)$. Nhưng

    $$
    n-1=p_1p_2-1\equiv p_2-1 \pmod{p_1-1}.
    $$

    Do đó $(p_1-1)\mid(p_2-1)$. Tương tự $(p_2-1)\mid(p_1-1)$, suy ra $p_1=p_2$, mâu thuẫn. Vậy số Carmichael phải có ít nhất $3$ ước nguyên tố khác nhau.

Dùng số học giải tích có thể suy ra thêm về phân bố số Carmichael. Gọi $C(n)$ là số lượng số Carmichael không vượt quá $n$. Alford, Granville và Pomerance[^alford1994infinitely] chứng minh rằng với $n$ đủ lớn, $C(n)>n^{2/7}$. Do đó có vô hạn số Carmichael. Trước đó, Erdős[^erdos1956pseudoprimes] đã chứng minh $C(n) < n\exp\left(-c\dfrac{\ln n\ln\ln\ln n}{\ln\ln n}\right)$, với $c$ là hằng số. Vì vậy, số Carmichael phân bố rất thưa so với số nguyên tố. Thực tế, có[^pinchcarmichael] $C(10^9)=646$, $C(10^{18})=1~401~644$.

## Tài liệu tham khảo và chú thích

-   [Primitive root modulo n - Wikipedia](https://en.wikipedia.org/wiki/Primitive_root_modulo_n)
-   [The order of a unit - Course Notes](https://crypto.stanford.edu/pbc/notes/numbertheory/order.html)
-   [The primitive root theorem - Amin Witno's notes](http://witno.com/philadelphia/notes/won5.pdf)
-   [Carmichael function - Wikipedia](https://en.wikipedia.org/wiki/Carmichael_function)
-   [Carmichael's Lambda Function - Brilliant Math & Science Wiki](https://brilliant.org/wiki/carmichaels-lambda-function/)
-   [Carmichael number - Wikipedia](https://en.wikipedia.org/wiki/Carmichael_number)
-   [Carmichael Number - Wolfram MathWorld](https://mathworld.wolfram.com/CarmichaelNumber.html)

[^yuan1959note]: Wang Y. "On the least primitive root of a prime." (in Chinese). Acta Math Sinica, 1959, 4: 432–441; English transl. in*Sci. Sinica*, 1961, 10: 1–14.

[^burgess1962character]: BURGESS, David A. "On character sums and primitive roots." Proceedings of the London Mathematical Society, 1962, 3.1: 179-192.

[^cohen1974least]: Cohen, S. D., R. W. K. Odoni, and W. W. Stothers. "On the least primitive root modulo p 2." Bulletin of the London Mathematical Society 6, no. 1 (1974): 42-46.

[^elliott1998least]: Elliott, P. D. T. A., and L. Murata. "The least primitive root mod 2p2." Mathematika 45, no. 2 (1998): 371-379.

[^fridlender1949least]: FRIDLENDER, V. R. "On the least n-th power non-residue." Dokl. Akad. Nauk SSSR. 1949. p. 351-352.

[^salie1949kleinsten]: SALIÉ, Hans. "Über den kleinsten positiven quadratischen Nichtrest nach einer Primzahl." Mathematische Nachrichten, 1949, 3.1: 7-8.

[^burgess1968average]: Burgess, D. A., and P. D. T. A. Elliott. "The average of the least primitive root." Mathematika 15, no. 1 (1968): 39-50.

[^elliott1997average]: Elliott, Peter DTA, and Leo Murata. "On the average of the least primitive root modulo p." Journal of The london Mathematical Society 56, no. 3 (1997): 435-454.

[^more-evidence]: 更多结果可以参考 [Least prime primitive root of prime numbers](https://sweet.ua.pt/tos/p_roots.html)．

[^density-prim-root]: 如果模 $m$ 的原根存在，那么，$\varphi(m)\ge\dfrac{1}{3}m$，且等号仅在 $m=2\times 3^e~(e\in\mathbf N_+)$ 处取得．进一步地，当 $m > 2$ 时，对欧拉函数 $\varphi(m)$ 有估计：$\varphi(m)>\dfrac{m}{e^{\gamma}\log\log m+\frac{3}{\log\log m}}$．将这两者结合，就得到文中的表达式．关于欧拉函数的该估计，可以参考论文 Rosser, J. Barkley, and Lowell Schoenfeld. "Approximate formulas for some functions of prime numbers." Illinois Journal of Mathematics 6, no. 1 (1962): 64-94．

[^korselt1899probleme]: Korselt, A. R. (1899). "Problème chinois." L'Intermédiaire des Mathématiciens. 6: 142–143.

[^alford1994infinitely]: W. R. Alford; Andrew Granville; Carl Pomerance (1994). "There are Infinitely Many Carmichael Numbers." Annals of Mathematics. 140 (3): 703–722.

[^erdos1956pseudoprimes]: Erdős, P. (1956). "On pseudoprimes and Carmichael numbers." Publ. Math. Debrecen. 4 (3–4): 201–206.

[^pinchcarmichael]: PINCH, Richard GE. The Carmichael numbers up to ${10}^{20}$.
