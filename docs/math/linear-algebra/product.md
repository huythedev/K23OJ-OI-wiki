Bài này giới thiệu các phép toán cơ bản giữa các vectơ.

Trước khi vào bài, lưu ý về dịch thuật: trong toán học và vật lý, “inner product” và “outer product” có nhiều cách dịch.

Trong vật lý, thường dịch là “标积” và “矢积”, nhấn mạnh kết quả là vô hướng và vectơ. Sách phổ thông gọi là “数量积” và “向量积”.

Trong toán học, thường dịch là “内积” và “外积”, dịch trực tiếp. “点乘” và “叉乘” là cách gọi theo ký hiệu, rất phổ biến.

Trong phép “dot”, thường lược dấu chấm; trong đại số tuyến tính còn coi như nhân ma trận.

## Tích vô hướng

Khái niệm tích vô hướng **áp dụng cho mọi số chiều**.

### Định nghĩa

Có nhiều cách định nghĩa tương đương.

#### Định nghĩa hình học

Trong $\mathbf{R}^n$, với $\boldsymbol{a}, \boldsymbol{b}$ có góc $\theta$:

$$
\boldsymbol{a} \cdot \boldsymbol{b} = |\boldsymbol{a}| |\boldsymbol{b}| \cos \theta
$$

gọi là **tích vô hướng** (dot/quantity product). $|\boldsymbol{b}|\cos \theta$ là hình chiếu của $\boldsymbol{b}$ lên hướng $\boldsymbol{a}$. Ý nghĩa: $\boldsymbol{a}\cdot\boldsymbol{b}$ bằng tích độ dài $\boldsymbol a$ và hình chiếu của $\boldsymbol b$ lên $\boldsymbol a$.

#### Định nghĩa đại số

Trong $\mathbf{R}^n$, với $\boldsymbol{a}=(a_1,\dots,a_n)$, $\boldsymbol{b}=(b_1,\dots,b_n)$:

$$
\boldsymbol{a} \cdot \boldsymbol{b} = \sum_{i = 1}^{n} a_i b_i
$$

là tích vô hướng. Trong không gian Euclid, hai định nghĩa tương đương, và định nghĩa đại số thuận tiện hơn.

Trong trường hợp không gây nhầm lẫn, dấu chấm có thể bỏ. Nếu có mũ $2$ ở góc phải của vectơ, nghĩa là tích vô hướng với chính nó, tức **bình phương độ dài**. Không được hiểu là “bình phương vectơ”. Tương tự, bình phương của bình phương độ dài không viết mũ $4$, mà coi $2$ như một khối.

### Tính chất

Tích vô hướng là một vô hướng, và là song tuyến tính:

$$
\begin{aligned}
(\boldsymbol{a} + \boldsymbol{b}) \cdot \boldsymbol{c} &= \boldsymbol{a} \cdot \boldsymbol{c} + \boldsymbol{b} \cdot \boldsymbol{c} \\
\boldsymbol{a} \cdot (\boldsymbol{b} + \boldsymbol{c}) &= \boldsymbol{a} \cdot \boldsymbol{b} + \boldsymbol{a} \cdot \boldsymbol{c} \\
(\lambda \boldsymbol{a}) \cdot \boldsymbol{b} &= \lambda (\boldsymbol{a} \cdot \boldsymbol{b}) \\
\boldsymbol{a} \cdot (\lambda \boldsymbol{b}) &= \lambda (\boldsymbol{a} \cdot \boldsymbol{b})
\end{aligned}
$$

Tích vô hướng giao hoán:

$$
\boldsymbol{a} \cdot \boldsymbol{b} = \boldsymbol{b} \cdot \boldsymbol{a}
$$

### Ứng dụng

1.  Kiểm tra vuông góc:

    $$
    \boldsymbol{a} \perp \boldsymbol{b} \iff \boldsymbol{a} \cdot \boldsymbol{b} = 0
    $$

    Nếu lấy “tích vô hướng bằng 0” làm định nghĩa vuông góc, thì vectơ không vuông góc với mọi vectơ.

2.  Kiểm tra cùng phương:

    $$
    \exists\lambda \in \mathbf{R} (\boldsymbol{a} = \lambda \boldsymbol{b}) \iff |\boldsymbol{a} \cdot \boldsymbol{b}| = |\boldsymbol{a}| |\boldsymbol{b}|
    $$

