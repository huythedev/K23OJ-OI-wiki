???+ note "Gợi ý"
    Bài viết này đang trong quá trình biên tập và thảo luận, rất hoan nghênh bổ sung lộ trình học sâu hơn hoặc nêu ý kiến tại phần bình luận!

Bài viết này sẽ giới thiệu lộ trình học lập trình thi đấu.

Lộ trình này vừa là hướng dẫn cho người mới, vừa là danh sách ôn tập.

## 1 Nền tảng ngôn ngữ C++

Bắt đầu từ cú pháp C++, đi từng bước một.

### 1.1 Hello, World!

Với câu `Hello, World!`, hãy bắt đầu hành trình lập trình thi đấu!

Đồng thời tìm hiểu sơ bộ khung chương trình C++.

-   [Hello, World!](../lang/helloworld.md)
-   [Cơ sở cú pháp C++](../lang/basic.md)

### 1.2 Biến và phép toán

Mục đích ban đầu của máy tính là tính toán. Vì vậy hãy học cách thực hiện các phép tính đơn giản.

-   [Biến](../lang/var.md)
-   [Phép toán](../lang/op.md)

### 1.3 Điều khiển luồng

#### 1.3.1 Cấu trúc rẽ nhánh

Khi cần lựa chọn câu lệnh theo điều kiện, ta cần các câu lệnh rẽ nhánh.

-   [Rẽ nhánh](../lang/branch.md)

Các câu lệnh rẽ nhánh gồm:

-   if
-   if-else
-   if-elif-else
-   switch

#### 1.3.2 Cấu trúc lặp

Lặp lại một nhóm câu lệnh nhiều lần sẽ dùng vòng lặp.

-   [Vòng lặp](../lang/loop.md)

Các vòng lặp gồm:

-   for
-   while
-   do-while

### 1.4 Mảng và cấu trúc

Mảng dùng để lưu nhiều dữ liệu cùng kiểu. Cấu trúc (struct) có thể gộp nhiều biến.

-   [Mảng](../lang/array.md)
-   [Cấu trúc](../lang/struct.md)

### 1.5 Hàm và đệ quy

Dùng hàm để chương trình có tính mô-đun, giảm chi phí triển khai.

Đệ quy là “cửa ải” của người mới. “Tự gọi chính mình” nghe khó hiểu, nhưng nếu đi sâu sẽ thấy không khác về bản chất so với “gọi hàm khác”.

-   [Hàm](../lang/func.md)
-   [Đệ quy & chia để trị](../basic/divide-and-conquer.md)

## 2 CSP-J Mức nhập môn

### 2.1 Liệt kê và mô phỏng

Từ đây bạn đã có thể dùng C++ cho các tác vụ đơn giản, nhưng vẫn chưa đủ.

Để giải các bài dễ, bạn cần biết liệt kê hoặc mô phỏng logic trong đầu để hiện thực hóa. Dù không tối ưu, cách này đôi khi rất hiệu quả.

-   [Liệt kê](../basic/enumerate.md)
-   [Mô phỏng](../basic/simulate.md)

### 2.2 Đệ quy và chia để trị

Đệ quy là khi hàm tự gọi lại chính nó; chia để trị là chia bài toán thành các bài con, giải rồi hợp nhất.

-   [Đệ quy & chia để trị](../basic/divide-and-conquer.md)

### 2.3 Chuỗi

Trong bài tin học, kiểu dữ liệu chuỗi rất thường gặp. Bạn cần học các hàm STL thao tác chuỗi; ngoài ra, mô phỏng cũng là cách tốt.

-   [Cơ sở chuỗi](../string/basic.md)
-   [Hàm STL](../string/lib-func.md)

### 2.4 Sắp xếp

Khi có một dãy dữ liệu, làm sao sắp xếp từ vô thứ tự thành có thứ tự là vấn đề quan trọng. Khi bí ý tưởng, hãy thử sắp xếp. Đây cũng là nền tảng của nhiều thuật toán khác.

Có nhiều phương pháp sắp xếp, nhưng hiểu rồi thì không khó nhớ.

-   [Giới thiệu sắp xếp](../basic/sort-intro.md)
-   [Sắp xếp chọn](../basic/selection-sort.md)
-   [Sắp xếp nổi bọt](../basic/bubble-sort.md)
-   [Sắp xếp chèn](../basic/insertion-sort.md)
-   [Sắp xếp đếm](../basic/counting-sort.md)
-   [Sắp xếp cơ số](../basic/radix-sort.md)
-   [Sắp xếp nhanh](../basic/quick-sort.md)
-   [Sắp xếp trộn](../basic/merge-sort.md)
-   [Sắp xếp vun đống](../basic/heap-sort.md)
-   [Sắp xếp theo xô](../basic/bucket-sort.md)
-   [STL liên quan đến sắp xếp](../basic/stl-sort.md)

Trong đề cương NOI, mức nhập môn chỉ yêu cầu chọn, nổi bọt, chèn (3 thuật toán). Nhưng các phương pháp khác không quá khó và có thể xuất hiện ở vòng sơ khảo, nên liệt kê luôn.

### 2.5 Tìm kiếm nhị phân và倍增

Tìm kiếm nhị phân là áp dụng tư duy chia để trị để thu hẹp phạm vi đến khi có đáp án. Lưu ý cần dữ liệu có thứ tự.

-   [Nhị phân](../basic/binary.md)

