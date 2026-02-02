## Phân rã Jordan

Cho $T$ là biến đổi tuyến tính trên không gian $V$ $n$ chiều. Nếu đa thức tối tiểu:

$$
m_A(\lambda)={(\lambda-\lambda_1)}^{r_1}{(\lambda-\lambda_2)}^{r_2}\cdots{(\lambda-\lambda_k)}^{r_k}
$$

Theo phân rã sơ cấp, $V$ tách thành trực tổng:

$$
V=V_1\oplus V_2\oplus\cdots\oplus V_k
$$

trong đó $V_i=N\left({(A-\lambda_i I)}^{r_i}\right)$, $A$ là ma trận của $T$. Các không gian con này bất biến theo $T$.

Đặt $T_i$ là phép chiếu của $V$ lên $V_i$, tức xây dựng đa thức $u_i(T)$ sao cho:

-   $$
    T_i=u_i(T)\frac{m_A(T)}{{(T-\lambda_i T_e)}^{r_i}}
    $$
-   $$
    T_1+T_2+\cdots+T_k=T_e
    $$

$T_e$ là biến đổi đồng nhất. Khi đó:

-   ${T_i|}_{V_i}$ là đồng nhất trên $V_i$.
-   Với $i\ne j$, ${T_i|}_{V_j}$ là biến đổi 0.

Do đó $T_i$ gửi vectơ $\xi$ thành thành phần $\xi_i$ trong $V_i$.

Xây:

$$
T_D=\lambda_1 T_1+\lambda_2 T_2+\cdots+\lambda_k T_k
$$

Vì mỗi $T_i$ là đa thức của $T$, nên $T_D$ cũng là đa thức của $T$, và mỗi $V_i$ bất biến theo $T_D$.

Trên $V_i$, ${T_D|}_{V_i}$ là phép vị tự hệ số $\lambda_i$, nên $T_D$ khả chéo hóa.

Đặt:

$$
T_N=T-T_D
$$

$T_N$ cũng là đa thức của $T$, nên mỗi $V_i$ bất biến theo $T_N$. Với $\xi_i\in V_i$:

$$
{T_N}^{r_i}(\xi_i)={T-T_D}^{r_i}(\xi_i)={T-\lambda_i T_i}^{r_i}(\xi_i)=0
$$

Gọi $r=\max r_i$, thì với mọi $\xi\in V$, $T_N^r(\xi)=0$. Vậy $T_N$ là biến đổi nilpotent.

Ta có:

$$
T=T_D+T_N
$$

và do $T_D,T_N$ là đa thức của $T$, nên:

$$
T_DT_N=T_NT_D
$$

Định lý: nếu $T_1,T_2$ khả chéo hóa và $T_1T_2=T_2T_1$, thì tồn tại một cơ sở chung sao cho cả hai có dạng chéo.

Định lý: với mọi $T$ trên $V$, tồn tại $T_D$ khả chéo hóa và $T_N$ nilpotent sao cho:

-   $$
    T=T_D+T_N
    $$
-   $$
    T_DT_N=T_NT_D
    $$

Chúng là đa thức của $T$ và xác định duy nhất. Đây là phân rã Jordan của $T$. $T_D$ là phần khả chéo hóa, $T_N$ là phần nilpotent.

Tương tự cho ma trận $A$:

Định lý: tồn tại $D$ khả chéo hóa và $N$ nilpotent sao cho:

-   $$
    A=D+N
    $$
-   $$
    DN=ND
    $$

Chúng là đa thức của $A$ và xác định duy nhất.

## Ma trận lambda

Tiếp theo là ma trận có tham số $\lambda$, không chỉ là số. Trường cơ sở trở thành trường các phân thức hữu tỉ theo $\lambda$.

Ma trận có phần tử là đa thức theo $\lambda$ gọi là ma trận $\lambda$, ký hiệu $A(\lambda)$.

Ma trận số là trường hợp đặc biệt. Ma trận đặc trưng $\lambda I-A$ là một ma trận $\lambda$.

### Biến đổi sơ cấp của ma trận lambda

Với ma trận $\lambda$, cũng định nghĩa cộng, trừ, nhân, biến đổi sơ cấp, hạng. Với ma trận vuông $\lambda$, định nghĩa định thức, phần bù, phần bù đại số.

