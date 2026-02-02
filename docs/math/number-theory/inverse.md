Bài viết giới thiệu nghịch đảo của phép nhân trong modulo và các phương pháp tính thường gặp．

## Khái niệm cơ bản

Số thực $a\in\mathbf R$ khác 0 thì nghịch đảo của nó là số nghịch đảo $a^{-1}$．Tương tự, trong lý thuyết số, ta cũng có thể định nghĩa nghịch đảo của một số nguyên $a$ trong modulo $m$ là $a^{-1}\bmod m$ hoặc đơn giản là $a^{-1}$．Đây là **nghịch đảo modulo** (modular multiplicative inverse), hay còn gọi là **số nghịch đảo**．

???+ abstract "Nghịch đảo"
    Với hai số nguyên $a,m$ khác 0, nếu tồn tại $b$ sao cho $ab\equiv 1\pmod m$ thì $b$ được gọi là **nghịch đảo** (inverse) của $a$ trong modulo $m$．

Điều này tương đương với việc $b$ là nghiệm của phương trình đồng dư tuyến tính $ax\equiv 1\pmod m$．Theo [phương trình đồng dư tuyến tính](./linear-equation.md) thì tồn tại nghiệm khi và chỉ khi $\gcd(a,m)=1$ tức là $a,m$ nguyên tố cùng nhau．

## Cách tính nghịch đảo đơn lẻ

Sử dụng thuật toán mở rộng Euclid hoặc thuật toán nhanh mũ, ta có thể tính nghịch đảo của một số nguyên trong $O(\log m)$ thời gian．

### Thuật toán mở rộng Euclid

