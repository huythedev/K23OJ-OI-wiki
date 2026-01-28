Tác giả: Ir1d, CBW2007, ChungZH, xhn16729, Xeonacid, tptpp, hsfzLZH1, ouuan, Marcythm, HeRaNO, greyqz, Chrogeek, partychicken, zhb2000, xyf007, Persdre, XiaoSuan250, hhc0001, ZhangZhanhaoxiang, Taoran\_01

Trang này chủ yếu giới thiệu tư tưởng cơ bản của Quy hoạch động (Dynamic Programming), cũng như tư duy thiết kế trạng thái và phương trình chuyển trạng thái trong quy hoạch động, giúp những người mới bắt đầu có cái nhìn sơ lược về nội dung này.

Các trang khác trong phần này sẽ giới thiệu phương pháp xây dựng mô hình quy hoạch động cho các loại bài toán khác nhau, cùng một số kỹ thuật tối ưu hóa quy hoạch động.

## Giới thiệu

???+ note "[\[IOI1994\] Tam giác số](https://www.luogu.com.cn/problem/P1216)"
    Cho một tam giác số gồm $r$ hàng ($r \leq 1000$), cần tìm một con đường từ đỉnh cao nhất đến một vị trí bất kỳ ở đáy sao cho tổng các số trên đường đi là lớn nhất. Mỗi bước có thể đi xuống điểm phía dưới bên trái hoặc phía dưới bên phải của điểm hiện tại.
    
    ```plain
            7 
          3   8 
        8   1   0 
      2   7   4   4 
    4   5   2   6   5 
    ```
    
    Trong ví dụ trên, đường đi tối ưu là $7 \to 3 \to 8 \to 7 \to 5$.

Cách tiếp cận đơn giản và thô bạo nhất là thử mọi con đường. Vì số lượng đường đi ở mức $O(2^r)$, cách làm này không thể chấp nhận được.

Chú ý một sự thật rằng: một con đường tối ưu thì mỗi quyết định trong từng bước của nó đều phải là tối ưu.

Lấy đường đi tối ưu trong ví dụ làm mẫu, nếu chỉ xét bốn bước đầu $7 \to 3 \to 8 \to 7$, sẽ không tồn tại con đường nào từ đỉnh đến số thứ $2$ của hàng $4$ có tổng giá trị lớn hơn.

Đối với mỗi điểm, quyết định tiếp theo chỉ có hai lựa chọn: xuống dưới bên trái hoặc xuống dưới bên phải (nếu có). Do đó, chỉ cần ghi lại giá trị lớn nhất tại điểm hiện tại, dùng giá trị này thực hiện quyết định bước tiếp theo để cập nhật giá trị lớn nhất cho các điểm kế tiếp.

Làm như vậy còn có một ưu điểm: chúng ta đã thu hẹp quy mô bài toán thành công, chia một bài toán lớn thành nhiều bài toán quy mô nhỏ hơn. Để tìm phương án tối ưu từ đỉnh đến hàng thứ $r$, chỉ cần biết thông tin về phương án tối ưu từ đỉnh đến hàng thứ $r-1$ là đủ.

Lúc này vẫn còn một vấn đề: phần chồng lấn giữa các bài toán con sẽ rất nhiều, cùng một bài toán con có thể bị truy cập lặp lại nhiều lần, hiệu suất vẫn không cao. Cách giải quyết vấn đề này là lưu trữ lời giải của mỗi bài toán con lại, thông qua phương pháp ghi nhớ (memoization) để hạn chế thứ tự truy cập, đảm bảo mỗi bài toán con chỉ được truy cập một lần.

Trên đây là một số tư tưởng cơ bản của quy hoạch động. Dưới đây sẽ giới thiệu hệ thống hơn về tư tưởng này.

## Nguyên lý Quy hoạch động

Một bài toán có thể giải bằng quy hoạch động cần thỏa mãn ba điều kiện: Cấu trúc con tối ưu, Tính không hệ quả và Bài toán con chồng lấn.

### Cấu trúc con tối ưu (Optimal Substructure)

