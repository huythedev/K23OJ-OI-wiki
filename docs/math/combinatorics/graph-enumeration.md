Trong tổ hợp, **đếm đồ thị** (Graph Enumeration) là nhánh nghiên cứu các bài toán đếm số đồ thị thỏa mãn một tính chất nhất định. [Hàm sinh](../poly/intro.md)、[định lý đếm Polya](./polya.md) và [phương pháp ký hiệu](../poly/symbolic-method.md#%E9%9B%86%E5%90%88%E7%9A%84-cycle-%E6%9E%84%E9%80%A0), cùng với [OEIS](https://oeis.org/), là những công cụ toán học quan trọng nhất để giải các bài toán loại này. Đếm đồ thị có thể chia thành hai loại chính: có nhãn và không nhãn. Trong đa số trường hợp[^1], bài toán có nhãn thường đơn giản hơn bài toán không nhãn tương ứng, vì vậy ta sẽ xét trước các bài toán có nhãn.

[^1]: Có lẽ cây nhị phân không nhãn là một phản ví dụ: khi cấu trúc đủ đơn giản, nhóm hoán vị tương ứng là nhóm đồng nhất (Identity Group), khi đó phiên bản có nhãn chỉ cần nhân thêm $n!$ là được.

## Cây có nhãn

Chính là công thức Cayley, xem bài [dãy Prüfer](../../graph/prufer.md). Ta cũng có thể dùng [định lý cây ma trận Kirchhoff](../../graph/matrix-tree.md) hoặc [hàm sinh](../poly/intro.md#生成函数) và [định lý Lagrange](https://codeforces.com/blog/entry/104184) để suy ra kết quả này.

### Bài tập

-   [Hihocoder 1047. Random Tree](https://vjudge.net/problem/HihoCoder-1047)

## Đồ thị liên thông có nhãn

### Ví dụ “POJ 1737” Connected Graph

???+ note "Ví dụ [“POJ 1737” Connected Graph](http://poj.org/problem?id=1737)"
    Tóm tắt đề: đếm số đồ thị liên thông có nhãn với $n$ đỉnh ($n \leq 50$).

Loại bài này xuất hiện sớm trong loạt “8 bài đàn ông” của Lou. Đặt $g_n$ là số đồ thị có nhãn trên $n$ đỉnh, $c_n$ là dãy cần tìm. Với $n$ đỉnh, số cạnh tối đa là $\binom{n}{2}$, mỗi cạnh có/không có độc lập nên $g_n = 2^{\binom{n}{2}}$. Cố định một đỉnh, liệt kê kích thước thành phần liên thông chứa nó, ta cần chọn thêm $i-1$ đỉnh từ $n-1$ đỉnh còn lại để tạo thành thành phần liên thông. Các đỉnh ngoài thành phần có thể nối cạnh tùy ý, nên có quan hệ đệ quy:

$$
\begin{align}
\sum_{i=1}^{n} \binom{n-1}{i-1} c_i g_{n-i} &= g_n \\
c_n &= g_n - \sum_{i=1}^{n-1} \binom{n-1}{i-1} c_i g_{n-i} 
\end{align}
$$

Chuyển vế ta được công thức đệ quy $O(n^2)$ cho $c_n$, đủ để AC bài này.

### Ví dụ “Bài tập đội tuyển 2013” Quy hoạch thành phố

???+ note "Ví dụ [“Bài tập đội tuyển 2013” Quy hoạch thành phố](https://www.luogu.com.cn/problem/P4841)"
    Tóm tắt đề: đếm số đồ thị liên thông có nhãn trên $n$ đỉnh ($n \leq 130000$).

Với phạm vi lớn hơn, ta thường cần xây dựng hàm sinh để dùng các thuật toán đa thức nhanh.

#### Cách 1: chia để trị + FFT

Công thức đệ quy trên có dạng tự chập, nên có thể dùng FFT chia để trị, độ phức tạp $O(n\log^2n)$.

#### Cách 2: nghịch đảo đa thức

Khai triển tổ hợp trong công thức đệ quy và biến đổi:

$$
\begin{align}
\sum_{i=1}^{n} \binom{n-1}{i-1} c_i g_{n-i} &= g_n \\
\sum_{i=1}^{n} \frac{c_i}{(i-1)!} \frac{g_{n-i}}{(n-i)!} &= \frac{g_n}{(n-1)!}
\end{align}
$$

Xây dựng đa thức:

$$
\begin{align}
C(x) &= \sum_{n=1} \frac{c_n}{(n-1)!} x^n \\
G(x) &= \sum_{n=0} \frac{g_n}{n!} x^n \\
H(x) &= \sum_{n=1} \frac{g_n}{(n-1)!} x^n
\end{align}
$$

Thế vào được $CG = H$, dùng [nghịch đảo đa thức](../poly/elementary-func.md#%E5%A4%9A%E9%A1%B9%E5%BC%8F%E6%B1%82%E9%80%86) rồi chập để tìm $C(x)$.

#### Cách 3: đa thức exp

Một cách khác là dùng [ý nghĩa tổ hợp của exp trong EGF](../poly/egf.md#egf-%E4%B8%AD%E5%A4%9A%E9%A1%B9%E5%BC%8F-exp-%E7%9A%84%E7%BB%84%E5%90%88%E6%84%8F%E4%B9%89). Gọi EGF của đồ thị liên thông có nhãn và đồ thị đơn có nhãn lần lượt là $C(x)$ và $G(x)$, ta có:

$$
\begin{align}
\exp(C(x)) &= G(x) \\
C(x) &= \ln(G(x))
\end{align}
$$

Dùng [đa thức ln](../poly/elementary-func.md#多项式对数函数--指数函数) để lấy $C(x)$.

## Đồ thị Euler, đồ thị hai phía có nhãn

### Ví dụ “SPOJ KPGRAPHS” Counting Graphs

???+ note "Ví dụ [“SPOJ KPGRAPHS” Counting Graphs](http://www.spoj.com/problems/KPGRAPHS/)"
    Tóm tắt đề: đếm số đồ thị có nhãn trên $n$ đỉnh thỏa các tính chất sau ($n \leq 1000$).
    
    -   Liên thông [A001187](https://oeis.org/A001187).
    -   Euler [A033678](https://oeis.org/A033678).
    -   Hai phía [A047864](https://oeis.org/A047864).

Bài này giới hạn độ dài code nên không thể dùng template đa thức, nhưng hàm sinh vẫn giúp phân tích.

Bài liên thông đã giải ở trên. Xét đồ thị Euler. Lưu ý các cách đếm liên thông ở trên đều có thể tổng quát cho đồ thị liên thông thỏa một tính chất bất kỳ. Ví dụ, thay $g_n$ trong công thức liên thông hóa từ “đồ thị bất kỳ” sang “đồ thị mà bậc đỉnh đều chẵn”, khi đó $c_n$ chính là số đồ thị Euler.

Ta gói quá trình ở POJ 1737 thành hàm liên thông hóa:

```cpp
void ln(Int C[], Int G[]) {
  for (int i = 1; i <= n; ++i) {
    C[i] = G[i];
    for (int j = 1; j <= i - 1; ++j)
      C[i] -= binom[i - 1][j - 1] * C[j] * G[i - j];
  }
}
```

Hai câu đầu có thể giải dễ dàng:

```cpp
for (int i = 1; i <= n; ++i) G[i] = pow(2, binom[i][2]);
ln(C, G);
for (int i = 1; i <= n; ++i) G[i] = pow(2, binom[i - 1][2]);
ln(E, G);
```

Lưu ý quá trình liên thông hóa này tương đương với lấy log của EGF. Tương tự, ta cũng có hàm “nghịch liên thông hóa”, tương đương với exp của EGF.

```cpp
void exp(Int G[], Int C[]) {
  for (int i = 1; i <= n; ++i) {
    G[i] = C[i];
    for (int j = 1; j <= i - 1; ++j)
      G[i] += binom[i - 1][j - 1] * C[j] * G[i - j];
  }
}
```

Tiếp theo bàn về đếm đồ thị hai phía có nhãn,

Gọi $b_n$ là số đồ thị hai phía trên $n$ đỉnh, $g_n$ là số đồ thị trên $n$ đỉnh được tô 2 màu sao cho các đỉnh cùng màu không có cạnh nối. Liệt kê số đỉnh của một màu, ta có[^2]:

$$
g_n = \sum_{i=0}^{n} \binom{n}{i}2^{i(n-i)}
$$

[^2]: [Blog của PinkRabbit](https://www.luogu.com.cn/blog/PinkRabbit/solution-sp4420) cho biết dãy này cũng có thể tối ưu bằng [Chirp Z-Transform](../poly/czt.md).

Sau đó ta dùng hai cách khác nhau để liên hệ $g_n$ và $b_n$.

#### Cách 1: tính hai lần

Gọi $c_{n, k}$ là số đồ thị hai phía có $k$ thành phần liên thông, dễ có:

$$
\begin{align}
b_n &= \sum_{i=1}^{n} c_{n, i} \\
g_n &= \sum_{i=1}^{n} c_{n, i} 2^i 
\end{align}
$$

So sánh hai biểu thức của $g_n$, khai triển:

$$
\begin{align}
\sum_{i=0}^{n} \binom{n}{i}2^{i(n-i)} &= \sum_{i=1}^{n} c_{n, i} 2^i \\
c_{n, i} &= \sum_{i=0}{n-1} \binom{n-1}{i-1} c_{n, 1}c_{n-i,k-1}
\end{align}
$$

Suy ra đệ quy cho $b_n$ với độ phức tạp $O(n^3)$, rồi dùng bao hàm–loại trừ tối ưu còn $O(n^2)$ để AC.

#### Cách 2: liên thông hóa đệ quy

Cách 2 và 3 đều dùng số đồ thị hai phía liên thông $b1_n$ [A001832](https://oeis.org/A001832) để làm cầu nối giữa $g_n$ và $b_n$.

Mỗi đồ thị hai phía liên thông có đúng 2 cách tô màu, ứng với hai đồ thị 2 màu liên thông khác nhau. Vì vậy, liên thông hóa $g_n$ cho ta dãy đúng bằng $2b1_n$, còn $b_n$ thì lấy nghịch liên thông hóa từ $b1_n$.

Do đó:

```cpp
for (int i = 1; i <= n; ++i) {
  G[i] = 0;
  for (int j = 0; j < i + 1; ++j) G[i] += binom[i][j] * pow(2, j * (i - j));
}
ln(B1, G);
for (int i = 1; i <= n; ++i) B1[i] /= 2;
exp(B, B1);
```

Cả hai quá trình đều $O(n^2)$, AC được bài.

#### Cách 3: đa thức exp

Ta cũng có thể hiểu bằng EGF.

Gọi $G(x)$ là EGF của $g_n$, $B1(x)$ là EGF của $b1_n$, $B(x)$ là EGF của $b_n$. Theo cách 2:

$$
\begin{align}
G(x) &= \exp(2B1(x)) \\
B(x) &= \exp(B1(x))  \\
     &= \exp(\frac{\ln{G(x)}}{2}) \\
     &= \sqrt{G}
\end{align}
$$

Lấy đạo hàm hai vế và so hệ số sẽ ra công thức đệ quy dễ code. Lưu ý cách 2 và 3 về bản chất là như nhau, và thường cách 3 cho thời gian tốt hơn.

$$
\begin{align}
B_n^2 &= G  \\
2B_nB_n' &= G' 
\end{align}
$$

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/combinatorics/graph-enumeration/graph-enumeration_1.cpp"
    ```

### Bài tập

-   [UOJ Goodbye Jihai D. 新年的追逐战](https://uoj.ac/contest/50/problem/498)
-   [BZOJ 3864. 大朋友和多叉树](https://hydro.ac/p/bzoj-P3864)
-   [BZOJ 2863. 愤怒的元首](https://hydro.ac/p/bzoj-P2863)
-   [Luogu P6295. 有标号 DAG 计数](https://www.luogu.com.cn/problem/P6295)
-   [LOJ 6569. 仙人掌计数](https://loj.ac/p/6569)
-   [LOJ 6570. 毛毛虫计数](https://loj.ac/p/6570)
-   [Luogu P5434. 有标号荒漠计数](https://www.luogu.com.cn/problem/P5434)
-   [Luogu P3343. \[ZJOI2015\] 地震后的幻想乡](https://www.luogu.com.cn/problem/P3343)
-   [HDU 5279. YJC plays Minecraft](https://acm.hdu.edu.cn/showproblem.php?pid=5279)
-   [Luogu P7364. 有标号二分图计数](https://www.luogu.com.cn/problem/P7364)
-   [Luogu P5827. 点双连通图计数](https://www.luogu.com.cn/problem/P5827)
-   [Luogu P5827. 边双连通图计数](https://www.luogu.com.cn/problem/P5828)
-   [Luogu P6596. How Many of Them](https://www.luogu.com.cn/problem/P6596)
-   [Luogu U152448. 有标号强连通图计数](https://www.luogu.com.cn/problem/U152448)
-   [Project Euler 434. Rigid graphs](https://projecteuler.net/problem=434)

## Công thức Riddell

Cách dùng exp trong EGF ở trên đôi khi được gọi là **công thức Riddell cho đồ thị có nhãn**. Hàm sinh với **biến đổi Euler** của [phương pháp ký hiệu](../poly/symbolic-method.md#%E9%9B%86%E5%90%88%E7%9A%84-multiset-%E6%9E%84%E9%80%A0) đôi khi cũng được gọi là **công thức Riddell cho đồ thị không nhãn**; dạng sau xuất hiện sớm trong nghiên cứu phân hoạch của Euler và cũng gặp trong bài toán ba lô vô hạn.

Với dãy $a_i$ và OGF $A(x)$ tương ứng, định nghĩa biến đổi Euler của $A(x)$:

$$
\begin{align}
\mathcal{E}(A(x)) &= \prod_{i} (1-x^i)^{-a_i}  \\
                  &= \exp (\sum_{i} \frac{A(x^i)}{i})  
\end{align}
$$

Gọi hệ số của $\mathcal{E}(A(x))$ là $b_i$, và định nghĩa mảng phụ $c_i = \sum_{d|n} d a_d$, ta có công thức đệ quy:

$$
n b_n = c_n + \sum_{i=1}^{n-1} c_i b_{n-i}
$$

## Cây không nhãn

### Ví dụ “SPOJ PT07D” Let us count 1 2 3

???+ note "Ví dụ [“SPOJ PT07D” Let us count 1 2 3](https://www.spoj.com/problems/PT07D/)"
    Tóm tắt đề: đếm số cây có $n$ đỉnh thỏa các tính chất:
    
    -   Cây có nhãn có gốc [A000169](https://oeis.org/A000169).
    -   Cây có nhãn không gốc [A000272](https://oeis.org/A000272).
    -   Cây không nhãn có gốc [A000081](https://oeis.org/A000081).
    -   Cây không nhãn không gốc [A000055](https://oeis.org/A000055).

#### Cây có gốc

Trường hợp có nhãn đã giải ở trên. Xét cây có gốc không nhãn, đặt OGF là $F(x)$, áp dụng biến đổi Euler, ta có:

$$
F(x) = x\mathcal{E}(F(x))
$$

Lấy hệ số là được.

#### Cây không gốc

Xét bao hàm–loại trừ: lấy số cây có gốc trừ đi số cây mà gốc không phải trọng tâm, và xét chẵn/lẻ của $n$.

Khi $n$ lẻ:

Tồn tại một cây con có kích thước $\geq \left\lceil \frac{n}{2}\right\rceil$, liệt kê kích thước cây con:

$$
g_n = f_n - \sum_{i=\left\lceil\frac{n}{2}\right\rceil}^{n-1} f_i f_{n-i}
$$

Khi $n$ chẵn:

Nếu có hai trọng tâm, quá trình trên chỉ trừ một lần, nên cần trừ thêm:

$$
g_n = f_n - \sum_{i=\left\lceil\frac{n}{2}\right\rceil}^{n-1} f_i f_{n-i} - \binom{f_{\frac{n}{2}}}{2}
$$

### Ví dụ “Luogu P5900” Đếm cây không nhãn không gốc

???+ note "Ví dụ [“Luogu P5900” Đếm cây không nhãn không gốc](https://www.luogu.com.cn/problem/P5900)"
    Tóm tắt đề: đếm số cây không nhãn không gốc trên $n$ đỉnh ($n \leq 200000$).

Với dữ liệu lớn hơn, làm tương tự: biến đổi Euler rồi dùng template đa thức.

## Đồ thị đơn không nhãn

### Ví dụ “SGU 282. Isomorphism” Isomorphism

???+ note "Ví dụ [“SGU 282. Isomorphism” Isomorphism](https://codeforces.com/problemsets/acmsguru/problem/99999/282)"
    Tóm tắt đề: đếm số cách tô màu $m$ màu cho các cạnh của đồ thị đầy đủ không nhãn trên $n$ đỉnh.

Khi $m = 2$, đối tượng chính là đồ thị đơn không nhãn [A000088](https://oeis.org/A000088). Xét định lý đếm Polya:

$$
\frac{1}{|G|}\sum_{g\in G} m^{c(g)}
$$

Nhóm hoán vị $G$ là nhóm đối xứng bậc $n$ của các đỉnh sinh ra hoán vị trên tập cạnh. Làm brute force cần $O(n!)$, không thể.

Phân loại theo cấu trúc chu trình của hoán vị; mỗi cấu trúc tương ứng một phân hoạch số. Dùng dfs() sinh phân hoạch, bài toán chuyển thành tính số hoán vị $w(p)$ ứng với phân hoạch $p$ và số chu trình $c(p)$ trong mỗi lớp. Đáp án:

$$
\frac{1}{|G|} \sum_{p \in P} w(p) m^{c(p)}
$$

Xét $w(p)$: mỗi phân hoạch tương ứng một hoán vị chu trình, các phần tử cùng kích thước không phân biệt thứ tự, nên:

$$
w(p) = \frac{n!}{\prod_{i}(p_i)\prod_{i}(q_i!)} 
$$

với $q_i$ là số lần phần tử kích thước $i$ xuất hiện trong $p$.

Xét $c(p)$: chu trình trên tập đỉnh là $|p|$, nhưng ta quan tâm chu trình trên tập cạnh.

Nếu một cạnh nối hai đỉnh cùng một chu trình, kích thước chu trình là $p_i$, thì số chu trình cạnh là $\left\lfloor \frac{p_i}{2} \right\rfloor$.

Nếu một cạnh nối hai đỉnh thuộc hai chu trình khác nhau, kích thước lần lượt $p_i,p_j$, mỗi chu trình cạnh có độ dài $\operatorname{lcm}(p_i,p_j)$, nên số chu trình cạnh là $\frac{p_i p_j}{\operatorname{lcm}(p_i,p_j)} = \gcd(p_i, p_j)$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/combinatorics/graph-enumeration/graph-enumeration_2.cpp"
    ```

## Bài tập

-   [CodeForces 438 E. The Child and Binary Tree](https://codeforces.com/problemset/problem/438/E)
-   [Luogu P5448. \[THUPC2018\] 好图计数](https://www.luogu.com.cn/problem/P5448)
-   [Luogu P5818. \[JSOI2011\] 同分异构体计数](https://www.luogu.com.cn/problem/P5818)
-   [Luogu P6597. 烯烃计数](https://www.luogu.com.cn/problem/P6597)
-   [Luogu P6598. 烷烃计数](https://www.luogu.com.cn/problem/P6598)
-   [Luogu P4128. \[SHOI2006\] 有色图](https://www.luogu.com.cn/problem/P4128)
-   [Luogu P4727. \[HNOI2009\] 图的同构计数](https://www.luogu.com.cn/problem/P4727)
-   [AtCoder Beginner Contest 222 H. Binary Tree](https://atcoder.jp/contests/abc222/tasks/abc222_h)
-   [AtCoder Beginner Contest 284 Ex. Count Unlabeled Graphs](https://atcoder.jp/contests/abc284/tasks/abc284_h)
-   [Luogu P4708. 画画](https://www.luogu.com.cn/problem/P4708)
-   [Luogu P7592. 数树（2021 CoE-II E）](https://www.luogu.com.cn/problem/P7592)
-   [Luogu P5206. \[WC2019\] 数树](https://www.luogu.com.cn/problem/P5206)

## Tài liệu tham khảo và chú thích

1.  [WC2015, Tài liệu trao đổi của học viên Gu Yuzhou: Graphical Enumeration](https://github.com/lychees/ACM-Training/blob/master/Note/%E5%86%AC%E4%BB%A4%E8%90%A5/2015/%E9%A1%BE%E6%98%B1%E6%B4%B2%E8%90%A5%E5%91%98%E4%BA%A4%E6%B5%81%E8%B5%84%E6%96%99%20Graphical%20Enumeration.pdf)
2.  [WC2019, Hàm sinh, thuật toán đa thức và đếm đồ thị](https://github.com/lychees/ACM-Training/tree/master/Note/%E5%86%AC%E4%BB%A4%E8%90%A5/2019/d4)
3.  [Counting labeled graphs - Algorithms for Competitive Programming](https://cp-algorithms.com/combinatorics/counting_labeled_graphs.html)
4.  [Graphical Enumeration Paperback, Frank Harary, Edgar M. Palmer](https://github.com/lychees/ACM-Training/blob/master/Note/Book/)
5.  [The encyclopedia of integer sequences, N. J. A. Sloane, Simon Plouffe](https://github.com/lychees/ACM-Training/blob/master/Note/Book/The%20encyclopedia%20of%20integer%20sequences%20\(N.%20J.A.%20Sloane%2C%20Simon%20Plouffe\).pdf)
6.  [Combinatorial Problems and Exercises, László Lovász](https://github.com/lychees/ACM-Training/blob/master/Note/Book/Combinatorial%20Problems%20and%20Exercises_L%C3%A1szl%C3%B3%20Lov%C3%A1sz.pdf)
7.  [Graph Theory and Additive Combinatorics](https://yufeizhao.com/gtacbook/)
