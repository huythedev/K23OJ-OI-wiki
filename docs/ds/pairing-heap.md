## Giới thiệu

Pairing Heap là một cấu trúc dữ liệu hỗ trợ các thao tác như chèn, truy vấn/xóa giá trị nhỏ nhất, hợp nhất, và thay đổi giá trị phần tử. Nó là một loại heap có thể hợp nhất. Pairing Heap có ưu điểm là tốc độ nhanh và cấu trúc đơn giản, nhưng do độ phức tạp phân tích khấu hao (amortized complexity) dựa trên thế năng, nó không hỗ trợ tính bền vững (persistence).

## Định nghĩa

Pairing Heap là một cây đa phân có trọng số thỏa mãn tính chất heap (như hình dưới), tức là trọng số của mỗi nút đều nhỏ hơn hoặc bằng tất cả các con của nó (lấy ví dụ là heap nhỏ nhất, tương tự bên dưới).
![](./images/pairingheap1.jpg)

Thông thường, chúng ta sử dụng biểu diễn con-huynh đệ (left-child, right-sibling representation) để lưu trữ một Pairing Heap (như hình dưới). Tất cả các con của một nút tạo thành một danh sách liên kết đơn. Mỗi nút lưu trữ con trỏ đến con đầu tiên của nó, tức là đầu của danh sách liên kết; và con trỏ đến anh em bên phải của nó.

Cách này giúp dễ dàng cài đặt Pairing Heap và cũng thuận tiện cho việc phân tích độ phức tạp.

![](./images/pairingheap2.jpg)

```cpp
struct Node {
  T v;  // T là kiểu dữ liệu của trọng số
  Node *child, *sibling;
  // child trỏ đến con đầu tiên của nút này, sibling trỏ đến anh em tiếp theo của nút này.
  // Nếu nút này không có con/anh em tiếp theo thì con trỏ trỏ đến nullptr.
};
```

Từ định nghĩa có thể thấy, so với các cấu trúc heap phổ biến khác, Pairing Heap không duy trì bất kỳ thông tin bổ sung nào như kích thước cây, độ sâu, thứ hạng, v.v. (Binary Heap cũng không duy trì thông tin bổ sung, nhưng nó đảm bảo độ phức tạp thao tác bằng cách duy trì cấu trúc cây nhị phân hoàn chỉnh nghiêm ngặt), và bất kỳ cây nào thỏa mãn tính chất heap đều là một Pairing Heap hợp lệ. Cấu trúc dữ liệu đơn giản và linh hoạt cao này đặt nền tảng cho hiệu suất tuyệt vời của Pairing Heap trong thực tế; ngược lại, hằng số lớn của Fibonacci Heap là do nó cần duy trì nhiều thông tin bổ sung.

Pairing Heap đảm bảo độ phức tạp tổng thể của nó thông qua một bộ thứ tự thao tác được thiết kế cẩn thận, bài báo gốc[^ref1] gọi nó là "một loại heap tự điều chỉnh (Self Adjusting Heap)". Về mặt này, nó khá giống với cây Splay (được gọi là "Self Adjusting Binary Tree" trong bài báo gốc).

## Quá trình

### Truy vấn giá trị nhỏ nhất

Từ định nghĩa của Pairing Heap có thể thấy, trọng số của nút gốc của Pairing Heap chắc chắn là nhỏ nhất, chỉ cần trả về nút gốc là được.

### Hợp nhất

Thao tác hợp nhất hai Pairing Heap rất đơn giản. Trước tiên, lấy nút gốc có trọng số nhỏ hơn làm nút gốc mới, sau đó chèn nút gốc lớn hơn vào làm con của nó. (Xem hình dưới)

![](./images/pairingheap3.jpg)

Cần lưu ý rằng, danh sách liên kết con của một nút được sắp xếp theo thời gian chèn, tức là nút bên phải nhất là con sớm nhất của nút cha, nút bên trái nhất là con mới nhất của nút cha.

???+ note "Hiện thực"
    ```cpp
    Node* meld(Node* x, Node* y) {
      // Nếu một trong hai rỗng thì trả về cái kia
      if (x == nullptr) return y;
      if (y == nullptr) return x;
      if (x->v > y->v) std::swap(x, y);  // swap xong x là heap có trọng số nhỏ, y là heap có trọng số lớn
      // Đặt y làm con của x
      y->sibling = x->child;
      x->child = y;
      return x;  // Nút gốc mới là x
    }
    ```

