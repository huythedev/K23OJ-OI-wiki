autor: iamtwz, billchenchina, CBW2007, CCXXXI, chinggg, Enter-tainer, eyedeng, FFjet, gaojude, Great-designer, H-J-Granger, Henry-ZHR, hsfzLZH1, Ir1d, kenlig, Konano, ksyx, luoguyuntianming, Marcythm, Menci, NachtgeistW, ouuan, Peanut-Tang, qwqAutomaton, sshwy, StudyingFather, Tiphereth-A, TrisolarisHD, TRSWNCA, Xeonacid, Yuuko10032, Zhangjiacheng2006, Zhoier, Hszzzx, shenshuaijie, kfy666

## Giới thiệu

**Lũy thừa nhanh** (fast exponentiation), còn gọi là **nhị phân lũy thừa** (binary exponentiation) hay **bình phương lũy thừa** (exponentiation by squaring), là một kỹ thuật tính $a^n$ trong $\Theta(\log n)$ thời gian, trong khi cách vét cạn cần $\Theta(n)$.

Kỹ thuật này áp dụng cho mọi bối cảnh mà phép nhân của $a$ thỏa mãn tính kết hợp, như lũy thừa modulo, lũy thừa ma trận, v.v., xem thêm ở mục [Ứng dụng](#Ứng dụng).

## Quy trình

Tính $a^n$ tức là nhân $n$ lần $a$: $a^{n} = \underbrace{a \times a \cdots \times a}_{n\text{ lần a}}$. Tuy nhiên khi $n$ lớn hoặc phép nhân tốn kém, cách này không phù hợp. Ý tưởng của nhị phân lũy thừa là chia nhỏ bài toán theo **biểu diễn nhị phân** của số mũ.

???+ example "Ví dụ"
    Cần tính $3^{13}$. Nếu nhân liên tiếp thì cần $13-1=12$ phép nhân. Nhưng vì
    
    $$
    3^{13} = 3^{(1101)_2} = 3^8 \times 3^4 \times 3^1,
    $$
    
    nên chỉ cần tính nhanh $3^{1},3^{2},3^{4},3^{8}$ rồi nhân lại, mất $2$ phép nhân để ra $3^{13}$. Ta chỉ cần cách tính nhanh dãy các lũy thừa $3^{2^k}$. Điều này dễ vì mỗi phần tử (trừ phần tử đầu) là bình phương của phần tử trước.
    
    Quá trình tính $3^{13}$:
    
    $$
    \begin{aligned}
    3^1 &= 3, \\
    3^2 &= \left(3^1\right)^2 = 3^2 = 9, \\
    3^4 &= \left(3^2\right)^2 = 9^2 = 81, \\
    3^8 &= \left(3^4\right)^2 = 81^2 = 6561, \\
    3^{13} &= 6561 \times 81 \times 3 = 1594323.
    \end{aligned}
    $$
    
    Quá trình chỉ thực hiện $5$ phép nhân.

Đó là ý tưởng cơ bản. Có hai cách cài đặt thường gặp.

### Phiên bản lặp

Gọi biểu diễn nhị phân của $n$ là $(n_tn_{t-1}\cdots n_1n_0)_2$, tức

$$
n = n_t2^t + n_{t-1}2^{t-1} + \cdots + n_12^1 + n_02^0,
$$

với $n_i\in\{0,1\}$. Khi đó

$$
\begin{aligned}
a^n & = a^{n_t2^t + n_{t-1}2^{t-1} + \cdots + n_12^1 + n_02^0}\\
& = a^{n_0 2^0} \times a^{n_1 2^1}\times \cdots \times a^{n_{t-1}2^{t-1}} \times a^{n_t2^t}.
\end{aligned}
$$

Chỉ các hạng với $n_i=1$ mới xuất hiện trong tích.

Từ biểu thức này, ta có thể tính $a^{2^k}$ trong $\Theta(\log n)$ thời gian, rồi nhân các lũy thừa tương ứng với bit bằng $1$ để ra kết quả trong $\Theta(\log n)$ thời gian. Đó là phiên bản lặp.

Mã giả:

$$
\begin{array}{l}
\textbf{Algorithm }\text{FastPow}(a, n): \\
\textbf{Input. }\text{Base }a\text{ and exponent }n.\\
\textbf{Output. }\text{Power }a^n.\\
\textbf{Method.}\\
\begin{array}{ll}
1 & \textit{result}\gets\mathrm{Id}\\
2 & \textbf{while }n > 0\textbf{ do}\\
3 & \qquad \textbf{if }n \bmod 2 = 1\textbf{ then}\\
4 & \qquad \qquad \textit{result} \gets \textit{result}\cdot a\\
5 & \qquad \textbf{end if}\\
6 & \qquad a \gets a \cdot a\\
7 & \qquad n \gets n / 2\\
8 & \textbf{end while}\\
9 & \textbf{return }\textit{result}
\end{array}
\end{array}
$$

Cách này cần $\Theta(\log n)$ phép nhân.

### Phiên bản đệ quy

Quy trình cũng có thể viết đệ quy. Lưu ý biểu diễn nhị phân của $n$ có thể viết:

$$
(n_tn_{t-1}\cdots n_1n_0)_2 = 2 \times (n_tn_{t-1}\cdots n_1)_2 + n_0.
$$

Do đó

$$
a^n = \begin{cases}
1, & n = 0,\\
(a^{\lfloor n/2\rfloor})^2, & n > 0 \text{ và }n\text{ chẵn},\\
(a^{\lfloor n/2\rfloor})^2\cdot a, & n > 0 \text{ và }n\text{ lẻ}.\\
\end{cases}
$$

Mã giả:

$$
\begin{array}{l}
\textbf{Algorithm }\text{FastPow}(a, n): \\
\textbf{Input. }\text{Base }a\text{ and exponent }n.\\
\textbf{Output. }\text{Power }a^n.\\
\textbf{Method.}\\
\begin{array}{ll}
1 & \textbf{if }n = 0\textbf{ then}\\
2 & \qquad \textbf{return }\mathrm{Id}\\
3 & \textbf{end if}\\
4 & \textit{result} \gets \text{FastPow}(a, n / 2) \\
5 & \textbf{if }n\bmod 2 = 0\textbf{ then}\\
6 & \qquad \textbf{return }\textit{result}\cdot\textit{result}\\
7 & \textbf{else}\\
8 & \qquad \textbf{return }\textit{result}\cdot\textit{result}\cdot a\\
9 & \textbf{end if}
\end{array}
\end{array}
$$

Cách này đệ quy $\Theta(\log n)$ lần và cũng cần $\Theta(\log n)$ phép nhân. Do overhead đệ quy, thực tế phiên bản lặp thường nhanh hơn.

## Ứng dụng

### Lũy thừa modulo

???+ example "[洛谷 P1226【模板】快速幂](https://www.luogu.com.cn/problem/P1226)"
    Cho ba số nguyên $a,b,p$, tính $a^b\bmod p$ với $p\ge 2$.

Đây là ứng dụng rất phổ biến, ví dụ để tính nghịch đảo nhân modulo. Vì phép lấy modulo không ảnh hưởng đến phép nhân, chỉ cần lấy modulo trong quá trình tính.

Ta có thể cài đặt trực tiếp theo bản đệ quy:

???+ note "Cài đặt tham khảo"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/binary-exponentiation/luogu-P1226-1.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/binary-exponentiation/luogu-P1226-1.py:core"
        ```

Cách thứ hai là không đệ quy. Trong vòng lặp, ta nhân vào kết quả khi bit nhị phân bằng 1. Dù độ phức tạp lý thuyết giống nhau, cách này thường nhanh hơn vì không có overhead đệ quy.

???+ note "Cài đặt tham khảo"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/binary-exponentiation/luogu-P1226-2.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/binary-exponentiation/luogu-P1226-2.py:core"
        ```

???+ warning "Lưu ý"
    -   Thông thường mô-đun lớn hơn $1$. Trong trường hợp rất đặc biệt $p=1$, cần xét riêng trường hợp $b=0$.
    -   Khi số mũ rất lớn, cần dùng [định lý Euler mở rộng](./number-theory/fermat.md#扩展欧拉定理) để giảm mũ.

### Tính số Fibonacci

Theo truy hồi $F_n = F_{n-1} + F_{n-2}$, ta có thể xây dựng ma trận $2\times 2$ biểu diễn biến đổi từ $F_i,F_{i+1}$ sang $F_{i+1},F_{i+2}$. Khi tính ma trận này mũ $n$, áp dụng lũy thừa nhanh sẽ cho kết quả trong $\Theta(\log n)$. Xem thêm [dãy Fibonacci](./combinatorics/fibonacci.md) và [tăng tốc truy hồi bằng ma trận](../math/linear-algebra/matrix.md#矩阵加速递推).

### Lặp hoán vị nhiều lần

???+ note "Mô tả bài toán"
    Cho một dãy độ dài $n$ và một hoán vị, áp dụng hoán vị đó $k$ lần.

Chỉ cần lấy lũy thừa $k$ của hoán vị rồi áp dụng lên dãy. Độ phức tạp $O(n \log k)$. Xem thêm [hợp thành hoán vị](./permutation.md#复合).

???+ warning "Lưu ý"
    Xây dựng đồ thị của hoán vị và xử lý từng chu trình với $k$ lần (tương đương $k$ modulo độ dài chu trình) sẽ giải trong $O(n)$.

### Tăng tốc thao tác hình học trên tập điểm

???+ example "[HDU 4087 A Letter to Programmers](https://acm.hdu.edu.cn/showproblem.php?pid=4087)"
    Cho $n$ điểm $p_i$ trong không gian 3D, áp dụng $m$ thao tác, gồm:
    
    1.  Dời điểm theo một vector (Shift).
    2.  Co/giãn theo tỉ lệ (Scale).
    3.  Quay quanh một đường thẳng (Rotate).
    
    Còn có thao tác đặc biệt lặp lại một chuỗi thao tác $k$ lần (Repeat), Repeat có thể lồng nhau. Yêu cầu tọa độ cuối cùng.

Tham khảo [Vector và ma trận](./linear-algebra/vector.md#向量与矩阵). Mỗi thao tác là một ma trận biến đổi, chuỗi thao tác là tích ma trận. Repeat tương đương lấy lũy thừa ma trận. Nhờ đó tính ma trận tổng hợp trong $O(m \log k)$, rồi áp dụng lên $n$ điểm, tổng $O(n + m \log k)$.

### Đếm đường đi độ dài cố định

???+ note "Mô tả bài toán"
    Cho đồ thị có hướng (trọng số cạnh bằng 1), đếm số đường đi độ dài $k$ từ $u$ đến $v$ cho mọi cặp.

Lấy ma trận kề $M$ mũ $k$, khi đó $M_{i,j}$ là số đường đi độ dài $k$ từ $i$ đến $j$. Độ phức tạp $O(n^3 \log k)$. Xem [Ma trận](./linear-algebra/matrix.md#定长路径统计).

### Nhân số nguyên modulo

???+ note "Mô tả bài toán"
    Cho $a,b$ không âm và $m$ dương, tính $a\times b\bmod m$, với $a,b\le m\le 10^{18}$.

Tương tự lũy thừa nhanh, ta biểu diễn một thừa số thành tổng các lũy thừa của 2. Vì nhân 2 và lấy modulo có thể chuyển thành cộng/trừ để tránh tràn, nên bài toán giải được trong $O(\log m)$. Công thức đệ quy:

$$
a \cdot b = \begin{cases}
0 &\text{if }a = 0 \\
2 \cdot \frac{a}{2} \cdot b &\text{if }a > 0 \text{ and }a \text{ even} \\
2 \cdot \frac{a-1}{2} \cdot b + b &\text{if }a > 0 \text{ and }a \text{ odd}
\end{cases}
$$

Tuy nhiên trong thực tế, cách này do phức tạp lớn hơn nên kém hiệu quả. Thường dùng [nhân nhanh](./number-theory/mod-arithmetic.md#快速乘) khi mô-đun trong `long long`.

### Lũy thừa nhanh với số lớn

Kỹ năng trước: [nhân số lớn](./bignum.md#乘法)

???+ example "[洛谷 P1045 \[NOIP 2003 普及组\] 麦森数](https://www.luogu.com.cn/problem/P1045)"
    Cho $P$ ($1000 < P < 3100000$), tính số chữ số của $2^P−1$ và 500 chữ số cuối (thập phân), nếu thiếu thì thêm 0 ở đầu.

??? note "Cài đặt"
    ```cpp
    --8<-- "docs/math/code/binary-exponentiation/luogu-P1045.cpp"
    ```

## Tiền xử lý lũy thừa nhanh khi cố định cơ số

Khi cơ số $a$ cố định, có thể dùng [phân khối](../ds/decompose.md) để tiền xử lý và trả lời mỗi truy vấn trong $O(1)$. Thuật toán này còn gọi là “lũy thừa ánh sáng”. Quy trình:

1.  Chọn $s$, tiền xử lý $a^0,a^1,\cdots,a^{s-1}$ và $a^0,a^s,\cdots,a^{\lfloor p/s\rfloor s}$ vào hai mảng;
2.  Với truy vấn $a^b$, tách $b=\lfloor b/s\rfloor s+(b\bmod s)$, khi đó $a^b=a^{\lfloor b/s\rfloor s}\cdot a^{b\bmod s}$, trả lời $O(1)$.

Nếu $b \in [0,n]$, thường chọn $s\approx \sqrt{n}$ hoặc gần lũy thừa của $2$. Chọn $\sqrt{n}$ tối ưu tiền xử lý $O(\sqrt{n})$, còn chọn lũy thừa của $2$ giúp đơn giản hóa bằng bit.

Đặc biệt với lũy thừa modulo, cơ số $a$ cố định ngầm nghĩa mô-đun $m$ cũng cố định. Do [định lý Euler mở rộng](./number-theory/fermat.md#扩展欧拉定理), biên trên mũ là $n = 2\varphi(m)$; với mô-đun nguyên tố $p$, biên là $n = p - 1$. Cả hai trường hợp đều tiền xử lý $O(\sqrt{m})$.

???+ example "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/binary-exponentiation/pre-exp.cpp:core"
    ```

## Bài tập

-   [UVa 1230 - MODEX](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=3671)
-   [UVa 374 - Big Mod](http://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=310)
-   [UVa 11029 - Leading and Trailing](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1970)
-   [Codeforces - Parking Lot](http://codeforces.com/problemset/problem/630/I)
-   [SPOJ - The last digit](http://www.spoj.com/problems/LASTDIG/)
-   [SPOJ - Locker](http://www.spoj.com/problems/LOCKER/)
-   [SPOJ - Just add it](http://www.spoj.com/problems/ZSUM/)

**Một phần nội dung trang này được dịch từ bài [Бинарное возведение в степень](http://e-maxx.ru/algo/binary_pow) và bản dịch tiếng Anh [Binary Exponentiation](https://cp-algorithms.com/algebra/binary-exp.html). Bản tiếng Nga có giấy phép Public Domain + Leave a Link; bản tiếng Anh có giấy phép CC-BY-SA 4.0.**
