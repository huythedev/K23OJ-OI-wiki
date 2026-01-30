## Đếm số vòng (chu trình) thông thường

???+ note "[Bài mẫu 1: Codeforces Beta Round 11 D. A Simple Task](https://codeforces.com/problemset/problem/11/D)"
    Cho một đồ thị đơn giản, hãy đếm số vòng đơn giản trong đồ thị. Vòng đơn giản là vòng không có đỉnh hoặc cạnh lặp lại.

    Số đỉnh $1\leq n\leq 19$.

??? note "Ý tưởng giải"
    Sử dụng quy hoạch động trạng thái (Bitmask DP). Gọi $f(s,i)$ là số đường đi mà tập các đỉnh đã đi qua là $s$, hiện tại đang ở đỉnh $i$, và đỉnh đầu tiên là đỉnh nhỏ nhất trong tập $s$.

    Với trạng thái $f(s,i)$, liệt kê đỉnh tiếp theo $u$. Nếu $u$ đã nằm trong $s$ và là đỉnh nhỏ nhất (tức là về lại điểm xuất phát), cộng $f(s,i)$ vào đáp án $A$. Nếu $u$ chưa nằm trong $s$, cập nhật $f(s\cup\{u\},u)$ bằng $f(s,i)$.

    Cách này sẽ đếm cả vòng độ dài $2$ (tức là cạnh song song), và mỗi vòng độ dài lớn hơn $2$ sẽ bị đếm hai lần (vì cố định điểm xuất phát, có thể đi hai chiều). Do đó, đáp án là $\dfrac{A-m}2$, với $m$ là số cạnh. Độ phức tạp $O(2^n m)$.

??? note "Code mẫu"
    ```cpp
    --8<-- "docs/graph/code/rings-count/rings-count_1.cpp"
    ```

## Đếm số vòng tam giác (ba đỉnh)

**Vòng tam giác** là một bộ ba đỉnh $(u, v, w)$ sao cho tồn tại ba cạnh $(u,v)$, $(v,w)$, $(w,u)$. Bài toán đếm số vòng tam giác yêu cầu đếm số lượng các vòng tam giác trong đồ thị.

Đầu tiên, định hướng các cạnh: luôn hướng từ đỉnh có bậc nhỏ hơn sang đỉnh có bậc lớn hơn, nếu bậc bằng nhau thì từ đỉnh có số nhỏ hơn sang đỉnh có số lớn hơn. Khi đó, đồ thị trở thành đồ thị có hướng không chu trình (DAG).

