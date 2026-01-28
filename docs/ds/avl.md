Cây AVL là một loại cây tìm kiếm nhị phân cân bằng. Do cách giới thiệu về AVL trong nhiều giáo trình thuật toán thường rất dài dòng, khiến nhiều người có ấn tượng rằng cây AVL phức tạp và không thực dụng. Tuy nhiên, trên thực tế, nguyên lý của cây AVL rất đơn giản và việc cài đặt cũng không hề phức tạp.

## Tính chất

1.  Cây nhị phân rỗng là một cây AVL.
2.  Nếu T là một cây AVL, thì các cây con bên trái và bên phải của nó cũng là cây AVL, và $|h(ls) - h(rs)| \leq 1$, trong đó $h$ là chiều cao của các cây con.
3.  Chiều cao của cây là $O(\log n)$.

Hệ số cân bằng: Chiều cao cây con bên phải - Chiều cao cây con bên trái.

???+ note "Chứng minh chiều cao cây"
    Gọi $f_n$ là số nút tối thiểu mà một cây AVL có chiều cao $n$ chứa, ta có:
    
    $$
    f_n=
    \begin{cases}
    1&(n=1)\\
    2&(n=2)\\
    f_{n-1}+f_{n-2}+1& (n>2)
    \end{cases}
    $$
    
    Theo cách giải phương trình sai phân tuyến tính không thuần nhất với hệ số hằng, $\{f_n+1\}$ là một dãy Fibonacci. Công thức tổng quát của $f_n$ là:
    
    $$
    f_n=\frac{5+2\sqrt{5}}{5}\left(\frac{1+\sqrt{5}}{2}\right)^n+\frac{5-2\sqrt{5}}{5}\left(\frac{1-\sqrt{5}}{2}\right)^n-1
    $$
    
    Dãy Fibonacci tăng trưởng theo tốc độ hàm mũ, đối với chiều cao cây $n$ ta có:
    
    $$
    n<\log_{\frac{1+\sqrt{5}}{2}} (f_n+1)<\frac{3}{2}\log_2 (f_n+1)
    $$
    
    Do đó chiều cao của cây AVL là $O(\log f_n)$, với $f_n$ là số lượng nút.

## Quá trình

### Chèn nút

Tương tự như trong BST (Cây tìm kiếm nhị phân), trước tiên thực hiện một lần tìm kiếm thất bại để xác định vị trí chèn. Sau khi chèn nút, dựa vào hệ số cân bằng để quyết định có cần điều chỉnh hay không.

### Xóa nút

Xóa nút tương tự như BST, đổi chỗ nút cần xóa với nút kế nhiệm (successor) rồi tiến hành xóa.

Việc xóa có thể dẫn đến thay đổi chiều cao cây và hệ số cân bằng, lúc này cần điều chỉnh dọc theo con đường từ nút bị xóa lên đến gốc.

### Duy trì sự cân bằng

Sau khi chèn hoặc xóa nút, tính chất 2 của cây AVL có thể bị phá vỡ. Do đó, cần duy trì cây dọc theo con đường từ nút vừa chèn/xóa đến gốc. Nếu tại một nút nào đó tính chất 2 không còn được thỏa mãn, vì chúng ta chỉ chèn/xóa một nút nên ảnh hưởng đến chiều cao cây không quá 1, do đó giá trị tuyệt đối của hệ số cân bằng tại nút đó tối đa là 2. Do tính đối xứng, ở đây chúng ta chỉ thảo luận trường hợp chiều cao cây con bên trái lớn hơn cây con bên phải là 2, tức là trong hình dưới đây $h(B)-h(E)=2$. Lúc này, cần thảo luận hai trường hợp dựa trên mối quan hệ giữa $h(A)$ và $h(C)$. Cần lưu ý rằng, vì chúng ta duy trì sự cân bằng từ dưới lên trên, nên đối với tất cả các hậu duệ của nút D, tính chất 2 vẫn được thỏa mãn.

![](./images/avl1.svg)

#### Trường hợp 1: Chiều cao nút A không nhỏ hơn chiều cao nút C

Gọi $h(E)=x$, ta có:

$$
\begin{cases}
    h(B)=x+2\\
    h(A)=x+1\\
    x\leq h(C)\leq x+1
\end{cases}
$$

