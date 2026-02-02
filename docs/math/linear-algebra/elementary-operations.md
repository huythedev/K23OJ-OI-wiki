## Ma trận sơ cấp

Ba loại ma trận vuông sau gọi là ma trận sơ cấp.

### Ma trận nhân bội

Ma trận nhân bội là một dạng đường chéo đặc biệt:

$$
D_i(k)=\operatorname{diag}\{1,\cdots,1,k,1,\cdots,1\}
$$

Phần tử đường chéo thứ $i$ là $k$, với $k\ne 0$, các phần tử khác là $1$.

Nếu $k=1$ thì $D_i(1)$ là đơn vị $I$.

### Ma trận đổi chỗ

Ma trận đổi chỗ là đối xứng đặc biệt:

$$
P_{ij}=\begin{pmatrix}
I_{i-1} &  &  &  & \\
 & 0 &  & 1 & \\
 &  & I_{j-i-1} &  & \\
 & 1 &  & 0 & \\
 &  &  &  & I_{n-j}\\
\end{pmatrix}
$$

Chỉ có $1$ và $0$, đường chéo chính toàn $1$ trừ vị trí $i,j$ là $0$, và hai vị trí đối xứng là $1$.

Yêu cầu $i\ne j$.

### Ma trận cộng bội

Ma trận cộng bội là $I$ với phần tử $(i,j)$ bằng $k$:

$$
T_{ij}(k)=\begin{pmatrix}
1 &  &  &  &  &  & \\
 & \ddots &  &  &  &  & \\
 &  & 1 & \cdots & k &  & \\
 &  &  & \ddots & \vdots &  & \\
 &  &  &  & 1 &  & \\
 &  &  &  &  & \ddots & \\
 &  &  &  &  &  & 1\\
\end{pmatrix}
$$

Yêu cầu $i\ne j$. Nếu $k=0$ thì $T_{ij}(0)=I$.

Ma trận cộng bội là tam giác trên hoặc dưới.

### Định thức của ma trận sơ cấp

Ba loại có:

$$
|D_i(k)|=k
$$

$$
|P_{ij}|=-1
$$

$$
|T_{ij}(k)|=1
$$

Nhờ tính chất định thức của tích ma trận và quan hệ giữa biến đổi sơ cấp và nhân ma trận, các tính chất này dùng để tính định thức.

## Biến đổi sơ cấp

Không chỉ ma trận vuông, ma trận bất kỳ đều có biến đổi hàng/cột sơ cấp (gọi chung là biến đổi sơ cấp). Có 3 loại: nhân bội, đổi chỗ, cộng bội.

Biến đổi hàng:

-   Nhân hàng $i$ với $k\ne 0$: $B\mapsto D_i(k)B$.
-   Đổi hàng $i$ và $j$: $B\mapsto P_{ij}B$.
-   Cộng $k$ lần hàng $j$ vào hàng $i$: $B\mapsto T_{ij}(k)B$.

Đổi “hàng” thành “cột” được biến đổi cột.

Trong các biến đổi, đổi chỗ có thể mô phỏng bằng nhân bội và cộng bội; cộng bội không mô phỏng được bằng hai loại kia. Dựa trên định thức và quan hệ với nhân ma trận cũng thấy nhân bội không mô phỏng được bằng hai loại còn lại.

Do đó, nhân bội và cộng bội là cơ bản, đổi chỗ là thao tác phụ để sắp xếp trong khử.

## Biến đổi sơ cấp và nhân ma trận

Ba loại ma trận sơ cấp đều là kết quả của một phép biến đổi trên $I$. Với ma trận $A$, biến đổi hàng tương đương nhân trái bởi ma trận sơ cấp; biến đổi cột tương đương nhân phải.

### Nhân bội

Nhân trái bởi $D_i(k)$ ⇔ nhân hàng $i$ với $k$. Nhân phải ⇔ nhân cột $i$ với $k$.

Tích của hai đường chéo là đường chéo; nhân các phần tử tương ứng. Nếu phần tử đường chéo khác 0, có thể tách thành tích của các ma trận nhân bội.

Với đường chéo bất kỳ, nhân trái là nhân các hàng, nhân phải là nhân các cột theo phần tử đường chéo.

Vì $|D_i(k)|=k$, nhân một hàng/cột sẽ nhân định thức với $k$. Định thức đường chéo là tích các phần tử đường chéo.

Nhân bội (và đường chéo) giao hoán. Đơn vị tương ứng không làm gì.

### Đổi chỗ

Nhân trái bởi $P_{ij}$ ⇔ đổi hàng $i,j$. Nhân phải ⇔ đổi cột $i,j$.

Đưa khái niệm ma trận hoán vị: mỗi hàng/cột có đúng một $1$, còn lại $0$. $I$ là trường hợp đặc biệt.

Nhân trái bởi ma trận hoán vị ⇔ hoán vị hàng; nhân phải ⇔ hoán vị cột.

