## Chuỗi Laurent hình thức

Ta đã biết vành chuỗi lũy thừa hình thức $\mathbb{C}\lbrack\lbrack x\rbrack\rbrack$, định nghĩa vành chuỗi Laurent hình thức:

$$
\mathbb{C}\left(\left(x\right)\right):=\left\lbrace \sum_{k\geq N}a_kx^k : N\in\mathbb{Z},a_k\in \mathbb{C}\right\rbrace
$$

Ta định nghĩa nghịch đảo nhân của phần tử trong $\mathbb{C}\left(\left(x\right)\right)$ tương tự chuỗi lũy thừa:

Với $f:=\sum_{k\geq N}f_kx^k$ và $f_N\neq 0$, tồn tại $g=\sum_{k\geq -N}g_kx^k$ sao cho $fg=1$, thì

$$
g_k:=
\begin{cases}
f_N^{-1}, &\text{ nếu }k=-N, \\
-f_N^{-1}\sum_{i> N}f_ig_{k-i}, &\text{ các trường hợp khác}
\end{cases}
$$

Tương tự chuỗi lũy thừa, với $f(x)=\sum_{k\geq N}f_kx^k\neq 0$, định nghĩa:

$$
\operatorname{ord} f:=\min\lbrace k:f_k\neq 0\rbrace
$$

Hiển nhiên với $g\neq 0$:

$$
\operatorname{ord} (fg)=\operatorname{ord}(f)+\operatorname{ord}(g)
$$

## Phần dư hình thức

Phần dư hình thức là hệ số của $x^{-1}$ trong chuỗi Laurent. Ký hiệu $\operatorname{res} f:=\lbrack x^{-1}\rbrack f$.

**Bổ đề**: Với mọi chuỗi Laurent hình thức $f$, ta có $\operatorname{res} f'=0$.

**Chứng minh**: Theo định nghĩa đạo hàm hình thức $\left(x^k\right)'=kx^{k-1}$.

**Bổ đề**: Với mọi chuỗi Laurent hình thức $f,g$, ta có $\operatorname{res}(f'g)=-\operatorname{res}(fg')$.

**Chứng minh**: Theo quy tắc nhân $(fg)'=f'g+fg'$, nên $0=\operatorname{res}((fg)')=\operatorname{res}(f'g)+\operatorname{res}(fg')$.

