## Định nghĩa

Quy hoạch động khoảng (Interval DP) là một dạng mở rộng của quy hoạch động tuyến tính. Khi phân chia bài toán theo các giai đoạn, nó có mối quan hệ mật thiết với thứ tự xuất hiện của các phần tử trong giai đoạn đó và việc các phần tử nào từ giai đoạn trước được hợp nhất lại.

Giả sử trạng thái $f(i, j)$ biểu thị giá trị tối ưu nhận được khi hợp nhất tất cả các phần tử từ vị trí chỉ số $i$ đến $j$. Khi đó, $f(i, j) = \max\{f(i, k) + f(k+1, j) + cost\}$, với $cost$ là giá trị tiêu tốn (hoặc nhận được) khi hợp nhất hai nhóm phần tử này.

## Tính chất

Interval DP có các đặc điểm sau:

**Hợp nhất**: Tức là tích hợp hai hoặc nhiều phần lại với nhau, tất nhiên cũng có thể làm ngược lại;

**Đặc trưng**: Có thể phân giải bài toán thành dạng các cặp có thể hợp nhất với nhau;

**Giải quyết**: Thiết lập giá trị tối ưu cho toàn bộ bài toán, duyệt qua các điểm hợp nhất (điểm chia), chia bài toán thành hai phần trái và phải, cuối cùng hợp nhất giá trị tối ưu của hai phần để có được giá trị tối ưu của bài toán gốc.

## Giải thích

### Ví dụ

???+ note "[「NOI1995」Hợp nhất sỏi](https://loj.ac/problem/10147)"
    Nội dung tóm tắt: Trên một vòng tròn có $n$ đống sỏi với số lượng $a_1, a_2, \dots, a_n$. Thực hiện $n-1$ lần thao tác hợp nhất, mỗi lần hợp nhất hai đống kề nhau thành một đống mới, nhận được số điểm bằng tổng số sỏi trong đống mới đó. Bạn cần tìm cách hợp nhất để tổng số điểm nhận được là lớn nhất.

Cần xem xét trường hợp không phải trên vòng tròn mà là trên một chuỗi hàng ngang.

Gọi $f(i, j)$ là số điểm lớn nhất khi hợp nhất tất cả sỏi trong khoảng $[i, j]$ lại với nhau.

Viết **phương trình chuyển trạng thái**: $f(i, j) = \max\{f(i, k) + f(k+1, j) + \sum_{t=i}^{j} a_t \}~(i \le k < j)$

Gọi $sum_i$ là tổng tiền tố của mảng $a$, phương trình chuyển trạng thái trở thành $f(i, j) = \max\{f(i, k) + f(k+1, j) + sum_j - sum_{i-1}\}$.

### Cách thực hiện chuyển trạng thái

Vì khi tính giá trị của $f(i, j)$ cần biết tất cả các giá trị $f(i, k)$ và $f(k+1, j)$, mà số lượng phần tử chứa trong hai trạng thái này đều nhỏ hơn $f(i, j)$, nên chúng ta lấy độ dài $len = j - i + 1$ làm giai đoạn của DP. Đầu tiên duyệt $len$ từ nhỏ đến lớn, sau đó duyệt giá trị của $i$, dựa vào $len$ và $i$ để tính ra giá trị của $j$, cuối cùng duyệt $k$. Độ phức tạp thời gian là $O(n^3)$.

### Cách xử lý vòng tròn

Trong đề bài, các đống sỏi xếp thành một vòng tròn chứ không phải một chuỗi, phải làm sao?

**Cách 1**: Vì các đống sỏi tạo thành vòng tròn, ta có thể duyệt qua từng vị trí ngắt để biến vòng tròn thành một chuỗi. Do phải duyệt $n$ lần, độ phức tạp thời gian cuối cùng là $O(n^4)$.

**Cách 2**: Chúng ta kéo dài chuỗi này gấp đôi thành $2 \times n$ đống, trong đó đống thứ $n+i$ giống hệt đống thứ $i$. Sau khi giải bằng quy hoạch động, lấy giá trị tối ưu trong các kết quả $f(1, n), f(2, n+1), \dots, f(n, 2n-1)$ làm đáp án cuối cùng. Độ phức tạp thời gian là $O(n^3)$.

## Cài đặt

=== "C++"
    ```cpp
    for (len = 2; len <= n; len++)
      for (i = 1; i <= 2 * n - len; i++) {
        int j = len + i - 1;
        for (k = i; k < j; k++)
          f[i][j] = max(f[i][j], f[i][k] + f[k + 1][j] + sum[j] - sum[i - 1]);
      }
    ```

=== "Python"
    ```python
    for len in range(2, n + 1):
        for i in range(1, 2 * n - len + 1):
            j = len + i - 1
            for k in range(i, j):
                f[i][j] = max(f[i][j], f[i][k] + f[k + 1][j] + sum[j] - sum[i - 1])
    ```

## Một số bài tập luyện tập

[NOIP 2006 Vòng cổ năng lượng](https://www.luogu.com.cn/problem/P1063)

[NOIP 2007 Trò chơi lấy số trên ma trận](https://www.luogu.com.cn/problem/P1005)

[「IOI2000」Bưu điện](https://www.luogu.com.cn/problem/P4767)