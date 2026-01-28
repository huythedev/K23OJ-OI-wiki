Trang này sẽ giới thiệu ngắn gọn về Quy hoạch động trên số (Số vị DP, Digit DP).

## Dẫn nhập

"Số vị" nghĩa là tách một số ra thành từng chữ số theo từng hàng đơn vị, chục, trăm, nghìn,... và quan tâm đến giá trị của từng chữ số đó. Nếu tách theo hệ thập phân, mỗi chữ số sẽ nằm trong khoảng 0~9, các hệ cơ số khác cũng tương tự.

**Số vị DP (Digit DP):** là kỹ thuật dùng để giải quyết một lớp bài toán đặc biệt, thường có các đặc điểm sau:

1.  Yêu cầu đếm số lượng các số thỏa mãn điều kiện nào đó (tức là mục tiêu cuối cùng là đếm số lượng);

2.  Các điều kiện này sau khi chuyển đổi có thể sử dụng tư duy "số vị" để kiểm tra và đánh giá;

3.  Đầu vào thường cho một đoạn số (đôi khi chỉ cho giới hạn trên);

4.  Giới hạn trên rất lớn (ví dụ $10^{18}$), vét cạn sẽ bị TLE.

**Nguyên lý cơ bản của số vị DP:**

Xét cách con người đếm số, cách đơn giản nhất là tăng dần từng số một. Nhưng với các số nhiều chữ số, quá trình này có nhiều phần lặp lại. Ví dụ, đếm từ 7000 đến 7999, từ 8000 đến 8999, từ 9000 đến 9999, ta thấy ba đoạn này đều có ba chữ số cuối chạy từ 000 đến 999, chỉ khác nhau ở chữ số hàng nghìn. Ta có thể gom các trường hợp này lại, lưu kết quả đếm vào một mảng chung. Mảng này (thường gọi là mảng trạng thái) sẽ được thiết kế tùy theo yêu cầu bài toán, và dùng quy hoạch động hoặc truy hồi để chuyển trạng thái.

Trong số vị DP, ta thường dùng các kỹ thuật đếm thông thường, ví dụ tách đoạn $[l, r]$ thành hai phần rồi lấy hiệu: $\mathit{ans}_{[l, r]} = \mathit{ans}_{[0, r]}-\mathit{ans}_{[0, l - 1]}$.

Sau khi có mảng trạng thái chung, việc còn lại là thống kê đáp án. Có thể dùng truy hồi có nhớ (memoization) hoặc lặp lại (iterative DP). Để không bị trùng hoặc thiếu, ta sẽ duyệt từng chữ số từ cao xuống thấp, xét mỗi chữ số có thể điền giá trị nào, rồi dùng mảng trạng thái để cộng dồn đáp án.

Tiếp theo, ta sẽ xem xét một số bài toán cụ thể.

## Ví dụ 1

???+ note "Ví dụ 1 [Luogu P2602 Đếm chữ số](https://www.luogu.com.cn/problem/P2602)"
    Đề bài: Cho hai số nguyên dương $a, b$, hãy đếm trong đoạn $[a, b]$ mỗi chữ số (digit) xuất hiện bao nhiêu lần.

### Cách 1

#### Giải thích

Nhận thấy với các số có đúng $\mathit{i}$ chữ số, mỗi chữ số xuất hiện số lần như nhau. Đặt mảng $\mathit{dp}_i$ là số lần mỗi chữ số xuất hiện trong tất cả các số có $i$ chữ số (chưa xét số 0 ở đầu). Khi đó, có công thức: $\mathit{dp}_i=10 \times \mathit{dp}_{i−1}+10^{i−1}$, trong đó phần đầu là đóng góp từ các chữ số trước, phần sau là đóng góp từ chữ số hiện tại.

Sau khi có mảng $\mathit{dp}$, ta sẽ tách giới hạn trên thành từng chữ số, duyệt từ cao xuống thấp. Nếu không "bám sát" giới hạn trên, các chữ số phía sau có thể điền tự do. Nếu bám sát, chỉ được điền từ 0 đến giá trị của giới hạn trên ở vị trí đó. Cuối cùng, cần xử lý trường hợp số 0 ở đầu: khi chữ số thứ $i$ là 0 ở đầu, tức là các chữ số trước cũng đều là 0, ta đã đếm thừa các trường hợp này, cần trừ đi.

