author: Early0v0

## Kiến thức tiền đề

-   [Hàm nhân tính](./basic.md#积性函数)

## Định nghĩa

Sàng Zhou Ge (洲阁筛) là một phương pháp có thể tính tổng tiền tố của phần lớn hàm nhân tính trong thời gian dưới tuyến tính.

Dưới đây lấy ví dụ tính $\displaystyle\sum_{i=1}^nf(i)$ để trình bày cụ thể nguyên lý của sàng Zhou Ge.

## Quy ước

-   $\mathbb P$ là tập số nguyên tố, $p_i$ là số nguyên tố thứ $i$.
-   $m$ là số lượng số nguyên tố không vượt $\sqrt n$.

## Yêu cầu

Với $p\in\mathbb P,c\in\mathbb N$, $f(p^c)$ là một đa thức bậc thấp theo $p$.

## Ý tưởng

-   Với mọi số nguyên trong $[1,n]$, nó có nhiều nhất một ước nguyên tố $>\sqrt n$.
-   Dùng tính chất $\left\lfloor\dfrac ni\right\rfloor(i\in[1,n]\cap\mathbb N)$ chỉ có số lượng giá trị cỡ $\sqrt n$ để giảm độ phức tạp.

## Quy trình

Chia các số nguyên trong $[1,n]$ thành hai loại theo việc có ước nguyên tố $>\sqrt n$ hay không:

$$
\sum_{i=1}^nf(i)=\sum_{i=1}^n\left[\exists d\in(\sqrt n,n]\cap\mathbb P,d\mid i\right]f(i)+\sum_{i=1}^n\left[\forall d\in(\sqrt n,n]\cap\mathbb P,d\nmid i\right]f(i)
$$

Với phần đầu, duyệt ước lớn nhất, theo tính chất nhân tính có thể chuyển:

$$
\sum_{i=1}^nf(i)=\sum_{i=1}^{\sqrt n}f(i)\cdot\left(\sum_{d=\lfloor\sqrt n\rfloor+1}^{\lfloor\frac ni\rfloor}[d\in\mathbb P]f(d)\right)+\sum_{i=1}^n\left[\forall d\in(\sqrt n,n]\cap\mathbb P,d\nmid i\right]f(i)
$$

Hai phần có thể tính riêng.

### Part 1

> Tính $\displaystyle\sum_{i=1}^{\sqrt n}f(i)\cdot\left(\sum_{d=\lfloor\sqrt n\rfloor+1}^{\lfloor\frac ni\rfloor}[d\in\mathbb P]f(d)\right)$.

Xét duyệt $i$, rồi tính phần trong ngoặc bằng $O(1)$.

Đặt $\displaystyle g(t,l)=\sum_{i=1}^l[\forall j\in[1,t],\gcd(i,p_j)=1]f(i)$, tức tổng $f(i)$ của các số trong $[1,l]$ đồng thời nguyên tố cùng nhau với $p_1,p_2,\dots,p_t$.

Khi đó Part 1 trở thành $\displaystyle\sum_{i=1}^{\sqrt n}f(i)\cdot g\left(m,\left\lfloor\frac ni\right\rfloor\right)$.

Biên $g(0,l)=\sum_{i=1}^lf(i)$, chuyển $g(t,l)=g(t-1,l)-f(p_t)\cdot g\left(t-1,\left\lfloor\frac l{p_t}\right\rfloor\right)$.

$l$ có cỡ $\sqrt n$ giá trị, với mỗi giá trị cần duyệt các ước nguyên tố, nên độ phức tạp là $\displaystyle O\left(\frac{\sqrt n}{\ln\sqrt n}\cdot\sqrt n\right)= O\left(\frac n{\log n}\right)$, cần tối ưu.

Nhận thấy khi $p_{t+1}^2>l$ thì các số thỏa chỉ còn $1$, nên $g(t,l)=f(1)=1$.

Thế vào truy hồi được: khi $p_t^2>l$ thì $g(t,l)=g(t-1,l)-f(p_t)$.

Vì vậy một khi phát hiện $p_t^2>l$ thì dừng chuyển, gọi $t$ khi đó là $t_l$, khi đó $\forall t>t_l,g(t,l)=g(t_l,l)-\sum_{i=t_l}^{t-1}f(p_i)$.

Tiền xử lý tổng tiền tố của $f(p_i)$ sẽ giúp tính nhanh $g$, độ phức tạp tối ưu tới $O\left(\dfrac{n^{\frac34}}{\log n}\right)$.

### Part 2

> Tính $\displaystyle\sum_{i=1}^n\left[\forall d\in(\sqrt n,n]\cap\mathbb P,d\nmid i\right]f(i)$.

Đặt $\displaystyle h(t,l)=\sum_{i=1}^l\left[i=\prod_{j=t}^mp_j^{c_j},c_j\in\mathbb N\right]f(i)$, tức tổng $f(i)$ của các số trong $[1,l]$ chỉ chứa các ước nguyên tố $p_t,p_{t+1},\dots,p_m$.

Part 2 chính là $h(0,n)$.

Biên $h(m+1,l)=1$, chuyển $\displaystyle h(t,l)=h(t+1,l)+\sum_{c\in\mathbb N^*}f(p_t^c)\cdot h\left(t+1,\left\lfloor\frac l{p_t^c}\right\rfloor\right)$.

$l$ có cỡ $\sqrt n$ giá trị, nên chuyển trực tiếp có độ phức tạp $\displaystyle O\left(\sqrt n\cdot\frac{\sqrt n}{\ln\sqrt n}\right)= O\left(\frac n{\log n}\right)$, cần tối ưu.

Tương tự cách tối ưu với $g$, nhận thấy khi $p_t>l$ thì các số chỉ dùng $p_t,p_{t+1},\dots,p_m$ chỉ có $1$, khi đó $h(t,l)=f(1)=1$.

Tương tự, suy ra $\forall p_t^2>l,h(t,l)=h(t+1,l)+f(p_t)$.

Do đó một khi $p_t^2>l$ thì dừng chuyển, gọi $t$ khi đó là $t_l$, sau đó khi cần dùng $h$ thì cộng thêm $\displaystyle\sum_{i=p_{t_l}}^{\min(l,\sqrt n)}[i\in\mathbb P]f(i)$.

Độ phức tạp được tối ưu đến $O\left(\dfrac{n^{\frac34}}{\log n}\right)$.

### Tổng hợp

Tính xong Part 1 và Part 2, cộng lại là $\displaystyle\sum_{i=1}^nf(i)$.

## Tham khảo

[积性函数线性筛/杜教筛/洲阁筛学习笔记 | Bill Yang's Blog](https://blog.bill.moe/multiplicative-function-sieves-notes)
