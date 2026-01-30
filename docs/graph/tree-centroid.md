author: Ir1d, Marcythm, LucienShui, Anguei, H-J-Granger, CornWorld, ttzc

Bài viết này giới thiệu khái niệm trọng tâm của cây (centroid) và các tính chất cơ bản.

## Định nghĩa

Trong cây $T$, nếu xóa một đỉnh $v$, mọi thành phần liên thông còn lại đều có kích thước không vượt quá một nửa số đỉnh của cây ban đầu, thì $v$ được gọi là **trọng tâm** (centroid) của cây. Kích thước thành phần liên thông lớn nhất sau khi xóa $v$ còn gọi là **trọng lượng** (weight) của đỉnh đó. Như vậy, trọng tâm là đỉnh có trọng lượng không vượt quá một nửa số đỉnh của cây.

???+ info "“Cây con”"
    Bài viết này có thể đề cập cả cây vô gốc, cây có gốc, và trường hợp đổi gốc cây. Để tránh nhầm lẫn, ký hiệu $T$ là cây vô gốc, $T^{(v)}$ là cây có gốc tại $v$. “Cây con” ở đây luôn là cây con trong **cây có gốc**: tức là một đỉnh và toàn bộ hậu duệ của nó. Cây con gốc $u$ trong $T^{(v)}$ ký hiệu là $T^{(v)}_u$. Định nghĩa này bao gồm cả toàn bộ cây. Nếu muốn loại trừ toàn bộ cây, sẽ gọi là “cây con thực sự”.

    Trong cây vô gốc, “cây con” thường chỉ thành phần liên thông. Khi nói về trọng tâm, một số tài liệu dùng “cây con” để chỉ thành phần liên thông lớn nhất sau khi xóa một đỉnh, hoặc thành phần thu được khi xóa một cạnh. Hai cách này đều cho cùng tập hợp cây con, và không bao gồm toàn bộ cây. Vì vậy, bài viết này sẽ tránh dùng “cây con” cho cây vô gốc.

    Khi giải bài toán trọng tâm, thường mặc định có một gốc. Khi xóa một đỉnh $v$, ngoài các cây con của các con $v$, còn có một “cây con hướng lên” (tức là phần còn lại của cây). Nếu $v$ có cha là $u$, thì cây con hướng lên là $T_u^{(v)}$. Khi đề cập đến loại cây con này, bài viết sẽ nói rõ là “cây con hướng lên”. Nếu không nói gì, “cây con” không bao gồm phần này.

