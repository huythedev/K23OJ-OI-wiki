Kiến thức tiền đề: [Giai thừa modulo](./factorial.md)

## Giới thiệu

Bài viết bàn về cách tính tổ hợp lớn theo modulo．Tổ hợp (còn gọi là hệ số nhị thức) là biểu thức:

$$
\binom{n}{k} = \dfrac{n!}{k!(n-k)!}.
$$

Kích thước nhỏ thì tổ hợp có thể tính bằng [công thức đệ quy](../combinatorics/combination.md#Tính tổ hợp theo công thức đệ quy) với độ phức tạp $O(nk)$; hoặc tính bằng cách tính giai thừa trong $O(n)$ thời gian với một số nguyên tố $p>n$．Nhưng khi kích thước lớn (n~10^18) thì các phương pháp này không còn áp dụng được．

Dựa vào định lý Lucas và các mở rộng, bài viết bàn về một phương pháp tính tổ hợp trong modulo không quá lớn ($m \sim 10^6$)．Cụ thể hơn, chỉ cần tổng các lũy thừa nguyên tố trong phân tích duy nhất $m=\prod p_i^{e_i}$ không quá $10^6$ thì có thể sử dụng phương pháp này, vì thời gian xử lý trước là tương đương với kích thước này．

## Định lý Lucas

Trước hết bàn về trường hợp modulo là số nguyên tố $p$．Khi đó, có định lý Lucas:

???+ note "Định lý Lucas"
    Với số nguyên tố $p$ thì
    
    $$
    \binom{n}{k}\equiv \binom{\lfloor n/p\rfloor}{\lfloor k/p\rfloor}\binom{n\bmod p}{k\bmod p}\pmod p.
    $$
    
    Trong đó, khi $n<k$ thì tổ hợp $\dbinom{n}{k}$ được quy ước là $0$．

??? note "Chứng minh bằng hàm sinh"
    Xét $\displaystyle\binom{p}{n} \bmod p$．Vì
    
    $$
    \binom{p}{n} = \frac{p!}{n!(p-n)!},
    $$
    
    nên khi $n\neq 0,p$ thì mẫu không có thừa số $p$ nhưng tử có thừa số $p$ nên phân số là bội của $p$ và dư là $0$; khi $n=0,p$ thì phân số là $1$．Do đó,
    
    $$
    \binom{p}{n} \equiv [n=0\lor n=p] \pmod p.
    $$
    
    Gọi $f(x) = ax^n + bx^m$．Thông thường, bằng [định lý nhị thức](../combinatorics/combination.md#Định lý nhị thức) và [định lý Fermat](./fermat.md#Định lý Fermat) ta có
    
    $$
    \begin{aligned}
    (f(x))^p 
    &= \left(ax^n + bx^m\right)^p \\
    &= \sum_{k=0}^p\binom{p}{k}(ax^n)^k(bx^m)^{p-k}\\
    &\equiv a^px^{pn} + b^px^{pm} \\
    &\equiv a(x^p)^n+b(x^p)^m\\
    &= f(x^p) \pmod p.
    \end{aligned}
    $$
    
    Trong đó, dòng thứ ba dùng kết quả đã nêu, chỉ có $k=0,p$ thì tổ hợp không phải bội của $p$．
    
    Dùng kết quả này, xét nhị thức:
    
    $$
    \begin{aligned}
    (1+x)^n &= (1+x)^{p\lfloor n/p\rfloor}(1+x)^{n\bmod p} \\
    &\equiv (1+x^p)^{\lfloor n/p\rfloor}(1+x)^{n\bmod p} \pmod p.
    \end{aligned}
    $$
    
    Vế trái, hệ số của $x^k$ là
    
    $$
    \binom{n}{k}\bmod p.
    $$
    
    Tính hệ số của $x^k$ ở vế phải．Đầu tiên, các hạng tử của nhân tử đầu tiên có bậc là bội của $p$; nhân tử thứ hai có bậc nhỏ hơn $p$; và $k$ phân tích thành hai phần như vậy là duy nhất, tức là phép chia có dư: $k=p\lfloor k/p\rfloor +(k\bmod p)$．Do đó, nhân tử đầu tiên chỉ có thể đóng góp hạng tử $p\lfloor k/p\rfloor$; nhân tử thứ hai chỉ có thể đóng góp hạng tử $k\bmod p$．Vậy, hệ số của $x^k$ ở vế phải là tích của hai hệ số:
    
    $$
    \binom{\lfloor n/p\rfloor}{\lfloor k/p\rfloor}\binom{n\bmod p}{k\bmod p}\bmod p.
    $$
    
    Đặt hai vế bằng nhau, ta được định lý Lucas.

??? note "Chứng minh bằng kết quả về giai thừa modulo"
    Đây là một cách chứng minh dựa vào kết quả về [giai thừa modulo](./factorial.md#Số nguyên tố modulo的情形)．Biết tổ hợp
    
    $$
    \binom{n}{k} = \dfrac{n!}{k!(n-k)!}.
    $$
    
    Tách giai thừa $n!$ thành $p$ và các thừa số khác, ta được phân tích:
    
    $$
    n! = p^{\nu_p(n!)}(n!)_p.
    $$
    
    Do đó, tổ hợp có thể viết thành:
    
    $$
    \binom{n}{k} = p^{\nu_p(n!)-\nu_p(k!)-\nu_p((n-k)!)}\dfrac{(n!)_p}{(k!)_p((n-k)!)_p}.
    $$
    
    Các mũ $\nu_p(n!)$ và dư của giai thừa $(n!)_p\bmod p$ đều có công thức đệ quy:
    
    $$
    \begin{aligned}
    \nu_p(n!) &= \lfloor n/p\rfloor+\nu_p( \lfloor n/p\rfloor!),\\
    (n!)_p &\equiv (-1)^{\lfloor n/p\rfloor}\cdot (n\bmod p)!\cdot (\lfloor n/p\rfloor!)_p\pmod p.
    \end{aligned}
    $$
    
    Trong đó, công thức đầu là hệ quả của Legendre, công thức thứ hai là hệ quả của Wilson．
    
    Thay công thức đệ quy vào biểu thức tổ hợp và rút gọn, ta được:
    
    $$
    \begin{aligned}
    \binom{n}{k} &\equiv (-p)^{\lfloor n/p\rfloor-\lfloor k/p\rfloor-\lfloor(n-k)/p\rfloor}\cdot\dfrac{(n\bmod p)!}{(k\bmod p)!((n-k)\bmod p)!} \\
    &\quad \cdot p^{\nu_p(\lfloor n/p\rfloor!)-\nu_p(\lfloor k/p\rfloor!)-\nu_p(\lfloor(n-k)/p\rfloor!)}\dfrac{(\lfloor n/p\rfloor!)_p}{(\lfloor k/p\rfloor!)_p(\lfloor(n-k)/p\rfloor!)_p} \pmod p.
    \end{aligned}
    $$
    
    Bây giờ xét $\lfloor n/p\rfloor-\lfloor k/p\rfloor-\lfloor(n-k)/p\rfloor$．Vì có
    
    $$
    \begin{aligned}
    n &= \lfloor n/p\rfloor p + (n\bmod p),\\
    k &= \lfloor k/p\rfloor p + (k\bmod p),\\
    n-k &= \lfloor (n-k)/p\rfloor p + ((n-k)\bmod p),\\
    \end{aligned}
    $$
    
    nên dùng công thức đầu trừ đi hai công thức sau, ta được
    
    $$
    (\lfloor n/p\rfloor-\lfloor k/p\rfloor-\lfloor(n-k)/p\rfloor)p = (k\bmod p)+((n-k)\bmod p)-(n\bmod p).
    $$
    
    Vế phải, tổng hai số đầu nhỏ hơn $2p$; số thứ ba $n\bmod p$ là dư của tổng hai số đầu, nên phải là số không âm nhưng nhỏ hơn $2p$; và cần là bội của $p$ nên chỉ có thể là $0$ hoặc $p$．Điều này có nghĩa là $\lfloor n/p\rfloor-\lfloor k/p\rfloor-\lfloor(n-k)/p\rfloor$ chỉ có thể là $0$ hoặc $1$:
    
    -   Nếu nó là $0$ thì cũng có $(n\bmod p) = (k\bmod p)+((n-k)\bmod p)$．Do đó, hệ số đầu tiên trong biểu thức là $1$; hệ số thứ hai là $\dbinom{n\bmod p}{k\bmod p}$; hệ số thứ ba là $\dbinom{\lfloor n/p\rfloor}{\lfloor k/p\rfloor}$．Do đó, công thức Lucas thành lập;
    -   Nếu nó là $1$ thì hệ số đầu tiên là $1$; hệ số thứ hai là $0$; nên tổ hợp có dư là $0$．Cũng như vậy, vế phải của định lý Lucas cũng phải là $0$; vì khi đó chắc chắn có $(n\bmod p)<(k\bmod p)$; nếu không thì sẽ có
    
        $$
        ((n-k)\bmod p) = p + (n\bmod p)  - (k\bmod p) \ge p.
        $$
    
        Điều này mâu thuẫn với định nghĩa về số dư．
    
    Tổng hợp hai trường hợp, ta được định lý Lucas．Điều này chứng minh rằng, khi tính tổ hợp theo modulo nguyên tố, dùng định lý Lucas và dùng thuật toán exLucas đều được kết quả tương đương．

Định lý Lucas nói rằng, khi modulo là số nguyên tố $p$ thì tổ hợp lớn có thể chuyển thành tổ hợp nhỏ hơn．Trong biểu thức, tổ hợp đầu tiên có thể tiếp tục đệ quy đến khi $n,k<p$; tổ hợp thứ hai thì có thể tính trực tiếp hoặc đã được xử lý trước．Viết thành mã là:

???+ example "Ví dụ"
    ```cpp
    long long Lucas(long long n, long long k, long long p) {
      if (k == 0) return 1;
      return (C(n % p, k % p, p) * Lucas(n / p, k / p, p)) % p;
    }
    ```

Trong đó, `C(n, k, p)` dùng để tính tổ hợp nhỏ．

Đệ quy tối đa $O(\log_p n)$ lần, nên độ phức tạp là $O(f(p)+g(p)\log_p n)$, trong đó $f(p)$ là độ phức tạp xử lý trước, $g(p)$ là độ phức tạp tính tổ hợp một lần．

### Tham khảo thực hiện

Ở đây đưa ra thực hiện tham khảo trong $O(p)$ thời gian xử lý $p$ trong phạm vi, có thể tính tổ hợp một lần trong $O(1)$:

??? example "Thực hiện tham khảo"
    ```cpp
    --8<-- "docs/math/code/lucas/lucas.cpp"
    ```

Thời gian thực hiện là $O(p+T\log_p n)$, trong đó $T$ là số lần hỏi．

## exLucas thuật toán

Định lý Lucas yêu cầu modulo $p$ phải là số nguyên tố, vậy với $p$ không phải là số nguyên tố thì cần dùng thuật toán exLucas．Dù tên như vậy, thuật toán này thực tế không dùng định lý Lucas．Các bước quan trọng là [tính giai thừa modulo](./factorial.md)．Trong phần chứng minh thứ hai đã nêu mối liên hệ với định lý Lucas．

### Trường hợp modulo là lũy thừa nguyên tố

Trước hết xét trường hợp modulo là lũy thừa nguyên tố $p^\alpha$．Tách giai thừa $n!$ thành $p$ và các thừa số khác, ta được phân tích:

$$
n! = p^{\nu_p(n!)}(n!)_p.
$$

Trong đó, $\nu_p(n!)$ là số mũ của $p$ trong phân tích nguyên tố của $n!$; $(n!)_p$ rõ ràng là nguyên tố cùng nhau với $p$．Do đó, tổ hợp có thể viết thành:

$$
\binom{n}{k} = p^{\nu_p(n!)-\nu_p(k!)-\nu_p((n-k)!)}\dfrac{(n!)_p}{(k!)_p((n-k)!)_p}.
$$

Các mũ $\nu_p(n!)$ có thể tính bằng [định lý Legendre](./factorial.md#legendre-公式); $(n!)_p$ có thể tính bằng [công thức đệ quy](./factorial.md#Số nguyên tố modulo的情形)．Vì $(n!)_p$ nguyên tố cùng nhau với $p^\alpha$ nên nghịch đảo của tích phân tử có thể tính bằng [thuật toán mở rộng Euclid](./inverse.md#mở rộng Euclid)．Vấn đề được giải quyết．

Chú ý, nếu mũ $\nu_p(n!)-\nu_p(k!)-\nu_p((n-k)!)\ge\alpha$ thì dư là $0$; không cần tính thêm．

### Trường hợp tổng quát

Với $m$ là một hợp số tổng quát, chỉ cần phân tích thành các thừa số nguyên tố:

$$
m = p_1^{\alpha_1}p_2^{\alpha_2}\cdots p_s^{\alpha_s}.
$$

Rồi tính tổ hợp $\dbinom{n}{k}$ theo modulo $p_i^{\alpha_i}$, ta được $s$ phương trình đồng dư:

$$
\begin{cases}
\dbinom{n}{k} \equiv r_1, &\pmod{p_1^{\alpha_1}}, \\
\dbinom{n}{k} \equiv r_2, &\pmod{p_2^{\alpha_2}}, \\
\quad\quad\cdots\\
\dbinom{n}{k} \equiv r_s, &\pmod{p_s^{\alpha_s}}.
\end{cases}
$$

Cuối cùng, dùng [định lý Trung Quốc](./crt.md) để tìm số dư theo modulo $m$．

### Tham khảo thực hiện

Cuối cùng, đưa ra thực hiện mẫu đề [Hệ số nhị thức](https://loj.ac/p/181)．

??? example "Thực hiện mẫu đề"
    ```cpp
    --8<-- "docs/math/code/lucas/exlucas.cpp"
    ```

Thuật toán này trong xử lý trước sẽ phân tích $m$ thành các lũy thừa nguyên tố, rồi đối với mọi $p^\alpha$ sẽ xử lý trước tất cả các số không phải bội của $p$ từ $1$ đến $p^\alpha$; và trong việc hợp nhất kết quả sẽ có hệ số tương ứng．Thời gian xử lý trước là $O(\sqrt{m}+\sum_ip_i^{\alpha_i})$; mỗi lần hỏi có độ phức tạp $O(\log m+\sum_i\log_{p_i}n)$; hai thành phần này là độ phức tạp tính nghịch đảo và tính mũ, giai thừa dư．

## Bài tập

-   [Luogu3807【Mẫu】Định lý Lucas](https://www.luogu.com.cn/problem/P3807)
-   [SDOI2010 古代猪文  卢卡斯定理](https://loj.ac/problem/10229)
-   [Luogu4720【Mẫu】Mở rộng Lucas](https://www.luogu.com.cn/problem/P4720)
-   [Ceizenpok’s formula](http://codeforces.com/gym/100633/problem/J)
