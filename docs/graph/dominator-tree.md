## Mở đầu

Năm 1959, khái niệm "chi phối" (dominator) được Reese T. Prosser đề xuất trong [một bài báo về mạng dòng chảy](http://portal.acm.org/ft_gateway.cfm?id=1460314&type=pdf&coll=GUIDE&dl=GUIDE&CFID=79528182&CFTOKEN=33765747), nhưng chưa có thuật toán giải cụ thể; đến năm 1969, Edward S. Lowry và C. W. Medlock mới lần đầu tiên đưa ra [thuật toán hiệu quả](http://portal.acm.org/ft_gateway.cfm?id=362838&type=pdf&coll=GUIDE&dl=GUIDE&CFID=79528182&CFTOKEN=33765747). Thuật toán phổ biến nhất hiện nay là Lengauer–Tarjan, được Lengauer và Tarjan công bố năm 1979 trong [một bài báo](https://www.cs.princeton.edu/courses/archive/fall03/cs528/handouts/a%20fast%20algorithm%20for%20finding.pdf).

Trong cộng đồng OI, khái niệm cây chi phối xuất hiện lần đầu ở bài [ZJOI2012 Thảm họa](https://www.luogu.com.cn/problem/P2597), khi đó còn gọi là "cây tuyệt diệt"; Chen Sunli cũng giới thiệu thuật toán này trong luận văn đội tuyển quốc gia năm 2020.

Hiện tại, cây chi phối chưa phổ biến trong các kỳ thi lập trình, số lượng bài tập liên quan còn ít; tuy nhiên, cây chi phối lại rất quan trọng trong công nghiệp, đặc biệt là lĩnh vực biên dịch.

Bài viết này sẽ giới thiệu khái niệm cây chi phối và các phương pháp giải.

## Quan hệ chi phối

Trên một đồ thị có hướng, giả sử chọn một đỉnh vào $s$. Với một đỉnh $u$, nếu mọi đường đi từ $s$ đến $u$ đều phải đi qua một đỉnh $v$, ta nói $v$ **chi phối** $u$, hay $v$ là một **điểm chi phối** của $u$, ký hiệu $v\ dom\ u$.

Với các đỉnh không thể đi từ $s$ tới, việc xét quan hệ chi phối là vô nghĩa, do đó mặc định $s$ có thể tới mọi đỉnh trên đồ thị.

![](images/dom-tree1.png)

Ví dụ, trên đồ thị này: $2$ bị $1$ chi phối, $3$ bị $1, 2$ chi phối, $4$ bị $1, 2, 3$ chi phối, $5$ bị $1, 2$ chi phối, v.v.

### Các bổ đề

Trong các bổ đề dưới đây, mặc định $u, v, w\ne s$

**Bổ đề 1:** $s$ là điểm chi phối của mọi đỉnh; mỗi đỉnh là điểm chi phối của chính nó.

**Chứng minh:** Rõ ràng mọi đường đi từ $s$ tới $u$ đều phải qua $s$ và $u$.

**Bổ đề 2:** Quan hệ chi phối xét trên đường đi đơn giản hay trên mọi đường đi đều giống nhau.

**Chứng minh:** Với đường đi không đơn giản, giữa hai lần qua một đỉnh, tập các đỉnh đi qua là $S$. Nếu loại bỏ các đỉnh trong $S$, mỗi đường đi không đơn giản sẽ tương ứng với một đường đi đơn giản. Các đỉnh chỉ xuất hiện trên đường đi không đơn giản sẽ không thể là điểm chi phối, vì tồn tại đường đi đơn giản không qua đỉnh đó; các đỉnh xuất hiện trên cả hai loại đường đi chỉ cần xét trên đường đi đơn giản. Do đó, loại bỏ đường đi không đơn giản không ảnh hưởng tới quan hệ chi phối.

**Bổ đề 3:** Nếu $u$ chi phối $v$, $v$ chi phối $w$, thì $u$ chi phối $w$.

**Chứng minh:** Đường đi tới $w$ phải qua $v$, đường đi tới $v$ phải qua $u$, nên đường đi tới $w$ phải qua $u$.

**Bổ đề 4:** Nếu $u$ chi phối $v$, $v$ chi phối $u$, thì $u=v$.

**Chứng minh:** Giả sử $u \ne v$, mọi đường đi tới $v$ đều đã qua $u$, mọi đường đi tới $u$ đều đã qua $v$, mâu thuẫn.

**Bổ đề 5:** Nếu $u \ne v \ne w$, $u$ chi phối $w$ và $v$ chi phối $w$, thì $u$ chi phối $v$ hoặc $v$ chi phối $u$.

**Chứng minh:** Xét đường đi $s \rightarrow \dots \rightarrow u \rightarrow \dots \rightarrow v \rightarrow \dots \rightarrow w$, nếu $u, v$ không có quan hệ chi phối, tồn tại đường đi từ $s$ tới $v$ không qua $u$, tức tồn tại đường đi $s \rightarrow \dots \rightarrow v \rightarrow \dots \rightarrow w$, mâu thuẫn với $u\ dom\ w$.

### Cách xác định quan hệ chi phối

#### Phương pháp xóa đỉnh

Một kết luận tương đương với định nghĩa: Nếu xóa một đỉnh khỏi đồ thị làm một số đỉnh không còn tới được, thì đỉnh bị xóa chi phối các đỉnh đó.

Chỉ cần thử xóa từng đỉnh rồi chạy dfs, độ phức tạp $O(n^3)$. Dưới đây là mã lõi.

```cpp
// Giả sử đồ thị có n đỉnh, điểm bắt đầu s = 1
std::bitset<N> vis;
std::vector<int> edge[N];
std::vector<int> dom[N];

void dfs(int u, int del) {
  vis[u] = true;
  for (int v : edge[u]) {
    if (v == del or vis[v]) {
      continue;
    }
    dfs(v, del);
  }
}

void getdom() {
  for (int i = 2; i <= n; ++i) {
    vis.reset();
    dfs(1, i);
    for (int j = 1; j <= n; ++j) {
      if (!vis[j]) {
        dom[j].push_back(i);
      }
    }
  }
}
```

#### Phương pháp lặp dữ liệu dòng

Phương pháp lặp dữ liệu dòng (data-flow iteration) cũng ít gặp trong OI, dưới đây là giới thiệu ngắn gọn.

Phân tích dữ liệu dòng là khái niệm trong lý thuyết biên dịch, dùng để phân tích cách dữ liệu di chuyển trên các đường đi của chương trình; phương pháp lặp dữ liệu dòng là liệt kê phương trình tại các đỉnh trên đồ thị luồng chương trình và lặp lại cho tới khi nghiệm ổn định. Ở đây, ta coi đồ thị có hướng như đồ thị luồng chương trình.

Phương trình ở đây là:

$$
dom(u)=\{u\} \cup \left(\bigcap_{v\in pre(u)}{dom(v)}\right)
$$

Trong đó $pre(u)$ là tập các đỉnh tiền nhiệm của $u$. Phương trình này có thể rút ra từ bổ đề 3.

Nói cách khác, tập điểm chi phối của một đỉnh là giao của tập điểm chi phối của các tiền nhiệm, hợp với chính nó. Lặp lại phương trình này cho tới khi ổn định.

Để tăng hiệu quả, nên duyệt các đỉnh theo thứ tự nghịch hậu DFS, tức là các tiền nhiệm đã được cập nhật xong trước khi cập nhật đỉnh hiện tại.

Dưới đây là mã tham khảo. Cần xử lý trước tập tiền nhiệm và thứ tự nghịch hậu DFS, không trình bày chi tiết ở đây.

```cpp
std::vector<int> pre[N];  // Tập tiền nhiệm của mỗi đỉnh
std::vector<int> ord;     // Thứ tự nghịch hậu DFS
std::bitset<N> dom[N];
std::vector<int> Dom[N];

void getdom() {
  dom[1][1] = true;
  flag = true;
  while (flag) {
    flag = false;
    for (int u : ord) {
      std::bitset<N> tmp;
      tmp[u] = true;
      for (int v : pre[u]) {
        tmp &= dom[v];
      }
      if (tmp != dom[u]) {
        dom[u] = tmp;
        flag = true;
      }
    }
  }
  for (int i = 2; i <= n; ++i) {
    for (int j = 1; j <= n; ++j) {
      if (dom[i][j]) {
        Dom[i].push_back(j);
      }
    }
  }
}
```

Thuật toán trên có độ phức tạp $O(n^2)$.

## Cây chi phối

Ta thấy rằng, ngoài $s$, mỗi đỉnh có ít nhất hai điểm chi phối: $s$ và chính nó.

Với mỗi đỉnh $u$, điểm chi phối gần nhất (không tính chính nó) gọi là điểm chi phối trực tiếp, ký hiệu $idom(u) = v$. Rõ ràng ngoài $s$, mỗi đỉnh đều có duy nhất một điểm chi phối trực tiếp.

Với mỗi đỉnh $u \ne s$, nối cạnh từ $idom(u)$ tới $u$, ta được một đồ thị có $n$ đỉnh, $n-1$ cạnh, không có chu trình (theo bổ đề 3 và 4), tức là một cây. Ta gọi cây này là **cây chi phối** của đồ thị gốc.

## Cách xác định cây chi phối

### Dựa vào tập dom

Xét tập điểm chi phối của một đỉnh $\{s_1, s_2, \dots, s_k\}$, sẽ tồn tại đường đi $s \rightarrow \dots \rightarrow s_1 \rightarrow \dots \rightarrow s_2 \rightarrow \dots \rightarrow \dots \rightarrow s_k \rightarrow\dots \rightarrow u$. Rõ ràng điểm chi phối trực tiếp của $u$ là $s_k$. Định nghĩa tương đương:

Với tập điểm chi phối $S$ của $u$, nếu $v \in S$ thỏa mãn $\forall w \in S\setminus\{u,v\}, w\ dom \ v$, thì $idom(u)=v$.

Dựa vào thuật toán trên, sau khi có tập điểm chi phối của mỗi đỉnh, chỉ cần áp dụng định nghĩa trên để tìm điểm chi phối trực tiếp, từ đó xây dựng cây chi phối. Dưới đây là mã tham khảo.

```cpp
std::bitset<N> dom[N];
std::vector<int> Dom[N];
int idom[N];

void getidom() {
  for (int u = 2; u <= n; ++u) {
    for (int v : Dom[u]) {
      std::bitset<N> tmp = (dom[v] & dom[u]) ^ dom[u];
      if (tmp.count() == 1 and tmp[u]) {
        idom[u] = v;
        break;
      }
    }
  }
  for (int u = 2; u <= n; ++u) {
    e[idom[u]].push_back(u);
  }
}
```

### Trường hợp đặc biệt trên cây

Rõ ràng cây chi phối của đồ thị dạng cây chính là bản thân nó.

### Trường hợp đặc biệt trên DAG

DAG có một tính chất tốt: khi duyệt theo thứ tự tôpô, nghiệm đã tìm được sẽ không ảnh hưởng tới các nghiệm sau. Có thể tận dụng tính chất này để xây dựng cây chi phối nhanh.

???+ warning "Lưu ý"
    Lưu ý DAG ở đây chỉ có một đỉnh xuất phát. Nếu có nhiều đỉnh xuất phát, các đỉnh bị chi phối bởi nhiều đỉnh gốc sẽ có nhiều cha trong cây chi phối, khi đó không thể biểu diễn quan hệ chi phối bằng cây đơn giản.

**Bổ đề 6:** Trên đồ thị có hướng, $v\ dom\ u$ khi và chỉ khi $\forall w \in pre(u), v\ dom \ w$.

**Chứng minh:** Đầu tiên chứng minh đủ. Mọi đường đi từ $s$ tới $u$ đều phải qua một đỉnh $w \in pre(u)$, mà $v$ chi phối $w$, nên mọi đường đi từ $s$ tới $u$ đều phải qua $v$, tức $v \ dom \ u$.

Chứng minh cần. Nếu tồn tại $w\in pre(u)$ mà $v$ không chi phối $w$, thì tồn tại đường đi từ $s$ tới $w$ không qua $v$, tức tồn tại đường đi từ $s$ tới $u$ không qua $v$, nên $v$ không chi phối $u$.

Như vậy, điểm chi phối của $u$ là tổ tiên chung của các tiền nhiệm của $u$ trên cây chi phối, tức là LCA của các tiền nhiệm. Có thể dùng kỹ thuật truy vấn LCA bằng nhảy bậc để xây dựng cây chi phối khi thêm từng đỉnh.

Mã tham khảo:

```cpp
std::stack<int> sta;
std::vector<int> e[N], g[N], tree[N];  // g là đồ thị ngược, tree là cây chi phối
int n, s, in[N], tpn[N], dep[N], idom[N];  // n là số đỉnh, s là gốc, in là bậc vào
int fth[N][17];

void topo(int s) {
  sta.push(s);
  while (!sta.empty()) {
    int u = sta.top();
    sta.pop();
    tpn[++tot] = u;
    for (int v : e[u]) {
      --in[v];
      if (!in[v]) {
        sta.push(v);
      }
    }
  }
}

int lca(int u, int v) {
  if (dep[u] < dep[v]) {
    std::swap(u, v);
  }
  for (int i = 15; i >= 0; --i) {
    if (dep[fth[u][i]] >= dep[v]) {
      u = fth[u][i];
    }
  }
  if (u == v) {
    return u;
  }
  for (int i = 15; i >= 0; --i) {
    if (fth[u][i] != fth[v][i]) {
      u = fth[u][i];
      v = fth[v][i];
    }
  }
  return fth[u][0];
}

void build() {
  topo(s);
  for (int i = 1; i <= n; ++i)
    for (int j = 0; j <= 15; ++j) fth[i][j] = s;
  for (int i = 1; i <= n; ++i) {
    int u = tpn[i];
    if (g[u].size()) {
      int v = g[u][0];
      for (int j = 1, q = g[u].size(); j < q; ++j) {
        v = lca(v, g[u][j]);
      }
      tree[v].push_back(u);
      fth[u][0] = v;
      dep[u] = dep[v] + 1;
      for (int i = 1; i <= 15; ++i) {
        fth[u][i] = fth[fth[u][i - 1]][i - 1];
      }
    }
  }
}

```

### Thuật toán Lengauer–Tarjan

Lengauer–Tarjan là một trong những thuật toán nổi tiếng nhất để xây dựng cây chi phối, có độ phức tạp $O(n\alpha(n, m))$. Thuật toán này sử dụng khái niệm **bán chi phối** (semi-dominator) để hỗ trợ tìm điểm chi phối trực tiếp.

#### Quy ước

Đầu tiên, thực hiện dfs từ $s$ trên đồ thị có hướng, các đỉnh và cạnh đi qua tạo thành cây $T$. Các cạnh đi qua gọi là cạnh cây, còn lại là cạnh ngoài cây; $dfn(u)$ là thứ tự duyệt của $u$; $u<v$ khi $dfn(u) < dfn(v)$.

#### Bán chi phối

Bán chi phối của một đỉnh $u$ là đỉnh nhỏ nhất $v$ sao cho tồn tại đường đi từ $v$ tới $u$, trên đường đi đó (trừ $u, v$) mọi đỉnh đều có thứ tự lớn hơn $u$. Ký hiệu $sdom(u)$:

$sdom(u) = \min(v|\exists v=v_0 \rightarrow v_1 \rightarrow\dots \rightarrow v_k = u, \forall 1\le i\le k - 1, v_i > u)$

Một số tính chất của bán chi phối:

**Bổ đề 7:** Với mọi $u$, $sdom(u) < u$.

**Chứng minh:** Theo định nghĩa, cha của $u$ trên cây $T$ cũng là bán chi phối, và $fa(u) < u$, nên mọi đỉnh lớn hơn $u$ không thể là bán chi phối.

**Bổ đề 8:** Với mọi $u$, $idom(u)$ là tổ tiên của $u$ trên cây $T$.

**Chứng minh:** Đường đi từ $s$ tới $u$ trên $T$ là một đường đi trên đồ thị, nên $idom(u)$ nằm trên đường đó.

**Bổ đề 9:** Với mọi $u$, $sdom(u)$ là tổ tiên của $u$ trên cây $T$.

**Chứng minh:** Nếu $sdom(u)$ không phải tổ tiên của $u$, thì không thể nối tới các đỉnh có thứ tự lớn hơn hoặc bằng $u$, mâu thuẫn.

**Bổ đề 10:** Với mọi $u$, $idom(u)$ là tổ tiên của $sdom(u)$ trên cây $T$.

**Chứng minh:** Có thể đi từ $s$ tới $sdom(u)$ rồi theo đường định nghĩa tới $u$. Trên đường từ $sdom(u)$ tới $u$, các đỉnh không chi phối $u$, nên $idom(u)$ là tổ tiên của $sdom(u)$.

**Bổ đề 11:** Với $u \ne v$, $v$ là tổ tiên của $u$, thì hoặc $v$ là tổ tiên của $idom(u)$, hoặc $idom(u)$ là tổ tiên của $idom(v)$.

**Chứng minh:** Với mọi đỉnh $w$ giữa $v$ và $idom(v)$, theo định nghĩa, tồn tại đường đi từ $s$ tới $idom(v)$ rồi tới $v$ không qua $w$, nên $w$ không phải $idom(u)$, do đó $idom(u)$ hoặc là hậu duệ của $v$, hoặc là tổ tiên của $idom(v)$.

Từ các bổ đề trên, ta có định lý sau:

**Định lý 1:** Bán chi phối của $u$ là đỉnh nhỏ nhất trong tập các tiền nhiệm của $u$ và bán chi phối của các tổ tiên lớn hơn $u$ trên cây $T$.

**Chứng minh:** Gọi $x$ là giá trị bên phải. Chứng minh $sdom(u) \le x$ và $sdom(u) \ge x$ như trong bản gốc.

Từ định lý 1, có thể tính bán chi phối cho mỗi đỉnh. Độ phức tạp chủ yếu nằm ở trường hợp thứ hai, có thể dùng DSU có trọng số để tối ưu.

```cpp
void dfs(int u) {
  dfn[u] = ++dfc;
  pos[dfc] = u;
  for (int i = h[0][u]; i; i = e[i].x) {
    int v = e[i].v;
    if (!dfn[v]) {
      dfs(v);
      fth[v] = u;
    }
  }
}

int find(int x) {
  if (fa[x] == x) {
    return x;
  }
  int tmp = fa[x];
  fa[x] = find(fa[x]);
  if (dfn[sdm[mn[tmp]]] < dfn[sdm[mn[x]]]) {
    mn[x] = mn[tmp];
  }
  return fa[x];
}

void getsdom() {
  dfs(1);
  for (int i = 1; i <= n; ++i) {
    mn[i] = fa[i] = sdm[i] = i;
  }
  for (int i = dfc; i >= 2; --i) {
    int u = pos[i], res = INF;
    for (int j = h[1][u]; j; j = e[j].x) {
      int v = e[j].v;
      if (!dfn[v]) {
        continue;
      }
      find(v);
      if (dfn[v] < dfn[u]) {
        res = std::min(res, dfn[v]);
      } else {
        res = std::min(res, dfn[sdm[mn[v]]]);
      }
    }
    sdm[u] = pos[res];
    fa[u] = fth[u];
  }
}

```

#### Tìm điểm chi phối trực tiếp

##### Chuyển về DAG

Nếu chưa hiểu bán chi phối để làm gì!

Trên cây $T$, thêm cạnh $sdom(u) \rightarrow u$ cho mỗi $u$. Theo bổ đề 9, đồ thị mới là DAG; theo bổ đề 10, thêm cạnh không làm thay đổi quan hệ chi phối, nên có thể chuyển về DAG và dùng thuật toán ở trên.

##### Tìm trực tiếp qua bán chi phối

Xây thêm đồ thị cũng không tối ưu!

**Định lý 2:** Với mỗi $u$, nếu trên cây $T$ từ $sdom(u)$ tới $w$, mọi $v$ trên đường đều có $sdom(v)\ge sdom(w)$, thì $idom(u) =sdom(u)$.

**Chứng minh:** Theo bổ đề 10, $idom(u)$ là $sdom(u)$ hoặc tổ tiên của nó, chỉ cần chứng minh $sdom(u)$ chi phối $u$.

Xét đường đi $P$ từ $s$ tới $u$, cần chứng minh $sdom(u)$ nằm trên $P$. Gọi $v$ là đỉnh cuối cùng trên $P$ với $v<sdom(u)$. Nếu không có thì $sdom(u)=idom(u) =s$; nếu có, gọi $w$ là đỉnh đầu tiên sau $v$ trên đường từ $sdom(u)$ tới $u$ trên cây DFS.

Chứng minh $sdom(w)\le v <sdom(v)$ như trong bản gốc.

**Định lý 3:** Với mỗi $u$, trong các đỉnh từ $sdom(u)$ tới $u$ trên cây $T$, đỉnh có bán chi phối nhỏ nhất $v$ thỏa $sdom(v)\le sdom(u)$ và $idom(v) = idom(u)$.

**Chứng minh:** Tương tự định lý 2.

Từ hai định lý trên, ta có quan hệ giữa $sdom(u)$ và $idom(u)$.

Gọi $v$ là đỉnh có $sdom(v)$ nhỏ nhất trong các đỉnh từ $sdom(u)$ tới $u$, thì:

$$
idom(u) =
\left\{ 
\begin{aligned} 
& sdom(u), &\text{nếu}\ sdom(u) = sdom(v)
\\
&idom(v), &\text{ngược lại}
\end{aligned}
\right.
$$

Chỉ cần sửa lại mã tính bán chi phối ở trên.

```cpp
struct E {
  int v, x;
} e[MAX * 4];

int h[3][MAX * 2];

int dfc, tot, n, m, u, v;
int fa[MAX], fth[MAX], pos[MAX], mn[MAX], idm[MAX], sdm[MAX], dfn[MAX],
    ans[MAX];

void add(int x, int u, int v) {
  e[++tot] = {v, h[x][u]};
  h[x][u] = tot;
}

void dfs(int u) {
  dfn[u] = ++dfc;
  pos[dfc] = u;
  for (int i = h[0][u]; i; i = e[i].x) {
    int v = e[i].v;
    if (!dfn[v]) {
      dfs(v);
      fth[v] = u;
    }
  }
}

int find(int x) {
  if (fa[x] == x) {
    return x;
  }
  int tmp = fa[x];
  fa[x] = find(fa[x]);
  if (dfn[sdm[mn[tmp]]] < dfn[sdm[mn[x]]]) {
    mn[x] = mn[tmp];
  }
  return fa[x];
}

void tar(int st) {
  dfs(st);
  for (int i = 1; i <= n; ++i) {
    fa[i] = sdm[i] = mn[i] = i;
  }
  for (int i = dfc; i >= 2; --i) {
    int u = pos[i], res = INF;
    for (int j = h[1][u]; j; j = e[j].x) {
      int v = e[j].v;
      if (!dfn[v]) {
        continue;
      }
      find(v);
      if (dfn[v] < dfn[u]) {
        res = std::min(res, dfn[v]);
      } else {
        res = std::min(res, dfn[sdm[mn[v]]]);
      }
    }
    sdm[u] = pos[res];
    fa[u] = fth[u];
    add(2, sdm[u], u);
    u = fth[u];
    for (int j = h[2][u]; j; j = e[j].x) {
      int v = e[j].v;
      find(v);
      if (sdm[mn[v]] == u) {
        idm[v] = u;
      } else {
        idm[v] = mn[v];
      }
    }
    h[2][u] = 0;
  }
  for (int i = 2; i <= dfc; ++i) {
    int u = pos[i];
    if (idm[u] != sdm[u]) {
      idm[u] = idm[idm[u]];
    }
  }
}

```

## Bài tập ví dụ

### [Luogu P5180【Mẫu】Cây chi phối](https://www.luogu.com.cn/problem/P5180)

Có thể chỉ cần xác định quan hệ chi phối, trong quá trình giải ghi lại số lượng đỉnh bị mỗi đỉnh chi phối, hoặc xây cây chi phối rồi tính size của mỗi đỉnh.

Dưới đây là mã xây cây chi phối.

??? note "Tham khảo mã nguồn"
    ```cpp
    --8<-- "docs/graph/code/dom-tree/dom-tree_1.cpp"
    ```

### [ZJOI2012 Thảm họa](https://www.luogu.com.cn/problem/P2597)

Trên DAG, xây cây chi phối rồi tính size cho mỗi đỉnh.

??? note "Tham khảo mã nguồn"
    ```cpp
    --8<-- "docs/graph/code/dom-tree/dom-tree_2.cpp"
    ```
