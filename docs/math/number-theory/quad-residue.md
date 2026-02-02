author: hly1204, ShaoChenHeng, Chrogeek, Enter-tainer, Great-designer, iamtwz, monkeysui, nanmenyangde, rgw2010, sshwy, StudyingFather, TachikakaMin, Tiphereth-A, Xeonacid, xyf007, marscheng1

## Giới thiệu

Số dư bậc hai có thể hiểu là bàn về tính khả thi của phép **khai căn** trong ý nghĩa modulo. Với căn bậc cao hơn, xem [k‑th residue](./residue.md).

## Định nghĩa

???+ abstract "Số dư bậc hai"
    Cho số nguyên $a$, $p$ với $(a,p)=1$, nếu tồn tại số nguyên $x$ sao cho
    
    $$
    x^2\equiv a\pmod p,
    $$
    
    thì gọi $a$ là số dư bậc hai modulo $p$, ngược lại gọi là số không dư bậc hai modulo $p$. Về sau, khi mô-đun $p$ là hiển nhiên, có thể viết gọn là dư (không dư) bậc hai.

## Tiêu chuẩn Euler

Khi mô-đun là số nguyên tố lẻ, ta có định lý sau:

???+ abstract "Tiêu chuẩn Euler"
    Với số nguyên tố lẻ $p$ và số nguyên $a$ thỏa $(a,p)=1$, ta có
    
    $$
    a^{\frac{p-1}{2}}\equiv\begin{cases}
        1 \pmod p,  & (\exists x\in\mathbf{Z}),~~a\equiv x^2\pmod p,\\
        -1 \pmod p, & \text{ngược lại}.\\
    \end{cases}
    $$
    
    Tức với các $p$ và $a$ như trên:
    
    1.  $a$ là số dư bậc hai modulo $p$ khi và chỉ khi $a^{\frac{p-1}{2}}\equiv 1 \pmod p$.
    2.  $a$ là số không dư bậc hai modulo $p$ khi và chỉ khi $a^{\frac{p-1}{2}}\equiv -1 \pmod p$.

