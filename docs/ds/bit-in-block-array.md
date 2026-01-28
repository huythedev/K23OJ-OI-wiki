tác giả: Backl1ght, Tiphereth-A, Enter-tainer, Ir1d, ksyx, leoleoasd, Xeonacid, aaron20100919

## Giới thiệu

Chia căn lồng cây chỉ số nhị phân (Fenwick tree/BIT) trong những điều kiện nhất định có thể được dùng để thực hiện một số việc mà cây lồng cây có thể làm, nhưng so với cây lồng cây, mã nguồn của chia căn lồng cây chỉ số nhị phân ngắn gọn hơn và dễ thực hiện hơn.

## Một ví dụ đơn giản

Một ví dụ đơn giản là truy vấn số lượng điểm trong một vùng hình chữ nhật trên mặt phẳng hai chiều.

???+ note "Truy vấn vùng hình chữ nhật"
    Cho $n$ điểm $(x_i, y_i)$ trong mặt phẳng hai chiều, trong đó $1 \le i \le n, 1 \le x_i, y_i \le n, 1 \le n \le 10^5$, yêu cầu thực hiện các thao tác sau:
    
    1.  Cho $a, b, c, d$, truy vấn số lượng điểm trong vùng hình chữ nhật có $(a, b)$ là góc trên bên trái và $c, d$ là góc dưới bên phải.
    2.  Cho $x, y$, thay đổi tung độ của điểm có hoành độ $x$ thành $y$.
    
    Bài toán **ép buộc trực tuyến (forced online)**, đảm bảo $x_i \ne x_j (1 \le i, j \le n, i \ne j)$.

Đối với thao tác 1, có thể thông qua nguyên lý bao hàm - loại trừ hình chữ nhật để chuyển nó thành 4 truy vấn 2D partial sum để giải quyết. Vì ép buộc trực tuyến nên các thuật toán ngoại tuyến như CDQ phân trị không thể giải quyết được, từ đó nghĩ đến cây lồng cây, ví dụ như cây chỉ số nhị phân lồng Treap. Điều này thực sự có thể giải quyết vấn đề, nhưng mã nguồn quá dài và không đặc biệt dễ thực hiện.

Lưu ý rằng, đề bài còn đảm bảo thêm $x_i \ne x_j (1 \le i, j \le n, i \ne j)$, lúc này có thể dùng chia căn lồng cây chỉ số nhị phân để giải quyết.

### Khởi tạo

Đầu tiên, một $x$ chỉ tương ứng với một $y$, nên có thể dùng một mảng để ghi lại quan hệ ánh xạ này, ví dụ gọi $Y_i$ biểu thị tung độ của điểm có hoành độ là $i$.

Sau đó, lấy $\sqrt n$ làm kích thước khối để chia căn theo hoành độ. Xây dựng một cây chỉ số nhị phân trọng số cho mỗi khối. Ký hiệu $T_i$ là cây chỉ số nhị phân tương ứng với khối thứ $i$, $T_{i, j}$ biểu thị số lượng điểm trong khối $i$ có tung độ nằm trong $(j - lowbit(j), j]$.

### Truy vấn

Đối với thao tác 1, chuyển nó thành 4 truy vấn 2D partial sum. Bây giờ chỉ cần giải quyết việc cho $a, b$, hỏi có bao nhiêu điểm thỏa mãn $1 \le x_i \le a, 1\le y_i \le b$.

Bây giờ cần truy vấn phạm vi hoành độ là $[1, a]$. Vì phía bên phải nhất của phạm vi truy vấn có thể có một đoạn không phải là khối hoàn chỉnh, nên quét qua đoạn này một cách thô bạo (brute-force), xem có thỏa mãn $Y_i \le b$ hay không, thống kê số lượng điểm thỏa mãn yêu cầu của đoạn này.

Bây giờ chỉ cần xử lý các khối hoàn chỉnh. Quét thô bạo qua các khối phía trước, truy vấn số lượng giá trị nhỏ hơn $b$ trong cây chỉ số nhị phân tương ứng với mỗi khối, rồi cộng dồn vào đáp án.

