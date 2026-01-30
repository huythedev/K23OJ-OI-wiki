Kiến thức nền tảng: [Thuật toán Dijkstra](./shortest-path.md#dijkstra-算法), [Thuật toán A\*](../search/astar.md), [Heap hợp nhất có thể truy hồi (Persistent Heap)](../ds/persistent-heap.md)

## Mô tả bài toán

Cho một đồ thị có hướng gồm $n$ đỉnh, $m$ cạnh, hãy tìm độ dài của đường đi khác nhau thứ $k$ ngắn nhất từ $s$ đến $t$.

???+ info "“Đường đi”"
    Trong tài liệu này, “đường đi” cho phép đi qua cùng một cạnh hoặc cùng một đỉnh nhiều lần, do đó tên gọi chính xác phải là “[walk](./concept.md#路径)” thay vì “path”. Vấn đề được bàn luận thực chất là **bài toán $k$ đường đi ngắn nhất** ($k$ shortest walk). Tuy nhiên, theo thói quen, tài liệu vẫn dùng từ “đường đi”, còn với đường đi không tự cắt (không đi qua đỉnh/cạnh nào hai lần) thì gọi là “đường đi đơn giản”.

## Thuật toán A\*

Thuật toán A\* là một thuật toán tìm kiếm. Với mỗi trạng thái hiện tại $x$, thuật toán đặt một hàm đánh giá $f(x)=g(x)+h(x)$, trong đó $g(x)$ là chi phí thực tế từ trạng thái khởi đầu đến $x$, còn $h(x)$ là ước lượng chi phí tốt nhất từ $x$ đến trạng thái đích. Khi tìm kiếm, mỗi lần lấy ra trạng thái $x$ có $f(x)$ nhỏ nhất để mở rộng các trạng thái kế tiếp. Có thể dùng **hàng đợi ưu tiên** để duy trì giá trị này.

Khi giải bài toán $k$ đường đi ngắn nhất, đặt $h(x)$ là độ dài đường đi ngắn nhất từ đỉnh hiện tại đến đích $t$. Có thể tiền xử lý giá trị này cho từng đỉnh bằng cách chạy thuật toán đường đi ngắn nhất trên đồ thị ngược từ $t$. Mỗi trạng thái cần lưu hai giá trị: đỉnh hiện tại $x$ và tổng quãng đường đã đi $g(x)$, ký hiệu trạng thái là $(x, g(x))$. Ban đầu, đưa trạng thái $(s, 0)$ vào hàng đợi ưu tiên. Mỗi lần lấy ra trạng thái có $f(x)=g(x)+h(x)$ nhỏ nhất, liệt kê tất cả các cạnh xuất phát từ $x$ để sinh trạng thái kế tiếp và đưa vào hàng đợi. Khi một đỉnh được truy cập lần thứ $k$, thì $g(x)$ tương ứng chính là độ dài đường đi ngắn thứ $k$ từ $s$ đến $x$.

Quá trình tìm kiếm này có thể tối ưu hơn. Vì chỉ cần tìm $k$ đường đi ngắn nhất từ $s$ đến $t$, nên nếu một đỉnh đã được lấy ra hơn $k$ lần thì không cần mở rộng trạng thái đó nữa. Trạng thái này sẽ không ảnh hưởng đến đáp án cuối cùng. Lý do là $k$ lần lấy ra trước đó đã tạo đủ $k$ đường đi hợp lệ đến đỉnh đó, đủ để xây dựng $k$ đường đi ngắn nhất đến $t$.

Nếu dùng hàng đợi ưu tiên để tối ưu Dijkstra, mỗi cạnh tối đa được đưa vào hàng đợi $k$ lần, nên độ phức tạp thuật toán là $O(km\log km)$, độ phức tạp bộ nhớ là $O(km)$. So với tìm kiếm trực tiếp, A\* cắt nhánh tốt hơn cho đỉnh đích $t$, nhưng chỉ cải thiện hằng số chứ không giảm bậc độ phức tạp. Thuật toán này tuy không tối ưu về độ phức tạp, nhưng có thể tìm được $k$ đường đi ngắn nhất từ $s$ đến **mọi đỉnh** (trong cây đường đi ngắn nhất gốc $t$) với cùng độ phức tạp.

### Cài đặt mẫu

??? example "Bài mẫu [Library Checker - K-Shortest Walk](https://judge.yosupo.jp/problem/k_shortest_walk) - Tham khảo cài đặt"
    ```cpp
    --8<-- "docs/graph/code/k-shortest-walk/k-shortest-walk-1.cpp"
    ```

## Phương pháp Heap hợp nhất có thể truy hồi (Persistent Heap)

Thuật toán trên thực chất tìm được $k$ đường đi ngắn nhất đến **mọi đỉnh**. Nếu chỉ cần tìm $k$ đường đi ngắn nhất đến một đỉnh đích $t$ cho trước, ta có thể làm nhanh hơn. Phần này trình bày một phương pháp dựa trên heap hợp nhất có thể truy hồi với độ phức tạp $O(m\log m + k\log k)$.

### Cây đường đi ngắn nhất và cạnh lệch (sidetrack)

Điểm nghẽn của thuật toán trước là chỉ khi đến đích $t$ mới cập nhật đáp án. Nhưng thực tế, các đường đi khác nhau có thể chỉ khác nhau ở một vài cạnh. Ví dụ, đường đi ngắn thứ hai có thể chỉ khác đường đi ngắn nhất ở một cạnh, còn các đoạn khác giống nhau; nhưng thuật toán trước có thể phải tìm lại toàn bộ các cạnh giống nhau này để tìm đường đi ngắn thứ hai. Vì chỉ đoạn “rẽ nhánh” mới là then chốt, nên để tìm $k$ đường đi ngắn nhất, chỉ cần xét $k$ cách rẽ nhánh có chi phí nhỏ nhất. Điều này dẫn đến khái niệm **cây đường đi ngắn nhất**.

Chạy thuật toán đường đi ngắn nhất trên đồ thị ngược từ $t$, lưu độ dài đường đi ngắn nhất từ mỗi đỉnh $x$ đến $t$ là $h(x)$, và lưu cạnh đầu tiên trên đường đi ngắn nhất từ $x$ là $f_x$ (nếu có nhiều cạnh tối ưu, chọn một bất kỳ). Tập các cạnh $f_x$ và các đỉnh đầu cuối của chúng tạo thành một cây, trong đó đường đi đơn giản từ mỗi đỉnh $x$ đến gốc $t$ là một đường đi ngắn nhất. Đó chính là **cây đường đi ngắn nhất** $T$.

Sau khi có cây $T$, ta có thể tính được mỗi cạnh không thuộc $T$ sẽ làm đường đi dài hơn bao nhiêu. Với cạnh $e=(u,v)\notin T$, trọng số $w$, định nghĩa một cạnh mới cũng từ $u$ đến $v$ với chi phí $\Delta(e)=w + h(v) - h(u)$. Gọi các cạnh này là **cạnh lệch** (sidetrack), và $\Delta(e)$ là **chi phí lệch**. Nếu một cạnh có đỉnh đầu/cuối không nằm trong $T$, nó không ảnh hưởng đến $k$ đường đi ngắn nhất đến $t$, có thể bỏ qua.

Hình dưới: bên trái là đồ thị $G$, bên phải là cây đường đi ngắn nhất $T$ (cạnh đậm) và các cạnh lệch (cạnh mảnh):

![](./images/k-shortest-path-1.svg)

Gọi tập các cạnh trên đường đi từ $s$ đến $t$ là $P$, loại bỏ các cạnh thuộc $T$ khỏi $P$ được $P'$. Sắp xếp các cạnh trong $P'$ theo thứ tự xuất hiện, hai cạnh liên tiếp $e_1=(u_1,v_1)$ và $e_2=(u_2,v_2)$ phải thỏa mãn:

-   Điều kiện $(*)$: Đỉnh bắt đầu $u_2$ của cạnh sau là tổ tiên (hoặc chính nó) của đỉnh kết thúc $v_1$ của cạnh trước trên cây $T$.

Lý do: trong đường đi gốc $P$, giữa $v_1$ và $u_2$ là một chuỗi các cạnh thuộc $T$. Ngược lại, với một tập cạnh lệch $P'$ thỏa mãn $(*)$, tồn tại duy nhất một đường đi $P$ trong $G$ tương ứng. Vì đường đi đơn giản trên cây $T$ là duy nhất. Như vậy, mỗi đường đi $P$ trong $G$ tương ứng một chuỗi cạnh lệch $P'$ thỏa mãn $(*)$. Độ dài đường đi $P$ là:

$$
h(s)+\sum_{e\in P'}\Delta(e).
$$

Vậy, bài toán tìm $k$ đường đi ngắn nhất chuyển thành tìm $k$ chuỗi cạnh lệch $P'$ thỏa mãn $(*)$ có tổng chi phí nhỏ nhất.

Để xử lý điều kiện $(*)$, thay vì mỗi lần truy vấn tổ tiên trên cây $T$, ta truyền tập cạnh lệch của mỗi đỉnh xuống các con trên cây $T$. Điều này tương đương xây dựng một đồ thị $G'$ như sau:

![](./images/k-shortest-path-2.svg)

Trên đồ thị này, điều kiện $(*)$ trở thành: các cạnh trong $P'$ phải nối tiếp nhau, tức $P'$ là một đường đi trong $G'$. Bài toán chuyển thành tìm $k$ đường đi ngắn nhất xuất phát từ $s$ đến **bất kỳ đỉnh nào** trong $G'$.

Bài toán này dễ giải: xuất phát từ $s$, chạy thuật toán đường đi ngắn nhất. Mỗi lần lấy ra một đỉnh từ hàng đợi ưu tiên là tìm được một đường đi trong $G'$, tương ứng với một đường đi từ $s$ đến $t$ trong $G$.

### Tối ưu bằng Heap hợp nhất có thể truy hồi

Ý tưởng đã rõ, nhưng nếu cài đặt trực tiếp thì độ phức tạp vẫn cao. Vì tại một đỉnh của $G'$, số cạnh có thể lên tới $\Theta(m)$, nên mỗi lần mở rộng có thể phải đưa $\Theta(m)$ cạnh vào hàng đợi. Tuy nhiên, thực tế chỉ cần quan tâm đến những cạnh ngắn nhất. Do đó, có thể gom toàn bộ tập cạnh lệch tại một đỉnh thành một cấu trúc dữ liệu (heap nhỏ nhất), mỗi lần chỉ cần truy cập cạnh nhỏ nhất.

Vậy, dùng heap nhỏ nhất để lưu tập cạnh lệch tại mỗi đỉnh. Trong hàng đợi ưu tiên, chỉ cần lưu các heap này, chi phí là giá trị nhỏ nhất trên đỉnh heap. Mỗi lần lấy heap đầu hàng đợi, lấy ra cạnh nhỏ nhất, rồi đưa heap còn lại (sau khi loại bỏ cạnh này) và heap tại đỉnh đích của cạnh này vào hàng đợi.

Vì cần truyền tập cạnh lệch xuống các con trên cây $T$, heap cần hỗ trợ hợp nhất (merge) và truy hồi (persistent). Đó chính là heap hợp nhất có thể truy hồi.

Tóm tắt thuật toán:

1.  Từ đích $t$, chạy thuật toán đường đi ngắn nhất để xây dựng cây đường đi ngắn nhất.
2.  Với mỗi đỉnh trên cây, xây dựng tập cạnh lệch và lưu vào heap hợp nhất có thể truy hồi.
3.  Truyền heap từ mỗi đỉnh xuống các con trên cây $T$.
4.  Từ $s$, đưa heap tại $s$ vào hàng đợi ưu tiên.
5.  Mỗi lần lấy heap đầu hàng đợi, ghi nhận đáp án, rồi đưa heap còn lại (sau khi loại bỏ cạnh nhỏ nhất) và heap tại đỉnh đích của cạnh này vào hàng đợi.

Thường dùng leftist heap hoặc heap ngẫu nhiên để cài đặt heap hợp nhất có thể truy hồi. Bước cuối còn có thể tối ưu hơn: thay vì hợp nhất hai heap con sau khi loại bỏ đỉnh, có thể đưa từng heap con vào hàng đợi riêng biệt, giảm chi phí hợp nhất $O(\log m)$ mỗi lần. Vì mỗi lần tối đa đưa ba heap mới vào hàng đợi, nên kích thước hàng đợi là $O(k)$. Như vậy, mỗi lần truy vấn chỉ mất $O(\log k)$, tổng cộng $O(k\log k)$.

Vì xây dựng cây đường đi ngắn nhất và heap hợp nhất có thể truy hồi đều $O(m\log m)$, nên tổng độ phức tạp là $O(m\log m + k\log k)$.

### Cài đặt mẫu

??? example "Bài mẫu [Library Checker - K-Shortest Walk](https://judge.yosupo.jp/problem/k_shortest_walk) - Tham khảo cài đặt"
    ```cpp
    --8<-- "docs/graph/code/k-shortest-walk/k-shortest-walk-2.cpp"
    ```

## Bài tập

-   [“SDOI2010” Học viện lợn phép thuật](https://www.luogu.com.cn/problem/P2483)

## Tài liệu tham khảo & chú thích

-   [\[Tutorial\] k shortest paths and Eppstein's algorithm by meooow - Codeforces](https://codeforces.com/blog/entry/102085)
