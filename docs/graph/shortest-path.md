author: du33169, lingkerio, Taoran-01

## Định nghĩa

(Bạn còn nhớ các định nghĩa này chứ? Trước khi đọc phần dưới, hãy chắc chắn bạn đã hiểu [các khái niệm cơ bản về đồ thị](./concept.md).)

-   Đường đi
-   Đường đi ngắn nhất (shortest path)
-   Đường đi ngắn nhất trên đồ thị có hướng, đồ thị vô hướng
-   Đường đi ngắn nhất từ một nguồn (single source), đường đi ngắn nhất giữa mọi cặp đỉnh

## Ký hiệu

Để tiện trình bày, dưới đây là các ký hiệu sẽ sử dụng:

-   $n$ là số đỉnh, $m$ là số cạnh của đồ thị;
-   $s$ là đỉnh nguồn của bài toán đường đi ngắn nhất;
-   $D(u)$ là độ dài **thực tế** của đường đi ngắn nhất từ $s$ đến $u$;
-   $dis(u)$ là độ dài **ước lượng** của đường đi ngắn nhất từ $s$ đến $u$. Luôn có $dis(u) \geq D(u)$. Đặc biệt, khi thuật toán kết thúc, $dis(u) = D(u)$.
-   $w(u,v)$ là trọng số của cạnh $(u,v)$.

## Tính chất

Với đồ thị có trọng số dương, đường đi ngắn nhất giữa hai đỉnh bất kỳ sẽ không đi qua đỉnh lặp lại.

Với đồ thị có trọng số dương, đường đi ngắn nhất giữa hai đỉnh bất kỳ sẽ không đi qua cạnh lặp lại.

Với đồ thị có trọng số dương, số đỉnh trên đường đi ngắn nhất giữa hai đỉnh bất kỳ không vượt quá $n$, số cạnh không vượt quá $n-1$.

## Thuật toán Floyd

Dùng để tìm đường đi ngắn nhất giữa mọi cặp đỉnh.

Độ phức tạp cao, nhưng hằng số nhỏ, dễ cài đặt (chỉ ba vòng `for`).

Áp dụng cho mọi loại đồ thị, bất kể có hướng/vô hướng, trọng số dương/âm, miễn là không có chu trình âm (đường đi ngắn nhất phải tồn tại).

### Cài đặt

Ta định nghĩa mảng `f[k][x][y]` là độ dài đường đi ngắn nhất từ $x$ đến $y$, chỉ cho phép đi qua các đỉnh $1$ đến $k$ (tức là trong đồ thị con $V' = \{1, 2, \ldots, k\}$, lưu ý $x, y$ không nhất thiết thuộc $V'$).

Rõ ràng, `f[n][x][y]` chính là độ dài đường đi ngắn nhất từ $x$ đến $y$ trên toàn đồ thị.

Xét cách tính mảng `f`:

-   `f[0][x][y]`: là trọng số cạnh giữa $x$ và $y$, hoặc $0$, hoặc $+\infty$ (khi nào là $+\infty$? Nếu $x$ và $y$ có cạnh nối trực tiếp thì là trọng số cạnh; nếu $x = y$ thì là $0$; nếu không có cạnh nối thì là $+\infty$).
-   `f[k][x][y] = min(f[k-1][x][y], f[k-1][x][k]+f[k-1][k][y])` (không đi qua $k$ hoặc đi qua $k$).

Như vậy, không gian là $O(N^3)$, ta tăng dần $k$ từ $1$ đến $n$, mỗi lần cập nhật đường đi ngắn nhất giữa mọi cặp đỉnh.

=== "C++"
    ```cpp
    for (k = 1; k <= n; k++) {
      for (x = 1; x <= n; x++) {
        for (y = 1; y <= n; y++) {
          f[k][x][y] = min(f[k - 1][x][y], f[k - 1][x][k] + f[k - 1][k][y]);
        }
      }
    }
    ```

=== "Python"
    ```python
    for k in range(1, n + 1):
        for x in range(1, n + 1):
            for y in range(1, n + 1):
                f[k][x][y] = min(f[k - 1][x][y], f[k - 1][x][k] + f[k - 1][k][y])
    ```

