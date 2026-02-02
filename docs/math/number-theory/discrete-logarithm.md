## Định nghĩa

Kiến thức trước: [Bậc và căn nguyên thủy](./primitive-root.md).

Định nghĩa logarit rời rạc tương tự logarit thường. Lấy mô-đun nguyên dương $m$ có căn nguyên thủy, đặt một căn nguyên thủy là $g$. Với số nguyên $a$ thỏa $(a,m)=1$, ta biết tồn tại duy nhất $0\leq k<\varphi(m)$ sao cho

$$
g^k\equiv a\pmod m
$$

Ta gọi $k$ là logarit rời rạc cơ số $g$ modulo $m$, ký hiệu $k=\operatorname{ind}_g a$, khi không gây nhầm lẫn có thể viết $\operatorname{ind} a$.

Hiển nhiên $\operatorname{ind}_g 1=0$, $\operatorname{ind}_g g=1$.

## Tính chất

Tính chất của logarit rời rạc cũng tương tự logarit thường.

???+ note "Tính chất"
    Gọi $g$ là căn nguyên thủy modulo $m$, $(a,m)=(b,m)=1$, thì:
    
    1.  $\operatorname{ind}_g(ab)\equiv\operatorname{ind}_g a+\operatorname{ind}_g b\pmod{\varphi(m)}$
    
        Suy ra $(\forall n\in\mathbf{N}),~~\operatorname{ind}_g a^n\equiv n\operatorname{ind}_g a\pmod{\varphi(m)}$
    2.  Nếu $g_1$ cũng là căn nguyên thủy modulo $m$ thì $\operatorname{ind}_g a\equiv\operatorname{ind}_{g_1}a \cdot \operatorname{ind}_g g_1\pmod{\varphi(m)}$
    3.  $a\equiv b\pmod m\iff \operatorname{ind}_g a=\operatorname{ind}_g b$

???+ note "Chứng minh"
    1.  $g^{\operatorname{ind}_g(ab)}\equiv ab\equiv g^{\operatorname{ind}_g a}g^{\operatorname{ind}_g b}\equiv g^{\operatorname{ind}_g a+\operatorname{ind}_g b}\pmod m$
    2.  Đặt $x=\operatorname{ind}_{g_1}a$ thì $a\equiv g_1^x\pmod m$. Đặt $y=\operatorname{ind}_g g_1$ thì $g_1\equiv g^y\pmod m$.
    
        Do đó $a\equiv g^{xy}\pmod m$, tức $\operatorname{ind}_g a\equiv xy\equiv\operatorname{ind}_{g_1}a \cdot \operatorname{ind}_g g_1\pmod{\varphi(m)}$
    3.  Lưu ý
    
        $$
        \begin{aligned}
            \operatorname{ind}_g a=\operatorname{ind}_g b&\iff \operatorname{ind}_g a\equiv\operatorname{ind}_g b\pmod{\varphi(m)}\\
            &\iff g^{\operatorname{ind}_g a}\equiv g^{\operatorname{ind}_g b}\pmod m\\
            &\iff a\equiv b\pmod m
        \end{aligned}
        $$

## Thuật toán BSGS (bước nhỏ bước lớn)

