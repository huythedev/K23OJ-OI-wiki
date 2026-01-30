author: Backl1ght

Thuật toán AHU dùng để kiểm tra hai cây có gốc có đồng cấu (isomorphic) hay không.

Ngoài AHU, một cách phổ biến khác để kiểm tra đồng cấu cây là [Tree Hash](tree-hash.md).

Kiến thức nền tảng: [Cơ bản về cây](tree-basic.md), [Trọng tâm của cây](tree-centroid.md)

Nên tham khảo ví dụ trong phần tài liệu tham khảo để hiểu rõ hơn.

## Định nghĩa đồng cấu cây

### Cây có gốc đồng cấu

Với hai cây có gốc $T_1(V_1,E_1,r_1)$ và $T_2(V_2,E_2,r_2)$, nếu tồn tại một song ánh $\varphi: V_1 \rightarrow V_2$ sao cho

$$
\forall u,v \in V_1,(u,v) \in E_1 \iff (\varphi(u),\varphi(v))  \in E_2
$$

**và** $\varphi(r_1)=r_2$ thì gọi hai cây có gốc $T_1(V_1,E_1,r_1)$ và $T_2(V_2,E_2,r_2)$ là đồng cấu.

### Cây vô gốc đồng cấu

Với hai cây vô gốc $T_1(V_1,E_1)$ và $T_2(V_2,E_2)$, nếu tồn tại một song ánh $\varphi: V_1 \rightarrow V_2$ sao cho

$$
\forall u,v \in V_1,(u,v) \in E_1 \iff (\varphi(u),\varphi(v))  \in E_2
$$

thì gọi hai cây vô gốc $T_1(V_1,E_1)$ và $T_2(V_2,E_2)$ là đồng cấu.

Nói đơn giản, nếu có thể đánh lại số các đỉnh của $T_1$ để $T_1$ và $T_2$ **hoàn toàn giống nhau**, thì hai cây đó đồng cấu.

## Chuyển hóa bài toán

Bài toán đồng cấu cây vô gốc có thể chuyển thành bài toán đồng cấu cây có gốc như sau:

Với hai cây vô gốc $T_1(V_1, E_1)$ và $T_2(V_2,E_2)$, trước tiên tìm **tất cả** các trọng tâm của mỗi cây.

