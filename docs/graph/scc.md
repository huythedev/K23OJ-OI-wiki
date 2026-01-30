## Giới thiệu

Trước khi đọc phần dưới đây, bạn cần nắm vững [các khái niệm cơ bản về đồ thị](./concept.md).

Định nghĩa mạnh liên thông: Một đồ thị có hướng $G$ được gọi là mạnh liên thông nếu với mọi cặp đỉnh bất kỳ trong $G$ đều có đường đi từ đỉnh này đến đỉnh kia.

Thành phần liên thông mạnh (Strongly Connected Components, SCC) là: tiểu đồ thị mạnh liên thông lớn nhất.

Dưới đây sẽ trình bày các thuật toán tìm thành phần liên thông mạnh.

## Thuật toán Tarjan

### Giới thiệu

Robert E. Tarjan (sinh năm 1948 tại Pomona, California, Mỹ) là một nhà khoa học máy tính nổi tiếng.

Tarjan đã phát minh ra nhiều thuật toán và cấu trúc dữ liệu. Nhiều thuật toán mang tên ông, đôi khi gây nhầm lẫn giữa các thuật toán khác nhau như thuật toán Tarjan tìm các loại thành phần liên thông, thuật toán Tarjan tìm tổ tiên chung thấp nhất (LCA). Các cấu trúc như Union-Find, Splay, Toptree cũng do Tarjan phát minh.

Ở đây, chúng ta sẽ tìm hiểu thuật toán Tarjan tìm thành phần liên thông mạnh trên đồ thị có hướng.

### Cây sinh DFS

Trước khi vào thuật toán, hãy tìm hiểu về **cây sinh DFS** qua ví dụ đồ thị có hướng dưới đây:

![DFS 生成树](./images/dfs-tree.svg)

Trong cây sinh DFS của đồ thị có hướng, có 4 loại cạnh chính (không nhất thiết phải xuất hiện đầy đủ):

1.  Cạnh cây (tree edge): Màu đen trên hình, mỗi lần DFS gặp một đỉnh chưa thăm sẽ sinh ra cạnh cây.
2.  Cạnh ngược (back edge): Màu đỏ trên hình (ví dụ $7 \rightarrow 1$), còn gọi là cạnh hồi, là cạnh đi về tổ tiên trong cây DFS.
3.  Cạnh ngang (cross edge): Màu xanh dương trên hình (ví dụ $9 \rightarrow 7$), là cạnh đi tới một đỉnh đã thăm nhưng không phải tổ tiên của đỉnh hiện tại.
4.  Cạnh tiến (forward edge): Màu xanh lá trên hình (ví dụ $3 \rightarrow 6$), là cạnh đi tới một đỉnh con trong cây DFS.

Hãy xem xét mối liên hệ giữa cây sinh DFS và thành phần liên thông mạnh.

Nếu đỉnh $u$ là đỉnh đầu tiên của một thành phần liên thông mạnh được gặp trong cây DFS, thì các đỉnh còn lại của thành phần này chắc chắn nằm trong cây con gốc $u$. Đỉnh $u$ được gọi là gốc của thành phần liên thông mạnh đó.

Chứng minh phản chứng: Giả sử có đỉnh $v$ thuộc thành phần liên thông mạnh này nhưng không nằm trong cây con gốc $u$, thì trên đường đi từ $u$ đến $v$ phải có một cạnh rời khỏi cây con này. Nhưng cạnh như vậy chỉ có thể là cạnh ngang hoặc cạnh ngược, mà hai loại cạnh này đều yêu cầu đỉnh đích đã được thăm, mâu thuẫn với giả thiết $v$ chưa nằm trong cây con gốc $u$. Vậy điều phải chứng minh.

### Thuật toán Tarjan tìm thành phần liên thông mạnh

Thuật toán Tarjan dựa trên [duyệt sâu (DFS)](./dfs.md). Mỗi thành phần liên thông mạnh tương ứng với một cây con trong cây DFS, trong quá trình duyệt sẽ duy trì một ngăn xếp, mỗi đỉnh chưa xử lý sẽ được đưa vào ngăn xếp.

