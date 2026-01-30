## Giới thiệu

Kiến thức tiên quyết: [Phân tách chuỗi nặng (Heavy-Light Decomposition - HLD)](../graph/hld.md)

Do độ phức tạp thời gian của Phân tách chuỗi nặng là $O(n\log^2 n)$, trong khi Link-Cut Tree (LCT) mà chúng ta thường biết có độ phức tạp $O(n\log n)$ nhưng hằng số lớn, có thể chậm hơn HLD. Vậy có phương pháp nào vừa là $O(n\log n)$ mà hằng số lại tương đối nhỏ không? Đây là lúc **Cây Nhị Phân Cân Bằng Toàn Cục** (Global Balanced Binary Tree - GBBT) xuất hiện.

GBBT thực chất là một rừng (forest) các cây nhị phân, trong đó mỗi cây nhị phân duy trì một chuỗi nặng (heavy path). Tuy nhiên, các cây nhị phân trong rừng này có mối liên hệ với nhau: gốc của mỗi cây nhị phân được nối với nút cha của đỉnh đầu chuỗi nặng tương ứng, giống như trong LCT. Điểm khác biệt là GBBT là cây tĩnh, hình dạng của cây không thay đổi sau khi xây dựng, không giống như LCT.

GBBT là một cấu trúc dữ liệu có thể xử lý các thao tác sửa đổi/truy vấn trên một chuỗi (chain) của cây, đạt được:

-   Sửa đổi toàn bộ một chuỗi trong $O(\log n)$.
-   Truy vấn toàn bộ một chuỗi trong $O(\log n)$.
-   Tìm Tổ Tiên Chung Gần Nhất (LCA), sửa đổi/truy vấn trên cây con, đều đạt độ phức tạp tương tự như HLD là $O(\log n)$.

## Tính chất chính

1.  GBBT được tạo thành từ nhiều cây nhị phân được nối với nhau bằng các cạnh nhẹ (light edges). Mỗi cây nhị phân duy trì một chuỗi nặng của cây gốc. Phép duyệt trung thứ tự (in-order traversal) của cây nhị phân này chính là thứ tự các nút trên chuỗi nặng đó theo độ sâu tăng dần. Mỗi nút chỉ thuộc về đúng một cây nhị phân.
2.  Các cạnh được chia thành cạnh nặng (heavy edge) và cạnh nhẹ (light edge). Cạnh nặng là cạnh nằm trong cây nhị phân, được duy trì giống như cách duy trì cây nhị phân thông thường (lưu trữ con trái, con phải và nút cha). Cạnh nhẹ nối từ gốc của một cây nhị phân đến nút cha của đỉnh đầu chuỗi nặng tương ứng. Cạnh nhẹ duy trì theo kiểu "nhận cha không nhận con", tức là chỉ có thể truy cập từ nút con đến nút cha, không thể ngược lại. Lưu ý, các cạnh trong GBBT không có mối quan hệ tương ứng trực tiếp với các cạnh trong cây gốc.
3.  Tổng số cạnh nặng và cạnh nhẹ tạo nên chiều cao của GBBT là $O(\log n)$. Đây là tính chất đảm bảo độ phức tạp thời gian của GBBT.

Dưới đây là ví dụ về xây dựng GBBT. Hình đầu tiên là cây gốc, lấy nút 1 làm gốc. Các đường nét liền là cạnh nặng.

![global-bst-1](images/global-bst-1.svg)

Hình thứ hai là GBBT được xây dựng, trong đó đường nét đứt là cạnh nhẹ, đường nét liền là cạnh nặng. Mỗi cây nhị phân được bao bởi vòng tròn màu đỏ.

![global-bst-2](images/global-bst-2.svg)

## Xây dựng

Đầu tiên, chúng ta thực hiện DFS giống như trong HLD thông thường để tìm con nặng của mỗi nút. Sau đó, bắt đầu từ nút gốc, tìm chuỗi nặng chứa nút gốc, đệ quy xây dựng cây cho các con nhẹ, và nối cạnh nhẹ. Tiếp theo, chúng ta cần xây dựng một cây nhị phân cho các nút trên chuỗi nặng. Ta lưu các nút trên chuỗi nặng vào một mảng, tính toán kích thước (size) đóng góp của mỗi nút (là tổng kích thước cây con của các con nhẹ cộng thêm 1 cho chính nó). Sau đó, ta tìm điểm giữa có trọng số của chuỗi này, dùng nó làm gốc của cây nhị phân, và đệ quy xây dựng hai bên, đồng thời nối cạnh nặng.

Mã nguồn như sau:

???+ note "Triển khai"
    ```cpp
    std::vector<int> G[N];
    int n, fa[N], son[N], sz[N];
    
    void dfsS(int u) {
      sz[u] = 1;
      for (int v : G[u]) {
        dfsS(v);
        sz[u] += sz[v];
        if (sz[v] > sz[son[u]]) son[u] = v;
      }
    }
    
    int b[N], bs[N], l[N], r[N], f[N], ss[N];
    
    // Xây dựng cây nhị phân cho các nút trong b[bl, br) và trả về gốc của cây nhị phân
    int cbuild(int bl, int br) {
      int x = bl, y = br;
      while (y - x > 1) {
        int mid = (x + y) >> 1;
        if (2 * (bs[mid] - bs[bl]) <= bs[br] - bs[bl])
          x = mid;
        else
          y = mid;
      }
      // Tìm điểm giữa có trọng số bằng tìm kiếm nhị phân
      y = b[x];
      ss[y] = br - bl;  // ss: kích thước cây con nặng trong cây nhị phân
      if (bl < x) {
        l[y] = cbuild(bl, x);
        f[l[y]] = y;
      }
      if (x + 1 < br) {
        r[y] = cbuild(x + 1, br);
        f[r[y]] = y;
      }
      return y;
    }
    
    int build(int x) {
      int y = x;
      do
        for (int v : G[y])
          if (v != son[y])
            f[build(v)] =
                y;  // Đệ quy xây dựng cây và nối cạnh nhẹ, chú ý nối từ gốc cây nhị phân, không phải từ con
      while (y = son[y]);
      y = 0;
      do {
        b[y++] = x;                              // Lưu trữ các nút trên chuỗi nặng
        bs[y] = bs[y - 1] + sz[x] - sz[son[x]];  // bs: prefix sum của (size con nhẹ + 1)
      } while (x = son[x]);
      return cbuild(0, y);
    }
    ```

Từ mã nguồn có thể thấy độ phức tạp xây dựng là $O(n\log n)$. Tiếp theo ta có thể chứng minh chiều cao cây là $O(\log n)$: xem xét nhảy từ một nút bất kỳ lên nút cha. Nếu nhảy cạnh nhẹ, tương đương với việc nhảy đến một chuỗi nặng khác trong cây gốc, theo tính chất của HLD, tối đa có $O(\log n)$ lần nhảy cạnh nhẹ; vì khi xây dựng cây nhị phân, nút gốc được chọn là điểm giữa có trọng số tính cả con nhẹ, nên sau mỗi lần nhảy cạnh nặng (trong cây nhị phân), tổng kích thước có trọng số ít nhất tăng gấp đôi, do đó số lần nhảy cạnh nặng cũng tối đa là $O(\log n)$. Tổng chiều cao cây là $O(\log n)$.

## Truy vấn

Phần trên là về GBBT. Các thao tác sửa đổi và truy vấn trên chuỗi còn lại tương đối đơn giản, chỉ cần bắt đầu từ nút cần thao tác và nhảy liên tục lên nút gốc. Thao tác trên tất cả các nút trên chuỗi nặng có độ sâu nhỏ hơn nút đang xét, về bản chất tương đương với việc thao tác trên tất cả các nút phía bên trái của nút mục tiêu trong cây nhị phân của chuỗi nặng đó. Các thao tác này có thể được phân rã thành một loạt các thao tác trên cây con, tương tự như cách duy trì cây nhị phân thông thường, trong đó có liên quan đến việc duy trì tổng cây con và gán nhãn cây con. Ở đây sử dụng kỹ thuật **tính vĩnh viễn của nhãn** (lazy propagation/tag permanentization). Cũng có thể sử dụng `pushdown` để gán nhãn và `pushup` để duy trì tổng cây con, nhưng cách này có thể phức tạp hơn, vì thông thường thao tác trên cây nhị phân được thực hiện từ trên xuống, nhưng ở đây cần xác định đường nhảy trước, sau đó mới thực hiện `pushdown` từ trên xuống, có thể dẫn đến hằng số lớn hơn.

Mã nguồn như sau:

???+ note "Triển khai"
    ```cpp
    // a: nhãn gán cho cây con
    // s: tổng cây con (không tính giá trị do nhãn gán)
    int a[N], s[N];
    
    void add(int x) {
      bool t = true;
      int z = 0;
      while (x) {
        s[x] += z;
        if (t) {
          a[x]++;
          if (r[x]) a[r[x]]--;
          z += 1 + ss[l[x]];
          s[x] -= ss[r[x]];
        }
        t = (x != l[f[x]]);
        if (t && x != r[f[x]]) z = 0;  // Nhảy qua cạnh nhẹ cần xóa bộ nhớ đệm (clear z)
        x = f[x];
      }
    }
    
    int query(int x) {
      int ret = 0;
      bool t = true;
      int z = 0;
      while (x) {
        if (t) {
          ret += s[x] - s[r[x]];
          ret -= 1ll * ss[r[x]] * a[r[x]];
          z += 1 + ss[l[x]];
        }
        ret += 1ll * z * a[x];
        t = (x != l[f[x]]);
        if (t && x != r[f[x]]) z = 0;  // Nhảy qua cạnh nhẹ cần xóa bộ nhớ đệm (clear z)
        x = f[x];
      }
      return ret;
    }
    ```

Ngoài ra, đối với các thao tác trên cây con, cần xem xét các con nhẹ. Cần duy trì tổng cây con và nhãn cây con bao gồm cả các con nhẹ. Có thể tham khảo cách làm trong bài toán "[P3384 [Template] Heavy-Light Decomposition](https://www.luogu.com.cn/problem/P3384)".

## Bài toán ví dụ

