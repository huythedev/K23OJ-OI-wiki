author: hsfzLZH1, Ir1d, JosephusW

## Giới thiệu

Link/Cut Tree là một cấu trúc dữ liệu dùng để giải quyết **các bài toán về cây động (dynamic tree)**.

Link/Cut Tree còn được gọi là Link-Cut Tree, viết tắt là LCT. Tuy nhiên, nó không được gọi là "Cây động" (Dynamic Tree), vì "Cây động" dùng để chỉ một lớp các bài toán.

Splay Tree là nền tảng của LCT, nhưng Splay Tree được sử dụng trong LCT có một số chi tiết khác biệt so với Splay thông thường (đã được mở rộng thêm).

## Giới thiệu vấn đề

Cần bảo trì một cái cây, hỗ trợ các thao tác sau:

-   Sửa đổi trọng số đường đi giữa hai điểm.
-   Truy vấn tổng trọng số đường đi giữa hai điểm.
-   Sửa đổi trọng số cây con của một điểm.
-   Truy vấn tổng trọng số cây con của một điểm.

Đây là một bài toán mẫu về Phân rã Heavy-Light (HLD - Heavy-Light Decomposition).

Tuy nhiên, nếu thêm một thao tác nữa:

-   Cắt và nối một số cạnh, đảm bảo đồ thị vẫn là một cây.

Yêu cầu đưa ra câu trả lời trực tuyến (online).

Lúc này bài toán trở thành bài toán cây động, và có thể sử dụng LCT để giải quyết.

## Bài toán Cây động

Bảo trì một **rừng (forest)**, hỗ trợ xóa một cạnh nào đó, thêm một cạnh nào đó, và đảm bảo sau khi thêm/xóa cạnh thì đồ thị vẫn là một rừng. Chúng ta cần bảo trì một số thông tin của rừng này.

Các thao tác thường gặp bao gồm: kiểm tra tính liên thông giữa hai điểm, tổng trọng số đường đi giữa hai điểm, nối hai điểm, cắt một cạnh nào đó, sửa đổi thông tin, v.v.

### Nhìn lại Phân rã Heavy-Light (HLD) từ góc độ LCT

-   Phân rã toàn bộ cây dựa trên kích thước cây con và đánh lại nhãn.
-   Chúng ta thấy rằng sau khi đánh lại nhãn, cây hình thành các khoảng liên tiếp dưới dạng chuỗi (chain), và có thể sử dụng Segment Tree để thao tác trên đoạn.

### Chuyển sang bài toán Cây động

Chúng ta thấy rằng HLD vừa đề cập sử dụng kích thước cây con làm điều kiện phân chia. Vậy liệu ta có thể định nghĩa lại một cách phân chia khác để phù hợp hơn với bài toán cây động không?

Hãy xem xét bài toán cây động cần loại chuỗi (chain) nào.

Vì ta bảo trì động một khu rừng, hiển nhiên ta mong muốn chuỗi này là chuỗi do ta chỉ định, để thuận tiện cho việc giải quyết bài toán.

## Phân rã chuỗi đặc (Solid Chain Decomposition)

Đối với các cạnh nối từ một điểm đến tất cả các con của nó, chúng ta tự chọn một cạnh để phân rã, ta gọi cạnh được chọn là **cạnh đặc** (solid edge), các cạnh khác là **cạnh ảo** (virtual edge). Đối với cạnh đặc, ta gọi nút con được kết nối là **con đặc** (solid child). Một chuỗi bao gồm các cạnh đặc được gọi là **chuỗi đặc** (solid chain). Hãy nhớ lý do quan trọng nhất khiến ta chọn phân rã chuỗi đặc: nó do ta lựa chọn, linh hoạt và có thể thay đổi. Chính vì tính linh hoạt này, ta sử dụng Splay Tree để bảo trì các chuỗi đặc này.

## LCT

Chúng ta có thể hiểu đơn giản LCT là việc sử dụng các cây Splay để bảo trì việc phân rã chuỗi cây một cách động, nhằm thực hiện các thao tác trên đoạn trên cây động. Với mỗi chuỗi đặc, ta xây dựng một Splay để bảo trì thông tin của cả chuỗi đó.

## Cây phụ trợ (Auxiliary Tree)

Trước tiên, hãy xem xét một số tính chất của cây phụ trợ, sau đó tìm hiểu cấu trúc cụ thể của nó qua một hình ảnh minh họa.

Trong bài viết này, bạn có thể coi một số Splay tạo thành một cây phụ trợ, mỗi cây phụ trợ bảo trì một cây trong rừng, và các cây phụ trợ tạo thành LCT bảo trì toàn bộ rừng.

1.  Cây phụ trợ bao gồm nhiều cây Splay, mỗi Splay bảo trì một đường đi trong cây gốc. Thứ tự duyệt trung tố (in-order traversal) của Splay này tạo ra dãy các điểm tương ứng với một đường đi "từ trên xuống dưới" trong cây gốc.
2.  Mỗi nút trong cây gốc tương ứng một-một với một nút Splay trong cây phụ trợ.
3.  Các cây Splay trong cây phụ trợ không độc lập với nhau. Nút cha của gốc mỗi cây Splay lẽ ra là rỗng, nhưng trong LCT, nút cha của gốc mỗi cây Splay trỏ đến nút cha của **chuỗi này** trong cây gốc (tức là cha của điểm nằm cao nhất trong chuỗi). Loại liên kết cha này khác với liên kết cha trong Splay thông thường ở chỗ con nhận cha nhưng cha không nhận con (Path Parent Pointer), tương ứng với một **cạnh ảo** trong cây gốc. Do đó, mỗi thành phần liên thông có đúng một điểm mà nút cha là rỗng.
4.  Nhờ các tính chất trên của cây phụ trợ, ta không cần bảo trì cây gốc cho bất kỳ thao tác nào. Từ cây phụ trợ, trong mọi tình huống ta đều có thể khôi phục ra một cây gốc duy nhất, nên ta chỉ cần bảo trì cây phụ trợ là đủ.

Bây giờ chúng ta có một cây gốc như hình vẽ. (Nét đậm là cạnh đặc, nét đứt là cạnh ảo.)

![tree](images/lct-atree-1.svg)

Theo định nghĩa vừa rồi, cấu trúc của cây phụ trợ sẽ như hình dưới đây.

![auxtree](images/lct-atree-2.svg)

### Xem xét mối quan hệ cấu trúc giữa cây gốc và cây phụ trợ

-   Chuỗi đặc trong cây gốc: Trong cây phụ trợ, các nút này nằm trong cùng một cây Splay.
-   Chuỗi ảo (cạnh ảo) trong cây gốc: Trong cây phụ trợ, Father của nút con (gốc của Splay con) trỏ đến nút cha, nhưng cả hai con của nút cha đều không trỏ đến nút con đó.
-   Lưu ý: Gốc của cây gốc không nhất thiết là gốc của cây phụ trợ.
-   Con trỏ Father trong cây gốc không giống với con trỏ Father trong cây phụ trợ.
-   Cây phụ trợ có thể thay đổi gốc (Change Root) tùy ý miễn là thỏa mãn tính chất của cây phụ trợ và Splay.
-   Việc chuyển đổi giữa cạnh ảo và cạnh đặc có thể dễ dàng thực hiện trên cây phụ trợ, đây chính là hiện thực hóa việc phân rã chuỗi động.

### Khai báo biến sẽ sử dụng

-   `ch[N][2]`: Con trái và con phải.
-   `f[N]`: Con trỏ cha.
-   `sum[N]`: Tổng trọng số đường đi.
-   `val[N]`: Trọng số của điểm.
-   `tag[N]`: Nhãn đảo ngược (reverse tag).
-   `laz[N]`: Nhãn trọng số (lazy tag cho update giá trị).
-   `siz[N]`: Kích thước cây con trên cây phụ trợ.
-   Other\_Vars: Các biến khác.

