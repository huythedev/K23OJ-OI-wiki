Heap hợp nhất bền vững (Persistent Mergeable Heap) thường được sử dụng để giải quyết bài toán đường đi ngắn thứ $k$.

Nếu độ phức tạp thời gian của một loại heap hợp nhất không phải là khấu hao (amortized), thì sau khi bền vững hóa, độ phức tạp thời gian của một thao tác đơn lẻ sẽ được đảm bảo là $O(\log n)$, nghĩa là độ phức tạp sẽ không bị suy giảm do dữ liệu đặc biệt.

## Cây lệch trái bền vững (Persistent Leftist Tree)

Trước khi tìm hiểu nội dung này, vui lòng tham khảo các nội dung liên quan đến [Cây lệch trái](./leftist-tree.md).

### Quá trình

Xem xét lại quá trình hợp nhất của cây lệch trái, giả sử chúng ta muốn hợp nhất hai cây lệch trái có gốc lần lượt là $x, y$ và cây lệch trái được duy trì thỏa mãn tính chất min-heap:

1.  Nếu một trong hai nút $x, y$ là rỗng, trả về $x+y$.

2.  Chọn nút có trọng số nhỏ hơn trong hai nút $x, y$ làm gốc của cây lệch trái sau khi hợp nhất.

3.  Đệ quy hợp nhất cây con phải của $x$ với $y$, và đặt gốc của cây sau khi hợp nhất làm con phải của $x$.

4.  Duy trì tính chất lệch trái của cây lệch trái hiện tại sau khi hợp nhất, cập nhật giá trị `dist`, trả về nút gốc đã chọn.

Vì mỗi lần đệ quy đều làm giảm `dist[x] + dist[y]` đi 1, mà `dist[x]` là $O(\log n)$, nên mỗi lần hợp nhất chỉ sửa đổi tối đa $O(\log n)$ nút, do đó độ phức tạp thời gian là $O(\log n)$.

Tính bền vững yêu cầu lưu giữ thông tin lịch sử để có thể truy cập các phiên bản trước đó. Để làm cho cây lệch trái bền vững, chúng ta cần sao chép các đường dẫn bị sửa đổi dọc theo quá trình hợp nhất.

Vì vậy, quá trình hợp nhất của cây lệch trái bền vững như sau:

1.  Nếu một trong hai nút $x, y$ là rỗng, trả về $x+y$.

2.  Chọn nút có trọng số nhỏ hơn trong hai nút $x, y$, tạo một bản sao $p$ của nút đó, làm gốc của cây lệch trái sau khi hợp nhất.

3.  Đệ quy hợp nhất cây con phải của $p$ với $y$, và đặt gốc của cây sau khi hợp nhất làm con phải của $p$.

4.  Duy trì tính chất lệch trái của cây lệch trái có gốc là $p$, cập nhật giá trị `dist` của nó, trả về $p$.

Vì mỗi lần hợp nhất cây lệch trái chỉ sửa đổi và tạo mới tối đa $O(\log n)$ nút, giả sử số lần thao tác là $m$, thì độ phức tạp thời gian và không gian của cây lệch trái bền vững đều là $O(m\log n)$.

### Cài đặt tham khảo

```cpp
int merge(int x, int y) {
  if (!x || !y) return x + y;
  if (v[x] > v[y]) swap(x, y);
  int p = ++cnt;
  lc[p] = lc[x];
  v[p] = v[x];
  rc[p] = merge(rc[x], y);
  if (dist[lc[p]] < dist[rc[p]]) swap(lc[p], rc[p]);
  dist[p] = dist[rc[p]] + 1;
  return p;
}
```