Việc có cấu trúc con tối ưu cũng có thể phù hợp để giải bằng phương pháp tham lam (Greedy).

Lưu ý cần đảm bảo chúng ta đã xem xét tất cả các bài toán con được sử dụng trong lời giải tối ưu.

1.  Chứng minh rằng thành phần đầu tiên của lời giải tối ưu là thực hiện một lựa chọn;
2.  Đối với một bài toán cho trước, trong các lựa chọn bước đầu tiên có thể, giả định bạn đã biết lựa chọn nào sẽ dẫn đến lời giải tối ưu. Lúc này bạn không quan tâm lựa chọn đó cụ thể có được như thế nào, chỉ giả định là đã biết nó;
3.  Sau khi có lựa chọn tối ưu, xác định lựa chọn này sẽ sinh ra những bài toán con nào và cách mô tả không gian bài toán con tốt nhất;
4.  Chứng minh rằng với tư cách là thành phần cấu thành lời giải tối ưu của bài toán gốc, lời giải của mỗi bài toán con chính là lời giải tối ưu của chính nó. Phương pháp là phản chứng: giả sử lời giải của một bài toán con không phải là tối ưu của chính nó, thì ta có thể thay thế lời giải không tối ưu đó bằng lời giải tối ưu của bài toán con đó vào lời giải bài toán gốc để được một lời giải tốt hơn, mâu thuẫn với giả thiết ban đầu.

Cần giữ cho không gian bài toán con đơn giản nhất có thể, chỉ mở rộng khi cần thiết.

Sự khác biệt của cấu trúc con tối ưu thể hiện ở hai khía cạnh:

1.  Lời giải tối ưu của bài toán gốc liên quan đến bao nhiêu bài toán con;
2.  Khi xác định sử dụng những bài toán con nào cho lời giải tối ưu, cần xem xét bao nhiêu lựa chọn.

Mỗi đỉnh trong đồ thị bài toán con tương ứng với một bài toán con, và các lựa chọn cần xem xét tương ứng với các cạnh nối đến đỉnh bài toán con đó.

### Tính không hệ quả (No-after-effect)

Các bài toán con đã được giải sẽ không bị ảnh hưởng bởi các quyết định sau đó.

### Bài toán con chồng lấn (Overlapping Subproblems)

Nếu có một lượng lớn các bài toán con chồng lấn, chúng ta có thể dùng không gian lưu trữ lời giải của các bài toán con này để tránh việc giải lặp lại, từ đó nâng cao hiệu suất.

### Tư tưởng cơ bản

Đối với một bài toán có thể giải bằng quy hoạch động, thường áp dụng quy trình sau:

1.  Chia bài toán gốc thành nhiều **giai đoạn**, mỗi giai đoạn tương ứng với một số bài toán con, trích xuất đặc trưng của các bài toán con này (gọi là **trạng thái**);
2.  Tìm các **quyết định** khả thi cho mỗi trạng thái, hoặc cách chuyển đổi giữa các trạng thái (mô tả bằng ngôn ngữ toán học gọi là **phương trình chuyển trạng thái**);
3.  Giải quyết bài toán của từng giai đoạn theo thứ tự.

Nếu hiểu theo tư tưởng đồ thị, chúng ta xây dựng một [đồ thị có hướng không chu trình (DAG)](../graph/dag.md), mỗi trạng thái tương ứng với một nút trên đồ thị, quyết định tương ứng với các cạnh nối giữa các nút. Như vậy bài toán chuyển thành việc tìm đường đi dài (ngắn) nhất trên DAG (xem thêm: [DP trên DAG](./dag.md)).

## Dãy con chung dài nhất (Longest Common Subsequence)

???+ note "Bài toán dãy con chung dài nhất"
    Cho một dãy $A$ độ dài $n$ và một dãy $B$ độ dài $m$ ($n, m \leq 5000$), tìm một dãy dài nhất sao cho dãy đó vừa là dãy con của $A$, vừa là dãy con của $B$.

