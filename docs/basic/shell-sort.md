Trang này giới thiệu ngắn gọn về Shell Sort.

## Định nghĩa

Shell sort (Tiếng Anh: Shell sort), còn gọi là phương pháp giảm dần khoảng cách, là phiên bản cải tiến của [insertion sort](./insertion-sort.md). Được đặt theo tên người phát minh Donald Shell.

## Quá trình

Sắp xếp bằng cách so sánh và di chuyển các phần tử không liền kề:

1.  Chia dãy cần sắp thành nhiều dãy con (mỗi dãy con gồm các phần tử có cùng khoảng cách trong mảng gốc);
2.  Thực hiện insertion sort cho từng dãy con;
3.  Giảm khoảng cách giữa các phần tử rồi lặp lại cho tới khoảng cách bằng 1.

## Tính chất

### Tính ổn định

Shell sort không phải là thuật toán ổn định.

### Độ phức tạp thời gian

Độ phức tạp tốt nhất của Shell sort là O(n).

Độ phức tạp trung bình và tệ nhất phụ thuộc vào dãy khoảng cách H; dưới đây đưa hai lựa chọn cổ điển của H, cả hai đều giúp độ phức tạp giảm xuống dưới O(n^2).

???+ note "Mệnh đề 1"
    Nếu H = {2^k - 1 | k = 1,2,..., floor(log_2 n)} (theo thứ tự giảm dần), thì thời gian của Shell sort là O(n^{3/2}).

???+ note "Mệnh đề 2"
    Nếu H = {k = 2^p * 3^q | p,q \in \mathbb N, k \le n} (theo thứ tự giảm dần), thì thời gian là O(n \log^2 n).

Để chứng minh, ta cần một định lý phản ánh đặc điểm chính của Shell sort.

???+ note "Định lý 1"
    Chỉ cần thực hiện một lần InsertionSort(h), bất kể sau đó gọi InsertionSort như thế nào và A biến đổi ra sao, thì tính chất sau luôn được giữ:
    
    $$
    \begin{array}{c}
    A_1,A_{1+h},A_{1+2h},\ldots \\
    A_2,A_{2+h},A_{2+2h},\ldots \\
    \vdots \\
    A_h,A_{h+h},A_{h+2h},\ldots
    \end{array}
    $$

Bằng chứng và các dẫn luận tiếp theo được nêu chi tiết trong file gốc.

### Độ phức tạp không gian

Shell sort có độ phức tạp không gian O(1).

## Cài đặt

=== "C++[^ref1]"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/shell-sort.md
    template <typename T>
    void shell_sort(T array[], int length) {
      int h = 1;
      while (h < length / 3) {
        h = 3 * h + 1;
      }
      while (h >= 1) {
        for (int i = h; i < length; i++) {
          for (int j = i; j >= h && array[j] < array[j - h]; j -= h) {
            std::swap(array[j], array[j - h]);
          }
        }
        h = h / 3;
      }
    }
    ```

=== "Python"
    ```python
    # filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/shell-sort.md
    def shell_sort(array, length):
        h = 1
        while h < length / 3:
            h = int(3 * h + 1)
        while h >= 1:
            for i in range(h, length):
                j = i
                while j >= h and array[j] < array[j - h]:
                    array[j], array[j - h] = array[j - h], array[j]
                    j -= h
            h = int(h / 3)
    ```

## Tài liệu tham khảo và chú thích

[^ref1]: [希尔排序 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F)
