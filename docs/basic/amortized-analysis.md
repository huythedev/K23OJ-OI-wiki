Kiến thức tiền đề: [Độ phức tạp](./complexity.md)

Trang này sẽ giới thiệu kiến thức cơ bản về phân tích chi phí bình quân (Amortized Analysis).

## Giới thiệu

Phân tích chi phí bình quân (Amortized Analysis) là một kỹ thuật dùng để phân tích hiệu năng của các thuật toán và cấu trúc dữ liệu động. Nó không chỉ quan tâm đến chi phí của một thao tác đơn lẻ, mà đánh giá chi phí trung bình trên một dãy thao tác để đưa ra nhận xét chính xác hơn về hiệu năng tổng thể. Phân tích bình quân không dựa vào xác suất; nó đảm bảo chi phí trung bình trên mỗi thao tác trong trường hợp xấu nhất, nhưng không mô tả hiệu năng trung bình thực tế theo phân phối đầu vào. Trong trường hợp xấu nhất, phân tích bình quân phân bổ chi phí của các thao tác tốn kém sang các thao tác rẻ hơn, giúp giữ chi phí trung bình ở mức hợp lý.

Phân tích bình quân thường dùng ba phương pháp chính: phân tích tập hợp (aggregate analysis), phương pháp kế toán (accounting method) và phương pháp thế năng (potential method). Mỗi phương pháp phù hợp với các ngữ cảnh khác nhau, nhưng cùng mục tiêu là cân bằng chi phí các thao tác để tối ưu hiệu năng tổng thể trong trường hợp xấu nhất.

## Nội dung

Xét một mảng động có thể mở rộng, ví dụ như `vector` trong C++, với dung lượng ban đầu $m = 1$. Mỗi lần chèn phần tử mới, nếu mảng đã đầy thì nhân đôi dung lượng, sao chép các phần tử cũ sang mảng mới rồi chèn phần tử mới.

Sau đây lấy ví dụ thao tác chèn trên mảng động để phân tích chi phí bình quân bằng ba phương pháp: phân tích tập hợp, phương pháp kế toán và phương pháp thế năng.

### Phân tích tập hợp

Phân tích tập hợp (Aggregate Analysis) tính tổng chi phí của một dãy thao tác rồi chia đều cho số thao tác để ra chi phí bình quân trên mỗi thao tác.

Với mảng động, có hai chi phí chính khi chèn:
- Nếu mảng chưa đầy, chèn tốn $O(1)$.
- Nếu mảng đầy, cần mở rộng và sao chép, chi phí sao chép là $O(m)$ với $m$ là dung lượng hiện tại.

Để tính tổng chi phí cho $n$ lần chèn, chia thành hai phần:

1. Chi phí chèn thông thường: mỗi chèn tốn hằng số $O(1)$, tổng cho $n$ lần là $O(n)$.
2. Chi phí mở rộng mảng: các lần mở rộng xảy ra khi kích thước mảng là $1,2,4,\ldots,2^k$ (với $2^k \le n$). Chi phí sao chép tương ứng là $1,2,4,\ldots,2^{k-1}$, tổng là $1+2+4+\ldots+2^{k-1}=2^k-1 = O(n)$.

Vậy tổng chi phí chèn cho toàn bộ là $O(n)$, trung bình mỗi thao tác là $O(1)$. Ngay cả trong trường hợp xấu nhất, chi phí bình quân mỗi thao tác vẫn là hằng số.

### Phương pháp kế toán

Phương pháp kế toán (Accounting Method) phân bổ trước một chi phí bình quân cố định cho mỗi thao tác, đảm bảo tổng chi phí thực tế của tất cả thao tác không vượt quá tổng chi phí đã phân bổ. Phương pháp này giống như trả trước: những thao tác rẻ sẽ tiết kiệm một phần chi phí để trả cho các thao tác đắt sau này.

Với mảng động, ta phân bổ một chi phí bình quân cố định cho mỗi lần chèn để khi cần mở rộng đã có đủ “tiền” tích luỹ.

1. Phân bổ chi phí:
   - Giả sử chi phí thực tế mỗi lần chèn là $1$, đặt chi phí bình quân là $3$.
   - Trong đó $1$ dùng cho chèn hiện tại, $2$ để dành cho mở rộng trong tương lai.