Định nghĩa dãy con có thể tham khảo [Xâu con](../string/basic.md). Ví dụ ngắn gọn: xâu `abcde` và xâu `acde` có các dãy con chung là `a`, `c`, `d`, `e`, `ac`, `ad`, `ae`, `cd`, `ce`, `de`, `acd`, `ade`, `ace`, `cde`, `acde`, độ dài dãy con chung dài nhất là 4.

Gọi $f(i, j)$ là độ dài dãy con chung dài nhất khi chỉ xét $i$ phần tử đầu của $A$ và $j$ phần tử đầu của $B$. Việc tìm độ dài này chính là **bài toán con**. $f(i, j)$ là cái chúng ta gọi là **trạng thái**, khi đó $f(n, m)$ là trạng thái cuối cùng cần đạt tới, tức là kết quả cần tìm.

Với mỗi $f(i, j)$, có ba quyết định: nếu $A_i = B_j$, ta có thể nối nó vào cuối dãy con chung; hai quyết định còn lại lần lượt là bỏ qua $A_i$ hoặc $B_j$. Phương trình chuyển trạng thái như sau:

$$
f(i,j)=\begin{cases}f(i-1,j-1)+1&A_i=B_j\\\max(f(i-1,j),f(i,j-1))&A_i\ne B_j\end{cases}
$$

