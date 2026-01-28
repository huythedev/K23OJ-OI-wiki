## Giới thiệu

Đồ thị hai phía (hay còn gọi là đồ thị hai phần, tiếng Anh: bipartite graph) là một loại đồ thị có cấu trúc đặc biệt. Tập đỉnh của nó có thể chia thành hai tập con không giao nhau, sao cho mọi cạnh trong đồ thị đều nối một đỉnh ở tập này với một đỉnh ở tập kia, và không có cạnh nào nối hai đỉnh trong cùng một tập.

Nhờ cấu trúc đơn giản này, đồ thị hai phía không chỉ có nhiều tính chất đẹp mà còn được ứng dụng rộng rãi trong thực tế như: phân công công việc, hệ thống gợi ý, thị trường ghép cặp, v.v. Nhiều bài toán tối ưu khó trên đồ thị tổng quát lại có thể giải hiệu quả và chính xác trên đồ thị hai phía.

## Định nghĩa

Nếu đồ thị $G=(V,E)$ có tập đỉnh $V$ chia được thành hai tập con không giao nhau $X$ và $Y$, sao cho mỗi cạnh $e\in E$ đều nối một đỉnh thuộc $X$ với một đỉnh thuộc $Y$, thì $G$ được gọi là **đồ thị hai phía** (bipartite graph). Hai tập $X$ và $Y$ thường gọi là hai **phần** (part) của đồ thị, hoặc lần lượt là phía trái và phía phải. Khi biết rõ hai phần $X$ và $Y$, ta cũng có thể ký hiệu đồ thị hai phía là bộ ba $(X, Y, E)$.

Một ví dụ điển hình về đồ thị hai phía như hình dưới:

![](./images/bi-graph-1.svg)

Các loại đồ thị như cây, chu trình chẵn, đồ thị lưới,... đều là ví dụ phổ biến của đồ thị hai phía.

## Đặc trưng

Đồ thị hai phía cũng có thể được định nghĩa tương đương qua các tính chất sau:

-   Đồ thị $G$ có thể tô màu bằng 2 màu (2-colorable). Tức là, chỉ cần dùng tối đa hai màu để tô các đỉnh sao cho hai đỉnh kề nhau luôn khác màu.
-   Đồ thị $G$ không chứa chu trình có độ dài lẻ.

Rõ ràng, tính chất đầu tiên tương đương với định nghĩa: chỉ cần tô mỗi phần một màu là được.

Tính chất thứ hai phức tạp hơn một chút. Hãy thử dùng hai màu để tô đồ thị $G$. Vì các thành phần liên thông tô màu độc lập, chỉ cần xét từng thành phần liên thông. Chọn một đỉnh $s$ bất kỳ trong thành phần, thực hiện DFS và ghi lại khoảng cách từ $s$ đến mỗi đỉnh $v$. Theo quy nạp trên cây DFS, nếu tồn tại cách tô màu hợp lý, thì mỗi đỉnh $v$ sẽ được tô màu dựa trên tính chẵn lẻ của khoảng cách từ $s$ đến $v$.

![](./images/bi-graph-2.svg)

Tiếp theo, xét các cạnh không thuộc cây DFS. Nếu hai đầu mút của cạnh này khác màu, thì cách tô màu hiện tại là hợp lệ; ngược lại, nếu cùng màu thì không thể tô màu hợp lệ. Hơn nữa, hai đỉnh khác màu khi và chỉ khi khoảng cách từ chúng đến gốc $s$ một chẵn một lẻ, tức là cạnh này tạo thành chu trình chẵn. Do đó, chỉ cần không có chu trình lẻ, các cạnh ngoài cây DFS đều nối hai đỉnh khác màu, và toàn bộ đồ thị có thể tô bằng hai màu, tức là đồ thị hai phía.

## Kiểm tra

Để kiểm tra một đồ thị có phải là đồ thị hai phía hay không, chỉ cần thử tô màu như trên. Có thể dùng [DFS](./dfs.md) hoặc [BFS](./bfs.md) để duyệt đồ thị. Nếu phát hiện chu trình lẻ (không thể tô màu), thì không phải đồ thị hai phía; ngược lại, nếu tô màu thành công thì là đồ thị hai phía.

Quy trình cụ thể:

-   Duyệt qua các đỉnh, nếu gặp đỉnh chưa tô màu thì đó là một thành phần liên thông mới.
-   Tô màu bất kỳ cho đỉnh đó, rồi dùng [DFS](./dfs.md) hoặc [BFS](./bfs.md) để tô màu cho cả thành phần liên thông.
-   Khi duyệt các đỉnh kề, nếu gặp đỉnh đã tô màu, kiểm tra xem có trùng màu với đỉnh hiện tại không. Nếu trùng màu thì không phải đồ thị hai phía, trả về luôn; nếu không thì tiếp tục duyệt.
-   Nếu gặp đỉnh chưa tô màu, tô màu ngược lại với đỉnh hiện tại.

Tham khảo mã nguồn sau:

???+ example "Mã nguồn tham khảo"
    ```cpp
    --8<-- "docs/graph/code/bi-graph/check-bipartite.cpp:core"
    ```

Độ phức tạp thuật toán là $O(|V|+|E|)$.

## Ứng dụng

Nhờ cấu trúc đơn giản, nhiều bài toán tối ưu trên đồ thị có thể giải hiệu quả trên đồ thị hai phía. Xem chi tiết ở các chủ đề liên quan.

-   Cực đại (trivial)
-   Tô màu đỉnh tối thiểu (trivial)
-   [Tô màu cạnh tối thiểu](./color.md#二分图-vizing-定理的构造性证明)
-   [Ghép cặp cực đại](./graph-matching/bigraph-match.md)
-   [Bao phủ cạnh tối thiểu](./graph-matching/graph-match.md#最小权边覆盖)
-   [Bao phủ đỉnh tối thiểu](./graph-matching/bigraph-match.md#二分图最小点覆盖)
-   [Tập độc lập cực đại](./graph-matching/bigraph-match.md#二分图最大独立集)
-   [Ghép cặp trọng số cực đại](./graph-matching/bigraph-weight-match.md)
-   [Lý thuyết trò chơi trên đồ thị hai phía](../math/game-theory/impartial-game.md#二分图博弈)
