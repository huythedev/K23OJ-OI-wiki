Trang này sẽ giới thiệu ngắn gọn về **sắp xếp chèn** (insertion sort).

## Định nghĩa

**Sắp xếp chèn** (tiếng Anh: Insertion sort) là một thuật toán sắp xếp đơn giản và trực quan. Nguyên lý là chia dãy cần sắp xếp thành hai phần: "đã sắp xếp" và "chưa sắp xếp", mỗi lần lấy một phần tử từ phần "chưa sắp xếp" chèn vào đúng vị trí trong phần "đã sắp xếp".

Một ví dụ tương tự là khi chơi bài, mỗi lần bốc một lá bài từ bàn, bạn sẽ chèn nó vào đúng vị trí trong bộ bài trên tay.

![insertion sort animate example](images/insertion-sort-animate.svg)

## Tính chất

### Tính ổn định

Sắp xếp chèn là một thuật toán **ổn định**.

### Độ phức tạp thời gian

Trường hợp tốt nhất của sắp xếp chèn là $O(n)$, rất hiệu quả khi dãy gần như đã có thứ tự.

Trường hợp xấu nhất và trung bình đều là $O(n^2)$.

## Cài đặt mã nguồn

### Giả mã

$$
\begin{array}{ll}
1 & \textbf{Input. } \text{Một mảng } A \text{ gồm }n\text{ phần tử.} \\
2 & \textbf{Output. } A\text{ sẽ được sắp xếp tăng dần, ổn định.} \\
3 & \textbf{Method. }  \\
4 & \textbf{for } i\gets 2\textbf{ to }n\\
5 & \qquad key\gets A[i]\\
6 & \qquad j\gets i-1\\
7 & \qquad\textbf{while }j>0\textbf{ and }A[j]>key\\
8 & \qquad\qquad A[j + 1]\gets A[j]\\
9 & \qquad\qquad j\gets j - 1\\
10 & \qquad A[j + 1]\gets key
\end{array}
$$

=== "C++"
    ```cpp
    --8<-- "docs/basic/code/insertion-sort/insertion-sort_1.cpp"
    ```

=== "Python"
    ```python
    --8<-- "docs/basic/code/insertion-sort/insertion-sort_1.py:core"
    ```

=== "Java"
    ```java
    --8<-- "docs/basic/code/insertion-sort/insertion-sort_1.java"
    ```

## Sắp xếp chèn nhị phân

Sắp xếp chèn còn có thể tối ưu bằng thuật toán nhị phân, đặc biệt hiệu quả khi số lượng phần tử lớn.

### Độ phức tạp thời gian

Sắp xếp chèn nhị phân và sắp xếp chèn thông thường có ý tưởng giống nhau, chỉ khác ở chỗ tối ưu hằng số trong độ phức tạp thời gian, nên độ phức tạp vẫn không đổi.

### Cài đặt mã nguồn

=== "C++"
    ```cpp
    void insertion_sort(int arr[], int len) {
      if (len < 2) return;
      for (int i = 1; i != len; ++i) {
        int key = arr[i];
        auto index = upper_bound(arr, arr + i, key) - arr;
        // Dùng memmove để di chuyển phần tử, nhanh hơn for, độ phức tạp vẫn là O(n)
        memmove(arr + index + 1, arr + index, (i - index) * sizeof(int));
        arr[index] = key;
      }
    }
    ```