Với mỗi đỉnh $u$, thuật toán Tarjan duy trì các biến sau:

1.  $\textit{dfn}_u$: Thứ tự duyệt DFS khi thăm $u$.
2.  $\textit{low}_u$: Trong cây con gốc $u$, có thể quay lại đỉnh nào sớm nhất vẫn còn trong ngăn xếp. Gọi cây con gốc $u$ là $\textit{Subtree}_u$. $\textit{low}_u$ là giá trị nhỏ nhất của $\textit{dfn}$ trong các đỉnh thuộc $\textit{Subtree}_u$ và các đỉnh có thể đi tới từ $\textit{Subtree}_u$ qua cạnh không thuộc cây DFS.

Các đỉnh trong cây con của một đỉnh có dfn lớn hơn dfn của đỉnh đó.

Trên một đường đi từ gốc, dfn tăng chặt, low không giảm.

Duyệt DFS toàn bộ đồ thị, cập nhật dfn và low cho từng đỉnh, mỗi đỉnh được đưa vào ngăn xếp. Khi tìm được một thành phần liên thông mạnh, sẽ lấy các đỉnh tương ứng ra khỏi ngăn xếp. Khi xét cạnh $(u, v)$ (với $v$ không phải cha của $u$), có 3 trường hợp:

1.  $v$ chưa được thăm: Tiếp tục DFS với $v$, sau đó cập nhật $\textit{low}_u$ bằng $\min(\textit{low}_u, \textit{low}_v)$. Vì có đường đi trực tiếp từ $u$ đến $v$, nên mọi đỉnh $v$ có thể quay lại, $u$ cũng có thể.
2.  $v$ đã được thăm và còn trong ngăn xếp: Cập nhật $\textit{low}_u$ bằng $\min(\textit{low}_u, \textit{dfn}_v)$.
3.  $v$ đã được thăm và không còn trong ngăn xếp: $v$ đã xử lý xong, không cần làm gì.

Thuật toán dạng giả mã:

???+ note "Cài đặt"
    ```text
    TARJAN_SEARCH(int u)
        vis[u]=true
        low[u]=dfn[u]=++dfncnt
        push u to the stack
        for each (u,v) then do
            if v hasn't been searched then
                TARJAN_SEARCH(v) // Duyệt tiếp
                low[u]=min(low[u],low[v]) // Quay lui
            else if v has been in the stack then
                low[u]=min(low[u],dfn[v])
    ```

Với mỗi thành phần liên thông mạnh, chỉ có duy nhất một đỉnh $u$ thỏa $\textit{dfn}_u=\textit{low}_u$. Đỉnh này là đỉnh đầu tiên của thành phần liên thông mạnh được thăm trong DFS, vì dfn và low của nó nhỏ nhất, không bị ảnh hưởng bởi các đỉnh khác trong thành phần.

Do đó, khi quay lui, nếu $\textit{dfn}_u=\textit{low}_u$, thì các đỉnh từ $u$ trở lên trong ngăn xếp tạo thành một SCC.

### Cài đặt

=== "C++"
    ```cpp
    int dfn[N], low[N], dfncnt, s[N], in_stack[N], tp;
    int scc[N], sc;  // Số hiệu SCC của đỉnh i
    int sz[N];       // Kích thước SCC i
    
    void tarjan(int u) {
      low[u] = dfn[u] = ++dfncnt, s[++tp] = u, in_stack[u] = 1;
      for (int i = h[u]; i; i = e[i].nex) {
        const int &v = e[i].t;
        if (!dfn[v]) {
          tarjan(v);
          low[u] = min(low[u], low[v]);
        } else if (in_stack[v]) {
          low[u] = min(low[u], dfn[v]);
        }
      }
      if (dfn[u] == low[u]) {
        ++sc;
        do {
          scc[s[tp]] = sc;
          sz[sc]++;
          in_stack[s[tp]] = 0;
        } while (s[tp--] != u);
      }
    }
    ```

