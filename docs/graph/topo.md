author: marscheng1

## Định nghĩa

Sắp xếp topo (Topological sorting) là bài toán sắp xếp tất cả các đỉnh của một đồ thị có hướng không chu trình (DAG).

Ta có thể lấy ví dụ xếp lịch học đại học: các môn như "Lập trình", "Ngôn ngữ thuật toán", "Toán cao cấp", "Toán rời rạc", "Kỹ thuật biên dịch", "Vật lý đại cương", "Cấu trúc dữ liệu", "Hệ quản trị cơ sở dữ liệu"... Khi muốn học "Cấu trúc dữ liệu" thì phải học trước "Toán rời rạc", sau đó mới đủ điều kiện học "Kỹ thuật biên dịch". Ngoài ra, "Kỹ thuật biên dịch" còn cần học trước "Ngôn ngữ thuật toán". Các môn học này tương ứng với các đỉnh $u$, các cạnh có hướng $(u,v)$ tương ứng với thứ tự học các môn. Việc phòng đào tạo sắp xếp lịch học sao cho hợp lý chính là quá trình sắp xếp topo.

![topo](images/topo-example-1.svg)

Nếu một ngày nào đó, giáo vụ nhầm lẫn, yêu cầu học "Cấu trúc dữ liệu" phải học trước "Hệ điều hành", mà "Hệ điều hành" lại yêu cầu học trước "Cấu trúc dữ liệu", thì không biết nên học môn nào trước (không xét trường hợp học song song). Khi đó giữa "Cấu trúc dữ liệu" và "Hệ điều hành" xuất hiện một chu trình, rõ ràng không thể xác định thứ tự học, cũng không thể sắp xếp topo. Vì vậy, nếu đồ thị có hướng có chu trình, không thể sắp xếp topo.

Vậy, trong một [DAG (đồ thị có hướng không chu trình)](./dag.md), ta sắp xếp các đỉnh thành một dãy tuyến tính sao cho với mọi cạnh $(u,v)$, $u$ luôn đứng trước $v$.

Nếu từ $i$ đến $j$ có cạnh, thì $j$ phụ thuộc vào $i$. Nếu từ $i$ đến $j$ có đường đi, gọi là $j$ phụ thuộc gián tiếp vào $i$.

Mục tiêu của sắp xếp topo là sắp xếp các đỉnh sao cho đỉnh đứng trước không phụ thuộc vào đỉnh đứng sau.

## Mạng AOV

Trong thực tế, một dự án lớn gồm nhiều công việc nhỏ, giữa các công việc này có thứ tự trước sau, tức là một số công việc phải hoàn thành trước khi các công việc khác bắt đầu.

Ta dùng đồ thị có hướng để biểu diễn quan hệ trước sau giữa các công việc, các cạnh có hướng thể hiện thứ tự, đồ thị này gọi là **mạng hoạt động trên đỉnh (AOV - Activity On Vertex Network)**. Một mạng AOV luôn là DAG. Khác với DAG thông thường, trong AOV, các hoạt động gắn với đỉnh. (Ví dụ trên chính là một mạng AOV.)

Trong AOV, đỉnh là hoạt động, cạnh là quan hệ ưu tiên giữa các hoạt động. Không nên có chu trình trong AOV, như vậy mới có thể tìm được một dãy các đỉnh sao cho mọi hoạt động tiền nhiệm đều đứng trước hoạt động đó, dãy này gọi là dãy topo (không duy nhất). Quá trình xây dựng dãy topo từ AOV gọi là sắp xếp topo. Nói cách khác, sắp xếp topo là sắp xếp các hoạt động sao cho mọi hoạt động tiền nhiệm đều đứng trước hoạt động đó (không duy nhất).

-   Hoạt động tiền nhiệm: Hoạt động ở đầu cạnh có hướng, là tiền nhiệm của hoạt động ở cuối cạnh (chỉ khi mọi hoạt động tiền nhiệm hoàn thành thì hoạt động này mới bắt đầu).
-   Hoạt động kế tiếp: Hoạt động ở cuối cạnh có hướng, là kế tiếp của hoạt động ở đầu cạnh.

Kiểm tra AOV có chu trình hay không bằng cách xây dựng dãy topo, xem có đủ tất cả các đỉnh không.

### Các bước xây dựng dãy topo

1.  Chọn một đỉnh có bậc vào bằng 0.
2.  Xuất đỉnh đó, xóa đỉnh và tất cả các cạnh xuất phát từ nó.

Lặp lại hai bước trên cho đến khi xuất hết các đỉnh (thành công), hoặc không còn đỉnh nào có bậc vào 0 (đồ thị có chu trình, không thể sắp xếp topo, rơi vào deadlock).

