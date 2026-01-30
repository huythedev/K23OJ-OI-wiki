## Định nghĩa

Cây tìm kiếm nhị phân (Binary Search Tree - BST) là một cấu trúc dữ liệu dạng cây nhị phân, được định nghĩa như sau:

1.  Cây rỗng là một cây tìm kiếm nhị phân.

2.  Nếu cây con bên trái của cây tìm kiếm nhị phân không rỗng, thì tất cả các nút trong cây con bên trái đều có giá trị khóa nhỏ hơn giá trị của nút gốc.

3.  Nếu cây con bên phải của cây tìm kiếm nhị phân không rỗng, thì tất cả các nút trong cây con bên phải đều có giá trị khóa lớn hơn giá trị của nút gốc.

4.  Cây con bên trái và cây con bên phải của cây tìm kiếm nhị phân cũng là cây tìm kiếm nhị phân.

Thời gian thực hiện các thao tác cơ bản trên cây tìm kiếm nhị phân tỷ lệ với chiều cao của cây. Đối với cây tìm kiếm nhị phân có $n$ nút, độ phức tạp thời gian tốt nhất cho các thao tác này là $O(\log n)$, và tệ nhất là $O(n)$. Chiều cao kỳ vọng của một cây tìm kiếm nhị phân được xây dựng ngẫu nhiên là $O(\log n)$.

## Quá trình

### Định nghĩa nút của cây tìm kiếm nhị phân

???+ note "Thực hiện"
    ```cpp
    struct TreeNode {
      int key;
      TreeNode* left;
      TreeNode* right;
      // Duy trì các thông tin khác, ví dụ: chiều cao, số lượng nút
      int size;   // Kích thước cây con với nút hiện tại là gốc
      int count;  // Số lần lặp lại của giá trị khóa hiện tại
    
      TreeNode(int value)
          : key(value), size(1), count(1), left(nullptr), right(nullptr) {}
    };
    ```

### Duyệt cây tìm kiếm nhị phân

Do định nghĩa đệ quy của cây tìm kiếm nhị phân, thứ tự duyệt trung thứ tự (inorder traversal) của các giá trị khóa là một dãy không giảm. Độ phức tạp thời gian là $O(n)$.

Dưới đây là đoạn mã duyệt cây tìm kiếm nhị phân:

???+ note "Thực hiện"
    ```cpp
    void inorderTraversal(TreeNode* root) {
      if (root == nullptr) {
        return;
      }
      inorderTraversal(root->left);
      std::cout << root->key << " ";
      inorderTraversal(root->right);
    }
    ```

### Tìm giá trị nhỏ nhất/lớn nhất

Do tính chất của cây tìm kiếm nhị phân, giá trị nhỏ nhất là đỉnh của chuỗi bên trái, giá trị lớn nhất là đỉnh của chuỗi bên phải. Độ phức tạp thời gian là $O(h)$.

???+ note "Thực hiện"
    ```cpp
    int findMin(TreeNode* root) {
      if (root == nullptr) {
        return -1;
      }
      while (root->left != nullptr) {
        root = root->left;
      }
      return root->key;
    }
    
    int findMax(TreeNode* root) {
      if (root == nullptr) {
        return -1;
      }
      while (root->right != nullptr) {
        root = root->right;
      }
      return root->key;
    }
    ```

### Tìm kiếm phần tử

Tìm kiếm một nút có giá trị `value` trong cây tìm kiếm nhị phân có gốc là `root`.

Thảo luận theo trường hợp:

-   Nếu `root` là rỗng, trả về `false`.
-   Nếu khóa của `root` bằng `value`, trả về `true`.
-   Nếu khóa của `root` lớn hơn `value`, tiếp tục tìm kiếm trong cây con bên trái của `root`.
-   Nếu khóa của `root` nhỏ hơn `value`, tiếp tục tìm kiếm trong cây con bên phải của `root`.

Độ phức tạp thời gian là $O(h)$.

???+ note "Thực hiện"
    ```cpp
    bool search(TreeNode* root, int target) {
      if (root == nullptr) {
        return false;
      }
      if (root->key == target) {
        return true;
      } else if (target < root->key) {
        return search(root->left, target);
      } else {
        return search(root->right, target);
      }
    }
    ```

Chèn, xóa, sửa đổi đều cần tìm kiếm trước trong cây tìm kiếm nhị phân.

### Chèn một phần tử

Chèn một nút có giá trị `value` vào cây tìm kiếm nhị phân có gốc là `root`.

Thảo luận theo trường hợp:

-   Nếu `root` là rỗng, trực tiếp trả về một nút mới có giá trị `value`.

-   Nếu khóa của `root` bằng `value`, số lần xuất hiện của giá trị bổ sung của nút này tăng thêm $1$.

