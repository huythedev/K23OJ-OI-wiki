## Giới thiệu

???+ note "[Luogu 4097 \[HEOI2013\]Segment](https://www.luogu.com.cn/problem/P4097)"
    Yêu cầu bảo trì hai thao tác sau trên hệ tọa độ Descartes (bắt buộc trực tuyến):
    
    1.  Thêm một đoạn thẳng vào mặt phẳng. Ký hiệu đoạn thẳng thứ $i$ được thêm vào có số hiệu là $i$, hai đầu mút của đoạn thẳng lần lượt là $(x_0,y_0)$, $(x_1,y_1)$.
    2.  Cho một số $k$, hỏi trong số các đoạn thẳng cắt đường thẳng $x = k$, đoạn thẳng nào có tung độ giao điểm lớn nhất (nếu có nhiều đoạn thẳng cùng có tung độ giao điểm lớn nhất, chọn đoạn thẳng có số hiệu nhỏ nhất). Đặc biệt, nếu không có đoạn thẳng nào cắt đường thẳng đã cho, in ra $0$.
    
    Dữ liệu thỏa mãn: Tổng số thao tác $1 \leq n \leq 10^5$, $1 \leq k, x_0, x_1 \leq 39989$, $1 \leq y_0, y_1 \leq 10^9$.

Chúng ta nhận thấy rằng Segment Tree truyền thống không thể bảo trì tốt thông tin này. Trong trường hợp này, **Li Chao Segment Tree** (Cây Li Chao) ra đời.

## Quá trình

Chúng ta có thể chuyển đổi bài toán thành việc bảo trì các thao tác sau:

-   Thêm một hàm bậc nhất, miền xác định là $[l,r]$;
-   Cho $k$, tìm hàm bậc nhất có giá trị lớn nhất tại $x=k$ trong số tất cả các hàm bậc nhất có miền xác định chứa $k$. Nếu có nhiều hàm có cùng giá trị lớn nhất, chọn hàm có số hiệu nhỏ nhất.

???+ warning "Lưu ý"
    Khi đoạn thẳng vuông góc với trục $x$, sẽ xảy ra trường hợp chia cho 0. Giả sử hai đầu mút của đoạn thẳng lần lượt là $(x,y_0)$ và $(x,y_1)$, $y_0<y_1$, ta thêm hàm bậc nhất $f(x)=0\cdot x+y_1$ với miền xác định là $[x,x]$.

Nhìn thấy việc sửa đổi khoảng, chúng ta áp dụng phương pháp giải quyết vấn đề khoảng phổ biến của Segment Tree: gán một nhãn lười (lazy tag) cho mỗi nút. Nhãn lười của mỗi nút $i$ là một đoạn thẳng, ký hiệu là $l_i$, biểu thị việc dùng $l_i$ để cập nhật toàn bộ khoảng mà nút đó đại diện.

Bây giờ chúng ta cần thêm một đoạn thẳng $f$, xét một khoảng Segment Tree nào đó được đoạn thẳng mới $f$ bao phủ hoàn toàn. Nếu khoảng này chưa có nhãn, ta đánh dấu trực tiếp bằng đoạn thẳng đó.

Nếu khoảng này đã có nhãn, do việc gộp nhãn rất khó, ta chỉ có thể đẩy nhãn xuống. Tuy nhiên, các nút con cũng có nhãn riêng của chúng và có thể xảy ra xung đột, vì vậy chúng ta phải đẩy nhãn xuống một cách đệ quy.

![](images/li-chao-tree-1.png)

Như hình vẽ, dựa vào việc giá trị của đoạn thẳng mới $f$ có lớn hơn nhãn cũ $g$ hay không, chúng ta có thể chia khoảng hiện tại thành hai khoảng con. Trong đó **chắc chắn có một khoảng con được bao phủ hoàn toàn bởi $f$ hoặc $g$**, nghĩa là trong hai đoạn thẳng, chắc chắn có một đoạn thẳng chỉ có thể trở thành đáp án của khoảng con bên trái, hoặc chỉ có thể trở thành đáp án của khoảng con bên phải. Chúng ta dùng đoạn thẳng này để cập nhật đệ quy cây con tương ứng, và dùng đoạn thẳng còn lại làm nhãn lười để cập nhật toàn bộ khoảng hiện tại. Điều này đảm bảo độ phức tạp của việc đẩy nhãn xuống đệ quy. Khi một đoạn thẳng chỉ có thể là đáp án của khoảng con trái hoặc phải, nó mới được đẩy xuống, vì vậy không cần lo lắng về việc bỏ sót một số đoạn thẳng.

