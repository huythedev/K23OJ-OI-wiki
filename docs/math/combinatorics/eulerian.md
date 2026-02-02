???+ warning "Chú ý"
    Trong phần dưới, “số Euler” chỉ Eulerian number. Đừng nhầm với Euler number hay Euler's number (các hằng số toán học như $\gamma$ hay $\mathrm{e}$).

Trong tổ hợp, **số Euler** (Eulerian Number) là **số lượng** hoán vị của $1$ đến $n$ có đúng $m$ phần tử lớn hơn phần tử đứng trước nó (tức có $m$ “tăng”). Định nghĩa:

$$
A(n, m) = 
\left\langle 
\begin{matrix}
  n\\
  m - 1
\end{matrix}
\right\rangle
$$

Ví dụ, với $1$ đến $3$ có $4$ hoán vị có đúng một phần tử lớn hơn phần tử trước:

| Hoán vị | Cặp liền kề thỏa điều kiện | Số lượng |
| ----- | ----------- | -- |
| 1 2 3 | 1, 2 & 2, 3 | 2  |
| 1 3 2 | 1, 3        | 1  |
| 2 1 3 | 1, 3        | 1  |
| 2 3 1 | 2, 3        | 1  |
| 3 1 2 | 1, 2        | 1  |
| 3 2 1 |             | 0  |

Vì vậy theo định nghĩa $A(n, m)$: nếu $n=3$ và $m=1$, số Euler là $4$, tức có $4$ hoán vị với $1$ phần tử lớn hơn phần tử trước nó.

Với $n,m$ nhỏ, ta có:

| $A(n, m)$ | Hoán vị thỏa | Số lượng |
| --------- | -------------------------------------------- | -- |
| $A(1, 0)$ | $(1)$                                        | 1  |
| $A(2, 0)$ | $(2, 1)$                                     | 1  |
| $A(2, 1)$ | $(1, 2)$                                     | 1  |
| $A(3, 0)$ | $(3, 2, 1)$                                  | 1  |
| $A(3, 1)$ | $(1, 3, 2), (2, 1, 3), (2, 3, 1), (3, 1, 2)$ | 4  |
| $A(3, 2)$ | $(1, 2, 3)$                                  | 1  |

## Công thức

Có thể tính số Euler bằng truy hồi.

Trước hết, khi $m \ge n$ hoặc $n=0$ thì không có hoán vị thỏa, nên giá trị là $0$.

Tiếp theo, khi $m=0$, chỉ có hoán vị giảm dần, nên giá trị là $1$.

Cuối cùng, xét chèn $n$ vào hoán vị của $n-1$ để được hoán vị của $n$. Chèn $n$ làm số “tăng” tối đa tăng 1, nên $A(n,m)$ chỉ chuyển từ $A(n-1,m-1)$ và $A(n-1,m)$.

Xét vị trí chèn $n$: khi $p_{i-1} < p_{i}$, nếu chèn $n$ trước $p_i$, tức chèn vào một “tăng”, số “tăng” không đổi; chèn vào đầu cũng không đổi; còn lại thì tăng 1.

Chuyển từ $A(n-1,m-1)$ sang $A(n,m)$ cần tăng “tăng” lên 1, nên không được chèn vào “tăng” hoặc đầu, có $n-(m-1)-1 = n-m$ cách.

Chuyển từ $A(n-1,m)$ sang $A(n,m)$ cần giữ nguyên, chỉ được chèn vào “tăng” hoặc đầu, có $m+1$ cách.

Vậy:

$$
A(n, m) = \begin{cases}
    0, & m > n \text{ hoặc } n = 0, \\
    1, & m = 0, \\
    (n-m) \cdot A(n-1, m-1) + (m+1) \cdot A(n-1, m), & \text{ngược lại}.
\end{cases}
$$

## Cài đặt

=== "C++"
    ```cpp
    int eulerianNumber(int n, int m) {
      if (m >= n || n == 0) return 0;
      if (m == 0) return 1;
      return (((n - m) * eulerianNumber(n - 1, m - 1)) +
              ((m + 1) * eulerianNumber(n - 1, m)));
    }
    ```

=== "Python"
    ```python
    def eulerianNumber(n, m):
        if m >= n or n == 0:
            return 0
        if m == 0:
            return 1
        return ((n - m) * eulerianNumber(n - 1, m - 1)) + (
            (m + 1) * eulerianNumber(n - 1, m)
        )
    ```

## Bài tập

-   [CF1349F1 Slime and Sequences (Easy Version)](https://codeforces.com/problemset/problem/1349/F1)
-   [CF1349F2 Slime and Sequences (Hard Version)](https://codeforces.com/problemset/problem/1349/F2)
-   [UOJ 593. 新年的军队](https://uoj.ac/problem/593)
-   [P7511 三到六](https://www.luogu.com.cn/problem/P7511)
