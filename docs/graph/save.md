author: Ir1d, sshwy, Xeonacid, partychicken, Anguei, HeRaNO
Để thao tác với đồ thị trong OI, trước tiên bạn cần nắm vững các phương pháp lưu trữ đồ thị.

## Quy ước

Bài viết này giả định bạn đã đọc và hiểu các [khái niệm cơ bản về đồ thị](./concept.md). Nếu gặp khó khăn khi đọc, bạn cũng có thể tham khảo lại tại [khái niệm đồ thị](./concept.md).

Trong bài này, $n$ là số đỉnh của đồ thị, $m$ là số cạnh, $d^+(u)$ là bậc ra của đỉnh $u$, tức số cạnh xuất phát từ $u$.

## Lưu trực tiếp danh sách cạnh

### Phương pháp

Sử dụng một mảng để lưu các cạnh, mỗi phần tử của mảng chứa thông tin đỉnh đầu và đỉnh cuối của một cạnh (nếu là đồ thị có trọng số thì lưu thêm trọng số). (Hoặc dùng nhiều mảng để lưu riêng đỉnh đầu, đỉnh cuối và trọng số.)

??? note "Mã mẫu"
    === "C++"
        ```cpp
        #include <iostream>
        #include <vector>
        
        using namespace std;
        
        struct Edge {
          int u, v;
        };
        
        int n, m;
        vector<Edge> e;
        vector<bool> vis;
        
        // Tìm xem có cạnh từ u đến v không
        bool find_edge(int u, int v) {
          for (int i = 1; i <= m; ++i) {
            if (e[i].u == u && e[i].v == v) {
              return true;
            }
          }
          return false;
        }
        
        // Duyệt DFS từ đỉnh u
        void dfs(int u) {
          if (vis[u]) return;
          vis[u] = true;
          for (int i = 1; i <= m; ++i) {
            if (e[i].u == u) {
              dfs(e[i].v);
            }
          }
        }
        
        int main() {
          cin >> n >> m;
        
          vis.resize(n + 1, false);
          e.resize(m + 1);
        
          for (int i = 1; i <= m; ++i) cin >> e[i].u >> e[i].v;
        
          return 0;
        }
        ```
    
    === "Python"
        ```python
        class Edge:
            def __init__(self, u=0, v=0):
                self.u = u
                self.v = v
        
        
        n, m = map(int, input().split())
        
        e = [Edge() for _ in range(m)]
        vis = [False] * n
        
        for i in range(m):
            e[i].u, e[i].v = map(int, input().split())
        
        
        # Tìm xem có cạnh từ u đến v không
        def find_edge(u, v):
            for i in range(m):
                if e[i].u == u and e[i].v == v:
                    return True
            return False
        
        
        # Duyệt DFS từ đỉnh u
        def dfs(u):
            if vis[u]:
                return
            vis[u] = True
            for i in range(m):
                if e[i].u == u:
                    dfs(e[i].v)
        ```

### Độ phức tạp

Truy vấn xem có cạnh nào tồn tại: $O(m)$.

Duyệt tất cả các cạnh xuất phát từ một đỉnh: $O(m)$.

Duyệt toàn bộ đồ thị: $O(nm)$.

Độ phức tạp không gian: $O(m)$.

### Ứng dụng

Do việc duyệt cạnh bằng cách lưu trực tiếp không hiệu quả, phương pháp này thường không dùng để duyệt đồ thị.