Cụ thể, giả sử trung điểm của khoảng hiện tại là $m$, ta so sánh giá trị của đoạn thẳng mới $f$ tại trung điểm với giá trị của đoạn thẳng tối ưu cũ $g$ tại trung điểm.

Nếu đoạn thẳng mới $f$ tốt hơn, ta hoán đổi $f$ và $g$. Bây giờ hãy xem xét trường hợp $f$ không tốt bằng $g$ tại trung điểm:

1.  Nếu tại đầu mút trái $f$ tốt hơn, thì $f$ và $g$ chắc chắn giao nhau trong nửa khoảng bên trái, $f$ chỉ có thể tốt hơn $g$ trong khoảng bên trái, đệ quy xuống con trái để đẩy nhãn;
2.  Nếu tại đầu mút phải $f$ tốt hơn, thì $f$ và $g$ chắc chắn giao nhau trong nửa khoảng bên phải, $f$ chỉ có thể tốt hơn $g$ trong khoảng bên phải, đệ quy xuống con phải để đẩy nhãn;
3.  Nếu tại cả hai đầu mút $g$ đều tốt hơn, thì $f$ không thể trở thành đáp án, không cần tiếp tục đẩy xuống.

Ngoài hai trường hợp trên, còn có một trường hợp là $f$ và $g$ giao nhau đúng tại trung điểm, trong quá trình cài đặt chương trình có thể gộp vào trường hợp $f$ không tốt bằng $g$ tại trung điểm, kết quả sẽ đệ quy xuống phía đầu mút mà $f$ tốt hơn.

Cuối cùng, lấy $g$ làm nhãn lười của khoảng hiện tại.

Đẩy nhãn xuống:

???+ note "Hiện thực"
    ```cpp
    constexpr double eps = 1e-9;
    
    int cmp(double x, double y) {  // Vì dùng số thực nên sẽ có sai số
      if (x - y > eps) return 1;
      if (y - x > eps) return -1;
      return 0;
    }
    
    //...
    
    void upd(int root, int cl, int cr, int u) {  // Sửa đổi trên khoảng được bao phủ hoàn toàn bởi đoạn thẳng
      int &v = s[root], mid = (cl + cr) >> 1;
      int bmid = cmp(calc(u, mid), calc(v, mid));
      if (bmid == 1 || (!bmid && u < v))  // Trong bài này nhớ xét số hiệu đoạn thẳng
        swap(u, v);
      int bl = cmp(calc(u, cl), calc(v, cl)), br = cmp(calc(u, cr), calc(v, cr));
      if (bl == 1 || (!bl && u < v)) upd(root << 1, cl, mid, u);
      if (br == 1 || (!br && u < v)) upd(root << 1 | 1, mid + 1, cr, u);
      // Điều kiện của hai lệnh if trên tối đa chỉ có một cái thỏa mãn, điều này đảm bảo độ phức tạp thời gian của Li Chao Tree
    }
    ```

Chia nhỏ đoạn thẳng:

???+ note "Hiện thực"
    ```cpp
    void update(int root, int cl, int cr, int l, int r,
                int u) {  // Định vị khoảng được bao phủ hoàn toàn bởi đoạn thẳng chèn vào
      if (l <= cl && cr <= r) {
        upd(root, cl, cr, u);  // Bao phủ hoàn toàn khoảng hiện tại, cập nhật nhãn của khoảng hiện tại
        return;
      }
      int mid = (cl + cr) >> 1;
      if (l <= mid) update(root << 1, cl, mid, l, r, u);  // Đệ quy chia nhỏ khoảng
      if (mid < r) update(root << 1 | 1, mid + 1, cr, l, r, u);
    }
    ```

Lưu ý nhãn lười không tương đương với đoạn thẳng có giá trị lớn nhất tại trung điểm của khoảng.

![](images/li-chao-tree-2.png)

Như hình vẽ, sau khi thêm đoạn thẳng màu vàng, chỉ có nhãn của nút màu đỏ được cập nhật, trong khi nhãn của các nút màu xanh lá cây vẫn chưa thay đổi. Nhưng tại trung điểm của khoảng xanh lá cây thứ hai, ba, bốn, rõ ràng đoạn thẳng màu vàng có giá trị lớn nhất.

