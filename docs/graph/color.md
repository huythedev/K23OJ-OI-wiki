## Tô màu đỉnh

(Chỉ xét đồ thị vô hướng không có khuyên)

Tô màu các đỉnh của đồ thị vô hướng sao cho hai đỉnh kề nhau không cùng màu. Nếu $G$ là $k$-tô màu được, nhưng không $(k-1)$-tô màu được, thì $k$ gọi là số sắc của $G$, ký hiệu $\chi(G)$.

Với mọi đồ thị $G$, ta có $\chi(G) \leq \Delta(G) + 1$, trong đó $\Delta(G)$ là bậc lớn nhất.

### Định lý Brooks

Giả sử đồ thị liên thông không phải là đồ thị đầy đủ cũng không phải chu trình lẻ, khi đó $\chi(G) \leq \Delta(G)$.

#### Chứng minh

???+ note "Chứng minh"
    Gọi $|V(G)|=n$, xét quy nạp theo $n$.
    
    Với $n\leq 3$, mệnh đề hiển nhiên đúng.
    
    Giả sử mệnh đề đúng với $n-1$, ta sẽ tăng cường mệnh đề.
    
    Chỉ cần xét $\Delta(G)$-chính quy, vì với đồ thị không chính quy, có thể coi là xóa cạnh từ đồ thị chính quy, điều này không ảnh hưởng kết luận.
    
    Với đồ thị chính quy không phải đầy đủ cũng không phải chu trình lẻ, chọn một đỉnh $v$, xét đồ thị con $H:=G-v$. Theo giả thiết quy nạp, $\chi(H)\leq\Delta(H)=\Delta(G)$. Ta chỉ cần chứng minh thêm $v$ vào $H$ không làm tăng số sắc vượt quá $\Delta(G)$.
    
    Gọi $\Delta:=\Delta(G)$, giả sử $H$ đã được tô $\Delta$ màu $c_1,c_2,\dots,c_{\Delta}$, các đỉnh kề $v$ là $v_1,v_2,\dots,v_{\Delta}$. Giả sử các đỉnh này có màu khác nhau, nếu không thì đã xong.
    
    Xét các đồ thị con $H_{i,j}$ gồm các đỉnh tô màu $c_i$ hoặc $c_j$ và các cạnh giữa chúng. Giả sử với mọi cặp $v_i,v_j$, chúng nằm trong cùng một thành phần liên thông của $H_{i,j}$, nếu không, có thể đổi màu một thành phần để $v_i,v_j$ cùng màu.
    
    > Đổi màu ở đây nghĩa là nếu chỉ có hai màu $a,b$, thì đổi tất cả đỉnh màu $a$ thành $b$ và ngược lại.
    
    Gọi thành phần liên thông đó là $C_{i,j}$, $C_{i,j}$ chỉ có thể là đường đi từ $v_i$ đến $v_j$. Vì $v_i$ trong $H$ có bậc $\Delta-1$, các đỉnh kề $v_i$ trong $H$ có màu khác nhau, nên trong $C_{i,j}$, $v_i$ chỉ kề một đỉnh, $v_j$ tương tự. Lấy đường đi $P$ từ $v_i$ đến $v_j$ trong $C_{i,j}$, nếu $C_{i,j}\ne P$, trên $P$ gặp đỉnh $u$ bậc $>2$, $u$ kề tối đa $\Delta-2$ màu, nên có thể đổi màu $u$ để tách $v_i,v_j$.
    
    Với mọi ba đỉnh $v_i,v_j,v_k$, ta có $V(C_{i,j})\cap V(C_{j,k})=\{v_j\}$.
    
    Đến đây đã tăng cường xong.
    
    Nếu các đỉnh kề $v$ đều kề nhau, mệnh đề đúng. Giả sử $v_1,v_2$ không kề, lấy $w$ là đỉnh kề $v_1$ trên $C_{1,2}$, đổi màu $C_{1,3}$, khi đó $w\in V(C_{1,2})\cap V(C_{2,3})$, mâu thuẫn.
    
    Vậy mệnh đề được chứng minh.

### Thuật toán Welsh–Powell

Thuật toán Welsh–Powell là một thuật toán tham lam tìm phương án tô màu **không giới hạn số màu tối đa**.

Với đồ thị vô hướng không khuyên $G$, giả sử $V(G):=\{v_1,v_2,\dots,v_n\}$ thỏa mãn

$\deg(v_i)\geq\deg(v_{i+1}),~\forall 1\leq i\leq n-1$

Sau khi tô màu theo Welsh–Powell, số màu tối đa là $\max_{i=1}^n\min\{\deg(v_i)+1,i\}$, độ phức tạp $O\left(n\max_{i=1}^n\min\{\deg(v_i)+1,i\}\right)=O(n^2)$.

