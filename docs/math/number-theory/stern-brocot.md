## Giới thiệu

Bài viết này giới thiệu cấu trúc dữ liệu lưu trữ phân số tối giản và các khái niệm liên quan. Chúng có quan hệ chặt chẽ với [phân số liên tục](./continued-fraction.md), trong lập trình thi đấu có thể dùng để giải một loạt bài toán số học và đôi khi là nền tảng ẩn của một số đề bài.

## Cây Stern–Brocot

Cây Stern–Brocot là một cấu trúc thanh nhã để quản lý phân số, chứa tất cả các số hữu tỉ dương khác nhau. Cấu trúc này được Moritz Stern (1858) và Achille Brocot (1861) phát hiện độc lập.

### Xây dựng

#### Xây dựng theo từng tầng

Cây Stern–Brocot có thể thu được trong quá trình lặp tạo dãy Stern–Brocot bậc $k$ (Stern–Brocot sequence of order $k$). Dãy Stern–Brocot bậc $0$ gồm hai phân số đơn giản:

$$
\frac{0}{1},\ \frac{1}{0}.
$$

Ở đây $\dfrac{1}{0}$ theo nghĩa chặt chẽ không phải là phân số hữu tỉ, có thể hiểu là phân số tối giản biểu diễn $\infty$.

Trong dãy Stern–Brocot bậc $k$, chèn phân số trung vị (mediant)[^mediant] $\dfrac{a+c}{b+d}$ vào giữa hai phân số kề nhau $\dfrac{a}{b}$ và $\dfrac{c}{d}$ thì ta thu được dãy Stern–Brocot bậc $k+1$. Mặc dù định nghĩa phân số trung vị cho phép rút gọn, trong xây dựng cây Stern–Brocot chỉ cần cộng tử và mẫu tương ứng, không cần lo rút gọn. Do đó có thể lặp để xây dựng dãy Stern–Brocot mọi bậc. Kết quả vài lần lặp đầu như sau:

$$
\begin{array}{ccccccccc}
&&&\dfrac{0}{1}, & \dfrac{1}{1}, & \dfrac{1}{0} &&&\\\\
&&\dfrac{0}{1}, & \dfrac{1}{2}, & \dfrac{1}{1}, & \dfrac{2}{1}, & \dfrac{1}{0} &&\\\\
\dfrac{0}{1}, & \dfrac{1}{3}, & \dfrac{1}{2}, & \dfrac{2}{3}, & \dfrac{1}{1}, & \dfrac{3}{2}, & \dfrac{2}{1}, & \dfrac{3}{1}, & \dfrac{1}{0}
\end{array}
$$

Nối các phân số mới thêm ở mỗi lần lặp thành cấu trúc cây thì được cây Stern–Brocot, như hình:

![](./images/stern-brocot-tree.svg)

Dãy Stern–Brocot bậc $k$, bỏ hai đầu mút, chính là duyệt trung thứ tự của cây Stern–Brocot có độ sâu $k-1$.

#### Xây dựng bằng bộ ba

Một cách xây dựng tương đương là lấy bộ ba

$$
\left(\dfrac{0}{1},\dfrac{1}{1},\dfrac{1}{0}\right)
$$

làm nút gốc, và với mỗi nút

$$
\left(\dfrac{a}{b},\dfrac{p}{q},\dfrac{c}{d}\right)
$$

thì thêm vào hai con trái/phải:

$$
\left(\dfrac{a}{b},\dfrac{a+p}{b+q},\dfrac{p}{q}\right),\ \left(\dfrac{p}{q},\dfrac{p+c}{q+d},\dfrac{c}{d}\right)
$$

là con trái/phải, ta xây được toàn bộ cây Stern–Brocot. Trong mỗi bộ ba của một nút, phân số thực sự lưu là phân số ở giữa $\dfrac{p}{q}$, còn hai phân số $\dfrac{a}{b}$ và $\dfrac{c}{d}$ xuất hiện từ trước đó. Hơn nữa, xét cách xây dựng trước, ta thấy $\dfrac{p}{q}$ đúng là phân số trung vị được chèn giữa $\dfrac{a}{b}$ và $\dfrac{c}{d}$.

#### Biểu diễn ma trận và hệ số Stern–Brocot

Cách xây dựng theo bộ ba cho thấy mỗi nút trên cây Stern–Brocot tương ứng với một ma trận

$$
S = \begin{pmatrix}
b & d\\
a & c
\end{pmatrix}.
$$

Nút gốc là ma trận đơn vị $I$, và việc đi sang con trái/con phải tương ứng với việc nhân phải ma trận hiện tại với

$$
L=\begin{pmatrix}
1 & 1 \\
0 & 1
\end{pmatrix},~
R=\begin{pmatrix}
1 & 0 \\
1 & 1
\end{pmatrix}.
$$

