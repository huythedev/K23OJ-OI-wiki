author: JiZiQian, llleixx, firefly-zjyjoe

## Cây lệch trái là gì?

**Cây lệch trái** (Leftist Tree), giống như [**Pairing Heap**](./pairing-heap.md), là một loại **Heap hợp nhất được** (Mergeable Heap), có các tính chất của heap và hỗ trợ thao tác hợp nhất nhanh chóng.

## Định nghĩa và tính chất của Cây lệch trái

Đối với một cây nhị phân, ta định nghĩa **nút ngoài** (external node) là nút có ít hơn hai con. Định nghĩa $\mathrm{dist}$ của một nút là số lượng cạnh đi qua trên đường đi ngắn nhất từ nút đó đến một nút ngoài trong cây con của nó. $\mathrm{dist}$ của nút rỗng (null node) là $0$.

???+ note "Lưu ý"
    Một số tài liệu định nghĩa $\mathrm{dist}$ nhỏ hơn định nghĩa trong bài viết này $1$ đơn vị. Định nghĩa như vậy giúp việc viết code bỏ qua được một số bước kiểm tra rỗng, nhưng cần lưu ý phải khởi tạo $\mathrm{dist}$ của nút rỗng là $-1$. Tất cả code trong bài viết này đều **sử dụng định nghĩa $\mathrm{dist}$ của nút rỗng là $-1$**, hãy chú ý sự khác biệt này so với định nghĩa trong văn bản.

Cây lệch trái là một cây nhị phân, không chỉ có tính chất của heap mà còn có tính chất "lệch trái": với mọi nút, $\mathrm{dist}$ của con trái luôn lớn hơn hoặc bằng $\mathrm{dist}$ của con phải.

Do đó, $\mathrm{dist}$ của mỗi nút trong Cây lệch trái luôn bằng $\mathrm{dist}$ của con phải cộng thêm $1$.

Cần lưu ý rằng, $\mathrm{dist}$ không phải là độ sâu, **độ sâu của Cây lệch trái không được đảm bảo**, một chuỗi các nút chỉ có con trái (dạng đường thẳng) cũng thỏa mãn định nghĩa của Cây lệch trái.

## Thao tác cốt lõi: Hợp nhất (merge)

Khi hợp nhất hai heap, để thỏa mãn tính chất heap, trước tiên ta chọn gốc có giá trị nhỏ hơn (để thuận tiện, bài viết này xét min-heap) làm gốc của heap sau khi hợp nhất. Sau đó, giữ nguyên con trái của gốc này, và đệ quy hợp nhất con phải của nó với heap còn lại để làm con phải mới. Để thỏa mãn tính chất lệch trái, sau khi hợp nhất, nếu $\mathrm{dist}$ của con trái nhỏ hơn $\mathrm{dist}$ của con phải, ta đổi chỗ hai con này.

Code tham khảo:

???+ note "Hiện thực"
    ```cpp
    int merge(int x, int y) {
      if (!x || !y) return x | y;  // Nếu một heap rỗng thì trả về heap kia
      if (t[x].val > t[y].val) swap(x, y);  // Lấy nút có giá trị nhỏ hơn làm gốc
      t[x].rs = merge(t[x].rs, y);          // Đệ quy hợp nhất con phải với heap kia
      if (t[t[x].rs].d > t[t[x].ls].d)
        swap(t[x].ls, t[x].rs);   // Nếu không thỏa mãn tính chất lệch trái thì đổi chỗ hai con
      t[x].d = t[t[x].rs].d + 1;  // Cập nhật dist
      return x;
    }
    ```

Do tính chất lệch trái, mỗi khi đệ quy xuống một tầng, $\mathrm{dist}$ của gốc một trong hai heap sẽ giảm đi $1$. Mà một cây nhị phân có $n$ nút thì $\mathrm{dist}$ của gốc không vượt quá $\left\lceil\log (n+1)\right\rceil$, nên độ phức tạp để hợp nhất hai heap có kích thước lần lượt là $n$ và $m$ là $O(\log n+\log m)$.

???+ note "Chứng minh về tính chất của $\mathrm{dist}$"
    Một cây nhị phân có gốc với $\mathrm{dist}$ bằng $x$ sẽ có ít nhất $x-1$ tầng là cây nhị phân đầy đủ (full binary tree), do đó nó có ít nhất $2^x-1$ nút. Lưu ý rằng tính chất này đúng với mọi cây nhị phân, không chỉ riêng Cây lệch trái.

