Cây AA là một cấu trúc cây cân bằng được sử dụng để lưu trữ và truy xuất dữ liệu có thứ tự một cách hiệu quả. Giáo sư Arne Andersson đã giới thiệu nó trong bài báo "Balanced search trees made simple" năm 1993 với mục đích giảm bớt các trường hợp cần xem xét so với cây Đỏ-Đen. Cây AA có thể thực hiện tìm kiếm, chèn và xóa trong thời gian $O(\log N)$. Dưới đây là một ví dụ về cây AA.

![aa-tree-1](images/aa-tree-1.jpg)

Cây AA là một biến thể của cây Đỏ-Đen. Khác với cây Đỏ-Đen, các nút đỏ trên cây AA chỉ có thể là nút con bên phải. Điều này khiến cây AA mô phỏng cây 2-3 thay vì cây 2-3-4, từ đó đơn giản hóa đáng kể các thao tác bảo trì. Các thuật toán bảo trì cây Đỏ-Đen cần xem xét bảy trường hợp khác nhau để cân bằng cây một cách chính xác.

![red-black tree](images/aa-tree-2.svg)

Vì nút đỏ chỉ có thể là nút con bên phải, cây AA chỉ cần xem xét hai trường hợp.

![aa-tree](images/aa-tree-3.svg)

## Định nghĩa

Cây AA tuân theo các quy tắc giống như cây Đỏ-Đen, nhưng thêm một quy tắc mới, **đó là nút đỏ không được xuất hiện dưới dạng con bên trái**.

1. Mỗi nút có thể là đỏ hoặc đen.
2. Nút gốc luôn là màu đen.
3. Các nút lá (NULL) luôn là màu đen.
4. Hai nút con của một nút đỏ phải là màu đen, nghĩa là không có hai nút đỏ liền kề.
5. Mỗi đường đi từ nút gốc đến nút NULL đều có cùng số lượng nút đen.
6. Nút đỏ chỉ có thể là nút con bên phải.

## Bảo trì cân bằng

Mỗi nút của cây AA duy trì một trường **level**, tương tự như mỗi nút của cây Đỏ-Đen duy trì một trường màu sắc ("RED" hoặc "BLACK"). Quy định về level thỏa mãn 5 điều kiện sau:

1. Level của mỗi nút lá là 1.
2. Level của mỗi nút con bên trái bằng level của nút cha trừ đi 1.
3. Level của mỗi nút con bên phải bằng level của nút cha hoặc bằng level của nút cha trừ đi 1.
4. Level của mỗi nút cháu nội bên phải phải nhỏ hơn nghiêm ngặt level của nút ông nội.
5. Mỗi nút có level lớn hơn 1 đều có hai con.

![aa-tree-4](images/aa-tree-4.jpg)

### Liên kết ngang (Horizontal Link)

Liên kết trong đó level của nút con bằng level của nút cha được gọi là **liên kết ngang**, tương tự như liên kết đỏ trong cây Đỏ-Đen. Cho phép liên kết ngang bên phải đơn lẻ, nhưng không cho phép các liên kết ngang bên phải liên tiếp; không cho phép liên kết ngang bên trái. Những hạn chế này chặt chẽ hơn so với cây Đỏ-Đen, do đó quá trình cân bằng cây AA đơn giản hơn nhiều về mặt thuật toán so với cây Đỏ-Đen.

![aa-tree-5](images/aa-tree-5.jpg)

Các thao tác chèn và xóa có thể tạm thời khiến cây AA mất cân bằng (vi phạm tính bất biến của cây AA). Để khôi phục sự cân bằng chỉ cần hai thao tác khác nhau: "**skew**" (nghiêng) và "**split**" (phân tách). "Skew" là thực hiện xoay phải một cây con chứa liên kết ngang bên trái để thay thế bằng một cây con chứa liên kết ngang bên phải. "Split" là thực hiện xoay trái và tăng level để thay thế một cây con chứa hai hoặc nhiều liên kết ngang bên phải liên tiếp, biến nó thành một cây con chứa ít liên kết ngang bên phải liên tiếp hơn. Việc triển khai chèn và xóa duy trì cân bằng trở nên đơn giản hơn bằng cách dựa vào các thao tác "skew" và "split" để chỉ sửa đổi cây khi cần thiết, thay vì để người gọi quyết định có thực hiện "skew" hay "split" hay không.

### split (Xoay trái)

Xuất hiện chuỗi liên kết ngang liên tiếp hướng sang phải (ba nút liên tiếp bên phải thuộc cùng một level, nút R và nút X đều là nút đỏ).

Lúc này xoay trái nút *T*, coi các nút nhỏ hơn hoặc bằng level này là một cây con.

1. Con bên phải của gốc cây con trở thành gốc cây con mới;
2. Gốc cây con ban đầu trở thành con bên trái của gốc cây con mới;
3. Level của gốc cây con mới +1.

![aa-tree-split](images/aa-tree-split.svg)

???+ note "Triển khai mã giả"
    $$
    \begin{array}{ll}
    1 & \textbf{function } \text{split}(\text{root}) \\
    2 & \qquad \textbf{if } \text{root}\rightarrow\text{right}\rightarrow\text{right}\rightarrow\text{level} == \text{root}\rightarrow\text{level} \\
    3 & \qquad\qquad \text{rotate\_left}(\text{root}) \\
    4 & \textbf{end function}
    \end{array}
    $$

