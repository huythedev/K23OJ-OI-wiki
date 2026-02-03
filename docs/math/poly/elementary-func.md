author: 97littleleaf11, abc1763613206, CCXXXI, EndlessCheng, Enter-tainer, fps5283, Great-designer, H-J-Granger, hly1204, hsfzLZH1, huayucaiji, Ir1d, kenlig, Marcythm, ouuan, SamZhangQingChuan, shuzhouliu, sshwy, StudyingFather, test12345-pupil, Tiphereth-A, TrisolarisHD, untitledunrevised

Trang này chứa các phép toán hàm sơ cấp thường gặp của đa thức. Cụ thể, gồm các nội dung sau:

1.  Nghịch đảo đa thức
2.  Căn bậc hai đa thức
3.  Chia đa thức
4.  Lấy mô-đun đa thức
5.  Hàm mũ đa thức
6.  Hàm logarit đa thức
7.  Hàm lượng giác đa thức
8.  Hàm lượng giác ngược đa thức

??? note "Hàm sơ cấp và hàm không sơ cấp"
    Định nghĩa hàm sơ cấp như sau[^ref1]：
    
    Nếu trong trường $F$ tồn tại ánh xạ $u\to \partial u$ thỏa:
    
    1.  $\partial(u+v)=\partial u+\partial v$
    2.  $\partial(uv)=u\partial v+v\partial u$
    
    thì gọi trường đó là **trường vi phân**.
    
    Nếu trong trường vi phân $F$, một hàm $u$ thỏa một trong các điều kiện sau thì gọi $u$ là hàm sơ cấp:
    
    1.  $u$ là hàm đại số trên $F$.
    2.  $u$ là hàm mũ trên $F$, tức tồn tại $a\in F$ sao cho $\partial u=u\partial a$.
    3.  $u$ là hàm logarit trên $F$, tức tồn tại $a\in F$ sao cho $\partial u=\frac{\partial a}{a}$.
    
    Một số hàm sơ cấp thường gặp:
    
    1.  Hàm đại số: tồn tại đa thức hữu hạn bậc $P$ sao cho $P(f(x))=0$, ví dụ $2x+1$,$\sqrt{x}$,$(1+x^2)^{-1}$,$|x|$.
    2.  Hàm mũ
    3.  Hàm logarit
    4.  Hàm lượng giác
    5.  Hàm lượng giác ngược
    6.  Hàm hyperbolic
    7.  Hàm hyperbolic ngược
    8.  Hợp của các hàm trên, ví dụ:
    
        $$
        \frac{\mathrm{e}^{\tan x}}{1+x^2}\sin\left(\sqrt{1+\ln^2 x}\right)
        $$
    
        $$
        -\mathrm{i} \ln\left(x+\mathrm{i}\sqrt{1-x^2}\right)
        $$
    
    Một số hàm không sơ cấp thường gặp:
    
    1.  Hàm sai số:
    
        $$
        \operatorname{erf}(x):=\frac{2}{\sqrt{\pi}}\int_{0}^{x}\exp\left(-t^2\right)\mathrm{d}t
        $$

## Nghịch đảo đa thức

Cho đa thức $f\left(x\right)$, tìm $f^{-1}\left(x\right)$.

### Cách giải

#### Phương pháp nhân đôi

Trước hết, dễ thấy

$$
\left[x^{0}\right]f^{-1}\left(x\right)=\left(\left[x^{0}\right]f\left(x\right)\right)^{-1}
$$

Giả sử đã tìm được nghịch đảo $f^{-1}_{0}\left(x\right)$ của $f\left(x\right)$ theo mô-đun $x^{\left\lceil\frac{n}{2}\right\rceil}$. Ta có:

$$
\begin{aligned}
    f\left(x\right)f^{-1}_{0}\left(x\right)&\equiv 1 &\pmod{x^{\left\lceil\frac{n}{2}\right\rceil}}\\
    f\left(x\right)f^{-1}\left(x\right)&\equiv 1 &\pmod{x^{\left\lceil\frac{n}{2}\right\rceil}}\\
    f^{-1}\left(x\right)-f^{-1}_{0}\left(x\right)&\equiv 0 &\pmod{x^{\left\lceil\frac{n}{2}\right\rceil}}
