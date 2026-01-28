author: Ir1d, greyqz, yjl9903, Anguei, Marcythm, ChungZH, Xeonacid, ylxmf2005

BFS là viết tắt của [Breadth First Search](https://en.wikipedia.org/wiki/Breadth-first_search), tiếng Việt là tìm kiếm theo chiều rộng, hay còn gọi là duyệt theo chiều rộng.

Đây là một trong những thuật toán cơ bản và quan trọng nhất trên đồ thị.

"Chiều rộng" ở đây nghĩa là mỗi lần đều cố gắng truy cập các đỉnh cùng tầng. Khi đã duyệt hết một tầng, mới chuyển sang tầng tiếp theo.

Kết quả của BFS là tìm được đường đi hợp lệ **ngắn nhất** từ điểm xuất phát. Nói cách khác, số cạnh trên đường đi này là ít nhất.

Khi BFS kết thúc, mỗi đỉnh đều được truy cập thông qua đường đi ngắn nhất từ điểm xuất phát.

Bạn có thể hình dung quá trình BFS giống như lửa lan trên đồ thị: ban đầu chỉ có điểm xuất phát "bị cháy", mỗi thời điểm, các đỉnh đang cháy sẽ lan lửa sang các đỉnh kề.

## Cài đặt

Các đoạn mã C++ và Python dưới đây sử dụng kiểu lưu đồ thị bằng danh sách cạnh liên kết (chain forward star), chi tiết xem tại [Lưu trữ đồ thị](./save.md).

=== "Giả mã"
    ```text
    bfs(s) {
      q = new queue()
      q.push(s), visited[s] = true
      while (!q.empty()) {
        u = q.pop()
        for each edge(u, v) {
          if (!visited[v]) {
            q.push(v)
            visited[v] = true
          }
        }
      }
    }
    ```

=== "C++"
    ```cpp
    void bfs(int u) {
      while (!Q.empty()) Q.pop();
      Q.push(u);
      vis[u] = 1;
      d[u] = 0;
      p[u] = -1;
      while (!Q.empty()) {
        u = Q.front();
        Q.pop();
        for (int i = head[u]; i; i = e[i].nxt) {
          if (!vis[e[i].to]) {
            Q.push(e[i].to);
            vis[e[i].to] = 1;
            d[e[i].to] = d[u] + 1;
            p[e[i].to] = u;
          }
        }
      }
    }
    
    void restore(int x) {
      vector<int> res;
      for (int v = x; v != -1; v = p[v]) {
        res.push_back(v);
      }
      std::reverse(res.begin(), res.end());
      for (int i = 0; i < res.size(); ++i) printf("%d", res[i]);
      puts("");
    }
    ```

=== "Python"
    ```python
    from queue import Queue
    
    
    def bfs(u):
        Q = Queue()
        Q.put(u)
        vis[u] = True
        d[u] = 0
        p[u] = -1
        while Q.qsize() != 0:
            u = Q.get()
            i = head[u]
            while i:
                if vis[e[i].to] == False:
                    Q.put(e[i].to)
                    vis[e[i].to] = True
                    d[e[i].to] = d[u] + 1
                    p[e[i].to] = u
                i = e[i].nxt
    
    
    def restore(x):
        res = []
        v = x
        while v != -1:
            res.append(v)
            v = p[v]
        res.reverse()
        for i in range(0, len(res)):
            print(res[i])
    ```

Cụ thể, ta dùng một hàng đợi Q để lưu các đỉnh cần xử lý, và một mảng boolean `vis[]` để đánh dấu các đỉnh đã được truy cập.

Ban đầu, tất cả các đỉnh đều có `vis` bằng 0 (chưa truy cập); sau đó, đưa điểm xuất phát s vào Q và gán `vis[s] = 1`.

Tiếp theo, mỗi lần lấy đỉnh u ở đầu Q ra, duyệt các đỉnh kề v, đánh dấu đã truy cập và đưa vào Q.

Lặp đến khi Q rỗng, tức là BFS kết thúc.

Trong quá trình BFS, ta có thể lưu thêm thông tin phụ. Ví dụ, mảng d lưu khoảng cách ngắn nhất từ điểm xuất phát đến mỗi đỉnh (số cạnh ít nhất), mảng p lưu đỉnh cha của mỗi đỉnh trên đường đi.

Nhờ mảng d, ta dễ dàng biết khoảng cách từ điểm xuất phát đến một đỉnh.

Nhờ mảng p, ta dễ dàng truy vết lại đường đi ngắn nhất từ điểm xuất phát đến một đỉnh. Hàm `restore` ở trên dùng mảng này để in ra đường đi.

Độ phức tạp thời gian $O(n + m)$

Độ phức tạp không gian $O(n)$ (mảng `vis` và hàng đợi)

## Bảng open-closed

Khi cài đặt BFS, về bản chất ta đưa các đỉnh chưa truy cập vào một container gọi là open, còn các đỉnh đã truy cập vào container closed.

## BFS trên cây/đồ thị

### Dãy BFS

Tương tự như dãy DFS, dãy BFS là thứ tự các đỉnh được truy cập trong quá trình BFS.

### BFS trên đồ thị tổng quát

Nếu đồ thị gốc không liên thông, chỉ truy cập được các đỉnh có thể đi từ điểm xuất phát.

Dãy BFS thường không duy nhất.

Ta cũng có thể định nghĩa cây BFS: trong quá trình BFS, ghi lại mỗi đỉnh được truy cập từ đâu, ta xây dựng được một cây, gọi là cây BFS.

## Ứng dụng

-   Tìm đường đi ngắn nhất từ điểm xuất phát đến các đỉnh khác trên đồ thị không trọng số.
-   Tìm tất cả các thành phần liên thông trong thời gian $O(n+m)$. (Chỉ cần bắt đầu BFS từ mỗi đỉnh chưa truy cập, mỗi lần BFS sẽ đi hết một thành phần liên thông)
-   Nếu coi mỗi thao tác trong game là một cạnh (một chuyển trạng thái), BFS giúp tìm số bước ít nhất để chuyển từ trạng thái này sang trạng thái khác.
-   Tìm chu trình nhỏ nhất trên đồ thị có hướng không trọng số. (Bắt đầu BFS từ mỗi đỉnh, khi sắp truy cập một đỉnh đã truy cập trước đó, tức là gặp chu trình. Chu trình nhỏ nhất là giá trị nhỏ nhất qua các lần BFS.)
-   Tìm các cạnh chắc chắn nằm trên đường đi ngắn nhất từ $(a, b)$. (BFS từ a và b, lấy hai mảng d. Với mỗi cạnh $(u, v)$, nếu $d_a[u]+1+d_b[v]=d_a[b]$, cạnh đó nằm trên đường đi ngắn nhất)
-   Tìm các đỉnh chắc chắn nằm trên đường đi ngắn nhất từ $(a, b)$. (BFS từ a và b, lấy hai mảng d. Với mỗi đỉnh v, nếu $d_a[v]+d_b[v]=d_a[b]$, đỉnh đó nằm trên một đường đi ngắn nhất)
-   Tìm đường đi ngắn nhất có độ dài chẵn. (Xây dựng đồ thị mới, mỗi đỉnh tách thành hai, cạnh $(u, v)$ thành $((u, 0), (v, 1))$ và $((u, 1), (v, 0))$. BFS trên đồ thị mới, đường đi từ $(s, 0)$ đến $(t, 0)$ là đường đi chẵn.)
-   Tìm đường đi ngắn nhất trên đồ thị có trọng số cạnh là 0/1, xem phần BFS hai đầu dưới đây.

## BFS hai đầu (0-1 BFS)

Nếu bạn chưa biết về deque (hàng đợi hai đầu), hãy xem [deque](../lang/csl/sequence-container.md#deque).

BFS hai đầu còn gọi là 0-1 BFS.

### Phạm vi áp dụng

Các bài toán đường đi ngắn nhất trên đồ thị có trọng số cạnh là 0 hoặc 1, hoặc có thể chuyển về dạng này.

Ví dụ, trong bài toán đi mê cung, bạn có thể dùng 1 đồng để đi 5 bước, hoặc không dùng đồng để đi 1 bước, có thể dùng 0-1 BFS.

### Cài đặt

Thông thường, các cạnh không trọng số thì đưa đỉnh mở rộng vào đầu hàng đợi, cạnh có trọng số thì đưa vào cuối. Như vậy, đảm bảo các đỉnh ở đầu hàng đợi luôn có tổng trọng số không giảm.

Giả mã:

```cpp
while (hàng đợi không rỗng) {
  int u = đầu hàng đợi;
  loại bỏ đầu hàng đợi;
  for (duyệt các đỉnh kề u) {
    cập nhật dữ liệu
    if (...)
      thêm vào đầu hàng đợi;
    else
      thêm vào cuối hàng đợi;
  }
}
```

### Bài mẫu

### [Codeforces 173B](http://codeforces.com/problemset/problem/173/B)

Cho một bảng $n \times m$, có một tia laser bắn từ góc trên trái sang phải. Mỗi khi gặp '#', bạn có thể chọn bắn tia theo 4 hướng hoặc không làm gì, hỏi cần tối thiểu bao nhiêu lần bắn theo 4 hướng để tia đi ra khỏi hàng $n$ bên phải.

Bài này lời giải chuẩn không phải 0-1 BFS, nhưng dùng 0-1 BFS vẫn hợp lý, giảm độ khó tư duy, nhiều cao thủ đã làm như vậy khi thi.

Cách làm: bắn theo hướng cũ thì không tốn chi phí (0), bắn theo 4 hướng thì tốn chi phí (1), cứ thế áp dụng.

#### Mã nguồn

```cpp
--8<-- "docs/graph/code/bfs/bfs_1.cpp"
```

## BFS dùng hàng đợi ưu tiên

Hàng đợi ưu tiên là một kiểu heap, STL có [`std::priority_queue`](../lang/csl/container-adapter.md) để dùng.

Trong BFS dùng hàng đợi ưu tiên, mỗi lần lấy đỉnh có chi phí nhỏ nhất ở đầu hàng đợi để mở rộng. Có thể chứng minh tư duy tham lam này là đúng, vì các đỉnh chi phí lớn hơn sẽ không được cập nhật lại.

Mỗi đỉnh có thể được đưa vào hàng đợi nhiều lần với chi phí khác nhau. Khi một đỉnh được lấy ra lần đầu, sau đó không cần mở rộng từ nó nữa. Do đó, mỗi đỉnh chỉ được xử lý một lần.

So với BFS thường, độ phức tạp tăng thêm $\log n$ do phải duy trì heap. Tuy nhiên, BFS thường có thể đưa một đỉnh vào hàng đợi nhiều lần, độ phức tạp có thể lên tới $O(n^2)$, không phải $O(n)$. Vì vậy, BFS dùng hàng đợi ưu tiên thường nhanh hơn.

Nghe quen không? Thực ra, Dijkstra dùng heap chính là BFS dùng hàng đợi ưu tiên.

## Bài tập

-   [「NOIP2017」Phô mai](https://uoj.ac/problem/332)

BFS hai đầu:

-   [CF1063B. Labyrinth](https://codeforces.com/problemset/problem/1063/B)
-   [CF173B. Chamber of Secrets](https://codeforces.com/problemset/problem/173/B)
-   [「BalticOI 2011 Day1」Bật đèn Switch the Lamp On](https://loj.ac/p/2632)

## Tham khảo

<https://cp-algorithms.com/graph/breadth-first-search.html>