3.  Tính độ dài:

    $$
    |\boldsymbol a| = \sqrt{\boldsymbol{a} \cdot \boldsymbol{a}}
    $$

4.  Tính góc:

    $$
    \theta = \arccos \frac{\boldsymbol{a} \cdot \boldsymbol{b}}{|\boldsymbol a| |\boldsymbol b|}
    $$

## Định thức cấp 2 và 3

Định thức cấp 2 và 3 là trường hợp đặc biệt. Trong giải tích (trường), công thức Green dùng định thức cấp 2, Gauss dùng tích vô hướng, Stokes dùng định thức cấp 3.

Định thức cấp 2:

$$
\begin{vmatrix}
    a & b \\
    c & d
\end{vmatrix}=ad-bc
$$

Định thức cấp 3:

$$
\begin{vmatrix}
    a & b & c \\
    d & e & f \\
    g & h & i
\end{vmatrix}=aei+dhc+gbf-ahf-dbi-gec
$$

Có thể nhớ bằng “quy tắc đường chéo”, nhưng chỉ dùng được cho cấp 2 và 3. Với cấp 4 trở lên, số hạng và dấu sẽ sai.

## Tích hữu hướng (ngoại tích)

Ngoại tích là **phép toán riêng của vectơ 3D**.

Trong vật lý, vectơ 3D thường liên hệ vị trí, nên dùng chữ in đậm; còn vectơ 4D tương đối tính dùng ký hiệu đặc biệt. Trong đại số tuyến tính, vectơ thường in đậm và khi viết tay có thể bỏ ký hiệu.

### Định nghĩa

#### Định nghĩa hình học

Trong $\mathbf{R}^3$, ngoại tích $\boldsymbol{a}\times \boldsymbol{b}$ là một vectơ, với:

1.  $|\boldsymbol{a} \times \boldsymbol{b}| = |\boldsymbol{a}| |\boldsymbol{b}| \sin \langle \boldsymbol{a}, \boldsymbol{b} \rangle$;
2.  Vuông góc với $\boldsymbol{a},\boldsymbol{b}$, và hướng theo quy tắc bàn tay phải.

Ý nghĩa: $|\boldsymbol{a} \times \boldsymbol{b}|$ là **diện tích hình bình hành** có cạnh là $\boldsymbol{a},\boldsymbol{b}$.

#### Định nghĩa đại số

Với $\boldsymbol{a}=(x_1,y_1,z_1)$, $\boldsymbol{b}=(x_2,y_2,z_2)$:

$$
\begin{vmatrix}
    \boldsymbol{i} & \boldsymbol{j} & \boldsymbol{k} \\
    x_1 & y_1 & z_1  \\
    x_2 & y_2 & z_2
\end{vmatrix}
$$

với $\boldsymbol{i},\boldsymbol{j},\boldsymbol{k}$ là vectơ đơn vị theo trục $x,y,z$.

Khai triển:

$$
\begin{aligned}
\boldsymbol{c} &= \boldsymbol{a} \times \boldsymbol{b} \\
&= (y_1z_2 - y_2z_1)\boldsymbol{i} + (z_1x_2 - z_2x_1)\boldsymbol{j} + (x_1y_2 - x_2y_1)\boldsymbol{k} \\
&= (y_1z_2 - y_2z_1, z_1x_2 - z_2x_1, x_1y_2 - x_2y_1)
\end{aligned}
$$

### Tính chất

1.  Ngoại tích là song tuyến tính:

    $$
    \begin{aligned}
    (\boldsymbol{a} + \boldsymbol{b}) \times \boldsymbol{c} &= \boldsymbol{a} \times \boldsymbol{c} + \boldsymbol{b} \times \boldsymbol{c} \\
    \boldsymbol{a} \times (\boldsymbol{b} + \boldsymbol{c}) &= \boldsymbol{a} \times \boldsymbol{b} + \boldsymbol{a} \times \boldsymbol{c} \\
    (\lambda \boldsymbol{a}) \times \boldsymbol{b} &= \lambda (\boldsymbol{a} \times \boldsymbol{b}) \\
    \boldsymbol{a} \times (\lambda \boldsymbol{b}) &= \lambda (\boldsymbol{a} \times \boldsymbol{b})
    \end{aligned}
    $$

