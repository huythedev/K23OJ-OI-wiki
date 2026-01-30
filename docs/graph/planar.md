Bài viết này giới thiệu về (đồ thị) phẳng và các khái niệm liên quan.

## Đồ thị phẳng

Nếu một đồ thị $G$ có thể được vẽ trên mặt phẳng $S$ sao cho không có hai cạnh nào cắt nhau (ngoại trừ tại các đỉnh), thì $G$ được gọi là **đồ thị có thể nhúng phẳng** (planar graph). Hình vẽ không có cạnh cắt nhau của $G$ gọi là biểu diễn phẳng của $G$ hoặc **nhúng phẳng** (planar embedding). Nhúng phẳng của một đồ thị có thể nhúng phẳng cũng được gọi là **đồ thị phẳng** (plane graph).

???+ info "“Đồ thị phẳng”"
    Trong các tài liệu tiếng Trung khác nhau, khái niệm “đồ thị phẳng” có thể khác nhau. Trong định nghĩa của bài này, “đồ thị có thể nhúng phẳng” là một đối tượng đồ thị, có thể có nhiều cách nhúng khác nhau lên mặt phẳng; còn “đồ thị phẳng” là một đối tượng hình học, ngoài cấu trúc đồ thị còn cần chỉ định cách vẽ cụ thể. Một đồ thị có thể nhúng phẳng thường tương ứng với nhiều đồ thị phẳng. Do đó, trong bài này, nếu kết luận chỉ phụ thuộc vào cấu trúc đồ thị, sẽ dùng từ “đồ thị có thể nhúng phẳng”; nếu còn phụ thuộc vào cách nhúng, sẽ dùng từ “đồ thị phẳng”.

Ví dụ đơn giản về đồ thị phẳng:

![](images/planar-1.svg)

(Bên trái: đồ thị bướm; bên phải: đồ thị đầy đủ bậc $4$ $K_4$)

Ví dụ về đồ thị không phẳng:

![](images/planar-2.svg)

(Bên trái: đồ thị đầy đủ bậc $5$ $K_5$; bên phải: đồ thị hai phía đầy đủ $K_{3,3}$ với mỗi phía $3$ đỉnh)

## Tính chất

Phần này trình bày các tính chất của đồ thị phẳng.

### Mặt và bậc của mặt

Giả sử $G$ là một đồ thị phẳng, các cạnh của $G$ chia mặt phẳng thành nhiều vùng, mỗi vùng gọi là một **mặt** (face) của $G$. Trong đó, mặt không bị giới hạn gọi là **mặt vô hạn** (unbounded face) hoặc **mặt ngoài** (external face), các mặt còn lại gọi là mặt hữu hạn hoặc mặt trong. Mỗi đồ thị phẳng chỉ có duy nhất một mặt ngoài.

Chu trình tạo bởi các cạnh bao quanh một mặt gọi là **biên** (boundary) của mặt đó, các cạnh trên biên được gọi là **liên thuộc** (incident) với mặt. Độ dài của biên gọi là **bậc** (degree) của mặt. Khi tính bậc của mặt, mỗi cạnh cầu được tính hai lần. Tổng bậc của tất cả các mặt trong đồ thị phẳng bằng $2|E|$.

Trong đồ thị phẳng, mặt bậc $1$ tương ứng với khuyên (self-loop), mặt bậc $2$ thường tương ứng với cặp cạnh song song[^face-2]. Với đồ thị phẳng liên thông đơn giản có $|V|\ge 3$, mọi mặt đều có bậc ít nhất là $3$.

### Công thức Euler

