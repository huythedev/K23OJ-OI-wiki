## Dẫn nhập

Cây trong lý thuyết đồ thị có hình dạng giống cây trong thực tế, chỉ khác là khi xử lý bài toán, ta thường đặt gốc cây ở phía trên. Cấu trúc dữ liệu này trông như một cái cây lộn ngược, nên được gọi là "cây".

## Định nghĩa

Một cây **vô gốc** (unrooted tree) là cây không có đỉnh gốc cố định. Có một số định nghĩa tương đương:

-   Đồ thị vô hướng liên thông có $n$ đỉnh, $n-1$ cạnh.
-   Đồ thị vô hướng liên thông không có chu trình.
-   Đồ thị vô hướng mà giữa mọi cặp đỉnh chỉ có đúng một đường đi đơn giản.
-   Đồ thị liên thông mà mọi cạnh đều là cầu.
-   Đồ thị không có chu trình, và nếu thêm một cạnh giữa hai đỉnh bất kỳ thì đồ thị thu được có đúng một chu trình.

Từ cây vô gốc, nếu chỉ định một đỉnh làm **gốc** thì thu được **cây có gốc** (rooted tree). Cây có gốc thường vẫn biểu diễn bằng đồ thị vô hướng, chỉ khác là quy ước quan hệ cha-con, xem chi tiết bên dưới.

## Một số khái niệm về cây

### Áp dụng cho cả cây vô gốc và cây có gốc

-   **Rừng (forest)**: Đồ thị mà mỗi thành phần liên thông là một cây. Theo định nghĩa này, một cây cũng là một rừng.
-   **Cây khung (spanning tree)**: Một cây con liên thông của đồ thị vô hướng, đồng thời là cây. Tức là chọn $n-1$ cạnh trong đồ thị để nối tất cả các đỉnh.
-   **Lá (leaf node) của cây vô gốc**: Đỉnh có bậc không quá $1$.

    ???+ question "Tại sao không phải là bậc đúng bằng $1$?"
        Xét trường hợp $n = 1$.

-   **Lá (leaf node) của cây có gốc**: Đỉnh không có con.

### Chỉ áp dụng cho cây có gốc

-   **Cha (parent node)**: Với mỗi đỉnh (trừ gốc), cha là đỉnh thứ hai trên đường đi từ đỉnh đó đến gốc.  
    Đỉnh gốc không có cha.
-   **Tổ tiên (ancestor)**: Các đỉnh trên đường từ một đỉnh đến gốc, trừ chính nó.  
    Gốc không có tổ tiên.
-   **Con (child node)**: Nếu $u$ là cha của $v$, thì $v$ là con của $u$.  
    Thứ tự các con thường không quan trọng, trừ cây nhị phân.
-   **Độ sâu (depth)**: Số cạnh trên đường từ đỉnh đến gốc.
-   **Chiều cao cây (height)**: Độ sâu lớn nhất trong các đỉnh.
-   **Anh em (sibling)**: Các đỉnh cùng cha là anh em.
-   **Hậu duệ (descendant)**: Con và hậu duệ của con.  
    Hoặc: nếu $u$ là tổ tiên của $v$, thì $v$ là hậu duệ của $u$.

![tree-definition.svg](images/tree-definition.svg)

-   **Cây con (subtree)**: Xóa cạnh nối với cha, phần còn lại là cây con gốc tại đỉnh đó.

    ![tree-definition-subtree.svg](images/tree-definition-subtree.svg)

## Một số loại cây đặc biệt

-   **Chuỗi (chain/path graph)**: Cây mà mọi đỉnh đều có bậc không quá $2$.
-   **Cây sao (star)**: Có một đỉnh $u$ mà mọi đỉnh còn lại đều nối trực tiếp với $u$.
-   **Cây nhị phân có gốc (rooted binary tree)**: Cây có gốc mà mỗi đỉnh có tối đa hai con. Thứ tự hai con thường được phân biệt là trái/phải.  
    Thông thường, "cây nhị phân" ngầm hiểu là cây nhị phân có gốc.
-   **Cây nhị phân đầy đủ (full/proper binary tree)**: Mỗi đỉnh hoặc là lá, hoặc có đúng hai con.  
    ![](images/tree-binary-proper.svg)
-   **Cây nhị phân hoàn chỉnh (complete binary tree)**: Chỉ hai tầng dưới cùng có thể có đỉnh bậc nhỏ hơn 2, và các đỉnh tầng dưới cùng phải nằm liên tiếp bên trái.  
    ![](images/tree-binary-complete.svg)
-   **Cây nhị phân hoàn hảo (perfect binary tree)**: Mọi lá có cùng độ sâu, mọi đỉnh không phải lá đều có đúng hai con.  
    ![](images/tree-binary-perfect.svg)

???+ warning "Cảnh báo"
    Tên tiếng Việt của Proper binary tree không thống nhất, và định nghĩa về cây nhị phân hoàn chỉnh/hoàn hảo có thể khác nhau giữa các tài liệu. Khi gặp cần đọc kỹ ngữ cảnh.

