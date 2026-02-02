## Không gian riêng

Tập mọi vectơ riêng ứng với $\lambda_0$ (thêm vectơ 0) tạo thành không gian tuyến tính, gọi là không gian riêng $E(\lambda_0)$ của ma trận $A$. Nó là không gian nghiệm của:

$$
(\lambda_0 I-A)X=0
$$

Với $E(\lambda_i)=N(\lambda_i I-A)$, theo định lý khuyết + hạng:

$$
r(\lambda_i I-A)+\operatorname{dim} N(\lambda_i I-A)=n
$$

Suy ra:

$$
\operatorname{dim} E(\lambda_i)=n-r(\lambda_i I-A)
$$

gọi là **bội hình học** của $\lambda_i$.

## Không gian con bất biến

Khi nghiên cứu biến đổi tuyến tính $T$, ta muốn chọn cơ sở sao cho ma trận đơn giản.

Cho $V$ trên trường $F$, $W$ là không gian con, $T$ là biến đổi tuyến tính. Nếu với mọi $x\in W$, $T(x)\in W$, thì $W$ là không gian con bất biến.

Không gian bất biến không phải “tọa độ không đổi”, mà là sau biến đổi vẫn nằm trong không gian đó.

-   Mọi không gian con của $V$ là bất biến với phép nhân vô hướng.
-   Với mọi $T$, $V$ và không gian 0 là bất biến (tầm thường).
-   Giao và tổng của các không gian bất biến cũng bất biến.

Với không gian bất biến $W$, hạn chế $T$ lên $W$ gọi là **hạn chế** ${T|}_W$.

Ảnh $R(T)$ và kernel $N(T)$ là không gian bất biến của $T$.

Không gian riêng cũng là không gian bất biến.

## Phân rã sơ cấp

Theo định lý cơ bản đại số, đa thức tối tiểu:

$$
m_A(\lambda)={(\lambda-\lambda_1)}^{r_1}\cdots{(\lambda-\lambda_S)}^{r_S}
$$

Xét các không gian con bất biến:

$$
W_i=N({(\lambda_i I-A)}^{r_i})
$$

Định lý: $\dim W_i$ bằng bội đại số của $\lambda_i$.

Bội đại số là số mũ trong đa thức đặc trưng, bội hình học là số chiều không gian riêng. $W_i$ đạt đến bội đại số sau lũy thừa $r_i$.

Gọi $T$ là biến đổi tương ứng với $A$, và $T_i={T|}_{W_i}$. Khi đó đa thức tối tiểu của $T_i$ là $(x-\lambda_i)^{r_i}$.

Định lý: $V$ phân rã thành tổng trực tiếp các không gian bất biến $W_i$:

$$
V=W_1\oplus W_2\oplus\cdots\oplus W_S
$$

Tương ứng ma trận là dạng khối chéo:

$$
\operatorname{diag}\{A_1,A_2,\cdots,A_S\}
$$

trong đó $A_i$ là ma trận của $T_i$.

## Ma trận khả chéo hóa

Ma trận vuông $A$ khả chéo hóa nếu tương tự một ma trận đường chéo.

-   Tổng, tích, nghịch đảo (nếu có) của ma trận đường chéo vẫn đường chéo, đường chéo là trị riêng.
-   $T$ khả chéo hóa ⇔ có cơ sở để ma trận là đường chéo.

Định lý: với trị riêng phân biệt $\lambda_1,\cdots,\lambda_m$, các mệnh đề sau tương đương:

-   $A$ khả chéo hóa.
-   $A$ có $n$ vectơ riêng độc lập.
-   $\sum \operatorname{dim} E(\lambda_i)=n$.

Do đó, $A$ khả chéo hóa ⇔ bội đại số = bội hình học cho mọi trị riêng.

Hệ quả: nếu $A$ có $n$ trị riêng phân biệt thì khả chéo hóa; ngược lại không nhất thiết.

Định lý: $A$ khả chéo hóa ⇔ đa thức tối tiểu không có nghiệm bội.

Tương tự, ma trận tương tự giữ quan hệ phụ thuộc giữa các vectơ riêng.

Vectơ riêng có thể không thuộc $\mathbb{R}$ hoặc không đủ $n$ vectơ độc lập.

Với trị riêng bội, không gian riêng có nhiều vectơ; thường chọn một cơ sở, rồi trực giao hóa và chuẩn hóa.

