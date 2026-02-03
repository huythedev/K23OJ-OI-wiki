Phương pháp ký hiệu hóa (symbolic method) là cách chuyển nhanh đối tượng tổ hợp sang hàm sinh; ta xét các phép toán đặc biệt trên tập hợp và suy ra phép toán tương ứng trên hàm sinh.

Ta gọi một lớp tổ hợp (hoặc gọi tắt là lớp) là $(\mathcal{A},\lvert \cdot \rvert)$, trong đó $\mathcal{A}$ là tập các đối tượng tổ hợp, hàm $\lvert \cdot \rvert$ ánh xạ mỗi đối tượng sang một số nguyên không âm, thường gọi là hàm kích thước. Lưu ý số nguyên này không được vô hạn. Ví dụ với chuỗi trên bảng chữ $\lbrace 0,1\rbrace$ có thể lấy độ dài chuỗi làm hàm kích thước; với cây hoặc đồ thị có thể lấy số đỉnh làm hàm kích thước, nhưng điều này không tuyệt đối, cũng có thể gán một số đỉnh đặc biệt có kích thước $0$, v.v.

Bài viết này được giản lược từ chương 1 của sách *Analytic Combinatorics*.

## Hệ không gán nhãn

Trong hệ không gán nhãn sử dụng hàm sinh thường (OGF). Với tập $\mathcal{A}$, OGF tương ứng ký hiệu

$$
A(z)=\sum_{\alpha\in\mathcal{A}}z^{\lvert \alpha \rvert}=\sum_{n\geq 0}a_nz^n
$$

Ta quy ước dùng cùng chữ cái biểu diễn lớp và hàm sinh tương ứng; ví dụ $a_n$ là $\lbrack z^n\rbrack A(z)$ tức hệ số của $z^n$ trong $A(z)$, và $\mathcal{A}_n$ là tập các đối tượng trong $\mathcal{A}$ có kích thước $n$ (nên $a_n=\operatorname{card}(\mathcal{A}_n)$ với $\operatorname{card}$ là lực lượng (cardinality)).

Bài viết không bàn về tính khả dung (admissibility), độc giả có thể tham khảo tài liệu gốc.

Sau đây giới thiệu hai lớp và đối tượng tổ hợp đặc biệt:

-   Ký hiệu $\epsilon$ là đối tượng trung hòa (neutral object) và $\mathcal{E}=\lbrace \epsilon \rbrace$ là lớp trung hòa (neutral class), đối tượng trung hòa có kích thước $0$, OGF của lớp trung hòa là $E(z)=1$.
-   Ký hiệu $\circ$ hoặc $\bullet$ là đối tượng nguyên tử (atom object) và $\mathcal{Z}_{\circ}=\lbrace \circ\rbrace$ hoặc $\mathcal{Z}_{\bullet}=\lbrace \bullet\rbrace$, hoặc viết gọn là $\mathcal{Z}$ là lớp nguyên tử (atom class), đối tượng nguyên tử có kích thước $1$, OGF của lớp nguyên tử là $Z(z)=z$.

Với hai lớp $\mathcal{A},\mathcal{B}$ đẳng cấu theo nghĩa tổ hợp ký hiệu $\mathcal{A}=\mathcal{B}$ hoặc $\mathcal{A}\cong\mathcal{B}$, nhưng chỉ dùng ký hiệu sau khi đẳng cấu là không tầm thường.

Ta có

$$
\mathcal{A}\cong\mathcal{E}\times \mathcal{A}\cong\mathcal{A}\times\mathcal{E}
$$

trong đó $\times$ là phép toán nhị phân, biểu diễn tích Descartes của các tập.

### Cấu trúc hợp (không giao) của tập

Với lớp $\mathcal{A}$ và $\mathcal{B}$, hợp của chúng ký hiệu

$$
\mathcal{A}+\mathcal{B}=(\mathcal{E}_{1}\times\mathcal{A})+(\mathcal{E}_2\times\mathcal{B})
$$

