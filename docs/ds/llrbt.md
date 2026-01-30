author: c-forrest, Enter-tainer, giiiiiithub, hly1204, iamtwz, Ir1d, kigawas, ksyx, luxuryspark567, mgt, orzAtalod, sandyzikun, SunsetGlow95, Tiphereth-A, current2020, untitledunrevised, yuhuoji

Cây đỏ đen lệch trái (Left Leaning Red Black Tree - LLRBT) là một biến thể của [Cây đỏ đen](./rbtree.md). Nó áp dụng các ràng buộc nhất định đối với vị trí của các cạnh (nút) màu đỏ, khiến các thao tác chèn và xóa của nó có thể tạo ra sự tương ứng một-một với [Cây 2-3](https://en.wikipedia.org/wiki/2%E2%80%933_tree).

Giả định rằng người đọc đã nắm vững ít nhất một loại cây cân bằng dựa trên phép xoay, vì vậy bài viết này sẽ không đi sâu vào giải thích các thao tác xoay.

## Cây đỏ đen (Red Black Tree)

### Tính chất

Một cây đỏ đen thỏa mãn các tính chất sau:

1.  Mỗi nút có màu đỏ hoặc đen;
2.  Nút NIL (nút lá rỗng) có màu đen;
3.  Tất cả các con của một nút màu đỏ phải có màu đen, nghĩa là trên bất kỳ đường đi nào từ mỗi lá đến gốc không thể có hai nút màu đỏ liên tiếp;
4.  Mọi đường dẫn đơn từ bất kỳ nút nào đến tất cả các lá trong cây con của nó đều chứa cùng số lượng nút đen. (Cân bằng chiều cao đen)

Điều này đảm bảo rằng đường đi dài nhất từ gốc đến bất kỳ lá nào (xen kẽ đỏ đen) sẽ không vượt quá hai lần đường đi ngắn nhất (toàn đen), từ đó đảm bảo tính cân bằng của cây.

Việc duy trì các tính chất này khá phức tạp. Nếu chúng ta muốn chèn một nút, trước hết, nó chắc chắn sẽ được tô màu đỏ, nếu không sẽ vi phạm tính chất 4. Ngay cả như vậy, chúng ta vẫn có thể vi phạm tính chất 3. Do đó cần phải thực hiện điều chỉnh. Việc xóa nút thậm chí còn rắc rối hơn, tương tự như chèn, chúng ta không thể xóa nút màu đen, nếu không sẽ phá vỡ sự cân bằng chiều cao đen. Làm thế nào để giải quyết những vấn đề này một cách thuận tiện?

## Cây đỏ đen lệch trái (Left Leaning Red Black Tree)

### Giải thích

Cây đỏ đen lệch trái là một biến thể của cây đỏ đen dễ cài đặt hơn.

Trong các sơ đồ cây đỏ đen lệch trái dưới đây, các cạnh có màu thay vì các nút. Chúng ta quy ước màu của một nút là màu của cạnh nối với cha của nó.

Cây đỏ đen lệch trái áp dụng thêm các ràng buộc cho cây đỏ đen: các con trái và phải của một nút màu đen:

-   Hoặc là toàn màu đen;
-   Hoặc con trái là màu đỏ, con phải là màu đen.

Trường hợp thỏa mãn điều kiện:

![llrbt1](./images/llrbt-1.png)

Trường hợp không thỏa mãn điều kiện:

![llrbt2](./images/llrbt-2.png)

Đây là tính chất "lệch trái" của cây lệch trái: cạnh màu đỏ chỉ có thể lệch về bên trái.

### Quá trình

#### Chèn

Đầu tiên, chúng ta sử dụng phương pháp chèn BST thông thường, chèn một nút lá màu đỏ vào đáy cây, sau đó thông qua việc điều chỉnh từ dưới lên trên, làm cho cây sau khi chèn vẫn tuân thủ các tính chất của cây đỏ đen lệch trái. Dưới đây mô tả quá trình điều chỉnh:

![llrbt3](./images/llrbt-3.png)

Sau khi chèn, có thể xuất hiện một cạnh màu đỏ lệch phải, do đó cần thực hiện một phép xoay trái đối với trường hợp cạnh đỏ lệch phải:

![llrbt4](./images/llrbt-4.png)

Xem xét sau khi xoay trái sẽ tạo ra hai cạnh đỏ lệch trái liên tiếp:

![llrbt5](./images/llrbt-5.png)

Do đó cần thực hiện một phép xoay phải đối với nó. Và đối với trường hợp sau khi xoay phải, chúng ta nên thực hiện `color_flip` cho nó: tức là đảo ngược màu của nút đó và hai con của nó

![llrbt6](./images/llrbt-6.png)

Từ đó loại bỏ cạnh đỏ lệch phải.

??? note "Mã tham khảo (một phần)"
    ```cpp
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::fix_up(
        Set::Node *root) const {
      if (is_red(root->rc) && !is_red(root->lc))  // fix right leaned red link
        root = rotate_left(root);
      if (is_red(root->lc) &&
          is_red(root->lc->lc))  // fix doubly linked left leaned red link
        // if (root->lc == nullptr), then the second expr won't be evaluated
        root = rotate_right(root);
      if (is_red(root->lc) && is_red(root->rc))
        // break up 4 node
        color_flip(root);
      root->size = size(root->lc) + size(root->rc) + 1;
      return root;
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node_Set<Key, Compare>::insert(
        Set::Node_root, const Key &key) const {
      if (root == nullptr) return new Node(key, kRed, 1);
      if (root->key == key)
        ;
      else if (cmp\_(key, root->key))  // if (key < root->key)
        root->lc = insert(root->lc, key);
      else
        root->rc = insert(root->rc, key);
      return fix_up(root);
    }
    ```

#### Xóa

Thao tác xóa dựa trên tư tưởng này: chúng ta không thể xóa các nút màu đen, vì điều này sẽ phá vỡ chiều cao đen. Vì vậy, chúng ta cần đảm bảo rằng nút cuối cùng chúng ta xóa là màu đỏ.

##### Xóa nút có giá trị nhỏ nhất

Trước hết hãy thử xóa nút nhỏ nhất trong toàn bộ cây.

Làm thế nào để đảm bảo nút cuối cùng bị xóa là màu đỏ? Chúng ta cần đảm bảo một tính chất trong quá trình đệ quy đi xuống: nếu nút hiện tại là `h`, thì cần đảm bảo `h` là màu đỏ, hoặc `h->lc` là màu đỏ.

Xem xét tính đúng đắn của việc này, nếu chúng ta có thể duy trì tính chất này thông qua các thao tác xoay và đảo màu khác nhau, thì khi chúng ta đến nút nhỏ nhất `h_min`, ta có `h_min` là màu đỏ, hoặc con trái của `h_min` - nhưng `h_min` hoàn toàn không có con trái! Vì vậy điều này đảm bảo nút nhỏ nhất chắc chắn là màu đỏ, vì nó là màu đỏ, chúng ta có thể mạnh dạn xóa nó, sau đó sử dụng cùng tư tưởng điều chỉnh như thao tác chèn để điều chỉnh cây.

Bây giờ chúng ta hãy xem xét làm thế nào để thỏa mãn tính chất này, lưu ý rằng chúng ta sẽ **tạm thời** phá vỡ một số tính chất của cây đỏ đen lệch trái khi đệ quy đi xuống, nhưng khi chúng ta quay trở lại từ đệ quy, chúng ta sẽ khôi phục chúng.

Như được mô tả trong hình dưới đây, là một trường hợp tương đối đơn giản, lúc này `h->rc->lc` là màu đen, chúng ta chỉ cần một lần đảo màu là đủ:

![llrbt-7](./images/llrbt-7.png)

Hơn nữa, sau khi đảo màu như trên, sẽ không làm cho `h->rc` và `h->rc->lc` tạo thành cạnh đỏ liên tiếp;

Nhưng nếu `h->rc->lc` là màu đỏ, tình huống sẽ phức tạp hơn:

![llrbt-8](./images/llrbt-8.png)

Nếu chỉ thực hiện đảo màu, sẽ tạo ra cạnh đỏ liên tiếp, và xem xét khi chúng ta quay trở lại từ đệ quy, chúng ta không thể sửa chữa tình huống như vậy, do đó cần phải xử lý.

Sau đó có thể tiến hành xóa:

??? note "Mã tham khảo (một phần)"
    ```cpp
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::move_red_left(
        Set::Node *root) const {
      color_flip(root);
      if (is_red(root->rc->lc)) {
        // assume that root->rc != nullptr when calling this function
        root->rc = rotate_right(root->rc);
        root = rotate_left(root);
        color_flip(root);
      }
      return root;
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::delete_min(
        Set::Node *root) const {
      if (root->lc == nullptr) {
        delete root;
        return nullptr;
      }
      if (!is_red(root->lc) && !is_red(root->lc->lc)) {
        // make sure either root->lc or root->lc->lc is red
        // thus make sure we will delete a red node in the end
        root = move_red_left(root);
      }
      root->lc = delete_min(root->lc);
      return fix_up(root);
    }
    ```

##### Xóa nút bất kỳ

Đầu tiên chúng ta xem xét xóa lá: tương tự như xóa giá trị nhỏ nhất, trong quá trình xóa giá trị bất kỳ chúng ta cũng phải duy trì một tính chất, nhưng lần này đặc biệt hơn, vì chúng ta không chỉ đi sang trái, mà có thể đi sang cả hai hướng trái phải, do đó tính chất được duy trì trong quá trình xóa là như sau: nếu đi sang trái, nút hiện tại là `h`, thì cần đảm bảo `h` là màu đỏ, hoặc `h->lc` là màu đỏ; nếu đi sang phải, nút hiện tại là `h`, thì cần đảm bảo `h` là màu đỏ, hoặc `h->rc` là màu đỏ. Điều này đảm bảo chúng ta cuối cùng sẽ luôn xóa một nút màu đỏ.

Tiếp theo xem xét xóa nút không phải lá, chúng ta chỉ cần tìm nút nhỏ nhất trong cây con phải của nó (nếu có), sau đó thay thế giá trị của nút đó bằng giá trị của nút nhỏ nhất trong cây con phải, cuối cùng xóa nút nhỏ nhất trong cây con phải.

![llrbt-9](./images/llrbt-9.png)

Vậy nếu không có cây con phải thì sao? Chúng ta cần xoay cây con trái sang, như vậy sẽ không xuất hiện vấn đề này nữa.

??? note "Mã tham khảo (một phần)"
    ```cpp
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::delete_arbitrary(
        Set::Node *root, Key key) const {
      if (cmp_(key, root->key)) {
        // key < root->key
        if (!is_red(root->lc) && !(is_red(root->lc->lc)))
          root = move_red_left(root);
        // ensure the invariant: either root->lc or root->lc->lc (or root and
        // root->lc after dive into the function) is red, to ensure we will
        // eventually delete a red node. therefore we will not break the black
        // height balance
        root->lc = delete_arbitrary(root->lc, key);
      } else {
        // key >= root->key
        if (is_red(root->lc)) root = rotate_right(root);
        if (key == root->key && root->rc == nullptr) {
          delete root;
          return nullptr;
        }
        if (!is_red(root->rc) && !is_red(root->rc->lc)) root = move_red_right(root);
        if (key == root->key) {
          root->key = get_min(root->rc);
          root->rc = delete_min(root->rc);
        } else {
          root->rc = delete_arbitrary(root->rc, key);
        }
      }
      return fix_up(root);
    }
    ```

## Cài đặt

Mã dưới đây được cài đặt bằng cây đỏ đen lệch trái cho `Set`, tức là tập hợp có thứ tự không trùng lặp:

??? note "Mã tham khảo"
    ```cpp
    #include <algorithm>
    #include <memory>
    #include <vector>
    
    template <class Key, class Compare = std::less<Key>>
    class Set {
     private:
      enum NodeColor { kBlack = 0, kRed = 1 };
    
      struct Node {
        Key key;
        Node *lc{nullptr}, *rc{nullptr};
        size_t size{0};
        NodeColor color;  // the color of the parent link
    
        Node(Key key, NodeColor color, size_t size)
            : key(key), color(color), size(size) {}
    
        Node() = default;
      };
    
      void destroyTree(Node *root) const {
        if (root != nullptr) {
          destroyTree(root->lc);
          destroyTree(root->rc);
          root->lc = root->rc = nullptr;
          delete root;
        }
      }
    
      bool is_red(const Node *nd) const {
        return nd == nullptr ? false : nd->color;  // kRed == 1, kBlack == 0
      }
    
      size_t size(const Node *nd) const { return nd == nullptr ? 0 : nd->size; }
    
      Node *rotate_left(Node *node) const {
        // left rotate a red link
        //          <1>                   <2>
        //        /    \\               //    \
        //       *      <2>    ==>     <1>     *
        //             /   \          /   \
        //            *     *        *     *
        Node *res = node->rc;
        node->rc = res->lc;
        res->lc = node;
        res->color = node->color;
        node->color = kRed;
        res->size = node->size;
        node->size = size(node->lc) + size(node->rc) + 1;
        return res;
      }
    
      Node *rotate_right(Node *node) const {
        // right rotate a red link
        //            <1>               <2>
        //          //    \           /    \\
        //         <2>     *   ==>   *      <1>
        //        /   \                    /   \
        //       *     *                  *     *
        Node *res = node->lc;
        node->lc = res->rc;
        res->rc = node;
        res->color = node->color;
        node->color = kRed;
        res->size = node->size;
        node->size = size(node->lc) + size(node->rc) + 1;
        return res;
      }
    
      NodeColor neg_color(NodeColor n) const { return n == kBlack ? kRed : kBlack; }
    
      void color_flip(Node *node) const {
        node->color = neg_color(node->color);
        node->lc->color = neg_color(node->lc->color);
        node->rc->color = neg_color(node->rc->color);
      }
    
      Node *insert(Node *root, const Key &key) const;
      Node *delete_arbitrary(Node *root, Key key) const;
      Node *delete_min(Node *root) const;
      Node *move_red_right(Node *root) const;
      Node *move_red_left(Node *root) const;
      Node *fix_up(Node *root) const;
      const Key &get_min(Node *root) const;
      void serialize(Node *root, std::vector<Key> *) const;
      void print_tree(Set::Node *root, int indent) const;
      Compare cmp_ = Compare();
      Node *root_{nullptr};
    
     public:
      using KeyType = Key;
      using ValueType = Key;
      using SizeType = std::size_t;
      using DifferenceType = std::ptrdiff_t;
      using KeyCompare = Compare;
      using ValueCompare = Compare;
      using Reference = Key &;
      using ConstReference = const Key &;
    
      Set() = default;
    
      Set(Set &) = default;
    
      Set(Set &&) noexcept = default;
    
      ~Set() { destroyTree(root_); }
    
      SizeType size() const;
    
      SizeType count(const KeyType &key) const;
    
      SizeType erase(const KeyType &key);
    
      void clear();
    
      void insert(const KeyType &key);
    
      bool empty() const;
    
      std::vector<Key> serialize() const;
    
      void print_tree() const;
    };
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::SizeType Set<Key, Compare>::count(
        ConstReference key) const {
      Node *x = root_;
      while (x != nullptr) {
        if (key == x->key) return 1;
        if (cmp_(key, x->key))  // if (key < x->key)
          x = x->lc;
        else
          x = x->rc;
      }
      return 0;
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::SizeType Set<Key, Compare>::erase(
        const KeyType &key) {
      if (count(key) > 0) {
        if (!is_red(root_->lc) && !(is_red(root_->rc))) root_->color = kRed;
        root_ = delete_arbitrary(root_, key);
        if (root_ != nullptr) root_->color = kBlack;
        return 1;
      } else {
        return 0;
      }
    }
    
    template <class Key, class Compare>
    void Set<Key, Compare>::clear() {
      destroyTree(root_);
      root_ = nullptr;
    }
    
    template <class Key, class Compare>
    void Set<Key, Compare>::insert(const KeyType &key) {
      root_ = insert(root_, key);
      root_->color = kBlack;
    }
    
    template <class Key, class Compare>
    bool Set<Key, Compare>::empty() const {
      return size(root_) == 0;
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::insert(
        Set::Node *root, const Key &key) const {
      if (root == nullptr) return new Node(key, kRed, 1);
      if (root->key == key)
        ;
      else if (cmp_(key, root->key))  // if (key < root->key)
        root->lc = insert(root->lc, key);
      else
        root->rc = insert(root->rc, key);
      return fix_up(root);
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::delete_min(
        Set::Node *root) const {
      if (root->lc == nullptr) {
        delete root;
        return nullptr;
      }
      if (!is_red(root->lc) && !is_red(root->lc->lc)) {
        // make sure either root->lc or root->lc->lc is red
        // thus make sure we will delete a red node in the end
        root = move_red_left(root);
      }
      root->lc = delete_min(root->lc);
      return fix_up(root);
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::move_red_right(
        Set::Node *root) const {
      color_flip(root);
      if (is_red(root->lc->lc)) {  // assume that root->lc != nullptr when calling
                                   // this function
        root = rotate_right(root);
        color_flip(root);
      }
      return root;
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::move_red_left(
        Set::Node *root) const {
      color_flip(root);
      if (is_red(root->rc->lc)) {
        // assume that root->rc != nullptr when calling this function
        root->rc = rotate_right(root->rc);
        root = rotate_left(root);
        color_flip(root);
      }
      return root;
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::fix_up(
        Set::Node *root) const {
      if (is_red(root->rc) && !is_red(root->lc))  // fix right leaned red link
        root = rotate_left(root);
      if (is_red(root->lc) &&
          is_red(root->lc->lc))  // fix doubly linked left leaned red link
        // if (root->lc == nullptr), then the second expr won't be evaluated
        root = rotate_right(root);
      if (is_red(root->lc) && is_red(root->rc))
        // break up 4 node
        color_flip(root);
      root->size = size(root->lc) + size(root->rc) + 1;
      return root;
    }
    
    template <class Key, class Compare>
    const Key &Set<Key, Compare>::get_min(Set::Node *root) const {
      Node *x = root;
      // will crash as intended when root == nullptr
      for (; x->lc != nullptr; x = x->lc);
      return x->key;
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::SizeType Set<Key, Compare>::size() const {
      return size(root_);
    }
    
    template <class Key, class Compare>
    typename Set<Key, Compare>::Node *Set<Key, Compare>::delete_arbitrary(
        Set::Node *root, Key key) const {
      if (cmp_(key, root->key)) {
        // key < root->key
        if (!is_red(root->lc) && !(is_red(root->lc->lc)))
          root = move_red_left(root);
        // ensure the invariant: either root->lc or root->lc->lc (or root and
        // root->lc after dive into the function) is red, to ensure we will
        // eventually delete a red node. therefore we will not break the black
        // height balance
        root->lc = delete_arbitrary(root->lc, key);
      } else {
        // key >= root->key
        if (is_red(root->lc)) root = rotate_right(root);
        if (key == root->key && root->rc == nullptr) {
          delete root;
          return nullptr;
        }
        if (!is_red(root->rc) && !is_red(root->rc->lc)) root = move_red_right(root);
        if (key == root->key) {
          root->key = get_min(root->rc);
          root->rc = delete_min(root->rc);
        } else {
          root->rc = delete_arbitrary(root->rc, key);
        }
      }
      return fix_up(root);
    }
    
    template <class Key, class Compare>
    std::vector<Key> Set<Key, Compare>::serialize() const {
      std::vector<int> v;
      serialize(root_, &v);
      return v;
    }
    
    template <class Key, class Compare>
    void Set<Key, Compare>::serialize(Set::Node *root,
                                      std::vector<Key> *res) const {
      if (root == nullptr) return;
      serialize(root->lc, res);
      res->push_back(root->key);
      serialize(root->rc, res);
    }
    
    template <class Key, class Compare>
    void Set<Key, Compare>::print_tree(Set::Node *root, int indent) const {
      if (root == nullptr) return;
      print_tree(root->lc, indent + 4);
      std::cout << std::string(indent, '-') << root->key << std::endl;
      print_tree(root->rc, indent + 4);
    }
    
    template <class Key, class Compare>
    void Set<Key, Compare>::print_tree() const {
      print_tree(root_, 0);
    }
    ```

## Quan hệ với Cây 2-3

Cây 2-3 là B-tree bậc 3, mỗi nút là nút 2 hoặc nút 3, lưu trữ một hoặc hai phần tử dữ liệu. Nút 2 và nút 3 không phải là lá lần lượt chỉ có thể có hai hoặc ba con. Hơn nữa, tất cả dữ liệu được lưu trữ trong Cây 2-3 đều có thứ tự.

Cây 2-3 và cây đỏ đen lệch trái về bản chất là tương đương. Một nút trong Cây 2-3 có thể lưu trữ 1 phần tử hoặc 2 phần tử, trong khi một nút của cây đỏ đen chỉ có thể lưu trữ một phần tử. Như hình dưới đây, nút 2 của Cây 2-3 tương ứng với một nút đen, nút 3 tương ứng với một nút đỏ và một nút đen (có thể xem bc là song song).

![2-3-tree-rbt](images/2-3-tree-rbt-1.svg)

![2-3-tree-rbt](images/2-3-tree-rbt-2.svg)

Hình dưới đây là cây đỏ đen lệch trái tương ứng với một Cây 2-3.

![2-3-tree-rbt](images/2-3-tree-rbt-3.svg)

Các thao tác chèn và xóa của Cây 2-3 và cây đỏ đen lệch trái là tương ứng một-một. [^23-vs-llrbt]

## Tài liệu tham khảo và đọc thêm

-   [Left-Leaning Red-Black Trees](https://sedgewick.io/wp-content/themes/sedgewick/papers/2008LLRB.pdf)-  Robert Sedgewick Princeton University
-   [Balanced Search Trees](https://algs4.cs.princeton.edu/lectures/keynote/33BalancedSearchTrees-2x2.pdf)-\_Algorithms\_Robert Sedgewick | Kevin Wayne

[^23-vs-llrbt]: [Bài viết blog này](https://riteme.site/blog/2016-3-12/2-3-tree-and-red-black-tree.html) cung cấp mô tả chi tiết. "Cây đỏ đen" trong bài viết thực chất đề cập đến "Cây đỏ đen lệch trái".