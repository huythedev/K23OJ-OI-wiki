tác giả: Backl1ght

Euler Tour Tree (Cây Du lịch Euler, Cây Chu trình Euler, sau đây gọi tắt là ETT) là một cấu trúc dữ liệu có thể giải quyết các bài toán **cây động** (dynamic tree). ETT chuyển các thao tác của cây động thành các thao tác trên đoạn của dãy thứ tự DFS của nó, sau đó dùng các cấu trúc dữ liệu khác để duy trì các thao tác trên đoạn của dãy, từ đó duy trì các thao tác của cây động. Ví dụ, ETT chuyển thao tác thêm cạnh của cây động thành nhiều thao tác tách dãy và hợp dãy, nếu có thể duy trì các thao tác tách dãy và hợp dãy, có thể duy trì thao tác thêm cạnh của cây động.

LCT (Link/Cut Tree) cũng là một cấu trúc dữ liệu có thể giải quyết các bài toán cây động, so với ETT thì LCT phổ biến hơn. LCT thực sự thích hợp hơn để duy trì thông tin trên **đường đi** (path), trong khi ETT thích hợp hơn để duy trì thông tin **cây con** (subtree). Ví dụ, ETT có thể duy trì giá trị nhỏ nhất trên cây con mà LCT không thể.

ETT có thể sử dụng bất kỳ cấu trúc dữ liệu nào để duy trì, chỉ cần cấu trúc dữ liệu đó hỗ trợ các thao tác trên đoạn dãy tương ứng, và thỏa mãn yêu cầu về độ phức tạp. Thông thường sẽ sử dụng các cây tìm kiếm nhị phân cân bằng như Splay, Treap để duy trì các thao tác trên đoạn, mà các cấu trúc dữ liệu này duy trì các thao tác trên đoạn với độ phức tạp đều là $O(\log n)$, từ đó có thể duy trì các thao tác cây động trong thời gian $O(\log n)$. Nếu sử dụng cây tìm kiếm cân bằng đa ngôi (multiway balanced search tree) ví dụ như B-tree để duy trì các thao tác trên đoạn, cũng có thể đạt được độ phức tạp tốt hơn.

Thực ra ETT có thể được hiểu là một tư tưởng, đó là thông qua việc duy trì một dãy nào đó tương ứng một-một với cây gốc, từ đó đạt được mục đích duy trì cây gốc. Bài viết này chỉ giới thiệu một số cách triển khai và ứng dụng khả thi của tư tưởng này.

## Biểu diễn Chu trình Euler của cây

Nếu coi một cạnh của cây là hai cạnh có hướng, thì có thể biểu diễn cây thành một chu trình Euler của đồ thị có hướng, gọi là Biểu diễn Chu trình Euler của cây (Euler tour representation, ETR).

Dãy cần duy trì sau này thực chất là một biến thể của ETR, coi các nút trong cây như là vòng lặp tự thân cũng được thêm vào ETR, nhưng do tác giả trong bài báo gốc không đặt tên mới cho nó, nên vẫn gọi nó là ETR.

Có thể thu được Biểu diễn Chu trình Euler của cây $T$ thông qua thuật toán sau:

$$
\begin{array}{ll}
1 & \textbf{Input. } \text{Một cây có gốc }T\\
2 & \textbf{Output. } \text{Dãy DFS của cây có gốc }T\\
3 & \operatorname{ET}(u)\\
4 & \qquad \text{thăm đỉnh }u\\
5 & \qquad \text{cho tất cả con } v \text{ của } u\\
6 & \qquad \qquad \text{thăm cạnh có hướng } u \to v\\
7 & \qquad \qquad \operatorname{ET}(v)\\
8 & \qquad \qquad \text{thăm cạnh có hướng } v \to u\\
\end{array}
$$

Biểu diễn Chu trình Euler $\operatorname{ETR}(T)$ của cây $T$ ban đầu là rỗng. Trong quá trình DFS, mỗi lần thăm một nút hoặc một cạnh có hướng thì thêm nó vào cuối $\operatorname{ETR}(T)$, như vậy có thể thu được $\operatorname{ETR}(T)$.

Nếu $T$ chứa $n$ nút, thì chứa $2n - 2$ cạnh có hướng, mà trong quá trình DFS, mỗi nút và mỗi cạnh có hướng đều được thăm một lần, nên độ dài của $\operatorname{ETR}(T)$ là $3n - 2$.

