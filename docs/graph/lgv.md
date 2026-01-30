## Giới thiệu

Định lý Lindström–Gessel–Viennot, hay còn gọi là bổ đề LGV, là công cụ mạnh để đếm số đường đi không giao nhau trên đồ thị có hướng không chu trình (DAG).

Kiến thức nền tảng: [Các khái niệm cơ bản về đồ thị](./concept.md), [Ma trận](../math/linear-algebra/matrix.md), [Tính định thức bằng Gauss](../math/numerical/gauss.md).

Bổ đề LGV chỉ áp dụng cho **đồ thị có hướng không chu trình**.

## Định nghĩa

$\omega(P)$ là tích các trọng số cạnh trên đường đi $P$. (Nếu chỉ đếm số đường đi, đặt trọng số các cạnh là $1$; thực tế, trọng số có thể là hàm sinh).

$e(u, v)$ là tổng $\omega(P)$ trên **mọi** đường đi $P$ từ $u$ đến $v$, tức $e(u, v)=\sum\limits_{P:u\rightarrow v}\omega(P)$.

Tập xuất phát $A$ là một tập con gồm $n$ đỉnh của đồ thị.

Tập đích $B$ cũng là một tập con gồm $n$ đỉnh của đồ thị.

Một bộ đường đi không giao nhau $A\rightarrow B$ là $S$: $S_i$ là đường đi từ $A_i$ đến $B_{\sigma(S)_i}$ (với $\sigma(S)$ là một hoán vị), với mọi $i\ne j$, $S_i$ và $S_j$ không có đỉnh chung.

$t(\sigma)$ là số nghịch thế (inversion) của hoán vị $\sigma$.

## Bổ đề

$$
M = \begin{bmatrix}e(A_1,B_1)&e(A_1,B_2)&\cdots&e(A_1,B_n)\\
e(A_2,B_1)&e(A_2,B_2)&\cdots&e(A_2,B_n)\\
\vdots&\vdots&\ddots&\vdots\\
e(A_n,B_1)&e(A_n,B_2)&\cdots&e(A_n,B_n)\end{bmatrix}
$$

$$
\det(M)=\sum\limits_{S:A\rightarrow B}(-1)^{t(\sigma(S))}\prod\limits_{i=1}^n \omega(S_i)
$$

Trong đó $\sum\limits_{S:A\rightarrow B}$ là tổng trên mọi bộ đường đi không giao nhau $A\rightarrow B$ thỏa mãn điều kiện trên.

### Chứng minh

Theo định nghĩa định thức:

$$
\begin{align}
\det(M)&=\sum_{\sigma}(-1)^{t(\sigma)}\prod_{i=1}^n e(a_i,b_{\sigma(i)})\\
&=\sum_{\sigma}(-1)^{t(\sigma)}\prod_{i=1}^n \sum_{P:a_i\to b_{\sigma(i)}} \omega(P)
\end{align}
$$

Nhận thấy $\prod\limits_{i=1}^n \sum\limits_{P:a_i\to b_{\sigma(i)}} \omega(P)$ chính là tổng $\omega(P)$ trên mọi bộ đường đi từ $A$ đến $B$ theo hoán vị $\sigma$.

$$
\begin{align}
&\sum_{\sigma}(-1)^{t(\sigma)}\prod_{i=1}^n \sum_{P:a_i\to b_{\sigma(i)}} \omega(P)\\
=&\sum_{\sigma}(-1)^{t(\sigma)}\sum_{P=\sigma}\omega(P)\\
=&\sum_{P:A\to B}(-1)^{t(\sigma)}\prod_{i=1}^n \omega(P_i)
\end{align}
$$

Ở đây $P$ là một bộ đường đi bất kỳ.

Gọi $U$ là bộ đường đi không giao nhau, $V$ là bộ đường đi có giao nhau,

$$
\begin{align}
&\sum_{P:A\to B}(-1)^{t(\sigma)}\prod_{i=1}^n \omega(P_i)\\
=&\sum_{U:A\to B}(-1)^{t(U)}\prod_{i=1}^n \omega(U_i)+\sum_{V:A\to B}(-1)^{t(V)}\prod_{i=1}^n \omega(V_i)
\end{align}
$$

