author: fudonglai, AngelKitty, labuladong

Trang này sẽ giới thiệu sự khác biệt và kết hợp giữa **đệ quy** và **chia để trị** (divide and conquer).

## Đệ quy

### Định nghĩa

**Đệ quy** (tiếng Anh: Recursion) trong toán học và khoa học máy tính là phương pháp định nghĩa một hàm bằng cách sử dụng chính nó; trong khoa học máy tính còn chỉ phương pháp giải quyết bài toán bằng cách lặp lại việc chia nhỏ bài toán thành các bài toán con cùng loại.

### Dẫn nhập

> Để hiểu đệ quy, trước hết bạn phải hiểu đệ quy là gì.

Tư tưởng cơ bản của đệ quy là một hàm tự gọi lại chính nó (trực tiếp hoặc gián tiếp), nhờ đó việc giải quyết bài toán gốc được chuyển thành giải quyết nhiều bài toán con cùng tính chất nhưng kích thước nhỏ hơn. Khi giải, chỉ cần quan tâm cách chia bài toán thành các bài toán con phù hợp, không cần quá chú ý chi tiết cách giải các bài toán con đó.

Một số ví dụ giúp bạn hiểu đệ quy:

1.  [Đệ quy là gì?](./divide-and-conquer.md)
2.  Làm sao sắp xếp một dãy số? Trả lời: Chia làm hai nửa, sắp xếp nửa trái rồi nửa phải, cuối cùng gộp lại. Còn sắp xếp nửa trái/nửa phải thế nào, hãy đọc lại câu này.
3.  Bạn năm nay bao nhiêu tuổi? Trả lời: Bằng tuổi năm ngoái cộng thêm một, tôi sinh năm 1999.
4.  ![Một ví dụ giúp hiểu đệ quy](images/divide-and-conquer-1.png)

Đệ quy rất phổ biến trong toán học. Ví dụ, trong lý thuyết tập hợp, số tự nhiên được định nghĩa: 1 là số tự nhiên, mỗi số tự nhiên đều có một số liền sau, số liền sau đó cũng là số tự nhiên.

Hai đặc điểm quan trọng nhất của mã đệ quy: **điều kiện dừng** và **gọi lại chính mình**. Gọi lại chính mình là để giải quyết bài toán con, còn điều kiện dừng xác định đáp án cho bài toán nhỏ nhất.

```cpp
int func(giá trị truyền vào) {
  if (điều kiện dừng) return đáp án cho bài toán nhỏ nhất;
  return func(giảm kích thước);
}
```

### Vì sao nên viết đệ quy

1.  **Cấu trúc rõ ràng, dễ đọc.** Ví dụ, cùng một bài toán [sắp xếp trộn (merge sort)](./merge-sort.md):

    === "C++"
        ```cpp
        // ...existing code...
        ```

    === "Python"
        ```python
        # ...existing code...
        ```

    Rõ ràng, phiên bản đệ quy dễ hiểu hơn phiên bản không đệ quy. Đệ quy: sắp xếp nửa trái, sắp xếp nửa phải, rồi gộp lại. Không đệ quy: nhiều chi tiết biên khó hiểu, dễ bug, khó debug.

2.  **Rèn luyện tư duy phân tích cấu trúc bài toán.** Khi nhận ra bài toán có thể chia nhỏ thành các bài toán con cùng cấu trúc, viết đệ quy nhiều sẽ giúp bạn nhạy bén hơn với đặc điểm này.

### Nhược điểm của đệ quy

Khi chạy, đệ quy sử dụng ngăn xếp (stack). Mỗi lần gọi hàm, stack tăng thêm một frame; mỗi lần trả về, stack giảm đi một frame. Stack có giới hạn, nếu đệ quy quá sâu sẽ gây **tràn stack**.

Đôi khi đệ quy hiệu quả (ví dụ merge sort), **nhưng đôi khi không hiệu quả**, ví dụ đếm số phần tử trong danh sách liên kết, vì stack tốn bộ nhớ, trong khi lặp đơn giản không tốn thêm gì. Ví dụ:

```cpp
// Khung lặp điển hình
int size(Node *head) {
  int size = 0;
  for (Node *p = head; p != nullptr; p = p->next) size++;
  return size;
}

// Cố tình viết đệ quy, "đệ quy là nhất"
int size_recursion(Node *head) {
  if (head == nullptr) return 0;
  return size_recursion(head->next) + 1;
}
```

