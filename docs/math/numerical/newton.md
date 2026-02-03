author: Marcythm, iamtwz, nutshellfool, sshwy, allenanswerzq, countercurrent-time, Enter-tainer, H-J-Granger, hly1204, Ir1d, Menci, NachtgeistW, SukkaW, Tiphereth-A, Xeonacid

## Giới thiệu

Bài viết này giới thiệu cách dùng phương pháp Newton (Newton's method for finding roots) để tìm nghiệm gần đúng của phương trình; phương pháp này do Newton đề xuất vào thế kỷ 17.

Nhiệm vụ cụ thể là: với hàm $f(x)$ liên tục và đơn điệu trên $[a,b]$, tìm nghiệm gần đúng của phương trình $f(x)=0$.

## Giải thích

Ban đầu ta có $f(x)$ và một nghiệm gần đúng $x_0$ (vấn đề chọn giá trị đầu liên quan đến fractal Newton, có thể xem [Newton fractal](https://www.bilibili.com/video/BV1HQ4y1q78v) của 3Blue1Brown).

Giả sử nghiệm gần đúng hiện tại là $x_i$, ta vẽ đường thẳng $l$ tiếp xúc với đồ thị $f(x)$ tại điểm $(x_i,f(x_i))$, lấy giao điểm của $l$ với trục $x$ có hoành độ $x_{i+1}$, đó là một nghiệm gần đúng tốt hơn. Lặp lại quá trình này.
Theo ý nghĩa hình học của đạo hàm, ta có:

$$
 f'(x_i) = \frac{f(x_i)}{x_{i} - x_{i+1}}
$$

Sắp xếp lại thu được công thức truy hồi:

$$
 x_{i+1} = x_i - \frac{f(x_i)}{f'(x_i)}
$$

Trực quan mà nói, nếu $f(x)$ đủ trơn thì khi số lần lặp tăng, $x_i$ sẽ tiến dần đến nghiệm.

Phương pháp Newton có tốc độ hội tụ bậc hai, tức mỗi lần lặp số chữ số chính xác tăng gấp đôi.
Chứng minh hội tụ có thể xem [citizendium - Newton method Convergence analysis](http://en.citizendium.org/wiki/Newton%27s_method#Convergence_analysis).

Tất nhiên Newton cũng có nhược điểm, xem thêm [Xiaolin Wu - Roots of Equations trang 18 - 20](https://www.ece.mcmaster.ca/~xwu/part2.pdf).

## Tính căn bậc hai

Ta thử dùng Newton để tính căn bậc hai. Đặt $f(x)=x^2-n$, nghiệm gần đúng chính là xấp xỉ $\sqrt{n}$. Khi đó:

$$
x_{i+1}=x_i-\frac{x_i^2-n}{2x_i}=\frac{x_i+\frac{n}{x_i}}{2}
$$

Khi cài đặt cần chọn độ chính xác phù hợp. Mã như sau:

### Cài đặt

=== "C++"
    ```cpp
    double sqrt_newton(double n) {
      constexpr static double eps = 1E-15;
      double x = 1;
      while (true) {
        double nx = (x + n / x) / 2;
        if (abs(x - nx) < eps) break;
        x = nx;
      }
      return x;
    }
    ```

=== "Python"
    ```python
    def sqrt_newton(n):
        eps = 1e-15
        x = 1
        while True:
            nx = (x + n / x) / 2
            if abs(x - nx) < eps:
                break
            x = nx
        return x
    ```

## Tính căn bậc hai nguyên

Dù có thể dùng `sqrt()` để lấy căn bậc hai, ở đây vẫn trình bày một biến thể Newton để tìm nghiệm nguyên lớn nhất của bất đẳng thức $x^2\le n$. Ta vẫn dùng quá trình tương tự Newton, nhưng chỉnh điều kiện biên: nếu trong quá trình lặp, giá trị gần đúng giảm ở lần trước và lần này lại tăng, thì không thực hiện bước lặp này và thoát vòng.

### Cài đặt

=== "C++"
    ```cpp
    int isqrt_newton(int n) {
      int x = 1;
      bool decreased = false;
      for (;;) {
        int nx = (x + n / x) >> 1;
        if (x == nx || (nx > x && decreased)) break;
        decreased = nx < x;
        x = nx;
      }
      return x;
    }
    ```

=== "Python"
    ```python
    def isqrt_newton(n):
        x = 1
        decreased = False
        while True:
            nx = (x + n // x) // 2
            if x == nx or (nx > x and decreased):
                break
            decreased = nx < x
            x = nx
        return x
    ```

## Căn bậc hai chính xác cao

Cuối cùng xét Newton độ chính xác cao. Phương pháp lặp không đổi, nhưng cần chú ý chọn giá trị gần đúng ban đầu $x_0$. Vì số cần tính thường rất lớn, chọn $x_0$ phù hợp sẽ ảnh hưởng đáng kể đến hiệu năng. Một ý tưởng tự nhiên là lấy $x_0=2^{\left\lfloor\frac{1}{2}\log_2n\right\rfloor}$, vừa tính nhanh, vừa gần với căn bậc hai.

### Cài đặt

Mã Java:

```java
public static BigInteger isqrtNewton(BigInteger n) {
  BigInteger a = BigInteger.ONE.shiftLeft(n.bitLength() / 2);
  boolean p_dec = false;
  for (;;) {
    BigInteger b = n.divide(a).add(a).shiftRight(1);
    if (a.compareTo(b) == 0 || a.compareTo(b) < 0 && p_dec)
      break;
    p_dec = a.compareTo(b) > 0;
    a = b;
  }
  return a;
}
```

Kết quả thực nghiệm: với $n=10^{1000}$ thuật toán chạy 60 ms; nếu không tối ưu $x_0$ mà bắt đầu từ $x_0=1$, thời gian tăng lên 120 ms.

## Bài tập

-   [UVa 10428 - The Roots](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=16&page=show_problem&problem=1369)
-   [LeetCode 69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

    **Trang này chủ yếu dịch từ blog [Метод Ньютона (касательных) для поиска корней](http://e-maxx.ru/algo/roots_newton) và bản tiếng Anh [Newton's method for finding roots](https://cp-algorithms.com/num_methods/roots_newton.html). Bản tiếng Nga theo giấy phép Public Domain + Leave a Link; bản tiếng Anh theo CC-BY-SA 4.0.**
