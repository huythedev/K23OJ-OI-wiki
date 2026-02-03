author: H-J-Granger, Chrogeek, countercurrent-time, Enter-tainer, Great-designer, iamtwz, Ir1d, ksyx, mao1t, Menci, NachtgeistW, Nanarikom, ShaoChenHeng, StudyingFather, SukkaW, Tiphereth-A, zyj-111

## Định nghĩa tích phân xác định

Nói đơn giản, tích phân xác định của hàm $f(x)$ trên đoạn $[l,r]$, $\int_{l}^{r}f(x)\mathrm{d}x$, là diện tích hình phẳng tạo bởi $f(x)$ với trục $x$ trên đoạn $[l,r]$ (phần phía trên trục $x$ là dương, phía dưới là âm).

Trong nhiều trường hợp, ta cần tính gần đúng giá trị tích phân một cách hiệu quả và chính xác. Dưới đây giới thiệu **phương pháp Simpson**, một phương pháp tính tích phân số.

## Phương pháp Simpson

Ý tưởng là chia đoạn tích phân thành nhiều đoạn nhỏ, mỗi đoạn dùng công thức tích phân của hàm bậc hai.

??? note "Công thức tích phân hàm bậc hai (công thức Simpson)"
    Với hàm bậc hai $f(x)=ax^2+bx+c$:
    
    $$
    \int_l^r f(x) {\mathrm d}x = \frac{(r-l)(f(l)+f(r)+4 f(\frac{l+r}{2}))}{6}
    $$
    
    Chứng minh:
    Với $f(x)=ax^2+bx+c$;
    Tính nguyên hàm $F(x)=\int_0^x f(x) {\mathrm d}x = \frac{a}{3}x^3+\frac{b}{2}x^2+cx+D$, trong đó $D$ là hằng số,
    
    $$
    \begin{aligned}
    \int_l^r f(x) {\mathrm d}x &= F(r)-F(l) \\
    &= \frac{a}{3}(r^3-l^3)+\frac{b}{2}(r^2-l^2)+c(r-l) \\
    &=(r-l)(\frac{a}{3}(l^2+r^2+lr)+\frac{b}{2}(l+r)+c) \\
    &=\frac{r-l}{6}(2al^2+2ar^2+2alr+3bl+3br+6c)\\
    &=\frac{r-l}{6}((al^2+bl+c)+(ar^2+br+c)+4(a(\frac{l+r}{2})^2+b(\frac{l+r}{2})+c)) \\
    &=\frac{r-l}{6}(f(l)+f(r)+4f(\frac{l+r}{2}))
    \end{aligned}
    $$

Dựa trên công thức Simpson, trước hết giới thiệu một dạng Simpson thường.

### Simpson thường

Năm 1743, phương pháp này được công bố trong một bài báo của Thomas Simpson.

#### Mô tả

Cho số tự nhiên $n$, chia đoạn $[l, r]$ thành $2n$ đoạn đều nhau.

$x_i = l + i h, ~~ i = 0 \ldots 2n,$ $h = \frac {r-l} {2n}.$

Ta tính tích phân trên mỗi đoạn $[x_ {2i-2}, x_ {2i}]$, $i = 1\ldots n$, rồi cộng lại.

Với đoạn $[x_ {2i-2}, x_ {2i}]$, chọn ba điểm $(x_ {2i-2}, x_ {2i-1}, x_ {2i})$ ta có một parabol xác định duy nhất, tương ứng hàm $P(x)$. Khi đó tích phân của $f$ xấp xỉ bằng tích phân của $P$ trên đoạn này, dùng công thức Simpson:

$\int_{x_ {2i-2}} ^ {x_ {2i}} f (x) ~dx \approx \int_{x_ {2i-2}} ^ {x_ {2i}} P (x) ~dx = \left(f(x_{2i-2}) + 4f(x_{2i-1})+f(x_{2i})\right)\frac {h} {3}$

Cộng theo từng đoạn được:

$\int_l ^ r f (x) dx \approx \left(f (x_0) + 4 f (x_1) + 2 f (x_2) + 4f(x_3) + 2 f(x_4) + \ldots + 4 f(x_{2N-1}) + f(x_{2N}) \right)\frac {h} {3}$

#### Sai số

Kết quả sai số của Simpson thường:

$$
-\tfrac{1}{90} \left(\tfrac{r-l}{2}\right)^5 f^{(4)}(\xi)
$$

với $\xi$ nằm trong $[l,r]$.

#### Cài đặt

=== "C++"
    ```cpp
    constexpr int N = 1000 * 1000;
    
    double simpson_integration(double a, double b) {
      double h = (b - a) / N;
      double s = f(a) + f(b);
      for (int i = 1; i <= N - 1; ++i) {
        double x = a + h * i;
        s += f(x) * ((i & 1) ? 4 : 2);
      }
      s *= h / 3;
      return s;
    }
    ```

=== "Python"
    ```python
    N = 1000 * 1000
    
    
    def simpson_integration(a, b):
        h = (b - a) / N
        s = f(a) + f(b)
        for i in range(1, N):
            x = a + h * i
            if i & 1:
                s = s + f(x) * 4
            else:
                s = s + f(x) * 2
        s = s * (h / 3)
        return s
    ```

### Simpson thích nghi

Phương pháp thường bị giới hạn bởi $n$ nếu muốn đảm bảo độ chính xác; ta cần phương pháp cân bằng giữa độ chính xác và thời gian.

Ý tưởng: nếu một đoạn đồ thị đã rất gần hàm bậc hai, áp dụng công thức trực tiếp là đủ chính xác, không cần chia tiếp.

Vậy ta chia đoạn theo cách: mỗi lần kiểm tra độ “gần” với bậc hai; nếu đủ gần thì tính trực tiếp, nếu không thì chia đôi và đệ quy.

Cách kiểm tra: tính Simpson trên đoạn hiện tại, rồi chia đôi đoạn và tính Simpson trên hai nửa. Nếu tổng hai nửa gần với kết quả đoạn hiện tại, coi như đủ gần.

Trong thực tế, ngoài kiểm tra sai số còn cần ép số lần lặp tối thiểu.

Mã tham khảo:

=== "C++"
    ```cpp
    double simpson(double l, double r) {
      double mid = (l + r) / 2;
      return (r - l) * (f(l) + 4 * f(mid) + f(r)) / 6;  // Công thức Simpson
    }
    
    double asr(double l, double r, double eps, double ans, int step) {
      double mid = (l + r) / 2;
      double fl = simpson(l, mid), fr = simpson(mid, r);
      if (abs(fl + fr - ans) <= 15 * eps && step < 0)
        return fl + fr + (fl + fr - ans) / 15;  // Nếu đủ gần thì trả về trực tiếp
      return asr(l, mid, eps / 2, fl, step - 1) +
             asr(mid, r, eps / 2, fr, step - 1);  // Nếu không thì chia đôi và đệ quy
    }
    
    double calc(double l, double r, double eps) {
      return asr(l, r, eps, simpson(l, r), 12);
    }
    ```

=== "Python"
    ```python
    def simpson(l, r):
        mid = (l + r) / 2
        return (r - l) * (f(l) + 4 * f(mid) + f(r)) / 6  # Công thức Simpson
    
    
    def asr(l, r, eps, ans, step):
        mid = (l + r) / 2
        fl = simpson(l, mid)
        fr = simpson(mid, r)
        if abs(fl + fr - ans) <= 15 * eps and step < 0:
            return fl + fr + (fl + fr - ans) / 15  # Nếu đủ gần thì trả về trực tiếp
        return asr(l, mid, eps / 2, fl, step - 1) + asr(
            mid, r, eps / 2, fr, step - 1
        )  # Nếu không thì chia đôi và đệ quy
    
    
    def calc(l, r, eps):
        return asr(l, r, eps, simpson(l, r), 12)
    ```

## Bài tập

-   [Luogu4525【模板】自适应辛普森法 1](https://www.luogu.com.cn/problem/P4525)
-   [HDU1724 Ellipse](https://acm.hdu.edu.cn/showproblem.php?pid=1724)
-   [NOI2005 月下柠檬树](https://www.luogu.com.cn/problem/P4207)

## Tài liệu tham khảo

<https://doi.org/10.1145/321526.321537>: bài báo thảo luận cải tiến phương pháp Simpson thích nghi, giải thích nguồn gốc và ưu điểm của hằng số `15` trong mã trên.
