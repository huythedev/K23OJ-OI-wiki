author: Tiphereth-A, ShaoChenHeng, Enter-tainer, ksyx, c-forrest, StudyingFather, H-J-Granger, iamtwz, imp2002, Ir1d, kenlig, LeBronGod, Marcythm, MegaOwIer, NachtgeistW, ouuan, Patchouliys, Soohti, TianKong-y, sun2snow

## Mở đầu

DP xác suất dùng để giải quyết các bài toán về xác suất và kỳ vọng, nên xem trước [Xác suất & Kỳ vọng](../math/probability/exp-var.md). Thông thường, giải bài toán xác suất cần duyệt thuận, còn bài toán kỳ vọng dùng duyệt ngược. Nếu phương trình chuyển trạng thái có hậu hiệu, cần dùng [Khử Gauss](../math/numerical/gauss.md) để tối ưu. DP xác suất cũng thường kết hợp với các kiến thức khác như [trạng thái nén](./state.md), DP trên cây, v.v.

## DP xác suất

Dạng này dùng duyệt thuận, tức là từ trạng thái đầu đến kết quả. Tương tự DP thông thường, khó nhất vẫn là xây dựng phương trình chuyển trạng thái, chỉ là bài toán được "bọc" bởi xác suất.

### Bài mẫu

???+ example "[Codeforces 148D Bag of mice](https://codeforces.com/problemset/problem/148/D)"
    Có $w$ chuột trắng và $b$ chuột đen trong túi, công chúa và rồng lần lượt bắt chuột. Ai bắt được chuột trắng trước thì thắng, nếu hết chuột mà chưa ai bắt được chuột trắng thì rồng thắng. Công chúa bắt trước. Hỏi xác suất công chúa thắng.

??? note "Giải thích"
    Gọi $f_{i,j}$ là xác suất công chúa thắng khi còn $i$ chuột trắng, $j$ chuột đen và đến lượt công chúa. Biên: $f_{0,j}=0$ (hết chuột trắng, rồng thắng), $f_{i,0}=1$ (chỉ còn chuột trắng, công chúa thắng).
    Xét chuyển trạng thái:
    
    -   Công chúa bắt được chuột trắng, thắng ngay, xác suất $\dfrac{i}{i+j}$.
    -   Công chúa bắt chuột đen, rồng bắt chuột trắng, rồng thắng, xác suất $\dfrac{j}{i+j}\cdot\dfrac{i}{i+j-1}$.
    -   Công chúa bắt chuột đen, rồng bắt chuột đen, chuột đen chạy ra, chuyển về $f_{i,j-3}$, xác suất $\dfrac{j}{i+j}\cdot\dfrac{j-1}{i+j-1}\cdot\dfrac{j-2}{i+j-2}$.
    -   Công chúa bắt chuột đen, rồng bắt chuột đen, chuột trắng chạy ra, chuyển về $f_{i-1,j-2}$, xác suất $\dfrac{j}{i+j}\cdot\dfrac{j-1}{i+j-1}\cdot\dfrac{i}{i+j-2}$.
    
    Chỉ tính xác suất công chúa thắng, trường hợp 2 không cộng vào. Cần kiểm tra số lượng chuột đủ cho các trường hợp.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/probability/probability_1.cpp"
    ```

### Bài tập

-   [POJ3071 Football](http://poj.org/problem?id=3071)
-   [CodeForces 768D Jon and Orbs](https://codeforces.com/problemset/problem/768/D)

## DP kỳ vọng

### Bài mẫu

???+ example "[POJ2096 Collecting Bugs](http://poj.org/problem?id=2096)"
    Một phần mềm có $s$ module, có $n$ loại bug. Mỗi ngày phát hiện một bug, bug này thuộc một loại và một module nào đó. Xác suất bug thuộc module $1/s$, thuộc loại $1/n$. Hỏi kỳ vọng số ngày để phát hiện đủ $n$ loại bug và tất cả $s$ module đều có bug.

??? note "Giải thích"
    Gọi $f_{i,j}$ là kỳ vọng số ngày để đạt trạng thái đã có $i$ loại bug và $j$ module đã có bug. Trạng thái đích là $f_{n,s}=0$. Bắt đầu truy hồi từ trạng thái đích, đáp án là $f_{0,0}$.
    
    Xét chuyển trạng thái:
    
    -   $f_{i,j}$, phát hiện bug đã có, xác suất $p_1=\dfrac{i}{n}\cdot\dfrac{j}{s}$.
    -   $f_{i,j+1}$, bug loại đã có, module mới, xác suất $p_2=\dfrac{i}{n}\cdot(1-\dfrac{j}{s})$.
    -   $f_{i+1,j}$, bug loại mới, module đã có, xác suất $p_3=(1-\dfrac{i}{n})\cdot\dfrac{j}{s}$.
    -   $f_{i+1,j+1}$, bug loại mới, module mới, xác suất $p_4=(1-\dfrac{i}{n})\cdot(1-\dfrac{j}{s})$.
    
    Theo tính chất tuyến tính của kỳ vọng, ta có:
    
    $$
    \begin{aligned}
    f_{i,j} &= p_1\cdot f_{i,j}+p_2\cdot f_{i,j+1}+p_3\cdot f_{i+1,j}+p_4\cdot f_{i+1,j+1} + 1\\
    &= \dfrac{p_2\cdot f_{i,j+1}+p_3\cdot f_{i+1,j}+p_4\cdot f_{i+1,j+1}+1}{1-p_1}
    \end{aligned}
    $$

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/probability/probability_2.cpp"
    ```

