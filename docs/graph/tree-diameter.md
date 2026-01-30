Đường đi đơn dài nhất giữa hai đỉnh bất kỳ trên cây được gọi là **đường kính** của cây.

Kiến thức nền tảng: [Cơ bản về cây](./tree-basic.md).

## Dẫn nhập

Rõ ràng, một cây có thể có nhiều đường kính, nhưng tất cả đều có cùng độ dài.

Có thể dùng hai lần DFS hoặc DP trên cây để tìm đường kính trong thời gian $O(n)$.

## Hai lần DFS

Bắt đầu từ một đỉnh bất kỳ $y$, thực hiện DFS đầu tiên để tìm đỉnh xa nhất $z$. Sau đó, từ $z$ thực hiện DFS lần hai để tìm đỉnh xa nhất $z'$. Khi đó, $\delta(z, z')$ chính là đường kính của cây.

Nếu $z$ là một đầu mút của đường kính, thì $z'$ chắc chắn là đầu mút còn lại. Ta cần chứng minh rằng, với mọi trường hợp, $z$ luôn là một đầu mút của đường kính.

Định lý: Trên một cây, bắt đầu từ bất kỳ đỉnh $y$, DFS đến đỉnh xa nhất $z$ thì $z$ luôn là một đầu mút của đường kính.

???+ note "Chứng minh"
    Dùng phản chứng. Gọi đỉnh xuất phát là $y$. Giả sử đường kính thực sự là $\delta(s, t)$, nhưng DFS từ $y$ lại đến đỉnh xa nhất $z$ mà $z$ không phải $s$ hoặc $t$. Xét ba trường hợp:
    
    -   Nếu $y$ nằm trên $\delta(s, t)$:
    
    ![y 在 s-t 上](./images/tree-diameter1.svg)
    
    Có $\delta(y,z) > \delta(y,t) \Longrightarrow \delta(x,z) > \delta(x,t) \Longrightarrow \delta(s,z) > \delta(s,t)$, mâu thuẫn với giả thiết $\delta(s,t)$ là đường kính.
    
    -   Nếu $y$ không nằm trên $\delta(s, t)$, và $\delta(y,z)$ có chung đoạn với $\delta(s,t)$:
    
    ![y 不在 s-t 上，y-z 与 s-t 存在重合路径](./images/tree-diameter2.svg)
    
    Có $\delta(y,z) > \delta(y,t) \Longrightarrow \delta(x,z) > \delta(x,t) \Longrightarrow \delta(s,z) > \delta(s,t)$, mâu thuẫn với giả thiết.
    
    -   Nếu $y$ không nằm trên $\delta(s, t)$, và $\delta(y,z)$ không có đoạn chung với $\delta(s,t)$:
    
    ![y 不在 s-t 上，y-z 与 s-t 不存在重合路径](./images/tree-diameter3.svg)
    
    Có $\delta(y,z) > \delta(y,t) \Longrightarrow \delta(x',z) > \delta(x',t) \Longrightarrow \delta(x,z) > \delta(x,t) \Longrightarrow \delta(s,z) > \delta(s,t)$, mâu thuẫn với giả thiết.
    
    Vậy cả ba trường hợp đều dẫn đến mâu thuẫn, định lý được chứng minh.

???+ warning "Cạnh có trọng số âm"
    Chứng minh trên chỉ đúng khi mọi cạnh đều không âm. Nếu cây có cạnh âm, không thể dùng hai lần DFS để tìm đường kính.

Nếu cần truy vết các đỉnh trên đường kính, chỉ cần lưu lại đỉnh cha trong lần DFS thứ hai, rồi lần ngược lại từ một đầu mút.

## DP trên cây

### Phương pháp 1

Gán gốc là $1$, với mỗi đỉnh, lưu $d_1$ là độ dài xích dài nhất đi xuống, $d_2$ là xích dài nhì (không trùng cạnh với $d_1$). Đường kính là giá trị lớn nhất của $d_1 + d_2$ trên toàn bộ các đỉnh.

DP trên cây có thể áp dụng cả khi có cạnh âm.

Nếu cần truy vết các đỉnh trên đường kính, khi tính $d_1, d_2$ nên lưu lại con tương ứng, rồi lần lượt đi theo hai nhánh này.

### Phương pháp 2

Có thể dùng chỉ một mảng. Đặt $dp[u]$ là độ dài xích dài nhất từ $u$ đi xuống. Khi duyệt các con $v$ của $u$, cập nhật $dp[u] = \max(dp[u], dp[v] + w(u, v))$.

Đường kính là giá trị lớn nhất của $dp[u] + dp[v] + w(u, v)$ trên mọi cặp $(u, v)$ cha-con.

## Bài tập ví dụ

???+ example "[Luogu B4016 Đường kính của cây](https://www.luogu.com.cn/problem/B4016)"
    Cho cây $n$ đỉnh, hãy tính độ dài đường kính. $1\leq n\leq 10^5$.

??? note "Cài đặt hai lần DFS"
    ```cpp
    --8<-- "docs/graph/code/tree-diameter/tree-diameter_1.cpp"
    ```

??? note "Cài đặt DP trên cây với hai mảng"
    ```cpp
    --8<-- "docs/graph/code/tree-diameter/tree-diameter_2.cpp"
    ```

??? note "Cài đặt DP trên cây với một mảng"
    ```cpp
    --8<-- "docs/graph/code/tree-diameter/tree-diameter_3.cpp"
    ```

## Tính chất

Đường kính của cây có tính chất: nếu mọi cạnh đều có trọng số dương, thì trung điểm của mọi đường kính đều trùng nhau.

???+ note "Chứng minh"
    Dùng phản chứng. Giả sử có hai đường kính $\delta(s,t)$ và $\delta(s',t')$ có trung điểm khác nhau, gọi là $x$ và $x'$. Rõ ràng, $\delta(s,x) = \delta(x,t) = \delta(s',x') = \delta(x',t')$.
    
    ![无负权边的树所有直径的中点重合](./images/tree-diameter4.svg)
    
    Có $\delta(s,t') = \delta(s,x) + \delta(x,x') + \delta(x',t') > \delta(s,x) + \delta(x,t) = \delta(s,t)$, mâu thuẫn với giả thiết.

## Bài tập

-   [CodeChef, Diameter of Tree](https://www.codechef.com/problems/DTREE)
-   [Educational Codeforces Round 35, Problem F, Tree Destruction](https://codeforces.com/contest/911/problem/F)
-   [ZOJ 3820 Building Fire Stations](https://pintia.cn/problem-sets/91827364500/exam/problems/type/7?problemSetProblemId=91827369872&page=28)
-   [CEOI2019/CodeForces 1192B. Dynamic Diameter](https://codeforces.com/contest/1192/problem/B)
-   [ICPC 2019 Shanghai Lightning Routing I](https://vjudge.net/problem/%E8%AE%A1%E8%92%9C%E5%AE%A2-A2290)
-   [NOIP2007 Nâng cao: Lõi mạng cây](https://www.luogu.com.cn/problem/P1099)
-   [SDOI2011 Phòng cháy chữa cháy](https://www.luogu.com.cn/problem/P2491)
-   [APIO2010 Tuần tra](https://www.luogu.com.cn/problem/P3629)
