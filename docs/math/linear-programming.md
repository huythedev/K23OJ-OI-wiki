author: Ir1d, YZircon, huhaoo, QAQAutoMaton, Enter-tainer, Marcythm, sshwy, partychicken, Konano, H-J-Granger, baker221, isdanni, ksyx

## Giới thiệu

Quy hoạch tuyến tính (linear programming, LP) là lĩnh vực nghiên cứu bài toán tối ưu tuyến tính dưới ràng buộc tuyến tính, là một nhánh của vận trù học và có nhiều ứng dụng. Một số trường hợp đặc biệt (như luồng mạng, đa hàng hóa) có thể xuất hiện trong thi đấu. Tuy nhiên các bài chỉ có thể giải bằng LP rất hiếm; đa số có thể mô hình hóa luồng và giải hiệu quả hơn.

### Một ví dụ đơn giản

Một bài toán viết được dưới dạng LP cần có các ràng buộc tuyến tính và hàm mục tiêu tuyến tính.

Xét ví dụ:

???+ example "Ví dụ"
    Mỗi ngày người làm bữa sáng có thể làm một số lượng nhất định bánh bao và quẩy, cả hai đều được ưa chuộng. Để tối đa hóa lợi nhuận, người thợ muốn làm nhiều nhất có thể, nhưng bị giới hạn bởi nguyên liệu, thời gian… Bảng sau là lượng nguyên liệu, thời gian và lợi nhuận mỗi phần:
    
    |  Bữa sáng | Dầu | Bột | Thời gian | Lợi nhuận |
    | :-: | :-: | :-: | :-: | :-: |
    |  Bánh bao | $4$ | $7$ | $8$ | $5$ |
    |  Quẩy | $7$ | $3$ | $6$ | $6$ |
    
    Giả sử mỗi ngày tối đa có $66$ đơn vị dầu và $60$ đơn vị bột, cùng tối đa $96$ đơn vị thời gian. Làm bao nhiêu bánh bao/quẩy để lợi nhuận lớn nhất?

Gọi $x_1,x_2$ là số lượng bánh bao/quẩy. Ràng buộc dầu:

$$
4x_1 + 7x_2 \le 66.
$$

Tương tự cho bột và thời gian:

$$
\begin{aligned}
7x_1 + 3x_2 &\le 60,\\
8x_1 + 6x_2 &\le 96.
\end{aligned}
$$

Không thể làm số âm, nên

$$
x_1,x_2\ge 0.
$$

Mục tiêu tối đa hóa lợi nhuận:

$$
z = 5x_1 + 6x_2.
$$

Đây là một LP điển hình: hàm mục tiêu tuyến tính, ràng buộc là các đẳng thức/bất đẳng thức tuyến tính.

### Phương pháp đồ thị

Với chỉ 2 biến, có thể giải bằng hình học.

Bài toán:

$$
\begin{aligned}
\max_{x_1,x_2}\;& z = 5x_1 + 6x_2 \\
\text{subject to } & 4x_1 + 7x_2 \le 66,\\
& 7x_1 + 3x_2 \le 60,\\
& 8x_1 + 6x_2 \le 96,\\
& x_1,x_2\ge 0
\end{aligned}
$$

Miền khả thi là giao của các nửa mặt phẳng dưới ba đường thẳng và góc phần tư thứ nhất, như vùng xanh:

![](images/linear-programming.svg)

Cần tối đa hóa $z=5x_1+6x_2$. Xem $5x_1+6x_2=z$ là một họ đường thẳng song song; $z$ càng lớn thì đường càng dịch lên phải. Dịch đến vị trí tới hạn khi vừa chạm miền xanh; khi đó $z$ lớn nhất.

Hình cho thấy điểm đỏ là giao của $4x_1 + 7x_2 = 66$ và $7x_1 + 3x_2 = 60$. Giải ra $(6,6)$. Đây là nghiệm tối ưu duy nhất. Lợi nhuận tối đa $z=66$.

Với hơn 2 biến, đồ thị không dùng được, nhưng quan sát vẫn đúng: mỗi bất đẳng thức là một “nửa không gian”, giao là một “đa diện lồi”. Nghiệm tối ưu luôn ở một “đỉnh”. Mở rộng lên không gian cao dẫn tới phương pháp đơn hình.

Một điểm đáng chú ý: số lượng bánh bao/quẩy là số nguyên. Bài này không ràng buộc nguyên, nhưng nghiệm tối ưu là nguyên nên vẫn hợp lệ. Nhiều bài khác nghiệm tối ưu không nguyên, đó là quy hoạch nguyên chứ không phải LP. Cuối bài sẽ nói sơ về loại này.

## Khái niệm cơ bản

### Bài toán quy hoạch tuyến tính

Một LP $P$ gồm:

-   Hàm mục tiêu tuyến tính:

    $$
    f(x_1,x_2,\cdots,x_n)=c_1x_1+c_2x_2+\cdots+c_nx_n
    $$

    với $c_i\in\mathbf R$ là hằng số;

-   Ràng buộc tuyến tính:

    $$
    g_j(x_1,x_2,\cdots,x_n)=a_{j1}x_1+a_{j2}x_2+\cdots+a_{jn}x_n \le (=,\ge) b_j
    $$

    với $a_{ji},b_j\in\mathbf R$ là hằng số.

LP là tối đa hóa hoặc tối thiểu hóa mục tiêu dưới các ràng buộc. Một nghiệm thỏa ràng buộc là **nghiệm khả thi**; nghiệm khả thi đạt giá trị tối ưu là **nghiệm tối ưu**.

### Dạng chuẩn

Để tiện mô tả, quy định dạng chuẩn:

$$
\begin{aligned}
\min_{\{x_i\}}\;& \sum_{i=1}^n c_ix_i \\
\text{subject to }& \sum_{i=1}^n a_{ji}x_i = b_i \ge 0,~j=1,\cdots,m,\\
& x_i \ge 0,~i = 1,\cdots,n.
\end{aligned}
$$

Tức là bài toán tối thiểu, mọi biến không âm, chỉ có ràng buộc đẳng thức với vế phải không âm. Viết gọn bằng [ma trận](./linear-algebra/matrix.md):

$$
\max\{c^Tx : Ax = b \ge 0,~ x\ge 0\}.
$$

Trong đó $x\in\mathbf R^n$ là biến, $b\in\mathbf R^m$, $A\in\mathbf R^{m\times n}$ là hằng. Quy mô là số biến và số ràng buộc.

