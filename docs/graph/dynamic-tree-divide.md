## Phân hoạch động trên cây (Dynamic Centroid Decomposition)

Phân hoạch động trên cây dùng để giải các bài toán thống kê thông tin đường đi trên cây khi có **thay đổi trọng số đỉnh/cạnh**.

### Cây phân hoạch (Centroid Tree)

Nhắc lại quá trình phân hoạch trên cây.

Với một đỉnh $x$, các đường đi đơn giản trong cây con của $x$ gồm hai loại: các đường đi qua $x$ (gồm một hoặc hai đường đi xuất phát từ $x$), và các đường đi không qua $x$ (đã nằm trong các cây con của các con của $x$).

Để tính thông tin các đường đi trong cây con, ta chọn một điểm phân hoạch (centroid) $rt$, tính thông tin các đường đi qua $rt$, sau đó với mỗi con của $rt$, loại bỏ $rt$ và tiếp tục phân hoạch trên phần còn lại. Các centroid được chọn tạo thành một cây gọi là **cây phân hoạch**. Tổng kích thước các khối liên thông ở cùng một tầng của cây phân hoạch là $O(n)$, nên độ phức tạp phụ thuộc vào độ sâu cây phân hoạch $h$, tổng là $O(nh)$.

Có thể chứng minh rằng nếu luôn chọn trọng tâm (centroid) của khối liên thông làm điểm phân hoạch, độ sâu cây phân hoạch là nhỏ nhất, $O(\log n)$. Như vậy, ta có thể thống kê thông tin $O(n^2)$ đường đi trên cây trong $O(n\log n)$ thời gian.

Vì cấu trúc cây không thay đổi trong quá trình phân hoạch động, nên hình dạng cây phân hoạch cũng không thay đổi.

Tham khảo mã xây dựng cây phân hoạch:

```cpp
void calcsiz(int x, int f) {
  siz[x] = 1;
  maxx[x] = 0;
  for (int j = h[x]; j; j = nxt[j])
    if (p[j] != f && !vis[p[j]]) {
      calcsiz(p[j], x);
      siz[x] += siz[p[j]];
      maxx[x] = max(maxx[x], siz[p[j]]);
    }
  maxx[x] =
      max(maxx[x], sum - siz[x]);  // maxx[x] là kích thước cây con lớn nhất khi lấy x làm gốc
  if (maxx[x] < maxx[rt])
    rt = x;  // Không dùng <= để đảm bảo rt không đổi ở lần calcsiz thứ hai
}

void pre(int x) {
  vis[x] = true;  // Đánh dấu không xét lại x ở các bước sau
  for (int j = h[x]; j; j = nxt[j])
    if (!vis[p[j]]) {
      sum = siz[p[j]];
      rt = 0;
      maxx[rt] = inf;
      calcsiz(p[j], -1);
      calcsiz(rt, -1);  // Tính lại để lấy kích thước các cây con khi rt làm gốc
      fa[rt] = x;
      pre(rt);  // Ghi nhận cha trên cây phân hoạch
    }
}

int main() {
  sum = n;
  rt = 0;
  maxx[rt] = inf;
  calcsiz(1, -1);
  calcsiz(rt, -1);
  pre(rt);
}
```

### Cách thực hiện cập nhật

Khi truy vấn hoặc cập nhật, ta sẽ duyệt lên các cha trên cây phân hoạch. Vì độ sâu cây phân hoạch tối đa $O(\log n)$ nên độ phức tạp được đảm bảo.

Trong quá trình phân hoạch động, cần biết khoảng cách từ một đỉnh tới các tổ tiên trên cây phân hoạch. Vì mỗi đỉnh có tối đa $O(\log n)$ tổ tiên, ta có thể tiền xử lý độ sâu $dep[x]$ hoặc dùng LCA để tính khoảng cách. **Lưu ý:** Khoảng cách từ một đỉnh tới các tổ tiên trên cây phân hoạch không nhất thiết tăng dần, không thể cộng dồn!

