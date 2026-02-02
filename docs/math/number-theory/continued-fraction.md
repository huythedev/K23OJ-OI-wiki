author: 383494, CCXXXI, chunibyo-wly, Enter-tainer, Great-designer, megakite, Menci, shawlleyw, shuzhouliu, StudyingFather, Tiphereth-A, untitledunrevised, c-forrest

## Giới thiệu

Liên phân số có thể biểu diễn số thực như giới hạn của một dãy phân số hữu tỉ hội tụ. Dãy phân số này dễ tính và cung cấp xấp xỉ tốt nhất cho số thực đó, nên thường được dùng trong lập trình thi đấu. Ngoài ra, liên phân số liên hệ chặt chẽ với thuật toán Euclid nên có thể áp dụng cho nhiều bài toán số học.

???+ info "Về hiện thực thuật toán liên phân số"
    Bài viết cung cấp một loạt hiện thực thuật toán liên phân số; một số thuật toán có thể không đảm bảo các số nguyên trung gian nằm trong phạm vi 32/64-bit. Trong trường hợp này, hãy tham khảo bản Python, hoặc thay kiểu số nguyên trong C++ bằng [lớp số nguyên lớn](../bignum.md). Để nhấn mạnh trọng tâm, một số đoạn mã sẽ gọi lại các hàm đã cài đặt ở phần trước và không lặp lại.

## Liên phân số

**Liên phân số** (continued fraction) tự thân chỉ là một ký hiệu hình thức.

???+ abstract "Liên phân số hữu hạn"
    Với dãy $\{a_k\}_{i=0}^n$, liên phân số $[a_0,a_1,\cdots,a_n]$ biểu diễn khai triển
    
    $$
    x = a_0+\dfrac{1}{a_1+\dfrac{1}{a_2+\dfrac{1}{\cdots+\dfrac{1}{a_n}}}}.
    $$
    
    Liên phân số có nghĩa khi và chỉ khi khai triển tương ứng có nghĩa. Các $a_k$ gọi là **hạng** (term) hoặc **hệ số** (coefficient) của liên phân số.

???+ info "Ký hiệu"
    Dạng tổng quát hơn cho phép tử số trong khai triển không nhất thiết bằng $1$, khi đó ký hiệu liên phân số cũng cần thay đổi; điều này vượt ra ngoài phạm vi bài viết. Ngoài ra, một số tài liệu viết dấu phẩy đầu tiên bằng dấu chấm phẩy “$;$”, nhưng ý nghĩa không khác với ký hiệu ở đây.

Liên phân số cũng có thể mở rộng cho dãy vô hạn.

???+ abstract "Liên phân số vô hạn"
    Với dãy vô hạn $\{a_k\}_{i=0}^\infty$, liên phân số $[a_0,a_1,\cdots]$ biểu diễn giới hạn
    
    $$
    x = \lim_{k\rightarrow\infty} x_k = \lim_{k\rightarrow\infty} [a_0,a_1,\cdots,a_k].
    $$
    
    Liên phân số có nghĩa khi và chỉ khi giới hạn này tồn tại. Ở đây $x_k=[a_0,a_1,\cdots,a_k]$ gọi là **phân số tiệm cận** (convergent) thứ $k$ của $x$, còn $r_k=[a_k,a_{k+1},\cdots]$ gọi là **phần dư** hoặc **thương hoàn chỉnh** (complete quotient) thứ $k$ của $x$. Tương ứng, hạng $a_k$ đôi khi gọi là **thương riêng** (partial quotient) thứ $k$.

### Liên phân số đơn giản

Trong số học, chủ yếu xét liên phân số có các hạng đều là số nguyên.

???+ abstract "Liên phân số đơn giản"
    Với liên phân số $[a_0,a_1,\cdots]$, nếu $a_0$ là số nguyên, $a_1,a_2,\cdots$ đều là số nguyên dương, thì gọi là **liên phân số đơn giản** (simple continued fraction), cũng gọi tắt là **liên phân số**. Nếu dãy $\{a_i\}$ hữu hạn thì gọi là **liên phân số (đơn giản) hữu hạn**; ngược lại là **liên phân số (đơn giản) vô hạn**. Ngoài ra, $a_0$ gọi là **phần nguyên** (integer part) của nó.

Trừ khi nói rõ, “liên phân số” trong bài đều là liên phân số đơn giản. Có thể chứng minh liên phân số đơn giản vô hạn luôn hội tụ và các phần dư của nó luôn dương.

Liên phân số có các tính chất cơ bản sau:

???+ note "Tính chất"
    Cho số thực $x=[a_0,a_1,a_2,\cdots]$. Khi đó:
    
    1.  Với mọi $k\in\mathbf Z$, $x+k=[a_0+k,a_1,a_2,\cdots]$;
    2.  Với $x>1$, ta có $a_0>0$, và $x^{-1}=[0,a_0,a_1,a_2,\cdots]$.

Liên phân số hữu hạn tương ứng với số hữu tỉ. Mỗi số hữu tỉ có đúng hai cách biểu diễn liên phân số, độ dài một chẵn một lẻ. Hai cách chỉ khác ở việc hạng cuối cùng có bằng $1$ hay không:

$$
x = [a_0,a_1,\cdots,a_n] = [a_0,a_1,\cdots,a_n-1,1].
$$

Hai liên phân số này gọi là **biểu diễn liên phân số** (continued fraction representation) của số hữu tỉ $x$. Cách có hạng cuối khác $1$ gọi là biểu diễn chuẩn, còn hạng cuối bằng $1$ gọi là biểu diễn không chuẩn.[^one-representation]

??? example "Ví dụ"
    Số hữu tỉ $x=\dfrac{5}{3}$ có biểu diễn liên phân số
    
    $$
    \begin{aligned}
    x = [1,1,1,1] &= 1+\dfrac{1}{1+\dfrac{1}{1+\dfrac{1}{1}}},\\
    x = [1,1,2] &= 1+\dfrac{1}{1+\dfrac{1}{2}}.
    \end{aligned}
    $$

Liên phân số vô hạn tương ứng với số vô tỉ. Mỗi số vô tỉ có duy nhất một biểu diễn liên phân số, gọi là biểu diễn liên phân số của số vô tỉ.

### Cách tìm biểu diễn liên phân số

Để tìm biểu diễn liên phân số của số thực $x$, lưu ý phần dư $r_k$ nếu không phải số nguyên thì luôn thỏa

$$
r_k = [a_k,a_{k+1},\cdots] = [a_k,r_{k+1}] = a_k + \dfrac{1}{r_{k+1}}.
$$

Và $r_{k+1}>1$. Do đó, bắt đầu từ $r_0=x$, ta đệ quy:

$$
a_k = \lfloor r_k\rfloor,\ r_{k+1} = \dfrac{1}{r_k-a_k}.
$$

Dãy $\{a_k\}$ sinh ra là duy nhất, trừ khi có phần dư $r_k$ là số nguyên. Nếu có, quá trình dừng và có thể xuất ra biểu diễn chuẩn hoặc không chuẩn.

Trong lập trình thi đấu, thường xét số hữu tỉ $x=\dfrac{p}{q}$. Khi đó mỗi $r_k$ đều là hữu tỉ $\dfrac{p_k}{q_k}$, và với $k>0$, do $r_k>1$ nên $p_k>q_k$. Từ đệ quy trên suy ra

$$
a_k = \left\lfloor\frac{p_k}{q_k}\right\rfloor,\ r_{k+1} = \dfrac{1}{r_k-a_k} = \dfrac{q_k}{p_k-a_kq_k} = \dfrac{q_k}{p_k\bmod q_k}.
$$