Trong [thuật toán Kruskal](./mst.md#kruskal-算法), do cần sắp xếp các cạnh theo trọng số, nên thường lưu trực tiếp danh sách cạnh.

Trong một số bài toán cần xây dựng lại đồ thị nhiều lần (ví dụ vừa lưu đồ thị gốc, vừa lưu đồ thị ngược), bạn có thể dùng nhiều cấu trúc dữ liệu khác nhau để lưu đồng thời nhiều đồ thị, hoặc chỉ cần lưu danh sách cạnh, khi cần xây lại đồ thị thì dùng danh sách cạnh này.

## Ma trận kề

### Phương pháp

Sử dụng một mảng hai chiều `adj` để lưu cạnh, trong đó `adj[u][v]` bằng 1 nếu tồn tại cạnh từ $u$ đến $v$, bằng 0 nếu không tồn tại. Nếu là đồ thị có trọng số, có thể lưu trọng số cạnh tại `adj[u][v]`.

??? note "Mã mẫu"
    === "C++"
        ```cpp
        #include <iostream>
        #include <vector>
        
        using namespace std;
        
        int n, m;
        vector<bool> vis;
        vector<vector<bool>> adj;
        
        // Kiểm tra có cạnh từ u đến v không
        bool find_edge(int u, int v) { return adj[u][v]; }
        
        // Duyệt DFS từ đỉnh u
        void dfs(int u) {
          if (vis[u]) return;
          vis[u] = true;
          for (int v = 1; v <= n; ++v) {
            if (adj[u][v]) {
              dfs(v);
            }
          }
        }
        
        int main() {
          cin >> n >> m;
        
          vis.resize(n + 1);
          adj.resize(n + 1, vector<bool>(n + 1));
        
          for (int i = 1; i <= m; ++i) {
            int u, v;
            cin >> u >> v;
            adj[u][v] = true;
          }
        
          return 0;
        }
        ```
    
    === "Python"
        ```python
        vis = [False] * (n + 1)
        adj = [[False] * (n + 1) for _ in range(n + 1)]
        
        for i in range(1, m + 1):
            u, v = map(lambda x: int(x), input().split())
            adj[u][v] = True
        
        
        # Kiểm tra có cạnh từ u đến v không
        def find_edge(u, v):
            return adj[u][v]
        
        
        # Duyệt DFS từ đỉnh u
        def dfs(u):
            if vis[u]:
                return
            vis[u] = True
            for v in range(1, n + 1):
                if adj[u][v]:
                    dfs(v)
        ```

### Độ phức tạp

Truy vấn xem có cạnh nào tồn tại: $O(1)$.

Duyệt tất cả các cạnh xuất phát từ một đỉnh: $O(n)$.

Duyệt toàn bộ đồ thị: $O(n^2)$.

Độ phức tạp không gian: $O(n^2)$.

### Ứng dụng

Ma trận kề chỉ phù hợp với đồ thị không có cạnh trùng (hoặc có thể bỏ qua cạnh trùng).

Ưu điểm lớn nhất là có thể truy vấn cạnh tồn tại trong $O(1)$.

Vì ma trận kề rất tốn bộ nhớ với đồ thị thưa (đặc biệt khi số đỉnh lớn), nên thường chỉ dùng với đồ thị dày đặc.

## Danh sách kề

### Phương pháp

Sử dụng một mảng mà mỗi phần tử là một cấu trúc dữ liệu động (như `vector<int> adj[n + 1]`) để lưu cạnh, trong đó `adj[u]` lưu thông tin tất cả các cạnh xuất phát từ $u$ (đỉnh cuối, trọng số, ...).

??? note "Mã mẫu"
    === "C++"
        ```cpp
        #include <iostream>
        #include <vector>
        
        using namespace std;
        
        int n, m;
        vector<bool> vis;
        vector<vector<int>> adj;
        
        // Kiểm tra có cạnh từ u đến v không
        bool find_edge(int u, int v) {
          for (int i = 0; i < adj[u].size(); ++i) {
            if (adj[u][i] == v) {
              return true;
            }
          }
          return false;
        }
        
        // Duyệt DFS từ đỉnh u
        void dfs(int u) {
          if (vis[u]) return;
          vis[u] = true;
          for (int i = 0; i < adj[u].size(); ++i) dfs(adj[u][i]);
        }
        
        int main() {
          cin >> n >> m;
        
          vis.resize(n + 1);
          adj.resize(n + 1);
        
          for (int i = 1; i <= m; ++i) {
            int u, v;
            cin >> u >> v;
            adj[u].push_back(v);
          }
        
          return 0;
        }
        ```
    
    === "Python"
        ```python
        vis = [False] * (n + 1)
        adj = [[] for _ in range(n + 1)]
        
        for i in range(1, m + 1):
            u, v = map(lambda x: int(x), input().split())
            adj[u].append(v)
        
        
        # Kiểm tra có cạnh từ u đến v không
        def find_edge(u, v):
            for i in range(0, len(adj[u])):
                if adj[u][i] == v:
                    return True
            return False
        
        
        # Duyệt DFS từ đỉnh u
        def dfs(u):
            if vis[u]:
                return
            vis[u] = True
            for i in range(0, len(adj[u])):
                dfs(adj[u][i])
        ```

### Độ phức tạp

Truy vấn xem có cạnh từ $u$ đến $v$: $O(d^+(u))$ (nếu đã sắp xếp trước thì có thể dùng [tìm kiếm nhị phân](../basic/binary.md) để đạt $O(\log(d^+(u)))$).

Duyệt tất cả các cạnh xuất phát từ một đỉnh $u$: $O(d^+(u))$.

Duyệt toàn bộ đồ thị: $O(n+m)$.

Độ phức tạp không gian: $O(m)$.

### Ứng dụng

Phù hợp với hầu hết các loại đồ thị, trừ khi có yêu cầu đặc biệt (như cần truy vấn cạnh nhanh và số đỉnh nhỏ thì dùng ma trận kề).

Đặc biệt thích hợp khi cần sắp xếp các cạnh xuất phát từ một đỉnh.

## Mô hình danh sách kề dạng chuỗi (Chain Forward Star)

### Phương pháp

Bản chất là dùng danh sách liên kết để hiện thực danh sách kề, mã lõi như sau:

=== "C++"
    ```cpp
    // Giá trị khởi tạo ban đầu của head[u] và cnt đều là -1
    void add(int u, int v) {
      nxt[++cnt] = head[u];  // Cạnh tiếp theo của đỉnh u
      head[u] = cnt;         // Cập nhật cạnh đầu tiên của u
      to[cnt] = v;           // Đỉnh cuối của cạnh hiện tại
    }
    
    // Duyệt các cạnh xuất phát từ u
    for (int i = head[u]; ~i; i = nxt[i]) {  // ~i nghĩa là i != -1
      int v = to[i];
    }
    ```

=== "Python"
    ```python
    # Giá trị khởi tạo ban đầu của head[u] và cnt đều là -1
    def add(u, v):
        cnt = cnt + 1
        nex[cnt] = head[u]  # Cạnh tiếp theo của đỉnh u
        head[u] = cnt  # Cập nhật cạnh đầu tiên của u
        to[cnt] = v  # Đỉnh cuối của cạnh hiện tại
    
    
    # Duyệt các cạnh xuất phát từ u
    i = head[u]
    while ~i:  # ~i nghĩa là i != -1
        v = to[i]
        i = nxt[i]
    ```

??? note "Mã mẫu"
    ```cpp
    #include <iostream>
    #include <vector>
    
    using namespace std;
    
    int n, m;
    vector<bool> vis;
    vector<int> head, nxt, to;
    
    // Thêm cạnh từ u đến v
    void add(int u, int v) {
      nxt.push_back(head[u]);
      head[u] = to.size();
      to.push_back(v);
    }
    
    // Kiểm tra có cạnh từ u đến v không
    bool find_edge(int u, int v) {
      for (int i = head[u]; ~i; i = nxt[i]) {  // ~i nghĩa là i != -1
        if (to[i] == v) {
          return true;
        }
      }
      return false;
    }
    
    // Duyệt DFS từ đỉnh u
    void dfs(int u) {
      if (vis[u]) return;
      vis[u] = true;
      for (int i = head[u]; ~i; i = nxt[i]) dfs(to[i]);
    }
    
    int main() {
      cin >> n >> m;
    
      vis.resize(n + 1, false);
      head.resize(n + 1, -1);
    
      for (int i = 1; i <= m; ++i) {
        int u, v;
        cin >> u >> v;
        add(u, v);
      }
    
      return 0;
    }
    ```

### Độ phức tạp

Truy vấn xem có cạnh từ $u$ đến $v$: $O(d^+(u))$.

Duyệt tất cả các cạnh xuất phát từ một đỉnh $u$: $O(d^+(u))$.

Duyệt toàn bộ đồ thị: $O(n+m)$.

Độ phức tạp không gian: $O(m)$.

### Ứng dụng

Phù hợp với hầu hết các loại đồ thị, nhưng không thể truy vấn cạnh nhanh, cũng không dễ sắp xếp các cạnh xuất phát từ một đỉnh.

Ưu điểm là mỗi cạnh đều có chỉ số, đôi khi rất hữu ích. Nếu khởi tạo `cnt` là số lẻ, khi lưu cạnh hai chiều thì `i ^ 1` sẽ là cạnh ngược của `i` (thường dùng trong [mạng lưới dòng chảy](./flow.md)).
