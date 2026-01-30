Cho một cây có gốc, trên một đỉnh của cây có một đồng xu. Tại mỗi thời điểm, đồng xu sẽ di chuyển ngẫu nhiên (xác suất đều) sang một đỉnh kề. Hỏi kỳ vọng quãng đường đồng xu di chuyển để đến một đỉnh kề.

## Các định nghĩa cần dùng

-   $T=(V,E)$: Cây đang xét
-   $d(u)$: Bậc của đỉnh $u$
-   $w(u,v)$: Trọng số cạnh giữa $u$ và $v$
-   $p_u$: Cha của đỉnh $u$
-   $\textit{root}$: Gốc của cây
-   $\textit{son}_u$: Tập các con của $u$
-   $\textit{sibling}_u$: Tập các anh em của $u$

## Kỳ vọng đi về phía cha

Gọi $f(u)$ là kỳ vọng quãng đường từ $u$ đi đến cha $p_u$, ta có:

$$
f(u) = \cfrac{w(u,p_u) + \sum\limits_{v \in \textit{son}_u}(w(u,v) + f(v) + f(u))}{d(u)}
$$

Tử số: phần đầu là đi thẳng về cha, phần sau là đi sang con rồi quay lại rồi mới về cha; mẫu số $d(u)$ là xác suất đều đến các đỉnh kề.

Rút gọn:

$$
\begin{aligned}
    f(u) &= \cfrac{w(u,p_u) + \sum\limits_{v \in \textit{son}_u}(w(u,v) + f(v) + f(u))}{d(u)} \\
         &= \cfrac{w(u,p_u) + \sum\limits_{v \in \textit{son}_u}(w(u,v) + f(v)) + (d(u)-1)f(u)}{d(u)} \\
         &= w(u,p_u) + \sum\limits_{v \in \textit{son}_u}(w(u,v) + f(v)) \\
         &= \sum\limits_{(u,t) \in E}w(u,t) + \sum\limits_{v \in \textit{son}_u}f(v)
\end{aligned}
$$

Với lá $l$, khởi tạo $f(l) = w(p_l, l)$.

Nếu mọi cạnh đều có trọng số $1$, công thức thành:

$$
f(u) = d(u) + \sum\limits_{v \in \textit{son}_u}f(v)
$$

Tức là tổng bậc các đỉnh trong cây con gốc $u$, cũng chính là $2 \times$ kích thước cây con $u$ $-1$ (mỗi cạnh đóng góp $2$ vào tổng bậc, trừ cạnh nối $u$ với cha chỉ đóng góp $1$).

## Kỳ vọng đi về phía con

Gọi $g(u)$ là kỳ vọng quãng đường từ cha $p_u$ đi đến con $u$, ta có:

$$
g(u) = \cfrac{w(p_u,u) + \left(w(p_u,p_{p_u})+g(p_u)+g(u)\right) + \sum\limits_{s \in \textit{sibling}_u}(w(p_u,s)+f(s)+g(u))}{d(p_u)}
$$

Tử số: phần đầu là đi thẳng đến $u$, phần hai là đi lên cha rồi quay lại rồi mới đến $u$, phần ba là đi sang anh em rồi quay lại rồi mới đến $u$; mẫu số $d(p_u)$ là xác suất đều đến các đỉnh kề.

Rút gọn:

$$
\begin{aligned}
    g(u) &= \cfrac{w(p_u,u) + \left(w(p_u,p_{p_u})+g(p_u)+g(u)\right) + \sum\limits_{s \in \textit{sibling}_u}(w(p_u,s)+f(s)+g(u))}{d(p_u)} \\
         &= \cfrac{w(p_u,u) + w(p_u,p_{p_u}) + g(p_u) + \sum\limits_{s \in \textit{sibling}_u}\left(w(p_u,s)+f(s)\right)+(d(p_u)-1)g(u)}{d(p_u)} \\
         &= w(p_u,u) + w(p_u,p_{p_u}) + g(p_u) + \sum\limits_{s \in \textit{sibling}_u}(w(p_u,s)+f(s)) \\
         &= \sum\limits_{(p_u,t) \in E}w(p_u,t) + g(p_u) + \sum\limits_{s \in \textit{sibling}_u}f(s) \\
         &= \sum\limits_{(p_u,t) \in E}w(p_u,t) + g(p_u) + \left(f(p_u)-\sum\limits_{(p_u,t) \in E}w(p_u,t)-f(u)\right) \\
         &= g(p_u) + f(p_u) - f(u)
\end{aligned}
$$

Khởi tạo $g(\text{root}) = 0$.

## Cài đặt (ví dụ với cây không trọng số)

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-random-walk.md
vector<int> G[MAXN];

void dfs1(int u, int p) {
  f[u] = G[u].size();
  for (auto v : G[u]) {
    if (v == p) continue;
    dfs1(v, u);
    f[u] += f[v];
  }
}

void dfs2(int u, int p) {
  if (u != root) g[u] = g[p] + f[p] - f[u];
  for (auto v : G[u]) {
    if (v == p) continue;
    dfs2(v, u);
  }
}
```