### Khai báo hàm

#### Các hàm cấu trúc dữ liệu chung (theo nghĩa đen)

1.  `PushUp(x)`: Cập nhật thông tin từ con lên cha.
2.  `PushDown(x)`: Đẩy thông tin từ cha xuống con.

#### Các hàm của Splay Tree

Dưới đây là các hàm dùng trong Splay Tree, chi tiết có thể xem tại bài viết [Splay Tree](./splay.md).

1.  `Get(x)`: Xác định $x$ là con trái hay con phải của cha nó.
2.  `Splay(x)`: Xoay $x$ lên **gốc của Splay hiện tại** thông qua sự phối hợp với thao tác Rotate.
3.  `Rotate(x)`: Xoay $x$ lên một tầng.

#### Các thao tác mới

1.  `Access(x)`: Đưa tất cả các điểm từ gốc (của cây gốc) đến $x$ vào một chuỗi đặc, làm cho đường đi từ gốc đến $x$ trở thành một đường đi đặc, và nằm trong cùng một cây Splay. **Chỉ có thao tác này là bắt buộc phải cài đặt, các thao tác khác tùy thuộc vào đề bài.**
2.  `IsRoot(x)`: Kiểm tra xem $x$ có phải là gốc của cây Splay chứa nó hay không.
3.  `Update(x)`: Sau khi `Access`, đệ quy từ trên xuống dưới để `PushDown` cập nhật thông tin.
4.  `MakeRoot(x)`: Biến điểm $x$ thành gốc của cây (cây gốc/rừng hiện tại).
5.  `Link(x, y)`: Nối một cạnh giữa hai điểm $x, y$.
6.  `Cut(x, y)`: Xóa cạnh giữa hai điểm $x, y$.
7.  `Find(x)`: Tìm số hiệu nút gốc của cây chứa $x$.
8.  `Fix(x, v)`: Sửa đổi trọng số của điểm $x$ thành $v$.
9.  `Split(x, y)`: Tách đường đi giữa $x$ và $y$ ra để thực hiện thao tác trên đoạn.

### Macro định nghĩa

-   `#define ls ch[p][0]`
-   `#define rs ch[p][1]`

## Giải thích hàm

### `PushUp()`

```cpp
void PushUp(int p) {
  // bảo trì các biến khác
  siz[p] = siz[ls] + siz[rs] + 1;
}
```

### `PushDown()`

```cpp
void PushDown(int p) {
  if (tag[p] != std_tag) {
    // đẩy tag xuống
    tag[p] = std_tag;
  }
}
```

### `Splay() && Rotate()`

Ở đây `Splay()` và `Rotate()` có một chút khác biệt so với Splay Tree thông thường.

```cpp
#define Get(x) (ch[f[x]][1] == x)

void Rotate(int x) {
  int y = f[x], z = f[y], k = Get(x);
  if (!isRoot(y)) ch[z][ch[z][1] == y] = x;
  // Dòng trên phải viết trước, Splay thường không cần vì không có isRoot (sẽ nói sau)
  ch[y][k] = ch[x][!k], f[ch[x][!k]] = y;
  ch[x][!k] = y, f[y] = x, f[x] = z;
  PushUp(y), PushUp(x);
}

void Splay(int x) {
  Update(
      x);  // Sẽ thấy ngay thôi. Trước khi Splay phải PushDown các điểm trên đường dẫn xoay
  for (int fa; fa = f[x], !isRoot(x); Rotate(x)) {
    if (!isRoot(fa)) Rotate(Get(fa) == Get(x) ? fa : x);
  }
}
```

Các hàm trên có thể tham khảo thêm ở [Splay Tree](./splay.md).

Dưới đây là các hàm đặc thù của LCT.

### `isRoot()`

```cpp
// Như đã nói ở trước, LCT có tính chất: nếu một con không phải là con đặc, cha của nó sẽ không nhận nó.
// Vì vậy khi một điểm vừa không phải con trái, vừa không phải con phải của cha nó, thì nó chính là gốc của Splay hiện tại.
#define isRoot(x) (ch[f[x]][0] != x && ch[f[x]][1] != x)
```

### `Access()`

```cpp
// Access là thao tác cốt lõi của LCT.
// Giả sử ta muốn truy vấn một đường đi, và đường đi đó tình cờ nằm trọn trong một cây Splay hiện tại,
// ta có thể trực tiếp lấy thông tin. Hãy xem code trước rồi kết hợp hình ảnh để hiểu quá trình.
int Access(int x) {
  int p;
  for (p = 0; x; p = x, x = f[x]) {
    Splay(x), ch[x][1] = p, PushUp(x);
  }
  return p;
}
```

-   Giả sử ta có một cái cây như sau, nét liền là cạnh đặc, nét đứt là cạnh ảo.

    ![initial tree](images/lct-access-1.svg)

-   Cây phụ trợ của nó có thể trông như thế này (cách xây dựng khác nhau có thể dẫn đến cấu trúc LCT khác nhau).

    ![initial auxtree](images/lct-access-2.svg)

-   Bây giờ ta muốn `Access(N)`, biến tất cả các cạnh trên đường đi từ $A$ đến $N$ thành cạnh đặc, tạo thành một cây Splay.

    ![access tree](images/lct-access-3.svg)

-   Phương pháp là cập nhật Splay từng bước từ dưới lên trên.

-   Đầu tiên, ta xoay $N$ lên gốc của Splay hiện tại.

-   Để đảm bảo tính chất của AuxTree (cây phụ trợ), cạnh đặc từ $N$ đến $O$ ban đầu phải chuyển thành cạnh ảo.

-   Do tính chất cha nhận con nhưng con không nhận cha (đối với cạnh ảo), ta có thể đơn phương đặt con của $N$ thành `NULL`.

-   Do đó AuxTree ban đầu sẽ thay đổi từ hình dưới sang hình tiếp theo.

    ![step 1 auxtree](images/lct-access-4.svg)

-   Bước tiếp theo, ta xoay cha của $N$ là $I$ lên gốc của cây Splay chứa $I$.

-   Cạnh đặc cũ $I$—$K$ cần bị loại bỏ. Lúc này ta đặt con phải của $I$ trỏ đến $N$, ta sẽ có một cây Splay $I$—$L$.

    ![step 2 auxtree](images/lct-access-5.svg)

-   Tiếp theo, theo các bước tương tự, vì cha của $I$ trỏ đến $H$, ta xoay $H$ lên gốc của Splay Tree chứa nó, sau đó đặt con phải (rs) của $H$ là $I$.

-   Cây sau đó sẽ như thế này.

    ![step 3 auxtree](images/lct-access-6.svg)

-   Tương tự, ta `Splay(A)`, và đặt con phải của $A$ trỏ đến $H$.

-   Kết quả ta được một AuxTree như thế này. Và nhận thấy toàn bộ đường đi $A$—$N$ đã nằm trong cùng một Splay.

    ![step final auxtree](images/lct-access-7.svg)

```cpp
// Xem lại code một lần nữa
int Access(int x) {
  int p;
  for (p = 0; x; p = x, x = f[x]) {
    Splay(x), ch[x][1] = p, PushUp(x);
  }
  return p;
}
```

Chúng ta thấy `Access()` thực ra rất đơn giản, chỉ có 4 bước:

1.  Xoay nút hiện tại lên gốc (Splay).
2.  Đổi con phải thành nút trước đó (nút ở độ sâu lớn hơn trong đường đi đang xét).
3.  Cập nhật thông tin nút hiện tại.
4.  Chuyển nút hiện tại thành cha của nó, tiếp tục thao tác.

