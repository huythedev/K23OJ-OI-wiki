Kiến thức trước: [Cơ sở quy hoạch tuyến tính](./linear-programming.md)

## Giới thiệu

Trong thi đấu thuật toán, phương pháp đơn hình (simplex) thường được dùng để giải bài toán quy hoạch tuyến tính. Tuy nhiên, do các bài toán quy hoạch tuyến tính trong thi đấu thường có cấu trúc đặc biệt và có thể chuyển về bài toán luồng mạng, nên phương pháp đơn hình không dùng nhiều và hiệu quả không bằng các thuật toán chuyên cho luồng mạng.

## Khái niệm cơ bản

Giả sử cần giải bài toán quy hoạch tuyến tính [dạng chuẩn](./linear-programming.md#标准形式) sau với $n$ biến quyết định và $m+n$ ràng buộc:

$$
\begin{aligned}
\min_{x}\; & z = c^Tx \\
\text{subject to }& Ax = b, \\
& x \ge 0.
\end{aligned}
$$

Giả sử hệ phương trình do $m$ ràng buộc đẳng thức xác định có nghiệm, và $A$ đầy hạng, thì $\operatorname{rank}A = m \le n$.

### Một ví dụ

Trước khi mô tả chặt chẽ các bước của phương pháp đơn hình, ta xét một ví dụ cụ thể để dễ hiểu.

???+ example "Ví dụ"
    Xét bài toán quy hoạch tuyến tính
    
    $$
    \begin{aligned}
    \max\; & 10 x_1 + 12 x_2 + 12 x_3 \\
    \text{subject to } & x_1 + 2 x_2 + 2x_3 \le 20, \\
    & 2x_1 + x_2 + 2x_3 \le 20, \\
    & 2x_1 + 2x_2 + x_3 \le 20,\\
    & x_1,x_2,x_3 \ge 0.
    \end{aligned}
    $$
    
    Thêm biến dư, ta được dạng chuẩn:
    
    $$
    \begin{aligned}
    \min\; & -10 x_1 - 12 x_2 - 12 x_3 \\
    \text{subject to } & x_1 + 2 x_2 + 2x_3 + x_4 = 20, \\
    & 2x_1 + x_2 + 2x_3 + x_5 = 20, \\
    & 2x_1 + 2x_2 + x_3 + x_6 = 20,\\
    & x_1,x_2,x_3,x_4,x_5,x_6 \ge 0.
    \end{aligned}
    $$
    
    Quan sát các ràng buộc đẳng thức, chúng tương đương với việc biểu diễn $x_4,x_5,x_6$ theo $x_1,x_2,x_3$. Viết lại:
    
    $$
    \begin{array}{rrrrrr}
    \min_{x_i\ge 0}  &  z  = &  0 &  -10x_1 &  -12x_2 &  -12x_3\;\\
    \text{subject to}& x_4 = & 20 &    -x_1 &   -2x_2 &   -2x_3, \\
                & x_5 = & 20 &   -2x_1 &    -x_2 &   -2x_3, \\
                & x_6 = & 20 &   -2x_1 &   -2x_2 &    -x_3. \\
    \end{array}
    $$
    
    Từ dạng này thấy rõ, nếu đặt $x_1=x_2=x_3=0$ thì có một nghiệm khả thi:
    
    $$
    x = (0,0,0,20,20,20)^T.
    $$
    
    Giá trị tương ứng $z=0$. Ta gọi các biến đặt bằng 0 là biến phi cơ sở (non-basic variable), ở đây là $x_1,x_2,x_3$; còn lại $x_4,x_5,x_6$ là biến cơ sở (basic variable).
    
    Đây không phải nghiệm tối ưu. Nếu tăng $x_1,x_2,x_3$ sao cho $x_4,x_5,x_6$ vẫn không âm thì nghiệm vẫn khả thi. Vì hệ số của $x_1,x_2,x_3$ trong hàm mục tiêu đều âm, tăng chúng sẽ làm giảm giá trị mục tiêu. Chẳng hạn tăng $x_1$. Để giảm mục tiêu nhiều nhất, tăng $x_1$ tối đa, nhưng phải đảm bảo $x_4,x_5,x_6\ge 0$. Do đó $x_1$ tối đa là
    
    $$
    \min\left\{\dfrac{20}{1},\dfrac{20}{2},\dfrac{20}{2}\right\} = 10.
    $$
    
    Khi đó, nghiệm trở thành
    
    $$
    x = (10,0,0,10,0,0)^T.
    $$
    
    Vì $x_1$ trở thành biến cơ sở, để quay lại dạng ban đầu (3 biến cơ sở biểu diễn theo 3 biến phi cơ sở), cần chọn biến phi cơ sở mới. Vì $x_5,x_6$ đều bằng 0, chọn một trong hai, chẳng hạn $x_5$. Khi đó
    
    $$
    x_1 = 10 - 0.5x_5 - 0.5x_2 - x_3
    $$
    
    thay vào, ta được
    
    $$
    \begin{array}{rrrrrr}
    \min_{x_i\ge 0}  &   z = &-100&   +5x_5 &   -7x_2 &   -2x_3\;\\
    \text{subject to}& x_4 = & 10 & +0.5x_5 & -1.5x_2 &    -x_3, \\
                & x_1 = & 10 & -0.5x_5 & -0.5x_2 &    -x_3, \\
                & x_6 = &  0 &    +x_5 &    -x_2 &    +x_3. \\
    \end{array}
    $$
    
    Ta lại có dạng ban đầu.
    
    Xét hàm mục tiêu: hệ số của $x_3$ vẫn âm, có thể tăng $x_3$. Để $x_4,x_1,x_6\ge 0$, $x_3$ tối đa
    
    $$
    \min\left\{\dfrac{10}{1},\dfrac{10}{1}\right\} = 10.
    $$
    
    Lưu ý vì trong biểu thức $x_6$, hệ số của $x_3$ dương, nên tăng $x_3$ không làm $x_6$ âm; do đó dấu ngoặc chỉ có hai mục. Khi $x_3=10$, $x_1,x_4$ đều bằng 0, có thể chọn một làm biến phi cơ sở, chọn $x_4$.
    
    Khi đó
    
    $$
    x_3 = 10 + 0.5x_5 - 1.5x_2 - x_4
    $$
    
    và bài toán thành
    
    $$
    \begin{array}{rrrrrr}
    \min_{x_i\ge 0}  &   z = &-120&   +4x_5 &   -4x_2 &   +2x_4\;\\
    \text{subject to}& x_3 = & 10 & +0.5x_5 & -1.5x_2 &    -x_4, \\
                & x_1 = &  0 &    -x_5 &    +x_2 &    +x_4, \\
                & x_6 = & 10 & +1.5x_5 & -2.5x_2 &    -x_4. \\
    \end{array}
    $$
    
    Đặt $x_5=x_2=x_4=0$ ta đọc được nghiệm khả thi
    
    $$
    x = (0,0,10,0,0,10)^T,
    $$
    
    giá trị $z=-120$.
    
    Tiếp tục. Vì hệ số $x_2$ âm, có thể tăng $x_2$; nhưng để $x_3,x_6$ không âm, $x_2$ tối đa
    
    $$
    \min\left\{\dfrac{10}{1.5},\dfrac{10}{2.5}\right\} = 4.
    $$
    
    Giá trị nhỏ nhất ở biểu thức $x_6$, nên khi $x_2=4$ thì $x_6=0$. Thay
    
    $$
    x_2 = 4 + 0.6x_5 - 0.4x_6 - 0.4x_4
    $$
    
    ta được
    
    $$
    \begin{array}{rrrrrr}
    \min_{x_i\ge 0}  &   z = &-136& +1.6x_5 & +1.6x_6 & +3.6x_4\;\\
    \text{subject to}& x_3 = &  4 & -0.4x_5 & +0.6x_6 & -0.4x_4, \\
                & x_1 = &  4 & -0.4x_5 & -0.4x_6 & +0.6x_4, \\
                & x_2 = &  4 & +1.5x_5 & -2.5x_6 &    -x_4. \\
    \end{array}
    $$
    
    Đặt $x_5=x_6=x_4=0$ được
    
    $$
    x = (4,4,4,0,0,0)^T,
    $$
    
    và $z=-136$.
    
    Vì hệ số của mọi biến phi cơ sở đều dương, không thể tiếp tục cải thiện, nên nghiệm hiện tại là tối ưu. Thuật toán dừng.

Trong ví dụ, thuật toán bắt đầu từ nghiệm khả thi và cải thiện mục tiêu đến khi không thể cải thiện nữa. Đó là ý tưởng cơ bản của đơn hình.

### Nghiệm khả thi cơ bản

Do $A$ đầy hạng, luôn chọn được tập con $B\subseteq\{1,2,\cdots,n\}$ cỡ $m$ sao cho $A_B$ khả nghịch. Khi đó biểu diễn $x_B$ theo $x_N$:

$$
x_B = A_B^{-1}b - A_B^{-1}A_Nx_N.
$$

Trong đó $N=\{1,2,\cdots,n\}\setminus B$, $A_B,A_N$ là các cột tương ứng, $x_B,x_N$ là các thành phần tương ứng. Nếu $i\in B$ thì $x_i$ là **biến cơ sở** (basic variable); nếu $i\in N$ là **biến phi cơ sở** (non-basic variable). Tập biến cơ sở gọi là một **cơ sở** (basis), ký hiệu $B$.

???+ tip "«Cơ sở»"
    Tên gọi «cơ sở» có thể hiểu theo đại số tuyến tính. Gọi không gian tuyến tính do các cột của $A$ sinh là $V$, thì các cột ứng với $B$ là một cơ sở của $V$.

Trong biểu thức $x_B$, đặt $x_N=0$ ta được một nghiệm của các ràng buộc đẳng thức[^notation]

$$
x = (x_B,x_N) = (A_B^{-1}b,0).
$$

Nghiệm này gọi là **nghiệm cơ bản** (basic solution). Nếu thêm điều kiện $x\ge 0$ thì là **nghiệm khả thi cơ bản** (basic feasible solution, BFS). Trong đơn hình, luôn duy trì nghiệm hiện tại là BFS.

### Pivot (chuyển trục)

Mỗi lần lặp của đơn hình gọi là một **pivot**. Mỗi pivot loại một biến cơ sở cũ, thêm một biến cơ sở mới, cải thiện giá trị mục tiêu.

???+ tip "«Pivot»"
    «Pivot» có thể hiểu theo đại số tuyến tính: cơ sở $B$ tương ứng với các trục tọa độ trong biểu diễn của không gian $V$, pivot là quay một trục sang vị trí mới.

Để chọn biến vào cơ sở, biểu diễn mục tiêu theo biến phi cơ sở:

$$
\begin{aligned}
c^Tx &= c^T_Bx_B + c^T_Nx_N \\
&= c_B^TA_B^{-1}b + (c_N^T - c_B^TA_B^{-1}A_N)x_N.
\end{aligned}
$$

Đặt $x_N=0$ thì giá trị tại BFS là $z=c_B^TA_B^{-1}b$. Hệ số của hạng hai là

$$
\tilde c_N = \dfrac{\partial z}{\partial x_N} = c_N - A_N^T(A_B^{-1})^Tc_B.
$$

Vì $c_B - A_B^T(A_B^{-1})^Tc_B = 0$, đặt

$$
\tilde c = (\tilde c_B^T,\tilde c_N^T)^T = c - A^T(A_B^{-1})^Tc_B
$$

là **chi phí giảm** (reduced cost) tại nghiệm khả thi. Thành phần $\tilde c_i<0$ nghĩa là tăng $x_i$ sẽ cải thiện mục tiêu. Biến như vậy (chỉ có thể là biến phi cơ sở) gọi là **biến vào cơ sở** (entering variable).

Chọn xong biến vào, cần chọn biến ra: biến cơ sở nào về 0 trước. Thay $x_N=(x_i,0)$ vào $x_B$:

$$
x_B = A_B^{-1}b - A_B^{-1}A_ix_i.
$$

Do đó mức tăng tối đa:

$$
\theta = \min\left\{\dfrac{(A_B^{-1}b)_j}{(A_B^{-1}A_i)_j}:(A_B^{-1}A_i)_j>0\right\}.
$$

Chỉ số $j$ đạt min là biến cơ sở ra (leaving variable). Cách chọn gọi là **kiểm tra tỉ lệ tối thiểu** (minimum ratio test).

Gọi biến vào là $x_i$, biến ra là $x_{i'}$. Sau pivot, biến cơ sở là $x_{B\setminus\{i\}\cup\{i'\}}$, biến phi cơ sở là $x_{N\setminus\{i'\}\cup\{i\}}$.

### Điều kiện dừng

Đơn hình là quá trình pivot liên tục. Các tình huống đặc biệt:

-   Không có biến vào, tức $\tilde c\ge 0$. Khi đó không thể cải thiện, nghiệm hiện tại là tối ưu. Chứng minh cần [điều kiện bổ sung bù](./linear-programming.md#互补松弛条件). Đặt $y=(A_B^{-1})^Tc_B$. Vì luôn duy trì $x$ khả thi và thỏa bổ sung bù:

$$
x^T(c-A^Ty) = \tilde c^Tx = \tilde c_B^Tx_B + \tilde c_N^Tx_N = 0.
$$

Nếu $y$ là nghiệm khả thi của đối ngẫu ($A^Ty\le c$), thì $x$ và $y$ là tối ưu. Điều kiện này chính là $\tilde c\ge 0$.

???+ tip "«Giá bóng»"
    $y=(A_B^{-1})^Tc_B$ gọi là **vector đối ngẫu**. Khi BFS là tối ưu của bài toán gốc, $y$ là nghiệm tối ưu của bài toán đối ngẫu. Vì $y$ là đạo hàm giá trị theo hằng số ràng buộc:
    
    $$
    \dfrac{\partial(c^Tx)}{\partial b} = (A_B^{-1})^Tc_B = y,
    $$
    
    nên còn gọi là **giá bóng** (shadow price).

-   Không có biến ra, tức $A_B^{-1}A_i\le 0$. Khi đó không có “nút cổ chai”, có thể tăng $x_i$ vô hạn làm mục tiêu giảm đến $-\infty$. Bài toán vô hạn, thuật toán dừng.

-   Biến vào/ra có thể không duy nhất. Cách chọn không phù hợp có thể làm thuật toán pivot quá nhiều hoặc lặp vô hạn. Cần [quy tắc pivot](#转轴规则).

### Bảng đơn hình

Khi hiện thực pivot, chỉ cần duy trì ma trận hệ số sau mỗi pivot:

$$
\tilde T_B = 
\begin{pmatrix}
-z_B & \tilde c^T_N \\
x & A_B^{-1}A_N
\end{pmatrix}
=
\begin{pmatrix}
-c_B^TA_B^{-1}b & c^T - c_B^TA_B^{-1}A_N \\
A_B^{-1}b & A_B^{-1}A_N 
\end{pmatrix}.
$$

Tương ứng bài toán:

$$
\begin{array}{rrrr}
\min_{x\ge 0}    &       & c_B^TA_B^{-1}b & + \tilde c_N^Tx_N\; \\
\text{subject to}& x_B = & A_B^{-1}b      & - A_B^{-1}A_Nx_N.
\end{array}
$$

Ma trận $\tilde T_B$ gọi là **bảng đơn hình rút gọn** (condensed simplex tableau). Ô trái trên là giá trị hiện tại (âm), hàng 0 cột $i$ là chi phí giảm của biến phi cơ sở $x_{N_i}$, hàng $j$ cột 0 là giá trị biến cơ sở $x_{B_j}$, còn $A_B^{-1}A_N$ là hệ số trong biểu diễn.

Mọi thông tin pivot đều có trong bảng. Một pivot gồm:

1.  Chọn cột $i$ sao cho $(\tilde T_B)_{0i}<0$. Nếu không có, nghiệm hiện tại tối ưu, giá trị là $-(\tilde T_B)_{00}$.
2.  Chọn hàng $j$ sao cho $(\tilde T_B)_{ji}>0$ và $(\tilde T_B)_{j0}/(\tilde T_B)_{ji}$ nhỏ nhất. Nếu không có, bài toán vô hạn.
3.  Cho $x_{N_i}$ vào cơ sở, $x_{B_j}$ ra cơ sở, cập nhật bảng.

Cập nhật bảng: trước cập nhật, hàng $j$ biểu diễn

$$
x_{B_j} = (\tilde T_B)_{j0} - \sum_{i=1}^{n-m}(\tilde T_B)_{ji}x_{N_i}.
$$

Cần biểu diễn $x_{N_i}$ theo $x_{N\setminus\{N_i\}\cup\{B_j\}}$:

$$
x_{N_i} = \dfrac{(\tilde T_B)_{j0}}{(\tilde T_B)_{ji}} - \dfrac{1}{(\tilde T_B)_{ji}}x_{B_j} - \sum_{i'\neq i}\dfrac{(\tilde T_B)_{ji'}}{(\tilde T_B)_{ji}}x_{N_{i'}}.
$$

Thay vào các dòng khác, được

$$
x_{B_{j'}} = \left((\tilde T_B)_{j'0} - (\tilde T_B)_{j'i}\dfrac{(\tilde T_B)_{j0}}{(\tilde T_B)_{ji}}\right) + \dfrac{(\tilde T_B)_{j'i}}{(\tilde T_B)_{ji}}x_{B_j} - \sum_{i'\neq i}\left((\tilde T_B)_{j'i'}-(\tilde T_B)_{j'i}\dfrac{(\tilde T_B)_{ji'}}{(\tilde T_B)_{ji}}\right)x_{N_i}.
$$

Dù phức tạp, khi cài đặt chỉ cần:

1.  Cập nhật hàng $j$: đặt $\alpha=(\tilde T_B)_{ji}$, đưa phần tử cột $i$ thành 1, chia cả hàng cho $\alpha$;
2.  Với $j'\neq j$: đặt $\beta=(\tilde T_B)_{j'i}$, đưa cột $i$ về 0 bằng cách trừ $\beta$ lần hàng $j$.

???+ tip "«Bảng đơn hình»"
    **Bảng đơn hình** (simplex tableau) là
    
    $$
    T_B = 
    \begin{pmatrix}
    -z & \tilde c^T \\
    x & A_B^{-1}A 
    \end{pmatrix}
    =
    \begin{pmatrix}
    -c_B^TA_B^{-1}b & c^T - c_B^TA_B^{-1}A \\
    A_B^{-1}b & A_B^{-1}A 
    \end{pmatrix}.
    $$
    
    So với bảng rút gọn, nó thêm $m$ cột ứng với $m$ biến cơ sở; các cột này là $e_j$ (chỉ hàng $j$ là 1, còn lại 0). Vì không có thêm thông tin nên thường bỏ, tạo bảng rút gọn.
    
    Nhìn từ bảng đầy đủ, mọi bảng $T_B$ có thể được từ một bảng $T_0$ bằng nhân trái với ma trận khả nghịch $L_B$:
    
    $$
    T_B=
    \begin{pmatrix}
    -c_B^TA_B^{-1}b & c^T - c_B^TA_B^{-1}A \\
    A_B^{-1}b & A_B^{-1}A 
    \end{pmatrix}
    =
    \begin{pmatrix}
    1 & -c_B^TA_B^{-1} \\
    O & A_B^{-1}
    \end{pmatrix}
    \begin{pmatrix}
    0 & c^T \\
    b & A 
    \end{pmatrix}=L_BT_0,
    $$
    
    nên có thể chuyển đổi bằng [phép biến đổi hàng sơ cấp](./linear-algebra/elementary-operations.md). Vì vậy khi cập nhật, chỉ cần biến đổi để cột biến vào thành $e_j$. Chiếu xuống bảng rút gọn chính là quy trình trên.

Cài đặt tham khảo cập nhật bảng:

???+ example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/math/code/simplex/simplex_0.cpp:pivot"
    ```

Từ đó thấy mỗi lần cập nhật bảng mất $O(mn)$. Chọn biến vào/ra cũng không vượt $O(mn)$, nên một pivot là $O(mn)$.

Để dễ hiểu, liệt kê các bước cho ví dụ trên dùng bảng rút gọn:

???+ example "Ví dụ (tiếp)"
    Ban đầu, bảng rút gọn:
    
    $$
    \begin{array}{|l|c|ccc|}
    \hline
        &    & x_1 & x_2 & x_3 \\
    \hline
        & 0  & -10 & -12 & -12 \\
    \hline
    x_4= & 20 &   1 & 2   &   2 \\
    x_5= & 20 &   2 & 1   &   2 \\
    x_6= & 20 &   2 & 2   &   1 \\
    \hline
    \end{array}
    $$
    
    Theo hàng 0, có thể chọn $x_1,x_2,x_3$ vào. Chọn $x_1$. Theo kiểm tra tỉ lệ, có thể chọn $x_5,x_6$ ra. Chọn $x_5$. Bảng cập nhật:
    
    $$
    \begin{array}{|l|c|ccc|}
    \hline
        &    & x_5 & x_2 & x_3 \\
    \hline
        &100 & 5   & -7  & -2  \\
    \hline
    x_4= & 10 &-0.5 & 1.5 &   1 \\
    x_1= & 10 & 0.5 & 0.5 &   1 \\
    x_6= &  0 & -1  & 1   &  -1 \\
    \hline
    \end{array}
    $$
    
    Theo hàng 0, chọn $x_2,x_3$ vào. Chọn $x_3$. Theo kiểm tra tỉ lệ, có thể chọn $x_4,x_1$ ra. Chọn $x_4$. Bảng:
    
    $$
    \begin{array}{|l|c|ccc|}
    \hline
        &    & x_5 & x_2 & x_4 \\
    \hline
        &120 & 4   & -4  & 2   \\
    \hline
    x_3= & 10 &-0.5 & 1.5 &  1  \\
    x_1= &  0 &  1  & -1  & -1  \\
    x_6= & 10 & -1.5& 2.5 &  1  \\
    \hline
    \end{array}
    $$
    
    Theo hàng 0, chỉ có thể chọn $x_2$ vào. Theo kiểm tra tỉ lệ, chỉ có thể chọn $x_6$ ra. Cập nhật:
    
    $$
    \begin{array}{|l|c|ccc|}
    \hline
        &    & x_5 & x_4  & x_6 \\
    \hline
        &136 & 1.6 & 3.6 & 1.6 \\
    \hline
    x_3= & 4  & 0.4 & 0.4 &-0.6 \\
    x_1= &  4 & 0.4 & -0.6& 0.4 \\
    x_2= &  4 & -0.6& 0.4 & 0.4 \\
    \hline
    \end{array}
    $$
    
    Theo hàng 0, không có biến vào. Vậy nghiệm hiện tại
    
    $$
    x=(4,4,4,0,0,0)^T
    $$
    
    là tối ưu, giá trị tối ưu (bài toán tối thiểu) là $-136$.

Ngoài bảng rút gọn, còn có phương pháp đơn hình sửa đổi (revised simplex method), giảm độ phức tạp mỗi cập nhật xuống $O(m^2)$, đặc biệt hiệu quả khi $m\ll n$ hoặc $A$ thưa.

## Nền tảng hình học

Phần này nói về nền tảng hình học của đơn hình.

Với miền khả thi

$$
\mathcal D = \{x\in\mathbf R^n : Ax = b,~ x\ge 0\}
$$

theo [phân tích](./linear-programming.md#可行域与问题的解):

-   Nếu nghiệm tối ưu tồn tại, có thể chọn là một đỉnh của $\mathcal D$. Giải bài toán là tìm đỉnh tối ưu.
-   Mỗi đỉnh là nghiệm của hệ $n$ ràng buộc chặt. Với dạng chuẩn, $m$ đẳng thức luôn chặt, còn $n-m$ ràng buộc chặt chọn từ điều kiện không âm, tương đương đặt $x_N=0$. Khi đó $Ax=b$ trở thành $A_Bx_B=b$, nếu $A_B$ khả nghịch thì $x_B=A_B^{-1}b$. Nếu $x_B\ge 0$ thì đó là đỉnh.

Thấy rằng khái niệm đỉnh trùng với BFS. Do đó chỉ cần tìm BFS tối ưu. Tuy nhiên số đỉnh là mũ, không thể duyệt hết.

Giải pháp là di chuyển dọc theo [cạnh](./linear-programming.md#可行域与问题的解) của miền khả thi, từ đỉnh này sang đỉnh kề. Hai đỉnh kề nằm trên cùng một cạnh nên chia sẻ ít nhất $n-1$ ràng buộc chặt, khác nhau đúng 1 ràng buộc. Do đó, từ BFS $x$ chỉ cần đổi một biến cơ sở thành biến phi cơ sở để được BFS kề $x'$. Đó chính là pivot.

Vì vậy đơn hình là quá trình đi trên miền khả thi từ đỉnh này sang đỉnh kề, cải thiện mục tiêu.

???+ example "Ví dụ (tiếp)"
    Trong ví dụ, miền khả thi là đa diện 3D có 5 đỉnh, như hình:
    
    ![](./images/simplex-geo.svg)
    
    Quá trình giải tương ứng đường đi:
    
    $$
    (0,0,0) \rightarrow (0,0,10) \rightarrow (10,0,0) \rightarrow (4,4,4).
    $$

## Chi tiết hiện thực

Bảng đơn hình giải được nhiều bài toán, nhưng với trường hợp tổng quát vẫn có nhiều chi tiết cần bàn.

### Dạng slack

Cách chuyển về dạng chuẩn đã nói [ở đây](./linear-programming.md#标准形式). Nhưng để dễ dùng đơn hình, còn cần $A$ đầy hạng. Một cách thường dùng:

1.  Chuyển về **dạng bất đẳng thức**: $\min\{c^Tx : Ax \le b,~ x \ge 0\}$;
2.  Thêm biến dư $s$: $\min\{c^Tx : Ax + s = b,~ x\ge 0,~ s \ge 0\}$.

Khi đó ma trận $(A,I)$ luôn đầy hạng, và luôn có nghiệm cơ bản (không nhất thiết khả thi) $(x,s)=(0,b)$. Dạng này gọi là **dạng slack** (slack form).

### BFS ban đầu

Đơn hình giả định có BFS ban đầu. Đôi khi dễ tìm: nếu $b\ge 0$ trong slack form thì $(x,s)=(0,b)$ là BFS, như ví dụ.

Trường hợp tổng quát dùng **hai giai đoạn** (two-phase). Giai đoạn 1 giải một bài toán khả thi để tìm BFS cho bài gốc; giai đoạn 2 từ BFS đó giải bài gốc.

Giả sử bài toán dạng chuẩn $\min\{c^Tx : Ax = b \ge 0,~ x\ge 0\}$. Giai đoạn 1 giải:

$$
\min\{1^Tx_a : Ax + x_a = b,~ x\ge 0,~ s\ge 0\}.
$$

Đây là bài toán khả thi, thêm biến **nhân tạo** (artificial variable) $x_a$. Nó có BFS $(x,x_a)=(0,b)$, dùng đơn hình giải. Nếu giá trị tối ưu >0 thì bài gốc vô nghiệm. Nếu =0 thì biến nhân tạo ở nghiệm tối ưu bằng 0. Nếu còn biến nhân tạo là biến cơ sở, pivot để loại. Khi tất cả biến nhân tạo là phi cơ sở, BFS thu được dùng cho giai đoạn 2.

???+ note "Cài đặt giai đoạn 1 không cần đưa biến nhân tạo tường minh"
    Khi khởi tạo một cơ sở $B$ bất kỳ, có
    
    $$
    x_B + A_B^{-1}A_Nx_N = A_B^{-1}b.
    $$
    
    Nếu $(A_B^{-1}b)_j\ge 0$ thì không cần biến nhân tạo; nếu <0 thì thêm $x^-_{B_j}$:
    
    $$
    x_{B_j} - x^-_{B_j} + (A_B^{-1}A_N)_{(j)}x_N = (A_B^{-1}b)_j.
    $$
    
    Gọi $L:=\{j:(A_B^{-1}b)_j<0\}$, bảng giai đoạn 1 là:
    
    $$
    \begin{array}{|r|c|cccc|}
    \hline
                    &                       & x_N                      & x_{B_{\sim L}} & x_{B_L}  &x_{B_{L}}^- \\  
    \hline
                    & 0                     & 0^T                      & 0^T            & 0^T      & 1^T        \\
    \hline
    x_{B_{\sim L}}=  & (A_B^{-1}b)_{\sim L}  & (A_B^{-1}A_N)_{(\sim L)} & I              &  O       &  O         \\
    x_{B_{L}}^-=     & (A_B^{-1}b)_L         & (A_B^{-1}A_N)_{(L)}      & O              &  I       & -I         \\
    \hline
    \end{array}
    $$
    
    Rút gọn như bảng rút gọn (lấy hàng $x_{B_{L}}^-$ nhân $1^T$ cộng vào hàng 0, rồi bỏ 3 cột sau):
    
    $$
    \begin{array}{|r|c|cccc|}
    \hline
                    &                      & x_N                      \\  
    \hline
                    & 1^Tb_L               & 1^T(A_B^{-1}A_N)_L       \\
    \hline
    x_{B_{\sim L}}= & (A_B^{-1}b)_{\sim L} & (A_B^{-1}A_N)_{(\sim L)} \\
    -x_{B_{L}}=     & (A_B^{-1}b)_L        & (A_B^{-1}A_N)_L          \\
    \hline
    \end{array}
    $$
    
    Bảng này giống bảng rút gọn, chỉ khác hàng cuối có dấu âm, biểu thị vẫn có biến nhân tạo (biến slack ban đầu chưa khả thi). Pivot theo:
    
    1.  Nếu $L=\varnothing$ thì dừng.
    2.  Chọn biến vào $x_{N_i}$ theo chi phí giảm âm ở hàng 0; nếu không có thì bài gốc vô nghiệm.
    3.  Chọn biến ra theo kiểm tra tỉ lệ tối thiểu, đồng thời đảm bảo biến khả thi vẫn khả thi và biến không khả thi vẫn không khả thi, tức chọn
    
        $$
        \arg\min_{j}\left\{\dfrac{(\tilde T_B)_{j0}}{(\tilde T_B)_{ji}}:(j\notin L\land(\tilde T_B)_{ji}>0)\lor(j\in L\land(\tilde T_B)_{ji}<0)\right\}
        $$
    
        nếu có nhiều, ưu tiên biến không khả thi.
    4.  Cho $x_{N_i}$ vào, $x_{B_j}$ ra, cập nhật bảng.
    5.  Nếu $j\in L$, loại $j$ khỏi $L$ (bỏ dấu âm) và cộng $(\tilde T_B)_{0i}$ vào 1.
    
    Bỏ cột biến nhân tạo vì nếu chúng còn là biến cơ sở thì cột là $e_j$; nếu không còn cơ sở thì không bao giờ vào lại. Khi biến nhân tạo ra khỏi cơ sở, thay bằng biến không nhân tạo, chính là mục đích bước cuối.
    
    Cài đặt tham khảo:
    
    ??? example "Cài đặt tham khảo"
        ```cpp
        --8<-- "docs/math/code/simplex/simplex_0.cpp:initialize"
        ```
    
    Trước khi pha 1 bắt đầu, thêm một hàng cho hàm mục tiêu pha 1. Pivot trên toàn bảng (bao gồm mục tiêu pha 2). Khi pha 1 kết thúc, mục tiêu pha 2 đã được cập nhật và có thể bắt đầu pha 2.

Hai giai đoạn cũng có thể thay bằng một lần đơn hình bằng cách dùng số lớn $M$:

$$
\min\{c^Tx + M1^Tx_a : Ax + x_a = b,~ x\ge 0,~ s\ge 0\}.
$$

Không gán giá trị cụ thể cho $M$, coi như một hằng dương đủ lớn. Cách này gọi là **phương pháp $M$ lớn** (big $M$ method).

???+ warning "Thuật toán ngây thơ có độ phức tạp thực tế là mũ"
    Vì slack form luôn có nghiệm cơ bản ban đầu (không nhất thiết khả thi), một ý tưởng đơn giản là từ nghiệm cơ bản không khả thi, liên tục pivot để đưa biến cơ sở âm ra ngoài và chọn biến phi cơ sở có hệ số âm vào, đến khi mọi biến cơ sở không âm. Cài đặt:
    
    ??? example "Cài đặt tham khảo"
        ```cpp
        --8<-- "docs/math/code/simplex/simplex_2.cpp:initialize"
        ```
    
    Cách này đơn giản nhưng so với hai giai đoạn thì không có hàm mục tiêu đo mức “không khả thi”, nên thiếu hướng cải thiện rõ ràng. Thực nghiệm cho thấy số pivot thường $O(2^m)$ (trong khi hai giai đoạn thường $O(m)$), và dễ lặp khi $n,m$ lớn. Chỉ phù hợp khi $n,m<50$.

### Quy tắc pivot

Khi có nhiều lựa chọn biến vào/ra, cần **quy tắc pivot**. Dùng bảng đơn hình, các quy tắc đều tìm được biến vào/ra trong $O(mn)$, nên mỗi pivot vẫn $O(mn)$.

Chọn biến vào quyết định số pivot. Quy tắc thường gặp:

-   Chọn biến vào đầu tiên gặp;
-   Chọn biến vào có chỉ số nhỏ nhất (một phần của quy tắc Bland);
-   Chọn biến vào có chi phí giảm tuyệt đối lớn nhất ($|c_i|$) (quy tắc Dantzig);
-   Chọn biến vào làm cải thiện mục tiêu mỗi pivot lớn nhất ($|c_i|\theta_i$);
-   Chọn biến vào trên cạnh dốc nhất (cải thiện mục tiêu trên đơn vị độ dài cạnh), tức $|c_i|/\|A_B^{-1}A_i\|$ lớn nhất;
-   Chọn ngẫu nhiên biến vào.

Trong thực tế, quy tắc cạnh dốc nhất thường hiệu quả nhất[^steepest-edge]. Thường tin rằng quy tắc phù hợp cho phép giải đa số bài toán trong khoảng $2m$ pivot. Tuy nhiên với mọi quy tắc hiện có đều có ví dụ cấu tạo[^klee-minty] đẩy số pivot lên mũ, nên đơn hình có hiệu năng thực tế tốt nhưng độ phức tạp xấu nhất là mũ.

Chọn biến ra ảnh hưởng việc lặp. Nếu có nhiều BFS tối ưu, thuật toán có thể lặp giữa chúng. Điều này ít gặp, nên nhiều cài đặt không quy định rõ. Các quy tắc tránh lặp:

-   Bland: luôn chọn biến vào/ra có chỉ số nhỏ nhất.
-   Từ điển (lexicographic): chọn hàng $j$ có $(A_B^{-1}A_i)_j>0$ và
    
    $$
    \left(\dfrac{(A_B^{-1}b)_j}{(A_B^{-1}A_i)_j},\dfrac{(A_B^{-1})_{j1}}{(A_B^{-1}A_i)_j},\cdots,\dfrac{(A_B^{-1})_{jm}}{(A_B^{-1}A_i)_j}\right)
    $$
    
    là nhỏ nhất theo thứ tự từ điển. Cách chọn biến vào không quan trọng.
    
    Lưu ý: nếu bài toán là slack form thì các lượng này lấy trực tiếp từ bảng $T_B$; nếu không thì cần một cơ sở ban đầu (không nhất thiết khả thi), dùng các cột của cơ sở (giữ thứ tự) làm $A_B^{-1}$.

Bland hiệu quả kém vì dễ làm cùng biến vào ra lặp lại. Quy tắc từ điển thực tế hơn, tương đương việc nhiễu nhỏ tham số[^lexico] để tránh các BFS có giá trị tối ưu bằng nhau.

## Cài đặt tham khảo

Cung cấp cài đặt tham khảo đơn hình hai giai đoạn dựa trên bảng rút gọn.

??? example "[Luogu P13337【模板】线性规划](https://www.luogu.com.cn/problem/P13337)"
    ```cpp
    --8<-- "docs/math/code/simplex/simplex_0.cpp:full-text"
    ```

## Bài toán mẫu

???+ example "[「NOI2008」志愿者招募](https://www.luogu.com.cn/problem/P3980)"
    Có $n$ ngày cần tuyển tình nguyện viên, ngày $i$ cần ít nhất $b_i$ người. Có $m$ loại tình nguyện viên, loại $j$ phục vụ trong đoạn liên tiếp $[l_j,r_j]$, chi phí mỗi người là $c_i$. Tìm phương án tuyển tối ưu để chi phí nhỏ nhất.

??? note "Lời giải"
    Gọi loại $j$ tuyển $x_j$ người. Khi đó bài toán:
    
    $$
    \begin{align*}
    \max_{x}\; & \sum_{j=1}^mc_jx_j \\
    \text{subject to }& \sum_{i=1}^n a_{ij}x_j \ge b_i,~i=1,\cdots,n,\\
    & x_j\ge 0,~j=1,\cdots,m.
    \end{align*}
    $$
    
    với
    
    $$
    a_{ij} = 
    \begin{cases}
    1,& l_j\le i\le r_j,\\
    0,& \text{otherwise.}
    \end{cases}
    $$
    
    Bài gốc không có nghiệm khả thi hiển nhiên, nên xét [đối ngẫu](./linear-programming.md#对偶问题):
    
    $$
    \begin{align*}
    \min_{y}\; & \sum_{i=1}^n b_iy_i \\
    \text{subject to } & \sum_{j=1}^na_{ij}y_i \le c_j,~j=1,\cdots,m,\\
    & y_i\ge 0,~i=1,\cdots,n.
    \end{align*}
    $$
    
    Thêm biến dư ta có nghiệm khả thi ban đầu, có thể bỏ qua pha 1 và dùng đơn hình trực tiếp. Theo đối ngẫu, nghiệm thu được là nghiệm bài gốc.
    
    ```cpp
    --8<-- "docs/math/code/simplex/simplex_1.cpp"
    ```

## Bài tập

-   [Luogu P13337【模板】线性规划](https://www.luogu.com.cn/problem/P13337)
-   [UOJ#179. 线性规划](https://uoj.ac/problem/179)
-   [Luogu P4232 无意识之外的捉迷藏](https://www.luogu.com.cn/problem/P4232)
-   [Codeforces 1430 G. Yet Another DAG Problem](https://codeforces.com/problemset/problem/1430/G)
-   [AtCoder Beginner Contest 231 H - Minimum Coloring](https://atcoder.jp/contests/abc231/tasks/abc231_h)

## Tài liệu tham khảo

-   [线性规划之单纯形法【超详解 + 图解】](https://www.cnblogs.com/ECJTUACM-873284962/p/7097864.html)
-   [2016 国家集训队论文](https://github.com/OI-wiki/libs/blob/master/%E9%9B%86%E8%AE%AD%E9%98%9F%E5%8E%86%E5%B9%B4%E8%AE%BA%E6%96%87/%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F2016%E8%AE%BA%E6%96%87%E9%9B%86.pdf)
-   算法导论
-   Matoušek, Jiří, and Bernd Gärtner. Understanding and using linear programming. Vol. 1. Berlin: Springer, 2007.
-   Inayatullah, Syed, Nasir Touheed, and Muhammad Imtiaz. "A streamlined artificial variable free version of simplex method." PloS one 10, no. 3 (2015): e0116156.
-   Floudas, Christodoulos A., and Panos M. Pardalos, eds. Encyclopedia of optimization. Springer Science & Business Media, 2008.

[^notation]: Về nguyên tắc, vì mọi vector đều mặc định là cột, $(x_B,x_N)$ nên viết $(x_B^T,x_N^T)^T$. Nhưng để đơn giản, trong bài này đều viết $(x_B,x_N)$ và bỏ ký hiệu chuyển vị.

[^steepest-edge]: Kết quả thử nghiệm xem Forrest, John J., and Donald Goldfarb. "Steepest-edge simplex algorithms for linear programming." Mathematical programming 57, no. 1 (1992): 341-374.

[^klee-minty]: Một phản ví dụ kinh điển xem Klee, Victor, and George J. Minty. "How good is the simplex algorithm." Inequalities 3, no. 3 (1972): 159-175.

[^lexico]: Giải thích chi tiết xem [bài giảng này](https://facultyweb.kennesaw.edu/mlavrov/courses/lp/lecture8.pdf).
````