![\[So sánh hai cách, compiler Clang 10.0, tối ưu O1\](https://quick-bench.com/q/rZ7jWPmSdltparOO5ndLgmS9BVc)](images/divide-and-conquer-2.png "\[So sánh hai cách, compiler Clang 10.0, tối ưu O1](https://quick-bench.com/q/rZ7jWPmSdltparOO5ndLgmS9BVc)")

### Tối ưu hóa đệ quy

Trang chính: [Tối ưu hóa tìm kiếm](../search/opt.md) và [Tìm kiếm có nhớ (memoization)](../dp/memo.md)

Các cài đặt đệ quy cơ bản có thể bị lặp lại quá nhiều, dễ bị TLE. Khi đó cần tối ưu hóa đệ quy.[^ref1]

## Chia để trị

### Định nghĩa

**Chia để trị** (tiếng Anh: Divide and Conquer), đúng như tên gọi, là chia một bài toán phức tạp thành hai hoặc nhiều bài toán con giống hoặc tương tự, cho đến khi các bài toán con đủ nhỏ để giải trực tiếp, rồi hợp các đáp án con thành đáp án bài toán gốc.

### Quy trình

Tư tưởng cốt lõi của chia để trị là "chia nhỏ để giải quyết".

Quy trình gồm ba bước: **chia nhỏ -> giải quyết -> hợp lại**.

1.  Chia bài toán gốc thành các bài toán con cùng cấu trúc.
2.  Khi các bài toán con đủ nhỏ, giải quyết trực tiếp (thường bằng đệ quy).
3.  Hợp các đáp án con thành đáp án bài toán gốc.

Bài toán phù hợp với chia để trị thường có các đặc điểm:

-   Khi kích thước đủ nhỏ thì dễ giải trực tiếp.
-   Có thể chia thành nhiều bài toán con nhỏ hơn cùng loại, tức là có tính chất **tối ưu con** (optimal substructure), đáp án bài toán gốc có thể hợp từ đáp án các bài toán con.
-   Các bài toán con **độc lập** với nhau, không có bài toán con chung.

???+ warning "Lưu ý"
    Nếu các bài toán con không độc lập, chia để trị sẽ lặp lại việc giải các bài toán con chung, gây lãng phí. Khi đó, nên dùng [quy hoạch động](../dp/basic.md).

Ví dụ merge sort. Giả sử hàm `merge_sort` có nhiệm vụ **sắp xếp một mảng**. Bài toán này có thể chia nhỏ: sắp xếp nửa trái, sắp xếp nửa phải, rồi gộp lại.

```cpp
void merge_sort(một mảng) {
  if (có thể giải trực tiếp) return;
  merge_sort(nửa trái);
  merge_sort(nửa phải);
  merge(nửa trái, nửa phải);
}
```

Truyền vào nửa mảng, xử lý xong thì nửa đó đã được sắp xếp. Lưu ý, `merge_sort` rất giống với khung duyệt hậu tự cây nhị phân, vì chia để trị là **chia nhỏ -> giải quyết (đệ quy đến đáy) -> hợp lại (khi quay lui)**, tức là chia trái/phải rồi hợp lại, giống hậu tự.

Hàm `merge` thực chất giống hợp hai danh sách liên kết đã sắp xếp.

## Lưu ý

### Lưu ý khi viết đệ quy

**Phải hiểu rõ nhiệm vụ của hàm và tin tưởng nó sẽ hoàn thành nhiệm vụ đó, đừng cố "đi vào trong" hàm để xem chi tiết,** nếu không sẽ bị rối và không kiểm soát được. Bộ não con người không thể "chồng nhiều stack" như máy tính.

Ví dụ duyệt cây nhị phân:

```cpp
void traverse(TreeNode* root) {
  if (root == nullptr) return;
  traverse(root->left);
  traverse(root->right);
}
```

Chỉ vài dòng này là đủ để duyệt mọi cây nhị phân. Với hàm `traverse(root)`, chỉ cần tin rằng truyền vào một gốc `root`, nó sẽ duyệt hết cây. Việc của bạn là truyền tiếp các con vào hàm.

Tương tự với cây N phân, cách viết giống hệt cây nhị phân, chỉ khác là không có duyệt trung thứ tự.

```cpp
void traverse(TreeNode* root) {
  if (root == nullptr) return;
  for (auto child : root->children) traverse(child);
}
```

## Phân biệt

### Đệ quy và liệt kê (enumeration)

Khác biệt: Liệt kê là chia ngang bài toán, giải từng bài toán con một cách tuần tự; đệ quy là chia dọc, phân rã bài toán thành các lớp nhỏ dần.

### Đệ quy và chia để trị

Đệ quy là một kỹ thuật lập trình, một cách tư duy giải quyết vấn đề; chia để trị là một tư tưởng thuật toán cụ thể, thường dựa trên đệ quy để giải quyết các bài toán cụ thể.

## Phân tích ví dụ

???+ note "[437. Tổng các đường đi bằng K (Path Sum III)](https://leetcode-cn.com/problems/path-sum-iii/)"
    Cho một cây nhị phân, mỗi nút chứa một số nguyên.

    Hãy đếm số đường đi có tổng bằng giá trị cho trước. Đường đi không nhất thiết bắt đầu từ gốc hoặc kết thúc ở lá, nhưng phải đi từ cha xuống con.

    Cây có không quá 1000 nút, giá trị mỗi nút trong \[-1000000,1000000].

    Ví dụ:

    ```text
    root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

          10
         /  \
        5   -3
       / \    \
      3   2   11
     / \   \
    3  -2   1

    Trả về 3. Các đường đi có tổng bằng 8 là:

    1.  5 -> 3
    2.  5 -> 2 -> 1
    3. -3 -> 11
    ```
    
    ```cpp
    --8<-- "docs/basic/code/divide-and-conquer/divide-and-conquer_1.h"
    ```

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/basic/code/divide-and-conquer/divide-and-conquer_1.cpp"
    ```

??? note "Phân tích đề"
    Đề bài nhìn có vẻ phức tạp nhưng code lại rất ngắn gọn.

    Đầu tiên, giải bài toán trên cây bằng đệ quy chắc chắn phải duyệt toàn bộ cây, nên khung duyệt cây nhị phân (gọi đệ quy cho hai con) chắc chắn phải xuất hiện trong hàm chính `pathSum`. Vậy với mỗi nút, nhiệm vụ là gì? Đó là: với nút này và các nút con, có bao nhiêu đường đi thỏa mãn điều kiện.

    Theo kỹ năng đã nói, hãy xác định rõ nhiệm vụ của từng hàm đệ quy:

    -   Hàm `PathSum`: cho một nút và một giá trị mục tiêu, trả về số đường đi trong cây gốc tại nút đó có tổng bằng giá trị mục tiêu.
    -   Hàm `count`: cho một nút và một giá trị mục tiêu, trả về số đường đi bắt đầu từ nút đó có tổng bằng giá trị mục tiêu.

    ??? note "Code tham khảo (có chú thích)"
        ```cpp
        int pathSum(TreeNode *root, int sum) {
          if (root == nullptr) return 0;
          int pathImLeading = count(root, sum);  // Số đường đi bắt đầu từ chính nút này
          int leftPathSum = pathSum(root->left, sum);  // Số đường đi bên trái (tin tưởng hàm này làm đúng)
          int rightPathSum =
              pathSum(root->right, sum);  // Số đường đi bên phải (tin tưởng hàm này làm đúng)
          return leftPathSum + rightPathSum + pathImLeading;
        }
        
        int count(TreeNode *node, int sum) {
          if (node == nullptr) return 0;
          // Có thể là một đường đi độc lập không?
          int isMe = (node->val == sum) ? 1 : 0;
          // Bên trái, có bao nhiêu đường đi tổng = sum - node.val?
          int leftNode = count(node->left, sum - node->val);
          // Bên phải, có bao nhiêu đường đi tổng = sum - node.val?
          int rightNode = count(node->right, sum - node->val);
          return isMe + leftNode + rightNode;  // Tổng số đường đi bắt đầu từ node này
        }
        ```

    Nhắc lại: **Hiểu rõ nhiệm vụ của từng hàm và tin tưởng nó sẽ hoàn thành.**

    Tổng kết: `PathSum` là khung duyệt cây nhị phân, trong quá trình duyệt gọi `count` cho từng nút (ở đây là duyệt theo thứ tự trước, nhưng duyệt giữa hoặc sau cũng được). `count` cũng là một duyệt cây nhị phân, dùng để đếm số đường đi bắt đầu từ nút đó có tổng đúng giá trị mục tiêu.

## Bài luyện tập

-   [Bộ bài luyện tập đệ quy trên LeetCode](https://leetcode.com/explore/learn/card/recursion-i/)
-   [Bộ bài luyện tập chia để trị trên LeetCode](https://leetcode.com/tag/divide-and-conquer/)

## Tài liệu tham khảo & chú thích

[^ref1]: [labuladong's algorithm notebook - Giải thích chi tiết về đệ quy](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/di-gui-xiang-jie)
