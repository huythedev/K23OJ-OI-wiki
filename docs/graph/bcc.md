## Giới thiệu

Trước khi đọc nội dung dưới đây, bạn cần nắm vững phần [Các khái niệm liên quan đến lý thuyết đồ thị](./concept.md).

Đọc thêm: [Điểm cắt và cầu](./cut.md)

## Định nghĩa

Định nghĩa chính xác về điểm cắt và cầu xem tại [Các khái niệm liên quan đến lý thuyết đồ thị](./concept.md).

Trên một đồ thị vô hướng liên thông, với hai đỉnh $u$ và $v$, nếu bất kể xóa cạnh nào (chỉ được xóa một cạnh) cũng không thể làm chúng bị ngắt kết nối, ta nói $u$ và $v$ **cạnh hai phía liên thông** (edge-biconnected).

Trên một đồ thị vô hướng liên thông, với hai đỉnh $u$ và $v$, nếu bất kể xóa đỉnh nào (chỉ được xóa một đỉnh, không xóa chính $u$ hoặc $v$) cũng không thể làm chúng bị ngắt kết nối, ta nói $u$ và $v$ **đỉnh hai phía liên thông** (vertex-biconnected).

Tính chất truyền: cạnh hai phía liên thông có tính chất truyền, tức là nếu $x,y$ cạnh hai phía liên thông, $y,z$ cạnh hai phía liên thông thì $x,z$ cũng cạnh hai phía liên thông.

Đỉnh hai phía liên thông **không** có tính chất truyền. Ví dụ dưới đây: $A,B$ đỉnh hai phía liên thông, $B,C$ đỉnh hai phía liên thông, nhưng $A,C$ **không** đỉnh hai phía liên thông.

![bcc-counterexample.png](./images/bcc-0.svg)

Một tiểu đồ thị **cạnh hai phía liên thông cực đại** gọi là **thành phần cạnh hai phía liên thông**.

Một tiểu đồ thị **đỉnh hai phía liên thông cực đại** gọi là **thành phần đỉnh hai phía liên thông**.

## Cây DFS

Với một đồ thị vô hướng liên thông, ta có thể bắt đầu DFS từ bất kỳ đỉnh nào để thu được một cây DFS (gốc là đỉnh bắt đầu), các cạnh thuộc cây này gọi là **cạnh cây**, các cạnh không thuộc cây gọi là **cạnh ngoài cây**.

Do tính chất của DFS, mọi cạnh ngoài cây đều nối hai đỉnh mà một đỉnh là tổ tiên của đỉnh còn lại trên cây.

Mã nguồn DFS như sau:

???+ note "Cài đặt"
    === "C++"
        ```cpp
        void DFS(int p) {
          visited[p] = true;
          for (int to : edge[p])
            if (!visited[to]) DFS(to);
        }
        ```
    
    === "Python"
        ```python
        def DFS(p):
            visited[p] = True
            for to in edge[p]:
                if visited[to] == False:
                    DFS(to)
        ```

## Thành phần cạnh hai phía liên thông

???+ note "[Bài mẫu: LuoGu P8436【Mẫu】Thành phần cạnh hai phía liên thông](https://www.luogu.com.cn/problem/P8436)"
    Cho một đồ thị vô hướng $n$ đỉnh $m$ cạnh, hãy xuất số lượng thành phần cạnh hai phía liên thông và liệt kê từng thành phần.

### Thuật toán Tarjan 1

Dùng Tarjan để tìm thành phần hai phía liên thông tương tự như tìm thành phần liên thông mạnh, có thể xem trước [Thành phần liên thông mạnh](./scc.md) với thuật toán Tarjan.

Ta sẽ tìm tất cả các cầu trước, sau đó DFS để tìm thành phần cạnh hai phía liên thông.

Cách tìm cầu xem tại [Điểm cắt và cầu](./cut.md).

Độ phức tạp $O(n+m)$.

??? note "Mã nguồn mẫu"
    ```cpp
    --8<-- "docs/graph/code/bcc/bcc_1.cpp"
    ```

### Thuật toán Tarjan 2

Tổng kết một tính chất quan trọng: trên đồ thị vô hướng, cạnh cây và cạnh ngoài cây là hai loại duy nhất.

Liên hệ với cách tìm thành phần liên thông mạnh: trên đồ thị vô hướng, nếu một thành phần không có cầu, thì trên cây DFS, tất cả các đỉnh thuộc cùng một thành phần liên thông mạnh.

Ngược lại, một thành phần liên thông mạnh trên cây DFS chính là một thành phần cạnh hai phía liên thông trên đồ thị gốc.

Vậy quá trình tìm thành phần cạnh hai phía liên thông thực chất là quá trình tìm thành phần liên thông mạnh.

Độ phức tạp $O(n+m)$.

??? note "Mã nguồn mẫu"
    ```cpp
    --8<-- "docs/graph/code/bcc/bcc_2.cpp"
    ```

### Thuật toán hiệu ứng chênh lệch (difference)

Tương tự Tarjan 1, ta tìm tất cả các cầu trước, sau đó dùng hiệu ứng chênh lệch để tìm thành phần cạnh hai phía liên thông.

Đầu tiên, thực hiện DFS trên đồ thị gốc.

![bcc-1.png](./images/bcc-1.svg)

Trong hình, cạnh đen và xanh lá là cạnh cây, cạnh đỏ là cạnh ngoài cây. Mỗi cạnh ngoài cây nối hai đỉnh, tương ứng duy nhất với một đường đi đơn giản trên cây gồm các cạnh cây, ta nói cạnh ngoài cây **phủ** lên tất cả các cạnh cây trên đường đi đó.