Cây lệch trái còn có một cách cài đặt không cần đổi chỗ hai con: coi con có $\mathrm{dist}$ lớn hơn là con trái, con có $\mathrm{dist}$ nhỏ hơn là con phải:

???+ note "Hiện thực"
    ```cpp
    int& rs(int x) { return t[x].ch[t[t[x].ch[1]].d < t[t[x].ch[0]].d]; }
    
    int merge(int x, int y) {
      if (!x || !y) return x | y;
      if (t[x].val < t[y].val) swap(x, y);
      int& rs_ref = rs(x);
      rs_ref = merge(rs_ref, y);
      t[x].d = t[rs(x)].d + 1;
      return x;
    }
    ```

## Các thao tác khác trên Cây lệch trái

### Chèn nút

Một nút đơn lẻ cũng có thể được xem là một heap, ta chỉ cần hợp nhất nó vào heap chính.

### Xóa gốc

Hợp nhất con trái và con phải của gốc.

### Xóa một nút bất kỳ

#### Cách làm

Đầu tiên hợp nhất con trái và con phải của nút cần xóa, sau đó cập nhật $\mathrm{dist}$ từ dưới lên trên (`pushup`). Nếu không thỏa mãn tính chất lệch trái thì đổi chỗ hai con. Khi $\mathrm{dist}$ không thay đổi nữa thì dừng đệ quy:

???+ note "Hiện thực"
    ```cpp
    int& rs(int x) { return t[x].ch[t[t[x].ch[1]].d < t[t[x].ch[0]].d]; }
    
    // Với hàm pushup, ta có thể xóa nút và duy trì tính chất lệch trái bằng cách merge hai con
    int merge(int x, int y) {
      if (!x || !y) return x | y;
      if (t[x].val < t[y].val) swap(x, y);
      int& rs_ref = rs(x);
      rs_ref = merge(rs_ref, y);
      t[rs_ref].fa = x;
      t[x].d = t[rs(x)].d + 1;
      return x;
    }
    
    void pushup(int x) {
      if (!x) return;
      if (t[x].d != t[rs(x)].d + 1) {
        t[x].d = t[rs(x)].d + 1;
        pushup(t[x].fa);
      }
    }
    
    void erase(int x) {
      int y = merge(t[x].ch[0], t[x].ch[1]);
      t[y].fa = t[x].fa;
      if (t[t[x].fa].ch[0] == x)
        t[t[x].fa].ch[0] = y;
      else if (t[t[x].fa].ch[1] == x)
        t[t[x].fa].ch[1] = y;
      pushup(t[y].fa);
    }
    ```

#### Chứng minh độ phức tạp

Đầu tiên xét quá trình `merge`, mỗi bước đều đi xuống một tầng của $x$ hoặc $y$. Trường hợp tệ nhất là luôn chọn đi về phía con phải của Cây lệch trái (nút có $\mathrm{dist}$ nhỏ nhất), khi đó $\mathrm{dist}$ giảm đi $1$.

Tiếp theo xét quá trình `pushup`. Gọi nút hiện tại đang `pushup` là $x$, cha của nó là $y$. "$\mathrm{dist}$ ban đầu" của một nút là $\mathrm{dist}$ của nó trước khi thực hiện `pushup`. Bắt đầu đệ quy từ cha của nút bị xóa, có hai trường hợp:

1.  $x$ là con phải của $y$. Khi đó $\mathrm{dist}$ ban đầu của $y$ bằng $\mathrm{dist}$ ban đầu của $x$ cộng $1$.
2.  $x$ là con trái của $y$. Do $\mathrm{dist}$ của một nút chỉ giảm tối đa $1$, nên đệ quy chỉ tiếp tục khi $\mathrm{dist}$ ban đầu của con trái và con phải của $y$ bằng nhau (khi đó việc con trái giảm $\mathrm{dist}$ sẽ dẫn đến hoán đổi hai con hoặc cập nhật lại $\mathrm{dist}$ của $y$). Do đó, $\mathrm{dist}$ ban đầu của $y$ vẫn bằng $\mathrm{dist}$ ban đầu của $x$ cộng $1$.

