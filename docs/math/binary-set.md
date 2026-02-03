Kiến thức trước: [phép toán bit](./bit.md#位运算)、[số nguyên và dãy bit](./bit.md#整数与位序列)．

Biểu diễn nhị phân của một số có thể xem như một tập hợp (`0` là không thuộc tập, `1` là thuộc tập). Ví dụ tập $\{1,3,4,8\}$ có thể biểu diễn thành $(100011010)_2$. Các phép toán bit tương ứng có thể xem là phép toán trên tập hợp.

| Phép toán   |    Biểu diễn tập     |         Biểu diễn bit          |
| ------ | :-------------: | :-------------------------: |
| Giao   |   $a \cap b$    | $a \operatorname{AND} b$ |
| Hợp    |   $a \cup b$    | $a \operatorname{OR} b$ |
| Phần bù   |    $\bar{a}$    | $\operatorname{NOT} a$（vũ trụ có tất cả bit là 1） |
| Hiệu   | $a \setminus b$ | $a \operatorname{AND} \operatorname{NOT} b$ |
| Hiệu đối xứng | $a\triangle b$  | $a \operatorname{XOR} b$  |

Trước khi giới thiệu duyệt các tập con, hãy xem một vài ứng dụng của phép toán bit.

### Modulo theo lũy thừa của 2

Một số lấy modulo với $2$ lũy thừa nguyên không âm tương đương với việc lấy vài bit thấp nhất của số đó trong nhị phân, tương đương với phép AND với $mod-1$.

=== "C++"
    ```cpp
    int modPowerOfTwo(int x, int mod) { return x & (mod - 1); }
    ```

=== "Python"
    ```python
    def modPowerOfTwo(x, mod):
        return x & (mod - 1)
    ```

Suy ra, $2$ lũy thừa nguyên không âm lấy modulo với chính nó bằng $0$, tức nếu $n$ là $2$ lũy thừa nguyên không âm thì $n$ AND $(n-1)$ bằng $0$.

Thật vậy, với số nguyên dương $n$, $n-1$ sẽ xóa bit `1` thấp nhất của $n$ và đặt tất cả các bit phía sau thành `1`. Do đó, $n$ AND $(n-1)$ tương đương với việc xóa bit `1` thấp nhất của $n$.

Nhờ đó có thể kiểm tra một số có phải là lũy thừa của $2$ không. Khi và chỉ khi biểu diễn nhị phân của $n$ chỉ có một bit `1`, thì $n$ là lũy thừa của $2$.

=== "C++"
    ```cpp
    bool isPowerOfTwo(int n) { return n > 0 && (n & (n - 1)) == 0; }
    ```

=== "Python"
    ```python
    def isPowerOfTwo(n):
        return n > 0 and (n & (n - 1)) == 0
    ```

### Duyệt tập con

Duyệt toàn bộ tập con của một tập biểu diễn bằng nhị phân tương đương với liệt kê mọi submask của mask nhị phân đó.

Mask là một chuỗi bit dùng để AND với nguồn, tạo ra toán hạng mới sau khi che đi một số bit.

Mask có tác dụng che: bit `1` trong mask giữ nguyên bit tương ứng ở nguồn, bit `0` trong mask sẽ đặt bit tương ứng thành `0`. Đổi một số bit `1` thành `0` sẽ tạo ra submask; mask cũng là submask của chính nó.

Cho một mask $m$, muốn lặp hiệu quả qua mọi submask $s$ của $m$ có thể dùng mẹo bit sau.

```cpp
// Duyệt giảm dần các tập con khác rỗng của m
int s = m;
while (s > 0) {
  // s là một tập con khác rỗng của m
  s = (s - 1) & m;
}
```

Hoặc dùng for gọn hơn:

```cpp
// Duyệt giảm dần các tập con khác rỗng của m
for (int s = m; s; s = (s - 1) & m)
// s là một tập con khác rỗng của m
```

Hai đoạn code này không xử lý submask bằng $0$. Muốn xử lý $0$ có thể dùng:

```cpp
// Duyệt giảm dần các tập con của m
for (int s = m;; s = (s - 1) & m) {
  // s là một tập con của m
  if (s == 0) break;
}
```

Tiếp theo chứng minh đoạn code trên duyệt đủ mọi submask của $m$, không lặp và theo thứ tự giảm dần.

Giả sử đang ở submask $s$ và muốn chuyển sang submask tiếp theo. Trừ $1$ khỏi $s$ tương đương với xóa bit `1` phải nhất và đặt tất cả bit phía phải thành `1`.

Để $s-1$ trở thành submask, cần xóa các bit `1` không có trong $m$, có thể dùng phép `(s - 1) & m`.

Hai bước này tương đương cắt bớt $s-1$ để tạo ra giá trị lớn nhất có thể tiếp theo, tức submask kế tiếp theo thứ tự giảm dần.

Do đó, thuật toán sinh mọi submask theo thứ tự giảm dần, mỗi vòng lặp chỉ có hai thao tác.

Trường hợp đặc biệt là $s=0$. Khi đó $s-1=-1$, tất cả bit là `1`. Sau khi AND với $m$ sẽ quay về $m$. Vì vậy nếu không dừng ở $s=0$ thì vòng lặp không kết thúc.

Gọi $\text{popcount}(m)$ là số bit `1` của $m$, cách này duyệt mọi submask của $m$ trong thời gian $O(2^{\text{popcount}(m)})$.

### Duyệt submask của mọi mask

Trong các bài toán quy hoạch động trạng thái, đôi khi cần với mỗi mask, duyệt mọi submask của nó:

```cpp
for (int m = 0; m < (1 << n); ++m)
  // Duyệt giảm dần các tập con khác rỗng của m
  for (int s = m; s; s = (s - 1) & m)
// s là một tập con khác rỗng của m
```

Cách này duyệt mọi “tập con của tập con” trong tập kích thước $n$.

Tiếp theo chứng minh độ phức tạp là $O(3^n)$, với $n$ là số bit của mask, tức số phần tử của tập.

Xét bit thứ $i$ (phần tử thứ $i$), có ba trường hợp:

- Trong $m$ là `0`, nên trong $s$ cũng là `0`, tức phần tử không ở cả hai tập.
- Trong $m$ là `1` nhưng trong $s$ là `0`, tức chỉ ở tập lớn, không ở tập nhỏ.
- Trong cả $m$ và $s$ đều là `1`, tức phần tử ở cả hai tập.

Có $n$ bit nên có $3^n$ tổ hợp khác nhau.

Một cách chứng minh khác:

Nếu mask $m$ có $k$ bit `1` thì có $2^k$ submask. Với mỗi $k$ có $\dbinom{n}{k}$ mask $m$, tổng số là:

$$
\sum_{k=0}^n \dbinom{n}{k} 2^k
$$

Tổng trên bằng khai triển nhị thức của $(1+2)^n$, do đó có $3^n$ tổ hợp.

### Tài liệu tham khảo

**Trang này chủ yếu dịch từ bài [Перебор всех подмасок данной маски](http://e-maxx.ru/algo/all_submasks) và bản dịch tiếng Anh [Submask Enumeration](https://cp-algorithms.com/algebra/all-submasks.html). Bản tiếng Nga có giấy phép Public Domain + Leave a Link; bản tiếng Anh có giấy phép CC-BY-SA 4.0.**

### Bài tập

- [Atcoder - Close Group](https://atcoder.jp/contests/abc187/tasks/abc187_f)
- [Codeforces - Nuclear Fusion](http://codeforces.com/problemset/problem/71/E)
- [Codeforces - Sandy and Nuts](http://codeforces.com/problemset/problem/599/E)
- [UVa 1439 - Exclusive Access 2](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=4185)
- [UVa 11825 - Hackers' Crackdown](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=2925)