-   Nếu khóa của `root` lớn hơn `value`, chèn nút có khóa `value` vào cây con bên trái của `root`.

-   Nếu khóa của `root` nhỏ hơn `value`, chèn nút có khóa `value` vào cây con bên phải của `root`.

Độ phức tạp thời gian là $O(h)$.

???+ note "Thực hiện"
    ```cpp
    TreeNode* insert(TreeNode* root, int value) {
      if (root == nullptr) {
        return new TreeNode(value);
      }
      if (value < root->key) {
        root->left = insert(root->left, value);
      } else if (value > root->key) {
        root->right = insert(root->right, value);
      } else {
        root->count++;  // Giá trị nút bằng nhau, tăng số lần lặp lại
      }
      root->size = root->count + (root->left ? root->left->size : 0) +
                   (root->right ? root->right->size : 0);  // Cập nhật kích thước cây con của nút
      return root;
    }
    ```

### Xóa một phần tử

Xóa một nút có giá trị `value` trong cây tìm kiếm nhị phân có gốc là `root`.

Trước tiên tìm kiếm nút có khóa `value` trong cây tìm kiếm nhị phân, thảo luận theo trường hợp:

-   Nếu `count` bổ sung của nút đó lớn hơn $1$, chỉ cần giảm `count`.

-   Nếu `count` của nút đó bằng $1$:

    -   Nếu `root` là nút lá, có thể xóa trực tiếp nút này.

    -   Nếu `root` là nút dây chuyền, tức là chỉ có một con, trả về con này.

    -   Nếu `root` có hai cây con không rỗng, thông thường sẽ thay thế nó bằng giá trị lớn nhất của cây con bên trái (nút ngoài cùng bên phải của cây con bên trái) hoặc giá trị nhỏ nhất của cây con bên phải (nút ngoài cùng bên trái của cây con bên phải), sau đó xóa nút đó.

Độ phức tạp thời gian $O(h)$.

???+ note "Thực hiện"
    Phương pháp sử dụng `root = remove(root, 1)` biểu thị việc xóa nút có giá trị 1 trong cây có gốc là `root`, và trả về gốc mới.
    
    ```cpp
    // Giá trị trả về là root mới sau khi xóa value
    TreeNode* remove(TreeNode* root, int value) {
      if (root == nullptr) {
        return root;
      }
      if (value < root->key) {
        root->left = remove(root->left, value);
      } else if (value > root->key) {
        root->right = remove(root->right, value);
      } else {
        if (root->count > 1) {
          root->count--;  // Số lần lặp lại của nút lớn hơn 1, giảm số lần lặp lại
        } else {
          if (root->left == nullptr) {
            TreeNode* temp = root->right;
            delete root;
            return temp;
          } else if (root->right == nullptr) {
            TreeNode* temp = root->left;
            delete root;
            return temp;
          } else {
            TreeNode* successor = findMinNode(root->right);
            root->key = successor->key;
            root->count = successor->count;  // Cập nhật số lần lặp lại
            // Khi successor->count > 1, cũng nên xóa nút này, nếu không
            // việc xóa sau này sẽ chỉ làm giảm số lần lặp lại
            successor->count = 1;
            root->right = remove(root->right, successor->key);
          }
        }
      }
      // Tiếp tục duy trì size, không viết thành --root->size;
      // Là vì value có thể không có trong cây, dẫn đến có thể không xảy ra việc xóa
      root->size = root->count + (root->left ? root->left->size : 0) +
                   (root->right ? root->right->size : 0);
      return root;
    }
    
    // Lấy ví dụ là nút nhỏ nhất của cây con bên phải
    TreeNode* findMinNode(TreeNode* root) {
      while (root->left != nullptr) {
        root = root->left;
      }
      return root;
    }
    ```

### Tính thứ hạng của một phần tử

Thứ hạng được định nghĩa là số lượng các số đứng trước phần tử đầu tiên có cùng giá trị sau khi sắp xếp các phần tử của mảng theo thứ tự tăng dần, cộng thêm một.

Để tìm thứ hạng của một phần tử, trước tiên nhảy từ nút gốc xuống nút chứa phần tử đó, nếu nhảy sang phải, đáp án cộng thêm số lượng nút con bên trái cộng với số lần lặp lại của nút hiện tại, cuối cùng đáp án cộng thêm kích thước cây con bên trái của nút đích cộng một.

Độ phức tạp thời gian $O(h)$.

???+ note "Thực hiện"
    ```cpp
    int queryRank(TreeNode* root, int v) {
      if (root == nullptr) return 0;
      if (root->key == v) return (root->left ? root->left->size : 0) + 1;
      if (root->key > v) return queryRank(root->left, v);
      return queryRank(root->right, v) + (root->left ? root->left->size : 0) +
             root->count;
    }
    ```

### Tìm phần tử có thứ hạng là k