??? note "Chứng minh"
    Trước hết, theo [định lý nhỏ Fermat](./fermat.md#费马小定理), $a^{p-1}\equiv 1\pmod p$, do đó
    
    $$
    \left(a^{\frac{p-1}{2}}+1\right)\left(a^{\frac{p-1}{2}}-1\right)\equiv 0\pmod p,
    $$
    
    suy ra với mọi $a$ thỏa $(a,p)=1$ đều có $a^{(p-1)/2}\equiv \pm 1\pmod p.$
    
    Mặt khác vì $p$ là số nguyên tố lẻ, ta có:
    
    $$
    x^{p-1}-a^{\frac{p-1}{2}}={\left(x^2\right)}^{\frac{p-1}{2}}-a^{\frac{p-1}{2}}=(x^2-a)P(x),
    $$
    
    trong đó $P(x)$ là một đa thức hệ số nguyên nào đó, suy ra:
    
    $$
    \begin{aligned}
        x^p-x&=x\left(x^{p-1}-a^{\frac{p-1}{2}}\right)+x\left(a^{\frac{p-1}{2}}-1\right)\\
        &=(x^2-a)xP(x)+\left(a^{\frac{p-1}{2}}-1\right)x.\\
    \end{aligned}
    $$
    
    Theo [định lý 5 về phương trình đồng dư](./congruence-equation.md#定理-5), $a$ là số dư bậc hai modulo $p$ khi và chỉ khi $a^{(p-1)/2}\equiv 1\pmod p$. Từ đó suy ra $a$ là số không dư bậc hai modulo $p$ khi và chỉ khi $a^{(p-1)/2}\equiv -1\pmod p$.

Từ tiêu chuẩn Euler, ta có hệ quả sau:

???+ note "Số lượng số dư bậc hai"
    Với số nguyên tố lẻ $p$, theo modulo $p$ số dư bậc hai và số không dư bậc hai đều có $\dfrac{p-1}{2}$ phần tử.

??? note "Chứng minh"
    Theo tiêu chuẩn Euler, xét $a^{\frac{p-1}{2}}\equiv 1\pmod p.$
    
    Vì $\dfrac{p-1}{2}\mid (p-1)$, theo [định lý 6 về phương trình đồng dư](./congruence-equation.md#定理-6), phương trình $a^{\frac{p-1}{2}}\equiv 1\pmod p$ có $\dfrac{p-1}{2}$ nghiệm. Do đó theo modulo $p$, số dư bậc hai và số không dư bậc hai đều có $\dfrac{p-1}{2}$ phần tử.

## Ký hiệu Legendre

Để tiện cho phần sau, ta giới thiệu ký hiệu sau:

???+ abstract "Ký hiệu Legendre"
    Với **số nguyên tố lẻ** $p$ và số nguyên $a$, định nghĩa ký hiệu Legendre như sau:
    
    $$
    \left(\frac{a}{p}\right)=\begin{cases}
        0,  & p\mid a,\\
        1,  & (p\nmid a) \land ((\exists x\in\mathbf{Z}),~~a\equiv x^2\pmod p),\\
        -1, & \text{ngược lại}.\\
    \end{cases}
    $$

Tức với $a$ thỏa $(a,p)=1$:

-   $a$ là số dư bậc hai modulo $p$ khi và chỉ khi $\left(\dfrac{a}{p}\right)=1.$
-   $a$ là số không dư bậc hai modulo $p$ khi và chỉ khi $\left(\dfrac{a}{p}\right)=-1.$

Bảng sau là một số giá trị của ký hiệu Legendre (từ [Wikipedia](https://en.wikipedia.org/wiki/Legendre_symbol#Table_of_values)):

![](./images/quad_residue.png)

### Tính chất

1.  Với mọi số nguyên $a$,

    $$
    a^{\frac{p-1}{2}}\equiv \left(\frac{a}{p}\right)\pmod p.
    $$

    Từ đó có hệ quả:

    -   $$
        \left(\dfrac{1}{p}\right)=1.
        $$
    -   $$
        \left(\dfrac{-1}{p}\right)=(-1)^{\frac{p-1}{2}}=\begin{cases}
            1,  & p\equiv 1\pmod 4,\\
            -1, & p\equiv 3\pmod 4.
            \end{cases}
        $$

2.  $a_1\equiv a_2\pmod p\implies \left(\dfrac{a_1}{p}\right)=\left(\dfrac{a_2}{p}\right).$

3.  ([Tính hoàn toàn tích](./basic.md#积性函数)) Với mọi số nguyên $a_1,a_2$,

    $$
    \left(\frac{a_1a_2}{p}\right)=\left(\frac{a_1}{p}\right)\left(\frac{a_2}{p}\right).
    $$

    Suy ra: với số nguyên $a,b$ và $p\nmid b$ thì

    $$
    \left(\frac{ab^2}{p}\right)=\left(\frac{a}{p}\right).
    $$

4.  $$
    \left(\frac{2}{p}\right)=(-1)^{\frac{p^2-1}{8}}=\begin{cases}
            1,  & p\equiv \pm 1\pmod 8, \\
            -1, & p\equiv \pm 3\pmod 8. \\
        \end{cases}
    $$

??? note "Chứng minh"
    1.  Suy ra trực tiếp từ [định nghĩa ký hiệu Legendre](#legendre-符号) và [tiêu chuẩn Euler](#euler-判别法).
    2.  Ta có
    
        $$
        a_1\equiv a_2\pmod p\implies \left(\frac{a_1}{p}\right)\equiv\left(\frac{a_2}{p}\right)\pmod p,
        $$
    
        mà $\left|\left(\dfrac{a_1}{p}\right)-\left(\dfrac{a_2}{p}\right)\right|\leq 2$ và $p>2$, nên
    
        $$
        a_1\equiv a_2\pmod p\implies \left(\frac{a_1}{p}\right)=\left(\frac{a_2}{p}\right).
        $$
    3.  Từ 1 suy ra
    
        $$
        \left(\frac{a_1a_2}{p}\right)\equiv a_1^{\frac{p-1}{2}}a_2^{\frac{p-1}{2}}\equiv\left(\frac{a_1}{p}\right)\left(\frac{a_2}{p}\right)\pmod p.
        $$
    
        Và $\left|\left(\dfrac{a_1a_2}{p}\right)-\left(\dfrac{a_1}{p}\right)\left(\dfrac{a_2}{p}\right)\right|\leq 2$ với $p>2$, nên
    
        $$
        \left(\frac{a_1a_2}{p}\right)=\left(\frac{a_1}{p}\right)\left(\frac{a_2}{p}\right).
        $$
    4.  Xem [luật tương hỗ bậc hai](#二次互反律).

Từ các tính chất trên, nếu với mọi số nguyên tố lẻ $p,q$ ta tính được $\left(\dfrac{p}{q}\right)$, thì có thể tính ký hiệu Legendre trong mọi trường hợp hợp lệ. Sau đây là một định lý đẹp, thiết lập mối liên hệ giữa $\left(\dfrac{p}{q}\right)$ và $\left(\dfrac{q}{p}\right)$, giúp ta tính toán theo tư duy tương tự [thuật toán Euclid](./gcd.md#欧几里得算法).

### Luật tương hỗ bậc hai

???+ note "Luật tương hỗ bậc hai"
    Với hai số nguyên tố lẻ khác nhau $p,q$, ta có
    
    $$
    \left(\frac{p}{q}\right)\left(\frac{q}{p}\right)=(-1)^{\frac{p-1}{2}\frac{q-1}{2}}.
    $$

Có nhiều cách chứng minh[^ref5]. Một cách là dựa trên bổ đề sau[^ref6]:

???+ note "Bổ đề Gauss"
    Cho $p$ là số nguyên tố lẻ, $(n,p)=1$. Với $k~\left(1\leq k\leq (p-1)/2\right)$, đặt $r_k=nk\bmod p$, gọi $A=\{r_k:r_k < p/2\}$, $B=\{r_k:r_k > p/2\}$, khi đó
    
    $$
    \left(\frac{n}{p}\right)=(-1)^{|B|}.
    $$

??? note "Chứng minh"
    Đặt $\lambda=|A|$, $\mu=|B|$, hiển nhiên $\lambda+\mu=(p-1)/2$, nên
    
    $$
    n^{\frac{p-1}{2}}\left(\frac{p-1}{2}\right)!=\prod_{k=1}^{\frac{p-1}{2}} nk\equiv\prod_{a\in A}a\prod_{b\in B}b\pmod{p}.
    $$
    
    Với mọi $b\in B$ ta có $\dfrac{p}{2} < b < p$, nên $0 < p-b < \dfrac{p}{2}$. Hơn nữa, với mọi $b\in B$ ta có $p-b\notin A$, nếu không thì tồn tại $a\in A$, $b\in B$ với $a=p-b$, tương đương tồn tại $0 < k_1,k_2 < (p-1)/2$ sao cho $a=nk_1$, $b=nk_2$ và $p\mid n(k_1+k_2)$. Vì $(n,p)=1$ nên $p\mid (k_1+k_2)$, nhưng $0 < k_1+k_2 < p$, mâu thuẫn. Do đó
    
    $$
    n^{\frac{p-1}{2}}\left(\frac{p-1}{2}\right)!\equiv(-1)^{\mu}\prod_{a\in A}a\prod_{b\in B}(p-b)=(-1)^{\mu}\left(\frac{p-1}{2}\right)!\pmod{p},
    $$
    
    tức
    
    $$
    n^{\frac{p-1}{2}}\equiv(-1)^{\mu}\pmod{p}.
    $$
    
    Suy ra theo [tính chất 1](#性质) của ký hiệu Legendre.

??? tip "Mở rộng"
    Bổ đề Gauss có thể mở rộng như sau[^ref7]:
    
    Với $p$ là số nguyên tố lẻ, chọn $I\subset\mathbf{Z}_p^*$ thỏa $I\cup -I=\mathbf{Z}_p^*$ và $I\cap -I=\varnothing$, trong đó $-I:=\{-i:i\in I\}$, khi đó với mọi $n$ nguyên tố cùng nhau với $p$,
    
    $$
    \left(\frac{n}{p}\right)=(-1)^{|J|},
    $$
    
    trong đó $J=\{j\in I:nj\in -I\}$.
    
    Dễ thấy chọn $I=\{1,2,\dots,(p-1)/2\}$ thì nhận được bổ đề Gauss. Cách chứng minh tương tự nên bỏ qua.

Suy ra hệ quả:

???+ note "Hệ quả"
    Với số nguyên tố lẻ $p$,
    
    $$
    \left(\frac{2}{p}\right)=(-1)^{\frac{p^2-1}{8}}=\begin{cases}
            1,  & p\equiv \pm 1\pmod 8, \\
            -1, & p\equiv \pm 3\pmod 8. \\
        \end{cases}
    $$
    
    Với số nguyên tố lẻ $p$ và số lẻ $n$ thỏa $(n,p)=1$,
    
    $$
    \left(\frac{n}{p}\right)=(-1)^{\sum_{i=1}^{(p-1)/2}\lfloor ni/p \rfloor}.
    $$

??? note "Chứng minh"
    Với $n,k,r_k,A,B,\lambda,\mu$ như trong bổ đề Gauss, ta có $nk=p\left\lfloor\dfrac{nk}{p}\right\rfloor+r_k$, do đó
    
    $$
    \begin{aligned}
        n\cdot\frac{p^2-1}{8}=\sum_{k=1}^{\frac{p-1}{2}}nk&=p\sum_{k=1}^{\frac{p-1}{2}}\left\lfloor\dfrac{nk}{p}\right\rfloor+\sum_{a\in A}a+\sum_{b\in B}b\\
        &=p\sum_{k=1}^{\frac{p-1}{2}}\left\lfloor\dfrac{nk}{p}\right\rfloor+\sum_{a\in A}a+\sum_{b\in B}(p-b)+2\sum_{b\in B}b-p\mu\\
        &=p\sum_{k=1}^{\frac{p-1}{2}}\left\lfloor\dfrac{nk}{p}\right\rfloor+\sum_{k=1}^{\frac{p-1}{2}}k+2\sum_{b\in B}b-p\mu\\
        &=p\sum_{k=1}^{\frac{p-1}{2}}\left\lfloor\dfrac{nk}{p}\right\rfloor+\frac{p^2-1}{8}+2\sum_{b\in B}b-p\mu,
    \end{aligned}
    $$
    
    nên
    
    $$
    (n-1)\frac{p^2-1}{8}=p\sum_{k=1}^{\frac{p-1}{2}}\left\lfloor\dfrac{nk}{p}\right\rfloor+2\sum_{b\in B}b-p\mu.
    $$
    
    Nếu $n=2$ thì $0 < \dfrac{nk}{p}\leq\dfrac{p-1}{p} < 1$, suy ra
    
    $$
    \frac{p^2-1}{8}\equiv\mu\pmod{2}.
    $$
    
    Nếu $2\nmid n$ thì
    
    $$
    \sum_{k=1}^{\frac{p-1}{2}}\left\lfloor\dfrac{nk}{p}\right\rfloor\equiv\mu\pmod{2}.
    $$

Từ hệ quả này, để chứng minh luật tương hỗ bậc hai chỉ cần kiểm tra

$$
\frac{p-1}{2}\frac{q-1}{2}=\sum_{k=1}^{\frac{p-1}{2}}\left\lfloor\dfrac{qk}{p}\right\rfloor+\sum_{k=1}^{\frac{q-1}{2}}\left\lfloor\dfrac{pk}{q}\right\rfloor.
$$

Xét tập điểm $(px,qy)$ với $1\leq x\leq \dfrac{q-1}{2},1\leq y\leq \dfrac{p-1}{2}$, chia thành hai phần theo quan hệ giữa $px$ và $qy$ (rõ ràng $px\neq qy$), rồi kiểm tra kích thước các tập là đủ.

Luật tương hỗ bậc hai không chỉ dùng để xét $n$ có là số dư bậc hai modulo $p$ hay không, mà còn dùng để xác định cấu trúc các mô-đun $p$ sao cho $n$ là số dư bậc hai.

???+ example "Ví dụ"
    -   Các số nguyên tố lẻ $p$ để $5$ là số dư bậc hai thỏa $p\equiv \pm 1\pmod 5.$
    -   Các số nguyên tố lẻ $p$ để $-3$ là số dư bậc hai thỏa $p\equiv 1\pmod 3.$
    -   Các số nguyên tố lẻ $p$ để $-2$ và $3$ đồng thời là số dư bậc hai thỏa $p\equiv 11\pmod{24}.$

Ngoài ra còn có thể chứng minh các kết quả như “số nguyên tố dạng $4k+1$ là vô hạn”, đây là hệ quả đơn giản của [định lý Dirichlet](https://en.wikipedia.org/wiki/Dirichlet%27s_theorem_on_arithmetic_progressions).

## Ký hiệu Jacobi

Dựa trên luật tương hỗ bậc hai, ta có thể tự nhiên nghĩ đến một tổng quát hóa của ký hiệu Legendre:

???+ abstract "Ký hiệu Jacobi"
    Với **số lẻ dương** $m=p_1^{\alpha_1}\dots p_k^{\alpha_k}$ và số nguyên $a$, trong đó $p_1,\dots,p_k$ là số nguyên tố, $\alpha_1,\dots,\alpha_k$ là số nguyên dương, định nghĩa ký hiệu Jacobi:
    
    $$
    \left(\frac{a}{m}\right):=\prod_{i=1}^k\left(\frac{a}{p_i}\right)^{\alpha_i}.
    $$
    
    Vế phải $\left(\frac{a}{p_i}\right)$ là [ký hiệu Legendre](#legendre-符号). Ngoài ra với mọi số nguyên $a$ ta có $\left(\dfrac{a}{1}\right)=1.$

???+ warning "Warning"
    Thường không phân biệt ký hiệu Legendre và Jacobi vì nhờ tính hoàn toàn tích, Jacobi có các tính chất giống Legendre, nên cách tính là như nhau. Tuy nhiên, cần lưu ý: khi $m$ **không phải số nguyên tố lẻ**, giá trị $\left(\dfrac{a}{m}\right)$ **không liên quan** đến việc $a$ có là số dư bậc hai modulo $m$ hay không. Nhưng nếu $\left(\dfrac{a}{m}\right)=-1$ thì chắc chắn tồn tại một (thực ra là số lẻ) ước nguyên tố $p$ của $m$ sao cho $a$ là số không dư bậc hai modulo $p$, nên $a$ là số không dư bậc hai modulo $m$.

Ta cũng có thể mở rộng mô-đun thành **số nguyên** (cần bổ sung định nghĩa cho $\left(\dfrac{a}{-1}\right)$, $\left(\dfrac{a}{0}\right)$ và $\left(\dfrac{a}{2}\right)$), khi đó nhận được [ký hiệu Kronecker](https://en.wikipedia.org/wiki/Kronecker_symbol).

## Khai căn trong modulo

Mục này bàn về thuật toán khai căn trong modulo. Đặc biệt, tập trung vào mô-đun là số nguyên tố. Với mô-đun tổng quát, xem [khai căn bậc cao trong modulo](./residue.md#模意义下开方).

### Thuật toán cho trường hợp đặc biệt

Xét phương trình $x^2\equiv a\pmod p$, với $p$ là số nguyên tố lẻ và $a$ là số dư bậc hai. Khi $p\bmod 4=3$ có cách giải đơn giản hơn, xét

$$
\begin{aligned}
\left(a^{(p+1)/4}\right)^2&\equiv a^{(p+1)/2}&\pmod p\\
&\equiv x^{p+1}&\pmod p\\
&\equiv \left(x^2\right)\left(x^{p-1}\right)&\pmod p\\
&\equiv x^2&\pmod p&\quad (\because{\text{định lý nhỏ Fermat}})
\end{aligned}
$$

Do đó $a^{(p+1)/4}\bmod p$ là một nghiệm.

#### Thuật toán Atkin

Vẫn xét phương trình trên, với $p\bmod 8=5$. Đặt $b\equiv (2a)^{(p-5)/8}\pmod p$ và $\mathrm{i}\equiv 2ab^2\pmod p$ thì $\mathrm{i}^2\equiv -1\pmod p$ và $ab(\mathrm{i}-1)\bmod p$ là một nghiệm.

???+ note "Chứng minh"
    $$
    \begin{aligned}
    \mathrm{i}^2&\equiv\left(2ab^2\right)^2&\pmod p\\
    &\equiv \left(2a\cdot \left(2a\right)^{(p-5)/4}\right)^2&\pmod p\\
    &\equiv \left(\left(2a\right)^{(p-1)/4}\right)^2&\pmod p\\
    &\equiv \left(2a\right)^{\frac{p-1}{2}}&\pmod p\\
    &\equiv -1&\pmod p
    \end{aligned}
    $$
    
    Khi đó
    
    $$
    \begin{aligned}
    \left(ab(\mathrm{i}-1)\right)^2&\equiv a^2\cdot \left(2a\right)^{(p-5)/4}\cdot (-2\mathrm{i})&\pmod p\\
    &\equiv a\cdot (-\mathrm{i})\cdot \left(2a\right)^{(p-1)/4}&\pmod p\\
    &\equiv a&\pmod p
    \end{aligned}
    $$

### Thuật toán Cipolla

Thuật toán Cipolla giải phương trình $y^2\equiv a\pmod p$, với $p$ là số nguyên tố lẻ và $a$ là số dư bậc hai.

Xét các phép toán trong $\mathbf{F}_p\lbrack x\rbrack /(x^2-g)$, với $g \in \mathbf{F}_p$.

??? note "Cách tính"
    Nếu chưa quen [vành đa thức](../algebra/ring-theory.md#多项式环), có thể hiểu các phần tử có dạng $a_0+a_1x$ với $a_0,a_1\in\mathbf F_p$, và tuân theo quy tắc:
    
    $$
    \begin{aligned}
    (a_0+a_1x)+(b_0+b_1x) &\equiv (a_0+b_0)+(a_1+b_1)x &\pmod{(x^2-g)}\\
    (a_0+a_1x)(b_0+b_1x) &\equiv (a_0b_0+a_1b_1g)+(a_1b_0+a_0b_1)x &\pmod{(x^2-g)}
    \end{aligned}
    $$
    
    Lưu ý $x$ không phải một số cụ thể, mà chỉ là ký hiệu hình thức của đa thức. Điểm then chốt là dùng $x^2 \equiv g \pmod{(x^2-g)}$ để biến bậc hai thành bậc nhất và hằng số. Mọi phép toán số nguyên đều lấy modulo $p$.
    
    Xem thêm [đa thức](../poly/intro.md) và [lý thuyết trường](../algebra/field-theory.md).

Bước đầu là tìm $r$ sao cho $r^2-a$ là số không dư bậc hai. Trường hợp $a \equiv 0 \pmod p$ không thể tìm được $r$ như vậy nên cần đặc biệt xử lý; ở đây chỉ xét $a \not\equiv 0 \pmod p$. Khi đó có thể chọn ngẫu nhiên $r$ và kiểm tra, kỳ vọng sau 2 lần là tìm được. Khi đó $(r-x)^{\frac{p+1}{2}}\bmod (x^2-(r^2-a))$ là một nghiệm, có thể tính bằng lũy thừa nhanh.

??? note "Vì sao kỳ vọng chỉ cần hai bước"
    Xét trường hợp $r^2-a$ là số dư bậc hai, thì tồn tại $x$ sao cho $r^2-a \equiv x^2 \pmod p$, chuyển vế được $(r+x)(r-x) \equiv a \pmod p$. Dễ thấy mỗi $(r+x) \in [1, p-1]$ tương ứng một cặp nghiệm $(r,x)$, nên số nghiệm của phương trình là $p-1$ cặp. Xét hai trường hợp $x \equiv 0$ và $x \not\equiv 0$. Với $x \equiv 0$, do $a$ là dư bậc hai nên có 2 giá trị $r$. Với $x \not\equiv 0$, có $p-1-2$ trường hợp, mỗi $r$ ứng với 2 trường hợp, tổng cộng $\dfrac{p-3}{2}$ giá trị $r$. Vậy có $2+\dfrac{p-3}{2}=\dfrac{p+1}{2}$ trường hợp khiến $r^2-a$ là số dư bậc hai, nên xác suất để nhận được không dư bậc hai là $\dfrac{p-1}{2p}$, kỳ vọng $\dfrac{2p}{p-1} \approx 2$.

???+ note "Chứng minh"
    Đặt $f(x)=x^2-(r^2-a)\in\mathbf{F}_p\lbrack x\rbrack$.
    
    Cần chứng minh $(r-x)^{\frac{p+1}{2}} \bmod f(x)$ là nghiệm của phương trình, và thuộc $\mathbf{F}_p$. Trước hết chứng minh $(r-x)^{p+1}\equiv a\pmod {f(x)}$. Ta cần hai bổ đề:
    
    **Bổ đề 1:** $x^p \equiv -x \pmod {f(x)}$
    
    Chứng minh:
    
    $$
    \begin{aligned}
    x^p&= x(x^2)^{\frac{p-1}{2}}\\
    &\equiv x(r^2-a)^{\frac{p-1}{2}}&\pmod{f(x)}&\quad (\because{x^2\equiv r^2-a\pmod{f(x)}})\\
    &\equiv -x&\pmod{f(x)}&\quad (\because{r^2-a}\text{ là số không dư bậc hai})
    \end{aligned}
    $$
    
    **Bổ đề 2:** $(a+b)^p \equiv a^p+b^p \pmod p$
    
    Dùng nhị thức Newton, mọi hệ số trung gian đều chia hết cho $p$, nên chỉ còn $a^p+b^p$.
    
    $$
    \begin{aligned}
    (a+b)^p&=\sum_{i=0}^p\binom{p}{i}a^ib^{p-i}\\
    &=\sum_{i=0}^p\frac{p!}{i!(p-i)!}a^ib^{p-i}\\
    &\equiv a^p+b^p\pmod p
    \end{aligned}
    $$
    
    Từ đó:
    
    $$
    \begin{aligned}
    (r-x)^{p+1}
    &= (r-x)^p(r-x)\\
    &\equiv (r^p-x^p)(r-x)&\pmod{f(x)}\\
    &\equiv (r+x)(r-x)&\pmod{f(x)}\\
    &= r^2-x^2\\
    &\equiv r^2-(r^2-a)&\pmod{f(x)}\\
    &= a\\
    \end{aligned}
    $$
    
    Tiếp theo chứng minh nghiệm thuộc $\mathbf{F}_p$, tức hệ số của $x$ bằng $0$.
    
    Giả sử tồn tại $(a_0+a_1x)^2 \equiv a \pmod {f(x)}$ với $a_1 \not\equiv 0 \pmod p$, tức $a_0^2+2a_0a_1x+a_1^2x^2 \equiv a \pmod {f(x)}$. Chuyển vế và rút gọn:
    
    $$
    a_0^2+a_1^2(r^2-a)-a \equiv -2a_0a_1x \pmod {f(x)}
    $$
    
    Vế trái có hệ số $x$ bằng $0$, nên vế phải cũng vậy, tức $a_0a_1 \equiv 0 \pmod p$. Vì $a_1 \not\equiv 0 \pmod p$ nên $a_0 \equiv 0 \pmod p$, suy ra $(a_1x)^2 \equiv a \pmod {f(x)}$, tức $r^2-a \equiv aa_1^{-2} \pmod p$.
    
    Vì $a$ và $a_1^{-2}$ đều là số dư bậc hai, theo tính tích của ký hiệu Legendre, $aa_1^{-2}$ cũng là dư bậc hai, mâu thuẫn với $r^2-a$ là không dư bậc hai. Vậy không tồn tại nghiệm có hệ số $x$ khác $0$, nghiệm ta tìm có hệ số $x$ bằng $0$.

??? example "Bài mẫu [Luogu P5491【Template】Quadratic Residue](https://www.luogu.com.cn/problem/P5491)"
    ```cpp
    --8<-- "docs/math/code/quad-residue/quad-residue_1.cpp"
    ```

### Thuật toán Bostan–Mori

Thuật toán này dựa trên Cipolla, chuyển bài toán sang [truy hồi tuyến tính thuần nhất hệ số hằng](../poly/linear-recurrence.md) rồi áp dụng Bostan–Mori. Một mô tả thường dùng của Cipolla: $b=x^{\left(p+1\right)/2}\bmod{\left(x^2-tx+a\right)}$ là nghiệm thỏa $b^2\equiv a\pmod{p}$[^ref3], trong đó $x^2-tx+a\in \mathbf{F}_p\lbrack x\rbrack$ là đa thức bất khả quy. Hệ số $t$ cũng chọn ngẫu nhiên. Chứng minh bỏ qua. Theo thuật toán trong bài của Bostan và Mori[^ref4], bài toán quy về lấy một hệ số của nghịch đảo nhân của chuỗi lũy thừa hình thức:

$$
b=\left\lbrack x^{(p+1)/2}\right\rbrack\dfrac{1}{1-tx+ax^2}
$$

và

$$
\left\lbrack x^n\right\rbrack\dfrac{k_0+k_1x}{1+k_2x+k_3x^2}=
\begin{cases}
\left\lbrack x^{(n-1)/2}\right\rbrack\dfrac{k_1-k_0k_2+k_1k_3x}{1+(2k_3-k_2^2)x+k_3^2x^2},&\text{nếu }n\bmod 2=1\\
\left\lbrack x^{n/2}\right\rbrack\dfrac{k_0+(k_0k_3-k_1k_2)x}{1+(2k_3-k_2^2)x+k_3^2x^2},&\text{nếu }n\neq 0 \text{ và } n\bmod 2=0
\end{cases}
$$

Khi $n=0$ thì rõ ràng $\left\lbrack x^0\right\rbrack\dfrac{k_0+k_1x}{1+k_2x+k_3x^2}=k_0$. Thuật toán này dùng ít phép nhân hơn Cipolla. Các thuật toán khác với số phép nhân ít hơn xem bài của Müller[^ref2].

### Thuật toán Legendre

Với phương trình $x^2\equiv a\pmod p$, $p$ là số nguyên tố lẻ và $a$ là số dư bậc hai. Thuật toán Legendre: tìm $r$ sao cho $r^2-a$ là số không dư bậc hai, đặt $a_0+a_1x=(r-x)^{\frac{p-1}{2}}\bmod (x^2-a)$, thì $a_0\equiv 0\pmod p$ và $a_1^{-2}\equiv a\pmod p$.

???+ note "Chứng minh"
    Chọn $b$ sao cho $b^2\equiv a\pmod p$, khi đó $(r-b)(r+b)=r^2-a$ là số không dư bậc hai, nên
    
    $$
    (r-b)^{\frac{p-1}{2}}(r+b)^{\frac{p-1}{2}}\equiv -1\pmod p
    $$
    
    Xét đồng cấu vành
    
    $$
    \begin{aligned}
    \phi:\mathbf{F}_p\lbrack x\rbrack/(x^2-a)&\to \mathbf{F}_p\times \mathbf{F}_p\\
    x&\mapsto (b,-b)
    \end{aligned}
    $$
    
    Khi đó
    
    $$
    \begin{aligned}
    (a_0+a_1b,a_0-a_1b)&=\phi(a_0+a_1x)\\
    &=\phi(r-x)^{\frac{p-1}{2}}\\
    &=((r-b)^{\frac{p-1}{2}},(r+b)^{\frac{p-1}{2}})\\
    &=(\pm 1,\mp 1)
    \end{aligned}
    $$
    
    Suy ra $2a_0=(\pm 1)+(\mp 1)=0$ và $2a_1b=(\pm 1)-(\mp 1)=\pm 2$.

### Thuật toán Tonelli–Shanks

Thuật toán Tonelli–Shanks dựa trên logarit rời rạc để giải $x^2\equiv a\pmod p$[^ref1], với $p$ là số nguyên tố lẻ và $a$ là số dư bậc hai modulo $p$.

Viết $p-1=m2^n$ với $m$ lẻ. Dùng ngẫu nhiên tìm $r\in\mathbf{F}_p$ là số không dư bậc hai. Đặt $g\equiv r^m\pmod p$ và $b\equiv a^{(m-1)/2}\pmod p$. Khi đó tồn tại $e\in\lbrace 0,1,2,\dots ,2^n-1\rbrace$ sao cho $ab^2\equiv g^e\pmod p$. Nếu $a$ là số dư bậc hai thì $e$ là chẵn và $\left(abg^{-e/2}\right)^2\equiv a\pmod p$.

???+ note "Chứng minh"
    Theo định lý nhỏ Fermat,
    
    $$
    g^{2^n} \equiv r^{m2^n} = r^{p-1} \equiv 1 \pmod p.
    $$
    
    Vì $r$ là số không dư bậc hai,
    
    $$
    g^{2^{n-1}} \equiv r^{m2^{n-1}} = r^{\frac{p-1}{2}} \equiv -1 \pmod p.  
    $$
    
    Do đó bậc của $g$ modulo $p$ là $2^n$. Lại vì $ab^2\equiv a^m\pmod p$ là nghiệm của $x^{2^n}\equiv 1\pmod p$, nên $a^m$ là lũy thừa của $g$. Gọi $a^m\equiv g^e\pmod p$.
    
    Vì $a$ là số dư bậc hai nên
    
    $$
    g^{e2^{n-1}} \equiv a^{m2^{n-1}} = a^{\frac{p-1}{2}} \equiv 1 \pmod p.
    $$
    
    Theo tính chất bậc, $2^n\mid e2^{n-1}$, nên $e$ là chẵn. Vì vậy $abg^{-e/2}\bmod p$ là xác định và
    
    $$
    \left(abg^{-e/2}\right)^2 = a^2b^2g^{-e} \equiv a^{m+1}g^{-e} \equiv a \pmod p.
    $$

Còn lại là cách tính $e$. Tonelli và Shanks đề xuất xác định từng bit của $e$. Viết $e=e_0+2e_1+4e_2+\cdots$ với $e_k\in\lbrace 0,1\rbrace$. Do $a$ là số dư bậc hai nên $e_0=0$. Ta dùng tính chất sau để xác định từng $e_k$:

$$
\left(g^eg^{-(e\bmod 2^k)}\right)^{2^{n-1-k}}\equiv g^{2^{n-1}\cdot e_k}\equiv 
\begin{cases}
1\pmod p,&\text{nếu }e_k=0\\
-1\pmod p,&\text{nếu }e_k=1
\end{cases}
$$

Ở đây $g^e\equiv ab^2\pmod p$ đã biết, còn $e\bmod 2^k$ tính được từ các bit $e_0,e_1,\cdots,e_{k-1}$. Khi cài đặt, chỉ cần duy trì $g^eg^{-(e\bmod 2^k)}\bmod p$.

## Bài tập

-   [Luogu P5491【Template】Quadratic Residue](https://www.luogu.com.cn/problem/P5491)
-   [Timus 1132 — Square Root](https://acm.timus.ru/problem.aspx?space=1&num=1132)

## Tài liệu tham khảo và chú thích

1.  [Quadratic residue - Wikipedia](https://en.wikipedia.org/wiki/Quadratic_residue)
2.  [Euler's criterion - Wikipedia](https://en.wikipedia.org/wiki/Euler%27s_criterion)

[^ref1]: Daniel. J. Bernstein. Faster Square Roots in Annoying Finite Fields.

[^ref2]: S. Müller, On the computation of square roots in finite fields, Design, Codes and Cryptography, Vol.31, pp. 301-312, 2004.

[^ref3]: A. Menezes, P. van Oorschot and S. Vanstone. Handbook of Applied Cryptography, 1996.

[^ref4]: Alin Bostan, Ryuhei Mori. A Simple and Fast Algorithm for Computing the N-th Term of a Linearly Recurrent Sequence. Available at <https://arxiv.org/abs/2008.08822>.

[^ref5]: [Proofs of quadratic reciprocity - Wikipedia](https://en.wikipedia.org/wiki/Proofs_of_quadratic_reciprocity)

[^ref6]: Carl Friedrich Gauss. Untersuchungen über höhere Arithmetik, 1965. Page 458-462.

[^ref7]: Kobi Kremnizer.[Lectures in number theory 2022](https://courses.maths.ox.ac.uk/pluginfile.php/29788/mod_resource/content/1/numbertheory-2022.pdf). Proposition 4.3.