Trong cộng đồng OI, "cây nhị phân đầy đủ" thường chỉ cây nhị phân hoàn hảo.

## Lưu trữ cây

### Chỉ lưu cha

Dùng mảng `parent[N]` lưu cha của mỗi đỉnh.

Cách này chỉ lấy được ít thông tin, không tiện cho các truy vấn từ trên xuống. Thường dùng cho các bài toán quy hoạch động từ dưới lên.

### Danh sách kề

-   Với cây vô gốc: mỗi đỉnh có một danh sách các đỉnh kề.
    ```cpp
    std::vector<int> adj[N];
    ```
-   Với cây có gốc:
    -   Cách 1: Nếu đầu vào là đồ thị vô hướng, vẫn lưu như trên. Sẽ hướng dẫn cách xác định quan hệ cha-con bên dưới.
    -   Cách 2: Nếu đầu vào đã xác định rõ quan hệ cha-con, mỗi đỉnh lưu danh sách con, và có thể lưu thêm cha.
        ```cpp
        std::vector<int> children[N];
        int parent[N];
        ```
        Có thể dùng các cấu trúc khác thay cho `std::vector`.

### Biểu diễn con trái - anh phải

#### Quy trình

Với cây có gốc, có thể dùng cách này:

Đầu tiên, gán thứ tự cho các con của mỗi đỉnh.

Sau đó, với mỗi đỉnh lưu hai giá trị: **con đầu tiên** `child[u]` và **anh em kế tiếp** `sib[u]`. Nếu không có con thì `child[u]` rỗng; nếu là con cuối cùng thì `sib[u]` rỗng.

#### Cài đặt

Duyệt các con của một đỉnh như sau:

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
int v = child[u];  // Bắt đầu từ con đầu tiên
while (v != EMPTY_NODE) {
  // ...
  // Xử lý con v
  // ...
  v = sib[v];  // Sang con tiếp theo (anh em)
}
```

Hoặc viết gọn:

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
for (int v = child[u]; v != EMPTY_NODE; v = sib[v]) {
  // ...
  // Xử lý con v
  // ...
}
```

### Cây nhị phân

Cần lưu hai con trái/phải của mỗi đỉnh.

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
    int parent[N];
    int lch[N], rch[N];
    // -- hoặc --
    int child[N][2];
    ```

## Duyệt cây

### DFS trên cây

DFS trên cây là: thăm gốc, rồi lần lượt thăm từng cây con của các con.

Có thể dùng để tính độ sâu, cha của mỗi đỉnh, v.v.

### Duyệt cây nhị phân bằng DFS

#### Duyệt tiền thứ tự (preorder)

![preorder](images/tree-basic-preorder.svg)

Duyệt theo thứ tự **gốc, trái, phải**.

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
    void preorder(BiTree* root) {
      if (root) {
        cout << root->key << " ";
        preorder(root->left);
        preorder(root->right);
      }
    }
    ```

#### Duyệt trung thứ tự (inorder)

![inorder](images/tree-basic-inorder.svg)

Duyệt theo thứ tự **trái, gốc, phải**.

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
    void inorder(BiTree* root) {
      if (root) {
        inorder(root->left);
        cout << root->key << " ";
        inorder(root->right);
      }
    }
    ```

#### Duyệt hậu thứ tự (postorder)

![postorder](images/tree-basic-postorder.svg)

Duyệt theo thứ tự **trái, phải, gốc**.

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
    void postorder(BiTree* root) {
      if (root) {
        postorder(root->left);
        postorder(root->right);
        cout << root->key << " ";
      }
    }
    ```

#### Suy luận ngược

Biết hai trong ba thứ tự duyệt (trung thứ tự và một thứ tự khác) có thể xác định thứ tự còn lại.

![reverse](images/tree-basic-reverse.svg)

1.  Phần tử đầu của tiền thứ tự là `root`, phần tử cuối của hậu thứ tự là `root`.
2.  Xác định gốc, rồi dựa vào trung thứ tự, các phần bên trái là cây con trái, bên phải là cây con phải.
3.  Với mỗi cây con, lặp lại quy tắc trên.

### BFS trên cây

Bắt đầu từ gốc, thăm các đỉnh theo từng tầng.

Có thể đồng thời tính độ sâu, cha của mỗi đỉnh.

#### Duyệt theo tầng (level order)

Duyệt cây theo từng tầng từ gốc đến lá, mỗi tầng từ trái sang phải. BFS chính là một cách duyệt theo tầng, nhưng duyệt theo tầng thường yêu cầu phân biệt các tầng, nên thường trả về mảng hai chiều.

Ví dụ, cây dưới đây có kết quả duyệt theo tầng là `[[1], [2, 3, 4], [5, 6]]`.