Phân số thực sự tương ứng với mỗi nút là $f(S)=\dfrac{a+c}{b+d}$. Ma trận $S$ của mỗi nút có thể viết như một tích các ma trận $L$ và $R$, có thể hiểu là một chuỗi ký tự từ $L$ và $R$ biểu diễn đường đi từ gốc đến nút đó. Biểu diễn mọi hữu tỉ dương duy nhất theo chuỗi như vậy chính là một dạng biểu diễn số hữu tỉ dương, nên còn gọi là **hệ số Stern–Brocot** (Stern–Brocot number system).

#### Cài đặt dựng cây

Thuật toán dựng cây chỉ cần mô phỏng quá trình trên. Dưới đây là mã duyệt trung thứ tự cây Stern–Brocot đến $n$ tầng.

???+ example "Dựng cây"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/stern-brocot/tree-build.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/stern-brocot/tree-build.py:core"
        ```

Độ phức tạp của thuật toán dựng cây là $O(n^2)$.

### Tính chất

Tiếp theo bàn về các tính chất của cây Stern–Brocot. Tóm lại, cây Stern–Brocot là [cây nhị phân tìm kiếm](../../ds/bst.md) chứa tất cả các phân số hữu tỉ dương tối giản; đồng thời là [heap](../../ds/binary-heap.md) theo tử và mẫu, và là [cây Cartesian](../../ds/cartesian-tree.md) của các cặp (mẫu, tử). Nếu xét các khoảng tạo bởi hai đầu mút trong cách xây dựng theo bộ ba, thì cây Stern–Brocot còn có thể xem như [cây đoạn](../../ds/seg.md) trên $[0,\infty]$. Các mệnh đề này có thể suy ra từ ba tính chất cơ bản sau:

#### Tính đơn điệu

Trong xây dựng trên, các phân số ở mỗi tầng là đơn điệu tăng. Chỉ cần chứng minh bằng quy nạp. Nếu $\dfrac{a}{b} < \dfrac{c}{d}$ thì tất yếu

$$
\dfrac{a}{b} < \dfrac{a+c}{b+d} < \dfrac{c}{d}.
$$

Điều này suy ra bằng cách khử mẫu trong bất đẳng thức. Trường hợp cơ sở là $\dfrac{0}{1} < \dfrac{1}{0}$, nên tính đơn điệu hiển nhiên đúng.

#### Tính tối giản

Trong xây dựng trên, mọi phân số đều là tối giản. Cũng cần quy nạp chứng minh rằng trong mỗi tầng, hai phân số kề nhau $\dfrac{a}{b}$ và $\dfrac{c}{d}$ luôn thỏa

$$
bc-ad = \det\begin{pmatrix}
b & d\\
a & c
\end{pmatrix} = 1.
$$

Ở nút gốc là ma trận đơn vị nên hiển nhiên đúng. Khi đi xuống, ma trận nhân $L$ và $R$ đều có định thức $1$, theo tính chất [định thức](../linear-algebra/determinant.md) thì ở tầng sau vẫn có

$$
\det\begin{pmatrix}b & b+d \\ a & a+c \end{pmatrix} = \det\begin{pmatrix}b+d & d \\ a+c & c\end{pmatrix} = 1.
$$

Với trường hợp cơ sở $\dfrac{0}{1}$ và $\dfrac{1}{0}$ cũng hiển nhiên. Do đó, theo [định lý Bézout](./bezouts.md), tử và mẫu của mọi phân số đều nguyên tố cùng nhau, tức là mọi phân số đều tối giản.

#### Tính đầy đủ

Cuối cùng, cần chứng minh cây Stern–Brocot chứa tất cả các phân số tối giản dương. Vì hai tính chất trước cho thấy cây Stern–Brocot là cây nhị phân tìm kiếm, mọi phân số tối giản dương $\dfrac{p}{q}$ đều nằm giữa $\dfrac{0}{1}$ và $\dfrac{1}{0}$. Theo cách tìm kiếm trên cây nhị phân tìm kiếm, khả năng duy nhất để không có $\dfrac{p}{q}$ là quá trình tìm kiếm vô hạn, điều này là không thể.

Giả sử đã biết

$$
\dfrac{a}{b} < \dfrac{p}{q} < \dfrac{c}{d},
$$

thì tất yếu

$$
bp-aq \ge 1,\ cq-dp \ge 1.
$$

Nhân bất đẳng thức lần lượt với $(c+d)$ và $(a+b)$, ta được

$$
(c+d)(bp-aq) + (a+b)(cq-dp) \ge a+b+c+d.
$$

Dùng hệ thức $bc-ad=1$ đã chứng minh ở trên, suy ra

$$
p+q \ge a+b+c+d.
$$

Mỗi khi tìm sâu thêm một tầng, vế phải tăng nghiêm ngặt còn vế trái không đổi, nên tìm kiếm chắc chắn dừng sau hữu hạn bước.

### Tìm phân số

Trong ứng dụng thực tế của cây Stern–Brocot, thường cần truy vấn vị trí của một phân số trong cây.

#### Thuật toán đơn giản

Vì Stern–Brocot là cây nhị phân tìm kiếm, chỉ cần so sánh phân số hiện tại với phân số cần tìm để quyết định đường đi từ gốc. Gọi việc đi sang con trái và con phải lần lượt là $L$ và $R$, thì mỗi đường đi tương ứng với một chuỗi gồm $L$ và $R$, chính là biểu diễn của số hữu tỉ trong hệ Stern–Brocot nói ở trên. Quá trình tìm đường đi tới một phân số chính là tìm biểu diễn của phân số đó trong hệ Stern–Brocot.

Cài đặt thuật toán tìm phân số đơn giản như sau:

???+ example "Tìm phân số đơn giản"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/stern-brocot/fraction-finding-1.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/stern-brocot/fraction-finding-1.py:core"
        ```