2. Sử dụng chi phí:
   - Khi mảng đầy và mở rộng, chi phí thực tế là $O(m)$ với $m$ là kích thước hiện tại.
   - Phần tử ở nửa sau của mảng trước khi mở rộng (khoảng $n/2$ phần tử) khi được chèn đã tích luỹ đủ chi phí để trả cho việc sao chép khi mở rộng.

Ví dụ minh họa:

```text
// Trạng thái ban đầu:
arr    = [1, 2, 3, 4]  // mảng ban đầu
amount = [2, 2, 2, 2]  // mỗi phần tử lưu trữ chi phí dự phòng

// Lần mở rộng đầu: mảng đầy, cần mở rộng
arr    = [1, 2, 3, 4, null, null, null, null]  // sau khi mở rộng
amount = [2, 2, 0, 0, 0, 0, 0, 0]  // chi phí của 3,4 được dùng để trả cho việc sao chép

// Tiếp tục chèn đến khi đầy lại
arr    = [1, 2, 3, 4, 5, 6, 7, 8]  // lấp đầy mảng
amount = [2, 2, 0, 0, 2, 2, 2, 2]  // các phần tử mới cũng lưu trữ chi phí dự phòng

// Lần mở rộng thứ hai: mảng lại đầy, mở rộng thêm
arr    = [1, 2, 3, 4, 5, 6, 7, 8, null, null, null, null, null, null, null, null]  // sau mở rộng
amount = [2, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]  // chi phí của 5,6,7,8 dùng để trả cho việc sao chép
```

Quá trình trên cho thấy chi phí dự phòng mỗi lần chèn đủ để trả cho các lần mở rộng sau này, nên chi phí bình quân mỗi thao tác vẫn là $O(1)$.

### Phương pháp thế năng

Phương pháp thế năng (Potential Method) định nghĩa một hàm thế năng $\Phi$ để đo “năng lượng tiềm ẩn” trong trạng thái cấu trúc dữ liệu, năng lượng này có thể được dùng để trả cho các thao tác tốn kém trong tương lai. Thay đổi thế năng giữa hai trạng thái được dùng để cân bằng chi phí thực tế của thao tác, từ đó suy ra chi phí bình quân.

Nguyên lý:

Định nghĩa trạng thái $S$ của cấu trúc dữ liệu tại một thời điểm (ví dụ số phần tử, dung lượng, các con trỏ...), với trạng thái ban đầu là $S_0$ (chưa có thao tác). Định nghĩa hàm thế năng $\Phi(S)$ sao cho:
1. Thế năng ban đầu: $\Phi(S_0) = 0$.
2. Tính không âm: với mọi trạng thái $S$, $\Phi(S) \ge 0$.