-   Nếu số lượng trọng tâm khác nhau, hai cây chắc chắn không đồng cấu.
-   Nếu mỗi cây có đúng 1 trọng tâm, gọi là $c_1$ và $c_2$, thì kiểm tra hai cây có gốc $T_1(V_1,E_1,c_1)$ và $T_2(V_2,E_2,c_2)$ có đồng cấu không. Nếu có thì hai cây vô gốc đồng cấu, ngược lại thì không.
-   Nếu mỗi cây có 2 trọng tâm, gọi là $c_1,c'_1$ và $c_2,c'_2$, thì chỉ cần một trong hai cặp $(c_1, c_2)$ hoặc $(c'_1, c_2)$ cho ra hai cây có gốc đồng cấu là đủ.

Như vậy, chỉ cần giải quyết bài toán đồng cấu cây có gốc, ta có thể giải quyết bài toán đồng cấu cây vô gốc theo cách trên.

Giả sử có thuật toán $O(|V|)$ kiểm tra đồng cấu cây có gốc, thì cũng kiểm tra được đồng cấu cây vô gốc trong $O(|V|)$.

## Thuật toán AHU cơ bản

Thuật toán AHU cơ bản dựa trên chuỗi ngoặc (bracket sequence).

### Nguyên lý 1

Một chuỗi ngoặc hợp lệ và một cây có gốc là tương ứng một-một. Chuỗi ngoặc của một cây được tạo bằng cách nối chuỗi ngoặc của các cây con. Nếu ta thay đổi thứ tự nối chuỗi ngoặc các cây con, ta được chuỗi ngoặc mới, nhưng cây tương ứng vẫn đồng cấu với cây ban đầu.

### Nguyên lý 2

Quan hệ đồng cấu của cây là quan hệ bắc cầu. Nếu $T_1$ đồng cấu $T_2$, $T_2$ đồng cấu $T_3$, thì $T_1$ đồng cấu $T_3$.

### Hệ quả

Xét thuật toán đệ quy sinh chuỗi ngoặc cho cây, khi quay lui, ta nối chuỗi ngoặc các cây con. Nếu luôn nối theo thứ tự từ nhỏ đến lớn (theo từ điển), kết quả cuối cùng gọi là $NAME$.

Gọi $NAME(r)$ là chuỗi ngoặc của cây gốc $r$. Nếu $NAME(r_1)=NAME(r_2)$ thì hai cây có gốc đồng cấu.

### Thuật toán đặt tên

???+ note "Cài đặt"
    $$
    \begin{array}{ll}
    1 & \textbf{Input. } \text{Một cây có gốc }T\\
    2 & \textbf{Output. } \text{Chuỗi tên của cây có gốc }T\\
    3 & \text{ASSIGN-NAME(u)}\\
    4 & \qquad \text{nếu  } u \text{  là lá}\\
    5 & \qquad \qquad \text{NAME(} u \text{) = (0)}\\
    6 & \qquad \text{ngược lại }\\
    7 & \qquad \qquad \text{với mọi con } v \text{ của } u\\
    8 & \qquad \qquad \qquad \text{ASSIGN-NAME(}v\text{)}\\
    9 & \qquad \text{sắp xếp tên các con của }u\\
    10 & \qquad \text{nối tên các con của }u\text{ vào temp}\\
    11 & \qquad \text{NAME(} u \text{) = (temp)}
    \end{array}
    $$

### Thuật toán AHU

???+ note "Cài đặt"
    $$
    \begin{array}{ll}
    1 & \textbf{Input. } \text{Hai cây có gốc }T_1(V_1,E_1,r_1)\text{ và }T_2(V_2,E_2,r_2) \\
    2 & \textbf{Output. } \text{Hai cây này có đồng cấu không}\\
    3 & \text{AHU}(T_1(V_1,E_1,r_1), T_2(V_2,E_2,r_2))\\
    4 & \qquad \text{ASSIGN-NAME(}r_1\text{)}\\
    5 & \qquad \text{ASSIGN-NAME(}r_2\text{)}\\
    6 & \qquad \text{nếu  NAME}(r_1) = \text{NAME}(r_2)\\
    7 & \qquad \qquad \text{trả về true}\\
    8 & \qquad \text{ngược lại}\\
    10 & \qquad \qquad \text{trả về false}
    \end{array}
    $$

### Phân tích độ phức tạp

Với cây có $n$ đỉnh, nếu là cây dạng chuỗi thì độ dài tên tối đa là $n$, nên thuật toán ASSIGN-NAME có độ phức tạp $1+2+\cdots+n = \Theta(n^2)$. Do đó, AHU cơ bản có độ phức tạp $O(n^2)$.

## Thuật toán AHU tối ưu

Nhược điểm của AHU cơ bản là độ dài $NAME$ có thể rất lớn. Ta có thể tối ưu như sau:

### Nguyên lý 1

Chia cây thành các lớp, lớp $i$ gồm các đỉnh cách gốc đúng $i$. Tên của đỉnh lớp $i$ chỉ phụ thuộc vào tên các đỉnh lớp $i+1$.

### Nguyên lý 2

Trong cùng một lớp, tên của mỗi đỉnh có thể được mã hóa bằng thứ hạng trong lớp đó.

**Lưu ý**: Thứ hạng này tính trên cả hai cây, tức là với đỉnh $u$ ở lớp $i$, thứ hạng là số đỉnh trong cả $T_1$ và $T_2$ ở lớp $i$ có $NAME$ nhỏ hơn $NAME(u)$.

### Hệ quả

Ta có thể thay $NAME$ của mỗi đỉnh bằng thứ hạng trong lớp, và thay việc nối chuỗi bằng việc thêm số vào mảng.

Như vậy, dùng số nguyên và mảng thay cho chuỗi, không ảnh hưởng tính đúng đắn mà giảm mạnh độ phức tạp.

### Phân tích độ phức tạp

Tổng độ dài các $NAME$ ở lớp $i$ là tổng bậc các đỉnh lớp $i$, tức là số đỉnh lớp $i+1$, ký hiệu $L_i$. Ở mỗi lớp, ta cần sắp xếp các chuỗi (mảng) độ dài tổng $L_i$.

1.  Có thể dùng Radix sort để sắp xếp trong $O(L+|\Sigma|)$, với $|\Sigma|$ là kích thước bảng chữ cái (ở đây là số thứ hạng, tối đa $L_i$).
2.  Hoặc dùng quicksort, độ phức tạp $O(L \log m)$ với $m$ là số chuỗi.

Trong AHU, $|\Sigma| \leq L_i$, nên radix sort là tuyến tính. Tổng $\sum_i L_i = O(n)$, nên tổng độ phức tạp là $O(n)$ nếu dùng radix sort, hoặc $O(n \log n)$ nếu dùng quicksort.

## Bài tập ví dụ

[SPOJ-TREEISO](https://www.spoj.com/problems/TREEISO/en/)

Đề bài: Cho hai cây vô gốc, kiểm tra chúng có đồng cấu không.

???+ note "Code mẫu"
    ```cpp
    --8<-- "docs/graph/code/tree-ahu/tree-ahu_1.cpp"
    ```

## Tài liệu tham khảo

Phần lớn nội dung dịch từ [Paper](http://wwwmayr.in.tum.de/konferenzen/Jass08/courses/1/smal/Smal_Paper.pdf) và [Slide](https://logic.pdmi.ras.ru/~smal/files/smal_jass08_slides.pdf). Các chứng minh trong tài liệu tham khảo đầy đủ và chặt chẽ hơn, bài viết này đã lược giản.

Phân tích độ phức tạp của AHU và radix sort chuỗi tuyến tính tham khảo The Design and Analysis of Computer Algorithms, mục 3.2 Radix sorting, và Example 3.2.