**Bổ đề**: Với chuỗi Laurent hình thức $f(x)\neq 0$, $\operatorname{res}(f'/f)=\operatorname{ord}f$.

**Chứng minh**: Đặt $\operatorname{ord}f=k$:

$$
\begin{aligned}
\operatorname{res}\left(\frac{f'}{f}\right)&=\operatorname{res}\left(\frac{kf_kx^{k-1}+\cdots}{f_kx^k+f_{k+1}x^{k+1}+\cdots}\right) \\
&=\operatorname{res}\left(\frac{kf_kx^{-1}+\cdots}{f_k+f_{k+1}x+\cdots}\right) \\
&=k
\end{aligned}
$$

**Bổ đề**: Với chuỗi Laurent hình thức $f$ và chuỗi lũy thừa hình thức $g\neq 0$, ta có $\operatorname{res}(f)\operatorname{ord}(g)=\operatorname{res}(f(g)g')$.

**Chứng minh**: Nhờ tính tuyến tính, chỉ cần chứng minh $f=x^k$ với $k\in\mathbb{Z}$. Nếu $k\neq -1$:

$$
\begin{aligned}
\operatorname{res}x^k&=0 \\
\operatorname{res}(g^kg')&=\operatorname{res}\left(\frac{1}{k+1}\left(g^{k+1}\right)'\right) \\
&=\frac{1}{k+1}\operatorname{res}\left(\left(g^{k+1}\right)'\right) \\
&=0
\end{aligned}
$$

Nếu $k=-1$:

$$
\begin{aligned}
\operatorname{res}f&=\operatorname{res}\left(x^{-1}\right)=1 \\
\operatorname{res}(f(g)g')&=\operatorname{res}(g'/g) \\
&=\operatorname{ord}(g) \\
&=\operatorname{res}(f)\operatorname{ord}(g)
\end{aligned}
$$

## Nghịch đảo hợp

Ký hiệu $A(x)\circ B(x):=A(B(x))$.

**Mệnh đề**: $f(x):=\sum_{k\geq 1}f_kx^k$ có nghịch đảo hợp $f^{\langle -1\rangle}(x)$ khi và chỉ khi $f(0)=0\neq f'(0)$. Khi đó $f^{\langle -1\rangle}(x)$ là duy nhất. Hơn nữa, nếu $g(x)=\sum_{k\geq 1}g_kx^k$ thỏa $f(g(x))=x$ hoặc $g(f(x))=x$ thì $g(x)=f^{\langle -1\rangle}(x)$.

**Chứng minh**: Xét

$$
\begin{aligned}
g(f(x))&=g_1(f_1x+f_2x^2+f_3x^3+\cdots ) \\
&+g_2(f_1x+f_2x^2+\cdots )^2 \\
&+g_3(f_1x+\cdots )^3 \\
&+\cdots \\
&=g_1f_1x+(g_1f_2+g_2f_1^2)x^2+(g_1f_3+2g_2f_1f_2+g_3f_1^3)x^3+\cdots
\end{aligned}
$$

Vì $g(f(x))=x$ nên hệ:

$$
\begin{cases}
g_1f_1&=1 \\
g_1f_2+g_2f_1^2&=0 \\
g_1f_3+2g_2f_1f_2+g_3f_1^3&=0 \\
\vdots
\end{cases}
$$

Chỉ khi $f_1\neq 0$ mới giải được phương trình đầu, rồi suy ra $g_2,\dots$.

Đặc biệt, nếu $f(h(x))=x$ thì $g(f(h(x)))=g(x)$, nên $g(x)=g\circ f\circ h(x)=x\circ h(x)=h(x)$.

## Công thức phản diễn Lagrange

Cho $f(x),g(x)\in\mathbb{C}\lbrack\lbrack x\rbrack\rbrack$ sao cho $f(g(x))=g(f(x))=x$. Lấy $\Phi(x)\in\mathbb{C}\lbrack\lbrack x\rbrack\rbrack$ (hoặc $\Phi(x)\in\mathbb{C}\left(\left(x\right)\right)$), khi đó

$$
\begin{aligned}
\lbrack x^n\rbrack\Phi(f(x))&=\lbrack x^{n-1}\rbrack\Phi(x)\frac{g'(x)}{g(x)}\left(\frac{x}{g(x)}\right)^n \\
&=\lbrack x^{-1}\rbrack\frac{\Phi(x)g'(x)}{g(x)^{n+1}}
\end{aligned}
$$

**Chứng minh**:

$$
\begin{aligned}
\lbrack x^n\rbrack\Phi(f(x))&=\operatorname{res}\left(\frac{\Phi(f(x))}{x^{n+1}}\right) \\
&=\operatorname{res}\left(\frac{\Phi(f(g(x)))g'(x)}{g(x)^{n+1}}\right)\cdot \left(\operatorname{ord}(g(x))\right)^{-1} \\
&=\operatorname{res}\left(\frac{\Phi(x)g'(x)}{g(x)^{n+1}}\right)
\end{aligned}
$$

Một số độc giả quen với dạng: với $k\in\mathbb{Z}_{\geq 0},n\in\mathbb{Z}_{>0}$:

$$
\lbrack x^n\rbrack f(x)^k=\frac{k}{n}\lbrack x^{n-k}\rbrack\left(\frac{x}{g(x)}\right)^n
$$

hoặc

$$
\begin{aligned}
\lbrack x^n\rbrack \Phi(f(x))&=\frac{1}{n}\lbrack x^{n-1}\rbrack \Phi'(x)\left(\frac{x}{g(x)}\right)^n \\
&=\frac{1}{n}\lbrack x^{-1}\rbrack\frac{\Phi'(x)}{g(x)^n}
\end{aligned}
$$

Ta thấy

$$
\begin{aligned}
\operatorname{res}\left(\frac{\Phi'(x)}{g(x)^n}-n\frac{\Phi(x)g'(x)}{g(x)^{n+1}}\right)&=\operatorname{res}\left(\left(\frac{\Phi(x)}{g(x)^n}\right)'\right) \\
&=0
\end{aligned}
$$

có thể suy ra từ các phần trước.

## Tài liệu tham khảo

1.  Richard P. Stanley and Sergey P. Fomin. Enumerative Combinatorics Volume 2 (Edition 1).
2.  Ira M. Gessel. Lagrange Inversion.