Trong hình, cạnh cây xanh lá **ít nhất** được một cạnh ngoài cây phủ, cạnh cây đen không được **bất kỳ** cạnh ngoài cây nào phủ.

Rõ ràng, **cạnh ngoài cây** và **cạnh cây xanh lá** chắc chắn không phải cầu, **cạnh cây đen** chắc chắn là cầu.

Xét một cách vét cạn: với mỗi cạnh ngoài cây, duyệt từng cạnh cây trên đường đi và đánh dấu là xanh lá, độ phức tạp $O(nm)$.

Dùng hiệu ứng chênh lệch để tối ưu: với mỗi cạnh ngoài cây, tại đỉnh sâu hơn trên cây DFS đánh dấu `+1`, tại đỉnh cạn hơn đánh dấu `-1`, sau đó $O(n)$ để tính tổng hiệu ứng trong mỗi cây con.

Với một đỉnh $u$, tổng hiệu ứng trong cây con bằng số lượng cạnh ngoài cây phủ lên cạnh cây giữa $u$ và $fa_u$. Nếu giá trị này bằng $0$, cạnh cây giữa $u$ và $fa_u$ là **cầu**.

Sau đó DFS để tìm thành phần cạnh hai phía liên thông.

Độ phức tạp $O(n+m)$.

??? note "Mã nguồn mẫu"
    ```cpp
    --8<-- "docs/graph/code/bcc/bcc_4.cpp"
    ```

???+ note "[#2788.「CEOI2015 Day1」Ống dẫn](https://loj.ac/p/2788)"
    Cho một đồ thị vô hướng $N$ đỉnh $M$ cạnh, không đảm bảo liên thông. Xem mỗi thành phần liên thông là một đồ thị con, hãy tìm các cầu trong từng đồ thị con. **Chỉ có 16 MB bộ nhớ.**

??? note "Phân tích"
    Đặc điểm lớn nhất của bài này là không đủ bộ nhớ để lưu tất cả các cạnh.

    Xét tối ưu lưu cạnh: nếu một cạnh ngoài cây bị một cạnh ngoài cây khác phủ hoàn toàn, thì cạnh đó không cần thiết.

    Dùng cấu trúc hợp nhất (union-find) để quản lý.

## Thành phần đỉnh hai phía liên thông

???+ note "[Bài mẫu: LuoGu P8435【Mẫu】Thành phần đỉnh hai phía liên thông](https://www.luogu.com.cn/problem/P8435)"
    Cho một đồ thị vô hướng $n$ đỉnh $m$ cạnh, hãy xuất số lượng thành phần đỉnh hai phía liên thông và liệt kê từng thành phần.

### Thuật toán Tarjan

Cần học về điểm cắt trước, xem tại [Điểm cắt và cầu](./cut.md) phần điểm cắt.

Hai tính chất:

1.  Hai thành phần đỉnh hai phía liên thông tối đa chỉ có một đỉnh chung, và đó chắc chắn là điểm cắt.
2.  Với một thành phần đỉnh hai phía liên thông, trên cây DFS, đỉnh có dfn nhỏ nhất chắc chắn là điểm cắt hoặc là gốc cây.

Theo tính chất thứ hai, phân thành các trường hợp:

1.  Nếu đỉnh đó là điểm cắt, nó chắc chắn là gốc của thành phần đỉnh hai phía liên thông, vì nếu chứa cha của nó thì nó vẫn là điểm cắt.
2.  Nếu đỉnh đó là gốc cây:
    1.  Có từ hai cây con trở lên, nó là điểm cắt.
    2.  Chỉ có một cây con, nó là gốc của thành phần đỉnh hai phía liên thông.
    3.  Không có cây con, coi như một thành phần đỉnh hai phía liên thông.

??? note "Mã nguồn mẫu"
    ```cpp
    --8<-- "docs/graph/code/bcc/bcc_3.cpp"
    ```

### Thuật toán hiệu ứng chênh lệch

![bcc-2.png](./images/bcc-2.svg)

Trong hình, cạnh đen là cạnh cây, cạnh đỏ là cạnh ngoài cây, mỗi cạnh ngoài cây nối hai đỉnh, tương ứng duy nhất với một đường đi đơn giản trên cây gồm các cạnh cây.

Xét một đồ thị mới, mỗi đỉnh trong đồ thị mới tương ứng với một cạnh cây trong đồ thị gốc (biểu diễn bằng các điểm xanh dương trong hình). Với mỗi cạnh ngoài cây, nối các điểm xanh dương tương ứng với các cạnh cây trên đường đi thành một thành phần liên thông (biểu diễn bằng các cạnh xanh dương).

Như vậy, một đỉnh **không phải** là điểm cắt khi và chỉ khi các cạnh cây nối với nó trong đồ thị mới đều **thuộc** cùng một thành phần liên thông.

Hai đỉnh **là** đỉnh hai phía liên thông khi và chỉ khi các cạnh cây trên đường đi giữa chúng trong đồ thị mới đều **thuộc** cùng một thành phần liên thông, tức là mỗi thành phần liên thông của các điểm xanh dương là một thành phần đỉnh hai phía liên thông.

Quan hệ liên thông giữa các điểm xanh dương có thể quản lý bằng hiệu ứng chênh lệch như khi tìm thành phần cạnh hai phía liên thông, độ phức tạp $O(n+m)$