## Đường găng (Critical Path) và mạng AOE

Tương ứng với AOV là **mạng hoạt động trên cạnh (AOE - Activity On Edge Network)**, tức là hoạt động gắn với cạnh. AOE là một DAG có trọng số, đỉnh là sự kiện, cạnh là hoạt động với thời lượng. Thường dùng AOE để ước lượng thời gian hoàn thành dự án. AOE phải là DAG, có duy nhất một đỉnh nguồn (bậc vào 0) và một đỉnh đích (bậc ra 0).

![topo](images/topo-example-2.svg)

Trong AOE, một số hoạt động có thể thực hiện song song, nên thời gian hoàn thành dự án là độ dài đường đi dài nhất từ nguồn đến đích (tổng trọng số các cạnh trên đường đi). Đường đi dài nhất này gọi là đường găng (critical path), quyết định thời gian hoàn thành dự án.

### Một số khái niệm cơ bản trong AOE

-   Hoạt động: Trong AOE, cạnh là hoạt động, trọng số là thời lượng, hoạt động bắt đầu khi sự kiện tiền nhiệm (đỉnh đầu cạnh) xảy ra.
-   Sự kiện: Đỉnh trong AOE là sự kiện, xảy ra khi mọi hoạt động tiền nhiệm hoàn thành.
-   Thời điểm sớm nhất của sự kiện $v_i$: $ve(i)$, là thời điểm sớm nhất có thể xảy ra, quyết định thời điểm sớm nhất các hoạt động bắt đầu từ $v_i$ có thể xảy ra. $ve(i) = \max\{ve(j) + val^j_i ~|~ j \in pre_i\}$, $val^j_i$ là trọng số cạnh $j \to i$, $pre_i$ là tập các sự kiện tiền nhiệm của $i$. Với nguồn, $ve=0$.
-   Thời điểm muộn nhất của sự kiện $v_i$: $vl(i)$, là thời điểm muộn nhất có thể xảy ra mà không làm chậm tiến độ, quyết định thời điểm muộn nhất các hoạt động kết thúc tại $v_i$ có thể xảy ra. $vl(i) = \min\{vl(j) - val^i_j ~|~ j \in nxt_i\}$, $val^i_j$ là trọng số cạnh $i \to j$, $nxt_i$ là tập các sự kiện kế tiếp của $i$.
-   Thời điểm sớm nhất của hoạt động $(u, v)$: $e(u,v) = ve(u)$.
-   Thời điểm muộn nhất của hoạt động $(u, v)$: $l(u,v) = vl(v) - val^u_v$.
-   Đường găng: Độ dài đường đi dài nhất từ nguồn đến đích trong AOE.
-   Hoạt động găng: Hoạt động trên đường găng, có thời điểm sớm nhất và muộn nhất bằng nhau.

### Cách tính thời điểm sớm nhất và muộn nhất

Tính theo thứ tự topo: thời điểm sớm nhất tính từ trước ra sau, thời điểm muộn nhất tính từ sau về trước, công thức như trên.

## Thuật toán Kahn

### Quy trình

Ban đầu, tập $S$ chứa tất cả các đỉnh có bậc vào 0, $L$ là danh sách rỗng.

Mỗi lần lấy một đỉnh $u$ từ $S$ (tùy ý), đưa vào $L$, rồi xóa tất cả các cạnh xuất phát từ $u$. Với mỗi cạnh $(u, v)$, nếu sau khi xóa, $v$ có bậc vào 0 thì thêm $v$ vào $S$.

Lặp lại đến khi $S$ rỗng. Nếu còn cạnh trong đồ thị, tức là có chu trình; nếu không, $L$ là thứ tự topo.