Có thể tham khảo [Trang web tương tác LCS của SourceForge](http://lcs-demo.sourceforge.net/) để hiểu rõ hơn quá trình thực hiện LCS.

???+ example "Tham khảo mã nguồn"
    === "C++"
        ```cpp
        --8<-- "docs/dp/code/basic/lcs.cpp:core"
        ```
    
    === "Python"
        ```cpp
        --8<-- "docs/dp/code/basic/lcs.py:core"
        ```

Độ phức tạp thời gian của cách làm này là $O(nm)$.

Ngoài ra, bài toán này tồn tại thuật toán $O\left(\dfrac{nm}{w}\right)$ [^ref1]. Các bạn có hứng thú có thể tự tìm hiểu.

## Dãy con không giảm dài nhất (Longest Non-decreasing Subsequence)

???+ note "Bài toán dãy con không giảm dài nhất"
    Cho một dãy $a$ độ dài $n$ ($n \leq 5000$), tìm một dãy con dài nhất của $a$ thỏa mãn phần tử đứng sau không nhỏ hơn phần tử đứng trước.

### Thuật toán 1

Gọi $f(i)$ là độ dài dãy con không giảm dài nhất kết thúc tại $a_i$, kết quả cần tìm là $\max_{1 \leq i \leq n} f(i)$.

Khi tính $f(i)$, thử nối $a_i$ vào sau các dãy con không giảm dài nhất khác để cập nhật kết quả. Do đó có thể viết phương trình chuyển trạng thái: $f(i)=\max_{1 \leq j < i,~a_j \leq a_i} (f(j)+1)$.

???+ example "Tham khảo mã nguồn"
    === "C++"
        ```cpp
        --8<-- "docs/dp/code/basic/lis-1.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/dp/code/basic/lis-1.py:core"
        ```

Dễ thấy độ phức tạp thời gian của thuật toán này là $O(n^2)$.

### Thuật toán 2

Khi phạm vi của $n$ mở rộng đến $n \leq 10^5$, cách làm thứ nhất không còn đủ nhanh, dưới đây là cách làm $O(n \log n)$.

Xét trạng thái $(i, l)$ đã định nghĩa trước đó, biểu thị dãy con không giảm kết thúc tại phần tử thứ $i$ có độ dài lớn nhất là $l$. Khác với phương pháp xử lý trạng thái theo $i$ cố định, ở đây ta trực tiếp kiểm tra xem $(i, l)$ có hợp lệ hay không:

-   Trạng thái ban đầu $(1, 1)$ chắc chắn hợp lệ.
-   Với bất kỳ $(i, l)$ nào, nếu tồn tại $j < i$ sao cho $(j, l-1)$ hợp lệ và $a_j \leq a_i$, thì $(i, l)$ hợp lệ.

Cuối cùng, chỉ cần tìm giá trị $l$ lớn nhất trong các trạng thái hợp lệ $(i, l)$ là có được độ dài dãy con không giảm dài nhất.

Giả sử dãy ban đầu là $a_1, \dots, a_n$, định nghĩa mảng $d$, trong đó vị trí thứ $x$ lưu giá trị nhỏ nhất của phần tử cuối cùng trong các dãy con không giảm độ dài $x$. Ban đầu dãy trống. Cho $i$ chạy từ $1$ đến $n$, lần lượt tìm độ dài dãy con không giảm dài nhất của $i$ phần tử đầu tiên. Đối với phần tử hiện tại $a_i$:

-   Nếu $a_i$ lớn hơn hoặc bằng phần tử cuối cùng trong dãy $d$, trực tiếp chèn $a_i$ vào cuối dãy $d$.
    -   Giải thích: Nếu $a_i$ không nhỏ hơn phần tử cuối của dãy con dài nhất hiện tại, chứng tỏ tồn tại một dãy con không giảm có thể nối thêm $a_i$. Không chèn vào sẽ làm mất tính tối ưu.
-   Nếu $a_i$ nhỏ hơn phần tử cuối cùng trong $d$, tìm phần tử **đầu tiên** lớn hơn nó và thay thế bằng $a_i$.
    -   Giải thích: Nếu chèn trực tiếp vào cuối sẽ phá hỏng tính đơn điệu của $d$; thao tác thay thế đảm bảo phần tử cuối cùng của mỗi độ dài là nhỏ nhất có thể, từ đó để lại nhiều khả năng hơn cho các phần tử sau.
    -   Tối ưu: Vì $d$ là dãy đơn điệu không giảm, có thể dùng tìm kiếm nhị phân để trực tiếp tìm vị trí chèn, giảm độ phức tạp tổng thể xuống $O(n \log n)$ thay vì $O(n^2)$ nếu tìm kiếm tuần tự.

Nếu cần xuất dãy con không giảm dài nhất cụ thể, có thể duy trì thêm mảng $d'_x$ lưu vị trí của phần tử cuối nhỏ nhất của dãy con độ dài $x$. Khi thực hiện, chỉ cần cập nhật $d'_x = i$ khi chèn $a_i$ vào $d_x$. Đồng thời, cần ghi lại tiền bối tối ưu $p_i$ của $i$ là $d'_{x-1}$. Cuối cùng, từ bất kỳ trạng thái có độ dài cực đại nào, lần theo tiền bối $p_i$ để truy hồi lại toàn bộ dãy con.

???+ example "Tham khảo mã nguồn"
    === "C++"
        ```cpp
        --8<-- "docs/dp/code/basic/lis-2.cpp:core"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/dp/code/basic/lis-2.py:core"
        ```

Độ phức tạp thời gian của thuật toán này là $O(n \log n)$. Độ phức tạp thời gian để xuất kết quả là $O(\textit{ans})$.

???+ tip "Lưu ý"
    Đối với bài toán dãy con **tăng** dài nhất, tương tự, có thể gọi $d_i$ là giá trị nhỏ nhất của phần tử cuối cùng trong tất cả các dãy con tăng dài nhất độ dài $i$.
    
    Cần chú ý rằng ở bước 2, nếu $a_i \leq d_{len}$, do các phần tử liền kề trong dãy con tăng không được bằng nhau, cần tìm phần tử **đầu tiên** **không nhỏ hơn** $a_i$ trong dãy $d$ và thay thế nó bằng $a_i$.
    
    Về mặt cài đặt (lấy C++ làm ví dụ), cần đổi hàm `upper_bound` thành `lower_bound`.

## Tài liệu tham khảo và chú thích

-   [Giải thích chi tiết thuật toán nlogn cho dãy con không giảm dài nhất - lvmememe - Blog Garden](https://www.cnblogs.com/itlqs/p/5743114.html)

[^ref1]: [Dùng phép toán bit tìm dãy con chung dài nhất - -Wallace- - Blog Garden](https://www.cnblogs.com/-Wallace-/p/bit-lcs.html)