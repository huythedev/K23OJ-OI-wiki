author: 383494, buuzzing, c-forrest, cr4c1an, Emp7iness, Enter-tainer, Great-designer, HeRaNO, jifbt, Kaiser-Yang, Koishilll, ksyx, Marcythm, Qiu-Quanzhi, Saisyc, sshwy, StarryReverie, StudyingFather, Tiphereth-A, Xeonacid, xyf007

Trong lập trình thi đấu, một phần quan trọng của số học là **modular arithmetic** (số học modulo), tức là thực hiện các phép toán số nguyên dưới một mô-đun nhất định．Ngoài bốn phép cơ bản và lũy thừa, còn có thể thuận tiện tính log rời rạc, căn bậc, giai thừa và tổ hợp, v.v．

Số học modulo xuất hiện rất thường xuyên trong các bài toán, không chỉ giới hạn ở số học．Nhiều bài có đáp án thực tế rất lớn, vượt quá phạm vi kiểu số nguyên thông thường．Khi đó, để tránh dùng số nguyên lớn và xuất ra số dài, đề bài thường yêu cầu lấy đáp án modulo．Điều này đòi hỏi nắm vững các kỹ thuật số học modulo．

## Phép chia nguyên và lấy dư trong C/C++

Trong C/C++, phép chia nguyên và lấy dư không hoàn toàn giống với định nghĩa toán học quen thuộc．

Với mọi chuẩn C/C++，quy định rằng trong phép chia nguyên：

1.  Khi số chia bằng 0, hành vi không xác định；
2.  Ngược lại, `(a / b) * b + a % b` có kết quả bằng `a`．

Tức dấu của phép lấy dư phụ thuộc vào cách làm tròn của phép chia; còn cách làm tròn là do trình biên dịch quyết định．

