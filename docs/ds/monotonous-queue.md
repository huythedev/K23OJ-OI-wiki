author: Link-cute, Xeonacid, ouuan, Alphnia, Lyccrius

## Giới thiệu

Trước khi học về hàng đợi đơn điệu, chúng ta hãy xem qua một bài toán ví dụ.

???+ note "Bài toán ví dụ"
    [Sliding Window](http://poj.org/problem?id=2823)
    
    Bài toán yêu cầu cho một mảng có độ dài $n$, hãy lập trình để xuất ra giá trị lớn nhất và nhỏ nhất trong mỗi $k$ số liên tiếp.

Ý tưởng đơn giản nhất là vét cạn, đối với mỗi đoạn $i \sim i+k-1$, so sánh từng phần tử để tìm ra giá trị lớn nhất (và nhỏ nhất), độ phức tạp thời gian khoảng $O(n \times k)$.

Rõ ràng, cách này thực hiện rất nhiều công việc lặp lại, ngoại trừ $k-1$ số đầu và $k-1$ số cuối, mỗi số đều được so sánh $k$ lần. Với $100\%$ dữ liệu bài toán có $n \le 1000000$, khi $k$ hơi lớn, chắc chắn sẽ bị TLE (Time Limit Exceeded).

Đây chính là lúc hàng đợi đơn điệu (Monotonic Queue) phát huy tác dụng.

## Định nghĩa

Như tên gọi, trọng tâm của hàng đợi đơn điệu bao gồm "đơn điệu" và "hàng đợi".

"Đơn điệu" ám chỉ "quy luật" của các phần tử - tăng dần (hoặc giảm dần).

"Hàng đợi" ám chỉ các phần tử chỉ có thể được thao tác từ đầu hàng đợi và cuối hàng đợi.

Ps. "Hàng đợi" trong hàng đợi đơn điệu có sự khác biệt nhất định so với hàng đợi thông thường, sẽ được đề cập sau.

## Phân tích bài toán ví dụ

### Giải thích

Với khái niệm "hàng đợi đơn điệu" ở trên, dễ dàng nghĩ đến việc sử dụng hàng đợi đơn điệu để tối ưu hóa.

Yêu cầu là tìm giá trị lớn nhất (nhỏ nhất) trong mỗi $k$ số liên tiếp. Rõ ràng, khi một số đi vào phạm vi cần "tìm" giá trị lớn nhất, nếu số này lớn hơn số đứng trước nó (vào hàng đợi trước), thì rõ ràng số đứng trước sẽ ra khỏi hàng đợi trước số này và không thể là giá trị lớn nhất nữa.

Nghĩa là - khi thỏa mãn điều kiện trên, ta có thể "loại bỏ" các số đứng trước, sau đó mới thực sự push số này vào cuối hàng đợi.

Điều này tương đương với việc duy trì một hàng đợi giảm dần, phù hợp với định nghĩa của hàng đợi đơn điệu, giảm thiểu số lần so sánh lặp lại. Hơn nữa, vì hàng đợi được duy trì nằm trong phạm vi truy vấn và giảm dần, nên đầu hàng đợi chắc chắn là giá trị lớn nhất trong vùng truy vấn đó, do đó khi xuất kết quả chỉ cần xuất phần tử đầu hàng đợi.

Dễ thấy rằng, trong thuật toán như vậy, mỗi số chỉ cần vào hàng đợi và ra khỏi hàng đợi một lần, do đó độ phức tạp thời gian giảm xuống còn $O(n)$.

Và vì độ dài khoảng truy vấn là cố định, giá trị vượt quá không gian truy vấn dù lớn đến đâu cũng không thể xuất ra, nên cần thêm mảng site để ghi lại vị trí của số thứ $i$ trong hàng đợi so với mảng gốc, nhằm loại bỏ phần tử đầu hàng đợi đã vượt quá giới hạn.

### Quá trình

Ví dụ, nếu chúng ta xây dựng một hàng đợi đơn điệu tăng dần sẽ như sau:

Dãy số ban đầu là:

```text
1 3 -1 -3 5 3 6 7
```

Vì chúng ta luôn phải duy trì hàng đợi đảm bảo tính chất **tăng dần**, nên sẽ xảy ra các sự việc sau: (giả sử $k = 3$)

| Thao tác | Trạng thái hàng đợi |
| --- | --- |
| 1 vào hàng đợi | `{1}` |
| 3 lớn hơn 1, 3 vào hàng đợi | `{1 3}` |
| -1 nhỏ hơn tất cả phần tử trong hàng đợi, nên xóa sạch hàng đợi, -1 vào hàng đợi | `{-1}` |
| -3 nhỏ hơn tất cả phần tử trong hàng đợi, nên xóa sạch hàng đợi, -3 vào hàng đợi | `{-3}` |
| 5 lớn hơn -3, vào hàng đợi trực tiếp | `{-3 5}` |
| 3 nhỏ hơn 5, 5 ra hàng đợi, 3 vào hàng đợi | `{-3 3}` |
| -3 đã ra khỏi cửa sổ trượt, nên -3 ra hàng đợi; 6 lớn hơn 3, 6 vào hàng đợi | `{3 6}` |
| 7 lớn hơn 6, 7 vào hàng đợi | `{3 6 7}` |

???+ note "Mã nguồn tham khảo bài toán ví dụ"
    ```cpp
    --8<-- "docs/ds/code/monotonous-queue/monotonous-queue_1.cpp"
    ```

Ps. Một điểm khác biệt lớn giữa "hàng đợi" ở đây và hàng đợi thông thường là có thể thao tác từ cuối hàng đợi, trong STL có cấu trúc dữ liệu tương tự là deque.

???+ note "Bài toán ví dụ 2 [Luogu P2698 Flowerpot S](https://www.luogu.com.cn/problem/P2698)"
    Cho tọa độ của $N$ giọt nước, $y$ biểu thị độ cao của giọt nước, $x$ biểu thị vị trí nó rơi xuống trục $x$. Mỗi giọt nước rơi với tốc độ 1 đơn vị độ dài mỗi giây. Bạn cần đặt chậu hoa ở một vị trí nào đó trên trục $x$, sao cho khoảng chênh lệch thời gian từ lúc hứng được giọt nước đầu tiên đến lúc hứng được giọt nước cuối cùng ít nhất là $D$.
    Chúng ta coi rằng, chỉ cần giọt nước rơi xuống trục $x$, thẳng hàng với mép chậu hoa, thì coi như được hứng. Cho tọa độ của $N$ giọt nước và kích thước $D$, hãy tính chiều rộng $W$ nhỏ nhất của chậu hoa. $1\leq N \leq 100000 , 1 \leq D \leq 1000000, 0 \leq x,y\leq 10^6$

Sau khi sắp xếp tất cả các giọt nước theo tọa độ $x$, đề bài có thể chuyển thành tìm một khoảng có hiệu tọa độ $x$ nhỏ nhất sao cho hiệu giữa giá trị lớn nhất và nhỏ nhất của tọa độ $y$ trong khoảng này ít nhất là $D$. Chúng ta nhận thấy bài toán này có điểm tương đồng với bài toán ví dụ trước, đều liên quan đến giá trị lớn nhất nhỏ nhất trong một khoảng, nhưng kích thước khoảng trong bài này không xác định, và bản thân kích thước khoảng lại là đáp án chúng ta cần tìm.

Chúng ta vẫn có thể sử dụng hai hàng đợi đơn điệu, một tăng dần và một giảm dần, để duy trì giá trị lớn nhất và nhỏ nhất trong $[L,R]$ khi $R$ liên tục dịch chuyển về phía sau. Tuy nhiên lúc này ta nhận thấy, nếu $L$ cố định, thì giá trị lớn nhất trong $[L,R]$ sẽ chỉ ngày càng lớn, giá trị nhỏ nhất chỉ ngày càng nhỏ, nên đặt $f(R) = \max[L,R]-\min[L,R]$, thì $f(R)$ là một hàm tăng theo $R$, do đó $f(R)\geq D \implies f(r)\geq D,R\lt r \leq N$. Điều này cho thấy với mỗi $L$ cố định, $R$ đầu tiên thỏa mãn điều kiện chính là đáp án tối ưu.
Vì vậy, quy trình giải tổng thể của chúng ta là: trước tiên cố định $L$, di chuyển $R$ từ trước ra sau, sử dụng hai hàng đợi đơn điệu để duy trì cực trị của $[L,R]$. Khi tìm thấy $R$ đầu tiên thỏa mãn điều kiện, cập nhật đáp án và di chuyển $L$ về phía sau. Khi $L$ di chuyển về phía sau, hai hàng đợi đơn điệu cũng cần kịp thời loại bỏ phần tử đầu hàng đợi. Như vậy, cho đến khi $R$ di chuyển đến cuối cùng, mỗi phần tử vẫn vào và ra hàng đợi một lần, đảm bảo độ phức tạp thời gian là $O(n)$.

???+ note "Mã nguồn tham khảo"
    ```cpp
    --8<-- "docs/ds/code/monotonous-queue/monotonous-queue_2.cpp"
    ```