Coi nút $u$ như một vòng lặp tự thân, khi đó $\operatorname{ETR}(T)$ có thể coi là một chu trình Euler trong đồ thị có hướng. Có thể cắt tại một điểm nào đó trong chu trình Euler, biến nó thành một chuỗi các cạnh nối đầu cuối với nhau; cũng có thể dán lại chỗ bị cắt để biến chuỗi này trở lại thành chu trình Euler; cũng có thể thông qua việc thêm một số cạnh để ghép hai chuỗi như vậy thành một chu trình Euler mới.

Trong phần sau, nếu không nói rõ, dãy được duy trì mặc định là biểu diễn chu trình Euler của cây.

## Các thao tác cơ bản của ETT

3 thao tác sau được coi là các thao tác cơ bản của ETT, đều có thể được chuyển đổi thành số lần thao tác trên dãy là hằng số, vì vậy độ phức tạp của 3 thao tác này cùng bậc với thao tác trên dãy.

Ở đây chỉ đưa ra một cách triển khai khả thi, chỉ cần có thể ghép dãy tương ứng với thao tác sau khi sửa đổi bằng số lần thao tác trên dãy là hằng số là được.

### MakeRoot(u)

Tức là thao tác đổi gốc. Thao tác đổi gốc trong ETT được chuyển đổi thành 1 thao tác tách dãy và 1 thao tác hợp dãy, cũng có thể hiểu là 1 thao tác dịch chuyển đoạn.

Ký hiệu cây chứa nút $u$ là $T$, gốc hiện tại là $r$, bây giờ muốn đổi gốc thành $u$. Dãy tương ứng với cây $T$ là $L$, tách $L$ tại $(u, u)$ thành dãy $L^1$ và $L^2$, phần trước chứa các phần tử trước $(u, u)$ trong $L$ và $(u, u)$, phần sau chứa các phần tử còn lại. Dãy thu được bằng cách hợp nhất lần lượt $L^2$ và $L^1$ là dãy tương ứng với cây sau khi đổi gốc.

Ở đây có thể hiểu là thao tác xoay chu trình Euler. Chu trình Euler là một vòng tròn, việc xoay không làm thay đổi cấu trúc của chu trình Euler, tức là không làm thay đổi cấu trúc cây, chỉ là đưa nút $u$ xoay đến vị trí gốc.

### Insert(u, v)

Tức là thao tác thêm cạnh. Thao tác thêm cạnh trong ETT được chuyển đổi thành 2 thao tác tách dãy và 5 thao tác hợp dãy.

Ký hiệu cây chứa nút $u$ là $T_1$, cây chứa nút $v$ là $T_2$, sau khi thêm cạnh hai cây hợp nhất thành một cây $T$. Dãy tương ứng với cây $T_1$ là $L_1$, dãy tương ứng với cây $T_2$ là $L_2$.

Tách $L_1$ tại $(u, u)$ thành dãy $L_1^1$ và $L_1^2$, phần trước chứa các phần tử trước $(u, u)$ trong $L_1$ và $(u, u)$, phần sau chứa các phần tử còn lại. Tương tự, tách $L_2$ tại $(v, v)$ thành dãy $L_2^1$ và $L_2^2$. Dãy tương ứng với cây $T$ là $L$ thu được bằng cách hợp nhất lần lượt $L_1^2, L_1^1, [(u, v)], L_2^2, L_2^1, [(v, u)]$.

Ở đây có thể hiểu là hai thao tác đổi gốc, sau đó ngắt hai chu trình Euler tại vị trí gốc hiện tại, rồi dùng hai cạnh có hướng mới để ghép hai chu trình Euler thành một chu trình Euler mới.

### Delete(u, v)

Tức là thao tác xóa cạnh. Thao tác xóa cạnh trong ETT được chuyển đổi thành 4 thao tác tách dãy và 1 thao tác hợp dãy.

Ký hiệu cây chứa cạnh $(u, v)$ và cạnh $(v, u)$ là $T$, dãy tương ứng là $L$. Sau khi xóa cạnh, $T$ được chia thành hai cây.

Tách $L$ thành $L_1, [(u, v)], L_2, [(v, u)], L_3$. Dãy tương ứng với hai cây tạo thành sau khi xóa cạnh lần lượt là $L_2$ và $L_1, L_3$. Chú ý, trong dãy $L$, $[(u, v)]$ có thể xuất hiện sau $[(v, u)]$, khi đó có thể đổi giá trị $u$ và $v$ rồi mới thao tác.

Ở đây có thể hiểu là cắt một chu trình Euler tại hai cạnh có hướng để tạo thành hai chuỗi, sau đó hai chuỗi tự nối đầu cuối để tạo thành hai chu trình Euler mới.

## Triển khai

Dưới đây lấy Treap không xoay (non-splay Treap) làm ví dụ để giới thiệu cách triển khai ETT, đòi hỏi người đọc cần biết trước về nội dung liên quan đến thao tác trên đoạn của cây tìm kiếm nhị phân không xoay.

