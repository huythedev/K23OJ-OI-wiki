author: countercurrent-time, StudyingFather

Từ thế kỷ trước, IOI đã có bài tương tác. Dù gần đây các kỳ thi dưới tỉnh选 ít gặp, năm 2019 loạt NOI liên tiếp出现 hai bài tương tác: 《P5208[WC2019]I 君的商店》 và 《P5473[NOI2019]I 君的探险》, có thể cho thấy bài tương tác quay lại.

Bài tương tác không đòi thuật toán tiền đề cao, thường cũng không có giới hạn thời gian nghiêm ngặt, chất lượng chương trình chủ yếu phụ thuộc số lần tương tác. Vì vậy nên học theo độ难 tăng dần. Nếu muốn rèn tư duy thuật toán, làm bài tương tác là cách tốt. Dù yêu cầu thuật toán không cao, vẫn建议 học thuật toán mức提高 và省选 trước rồi mới做, vì lúc đó tư duy và kiến thức đủ. Giới thiệu cơ bản có thể xem **OI Wiki** [题型介绍 - 交互题](./problems.md#交互题).

Lỗi đặc thù của tương tác:

-   Sau mỗi output phải flush,否则 gây Idleness limit exceeded. Nếu nhiều test và có thể biết đáp án trước khi đọc hết input,仍 phải đọc hết dữ liệu,否则 cũng gây ILE (có thể hỏi nhiều lần, nhận trả lời một lần). Đồng thời尽量不要用 fast input.
-   Nếu truy vấn quá nhiều, Codeforces cho WA (và nêu原因); UVa cho Protocol Limit Exceeded (PLE).
-   Nếu format tương tác sai, UVa cho Protocol Violation (PV).

Vì I/O tương tác rườm rà,建议 bọc hàm nhập/xuất riêng.

Trong thi nếu ra đề cung cấp header grader (cho grader debug) hoặc checker (cho stdio debug), việc debug dễ hơn; vì đối拍 bài tương tác khó hơn nhiều so với bài thường. Không có `testlib.h` thì库 stdio tương tác với nhiều细节 thường dài 3k dòng, cộng đối拍 3k dòng,至少 cần 1 giờ. Dù có hay không có工具, debug tương tác thường phải mô phỏng tương tác; nên thí sinh cần thiết kế chương trình chất lượng cao,尽量 một lần đúng, và có khả năng rà lỗi tĩnh tốt.

Ví dụ:

-   [CF679A Bear and Prime 100](https://codeforces.com/problemset/problem/679/A)
-   [CF843B Interactive LowerBound](https://codeforces.com/problemset/problem/843/B)
-   [UOJ206[APIO2016]Gap](http://uoj.ac/problem/206)
-   [CF750F New Year and Finding Roots](https://codeforces.com/problemset/problem/750/F)
-   [UVa12731 太空站之谜 Mysterious Space Station](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=823&page=show_problem&problem=4584)

## CF679A Bear and Prime 100

Mỗi số nguyên tố chỉ có hai ước, nên ta trực tiếp liệt kê ước của số cần đoán. Do chỉ được hỏi 20 lần, và với số lớn (như 92) phân tích cần liệt kê tới $\lfloor\frac{n}{2}\rfloor$ số nguyên tố, nên ta sàng các số nguyên tố ≤ 50 và hỏi lần lượt.

Đối拍 dễ, có thể thử toàn bộ giá trị trong miền. Sẽ发现 chương trình không xử lý tốt平方 của số nguyên tố. Vì vậy thêm 4,9,25,49 (bình phương của 2,3,5,7), tổng 19 số, đúng đề.

??? note "参考代码"
    ```cpp
    #include <cstdio>
    constexpr int prime[] = {2,  3,  4,  5,  7,  9,  11, 13, 17, 19,
                             23, 25, 29, 31, 37, 41, 43, 47, 49};
    int cnt = 0;
    char res[5];
    
    int main() {
      for (int i : prime) {
        printf("%d\n", i);
        fflush(stdout);
        scanf("%s", res);
        if (res[0] == 'y' && ++cnt == 2) return printf("composite"), 0;
      }
      printf("prime");
      return 0;
    }
    ```

## CF843B Interactive LowerBound

Danh sách liên kết最多 $5 \times 10 ^ 4$ phần tử, chỉ được hỏi 1999 lần và chỉ biết node tiếp theo, nên không thể duyệt toàn bộ. Cách duy nhất là rải điểm ngẫu nhiên.

Với $n < 2000$ thì liệt kê, với $n \ge 2000$ thì rải 1000 điểm. Khi đó khoảng cách kỳ vọng nhỏ, ta bắt đầu từ giá trị lớn nhất < $x$ và duyệt; có thể chứng minh sẽ gặp答案 trước khi chạm điểm tiếp theo. Trong quá trình duyệt,遇到 ≥ $x$ là ra答案.

Ý tưởng đơn giản nhưng nếu chưa học các kỹ thuậtRandom không hoàn hảo như mô phỏng annealing, sẽ khá khó nghĩ.

Vì Codeforces có hack, nhiều người故意卡 code không init seed, nên trước `random_shuffle()` cần `srand((size_t)new char)`.

??? note "参考代码"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstdlib>
    constexpr int N = 50005;
    int n, start, x;
    int a[N];
    
    int main() {
      scanf("%d%d%d", &n, &start, &x);
      if (n < 2000) {
        int ans = 2e9;
        for (int i = 1; i <= n; i++) {
          printf("? %d\n", i), fflush(stdout);
          int val, next;
          scanf("%d%d", &val, &next);
          if (val >= x) ans = std::min(ans, val);
        }
        if (ans == 2e9) ans = -1;
        printf("! %d", ans), fflush(stdout);
      } else {
        srand((size_t) new char);
        int p = start, ans = 0;
        for (int i = 1; i <= n; i++) a[i] = i;
        std::random_shuffle(a + 1, a + n + 1);
        for (int i = 1; i <= 1000; i++) {
          printf("? %d\n", a[i]), fflush(stdout);
          int val, next;
          scanf("%d%d", &val, &next);
          if (val < x && val > ans) p = a[i], ans = val;
        }
        while (p != -1 && ans < x) {
          printf("? %d\n", p), fflush(stdout);
          int val, next;
          scanf("%d%d", &val, &next);
          ans = val;
          p = next;
        }
        if (ans < x) ans = -1;
        printf("! %d", ans), fflush(stdout);
      }
      return 0;
    }
    ```

## UOJ206[APIO2016]Gap

Chia hai subtask:

1.  Giới hạn số truy vấn.
    
    Lần truy vấn đầu: chưa biết gì nên hỏi $[1, 10 ^ {18}]$ để lấy min/max.
    
    Vì số truy vấn giới hạn $\frac{N + 1}{2}$, ta cần mỗi lần lấy giá trị chưa lấy. Cách đơn giản: hỏi $[s,t]$ nhận $mn, mx$, lần sau hỏi $[mn+1, mx-1]$.

2.  Giới hạn độ dài đoạn hỏi.
    
    Vì tổng số phần tử trong các đoạn hỏi không vượt $3N$, nên cần tối thiểu hóa đoạn hỏi. Cách trên không còn dùng vì tổng规模 $O(N^2)$. Có thể nghĩ đến nhị phân giá trị, nhưng không可靠, worst-case vẫn $O(N^2)$. Cần chia miền giá trị hiệu quả để tránh hỏi trùng.
    
    Nhận xét: đáp án ≥ $\lfloor\frac{a_n - a_1}{N - 1}\rfloor$. Ta chia miền theo giá trị này: đặt $i$ ban đầu 0, $ans$ là giá trị trên, mỗi lần hỏi $[i, i+ans]$ và cập nhật $ans$, rồi tăng $i$ theo bước $ans$.
    
    Cách này không phù hợp subtask 1 vì có thể nhiều đoạn không có số nào.

??? note "参考代码"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    
    #include "gap.h"
    
    long long findGap(int T, int N) {
      static long long a[100005] = {}, ans = 0;
      long long s = 0, t = 1e18, s1, t1;
      if (T == 1) {
        int l = 1, r = N;
        while (l <= r) {
          MinMax(s, t, &s1, &t1);
          a[l++] = s1, a[r--] = t1;
          s = s1 + 1, t = t1 - 1;
        }
        for (int i = 2; i <= N; i++) ans = std::max(ans, a[i] - a[i - 1]);
      } else if (T == 2) {
        MinMax(s, t, &s1, &t1);
        ans = (t1 - s1) / (N - 1);
        long long l = s1 + 1, r = t1, last = s1;
        for (long long i = l; i <= r;) {
          MinMax(i, i + ans, &s1, &t1);
          i += ans + 1;
          if (s1 != -1) ans = std::max(ans, s1 - last), last = t1;
        }
      }
      return ans;
    }
    ```

## CF750F New Year and Finding Roots

Vì $h \le 7$ và giới hạn 16 truy vấn nghiêm, cần tận dụng tối đa thông tin.

$h \le 4$ có thể vét cạn. Khi $h > 4$ cần thuật toán duyệt hiệu quả.

Rải điểm ngẫu nhiên không tốt vì không biết是否 đủ gần gốc, và xác suất碰到根 rất nhỏ.

Do $1 \le k \le 3$ và không biết hướng nào gần gốc, xét worst-case: nếu $k=3$, hai lần đầu đi xa gốc, lần thứ ba mới gần gốc. Vì vậy phải duyệt theo ba hướng.

Xét BFS và DFS; BFS có cây tìm kiếm lớn, ưu先 DFS. Nếu biết độ sâu hiện tại nhỏ và cây BFS trong phạm vi số truy vấn còn lại, có thể BFS.

Biết độ sâu vàDirection sẽ có ưu thế, nhưng biết đang đi về gốc hay về lá rất khó. Nếu DFS, chỉ khi đến gốc ($k=2$) hoặc lá ($k=1$) mới biết hướng. Vì vậy cần尽量 biết độ sâu hiện tại và không dùng迭代加深, tránh dừng giữa chừng.

Chọn ngẫu nhiên một节点 bắt đầu có thể rơi vào worst-case.

Nếu $k=1$, ta biết độ sâu.

Nếu $k=2$,当前就是根.

Nếu $k=3$, ta DFS theo ba hướng. Hai hướng là về lá (độ dài đường đi giống nhau), một hướng về gốc nhưng có thể走错 về lá,路径更长. Khi đó có thể tính độ sâu.

Khi $k=1$ hoặc $k=3$, đường đi dài. Ta biếtNodes có độ sâu nhỏ nhất trên路径 (nhỏ hơn điểm bắt đầu). Nếu đánh dấu已访问, từ nodes đó chỉ còn mộtDirection, dù có thể vẫn走向叶, trên path vẫn có nodes có độ sâu nhỏ hơn; ta lặp lại步骤.

Xét worst-case $h=7$ (mỗi lần chỉ về gốc 1 bước rồi về lá), DFS đơn thuần cần $\frac{(1 + 7) \times 7}{2} = 28$ truy vấn. Nhưng đã biết độ sâu ban đầu, có thể算 ra深度 của已访问 nodes và决定是否 BFS từ nodes depth nhỏ nhất.

Khi đó, worst-case cần 17. Ta có thể减少 1 truy vấn: trong BFS sâu $k$, cây có $2^k-1$ nodes, cần $2^k-1$ truy vấn để确定 nodes có 2 neighbors. Nhưng nếu已经问了 $2^k-2$ nodes,最后一个 node必是根.

Do đó最优: $h=7$ 时，从叶子 DFS，每次只向根走一步 rồi về叶, 询问 10 次后，已知最小深度为 4; biết父节点 nên从父节点 BFS (depth 3，nodes $2^3-1=7$). BFS hỏi $2^3-2=6$ 次即可确定最后一个是根.

最坏 16 次刚好。

??? note "参考代码"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <queue>
    #include <vector>
    using namespace std;
    constexpr int N = 256 + 5;
    int T, h, chance;
    bool ok;
    vector<int> to[N], path;
    
    bool read(int x) {
      if (to[x].empty()) {
        printf("? %d\n", x), fflush(stdout);
        int k, t;
        scanf("%d", &k);
        if (k == 0) exit(0);
        for (int i = 0; i < k; i++) {
          scanf("%d", &t);
          to[x].push_back(t);
        }
        if (k == 2) {
          printf("! %d\n", x), fflush(stdout);
          return ok = true;
        }
        chance--;
      }
      return false;
    }
    
    bool dfs(int x) {
      if (to[x].empty()) path.push_back(x);
      if (read(x)) return true;
      for (int i : to[x])
        if (to[i].empty()) return dfs(i);
      return false;
    }
    
    void bfs(int s, int k) {
      queue<int> q;
      for (int i : to[s])
        if (to[i].empty()) q.push(i);
      for (int i = 1; i < k; i++) {
        int x = q.front();
        q.pop();
        if (read(x)) return;
        for (int j : to[x])
          if (to[j].empty()) q.push(j);
      }
      for (int i = 1; i < k; i++) {
        int x = q.front();
        q.pop();
        if (read(x)) return;
      }
      printf("! %d\n", q.front()), fflush(stdout);
    }
    
    int main() {
      for (scanf("%d", &T); T--;) {
        ok = false;
        for (int i = 0; i < N; i++) to[i].clear();
        chance = 16;
        scanf("%d", &h);
        if (h == 0) exit(0);
        vector<int> long_path;
        if (read(1)) continue;
        int root, dep;
        if (to[1].size() == 1)
          root = 1, dep = h;
        else {
          for (int i : to[1]) {
            path.clear();
            if (dfs(i)) break;
            if (path.size() > long_path.size()) swap(path, long_path);
          }
          if (ok) continue;
          dep = h - (path.size() + long_path.size()) / 2;
          root = long_path.at((long_path.size() - (h - dep)) - 1);
        }
        while ((1 << (dep - 1)) - 2 > chance) {
          path.clear();
          if (dfs(root)) break;
          dep = h - (h - dep + path.size()) / 2;
          root = path.at((path.size() - (h - dep)) - 1);
        }
        if (!ok) bfs(root, 1 << (dep - 2));
      }
      return 0;
    }
    ```

## UVa12731 太空站之谜 Mysterious Space Station

Vì phản hồi duy nhất là có撞墙 hay không, nên尽量 đi sát tường để:

-   Dễ biết có撞墙 không,获取更多信息.
-   Các ô sát tường không có portal, tránh机器人走丢.

Nếu已知 robot có thể ở một vị trí sát tường, có thể dùng [“đi theo tường một bên”](https://en.wikipedia.org/wiki/Maze_solving_algorithm) để xác định. Theo tôpô, trong迷宫两 bên đều là tường, nếu từ入口 và luôn bám một bên tường, sẽ找到出口. Vì tường trong bài khép kín, chỉ cần沿 tường đi là quay về điểm xuất phát mà không撞墙. Thực tế không cần cố撞墙, có thể đánh dấu các ô sát tường. Nếu撞墙, cần quay lại ngay để tránh走丢 và减少步数.

Suy ra: để xác định robot có ở ô X, đưa robot ra đường tường rồi đi một vòng; nếu không撞墙 thì确实 ở X.

Dùng方法 này:先标 tất cả ô未知, rồi từ trên xuống, trái qua phải判断 từng ô是否是 portal. Có thể先到 ô trên của nó, đi xuống, sang trái, rồi用方法 trên判断 robot có ở bên trái hay không. Nếu không, tức robot不在 vị trí dự kiến ⇒ ô là portal.

Xác định 2k ô portal xong cần匹配; vì $k \le 5$,最多只需 $9+7+5+3$ lần thử. So với判断所有未知 ô,最多 $121-40$ lần.

Do code dưới chỉ qua bản镜像 UOJ [#247.【Rujia Liu's Present 7】Mysterious Space Station](http://uoj.ac/problem/247), không qua UVa gốc. Sửa标程 của Liu Rujia trên UOJ vẫn不行,且无法联系. Vì vậy code dưới以 UOJ 为准.

Dù vậy,标程 của Liu Rujia chất lượng tốt hơn nhiều; có thể xem [submission](http://uoj.ac/submission/105789). Cùng dữ liệu,标程 dùng rất ít bước di chuyển.

??? note "参考代码"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    #include <iostream>
    #include <queue>
    #include <stack>
    
    #define Wall 0
    #define Unknown 1
    #define Space 2
    #define Gate 3
    #define Path 4
    
    const int N = 20;
    const int dir[8][2] = {{0, 1},  {1, 0}, {0, -1}, {-1, 0},
                           {-1, 1}, {1, 1}, {1, -1}, {-1, -1}};
    const char dirs[5] = "ESWN";
    int n, m, k;
    int a[N][N], id[N][N];
    
    struct point {
      int x, y;
    
      point(int x = 0, int y = 0) : x(x), y(y) {}
    
      bool operator==(const point& tmp) const { return x == tmp.x && y == tmp.y; }
    
      bool operator!=(const point& tmp) const { return !(*this == tmp); }
    
      point side(int d) const { return point(x + dir[d][0], y + dir[d][1]); }
    
      int check(int d) { return a[x + dir[d][0]][y + dir[d][1]]; }
    
      int id() { return ::id[x][y]; }
    } start;
    
    std::vector<std::pair<point, int>> path;
    std::pair<point, point> ans[N];
    std::pair<point, bool> vis[N];
    
    bool walk(int d) {
      printf("MoveRobot %c\n", dirs[d]);
      fflush(stdout);
      int ret;
      scanf("%d", &ret);
      return ret;
    }
    
    bool walk(int d, std::stack<int>& st) {
      if (walk(d)) {
        st.push(d);
        return true;
      }
      return false;
    }
    
    bool read() {
      if (scanf("%d%d%d", &n, &m, &k) != 3) return false;
      if (n == 0) return false;
      memset(a, 0, sizeof(a));
      for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++) {
          char c;
          std::cin >> c;
          if (c == 'S') start = point(i, j);
          if (c == '*')
            a[i][j] = Wall;
          else
            a[i][j] = Unknown;
        }
      return true;
    }
    
    void answer() {
      for (int i = 0; i < k; i++)
        printf("Answer %d %d\n", ans[i].first.id(), ans[i].second.id());
      fflush(stdout);
    }
    
    // Đi theo một bên tường; vì Path sát tường là vòng khép kín lớn nhất,
    // chỉ cần đi沿 Path mà không撞障碍 là được
    void wall_follower_init(point x, int last, int wallside, point s) {
      if (x == s && !path.empty()) return;
      if (x.check(wallside) == Path) {
        path.push_back(std::make_pair(x, wallside));
        wall_follower_init(x.side(wallside), wallside, last ^ 2, s);
      } else if (x.check(last) == Wall) {
        for (int i = 0; i < 4; i++)
          if (i != (last ^ 2) && x.check(i) != Wall) {
            path.push_back(std::make_pair(x, i));
            wall_follower_init(x.side(i), i, last, s);
            return;
          }
      } else {
        path.push_back(std::make_pair(x, last));
        wall_follower_init(x.side(last), last, wallside, s);
      }
    }
    
    void init() {
      int cnt = 1;
      for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++) {
          if (a[i][j] == Unknown) {
            id[i][j] = cnt++;
            for (int k = 0; k < 8; k++)
              if (point(i, j).check(k) == Wall) {
                a[i][j] = Path;
                break;
              }
          } else
            id[i][j] = 0;
        }
      path.clear();
      int wallside = 0, last = 0;
      for (int i = 0; i < 4; i++)
        if (start.check(i) == Wall) {
          wallside = i;
          break;
        }
      for (int i = 0; i < 4; i++)
        if (start.check(i) == Path && i != (wallside ^ 2)) {
          last = i;
          break;
        }
      wall_follower_init(start, last, wallside, start);
    }
    
    void undo(std::stack<int>& st) {
      while (!st.empty()) walk(st.top() ^ 2), st.pop();
    }
    
    bool wall_follower(point x) {
      std::stack<int> st;
      bool ok = true;
      int i = 0;
      while (i < path.size() && path[i].first != x) i++;
      for (int j = i; ok && j < path.size(); j++) {
        if (walk(path[j].second))
          st.push(path[j].second);
        else
          ok = false;
      }
      for (int j = 0; ok && j < i; j++) {
        if (walk(path[j].second))
          st.push(path[j].second);
        else
          ok = false;
      }
      if (!ok) undo(st);
      return ok;
    }
    
    // Xác định hiện tại ở x; dùng “mò đá qua sông” để đi đến Path
    // Tránh障碍, ô未知 và portal; dùng khi tìm portal và ghép portal
    void bfs(point s, point t, std::vector<int>& v) {
      static int map[N][N] = {};
      memset(map, -1, sizeof(map));
      std::queue<point> q;
      map[s.x][s.y] = 4;
      q.push(s);
      while (!q.empty()) {
        point x = q.front();
        q.pop();
        if (x == t) break;
        for (int i = 0; i < 4; i++) {
          point y = x.side(i);
          if ((x.check(i) == Path || x.check(i) == Space) && map[y.x][y.y] == -1) {
            map[y.x][y.y] = i;
            q.push(y);
          }
        }
      }
      for (point x = t; x != s; x = x.side(map[x.x][x.y] ^ 2)) {
        v.push_back(map[x.x][x.y]);
      }
      std::reverse(v.begin(), v.end());
    }
    
    bool move(point s, point t, std::stack<int>& st) {  // Dùng khi áp sát portal
      static std::vector<int> v;
      v.clear();
      bfs(s, t, v);
      for (int i : v)
        if (!walk(i, st)) return false;
      return true;
    }
    
    // Di chuyển nhanh nhất về tường
    bool make_sure(point x, int last) {
      if (a[x.x][x.y] == Path) return wall_follower(x);
      for (int i = 0; i < 4; i++)
        if ((x.check(i) == Path || x.check(i) == Space) && i != (last ^ 2)) {
          if (!walk(i)) return false;
          bool ret = make_sure(x.side(i), i);
          walk(i ^ 2);
          return ret;
        }
      return false;
    }
    
    void find_gate() {
      int cnt = 0;
      std::stack<int> st;
      for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
          if (cnt == k * 2 && a[i][j] == Unknown)
            a[i][j] = Space;
          else if (a[i][j] == Unknown) {
            bool ok = true;
            if (!move(start, point(i - 1, j), st))
              ok = false;
            else if (!walk(1, st))
              ok = false;
            else if (!walk(2, st))
              ok = false;
            else if (!make_sure(point(i, j - 1), -1))
              ok = false;
            if (!ok) {
              vis[cnt++] = std::make_pair(point(i, j), false);
              a[i][j] = Gate;
              for (int k = 0; k < 8; k++) {
                point y = point(i, j).side(k);
                if (point(i, j).check(k) == Unknown) a[y.x][y.y] = Space;
              }
            } else
              a[i][j] = Space;
            undo(st);
          }
    }
    
    void make_gate_pair() {
      int cnt = 0;
      std::stack<int> st;
      for (int i = 0; i < k * 2; i++)
        if (!vis[i].second)
          for (int j = 0; !vis[i].second && j < k * 2; j++)
            if (j != i && !vis[j].second) {
              bool ok = true;
              if (!move(start, vis[i].first.side(2), st))
                ok = false;
              else if (!walk(0, st))
                ok = false;
              else if (!make_sure(vis[j].first.side(0), -1))
                ok = false;
              if (ok) {
                ans[cnt++] = std::make_pair(vis[i].first, vis[j].first);
                vis[i].second = vis[j].second = true;
              }
              undo(st);
            }
    }
    
    int main() {
      while (read()) {
        init();
        find_gate();
        make_gate_pair();
        answer();
      }
      return 0;
    }
    ```

## Bài tập

-   [Rujia Liu's Present 7 (chuyên đề tương tác) rất chất lượng, nên làm.](https://onlinejudge.org/contests/328-9976a2e2/)
-   [P5473[NOI2019]I 君的探险](https://www.luogu.com.cn/problem/P5473)
-   [P5208[WC2019]I 君的商店](https://www.luogu.com.cn/problem/P5208)

## Tài liệu tham khảo và đọc thêm

-   [Dùng Linux pipe để实现 tương tác cho online judge](https://www.cnblogs.com/tsreaper/p/pipe-interactive.html)
