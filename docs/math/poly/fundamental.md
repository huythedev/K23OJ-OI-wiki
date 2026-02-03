## Định nghĩa

Mọi phương trình đa thức một biến bậc $n$ hệ số phức ($n$ ít nhất là $1$) đều có ít nhất một nghiệm trong $\mathbb{C}$.

Suy ra, phương trình đa thức bậc $n$ hệ số phức có đúng $n$ nghiệm trong $\mathbb{C}$, tính theo bội số.

Đôi khi định lý cũng được phát biểu:

Mọi đa thức một biến bậc $n$ hệ số phức khác không đều có đúng $n$ nghiệm phức.

Chứng minh định lý cơ bản của đại số thường dùng giải tích phức hoặc đại số hiện đại, nên thường được xem như kết quả đã biết và dùng trực tiếp.

Theo định lý cơ bản của đại số, đa thức hệ số phức $f(x)=a_nx^n+a_{n-1}x^{n-1}+\ldots+a_0$ có thể phân tích duy nhất thành:

$$
f(x)=a_n{(x-x_1)}^{k_1}{(x-x_2)}^{k_2}\ldots{(x-x_t)}^{k_t}
$$

trong đó mọi nghiệm đều là số phức, $k_1+k_2+\ldots+k_t=n$.

## Định lý cặp nghiệm ảo

Định lý cơ bản của đại số xét đa thức hệ số phức. Khi nghiên cứu đa thức hệ số thực, dù vẫn phân tích ra nghiệm phức, nhưng phải mở rộng trường số, gây bất tiện.

Nghiệm ảo: nghiệm không thực.

Định lý: với đa thức hệ số thực, nghiệm phức liên hợp cũng là nghiệm.

Chứng minh: lấy liên hợp hai vế của phân tích theo định lý cơ bản của đại số là xong.

Nếu nghiệm là số thực thì liên hợp chính nó.

Nếu nghiệm là nghiệm ảo thì nghiệm liên hợp cũng là nghiệm; do đó hai nghiệm ảo có thể ghép cặp.

Định lý: nghiệm ảo liên hợp của phương trình hệ số thực luôn xuất hiện thành cặp và có cùng bội số.

Chứng minh: giả sử một nghiệm là $a+b\mathrm{i}$ thì nghiệm còn lại là $a-b\mathrm{i}$. Khi đó có hai nhân tử:

$$
(x-a-b\mathrm{i})(x-a+b\mathrm{i})=x^2-2ax+a^2+b^2
$$

Nhân hai nhân tử, các hệ số đều trở thành số thực. Đa thức bậc hai thực này chia hết đa thức gốc.

Do đó trong phân tích theo định lý cơ bản của đại số, chia đồng thời hai vế cho nhân tử bậc hai này, ta vẫn có đẳng thức giữa các đa thức hệ số thực. Lặp lại, bậc giảm dần, cuối cùng không còn nghiệm ảo.

Vì vậy, mỗi cặp nghiệm ảo liên hợp có cùng bội số. Q.E.D.

Suy luận từ định lý cặp nghiệm ảo:

-   Đa thức hệ số thực bậc lẻ có ít nhất một nghiệm thực, và tổng số nghiệm thực là số lẻ.
-   Đa thức hệ số thực bậc chẵn có thể không có nghiệm thực, và tổng số nghiệm thực là số chẵn.

Gọi đa thức bậc hai hệ số thực $x^2-2ax+a^2+b^2=x^2+px+q$ là **nhân tử bất khả quy bậc hai hệ số thực**. “Bất khả quy” là không phân tích được trên $\mathbb{R}$.

Định lý: đa thức hệ số thực luôn phân tích thành tích của các nhân tử bậc nhất hoặc các nhân tử bất khả quy bậc hai hệ số thực.

Chứng minh:

Nếu đa thức có nghiệm thực $c$ thì có nhân tử thực $x-c$. Nếu có cặp nghiệm ảo $a\pm b\mathrm{i}$ thì có nhân tử thực $x^2-2ax+a^2+b^2$.

Do đó chỉ cần ghép cặp nghiệm ảo trong phân tích theo định lý cơ bản của đại số là xong.

Theo định lý cặp nghiệm ảo, đa thức hệ số thực $f(x)=a_nx^n+a_{n-1}x^{n-1}+\ldots+a_0$ phân tích duy nhất thành:

$$
f(x)=a_n{(x-x_1)}^{k_1}{(x-x_2)}^{k_2}\ldots{(x-x_t)}^{k_t}{(x^2+p_1x+q_1)}^{l_1}{(x^2+p_2x+q_2)}^{l_2}\ldots{(x^2+p_sx+q_s)}^{l_s}
$$

trong đó các hệ số đều thực, và $k_1+k_2+\ldots+k_t+2(l_1+l_2+\ldots+l_s)=n$.

## Thuật toán Lin–Shieh

### Giới thiệu

Làm thế nào phân tích đa thức hệ số thực theo định lý cơ bản của đại số? Nếu mở rộng trường số lên phức sẽ phức tạp.

Nếu chỉ làm việc trên $\mathbb{R}$ thì khi bậc lớn hơn $2$, chắc chắn tồn tại một nhân tử bậc hai hệ số thực.

Vì nếu đa thức có nghiệm ảo, chỉ cần ghép một cặp liên hợp; nếu chỉ có nghiệm thực, chọn hai nghiệm thực tương ứng với hai nhân tử bậc nhất rồi nhân lại cũng được một nhân tử bậc hai thực.

