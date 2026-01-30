## Treap không xoay bền vững (Persistent Non-rotating Treap)

### Kiến thức tiên quyết

**Cây cân bằng bền vững thường được sử dụng trong lập trình thi đấu** là **Treap không xoay bền vững**, vì vậy trước tiên bạn nên tìm hiểu về [**Treap không xoay (Non-rotating Treap)**](./treap.md).

### Ý tưởng/Phương pháp

Đối với Treap không xoay, tính bền vững có thể đạt được bằng cách sao chép các nút trên đường đi trong quá trình thực hiện các thao tác **Merge** và **Split** (thường sao chép trong thao tác **Split** để đảm bảo không ảnh hưởng đến các phiên bản trước đó).

Đối với Treap có xoay, ngoài việc sao chép các nút trên đường đi, còn cần sao chép các nút bị ảnh hưởng bởi thao tác xoay (nếu nút đó đã được sao chép trong thao tác này thì không cần sao chép lại). Vì một lần xoay thường chỉ ảnh hưởng đến hai nút, nên độ phức tạp thời gian không tăng thêm.

Phương pháp trên thường được gọi là sao chép đường đi (path copying).

"Mọi thao tác được hỗ trợ đều có thể hoàn thành thông qua **Merge, Split, Newnode, Build**". Trong đó, thao tác **Build** chỉ dùng để xây dựng cây ban đầu nên không cần quan tâm nhiều, **Newnode** (tạo nút mới) chính là công cụ dùng để thực hiện tính bền vững.

Hãy quan sát **Merge** và **Split**, ta sẽ thấy chúng đều là các thao tác từ trên xuống dưới!

Do đó, ta hoàn toàn có thể **tham khảo thao tác bền vững của cây phân đoạn (Segment Tree)** để thực hiện tính bền vững cho nó.

### Thao tác bền vững

**Tính bền vững** (Persistence) là một thao tác trên **cấu trúc dữ liệu**, tức là giữ lại thông tin lịch sử để có thể gọi lại các phiên bản trước đó sau này.

Đối với **Cây phân đoạn bền vững**, mỗi lần tạo phiên bản lịch sử mới là sao chép **đường đi sửa đổi**.

Vậy đối với Treap bền vững (phiên bản thường dùng trong các cuộc thi lập trình hiện nay):

Khi sao chép một phiên bản mới $X_{a+1}$ (phiên bản thứ $a+1$ của nút $X$) từ nút $X_{a}$ (phiên bản thứ $a$ của nút $X$):

-   Nếu một nút con $Y$ nào đó không cần sửa đổi thông tin, thì chỉ cần trỏ con trỏ của $X_{a+1}$ trực tiếp đến $Y_{a}$ (phiên bản thứ $a$ của nút $Y$).
-   Ngược lại, nếu cần sửa đổi $Y$, thì khi **đệ quy xuống tầng dưới**, ta **tạo mới** nút $Y_{a+1}$ (phiên bản thứ $a+1$ của nút $Y$) để **lưu trữ thông tin mới**, đồng thời trỏ con trỏ của $X_{a+1}$ đến $Y_{a+1}$ (phiên bản thứ $a+1$ của nút $Y$).

### Cài đặt bền vững

Những thứ cần thiết:

-   Một mảng `struct` để lưu thông tin của **mỗi nút** (thường gọi là mảng `tree`); (tất nhiên những cao thủ viết cây cân bằng bằng **con trỏ** có thể không cần mảng này)

-   Một **mảng gốc**, lưu *gốc* của mỗi phiên bản, mỗi lần truy vấn thông tin phiên bản thì bắt đầu từ **nút được lưu trong mảng gốc**;

-   `split()` tách **một cây thành hai cây**

-   `merge()` hợp nhất **hai cây theo trọng số ngẫu nhiên**

-   `newNode()` tạo một nút mới

-   `build()` xây dựng cây

#### Split

Đối với **thao tác tách**, mỗi lần tách đường đi, **tạo nút mới** trỏ đến đường đi được tách ra, dùng `std::pair` để lưu gốc của hai cây mới được tách ra.