“倍增” (binary lifting) thì khác: liên tục nhân đôi để biến xử lý tuyến tính thành log, tối ưu độ phức tạp. (Điểm này cần chút nền tảng toán, tạm thời bỏ qua cũng không sao)

-   [倍增](../basic/binary-lifting.md)

### 2.6 Tìm kiếm

Ở mức nhập môn, bài tìm kiếm thường gặp trong bài mê cung với dữ liệu dạng bản đồ; ngoài ra còn dùng để liệt kê cấu hình hợp lệ, hoặc “ăn điểm” nhanh.

#### 2.6.1 Tìm kiếm theo chiều sâu (DFS)

DFS dùng đệ quy để duyệt vét cạn; có điểm giống DFS trong đồ thị nhưng không hoàn toàn trùng.

-   [DFS (tìm kiếm)](../search/dfs.md)

#### 2.6.2 Tìm kiếm theo chiều rộng (BFS)

Mỗi trạng thái xem như một đỉnh trong đồ thị, có thể mở rộng theo kiểu “trải thảm”.

-   [BFS (tìm kiếm)](../search/bfs.md)

#### 2.6.3 Tối ưu tìm kiếm

Nhiều bài có thể giải bằng DFS, nhưng độ phức tạp quá lớn. Do đó cần tối ưu, gọi là “cắt tỉa”. Với BFS cũng tương tự, nhưng linh hoạt hơn.

-   [Tối ưu cắt tỉa DFS](../search/opt.md)

### 2.7 Nhập môn cấu trúc dữ liệu

#### 2.7.1 Cấu trúc dữ liệu tuyến tính

Mảng, danh sách liên kết, hàng đợi, ngăn xếp đều là tuyến tính. Tận dụng tốt sẽ rất tiện.

-   [Ngăn xếp](../ds/stack.md)
-   [Hàng đợi](../ds/queue.md)
-   [Danh sách liên kết](../ds/linked-list.md)

#### 2.7.2 Cấu trúc dữ liệu phức tạp

-   [Cây và cây nhị phân](../graph/tree-basic.md)
-   [Khái niệm đồ thị](../graph/concept.md)
-   [Lưu trữ đồ thị](../graph/save.md)

### 2.8 Nhập môn quy hoạch động

Quy hoạch động (Dynamic Programming, DP) là phương pháp giải bài toán phức tạp bằng cách tách thành các bài con đơn giản.

DP không phải một thuật toán cụ thể mà là một phương pháp giải; vì thế nó xuất hiện trong nhiều chủ đề.

-   [Giới thiệu DP](../dp/index.md)

#### 2.8.1 Bài toán ba lô

Cho ba lô giới hạn dung tích và các vật có trọng lượng, giá trị, chọn sao cho tổng giá trị lớn nhất. Đây là “cửa ải” đầu tiên của nhiều OIer.

-   [DP ba lô](../dp/knapsack.md)

#### 2.8.2 DP tuyến tính

Khó nhất trong DP là thiết kế trạng thái, cần kỹ thuật xây dựng. Khi đã có trạng thái và công thức chuyển, bài DP thường không quá khó.

-   [Xây dựng](../basic/construction.md)
-   [DP cơ bản](../dp/basic.md)

Tìm kiếm có ghi nhớ (memoization) là lưu trạng thái đã duyệt để tránh lặp. Nhiều bài có thể dùng cách này để giảm độ khó tư duy.

Vì mỗi trạng thái chỉ duyệt một lần, đây cũng là cách triển khai DP phổ biến.

-   [Tìm kiếm có ghi nhớ](../dp/memo.md)

#### 2.8.3 DP phức tạp

DP đoạn là mở rộng của DP tuyến tính, phụ thuộc mạnh vào thứ tự và cách gộp phần tử giữa các giai đoạn.

-   [DP đoạn](../dp/interval.md)

### 2.9 Toán

#### 2.9.1 Số lớn

Nếu `long long` chưa đủ, dùng số lớn: mô phỏng bốn phép tính.

-   [Tính toán số lớn](../math/bignum.md)

#### 2.9.2 Đổi cơ số

Ngoài nhị phân, còn hay dùng bát phân và thập lục phân. Đổi cơ số đôi khi rất hữu ích.

-   [Hệ cơ số](../math/base.md)

#### 2.9.3 Phép toán bit

Phép toán bit dựa trên biểu diễn nhị phân, rất nhanh.

6 phép cơ bản: AND, OR, XOR, NOT, dịch trái, dịch phải.

-   [Phép toán bit](../math/bit.md)

#### 2.9.4 Số học

-   [Cơ sở số học](../math/number-theory/basic.md)
-   [Số nguyên tố](../math/number-theory/prime.md)
-   [Sàng](../math/number-theory/sieve.md)
-   [Ước chung lớn nhất](../math/number-theory/gcd.md)
-   [Hàm Euler](../math/number-theory/euler-totient.md)
-   [Phân tích thừa số nguyên tố](../math/number-theory/pollard-rho.md)

#### 2.9.5 Tổ hợp đếm

-   [Chỉnh hợp, tổ hợp](../math/combinatorics/combination.md)
-   [Nguyên lý Dirichlet](../math/combinatorics/drawer-principle.md)
-   [Bao hàm - loại trừ](../math/combinatorics/inclusion-exclusion-principle.md)

***

Đến đây bạn đã hoàn thành phần nhập môn, nhưng để thành thạo bạn cần luyện tập đủ nhiều bài để củng cố kiến thức.
