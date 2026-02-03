## Giới thiệu

Dãy truy hồi tuyến tính thuần nhất hệ số hằng (còn gọi là C-finite hoặc C-recursive) là một lớp dãy cơ bản.

Với dãy $\left(a_j\right)_{j\geq 0}$ và truy hồi

$$
a_n=\sum_{j=1}^{d}c_ja_{n-j},\qquad (n\geq d)
$$

trong đó $c_j$ không đồng thời bằng 0, mục tiêu là khi cho $a_0,\dots ,a_{d-1}$ và $c_1,\dots ,c_d$ thì tính $a_k$. Nếu $k\gg d$, ta cần thuật toán nhanh hơn.

Dãy $\left(a_j\right)_{j\geq 0}$ gọi là dãy truy hồi tuyến tính thuần nhất hệ số hằng bậc $d$.

### Thuật toán Fiduccia

Thuật toán Fiduccia dùng modulo đa thức và lũy thừa nhanh để tính $a_k$, thời gian $O(\mathsf{M}(d)\log k)$, với $O(\mathsf{M}(d))$ là thời gian nhân hai đa thức bậc $O(d)$.

**Thuật toán**: Xây đa thức $\Gamma(x):=x^d-\sum_{j=0}^{d-1}c_{d-j}x^j$ và $A(x):=\sum_{j=0}^{d-1}a_jx^j$, khi đó

$$
a_k=\left\langle x^k\bmod{\Gamma(x)},A(x)\right\rangle
$$

với $\left\langle \left(\sum_{j=0}^{n-1}f_jx^j\right),\left(\sum_{j=0}^{n-1}g_jx^j\right) \right\rangle :=\sum_{j=0}^{n-1}f_jg_j$ là tích vô hướng.

**Chứng minh**: Định nghĩa ma trận đồng hành của $\Gamma(x)$:

$$
C_\Gamma:=
\begin{bmatrix}
&&&c_d \\
1&&&c_{d-1} \\
&\ddots &&\vdots \\
&&1&c_1
\end{bmatrix}
$$

Định nghĩa $b(x):=\sum_{j=0}^{d-1}b_jx^j$ và

$$
B_b:=\begin{bmatrix}b_0&b_1&\cdots &b_{d-1}\end{bmatrix}^{\intercal}
$$

Quan sát:

$$
\underbrace{\begin{bmatrix}
&&&c_d \\
1&&&c_{d-1} \\
&\ddots &&\vdots \\
&&1&c_1
\end{bmatrix}}_{C_\Gamma}
\underbrace{\begin{bmatrix}
b_0 \\
b_1 \\
\vdots \\
b_{d-1}
\end{bmatrix}}_{B_b}=
\underbrace{\begin{bmatrix}
c_db_{d-1} \\
b_0+c_{d-1}b_{d-1} \\
\vdots \\
b_{d-2}+c_1b_{d-1}
\end{bmatrix}} _ {B_{xb\bmod{\Gamma}}}
$$

và

$$
\begin{aligned}
C_\Gamma&=\begin{bmatrix}B_{x\bmod{\Gamma}}&B_{x^2\bmod{\Gamma}}&\cdots &B_{x^d\bmod{\Gamma}}\end{bmatrix}, \\
\left(C_\Gamma\right)^2&=\begin{bmatrix}B_{x^2\bmod{\Gamma}}&B_{x^3\bmod{\Gamma}}&\cdots &B_{x^{d+1}\bmod{\Gamma}}\end{bmatrix}, \\
\cdots \\
\left(C_\Gamma\right)^k&=\begin{bmatrix}B_{x^k\bmod{\Gamma}}&B_{x^{k+1}\bmod{\Gamma}}&\cdots &B_{x^{k+d}\bmod{\Gamma}}\end{bmatrix}
\end{aligned}
$$

Biểu diễn truy hồi dưới dạng ma trận:

$$
\begin{bmatrix}
a_{k} \\
a_{k+1} \\
\vdots \\
a_{k+d-1}
\end{bmatrix}=\underbrace{\begin{bmatrix}
&1&& \\
&&\ddots & \\
&&&1 \\
c_d&c_{d-1}&\cdots &c_1
\end{bmatrix}^k} _ {\left(\left(C_\Gamma\right)^{\intercal}\right)^k=\left(\left(C_\Gamma\right)^{k}\right)^{\intercal}}
\begin{bmatrix}
a_0 \\
a_{1} \\
\vdots \\
a_{d-1}
\end{bmatrix}
$$

Suy ra hàng đầu của $\left(\left(C_\Gamma\right)^{k}\right)^{\intercal}$ là $B_{x^k\bmod{\Gamma}}$, theo định nghĩa nhân ma trận là đúng.

### Biểu diễn dưới dạng hàm hữu tỉ

Với dãy $\left(a_j\right)_{j\geq 0}$ trên, luôn tồn tại hàm hữu tỉ

$$
\frac{P(x)}{Q(x)}=\sum_{j\geq 0}a_jx^j
$$

với $Q(x)=x^d\Gamma\left(x^{-1}\right)$, $\deg{P}<d$. Gọi là “**hàm hữu tỉ**” vì $P(x),Q(x)$ là “**đa thức**”.