???+ example "[「NOIP2016」Đổi phòng học](http://uoj.ac/problem/262)"
    NiuNiu học $n$ tiết, tiết $i$ ở phòng $c_i$, có thể xin đổi sang $d_i$, xác suất thành công $p_i$, tối đa xin đổi $m$ tiết. Sau mỗi tiết phải di chuyển sang phòng tiết tiếp theo, cho đồ thị $v$ phòng $e$ đường, di chuyển tốn sức. Hỏi nên xin đổi tiết nào để tổng kỳ vọng sức di chuyển là nhỏ nhất.

??? note "Giải thích"
    Dùng Floyd tìm đường đi ngắn nhất giữa các phòng. Định nghĩa $f_{i,j,0/1}$ là kỳ vọng sức di chuyển nhỏ nhất khi ở tiết $i$, đã dùng $j$ lần đổi, trạng thái hiện tại là không đổi (0) hoặc đổi (1). Đáp án là $\min \{f_{n,i,0},f_{n,i,1}\} ,i\in[0,m]$. Biên: $f_{1,0,0}=f_{1,1,1}=0$.
    
    Xét chuyển trạng thái:
    
    -   Nếu không đổi ở tiết này: $f_{i,j,0}=\min(f_{i-1,j,0}+w_{c_{i-1},c_{i}},f_{i-1,j,1}+w_{d_{i-1},c_{i}}\cdot p_{i-1}+w_{c_{i-1},c_{i}}\cdot (1-p_{i-1}))$
    -   Nếu đổi ở tiết này: tương tự, xét các khả năng chuyển tiếp từ trạng thái trước.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/probability/probability_3.cpp"
    ```

So sánh hai bài toán trên có thể thấy, DP xác suất và DP kỳ vọng đều cần kiến thức xác suất và lập phương trình chuyển trạng thái, chỉ khác nhau ở mục tiêu tối ưu hóa hay tính giá trị.

### Bài tập

-   [HDU3853 LOOPS](https://acm.hdu.edu.cn/showproblem.php?pid=3853)
-   [HDU4035 Maze](https://acm.hdu.edu.cn/showproblem.php?pid=4035)
-   [「SCOI2008」Màn thưởng](https://www.luogu.com.cn/problem/P2473)

## DP có hậu hiệu

### Bài mẫu

???+ example "[CodeForces 24D Broken robot](https://codeforces.com/problemset/problem/24/D)"
    Cho một ma trận $n \times m$. Robot bắt đầu ở dòng $x$, cột $y$, mỗi bước chọn ngẫu nhiên dừng lại, đi trái, đi phải, đi xuống. Nếu ở biên thì không ra ngoài. Hỏi kỳ vọng số bước để đến dòng cuối cùng.

??? note "Giải thích"
    Với $m=1$, mỗi bước xác suất $1/2$ không di chuyển, $1/2$ xuống dưới, đáp án $2\cdot (n-x)$. Gọi $f_{i,j}$ là kỳ vọng số bước từ $(i,j)$ đến dòng $n$. Trạng thái cuối $f_{n,j}=0$.
    Xét chuyển trạng thái:
    
    -   $f_{i,1}=\dfrac{1}{3}\cdot(f_{i+1,1}+f_{i,2}+f_{i,1})+1$
    -   $f_{i,j}=\dfrac{1}{4}\cdot(f_{i,j}+f_{i,j-1}+f_{i,j+1}+f_{i+1,j})+1$
    -   $f_{i,m}=\dfrac{1}{3}\cdot(f_{i,m}+f_{i,m-1}+f_{i+1,m})+1$
    
    Chuyển đổi phương trình:
    
    -   $2f_{i,1}-f_{i,2}=3+f_{i+1,1}$
    -   $3f_{i,j}-f_{i,j-1}-f_{i,j+1}=4+f_{i+1,j}$
    -   $2f_{i,m}-f_{i,m-1}=3+f_{i+1,m}$
    
    Dùng khử Gauss để giải hệ phương trình.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/probability/probability_4.cpp"
    ```

### Bài tập

-   [HDU 4418 Du hành thời gian](https://acm.hdu.edu.cn/showproblem.php?pid=4418)
-   [「HNOI2013」Dạo chơi](https://loj.ac/problem/2383)

## Tài liệu tham khảo

[kuangbin Tổng kết DP xác suất](https://www.cnblogs.com/kuangbin/archive/2012/10/02/2710606.html)
