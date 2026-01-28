author: Ir1d, sshwy, GavinZhengOI, Planet6174, ouuan, Marcythm, ylxmf2005, 0xis-cn

Đọc thêm: [Thành phần hai phía liên thông](./bcc.md)

Định nghĩa chính xác về điểm cắt và cầu xem tại [Các khái niệm liên quan đến lý thuyết đồ thị](./concept.md).

## Điểm cắt

> Trên một đồ thị vô hướng, nếu khi xóa một đỉnh mà số lượng thành phần liên thông cực đại của đồ thị tăng lên, thì đỉnh đó gọi là điểm cắt (hay còn gọi là điểm khớp).

### Quy trình

Nếu thử xóa từng đỉnh rồi kiểm tra liên thông, độ phức tạp sẽ rất cao. Vì vậy, ta dùng một thuật toán phổ biến: Tarjan.

Trước hết, xét một đồ thị:

![](./images/cut1.svg)

Dễ thấy điểm cắt là đỉnh 2, và đồ thị này chỉ có một điểm cắt.

Đánh số thứ tự truy cập (DFS order) cho các đỉnh:

![](./images/cut2.svg)

Thông tin này lưu trong mảng `dfn`.

Cần thêm mảng `low`, dùng để lưu thời điểm truy cập nhỏ nhất mà đỉnh đó có thể đến được mà không qua cha.

Ví dụ: `low[2] = 1`, `low[5]` và `low[6] = 3`.

Khi DFS, điều kiện để một đỉnh $u$ là điểm cắt: tồn tại ít nhất một đỉnh con $v$ của $u$ sao cho $low_v \geq dfn_u$, tức là không thể quay lại tổ tiên, khi đó $u$ là điểm cắt.

Trường hợp đặc biệt với đỉnh gốc: nếu không phải điểm cắt thì mọi đường đi đều có thể đến mọi đỉnh, tức là trong cây DFS chỉ có một con. Nếu có từ hai con trở lên thì chắc chắn là điểm cắt (ví dụ, nếu bắt đầu DFS từ 2, cây sẽ có hai con: 3 hoặc 4 và 5 hoặc 6). Nếu chỉ có một con thì xóa đi không ảnh hưởng gì. Ví dụ dưới đây tạo thành một chu trình.

![](./images/cut3.svg)

Khi duyệt con của 1, giả sử DFS đến 2 trước, đánh dấu đã truy cập, rồi đệ quy xuống 4, 4 lại đến 3, khi quay lại sẽ thấy 3 đã truy cập, nên không phải điểm cắt.

Cập nhật mảng `low` theo giả mã sau:

$$
\begin{array}{ll}
1 & \textbf{if } v \text{ là con của } u \\
2 & \qquad \text{low}_u = \min(\text{low}_u, \text{low}_v) \\
3 & \textbf{else} \\
4 & \qquad \text{low}_u = \min(\text{low}_u, \text{dfn}_v) \\
\end{array}
$$

### Bài mẫu