**Chứng minh**: Với $P(x)=\sum_{j=0}^{d-1}p_jx^j$ và $Q(x):=\sum_{j=0}^{d}q_jx^j$, xét định nghĩa hệ số của $\dfrac{P(x)}{Q(x)}=\sum_{j\geq 0}\tilde{q}_jx^j$ (tương ứng phép “chia” của chuỗi lũy thừa):

$$
\tilde{q}_N=
\begin{cases}
p_0q_0^{-1},&\text{ nếu }N=0, \\
\left(p_N-\sum_{j=1}^{N}q_j\tilde{q}_{N-j}\right)\cdot q_0^{-1},&\text{ nếu }N<d, \\
-q_0^{-1}\sum_{j=1}^{d}q_j\tilde{q}_{N-j},&\text{ các trường hợp khác}.
\end{cases}
$$

Chỉ cần lấy

$$
P(x)=\left(\left(\sum_{j\geq 0}a_jx^j\right)\cdot x^d\Gamma\left(x^{-1}\right)\right)\bmod{x^d}
$$

thì theo định nghĩa $\tilde{q}_N$ sẽ có $\dfrac{P(x)}{Q(x)}=\sum_{j\geq 0}a_jx^j$.

### Thuật toán Bostan–Mori

#### Tính một hệ số

Mục tiêu: cho $P(x),Q(x)$ như trên, tính $\left\lbrack x^k\right\rbrack\dfrac{P(x)}{Q(x)}$.

Bostan–Mori dựa trên lặp Graeffe:

$$
\frac{P(x)}{Q(x)}=\frac{P(x)Q(-x)}{Q(x)Q(-x)}=\frac{U_0(x^2)+xU_1(x^2)}{V(x^2)}
$$

Vì mẫu $V(x^2)$ là hàm chẵn, nên chỉ cần xét một nửa:

$$
\left\lbrack x^k\right\rbrack\dfrac{P(x)}{Q(x)}=\left\lbrack x^{\left\lfloor k/2\right\rfloor}\right\rbrack \frac{U_{k\bmod{2}}(x)}{V(x)}
$$

Chi phí hai phép nhân đa thức để giảm ít nhất một nửa bài toán; khi $k=0$ thì $\left\lbrack x^0\right\rbrack \dfrac{P(x)}{Q(x)}=\dfrac{P(0)}{Q(0)}$. Độ phức tạp tương tự.

#### Tính một đoạn liên tiếp

Mục tiêu: cho $P(x),Q(x)$, tính $\left\lbrack x^{\left\lbrack L,R\right)}\right\rbrack\dfrac{P(x)}{Q(x)}$. Ta chỉ xét các hệ số “có ảnh hưởng”.

Giả sử $\deg{P}<\deg{Q}$, nếu không thì dùng phép chia đa thức để đưa về dạng này.

Xét bài toán đơn giản:

$$
\left\lbrack x^{\left\lbrack L,R\right)}\right\rbrack\frac{1}{Q(x)}=\left\lbrack x^{\left\lbrack L,R\right)}\right\rbrack\frac{1}{Q(x)Q(-x)}\cdot Q(-x)
$$

Cần tính $\left\lbrack x^{\left\lbrack L-\deg{Q},R\right)}\right\rbrack\dfrac{1}{Q(x)Q(-x)}$, rồi nhân và lấy $x^L,\dots ,x^{R-1}$. Đặt $V(x^2)=Q(x)Q(-x)$, khi đó chỉ cần

$$
\left\lbrack x^{\left\lbrack \left\lceil\frac{L-\deg{Q}}{2}\right\rceil,\left\lceil\frac{R}{2}\right\rceil\right)}\right\rbrack\frac{1}{V(x)}
$$

để khôi phục $\left\lbrack x^{\left\lbrack L-\deg{Q},R\right)}\right\rbrack\dfrac{1}{Q(x)Q(-x)}$. Tiếp theo chỉ cần tính $\left\lbrack x^{\left\lbrack L-\deg{P},R\right)}\right\rbrack\dfrac{1}{Q(x)}$ rồi nhân với $P(x)$ để ra $\left\lbrack x^{\left\lbrack L,R\right)}\right\rbrack\dfrac{P(x)}{Q(x)}$.

Tuy nhiên thuật toán này còn phụ thuộc $R-L$ ở mỗi bước; ta muốn giảm. Hãy tính $\left\lbrack x^{\left\lbrack L,L+\deg Q+1\right)}\right\rbrack \dfrac{1}{Q(x)}$:

$$
\left\lbrack x^{\left\lbrack L,L+\deg Q+1\right)}\right\rbrack \frac{1}{Q(x)}=\left\lbrack x^{\left\lbrack L,L+\deg Q+1\right)}\right\rbrack \dfrac{1}{Q(x)Q(-x)}\cdot Q(-x)
$$

Cần:

$$
\left\lbrack x^{\left\lbrack L-\deg Q,L+\deg Q+1\right)}\right\rbrack \dfrac{1}{Q(x)Q(-x)}
$$

