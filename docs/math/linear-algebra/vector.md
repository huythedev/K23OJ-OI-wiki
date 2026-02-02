Trong bài này, cần nói rõ về vấn đề dịch thuật. Do lịch sử, trong toán học và vật lý, từ “vector” được dịch khác nhau.

Trong vật lý, thường dịch là “矢量”, đối lập với “标量”. Trong toán học, thường dịch là “向量”. Các khác biệt tương tự còn có “本征/特征”, “幺正/酉”, v.v.

Ở **OI Wiki**, hướng tới khoa học máy tính và kỹ thuật, gần với toán học hơn, nên dùng từ “向量”.

## Định nghĩa và khái niệm liên quan

**Vectơ**: đại lượng có độ lớn và hướng. Trong toán học, đối tượng nghiên cứu là **vectơ tự do**, tức là chỉ cần không thay đổi độ lớn và hướng thì có thể tịnh tiến song song. Ký hiệu $\vec a$ hoặc $\boldsymbol{a}$.

**Đoạn thẳng có hướng**: đoạn thẳng có hướng gọi là đoạn thẳng có hướng. Nó có ba yếu tố: **điểm đầu, hướng, độ dài**; biết ba yếu tố thì điểm cuối xác định duy nhất. Thường dùng đoạn thẳng có hướng để biểu diễn vectơ.

**Độ dài (mô) của vectơ**: độ dài của $\overrightarrow{AB}$ gọi là độ lớn của vectơ, ký hiệu $|\overrightarrow{AB}|$ hoặc $|\boldsymbol{a}|$.

**Vectơ không**: vectơ có độ dài $0$. Hướng của vectơ không tùy ý. Ký hiệu $\vec 0$ hoặc $\boldsymbol{0}$.

**Vectơ đơn vị**: vectơ có độ dài $1$ gọi là vectơ đơn vị theo hướng đó, ký hiệu $\vec e$ hoặc $\boldsymbol{e}$.

**Vectơ song song**: hai vectơ **không-zero** có cùng hướng hoặc ngược hướng. Ký hiệu $\boldsymbol a\parallel \boldsymbol b$. Với nhiều vectơ song song, có thể tịnh tiến về cùng một đường thẳng, nên còn gọi là **vectơ đồng tuyến**.

**Vectơ bằng nhau**: độ dài bằng nhau và hướng giống nhau.

**Vectơ đối nhau**: độ dài bằng nhau và hướng ngược nhau.

**Góc giữa hai vectơ**: với hai vectơ không-zero $\boldsymbol a,\boldsymbol b$, lấy $\overrightarrow{OA}=\boldsymbol a,\overrightarrow{OB}=\boldsymbol b$, thì $\theta=\angle AOB$ là góc giữa $\boldsymbol a$ và $\boldsymbol b$. Ký hiệu $\langle \boldsymbol a,\boldsymbol b\rangle$. Khi $\theta=0$ cùng hướng, $\theta=\pi$ ngược hướng, $\theta=\frac{\pi}{2}$ vuông góc (ký hiệu $\boldsymbol a\perp \boldsymbol b$), và $\theta \in [0,\pi]$.

Lưu ý: vectơ phẳng có hướng nên không so sánh “lớn nhỏ” (chỉ so độ dài). Hai vectơ có thể bằng nhau.

## Phép toán tuyến tính trên vectơ

### Cộng trừ vectơ

Khi có một đại lượng, ta muốn định nghĩa phép toán. Phép toán trên vectơ có thể so sánh với phép toán số hoặc từ góc vật lý.

Ví dụ: một người đi từ $A$ qua $B$ đến $C$, độ dời là $\overrightarrow{AB}+\overrightarrow{BC}$, tương đương $\overrightarrow{AC}$.

Quy tắc hình bình hành (tổng lực) cũng là cộng vectơ.

Tóm lại:

1.  **Quy tắc tam giác**: nối đuôi theo thứ tự, tổng là vectơ từ đầu vectơ đầu tiên đến cuối vectơ cuối cùng;
2.  **Quy tắc hình bình hành**: hai vectơ **cùng gốc**, tổng là đường chéo của hình bình hành, hướng theo đường chéo.

Cộng vectơ có ý nghĩa hình học và thỏa **giao hoán** và **kết hợp**.

Vì trừ số là cộng với đối số, nên $\boldsymbol a-\boldsymbol b=\boldsymbol a+(-\boldsymbol b)$.

