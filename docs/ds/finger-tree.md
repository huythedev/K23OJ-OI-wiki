author: isdanni

???+ warning "Lưu ý"
    Chương này là nội dung tùy chọn. Vui lòng đảm bảo bạn đã có hiểu biết nhất định về Lập trình Hàm (Functional Programming) trước khi đọc.

## Giới thiệu

**Cây Ngón Tay** (Finger Tree) là một cấu trúc dữ liệu **thuần hàm** (purely functional) được đề xuất bởi Ralf Hinze và Ross Paterson.

## Tại sao cần Cây Ngón Tay?

Trong lập trình hàm, danh sách (list/sequence) là một kiểu dữ liệu rất phổ biến. Hầu hết các ngôn ngữ hàm đều hỗ trợ các thao tác trên dãy (sequence), bao gồm thêm/xóa phần tử ở hai đầu (thao tác Deque/Hàng đợi hai đầu), chèn/nối/xóa tại vị trí bất kỳ, tìm kiếm phần tử thỏa mãn điều kiện, hoặc chia dãy thành các dãy con. Tuy nhiên, để thực hiện các thao tác này một cách hiệu quả là rất khó. Ngay cả khi có các triển khai tương ứng, chúng thường rất phức tạp và khó sử dụng trong thực tế.

Cây Ngón Tay cung cấp một cấu trúc dữ liệu dãy thuần hàm, cho phép thực hiện các thao tác như truy cập, thêm vào đầu và cuối dãy với **thời gian hằng số trung bình** (amortized constant time), và các thao tác nối (concatenation) cùng truy cập ngẫu nhiên với **thời gian logarit** (logarithmic time). Ngoài giới hạn thời gian tiệm cận tốt, Cây Ngón Tay còn rất linh hoạt: khi kết hợp với các **nhãn đơn điệu** (monoidal tag) trên các phần tử, nó có thể được sử dụng để triển khai các dãy truy cập ngẫu nhiên hiệu quả, dãy có thứ tự, cây khoảng (interval tree) và hàng đợi ưu tiên.

## Cấu trúc cơ bản

Cây Ngón Tay lưu trữ dữ liệu tại các "ngón tay" (lá) của cây, nơi việc truy cập có độ phức tạp thời gian trung bình hằng số. "Ngón tay" là một điểm có thể truy cập một phần của cấu trúc dữ liệu. Trong ngôn ngữ mệnh lệnh (imperative language), điều này được gọi là con trỏ. Trong Cây Ngón Tay, "ngón tay" là cấu trúc trỏ đến các phần cuối của dãy hoặc các nút lá. Cây Ngón Tay còn lưu trữ kết quả của một số phép toán kết hợp (associative operation) được áp dụng cho các hậu duệ của nó tại mỗi nút bên trong. Dữ liệu được lưu trữ tại các nút bên trong có thể cung cấp các chức năng vượt ra ngoài cấu trúc cây điển hình.

1. Độ sâu của Cây Ngón Tay được tính từ dưới lên.
2. Cấp đầu tiên của Cây Ngón Tay, tức là các nút lá, chỉ chứa giá trị, có độ sâu là $0$. Cấp thứ hai có độ sâu $1$. Cấp thứ ba có độ sâu $2$, v.v.
3. Càng gần gốc, nút càng trỏ đến một cây con sâu hơn của cây gốc (trước khi nó trở thành Cây Ngón Tay). Do đó, làm việc đi xuống cây là đi từ lá lên gốc, điều này trái ngược với cấu trúc cây điển hình. Để đạt được cấu trúc này, chúng ta phải đảm bảo cây gốc có độ sâu đồng nhất. Khi khai báo đối tượng nút, nó phải được tham số hóa bằng kiểu của các nút con của nó. Các nút trên đường sống (spine) có độ sâu $1$ trở lên có thể được biểu diễn bằng các nút lồng nhau thông qua tham số hóa này.

### Biến đổi một cây thành Cây Ngón Tay

???+ note "Chú thích"
    **Cây 2-3** là một cấu trúc dữ liệu dạng cây, trong đó mỗi nút có các nút con (nút trong) có hai con (nút $2$) và một phần tử dữ liệu, hoặc có ba con (nút $3$) và hai phần tử dữ liệu. Cây 2-3 là cây B bậc $3$. Các nút bên ngoài cây (nút lá) không có con và chứa một hoặc hai phần tử dữ liệu.