Trong một cây con, thứ hạng của nút gốc phụ thuộc vào kích thước của cây con bên trái của nó.

-   Nếu kích thước cây con bên trái lớn hơn hoặc bằng $k$, thì phần tử đó nằm trong cây con bên trái;

-   Nếu kích thước cây con bên trái nằm trong khoảng $[k-\textit{count}, k-1]$ (với $\textit{count}$ là số lần giá trị nút hiện tại xuất hiện), thì phần tử đó là nút gốc của cây con;

-   Nếu kích thước cây con bên trái nhỏ hơn $k-\textit{count}$, thì phần tử đó nằm trong cây con bên phải.

Độ phức tạp thời gian $O(h)$.

???+ note "Thực hiện"
    ```cpp
    int querykth(TreeNode* root, int k) {
      if (root == nullptr) return -1;  // Hoặc trả về giá trị phù hợp khác theo yêu cầu
      if (root->left) {
        if (root->left->size >= k) return querykth(root->left, k);
        if (root->left->size + root->count >= k) return root->key;
      } else {
        if (k <= root->count) return root->key;
      }
      return querykth(root->right,
                      k - (root->left ? root->left->size : 0) - root->count);
    }
    ```

## Giới thiệu về cây cân bằng (Balanced Tree)

Một trong những mục đích sử dụng cây tìm kiếm là rút ngắn thời gian cho các thao tác chèn, xóa, sửa đổi và tìm kiếm (chèn, xóa, sửa đổi đều bao gồm thao tác tìm kiếm) nút.

Về hiệu suất tìm kiếm, nếu chiều cao của cây là $h$, trong trường hợp xấu nhất, việc tìm kiếm một khóa cần so sánh $h$ lần, độ phức tạp thời gian tìm kiếm (cũng là độ dài tìm kiếm trung bình ASL, Average Search Length) không vượt quá $O(h)$. Một cây tìm kiếm nhị phân lý tưởng có thể rút ngắn thời gian của tất cả các thao tác xuống $O(\log n)$ ($n$ là tổng số nút).

Tuy nhiên, độ phức tạp thời gian $O(\log n)$ chỉ là trường hợp lý tưởng. Trong trường hợp xấu nhất, cây tìm kiếm có thể thoái hóa thành một danh sách liên kết. Hãy tưởng tượng một cây tìm kiếm nhị phân mà mỗi nút chỉ có con phải, thì tính chất của nó giống hệt danh sách liên kết, thời gian cho tất cả các thao tác (thêm, xóa, sửa, tìm) là $O(n)$.

Có thể thấy độ phức tạp của các thao tác liên quan đến chiều cao $h$ của cây. Từ đó dẫn đến cây cân bằng, thông qua một số thao tác nhất định để duy trì chiều cao (tính cân bằng) của cây nhằm giảm độ phức tạp của các thao tác.

### Định nghĩa về tính cân bằng

Về việc một cây tìm kiếm có "cân bằng" hay không, các cây cân bằng khác nhau có các định nghĩa khác nhau về "cân bằng". Ví dụ, nếu cây tìm kiếm với gốc là T, sự chênh lệch chiều cao giữa cây con bên trái và cây con bên phải rất lớn, hoặc số lượng nút ở cây con bên trái lớn hơn nhiều so với cây con bên phải, thì cây này rõ ràng là không có tính cân bằng.

Đối với cây tìm kiếm nhị phân, định nghĩa phổ biến về tính cân bằng là: cây tìm kiếm với gốc là T, sự chênh lệch chiều cao giữa cây con bên trái và cây con bên phải của **mọi** nút không vượt quá 1.

-   Trong [Cây Splay](splay.md), đối với bất kỳ thao tác truy cập nút nào (tìm kiếm, chèn hay xóa), nút được truy cập sẽ được di chuyển lên vị trí gốc của cây.

-   [Cây AVL](avl.md) duy trì thông tin chiều cao của cây con với gốc là N tại mỗi nút N. Định nghĩa về tính cân bằng của AVL: Nếu T là một cây AVL, thì chỉ khi cây con trái và cây con phải cũng là cây AVL, và $|height(T->left) - height(T->right)| \leq 1$.

-   [Size Balanced Tree (SBT)](sbt.md) mỗi nút N duy trì số lượng nút `size` trong cây con với gốc là N. Định nghĩa về tính cân bằng: `size` của bất kỳ nút nào không nhỏ hơn `size` của tất cả các nút con (Nephew) của nút anh em (Sibling) của nó.

Ngoài ra, đối với các cây tìm kiếm có cùng tập hợp giá trị phần tử, trạng thái cân bằng có thể không duy nhất. Nghĩa là, có thể có hai cây tìm kiếm khác nhau, chứa cùng một tập hợp giá trị phần tử, và cả hai đều cân bằng.