Như vậy là xong? Không, lưu ý rằng khi xử lý các khối hoàn chỉnh, thực chất tương đương với việc truy vấn tổng tiền tố của $T$, nếu khi sửa đổi cũng sử dụng kỹ thuật cây chỉ số nhị phân để xử lý $T$, thì độ phức tạp khi truy vấn sẽ thấp hơn.

### Sửa đổi

Cách làm thông thường là trước tiên tìm khối chứa điểm $x$, sau đó thực hiện một phép trừ và một phép cộng để sửa đổi điểm trên hai cây chỉ số nhị phân trọng số, rồi đặt $Y_x$ thành $y$.

Nếu sử dụng tối ưu hóa đã nói ở trên, thì đó là thực hiện một quy trình sửa đổi cây chỉ số nhị phân đối với $T$, mỗi lần sửa đổi cũng là một phép trừ và một phép cộng để sửa đổi điểm trên hai cây chỉ số nhị phân trọng số.

Thực hiện một số thay đổi đối với các bước trên, ví dụ như đổi một phép trừ và một phép cộng thành chỉ trừ, chính là xóa điểm; đổi thành chỉ cộng, chính là thêm điểm. Nhưng phải lưu ý một $x$ chỉ có thể tương ứng với một $y$.

### Độ phức tạp không gian

Chia căn thành $\sqrt n$ khối, mỗi khối có một cây chỉ số nhị phân chiếm không gian $O(n)$, nên độ phức tạp không gian là $O(n \sqrt n)$.

### Độ phức tạp thời gian

Đối với truy vấn, duyệt qua đoạn không hoàn chỉnh mất $O(\sqrt n)$. Sau đó, thực hiện truy vấn cây chỉ số nhị phân trên $T$, mỗi $T_i$ đi qua cũng thực hiện truy vấn cây chỉ số nhị phân, bước này có độ phức tạp là $O(\log (\sqrt n) \log n)$. Vì vậy độ phức tạp thời gian của truy vấn là $O (\sqrt n + \log (\sqrt n) \log n)$.

Sửa đổi cũng giống như truy vấn, độ phức tạp là $O (\sqrt n + \log (\sqrt n) \log n)$.

## Ví dụ 1

???+ note "[Intersection of Permutations](https://codeforces.com/problemset/problem/1093/E)"
    Cho hai hoán vị $a$ và $b$, yêu cầu thực hiện hai loại thao tác sau:
    
    1.  Cho $l_a, r_a, l_b, r_b$, yêu cầu truy vấn số lượng phần tử xuất hiện đồng thời trong $a[l_a ... r_a]$ và $b[l_b ... r_b]$.
    2.  Cho $x, y$, thực hiện $swap(b_x, b_y)$.
    
    Độ dài dãy $n$ thỏa mãn $2 \le n \le 2 \cdot 10^5$, số lượng thao tác $q$ thỏa mãn $1 \le q \le 2 \cdot 10^5$.

Đối với mỗi giá trị $i$, ký hiệu $x_i$ là chỉ số của nó trong hoán vị $b$, $y_i$ là chỉ số của nó trong hoán vị $a$. Như vậy, thao tác 1 trở thành một truy vấn số lượng điểm trong vùng hình chữ nhật, thao tác 2 có thể coi là hai thao tác sửa đổi. Và vì là hoán vị nên thỏa mãn một $x$ tương ứng với một $y$, do đó bài này có thể dùng chia căn lồng cây chỉ số nhị phân để viết.

