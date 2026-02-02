author: PeterlitsZo, Tiphereth-A

Bài viết này thảo luận định lý nhỏ Fermat, định lý Euler và các mở rộng của chúng. Các định lý này giải quyết việc tính lũy thừa với số mũ rất lớn dưới mọi mô-đun.

## Định lý nhỏ Fermat

**Định lý nhỏ Fermat** (Fermat's little theorem) là một trong những định lý cơ bản nhất của số học. Nó cũng là nền tảng lý thuyết của [kiểm tra nguyên tố Fermat](./prime.md#fermat-素性测试).

???+ note "Định lý nhỏ Fermat"
    Cho $p$ là số nguyên tố. Với mọi số nguyên $a$ và $p\nmid a$, ta có $a^{p-1}\equiv 1\pmod p$.

???+ note "Định lý"
    Cho $p$ là số nguyên tố. Với mọi số nguyên $a$, ta có $a^{p}\equiv a\pmod p$.

Hai quan hệ đồng dư này tương đương khi $p\nmid a$; còn khi $p\mid a$ thì $a^p\equiv 0\equiv a\pmod p$ hiển nhiên đúng. Vì vậy hai mệnh đề là tương đương và đều thường gọi là định lý nhỏ Fermat.

??? note "Chứng minh 1"
    Cho $p$ là số nguyên tố và $p\nmid a$. Trước hết chứng minh rằng với $i=1,2,\cdots,p-1$, các số dư $ia \bmod p$ là khác nhau. Dùng phản chứng: nếu có $1\le i < j < p$ sao cho
    
    $$
    ia \bmod p = ja \bmod p. \iff (j-i)a\equiv 0.\pmod p
    $$
    
    Nhưng $(j-i)$ và $a$ đều không chia hết cho $p$, mâu thuẫn.
    
    Nói cách khác, các số dư này là một hoán vị của $\{1,2,\cdots,p-1\}$. Do đó
    
    $$
    \prod_{i=1}^{p-1}i = \prod_{i=1}^{p-1}(ia\bmod p) \equiv \prod_{i=1}^{p-1}ia = a^{p-1}\prod_{i=1}^{p-1}i.\pmod p
    $$
    
    Suy ra
    
    $$
    (a^{p-1}-1)\prod_{i=1}^{p-1}i \equiv 0. \pmod{p}
    $$
    
    Nghĩa là vế trái là bội của $p$, nhưng $i=1,2,\cdots, p-1$ đều không chia hết cho $p$, nên chỉ có thể $p\mid (a^{p-1}-1)$, tức định lý Fermat đúng.

??? note "Chứng minh 2"
    Lưu ý rằng dạng thứ hai của định lý nhỏ Fermat đúng với mọi $a\in\mathbf N$, nên có thể dùng quy nạp. Trường hợp số nguyên âm chuyển về số không âm dễ dàng.
    
    Cơ sở quy nạp là $0^p\equiv 0\pmod p$, hiển nhiên. Giả sử đúng với $a\in\mathbf N$, chứng minh với $a+1$. Theo nhị thức Newton:
    
    $$
    (a+1)^p=a^p+\binom{p}{1}a^{p-1}+\binom{p}{2}a^{p-2}+\cdots +\binom{p}{p-1}a+1.
    $$
    
    Ngoài hai hạng đầu cuối, các tổ hợp $\dbinom{p}{k} = \dfrac{p!}{k!(p-k)!}$ đều có tử chia hết cho $p$ mà mẫu không, nên với $k\neq 0,p$ chúng đều là bội của $p$. Do đó
    
    $$
    (a+1)^p \equiv a^p + 1\equiv a + 1. \pmod{p}
    $$
    
    Bước thứ hai dùng giả thiết quy nạp. Suy ra định lý đúng.

Nghịch đảo của định lý nhỏ Fermat không đúng. Dù với mọi $a$ nguyên tố cùng nhau với $n$ đều có $a^{n-1}\equiv 1\pmod n$, $n$ vẫn có thể không nguyên tố. Xem thêm [kiểm tra nguyên tố Fermat](./prime.md#fermat-素性测试).

## Định lý Euler

**Định lý Euler** (Euler's theorem) mở rộng định lý Fermat cho mô-đun bất kỳ, nhưng vẫn yêu cầu cơ sở và mô-đun nguyên tố cùng nhau.

???+ note "Định lý Euler"
    Với số nguyên $m>0$ và số nguyên $a$ thỏa $\gcd(a,m)=1$, ta có $a^{\varphi(m)}\equiv 1\pmod{m}$, trong đó $\varphi(\cdot)$ là [hàm Euler](./euler-totient.md).

??? note "Chứng minh"
    Tương tự chứng minh 1 của Fermat, lấy một dãy nguyên tố cùng nhau với $m$ và thao tác. Xét tập
    
    $$
    R = \{r\in\mathbf N : 0 < r < m,~\gcd(r,m)=1\}.
    $$
    
    Đây là [hệ thặng dư tối giản](./basic.md#同余类与剩余系) modulo $m$. Theo định nghĩa, $|R|=\varphi(m)$. Nhân tất cả phần tử với $a$ tương đương hoán vị lại tập:
    
    $$
    R = \{ar\bmod m: r\in R\}.
    $$
    
    Vì $\gcd(ar,m)=1$ và các $ar\bmod m$ khác nhau. Do đó
    
    $$
    \prod_{r\in R}r \equiv \prod_{r\in R}ar = a^{\varphi(m)}\prod_{r\in R}r. \pmod{m}
    $$
    
    Khử $\prod_{r\in R}r$ như trước, được $a^{\varphi(m)}\equiv 1\pmod m$.

Với $p$ nguyên tố, $\varphi(p)=p-1$, nên Fermat là trường hợp đặc biệt. Ngoài ra, số mũ $\varphi(m)$ không nhất thiết là nhỏ nhất; có thể cải thiện thành $\lambda(m)$, trong đó $\lambda$ là [hàm Carmichael](./primitive-root.md#carmichael-函数). Xem thêm [nhóm nhân các lớp đồng dư](../algebra/ring-theory.md#应用整数同余类的乘法群).

## Định lý Euler mở rộng

Định lý Euler mở rộng[^ex-euler] tiếp tục mở rộng sang trường hợp cơ sở và mô-đun không nguyên tố cùng nhau. Nó giải quyết việc tính lũy thừa với mô-đun bất kỳ, bằng cách giảm số mũ về dưới $2\varphi(m)$, từ đó tính bằng [lũy thừa nhanh](../binary-exponentiation.md) trong $O(\log\varphi(m))$.

???+ note "Định lý Euler mở rộng"
    Với mọi số nguyên dương $m$, số nguyên $a$ và số nguyên không âm $k$, ta có
    
    $$
    a^k \equiv \begin{cases}
    a^{k \bmod \varphi(m)},                &\gcd(a,m) =  1,                   \\
    a^k,                                   &\gcd(a,m)\ne 1, k <   \varphi(m), \\
    a^{(k \bmod \varphi(m)) + \varphi(m)}, &\gcd(a,m)\ne 1, k \ge \varphi(m).
    \end{cases} \pmod m
    $$

Trường hợp thứ hai nói rằng nếu $k < \varphi(m)$ thì không cần giảm thêm, cứ tính nhanh trực tiếp; còn trường hợp thứ ba khác trường hợp đầu ở chỗ sau khi lấy dư có cần cộng thêm $\varphi(m)$ hay không. Dĩ nhiên có thể gộp trường hợp đầu vào hai trường hợp còn lại.

### Trực quan

Trước khi chứng minh chặt chẽ, ta có thể hiểu trực quan.

![fermat1](./images/fermat.svg)

Xét phần dư $a^k\bmod m$ khi $k$ tăng. Vì phần dư nằm trong $[0,m)$ mà $k$ vô hạn, nên các nút dư và cạnh $a^k\bmod m \to a^{k+1}\bmod m$ tạo thành chu trình.

Định lý Euler mở rộng nói rằng chu trình có thể là chu trình thuần (trường hợp 1) hoặc chu trình có đuôi (trường hợp 2,3). Chu trình thuần không có nút có hai tiền nhiệm, còn chu trình có đuôi thì có. Vì vậy, nếu biết độ dài chu kỳ và độ dài phần đuôi, ta có thể giảm số mũ.

### Chứng minh chặt chẽ

??? note "Chứng minh"
    Trước hết chứng minh tồn tại $k_0\in\mathbf N$ sao cho $a$ và $m':=\dfrac{m}{\gcd(a^{k_0},m)}$ nguyên tố cùng nhau. Gọi $\nu_p(n)$ là số mũ của $p$ trong phân tích nguyên tố của $n$. Lấy
    
    $$
    k_0 = \max\left\{\left\lceil\dfrac{\nu_p(m)}{\nu_p(a)}\right\rceil : \nu_p(a)>0\right\}.
    $$
    
    Vì mọi ước chung của $a$ và $m$ đều đã nằm trong $a^{k_0}$, nên $a$ nguyên tố cùng nhau với $m'=\dfrac{m}{\gcd(a^{k_0},m)}$.
    
    Với $k\ge k_0$, xét
    
    $$
    b\equiv a^k. \pmod m
    $$
    
    Do $\gcd(a^{k_0},m)=\gcd(a^k,m)\mid b$, chia cả hai vế (và mô-đun) cho $\gcd(a^{k_0},m)$:
    
    $$
    \dfrac{b}{\gcd(a^{k_0},m)} = \dfrac{a^{k_0}}{\gcd(a^{k_0},m)}\cdot a^{k-k_0}. \pmod{m'}
    $$
    
    Vì $a$ nguyên tố cùng nhau với $m'$, áp dụng Euler:
    
    $$
    \dfrac{b}{\gcd(a^{k_0},m)} \equiv \dfrac{a^{k_0}}{\gcd(a^{k_0},m)}\cdot a^{(k-k_0)\bmod\varphi(m')}. \pmod{m'}
    $$
    
    Nhân lại $\gcd(a^{k_0},m)$:
    
    $$
    b \equiv a^{k_0}\cdot a^{(k-k_0)\bmod\varphi(m')} = a^{k_0 + (k-k_0)\bmod\varphi(m')}. \pmod{m}
    $$
    
    Ta được dạng của định lý Euler mở rộng. Chu kỳ là $\varphi(m')$, đuôi là $k_0$.
    
    Các tham số này chặt hơn, nhưng khó tính. Có thể nới ra thành dạng trong định lý. Trước hết, từ [công thức Euler](./euler-totient.md) suy ra $\varphi(m')\mid\varphi(m)$ nên $\varphi(m)$ cũng là một chu kỳ. Thứ hai, $k_0$ có thể nới lên $\varphi(m)$. Vì với mọi $m\in\mathbf N_+$ và $p\mid m$:
    
    $$
    \begin{aligned}
    \varphi(m) &\ge \varphi(p^{\nu_p(m)}) = (p-1)p^{\nu_p(m)-1} \ge p^{\nu_p(m)-1} \\
    &= (1+(p-1))^{\nu_p(m)-1} \ge 1 + (p-1)(\nu_p(m)-1) \\
    &\ge 1 + (\nu_p(m)-1) = \nu_p(m).
    \end{aligned}
    $$
    
    Dòng hai dùng khai triển nhị thức, chỉ giữ hằng và bậc nhất. Do đó
    
    $$
    k_0 \le \max\{\nu_p(m):p\in\mathbf P\}\le \varphi(m).
    $$
    
    Suy ra kết luận.

## Ví dụ

Phần này minh họa ứng dụng của định lý Euler mở rộng để tính tháp lũy thừa theo mô-đun. **Tháp lũy thừa** (power tower) là biểu thức dạng $A\uparrow(B\uparrow(C\uparrow(D\uparrow\cdots)))$ với ký hiệu mũ Knuth, và $A,B,C,D,\cdots$ là các số không âm.

???+ example "[Library Checker - Tetration Mod](https://judge.yosupo.jp/problem/tetration_mod)"
    Có $T$ bộ test. Mỗi test cho $A,B,M$, tính $(A\uparrow\uparrow B)\bmod M$. Với $A\uparrow\uparrow B$ là tháp gồm $B$ số $A$, hay
    
    $$
    A \uparrow\uparrow B =
    \begin{cases}
    1 , & B = 0,\\
    A\uparrow(A\uparrow\uparrow(B-1)), & B > 0.
    \end{cases}
    $$
    
    Quy ước $0^0=1$.

??? note "Lời giải"
    Dựa vào định nghĩa, tính đệ quy. Để tính $(A\uparrow\uparrow B)\bmod M$, chỉ cần dùng định lý Euler mở rộng để tính $(A\uparrow\uparrow(B-1))\bmod\varphi(M)$. Vì $\varphi(\varphi(n)) \le n/2$ với mọi $n\ge 2$, quá trình đệ quy dừng trong $O(\log M)$. Vì cần phân biệt kết quả có nhỏ hơn mô-đun hay không, khi lấy dư cần kiểm tra thêm. Cũng cần xử lý biên.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/fermat/tetration.cpp"
    ```

## Bài tập

-   [Luogu P5091【模板】扩展欧拉定理](https://www.luogu.com.cn/problem/P5091)
-   [Codeforces 906 D. Power Tower](https://codeforces.com/problemset/problem/906/D)
-   [Luogu P3747 \[六省联考 2017\] 相逢是问候](https://www.luogu.com.cn/problem/P3747)
-   [Luogu P4139 上帝与集合的正确用法](https://www.luogu.com.cn/problem/P4139)
-   [Luogu P3934 \[Ynoi Easy Round 2016\] 炸脖龙 I](https://www.luogu.com.cn/problem/P3934)
-   [Luogu P6736「Wdsr-2」白泽教育](https://www.luogu.com.cn/problem/P6736)

## Tài liệu tham khảo và chú thích

-   [Fermat's little theorem - Wikipedia](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem)
-   [Euler's theorem - Wikipedia](https://en.wikipedia.org/wiki/Euler%27s_theorem)
-   Hardy, Godfrey Harold, and Edward Maitland Wright. An introduction to the theory of numbers. Oxford university press, 1979.

[^ex-euler]: Tên này chủ yếu xuất hiện trong cộng đồng thi lập trình, không phải tên gọi phổ biến của kết luận.
