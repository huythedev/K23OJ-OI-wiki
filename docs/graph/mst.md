author: Chrogeek, Enter-tainer, HeRaNO, Ir1d, Marcythm, ShadowsEpic, StudyingFather, Xeonacid, bear-good, billchenchina, diauweb, diauweb, greyqz, kawa-yoiko, ouuan, partychicken, sshwy, stevebraveman, zhouyuyang2002, renbaoshuo, Hszzzx, y-kx-b, toprise

## Định nghĩa

Trước khi đọc phần này, bạn nên đọc [Các khái niệm cơ bản về đồ thị](./concept.md) và [Cây cơ bản](./tree-basic.md), đồng thời nắm được các định nghĩa sau:

1.  Đồ thị con sinh (subgraph)
2.  Cây khung (spanning tree)

Ta định nghĩa **Cây khung nhỏ nhất** (Minimum Spanning Tree, MST) của một đồ thị vô hướng liên thông là cây khung có tổng trọng số các cạnh nhỏ nhất.

Lưu ý: Chỉ đồ thị liên thông mới có cây khung, còn với đồ thị không liên thông thì chỉ tồn tại rừng khung (spanning forest).

## Thuật toán Kruskal

Thuật toán Kruskal là một trong những thuật toán tìm cây khung nhỏ nhất phổ biến và dễ cài đặt, do Kruskal phát minh. Ý tưởng cơ bản là thêm các cạnh theo thứ tự tăng dần trọng số, đây là một thuật toán tham lam (greedy).

### Kiến thức nền tảng

[DSU - Hợp nhất tập hợp (Union-Find)](../ds/dsu.md), [Tham lam (Greedy)](../basic/greedy.md), [Lưu trữ đồ thị](./save.md).

### Cài đặt

Minh họa:

![](./images/mst-2.apng)

Giả mã:

<!--
```pseudo
\begin{algorithm}
\caption{Kruskal}
\begin{algorithmic}
\INPUT{ The edges of the graph $e$ where each element in $e$ is $(u, v, w)$ denoting that there is an edge between $u$ and $v$ weighted $w$. }
\OUTPUT The edges of the MST of the input graph
\STATE $result \gets \varnothing$
\STATE sort $e$ into nondecreasing order by weight $w$
\FOR{each $(u, v, w)$ in the sorted $e$}
    \IF{$u$ \AND $v$ are not connected in the union-find set}
        \STATE connect $u$ \AND $v$ in the union-find set
        \STATE $result \gets result \bigcup (u, v, w)$
    \ENDIF
\ENDFOR
\RETURN $result$
\end{algorithmic}
\end{algorithm}
```
-->

$$
\begin{array}{ll}
1 &  \textbf{Input. } \text{Các cạnh của đồ thị } e , \text{ mỗi phần tử } (u, v, w) \\
  &  \text{ nghĩa là có cạnh nối } u \text{ và } v \text{ với trọng số } w . \\
2 &  \textbf{Output. } \text{Các cạnh của cây khung nhỏ nhất của đồ thị đầu vào}.\\
3 &  \textbf{Phương pháp. } \\ 
4 &  result \gets \varnothing \\
5 &  \text{Sắp xếp } e \text{ theo thứ tự không giảm của trọng số } w \\ 
6 &  \textbf{for} \text{ mỗi } (u, v, w) \text{ trong } e \text{ đã sắp xếp} \\ 
7 &  \qquad \textbf{if } u \text{ và } v \text{ chưa cùng tập hợp trong DSU } \\
8 &  \qquad\qquad \text{Hợp nhất } u \text{ và } v \text{ trong DSU} \\
9 &  \qquad\qquad  result \gets result\;\bigcup\ \{(u, v, w)\} \\
10 &  \textbf{return }  result
\end{array}
$$

Thuật toán đơn giản nhưng cần cấu trúc dữ liệu phù hợp... Cụ thể, cần duy trì một rừng, kiểm tra hai đỉnh có cùng cây không, và hợp nhất hai cây.

