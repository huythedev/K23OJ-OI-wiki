author: Ir1d, greyqz, yjl9903, partychicken, ChungZH, qq1010903229, Marcythm, Acfboy, shenshuaijie, Craneplayz

## Giới thiệu

DFS là viết tắt của [Depth First Search](https://en.wikipedia.org/wiki/Depth-first_search), tiếng Việt là "Tìm kiếm theo chiều sâu" (hay "Duyệt theo chiều sâu"), là một thuật toán dùng để duyệt hoặc tìm kiếm trên cây hoặc đồ thị. "Theo chiều sâu" nghĩa là mỗi lần đều cố gắng đi sâu hơn vào các đỉnh kế tiếp.

Thuật toán này thường được trình bày cùng với BFS, nhưng ngoài việc cả hai đều có thể duyệt các thành phần liên thông của đồ thị, mục đích sử dụng của chúng hoàn toàn khác nhau, rất hiếm khi có thể thay thế lẫn nhau.

DFS thường dùng để chỉ phương pháp tìm kiếm sử dụng hàm đệ quy, nhưng thực chất hai khái niệm này không hoàn toàn giống nhau. Để tìm hiểu thêm về tư duy tìm kiếm này, hãy tham khảo [DFS (tìm kiếm)](../search/dfs.md).

## Quy trình

Đặc điểm nổi bật nhất của DFS là **gọi đệ quy chính nó**. Tương tự như BFS, DFS sẽ đánh dấu các đỉnh đã được thăm, khi duyệt đồ thị sẽ bỏ qua các đỉnh đã đánh dấu, đảm bảo **mỗi đỉnh chỉ được thăm một lần**. Hàm tuân thủ hai quy tắc trên được gọi là DFS theo nghĩa rộng.

Cấu trúc cơ bản của DFS như sau:

    DFS(v) // v có thể là một đỉnh trong đồ thị, hoặc là một khái niệm trừu tượng như trạng thái dp.
      Đánh dấu đã thăm v
      for u trong các đỉnh kề với v
        if u chưa được đánh dấu thì
          DFS(u)
        end
      end
    end

Đoạn mã trên chỉ thể hiện cấu trúc tối thiểu của DFS. Trong thực tế, DFS thường được bổ sung thêm các thao tác khác để tận dụng đặc tính của DFS.

## Tính chất

Thuật toán này thường có độ phức tạp thời gian là $O(n+m)$, độ phức tạp không gian là $O(n)$, với $n$ là số đỉnh, $m$ là số cạnh. Lưu ý rằng độ phức tạp không gian bao gồm cả không gian ngăn xếp, vốn cũng là $O(n)$. Để đạt được độ phức tạp thời gian này, cần đảm bảo việc duyệt mỗi cạnh trung bình $O(1)$, ví dụ như sử dụng danh sách kề hoặc forward star; nếu dùng ma trận kề thì không đảm bảo được độ phức tạp này.

> Lưu ý: Hiện nay hầu hết các kỳ thi lập trình (bao gồm NOIP, đa số kỳ thi cấp tỉnh và các cuộc thi do CCF tổ chức) đều **không giới hạn không gian ngăn xếp**, tức là: không gian ngăn xếp không bị giới hạn riêng, nhưng tổng bộ nhớ vẫn bị giới hạn theo đề bài. Tuy nhiên, đa số hệ điều hành sẽ giới hạn thêm không gian ngăn xếp, nên khi chạy thử trên máy cá nhân cần thiết lập lại giới hạn này.
>
> -   Trên Windows, thường thêm tùy chọn biên dịch `-Wl,--stack=1000000000` để đặt giới hạn ngăn xếp thành 1000000000 byte.
> -   Trên Linux, thường chạy lệnh `ulimit -s unlimited` trong terminal trước khi chạy chương trình để bỏ giới hạn ngăn xếp. Mỗi terminal chỉ cần thực hiện một lần, có hiệu lực cho mọi lần chạy sau đó.

## Cài đặt

### Cài đặt bằng ngăn xếp

DFS có thể sử dụng [ngăn xếp (Stack)](../ds/stack.md) để lưu tạm các đỉnh trong quá trình duyệt; điều này tương ứng với việc BFS sử dụng [hàng đợi (Queue)](../ds/queue.md).

=== "C++"
    ```cpp
    vector<vector<int>> adj;  // Danh sách kề
    vector<bool> vis;         // Đánh dấu các đỉnh đã được duyệt
    
    void dfs(int s) {
      stack<int> st;
      st.push(s);
      vis[s] = true;
    
      while (!st.empty()) {
        int u = st.top();
        st.pop();
    
        for (int v : adj[u]) {
          if (!vis[v]) {
            vis[v] = true;  // Đảm bảo không có phần tử trùng trong ngăn xếp
            st.push(v);
          }
        }
      }
    }
    ```

=== "Python"
    ```python
    # adj : List[List[int]] danh sách kề
    # vis : List[bool] đánh dấu các đỉnh đã được duyệt
    
    
    def dfs(s: int) -> None:
        stack = [s]  # Dùng list để mô phỏng ngăn xếp, thêm điểm bắt đầu vào ngăn xếp
        vis[s] = True  # Đánh dấu điểm bắt đầu đã được duyệt
    
        while stack:  # Khi ngăn xếp còn phần tử thì tiếp tục
            u = (
                stack.pop()
            )  # Lấy và loại bỏ phần tử cuối cùng (đỉnh ngăn xếp), tức là đi tới u
    
            for v in adj[u]:  # Với mỗi đỉnh kề v của u
                if not vis[v]:  # Nếu v chưa được duyệt
                    vis[v] = True  # Đảm bảo không có phần tử trùng trong ngăn xếp
                    stack.append(v)  # Thêm v vào ngăn xếp
    ```

### Cài đặt bằng đệ quy

Khi gọi hàm đệ quy, thứ tự thêm và loại bỏ phần tử trên ngăn xếp giống như thao tác trên ngăn xếp thực tế, do đó vùng nhớ dùng cho gọi hàm đệ quy được gọi là "ngăn xếp gọi hàm" (Call Stack). DFS có thể cài đặt bằng phương pháp đệ quy.

Ví dụ với [danh sách kề (Adjacency List)](./save.md#邻接表):

=== "C++"
    ```cpp
    vector<vector<int>> adj;  // Danh sách kề
    vector<bool> vis;         // Đánh dấu các đỉnh đã được duyệt
    
    void dfs(const int u) {
      vis[u] = true;
      for (int v : adj[u])
        if (!vis[v]) dfs(v)
    }
    ```

=== "Python"
    ```python
    # adj : List[List[int]] danh sách kề
    # vis : List[bool] đánh dấu các đỉnh đã được duyệt
    
    
    def dfs(u: int) -> None:
        vis[u] = True
        for v in adj[u]:
            if not vis[v]:
                dfs(v)
    ```

Ví dụ với [forward star dạng liên kết](./save.md#链式前向星):

=== "C++"
    ```cpp
    void dfs(int u) {
      vis[u] = 1;
      for (int i = head[u]; i; i = e[i].x) {
        if (!vis[e[i].t]) {
          dfs(v);
        }
      }
    }
    ```

=== "Java"
    ```Java
    public void dfs(int u) {
        vis[u] = true;
        for (int i = head[u]; i != 0; i = e[i].x) {
            if (!vis[e[i].t]) {
                dfs(v);
            }
        }
    }
    ```

=== "Python"
    ```python
    def dfs(u):
        vis[u] = True
        i = head[u]
        while i:
            if vis[e[i].t] == False:
                dfs(v)
            i = e[i].x
    ```

### Dãy DFS

Dãy DFS là thứ tự các đỉnh được truy cập trong quá trình gọi DFS.

Ta nhận thấy, mỗi cây con đều tương ứng với một đoạn liên tiếp (một đoạn chỉ số) trong dãy DFS.

### Dãy dấu ngoặc

Khi DFS đi vào một đỉnh thì ghi lại một dấu ngoặc trái `(`, khi thoát khỏi một đỉnh thì ghi lại một dấu ngoặc phải `)`.

Mỗi đỉnh sẽ xuất hiện hai lần. Hai đỉnh liên tiếp trong dãy có độ sâu chênh lệch 1.

### DFS trên đồ thị tổng quát

Với đồ thị không liên thông, chỉ có thể truy cập thành phần liên thông chứa điểm xuất phát.

Với đồ thị liên thông, dãy DFS thường không duy nhất.

Lưu ý: Dãy DFS của cây cũng không duy nhất.

Trong quá trình DFS, nếu ghi lại mỗi đỉnh được truy cập từ đâu, ta có thể xây dựng một cấu trúc cây, gọi là cây DFS. Cây DFS là một cây khung của đồ thị gốc.

[Cây DFS](./scc.md#dfs-生成树) có nhiều tính chất, ví dụ có thể dùng để tìm [thành phần liên thông mạnh](./scc.md).
