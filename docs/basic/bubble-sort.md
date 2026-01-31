Trang này sẽ giới thiệu ngắn gọn về thuật toán **Sắp xếp nổi bọt** (Bubble Sort).

## Định nghĩa

**Sắp xếp nổi bọt** (tiếng Anh: Bubble sort) là một thuật toán sắp xếp đơn giản. Trong quá trình thực hiện, các phần tử nhỏ hơn sẽ "nổi" dần lên đầu dãy số như các bong bóng, do đó có tên gọi là sắp xếp nổi bọt.

## Quy trình

Nguyên lý hoạt động là mỗi lần kiểm tra hai phần tử kề nhau, nếu phần tử phía trước và phía sau không đúng thứ tự mong muốn, sẽ hoán đổi chúng. Khi không còn cặp phần tử nào cần hoán đổi, quá trình sắp xếp kết thúc.

Sau $i$ lượt quét, $i$ phần tử cuối cùng của dãy chắc chắn là $i$ phần tử lớn nhất, do đó tối đa cần $n-1$ lượt quét để hoàn thành sắp xếp.

## Tính chất

### Tính ổn định

Sắp xếp nổi bọt là một thuật toán sắp xếp **ổn định**.

### Độ phức tạp thời gian

Khi dãy đã có thứ tự, sắp xếp nổi bọt chỉ cần duyệt một lần, không cần hoán đổi, độ phức tạp là $O(n)$.

Trường hợp xấu nhất, thuật toán thực hiện $\frac{(n-1)n}{2}$ lần hoán đổi, độ phức tạp là $O(n^2)$.

Trung bình, độ phức tạp thời gian của sắp xếp nổi bọt là $O(n^2)$.

## Cài đặt mã nguồn

### Giả mã

$$
\begin{array}{ll}
1 & \textbf{Input. } \text{Một mảng } A \text{ gồm }n\text{ phần tử.} \\
2 & \textbf{Output. } A\text{ sẽ được sắp xếp tăng dần, ổn định.} \\
3 & \textbf{Method. }  \\
4 & flag\gets True\\
5 & \textbf{while }flag\\
6 & \qquad flag\gets False\\
7 & \qquad\textbf{for }i\gets1\textbf{ to }n-1\\
8 & \qquad\qquad\textbf{if }A[i]>A[i + 1]\\
9 & \qquad\qquad\qquad flag\gets True\\
10 & \qquad\qquad\qquad \text{Hoán đổi } A[i]\text{ và }A[i + 1]
\end{array}
$$

=== "C++"
    ```cpp
    --8<-- "docs/basic/code/bubble-sort/bubble-sort_1.cpp"
    ```

=== "Python"
    ```python
    --8<-- "docs/basic/code/bubble-sort/bubble-sort_1.py:core"
    ```

=== "Java"
    ```java
    // Giả sử mảng có kích thước n + 1, sắp xếp nổi bọt bắt đầu từ chỉ số 1
    static void bubble_sort(int[] a, int n) {
        boolean flag = true;
        while (flag) {
            flag = false;
            for (int i = 1; i < n; i++) {
                if (a[i] > a[i + 1]) {
                    flag = true;
                    int t = a[i];
                    a[i] = a[i + 1];
                    a[i + 1] = t;
                }
            }
        }
    }
    ```
