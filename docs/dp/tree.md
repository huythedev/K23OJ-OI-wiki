author: aaron20100919

DP trên cây, tức là thực hiện DP trên cấu trúc cây. Do cây có tính chất đệ quy vốn có, DP trên cây thường được thực hiện bằng đệ quy.

## Cơ bản

Lấy ví dụ sau để giới thiệu quá trình DP trên cây thông thường.

???+ note "Bài mẫu [LuoGu P1352 Bữa tiệc không có sếp](https://www.luogu.com.cn/problem/P1352)"
    Một trường đại học có $n$ nhân viên, được đánh số từ $1 \sim N$. Giữa họ có quan hệ cấp trên - cấp dưới, tức là quan hệ của họ giống như một cây với hiệu trưởng là gốc, nút cha là cấp trên trực tiếp của nút con. Hiện tại có một bữa tiệc kỷ niệm, mỗi khi mời một nhân viên sẽ tăng chỉ số hạnh phúc $a_i$, tuy nhiên, nếu cấp trên trực tiếp của một nhân viên đã tham dự, thì nhân viên đó nhất quyết không tham dự. Hãy lập trình tính toán, mời những nhân viên nào để tổng chỉ số hạnh phúc là lớn nhất, và cho biết chỉ số hạnh phúc lớn nhất đó.

Ta đặt $f(i,0/1)$ là đáp án tối ưu của cây con gốc tại $i$ (giá trị thứ hai là 0 nghĩa là $i$ không tham dự, 1 nghĩa là $i$ tham dự).

Với mỗi trạng thái, tồn tại hai quyết định (trong đó $x$ là con của $i$):

-   Nếu cấp trên không tham dự, cấp dưới có thể tham dự hoặc không, khi đó $f(i,0) = \sum\max \{f(x,1),f(x,0)\}$;
-   Nếu cấp trên tham dự, cấp dưới đều không tham dự, khi đó $f(i,1) = \sum{f(x,0)} + a_i$.

Ta có thể dùng DFS, khi quay về cha thì cập nhật đáp án tối ưu của nút hiện tại.

```cpp
--8<-- "docs/dp/code/tree/tree_1.cpp"
```

Thông thường, trạng thái DP trên cây là đáp án tối ưu tại nút hiện tại. Trước tiên DFS qua các cây con để lấy đáp án tối ưu, sau đó truyền lên cha để chuyển trạng thái, cuối cùng giá trị tại gốc là đáp án tối ưu cần tìm.

### Bài tập