Trừu tượng hơn, cần duy trì nhiều **tập hợp**, kiểm tra hai phần tử có cùng tập hợp không, và hợp nhất hai tập hợp.

Việc kiểm tra hai đỉnh liên thông và hợp nhất hai đỉnh có thể dùng DSU (Disjoint Set Union).

Nếu dùng thuật toán sắp xếp $O(m\log m)$ và DSU $O(m\alpha(m, n))$ hoặc $O(m\log n)$, ta đạt độ phức tạp $O(m\log m)$ cho Kruskal.

### Chứng minh

Ý tưởng đơn giản: Để xây dựng cây khung nhỏ nhất, ta bắt đầu từ cạnh nhỏ nhất, thêm dần các cạnh theo thứ tự tăng dần trọng số, nếu thêm cạnh tạo thành chu trình thì bỏ qua, cho đến khi có đủ $n-1$ cạnh.

Chứng minh: Dùng quy nạp, chứng minh tại mọi thời điểm, tập cạnh mà Kruskal chọn đều nằm trong một cây khung nhỏ nhất nào đó.

Cơ sở: Ban đầu, rõ ràng đúng (cây khung nhỏ nhất tồn tại).

Bước quy nạp: Giả sử đúng tại thời điểm hiện tại, tập cạnh là $F$, $T$ là một cây khung nhỏ nhất chứa $F$, xét cạnh tiếp theo $e$.

Nếu $e$ thuộc $T$, hiển nhiên đúng.

Nếu không, $T+e$ tạo thành một chu trình, xét cạnh $f$ trên chu trình này không thuộc $F$ (tồn tại ít nhất một cạnh như vậy).

Trọng số $f$ không nhỏ hơn $e$, nếu nhỏ hơn thì $f$ đã được chọn trước $e$.

Trọng số $f$ cũng không lớn hơn $e$, nếu lớn hơn thì $T+e-f$ là cây khung tốt hơn $T$ (mâu thuẫn).

Vậy $T+e-f$ vẫn là cây khung nhỏ nhất chứa $F$, quy nạp thành công.

### Bài mẫu

???+ note "[洛谷 P1195 Bầu trời trong túi](https://www.luogu.com.cn/problem/P1195)"
    Có $n$ đám mây, bạn cần nối chúng thành $k$ viên kẹo bông, nối đám mây $X_i$ và $Y_i$ tốn $L_i$, hỏi tổng chi phí nhỏ nhất.

??? note "Code mẫu"
    === "C++"
        ```cpp
        --8<-- "docs/graph/code/mst/mst_3.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/graph/code/mst/mst_3.py"
        ```
    
    === "Java"
        ```java
        --8<-- "docs/graph/code/mst/mst_3.java"
        ```

## Thuật toán Prim

Thuật toán Prim là một thuật toán phổ biến khác để tìm cây khung nhỏ nhất. Ý tưởng là bắt đầu từ một đỉnh, liên tục thêm đỉnh mới (khác với Kruskal là thêm cạnh).

### Cài đặt

Minh họa:

![](./images/mst-3.apng)

Cụ thể, mỗi lần chọn đỉnh có khoảng cách nhỏ nhất tới tập đã chọn, và dùng các cạnh mới cập nhật khoảng cách các đỉnh còn lại.

Thực chất giống thuật toán Dijkstra: mỗi lần chọn đỉnh có khoảng cách nhỏ nhất, có thể tìm bằng vét cạn hoặc dùng heap.

Nếu dùng heap nhị phân (không hỗ trợ decrease-key $O(1)$), độ phức tạp không tốt hơn Kruskal, và hằng số cũng lớn hơn. Thông thường, Kruskal được ưu tiên hơn, nhưng với đồ thị dày đặc (đặc biệt là đồ thị đầy đủ), Prim vét cạn có thể nhanh hơn Kruskal, nhưng **không chắc** chạy nhanh hơn thực tế.