2.  Phản giao hoán:

    $$
    \boldsymbol a \times \boldsymbol b=-\boldsymbol b \times \boldsymbol a
    $$

3.  Theo định nghĩa hình học:

    $$
    \begin{aligned}
    |\boldsymbol a \times \boldsymbol b| &= |\boldsymbol a| |\boldsymbol b| \sin \langle \boldsymbol a, \boldsymbol b \rangle \\
    \boldsymbol a \cdot \boldsymbol b &= |\boldsymbol a| |\boldsymbol b| \cos \langle \boldsymbol a, \boldsymbol b\rangle
    \end{aligned}
    $$

    Suy ra:

    $$
    (\boldsymbol a\times \boldsymbol b) \cdot (\boldsymbol a\times \boldsymbol b) = |\boldsymbol a|^2 |\boldsymbol b|^2-{(\boldsymbol a \cdot \boldsymbol b)}^2
    $$

4.  Thỏa đẳng thức Jacobi:

    $$
    \boldsymbol a \times (\boldsymbol b \times \boldsymbol c) + \boldsymbol b \times (\boldsymbol c \times \boldsymbol a) + \boldsymbol c \times (\boldsymbol a \times \boldsymbol b) = \boldsymbol 0
    $$

### Ứng dụng

1.  Kiểm tra cùng phương:

    $$
    \exists\lambda \in \mathbf{R} (\boldsymbol{a} = \lambda \boldsymbol{b}) \iff \boldsymbol{a} \times \boldsymbol{b} = \boldsymbol{0}
    $$

2.  Tính diện tích hình bình hành:

    $$
    S \langle \boldsymbol a, \boldsymbol b \rangle = |\boldsymbol a \times \boldsymbol b|
    $$

#### Trường hợp 2D

Trong 2D không có ngoại tích, nhưng diện tích hình bình hành vẫn tính được:

Với $\boldsymbol{a}=(m,n)$, $\boldsymbol{b}=(p,q)$, nhúng vào 3D thành $(m,n,0)$ và $(p,q,0)$. Ngoại tích là $(0,0,mq-np)$, nên diện tích là $|mq-np|$, tương đương giá trị tuyệt đối định thức cấp 2.

Dựa vào dấu của tọa độ $z$, suy ra hướng của $\boldsymbol b$ so với $\boldsymbol a$; quy ước **thuận âm, nghịch dương**.

## Hỗn tích

Hỗn tích là **phép toán riêng của 3D**.

### Định nghĩa

Với $\boldsymbol a, \boldsymbol b, \boldsymbol c$ trong 3D, $(\boldsymbol a \times \boldsymbol b) \cdot \boldsymbol c$ gọi là hỗn tích, ký hiệu $[\boldsymbol a \boldsymbol b \boldsymbol c]$ hoặc $(\boldsymbol a, \boldsymbol b, \boldsymbol c)$ hoặc $\det(\boldsymbol a, \boldsymbol b, \boldsymbol c)$. Giá trị tuyệt đối là thể tích hình hộp.

Hỗn tích có thể viết bằng định thức cấp 3:

$$
\begin{aligned}
(\boldsymbol a \times \boldsymbol b) \cdot \boldsymbol c &= \det(\boldsymbol a, \boldsymbol b, \boldsymbol c) \\
&= \begin{vmatrix}
    a_x & b_x & c_x \\
    a_y & b_y & c_y \\
    a_z & b_z & c_z
\end{vmatrix} \\
&= a_x b_y c_z + a_y b_z c_x + a_z b_x c_y - a_z b_y c_x -a _y b_x c_z - a_x b_z c_y
\end{aligned}
$$

### Tính chất

