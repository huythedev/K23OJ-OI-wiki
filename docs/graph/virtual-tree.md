author: HeRaNO, Ir1d, konnyakuxzy, ksyx, Xeonacid, konnyakuxzy, greyqz, sshwy, y-kx-b

## Dẫn nhập

???+ note "[「SDOI2011」Tiêu hao chiến](https://www.luogu.com.cn/problem/P2495)"
    Trong một trận chiến, mặt trận gồm $n$ hòn đảo và $n-1$ cây cầu, đảm bảo mỗi hai hòn đảo có đúng một đường đi.
    Hiện tại, quân địch đã phát hiện trung tâm của họ ở đảo $1$, và họ đã không còn đủ năng lượng để tiếp tục chiến đấu.
    Quân ta thắng lợi trong tầm nhìn.

    Biết rằng có $k$ hòn đảo khác có nhiều năng lượng, để ngăn chặn địch lấy được năng lượng, quân ta cần phá một số cây cầu.
    Mỗi cây cầu có một chi phí khác nhau, ta muốn tìm chi phí nhỏ nhất để đạt được mục tiêu.

    Trong khi đó, quân địch có một máy bí mật. Dù ta cắt tất cả năng lượng, họ vẫn có thể dùng máy đó.
    Máy tạo ra hiệu ứng không chỉ phục hồi tất cả các cây cầu ta phá, mà còn làm thay đổi phân bố năng lượng (nhưng đảm bảo không đến được đảo $1$).
    Máy chỉ có thể dùng $m$ lần, nên ta chỉ cần hoàn thành mỗi nhiệm vụ.

    Với tất cả dữ liệu, $2\le n\le 2.5\times 10^5,1\le m\le 5\times 10^5,\sum k_i\le 5\times 10^5,1\le k_i\le n-1$.

### Cách làm ngây thơ

Với bài trên, nếu số đỉnh nhỏ, ta có thể DP trực tiếp.

Gọi các đỉnh được chọn trong mỗi truy vấn là **đỉnh quan trọng** (hay "đỉnh khóa", "đỉnh cần thiết").

Đặt $Dp(i)$ là chi phí nhỏ nhất để tách $i$ khỏi mọi đỉnh quan trọng trong cây con của nó.

Gọi $w(a,b)$ là trọng số cạnh $(a,b)$.

Duyệt từng con $v$ của $i$:

-   Nếu $v$ không phải đỉnh quan trọng: $Dp(i)=Dp(i) + \min \{Dp(v),w(i,v)\}$;
-   Nếu $v$ là đỉnh quan trọng: $Dp(i)=Dp(i) + w(i,v)$.

Cách này có độ phức tạp $O(nq)$.

### Cách làm tối ưu

Nhận xét: nhiều đỉnh là "vô dụng". Xét hình dưới:

![vtree-1](images/vtree-tree.svg)

Nếu chọn các đỉnh quan trọng như sau:

![vtree-2](images/vtree-key-vertex.svg)

Chỉ hai đỉnh đỏ là quan trọng, các đỉnh khác là "không quan trọng".

Ta chỉ cần đảm bảo các đỉnh đỏ không đến được đỉnh $1$.

Quan sát đề, tổng số đỉnh quan trọng mỗi truy vấn là cùng bậc với $n$, tức là rất thưa. Nếu thuật toán phụ thuộc vào số đỉnh quan trọng thì sẽ tối ưu hơn.

Vậy ta cần **nén thông tin, thu nhỏ cây lớn thành cây nhỏ**.

## Cây ảo (Virtual Tree)

Từ đó xuất hiện khái niệm **cây ảo**.

Trực quan, cây ảo gồm các đỉnh quan trọng (đỏ) và các đỉnh đen (LCA), các cạnh đen là cạnh cây ảo.

![vtree-3](images/vtree-vtree1.svg)

![vtree-4](images/vtree-vtree2.svg)

![vtree-5](images/vtree-vtree3.svg)

![vtree-6](images/vtree-vtree4.svg)

Vì LCA của hai đỉnh quan trọng cũng cần lưu, nên cây ảo không chỉ gồm đỉnh quan trọng.

Quan hệ tổ tiên-hậu duệ trong cây ảo không thay đổi so với cây gốc.

Không thể $O(k^2)$ liệt kê LCA, nên ta sẽ sắp xếp các đỉnh quan trọng theo thứ tự DFS, rồi với mỗi cặp liên tiếp, lấy LCA và thêm vào cây ảo.

Vấn đề là làm sao xây cây ảo.

Trước khi trình bày, lưu ý: chỉ cần giữ nguyên quan hệ tổ tiên-hậu duệ, có thể thêm bao nhiêu đỉnh cũng được (dù sẽ chậm).

Vì vậy, có thể thêm đỉnh $1$ vào cây ảo mà không ảnh hưởng đáp án.

### Cách 1: Hai lần sắp xếp + LCA nối cạnh

Vì nhiều cặp có thể có cùng LCA, nên cần tránh thêm trùng.

Cách làm:

-   Sắp xếp các đỉnh quan trọng theo DFS.
-   Duyệt từng cặp liên tiếp, lấy LCA, thêm vào mảng $A$.
-   Sắp xếp $A$ theo DFS, loại trùng.
-   Duyệt từng cặp liên tiếp trong $A$, nối $\operatorname{LCA}(x,y)$ với $y$.

Tại sao nối $\operatorname{LCA}(x,y)$ với $y$ là đủ?

