author: jifbt, Mayuri0v0

## Định nghĩa

Các khái niệm dưới đây, xem tại [Các khái niệm liên quan đến lý thuyết đồ thị](./concept.md):

-   Bậc liên thông cạnh, tập cắt cạnh;
-   Bậc liên thông đỉnh, tập cắt đỉnh;
-   Cực đại (clique).

## Tính chất

### Bất đẳng thức Whitney

**Bất đẳng thức Whitney** (1932) mô tả mối quan hệ giữa bậc liên thông đỉnh $\kappa$, bậc liên thông cạnh $\lambda$ và bậc nhỏ nhất $\delta$:

$$
\kappa \le \lambda \le \delta
$$

???+ note "Chứng minh"
    Trực giác: nếu có một tập cắt cạnh kích thước $\lambda$, mỗi cạnh chọn một đầu mút sẽ tạo thành một tập cắt đỉnh kích thước $\lambda$, nên bất đẳng thức đầu tiên đúng.

    Các cạnh kề với đỉnh có bậc nhỏ nhất (chọn một đỉnh bất kỳ nếu có nhiều) tạo thành tập cắt cạnh kích thước $\delta$, nên bất đẳng thức thứ hai cũng đúng.

Bất đẳng thức này là chặt; tức là, với mỗi bộ ba số thỏa mãn, đều có thể xây dựng được đồ thị tương ứng.

???+ note "Cấu trúc"
    Nối hai cực đại kích thước $\delta + 1$ bằng $\lambda$ cạnh, sao cho mỗi cực đại có $\lambda$ và $\kappa$ đỉnh khác nhau được nối bởi các cạnh này.

### Định lý Menger

Từ [Định lý max-flow min-cut](./flow/min-cut.md) (hay còn gọi là định lý Ford–Fulkerson), ta có thể suy ra: số lượng đường đi không giao nhau (không chung cạnh) lớn nhất giữa hai đỉnh bằng kích thước nhỏ nhất của tập cắt (kết quả này còn gọi là **Định lý Menger**).

## Tính toán

Giả sử các cạnh của đồ thị đều có trọng số $1$.

### Tính bậc liên thông cạnh bằng max-flow

Duyệt qua tất cả các cặp đỉnh $(s, t)$, lấy $s$ làm nguồn, $t$ làm đích, chạy max-flow với trọng số cạnh bằng $1$. Cần $O(n^2)$ lần max-flow, nếu dùng thuật toán Edmonds–Karp thì độ phức tạp $O(|V|^3 |E|^2)$. Dùng thuật toán Dinic sẽ tốt hơn, độ phức tạp $O(|V|^2 |E| \min(|V|^{2/3}, |E|^{1/2}))$.

### Cắt nhỏ toàn cục

Dùng [Thuật toán Stoer–Wagner](./stoer-wagner.md) chỉ cần chạy một lần min-cut không nguồn đích. Độ phức tạp $O(|V||E| + |V|^{2}\log|V|)$, thường coi xấp xỉ $O(|V|^3)$.

### Bậc liên thông đỉnh

Tiếp tục duyệt cặp đỉnh, lần này với mỗi đỉnh không phải nguồn/đích $x$, tách thành hai đỉnh $x_1$ và $x_2$, nối cạnh $(x_1, x_2)$. Các cạnh $(u, v)$ trong đồ thị gốc chuyển thành hai cạnh $(u_2, v_1)$ và $(v_2, u_1)$. Khi đó, max-flow giữa $s$ và $t$ bằng kích thước tập cắt đỉnh nhỏ nhất giữa $s$ và $t$ (còn gọi là bậc liên thông đỉnh cục bộ). Độ phức tạp như tính bậc liên thông cạnh bằng max-flow.

**Trang này dịch từ các bài viết [Рёберная связность. Свойства и нахождение](http://e-maxx.ru/algo/rib_connectivity)、[Вершинная связность. Свойства и нахождение](http://e-maxx.ru/algo/vertex_connectivity) và bản tiếng Anh [Edge connectivity/Vertex connectivity](https://cp-algorithms.com/graph/edge_vertex_connectivity.html). Bản tiếng Nga: Public Domain + Leave a Link; bản tiếng Anh: CC-BY-SA 4.0.**

## Đọc thêm

-   Bài báo [*Connectivity Algorithms*](https://www.cse.msu.edu/~cse835/Papers/Graph_connectivity_revised.pdf) giới thiệu các tiến bộ gần đây về thuật toán tính liên thông. Bạn đọc quan tâm có thể tham khảo thêm.