`Split` và `Merge` đều là các thao tác cơ bản của Treap không xoay, ở đây không nhắc lại.

### SplitUp2(u)

Giả sử dãy mà $u$ thuộc về là $L$, tách $L$ tại $u$ thành dãy $L^1$ và $L^2$, phần trước chứa các phần tử trước $(u, u)$ trong $L$ và $(u, u)$, phần sau chứa các phần tử còn lại.

Nếu mỗi nút trong Treap còn duy trì cha của nó, thì có thể thực hiện tính toán vị trí của phần tử tương ứng với nút Treap trong dãy trong thời gian $O(\log n)$, sau đó dựa vào vị trí để `Split` có thể thực hiện chức năng trên.

Cũng có thể tách từ dưới lên để thực hiện chức năng trên, cách này so với phương pháp trên sẽ hiệu quả hơn. Cụ thể là, trong quá trình nhảy từ nút $u$ lên gốc, dựa vào tính chất của cây tìm kiếm nhị phân có thể xác định mỗi nút trong $L$ nằm trước hay sau $u$, dựa vào điều này có thể tính toán vị trí của $u$ trong dãy, cũng có thể xác định mỗi nút thuộc về cây nào trong các cây sau khi tách.

```cpp
/*
 * Bottom up split treap p into 2 treaps a and b.
 *   - a: a treap containing nodes with position less than or equal to p.
 *   - b: a treap containing nodes with postion greater than p.
 *
 * In the other word, split sequence containning p into two sequences, the first
 * one contains elements before p and element p, the second one contains
 * elements after p.
 */
static std::pair<Node*, Node*> SplitUp2(Node* p) {
  Node *a = nullptr, *b = nullptr;
  b = p->right_;
  if (b) b->parent_ = nullptr;
  p->right_ = nullptr;

  bool is_p_left_child_of_parent = false;
  bool is_from_left_child = false;
  while (p) {
    Node* parent = p->parent_;

    if (parent) {
      is_p_left_child_of_parent = (parent->left_ == p);
      if (is_p_left_child_of_parent) {
        parent->left_ = nullptr;
      } else {
        parent->right_ = nullptr;
      }
      p->parent_ = nullptr;
    }

    if (!is_from_left_child) {
      a = Merge(p, a);
    } else {
      b = Merge(b, p);
    }

    is_from_left_child = is_p_left_child_of_parent;
    p->Maintain();
    p = parent;
  }

  return {a, b};
}
```

### SplitUp3(u)

Giả sử dãy mà $u$ thuộc về là $L$, tách $L$ tại $u$ thành dãy $L^1, u$ và $L^2$, phần trước chứa các phần tử trước $u$ trong $L$, phần sau chứa các phần tử còn lại.

Có thể thực hiện chức năng trên bằng cách sửa đổi một chút dựa trên `SplitUp2`.

### MakeRoot(u)

Dễ dàng thu được dựa trên `SplitUp2` và `Merge`.

```cpp
void MakeRoot(int u) {
  Node* vertex_u = vertices_[u];
  auto [L1, L2] = Treap::SplitUp2(vertex_u);
  Treap::Merge(L2, L1);
}
```

### Insert(u, v)

Dễ dàng thu được dựa trên `SplitUp2` và `Merge`.

```cpp
void Insert(int u, int v) {
  Node* vertex_u = vertices_[u];
  Node* vertex_v = vertices_[v];

  Node* edge_uv = AllocateNode(u, v);
  Node* edge_vu = AllocateNode(v, u);
  tree_edges_[u][v] = edge_uv;
  tree_edges_[v][u] = edge_vu;

  auto [L11, L12] = Treap::SplitUp2(vertex_u);
  auto [L21, L22] = Treap::SplitUp2(vertex_v);

  Node* L = L12;
  L = Treap::Merge(L, L11);
  L = Treap::Merge(L, edge_uv);
  L = Treap::Merge(L, L22);
  L = Treap::Merge(L, L21);
  L = Treap::Merge(L, edge_vu);
}
```

### Delete(u, v)

Dễ dàng thu được dựa trên `SplitUp3` và `Merge`.

```cpp
void Delete(int u, int v) {
  Node* edge_uv = tree_edges_[u][v];
  Node* edge_vu = tree_edges_[v][u];
  tree_edges_[u].erase(v);
  tree_edges_[v].erase(u);

  int position_uv = Treap::GetPosition(edge_uv);
  int position_vu = Treap::GetPosition(edge_vu);
  if (position_uv > position_vu) {
    std::swap(edge_uv, edge_vu);
    std::swap(position_uv, position_vu);
  }

  auto [L1, uv, _] = Treap::SplitUp3(edge_uv);
  auto [L2, vu, L3] = Treap::SplitUp3(edge_vu);
  Treap::Merge(L1, L3);

  FreeNode(edge_uv);
  FreeNode(edge_vu);
}
```