Vì chiều thứ nhất không ảnh hưởng kết quả, ta có thể bỏ đi, chuyển thành `f[x][y] = min(f[x][y], f[x][k]+f[k][y])`.

???+ note "Chứng minh có thể bỏ chiều thứ nhất"
    Với mỗi $k$, khi cập nhật `f[k][x][y]`, chỉ dùng các phần tử ở dòng $k$ và cột $k$ của `f[k-1]`. Khi cập nhật `f[k][k][y]` hoặc `f[k][x][k]`, giá trị không đổi vì $f[k-1][k][k]=0$. Do đó, khi bỏ chiều thứ nhất, mỗi phần tử cập nhật đều dùng giá trị chưa bị thay đổi trong vòng lặp hiện tại, nên kết quả không bị ảnh hưởng.

=== "C++"
    ```cpp
    for (k = 1; k <= n; k++) {
      for (x = 1; x <= n; x++) {
        for (y = 1; y <= n; y++) {
          f[x][y] = min(f[x][y], f[x][k] + f[k][y]);
        }
      }
    }
    ```

=== "Python"
    ```python
    for k in range(1, n + 1):
        for x in range(1, n + 1):
            for y in range(1, n + 1):
                f[x][y] = min(f[x][y], f[x][k] + f[k][y])
    ```

Tổng kết: thời gian $O(N^3)$, không gian $O(N^2)$.

### Ứng dụng

???+ question "Cho một đồ thị vô hướng trọng số dương, tìm chu trình đơn có tổng trọng số nhỏ nhất."
    Chu trình này chắc chắn là chu trình đơn giản.

    Hãy nghĩ xem chu trình này được tạo thành như thế nào.

    Xét đỉnh có số lớn nhất $u$ trên chu trình.

    `f[u-1][x][y]` cùng với $(u,x)$, $(u,y)$ tạo thành chu trình.

    Khi chạy Floyd, liệt kê $u$, tính tổng này và lấy giá trị nhỏ nhất.

    Độ phức tạp $O(n^3)$.

    Xem thêm ở [chu trình nhỏ nhất](./min-cycle.md).

???+ question "Biết với mỗi cặp đỉnh của đồ thị có hướng có cạnh nối hay không, hãy xác định mọi cặp đỉnh có liên thông không."
    Đây chính là bài toán **bao đóng bắc cầu** (transitive closure) của đồ thị.

    Chỉ cần chạy Floyd, mỗi cạnh coi là $1/0$, phép $\min$ thay bằng phép **hoặc**.

    Có thể tối ưu bằng bitset, độ phức tạp $O(\frac{n^3}{w})$.

    ```cpp
    // std::bitset<SIZE> f[SIZE];
    for (k = 1; k <= n; k++)
      for (i = 1; i <= n; i++)
        if (f[i][k]) f[i] = f[i] | f[k];
    ```

## Thuật toán Bellman–Ford

Bellman–Ford là thuật toán dựa trên phép nới lỏng (relax), có thể tìm đường đi ngắn nhất trên đồ thị có trọng số âm, và còn phát hiện được trường hợp không tồn tại đường đi ngắn nhất.

Ở Việt Nam, bạn có thể nghe đến "SPFA", thực chất là một cách cài đặt Bellman–Ford.

### Quy trình

Trước tiên, giải thích phép nới lỏng (relax) (Dijkstra cũng dùng phép này):

Với cạnh $(u,v)$, phép nới lỏng là: $dis(v) = \min(dis(v), dis(u) + w(u, v))$.

Ý nghĩa: thử dùng đường đi $S \to u \to v$ (trong đó $S \to u$ là đường đi ngắn nhất hiện tại) để cập nhật $dis(v)$, nếu tốt hơn thì cập nhật.

Bellman–Ford liên tục thử nới lỏng tất cả các cạnh. Mỗi vòng lặp, thử nới lỏng mọi cạnh một lần; khi không còn cạnh nào được nới lỏng, thuật toán dừng.

Mỗi vòng $O(m)$, tối đa bao nhiêu vòng?

