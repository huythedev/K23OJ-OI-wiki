## Giới thiệu

Lý thuyết thứ tự là nhánh toán học dùng quan hệ hai ngôi để hình thức hóa khái niệm “thứ tự”. Dưới đây là các định nghĩa cơ bản.

## Định nghĩa

### Quan hệ hai ngôi

???+ note "Định nghĩa"
    Một **quan hệ hai ngôi** (binary relation) $R$ trên tập $X$ và $Y$ được định nghĩa là bộ $(X,Y,G(R))$, trong đó $X$ là miền xác định (domain), $Y$ là miền đối (codomain), và $G(R)\subseteq X\times Y=\{(x,y):x\in X,y\in Y\}$ là đồ thị (graph) của $R$. Ta viết $xRy$ khi và chỉ khi $(x,y)\in G(R)$.
    
    Nếu $X=Y$ thì gọi là quan hệ hai ngôi đồng nhất (homogeneous relation) hay quan hệ nội (endorelation).
    
    Nếu không nói khác, các quan hệ sau đây đều là quan hệ đồng nhất.

Ví dụ: quan hệ chia hết $\mid$ và quan hệ $\leq$ trên $\mathbf{N}_+$ đều là quan hệ hai ngôi.

Khi nghiên cứu quan hệ, ta quan tâm đến các tính chất:

1.  Phản xạ (reflexive): $(\forall~a \in S)~~aRa$,
2.  Phản phản xạ (irreflexive, anti-reflexive): $(\forall~a \in S)~~\lnot(aRa)$,
3.  Đối xứng (symmetric): $(\forall~a,b \in S)~~aRb \iff bRa$,
4.  Phản đối xứng (antisymmetric): $(\forall~a,b \in S)~~(aRb \land bRa) \implies a=b$,
5.  Bất đối xứng (asymmetric): $(\forall~a,b \in S)~~aRb \implies \lnot(bRa)$,
6.  Bắc cầu (transitive): $(\forall~a,b,c \in S)~~(aRb \land bRc) \implies aRc$,
7.  Liên thông (connected): $(\forall~a,b \in S)~~a \neq b \implies (aRb \lor bRa)$,
8.  Cơ sở tốt (well-founded): $(\exists~m \in S \neq \varnothing)~~(\forall~a \in S\setminus\{m\})~~\lnot(aRm)$ (tồn tại phần tử tối tiểu trong mọi tập con không rỗng),
9.  Bắc cầu của không so sánh (transitive of incomparability): $(\forall~a,b,c \in S)~~(\lnot(aRb \lor bRa) \land \lnot(bRc \lor cRb)) \implies \lnot(aRc \lor cRa)$ (nếu $\lnot(aRb \lor bRa)$ thì gọi $a$ và $b$ không so sánh được).

Một số loại quan hệ đặc biệt:

| Quan hệ                     | Phản xạ | Phản phản xạ | Đối xứng | Phản đối xứng | Bất đối xứng | Bắc cầu | Liên thông | Cơ sở tốt | Bắc cầu của không so sánh |
| -------------------------- | --- | ---- | --- | ---- | ---- | --- | --- | --- | ------- |
| Tương đương (equivalence)  | Có  |      | Có  |      |      | Có  |     |     |         |
| Tiền thứ tự (preorder)     | Có  |      |     |      |      | Có  |     |     |         |
| Thứ tự bộ phận (partial)   | Có  |      |     | Có   |      | Có  |     |     |         |
| Thứ tự toàn phần (total)   | Có  |      |     | Có   |      | Có  | Có  |     |         |
| Thứ tự tốt (well-order)    | Có  |      |     | Có   |      | Có  | Có  | Có  |         |
| Tiền thứ tự nghiêm         |     | Có   |     |      |      | Có  |     |     |         |
| Thứ tự bộ phận nghiêm      |     | Có   |     |      | Có   | Có  |     |     |         |
| Thứ tự yếu nghiêm          |     | Có   |     |      | Có   | Có  |     |     | Có       |
| Thứ tự toàn phần nghiêm    |     | Có   |     |      | Có   | Có  | Có  |     |         |

### Phép toán giữa các quan hệ

Cho quan hệ $R,S$ trên $X,Y$:

1.  Hợp $R\cup S$: $G(R\cup S):=\{(x,y):xRy \lor xSy\}$ (ví dụ $\leq$ là hợp của $<$ và $=$),
2.  Giao $R\cap S$: $G(R\cap S):=\{(x,y):xRy \land xSy\}$,
3.  Bổ sung $\bar{R}$: $G(\bar{R}):=\{(x,y):\lnot(xRy)\}$,
4.  Đối ngẫu $R^T$: $G(R^T):=\{(y,x):xRy\}$.

Cho quan hệ $R$ trên $X,Y$ và $S$ trên $Y,Z$, hợp thành $S\circ R$ có:

$G(S\circ R):=\{(x,z):(\exists~y\in Y)~~xRy\land ySz\}$.

### Tập có thứ tự bộ phận

???+ note "Định nghĩa"
    Nếu quan hệ $\preceq$ trên $S$ có **phản xạ**, **phản đối xứng**, **bắc cầu**, thì $S$ là **tập có thứ tự bộ phận** (poset), $\preceq$ là **thứ tự bộ phận**.
    
    Nếu $\preceq$ còn **liên thông**, thì là **thứ tự toàn phần**; $S$ là **tập có thứ tự toàn phần** (totally ordered set), còn gọi là **linearly ordered set** hay **simply ordered set**.

Ta có $\mathbf{N}$, $\mathbf{Z}$, $\mathbf{Q}$, $\mathbf{R}$ đều là toàn phần theo $\leq$.

### Biểu diễn trực quan: Hasse diagram

Với poset hữu hạn, có thể dùng Hasse diagram.

???+ note "Định nghĩa"
    Với poset hữu hạn $S$ và thứ tự $\preceq$, đặt $x\prec y\iff (x\preceq y\land x\neq y)$. **Hasse diagram** là đồ thị $G=\langle V,E\rangle$ thỏa:
    
    -   $V=S$,
    -   $E=\{(x,y)\in S\times S: x\prec y \land ((\nexists~z\in S)~~x\prec z\prec y)\}$

Ví dụ: với $S$ là lũy thừa của $\{0,1,2\}$ và $\subseteq$, Hasse diagram:

![](images/order-theory1.svg)

Do phản đối xứng, Hasse diagram là [DAG](../graph/dag.md). Có thể dùng [topo sort](../graph/topo.md) để tạo thứ tự toàn phần.

### Chuỗi và phản chuỗi

???+ note "Định nghĩa"
    Với poset $S$ và $\preceq$, tập con toàn phần gọi là **chuỗi** (chain). Nếu $T\subseteq S$ mà mọi cặp khác nhau đều không so sánh được thì $T$ là **phản chuỗi** (antichain).
    
    Độ dài phản chuỗi lớn nhất gọi là **độ rộng** (width) của poset.

Ví dụ: với $S$ là lũy thừa của $\{0,1,2\}$, $\{\varnothing,\{1\},\{1,2\}\}$ là chuỗi, $\{\{1\},\{0,2\}\}$ là phản chuỗi, độ rộng là $3$.

### Phần tử đặc biệt trong tiền thứ tự

Trong tiền thứ tự, ta có phần tử cực đại/cực tiểu, cận trên/dưới, cận trên/dưới đúng.

???+ note "Định nghĩa"
    Với tiền thứ tự $S$ và $\preceq$, phần tử $m$:
    
    1.  Nếu $(\forall~a \in S\setminus\{m\})~~\lnot(m\preceq a)$ thì $m$ là **cực đại**,
    2.  Nếu với $T \subseteq S$, $(\forall~t\in T)~~t\preceq m$ thì $m$ là **cận trên** của $T$,
    3.  Nếu $m$ là cận trên và mọi cận trên $n$ đều thỏa $m \preceq n$ thì $m$ là **cận trên đúng** (supremum).
    
    Tương tự định nghĩa cực tiểu, cận dưới, cận dưới đúng.

Ví dụ: $1$ là cực tiểu và cận dưới của $\mathbf{N}_+$.

Có thể chứng minh:

-   Trong tiền thứ tự, cực đại/cực tiểu, cận trên/dưới, cận trên/dưới đúng không nhất thiết tồn tại, và nếu tồn tại cũng không nhất thiết duy nhất.
-   Trong poset, nếu cận trên/dưới đúng tồn tại thì duy nhất.

Ký hiệu $\sup T$, $\inf T$. Nếu poset có cả cận trên và cận dưới thì gọi là **có bị chặn**.

