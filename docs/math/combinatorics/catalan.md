## Mở đầu

Số Catalan xuất hiện thường xuyên trong các bài toán đếm. Nhà toán học Bỉ Eugène Charles Catalan năm 1958 nghiên cứu bài toán đếm chuỗi ngoặc và phát hiện dãy này. Nhà toán học Trung Quốc Ming Antu đã phát hiện dãy này từ thập niên 1730.

Số Catalan thỏa:

$$
C_n = \begin{cases}
1, & n = 0, \\
\sum_{i=0}^{n-1} C_{i}C_{n-1-i}, & n > 0.
\end{cases}\tag{1}
$$

Dãy bắt đầu ( [OEIS: A000108](https://oeis.org/A000108), chỉ số từ $0$ ):

$$
1,1,2,5,14,42,132,429,1430,\ldots
$$

## Ứng dụng

Truy hồi của Catalan có cấu trúc đệ quy tự nhiên: bài toán quy mô $n$ có thể chia theo điểm phân tách thành hai bài toán con $i$ và $(n-1-i)$. Vì vậy Catalan xuất hiện rộng rãi.

-   <a id="path-counting"></a>**Đếm đường đi**: Lưới $n\times n$, đi từ $(0,0)$ tới $(n,n)$, mỗi bước đi phải sang phải hoặc lên, không đi lên trên đường chéo $y=x$ (có thể chạm). Số đường đi là $C_n$.

    ??? note "Chứng minh"
        Gọi số đường đi là $T_n$. Với $n \ge 2$, xét điểm **đầu tiên** chạm đường chéo $y=x$ là $(k,k)~(k \in [1,n])$. Xét đường đi từ $(0,0)$ đến $(k,k)$ **không chạm đường chéo** trừ hai đầu.
        
        ![catalan2](./images/catalan-2.svg)
        
        Đường đi đó bắt đầu bằng bước sang phải $(1,0)$, kết thúc bằng bước lên $(k,k-1)\to(k,k)$. Do đó, nó tương ứng với đường đi từ $(1,0)$ đến $(k,k-1)$ không vượt quá đường $y=x-1$, số lượng là $T_{k-1}$. Từ $(k,k)$ đến $(n,n)$ có $T_{n-k}$ cách. Theo nguyên lý nhân, số đường đi có điểm chạm đầu tiên tại $(k,k)$ là $T_{k-1}T_{n-k}$. Cộng theo $k$:
        
        $$
        T_n = \sum_{k=1}^n T_{k-1}T_{n-k}.
        $$
        
        Đặt $k=i+1$ ta được truy hồi Catalan. Với $T_0=1$ suy ra $T_n = C_n$.

    <!-- To make bot happy. Do NOT delete this line. -->

-   **Đếm dây không cắt nhau trong đường tròn**: Có $2n$ điểm trên đường tròn, nối các điểm thành $n$ đoạn thẳng đôi một không cắt. Số cách là $C_n$.

    ??? note "Chứng minh"
        Gọi số cách là $T_n$. Đánh số các điểm $1..2n$ theo chiều kim đồng hồ. Do dây không cắt, điểm $1$ chỉ có thể nối với điểm chẵn; nếu không thì giữa hai điểm có số lẻ điểm không thể ghép. Nếu nối $1$ với $2k~(k\in[1,n])$, bên trái có $2k-2$ điểm, bên phải có $2n-2k$ điểm. Theo nguyên lý nhân, số cách là $T_{k-1}T_{n-k}$. Do đó:
        
        $$
        T_n = \sum_{k=1}^n T_{k-1} T_{n-k}.
        $$
        
        Đặt $k=i+1$ ta được truy hồi Catalan. Với $T_0=1$ suy ra $T_n=C_n$.

    <!-- To make bot happy. Do NOT delete this line. -->

-   <a id="triangulation-counting"></a>**Đếm số cách chia tam giác**: Với đa giác lồi $(n+2)$ cạnh, chia thành tam giác bằng các đường chéo không cắt nhau. Số cách là $C_n$.

    ??? note "Chứng minh"
        Gọi số cách là $T_n$. Chọn một cạnh cố định $(1,n+2)$ làm cạnh đáy. Nó thuộc một tam giác, đỉnh thứ ba là $k~(k\in[2,n+1])$. Đa giác ban đầu chia thành:
        
        -   Tam giác $(1,k,n+2)$.
        -   Đa giác $k$ cạnh, đỉnh $1\sim k$.
        -   Đa giác $(n+3-k)$ cạnh, đỉnh $k\sim (n+2)$.
        
        Hai phần sau là bài toán con, nên:
        
        $$
        T_n = \sum_{k=2}^{n+1} T_{k-2}T_{n+1-k}.
        $$
        
        Đặt $k=i+2$ ta được truy hồi Catalan. Với $T_0=T_1=1$ suy ra $T_n=C_n$.

    <!-- To make bot happy. Do NOT delete this line. -->

-   **Đếm cây nhị phân**: số dạng cây nhị phân khác nhau với $n$ đỉnh là $C_n$. Tương đương số cây nhị phân đầy đủ có $n$ đỉnh trong.

    ??? note "Chứng minh"
        Gọi số cây là $T_n$. Chọn gốc, liệt kê kích thước cây con trái $i\in[0,n-1]$, cây con phải $(n-1-i)$. Cả hai là bài toán con, nên:
        
        $$
        T_n = \sum_{i=0}^{n-1}T_iT_{n-1-i}.
        $$
        
        Đây là truy hồi Catalan. Với $T_0=T_1=1$ suy ra $T_n=C_n$.

    <!-- To make bot happy. Do NOT delete this line. -->

-   **Đếm chuỗi ngoặc hợp lệ**: Số chuỗi ngoặc hợp lệ với $n$ cặp ngoặc là $C_n$.

    ??? note "Chứng minh"
        Liên hệ bài đếm đường đi. Xem ngoặc trái là bước lên, ngoặc phải là bước phải. Điều kiện hợp lệ là mọi lúc số ngoặc trái không ít hơn số ngoặc phải, tương đương đường đi không vượt quá đường chéo. Do đó có song ánh. Số chuỗi là $C_n$.

    <!-- To make bot happy. Do NOT delete this line. -->

-   **Đếm dãy ra khỏi stack**: Dãy vào stack là $1,2,\ldots ,n$, số dãy ra hợp lệ là $C_n$.

    ??? note "Chứng minh"
        Liên hệ với chuỗi ngoặc. Vào stack là ngoặc trái, ra stack là ngoặc phải. Mọi lúc số vào không ít hơn số ra, nên song ánh với chuỗi ngoặc hợp lệ. Số dãy là $C_n$.

    <!-- To make bot happy. Do NOT delete this line. -->

-   <a id="seq-counting"></a>**Đếm dãy $\pm 1$**: Dãy gồm $n$ số $+1$ và $n$ số $-1$ sao cho mọi tổng đầu $a_1+\cdots+a_k \ge 0$ với $k=1..2n$. Số dãy là $C_n$.

    ??? note "Chứng minh"
        Xem $+1$ là ngoặc trái, $-1$ là ngoặc phải. Điều kiện tổng đầu không âm tương đương ngoặc trái không ít hơn ngoặc phải. Song ánh với chuỗi ngoặc hợp lệ. Số dãy là $C_n$.

Truy hồi phổ biến nhưng tính trực tiếp tốn kém, cần công thức đơn giản hơn.

## Các dạng thường gặp

Các biểu thức thường dùng:

$$
C_n = \frac{1}{n+1}\binom{2n}{n} = \dfrac{(2n)!}{n!(n+1)!},~ n\ge 0. \tag{2}
$$

$$
C_n = \binom{2n}{n} - \binom{2n}{n+1},~n \ge 0. \tag{3}
$$

$$
C_n = \frac{(4n-2)}{n+1}C_{n-1},~ n > 0,~ C_0 = 1. \tag{4}
$$

Ba dạng này có thể tính hiệu quả: hai dạng đầu quy về giai thừa và tổ hợp, dạng ba là truy hồi tuyến tính.

Bên dưới có hai cách chứng minh.

### Suy luận đại số

Hai bước: chứng minh ba dạng tương đương, rồi chứng minh chúng thỏa truy hồi.

??? note "Chứng minh $(2)\sim(4)$ tương đương"
    Chỉ cần chứng minh $(3)$ suy ra dạng giai thừa của $(2)$:
    
    $$
    \begin{aligned}
    C_n &= \binom{2n}{n} - \binom{2n}{n+1} \\
    &= \frac{(2n)!}{n!n!} - \frac{(2n)!}{(n-1)!(n+1)!} \\
    &= \frac{(2n)!}{n!n!}\left(1 - \frac{n!}{(n-1)!(n+1)}\right) \\
    &= \frac{(2n)!}{n!n!}\left(1- \frac{n}{n+1}\right) \\
    &= \dfrac{(2n)!}{n!(n+1)!}.
    \end{aligned}
    $$
    
    Và $(4)$ cũng suy ra dạng giai thừa:
    
    $$
    C_n = \prod_{i=1}^n\frac{(4i-2)}{i+1} = \prod_{i=1}^n\frac{2i(2i-1)}{i(i+1)} = \dfrac{(2n)!}{n!(n+1)!}.
    $$
    
    Do đó ba dạng tương đương.

Tiếp theo, dùng hàm sinh để giải truy hồi $(1)$.

??? note "Giải truy hồi $(1)$ bằng hàm sinh"
    Xét OGF $C(x)=\sum_{n=0}^{\infty}C_nx^n$. Do truy hồi là tích chập:
    
    $$
    \begin{aligned}
    C(x)&=\sum_{n=0}^{\infty}C_nx^n\\
    &=1+\sum_{n=1}^{\infty}\left(\sum_{i=0}^{n-1}C_iC_{n-i-1}\right)x^{n}\\
    &=1+x\sum_{n=1}^{\infty}\sum_{i=0}^{n-1}C_ix^iC_{n-i-1}x^{n-i-1}\\
    &=1+x\sum_{i=0}^{\infty}C_ix^i\sum_{j=0}^{\infty}C_jx^j\\
    &=1+xC^2(x).
    \end{aligned}
    $$
    
    Đổi thứ tự tổng và đặt $j=n-1-i$. Suy ra:
    
    $$
    C(x)=\dfrac{1\pm \sqrt{1-4x}}{2x} = \frac{2}{1\mp \sqrt{1-4x}}.
    $$
    
    Theo $C_0=1$, chọn nghiệm:
    
    $$
    C(x) = \dfrac{1- \sqrt{1-4x}}{2x}.
    $$
    
    Khai triển chuỗi. Dùng khai triển [chuỗi lũy thừa](../poly/intro.md#常见的幂级数展开式):
    
    $$
    \sqrt{1-4x} = \sum_{n=0}^{\infty} \dfrac{\left(\frac{1}{2}\right)_{-n}}{n!}(-4x)^n,
    $$
    
    với $\left(\dfrac{1}{2}\right)_{-n}$ là giai thừa giảm:
    
    $$
    \begin{aligned}
    \left(\frac{1}{2}\right)_{-n} &= \prod_{k=0}^{n-1}\left(\dfrac{1}{2}-k\right) = \dfrac{1}{2^n}\prod_{k=1}^{n-1}(1-2k) = \dfrac{(-1)^{n-1}}{2^n}\prod_{k=1}^{n-1}(2k-1)\\
    &= \dfrac{(-1)^{n-1}}{2^{2n-1}}\prod_{k=1}^{n-1}\dfrac{(2k-1)2k}{k} = \dfrac{(-1)^{n-1}}{2^{2n-1}}\dfrac{(2n-2)!}{(n-1)!}.
    \end{aligned}
    $$
    
    Thế vào $C(x)$:
    
    $$
    \begin{aligned}
    C(x) &= \dfrac{1}{2x}\left(1-\sum_{n=0}^{\infty} \dfrac{\left(\frac{1}{2}\right)_{-n}}{n!}(-4x)^n\right)\\
    &= -\dfrac{1}{2x}\sum_{n=1}^\infty \dfrac{(-4x)^n}{n!}\left(\frac{1}{2}\right)_{-n} \\
    &= -\dfrac{1}{2x}\sum_{n=1}^\infty \dfrac{(-4x)^n}{n!}\dfrac{(-1)^{n-1}}{2^{2n-1}}\dfrac{(2n-2)!}{(n-1)!} \\
    &= \sum_{n=1}^{\infty}\dfrac{(2n-2)!}{(n-1)!n!}x^{n-1}\\
    &= \sum_{n=0}^{\infty}\dfrac{(2n)!}{n!(n+1)!}x^n.
    \end{aligned}
    $$
    
    Suy ra biểu thức $(2)$.

### Ý nghĩa tổ hợp

Do Catalan có ý nghĩa tổ hợp rõ ràng, ta có thể chứng minh bằng đếm.

??? note "Chứng minh biểu thức $(2)$"
    Xét [bài đếm dãy](#seq-counting). Với dãy $\{a_i\}_{i=1}^{2n}$ gồm $\pm 1$, gọi tổng đầu là $S_i = \sum_{j=1}^{i}a_i$, và **độ vượt** (exceedance) là số chỉ số thỏa $S_i < 0$ và $a_i = -1$. Độ vượt bằng 0 khi dãy hợp lệ. Độ vượt nằm trong $[0,n]$ có $n+1$ giá trị. Ta cần chứng minh số dãy ở các độ vượt khác nhau là như nhau.
    
    Xây dựng ánh xạ $f$ từ dãy có độ vượt $e>0$ sang dãy có độ vượt $e-1$. Với dãy $\{a_i\}$, chọn chỉ số nhỏ nhất $k$ sao cho $S_i=0$ và $a_i=+1$. Đổi chỗ đoạn bên trái và bên phải $a_k$:
    
    $$
    a_{k+1},a_{k+2},\cdots,a_{2n},a_k,a_{1},a_{2},\cdots,a_{k-1}.
    $$
    
    Phần bên phải $a_k$ giữ nguyên độ vượt. Phần bên trái sau khi đổi, mọi tổng đầu tăng 1, nên giảm số chỉ số thỏa $S_i=-1$ và $a_i=-1$ đúng 1 lần. Do đó độ vượt giảm đúng 1.
    
    Ánh xạ $f$ là song ánh. Trong dãy mới, vị trí $a_k$ là chỉ số lớn nhất thỏa $S'_k=+1$ và $a'_k=+1$. Lý do: các tổng đầu bên trái tăng 1, nên $S'_k=+1$ tương ứng $S_k=0$ trước khi đổi, và theo chọn $k$, bên trái không có chỉ số nào thỏa điều đó. Vì vậy $f$ là song ánh giữa độ vượt $e$ và $e-1$. Suy ra số dãy ở mọi độ vượt bằng nhau.
    
    Tổng số dãy là $\dbinom{2n}{n}$, nên số dãy hợp lệ (độ vượt 0) là
    
    $$
    C_n = \dfrac{1}{n+1}\dbinom{2n}{n}.
    $$

??? note "Chứng minh biểu thức $(3)$"
    Xét [bài đếm đường đi](#path-counting). Dùng nguyên lý phản xạ. Tổng số đường đi là $\dbinom{2n}{n}$. Đường đi không hợp lệ khi chạm đường $y=x+1$. Với một đường đi không hợp lệ, tìm điểm đầu tiên chạm $y=x+1$, phản xạ phần sau qua đường đó. Khi đó, đường đi từ $(0,0)$ tới $(n,n)$ không hợp lệ biến thành đường đi từ $(0,0)$ tới $(n-1,n+1)$.
    
    ![catalan1](./images/catalan-1.svg)
    
    Mỗi đường từ $(0,0)$ đến $(n-1,n+1)$ đều cắt $y=x+1$, nên tương ứng 1-1 với đường không hợp lệ. Số đường như vậy là $\dbinom{2n}{n+1}$. Vậy số đường hợp lệ:
    
    $$
    C_n = \binom{2n}{n} - \binom{2n}{n+1}.
    $$

??? note "Chứng minh biểu thức $(4)$"
    Xét [bài chia tam giác](#triangulation-counting). Gọi $P$ là đa giác lồi $(n+2)$ cạnh, cố định một cạnh đáy. Với mỗi cách chia tam giác của $P$, chọn và định hướng một cạnh không phải cạnh đáy (kể cả cạnh mới). Có $(4n+2)C_n$ cách “chia + đánh dấu”.
    
    Gọi $Q$ là đa giác lồi $(n+3)$ cạnh, cố định cạnh đáy. Với $Q$, chọn một cạnh không phải đáy để đánh dấu rồi chia tam giác: có $(n+2)C_{n+1}$ cách.
    
    ![](./images/catalan-triangulation.svg)
    
    Hai thao tác có song ánh rõ ràng: từ $P$ với cạnh đánh dấu, mở rộng cạnh đó thành tam giác và thêm một cạnh mới được đánh dấu trong $Q$; ngược lại, từ $Q$ với cạnh đánh dấu, co lại thành một đỉnh và đánh dấu cạnh tương ứng trong $P$. Do đó:
    
    $$
    (4n+2)C_n = (n+2)C_{n+1}.
    $$
    
    Sắp xếp lại và dùng $C_0=1$ ra biểu thức $(4)$.

## Ví dụ

???+ example "[Luogu P1044 栈](https://www.luogu.com.cn/problem/P1044)"
    Thứ tự vào stack là $1,2,\ldots ,n$, hỏi số dãy ra hợp lệ.

??? note "Mã tham khảo"
    === "C++"
        ```cpp
        --8<-- "docs/math/code/combinatorics/catalan/catalan_1.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/math/code/combinatorics/catalan/catalan_1.py"
        ```

## Bài tập

-   [Luogu P2532 \[AHOI2012\] 树屋阶梯](https://www.luogu.com.cn/problem/P2532)
-   [Luogu P1641 \[SCOI2010\] 生成字符串](https://www.luogu.com.cn/problem/P1641)
-   [Luogu P3200 \[HNOI2009\] 有趣的数列](https://www.luogu.com.cn/problem/P3200)
-   [AtCoder Beginner Contest 205 E - White and Black Balls](https://atcoder.jp/contests/abc205/tasks/abc205_e)
-   [AtCoder Regular Contest 145 C - Split and Maximize](https://www.luogu.com.cn/problem/AT_arc145_c)
-   [Luogu P5014 水の三角（修改版）](https://www.luogu.com.cn/problem/P5014)
-   [Luogu P3978 \[TJOI2015\] 概率论](https://www.luogu.com.cn/problem/P3978)

## Tài liệu tham khảo và chú thích

-   [Catalan number - Wikipedia](https://en.wikipedia.org/wiki/Catalan_number)