[Luogu P3388【Mẫu】Điểm cắt (điểm khớp)](https://www.luogu.com.cn/problem/P3388)

??? note "Mã nguồn mẫu"
    ```cpp
    --8<-- "docs/graph/code/cut/cut_1.cpp"
    ```

## Cầu (trường hợp không có cạnh lặp)

Tương tự điểm cắt, cầu còn gọi là cạnh cắt.

> Trên một đồ thị vô hướng, nếu khi xóa một cạnh mà số lượng thành phần liên thông tăng lên, thì cạnh đó gọi là cầu (cạnh cắt). Chính xác: với đồ thị liên thông $G=\{V,E\}$, $e$ là một cạnh ($e \in E$), nếu $G-e$ không liên thông thì $e$ là cầu của $G$.

Ví dụ:

![Ví dụ cầu](./images/bridge1.svg)

Các cạnh đỏ là cầu.

### Quy trình

Tương tự điểm cắt, chỉ cần thay điều kiện: $low_v > dfn_u$, không cần xét trường hợp gốc.

Cầu không liên quan đến gốc, khi tìm điểm cắt thì $v$ không thể quay lại tổ tiên (kể cả cha), nên $u$ là điểm cắt. Nếu $low_v = dfn_u$ thì vẫn có thể quay lại cha. Nếu $v$ không thể quay lại tổ tiên và không có đường nào về cha, thì cạnh $u-v$ là cầu.

### Cài đặt

Đoạn mã dưới đây tìm cầu trên đồ thị vô hướng **không có cạnh lặp**, khi `isbridge[x]` đúng thì cạnh `(father[x],x)` là cầu.

=== "C++"
    ```cpp
    int low[MAXN], dfn[MAXN], idx;
    bool isbridge[MAXN];
    vector<int> G[MAXN];
    int cnt_bridge;
    int father[MAXN];
    
    void tarjan(int u, int fa) {
      bool flag = false;
      father[u] = fa;
      low[u] = dfn[u] = ++idx;
      for (const auto &v : G[u]) {
        if (!dfn[v]) {
          tarjan(v, u);
          low[u] = min(low[u], low[v]);
          if (low[v] > dfn[u]) {
            isbridge[v] = true;
            ++cnt_bridge;
          }
        } else {
          if (v != fa || flag)
            low[u] = min(low[u], dfn[v]);
          else
            flag = true;
        }
      }
    }
    ```

=== "Python"
    ```python
    low = [0] * MAXN
    dfn = [0] * MAXN
    idx = 0
    isbridge = [False] * MAXN
    G = [[0 for i in range(MAXN)] for j in range(MAXN)]
    cnt_bridge = 0
    father = [0] * MAXN
    
    
    def tarjan(u, fa):
        father[u] = fa
        idx = idx + 1
        low[u] = dfn[u] = idx
        for i in range(0, len(G[u])):
            v = G[u][i]
            if dfn[v] == False:
                tarjan(v, u)
                low[u] = min(low[u], low[v])
                if low[v] > dfn[u]:
                    isbridge[v] = True
                    cnt_bridge = cnt_bridge + 1
            elif v != fa:
                low[u] = min(low[u], dfn[v])
    ```

## Cầu (trường hợp có cạnh lặp)

Cách trên không áp dụng cho đồ thị có cạnh lặp.

Vì giữa hai đỉnh có thể có nhiều cạnh, khi đó không cạnh nào là cầu.

### Quy trình

Một cách là thay tham số `fa` bằng chỉ số cạnh vừa đi qua (mỗi cạnh có chỉ số riêng), tức là "không dùng cạnh vừa đi qua để cập nhật".

Cách khác là đặt một biến đánh dấu đã từng đi qua cha, lần sau gặp lại cha thì cập nhật bình thường.

Đoạn mã dưới đây tìm cầu trên đồ thị vô hướng **có thể có cạnh lặp**.

=== "C++"
    ```cpp
    int low[MAXN], dfn[MAXN], idx;
    bool isbridge[MAXN];
    vector<int> G[MAXN];
    int cnt_bridge;
    int father[MAXN];
    
    void tarjan(int u, int fa) {
      bool flag = false;
      father[u] = fa;
      low[u] = dfn[u] = ++idx;
      for (const auto &v : G[u]) {
        if (!dfn[v]) {
          tarjan(v, u);
          low[u] = min(low[u], low[v]);
          if (low[v] > dfn[u]) {
            isbridge[v] = true;
            ++cnt_bridge;
          }
        } else {
          if (v != fa || flag)
            low[u] = min(low[u], dfn[v]);
          else
            flag = true;
        }
      }
    }
    ```

## Bài tập

-   [P3388【Mẫu】Điểm cắt (điểm khớp)](https://www.luogu.com.cn/problem/P3388)
-   [POJ2117 Electricity](http://poj.org/problem?id=2117)
-   [HDU4738 Caocao's Bridges](https://acm.hdu.edu.cn/showproblem.php?pid=4738)
-   [HDU2460 Network](https://acm.hdu.edu.cn/showproblem.php?pid=2460)
-   [POJ1523 SPF](http://poj.org/problem?id=1523)

Thuật toán Tarjan còn có nhiều ứng dụng khác, như tìm thành phần liên thông mạnh, thu gọn đồ thị, giải 2-SAT, v.v.