Biến đổi sơ cấp như ma trận số, nhưng phép “cộng bội” (giả sử theo hàng) là:

-   Nhân một hàng với đa thức $\varphi(\lambda)$ và cộng vào hàng khác.

Phép nhân hàng vẫn như cũ vì nhân hàng làm đổi định thức; để giữ tính chất hạng, định thức chỉ thay đổi trong trường số.

Ma trận sơ cấp tương ứng cũng được sửa như vậy.

Ba loại ma trận sơ cấp đều có định thức là hằng khác 0, nên đều full rank. Nhân trái hoặc phải không đổi hạng.

Nếu $A(\lambda)$ biến đổi sơ cấp hữu hạn lần thành $B(\lambda)$, gọi là tương đương.

Tương đương ⇒ hạng bằng nhau, nhưng ngược lại không đúng (khác ma trận số).

## Dạng chuẩn Smith

Định lý: nếu hạng của $A(\lambda)$ là $r$, thì $A(\lambda)$ tương đương với:

$$
\begin{pmatrix}
D(\lambda) & 0\\
0 & 0\\
\end{pmatrix}
$$

trong đó:

$$
D(\lambda)=\begin{pmatrix}
d_1(\lambda) &  & \\
 & \ddots & \\
 &  & d_r(\lambda)\\
\end{pmatrix}
$$

Mỗi $d_i(\lambda)$ là đa thức đơn thức đầu hệ số 1, và $d_i(\lambda)|d_{i+1}(\lambda)$.

Gọi là dạng chuẩn Smith, các $d_i(\lambda)$ là **bất biến tử**.

Cách tính: khử từ góc trên trái, mỗi lần chọn phần tử góc là GCD của phần còn lại, rồi khử hàng/cột.

Định lý: $A(\lambda)$ tương đương $B(\lambda)$ khi và chỉ khi có cùng bất biến tử.

### Thừa số sơ cấp

Theo định lý cơ bản đại số, với bất biến tử:

$$
d_i(\lambda)={(\lambda-\lambda_1)}^{e_{i1}}{(\lambda-\lambda_2)}^{e_{i2}}\cdots{(\lambda-\lambda_S)}^{e_{iS}}
$$

$\lambda_1,\cdots,\lambda_S$ phân biệt. Do $d_i|d_{i+1}$ nên $e_{ij}$ tăng dần và $d_m$ có các số mũ đều khác 0.

Các nhân tử có số mũ dương gọi là **thừa số sơ cấp** (tính cả bội).

Định lý: $A(\lambda)$ và $B(\lambda)$ có cùng bất biến tử ⇔ có cùng thừa số sơ cấp và cùng hạng.

Có thể đưa $A(\lambda)$ về đường chéo, rồi suy ra thừa số sơ cấp, hạng, bất biến tử. Nếu:

$$
\operatorname{diag}\{f_1(\lambda),f_2(\lambda),\cdots,f_r(\lambda),0,\cdots,0\}
$$

thì các thừa số bậc một của $f_i$ tạo thành thừa số sơ cấp.

Từ thừa số sơ cấp và hạng, cách dựng bất biến tử: nhóm theo nhân tử, sắp giảm mũ theo hàng, lấy cột bằng số hạng $r$, thêm $1$ nếu thiếu; tích mỗi cột là một bất biến tử.

### Ứng dụng cho ma trận đặc trưng

Nếu $A,B$ là ma trận số, thì $\lambda I-A$ và $\lambda I-B$ là ma trận $\lambda$. Có:

Định lý: $A$ tương tự $B$ ⇔ $\lambda I-A$ tương đương $\lambda I-B$.

Định thức đặc trưng có hạng $n$, nên hạng bằng nhau. Suy ra:

$A$ tương tự $B$ ⇔ $\lambda I-A$ và $\lambda I-B$ có cùng thừa số sơ cấp.

Với $\lambda I-A$, biến đổi sơ cấp không đổi hạng. Phép cộng bội không đổi định thức, nên định thức chỉ đổi hằng số; do đó phân tích nhân tử và bậc không đổi.

Vì $\det(\lambda I-A)$ bậc $n$, tổng bậc các thừa số sơ cấp bằng $n$.

## Dạng chuẩn Jordan

Ma trận