Quá trình này thực chất là [thuật toán Euclid](./gcd.md#欧几里得算法) trên $p$ và $q$. Do đó, độ dài biểu diễn liên phân số của $\dfrac{p}{q}$ là $O(\log\min\{p, q\})$, và độ phức tạp tính toán cũng là $O(\log\min\{p, q\})$.

???+ example "Hiện thực tham khảo"
    Cho tử số $p$ và mẫu số $q$, xuất ra dãy hệ số $[a_0,a_1,\cdots,a_n]$.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/diophantine.cpp:fraction"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/diophantine.py:fraction"
        ```

## Phân số tiệm cận

Ở định nghĩa liên phân số đã nêu khái niệm phân số tiệm cận. Phân số tiệm cận của số thực là các phân số tiệm cận của biểu diễn liên phân số của nó: trong biểu diễn liên phân số của $x$, giữ lại $k$ hạng đầu, thu được $x_k$ gọi là phân số tiệm cận thứ $k$ của $x$. Các $x_k$ đều là hữu tỉ và dãy $\{x_k\}$ hội tụ về $x$.

??? example "Ví dụ: phân số tiệm cận của tỉ lệ vàng"
    Liên phân số $x=[1,1,1,1,\cdots]$ có các phân số tiệm cận đầu là
    
    $$
    \begin{aligned}
    x_0 &= [1]=1,\\
    x_1 &= [1,1]=2,\\
    x_2 &= [1,1,1]=\dfrac{3}{2},\\
    x_3 &= [1,1,1,1]=\dfrac{5}{3},\\
    x_4 &= [1,1,1,1,1]=\dfrac{8}{5}.
    \end{aligned}
    $$
    
    Có thể quy nạp chứng minh
    
    $$
    x_k = \frac{F_{k+2}}{F_{k+1}},
    $$
    
    trong đó $\{F_k\}$ là [dãy Fibonacci](../combinatorics/fibonacci.md). Từ công thức tổng quát suy ra
    
    $$
    x_k = \frac{\phi^{k+2}-(-\phi)^{-(k+2)}}{\phi^{k+1}-(-\phi)^{-(k+1)}},
    $$
    
    với $\phi=\dfrac{1+\sqrt{5}}{2}$ là tỉ lệ vàng. Khi $k\to\infty$,
    
    $$
    x=\lim_{k\rightarrow\infty}x_k=\phi.
    $$
    
    Do đó liên phân số $x=[1,1,1,1,\cdots]$ biểu diễn tỉ lệ vàng $\phi$.

Các phân số tiệm cận này tiến gần số thực tương ứng nên dùng để xấp xỉ số thực. Vì vậy cần hiểu các tính chất của chúng.

### Công thức truy hồi

Để tính các phân số tiệm cận, không cần tính lại từ đầu mỗi khi thêm hạng. Thực tế có quan hệ truy hồi sau:

???+ note "Công thức truy hồi"
    Với $x=[a_0,a_1,a_2,\cdots]$, giả sử phân số tiệm cận thứ $k$ là $\dfrac{p_k}{q_k}$. Khi đó
    
    $$
    \begin{aligned}
    p_k &= a_kp_{k-1}+p_{k-2},\\
    q_k &= a_kq_{k-1}+q_{k-2}.
    \end{aligned}
    $$
    
    Điểm xuất phát (phân số hình thức) là
    
    $$
    x_{-1}=\frac{p_{-1}}{q_{-1}}=\frac{1}{0},\ x_{-2}=\frac{p_{-2}}{q_{-2}}=\frac{0}{1}.
    $$

??? note "Chứng minh"
    Tử và mẫu của phân số tiệm cận $x_k$ có thể coi là đa thức nhiều biến của $a_0, a_1, \cdots, a_k$:
    
    $$
    r_k = \frac{P_k(a_0, a_1, \cdots, a_k)}{Q_k(a_0,a_1, \cdots, a_k)}.
    $$
    
    Theo định nghĩa,
    
    $$
    r_k = a_0 + \frac{1}{[a_1,a_2,\cdots, a_k]}= a_0 + \frac{Q_{k-1}(a_1, \cdots, a_k)}{P_{k-1}(a_1, \cdots, a_k)} = \frac{a_0 P_{k-1}(a_1, \dots, a_k) + Q_{k-1}(a_1, \cdots, a_k)}{P_{k-1}(a_1, \cdots, a_k)}.
    $$
    
    So sánh với trên, đặt $Q_k(a_0, \cdots, a_k) = P_{k-1}(a_1, \cdots, a_k)$ thì
    
    $$
    r_k =  \frac{P_k(a_0, a_1, \cdots, a_k)}{P_{k-1}(a_1, \cdots, a_k)}
    $$
    
    và $P_k$ thỏa
    
    $$
    P_k(a_0, \cdots, a_k) = a_0 P_{k-1}(a_1, \cdots, a_k) + P_{k-2}(a_2, \cdots, a_k).
    $$
    
    Vì
    
    $$
    r_0 = a_0,\ r_1 = a_0+\dfrac{1}{a_1} = \frac{a_0a_1+1}{a_1},
    $$
    
    nên điểm đầu:
    
    $$
    P_0(a_0) = a_0,\ P_1(a_0,a_1) = a_0a_1 + 1.
    $$
    
    Đặt
    
    $$
    P_{-1} = 1,\ P_{-2} = 0,
    $$
    
    thì với $k=0,1$ cũng thỏa quan hệ trên. Điều này tương đương với việc quy ước phân số hình thức $r_{-1}=\dfrac{1}{0}$ và $r_{-2}=\dfrac{0}{1}$.
    
    Dãy $P_k$ gọi là **liên đa thức**[^continuant] (continuant). Nó có thể viết dạng định thức:
    
    $$
    P_k(a_0,\cdots,a_k)=\det
    \begin{pmatrix}
    a_0 & 1 & 0 & \cdots & 0 \\
    -1 & a_1 & 1 & \ddots & \vdots \\
    0 & -1 & a_2 & \ddots & 0 \\
    \vdots & \ddots & \ddots & \ddots & 1 \\
    0 & \cdots & 0 & -1 & a_k
    \end{pmatrix}.
    $$
    
    Đây là định thức của [ma trận ba đường chéo](https://en.wikipedia.org/wiki/Tridiagonal_matrix). Khai triển từ góc trái trên suy ra truy hồi và điều kiện đầu. Khai triển từ góc phải dưới thu được
    
    $$
    P_k(a_0, \cdots, a_k) = a_k P_{k-1}(a_0, \cdots, a_{k-1}) + P_{k-2}(a_0, \cdots, a_{k-2}),
    $$
    
    đúng như cần chứng minh.

???+ info "Ký hiệu"
    Khi ký hiệu phân số tiệm cận $x_k$ là $\dfrac{p_k}{q_k}$, mặc định $p_k,q_k$ được cho bởi truy hồi trên. Sau đó sẽ thấy luôn thu được dạng tối giản.

Hệ thức này cho thấy

$$
x_k=\dfrac{a_kp_{k-1}+p_{k-2}}{a_kq_{k-1}+q_{k-2}}
$$

nằm giữa $x_{k-1}$ và $x_{k-2}$.

Từ truy hồi có các định lý đảo thứ tự và định lý nghịch đảo:

???+ note "Định lý đảo thứ tự"
    Với $x=[a_0,a_1,a_2,\cdots]$ có phân số tiệm cận thứ $k$ là $\dfrac{p_k}{q_k}$, ta có
    
    $$
    \begin{aligned}
    \frac{p_k}{p_{k-1}}&=[a_k,a_{k-1},\cdots,a_1,a_0],\\
    \frac{q_k}{q_{k-1}}&=[a_k,a_{k-1},\cdots,a_1].
    \end{aligned}
    $$
    
    Nếu $a_0=0$ thì liên phân số đầu nên hiểu là cắt ở hạng áp chót, tức $[a_k,a_{k-1},\cdots,a_2]$.

??? note "Chứng minh"
    Chia hai vế truy hồi của $p_k$ và $q_k$ cho $p_{k-1}$, $q_{k-1}$, được
    
    $$
    \begin{aligned}
    \frac{p_k}{p_{k-1}} &= a_k + \frac{p_{k-2}}{p_{k-1}},\\
    \frac{q_k}{q_{k-1}} &= a_k + \frac{q_{k-2}}{q_{k-1}}.
    \end{aligned}
    $$
    
    Lặp lại sẽ cho hai liên phân số, rồi thay $ \dfrac{p_0}{p_{-1}}=a_0$ và $\dfrac{q_1}{q_0}=a_1$. Với $a_0=0$, ta hiểu liên phân số như biểu thức hình thức; phần dư
    
    $$
    [a_2,a_1,0]=a_2+\dfrac{1}{a_1+\dfrac{1}{0}}=a_2+\dfrac{0}{0a_1+1}=a_2.
    $$
    
    Do đó bỏ hai hạng cuối. Muốn chặt chẽ, xem như giới hạn $a_0\to 0$.

???+ note "Định lý nghịch đảo"
    Với $x>0$, nghịch đảo của các phân số tiệm cận của $x$ là các phân số tiệm cận của $x^{-1}$.

??? note "Chứng minh"
    Giả sử $x>1$ có biểu diễn $[a_0,a_1,a_2,\cdots]$ thì $x^{-1}=[0,a_0,a_1,a_2,\cdots]$\. Các phân số tiệm cận suy ra từ truy hồi. Với $x$ có $x_{-2}=\dfrac{0}{1}$, $x_{-1}=\dfrac{1}{0}$; với $y=x^{-1}$ có $y_{-1}=\dfrac{1}{0}$, $y_0=\dfrac{0}{1}$. Do đó $x_{-2}=(y_{-1})^{-1}$ và $x_{-1}=(y_0)^{-1}$. Từ truy hồi suy ra $x_k=y_{k+1}^{-1}$. Trường hợp $0<x\le1$ tương tự.

Từ truy hồi ta có thuật toán tính phân số tiệm cận:

???+ example "Hiện thực tham khảo"
    Cho các hệ số $a_0,a_1,\cdots,a_n$, tính dãy $(p_0,q_0),(p_1,q_1),\cdots,(p_n,q_n)$.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/diophantine.cpp:convergents"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/diophantine.py:convergents"
        ```

### Ước lượng sai số

Từ truy hồi, có thể ước lượng sai số khi dùng phân số tiệm cận để xấp xỉ số thực.

Trước hết, tính hiệu giữa hai phân số tiệm cận kề nhau:

???+ note "Hiệu giữa hai phân số tiệm cận"
    Với $x_k=\dfrac{p_k}{q_k}$ là phân số tiệm cận thứ $k$ của $x$, ta có
    
    $$
    p_{k+1}q_k − p_kq_{k+1} = (−1)^k.
    $$
    
    Do đó
    
    $$
    x_{k+1} - x_k = \dfrac{(-1)^k}{q_{k+1}q_k}.
    $$

??? note "Chứng minh"
    Theo truy hồi,
    
    $$
    \begin{aligned}
    \det\begin{pmatrix}
    p_{k+1} & p_k \\
    q_{k+1} & q_k 
    \end{pmatrix}
    &=
    \det\begin{pmatrix}
    a_{k+1}p_{k}+p_{k-1} & p_k \\
    a_{k+1}q_{k}+q_{k-1} & q_k 
    \end{pmatrix}
    =
    \det\begin{pmatrix}
    p_{k-1} & p_k\\
    q_{k-1} & q_k
    \end{pmatrix}
    \\
    &=
    -
    \det\begin{pmatrix}
    p_k & p_{k-1}\\
    q_k & q_{k-1}
    \end{pmatrix}
    =
    (-1)^{k+2}
    \det\begin{pmatrix}
    1 & 0\\
    0 & 1
    \end{pmatrix}
    =(-1)^k.
    \end{aligned}
    $$
    
    Chia hai vế cho $q_{k+1}q_k$ thu được công thức cho $x_{k+1}-x_k$.

Do đó các phân số tiệm cận ở vị trí lẻ luôn lớn hơn hai phân số kề, còn vị trí chẵn thì nhỏ hơn hai phân số kề: dãy đan xen.

Nếu chỉ xét các hạng chẵn (hoặc lẻ) thì dãy đơn điệu tăng (hoặc giảm), vì

$$
x_{k+2}-x_k = \dfrac{(-1)^{k+1}}{q_{k+2}q_{k+1}}+\dfrac{(-1)^{k}}{q_{k+1}q_{k}} = \dfrac{(-1)^k(q_{k+2}-q_k)}{q_{k+2}q_{k+1}q_k} = \dfrac{(-1)^ka_{k+2}}{q_{k+2}q_k}
$$

dương khi $k$ chẵn và âm khi $k$ lẻ. Đồng thời, do $q_k$ tăng không chậm hơn Fibonacci, nên hiệu kề nhau tiến về $0$. Vì vậy, dãy chẵn và lẻ cùng hội tụ về một giới hạn. Điều này chứng minh liên phân số đơn giản vô hạn luôn hội tụ. Hình dưới minh họa:

![](./images/golden-ratio-convergents.svg)

???+ abstract "Phân số tiệm cận trên/dưới"
    Với $x$ và phân số tiệm cận $x_k$, nếu $x_k>x$ ($x_k<x$) thì gọi $x_k$ là **tiệm cận trên (dưới)** (upper/lower convergent) của $x$.

Từ trước: tiệm cận trên là các hạng lẻ, tiệm cận dưới là các hạng chẵn.

Từ công thức hiệu, $x$ có thể viết thành chuỗi luân phiên:

$$
x = a_0+\sum_{k=0}^{\infty}\dfrac{(-1)^k}{q_{k+1}q_k}.
$$

Các phân số tiệm cận và phần dư chính là tổng từng phần và phần dư của chuỗi này.

Cũng từ công thức hiệu, ước lượng sai số:

???+ note "Sai số"
    Với $x_k=\dfrac{p_k}{q_k}\neq x$ là phân số tiệm cận thứ $k$ của $x$, ta có
    
    $$
    x_k - x = \dfrac{(-1)^k}{q_k\left(r_{k+1}q_k+q_{k-1}\right)},
    $$
    
    trong đó $r_{k+1}$ là phần dư thứ $k+1$. Suy ra
    
    $$
    \dfrac{1}{2q_{k+1}^2} \le \dfrac{1}{q_k(q_k+q_{k+1})} \le \left|x-\frac{p_k}{q_k}\right| \le \dfrac{1}{q_kq_{k+1}} \le \dfrac{1}{q_k^2}.
    $$

??? note "Chứng minh"
    Vì $x=[a_0,a_1,\cdots,a_k,r_{k+1}]$, và với liên phân số hình thức cũng có công thức hiệu, nên
    
    $$
    x-x_k = \dfrac{(-1)^k}{q_k\left(r_{k+1}q_k+q_{k-1}\right)},
    $$
    
    trong đó $r_{k+1}q_k+q_{k-1}$ là mẫu của phân số tiệm cận thứ $k+1$ của liên phân số hình thức này.
    
    Để suy ra bất đẳng thức, chỉ cần dùng khi $x_k\neq x$ thì
    
    $$
    1\le a_{k+1}\le r_{k+1} \le a_{k+1}+1,
    $$
    
    nên
    
    $$
    q_{k+1}=a_{k+1}q_k+q_{k-1}\le r_{k+1}q_k+q_{k-1} \le q_k+(a_{k+1}q_k+q_{k-1}) = q_k+q_{k+1}.
    $$
    
    Do đó
    
    $$
    \dfrac{1}{q_k(q_k+q_{k+1})} \le \left|x-\frac{p_k}{q_k}\right| = \dfrac{1}{q_k\left(r_{k+1}q_k+q_{k-1}\right)} \le \dfrac{1}{q_kq_{k+1}}.
    $$
    
    Hai biên ngoài cùng suy ra từ $q_k\le q_{k+1}$.

Từ công thức hiệu có hệ quả: mọi phân số tiệm cận đều tối giản.

???+ note "Hệ quả"
    Với mọi $x$, nếu $x_k=\dfrac{p_k}{q_k}$ được cho bởi truy hồi thì $\gcd(p_k,q_k)=1$.

??? note "Chứng minh"
    Áp dụng [định lý Bézout](./bezouts.md) cho công thức hiệu.

Thực ra, có thể giải phương trình Diophantine tuyến tính bằng liên phân số.

???+ example "Giải phương trình Diophantine tuyến tính"
    Cho $A, B, C \in \mathbf Z$. Tìm $x, y \in \mathbf Z$ sao cho $Ax + By = C$.

??? note "Lời giải"
    Bài toán thường giải bằng [Euclid mở rộng](./bezouts.md#两个变量的情形), nhưng cũng có thể dùng liên phân số.
    
    Gọi $\dfrac{A}{B}=[a_0, a_1, \cdots, a_k]$. Ta đã có $p_k q_{k-1} - p_{k-1} q_k = (-1)^{k-1}$. Thay $p_k,q_k$ bằng $A,B$:
    
    $$
    Aq_{k-1} - Bp_{k-1} = (-1)^{k-1} g,
    $$
    
    với $g = \gcd(A, B)$. Nếu $g\mid C$ thì một nghiệm là $x = (-1)^{k-1}\dfrac{C}{g} q_{k-1}$, $y = (-1)^{k}\dfrac{C}{g} p_{k-1}$; ngược lại vô nghiệm.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/diophantine.cpp:dio"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/diophantine.py:dio"
        ```

## Xấp xỉ Diophantine

Một ứng dụng quan trọng của lý thuyết liên phân số là xấp xỉ Diophantine: dùng số hữu tỉ để xấp xỉ số thực. Do tính dày đặc của hữu tỉ, nếu không hạn chế thì sai số có thể tùy ý nhỏ. Vì vậy cần giới hạn, ví dụ chỉ chọn mẫu số nhỏ hơn một ngưỡng. Phần này bàn về xấp xỉ tốt nhất và liên hệ với liên phân số.

### Dùng phân số tiệm cận để xấp xỉ

Từ ước lượng sai số, ta có:

???+ note "Định lý (Dirichlet)"
    Với số vô tỉ $x$, tồn tại vô hạn phân số tối giản $\dfrac{p}{q}$ sao cho
    
    $$
    \left|x-\dfrac{p}{q}\right| < \dfrac{1}{q^2} 
    $$
    
    đúng.

??? note "Chứng minh"
    Với phân số tiệm cận $x_k=\dfrac{p_k}{q_k}$ của số vô tỉ $x$,
    
    $$
    \left|x-\dfrac{p_k}{q_k}\right|\le\frac{1}{q_k^2}.
    $$
    
    Từ chứng minh công thức sai số suy ra với vô tỉ, dấu bằng không xảy ra. Do đó mọi phân số tiệm cận đều thỏa.

Đây là hệ quả của [định lý xấp xỉ Dirichlet](https://en.wikipedia.org/wiki/Dirichlet%27s_approximation_theorem). Hệ số $2$ ở mẫu không thể cải thiện, nhưng hằng số có thể tốt hơn. Định lý Hurwitz cho phép thay bằng $\dfrac{1}{\sqrt{5}q^2}$ và đó là tốt nhất.

???+ note "Định lý Hurwitz"
    Với vô tỉ $x$, tồn tại vô hạn phân số tối giản $\dfrac{p}{q}$ sao cho
    
    $$
    \left|x-\dfrac{p}{q}\right| < \dfrac{1}{\sqrt{5}q^2} 
    $$
    
    và $\sqrt{5}$ không thể thay bằng số thực lớn hơn.

??? note "Chứng minh (Borel)"
    Borel chứng minh: trong ba phân số tiệm cận liên tiếp của $x$, có ít nhất một phân số thỏa điều kiện. Vì có vô hạn phân số tiệm cận, phần đầu định lý đúng.
    
    Phản chứng. Giả sử tồn tại vô tỉ $x$ và ba phân số tiệm cận $x_{k-1},x_k,x_{k+1}$ đều thỏa
    
    $$
    \left|x-\dfrac{p_{k-1}}{q_{k-1}}\right|\ge\dfrac{1}{\sqrt{5}q_{k-1}^2},\ 
    \left|x-\dfrac{p_{k}}{q_{k}}\right|\ge\dfrac{1}{\sqrt{5}q_{k}^2},\ 
    \left|x-\dfrac{p_{k+1}}{q_{k+1}}\right|\ge\dfrac{1}{\sqrt{5}q_{k+1}^2}
    $$
    
    Do các phân số kề nhau nằm hai phía $x$, từ công thức hiệu:
    
    $$
    \dfrac{1}{q_{k-1}q_{k}}=\left|\dfrac{p_{k-1}}{q_{k-1}}-\dfrac{p_{k}}{q_{k}}\right|=\left|x-\dfrac{p_{k-1}}{q_{k-1}}\right|+\left|x-\dfrac{p_{k}}{q_{k}}\right|\ge\dfrac{1}{\sqrt{5}q_{k-1}^2}+\dfrac{1}{\sqrt{5}q_{k}^2}.
    $$
    
    Viết theo tỉ số $\dfrac{q_k}{q_{k-1}}$:
    
    $$
    \dfrac{q_k}{q_{k-1}}+\dfrac{q_{k-1}}{q_k}\le\sqrt 5.
    $$
    
    Vế trái là hữu tỉ, vế phải vô tỉ nên không thể bằng. Vì $q_k\ge q_{k-1}$,
    
    $$
    1\le \dfrac{q_{k}}{q_{k-1}} < \dfrac{\sqrt{5}+1}{2}.
    $$
    
    Tương tự,
    
    $$
    1\le \dfrac{q_{k+1}}{q_{k}} < \dfrac{\sqrt{5}+1}{2}.
    $$
    
    Từ truy hồi:
    
    $$
    a_{k+1} = \frac{q_{k+1}}{q_k}-\frac{q_{k-1}}{q_k} < \dfrac{\sqrt{5}+1}{2}-\dfrac{\sqrt{5}-1}{2} = 1,
    $$
    
    mâu thuẫn với định nghĩa liên phân số đơn giản. Do đó kết luận của Borel đúng.
    
    Để chứng minh cận là tốt nhất, chỉ cần tìm $x$ sao cho với mọi $C>\sqrt{5}$, chỉ có hữu hạn phân số tối giản $\dfrac{p}{q}$ thỏa
    
    $$
    \left|x-\dfrac{p}{q}\right| < \dfrac{1}{Cq^2} 
    $$
    
    Ta sẽ chứng minh $x=\phi=\dfrac{\sqrt{5}+1}{2}$ là như vậy.[^sqrt5]
    
    Gọi $\phi'=\dfrac{-\sqrt{5}+1}{2}$ là nghiệm liên hợp. Khi đó
    
    $$
    x^2-x-1 = (x-\phi)(x-\phi').
    $$
    
    Thay $x=\dfrac{p}{q}$:
    
    $$
    \dfrac{1}{q^2}\le\frac{|p^2-pq-q^2|}{q^2}=\left|\dfrac{p}{q}-\phi\right|\left|\dfrac{p}{q}-\phi'\right|\le\left|\dfrac{p}{q}-\phi\right|\left(\left|\dfrac{p}{q}-\phi\right|+|\phi-\phi'|\right)<\dfrac{1}{Cq^2}\left(\dfrac{1}{Cq^2}+\sqrt{5}\right).
    $$
    
    Với $C>\sqrt{5}$, suy ra $q<\sqrt{C(C-\sqrt{5})}$, nên không thể có vô hạn nghiệm.

Các chứng minh cho thấy phân số tiệm cận cho xấp xỉ rất tốt, nhưng chưa chắc là tối ưu. Để bàn “tốt nhất”, cần định nghĩa thước đo, thường có hai cách.

???+ warning "Có thể có trường hợp kết luận về xấp xỉ tốt nhất không đúng"
    Hai mục sau nêu một số kết quả về xấp xỉ tốt nhất. Các kết quả này có thể không đúng ở vài trường hợp đặc biệt. Ví dụ, với $x=n+\dfrac12$ ($n\in\mathbf Z$), liên phân số có thể là $[n,1,1]$. Khi đó $x_0=n$ và $x_1=n+1$ có mẫu số đều là $1$ và cùng khoảng cách đến $x$, nên đều không phải xấp xỉ tốt nhất. Khi đọc, mặc định đã loại trừ các trường hợp như vậy; nếu chỉ quan tâm vô tỉ hoặc bỏ qua vài phân số cuối, có thể bỏ qua phức tạp này.

### Xấp xỉ tốt nhất loại 1: phân số trung gian

Loại 1 đo bằng

$$
\left|x-\dfrac{p}{q}\right|
$$

???+ abstract "Xấp xỉ tốt nhất loại 1"
    Với số thực $x$ và hữu tỉ $\dfrac{p}{q}$, nếu với mọi $\dfrac{p'}{q'}\neq \dfrac{p}{q}$ và $0<q'\le q$ đều có
    
    $$
    \left|x-\dfrac{p}{q}\right|<\left|x-\dfrac{p'}{q'}\right|,
    $$
    
    thì $\dfrac{p}{q}$ là **xấp xỉ tốt nhất loại 1** (best approximation of the first kind) của $x$.

Xấp xỉ loại 1 không nhất thiết là phân số tiệm cận mà thuộc lớp rộng hơn.

???+ abstract "Phân số trung gian"
    Với phân số tiệm cận $x_{k+1}=[a_0,a_1,\cdots,a_k,a_{k+1}]$ của $x$, và số nguyên $t$ thỏa $0\le t\le a_{k+1}$[^semi-range], thì
    
    $$
    x_{k,t}=[a_0,a_1,\cdots,a_{k},t]
    $$
    
    gọi là **phân số trung gian** (intermediate fraction), **bán hội tụ** (semiconvergent) hay **phân số tiệm cận thứ cấp** (secondary convergent).[^^semiconvergent]
    
    Tương tự, phân số trung gian lớn hơn (nhỏ hơn) $x$ gọi là **trung gian trên/dưới** (upper/lower semiconvergent).

Theo truy hồi,

$$
x_{k,t} = \frac{tp_{k}+p_{k-1}}{tq_{k}+q_{k-1}}.
$$

Nó luôn tối giản và nằm giữa $x_{k-1}$ và $x_{k+1}$. Khi $t$ tăng, nó tiến gần $x_{k+1}$ (giả sử $k$ chẵn):

$$
x_{k-1} = x_{k,0} < x_{k,1} < x_{k,2} < \cdots < x_{k,a_{k+1}} = x_{k+1}.
$$

Vì tử/mẫu của phân số tiệm cận tăng dần, nên tử/mẫu của $x_{k,t}$ (với $t\neq0$) nằm giữa $x_k$ và $x_{k+1}$. Sắp theo mẫu số, các phân số trung gian nằm giữa hai phân số tiệm cận kề.

Mọi xấp xỉ loại 1 đều là phân số trung gian, nhưng không phải mọi phân số trung gian đều là xấp xỉ loại 1.

???+ note "Định lý"
    Mọi xấp xỉ tốt nhất loại 1 đều là phân số trung gian.

??? note "Chứng minh"
    Vì $a_0\le x\le a_0+1$, nên xấp xỉ loại 1 nằm giữa $x_{1,0}=a_0$ và $x_{0,1}=a_0+1$. Sắp các phân số trung gian:
    
    $$
    x_{1,0}<x_{1,1}< \cdots < x_{1,a_2}=x_{3,0}<\ldots<x<\ldots<x_{2,0}=x_{0,a_1}<\cdots<x_{0,1}.
    $$
    
    Các phân số trung gian cùng bậc xuất hiện liên tiếp, và giữa hai bậc không có khoảng trống. Do đó mọi hữu tỉ $\dfrac{p}{q}$ giữa $a_0$ và $a_0+1$ đều nằm giữa hai phân số trung gian liên tiếp $x_{k,t}$ và $x_{k,t+1}$. Giả sử nó không phải trung gian và nhỏ hơn $x$, tức
    
    $$
    x_{k,t}<\dfrac{p}{q}<x_{k,t+1}<x.
    $$
    
    Khi đó,
    
    $$
    \left|x_{k,t}-\dfrac{p}{q}\right| \le \left|x_{k,t}-x_{k,t+1}\right| = \dfrac{1}{((t+1)q_k+q_{k-1})(tq_k+q_{k-1})}.
    $$
    
    Mặt khác,
    
    $$
    \left|x_{k,t}-\dfrac{p}{q}\right| = \dfrac{|q(tp_k+p_{k-1})-p((t+1)q_k+q_{k-1})|}{q(tq_k+q_{k-1})}\ge\dfrac{1}{q(tq_k+q_{k-1})}.
    $$
    
    Suy ra
    
    $$
    q>(t+1)q_k+q_{k-1}.
    $$
    
    Nghĩa là mẫu số của $\dfrac{p}{q}$ lớn hơn mẫu của $x_{k,t+1}$, nhưng nó không xấp xỉ tốt hơn:
    
    $$
    \left|x-\dfrac{p}{q}\right|>\left|x-x_{k,t+1}\right|.
    $$
    
    Do đó $\dfrac{p}{q}$ không thể là xấp xỉ loại 1.

Ngược lại, không phải mọi phân số trung gian đều là xấp xỉ loại 1, nhưng có điều kiện:

???+ note "Định lý"
    Mọi phân số tiệm cận đều là xấp xỉ loại 1. Ngoài ra, với $0<t<a_{k+1}$, phân số trung gian $x_{k,t}$ là xấp xỉ loại 1 khi và chỉ khi $t>\dfrac{a_{k+1}}{2}$, hoặc $t=\dfrac{a_{k+1}}{2}$ và $r_{k+2}>\dfrac{q_k}{q_{k-1}}$.

??? note "Chứng minh"
    Ta sẽ thấy mọi phân số tiệm cận đều là xấp xỉ loại 2, nên là loại 1. Còn lại xét phân số trung gian không phải tiệm cận.
    
    Trước hết, $x_{k,t}$ có mẫu nằm giữa $x_k$ và $x_{k+1}$, tăng theo $t$, và tiến gần $x_{k+1}$ nên gần $x$ hơn. Với $x_{k,t}<x$:
    
    $$
    x_{k-1} < x_{k,t} < x_{k+1} < x < x_{k}.
    $$
    
    Sai số
    
    $$
    |x_{k,t}-x|\ge |x_{k,t}-x_{k+1}|=\left|\dfrac{p}{q}-\dfrac{p_{k+1}}{q_{k+1}}\right|=\dfrac{|pq_{k+1}-p_{k+1}q|}{qq_{k+1}}\ge\dfrac{1}{qq_{k+1}},
    $$
    
    nên
    
    $$
    |qx_{k,t}-p| \ge \dfrac{1}{q_{k+1}} \ge |q_kx_k-p_k|.
    $$
    
    Do đó $x_{k,t}$ không thể tốt hơn $x_k$ dù mẫu lớn hơn, nên không thể là loại 2.
    
    Phần 2: Với phân số tiệm cận $x_k=\dfrac{p_k}{q_k}$, cần chứng minh với mọi $\dfrac{p}{q}$ có $q\le q_k$ thì $|q_kx-p_k|<|qx-p|$. Giả sử $k>0$ (loại trừ trường hợp nửa số lẻ).
    
    Từ ước lượng sai số,
    
    $$
    |q_{k-1} x-p_{k-1}| \ge \frac{1}{q_{k-1}+q_{k}} \ge \dfrac{1}{q_{k+1}}\ge |q_kx-p_k|.
    $$
    
    Dấu bằng xảy ra chỉ khi $a_{k+1}=1$ và là hạng cuối; bỏ trường hợp này thì $x_{k-1}$ kém hơn $x_k$.
    
    Lấy $\dfrac{p}{q}\neq x_k$ với $0<q\le q_k$. Từ $p_{k}q_{k-1} − p_{k-1}q_{k} = (−1)^{k-1}$, theo Cramer hệ
    
    $$
    \begin{cases}
    \lambda p_k+\mu p_{k-1} = p,\\
    \lambda q_k+\mu q_{k-1} = q
    \end{cases}
    $$
    
    có nghiệm nguyên duy nhất $(\lambda,\mu)$. Nếu $\lambda\mu>0$ thì $q>|\lambda|q_k\ge q_k$, mâu thuẫn. Vậy $\lambda\mu\le0$, tức $\lambda$ và $\mu$ trái dấu. Do $q_{k-1}x-p_{k-1}$ và $q_kx-p_k$ cũng trái dấu, suy ra $\lambda(q_{k-1}x-p_{k-1})$ và $\mu(q_kx-p_k)$ cùng dấu, nên
    
    $$
    |qx-p|=|\lambda||q_kx-p_k|+|\mu||q_{k-1}x-p_{k-1}|>|q_{k}x-p_{k}|.
    $$
    
    Dấu “>” là chặt vì $x_{k-1}$ kém $x_k$ và $\dfrac{p}{q}\neq x_k$. Vậy $x_k$ là xấp xỉ loại 2.

Tính chất này cho thấy phân số tiệm cận là xấp xỉ rất tốt.

### Nhận biết phân số tiệm cận

Xấp xỉ loại 2 cho điều kiện cần và đủ để một phân số là tiệm cận. Legendre đưa ra tiêu chuẩn theo mức độ xấp xỉ tuyệt đối. Bản gốc của tiêu chuẩn là cần và đủ nhưng khó dùng; ở đây nêu dạng rút gọn và cho thấy không bỏ sót quá nhiều.

???+ note "Định lý (Legendre)"
    Với số thực $x$ và phân số $\dfrac{p}{q}$, nếu
    
    $$
    \left|x−\dfrac{p}{q}\right|<\dfrac{1}{2q^2}
    $$
    
    thì $\dfrac{p}{q}$ là phân số tiệm cận của $x$.

??? note "Chứng minh"
    Chọn $\epsilon\in\{-1,1\}$ và $\theta\in(0,1/2)$ sao cho
    
    $$
    x−\dfrac{p}{q} = \dfrac{\epsilon\theta}{q^2}.
    $$
    
    Viết $\dfrac{p}{q}$ dưới dạng liên phân số $[a_0,a_1,\cdots,a_n]$. Vì có hai biểu diễn (độ dài lệch 1), chọn sao cho $(-1)^n=\epsilon$, và gọi các phân số tiệm cận là $\dfrac{p_k}{q_k}$. Tồn tại $\omega$ sao cho
    
    $$
    x = \dfrac{\omega p_n+p_{n-1}}{\omega q_n+q_{n-1}}.
    $$
    
    Khi đó
    
    $$
    \dfrac{\epsilon\theta}{q^2} = x−\dfrac{p}{q} = x-\dfrac{p_n}{q_n} = \dfrac{p_{n-1}q_n-p_nq_{n-1}}{(\omega q_n+q_{n-1})q_n} = \dfrac{(-1)^n}{(\omega q_n+q_{n-1})q_n}.
    $$
    
    Suy ra
    
    $$
    \theta = \dfrac{q_n}{\omega q_n+q_{n-1}},
    $$
    
    nên
    
    $$
    \omega=\dfrac{1}{\theta}-\dfrac{q_{n-1}}{q_n}>1.
    $$
    
    Viết $\omega=[b_0,b_1,\cdots]$ thì
    
    $$
    x = \dfrac{\omega p_n+p_{n-1}}{\omega q_n+q_{n-1}} = [a_0,a_1,\cdots,a_n,\omega] = [a_0,a_1,\cdots,a_n,b_0,b_1,\cdots].
    $$
    
    Đây là liên phân số đơn giản hợp lệ, nên $\dfrac{p}{q}$ là phân số tiệm cận.
    
    Chứng minh cũng cho thấy điều kiện cần và đủ là $\omega>1$, đúng với dạng gốc của tiêu chuẩn Legendre.

Tiêu chuẩn này cho thấy nếu xấp xỉ đủ tốt thì chắc chắn là tiệm cận. Định lý tiếp theo cho thấy “tốt đủ” xảy ra ít nhất ở một nửa các phân số tiệm cận.

???+ note "Định lý (Valhen)"
    Trong hai phân số tiệm cận liên tiếp của $x$, ít nhất một phân số thỏa
    
    $$
    \left|x−\dfrac{p}{q}\right|<\dfrac{1}{2q^2}.
    $$

??? note "Chứng minh"
    Giả sử tồn tại $x$ có hai phân số tiệm cận kề $x_{k-1}$ và $x_k$ đều thỏa
    
    $$
    \left|x-\dfrac{p_k}{q_k}\right|\ge \dfrac{1}{2q_{k}^2},\ \left|x-\dfrac{p_{k+1}}{q_{k+1}}\right|\ge \dfrac{1}{2q_{k+1}^2}.
    $$
    
    Vì $x$ nằm giữa $x_{k-1}$ và $x_k$,
    
    $$
    \dfrac{1}{2q_{k}^2} + \dfrac{1}{2q_{k+1}^2} \le \left|x-\dfrac{p_k}{q_k}\right| + \left|x-\dfrac{p_{k+1}}{q_{k+1}}\right| = \left|\dfrac{p_k}{q_k}-\dfrac{p_{k+1}}{q_{k+1}}\right| = \dfrac{1}{q_kq_{k+1}}.
    $$
    
    Suy ra $q_k=q_{k+1}$, do đó $k=0$ và $a_1=1$. Khi ấy $x_0=a_0$, $x_1=a_0+1$, tức phản ví dụ duy nhất là nửa số lẻ; như đã nói, ta bỏ qua trường hợp này.

## Diễn giải hình học

Lý thuyết liên phân số có diễn giải hình học đẹp.

![](./images/continued-convergents-geometry.svg)

Với $\xi>0$, đường thẳng $y=\xi x$ chia các điểm nguyên (lattice point) trong góc phần tư thứ nhất (kể cả trên trục, trừ gốc) thành hai phần. Nếu $\xi$ hữu tỉ, các điểm trên đường thẳng được tính vào cả hai phía. Lấy bao lồi của hai phần: các phân số tiệm cận lẻ là đỉnh của bao lồi phía trên, các tiệm cận chẵn là đỉnh phía dưới. Các điểm nguyên trên đoạn nối hai đỉnh liên tiếp chính là phân số trung gian. Hình minh họa $\xi=\dfrac{9}{7}$.

Nhiều kết luận trước có diễn giải hình học:

??? note "Diễn giải hình học"
    -   Mỗi phân số $\nu=\dfrac{p}{q}$ ứng với điểm nguyên $\vec\nu=(q,p)$, giá trị phân số tương ứng với độ dốc của đoạn nối gốc.
    -   Vector hướng của $y=\xi x$ là $\vec\xi=(1,\xi)$. Dùng [tích có hướng](../linear-algebra/product.md#二维向量的情形) $(x_1,y_1)\times(x_2,y_2)=x_1y_2-x_2y_1$, dấu của $\vec\xi\times\vec\nu=p-q\xi$ cho biết điểm ở trên hay dưới đường. Do đó điểm phía trên tương ứng phân số $\ge\xi$, phía dưới tương ứng $\le\xi$. Giá trị $|\vec\xi\times\vec\nu|$ tỉ lệ với khoảng cách từ $\vec\nu$ đến đường:
    
        $$
        \dfrac{|p-qx|}{\sqrt{1+\xi^2}},
        $$
    
        tương ứng mức độ xấp xỉ của $\nu$ với $\xi$.
    -   Gọi điểm ứng với phân số tiệm cận $\xi_k=\dfrac{p_k}{q_k}$ là $\vec\xi_k=(p_k,q_k)$, thì truy hồi viết thành
    
        $$
        \vec\xi_k = a_k\vec\xi_{k-1} + \vec\xi_{k-2}.
        $$
    
        Điểm đầu là $\xi_{-2} = (1,0)$ và $\xi_{-1} = (0,1)$.
    -   Với $0\le t\le a_k$, điểm
    
        $$
        \vec\xi_{k-1,t} = t\vec\xi_{k-1} + \vec\xi_{k-2}
        $$
    
        nằm trên đoạn nối $\vec\xi_{k-2}$ và $\vec\xi_k$, tương ứng phân số trung gian $\xi_{k-1,t}$.
    -   Có thể dựng mọi phân số tiệm cận và trung gian bằng hình học: bắt đầu từ $\vec\xi_{-2}=(1,0)$ và $\vec\xi_{-1}=(0,1)$ ở hai phía đường $y=\xi x$ (tức $\vec\xi\times\vec\xi_{-2}$ và $\vec\xi\times\vec\xi_{-1}$ trái dấu). Cộng $\vec\xi_{-1}$ vào $\vec\xi_{-2}$ cho đến khi sắp vượt qua đường thì dừng, được $\vec\xi_0$; vẫn ở phía đối diện với $\vec\xi_{-1}$. Tiếp tục cộng $\vec\xi_0$ vào $\vec\xi_{-1}$ để được $\vec\xi_1$, v.v. Quá trình tiếp tục vô hạn, trừ khi một $\vec\xi_n$ nằm đúng trên đường, tương ứng $\xi=\dfrac{p_n}{q_n}$ hữu tỉ. Boris Delaunay gọi đây là thuật toán “kéo mũi” (nose-streching)[^nose-streching].
    -   Muốn nhanh chóng biết số lần cộng, dùng tích có hướng. Vì $\vec\xi\times\vec\xi_{k-1}$ và $\vec\xi\times\vec\xi_{k-2}$ trái dấu, với $\vec\xi_{k-1,t}=t\vec\xi_{k-1}+\vec\xi_{k-2}$, điều kiện không vượt qua đường là dấu của $\vec\xi\times\vec\xi_{k-1,t}$ không đổi. Trước khi đổi dấu, trị tuyệt đối giảm dần. Đặt
    
        $$
        r_{k} = \left|\dfrac{\vec\xi\times\vec\xi_{k-2}}{\vec\xi\times\vec\xi_{k-1}}\right| = -\dfrac{\vec\xi\times\vec\xi_{k-2}}{\vec\xi\times\vec\xi_{k-1}}.
        $$
    
        Số lần tối đa là
    
        $$
        a_{k} = \lfloor r_{k}\rfloor = \left\lfloor\left|\dfrac{q_{k-1}\xi-p_{k-1}}{q_{k-2}\xi-p_{k-2}}\right|\right\rfloor.
        $$
    
        Đây là hạng thứ $k$ của liên phân số. Và $r_k$ là phần dư, thỏa
    
        $$
        r_k = -\dfrac{q_{k-1}\xi-p_{k-1}}{q_{k-2}\xi-p_{k-2}} \iff \xi = \dfrac{p_{k-1}r_k + p_{k-2}}{q_{k-1}r_k+q_{k-2}}.
        $$
    
        Đây chính là $\xi = [a_0,a_1,\cdots,a_{k-1},r_k]$.
    -   Mỗi lần cộng, bước giảm của $\vec\xi\times\vec\xi_{k-1,t}$ là $|\vec\xi\times\vec\xi_{k-1}|$, nên $|\vec\xi\times\vec\xi_k|<|\vec\xi\times\vec\xi_{k-1}|$. Nghĩa là mức xấp xỉ (theo $|qx-p|$) tốt dần khi $k$ tăng.
    -   Dùng tính chất tích có hướng:
    
        $$
        \vec\xi_{k}\times\vec\xi_{k+1} = \vec\xi_{k}\times(a_{k+1}\vec\xi_k+\vec\xi_{k-1}) = \vec\xi_{k}\times\vec\xi_{k-1} = -\vec\xi_{k-1}\times\vec\xi_{k}.
        $$
    
        Suy ra
    
        $$
        \vec\xi_{k}\times\vec\xi_{k+1} = (-1)^{k+2}\vec\xi_{k-2}\times\vec\xi_{k-1} = (-1)^{k},
        $$
    
        tương đương $p_{k+1}q_k-p_kq_{k+1}=(-1)^k$.
    -   Diện tích giữa hai bao lồi có thể chia thành các tam giác với đỉnh $\vec\xi_{k-2}$, $\vec\xi_k$ và $\vec 0$; mỗi tam giác có diện tích
    
        $$
        \dfrac12|\vec\xi_{k-2}\times\vec\xi_k| = \dfrac12|\vec\xi_{k-2}\times(a_k\vec\xi_{k-1}+\vec\xi_{k-2})| = \dfrac{a_k}{2}.
        $$
    
        Theo [định lý Pick](../../geometry/pick.md), nếu $I$ là số điểm nguyên trong tam giác và $B$ là số điểm nguyên trên biên thì
    
        $$
        I + \dfrac{B}{2} - 1 = \dfrac{a_k}{2}.
        $$
    
        Biên đã có $\{\vec 0\}\cup\{\vec\xi_{k-1,t}:0\le t\le a_k\}$ tổng cộng $a_k+2$ điểm nguyên. Suy ra $I=0$, $B=a_k+2$. Tức là không có điểm nguyên khác trên biên/ trong tam giác; điều này tương đương $q_k$ và $p_k$ tối giản, các phân số trung gian là toàn bộ điểm nguyên trên cạnh nối $\vec\xi_{k-2}$ và $\vec\xi_k$, và mọi điểm nguyên trong góc phần tư nằm trong hai bao lồi.

Hai bao lồi này gọi là đa giác Klein. Trong không gian cao hơn có [đa diện Klein](https://en.wikipedia.org/wiki/Klein_polyhedron), giúp mở rộng khái niệm liên phân số.

## Cây liên phân số

Mục chính: [Cây Stern–Brocot và dãy Farey](./stern-brocot.md)

Cây Stern–Brocot lưu tất cả phân số trong $[0,\infty]$ như một [cây BST](../../ds/bst.md). Liên phân số hữu hạn mã hóa đường đi từ gốc đến phân số đó. Nghĩa là liên phân số $[a_0,a_1,\cdots,a_{n-1},1]$ cho biết từ gốc $\dfrac{1}{1}$ đi sang phải $a_0$ lần, rồi sang trái $a_1$ lần, xen kẽ, cho đến khi đi cùng hướng $a_{n-1}$ lần. Lưu ý phải dùng biểu diễn kết thúc bằng $1$.

Diễn giải này cho phép so sánh liên phân số.

???+ example "So sánh độ lớn liên phân số"
    Cho $\alpha=[\alpha_0,\alpha_1,\cdots,\alpha_n]$ và $\beta=[\beta_0,\beta_1,\cdots,\beta_m]$, so sánh độ lớn.

??? note "Lời giải"
    Chuyển cả hai về dạng kết thúc bằng $1$. Giả sử đã ở dạng này, tức $\alpha_n=\beta_m=1$. Vì vị trí chẵn (tính từ $0$) là bước sang phải, vị trí lẻ là bước sang trái, nên $\alpha<\beta$ khi và chỉ khi so theo [từ điển](../../string/basic.md#字典序) ta có
    
    $$
    (\alpha_0,-\alpha_1,\alpha_2,\cdots,(-1)^{n-1}\alpha_{n-1},0,\cdots)<(\beta_0,-\beta_1,\beta_2,\cdots,(-1)^{m-1}\beta_{m-1},0,\cdots).
    $$
    
    Tức là luân phiên dấu, bỏ hạng cuối $1$, và bù $0$ cho đủ độ dài.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/compare.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/compare.py:core"
        ```

???+ example "Điểm trong tốt nhất"
    Với $\dfrac{0}{1}\le\dfrac{p_0}{q_0}<\dfrac{p_1}{q_1}\le\dfrac{1}{0}$, tìm hữu tỉ $\dfrac{p}{q}$ sao cho $\dfrac{p_0}{q_0}<\dfrac{p}{q}<\dfrac{p_1}{q_1}$ và $(q,p)$ nhỏ nhất.

??? note "Lời giải"
    Cây Stern–Brocot vừa là BST các phân số trong $[0,\infty]$ vừa là [cây Cartesian](../../ds/cartesian-tree.md) theo cặp $(q,p)$, nên bài toán gần như là LCA của hai điểm. Nhưng LCA chỉ xử lý đoạn đóng và có thể trùng biên, nên ta xét $\dfrac{p_0}{q_0}+\varepsilon$ và $\dfrac{p_1}{q_1}-\varepsilon$, rồi lấy LCA. Khi đã có đường đi từ gốc, LCA là tiền tố chung dài nhất.
    
    Để dựng $x\pm\varepsilon$, chỉ cần từ $x$ đi phải (trái) 1 lần rồi đi trái (phải) vô hạn lần. Với $x=[a_0,a_1,\cdots,a_{n-1},1]$, ta có $x\pm\varepsilon$ là $[a_0,a_1,\cdots,a_{n-1}+1,\infty]$ và $[a_0,a_1,\cdots,a_{n-1},1,\infty]$, lấy giá trị lớn/nhỏ tương ứng.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/inner-point.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/inner-point.py:core"
        ```

???+ example "[GCJ 2019, Round 2 - New Elements: Part 2](https://github.com/google/coding-competitions-archive/blob/main/codejam/2019/round_2/new_elements_part_2/statement.pdf)"
    Cho $N$ cặp số nguyên dương $(C_i,J_i)$, tìm cặp dương $(x,y)$ sao cho $\{C_ix+J_iy\}$ tăng строго. Trong các đáp án, chọn cặp nhỏ nhất theo từ điển.

??? note "Lời giải"
    Đặt $A_i=C_i-C_{i-1}$, $B_i=J_i-J_{i-1}$. Bài toán thành tìm $(x,y)$ sao cho mọi $A_ix+B_iy$ là số nguyên. Có bốn trường hợp:
    
    1.  $A_i,B_i>0$ bỏ qua vì đã giả sử $(x,y)>0$;
    2.  $A_i,B_i\le 0$ => “IMPOSSIBLE”;
    3.  $A_i>0,B_i\le 0$ tương đương ràng buộc $\dfrac{y}{x}<\dfrac{A_i}{-B_i}$;
    4.  $A_i\le 0,B_i>0$ tương đương ràng buộc $\dfrac{y}{x}>\dfrac{-A_i}{B_i}$.
    
    Lấy $\dfrac{p_0}{q_0}$ là giá trị lớn nhất của $\dfrac{-A_i}{B_i}$ (trường hợp 4), và $\dfrac{p_1}{q_1}$ là giá trị nhỏ nhất của $\dfrac{A_i}{-B_i}$ (trường hợp 3). Bài toán trở thành tìm $(q,p)$ nhỏ nhất theo từ điển sao cho $\dfrac{p_0}{q_0}<\dfrac{p}{q}<\dfrac{p_1}{q_1}$.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/gcj-2019.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/gcj-2019.py:core"
        ```

Muốn tìm hiểu thêm về Stern–Brocot, xem mục chính.

## Biến đổi phân thức tuyến tính

Một khái niệm quan trọng khác liên quan liên phân số là biến đổi phân thức tuyến tính.

???+ abstract "Biến đổi phân thức tuyến tính"
    **Biến đổi phân thức tuyến tính** (fractional linear transformation) là hàm $L:\mathbf R\rightarrow\mathbf R$ dạng
    
    $$
    L(x) = \dfrac{ax+b}{cx+d},
    $$
    
    với $a,b,c,d\in\mathbf R$ và $ad-bc\neq 0$.

???+ info "Về điều kiện $ad-bc\neq 0$"
    Dễ thấy nếu $ad-bc=0$ thì hàm có thể không xác định hoặc là hằng số.

Các tính chất:

???+ note "Tính chất của biến đổi phân thức tuyến tính"
    Với $L_1,L_2,L_3$ và ma trận hệ số
    
    $$
    M_i=\begin{pmatrix}a_i & b_i \\ c_i & d_i\end{pmatrix}
    $$
    
    thì[^pgl2]
    
    1.  Hợp và nghịch đảo của biến đổi phân thức tuyến tính vẫn là biến đổi phân thức tuyến tính; các phép biến đổi tạo thành một [nhóm](../algebra/basic.md#群);
    2.  Nhân tất cả hệ số với hằng khác 0 không đổi hàm: $M_2=\lambda M_1\Rightarrow L_2=L_1$;
    3.  Ma trận hệ số của phép hợp là tích ma trận: $M_1M_2=M_3\Rightarrow L_1\circ L_2=L_3$;
    4.  Ma trận hệ số của nghịch đảo là nghịch đảo ma trận: $M_1^{-1}=M_2\Rightarrow L_1^{-1}=L_2$.

??? note "Chứng minh"
    Chỉ nêu dạng hợp và nghịch đảo.
    
    Hợp $L_1\circ L_2$:
    
    $$
    \begin{aligned}
    L_1\circ L_2 &= \dfrac{a_1\dfrac{a_2x+b_2}{c_2x+d_2}+b_1}{c_1\dfrac{a_2x+b_2}{c_2x+d_2}+d_1} = \dfrac{(a_1a_2+b_1c_2)x+(a_1b_2+b_1d_2)}{(c_1a_2+d_1c_2)x+(c_1b_2+d_1d_2)}.
    \end{aligned}
    $$
    
    Nghịch đảo:
    
    $$
    y = L_1(x) = \dfrac{a_1x+b_1}{c_1x+d_1} \iff x = L_1^{-1}(y) = \dfrac{d_1y - b_1}{-c_1y + a_1}.
    $$

Liên phân số hữu hạn $[a_0,a_1,\cdots,a_n]$ là hợp của các biến đổi phân thức tuyến tính. Đặt

$$
L_i(x) = \dfrac{a_ix+1}{x} = [a_i,x]. 
$$

Khi đó

$$
[a_0,a_1,\cdots,a_n] = L_0\circ L_1\circ \cdots L_n(\infty).
$$

Với $L(x)=\dfrac{ax+b}{cx+d}$, giá trị tại $x=\infty$ là $\dfrac{a}{c}$ (giới hạn khi $x\to\pm\infty$).

Với liên phân số tổng quát $x=[a_0,\cdots,a_k,r_{k+1}]$,

$$
x = L_0\circ L_1\circ \cdots L_k(r_{k+1}) = \dfrac{p_kr_{k+1}+p_{k-1}}{q_kr_{k+1}+q_{k-1}}.
$$

Đây cũng là dạng của $L_0\circ\cdots\circ L_k$.

Có thể kiểm tra trực tiếp. Ban đầu

$$
x=\dfrac{x+0}{0x+1}=\dfrac{p_{-1}x+p_{-2}}{q_{-1}x+q_{-2}}.
$$

Nếu $L_0\circ\cdots\circ L_{k-1}$ có dạng

$$
\dfrac{p_{k-1}x+p_{k-2}}{q_{k-1}x+q_{k-2}},
$$

thì

$$
L_0\circ\cdots\circ L_{k-1}\circ L_k = \dfrac{(p_{k-1}a_k+p_{k-2})x+p_{k-1}}{(q_{k-1}a_k+q_{k-2})x+q_{k-1}} = \dfrac{p_kx+p_{k-1}}{q_kx+q_{k-1}}.
$$

Suy ra bằng quy nạp.

???+ example "[DMOPC '19 Contest 7 P4 - Bob and Continued Fractions](https://dmoj.ca/problem/dmopc19c7p4)"
    Cho mảng $a_1,\cdots,a_n$ và $m$ truy vấn, mỗi truy vấn cho $l\le r$, tính $[a_l,\cdots,a_r]$.

??? note "Lời giải"
    Hiểu liên phân số là hợp các biến đổi phân thức tuyến tính tại $x=\infty$. Cần truy vấn hợp của một đoạn. Vì mỗi phép có nghịch đảo, có thể tiền xử lý tiền tố rồi truy vấn bằng hiệu (độ phức tạp $O(n+m)$); nếu có cập nhật thì dùng cây đoạn.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/flt-presum.cpp"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/flt-presum.py"
        ```

### Bốn phép toán trên liên phân số

Dùng biến đổi phân thức tuyến tính có thể thực hiện bốn phép toán trên liên phân số; thuật toán do Gosper đề xuất.

Cốt lõi là tính biến đổi phân thức tuyến tính của liên phân số. Dù phần này lấy ví dụ liên phân số hữu hạn, thuật toán vẫn áp dụng cho vô hạn vì mỗi lần chỉ cần đọc hữu hạn hạng, và có thể tính đến độ chính xác tùy ý. Kết hợp thuật toán so sánh liên phân số, có thể so sánh số thực với độ chính xác bất kỳ.

???+ example "Biến đổi phân thức tuyến tính của liên phân số"
    Cho $L(x)=\dfrac{ax+b}{cx+d}$ và $\alpha=[\alpha_0,\alpha_1,\cdots,\alpha_n]$, tìm biểu diễn liên phân số của $\beta=L(\alpha)$.

??? note "Lời giải"
    Ý tưởng là xác định lần lượt $\beta_i$. Đặt
    
    $$
    L_\gamma(x) = \gamma+\dfrac{1}{x} = \dfrac{\gamma x+1}{x}.
    $$
    
    Vì
    
    $$
    L(\alpha) = L\circ L_{\alpha_0}\circ L_{\alpha_1}\circ \cdots \circ L_{\alpha_n}(\infty),
    $$
    
    ta có thể hợp dần các $L_{\alpha_k}$. Nhưng để lấy liên phân số của $L(\alpha)$, không cần tính giá trị cuối rồi chia; có thể xác định dần các $\beta_i$ ngay trong quá trình hợp.
    
    Giả sử hiện tại
    
    $$
    L\circ L_{\alpha_0}\circ \cdots \circ L_{\alpha_k}(x) = \dfrac{a_kx+b_k}{c_kx+d_k}
    $$
    
    và $c_k,d_k$ cùng dấu. Khi đó hàm đơn điệu trên $[0,\infty]$ và giá trị nằm giữa $\dfrac{a_k}{c_k}$ và $\dfrac{b_k}{d_k}$. Nếu
    
    $$
    \left\lfloor\dfrac{a_k}{c_k}\right\rfloor = \left\lfloor\dfrac{b_k}{d_k}\right\rfloor,
    $$
    
    thì đó là phần nguyên $\beta_0$. Lúc này trái hợp với $L_{\beta_0}^{-1}$ và tiếp tục thêm $L_{\alpha_{k+1}},L_{\alpha_{k+2}},\cdots$ để xác định $\beta_1$, v.v.
    
    Điều kiện $c,d$ cùng dấu đảm bảo điểm gián đoạn không nằm trong $[0,\infty]$. Với liên phân số đơn giản, ta có thể chứng minh sau hữu hạn bước thì $c,d$ cùng dấu và về sau luôn vậy.
    
    Cài đặt: giữ ma trận $\begin{pmatrix}a&b\\c&d\end{pmatrix}$, kiểm tra $c,d$ cùng dấu và $\dfrac{a}{c},\dfrac{b}{d}$ có cùng phần nguyên. Khi phải hợp phải với $L_{\alpha_i}$, cập nhật thành $\begin{pmatrix}a\alpha_i+b&a\\ c\alpha_i+d&c\end{pmatrix}$. Nếu phần nguyên chung là $\beta_j$, thêm $\beta_j$ vào kết quả và trái hợp với $L_{\beta_j}^{-1}$, tương đương $\begin{pmatrix}c&d\\ a\bmod c & b \bmod d \end{pmatrix}$.

Từ đó có thể xử lý bốn phép toán với phân số và liên phân số:

$$
\dfrac{p}{q}\pm x = \dfrac{\pm qx+p}{0x+q},\ \dfrac{p}{q}x = \dfrac{px+0}{0x+q},\ \frac{p}{q}/x = \dfrac{0x+p}{qx+0}.
$$

Với phép toán giữa hai liên phân số, dùng biến đổi phân thức tuyến tính hai biến:

$$
x+y = \dfrac{0xy+x+y+0}{0xy+0x+0y+1},\ xy = \dfrac{1xy+0x+0y+0}{0xy+0x+0y+1},\ \dfrac{x}{y} = \dfrac{0xy+x+0y+0}{0xy+0x+y+0}.
$$

???+ example "Biến đổi phân thức tuyến tính hai biến"
    Cho $L(x,y)=\dfrac{axy+bx+cy+d}{exy+fx+gy+h}$ và $\alpha=[\alpha_0,\alpha_1,\cdots,\alpha_n]$, $\beta=[\beta_0,\beta_1,\cdots,\beta_m]$, tìm biểu diễn liên phân số của $\gamma=L(\alpha,\beta)$.

??? note "Lời giải"
    Tương tự một biến, để xác định phần nguyên, chỉ cần đảm bảo $e,f,g,h$ cùng dấu và
    
    $$
    \left\lfloor\dfrac{a}{e}\right\rfloor = \left\lfloor\dfrac{b}{f}\right\rfloor = \left\lfloor\dfrac{c}{g}\right\rfloor = \left\lfloor\dfrac{d}{h}\right\rfloor.
    $$
    
    Hợp phải tương ứng với $L(x,y)\mapsto L(L_{\alpha_i}(x),y)$ và $L(x,y)\mapsto L(x,L_{\beta_j}(y))$, đều là biến đổi tuyến tính của hệ số. Hợp trái giống trường hợp một biến, chỉ cần lấy mô-đun.
    
    Cần chọn thứ tự hợp $L_{\alpha_i}$ hay $L_{\beta_j}$. Thứ tự không ảnh hưởng kết quả, nên có thể xen kẽ, hoặc ưu tiên chiều có độ chênh lớn hơn: nếu $\left|\dfrac{b}{f}-\dfrac{d}{h}\right|>\left|\dfrac{c}{g}-\dfrac{d}{h}\right|$ thì hợp $L_{\alpha_i}$ trước, ngược lại hợp $L_{\beta_j}$.

## Liên phân số tuần hoàn

Tương tự thập phân tuần hoàn, nếu hệ số của liên phân số lặp chu kỳ thì gọi là liên phân số tuần hoàn.

???+ abstract "Liên phân số tuần hoàn"
    Với $x=[a_0,a_1,a_2,\cdots]$, nếu tồn tại $K$ và $L>0$ sao cho mọi $k\ge K$ có $a_k=a_{k+L}$, thì $x$ là **liên phân số tuần hoàn** (periodic continued fraction). $L$ nhỏ nhất gọi là chu kỳ dương nhỏ nhất, và dãy $a_k,\cdots,a_{k+L-1}$ là chu kỳ. Khi đó
    
    $$
    x=[a_0,\cdots,a_{k-1},\overline{a_k,\cdots,a_{k+L-1}}].
    $$
    
    Nếu $K=0$ (tức $x=[\overline{a_0,\cdots,a_{L-1}}]$) thì gọi là **thuần tuần hoàn** (purely periodic); ngược lại là **hỗn tuần hoàn** (eventually periodic).

### Số vô tỉ bậc hai

Khái niệm liên quan chặt chẽ: [số vô tỉ bậc hai](./quadratic.md) (quadratic irrational), là nghiệm vô tỉ của phương trình bậc hai hệ số nguyên. Mọi số vô tỉ bậc hai đều có dạng

$$
a+b\sqrt D
$$

với $a,b$ hữu tỉ và $D$ là số nguyên dương không có bình phương. Ở đây mặc định là số thực; liên hợp của $a+b\sqrt D$ là $a-b\sqrt D$.

Kết quả của Euler:

???+ note "Định lý (Euler)"
    Mọi liên phân số tuần hoàn đều là số vô tỉ bậc hai.

??? note "Chứng minh"
    Với $x=[a_0,\cdots,a_{k-1},\overline{a_k,\cdots,a_{k+L-1}}]$, đặt $y=[\overline{a_k,\cdots,a_{k+L-1}}]$ thì
    
    $$
    \begin{aligned}
    x&=[a_0,\cdots,a_{k-1},y] = L_0(y),\\
    y&=[a_k,\cdots,a_{k+L-1},y] = L_1(y),
    \end{aligned}
    $$
    
    với $L_0,L_1$ là biến đổi phân thức tuyến tính. Suy ra
    
    $$
    x = L_0\circ L_1\circ L_0^{-1}(x). 
    $$
    
    Gọi $L_0\circ L_1\circ L_0^{-1}(x) = \dfrac{ax+b}{cx+d}$, ta có
    
    $$
    cx^2+(d-a)x-b=0.
    $$
    
    Vậy liên phân số tuần hoàn là nghiệm của phương trình bậc hai hệ số nguyên. Vì liên phân số vô hạn là vô tỉ, nên đây là vô tỉ bậc hai.

Định lý Lagrange đảo lại:

???+ note "Định lý (Lagrange)"
    Mọi số vô tỉ bậc hai đều có biểu diễn liên phân số tuần hoàn.

??? note "Chứng minh"
    Ý tưởng: chứng minh các phần dư lặp lại. Viết
    
    $$
    x = \dfrac{P_0+\sqrt{D}}{Q_0}
    $$
    
    với $P_0,Q_0,D$ là số nguyên và $Q_0\mid D-P_0^2$. Luôn có thể, ví dụ:
    
    $$
    a+b\sqrt{D'} = \dfrac{p_a}{q_a}+\dfrac{p_b}{q_b}\sqrt{D'} = \dfrac{p_aq_b+p_bq_a\sqrt{D'}}{q_aq_b} = \dfrac{p_ap_bq_aq_b+\sqrt{(q_aq_b)^2D'}}{(q_aq_b)^2}
    $$
    
    rồi đặt $P=p_ap_bq_aq_b$, $Q=(q_aq_b)^2$, $D=QD'$.
    
    Khi đó mọi phần dư có dạng
    
    $$
    r_k=\dfrac{P_k+\sqrt D}{Q_k},
    $$
    
    với $P_k,Q_k$ nguyên và $Q_k\mid D-P_k^2$. Điều kiện này đảm bảo hệ số của $\sqrt D$ là $1$.
    
    Dùng quy nạp. Với $k=0$ hiển nhiên. Giả sử đúng với $r_k$, đặt $a_k=\lfloor r_k\rfloor$, ta có
    
    $$
    r_k = a_k+\dfrac{1}{r_{k+1}}.
    $$
    
    Viết $r_{k+1}$ ở dạng tương tự và thế vào:
    
    $$
    \dfrac{P_k+\sqrt D}{Q_k} = a_k + \dfrac{Q_{k+1}}{P_{k+1}+\sqrt{D}} = a_k + \dfrac{Q_{k+1}P_{k+1}-Q_{k+1}\sqrt{D}}{P_{k+1}^2-D}.
    $$
    
    So sánh hệ số:
    
    $$
    \dfrac{P_k}{Q_k} = a_k+\dfrac{Q_{k+1}P_{k+1}}{P_{k+1}^2-D},\ \dfrac{1}{Q_k}=-\dfrac{Q_{k+1}}{P_{k+1}^2-D}.
    $$
    
    Suy ra
    
    $$
    P_{k+1} = a_kQ_k-P_k.
    $$
    
    Thế lại:
    
    $$
    Q_{k+1} = \dfrac{D-P_{k+1}^2}{Q_k} = \dfrac{D-(a_kQ_k-P_k)^2}{Q_k} = -a_k^2Q_k+2a_kP_k+\dfrac{D-P_k^2}{Q_k}.
    $$
    
    Vì $Q_k\mid D-P_k^2$ nên $P_{k+1},Q_{k+1}$ nguyên.
    
    Cuối cùng, chứng minh $r_k$ chỉ có hữu hạn giá trị. Ta có
    
    $$
    \dfrac{P_k+\sqrt{D}}{Q_k} = r_k = -\dfrac{q_{k-2}x-p_{k-2}}{q_{k-1}x-p_{k-1}},
    $$
    
    và với vô tỉ, $r_k>1$. Xét liên hợp:
    
    $$
    \dfrac{P_k-\sqrt{D}}{Q_k} = r_k^* = -\dfrac{q_{k-2}x^*-p_{k-2}}{q_{k-1}x^*-p_{k-1}} = -\dfrac{q_{k-2}}{q_{k-1}}\dfrac{x^*-\dfrac{p_{k-2}}{q_{k-2}}}{x^*-\dfrac{p_{k-1}}{q_{k-1}}}
    $$
    
    Với $k$ đủ lớn thì $r_k^*<0$, vì
    
    $$
    \dfrac{q_{k-2}}{q_{k-1}}>0,\ \lim_{k\rightarrow\infty}\dfrac{x^*-\dfrac{p_{k-2}}{q_{k-2}}}{x^*-\dfrac{p_{k-1}}{q_{k-1}}}=\dfrac{x^*-x}{x^*-x}=1.
    $$
    
    Suy ra
    
    $$
    \dfrac{2\sqrt{D}}{Q_k} = r_k-r_k^*>1 \iff 0<Q_k\le 2\sqrt{D}.
    $$
    
    Vậy $Q_k$ chỉ có hữu hạn giá trị. Mặt khác,
    
    $$
    D-P_{k}^2=Q_kQ_{k-1}>0 \iff |P_k|<\sqrt{D},
    $$
    
    nên $P_k$ cũng hữu hạn giá trị. Do đó $r_k$ lặp lại sau hữu hạn bước.

Từ chứng minh rút ra truy hồi phần dư của số vô tỉ bậc hai:

???+ note "Công thức truy hồi phần dư của vô tỉ bậc hai"
    Mọi vô tỉ bậc hai có thể viết
    
    $$
    x=\dfrac{P_0+\sqrt{D}}{Q_0}
    $$
    
    với $Q_0\mid D-P^2_0$. Phần dư
    
    $$
    r_{k}=\dfrac{P_k+\sqrt{D}}{Q_k}
    $$
    
    thỏa
    
    $$
    \begin{aligned}
    P_{k+1} &= a_kQ_k-P_k,\\
    Q_{k+1} &= \dfrac{D-P_{k+1}^2}{Q_k}.
    \end{aligned}
    $$

Công thức này dùng trực tiếp để tính liên phân số của vô tỉ bậc hai; từ chứng minh có $|P_k|<\sqrt{D}$ và $Q_k\le 2\sqrt{D}$. Độ phức tạp phụ thuộc độ dài chu kỳ, là $O(\sqrt{D}\log D)$[^period-surd].

???+ example "Vô tỉ bậc hai"
    Cho $\alpha=\dfrac{x+y\sqrt{n}}{z}$, tìm biểu diễn liên phân số. Với $x,y,z,n\in\mathbf Z$, $n>0$ không là chính phương.

??? note "Lời giải"
    Đưa về dạng chuẩn và áp dụng truy hồi. Hạng $a_k=\lfloor r_k\rfloor$. Để tìm chu kỳ, lưu lại chỉ số đầu tiên xuất hiện của $(P_k,Q_k)$.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/quadratic-irrational.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/quadratic-irrational.py:core"
        ```

???+ example "[Tavrida NU Akai Contest - Continued Fraction](https://timus.online/problem.aspx?space=1&num=1814)"
    Cho $x$ và $k$, với $x$ không là chính phương, $0\le k\le 10^9$. Tính phân số tiệm cận thứ $k$ của $\sqrt{x}$.

??? note "Lời giải"
    Tính chu kỳ của $\sqrt{x}$ rồi biểu diễn chu kỳ thành biến đổi phân thức tuyến tính; dùng [lũy thừa nhanh](../binary-exponentiation.md) để lấy $x_k$. Phần trước chu kỳ và phần chưa đủ một chu kỳ xử lý riêng.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/surd-convergent.cpp"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/surd-convergent.py"
        ```

### Liên phân số thuần tuần hoàn

Điều kiện cần và đủ để một số thực có biểu diễn thuần tuần hoàn.

Vì liên phân số thuần tuần hoàn có cấu trúc gần như hữu hạn nên có thể “đảo thứ tự”. Dạng đảo có quan hệ xác định với dạng ban đầu.

???+ note "Định lý (Galois)"
    Với
    
    $$
    x = \left[\overline{a_0,a_1,\cdots,a_{\ell}}\right],
    $$
    
    đặt
    
    $$
    x' = \left[\overline{a_{\ell},\cdots,a_1,a_0}\right].
    $$
    
    Khi đó $x$ và $x'$ là “nghịch đảo của liên hợp với dấu âm”, tức $x'$ là đối của nghịch đảo liên hợp của $x$.

??? note "Chứng minh"
    Không cần $\ell+1$ là chu kỳ nhỏ nhất, giả sử $\ell>0$. Theo định lý đảo thứ tự,
    
    $$
    \begin{aligned}
    \dfrac{p_\ell}{p_{\ell-1}} &= [a_\ell,\cdots,a_1,a_0] = \dfrac{p'_\ell}{q'_\ell},\\
    \dfrac{q_\ell}{q_{\ell-1}} &= [a_\ell,\cdots,a_1] = \dfrac{p'_{\ell-1}}{q'_{\ell-1}}.
    \end{aligned}
    $$
    
    Hai vế là phân số tối giản nên
    
    $$
    p'_\ell = p_\ell,\ q'_\ell = p_{\ell-1},\ p'_{\ell-1}=q_\ell,\ q'_{\ell-1} = q_{\ell-1}.
    $$
    
    Với liên phân số thuần tuần hoàn $x$, phần dư thứ $\ell+1$ chính là $x$, do đó
    
    $$
    x = \dfrac{xp_\ell+p_{\ell-1}}{xq_\ell+q_{\ell-1}},
    $$
    
    nên $x$ là nghiệm của
    
    $$
    q_\ell x^2+(q_{\ell-1}-p_\ell)x-p_{\ell-1} = 0.
    $$
    
    Tương tự $x'$ thỏa
    
    $$
    q'_\ell(x')^2+(q'_{\ell-1}-p'_\ell)x'-p'_{\ell-1} = 0,
    $$
    
    hay
    
    $$
    p_{\ell-1}(x')^2+(q_{\ell-1}-p_\ell)x'-q_\ell = 0.
    $$
    
    Đặt $y=-\dfrac{1}{x'}$, thì $x$ và $y$ là nghiệm của cùng một phương trình, nhưng $x>0>y$, nên là hai nghiệm liên hợp. Suy ra $x'$ là đối của nghịch đảo liên hợp của $x$.

Galois dùng quan sát này để đưa ra điều kiện cần và đủ:

???+ note "Định lý (Galois)"
    Số vô tỉ bậc hai $x$ có biểu diễn thuần tuần hoàn khi và chỉ khi $x>1$ và liên hợp $x^*$ thỏa $-1<x^*<0$.

??? note "Chứng minh"
    Nếu $x$ thuần tuần hoàn, thì $a_0=a_{\ell+1}\ge 1$ nên $x>1$. Do nghịch đảo liên hợp với dấu âm cũng là tuần hoàn, nên $-\dfrac{1}{x^*}>1$, tức $-1<x^*<0$.
    
    Ngược lại, giả sử $x>1$ và $-1<x^*<0$. Với phần dư $r_k$:
    
    $$
    r_k = a_k+\dfrac{1}{r_{k+1}},
    $$
    
    lấy liên hợp:
    
    $$
    r_{k}^* = a_k+\dfrac{1}{r_{k+1}^*}.
    $$
    
    Từ đây suy ra $-1<r_k^*<0$ với mọi $k\ge 0$. Với $k=0$ đúng vì $r_0^*=x^*$. Nếu $-1<r_k^*<0$ thì
    
    $$
    -1<-\dfrac{1}{a_k}< r_{k+1}^* = \dfrac{1}{r_k^*-a_k} < -\dfrac{1}{1+a_k} < 0.
    $$
    
    Suy ra
    
    $$
    a_k = -\dfrac{1}{r_{k+1}^*}+r_k^* = \left\lfloor-\dfrac{1}{r_{k+1}^*}\right\rfloor.
    $$
    
    Vì số vô tỉ bậc hai là tuần hoàn, tồn tại $L$ và $k$ đủ lớn sao cho $r_k=r_{k+L}$. Khi đó
    
    $$
    a_{k-1} = \left\lfloor-\dfrac{1}{r_{k}^*}\right\rfloor = \left\lfloor-\dfrac{1}{r_{k+L}^*}\right\rfloor = a_{k+L-1},
    $$
    
    và
    
    $$
    r_{k-1} = a_{k-1}+\dfrac{1}{r_k} = a_{k+L-1}+\dfrac{1}{r_{k+L}} = r_{k+L-1}.
    $$
    
    Suy ra chỉ số nhỏ nhất để lặp là $0$, tức $x$ thuần tuần hoàn.

Định lý Galois cho quy luật của **căn bậc hai thuần** (pure quadratic surd), tức dạng $\sqrt{r}$.

???+ note "Hệ quả"
    Với $r>1$ hữu tỉ, nếu $\sqrt{r}$ vô tỉ thì
    
    $$
    \sqrt{r} = [\lfloor\sqrt{r}\rfloor,\overline{a_1,\cdots,a_{\ell},2\lfloor\sqrt{r}\rfloor}]
    $$
    
    và $a_k = a_{\ell+1-k}$ với mọi $1\le k\le\ell$.

??? note "Chứng minh"
    Với $\sqrt{r}$, vì $\lfloor\sqrt{r}\rfloor+\sqrt{r}>1$ và $-1<\lfloor\sqrt{r}\rfloor-\sqrt{r}<0$, nên
    
    $$
    \lfloor\sqrt{r}\rfloor+\sqrt{r} = [\overline{2\lfloor\sqrt{r}\rfloor,a_1,\cdots,a_\ell}].
    $$
    
    Theo định lý trên, nghịch đảo liên hợp với dấu âm có dạng
    
    $$
    \dfrac{1}{\sqrt{r}-\lfloor\sqrt{r}\rfloor} = [\overline{a_\ell,\cdots,a_1,2\lfloor\sqrt{r}\rfloor}].
    $$
    
    Suy ra
    
    $$
    \sqrt{r}=\lfloor\sqrt{r}\rfloor+\dfrac{1}{\dfrac{1}{\sqrt{r}-\lfloor\sqrt{r}\rfloor}}=[\lfloor\sqrt{r}\rfloor,\overline{a_\ell,\cdots,a_1,2\lfloor\sqrt{r}\rfloor}].
    $$
    
    Mặt khác,
    
    $$
    \sqrt{r} = -\lfloor\sqrt{r}\rfloor+\left(\lfloor\sqrt{r}\rfloor+\sqrt{r}\right) = [\lfloor\sqrt{r}\rfloor,\overline{a_1,\cdots,a_\ell,2\lfloor\sqrt{r}\rfloor}].
    $$
    
    Do biểu diễn liên phân số của vô tỉ là duy nhất, suy ra $a_k=a_{\ell+1-k}$.

??? example "Ví dụ: khai triển liên phân số của $\sqrt{74}$"
    (Chỉ để minh họa; khi lập trình nên dùng thuật toán truy hồi ở trên.)
    
    $$
    \begin{aligned}
    \sqrt{74}&=8+(-8)+\sqrt{74}=\left[8,\frac{8+\sqrt{74}}{10}\right]\\
    &=\left[8,1+\frac{-2+\sqrt{74}}{10}\right]=\left[8,1,\frac{2+\sqrt{74}}{7}\right]\\
    &=\left[8,1,1+\frac{-5+\sqrt{74}}{7}\right]=\left[8,1,1,\frac{5+\sqrt{74}}{7}\right]\\
    &=\left[8,1,1,1+\frac{-2+\sqrt{74}}{7}\right]=\left[8,1,1,1,\frac{2+\sqrt{74}}{10}\right]\\
    &=\left[8,1,1,1,1+\frac{-8+\sqrt{74}}{10}\right]=\left[8,1,1,1,1,8+\sqrt{74}\right]\\
    &=\left[8,1,1,1,1,16+(-8)+\sqrt{74}\right]=\left[8,\overline{1,1,1,1,16}\right]
    \end{aligned}
    $$
    
    Các phần dư:
    
    $$
    \begin{alignedat}{3}
    r_1&=\frac{8+\sqrt{74}}{10}&&=\left[\overline{1,1,1,1,16}\right]\\
    r_2&=\frac{2+\sqrt{74}}{7}&&=\left[\overline{1,1,1,16,1}\right]\\
    r_3&=\frac{5+\sqrt{74}}{7}&&=\left[\overline{1,1,16,1,1}\right]\\
    r_4&=\frac{2+\sqrt{74}}{10}&&=\left[\overline{1,16,1,1,1}\right]\\
    r_5&=8+\sqrt{74}&&=\left[\overline{16,1,1,1,1}\right]
    \end{alignedat}
    $$
    
    Theo Galois, $r_k$ và $r_{L+1-k}$ có phần tuần hoàn đảo ngược nên là nghịch đảo liên hợp với dấu âm. Nếu độ dài chu kỳ $L$ lẻ, hạng giữa tự là nghịch đảo liên hợp của chính nó; nếu $L$ chẵn thì không có. Phần về phương trình Pell sẽ cho thấy tính chẵn/lẻ của $L$ quyết định phương trình $x^2-Dy^2=-1$ có nghiệm hay không.

Liên phân số của $\sqrt{D}$ chủ yếu dùng trong [phương trình Pell](./pell-equation.md).

## Ví dụ bài toán

Sau khi nắm khái niệm cơ bản, xem một số bài toán cụ thể.

???+ example "Bao lồi dưới đường thẳng"
    Cho $r=[a_0,a_1,\cdots,a_n]$, tìm bao lồi của các điểm nguyên $(x,y)$ với $0\le x\le N$ và $0\le y\le rx$.

??? note "Lời giải"
    Với miền không bị chặn $x\ge 0$, bao lồi trên chính là đường $y=rx$. Nhưng khi thêm $x\le N$, bao lồi sẽ lệch khỏi đường.
    
    ![](./images/lattice-hull.svg)
    
    Bắt đầu từ $(0,0)$, duyệt các điểm nguyên của bao lồi trên từ trái sang phải. Giả sử điểm cuối hiện tại là $(x,y)$. Tìm điểm tiếp theo $(x',y')$ với $(\Delta x,\Delta y)=(x'-x,y'-y)$. Khi đó
    
    $$
    0<\Delta x\le N-x,\ 0\le \Delta y\le r\Delta x.
    $$
    
    Bất đẳng thức thứ hai vì nếu $\Delta y>r\Delta x$ thì $(x,y)$ không nằm trên bao lồi. Với mỗi $(x,y)$, chỉ cận trên của $\Delta x$ thay đổi. Do đó chỉ cần giải bài toán con: đặt cận trên của $x$ là $N'$, tìm điểm nguyên đầu tiên trên bao lồi trên kề gốc. Gọi nghiệm $(q,p)$. Khi đó $p,q$ nguyên tố cùng nhau, và độ dốc $\dfrac{p}{q}$ là lớn nhất trong các điểm dưới đường $y=rx$ với hoành độ $\le N'$. Theo [diễn giải hình học](#几何解释), điểm này tương ứng một phân số trung gian dưới của $r$. Vì phân số trung gian dưới có mẫu càng lớn càng gần $r$, nên nghiệm là phân số trung gian dưới có mẫu lớn nhất không vượt $N'$.
    
    Thực tế, không cần giải lại mỗi bài con. Hãy tính các phân số tiệm cận để duyệt mọi phân số trung gian dưới. Sau đó duyệt phân số trung gian dưới theo mẫu giảm dần, mỗi lần cộng vào điểm trước nếu được, đến khi không cộng được thì xét phân số tiếp theo.
    
    Có tối ưu hiển nhiên: với phân số trung gian dưới $(q,p)$, tồn tại $k$ lẻ và $0\le t<a_k$ sao cho $(q,p)=(q_{k-1},p_{k-1})+t(q_k,p_k)$. Chỉ cần $t=\left\lfloor\dfrac{N-q_{k-1}-x}{q_k}\right\rfloor$. Không lo $t$ vượt vì phân số tiệm cận dưới lớn hơn $(q_{k+2},p_{k+2})$ đã được cộng.
    
    Độ phức tạp là $O(n)$. Mặc dù số điểm trung gian có thể nhiều, số bước tăng thực sự không nhiều. Ta có thể chứng minh trong mỗi đoạn trung gian dưới $(q,p)=(q_{k-1},p_{k-1})+t(q_k,p_k)$ với $0\le t<a_k$, tối đa chỉ có hai bước tăng. Vì nếu có bước tăng thì $q_{k-1}\le N-x<q_{k+1}$. Lấy $t=\left\lfloor\dfrac{N-q_{k-1}-x}{q_k}\right\rfloor$. Nếu $t=0$ thì $\Delta x=q_{k-1}$ và sau khi cộng, $N-x'<q_{k-1}$, không còn tăng nữa. Nếu $t>0$, sau khi cộng, $N-x'=(N-q_{k-1}-x)\bmod q_k<q_k$, nên lần sau chỉ có thể $t'=0$. Do đó mỗi đoạn có nhiều nhất hai tăng, tổng thời gian $O(n)$.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/hull-under-line.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/hull-under-line.py:core"
        ```

???+ example "[Timus - Crime and Punishment](https://timus.online/problem.aspx?space=1&num=1430)"
    Cho $A,B,N \le 2\times 10^9$, tìm $x,y\ge 0$ sao cho $Ax+By\le N$ và $Ax+By$ lớn nhất.

??? note "Lời giải"
    Có lời giải $O(\sqrt N)$: giả sử $A\ge B$ , vì $A(B+x)+By=Ax+B(A+y)$, chỉ cần tìm trong $x\le\min\{N/A, B\}$. Cách này đủ AC. Nhưng dùng liên phân số có thể giảm còn $O(\log N)$.
    
    Đặt $x\mapsto\left\lfloor N/A\right\rfloor-x$, $C=N\bmod A$, $M=\left\lfloor N/A\right\rfloor$. Bài toán thành: với $0\le x\le M$, $By-Ax\le C$, tối đa hóa $By-Ax$. Với mỗi $x$, $y$ tốt nhất là $\left\lfloor\dfrac{Ax+C}{B}\right\rfloor$.
    
    Bài toán tương tự ví dụ trước, nhưng thay vì dùng trung gian dưới để tránh đường thẳng, ở đây dùng trung gian trên để tiến gần đường $By-Ax=C$. Khoảng cách tỷ lệ với $C-(By-Ax)$. Tối đa hóa $By-Ax$ tương đương tối thiểu hóa khoảng cách tới đường thẳng. Ý tưởng: bắt đầu từ điểm ngoài cùng bên trái, đi dọc bao lồi trên của các điểm nguyên, dần thu hẹp khoảng cách cho tới tối ưu.
    
    Trong hệ tọa độ $(x,y)$, bắt đầu từ $(0,\lfloor C/B\rfloor)$, tìm và cộng dần các tăng lượng $(\Delta x,\Delta y)$ sao cho điểm mới gần đường hơn nhưng không vượt sang phía kia và không vượt $x>M$. Điều kiện:
    
    $$
    0<B\Delta y-A\Delta x\le C-(By-Ax),\ 0<\Delta x\le M-x.
    $$
    
    Viết lại
    
    $$
    \Delta y \le \dfrac{A}{B}\Delta x+\dfrac{C-(By-Ax)}{B}.
    $$
    
    Nếu hằng số phía sau <1, thì điểm nguyên có hoành độ nhỏ nhất thỏa là một phân số trung gian trên (theo [diễn giải hình học](#几何解释)). Khi cộng một tăng lượng, cận trên giảm, nên phải xét các trung gian trên có mẫu lớn hơn.
    
    Duyệt các trung gian trên theo mẫu tăng dần. Nếu thỏa cả ràng buộc hoành và tung độ, cộng vào và cập nhật cận. Khi hết, thu được nghiệm tối ưu. Lập luận tương tự ví dụ trước (thay $\Delta x$ bằng $B\Delta y-A\Delta x$) cho thấy độ phức tạp $O(\log\min\{A,B\})$.
    
    === "C++"
        ```py
        --8<-- "docs/math/code/continued-fraction/closest-dio.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/closest-dio.py:core"
        ```

???+ example "[June Challenge 2017 - Euler Sum](https://www.codechef.com/problems/ES)"
    Tính $\sum\limits_{x=1}^N \lfloor \mathrm{e}x \rfloor$, với $\mathrm{e}$ là cơ số log tự nhiên.
    
    Gợi ý: $e = [2,1,2,1,1,4,1,1,6,1,\cdots,1,2n,1, \cdots]$.[^continued-fraction-of-e]

??? note "Lời giải"
    Tổng bằng số điểm nguyên trong tập $\{(x,y):1\le x\le N,1\le y\le\mathrm{e}x\}$. Dựng bao lồi của các điểm dưới đường $y=\mathrm{e}x$, rồi dùng [định lý Pick](../../geometry/pick.md) để đếm. Độ phức tạp $O(\log N)$.
    
    Bài gốc yêu cầu $N \le 10^{4000}$. Mã C++ chỉ minh họa, không hiện thực số lớn.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/sum-floor.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/sum-floor.py:core"
        ```

???+ example "[NAIPC 2019 - It's a Mod, Mod, Mod, Mod World](https://open.kattis.com/problems/itsamodmodmodmodworld)"
    Cho $p,q,n$, tính $\sum\limits_{i=1}^n [pi \bmod q]$.

??? note "Lời giải"
    Vì
    
    $$
    \sum_{i=1}^n [pi \bmod q] 
    =\sum_{i=1}^n\left(pi - q\left\lfloor\dfrac{pi}{q}\right\rfloor\right) = \dfrac{pn(n+1)}{2} - q\sum_{i=1}^n\left\lfloor\dfrac{p}{q}i\right\rfloor,
    $$
    
    bài toán quy về ví dụ trước với $\dfrac{p}{q}$ thay cho $\mathrm{e}$. Độ phức tạp cho một truy vấn là $O(\log\min\{p,q\})$.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/mod-mod-mod.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/mod-mod-mod.py:core"
        ```

???+ example "[Library Checker - Sum of Floor of Linear](https://judge.yosupo.jp/problem/sum_of_floor_of_linear)"
    Cho $N,M,A,B$, tính $\displaystyle\sum_{i=0}^{N-1} \left\lfloor \frac{A \cdot i + B}{M} \right\rfloor$.

??? note "Lời giải"
    Đây là bài khó nhất trong chuỗi ví dụ. Có thể giải bằng [thuật toán kiểu Euclid](./euclidean.md). Ở đây nêu thuật toán dựa trên liên phân số với độ phức tạp $O(\log\min\{A,B\})$.
    
    Dựng bao lồi của các điểm nguyên dưới đường $y=\dfrac{Ax+B}{M}$ với $0\le x< N$, rồi dùng Pick để đếm. Trường hợp $B=0$ đã xử lý trước. Với $B\neq 0$, có thể làm hai bước: trước hết dùng trung gian trên để tiến gần đường (như ví dụ 2), sau đó dùng trung gian dưới để đi xa dần (như ví dụ 1).
    
    === "C++"
        ```py
        --8<-- "docs/math/code/continued-fraction/sum-floor-axbc.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/sum-floor-axbc.py:core"
        ```

???+ example "[OKC 2 - From Modular to Rational](https://codeforces.com/gym/102354/problem/I)"
    Có số hữu tỉ chưa biết $\dfrac{p}{q}$ với $1\le p, q\le 10^9$. Có thể hỏi giá trị $pq^{-1}$ mod $m$ với $m$ nguyên tố trong $[10^9,10^{12}]$. Hãy xác định $p,q$ trong không quá 10 lần hỏi.
    
    Bài toán tương đương: tìm $x$ trong $[1,N]$ sao cho $Ax\bmod M$ nhỏ nhất.

??? note "Lời giải"
    Theo [CRT](./crt.md), hỏi trên nhiều mod nguyên tố tương đương hỏi trên tích của chúng. Do đó coi như hỏi trên mô-đun đủ lớn $m$, cần xác định tử mẫu.
    
    Với mô-đun $m$, có thể có nhiều cặp $(p,q)$ thỏa $qr\equiv p\pmod m$. Nếu $(p_1,q_1)$ và $(p_2,q_2)$ đều thỏa, thì $(p_1q_2-p_2q_1)r\equiv 0\pmod m$. Vì $r$ nguyên tố cùng $m$, suy ra $m\mid(p_1q_2-p_2q_1)$. Nếu hiệu không bằng 0 thì trị tuyệt đối ít nhất là $m$. Nhưng $p,q\in[1,10^9]$ nên $|p_1q_2-p_2q_1|\le 10^{18}$. Do đó nếu $m>10^{18}$ thì nghiệm duy nhất.
    
    Bài toán quy về: cho $m$ và $r$, tìm $(p,q)$ với $q\le n$ thỏa $qr\equiv p\pmod m$. Khi nghiệm là duy nhất, chỉ cần tìm $q\in[1,n]$ sao cho $qr\bmod m$ nhỏ nhất, vì chỉ có một $q$ cho phần dư $\le n$. Đây là phát biểu tương đương.
    
    Trên mặt phẳng $(q,k)$, tương đương tìm điểm nguyên dưới đường $qr-km=0$ gần nhất. Theo [diễn giải hình học](#几何解释), điểm này tương ứng một phân số trung gian dưới của $\dfrac{r}{m}$. Độ phức tạp $O(\log\min\{r,m\})$.
    
    === "C++"
        ```cpp
        --8<-- "docs/math/code/continued-fraction/recover-fraction.cpp:core"
        ```
    
    === "Python"
        ```py
        --8<-- "docs/math/code/continued-fraction/recover-fraction.py:core"
        ```

## Bài tập

-   [UVa OJ - Continued Fractions](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=775)
-   [ProjectEuler+ #64: Odd period square roots](https://www.hackerrank.com/contests/projecteuler/challenges/euler064/problem)
-   [「LibreOJ NOI Round #2」单枪匹马](https://loj.ac/p/573)
-   [Codeforces Round #184 (Div. 2) - Continued Fractions](https://codeforces.com/contest/305/problem/B)
-   [Codeforces Round #201 (Div. 1) - Doodle Jump](https://codeforces.com/contest/346/problem/E)
-   [Codeforces Round #325 (Div. 1) - Alice, Bob, Oranges and Apples](https://codeforces.com/contest/585/problem/C)
-   [POJ Founder Monthly Contest 2008.03.16 - A Modular Arithmetic Challenge](http://poj.org/problem?id=3530)
-   [2019 Multi-University Training Contest 5 - fraction](http://acm.hdu.edu.cn/showproblem.php?pid=6624)
-   [SnackDown 2019 Elimination Round - Election Bait](https://www.codechef.com/SNCKEL19/problems/EBAIT)
-   [Luogu P5179. Fraction](https://www.luogu.com.cn/problem/P5179)
-   [Luogu P7739. \[NOI2021\] 密码箱](https://www.luogu.com.cn/problem/P7739)

## Tài liệu tham khảo và đọc thêm

-   Hardy, G. H., Wright, E. M., Heath-Brown, R., & Silverman, J. (2008). An Introduction to the Theory of Numbers. Oxford Mathematics.
-   朱尧辰，王连祥《丢番图逼近引论》
-   [Blog FatFish - Nhập môn liên phân số](https://chaoli.club/index.php/2756)
-   [Simple continued fraction - Wikipedia](https://en.wikipedia.org/wiki/Simple_continued_fraction)
-   [Periodic continued fraction - Wikipedia](https://en.wikipedia.org/wiki/Periodic_continued_fraction)
-   [Gosper's original notes on continued fraction arithmetic algorithms](https://perl.plover.com/yak/cftalk/INFO/gosper.txt)
-   [Understanding Bill Gosper's continued fraction arithmetic (implemented in Python)](https://hsinhaoyu.github.io/cont_frac/)

**Trang này chủ yếu dịch từ [Continued fractions](https://cp-algorithms.com/algebra/continued-fractions.html), giấy phép CC-BY-SA 4.0, có chỉnh sửa nội dung.**

[^one-representation]: Số tự nhiên $1$ chỉ có biểu diễn không chuẩn: $1=[1]=[0,1]$.
[^continuant]: Tên dịch theo bản dịch “Concrete Mathematics” của 张明尧、张凡, mục 6.7.
[^sqrt5]: Khi đó không thể mặc định $\dfrac{p}{q}$ là phân số tiệm cận, dù định lý Legendre cho thấy chỉ có thể là một phân số tiệm cận. Với trường hợp phân số tiệm cận, có thể chứng minh qua công thức sai số.
[^semi-range]: Một số tài liệu xử lý khác nhau việc $t$ có bao gồm hai đầu hay không.
[^semiconvergent]: Với $t=0$ hiểu là liên phân số hình thức, tương đương cắt ở hạng áp chót.
[^nose-streching]: Đây không phải thuật ngữ chuẩn. Có thể dịch từ tài liệu Nga [ЦЕПНЫЕ ДРОБИ](https://old.mccme.ru/free-books/mmmf-lectures/book.14-full.pdf), mục Алгоритм «вытягивания носов».
[^pgl2]: Các tính chất cho thấy nhóm biến đổi phân thức tuyến tính đẳng cấu với [nhóm tuyến tính xạ ảnh](https://en.wikipedia.org/wiki/Projective_linear_group) $PGL_2(\mathbf R)$.
[^period-surd]: Chứng minh xem [Wikipedia](https://en.wikipedia.org/wiki/Periodic_continued_fraction#Length_of_the_repeating_block).
[^continued-fraction-of-e]: Chứng minh khai triển liên phân số của $\mathrm{e}$ xem [tại đây](https://proofwiki.org/wiki/Continued_Fraction_Expansion_of_Euler%27s_Number).
````
