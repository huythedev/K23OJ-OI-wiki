## Dẫn nhập

Tiền tố tổng (prefix sum) và hiệu sai phân là những kỹ thuật thường dùng trong lập trình thi đấu, tiền tố tổng dùng để truy vấn tổng đoạn nhanh, còn hiệu sai phân dùng để cập nhật đoạn hiệu quả.

???+ tip "Quy ước"
    Để thuận tiện, mặc định trong bài này mảng $\{a_i\}$ đánh chỉ số từ $1$, và bổ sung $a_0 = 0$.

## Tiền tố tổng

Tiền tố tổng có thể hiểu đơn giản là "tổng $n$ phần tử đầu của dãy", là một kỹ thuật tiền xử lý quan trọng.

### Tiền tố tổng một chiều

Với dãy $\{a_i\}$ độ dài $n$, nếu cần truy vấn nhiều lần tổng đoạn $[l,r]$, ta có thể dùng tiền tố tổng:

$$
S_{i} = \sum_{j=1}^i a_j.
$$

Có thể tính dần theo công thức truy hồi:

$$
S_0 = 0,~ S_i = S_{i-1} + a_i
$$

Khi đó, tổng đoạn $[l,r]$ chỉ cần lấy hiệu:

$$
S([l,r]) = S_r - S_{l-1}.
$$

Như vậy, sau $O(n)$ tiền xử lý, mỗi truy vấn tổng đoạn chỉ còn $O(1)$.

???+ example "Cài đặt mẫu"
    === "C++"
        ```cpp
        --8<-- "docs/basic/code/prefix-sum/prefix-sum_1.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/basic/code/prefix-sum/prefix-sum_1.py:core"
        ```