#### Cài đặt

???+ note "Code tham khảo"
    ```cpp
    #include <cstdio>
    using namespace std;
    constexpr int N = 15;
    using ll = long long;
    ll l, r, dp[N], mi[N];
    ll ans1[N], ans2[N];
    int a[N];
    
    void solve(ll n, ll *ans) {
      ll tmp = n;
      int len = 0;
      while (n) a[++len] = n % 10, n /= 10;
      for (int i = len; i >= 1; --i) {
        for (int j = 0; j < 10; j++) ans[j] += dp[i - 1] * a[i];
        for (int j = 0; j < a[i]; j++) ans[j] += mi[i - 1];
        tmp -= mi[i - 1] * a[i], ans[a[i]] += tmp + 1;
        ans[0] -= mi[i - 1];
      }
    }
    
    int main() {
      scanf("%lld%lld", &l, &r);
      mi[0] = 1ll;
      for (int i = 1; i <= 13; ++i) {
        dp[i] = dp[i - 1] * 10 + mi[i - 1];
        mi[i] = 10ll * mi[i - 1];
      }
      solve(r, ans1), solve(l - 1, ans2);
      for (int i = 0; i < 10; ++i) printf("%lld ", ans1[i] - ans2[i]);
      return 0;
    }
    ```

### Cách 2

#### Giải thích

Bài này cũng có thể dùng truy hồi có nhớ (memoization). $\mathit{dp}_i$ là đáp án với số có $i$ chữ số, không bám sát giới hạn trên và không có số 0 ở đầu.

Chi tiết xem trong chú thích code.

#### Quy trình

???+ note "Code tham khảo"
    ```cpp
    #include <cstdio>
    #include <cstring>
    #include <iostream>
    using namespace std;
    using ll = long long;
    constexpr int N = 50005;
    ll a, b;
    ll f[15], ksm[15], p[15], now[15];
    
    ll dfs(int u, int x, bool f0,
           bool lim) {  // u: số chữ số còn lại, f0: có phải đang ở số 0 đầu, lim: có bám sát giới hạn trên không
      if (!u) {
        if (f0) f0 = false;
        return 0;
      }
      if (!lim && !f0 && (~f[u])) return f[u];
      ll cnt = 0;
      int lst = lim ? p[u] : 9;
      for (int i = 0; i <= lst; i++) {  // Duyệt chữ số hiện tại
        if (f0 && i == 0)
          cnt += dfs(u - 1, x, 1, lim && i == lst);  // Xử lý số 0 đầu
        else if (i == x && lim && i == lst)
          cnt += now[u - 1] + 1 +
                 dfs(u - 1, x, 0,
                     lim && i == lst);  // Trường hợp các chữ số trước đều bám sát giới hạn trên
        else if (i == x)
          cnt += ksm[u - 1] + dfs(u - 1, x, 0, lim && i == lst);
        else
          cnt += dfs(u - 1, x, 0, lim && i == lst);
      }
      if ((!lim) && (!f0)) f[u] = cnt;  // Chỉ lưu khi không bám sát giới hạn trên và không có số 0 đầu
      return cnt;
    }
    
    ll gans(ll d, int dig) {
      int len = 0;
      memset(f, -1, sizeof(f));
      while (d) {
        p[++len] = d % 10;
        d /= 10;
        now[len] = now[len - 1] + p[len] * ksm[len - 1];
      }
      return dfs(len, dig, 1, 1);
    }
    
    int main() {
      scanf("%lld%lld", &a, &b);
      ksm[0] = 1;
      for (int i = 1; i <= 12; i++) ksm[i] = ksm[i - 1] * 10ll;
      for (int i = 0; i < 9; i++) printf("%lld ", gans(b, i) - gans(a - 1, i));
      printf("%lld\n", gans(b, 9) - gans(a - 1, 9));
      return 0;
    }
    ```

## Ví dụ 2

???+ note "Ví dụ 2 [HDU 2089 Không có 62](https://acm.hdu.edu.cn/showproblem.php?pid=2089)"
    Đề bài: Đếm số lượng số trong một đoạn mà không có chữ số 4 và không có cặp liên tiếp 62.

### Giải thích