Một tính chất quan trọng của đồ thị phẳng là **công thức Euler** (Euler's formula), liên hệ giữa số đỉnh $|V|$, số cạnh $|E|$ và số mặt $|F|$.

???+ note "Công thức Euler"
    Với đồ thị phẳng liên thông $G$, ta có
    $$
    |V| - |E| + |F| = 2.
    $$

??? note "Chứng minh"
    Chứng minh bằng quy nạp theo số mặt $|F|$. Cơ sở: $|F|=1$, khi đó đồ thị chỉ có một mặt ngoài, tất cả các cạnh là cạnh cầu, tức là $G$ là một cây, nên $|E|=|V|-1$, thay vào công thức Euler thấy đúng. Giả sử công thức Euler đúng với $|F|=k$. Với $|F|=k+1$, tồn tại cạnh không phải là cạnh cầu $e$, là cạnh chung của hai mặt. Xóa $e$ khỏi $G$ được đồ thị $G-e$ có $|V|$ đỉnh, $|E|-1$ cạnh, $|F|-1$ mặt. Theo giả thiết quy nạp, $|V|-(|E|-1)+(|F|-1)=2$, sắp xếp lại được công thức Euler cho $G$. Vậy công thức Euler đúng với mọi đồ thị phẳng.

???+ note "Hệ quả"
    Với đồ thị phẳng có $k$ thành phần liên thông, ta có
    $$
    |V| - |E| + |F| = k + 1.
    $$

??? note "Chứng minh"
    Mỗi thành phần liên thông là một đồ thị phẳng, nhưng các thành phần này dùng chung một mặt ngoài. Áp dụng công thức Euler cho từng thành phần rồi cộng lại, tổng số đỉnh và cạnh là đúng, nhưng tổng số mặt bị thừa $(k-1)$ do mặt ngoài được tính $k$ lần. Sửa lại được $|V|-|E|+|F| = 2k - (k-1) = k+1$.

Từ đây, có thể suy ra mối quan hệ giữa số cạnh và số đỉnh của đồ thị phẳng.

???+ note "Định lý"
    Với đồ thị phẳng có $k$ thành phần liên thông, nếu mọi mặt đều có bậc ít nhất $l \ge 3$, thì
    $$
    |E| \le \dfrac{l}{l-2}(|V|-k-1).
    $$

??? note "Chứng minh"
    Vì mỗi mặt có bậc ít nhất $l$, tổng bậc các mặt ít nhất là $l|F|$, tức là $2|E| \ge l|F|$. Thay vào hệ quả của công thức Euler $|V| - |E| + |F| = k + 1$, được
    $$
    2|E| \ge l(k + 1 - |V| + |E|).
    $$
    Giải ra được
    $$
    |E| \le \dfrac{l}{l-2}(|V|-k-1).
    $$

???+ note "Hệ quả"
    Giả sử $G$ là đồ thị đơn giản có thể nhúng phẳng, $|V|\ge 3$, thì
    $$
    |E| \le 3|V|-6.
    $$

??? note "Chứng minh"
    Nếu $G$ liên thông, mọi mặt đều có bậc ít nhất $3$. Thay $k=1$, $l=3$ vào định lý trên được $|E|\le 3|V|-6$.

    Nếu $G$ không liên thông, xét hai trường hợp:
    - Nếu có thành phần liên thông có ít nhất $3$ đỉnh, áp dụng bất đẳng thức $|E_i|\le 3|V_i|-6$ cho từng thành phần, các thành phần có ít hơn $3$ đỉnh thì $|E_i|\le |V_i| \le 3|V_i|$. Cộng lại được $|E|\le 3|V|-6$.
    - Nếu mọi thành phần liên thông đều có ít hơn $3$ đỉnh, thì $|E|\le |V|$, mà $|V|\le 3|V|-6$ với $|V|\ge 3$, nên $|E|\le 3|V|-6$ vẫn đúng.

    Vậy mệnh đề được chứng minh.

Hệ quả này cho thấy đồ thị đơn giản có thể nhúng phẳng là đồ thị thưa.

### Đồ thị đối ngẫu

Mỗi đồ thị phẳng đều có một (đồ thị) đối ngẫu tương ứng.

![](images/planar-dual-1.svg)

Giả sử $G$ là đồ thị phẳng, có thể xây dựng đồ thị $G^*$ như sau:

1.  Với mỗi mặt $f_i$ của $G$, đặt một đỉnh $v_i^*$ bên trong mặt đó.
2.  Với mỗi cạnh $e$ của $G$, nếu $e$ nằm trên biên chung của hai mặt $f_i$ và $f_j$, thì vẽ một cạnh $e^*$ nối $v_i^*$ và $v_j^*$, sao cho $e^*$ cắt $e$ đúng một lần và không cắt các cạnh khác của $G$ hoặc $G^*$. Đặc biệt, nếu $e$ chỉ nằm trên biên của một mặt $f_i$, thì vẽ một khuyên nối $v_i^*$, cắt $e$.

Đồ thị thu được $G^*$ gọi là **đồ thị đối ngẫu** (dual graph) của $G$.

???+ note "Định lý"
    Giả sử $G^*$ là đồ thị đối ngẫu của đồ thị phẳng $G$. Khi đó, $G^*$ là đồ thị phẳng liên thông. Hơn nữa, $G^{**}$ là đối ngẫu của $G^*$, $G^{**}$ đẳng cấu với $G$ khi và chỉ khi $G$ liên thông.

??? note "Chứng minh"
    $G^*$ là đồ thị phẳng do cách xây dựng. Cần chứng minh $G^*$ liên thông. Với hai đỉnh bất kỳ $v^*_i, v^*_j$ của $G^*$, nối chúng bằng đoạn thẳng trên mặt phẳng, đoạn này đi qua các mặt và cạnh của $G$ theo thứ tự $f_i,e_{s_1},f_{s_1},\cdots,f_{s_{r-1}},e_{s_r},f_j$, tương ứng với các đỉnh và cạnh $v_i^*,e_{s_1}^*,v^*_{s_1},\cdots,v^*_{s_{r-1}},e^*_{s_r},v^*_j$ trong $G^*$. Theo cách xây dựng, các đỉnh và cạnh này liên kết với nhau, nên tồn tại đường đi trong $G^*$. Vậy $G^*$ liên thông.

    $G^{**}$ là đối ngẫu của $G^*$, nên cũng liên thông. Do đó, $G$ và $G^{**}$ đẳng cấu là điều kiện cần $G$ liên thông. Ngược lại, nếu $G$ liên thông, cần chứng minh $G$ thỏa mãn điều kiện xây dựng đối ngẫu của $G^*$. Các cạnh của $G^*$ và $G$ tương ứng nhau, chỉ cần chứng minh mỗi mặt của $G^*$ chứa đúng một đỉnh của $G$. Với mỗi mặt $f^*$ của $G^*$, lấy một cạnh $e^*$ trên biên, cạnh $e$ tương ứng của $G$ có một đầu mút nằm trong $f^*$. Vì $G^*$ và $G$ đều liên thông, áp dụng công thức Euler, số cạnh của $G$ và $G^*$ bằng nhau, số mặt của $G$ bằng số đỉnh của $G^*$, nên số đỉnh của $G$ bằng số mặt của $G^*$. Vậy mỗi mặt của $G^*$ chứa đúng một đỉnh của $G$. Định lý được chứng minh.

Có nhiều mối liên hệ giữa cấu trúc của đồ thị phẳng và đồ thị đối ngẫu:

-   Mặt của $G$ tương ứng với đỉnh của $G^*$, cạnh của $G$ tương ứng với cạnh của $G^*$, đỉnh của $G$ tương ứng với mặt của $G^*$.
-   Khuyên trong $G$ tương ứng với cạnh cầu trong $G^*$, khuyên trong $G^*$ tương ứng với cạnh cầu trong $G$.
-   Tập cắt cạnh trong $G$ tương ứng với chu trình trong $G^*$, chu trình trong $G^*$ tương ứng với tập cắt cạnh trong $G$.

Lưu ý: Khái niệm đối ngẫu chỉ xác định trên một đồ thị phẳng cụ thể, không xác định trên mọi đồ thị có thể nhúng phẳng. Thực tế, hai đồ thị phẳng đẳng cấu có thể có đồ thị đối ngẫu không đẳng cấu. Nói cách khác, cùng một đồ thị nhưng với các nhúng phẳng khác nhau có thể có đối ngẫu khác nhau.

???+ example "Ví dụ"
    Hình dưới đây vẽ hai đồ thị phẳng đẳng cấu, nhưng đồ thị đối ngẫu của chúng không đẳng cấu.

    ![](images/planar-dual-2.svg)

    Nguyên nhân là ở hình bên phải có mặt bậc một, nên đồ thị đối ngẫu có đỉnh bậc một, còn hình bên trái thì không.

Chuyển bài toán trên đồ thị phẳng sang đồ thị đối ngẫu đôi khi giúp giải quyết dễ hơn. Ví dụ điển hình là bài toán [min-cut trên đồ thị phẳng](./flow/min-cut.md) có thể chuyển thành bài toán [đường đi ngắn nhất trên đồ thị đối ngẫu](./shortest-path.md). Giả sử $G$ là đồ thị phẳng có trọng số cạnh, $s,t$ là hai đỉnh, cần tìm min-cut giữa $s$ và $t$.

![](images/planar-dual-3.svg)

Như hình, chọn nhúng phẳng phù hợp, luôn có thể đặt $s,t$ trên biên mặt ngoài. Thêm các tia kéo dài từ $s$ và $t$ ra ngoài, chia mặt ngoài thành hai phần $f_{+}$ và $f_{-}$. Dựa trên đó, xây dựng đồ thị đối ngẫu, gán trọng số cho các cạnh tương ứng. Khi đó, đường đi giữa hai đỉnh tương ứng với $f_{+}$ và $f_{-}$ trong $G^*$ (đường đỏ đậm) một-một với các min-cut giữa $s$ và $t$ trong $G$ (đường đen đậm), và tổng trọng số bằng nhau. Như vậy, tìm đường đi ngắn nhất trong đồ thị đối ngẫu chính là tìm min-cut trong đồ thị gốc.

### Một số kết quả nổi tiếng khác

Đồ thị phẳng còn có nhiều kết quả nổi tiếng khác, dưới đây chỉ liệt kê mà không bàn sâu.

???+ note "Định lý bốn màu"
    (Đồ thị phẳng không có khuyên) luôn tô được $4$ màu.

???+ note "Định lý Fáry"
    Đồ thị đơn giản có thể nhúng phẳng luôn tồn tại một nhúng phẳng mà mọi cạnh đều là đoạn thẳng.

???+ note "Định lý (Wood)"
    Đồ thị có thể nhúng phẳng có nhiều nhất $8|V|-16$ clique cực đại.

???+ note "Định lý (Tutte)"
    Đồ thị có thể nhúng phẳng $4$-liên thông luôn là đồ thị Hamilton.

## Nhận biết đồ thị phẳng

Phần này bàn về cách xác định một đồ thị có phải là đồ thị có thể nhúng phẳng hay không.

### Đồ thị cấm (forbidden graph)

Cách mô tả kinh điển nhất về đồ thị có thể nhúng phẳng là dùng **đồ thị cấm**.

Trước hết, $K_5$ và $K_{3,3}$ không phải là đồ thị có thể nhúng phẳng.

???+ note "Định lý"
    $K_5$ và $K_{3,3}$ không phải là đồ thị có thể nhúng phẳng.

??? note "Chứng minh"
    Như đã trình bày, với đồ thị phẳng liên thông đơn giản $|V|\ge 3$ phải thỏa mãn
    $$
    |E| \le \dfrac{l}{l-2}(|V|-2).
    $$
    Trong đó $l$ là bậc mặt nhỏ nhất. Với $K_5$, $l=3,~|V|=5,~|E|=10$, nên không thể nhúng phẳng. Với $K_{3,3}$, $l=4,~|V|=6,~|E|=9$, cũng không thể nhúng phẳng.

Thực tế, chúng chính là cấu trúc nhỏ nhất khiến một đồ thị không thể nhúng phẳng. Tức là, chỉ cần đồ thị không chứa hai đồ thị này (dưới một số phép biến đổi) làm cấu trúc con, thì đồ thị đó là đồ thị có thể nhúng phẳng.

Định lý nhận biết đồ thị phẳng đầu tiên là định lý Kuratowski, sử dụng khái niệm đồng hình (homeomorphic): Hai đồ thị $G_1, G_2$ gọi là **đồng hình** nếu chúng đẳng cấu, hoặc có thể chuyển thành nhau bằng cách chèn hoặc xóa các đỉnh bậc $2$. Khi đó:

???+ note "Định lý Kuratowski"
    Đồ thị $G$ là đồ thị có thể nhúng phẳng khi và chỉ khi $G$ không chứa đồ thị con đồng hình với $K_5$ hoặc $K_{3,3}$.

Một định lý liên quan là định lý Wagner, sử dụng phép co cạnh: Lặp lại nhiều lần phép co một cạnh thành một đỉnh. Khi đó:

???+ note "Định lý Wagner"
    Đồ thị $G$ là đồ thị có thể nhúng phẳng khi và chỉ khi $G$ không có đồ thị con nào co được thành $K_5$ hoặc $K_{3,3}$.

Việc không chứa các loại đồ thị con này là điều kiện cần khá hiển nhiên, nên điểm mấu chốt của hai định lý trên là tính đủ của điều kiện cấm đồ thị. Vì mọi đồ thị con đồng hình với $K_5$ hoặc $K_{3,3}$ đều co được thành chúng, nhưng ngược lại không đúng, nên định lý Kuratowski cho điều kiện yếu hơn và dễ kiểm tra hơn.

### Thuật toán kiểm tra tính phẳng

Dù có vẻ không dễ, bài toán kiểm tra tính phẳng thực tế có nhiều thuật toán tuyến tính. Tuy nhiên, các thuật toán này thường phức tạp, hiếm khi xuất hiện trong các kỳ thi lập trình.

Thuật toán tuyến tính đầu tiên là Hopcroft–Tarjan[^ht74], nhưng rất khó cài đặt. Thuật toán de Fraysseix–Ossona de Mendez–Rosenstiehl (còn gọi là thuật toán LR)[^dor06][^df08][^bra09] cải tiến quy trình của Hopcroft–Tarjan, là một trong những thuật toán kiểm tra tính phẳng tốt nhất hiện nay. Thư viện NetworkX của Python đã [cài đặt](https://github.com/networkx/networkx/blob/main/networkx/algorithms/planarity.py) thuật toán này.

Một thuật toán nổi bật khác là Boyer–Myrvold[^bm99][^bm04], kiểm tra tính phẳng trong thời gian tuyến tính. Nếu đồ thị có thể nhúng phẳng, thuật toán trả về một nhúng phẳng; nếu không, trả về một đồ thị con Kuratowski (đồng hình với $K_5$ hoặc $K_{3,3}$). Thư viện Boost của C++ đã [cài đặt](https://www.boost.org/doc/libs/1_67_0/boost/graph/planar_detail/boyer_myrvold_impl.hpp) thuật toán này.

Tham khảo thêm các tài liệu ở cuối bài.

## Một số loại đồ thị phẳng đặc biệt

Phần này giới thiệu một số lớp đồ thị phẳng đặc biệt.

### Đồ thị phẳng cực đại

Với đồ thị đơn giản có thể nhúng phẳng $G$, nếu thêm bất kỳ cạnh nào giữa hai đỉnh không kề nhau đều làm mất tính phẳng, thì $G$ gọi là **đồ thị phẳng cực đại** (maximal planar graph). Nhúng phẳng của đồ thị này gọi là **đồ thị phẳng cực đại**.

???+ note "Định lý"
    Đồ thị phẳng cực đại $G$ luôn liên thông. Hơn nữa, nếu $|V|\ge 3$, $G$ không có cạnh cầu.

??? note "Chứng minh"
    Nếu $G$ không liên thông, chọn một nhúng phẳng bất kỳ, luôn có thể nối hai đỉnh thuộc hai thành phần liên thông khác nhau trong mặt ngoài, vẫn được đồ thị phẳng, mâu thuẫn với tính cực đại. Vậy $G$ phải liên thông.

    Nếu $|V|\ge 3$ và $G$ có cạnh cầu $e=(u,v)$, xóa $e$ được hai thành phần liên thông, $u,v$ thuộc hai thành phần khác nhau. Giả sử thành phần của $v$ có ít nhất hai đỉnh. Vẽ thành phần của $u$ trên mặt phẳng, chọn một mặt chứa $u$, vẽ thành phần của $v$ trong mặt đó. Vì là đồ thị đơn giản, mặt ngoài của thành phần $v$ không phải là khuyên, nên còn có đỉnh $w\neq u,v$. Nối $v,w$ với $u$ sẽ tạo ra đồ thị phẳng chứa $G$ như đồ thị con, mâu thuẫn với tính cực đại. Vậy $G$ không có cạnh cầu.

Cấu trúc của đồ thị phẳng cực đại có thể mô tả chính xác hơn.

???+ note "Định lý"
    Với đồ thị phẳng $G$ có $|V|\ge 3$, $G$ là đồ thị phẳng cực đại khi và chỉ khi $G$ là đồ thị đơn giản và mọi mặt đều có bậc $3$.

??? note "Chứng minh"
    Điều kiện đủ là hiển nhiên. Chỉ cần chứng minh điều kiện cần: Nếu $G$ là đồ thị phẳng cực đại, $|V|\ge 3$, thì mọi mặt đều có bậc $3$. Vì $G$ là đồ thị phẳng liên thông đơn giản, mọi mặt có bậc ít nhất $3$. Giả sử tồn tại mặt $f$ có bậc ít nhất $4$, biên là chu trình $v_1v_2v_3v_4\cdots v_1$. Nếu $v_1$ và $v_3$ không kề nhau, nối chúng trong mặt $f$ không phá vỡ tính phẳng, mâu thuẫn với tính cực đại, nên $v_1$ và $v_3$ kề nhau; tương tự $v_2$ và $v_4$. Nhưng hai cạnh $(v_1,v_3)$ và $(v_2,v_4)$ không nằm trong mặt $f$, tức là nằm ngoài mặt $f$. Dù vẽ thế nào, hai cạnh này sẽ cắt nhau, điều này là không thể. Vậy mọi mặt đều có bậc $3$.

???+ note "Hệ quả"
    Với đồ thị $G$ có $|V|\ge 3$, luôn có $|E|=3|V|-6$ và $|F|=2|V|-4$.

Vì mọi mặt trong đồ thị phẳng cực đại đều là tam giác, nên còn gọi là **tam giác hóa phẳng** (plane triangulation).

### Đồ thị phẳng ngoài

Giả sử $G$ là đồ thị có thể nhúng phẳng, nếu tồn tại một nhúng phẳng $\tilde{G}$ sao cho mọi đỉnh của $G$ đều nằm trên biên của một mặt, thì $G$ gọi là **đồ thị phẳng ngoài** (outerplanar graph). Nhúng này gọi là nhúng phẳng ngoài hoặc **đồ thị phẳng ngoài**. Thường vẽ mặt ngoài là mặt chứa tất cả các đỉnh.

![](images/planar-outer.svg)

Đồ thị phẳng ngoài là đồ thị có thể nhúng phẳng, nhưng ngược lại không đúng. Đồ thị phẳng ngoài cũng có thể mô tả bằng đồ thị cấm.

???+ note "Định lý"
    Đồ thị $G$ là đồ thị phẳng ngoài khi và chỉ khi $G$ không chứa đồ thị con đồng hình với $K_4$ hoặc $K_{2,3}$.

Với đồ thị phẳng ngoài, cũng có thể xét khái niệm đồ thị phẳng ngoài cực đại. Với đồ thị phẳng ngoài đơn giản $G$, nếu thêm bất kỳ cạnh nào giữa hai đỉnh không kề nhau đều làm mất tính phẳng ngoài, thì $G$ gọi là **đồ thị phẳng ngoài cực đại** (maximal outerplanar graph). Nhúng ngoài của đồ thị này gọi là **đồ thị phẳng ngoài cực đại**. Đồ thị phẳng ngoài cực đại thực chất là tam giác hóa của đa giác trên mặt phẳng.

???+ note "Định lý"
    Với đồ thị phẳng ngoài cực đại $G$ có $|V|\ge 3$ và mọi đỉnh đều nằm trên biên mặt ngoài, $G$ có đúng $|V|-2$ mặt trong.

??? note "Chứng minh"
    Chứng minh quy nạp theo $|V|$. Cơ sở: $|V|=3$, $G$ là tam giác, có $1$ mặt trong, đúng. Giả sử đúng với $|V|=k$, chứng minh với $|V|=k+1$.

    Đầu tiên, $G$ luôn có đỉnh bậc $2$. Nếu không, ngoài hai đỉnh kề nhau trên biên mặt ngoài, mọi đỉnh đều phải nối với một đỉnh thứ ba. Đánh số các đỉnh trên biên, với mỗi $i=1,2,\cdots,k+1$, đặt $f(i)$ là đỉnh nhỏ nhất không kề $i$ nhưng nối với $i$. Xét các giá trị $f(i)$, quá trình này sẽ dừng lại ở một $i^*$, khi đó $i^*<i^*+1<f(i^*)$, lặp lại lập luận sẽ mâu thuẫn với tính cực đại. Vậy luôn tồn tại đỉnh bậc $2$.

    Gọi $v$ là đỉnh bậc $2$, xóa $v$ khỏi $G$ được đồ thị phẳng ngoài cực đại $G-v$ với $k$ đỉnh, theo giả thiết quy nạp có $k-2$ mặt trong, xóa $v$ giảm đúng một mặt trong, nên $G$ có $k-1$ mặt trong. Định lý được chứng minh.

???+ note "Định lý"
    Với đồ thị phẳng ngoài $G$ có $|V|\ge 3$ và mọi đỉnh đều nằm trên biên mặt ngoài, $G$ là đồ thị phẳng ngoài cực đại khi và chỉ khi biên mặt ngoài là chu trình độ dài $|V|$, và mọi mặt trong đều là tam giác.

??? note "Chứng minh"
    Điều kiện đủ là hiển nhiên. Nếu nối hai đỉnh không kề nhau trên biên mặt ngoài, hoặc là không còn mặt nào chứa tất cả các đỉnh, hoặc cạnh mới sẽ cắt biên mặt trong.

    Ngược lại, giả sử biên mặt ngoài không là chu trình, tức là có đỉnh xuất hiện nhiều lần, khi đó có thể nối hai đỉnh không kề nhau trên biên ngoài mà vẫn giữ tính phẳng ngoài, mâu thuẫn với tính cực đại. Mọi mặt trong là tam giác do lý do tương tự như đồ thị phẳng cực đại.

???+ note "Hệ quả"
    Với đồ thị phẳng ngoài cực đại $G$ có $|V|\ge 3$:
    1.  $|E|=2|V|-3$.
    2.  Có ít nhất $3$ đỉnh bậc không quá $3$, và ít nhất $2$ đỉnh bậc $2$.
    3.  Độ liên thông đỉnh của $G$ là $2$.

## Bài tập

-   [Luogu P3209 \[HNOI2010\] Kiểm tra đồ thị phẳng](https://www.luogu.com.cn/problem/P3209)
-   [Luogu P3249 \[HNOI2016\] Mỏ quặng](https://www.luogu.com.cn/problem/P3249)
-   [Luogu P4001 \[ICPC-Beijing 2006\] Sói bắt thỏ](https://www.luogu.com.cn/problem/P4001)
-   [Luogu P4073 \[WC2013\] Đồ thị phẳng](https://www.luogu.com.cn/problem/P4073)
-   [Luogu P7295 \[USACO21JAN\] Paint by Letters P](https://www.luogu.com.cn/problem/P7295)

## Tài liệu tham khảo & chú thích

-   [Planar graph - Wikipedia](https://en.wikipedia.org/wiki/Planar_graph)
-   [Planarity testing - Wikipedia](https://en.wikipedia.org/wiki/Planarity_testing)
-   Bondy, John Adrian, and Uppaluri Siva Ramachandra Murty. Graph theory with applications. Vol. 290. London: Macmillan, 1976.
-   Diestel, Reinhard. Graph theory. Vol. 173. Springer Nature, 2025.
-   Patrignani, Maurizio. "Planarity Testing and Embedding." (2013): 1-42.

[^face-2]: Tuy nhiên đây không phải là trường hợp duy nhất. Hai khuyên lồng nhau cũng tạo thành mặt bậc hai. Ngoài ra, có mặt bậc hai không nhất thiết nghĩa là đồ thị không đơn giản, ví dụ đồ thị chỉ có một cạnh thì mặt duy nhất (mặt ngoài) cũng là mặt bậc hai.

[^ht74]: Hopcroft, John, and Robert Tarjan. "Efficient planarity testing." Journal of the ACM (JACM) 21, no. 4 (1974): 549-568.

[^dor06]: De Fraysseix, Hubert, Patrice Ossona De Mendez, and Pierre Rosenstiehl. "Trémaux trees and planarity." International Journal of Foundations of Computer Science 17, no. 05 (2006): 1017-1029.

[^df08]: De Fraysseix, Hubert. "Trémaux trees and planarity." Electronic Notes in Discrete Mathematics 31 (2008): 169-180.

[^bra09]: Brandes, Ulrik. "The left-right planarity test." Manuscript submitted for publication 3 (2009).

[^bm99]: Boyer, John M., and Wendy J. Myrvold. "Stop Minding Your p's and q's: A Simplified O (n) Planar Embedding Algorithm." In SODA, vol. 99, pp. 140-146. 1999.

[^bm04]: Boyer, John M., and Wendy J. Myrvold. "Simplified o (n) planarity by edge addition." Graph Algorithms and Applications 5 (2006): 241.
