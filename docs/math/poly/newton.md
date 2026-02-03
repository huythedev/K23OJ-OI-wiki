## Mô tả

Cho đa thức $G\left(x, y\right)$, biết đa thức $f\left(x\right)$ thỏa:

$$
G\left(x, f\left(x\right)\right)\equiv 0\pmod{x^{n}}
$$

và tồn tại giá trị $f_1$ sao cho $G\left(x, y\right)$ thỏa:

-   $G(0, f_1) = 0$;
-   $\dfrac{\partial G}{\partial y}(0, f_1) \neq 0$.

Hãy tìm $f\left(x\right)$ theo modulo $x^{n}$.

## Phương pháp Newton

Xét nhân đôi.

Khi $n=1$, nghiệm của $\left[x^{0}\right]G\left(x, f\left(x\right)\right)=0$ cần tìm riêng; $f_1$ trong giả thiết chính là một nghiệm.

Giả sử đã có nghiệm modulo $x^{\left\lceil\frac{n}{2}\right\rceil}$ là $f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)$, cần tìm nghiệm modulo $x^{n}$, tức $f\left(x\right)=f_n\left(x\right)$.

Khai triển Taylor $G\left(x, f(x)\right)$ theo $f(x)$ tại $f(x)=f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)$:

$$
\sum_{i=0}^{+\infty}\frac{\frac{\partial^i G}{\partial y^i}\left(x, f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)}{i!}\left(f\left(x\right)-f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)^{i}\equiv 0\pmod{x^{n}}
$$

Vì bậc thấp nhất khác 0 của $f\left(x\right)-f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)$ là $\left\lceil\frac{n}{2}\right\rceil$, nên:

$$
\forall 2\leqslant i:\left(f\left(x\right)-f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)^{i}\equiv 0\pmod{x^{n}}
$$

Suy ra:

$$
\begin{aligned}
\sum_{i=0}^{+\infty}\frac{\frac{\partial^i G}{\partial y^i}\left(x, f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)}{i!}\left(f\left(x\right)-f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)^{i}&\equiv G\left(x, f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)+\frac{\partial G}{\partial y}\left(x, f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)\left[f\left(x\right)-f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right]\\
&\equiv 0\pmod{x^{n}}
\end{aligned}
$$

$$
f_n\left(x\right)\equiv f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)-\frac{G\left(x, f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)}{\frac{\partial G}{\partial y}\left(x, f_{\left\lceil\frac{n}{2}\right\rceil}\left(x\right)\right)}\pmod{x^{n}}
$$

Hoặc

$$
f_{2n}\left(x\right)\equiv f_n\left(x\right)-\frac{G\left(x, f_n\left(x\right)\right)}{\frac{\partial G}{\partial y}\left(x, f_n\left(x\right)\right)}\pmod{x^{2n}}
$$

## Ví dụ

### [Nghịch đảo đa thức](./elementary-func.md#多项式求逆)

Đặt hàm đã cho là $h\left(x\right)$, khi đó:

$$
G\left(x, y\right)=\frac{1}{y}-h\left(x\right)
$$

Áp dụng Newton:

$$
\begin{aligned}
    f_{2n}\left(x\right)&\equiv f_{n}\left(x\right)-\frac{1/f_{n}\left(x\right)-h\left(x\right)}{-1/f_{n}^{2}\left(x\right)}&\pmod{x^{2n}}\\
    &\equiv 2f_{n}\left(x\right)-f_{n}^{2}\left(x\right)h\left(x\right)&\pmod{x^{2n}}
\end{aligned}
$$

Độ phức tạp:

$$
T\left(n\right)=T\left(\frac{n}{2}\right)+O\left(n\log{n}\right)=O\left(n\log{n}\right)
$$

### [Căn bậc hai đa thức](./elementary-func.md#多项式开方)

Đặt hàm đã cho là $h\left(x\right)$, khi đó:

$$
G\left(x, y\right)=y^{2}-h\left(x\right)\equiv 0
$$

Áp dụng Newton:

$$
\begin{aligned}
    f_{2n}\left(x\right)&\equiv f_{n}\left(x\right)-\frac{f_{n}^{2}\left(x\right)-h\left(x\right)}{2f_{n}\left(x\right)}&\pmod{x^{2n}}\\
    &\equiv\frac{f_{n}^{2}\left(x\right)+h\left(x\right)}{2f_{n}\left(x\right)}&\pmod{x^{2n}}
\end{aligned}
$$

