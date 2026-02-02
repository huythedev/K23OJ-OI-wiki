Nghiên cứu ánh xạ tuyến tính là nghiên cứu các ánh xạ giữa các không gian tuyến tính.

Ánh xạ tuyến tính có thể biểu diễn bằng ma trận, nên nhiều khái niệm trong ma trận có đối ứng ở đây.

## Ánh xạ tuyến tính và biến đổi tuyến tính

Giả sử $V$ và $W$ là không gian tuyến tính trên trường $F$, và $T$ là ánh xạ từ $V$ sang $W$.

Nếu với mọi vectơ $x,y\in W$ và mọi vô hướng $k,l\in F$ ta có:

$$
T(kx+ly)=kTx+lTy
$$

thì $T$ là ánh xạ tuyến tính từ $V$ sang $W$. Nếu $W=V$ thì $T$ là biến đổi tuyến tính trên $V$.

Ví dụ: biến đổi đồng nhất $T_e$ giữ nguyên không gian, biến đổi 0 $T_0$ đưa mọi thứ về không gian 0.

Ký hiệu $L(V,W)$ là tập mọi ánh xạ tuyến tính từ $V$ sang $W$. Với biến đổi tuyến tính $L(V,V)$ cũng ký hiệu $L(V)$.

### Tính chất

-   Ánh xạ tuyến tính đưa vectơ không về vectơ không.
-   Ánh xạ tuyến tính giữ dạng phép toán tuyến tính.
-   Ánh xạ tuyến tính bảo toàn tính phụ thuộc tuyến tính.

Nhưng không bảo toàn độc lập tuyến tính.

## Biểu diễn ma trận của ánh xạ tuyến tính

Giả sử $\dim V = n$ có cơ sở $\alpha_1,\cdots,\alpha_n$, $\dim W = m$ có cơ sở $\beta_1,\cdots,\beta_m$, và $T$ là ánh xạ tuyến tính từ $V$ sang $W$.

Biểu diễn $T\alpha_j$ theo cơ sở $\beta$:

$$
T\alpha_j=a_{1j}\beta_1+\cdots+a_{mj}\beta_m
$$

Viết ma trận:

$$
T(\alpha_1,\cdots,\alpha_n)=(T\alpha_1,\cdots,T\alpha_n)=(\beta_1,\cdots,\beta_m)A
$$

Ma trận $A$ gọi là ma trận biểu diễn của $T$ theo hai cơ sở trên.

## Kernel và Image của ánh xạ tuyến tính

Đây là góc nhìn ánh xạ. Dễ thấy kernel/image của ánh xạ trùng với kernel/image của ma trận biểu diễn.

Với $T:V\to W$:

$$
N(T)=\{x\in V|Tx=0\}
$$

$$
R(T)=Im(T)=\{y\in W|y=Tx,Vx\in V\}
$$

$N(T)$ là không gian con của $V$, $R(T)$ là không gian con của $W$. Số chiều của $N(T)$ là **khuyết** (nullity), của $R(T)$ là **hạng**.

Định lý: nếu $\dim V$ hữu hạn thì:

$$
\operatorname{dim} N(T)+\operatorname{dim} R(T)=\operatorname{dim} V
$$

## Biểu diễn ma trận của biến đổi tuyến tính

Với $V$ $n$ chiều, cơ sở $\alpha_1,\cdots,\alpha_n$, biến đổi $T$:

$$
T\alpha_j=a_{1j}\alpha_1+\cdots+a_{nj}\alpha_n
$$

Ma trận:

$$
T(\alpha_1,\cdots,\alpha_n)=(T\alpha_1,\cdots,T\alpha_n)=(\alpha_1,\cdots,\alpha_n)A
$$

Ma trận $A$ là biểu diễn của $T$ theo cơ sở này.

Do cấu trúc không gian và tính tuyến tính, $T$ xác định duy nhất bởi $T\alpha_1,\cdots,T\alpha_n$, nên cũng xác định duy nhất $A$.

Định lý: Với $n$ chiều và cơ sở $\alpha$, mọi ma trận vuông $A$ xác định duy nhất một biến đổi tuyến tính $T$ sao cho $A$ là ma trận của $T$.

Hệ quả: giữa $L(V)$ và tập ma trận vuông bậc $n$ có tương ứng một-một.

Ví dụ: biến đổi 0 ↔ ma trận 0, biến đổi đồng nhất ↔ ma trận đơn vị.

## Không gian các biến đổi tuyến tính

Định lý: $L(V)$ cũng là một không gian tuyến tính, với:

$$
(T_1+T_2)x=T_1x+T_2x
$$

$$
(kT_1)x=k(T_1x)
$$

Nên $L(V)$ là không gian tuyến tính.

Với $T_1,T_2\in L(V)$, định nghĩa tích:

$$
(T_1T_2)x=T_2(T_1x)
$$

Thì $T_1T_2$ vẫn là biến đổi tuyến tính, kết hợp nhưng không giao hoán, giống ma trận.

Nếu tồn tại $T_2$ sao cho:

$$
(T_1T_2)x=T_1(T_2x)=x
$$

thì $T_2$ là nghịch biến của $T_1$, ký hiệu:

$$
T_2=T_1^{-1}
$$

và:

$$
T_1T_2=T_2T_1=T_e
$$

Định lý: nếu $T_1$ có ma trận $A$ và $T_2$ có ma trận $B$ dưới cùng cơ sở, thì:

-   $T_1+T_2$ có ma trận $A+B$
-   $kT_1$ có ma trận $kA$
-   $T_1T_2$ có ma trận $AB$
-   $T_1^{-1}$ (nếu có) có ma trận $A^{-1}$

