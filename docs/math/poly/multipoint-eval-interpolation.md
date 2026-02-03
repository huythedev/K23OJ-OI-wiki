## Đa điểm giá trị của đa thức

### Mô tả

Cho đa thức $f\left(x\right)$ và $n$ điểm $x_{1},x_{2},\dots,x_{n}$, hãy tính

$$
f\left(x_{1}\right),f\left(x_{2}\right),\dots,f\left(x_{n}\right)
$$

### Lời giải

Dùng chia để trị để giảm kích thước bài toán.

Chia các điểm thành hai phần:

$$
\begin{aligned}
    X_{0}&=\left\{x_{1},x_{2},\dots,x_{\left\lfloor\frac{n}{2}\right\rfloor}\right\}\\
    X_{1}&=\left\{x_{\left\lfloor\frac{n}{2}\right\rfloor+1},x_{\left\lfloor\frac{n}{2}\right\rfloor+2},\dots,x_{n}\right\}
\end{aligned}
$$

Xây đa thức

$$
g_{0}\left(x\right)=\prod_{x_{i}\in X_{0}}\left(x-x_{i}\right)
$$

thì $\forall x\in X_{0}:g_{0}\left(x\right)=0$.

Biểu diễn $f\left(x\right)$ dưới dạng $g_{0}\left(x\right)Q\left(x\right)+f_{0}\left(x\right)$, tức:

$$
f_{0}\left(x\right)\equiv f\left(x\right)\pmod{g_{0}\left(x\right)}
$$

Suy ra với $\forall x\in X_{0}$: $f\left(x\right)=g_{0}\left(x\right)Q\left(x\right)+f_{0}\left(x\right)=f_{0}\left(x\right)$; tương tự cho $X_{1}$.

Như vậy kích thước giảm một nửa, có thể dùng chia để trị + lấy modulo đa thức.

Độ phức tạp

$$
T\left(n\right)=2T\left(\frac{n}{2}\right)+O\left(n\log{n}\right)=O\left(n\log^{2}{n}\right)
$$

## Nội suy nhanh đa thức

### Mô tả

Cho tập $n+1$ điểm

$$
X=\left\{\left(x_{0},y_{0}\right),\left(x_{1},y_{1}\right),\dots,\left(x_{n},y_{n}\right)\right\}
$$

Hãy tìm đa thức bậc $n$ là $f\left(x\right)$ sao cho $\forall\left(x,y\right)\in X:f\left(x\right)=y$.

### Lời giải

Xét công thức nội suy Lagrange

$$
f(x) = \sum_{i=1}^{n} \prod_{j\neq i }\frac{x-x_j}{x_i-x_j} y_i
$$

Đặt $M(x) = \prod_{i=1}^n (x - x_i)$, theo định lý L'Hôpital:

$$
\prod_{j\neq i} (x_i - x_j) = \lim_{x\rightarrow x_i} \frac{\prod_{j=1}^n (x - x_j)}{x - x_i} = M'(x_i)
$$

Do đó

$$
f(x) = \sum_{i = 1}^n \frac{y_i}{M'(x_i)}\prod_{j \neq i}(x - x_j)
$$

Ta dùng chia để trị để tính hệ số của $M(x)$, rồi dùng đa điểm giá trị trong $O(n\log^2 n)$ để tính các $M'(x_i)$.

Đặt $v_i = \frac{y_i}{M'(x_i)}$, tiếp theo tính $f(x)$. Với $n=1$, có $f(x) = v_1, M(x) = x - x_1$. Nếu không, đặt

$$
\begin{aligned}
f_0(x) & = \sum_{i = 1}^{\left\lfloor \frac n2 \right \rfloor} v_i\prod_{j \neq i \wedge j \le \left\lfloor \frac n2 \right \rfloor}(x - x_j)\\
M_0(x) & = \prod_{i = 1}^{\left\lfloor \frac n2 \right \rfloor} (x - x_i)\\
f_1(x) & = \sum_{i = \left\lfloor \frac n2 \right \rfloor+1}^n v_i\prod_{j \neq i \wedge \left\lfloor \frac n2 \right \rfloor < j \le n}(x - x_j) \\
M_1(x) & = \prod_{i = \left\lfloor \frac n2 \right \rfloor+1}^n (x - x_i)
\end{aligned}
$$

Suy ra $f(x) = f_0(x)M_1(x) + f_1(x)M_0(x), M(x) = M_0(x)M_1(x)$, vì vậy có thể chia để trị, độ phức tạp là $O(n\log^2 n)$