![tree-basic-levelOrder](images/tree-basic-levelOrder.svg)

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
    vector<vector<int>> levelOrder(Node* root) {
      if (!root) {
        return {};
      }
      vector<vector<int>> res;
      queue<Node*> q;
      q.push(root);
      while (!q.empty()) {
        int currentLevelSize = q.size();  // Số đỉnh ở tầng hiện tại
        res.push_back(vector<int>());
        for (int i = 0; i < currentLevelSize; ++i) {
          Node* cur = q.front();
          q.pop();
          res.back().push_back(cur->val);
          for (Node* child : cur->children) {  // Thêm các con vào hàng đợi
            q.push(child);
          }
        }
      }
      return res;
    }
    ```

### Duyệt cây nhị phân kiểu Morris

Vấn đề cốt lõi khi duyệt cây nhị phân là: sau khi duyệt xong con, làm sao quay lại cha để tiếp tục duyệt. Cách đệ quy và không đệ quy đều dùng stack để lưu đường quay lại, nên độ phức tạp bộ nhớ tốt nhất là $O(\log n)$, xấu nhất $O(n)$ (cây là chuỗi).

Morris traversal tránh dùng stack, tận dụng con trỏ phải còn trống ở các đỉnh lá để trỏ ngược lên cha, giúp quay lại cha.

#### Quy trình Morris

Giả sử đang ở đỉnh `cur`, ban đầu là gốc.

1.  Nếu `cur` rỗng thì dừng, ngược lại tiếp tục.
2.  Nếu `cur` không có con trái, chuyển sang phải (`cur = cur->right`).
3.  Nếu `cur` có con trái, tìm đỉnh phải nhất trong cây con trái, gọi là `mostRight`.
    -   Nếu `mostRight->right` rỗng, gán nó trỏ về `cur`, rồi sang trái (`cur = cur->left`).
    -   Nếu `mostRight->right` trỏ về `cur`, gán lại thành `nullptr`, rồi sang phải (`cur = cur->right`).

Ví dụ, bắt đầu từ đỉnh 1.

![tree-basic-morris-1](images/tree-basic-morris-1.svg)

Lần đầu đến đỉnh 2, tìm con phải nhất là 4, gán 4 trỏ về 2.

![tree-basic-morris-2](images/tree-basic-morris-2.svg)

Sau đó, qua 4 quay lại 2, lần hai đến 2, lại tìm 4, gán lại trỏ về nullptr, rồi sang phải. Quá trình tiếp theo tương tự.

![tree-basic-morris-1](images/tree-basic-morris-1.svg)

Thứ tự thăm: `1242513637`. Đỉnh có con trái được thăm hai lần, không có thì một lần.

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
    void morris(TreeNode* root) {
      TreeNode* cur = root;
      while (cur) {
        if (!cur->left) {
          // Nếu không có con trái, in giá trị rồi sang phải
          std::cout << cur->val << " ";
          cur = cur->right;
          continue;
        }
        // Tìm con phải nhất của cây con trái
        TreeNode* mostRight = cur->left;
        while (mostRight->right && mostRight->right != cur) {
          mostRight = mostRight->right;
        }
        if (!mostRight->right) {
          // Nếu con phải nhất chưa trỏ về cur, gán rồi sang trái
          mostRight->right = cur;
          cur = cur->left;
        } else {
          // Nếu đã trỏ về cur, gán lại nullptr, in giá trị rồi sang phải
          mostRight->right = nullptr;
          std::cout << cur->val << " ";
          cur = cur->right;
        }
      }
    }
    ```

### Cây vô gốc

#### Quy trình

Duyệt cây thường là DFS, cần tránh thăm lại đỉnh đã đi qua.

Vì cây không có chu trình, chỉ cần nhớ đỉnh đến từ đâu, khi duyệt thì bỏ qua đỉnh đó.

???+ note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-basic.md
    void dfs(int u, int from) {
      // Đệ quy vào tất cả các đỉnh kề trừ from
      // Với đỉnh xuất phát, from rỗng nên sẽ duyệt hết các đỉnh kề
      for (int v : adj[u])
        if (v != from) {
          dfs(v, u);
        }
    }
    
    // Bắt đầu duyệt
    int EMPTY_NODE = -1;  // Giá trị không tồn tại
    int root = 0;         // Chọn một đỉnh bất kỳ làm gốc
    dfs(root, EMPTY_NODE);
    ```

### Cây có gốc

Với cây có gốc, cần xác định quan hệ cha-con.

Như trên, khi duyệt từ gốc, mỗi lần đến một đỉnh, biến `from` chính là cha của đỉnh đó.

Nhờ vậy, từ đầu vào là đồ thị vô hướng, ta có thể xác định cha và danh sách con của từng đỉnh.

**Một phần nội dung trang này tham khảo bài viết [二叉树：前序遍历、中序遍历、后续遍历](https://blog.csdn.net/weixin_43357638/article/details/99730284), tuân thủ giấy phép CC 4.0 BY-SA.**