Một đỉnh có thể bị tính lặp trong thông tin của các tổ tiên trên cây phân hoạch, cần loại bỏ phần trùng lặp. Thường sẽ lưu hai loại thông tin cho mỗi khối liên thông: một là thông tin khoảng cách tới centroid, hai là thông tin khoảng cách tới cha centroid trên cây phân hoạch. Phần này sẽ được trình bày trong ví dụ.

??? note "Ví dụ [「ZJOI2007」Trốn tìm](https://www.luogu.com.cn/problem/P2056)"
    Cho cây $n$ đỉnh, ban đầu mọi đỉnh đều màu đen. Thực hiện hai thao tác:
    
    1.  Đảo màu một đỉnh (đen thành trắng, trắng thành đen);
    2.  Hỏi khoảng cách lớn nhất giữa hai đỉnh đen trên cây.
    
        $n\le 10^5,m\le 5\times 10^5$

Xây cây phân hoạch, với mỗi đỉnh $x$ lưu hai **heap có thể xóa**. $dist[x]$ lưu khoảng cách từ các đỉnh đen trong khối liên thông của $x$ tới $x$, $ch[x]$ lưu khoảng cách từ các đỉnh đen trong khối liên thông của $x$ và các con trên cây phân hoạch tới $x$. Vì khi tìm đường đi dài nhất, hai đầu không được nằm trong cùng một cây con, nên chỉ lưu giá trị của chính nó và giá trị lớn nhất từ mỗi cây con. Tổng lớn nhất hai giá trị trong $ch[x]$ là đường đi dài nhất qua centroid $x$. Dùng heap $ans$ lưu đáp án của mọi centroid, giá trị lớn nhất là đáp án cần tìm.

Khi $dist[x]$ thay đổi, có thể cập nhật $ch[x],ans$ trong $O(\log n)$.

Khi đảo màu một đỉnh $x$, nếu ban đầu là đen thì xóa, nếu là trắng thì thêm. Với mọi tổ tiên $u$ của $x$, thêm/xóa $dist(x,u)$ vào $dist[u]$, đồng thời cập nhật $ch[x],ans$. Đặc biệt, thêm/xóa giá trị $0$ vào $ch[x]$.

Mã tham khảo:

```cpp
--8<-- "docs/graph/code/dynamic-tree-divide/dynamic-tree-divide_1.cpp"
```

???+ note "Ví dụ [Luogu P6329【Mẫu】Cây phân hoạch | Sóng chấn động](https://www.luogu.com.cn/problem/P6329)"
    Cho cây $n$ đỉnh, mỗi đỉnh có trọng số $v[x]$. Thực hiện hai thao tác:
    
    1.  Hỏi tổng trọng số các đỉnh cách $x$ không quá $y$;
    2.  Đổi trọng số đỉnh $x$ thành $y$, tức $v[x]=y$.

Dùng segment tree động lưu thông tin trọng số theo khoảng cách.

Tương tự ví dụ trên, với mỗi centroid $x$ lưu segment tree $dist[x]$ (lưu tổng trọng số các đỉnh trong khối liên thông của $x$ theo khoảng cách tới $x$), và segment tree $ch[x]$ (lưu tổng trọng số các đỉnh trong khối liên thông của $x$ theo khoảng cách tới cha centroid trên cây phân hoạch).

Khi truy vấn hoặc cập nhật, cần duyệt lên các tổ tiên trên cây phân hoạch.

Ví dụ truy vấn: hỏi tổng trọng số các đỉnh cách $x$ không quá $y$, cộng tổng từ $dist[x]$ với khoảng cách $0$ đến $y$, sau đó với mỗi tổ tiên $u$ của $x$, gọi $v$ là tổ tiên thấp hơn, $d=dist(x,u)$, cộng tổng từ $dist[u]$ với khoảng cách $0$ đến $y-d$, rồi trừ đi tổng từ $ch[v]$ với khoảng cách $0$ đến $y-d$ (loại bỏ phần trùng).

Khi cập nhật, cần cập nhật cả $dist[x]$ và $ch[x]$.

Mã tham khảo:

```cpp
--8<-- "docs/graph/code/dynamic-tree-divide/dynamic-tree-divide_2.cpp"
```
