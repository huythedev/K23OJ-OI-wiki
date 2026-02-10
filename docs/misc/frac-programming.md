author: greyqz, Ir1d, hsfzLZH1, huaruoji, banglee13

Quy hoạch phân số dùng để tìm cực trị của một phân thức. Mô tả hình thức: cho $a_i$ và $b_i$, tìm một bộ $w_i\in\{0,1\}$ để tối thiểu hóa hoặc tối đa hóa

$$
\displaystyle\frac{\sum\limits_{i=1}^na_i\times w_i}{\sum\limits_{i=1}^nb_i\times w_i}
$$

Hiểu nôm na: mỗi vật có hai trọng số $a$ và $b$, chọn một số vật sao cho $\displaystyle\frac{\sum a}{\sum b}$ nhỏ nhất hoặc lớn nhất.

Các bài quy hoạch phân số thường có ràng buộc đặc biệt, ví dụ “mẫu số ít nhất là $W$”.

## Giải

### Chia đôi đáp án

Cách chung là chia đôi đáp án. Giả sử đáp án hiện tại là $\textit{mid}$, thì một bộ $\{w_i\}$ hợp lệ sẽ khiến tỉ số không nhỏ hơn $\textit{mid}$. Lập bất đẳng thức và biến đổi:

$$
\displaystyle
\begin{aligned}
&\frac{\sum a_i\times w_i}{\sum b_i\times w_i}\ge mid\\
\Longrightarrow&\sum a_i\times w_i-mid\times \sum b_i\cdot w_i\ge 0\\
\Longrightarrow&\sum w_i\times(a_i-mid\times b_i)\ge 0
\end{aligned}
$$

Chỉ cần tìm giá trị lớn nhất của vế trái. Nếu lớn hơn $0$ thì $mid$ khả thi, nếu không thì không. Khó nhất là làm sao tính $\displaystyle \sum w_i\times(a_i-mid\times b_i)$ max/min.

### Thuật toán Dinkelbach

Thuật toán Dinkelbach[^note1] có ý tưởng: mỗi lần lấy đáp án vòng trước làm $L$ mới, lặp cho tới khi hội tụ.

## Ví dụ

???+ example "[LOJ 149 01 分数规划](https://loj.ac/p/149)"
    Có $n$ vật, mỗi vật có hai trọng số $a$ và $b$. Tìm $w_i\in\{0,1\}$ sao cho đúng $k$ giá trị là $1$, tối đa hóa $\displaystyle\frac{\sum a_i\times w_i}{\sum b_i\times w_i}$.

??? note "Lời giải"
    Lấy $a_i-mid\times b_i$ làm trọng số của vật $i$, tham lam chọn $k$ vật có trọng số lớn nhất. Nếu tổng > 0 thì khả thi, ngược lại không khả thi.

??? note "参考代码"
    ```cpp
    --8<-- "docs/misc/code/frac-programming/frac-1.cpp"
    ```

???+ example "[洛谷 4377 Talent Show G](https://www.luogu.com.cn/problem/P4377)"
    Có $n$ vật, mỗi vật có hai trọng số $a$ và $b$.
    
    Cần chọn $w_i\in\{0,1\}$ để tối đa hóa $\displaystyle\frac{\sum w_i\times a_i}{\sum w_i\times b_i}$.
    
    Yêu cầu $\displaystyle\sum w_i\times b_i \geq W$.

??? note "Lời giải"
    Bài này thêm ràng buộc mẫu số ít nhất $W$, nên không thể dùng tham lam như trên.
    
    Dùng ba lô 0/1. Lấy $b_i$ làm trọng lượng, $a_i-mid\times b_i$ làm giá trị, bài toán thành ba lô. Khi đó $dp[n][W]$ là giá trị lớn nhất.
    
    Trong DP, trọng lượng có thể vượt $W$, khi đó coi như $W$.

??? note "参考代码"
    ```cpp
    --8<-- "docs/misc/code/frac-programming/frac-2.cpp"
    ```

???+ example "[POJ2728 Desert King](http://poj.org/problem?id=2728)"
    Mỗi cạnh có hai trọng số $a_i$ và $b_i$, tìm cây khung $T$ sao cho $\displaystyle\frac{\sum_{e\in T}a_e}{\sum_{e\in T}b_e}$ nhỏ nhất.

??? note "Lời giải"
    Lấy $a_i-mid\times b_i$ làm trọng số mỗi cạnh, khi đó cây khung nhỏ nhất là nghiệm. Do đồ thị đầy đủ, dùng Prim.

??? note "参考代码"
    ```cpp
    --8<-- "docs/misc/code/frac-programming/frac-3.cpp"
    ```

???+ example "[\[HNOI2009\] 最小圈](https://www.luogu.com.cn/problem/P3199)"
    Mỗi cạnh có trọng số $w$, tìm chu trình $C$ sao cho $\displaystyle\frac{\sum_{e\in C}w}{|C|}$ nhỏ nhất.

??? note "Lời giải"
    Lấy $a_i-mid$ làm trọng số cạnh, chu trình có tổng nhỏ nhất là nghiệm.
    
    Vì chỉ cần kiểm tra nhỏ hơn $0$, nên chỉ cần phát hiện chu trình âm.
    
    Bài này có thuật toán $O(nm)$; nếu quan tâm xem [bài viết này](https://www.cnblogs.com/y-clever/p/7043553.html).

??? note "参考代码"
    ```cpp
    --8<-- "docs/misc/code/frac-programming/frac-4.cpp"
    ```

## Bài tập

-   [JSOI2016 最佳团体](https://loj.ac/problem/2071)
-   [SDOI2017 新生舞会](https://loj.ac/problem/2003)
-   [UVa1389 Hard Life](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=4135)
-   [洛谷 P2868 \[USACO07DEC\] Sightseeing Cows G](https://www.luogu.com.cn/problem/P2868)
-   [AtCoder Beginner Contest 324 F - Beautiful Path](https://atcoder.jp/contests/abc324/tasks/abc324_f)

## Tài liệu tham khảo & chú thích

[^note1]: [Dinkelbach, Werner. "On nonlinear fractional programming." Management science 13.7 (1967): 492-498.](https://doi.org/10.1287/mnsc.13.7.492)