Từ [C99](https://en.cppreference.com/w/c/language/operator_arithmetic) và [C++11](https://en.cppreference.com/w/cpp/language/operator_arithmetic) trở đi, quy định **làm tròn về 0** (bỏ phần thập phân); dấu của phần dư cùng dấu với số bị chia．Vì vậy các khẳng định sau luôn đúng：

```c
assert(5 % 3 == 2);
assert(5 % -3 == 2);
assert(-5 % 3 == -2);
assert(-5 % -3 == -2);
```

## Lớp số nguyên modulo

Số học modulo có thể xem như là thực hiện các phép toán trên các lớp đồng dư．Nếu dùng một cấu trúc để biểu diễn một lớp đồng dư, và đóng gói các phép toán giữa các lớp đồng dư thành phương pháp hoặc toán tử, thì số học modulo có thể tự nhiên được thực hiện như một lớp số nguyên modulo．Dưới đây là một ví dụ đơn giản, hỗ trợ phép cộng, trừ, nhân và lũy thừa nhanh cho số nguyên 32 bit với mô-đun $M < 2^{30}$:

???+ example "một lớp số nguyên modulo đơn giản"
    ```cpp
    --8<-- "docs/math/code/mod-arithmetic/mod-arithmetic.cpp:core"
    ```

Cài đặt này cố ý giảm số lần lấy dư, vì lấy dư thường tốn nhiều thời gian hơn so với các phép cộng, trừ, nhân hoặc so sánh．Mã bình luận cung cấp cách thực hiện tương đương và trực tiếp hơn．Các ý tưởng chính là, hai số nguyên trong khoảng $[0,M)$ khi cộng hoặc trừ, kết quả luôn nằm trong khoảng $(-M,2M)$, nên có thể điều chỉnh lại về khoảng $[0,M)$ bằng một phép cộng hoặc trừ．Cài đặt này dùng kỹ thuật [lũy thừa nhanh](../binary-exponentiation.md#mod意义下取幂) để tính lũy thừa.

Ngoài các phép toán cơ bản, còn có thể thực hiện các phép toán sau:

-   [Đảo số](./inverse.md)
-   [Phép chia](./linear-equation.md)
-   [Giai thừa](./factorial.md)
-   [Tổ hợp](./lucas.md)
-   [Căn bậc](./quad-residue.md#mod意义下开平方)
-   [Lấy log rời rạc](./discrete-logarithm.md)
-   [Căn bậc](./residue.md#mod意义下开方)

Các phép toán này thường dễ dàng hơn khi thực hiện trên mô-đun nguyên tố．Với mô-đun hợp, thường cần dùng các phiên bản mở rộng của các thuật toán và [Định lý Trung Quốc](./crt.md)．Các phép toán này thường được xem như là giải một phương trình đồng dư．Phương pháp giải phương trình đồng dư tổng quát có thể tham khảo [Phương trình đồng dư](./congruence-equation.md) trang.

## Các thuật toán liên quan

Phần này sẽ giới thiệu một số phương pháp tối ưu hóa việc lấy dư, nhân và lũy thừa nhanh trong modulo．Với phần lớn các bài toán, cách thực hiện đơn giản đã đủ hiệu quả．Tuy nhiên, khi đề bài yêu cầu độ phức tạp thấp, các phương pháp này có thể phát huy hiệu quả, giảm bớt các phép toán không cần thiết, giảm bớt thời gian thực hiện.

### Phép nhân nhanh

Trong kiểm tra tính nguyên tố và phân tích thừa số, thường xuyên gặp các phép nhân modulo với số lớn trong `long long`．Để tránh vấn đề tràn kiểu nguyên, phần này giới thiệu một phương pháp có thể xử lý số lớn trong `long long` mà không cần dùng `__int128` và độ phức tạp $O(1)$．Phương pháp này yêu cầu hệ thống đánh giá phải hỗ trợ `long double` ít nhất 80 bit[^long-double-80bit]．

Giả sử $0 \le a, b < m$，tính $ab\bmod m$．Chú ý:

$$
ab\bmod m=ab-\left\lfloor \dfrac{ab}m \right\rfloor m.
$$

Dùng `unsigned long long` tự nhiên tràn:

$$
ab\bmod m=ab-\left\lfloor \dfrac{ab}m \right\rfloor m=\left(ab-\left\lfloor \dfrac{ab}m \right\rfloor m\right)\bmod 2^{64}.
$$

Chỉ cần tính được thương $\left\lfloor\dfrac{ab}m\right\rfloor$，các phép nhân và trừ trong biểu thức này có thể dùng `unsigned long long` trực tiếp tính toán．

Tiếp theo, cần xem xét cách tính $\left\lfloor\dfrac {ab}m\right\rfloor$．Giải pháp là dùng `long double` tính $\dfrac am$ rồi nhân với $b$．Vì đã dùng `long double` nên chắc chắn có sai số．Giả sử `long double` biểu diễn là 80 bit (tức là dấu 1 bit, chỉ số 15 bit, phần thập phân 64 bit)，thì `long double` có thể biểu diễn chính xác đến 64 chữ số[^floating-format]．Vì vậy $\dfrac am$ có thể sai từ vị trí thứ 65 trở đi, sai số[^ld-mul-err] là $\left(-2^{-64},2^{-64}\right)$．Nhân với $b$ là một số nguyên 64 bit, sai số là $(-0.5,0.5)$．Để đơn giản hóa việc thảo luận, có thể cộng thêm 0.5 rồi lấy nguyên, sai số cuối cùng là $\{0,1\}$．

Cuối cùng, thay vào biểu thức này cần nhân với $-m$，vì vậy sai số cuối cùng là $\{0,-m\}$．Vì $m$ trong `long long` nên khi kết quả $r\in[0,m)$ thì trả về $r$，ngược lại trả về $r+m$．

Cài đặt như sau:

???+ example "tham khảo thực hiện"
    ```cpp
    --8<-- "docs/math/code/mod-arithmetic/i64-mul.cpp:ld-mul"
    ```

Ngày nay, phần lớn hệ thống đánh giá đã hỗ trợ `__int128`[^int128]，vì vậy cũng có thể trực tiếp nâng cấp kiểu nhân lên `__int128` rồi lấy dư:

???+ example "tham khảo thực hiện"
    ```cpp
    --8<-- "docs/math/code/mod-arithmetic/i64-mul.cpp:i128-mul"
    ```

Dĩ nhiên, phép lấy dư của `__int128` tốn thời gian không ít．Nếu cần giảm thêm, có thể xem xét các phương pháp tiếp theo.

### Barrett Reduction

Trước đây, phép chia và lấy dư thường tốn nhiều thời gian hơn so với các phép toán khác．Để giảm bớt chi phí lấy dư, có một số thuật toán có thể được thực hiện mà không cần trực tiếp lấy dư．Phương pháp này là Barrett Reduction．

Giả sử $m$ là một mô-đun cố định, giả sử cần tính $a\bmod m$ nhiều lần với $a > 0$．Theo định lý chia có dư:

$$
z = a\bmod m = a - \left\lfloor\dfrac{a}{m}\right\rfloor m.
$$

Điểm quan trọng là thương $\left\lfloor\dfrac{a}{m}\right\rfloor$．Giả sử $R$ là một hằng số, thì có[^floor-barrett]

$$
\left\lfloor\dfrac{a}{m}\right\rfloor = \left\lfloor a\dfrac{R}{m} / R\right\rfloor \approx \left\lfloor a\left\lfloor\dfrac{R}{m}\right\rfloor/R\right\rfloor.
$$

Nếu chọn $R = 2^k$，thì $\left\lfloor\dfrac{R}{m}\right\rfloor$ có thể được xử lý trước, chia cho $R$ có thể thực hiện bằng phép dịch bit．Vì vậy, dùng biểu thức này để tính thương chỉ cần một phép nhân và một phép dịch bit．Thay vào biểu thức $a\bmod m$ ta được ước lượng $z'$．

Phân tích sai số của việc làm như vậy．[Hàm làm tròn](./basic.md#làm tròn hàm) có tính chất: với $x > y > 0$ thì $\lfloor x\rfloor - \lfloor y\rfloor \le \lceil x - y\rceil$．Vì vậy sai số

$$
\begin{aligned}
\Delta &= |z' - z| = m\left|\left\lfloor\dfrac{a}{m}\right\rfloor - \left\lfloor a\left\lfloor\dfrac{R}{m}\right\rfloor/R\right\rfloor\right|\\
&\le m\left\lceil a\left(\dfrac{R}{m} - \left\lfloor\dfrac{R}{m}\right\rfloor\right) /R\right\rceil \le m\left\lceil\dfrac{a}{R}\right\rceil.
\end{aligned}
$$

Chỉ cần $a \le R$ thì sai số $\Delta$ không vượt quá $m$．Vì $z' \ge z$ nên ước lượng $z'$ chỉ có thể là $z$ hoặc $z + m$．Chỉ cần khi được ước lượng, nếu $z' \ge m$ thì trừ đi $m$ là được kết quả đúng．

Trong quá trình tính toán Barrett Reduction, chỉ dùng hai phép nhân, một phép dịch bit và tối đa hai phép trừ, đã hoàn thành việc lấy dư．Nhưng hiệu suất tăng không phải là vô giá, thực tế Barrett Reduction cần các biến trung gian dài hơn rất nhiều so với đầu vào．Dễ thấy, biến trung gian dài nhất là $a\left\lfloor\dfrac{R}{m}\right\rfloor$．Giả sử $\ell(x)$ là độ dài biểu diễn nhị phân của số nguyên $x$．Thì có

$$
\ell\left(a\left\lfloor\dfrac{R}{m}\right\rfloor\right) \approx \ell(a) + \ell(R) - \ell(m).
$$

Vì $R$ cần chọn sao cho $a < R$，nên độ dài này ít nhất là $2\ell(a) - \ell(m)$．Nhưng khi cần lấy dư, thường có $\ell(m)\le\ell(a)$，vì vậy độ dài này có thể lớn hơn độ dài đầu vào $\ell(a)$．Ví dụ, nếu cần lấy dư của một số nguyên 64 bit với một số nguyên 32 bit, thực tế cần một số nguyên 64 bit × 2 - 32 = 96 bit．

Barrett Reduction có một ứng dụng là tính dư của tích $ab\bmod m$．Nếu một trong hai thừa số cố định, ví dụ $b$ cố định, thì có thể dùng

$$
ab\bmod m = ab - \left\lfloor a\left\lfloor\dfrac{bR}{m}\right\rfloor/R\right\rfloor m
$$

để ước lượng, chỉ cần xử lý trước $\left\lfloor\dfrac{bR}{m}\right\rfloor$．Đây là trường hợp $b$ cố định, đôi khi gọi là Shoup modular multiplication[^shoup]．

Trường hợp phổ biến hơn là $a, b$ đều không cố định．Khi đó, cần tính $ab$ trước, rồi dùng Barrett Reduction để được $ab\bmod m$．Ví dụ, thực hiện phép nhân modulo, cần tính $ab\bmod m$ với $0 \le a,b < m$．Khi đó, $r$ cần chọn sao cho $ab < R$．Theo phân tích trước, quá trình tính toán có thể có biến trung gian dài nhất là $2\ell(ab)-\ell(m)$．Khi $\ell(a)\approx\ell(b)\approx\ell(m)$, thì độ dài này là $3\ell(m)$．Tức là, nếu dùng Barrett Reduction để thực hiện nhân modulo của các số nguyên 32 bit, cần một số nguyên 96 bit．Đây là một giới hạn thực tế của Barrett Reduction trong các bài toán thi đấu.

Ví dụ, dùng Barrett Reduction để thực hiện nhân modulo của các số nguyên 32 bit:

???+ example "tham khảo thực hiện"
    ```cpp
    --8<-- "docs/math/code/mod-arithmetic/i32-mul.cpp:barrett"
    ```

Cài đặt này cần dùng 128 bit[^int128]．

### Montgomery Modular Multiplication

Montgomery Modular Multiplication có chức năng và Barrett Reduction rất tương tự, nó cũng có thể giảm bớt chi phí lấy dư trong các phép toán modulo．Với hai thuật toán trước đều là gần đúng tính thương, Montgomery Modular Multiplication sẽ ánh xạ tất cả các số nguyên lên không gian Montgomery, và các phép toán trong không gian Montgomery tương đối dễ dàng, từ đó giảm bớt chi phí tính toán.

Giả sử mô-đun $m$ là số lẻ, chọn $R = 2^k > m$．Thì dạng Montgomery của lớp đồng dư $a \bmod m$ là

$$
aR\bmod m.
$$

Vì $R\perp m$，nên lớp đồng dư $a \bmod m$ với dạng Montgomery $aR\bmod m$ tồn tại song song．Vì vậy, có thể chuyển đổi số nguyên thành dạng Montgomery, thực hiện một số phép toán modulo $m$, rồi chuyển đổi lại thành số nguyên, kết quả luôn đúng.

Dùng dạng Montgomery có thể thực hiện nhiều phép toán số nguyên modulo dễ dàng．Vừa rồi đã nói, để kiểm tra hai lớp đồng dư có bằng nhau, chỉ cần so sánh dạng Montgomery của chúng．Vì

$$
(a+b)R\bmod m = ((aR\bmod m)\pm(bR\bmod m)) \bmod{m},
$$

nên phép cộng, trừ tương ứng với phép cộng, trừ dạng Montgomery．Nhưng để tính toán phép nhân của hai lớp đồng dư, không thể trực tiếp nhân hai dạng Montgomery．Vì

$$
(ab)R\bmod m =  ((aR\bmod m)(bR\bmod m)R^{-1}) \bmod{m},
$$

nên khi tính toán hai dạng Montgomery, cần thực hiện **Montgomery Reduction** (Montgomery reduction) như sau:

$$
\operatorname{REDC}: x \mapsto xR^{-1}\bmod m.
$$

Dùng phép này, dạng Montgomery của tích $ab$ là $\operatorname{REDC}((aR\bmod m)(bR\bmod m))$．Montgomery Reduction là cốt lõi của Montgomery Modular Multiplication:

-   Chuyển đổi $a$ thành dạng Montgomery là $\operatorname{REDC}((a\bmod m)(R^2\bmod m))$．
-   Chuyển đổi dạng Montgomery của $a$ trở lại $a\bmod m$ là $\operatorname{REDC}(aR\bmod m)$．
-   Đảo số $a^{-1}\bmod m$ tương ứng với dạng Montgomery là $\operatorname{REDC}((aR\bmod m)^{-1}(R^3\bmod m))$．

Giờ phân tích cách thực hiện Montgomery Reduction $\operatorname{REDC}$．Trong tính toán $\operatorname{REDC}(x)$, luôn giả định $0 \le x < m^2$，điều này đúng với các trường hợp trên．Vì $R\perp m$，nên theo [Định lý Bézout](./bezouts.md)，tồn tại các số nguyên $R^{-1},m'$ sao cho

$$
RR^{-1} + mm' = 1.
$$

Vì vậy, đặt $q=\lfloor xm' / R\rfloor$，thì có

$$
\begin{aligned}
xR^{-1} &= x\dfrac{1 - mm'}{R} \equiv \dfrac{x-xmm' + qmR}{R} = \dfrac{x - m(xm'\bmod R)}{R} \pmod{m}.
\end{aligned}
$$

Vì $0 \le x < m^2 < mR$ và $0 \le xm'\bmod R < R$，nên

$$
-m < \dfrac{x - m(xm'\bmod R)}{R} < m.
$$

Tức là, thương và $xR^{-1}\bmod m$ chỉ khác nhau một $m$．Chỉ cần khi thương nhỏ hơn 0, thì cộng thêm $m$ là được $\operatorname{REDC}(x)$．Tính thương này chỉ cần hai phép nhân, một phép trừ và hai phép dịch bit (lần lượt là lấy dư với $R=2^k$ và làm chia). Vì vậy, Montgomery Reduction có thể thực hiện hiệu quả.

Để thực hiện Montgomery Modular Multiplication, cần xử lý trước một loạt hằng số．Trước tiên, Montgomery Reduction dùng $m' = m^{-1}\bmod R$，có thể tính bằng [phương pháp Newton–Hensel](../poly/newton.md)．Thứ hai, khi quy về Montgomery Reduction, còn liên quan đến các hằng số như $R^2\bmod m$．Để được nó, cần tính một lần $R\bmod m$，rồi cộng với chính nó để được $2R\bmod m$．Sau đó, coi nó là dạng Montgomery, trực tiếp tính lũy thừa nhanh, có thể được $2^kR\bmod m = R^2\bmod m$．

Ví dụ, thực hiện nhân modulo của các số nguyên 32 bit:

???+ example "tham khảo thực hiện"
    ```cpp
    --8<-- "docs/math/code/mod-arithmetic/i32-mul.cpp:montgomery"
    ```

So sánh với Barrett Reduction, Montgomery Modular Multiplication có nhiều bước như chuyển đổi, nhân dạng Montgomery, đảo ngược, v.v．Vì vậy, chỉ khi số lần thực hiện phép toán modulo giữa các bước chuyển đổi đủ nhiều, thì chi phí chuyển đổi và đảo ngược mới được phân tán, đạt hiệu suất cao．Nhưng, do Montgomery Modular Multiplication chỉ cần các biến trung gian dài $2\ell(m)$, nên thực hiện linh hoạt hơn．Ví dụ, nhân modulo của các số nguyên 32 bit chỉ cần một số nguyên 64 bit．Vì vậy, nếu cần thực hiện một lớp số nguyên modulo cho các bài toán số học, Montgomery Modular Multiplication phù hợp hơn.

### Lớp số nguyên modulo 2^k

Phần này thảo luận về việc thực hiện lớp số nguyên modulo $2^k$．Trong trường hợp đặc biệt này, phép chia và lấy dư có thể thực hiện bằng các phép dịch bit, hiệu suất rất cao．Barrett Reduction và Montgomery Modular Multiplication đều tận dụng tính chất của $2^e$ như là số chia và mô-đun để tăng tốc độ tính toán．Đặc biệt, khi mô-đun chính xác là $2^{32}$ và $2^{64}$, có thể dùng các kiểu nguyên không dấu kết hợp tự nhiên tràn để thực hiện lớp số nguyên modulo, không cần bất kỳ phép lấy dư nào．Ngay cả khi mô-đun không chính xác như vậy, cũng có thể chuyển đổi thành các trường hợp đặc biệt này．Ví dụ, mô-đun $2^{58}$, có thể hoàn thành các phép tính trong mô-đun $2^{64}$, rồi lấy dư với $2^{58}$．Ngoài việc lấy dư dễ dàng, các phép toán khác trên lớp số nguyên modulo $2^e$ cũng có nhiều cách thực hiện đặc biệt．Phần này tập trung vào việc thực hiện đảo số và lấy lũy thừa.

Đầu tiên là phép đảo số: cho một số nguyên lẻ $a$ và mô-đun $m=2^e~(e > 2)$, cần tìm $a^{-1}\bmod m$．Các phương pháp phổ biến bao gồm thuật toán mở rộng Euclid và lũy thừa nhanh．Thuật toán mở rộng Euclid có thể thực hiện phép chia với mô-đun thông thường；phương pháp nhanh cần tính $a^{\varphi(m)-1}\bmod{m}$, điều này cần $\Theta(e)$ phép nhân nguyên．Phương pháp hiệu quả hơn là [Newton–Hensel](../poly/newton.md)．Cụ thể, xét ứng dụng kết luận:

$$
mx \equiv 1 \pmod{2^e} \implies mx(2 - mx) \equiv 1\pmod{2^{2e}}.
$$

Theo biểu thức này, chỉ cần từ $x = 1$ bắt đầu, lặp lại $x \gets x(2-mx)$, có thể được $m^{-1}\bmod R$ sau $\lceil\log_2 e\rceil$ lần lặp．

Ví dụ, lấy nghịch đảo của số nguyên modulo $2^{32}$:

???+ example "tham khảo thực hiện"
    ```cpp
    --8<-- "docs/math/code/mod-arithmetic/mod-32-inv-pow.cpp:inv"
    ```

Tiếp theo, thảo luận về phép lấy lũy thừa: cho $x,a,b$ và mô-đun $m=2^e~(e > 2)$, cần tìm $xa^b\bmod m$，với $a$ là số lẻ．Theo phân tích cấu trúc phép nhân modulo $2^e$ trong [Phân tích](./primitive-root.md#mod-pow-2) có thể biết, $a$ luôn có thể viết thành $\pm g^{\ell}$ dạng[^mod-2-g]，và dấu âm chỉ xuất hiện khi $a\equiv 3\pmod 4$．Với trường hợp này, có thể thay $a$ bằng $-a$ và nhân kết quả với $(-1)^b$．Vì vậy, không mất tổng quát, có thể giả sử $a\equiv 1\pmod 4$．Tình huống này, thuật toán chính là viết $a$ thành $g^{L(a)}\bmod m$ dạng, rồi dùng $xg^{bL(a)}\bmod m$ để tính lũy thừa.

Tính $L(a)$ là tính log rời rạc $\operatorname{ind}_ga$．Chú ý, chỉ cần $a\equiv 1\pmod 4$ thì $a$ luôn có thể viết thành dạng:

$$
a \equiv (2^{e_1}+1)(2^{e_2}+1)\cdots(2^{e_s}+1) \pmod{m},
$$

với $1 < e_1 < e_2 < \cdots < e_s < e$．Vì trực tiếp mở rộng tích này có thể thấy, $a$ có bit bằng 1 ở vị trí thứ $e_1$ (từ 0), nên có thể tìm được biểu diễn này theo cách đệ quy．Theo tính chất của log rời rạc, có:

$$
4L(a) \equiv 4L(2^{e_1}+1) + 4L(2^{e_2}+1) + \cdots + 4L(2^{e_s}+1) \pmod{m}. 
$$

Vì số mũ của log rời rạc bằng bậc $\delta_m(g)=2^{e-2}=m/4$，nên nhân cả biểu thức này với 4 để đảm bảo tính toán trong lớp dư modulo $m$．Vì vậy, chỉ cần xử lý trước tất cả $4L(2^d+1)$ với $1 < d < e$，có thể tính nhanh $4L(a)$．

Ngược lại, từ $L(a)$ cũng dễ dàng được $g^a\bmod{m}$．Theo [Định lý nhị thức](../combinatorics/combination.md#Định lý nhị thức), với $1 < d < e$ đều có:

$$
\begin{aligned}
(2^d+1)^{2^{e-d}} \equiv 1 \pmod{m},\quad
(2^d+1)^{2^{e-d-1}} \equiv 1 + 2^{e-1} \pmod{m},
\end{aligned}
$$

nên $\delta_m(2^d+1) = 2^{e-d}$．Theo tính chất của bậc, có:

$$
\delta_m(2^d+1) = \dfrac{\delta_m(g)}{\gcd(\delta_m(g), \operatorname{ind}_g(2^d+1))}.
$$

Vì vậy, $\gcd(\delta_m(g), \operatorname{ind}_g(2^d+1)) = 2^{d-2}$．Điều này có nghĩa là $L(2^d+1) = \operatorname{ind}_g(2^d+1) = 2^{d-2}r$，với $2\nmid r$．Vì vậy, $4L(2^d+1)$ có bit bằng 1 ở vị trí thứ $d$ (từ 0)．Vì vậy, có thể phân tích $4L(a)$ thành tổng của các số dạng $4L(2^d+1)$．Từ đó, có thể tìm được $a$.

Cụ thể thực hiện, có một số điểm có thể tối ưu hóa hơn．Trước tiên, khi phân tích $a$ thành tích, vẫn cần dùng phép chia．Dễ hơn là tính phân tích của $a^{-1}$, tức là tìm $1 < e_1 < e_2 < \cdots < e_s < e$ sao cho

$$
a(2^{e_1}+1)(2^{e_2}+1)\cdots(2^{e_s}+1) \equiv 1 \pmod{m}
$$

thực hiện．Cũng như tìm vị trí thứ $e_1$ bằng cách tìm vị trí thứ $e_1$ trong $a^{-1}$, nhưng cần loại bỏ $2^{e_1}+1$ nhân tử, chỉ cần nhân $a$ với $2^{e_1}+1$ là được, có thể thực hiện bằng phép dịch bit．Vì $4L(a^{-1})=-4L(a)$, nên khi thống kê $4L(a)$ cần dùng phép trừ thay cho phép cộng．Thứ hai, với lựa chọn cơ sở $g$ đặc biệt, không cần lặp đến $d = e-1$ mà chỉ cần đến $d = \lceil e/2\rceil - 1$．Để đạt điều này, cần chọn $g$ sao cho

$$
4L(2^{\lceil e/2\rceil} + 1) = 2^{\lceil e/2\rceil}.
$$

Với $d \ge e / 2$ đều có

$$
(2^d+1)^2 = 2^{2d} + 2^{d+1} + 1 \equiv 2^{d+1} + 1 \pmod{m}.
$$

Vì vậy, từ $d = \lceil e/2\rceil$ bắt đầu quy nạp có thể biết, $L(2^d+1)=2^d$ đối với mọi $d \ge e/2$．Từ đó, chỉ cần $e/2 \le e_1 < e_2 < \cdots < e_s < e$ thì

$$
(2^{e_1}+1)(2^{e_2}+1)\cdots(2^{e_s}+1) \equiv 1 + 2^{e_1} + 2^{e_2} + \cdots + 2^{e_s} \pmod{m}
$$

và

$$
4L((2^{e_1}+1)(2^{e_2}+1)\cdots(2^{e_s}+1)) = 2^{e_1} + 2^{e_2} + \cdots + 2^{e_s}.
$$

Vì vậy, sau khi xử lý hết các bit nhị phân nhỏ hơn $e/2$ thì có thể trực tiếp được phần còn lại của log rời rạc, không cần tính từng bit．Sau khi áp dụng các tối ưu hóa, toàn bộ phép lấy lũy thừa chỉ cần $O(e)$ phép cộng, trừ, dịch bit và một phép nhân．

Ví dụ, lấy lũy thừa của số nguyên modulo $2^{32}$:

???+ example "tham khảo thực hiện"
    ```cpp
    --8<-- "docs/math/code/mod-arithmetic/mod-32-inv-pow.cpp:pow"
    ```

Tính log rời rạc có thể thực hiện bằng thuật toán Pohlig–Hellman, cơ sở $g$ có thể chọn là

$$
5^{\operatorname{ind}_5(2^{\lceil e/2\rceil})/2^{\lceil e/2\rceil - 2}}\bmod{2^e}.
$$

## Tài liệu tham khảo và chú thích

-   [Fast modular multiplication by orz - Codeforces](https://codeforces.com/blog/entry/96759)
-   [Barrett Reduction - Wikipedia](https://en.wikipedia.org/wiki/Barrett_reduction)
-   [Barrett Reduction - A41](https://encrypt.a41.io/primitives/modular-arithmetic/modular-reduction/barrett-reduction#cost-analysis-of-modular-multiplication)
-   [Barrett 约减原理及正确性证明 by Chen - 知乎专栏](https://zhuanlan.zhihu.com/p/690876166)
-   [Montgomery Multiplication - CP Algorithms](https://cp-algorithms.com/algebra/montgomery_multiplication.html)
-   [Montgomery 模乘 by Chen - 知乎专栏](https://zhuanlan.zhihu.com/p/645428404)
-   [Binary Exponentiation by Factoring - CP Algorithms](https://cp-algorithms.com/algebra/factoring-exp.html)
-   Barrett, Paul. "Implementing the Rivest Shamir and Adleman public key encryption algorithm on a standard digital signal processor." In Conference on the Theory and Application of Cryptographic Techniques, pp. 311-323. Berlin, Heidelberg: Springer Berlin Heidelberg, 1986.
-   Becker, Hanno, Vincent Hwang, Matthias J. Kannwischer, Bo-Yin Yang, and Shang-Yi Yang. "Neon NTT: Faster Dilithium, Kyber, and Saber on Cortex-A72 and Apple M1." IACR Transactions on Cryptographic Hardware and Embedded Systems (2022): 221-244.
-   Montgomery, Peter L. "Modular multiplication without trial division." Mathematics of computation 44, no. 170 (1985): 519-521.

[^long-double-80bit]: Phù hợp với hầu hết các hệ thống 64 bit trên GCC hoặc Clang.

[^floating-format]: Tham khảo [Double-precision floating-point format - Wikipedia](https://en.wikipedia.org/wiki/Double-precision_floating-point_format).

[^ld-mul-err]: Dùng điều kiện $a < m$，tức $a / m \in [0,1)$.

[^int128]: Trong môi trường biên dịch hiện đại, chỉ có MSVC trên Windows không hỗ trợ `__int128`．Nếu cần viết mã có thể chạy trên nhiều nền tảng, có thể dùng macro `_MSC_VER` để kiểm tra môi trường MSVC, và trong điều kiện đó bao gồm [`<intrin.h>`](https://learn.microsoft.com/en-us/cpp/intrinsics/x64-amd64-intrinsics-list?view=msvc-170) đầu file, dùng các hàm nội bộ (như `_umul128` v.v.) để gián tiếp thực hiện phép toán 128 bit (chỉ có thể dùng trên nền tảng 64 bit).

[^floor-barrett]: $\left\lfloor\dfrac{r}{m}\right\rfloor$ cũng có thể thay bằng $\dfrac{r}{m}$ của các ước lượng khác, như là hàm làm tròn lên $\left\lceil\dfrac{r}{m}\right\rceil$ và hàm làm tròn gần nhất $\left\lfloor\dfrac{r}{m}\right\rceil$ v.v., chỉ cần điều chỉnh bước sửa lỗi tương ứng.

[^shoup]: Shoup đã thực hiện mở rộng Barrett Reduction trong thư viện số học [NTL](https://libntl.org/) của ông, nên được gọi là Shoup modular multiplication.

[^newton-hensel]: Kiểm tra trực tiếp: từ $mx \equiv 1 \pmod{2^e}$, có thể đặt $mx = 1 + \lambda 2^e$，thì có $mx(2-mx) = (1+\lambda 2^e)(1-\lambda 2^e) = 1 - \lambda^2 2^{2e} \equiv 1\pmod{2^{2e}}$．

[^mod-2-g]: Trang đã chỉ ra rằng $g$ có thể lấy $5$．Thực tế, lặp lại chứng minh, có thể thấy $g$ có thể lấy bất kỳ số nguyên nào có dạng $5$ modulo $8$．Sau đây sẽ thảo luận về cách chọn $g$.