Không có số 4 thì khi duyệt chỉ cần bỏ qua 4 là đảm bảo hợp lệ, không cần nhớ trạng thái. Nhưng với cặp 62, liên quan đến hai chữ số liên tiếp, nên trạng thái phải ghi nhớ chữ số trước có phải là 6 hay không. Đặt $\mathit{dp}_{\mathit{pos},\mathit{sta}}$ với $\mathit{sta}$ chỉ cần 0 hoặc 1 (không phải 6 hoặc là 6).

### Cài đặt

???+ note "Code tham khảo"
    ```cpp
    #include <cstdio>
    #include <cstring>
    #include <iostream>
    using namespace std;
    int x, y, dp[15][3], p[50];
    
    void pre() {
      memset(dp, 0, sizeof(dp));
      dp[0][0] = 1;
      for (int i = 1; i <= 10; i++) {
        dp[i][0] = dp[i - 1][0] * 9 - dp[i - 1][1];
        dp[i][1] = dp[i - 1][0];
        dp[i][2] = dp[i - 1][2] * 10 + dp[i - 1][1] + dp[i - 1][0];
      }
    }
    
    int cal(int x) {
      int cnt = 0, ans = 0, tmp = x;
      while (x) {
        p[++cnt] = x % 10;
        x /= 10;
      }
      bool flag = false;
      p[cnt + 1] = 0;
      for (int i = cnt; i; i--) {  // Duyệt từng chữ số từ cao xuống thấp
        ans += p[i] * dp[i - 1][2];
        if (flag)
          ans += p[i] * dp[i - 1][0];
        else {
          if (p[i] > 4) ans += dp[i - 1][0];
          if (p[i] > 6) ans += dp[i - 1][1];
          if (p[i] > 2 && p[i + 1] == 6) ans += dp[i][1];
          if (p[i] == 4 || (p[i] == 2 && p[i + 1] == 6)) flag = true;
        }
      }
      return tmp - ans;
    }
    
    int main() {
      pre();
      while (~scanf("%d%d", &x, &y)) {
        if (!x && !y) break;
        if (x > y) swap(x, y);
        printf("%d\n", cal(y + 1) - cal(x));
      }
      return 0;
    }
    ```

## Ví dụ 3

???+ note "Ví dụ 3 [SCOI2009 windy số](https://loj.ac/problem/10165)"
    Đề bài: Cho đoạn $[l,r]$, đếm số lượng số **không có số 0 đầu và hai chữ số liên tiếp chênh lệch ít nhất 2**.

### Giải thích

Chuyển bài toán về dạng: đặt $\mathit{ans}_i$ là số lượng số thỏa mãn trong $[1,i]$, đáp án là $\mathit{ans}_r-\mathit{ans}_{l-1}$.

Với một số nhỏ hơn $n$, khi duyệt từ cao xuống thấp, sẽ có một vị trí mà chữ số ở đó nhỏ hơn chữ số tương ứng của $n$, các vị trí trước đó đều giống $n$.

Ta định nghĩa $f(i,st,op)$ là số lượng số khi đang ở vị trí $i$ (từ cao xuống), trạng thái trước là $st$, và $op$ là quan hệ với giới hạn trên ($op=1$ là bằng, $op=0$ là nhỏ hơn). Ở bài này, trạng thái là giá trị chữ số trước. Ở các bài khác, trạng thái có thể là tổng chữ số, gcd, phần dư,... hoặc kết hợp nhiều thứ.

**Công thức chuyển:**
$$
f(i,st,op)=\sum_{k=1}^{\mathit{maxx}} f(i+1,k,op=1~ \operatorname{and}~ k=\mathit{maxx} )\quad (|\mathit{st}-k|\ge 2)
$$

Ở đây $k$ là chữ số hiện tại, $\mathit{maxx}$ là giá trị lớn nhất có thể điền ở vị trí đó (nếu bám sát giới hạn trên thì là chữ số tương ứng của $n$, ngược lại là 9).

Có thể dùng truy hồi có nhớ để tránh tính trùng.

### Cài đặt