=== "Python"
    ```python
    dfn = [0] * N
    low = [0] * N
    dfncnt = 0
    s = [0] * N
    in_stack = [0] * N
    tp = 0
    scc = [0] * N
    sc = 0  # Số hiệu SCC của đỉnh i
    sz = [0] * N  # Kích thước SCC i
    
    
    def tarjan(u):
        low[u] = dfn[u] = dfncnt
        s[tp] = u
        in_stack[u] = 1
        dfncnt = dfncnt + 1
        tp = tp + 1
        i = h[u]
        while i:
            v = e[i].t
            if dfn[v] == False:
                tarjan(v)
                low[u] = min(low[u], low[v])
            elif in_stack[v]:
                low[u] = min(low[u], dfn[v])
            i = e[i].nex
        if dfn[u] == low[u]:
            sc = sc + 1
            while s[tp] != u:
                scc[s[tp]] = sc
                sz[sc] = sz[sc] + 1
                in_stack[s[tp]] = 0
                tp = tp - 1
            scc[s[tp]] = sc
            sz[sc] = sz[sc] + 1
            in_stack[s[tp]] = 0
            tp = tp - 1
    ```

Độ phức tạp thời gian $O(n + m)$.

### Quan hệ giữa số hiệu thành phần và thứ tự topo

Thuật toán Tarjan thực chất phát hiện các thành phần liên thông mạnh theo **thứ tự topo ngược**. Bởi vì DFS sẽ kết thúc ở các đỉnh không còn cạnh ra trước, điều này ngược với quá trình sắp xếp topo.

Nếu co mỗi thành phần liên thông mạnh thành một đỉnh, thì đồ thị mới là DAG. Thứ tự topo của DAG này sẽ ngược lại với thứ tự số hiệu SCC mà Tarjan trả về.

Nói cách khác, **thứ tự số hiệu SCC (sau khi co) là thứ tự topo ngược** của DAG các SCC. Lưu ý, điều này chỉ đúng khi xét quan hệ giữa các SCC (các cạnh giữa các SCC), còn bên trong SCC thì không có thứ tự topo do tồn tại chu trình.

## Thuật toán Kosaraju

### Giới thiệu

Thuật toán Kosaraju được S. Rao Kosaraju đề xuất năm 1978 (trong một bài báo chưa xuất bản), nhưng Micha Sharir là người đầu tiên công bố.

### Quy trình

Thuật toán này dựa trên hai lần DFS đơn giản:

Lần DFS thứ nhất, chọn một đỉnh bất kỳ làm gốc, duyệt toàn bộ các đỉnh chưa thăm, và đánh số đỉnh theo thứ tự hậu tố (sau khi quay lui).

Lần DFS thứ hai, trên đồ thị đảo ngược, bắt đầu từ đỉnh có số hậu tố lớn nhất, mỗi lần DFS sẽ tìm ra một thành phần liên thông mạnh. Lặp lại với các đỉnh chưa thăm.

Sau hai lần DFS, các thành phần liên thông mạnh sẽ được tìm ra. Độ phức tạp thuật toán là $O(n+m)$.

### Cài đặt

=== "C++"
    ```cpp
    // g là đồ thị gốc, g2 là đồ thị đảo ngược
    
    void dfs1(int u) {
      vis[u] = true;
      for (int v : g[u])
        if (!vis[v]) dfs1(v);
      s.push_back(u);
    }
    
    void dfs2(int u) {
      color[u] = sccCnt;
      for (int v : g2[u])
        if (!color[v]) dfs2(v);
    }
    
    void kosaraju() {
      sccCnt = 0;
      for (int i = 1; i <= n; ++i)
        if (!vis[i]) dfs1(i);
      for (int i = n; i >= 1; --i)
        if (!color[s[i]]) {
          ++sccCnt;
          dfs2(s[i]);
        }
    }
    ```