Hiện chưa có thuật toán đa thức thời gian cổ điển cho bài toán logarit rời rạc (kích thước đầu vào là số bit). Trong mật mã, điều này làm nền cho nhiều hệ mật mã bất đối xứng như [Ed25519](https://en.wikipedia.org/wiki/EdDSA#Ed25519).

Trong lập trình thi đấu, BSGS (baby-step giant-step) thường dùng để giải logarit rời rạc. Cụ thể, với $a,b,m\in\mathbf{Z}^+$, thuật toán giải trong $O(\sqrt{m})$ phương trình

$$
a^x \equiv b \pmod m
$$

trong đó $a\perp m$. Nghiệm $x$ thỏa $0 \le x < m$. (Lưu ý $m$ không nhất thiết là số nguyên tố.)

### Mô tả thuật toán

Đặt $x = A \left \lceil \sqrt m \right \rceil - B$ với $0\le A,B \le \left \lceil \sqrt m \right \rceil$. Khi đó $a^{A\left \lceil \sqrt m \right \rceil -B} \equiv b \pmod m$, suy ra $a^{A\left \lceil \sqrt m \right \rceil} \equiv ba^B \pmod m$.

Vì biết $a,b$, ta tính trước mọi giá trị $ba^B$ (duyệt $B$), lưu vào `hash`/`map`, rồi tính $a^{A\left \lceil \sqrt m \right \rceil}$ (duyệt $A$), tìm trùng khớp với $ba^B$. Khi đó $x=A \left \lceil \sqrt m \right \rceil - B$.

Vì $A,B < \left \lceil \sqrt m \right \rceil$, độ phức tạp là $\Theta(\sqrt m)$; dùng `map` thêm một $\log$.

??? note "Vì sao cần $a$ và $m$ nguyên tố cùng nhau"
    Ta cần từ $a^{A\left \lceil \sqrt m \right \rceil} \equiv ba^B \pmod m$ suy ra $a^{A\left \lceil \sqrt m \right \rceil -B} \equiv b \pmod m$, tức chia hai vế cho $a^B$. Do đó phải có $a^B \perp m$, tức $a\perp m$.

## Thuật toán BSGS mở rộng

Với $a,b,m\in\mathbf{Z}^+$, giải

$$
a^x\equiv b\pmod m
$$

trong đó $a,m$ không nhất thiết nguyên tố cùng nhau.

Khi $(a, m)=1$ thì $a$ có nghịch đảo modulo $m$, nên dùng BSGS. Ta biến đổi để chúng trở nên nguyên tố cùng nhau.

Cụ thể, đặt $d_1=(a, m)$. Nếu $d_1\nmid b$ thì vô nghiệm. Nếu có, chia hai vế cho $d_1$:

$$
\frac{a}{d_1}\cdot a^{x-1}\equiv \frac{b}{d_1}\pmod{\frac{m}{d_1}}
$$

Nếu $a$ và $\frac{m}{d_1}$ vẫn không nguyên tố cùng nhau thì tiếp tục: $d_2=\left(a, \frac{m}{d_1}\right)$. Nếu $d_2\nmid \frac{b}{d_1}$ thì vô nghiệm; nếu có, chia tiếp:

$$
\frac{a^2}{d_1d_2}\cdot a^{x-2}≡\frac{b}{d_1d_2} \pmod{\frac{m}{d_1d_2}}
$$

Lặp cho đến khi $a\perp \dfrac{m}{d_1d_2\cdots d_k}$.

Đặt $D=\prod_{i=1}^kd_i$, phương trình trở thành:

$$
\frac{a^k}{D}\cdot a^{x-k}\equiv\frac{b}{D} \pmod{\frac{m}{D}}
$$

Vì $a\perp\dfrac{m}{D}$ nên $\dfrac{a^k}{D}\perp \dfrac{m}{D}$, do đó có nghịch đảo. Đưa sang vế phải, ta được một bài BSGS chuẩn, giải $x-k$ rồi cộng $k$.

Lưu ý có thể có nghiệm $\le k$, nên trước khi khử nhân tử hãy kiểm tra $\Theta(k)$ giá trị $a^i\equiv b \pmod m$ để tránh bỏ sót.

## Bài tập

-   [SPOJ MOD](https://www.spoj.com/problems/MOD/) mẫu
-   [SDOI2013 随机数生成器](https://www.luogu.com.cn/problem/P3306)
-   [SGU261 Discrete Roots](https://codeforces.com/problemsets/acmsguru/problem/99999/261) mẫu
-   [SDOI2011 计算器](https://loj.ac/problem/10214) mẫu
-   [Luogu4195【模板】exBSGS/Spoj3105 Mod](https://www.luogu.com.cn/problem/P4195) mẫu
-   [Codeforces - Lunar New Year and a Recursive Sequence](https://codeforces.com/contest/1106/problem/F)
-   [LOJ6542 离散对数](https://loj.ac/problem/6542) phương pháp index calculus, không phải mẫu

**Một phần nội dung và code của trang này dịch từ [Дискретное извлечение корня](http://e-maxx.ru/algo/discrete_root) và bản dịch tiếng Anh [Discrete Root](https://cp-algorithms.com/algebra/discrete-root.html). Bản tiếng Nga: Public Domain + Leave a Link; bản tiếng Anh: CC-BY-SA 4.0.**

## Tài liệu tham khảo

1.  [Discrete logarithm - Wikipedia](https://en.wikipedia.org/wiki/Discrete_logarithm)
2.  潘承洞，潘承彪．初等数论．
3.  冯克勤．初等数论及其应用．
