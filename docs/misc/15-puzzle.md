## Giới thiệu

**15-puzzle** (tiếng Anh: 15-puzzle, còn gọi Gem Puzzle, Boss Puzzle, Game of 15, Mystic Square, N-puzzle, v.v.) là một trò chơi xếp hình dạng trượt (sliding puzzle). Bàn cờ có kích thước $4\times 4$, trong đó 15 vị trí đặt các ô đã bị xáo trộn, còn lại một ô trống. Ô cùng hàng hoặc cùng cột với ô trống có thể trượt ngang hoặc dọc để di chuyển. Mục tiêu là sắp xếp các ô theo thứ tự.

15-puzzle thường được gọi là **n-puzzle**, trong đó $n$ là số ô có mặt trên bàn. Các biến thể kích thước khác cũng dùng tên tương tự, ví dụ $8$-puzzle là $3\times3$ với $8$ ô. Nhưng 15-puzzle cũng có thể gọi là 16-puzzle, ở đây 16 là sức chứa của bàn. Bài toán mở rộng còn bao gồm bàn $n \times m$.

15-puzzle là bài toán kinh điển cho mô hình hóa bằng [thuật toán heuristic](../search/heuristic.md). Hai heuristic phổ biến là [khoảng cách Manhattan](../geometry/distance.md#曼哈顿距离) và số ô sai vị trí; cả hai đều là heuristic chấp nhận được (admissible heuristic), tức không bao giờ đánh giá cao số bước còn lại, đảm bảo tính tối ưu cho một số thuật toán tìm kiếm (ví dụ [A*](../search/astar.md)).

???+ note "Chú thích"
    **Trò chơi trượt** là dạng trò chơi trí tuệ xếp các ô bằng cách trượt trên mặt phẳng để đạt một cấu hình nhất định. Các trò chơi trượt phổ biến gồm xếp số, Hoa dung đạo, và kẹt xe. 15-puzzle là trò chơi trượt lâu đời nhất, do Noyes Chapman phát minh và phổ biến trong thập niên 1880. Khác với các trò giải đố dạng tour, trò chơi trượt không cho phép ô rời khỏi bàn, đặc điểm này khác với các trò xếp lại.

## Định nghĩa

Cho một bàn $4 \times 4$ với 15 ô xếp ngẫu nhiên. Ta cần sắp xếp lại theo thứ tự như hình. Mỗi bước chỉ được đổi vị trí ô trống với một ô kề cạnh. Bài toán thường gặp: tìm số bước tối thiểu để giải, đếm số ô sai vị trí, hoặc xác định có/không thể đạt cấu hình đích.

![](./images/15puzzle-1.svg)

## Chứng minh khả giải

Johnson & Story (1879) chứng minh rằng nếu $m$ và $n$ đều ít nhất là $2$, thì quy tắc ngược đúng với bàn $m\times n$: bằng quy nạp từ $m=n=2$, mọi hoán vị chẵn đều giải được. Archer (1999) đưa ra chứng minh khác dựa trên phân lớp tương đương bằng đường đi Hamilton.

## Thuật toán

Tìm một lời giải cho trò chơi trượt số khá dễ, nhưng tìm **lời giải tối ưu** là một bài toán **NP-hard**. 15-puzzle có lời giải tối ưu dài tối đa 80 bước; còn 8-puzzle tối đa 31 bước.

N-puzzle hỗ trợ các thuật toán tìm kiếm theo đồ thị như BFS và DFS, đồng thời có thể dùng [A*](../search/astar.md) để tìm tối ưu. Hàm heuristic $h(n)$ có thể là:

-   Số ô đặt sai vị trí.
-   Tổng khoảng cách Euclid từ các ô sai đến vị trí đích.
-   Tổng khoảng cách Manhattan từ các ô sai đến vị trí đích.

### Lý thuyết nhóm

Vì các cấu hình của trò chơi đẩy số 15 ô có thể sinh bởi “chu trình 3” (3-cycles), nên có thể chứng minh trò chơi này biểu diễn bởi nhóm luân phiên $A_{15}$. Thực tế, mọi trò chơi trượt số dùng $2\times k-1$ ô vuông cùng kích thước đều có thể biểu diễn bởi nhóm luân phiên $A_{2k-1}$.

## Bài tập

-   [N Puzzle](https://www.hackerrank.com/challenges/n-puzzle)
-   [A. Amity Assessment](https://codeforces.com/problemset/problem/645/A)
-   [Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/)
-   [POJ 1077 - Eight](http://poj.org/problem?id=1077)

## Tài liệu tham khảo & đọc thêm

1.  [15 puzzle - Wikipedia](https://en.wikipedia.org/wiki/15_puzzle)
2.  jrdnjacobson,[How to Solve the 15 Puzzle - instructables](https://www.instructables.com/How-To-Solve-The-15-Puzzle/)
3.  Korf, R. E. (2000),["Recent Progress in the Design and Analysis of Admissible Heuristic Functions"](https://www.researchgate.net/publication/2604757_Recent_Progress_in_the_Design_and_Analysis_of_Admissible_Heuristic_Functions), in Choueiry, B. Y.; Walsh, T. (eds.), Abstraction, Reformulation, and Approximation (PDF), SARA 2000. Lecture Notes in Computer Science, vol. 1864, Springer, Berlin, Heidelberg, pp. 45–55, doi:10.1007/3-540-44914-0\_3, ISBN 978-3-540-67839-7, retrieved 2010-04-26
4.  [Welcome to N-Puzzle - web demo](https://tristanpenman.com/demos/n-puzzle/)