### skew (Xoay phải)

Xuất hiện chuỗi liên kết ngang hướng sang trái (hai nút liên tiếp bên trái thuộc cùng một level).

Xoay phải nút *T*, coi các nút nhỏ hơn hoặc bằng level này là một cây con.

1. Con bên trái của gốc cây con trở thành gốc cây con mới;
2. Gốc cây con ban đầu trở thành con bên phải của gốc cây con mới.

![aa-tree-skew](images/aa-tree-skew.svg)

???+ note "Triển khai mã giả"
    $$
    \begin{array}{ll}
    1 & \textbf{function } \text{skew}(\text{root}) \\
    2 & \qquad \textbf{if } \text{root}\rightarrow\text{left}\rightarrow\text{level} == \text{root}\rightarrow\text{level} \\
    3 & \qquad\qquad \text{rotate\_right}(\text{root}) \\
    4 & \textbf{end function}
    \end{array}
    $$

## Thao tác trên cây AA

Bản thân cây AA là một cây tìm kiếm nhị phân, vì vậy thao tác tìm kiếm giống như các cây tìm kiếm nhị phân khác. Các thao tác chèn và xóa tương tự như cây *AVL*, đầu tiên chèn hoặc xóa khóa (key) trong cây, sau đó quay lui dọc theo đường tìm kiếm về gốc và tái cấu trúc cây trong quá trình này.

### Chèn

???+ note "Triển khai mã giả"
    $$
    \begin{array}{ll}
    1 & \textbf{function } \text{insert}(\text{root}, \text{add}) \\
    2 & \qquad \textbf{if } \text{root} == \text{NULL} \\
    3 & \qquad\qquad \text{root} \gets \text{add} \\
    4 & \qquad \textbf{else if } \text{add}\rightarrow\text{key} < \text{root}\rightarrow\text{key} \\ 
    5 & \qquad\qquad \text{insert}(\text{root}\rightarrow\text{left}, \text{add}) \\
    6 & \qquad \textbf{else if } \text{add}\rightarrow\text{key} > \text{root}\rightarrow\text{key} \\
    7 & \qquad\qquad \text{insert}(\text{root}\rightarrow\text{right}, \text{add}) \\
    8 & \qquad \textbf{end if} \\
    9 & \qquad \text{// Thực hiện skew và split trên mỗi level} \\
    10 & \qquad \text{skew}(\text{root}); \\
    11 & \qquad \text{split}(\text{root}); \\
    12 & \textbf{end function}
    \end{array}
    $$

### Xóa

Quá trình xóa tương tự như các cây cân bằng nhị phân khác, đầu tiên chuyển đổi việc xóa nút nội bộ thành việc xóa nút lá. Phương pháp cụ thể là thay thế nút nội bộ bằng nút tiền nhiệm hoặc nút kế nhiệm gần nhất của nó. Vì tất cả các nút có level lớn hơn 1 của cây AA đều có hai nút con, nút tiền nhiệm hoặc nút kế nhiệm sẽ nằm ở level 1, việc xóa nút ở level 1 đơn giản hơn.

???+ note "Triển khai mã giả"
    $$
    \begin{array}{ll}
    1 &  \text{// Để tái cân bằng cây} \\
    2 &  \textbf{if} \ \text{root->left->level} < \text{root->level} -1 \ \textbf{or} \ \text{root->right->level} < \text{root->level} -1 \\
    3 &  \{ \\
    4 & \qquad \textbf{if} \ \text{root->right->level} > \text{--root->level} \\
    5 & \qquad \{ \\
    6 & \qquad\qquad \text{root->right->level} \gets \text{root->level} \\
    7 & \qquad \} \\
    8 & \qquad \text{skew}(\text{root}) \\
    9 & \qquad \text{skew}(\text{root->right}) \\
    10 & \qquad \text{skew}(\text{root->right->right}) \\
    11 & \qquad \text{split}(\text{root}) \\
    12 & \qquad \text{split}(\text{root->right}) \\
    13 &  \} \\
    \end{array}
    $$

## Hiệu suất

Hiệu suất của cây AA tương đương với hiệu suất của cây Đỏ-Đen. Mặc dù cây AA thực hiện nhiều thao tác xoay hơn cây Đỏ-Đen, nhưng thuật toán của cây AA đơn giản hơn, dẫn đến hiệu suất tương đương. Hiệu suất của cây Đỏ-Đen nhất quán hơn trong các tình huống khác nhau, trong khi cây AA có xu hướng phẳng hơn, điều này giúp cây AA có tốc độ tìm kiếm nhanh hơn một chút.

## Tài liệu tham khảo

1. [AA tree - Wikipedia](https://en.wikipedia.org/wiki/AA_tree)
2. [Introduction to AA trees](https://iq.opengenus.org/aa-trees/)
3. [AA tree - Visualization](https://kubokovac.eu/gnarley-trees/AAtree.html)
4. [CMSC 420 Lecture 6: 2-3, Red-black, and AA trees](https://www.cs.umd.edu/class/fall2019/cmsc420-0201/Lects/lect06-aa.pdf)