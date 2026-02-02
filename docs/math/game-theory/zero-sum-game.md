Tiền đề: [Giới thiệu về lý thuyết trò chơi](./intro.md)

Bài viết này thảo luận (hai người) [trò chơi tổng bằng không](./intro.md#零和非零和博弈)．

Trong trò chơi tổng bằng không, tổng lợi ích của hai người chơi luôn bằng 0; lợi ích của một bên tất yếu là tổn thất của bên kia．Trò chơi tổng bằng không có thể xem là trường hợp đặc biệt của trò chơi tổng hằng．Tuy nhiên, mọi trò chơi tổng hằng đều có thể chuyển tương đương về trò chơi tổng bằng không bằng cách cộng/trừ một hằng số vào lợi ích của một bên, nên chỉ cần thảo luận trò chơi tổng bằng không．

Trong thi đấu thuật toán, trò chơi tổng bằng không thường gặp chia thành hai loại: trò chơi tuần tự và trò chơi đồng thời．

## Trò chơi tổng bằng không tuần tự

Trong trò chơi tuần tự, hai người luân phiên hành động đến khi trò chơi kết thúc．

Trong trò chơi tuần tự, hàm lợi ích có cấu trúc đệ quy．Thế cờ $S$ chia thành ba loại: kết thúc $S_0$, lượt người chơi $1$ $S_1$, và lượt người chơi $2$ $S_2$．Giả sử ở thế kết thúc $s\in S_0$, lợi ích của người chơi $1$ là $v(s)$, thì lợi ích của người chơi $2$ là $-v(s)$．Do đó, khi đến lượt người chơi $2$, tối đa hóa lợi ích của họ tương đương với tối thiểu hóa lợi ích của người chơi $1$．Vì vậy, nếu cả hai tối ưu, lợi ích tối đa mà người chơi $1$ đạt được ở $s\in S$ thỏa:

$$
V(s) = \begin{cases}
v(s), & s \in S_0,\\
\max_{t\in s} V(t), & s\in S_1,\\
\min_{t\in s} V(t), & s\in S_2.
\end{cases}
$$

Trong đó, $t\in s$ nghĩa là $t$ là thế kế tiếp của $s$．Đây là [ý tưởng minimax](../../search/alpha-beta.md#minimax-算法)．

Áp dụng thuật toán này vào bài toán thực tế, thường có các cách：

-   Nếu số thế cờ ít, có thể vét cạn trực tiếp．

-   Nếu số thế cờ lớn và không có cấu trúc đặc biệt, có thể dùng [cắt tỉa Alpha–Beta](../../search/alpha-beta.md#alphabeta-剪枝) kết hợp các kỹ thuật khác．

-   Nếu một thế cờ thường là hậu thế của nhiều thế cờ khác, để tránh lặp, dùng memo hoặc DP．

-   Nếu lợi ích cuối cùng là tổng lợi ích của các hành động trước đó, có thể tối ưu mô hình．Cụ thể, giả sử đến kết thúc $s\in S_0$，chuỗi hành động của người chơi $i=1,2$ là $\{a^{(i)}_j\}_{j=1}^{k_i}$, hành động $a$ có lợi ích $w(a)$, thì lợi ích người chơi $1$ là

    $$
    v(s) = \sum_{j=1}^{k_1}w(a_j^{(1)}) - \sum_{j=1}^{k_2}w(a_j^{(2)}).
    $$

    Khi đó, đặt $\tilde V(s)$ là điểm tối đa mà người chơi hiện tại có thể đạt được trong phần còn lại của trò chơi từ $s\in S$．Với trạng thái đầu $s_0$，$V(s_0)=\tilde V(s_0)$, nên tính $\tilde V(\cdot)$ là đủ．Ta có：

    $$
    \tilde V(s) = \begin{cases}
    0, & s \in S_0, \\
    \max_{t\in s} w(a_{s\to t}) - \tilde V(t), & s\in S_1\cup S_2.
    \end{cases}
    $$

    Trong đó $a_{s\to t}$ là hành động đưa $s$ sang $t$；nếu có nhiều, chọn hành động có $w(a)$ lớn nhất．

-   Trò chơi tổ hợp công bằng đều là trò chơi tuần tự tổng bằng không, chỉ cần đặt lợi ích của người thắng/ thua là $+1/-1$．Khi đó, công thức của $V(\cdot)$ chính là [bổ đề判定 thắng/thua](./impartial-game.md#博弈图和状态)．

    Dạng biến thể thường gặp là hỏi số lượt ít nhất người thắng cần và số lượt nhiều nhất người thua có thể cầm cự．Để làm vậy, chỉ cần bắt đầu BFS từ trạng thái kết thúc và theo bổ đề判定 thắng/thua, đồng thời ghi lại số vòng BFS khi判定 trạng thái thắng/thua；đó chính là số lượt cần tìm．Vì một trạng thái thắng chỉ cần một hậu thế thua (nhận từ hậu thế thua có số lượt nhỏ nhất), còn một trạng thái thua cần mọi hậu thế thắng (nhận từ hậu thế thắng có số lượt lớn nhất)．

    Phương pháp này cũng mở rộng cho [trò chơi đồ thị có hướng](./impartial-game.md#有向图游戏)．

### Ví dụ

???+ example "[Codeforces 794 E. Choosing Carrot](https://codeforces.com/problemset/problem/794/E)"
    Có dãy dài $n$ ${a_i}$．Hai người 1 và 2 luân phiên lấy một số ở hai đầu dãy cho đến khi chỉ còn một số．Người chơi 1 muốn tối đa hóa số còn lại, người chơi 2 muốn tối thiểu hóa nó．Trước khi bắt đầu, người chơi 1 có thể đi trước $k$ bước．Giả sử hai người đều tối ưu．Với mỗi $k = 0,1,2,\cdots,n-1$，hãy tính số cuối cùng．$1 \le n \le 3\times 10^5$．

??? note "Lời giải"
    Vì sau mỗi lượt, phần còn lại luôn là một đoạn liên tiếp, nên thế cờ chỉ bởi đoạn $[l,r]$ và người chơi $i=1,2$．Có thể dùng DP．Đặt $f(l,r,i)$ là số cuối cùng khi thế cờ là $(l,r,i)$．Khi $l < r$，ta có：
    
    $$
    \begin{aligned}
    f(l,r,1) &= \max\{f(l+1,r,2),f(l,r-1,2)\},\\
    f(l,r,2) &= \min\{f(l+1,r,1),f(l,r-1,1)\}.
    \end{aligned}
    $$
    
    Điều kiện biên $f(l,l,1)=f(l,l,2) = a_l$．Từ đó tính được mọi giá trị trong $\Theta(n^2)$．Với mỗi $k$，đáp án là
    
    $$
    g(k) = \max f(l,r,1) \text{ subject to } r - l + 1 = k.
    $$
    
    Thuật toán này quá chậm, cần tối ưu chuyển trạng thái．Có nhiều cách, bài viết chỉ nêu một cách．
    
    Nhìn chuyển trạng thái như thao tác trên cả dãy．Hai phương trình tương ứng với thao tác lấy max của hai phần tử kề và thao tác lấy min của hai phần tử kề, gọi là "tối đa hóa" và "tối thiểu hóa"．Mỗi lần thao tác làm độ dài dãy giảm 1．Mọi đoạn dài $d$ tương ứng với $(n-d+1)$ kết quả, tức là dãy sau $(d-1)$ lần thao tác．Để得到 $f(l,r,1)$，lần thao tác cuối phải là tối đa hóa．Vì vậy các chuỗi thao tác luôn kết thúc bằng tối đa hóa．
    
    Xét hai thao tác liên tiếp．Giả sử tối thiểu hóa rồi tối đa hóa．Khi đó dãy $a_1,a_2,a_3$ thành
    
    $$
    \max\{\min\{a_1,a_2\},\min\{a_2,a_3\}\}.
    $$
    
    Liệt kê quan hệ大小 giữa $a_1,a_2,a_3$ ta thấy, ngoài trường hợp $a_1 < a_2$ và $a_2 > a_3$ (đỉnh cực đại nghiêm ngặt), biểu thức luôn bằng $a_2$．Tức là nếu dãy không có điểm cực đại nghiêm ngặt, thì hai thao tác liên tiếp chỉ đơn giản loại bỏ hai đầu dãy．Điều này giảm đáng kể chuyển trạng thái．Vấn đề còn lại: làm sao đảm bảo dãy không có điểm cực đại nghiêm ngặt？Thực tế, chỉ cần một lần tối đa hóa là đủ để không còn điểm cực đại nghiêm ngặt．Do đó, kết quả sau số lần thao tác chẵn có thể lấy từ dãy sau hai thao tác đầu, rồi lần lượt bỏ hai đầu；kết quả sau số lần thao tác lẻ lấy từ dãy sau một thao tác đầu, rồi lần lượt bỏ hai đầu．
    
    Vì tổng số thao tác đầy đủ最多 chỉ 3 lần, còn thống kê đáp án chỉ cần 2 lượt, nên tổng độ phức tạp là $\Theta(n)$．

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/zero-sum-game/zero-sum-game-1.cpp"
    ```

### Bài tập

-   [Luogu P2734 \[USACO3.3\] 游戏 A Game](https://www.luogu.com.cn/problem/P2734)
-   [Luogu P4576 \[CQOI2013\] 棋盘游戏](https://www.luogu.com.cn/problem/P4576)
-   [Luogu P7097 \[yLOI2020\] 牵丝戏](https://www.luogu.com.cn/problem/P7097)
-   [Codeforces 388 C. Fox and Card Game](https://codeforces.com/problemset/problem/388/C)
-   [Codeforces 794 E. Choosing Carrot](https://codeforces.com/problemset/problem/794/E)
-   [Codeforces 1628 D2. Game on Sum (Hard Version)](https://codeforces.com/problemset/problem/1628/D2)
-   [Luogu P3210 \[HNOI2010\] 取石头游戏](https://www.luogu.com.cn/problem/P3210)

## Trò chơi tổng bằng không đồng thời

Trong trò chơi đồng thời, hai người chơi hành động cùng lúc．

Trò chơi tổng bằng không đồng thời thường dùng ma trận lợi ích．Giả sử tập hành động của người chơi $i=1,2$ là $A_i$，và khi họ chọn $a_i\in A_i$ thì lợi ích lần lượt là $v(a_1,a_2)$ và $-v(a_1,a_2)$．

???+ example "Ví dụ"
    Xét kéo-búa-bao．Giả sử thắng $1$ điểm, thua $-1$, hòa $0$．Khi đó lợi ích có thể biểu diễn：
    
    $$
    \begin{pmatrix}
    0,0 & 1,-1 & -1,1 \\
    -1,1 & 0,0 & 1,-1 \\
    1,-1 & -1,1 & 0,0
    \end{pmatrix}.
    $$
    
    Trò chơi đồng thời hai người bất kỳ đều có dạng này, còn gọi là [trò chơi song ma trận](https://en.wikipedia.org/wiki/Bimatrix_game)（bimatrix game）．Với trò chơi tổng bằng không, ma trận của người chơi 1 và 2 là đối nhau, nên chỉ cần xét ma trận của người chơi 1：
    
    $$
    V = (v(a_1,a_2))_{(a_1,a_2)\in A_1\times A_2} = \begin{pmatrix}
    0 & 1 & -1 \\
    -1 & 0 & 1 \\
    1 & -1 & 0
    \end{pmatrix}.
    $$

Vấn đề: cho ma trận $V = (v(a_1,a_2))_{(a_1,a_2)\in A_1\times A_2}$，làm sao tìm chiến lược tối ưu và lợi ích tối đa của hai người?

### Chiến lược hỗn hợp

Trong trò chơi đồng thời, hai người đối xứng．Nhưng vì đã giải trò chơi tuần tự, ta xét phiên bản tuần tự của trò chơi đồng thời．Nếu người chơi 1 đi trước, người chơi 2 đi sau, thì theo phân tích, lợi ích của người chơi 1 là

$$
w_-=\max_{a_1\in A_1}\min_{a_2\in A_2} v(a_1,a_2)
$$

Do người chơi 2 quan sát được hành động của người chơi 1, đây là kết quả tệ nhất với người chơi 1．Ngược lại, nếu người chơi 2 đi trước, lợi ích người chơi 1 là

$$
w_+ = \min_{a_2\in A_2}\max_{a_1\in A_1} v(a_1,a_2)
$$

Đây là kết quả tốt nhất với người chơi 1．Do đó trong trò chơi thực tế, người chơi 1 mong đợi lợi ích $w\in[w_-,w_+]$．Dù luôn có $w_-\le w_+$ (xem [định lý đối偶 yếu](../linear-programming.md#对偶原理)), nhưng không必然 bằng nhau, nên chỉ dùng phân tích tuần tự thì không xác định duy nhất kết quả．

???+ example "Ví dụ (tiếp)"
    Trong kéo-búa-bao, nếu có thứ tự đi, người đi trước必输, người đi sau必 thắng. Toán học:
    
    $$
    w_- = -1 \le +1 = w_+.
    $$
    
    Ở đây $w_-\neq w_+$．

Điểm then chốt của trò chơi đồng thời là người chơi không thể dự đoán chính xác hành động đối thủ．Do đó cần chiến lược ngẫu nhiên．Trong trò chơi tuần tự, ngẫu nhiên không giúp vì đối thủ quan sát được, còn trong đồng thời, ngẫu nhiên tạo ra mơ hồ chiến lược khiến đối thủ không thể phản công hiệu quả．

???+ example "Ví dụ (tiếp)"
    Trong kéo-búa-bao, nếu người chơi 1 chọn đều kéo/đá/bao, thì theo lựa chọn của người chơi 2, lợi ích kỳ vọng của người chơi 1 là
    
    $$
    \dfrac{1}{3}(0,1,-1)^T + \dfrac{1}{3}(-1,0,1)^T + \dfrac{1}{3}(1,-1,0)^T = (0,0,0)^T.
    $$
    
    Lúc này, bất kể người chơi 2 làm gì, lợi ích kỳ vọng luôn $0$，tốt hơn việc chọn một hành động cố định．

Từ đó có khái niệm chiến lược hỗn hợp．

???+ abstract "Chiến lược hỗn hợp"
    Trong trò chơi đồng thời, **chiến lược hỗn hợp** (mixed strategy) của người chơi $i$ là hàm $s_i:A_i\to[0,1]$ thỏa $\sum_{a_i\in A_i}s_i(a_i)=1$．Tức $s_i$ là một phân phối xác suất trên tập hành động $A_i$．Tập tất cả chiến lược hỗn hợp của người chơi $i$ ký hiệu $S_i=\Delta(A_i)$, với $\Delta(A_i)$ là tập tất cả phân phối xác suất trên $A_i$．Nếu $s_i$ là phân phối suy biến, tức tồn tại $a\in A_i$ sao cho $s_i(a)=1$, thì gọi $s_i$ là **chiến lược thuần** (pure strategy)．

Lợi ích của chiến lược hỗn hợp là kỳ vọng của lợi ích hành động đơn：

$$
v(s_1,s_2) = \sum_{a_1\in A_1}\sum_{a_2\in A_2}s_1(a_1)s_2(a_2)v(a_1,a_2).
$$

Xem hành động đơn là chiến lược thuần, ta nhúng $A_i$ vào $S_i$, và $v(s_1,s_2)$ là phép kéo dài $v(a_1,a_2)$ từ $A_1\times A_2$ lên $S_1\times S_2$．

### Định lý von Neumann

Khi cho phép chiến lược hỗn hợp, minimax và maximin trở thành一致, nên kết quả của trò chơi đồng thời được xác định duy nhất．

???+ note "Định lý (von Neumann)"
    Trong trò chơi tổng bằng không cho phép chiến lược hỗn hợp, nếu cả hai tối ưu, lợi ích tối đa của người chơi $1$ là
    
    $$
    w = \max_{s_1\in S_1}\min_{s_2\in S_2} v(s_1,s_2) = \min_{s_2\in S_2}\max_{s_1\in S_1} v(s_1,s_2),
    $$
    
    và lợi ích tối đa của người chơi $2$ là $-w$．

??? note "Chứng minh"
    Đặt $w = \max_{s_1\in S_1}\min_{s_2\in S_2} v(s_1,s_2)$．Xét bài toán tối thiểu hóa bên trong, vì $v(s_1,s_2)=\sum_{a_2\in A_2}s_2(a_2)v(s_1,a_2)$ nên $\max_{s_2\in S_2}v(s_1,s_2)=\max_{a_2\in A_2}v(s_1,a_2)$, nghiệm tối ưu là một chiến lược thuần．Suy ra $w = \max_{s_1\in S_1}\min_{a_2\in A_2} v(s_1,a_2)$．Đặt biến phụ $u$，bài toán thành
    
    $$
    w = \max_{s_1\in S_1} u \text{ subject to }u \le \min_{a_2\in A_2} v(s_1,a_2).
    $$
    
    Ràng buộc tương đương $u\le v(s_1,a_2)$ với mọi $a_2\in A_2$．Thay định nghĩa chiến lược hỗn hợp và biểu thức $v(s_1,a_2)$，bài toán tương đương [quy hoạch tuyến tính](../linear-programming.md)
    
    $$
    (P) \qquad
    \begin{aligned}
    w = \max_{u,s_1}\; & u\\
    \text{subject to }& \sum_{a_1\in A_1}s_1(a_1)v(a_1,a_2) \ge u,~\forall a_2\in A_2,\\
    & \sum_{a_1\in A_1}s_1(a_1) = 1,\\
    & s_1(a_1) \ge 0,~\forall a_1\in A_1.
    \end{aligned}
    $$
    
    Bài toán khả thi và có nghiệm tối ưu．Theo [đối偶](../linear-programming.md#对偶原理), nghiệm tối ưu bằng nghiệm của đối偶：
    
    $$
    (D) \qquad
    \begin{aligned}
    w = \min_{t,s_2}\; & t\\
    \text{subject to }&\sum_{a_2\in A_2}s_2(a_2)v(a_1,a_2) \le t,~\forall a_1\in A_1,\\
    &\sum_{a_2\in A_2}s_2(a_2) = 1,\\
    &s_2(a_2)\ge 0,~\forall a_2\in A_2.
    \end{aligned}
    $$
    
    Lặp lại lập luận, bài toán này tương đương $\min_{s_2\in A_2}\min_{s_1\in S_1}v(s_1,s_2)$．Định lý được chứng minh．

Kết quả này chính là [cân bằng Nash](https://en.wikipedia.org/wiki/Nash_equilibrium) của trò chơi．Nếu cả hai chọn chiến lược cân bằng, không ai có thể được lợi strictly bằng cách lệch khỏi cân bằng．

### Chuyển thành bài toán quy hoạch tuyến tính

Chứng minh von Neumann cũng chỉ ra cách giải．Gọi $n,m$ là số hành động của người chơi 1 và 2．Cho ma trận lợi ích của người chơi 1 là $V\in\mathbf R^{n\times m}$，ta giải bài toán：

$$
\begin{aligned}
w = \max_{(u,s)\in\mathbf R\times\mathbf R^n}\; & u\\
\text{subject to }& V^Ts \ge u\mathbf 1,\\
& \mathbf 1^Ts = 1,\\
& s \ge 0.
\end{aligned}
$$

Đây là bài toán quy hoạch tuyến tính kích thước $\Theta(n+m)$, có thể giải hiệu quả bằng [đơn hình](../simplex.md)．Nghiệm tối ưu $s$ là chiến lược tối ưu của người chơi 1．Chiến lược tối ưu của người chơi 2 có thể lấy từ biến đối偶 (shadow price) trong bảng đơn hình．

### Bài tập

-   [Luogu P4232 无意识之外的捉迷藏](https://www.luogu.com.cn/problem/P4232)

## Tài liệu tham khảo và chú thích

-   [Zero-sum game - Wikipedia](https://en.wikipedia.org/wiki/Zero-sum_game)
-   [Minimax theorem - Wikipedia](https://en.wikipedia.org/wiki/Minimax_theorem)