### Quá trình điều chỉnh tính cân bằng

Thực hiện các thao tác điều chỉnh trên cây tìm kiếm không thỏa mãn điều kiện cân bằng, có thể làm cho cây mất cân bằng trở nên cân bằng trở lại.

Các thao tác điều chỉnh tính cân bằng của cây nhị phân cân bằng bao gồm **Xoay trái (Left Rotate hay zag)** và **Xoay phải (Right Rotate hay zig)**. Do các cây cân bằng nhị phân cần đảm bảo thứ tự duyệt trung thứ tự không thay đổi khi điều chỉnh, nên hai thao tác này đều không làm thay đổi thứ tự duyệt trung thứ tự.

Ở đây giới thiệu trước thao tác xoay phải, thao tác xoay phải còn được gọi là "Xoay đơn bên phải" hay "Xoay LL". Thao tác xoay phải trên nút $A$ là: cho con trái $B$ của $A$ xoay lên trên bên phải, thay thế $A$ trở thành nút gốc, nút $A$ xoay xuống dưới bên phải trở thành gốc của cây con bên phải của $B$, cây con bên phải ban đầu của $B$ trở thành cây con bên trái của $A$.

![bst-rotate](images/bst-rotate.svg)

Thao tác xoay phải chỉ thay đổi liên kết của ba nhóm nút, tương đương với việc hoán vị vòng quanh ba nhóm cạnh, do đó cần lưu tạm một nút rồi mới tiến hành luân chuyển và cập nhật.

Thứ tự cập nhật thông thường cho thao tác xoay phải là: Lưu tạm $B$ (nút gốc mới), cho con trái của $A$ trỏ tới cây con bên phải $T2$ của $B$, sau đó cho con phải của $B$ trỏ tới $A$, cuối cùng cho nút cha của $A$ trỏ tới $B$ đã lưu tạm.

Hoàn toàn tương tự, có thao tác xoay trái, còn được gọi là "Xoay đơn bên trái" hay "Xoay RR". Thao tác xoay trái là hình ảnh phản chiếu của thao tác xoay phải.

Dưới đây là mã cho thao tác xoay trái và xoay phải.

???+ note "Thực hiện"
    ```cpp
    TreeNode* rotateLeft(TreeNode* root) {
      TreeNode* newRoot = root->right;
      root->right = newRoot->left;
      newRoot->left = root;
      // Cập nhật thông tin các nút liên quan
      updateHeight(root);
      updateHeight(newRoot);
      return newRoot;  // Trả về nút gốc mới
    }
    
    TreeNode* rotateRight(TreeNode* root) {
      TreeNode* newRoot = root->left;
      root->left = newRoot->right;
      newRoot->right = root;
      updateHeight(root);
      updateHeight(newRoot);
      return newRoot;
    }
    ```

Đối với đoạn mã ví dụ này, khi gọi cần lưu lại nút cha `pre` của `root`. Hàm trả về con trỏ trỏ đến gốc mới, chỉ cần cho `pre` trỏ tới gốc mới là được.

#### Bốn tình huống phá vỡ tính cân bằng

Mặc dù định nghĩa về cây cân bằng nhị phân khác nhau, sự khác biệt giữa các cây cân bằng nhị phân chỉ nằm ở thông tin mà nút duy trì và thông tin được cập nhật sau khi xoay nút. Các tình huống phá vỡ tính cân bằng của cây nhị phân chỉ có bốn loại sau. Các thao tác điều chỉnh tính cân bằng chỉ bao gồm xoay trái và xoay phải. Dưới đây giới thiệu bốn tình huống, sau đó so sánh giữa các cây cân bằng nhị phân khác nhau.

Kiểu LL: Cây con bên trái của con trái của T bị quá dài dẫn đến phá vỡ tính cân bằng.

Thao tác điều chỉnh: Xoay phải nút T.

![bst-LL](images/bst-LL.svg)

Kiểu RR: Tương tự kiểu LL, cây con bên phải của con phải của T bị quá dài dẫn đến phá vỡ tính cân bằng.

Thao tác điều chỉnh: Xoay trái nút T.

![bst-RR](images/bst-RR.svg)

Kiểu LR: Cây con bên phải của con trái của T bị quá dài dẫn đến phá vỡ tính cân bằng.

Thao tác điều chỉnh: Xoay trái nút L trước, trở thành kiểu LL, sau đó xoay phải nút T.

![bst-LR](images/bst-LR.svg)

Kiểu RL: Tương tự kiểu LR, cây con bên trái của con phải của T bị quá dài dẫn đến phá vỡ tính cân bằng.

Thao tác điều chỉnh: Xoay phải nút R trước, trở thành kiểu RR, sau đó xoay trái nút T.

![bst-RL](images/bst-RL.svg)