1.  Hỗn tích tuyến tính theo từng biến:

    $$
    \begin{aligned}
    \det(\lambda\boldsymbol{u} + \mu\boldsymbol{v}, \boldsymbol{b}, \boldsymbol{c}) &= \lambda\det(\boldsymbol{u}, \boldsymbol{b}, \boldsymbol{c}) + \mu\det(\boldsymbol{v}, \boldsymbol{b}, \boldsymbol{c}) \\
    \det(\boldsymbol{a}, \lambda\boldsymbol{u} + \mu\boldsymbol{v}, \boldsymbol{c}) &= \lambda\det(\boldsymbol{a}, \boldsymbol{u}, \boldsymbol{c}) + \mu\det(\boldsymbol{a}, \boldsymbol{v}, \boldsymbol{c}) \\
    \det(\boldsymbol{a}, \boldsymbol{b}, \lambda\boldsymbol{u} + \mu\boldsymbol{v}) &= \lambda\det(\boldsymbol{a}, \boldsymbol{b}, \boldsymbol{u}) + \mu\det(\boldsymbol{a}, \boldsymbol{b}, \boldsymbol{v})
    \end{aligned}
    $$

2.  Phản đối xứng:

    $$
    \det(\boldsymbol a, \boldsymbol b, \boldsymbol c) = \det(\boldsymbol b, \boldsymbol c, \boldsymbol a) = \det(\boldsymbol c, \boldsymbol a, \boldsymbol b) = -\det(\boldsymbol b, \boldsymbol a, \boldsymbol c) = -\det(\boldsymbol a, \boldsymbol c, \boldsymbol b)= -\det(\boldsymbol c, \boldsymbol b, \boldsymbol a)
    $$

    Suy ra:

    $$
    (\boldsymbol a \times \boldsymbol b) \cdot \boldsymbol c = \boldsymbol a \cdot (\boldsymbol b \times \boldsymbol c)
    $$

### Ứng dụng

1.  Thể tích tứ diện $ABCD$:

    $$
    V=\frac{1}{6}\left|\det(\overrightarrow{AB}, \overrightarrow{AC}, \overrightarrow{AD})\right|
    $$

2.  Kiểm tra đồng phẳng:

    $\boldsymbol a,\boldsymbol b,\boldsymbol c$ đồng phẳng $\iff \det(\boldsymbol a, \boldsymbol b, \boldsymbol c)=0$.

3.  Xác định tính tay (handedness):

    -   $\det(\boldsymbol a, \boldsymbol b, \boldsymbol c) < 0$ ⇔ tạo hệ tay trái;
    -   $\det(\boldsymbol a, \boldsymbol b, \boldsymbol c) > 0$ ⇔ tạo hệ tay phải.

## Nhị trùng ngoại tích

Hỗn tích là sự kết hợp giữa nội và ngoại tích. Ngoại tích của ngoại tích có kết luận gì?

Chứng minh bổ đề:

$$
(\boldsymbol a \times \boldsymbol b)\times \boldsymbol a = (\boldsymbol a \cdot \boldsymbol a) \boldsymbol b - (\boldsymbol a \cdot \boldsymbol b) \boldsymbol a
$$

Chứng minh: $\boldsymbol a \times \boldsymbol b$ vuông góc với $\boldsymbol a,\boldsymbol b$, nên vế trái vuông góc với $\boldsymbol a \times \boldsymbol b$, do đó đồng phẳng với $\boldsymbol a,\boldsymbol b$:

$$
(\boldsymbol a \times \boldsymbol b)\times \boldsymbol a = \lambda \boldsymbol a + \mu \boldsymbol b
$$

Lấy tích vô hướng hai vế với $\boldsymbol a,\boldsymbol b$:

$$
\begin{aligned}
\lambda (\boldsymbol a \cdot \boldsymbol a)+\mu (\boldsymbol a \cdot \boldsymbol b) &= 0 \\
\lambda (\boldsymbol a \cdot \boldsymbol b) + \mu (\boldsymbol b \cdot \boldsymbol b) &= (\boldsymbol a \times \boldsymbol b) \cdot (\boldsymbol a \times \boldsymbol b)
\end{aligned}
$$

Dùng:

$$
(\boldsymbol a \times \boldsymbol b) \cdot (\boldsymbol a \times \boldsymbol b) = |\boldsymbol a|^2|\boldsymbol b|^2-(\boldsymbol a \cdot \boldsymbol b)^2
$$

Giải ra:

$$
\begin{aligned}
\lambda &= -\boldsymbol a \cdot \boldsymbol b \\
\mu &= \boldsymbol a \cdot \boldsymbol a
\end{aligned}
$$

Chứng minh xong.