??? note "[P4751 [Template] "DP động" & Chia để trị động trên cây (Bản nâng cao)](https://www.luogu.com.cn/problem/P4751)"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    constexpr int MAXN = 1000000;
    constexpr int MAXM = 3000000;
    constexpr int INF = 0x3FFFFFFF;
    using namespace std;
    
    struct edge {
      int to;
      edge *nxt;
    } edges[MAXN * 2 + 5];
    
    edge *ncnt = &edges[0], *Adj[MAXN + 5];
    int n, m;
    
    struct Matrix {
      int M[2][2];
    
      Matrix operator*(const Matrix &B) const {
        static Matrix ret;
        for (int i = 0; i < 2; i++)
          for (int j = 0; j < 2; j++) {
            ret.M[i][j] = -INF;
            for (int k = 0; k < 2; k++)
              ret.M[i][j] = max(ret.M[i][j], M[i][k] + B.M[k][j]);
          }
        return ret;
      }
    } matr1[MAXN + 5], matr2[MAXN + 5];  // Mỗi điểm duy trì hai ma trận
    
    int root;
    int w[MAXN + 5], dep[MAXN + 5], son[MAXN + 5], siz[MAXN + 5], lsiz[MAXN + 5];
    int g[MAXN + 5][2], f[MAXN + 5][2], trfa[MAXN + 5], bstch[MAXN + 5][2];
    int stk[MAXN + 5], tp;
    bool vis[MAXN + 5];
    
    void AddEdge(int u, int v) {
      edge *p = ++ncnt;
      p->to = v;
      p->nxt = Adj[u];
      Adj[u] = p;
    
      edge *q = ++ncnt;
      q->to = u;
      q->nxt = Adj[v];
      Adj[v] = q;
    }
    
    void DFS(int u, int fa) {
      siz[u] = 1;
      for (edge *p = Adj[u]; p != NULL; p = p->nxt) {
        int v = p->to;
        if (v == fa) continue;
        dep[v] = dep[u] + 1;
        DFS(v, u);
        siz[u] += siz[v];
        if (!son[u] || siz[son[u]] < siz[v]) son[u] = v;
      }
      lsiz[u] = siz[u] - siz[son[u]];  // size của các con nhẹ + 1
    }
    
    void DFS2(int u, int fa) {
      f[u][1] = w[u], f[u][0] = 0;
      g[u][1] = w[u], g[u][0] = 0;
      if (son[u]) {
        DFS2(son[u], u);
        f[u][0] += max(f[son[u]][0], f[son[u]][1]);
        f[u][1] += f[son[u]][0];
      }
      for (edge *p = Adj[u]; p != NULL; p = p->nxt) {
        int v = p->to;
        if (v == fa || v == son[u]) continue;
        DFS2(v, u);
        f[u][0] += max(f[v][0], f[v][1]);  // f[][] là mảng DP thông thường
        f[u][1] += f[v][0];
        g[u][0] += max(f[v][0], f[v][1]);  // g[][] chỉ thống kê thông tin của chính nó và con nhẹ
        g[u][1] += f[v][0];
      }
    }
    
    void PushUp(int u) {
      matr2[u] = matr1[u];  // matr1 là thông tin của một điểm cộng với con nhẹ, matr2 là thông tin khoảng
      if (bstch[u][0]) matr2[u] = matr2[bstch[u][0]] * matr2[u];
      // Chú ý hướng chuyển đổi, nhưng nếu định nghĩa phép nhân ma trận khác, hướng có thể khác
      if (bstch[u][1]) matr2[u] = matr2[u] * matr2[bstch[u][1]];
    }
    
    int getmx2(int u) { return max(matr2[u].M[0][0], matr2[u].M[0][1]); }
    
    int getmx1(int u) { return max(getmx2(u), matr2[u].M[1][0]); }
    
    int SBuild(int l, int r) {
      if (l > r) return 0;
      int tot = 0;
      for (int i = l; i <= r; i++) tot += lsiz[stk[i]];
      for (int i = l, sumn = lsiz[stk[l]]; i <= r; i++, sumn += lsiz[stk[i]])
        if (sumn * 2 >= tot)  // Là trọng tâm (centroid)
        {
          int lch = SBuild(l, i - 1), rch = SBuild(i + 1, r);
          bstch[stk[i]][0] = lch;
          bstch[stk[i]][1] = rch;
          trfa[lch] = trfa[rch] = stk[i];
          PushUp(stk[i]);  // Thống kê thông tin khoảng lên
          return stk[i];
        }
      return 0;
    }
    
    int Build(int u) {
      for (int pos = u; pos; pos = son[pos]) vis[pos] = true;
      for (int pos = u; pos; pos = son[pos])
        for (edge *p = Adj[pos]; p != NULL; p = p->nxt)
          if (!vis[p->to])  // Là con nhẹ
          {
            int v = p->to, ret = Build(v);
            trfa[ret] = pos;  // trfa[] của cây con nhẹ nối với gốc chuỗi nặng
          }
      tp = 0;
      for (int pos = u; pos; pos = son[pos]) stk[++tp] = pos;  // Lấy ra các nút trên chuỗi nặng
      int ret = SBuild(1, tp);  // Xây dựng cây nhị phân riêng cho chuỗi nặng
      return ret;               // Trả về gốc cây nhị phân của chuỗi nặng này
    }
    
    void Modify(int u, int val) {
      matr1[u].M[1][0] += val - w[u];
      w[u] = val;
      for (int pos = u; pos; pos = trfa[pos])
        if (trfa[pos] && bstch[trfa[pos]][0] != pos && bstch[trfa[pos]][1] != pos) {
          matr1[trfa[pos]].M[0][0] -= getmx1(pos);
          matr1[trfa[pos]].M[0][1] = matr1[trfa[pos]].M[0][0];
          matr1[trfa[pos]].M[1][0] -= getmx2(pos);
          PushUp(pos);
          matr1[trfa[pos]].M[0][0] += getmx1(pos);
          matr1[trfa[pos]].M[0][1] = matr1[trfa[pos]].M[0][0];
          matr1[trfa[pos]].M[1][0] += getmx2(pos);
        } else
          PushUp(pos);
    }
    
    int read() {
      int ret = 0, f = 1;
      char c = 0;
      while (c < '0' || c > '9') {
        c = getchar();
        if (c == '-') f = -f;
      }
      ret = 10 * ret + c - '0';
      while (true) {
        c = getchar();
        if (c < '0' || c > '9') break;
        ret = 10 * ret + c - '0';
      }
      return ret * f;
    }
    
    void print(int x) {
      if (x == 0) return;
      print(x / 10);
      putchar(x % 10 + '0');
    }
    
    int main() {
      scanf("%d %d", &n, &m);
      for (int i = 1; i <= n; i++) w[i] = read();
      int u, v;
      for (int i = 1; i < n; i++) {
        u = read(), v = read();
        AddEdge(u, v);
      }
      DFS(1, -1);
      // Tìm con nặng
      DFS2(1, -1);
      // Tính giá trị DP ban đầu, cũng có thể tính trong Build(), nhưng viết thế này đồng bộ với cách viết HLD
      for (int i = 1; i <= n; i++) {
        matr1[i].M[0][0] = matr1[i].M[0][1] = g[i][0];
        matr1[i].M[1][0] = g[i][1], matr1[i].M[1][1] = -INF;  // Khởi tạo ma trận
      }
      root = Build(1);  // root là trọng tâm của chuỗi nặng chứa nút gốc
      int lastans = 0;
      for (int i = 1; i <= m; i++) {
        u = read(), v = read();
        u ^= lastans;  // Yêu cầu trực tuyến (online)
        Modify(u, v);
        lastans = getmx1(root);  // Lấy giá trị trực tiếp
        if (lastans == 0)
          putchar('0');
        else
          print(lastans);
        putchar('\n');
      }
      return 0;
    }
    ```

## Tham khảo

[P4211 [LNOI2014] LCA | Global Balanced Binary Tree](https://www.luogu.com.cn/blog/nederland/globalbst)