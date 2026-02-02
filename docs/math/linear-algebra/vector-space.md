author: codewasp942, Tiphereth-A

Không gian tuyến tính là sự khái quát của không gian Euclid $d$ chiều ($0\leq d\leq 3$), quan hệ khái niệm xem [Quan hệ giữa không gian Euclid và không gian tuyến tính](#quan-he-giua-khong-gian-euclid-va-khong-gian-tuyen-tinh).

Kiến thức trước: nhóm Abel, trường.

Hiểu một cách đơn giản: một tập đóng theo một phép toán, thỏa kết hợp, đơn vị, nghịch đảo thì là nhóm. Nếu thêm giao hoán thì là nhóm Abel.

Một tập đóng theo bốn phép toán thì là trường. Xem [khái niệm đại số trừu tượng](../algebra/basic.md#域).

## Định nghĩa

Không gian tuyến tính (không gian vectơ) là khái niệm cơ bản trong đại số tuyến tính. Nó là cấu trúc đại số gồm tập vectơ $V$, trường $\Bbb{P}$, phép cộng $+$ và phép nhân vô hướng.

Cụ thể, giả sử $(V,+)$ là một nhóm Abel, $\Bbb{P}$ là một trường.

Định nghĩa phép nhân vô hướng là một phép toán $\cdot:\Bbb{P}\times V\mapsto V$, ký hiệu $p\cdot v$ hoặc $pv$, với $p\in\Bbb{P}$, $v\in V$. Yêu cầu phép này đóng trong $V$.

Và thỏa các điều kiện:

1.  **Phân phối theo cộng vectơ**: với $\mathbf u,\mathbf v\in V,a\in \Bbb{P}$, $a(\mathbf u+\mathbf v)=a\mathbf u+a\mathbf v$
2.  **Phân phối theo cộng vô hướng**: với $a,b\in \Bbb{P},\mathbf u\in V$, $(a+b)\mathbf u=a\mathbf u+b\mathbf u$
3.  **Kết hợp nhân vô hướng**: với $a,b\in \Bbb{P},\mathbf u\in V$, $a(b\mathbf u)=(ab)\mathbf u$
4.  **Đơn vị nhân**: với $1\in \Bbb{P}$, $1\mathbf u=\mathbf u$

Khi đó $(V,+,\cdot,\mathbb{P})$ là một **không gian tuyến tính** trên $\Bbb{P}$. $\Bbb{P}$ là **trường cơ sở**, phần tử của $V$ gọi là **vectơ**, phần tử của $\Bbb{P}$ gọi là **vô hướng**. Nếu $\Bbb{P}=\Bbb{R}$ thì là không gian tuyến tính thực; nếu $\Bbb{P}=\Bbb{C}$ là không gian tuyến tính phức.

Bất kể là dãy số, mũi tên hay thứ gì, miễn thỏa các tiên đề, đều coi là vectơ và nghiên cứu bằng đại số tuyến tính.

Phần tử không của nhóm cộng gọi là vectơ không, ký hiệu $\mathbf 0$ hoặc $\mathbf\theta$.

Cộng/trừ trong nhóm và nhân vô hướng trong không gian gọi chung là **phép toán tuyến tính**.

???+ note "Note"
    Để tiện diễn đạt, ở dưới:
    
    1.  Không in đậm phần tử của $V$.
    2.  Gọi luôn hệ $(V,+,\cdot,\mathbb{P})$ là không gian tuyến tính.
    
    Xin chú ý phân biệt.

### Hiểu trực quan

Không nghiêm ngặt, nhân vô hướng là “**co giãn**”, phần tử của $\Bbb{P}$ là “**tỉ lệ**”, còn cộng vectơ là “**chồng chập**”. Đồng thời $\Bbb{P}$ cũng quy định miền giá trị của “tọa độ”.

Các điều kiện 1-4 mô tả quan hệ giữa co giãn và chồng chập, có thể hiểu qua hình học 2D.

### Tính chất đơn giản

???+ note "Note"
    Các tính chất này có thể tìm trong lý thuyết nhóm.

Với $(V,+,\cdot,\Bbb{P})$:

1.  $\theta$ là duy nhất
2.  Với mọi $\alpha\in V$, $-\alpha$ là duy nhất
3.  Tồn tại $0\in\mathbb{P}$ sao cho $\forall\alpha\in V$, $0\alpha=\theta$
4.  $\forall k\in\mathbb{P}$, $k\theta=\theta$
5.  $(-1)\alpha=-\alpha,~\forall\alpha\in V$
6.  Không có ước số không: $\forall\alpha\in V,k\in\mathbb{P}$, $k\alpha=\theta\implies k=0\lor\alpha=\theta$
7.  Luật khử cộng: $\forall\alpha,\beta,\gamma\in V$, $\alpha+\beta=\alpha+\gamma\implies\beta=\gamma$

    > Đây là tính chất của nhóm Abel.

### Ví dụ

1.  $\Bbb{P}^n$ với cộng và nhân vô hướng là không gian tuyến tính trên $\Bbb{P}$ (ví dụ $\Bbb{R}$, $\Bbb{C}$, $\Bbb{N}_p$ với $p$ nguyên tố).
2.  Tập ma trận $n\times m$ trên $\Bbb{P}$ với cộng và nhân vô hướng là không gian tuyến tính.
3.  Vành đa thức một biến $\Bbb{P}[x]$ với cộng và nhân vô hướng là không gian tuyến tính.
4.  Tập hàm liên tục trên $[a,b]$ (ký hiệu $C[a,b]$) với cộng hàm và nhân vô hướng là không gian tuyến tính.

## Khái niệm liên quan

### Phụ thuộc và độc lập tuyến tính

Với $(V,+,\cdot,\Bbb{P})$:

1.  Gọi $a_1,a_2,\dots,a_n\in V$ là một **nhóm vectơ**.
2.  Với $k_1,\dots,k_n\in\Bbb{P}$, $\sum_{i=1}^nk_ia_i$ là **tổ hợp tuyến tính**.
3.  Nếu $\beta\in V$ biểu diễn được bởi $a_1,\dots,a_n$ thì nói $\beta$ **biểu diễn tuyến tính** theo nhóm này.
4.  Nếu $\sum_{i=1}^nk_ia_i=\theta\iff k_i=0$ với mọi $i$, thì nhóm **tuyến tính độc lập**; ngược lại là **tuyến tính phụ thuộc**.

Quy ước vectơ không phụ thuộc tuyến tính với mọi vectơ.

Biểu thức tuyến tính có thể viết dạng nhân ma trận:

$$
\beta=k_1a_1+k_2a_2+\cdots+k_ra_r=(a_1,a_2,\cdots,a_r)\begin{pmatrix} k_1 \\ k_2 \\ \vdots \\ k_r \end{pmatrix}
$$

Theo thói quen, các vectơ $a$ xếp ngang bên trái, các vô hướng $k$ xếp dọc bên phải thành “vectơ cột”.

Lưu ý: “vectơ cột” chỉ là ký hiệu hình thức, không thuộc $V$, khác bản chất với các vectơ bên trái. Nếu các $a$ cũng là vectơ cột thì ghép lại thành ma trận, và phép nhân chính là “ma trận nhân vectơ cột”.

Dưới đây, biểu diễn tuyến tính cũng tương đương với việc $\beta$ nằm trong không gian ảnh của ma trận $(a_1,\dots,a_r)$.

Theo định nghĩa, vectơ không luôn thuộc ảnh. Dưới góc nhìn biến đổi tuyến tính, phụ thuộc tuyến tính nghĩa là nhiều vectơ bị biến về 0, độc lập tuyến tính nghĩa là chỉ có 0 bị biến về 0.

#### Tính chất

Với $(V,+,\cdot,\Bbb{P})$:

1.  Nếu một phần của nhóm phụ thuộc thì cả nhóm phụ thuộc. Nếu nhóm độc lập thì mọi phần con không rỗng độc lập. Tóm tắt: **“lớn độc lập ⇒ nhỏ độc lập; nhỏ phụ thuộc ⇒ lớn phụ thuộc”**.
2.  Nhóm chứa $\theta$ thì phụ thuộc.
3.  Nhóm phụ thuộc khi và chỉ khi một vectơ biểu diễn được bằng các vectơ còn lại.
4.  Nếu $\beta$ biểu diễn được theo $a_1,\dots,a_n$, thì cách biểu diễn duy nhất khi và chỉ khi nhóm đó độc lập.
5.  Nếu $a_1,\dots,a_n$ độc lập, thì $\beta$ biểu diễn được theo nhóm này khi và chỉ khi nhóm $a_1,\dots,a_n,\beta$ phụ thuộc.

### Nhóm độc lập cực đại, hạng

Phụ thuộc tuyến tính có thể hiểu là “thừa”. Xóa bớt các vectơ thừa sẽ còn một nhóm độc lập cực đại.

Với $(V,+,\cdot,\Bbb{P})$:

1.  Với nhóm $b_1,\dots,b_m$, lấy $\{a_1,\dots,a_n\}\subseteq\{b_1,\dots,b_m\}$. Nếu:

    -   $a_1,\dots,a_n$ độc lập.
    -   Với mọi $\beta$ còn lại, $a_1,\dots,a_n,\beta$ phụ thuộc.

    Thì $a_1,\dots,a_n$ là **nhóm độc lập cực đại** của $b_1,\dots,b_m$. Tương tự cho không gian $V$.

    Quy ước nhóm $\theta,\theta,\dots,\theta$ có nhóm độc lập cực đại rỗng; ma trận 0 không có nhóm độc lập cực đại.

    Cách xóa vectơ không duy nhất, nên nhóm độc lập cực đại không duy nhất. Thường xóa từ trái sang phải.

    Ngẫu nhiên là cách này trùng với việc “nhìn theo hàng” trong khử Gauss: các cột có phần tử $1$ trong dạng bậc thang rút gọn.

    Kích thước của nhóm độc lập cực đại gọi là **hạng** của nhóm, ký hiệu $\operatorname{rank}\{b_1,\dots,b_m\}$, và $\operatorname{rank}\{\theta,\dots,\theta\}=0$.

    Định nghĩa hạng nhóm vectơ trùng với hạng ma trận.

2.  Nếu nhóm $a_1,\dots,a_n$ có thể biểu diễn tuyến tính mọi vectơ của $b_1,\dots,b_m$, nói $b_1,\dots,b_m$ **biểu diễn được** bởi $a_1,\dots,a_n$.

3.  Nếu hai nhóm biểu diễn được lẫn nhau, gọi là **tương đương**, ký hiệu $\{a_1,\dots,a_n\}\cong\{b_1,\dots,b_m\}$.

    Tương đương nghĩa là không gian sinh ra giống nhau. Nếu không gian sinh khác nhau thì không tương đương.

    Tương đương nhóm mạnh hơn tương đương ma trận: không chỉ hạng bằng nhau, mà không gian cũng phải giống. Vì vậy, ghép **ngang** hai ma trận, hạng không đổi.

    Tương đương ma trận chỉ yêu cầu hạng bằng nhau, tức có biến đổi khả nghịch đưa ma trận này sang ma trận kia.

#### Tính chất

Với $(V,+,\cdot,\Bbb{P})$:

1.  Nếu $a_1,\dots,a_n$ biểu diễn được bởi $b_1,\dots,b_m$:
    -   Nếu $n>m$ thì $a_1,\dots,a_n$ phụ thuộc.
    -   Nếu $a_1,\dots,a_n$ độc lập thì $n\le m$.

2.  Hai nhóm độc lập tương đương có cùng kích thước.

    Mọi nhóm độc lập cực đại của một nhóm có cùng kích thước.

3.  Nhóm độc lập khi và chỉ khi hạng bằng kích thước.

4.  Nếu $a_1,\dots,a_n$ biểu diễn được bởi $b_1,\dots,b_m$ thì $\operatorname{rank}\{a_1,\dots,a_n\}\le\operatorname{rank}\{b_1,\dots,b_m\}$.

5.  Nhóm tương đương có cùng hạng.

### Không gian sinh

Với $(V,+,\cdot,\Bbb{P})$, tập

$\left\{v=\sum_{i=1}^nk_ia_i:a_i\in V,k_i\in\Bbb{P},i=1,2,\dots,n\right\}$

là một không gian tuyến tính, gọi là không gian do $a_1,\dots,a_n$ **sinh** (hay **bao tuyến tính**), ký hiệu $\operatorname{span}\{a_1,\dots,a_n\}$.

Các $a_i$ không nhất thiết độc lập.

### Không gian con tuyến tính

Với $(V,+,\cdot,\Bbb{P})$, nếu $(V_1,+,\cdot,\Bbb{P})$ thỏa:

1.  $\varnothing\ne V_1$
2.  $V_1\subseteq V$
3.  $V_1$ là không gian tuyến tính trên $\mathbb{P}$

thì $V_1$ là **không gian con** của $V$, ký hiệu $V_1\leq V$.

Mọi $V$ có hai không gian con **tầm thường**: chính nó và không gian 0. Không gian 0 chỉ chứa vectơ không.

Nếu thay $\subseteq$ bằng $\subset$, gọi là không gian con thật, ký hiệu $V_1<V$.

Có thể chứng minh: một tập con không rỗng $V_1$ là không gian con khi và chỉ khi phép toán tuyến tính đóng trên $V_1$:

1.  $\forall u,v\in V_1$,$u+v\in V_1$
2.  $\forall v\in V_1$,$\forall k\in \Bbb{P}$,$kv\in V_1$

### Giao, tổng, tổng trực tiếp, tích trực tiếp

Với không gian tuyến tính $(V_1,+,\cdot,\Bbb{P})$ và $(V_2,+,\cdot,\Bbb{P})$:

1.  $V_1\cap V_2$ đóng với $+,\cdot$, nên là **giao**. Tương tự với nhiều không gian: $\bigcap_{i=1}^m V_i$.

2.  Nếu $V=\{u+v|u\in V_1,v\in V_2\}$ thì $V$ là **tổng** $V_1+V_2$. Đây là không gian con nhỏ nhất chứa $V_1\cup V_2$. Tương tự $\sum_{i=1}^m V_i$.

3.  Nếu với mọi $v\in V$ biểu diễn $v=v_1+v_2$ là duy nhất, thì $V=V_1\oplus V_2$ là **tổng trực tiếp**. Tương tự $\bigoplus_{i=1}^m V_i$.

4.  **Tích trực tiếp** $V_1\times V_2$ là tích Descartes với các phép toán:
    1.  $+:(V_1\times V_2)\times(V_1\times V_2)\mapsto V_1\times V_2; ((u_1,v_1),(u_2,v_2))\to (u_1+u_2,v_1+v_2)$
    2.  $\cdot:\Bbb{P}\times(V_1\times V_2)\mapsto V_1\times V_2; (k,(u,v))\to (ku,kv)$

    Tương tự $\prod_{i=1}^m V_i$.

#### Ví dụ

Với $V=\Bbb{R}^3$, đặt:

-   $V_1:=\{(x,0,0)|x\in\Bbb{R}\}$
-   $V_2:=\{(x,y,0)|x,y\in\Bbb{R}\}$
-   $V_3:=\{(0,y,z)|y,z\in\Bbb{R}\}$
-   $V_4:=\{(x,0,z)|x,z\in\Bbb{R}\}$

Thì:

1.  $V_1<V_2<V$,$V_3<V$
2.  $V_2=V_1+V_2$
3.  $V=V_1\oplus V_3=V_2+V_3$
4.  $V_2\oplus V_3=V_4$,$V_2\oplus V_4=V_3$,$V_3\oplus V_4=V_2$
5.  $V_2+V_3\leq V$

#### Tính chất

1.  Với $V_1,V_2,V_3$:
    1.  Giao có giao hoán: $V_1\cap V_2=V_2\cap V_1$
    2.  Giao có kết hợp: $V_1\cap(V_2\cap V_3)=(V_1\cap V_2)\cap V_3$
2.  Với $V_1,V_2,V_3$:
    1.  Tổng có giao hoán: $V_1+V_2=V_2+V_1$
    2.  Tổng có kết hợp: $V_1+(V_2+V_3)=(V_1+V_2)+V_3$
3.  Quan hệ giao và tổng:
    1.  $V_1\cap (V_2+V_3)\supseteq (V_1\cap V_2)+(V_1\cap V_3)$
    2.  $V_1+(V_2\cap V_3)\subseteq (V_1+V_2)\cap (V_1+V_3)$
4.  $\operatorname{span}\{a_1,\dots,a_n\}+\operatorname{span}\{b_1,\dots,b_m\}=\operatorname{span}\{a_1,\dots,a_n,b_1,\dots,b_m\}$
5.  Với $V_1,V_2$, các mệnh đề sau tương đương:

    1.  $V_1+V_2=V_1\oplus V_2$

    2.  $\exists \beta\in V_1+V_2$ có cách phân rã duy nhất (suy ra “mọi $\to$ tồn tại”)

    3.  $\theta$ có cách phân rã duy nhất

    4.  $V_1\cap V_2=\{\theta\}$

    ???+ note "Chứng minh"
        $1\implies 2$: theo định nghĩa.
        
        $2 \implies 3$:
        
        Lấy $\beta=\beta_1+\beta_2$, nếu $\theta=\alpha_1+\alpha_2$ với $\alpha_1\in V_1,\alpha_2\in V_2$ và $\theta\ne\alpha_1$, thì $\beta=(\beta_1+\alpha_1)+(\beta_2+\alpha_2)$, mâu thuẫn tính duy nhất.
        
        $3 \implies 4$:
        
        Nếu $\exists \alpha\ne 0$ thuộc giao, thì $\theta=\alpha+(-\alpha)=(-\alpha)+\alpha$ là hai cách khác nhau.
        
        $4 \implies 1$:
        
        Nếu không là trực tổng, tồn tại $\beta=\beta_1+\beta_2=\gamma_1+\gamma_2$ với các thành phần khác nhau. Khi đó $\theta\ne\beta_1-\gamma_1=\gamma_2-\beta_2\in V_1\cap V_2$, mâu thuẫn.

### Đẳng cấu

Cho $V,V'$ là không gian tuyến tính trên $\Bbb{P}$. Nếu tồn tại song ánh $\sigma:V\mapsto V'$ giữ phép cộng và nhân vô hướng:

1.  $\sigma(u+v)=\sigma(u)+\sigma(v)$
2.  $\sigma(ku)=k\sigma(u)$

thì $\sigma$ là **đẳng cấu**, và $V\cong V'$.

???+ note "Note"
    Nếu $\sigma$ đơn ánh gọi **đơn đồng cấu**; nếu toàn ánh gọi **mãn đồng cấu**.

#### Tính chất

1.  Hai không gian tuyến tính trên $\Bbb{P}$ đẳng cấu khi và chỉ khi có cùng số chiều (xem [cơ sở tuyến tính](./basis.md)).
2.  (Hệ quả) Mọi không gian $n$ chiều trên $\Bbb{P}$ đẳng cấu với $\Bbb{P}^n$.

    ???+ note "Note"
        Tính chất này cho thấy có thể coi tọa độ và vectơ là tương đương về bản chất.

## Quan hệ giữa không gian Euclid và không gian tuyến tính

Lấy không gian Euclid 3D làm ví dụ, quan hệ tương ứng:

| Không gian Euclid 3D | Không gian tuyến tính |
| -------- | ----------------- |
| Vectơ | Vectơ |
| Vuông góc | Trực giao (tích vô hướng $0$) |
| 3 vectơ đồng tuyến/đồng phẳng | $k$ vectơ phụ thuộc tuyến tính |
| 3 vectơ không đồng phẳng | $k$ vectơ độc lập tuyến tính |
| Vectơ cơ sở | [Cơ sở tuyến tính](./basis.md) |
| Số chiều không gian | Số chiều không gian |

## Ứng dụng

Từ đây chủ yếu nói về cách nhìn “theo cột” của hệ phương trình tuyến tính.

Ma trận $A$ gồm các vectơ cột. Xem $A$ như một nhóm vectơ cột, $x$ là hệ số chưa biết, ta hỏi liệu nhóm này có thể kết hợp để tạo vectơ $b$ hay không.

Phương trình $Ax=b$ trở thành:

$$
\alpha_1 x_1 +\alpha_2 x_2 +\cdots+\alpha_n x_n=b 
$$

Ma trận $A$ là một nhóm vectơ, sinh ra một không gian; ta xét $b$ có nằm trong không gian đó không.

### Nhìn theo cột về nghiệm hệ

Hạng là số ràng buộc. Các vectơ còn lại tạo độ tự do. Nếu $n$ là số cột, $r(A)$ là hạng, thì độ tự do:

$$
S=n-r(A)
$$

Tập nghiệm cũng là một nhóm vectơ, độ tự do $S$ chính là hạng của nghiệm $Ax=0$, tức số chiều của không gian kernel.

### Hệ phương trình cùng nghiệm

Hai hệ có **nghiệm chung** là giao của hai tập nghiệm. Hai hệ **cùng nghiệm** nếu tập nghiệm bằng nhau.

Cùng nghiệm mạnh hơn tương đương ma trận: không chỉ hạng bằng nhau mà khi ghép **dọc** cũng không đổi hạng.

So sánh với tương đương nhóm vectơ (ghép ngang), ta có:

Ma trận tương đương không suy ra nhóm tương đương hay hệ cùng nghiệm, nhưng nếu nhóm tương đương hoặc hệ cùng nghiệm thì ma trận tương đương (hạng bằng nhau).

Nếu nhóm tương đương, thì chuyển vị ma trận tương ứng cho hệ cùng nghiệm, và ngược lại.

### Không gian kernel và image của ma trận

Xét ma trận $A$, gọi $W$ là tập nghiệm của $Ax=0$. Khi đó $W$ là không gian tuyến tính.

Gọi $W$ là **kernel** (không gian hạt nhân) của $A$, ký hiệu $N(A)$.

Kernel $N(A)$ chính là **không gian nghiệm** của $Ax=0$. Theo định nghĩa cơ sở, **hệ nghiệm cơ bản** là cơ sở của kernel.

Nếu $A$ khả nghịch thì $N(A)$ chỉ có vectơ không.

Với ma trận $A$, gọi các cột là $\alpha$, không gian sinh bởi các cột là **image** (không gian ảnh), ký hiệu:

$$
R(A)=\operatorname{span}\{\alpha_1,\alpha_2,\cdots,\alpha_n\}
$$

Số chiều của $R(A)$ bằng hạng của $A$.

Mỗi $y\in R(A)$ có:

$$
y=k_1\alpha_1+k_2\alpha_2+\cdots+k_n\alpha_n=(\alpha_1,\alpha_2,\cdots,\alpha_n)\begin{pmatrix}k_1\\k_2\\\vdots\\k_n\end{pmatrix}=A\begin{pmatrix}k_1\\k_2\\\vdots\\k_n\end{pmatrix}
$$

Do đó $R(A)$ là **ảnh** của phép biến đổi $x\mapsto Ax$.

Tương tự, định nghĩa **không gian hàng** là $R(A^T)$.

Vì hạng hàng bằng hạng cột, chuyển vị chỉ đổi không gian ảnh, không đổi số chiều.

Quan hệ với phần trước:

Nhóm vectơ tương đương $\iff$ không gian ảnh $R(A)$ giống nhau.

Hệ phương trình cùng nghiệm $\iff$ không gian hàng $R(A^T)$ giống nhau.

## Tài liệu tham khảo và chú thích

1.  丘维声，高等代数（下）．清华大学出版社．
2.  [Vector space](https://en.wikipedia.org/w/index.php?title=Vector_space&oldid=1108546097).*Wikipedia, The Free Encyclopedia*.