Độ phức tạp của thuật toán là $O(p+q)$, nên không thực tế trong thi đấu.

Trong hệ Stern–Brocot, mỗi số vô tỉ dương tương ứng với một chuỗi vô hạn duy nhất. Có thể dùng cùng thuật toán để xây chuỗi này. Mỗi tiền tố của chuỗi vô hạn tương ứng với một phân số tối giản hữu tỉ. Xếp các phân số này thành một dãy, mẫu số tăng nghiêm ngặt và giới hạn của dãy chính là số vô tỉ đó. Vì vậy, cây Stern–Brocot có thể dùng để tìm xấp xỉ hữu tỉ với độ chính xác tùy ý cho một số vô tỉ. Tuy nhiên, cần lưu ý khoảng cách giữa các phân số trong dãy và số vô tỉ không giảm nghiêm ngặt. Với lý thuyết xấp xỉ hữu tỉ chặt chẽ, hãy xem mục [xấp xỉ Diophantine](./continued-fraction.md#丢番图逼近) của trang phân số liên tục. Trong ứng dụng thực tế khi tìm xấp xỉ tốt nhất với mẫu số không vượt một ngưỡng, cuối cùng cần so sánh khoảng cách từ hai đầu mút tới số thực đó.

#### Thuật toán nhanh

Thuật toán đơn giản chưa hiệu quả, nhưng có thể tối ưu để đạt $O(\log(p+q))$ bằng cách gộp các bước $L$ hoặc $R$ liên tiếp.

Nếu phân số cần tìm $\dfrac{p}{q}$ nằm giữa $\dfrac{a}{b}$ và $\dfrac{c}{d}$, thì khi đi sang phải liên tiếp $t$ lần, biên phải giữ nguyên, biên trái cập nhật thành $\dfrac{a+tc}{b+td}$; ngược lại, khi đi sang trái liên tiếp $t$ lần, biên trái giữ nguyên, biên phải cập nhật thành $\dfrac{ta+c}{tb+d}$. Vì vậy có thể trực tiếp xác định số lần đi phải/trái từ $\dfrac{a+tc}{b+td}<\dfrac{p}{q}$ hoặc $\dfrac{p}{q}<\dfrac{ta+c}{tb+d}$. Dùng bất đẳng thức nghiêm vì ta đang di chuyển các đầu mút, còn phân số cần tìm là trung vị của hai đầu mút cuối cùng.

???+ example "Tìm phân số nhanh"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/stern-brocot/fraction-finding-2.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/stern-brocot/fraction-finding-2.py:core"
        ```

Thuật toán trên cần biết trước $\dfrac{p}{q}$. Nếu mục tiêu chưa biết, thường phải dùng tăng đôi hoặc nhị phân để tìm số bước mỗi lần đi trái/phải. Khi đó, độ phức tạp là $O(\log^2(p+q))$.

#### Thuật toán dựa trên phân số liên tục

Khi phân số đã biết, có thể dùng phân số liên tục để có thuật toán đơn giản hơn. Giả sử nhóm di chuyển đầu tiên là sang phải; nếu không, đặt số lần đi phải đầu tiên là 0. Xen kẽ đi phải và trái, các đầu mút sau mỗi nhóm di chuyển là:

$$
\dfrac{p_0}{q_0},~\dfrac{p_1}{q_1},~\dfrac{p_2}{q_2},~\cdots,~\dfrac{p_{n-2}}{q_{n-2}},~\dfrac{p_{n-1}}{q_{n-1}},~\dfrac{p_n}{q_n}.
$$

Trong đó nhóm chẵn là đi phải nên ghi vị trí biên trái; nhóm lẻ là đi trái nên ghi vị trí biên phải. Thêm hai đầu mút trước đó:

$$
\dfrac{p_{-2}}{q_{-2}}=\dfrac{0}{1},~\dfrac{p_{-1}}{q_{-1}}=\dfrac{1}{0}.
$$

Gọi số bước của nhóm thứ $k$ là $t_k$, thì theo quan hệ giữa số bước và đầu mút:

$$
\dfrac{p_k}{q_k} = \dfrac{t_kp_{k-1}+p_{k-2}}{t_kq_{k-1}+q_{k-2}}.
$$

Theo [công thức truy hồi](./continued-fraction.md#递推关系) của phân số liên tục:

$$
\dfrac{p_k}{q_k} = [t_0,t_1,\cdots,t_k].
$$

Phân số cuối cùng là:

$$
\dfrac{p}{q} = \dfrac{p_k+p_{k-1}}{q_k+q_{k-1}} = [t_0,t_1,\cdots,t_{n-1},t_n,1].
$$

Vì vậy, trong [biểu diễn phân số liên tục đơn giản](./continued-fraction.md#简单连分数) có đuôi là 1, bỏ số 1 cuối, các hạng trước đó mã hóa đường đi từ gốc đến nút trong cây Stern–Brocot. Các hạng chẵn (tính từ 0) là các cạnh đi sang phải, hạng lẻ là các cạnh đi sang trái.

Biểu diễn phân số liên tục của hữu tỉ có thể tìm bằng thuật toán Euclid, nên độ phức tạp của thuật toán tìm phân số dựa trên phân số liên tục là $O(\log\min\{p,q\})$.

???+ example "Tìm phân số dựa trên phân số liên tục"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/stern-brocot/fraction-finding-3.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/stern-brocot/fraction-finding-3.py:core"
        ```

Dựa trên phân số liên tục, có thể biểu diễn đơn giản cha và con của một nút. Với nút $[t_0,t_1,\cdots,t_n,1]$, nút cha là nút lùi một bước theo hướng di chuyển cuối cùng: nếu $t_k>1$ thì cha là $[t_0,t_1,\cdots,t_n - 1,1]$; nếu không thì cha là $[t_0,t_1,\cdots,t_{n-1},1]$. Hai con lần lượt là $[t_0,t_1,\cdots,t_n+1,1]$ và $[t_0,t_1,\cdots,t_n,1,1]$; con trái/phải phụ thuộc vào chẵn lẻ của $n$.

## Cây Calkin–Wilf

Một cấu trúc đơn giản hơn để lưu hữu tỉ dương là cây Calkin–Wilf. Thường được vẽ như sau:

![pic](./images/calkin-wilf-tree.svg)

Nút gốc là $\dfrac{1}{1}$. Với phân số $\dfrac{p}{q}$ ở một nút, con trái/phải lần lượt là $\dfrac{p}{p+q}$ và $\dfrac{p+q}{q}$. Tương tự cây Stern–Brocot, mọi phân số đều tối giản và chứa tất cả các phân số tối giản dương đúng một lần.

### Quan hệ với phân số liên tục

Khác với cây Stern–Brocot, cây Calkin–Wilf không phải cây nhị phân tìm kiếm, nên không dùng để tìm kiếm nhị phân hữu tỉ.

Trong cây Calkin–Wilf, khi $p>q$, nút cha của $\dfrac{p}{q}$ là $\dfrac{p-q}{q}$; khi $p<q$, là $\dfrac{p}{q-p}$. Với trường hợp thứ nhất, từ $\dfrac{p}{q}$ đi lên theo nhánh phải của cha liên tiếp cho đến khi tử không còn lớn hơn mẫu, khi đó phân số tại nút là $\dfrac{p\bmod q}{q}$, số bước là $\left\lfloor\dfrac{p}{q}\right\rfloor$. Với trường hợp thứ hai, đi lên theo nhánh trái cho đến khi mẫu không còn lớn hơn tử, khi đó phân số là $\dfrac{p}{q\bmod p}$, số bước là $\left\lfloor\dfrac{q}{p}\right\rfloor$.

Dùng ngôn ngữ phân số liên tục, gọi phần dư tại nút hiện tại là $s_k$, thì đi lên theo nhánh phải $\lfloor s_k\rfloor$ lần đến phân số $\dfrac{1}{s_{k+1}}$, rồi đi lên theo nhánh trái $\lfloor s_{k+1}\rfloor$ lần đến phân số $s_{k+2}$. Vì vậy, đường đi từ $s_0=\dfrac{p}{q}$ lên gốc $\dfrac{1}{1}$ được mã hóa bởi phân số liên tục $[t_0,t_1,\cdots,t_n,1]$: bỏ $1$ cuối, các hạng chẵn (tính từ 0) là số lần đi lên nhánh phải của cha, các hạng lẻ là số lần đi lên nhánh trái.

Với nút chứa $\dfrac{p}{q}=[t_0,t_1,\cdots,t_n,1]$, nút cha có biểu diễn:

1.  Khi $t_0>0$, cha là $\dfrac{p-q}{q}=[t_0 - 1, t_1, \cdots, t_n,1]$;
2.  Khi $t_0=0$ và $t_1>1$, cha là $\dfrac{p}{q-p} = [0, t_1 - 1, t_2, \cdots, t_n,1]$;
3.  Khi $t_0=0$ và $t_1=1$, cha là $\dfrac{p}{q-p} = [t_2, t_3, \cdots, t_n,1]$.

Ngược lại, hai con là $\dfrac{p+q}{q}=[t_0+1,t_1,\cdots,t_n,1]$ và $\dfrac{p}{p+q}=[0,1,t_0,t_1,\cdots,t_n,1]$. Với biểu diễn của con thứ hai, nếu $t_0=0$ thì hiểu là $[0,1+t_1,\cdots,t_n,1]$.

### Quan hệ với cây Stern–Brocot

Tương tự liên hệ với phân số liên tục: trên cây Stern–Brocot, các nút trên đường đi thể hiện quan hệ truy hồi của các phân số hội tụ, còn trên cây Calkin–Wilf thể hiện quan hệ truy hồi của phần dư. Biểu diễn phân số liên tục của một phân số là duy nhất, nên mã đường đi từ nút đó lên gốc trong cây Calkin–Wilf trùng với mã đường đi từ gốc đến nút đó trong cây Stern–Brocot. Tuy nhiên do hướng đường đi ngược nhau, nên dù cùng tầng chứa cùng các phân số, vị trí của chúng khác nhau.

Nếu duyệt [BFS](../../graph/bfs.md) mỗi cây và đánh số nút, gốc là 1, thì con trái/phải của nút $v$ là $2v$ và $2v+1$. Từ biểu diễn nhị phân của số hiệu, bỏ bit 1 đầu, mỗi bit 1 tương ứng đi sang phải, mỗi bit 0 tương ứng đi sang trái. Trên cây Calkin–Wilf, số hiệu của nút chứa phân số $[t_0,t_1,\cdots,t_n,1]$ là

$$
1\cdots \underbrace{0\cdots 0}_{t_3}\underbrace{1\cdots 1}_{t_2}\underbrace{0\cdots 0}_{t_1}\underbrace{1\cdots 1}_{t_0}.
$$

Tương ứng, trên cây Stern–Brocot, số hiệu của nút chứa phân số $[t_0,t_1,\cdots,t_n,1]$ là

$$
1\underbrace{1\cdots 1}_{t_0}\underbrace{0\cdots 0}_{t_1}\underbrace{1\cdots 1}_{t_2}\underbrace{0\cdots 0}_{t_3}\cdots.
$$

Bỏ bit 1 đầu, các bit còn lại chính là số thứ tự từ trái sang phải của cùng tầng (tính từ 0). Suy ra thứ tự các phân số cùng tầng giữa hai cây là hoán vị đảo bit (bit-reversal permutation), tức hoán vị của $0\sim (2^k-1)$ khi đảo ngược các bit của chỉ số (đệm thêm bit 0 đầu).

Vì vậy, đôi khi các nút trên cây Stern–Brocot được đánh số theo số hiệu nút tương ứng trên cây Calkin–Wilf, như hình:

![pic](./images/stern-brocot-index.svg)

Cách đánh số này có thể dựng đệ quy: gốc là 1, mỗi lần đi trái thì thay bit đầu 1 thành 10, đi phải thì thay thành 11. Đọc số hiệu từ phải sang trái sẽ thu được đường đi từ gốc đến nút đó.

### Dãy Stern song nguyên tử

Sắp các phân số trong cây Calkin–Wilf theo thứ tự BFS, hoặc sắp các phân số trong cây Stern–Brocot theo số hiệu như hình trên, ta được dãy:

$$
\frac{1}{1},~\dfrac{1}{2},~\dfrac{2}{1},~\dfrac{1}{3},~\dfrac{3}{2},~\dfrac{2}{3},~\dfrac{3}{1},~\dfrac{1}{4},~\dfrac{4}{3},~\dfrac{3}{5},~\dfrac{5}{2},\cdots.
$$

Từ cách xây cây Calkin–Wilf, có thể chứng minh hai phân số kề nhau trong dãy này luôn thỏa mẫu của phân số trước bằng tử của phân số sau. Lấy riêng tử số tạo thành dãy Stern song nguyên tử (Stern diatomic sequence, [OEIS A002487](https://oeis.org/A002487)), cũng gọi là dãy Stern–Brocot (Stern–Brocot sequence). Dãy trên được đánh số từ 1, và bổ sung quy ước số thứ 0 là 0.

Gọi $a_n$ là phần tử thứ $n$ của dãy Stern song nguyên tử, thì có truy hồi:

$$
\begin{aligned}
a_{2n} &= a_n,\\
a_{2n+1} &= a_n + a_{n+1}. 
\end{aligned}
$$

Điểm khởi đầu là $a_0=0$ và $a_1=1$. Tính $a_n$ trực tiếp bằng truy hồi có độ phức tạp $O(\log^2n)$, chưa tốt. Cách tốt hơn là xem $a_n$ như tử của phân số có số hiệu $n$ trên cây Calkin–Wilf, dùng quan hệ truy hồi dựa trên phân số liên tục ở trên, đạt $O(\log n)$.

## Dãy Farey

Dãy Farey có các đặc trưng rất giống cây Stern–Brocot. Gọi **dãy Farey bậc $n$** (Farey sequence of order $n$) là $F_n$, là dãy các phân số tối giản nằm trong $[0,1]$ với mẫu không vượt $n$, sắp theo thứ tự tăng:

$$
\begin{array}{lllllllllllll}
F_1=\bigg\{&\dfrac{0}{1},&&&&&&&&&&\dfrac{1}{1}&\bigg\}\\\\
F_2=\bigg\{&\dfrac{0}{1},&&&&&\dfrac{1}{2},&&&&&\dfrac{1}{1}&\bigg\}\\\\
F_3=\bigg\{&\dfrac{0}{1},&&&\dfrac{1}{3},&&\dfrac{1}{2},&&\dfrac{2}{3},&&&\dfrac{1}{1}&\bigg\}\\\\
F_4=\bigg\{&\dfrac{0}{1},&&\dfrac{1}{4},&\dfrac{1}{3},&&\dfrac{1}{2},&&\dfrac{2}{3},&\dfrac{3}{4},&&\dfrac{1}{1}&\bigg\}\\\\
F_5=\bigg\{&\dfrac{0}{1},&\dfrac{1}{5},&\dfrac{1}{4},&\dfrac{1}{3},&\dfrac{2}{5},&\dfrac{1}{2},&\dfrac{3}{5},&\dfrac{2}{3},&\dfrac{3}{4},&\dfrac{4}{5},&\dfrac{1}{1}&\bigg\}
\end{array}
$$

Theo định nghĩa, dãy Farey tự nhiên thỏa tính đơn điệu, tối giản và đầy đủ. Như hình trên, các phân số mới thêm vào $F_k$ so với $F_{k-1}$ luôn là trung vị của hai phân số kề nhau trong $F_{k-1}$.

Thuật toán xây cây Stern–Brocot ở trên cũng áp dụng để xây dãy Farey. Vì cây Stern–Brocot chứa mọi phân số tối giản, chỉ cần sửa điều kiện biên theo ràng buộc mẫu là được mã xây dãy Farey. Có thể xem $F_n$ là một dãy con của dãy Stern–Brocot bậc $n-1$.

???+ example "Xây dãy Farey"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/stern-brocot/farey-build.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/stern-brocot/farey-build.py:core"
        ```

Xây trực tiếp dãy Farey có độ phức tạp $O(|F_n|)=O(n^2)$.

### Độ dài dãy và tìm phân số

Độ dài dãy Farey có thể tính đệ quy. So với $F_{n-1}$, các phân số mới trong $F_n$ có mẫu bằng $n$, tử không vượt $n$ và nguyên tố cùng nhau với $n$, do đó:

$$
\begin{aligned}
|F_n| &= |F_{n-1}| + \varphi(n) = 1 + \sum_{k=1}^n\varphi(k).
\end{aligned}
$$

Ở đây $\varphi(n)$ là [hàm Euler](./euler-totient.md). Biểu thức này có thể tính $O(n)$ bằng [sàng tuyến tính](./sieve.md#筛法求欧拉函数), hoặc giảm xuống $O(n^{2/3})$ bằng [sàng Dujiao](./du.md#问题一).

So với việc chỉ tính độ dài, tình huống phổ biến hơn là cần tìm thứ tự của một phân số $r=\dfrac{p}{q}$ trong $F_k$. Tương đương tính:

$$
1 + \sum_{k = 1}^n\sum_{i=1}^{\lfloor rk\rfloor}[i\perp k] = 1 + \sum_{d=1}^n\mu(d)\sum_{j=1}^{\lfloor n/d\rfloor}\lfloor rj\rfloor.
$$

Để được vế phải, sử dụng [phép đảo Möbius](./mobius.md). Kết hợp sàng tuyến tính và liệt kê ước, có thể tiền xử lý $O(n)$ và trả lời $O(n\log n)$; kết hợp sàng Dujiao và [thuật toán gần Euclid](./euclidean.md) có thể tiền xử lý $O(n^{2/3})$ và trả lời $O(\sqrt n\log n)$.

Ngược lại, biết thứ tự để tìm phân số, có thể nhị phân trên số thực trong $[0,1]$, hoặc [nhị phân](#快速算法) trên cây Stern–Brocot. Cách đầu có thể bị giới hạn bởi sai số số thực, số lần hỏi là $O(\log V)$ với $V$ là độ chính xác; cách sau không bị sai số, nhưng số lần hỏi là $O(\log^2n)$.

### Lân cận Farey

Nếu $\dfrac{a}{b}$ và $\dfrac{c}{d}$ kề nhau trong một dãy Farey, gọi chúng là **lân cận Farey** (Farey neighbors), hay cặp Farey (Farey pair).

Giả sử $\dfrac{a}{b}<\dfrac{c}{d}$. Theo cách xây dãy Farey, trong hai phân số kề nhau, phân số xuất hiện sau là trung vị của phân số kia với lân cận trước đó; do đó hai lân cận Farey cũng kề nhau trong một dãy Stern–Brocot nào đó, và theo kết luận ở mục [tính tối giản](#最简性) ta có

$$
bc-ad=1.
$$

Ngược lại, đây cũng là điều kiện đủ để hai phân số tối giản thực sự là lân cận Farey. Ta chứng minh điều này. Giả sử $\dfrac{a}{b}$ có mẫu lớn hơn; thì cả hai phân số đều nằm trong $F_b$. Gọi $\dfrac{e}{f}$ là phân số đứng bên phải $\dfrac{a}{b}$ trong $F_b$, theo tính cần thiết ở trên có $be-af=1$. Nhưng [phương trình đồng dư tuyến tính](./linear-equation.md) $bx-ay=1$ chỉ có một nghiệm nguyên dương $y\le b$, nên $(e,f)=(c,d)$.

Thực ra, vì phân số tối giản xuất hiện tiếp theo giữa hai phân số là $\dfrac{a+c}{b+d}$, nên $\dfrac{a}{b}$ và $\dfrac{c}{d}$ kề nhau trong các dãy Farey bậc từ $\max\{b,d\}$ đến $(b+d-1)$.

Quan hệ lân cận Farey với mẫu không quá $9$ như hình:

![](./images/farey-diagram-ford-circle.svg)

Các vòng tròn trong hình gọi là [vòng tròn Ford](https://en.wikipedia.org/wiki/Ford_circle): với mỗi phân số tối giản $\dfrac{p}{q}$ trong $[0,1]$, vẽ một đường tròn tâm $\left(\dfrac{p}{q},\dfrac{1}{2q^2}\right)$ và bán kính $\dfrac{1}{2q^2}$. Hình cho thấy, hai vòng tròn chỉ có thể tiếp xúc hoặc rời nhau, và tiếp xúc khi và chỉ khi hai phân số là lân cận Farey. Hơn nữa, với hai vòng tròn tiếp xúc bất kỳ, luôn tồn tại duy nhất vòng tròn thứ ba tiếp xúc với cả hai; phân số tương ứng với vòng tròn đó chính là trung vị của hai phân số ban đầu.

Để kiểm tra hai vòng tròn tiếp xúc tương ứng với lân cận Farey, tính khoảng cách giữa hai tâm:

$$
\left(\dfrac{a}{b}-\dfrac{c}{d}\right)^2 + \left(\dfrac{1}{2b^2}-\dfrac{1}{2d^2}\right)^2 = \left(\dfrac{1}{2b^2}+\dfrac{1}{2d^2}\right)^2+\frac{(bc-ad)^2-1}{b^2d^2}.
$$

Vì hai phân số tối giản khác nhau nên $|bc-ad|\ge 1$, do đó hai vòng tròn chỉ có thể tiếp xúc hoặc rời nhau. Và chúng tiếp xúc khi và chỉ khi $|bc-ad|=1$, tương đương hai phân số là lân cận Farey.

Cuối cùng, tính số lượng lân cận Farey. Ngoại trừ $\left(\dfrac{0}{1},\dfrac{1}{1}\right)$, các cặp lân cận Farey còn lại có mẫu khác nhau. Giả sử $\dfrac{p}{q}$ là phân số có mẫu lớn hơn, thì phân số còn lại có thể giải từ phương trình Diophantine

$$
qx-py=\pm 1
$$

Mỗi phương trình có đúng một nghiệm nguyên dương với $y<q$, lần lượt tương ứng với lân cận bên trái và bên phải của $\dfrac{p}{q}$. Do đó, mỗi phân số thực sự trong $(0,1)$ có đúng hai lân cận Farey có mẫu nhỏ hơn nó, cộng thêm $\dfrac{0}{1}$ và $\dfrac{1}{1}$, suy ra trong $F_n$ có tổng cộng $(2|F_n|-3)$ cặp lân cận Farey.

Tất nhiên, hai phân số $\dfrac{a}{b}<\dfrac{c}{d}$ tìm được trong quá trình này chính là lân cận trái/phải khi chèn $\dfrac{p}{q}$, nên chúng vốn là lân cận Farey và $\dfrac{p}{q}$ là trung vị của chúng. Nếu $\dfrac{p}{q}=[t_0,t_1,\cdots,t_n,1]$, thì hai lân cận Farey có mẫu nhỏ hơn là $[t_0,t_1,\cdots,t_n]$ và $[t_0,t_1,\cdots,t_{n-1}]$.

Để tính các lân cận Farey khác của $\dfrac{p}{q}$, chỉ cần dùng [thuật toán Euclid mở rộng](./bezouts.md#两个变量的情形) để liệt kê nghiệm thỏa điều kiện.

### Quan hệ truy hồi

Dãy Farey có một quan hệ truy hồi gọn, có thể liệt kê từ trái sang phải toàn bộ phân số của $F_n$.

Trước hết, phân tích trên chỉ ra rằng trong $F_n$, hạng mới $\dfrac{p}{n}$ luôn là trung vị của hai phân số kề nhau. Thực ra, điều này đúng với mọi phân số (trừ hai đầu mút) trong $F_n$. Giả sử $\dfrac{a}{b}<\dfrac{p}{q}<\dfrac{c}{d}$, theo điều kiện cần và đủ của lân cận Farey luôn có:

$$
bp-aq = 1 = cq-dp \iff \frac{p}{q} = \dfrac{a+c}{b+d}.
$$

Tuy nhiên, trong trường hợp chung, $\dfrac{a+c}{b+d}$ có thể cần rút gọn.

Từ quan sát này xây quan hệ truy hồi sau. Giả sử $\dfrac{a}{b}$ và $\dfrac{p}{q}$ đã biết, cần tìm $\dfrac{c}{d}$. Khi đó tồn tại $k$ sao cho

$$
a+c = kp,\ b+d=kq
$$

vì

$$
\frac{kp-a}{kq-b} - \dfrac{p}{q} = \dfrac{bp-aq}{q(kq-b)} = \dfrac{1}{q(kq-b)}
$$

giảm dần khi $k$ tăng, và phân số kề ngay $\dfrac{p}{q}$ là phân số có sai khác nhỏ nhất trong số các phân số thỏa $kq-d\le n$, nên bắt buộc

$$
k = \left\lfloor\dfrac{n+b}{q}\right\rfloor.
$$

Có thể kiểm chứng phân số thu được chắc chắn thuộc $F_n$. Do đó, tử và mẫu của $F_n$ thỏa truy hồi:

$$
\begin{aligned}
p_k &= \left\lfloor\frac{n + q_{k-2}}{q_{k-1}}\right\rfloor p_{k-1} - p_{k-2},\\
q_k &= \left\lfloor\frac{n + q_{k-2}}{q_{k-1}}\right\rfloor q_{k-1} - q_{k-2}.
\end{aligned}
$$

Điểm khởi đầu là $(p_0,q_0)=(0,1)$ và $(p_1,q_1)=(1,n)$.

## Bài tập

Các bài toán lấy tài liệu này làm nền:

-   [LOJ 6685. 迷宫](https://loj.ac/p/6685)
-   [UVa 10077. The Stern-Brocot Number System](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=33&page=show_problem&problem=1018)
-   [Luogu P8058. \[BalkanOI2003\] Farey 序列](https://www.luogu.com.cn/problem/P8058)
-   [UVa 12995. Farey Sequence](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=862&page=show_problem&problem=4878)
-   [UVa 10408. Farey Sequences](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=16&page=show_problem&problem=1349)
-   [UVa 12438. Farey Polygon](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=279&page=show_problem&problem=3869)
-   [AtCoder ARC123F. Insert Addition](https://atcoder.jp/contests/arc123/tasks/arc123_f)

Các bài cần nhị phân trên cây Stern–Brocot:

-   [AtCoder ABC333G. Nearest Fraction](https://atcoder.jp/contests/abc333/tasks/abc333_g)
-   [SPOJ DIVCNT1 - Counting Divisors](https://www.spoj.com/problems/DIVCNT1/)
-   [SPOJ AFS3 - Amazing Factor Sequence (hard)](https://www.spoj.com/problems/AFS3/)

## Tài liệu tham khảo và chú thích

-   [Stern–Brocot tree - Wikipedia](https://en.wikipedia.org/wiki/Stern%E2%80%93Brocot_tree)
-   [Calkin–Wilf tree - Wikipedia](https://en.wikipedia.org/wiki/Calkin%E2%80%93Wilf_tree)
-   [Farey sequence - Wikipedia](https://en.wikipedia.org/wiki/Farey_sequence)

**Một phần nội dung trang này được dịch từ bài viết [Дерево Штерна-Броко. Ряд Фарея](http://e-maxx.ru/algo/stern_brocot_farey) và bản dịch tiếng Anh [The Stern-Brocot Tree and Farey Sequences](https://cp-algorithms.com/others/stern_brocot_tree_farey_sequences.html). Bản tiếng Nga có giấy phép Public Domain + Leave a Link; bản tiếng Anh có giấy phép CC-BY-SA 4.0. Trang này còn dịch một phần từ bài viết [Continued fractions](https://cp-algorithms.com/algebra/continued-fractions.html), giấy phép CC-BY-SA 4.0. Nội dung đều đã chỉnh sửa.**

[^mediant]: Tên dịch lấy theo bản dịch của Trương Minh Nghiêu, Trương Phàm cho “Concrete Mathematics” mục 4.5.