Vectơ riêng không nhất thiết trực giao; các trị riêng khác nhau có thể không trực giao. Trực giao hóa chỉ áp dụng cho vectơ riêng của cùng một trị riêng; chuẩn hóa áp dụng cho mọi vectơ riêng.

## Ma trận nilpotent

Biến đổi $T$ là nilpotent nếu tồn tại $r$ sao cho $T^r$ là biến đổi 0.

Ma trận $N$ gọi nilpotent nếu $N^r=0$.

Thường lấy $r$ nhỏ nhất, khi đó đa thức tối tiểu là $x^r$. Tồn tại $\xi_0$ sao cho:

-   $$
    T^r(\xi_0)=0
    $$
-   $$
    T^{r-1}(\xi_0)\neq 0
    $$

### Không gian tuần hoàn

Định lý: nếu tồn tại $s$ sao cho $T^s(\xi)=0$ và $T^{s-1}(\xi)\ne 0$ thì các vectơ $\xi,T(\xi),\cdots,T^{s-1}(\xi)$ độc lập.

Định nghĩa: nếu tồn tại $\xi_0$ và $r$ sao cho:

-   $\xi_0,T(\xi_0),\cdots,T^{r-1}(\xi_0)$ là cơ sở của $W$;
-   $T^r(\xi_0)=0$,

thì $W$ là **không gian tuần hoàn** của $T$. $\xi_0$ là vectơ sinh, và dãy trên là cơ sở tuần hoàn.

Rõ ràng $W$ bất biến và mọi $\xi\in W$ có $T^r(\xi)=0$.

### Jordan block nilpotent

Nếu $W$ là tuần hoàn, thì ${T|}_W$ là nilpotent và trong cơ sở tuần hoàn đảo ngược $T^{r-1}(\xi_0),\dots,\xi_0$, ma trận là:

$$
N_r=\begin{pmatrix}
0 & 1 & 0 & \cdots & 0 & 0\\
0 & 0 & 1 & \cdots & 0 & 0\\
0 & 0 & 0 & \cdots & 0 & 0\\
\vdots & \vdots & \vdots &   & \vdots& \vdots\\
0 & 0 & 0 & \cdots & 0 & 1\\
0 & 0 & 0 & \cdots & 0 & 0\\
\end{pmatrix}
$$

Gọi là Jordan block nilpotent bậc $r$.

Với biến đổi nilpotent $T$ trên $V$ $n$ chiều, dãy $r_1\ge\cdots\ge r_S$ xác định từ phân rã không gian tuần hoàn gọi là **chỉ số bất biến** của $T$.

Tương tự cho ma trận nilpotent.

Ma trận nilpotent tương tự dạng chuẩn gồm các Jordan block nilpotent. Đây là phần nilpotent trong dạng Jordan.

### Một số định lý

1.  Với nilpotent $T$ và đa thức $h(x)=a_0+\cdots+a_mx^m$, thì $h(T)$ khả nghịch ⇔ $a_0\ne 0$. Khi đó $h(T)^{-1}$ cũng là đa thức của $T$.

2.  Với nilpotent $T$, $W$ là không gian tuần hoàn $r$ chiều, $\xi\in W$. Nếu tồn tại $k$ sao cho $T^{r-k}(\xi)=0$, thì tồn tại $\eta\in W$ sao cho $\xi=T^k(\eta)$.

3.  Với nilpotent $T$ trên $V$ $n$ chiều, đa thức tối tiểu $x^r$, nếu $W_1$ là không gian tuần hoàn $r$ chiều thì tồn tại $W_2$ sao cho:

    $$
    V=W_1\oplus W_2
    $$

    và $W_2$ bất biến.

4.  Với nilpotent $T$, $V$ phân rã thành tổng trực tiếp các không gian tuần hoàn:

    $$
    V=W_1\oplus W_2\oplus\cdots\oplus W_S
    $$

5.  Mọi ma trận nilpotent bậc $n$ tương tự ma trận:

    $$
    N=\begin{pmatrix}
    N_{r_1} &   &   & 0\\
      & N_{r_2} &   &  \\
      &   & \cdots &  \\
    0 &   &   & N_{r_S}\\
    \end{pmatrix}
    $$

6.  Nếu sắp $r_1\ge\cdots\ge r_S$, thì phân rã tuần hoàn của $V$ là duy nhất.