??? note "Mã tham khảo (Chia căn lồng cây chỉ số nhị phân - 1s)"
    ```cpp
    #include <cmath>
    #include <cstdio>
    using namespace std;
    constexpr int N = 2e5 + 5;
    constexpr int M = 447 + 5;  // sqrt(N) + 5
    
    int n, m, pa[N], pb[N];
    
    int nn, block_size, block_cnt, block_id[N], L[N], R[N], T[M][N];
    
    void build(int n) {
      nn = n;
      block_size = sqrt(nn);
      block_cnt = nn / block_size;
      for (int i = 1; i <= block_cnt; ++i) {
        L[i] = R[i - 1] + 1;
        R[i] = i * block_size;
      }
      if (R[block_cnt] < nn) {
        ++block_cnt;
        L[block_cnt] = R[block_cnt - 1] + 1;
        R[block_cnt] = nn;
      }
      for (int j = 1; j <= block_cnt; ++j)
        for (int i = L[j]; i <= R[j]; ++i) block_id[i] = j;
    }
    
    int lb(int x) { return x & -x; }
    
    void add(int p, int v, int d) {
      for (int i = block_id[p]; i <= block_cnt; i += lb(i))
        for (int j = v; j <= nn; j += lb(j)) T[i][j] += d;
    }
    
    int getsum(int p, int v) {
      if (!p) return 0;
      int res = 0;
      int id = block_id[p];
      for (int i = L[id]; i <= p; ++i)
        if (pb[i] <= v) ++res;
      for (int i = id - 1; i; i -= lb(i))
        for (int j = v; j; j -= lb(j)) res += T[i][j];
      return res;
    }
    
    void update(int x, int y) {
      add(x, pb[x], -1);
      add(y, pb[y], -1);
      swap(pb[x], pb[y]);
      add(x, pb[x], 1);
      add(y, pb[y], 1);
    }
    
    int query(int la, int ra, int lb, int rb) {
      int res = getsum(rb, ra) - getsum(rb, la - 1) - getsum(lb - 1, ra) +
                getsum(lb - 1, la - 1);
      return res;
    }
    
    int main() {
      scanf("%d %d", &n, &m);
      int v;
      for (int i = 1; i <= n; ++i) scanf("%d", &v), pa[v] = i;
      for (int i = 1; i <= n; ++i) scanf("%d", &v), pb[i] = pa[v];
    
      build(n);
      for (int i = 1; i <= n; ++i) add(i, pb[i], 1);
    
      int op, la, lb, ra, rb, x, y;
      for (int i = 1; i <= m; ++i) {
        scanf("%d", &op);
        if (op == 1) {
          scanf("%d %d %d %d", &la, &ra, &lb, &rb);
          printf("%d\n", query(la, ra, lb, rb));
        } else if (op == 2) {
          scanf("%d %d", &x, &y);
          update(x, y);
        }
      }
      return 0;
    }
    ```

