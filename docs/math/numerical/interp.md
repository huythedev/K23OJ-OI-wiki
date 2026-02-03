author: AtomAlpaca, billchenchina, caibyte, Chrogeek, Early0v0, EndlessCheng, Enter-tainer, Henry-ZHR, hly1204, hsfzLZH1, Ir1d, Ghastlcon, kenlig, Marcythm, megakite, Peanut-Tang, qwqAutomaton, qz-cqy, StudyingFather, swift-zym, swiftqwq, Tiphereth-A, TrisolarisHD, Watersail2005, x4Cx58x54, Xeonacid, xiaopangfeiyu, YanWQ-monad

## Giới thiệu

Nội suy là phương pháp suy ra giá trị dữ liệu mới trong một khoảng bằng cách dựa vào các điểm dữ liệu rời rạc đã biết. Nội suy thường dùng trong khớp hàm.

Ví dụ với các điểm dữ liệu:

| $x$    | $0$ | $1$      | $2$      | $3$      | $4$       | $5$       | $6$       |
| ------ | --- | -------- | -------- | -------- | --------- | --------- | --------- |
| $f(x)$ | $0$ | $0.8415$ | $0.9093$ | $0.1411$ | $-0.7568$ | $-0.9589$ | $-0.2794$ |

![](../images/interp-1.svg)

Trong đó $f(x)$ chưa biết, nội suy có thể ước lượng các điểm chưa biết bằng cách khớp $f(x)$ theo một dạng nào đó.

Ví dụ, ta có thể dùng hàm tuyến tính từng đoạn:

![](../images/interp-2.svg)