=== "Python"
    ```python
    def dfs1(u):
        vis[u] = True
        for v in g[u]:
            if vis[v] == False:
                dfs1(v)
        s.append(u)
    
    
    def dfs2(u):
        color[u] = sccCnt
        for v in g2[u]:
            if color[v] == False:
                dfs2(v)
    
    
    def kosaraju(u):
        sccCnt = 0
        for i in range(1, n + 1):
            if vis[i] == False:
                dfs1(i)
        for i in range(n, 0, -1):
            if color[s[i]] == False:
                sccCnt = sccCnt + 1
                dfs2(s[i])
    ```

## Thuật toán Garbow

### Quy trình

Thuật toán Garbow là một biến thể khác của Tarjan. Nếu Tarjan dùng dfn và low để xác định gốc SCC, thì Garbow dùng hai ngăn xếp: một ngăn xếp lưu các đỉnh, một ngăn xếp phụ để xác định khi nào các đỉnh thuộc cùng một SCC sẽ được lấy ra khỏi ngăn xếp chính. Khi DFS từ đỉnh $w$, nếu một đường đi cho thấy các đỉnh thuộc cùng một SCC, chỉ cần ngăn xếp phụ có đỉnh với thời gian thăm lớn hơn $w$, thì loại bỏ đỉnh đó khỏi ngăn xếp phụ, cuối cùng chỉ còn lại $w$. Mỗi đỉnh bị loại khỏi ngăn xếp chính trong quá trình này đều thuộc cùng một SCC.

Khi quay lui về một đỉnh $w$, nếu $w$ nằm trên đỉnh ngăn xếp phụ, thì các đỉnh được thăm sau $w$ đều thuộc cùng một SCC, và sẽ được lấy ra khỏi ngăn xếp chính.

### Cài đặt

=== "C++"
    ```cpp
    int garbow(int u) {
      stack1[++p1] = u;
      stack2[++p2] = u;
      low[u] = ++dfs_clock;
      for (int i = head[u]; i; i = e[i].next) {
        int v = e[i].to;
        if (!low[v])
          garbow(v);
        else if (!sccno[v])
          while (low[stack2[p2]] > low[v]) p2--;
      }
      if (stack2[p2] == u) {
        p2--;
        scc_cnt++;
        do {
          sccno[stack1[p1]] = scc_cnt;
          // all_scc[scc_cnt] ++;
        } while (stack1[p1--] != u);
      }
      return 0;
    }
    
    void find_scc(int n) {
      dfs_clock = scc_cnt = 0;
      p1 = p2 = 0;
      memset(sccno, 0, sizeof(sccno));
      memset(low, 0, sizeof(low));
      for (int i = 1; i <= n; i++)
        if (!low[i]) garbow(i);
    }
    ```

=== "Python"
    ```python
    def garbow(u):
        stack1[p1] = u
        stack2[p2] = u
        p1 = p1 + 1
        p2 = p2 + 1
        low[u] = dfs_clock
        dfs_clock = dfs_clock + 1
        i = head[u]
        while i:
            v = e[i].to
            if low[v] == False:
                garbow(v)
            elif sccno[v] == False:
                while low[stack2[p2]] > low[v]:
                    p2 = p2 - 1
        if stack2[p2] == u:
            p2 = p2 - 1
            scc_cnt = scc_cnt + 1
            while stack1[p1] != u:
                p1 = p1 - 1
                sccno[stack1[p1]] = scc_cnt
    
    
    def find_scc(n):
        dfs_clock = scc_cnt = 0
        p1 = p2 = 0
        sccno = []
        low = []
        for i in range(1, n + 1):
            if low[i] == False:
                garbow(i)
    ```

## Ứng dụng

Ta có thể co mỗi thành phần liên thông mạnh của đồ thị thành một đỉnh.

Khi đó, đồ thị trở thành DAG, có thể sắp xếp topo và thực hiện nhiều thao tác khác.

Ví dụ đơn giản: Tìm một đường đi (có thể đi qua đỉnh trùng lặp), sao cho số lượng đỉnh khác nhau đi qua là lớn nhất.

## Bài tập

[USACO Fall/HAOI 2006 Những con bò được yêu thích](https://loj.ac/problem/10091)

[POJ1236 Network of Schools](http://poj.org/problem?id=1236)