???+ tip "Bất đẳng thức vector"
    Nhiều chỗ có $b \ge 0$. Với $x,y\in\mathbf R^n$, $x\le y$ nghĩa là $\forall i(x_i\le y_i)$, so sánh từng thành phần. Đây là quan hệ [bán thứ tự](./order-theory.md#二元关系), có thể có hai vector không so sánh được.

Dạng chuẩn chỉ để tiện, mọi LP có thể đổi về một trong sáu dạng:

$$
\begin{aligned}
&\min\{c^Tx : Ax = b,~ x\ge 0\},\\
&\min\{c^Tx : Ax \ge b\},\\
&\min\{c^Tx : Ax \ge b,~ x\ge 0\},\\
&\max\{c^Tx : Ax = b,~ x\ge 0\}, \\
&\max\{c^Tx : Ax \le b\},\\
&\max\{c^Tx : Ax \le b,~ x\ge 0\}.\\
\end{aligned}
$$

Có thể chuyển giữa các dạng bằng:

1.  Đổi dấu $c$ để chuyển tối đa hóa ↔ tối thiểu hóa.
2.  Đổi dấu ràng buộc để đổi hướng bất đẳng thức, hoặc làm vế phải không âm.
3.  Ràng buộc đẳng thức thay bằng hai bất đẳng thức ngược chiều.
4.  Bất đẳng thức $a_j^Tx \le(\ge) b_j$ thêm biến slack $s_j\ge 0$ thành $a_j^Tx +(-) s_j = b_j$.
5.  Biến không bị ràng buộc không âm thay bằng hiệu của hai biến không âm: $x_j = x^+_j - x^-_j$.

Quy mô tăng không quá 2 lần, nghiệm chuyển đổi dễ dàng. Vì vậy luôn có thể chuyển bài toán về dạng chuẩn rồi giải.

??? example "Ví dụ"
    Xét
    
    $$
    \begin{aligned}
    \max\;& 3x_1 - 2x_2 + x_3 \\
    \text{subject to }& 2x_1 + 3x_2 + 4x_3 \ge 1,\\
    & 3x_1 + 4x_2 \le 5,\\
    & 5x_2 - x_3 = -1, \\
    & x_1, x_2 \ge 0.
    \end{aligned}
    $$
    
    Áp dụng thao tác 1,2,3 thành $\min\{c^Tx : Ax \ge b\}$:
    
    $$
    \begin{aligned}
    \min\;& -3x_1 + 2x_2 - x_3 \\
    \text{subject to }& 2x_1 + 3x_2 + 4x_3 \ge 1,\\
    & -3x_1 - 4x_2 \ge -5,\\
    & 5x_2 - x_3 \ge -1, \\
    & -5x_2 + x_3 \ge 1, \\
    & x_1 \ge 0,\\
    & x_2 \ge 0.
    \end{aligned}
    $$
    
    Áp dụng thao tác 4,5 thành $\max\{c^Tx : Ax = b,~ x\ge 0\}$:
    
    $$
    \begin{aligned}
    \max\;& 3x_1 - 2x_2 + x^+_3 - x^-_3 \\
    \text{subject to }& 2x_1 + 3x_2 + 4x^+_3 - 4x^-_3 - x_4 = 1,\\
    & 3x_1 + 4x_2 + x_5 = 5,\\
    & 5x_2 - x^+_3 + x^-_3 = -1, \\
    & x_1, x_2, x^+_3, x^-_3, x_4, x_5 \ge 0.
    \end{aligned}
    $$

### Miền khả thi và nghiệm

Tập các nghiệm khả thi $\mathcal D\subseteq\mathbf R^n$ gọi là **miền khả thi**. Về hình học, mỗi bất đẳng thức $a_j^T x \le b_j$ là một nửa không gian, mỗi đẳng thức $a^T_jx = b_j$ là một siêu phẳng. Do đó miền khả thi là giao của hữu hạn nửa không gian và siêu phẳng. Trong tối ưu[^poly-names], đối tượng này gọi là **đa diện** (polyhedron). Đa diện luôn lồi và đóng, có thể không bị chặn. Nếu bị chặn, gọi là **đa giác lồi/đa bào** (polytope).

???+ example "Ví dụ về đa diện"
    Một số ví dụ:
    
    1.  Tập rỗng $\varnothing$, gọi là **đa bào rỗng** (nullitope), quy ước bậc $-1$.
    2.  **Không gian affine**, tức giao của các siêu phẳng $\{x\in\mathbf R^n:Ax = b\}$, là nghiệm của hệ tuyến tính. Nếu vô nghiệm thì rỗng; nếu có nghiệm thì viết dạng $x_0+V$, với $V$ là không gian con tuyến tính bậc $n-\operatorname{rank}(A)$. Siêu phẳng là trường hợp đặc biệt.
    3.  **Nón đa diện** (polyhedral cone): tập tổ hợp tuyến tính không âm của hữu hạn điểm $\{x_i\}$, tương đương $\{x\in\mathbf R^n : Ax\le 0\}$. Nửa không gian là trường hợp đặc biệt.
    4.  **Đa bào**: đa diện bị chặn. Các bậc $-1,0,1,2,3$ tương ứng rỗng, điểm, đoạn thẳng, đa giác, đa diện. Một tập là đa bào khi và chỉ khi nó là bao lồi của hữu hạn điểm. Một đa bào bậc $k$ cần ít nhất $k+1$ điểm sinh.
    5.  **Đơn hình** (simplex): đa bào bậc $k$ sinh bởi đúng $k+1$ điểm. Ví dụ bậc $-1,0,1,2,3$ là rỗng, điểm, đoạn thẳng, tam giác, tứ diện. Đơn hình chuẩn là $\{x\in\mathbf R^k:x_i\ge 0,~\sum_ix_i=1\}$, mọi đơn hình đều có thể đưa về dạng này bằng biến đổi affine.
    
    Mọi đa diện là [tổng Minkowski](../geometry/convex-hull.md#闵可夫斯基和) của một nón đa diện (phần vô hạn) và một đa bào (phần hữu hạn). Nón này là duy nhất: với $\{x\in\mathbf R^n:Ax\le b\}$ thì nón là $\{x\in\mathcal R^n:Ax\le 0\}$.

Nghiệm LP liên quan chặt chẽ đến cấu trúc đa diện. Với đa diện $\mathcal D\in\mathbf R^n$ và vector $c\in\mathbf R^n\setminus\{0\}$, xét bài toán (tối đa hóa):

$$
\max\{c^Tx:x\in\mathcal D\}.
$$

Hình học: dịch siêu phẳng $H:c^Tx=z$ theo hướng $c$ để $z$ lớn nhất mà vẫn cắt $\mathcal D$. Có ba khả năng:

-   $\mathcal D$ rỗng: bài toán vô nghiệm, gọi là **không khả thi** (infeasible), giá trị tối ưu quy ước $-\infty$.

-   $\mathcal D$ không rỗng nhưng chứa một tia theo hướng $c$: tồn tại $x_0$ sao cho $x_0+tc\in\mathcal D$ với mọi $t\ge 0$. Khi đó $c^Tx$ có thể tăng vô hạn, bài toán **vô hạn** (unbounded), giá trị tối ưu $+\infty$.

-   $\mathcal D$ không rỗng và không chứa tia theo hướng $c$: bài toán **hữu hạn** (bounded). Gọi $z^*$ là giá trị tối ưu. Siêu phẳng $H^*:c^Tx=z^*$ là **siêu phẳng đỡ** (supporting hyperlane). Tập nghiệm tối ưu là $H^*\cap\mathcal D$, gọi là một **mặt** (face) của $\mathcal D$. Ngoài các mặt như vậy, $\varnothing$ và toàn bộ $\mathcal D$ cũng là mặt. Các mặt theo quan hệ bao hàm tạo thành một [lattice](../math/order-theory.md#有向集与格).

    Mặt của đa diện bậc $d$ có bậc từ 0 đến $d$. Bậc 0 là **đỉnh** (vertex), bậc 1 là **cạnh**, bậc $d-1$ là **mặt** (facet). Không phải đa diện nào cũng có đỉnh: các mặt tối tiểu là không gian affine. Với $\mathcal D=\{x\in\mathbf R^n:Ax\le b\}$, bậc mặt tối tiểu là $n-\operatorname{rank}A$.

    Để mô tả phương trình của mặt: nếu một ràng buộc đạt dấu bằng trên mọi $x\in F$ thì gọi là **chặt** (tight) trên mặt đó. Giao của các ràng buộc chặt (dưới dạng đẳng thức) với $\mathcal D$ chính là mặt $F$. Chọn càng nhiều ràng buộc chặt, mặt càng nhỏ.

    Đặc biệt, với dạng chuẩn $\mathcal D=\{x\in\mathbf R^n:Ax=b,~x\ge 0\}$, ma trận $\begin{pmatrix}A\\ I\end{pmatrix}$ có hạng $n$, nên các mặt tối tiểu là đỉnh. Nếu bài toán hữu hạn, nghiệm tối ưu có thể chọn là một đỉnh. Đỉnh xác định bởi $n$ ràng buộc chặt độc lập.

???+ example "Ví dụ"
    Hình dưới: $\mathcal D$ là miền khả thi. Với các hệ số $c_1,c_2,c_3$ trong mục tiêu, tương ứng ba trường hợp: nghiệm tối ưu duy nhất, nhiều nghiệm tối ưu, và vô hạn. Với hai trường hợp đầu, đường đỏ đậm là siêu phẳng đỡ; nghiệm tối ưu là đỉnh $B$ và cạnh $\overline{CD}$. Với trường hợp thứ ba, miền chứa tia theo hướng $c_3$ nên bài toán vô hạn.
    
    ![](./images/lp-feasible.svg)

Bỏ qua trường hợp $c=0$. Khi đó bài toán không thể vô hạn: либо không khả thi, либо giá trị tối ưu $0$ và nghiệm tối ưu là $\mathcal D$. Đây gọi là **LP khả thi** (feasibility LP).

Lưu ý, kiểm tra tính khả thi, tính hữu hạn, hoặc tìm nghiệm khả thi của hệ bất đẳng thức đều khó ngang với giải LP[^reducible]. Ví dụ, chứng minh đối ngẫu mạnh cho thấy giải LP hữu hạn tương đương tìm nghiệm khả thi của một hệ. Vì vậy cách hiệu quả nhất để kiểm tra tính khả thi của hệ bất đẳng thức/đẳng thức với ràng buộc không âm là giải một LP khả thi[^other-methods].

Nếu một ràng buộc không chặt ở mọi mặt của miền khả thi, nó là **ràng buộc dư** (redundant). Trong ví dụ đầu, ràng buộc thời gian là dư. Kiểm tra $a_j^Tx\le b_j$ dư hay không có thể giải $\max\{a_j^Tx:x\in\mathcal D\}$ và so với $b_j$.

## Thuật toán thường dùng

Trong thi đấu, hiếm khi bài chỉ giải được bằng LP. Đa số có thể giải bằng luồng hay thuật toán chuyên dụng.

Các thuật toán phổ biến:

-   [Phương pháp đơn hình](./simplex.md)
-   Phương pháp ellipsoid
-   Phương pháp nội điểm

Đơn hình có độ phức tạp xấu nhất mũ, nội điểm là đa thức, nhưng cả hai thường rất nhanh trong thực tế. Ellipsoid có độ phức tạp đa thức nhưng thường chậm.

Hiện chưa rõ LP có thuật toán thời gian đa thức mạnh hay không.

## Bài toán đối ngẫu

Mỗi LP có một bài toán đối ngẫu. Nghiệm của chúng liên hệ chặt chẽ, giúp hiểu cấu trúc và đôi khi giải nhanh hơn.

Với LP $P$:

$$
\begin{aligned}
\min_{x_1,x_2,x_3}\;& c_1^Tx_1 + c_2^Tx_2 + c_3^Tx_3 \\
\text{subject to }& A_{11}x_1 + A_{12}x_2 + A_{13}x_3 \ge b_1,\\
& A_{21}x_1 + A_{22}x_2 + A_{23}x_3 = b_2,\\
& A_{31}x_1 + A_{32}x_2 + A_{33}x_3 \le b_3,\\
& x_1\ge 0,~ x_3\le 0,
\end{aligned}
$$

đối ngẫu $D$ là:

$$
\begin{aligned}
\max_{y_1,y_2,y_3}\;&b_1^Ty_1+b_2^Ty_2+b_3^Ty_3 \\
\text{subject to }&A_{11}^Ty_1 + A_{21}^Ty_2 + A_{31}^Ty_3\le c_1,\\
&A_{12}^Ty_1 + A_{22}^Ty_2 + A_{32}^Ty_3 = c_2,\\
&A_{13}^Ty_1 + A_{23}^Ty_2 + A_{33}^Ty_3 \ge c_3,\\
&y_1\ge 0,~ y_3\le 0.
\end{aligned}
$$

Biến đối ngẫu $y_1,y_2,y_3$ là các nhân tử Lagrange cho ràng buộc của bài gốc; ngược lại, biến của bài gốc là nhân tử Lagrange của bài đối ngẫu. Đối ngẫu của đối ngẫu là bài gốc.

Bảng đối ứng:

|  Bài tối thiểu |  Bài tối đa |
| :----: | :----: |
| Ràng buộc ≥ |  Biến không âm |
| Ràng buộc ≤ |  Biến không dương |
| Ràng buộc = |  Biến tự do |
| Biến không âm | Ràng buộc ≤ |
| Biến không dương | Ràng buộc ≥ |
| Biến tự do |  Ràng buộc = |
| Hệ số mục tiêu | Vế phải ràng buộc |
| Vế phải ràng buộc | Hệ số mục tiêu |

Đặc biệt, với dạng chuẩn:

$$
\min\{c^Tx:Ax=b,~x\ge 0\}
$$

đối ngẫu là

$$
\max\{b^Ty:A^Ty\le c\}.
$$

### Nguyên lý đối ngẫu

Bài gốc và đối ngẫu đối xứng, nghiệm liên hệ chặt chẽ — gọi là **nguyên lý đối ngẫu**. Ta dùng dạng chuẩn để trình bày.

**Định lý đối ngẫu yếu**: giá trị tối đa của đối ngẫu không vượt giá trị tối thiểu của bài gốc.

???+ note "Định lý đối ngẫu yếu"
    Với mọi $A\in\mathbf R^{m\times n}$, $b\in\mathbf R^m$, $c\in\mathbf R^n$:
    
    $$
    \max\{b^Ty:A^Ty\le c\} \le \min\{c^Tx:Ax=b,~x\ge 0\}.
    $$

??? note "Chứng minh"
    Nếu một trong hai bài không khả thi, bất đẳng thức hiển nhiên. Giả sử cả hai khả thi. Với mọi nghiệm khả thi $x,y$:
    
    $$
    b^Ty = x^TA^Ty \le x^Tc.
    $$
    
    Lấy cực trị hai vế được định lý.

Từ đối ngẫu yếu, có bốn khả năng:

1.  Cả hai không khả thi: $-\infty\le+\infty$.
2.  Gốc không khả thi, đối ngẫu vô hạn: $+\infty\le+\infty$.
3.  Gốc vô hạn, đối ngẫu không khả thi: $-\infty\le-\infty$.
4.  Cả hai hữu hạn.

Đối ngẫu yếu cho suy luận:

???+ note "Hệ quả"
    Bài toán vô hạn khi và chỉ khi nó khả thi và đối ngẫu không khả thi.

Áp dụng cho LP khả thi được Farkas và các biến thể.

???+ note "Bổ đề Farkas"
    Với $A\in\mathbf R^{m\times n}$ và $b\in\mathbf R^n$, đúng đúng một trong hai:
    
    1.  Tồn tại $x\in\mathbf R^n$ sao cho $Ax=b$ và $x\ge 0$;
    2.  Tồn tại $y\in\mathbf R^m$ sao cho $A^T y\ge 0$ và $b^Ty<0$.

??? note "Chứng minh"
    Xét LP $\max\{0:Ax=b,~x\ge 0\}$, đối ngẫu là $\min\{b^Ty:A^Ty\ge 0\}$. Đối ngẫu rõ ràng khả thi vì $0$ là nghiệm. Theo đối ngẫu yếu, hoặc bài gốc khả thi, hoặc đối ngẫu vô hạn. Tương ứng hai trường hợp của Farkas. QED.

Farkas là một dạng [định lý tách siêu phẳng](https://en.wikipedia.org/wiki/Hyperplane_separation_theorem). Trường hợp 1 nói $b$ thuộc nón đa diện sinh bởi các cột của $A$; trường hợp 2 nói tồn tại siêu phẳng qua gốc tách mạnh $b$ khỏi nón đó.

Đối với trường hợp thứ tư, có kết luận mạnh hơn: giá trị tối ưu của gốc và đối ngẫu bằng nhau. Kết hợp, ta có **định lý đối ngẫu mạnh**: nếu một trong hai khả thi, thì giá trị tối ưu bằng nhau.

???+ note "Định lý đối ngẫu mạnh"
    Với mọi $A\in\mathbf R^{m\times n}$, $b\in\mathbf R^m$, $c\in\mathbf R^n$:
    
    $$
    \max\{b^Ty:A^Ty\le c\} = \min\{c^Tx:Ax=b,~x\ge 0\}.
    $$
    
    nếu ít nhất một trong hai tập khả thi.

??? note "Chứng minh"
    Trường hợp còn thiếu trong đối ngẫu yếu là cả hai khả thi. Xét LP khả thi:
    
    $$
    \max\{0:c^Tx \le b^Ty,~Ax=b,~x\ge 0,~A^Ty\le c\}.
    $$
    
    Nếu có nghiệm khả thi $(x^*,y^*)$, thì
    
    $$
    b^Ty^* \le \max\{b^Ty:A^Ty\le c\} \le \min\{c^Tx:Ax=b,~x\ge 0\} \le c^Tx^*,
    $$
    
    nhưng $c^Tx^*\le b^Ty^*$, nên tất cả bằng nhau; suy ra đối ngẫu mạnh và $x^*,y^*$ là tối ưu.
    
    Cần chứng minh LP trên khả thi. Giả sử không. Xét đối ngẫu $DQ$:
    
    $$
    \min\{c^T\mu - b^T\lambda : ct - A^T\lambda \ge 0,~ -bt + A\mu = 0,~t\ge 0,~\mu\ge 0\}.
    $$
    
    Vì $(t,\lambda,\mu)=(0,0,0)$ khả thi, nên nếu $Q$ vô nghiệm thì $DQ$ vô hạn, tức tồn tại $(t^*,\lambda^*,\mu^*)$ sao cho
    
    $$
    c^T\mu^* - b^T\lambda^* <0,~ ct^* - A^T\lambda^* \ge 0,~ -bt^* + A\mu^* = 0,~t^*\ge 0,~\mu^*\ge 0.
    $$
    
    Nếu $t^*>0$, thì $(x,y)=(\mu^*/t^*,\lambda^*/t^*)$ là nghiệm khả thi của $Q$, mâu thuẫn. Vậy $t^*=0$, suy ra
    
    $$
    c^T\mu^* < b^T\lambda^*,~ A^T\lambda^*\le 0,~ A\mu^*=0,~\mu^*\ge 0.
    $$
    
    Vì giả sử cả gốc và đối ngẫu đều khả thi, tồn tại $(x_0,y_0)$ thỏa
    
    $$
    Ax_0 = b,~ x_0\ge 0,~ A^Ty_0\le c,
    $$
    
    suy ra
    
    $$
    0 = (A\mu^*)^Ty_0 = (A^Ty_0)^T\mu^* \le c^T\mu^* < b^T\lambda^* = x_0^TA^T\lambda^* \le 0,
    $$
    
    mâu thuẫn. Vậy $Q$ khả thi, đối ngẫu mạnh đúng.

Từ chứng minh đối ngẫu mạnh có:

???+ note "Hệ quả"
    Nếu $x^*,y^*$ là nghiệm khả thi và thỏa $c^Tx^* = b^Ty^*$ thì $x^*,y^*$ là nghiệm tối ưu của bài gốc và đối ngẫu.

Đối ngẫu mạnh cho thấy, với LP khả thi, chỉ cần giải đối ngẫu là biết giá trị tối ưu.

### Điều kiện bổ sung bù

Trong tối ưu, bổ sung bù là một phần của điều kiện tối ưu. Với LP, nó vừa cần vừa đủ.

**Bổ sung bù** nói rằng: chỉ khi một ràng buộc đạt dấu bằng thì biến tương ứng ở bài đối ngẫu (hoặc bài gốc) mới có thể khác 0. Nói cách khác, một ràng buộc và biến đối ứng không thể cùng “lỏng”.

Với dạng chuẩn:

???+ note "Định lý"
    Cho $x^*$ và $y^*$ là nghiệm khả thi của
    
    $\min\{c^Tx:Ax=b,~x\ge 0\}$ và $\max\{b^Ty:A^Ty\le c\}$.
    
    Khi và chỉ khi bổ sung bù
    
    $$
    x^T(A^Ty-c) = 0
    $$
    
    thì $x^*$ và $y^*$ là nghiệm tối ưu.

??? note "Chứng minh"
    Vì $x^*,y^*$ khả thi:
    
    $$
    b^Ty^* - c^Tx^* = (x^*)^T(A^T y^* - c).
    $$
    
    Do đó bổ sung bù $\Leftrightarrow b^Ty^* = c^Tx^*$, và theo đối ngẫu mạnh, đó là điều kiện tối ưu.

Dạng tổng quát hơn:

???+ note "Định lý"
    Nếu $x^*,y^*$ khả thi của
    
    $\min\{c^Tx:Ax\ge b,~x\ge 0\}$ và $\max\{b^Ty:A^Ty\le c,~y\ge 0\}$,
    
    thì khi và chỉ khi
    
    $$
    x^T(A^Ty-c) = y^T(Ax-b) = 0
    $$
    
    thì $x^*,y^*$ tối ưu.

??? note "Chứng minh"
    Tương tự, chỉ khác viết:
    
    $$
    b^Ty^* - c^Tx^* = (x^*)^T(A^T y^* - c) - (y^*)^T(Ax^*-b).
    $$

Bổ sung bù cho điều kiện đơn giản để kiểm tra tối ưu của một nghiệm khả thi.

### Phương pháp nguyên‑đối ngẫu

Phương pháp **nguyên‑đối ngẫu** (primal-dual) giải LP bằng cách cải thiện dần nghiệm đối ngẫu, qua các bài toán phụ đơn giản.

Với bài gốc chuẩn

$$
(P)\qquad\min\{c^Tx : Ax=b\ge 0,~ x\ge 0\}
$$

và đối ngẫu

$$
(D)\qquad\max\{b^Ty : A^Ty\le c\},
$$

muốn tối ưu chỉ cần tìm $x,y$ khả thi sao cho $x^T(A^Ty-c)=0$. Quy trình:

1.  Bắt đầu từ nghiệm khả thi $y$ của $(D)$, lấy tập ràng buộc chặt:

    $$
    I = \{i : (A^Ty - c)_i = 0\}.
    $$

2.  Nếu tồn tại nghiệm khả thi $x$ của $(P)$ với $x_i>0$ chỉ khi $i\in I$, thì đã tối ưu. Xét bài toán:

    $$
    (RP)\qquad
    \begin{aligned}
    \min_{x,s}\;& \mathbf 1^Ts \\
    \text{subject to } & Ax + s = b, \\
    & x_i \ge 0,~\forall i \in I,\\
    & x_i = 0,~\forall i \notin I,\\
    & s \ge 0.
    \end{aligned}
    $$

3.  Nếu $(RP)$ tối ưu bằng $0$, nghiệm $(x^*,0)$ là nghiệm tối ưu của $(P)$. Nếu không, xét đối ngẫu $(DRP)$:

    $$
    (DRP)\qquad
    \begin{aligned}
    \max_{y}\;& b^Ty \\
    \text{subject to }& \sum_{j}a_{ji}y_j \le 0,~\forall i\in I,\\
    & y \le 1.
    \end{aligned}
    $$

    Theo đối ngẫu mạnh, $b^T\bar y = 1^Ts^*>0$.

4.  Cải thiện nghiệm đối ngẫu: đặt $y' = y + \varepsilon \bar y$, $\varepsilon>0$. Khi đó $b^Ty' > b^Ty$. Chọn $\varepsilon$ lớn nhất sao cho $y'$ vẫn khả thi.

    Với $i\in I$:
    
    $$
    \sum_ja_{ji}y'_j = \sum_ja_{ji}y_j + \varepsilon \sum_ja_{ji}\bar y_j \le c_i,
    $$
    
    luôn thỏa.

    Với $i\notin I$, chọn
    
    $$
    \varepsilon = \min\left\{\dfrac{c_i - \sum_{j}a_{ji}y_j}{\sum_{j}a_{ji}\bar y_j}:i\notin I,~\textstyle\sum_{j}a_{ji}\bar y_j>0\right\}
    $$
    
    để vẫn khả thi và cải thiện tối đa. Nếu tập rỗng (tức $\varepsilon=+\infty$), thì $(D)$ vô hạn và $(P)$ không khả thi.

Quy trình này chỉ cần giải $(DRP)$, dạng đơn giản hơn $(D)$. Tính khả thi của $(DRP)$ do Farkas đảm bảo; ràng buộc $y\le 1$ chỉ để chuẩn hóa, đảm bảo hữu hạn.

Trong thi đấu, phương pháp nguyên‑đối ngẫu được dùng rộng rãi trong tối ưu tổ hợp: [Hungarian](../graph/graph-matching/bigraph-weight-match.md#hungarian-algorithmkuhnmunkres-algorithm), [hủy chu trình](../graph/flow/min-cost.md), [SSP (nguyên‑đối ngẫu)](../graph/flow/min-cost.md#ssp-算法)、[Dijkstra](../graph/shortest-path.md#dijkstra-算法)、[Ford–Fulkerson](../graph/flow/max-flow.md#fordfulkerson-增广) v.v.

## Quy hoạch nguyên

**Quy hoạch nguyên** (integer programming), thường chỉ **quy hoạch nguyên tuyến tính** (ILP). Dạng chuẩn:

$$
\begin{aligned}
\min_{x}\; & c^Tx \\
\text{subject to } & Ax = b \ge 0,\\
& x \ge 0,\\
& x \in \mathbf Z^n,
\end{aligned}
$$

với $A\in\mathbf R^{m\times n}$, $b\in\mathbf R^m$, $c\in\mathbf R^n$. Tức là thêm ràng buộc biến phải nguyên.

Ràng buộc nguyên làm bài toán khó hơn nhiều. Nhiều bài tối ưu tổ hợp (knapsack, feasibility, nhiều bài đồ thị) có thể mô hình bằng ILP; đa số là NP‑khó.

### Ma trận toàn unimodular

Vì vậy, đôi khi ta bỏ ràng buộc nguyên để giải LP lấy cận dưới (với bài tối thiểu). Nhưng nếu nghiệm LP tối ưu lại là nguyên, thì đó là nghiệm tối ưu của ILP. Khi nào đảm bảo nghiệm LP là nguyên? Khái niệm **ma trận toàn unimodular** trả lời.

???+ abstract "Ma trận toàn unimodular"
    Nếu mọi định thức con của $A\in\mathbf R^{m\times n}$ đều bằng $0$ hoặc $\pm 1$, thì $A$ là **ma trận toàn unimodular** (totally unimodular).

Đặc biệt, mọi phần tử là $0$ hoặc $\pm 1$. Có kết luận:

???+ note "Định lý"
    Với $A\in\mathbf Z^{m\times n}$ toàn unimodular, $b\in\mathbf Z^{m}$ và $c\in\mathbf Z^n$, các bài toán
    
    $$
    \min\{c^Tx : Ax=b,x\ge 0\} = \max\{b^Ty: A^Ty\le c\}
    $$
    
    đều có nghiệm tối ưu nguyên nếu hữu hạn.

??? note "Chứng minh"
    Nghiệm tối ưu nằm trên một mặt tối tiểu, là nghiệm của hệ ràng buộc chặt độc lập:
    
    $$
    \{x\in\mathbf R^n : a_j^Tx = b_j,~\forall j\in J\}.
    $$
    
    Gọi hệ là $A_Jx=b_J$, với $A_J=(A_1,A_2)$ và $A_1$ là ma trận vuông đầy hạng, định thức $\pm1$. Theo quy tắc Cramer:
    
    $$
    x = \begin{pmatrix}A_1^{-1}b_J \\ 0\end{pmatrix}
    $$
    
    là nghiệm nguyên.

Nhiều mô hình đồ thị (luồng, đường đi ngắn, ghép đôi hai phía…) có ma trận toàn unimodular, nên chỉ cần dữ liệu nguyên thì nghiệm LP có thể chọn nguyên; không phải lo “luồng phân số”. Vì vậy [max flow](../graph/flow/max-flow.md), [min cut](../graph/flow/min-cut.md), [min cost flow](../graph/flow/min-cost.md), [shortest path](../graph/shortest-path.md), [difference constraints](../graph/diff-constraints.md), [matching hai phía](../graph/graph-matching/bigraph-match.md#线性规划形式) đều có thể chuyển về LP; và các cặp tương ứng là bài toán đối ngẫu.

Ngoài ra, một số mô hình đồ thị có tập nghiệm là các đỉnh nguyên của một đa bào, nên có thể dùng ràng buộc thích hợp để biến bài toán tổ hợp thành LP. Ví dụ matching tổng quát, cây khung tối thiểu… nên [matching tổng quát](../graph/graph-matching/general-weight-match.md) và [MST](../graph/mst.md) cũng có thể chuyển về LP.

## Tài liệu tham khảo & chú thích

-   Schrijver, Alexander. Theory of linear and integer programming. John Wiley & Sons, 1998.
-   Papadimitriou, Christos H., and Kenneth Steiglitz. Combinatorial optimization: algorithms and complexity. Courier Corporation, 1998.
-   [Duality in linear programming. Part 1—definition and construction. by adamant - Codeforces blog](https://codeforces.com/blog/entry/105049)
-   [Duality in linear programming. Part 2—in competitive programming. by adamant - Codeforces blog](https://codeforces.com/blog/entry/105789)

[^poly-names]: Một số tài liệu định nghĩa khác nhau: có nơi gọi trường hợp hữu hạn là “đa diện”, vô hạn là “đa bào”; có nơi không giả định lồi; có nơi gọi đa bào 3D là “đa diện”. Ở đây dùng định nghĩa nhất quán với Schrijver (1998) và Boyd & Vandenberghe (2004).

[^reducible]: Nói chính xác là có thể quy giảm qua lại trong thời gian đa thức.

[^other-methods]: Các phương pháp khác giải hệ bất đẳng thức gồm Fourier–Motzkin và Agmon–Motzkin–Schoenberg, trực tiếp nhưng thường không hiệu quả.
