## Mở đầu

???+ question "Bài toán"
    Cho một đồ thị, hãy tìm chu trình (vòng) gồm $n$ đỉnh ($n\ge 3$) có tổng trọng số cạnh nhỏ nhất.

Chu trình nhỏ nhất của đồ thị còn gọi là **chu vi nhỏ nhất**.

## Quy trình

### Giải pháp vét cạn

Giả sử giữa $u$ và $v$ có một cạnh trọng số $w$, $dis(u,v)$ là độ dài đường đi ngắn nhất giữa $u$ và $v$ sau khi xóa cạnh trực tiếp giữa $u$ và $v$.

Khi đó, chu trình nhỏ nhất trong đồ thị vô hướng là $dis(u,v)+w$.

Lưu ý: Nếu là đồ thị có hướng, công thức tương ứng là $dis(v,u)+w$.

Tổng độ phức tạp: $O(n^2m)$.

### Dijkstra

Liên quan: [Đường đi ngắn nhất/Dijkstra](./shortest-path.md#dijkstra-算法)

#### Quy trình

Duyệt tất cả các cạnh, mỗi lần xóa một cạnh rồi chạy Dijkstra từ đỉnh đầu của cạnh đó.

#### Độ phức tạp

Độ phức tạp: $O(m(n+m)\log n)$.

### Floyd

Liên quan: [Đường đi ngắn nhất/Floyd](./shortest-path.md#floyd-算法)

#### Quy trình

Gọi $val(u,v)$ là trọng số cạnh giữa $u$ và $v$ trong đồ thị gốc.

Lưu ý một tính chất của Floyd: khi vòng lặp ngoài cùng đến $k$ (chưa bắt đầu vòng $k$), mảng $dis$ lưu đường đi ngắn nhất giữa $u$ và $v$ chỉ qua các đỉnh có chỉ số $<k$.

Theo định nghĩa chu trình nhỏ nhất, chu trình phải có ít nhất 3 đỉnh. Gọi $w$ là đỉnh có chỉ số lớn nhất trên chu trình, hai đỉnh kề $w$ trên chu trình là $u,v$. Khi vòng ngoài cùng đến $k=w$, độ dài chu trình là $dis_{u,v}+val(v,w)+val(w,u)$.

Vì vậy, khi lặp $k$, với mọi $i<k, j<i$, cập nhật đáp án với $dis_{i,j}+val(i,k)+val(k,j)$.

#### Lưu vết đường đi

Đã biết chu trình có dạng $u\to k\to v$, rồi từ $v$ về $u$ (chỉ qua các đỉnh $<k$).

Bài toán trở thành tìm đường $v\leadsto u$. Dùng bất đẳng thức tam giác $dis_{u,v}\le dis_{u,i}+dis_{i,v}$, lưu $pos_{u,v}=j$ là đỉnh sao cho $dis_{u,v}=dis_{u,j}+dis_{j,v}$. Rõ ràng $j$ nằm trên đường $v\leadsto u$.

Vậy có thể chia đường thành $v\leadsto j$ và $j\leadsto u$, xử lý đệ quy.

???+ note "Chứng minh đệ quy không lặp vô hạn"
    Dùng phản chứng.

    Giả sử chu trình lặp lại một đỉnh $u$, thì trên chu trình sẽ có một đoạn từ $u$ quay lại $u$. Đó là một chu trình mới.

    Vì đồ thị không có chu trình âm (nếu có thì không tồn tại chu trình nhỏ nhất), nên chu trình mới có tổng trọng số không lớn hơn chu trình ban đầu.

    Vậy chỉ cần lấy chu trình này, nên không thể lặp lại đỉnh $u$, giả thiết sai.

    Do đó, khi đệ quy đến $u,v$, $pos_{u,v}$ luôn là đỉnh mới, không trùng $u,v$.

    Đặc biệt, khi $u,v$ kề nhau, trả về luôn.

    Vì tổng số đỉnh là $n$, số lần thêm mới không vượt quá $n$, nên đệ quy không lặp vô hạn.

#### Độ phức tạp

Độ phức tạp: $O(n^3)$.

#### Cài đặt

Dưới đây là cài đặt C++ và Python (có lưu vết đường đi):

=== "C++"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/min-cycle.md
    ```

=== "Python"
    ```python
    # filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/min-cycle.md
    ```

## Bài mẫu

??? note "[AcWing 344 Du lịch tham quan](https://www.acwing.com/problem/content/346)"
    Cho đồ thị vô hướng $n$ đỉnh, tìm một chu trình gồm ít nhất $3$ đỉnh, các đỉnh trên chu trình không lặp lại, tổng trọng số các cạnh nhỏ nhất.

    Đây là bài toán chu trình nhỏ nhất trên đồ thị vô hướng.

    Yêu cầu in ra một phương án, nếu có nhiều đáp án, in ra bất kỳ.

    $n \le 100$

Độ phức tạp $O(n^3)$, dùng Floyd tìm chu trình nhỏ nhất.

=== "C++"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/min-cycle.md
    ```

=== "Python"
    ```python
    # filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/min-cycle.md
    ```

## Bài mẫu 2

GDOI2018 Day2 Tuần tra

Cho đồ thị vô hướng $n$ đỉnh, không có cạnh âm, thực hiện $q$ thao tác, gồm 3 loại:

1.  Xóa một đỉnh và các cạnh liên quan
2.  Khôi phục một đỉnh và các cạnh liên quan
3.  Hỏi chu trình nhỏ nhất chứa đỉnh $x$

Với $50\%$ dữ liệu, $n,q \le 100$

Với mỗi chu trình đơn chứa $x$, luôn tồn tại hai cạnh kề $x$, xóa một trong hai cạnh thì chu trình thành đường đơn.

Vậy duyệt tất cả các cạnh kề $x$, mỗi lần xóa một cạnh rồi chạy Dijkstra.

Hoặc mỗi lần hỏi thì chạy Floyd tìm chu trình nhỏ nhất, $O(qn^3)$.

Với $100\%$ dữ liệu, $n,q \le 400$.

Vẫn dùng Floyd tìm chu trình nhỏ nhất.

Nếu không xóa, xóa đỉnh hỏi sẽ làm chu trình thành đường đơn.

Bước 2 dùng Floyd để tính.

Vậy đáp án là khoảng cách ngắn nhất giữa hai đỉnh bất kỳ không qua $x$.

Làm sao để online?

Có thể offline, xây dựng cây phân đoạn theo thời gian các truy vấn.

Mỗi đỉnh xuất hiện trong tất cả các truy vấn trừ lúc bị hỏi, chia thành $x+1$ đoạn, chèn vào cây phân đoạn.

Duyệt cây phân đoạn, mỗi lần lưu lại mảng Floyd, thêm các đỉnh vào, khi rời khỏi thì khôi phục lại.

Độ phức tạp $O(qn^2\log q)$.

Có một cách online tốt hơn.

Với mỗi truy vấn đỉnh $x$, chạy một lần đường đi ngắn nhất từ $x$, xây dựng cây đường đi ngắn nhất, xác định mỗi đỉnh thuộc cây con nào của $x$.

Luôn tồn tại một cạnh không thuộc cây, nối hai đỉnh thuộc hai cây con khác nhau, cộng với hai đường từ hai đỉnh đó về gốc $x$ tạo thành chu trình.

Chứng minh:

Chu trình nhỏ nhất luôn chứa ít nhất một cạnh nối hai cây con khác nhau.

Giả sử cạnh đó là $(u,v)$, đường từ $x$ đến $u$ và $x$ đến $v$ là ngắn nhất, nên chu trình $x\to u\to v\to x$ không dài hơn chu trình nhỏ nhất.

Vậy chỉ cần duyệt các cạnh không thuộc cây, cập nhật đáp án.

Mỗi truy vấn tốn $O(n^2)$.

Tổng độ phức tạp $O(qn^2)$