`split(x,k)` trả về một `std::pair`;

Biểu thị việc đặt $k$ phần tử đầu tiên của cây có gốc là $_x$ vào **một cây**, các nút còn lại tạo thành một cây khác, trả về gốc của hai cây này (first là gốc của cây thứ nhất, second là gốc của cây thứ hai).

-   Nếu $key$ của **cây con trái** của $x$ $\geq k$, thì **trực tiếp đệ quy vào cây con trái**, hợp nhất cây thứ hai được tách ra từ cây con trái với **cây con phải** hiện tại của $x$.
-   Ngược lại đệ quy **cây con phải**.

```cpp
static std::pair<int, int> _split(int _x, int k) {
  if (_x == 0)
    return std::make_pair(0, 0);
  else {
    int _vs = ++_cnt;  // Tạo nút mới (tinh túy của tính bền vững)
    _trp[_vs] = _trp[_x];
    std::pair<int, int> _y;
    if (_trp[_vs].key <= k) {
      _y = _split(_trp[_vs].leaf[1], k);
      _trp[_vs].leaf[1] = _y.first;
      _y.first = _vs;
    } else {
      _y = _split(_trp[_vs].leaf[0], k);
      _trp[_vs].leaf[0] = _y.second;
      _y.second = _vs;
    }
    _trp[_vs]._update();
    return _y;
  }
}
```

#### Merge

`merge(x,y)` trả về gốc của cây sau khi hợp nhất.

Cũng thực hiện đệ quy. Nếu **trọng số ngẫu nhiên của x** > **trọng số ngẫu nhiên của y**, thì `merge(x_{rc},y)`, ngược lại `merge(x,y_{lc})`.

```cpp
static int _merge(int _x, int _y) {
  if (_x == 0 || _y == 0)
    return _x ^ _y;
  else {
    if (_trp[_x].fix < _trp[_y].fix) {
      _trp[_x].leaf[1] = _merge(_trp[_x].leaf[1], _y);
      _trp[_x]._update();
      return _x;
    } else {
      _trp[_y].leaf[0] = _merge(_x, _trp[_y].leaf[0]);
      _trp[_y]._update();
      return _y;
    }
  }
}
```

## WBLT Bền vững

### Kiến thức tiên quyết

WBLT bền vững được cải tiến từ WBLT, vì vậy trước tiên hãy tìm hiểu về [WBLT](./wblt.md).

### Ý tưởng/Phương pháp

Sử dụng phương pháp **sao chép đường đi**, sao chép các nút đã **bị sửa đổi** trong một thao tác, không được ảnh hưởng đến các nút trước đó.

### Xử lý Lazy Tag (Nhãn lười)

Để xử lý lazy tag, ta xem xét như sau: trên một cây WBLT bền vững, một điểm có thể có nhiều cha, nhưng số lượng con chỉ có thể là $0$ hoặc $2$. Thao tác đẩy lazy tag xuống (pushdown) chỉ ảnh hưởng đến con của nó. Việc pushdown một điểm không có ảnh hưởng gì đến điểm đó; ngược lại là con của nó, con của nó có thể có nhiều hơn một cha, việc đẩy nhãn xuống con có thể dẫn đến việc trên phiên bản của một người cha khác, xuất hiện thêm một lazy tag không thuộc về phiên bản đó, điều này là sai; trừ khi con của nó chỉ có một người cha là nó. Vì vậy ta nên sao chép một bản sao của con khi pushdown, và đánh lazy tag lên con mới.

### Thực hiện sao chép đường đi

Khi thực hiện sao chép đường đi, ta có thể định nghĩa một hàm refresh, hàm này nhận tham chiếu đến một nút $p$, biểu thị việc sao chép nút $p$, tạo ra một nút mới và gán lại cho $p$. Nguyên tắc sử dụng hàm refresh là, nếu nó sắp bị sửa đổi, hoặc con của nó sắp thay đổi (chứ không phải thông tin của con nó sắp bị sửa đổi), thì refresh nó, ngược lại không cần.