$$
\begin{pmatrix}
\lambda & 1 & 0 & \cdots & 0 & 0\\
0 & \lambda & 1 & \cdots & 0 & 0\\
0 & 0 & \lambda & \cdots & 0 & 0\\
\vdots & \vdots & \vdots &  & \vdots & \vdots\\
0 & 0 & 0 & \cdots & \lambda & 1\\
0 & 0 & 0 & \cdots & 0 & \lambda\\
\end{pmatrix}
$$

gọi là một **Jordan block** thuộc $\lambda$.

Jordan nilpotent là trường hợp $\lambda=0$.

Định lý: nếu $T$ có các trị riêng phân biệt $\lambda_1,\dots,\lambda_k$, thì tồn tại cơ sở sao cho ma trận của $T$ có dạng:

$$
\begin{pmatrix}
B_1 &  &  & 0\\
 & B_2 &  & \\
 &  & \ddots & \\
0 &  &  & B_k\\
\end{pmatrix}
$$

với

$$
B_i=\begin{pmatrix}
J_{i1} &  &  & 0\\
 & J_{i2} &  & \\
 &  & \ddots & \\
0 &  &  & J_{is_i}\\
\end{pmatrix}
$$

và $J_{ij}$ là các Jordan block thuộc $\lambda_i$.

Lý do: theo đa thức tối tiểu:

$$
m_A(\lambda)={(\lambda-\lambda_1)}^{r_1}{(\lambda-\lambda_2)}^{r_2}\cdots{(\lambda-\lambda_k)}^{r_k}
$$

ta có phân rã:

$$
V=V_1\oplus V_2\oplus\cdots\oplus V_k
$$

với:

$$
V_i=N\left({(A-\lambda_i I)}^{r_i}\right)
$$

Gọi $S_i={T|}_{V_i}$. Khi đó:

$$
S_i=\lambda_i T_e+T_i
$$

với $T_i$ nilpotent trên $V_i$. Không gian $V_i$ phân rã thành tổng trực tiếp của các không gian tuần hoàn của $T_i$:

$$
V_i=W_{i1}\oplus W_{i2}\oplus\cdots\oplus W_{is_i}
$$

Chọn cơ sở tuần hoàn đảo ngược cho từng $W_{ij}$, ghép lại thành cơ sở của $V_i$, ta được ma trận $T_i$ là khối Jordan nilpotent. Khi cộng với $\lambda_i I$, ta được các Jordan block.

Ghép cơ sở các $V_i$ lại, ta được dạng Jordan chuẩn của $T$.

Ma trận dạng:

$$
\begin{pmatrix}
J_1 &  &  & 0\\
 & J_2 &  & \\
 &  & \ddots & \\
0 &  &  & J_m\\
\end{pmatrix}
$$

là Jordan chuẩn.

Định lý: mọi ma trận vuông $A$ tương tự một Jordan chuẩn; dạng Jordan chuẩn xác định duy nhất (trừ thứ tự các block).

Trong dạng Jordan, đường chéo chính tạo thành phần khả chéo hóa, còn phần trên đường chéo là phần nilpotent.

Định lý: mỗi Jordan block

$$
J_i=\begin{pmatrix}
\lambda_i & 1 &  &  & \\
 & \lambda_i & 1 &  & \\
 &  & \ddots & \ddots & \\
 &  &  & \ddots & 1\\
 &  &  &  & \lambda_i\\
\end{pmatrix}
$$

tương ứng với một thừa số sơ cấp ${(\lambda-\lambda_i)}^{n_i}$ của $\lambda I-A$. Các thừa số sơ cấp tương ứng với các Jordan block.

Hệ quả: $A$ khả chéo hóa ⇔ các thừa số sơ cấp của $\lambda I-A$ đều bậc 1.

## Định lý Frobenius

Smith chuẩn của $\lambda I-A$ có hạng $n$.

Định lý: nếu Smith chuẩn của $\lambda I-A$ là

$$
\operatorname{diag}\{d_1(\lambda),d_2(\lambda),\cdots,d_n(\lambda)\}
$$

thì $d_n(\lambda)$ chính là đa thức tối tiểu $m_A(\lambda)$.

Hệ quả: $A$ khả chéo hóa ⇔

-   $m_A(\lambda)$ không có nghiệm bội.
-   Bất biến tử của $\lambda I-A$ không có nghiệm bội.
-   Thừa số sơ cấp của $\lambda I-A$ đều bậc 1.