Với $V(x^2)=Q(x)Q(-x)$, chỉ cần:

$$
\left\lbrack x^{\left\lbrack \lceil (L-\deg Q)/2 \rceil,\lceil (L+\deg Q+1)/2 \rceil\right)}\right\rbrack \frac{1}{V(x)}
$$

bởi vì:

$$
\left\lbrack x^{k}\right\rbrack\dfrac{1}{Q(x)Q(-x)}=
\begin{cases}
\left\lbrack x^{k/2}\right\rbrack\dfrac{1}{V(x)},&\text{nếu }k\equiv 0\pmod{2}, \\
0,&\text{ngược lại}.
\end{cases}
$$

Ta biết $L+\deg Q$ và $L-\deg Q$ cùng parity, nên:

$$
\left\lceil \frac{L+\deg Q+1}{2}\right\rceil -\left\lceil \frac{L-\deg Q}{2}\right\rceil =
\begin{cases}
\deg Q+1,&\text{nếu }L+\deg Q\equiv 0\pmod{2}, \\
\deg Q,&\text{ngược lại}.
\end{cases}
$$

Mã giả:

$$
\begin{array}{ll}
&\textbf{Algorithm }\operatorname{Slice-Coefficients}(Q,L)\text{:} \\
&\textbf{Input}\text{: }Q(x)\in\mathbb{C}\left\lbrack x\right\rbrack,L\in\mathbb{Z}\text{.} \\
&\textbf{Output}\text{: }\left\lbrack x^{\left\lbrack L,L+\deg Q+1\right)}\right\rbrack Q(x)^{-1}\text{.} \\
1&\textbf{if }L\leq 1\textbf{ then return }\left\lbrack x^{\left\lbrack L,L+\deg Q+1\right)}\right\rbrack Q(x)^{-1} \\
&\text{Use other algorithm to compute }Q(x)^{-1} \\
2&V(x^2)\gets Q(x)Q(-x) \\
3&k\gets \left\lceil \frac{L-\deg Q}{2}\right\rceil \\
4&(t_k,\dots ,t_{k+\deg Q})\gets \operatorname{Slice-Coefficients}\left(V,k\right) \\
5&T(x)\gets x^{(L-\deg Q)\bmod{2}}\sum_{j=0}^{\deg Q}t_{j+k}x^{2j} \\
6&\textbf{return }\left\lbrack x^{\left\lbrack \deg Q,2\deg Q+1\right)}\right\rbrack T(x)Q(-x)
\end{array}
$$

Nhưng chỉ vậy chưa đủ; cần tìm một biểu diễn hữu tỉ mới để lấy thêm hệ số.

#### Tìm biểu diễn hữu tỉ mới

Biết $Q(x)$ và một đoạn hệ số của $Q(x)^{-1}$ như $\left\lbrack x^{\left\lbrack L,L+\deg Q\right)}\right\rbrack Q(x)^{-1}$ với $L\geq 0$, ta muốn tính $\left\lbrack x^{\left\lbrack L+\deg Q,L+2\deg Q\right)}\right\rbrack Q(x)^{-1}$. Tức là cần tìm $P(x)$, $\deg P<\deg Q$, sao cho $\dfrac{P(x)}{Q(x)}$ có $\deg Q$ hệ số đầu giống $\left\lbrack x^{\left\lbrack L,L+\deg Q\right)}\right\rbrack Q(x)^{-1}$. Nói cách khác: quan hệ truy hồi (mẫu) giữ nguyên, chỉ thay đổi điều kiện đầu.

Cụ thể:

$$
\frac{P(x)}{Q(x)}=\sum_{j\geq 0}a_jx^j
$$

Muốn dịch tiến $n$ bước:

$$
\sum_{j\geq n}a_jx^{j-n}=\frac{P(x)}{Q(x)x^n}-\frac{Q(x)\sum_{j=0}^{n-1}a_jx^j}{Q(x)x^n}
$$

Ta dùng $\operatorname{Slice-Coefficients}(Q,L-\deg{P})$ để tính $\left\lbrack x^{\left\lbrack L-\deg{P},L-\deg{P}+\deg{Q}+1\right)}\right\rbrack Q(x)^{-1}$, rồi ghép thành $\left\lbrack x^{\left\lbrack L-\deg{P},L+\deg{Q}\right)}\right\rbrack Q(x)^{-1}$. Sau đó tính lại tử số sao cho

$$
\frac{\widetilde{P}(x)}{Q(x)}=\sum_{j\geq 0}\left(\left\lbrack x^{L+j}\right\rbrack \frac{P(x)}{Q(x)}\right)x^j
$$

Cuối cùng dùng phép chia chuỗi lũy thừa để tính $\left\lbrack x^{\left\lbrack 0,R-L\right)}\right\rbrack\dfrac{\widetilde{P}(x)}{Q(x)}$, thời gian $O(\mathsf{M}(d)\log L+\mathsf{M}(R-L))$.

## Tài liệu tham khảo

1.  Alin Bostan, Ryuhei Mori.[A Simple and Fast Algorithm for Computing the $N$-th Term of a Linearly Recurrent Sequence](https://arxiv.org/abs/2008.08822).