Tính nghịch đảo, tương đương với việc giải phương trình đồng dư tuyến tính．Do đó, ta có thể sử dụng [thuật toán mở rộng Euclid](./gcd.md#Thuật toán mở rộng Euclid) trong $O(\log\min\{a,m\})$ thời gian để giải nghịch đảo．Do phương trình đồng dư đặc biệt, ta có thể đơn giản hóa các bước tương ứng．

???+ example "Tham khảo thực hiện"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/inverse/inverse-1.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/inverse/inverse-1.py:core"
        ```

Thuật toán này phù hợp với mọi trường hợp tồn tại nghịch đảo．

### Thuật toán nhanh mũ

Thuật toán này chủ yếu phù hợp với trường hợp số modulo là số nguyên tố $p$．Khi đó, theo [định lý Fermat](./fermat.md#Định lý Fermat) thì với mọi $a\perp p$ ta đều có

$$
a\cdot a^{p-2} = a^{p-1} \equiv 1 \pmod p.
$$

Theo tính duy nhất của nghịch đảo, nghịch đảo $a^{-1}\bmod p$ bằng $a^{p-2}\bmod p$．Do đó, ta có thể sử dụng [thuật toán nhanh mũ](../binary-exponentiation.md) trong $O(\log p)$ thời gian để tính:

???+ example "Tham khảo thực hiện"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/inverse/inverse-2.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/inverse/inverse-2.py:core"
        ```

Tuy nhiên, lý thuyết, thuật toán này có thể mở rộng đến trường hợp tổng quát với số modulo $m$ bằng cách sử dụng $a^{\varphi(m)-1}\bmod m$ để tính nghịch đảo．Nhưng, tính toán [hàm Euler](./euler-totient.md) $\varphi(m)$ một lần rất khó, do đó thuật toán này không hiệu quả trong trường hợp tổng quát．

## Cách tính nghịch đảo nhiều lẻ

Một số trường hợp, cần tính nghịch đảo của nhiều số nguyên $a_1,a_2,\cdots,a_n$ trong modulo $m$．Khi đó, tính nghịch đảo từng cái, tổng cộng cần $O(n\log m)$ thời gian．Thực tế, nếu xử lý chúng đồng thời, ta có thể tính tất cả các nghịch đảo trong $O(n+\log m)$ thời gian．

Xem xét dãy $\{a_i\}$ của các tiền tố:

$$
S_0 = 1,~ S_i = a_iS_{i-1},~ i=1,2,\cdots,n.
$$

Nếu mỗi $a_i$ đều nguyên tố cùng nhau với $m$ thì tích $S_n$ cũng nguyên tố cùng nhau với $m$．Do đó, ta có thể sử dụng thuật toán đã nêu để tính $S_n^{-1}\bmod m$．Vì nghịch đảo của tích là tích của nghịch đảo, nên từ $S_n^{-1}$, đi ngược lại dãy ta có thể tính được nghịch đảo của mỗi $S_i$:

$$
S_{i-1}^{-1} = a_iS_i^{-1} \bmod m,~ i = n,n-1,\cdots,1.
$$

Từ đó, nghịch đảo của mỗi $a_i$ có thể tính bằng:

$$
a_i^{-1} = S_{i-1}S_i^{-1} \bmod m,~ i = 1,2,\cdots,n.
$$

Tham khảo thực hiện:

???+ example "Tham khảo thực hiện"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/inverse/inverse-3.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/inverse/inverse-3.py:core"
        ```

Thuật toán này chỉ tính một lần nghịch đảo của một phần tử, nên tổng thời gian là $O(n+\log m)$．

## Tính nghịch đảo trước

Nếu cần tính nghịch đảo của các số nguyên dương đầu tiên $n$ trong modulo $p$ nguyên tố, ta có thể sử dụng mối quan hệ đệ quy trong $O(n)$ thời gian．Phương pháp này thường được dùng để tính nghịch đảo của các số nguyên dương đầu tiên $n$ trong modulo $p$ nguyên tố．

Đối với $1< i < p$ thì xét phép chia có dư:

$$
p = \left\lfloor \dfrac{p}{i} \right\rfloor i + (p\bmod i).
$$

Lấy phương trình này đối với số nguyên tố $p$ thì ta được

$$
0 \equiv \left\lfloor \dfrac{p}{i} \right\rfloor i + (p\bmod i) \pmod p.
$$

Nhân cả hai vế với $i^{-1}(p\bmod i)^{-1}$ ta được

$$
i^{-1} \equiv - \left\lfloor \dfrac{p}{i} \right\rfloor (p\bmod i)^{-1} \pmod p.
$$

Đây là công thức dùng để tính nghịch đảo tuyến tính．Do $p\bmod i < i$ thì công thức này chuyển việc tìm $i^{-1}\bmod p$ thành việc tìm nghịch đảo của một số nhỏ hơn $(p\bmod i)^{-1}\bmod p$．Do đó, từ $1^{-1}\bmod p=1$ bắt đầu, áp dụng công thức này lần lượt cho mỗi $i$ thì ta có thể tính được nghịch đảo của các số nguyên dương đầu tiên $n$ trong $O(n)$ thời gian．

Tham khảo thực hiện:

???+ example "Tham khảo thực hiện"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/inverse/inverse-4.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/inverse/inverse-4.py:core"
        ```

Thuật toán này chỉ phù hợp với trường hợp modulo là số nguyên tố．Với modulo $m$ không phải là số nguyên tố, không thể đảm bảo rằng $m\bmod i$ vẫn nguyên tố cùng nhau với $m$．Do đó, nghịch đảo $(m\bmod i)^{-1}$ có thể không tồn tại．Ví dụ: $m=8,i=3$．Khi đó, $m\bmod i = 2$ và không tồn tại nghịch đảo modulo $m$．

Ngoài ra, sau khi có được công thức đệ quy, một ý tưởng tự nhiên là trực tiếp đệ quy tìm nghịch đảo của một số $a$．Mỗi lần đệ quy, ta dùng công thức đệ quy để chuyển nó thành nghịch đảo của số dư $p\bmod a$ cho đến khi dư bằng $1$ thì dừng lại．Hiện tại chưa rõ độ phức tạp của việc làm như vậy[^linear-recursion]．Do đó, khuyến nghị sử dụng phương pháp thông thường như đã nêu．

## Bài tập

-   [LOJ 110 乘法逆元](https://loj.ac/problem/110)
-   [LOJ 161 乘法逆元 2](https://loj.ac/problem/161)
-   [LOJ 2605「NOIP2012」同余方程](https://loj.ac/problem/2605)
-   [Luogu P2054「AHOI2005」洗牌](https://www.luogu.com.cn/problem/P2054)
-   [LOJ 2034「SDOI2016」排列计数](https://loj.ac/problem/2034)

## Tài liệu tham khảo và chú thích

-   [Modular multiplicative inverse - Wikipedia](https://en.wikipedia.org/wiki/Modular_multiplicative_inverse)

[^linear-recursion]: [riteme trong Zhihu](https://www.zhihu.com/question/59033693/answer/323292359) chỉ ra rằng, độ phức tạp lý thuyết của việc làm như vậy là $O(p^{1/3+\varepsilon})$ và thực tế với dữ liệu ngẫu nhiên thì gần như $O(\log p)$．