## Tọa độ

Với cơ sở $x_1,\cdots,x_n$ của $V$, mọi $y\in V$:

$$
y=a_1x_1+\cdots+a_nx_n=(x_1,\cdots,x_n)\begin{pmatrix}a_1\\\vdots\\a_n\end{pmatrix}
$$

Vectơ cột trên gọi là **tọa độ** của $y$ theo cơ sở đó.

Tọa độ là vectơ trong trường, khác với vectơ trong $V$.

## Công thức đổi tọa độ

Giả sử $T$ có ma trận $A$ theo cơ sở $\alpha_1,\cdots,\alpha_n$. Đặt:

$$
\xi=(\alpha_1,\cdots,\alpha_n)\begin{pmatrix}x_1\\\vdots\\x_n\end{pmatrix}
$$

và:

$$
T\xi=T(\alpha_1,\cdots,\alpha_n)\begin{pmatrix}y_1\\\vdots\\y_n\end{pmatrix}
$$

Suy ra:

$$
T\xi=(\alpha_1,\cdots,\alpha_n)A\begin{pmatrix}x_1\\\vdots\\x_n\end{pmatrix}
$$

Điểm trong $V$ luôn là “cơ sở nhân tọa độ”. Với cơ sở đơn vị $I$, ta có $x=Ix$.

Giữ nguyên cơ sở, biến đổi $T$ tương đương nhân ma trận lên tọa độ.

Có thể hiểu $T$ như bộ lọc quan sát: không gian bị biến dạng nhưng điểm “thật” không đổi.

Điều này cho thấy: biến đổi tuyến tính tương đương với nhân bên phải vào ma trận cơ sở.

Do đó, giữa các cơ sở khác nhau, tọa độ đổi bằng nhân trái với nghịch đảo ma trận chuyển đổi.

## Ma trận chuyển đổi cơ sở

Cho hai cơ sở $x_1,\dots,x_n$ và $y_1,\dots,y_n$. Với mỗi $y_i$:

$$
y_i=(x_1,\cdots,x_n)\begin{pmatrix}a_{1i}\\\vdots\\a_{ni}\end{pmatrix}
$$

Ghép lại:

$$
(y_1,\cdots,y_n)=(x_1,\cdots,x_n)A
$$

$A$ là **ma trận chuyển cơ sở** từ $x$ sang $y$. Ma trận này khả nghịch.

Với cùng vectơ $z$:

$$
z=(x_1,\cdots,x_n)\begin{pmatrix}\xi_1\\\vdots\\\xi_n\end{pmatrix}=(y_1,\cdots,y_n)\begin{pmatrix}\eta_1\\\vdots\\\eta_n\end{pmatrix}
$$

Thay $(y_1,\cdots,y_n)=(x_1,\cdots,x_n)A$:

$$
\begin{pmatrix}\xi_1\\\vdots\\\xi_n\end{pmatrix}=A\begin{pmatrix}\eta_1\\\vdots\\\eta_n\end{pmatrix}
$$

Hay:

$$
\begin{pmatrix}\eta_1\\\vdots\\\eta_n\end{pmatrix}=A^{-1}\begin{pmatrix}\xi_1\\\vdots\\\xi_n\end{pmatrix}
$$

Đây là đổi tọa độ hoàn toàn trong trường.

Ma trận có thể biến đổi toàn bộ không gian tọa độ. Nhân trái bởi $A$ tương đương biến đổi không gian, vì $A$ gửi các vectơ đơn vị thành các cột của $A$.

Vectơ trái nhân ma trận cũng có thể hiểu là “tọa độ nhân bởi nhóm cơ sở”.

Như:

$$
Iy=Xa
$$

Với cùng vectơ $y$, trong không gian “chuẩn” (cơ sở $I$), tọa độ là $y$, còn trong không gian mới, tọa độ là $a$. Ma trận $X$ vừa là cơ sở mới, vừa là ma trận chuyển từ $I$ sang $X$.

Biến đổi tuyến tính $T$ gửi một cơ sở sang cơ sở khác, nên tọa độ cũng chuyển.

Nếu $T$ gửi cơ sở $\alpha$ sang $\beta$ và ma trận chuyển là $A$, thì $\beta=\alpha A$. Tọa độ đổi ngược lại.

## Biến đổi tuyến tính và ma trận tương tự

Cho biến đổi $T$ trên $V$ và cơ sở $\alpha$:

$T(\alpha)=\alpha A$.

Ma trận tương tự xét cùng một $T$ trong hai cơ sở khác nhau. Nếu $\beta=\alpha C$, thì:

$$
T(\beta)=T(\alpha)C=\alpha AC
$$

Mặt khác:

$$
T(\beta)=\beta B=\alpha CB
$$

Suy ra:

$$
B=C^{-1}AC
$$

Định lý: ma trận của cùng một biến đổi tuyến tính trong các cơ sở khác nhau là **tương tự**.

Nếu tồn tại khả nghịch $C$ sao cho $B=C^{-1}AC$, thì $A$ và $B$ tương tự.

Tương tự bảo toàn hạng, nên suy ra tương đương ma trận, nhưng ngược lại không đúng.

Tương tự liên quan hình dạng, không liên hệ với tương đương nhóm hay hệ cùng nghiệm.

Giải thích lại: $\beta=\alpha C$, $T(\alpha)=\alpha A$, $T(\beta)=\beta B$, $T(\beta)=T(\alpha)C$.

## Tài liệu tham khảo

-   [【官方双语/合集】线性代数的本质 - 系列合集 P13 09 - 基变换](https://www.bilibili.com/video/BV1Ls411b7r2)