Vét cạn: $O(n^2+m)$.

Heap nhị phân: $O((n+m) \log n)$.

Heap Fibonacci: $O(n \log n + m)$.

Giả mã:

$$
\begin{array}{ll}
1 &  \textbf{Input. } \text{Các đỉnh của đồ thị }V\text{ ; hàm }g(u, v)\text{ là trọng số cạnh }(u, v)\text{;}\\
  &  \text{hàm }adj(v)\text{ là tập đỉnh kề với }v.\\
2 &  \textbf{Output. } \text{Tổng trọng số cây khung nhỏ nhất của đồ thị.} \\
3 &  \textbf{Phương pháp.} \\
4 &  result \gets 0 \\
5 & \text{Chọn một đỉnh bất kỳ làm }root \\
6 &  dis(root)\gets 0 \\
7 &  \textbf{for } \text{mỗi đỉnh }v\in(V-\{root\}) \\
8 &  \qquad  dis(v)\gets\infty \\
9 &  rest\gets V \\
10 &  \textbf{while }  rest\ne\varnothing \\
11 &  \qquad cur\gets \text{đỉnh có }dis\text{ nhỏ nhất trong }rest \\
12 &  \qquad  result\gets result+dis(cur) \\
13 &  \qquad  rest\gets rest-\{cur\} \\
14 &  \qquad  \textbf{for}\text{ mỗi đỉnh }v\in adj(cur) \\
15 &  \qquad\qquad  dis(v)\gets\min(dis(v), g(cur, v)) \\
16 &  \textbf{return }  result 
\end{array}
$$

Lưu ý: Đoạn code trên chỉ tính tổng trọng số cây khung nhỏ nhất, nếu muốn in ra phương án cần lưu lại cạnh ứng với mỗi $dis$.

??? note "Code mẫu"
    ```cpp
    // Prim tối ưu bằng heap nhị phân.
    #include <cstring>
    #include <iostream>
    #include <queue>
    using namespace std;
    constexpr int N = 5050, M = 2e5 + 10;
    
    struct E {
      int v, w, x;
    } e[M * 2];
    
    int n, m, h[N], cnte;
    
    void adde(int u, int v, int w) { e[++cnte] = E{v, w, h[u]}, h[u] = cnte; }
    
    struct S {
      int u, d;
    };
    
    bool operator<(const S &x, const S &y) { return x.d > y.d; }
    
    priority_queue<S> q;
    int dis[N];
    bool vis[N];
    
    int res = 0, cnt = 0;
    
    void Prim() {
      memset(dis, 0x3f, sizeof(dis));
      dis[1] = 0;
      q.push({1, 0});
      while (!q.empty()) {
        if (cnt >= n) break;
        int u = q.top().u, d = q.top().d;
        q.pop();
        if (vis[u]) continue;
        vis[u] = true;
        ++cnt;
        res += d;
        for (int i = h[u]; i; i = e[i].x) {
          int v = e[i].v, w = e[i].w;
          if (w < dis[v]) {
            dis[v] = w, q.push({v, w});
          }
        }
      }
    }
    
    int main() {
      cin >> n >> m;
      for (int i = 1, u, v, w; i <= m; ++i) {
        cin >> u >> v >> w, adde(u, v, w), adde(v, u, w);
      }
      Prim();
      if (cnt == n)
        cout << res;
      else
        cout << "No MST.";
      return 0;
    }
    ```

### Chứng minh

Bắt đầu từ một đỉnh bất kỳ, chia các đỉnh thành hai loại: đã chọn và chưa chọn.

Mỗi lần chọn đỉnh chưa chọn có cạnh nối với tập đã chọn có trọng số nhỏ nhất.

Sau đó thêm đỉnh này vào, nối bằng cạnh nhỏ nhất.

Lặp lại $n-1$ lần.

