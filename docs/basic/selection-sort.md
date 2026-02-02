Trang này sẽ giới thiệu ngắn gọn về chọn lọc (Selection Sort).

## Định nghĩa

Sắp xếp chọn (Tiếng Anh: Selection sort) là một thuật toán sắp xếp đơn giản và trực quan. Nguyên lý là ở mỗi bước tìm phần tử nhỏ thứ i (tức là phần tử nhỏ nhất trong A_{i..n}), rồi hoán đổi phần tử đó với phần tử tại vị trí i của mảng.

![selection sort animate example](images/selection-sort-animate.svg)

## Tính chất

### Tính ổn định

Tính ổn định của selection sort phụ thuộc vào cách triển khai.

Nếu dùng danh sách liên kết (linked list), vì chèn/xóa ở vị trí bất kỳ có thể là O(1), ta không cần dùng swap; mỗi lần chọn phần tử nhỏ nhất từ phần chưa sắp và chèn nó trước phần đầu của phần chưa sắp sẽ đảm bảo tính ổn định.

Nếu dùng mảng (cách thường dùng trong OI), chèn/xóa ở giữa mảng là O(n), nên thường dùng swap để di chuyển phần tử, dẫn đến lựa chọn mảng của selection sort là không ổn định.

Ví dụ cài đặt dưới đây đều dùng hoán đổi phần tử trong mảng, nên là không ổn định.

### Độ phức tạp thời gian

Độ phức tạp thời gian tốt nhất, trung bình và tệ nhất của selection sort đều là O(n^2).

## Cài đặt

### Pseudocode

$$
\begin{array}{ll}
1 & \textbf{Input. } \text{An array } A \text{ consisting of }n\text{ elements.} \\
2 & \textbf{Output. } A\text{ will be sorted in nondecreasing order.} \\
3 & \textbf{Method. }  \\
4 & \textbf{for } i\gets 1\textbf{ to }n-1\\
5 & \qquad ith\gets i\\
6 & \qquad \textbf{for }j\gets i+1\textbf{ to }n\\
7 & \qquad\qquad\textbf{if }A[j]<A[ith]\\
8 & \qquad\qquad\qquad ith\gets j\\
9 & \qquad \text{swap }A[i]\text{ and }A[ith]\\
\end{array}
$$

=== "C++"
    ```cpp
    --8<-- "docs/basic/code/selection-sort/selection-sort_1.cpp"
    ```

=== "Python"
    ```python
    --8<-- "docs/basic/code/selection-sort/selection-sort_1.py:core"
    ```

=== "Java"
    ```java
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/selection-sort.md
    // Chỉ số mảng arr bắt đầu từ 1
    static void selection_sort(int[] arr, int n) {
        for (int i = 1; i < n; i++) {
            int ith = i;
            for (int j = i + 1; j <= n; j++) {
                if (arr[j] < arr[ith]) {
                    ith = j;
                }
            }
            // hoán đổi
            int temp = arr[i];
            arr[i] = arr[ith];
            arr[ith] = temp;
        }
    }
    ```