Hàm Access được cung cấp ở đây có một giá trị trả về. Giá trị này tương đương với số hiệu của nút cha của cạnh ảo trong lần biến đổi cạnh ảo/đặc cuối cùng. Giá trị này có hai ý nghĩa:

-   Khi thực hiện hai lần Access liên tiếp, giá trị trả về của lần Access thứ hai chính là LCA (Lowest Common Ancestor - Tổ tiên chung gần nhất) của hai nút đó.
-   Nó biểu thị gốc của cây Splay chứa chuỗi từ $x$ đến gốc (cây gốc). Nút này chắc chắn đã được xoay lên gốc của Splay, và cha của nó chắc chắn là rỗng.

### `Update()`

```cpp
// PushDown từ trên xuống dưới từng tầng một
void Update(int p) {
  if (!isRoot(p)) Update(f[p]);
  pushDown(p);
}
```

### `makeRoot()`

-   Tầm quan trọng của `Make_Root()` không kém gì `Access()`. Khi cần bảo trì thông tin đường đi, chắc chắn sẽ xuất hiện trường hợp độ sâu của đường đi không tăng nghiêm ngặt, theo tính chất của AuxTree, đường đi như vậy không thể xuất hiện trong cùng một Splay.
-   Lúc này ta cần dùng đến `Make_Root()`.
-   Tác dụng của `Make_Root()` là biến điểm được chỉ định thành gốc của cây gốc, hãy xem xét cách thực hiện.
-   Giả sử giá trị trả về của `Access(x)` là $y$, thì lúc này đường đi từ $x$ đến gốc hiện tại tạo thành một Splay, và gốc của Splay đó là $y$.
-   Hãy coi cây như một đồ thị có hướng, quy định chiều từ con đến cha. Dễ thấy việc đổi gốc tương đương với việc đảo ngược chiều tất cả các cạnh trên đường đi từ $x$ đến gốc (hãy suy nghĩ kỹ).
-   Do đó, chỉ cần đảo ngược (reverse) đường đi từ $x$ đến gốc hiện tại.
-   Vì $y$ là gốc của Splay đại diện cho đường đi từ $x$ đến gốc hiện tại, nên ta chỉ cần thực hiện đảo ngược khoảng (interval reversal) trên cây Splay có gốc là $y$.

```cpp
void makeRoot(int p) {
  p = Access(p);
  swap(ch[p][0], ch[p][1]);
  tag[p] ^= 1;
}
```

### `Link()`

-   Link hai điểm thực ra rất đơn giản, đầu tiên `Make_Root(x)`, sau đó trỏ cha của $x$ đến $y$. Hiển nhiên, thao tác này không thể xảy ra trong cùng một cây, nên nhớ kiểm tra trước.

```cpp
void Link(int x, int p) {
  makeRoot(x);
  splay(x);
  f[x] = p;
}
```

### `Split()`

-   Ý nghĩa của `Split` rất đơn giản, đó là trích xuất một cây Splay bảo trì đường đi từ $x$ đến $y$.
-   Đầu tiên `MakeRoot(x)`, sau đó `Access(y)`. Nếu muốn $y$ làm gốc Splay, thì `Splay(y)`.
-   Ngoài ra, ba thao tác này trong Split giúp lấy ra đường đi cần thiết vào cây con của $y$, để có thể thực hiện các thao tác khác.

### `Cut()`

-   `Cut` có hai trường hợp: đảm bảo hợp lệ và không đảm bảo hợp lệ.
-   Nếu đảm bảo hợp lệ, trực tiếp `Split(x, y)`, lúc này $y$ là gốc, $x$ chắc chắn là con của $y$, ngắt kết nối hai chiều là được. Như sau:

```cpp
void Cut(int x, int p) { makeRoot(x), Access(p), Splay(p), ls = f[x] = 0; }
```

Nếu không đảm bảo hợp lệ, ta cần kiểm tra xem có cạnh hay không. Có thể dùng `map` để lưu, nhưng có một cách dựa trên tính chất:

Muốn xóa cạnh, phải thỏa mãn ba điều kiện sau:

1.  $x, y$ liên thông.
2.  Trên đường đi giữa $x, y$ không có nút nào khác.
3.  $x$ không có con phải.

Tóm lại, ba câu trên có một ý nghĩa: Giữa $x, y$ có cạnh trực tiếp.

Việc cài đặt cụ thể để lại như một bài tập suy nghĩ cho các bạn. Kiểm tra liên thông cần dùng `Find` (sẽ nói sau), hai điểm còn lại phân tích cấu trúc một chút là biết cách kiểm tra.

### `Find()`

-   `Find()` tìm kiếm gốc của **cây gốc** chứa $x$, đừng nhầm lẫn giữa gốc cây gốc và gốc cây phụ trợ. Sau khi `Access(p)`, rồi `Splay(p)`. Lúc này gốc Splay chính là nút có độ sâu nhỏ nhất trong đường đi (tức là gốc của cây liên thông), cứ đi về phía con trái, dọc đường `PushDown` là được.
-   Đi mãi đến khi không còn con trái (`ls`), rất đơn giản.
-   Lưu ý, sau mỗi lần truy vấn cần `Splay` nút tìm được lên để đảm bảo độ phức tạp.

```cpp
int Find(int p) {
  Access(p);
  Splay(p);
  pushDown(p);
  while (ls) p = ls, pushDown(p);
  Splay(p);
  return p;
}
```

### Lưu ý

-   Trước khi thao tác, hãy suy nghĩ xem có cần `PushUp` hoặc `PushDown` không. Do LCT rất linh hoạt, thiếu một lần `Pushdown` hoặc `Pushup` có thể khiến việc sửa đổi cập nhật sai điểm!
-   `Rotate` của LCT khác với Splay thông thường, `if (z)` (kiểm tra `y` có phải là gốc không) nhất định phải đặt ở trước.
-   Thao tác `Splay` của LCT chỉ xoay lên gốc (gốc của Splay hiện tại), không có thao tác xoay đến con của ai đó, vì không cần thiết.

## Độ phức tạp thời gian

Hầu hết các thao tác trong LCT đều dựa trên `Access`, các thao tác còn lại có độ phức tạp thời gian là hằng số, do đó ta chỉ cần phân tích độ phức tạp thời gian của thao tác `Access`.

Trong đó, độ phức tạp của `Access` chủ yếu đến từ nhiều lần splay và việc truy cập các cạnh ảo trên đường đi, tiếp theo ta sẽ phân tích độ phức tạp của hai phần này.

