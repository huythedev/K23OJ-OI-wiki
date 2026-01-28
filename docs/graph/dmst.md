## Định nghĩa

Cây khung nhỏ nhất trên đồ thị có hướng (Directed Minimum Spanning Tree) được gọi là **cây khung có hướng nhỏ nhất** (hay "Minimum Arborescence").

Thuật toán thường dùng là thuật toán Chu-Liu/Edmonds, có thể giải bài toán cây khung có hướng nhỏ nhất trong thời gian $O(nm)$.

## Quy trình

1.  Với mỗi đỉnh, chọn cạnh có trọng số nhỏ nhất đi vào nó.
2.  Nếu không có chu trình, thuật toán kết thúc; nếu có chu trình thì tiến hành thu gọn chu trình và cập nhật lại khoảng cách từ các đỉnh khác tới chu trình.

## Cài đặt

```cpp
bool solve() {
  ans = 0;
  int u, v, root = 0;
  for (;;) {
    f(i, 0, n) in[i] = 1e100;
    f(i, 0, m) {
      u = e[i].s;
      v = e[i].t;
      if (u != v && e[i].w < in[v]) {
        in[v] = e[i].w;
        pre[v] = u;
      }
    }
    f(i, 0, m) if (i != root && in[i] > 1e50) return 0;
    int tn = 0;
    memset(id, -1, sizeof id);
    memset(vis, -1, sizeof vis);
    in[root] = 0;
    f(i, 0, n) {
      ans += in[i];
      v = i;
      while (vis[v] != i && id[v] == -1 && v != root) {
        vis[v] = i;
        v = pre[v];
      }
      if (v != root && id[v] == -1) {
        for (int u = pre[v]; u != v; u = pre[u]) id[u] = tn;
        id[v] = tn++;
      }
    }
    if (tn == 0) break;
    f(i, 0, n) if (id[i] == -1) id[i] = tn++;
    f(i, 0, m) {
      u = e[i].s;
      v = e[i].t;
      e[i].s = id[u];
      e[i].t = id[v];
      if (e[i].s != e[i].t) e[i].w -= in[v];
    }
    n = tn;
    root = id[root];
  }
  return ans;
}
```

## Thuật toán DMST của Tarjan

Tarjan đã đề xuất một thuật toán có thể giải bài toán cây khung có hướng nhỏ nhất trong thời gian $O(m+n\log n)$.

Phần mô tả thuật toán và mã tham khảo dưới đây dựa trên bài giảng của Giáo sư Uri Zwick, bạn có thể tham khảo chi tiết trong tài liệu gốc.

### Quy trình

Thuật toán Tarjan gồm hai bước: **thu gọn** và **mở rộng**. Trước tiên, ta sẽ trình bày quá trình **thu gọn**.

Giả sử đồ thị đầu vào là đồ thị mạnh liên thông; nếu không, hãy thêm $O(n)$ cạnh với trọng số vô cùng lớn để đảm bảo điều kiện này.

Ta cần một cấu trúc heap để lưu thông tin về các cạnh đi vào mỗi đỉnh: chỉ số cạnh, trọng số cạnh, tổng chi phí của đỉnh,... Do quá trình sau sẽ có thao tác trộn heap, nên sử dụng [cây trái lệch (Leftist Tree)](../ds/leftist-tree.md) và [DSU (Disjoint Set Union)](../ds/dsu.md). Mỗi bước, chọn một đỉnh $v$ bất kỳ (không phải gốc, và heap của nó không rỗng), thêm cạnh nhỏ nhất đi vào $v$ vào heap. Nếu cạnh mới thêm tạo thành chu trình, thu gọn các đỉnh trong chu trình thành một "siêu đỉnh", tiếp tục quá trình này. Khi tất cả các đỉnh đã được thu gọn thành một siêu đỉnh, quá trình thu gọn kết thúc và ta thu được một cây thu gọn, sau đó tiến hành mở rộng.

Các cạnh trong heap luôn tạo thành một đường đi $v_0\leftarrow v_1\leftarrow \dots\leftarrow v_k$, do đồ thị mạnh liên thông nên đường đi này luôn tồn tại, và mỗi $v_i$ có thể là đỉnh ban đầu hoặc siêu đỉnh sau khi thu gọn.

Ban đầu $v_0=a$, với $a$ là một đỉnh bất kỳ. Mỗi lần chọn cạnh nhỏ nhất đi vào $v_k$ từ $u$, nếu $u$ không thuộc $v_0,v_1,\dots,v_k$ thì mở rộng đường đi tới $v_{k+1}=u$. Nếu $u$ thuộc tập đó, đã tìm được chu trình $v_i\leftarrow\dots\leftarrow v_k\leftarrow v_i$, thu gọn thành siêu đỉnh $c$.

Đưa tất cả các đỉnh hoặc siêu đỉnh vào hàng đợi $P$, chọn một đỉnh bất kỳ $a$ làm điểm bắt đầu, khi hàng đợi còn phần tử thì thực hiện:

1.  Chọn cạnh nhỏ nhất đi vào $a$, đảm bảo không phải cạnh tự nối, tìm đỉnh đầu còn lại là $b$. Nếu $b$ chưa được ghi nhận, tức là chưa tạo chu trình, gán $a\leftarrow b$ và tiếp tục tìm chu trình.
2.  Nếu $b$ đã được ghi nhận, tức là đã tạo chu trình. Tăng số lượng đỉnh, đánh số lại các đỉnh trong chu trình, trộn heap, cập nhật tổng chi phí của các đỉnh/siêu đỉnh. Việc cập nhật chi phí là gom tất cả các cạnh đi vào chu trình và trừ đi trọng số các cạnh trong chu trình.