Khi hai vectơ cùng gốc, vectơ hiệu là đoạn thẳng có hướng từ “vectơ trừ” đến “vectơ bị trừ”. Đây là ý nghĩa hình học của phép trừ.

Nếu biết $A,B$, có thể tính $\overrightarrow{AB}=\overrightarrow{OB}-\overrightarrow{OA}$.

### Nhân vô hướng

Định nghĩa tích “số thực $\lambda$ với vectơ $\boldsymbol a$” là một vectơ, gọi là **nhân vô hướng**, ký hiệu $\lambda \boldsymbol a$, với:

1.  $|\lambda \boldsymbol a|=|\lambda||\boldsymbol a|$;
2.  $\lambda>0$ cùng hướng, $\lambda=0$ là $\boldsymbol 0$, $\lambda<0$ ngược hướng.

Có các tính chất:

$$
\begin{aligned}
\lambda(\mu \boldsymbol a)&=(\lambda \mu)\boldsymbol a\\
(\lambda+\mu)\boldsymbol a&=\lambda \boldsymbol a+\mu \boldsymbol a\\
\lambda(\boldsymbol a+\boldsymbol b)&=\lambda \boldsymbol a+\lambda \boldsymbol b
\end{aligned}
$$

Đặc biệt:

$$
\begin{gathered}
(-\lambda)\boldsymbol a=-(\lambda \boldsymbol a)=-\lambda(\boldsymbol a)\\
\lambda(\boldsymbol a-\boldsymbol b)=\lambda \boldsymbol a-\lambda \boldsymbol b
\end{gathered}
$$

### Kiểm tra hai vectơ cùng phương

Hai vectơ **không-zero** $\boldsymbol a$ và $\boldsymbol b$ cùng phương $\iff$ tồn tại duy nhất $\lambda$ sao cho $\boldsymbol b=\lambda \boldsymbol a$.

Chứng minh: theo định nghĩa nhân vô hướng, nếu tồn tại $\lambda$ sao cho $\boldsymbol b=\lambda \boldsymbol a$ thì $\boldsymbol a\parallel \boldsymbol b$.

Ngược lại, nếu $\boldsymbol a\parallel \boldsymbol b$, $\boldsymbol a\ne \boldsymbol 0$, và $|\boldsymbol b|=\mu |\boldsymbol a|$, thì cùng hướng có $\boldsymbol b=\mu \boldsymbol a$, ngược hướng có $\boldsymbol b=-\mu \boldsymbol a$.

Cộng, trừ, nhân vô hướng được gọi chung là phép toán tuyến tính.

## Định lý cơ bản về vectơ phẳng và biểu diễn tọa độ

### Định lý cơ bản về vectơ phẳng

Nếu hai vectơ $\boldsymbol{e_1},\boldsymbol{e_2}$ không cùng phương, thì tồn tại duy nhất cặp số thực $(x,y)$ sao cho mọi vectơ $\boldsymbol p$ cùng mặt phẳng thỏa $\mathbf p=x\boldsymbol{e_1}+y\boldsymbol{e_2}$.

Làm sao dùng ít vectơ để biểu diễn mọi vectơ phẳng?

Một vectơ không đủ (chỉ biểu diễn được một đường thẳng).

Thêm một vectơ không cùng phương, mọi vectơ phẳng đều phân giải được theo hai hướng đó.

Hai vectơ không cùng phương trong cùng mặt phẳng gọi là **cơ sở**. Nếu cơ sở vuông góc, ta có phân tích trực giao.

### Biểu diễn tọa độ của vectơ phẳng

Chọn vectơ đơn vị $i,j$ theo trục $x,y$ làm cơ sở. Theo định lý, mọi vectơ tương ứng một cặp $(x,y)$.

Cặp $(x,y)$ tương ứng một điểm trên hệ trục. Với $\overrightarrow{OP}=\boldsymbol p$, điểm $P(x,y)$ xác định duy nhất. Vì là vectơ tự do, có thể tịnh tiến gốc, nên mỗi vectơ có biểu diễn duy nhất bằng $(x,y)$.

## Phép toán tọa độ trong mặt phẳng

### Phép toán tuyến tính theo tọa độ

Với $\boldsymbol a=(m,n)$, $\boldsymbol b=(p,q)$:

$$
\begin{aligned}
\boldsymbol a+\boldsymbol b&=(m+p,n+q)\\
\boldsymbol a-\boldsymbol b&=(m-p,n-q)\\
k\boldsymbol a&=(km,kn)
\end{aligned}
$$

### Tọa độ vectơ giữa hai điểm

Với $A(a,b),B(c,d)$, $\overrightarrow{AB}=(c-a,d-b)$.

### Tịnh tiến một điểm

Muốn tịnh tiến điểm $P$ theo hướng và độ dài cho trước, tạo vectơ tịnh tiến rồi cộng theo quy tắc tam giác; đầu mút là điểm mới.

### Kiểm tra ba điểm thẳng hàng

Nếu $A,B,C$ thẳng hàng thì $\overrightarrow{OB}=\lambda \overrightarrow{OA}+(1-\lambda)\overrightarrow{OC}$.

### Mở rộng kiểm tra thẳng hàng

Trong tam giác $ABC$, nếu $D$ là điểm chia $BC$ theo tỉ lệ $n$ phần ($n\ BD=k\ DC$), thì:

$\overrightarrow{AD}=\frac{n}{k+n}\overrightarrow{AB}+\frac{k}{k+n}\overrightarrow{AC}$.

## Mở rộng sang không gian 3D

Trong không gian, các nội dung trên vẫn đúng. Thêm:

### Định lý cơ bản về vectơ không gian

Nếu ba vectơ $\boldsymbol{e_1},\boldsymbol{e_2},\boldsymbol{e_3}$ không đồng phẳng, thì tồn tại duy nhất $(x,y,z)$ sao cho mọi vectơ $\boldsymbol p$ thỏa $\mathbf p=x\boldsymbol{e_1}+y\boldsymbol{e_2}+z\boldsymbol{e_3}$.

Có thể chọn ba vectơ vuông góc làm cơ sở trực giao, lập hệ tọa độ không gian và biểu diễn bằng $(x,y,z)$.

### Định lý đồng phẳng

Nếu tồn tại hai vectơ không cùng phương $\boldsymbol{x},\boldsymbol{y}$, thì $\boldsymbol{p}$ đồng phẳng với $\boldsymbol{x},\boldsymbol{y}$ khi và chỉ khi tồn tại duy nhất $(a,b)$ sao cho $\boldsymbol{p}=a\boldsymbol{x}+b\boldsymbol{y}$.

### Vectơ chỉ phương

Hướng của một đường thẳng không gian được biểu diễn bởi một vectơ không-zero song song với nó, gọi là vectơ chỉ phương. Vị trí đường thẳng được xác định bởi một điểm và một vectơ chỉ phương.

Lưu ý: đường thẳng trong mặt phẳng cũng có vectơ chỉ phương.

Cách tìm vectơ chỉ phương trong **không gian**:

-   Với $A(x_1,y_1,z_1),B(x_2,y_2,z_2)$, vectơ chỉ phương là $\boldsymbol{s}=(x_2-x_1,y_2-y_1,z_2-z_1)$.
-   Nếu biết một mặt phẳng **vuông góc** với đường thẳng, phương trình $ax+by+cz+d=0$, thì vectơ chỉ phương là $\boldsymbol{s}=(a,b,c)$, đồng thời là **pháp tuyến** của mặt phẳng.

### Pháp tuyến

Với mặt $ABCD$, pháp tuyến $\boldsymbol{n}$ vuông góc mặt đó.

Cách tính: lấy hai đường trong mặt $\overrightarrow{AB},\overrightarrow{AD}$ sao cho $\overrightarrow{AB} \cdot \boldsymbol{n}=\boldsymbol{0}$ và $\overrightarrow{AD} \cdot \boldsymbol{n}=\boldsymbol{0}$, giải bằng tọa độ.

## Vectơ và ma trận

Trong đại số tuyến tính, biến đổi tuyến tính biểu diễn bằng ma trận. Với $T:\mathbf R^n\to\mathbf R^m$, $\mathbf x$ là vectơ cột $n$ chiều, tồn tại ma trận $m\times n$ $A$ sao cho

$$
T(\mathbf x)=A\mathbf x.
$$

$A$ gọi là ma trận biến đổi của $T$. Trong bài toán thuật toán thường $n=m$, nên $A$ là ma trận vuông. Khi đó bài toán biến đổi vectơ chuyển thành nhân ma trận.