Nếu tồn tại đường đi ngắn nhất, mỗi lần nới lỏng tăng số cạnh ít nhất $+1$, mà đường đi ngắn nhất tối đa $n-1$ cạnh, nên tối đa $n-1$ vòng, tổng $O(nm)$.

Nếu từ $S$ đi được tới chu trình âm, phép nới lỏng sẽ không bao giờ dừng. Nếu vòng thứ $n$ vẫn còn nới lỏng được, tức là từ $S$ đi được tới chu trình âm.

???+ warning "Những sai lầm thường gặp khi phát hiện chu trình âm"
    Khi chạy Bellman–Ford từ $S$, nếu không phát hiện chu trình âm, chỉ có thể kết luận từ $S$ không đi được tới chu trình âm, **không thể** kết luận toàn đồ thị không có chu trình âm.

    Nếu cần kiểm tra toàn đồ thị có chu trình âm, hãy thêm một siêu nguồn nối tới mọi đỉnh bằng cạnh trọng số $0$, rồi chạy Bellman–Ford từ siêu nguồn.

### Cài đặt

??? note "Cài đặt tham khảo"
    === "C++"
        ```cpp
        struct Edge {
          int u, v, w;
        };
        
        vector<Edge> edge;
        
        int dis[MAXN], u, v, w;
        constexpr int INF = 0x3f3f3f3f;
        
        bool bellmanford(int n, int s) {
          memset(dis, 0x3f, (n + 1) * sizeof(int));
          dis[s] = 0;
          bool flag = false;  // Kiểm tra vòng lặp này có nới lỏng được không
          for (int i = 1; i <= n; i++) {
            flag = false;
            for (int j = 0; j < edge.size(); j++) {
              u = edge[j].u, v = edge[j].v, w = edge[j].w;
              if (dis[u] == INF) continue;
              // INF cộng/trừ với số hữu hạn vẫn là INF
              // Nên cạnh xuất phát từ đỉnh có dis = INF không thể nới lỏng
              if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                flag = true;
              }
            }
            // Không còn cạnh nào nới lỏng được thì dừng
            if (!flag) {
              break;
            }
          }
          // Nếu vòng thứ n vẫn còn nới lỏng được, tức là từ s đi được tới chu trình âm
          return flag;
        }
        ```
    
    === "Python"
        ```python
        class Edge:
            def __init__(self, u=0, v=0, w=0):
                self.u = u
                self.v = v
                self.w = w
        
        
        INF = 0x3F3F3F3F
        edge = []
        
        
        def bellmanford(n, s):
            dis = [INF] * (n + 1)
            dis[s] = 0
            for i in range(1, n + 1):
                flag = False
                for e in edge:
                    u, v, w = e.u, e.v, e.w
                    if dis[u] == INF:
                        continue
                    # INF cộng/trừ với số hữu hạn vẫn là INF
                    # Nên cạnh xuất phát từ đỉnh có dis = INF không thể nới lỏng
                    if dis[v] > dis[u] + w:
                        dis[v] = dis[u] + w
                        flag = True
                # Không còn cạnh nào nới lỏng được thì dừng
                if not flag:
                    break
            # Nếu vòng thứ n vẫn còn nới lỏng được, tức là từ s đi được tới chu trình âm
            return flag
        ```

### Tối ưu bằng hàng đợi: SPFA

SPFA (Shortest Path Faster Algorithm).

Nhiều khi không cần nới lỏng tất cả các cạnh.

Chỉ những đỉnh vừa được nới lỏng, các cạnh xuất phát từ đó mới có khả năng tiếp tục nới lỏng.

Dùng hàng đợi để lưu các đỉnh có khả năng nới lỏng, chỉ duyệt các cạnh cần thiết.

SPFA cũng có thể dùng để kiểm tra từ $s$ đi được tới chu trình âm không: chỉ cần đếm số cạnh trên đường đi ngắn nhất, nếu vượt quá $n$ thì chắc chắn đi qua chu trình âm.

