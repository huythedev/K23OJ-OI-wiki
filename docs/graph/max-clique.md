author: Persdre

Kiến thức nền tảng: [Khái niệm về Clique (Đoàn)](./concept.md)

## Giới thiệu

Trong khoa học máy tính, bài toán clique (đoàn) là bài toán tìm một clique (tập con các đỉnh mà mọi cặp đều kề nhau, còn gọi là đồ thị con đầy đủ) trong một đồ thị cho trước.

Bài toán clique cũng xuất hiện trong thực tế. Ví dụ, xét một mạng xã hội, mỗi đỉnh là một người dùng, mỗi cạnh là hai người quen biết nhau. Khi tìm được một clique, tức là tìm được một nhóm người mà ai cũng quen biết nhau.

Nếu muốn tìm nhóm lớn nhất mà mọi người đều quen biết nhau trong mạng xã hội này, ta cần thuật toán tìm clique lớn nhất.

Khái niệm [clique cực đại](./concept.md) đã được giới thiệu, clique lớn nhất là clique cực đại có số đỉnh nhiều nhất.

## Giải thích

Ý tưởng là sử dụng đệ quy và quay lui (backtrack), dùng một danh sách lưu các đỉnh, mỗi lần thêm một đỉnh vào đều kiểm tra xem các đỉnh này còn tạo thành một clique không. Nếu thêm vào mà không còn là clique, thì quay lui về trạng thái trước, thử đỉnh khác.

Lý do dùng quay lui là vì ta không biết trước một đỉnh $v$ **cuối cùng** có thuộc clique lớn nhất hay không. Nếu chọn $v$ mà không tìm được clique lớn nhất, cần quay lui và thử nghiệm các clique không chứa $v$.

## Quy trình

**Thuật toán Bron–Kerbosch** tối ưu hóa ý tưởng này. Dạng cơ bản là đệ quy với ba tập hợp: $R$, $P$, $X$. Các bước như sau:

1.  Khởi tạo $R, X$ rỗng, $P$ là tập tất cả các đỉnh của đồ thị.
2.  Mỗi lần lấy một đỉnh $v$ từ $P$, khi cả $P$ và $X$ đều rỗng thì có hai trường hợp:
    1.  $R$ là một clique cực đại, khi đó $X$ rỗng
    2.  Không phải clique cực đại, khi đó quay lui
3.  Với mỗi đỉnh $v$ lấy từ $P$:
    1.  Thêm $v$ vào $R$, sau đó đệ quy với $R, P, X$
    2.  Xóa $v$ khỏi $P$, thêm $v$ vào $X$
    3.  Nếu cả $P$ và $X$ đều rỗng, $R$ là clique lớn nhất

Phương pháp này còn có thể tối ưu thêm. Để tăng tốc quay lui, có thể chọn đỉnh chốt (pivot vertex) để tìm kiếm. Một cách khác là sắp xếp các đỉnh ngay từ đầu, khi liệt kê thì theo thứ tự chỉ số để tránh lặp lại.

## Cài đặt

### Pseudocode

```text
R := {}
P := node set of G 
X := {}

BronKerbosch1(R, P, X):
    if P and X are both empty:
        report R as a maximal clique
    for each vertex v in P:
        BronKerbosch1(R ⋃ {v}, P ⋂ N(v), X ⋂ N(v))
        P := P \ {v}
        X := X ⋃ {v}
```

### C++ Implementation

??? note "Mã nguồn minh họa"
    ```cpp
    --8<-- "docs/graph/code/max-clique/max-clique_1.cpp"
    ```

## Bài tập ví dụ

???+ note "[POJ 2989: All Friends](http://poj.org/problem?id=2989)"
    Đề bài: Có $n$ người, $m$ cặp bạn bè, hỏi số lượng clique lớn nhất.

Ý tưởng: Bài mẫu, dùng thuật toán Bron–Kerbosch.

Pseudocode:

```text
 BronKerbosch(All, Some, None):  
     if Some and None are both empty:  
         report All as a maximal clique // Tất cả đỉnh đã chọn, không còn đỉnh không chọn, cộng đáp án  
     for each vertex v in Some: // Duyệt từng đỉnh trong Some  
         BronKerbosch1(All ⋃ {v}, Some ⋂ N(v), None ⋂ N(v))   
         // Thêm v vào All, chỉ những người là bạn với v mới có thể tiếp tục, None cũng chỉ giữ lại các đỉnh là bạn với v  
         Some := Some - {v} // Đã duyệt xong, xóa khỏi Some, thêm vào None  
         None := None ⋃ {v} 
```

Để tăng tốc và giảm lặp, có thể chọn pivot $v$ để tối ưu.

Như đã nói, thuật toán trên có nhiều phép tính lặp lại các clique cực đại đã xét.

Xét ba tập $R, P, X$:

Chọn một đỉnh $u$ trong $P\cup X$, muốn cùng $R$ tạo thành clique cực đại thì chỉ cần xét các đỉnh trong $P\cap N(u)$ ($N(u)$ là tập đỉnh kề $u$).

Nếu sau khi chọn $u$ mà các đỉnh kề $u$ như $v$ cũng có thể vào clique cực đại, thì chỉ cần chọn $u$ là đủ. Như vậy giảm được lặp lại với $v$. Sau đó chỉ cần xét các đỉnh không kề $u$.

Cài đặt C++ tối ưu hóa:

??? note "Mã nguồn minh họa"
    ```cpp
    --8<-- "docs/graph/code/max-clique/max-clique_2.cpp"
    ```

## Bài tập

-   [ZOJ 1492 Maximum Clique](https://pintia.cn/problem-sets/91827364500/exam/problems/type/7?page=4&problemSetProblemId=91827364991)
-   [POJ 1419 Maximum Clique trong đồ thị vô hướng](http://poj.org/problem?id=1419)
-   [POJ 1129 Đài phát thanh](http://poj.org/problem?id=1129)

## Tài liệu tham khảo

-   [Clique problem - Wikipedia](https://en.wikipedia.org/wiki/Clique_problem)
-   [Cực đại đoàn, đoàn lớn nhất (Bron–Kerbosch)](https://blog.csdn.net/yo_bc/article/details/77453478)
-   [Bài toán clique lớn nhất — Bron–Kerbosch](https://hallelujahjeff.github.io/2018/04/12/34/)
-   [Bài toán clique lớn nhất](https://www.cnblogs.com/zhj5chengfeng/archive/2013/07/29/3224092.html)
