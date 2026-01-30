Khi gặp các bài toán dạng như "Cho $n$ số nguyên, hỏi có thể ghép được bao nhiêu số nguyên khác từ $n$ số này (mỗi số có thể lấy nhiều lần)", hoặc "Cho $n$ số nguyên, hỏi số nguyên nhỏ nhất (lớn nhất) không thể ghép được từ $n$ số này", hoặc "Ít nhất cần ghép mấy lần để được số có phần dư $p$ khi chia cho $K$" thì có thể sử dụng phương pháp "đường đi ngắn nhất theo đồng dư" (modulo shortest path).

Phương pháp này tận dụng việc xây dựng trạng thái dựa trên đồng dư để tối ưu hóa độ phức tạp không gian.

Tương tự như [ràng buộc hiệu (difference constraints)](./diff-constraints.md), các trạng thái xây dựng dựa trên đồng dư có thể coi như các đỉnh trong bài toán đường đi ngắn nhất một nguồn. Chuyển trạng thái thường có dạng $f(i+y) = f(i) + y$, tương tự như trong đường đi ngắn nhất $f(v) = f(u) +edge(u,v)$.

## Bài toán mẫu

### Ví dụ 1

???+ note "[P3403 Thang máy nhảy tầng](https://www.luogu.com.cn/problem/P3403)"
    Đề bài: Cho $x, y, z, h$, với mỗi $k \in [1,h]$, có bao nhiêu $k$ thỏa mãn $ax+by+cz=k$ ($0\leq a,b,c$, $1\le x,y,z\le 10^5$, $h\le 2^{63}-1$).

Giả sử $x < y < z$.

Gọi $d_i$ là tầng thấp nhất $p$ có $p \bmod x = i$ chỉ sử dụng **phép cộng $y$** và **phép cộng $z$** (tức là chỉ dùng thao tác 2 và 3), tức là số nhỏ nhất đồng dư $i$ modulo $x$ có thể đạt được bằng các thao tác này, dùng để tính số lượng số thỏa mãn trong mỗi lớp đồng dư.

Có hai trạng thái chuyển:

-   $i \xrightarrow{y} (i+y) \bmod x$

-   $i \xrightarrow{z} (i+z) \bmod x$

Thông thường, ta chọn số nhỏ nhất trong dãy $a_i$ để làm modulo (ở đây là $x$) nhằm giảm số trạng thái cần xét (hệ dư nhỏ nhất).

Thực chất, thao tác này tương đương với việc xây dựng cạnh trong bài toán đường đi ngắn nhất:

`add(i, (i+y) % x, y)`

`add(i, (i+z) % x, z)`

Tiếp theo, chỉ cần tính $d_0, d_1, d_2, \dots, d_{x-1}$, tức là chạy một lần thuật toán đường đi ngắn nhất là đủ.

??? example "Cài đặt dựa trên đường đi ngắn nhất"
    ```cpp
    --8<-- "docs/graph/code/mod-shortest-path/mod-shortest-path_1.cpp"
    ```

Tuy nhiên, thực tế không cần chạy thuật toán đường đi ngắn nhất thông thường, vì có hai tính chất đặc biệt:

Thứ nhất, chỉ có hai loại trọng số cạnh, và với mỗi đường đi, do tính chất giao hoán của phép cộng, thứ tự đi các loại cạnh không ảnh hưởng kết quả. Do đó, có thể thực hiện hai lần đường đi ngắn nhất, mỗi lần chỉ xây dựng một loại cạnh.

Thứ hai, với đồ thị chỉ có một loại trọng số, mỗi đỉnh $u$ có một cạnh vào (từ $(u-y) \bmod x$) và một cạnh ra (tới $(u+y) \bmod x$), nên toàn bộ đồ thị là hợp của một số vòng. Có thể chứng minh có đúng $\gcd(x, y)$ vòng, mỗi vòng có độ dài bằng nhau.