\end{aligned}
$$

Bình phương hai vế:

$$
f^{-2}\left(x\right)-2f^{-1}\left(x\right)f^{-1}_{0}\left(x\right)+f^{-2}_{0}\left(x\right)\equiv 0 \pmod{x^{n}}
$$

Nhân hai vế với $f\left(x\right)$ và chuyển vế:

$$
f^{-1}\left(x\right)\equiv f^{-1}_{0}\left(x\right)\left(2-f\left(x\right)f^{-1}_{0}\left(x\right)\right) \pmod{x^{n}}
$$

Đệ quy là xong.

**Độ phức tạp thời gian**

$$
T\left(n\right)=T\left(\frac{n}{2}\right)+O\left(n\log{n}\right)=O\left(n\log{n}\right)
$$

#### Newton's Method

Xem [Newton's Method](./newton.md#newtons-method).

#### Phương pháp Graeffe

Muốn tính $f^{-1}(x)\bmod x^{2n}$, xét

$$
\begin{aligned}
f^{-1}(x)\bmod x^{2n}&= f(-x)(f(x)f(-x))^{-1}\bmod x^{2n}\\
&=f(-x)g^{-1}(x^2)\bmod x^{2n}
\end{aligned}
$$

Chỉ cần tìm $g^{-1}(x)\bmod x^n$ là có thể khôi phục $g^{-1}(x^2)\bmod x^{2n}$ vì $f(x)f(-x)$ là hàm chẵn, độ phức tạp tương tự.

### Mã

??? note "Nghịch đảo đa thức"
    ```cpp
    constexpr int MAXN = 262144;
    constexpr int mod = 998244353;
    
    using i64 = long long;
    using poly_t = int[MAXN];
    using poly = int *const;
    
    void polyinv(const poly &h, const int n, poly &f) {
      /* f = 1 / h = f_0 (2 - f_0 h) */
      static poly_t inv_t;
      std::fill(f, f + n + n, 0);
      f[0] = fpow(h[0], mod - 2);
      for (int t = 2; t <= n; t <<= 1) {
        const int t2 = t << 1;
        std::copy(h, h + t, inv_t);
        std::fill(inv_t + t, inv_t + t2, 0);
    
        DFT(f, t2);
        DFT(inv_t, t2);
        for (int i = 0; i != t2; ++i)
          f[i] = (i64)f[i] * sub(2, (i64)f[i] * inv_t[i] % mod) % mod;
        IDFT(f, t2);
    
        std::fill(f + t, f + t2, 0);
      }
    }
    ```

### Bài tập mẫu

1.  Đếm đồ thị vô hướng liên thông đơn có nhãn: [「POJ 1737」Connected Graph](http://poj.org/problem?id=1737)

## Căn bậc hai đa thức

Cho đa thức $g\left(x\right)$, tìm $f\left(x\right)$ sao cho:

$$
f^{2}\left(x\right)\equiv g\left(x\right) \pmod{x^{n}}
$$

### Cách giải

#### Phương pháp nhân đôi

Trước hết xét trường hợp $\left[x^0\right]g(x) \neq 0$.

Dễ thấy:

$$
\left[x^0\right]f(x) = \sqrt{\left[x^0\right]g(x)}
$$

Nếu $\left[x^0\right]g(x)$ không có căn bậc hai thì $g(x)$ không có căn bậc hai.

> $\left[x^0\right]g(x)$ có thể có nhiều căn bậc hai, chọn khác nhau sẽ cho $f(x)$ khác nhau.

Giả sử đã có căn bậc hai $f_{0}\left(x\right)$ của $g\left(x\right)$ theo mô-đun $x^{\left\lceil\frac{n}{2}\right\rceil}$, ta có:

$$
\begin{aligned}
    f_{0}^{2}\left(x\right)&\equiv g\left(x\right) &\pmod{x^{\left\lceil\frac{n}{2}\right\rceil}}\\
    f_{0}^{2}\left(x\right)-g\left(x\right)&\equiv 0 &\pmod{x^{\left\lceil\frac{n}{2}\right\rceil}}\\
    \left(f_{0}^{2}\left(x\right)-g\left(x\right)\right)^{2}&\equiv 0 &\pmod{x^{n}}\\
    \left(f_{0}^{2}\left(x\right)+g\left(x\right)\right)^{2}&\equiv 4f_{0}^{2}\left(x\right)g\left(x\right) &\pmod{x^{n}}\\
    \left(\frac{f_{0}^{2}\left(x\right)+g\left(x\right)}{2f_{0}\left(x\right)}\right)^{2}&\equiv g\left(x\right) &\pmod{x^{n}}\\
    \frac{f_{0}^{2}\left(x\right)+g\left(x\right)}{2f_{0}\left(x\right)}&\equiv f\left(x\right) &\pmod{x^{n}}\\
    2^{-1}f_{0}\left(x\right)+2^{-1}f_{0}^{-1}\left(x\right)g\left(x\right)&\equiv f\left(x\right) &\pmod{x^{n}}
\end{aligned}
$$

Dùng nhân đôi để tính.

**Độ phức tạp thời gian**

$$
T\left(n\right)=T\left(\frac{n}{2}\right)+O\left(n\log{n}\right)=O\left(n\log{n}\right)
$$

Một cách khác có hằng số nhỏ là khi nhân đôi $f\left(x\right)$ thì đồng thời duy trì $f^{-1}\left(x\right)$ thay vì mỗi lần đều求逆.

> Khi $\left[x^{0}\right]g\left(x\right)\neq 1$, có thể cần dùng phần dư bậc hai để tính $\left[x^{0}\right]f\left(x\right)$.

Phương pháp trên cần nghịch đảo của $f_{0}(x)$ nên hạng tự do không được bằng $0$.

Nếu $\left[x^0\right]g(x) = 0$, ta phân tích $g(x)$ thành $x^{k}h(x)$ với $\left[x^0\right]h(x) \not = 0$.

-   Nếu $k$ lẻ thì $g(x)$ không có căn bậc hai.
-   Nếu $k$ chẵn thì tính $\sqrt{h(x)}$, rồi $f(x) \equiv x^{k/2} \sqrt{h(x)} \pmod{x^{n}}$.

??? note "Bài mẫu Luogu [P5205【模板】多项式开根](https://www.luogu.com.cn/problem/P5205) mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/poly/sqrt/sqrt_1.cpp"
    ```

#### Newton's Method

Xem [Newton's Method](./newton.md#newtons-method).

### Bài tập mẫu

1.  [「Codeforces Round #250」E. The Child and Binary Tree](https://codeforces.com/contest/438/problem/E)

## Chia đa thức & Lấy mô-đun

Cho đa thức $f\left(x\right),g\left(x\right)$, tìm thương $Q\left(x\right)$ và dư $R\left(x\right)$ khi $f(x)$ chia cho $g(x)$.

### Cách giải

Nếu có thể loại bỏ ảnh hưởng của $R\left(x\right)$ thì có thể giải bằng [nghịch đảo đa thức](#多项式求逆).

Xét biến đổi

$$
f^{R}\left(x\right)=x^{\operatorname{deg}{f}}f\left(\frac{1}{x}\right)
$$

bản chất là đảo ngược các hệ số của $f\left(x\right)$.

Đặt $n=\operatorname{deg}{f},m=\operatorname{deg}{g}$.

Thay $x$ bằng $\frac{1}{x}$ trong $f\left(x\right)=Q\left(x\right)g\left(x\right)+R\left(x\right)$ rồi nhân hai vế với $x^{n}$:

$$
\begin{aligned}
    x^{n}f\left(\frac{1}{x}\right)&=x^{n-m}Q\left(\frac{1}{x}\right)x^{m}g\left(\frac{1}{x}\right)+x^{n-m+1}x^{m-1}R\left(\frac{1}{x}\right)\\
    f^{R}\left(x\right)&=Q^{R}\left(x\right)g^{R}\left(x\right)+x^{n-m+1}R^{R}\left(x\right)
\end{aligned}
$$

Lưu ý $R^{R}\left(x\right)$ có hệ số bắt đầu từ $x^{n-m+1}$, nên lấy mô-đun $x^{n-m+1}$ để loại bỏ ảnh hưởng của $R^{R}\left(x\right)$.

Vì bậc của $Q^{R}\left(x\right)$ là $\left(n-m\right)<\left(n-m+1\right)$ nên $Q^{R}\left(x\right)$ không bị ảnh hưởng.

Do đó:

$$
f^{R}\left(x\right)\equiv Q^{R}\left(x\right)g^{R}\left(x\right)\pmod{x^{n-m+1}}
$$

Dùng nghịch đảo đa thức để tìm $Q\left(x\right)$, rồi thay ngược để có $R\left(x\right)$.

**Độ phức tạp** $O\left(n\log{n}\right)$.

## Hàm logarit đa thức & hàm mũ đa thức

Cho đa thức $f(x)$, tính $\ln{f(x)}$ và $\exp{f(x)}$ theo mô-đun $x^{n}$.

### Cách giải

#### Phương pháp thường

=== "Hàm logarit đa thức"
    Trước hết, để $\ln{f(x)}$ tồn tại, theo [định nghĩa](./intro.md#复合), cần:
    
    $$
    [x^{0}]f(x)=1
    $$
    
    Lấy đạo hàm rồi tích phân:
    
    $$
    \begin{aligned}
        \frac{\mathrm{d} \ln{f(x)}}{\mathrm{d} x} & \equiv \frac{f'(x)}{f(x)} & \pmod{x^{n}} \\
        \ln{f(x)} & \equiv \int \mathrm{d} \ln{f(x)} \equiv \int\frac{f'(x)}{f(x)} \mathrm{d} x & \pmod{x^{n}}
    \end{aligned}
    $$
    
    Đạo hàm và tích phân đa thức mất $O(n)$, nghịch đảo mất $O(n\log{n})$, nên $\ln$ đa thức là $O(n\log{n})$.

=== "Hàm mũ đa thức"
    Trước hết, để $\exp{f(x)}$ tồn tại, cần:
    
    $$
    [x^{0}]f(x)=0
    $$
    
    nếu không hạng tự do của $\exp{f(x)}$ không hội tụ.
    
    Lấy đạo hàm:
    
    $$
    \frac{\mathrm{d} \exp{f(x)}}{\mathrm{d} x} \equiv \exp{f(x)}f'(x)\pmod{x^{n}}
    $$
    
    So sánh hệ số:
    
    $$
    [x^{n-1}]\frac{\mathrm{d} \exp{f(x)}}{\mathrm{d} x} = \sum_{i = 0}^{n - 1} \left([x^{i}]\exp{f(x)}\right) \left([x^{n-i-1}]f'(x)\right)
    $$
    
    $$
    n[x^{n}]\exp{f(x)} = \sum_{i = 0}^{n - 1} \left([x^{i}]\exp{f(x)}\right) \left((n - i)[x^{n - i}]f(x)\right)
    $$
    
    Dùng FFT chia để trị là được.
    
    **Độ phức tạp** $O(n\log^{2}{n})$.

#### Newton's Method

Dùng [Newton's Method](./newton.md#newtons-method) để tính $\exp$ đa thức trong $O(n\log{n})$.

### Mã

??? note "Đa thức ln/exp"
    ```cpp
    constexpr int MAXN = 262144;
    constexpr int mod = 998244353;
    
    using i64 = long long;
    using poly_t = int[MAXN];
    using poly = int *const;
    
    void derivative(const poly &h, const int n, poly &f) {
      for (int i = 1; i != n; ++i) f[i - 1] = (i64)h[i] * i % mod;
      f[n - 1] = 0;
    }
    
    void integrate(const poly &h, const int n, poly &f) {
      for (int i = n - 1; i; --i) f[i] = (i64)h[i - 1] * inv[i] % mod;
      f[0] = 0; /* C */
    }
    
    void polyln(const poly &h, const int n, poly &f) {
      /* f = ln h = ∫ h' / h dx */
      assert(h[0] == 1);
      static poly_t ln_t;
      const int t = n << 1;
    
      derivative(h, n, ln_t);
      std::fill(ln_t + n, ln_t + t, 0);
      polyinv(h, n, f);
    
      DFT(ln_t, t);
      DFT(f, t);
      for (int i = 0; i != t; ++i) ln_t[i] = (i64)ln_t[i] * f[i] % mod;
      IDFT(ln_t, t);
    
      integrate(ln_t, n, f);
    }
    
    void polyexp(const poly &h, const int n, poly &f) {
      /* f = exp(h) = f_0 (1 - ln f_0 + h) */
      assert(h[0] == 0);
      static poly_t exp_t;
      std::fill(f, f + n + n, 0);
      f[0] = 1;
      for (int t = 2; t <= n; t <<= 1) {
        const int t2 = t << 1;
    
        polyln(f, t, exp_t);
        exp_t[0] = sub(pls(h[0], 1), exp_t[0]);
        for (int i = 1; i != t; ++i) exp_t[i] = sub(h[i], exp_t[i]);
        std::fill(exp_t + t, exp_t + t2, 0);
    
        DFT(f, t2);
        DFT(exp_t, t2);
        for (int i = 0; i != t2; ++i) f[i] = (i64)f[i] * exp_t[i] % mod;
        IDFT(f, t2);
    
        std::fill(f + t, f + t2, 0);
      }
    }
    ```

### Bài tập mẫu

1.  Tính $f^{k}(x)$

    Cách thường là lũy thừa nhanh đa thức, độ phức tạp $O(n\log{n}\log{k})$.

    Khi $[x^{0}]f(x)=1$:

    $$
    f^{k}(x)=\exp{\left(k\ln{f(x)}\right)}
    $$

    Khi $[x^{0}]f(x)\neq 1$, giả sử hạng bậc thấp nhất là $f_{i}x^{i}$:

    $$
    f^{k}(x)=f_{i}^{k}x^{ik}\exp{\left(k\ln{\frac{f(x)}{f_{i}x^{i}}}\right)}
    $$

    **Độ phức tạp** $O(n\log{n})$.

## Hàm lượng giác đa thức

Cho đa thức $f\left(x\right)$, tính $\sin{f\left(x\right)}, \cos{f\left(x\right)}$ và $\tan{f\left(x\right)}$ theo mô-đun $x^{n}$.

### Cách giải

Theo [công thức Euler](../complex.md#欧拉公式) $\left(\mathrm{e}^{\mathrm{i}x} = \cos{x} + \mathrm{i}\sin{x}\right)$, ta có [biểu diễn khác của hàm lượng giác](https://en.wikipedia.org/wiki/Trigonometric_functions#Relationship_to_exponential_function_and_complex_numbers):

$$
\begin{aligned}
    \sin{x} &= \frac{\mathrm{e}^{\mathrm{i}x} - \mathrm{e}^{-\mathrm{i}x}}{2\mathrm{i}} \\
    \cos{x} &= \frac{\mathrm{e}^{\mathrm{i}x} + \mathrm{e}^{-\mathrm{i}x}}{2}
\end{aligned}
$$

Thay $f\left(x\right)$ vào:

$$
\begin{aligned}
    \sin{f\left(x\right)} &= \frac{\exp{\left(\mathrm{i}f\left(x\right)\right)} - \exp{\left(-\mathrm{i}f\left(x\right)\right)}}{2\mathrm{i}} \\
    \cos{f\left(x\right)} &= \frac{\exp{\left(\mathrm{i}f\left(x\right)\right)} + \exp{\left(-\mathrm{i}f\left(x\right)\right)}}{2}
\end{aligned}
$$

Cài đặt theo các biểu thức trên là được $\sin{f\left(x\right)}$ và $\cos{f\left(x\right)}$ theo mô-đun $x^{n}$. Sau đó dùng $\tan{f\left(x\right)} = \frac{\sin{f\left(x\right)}}{\cos{f\left(x\right)}}$ để có $\tan{f\left(x\right)}$.

### Mã

??? note "Hàm lượng giác đa thức"
    Lưu ý ta làm trên $\mathbb{Z}_{998244353}$ với NTT, nên đơn vị ảo $\mathrm{i}$ cần thay bằng $86583718$ hoặc $911660635$:
    
    $$
    \begin{aligned}
               & \mathrm{i} = \sqrt{-1} \equiv \sqrt{998244352} \pmod{998244353}       \\
      \implies & \phantom{\text{or}} \quad \mathrm{i} \equiv 86583718 \pmod{998244353} \\
               & \text{hoặc} \quad \mathrm{i} \equiv 911660635 \pmod{998244353}
    \end{aligned}
    $$
    
    ```cpp
    constexpr int MAXN = 262144;
    constexpr int mod = 998244353;
    constexpr int imgunit = 86583718; /* sqrt(-1) = sqrt(998233452) */
    
    using i64 = long long;
    using poly_t = int[MAXN];
    using poly = int *const;
    
    void polytri(const poly &h, const int n, poly &sin_t, poly &cos_t) {
      /* sin(f) = (exp(i * f) - exp(- i * f)) / 2i */
      /* cos(f) = (exp(i * f) + exp(- i * f)) / 2 */
      /* tan(f) = sin(f) / cos(f) */
      assert(h[0] == 0);
      static poly_t tri1_t, tri2_t;
    
      for (int i = 0; i != n; ++i) tri2_t[i] = (i64)h[i] * imgunit % mod;
      polyexp(tri2_t, n, tri1_t);
      polyinv(tri1_t, n, tri2_t);
    
      if (sin_t != nullptr) {
        const int invi = fpow(pls(imgunit, imgunit), mod - 2);
        for (int i = 0; i != n; ++i)
          sin_t[i] = (i64)(tri1_t[i] - tri2_t[i] + mod) * invi % mod;
      }
      if (cos_t != nullptr) {
        for (int i = 0; i != n; ++i) cos_t[i] = div2(pls(tri1_t[i], tri2_t[i]));
      }
    }
    ```

## Hàm lượng giác ngược đa thức

Cho đa thức $f\left(x\right)$, tính $\arcsin{f\left(x\right)}, \arccos{f\left(x\right)}$ và $\arctan{f\left(x\right)}$ theo mô-đun $x^{n}$.

### Cách giải

Tương tự cách tính $\ln$ đa thức, lấy đạo hàm rồi tích phân:

$$
\begin{aligned}
    \frac{\mathrm{d}}{\mathrm{d} x} \arcsin{x} &= \frac{1}{\sqrt{1 - x^{2}}} \\
    \arcsin{x} &= \int \frac{1}{\sqrt{1 - x^{2}}} \mathrm{d} x \\
    \frac{\mathrm{d}}{\mathrm{d} x} \arccos{x} &= - \frac{1}{\sqrt{1 - x^{2}}} \\
    \arccos{x} &= - \int \frac{1}{\sqrt{1 - x^{2}}} \mathrm{d} x \\
    \frac{\mathrm{d}}{\mathrm{d} x} \arctan{x} &= \frac{1}{1 + x^{2}} \\
    \arctan{x} &= \int \frac{1}{1 + x^{2}} \mathrm{d} x
\end{aligned}
$$

Thay $f\left(x\right)$:

$$
\begin{aligned}
    \frac{\mathrm{d}}{\mathrm{d} x} \arcsin{f\left(x\right)} &= \frac{f'\left(x\right)}{\sqrt{1 - f^{2}\left(x\right)}} \\
    \arcsin{f\left(x\right)} &= \int \frac{f'\left(x\right)}{\sqrt{1 - f^{2}\left(x\right)}} \mathrm{d} x \\
    \frac{\mathrm{d}}{\mathrm{d} x} \arccos{f\left(x\right)} &= - \frac{f'\left(x\right)}{\sqrt{1 - f^{2}\left(x\right)}} \\
    \arccos{f\left(x\right)} &= - \int \frac{f'\left(x\right)}{\sqrt{1 - f^{2}\left(x\right)}} \mathrm{d} x \\
    \frac{\mathrm{d}}{\mathrm{d} x} \arctan{f\left(x\right)} &= \frac{f'\left(x\right)}{1 + f^{2}\left(x\right)} \\
    \arctan{f\left(x\right)} &= \int \frac{f'\left(x\right)}{1 + f^{2}\left(x\right)} \mathrm{d} x
\end{aligned}
$$

Tính trực tiếp theo công thức là được.

### Mã

??? note "Hàm lượng giác ngược đa thức"
    ```cpp
    constexpr int MAXN = 262144;
    constexpr int mod = 998244353;
    
    using i64 = long long;
    using poly_t = int[MAXN];
    using poly = int *const;
    
    void derivative(const poly &h, const int n, poly &f) {
      for (int i = 1; i != n; ++i) f[i - 1] = (i64)h[i] * i % mod;
      f[n - 1] = 0;
    }
    
    void integrate(const poly &h, const int n, poly &f) {
      for (int i = n - 1; i; --i) f[i] = (i64)h[i - 1] * inv[i] % mod;
      f[0] = 0; /* C */
    }
    
    void polyarcsin(const poly &h, const int n, poly &f) {
      /* arcsin(f) = ∫ f' / sqrt(1 - f^2) dx  */
      static poly_t arcsin_t;
      const int t = n << 1;
      std::copy(h, h + n, arcsin_t);
      std::fill(arcsin_t + n, arcsin_t + t, 0);
    
      DFT(arcsin_t, t);
      for (int i = 0; i != t; ++i) arcsin_t[i] = sqr(arcsin_t[i]);
      IDFT(arcsin_t, t);
    
      arcsin_t[0] = sub(1, arcsin_t[0]);
      for (int i = 1; i != n; ++i)
        arcsin_t[i] = arcsin_t[i] ? mod - arcsin_t[i] : 0;
    
      polysqrt(arcsin_t, n, f);
      polyinv(f, n, arcsin_t);
      derivative(h, n, f);
    
      DFT(f, t);
      DFT(arcsin_t, t);
      for (int i = 0; i != t; ++i) arcsin_t[i] = (i64)f[i] * arcsin_t[i] % mod;
      IDFT(arcsin_t, t);
    
      integrate(arcsin_t, n, f);
    }
    
    void polyarccos(const poly &h, const int n, poly &f) {
      /* arccos(f) = - ∫ f' / sqrt(1 - f^2) dx  */
      polyarcsin(h, n, f);
      for (int i = 0; i != n; ++i) f[i] = f[i] ? mod - f[i] : 0;
    }
    
    void polyarctan(const poly &h, const int n, poly &f) {
      /* arctan(f) = ∫ f' / (1 + f^2) dx  */
      static poly_t arctan_t;
      const int t = n << 1;
      std::copy(h, h + n, arctan_t);
      std::fill(arctan_t + n, arctan_t + t, 0);
    
      DFT(arctan_t, t);
      for (int i = 0; i != t; ++i) arctan_t[i] = sqr(arctan_t[i]);
      IDFT(arctan_t, t);
    
      inc(arctan_t[0], 1);
      std::fill(arctan_t + n, arctan_t + t, 0);
    
      polyinv(arctan_t, n, f);
      derivative(h, n, arctan_t);
    
      DFT(f, t);
      DFT(arctan_t, t);
      for (int i = 0; i != t; ++i) arctan_t[i] = (i64)f[i] * arctan_t[i] % mod;
      IDFT(arctan_t, t);
    
      integrate(arctan_t, n, f);
    }
    ```

## Tài liệu tham khảo và liên kết

[^ref1]: [Elementary function——Wikipedia](https://en.wikipedia.org/wiki/Elementary_function)