Khi truy vấn, chúng ta có thể sử dụng tư tưởng "nhãn vĩnh viễn" (marker permanentization), so sánh trong các đoạn thẳng nhãn của tất cả các khoảng Segment Tree chứa $x$ (không quá $O(\log n)$ cái) để tìm ra đáp án cuối cùng.

Truy vấn:

???+ note "Hiện thực"
    ```cpp
    pdi query(int root, int l, int r, int d) {  // Truy vấn
      if (r < d || d < l) return {0, 0};
      int mid = (l + r) >> 1;
      double res = calc(s[root], d);
      if (l == r) return {res, s[root]};
      return pmax({res, s[root]}, pmax(query(root << 1, l, mid, d),
                                       query(root << 1 | 1, mid + 1, r, d)));
    }
    ```

Theo mô tả trên, độ phức tạp thời gian của quá trình truy vấn rõ ràng là $O(\log n)$. Trong quá trình chèn, chúng ta cần chia đoạn thẳng ban đầu vào $O(\log n)$ khoảng, đối với mỗi khoảng, chúng ta lại tốn $O(\log n)$ thời gian để đẩy nhãn xuống đệ quy, do đó độ phức tạp thời gian của quá trình chèn là $O(\log^2 n)$.

??? note "[\[HEOI2013\]Segment](https://www.luogu.com.cn/problem/P4097) Code tham khảo"
    ```cpp
    --8<-- "docs/ds/code/li-chao-tree/li-chao-tree_1.cpp"
    ```

## Hợp nhất

Tương tự như hợp nhất Segment Tree thông thường, chúng ta định nghĩa quá trình sau để hợp nhất hai nút Li Chao Tree $u,v$, và lấy $u$ làm gốc mới.

1.  Nếu $v$ rỗng, kết thúc quá trình.

2.  Nếu $u$ rỗng, gán $v$ cho $u$.

3.  Chèn đoạn thẳng tương ứng với $v$ vào cây con có gốc là $u$.

4.  Đệ quy hợp nhất các cây con trái phải tương ứng của $u,v$.

Nếu hợp nhất một số Li Chao Tree liên quan đến tổng số điểm là $n$, thì độ phức tạp của quá trình này là $O(n\log n)$: đối với bất kỳ đoạn thẳng nào tương ứng với một nút trên cây, mỗi lần di chuyển nó, chúng ta hoặc tăng độ sâu của nó lên 1, hoặc xóa trực tiếp khỏi cây, chi phí của hai thao tác này đều là $O(1)$, mà độ sâu của mỗi điểm tối đa là $O(\log n)$, vì vậy độ phức tạp như trên.

???+ note "Hiện thực"
    ```cpp
    void upd(int &root, int cl, int cr,
             int u) {  // Hợp nhất nhiều cây Li Chao Tree, sử dụng cấp phát động nút (dynamic node creation).
      static int idx = 0;
      if (!root) {
        s[root = ++idx] = u;
        return;
      }
      int &v = s[root], mid = (cl + cr) >> 1;
      int bmid = cmp(calc(u, mid), calc(v, mid));
      if (bmid == 1 || (!bmid && u < v)) swap(u, v);
      int bl = cmp(calc(u, cl), calc(v, cl)), br = cmp(calc(u, cr), calc(v, cr));
      if (bl == 1 || (!bl && u < v)) upd(ls[root], cl, mid, u);
      if (br == 1 || (!br && u < v)) upd(rs[root], mid + 1, cr, u);
    }
    
    int merge(int &u, int &v, int l, int r) {
      if (!u || !v) {
        return u + v;
      }
      if (l == r) {
        int b = cmp(calc(s[v], l), calc(s[u], l));
        if (b == 1 || (!b && s[v] < s[u])) return v;
        return u;
      }
      upd(u, l, r, s[v]);
      int mid = (l + r) >> 1;
      ls[u] = merge(ls[u], ls[v], l, mid);
      rs[u] = merge(rs[u], rs[v], mid + 1, r);
      return u;
    }
    ```

## Bài tập

[「JSOI2008」Blue Mary mở công ty](https://www.luogu.com.cn/problem/P4254)

[「CodeChef」TSUM2 Sum on Tree](https://www.codechef.com/problems/TSUM2)

[「USACO13MAR」Hill Walk G](https://www.luogu.com.cn/problem/P3081)

[「CF932F」Escape Through Leaf](https://codeforces.com/problemset/problem/932/F)