Trong đó $h(C)\geq x$ là vì nút B thỏa mãn tính chất 2, nên chênh lệch giữa $h(C)$ và $h(A)$ không quá 1. Lúc này chúng ta thực hiện một thao tác xoay phải tại nút D (thao tác xoay giống như các loại cây tìm kiếm nhị phân cân bằng khác), như hình dưới đây.

![](./images/avl2.svg)

Rõ ràng chiều cao các nút A, C, E không thay đổi, và ta có:

$$
\begin{cases}
    0\leq h(C)-h(E)\leq 1\\
    x+1\leq h'(D)=\max(h(C),h(E))+1=h(C)+1\leq x+2\\
    0\leq h'(D)-h(A)\leq 1
end{cases}
$$

Vì vậy sau khi xoay, các nút B và D cũng thỏa mãn tính chất 2.

#### Trường hợp 2: Chiều cao nút A nhỏ hơn chiều cao nút C

Gọi $h(E)=x$, tương tự như trên, ta có:

$$
\begin{cases}
    h(B)=x+2\\
    h(C)=x+1\\
    h(A)=x
\end{cases}
$$

Lúc này trước tiên thực hiện một thao tác xoay trái tại nút B, sau đó thực hiện một thao tác xoay phải tại nút D, như hình dưới đây.

![](./images/avl3.svg)

Rõ ràng chiều cao các nút A, E không thay đổi, và con bên phải mới của B cùng con bên trái mới của D lần lượt là con trái và con phải ban đầu của C, ta có:

$$
\begin{cases}
    x-1\leq h'(rs_B),h'(ls_D)\leq x\\
    0\leq h(A)-h'(rs_B)\leq 1\\
    0\leq h(E)-h'(ls_D)\leq 1\\
    h'(B)=\max(h(A),h'(rs_B))+1=x+1\\
    h'(D)=\max(h(E),h'(ls_D))+1=x+1\\
    h'(B)-h'(D)=0
\end{cases}
$$

Vì vậy sau khi xoay, các nút B, C, D cũng thỏa mãn tính chất 2.

???+ note "Thao tác duy trì cân bằng: Mã giả"
    $$
    \begin{array}{ll}
    1 &  \textbf{function } \mathrm{MaintainBalance}(p) \\
    2 &  \qquad l \gets ls_p, r \gets rs_p \\
    3 &  \qquad \textbf{if } h(l)-h(r)=2 \\
    4 &  \qquad\qquad \textbf{if } h(ls_l) \ge h(rs_l) \\
    5 &  \qquad\qquad\qquad \mathrm{RightRotate}(p) \\
    6 &  \qquad\qquad \textbf{else} \\
    7 &  \qquad\qquad\qquad \mathrm{LeftRotate}(l) \\
    8 &  \qquad\qquad\qquad \mathrm{RightRotate}(p) \\
    9 &  \qquad \textbf{else if } h(l)-h(r)=-2 \\
    10 &  \qquad\qquad \textbf{if } h(ls_r) \le h(rs_r) \\
    11 &  \qquad\qquad\qquad \mathrm{LeftRotate}(p) \\
    12 &  \qquad\qquad \textbf{else} \\
    13 &  \qquad\qquad\qquad \mathrm{RightRotate}(r) \\
    14 &  \qquad\qquad\qquad \mathrm{LeftRotate}(p) \\
    \end{array}
    $$

Giống như các cây tìm kiếm nhị phân cân bằng khác, các thông tin như chiều cao nút, kích thước cây con của cây AVL cần được duy trì khi xoay.

## Các thao tác khác

Các thao tác khác của cây AVL (Predecessor, Successor, Select, Rank, v.v.) giống như cây tìm kiếm nhị phân thông thường.

## Mã tham khảo

Mã dưới đây là một `Map` được cài đặt bằng cây AVL (ánh xạ có thứ tự không lặp lại):

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/ds/code/avl-tree/AvlTreeMap.hpp"
    ```

## Tài liệu khác

Tại [AVL Tree Visualization](https://www.cs.usfca.edu/~galles/visualization/AVLtree.html) có thể quan sát quá trình duy trì sự cân bằng của cây AVL.

[Wikipedia -- AVL Tree](https://en.wikipedia.org/wiki/AVL_tree)