#### Quy trình

1.  Sắp xếp các đỉnh chưa tô màu theo thứ tự giảm dần bậc.
2.  Tô màu đỉnh đầu tiên bằng màu chưa dùng.
3.  Duyệt các đỉnh tiếp theo, nếu đỉnh hiện tại không kề với bất kỳ đỉnh nào đã tô cùng màu, thì tô cùng màu với đỉnh đầu.
4.  Nếu còn đỉnh chưa tô, lặp lại bước 1, ngược lại kết thúc.

Ví dụ:

![Orignal](images/color1.png)

(Được tạo bởi [Graph Editor](https://csacademy.com/app/graph_editor/))

Sắp xếp các đỉnh theo bậc giảm dần:

| Thứ tự                    | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9  | 10 | 11 | 12 | 13 |
| ----------------------- | - | - | - | - | - | - | - | - | -- | -- | -- | -- | -- |
| Số hiệu đỉnh                | 4 | 5 | 0 | 2 | 9 | 1 | 3 | 6 | 10 | 12 | 7  | 8  | 11 |
| Bậc                        | 5 | 5 | 4 | 4 | 4 | 3 | 3 | 3 | 3  | 3  | 2  | 2  | 1  |
| $\min\{\deg(v_i)+1,i\}$ | 1 | 2 | 3 | 4 | 5 | 4 | 4 | 4 | 4  | 4  | 3  | 3  | 2  |

Vậy số màu tối đa theo Welsh–Powell là 5.

Ngoài ra, vì đồ thị có chu trình $C_3$, nên số sắc tối thiểu phải lớn hơn hoặc bằng 3.

-   Lần tô màu thứ nhất:

    ![Colored 1](images/color2.png)

    Tô màu các đỉnh `4 9 3 11`.
-   Lần tô màu thứ hai:

    ![Colored 2](images/color3.png)

    Tô màu các đỉnh `5 2 6 7 8`.
-   Lần tô màu thứ ba:

    ![Colored 3](images/color4.png)

    Tô màu các đỉnh `0 1 10 12`.

#### Chứng minh

???+ note "Chứng minh"
    Với đồ thị vô hướng không khuyên $G$, giả sử $V(G):=\{v_1,v_2,\dots,v_n\}$ thỏa mãn
    
    $\deg(v_i)\geq\deg(v_{i+1}),~\forall 1\leq i\leq n-1$
    
    Đặt $V_0=\varnothing$, chọn dãy tập con $V_m$ của $V(G)\setminus\bigcup_{i=0}^{m-1} V_i$ sao cho:
    
    1.  $v_{k_m}\in V_m$, với $k_m=\min\{k:v_k\notin\bigcup_{i=0}^{m-1} V_i\}$
    2.  Nếu
    
        $\{v_{i_{m,1}},v_{i_{m,2}},\dots,v_{i_{m,l_m}}\}\subset V_m,~i_{m,1}<i_{m,2}<\dots<i_{m,l_m}$
    
        thì $v_j\in V_m$ khi và chỉ khi
    
        1.  $j>i_{m,l_m}$
        2.  $v_j$ không kề với bất kỳ $v_{i_{m,1}},v_{i_{m,2}},\dots,v_{i_{m,l_m}}$
    
    Nếu tô màu mỗi $V_i$ bằng màu thứ $i$, đây chính là phương án của Welsh–Powell. Rõ ràng:
    
    -   $V_1\neq\varnothing$
    -   $V_i\cap V_j=\varnothing\iff i\neq j$
    -   Tồn tại $\alpha(G)\in\Bbb{N}^*,\forall i>\alpha(G),~ V_i=\varnothing$
    
    Cần chứng minh:
    
    $\bigcup_{i=1}^{\alpha(G)} V_i=V(G)$
    
    Trong đó
    
    $\chi(G)\leq\alpha(G)\leq\max_{i=1}^n\min\{\deg(v_i)+1,i\}$
    
    Bất đẳng thức trái hiển nhiên, xét phải.
    
    Nếu $v\notin\bigcup_{i=1}^mV_i$, thì $v$ kề với ít nhất một đỉnh trong mỗi $V_1,V_2,\dots,V_m$, nên $\deg(v)\geq m$
    
    Do đó
    
    $v_j\in\bigcup_{i=1}^{\deg(v_j)+1}V_i$
    
    Mặt khác, theo cách xây dựng, $v_j\in\bigcup_{i=1}^j V_i$
    
    Kết hợp hai điều trên là xong.

## Tô màu cạnh

Tô màu các cạnh của đồ thị vô hướng sao cho hai cạnh kề nhau không cùng màu. Nếu $G$ là $k$-tô màu cạnh được, nhưng không $(k-1)$-tô màu cạnh được, thì $k$ gọi là số sắc cạnh của $G$, ký hiệu $\chi'(G)$.

### Định lý Vizing

Với đồ thị đơn $G$, ta có $\Delta(G) \leq \chi'(G) \leq \Delta(G) + 1$

Nếu $G$ là đồ thị hai phía (bipartite), thì $\chi'(G)=\Delta(G)$

Với $n$ lẻ ($n \neq 1$), $\chi'(K_n)=n$; với $n$ chẵn, $\chi'(K_n)=n-1$

### Chứng minh mang tính xây dựng của định lý Vizing cho đồ thị hai phía

???+ note "Chứng minh"
    Thêm cạnh vào đồ thị hai phía theo thứ tự.
    
    Khi thêm cạnh $(x,y)$, tìm màu nhỏ nhất chưa dùng ở $x$ và $y$, gọi là $l_x$ và $l_y$.
    
    Nếu $l_x=l_y$ thì tô màu cạnh này bằng $l_x$.
    
    Nếu $l_x<l_y$, thử đổi màu các cạnh nối từ $y$ có màu $l_x$ thành $l_y$.
    
    Quá trình đổi màu này là một đường tăng duy nhất bắt đầu từ $y$ qua các cạnh màu $l_x,l_y,\cdots$.
    
    Đường tăng hữu hạn nên có thể đảo màu trên đường tăng: màu $l_x$ thành $l_y$, màu $l_y$ thành $l_x$.
    
    Theo tính chất đồ thị hai phía, $x$ không thể nằm trên đường tăng, nếu không mâu thuẫn với việc $l_x$ là màu nhỏ nhất chưa dùng ở $x$.
    
    Sau khi đảo màu, tô cạnh $(x,y)$ bằng $l_x$.
    
    Độ phức tạp tổng $O(nm)$.

???+ note "Code mẫu [UVa10615 Rooks](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=18&page=show_problem&problem=1556)"
    ```cpp
    --8<-- "docs/graph/code/color/color_1.cpp"
    ```

??? note "Một ví dụ không đơn giản [uoj 444 Đồ thị hai phía](https://uoj.ac/problem/444)"
    Đây là bài tập vòng 1 đội tuyển năm 2018 do tác giả biên soạn.
    
    Nhận thấy đáp án nhỏ nhất là số đỉnh có bậc không chia hết cho $k$.
    
    Cách xây dựng: tách đỉnh của đồ thị hai phía.
    
    Nếu $degree \bmod k \neq 0$, tách thành $\lfloor degree/k \rfloor$ đỉnh bậc $k$ và một đỉnh bậc $degree \bmod k$.
    
    Nếu $degree \bmod k = 0$, tách thành $degree/k$ đỉnh bậc $k$.
    
    Các đỉnh tách ra vẫn giữ ý nghĩa như cũ, tức là mỗi cạnh có thể nối với bất kỳ đỉnh nào được tách ra từ đỉnh gốc.
    
    Theo định lý Vizing, có thể tô màu cạnh với $k$ màu.
    
    Phần xóa cạnh không liên quan trực tiếp đến định lý Vizing nên không trình bày ở đây.
    
    Bạn đọc có thể tham khảo lời giải chi tiết của tác giả.

## Đa thức tô màu

$P(G,k)$ là số cách tô màu $k$ màu khác nhau cho $G$.

$P(K_n, k) = k(k-1)\cdots(k-n+1)$

$P(N_n, k) = k^n$

Với đồ thị vô hướng không chu trình,

1.  Nếu $e=(v_i, v_j) \notin E(G)$, thì $P(G, k) = P(G \cup e, k)+P(G\setminus e, k)$
2.  Nếu $e=(v_i, v_j) \in E(G)$, thì $P(G,k)=P(G-e,k)-P(G\setminus e,k)$

Định lý: Gọi $V_1$ là tập cắt đỉnh của $G$, $G[V_1]$ là đồ thị đầy đủ bậc $|V_1|$, $G-V_1$ có $p(p \geq 2)$ thành phần liên thông, khi đó:

$P(G,k)=\frac{\Pi_{i=1}^{p}{(P(H_i, k))}}{P(G[V_1], k)^{p-1}}$

trong đó $H_i=G[V_1 \cup V(G_i)]$

## Tài liệu tham khảo

1.  [Graph coloring - Wikipedia](https://en.wikipedia.org/wiki/Graph_coloring)
2.  Welsh, D. J. A.; Powell, M. B. (1967), "[An upper bound for the chromatic number of a graph and its application to timetabling problems](https://doi.org/10.1093%2Fcomjnl%2F10.1.85)", The Computer Journal, 10 (1): 85–86
