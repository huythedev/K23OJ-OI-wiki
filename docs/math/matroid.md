## Giới thiệu

**Matroid (拟阵)** là một cấu trúc đại số trừu tượng do Hassler Whitney đề xuất năm 1935, nhằm thống nhất và khái quát khái niệm độc lập, chẳng hạn như độc lập tuyến tính trong đại số tuyến tính và tính không chu trình trong lý thuyết đồ thị.

Matroid cung cấp công cụ lý thuyết mạnh mẽ để xử lý các bài toán tối ưu liên quan đến tính độc lập, được ứng dụng rộng rãi trong tổ hợp, lý thuyết đồ thị, thiết kế thuật toán, đặc biệt là làm nền tảng toán học cho các phương pháp tối ưu kiểu tham lam.

## Định nghĩa

### Matroid

Một **matroid (拟阵)** có thể biểu diễn dưới dạng $M = (E, \mathcal{I})$, trong đó:

-   $E$ là một tập hữu hạn, gọi là **tập nền (Ground Set)**.
-   $\mathcal{I}$ là một họ các tập con của $E$, gọi là **họ tập độc lập (Family of Independent Sets)**, trong đó các tập được gọi là **tập độc lập (Independent Set)**. Thỏa ba tính chất:

    -   **Không rỗng**: Tập rỗng là độc lập, tức $\emptyset \in \mathcal{I}$.

    -   **Di truyền**: Mọi tập con của một tập độc lập cũng là độc lập. Nếu $I \in \mathcal{I}$ thì với mọi $I' \subseteq I$ đều có $I' \in \mathcal{I}$.

    -   **Mở rộng**: Nếu $I, J \in \mathcal{I}$ và $|I| < |J|$ thì tồn tại $j \in J \setminus I$ sao cho $I \cup \{j\} \in \mathcal{I}$.

Nếu một cấu trúc dạng $(E, \mathcal{I})$ thỏa ba tính chất trên thì gọi là một matroid.

### Cơ sở

**Cơ sở (Basis)** là tập độc lập cực đại trong matroid, tức không thể thêm phần tử nào mà vẫn độc lập. Tập tất cả các cơ sở gọi là **họ cơ sở**, ký hiệu $\mathcal{B}$.

**Tính chất**:

1.  **Đồng kích thước**: Mọi cơ sở có cùng kích thước, gọi là **hạng (Rank)** của matroid.

2.  **Mở rộng**: Bất kỳ tập độc lập nào cũng có thể thêm các phần tử từ một cơ sở để mở rộng thành một cơ sở.

### Chu trình

**Chu trình (Circuit)** là tập phụ thuộc tối tiểu trong matroid, tức mọi tập con thực sự đều độc lập nhưng bản thân nó không độc lập, và giữa hai chu trình bất kỳ không có quan hệ bao hàm.

### Hạng

**Hàm hạng (Rank Function)** $r: 2^E \rightarrow \mathbb{Z}_{\geq 0}$ ánh xạ một tập con của $E$ tới số nguyên không âm. Với mọi $S \subseteq E$, $r(S)$ là kích thước của tập độc lập lớn nhất trong $S$, tức

$$
r(S) = \max \{ |I| \mid I \subseteq S \wedge I \in \mathcal{I} \}.
$$

**Tính chất**:

1.  **Không âm**: Với mọi $S \subseteq E$, có $0 \leq r(S) \leq |S|$.

2.  **Đơn điệu**: Nếu $A \subseteq B \subseteq E$ thì $r(A) \leq r(B)$.

3.  **Tính dưới mô-đun**: Với mọi $A, B \subseteq E$, có $r(A \cup B) + r(A \cap B) \leq r(A) + r(B)$.

## Ví dụ tiêu biểu

### 1. Matroid đều (Uniform Matroid)

**Định nghĩa**: Cho tập nền $E$ và số nguyên không âm $k$, matroid đều $U_{k,E}$ có họ tập độc lập là mọi tập con có kích thước không vượt quá $k$:

$$
\mathcal{I} = \{ I \subseteq E \mid |I| \leq k \}．
$$