Đối với truy vấn tĩnh, ngoại trừ pushdown thì không cần refresh. Nếu đảm bảo mọi thao tác đều thực hiện sao chép đường đi, thì thứ tự của pushdown và refresh là không quan trọng.

### Tối ưu hóa nhỏ cho WBLT bền vững

Có một tối ưu hóa ở đây. Quan sát thấy khi pushdown cần sao chép hai nút, có thể viết đánh dấu vĩnh viễn (mark permanentization), nhưng như đã nói ở trên, nếu con của nó chỉ có một người cha là nó, thì không cần sao chép. Dựa trên tính chất này, có thể tối ưu hóa để giảm việc sao chép các nút dư thừa.

Xem xét việc ghi lại số lượng cha của mỗi nút (coi mỗi phiên bản gốc đều có một cha), ký hiệu là $use$. Mỗi lần refresh, nếu $use\leq 1$ thì không cần sao chép lại nút, ngược lại tạo nút mới, và $use$ tự giảm $1$, biểu thị cha đã mang con này đi, như vậy cha có thể tùy ý sửa đổi nút mới mà không ảnh hưởng đến các phiên bản khác. Ngoài ra mỗi lần sao chép nút, nếu nút có con, thì $use$ của hai con tự tăng $1$; khi hợp nhất hai cây con, nút được trả về cũng có một cha $use$ đối với hai con; khi xóa nút, cả hai nút con đều mất một cha: như vậy có thể tối ưu hóa một chút về thời gian và không gian.

### Cài đặt

??? note "Code đầy đủ (Cây cân bằng nghệ thuật bền vững)"
    ```cpp
    --8<-- "docs/ds/code/persistent-balanced/persistent-wblt.cpp"
    ```

## Bài tập ví dụ

???+ note "[Luogu P3835【Template】Persistent Balanced Tree](https://www.luogu.com.cn/problem/P3835)"
    Bạn cần cài đặt một cấu trúc dữ liệu, yêu cầu cung cấp các thao tác sau (ban đầu cấu trúc dữ liệu không có dữ liệu):
    
    1.  Chèn số $x$;
    2.  Xóa số $x$ (nếu có nhiều số giống nhau, chỉ xóa một số, nếu không có vui lòng bỏ qua thao tác này);
    3.  Truy vấn thứ hạng của số $x$ (thứ hạng được định nghĩa là số lượng số nhỏ hơn số hiện tại + 1);
    4.  Truy vấn số có thứ hạng là $x$;
    5.  Tìm tiền thân của $x$ (tiền thân được định nghĩa là số lớn nhất nhỏ hơn $x$, nếu không tồn tại in ra $-2\,147\,483\,647$);
    6.  Tìm hậu duệ của $x$ (hậu duệ được định nghĩa là số nhỏ nhất lớn hơn $x$, nếu không tồn tại in ra $2\,147\,483\,647$).
    
    Các thao tác trên đều dựa trên một phiên bản lịch sử nào đó, đồng thời tạo ra một phiên bản mới (thao tác 3, 4, 5, 6 giữ nguyên phiên bản gốc không thay đổi). Và số hiệu của mỗi phiên bản là số thứ tự của thao tác. Đặc biệt, phiên bản ban đầu có số hiệu là 0.

Đây chính là phiên bản bền vững của bài **Cây cân bằng thông thường**, các thao tác tương tự như bài đó.

Chỉ là sử dụng các thao tác merge và split bền vững.

## Bài tập luyện tập được đề xuất

1.  [「Luogu P3919」Mảng bền vững (Bài mẫu)](https://www.luogu.com.cn/problem/P3919)

2.  [「Codeforces 702F」T-shirt](http://codeforces.com/problemset/problem/702/F)

3.  [「Luogu P5055」Cây cân bằng nghệ thuật bền vững](https://www.luogu.com.cn/problem/P5055)

4.  [「Luogu P5350」Dãy số](https://www.luogu.com.cn/problem/P5350)