Chứng minh: Tại mỗi bước, luôn tồn tại một cây khung nhỏ nhất chứa tập cạnh đã chọn.

Cơ sở: Khi chỉ có một đỉnh, hiển nhiên đúng.

Bước quy nạp: Nếu đúng ở bước hiện tại, tập cạnh là $F$, thuộc cây khung nhỏ nhất $T$, xét cạnh $e$ sắp thêm.

Nếu $e$ thuộc $T$, hiển nhiên đúng.

Nếu không, xét chu trình $T+e$ và cạnh $f$ khác có thể thay thế.

Trọng số $f$ không nhỏ hơn $e$, nếu nhỏ hơn thì đã chọn $f$ trước.

Trọng số $f$ cũng không lớn hơn $e$, nếu lớn hơn thì $T+e-f$ là cây khung tốt hơn $T$.

Vậy $e$ và $f$ bằng nhau, $T+e-f$ vẫn là cây khung nhỏ nhất chứa $F$.

## Thuật toán Boruvka

Tiếp theo là một thuật toán khác để tìm cây khung nhỏ nhất: Boruvka. Ý tưởng là kết hợp hai thuật toán trên. Boruvka có thể dùng để tìm rừng khung nhỏ nhất cho đồ thị vô hướng (đồ thị liên thông thì là cây khung nhỏ nhất).