Do $\boldsymbol a \times \boldsymbol b$ luôn cho kết quả đồng phẳng với $\boldsymbol a,\boldsymbol b$, ta có công thức **nhị trùng ngoại tích**:

$$
(\boldsymbol a\times \boldsymbol b)\times \boldsymbol c=(\boldsymbol a \cdot \boldsymbol c)\boldsymbol b - (\boldsymbol b \cdot \boldsymbol c)\boldsymbol a
$$

Chứng minh: giả sử $\boldsymbol a,\boldsymbol b,\boldsymbol c$ không-zero và không đồng phương. Viết:

$$
\boldsymbol c = \alpha \boldsymbol a + \beta \boldsymbol b + \gamma(\boldsymbol a \times \boldsymbol b)
$$

Suy ra:

$$
\begin{aligned}
(\boldsymbol a \times \boldsymbol b) \times \boldsymbol c
&= \alpha(\boldsymbol a \times \boldsymbol b) \times \boldsymbol a + \beta(\boldsymbol a \times \boldsymbol b) \times \boldsymbol b
\end{aligned}
$$

Áp dụng bổ đề:

$$
\begin{aligned}
(\boldsymbol a \times \boldsymbol b) \times \boldsymbol a &=(\boldsymbol a \cdot \boldsymbol a) \boldsymbol b - (\boldsymbol a \cdot \boldsymbol b) \boldsymbol a \\
(\boldsymbol a \times \boldsymbol b) \times \boldsymbol b
&= -(\boldsymbol b \times \boldsymbol a) \times \boldsymbol b \\
&= -(\boldsymbol b \cdot \boldsymbol b)\boldsymbol a+(\boldsymbol a \cdot \boldsymbol b) \boldsymbol b
\end{aligned}
$$

Nên:

$$
\begin{aligned}
(\boldsymbol a \times \boldsymbol b) \times \boldsymbol c
&=(\boldsymbol a \cdot \boldsymbol c) \boldsymbol b - (\boldsymbol b \cdot \boldsymbol c) \boldsymbol a
\end{aligned}
$$

Từ phản giao hoán, có:

$$
\begin{aligned}
(\boldsymbol a\times \boldsymbol b)\times \boldsymbol c &=(\boldsymbol a \cdot \boldsymbol c)\boldsymbol b - (\boldsymbol b \cdot \boldsymbol c)\boldsymbol a \\
\boldsymbol a \times(\boldsymbol b \times \boldsymbol c) &= (\boldsymbol a \cdot \boldsymbol c)\boldsymbol b - (\boldsymbol a \cdot \boldsymbol b)\boldsymbol c
\end{aligned}
$$

Nhị trùng ngoại tích phụ thuộc chặt vào thứ tự.

Từ hỗn tích và nhị trùng ngoại tích, suy ra đẳng thức Lagrange:

$$
(\boldsymbol a \times \boldsymbol b) \cdot (\boldsymbol c \times \boldsymbol d)=(\boldsymbol a \cdot \boldsymbol c)(\boldsymbol b \cdot \boldsymbol d)-(\boldsymbol a \cdot \boldsymbol d)(\boldsymbol b \cdot \boldsymbol c)
$$

Chứng minh:

$$
\begin{aligned}
(\boldsymbol a \times \boldsymbol b) \cdot (\boldsymbol c \times \boldsymbol d)
&= ((\boldsymbol a \times \boldsymbol b)\times \boldsymbol c)\cdot \boldsymbol d \\
&= (\boldsymbol b(\boldsymbol a \cdot \boldsymbol c)- \boldsymbol a(\boldsymbol b \cdot \boldsymbol c))\cdot \boldsymbol d \\
&= (\boldsymbol a \cdot \boldsymbol c)(\boldsymbol b \cdot \boldsymbol d) - (\boldsymbol a \cdot \boldsymbol d)(\boldsymbol b \cdot \boldsymbol c)
\end{aligned}
$$

Do đó đẳng thức

$$
(\boldsymbol a \times \boldsymbol b) \cdot (\boldsymbol a \times \boldsymbol b) = |\boldsymbol a|^2|\boldsymbol b|^2 - (\boldsymbol a \cdot \boldsymbol b)^2
$$

là trường hợp riêng của Lagrange.
