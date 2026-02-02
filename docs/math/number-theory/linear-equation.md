Bài viết bàn về việc giải phương trình đồng dư tuyến tính．

## Khái niệm cơ bản

Thiết $a,b,n$ là các số nguyên, $x$ là ẩn số, thì phương trình có dạng

$$
ax\equiv b\pmod n
$$

gọi là **phương trình đồng dư tuyến tính** (linear congruence equation)．

Tìm nghiệm của phương trình đồng dư tuyến tính, cần tìm tất cả các nghiệm $x$ trong khoảng $[0,n-1]$．Dĩ nhiên, thêm hoặc bớt bội số của $n$ vào bất kỳ nghiệm nào cũng là nghiệm của phương trình．Trong nghĩa modulo $n$ thì những nghiệm này là tất cả các nghiệm của phương trình．

Bài viết tiếp theo giới thiệu hai cách giải phương trình đồng dư tuyến tính, lần lượt sử dụng nghịch đảo và phương trình đồng dư không xác định．Đối với trường hợp tổng quát, việc tìm nghịch đảo và nghiệm của phương trình đồng dư không xác định đều cần dùng đến [thuật toán mở rộng của Euclid](./gcd.md#thuật-algorithm-mở-rộng-của-euclid)．Do đó, hai cách tiếp cận này thực chất là giống nhau．

## Cách giải bằng nghịch đảo

Trước tiên, xét trường hợp $a$ và $n$ nguyên tố cùng nhau, tức là $\gcd(a,n)=1$．Khi đó, có thể tính nghịch đảo $a^{-1}$ của $a$ và nhân cả hai vế của phương trình với $a^{-1}$, ta được nghiệm duy nhất:

$$
x \equiv ba^{-1} \pmod n.
$$

Tiếp theo, xét trường hợp $a$ và $n$ không nguyên tố cùng nhau, tức là $\gcd(a,n)=d>1$．Khi đó, phương trình không nhất thiết có nghiệm．Ví dụ, $2x\equiv 1\pmod 4$ thì không có nghiệm．Do đó, cần xét hai trường hợp:

-   Khi $d$ không chia hết $b$ thì phương trình vô nghiệm．Vì bất kỳ $x$ nào, vế trái $ax$ đều là bội của $d$ nhưng vế phải $b$ không phải là bội của $d$．Do đó, chúng không thể chênh lệch một bội của $n$ vì $n$ cũng là bội của $d$．Do đó, phương trình vô nghiệm．

-   Khi $d$ chia hết $b$ thì có thể chia cả ba tham số $a,b,n$ cho $d$ để được một phương trình mới:

    $$
    a'x \equiv b'\pmod{n'}.
    $$

    Trong đó, $\gcd(a',n')=1$ tức là $a'$ và $n'$ nguyên tố cùng nhau．Trường hợp này đã được giải quyết ở trên, nên có thể tìm được một nghiệm $x'$.

    Rõ ràng, $x'$ cũng là một nghiệm của phương trình gốc．Nhưng đây không phải là nghiệm duy nhất．Do các nghiệm của phương trình mới là

    $$
    \{x' + kn' : k\in\mathbf Z\}.
    $$

    Những nghiệm này nằm trong khoảng $[0,n-1]$ chính là tất cả các nghiệm của phương trình gốc:

    $$
    x \equiv (x' + kn')\pmod{n},\quad k = 0, 1, \cdots, d-1.
    $$

Tổng hợp hai trường hợp này, số lượng nghiệm của phương trình đồng dư tuyến tính bằng $d=\gcd(a,n)$ hoặc $0$．

## Cách giải bằng phương trình đồng dư không xác định

Phương trình đồng dư tuyến tính tương đương với phương trình đồng dư không xác định hai biến:

$$
ax + ny = b.
$$

Theo như trang đã đề cập, phương trình có nghiệm khi và chỉ khi $\gcd(a,n)\mid b$ và một nghiệm tổng quát là

$$
\begin{aligned}
x &= x_0 + t\dfrac{n}{d},\\
y &= y_0 - t\dfrac{a}{d},
\end{aligned}
$$

trong đó, $d=\gcd(a,n)$ là ước lớn nhất, $t$ là số nguyên tùy ý．

Do đó, nghiệm tổng quát của phương trình đồng dư tuyến tính là

$$
x \equiv \left(x_0+t\frac{n}{d}\right)\pmod{n},\quad t\in\mathbf Z.
$$

Lấy $x_0$ chia cho $n/d$ ta được nghiệm nhỏ nhất (không âm) của phương trình đồng dư, cũng chính là $x'$ ở trên．

## Tham khảo thực hiện

Phần tham khảo thực hiện này có thể tìm được nghiệm nhỏ nhất không âm của phương trình đồng dư．Nếu nghiệm không tồn tại thì xuất ra $-1$．

???+ example "Tham khảo thực hiện"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/linear-equation/linear-equation.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/linear-equation/linear-equation.py:core"
        ```

## Bài tập

-   [「NOIP2012」Đồng dư phương trình](https://loj.ac/problem/2605)

**Trang này chủ yếu dịch từ bài viết [Модульное линейное уравнение первого порядка](http://e-maxx.ru/algo/diofant_1_equation) 与其英文翻译版 [Linear Congruence Equation](https://cp-algorithms.com/algebra/linear_congruence_equation.html)．其中俄文版版权协议为 Public Domain + Leave a Link；英文版版权协议为 CC-BY-SA 4.0．内容有改动．**
