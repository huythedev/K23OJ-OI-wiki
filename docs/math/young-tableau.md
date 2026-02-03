## Giới thiệu

**Bảng Young** (Young tableau), còn gọi là Young tableau, là một đối tượng tổ hợp thường dùng trong lý thuyết biểu diễn và giải tích Schubert.

Bảng Young là một dạng ma trận đặc biệt. Nó thuận tiện cho việc nghiên cứu biểu diễn của nhóm đối xứng và nhóm tuyến tính tổng quát. Bảng Young do nhà toán học Đại học Cambridge Alfred Young đề xuất năm 1900, và được nhà toán học Đức Ferdinand Georg Frobenius ứng dụng vào nghiên cứu nhóm đối xứng năm 1903.

???+ note "Chú thích"
    **Lý thuyết biểu diễn** (Representation theory) là một nhánh của toán học, nghiên cứu các cấu trúc đại số trừu tượng thông qua các biến đổi tuyến tính trên không gian vector. **Giải tích Schubert** (Schubert calculus) là một nhánh của hình học đại số, được Hermann Schubert đưa ra vào thế kỷ 19 để giải các bài toán đếm trong hình học xạ ảnh.

## Định nghĩa

### Đồ thị Young

**Đồ thị Young** (Young diagram, khi dùng điểm biểu diễn còn gọi là [Ferrers diagram](https://en.wikipedia.org/wiki/Partition_%28number_theory%29#Ferrers_diagram), đã giới thiệu ở mục [phân hoạch số](./combinatorics/partition.md#ferrers-%E5%9B%BE)) là một tập hữu hạn các ô (cells) hoặc khung, căn trái, độ dài hàng không tăng. Liệt kê số ô mỗi hàng, ta được một số nguyên không âm $n$ (tổng số ô) và một **phân hoạch số nguyên** (integer partition) $\lambda$. Vì vậy, hình dạng của đồ thị Young có thể xem là $\lambda$, vì mang cùng thông tin như phân hoạch.

Quan hệ bao hàm giữa các đồ thị Young tạo nên một [thứ tự bộ phận](../math/order-theory.md#偏序集) trên các phân hoạch, có cấu trúc [lattice](../math/order-theory.md#有向集与格), gọi là **Young's lattice**. Nếu liệt kê số ô theo cột, ta được phân hoạch liên hợp (conjugate/transpose) của $\lambda$, tương ứng với đồ thị Young thu được bằng phép đối xứng qua đường chéo chính.

Vị trí mỗi ô được xác định bởi tọa độ hàng và cột. Cột từ trái qua phải, hàng theo thứ tự số ô từ nhiều đến ít. Có hai cách vẽ: cách Anh (English) đặt hàng ngắn hơn ở dưới, cách Pháp (French) xếp hàng từ lớn lên nhỏ theo chiều lên trên. Theo thói quen, gọi lần lượt là cách Anh và cách Pháp.

Bảng sau là đồ thị Young của phân hoạch $(5,4,1)$ theo hai cách vẽ:

-   Cách Anh: ![](./images/young-diagram-1.svg)
-   Cách Pháp: ![](./images/young-diagram-2.svg)

### Bảng Young

#### Định nghĩa

**Bảng Young** (Young tableau) thu được bằng cách điền các ký hiệu từ một bảng chữ cái vào các ô của đồ thị Young, thường yêu cầu là một tập có thứ tự toàn phần. Các phần tử được viết là $x_{1}$,$x_{2}$,$x_{3}$,$\ldots$; để tiện, thường dùng số nguyên dương.

Ban đầu, trong lý thuyết biểu diễn của nhóm đối xứng, cho phép điền tùy ý các số khác nhau từ $1$ đến $n$ vào $n$ ô. Hiện nay thường xét **bảng Young chuẩn** (standard), trong đó các số trong mỗi hàng và mỗi cột đều tăng строго. Số bảng Young chuẩn với $n$ ô tạo thành [dãy đối hợp](https://en.wikipedia.org/wiki/Telephone_number_%28mathematics%29):

???+ note "Chú thích"
    **Số đối hợp** (involution number/telephone number) là một dãy số nguyên dùng để đếm số cách ghép đôi các đường dây điện thoại, số ghép cặp trong đồ thị đầy đủ, số hoán vị là đối hợp, tổng trị tuyệt đối hệ số của đa thức Hermite, số bảng Young chuẩn với $n$ ô, và tổng bậc của các biểu diễn bất khả quy của nhóm đối xứng.

$1, 1, 2, 4, 10, 26, 76, 232, 764, 2620, 9496, \ldots$ (dãy [A000085](https://oeis.org/A000085) trên [OEIS](https://en.wikipedia.org/wiki/On-Line_Encyclopedia_of_Integer_Sequences))

Trong các ứng dụng khác, đồ thị Young có thể được điền các số giống nhau. Nếu các số trong cùng cột tăng строго, còn trong cùng hàng không giảm, thì bảng gọi là **nửa chuẩn** (Semistandard Young Tableaux, đôi khi gọi là cột nghiêm). Ghi lại số lần xuất hiện của mỗi số được coi là **trọng số** của bảng. Do đó, bảng chuẩn có trọng số $(1,1,\ldots,1)$ vì mỗi số từ $1$ đến $n$ xuất hiện đúng một lần.

#### Thuật toán chèn cho bảng Young chuẩn

Tính chất của hoán vị có thể được biểu diễn trực quan bởi bảng Young. **Thuật toán chèn RSK** liên hệ bảng Young và hoán vị, do Robinson, Schensted, Knuth đề xuất.

Cho $S$ là một bảng Young, ký hiệu $S \leftarrow x$ là chèn $x$ vào hàng đầu tiên, như sau:

1.  Trong hàng hiện tại, tìm số nhỏ nhất lớn hơn $x$, ký hiệu $y$.
2.  Nếu tìm thấy, thay $y$ bằng $x$, chuyển xuống hàng tiếp theo, đặt $x \leftarrow y$ và lặp bước 1.
3.  Nếu không tìm thấy, đặt $x$ vào cuối hàng và dừng. Gọi $x$ nằm ở hàng $s$, cột $t$, thì $(s, t)$ là một góc. Một ô $(s, t)$ là góc khi và chỉ khi $(s + 1, t)$ và $(s, t + 1)$ đều không tồn tại.

Ví dụ, chèn $3$ vào bảng $(2, 5, 9)(6, 7)(8)$:

![](./images/young-tableau-insert.svg)

### Biến thể

Các bảng Young không hoàn toàn chuẩn có nhiều biến thể. Ví dụ, bảng nghiêm theo hàng yêu cầu hàng tăng строго, còn cột không giảm, tức là liên hợp của bảng nghiêm theo cột. Trong lý thuyết phân hoạch phẳng (plane partitions), thường thay điều kiện tăng bằng giảm. Các biến thể khác như bảng dạng dải (ribbon), gom các ô thành nhóm và yêu cầu các ô trong cùng nhóm có cùng số.

### Bảng Young lệch

Cho hai đồ thị Young $\lambda = (\lambda{1}, \lambda{2} \ldots)$ và $\mu = (\mu{1}, \mu{2},\ldots)$, với $\lambda$ chứa $\mu$, tức $\mu{i} \leq \mu{i}$ với mọi $i$. Định nghĩa **đồ thị Young lệch** $\lambda/\mu$ là phần các ô trong $\lambda$ trừ đi các ô trong $\mu$. Điền các phần tử vào các ô này được **bảng Young lệch** (Skew tableaux).

Ví dụ, hình dưới là một bảng Young lệch chuẩn tương ứng với phân hoạch $(5,4,1)$:

![](./images/skew-tableau.svg)

Tương tự, nếu số trong cùng cột tăng строго và trong cùng hàng không giảm, thì gọi là **bảng Young lệch nửa chuẩn**; nếu bảng nửa chuẩn lệch có các số $1$ đến $n$ không trùng lặp, thì là **bảng Young lệch chuẩn**. Lưu ý các cặp $\lambda$ và $\mu$ khác nhau có thể cho cùng $\lambda/\mu$. Dù nhiều tính chất chỉ phụ thuộc vào phần lệch, một số phép toán vẫn phụ thuộc vào $\lambda$ và $\mu$, nên $\lambda/\mu$ phải được coi là chứa cả hai thông tin. Nếu $\mu$ là phân hoạch rỗng (phân hoạch duy nhất của $0$), thì $\lambda/\mu$ chính là bảng Young $\lambda$.

## Ứng dụng

Bảng Young thường dùng trong tổ hợp, lý thuyết biểu diễn và hình học đại số, để định nghĩa hàm Schur và các đẳng thức liên quan. Trong lập trình thi đấu, thường gặp công thức độ dài móc.

### Độ dài móc

Cho bảng Young $\pi_{\lambda}$ có $n$ ô, điền các số $1$ đến $n$ sao cho mỗi hàng tăng từ trái sang phải, mỗi cột tăng từ dưới lên trên. Số cách điền gọi là $dim_{\pi_{\lambda}}$.

Với một ô $v$, **độ dài móc** $\mathrm{hook}(v)$ bằng số ô bên phải cùng hàng cộng số ô phía trên cùng cột, cộng 1 (chính nó).

### Công thức độ dài móc

Nếu dùng $dim_{\lambda}$ là số cách điền, **công thức độ dài móc** cho biết:

$$
\dim \pi _{\lambda}={\frac {n!}{\prod_{{x\in Y(\lambda)}}{\mathrm {hook}}(x)}}.
$$

![](./images/young-tableau-2.svg)

Với phân hoạch $10 = 5 + 4 + 1$ như hình, ta có:

$$
\dim \pi _{\lambda }={\frac  {10!}{7\cdot 5\cdot 4\cdot 3\cdot 1\cdot 5\cdot 3\cdot 2\cdot 1\cdot 1}}=288.
$$

## Bài tập

### Bài toán dãy con

Với bảng Young $P$, xét hoán vị $X = x_{1}, \ldots , x_{n}$:

1.  Độ dài hàng đầu của $P_X$ bằng độ dài **dãy con tăng dài nhất (LIS)** của $X$. Lưu ý hàng đầu không nhất thiết là LIS nên không thể dùng trực tiếp cho bài toán “phân hoạch LIS”.

2.  Với hoán vị $X$ và bảng $P_X$, nếu $X^R$ là dãy đảo, thì bảng $P_{X^R}$ là bảng $P_X$ sau khi đổi hàng và cột.

    Ví dụ, với $X = 1, 5, 7, 2, 8, 6, 3, 4$ và $X^R = 4, 3, 6, 8, 2, 7, 5, 1$, ta có:

    ![](./images/young-tableau-LIS.svg)

3.  Độ dài cột đầu của $P_X$ bằng độ dài **dãy con giảm dài nhất (LDS)** của $X$.

Định nghĩa độ dài không vượt quá $k$ của LIS/LDS là $k$-LIS và $k$-LDS. Với $1$-LIS thì rõ ràng chính là LDS, tương ứng cột đầu. Tương tự, tổng độ dài $k$ cột đầu là độ dài $k$-LIS dài nhất. Chứng minh:

Với hoán vị $X$ và bảng $P$ có $m$ hàng, đặt $X^∗ = (P_{m,1}\ldots,P_{m,λ_{m}},P_{m−1,1}\ldots,P_{1,1}\ldots P_{1,λ_{1}})$ (ghi các hàng từ dưới lên). Khi đó $X$ có thể được chuyển thành $X^*$ bằng hoán đổi.

Do đó, độ dài $k$-LIS dài nhất là $F(k)=\sum_{i=1}^{m} \min(k,\lambda_{i})$, chính là tổng độ dài $k$ cột đầu.

???+ note "[CTSC2017 Dãy con tăng dài nhất](https://uoj.ac/problem/301)"
    Có dãy $b$ độ dài $n$. Với $B_{m} = (b_{1}, b_{2},\ldots, b_{m})$, gọi $C$ là một dãy con của $B_m$, và độ dài LIS của $C$ không vượt quá $k$. Hỏi giá trị lớn nhất của $|C|$.

??? note "Hướng giải"
    Nhiều truy vấn có thể dùng sweep line. Ta cần duy trì bảng Young cho mỗi tiền tố. Dùng kết luận trên, bài toán thành duy trì tổng độ dài $k$ cột đầu. Nếu trực tiếp sẽ $O(n^2 \log n)$ không đủ. Hãy duy trì trước $\sqrt{n}$ cột và $\sqrt{n}$ hàng.
    
    Ta thấy bảng Young không thể phủ kín hoàn toàn hình chữ nhật $W \times H$. Nếu $K \leq W$, trả lời trực tiếp; nếu $K > W$, phần vượt $W$ chỉ nằm trong $H$ hàng. Do đó có thể duy trì đồng thời trước $\sqrt{n}$ cột và $\sqrt{n}$ hàng. Đảo dãy sẽ cho bảng Young chuyển vị, nên chỉ cần thêm duy trì $-A_i$, độ phức tạp $O(n \sqrt{n} \log n)$.

???+ note "[BJWC2018 Dãy con tăng dài nhất](https://www.luogu.com.cn/problem/P4484)"
    Cho một hoán vị ngẫu nhiên độ dài $n$, tính kỳ vọng độ dài LIS.

???+ note "[CF1268B Domino Young](https://codeforces.com/problemset/problem/1268/B)"
    Cho biểu đồ cột có $n$ cột chiều dài $a_{1} ,a_{2},\ldots,a_{n}\,(a_{1} \geq a_{2} \geq \ldots \geq a_{n} \geq 1)$, ví dụ $a=[3,2,2,2,1]$. Tìm số domino không chồng lấn tối đa ($1 \times 2$ hoặc $2 \times 1$) có thể đặt.

## Tài liệu tham khảo và đọc thêm

1.  [Young Tableau - from Wolfram MathWorld](https://mathworld.wolfram.com/YoungTableau.html)
2.  [Young tableau - Wikipedia](https://en.wikipedia.org/wiki/Young_tableau)
3.  [Hook length formula - Wikipedia](https://en.wikipedia.org/wiki/Hook_length_formula)
4.  袁方舟，[《浅谈杨氏矩阵在信息学竞赛中的应用》IOI2019](https://github.com/OI-wiki/libs/blob/master/%E9%9B%86%E8%AE%AD%E9%98%9F%E5%8E%86%E5%B9%B4%E8%AE%BA%E6%96%87/%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F2019%E8%AE%BA%E6%96%87%E9%9B%86.pdf), 中国国家候选队论文集，202-229
