author:ouuan, Backl1ght, billchenchina, CCXXXI, ChickenHu, ChungZH, cjsoft, countercurrent-time, diauweb, Early0v0, Enter-tainer, EtaoinWu, H-J-Granger, H-Shen, Henry-ZHR, HeRaNO, hsfzLZH1, huaruoji, iamtwz, imp2002, Ir1d, kenlig, Konano, Lyccrius, Marcythm, Menci, NachtgeistW, PeterlitsZo, psz2007, shuzhouliu, SkqLiao, sshwy, SukkaW, therehello, TrisolarisHD, ttzztztz, vincent-163, WAAutoMaton, Hunter19019

## Định nghĩa

Tổ tiên chung gần nhất, viết tắt là LCA (Lowest Common Ancestor). Với hai đỉnh bất kỳ, tổ tiên chung gần nhất là tổ tiên chung nằm xa gốc nhất (gần hai đỉnh nhất).
Để thuận tiện, ta ký hiệu tổ tiên chung gần nhất của tập $S=\{v_1,v_2,\ldots,v_n\}$ là $\text{LCA}(v_1,v_2,\ldots,v_n)$ hoặc $\text{LCA}(S)$.

## Tính chất

> Phần **tính chất** này được dịch và chỉnh sửa từ [wcipeg](http://wcipeg.com/wiki/Lowest_common_ancestor).

1.  $\text{LCA}(\{u\})=u$;
2.  $u$ là tổ tiên của $v$ khi và chỉ khi $\text{LCA}(u,v)=u$;
3.  Nếu $u$ không là tổ tiên của $v$ và $v$ không là tổ tiên của $u$, thì $u,v$ nằm ở hai cây con khác nhau của $\text{LCA}(u,v)$;
4.  Trong phép duyệt tiền thứ tự, $\text{LCA}(S)$ xuất hiện trước tất cả các phần tử của $S$; trong phép duyệt hậu thứ tự, $\text{LCA}(S)$ xuất hiện sau tất cả các phần tử của $S$;
5.  Tổ tiên chung gần nhất của hợp hai tập là tổ tiên chung gần nhất của hai tổ tiên chung gần nhất của từng tập, tức là $\text{LCA}(A\cup B)=\text{LCA}(\text{LCA}(A), \text{LCA}(B))$;
6.  Tổ tiên chung gần nhất của hai đỉnh luôn nằm trên đường đi ngắn nhất giữa hai đỉnh đó trên cây;
7.  $d(u,v)=h(u)+h(v)-2h(\text{LCA}(u,v))$, trong đó $d$ là khoảng cách giữa hai đỉnh trên cây, $h$ là khoảng cách từ đỉnh đến gốc.

## Các phương pháp tìm LCA

### Thuật toán đơn giản

#### Quy trình

Có thể mỗi lần chọn đỉnh có độ sâu lớn hơn để nhảy lên trên. Rõ ràng trên cây, hai đỉnh cuối cùng sẽ gặp nhau, vị trí gặp chính là LCA cần tìm.
Hoặc có thể đưa hai đỉnh về cùng độ sâu trước, sau đó cùng nhảy lên, cuối cùng cũng sẽ gặp nhau.

#### Độ phức tạp

Thuật toán đơn giản cần duyệt DFS toàn bộ cây để tiền xử lý, độ phức tạp $O(n)$, mỗi truy vấn mất $\Theta(n)$. Nếu cây có tính chất ngẫu nhiên, độ phức tạp phụ thuộc vào chiều cao kỳ vọng của cây.

### Thuật toán nhảy bậc (Binary Lifting)

#### Quy trình

Thuật toán nhảy bậc là phương pháp kinh điển nhất để tìm LCA, cải tiến từ thuật toán đơn giản. Bằng cách tiền xử lý mảng $\text{fa}_{x,i}$, con trỏ có thể nhảy xa hơn, giảm mạnh số lần nhảy. $\text{fa}_{x,i}$ là tổ tiên thứ $2^i$ của đỉnh $x$. Mảng này có thể tính bằng DFS.

Cách tối ưu hóa:
Ở giai đoạn đầu, cần đưa $u,v$ về cùng độ sâu. Tính hiệu độ sâu $y$, phân tích nhị phân $y$ để thực hiện số lần nhảy bằng số bit 1 trong $y$.
Giai đoạn hai, từ $i$ lớn nhất về $0$, nếu $\text{fa}_{u,i}\not=\text{fa}_{v,i}$ thì cập nhật $u\gets\text{fa}_{u,i},v\gets\text{fa}_{v,i}$. Cuối cùng, LCA là $\text{fa}_{u,0}$.

#### Độ phức tạp

Tiền xử lý $O(n \log n)$, mỗi truy vấn $O(\log n)$.
Có thể đổi thứ tự hai chiều của mảng `fa` để tối ưu cache.

??? note "Ví dụ"
    [HDU 2586 How far away?](https://acm.hdu.edu.cn/showproblem.php?pid=2586) - Truy vấn đường đi ngắn nhất trên cây.

Có thể tìm LCA trước rồi dùng tính chất 7 để trả lời, hoặc tính luôn khi tìm LCA.

??? note "Mã mẫu"
    ```cpp
    --8<-- "docs/graph/code/lca/lca_1.cpp"
    ```

### Thuật toán Tarjan

#### Quy trình

Thuật toán Tarjan là **thuật toán offline**, cần dùng [DSU - cấu trúc hợp nhất-tìm đại diện](../ds/dsu.md) để lưu tổ tiên của mỗi đỉnh. Cách làm:

1.  Nhập các cạnh (dạng danh sách kề), các truy vấn (lưu ở một danh sách kề khác). Mỗi truy vấn thêm cả hai chiều vào mảng `queryEdge`.
2.  Thực hiện DFS, dùng mảng `visited` để đánh dấu đã thăm, `parent` lưu cha hiện tại.
3.  Áp dụng **tư duy quay lui**: khi duyệt đến một đỉnh, coi nó là gốc của chính nó. Sau khi DFS xong toàn bộ cây con, cập nhật gốc của nó là cha của nó.
4.  Khi quay lui, nếu truy vấn xuất phát từ đỉnh này và đỉnh còn lại đã được thăm, cập nhật kết quả LCA.
5.  In kết quả.

#### Độ phức tạp

Cần khởi tạo DSU, tiền xử lý $O(n)$.

Thuật toán Tarjan cơ bản xử lý $m$ truy vấn trong $O(m \alpha(m+n, n) + n)$, nhưng hằng số lớn hơn thuật toán nhảy bậc. Có thể đạt $O(m+n)$.

???+ warning "Chú ý"
    Không có chuyện “DSU trong Tarjan LCA có truy vấn `find()` $O(1)$”. 
    Thuật toán Tarjan cơ bản có độ phức tạp $O(m \alpha(m+n, n) + n)$. Nếu muốn tuyến tính nghiêm ngặt, xem [bài báo của Gabow và Tarjan 1983](https://dl.acm.org/doi/pdf/10.1145/800061.808753) với cách $O(m+n)$.

#### Mã mẫu

??? note "Mã mẫu"
    ```cpp
    --8<-- "docs/graph/code/lca/lca_tarjan.cpp"
    ```

### Dùng Euler Tour chuyển về bài toán RMQ

#### Định nghĩa

Duyệt DFS cây, mỗi lần đến một đỉnh (kể cả khi quay lui) đều ghi lại chỉ số, thu được một dãy độ dài $2n-1$, gọi là Euler Tour của cây.

Ký hiệu vị trí xuất hiện đầu tiên của $u$ trong Euler Tour là $pos(u)$ (Euler index), dãy Euler Tour là $E[1..2n-1]$.

#### Quy trình

Có Euler Tour, bài toán LCA chuyển thành bài toán RMQ trong thời gian tuyến tính: $pos(LCA(u, v))=\min\{pos(k)|k\in E[pos(u)..pos(v)]\}$.

Giải thích: từ $u$ đến $v$ luôn đi qua $LCA(u,v)$, nhưng không đi qua tổ tiên của $LCA(u,v)$. Do đó, trong đoạn từ $u$ đến $v$ trên Euler Tour, đỉnh có chỉ số nhỏ nhất chính là $LCA(u,v)$.

DFS tính Euler Tour $O(n)$, độ dài dãy cũng $O(n)$, nên chuyển bài toán LCA về RMQ cùng kích thước trong $O(n)$.

#### Mã mẫu

???+ note "Mã mẫu"
    ```cpp
    int dfn[N << 1], pos[N], tot, st[30][(N << 1) + 2],
        rev[30][(N << 1) + 2];  // rev biểu diễn chỉ số đỉnh ứng với độ sâu nhỏ nhất
    
    void dfs(int cur, int dep) {
      dfn[++tot] = cur;
      depth[tot] = dep;
      pos[cur] = tot;
      for (int i = head[t]; i; i = side[i].next) {
        int v = side[i].to;
        if (!pos[v]) {
          dfs(v, dep + 1);
          dfn[++tot] = cur, depth[tot] = dep;
        }
      }
    }
    
    void init() {
      for (int i = 2; i <= tot + 1; ++i)
        lg[i] = lg[i >> 1] + 1;  // Tiền xử lý log2 để tối ưu hằng số
      for (int i = 1; i <= tot; i++) st[0][i] = depth[i], rev[0][i] = dfn[i];
      for (int i = 1; i <= lg[tot]; i++)
        for (int j = 1; j + (1 << i) - 1 <= tot; j++)
          if (st[i - 1][j] < st[i - 1][j + (1 << i - 1)])
            st[i][j] = st[i - 1][j], rev[i][j] = rev[i - 1][j];
          else
            st[i][j] = st[i - 1][j + (1 << i - 1)],
            rev[i][j] = rev[i - 1][j + (1 << i - 1)];
    }
    
    int query(int l, int r) {
      int k = lg[r - l + 1];
      return st[k][l] < st[k][r + 1 - (1 << k)] ? rev[k][l]
                                                : rev[k][r + 1 - (1 << k)];
    }
    ```

Khi cần truy vấn LCA của $(u, v)$, chỉ cần truy vấn đoạn $[\min\{pos[u], pos[v]\}, \max\{pos[u], pos[v]\}]$ để lấy đỉnh có độ sâu nhỏ nhất.

Nếu dùng Sparse Table để giải RMQ, thuật toán không hỗ trợ cập nhật online, tiền xử lý $O(n\log n)$, mỗi truy vấn LCA $O(1)$.

### Heavy-Light Decomposition (HLD)

LCA là đỉnh có độ sâu nhỏ hơn khi hai con trỏ cùng nhảy lên một heavy chain.

Tiền xử lý $O(n)$, mỗi truy vấn $O(\log n)$, hằng số nhỏ.

### Link Cut Tree

Trong [Link Cut Tree](../ds/lct.md), nếu thực hiện hai thao tác [access](../ds/lct.md#access) liên tiếp với hai đỉnh `u` và `v`, thì kết quả trả về của lần thứ hai chính là LCA của `u` và `v`.

Nếu không có thao tác link/cut, mỗi truy vấn LCA bằng Link Cut Tree mất $O(\log n)$.

### Chuẩn RMQ

Như đã nói, chuyển LCA về RMQ, điểm nghẽn là RMQ. Nếu giải RMQ trong $O(n)\sim O(1)$ thì LCA cũng vậy.

Chú ý Euler Tour có tính chất: hiệu hai phần tử liên tiếp là $1$ hoặc $-1$, nên có thể dùng [RMQ cộng/trừ 1](../topic/rmq.md#加减-1rmq) để giải trong $O(n)\sim O(1)$.

Độ phức tạp $O(n)\sim O(1)$, bộ nhớ $O(n)$, hỗ trợ truy vấn online, hằng số lớn.

#### Ví dụ [Luogu P3379【Mẫu】LCA](https://www.luogu.com.cn/problem/P3379)

??? note "Mã mẫu"
    ```cpp
    --8<-- "docs/graph/code/lca/lca_2.cpp"
    ```

## Bài tập

-   [Truy vấn tổ tiên](https://loj.ac/problem/10135)
-   [Vận chuyển hàng hóa](https://loj.ac/problem/2610)
-   [Khoảng cách giữa hai đỉnh](https://loj.ac/problem/10130)