### Chèn

Đã có hợp nhất rồi, chèn thì chỉ cần coi phần tử mới như một Pairing Heap mới và hợp nhất với heap cũ là xong.

### Xóa giá trị nhỏ nhất

Điểm đầu tiên cần đề cập là các thao tác trên đều rất "lười biếng", hoàn toàn không bảo trì cấu trúc dữ liệu, vì vậy chúng ta cần cẩn thận thiết kế thao tác xóa giá trị nhỏ nhất để đảm bảo độ phức tạp tổng thể không xảy ra vấn đề.

Nút gốc chính là giá trị nhỏ nhất, vì vậy cái cần xóa là nút gốc. Xem xét điều gì sẽ xảy ra sau khi loại bỏ nút gốc: tất cả các con ban đầu của nút gốc tạo thành một khu rừng; và Pairing Heap phải là một cái cây, vì vậy chúng ta cần hợp nhất tất cả các con này lại theo một thứ tự nào đó.

Một ý tưởng tự nhiên là sử dụng hàm `meld` để hợp nhất các con lần lượt từ trái sang phải. Làm như vậy thì tính đúng đắn là hiển nhiên, nhưng sẽ khiến độ phức tạp của một lần thao tác suy biến thành $O(n)$.

Để đảm bảo độ phức tạp khấu hao tổng thể, cần sử dụng phương pháp hợp nhất "hai bước":

1.  Ghép đôi các con từng cặp một, dùng thao tác `meld` để hợp nhất hai con được ghép đôi lại với nhau (xem Hình 1 dưới đây),
2.  Hợp nhất các heap mới được tạo ra lần lượt **từ phải sang trái** (tức là hướng từ con cũ đến con mới) lại với nhau (xem Hình 2 dưới đây).

![](./images/pairingheap4.jpg)

![](./images/pairingheap5.jpg)

Trước tiên, cài đặt một hàm hỗ trợ `merges`, tác dụng là hợp nhất tất cả anh em của một nút.

???+ note "Hiện thực"
    ```cpp
    Node* merges(Node* x) {
      if (x == nullptr || x->sibling == nullptr)
        return x;  // Nếu cây rỗng hoặc nó không có anh em tiếp theo, thì không cần hợp nhất, return.
      Node* y = x->sibling;                // y là anh em tiếp theo của x
      Node* c = y->sibling;                // c là anh em tiếp theo nữa
      x->sibling = y->sibling = nullptr;   // Tách rời
      return meld(merges(c), meld(x, y));  // Phần cốt lõi
    }
    ```

Câu cuối cùng là cốt lõi của hàm này, câu này chia làm ba phần:

1.  `meld(x,y)` "ghép đôi" x và y.
2.  `merges(c)` đệ quy hợp nhất c và các anh em của nó.
3.  Hợp nhất 2 cây mới được tạo ra từ 2 thao tác trên.

Cần lưu ý rằng, phần trên đã đề cập đến yêu cầu về hướng hợp nhất ở bước thứ hai (hợp nhất từ phải sang trái), việc cài đặt hàm đệ quy này đã đảm bảo thứ tự đó. Nếu độc giả cần tự cài đặt phiên bản lặp, vui lòng chú ý đảm bảo thứ tự này, nếu không độ phức tạp sẽ không được đảm bảo.

Với hàm `merges`, thao tác `delete-min` trở nên hiển nhiên.

???+ note "Hiện thực"
    ```cpp
    Node* delete_min(Node* x) {
      Node* t = merges(x->child);
      delete x;  // Nếu cần thu hồi bộ nhớ
      return t;
    }
    ```

### Giảm giá trị của một phần tử

Để thực hiện thao tác này, cần thêm một con trỏ "cha" cho nút. Khi nút có anh em bên trái, con trỏ này trỏ đến anh em bên trái thay vì cha thực sự; ngược lại, nó trỏ đến cha của nó.

Đầu tiên, định nghĩa của nút được sửa đổi như sau:

???+ note "Hiện thực"
    ```cpp
    struct Node {
      LL v;
      int id;
      Node *child, *sibling;
      Node *father;  // Mới thêm: con trỏ cha, nếu nút này là nút gốc thì trỏ đến nút rỗng nullptr
    };
    ```

Thao tác `meld` được sửa đổi như sau:

???+ note "Hiện thực"
    ```cpp
    Node* meld(Node* x, Node* y) {
      if (x == nullptr) return y;
      if (y == nullptr) return x;
      if (x->v > y->v) std::swap(x, y);
      if (x->child != nullptr) {  // Mới thêm: bảo trì con trỏ cha
        x->child->father = y;
      }
      y->sibling = x->child;
      y->father = x;  // Mới thêm: bảo trì con trỏ cha
      x->child = y;
      return x;
    }
    ```

Thao tác `merges` được sửa đổi như sau:

???+ note "Hiện thực"
    ```cpp
    Node *merges(Node *x) {
      if (x == nullptr) return nullptr;
      x->father = nullptr;  // Mới thêm: bảo trì con trỏ cha
      if (x->sibling == nullptr) return x;
      Node *y = x->sibling, *c = y->sibling;
      y->father = nullptr;  // Mới thêm: bảo trì con trỏ cha
      x->sibling = y->sibling = nullptr;
      return meld(merges(c), meld(x, y));
    }
    ```

Bây giờ chúng ta xem xét cách cài đặt thao tác `decrease-key`.  
Đầu tiên chúng ta nhận thấy rằng, sau khi giảm trọng số của nút `x`, cây con có gốc tại `x` vẫn thỏa mãn tính chất Pairing Heap, nhưng cha của `x` và `x` có thể không còn thỏa mãn tính chất heap nữa.  
Do đó, chúng ta tách toàn bộ cây con có gốc tại `x` ra, bây giờ cả hai cây đều thỏa mãn tính chất Pairing Heap, sau đó hợp nhất chúng lại với nhau là hoàn thành toàn bộ thao tác.

???+ note "Hiện thực"
    ```cpp
    // root là gốc của heap, x là nút cần thao tác, v là trọng số mới, khi gọi cần đảm bảo v <= x->v
    // Giá trị trả về là nút gốc mới
    Node *decrease_key(Node *root, Node *x, LL v) {
      x->v = v;                 // Cập nhật trọng số
      if (x == root) return x;  // Nếu x là gốc, trả về ngay
      // Tách x ra khỏi các nút con của fa, ở đây cần chia trường hợp vị trí của x.
      if (x->father->child == x) {
        x->father->child = x->sibling;
      } else {
        x->father->sibling = x->sibling;
      }
      if (x->sibling != nullptr) {
        x->sibling->father = x->father;
      }
      x->sibling = nullptr;
      x->father = nullptr;
      return meld(root, x);  // Hợp nhất lại x và nút gốc
    }
    ```

## Phân tích độ phức tạp

Cấu trúc và cài đặt của Pairing Heap đơn giản, nhưng phân tích độ phức tạp thời gian lại không dễ dàng.

Bài báo gốc[^ref1] chỉ phân tích độ phức tạp đến mức các thao tác `meld` và `delete-min` đều là khấu hao $O(\log n)$, nhưng đưa ra giả thuyết rằng các thao tác của nó có cùng độ phức tạp với Fibonacci Heap.

Đáng tiếc là, sau đó người ta phát hiện ra rằng, Pairing Heap không bảo trì thông tin bổ sung, trong một chuỗi thao tác cụ thể, độ phức tạp khấu hao của thao tác `decrease-key` có cận dưới ít nhất là $\Omega (\log \log n)$[^ref2].

Hiện tại, các ước lượng tốt hơn về cận trên độ phức tạp bao gồm: $O(1)$ cho `meld` và $O(\log n)$ cho `decrease-key` của Iacono[^ref3]; $O(2^{2 \sqrt{\log \log n}})$ cho `meld` và `decrease-key` của Pettie[^ref4]. Cần lưu ý rằng, các độ phức tạp trên đều là độ phức tạp khấu hao, do đó không thể lấy giá trị nhỏ nhất cho từng kết quả riêng biệt.

## Tài liệu tham khảo

[^ref1]: [The pairing heap: a new form of self-adjusting heap](http://www.cs.cmu.edu/~sleator/papers/pairing-heaps.pdf)

[^ref2]: [On the efficiency of pairing heaps and related data structures](https://dl.acm.org/doi/10.1145/320211.320214)

[^ref3]: [Improved upper bounds for pairing heaps](https://arxiv.org/abs/1110.4428)

[^ref4]: [Towards a Final Analysis of Pairing Heaps](http://web.eecs.umich.edu/~pettie/papers/focs05.pdf)

-   <https://en.wikipedia.org/wiki/Pairing_heap>
-   <https://brilliant.org/wiki/pairing-heap/>