Dưới đây là giả mã từ [Wikipedia](https://en.wikipedia.org/wiki/Topological_sorting#Kahn's_algorithm):

???+ note "Cài đặt"
    ```text
    L ← Empty list that will contain the sorted elements
    S ← Set of all nodes with no incoming edges
    while S is not empty do
        remove a node n from S
        insert n into L
        for each node m with an edge e from n to m do
            remove edge e from the graph
            if m has no other incoming edges then
                insert m into S
    if graph has edges then
        return error (graph has at least one cycle)
    else
        return L (a topologically sorted order)
    ```

Cốt lõi là duy trì tập các đỉnh có bậc vào 0.

Tham khảo hình minh họa:

![topo](images/topo-example.svg)

Kết quả sắp xếp topo: 2 -> 8 -> 0 -> 3 -> 7 -> 1 -> 5 -> 6 -> 9 -> 4 -> 11 -> 10 -> 12

### Độ phức tạp

Giả sử đồ thị $G = (V, E)$, khi khởi tạo tập $S$ phải duyệt toàn bộ đồ thị và các cạnh, $O(E+V)$. Các thao tác tiếp theo cũng $O(E+V)$.

Tổng độ phức tạp $O(E+V)$.

### Cài đặt

=== "C++"
    ```cpp
    int n, m;
    vector<int> G[MAXN];
    int in[MAXN];  // 存储每个结点的入度
    
    bool toposort() {
      vector<int> L;
      queue<int> S;
      for (int i = 1; i <= n; i++)
        if (in[i] == 0) S.push(i);
      while (!S.empty()) {
        int u = S.front();
        S.pop();
        L.push_back(u);
        for (auto v : G[u]) {
          if (--in[v] == 0) {
            S.push(v);
          }
        }
      }
      if (L.size() == n) {
        for (auto i : L) cout << i << ' ';
        return true;
      }
      return false;
    }
    ```

=== "Python"
    ```python
    from collections import defaultdict, deque
    
    
    def topo_sort(graph):
        lst = []
        in_degree = defaultdict(int)
        for u in graph:
            for v in graph[u]:
                in_degree[v] += 1
    
        s = deque([u for u in graph if in_degree[u] == 0])
        while s:
            u = s.popleft()
            lst.append(u)
            for v in graph.get(u, []):
                in_degree[v] -= 1
                if in_degree[v] == 0:
                    s.append(v)
    
        return None if any(in_degree.values()) else lst
    ```

## Thuật toán DFS

### Cài đặt

=== "C++"
    ```cpp
    using Graph = vector<vector<int>>;  // 邻接表
    
    struct TopoSort {
      enum class Status : uint8_t { to_visit, visiting, visited };
    
      const Graph& graph;
      const int n;
      vector<Status> status;
      vector<int> order;
      vector<int>::reverse_iterator it;
    
      TopoSort(const Graph& graph)
          : graph(graph),
            n(graph.size()),
            status(n, Status::to_visit),
            order(n),
            it(order.rbegin()) {}
    
      bool sort() {
        for (int i = 0; i < n; ++i) {
          if (status[i] == Status::to_visit && !dfs(i)) return false;
        }
        return true;
      }
    
      bool dfs(const int u) {
        status[u] = Status::visiting;
        for (const int v : graph[u]) {
          if (status[v] == Status::visiting) return false;
          if (status[v] == Status::to_visit && !dfs(v)) return false;
        }
        status[u] = Status::visited;
        *it++ = u;
        return true;
      }
    };
    ```

=== "Python"
    ```python
    from enum import Enum, auto
    
    
    class Status(Enum):
        to_visit = auto()
        visiting = auto()
        visited = auto()
    
    
    def topo_sort(graph: list[list[int]]) -> list[int] | None:
        n = len(graph)
        status = [Status.to_visit] * n
        order = []
    
        def dfs(u: int) -> bool:
            status[u] = Status.visiting
            for v in graph[u]:
                if status[v] == Status.visiting:
                    return False
                if status[v] == Status.to_visit and not dfs(v):
                    return False
            status[u] = Status.visited
            order.append(u)
            return True
    
        for i in range(n):
            if status[i] == Status.to_visit and not dfs(i):
                return None
    
        return order[::-1]
    ```

Độ phức tạp: $O(E+V)$, bộ nhớ $O(V)$

### Chứng minh hợp lý

Xét một đồ thị, nếu xóa một đỉnh bậc vào 0 mà đồ thị còn lại sắp xếp topo được, thì đồ thị ban đầu cũng sắp xếp topo được. Ngược lại, nếu đồ thị ban đầu sắp xếp topo được, thì xóa đi cũng được.

### Ứng dụng

Sắp xếp topo dùng để kiểm tra đồ thị có chu trình không, kiểm tra đồ thị có phải là một chuỗi không. Sắp xếp topo còn dùng để tìm đường găng trong AOE, ước lượng thời gian hoàn thành dự án.

### Tìm thứ tự topo lớn nhất/nhỏ nhất theo từ điển

Chỉ cần thay queue trong thuật toán Kahn bằng heap lớn nhất/nhỏ nhất (priority queue), tổng độ phức tạp $O(E+V \log{V})$.

## Bài tập

[CF 1385E](https://codeforces.com/problemset/problem/1385/E): Cần xây dựng bằng sắp xếp topo.

[Luogu P1347](https://www.luogu.com.cn/problem/P1347): Bài mẫu sắp xếp topo.

## Tham khảo

1.  Giáo trình Toán rời rạc và ứng dụng. ISBN:9787111555391
2.  [Topological sorting - Wikipedia](https://en.wikipedia.org/wiki/Topological_sorting)
3.  [数据结构第九讲（图：拓扑排序，关键路径，最短路径）- Zhihu](https://zhuanlan.zhihu.com/p/164751109)