Cách này gọi là [nội suy tuyến tính](https://en.wikipedia.org/wiki/Linear_interpolation).

Ta cũng có thể khớp bằng đa thức:

![](../images/interp-3.svg)

Cách này gọi là [nội suy đa thức](https://en.wikipedia.org/wiki/Polynomial_interpolation).

Dạng tổng quát của nội suy đa thức:

???+ note "Nội suy đa thức"
    Với $n+1$ điểm $(x_0,y_0),(x_1,y_1),\dots,(x_n,y_n)$ đã biết, tìm đa thức dạng $f(x)=\sum_{i=0}^n a_ix^i$ thỏa
    
    $$
    f(x_i)=y_i,\qquad\forall i=0,1,\dots,n
    $$
    
    .

Sau đây giới thiệu hai cách trong nội suy đa thức: Lagrange và Newton. Có thể chứng minh hai cách cho cùng kết quả.

## Nội suy Lagrange

Do cần dựng hàm $f(x)$ đi qua các điểm $P_1(x_1, y_1), P_2(x_2,y_2),\cdots,P_n(x_n,y_n)$. Trước hết đặt hình chiếu của điểm thứ $i$ lên trục $x$ là $P_i^{\prime}(x_i,0)$.

Xét dựng $n$ hàm $f_1(x), f_2(x), \cdots, f_n(x)$ sao cho với hàm $f_i(x)$, đồ thị của nó đi qua $\begin{cases}P_j^{\prime}(x_j,0),(j\neq i)\\P_i(x_i,y_i)\end{cases}$, khi đó hàm cần tìm là $f(x)=\sum\limits_{i=1}^nf_i(x)$.

Ta đặt $f_i(x)=a\cdot\prod_{j\neq i}(x-x_j)$, thay $P_i(x_i,y_i)$ vào suy ra $a=\dfrac{y_i}{\prod_{j\neq i} (x_i-x_j)}$, nên

$$
f_i(x)=y_i\cdot\dfrac{\prod_{j\neq i} (x-x_j)}{\prod_{j\neq i} (x_i-x_j)}=y_i\cdot\prod_{j\neq i}\dfrac{x-x_j}{x_i-x_j}
$$

Suy ra dạng Lagrange:

$$
f(x)=\sum_{i=1}^ny_i\cdot\prod_{j\neq i}\dfrac{x-x_j}{x_i-x_j}
$$

Cài đặt ngây thơ có độ phức tạp $O(n^2)$, có thể tối ưu xuống $O(n\log^2 n)$, xem [Nội suy đa thức nhanh](../poly/multipoint-eval-interpolation.md#多项式的快速插值).

???+ note "[Luogu P4781【模板】拉格朗日插值](https://www.luogu.com.cn/problem/P4781)"
    Cho $n$ điểm $(x_i,y_i)$ và $k$, với $\forall i,j$ có $i\neq j \iff x_i\neq x_j$ và $f(x_i)\equiv y_i\pmod{998244353}$ và $\deg(f(x)) < n$ (định nghĩa $\deg(0)=-\infty$), tính $f(k)\bmod{998244353}$.
    
    ??? note "Lời giải"
        Ở bài này chỉ cần $f(k)$ nên có thể thế trực tiếp $k$ vào công thức; đôi khi cần nhiều lần tính hoặc thao tác phức tạp hơn thì phải tìm hệ số của $f$. Mã dưới đây cho cách tính hệ số.
        
        $$
        f(k)=\sum_{i=1}^{n}y_i\prod_{j\neq i }\frac{k-x_j}{x_i-x_j}
        $$
        
        Cần tính nghịch đảo. Nếu tính riêng tử và mẫu rồi nhân tử với nghịch đảo của mẫu, cộng dồn vào đáp án, nút thắt không còn ở nghịch đảo; độ phức tạp $O(n^2)$.
        
        Vì làm việc trên modulo $998244353$, tạm coi thời gian tính nghịch đảo là hằng số.
    
    ??? note "Mã"
        ```cpp
        --8<-- "docs/math/code/numerical/interp/interp_1.cpp"
        ```

### Nội suy Lagrange với hoành độ là các số nguyên liên tiếp

Nếu biết hoành độ là các số nguyên liên tiếp, có thể nội suy $O(n)$.

Gọi đa thức cần tìm là $f(x)$, biết $f(1),\cdots,f(n+1)$ ($1\le i\le n+1$), xét công thức:

$$
\begin{aligned}
f(x)&=\sum\limits_{i=1}^{n+1}y_i\prod\limits_{j\ne i}\frac{x-x_j}{x_i-x_j}\\
&=\sum\limits_{i=1}^{n+1}y_i\prod\limits_{j\ne i}\frac{x-j}{i-j}
\end{aligned}
$$

Tích phía sau xét tử và mẫu riêng; tử là:

$$
\dfrac{\prod\limits_{j=1}^{n+1}(x-j)}{x-i}
$$

Mẫu tích $i-j$ tách thành hai giai đoạn theo giai thừa:

$$
(-1)^{n+1-i}\cdot(i-1)!\cdot(n+1-i)!
$$

Suy ra công thức nội suy khi hoành độ là $1,\cdots,n+1$:

$$
f(x)=\sum\limits_{i=1}^{n+1}(-1)^{n+1-i}y_i\cdot\frac{\prod\limits_{j=1}^{n+1}(x-j)}{(i-1)!(n+1-i)!(x-i)}
$$

Tiền xử lý tích tiền/sau của $(x-i)$, giai thừa và nghịch đảo giai thừa, rồi thay vào công thức, độ phức tạp $O(n)$.

???+ note "Ví dụ [CF622F The Sum of the k-th Powers](https://codeforces.com/contest/622/problem/F)"
    Cho $n,k$, tính $\sum\limits_{i=1}^ni^k$ modulo $10^9+7$.
    
    ??? note "Lời giải"
        Đáp án là đa thức bậc $k+1$, nên có thể sàng tuyến tính $1^i,\cdots,(k+2)^i$ rồi nội suy $O(n)$.
        
        Cũng có thể từ kiến thức tổ hợp và sai phân suy ra:
        
        $$
        f(x)=\sum_{i=1}^{n+1}\binom{x-1}{i-1}\sum_{j=1}^{i}(-1)^{i+j}\binom{i-1}{j-1}y_{j}=\sum\limits_{i=1}^{n+1}y_i\cdot\frac{\prod\limits_{j=1}^{n+1}(x-j)}{(x-i)\cdot(-1)^{n+1-i}\cdot(i-1)!\cdot(n+1-i)!}
        $$
    
    ??? note "Mã"
        ```cpp
        --8<-- "docs/math/code/numerical/interp/interp_2.cpp"
        ```

## Nội suy Newton

Nội suy Newton dựa trên sai phân bậc cao, ưu điểm là hỗ trợ thêm điểm mới trong $O(n)$.

Để chèn điểm trong $O(n)$, đặt:

$$
f(x)=\sum_{j=0}^n a_jn_j(x)
$$

với $n_j(x):=\prod_{i=0}^{j-1}(x-x_i)$ gọi là **cơ sở Newton** (Newton basis).

Giải $a_j$ sẽ được đa thức nội suy. Ta định nghĩa **thương sai tiến** (forward divided differences):

$$
\begin{aligned}
    \lbrack y_k\rbrack  & := y_k,                                                                & k=0,\dots,n, \\
    [y_k,\dots,y_{k+j}] & := \dfrac{[y_{k+1},\dots,y_{k+j}]-[y_k,\dots,y_{k+j-1}]}{x_{k+j}-x_k}, & k=0,\dots,n-j,~j=1,\dots,n.
\end{aligned}
$$

Khi đó:

$$
\begin{aligned}
    f(x)&=[y_0]+[y_0,y_1](x-x_0)+\dots+[y_0,\dots,y_n](x-x_0)\dots(x-x_{n-1})\\
    &=\sum_{j=0}^n [y_0,\dots,y_j]n_j(x)
\end{aligned}
$$

Đây là dạng Newton. Cài đặt ngây thơ $O(n^2)$.

Nếu các điểm mẫu cách đều (tức $x_i=x_0+ih$, $i=1,\dots,n$), ta có:

$$
[y_k,\dots,y_{k+j}]=\frac{1}{j!h^j}\Delta^{(j)}y_k,
$$

trong đó $\Delta^{(j)}y_k$ là **sai phân tiến** (forward differences), định nghĩa:

$$
\begin{aligned}
    \Delta^{(0)}y_k & := y_k,                                       & k=0,\dots,n, \\
    \Delta^{(j)}y_k & := \Delta^{(j-1)} y_{k+1}-\Delta^{(j-1)} y_k, & k=0,\dots,n-j,~j=1,\dots,n.
\end{aligned}
$$

Đặt $x=x_0+sh$, công thức Newton thành:

$$
f(x)=\sum_{j=0}^n \binom{s}{j}j!h^j[y_0,\dots,y_j]=\sum_{j=0}^n \binom{s}{j}\Delta^{(j)}y_0.
$$

??? note "Code ([Luogu P4781【模板】拉格朗日插值](https://www.luogu.com.cn/problem/P4781))"
    ```cpp
    --8<-- "docs/math/code/numerical/interp/interp_3.cpp"
    ```

### Nội suy Newton với hoành độ là các số nguyên liên tiếp

Ví dụ: tìm hệ số của đa thức $f(x)=\sum_{i=0}^{3} a_ix^i$, biết $f(1)$ đến $f(6)$ lần lượt là $1, 5, 14, 30, 55, 91$.

$$
\begin{array}{cccccccccccc}
1 &    &  5 &    & 14 &    & 30 &    & 55 &    & 91 & \\
&  4 &    &  9 &    & 16 &    & 25  &    & 36 & \\
&    &  5 &    &  7 &    &  9 &    &  11 & \\
&    &    &  2 &    &  2 &    &  2 & \\
\end{array}
$$

Hàng đầu là các giá trị liên tiếp của $f(x)$; mỗi hàng sau là hiệu giữa hai phần tử kề nhau của hàng trước. Quan sát: nếu thao tác đủ nhiều lần (với $f(x)$ là đa thức), cuối cùng sẽ thu được một giá trị hằng.

Phần tử đầu của sai phân bậc $i-1$ là $\sum_{j=1}^{i}(-1)^{i+j}\binom{i-1}{j-1}f(j)$, và đóng góp của nó vào $f(k)$ là $\binom{k-1}{i-1}$ lần.

$$
f(k)=\sum_{i=1}^n\binom{k-1}{i-1}\sum_{j=1}^{i}(-1)^{i+j}\binom{i-1}{j-1}f(j)
$$

Độ phức tạp $O(n^2)$.

## Cài đặt trong C++

Từ C++20, thư viện chuẩn có [`std::midpoint`](https://en.cppreference.com/w/cpp/numeric/midpoint) và [`std::lerp`](https://en.cppreference.com/w/cpp/numeric/lerp) để tính trung điểm và nội suy tuyến tính.

## Bài tập

-   [「NOIP2020」微信步数](https://loj.ac/p/3389)
-   [「联合省选 2022」填树](https://loj.ac/p/3701)
-   [「NOI2019」机器人](https://loj.ac/p/3157)

## Tài liệu tham khảo

1.  [Interpolation - Wikipedia](https://en.wikipedia.org/wiki/Interpolation)
2.  [Newton polynomial - Wikipedia](https://en.wikipedia.org/wiki/Newton_polynomial)