??? note "Mã tham khảo (Cây chỉ số nhị phân lồng Treap—TLE)"
    ```cpp
    #include <cstdio>
    #include <random>
    using namespace std;
    constexpr int N = 2e5 + 5;
    mt19937 rng(random_device{}());
    
    int n, m, pa[N], pb[N];
    
    // Treap
    struct Treap {
      struct node {
        node *l, *r;
        int sz, rnd, v;
    
        node(int _v) : l(NULL), r(NULL), sz(1), rnd(rng()), v(_v) {}
      };
    
      int get_size(node*& p) { return p ? p->sz : 0; }
    
      void push_up(node*& p) {
        if (!p) return;
        p->sz = get_size(p->l) + get_size(p->r) + 1;
      }
    
      node* root;
    
      node* merge(node* a, node* b) {
        if (!a) return b;
        if (!b) return a;
        if (a->rnd < b->rnd) {
          a->r = merge(a->r, b);
          push_up(a);
          return a;
        } else {
          b->l = merge(a, b->l);
          push_up(b);
          return b;
        }
      }
    
      void split_val(node* p, const int& k, node*& a, node*& b) {
        if (!p)
          a = b = NULL;
        else {
          if (p->v <= k) {
            a = p;
            split_val(p->r, k, a->r, b);
            push_up(a);
          } else {
            b = p;
            split_val(p->l, k, a, b->l);
            push_up(b);
          }
        }
      }
    
      void split_size(node* p, int k, node*& a, node*& b) {
        if (!p)
          a = b = NULL;
        else {
          if (get_size(p->l) <= k) {
            a = p;
            split_size(p->r, k - get_size(p->l), a->r, b);
            push_up(a);
          } else {
            b = p;
            split_size(p->l, k, a, b->l);
            push_up(b);
          }
        }
      }
    
      void ins(int val) {
        node *a, *b;
        split_val(root, val, a, b);
        a = merge(a, new node(val));
        root = merge(a, b);
      }
    
      void del(int val) {
        node *a, *b, *c, *d;
        split_val(root, val, a, b);
        split_val(a, val - 1, c, d);
        delete d;
        root = merge(c, b);
      }
    
      int qry(int val) {
        node *a, *b;
        split_val(root, val, a, b);
        int res = get_size(a);
        root = merge(a, b);
        return res;
      }
    
      int qry(int l, int r) { return qry(r) - qry(l - 1); }
    };
    
    // Fenwick Tree
    Treap T[N];
    
    int lb(int x) { return x & -x; }
    
    void ins(int x, int v) {
      for (; x <= n; x += lb(x)) T[x].ins(v);
    }
    
    void del(int x, int v) {
      for (; x <= n; x += lb(x)) T[x].del(v);
    }
    
    int qry(int x, int mi, int ma) {
      int res = 0;
      for (; x; x -= lb(x)) res += T[x].qry(mi, ma);
      return res;
    }
    
    int main() {
      scanf("%d %d", &n, &m);
      int v;
      for (int i = 1; i <= n; ++i) scanf("%d", &v), pa[v] = i;
      for (int i = 1; i <= n; ++i) scanf("%d", &v), pb[i] = pa[v];
      for (int i = 1; i <= n; ++i) ins(i, pb[i]);
    
      int op, la, lb, ra, rb, x, y;
      for (int i = 1; i <= m; ++i) {
        scanf("%d", &op);
        if (op == 1) {
          scanf("%d %d %d %d", &la, &ra, &lb, &rb);
          printf("%d\n", qry(rb, la, ra) - qry(lb - 1, la, ra));
        } else if (op == 2) {
          scanf("%d %d", &x, &y);
          del(x, pb[x]);
          del(y, pb[y]);
          swap(pb[x], pb[y]);
          ins(x, pb[x]);
          ins(y, pb[y]);
        }
      }
      return 0;
    }
    ```

## Ví dụ 2

???+ note "[Complicated Computations](https://codeforces.com/contest/1436/problem/E)"
    Cho một dãy $a$, lấy MEX của tất cả các dãy con liên tục của $a$ tạo thành mảng $b$, hỏi MEX của $b$. MEX của một dãy là số nguyên dương nhỏ nhất không xuất hiện trong dãy.
    
    Độ dài dãy $n$ thỏa mãn $1 \le n \le 10^5$.

**Quan sát**: MEX của một dãy là $mex$, khi và chỉ khi dãy này chứa từ $1$ đến $mex-1$, nhưng không chứa $mex$.

Lần lượt phán đoán xem có tồn tại các dãy con liên tục có MEX từ $1$ đến $n+1$ hay không. Nếu không có dãy con liên tục nào có MEX là $i$, thì đáp án chính là $i$. Nếu đều tồn tại, thì đáp án là $n + 2$.

Khi phán đoán $i$, coi dãy như là nhiều đoạn được phân tách bởi không hoặc nhiều số $i$. Nếu tồn tại một đoạn, đoạn này chứa từ $1$ đến $i - 1$, nhưng không chứa $i$, thì điều đó có nghĩa là tồn tại dãy con liên tục có giá trị $i$.

Dùng một mảng $Y_j$ ghi lại vị trí của phần tử có giá trị là $a_j$ trước đó, lấy $j$ làm $x$, $Y_j$ làm $y$, $a_j$ làm $z$. Như vậy, việc tính xem trong đoạn có chứa từ $1$ đến $i - 1$ hay không là một vấn đề ba chiều partial order. Một cách hình thức, phán đoán xem giá trị MEX của đoạn $[l, r]$ có phải là $i$ hay không, chính là xem số lượng điểm thỏa mãn $l \le j \le r, Y_j \le l - 1, a_j \le i - 1$ có phải là $i-1$ hay không.