Như vậy, mỗi khi đệ quy lên một tầng, $\mathrm{dist}$ ban đầu của $x$ sẽ tăng thêm $1$. Do đó số tầng đệ quy tối đa là $O(\log n)$.

### Cộng/Trừ một giá trị, Nhân một số dương cho cả heap

Thực tế, mọi thao tác có thể dùng lazy tag (đánh dấu) và không làm thay đổi thứ tự tương đối giữa các phần tử đều có thể thực hiện được.

Ta đánh dấu (tag) tại gốc, và đẩy nhãn (pushdown) khi xóa gốc hoặc hợp nhất (truy cập vào con):

???+ note "Hiện thực"
    ```cpp
    int merge(int x, int y) {
      if (!x || !y) return x | y;
      if (t[x].val > t[y].val) swap(x, y);
      pushdown(x);
      t[x].rs = merge(t[x].rs, y);
      if (t[t[x].rs].d > t[t[x].ls].d) swap(t[x].ls, t[x].rs);
      t[x].d = t[t[x].rs].d + 1;
      return x;
    }
    
    int pop(int x) {
      pushdown(x);
      return merge(t[x].ls, t[x].rs);
    }
    ```

## Các loại Heap hợp nhất được khác

### Heap ngẫu nhiên (Randomized Heap)

???+ note "Hiện thực"
    ```cpp
    int merge(int x, int y) {
      if (!x || !y) return x | y;
      if (t[y].val < t[x].val) swap(x, y);
      if (rand() & 1)  // Ngẫu nhiên chọn có đổi chỗ hai con hay không
        swap(t[x].ls, t[x].rs);
      t[x].ls = merge(t[x].ls, y);
      return x;
    }
    ```