Độ phức tạp:

$$
T\left(n\right)=T\left(\frac{n}{2}\right)+O\left(n\log{n}\right)=O\left(n\log{n}\right)
$$

### [Hàm mũ đa thức](./elementary-func.md#多项式对数函数--指数函数)

Đặt hàm đã cho là $h\left(x\right)$, khi đó:

$$
G\left(x, y\right)=\ln{y}-h\left(x\right)
$$

Áp dụng Newton:

$$
\begin{aligned}
    f_{2n}\left(x\right)&\equiv f_{n}\left(x\right)-\frac{\ln{f_{n}\left(x\right)}-h\left(x\right)}{1/f_{n}\left(x\right)}&\pmod{x^{2n}}\\
    &\equiv f_{n}\left(x\right)\left(1-\ln{f_{n}\left(x\right)+h\left(x\right)}\right)&\pmod{x^{2n}}
\end{aligned}
$$

Độ phức tạp:

$$
T\left(n\right)=T\left(\frac{n}{2}\right)+O\left(n\log{n}\right)=O\left(n\log{n}\right)
$$

## Minh họa tính tay

Để dễ hiểu, dưới đây là vài ví dụ minh họa quy trình.

### Căn bậc hai của đa thức phức theo modulo đa thức

Giả sử $h$ là đa thức phức không chia hết cho $x$ (có hằng số), tìm căn bậc hai của nó modulo $x^n$.

Ta có:

$$
G\left(f(x)\right) = f^2(x)-h(x) \equiv 0\pmod{x^{n}}
$$

Khai triển Taylor $G$ (theo $f$):

$$
G(f(x)) = \sum_{i=0}^{+\infty}\frac{G^{\left(i\right)}\left(f_{0}(x)\right)}{i!}\left(f(x)-f_{0}(x)\right)^{i}
= G(f_0(x)) + 2f_0(x)(f(x)-f_0(x)) + (f(x)-f_0(x))^2
$$

Dùng nhân đôi. Giả sử các kết quả trung gian là $f_0(x), f_1(x), \ldots, f_j(x)$; hoặc nghiêm ngặt hơn, $f_j(x)$ thỏa $G(f_j(x))\equiv 0\pmod{x^{2^j}}$, và để đảm bảo duy nhất, còn thỏa:

-   Bậc của $f_{j}(x)$ không vượt $x^{2^j}$;
-   $f_{j+k}(x)-f_j(x)\equiv 0\pmod{x^{2^j}}$ với mọi $k$.

Thay $f_{j+1}(x)$ và $f_j(x)$ vào:

$$
G(f_{j+1}(x)) = G(f_j(x)) + 2f_j(f_{j+1}(x)-f_j(x)) + (f_{j+1}(x)-f_{j}(x))^2  \equiv 0 \pmod{x^{2^{j+1}}}
$$

Do $f_{j+1}(x)-f_j(x)$ là bội của $x^{2^j}$, suy ra

$$
f_{j+1}(x) \equiv f_j(x) - \frac{f_j^2(x)-h(x)}{2f_j(x)} \equiv \frac{f_j(x)^2 + h(x)}{2f_j(x)} \pmod{x^{2^{j+1}}}
$$

Nếu $f_j(x)$ tồn tại, thì $2f_j(x)$ không chia hết cho $x$ (có hằng số), nên có nghịch đảo modulo $x^{2^{j+1}}$. Do đó dãy $f_0,f_1,\ldots,f_j$ tồn tại khi và chỉ khi $f_0$ tồn tại. Đa thức $h(x)$ không chia hết cho $x$ luôn có căn bậc hai modulo $x$ vì chỉ là căn của số phức khác 0, nên luôn có hai nghiệm. Vì vậy thuật toán dùng được cho mọi $h(x)$ có hằng số.

Chọn $h(x)=x+1$:

-   $f_0(x)=1$,$f_1(x)=\dfrac{1^2+x+1}{2\times 1}\mod x^2 = \dfrac{1}{2}x+1$,$f_2(x)=\dfrac{\left(\dfrac{1}{2}x+1\right)^2+x+1}{2\times \left(\dfrac{1}{2}x+1\right)}\mod x^4 = \dfrac{1}{16}x^3-\dfrac{1}{8}x^2+\dfrac{1}{2}x+1$,$\ldots$
-   $f_0(x)=-1$,$f_1(x)=\dfrac{(-1)^2+x+1}{2\times (-1)}\mod x^2 = -\dfrac{1}{2}x-1$,$\ldots$ (bằng âm của dãy trên)