Nếu sau khi phán đoán xong các phần tử có giá trị $i$ mới chèn các điểm tương ứng vào, lúc này vì trong $[l, r]$ chỉ tồn tại các phần tử có $a_j \le i - 1$, nên vấn đề ba chiều partial order nói trên có thể chuyển đổi thành vấn đề hai chiều partial order.

??? note "Mã tham khảo (Chia căn lồng cây chỉ số nhị phân - 78ms)"
    ```cpp
    #include <cmath>
    #include <cstdio>
    #include <vector>
    using namespace std;
    constexpr int N = 1e5 + 5;
    constexpr int M = 316 + 5;  // sqrt(N) + 5
    
    // Chia căn
    int nn, b[N], block_size, block_cnt, block_id[N], L[N], R[N], T[M][N];
    
    void build(int n) {
      nn = n;
      block_size = sqrt(nn);
      block_cnt = nn / block_size;
      for (int i = 1; i <= block_cnt; ++i) {
        L[i] = R[i - 1] + 1;
        R[i] = i * block_size;
      }
      if (R[block_cnt] < nn) {
        ++block_cnt;
        L[block_cnt] = R[block_cnt - 1] + 1;
        R[block_cnt] = nn;
      }
      for (int j = 1; j <= block_cnt; ++j)
        for (int i = L[j]; i <= R[j]; ++i) block_id[i] = j;
    }
    
    int lb(int x) { return x & -x; }
    
    // d = 1: Thêm điểm (p, v)
    // d = -1: Xóa điểm (p, v)
    void add(int p, int v, int d) {
      for (int i = block_id[p]; i <= block_cnt; i += lb(i))
        for (int j = v; j <= nn; j += lb(j)) T[i][j] += d;
    }
    
    // Hỏi trong [1, r], có bao nhiêu điểm có tung độ nhỏ hơn hoặc bằng val
    int getsum(int p, int v) {
      if (!p) return 0;
      int res = 0;
      int id = block_id[p];
      for (int i = L[id]; i <= p; ++i)
        if (b[i] && b[i] <= v) ++res;
      for (int i = id - 1; i; i -= lb(i))
        for (int j = v; j; j -= lb(j)) res += T[i][j];
      return res;
    }
    
    // Hỏi trong [l, r], có bao nhiêu điểm có tung độ nhỏ hơn hoặc bằng val
    int query(int l, int r, int val) {
      if (l > r) return -1;
      int res = getsum(r, val) - getsum(l - 1, val);
      return res;
    }
    
    // Thêm điểm (p, v)
    void update(int p, int v) {
      b[p] = v;
      add(p, v, 1);
    }
    
    int n, a[N];
    vector<int> g[N];
    
    int main() {
      scanf("%d", &n);
    
      // Để giảm bớt thảo luận, thêm nút lính canh
      // Vì khi thêm vào cây chỉ số nhị phân, nếu là 0 có thể bị lặp vô hạn, nên dịch chuyển sang phải một vị trí
      // a_1 và a_{n+2} là nút lính canh
      for (int i = 2; i <= n + 1; ++i) scanf("%d", &a[i]);
      for (int i = 2; i <= n + 1; ++i) g[a[i]].push_back(i);
    
      // Chia căn
      build(n + 2);
    
      int ans = n + 2, lst, ok;
      for (int i = 1; i <= n + 1; ++i) {
        g[i].push_back(n + 2);
    
        lst = 1;
        ok = 0;
        for (int pos : g[i]) {
          if (query(lst + 1, pos - 1, lst) == i - 1) {
            ok = 1;
            break;
          }
          lst = pos;
        }
    
        if (!ok) {
          ans = i;
          break;
        }
    
        lst = 1;
        g[i].pop_back();
        for (int pos : g[i]) {
          update(pos, lst);
          lst = pos;
        }
      }
      printf("%d\n", ans);
      return 0;
    }
    ```

