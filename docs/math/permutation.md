author: 2008verser, aofall, CoelacanthusHex, Early0v0, Great-designer, Marcythm, Persdre, shuzhouliu, Tiphereth-A, Enter-tainer, gavinliu266, gi-b716, hjsjhn, Ir1d, MegaOwIer, wjy-yy, c-forrest

Hoán vị và sắp xếp là những khái niệm rất thường gặp trong nhiều bài toán.

???+ warning "Bài này không nói về số hoán vị"
    Chủ đề ở đây là hoán vị toàn phần, không phải số chỉnh hợp trong tổ hợp. Về chỉnh hợp, xem [tổ hợp và chỉnh hợp](./combinatorics/combination.md).

???+ info "Quy ước"
    Nếu không nói rõ, ta luôn xét các tập hữu hạn.

## Định nghĩa

Một song ánh từ tập $X$ tới chính nó $\sigma$ được gọi là một **hoán vị** (permutation) của $X$. Nếu $X$ còn có [thứ tự toàn phần](./order-theory.md#二元关系) thì một hoán vị cũng gọi là một **(toàn) sắp xếp** (arrangement). Thứ tự toàn phần này gọi là thứ tự tự nhiên của tập.

??? info "“Hoán vị” và “sắp xếp”"
    Trong tiếng Việt, “hoán vị” thường nhấn mạnh việc thay đổi thứ tự, “sắp xếp” thường là dãy kết quả. Khi các phần tử có thứ tự tự nhiên, hai khái niệm này trùng nhau: “sắp xếp” là kết quả của “hoán vị”; “hoán vị” chỉ định thứ tự trong “sắp xếp” so với thứ tự tự nhiên. Vì bài viết dùng “sắp xếp” khi giả định có thứ tự tự nhiên, nên không phân biệt sâu hai khái niệm.
    
    Dù vậy, các phần tử không có thứ tự tự nhiên vẫn có thể “sắp xếp” trong các bài toán đếm tổ hợp, nhưng nằm ngoài phạm vi bài này.

Nếu $|X| = n$ thì số hoán vị của $X$ là $n!$. Đặc biệt $0!=1$, tức tập rỗng có đúng một hoán vị, là hoán vị rỗng.

???+ info "Ký hiệu"
    Hoán vị chỉ xét quan hệ tương ứng giữa các phần tử, không quan tâm phần tử cụ thể. Vì vậy, khi xét tập kích thước $n$ thường giả định là $\{1,2,\cdots,n\}$; nếu cần thứ tự tự nhiên thì dùng thứ tự tự nhiên trên $\mathbf{N}$.

## Cách biểu diễn

Hoán vị có nhiều cách biểu diễn. Xét ví dụ:

$$
\sigma(1) = 2,\
\sigma(2) = 6,\
\sigma(3) = 5,\
\sigma(4) = 4,\
\sigma(5) = 3,\
\sigma(6) = 1.
$$

### Ký hiệu hai hàng

Hoán vị trên $X=\{x_1,x_2,\cdots,x_n\}$ có thể viết:

$$
\sigma=\begin{pmatrix}x_1&x_2&\cdots&x_n\\
x_{p_1}&x_{p_2}&\cdots&x_{p_n}
\end{pmatrix}.
$$

Nó nghĩa là $\sigma$ ánh xạ $x_i$ thành $x_{p_i}$. Ở đây $X=\{x_{p_1},x_{p_2},\cdots,x_{p_n}\}$. Trong ký hiệu hai hàng, thứ tự của hàng trên không quan trọng, quan trọng là quan hệ tương ứng.

Ví dụ trên có thể viết:

$$
\sigma=
\begin{pmatrix}
1 & 2 & 3 & 4 & 5 & 6 \\
2 & 6 & 5 & 4 & 3 & 1
\end{pmatrix},
$$

hoặc

$$
\sigma=
\begin{pmatrix}
6 & 5 & 4 & 3 & 2 & 1 \\
1 & 3 & 4 & 5 & 6 & 2
\end{pmatrix}.
$$

### Ký hiệu một hàng

Nhiều khi $X$ có thứ tự tự nhiên. Nếu trong ký hiệu hai hàng ta mặc định hàng trên theo thứ tự tự nhiên và bỏ đi hàng trên, ta được:

$$
\sigma=\sigma(1)\sigma(2)\cdots\sigma(n).
$$

Gần với khái niệm dãy sắp xếp. Ví dụ:

$$
\sigma=265431.
$$

Ký hiệu này thường dùng để so sánh thứ tự giữa các hoán vị.

### Biểu diễn bằng chu trình

Hoán vị còn có cách biểu diễn gọn hơn bằng **chu trình**. Dưới đây là cách viết hoán vị thành tích các chu trình rời nhau.

Cho hoán vị $\sigma$, thực hiện:

1.  Nếu còn phần tử chưa viết, viết một dấu ngoặc trái và chọn một phần tử chưa viết.
2.  Nếu phần tử vừa viết là $x$:
    -   Nếu $\sigma(x)$ đã xuất hiện trước đó, viết ngoặc phải và quay lại bước 1;
    -   Nếu $\sigma(x)$ chưa xuất hiện, viết $\sigma(x)$ và tiếp tục bước 2.
3.  Dừng khi mọi phần tử đã viết.

Mỗi cặp ngoặc là một chu trình. Số phần tử trong ngoặc là độ dài chu trình. Thực tế thường bỏ chu trình độ dài 1.

Ví dụ:

$$
\sigma=(126)(35)(4)=(126)(35).
$$

Trong phép đồng nhất, mọi chu trình đều dài 1, thường ký hiệu $(1)$ thay vì bỏ hết.

## Hợp thành

Hợp thành của hoán vị là hợp thành ánh xạ, còn gọi là phép nhân hoán vị.

Cho

$$
\sigma=\begin{pmatrix}x_1&x_2&\cdots&x_n\\ x_{p_1}&x_{p_2}&\cdots&x_{p_n}\end{pmatrix},\ \pi=\begin{pmatrix}x_{p_1}&x_{p_2}&\cdots&x_{p_n}\\ x_{q_1}&x_{q_2}&\cdots&x_{q_n}\end{pmatrix},
$$

thì

$$
\pi\circ\sigma=\begin{pmatrix}x_1&x_2&\cdots&x_n\\
x_{q_1}&x_{q_2}&\cdots&x_{q_n}\end{pmatrix}.
$$

Nói cách khác, trước hết qua $\sigma$, sau đó qua $\pi$. Trong ký hiệu hai hàng, hàng thứ hai của $\sigma$ trùng thứ tự với hàng thứ nhất của $\pi$.

Vì $\sigma,\pi$ là ánh xạ nên $(\pi\circ\sigma)(x)=\pi(\sigma(x))$. Thứ tự nhân là từ phải sang trái; phép nhân không giao hoán.

Tích nhiều hoán vị gọi là lũy thừa hoán vị, có thể dùng [lũy thừa nhanh](./binary-exponentiation.md#多次置换).

### Hoán vị nghịch đảo

Vì hoán vị là song ánh nên luôn có nghịch đảo.

Cho

$$
\sigma=\begin{pmatrix}x_1&x_2&\cdots&x_n\\ x_{p_1}&x_{p_2}&\cdots&x_{p_n}\end{pmatrix},
$$

thì

$$
\sigma^{-1}=\begin{pmatrix}x_{p_1}&x_{p_2}&\cdots&x_{p_n}\\x_1&x_2&\cdots&x_n\end{pmatrix}.
$$

Trong biểu diễn chu trình, đảo mỗi chu trình bằng cách đảo thứ tự phần tử. Ví dụ:

$$
\sigma^{-1} = (621)(53) = (162)(35).
$$

Một hoán vị $1\sim n$ và dãy thứ hạng của từng phần tử là hai hoán vị nghịch đảo nhau.

## Chu trình

**Chu trình** (cycle) là một hoán vị đặc biệt: từ bất kỳ $x$ trong chu trình, lặp $\sigma$ sẽ đi qua mọi phần tử trong chu trình. Chu trình độ dài $k$ gọi là **$k$‑chu trình** ($k$-cycle). Lặp $k$ lần sẽ trở về đồng nhất.

Biểu diễn hoán vị bằng chu trình là **phân tích chu trình**. Mỗi hoán vị có phân tích chu trình duy nhất (bỏ thứ tự các chu trình). Chu trình là đơn vị cơ bản của hoán vị.

Về hình học, nếu xem mỗi cặp $(x,\sigma(x))$ là cạnh có hướng trên đồ thị với đỉnh $S$, thì các chu trình là các vòng trên đồ thị. Nếu $\sigma$ phân tích thành $m$ chu trình thì đồ thị có $m$ vòng (gồm cả tự vòng).

### Điểm bất động

Chu trình độ dài 1 là **điểm bất động** (fixed point). Với hoán vị $\sigma$ trên $X$, ký hiệu $X^\sigma=\{x\in X:\sigma(x)=x\}$.

### Đối hoán

Chu trình độ dài 2 gọi là **đối hoán** (transposition), ký hiệu $(x_ix_j)$, nghĩa là hoán đổi $x_i$ và $x_j$.

Mọi hoán vị đều có thể viết thành tích các đối hoán. Tương đương với việc mọi dãy có thể trở về thứ tự chuẩn bằng các phép đổi chỗ hai phần tử. Đây là bản chất của các thuật toán sắp xếp dựa trên đổi chỗ.

Hơn nữa, [bubble sort](../basic/bubble-sort.md) cho thấy mọi hoán vị có thể viết thành tích các **đối hoán kề** (adjacent transposition), chỉ đổi chỗ hai phần tử kề nhau.

## Tính chất

### Tính chẵn lẻ

Phân tích hoán vị thành đối hoán không duy nhất. Ví dụ:

$$
(123)=(13)(12)=(12)(23)=(12)(13)(12)(13).
$$

Tuy nhiên, **tính chẵn lẻ** của số đối hoán là bất biến. Hoán vị có số đối hoán chẵn gọi là hoán vị chẵn, lẻ gọi là hoán vị lẻ. Với $n\ge2$, số hoán vị chẵn và lẻ bằng nhau.

### Dấu

Định nghĩa **dấu** (sign) của hoán vị $\sigma$ là $\operatorname{sgn}\sigma$. Hoán vị chẵn có dấu $+1$, lẻ có dấu $-1$.

Dấu của tích:

$$
\operatorname{sgn}(\pi\circ\sigma)=\operatorname{sgn}
\pi\cdot\operatorname{sgn}\sigma.
$$

Hai hoán vị cùng chẵn lẻ thì tích là chẵn, khác chẵn lẻ thì tích là lẻ. Đổi chỗ một lần luôn đổi chẵn lẻ.

Dấu xuất hiện trong [khai triển Leibniz của định thức](../math/linear-algebra/determinant.md#全排列方法定义).

### Bậc của hoán vị

**Bậc** (order) của hoán vị là số nguyên dương nhỏ nhất $a$ sao cho lặp $a$ lần trở về đồng nhất:

$$
\operatorname{ord}\sigma=\min\{a\in\mathbf N_+:\sigma^a=(1)\}.
$$

Trên tập hữu hạn, bậc luôn hữu hạn.

### Kiểu của hoán vị

Phân tích hoán vị thành chu trình, **kiểu** (cycle type) là đa tập độ dài các chu trình. Các độ dài tạo thành một phân hoạch của $n$. Nếu độ dài $k$ xuất hiện $\alpha_k$ lần, kiểu viết:

$$
1^{\alpha_1}2^{\alpha_2}\cdots n^{\alpha_n},
$$

với $\sum_{k=1}^nk\alpha_k=n$.

Số hoán vị có kiểu đó là:

$$
\frac{n!}{1^{\alpha_1}2^{\alpha_2}\cdots n^{\alpha_n}\alpha_1!\alpha_2!\cdots\alpha_n!}.
$$

??? note "Phân tích"
    Vì mọi hoán vị đều có thể phân tích theo kiểu. Các chu trình cùng độ dài có thể hoán đổi vị trí nên cần chia $\prod_k\alpha_k!$. Mỗi chu trình là một vòng tròn nên chọn điểm bắt đầu không ảnh hưởng, cần chia thêm $\prod_k k^{\alpha_k}$.

Nếu chỉ biết số chu trình $c(\sigma)$, số hoán vị là [Stirling loại 1](./combinatorics/stirling.md#第一类斯特林数stirling-number) $\begin{bmatrix}n\\ k\end{bmatrix}$. Số kiểu khác nhau là [số phân hoạch](./combinatorics/partition.md) $p_n$.

Từ kiểu có thể suy ra bậc và chẵn lẻ.

Vì $k$‑chu trình có bậc $k$ và các chu trình rời nhau, nên

$$
\operatorname{lcm}\{k:\alpha_k>0\}.
$$

Chẵn lẻ là:

$$
\sum_k(k-1)\alpha_k=\sum_{k}k\alpha_k-\sum_{k}\alpha_k=n-c(\sigma)
$$

lấy theo parity. Ở đây $c(\sigma)$ là số chu trình (kể cả 1‑chu trình).

Kiểu hoán vị quan trọng trong [đếm Pólya](./combinatorics/polya.md).

## Liên quan đến sắp xếp

Nếu tập $X$ có thứ tự tự nhiên, hoán vị $\sigma$ gọi là sắp xếp và dùng ký hiệu một hàng:

$$
\sigma(1)\sigma(2)\cdots\sigma(n)
$$

Không nhầm với chu trình.

### Số nghịch thế

Trong một sắp xếp, nếu một số lớn đứng trước một số nhỏ thì tạo thành **nghịch thế** (inversion). Tổng số nghịch thế gọi là **số nghịch thế**. Đây là số lần đổi chỗ kề tối thiểu để đưa về thứ tự chuẩn, nên parity trùng với parity của hoán vị.

Tính nghịch thế có thể dùng [merge sort](../basic/merge-sort.md#逆序对) hoặc [Fenwick](../ds/fenwick.md#全局逆序对全局二维偏序), đều $O(n\log n)$. Dưới đây là tham khảo.

??? example "Tham khảo"
    === "Merge sort"
        ```cpp
        --8<-- "docs/math/code/permutation/inversion_2.cpp"
        ```
    
    === "Fenwick"
        ```cpp
        --8<-- "docs/math/code/permutation/inversion_1.cpp"
        ```

### Thứ tự từ điển

Các sắp xếp có thể so sánh theo **thứ tự từ điển** trên chuỗi ký hiệu một hàng. Trong C++ STL, dùng `prev_permutation` và `next_permutation` để lấy hoán vị trước/sau theo thứ tự từ điển.

### Xếp hạng

Liệt kê mọi sắp xếp theo thứ tự từ điển, vị trí của một sắp xếp gọi là **xếp hạng**. Nó tạo song ánh giữa sắp xếp và số nguyên, thường dùng để nén trạng thái.

Trong cộng đồng, thường gọi là “khai triển Cantor”, nhưng tên này không chuẩn. Đúng hơn, xếp hạng của một sắp xếp tương ứng với **mã Lehmer** (Lehmer code).

??? info "Về “khai triển Cantor”"
    Khai triển Cantor là cách biểu diễn số tự nhiên bằng hệ cơ số giai thừa (factorial number system), với cơ số khác nhau ở từng vị trí. Ví dụ:
    
    $$
    463_{10}=341010_{!}.
    $$
    
    Nghĩa là
    
    $$
    463=3\times 5!+4\times 4!+1\times 3!+0\times 2!+1\times 1!+0\times 0!.
    $$
    
    Cantor đã nghiên cứu hệ cơ số hỗn hợp này, nên biểu diễn còn gọi là khai triển Cantor.

??? example "Ví dụ"
    Với sắp xếp $\sigma=452631$, xếp hạng là số sắp xếp nhỏ hơn nó theo thứ tự từ điển cộng một. Tư tưởng giống [DP theo chữ số](../dp/number.md).
    
    -   Vị trí 1 chọn nhỏ hơn 4: $\{1,2,3\}$, còn 5 vị trí: $3\times 5!$;
    -   Vị trí 1 là 4, vị trí 2 nhỏ hơn 5: $\{1,2,3\}$, còn 4 vị trí: $3\times 4!$;
    -   Vị trí 3 nhỏ hơn 2: $\{1\}$, còn 3 vị trí: $1\times 3!$;
    -   Vị trí 4 nhỏ hơn 6: $\{1,3\}$, còn 2 vị trí: $2\times 2!$;
    -   Vị trí 5 nhỏ hơn 3: $\{1\}$, còn 1 vị trí: $1\times 1!$;
    -   Vị trí 6 không còn lựa chọn nhỏ hơn: $0\times 0!$.
    
    Do đó:
    
    $$
    1+3\times 5!+3\times 4!+1\times 3!+2\times 2!+1\times 1!+0\times 0!=444.
    $$
    
    Điểm mấu chốt là hệ số trước giai thừa chính là số phần tử chưa dùng nhỏ hơn phần tử hiện tại.

Từ ví dụ, thuật toán xếp hạng gồm:

1.  Chuyển sắp xếp dài $n$ thành mã Lehmer $L_\sigma$, với

    $$
    L_\sigma(i)=\#\{j>i:\sigma(j)<\sigma(i)\},
    $$

    tức số phần tử sau vị trí $i$ nhỏ hơn $\sigma(i)$, bằng thứ hạng của phần tử trong tập chưa dùng (trừ 1).

2.  Xem mã Lehmer là khai triển Cantor và tính số:

    $$
    \operatorname{rank}\sigma=1+L_\sigma(1)(n-1)!+L_\sigma(2)(n-2)!+\cdots+L_\sigma(n)0!.
    $$

Bài toán ngược lại (từ xếp hạng ra sắp xếp) chỉ cần đảo các bước. Tổng các chữ số trong mã Lehmer bằng số nghịch thế.

Khi cài đặt, cần hỗ trợ truy vấn “thứ hạng trong tập chưa dùng” và “phần tử theo thứ hạng” nhanh, có thể dùng [Fenwick](../ds/fenwick.md) hoặc [segment tree](../ds/seg.md). Cả hai chiều đều $O(n\log n)$.

??? example "Tham khảo"
    === "Tính xếp hạng của sắp xếp"
        ```cpp
        --8<-- "docs/math/code/permutation/perm_rank.cpp"
        ```
    
    === "Từ xếp hạng ra sắp xếp"
        ```cpp
        --8<-- "docs/math/code/permutation/rank_perm.cpp"
        ```

## Tài liệu tham khảo và chú thích

-   [Permutation - Wikipedia](https://en.wikipedia.org/wiki/Permutation)
-   [Lehmer code - Wikipedia](https://en.wikipedia.org/wiki/Lehmer_code)
-   [Factorial number system - Wikipedia](https://en.wikipedia.org/wiki/Factorial_number_system)