??? note "Cài đặt"
    === "C++"
        ```cpp
        struct edge {
          int v, w;
        };
        
        vector<edge> e[MAXN];
        int dis[MAXN], cnt[MAXN], vis[MAXN];
        queue<int> q;
        
        bool spfa(int n, int s) {
          memset(dis, 0x3f, (n + 1) * sizeof(int));
          dis[s] = 0, vis[s] = 1;
          q.push(s);
          while (!q.empty()) {
            int u = q.front();
            q.pop(), vis[u] = 0;
            for (auto ed : e[u]) {
              int v = ed.v, w = ed.w;
              if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                cnt[v] = cnt[u] + 1;  // Đếm số cạnh trên đường đi ngắn nhất
                if (cnt[v] >= n) return false;
                // Nếu không qua chu trình âm, đường đi ngắn nhất tối đa n-1 cạnh
                // Nếu vượt quá n cạnh, chắc chắn qua chu trình âm
                if (!vis[v]) q.push(v), vis[v] = 1;
              }
            }
          }
          return true;
        }
        ```
    
    === "Python"
        ```python
        from collections import deque
        
        
        class Edge:
            def __init__(self, v=0, w=0):
                self.v = v
                self.w = w
        
        
        e = [[Edge() for i in range(MAXN)] for j in range(MAXN)]
        INF = 0x3F3F3F3F
        
        
        def spfa(n, s):
            dis = [INF] * (n + 1)
            cnt = [0] * (n + 1)
            vis = [False] * (n + 1)
            q = deque()
        
            dis[s] = 0
            vis[s] = True
            q.append(s)
            while q:
                u = q.popleft()
                vis[u] = False
                for ed in e[u]:
                    v, w = ed.v, ed.w
                    if dis[v] > dis[u] + w:
                        dis[v] = dis[u] + w
                        cnt[v] = cnt[u] + 1  # Đếm số cạnh trên đường đi ngắn nhất
                        if cnt[v] >= n:
                            return False
                        # Nếu không qua chu trình âm, đường đi ngắn nhất tối đa n-1 cạnh
                        # Nếu vượt quá n cạnh, chắc chắn qua chu trình âm
                        if not vis[v]:
                            q.append(v)
                            vis[v] = True
        ```

Dù SPFA thường chạy rất nhanh, nhưng trường hợp xấu nhất vẫn là $O(nm)$, và có thể bị "hack" để đạt độ phức tạp này. Khi thi, hãy cân nhắc: nếu không có cạnh âm thì nên dùng Dijkstra; nếu có cạnh âm mà đề không có tính chất đặc biệt, nếu SPFA là lời giải mẫu thì đề không nên cho dữ liệu mà Bellman–Ford không chạy được.