Lưu ý, các thành phần liên thông sau khi xóa trọng tâm cũng là cây vô gốc. Khi xóa trọng tâm, cây sẽ tách thành nhiều cây nhỏ, mỗi cây có kích thước không quá một nửa cây gốc. Tính chất này rất hữu ích cho các bài toán chia để trị trên cây, như [phân chia theo trọng tâm](./tree-divide.md#点分治).

## Tính chất

Phần này bàn về các tính chất của trọng tâm. Trước hết, trọng tâm có các định nghĩa tương đương sau:

???+ note "Định nghĩa tương đương"
    Đỉnh $v$ là trọng tâm của cây $T$ khi và chỉ khi thỏa mãn một trong các điều kiện sau:
    
    === "Cây vô gốc"
        1.  Xóa $v$, mọi thành phần liên thông còn lại đều có kích thước không vượt quá một nửa số đỉnh cây.
        2.  Trong tất cả các đỉnh, xóa đỉnh nào thì thành phần lớn nhất còn lại là nhỏ nhất; $v$ là đỉnh đạt giá trị nhỏ nhất này.
        3.  Tổng khoảng cách từ $v$ đến mọi đỉnh khác là nhỏ nhất.
    
    === "Cây có gốc"
        1.  Khi lấy $v$ làm gốc, mọi cây con thực sự đều có kích thước không vượt quá một nửa số đỉnh cây.
        2.  Trong tất cả các gốc, gốc nào có cây con lớn nhất nhỏ nhất thì đó là trọng tâm.
        3.  Tổng độ sâu các đỉnh nhỏ nhất khi lấy $v$ làm gốc.

??? note "Chứng minh"
    ...existing code...

Ngoài các định nghĩa trên, trọng tâm còn có các tính chất sau:

???+ note "Tính chất"
    1.  Nếu trọng tâm không duy nhất thì có đúng hai đỉnh, và hai đỉnh này kề nhau. Nếu xóa cạnh nối hai trọng tâm, cây tách thành hai phần bằng nhau.
    2.  Khi thêm hoặc xóa một lá, trọng tâm chỉ dịch chuyển tối đa một cạnh.
    3.  Khi nối hai cây bằng một cạnh, trọng tâm của cây mới nằm trên đường nối hai trọng tâm cũ.
    4.  Trọng tâm của cây có gốc luôn nằm trên heavy path của gốc. Trọng tâm của cây là tổ tiên của trọng tâm các cây con nặng nhất của gốc.

??? note "Chứng minh"
    ...existing code...

## Cách tìm trọng tâm

Dựa vào các định nghĩa tương đương, có hai cách tìm trọng tâm cây trong $O(n)$ với $n$ là số đỉnh.

### DFS tính kích thước cây con

Dùng DFS tính kích thước mỗi cây con. Với mỗi đỉnh, lưu kích thước các cây con, và phần còn lại (cây con hướng lên) bằng tổng số đỉnh trừ kích thước cây con hiện tại. Từ đó xác định trọng tâm.

??? example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-centroid/tree-centroid-2.cpp:core"
    ```

### DP đổi gốc tính tổng độ sâu

Có thể dùng DP đổi gốc để tính tổng độ sâu các đỉnh khi lấy từng đỉnh làm gốc. Đỉnh nào có tổng nhỏ nhất là trọng tâm.

??? example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-centroid/tree-centroid-3.cpp:core"
    ```

## Bài tập ví dụ

???+ example "[Codeforces Round 359 (Div. 1) B. Kay and Snowflake](https://codeforces.com/problemset/problem/685/B)"
    Cho một cây có gốc, hãy tìm trọng tâm của mỗi cây con.

??? note "Hướng dẫn giải"
    Theo tính chất 3, trọng tâm của cây con gốc $u$ luôn nằm trên đường nối trọng tâm các cây con trực tiếp của $u$ với $u$.
    
    Tương tự cách DFS tìm trọng tâm, với mỗi cây con gốc $u$, trước tiên tìm trọng tâm các cây con trực tiếp (lá thì trọng tâm là chính nó), sau đó kiểm tra các đỉnh trên đường lên $u$ để xác định trọng tâm.

    Có thể tìm trọng tâm mọi cây con trong $O(n)$.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-centroid/tree-centroid-1.cpp"
    ```

## Bài tập

-   [Gym 101649G Godfather](https://codeforces.com/gym/101649/problem/G)
-   [POJ 1655 Balancing Art](http://poj.org/problem?id=1655)
-   [Luogu P1364 Bố trí bệnh viện](https://www.luogu.com.cn/problem/P1364)
-   [Codeforces 1406C Link Cut Centroids](https://codeforces.com/contest/1406/problem/C)
-   [Codeforces 708C Centroids](https://codeforces.com/problemset/problem/708/C)

## Tài liệu tham khảo

-   [Một số tính chất và cách duy trì động trọng tâm cây - fanhq666](https://web.archive.org/web/20181122041458/http://fanhq666.blog.163.com/blog/static/81943426201172472943638) ([bản lưu trên cnblogs](https://www.cnblogs.com/qlky/p/5781081.html))
-   [Đường kính, trọng tâm và phân chia trọng tâm trên cây - cyendra](https://www.cnblogs.com/zinthos/p/3899075.html)
-   [Tính chất và chứng minh về trọng tâm cây - suxxsfe](https://www.cnblogs.com/suxxsfe/p/13543253.html)
-   “Từ điển Olympic Tin học” mục 2.4.7.11, chương 1: Trọng tâm cây