1.  Splay

    -   Định nghĩa $w(x) = \log size(x)$, trong đó $size(x)$ là tổng số lượng cạnh ảo và cạnh đặc trong cây con gốc $x$.

    -   Định nghĩa hàm thế năng $\Phi = \sum_{x \in T} w(x)$, trong đó $T$ là tập hợp tất cả các nút.

    Từ phân tích [Độ phức tạp thời gian của Splay](./splay.md#độ-phức-tạp-thời-gian), dễ thấy độ phức tạp trung bình (amortized) của thao tác splay là $O(\log n)$.

2.  Truy cập cạnh ảo

    Tham khảo [Phân rã chuỗi nặng (Heavy path decomposition)](../graph/hld.md#heavy-path-decomposition), định nghĩa hai loại cạnh ảo:

    -   **Cạnh ảo nặng (Heavy virtual edge)**: Cạnh ảo từ nút $v$ đến cha của nó, trong đó $size(v) > \frac{1}{2} size(parent(v))$.

    -   **Cạnh ảo nhẹ (Light virtual edge)**: Cạnh ảo từ nút $v$ đến cha của nó, trong đó $size(v) \leq \frac{1}{2} size(parent(v))$.

    Đối với việc xử lý cạnh ảo, có thể dùng phân tích thế năng. Định nghĩa hàm thế năng $\Phi$ là số lượng cạnh ảo nặng. Định nghĩa chi phí trung bình $c_i = t_i + \Delta \Phi_i$, trong đó $t_i$ là chi phí thực tế, $\Delta \Phi_i$ là sự thay đổi thế năng.

    -   Khi đi qua một cạnh ảo nặng, nó sẽ chuyển thành cạnh đặc. Thao tác này làm giảm thế năng đi 1, vì nó tối ưu cấu trúc cây bằng cách tăng cường kết nối quan trọng. Và vì chi phí thực tế là $O(1)$, bù trừ với việc giảm thế năng, nên không làm tăng chi phí trung bình. Mọi chi phí trung bình tập trung vào việc xử lý cạnh ảo nhẹ.

    -   Mỗi thao tác `Access` duyệt qua tối đa $O(\log n)$ cạnh ảo nhẹ, do đó tiêu tốn tối đa $O(\log n)$ chi phí thực tế, và chuyển đổi thành $O(\log n)$ cạnh ảo nặng, tức là thế năng tăng với chi phí $O(\log n)$.

    Do đó, độ phức tạp trung bình của việc truy cập cạnh ảo là tổng của chi phí thực tế và thay đổi thế năng, tức là $O(\log n)$.

Tóm lại, độ phức tạp thời gian của thao tác `Access` trong LCT là tổng độ phức tạp của splay và truy cập cạnh ảo, nên độ phức tạp trung bình cuối cùng là $O(\log n)$. Tức là với LCT có $n$ nút, thực hiện $m$ lần `Access` có độ phức tạp $O(n \log n + m \log n)$. Từ đó, các thao tác dựa trên `Access` như `Cut`, `Link`, `Findroot` cũng có độ phức tạp trung bình là $O(\log n)$.

## Bài tập

-   [「BZOJ 3282」Tree](https://hydro.ac/p/bzoj-P3282)
-   [「HNOI2010」Bouncing Sheep](https://www.luogu.com.cn/problem/P3203)

## Bảo trì thông tin chuỗi cây (Tree Chain Information)

LCT thông qua thao tác `Split(x,y)` có thể trích xuất đường đi từ điểm $x$ đến điểm $y$ vào trong một Splay có gốc là $y$. Việc sửa đổi và thống kê thông tin chuỗi cây chuyển thành thao tác trên cây cân bằng, điều này làm cho LCT có ưu thế trong việc bảo trì thông tin chuỗi cây. Ngoài ra, việc tìm kiếm nhị phân trên chuỗi cây bằng LCT giảm được một độ phức tạp $O(\log n)$ so với HLD.

???+ note "Ví dụ [「National Training Team」Tree II](https://www.luogu.com.cn/problem/P1501)"
    Cho một cây có $n$ nút, trọng số ban đầu của mỗi điểm là $1$. Có $q$ thao tác, mỗi thao tác thuộc một trong bốn loại sau:
    
    1.  `- u1 v1 u2 v2`: Xóa cạnh giữa $u_1, v_1$, nối $u_2, v_2$, đảm bảo thao tác hợp lệ và sau khi nối vẫn là một cây.
    2.  `+ u v c`: Cộng thêm $c$ vào trọng số của tất cả các điểm trên đường đi giữa $u, v$.
    3.  `* u v c`: Nhân trọng số của tất cả các điểm trên đường đi giữa $u, v$ với $c$.
    4.  `/ u v`: In ra tổng trọng số các điểm trên đường đi giữa $u, v$ mô-đun $51061$.
    
        $1\le n,q\le 10^5,0\le c\le 10^4$
    
        Thao tác `-` có thể thực hiện trực tiếp bằng `Cut(u1,v1), Link(u2,v2)`.

Khi sửa đổi đường đi giữa $u, v$, trước hết `Split(u, v)`.

Bài này yêu cầu thực hiện cộng cây con, nhân cây con, tính tổng cây con trên cây phụ trợ (AuxTree), nên ngoài nhãn đảo ngược (reverse tag) thông thường của LCT, ta cần bảo trì thêm nhãn cộng (add tag) và nhãn nhân (mul tag). Cách xử lý nhãn giống như trên Splay.

Khi đánh dấu và đẩy nhãn cộng xuống, sự thay đổi tổng trọng số cây con liên quan đến số lượng nút trong cây con, nên ta cần bảo trì thêm kích thước cây con `siz`.

Khi đẩy nhãn xuống (pushdown), cần chú ý thứ tự: đẩy nhãn nhân trước rồi mới đến nhãn cộng. Nhãn đảo ngược và nhãn cộng/nhân không xung đột nhau.

??? note "Code tham khảo"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    constexpr long long MAXN = 100010;
    constexpr long long mod = 51061;
    long long n, q, u, v, c;
    char op;
    
    struct Splay {
      long long ch[MAXN][2], fa[MAXN], siz[MAXN], val[MAXN], sum[MAXN], rev[MAXN],
          add[MAXN], mul[MAXN];
    
      void clear(long long x) {
        ch[x][0] = ch[x][1] = fa[x] = siz[x] = val[x] = sum[x] = rev[x] = add[x] =
            0;
        mul[x] = 1;
      }
    
      long long getch(long long x) { return (ch[fa[x]][1] == x); }
    
      long long isroot(long long x) {
        clear(0);
        return ch[fa[x]][0] != x && ch[fa[x]][1] != x;
      }
    
      void maintain(long long x) {
        clear(0);
        siz[x] = (siz[ch[x][0]] + 1 + siz[ch[x][1]]) % mod;
        sum[x] = (sum[ch[x][0]] + val[x] + sum[ch[x][1]]) % mod;
      }
    
      void pushdown(long long x) {
        clear(0);
        if (mul[x] != 1) {
          if (ch[x][0])
            mul[ch[x][0]] = (mul[x] * mul[ch[x][0]]) % mod,
            val[ch[x][0]] = (val[ch[x][0]] * mul[x]) % mod,
            sum[ch[x][0]] = (sum[ch[x][0]] * mul[x]) % mod,
            add[ch[x][0]] = (add[ch[x][0]] * mul[x]) % mod;
          if (ch[x][1])
            mul[ch[x][1]] = (mul[x] * mul[ch[x][1]]) % mod,
            val[ch[x][1]] = (val[ch[x][1]] * mul[x]) % mod,
            sum[ch[x][1]] = (sum[ch[x][1]] * mul[x]) % mod,
            add[ch[x][1]] = (add[ch[x][1]] * mul[x]) % mod;
          mul[x] = 1;
        }
        if (add[x]) {
          if (ch[x][0])
            add[ch[x][0]] = (add[ch[x][0]] + add[x]) % mod,
            val[ch[x][0]] = (val[ch[x][0]] + add[x]) % mod,
            sum[ch[x][0]] = (sum[ch[x][0]] + add[x] * siz[ch[x][0]] % mod) % mod;
          if (ch[x][1])
            add[ch[x][1]] = (add[ch[x][1]] + add[x]) % mod,
            val[ch[x][1]] = (val[ch[x][1]] + add[x]) % mod,
            sum[ch[x][1]] = (sum[ch[x][1]] + add[x] * siz[ch[x][1]] % mod) % mod;
          add[x] = 0;
        }
        if (rev[x]) {
          if (ch[x][0]) rev[ch[x][0]] ^= 1, swap(ch[ch[x][0]][0], ch[ch[x][0]][1]);
          if (ch[x][1]) rev[ch[x][1]] ^= 1, swap(ch[ch[x][1]][0], ch[ch[x][1]][1]);
          rev[x] = 0;
        }
      }
    
      void update(long long x) {
        if (!isroot(x)) update(fa[x]);
        pushdown(x);
      }
    
      void print(long long x) {
        if (!x) return;
        pushdown(x);
        print(ch[x][0]);
        printf("%lld ", x);
        print(ch[x][1]);
      }
    
      void rotate(long long x) {
        long long y = fa[x], z = fa[y], chx = getch(x), chy = getch(y);
        fa[x] = z;
        if (!isroot(y)) ch[z][chy] = x;
        ch[y][chx] = ch[x][chx ^ 1];
        fa[ch[x][chx ^ 1]] = y;
        ch[x][chx ^ 1] = y;
        fa[y] = x;
        maintain(y);
        maintain(x);
        maintain(z);
      }
    
      void splay(long long x) {
        update(x);
        for (long long f = fa[x]; f = fa[x], !isroot(x); rotate(x))
          if (!isroot(f)) rotate(getch(x) == getch(f) ? f : x);
      }
    
      void access(long long x) {
        for (long long f = 0; x; f = x, x = fa[x])
          splay(x), ch[x][1] = f, maintain(x);
      }
    
      void makeroot(long long x) {
        access(x);
        splay(x);
        swap(ch[x][0], ch[x][1]);
        rev[x] ^= 1;
      }
    
      long long find(long long x) {
        access(x);
        splay(x);
        while (ch[x][0]) x = ch[x][0];
        splay(x);
        return x;
      }
    } st;
    
    main() {
      scanf("%lld%lld", &n, &q);
      for (long long i = 1; i <= n; i++) st.val[i] = 1, st.maintain(i);
      for (long long i = 1; i < n; i++) {
        scanf("%lld%lld", &u, &v);
        if (st.find(u) != st.find(v)) st.makeroot(u), st.fa[u] = v;
      }
      while (q--) {
        scanf(" %c%lld%lld", &op, &u, &v);
        if (op == '+') {
          scanf("%lld", &c);
          st.makeroot(u), st.access(v), st.splay(v);
          st.val[v] = (st.val[v] + c) % mod;
          st.sum[v] = (st.sum[v] + st.siz[v] * c % mod) % mod;
          st.add[v] = (st.add[v] + c) % mod;
        }
        if (op == '-') {
          st.makeroot(u);
          st.access(v);
          st.splay(v);
          if (st.ch[v][0] == u && !st.ch[u][1]) st.ch[v][0] = st.fa[u] = 0;
          scanf("%lld%lld", &u, &v);
          if (st.find(u) != st.find(v)) st.makeroot(u), st.fa[u] = v;
        }
        if (op == '*') {
          scanf("%lld", &c);
          st.makeroot(u), st.access(v), st.splay(v);
          st.val[v] = st.val[v] * c % mod;
          st.sum[v] = st.sum[v] * c % mod;
          st.mul[v] = st.mul[v] * c % mod;
        }
        if (op == '/')
          st.makeroot(u), st.access(v), st.splay(v), printf("%lld\n", st.sum[v]);
      }
      return 0;
    }
    ```

### Bài tập

-   [luogu P3690【Template】Link Cut Tree (Dynamic Tree)](https://www.luogu.com.cn/problem/P3690)
-   [「SDOI2011」Coloring](https://www.luogu.com.cn/problem/P2486)
-   [「SHOI2014」Trigeminal Nerve Tree](https://loj.ac/problem/2187)

## Bảo trì tính liên thông

### Kiểm tra liên thông

Dựa vào hàm `Find()` của LCT, ta có thể kiểm tra hai điểm trên rừng động có liên thông hay không. Nếu `Find(x) == Find(y)`, nghĩa là $x, y$ nằm trên cùng một cây, tức là liên thông.

???+ note "Ví dụ [「SDOI2008」Cave Exploration](https://www.luogu.com.cn/problem/P2147)"
    Ban đầu có $n$ điểm độc lập, $m$ thao tác. Mỗi thao tác là một trong các loại sau:
    
    1.  `Connect u v`: Nối cạnh giữa $u, v$.
    2.  `Destroy u v`: Xóa cạnh giữa $u, v$, đảm bảo cạnh này tồn tại.
    3.  `Query u v`: Hỏi xem $u, v$ có liên thông hay không.
    
    Đảm bảo đồ thị luôn là một rừng tại mọi thời điểm.
    
    $n\le 10^4, m\le 2\times 10^5$

??? note "Code tham khảo"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    constexpr int MAXN = 10010;
    
    struct Splay {
      int ch[MAXN][2], fa[MAXN], tag[MAXN];
    
      void clear(int x) { ch[x][0] = ch[x][1] = fa[x] = tag[x] = 0; }
    
      int getch(int x) { return ch[fa[x]][1] == x; }
    
      int isroot(int x) { return ch[fa[x]][0] != x && ch[fa[x]][1] != x; }
    
      void pushdown(int x) {
        if (tag[x]) {
          if (ch[x][0]) swap(ch[ch[x][0]][0], ch[ch[x][0]][1]), tag[ch[x][0]] ^= 1;
          if (ch[x][1]) swap(ch[ch[x][1]][0], ch[ch[x][1]][1]), tag[ch[x][1]] ^= 1;
          tag[x] = 0;
        }
      }
    
      void update(int x) {
        if (!isroot(x)) update(fa[x]);
        pushdown(x);
      }
    
      void rotate(int x) {
        int y = fa[x], z = fa[y], chx = getch(x), chy = getch(y);
        fa[x] = z;
        if (!isroot(y)) ch[z][chy] = x;
        ch[y][chx] = ch[x][chx ^ 1];
        fa[ch[x][chx ^ 1]] = y;
        ch[x][chx ^ 1] = y;
        fa[y] = x;
      }
    
      void splay(int x) {
        update(x);
        for (int f = fa[x]; f = fa[x], !isroot(x); rotate(x))
          if (!isroot(f)) rotate(getch(x) == getch(f) ? f : x);
      }
    
      void access(int x) {
        for (int f = 0; x; f = x, x = fa[x]) splay(x), ch[x][1] = f;
      }
    
      void makeroot(int x) {
        access(x);
        splay(x);
        swap(ch[x][0], ch[x][1]);
        tag[x] ^= 1;
      }
    
      int find(int x) {
        access(x);
        splay(x);
        while (ch[x][0]) x = ch[x][0];
        splay(x);
        return x;
      }
    } st;
    
    int n, q, x, y;
    char op[MAXN];
    
    int main() {
      scanf("%d%d", &n, &q);
      while (q--) {
        scanf("%s%d%d", op, &x, &y);
        if (op[0] == 'Q') {
          if (st.find(x) == st.find(y))
            printf("Yes\n");
          else
            printf("No\n");
        }
        if (op[0] == 'C')
          if (st.find(x) != st.find(y)) st.makeroot(x), st.fa[x] = y;
        if (op[0] == 'D') {
          st.makeroot(x);
          st.access(y);
          st.splay(y);
          if (st.ch[y][0] == x && !st.ch[x][1]) st.ch[y][0] = st.fa[x] = 0;
        }
      }
      return 0;
    }
    ```

### Bảo trì thành phần song liên thông cạnh (Edge-Biconnected Components)

Nếu yêu cầu co thành phần song liên thông cạnh thành một điểm, mỗi khi thêm một cạnh, nếu hai điểm được nối đã liên thông, thì tất cả các điểm trên đường đi này sẽ được co lại thành một điểm.

???+ note "Ví dụ [「AHOI2005」Route Planning](https://www.luogu.com.cn/problem/P2542)"
    Cho $n$ điểm, ban đầu có $m$ cạnh vô hướng, $q$ thao tác, mỗi thao tác là một trong các loại sau:
    
    1.  `0 u v`: Xóa cạnh giữa $u, v$, đảm bảo cạnh này tồn tại.
    2.  `1 u v`: Hỏi số lượng cạnh bắt buộc phải đi qua trên mọi đường đi có thể giữa $u, v$.
    
    Đảm bảo đồ thị luôn liên thông.
    
    $1<n<3\times 10^4,1<m<10^5,0\le q\le 4\times 10^4$

Ta có thể thấy, số lượng cạnh bắt buộc phải đi qua giữa $u, v$ chính là số lượng nút trên đường đi giữa điểm đại diện cho $u$ và điểm đại diện cho $v$ sau khi co tất cả các thành phần song liên thông cạnh thành điểm, trừ đi 1.

Do thao tác xóa cạnh trong đề bài khó thực hiện, ta cân nhắc làm ngược từ cuối lên (offline), đổi xóa cạnh thành thêm cạnh.

Khi thêm một cạnh, nếu hai điểm ban đầu không liên thông, thì nối hai điểm trên LCT; nếu không, trích xuất đường đi giữa hai điểm trên LCT trước khi thêm cạnh, duyệt cây con trên cây phụ trợ tương ứng với đường đi này, hợp nhất các điểm này, sử dụng Disjoint Set Union (DSU - Cấu trúc dữ liệu các tập hợp không giao nhau) để bảo trì thông tin hợp nhất.

Dùng phần tử đại diện của DSU sau khi hợp nhất để thay thế cho đường đi trên cây ban đầu. Lưu ý sau mỗi thao tác đều phải tìm phần tử đại diện trên DSU của điểm cần thao tác để thực hiện.

??? note "Code tham khảo"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    #include <map>
    using namespace std;
    constexpr int MAXN = 200010;
    int f[MAXN];
    
    int findp(int x) { return f[x] ? f[x] = findp(f[x]) : x; }
    
    void merge(int x, int y) {
      x = findp(x);
      y = findp(y);
      if (x != y) f[x] = y;
    }
    
    struct Splay {
      int ch[MAXN][2], fa[MAXN], tag[MAXN], siz[MAXN];
    
      void clear(int x) { ch[x][0] = ch[x][1] = fa[x] = tag[x] = siz[x] = 0; }
    
      int getch(int x) { return ch[findp(fa[x])][1] == x; }
    
      int isroot(int x) {
        return ch[findp(fa[x])][0] != x && ch[findp(fa[x])][1] != x;
      }
    
      void maintain(int x) {
        clear(0);
        if (x) siz[x] = siz[ch[x][0]] + 1 + siz[ch[x][1]];
      }
    
      void pushdown(int x) {
        if (tag[x]) {
          if (ch[x][0]) tag[ch[x][0]] ^= 1, swap(ch[ch[x][0]][0], ch[ch[x][0]][1]);
          if (ch[x][1]) tag[ch[x][1]] ^= 1, swap(ch[ch[x][1]][0], ch[ch[x][1]][1]);
          tag[x] = 0;
        }
      }
    
      void print(int x) {
        if (!x) return;
        pushdown(x);
        print(ch[x][0]);
        printf("%d ", x);
        print(ch[x][1]);
      }
    
      void update(int x) {
        if (!isroot(x)) update(findp(fa[x]));
        pushdown(x);
      }
    
      void rotate(int x) {
        x = findp(x);
        int y = findp(fa[x]), z = findp(fa[y]), chx = getch(x), chy = getch(y);
        fa[x] = z;
        if (!isroot(y)) ch[z][chy] = x;
        ch[y][chx] = ch[x][chx ^ 1];
        fa[ch[x][chx ^ 1]] = y;
        ch[x][chx ^ 1] = y;
        fa[y] = x;
        maintain(y);
        maintain(x);
        if (z) maintain(z);
      }
    
      void splay(int x) {
        x = findp(x);
        update(x);
        for (int f = findp(fa[x]); f = findp(fa[x]), !isroot(x); rotate(x))
          if (!isroot(f)) rotate(getch(x) == getch(f) ? f : x);
      }
    
      void access(int x) {
        for (int f = 0; x; f = x, x = findp(fa[x]))
          splay(x), ch[x][1] = f, maintain(x);
      }
    
      void makeroot(int x) {
        x = findp(x);
        access(x);
        splay(x);
        tag[x] ^= 1;
        swap(ch[x][0], ch[x][1]);
      }
    
      int find(int x) {
        x = findp(x);
        access(x);
        splay(x);
        while (ch[x][0]) x = ch[x][0];
        splay(x);
        return x;
      }
    
      void dfs(int x) {
        pushdown(x);
        if (ch[x][0]) dfs(ch[x][0]), merge(ch[x][0], x);
        if (ch[x][1]) dfs(ch[x][1]), merge(ch[x][1], x);
      }
    } st;
    
    int n, m, q, x, y, cur, ans[MAXN];
    
    struct oper {
      int op, a, b;
    } s[MAXN];
    
    map<pair<int, int>, int> mp;
    
    int main() {
      scanf("%d%d", &n, &m);
      for (int i = 1; i <= n; i++) st.maintain(i);
      for (int i = 1; i <= m; i++)
        scanf("%d%d", &x, &y), mp[{x, y}] = mp[{y, x}] = 1;
      while (scanf("%d", &s[++q].op)) {
        if (s[q].op == -1) {
          q--;
          break;
        }
        scanf("%d%d", &s[q].a, &s[q].b);
        if (!s[q].op) mp[{s[q].a, s[q].b}] = mp[{s[q].b, s[q].a}] = 0;
      }
      reverse(s + 1, s + q + 1);
      for (map<pair<int, int>, int>::iterator it = mp.begin(); it != mp.end(); it++)
        if (it->second) {
          mp[{it->first.second, it->first.first}] = 0;
          x = findp(it->first.first);
          y = findp(it->first.second);
          if (st.find(x) != st.find(y))
            st.makeroot(x), st.fa[x] = y;
          else {
            if (x == y) continue;
            st.makeroot(x);
            st.access(y);
            st.splay(y);
            st.dfs(y);
            int t = findp(y);
            st.fa[t] = findp(st.fa[y]);
            st.ch[t][0] = st.ch[t][1] = 0;
            st.maintain(t);
          }
        }
      for (int i = 1; i <= q; i++) {
        if (s[i].op == 0) {
          x = findp(s[i].a);
          y = findp(s[i].b);
          st.makeroot(x);
          st.access(y);
          st.splay(y);
          st.dfs(y);
          int t = findp(y);
          st.fa[t] = st.fa[y];
          st.ch[t][0] = st.ch[t][1] = 0;
          st.maintain(t);
        }
        if (s[i].op == 1) {
          x = findp(s[i].a);
          y = findp(s[i].b);
          st.makeroot(x);
          st.access(y);
          st.splay(y);
          ans[++cur] = st.siz[y] - 1;
        }
      }
      for (int i = cur; i >= 1; i--) printf("%d\n", ans[i]);
      return 0;
    }
    ```

### Bài tập

-   [Luogu P3950 Clash of Clans](https://www.luogu.com.cn/problem/P3950)
-   [BZOJ 4998 Planet Alliance](https://hydro.ac/p/bzoj-P4998)
-   [BZOJ 2959 Long Run](https://hydro.ac/p/bzoj-P2959)

## Bảo trì trọng số cạnh

LCT không thể trực tiếp xử lý trọng số cạnh, lúc này cần tạo một điểm tương ứng cho mỗi cạnh để thuận tiện cho việc truy vấn thông tin trên chuỗi. Kỹ thuật này có thể dùng để bảo trì cây khung động.

???+ note "Ví dụ [luogu P4234 Minimum Difference Spanning Tree](https://www.luogu.com.cn/problem/P4234)"
    Cho đồ thị vô hướng có trọng số gồm $n$ điểm, $m$ cạnh. Tìm cây khung sao cho hiệu giữa cạnh có trọng số lớn nhất và cạnh có trọng số nhỏ nhất là nhỏ nhất, in ra hiệu này.
    
    Dữ liệu đảm bảo tồn tại ít nhất một cây khung.
    
    $1\le n\le 5\times 10^4,1\le m\le 2\times 10^5,1\le w_i\le 10^4$

Sắp xếp các cạnh theo trọng số từ nhỏ đến lớn, duyệt qua từng cạnh chọn làm cạnh có trọng số lớn nhất (cạnh bên phải). Để có kết quả tối ưu, ta cần làm cho trọng số của cạnh nhỏ nhất trong cây khung là lớn nhất có thể.

Mỗi lần thêm cạnh theo thứ tự, nếu hai điểm cần nối đã liên thông, thì xóa cạnh có trọng số nhỏ nhất trên đường đi giữa hai điểm đó. Nếu toàn bộ đồ thị đã liên thông thành một cây, thì dùng trọng số cạnh hiện tại trừ đi trọng số cạnh nhỏ nhất để cập nhật đáp án. Trọng số cạnh nhỏ nhất có thể cập nhật bằng phương pháp hai con trỏ (two pointers).

Trên LCT không có quan hệ cha con cố định, nên không thể lưu trọng số cạnh vào trọng số điểm.

Để ghi lại thông tin cạnh trên chuỗi cây, ta có thể dùng **tách cạnh (edge splitting)**. Tạo một điểm tương ứng cho mỗi cạnh, nối từ điểm cạnh này đến hai đầu mút của nó. Các thao tác nối và xóa cạnh ban đầu sẽ biến thành hai thao tác.

??? note "Code tham khảo"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    #include <set>
    using namespace std;
    constexpr int MAXN = 5000010;
    
    struct Splay {
      int ch[MAXN][2], fa[MAXN], tag[MAXN], val[MAXN], minn[MAXN];
    
      void clear(int x) {
        ch[x][0] = ch[x][1] = fa[x] = tag[x] = val[x] = minn[x] = 0;
      }
    
      int getch(int x) { return ch[fa[x]][1] == x; }
    
      int isroot(int x) { return ch[fa[x]][0] != x && ch[fa[x]][1] != x; }
    
      void maintain(int x) {
        if (!x) return;
        minn[x] = x;
        if (ch[x][0]) {
          if (val[minn[ch[x][0]]] < val[minn[x]]) minn[x] = minn[ch[x][0]];
        }
        if (ch[x][1]) {
          if (val[minn[ch[x][1]]] < val[minn[x]]) minn[x] = minn[ch[x][1]];
        }
      }
    
      void pushdown(int x) {
        if (tag[x]) {
          if (ch[x][0]) tag[ch[x][0]] ^= 1, swap(ch[ch[x][0]][0], ch[ch[x][0]][1]);
          if (ch[x][1]) tag[ch[x][1]] ^= 1, swap(ch[ch[x][1]][0], ch[ch[x][1]][1]);
          tag[x] = 0;
        }
      }
    
      void update(int x) {
        if (!isroot(x)) update(fa[x]);
        pushdown(x);
      }
    
      void print(int x) {
        if (!x) return;
        pushdown(x);
        print(ch[x][0]);
        printf("%d ", x);
        print(ch[x][1]);
      }
    
      void rotate(int x) {
        int y = fa[x], z = fa[y], chx = getch(x), chy = getch(y);
        fa[x] = z;
        if (!isroot(y)) ch[z][chy] = x;
        ch[y][chx] = ch[x][chx ^ 1];
        fa[ch[x][chx ^ 1]] = y;
        ch[x][chx ^ 1] = y;
        fa[y] = x;
        maintain(y);
        maintain(x);
        if (z) maintain(z);
      }
    
      void splay(int x) {
        update(x);
        for (int f = fa[x]; f = fa[x], !isroot(x); rotate(x))
          if (!isroot(f)) rotate(getch(x) == getch(f) ? f : x);
      }
    
      void access(int x) {
        for (int f = 0; x; f = x, x = fa[x]) splay(x), ch[x][1] = f, maintain(x);
      }
    
      void makeroot(int x) {
        access(x);
        splay(x);
        tag[x] ^= 1;
        swap(ch[x][0], ch[x][1]);
      }
    
      int find(int x) {
        access(x);
        splay(x);
        while (ch[x][0]) x = ch[x][0];
        splay(x);
        return x;
      }
    
      void link(int x, int y) {
        makeroot(x);
        fa[x] = y;
      }
    
      void cut(int x, int y) {
        makeroot(x);
        access(y);
        splay(y);
        ch[y][0] = fa[x] = 0;
        maintain(y);
      }
    } st;
    
    constexpr int inf = 2e9 + 1;
    int n, m, ans, nww, x, y;
    
    struct Edge {
      int u, v, w;
    
      bool operator<(Edge x) const { return w < x.w; };
    } s[MAXN];
    
    multiset<int> mp;
    
    int main() {
      scanf("%d%d", &n, &m);
      for (int i = 1; i <= n; i++) st.val[i] = inf, st.maintain(i);
      for (int i = 1; i <= m; i++) scanf("%d%d%d", &s[i].u, &s[i].v, &s[i].w);
      sort(s + 1, s + m + 1);
      for (int i = 1; i <= m; i++) st.val[n + i] = s[i].w, st.maintain(n + i);
      for (int i = 1; i <= m; i++) {
        x = s[i].u;
        y = s[i].v;
        if (x == y) continue;
        if (st.find(x) != st.find(y)) {
          nww++;
          st.link(x, n + i);
          st.link(n + i, y);
          mp.insert(s[i].w);
          if (nww == n - 1) ans = s[i].w - (*(mp.begin()++));
        } else {
          st.makeroot(x);
          st.access(y);
          st.splay(y);
          int t = st.minn[y] - n;
          st.cut(s[t].u, t + n);
          st.cut(t + n, s[t].v);
          mp.erase(mp.find(s[t].w));
          st.link(x, n + i);
          st.link(n + i, y);
          mp.insert(s[i].w);
          if (nww == n - 1) ans = min(ans, s[i].w - (*(mp.begin()++)));
        }
      }
      printf("%d\n", ans);
      return 0;
    }
    ```

### Bài tập

-   [「WC2006」Water Tube Manager](https://www.luogu.com.cn/problem/P4172)
-   [「BJWC2010」Strictly Second Minimum Spanning Tree](https://www.luogu.com.cn/problem/P4180)
-   [「NOI2014」Magic Forest](https://uoj.ac/problem/3)

## Bảo trì thông tin cây con

LCT không giỏi bảo trì thông tin cây con. Tuy nhiên, bằng cách thống kê thông tin của tất cả các cây con ảo (virtual subtrees) của một nút, ta có thể tính được thông tin của cả cây.

???+ note "Ví dụ [「BJOI2014」Great Fusion](https://loj.ac/problem/2230)"
    Cho $n$ nút và $q$ thao tác, mỗi thao tác có dạng:
    
    1.  `A x y`: Nối cạnh giữa nút $x$ và $y$.
    2.  `Q x y`: Cho một cạnh đã tồn tại $(x, y)$, hỏi có bao nhiêu đường đi đơn chứa cạnh $(x, y)$.
    
    Đảm bảo tại mọi thời điểm, đồ thị là một khu rừng.
    
    $1\le n,q,x,y\le 10^5$

Đối với câu hỏi `Q`, ta có thể phát biểu lại: đáp án bằng tích số lượng nút ở phía $x$ và số lượng nút ở phía $y$ của cạnh $(x, y)$, tức là số lượng nút của hai cây chứa $x$ và $y$ sau khi cắt cạnh $(x, y)$. Để loại bỏ ảnh hưởng của việc cắt cạnh, sau khi truy vấn ta nối lại cạnh $(x, y)$.

Các thao tác trong bài vừa có nối cạnh, vừa có xóa cạnh, lại đảm bảo luôn là rừng, nên ta nghĩ ngay đến việc dùng LCT. Tuy nhiên, LCT trong bài này cần bảo trì kích thước cây con, không giống như bảo trì thông tin chuỗi mà ta thường thấy. Hơn nữa cấu trúc LCT **nhận cha không nhận con** khiến ta khó thống kê trực tiếp. Vậy phải làm sao?

Phương pháp là thống kê đóng góp của tất cả các con ảo của một nút $x$ (tức là cha của chúng là $x$, nhưng chúng không phải là con trái/phải của $x$ trong Splay).

Định nghĩa $siz2[x]$ là tổng số lượng nút của các cây con đại diện bởi các con ảo của $x$, $siz[x]$ là tổng số lượng nút trong cây con của $x$.

Khác với cách bảo trì số lượng nút cây con trong Splay thông thường, khi tính số lượng nút trong cây con của $x$, ta cần cộng thêm $siz2[x]$, tức là:

```cpp
void maintain(int x) {
  clear(0);
  if (x) siz[x] = siz[ch[x][0]] + 1 + siz[ch[x][1]] + siz2[x];
}
```

Và khi chúng ta **thay đổi hình thái của Splay** (tức là thay đổi con trỏ trái/phải của một nút trong Splay), ta cần kịp thời sửa đổi giá trị $siz2[x]$.

Trong thao tác `Rotate(), Splay()`, ta chỉ thay đổi vị trí tương đối của các nút trong Splay, không thay đổi tính chất ảo/đặc của bất kỳ cạnh nào, nên không cần sửa đổi $siz2[x]$.

Trong thao tác `access`, sau mỗi lần splay, ta sẽ thay đổi con phải của nút vừa splay xong. Tức là cạnh nối nút đó với con phải cũ và cạnh nối nút đó với con phải mới sẽ thay đổi tính chất ảo/đặc. Ta cần cộng thêm đóng góp của cây con nối bởi cạnh vừa trở thành cạnh ảo, và trừ đi đóng góp của cây con nối bởi cạnh vừa trở thành cạnh đặc. Code như sau:

```cpp
void access(int x) {
  for (int f = 0; x; f = x, x = fa[x])
    splay(x), siz2[x] += siz[ch[x][1]] - siz[f], ch[x][1] = f, maintain(x);
}
```

Trong thao tác `MakeRoot(), Find()`, ta chỉ gọi các hàm trước đó hoặc đảo ngược trên Splay, không cần sửa đổi gì thêm.

Khi nối hai điểm (`Link`), ta sửa đổi cha của một nút. Ta cần cộng thêm đóng góp kích thước cây con của nút con mới vào giá trị $siz2$ của nút cha.

```cpp
st.makeroot(x);
st.makeroot(y);
st.fa[x] = y;
st.siz2[y] += st.siz[x];
```

Khi ngắt kết nối một cạnh (`Cut`), ta chỉ xóa một cạnh đặc trên Splay, thao tác `Maintain` sẽ bảo trì các thông tin này, không cần sửa đổi gì thêm (đối với $siz2$).

Trên đây là các chi tiết sửa đổi code. Cuối cùng tổng kết lại yêu cầu và phương pháp dùng LCT bảo trì thông tin cây con:

1.  Thông tin bảo trì phải có **tính khả trừ (subtractability)**, như số lượng nút cây con, tổng trọng số cây con. Không thể trực tiếp bảo trì max/min cây con vì khi chuyển cạnh ảo thành cạnh đặc cần loại bỏ đóng góp của cạnh ảo cũ.
2.  Tạo thêm một giá trị phụ để lưu đóng góp của các cây con ảo, khi thống kê thì cộng vào đáp án của nút hiện tại, và cập nhật kịp thời khi thay đổi tính chất ảo/đặc của cạnh.
3.  Các phần còn lại giống LCT thông thường. Khi thống kê thông tin cây con, nhất định phải đảm bảo nút đó là gốc.
4.  Nếu thông tin bảo trì không có tính khả trừ (như max/min), có thể dùng một cấu trúc dữ liệu khác (như `std::multiset` hoặc một cây cân bằng khác) cho mỗi nút để bảo trì tập hợp giá trị của các con ảo.

??? note "Code tham khảo"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <cstring>
    using namespace std;
    constexpr int MAXN = 100010;
    using ll = long long;
    
    struct Splay {
      int ch[MAXN][2], fa[MAXN], siz[MAXN], siz2[MAXN], tag[MAXN];
    
      void clear(int x) {
        ch[x][0] = ch[x][1] = fa[x] = siz[x] = siz2[x] = tag[x] = 0;
      }
    
      int getch(int x) { return ch[fa[x]][1] == x; }
    
      int isroot(int x) { return ch[fa[x]][0] != x && ch[fa[x]][1] != x; }
    
      void maintain(int x) {
        clear(0);
        if (x) siz[x] = siz[ch[x][0]] + 1 + siz[ch[x][1]] + siz2[x];
      }
    
      void pushdown(int x) {
        if (tag[x]) {
          if (ch[x][0]) swap(ch[ch[x][0]][0], ch[ch[x][0]][1]), tag[ch[x][0]] ^= 1;
          if (ch[x][1]) swap(ch[ch[x][1]][0], ch[ch[x][1]][1]), tag[ch[x][1]] ^= 1;
          tag[x] = 0;
        }
      }
    
      void update(int x) {
        if (!isroot(x)) update(fa[x]);
        pushdown(x);
      }
    
      void rotate(int x) {
        int y = fa[x], z = fa[y], chx = getch(x), chy = getch(y);
        fa[x] = z;
        if (!isroot(y)) ch[z][chy] = x;
        ch[y][chx] = ch[x][chx ^ 1];
        fa[ch[x][chx ^ 1]] = y;
        ch[x][chx ^ 1] = y;
        fa[y] = x;
        maintain(y);
        maintain(x);
        maintain(z);
      }
    
      void splay(int x) {
        update(x);
        for (int f = fa[x]; f = fa[x], !isroot(x); rotate(x))
          if (!isroot(f)) rotate(getch(x) == getch(f) ? f : x);
      }
    
      void access(int x) {
        for (int f = 0; x; f = x, x = fa[x])
          splay(x), siz2[x] += siz[ch[x][1]] - siz[f], ch[x][1] = f, maintain(x);
      }
    
      void makeroot(int x) {
        access(x);
        splay(x);
        swap(ch[x][0], ch[x][1]);
        tag[x] ^= 1;
      }
    
      int find(int x) {
        access(x);
        splay(x);
        while (ch[x][0]) x = ch[x][0];
        splay(x);
        return x;
      }
    } st;
    
    int n, q, x, y;
    char op;
    
    int main() {
      scanf("%d%d", &n, &q);
      while (q--) {
        scanf(" %c%d%d", &op, &x, &y);
        if (op == 'A') {
          st.makeroot(x);
          st.makeroot(y);
          st.fa[x] = y;
          st.siz2[y] += st.siz[x];
        }
        if (op == 'Q') {
          st.makeroot(x);
          st.access(y);
          st.splay(y);
          st.ch[y][0] = st.fa[x] = 0;
          st.maintain(x);
          st.makeroot(x);
          st.makeroot(y);
          printf("%lld\n", (ll)(st.siz[x] * st.siz[y]));
          st.makeroot(x);
          st.makeroot(y);
          st.fa[x] = y;
          st.siz2[y] += st.siz[x];
        }
      }
      return 0;
    }
    ```

### Bài tập

-   [luogu P4299 Capital](https://www.luogu.com.cn/problem/P4299)
-   [SPOJ QTREE5 - Query on a tree V](https://www.spoj.com/problems/QTREE5)