Ma trận hoán vị tương ứng một hoán vị, và nhóm của chúng đẳng cấu với nhóm hoán vị. Mọi hoán vị là tích các đổi chỗ.

Vì $|P_{ij}|=-1$, đổi hàng/cột đổi dấu định thức.

Nhân đổi chỗ không giao hoán. Định thức ma trận hoán vị là ${(-1)}^p$ với $p$ là số nghịch thế.

### Cộng bội

Nhân trái bởi $T_{ij}(k)$ ⇔ cộng $k$ lần hàng $j$ vào hàng $i$. Nhân phải ⇔ cộng $k$ lần cột $i$ vào cột $j$.

Có thể nhớ bằng cách quan sát $T_{ij}(k)$ là biến đổi của $I$; nhân trái là thao tác hàng, nhân phải là thao tác cột (hàng trái cột phải).

Vì $|T_{ij}(k)|=1$, cộng bội không đổi định thức.

Cộng bội không giao hoán. Đơn vị tương ứng không làm gì.

#### Ma trận tam giác trên

Ma trận cộng bội là tam giác trên/dưới. Chỉ xét tam giác trên.

Nếu đường chéo toàn 1, có thể tách thành tích các ma trận cộng bội bằng cách thao tác từng hàng của $I$ từ trên xuống.

Vì không giao hoán nên thứ tự không đổi.

Nếu đường chéo khác 0, có thể tách thành tích của cộng bội và nhân bội (trước nhân bội để đặt đường chéo).

Nếu đường chéo có 0 thì không thể tách thành tích các ma trận sơ cấp.

Định thức tam giác bằng tích đường chéo, giống đường chéo.

#### Dùng cộng bội đưa ma trận vuông về đường chéo

Chỉ dùng cộng bội cũng có thể đưa ma trận vuông về đường chéo, nhưng cần cả biến đổi hàng và cột.

Nếu hàng/cột đầu có phần tử khác 0, có thể dùng cộng bội để đưa góc trên trái khác 0, rồi khử các phần tử khác trên hàng/cột.

Nếu hàng/cột đầu toàn 0 thì xét hàng/cột tiếp theo.

Có thể sắp xếp để các phần tử khác 0 ở góc trên trái.

Nếu phần còn lại có phần tử khác 0, dùng cộng bội để tạo phần tử khác 0 ở hàng/cột đầu rồi quay về trường hợp trước. Nếu phần còn lại toàn 0 thì ma trận còn lại là 0.

#### Dạng chuẩn

Biến đổi sơ cấp đưa mọi ma trận về dạng chuẩn: có một khối đơn vị $I$ ở góc trên trái, phần còn lại là 0. Số lượng phần tử 1 bằng hạng.

## Ma trận khả nghịch

Với ma trận vuông $A$, nếu tồn tại $B$ sao cho $AB=BA=I$ thì $A$ khả nghịch (không suy biến), $B$ là nghịch đảo, ký hiệu $A^{-1}$.

Nghịch đảo là duy nhất. $A^{-1}$ cũng khả nghịch và $(A^{-1})^{-1}=A$.

Tích của hai ma trận khả nghịch cũng khả nghịch, nghịch đảo là $B^{-1}A^{-1}$.

Chuyển vị của ma trận khả nghịch cũng khả nghịch, và $(A^T)^{-1}=(A^{-1})^T$.

### Nghịch đảo của ma trận sơ cấp

Ma trận sơ cấp đều khả nghịch, nghịch đảo cùng loại:

$$
{D_i(k)}^{-1}=D_i\left(\frac{1}{k}\right)
$$

$$
P_{ij}^{-1}=P_{ij}
$$

$$
T_{ij}(k)^{-1}=T_{ij}(-k)
$$

$ I$ khả nghịch và tự nghịch.

Biến đổi sơ cấp bảo toàn khả nghịch.

$A$ khả nghịch ⇔ $A$ là tích các ma trận sơ cấp ⇔ $A$ biến đổi sơ cấp được về $I$.

Sau khi có định thức: $A$ khả nghịch ⇔ hạng $n$ ⇔ định thức khác 0.

Một cách ký hiệu: $E_{ij}$ là ma trận có 1 tại $(i,j)$, còn lại 0, thì

-   $D_i(k)=I_n+(k-1)E_{ii}$
-   $P_{ij}=I_n-E_{ii}-E_{jj}+E_{ij}+E_{ji}$
-   $T_{ij}(k)=I_n+kE_{ij}$

Cũng áp dụng cho nghịch đảo.

## Ứng dụng

### Giải hệ phương trình tuyến tính

Hệ phương trình có ma trận hệ số và ma trận mở rộng. Dùng biến đổi hàng sơ cấp để đưa về dạng bậc thang, rồi bậc thang rút gọn, từ đó giải hệ. Đây là phương pháp khử Gauss–Jordan.

### Tính định thức

Vì định thức của tích bằng tích định thức, cùng với tính chất của ma trận sơ cấp và biến đổi sơ cấp, có thể dùng để tính định thức. Khử Gauss–Jordan cũng dùng cho mục này.
