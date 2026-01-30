Trang này sẽ giới thiệu tóm tắt về danh sách liên kết.

## Giới thiệu

Danh sách liên kết (Linked List) là một cấu trúc dữ liệu dùng để lưu trữ dữ liệu, kết nối các phần tử thông qua các con trỏ giống như một sợi dây xích. Đặc điểm của nó là việc chèn và xóa dữ liệu rất thuận tiện, nhưng hiệu suất tìm kiếm và đọc dữ liệu lại kém.

## So sánh với mảng

Cả danh sách liên kết và mảng (Array) đều có thể dùng để lưu trữ dữ liệu. Khác với danh sách liên kết, mảng lưu trữ tất cả các phần tử theo thứ tự liên tiếp. Các cấu trúc lưu trữ khác nhau mang lại những ưu thế khác nhau cho chúng:

Danh sách liên kết nhờ cấu trúc dạng chuỗi nên có thể dễ dàng xóa, chèn dữ liệu, độ phức tạp thao tác là $O(1)$. Nhưng cũng chính vì vậy, hiệu quả tìm kiếm, đọc dữ liệu không cao bằng mảng, độ phức tạp thao tác truy cập ngẫu nhiên là $O(n)$.

Mảng có thể thuận tiện tìm kiếm và đọc dữ liệu, độ phức tạp thao tác truy cập ngẫu nhiên là $O(1)$. Nhưng độ phức tạp thao tác xóa, chèn là $O(n)$.

## Xây dựng danh sách liên kết

???+ tip "Mẹo"
    Khi xây dựng danh sách liên kết, phần sử dụng con trỏ khá trừu tượng. Chỉ dựa vào văn bản và code có thể khó hiểu, khuyến khích kết hợp với hình vẽ để hiểu rõ hơn.

### Danh sách liên kết đơn

Danh sách liên kết đơn bao gồm trường dữ liệu và trường con trỏ, trong đó trường dữ liệu dùng để chứa dữ liệu, trường con trỏ dùng để kết nối nút hiện tại và nút tiếp theo.

![](images/list.svg)

???+ note "Hiện thực"
    === "C++"
        ```cpp
        struct Node {
          int value;
          Node *next;
        };
        ```
    
    === "Python"
        ```python
        class Node:
            def __init__(self, value=None, next=None):
                self.value = value
                self.next = next
        ```

### Danh sách liên kết đôi

Danh sách liên kết đôi cũng có trường dữ liệu và trường con trỏ. Điểm khác biệt là trường con trỏ có phân biệt trái phải (hoặc trước, sau), dùng để kết nối nút phía trước, nút hiện tại và nút phía sau.

![](images/double-list.svg)

???+ note "Hiện thực"
    === "C++"
        ```cpp
        struct Node {
          int value;
          Node *left;
          Node *right;
        };
        ```
    
    === "Python"
        ```python
        class Node:
            def __init__(self, value=None, left=None, right=None):
                self.value = value
                self.left = left
                self.right = right
        ```

## Chèn dữ liệu vào danh sách liên kết

### Danh sách liên kết đơn

Quy trình đại khái như sau:

1.  Khởi tạo dữ liệu `node` cần chèn;
2.  Trỏ con trỏ `next` của `node` vào nút tiếp theo của `p`;
3.  Trỏ con trỏ `next` của `p` vào `node`.

Quá trình cụ thể có thể tham khảo hình dưới đây:

1.  ![](./images/list-insert-1.svg)
2.  ![](./images/list-insert-2.svg)
3.  ![](./images/list-insert-3.svg)

Code hiện thực như sau:

???+ note "Hiện thực"
    === "C++"
        ```cpp
        void insertNode(int i, Node *p) {
          Node *node = new Node;
          node->value = i;
          node->next = p->next;
          p->next = node;
        }
        ```
    
    === "Python"
        ```python
        def insertNode(i, p):
            node = Node()
            node.value = i
            node.next = p.next
            p.next = node
        ```

### Danh sách liên kết vòng đơn

Nối đầu và đuôi danh sách lại, danh sách liên kết trở thành danh sách liên kết vòng (Circular Linked List). Do danh sách nối đầu đuôi, khi chèn dữ liệu cần kiểm tra danh sách gốc có rỗng không: nếu rỗng thì tự vòng lại chính nó, nếu không rỗng thì chèn dữ liệu bình thường.

Quy trình đại khái như sau:

1.  Khởi tạo dữ liệu `node` cần chèn;
2.  Kiểm tra danh sách `p` đã cho có rỗng không;
3.  Nếu rỗng, trỏ con trỏ `next` của `node` và cả `p` vào chính `node`;
4.  Nếu không, trỏ con trỏ `next` của `node` vào nút tiếp theo của `p`;
5.  Trỏ con trỏ `next` của `p` vào `node`.

Quá trình cụ thể có thể tham khảo hình dưới đây:

1.  ![](./images/list-insert-cyclic-1.svg)
2.  ![](./images/list-insert-cyclic-2.svg)
3.  ![](./images/list-insert-cyclic-3.svg)

Code hiện thực như sau:

???+ note "Hiện thực"
    === "C++"
        ```cpp
        void insertNode(int i, Node *p) {
          Node *node = new Node;
          node->value = i;
          node->next = NULL;
          if (p == NULL) {
            p = node;
            node->next = node;
          } else {
            node->next = p->next;
            p->next = node;
          }
        }
        ```
    
    === "Python"
        ```python
        def insertNode(i, p):
            node = Node()
            node.value = i
            node.next = None
            if p == None:
                p = node
                node.next = node
            else:
                node.next = p.next
                p.next = node
        ```

