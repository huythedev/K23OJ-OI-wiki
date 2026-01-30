## Điểm phân chia (Point Centroid Decomposition)

Điểm phân chia (hay còn gọi là phân chia theo trọng tâm, centroid decomposition) rất phù hợp để xử lý các bài toán về thông tin đường đi trên cây quy mô lớn.

??? note "Ví dụ 1 [Luogu P3806【Mẫu】Điểm phân chia 1](https://www.luogu.com.cn/problem/P3806)"
    Cho một cây có $n$ đỉnh, $m$ truy vấn, mỗi truy vấn cho một số $k$, hỏi có tồn tại một đường đi độ dài $k$ hay không.

    $n\le 10000,m\le 100,k\le 10000000$

Ta chọn một đỉnh bất kỳ làm gốc $\mathit{rt}$, mọi đường đi hoàn toàn nằm trong cây con của nó có thể chia thành hai loại: loại đi qua gốc hiện tại và loại không đi qua gốc. Với các đường đi qua gốc, lại chia thành hai loại: một đầu là gốc, hoặc cả hai đầu không phải gốc (loại sau có thể ghép từ hai đường đi loại trước). Do đó, với mỗi gốc $rt$, ta tính trước đóng góp của các đường đi qua $rt$, sau đó đệ quy xử lý các cây con cho các đường đi không qua $rt$.

Trong bài này, với các đường đi qua $rt$, ta liệt kê từng con $ch$ của $rt$, tính khoảng cách từ mọi đỉnh trong cây con $ch$ đến $rt$. Gọi $\mathit{dist}_i$ là khoảng cách từ $i$ đến $rt$, $\mathit{tf}_d$ là mảng đánh dấu xem đã có đỉnh nào ở các cây con trước có khoảng cách $d$ đến $rt$ chưa. Nếu với truy vấn $k$ có $tf_{k-\mathit{dist}_i}=true$, tức là tồn tại một đường đi độ dài $k$. Sau khi xử lý xong cây con $ch$, cập nhật các giá trị mới vào $\mathit{tf}$.

Khi xóa mảng $\mathit{tf}$, không nên dùng `memset`, mà nên lưu lại các vị trí đã dùng vào một hàng đợi để xóa, đảm bảo đúng độ phức tạp.

Trong mỗi tầng của phân chia, tổng số lần xử lý mỗi đỉnh là $1$, nếu tổng cộng đệ quy $h$ tầng thì độ phức tạp $O(hn)$.

Nếu mỗi lần chọn trọng tâm ([centroid](./tree-centroid.md)) làm gốc, số tầng tối đa là $O(\log n)$, tổng độ phức tạp $O(n\log n)$. Vì vậy, phương pháp này còn gọi là **trọng tâm phân chia** (centroid decomposition).

Lưu ý: mỗi lần chọn lại gốc phải tính lại kích thước cây con, nếu không sẽ sai độ phức tạp hoặc sai kết quả.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-divide/tree-divide_1.cpp"
    ```

??? note "Ví dụ 2 [Luogu P4178 Tree](https://www.luogu.com.cn/problem/P4178)"
    Cho một cây có $n$ đỉnh, $k$, hỏi số cặp đỉnh có khoảng cách không vượt quá $k$.

    $n\le 40000,k\le 20000,w_i\le 1000$

Vì ở đây hỏi số cặp đỉnh có khoảng cách không vượt quá $k$, ta dùng segment tree để hỗ trợ truy vấn và cập nhật.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-divide/tree-divide_2.cpp"
    ```

??? note "Ví dụ 3 [Luogu P2664 Trò chơi trên cây](https://www.luogu.com.cn/problem/P2664)"
    Một cây mỗi đỉnh được gán một màu, định nghĩa $s(i,j)$ là số màu trên đường đi từ $i$ đến $j$, $\mathit{sum_{i}}=\sum_{j=1}^n s(i,j)$．Đối với mọi $1\leq i\leq n$，tìm $sum_i$．（$1 \le n, c_i \le 10^5$）

Bài này kiểm tra sâu về tư duy điểm phân chia, rất phù hợp luyện tập nâng cao.

Trước hết, cần chuyển đổi ý nghĩa của $\mathit{sum_i}$. Nếu tính trực tiếp như đề, rất khó hợp nhất thông tin giữa các cây con. Ta chuyển sang xét đóng góp của từng màu $j$ cho $\mathit{sum_i}$, gọi $\mathit{cnt_j}$ là số đường đi qua $i$ có màu $j$, khi đó $\mathit{sum_i} = \sum \mathit{cnt_j}$. Để tính $\mathit{cnt_j}$, chỉ cần mỗi khi gặp màu mới thì $\mathit{cnt_{col_u}}+=\mathit{size_u}$, với $\mathit{size_u}$ là kích thước cây con gốc $u$.

Trong quá trình điểm phân chia, cần thống kê:

1.  Đường đi có một đầu là gốc, đóng góp cho gốc.
2.  Đường đi có LCA là gốc, đóng góp cho các đỉnh trong cây con.

