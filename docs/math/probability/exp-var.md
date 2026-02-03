Bài viết này giới thiệu kỳ vọng, phương sai và các đặc trưng số của biến ngẫu nhiên.

## Kỳ vọng

### Định nghĩa

#### Biến ngẫu nhiên rời rạc

Gọi phân phối của biến rời rạc $X$ là $p_i = P\{ X = x_i \}$. Nếu tổng

$$
\sum x_i p_i
$$

hội tụ tuyệt đối, thì gọi giá trị đó là **kỳ vọng** của $X$, ký hiệu $EX$.

#### Biến ngẫu nhiên liên tục

Gọi mật độ của biến liên tục $X$ là $f(x)$. Nếu tích phân

$$
\int_{\mathbb{R}} xf(x) \text{d} x
$$

hội tụ tuyệt đối, thì gọi giá trị đó là **kỳ vọng** của $X$, ký hiệu $EX$.

#### Định nghĩa thống nhất

Gọi hàm phân phối của $X$ là $F(x)$. Nếu [tích phân Stieltjes](https://en.wikipedia.org/wiki/Riemann%E2%80%93Stieltjes_integral)

$$
\int_{\mathbb{R}} x \text{d} F(x)
$$

hội tụ tuyệt đối, thì giá trị này là **kỳ vọng** của $X$, ký hiệu $EX$.

??? example "Ví dụ kỳ vọng không tồn tại"
    Xét biến rời rạc $X$ có phân phối
    
    $$
    P\left\{ X = (-1)^k \frac{2^k}{k} \right\} = \frac{1}{2^k}, \quad k = 1, 2, \cdots
    $$
    
    Dù $\sum x_i p_i$ hội tụ về $- \ln 2$, nhưng không hội tụ tuyệt đối nên kỳ vọng của $X$ không tồn tại.
    
    Xét tiếp biến liên tục $Y$ có mật độ
    
    $$
    f(y) = \frac{1}{\pi} \cdot \frac{1}{1 + y^2}, \quad y \in (-\infty, +\infty)
    $$
    
    Dễ thấy kỳ vọng của $Y$ cũng không tồn tại.

### Tính chất của kỳ vọng

#### Tính tuyến tính

Nếu kỳ vọng của $X, Y$ tồn tại thì

-   Với mọi $a, b$ thực, $E(aX + b) = a \cdot EX + b$.
-   $E(X + Y) = EX + EY$.

#### Kỳ vọng của tích

Nếu kỳ vọng của $X$,$Y$ tồn tại và $X$,$Y$ độc lập thì

$$
E(XY) = EX \cdot EY
$$

Lưu ý: tính độc lập **không** phải điều kiện cần.

??? example "Phản ví dụ"
    Xét $X$ phân phối đều trên $[-1, 1]$, và $Y = X^2$.

### Chuyển đổi giữa kỳ vọng và xác suất

Với biến cố $A$, xét hàm chỉ thị $I_A$:

$$
I_A(\omega) = \begin{cases}
    1, & \omega \in A \\
    0, & \omega \notin A
\end{cases}
$$

Theo định nghĩa, $EI_A = P(A)$. Chuyển đổi này rất phổ biến trong ứng dụng.

??? example "Ví dụ"
    Với dãy dài $n$ là $\{ a_i \}$, trong đó $a_k$ nhận giá trị $k$ với xác suất $p_k$, nhận $0$ với xác suất $1 - p_k$. Tính kỳ vọng $S = \sum_{i=1}^{n} a_i$.
    
    Dùng định nghĩa trực tiếp cần phân phối của $S$, khá rườm rà, nên bỏ qua.
    
    Gọi $I_k$ là chỉ thị của biến cố $a_k = k$, khi đó
    
    $$
    S = \sum_{k=1}^{n} k \cdot I_k
    $$
    
    Suy ra
    
    $$
    ES = E \left( \sum_{k=1}^{n} k \cdot I_k \right) = \sum_{k=1}^{n} k \cdot E[I_k] = \sum_{k=1}^{n} k \cdot p_k
    $$

## Phân phối có điều kiện và kỳ vọng có điều kiện

Ta đã xét xác suất có điều kiện, tương tự có thể định nghĩa kỳ vọng có điều kiện.

### Định nghĩa

Với hai biến $X$,$Y$, trong điều kiện biết $Y = y$, phân phối (mật độ) của $X$ gọi là **phân phối có điều kiện (mật độ có điều kiện)**, ký hiệu

$$
P( X = x_i | Y = y ) \qquad f_{X|Y}(x|y)
$$

Kỳ vọng của $X$ trong điều kiện này gọi là **kỳ vọng có điều kiện**, ký hiệu $E[X|Y=y]$.

### Tính chất của kỳ vọng có điều kiện

Các tính chất có thể suy ra từ xác suất có điều kiện, không trình bày chi tiết.

Đáng chú ý, $E[X | Y]$ thường là hàm của $Y$ và không tuyến tính. Tuy nhiên luôn có

$$
E[E[X|Y]] = EX
$$

gọi là **công thức kỳ vọng toàn phần**.

### Ứng dụng

???+ example "[HDU 5984 Pocky](https://acm.hdu.edu.cn/showproblem.php?pid=5984)"
    Có một que Pocky dài $L$, mỗi lần bẻ ngẫu nhiên thành hai đoạn. Nếu đoạn bên phải dài không quá $d$ thì dừng, nếu không thì lặp lại với đoạn bên phải. Hỏi kỳ vọng số lần lặp.

??? note "Lời giải"
    Gọi $f(x)$ là kỳ vọng số lần khi độ dài là $x$. Trường hợp $x \leq d$ là hiển nhiên.
    
    Khi $x > d$, giả sử vị trí bẻ cách đầu phải là $k$, thì $k \sim U[0, x]$. Kỳ vọng số lần:
    
    $$
    g(k) = \begin{cases}
        1, & k \leq d \\
        1 + f(k), & k > d
    \end{cases}
    $$
    
    Theo công thức kỳ vọng toàn phần
    
    $$
    f(x) = Eg(k) = 1 + \frac{1}{x} \cdot \int_{d}^{x} f(t) \text{d} t
    $$
    
    Giải phương trình tích phân và thế điều kiện đầu được
    
    $$
    f(x) = 1 + \ln \frac{x}{d}
    $$

## Phương sai

### Định nghĩa

Giả sử kỳ vọng $EX$ tồn tại và kỳ vọng

$$
E(X - EX)^2
$$

cũng tồn tại, thì gọi giá trị này là **phương sai** của $X$, ký hiệu $DX$ hoặc $Var(X)$．
Căn bậc hai của phương sai gọi là **độ lệch chuẩn**, ký hiệu $\sigma(X) = \sqrt{DX}$.

### Tính chất của phương sai

Nếu phương sai của $X$ tồn tại thì

-   Với mọi hằng số $a, b$, $D(aX + b) = a^2 \cdot DX$
-   $DX = E(X^2) - (EX)^2$

## Hiệp phương sai và hệ số tương quan

Nói chung, $D(X + Y) = DX + DY$ không đúng, nên đặt ra:

-   Phần chênh giữa $D(X + Y)$ và $DX + DY$ là gì?
-   Khi nào $D(X + Y)$ bằng $DX + DY$?

Với câu hỏi đầu, ta dùng hiệp phương sai.

### Định nghĩa hiệp phương sai

Với biến $X, Y$, gọi

$$
E((X - EX)(Y - EY))
$$

là **hiệp phương sai** của $X$ và $Y$, ký hiệu $\operatorname{Cov}(X, Y)$.

### Tính chất của hiệp phương sai

Với $X, Y, Z$, ta có

-   $\operatorname{Cov}(X, Y) = \operatorname{Cov}(Y, X)$
-   Với mọi hằng số $a, b$, $\operatorname{Cov}(aX + bY, Z) = a \cdot \operatorname{Cov}(X, Z) + b \cdot \operatorname{Cov}(Y, Z)$

Quan hệ với phương sai:

-   $DX = \operatorname{Cov}(X, X)$
-   $D(X + Y) = DX + 2 \operatorname{Cov}(X, Y) + DY$

??? note "Về hiệp phương sai"
    Có thể nhận thấy tính chất của hiệp phương sai giống với tích vô hướng.
    
    Theo quan điểm giải tích hàm, tập các biến ngẫu nhiên trên một không gian xác suất tạo thành một không gian tuyến tính, hiệp phương sai là một tích vô hướng, và độ lệch chuẩn là chuẩn sinh từ tích vô hướng đó.

Với câu hỏi thứ hai, $D(X + Y) = DX + DY$ khi và chỉ khi $\operatorname{Cov}(X, Y) = 0$. Một điều kiện đủ trực quan là $X$ và $Y$ độc lập, khi đó

$$
\operatorname{Cov}(X, Y) = E((X - EX)(Y - EY)) = E(X - EX) E(Y - EY) = 0
$$

Nhưng điều kiện này không đủ. Để mô tả quan hệ giữa $X$,$Y$ khi $\operatorname{Cov}(X, Y) = 0$, ta dùng hệ số tương quan.

### Hệ số tương quan

Với $X, Y$, gọi

$$
\frac{ \operatorname{Cov}(X, Y)}{ \sigma(X)\sigma(Y) }
$$

là **hệ số tương quan Pearson**, ký hiệu $\rho_{X,Y}$.

Hệ số Pearson mô tả mức độ liên hệ tuyến tính. $|\rho_{X,Y}|$ càng lớn thì liên hệ tuyến tính càng mạnh. Có $|\rho_{X,Y}| \leq 1$, và $|\rho_{X,Y}| = 1$ chỉ khi

-   Tồn tại $a$ thực và $b>0$ sao cho $P(X = a + bY) = 1$ thì $\rho_{X,Y} = 1$;
-   Tồn tại $a$ thực và $b<0$ sao cho $P(X = a + bY) = 1$ thì $\rho_{X,Y} = -1$.

Khi $\rho_{X,Y} = 0$ ta nói $X$ và $Y$ **không tương quan**, khi đó không có quan hệ tuyến tính.

??? note "“Không tương quan” và “độc lập”"
    Không tương quan chỉ nói không có liên hệ tuyến tính, không loại trừ các dạng liên hệ khác.
    
    Do đó, không tương quan là điều kiện **cần nhưng không đủ** để độc lập.

Kết luận cho câu hỏi thứ hai: $\operatorname{Cov}(X, Y) = 0$ khi và chỉ khi một trong $X$,$Y$ là hằng với xác suất $1$, hoặc $X, Y$ không tương quan.