Sau đây là ba biến đổi thường gặp: co giãn (ma trận $S$), quay (ma trận $R$), tịnh tiến (ma trận $T$).

### Co giãn

Với vectơ cột $n$ chiều $\boldsymbol a$, co giãn mỗi chiều $v_1,\ldots,v_n$. Ma trận là đường chéo:
$S=\operatorname{diag}\{v_1,\ldots,v_n\}$.

### Quay

Quay vectơ phức tạp hơn; chỉ xét 2D và 3D.

#### Quay quanh một điểm

Thường quay quanh gốc. Nếu quay quanh điểm $P$, dùng tịnh tiến đưa $P$ về gốc, quay, rồi tịnh tiến lại. Ma trận là $TRT^{-1}$.

Trong 2D, $\boldsymbol a=(x,y)$, góc nghiêng $\theta$, độ dài $l=\sqrt{x^2+y^2}$, nên $x=l\cos\theta,y=l\sin\theta$. Quay ngược chiều kim đồng hồ $\alpha$:

$$
\boldsymbol b=(l\cos(\theta+\alpha),l\sin(\theta+\alpha))
$$

![](./images/vector-rotation.svg)

Suy ra:

$$
\boldsymbol b=(x\cos\alpha-y\sin\alpha,y\cos\alpha+x\sin\alpha)
$$

Ma trận quay 2D:

$$
R=
\begin{bmatrix}
\cos\alpha & -\sin\alpha\\
\sin\alpha & \cos\alpha
\end{bmatrix}.
$$

Trong 3D, cần hai góc (thiên đỉnh và phương vị), có thể dùng [tọa độ cầu](../coordinate.md#空间球坐标系).

#### Quay quanh một đường thẳng

Với 3D, thường quay quanh một đường thẳng qua gốc. Nếu không qua gốc thì tịnh tiến.

Lấy vectơ chỉ phương $\boldsymbol u=(u_x,u_y,u_z)$, quay ngược chiều kim đồng hồ góc $\theta$. Ma trận:

$$
R=
\begin{bmatrix}
u_x^2 \left(1-\cos \theta\right) + \cos \theta & u_x u_y \left(1-\cos \theta\right) - u_z \sin \theta & u_x u_z \left(1-\cos \theta\right) + u_y \sin \theta \\ 
u_x u_y \left(1-\cos \theta\right) + u_z \sin \theta & u_y^2\left(1-\cos \theta\right) + \cos \theta & u_y u_z \left(1-\cos \theta\right) - u_x \sin \theta \\ 
u_x u_z \left(1-\cos \theta\right) - u_y \sin \theta & u_y u_z \left(1-\cos \theta\right) + u_x \sin \theta & u_z^2\left(1-\cos \theta\right) + \cos \theta
\end{bmatrix}.
$$

### Tịnh tiến

Tịnh tiến không phải biến đổi tuyến tính mà là biến đổi affine. Nhưng có thể biểu diễn bằng tuyến tính trong $\mathbf R^{n+1}$.

Với $\boldsymbol a=(a_1,\ldots,a_n)$, tịnh tiến theo $\boldsymbol t=(t_1,\ldots,t_n)$. Thêm một chiều: $\boldsymbol a'=(a_1,\ldots,a_n,1)$. Khi đó:

$$
T=
\begin{bmatrix}
1 &   &        &   & t_1    \\
  & 1 &        &   & t_2    \\
  &   & \ddots &   & \vdots \\
  &   &        & 1 & t_n    \\
  &   &        &   & 1      \\
\end{bmatrix}.
$$

Các biến đổi tuyến tính khác có thể chuyển thành affine bằng cách thêm một hàng và một cột (góc phải dưới là $1$, còn lại $0$). Ví dụ quay 2D:

$$
R'=
\begin{bmatrix}
\cos\alpha & -\sin\alpha & 0\\
\sin\alpha & \cos\alpha & 0\\
0 & 0 & 1
\end{bmatrix}.
$$

## Định nghĩa nghiêm ngặt hơn về vectơ

Trên đây vectơ được xem là đoạn thẳng có hướng. Nhưng để định nghĩa chặt chẽ hơn, cần [không gian tuyến tính](./vector-space.md). Xem thêm trang [không gian tuyến tính](./vector-space.md).

[^note1]: Xem [Rotation matrix from axis and angle - Wikipedia](https://en.wikipedia.org/wiki/Rotation_matrix#Rotation_matrix_from_axis_and_angle)
