## Định nghĩa

DAG chính là [Đồ thị có hướng không chu trình](../graph/dag.md). Một số quan hệ nhị phân trong các bài toán thực tế có thể được mô hình hóa bằng DAG, từ đó chuyển đổi các bài toán này thành bài toán tìm đường đi dài (ngắn) nhất trên DAG.

## Giải thích

Lấy bài toán này làm ví dụ để phân tích quá trình mô hình hóa DAG.

???+ note "Ví dụ [UVa 437 Tòa tháp Babylon The Tower of Babylon](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=378)"
    Có $n (n\leqslant 30)$ loại khối gạch, biết độ dài ba cạnh, mỗi loại có số lượng vô hạn. Yêu cầu chọn một số khối lập phương chồng thành một cột cao nhất có thể (mỗi khối gạch có thể tự chọn một cạnh làm chiều cao), sao cho chiều dài và chiều rộng mặt đáy của mỗi khối gạch phải nhỏ hơn nghiêm ngặt chiều dài và chiều rộng mặt đáy của khối gạch nằm dưới nó. Tìm chiều cao tối đa của tháp.

## Quá trình

### Xây dựng DAG

Vì chiều dài và chiều rộng mặt đáy của mỗi khối gạch phải nhỏ hơn nghiêm ngặt khối gạch bên dưới, nên không khó để dùng quan hệ này làm cơ sở dựng đồ thị, và bài toán này sẽ chuyển thành bài toán đường đi dài nhất.

Nói cách khác, nếu khối gạch $j$ có thể đặt lên trên khối gạch $i$, thì giữa $i$ và $j$ tồn tại một cạnh $(i, j)$, và trọng số cạnh chính là chiều cao được chọn của khối gạch $j$.

Một vấn đề khác của bài này là mỗi khối gạch có ba cách chọn chiều cao, vậy dựng đồ thị thế nào cho phù hợp?

Ta có thể phân rã mỗi khối gạch thành ba cách xếp chồng, tức là chia một khối gạch thành ba thực thể khác nhau, mỗi thực thể chọn một cạnh khác nhau làm chiều cao.

Điểm bắt đầu ban đầu là mặt đất, mặt đất có diện tích đáy vô hạn, nên mặt đất có thể nối đến bất kỳ khối gạch nào. Tất nhiên khi lập trình chúng ta không cần viết cụ thể giá trị vô hạn.

Giả sử có hai khối gạch với các cạnh lần lượt là $31, 41, 59$ và $33, 83, 27$, thì toàn bộ DAG sẽ như hình dưới đây.

![](./images/dag-babylon.png)

Trong hình, khung nét liền màu xanh lam biểu thị một nhóm khối gạch được phân rã từ một khối gạch gốc. Sở dĩ dùng $\{\}$ để biểu thị cạnh đáy là vì một khi khối gạch đã chọn chiều cao, thứ tự chiều dài và chiều rộng mặt đáy là không quan trọng (miễn là thỏa mãn điều kiện nhỏ hơn).

Khung nét đứt màu vàng biểu thị phần tính toán bị lặp lại, có thể sử dụng phương pháp [Tìm kiếm ghi nhớ](./memo.md) để tránh việc này.

### Chuyển trạng thái

Yêu cầu của đề bài là chiều cao tối đa của tháp, đã được chuyển thành bài toán đường đi dài nhất. Điểm bắt đầu như đã nêu trên là mặt đất, vậy điểm kết thúc ở đâu? Hiển nhiên điểm kết thúc được xác định tự nhiên khi không thể đặt thêm khối gạch nào lên trên một khối gạch nữa.

Tiếp theo chúng ta xem xét phương trình chuyển trạng thái.

Gọi $d(i, r)$ là chiều cao tối đa khi khối gạch thứ $i$ nằm ở dưới cùng và được đặt theo cách thứ $r$. Khi đó ta có phương trình chuyển trạng thái sau:

$$
d(i, r) = \max\left\{d(j, r') + h\right\}
$$

Trong đó $j$ là tất cả những khối gạch có thể đặt lên trên khối gạch $i$ khi $i$ đang ở trạng thái $r$, $r'$ tương ứng với cách đặt của $j$ lúc đó, và $h$ là chiều cao của khối gạch $i$ khi chọn cách đặt thứ $r$.

??? note "Cài đặt"
    ```cpp
    --8<-- "docs/dp/code/dag/dag_1.cpp"
    ```