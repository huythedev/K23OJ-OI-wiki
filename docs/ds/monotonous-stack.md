## Giới thiệu

Ngăn xếp đơn điệu (Monotonic Stack) là gì? Đúng như tên gọi, ngăn xếp đơn điệu là một cấu trúc ngăn xếp thỏa mãn tính đơn điệu. So với hàng đợi đơn điệu, nó chỉ thực hiện việc thêm và xóa phần tử ở một đầu.

Để thuận tiện cho việc mô tả, các ví dụ và mã giả dưới đây sẽ sử dụng việc duy trì một ngăn xếp số nguyên đơn điệu tăng làm ví dụ.

## Quá trình

### Chèn

Khi chèn một phần tử vào ngăn xếp đơn điệu, để duy trì tính đơn điệu của ngăn xếp, cần phải xóa ít phần tử nhất có thể sao cho sau khi chèn phần tử đó vào đỉnh ngăn xếp, toàn bộ ngăn xếp vẫn thỏa mãn tính đơn điệu.

Ví dụ, các phần tử trong ngăn xếp từ đỉnh xuống đáy là $\{0,11,45,81\}$.

![](images/monotonous-stack-before.svg)

Khi chèn phần tử $14$, để đảm bảo tính đơn điệu, cần lần lượt xóa các phần tử $0, 11$. Sau thao tác này, ngăn xếp trở thành $\{14,45,81\}$.

![](images/monotonous-stack-after.svg)

Mô tả bằng mã giả như sau:

???+ note "Hiện thực"
    ```text
    insert x
    while !sta.empty() && sta.top()<x
        sta.pop()
    sta.push(x)
    ```

### Sử dụng

Đơn giản là lấy một phần tử từ đỉnh ngăn xếp, phần tử này thỏa mãn cực trị của tính đơn điệu (ví dụ: nhỏ nhất hoặc lớn nhất).

Trong ví dụ trên, phần tử lấy ra chính là giá trị nhỏ nhất trong ngăn xếp.

## Ứng dụng

??? note "[POJ3250 Bad Hair Day](http://poj.org/problem?id=3250)"
    Có $N$ con bò xếp thành một hàng từ trái sang phải, mỗi con bò có chiều cao $h_i$. Gọi $c_i$ là số lượng con bò nằm giữa con bò thứ $i$ (tính từ trái sang) và "con bò đầu tiên bên phải có chiều cao $\ge h_i$". Hãy tính $\sum_{i=1}^{N} c_i$.

Một ứng dụng cơ bản là bài toán này, đây là một ứng dụng đơn giản của ngăn xếp đơn điệu. Ghi lại vị trí mà mỗi con bò bị đẩy ra khỏi ngăn xếp, nếu chưa từng bị đẩy ra thì vị trí đó là cực phải. Sau một chút xử lý, ta có thể tính được kết quả yêu cầu của bài toán.

Ngoài ra, ngăn xếp đơn điệu cũng có thể được sử dụng để giải quyết bài toán RMQ (Range Minimum/Maximum Query) offline.

Chúng ta có thể sắp xếp tất cả các truy vấn theo điểm cuối bên phải, sau đó mỗi lần quét trên dãy từ trái sang phải đến điểm cuối bên phải của truy vấn hiện tại, và chèn các phần tử đã quét vào ngăn xếp đơn điệu. Như vậy, mỗi lần trả lời truy vấn, các giá trị được lưu trữ trong ngăn xếp đơn điệu là các điểm quyết định có thể trở thành đáp án với vị trí $\le r$, và các phần tử này thỏa mãn tính chất đơn điệu. Lúc này, phần tử đầu tiên trong ngăn xếp đơn điệu có vị trí $\ge l$ chính là đáp án cho truy vấn hiện tại, quá trình này có thể được thực hiện bằng tìm kiếm nhị phân. Độ phức tạp thời gian khi sử dụng ngăn xếp đơn điệu để giải quyết bài toán RMQ là $O(q\log q + q\log n)$, độ phức tạp không gian là $O(n)$.

## Bài tập

-   [Luogu P5788【Template】Monotonic Stack](https://www.luogu.com.cn/problem/P5788)
-   [Luogu P1901 Trạm phát sóng](https://www.luogu.com.cn/problem/P1901)