??? note "Chứng minh đồ thị không có chu trình"
    Giả sử tồn tại chu trình, thì bậc các đỉnh trên chu trình phải tăng dần, để tạo thành chu trình thì các đỉnh phải có cùng bậc, nhưng số hiệu đỉnh lại khác nhau, mâu thuẫn.

    Thực chất, quy tắc định hướng này tạo ra một [quan hệ thứ tự bộ phận (partial order)](../math/order-theory.md#二元关系), nên đồ thị định hướng này (tức là Hasse diagram của quan hệ đó) chắc chắn là DAG.

Duyệt từng đỉnh $u$, với mỗi đỉnh $v$ mà $u$ hướng tới, tiếp tục duyệt các đỉnh $w$ mà $v$ hướng tới, kiểm tra xem $u$ có nối với $w$ không.

Độ phức tạp $O(m\sqrt m)$.

???+ note "Chứng minh độ phức tạp"
    Định hướng các cạnh duyệt hết $O(n+m)$.

    Với mỗi cặp $(v,w)$, số lượng $u$ không vượt quá bậc vào $d^-(v)$. Nếu $d^-(v)\leq\sqrt m$, số $w$ tối đa $n$, tổng $O(n\sqrt m)$. Nếu $d^-(v) > \sqrt m$, vì $v$ hướng tới $w$ nên $d(v)\leq d(w)$, suy ra $d(w)>\sqrt m$, mà tổng số cạnh là $m$, nên số $w$ như vậy tối đa $\sqrt m$, tổng $O(m\sqrt m)$. Tổng hợp lại là $O(n+m+n\sqrt m+m\sqrt m)=O(m\sqrt m)$.

    Nếu định hướng ngược lại (từ bậc lớn sang bậc nhỏ), chỉ cần đổi vai trò $u,w$, chứng minh vẫn đúng.

???+ note "Code mẫu ([洛谷 P1989 Đếm số vòng tam giác trong đồ thị vô hướng](https://www.luogu.com.cn/problem/P1989))"
    ```cpp
    --8<-- "docs/graph/code/rings-count/rings-count_2.cpp"
    ```

### Bài mẫu 2

???+ note "[HDU 6184 Counting Stars](https://acm.hdu.edu.cn/showproblem.php?pid=6184)"
    Cho một đồ thị vô hướng có $n$ đỉnh, $m$ cạnh, hãy đếm số lần xuất hiện của hình dưới đây.

    ![](./images/rings-count1.svg)

    $2\leq n\leq 10^5$, $1\leq m\leq\min\left\{2\times 10^5,\ \dfrac{n(n-1)}2\right\}$.

??? note "Ý tưởng giải"
    Hình này là hai vòng tam giác chung một cạnh. Đầu tiên đếm số vòng tam giác trên mỗi cạnh, sau đó với mỗi cạnh có $x$ vòng tam giác đi qua, cộng $\dbinom x2$ vào đáp án.

    Độ phức tạp $O(m\sqrt m)$.

??? note "Code mẫu"
    ```cpp
    --8<-- "docs/graph/code/rings-count/rings-count_3.cpp"
    ```

## Đếm số vòng bốn đỉnh

Tương tự, **vòng bốn đỉnh** là bốn đỉnh $a, b, c, d$ sao cho $(a,b)$, $(b,c)$, $(c,d)$, $(d,a)$ đều là cạnh.

Sắp xếp các đỉnh theo bậc tăng dần.

Duyệt đỉnh cuối cùng $a$ (bậc lớn nhất), với mỗi đỉnh $c$ có bậc nhỏ hơn $a$, đếm số đỉnh $b$ (bậc nhỏ hơn $a$) sao cho $(a,b)$, $(b,c)$ đều là cạnh. Với mỗi cặp $b$ như vậy, chọn hai đỉnh bất kỳ sẽ tạo thành một vòng bốn đỉnh. Đếm số $b$ chỉ cần duyệt qua các đỉnh $b$ và $c$.

Độ phức tạp tương đương đếm vòng tam giác, tức là $O(m\sqrt m)$ (giả sử $n,m$ cùng bậc).

Lưu ý: $(a,b,c,d)$ và $(a,c,b,d)$ có thể là hai vòng bốn đỉnh khác nhau.

Ngoài ra, các đỉnh cùng bậc sẽ có thứ tự khác nhau, cần đảm bảo $a\neq c$.

???+ note "Code mẫu ([LibreOJ P191 Đếm số vòng bốn đỉnh trong đồ thị vô hướng](https://loj.ac/p/191))"
    ```cpp
    --8<-- "docs/graph/code/rings-count/rings-count_4.cpp"
    ```

### Bài mẫu 3

???+ note "[Gym 102028L Connected Subgraphs](https://codeforces.com/gym/102028/problem/L)"
    Cho một đồ thị vô hướng có $n$ đỉnh, $m$ cạnh, hỏi có bao nhiêu cách chọn bốn cạnh sao cho đồ thị con tạo thành là liên thông.

    $4\leq n\leq 10^5$, $4\leq m\leq 2\times 10^5$.

??? note "Ý tưởng giải"
    Có năm trường hợp: đồ thị hình hoa (star), vòng bốn đỉnh, vòng tam giác với một đỉnh nối thêm một cạnh, đường bốn đỉnh với một đỉnh nối thêm một cạnh, và đường năm đỉnh.

    Đồ thị hình hoa: duyệt từng đỉnh, dùng tổ hợp để đếm.

    Vòng bốn đỉnh: dùng thuật toán ở trên.

    Vòng tam giác: duyệt từng vòng tam giác $(u,v,w)$, cộng $[d(u)-2]+[d(v)-2]+[d(w)-2]$ vào đáp án.

    Trường hợp thứ tư: duyệt từng đỉnh bậc $2$ là $x$, duyệt một đỉnh kề $y$ là đỉnh bậc $3$, cộng $[d(x)-1]\cdot\dbinom{d(y)-1}2$ vào đáp án. Tuy nhiên, các đỉnh kề của $y$ có thể trùng với đỉnh kề của $x$, khi đó sẽ trùng với trường hợp ba. Mỗi trường hợp ba bị đếm hai lần (vì có hai đỉnh bậc $3$), nên cần trừ đi hai lần số trường hợp ba.

    Trường hợp cuối: duyệt đỉnh giữa $x$, đáp án cộng
    $$
    \sum_{y\in son_x}\sum_{z\in son_x}[d(y)-1]\cdot[d(z)-1].
    $$
    Trong đó, có các trường hợp bị đếm trùng:
    1.  $y$ trùng $t$ nhưng $s$ khác $z$, trùng với trường hợp ba;
    2.  $s$ trùng $z$ nhưng $y$ khác $t$, cũng trùng với trường hợp ba;
    3.  $y$ trùng $t$, $s$ trùng $z$, trùng với vòng tam giác;
    4.  $s$ trùng $t$, trùng với vòng bốn đỉnh.

    Trường hợp ba có hai đỉnh bậc $2$ làm $x$, đúng bằng hai trường hợp trên, nên cần trừ hai lần số trường hợp ba. Với một vòng tam giác, ba đỉnh đều có thể làm $x$, bị đếm ba lần. Vòng bốn đỉnh bị đếm bốn lần.

    Như vậy, đã liệt kê đủ các trường hợp, độ phức tạp $O(n+m\sqrt m)$.

??? note "Code mẫu"
    ```cpp
    --8<-- "docs/graph/code/rings-count/rings-count_5.cpp"
    ```

## Bài tập

[洛谷 P3547 \[POI2013\] CEN-Price List](https://www.luogu.com.cn/problem/P3547)

[CodeForces 985G Team Players](https://codeforces.com/contest/985/problem/G) (nguyên lý bao hàm loại trừ)
