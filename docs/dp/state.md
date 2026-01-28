## Giới thiệu

DP trạng thái nén là một dạng của quy hoạch động, bằng cách chuyển tập hợp trạng thái thành số nguyên để ghi lại trong trạng thái DP nhằm thực hiện chuyển trạng thái.

Để đạt được độ phức tạp thời gian thấp hơn, thường cần tìm trạng thái có số lượng nhỏ hơn. Phần lớn bài toán sẽ sử dụng trạng thái nhị phân, dùng số nhị phân $n$ bit để biểu diễn $n$ trạng thái độc lập hai giá trị.

Sử dụng trạng thái nén thường liên quan đến thao tác bit, xem thêm về thao tác bit tại trang [Toán tử bit](../math/bit.md).

## Bài mẫu 1

???+ note "[「SCOI2005」Không xâm phạm lẫn nhau](https://loj.ac/problem/2153)"
    Trên bàn cờ $N\times N$ đặt $K$ quân vua ($1 \leq N \leq 9, 1 \leq K \leq N \times N$), sao cho chúng không tấn công lẫn nhau, hỏi có bao nhiêu cách đặt quân.
    
    Quân vua có thể tấn công 8 ô xung quanh nó (trên, dưới, trái, phải, và 4 ô chéo).

### Giải thích

Gọi $f(i,j,l)$ là số cách hợp lệ khi đã xét $i$ hàng, trạng thái hàng $i$ là $j$, và đã đặt $l$ quân vua trên bàn cờ.

Với trạng thái $j$, ta dùng số nguyên nhị phân $sit(j)$ để biểu diễn cách đặt quân vua, bit $0$ là không đặt, bit $1$ là có đặt; $sta(j)$ là số quân vua trong trạng thái đó, tức là số bit $1$ trong $sit(j)$. Ví dụ, trạng thái như hình dưới có thể biểu diễn bằng $100101$ (bit thấp bên trái), $sit(j)=100101_{(2)}=37, sta(j)=3$.

![](./images/SCOI2005-互不侵犯.png)

Gọi trạng thái hàng hiện tại là $j$, trạng thái hàng trước là $x$, ta có phương trình chuyển trạng thái: $f(i,j,l) = \sum f(i-1,x,l-sta(j))$.

Khi xét trạng thái $x$ của hàng trước, cần đảm bảo không xung đột với trạng thái $j$ của hàng hiện tại, liệt kê tất cả $x$ hợp lệ để chuyển trạng thái:

$$
f(i,j,l) = \sum f(i-1,x,l-sta(j))
$$

### Cài đặt

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/state/state_1.cpp"
    ```

## Bài mẫu 2

???+ note "[\[POI2004\] PRZ](https://www.luogu.com.cn/problem/P5911)"
    Có $n$ người cần qua cầu, người thứ $i$ nặng $w_i$, thời gian qua cầu là $t_i$. Những người này sẽ chia thành nhiều nhóm, chỉ khi một nhóm qua xong thì nhóm khác mới được qua. Cầu chịu tải tối đa $W$, hỏi thời gian ngắn nhất để tất cả qua cầu.
    
    $100\le W \le 400$, $1\le n\le 16$, $1\le t_i\le 50$, $10\le w_i\le 100$.

### Giải thích

Dùng $S$ để biểu diễn một tập con người, $t(S)$ là thời gian lâu nhất của nhóm $S$, $w(S)$ là tổng trọng lượng nhóm $S$, $f(S)$ là thời gian ngắn nhất để tất cả $S$ qua cầu:

$$
\begin{cases}
    f(\varnothing)=0,\\
    f(S)=\min\limits_{T\subseteq S;~w(T)\leq W}\left\{t(T)+f(S\setminus T)\right\}.
\end{cases}
$$

Lưu ý không nên liệt kê tập con rồi kiểm tra, mà nên dùng [liệt kê tập con](../math/binary-set.md#遍历所有掩码的子掩码), độ phức tạp $O(3^n)$.

### Cài đặt

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/state/state_2.cpp"
    ```

## Bài tập

-   [「NOI2001」Trận địa pháo binh](https://loj.ac/problem/10173)
-   [「USACO06NOV」Cánh đồng ngô Corn Fields](https://www.luogu.com.cn/problem/P1879)
-   [「Kỳ thi 9 tỉnh 2018」Một ván cờ gỗ](https://loj.ac/problem/2471)