Có thể kiểm tra đều đúng.

### Căn bậc hai của số nguyên theo modulo lũy thừa số nguyên tố

Newton cũng áp dụng cho số nguyên modulo lũy thừa nguyên tố. Giả sử $h$ là số nguyên không chia hết cho 3 và “thuận tiện” (tức chắc chắn có nghiệm). Tìm căn bậc hai của $h$ modulo $3^n$.

Ta có:

$$
G\left(f\right) = f^2-h \equiv 0\pmod{3^{n}}
$$

Khai triển Taylor:

$$
G(f) = \sum_{i=0}^{+\infty}\frac{G^{\left(i\right)}\left(f_{0}\right)}{i!}\left(f-f_{0}\right)^{i}
= G(f_0) + 2f_0(f-f_0) + (f-f_0)^2
$$

Dùng nhân đôi. Giả sử các kết quả trung gian là $f_0, f_1, \ldots, f_j$, hoặc chính xác hơn, $f_j$ thỏa $G(f_j)\equiv 0\pmod{3^{2^j}}$, và thỏa:

-   $0 < f_{j} < 3^{2^j}$;
-   $f_{j+k}-f_j\equiv 0\pmod{3^{2^j}}$ với mọi $k$.

Thay vào:

$$
G(f_{j+1}) = G(f_j) + 2f_j(f_{j+1}-f_j) + (f_{j+1}-f_{j})^2  \equiv 0 \pmod{3^{2^{j+1}}}
$$

Suy ra:

$$
f_{j+1} \equiv f_j - \frac{f_j^2-h}{2f_j} \equiv \frac{f_j^2 + h}{2f_j} \pmod{3^{2^{j+1}}}
$$

Nếu $f_j$ tồn tại, $2f_j$ không chia hết cho 3 nên có nghịch đảo modulo $3^{2^{j+1}}$. Vì vậy dãy tồn tại khi và chỉ khi $f_0$ tồn tại. Với $h$ không chia hết cho 3, căn bậc hai modulo 3 có hoặc không, nếu có thì có hai nghiệm. Điều kiện tồn tại nghiệm modulo 3 là điều kiện duy nhất để thuật toán chạy.

Chọn $h=46$:

-   $f_0=1$,$f_1=\dfrac{1^2+46}{2\times 1}\mod 9 = 1$,$f_2=\dfrac{1^2+46}{2\times 1}\mod 81 = 64$,$f_3=\dfrac{64^2+46}{2\times 64}\mod 6561 = 955$,$\ldots$
-   $f_0=2$,$f_1=\dfrac{2^2+46}{2\times 2}\mod 9 = 8$,$f_2=\dfrac{8^2+46}{2\times 8}\mod 81 = 17$,$f_3=\dfrac{17^2+46}{2\times 17}\mod 6561 = 5606$,$\ldots$ (bằng âm của dãy trên)

Có thể kiểm tra đều đúng.

## Chứng minh đại số

Phần này mở rộng lập luận, dùng ngôn ngữ đại số trừu tượng để chứng minh: chỉ cần $f$ thỏa điều kiện nghiệm ban đầu, Newton cho nghiệm với mọi $n$, và cho ra toàn bộ nghiệm.

### Chứng minh tồn tại nghiệm

???+ note "Bổ đề 1"
    Cho [miền nguyên](../algebra/ring-theory.md#整环) $R$ có đa thức hoặc [chuỗi lũy thừa hình thức](../algebra/ring-theory.md#形式幂级数环) $f(X) = \sum_{i\geq 0}a_iX^i$ và $r,p\in R$ sao cho $f(r)\in Rp$ (tức $r$ là nghiệm của $f(X)$ modulo $p$) và $f'(r)\in R$ khả nghịch modulo $p$. Ở đây $f'(X) := \sum_{i\geq 0}(i+1)a_{i+1}X^i$ là **đạo hàm hình thức**. Khi đó $f\left(r-\dfrac{f(r)}{f'(r)}\right) \equiv 0\pmod {p^2}$.

??? note "Chứng minh"
    Với mọi $s\in R$,
    
    $$
    \begin{aligned}
    f(r+sp) &= \sum_{i\geq 0}a_i(r+sp)^i \\
    &= \sum_{i\geq 0}a_ir^i + sp\sum_{i\geq 1}ia_ir^{i-1} + s^2p^2\left(\ldots\right) \\
    &= f(r) + spf'(r) + s^2p^2\left(\frac{f''(r)}{2!} + \cdots\right),
    \end{aligned}
    $$
    
    nên
    
    $$
    f(r+sp) \in Rp^2 \iff f(r)+f'(r)sp \in Rp^2
    $$
    
    Vì $f(r)\in Rp$ và $f'(r)$ khả nghịch, lấy $sp = -\dfrac{f(r)}{f'(r)}$ là đủ; $\dfrac{1}{f'(r)}$ là nghịch đảo modulo $p^2$. Do $f'(r)$ khả nghịch modulo $p$ nên cũng có nghịch đảo modulo $p^2$: tồn tại $a,b,c\in R$ sao cho $af'(r) = bp+1$ và $f(r)=cp$, khi đó $\left(a^2f'(r)-2\right)f'(r) = b^2p^2+1$, do đó có thể lấy $s=c(2-a^2f'(r))$.