-   [HDU 2196 Computer](https://acm.hdu.edu.cn/showproblem.php?pid=2196)

-   [POJ 1463 Strategic game](http://poj.org/problem?id=1463)

-   [\[POI2014\]FAR-FarmCraft](https://www.luogu.com.cn/problem/P3574)

## Balo trên cây

Bài toán balo trên cây, nói đơn giản là sự kết hợp giữa bài toán balo và DP trên cây.

???+ note "Bài mẫu [LuoGu P2014 CTSC1997 Chọn môn học](https://www.luogu.com.cn/problem/P2014)"
    Hiện có $n$ môn học, môn thứ $i$ có số tín chỉ là $a_i$, mỗi môn có thể có hoặc không có một môn tiên quyết, nếu có thì phải học xong môn tiên quyết mới được học môn đó.
    
    Một học sinh cần học $m$ môn, hỏi tổng số tín chỉ lớn nhất có thể đạt được là bao nhiêu.
    
    $n,m \leq 300$

Đặc điểm mỗi môn chỉ có tối đa một môn tiên quyết, giống như trong cây có gốc, mỗi nút chỉ có một cha.

Do đó, có thể xây dựng cây dựa trên đặc điểm này, toàn bộ các môn tạo thành một rừng. Để thuận tiện, ta thêm một môn số $0$ có $0$ tín chỉ (đánh số là $0$), làm tiên quyết cho tất cả các môn không có tiên quyết, như vậy biến rừng thành một cây gốc $0$.

Đặt $f(u,i,j)$ là đáp án tối ưu khi xét cây con gốc $u$, đã duyệt qua $i$ cây con đầu tiên của $u$, chọn $j$ môn.

Quá trình chuyển trạng thái kết hợp DP trên cây và [DP balo](./knapsack.md), ta liệt kê từng con $v$ của $u$, đồng thời liệt kê số môn chọn trong cây con $v$, rồi gộp kết quả cây con vào $u$.

Gọi số con của $x$ là $s_x$, kích thước cây con gốc $x$ là $\textit{siz}_x$, có thể viết phương trình chuyển trạng thái:

$$
f(u,i,j)=\max_{v,k \leq j,k \leq \textit{siz}_v} f(u,i-1,j-k)+f(v,s_v,k)
$$

Lưu ý các điều kiện giới hạn trong phương trình trên, đảm bảo không truy cập các trạng thái vô nghĩa.

Chiều thứ hai của $f$ có thể dễ dàng loại bỏ bằng mảng cuộn, lưu ý khi đó cần liệt kê $j$ ngược lại.

Có thể chứng minh độ phức tạp là $O(nm)$[^note1].

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/tree/tree_2.cpp"
    ```

### Bài tập

-   [「CTSC1997」Chọn môn học](https://www.luogu.com.cn/problem/P2014)

-   [「JSOI2018」Hành động thâm nhập](https://loj.ac/problem/2546)

-   [「SDOI2017」Cây táo](https://loj.ac/problem/2268)

-   [「Codeforces Round 875 Div. 1」Problem D. Mex Tree](https://codeforces.com/contest/1830/problem/D)

## DP đổi gốc trên cây

DP đổi gốc trên cây còn gọi là quét hai lần, thường không chỉ định gốc, và việc thay đổi gốc sẽ ảnh hưởng đến các giá trị như tổng độ sâu con, tổng trọng số, v.v.

Thường cần hai lần DFS, lần đầu để tiền xử lý các thông tin như độ sâu, tổng trọng số, lần hai để chạy DP đổi gốc.

Sau đây là một số ví dụ để làm quen với nội dung này.

???+ note "Bài mẫu [\[POI2008\]STA-Station](https://www.luogu.com.cn/problem/P3478)"
    Cho một cây $n$ đỉnh, hãy tìm một nút làm gốc sao cho tổng độ sâu các nút là lớn nhất.

Giả sử $u$ là nút hiện tại, $v$ là con của $u$. Đầu tiên cần dùng $s_i$ để biểu diễn số nút trong cây con gốc $i$, có $s_u=1+\sum s_v$. Rõ ràng cần một lần DFS để tính tất cả $s_i$, đây là bước tiền xử lý, ta có tổng số nút trong cây con gốc mỗi nút.

Xét chuyển trạng thái, đây là lúc "đổi gốc". Gọi $f_u$ là tổng độ sâu các nút khi gốc là $u$.

Chuyển từ $f_u$ sang $f_v$ thể hiện đổi gốc, tức là chuyển từ gốc $u$ sang gốc $v$. Khi đổi gốc, các nút trong cây con $v$ giảm độ sâu đi 1, tổng độ sâu giảm $s_v$; các nút không thuộc cây con $v$ tăng độ sâu lên 1, tổng độ sâu tăng $n-s_v$.

Từ đó có phương trình chuyển trạng thái $f_v = f_u - s_v + n - s_v=f_u + n - 2 \times s_v$.

Lần DFS thứ hai duyệt toàn bộ cây và chuyển trạng thái $f_v=f_u + n - 2 \times s_v$, như vậy có thể tính tổng độ sâu với mỗi nút làm gốc. Cuối cùng duyệt qua tất cả các tổng độ sâu để lấy đáp án.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/tree/tree_3.cpp"
    ```

### Bài tập

-   [Atcoder Educational DP Contest, Problem V, Subtree](https://atcoder.jp/contests/dp/tasks/dp_v)

-   [Educational Codeforces Round 67, Problem E, Tree Painting](https://codeforces.com/contest/1187/problem/E)

-   [POJ 3585 Accumulation Degree](http://poj.org/problem?id=3585)

-   [\[USACO10MAR\]Great Cow Gathering G](https://www.luogu.com.cn/problem/P2986)

-   [CodeForce 708C Centroids](http://codeforces.com/problemset/problem/708/C)

## Tài liệu tham khảo & chú thích

[^note1]: [Chứng minh độ phức tạp DP balo hợp nhất cây con - Blog CSDN của LYD729](https://blog.csdn.net/lyd_7_29/article/details/79854245)