??? note "Mã tham khảo (Cây phân đoạn lồng Treap - 468ms)"
    ```cpp
    #include <cstdio>
    #include <random>
    #include <vector>
    using namespace std;
    constexpr int N = 1e5 + 5;
    
    vector<int> g[N];
    int n, a[N];
    
    mt19937 rng(random_device{}());
    
    struct Treap {
      struct node {
        node *l, *r;
        unsigned rnd;
        int sz, v;
    
        node(int _v) : l(NULL), r(NULL), rnd(rng()), sz(1), v(_v) {}
      };
    
      int get_size(node*& p) { return p ? p->sz : 0; }
    
      void push_up(node*& p) {
        if (!p) return;
        p->sz = get_size(p->l) + get_size(p->r) + 1;
      }
    
      node* root;
    
      node* merge(node* a, node* b) {
        if (!a) return b;
        if (!b) return a;
        if (a->rnd < b->rnd) {
          a->r = merge(a->r, b);
          push_up(a);
          return a;
        } else {
          b->l = merge(a, b->l);
          push_up(b);
          return b;
        }
      }
    
      void split_val(node* p, const int& k, node*& a, node*& b) {
        if (!p)
          a = b = NULL;
        else {
          if (p->v <= k) {
            a = p;
            split_val(p->r, k, a->r, b);
            push_up(a);
          } else {
            b = p;
            split_val(p->l, k, a, b->l);
            push_up(b);
          }
        }
      }
    
      void split_size(node* p, int k, node*& a, node*& b) {
        if (!p)
          a = b = NULL;
        else {
          if (get_size(p->l) <= k) {
            a = p;
            split_size(p->r, k - get_size(p->l), a->r, b);
            push_up(a);
          } else {
            b = p;
            split_size(p->l, k, a, b->l);
            push_up(b);
          }
        }
      }
    
      void insert(int val) {
        node *a, *b;
        split_val(root, val, a, b);
        a = merge(a, new node(val));
        root = merge(a, b);
      }
    
      int query(int val) {
        node *a, *b;
        split_val(root, val, a, b);
        int res = get_size(a);
        root = merge(a, b);
        return res;
      }
    
      int qry(int l, int r) { return query(r) - query(l - 1); }
    };
    
    // Cây phân đoạn
    Treap T[N << 2];
    
    void insert(int x, int l, int r, int p, int val) {
      T[x].insert(val);
      if (l == r) return;
      int mid = (l + r) >> 1;
      if (p <= mid)
        insert(x << 1, l, mid, p, val);
      else
        insert(x << 1 | 1, mid + 1, r, p, val);
    }
    
    int query(int x, int l, int r, int L, int R, int val) {
      if (l == L && r == R) return T[x].query(val);
      int mid = (l + r) >> 1;
      if (R <= mid) return query(x << 1, l, mid, L, R, val);
      if (L > mid) return query(x << 1 | 1, mid + 1, r, L, R, val);
      return query(x << 1, l, mid, L, mid, val) +
             query(x << 1 | 1, mid + 1, r, mid + 1, R, val);
    }
    
    int query(int l, int r, int val) {
      if (l > r) return -1;
      return query(1, 1, n, l, r, val);
    }
    
    int main() {
      scanf("%d", &n);
      for (int i = 1; i <= n; ++i) scanf("%d", &a[i]);
      for (int i = 1; i <= n; ++i) g[a[i]].push_back(i);
    
      // a_0 và a_{n+1} là nút lính canh
      int ans = n + 2, lst, ok;
      for (int i = 1; i <= n + 1; ++i) {
        g[i].push_back(n + 1);
    
        lst = 0;
        ok = 0;
        for (int pos : g[i]) {
          if (query(lst + 1, pos - 1, lst) == i - 1) {
            ok = 1;
            break;
          }
          lst = pos;
        }
    
        if (!ok) {
          ans = i;
          break;
        }
    
        lst = 0;
        g[i].pop_back();
        for (int pos : g[i]) {
          insert(1, 1, n, pos, lst);
          lst = pos;
        }
      }
      printf("%d\n", ans);
      return 0;
    }
    ```