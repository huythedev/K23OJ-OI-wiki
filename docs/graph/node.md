author: Anguei, sshwy, Xeonacid, Ir1d, MonkeyOliver, hsfzLZH1

Tách đỉnh (拆点) là một kỹ thuật mô hình hóa trong đồ thị, thường dùng trong [Luồng mạng (Network Flow)](./flow.md), để xử lý các bài toán **giới hạn trên đỉnh hoặc giới hạn lưu lượng qua đỉnh**, và cũng thường dùng trong **đồ thị phân lớp (layered graph)**.

## Bài toán cực đại hóa luồng với giới hạn trên đỉnh

Nếu chuyển các ràng buộc trên đỉnh thành ràng buộc trên cạnh, thì bài toán này có thể áp dụng các mẫu thuật toán chuẩn.

Ta xét chuyển mỗi đỉnh có giới hạn lưu lượng thành một cấu trúc gồm hai đỉnh $u, v$ và một cạnh $\left\langle u,v \right\rangle$. Trong đó, đỉnh $u$ nhận tất cả các cạnh đi vào từ các đỉnh khác trong đồ thị gốc, còn đỉnh $v$ phát ra tất cả các cạnh đi ra từ đỉnh đó trong đồ thị gốc. Cạnh $\left\langle u,v \right\rangle$ có giới hạn lưu lượng đúng bằng giới hạn của đỉnh gốc. Sau khi chuyển đổi, chỉ cần áp dụng các thuật toán chuẩn cho bài toán luồng. Đây chính là ý tưởng cơ bản của kỹ thuật tách đỉnh.

Nếu đồ thị gốc như sau:

![](./images/node.svg)

Sau khi tách đỉnh, đồ thị sẽ như thế này:

![](./images/node-split.svg)

## Đường đi ngắn nhất trên đồ thị phân lớp

Bài toán đường đi ngắn nhất trên đồ thị phân lớp, ví dụ: Có $k$ lần được đi qua một cạnh với chi phí $0$, hỏi tổng chi phí nhỏ nhất từ $s$ đến $t$. Với dạng bài này, ta có thể sử dụng ý tưởng quy hoạch động (DP), đặt $\text{dis}_{i, j}$ là độ dài đường đi ngắn nhất từ đỉnh $i$ khi đã sử dụng $j$ lần quyền đi miễn phí. Rõ ràng, mảng $\text{dis}$ có thể chuyển trạng thái như sau:

$\text{dis}_{i, j} = \min\{\min\{\text{dis}_{from, j - 1}\}, \min\{\text{dis}_{from,j} + w\}\}$

Trong đó, $from$ là các đỉnh kề với $i$, $w$ là trọng số cạnh hiện tại. Khi $j - 1 \geq k$, $\text{dis}_{from, j} = \infty$.

Thực chất, DP này tương đương với việc tách mỗi đỉnh thành $k+1$ đỉnh, mỗi đỉnh mới đại diện cho trạng thái đã sử dụng một số lần quyền miễn phí khác nhau khi đến đỉnh gốc. Nói cách khác, mỗi đỉnh $u_i$ biểu diễn trạng thái đã dùng $i$ lần quyền miễn phí khi đến $u$.

??? note "[「JLOI2011」Tuyến bay](https://www.luogu.com.cn/problem/P4568)"
    Đề bài: Cho một đồ thị vô hướng $n$ đỉnh $m$ cạnh, bạn có thể chọn tối đa $k$ cạnh để đi qua với chi phí $0$, hỏi chi phí nhỏ nhất từ $s$ đến $t$.

    Tham khảo code cốt lõi:

    ```cpp
    struct State {    // Cấu trúc trạng thái cho hàng đợi ưu tiên
      int v, w, cnt;  // cnt là số lần đã dùng quyền miễn phí

      State() {}

      State(int v, int w, int cnt) : v(v), w(w), cnt(cnt) {}

      bool operator<(const State &rhs) const { return w > rhs.w; }
    };

    void dijkstra() {
      memset(dis, 0x3f, sizeof dis);
      dis[s][0] = 0;
      pq.push(State(s, 0, 0));  // Đến đỉnh xuất phát không cần dùng quyền miễn phí, khoảng cách là 0
      while (!pq.empty()) {
        const State top = pq.top();
        pq.pop();
        int u = top.v, nowCnt = top.cnt;
        if (done[u][nowCnt]) continue;
        done[u][nowCnt] = true;
        for (int i = head[u]; i; i = edge[i].next) {
          int v = edge[i].v, w = edge[i].w;
          if (nowCnt < k && dis[v][nowCnt + 1] > dis[u][nowCnt]) {  // Có thể đi miễn phí
            dis[v][nowCnt + 1] = dis[u][nowCnt];
            pq.push(State(v, dis[v][nowCnt + 1], nowCnt + 1));
          }
          if (dis[v][nowCnt] > dis[u][nowCnt] + w) {  // Không đi miễn phí
            dis[v][nowCnt] = dis[u][nowCnt] + w;
            pq.push(State(v, dis[v][nowCnt], nowCnt));
          }
        }
      }
    }

    int main() {
      n = read(), m = read(), k = read();
      // Thói quen của tác giả là đánh số từ 1 đến n, nhưng đề này đánh số từ 0 đến n-1 nên cần xử lý lại
      s = read() + 1, t = read() + 1;
      while (m--) {
        int u = read() + 1, v = read() + 1, w = read();
        add(u, v, w), add(v, u, w);  // Đồ thị vô hướng
      }
      dijkstra();
      int ans = std::numeric_limits<int>::max();  // Khởi tạo ans là giá trị lớn nhất của int
      for (int i = 0; i <= k; ++i)
        ans = std::min(ans, dis[t][i]);  // Lấy kết quả tốt nhất trong mọi trường hợp đến đích
      println(ans);
    }
    ```