Chúng ta sẽ bắt đầu quá trình này từ một cây 2-3 cân bằng. Để Cây Ngón Tay hoạt động chính xác, tất cả các nút lá cần phải nằm trên cùng một mức. Hình dưới đây minh họa điều này (hình ảnh lấy từ bài báo về Cây Ngón Tay):

![](./images/finger-tree-1.png)

"Ngón tay" là "một cấu trúc có thể truy cập hiệu quả các nút của cây gần một vị trí cụ thể." Để tạo Cây Ngón Tay, chúng ta cần đặt các ngón tay ở hai đầu trái và phải của cây, lấy nút trong ngoài cùng bên trái và bên phải nhất, và kéo chúng ra ngoài, khiến phần còn lại của cây lơ lửng ở giữa. Điều này cho phép chúng ta có được thời gian truy cập hằng số trung bình đến các đầu dãy.

![](./images/finger-tree-2.png)

Cấu trúc dữ liệu mới này được gọi là Cây Ngón Tay. Cây Ngón Tay bao gồm một vài cấp (khung màu xanh lam bên dưới) được phân bố dọc theo đường sống của cây (đường màu nâu):

![](./images/finger-tree-3.png)

```haskell
data FingerTree a = Empty
                  | Single a
                  | Deep (Digit a) (FingerTree (Node a)) (Digit a)

data Digit a = One a | Two a a | Three a a a | Four a a a a
data Node a = Node2 a a | Node3 a a a
```

Các số trong ví dụ là các nút có chứa các chữ cái. Mỗi danh sách được chia bởi tiền tố hoặc hậu tố của các nút trên đường sống. Trong cây 2-3 đã chuyển đổi, danh sách các số ở tầng trên cùng dường như có độ dài hai hoặc ba, trong khi các cấp thấp hơn chỉ có độ dài một hoặc hai. Để một số ứng dụng của Cây Ngón Tay chạy hiệu quả, Cây Ngón Tay cho phép $1$ đến $4$ cây con tại mỗi cấp. Các "chữ số" (Digit) của Cây Ngón Tay có thể được chuyển đổi thành một danh sách như sau:

```haskell
type Digit a = One a | Two a a | Three a a a | Four a a a a
```

Cấp trên cùng chứa các phần tử kiểu $a$, cấp tiếp theo chứa các phần tử kiểu nút $a$, bởi vì các nút giữa đường sống và lá, điều này thường có nghĩa là tầng thứ $n$ của cây chứa các phần tử kiểu $Node^{n} a$, hay cây $2-3$ có độ sâu $n$. Điều này có nghĩa là dãy $n$ phần tử được biểu diễn bởi một cây có độ sâu $\Theta(\log n)$. Phần tử cách đầu mút $d$ gần nhất được lưu trữ ở độ sâu $\Theta(\log d)$ trong cây.

### Thao tác Hàng đợi hai đầu (Deque)

Cây Ngón Tay cũng có thể tạo thành một Hàng đợi hai đầu hiệu quả. Tất cả các thao tác đều cần $\Theta(1)$ thời gian, bất kể cấu trúc có bền vững (persistent) hay không. Nó có thể được xem là phần mở rộng của hàng đợi hai đầu được biểu diễn ẩn[^okasaki1999purely]:

1. Thay thế các nút $2-3$ bằng các nút $2-4$ cung cấp đủ sự linh hoạt để hỗ trợ nối hiệu quả. (Để duy trì thao tác hàng đợi hai đầu thời gian hằng số, $Digit$ phải được mở rộng thành bốn).
2. Ghi nhãn các nút bên trong bằng một đơn điệu (monoid) cho phép chia tách hiệu quả.

```haskell
data ImplicitDeque a = Empty
                     | Single a
                     | Deep (Digit a) (ImplicitDeque (a, a)) (Digit a)

data Digit a = One a | Two a a | Three a a a
```

## Độ phức tạp Thời gian