???+ note "Code tham khảo"
    ```cpp
    int dfs(int x, int st, int op)  // op=1: bằng giới hạn trên; op=0: nhỏ hơn
    {
      if (!x) return 1;
      if (!op && ~f[x][st]) return f[x][st];
      int maxx = op ? dim[x] : 9, ret = 0;
      for (int i = 0; i <= maxx; i++) {
        if (abs(st - i) < 2) continue;
        if (st == 11 && i == 0)
          ret += dfs(x - 1, 11, op & (i == maxx));
        else
          ret += dfs(x - 1, i, op & (i == maxx));
      }
      if (!op) f[x][st] = ret;
      return ret;
    }
    
    int solve(int x) {
      memset(f, -1, sizeof f);
      dim.clear();
      dim.push_back(-1);
      int t = x;
      while (x) {
        dim.push_back(x % 10);
        x /= 10;
      }
      return dfs(dim.size() - 1, 11, 1);
    }
    ```

## Ví dụ 4

???+ note "Ví dụ 4.[SPOJMYQ10](https://www.spoj.com/problems/MYQ10/en/)"
    Đề bài: Nếu viết tất cả các số trong đoạn $[n,m]$, có bao nhiêu số nhìn trong gương vẫn giống hệt như ban đầu? ($n,m<10^{44}, T<10^5$)

### Giải thích

Chú ý: chỉ có $0,1,8$ là đối xứng gương với chính nó. Vậy "giống hệt" ở đây là số chỉ gồm $0,1,8$ và là số đối xứng (palindrome).

Trong quá trình số vị DP, chỉ được chọn $0,1,8$.

Vì số rất lớn, không thể dùng $[1,m]-[1,n-1]$ như thường lệ (so sánh số lớn phức tạp), mà phải kiểm tra riêng $n$ có hợp lệ không: $[n,m]=[1,m]-[1,n]+\mathrm{check}(n)$.

Để kiểm tra đối xứng, cần lưu lại các chữ số đã chọn. Khi chưa đi quá nửa, chỉ cần không vượt giới hạn trên; khi đã đi quá nửa, phải kiểm tra đối xứng với vị trí tương ứng.

Lưu ý: phần memoization không dùng `memset` để tránh TLE.

### Cài đặt

???+ note "Code tham khảo"
    ```cpp
    int check(char cc[]) {  // Kiểm tra riêng cho n
      int strc = strlen(cc);
      for (int i = 0; i < strc; ++i) {
        if (!(cc[i] == cc[strc - i - 1] &&
              (cc[i] == '1' || cc[i] == '8' || cc[i] == '0')))
          return 0ll;
      }
      return 1ll;
    }
    
    // now: vị trí hiện tại, eff: số chữ số, fulc: có bám sát giới hạn trên không, ful0: có phải toàn số 0 đầu không
    int dfs(int now, int eff, bool ful0, bool fulc) {
      if (now == 0) return 1ll;
      if (!fulc && f[now][eff][ful0] != -1)  // Memoization
        return f[now][eff][ful0];
    
      int res = 0, maxk = fulc ? dig[now] : 9;
      for (int i = 0; i <= maxk; ++i) {
        if (i != 0 && i != 1 && i != 8) continue;
        b[now] = i;
        if (ful0 && i == 0)  // Toàn số 0 đầu
          res += dfs(now - 1, eff - 1, 1, 0);
        else if (now > eff / 2)                                  // Chưa đi quá nửa
          res += dfs(now - 1, eff, 0, fulc && (dig[now] == i));  // Đã đi quá nửa
        else if (b[now] == b[eff - now + 1])
          res += dfs(now - 1, eff, 0, fulc && (dig[now] == i));
      }
      if (!fulc) f[now][eff][ful0] = res;
      return res;
    }
    
    char cc1[100], cc2[100];
    int strc, ansm, ansn;
    
    int get(char cc[]) {  // Hàm xử lý
      strc = strlen(cc);
      for (int i = 0; i < strc; ++i) dig[strc - i] = cc[i] - '0';
      return dfs(strc, strc, 1, 1);
    }
    
    scanf("%s%s", cc1, cc2);
    printf("%lld\n", get(cc2) - get(cc1) + check(cc1));
    ```

## Ví dụ 5