Cách định nghĩa này không vi phạm yêu cầu “không giao” trong lý thuyết tập hợp; có thể tưởng tượng các đối tượng của $\mathcal{A}$ được tô đỏ, còn của $\mathcal{B}$ tô xanh.

OGF tương ứng là

$$
A(z)+B(z)
$$

Xét

$$
A(z)+B(z)=\sum _ {\alpha\in\mathcal{A}}z^{\lvert \alpha\rvert} + \sum _ {\beta\in\mathcal{B}}z^{\lvert \beta\rvert}=\sum_{n\geq 0}(a_n+b_n)z^n
$$

Tương ứng với phép cộng của chuỗi lũy thừa hình thức.

### Cấu trúc tích Descartes của tập

Với lớp $\mathcal{A}$ và $\mathcal{B}$, tích Descartes ký hiệu

$$
\mathcal{A}\times \mathcal{B}=\left\lbrace (\alpha, \beta)\mid \alpha \in \mathcal{A},\beta\in\mathcal{B}\right\rbrace
$$

OGF tương ứng là

$$
A(z)\cdot B(z)
$$

Ta định nghĩa kích thước của $(\alpha,\beta)$ là tổng kích thước của các thành phần, hiển nhiên cũng có

$$
\gamma =(\alpha_1,\alpha_2,\dots ,\alpha_n)\implies \lvert \gamma\rvert =\lvert \alpha_1\rvert +\lvert \alpha_2\rvert +\cdots +\lvert \alpha_n\rvert
$$

Do đó

$$
A(z)\cdot B(z)=\left(\sum _ {\alpha\in\mathcal{A}}z^{\lvert \alpha\rvert}\right)\left(\sum _ {\beta\in\mathcal{B}}z^{\lvert \beta\rvert}\right)=\sum _ {(\alpha, \beta)\in(\mathcal{A}\times \mathcal{B})}z^{\lvert \alpha\rvert +\lvert \beta\rvert}=\sum_{n\geq 0}\sum_{i+j=n}a_ib_jz^n
$$

Tương ứng với phép nhân của chuỗi lũy thừa hình thức.

### Cấu trúc Sequence của tập

Cấu trúc Sequence sinh ra mọi tổ hợp có thể.

???+ note "Ví dụ"
    $$
    \begin{aligned}
    \operatorname{SEQ}(\lbrace a\rbrace)&=\lbrace \epsilon\rbrace +\lbrace a\rbrace +\lbrace (a,a)\rbrace +\lbrace (a,a,a)\rbrace +\cdots\\
    \operatorname{SEQ}(\lbrace a,b\rbrace)&=\lbrace \epsilon\rbrace +\lbrace a,b\rbrace +\lbrace (a,b)\rbrace + \lbrace(b,a)\rbrace +\lbrace (a,a)\rbrace +\lbrace (b,b)\rbrace\\
    &+\lbrace (a,b,a)\rbrace +\lbrace (a,b,b)\rbrace +\lbrace (a,a,b)\rbrace\\
    &+\lbrace (b,b,a)\rbrace +\lbrace (b,a,b)\rbrace +\lbrace (b,b,b)\rbrace +\lbrace (a,a,a)\rbrace +\lbrace (b,a,a)\rbrace\\
    &+\cdots
    \end{aligned}
    $$
    
    Ta thấy các phần tử có thứ tự khác nhau như $\lbrace (a,b)\rbrace ,\lbrace (b,a)\rbrace$ đều xuất hiện, nên Sequence sinh ra tổ hợp có thứ tự.

Định nghĩa

$$
\operatorname{SEQ}(\mathcal{A})=\mathcal{E}+\mathcal{A}+(\mathcal{A}\times \mathcal{A})+(\mathcal{A}\times \mathcal{A}\times \mathcal{A})+\cdots
$$

và yêu cầu $\mathcal{A}_0=\varnothing$, tức trong $\mathcal{A}$ không có đối tượng kích thước $0$.

OGF tương ứng:

$$
Q(A(z))=1+A(z)+A(z)^2+A(z)^3+\cdots =\frac{1}{1-A(z)}
$$

trong đó $Q$ là Pólya quasi-inversion.

???+ note "Ví dụ: cây có gốc có thứ tự (ordered rooted tree)"
    Ta có thể dùng cấu trúc Sequence để định nghĩa cây có gốc có thứ tự, tức thứ tự giữa các con là quan trọng. Gọi lớp tổ hợp là $\mathcal{T}$ thì một cây gồm một nút gốc và một Sequence các cây con:
    
    $$
    \mathcal{T}=\lbrace \bullet\rbrace\times\operatorname{SEQ}(\mathcal{T})
    $$
    
    OGF tương ứng:
    
    $$
    T(z)=\frac{z}{1-T(z)}
    $$
    
    Các hệ số đầu là `0 1 1 2 5 14 42 132 429 1430 4862 16796`, bỏ hằng số là OEIS [A000108](http://oeis.org/A000108).

### Cấu trúc Multiset của tập

Cấu trúc Multiset sinh ra mọi tổ hợp nhưng không phân biệt thứ tự các phần tử.

???+ note "Ví dụ"
    $$
    \begin{aligned}
    \operatorname{MSET}(\lbrace a\rbrace)&=\lbrace \epsilon\rbrace +\lbrace a\rbrace +\lbrace (a,a)\rbrace +\lbrace (a,a,a)\rbrace +\cdots\\
    \operatorname{MSET}(\lbrace a,b\rbrace)&=\lbrace \epsilon\rbrace +\lbrace a\rbrace +\lbrace (a,a)\rbrace +\lbrace (a,a,a)\rbrace +\cdots\\
    &+\lbrace b\rbrace +\lbrace (a,b)\rbrace +\lbrace (a,a,b)\rbrace +\cdots \\
    &+\lbrace (b,b)\rbrace + \lbrace (a,b,b)\rbrace +\lbrace (a,a,b,b)\rbrace + \cdots\\
    &+\cdots
    \end{aligned}
    $$
    
    Ta thấy $\lbrace (b,a)\rbrace,\lbrace (a,b,a)\rbrace$ xuất hiện trong $\operatorname{SEQ}(\lbrace a,b\rbrace)$ nhưng không xuất hiện trong $\operatorname{MSET}(\lbrace a,b\rbrace)$, nên Multiset sinh ra tổ hợp không thứ tự.

Định nghĩa truy hồi:

$$
\operatorname{MSET}(\lbrace \alpha_0,\alpha_1,\dots, \alpha_n\rbrace)=\operatorname{MSET}(\lbrace \alpha_0,\alpha_1,\dots, \alpha_{n-1}\rbrace)\times \operatorname{SEQ}(\lbrace \alpha_n\rbrace)
$$

tức

$$
\operatorname{MSET}(\mathcal{A})=\prod _ {\alpha\in\mathcal{A}}\operatorname{SEQ}(\lbrace \alpha\rbrace)
$$

và yêu cầu $\mathcal{A}_0=\varnothing$. Hoặc tương đương:

$$
\operatorname{MSET}(\mathcal{A})=\operatorname{SEQ}(\mathcal{A})/\mathbf{R}
$$

trong đó $\mathbf{R}$ là quan hệ tương đương: $(\alpha_1,\dots,\alpha_n)\mathbf{R}(\beta_1,\dots,\beta_n)$ khi và chỉ khi tồn tại một hoán vị $\sigma$ sao cho với mọi $j$, $\beta_{j}=\alpha_{\sigma(j)}$.

OGF tương ứng:

$$
\operatorname{Exp}(A(z))=\prod _ {\alpha \in\mathcal{A}}\left(1-z^{\lvert \alpha \rvert}\right)^{-1}=\prod _ {n\geq 1}\left(1-z^n\right)^{-a_n}
$$

Lưu ý:

$$
\ln(1+z)=\frac{z}{1}-\frac{z^2}{2}+\frac{z^3}{3}-\cdots =\sum_{n\geq 1}\frac{(-1)^{n-1}z^n}{n}
$$

và $A(z)=\exp(\ln(A(z)))$, do đó

$$
\begin{aligned}
\operatorname{Exp}(A(z))&=\exp\left(\sum _ {n\geq 1}-a_n\cdot \ln\left(1-z^n\right)\right)\\
&=\exp\left(\sum _ {n\geq 1}-a_n\cdot \sum _ {m\geq 1}\frac{-z^{nm}}{m}\right)\\
&=\exp\left(\frac{A(z)}{1}+\frac{A(z^2)}{2}+\frac{A(z^3)}{3}+\cdots \right)
\end{aligned}
$$

trong đó $\operatorname{Exp}$ là chỉ số Pólya, còn gọi là biến đổi Euler.

???+ note "Ví dụ bài [LOJ 6268. Phân hoạch số](https://loj.ac/p/6268)"
    **Đề**: Gọi $f(n)$ là số cách phân hoạch $n$, hãy tính $f(1),f(2),\dots,f(10^5)$ modulo $998244353$.
    
    **Giải**: Gọi lớp số nguyên dương là $\mathcal{I}$, khi đó $\mathcal{I}=\operatorname{SEQ}_{\geq 1}(\mathcal{Z})=\mathcal{Z}\times \operatorname{SEQ}(\mathcal{Z})$ (chỉ số $\geq 1$ là cấu trúc có ràng buộc, xem phần sau). Ta cần
    
    $$
    \operatorname{MSET}(\mathcal{I})
    $$
    
    OGF có các hệ số đầu `1 2 3 5 7 11 15 22 30 42` (bỏ hằng số) tức OEIS [A000041](https://oeis.org/A000041).

???+ note "Ví dụ bài [洛谷 P4389 付公主的背包](https://www.luogu.com.cn/problem/P4389)"
    **Đề**: Cho $n$ loại hàng có thể tích $v_1,\dots ,v_n$ và số nguyên dương $m$, đếm số cách lấp đầy ba lô thể tích $1,2,\dots,m$ (không giới hạn số lượng, cùng thể tích có nhiều loại hàng khác nhau) modulo $998244353$. Giả sử $1\leq n,m\leq 10^5$, $1\leq v_i\leq m$.
    
    **Giải**: Gọi lớp hàng hóa là $\mathcal{A}$, ta cần $\operatorname{MSET}(\mathcal{A})$, lấy hệ số OGF tương ứng.

???+ note "Ví dụ bài [洛谷 P5900 Đếm cây vô gốc không gán nhãn](https://www.luogu.com.cn/problem/P5900)"
    **Đề**: Đếm số cây vô gốc không gán nhãn có $n$ đỉnh, modulo $998244353$, $1\leq n\leq 2\times 10^5$.
    
    **Giải**: Gọi lớp cây có gốc không gán nhãn là $\mathcal{T}$, khi đó
    
    $$
    \mathcal{T}=\lbrace \bullet\rbrace\times\operatorname{MSET}(\mathcal{T})
    $$
    
    Theo mô tả của Richard Otter trong [The Number of Trees](https://users.math.msu.edu/users/magyarp/Math482/Otter-Trees.pdf), OGF của cây vô gốc là
    
    $$
    T(z)-\frac{1}{2}T^2(z)+\frac{1}{2}T(z^2)
    $$
    
    Các hệ số đầu `1 1 1 2 3 6 11 23 47 106` (bỏ hằng số) là OEIS [A000055](https://oeis.org/A000055).

### Cấu trúc Powerset của tập

Cấu trúc Powerset sinh ra mọi tập con.

???+ note "Ví dụ"
    $$
    \begin{aligned}
    \operatorname{PSET}(\lbrace a\rbrace)&=\lbrace \epsilon\rbrace +\lbrace a\rbrace \\
    \operatorname{PSET}(\lbrace a,b\rbrace)&=\lbrace \epsilon\rbrace +\lbrace a\rbrace +\lbrace b\rbrace +\lbrace (a,b)\rbrace \\
    \operatorname{PSET}(\lbrace a,b,c\rbrace)&=\lbrace \epsilon\rbrace +\lbrace a\rbrace +\lbrace b\rbrace +\lbrace (a,b)\rbrace +\lbrace c\rbrace +\lbrace (a,c)\rbrace +\lbrace (b,c)\rbrace +\lbrace (a,b,c)\rbrace\\
    \end{aligned}
    $$

Định nghĩa truy hồi:

$$
\operatorname{PSET}(\lbrace \alpha_0,\alpha_1,\dots, \alpha_n\rbrace)=\operatorname{PSET}(\lbrace \alpha_0,\alpha_1,\dots, \alpha_{n-1}\rbrace)\times (\lbrace \epsilon\rbrace +\lbrace \alpha_n\rbrace)
$$

tức

$$
\operatorname{PSET}(\mathcal{A})\cong \prod _ {\alpha\in\mathcal{A}}\left(\lbrace \epsilon \rbrace +\lbrace \alpha\rbrace\right)
$$

và yêu cầu $\mathcal{A}_0=\varnothing$.

OGF tương ứng:

$$
\begin{aligned}
\overline{\operatorname{Exp}}(A(z))&=\prod _ {\alpha\in\mathcal{A}}\left(1+z^{\lvert \alpha \rvert}\right)=\prod _ {n\geq 1}\left(1+z^n\right)^{a_n}\\
&=\exp\left(\sum _ {n\geq 1}a_n\cdot \ln\left(1+z^n\right)\right)\\
&=\exp\left(\sum _ {n\geq 1}a_n\cdot \sum _ {m\geq 1}\frac{(-1)^{m-1}z^{nm}}{m}\right)\\
&=\exp\left(\frac{A(z)}{1}-\frac{A(z^2)}{2}+\frac{A(z^3)}{3}-\cdots \right)
\end{aligned}
$$

trong đó $\overline{\operatorname{Exp}}$ là chỉ số Pólya sửa đổi.

Dễ thấy $\operatorname{PSET}(\mathcal{A})\subset \operatorname{MSET}(\mathcal{A})$.

### Cấu trúc Cycle của tập

Cấu trúc Cycle sinh ra mọi tổ hợp nhưng không phân biệt các tổ hợp chỉ khác nhau bởi phép quay vòng.

Định nghĩa:

$$
\operatorname{CYC}(\mathcal{A})=\left(\operatorname{SEQ}(\mathcal{A})\setminus\lbrace \epsilon\rbrace\right)/\mathbf{S}
$$

trong đó $\mathbf{S}$ là quan hệ tương đương: $(\alpha_1,\dots,\alpha_n)\mathbf{S}(\beta_1,\dots,\beta_n)$ khi và chỉ khi tồn tại một phép quay vòng $\tau$ sao cho với mọi $j$ đều có $\beta_j=\alpha_{\tau(j)}$.

???+ note "Ví dụ"
    Để đơn giản, lấy $\texttt{a},\texttt{b}$ là các ký tự kích thước $1$, chỉ liệt kê chuỗi kích thước $3$ và $4$:
    
    $$
    \operatorname{CYC}(\lbrace \texttt{a},\texttt{b}\rbrace)_3=\lbrace \texttt{aaa}\rbrace +\lbrace \texttt{aab}\rbrace+\lbrace \texttt{abb}\rbrace+\lbrace \texttt{bbb}\rbrace
    $$
    
    Trong đó $\texttt{aab}\mathbf{S}\texttt{baa}\mathbf{S}\texttt{aba}$ chỉ giữ một; tương tự $\texttt{abb}\mathbf{S}\texttt{bab}\mathbf{S}\texttt{bba}$ chỉ giữ một.
    
    $$
    \operatorname{CYC}(\lbrace \texttt{a},\texttt{b}\rbrace)_4=\lbrace \texttt{aaaa}\rbrace +\lbrace \texttt{aaab}\rbrace+\lbrace \texttt{aabb}\rbrace+\lbrace \texttt{abbb}\rbrace+\lbrace \texttt{bbbb}\rbrace +\lbrace \texttt{abab}\rbrace
    $$
    
    Trong đó $\texttt{aaab}\mathbf{S}\texttt{baaa}\mathbf{S}\texttt{abaa}\mathbf{S}\texttt{aaba}$, $\texttt{aabb}\mathbf{S}\texttt{baab}\mathbf{S}\texttt{bbaa}\mathbf{S}\texttt{abba}$, $\texttt{abbb}\mathbf{S}\texttt{babb}\mathbf{S}\texttt{bbab}\mathbf{S}\texttt{bbba}$ và $\texttt{abab}\mathbf{S}\texttt{baba}$.

OGF tương ứng:

$$
\operatorname{Log}(A(z))=\sum _ {n\geq 1}\frac{\varphi(n)}{n}\ln\frac{1}{1-A(z^n)}
$$

trong đó $\varphi$ là hàm Euler, $\operatorname{Log}$ là Pólya logarithm.

Chứng minh khá phức tạp, độc giả có thể tham khảo [The Cycle Construction](https://epubs.siam.org/doi/10.1137/0404006) của Flajolet hoặc phụ lục của *Analytic Combinatorics*.

### Cấu trúc có ràng buộc

Với mọi cấu trúc ở trên, ta chưa giới hạn số lượng “thành phần”. Nếu trong $\operatorname{SEQ}$ thêm chỉ số là một vị từ trên số nguyên để ràng buộc số lượng thành phần, như

$$
\operatorname{SEQ}_{=k}(\mathcal{B}),\quad \operatorname{SEQ}_{\geq k}(\mathcal{B}),\quad \operatorname{SEQ}_{1..k}(\mathcal{B})
$$

trong đó $\operatorname{SEQ}_{=k}(\mathcal{B})$ cũng viết tắt là $\operatorname{SEQ}_k(\mathcal{B})$, $\operatorname{SEQ}_{1..k}(\mathcal{B})$ nghĩa là trong đoạn $\lbrack 1..k\rbrack$.

Gọi $\mathfrak{K}$ là một trong $\operatorname{SEQ},\operatorname{PSET},\operatorname{MSET},\operatorname{CYC}$, và

$$
\mathcal{A}=\mathfrak{K}_k(\mathcal{B})
$$

tức với $\alpha\in\mathcal{A}$ có

$$
\alpha =\lbrace (\beta_1,\beta_2,\dots ,\beta_k)\mid \beta\in\mathcal{B}\rbrace
$$

Gọi $\chi$ là hàm đếm số thành phần của đối tượng tổ hợp, tức $\chi(\alpha)=k$; ta thêm một biến để “theo dõi” số thành phần.

Đặt

$$
A _ {n,k}=\operatorname{card}\left\lbrace \alpha\in\mathcal{A}\mid \lvert \alpha\rvert =n,\chi(\alpha)=k\right\rbrace
$$

khi đó

$$
A(z,u)=\sum _ {n,k}A _ {n,k}u^kz^n=\sum _ {\alpha\in\mathcal{A}}z^{\lvert \alpha\rvert}u^{\chi(\alpha)}
$$

Rồi ta chỉ cần lấy hệ số $u^k$ để có biểu thức tương ứng, ví dụ $\mathcal{A}=\operatorname{SEQ}_k(\mathcal{B})$ cho

$$
\begin{aligned}
&{}A(z,u)=\sum _ {k\geq 0}u^kB(z)^k=\frac{1}{1-uB(z)}\\
\implies &{}A(z)=B(z)^k
\end{aligned}
$$

Hiển nhiên

$$
\mathcal{A}=\operatorname{SEQ}_{\geq k}(\mathcal{B})\implies A(z)=\frac{B(z)^k}{1-B(z)}
$$

Còn với $\operatorname{MSET} _ k(\mathcal{B})$ và $\operatorname{PSET} _ k(\mathcal{B})$ ta có

$$
\begin{aligned}
&{}A(z,u)=\prod_n\left(1-uz^n\right)^{-b_n}\\
\implies &{}A(z)=\lbrack u^k\rbrack \exp\left(\frac{u}{1}B(z)+\frac{u^2}{2}B(z^2)+\frac{u^3}{3}B(z^3)+\cdots\right)
\end{aligned}
$$

và

$$
\begin{aligned}
&{}A(z,u)=\prod_n\left(1+uz^n\right)^{b_n}\\
\implies &{}A(z)=\lbrack u^k\rbrack \exp\left(\frac{u}{1}B(z)-\frac{u^2}{2}B(z^2)+\frac{u^3}{3}B(z^3)-\cdots\right)
\end{aligned}
$$

Với $\operatorname{CYC}_k(\mathcal{B})$ tương tự.

??? note "Dùng công thức trên tính OGF của $\operatorname{MSET}_3(\mathcal{B})$ và $\operatorname{MSET}_4(\mathcal{B})$"
    Thử tính $\mathcal{A}=\operatorname{MSET}_3(\mathcal{B})$:
    
    $$
    \begin{aligned}
    \lbrack u^3\rbrack A(z,u)&= \frac{1}{0!}\left(\lbrack u^3\rbrack 1\right)+\frac{1}{1!}\left(\lbrack u^3\rbrack \left(\frac{u}{1}B(z)+\frac{u^2}{2}B(z^2)+\frac{u^3}{3}B(z^3)+\cdots \right)\right)\\
    &+\frac{1}{2!}\left(\lbrack u^3\rbrack \left(\frac{u}{1}B(z)+\frac{u^2}{2}B(z^2)+\cdots \right)^2\right)\\
    &+\frac{1}{3!}\left(\lbrack u^3\rbrack \left(\frac{u}{1}B(z)+\cdots \right)^3\right)\\
    &=\frac{B(z)^3}{6}+\frac{B(z)B(z^2)}{2}+\frac{B(z)^3}{3}
    \end{aligned}
    $$
    
    Thử tính $\mathcal{A}=\operatorname{MSET}_4(\mathcal{B})$:
    
    $$
    \begin{aligned}
    \lbrack u^4\rbrack A(z,u)&= \frac{1}{0!}\left(\lbrack u^4\rbrack 1\right)+\frac{1}{1!}\left(\lbrack u^4\rbrack \left(\frac{u}{1}B(z)+\frac{u^2}{2}B(z^2)+\frac{u^3}{3}B(z^3)+\frac{u^4}{4}B(z^4)+\cdots \right)\right)\\
    &+\frac{1}{2!}\left(\lbrack u^4\rbrack \left(\frac{u}{1}B(z)+\frac{u^2}{2}B(z^2)+\frac{u^3}{3}B(z^3)+\cdots \right)^2\right)\\
    &+\frac{1}{3!}\left(\lbrack u^4\rbrack \left(\frac{u}{1}B(z)+\frac{u^2}{2}B(z^2)+\cdots \right)^3\right)\\
    &+\frac{1}{4!}\left(\lbrack u^4\rbrack \left(\frac{u}{1}B(z)+\cdots \right)^4\right)\\
    &=\frac{B(z^4)}{4}+\frac{1}{2!}\left(\frac{B(z^2)^2}{4}+\frac{2B(z)B(z^3)}{3}\right)+\frac{1}{3!}\left(\frac{3B(z)^2B(z^2)}{2}\right)+\frac{B(z)^4}{4!}\\
    &=\frac{B(z)^4}{24}+\frac{B(z)^2B(z^2)}{4}+\frac{B(z)B(z^3)}{3}+\frac{B(z^2)^2}{8}+\frac{B(z^4)}{4}
    \end{aligned}
    $$

Ta thấy $\mathcal{A}=\mathfrak{K}_k(\mathcal{B})$ thì $A(z)$ là biểu thức theo $B(z),B(z^2),\dots ,B(z^k)$.

Lưu ý với cấu trúc có ràng buộc $\mathfrak{K}_k(\mathcal{B})$ không yêu cầu $\mathcal{B}_0=\varnothing$.

???+ note "Các cấu trúc có ràng buộc thường dùng"
    $$
    \begin{aligned}
    \operatorname{PSET} _ {2}(\mathcal{A})&:\quad \frac{A(z)^2}{2}-\frac{A(z^2)}{2}\\
    \operatorname{MSET} _ {2}(\mathcal{A})&:\quad \frac{A(z)^2}{2}+\frac{A(z^2)}{2}\\
    \operatorname{CYC} _ {2}(\mathcal{A})&:\quad \frac{A(z)^2}{2}+\frac{A(z^2)}{2}
    \end{aligned}
    $$
    
    $$
    \begin{aligned}
    \operatorname{PSET} _ {3}(\mathcal{A})&:\quad \frac{A(z)^3}{6}-\frac{A(z)A(z^2)}{2}+\frac{A(z^3)}{3}\\
    \operatorname{MSET} _ {3}(\mathcal{A})&:\quad \frac{A(z)^3}{6}+\frac{A(z)A(z^2)}{2}+\frac{A(z^3)}{3}\\
    \operatorname{CYC} _ {3}(\mathcal{A})&:\quad \frac{A(z)^3}{3}+\frac{2A(z^3)}{3}\\
    \end{aligned}
    $$
    
    $$
    \begin{aligned}
    \operatorname{PSET} _ {4}(\mathcal{A})&:\quad \frac{A(z)^4}{24}-\frac{A(z)^2A(z^2)}{4}+\frac{A(z)A(z^3)}{3}+\frac{A(z^2)^2}{8}-\frac{A(z^4)}{4}\\
    \operatorname{MSET} _ {4}(\mathcal{A})&:\quad \frac{A(z)^4}{24}+\frac{A(z)^2A(z^2)}{4}+\frac{A(z)A(z^3)}{3}+\frac{A(z^2)^2}{8}+\frac{A(z^4)}{4}\\
    \operatorname{CYC} _ {4}(\mathcal{A})&:\quad \frac{A(z)^4}{4}+\frac{A(z^2)^2}{4}+\frac{A(z^4)}{2}\\
    \end{aligned}
    $$

Phương pháp tính trên dù hiệu quả nhưng khá rườm rà; độc giả có thể đọc [Pólya Enumeration Theorem](https://mathworld.wolfram.com/PolyaEnumerationTheorem.html) và [Cycle Index](https://mathworld.wolfram.com/CycleIndex.html) trên WolframMathWorld; Cycle Index cũng thường xuất hiện trong biểu thức hàm sinh của OEIS.

???+ note "Ví dụ bài [LOJ 6538. Đếm alkyl nâng cao](https://loj.ac/p/6538)"
    **Đề**: Đếm số cây vô thứ tự có gốc, trong đó bậc của gốc không quá $3$, các đỉnh còn lại không quá $4$, với $n$ đỉnh, modulo $998244353$, $1\leq n\leq 10^5$.
    
    **Giải**: Gọi lớp tổ hợp $\mathcal{T}$, khi đó
    
    $$
    \mathcal{T}=\lbrace \bullet\rbrace\times\operatorname{MSET}_{0,1,2,3}(\mathcal{T})
    $$
    
    Hoặc đặt $\hat{\mathcal{T}}=\mathcal{T}+\lbrace \epsilon\rbrace$ thì
    
    $$
    \hat{\mathcal{T}}=\lbrace \epsilon\rbrace +\lbrace \bullet\rbrace\times\operatorname{MSET}_{3}(\hat{\mathcal{T}})
    $$
    
    Cho ra cùng kết quả.

## Tài liệu tham khảo

-   Philippe Flajolet and Robert Sedgewick.[Analytic Combinatorics](http://algo.inria.fr/flajolet/Publications/books.html).