Thư viện chuẩn C++ có hàm tiền tố tổng [`std::partial_sum`](https://zh.cppreference.com/w/cpp/algorithm/partial_sum) trong `<numeric>`. Từ C++17 còn có [`std::inclusive_scan`](https://zh.cppreference.com/w/cpp/algorithm/inclusive_scan) cũng trong `<numeric>`.

### Tiền tố tổng đa chiều

Tiền tố tổng một chiều có thể mở rộng lên nhiều chiều. Có hai cách tính tiền tố tổng đa chiều thường gặp.

#### Dựa trên nguyên lý bao hàm loại trừ

Cách này thường dùng cho tiền tố tổng hai chiều. Cho mảng $A$ kích thước $m\times n$, cần tính tiền tố tổng $S$ cũng kích thước $m\times n$:

$$
S_{i,j} = \sum_{i'\le i}\sum_{j'\le j}A_{i',j'}.
$$

Tương tự một chiều, $S_{i,j}$ có thể tính từ $S_{i-1,j}$ hoặc $S_{i,j-1}$, nhưng nếu cộng cả hai rồi cộng $A_{i,j}$ sẽ bị tính trùng $S_{i-1,j-1}$, nên phải trừ đi phần này (nguyên lý bao hàm loại trừ):

$$
S_{i,j} = A_{i,j} + S_{i-1,j} + S_{i,j-1} - S_{i-1,j-1}. 
$$

Cứ duyệt $(i,j)$ và tính theo công thức trên.

???+ note "Ví dụ"
    Xét ví dụ cụ thể:
    
    ![二维前缀和示例](./images/prefix-sum-2d.svg)
    
    $S$ là tiền tố tổng của ma trận $A$. Theo định nghĩa, $S_{3,3}$ là tổng các phần tử trong khung nét đứt. $S_{3,2}$ là tổng vùng xanh, $S_{2,3}$ là tổng vùng đỏ, phần giao là $S_{2,2}$. Nếu cộng $S_{3,2}$ và $S_{2,3}$ sẽ bị tính trùng $S_{2,2}$, nên:

    $$
    S_{3,3} = A_{3,3} + S_{2,3} + S_{3,2} - S_{2,2} = 5 + 18 + 15 - 9 = 29.
    $$

Sau khi có tiền tố tổng hai chiều, muốn truy vấn tổng hình chữ nhật từ $(i_1,j_1)$ đến $(i_2,j_2)$:

$$
S_{i_2,j_2} - S_{i_1-1,j_2} - S_{i_2,j_1-1} + S_{i_1-1,j_1-1}.
$$

Chỉ mất $O(1)$ thời gian.

Với hai chiều, độ phức tạp là $O(mn)$. Nhưng với $k$ chiều, số lượng các thành phần bao hàm loại trừ tăng theo $2^k$, nên độ phức tạp là $O(2^kN)$ với $N$ là số phần tử, không còn hiệu quả với $k$ lớn.

???+ example "[洛谷 P1387 Hình vuông lớn nhất](https://www.luogu.com.cn/problem/P1387)"
    Trong ma trận $n\times m$ chỉ gồm $0$ và $1$, tìm hình vuông lớn nhất không chứa $0$, in ra cạnh.

??? note "Code mẫu"
    === "C++"
        ```cpp
        --8<-- "docs/basic/code/prefix-sum/prefix-sum_2.cpp:full-text"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/basic/code/prefix-sum/prefix-sum_2.py:full-text"
        ```

#### Tiền tố tổng từng chiều

Với mảng $k$ chiều $A$ kích thước $N$, cũng có thể tính tiền tố tổng $S$ như sau:

$$
S_{i_1,\cdots,i_k} = \sum_{i'_1\le i_1}\cdots\sum_{i'_k\le i_k} A_{i'_1,\cdots,i'_k}.
$$

Tức là, mỗi lần chỉ tính tiền tố tổng theo một chiều, cố định các chiều còn lại, lặp lại cho $k$ chiều. Độ phức tạp $O(kN)$, thường chấp nhận được.

??? example "Code mẫu tiền tố tổng 3 chiều"
    ```cpp
    --8<-- "docs/basic/code/prefix-sum/prefix-sum_4.cpp:core"
    ```

#### Trường hợp đặc biệt: DP tổng trên tập con (SOS DP)

Khi $k$ lớn, thường gặp trong bài toán **tổng trên tập con** (sum over subsets, SOS). Đây là trường hợp đặc biệt của tiền tố tổng nhiều chiều.

Bài toán: Cho hàm $f$ trên tập các tập con của $n$ phần tử, tính hàm $g$:

$$
g(S) = \sum_{T\subseteq S}f(T).
$$

Tức là $g(S)$ là tổng $f(T)$ trên mọi tập con $T$ của $S$.

Có thể coi $f$ là mảng $n$ chiều, mỗi chiều chỉ nhận $0$ hoặc $1$. Quan hệ tập con tương ứng với quan hệ chỉ số: $T\subseteq S \iff \forall i(t_i \le s_i)$. Vậy tổng trên tập con chính là tiền tố tổng nhiều chiều.

Có thể dùng cách trên để tính, độ phức tạp $O(n2^n)$.

??? example "Code mẫu"
    ```cpp
    --8<-- "docs/basic/code/prefix-sum/prefix-sum_5.cpp:core"
    ```

Phép nghịch đảo của tổng trên tập con cần dùng [nguyên lý bao hàm loại trừ](../math/combinatorics/inclusion-exclusion-principle.md). Đây cũng là bước cần thiết trong biến đổi Möbius nhanh.

### Tiền tố tổng trên cây

Tiền tố tổng một chiều còn có thể mở rộng lên cây có gốc (gốc là $1$). Sau khi tiền xử lý, có thể truy vấn nhanh tổng trọng số trên đường đi.

#### Trường hợp trọng số trên nút

Giả sử mỗi nút $x$ có trọng số $a_x$. Có thể tính tiền tố tổng $S_x$ từ gốc đến $x$ theo:

$$
S_1 = a_1,~ S_{x} = S_{\operatorname{fa}(x)} + a_x
$$

Sau khi tiền xử lý, tổng trọng số trên đường đi từ $x$ đến $y$ là:

$$
S_x + S_y - S_{\operatorname{lca}(x, y)} - S_{\operatorname{fa}(\operatorname{lca}(x, y))}
$$

với $\operatorname{lca}(x, y)$ là [tổ tiên chung gần nhất](../graph/lca.md).

#### Trường hợp trọng số trên cạnh

Trọng số trên cạnh có thể chuyển về trọng số trên nút. Với mỗi nút $x\neq 1$, gọi $\operatorname{edge}(x)$ là cạnh nối $x$ với cha $\operatorname{fa}(x)$. Gán trọng số cạnh vào nút xa gốc hơn, gốc nhận $0$. Khi đó, dùng công thức trên để tính tiền tố tổng cạnh.

Tổng trọng số trên đường đi từ $x$ đến $y$ là:

$$
S_x + S_y - 2S_{\operatorname{lca}(x, y)}
$$

Khác với trường hợp trọng số trên nút, không tính trọng số tại $\operatorname{lca}(x, y)$.

#### Tổng trọng số dưới cây con

Khác với mảng, trên cây hướng, tổng tiền tố từ dưới lên (từ lá lên gốc) và từ trên xuống (từ gốc xuống lá) cho kết quả khác nhau. Thông thường, "tiền tố tổng trên cây" là từ gốc xuống. Để phân biệt, gọi tổng từ dưới lên là **tổng dưới cây con**.

Tổng trọng số dưới cây con gốc $x$ là:

$$
T_x = \sum_{y\in\operatorname{desc}(x)} a_x.
$$

với $\operatorname{desc}(x)$ là tập các con cháu (kể cả $x$). Tổng dưới cây con không dùng để truy vấn nhanh tổng đường đi, nhưng lại hữu ích cho phần hiệu sai phân trên cây.

## Hiệu sai phân

Hiệu sai phân là phép toán ngược với tiền tố tổng. Trong thi đấu, thường dùng hiệu sai phân để thực hiện nhiều phép cộng đoạn, sau đó dùng tiền tố tổng để khôi phục giá trị từng phần tử. Lưu ý phải thực hiện cập nhật trước khi truy vấn.

Nếu cần hỗ trợ cập nhật và truy vấn lẫn nhau, hãy dùng [Fenwick tree](../ds/fenwick.md), nhưng ý tưởng vẫn giống nhau.

### Hiệu sai phân một chiều

Với dãy $\{a_i\}$, hiệu sai phân $\{D_i\}$ là:

$$
D_i = a_i - a_{i-1},~ a_0 = 0.
$$

Thư viện chuẩn C++ có hàm [`std::adjacent_difference`](https://zh.cppreference.com/w/cpp/algorithm/adjacent_difference) trong `<numeric>`.

Quan hệ giữa tiền tố tổng và hiệu sai phân:

???+ note "Tính chất"
    Nếu $\{D_i\}$ là hiệu sai phân của $\{a_i\}$, thì:

    -   $\{a_i\}$ là tiền tố tổng của $\{D_i\}$:
    
        $$
        a_i = \sum_{j=1}^i D_i.
        $$
    -   Tiền tố tổng của $\{a_i\}$ là:
    
        $$
        S_i = \sum_{j=1}^i\sum_{k=1}^jD_k = \sum_{j=1}^i(i-j+1)D_j. 
        $$

Hiệu sai phân thường dùng để thực hiện nhiều phép cộng đoạn, sau đó truy vấn giá trị từng phần tử.

Giả sử cần cộng $v$ cho tất cả $a_i$ với $l\leq i\leq r$, chỉ cần:

$$
D_{l} \gets D_{l} + v,~ D_{r+1}\gets D_{r+1} - v.
$$

Sau khi cập nhật xong, dùng tiền tố tổng để khôi phục dãy $a_i$. Mỗi lần cập nhật $O(1)$, truy vấn sau khi tiền xử lý là $O(1)$.

???+ example "Code mẫu"
    ```cpp
    --8<-- "docs/basic/code/prefix-sum/prefix-sum_6.cpp:core"
    ```

### Hiệu sai phân đa chiều

Hiệu sai phân cũng mở rộng được cho nhiều chiều, là phép toán ngược với tiền tố tổng đa chiều. Có thể dùng nguyên lý bao hàm loại trừ, ví dụ hiệu sai phân hai chiều:

$$
D_{i,j} = a_{i,j} - a_{i-1,j} - a_{i,j-1} + a_{i-1,j-1}.
$$

Tuy nhiên, cách hiệu quả hơn là thực hiện hiệu sai phân từng chiều.

Hiệu sai phân hai chiều thường dùng để thực hiện nhiều phép cộng hình chữ nhật. Để cộng $v$ cho mọi phần tử trong hình chữ nhật từ $(x_1,y_1)$ đến $(x_2,y_2)$, chỉ cần:

$$
\begin{aligned}
D_{x_1,y_1} &\gets D_{x_1,y_1} + v, \\
D_{x_1,y_2+1} &\gets D_{x_1,y_2+1} - v,\\
D_{x_2+1,y_1} &\gets D_{x_2+1,y_1} - v,\\
D_{x_2+1,y_2+1} &\gets D_{x_2+1,y_2+1} + v.
\end{aligned}
$$

Sau khi cập nhật xong, chỉ cần tính tiền tố tổng hai chiều để khôi phục mảng.

??? example "Code mẫu"
    ```cpp
    --8<-- "docs/basic/code/prefix-sum/prefix-sum_7.cpp:core"
    ```

Tương tự, với $k>2$ chiều cũng làm được, nhưng mỗi lần cập nhật tốn $O(2^k)$.

### Hiệu sai phân trên cây

Hiệu sai phân cũng mở rộng lên cây có gốc, dùng để thực hiện phép cộng đoạn trên đường đi. Tùy vào việc lưu thông tin ở nút hay cạnh, có hai loại: **hiệu sai phân trên nút** và **hiệu sai phân trên cạnh**.

Khác với tiền tố tổng trên cây, hiệu sai phân thường dùng để cập nhật xong rồi tính tổng dưới cây con để truy vấn.

#### Hiệu sai phân trên nút

Để cộng $v$ cho tất cả các nút trên đường đi từ $x$ đến $y$, chỉ cần:

$$
\begin{aligned}
D_x &\gets D_x + v, \\
D_{\operatorname{lca}(x, y)} &\gets D_{\operatorname{lca}(x, y)} - v,\\
D_y &\gets D_y + v, \\
D_{\operatorname{fa}(\operatorname{lca}(x, y))} &\gets D_{\operatorname{fa}(\operatorname{lca}(x, y))} - v.
\end{aligned}
$$

Sau khi cập nhật xong, tính tổng dưới cây con để lấy giá trị từng nút.

???+ example "Ví dụ"
    Khi cộng đoạn trên đường đi từ $S$ đến $T$, hai dòng đầu là cộng trên đoạn xanh, hai dòng sau là cộng trên đoạn đỏ:
    
    ![](./images/prefix_sum1.svg)
    
    Tính tổng dưới cây con từ dưới lên là đúng.

#### Hiệu sai phân trên cạnh

Để cộng $v$ cho tất cả các cạnh trên đường đi từ $x$ đến $y$, chỉ cần:

$$
\begin{aligned}
D_x &\gets D_x + v, \\
D_y &\gets D_y + v, \\
D_{\operatorname{lca}(x, y)} &\gets D_{\operatorname{lca}(x, y)} - 2v.
\end{aligned}
$$

Sau khi cập nhật xong, tính tổng dưới cây con để lấy giá trị từng cạnh.

???+ example "Ví dụ"
    Như hình, hiệu sai phân trên cạnh dùng để cộng đoạn trên các cạnh màu đỏ.
    
    ![](./images/prefix_sum2.svg)
    
    Vì khó cộng trực tiếp trên cạnh, nên chuyển về cộng trên nút liền kề. So sánh với hiệu sai phân trên nút sẽ hiểu công thức này.

### Bài tập ví dụ

???+ example "[洛谷 3128 Lưu lượng lớn nhất](https://www.luogu.com.cn/problem/P3128)"
    FJ lắp $N(2 \le N \le 50,000)$ đường ống nối $N-1$ kho, đánh số $1$ đến $N$, tất cả đều liên thông.

    Có $K(1 \le K \le 100,000)$ tuyến vận chuyển sữa, tuyến $i$ từ kho $s_i$ đến $t_i$. Mỗi tuyến làm tăng áp lực lên các kho trên đường đi thêm $1$. Hỏi kho nào chịu áp lực lớn nhất.

??? note "Hướng dẫn giải"
    Cần đếm số lần mỗi nút bị đi qua, dùng hiệu sai phân trên cây, mỗi lần cộng $1$ trên đường đi. Dùng phương pháp bội số hóa để tính LCA, cuối cùng duyệt DFS toàn bộ cây, khi quay lui cộng dồn hiệu sai phân để lấy đáp án.

??? note "Code mẫu"
    ```cpp
    --8<-- "docs/basic/code/prefix-sum/prefix-sum_3.cpp"
    ```

## Bài tập

Tiền tố tổng:

-   [洛谷 B3612【深进 1. 例 1】Tổng đoạn](https://www.luogu.com.cn/problem/B3612)
-   [洛谷 U69096 Nghịch đảo tiền tố tổng](https://www.luogu.com.cn/problem/U69096)
-   [AtCoder joi2007ho\_a Tổng lớn nhất](https://atcoder.jp/contests/joi2007ho/tasks/joi2007ho_a)
-   [「USACO16JAN」Các đoạn con tổng 7](https://www.luogu.com.cn/problem/P3131)
-   [「USACO05JAN」Moo Volume S](https://www.luogu.com.cn/problem/P6067)

Tiền tố tổng đa chiều:

-   [HDU 6514 Monitor](https://acm.hdu.edu.cn/showproblem.php?pid=6514)
-   [洛谷 P1387 Hình vuông lớn nhất](https://www.luogu.com.cn/problem/P1387)
-   [「HNOI2003」Bom laser](https://www.luogu.com.cn/problem/P2280)
-   [CF 165E Compatible Numbers](https://codeforces.com/contest/165/problem/E)
-   [CF 383E Vowels](https://codeforces.com/problemset/problem/383/E)
-   [ARC 100C Or Plus Max](https://atcoder.jp/contests/arc100/tasks/arc100_c)

Tiền tố tổng trên cây:

-   [LOJ 10134.Dis](https://loj.ac/problem/10134)
-   [LOJ 2491. Tổng](https://loj.ac/problem/2491)

Hiệu sai phân:

-   [Fenwick tree 3: Cập nhật đoạn, truy vấn đoạn](https://loj.ac/problem/132)
-   [「Poetize6」Dãy tăng giảm](https://www.luogu.com.cn/problem/P4552)
-   [洛谷 P4231 Ba bước tất sát](https://www.luogu.com.cn/problem/P4231)

Hiệu sai phân đa chiều:

-   [洛谷 P3397 Thảm](https://www.luogu.com.cn/problem/P3397)
-   [洛谷 P8228「Wdoi-5」Lò phản ứng mô-đun hóa](https://www.luogu.com.cn/problem/P8228)

Hiệu sai phân trên cây:

-   [洛谷 3128 Lưu lượng lớn nhất](https://www.luogu.com.cn/problem/P3128)
-   [JLOI2014 Nhà mới của sóc](https://loj.ac/problem/2236)
-   [NOIP2015 Kế hoạch vận chuyển](http://uoj.ac/problem/150)
-   [NOIP2016 Chạy bộ mỗi ngày](http://uoj.ac/problem/261)