Trong poset vô hạn, cực đại có thể không tồn tại. Dùng **Bổ đề Zorn** để xét.

???+ note "[Bổ đề Zorn](https://en.wikipedia.org/wiki/Zorn%27s_lemma)"
    Nếu mọi chuỗi của một poset không rỗng đều có cận trên, thì poset đó có cực đại.

Bổ đề Zorn tương đương với **[tiên đề lựa chọn](https://en.wikipedia.org/wiki/Axiom_of_choice)** và **[định lý sắp thứ tự tốt](https://en.wikipedia.org/wiki/Well-ordering_theorem)**.

### Tập có hướng và lattice

Trong poset, cực đại/cực tiểu không nhất thiết duy nhất. Ta thêm điều kiện để có khái niệm tối đại/tối tiểu duy nhất.

???+ note "Tập có hướng"
    Với tiền thứ tự $S$ và $\preceq$, nếu $(\forall~a,b\in S)~~(\exists~c\in S)~~a\preceq c\land b\preceq c$ thì $\preceq$ là một **hướng**, $S$ là **tập có hướng** (directed set) hay **filtered set**.
    
    Tương tự định nghĩa tập có hướng xuống.

Tương đương:

???+ note "Định nghĩa tương đương"
    Với tiền thứ tự $S$ và $\preceq$, nếu mọi tập con hữu hạn $T$ đều có cận trên, thì $S$ là tập có hướng.

Nhận xét:

-   Nếu tập có hướng lên có cực đại thì duy nhất, gọi là **phần tử lớn nhất**.
-   Nếu tập có hướng xuống có cực tiểu thì duy nhất, gọi là **phần tử nhỏ nhất**.

Trong poset có hướng, nếu mọi cặp $\{a,b\}$ có cận trên đúng, ta có nửa lattice.

???+ note "Join-semilattice"
    Nếu với mọi $a,b$ tồn tại cận trên đúng $c$, thì $S$ là **join-semilattice**, và $c$ là **join**, ký hiệu $a\lor b$.

???+ note "Meet-semilattice"
    Nếu với mọi $a,b$ tồn tại cận dưới đúng $c$, thì $S$ là **meet-semilattice**, và $c$ là **meet**, ký hiệu $a\land b$.

???+ note "Lattice"
    Nếu $S$ vừa là join-semilattice vừa là meet-semilattice thì gọi là **lattice**.

Ví dụ: các ước dương của $60$ với quan hệ chia hết tạo poset; $\operatorname{lcm}$ là join, $\gcd$ là meet, nên là lattice.

### Đối ngẫu

Đối ngẫu rất phổ biến trong lý thuyết thứ tự: cực đại/cực tiểu, cận trên/cận dưới, $\sup/\inf$ là các cặp đối ngẫu.

Với poset $P$ và $\preceq$, **đối ngẫu** $P^d$ thỏa: $x \preceq y$ trong $P$ khi và chỉ khi $y \preceq x$ trong $P^d$. Đảo hướng cạnh Hasse diagram của $P$ được $P^d$.

## Định lý Dilworth và Mirsky

Với poset hữu hạn $S$, có cặp định lý đối ngẫu:

???+ note "Định lý Dilworth"
    Độ rộng (phản chuỗi dài nhất) bằng số phủ chuỗi nhỏ nhất.
    
    ??? note "Chứng minh"
        Dùng quy nạp. Với $|S|\leq 3$ thì hiển nhiên.
        
        Giả sử đúng với mọi poset nhỏ hơn. Gọi độ rộng $d$. Nếu mọi phần tử đều không so sánh được thì hiển nhiên. Ngược lại, lấy chuỗi dài hơn 1 với phần tử nhỏ nhất $m$, lớn nhất $M$.
        
        Đặt $T=S\setminus\{m,M\}$. Nếu độ rộng của $T$ không vượt $d-1$ thì theo giả thiết quy nạp, $T$ phủ bởi $\le d-1$ chuỗi; thêm chuỗi $\{m,M\}$ là đủ. Nếu không, độ rộng của $T$ là $d$, lấy phản chuỗi dài nhất $A$ của $T$.
        
        Xét:
        
        $$
        S^+:=\{x\in S:(\exists~a\in A)~~a\preceq x\}
        $$
        
        $$
        S^-:=\{x\in S:(\exists~a\in A)~~x\preceq a\}
        $$
        
        Ta có:
        
        -   $S^+\cup S^-=S$,
        -   $S^+\cap S^-=A$,
        -   $|S^+|<|S|$,$|S^-|<|S|$ (vì $m\notin S^+$ và $M\notin S^-$).
        
        Áp dụng quy nạp cho $S^+$ và $S^-$, ta được mỗi tập có phủ chuỗi tối thiểu $d$, và mỗi chuỗi chứa đúng một phần tử $a\in A$, ký hiệu $C_a^+, C_a^-$. Khi đó $\{C_a^-\cup\{a\}\cup C_a^+\}_{a\in A}$ là phủ chuỗi tối thiểu của $S$.

???+ note "Định lý Mirsky"
    Độ dài chuỗi dài nhất bằng số phủ phản chuỗi nhỏ nhất.
    
    ??? note "Chứng minh"
        Gọi độ dài chuỗi dài nhất là $d$, suy ra số phủ phản chuỗi nhỏ nhất $\ge d$.
        
        Đặt $f(s)$ là độ dài chuỗi dài nhất có $s$ là phần tử nhỏ nhất. Nếu $f(s)=f(t)$ thì $s$ và $t$ không so sánh được, nên $(\forall~n\in\mathbf{N})~~f^{-1}(\{n\})$ là phản chuỗi, với $f^{-1}(\{n\}):=\{a\in S:f(a)=n\}$ là [level set](https://en.wikipedia.org/wiki/Level_set).
        
        Do đó $\{f^{-1}(\{i\}):1\leq i\leq d\}$ là một phủ phản chuỗi, nên số phủ phản chuỗi nhỏ nhất $\le d$.

Dilworth tương đương với [định lý Hall](../graph/graph-matching/graph-match.md#hall-定理).

Có thể dùng Dilworth chứng minh:

???+ note "Định lý Erdős–Szekeres"
    Dãy thực $\{a_i\}$ có ít nhất $rs+1$ phần tử thì либо có dãy con không giảm dài $r+1$, либо có dãy con không tăng dài $s+1$.
    
    ??? note "Chứng minh"
        Gọi $n\geq rs+1$. Xét poset $\{(i,a_i)\}_{i=1}^{n}$ với:
        
        $$
        (i,a_i)\preceq (j,a_j)\iff (i\leq j\land a_i\leq a_j)
        $$
        
        Nếu độ rộng không vượt $s$, theo Dilworth, poset phủ bởi $\le s$ chuỗi. Nếu mỗi chuỗi dài $\le r$ thì tổng phần tử $\le rs$, mâu thuẫn.

### Bài tập

???+ note "[Luogu P1020 \[NOIP1999提高组\] Đánh chặn tên lửa](https://www.luogu.com.cn/problem/P1020)"
    Một hệ thống phòng thủ có thể bắn quả đầu tiên ở bất kỳ độ cao, nhưng các quả sau không được cao hơn quả trước. Có một hệ thống, nên có thể không chặn hết tên lửa. Cho độ cao các tên lửa đến theo thứ tự, tính số tên lửa tối đa có thể chặn, và số hệ thống tối thiểu để chặn hết.
    
    Dữ liệu: độ cao là số nguyên dương, không quá $5\times 10^4$.
    
    ??? note "Lời giải"
        Gọi $n$ là số tên lửa, độ cao $h_i$. Xét poset $\{(i,h_i)\}_{i=1}^{n}$ với:
        
        $$
        (i,h_i)\preceq(j,h_j) \iff (i\leq j \land h_i\geq h_j)
        $$
        
        Theo Dilworth: **số dãy con không tăng tối thiểu bằng độ dài LIS**. Do đó giải bằng [LIS không giảm $O(n\log n)$](../dp/basic.md#算法二).
    
    ??? note "Mã tham khảo"
        ```cpp
        --8<-- "docs/math/code/order-theory/order-theory_1.cpp"
        ```

???+ note "[\[TJOI2015\] Tổ hợp](https://www.luogu.com.cn/problem/P3974)"
    Cho lưới $n\times m$, mỗi ô có một số kho báu. Mỗi lần đi từ góc trái trên chỉ đi phải hoặc xuống, qua mỗi ô lấy tối đa 1 kho báu. Hỏi cần ít nhất bao nhiêu lần đi để lấy hết kho báu.
    
    $1\le n \le 1000$，$1\le m \le 1000$，mỗi ô không quá $10^6$.
    
    ??? note "Lời giải"
        Bỏ qua trọng số, đường đi trong lưới tương đương đường đi trong DAG, có thể xem như Hasse diagram. Theo Dilworth: **số phủ chuỗi tối thiểu của DAG bằng kích thước tập độc lập lớn nhất**.
        
        Vì vậy bài toán là tìm tổng trọng số của tập độc lập lớn nhất.
        
        Gọi $a_{ij}$ là trọng số tại $(i,j)$, $f(i,j)$ là đáp án trên ô con từ $(i,j)$ đến $(1,m)$. Mỗi điểm không kề với điểm trên-phải, nên:
        
        $$
        f(i,j)=\max\{f(i-1,j),f(i,j+1),f(i-1,j+1)+a_{ij}\}
        $$
        
        Đáp án là $f(n,1)$.
    
    ??? note "Mã tham khảo"
        ```cpp
        --8<-- "docs/math/code/order-theory/order-theory_2.cpp"
        ```

### Bài luyện tập

-   [\[CTSC2008\] 祭祀](https://www.luogu.com.cn/problem/P4298)
-   [CodeForces 590E Birthday](https://codeforces.com/problemset/problem/590/E)

## Ứng dụng trong C++

Xem thêm: [STL sắp xếp - thuật toán cơ bản](../basic/stl-sort.md).

Trong C++ STL, các [thuật toán và cấu trúc dữ liệu cần so sánh](https://en.cppreference.com/w/cpp/named_req/Compare#Standard_library) dùng lý thuyết thứ tự. Khi tự định nghĩa comparator, STL [yêu cầu](https://en.cppreference.com/w/cpp/named_req/Compare) đó là **thứ tự yếu nghiêm**. Nếu ký hiệu comparator là `<`, có thể định nghĩa:

-   $x>y$ là $y<x$;
-   $x \leq y$ là $y \nless x$;
-   $x \geq y$ là $x \nless y$;
-   $x=y$ là $x \nless y\land y \nless x$.

## Tài liệu tham khảo và đọc thêm

1.  [Order theory - From Academic Kids](https://academickids.com/encyclopedia/index.php/Order_theory)
2.  [Binary Relation - Wikipedia](https://en.wikipedia.org/wiki/Binary_relation)
3.  [Order Theory - Wikipedia](https://en.wikipedia.org/wiki/Order_theory)
4.  [Hasse diagram - Wikipedia](https://en.wikipedia.org/wiki/Hasse_diagram)
5.  [Directed set - Wikipedia](https://en.wikipedia.org/wiki/Directed_set)
6.  [Order Theory, Lecture Notes by Mark Dean for Decision Theory](http://www.columbia.edu/~md3405/DT_Order_15.pdf)
7.  卢开澄，卢华明，[《组合数学》（第 3 版）](http://www.tup.tsinghua.edu.cn/bookscenter/book_00458101.html), 2006
8.  [List of Order Theory Topics - Wikipedia](https://en.wikipedia.org/wiki/List_of_order_theory_topics)
9.  [浅谈邻项交换排序的应用以及需要注意的问题 by ouuan](https://ouuan.github.io/post/%E6%B5%85%E8%B0%88%E9%82%BB%E9%A1%B9%E4%BA%A4%E6%8D%A2%E6%8E%92%E5%BA%8F%E7%9A%84%E5%BA%94%E7%94%A8%E4%BB%A5%E5%8F%8A%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E7%9A%84%E9%97%AE%E9%A2%98/)
10. [One thing you should know about comparators—Strict Weak Ordering](https://codeforces.com/blog/entry/72525)
11. [Dilworth's theorem - Wikipedia](https://en.wikipedia.org/wiki/Dilworth%27s_theorem)
12. [Dilworth's Theorem | Brilliant Math & Science Wiki](https://brilliant.org/wiki/dilworths-theorem/)
13. [Hall's marriage theorem - Wikipedia](https://en.wikipedia.org/wiki/Hall's_marriage_theorem)
14. [Hall's Marriage Theorem | Brilliant Math & Science Wiki](https://brilliant.org/wiki/hall-marriage-theorem/)
15. [Dilworth 学习笔记 - Selfish](https://www.luogu.com.cn/blog/Rolling-Code/dilworth)