???+ note "Chứng minh"
    Gọi $d=\gcd(x,y)$, $x=da, y=db$, với $\gcd(a,b)=1$.

    Xét đi từ $u$ qua $k$ bước, đến $(u+ky) \bmod x$. Nếu tạo thành vòng, thì $ky \equiv 0 \pmod x$, tức là $kb \equiv 0 \pmod a$.

    Do $\gcd(a,b)=1$, giá trị nhỏ nhất của $k$ là $a$, tức là độ dài vòng là $a = \dfrac{x}{d}$. Vì có thể bắt đầu từ bất kỳ đỉnh nào, nên số vòng là $d$.

Hơn nữa, trọng số cạnh dương, nên sau hai vòng chắc chắn không thể tiếp tục nới lỏng. Chỉ cần cập nhật một vòng là đủ. Nhờ đó, độ phức tạp không bị giới hạn bởi thuật toán đường đi ngắn nhất, có thể đạt $O(x)$.

Tương tự như bài toán ràng buộc hiệu, nếu tồn tại một bộ nghiệm $\{a_1,a_2,\cdots,a_n\}$ thì $\{a_1+d,a_2+d,\cdots,a_n+d\}$ cũng là nghiệm. Do đó, chọn $i=1$ làm đỉnh nguồn, khi đó $dis_{1}=1$ là nhỏ nhất trong phạm vi đã biết, nên nghiệm thu được cũng là nhỏ nhất.

Đáp án là:

$$
\sum_{i=0}^{x-1}\left(\frac{h-d_i}{x} + 1\right)
$$

Cộng thêm 1 vì tầng $d_i$ cũng được tính.

Lưu ý: $h$ có thể rất lớn ($h \leq 2^{63}-1$), nên trước khi chạy đường đi ngắn nhất, giá trị khởi tạo của $d_i$ phải ít nhất là $2^{63}$, vượt quá giá trị lớn nhất của `long long` trong C++. Có thể dùng `unsigned long long` hoặc giảm $h \gets h - 1$, đặt tầng thấp nhất là $0$, các phần còn lại không đổi.

??? example "Cài đặt tối ưu dựa trên vòng"
    ```cpp
    --8<-- "docs/graph/code/mod-shortest-path/mod-shortest-path_2.cpp"
    ```

### Ví dụ 2

???+ note "[ARC084B Small Multiple](https://atcoder.jp/contests/arc084/tasks/arc084_b)"
    Đề bài: Cho $n$, tìm số nhỏ nhất là bội của $n$ có tổng các chữ số nhỏ nhất. ($1\le n\le 10^5$)

Bài này có thể dùng tích chập tuần hoàn để tối ưu bài toán ba lô đầy đủ trong $O(n\log^2 n)$, nhưng ta mong muốn thuật toán tuyến tính.

Nhận thấy mọi số nguyên dương đều có thể bắt đầu từ $1$, thực hiện một chuỗi thao tác nhân $10$ và cộng $1$ để thu được, trong đó số lần cộng $1$ chính là tổng các chữ số. Điều này gợi ý sử dụng đường đi ngắn nhất.

Với mọi $0\le k\le n-1$, từ $k$ nối cạnh trọng số $0$ tới $10k$; từ $k$ nối cạnh trọng số $1$ tới $k+1$ (tất cả tính theo modulo $n$).

Mỗi bội của $n$ tương ứng với một đường đi từ đỉnh $1$ tới đỉnh $0$ trên đồ thị này, tìm đường đi ngắn nhất từ $1$ tới $0$ là đáp án. Một số đường đi không hợp lệ (ví dụ đi liên tiếp $10$ cạnh trọng số $1$), nhưng các đường đi này không cho đáp án tốt hơn nên không ảnh hưởng kết quả.

Độ phức tạp $O(n)$.

## Bài tập

[洛谷 P3403 Thang máy nhảy tầng](https://www.luogu.com.cn/problem/P3403)

[洛谷 P2662 Hàng rào chuồng bò](https://www.luogu.com.cn/problem/P2662)

[\[Đội tuyển quốc gia\] Phương trình của Momo](https://www.luogu.com.cn/problem/P2371)

[「NOIP2018」Hệ thống tiền tệ](https://loj.ac/problem/2951)

[AGC057D - Sum Avoidance](https://atcoder.jp/contests/agc057/tasks/agc057_d)

[「THUPC 2023 Sơ khảo」Ba lô](https://loj.ac/p/6872)