??? note "Chứng minh"
    Nếu $x$ là $y$'s ancestor, thì $x$ trực tiếp đến $y$.
    Vì DFS đảm bảo $x$ và $y$ là liên tiếp, nên không có đỉnh quan trọng nào trên đường đi.

    Nếu $x$ không phải là $y$'s ancestor, thì lấy $\operatorname{LCA}(x,y)$ làm $y$'s ancestor.
    Cũng có thể chứng minh rằng không có đỉnh quan trọng nào trên đường đi.

    Vậy nối $\operatorname{LCA}(x,y)$ với $y$ sẽ không bỏ sót, không lặp.

    Ngoài ra, đỉnh đầu tiên không bị nối, nhưng vì nó là gốc, nên không ảnh hưởng, tổng số cạnh là $m-1$.

Số đỉnh cây ảo tối đa là $2m$, với $m$ là số đỉnh quan trọng.

Độ phức tạp $O(m\log n)$.

#### Cài đặt

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/virtual-tree.md
int dfn[MAXN];
int h[MAXN], m, a[MAXN], len;  // Lưu đỉnh quan trọng

bool cmp(int x, int y) {
  return dfn[x] < dfn[y];  // Sắp xếp theo DFS
}

void build_virtual_tree() {
  sort(h + 1, h + m + 1, cmp);  // Sắp xếp đỉnh quan trọng theo DFS
  for (int i = 1; i < m; ++i) {
    a[++len] = h[i];
    a[++len] = lca(h[i], h[i + 1]);  // Thêm LCA
  }
  a[++len] = h[m];
  sort(a + 1, a + len + 1, cmp);  // Sắp xếp lại
  len = unique(a + 1, a + len + 1) - a - 1;  // Loại trùng
  for (int i = 1, lc; i < len; ++i) {
    lc = lca(a[i], a[i + 1]);
    conn(lc, a[i + 1]);  // Nối cạnh, nếu có trọng số thì là distance(lc,a[i+1])
  }
}
```

Như vậy đã đủ để xây cây ảo.

### Cách 2: Dùng stack đơn điệu

Ý tưởng: dùng stack duy trì một chuỗi trên cây ảo.

Stack lưu các đỉnh theo DFS tăng dần, mỗi đỉnh cha là phần tử dưới nó.

Ban đầu cho đỉnh $1$ vào stack.

Sau đó, lần lượt thêm các đỉnh quan trọng theo DFS.

Nếu LCA của đỉnh hiện tại và đỉnh trên cùng stack chính là đỉnh trên cùng, thì cùng một chuỗi, chỉ cần push vào.

Nếu không, cần pop các đỉnh không cùng chuỗi, nối cạnh trước khi pop.

Nếu LCA chưa có trong stack, push vào.

Ví dụ xây cây ảo cho các đỉnh $4,6,7$:

-   Sắp xếp thành $[4,6,7]$.
-   Push $1$ vào stack.

![vtree-7](./images/vtree-add1.svg)

-   Lấy $4$ làm đỉnh hiện tại, LCA với $1$ là $1$.
-   $LCA(1,4)=1$ là đỉnh trên cùng, nên push $4$ vào stack.

![vtree-8](./images/vtree-add2.svg)

-   Lấy $6$ làm đỉnh hiện tại, LCA với $4$ là $1$.
-   $LCA(6,4)=1$ không phải là đỉnh trên cùng, nên pop các đỉnh không cùng chuỗi.
-   Pop $4$.
-   Nối $1\to4$.
-   Push $6$ vào stack.

![vtree-9](./images/vtree-add3.svg)

-   Lấy $7$ làm đỉnh hiện tại, LCA với $6$ là $3$.
-   $LCA(7,6)=3$ không phải là đỉnh trên cùng, nên pop các đỉnh không cùng chuỗi.
-   Pop $6$.
-   Nối $3\to6$.
-   Push $7$ vào stack.

![vtree-10](./images/vtree-add4.svg)

-   Đã hết các đỉnh quan trọng, kết thúc.
-   Stack còn $1,3,7$.
-   Nối $1\to3$ và $3\to7$.

Cây ảo đã xây xong!

#### Cài đặt

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/virtual-tree.md
    bool cmp(const int x, const int y) { return id[x] < id[y]; }

    void build() {
      sort(h + 1, h + k + 1, cmp);
      sta[top = 1] = 1, g.sz = 0, g.head[1] = -1;
      // 1 vào stack, clear danh sách kề
      for (int i = 1, l; i <= k; ++i)
        if (h[i] != 1) {
          l = lca(h[i], sta[top]);
          if (l != sta[top]) {
            while (id[l] < id[sta[top - 1]])
              g.push(sta[top - 1], sta[top]), top--;
            if (id[l] > id[sta[top - 1]])
              g.head[l] = -1, g.push(l, sta[top]), sta[top] = l;
            else
              g.push(l, sta[top--]);
          }
          g.head[h[i]] = -1, sta[++top] = h[i];
        }
      for (int i = 1; i < top; ++i)
        g.push(sta[i], sta[i + 1]);
      return;
    }
    ```

Như vậy đã biết cách xây cây ảo!

Với bài Tiêu hao chiến, chỉ cần DP trên cây ảo như phần đầu:

-   Nếu $v$ không phải đỉnh quan trọng: $Dp(i)=Dp(i) + \min \{Dp(v),w(i,v)\}$
-   Nếu $v$ là đỉnh quan trọng: $Dp(i)=Dp(i) + w(i,v)$

## Bài tập khuyến nghị

-   [「SDOI2011」Tiêu hao chiến](https://www.luogu.com.cn/problem/P2495)
-   [「HEOI2014」Đại công trình](https://www.luogu.com.cn/problem/P4103)
-   [CF613D Kingdom and its Cities](http://codeforces.com/contest/613/problem/D/)
-   [「HNOI2014」Thế giới cây](https://www.luogu.com.cn/problem/P3233)