Giả sử $P$ có một bộ đường đi giao nhau $P_i:a_1 \to u \to b_1, P_j:a_2 \to u \to b_2$, thì tồn tại bộ đối ứng $P_i'=a_1\to u\to b_2, P_j'=a_2\to u\to b_1$, các đường còn lại giống $P$. Khi đó $\omega(P)=\omega(P'), t(P)=t(P')\pm 1$.

Do đó $\sum\limits_{V:A\to B}(-1)^{t(\sigma)}\prod\limits_{i=1}^n \omega(V_i)=0$.

Vậy $\det(M)=\sum\limits_{U:A\to B}(-1)^{t(U)}\prod\limits_{i=1}^n \omega(U_i)=0$.

Chứng minh xong[^1].

## Bài tập ví dụ

???+ note "Ví dụ 1 [CF348D Turtles](https://codeforces.com/contest/348/problem/D)"
    Đề bài: Cho bàn cờ $n\times m$, một số ô đi được, một số ô không. Một con rùa từ $(x, y)$ chỉ được đi sang $(x+1, y)$ hoặc $(x, y+1)$. Hỏi số cách đi không giao nhau từ $(1, 1)$ đến $(n, m)$, lấy kết quả modulo $10^9+7$. $2\le n,m\le3000$.

Đây là ứng dụng trực tiếp của bổ đề LGV. Xét mọi đường đi hợp lệ, nhận thấy từ $(1,1)$ phải đi qua $A=\{(1,2), (2,1)\}$, đến đích phải qua $B=\{(n-1, m), (n, m-1)\}$, nên $A, B$ đã xác định. Áp dụng LGV, đáp án là:

$$
\begin{vmatrix}
f(a_1, b_1) & f(a_1, b_2) \\
f(a_2, b_1) & f(a_2, b_2)
\end{vmatrix} = f(a_1, b_1)\times f(a_2, b_2) - f(a_1, b_2)\times f(a_2, b_1)
$$

Trong đó $f(a, b)$ là số đường đi từ $a$ đến $b$ trên đồ thị, có thể dùng quy hoạch động $O(nm)$ để tính kể cả khi có ô cấm. Độ phức tạp cuối cùng $O(nm)$.

??? note "Mã mẫu"
    ```cpp
    --8<-- "docs/graph/code/lgv/lgv_2.cpp"
    ```

???+ note "Ví dụ 2 [HDU 5852 Intersection is not allowed!](https://acm.hdu.edu.cn/showproblem.php?pid=5852)"
    Đề bài: Cho bàn cờ $n\times n$, có $k$ quân cờ, quân thứ $i$ xuất phát ở $(1, a_i)$, đích là $(n, b_i)$, chỉ được đi sang phải hoặc xuống dưới, các đường đi không được giao nhau. Tính số cách, lấy modulo $10^9+7$. $1\le n\le 10^5$, $1\le k\le 100$, $1\le a_1<a_2<\dots<a_n\le n$, $1\le b_1<b_2<\dots<b_n\le n$.

Nhận thấy nếu các đường không giao nhau thì nhất thiết $a_i$ đi tới $b_i$, nên trong LGV luôn có $\sigma(S)_i=i$, không cần xét dấu. Trọng số cạnh là $1$, áp dụng bổ đề trực tiếp.

Số đường đi từ $(1, a_i)$ đến $(n, b_j)$ là chọn $n-1$ bước xuống trong tổng $n-1+b_j-a_i$ bước, tức $e(A_i, B_j)=\binom{n-1+b_j-a_i}{n-1}$.

Tính định thức có thể dùng Gauss.

Độ phức tạp $O(n+k(k^2 + \log p))$, với $\log p$ là tính nghịch đảo modulo.

??? note "Mã mẫu"
    ```cpp
    --8<-- "docs/graph/code/lgv/lgv_1.cpp"
    ```

## Tài liệu tham khảo

[^1]: Chứng minh tham khảo [Zhihu - Chứng minh bổ đề LGV](https://zhuanlan.zhihu.com/p/517819133)