???+ note "Ví dụ 5.[P3311 Đếm số](https://www.luogu.com.cn/problem/P3311)"
    Đề bài: Gọi một số nguyên dương $x$ là số may mắn nếu trong biểu diễn thập phân của nó không chứa bất kỳ chuỗi nào trong tập $S$ dưới dạng chuỗi con. Ví dụ $S = \{22, 333, 0233\}$ thì $233233$ là số may mắn, còn $23332333$, $2023320233$, $32233223$ không phải. Cho $n$ và $S$, đếm số lượng số may mắn không vượt quá $n$. Kết quả lấy mod $10^9 + 7$.

    $1 \leq n<10^{1201}, 1 \leq m \leq 100, 1 \leq \sum_{i = 1}^m |s_i| \leq 1500, \min_{i = 1}^m |s_i| \geq 1$. $n$ không có số 0 đầu, nhưng $s_i$ có thể có.

### Giải thích

Nhận thấy nếu coi số là chuỗi, đây là bài toán nhiều mẫu (multi-pattern matching), nên dùng tự động hóa AC (AC Automaton).

Trong số vị DP thông thường, ta duyệt từng chữ số từ cao xuống thấp, mỗi lần xét điền chữ số nào. Ở bài này, trạng thái còn bao gồm vị trí hiện tại trên cây AC. Đặt $f(i,j,0/1)$ là số cách đã điền $i$ chữ số, đang ở nút $j$ trên AC, và có bám sát giới hạn trên không.

Điều kiện "không chứa" chỉ cần đánh dấu các nút kết thúc mẫu trên AC, nếu gặp thì bỏ qua.

Chuyển trạng thái khá trực tiếp, xem chi tiết trong code.

### Cài đặt

???+ note "Code tham khảo"
    ```cpp
    #include <cstdio>
    #include <cstring>
    #include <queue>
    using namespace std;
    using ll = long long;
    constexpr int N = 1505;
    constexpr int mod = 1000000007;
    int n, m;
    char s[N], c[N];
    int ch[N][10], fail[N], ed[N], tot, len;
    
    void insert() {
      int now = 0;
      int L = strlen(s);
      for (int i = 0; i < L; ++i) {
        if (!ch[now][s[i] - '0']) ch[now][s[i] - '0'] = ++tot;
        now = ch[now][s[i] - '0'];
      }
      ed[now] = 1;
    }
    
    queue<int> q;
    
    void build() {
      for (int i = 0; i < 10; ++i)
        if (ch[0][i]) q.push(ch[0][i]);
      while (!q.empty()) {
        int u = q.front();
        q.pop();
        for (int i = 0; i < 10; ++i) {
          if (ch[u][i]) {
            fail[ch[u][i]] = ch[fail[u]][i], q.push(ch[u][i]),
            ed[ch[u][i]] |= ed[fail[ch[u][i]]];
          } else
            ch[u][i] = ch[fail[u][i]];
        }
      }
      ch[0][0] = 0;
    }
    
    ll f[N][N][2], ans;
    
    void add(ll &x, ll y) { x = (x + y) % mod; }
    
    int main() {
      scanf("%s", c);
      n = strlen(c);
      scanf("%d", &m);
      for (int i = 1; i <= m; ++i) scanf("%s", s), insert();
      build();
      f[0][0][1] = 1;
      for (int i = 0; i < n; ++i) {
        for (int j = 0; j <= tot; ++j) {
          if (ed[j]) continue;
          for (int k = 0; k < 10; ++k) {
            if (ed[ch[j][k]]) continue;
            add(f[i + 1][ch[j][k]][0], f[i][j][0]);
            if (k < c[i] - '0') add(f[i + 1][ch[j][k]][0], f[i][j][1]);
            if (k == c[i] - '0') add(f[i + 1][ch[j][k]][1], f[i][j][1]);
          }
        }
      }
      for (int j = 0; j <= tot; ++j) {
        if (ed[j]) continue;
        add(ans, f[n][j][0]);
        add(ans, f[n][j][1]);
      }
      printf("%lld\n", ans - 1);
      return 0;
    }
    ```

Bài này giúp hiểu sâu hơn về nguyên lý số vị DP.

## Bài tập

[Ahoi2009 self Phân phối đồng loại](https://www.luogu.com.cn/problem/P4127)

[Luogu  P3413 SAC#1 - Số Moe](https://www.luogu.com.cn/problem/P3413)

[HDU 6148 Valley Number](https://acm.hdu.edu.cn/showproblem.php?pid=6148)

[CF55D Beautiful numbers](http://codeforces.com/problemset/problem/55/D)

[CF628D Magic Numbers](http://codeforces.com/problemset/problem/628/D)
