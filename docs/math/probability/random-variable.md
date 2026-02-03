## Các khái niệm liên quan

### Biến ngẫu nhiên

Cho không gian xác suất $(\Omega, \mathcal{F}, P)$, một hàm $X : \Omega \to \mathbb{R}$ xác định trên không gian mẫu $\Omega$ nếu thỏa: với mọi $t \in \mathbb{R}$ đều có

$$
\{ \omega \in \Omega : X(\omega) \le t \} \in \mathcal{F}
$$

thì gọi $X$ là **biến ngẫu nhiên**.

### Hàm chỉ thị

Với biến cố $A$ trên không gian mẫu $\Omega$, định nghĩa biến ngẫu nhiên

$$
I_A(\omega) = \begin{cases}
    1, & \omega \in A \\
    0, & \omega \notin A
\end{cases}
$$

gọi $I_A$ là **hàm chỉ thị** của biến cố $A$.

### Hàm phân phối

Với biến ngẫu nhiên $X$, hàm

$$
F(x) = P( X \leq x )
$$

được gọi là **hàm phân phối** của $X$. Ký hiệu $X \sim F(x)$.

Hàm phân phối có các tính chất:

-   **Tính liên tục phải**: $F(x) = F(x + 0)$
-   **Tính đơn điệu**: đơn điệu tăng (không nhất thiết chặt) trên $\mathbb{R}$
-   $F(-\infty) = 0$,$F(+\infty) = 1$

Đồng thời có thể chứng minh mọi hàm thỏa các điều kiện trên đều là hàm phân phối của một biến ngẫu nhiên. Do đó, hàm phân phối và biến ngẫu nhiên tương ứng một-một.

## Phân loại biến ngẫu nhiên

Theo miền giá trị (theo định nghĩa, biến ngẫu nhiên là một hàm), biến ngẫu nhiên chia thành hai loại: **rời rạc** và **liên tục**.

### Biến ngẫu nhiên rời rạc

Giả sử $X$ rời rạc, các giá trị có thể là $x_1, x_2, \cdots$, ta có thể mô tả bằng các đẳng thức dạng $P\{ X = x_i \} = p_i$. Đây chính là **dãy phân phối** trong sách phổ thông.

### Biến ngẫu nhiên liên tục

Giả sử $X$ liên tục, xét $P\{ X = x \}$ thường là vô nghĩa (vì xác suất này rất có thể bằng $0$).

??? note "Vì sao xác suất “rất có thể” là $0$"
    Xét biến ngẫu nhiên $X$: lấy giá trị $0$ với xác suất $\frac{1}{2}$, và với xác suất $\frac{1}{2}$ tuân theo phân phối đều trên khoảng mở $(0, 1)$. Rõ ràng $X$ thỏa định nghĩa biến liên tục.
    
    Với mọi $r \in (0, 1)$, ta có $P\{ X = r \} = 0$, nhưng lại có $P\{ X = 0 \} = \frac{1}{2}$.

Mặt khác, với $X \sim F(x)$, ta có

$$
P( l < x \leq l + \Delta x ) = F(l + \Delta x) - F(l)
$$

Một ý tưởng tự nhiên là dùng giới hạn $\lim\limits_{\Delta x \to 0^+} \frac{F(l + \Delta x) - F(l)}{\Delta x}$ để mô tả khả năng $X$ nhận giá trị $l$.

Biểu thức này chính là đạo hàm, nên ta tìm một hàm không âm $f(x)$ sao cho

$$
F(x) = \int_{-\infty}^{x} f(x) \text{d} x
$$

Nếu tồn tại $f(x)$ như vậy, gọi $f(x)$ là **hàm mật độ** của $X$.

## Tính độc lập của biến ngẫu nhiên

Đã bàn về tính độc lập của biến cố; do biến ngẫu nhiên và biến cố liên hệ chặt chẽ, ta có thể định nghĩa tương tự cho biến ngẫu nhiên.

### Định nghĩa

Nếu biến ngẫu nhiên $X, Y$ thỏa với mọi $x, y \in \mathbb{R}$:

$$
P( X \leq x, Y \leq y ) = P( X \leq x ) P( Y \leq y )
$$

thì gọi $X, Y$ **độc lập**.

??? note "Note"
    Một số bạn có thể thấy sách phổ thông định nghĩa độc lập qua xác suất dạng $P(X = \alpha)$, nhưng với biến liên tục thì xác suất tại một điểm thường là $0$, nên trong trường hợp tổng quát dùng hàm phân phối là hợp lý hơn.

### Tính chất

Nếu $X$,$Y$ độc lập thì với mọi hàm $f, g$, các biến $f(X)$ và $g(Y)$ độc lập.

??? warning "Chú ý"
    Đôi khi ta xét hàm $f(X, Y)$ (như $XY^2$) của hai biến độc lập $X$,$Y$.
    
    Dù $X$ và $Y$ độc lập, không thể suy đoán rằng với một giá trị $y$ bất kỳ, $f(X, y)$ và $f(X, Y)$ có cùng phân phối.