???+ note "Các tối ưu khác của Bellman–Ford"
    Ngoài tối ưu bằng hàng đợi (SPFA), Bellman–Ford còn nhiều tối ưu khác, có thể hiệu quả trên một số đồ thị, nhưng trường hợp xấu nhất có thể lên tới độ phức tạp lũy thừa.

    -   Tối ưu bằng heap: thay hàng đợi bằng heap, cho phép một đỉnh vào heap nhiều lần (khác Dijkstra). Nếu có cạnh âm, có thể bị hack lên độ phức tạp lũy thừa.
    -   Tối ưu bằng stack: thay hàng đợi bằng stack (BFS thành DFS), khi tìm chu trình âm có thể nhanh hơn, nhưng trường hợp xấu nhất vẫn lũy thừa.
    -   LLL: dùng deque, so sánh khoảng cách với trung bình các đỉnh trong hàng đợi, lớn hơn thì vào cuối, nhỏ hơn thì vào đầu.
    -   SLF: dùng deque, so sánh với đầu hàng đợi, lớn hơn thì vào cuối, nhỏ hơn thì vào đầu.
    -   D´Esopo–Pape: dùng deque, nếu đỉnh chưa từng vào hàng đợi thì vào cuối, nếu đã vào thì vào đầu.

    Tham khảo thêm các tối ưu và cách hack tại [fstqwq trên Zhihu](https://www.zhihu.com/question/292283275/answer/484871888).

## Thuật toán Dijkstra

Dijkstra (/ˈdikstrɑ/ hoặc /ˈdɛikstrɑ/) do nhà khoa học máy tính người Hà Lan E. W. Dijkstra phát minh năm 1956, công bố năm 1959. Là thuật toán tìm đường đi ngắn nhất từ một nguồn trên đồ thị **không có cạnh âm**.

### Quy trình

Chia các đỉnh thành hai tập: tập đã xác định được độ dài đường đi ngắn nhất ($S$), và tập chưa xác định ($T$). Ban đầu, tất cả các đỉnh thuộc $T$.

Khởi tạo $dis(s)=0$, các đỉnh khác $dis=+\infty$.

Lặp lại:

1.  Chọn đỉnh có $dis$ nhỏ nhất trong $T$, chuyển sang $S$.
2.  Với các cạnh xuất phát từ đỉnh vừa chuyển sang $S$, thực hiện phép nới lỏng.

Lặp đến khi $T$ rỗng, thuật toán kết thúc.

### Độ phức tạp

Cài đặt đơn giản: mỗi lần tìm đỉnh nhỏ nhất trong $T$ bằng vét cạn, tổng $O(n^2)$; phép nới lỏng tổng $O(m)$.

Có thể tối ưu bằng heap: mỗi lần nới lỏng cạnh $(u,v)$, đưa $v$ vào heap (nếu đã có thì Decrease-key). Tổng $O(m)$ lần Decrease-key, $O(n)$ lần pop. Tùy loại heap, độ phức tạp khác nhau, xem thêm [heap](../ds/heap.md). Tốt nhất là $O(n\log n + m)$ (heap Fibonacci).

Có thể dùng priority_queue, không hỗ trợ Decrease-key, nhưng mỗi lần nới lỏng lại đưa vào heap, khi lấy ra kiểm tra đã cập nhật chưa, độ phức tạp $O(m\log n)$, dễ cài đặt.

Cũng có thể dùng segment tree, độ phức tạp $O(m\log n)$, đôi khi hằng số nhỏ hơn heap, và hỗ trợ nhiều thao tác hơn.

Đồ thị thưa ($m=O(n)$), Dijkstra heap tối ưu; đồ thị dày ($m=O(n^2)$), cài đặt đơn giản tối ưu.

### Chứng minh đúng

Dùng quy nạp toán học, với giả thiết **mọi cạnh đều không âm**.

Cần chứng minh: mỗi lần chọn đỉnh $u$ vào $S$, thì $D(u)=dis(u)$.

Ban đầu $S=\varnothing$, đúng.

Giả sử $u$ là đỉnh đầu tiên vào $S$ mà $D(u)\neq dis(u)$. Vì $s$ luôn đúng, nên trước đó $S\neq\varnothing$. Nếu không có đường từ $s$ đến $u$, thì $D(u)=dis(u)=+\infty$, mâu thuẫn.

Nên tồn tại đường $s\to x\to y\to u$, với $y$ là đỉnh đầu tiên trên đường thuộc $T$, $x$ là trước $y$ ($x\in S$). Có thể $s=x$ hoặc $y=u$.

Vì các đỉnh trước đều đúng, nên khi $x$ vào $S$, $D(x)=dis(x)$, cạnh $(x,y)$ sẽ được nới lỏng, nên khi $u$ vào $S$, $D(y)=dis(y)$.

Vì mọi cạnh không âm, $D(y)\leq D(u)$, nên $dis(y)=D(y)\leq D(u)\leq dis(u)$. Nhưng $u$ vào $S$ trước $y$, nên $dis(u)\leq dis(y)$, do đó $dis(y)=D(y)=D(u)=dis(u)$, mâu thuẫn với giả thiết $D(u)\neq dis(u)$.

Vậy mỗi đỉnh vào $S$ đều đúng.

Chú ý: bất đẳng thức $D(y)\leq D(u)$ chỉ đúng khi mọi cạnh không âm. Nếu có cạnh âm, Dijkstra có thể sai.

### Cài đặt

Dưới đây là cả cài đặt vét cạn $O(n^2)$ và cài đặt dùng priority_queue $O(m\log n)$.

???+ note "Cài đặt vét cạn"
    === "C++"
        ```cpp
        struct edge {
          int v, w;
        };
        
        vector<edge> e[MAXN];
        int dis[MAXN], vis[MAXN];
        
        void dijkstra(int n, int s) {
          memset(dis, 0x3f, (n + 1) * sizeof(int));
          dis[s] = 0;
          for (int i = 1; i <= n; i++) {
            int u = 0, mind = 0x3f3f3f3f;
            for (int j = 1; j <= n; j++)
              if (!vis[j] && dis[j] < mind) u = j, mind = dis[j];
            vis[u] = true;
            for (auto ed : e[u]) {
              int v = ed.v, w = ed.w;
              if (dis[v] > dis[u] + w) dis[v] = dis[u] + w;
            }
          }
        }
        ```
    
    === "Python"
        ```python
        class Edge:
            def __init(self, v=0, w=0):
                self.v = v
                self.w = w
        
        
        e = [[Edge() for i in range(MAXN)] for j in range(MAXN)]
        INF = 0x3F3F3F3F
        
        
        def dijkstra(n, s):
            dis = [INF] * (n + 1)
            vis = [0] * (n + 1)
        
            dis[s] = 0
            for i in range(1, n + 1):
                u = 0
                mind = INF
                for j in range(1, n + 1):
                    if not vis[j] and dis[j] < mind:
                        u = j
                        mind = dis[j]
                vis[u] = True
                for ed in e[u]:
                    v, w = ed.v, ed.w
                    if dis[v] > dis[u] + w:
                        dis[v] = dis[u] + w
        ```

???+ note "Cài đặt dùng priority_queue"
    === "C++"
        ```cpp
        struct edge {
          int v, w;
        };
        
        struct node {
          int dis, u;
        
          bool operator>(const node& a) const { return dis > a.dis; }
        };
        
        vector<edge> e[MAXN];
        int dis[MAXN], vis[MAXN];
        priority_queue<node, vector<node>, greater<node>> q;
        
        void dijkstra(int n, int s) {
          memset(dis, 0x3f, (n + 1) * sizeof(int));
          memset(vis, 0, (n + 1) * sizeof(int));
          dis[s] = 0;
          q.push({0, s});
          while (!q.empty()) {
            int u = q.top().u;
            q.pop();
            if (vis[u]) continue;
            vis[u] = 1;
            for (auto ed : e[u]) {
              int v = ed.v, w = ed.w;
              if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                q.push({dis[v], v});
              }
            }
          }
        }
        ```
    
    === "Python"
        ```python
        def dijkstra(e, s):
            """
            输入：
            e:邻接表
            s:起点
            返回：
            dis:从s到每个顶点的最短路长度
            """
            dis = defaultdict(lambda: float("inf"))
            dis[s] = 0
            q = [(0, s)]
            vis = set()
            while q:
                _, u = heapq.heappop(q)
                if u in vis:
                    continue
                vis.add(u)
                for v, w in e[u]:
                    if dis[v] > dis[u] + w:
                        dis[v] = dis[u] + w
                        heapq.heappush(q, (dis[v], v))
            return dis
        ```

## Thuật toán Johnson cho bài toán mọi cặp đường đi ngắn nhất

Johnson, giống Floyd, là thuật toán tìm đường đi ngắn nhất giữa mọi cặp đỉnh trên đồ thị không có chu trình âm. Được Donald B. Johnson đề xuất năm 1977.

Có thể giải bài toán này bằng cách chạy Bellman–Ford $n$ lần (mỗi đỉnh làm nguồn), độ phức tạp $O(n^2m)$, hoặc dùng Floyd $O(n^3)$.

Nhưng Dijkstra heap tối ưu cho đơn nguồn, nên nếu chạy $n$ lần Dijkstra, tổng $O(nm\log m)$, tốt hơn Bellman–Ford và Floyd trên đồ thị thưa.

Tuy nhiên, Dijkstra không xử lý được cạnh âm, nên cần biến đổi đồ thị để mọi cạnh đều không âm.

Một ý tưởng là cộng thêm một số dương $x$ vào mọi cạnh, rồi trừ $kx$ với đường đi qua $k$ cạnh. Nhưng cách này **sai**. Xem ví dụ:

![](./images/shortest-path1.svg)

$1 \to 2$ ngắn nhất là $1 \to 5 \to 3 \to 2$, độ dài $-2$.

Nếu cộng $5$ vào mọi cạnh:

![](./images/shortest-path2.svg)

Lúc này $1 \to 2$ ngắn nhất lại là $1 \to 4 \to 2$, không đúng.

Johnson dùng cách khác: thêm một đỉnh ảo (đánh số $0$), nối tới mọi đỉnh bằng cạnh trọng số $0$.

Chạy Bellman–Ford từ đỉnh $0$, gọi $h_i$ là đường đi ngắn nhất từ $0$ đến $i$.

Với mỗi cạnh $(u,v)$ trọng số $w$, thay bằng $w+h_u-h_v$.

Sau đó, chạy Dijkstra $n$ lần từ mỗi đỉnh, sẽ tìm được mọi đường đi ngắn nhất.

Bellman–Ford ban đầu không phải nút thắt thời gian, nếu dùng priority_queue cho Dijkstra thì tổng độ phức tạp $O(nm\log m)$.

### Chứng minh đúng

Tại sao cách này đúng?

Trước hết, xét khái niệm vật lý: thế năng (potential). Thế năng chỉ phụ thuộc vào vị trí đầu/cuối, không phụ thuộc đường đi.

Thế năng tuyệt đối phụ thuộc vào điểm gốc, nhưng hiệu thế giữa hai điểm là không đổi.

Quay lại bài toán:

Trên đồ thị mới, đường đi $s \to t$ qua $s \to p_1 \to p_2 \to \dots \to p_k \to t$ có độ dài:

$(w(s,p_1)+h_s-h_{p_1})+(w(p_1,p_2)+h_{p_1}-h_{p_2})+ \dots +(w(p_k,t)+h_{p_k}-h_t)$

Rút gọn còn:

$w(s,p_1)+w(p_1,p_2)+ \dots +w(p_k,t)+h_s-h_t$

Dù đi đường nào, $h_s-h_t$ không đổi, đúng như tính chất thế năng.

Gọi $h_i$ là thế năng của đỉnh $i$.

Như vậy, đường đi ngắn nhất trên đồ thị mới bằng đường đi ngắn nhất trên đồ thị gốc cộng hiệu thế năng hai đầu.

Cần chứng minh mọi cạnh trên đồ thị mới đều không âm: theo bất đẳng thức tam giác, với mọi cạnh $(u,v)$, $h_v \leq h_u + w(u,v)$, nên $w'(u,v)=w(u,v)+h_u-h_v \geq 0$.

Vậy Johnson là đúng.

## So sánh các phương pháp

| Thuật toán ngắn nhất | Floyd      | Bellman–Ford | Dijkstra     | Johnson       |
| ------------------- | ---------- | ------------ | ------------ | ------------- |
| Loại đường đi       | Mọi cặp     | Đơn nguồn     | Đơn nguồn     | Mọi cặp        |
| Áp dụng cho         | Mọi đồ thị   | Mọi đồ thị    | Đồ thị không âm | Mọi đồ thị      |
| Phát hiện chu trình âm? | Có      | Có           | Không         | Có            |
| Độ phức tạp         | $O(N^3)$   | $O(NM)$      | $O(M\log M)$ | $O(NM\log M)$ |

Lưu ý: Dijkstra trong bảng dùng priority_queue.

## Xuất đường đi

Dùng mảng `pre`, khi cập nhật khoảng cách thì lưu lại đỉnh trước đó, sau thuật toán thì truy vết ngược để in đường đi.

Ví dụ Floyd lưu `pre[i][j] = k;`, Bellman–Ford và Dijkstra thường lưu `pre[v] = u`.

## Một số trường hợp đặc biệt

-   Đồ thị chỉ có trọng số $0$ và $1$: [0-1 BFS](./bfs.md#双端队列-bfs);
-   Bài toán cho phép tối đa $k$ lần thay đổi chi phí cạnh: [Đồ thị phân lớp và đường đi ngắn nhất](./node.md#分层图最短路).

## Tài liệu tham khảo & chú thích

[^1]: "Giáo trình Thuật toán (CLRS bản dịch lần 3)", NXB Cơ khí Công nghiệp, 2013, trang 384 - 385.
