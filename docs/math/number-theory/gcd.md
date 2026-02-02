## Định nghĩa

Ước chung lớn nhất là Greatest Common Divisor, thường viết tắt là gcd.

Ước chung của một nhóm số nguyên là số đồng thời là ước của từng số trong nhóm. $\pm 1$ là ước chung của mọi nhóm số nguyên.

Ước chung lớn nhất của một nhóm số nguyên là số lớn nhất trong các ước chung.

Với hai số nguyên $a,b$ không đồng thời bằng $0$, ký hiệu $\gcd(a,b)$; khi không gây mơ hồ có thể viết $(a,b)$.

Với các số nguyên $a_1,\dots,a_n$ không đồng thời bằng $0$, ký hiệu $\gcd(a_1,\dots,a_n)$; khi không gây mơ hồ có thể viết $(a_1,\dots,a_n)$.

Các tính chất của ước chung lớn nhất và bội chung nhỏ nhất xem tại [Cơ sở số học](./basic.md#最大公约数与最小公倍数).

Vậy làm thế nào để tính ước chung lớn nhất? Trước hết xét trường hợp hai số.

### Thuật toán Euclid

#### Quá trình

Nếu biết hai số $a$ và $b$, làm thế nào để tìm $\gcd(a,b)$?

Giả sử $a > b$.

Nếu $b$ là ước của $a$ thì $b$ chính là ước chung lớn nhất của chúng.
Xét trường hợp không chia hết, tức $a = b \times q + r$ với $r < b$.

Ta có thể chứng minh $\gcd(a,b)=\gcd(b,a \bmod b)$ như sau:

???+ note "Chứng minh"
    Đặt $a=bk+c$, hiển nhiên $c=a \bmod b$. Gọi $d \mid a,~d \mid b$, khi đó $c=a-bk, \frac{c}{d}=\frac{a}{d}-\frac{b}{d}k$.
    
    Từ biểu thức bên phải suy ra $\frac{c}{d}$ là số nguyên, tức $d \mid c$. Vậy mọi ước chung của $a,b$ cũng là ước chung của $b,a \bmod b$.
    
    Ngược lại cần chứng minh:
    
    Giả sử $d \mid b,~d\mid (a \bmod b)$, ta vẫn có $\frac{a\bmod b}{d}=\frac{a}{d}-\frac{b}{d}k,~\frac{a\bmod b}{d}+\frac{b}{d}k=\frac{a}{d}$.
    
    Vì vế trái là số nguyên nên $\frac{a}{d}$ là số nguyên, tức $d \mid a$, do đó mọi ước chung của $b,a\bmod b$ cũng là ước chung của $a,b$.
    
    Vì tập ước chung trùng nhau nên ước chung lớn nhất cũng trùng nhau.
    
    Suy ra $\gcd(a,b)=\gcd(b,a\bmod b)$.

Từ $\gcd(a, b) = \gcd(b, r)$, kích thước hai số không tăng, nên ta có một cách đệ quy để tính gcd của hai số.

#### Cài đặt

=== "C++"
    ```cpp
    // Phiên bản 1
    int gcd(int a, int b) {
      if (b == 0) return a;
      return gcd(b, a % b);
    }
    
    // Phiên bản 2
    int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
    ```

=== "Java"
    ```java
    // Phiên bản 1
    public int gcd(int a, int b) {
        if (b == 0) return a;
        return gcd(b, a % b);
    }
    
    // Phiên bản 2
    public int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
    ```

=== "Python"
    ```python
    def gcd(a, b):
        if b == 0:
            return a
        return gcd(b, a % b)
    ```

Đệ quy đến `b == 0` (tức `a % b == 0` ở bước trước) rồi trả kết quả.

Theo cách đệ quy trên, ta cũng có thể viết cách lặp:

=== "C++"
    ```cpp
    int gcd(int a, int b) {
      while (b != 0) {
        int tmp = a;
        a = b;
        b = tmp % b;
      }
      return a;
    }
    ```

=== "Java"
    ```java
    public int gcd(int a, int b) {
        while(b != 0) {
            int tmp = a;
            a = b;
            b = tmp % b;
        }
        return a;
    }
    ```

=== "Python"
    ```python
    def gcd(a, b):
        while b != 0:
            a, b = b, a % b
        return a
    ```

Các thuật toán trên đều được gọi là thuật toán Euclid (Euclidean algorithm).

Ngoài ra, với C++17 ta có thể dùng [`std::gcd`](https://en.cppreference.com/w/cpp/numeric/gcd) và [`std::lcm`](https://en.cppreference.com/w/cpp/numeric/lcm) trong header [`<numeric>`](https://en.cppreference.com/w/cpp/header/numeric) để tính gcd và lcm.

???+ warning "Lưu ý"
    Ở một số trình biên dịch, trong C++14 có thể dùng `std::__gcd(a,b)` để tính gcd, nhưng nó chỉ là hàm phụ trợ nội bộ của `std::rotate`.[^1] Dùng hàm này có thể gây vấn đề ngoài dự kiến, nên thường không khuyến nghị.

Nếu hai số $a$ và $b$ thỏa $\gcd(a, b) = 1$ thì gọi $a$ và $b$ là nguyên tố cùng nhau.

#### Tính chất

Độ hiệu quả thời gian của thuật toán Euclid ra sao? Ta chứng minh rằng với đầu vào là hai số nguyên nhị phân dài $n$, độ phức tạp thời gian là $O(n)$ (tức là $O(\log\max(a,b))$ khi mặc định $a,b$ cùng bậc).

???+ note "Chứng minh"
    Khi tính $\gcd(a,b)$, có hai trường hợp:
    
    -   $a < b$, khi đó $\gcd(a,b)=\gcd(b,a)$;
    -   $a \geq b$, khi đó $\gcd(a,b)=\gcd(b,a \bmod b)$, và phép lấy modulo làm $a$ ít nhất giảm một nửa. Nghĩa là quá trình này xảy ra nhiều nhất $O(\log a) = O(n)$ lần.
    
    Trường hợp thứ nhất xảy ra thì chắc chắn sau đó sẽ xảy ra trường hợp thứ hai, vì vậy số lần xảy ra trường hợp thứ nhất **không nhiều hơn** số lần xảy ra trường hợp thứ hai.
    
    Do đó tối đa đệ quy $O(n)$ lần là có kết quả.

Thực tế, nếu dùng thuật toán Euclid để tính gcd của hai số liên tiếp trong [dãy Fibonacci](../combinatorics/fibonacci.md), thuật toán đạt độ phức tạp tệ nhất.

### Thuật toán trừ dần (更相减损术)

Phép lấy modulo với số lớn có độ phức tạp cao, trong khi phép cộng trừ thì thấp hơn. Với số lớn, ta có thể dùng cộng trừ thay cho chia để tìm gcd.

#### Quá trình

Cho hai số $a$ và $b$, tính $\gcd(a,b)$.

Giả sử $a \ge b$. Nếu $a=b$ thì $\gcd(a,b)=a=b$.
Nếu không, với mọi $d\mid a, d\mid b$, có thể chứng minh $d\mid a-b$.

Vì vậy mọi ước chung của $a,b$ đều là ước chung của $a-b$ và $b$, tức $\gcd(a,b) = \gcd(a-b, b)$.

#### Tối ưu bằng thuật toán Stein

Nếu $a\gg b$ thì thuật toán trừ dần có độ phức tạp tệ nhất $O(n)$.

Xét tối ưu: nếu $2\mid a,2\mid b$ thì $\gcd(a,b) = 2\gcd\left(\dfrac a2, \dfrac b2\right)$.

Nếu $2\mid a$ (tương tự với $2\mid b$), vì đã xét trường hợp $2\mid b$, nên $2 \nmid b$. Do đó $\gcd(a,b)=\gcd\left(\dfrac a2,b\right)$.

Thuật toán tối ưu (Stein) có độ phức tạp $O(\log n)$.

???+ note "Chứng minh"
    Nếu $2\mid a$ hoặc $2\mid b$, mỗi lần đệ quy sẽ giảm ít nhất một trong $a,b$ đi một nửa.
    
    Nếu không, thì $2\mid a-b$, quay về trường hợp trước.
    
    Thuật toán đệ quy tối đa $O(\log n)$ lần.

#### Cài đặt

Mẫu số lớn xem tại [Tính toán số lớn](../bignum.md).

Phép tính số lớn cần: trừ, so sánh, dịch trái, dịch phải (có thể thay bằng nhân/chia nhỏ), đếm số bit 0 ở cuối (có thể kiểm tra chẵn lẻ để tính thô).

??? note "C++"
    ```cpp
    Big gcd(Big a, Big b) {
      if (a == 0) return b;
      if (b == 0) return a;
      // Ghi lại số lần 2 là ước chung của a và b, countr_zero là số bit 0 ở cuối
      int atimes = countr_zero(a);
      int btimes = countr_zero(b);
      int mintimes = min(atimes, btimes);
      a >>= atimes;
      for (;;) {
        // 2 trong các ước chung của a và b đã được xử lý, nên về sau a không thể chẵn
        b >>= btimes;
        // Đảm bảo a<=b
        if (a > b) swap(a, b);
        b -= a;
        if (b == 0) break;
        btimes = countr_zero(b);
      }
      return a << mintimes;
    }
    ```

Đoạn code trên tham khảo [libstdc++](https://github.com/gcc-mirror/gcc/blob/1667962ae755db27965778b8c8c684c6c0c4da21/libstdc%2B%2B-v3/include/std/numeric#L173) và [MSVC](https://github.com/microsoft/STL/blob/9aca22477df4eed3222b4974746ee79129eb44e7/stl/inc/numeric#L591) trong cài đặt `std::gcd` của C++17. Với phạm vi `unsigned int` và `unsigned long long`, nếu tính `countr_zero` cực nhanh thì Stein nhanh hơn Euclid; ngược lại có thể chậm hơn.

???+ note "Về countr_zero"
    1.  gcc có [hàm dựng sẵn](../bit.md#gcc-内建函数) `__builtin_ctz` (32 bit) hoặc `__builtin_ctzll` (64 bit) thay cho `countr_zero` ở trên;
    2.  Từ C++20, header `<bit>` có [`std::countr_zero`](https://en.cppreference.com/w/cpp/numeric/countr_zero);
    3.  Nếu không dùng hàm ngoài chuẩn và không thể dùng C++20, đoạn sau là một cài đặt $O(1)$ sau tiền xử lý trong mô hình Word-RAM with multiplication:
    
    ```cpp
    constexpr int loghash[64] = {0,  32, 48, 56, 60, 62, 63, 31, 47, 55, 59, 61, 30,
                                 15, 39, 51, 57, 28, 46, 23, 43, 53, 58, 29, 14, 7,
                                 35, 49, 24, 44, 54, 27, 45, 22, 11, 37, 50, 25, 12,
                                 38, 19, 41, 52, 26, 13, 6,  3,  33, 16, 40, 20, 42,
                                 21, 10, 5,  34, 17, 8,  36, 18, 9,  4,  2,  1};
    
    int countr_zero(unsigned long long x) {
      return loghash[(x & -x) * 0x9150D32D8EB9EFC0Ui64 >> 58];
    }
    ```
    
    Với số lớn, nếu cài đặt kiểu `bitset`, kết hợp cách `countr_zero` trên có thể đạt $O(n / w)$. Nếu không tiện chia theo bit, chỉ có thể tính thô số mũ lớn nhất của $2$, độ phức tạp tùy theo cài đặt. Ví dụ:
    
    ```cpp
    // Big dạng nhị phân theo thứ tự little-endian, cần duyệt từng phần tử
    int countr_zero(Big a) {
      int ans = 0;
      for (auto x : a) {
        if (x != 0) {
          ans += 32;  // Độ dài bit của kiểu dữ liệu mỗi phần tử
        } else {
          return ans + countr_zero(x);
        }
      }
      return ans;
    }
    
    // Tính thô, nếu dùng thì nên viết trực tiếp vào gcd để giảm hằng số
    int countr_zero(Big a) {
      int ans = 0;
      while ((a & 1) == 0) {
        a >>= 1;
        ++ans;
      }
      return ans;
    }
    ```

Thảo luận thêm về tốc độ của `gcd` xem tại [Fastest way to compute the greatest common divisor](https://lemire.me/blog/2013/12/26/fastest-way-to-compute-the-greatest-common-divisor/).

### GCD của nhiều số

Vậy tính gcd của nhiều số thế nào? Hiển nhiên đáp án phải là ước của mọi số, nên cũng là ước của từng cặp số kề nhau. Dùng quy nạp có thể chứng minh rằng, mỗi lần lấy hai số tính gcd rồi đưa kết quả lại vào dãy sẽ không ảnh hưởng đến đáp án.

## Bội chung nhỏ nhất

Tiếp theo giới thiệu cách tính bội chung nhỏ nhất (Least Common Multiple, LCM).

### Định nghĩa

Bội chung của một nhóm số nguyên là số đồng thời là bội của từng số trong nhóm. 0 là bội chung của mọi nhóm số nguyên.

Bội chung nhỏ nhất của một nhóm số nguyên là số nhỏ nhất trong các bội chung dương.

Với số nguyên $a,b$, ký hiệu $\operatorname{lcm}(a,b)$; khi không gây mơ hồ có thể viết $[a,b]$.

Với các số nguyên $a_1,\dots,a_n$, ký hiệu $\operatorname{lcm}(a_1,\dots,a_n)$; khi không gây mơ hồ có thể viết $[a_1,\dots,a_n]$.

### Hai số

Đặt $a = p_1^{k_{a_1}}p_2^{k_{a_2}} \cdots p_s^{k_{a_s}}$, $b = p_1^{k_{b_1}}p_2^{k_{b_2}} \cdots p_s^{k_{b_s}}$

Khi đó gcd của $a$ và $b$ là

$p_1^{\min(k_{a_1}, k_{b_1})}p_2^{\min(k_{a_2}, k_{b_2})} \cdots p_s^{\min(k_{a_s}, k_{b_s})}$

LCM là

$p_1^{\max(k_{a_1}, k_{b_1})}p_2^{\max(k_{a_2}, k_{b_2})} \cdots p_s^{\max(k_{a_s}, k_{b_s})}$

Do $k_a + k_b = \max(k_a, k_b) + \min(k_a, k_b)$

Suy ra $\gcd(a, b) \times \operatorname{lcm}(a, b) = a \times b$.

Muốn tính lcm của hai số, hãy tính gcd trước.

### Nhiều số

Khi đã có gcd của hai số, lcm của chúng là $O(1)$. Với nhiều số, không cần tính gcd chung trước rồi xử lý, mà có thể thay gcd bằng lcm: mỗi khi tính gcd của hai số, nếu ta đưa kết quả vào dãy để tiếp tục, thì với lcm cũng làm tương tự.

## Thuật toán Euclid mở rộng

Thuật toán Euclid mở rộng (Extended Euclidean algorithm, EXGCD) thường dùng để tìm nghiệm của $ax+by=\gcd(a,b)$.

### Quá trình

Đặt

$ax_1+by_1=\gcd(a,b)$

$bx_2+(a\bmod b)y_2=\gcd(b,a\bmod b)$

Theo định lý Euclid: $\gcd(a,b)=\gcd(b,a\bmod b)$

Suy ra $ax_1+by_1=bx_2+(a\bmod b)y_2$

Mà $a\bmod b=a-(\lfloor\frac{a}{b}\rfloor\times b)$

Suy ra $ax_1+by_1=bx_2+(a-(\lfloor\frac{a}{b}\rfloor\times b))y_2$

$ax_1+by_1=ay_2+bx_2-\lfloor\frac{a}{b}\rfloor\times by_2=ay_2+b(x_2-\lfloor\frac{a}{b}\rfloor y_2)$

Vì $a=a,b=b$, nên $x_1=y_2,y_1=x_2-\lfloor\frac{a}{b}\rfloor y_2$

Thế $x_2,y_2$ liên tục vào đệ quy đến khi $\gcd$ (ước chung lớn nhất, tương tự) bằng $0$, lúc đó $x=1,y=0$ rồi truy hồi.

### Cài đặt

=== "C++"
    ```cpp
    int Exgcd(int a, int b, int &x, int &y) {
      if (!b) {
        x = 1;
        y = 0;
        return a;
      }
      int d = Exgcd(b, a % b, x, y);
      int t = x;
      x = y;
      y = t - (a / b) * y;
      return d;
    }
    ```

=== "Python"
    ```python
    def Exgcd(a, b):
        if b == 0:
            return a, 1, 0
        d, x, y = Exgcd(b, a % b)
        return d, y, x - (a // b) * y
    ```

Hàm trả về $\gcd$, trong quá trình đó ta tính $x,y$.

### Phân tích giá trị

Nghiệm của $ax+by=\gcd(a,b)$ là vô số, và có nghiệm sẽ vượt quá long long.  
May mắn là, nếu $b\not= 0$, nghiệm của exgcd luôn thỏa $|x|\le b,|y|\le a$.  
Dưới đây là chứng minh.

??? note "Chứng minh"
    -   Khi $\gcd(a,b)=b$, $a\bmod b=0$, dừng ở tầng sau.  
        Ta có $x_1=0,y_1=1$, hiển nhiên $a,b\ge 1\ge |x_1|,|y_1|$.
    -   Khi $\gcd(a,b)\not= b$, giả sử $|x_2|\le (a\bmod b),|y_2|\le b$.  
        Vì $x_1=y_2,y_1=x_2-{\left\lfloor\dfrac{a}{b}\right\rfloor}y_2$   
        nên $|x_1|=|y_2|\le b,|y_1|\le|x_2|+|{\left\lfloor\dfrac{a}{b}\right\rfloor}y_2|\le (a\bmod b)+{\left\lfloor\dfrac{a}{b}\right\rfloor}|y_2|$  
        $\le a-{\left\lfloor\dfrac{a}{b}\right\rfloor}b+{\left\lfloor\dfrac{a}{b}\right\rfloor}|y_2|\le a-{\left\lfloor\dfrac{a}{b}\right\rfloor}(b-|y_2|)$   
        $a\bmod b=a-{\left\lfloor\dfrac{a}{b}\right\rfloor}b\le a-{\left\lfloor\dfrac{a}{b}\right\rfloor}(b-|y_2|)\le a$   
        Do đó $|x_1|\le b,|y_1|\le a$.

### Viết exgcd bằng vòng lặp

Đầu tiên, khi $x = 1$，$y = 0$，$x_1 = 0$，$y_1 = 1$ thì:

$$
\begin{cases}
    ax + by     & = a \\
    ax_1 + by_1 & = b
\end{cases}
$$

Đúng.

Biết $a\bmod b = a - (\lfloor \frac{a}{b} \rfloor \times b)$, đặt $q = \lfloor \frac{a}{b} \rfloor$. Mỗi vòng lặp của gcd có thể viết:

$$
(a, b) \rightarrow (b, a - qb)
$$

Thay $a$ bằng $ax+by=a$, $b$ bằng $ax_1+by_1=b$, ta được:

$$
\begin{aligned}
                & \begin{cases}
                      ax + by     & = a \\
                      ax_1 + by_1 & = b
                  \end{cases}                    \\
    \rightarrow & \begin{cases}
                      ax_1 + by_1               & = b      \\
                      a(x - qx_1) + b(y - qy_1) & = a - qb
                  \end{cases}
\end{aligned}
$$

Từ đó có thể suy ra exgcd dạng lặp.

Vì tránh đệ quy nên tốc độ nhanh hơn một chút.

```cpp
int gcd(int a, int b, int& x, int& y) {
  x = 1, y = 0;
  int x1 = 0, y1 = 1, a1 = a, b1 = b;
  while (b1) {
    int q = a1 / b1;
    tie(x, x1) = make_tuple(x1, x - q * x1);
    tie(y, y1) = make_tuple(y1, y - q * y1);
    tie(a1, b1) = make_tuple(b1, a1 - q * b1);
  }
  return a1;
}
```

Nếu quan sát $a_1$ và $b_1$, ta thấy chúng có giá trị hoàn toàn giống trong phiên bản Euclid lặp, và các công thức $x \cdot a +y \cdot b =a_1$ và $x_1 \cdot a +y_1 \cdot b= b_1$ luôn đúng (trước vòng while và sau mỗi vòng). Do đó thuật toán chắc chắn đúng.

Cuối cùng $a_1$ là $\gcd$, ta có $x \cdot a +y \cdot b = g$.

#### Diễn giải bằng ma trận

Với số nguyên dương $a,b$, một bước chia lấy dư $\gcd(a,b)=\gcd(b,a\bmod b)$ viết dưới dạng ma trận:

$$
\begin{bmatrix}
b\\a\bmod b
\end{bmatrix}
=
\begin{bmatrix}
0&1\\1&-\lfloor a/b\rfloor
\end{bmatrix}
\begin{bmatrix}
a\\b
\end{bmatrix}
$$

Trong đó $\lfloor c\rfloor$ là phần nguyên không vượt quá $c$. Định nghĩa biến đổi $\begin{bmatrix}a\\b\end{bmatrix}\mapsto \begin{bmatrix}0&1\\1&-\lfloor a/b\rfloor\end{bmatrix}\begin{bmatrix}a\\b\end{bmatrix}$.

Dễ thấy thuật toán Euclid là lặp biến đổi này:

$$
\begin{bmatrix}
\gcd(a,b)\\0
\end{bmatrix}
=
\left(
\cdots 
\begin{bmatrix}
0&1\\1&-\lfloor a/b\rfloor
\end{bmatrix}
\begin{bmatrix}
1&0\\0&1
\end{bmatrix}
\right)
\begin{bmatrix}
a\\b
\end{bmatrix}
$$

Đặt

$$
\begin{bmatrix}
x_1&x_2\\x_3&x_4
\end{bmatrix}
=
\cdots 
\begin{bmatrix}
0&1\\1&-\lfloor a/b\rfloor
\end{bmatrix}
\begin{bmatrix}
1&0\\0&1
\end{bmatrix}
$$

Khi đó

$$
\begin{bmatrix}
\gcd(a,b)\\0
\end{bmatrix}
=
\begin{bmatrix}
x_1&x_2\\x_3&x_4
\end{bmatrix}
\begin{bmatrix}
a\\b
\end{bmatrix}
$$

Thỏa $a\cdot x_1+b\cdot x_2=\gcd(a,b)$ chính là Euclid mở rộng. Lưu ý nhân với ma trận đơn vị ở cuối không đổi kết quả, gợi ý ta có thể duy trì ma trận đơn vị $2\times 2$ để viết dạng lặp ngắn gọn:

```cpp
int exgcd(int a, int b, int &x, int &y) {
  int x1 = 1, x2 = 0, x3 = 0, x4 = 1;
  while (b != 0) {
    int c = a / b;
    std::tie(x1, x2, x3, x4, a, b) =
        std::make_tuple(x3, x4, x1 - x3 * c, x2 - x4 * c, b, a - b * c);
  }
  x = x1, y = x2;
  return a;
}
```

Cách này đơn giản hơn đệ quy.

## Ứng dụng

-   [10104 - Euclid Problem](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1045)
-   [GYM - (J) once upon a time](http://codeforces.com/gym/100963)
-   [UVa - 12775 - Gift Dilemma](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=4628)

## Tài liệu tham khảo và liên kết

[^1]: [libstdc++: std Namespace Reference](https://gcc.gnu.org/onlinedocs/libstdc++/libstdc++-html-USERS-4.4/a00978.html#a2686a128df5a576cb53a1ed5f674607)