Với các bài toán có tính chất đặc biệt về cạnh, Boruvka có ưu thế, ví dụ bài [CF888G](https://codeforces.com/problemset/problem/888/G) với đồ thị đầy đủ.

Một số định nghĩa:

1.  Gọi $E'$ là tập cạnh của rừng khung nhỏ nhất hiện tại. Trong quá trình thuật toán, ta dần thêm cạnh vào $E'$. **Khối liên thông** là tập đỉnh $V'\subseteq V$ sao cho mọi cặp $u,v$ trong $V'$ liên thông qua các cạnh trong $E'$.
2.  **Cạnh nhỏ nhất** của một khối liên thông là cạnh nối khối đó với khối khác có trọng số nhỏ nhất.

Ban đầu, $E'=\varnothing$, mỗi đỉnh là một khối liên thông riêng:

1.  Xác định mỗi đỉnh thuộc khối liên thông nào. Đặt mỗi khối là "chưa có cạnh nhỏ nhất".
2.  Duyệt từng cạnh $(u, v)$, nếu $u$ và $v$ khác khối, dùng cạnh này cập nhật cạnh nhỏ nhất của hai khối.
3.  Nếu mọi khối đều "chưa có cạnh nhỏ nhất", dừng thuật toán, $E'$ là rừng khung nhỏ nhất. Ngược lại, thêm các cạnh nhỏ nhất của từng khối vào $E'$, quay lại bước 1.

Minh họa động (nguồn: [Wikipedia](https://en.wikipedia.org/wiki/Bor%C5%AFvka%27s_algorithm)):

![eg](./images/mst-1.apng)

Nếu đồ thị liên thông, mỗi vòng lặp số khối liên thông giảm ít nhất một nửa, nên thuật toán lặp tối đa $O(\log V)$ lần. Nếu không liên thông thì là nhiều bài toán con. Độ phức tạp $O(E\log V)$. Giả mã (chỉnh sửa từ [Wikipedia](https://en.wikipedia.org/wiki/Bor%C5%AFvka%27s_algorithm)):

$$
\begin{array}{ll}
1 &  \textbf{Input. } \text{Đồ thị }G\text{ với các cạnh có trọng số phân biệt. } \\
2 &  \textbf{Output. } \text{Rừng khung nhỏ nhất của }G .  \\
3 &  \textbf{Phương pháp. }  \\
4 & \text{Khởi tạo rừng }F\text{ gồm các cây một đỉnh} \\
5 &  \textbf{while } \text{True} \\
6 &  \qquad \text{Tìm các thành phần liên thông của }F\text{ và gán nhãn cho mỗi đỉnh của }G \\
7 &  \qquad \text{Khởi tạo cạnh rẻ nhất cho mỗi thành phần là "None"} \\
8 &  \qquad  \textbf{for } \text{mỗi cạnh }(u, v)\text{ của }G  \\
9 &  \qquad\qquad  \textbf{if }  u\text{ và }v\text{ khác thành phần} \\
10 &  \qquad\qquad\qquad  \textbf{if }  (u, v)\text{ rẻ hơn cạnh rẻ nhất của thành phần }u  \\
11 &  \qquad\qquad\qquad\qquad\text{ Gán }(u, v)\text{ là cạnh rẻ nhất của thành phần }u \\
12 &  \qquad\qquad\qquad  \textbf{if }  (u, v)\text{ rẻ hơn cạnh rẻ nhất của thành phần }v  \\
13 &  \qquad\qquad\qquad\qquad\text{ Gán }(u, v)\text{ là cạnh rẻ nhất của thành phần }v \\
14 &  \qquad  \textbf{if }\text{ mọi thành phần đều không có cạnh rẻ nhất} \\
15 &  \qquad\qquad  \textbf{return }  F \\
16 &  \qquad  \textbf{for }\text{ mỗi thành phần có cạnh rẻ nhất} \\
17 &  \qquad\qquad\text{ Thêm cạnh rẻ nhất vào }F \\
\end{array}
$$

Lưu ý: Khi so sánh cạnh, nên dùng thêm tiêu chí phụ (ví dụ chỉ số cạnh) để phân biệt khi trọng số bằng nhau.

## Bài tập

-   [「HAOI2006」Khỉ thông minh](https://www.luogu.com.cn/problem/P2504)
-   [「SCOI2005」Thành phố bận rộn](https://loj.ac/problem/2149)

## Tính duy nhất của cây khung nhỏ nhất

Xét tính duy nhất của cây khung nhỏ nhất. Nếu một cạnh **không thuộc cây khung nhỏ nhất**, nhưng có thể thay thế cho một cạnh **cùng trọng số, thuộc cây khung nhỏ nhất**, thì cây khung nhỏ nhất là không duy nhất.

Với Kruskal, chỉ cần đếm số cạnh cùng trọng số có thể chọn và số cạnh thực sự được chọn. Nếu hai số này khác nhau, tức là các cạnh này cùng tạo thành một chu trình (trong chu trình có ít nhất hai cạnh cùng trọng số), tức là cây khung nhỏ nhất không duy nhất.

Để tìm các cạnh cùng trọng số, chỉ cần lưu lại chỉ số đầu/cuối, dùng hàng đợi đơn điệu là có thể giải quyết trong $O(\alpha(m))$ (với $m$ là số cạnh), gần như không tăng độ phức tạp so với thuật toán gốc.

??? note "Bài mẫu: [POJ 1679](http://poj.org/problem?id=1679)"
    ```cpp
    --8<-- "docs/graph/code/mst/mst_1.cpp"
    ```

## Cây khung nhỏ thứ hai

### Cây khung nhỏ thứ hai không chặt (non-strict)

#### Định nghĩa

Trong đồ thị vô hướng, cây khung có tổng trọng số nhỏ nhất **lớn hơn hoặc bằng** cây khung nhỏ nhất.

#### Cách tìm

-   Tìm cây khung nhỏ nhất $T$, tổng trọng số $M$.
-   Duyệt từng cạnh chưa chọn $e = (u,v,w)$, tìm trên $T$ cạnh lớn nhất trên đường từ $u$ đến $v$ là $e' = (s,t,w')$, thay $e'$ bằng $e$ được cây khung mới $T'$ có tổng trọng số $M' = M + w - w'$.
-   Lấy giá trị nhỏ nhất trong các $M'$.

Làm sao tìm cạnh lớn nhất trên đường $u,v$? Dùng kỹ thuật "bậc thang" (binary lifting), tiền xử lý tổ tiên $2^i$ của mỗi đỉnh và trọng số lớn nhất trên đường đó, khi tìm LCA thì truy vấn luôn được.

### Cây khung nhỏ thứ hai chặt (strict)

#### Định nghĩa

Trong đồ thị vô hướng, cây khung có tổng trọng số nhỏ nhất **lớn hơn** cây khung nhỏ nhất.

#### Cách tìm

Ở cách trên, vì cây khung nhỏ nhất đảm bảo cạnh lớn nhất trên đường $u$ đến $v$ không lớn hơn các đường khác, nên nếu cạnh thay thế có trọng số bằng cạnh bị thay, kết quả là cây khung nhỏ thứ hai không chặt.

Cách giải quyết: Khi tiền xử lý tổ tiên $2^i$, ngoài trọng số lớn nhất còn lưu trọng số lớn thứ hai (nếu có). Khi thay thế, nếu trọng số cạnh thay bằng trọng số lớn nhất, thì dùng trọng số lớn thứ hai.

Có thể dùng binary lifting, độ phức tạp $O(m \log m)$.

??? note "Code mẫu"
    ```cpp
    #include <algorithm>
    #include <iostream>
    
    constexpr int INF = 0x3fffffff;
    constexpr long long INF64 = 0x3fffffffffffffffLL;
    
    struct Edge {
      int u, v, val;
    
      bool operator<(const Edge &other) const { return val < other.val; }
    };
    
    Edge e[300010];
    bool used[300010];
    
    int n, m;
    long long sum;
    
    class Tr {
     private:
      struct Edge {
        int to, nxt, val;
      } e[600010];
    
      int cnt, head[100010];
    
      int pnt[100010][22];
      int dpth[100010];
      // 到祖先的路径上边权最大的边
      int maxx[100010][22];
      // 到祖先的路径上边权次大的边，若不存在则为 -INF
      int minn[100010][22];
    
     public:
      void addedge(int u, int v, int val) {
        e[++cnt] = Edge{v, head[u], val};
        head[u] = cnt;
      }
    
      void insedge(int u, int v, int val) {
        addedge(u, v, val);
        addedge(v, u, val);
      }
    
      void dfs(int now, int fa) {
        dpth[now] = dpth[fa] + 1;
        pnt[now][0] = fa;
        minn[now][0] = -INF;
        for (int i = 1; (1 << i) <= dpth[now]; i++) {
          pnt[now][i] = pnt[pnt[now][i - 1]][i - 1];
          int kk[4] = {maxx[now][i - 1], maxx[pnt[now][i - 1]][i - 1],
                       minn[now][i - 1], minn[pnt[now][i - 1]][i - 1]};
          // 从四个值中取得最大值
          std::sort(kk, kk + 4);
          maxx[now][i] = kk[3];
          // 取得严格次大值
          int ptr = 2;
          while (ptr >= 0 && kk[ptr] == kk[3]) ptr--;
          minn[now][i] = (ptr == -1 ? -INF : kk[ptr]);
        }
    
        for (int i = head[now]; i; i = e[i].nxt) {
          if (e[i].to != fa) {
            maxx[e[i].to][0] = e[i].val;
            dfs(e[i].to, now);
          }
        }
      }
    
      int lca(int a, int b) {
        if (dpth[a] < dpth[b]) std::swap(a, b);
    
        for (int i = 21; i >= 0; i--)
          if (dpth[pnt[a][i]] >= dpth[b]) a = pnt[a][i];
    
        if (a == b) return a;
    
        for (int i = 21; i >= 0; i--) {
          if (pnt[a][i] != pnt[b][i]) {
            a = pnt[a][i];
            b = pnt[b][i];
          }
        }
        return pnt[a][0];
      }
    
      int query(int a, int b, int val) {
        int res = -INF;
        for (int i = 21; i >= 0; i--) {
          if (dpth[pnt[a][i]] >= dpth[b]) {
            if (val != maxx[a][i])
              res = std::max(res, maxx[a][i]);
            else
              res = std::max(res, minn[a][i]);
            a = pnt[a][i];
          }
        }
        return res;
      }
    } tr;
    
    int fa[100010];
    
    int find(int x) { return fa[x] == x ? x : fa[x] = find(fa[x]); }
    
    void Kruskal() {
      int tot = 0;
      std::sort(e + 1, e + m + 1);
      for (int i = 1; i <= n; i++) fa[i] = i;
    
      for (int i = 1; i <= m; i++) {
        int a = find(e[i].u);
        int b = find(e[i].v);
        if (a != b) {
          fa[a] = b;
          tot++;
          tr.insedge(e[i].u, e[i].v, e[i].val);
          sum += e[i].val;
          used[i] = true;
        }
        if (tot == n - 1) break;
      }
    }
    
    int main() {
      std::ios::sync_with_stdio(false);
      std::cin.tie(nullptr);
    
      std::cin >> n >> m;
      for (int i = 1; i <= m; i++) {
        int u, v, val;
        std::cin >> u >> v >> val;
        e[i] = Edge{u, v, val};
      }
    
      Kruskal();
      long long ans = INF64;
      tr.dfs(1, 0);
    
      for (int i = 1; i <= m; i++) {
        if (!used[i]) {
          int _lca = tr.lca(e[i].u, e[i].v);
          // 找到路径上不等于 e[i].val 的最大边权
          long long tmpa = tr.query(e[i].u, _lca, e[i].val);
          long long tmpb = tr.query(e[i].v, _lca, e[i].val);
          // 这样的边可能不存在，只在这样的边存在时更新答案
          if (std::max(tmpa, tmpb) > -INF)
            ans = std::min(ans, sum - std::max(tmpa, tmpb) + e[i].val);
        }
      }
      // 次小生成树不存在时输出 -1
      std::cout << (ans == INF64 ? -1 : ans) << '\n';
      return 0;
    }
    ```

## Cây khung nút thắt (Bottleneck Spanning Tree)

### Định nghĩa

Cây khung nút thắt của đồ thị vô hướng $G$ là cây khung mà trọng số cạnh lớn nhất là nhỏ nhất trong tất cả các cây khung của $G$.

### Tính chất

**Cây khung nhỏ nhất luôn là cây khung nút thắt, nhưng ngược lại thì không chắc.** Có thể chứng minh bằng phản chứng: Nếu cây khung nhỏ nhất có cạnh lớn nhất là $w$, giả sử tồn tại cây khung nút thắt mà mọi cạnh đều nhỏ hơn $w$, thì thay cạnh lớn nhất của cây khung nhỏ nhất bằng một cạnh trong cây khung nút thắt sẽ được cây khung có tổng trọng số nhỏ hơn, mâu thuẫn.

### Bài mẫu

???+ note "POJ 2395 Out of Hay"
    Cho $n$ trang trại và $m$ cạnh, các trang trại đánh số $1$ đến $n$. Một người xuất phát từ trang trại $1$ đến các trang trại khác, hỏi trên đường đi anh ta cần mang nhiều nhất bao nhiêu nước (mỗi lần đến trang trại có thể tiếp nước, tổng đường đi phải nhỏ nhất). Bài này yêu cầu cạnh lớn nhất trên cây khung nhỏ nhất.

## Đường đi nút thắt nhỏ nhất (Minimum Bottleneck Path)

### Định nghĩa

Trong đồ thị vô hướng $G$, đường đi nút thắt nhỏ nhất từ $x$ đến $y$ là đường đi đơn giản mà cạnh lớn nhất trên đường đi là nhỏ nhất trong tất cả các đường đi từ $x$ đến $y$.

### Tính chất

Theo định nghĩa cây khung nhỏ nhất, cạnh lớn nhất trên đường đi từ $x$ đến $y$ trên cây khung nhỏ nhất chính là giá trị nhỏ nhất có thể. Dù cây khung nhỏ nhất không duy nhất, nhưng trên mọi cây khung nhỏ nhất, giá trị này đều như nhau.

Tuy nhiên, không phải mọi đường đi nút thắt nhỏ nhất đều là đường đi trên cây khung nhỏ nhất.

Ví dụ:

![](./images/mst5.png)

Đường đi nút thắt nhỏ nhất từ $1$ đến $4$ có hai đường: 1-2-3-4 và 1-3-4.

Nhưng cạnh 1-2 không xuất hiện trong bất kỳ cây khung nhỏ nhất nào.

### Ứng dụng

Vì đường đi nút thắt nhỏ nhất không duy nhất, thường chỉ hỏi giá trị cạnh lớn nhất trên đường đi đó.

Tức là, cần truy vấn max trên đường đi trong cây khung nhỏ nhất.

Có thể dùng binary lifting, HLD,... để giải quyết.

## Cây tái cấu trúc Kruskal

### Định nghĩa

Trong quá trình chạy Kruskal, ta thêm các cạnh theo thứ tự tăng dần trọng số.

Ban đầu tạo $n$ tập hợp, mỗi tập có một đỉnh, trọng số $0$.

Mỗi lần thêm cạnh, hợp nhất hai tập hợp, tạo một đỉnh mới có trọng số bằng trọng số cạnh vừa thêm, hai gốc của hai tập hợp làm con trái/phải của đỉnh mới. Sau đó hợp nhất hai tập và đỉnh mới thành một tập, đỉnh mới là gốc.

Sau $n-1$ lần, ta được một cây nhị phân có đúng $n$ lá, mỗi nút trong có hai con. Đó là cây tái cấu trúc Kruskal.

Ví dụ:

![](./images/mst5.png)

Cây tái cấu trúc Kruskal của đồ thị trên:

![](./images/mst6.png)

### Tính chất

Dễ thấy, giá trị nhỏ nhất của cạnh lớn nhất trên mọi đường đi đơn giản giữa hai đỉnh $x, y$ trong đồ thị gốc = max cạnh trên đường đi giữa $x, y$ trên cây khung nhỏ nhất = giá trị tại LCA của $x, y$ trên cây tái cấu trúc Kruskal.

Tức là, tập các đỉnh $y$ sao cho max cạnh trên đường từ $x$ đến $y$ không vượt quá $val$ chính là tập lá của một cây con trên cây tái cấu trúc Kruskal.

Để tìm tập này, chỉ cần tìm trên cây tái cấu trúc Kruskal đỉnh tổ tiên nông nhất trên đường từ $x$ lên gốc có trọng số $\leq val$.

Nếu muốn tìm min cạnh lớn nhất trên mọi đường đi đơn giản giữa hai đỉnh, thì chạy Kruskal theo thứ tự giảm dần trọng số.

??? note "[「LOJ 137」Đường đi nút thắt nhỏ nhất - bản mở rộng](https://loj.ac/problem/137)"
    ```cpp
    --8<-- "docs/graph/code/mst/mst_2.cpp"
    ```

??? note "[NOI 2018 Quy trình trở về](https://uoj.ac/problem/393)"
    Đầu tiên tiền xử lý đường đi ngắn nhất từ mỗi đỉnh đến gốc.

    Xây dựng cây khung lớn nhất theo độ cao. Rõ ràng, mỗi truy vấn tìm các đỉnh có thể đến được là các đỉnh trên đường đi từ đỉnh truy vấn đến gốc trên cây khung lớn nhất có trọng số cạnh nhỏ nhất $> p$.

    Theo tính chất cây tái cấu trúc Kruskal, các đỉnh này là tập lá của một cây con.

    Chỉ cần truy vấn min trọng số lá trong cây con đó.

    Có thể tìm gốc cây con bằng binary lifting trên cây tái cấu trúc Kruskal.

    Độ phức tạp $O((n+m+Q) \log n)$.
````
