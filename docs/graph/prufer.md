???+ note "Note"
    Bài viết này dịch từ [e-maxx Prüfer Code](https://github.com/e-maxx-eng/e-maxx-eng/blob/master/src/graph/pruefer_code.md). Ngoài ra, cần lưu ý: trong bản gốc các đỉnh đánh số từ $0$, còn trong bài này sẽ chuyển sang đánh số từ $1$ cho phù hợp với thói quen số đông.

Bài viết này giới thiệu về dãy Prüfer (Prüfer code), một phương pháp biểu diễn duy nhất một cây có gán nhãn bằng một dãy số nguyên.

Dãy Prüfer có thể dùng để chứng minh [Công thức Cayley](#cayley-公式-cayleys-formula) (Cayley's formula). Ngoài ra, bài viết cũng trình bày cách tính số phương án thêm cạnh để liên thông đồ thị.

**Lưu ý**: Không xét trường hợp cây chỉ có $1$ đỉnh.

## Dãy Prüfer

### Giới thiệu

Dãy Prüfer cho phép biểu diễn một cây có $n$ đỉnh gán nhãn bằng một dãy gồm $n-2$ số nguyên trong $[1,n]$. Có thể hiểu đây là một song ánh giữa tập các cây khung của đồ thị đầy đủ và các dãy số. Thường gặp trong các bài toán đếm tổ hợp.

Heinz Prüfer phát minh ra dãy này năm 1918 để chứng minh [Công thức Cayley](#cayley-公式-cayleys-formula).

### Cách xây dựng dãy Prüfer từ cây

Cách xây dựng dãy Prüfer như sau: mỗi lần chọn lá có số nhỏ nhất, xóa nó khỏi cây và ghi lại đỉnh kề với nó vào dãy. Lặp lại $n-2$ lần, cuối cùng còn lại hai đỉnh, thuật toán kết thúc.

Rõ ràng, nếu dùng heap thì độ phức tạp là $O(n\log n)$.

???+ note "Cài đặt"
    === "C++"
        ```cpp
        // 代码摘自原文，结点是从 0 标号的
        vector<vector<int>> adj;
        
        vector<int> pruefer_code() {
          int n = adj.size();
          set<int> leafs;
          vector<int> degree(n);
          vector<bool> killed(n);
          for (int i = 0; i < n; i++) {
            degree[i] = adj[i].size();
            if (degree[i] == 1) leafs.insert(i);
          }
        
          vector<int> code(n - 2);
          for (int i = 0; i < n - 2; i++) {
            int leaf = *leafs.begin();
            leafs.erase(leafs.begin());
            killed[leaf] = true;
            int v;
            for (int u : adj[leaf])
              if (!killed[u]) v = u;
            code[i] = v;
            if (--degree[v] == 1) leafs.insert(v);
          }
          return code;
        }
        ```
    
    === "Python"
        ```python
        # 结点是从 0 标号的
        adj = [[]]
        
        
        def pruefer_code():
            n = len(adj)
            leafs = set()
            degree = [0] * n
            killed = [False] * n
            for i in range(1, n):
                degree[i] = len(adj[i])
                if degree[i] == 1:
                    leafs.intersection(i)
            code = [0] * (n - 2)
            for i in range(1, n - 2):
                leaf = leafs[0]
                leafs.pop()
                killed[leaf] = True
                for u in adj[leaf]:
                    if killed[u] == False:
                        v = u
                code[i] = v
                if degree[v] == 1:
                    degree[v] = degree[v] - 1
                    leafs.intersection(v)
            return code
        ```

Ví dụ, dưới đây là quá trình xây dựng dãy Prüfer cho một cây $7$ đỉnh:

![Prüfer](./images/prufer1.png)

Dãy cuối cùng là $2,2,3,3,2$.

Ngoài ra còn có một thuật toán xây dựng tuyến tính.

### Thuật toán xây dựng dãy Prüfer tuyến tính

Bản chất của thuật toán tuyến tính là duy trì một con trỏ chỉ tới đỉnh sẽ bị xóa tiếp theo. Nhận thấy số lá giảm đơn điệu, mỗi lần xóa một lá thì tổng số lá hoặc giữ nguyên hoặc giảm $1$.

Ta thực hiện như sau: duy trì một con trỏ $p$, ban đầu trỏ tới lá nhỏ nhất. Đồng thời duy trì mảng bậc của các đỉnh để biết khi xóa một đỉnh có sinh ra lá mới không. Các bước:

1.  Xóa đỉnh mà $p$ trỏ tới, kiểm tra có sinh ra lá mới không.
2.  Nếu có lá mới, giả sử là $x$, so sánh $x$ với $p$. Nếu $x>p$ thì không làm gì; nếu $x<p$ thì xóa luôn $x$, kiểm tra tiếp, lặp lại bước $2$ cho đến khi không còn lá mới hoặc lá mới có số lớn hơn $p$.
3.  Tăng $p$ đến khi gặp lá chưa bị xóa.

#### Đúng đắn

Lặp lại $n-2$ lần là xong. Đúng đắn vì:

-   Nếu xóa $p$ không sinh lá mới, chỉ tìm lá tiếp theo.
-   Nếu sinh lá mới $x$:
    - Nếu $x>p$, sau này $p$ sẽ quét tới $x$.
    - Nếu $x<p$, vì $p$ là nhỏ nhất, $x$ còn nhỏ hơn nên phải xóa $x$ trước.

Mỗi cạnh chỉ bị truy cập tối đa một lần (khi giảm bậc), con trỏ cũng chỉ duyệt mỗi đỉnh một lần, nên độ phức tạp $O(n)$.

#### Cài đặt

=== "C++"
    ```cpp
    // 从原文摘的代码，同样以 0 为起点
    vector<vector<int>> adj;
    vector<int> parent;
    
    void dfs(int v) {
      for (int u : adj[v]) {
        if (u != parent[v]) parent[u] = v, dfs(u);
      }
    }
    
    vector<int> pruefer_code() {
      int n = adj.size();
      parent.resize(n), parent[n - 1] = -1;
      dfs(n - 1);
    
      int ptr = -1;
      vector<int> degree(n);
      for (int i = 0; i < n; i++) {
        degree[i] = adj[i].size();
        if (degree[i] == 1 && ptr == -1) ptr = i;
      }
    
      vector<int> code(n - 2);
      int leaf = ptr;
      for (int i = 0; i < n - 2; i++) {
        int next = parent[leaf];
        code[i] = next;
        if (--degree[next] == 1 && next < ptr) {
          leaf = next;
        } else {
          ptr++;
          while (degree[ptr] != 1) ptr++;
          leaf = ptr;
        }
      }
      return code;
    }
    ```

=== "Python"
    ```python
    # 同样以 0 为起点
    adj = [[]]
    parent = [0] * n
    
    
    def dfs(v):
        for u in adj[v]:
            if u != parent[v]:
                parent[u] = v
                dfs(u)
    
    
    def pruefer_code():
        n = len(adj)
        parent[n - 1] = -1
        dfs(n - 1)
    
        ptr = -1
        degree = [0] * n
        for i in range(0, n):
            degree[i] = len(adj[i])
            if degree[i] == 1 and ptr == -1:
                ptr = i
    
        code = [0] * (n - 2)
        leaf = ptr
        for i in range(0, n - 2):
            next = parent[leaf]
            code[i] = next
            if degree[next] == 1 and next < ptr:
                degree[next] = degree[next] - 1
                leaf = next
            else:
                ptr = ptr + 1
                while degree[ptr] != 1:
                    ptr = ptr + 1
                leaf = ptr
        return code
    ```

### Tính chất của dãy Prüfer

1.  Sau khi xây dựng dãy Prüfer, cây gốc còn lại hai đỉnh, một trong số đó chắc chắn là đỉnh lớn nhất $n$.
2.  Số lần xuất hiện của mỗi đỉnh trong dãy bằng bậc của nó trừ $1$ (đỉnh không xuất hiện là lá).

### Dựng lại cây từ dãy Prüfer

Cách dựng lại cây cũng tương tự. Dựa vào tính chất của dãy Prüfer, ta biết được bậc của mỗi đỉnh. Từ đó, luôn tìm được lá nhỏ nhất, và lá này sẽ nối với phần tử đầu tiên của dãy. Sau đó giảm bậc hai đỉnh đó đi $1$.

Cứ lặp lại: mỗi lần chọn đỉnh bậc $1$ nhỏ nhất, nối với phần tử tiếp theo của dãy, giảm bậc hai đỉnh. Cuối cùng còn lại hai đỉnh bậc $1$, một trong số đó là $n$, nối chúng lại. Dùng heap để duy trì các lá, mỗi khi bậc giảm về $1$ thì thêm vào heap, độ phức tạp $O(n\log n)$.

???+ note "Cài đặt"
    ```cpp
    // 原文摘代码
    vector<pair<int, int>> pruefer_decode(vector<int> const& code) {
      int n = code.size() + 2;
      vector<int> degree(n, 1);
      for (int i : code) degree[i]++;
    
      set<int> leaves;
      for (int i = 0; i < n; i++)
        if (degree[i] == 1) leaves.insert(i);
    
      vector<pair<int, int>> edges;
      for (int v : code) {
        int leaf = *leaves.begin();
        leaves.erase(leaves.begin());
    
        edges.emplace_back(leaf, v);
        if (--degree[v] == 1) leaves.insert(v);
      }
      edges.emplace_back(*leaves.begin(), n - 1);
      return edges;
    }
    ```

### Dựng lại cây tuyến tính

Tương tự như xây dựng dãy Prüfer tuyến tính. Khi giảm bậc sinh ra lá mới, nếu nhỏ hơn con trỏ thì ưu tiên xử lý.

#### Cài đặt

```cpp
// 原文摘代码
vector<pair<int, int>> pruefer_decode(vector<int> const& code) {
  int n = code.size() + 2;
  vector<int> degree(n, 1);
  for (int i : code) degree[i]++;

  int ptr = 0;
  while (degree[ptr] != 1) ptr++;
  int leaf = ptr;

  vector<pair<int, int>> edges;
  for (int v : code) {
    edges.emplace_back(leaf, v);
    if (--degree[v] == 1 && v < ptr) {
      leaf = v;
    } else {
      ptr++;
      while (degree[ptr] != 1) ptr++;
      leaf = ptr;
    }
  }
  edges.emplace_back(leaf, n - 1);
  return edges;
}
```

Qua các bước trên, có thể thấy dãy Prüfer và cây vô hướng có gán nhãn là song ánh.

## Công thức Cayley (Cayley's formula)

Đồ thị đầy đủ $K_n$ có $n^{n-2}$ cây khung.

Chứng minh? Có nhiều cách, nhưng dùng dãy Prüfer là đơn giản nhất. Mỗi dãy số độ dài $n-2$ với giá trị trong $[1,n]$ ứng với đúng một cây, nên số cây là $n^{n-2}$.

## Số phương án liên thông đồ thị

Dãy Prüfer còn mạnh hơn bạn nghĩ, nó cho phép xây dựng công thức tổng quát hơn cả [Công thức Cayley](#cayley-公式-cayleys-formula). Ví dụ:

> Cho đồ thị vô hướng có gán nhãn $n$ đỉnh $m$ cạnh, có $k$ thành phần liên thông. Hỏi có bao nhiêu cách thêm $k-1$ cạnh để làm cho đồ thị liên thông?

### Chứng minh

Gọi $s_i$ là số đỉnh của thành phần liên thông thứ $i$. Xét xây dựng dãy Prüfer cho $k$ thành phần liên thông. Vì có nhiều cách nối giữa các thành phần, đây không phải dãy Prüfer thông thường. Gọi $d_i$ là bậc của thành phần $i$. Tổng bậc là $2k-2$, nên $\sum_{i=1}^k d_i=2k-2$. Với dãy $d$ cho trước, số cách xây dựng dãy Prüfer là

$$
\binom{k-2}{d_1-1,d_2-1,\cdots,d_k-1}=\frac{(k-2)!}{(d_1-1)!(d_2-1)!\cdots(d_k-1)!}
$$

Với thành phần $i$, có ${s_i}^{d_i}$ cách nối. Vậy với dãy $d$ cho trước, số phương án là

$$
\binom{k-2}{d_1-1,d_2-1,\cdots,d_k-1}\cdot \prod_{i=1}^k{s_i}^{d_i}
$$

Tổng trên mọi dãy $d$:

$$
\sum_{d_i\ge 1，\sum_{i=1}^kd_i=2k-2}\binom{k-2}{d_1-1,d_2-1,\cdots,d_k-1}\cdot \prod_{i=1}^k{s_i}^{d_i}
$$

Nhìn phức tạp, nhưng dùng đa thức Newton:

$$
(x_1 + \dots + x_m)^p = \sum_{\substack{c_i \ge 0 ,\  \sum_{i=1}^m c_i = p}} \binom{p}{c_1, c_2, \cdots ,c_m}\cdot \prod_{i=1}^m{x_i}^{c_i}
$$

Đổi biến $e_i=d_i-1$, $\sum_{i=1}^k e_i=k-2$, công thức thành

$$
\sum_{e_i\ge 0，\sum_{i=1}^ke_i=k-2}\binom{k-2}{e_1,e_2,\cdots,e_k}\cdot \prod_{i=1}^k{s_i}^{e_i+1}
$$

Rút gọn:

$$
(s_1+s_2+\cdots+s_k)^{k-2}\cdot \prod_{i=1}^ks_i
$$

Tức là

$$
n^{k-2}\cdot\prod_{i=1}^ks_i
$$

là đáp số.

## Bài tập

-   [Luogu P6086【Mẫu】Dãy Prüfer](https://www.luogu.com.cn/problem/P6086) (bài mẫu)
-   [Luogu P11039【MX-X3-T6】「RiOI-4」TECHNOPOLIS 2085](https://www.luogu.com.cn/problem/P11039)
-   [UVa #10843 - Anne's game](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=20&page=show_problem&problem=1784)
-   [Timus #1069 - Prufer Code](http://acm.timus.ru/problem.aspx?space=1&num=1069)
-   [Codeforces - Clues](http://codeforces.com/contest/156/problem/D)
-   [Topcoder - TheCitiesAndRoadsDivTwo](https://archive.topcoder.com/ProblemStatement/pm/10774)
