## Định nghĩa

Đồ thị có cạnh có hướng, không chứa chu trình.

Tên tiếng Anh là Directed Acyclic Graph, viết tắt là DAG.

## Tính chất

-   Đồ thị có thể [sắp xếp tôpô](./topo.md) chắc chắn là đồ thị có hướng không chu trình;

    Nếu tồn tại chu trình, thì với bất kỳ hai đỉnh trên chu trình đó đều không thể thỏa mãn điều kiện trong bất kỳ thứ tự nào.

-   Đồ thị có hướng không chu trình chắc chắn có thể sắp xếp tôpô;

    (Chứng minh quy nạp) Giả sử với đồ thị có số đỉnh không vượt quá $k$ đều có thể sắp xếp tôpô, thì với đồ thị có $k$ đỉnh, chỉ cần xét trường hợp sau khi thực hiện bước đầu tiên của sắp xếp tôpô.

## Cách nhận biết

Làm thế nào để kiểm tra một đồ thị có phải là đồ thị có hướng không chu trình?

Chỉ cần kiểm tra xem nó có thể [sắp xếp tôpô](./topo.md) hay không.

Ngoài ra còn có cách khác, có thể thực hiện một lượt [DFS](../search/dfs.md) trên đồ thị, sau đó kiểm tra trên cây DFS xem có tồn tại cạnh không thuộc cây nhưng nối tới tổ tiên (cạnh ngược) hay không. Nếu có thì đồ thị có chu trình.

## Ứng dụng

### Quy hoạch động (DP) tìm đường đi dài nhất (ngắn nhất)

Trên đồ thị tổng quát, độ phức tạp tối ưu để tìm đường đi dài nhất (ngắn nhất) từ một đỉnh là $O(nm)$ ([Thuật toán Bellman–Ford](./shortest-path.md#bellmanford-算法), áp dụng cho đồ thị có cạnh âm) hoặc $O(m \log m)$ ([Thuật toán Dijkstra](./shortest-path.md#dijkstra-算法), áp dụng cho đồ thị không có cạnh âm).

Nhưng trên DAG, ta có thể dùng quy hoạch động (DP) để tìm đường đi dài nhất (ngắn nhất), giúp giảm độ phức tạp xuống $O(n+m)$. Phương trình chuyển trạng thái là $dis_v = min(dis_v, dis_u + w_{u,v})$ hoặc $dis_v = max(dis_v, dis_u + w_{u,v})$.

Sau khi sắp xếp tôpô, duyệt các đỉnh theo thứ tự tôpô, dùng giá trị tại đỉnh hiện tại để cập nhật các đỉnh phía sau.

```cpp
struct edge {
  int v, w;
};

int n, m;
vector<edge> e[MAXN];
vector<int> L;                               // Lưu kết quả sắp xếp tôpô
int max_dis[MAXN], min_dis[MAXN], in[MAXN];  // in lưu bậc vào của mỗi đỉnh

void toposort() {  // Sắp xếp tôpô
  queue<int> S;
  memset(in, 0, sizeof(in));
  for (int i = 1; i <= n; i++) {
    for (int j = 0; j < e[i].size(); j++) {
      in[e[i][j].v]++;
    }
  }
  for (int i = 1; i <= n; i++)
    if (in[i] == 0) S.push(i);
  while (!S.empty()) {
    int u = S.front();
    S.pop();
    L.push_back(u);
    for (int i = 0; i < e[u].size(); i++) {
      if (--in[e[u][i].v] == 0) {
        S.push(e[u][i].v);
      }
    }
  }
}

void dp(int s) {  // Tìm đường đi dài nhất (ngắn nhất) từ đỉnh s
  toposort();     // Sắp xếp tôpô trước
  memset(min_dis, 0x3f, sizeof(min_dis));
  memset(max_dis, 0, sizeof(max_dis));
  min_dis[s] = 0;
  for (int i = 0; i < L.size(); i++) {
    int u = L[i];
    for (int j = 0; j < e[u].size(); j++) {
      min_dis[e[u][j].v] = min(min_dis[e[u][j].v], min_dis[u] + e[u][j].w);
      max_dis[e[u][j].v] = max(max_dis[e[u][j].v], max_dis[u] + e[u][j].w);
    }
  }
}
```

Tham khảo: [DP trên DAG](../dp/dag.md).
