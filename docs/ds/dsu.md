tác giả: HeRaNO, JuicyMio, Xeonacid, sailordiary, ouuan, Pig-Eat-Earth

![](images/disjoint-set.svg)

## Giới thiệu

Tập hợp hợp nhất (Disjoint Set Union - DSU) là một cấu trúc dữ liệu dùng để quản lý các tập hợp của các phần tử, được hiện thực dưới dạng một rừng (forest), trong đó mỗi cây biểu thị một tập hợp, các nút trong cây biểu thị các phần tử tương ứng của tập hợp đó.

Đúng như tên gọi, DSU hỗ trợ hai thao tác:

-   Hợp nhất (Unite): Hợp nhất tập hợp chứa hai phần tử (hợp nhất các cây tương ứng).
-   Truy vấn (Find): Truy vấn tập hợp chứa một phần tử (truy vấn nút gốc của cây tương ứng), điều này có thể dùng để phán đoán hai phần tử có thuộc cùng một tập hợp hay không.

DSU sau khi được sửa đổi có thể hỗ trợ việc xóa, di chuyển một phần tử đơn lẻ hoặc duy trì trọng số cạnh trên cây. Sử dụng cây đoạn với điểm mở động, ta còn có thể thực hiện [DSU bền vững (Persistent DSU)](persistent-seg.md#拓展基于主席树的可持久化并查集).

???+ warning "Cảnh báo"
    DSU không thể thực hiện việc tách tập hợp với độ phức tạp thấp.

## Khởi tạo

Ban đầu, mỗi phần tử nằm trong một tập hợp riêng biệt, được biểu diễn dưới dạng một cây chỉ có nút gốc. Để tiện cho việc tham khảo, ta đặt cha của nút gốc là chính nó.

???+ example "Triển khai"
    === "C++"
        ```cpp
        struct dsu {
          vector<size_t> pa;
        
          explicit dsu(size_t size) : pa(size) { iota(pa.begin(), pa.end(), 0); }
        };
        ```
    
    === "Python"
        ```python
        class Dsu:
            def __init__(self, size):
                self.pa = list(range(size))
        ```

## Truy vấn

Ta cần di chuyển lên trên dọc theo cây cho đến khi tìm thấy nút gốc.

![](images/disjoint-set-find.svg)

???+ example "Triển khai"
    === "C++"
        ```cpp
        size_t dsu::find(size_t x) { return pa[x] == x ? x : find(pa[x]); }
        ```
    
    === "Python"
        ```python
        def find(self, x):
            return x if self.pa[x] == x else self.find(self.pa[x])
        ```

### Nén đường đi (Path Compression)

Mỗi phần tử đi qua trong quá trình truy vấn đều thuộc tập hợp này, ta có thể nối trực tiếp chúng với nút gốc để tăng tốc truy vấn sau này.

![](images/disjoint-set-compress.svg)

???+ example "Triển khai"
    === "C++"
        ```cpp
        size_t dsu::find(size_t x) { return pa[x] == x ? x : pa[x] = find(pa[x]); }
        ```
    
    === "Python"
        ```python
        def find(self, x):
            if self.pa[x] != x:
                self.pa[x] = self.find(self.pa[x])
            return self.pa[x]
        ```

## Hợp nhất

Để hợp nhất hai cây, ta chỉ cần nối nút gốc của một cây vào nút gốc của cây kia.

![](images/disjoint-set-merge.svg)

???+ example "Triển khai"
    === "C++"
        ```cpp
        void dsu::unite(size_t x, size_t y) { pa[find(x)] = find(y); }
        ```
    
    === "Python"
        ```python
        def unite(self, x, y):
            self.pa[self.find(x)] = self.find(y)
        ```

### Hợp nhất Heuristic (Union by Size/Rank)

Khi hợp nhất, việc chọn nút gốc của cây nào làm nút gốc của cây mới sẽ ảnh hưởng đến độ phức tạp của các thao tác trong tương lai. Ta có thể nối cây có số lượng nút nhỏ hơn hoặc độ sâu nhỏ hơn vào cây kia, để tránh suy biến.

??? note "Thảo luận chi tiết về độ phức tạp"
    Vì ta chỉ cần hỗ trợ các thao tác hợp nhất và truy vấn tập hợp, khi cần hợp nhất hai tập hợp thành một, việc nối cây của tập hợp nào vào cây của tập hợp nào cũng sẽ cho ra kết quả đúng. Nhưng các phương pháp nối khác nhau sẽ có sự khác biệt về độ phức tạp thời gian. Cụ thể, nếu ta nối cây của tập hợp có số lượng nút và độ sâu nhỏ hơn vào một cây tập hợp lớn hơn, rõ ràng so với phương án nối ngược lại, thời gian thực hiện thao tác tìm kiếm sau này sẽ nhỏ hơn (cũng sẽ mang lại độ phức tạp thời gian xấu nhất tối ưu hơn).

    Tất nhiên, ta không phải lúc nào cũng gặp các tập hợp mà số lượng nút và độ sâu đều nhỏ hơn. Do đặc điểm về số lượng nút và độ sâu đều dễ duy trì, ta thường chọn một trong hai làm hàm đánh giá. Mà dù chọn cái nào, độ phức tạp thời gian vẫn là $O (m\alpha(m,n))$, chứng minh cụ thể có thể xem tại các bài báo được trích dẫn trong References.

    Trong mã thực tế của các cuộc thi thuật toán, ngay cả khi không sử dụng hợp nhất heuristic, mã nguồn vẫn thường hoàn thành trong thời gian quy định. Trong bài báo của Tarjan[^tarjan1984worst], đã chứng minh rằng độ phức tạp thời gian xấu nhất của DSU chỉ dùng nén đường đi mà không dùng hợp nhất heuristic là $O (m \log n)$. Trong bài báo của Andrew Yao[^yao1985expected], đã chứng minh rằng trong trường hợp trung bình, độ phức tạp thời gian của DSU chỉ dùng nén đường đi mà không dùng hợp nhất heuristic vẫn là $O (m\alpha(m,n))$.

    Nếu chỉ dùng hợp nhất heuristic mà không dùng nén đường đi, độ phức tạp thời gian là $O(m\log n)$. Vì nén đường đi có thể gây ra sửa đổi lớn trong một lần hợp nhất, đôi khi nén đường đi không phù hợp để sử dụng. Ví dụ, trong DSU bền vững, hoặc DSU + Chia để trị trên cây đoạn, thông thường sử dụng DSU chỉ có hợp nhất heuristic.

Tham khảo cài đặt hợp nhất theo số lượng nút: (Chú ý cần điều chỉnh phương thức khởi tạo)

???+ example "Triển khai tham khảo"
    === "C++"
        ```cpp
        struct dsu {
          vector<size_t> pa, size;
        
          explicit dsu(size_t size_) : pa(size_), size(size_, 1) {
            iota(pa.begin(), pa.end(), 0);
          }
        
          void unite(size_t x, size_t y) {
            x = find(x), y = find(y);
            if (x == y) return;
            if (size[x] < size[y]) swap(x, y);
            pa[y] = x;
            size[x] += size[y];
          }
        };
        ```
    
    === "Python"
        ```python
        class Dsu:
            def __init__(self, size):
                self.pa = list(range(size))
                self.size = [1] * size
        
            def unite(self, x, y):
                x, y = self.find(x), self.find(y)
                if x == y:
                    return
                if self.size[x] < self.size[y]:
                    x, y = y, x
                self.pa[y] = x
                self.size[x] += self.size[y]
        ```

## Triển khai tham khảo

Triển khai hoàn chỉnh của DSU có nén đường đi và hợp nhất theo số lượng nút như sau:

??? example "Tham khảo triển khai cho bài toán mẫu [Luogu P3367【Mẫu】DSU](https://www.luogu.com.cn/problem/P3367)"
    === "C++"
        ```cpp
        --8<-- "docs/ds/code/dsu/dsu_0.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/ds/code/dsu/dsu_0.py"
        ```

## Độ phức tạp

Sau khi sử dụng cả nén đường đi và hợp nhất heuristic, thời gian trung bình cho mỗi thao tác DSU chỉ là $O(\alpha(n))$. Trong đó, $\alpha$ là hàm ngược của hàm Ackermann, tăng trưởng cực kỳ chậm. Điều này có nghĩa là, thời gian chạy trung bình của một thao tác DSU có thể được coi là một hằng số rất nhỏ. Chứng minh độ phức tạp thời gian nằm ở [trang này](./dsu-complexity.md).

???+ info "Hàm Ackermann ngược"
    Định nghĩa của hàm [Ackermann](https://en.wikipedia.org/wiki/Ackermann_function) $A(m, n)$ là:
    
    $A(m, n) = \begin{cases}n+1&\text{if }m=0\\A(m-1,1)&\text{if }m>0\text{ and }n=0\\A(m-1,A(m,n-1))&\text{otherwise}\end{cases}$
    
    Và hàm Ackermann ngược $\alpha(n)$ được định nghĩa là số nguyên lớn nhất $m$ sao cho $A(m, m) \leqslant n$.

Độ phức tạp không gian của DSU rõ ràng là $O(n)$.

## Thao tác mở rộng

Dựa trên DSU thông thường, có thể thực hiện một loạt các sửa đổi để hỗ trợ nhiều thao tác hơn hoặc duy trì thông tin phức tạp hơn.

### DSU có hỗ trợ xóa (DSU with Deletion)

DSU thông thường không hỗ trợ thao tác xóa, vì khi xóa một nút, không thể tránh khỏi việc xóa tất cả các nút trong cây con có gốc là nút đó. Để giải quyết vấn đề này, trong DSU có hỗ trợ xóa, thông qua việc xây dựng các nút ảo (dummy nodes), ta có thể đảm bảo rằng tất cả các nút thực sự lưu trữ dữ liệu luôn là nút lá. Để làm được điều này, khi khởi tạo, cần tạo một nút ảo cho mỗi nút dữ liệu, và đặt cha của nút dữ liệu là nút ảo tương ứng. Do mỗi lần hợp nhất hai tập hợp, ta luôn nối gốc của một cây vào gốc của cây kia, mà các nút gốc đều là nút ảo, nên từ đầu đến cuối chỉ có các nút ảo mới có nút con, còn tất cả các nút lưu trữ phần tử thực tế đều không có nút con. Khi đó, việc di chuyển một phần tử trở nên dễ thực hiện hơn nhiều.

Chú ý, sau khi xóa một nút đơn lẻ, cần phải xây dựng lại một nút ảo mới làm cha của nút đó; nếu không, các thao tác hợp nhất và xóa tiếp theo sẽ không thực hiện chính xác.

??? example "Tham khảo triển khai cho bài toán mẫu [SPOJ JMFILTER - Junk-Mail Filter](https://www.spoj.com/problems/JMFILTER/)"
    === "C++"
        ```cpp
        --8<-- "docs/ds/code/dsu/dsu_4.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/ds/code/dsu/dsu_4.py"
        ```

Phương pháp tương tự cũng có thể được sử dụng để thực hiện việc di chuyển một phần tử đơn lẻ giữa các tập hợp. Chi tiết triển khai xem trong ví dụ.

### DSU có trọng số (Weighted DSU)

Ta cũng có thể định nghĩa một loại trọng số trên các cạnh của DSU và các phép toán tương ứng sinh ra khi nén đường đi, từ đó giải quyết nhiều vấn đề hơn. Ví dụ, đối với bài toán kinh điển "NOI2001" Chuỗi thức ăn, ta có thể duy trì phép cộng modulo $3$ trên trọng số cạnh. Đối với các vấn đề duy trì trọng số cạnh theo modulo mà modulo rất nhỏ, ta còn có thể giải quyết bằng cách tách một điểm trong DSU thành nhiều trạng thái. Kỹ thuật đặc biệt này cũng được gọi là "DSU theo loại" (Species DSU) hoặc "DSU miền mở rộng". Phần sau sẽ dùng ví dụ để minh họa các cách làm này.

Để duy trì trọng số cạnh trong DSU, cần lưu trữ trọng số cạnh dưới dạng giá trị đi xuống nút con. Do đó, mỗi nút lưu trữ trọng số cạnh giữa nó và nút cha của nó. Chỉ khi nút cha của một nút thay đổi, ta mới cần điều chỉnh trọng số cạnh tương ứng. Trong trường hợp tổng quát, điều này có thể xảy ra khi nén đường đi và hợp nhất hai nút. Ví dụ, nếu trọng số cạnh là khoảng cách giữa nút hiện tại và nút cha, thì khi nén đường đi, mỗi lần thay thế nút cha hiện tại bằng nút gốc, cần cộng khoảng cách từ nút cha đến nút gốc vào trọng số cạnh được lưu trữ của nút hiện tại; tương tự, khi hợp nhất hai tập hợp nút, cần tính toán trọng số của cạnh mới nối hai nút gốc.

??? example "Tham khảo triển khai cho bài toán mẫu [Library Checker - Unionfind with Potential](https://judge.yosupo.jp/problem/unionfind_with_potential)"
    === "C++"
        ```cpp
        --8<-- "docs/ds/code/dsu/dsu_5.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/ds/code/dsu/dsu_5.py"
        ```

## Bài tập ví dụ

Trong các cuộc thi thuật toán, các bài toán trực tiếp kiểm tra DSU đa số đều cần thiết kế cấu trúc đặc biệt cho bài toán.

???+ example "[UVa11987 Almost Union-Find](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=229&page=show_problem&problem=3138)"
    Triển khai một cấu trúc dữ liệu tương tự DSU, hỗ trợ các thao tác sau:
    
    1.  Hợp nhất tập hợp chứa hai phần tử.
    2.  Di chuyển một phần tử đơn lẻ sang tập hợp chứa một phần tử khác.
    3.  Truy vấn kích thước tập hợp chứa một phần tử và tổng các phần tử trong tập hợp đó.

??? note "Giải đáp"
    Trong bài toán này, thao tác 1 và 3 dễ xử lý, điểm khó là thao tác 2. Giả sử cần di chuyển phần tử $x$ sang tập hợp chứa phần tử $y$. Trong DSU thông thường, việc trực tiếp đặt cha của phần tử $x$ là nút gốc của tập hợp chứa $y$ là không được, vì điều này sẽ di chuyển tất cả các phần tử trong cây con có gốc là $x$ cùng một lúc. Để giải quyết vấn đề này, giải pháp là đảm bảo phần tử $x$ không có nút con. Để làm được điều này, khi xây dựng DSU, ta cho mỗi phần tử $x$ tạo một nút ảo $\tilde x$, và đặt cha của phần tử $x$ trỏ đến nút ảo $\tilde x$ tương ứng. Khi đó, khi hợp nhất hai tập hợp, vì luôn nối gốc của một cây vào gốc của cây kia, mà các nút gốc đều là nút ảo, nên chỉ có các nút ảo mới có nút con, còn tất cả các nút lưu trữ phần tử thực tế đều không có nút con. Lúc này, việc di chuyển một phần tử trở nên dễ thực hiện hơn nhiều.

??? note "Tham khảo triển khai"
    === "C++"
        ```cpp
        --8<-- "docs/ds/code/dsu/dsu_1.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/ds/code/dsu/dsu_1.py"
        ```

???+ example "[Luogu P2024「NOI2011」Chuỗi Thức Ăn](https://www.luogu.com.cn/problem/P2024)"
    Vương quốc động vật có ba loại động vật $A, B, C$, chuỗi thức ăn của ba loại này tạo thành một vòng tròn thú vị. $A$ ăn $B$, $B$ ăn $C$, $C$ ăn $A$.
    
    Có $N$ con vật, được đánh số từ $1$ đến $N$. Mỗi con vật thuộc một trong ba loại $A, B, C$, nhưng ta không biết nó là loại nào.
    
    Có người dùng hai loại phát biểu để mô tả mối quan hệ chuỗi thức ăn của $N$ con vật này:
    
    -   Phát biểu loại 1 là `1 X Y`, biểu thị $X$ và $Y$ cùng loại.
    -   Phát biểu loại 2 là `2 X Y`, biểu thị $X$ ăn $Y$.
    
    Người này nói $K$ câu, mỗi câu là một trong hai loại trên đối với $N$ con vật, trong số $K$ câu này có câu đúng, có câu sai. Khi một câu thỏa mãn một trong ba điều kiện sau, câu đó là câu sai, nếu không là câu đúng:
    
    -   Câu hiện tại mâu thuẫn với một số câu đúng trước đó, là câu sai;
    -   Câu hiện tại có $X$ hoặc $Y$ lớn hơn $N$, là câu sai;
    -   Câu hiện tại biểu thị $X$ ăn $X$, là câu sai.
    
    Nhiệm vụ của bạn là dựa vào $N$ và $K$ câu đã cho, xuất ra tổng số câu sai.

??? note "Giải đáp 1"
    Cân nhắc sử dụng DSU có trọng số để duy trì thông tin chuỗi thức ăn. Nếu $x$ và $y$ cùng loại, thì $x\equiv y\pmod 3$; nếu $x$ ăn $y$, thì $x - y \equiv 1 \pmod 3$. Như vậy bài toán này được quy về bài toán mẫu ở phần trước.
    
    Cụ thể, đối với mỗi câu, loại trừ những câu sai hiển nhiên như $x>n$ hoặc $y>n$, cần phán đoán $x$ và $y$ đã được kết nối hay chưa: nếu đã kết nối, tính toán khoảng cách modulo của chúng và so sánh với thông tin mà câu này tuyên bố; nếu chưa, kết nối hai nút theo thông tin mà câu này cung cấp. Ngoài các trường hợp hiển nhiên, một câu là câu sai khi và chỉ khi hai nút được đề cập đã được kết nối và khoảng cách tương ứng mâu thuẫn với thông tin mà câu này tuyên bố.

??? note "Tham khảo triển khai 1"
    === "C++"
        ```cpp
        --8<-- "docs/ds/code/dsu/dsu_6.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/ds/code/dsu/dsu_6.py"
        ```

??? note "Giải đáp 2"
    Tách một loài vật $x$ thành ba trạng thái. Trong triển khai cụ thể, ta có thể coi các trạng thái khác nhau là các phần tử khác nhau:
    
    -   Trạng thái cùng tập hợp với $x$ có nghĩa là $x$ cùng loài;
    -   Trạng thái cùng tập hợp với $x+n$ có nghĩa là $x$ bị ăn bởi;
    -   Trạng thái cùng tập hợp với $x+2n$ có nghĩa là ăn $x$.
    
    Vậy, đối với một câu:
    
    -   `1 x y` là câu sai khi và chỉ khi:
    
        1.  $x>N$ hoặc $y>N$;
        2.  $y$ cùng tập hợp với $x+n$ hoặc $x+2n$.
    -   `2 x y` là câu sai khi và chỉ khi:
    
        1.  $x>N$ hoặc $y>N$;
        2.  $y$ cùng tập hợp với $x$ hoặc $x+2n$.
    -   Nếu là câu đúng, hợp nhất các trạng thái tương ứng.

??? note "Tham khảo triển khai 2"
    === "C++"
        ```cpp
        --8<-- "docs/ds/code/dsu/dsu_2.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/ds/code/dsu/dsu_2.py"
        ```

???+ example "[ABC396E Min of Restricted Sum](https://atcoder.jp/contests/abc396/tasks/abc396_e)"
    Cho các số nguyên $N, M$ và các dãy số nguyên có độ dài $M$: $X=(X_1,X_2,\ldots,X_M)$、$Y=(Y_1,Y_2,\ldots,Y_M)$、$Z=(Z_1,Z_2,\ldots,Z_M)$. Trong đó, đảm bảo tất cả các phần tử của $X$ và $Y$ đều nằm trong phạm vi $1$ đến $N$.
    
    Dãy số nguyên không âm có độ dài $N$, $A=(A_1,A_2,\ldots,A_N)$, được định nghĩa là **dãy số nguyên tốt** khi và chỉ khi thỏa mãn điều kiện sau:
    
    -   Với mọi số nguyên $i$ thỏa mãn $1 \leq i \leq M$, có $A_{X_i} \oplus A_{Y_i} = Z_i$, trong đó $\oplus$ biểu thị phép toán XOR.
    
    Hãy phán đoán xem có tồn tại dãy số nguyên tốt như vậy không. Nếu tồn tại, hãy tìm dãy số nguyên tốt sao cho tổng các phần tử $\displaystyle \sum_{i=1}^N A_i$ là nhỏ nhất, và xuất ra dãy đó.

??? note "Giải đáp"
    Phép toán XOR chính là quan hệ "giống nhau" hay "khác nhau" trên từng bit nhị phân. Vậy, tách tất cả các bit của $A_i$ ra, quan hệ XOR có thể được duy trì bằng DSU có trọng số (hoặc DSU theo loại). Khi tính tổng, các phần tử trong cùng một thành phần liên thông thường được chia thành hai nhóm, giữa hai nhóm này cần có giá trị khác nhau, chỉ cần gán giá trị 0 cho nhóm có giá trị lớn hơn và 1 cho nhóm còn lại là đảm bảo tổng trọng số nhỏ nhất.

??? note "Tham khảo triển khai"
    === "C++"
        ```cpp
        --8<-- "docs/ds/code/dsu/dsu_3.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/ds/code/dsu/dsu_3.py"
        ```

???+ example "Bài tập thực hành"
-   [「NOI2015」Phân tích chương trình tự động](https://uoj.ac/problem/127)
-   [「JSOI2008」Chiến tranh giữa các vì sao](https://www.luogu.com.cn/problem/P1197)
-   [「NOIP2023」Logic ba giá trị](https://www.luogu.com.cn/problem/P9869)
-   [「NOI2002」Truyền thuyết anh hùng thiên hà](https://www.luogu.com.cn/problem/P1196)

## Các ứng dụng khác

Thuật toán Kruskal trong [Thuật toán cây bao trùm nhỏ nhất](../graph/mst.md) và thuật toán Tarjan trong [Tổ tiên chung gần nhất](../graph/lca.md) đều là các thuật toán dựa trên DSU.

Xem các chuyên đề liên quan tại [Ứng dụng của DSU](../topic/dsu-app.md).

## Tài liệu tham khảo và Đọc thêm

1.  [Trả lời trên Zhihu: Có thực sự tối ưu hóa nén đường đi hai lần trong DSU không?](https://www.zhihu.com/question/28410263/answer/40966441)
2.  Gabow, H. N., & Tarjan, R. E. (1985). A Linear-Time Algorithm for a Special Case of Disjoint Set Union. JOURNAL OF COMPUTER AND SYSTEM SCIENCES, 30, 209-221.[PDF](https://dl.acm.org/doi/pdf/10.1145/800061.808753)
3.  [CSDN: DSU miền mở rộng & DSU có trọng số](https://blog.csdn.net/qqqqqwerttwtwe/article/details/145440100)

[^tarjan1984worst]: Tarjan, R. E., & Van Leeuwen, J. (1984). Worst-case analysis of set union algorithms. Journal of the ACM (JACM), 31(2), 245-281.[ResearchGate PDF](https://www.researchgate.net/profile/Jan_Van_Leeuwen2/publication/220430653_Worst-case-Analysis-of-Set-Union-Algorithms/links/0a85e53cd28bfdf5eb000000/Worst-case-Analysis-of-Set-Union-Algorithms.pdf)

[^yao1985expected]: Yao, A. C. (1985). On the expected performance of path compression algorithms.[SIAM Journal on Computing, 14(1), 129-133.](https://epubs.siam.org/doi/abs/10.1137/0214010?journalCode=smjcat)