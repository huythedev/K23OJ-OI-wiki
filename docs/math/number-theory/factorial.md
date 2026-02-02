author: aofall, c-forrest, CoelacanthusHex, Early0v0, Enter-tainer, Great-designer, iamtwz, Marcythm, Persdre, shuzhouliu, Tiphereth-A, wsyhb, Xeonacid

## Giới thiệu

Bài viết thảo luận các kết quả liên quan đến tính giai thừa theo một mô-đun nhất định, và đưa ra phương pháp có độ phức tạp tuyến tính theo mô-đun, phù hợp khi mô-đun không quá lớn ($\sim 10^6$). Ngoài cách này, tùy tình huống còn có thể dùng [kỹ thuật đa thức](../poly/shift.md#模素数意义下阶乘) để tính nhanh.

Theo [định lý phần dư Trung Hoa](./crt.md), bài toán giai thừa theo mô-đun có thể quy về trường hợp mô-đun là lũy thừa của số nguyên tố $p^\alpha$. Khi xử lý, thường cần tách tất cả thừa số $p$ trong $n!$ để viết:

$$
n! = p^{\nu_p(n!)}(n!)_p.
$$

Trong đó $\nu_p(n!)$ là số mũ của $p$ trong phân tích nguyên tố của $n!$, còn $(n!)_p$ là phần còn lại sau khi bỏ hết thừa số $p$. Bài viết sẽ bàn về cách tính $(n!)_p$ modulo số nguyên tố (lũy thừa) và cách tính $\nu_p(n!)$.

Phân rã này hữu ích khi giai thừa vừa ở tử vừa ở mẫu, như [tính hệ số nhị thức modulo](./lucas.md). Khi đó các số mũ $p$ ở tử và mẫu trừ nhau, phần còn lại $(n!)_p$ có thể dùng [nghịch đảo nhân](./inverse.md).

Bài viết cũng giới thiệu định lý Wilson và mở rộng, công thức Legendre và định lý Kummer.

## Định lý Wilson

Định lý Wilson cho một điều kiện cần và đủ để một số là nguyên tố.

???+ note "Định lý Wilson"
    Với $n>1$, ta có $(n-1)!\equiv -1\pmod n$ khi và chỉ khi $n$ là số nguyên tố.

??? note "Chứng minh"
    Trước hết chứng minh với $p$ nguyên tố, $(p-1)!\equiv -1\pmod{p}$. Có thể dùng [phương trình đồng dư](./congruence-equation.md#推论-2) hoặc [căn nguyên thủy](./primitive-root.md) để chứng minh ngắn gọn, ở đây bỏ qua và đưa một cách chứng minh cơ bản hơn:
    
    Với $p=2$ thì hiển nhiên. Xét $p\ge 3$. Ta chứng minh tích tất cả phần tử khác $0$ của $\mathbf{Z}_p$ bằng $\overline{-1}$. Vì mọi phần tử khác $0$ đều có nghịch đảo, các cặp nghịch đảo nhân lại được $\overline{1}$. Nhưng có thể có phần tử tự nghịch đảo: $\overline{a}=\overline{a}^{-1}$ khi và chỉ khi $a^2\equiv 1\pmod p$, tức
    
    $$
    0\equiv a^2-1\equiv (a+1)(a-1),\pmod p
    $$
    
    nên $a\equiv 1\pmod p$ hoặc $a\equiv -1\pmod p$. Vì vậy tích của $\mathbf{Z}_p\setminus\{\overline{0},\overline{1},\overline{-1}\}$ là $\overline{1}$, suy ra tích các phần tử khác $0$ là $\overline{-1}$.
    
    Ngược lại, với $n$ hợp số, giả sử $(n-1)!\equiv -1\pmod{n}$. Tức có $k$ sao cho $(n-1)!=kn-1$. Vì $n$ hợp số, tồn tại $p<n$ nguyên tố với $n=pm$. Khi đó $(n-1)!=kpm-1\equiv -1\pmod{p}$. Nhưng $(n-1)!$ đã chứa thừa số $p$, nên $(n-1)!\equiv 0\pmod{p}$, mâu thuẫn. Vậy $(n-1)!\not\equiv-1\pmod{n}$.

Theo ký hiệu ở đây, Wilson có thể viết là $(p!)_p\equiv -1\pmod{p}$.

### Mở rộng

Wilson có thể mở rộng cho mô-đun bất kỳ.

???+ note "Định lý (Gauss)"
    Với $m>1$, ta có
    
    $$
    \prod_{1\le k<m,\ k\perp m} k \equiv \pm 1 \pmod{m}.
    $$
    
    Trong đó $\pm 1$ bằng $-1$ khi và chỉ khi tồn tại [căn nguyên thủy](./primitive-root.md#原根存在定理), tức $m=2,4,p^\alpha,2p^\alpha$ với $p$ lẻ và $\alpha$ là số nguyên dương.

??? note "Chứng minh"
    Có thể chứng minh bằng cấu trúc [nhóm nhân modulo](../algebra/ring-theory.md#应用整数同余类的乘法群). Dưới đây là chứng minh sơ cấp tương tự.
    
    Với $m=2$, $1!=1\equiv -1\pmod{2}$. Với các mô-đun có căn nguyên thủy, gọi $g$ là căn nguyên thủy, mọi $k$ nguyên tố cùng nhau với $m$ có dạng $g^i\bmod m$, $0\le i<\varphi(m)$. Dễ thấy $\varphi(m)$ chẵn. Vì $g^i$ và $g^{\varphi(m)-i}$ là nghịch đảo, ghép cặp:
    
    $$
    \prod_{1\le k<m,\ k\perp m} k \equiv \prod_{i=0}^{\varphi(m)-1}g^i = g^{\varphi(m)/2}\prod_{i=1}^{\varphi(m)/2-1}g^{i}g^{\varphi(m)-i} \equiv g^{\varphi(m)/2} \pmod{m}.
    $$
    
    Vì $g^{\varphi(m)/2}\bmod m$ là phần tử duy nhất khác $1$ và tự nghịch đảo, nên nó bằng $-1\bmod m$. Do đó dư số là $-1$.
    
    Với mô-đun không có căn nguyên thủy, cần chứng minh dư số là $1$. Phân tích $m=p_1^{e_1}p_2^{e_2}\cdots p_s^{e_s}$, dùng [CRT](./crt.md), chỉ cần chứng minh
    
    $$
    \prod_{1\le k<m,\ k\perp m} k\equiv 1\pmod{p_j^{e_j}}
    $$
    
    với mọi $p_j^{e_j}$. Theo CRT, mỗi bộ dư $(r_1,\cdots,r_s)$ với $1\le r_j<p_j^{e_j}$ và $p_j\perp r_j$ tương ứng duy nhất một $1\le k<m$ nguyên tố cùng nhau với $m$. Vì thế với một dư $r_j$, có đúng ${\varphi(m)}/{\varphi(p_j^{e_j})}$ số $k$ có $k\equiv r_j\pmod{p_j^{e_j}}$. Nhóm theo dư:
    
    $$
    \prod_{1\le k<m,\ k\perp m} k\equiv\left(\prod_{1\le r_j<p_j^{e_j},\ r_j\perp p_j} r_j\right)^{{\varphi(m)}/{\varphi(p_j^{e_j})}}\pmod{p_j^{e_j}}.
    $$
    
    Chỉ khi ${\varphi(m)}/{\varphi(p_j^{e_j})}=\varphi(m/p_j^{e_j})$ là lẻ, thì $m/p_j^{e_j}$ mới là $1$ hoặc $2$, vì $\varphi(n)$ chẵn với $n\ge 3$. Nếu $p_j$ lẻ thì vì không có căn nguyên thủy nên $m/p_j^{e_j}\neq 1,2$; nếu $p_j^{e_j}=2,4$ thì $m/p_j^{e_j}$ có ước nguyên tố lẻ, nên >2. Vậy số mũ là chẵn. Mà ngoặc trong đã là $-1$ modulo $p_j^{e_j}$, nên lũy thừa là $1$.
    
    Còn lại $p_j=2$ và $e_j>2$, khi đó có thể chứng minh trực tiếp:
    
    $$
    \prod_{1\le r_j<2^{e_j},\ r_j\perp 2}r_j \equiv 1\pmod{2^{e_j}}.
    $$
    
    Ghép cặp các số lẻ như trước, các phần tử không ghép được là nghiệm của $x^2\equiv 1\pmod{2^{e_j}}$. Khi đó $2^{e_j}\mid (x-1)(x+1)$. Đặt $x=2y+1$, suy ra $2^{e_j-2}\mid y(y+1)$, nên $y=t2^{e_j-2}$ hoặc $y=t2^{e_j-2}-1$. Do đó $x=t2^{e_j-1}\pm 1$. Modulo $2^{e_j}$ chỉ có $\pm 1$ và $2^{e_j-1}\pm 1$. Suy ra
    
    $$
    \prod_{1\le r_j<2^{e_j},\ r_j\perp 2}r_j \equiv (-1)(2^{e_j-1}-1)(2^{e_j-1}+1) \equiv 1\pmod{2^{e_j}}.
    $$
    
    Hoàn tất chứng minh.

Trường hợp quan trọng là mô-đun lũy thừa nguyên tố:

???+ note "Hệ quả"
    Với số nguyên tố $p$ và $\alpha$ nguyên dương:
    
    $$
    \prod_{1\le k<p^\alpha,\ k\perp p}k \equiv 
    \begin{cases}
    1, & p=2\text{ và }\alpha\ge3,\\
    -1, &\text{ngược lại}
    \end{cases}
    \pmod{p^\alpha}.
    $$

Lưu ý vế trái không phải $(p^\alpha!)_p$, vì còn phải tính đóng góp từ bội của $p$.

## Tính phần dư của giai thừa

Phần này bàn về cách tính $(n!)_p\bmod p^{\alpha}$.

### Trường hợp mô-đun nguyên tố

$(n!)_p$ có cấu trúc đệ quy rõ ràng. Xét ví dụ:

???+ example "Ví dụ"
    Tính $(32!)_5 \bmod{5}$:
    
    $$
    \begin{aligned}
    (32!)_5 &= 1\times 2\times 3 \times 4\times \underbrace{1}_{5}\times 6 \times 7 \times 8\times 9 \times \underbrace{2}_{10} \\
    &\quad\times 11 \times 12 \times 13 \times 14\times \underbrace{3}_{15}\times 16 \times 17\times 18\times 19\times \underbrace{4}_{20} \\
    &\quad\times 21 \times 22 \times 23 \times 24\times \underbrace{1}_{25}\times 26 \times 27\times 28\times 29\times \underbrace{6}_{30} \times 31 \times 32 \\
    &\equiv 1\times 2\times 3 \times 4\times \underbrace{1}_{5}\times 1 \times 2 \times 3\times 4 \times \underbrace{2}_{10} \\
    &\quad\times 1 \times 2 \times 3 \times 4\times \underbrace{3}_{15}\times 1 \times 2\times 3\times 4\times \underbrace{4}_{20} \\
    &\quad\times 1 \times 2 \times 3 \times 4\times \underbrace{1}_{25}\times 1 \times 2\times 3\times 4\times \underbrace{1}_{30}\times 1\times 2\\
    &= (1\times 2\times 3\times 4)^{6}\times(1\times 2)\times(\underbrace{1}_{5}\times \underbrace{2}_{10}\times \underbrace{3}_{15} \times \underbrace{4}_{20}\times \underbrace{1}_{25}\times \underbrace{1}_{30}) \pmod{5}
    \end{aligned}
    $$
    
    Do tính chu kỳ mod $5$, có thể chia thành các khối độ dài $5$, khác nhau ở phần tử cuối. Vì $32=6\cdot 5+2$, nên có 6 khối đầy đủ và một đuôi dài 2. Ta lấy phần chính của 6 khối (dùng Wilson), nhân với đuôi, rồi nhân với tích các phần tử cuối (sau khi bỏ thừa số 5), tức $(6!)_{5}\pmod{5}$. Bài toán được quy về nhỏ hơn.

Tổng quát hóa:

???+ note "Công thức đệ quy"
    Với $p$ nguyên tố và $n$ nguyên dương:
    
    $$
    (n!)_p \equiv (-1)^{\left\lfloor n/p\right\rfloor}\cdot (n\bmod p)!\cdot\left(\left\lfloor n/p\right\rfloor!\right)_p\pmod{p}.
    $$

??? note "Chứng minh"
    Ký hiệu $(n)_p$ là $n$ sau khi bỏ hết thừa số $p$. Khi đó
    
    $$
    \begin{aligned}
    (n!)_p &= \prod_{k=1}^n(k)_p = \left(\prod_{1\le k\le n,\ k\perp p}(k)_p\right)\left(\prod_{1\le k\le\lfloor n/p\rfloor}(pk)_p\right) \\
    &= \left(\prod_{i=0}^{\lfloor n/p\rfloor-1}\prod_{j=1}^{p-1}(ip+j)\right)\left(\prod_{j=1}^{n\bmod p}(\lfloor n/k\rfloor k+j)\right)\left(\prod_{1\le k\le\lfloor n/p\rfloor}(k)_p\right) \\
    &\equiv\left(\prod_{j=1}^{p-1}j\right)^{\lfloor n/p\rfloor}\left(\prod_{j=1}^{n\bmod p}j\right)(\lfloor n/p\rfloor!)_p \\
    &\equiv (-1)^{\lfloor n/p\rfloor}\cdot(n\bmod p)!\cdot(\lfloor n/p\rfloor!)_p \pmod p.
    \end{aligned}
    $$
    
    Diễn giải: 
    
    $$
    \begin{aligned}
    (n!)_p &= 1 \cdot 2 \cdot 3 \cdot \ldots \cdot (p-2) \cdot (p-1) \cdot \underbrace{1}_{p} \cdot (p+1) \cdot (p+2) \cdot \ldots \cdot (2p-1) \cdot \underbrace{2}_{2p} \\
    &\quad \cdot (2p+1) \cdot \ldots \cdot (p^2-1) \cdot \underbrace{1}_{p^2} \cdot (p^2 +1) \cdot \ldots \cdot n \pmod{p} \\
    &= 1 \cdot 2 \cdot 3 \cdot \ldots \cdot (p-2) \cdot (p-1) \cdot \underbrace{1}_{p} \cdot 1 \cdot 2 \cdot \ldots \cdot (p-1) \cdot \underbrace{2}_{2p} \cdot 1 \cdot 2 \\
    &\quad \cdot \ldots \cdot (p-1) \cdot \underbrace{1}_{p^2} \cdot 1 \cdot 2 \cdot \ldots \cdot (n \bmod p) \pmod{p}.
    \end{aligned}
    $$
    
    Ngoại trừ khối cuối, giai thừa chia thành nhiều khối bằng nhau:
    
    $$
    \begin{aligned}
    (n!)_p&= \underbrace{1 \cdot 2 \cdot 3 \cdot \ldots \cdot (p-2) \cdot (p-1) \cdot 1}_{1\text{st}} \cdot \underbrace{1 \cdot 2 \cdot 3 \cdot \ldots \cdot (p-2) \cdot (p-1) \cdot 2}_{2\text{nd}} \cdot \ldots \\
    &\quad \cdot \underbrace{1 \cdot 2 \cdot 3 \cdot \ldots \cdot (p-2) \cdot (p-1) \cdot 1}_{p\text{th}} \cdot \ldots \cdot \quad \underbrace{1 \cdot 2 \cdot \cdot \ldots \cdot (n \bmod p)}_{\text{tail}} \pmod{p}.
    \end{aligned}
    $$
    
    Phần chính của khối đầy đủ là $(p-1)!\ \mathrm{mod}\ p$, dùng Wilson:
    
    $$
    (p-1)!\equiv -1\pmod p.
    $$
    
    Có $\left\lfloor \dfrac{n}{p} \right\rfloor$ khối đầy đủ, nên có số mũ $\left\lfloor \dfrac{n}{p} \right\rfloor$.
    
    Phần đuôi là $(n\bmod p)!\bmod p$.
    
    Phần còn lại là tích các phần tử cuối của mỗi khối, dạng:
    
    $$
    (n!)_p = \underbrace{ \ldots \cdot 1 } \cdot \underbrace{ \ldots \cdot 2} \cdot \ldots \cdot \underbrace{ \ldots \cdot (p-1)} \cdot \underbrace{ \ldots \cdot 1 } \cdot \underbrace{ \ldots \cdot 1} \cdot \underbrace{ \ldots \cdot 2} \cdots
    $$
    
    Đây là một giai thừa “rút gọn”:
    
    $$
    \left(\left\lfloor \frac{n}{p} \right\rfloor !\right)_p.
    $$
    
    Nhân các phần lại, thu được công thức đệ quy.

Dùng công thức này, độ sâu đệ quy là $O(\log_p n)$. Nếu mỗi lần tính lại phần trung gian thì mỗi tầng là $O(p)$, tổng $O(p\log_p n)$; nếu tiền xử lý $n!\bmod p$ cho $n=1,\dots,p-1$, tiền xử lý $O(p)$, mỗi tầng $O(1)$, tổng $O(p+\log_p n)$.

Khi cài đặt, vì là đệ quy đuôi nên có thể viết vòng lặp. Đoạn dưới tiền xử lý $1\ldots p-1$; nếu gọi nhiều lần, có thể đưa tiền xử lý ra ngoài.

??? example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/math/code/factorial/fact-mod-p.cpp:core"
    ```

Nếu không đủ bộ nhớ để lưu hết giai thừa, có thể chỉ tính các giá trị $n!\bmod p$ thật sự dùng, rồi sắp xếp và tính một lần, tránh lưu toàn bộ.

### Trường hợp mô-đun là lũy thừa nguyên tố

Có thể làm tương tự, chỉ thay Wilson bằng dạng mở rộng. Trong hai kết luận, $\pm 1$ được hiểu là: nếu $p=2$ và $\alpha\ge 3$ thì lấy $1$, còn lại lấy $-1$.

???+ note "Công thức đệ quy"
    Với $p$ nguyên tố và $\alpha,n$ nguyên dương:
    
    $$
    (n!)_{p} \equiv (\pm 1)^{\lfloor n/p^\alpha\rfloor}\cdot\left(\prod_{1\le j\le (n\bmod p^\alpha),\ j\perp p}j\right)\cdot(\lfloor n/p\rfloor!)_p\pmod{p^\alpha}.
    $$
    
    Trong đó $\pm 1$ lấy như ở [mở rộng Wilson](#推广).

??? note "Chứng minh"
    Tương tự trường hợp mô-đun nguyên tố. Gọi $(k)_p$ là $k$ sau khi bỏ hết thừa số $p$:
    
    $$
    \begin{aligned}
    (n!)_p
    &= \prod_{1\le k\le n}(k)_p = \left(\prod_{1\le k\le n,\ k\perp p}(k)_p\right)\left(\prod_{1\le k\le\lfloor n/p\rfloor}(pk)_p\right) \\
    &= \left(\prod_{i=0}^{\lfloor n/p^\alpha\rfloor-1}\prod_{1\le j\le p^\alpha,\ j\perp p}(ip^\alpha+j)_p\right)\left(\prod_{1\le j\le (n\bmod p^\alpha),\ j\perp p}(\lfloor n/p^\alpha\rfloor p^\alpha+j)_p\right)\left(\prod_{1\le k\le\lfloor n/p\rfloor}(k)_p\right)\\
    &\equiv \left(\prod_{1\le j\le p^\alpha,\ j\perp p}j\right)^{\lfloor n/p^\alpha\rfloor}\cdot\left(\prod_{1\le j\le (n\bmod p^\alpha),\ j\perp p}j\right)\cdot(\lfloor n/p\rfloor!)_p\\
    &\equiv (\pm 1)^{\lfloor n/p^\alpha\rfloor}\cdot\left(\prod_{1\le j\le (n\bmod p^\alpha),\ j\perp p}j\right)\cdot(\lfloor n/p\rfloor!)_p \pmod{p^\alpha}.
    \end{aligned}
    $$

So với mô-đun nguyên tố, ngoài việc $-1$ có thể thành $\pm 1$, cần lưu ý tiền xử lý khác. Với mô-đun $p^\alpha$, cần tiền xử lý tích của các số từ $1$ đến $n$ không chia hết cho $p$:

$$
\prod_{1\le k\le n,\ k\perp p} k\bmod{p^\alpha}.
$$

Trong trường hợp mô-đun nguyên tố, nó trở thành $n!\bmod p$, nhưng với lũy thừa nguyên tố thì không còn vậy.

Ví dụ tính trong trường hợp $p^\alpha$:

???+ example "Ví dụ"
    Tính $(32!)_3\bmod 9$:
    
    $$
    \begin{aligned}
    (32!)_3 
    &= 1\times 2\times \underbrace{1}_{3} \times 4\times 5\times \underbrace{2}_{6}\times 7\times 8\times\underbrace{1}_{9}\\
    &\quad\times 10\times 11\times\underbrace{4}_{12}\times 13\times 14\times\underbrace{5}_{15}\times 16\times 17\times\underbrace{2}_{18}\\
    &\quad\times 19\times 20\times\underbrace{7}_{21}\times 22\times 23\times\underbrace{8}_{24}\times 25\times 26\times\underbrace{1}_{27}\\
    &\quad\times 28\times 29\times\underbrace{10}_{30}\times 31\times 32\\
    &\equiv 1\times 2\times \underbrace{1}_{3} \times 4\times 5\times \underbrace{2}_{6}\times 7\times 8\times\underbrace{1}_{9}\\
    &\quad\times 1\times 2\times\underbrace{4}_{12}\times 4\times 5\times\underbrace{5}_{15}\times 7\times 8\times\underbrace{2}_{18}\\
    &\quad\times 1\times 2\times\underbrace{7}_{21}\times 4\times 5\times\underbrace{8}_{24}\times 7\times 8\times\underbrace{1}_{27}\\
    &\quad\times 1\times 2\times\underbrace{1}_{30}\times 4\times 5\\
    &=(1\times 2\times 4\times 5\times 7\times 8)^{3}\times (1\times 2\times 4\times 5)\\
    &\quad\times\begin{pmatrix}\underbrace{1}_{3}\times\underbrace{2}_{6}\times\underbrace{1}_{9}\times\underbrace{4}_{12}\times\underbrace{5}_{15}\times\underbrace{2}_{18}\\\times\underbrace{7}_{21}\times\underbrace{8}_{24}\times\underbrace{1}_{27}\times\underbrace{1}_{30}\end{pmatrix}\pmod{9}.
    \end{aligned}
    $$
    
    Ta tách thành:
    
    -   Khối đầy đủ: tích các số từ $1$ đến $9$ không chia hết cho $3$, có $\lfloor 32/9\rfloor=3$ khối;
    -   Đuôi: tích các số không chia hết cho $3$ từ $1$ đến $32\bmod 9$;
    -   Tích các số chia hết cho $3$, chính là $(\lfloor 32/3\rfloor!)_3\bmod 9$.
    
    Đệ quy phần cuối, quy về bài nhỏ hơn.

Suy ra kết quả đệ quy:

???+ note "Kết quả đệ quy"
    Với $p$ nguyên tố và $\alpha,n$ nguyên dương:
    
    $$
    (n!)_p \equiv (\pm 1)^{\sum_{j\ge\alpha}\lfloor{n}/{p^j}\rfloor}\prod_{j\ge 0}F(\lfloor n/p^j\rfloor\bmod p^\alpha),
    $$
    
    trong đó $F(m) = \prod_{1\le k\le m,\ k\perp p} k\bmod{p^\alpha}$ và $\pm 1$ lấy như trên.

Cài đặt tương tự trường hợp mô-đun nguyên tố, chỉ khác vài chi tiết. Tiền xử lý có thể đặt ngoài hàm.

??? example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/math/code/factorial/fact-mod-pa.cpp:core"
    ```

Tiền xử lý $O(p^\alpha)$, mỗi truy vấn $O(\log_p n)$.

## Tính số mũ

Phần này bàn về tính số mũ của $p$ trong $n!$, tức $\nu_p(n!)$, dùng để tính hệ số nhị thức. Vì tử và mẫu đều có giai thừa, việc các thừa số $p$ triệt tiêu hay không quyết định phần dư cuối.

### Công thức Legendre

Số mũ của $p$ trong $n!$ tính bằng công thức Legendre và liên quan đến biểu diễn cơ số $p$ của $n$.

???+ note "Công thức Legendre"
    Với $n$ nguyên dương, số mũ của $p$ trong $n!$ là
    
    $$
    \nu_p(n!) = \sum_{i=1}^{\infty} \left\lfloor \dfrac{n}{p^i} \right\rfloor = \dfrac{n-S_p(n)}{p-1},
    $$
    
    trong đó $S_p(n)$ là tổng các chữ số của $n$ trong hệ cơ số $p$. Đặc biệt, $\nu_2(n!)=n-S_2(n)$.

??? note "Chứng minh"
    Vì
    
    $$
    n! = 1\times 2\times \cdots \times p\times \cdots \times 2p\times \cdots \times \lfloor n/p\rfloor p\times \cdots \times n.
    $$
    
    Tích các bội của $p$ là $p\times 2p\times \cdots \times \lfloor n/p\rfloor p=p^{\lfloor n/p\rfloor }\lfloor n/p\rfloor !$, và $\lfloor n/p\rfloor !$ có thể còn chứa $p$. Do đó:
    
    $$
    \nu_p(n!) = \lfloor n/p\rfloor + \nu_p(\lfloor n/p\rfloor!).
    $$
    
    Mở rộng ra được công thức Legendre.
    
    Để chứng minh đẳng thức thứ hai, viết $n$ ở hệ $p$:
    
    $$
    n = n_\ell p^{\ell} + \cdots + n_1 p + n_0 = \sum_{k=0}^\ell n_kp^k.
    $$
    
    Khi đó
    
    $$
    \begin{aligned}
    \nu_p(n!)
    &= \sum_{i=1}^{\ell}\left\lfloor\dfrac{n}{p^i}\right\rfloor 
    = \sum_{i=1}^\ell\sum_{k=i}^{\ell}n_kp^{k-i}
    = \sum_{k=1}^\ell n_k\sum_{i=1}^kp^{k-i} \\
    &= \sum_{k=1}^\ell n_k\dfrac{p^k-1}{p-1} 
    = \dfrac{\sum_{k=0}^\ell n_kp^k - \sum_{k=0}^\ell n_k}{p-1} 
    = \dfrac{n - S_p(n)}{p-1}.
    \end{aligned}
    $$

Cài đặt tham khảo:

??? example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/math/code/factorial/multiplicity.cpp:core"
    ```

Độ phức tạp $O(\log n)$.

### Định lý Kummer

Hệ số nhị thức modulo thường tạo cấu trúc phân hình, ví dụ tam giác Sierpinski là từ hệ số nhị thức mod 2.

Nếu phân tích kỹ, việc $p$ chia hệ số nhị thức liên quan đến việc trừ trong hệ cơ số $p$ có mượn hay không. Đó là **định lý Kummer**.

???+ note "Định lý Kummer"
    Số mũ của $p$ trong $\dbinom{m}{n}$ đúng bằng số lần mượn khi trừ $n$ khỏi $m$ trong hệ cơ số $p$, tức
    
    $$
    \nu_p\left(\dbinom{m}{n}\right)=\frac{S_p(n)+S_p(m-n)-S_p(m)}{p-1}.
    $$
    
    Đặc biệt, số mũ của $2$ trong tổ hợp là $\nu_2\left(\dbinom{m}{n}\right)=S_2(n)+S_2(m-n)-S_2(m)$.

??? note "Chứng minh"
    Dùng công thức Legendre:
    
    $$
    \begin{aligned}
    \nu_p\left(\dbinom{m}{n}\right)
    &=\nu_p(m!)-\nu_p(n!)-\nu_p((m-n)!)\\
    &=\sum_{i=1}^\infty\left(\left\lfloor\dfrac{m}{p^i}\right\rfloor-\left\lfloor\dfrac{n}{p^i}\right\rfloor-\left\lfloor\dfrac{m-n}{p^i}\right\rfloor\right)\\
    &=\frac{S_p(n)+S_p(m-n)-S_p(m)}{p-1}.
    \end{aligned}
    $$
    
    Biểu thức này chính là số lần mượn khi trừ $n$ khỏi $m$ ở hệ $p$. Nếu tại chữ số thứ $i$ (tính từ 1) phải mượn, thì phần trước đó của $m-n$ là $\left\lfloor\dfrac{m}{p^i}\right\rfloor-1-\left\lfloor\dfrac{n}{p^i}\right\rfloor$, do đó
    
    $$
    \left\lfloor\dfrac{m}{p^i}\right\rfloor-\left\lfloor\dfrac{n}{p^i}\right\rfloor-\left\lfloor\dfrac{m-n}{p^i}\right\rfloor = 1
    $$
    
    khi và chỉ khi có mượn; ngược lại là $0$. Vậy tổng là số lần mượn.

## Ví dụ

???+ example "Ví dụ [HDU 2973 - YAPTCHA](https://acm.hdu.edu.cn/showproblem.php?pid=2973)"
    Cho $n$, tính
    
    $$
    \sum_{k=1}^n\left\lfloor\frac{(3k+6)!+1}{3k+7}-\left\lfloor\frac{(3k+6)!}{3k+7}\right\rfloor\right\rfloor
    $$

??? note "Ý tưởng"
    Nếu $3k+7$ là số nguyên tố thì
    
    $$
    (3k+6)!\equiv-1\pmod{3k+7}
    $$
    
    Đặt $(3k+6)!+1=k(3k+7)$
    
    Khi đó
    
    $$
    \left\lfloor\frac{(3k+6)!+1}{3k+7}-\left\lfloor\frac{(3k+6)!}{3k+7}\right\rfloor\right\rfloor=\left\lfloor k-\left\lfloor k-\frac{1}{3k+7}\right\rfloor\right\rfloor=1
    $$
    
    Nếu $3k+7$ không nguyên tố, thì $(3k+7)\mid(3k+6)!$, tức
    
    $$
    (3k+6)!\equiv 0\pmod{3k+7}
    $$
    
    Đặt $(3k+6)!=k(3k+7)$, suy ra
    
    $$
    \left\lfloor\frac{(3k+6)!+1}{3k+7}-\left\lfloor\frac{(3k+6)!}{3k+7}\right\rfloor\right\rfloor=\left\lfloor k+\frac{1}{3k+7}-k\right\rfloor=0
    $$
    
    Do đó
    
    $$
    \sum_{k=1}^n\left\lfloor\frac{(3k+6)!+1}{3k+7}-\left\lfloor\frac{(3k+6)!}{3k+7}\right\rfloor\right\rfloor=\sum_{k=1}^n[3k+7\text{ là số nguyên tố}]
    $$

??? example "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/factorial/wilson_1.cpp"
    ```

## Tài liệu tham khảo

-   冯克勤．《初等数论及其应用》．
-   [Wilson's theorem - Wikipedia](https://en.wikipedia.org/wiki/Wilson%27s_theorem)
-   [Legendre's formula - Wikipedia](https://en.wikipedia.org/wiki/Legendre%27s_formula)

**Trang này chủ yếu dịch từ bài viết [Вычисление факториала по модулю](http://e-maxx.ru/algo/modular_factorial) và bản dịch tiếng Anh [Factorial modulo p](https://cp-algorithms.com/algebra/factorial-modulo.html). Bản tiếng Nga cấp phép Public Domain + Leave a Link; bản tiếng Anh cấp phép CC-BY-SA 4.0. Nội dung có chỉnh sửa.**