![dmst1](./images/dmst1.png)

Ví dụ: đồ thị mạnh liên thông bên trái sau khi thu gọn sẽ thành cây thu gọn bên phải, trong đó $a$ là siêu đỉnh gồm đỉnh 1 và 2, $b$ là siêu đỉnh gồm đỉnh 3, 4, 5, $A$ là siêu đỉnh gồm $a$ và $b$.

Quá trình mở rộng khá đơn giản: bắt đầu từ đỉnh gốc $r$, mở rộng các chu trình trên đường từ $r$ tới gốc của cây thu gọn. Sau đó, bắt đầu từ tổ tiên của $r$ là $f_r$, tiếp tục mở rộng các chu trình trên đường tới gốc, cho tới khi duyệt hết tất cả các đỉnh.

### Cài đặt

```cpp
#include <cstdio>
#include <cstring>
#include <queue>
#include <vector>
using namespace std;

using ll = long long;
constexpr int MAXN = 102;
constexpr int INF = 0x3f3f3f3f;

struct UnionFind {
  int fa[MAXN << 1];

  UnionFind() { memset(fa, 0, sizeof(fa)); }

  void clear(int n) { memset(fa + 1, 0, sizeof(int) * n); }

  int find(int x) { return fa[x] ? fa[x] = find(fa[x]) : x; }

  int operator[](int x) { return find(x); }
};

struct Edge {
  int u, v, w, w0;
};

struct Heap {
  Edge *e;
  int rk, constant;
  Heap *lch, *rch;

  Heap(Edge *_e) : e(_e), rk(1), constant(0), lch(NULL), rch(NULL) {}

  void push() {
    if (lch) lch->constant += constant;
    if (rch) rch->constant += constant;
    e->w += constant;
    constant = 0;
  }
};

Heap *merge(Heap *x, Heap *y) {
  if (!x) return y;
  if (!y) return x;
  if (x->e->w + x->constant > y->e->w + y->constant) swap(x, y);
  x->push();
  x->rch = merge(x->rch, y);
  if (!x->lch || x->lch->rk < x->rch->rk) swap(x->lch, x->rch);
  if (x->rch)
    x->rk = x->rch->rk + 1;
  else
    x->rk = 1;
  return x;
}

Edge *extract(Heap *&x) {
  Edge *r = x->e;
  x->push();
  x = merge(x->lch, x->rch);
  return r;
}

vector<Edge> in[MAXN];
int n, m, fa[MAXN << 1], nxt[MAXN << 1];
Edge *ed[MAXN << 1];
Heap *Q[MAXN << 1];
UnionFind id;

void contract() {
  bool mark[MAXN << 1];
  // 将图上的每一个结点与其相连的那些结点进行记录．
  for (int i = 1; i <= n; i++) {
    queue<Heap *> q;
    for (int j = 0; j < in[i].size(); j++) q.push(new Heap(&in[i][j]));
    while (q.size() > 1) {
      Heap *u = q.front();
      q.pop();
      Heap *v = q.front();
      q.pop();
      q.push(merge(u, v));
    }
    Q[i] = q.front();
  }
  mark[1] = true;
  for (int a = 1, b = 1, p; Q[a]; b = a, mark[b] = true) {
    // 寻找最小入边以及其端点，保证无环．
    do {
      ed[a] = extract(Q[a]);
      a = id[ed[a]->u];
    } while (a == b && Q[a]);
    if (a == b) break;
    if (!mark[a]) continue;
    // 对发现的环进行收缩，以及环内的结点重新编号，总权值更新．
    for (a = b, n++; a != n; a = p) {
      id.fa[a] = fa[a] = n;
      if (Q[a]) Q[a]->constant -= ed[a]->w;
      Q[n] = merge(Q[n], Q[a]);
      p = id[ed[a]->u];
      nxt[p == n ? b : p] = a;
    }
  }
}

ll expand(int x, int r);

ll expand_iter(int x) {
  ll r = 0;
  for (int u = nxt[x]; u != x; u = nxt[u]) {
    if (ed[u]->w0 >= INF)
      return INF;
    else
      r += expand(ed[u]->v, u) + ed[u]->w0;
  }
  return r;
}

ll expand(int x, int t) {
  ll r = 0;
  for (; x != t; x = fa[x]) {
    r += expand_iter(x);
    if (r >= INF) return INF;
  }
  return r;
}

void link(int u, int v, int w) { in[v].push_back({u, v, w, w}); }

int main() {
  int rt;
  scanf("%d %d %d", &n, &m, &rt);
  for (int i = 0; i < m; i++) {
    int u, v, w;
    scanf("%d %d %d", &u, &v, &w);
    link(u, v, w);
  }
  // 保证强连通
  for (int i = 1; i <= n; i++) link(i > 1 ? i - 1 : n, i, INF);
  contract();
  ll ans = expand(rt, n);
  if (ans >= INF)
    puts("-1");
  else
    printf("%lld\n", ans);
  return 0;
}
```

## Tài liệu tham khảo

Uri Zwick. (2013), [Directed Minimum Spanning Trees](http://www.cs.tau.ac.il/~zwick/grad-algo-13/directed-mst.pdf), Lecture notes on "Analysis of Algorithms"

<https://riteme.site/blog/2018-6-18/mdst.html#_3>
