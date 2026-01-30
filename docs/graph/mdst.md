Trước khi học về cây khung có đường kính nhỏ nhất (Minimum Diameter Spanning Tree), bạn nên đọc trước phần [Đường kính của cây](./tree-diameter.md).

## Định nghĩa

Trong tất cả các cây khung của một đồ thị vô hướng, cây có đường kính nhỏ nhất được gọi là cây khung có đường kính nhỏ nhất.

## Trung tâm tuyệt đối của đồ thị

Để giải bài toán cây khung có đường kính nhỏ nhất, trước tiên cần xác định **trung tâm tuyệt đối của đồ thị**. **Trung tâm tuyệt đối** có thể nằm trên một cạnh hoặc tại một đỉnh, là điểm mà khoảng cách lớn nhất từ nó đến mọi đỉnh khác là nhỏ nhất.

Theo định nghĩa, sẽ luôn có ít nhất hai đỉnh ở xa trung tâm tuyệt đối nhất.

Gọi $d(i,j)$ là độ dài đường đi ngắn nhất giữa hai đỉnh $i$ và $j$, có thể tính bằng thuật toán đa nguồn ngắn nhất.

$\textit{rk}(i,j)$ lưu đỉnh xa thứ $j$ từ $i$.

Trung tâm tuyệt đối có thể nằm trên một cạnh, xét từng cạnh $w=(u,v)$, giả sử trung tâm tuyệt đối $c$ nằm trên cạnh này. Khi đó, khoảng cách từ $c$ đến $u$ là $x$ ($x \leq w$), đến $v$ là $w-x$.

Với mỗi đỉnh $i$, khoảng cách từ $c$ đến $i$ là $d(c,i)=\min(d(u,i) + x, d(v,i) + (w - x))$.

Ví dụ với một đỉnh $i$, vị trí tương đối với trung tâm tuyệt đối như hình dưới:

![mdst1](./images/mdst-graph.svg)

Khi trung tâm tuyệt đối $c$ di chuyển trên cạnh, ta được một hàm biểu diễn khoảng cách theo vị trí $c$. Rõ ràng, $d(c,i)$ là một đoạn gấp khúc gồm hai đoạn thẳng có cùng độ dốc.

![mdst2](./images/mdst-plot1.svg)

Với mọi đỉnh, hàm khoảng cách lớn nhất từ trung tâm tuyệt đối đến các đỉnh là $f = \max\{ d(c,i)\},i \in[1,n]$, đồ thị hàm như sau:

![mdst3](./images/mdst-plot2.svg)

Điểm thấp nhất trong các giao điểm của các đoạn gấp khúc này (theo trục hoành) chính là vị trí trung tâm tuyệt đối.

Trung tâm tuyệt đối cũng có thể nằm tại một đỉnh, khi đó chỉ cần lấy khoảng cách xa nhất từ đỉnh đó đến các đỉnh khác, tức $\textit{ans}\leftarrow \min(\textit{ans},d(i,\textit{rk}(i,n))\times 2)$.

### Quy trình