## Duy trì tính liên thông

Hai điểm $u$ và $v$ liên thông khi và chỉ khi hai điểm thuộc cùng một cây $T$, tức là $(u, u)$ và $(v, v)$ thuộc $\operatorname{ETR}(T)$, điều này có thể được phán đoán bằng cách kiểm tra xem nút Treap tương ứng với nút $u$ và nút $v$ có nằm trong cùng một Treap (có cùng nút gốc) hay không.

### Bài toán ví dụ [P2147 [SDOI2008] Khảo sát hang động](https://www.luogu.com.cn/problem/P2147)

Bài toán mẫu kiểm tra tính liên thông.

??? note "Mã nguồn tham khảo"
    ```cpp
    --8<-- "docs/ds/code/ett/ett_connectivity.cpp"
    ```

## Duy trì thông tin cây con

Dưới đây lấy số lượng nút cây con làm ví dụ để giải thích.

Đối với mỗi phần tử trong $\operatorname{ETR}(T)$, nếu phần tử đó tương ứng với một nút trong cây, thì gán trọng số của nó là $1$; nếu phần tử đó tương ứng với một cạnh trong cây, thì gán trọng số của nó là $0$. Số lượng nút của cây $T$ lúc này có thể coi là tổng trọng số của các phần tử trong $\operatorname{ETR}(T)$, chỉ cần duy trì tổng trọng số của dãy là có thể thực hiện duy trì số lượng nút cây con. Mà việc duy trì tổng trọng số của dãy là thao tác cổ điển của Treap không xoay.

Tương tự, có thể chuyển các thao tác như giá trị nhỏ nhất cây con thành các thao tác kinh điển của cây cân bằng trên dãy (ví dụ: tìm giá trị nhỏ nhất trên đoạn) rồi duy trì bằng cấu trúc dữ liệu tương ứng.

### Bài toán ví dụ [LOJ #2230.「BJOI2014」Hợp nhất lớn](https://loj.ac/p/2230)

??? note "Mã nguồn tham khảo"
    ```cpp
    --8<-- "docs/ds/code/ett/ett_subtree_size.cpp"
    ```

## Duy trì thông tin đường đi cây

Có một kỹ thuật khá phổ biến là dựa vào tính chất của thứ tự ngoặc để chuyển đổi thông tin đường đi cây thành thông tin đoạn, sau đó có thể dựa vào cấu trúc dữ liệu duy trì dãy để duy trì thông tin đường đi cây. Tuy nhiên, kỹ thuật này yêu cầu thông tin cần duy trì phải có **tính trừ được**.

Các thao tác cây động tương ứng với dãy được giới thiệu trước đó có thể làm thay đổi thứ tự trước sau của dấu ngoặc phải so với dấu ngoặc trái trong thứ tự ngoặc, vì vậy khi duy trì các thông tin như tổng điểm nút trên đường đi cây, cần chú ý thêm, thao tác không được làm thay đổi thứ tự trước sau của dấu ngoặc tương ứng, mà điều này có thể cần suy nghĩ lại về thao tác trên dãy tương ứng với thao tác cây động, thậm chí suy nghĩ lại về việc duy trì thứ tự DFS nào.

Ngoài ra, ETT khó duy trì thao tác sửa đổi trên đường đi cây.

### Bài toán ví dụ [「Thăm dò không gian」](https://hydro.ac/p/bzoj-P3786)

Bài toán này thao tác cây động chỉ có đổi cha, có thể coi là xóa cạnh rồi thêm cạnh, nhưng cách này có thể làm thay đổi thứ tự trước sau của các dấu ngoặc tương ứng.

Có thể chuyển trọng số nút thành trọng số cạnh, duy trì thứ tự ngoặc của cây, thao tác đổi cha được chuyển đổi thành việc dịch chuyển toàn bộ dãy ngoặc tương ứng với cây con đến sau dấu ngoặc trái của nút cha.

??? note "Mã nguồn tham khảo"
    ```cpp
    --8<-- "docs/ds/code/ett/ett_1.cpp"
    ```

## Tài liệu tham khảo

-   Dynamic trees as search trees via euler tours, applied to the network simplex algorithm - Robert E. Tarjan
-   Randomized fully dynamic graph algorithms with polylogarithmic time per operation - Henzinger et al.