Phần 1 dễ xử lý, vì mỗi tầng chỉ cần duyệt toàn bộ cây con, dùng định nghĩa $\mathit{sum_i}$ để cộng dồn.

Với phần 2, giả sử gốc $u$ có con $d$, chọn $v$ trong cây con $d$. Khi đó, đáp án cho $v$ gồm:

1.  Số màu xuất hiện trên đường $(u, v)$, gọi là $\mathit{num}$, nhân với tổng kích thước các cây con khác $d$ của $u$, gọi là $\mathit{siz1}$, đóng góp là $\mathit{num}\times \mathit{siz1}$.
2.  Với màu $j$ không xuất hiện trên $(u, v)$, cộng thêm tổng $\mathit{cnt_j}$ của các cây con khác $d$.

Chi tiết xem code tham khảo.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-divide/tree-divide_3.cpp"
    ```

## Phân chia theo cạnh (Edge Centroid Decomposition)

Tương tự điểm phân chia, nhưng chọn một cạnh để chia cây thành hai phần có kích thước gần nhau nhất, rồi đệ quy xử lý hai phần.

Tuy nhiên, cách này không hiệu quả với cây nhiều nhánh như cây sao:

![菊花图](./images/tree-divide1.svg)

Nếu một đỉnh có nhiều con kích thước gần nhau, phân chia theo cạnh sẽ rất tệ.

Nếu cây là nhị phân, sẽ tránh được vấn đề này. Ta có thể chuyển cây đa nhánh thành cây nhị phân như xây segment tree:

![建树](./images/tree-divide2.svg)

Các đỉnh mới gán thông tin phù hợp với bài toán. Ví dụ, khi tính độ dài đường đi, gán trọng số cạnh gốc là $1$, cạnh mới là $0$.

Tổng số đỉnh tăng tối đa $O(n)$, nên độ phức tạp vẫn $O(n\log n)$.

Hầu hết các bài điểm phân chia đều có thể giải bằng phân chia theo cạnh (thường hằng số lớn hơn, nhưng không bị "hack" nặng), nên không cần ví dụ riêng.

## Cây phân chia (Centroid Tree)

Cây phân chia là cây được xây lại từ cây gốc bằng cách phân chia theo trọng tâm, sao cho chiều cao cây mới là $O(\log n)$.

Thường dùng cho các bài toán có truy vấn cập nhật động, không phụ thuộc hình dạng cây gốc.

### Phân tích thuật toán

Mỗi lần tìm trọng tâm, liên kết nó với trọng tâm tầng trước thành cha-con, tạo thành cây mới có tối đa $\log n$ tầng.

Nhờ vậy, nhiều thuật toán brute-force trên cây gốc sẽ chạy đúng và nhanh trên cây phân chia.

### Cài đặt

Một mẹo nhỏ: mỗi lần truyền tổng kích thước tầng trước trừ đi kích thước con nặng nhất, sẽ ra tổng kích thước tầng hiện tại. Như vậy chỉ cần một DFS để tìm trọng tâm.

???+ note "Code tham khảo"
    ```cpp
    #include <algorithm>
    #include <iostream>
    #include <vector>
    using namespace std;
    
    using IT = vector<int>::iterator;
    
    struct Edge {
      int to, nxt, val;
    
      Edge() {}
    
      Edge(int to, int nxt, int val) : to(to), nxt(nxt), val(val) {}
    } e[300010];
    
    int head[150010], cnt;
    
    void addedge(int u, int v, int val) {
      e[++cnt] = Edge(v, head[u], val);
      head[u] = cnt;
    }
    
    int siz[150010], son[150010];
    bool vis[150010];
    
    int tot, lasttot;
    int maxp, root;
    
    void getG(int now, int fa) {
      siz[now] = 1;
      son[now] = 0;
      for (int i = head[now]; i; i = e[i].nxt) {
        int vs = e[i].to;
        if (vs == fa || vis[vs]) continue;
        getG(vs, now);
        siz[now] += siz[vs];
        son[now] = max(son[now], siz[vs]);
      }
      son[now] = max(son[now], tot - siz[now]);
      if (son[now] < maxp) {
        maxp = son[now];
        root = now;
      }
    }
    
    struct Node {
      int fa;
      vector<int> anc;
      vector<int> child;
    } nd[150010];
    
    int build(int now, int ntot) {
      tot = ntot;
      maxp = 0x7f7f7f7f;
      getG(now, 0);
      int g = root;
      vis[g] = true;
      for (int i = head[g]; i; i = e[i].nxt) {
        int vs = e[i].to;
        if (vis[vs]) continue;
        int tmp = build(vs, ntot - son[vs]);
        nd[tmp].fa = now;
        nd[now].child.push_back(tmp);
      }
      return g;
    }
    
    int virtroot;
    
    int main() {
      int n;
      cin >> n;
      for (int i = 1; i < n; i++) {
        int u, v, val;
        cin >> u >> v >> val;
        addedge(u, v, val);
        addedge(v, u, val);
      }
      virtroot = build(1, n);
    }
    ```