-   **Cơ sở**: Mọi tập con kích thước $k$.
-   **Chu trình**: Mọi tập con kích thước $k + 1$.
-   **Hạng**: $r(E) = \min(k, |E|)$, tức nhiều nhất có $k$ phần tử độc lập.

### 2. Matroid đồ thị (Graphical Matroid)

**Định nghĩa**: Cho đồ thị vô hướng $G = (V, E)$, matroid đồ thị $M(G)$ có tập nền là tập cạnh $E$, họ độc lập là các tập cạnh không chứa chu trình, tức các rừng.

-   **Cơ sở**: Cây khung (trong trường hợp đồ thị liên thông). Cây khung là tập độc lập cực đại, không thể thêm cạnh mà không tạo chu trình.
-   **Chu trình**: Các chu trình đơn trong đồ thị; bỏ bất kỳ cạnh nào thì phần còn lại độc lập.
-   **Hạng**: $r(E) = |V|  - c$, trong đó $c$ là số thành phần liên thông. Với đồ thị liên thông, $r(E) = |V| - 1$.

### 3. Matroid tuyến tính (Linear Matroid)

**Định nghĩa**: Matroid tuyến tính dựa trên không gian vector. Cho không gian vector $V$, tập nền $E$ là một tập hữu hạn vector trong $V$, họ độc lập là các tập con độc lập tuyến tính.

-   **Cơ sở**: Tập vector độc lập tuyến tính cực đại, kích thước bằng số chiều của không gian.
-   **Chu trình**: Tập vector phụ thuộc tuyến tính tối tiểu; mọi tập con thực sự độc lập, còn bản thân là phụ thuộc.
-   **Hạng**: $r(E) = \dim(V)$, tức kích thước tập độc lập không vượt quá số chiều.

### 4. Matroid phân hoạch (Partition Matroid)

**Định nghĩa**: Chia tập nền $E$ thành các tập con rời nhau $E_1, E_2, \dots, E_m$, và với mỗi $E_i$ gán một số nguyên không âm $k_i$. Họ độc lập là các tập con thỏa mỗi phần chọn không quá $k_i$ phần tử:

$$
\mathcal{I} = \left\{ I \subseteq E \mid \forall i,\, |I \cap E_i|  \leq k_i \right\}．
$$

-   **Cơ sở**: Tập độc lập thỏa $|I \cap E_i| = k_i$ với mọi $i$.
-   **Chu trình**: Tập phụ thuộc tối tiểu chứa ít nhất một phần mà số phần tử vượt $k_i$.
-   **Hạng**: $r(E) = \sum_{i=1}^m k_i$.

### 5. Matroid có màu (Colored Matroid)

**Định nghĩa**: Là trường hợp đặc biệt của matroid phân hoạch, mỗi phần tử có một màu. Cho tập nền $E$ và tập màu $C$, mỗi $e \in E$ gắn với một màu $c \in C$. Họ độc lập ngoài điều kiện độc lập còn phải thỏa ràng buộc về màu, ví dụ mỗi màu chỉ được chọn tối đa một số lượng.

-   **Cơ sở**: Tập độc lập cực đại thỏa ràng buộc màu.
-   **Chu trình**: Tập phụ thuộc tối tiểu, chứa ít nhất một vi phạm độc lập hoặc vi phạm ràng buộc màu.
-   **Hạng**: Kích thước tối đa của tập độc lập dưới các ràng buộc màu.

## Xây dựng và phép toán

### Đối ngẫu

Cho matroid $M = (E, \mathcal{I})$, **matroid đối ngẫu** $M^* = (E, \mathcal{I}^*)$ được định nghĩa bởi:

$$
\mathcal{I}^* = \{ I^* \subseteq E \mid \exists B \in \mathcal{I}, |B| = r(E), B \subseteq E \setminus I^* \}．
$$

**Tính chất**:

-   **Cơ sở**: Cơ sở của $M^*$ là phần bù trong $E$ của cơ sở của $M$. Nếu $B$ là cơ sở của $M$ thì $E \setminus B$ là cơ sở của $M^*$.
-   **Hàm hạng**: $r^*(S) = |S| - r(E) + r(E \setminus S)$.
-   **Tự phản**: $(M^*)^* = M$.

**Ví dụ**:

Với đồ thị vô hướng $G = (V, E)$, matroid đồ thị $M(G)$ có đối ngẫu $M(G)^*$ là matroid các cắt. Cơ sở của $M(G)$ là các cây khung, còn cơ sở của $M(G)^*$ là phần bù của các cây khung; chu trình của $M(G)^*$ là các cắt tối tiểu.

Ví dụ, đồ thị tam giác $G$ có $E = \{e_1, e_2, e_3\}$. Cơ sở của $M(G)$ là hai cạnh (như $\{e_1, e_2\}$), cơ sở của $M(G)^*$ là một cạnh (như $\{e_3\}$), và chu trình của $M(G)^*$ là hai cạnh tạo cắt tối tiểu (như $\{e_2,e_3\}$).

### Xóa và co

**Xóa (Deletion)**:

Với $A \subseteq E$, matroid $M$ sau khi xóa $A$ là $M \setminus A$, họ độc lập $\mathcal{I}'$:

$$
\mathcal{I}' = \{ I \subseteq E \setminus A \mid I \in \mathcal{I} \}．
$$

Xóa chỉ loại bỏ các phần tử, giữ nguyên tính độc lập của phần còn lại.

**Co (Contraction)**:

Với $A \subseteq E$, matroid $M$ sau khi co $A$ là $M / A$, họ độc lập $\mathcal{I}''$:

$$
\mathcal{I}'' = \left\{ I \subseteq E \setminus A \,\bigg|\, \exists B \subseteq A,\, B \in \mathcal{I},\, r(B) = r(A),\, I \cup B \in \mathcal{I} \right\}
$$

Co có thể hiểu là co các phần tử trong $A$ và xét các tập độc lập còn lại khi kết hợp với một cơ sở của $A$.

**Ví dụ - Matroid đồ thị**:

-   **Xóa**: Xóa một cạnh trong đồ thị. Các tập cạnh còn lại tạo rừng là độc lập. Ví dụ xóa một cạnh trong tam giác thì hai cạnh còn lại vẫn là rừng.
-   **Co**: Co một cạnh thành một đỉnh. Trong tam giác, co một cạnh sẽ hợp nhất hai đỉnh, hai cạnh còn lại tạo một matroid mới.

## Matroid và tham lam

**Mô tả bài toán**:

Cho matroid $M = (S, \mathcal{I})$, mỗi phần tử $x \in S$ có trọng số dương $w(x)$. Mục tiêu là tìm tập độc lập có tổng trọng số lớn nhất:

$$
\max_{A \in \mathcal{I}} w(A) = \max_{A \in \mathcal{I}} \sum_{x \in A} w(x)
$$

Tập tối ưu phải là tập độc lập cực đại.

### Các bước

1.  **Sắp xếp phần tử**: Sắp theo trọng số giảm dần, được $e_1, e_2, \dots, e_n$.
2.  **Khởi tạo**: $A = \emptyset$.
3.  **Xây dựng tập độc lập**: Duyệt $e_i$, nếu $A \cup \{ e_i \} \in \mathcal{I}$ thì cập nhật $A$.
4.  **Kết quả**: $A$ là tập độc lập có tổng trọng số lớn nhất.

**Phân tích độ phức tạp**:

Gọi $n = |S|$, $f(n)$ là độ phức tạp kiểm tra độc lập, thì:

$$
O(n \log n + n f(n))
$$

Trong đó $O(n \log n)$ là sắp xếp, $O(n f(n))$ là kiểm tra độc lập.

???+ note "Ghi chú"
    -   Trong matroid đồ thị, dùng [DSU](../ds/dsu.md) để kiểm tra chu trình, $f(n)$ gần hằng số.
    -   Trong matroid tuyến tính, kiểm tra độc lập thường là phép toán ma trận, độ phức tạp phụ thuộc cài đặt.

**Chứng minh đúng đắn**:

Cho $M = (S, \mathcal{I})$, $A \in \mathcal{I}$ là tập độc lập và $A$ là tập con của một tập tối ưu $T$. Đặt $P = \{ x \in S \setminus A \mid A \cup \{x\} \in \mathcal{I} \}$.

Lấy $y$ là phần tử có trọng số lớn nhất trong $P$, khi đó $A' = A \cup \{ y \}$ cũng là tập con của một tập tối ưu.

Giả sử ngược lại, $A'$ không là tập con của bất kỳ tập tối ưu nào, tồn tại tập tối ưu $T$ với $|A'| < |T|$.

Do **mở rộng**, tồn tại $x \in T \setminus A'$ sao cho $A' \cup \{ x \} \in \mathcal{I}$. Lặp lại, thu được $A''$ với $|A''| = |T|$.

Đặt $K = A'' \cap T$, khi đó $x = T \setminus K$, $y = A'' \setminus K$. Vì $y$ là lớn nhất trong $P$, có $w(x) \leq w(y)$.

Suy ra $w(A'') = w(K) + w(y) \geq w(K) + w(x) = w(T)$:

-   Nếu $w(A'') > w(T)$, mâu thuẫn với tối ưu của $T$.
-   Nếu $w(A'') = w(T)$, thì $A''$ là tối ưu và $A'$ là tập con, mâu thuẫn giả thiết.

Do đó giả sử sai, và thuật toán tham lam đúng.

### Ví dụ

**Cây khung nhỏ nhất**:

Cho đồ thị vô hướng liên thông $G = (V, E)$, mỗi cạnh có trọng số $w(e)$. Mục tiêu tìm cây khung có tổng trọng số nhỏ nhất.

**Xây dựng matroid**:

-   **Tập nền**: $S = E$.
-   **Họ độc lập**: $\mathcal{I}$ là các tập cạnh không chứa chu trình (rừng).

**Thuật toán tham lam**:

Trong khuôn khổ matroid đồ thị, [Kruskal](../graph/mst.md#kruskal-算法) là ví dụ điển hình. [Prim](../graph/mst.md#prim-算法) cũng là tham lam nhưng không dựa chặt vào tính mở rộng của matroid, nên Kruskal được coi là ví dụ chuẩn.

-   **Kruskal**:
    1.  **Sắp xếp cạnh**: tăng dần theo trọng số.
    2.  **Chọn dần**: thêm cạnh nhỏ nhất nếu không tạo chu trình.
    3.  **Dừng**: khi có $|V| - 1$ cạnh.

-   **Prim**:
    -   **Nguyên lý**: mở rộng cây từ một đỉnh, mỗi bước chọn cạnh nhỏ nhất nối trong và ngoài.
    -   Dù là tham lam, không phải ví dụ chuẩn của tham lam matroid.

## Giao của matroid

Cho hai matroid $M_1 = (S, \mathcal{I}_1)$ và $M_2 = (S, \mathcal{I}_2)$ trên cùng tập nền $S$. Nếu $\mathcal{I} = \mathcal{I}_1 \cap \mathcal{I}_2$ thỏa ba tính chất của họ độc lập, thì $M = (S, \mathcal{I})$ là **giao** của $M_1$ và $M_2$.

**Lưu ý**: Giao của hai matroid không luôn là matroid; chỉ khi thỏa đủ ba tính chất.

### Mô tả bài toán

1.  **Tập độc lập lớn nhất**: tìm tập độc lập lớn nhất trong $\mathcal{I}_1 \cap \mathcal{I}_2$.
2.  **Tập độc lập có trọng số lớn nhất**: với trọng số $w: S \to \mathbb{R}$, tìm tập độc lập có tổng trọng số lớn nhất trong $\mathcal{I}_1 \cap \mathcal{I}_2$.

### Thuật toán

**Phi trọng số**:

1.  **Khởi tạo**: chọn $I \in \mathcal{I}_1 \cap \mathcal{I}_2$, thường $I = \emptyset$.
2.  **Lặp**:
    -   **Xây đồ thị trao đổi**: dựng $D_{M_1, M_2}(I)$.
    -   **Tìm đường**: tìm đường tăng từ $s$ đến $t$.
    -   **Tăng**: duyệt đường $P$:
        -   Nếu thuộc trái ($I$), loại khỏi $I$.
        -   Nếu thuộc phải ($S \setminus I$), thêm vào $I$.
    -   **Lặp lại** đến khi không còn đường tăng.
3.  **Kết quả**: $I$ là tập độc lập lớn nhất.

**Có trọng số**:

1.  **Gán trọng số**: mỗi phần tử $e$ có
    -   **Trái ($I$)**: $w'(e) = -w(e)$.
    -   **Phải ($S \setminus I$)**: $w'(e) = w(e)$.
2.  **Chọn đường**: tìm đường tăng $P$ sao cho tăng tổng trọng số lớn nhất.
    -   **Điều kiện tăng**: $\sum_{y \in \text{thêm}} w(y) > \sum_{x \in \text{bỏ}} w(x)$
3.  **Tăng**: cập nhật như trên.
4.  **Lặp**: đến khi không còn đường thỏa điều kiện.
5.  **Kết quả**: tập độc lập có trọng số lớn nhất.

**Độ phức tạp**:

-   **Số lần tăng**: tối đa $\min(r_1, r_2)$.
-   **Mỗi lần tăng**:
    -   Dựng đồ thị: $O(n^2)$.
    -   Tìm đường tăng: thường $O(n^2)$.
-   **Tổng**: $O(r \cdot n^2)$ với $r = \min(r_1, r_2)$.

## Bài tập mẫu

**Cây khung nhỏ nhất**:

Cho đồ thị vô hướng $G = (V, E)$, mỗi cạnh $e$ có trọng số $w(e)$. Tìm cây khung nhỏ nhất.

-   Xem chi tiết: [Cây khung nhỏ nhất](../graph/mst.md).
-   Bài mẫu: [Luogu P3366](https://www.luogu.com.cn/problem/P3366).

??? note "Hướng giải"
    Dùng Kruskal: sắp cạnh tăng dần, thêm cạnh nếu không tạo chu trình.

**Colorful Graph**:

Đồ thị vô hướng $G = (V, E)$ với cạnh có màu. Tìm tập cạnh lớn nhất sao cho:

1.  Không có chu trình.
2.  Mỗi màu không quá $k$ cạnh.

??? note "Hướng giải"
    1.  **Mô hình matroid**:
    
        -   **Matroid đồ thị ($M_1$)**: các tập cạnh không tạo chu trình.
        -   **Matroid màu ($M_2$)**: mỗi màu có số cạnh $\leq k$.
    2.  **Giải giao matroid**: tìm tập lớn nhất trong $M_1 \cap M_2$.

**Bài toán phân bổ tài nguyên có ràng buộc**:

Có tài nguyên $R = \{r_1, r_2, \dots, r_n\}$ và dự án $P = \{p_1, p_2, \dots, p_m\}$. Mỗi dự án cần một số tài nguyên, và tổng phân bổ mỗi loại không vượt cung.

**Mục tiêu**: tìm phương án phân bổ thỏa nhu cầu và không vượt cung.

??? note "Hướng giải"
    1.  **Mô hình matroid**:
    
        -   **Matroid nhu cầu ($M_1$)**: các phương án thỏa nhu cầu dự án.
        -   **Matroid cung ($M_2$)**: các phương án không vượt cung.
    2.  **Giải giao matroid**: tìm phương án trong $M_1 \cap M_2$.

## Tài liệu tham khảo và chú thích

1.  [Wikipedia - Matroid](https://en.wikipedia.org/wiki/Matroid)
2.  [百度百科 - 拟阵](https://baike.baidu.com/item/%E6%8B%9F%E9%98%B5)
3.  [洛谷 - 拟阵与最优化问题](https://www.luogu.com.cn/article/87d02q9f)
4.  [洛谷 - 从拟阵基础到 Shannon 开关游戏](https://www.luogu.com.cn/article/fuj3x886)