Có thể thấy điểm khác biệt duy nhất của phương pháp này là sử dụng số ngẫu nhiên để quyết định việc hợp nhất, nhờ đó có thể bỏ qua việc tính toán $\mathrm{dist}$. Độ phức tạp thời gian trung bình vẫn là $O(\log n)$. Chứng minh chi tiết có thể tham khảo [Randomized Heap](https://cp-algorithms.com/data_structures/randomized_heap.html).

### Heap nghiêng (Skew Heap)

Skew Heap là dạng tự điều chỉnh của Cây lệch trái. Khi hợp nhất hai heap, nó vô điều kiện đổi chỗ tất cả các nút trên đường đi hợp nhất, nhằm mục đích duy trì sự cân bằng. Theo phân tích khấu hao (amortized analysis), độ phức tạp của các thao tác chèn, hợp nhất, xóa giá trị nhỏ nhất trên Skew Heap (top-down) là $O(\log n)$[^ref1].

## Bài tập ví dụ

### Bài tập mẫu

[luogu P3377【Template】Leftist Tree (Mergeable Heap)](https://www.luogu.com.cn/problem/P3377)

[Monkey King](https://www.luogu.com.cn/problem/P1456)

[Roman Game](https://www.luogu.com.cn/problem/P2713)

Cần lưu ý:

1.  Trước khi hợp nhất phải kiểm tra xem hai nút đã nằm trong cùng một heap chưa.

2.  Độ sâu của Cây lệch trái có thể lên tới $O(n)$, do đó để tìm gốc của heap chứa một nút, ta cần dùng DSU (Cấu trúc dữ liệu các tập hợp không giao nhau), không thể nhảy cha (brute force) trực tiếp. (Mặc dù nhiều bài có dữ liệu yếu nên nhảy cha vẫn qua được...) (Khi dùng DSU để bảo trì gốc, cần đảm bảo gốc cũ trỏ vào gốc mới, và gốc mới trỏ vào chính nó.)

??? note "Code tham khảo bài Roman Game"
    ```cpp
    --8<-- "docs/ds/code/leftist-tree/leftist-tree_1.cpp"
    ```

### Bài toán trên cây

[「APIO2012」Dispatching](https://www.luogu.com.cn/problem/P1552)

[「JLOI2015」City Capture](https://loj.ac/problem/2107)

Loại bài này thường yêu cầu mỗi nút bảo trì một heap, hợp nhất với heap của các con, sau đó thực hiện pop, sửa đổi, tính toán đáp án theo đề bài, hơi giống với các bài dùng Segment Tree Merge (Hợp nhất cây phân đoạn).

??? note "Code tham khảo bài City Capture"
    ```cpp
    --8<-- "docs/ds/code/leftist-tree/leftist-tree_2.cpp"
    ```

### [「SCOI2011」Tricky Operations](https://loj.ac/problem/2441)

Đầu tiên, việc tìm gốc của heap chứa một nút phải dùng DSU, không thể nhảy cha trực tiếp.

Tiếp theo xét thao tác truy vấn đơn điểm. Nếu dùng cách đánh dấu (lazy tag) thông thường, ta phải tính tổng các tag trên đường đi từ nút đó đến gốc, trong trường hợp xấu nhất độ phức tạp có thể lên tới $O(n)$. Nếu chỉ có tag ở gốc heap thì có thể truy vấn nhanh, nhưng làm sao để thực hiện điều này?

Ta có thể dùng phương pháp tương tự như **hợp nhất theo quy tắc nhỏ-gộp-lớn (small-to-large merging)**. Mỗi khi hợp nhất, ta đẩy tag (pushdown) một cách thủ công (brute force) xuống từng nút của heap nhỏ hơn, sau đó lấy tag của heap lớn hơn làm tag chung cho heap sau khi hợp nhất. Vì heap sau khi hợp nhất có chứa tag của heap lớn, nên khi đẩy tag cho heap nhỏ, ta phải trừ đi tag của heap lớn. Do mỗi lần hợp nhất, kích thước của heap chứa một nút bất kỳ sẽ tăng ít nhất gấp đôi, nên mỗi nút chỉ bị đẩy tag thủ công tối đa $O(\log n)$ lần. Tổng độ phức tạp của việc đẩy tag thủ công là $O(n\log n)$.

Đối với thao tác cộng đơn điểm: xóa nút, cập nhật giá trị, sau đó chèn lại.

Cuối cùng là thao tác tìm giá trị lớn nhất toàn cục. Ta có thể dùng một cây cân bằng / heap hỗ trợ xóa nút bất kỳ (như Cây lệch trái) / `multiset` để bảo trì giá trị lớn nhất của các heap (tức là giá trị tại gốc của mỗi heap).

Tóm lại, các thao tác được xử lý như sau:

1.  Đẩy tag thủ công cho heap nhỏ hơn, hợp nhất hai heap, cập nhật size, tag. Trong `multiset`, xóa gốc cũ không còn là gốc sau khi hợp nhất.
2.  Xóa nút, cập nhật giá trị, chèn lại, cập nhật `multiset`. Cần phân trường hợp nếu nút bị xóa là gốc.
3.  Đánh tag tại gốc heap, cập nhật `multiset`.
4.  Đánh tag toàn cục.
5.  Truy vấn giá trị + tag tại gốc heap + tag toàn cục.
6.  Truy vấn giá trị tại gốc + tag tại gốc heap + tag toàn cục.
7.  Truy vấn giá trị lớn nhất trong `multiset` + tag toàn cục.

??? note "Code tham khảo bài Tricky Operations"
    ```cpp
    --8<-- "docs/ds/code/leftist-tree/leftist-tree_3.cpp"
    ```

### [「BOI2004」Sequence](https://www.luogu.com.cn/problem/P4331)

Đây là một bài toán từ bài báo khoa học, chi tiết xem tại [《Hoàng Nguyên Hà -- Đặc điểm và ứng dụng của Cây lệch trái》](https://github.com/OI-wiki/libs/blob/master/%E9%9B%86%E8%AE%AD%E9%98%9F%E5%8E%86%E5%B9%B4%E8%AE%BA%E6%96%87/%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F2005%E8%AE%BA%E6%96%87%E9%9B%86/%E9%BB%84%E6%BA%90%E6%B2%B3--%E5%B7%A6%E5%81%8F%E6%A0%91%E7%9A%84%E7%89%B9%E7%82%B9%E5%8F%8A%E5%85%B6%E5%BA%94%E7%94%A8/%E9%BB%84%E6%BA%90%E6%B2%B3.pdf).

## Tài liệu tham khảo

[^ref1]: [Self-Adjusting Heaps](https://epubs.siam.org/doi/10.1137/0215004)