1.  Dùng thuật toán đa nguồn ngắn nhất ([Floyd](./shortest-path.md#floyd-算法), [Johnson](./shortest-path.md#johnson-全源最短路径算法), ...) để tính mảng $d$;

2.  Tính $\textit{rk}(i,j)$, sắp xếp tăng dần theo khoảng cách;

3.  Trung tâm tuyệt đối có thể nằm tại một đỉnh, với mỗi đỉnh cập nhật $\textit{ans}\leftarrow \min(\textit{ans},d(i,\textit{rk}(i,n)) \times 2)$;

4.  Trung tâm tuyệt đối có thể nằm trên một cạnh, duyệt từng cạnh $w(u,v)$, bắt đầu từ đỉnh xa nhất của $u$. Khi gặp $d(v,\textit{rk}(u,i)) > \max_{j=i+1}^n d(v,\textit{rk}(u,j))$, cập nhật $\textit{ans}\leftarrow  \min(\textit{ans}, d(u,\textit{rk}(u,i))+\max_{j=i+1}^n d(v,\textit{rk}(u,j))+w(u,v))$ vì lúc này trung tâm tuyệt đối thay đổi vị trí.

??? note "Cài đặt"
    ```cpp
    bool cmp(int a, int b) { return val[a] < val[b]; }
    
    void Floyd() {
      for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
          for (int j = 1; j <= n; j++) d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
    }
    
    void solve() {
      Floyd();
      for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
          rk[i][j] = j;
          val[j] = d[i][j];
        }
        sort(rk[i] + 1, rk[i] + 1 + n, cmp);
      }
      int ans = INF;
      // Trung tâm tuyệt đối có thể nằm tại một đỉnh
      for (int i = 1; i <= n; i++) ans = min(ans, d[i][rk[i][n]] * 2);
      // Trung tâm tuyệt đối có thể nằm trên một cạnh
      for (int i = 1; i <= m; i++) {
        int u = a[i].u, v = a[i].v, w = a[i].w;
        for (int p = n, i = n - 1; i >= 1; i--) {
          if (d[v][rk[u][i]] > d[v][rk[u][p]]) {
            ans = min(ans, d[u][rk[u][i]] + d[v][rk[u][p]] + w);
            p = i;
          }
        }
      }
    }
    ```

### Bài tập ví dụ

-   [CodeForce 266D BerDonalds](https://codeforces.com/contest/266/problem/D)

## Cây khung có đường kính nhỏ nhất

Theo định nghĩa, trung tâm tuyệt đối của đồ thị chính là trung điểm của đường kính cây khung có đường kính nhỏ nhất.

Để tìm cây khung có đường kính nhỏ nhất, trước hết cần xác định trung tâm tuyệt đối. Từ trung tâm tuyệt đối, xây dựng một cây đường đi ngắn nhất, ta sẽ thu được cây khung có đường kính nhỏ nhất.

??? note "Cài đặt"
    ```cpp
    #include <algorithm>
    #include <climits>
    #include <iostream>
    #include <vector>
    using namespace std;
    constexpr int MAXN = 502;
    using ll = long long;
    using pii = pair<int, int>;
    ll d[MAXN][MAXN], dd[MAXN][MAXN], rk[MAXN][MAXN], val[MAXN];
    constexpr ll INF = 1e17;
    int n, m;
    
    bool cmp(int a, int b) { return val[a] < val[b]; }
    
    void floyd() {
      for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
          for (int j = 1; j <= n; j++) d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
    }
    
    struct node {
      ll u, v, w;
    } a[MAXN * (MAXN - 1) / 2];
    
    void solve() {
      // 求图的绝对中心
      floyd();
      for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
          rk[i][j] = j;
          val[j] = d[i][j];
        }
        sort(rk[i] + 1, rk[i] + 1 + n, cmp);
      }
      ll P = 0, ansP = INF;
      // 在点上
      for (int i = 1; i <= n; i++) {
        if (d[i][rk[i][n]] * 2 < ansP) {
          ansP = d[i][rk[i][n]] * 2;
          P = i;
        }
      }
      // 在边上
      int f1 = 0, f2 = 0;
      ll disu = INT_MIN, disv = INT_MIN, ansL = INF;
      for (int i = 1; i <= m; i++) {
        ll u = a[i].u, v = a[i].v, w = a[i].w;
        for (int p = n, i = n - 1; i >= 1; i--) {
          if (d[v][rk[u][i]] > d[v][rk[u][p]]) {
            if (d[u][rk[u][i]] + d[v][rk[u][p]] + w < ansL) {
              ansL = d[u][rk[u][i]] + d[v][rk[u][p]] + w;
              f1 = u, f2 = v;
              disu = (d[u][rk[u][i]] + d[v][rk[u][p]] + w) / 2 - d[u][rk[u][i]];
              disv = w - disu;
            }
            p = i;
          }
        }
      }
      cout << min(ansP, ansL) / 2 << '\n';
      // 最小路径生成树
      vector<pii> pp;
      for (int i = 1; i <= 501; ++i)
        for (int j = 1; j <= 501; ++j) dd[i][j] = INF;
      for (int i = 1; i <= 501; ++i) dd[i][i] = 0;
      if (ansP <= ansL) {
        for (int j = 1; j <= n; j++) {
          for (int i = 1; i <= m; ++i) {
            ll u = a[i].u, v = a[i].v, w = a[i].w;
            if (dd[P][u] + w == d[P][v] && dd[P][u] + w < dd[P][v]) {
              dd[P][v] = dd[P][u] + w;
              pp.push_back({u, v});
            }
            u = a[i].v, v = a[i].u, w = a[i].w;
            if (dd[P][u] + w == d[P][v] && dd[P][u] + w < dd[P][v]) {
              dd[P][v] = dd[P][u] + w;
              pp.push_back({u, v});
            }
          }
        }
        for (auto [x, y] : pp) cout << x << ' ' << y << '\n';
      } else {
        d[n + 1][f1] = disu;
        d[f1][n + 1] = disu;
        d[n + 1][f2] = disv;
        d[f2][n + 1] = disv;
        a[m + 1].u = n + 1, a[m + 1].v = f1, a[m + 1].w = disu;
        a[m + 2].u = n + 1, a[m + 2].v = f2, a[m + 2].w = disv;
        n += 1;
        m += 2;
        floyd();
        P = n;
        for (int j = 1; j <= n; j++) {
          for (int i = 1; i <= m; ++i) {
            ll u = a[i].u, v = a[i].v, w = a[i].w;
            if (dd[P][u] + w == d[P][v] && dd[P][u] + w < dd[P][v]) {
              dd[P][v] = dd[P][u] + w;
              pp.push_back({u, v});
            }
            u = a[i].v, v = a[i].u, w = a[i].w;
            if (dd[P][u] + w == d[P][v] && dd[P][u] + w < dd[P][v]) {
              dd[P][v] = dd[P][u] + w;
              pp.push_back({u, v});
            }
          }
        }
        cout << f1 << ' ' << f2 << '\n';
        for (auto [x, y] : pp)
          if (x != n && y != n) cout << x << ' ' << y << '\n';
      }
    }
    
    void init() {
      for (int i = 1; i <= 501; ++i)
        for (int j = 1; j <= 501; ++j) d[i][j] = INF;
      for (int i = 1; i <= 501; ++i) d[i][i] = 0;
    }
    
    int main() {
      init();
      cin >> n >> m;
      for (int i = 1; i <= m; ++i) {
        ll u, v, w;
        cin >> u >> v >> w;
        w *= 2;
        d[u][v] = w, d[v][u] = w;
        a[i].u = u, a[i].v = v, a[i].w = w;
      }
      solve();
      return 0;
    }
    ```

### Bài tập ví dụ

[SPOJ MDST](https://www.spoj.com/problems/MDST/)

[timus 1569. Networking the "Iset"](https://acm.timus.ru/problem.aspx?space=1&num=1569)

[SPOJ PT07C - The GbAaY Kingdom](https://www.spoj.com/problems/PT07C)

## Tài liệu tham khảo

[Play with Trees Solutions The GbAaY Kingdom](https://adn.botao.hu/adn-backup/blog/attachments/month_0705/32007531153238.pdf)