Với mỗi thao tác, chi phí bình quân $\hat{c}$ được định nghĩa bởi:
$$
\hat{c} = c + \Phi(S') - \Phi(S)
$$
trong đó $c$ là chi phí thực tế, $S$ và $S'$ là trạng thái trước và sau thao tác. Nếu thao tác tăng thế năng thì chi phí bình quân tăng; nếu thao tác giảm thế năng thì chi phí bình quân giảm.

Xét chuỗi trạng thái $S_1, S_2, \dots, S_m$ sau $m$ thao tác từ trạng thái $S_0$, với chi phí thực tế các thao tác là $c_i$, chi phí bình quân của thao tác thứ $i$ là:
$$
p_i = c_i + \Phi(S_i) - \Phi(S_{i-1})
$$
Do đó tổng chi phí thực tế của $m$ thao tác là:
$$
\sum_{i=1}^m c_i = \sum_{i=1}^m p_i + \Phi(S_0) - \Phi(S_m)
$$
Vì $\Phi(S_m) \ge \Phi(S_0)$, ta có:
$$
\sum_{i=1}^m p_i \ge \sum_{i=1}^m c_i
$$
Do đó nếu $p_i = O(T(n))$ thì $O(T(n))$ là một giới trên cho độ phức tạp bình quân.

#### Ví dụ: phân tích mở rộng mảng động

Với thao tác chèn trên `vector`, định nghĩa hàm thế năng:
$$
\Phi(h) = 2n - m
$$
trong đó $n$ là số phần tử hiện có, $m$ là dung lượng hiện tại. Hàm thế năng phản ánh không gian còn trống so với dung lượng.

1. Chèn (không cần mở rộng):
   - Chi phí thực tế: $O(1)$.
   - Thay đổi thế năng: khi chèn thêm 1 phần tử, thế năng tăng $2$.
     - $\Phi(h') - \Phi(h) = 2(n + 1) - m - (2n - m) = 2$
   - Chi phí bình quân: $1 + 2 = 3$.

2. Chèn (khi gây mở rộng):
   - Giả sử $m = n$, chèn sẽ gây mở rộng lên $2n$.
   - Chi phí thực tế: $O(n)$ do sao chép $n$ phần tử rồi chèn.
   - Thay đổi thế năng: sau mở rộng, thế năng giảm $n - 2$ (hoặc nói trực tiếp là $2 - n$ theo biểu thức).
     - $\Phi(h') - \Phi(h) = 2(n + 1) - 2n - (2n - n) = 2 - n$
   - Chi phí bình quân: $n + 1 + (2 - n) = 3$.

Từ đó ta thấy dù thao tác mở rộng có chi phí lớn, thiết kế hàm thế năng cho phép chi phí bình quân mỗi chèn là $O(1)$.

## Ví dụ mở rộng: các thao tác trên ngăn xếp

Ngăn xếp là ví dụ kinh điển cho phân tích bình quân. Giả sử ngăn xếp `S` hỗ trợ các thao tác:

| Thao tác               | Ý nghĩa           | Chi phí thực tế $c_i$                   |
| --------------------- | ---------------- | -------------------------------------- |
| `S.push(x)`           | đẩy phần tử x lên | $1$                                    |
| `S.pop()`             | lấy phần tử trên cùng | $1$                                |
| `S.multi-pop(k)`      | pop k phần tử     | $O(\min{\lvert S\rvert, k})$           |

Phân tích bằng ba phương pháp:

### Phân tích tập hợp

Tính tổng chi phí rồi chia đều:
1. Với $n_{push}$ lần `push(x)`, mỗi lần $O(1)$, tổng $O(n_{push})$.
2. Với $n_{pop}$ lần `pop()`, mỗi lần $O(1)$, tổng $O(n_{pop})$.
3. Với $n_{multi-pop}$ lần `multi-pop(k)`, tổng số phần tử bị pop không vượt quá số phần tử đã được push, nên tổng chi phí bị giới hạn bởi $n_{push}$.

Vì tổng số thao tác $n = n_{push} + n_{pop} + n_{multi-pop} \le 2 \times n_{push}$, tổng chi phí là $O(n_{push}) = O(n)$, nên mỗi thao tác có chi phí bình quân $O(1)$.

### Phương pháp kế toán

Dành trước chi phí cho mỗi `push(x)` để trả cho `pop()` hoặc `multi-pop(k)` sau này.
1. `S.push(x)`: gán chi phí bình quân là $2$ (1 cho thao tác hiện tại, 1 để dành).
2. `S.pop()`: chi phí thực tế $1$, nhưng đã được trả bởi chi phí dự phòng của một `push(x)`, nên chi phí bình quân khai báo là $0$.
3. `S.multi-pop(k)`: mỗi phần tử pop có chi phí 1 được trả bởi chi phí dự phòng của lần push tương ứng, nên chi phí bình quân là $0$.

Do vậy chi phí bình quân mỗi thao tác là $O(1)$.

### Phương pháp thế năng

Đặt hàm thế năng là số phần tử hiện có: $\Phi(h) = \lvert S \rvert$ (mỗi phần tử đóng góp 1 đơn vị thế năng).
1. `S.push(x)`: tăng thế năng 1, chi phí bình quân $1 + 1 = 2$.
2. `S.pop()`: giảm thế năng 1, chi phí bình quân $1 - 1 = 0$.
3. `S.multi-pop(k)`: giảm thế năng $k$, chi phí bình quân $k - k = 0$.

Vì vậy `push(x)` chi phí bình quân là $2$, còn `pop()` và `multi-pop(k)` là $0$; tổng thể mỗi thao tác có chi phí bình quân $O(1)$.

## Tài liệu tham khảo

- [Amortized Analysis - Wikipedia](https://en.wikipedia.org/wiki/Amortized_analysis)
- [Cornell CS 3110 - Lecture 20: Amortized Analysis](https://www.cs.cornell.edu/courses/cs3110/2011sp/Lectures/lec20-amortized/amortized.htm)