Sau khi tìm được nhân tử bậc hai, từ đó giải nghiệm thực hoặc phức là dễ dàng. Như vậy có phương pháp lặp để **tìm một nhân tử bậc hai** nhằm giải nghiệm phức, tránh phép toán phức.

Tháng 8/1940, 8/1943 và 7/1947, Lin–Shieh liên tiếp công bố 3 bài trên tạp chí “Mathematical Physics” của MIT về phương pháp tính nghiệm phức của phương trình bậc cao[^note1], mỗi lần đều có cải tiến.

Ngày nay phương pháp này vẫn dùng trong tính toán nhanh; các gói phần mềm (như MATLAB) tìm nghiệm đa thức cũng dựa trên thuật toán này.

### Quy trình

Để tìm một nhân tử bậc hai, ta phân tích:

$$
f(x)=(x^2+p_1x+q_1)g(x)
$$

Vì không thể tìm ngay nhân tử bậc hai, theo hướng lặp, ta bắt đầu với:

$$
f(x)=(x^2+px+q)g(x)+rx+s
$$

sinh ra một phần dư bậc nhất. Chỉ cần phần dư đủ nhỏ thì gần như tìm được nhân tử.

Ta đặt nghiệm cuối là giá trị ban đầu cộng hiệu chỉnh:

$$
p_1=p+dp
$$

$$
q_1=q+dq
$$

Hai số dư $(r, s)$ phụ thuộc vào $(p, q)$, có quan hệ đạo hàm riêng:

$$
dr=\frac{\partial r}{\partial p}dp+\frac{\partial r}{\partial q}dq
$$

$$
ds=\frac{\partial s}{\partial p}dp+\frac{\partial s}{\partial q}dq
$$

Trong đẳng thức ban đầu, bị chia $f(x)$ là cố định, còn thương $g(x)$ và dư $rx+s$ thay đổi theo $x^2+px+q$. Do đó:

$$
0=xg(x)+\frac{\partial g(x)}{\partial p}(x^2+px+q)+\frac{\partial r}{\partial p}x+\frac{\partial s}{\partial p}
$$

$$
0=g(x)+\frac{\partial g(x)}{\partial q}(x^2+px+q)+\frac{\partial r}{\partial q}x+\frac{\partial s}{\partial q}
$$

Lưu ý đạo hàm riêng là hằng số, không phụ thuộc $x$. Do đó có quan hệ chia hết:

$$
xg(x)=-\frac{\partial g(x)}{\partial p}(x^2+px+q)-\frac{\partial r}{\partial p}x-\frac{\partial s}{\partial p}
$$

$$
g(x)=-\frac{\partial g(x)}{\partial q}(x^2+px+q)-\frac{\partial r}{\partial q}x-\frac{\partial s}{\partial q}
$$

Kết luận: các đạo hàm riêng cần tìm chính là phần dư khi tiếp tục chia thương. Chia đa thức cho nhị thức bậc hai đã cho là tính trực tiếp được, từ đó có bốn đạo hàm riêng.

Ta muốn $s,r$ cộng với $ds,dr$ thành $0$, tức $ds,dr$ là đối của $s,r$. Cần giải hệ:

$$
-\frac{\partial r}{\partial p}dp-\frac{\partial r}{\partial q}dq=r
$$

$$
-\frac{\partial s}{\partial p}dp-\frac{\partial s}{\partial q}dq=s
$$

Giải hệ này (định thức bậc hai) để lấy $dp,dq$, rồi cập nhật $p,q$.

### Cài đặt

```C
// a là đa thức ban đầu, n là bậc đa thức, p là hệ số bậc nhất cần tìm, q là hằng số cần tìm
void Shie(double a[], int n, double *p, double *q) {
  // mảng b là thương của đa thức a chia cho nhị thức bậc hai hiện tại
  memset(b, 0, sizeof(b));
  // mảng c là thương của đa thức b nhân x^2 rồi chia cho nhị thức bậc hai hiện tại
  memset(c, 0, sizeof(c));
  *p = 0;
  *q = 0;
  double dp = 1;
  double dq = 1;
  while (dp > eps || dp < -eps || dq > eps || dq < -eps)  // eps tự đặt
  {
    double p0 = p;
    double q0 = q;
    b[n - 2] = a[n];
    c[n - 2] = b[n - 2];
    b[n - 3] = a[n - 1] - p0 * b[n - 2];
    c[n - 3] = b[n - 3] - p0 * b[n - 2];
    int j;
    for (j = n - 4; j >= 0; j--) {
      b[j] = a[j + 2] - p0 * b[j + 1] - q0 * b[j + 2];
      c[j] = b[j] - p0 * c[j + 1] - q0 * c[j + 2];
    }
    double r = a[1] - p0 * b[0] - q0 * b[1];
    double s = a[0] - q0 * b[0];
    double rp = c[1];
    double sp = b[0] - q0 * c[2];
    double rq = c[0];
    double sq = -q0 * c[1];
    dp = (rp * s - r * sp) / (rp * sq - rq * sp);
    dq = (r * sq - rq * s) / (rp * sq - rq * sp);
    *p += dp;
    *q += dq;
  }
}
```

## Tài liệu tham khảo và chú thích

[^note1]: [Lin–Shieh．Về ứng dụng phương pháp tách nhân tử để giải nghiệm của phương trình đặc trưng bậc cao．Tiến bộ Toán học, 1963(03):207-217.](https://cnki.net/kcms/detail/detail.aspx?filename=SXJZ196303000&dbcode=CJFD&dbname=CJFD1979)