Với vành đa thức $k[X]$ trên trường $k$, nếu có $G(X, Y)\in k[X, Y]$ và $f_n\in k[X]$ sao cho $G(X, f_n(X))\in k[X]X^n$, áp dụng Bổ đề 1 ta được

$$
G\left(X, f_n(X) - \frac{G(X, f_n(X))}{\frac{\partial G}{\partial Y}(X, f_n(X))} \right)\equiv 0 \pmod {X^{2n}}
$$

Điều kiện khởi tạo chỉ cần có $f_1\in k$ sao cho $G(X, f_1)\equiv 0\pmod X$ và $\dfrac{\partial G}{\partial Y}(X, f_1)\not\equiv 0\pmod X$. Điều kiện sau đảm bảo $\dfrac{\partial G}{\partial Y}$ có hằng số khác 0; hơn nữa vì $X\left| \dfrac{G(X, f_n(X))}{\frac{\partial G}{\partial Y}(X, f_n(X))} \right.$, nên với mọi $n$, $\dfrac{\partial G}{\partial Y}(X, f_n)$ luôn khả nghịch modulo $X^n$, thỏa điều kiện lần lặp tiếp theo.

### Chứng minh thu được toàn bộ nghiệm

???+ note "Bổ đề 2"
    Nếu $R$ là [UFD](../algebra/ring-theory.md#唯一分解整环), $f,r,p$ như Bổ đề 1, thì giá trị $r-\dfrac{f(r)}{f'(r)}$ từ Bổ đề 1 là nghiệm duy nhất modulo $p^{2}$ thỏa:
    
    -   $f(x)\in Rp^{2}$
    -   $x-r\in Rp$
    
    tức
    
    $$
    \forall x\in R,\qquad p^2\mid f(x)\wedge p\mid (x-r) \implies x\equiv r-\dfrac{f(r)}{f'(r)} \pmod {p^2}
    $$

??? note "Chứng minh"
    Đặt $s = -\dfrac{f(r)}{f'(r)p}$ và $u = r+sp$, Bổ đề 1 đảm bảo $u$ thỏa hai điều kiện, và $f(r) + f'(r)sp \in Rp^{2}$.  
    Nếu $v$ thỏa các điều kiện thì $v = r+tp$ và $f(r) + f'(r)tp \in Rp^{2}$.  
    Suy ra $f'(r)(t-s)p\in Rp^{2}$ và $v-u\in Rp^{2}$.

Newton đảm bảo tìm được toàn bộ nghiệm modulo $X^{2^n}$. Nếu $G(X, h)\equiv 0\pmod {X^{2^n}}$, đặt $h_{2^i} := h\pmod {X^{2^i}}$, rồi lấy $f_1 = h_1$ và chạy Newton, theo Bổ đề 2 ta có $f_{2^i} \equiv h_{2^i}\pmod {X^{2^i}}$, nên $f_{2^n} = h$.

Lập luận trên cũng cho thấy khi $\dfrac{\partial G}{\partial y}(0, y)$ luôn khả nghịch, số nghiệm của $G(X, f)\equiv 0\pmod {X^n}$ bằng số nghiệm của $G(0, f)\equiv 0\pmod X$. Điều này không tầm thường, xem ví dụ sau.

??? example "Ví dụ số nghiệm tăng khi Newton không áp dụng"
    Modulo $X$, $X^2$ chỉ có căn là $0$, nhưng modulo $X^4$, $X^2$ có căn $X, -X, X^3+X, \ldots$.