Cây Ngón Tay cung cấp thời gian truy cập trung bình hằng số đến các "ngón tay" (lá) của cây, nơi dữ liệu được lưu trữ, và thời gian logarit để nối và tách tại các phần nhỏ. Nó cũng lưu trữ kết quả của một số phép toán kết hợp được áp dụng cho các hậu duệ của nó tại mỗi nút bên trong. Dữ liệu "tóm tắt" (summary) được lưu trữ trong các nút bên trong có thể cung cấp các chức năng vượt ra ngoài chức năng của cấu trúc cây khác.

| Thao tác | Cây Ngón Tay | Cây 2-3 có nhãn (annotated 2-3 tree) | Danh sách (list) | Vector |
| :--- | :--- | :--- | :--- | :--- |
| `const`, `snoc` (Thêm cuối) | $O(1)$ | $O(\log n)$ | $O(1)$/$O(n)$ | $O(n)$ |
| `viewl`, `viewr` (Xem đầu/cuối) | $O(1)$ | $O(\log n)$ | $O(1)$/$O(n)$ | $O(1)$ |
| `measure`/`length` (Đo/Độ dài) | $O(1)$ | $O(1)$ | $O(n)$ | $O(1)$ |
| `append` (Nối) | $O(\log \min(l_1, l_2))$ | $O(\log n)$ | $O(n)$ | $O(m+n)$ |
| `split` (Tách) | $O(\log \min(n, l-n))$ | $O(\log n)$ | $O(n)$ | $O(1)$ |
| `replicate` (Lặp lại) | $O(\log n)$ | $O(\log n)$ | $O(n)$ | $O(n)$ |
| `fromList`, `toList`, `reverse` | $O(l)$/$O(l)$/$O(l)$ | $O(l)$ | $O(1)$/$O(1)$/$O(n)$ | $O(n)$ |
| `index` (Truy cập theo chỉ mục) | $O(\log \min(n, l-n))$ | $O(\log n)$ | $O(n)$ | $O(1)$ |

## Ứng dụng

Cây Ngón Tay có thể được sử dụng để xây dựng các cây khác. Ví dụ, một hàng đợi ưu tiên có thể được hiện thực bằng cách gán nhãn các nút bên trong bằng giá trị ưu tiên nhỏ nhất của các nút con của chúng, hoặc một danh sách/mảng có chỉ mục có thể được hiện thực bằng cách gán nhãn các nút bằng số lượng lá của cây con của chúng. Các ứng dụng khác bao gồm dãy truy cập ngẫu nhiên (như đã đề cập), dãy có thứ tự và cây khoảng.

Cây Ngón Tay có thể cung cấp trung bình $O(1)$ cho các thao tác đẩy (push), đảo ngược (reverse), và bật ra (pop), $O(\log n)$ cho việc nối và tách; và có thể được điều chỉnh cho các dãy có chỉ mục hoặc có thứ tự. Giống như tất cả các cấu trúc dữ liệu hàm, nó về bản chất là bền vững (persistent); nghĩa là, các phiên bản cũ của cây luôn được bảo toàn.

Về mặt hiện thực mã, dãy hữu hạn `Seq` trong thư viện lõi Haskell sử dụng hiện thực cây ngón tay $2-3$ ([Data.Sequence](https://hackage.haskell.org/package/containers-0.6.5.1/docs/Data-Sequence.html)). [Hiện thực](https://ocaml-batteries-team.github.io/batteries-included/hdoc2/BatFingerTree.html) trong mô-đun `BatFingerTree` của OCaml cũng sử dụng cấu trúc dữ liệu Cây Ngón Tay tổng quát. Cây Ngón Tay có thể được hiện thực có hoặc không có đánh giá lười (lazy evaluation), nhưng đánh giá lười cho phép hiện thực đơn giản hơn.

## Tài liệu tham khảo và Đọc thêm

1. Ralf Hinze và Ross Paterson, "[Finger trees: a simple general-purpose data structure](http://www.staff.city.ac.uk/~ross/papers/FingerTree.html)", Journal of Functional Programming 16:2 (2006) trang 197-217.
2. [Finger Tree - Wikipedia](https://en.wikipedia.org/wiki/Finger_tree)

[^okasaki1999purely]: [Purely Functional Data Structures](https://www.cambridge.org/us/academic/subjects/computer-science/programming-languages-and-applied-logic/purely-functional-data-structures), Chris Okasaki (1999)