### Danh sách liên kết vòng đôi

Khi chèn dữ liệu vào danh sách liên kết vòng đôi, ngoài việc kiểm tra danh sách đã cho có rỗng không, còn cần sửa đổi đồng thời hai con trỏ trái, phải.

Quy trình đại khái như sau:

1.  Khởi tạo dữ liệu `node` cần chèn;
2.  Kiểm tra danh sách `p` đã cho có rỗng không;
3.  Nếu rỗng, trỏ cả con trỏ `left` và `right` của `node`, cũng như `p` vào chính `node`;
4.  Nếu không, trỏ con trỏ `left` của `node` vào `p`;
5.  Trỏ con trỏ `right` của `node` vào nút bên phải của `p`;
6.  Trỏ con trỏ `left` của nút bên phải `p` vào `node`;
7.  Trỏ con trỏ `right` của `p` vào `node`.

Code hiện thực như sau:

???+ note "Hiện thực"
    === "C++"
        ```cpp
        void insertNode(int i, Node *p) {
          Node *node = new Node;
          node->value = i;
          if (p == NULL) {
            p = node;
            node->left = node;
            node->right = node;
          } else {
            node->left = p;
            node->right = p->right;
            p->right->left = node;
            p->right = node;
          }
        }
        ```
    
    === "Python"
        ```python
        def insertNode(i, p):
            node = Node()
            node.value = i
            if p == None:
                p = node
                node.left = node
                node.right = node
            else:
                node.left = p
                node.right = p.right
                p.right.left = node
                p.right = node
        ```

## Xóa dữ liệu khỏi danh sách liên kết

### Danh sách liên kết đơn (vòng)

Giả sử nút cần xóa là `p`, khi xóa nó khỏi danh sách, chỉ cần ghi đè giá trị của nút tiếp theo `p->next` lên `p`, đồng thời cập nhật nút sau của nút tiếp theo. (Lưu ý: Cách này giả định `p` không phải là nút cuối cùng hoặc danh sách là vòng).

Quy trình đại khái như sau:

1.  Gán giá trị của nút tiếp theo cho `p`, để xóa bỏ `p->value` cũ;
2.  Tạo một nút tạm `t` để lưu địa chỉ của `p->next`;
3.  Trỏ con trỏ `next` của `p` vào nút sau của nút tiếp theo (tức là `p->next->next`), để xóa bỏ liên kết tới `p->next` cũ;
4.  Xóa `t`. Lúc này tuy địa chỉ của nút `p` gốc vẫn đang được sử dụng, cái bị xóa thực chất là nút `p->next` gốc, nhưng do dữ liệu của `p` đã bị `p->next` ghi đè, `p` cũ coi như đã biến mất.

Quá trình cụ thể có thể tham khảo hình dưới đây:

1.  ![](./images/list-delete-1.svg)
2.  ![](./images/list-delete-2.svg)
3.  ![](./images/list-delete-3.svg)

Code hiện thực như sau:

???+ note "Hiện thực"
    === "C++"
        ```cpp
        void deleteNode(Node *p) {
          p->value = p->next->value;
          Node *t = p->next;
          p->next = p->next->next;
          delete t;
        }
        ```
    
    === "Python"
        ```python
        def deleteNode(p):
            p.value = p.next.value
            p.next = p.next.next
        ```

### Danh sách liên kết vòng đôi

Quy trình đại khái như sau:

1.  Trỏ con trỏ phải của nút bên trái `p` vào nút bên phải `p`;
2.  Trỏ con trỏ trái của nút bên phải `p` vào nút bên trái `p`;
3.  Tạo một nút tạm `t` lưu địa chỉ của `p`;
4.  Gán địa chỉ nút bên phải `p` cho `p`, để tránh `p` trở thành con trỏ lơ lửng (dangling pointer);
5.  Xóa `t`.

Code hiện thực như sau:

???+ note "Hiện thực"
    === "C++"
        ```cpp
        void deleteNode(Node *&p) {
          p->left->right = p->right;
          p->right->left = p->left;
          Node *t = p;
          p = p->right;
          delete t;
        }
        ```
    
    === "Python"
        ```python
        def deleteNode(p):
            p.left.right = p.right
            p.right.left = p.left
            p = p.right
        ```

## Kỹ thuật

### Danh sách liên kết XOR

Danh sách liên kết XOR (XOR Linked List) về bản chất vẫn là **danh sách liên kết đôi**, nhưng nó tận dụng giá trị phép XOR bit để chỉ sử dụng bộ nhớ của một con trỏ mà vẫn thực hiện được chức năng của danh sách liên kết đôi.

Chúng ta định nghĩa `lr = left ^ right` trong cấu trúc `Node`, tức là **giá trị XOR bit** của địa chỉ hai phần tử trước và sau. Khi duyệt xuôi, dùng địa chỉ phần tử trước XOR với `lr` của nút hiện tại sẽ được địa chỉ phần tử sau; khi duyệt ngược, dùng địa chỉ phần tử sau XOR với `lr` của nút hiện tại sẽ được địa chỉ phần tử trước.
Như vậy, có thể thực hiện chức năng tương tự danh sách liên kết đôi với một nửa lượng bộ nhớ dành cho con trỏ.