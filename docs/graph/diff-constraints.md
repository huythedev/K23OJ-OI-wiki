author: Ir1d, Anguei, hsfzLZH1

## Định nghĩa

**Hệ ràng buộc hiệu** (hay "hệ chênh lệch ràng buộc") là một hệ bất phương trình tuyến tính đặc biệt gồm $n$ biến $x_1,x_2,\dots,x_n$ và $m$ điều kiện ràng buộc, mỗi điều kiện đều là hiệu của hai biến, có dạng $x_i-x_j\leq c_k$, với $1 \leq i, j \leq n, i \neq j, 1 \leq k \leq m$ và $c_k$ là hằng số (có thể âm hoặc dương). Bài toán đặt ra là: tìm một bộ giá trị $x_1=a_1,x_2=a_2,\dots,x_n=a_n$ sao cho mọi ràng buộc đều được thỏa mãn, hoặc kết luận là vô nghiệm.

Mỗi ràng buộc $x_i-x_j\leq c_k$ có thể biến đổi thành $x_i\leq x_j+c_k$, rất giống với bất đẳng thức tam giác trong bài toán đường đi ngắn nhất đơn nguồn: $dist[y]\leq dist[x]+z$. Do đó, ta có thể coi mỗi biến $x_i$ là một đỉnh trong đồ thị, với mỗi ràng buộc $x_i-x_j\leq c_k$ thì nối một cạnh có hướng từ đỉnh $j$ tới đỉnh $i$ với trọng số $c_k$.

Lưu ý rằng, nếu $\{a_1,a_2,\dots,a_n\}$ là một nghiệm của hệ ràng buộc hiệu, thì với bất kỳ hằng số $d$, bộ $\{a_1+d,a_2+d,\dots,a_n+d\}$ cũng là một nghiệm, vì khi lấy hiệu thì $d$ bị triệt tiêu.

## Quy trình

Đặt $dist[0]=0$ và nối từ đỉnh 0 tới mỗi đỉnh một cạnh trọng số 0, sau đó chạy thuật toán đường đi ngắn nhất đơn nguồn. Nếu đồ thị có chu trình âm, hệ ràng buộc hiệu vô nghiệm; ngược lại, $x_i=dist[i]$ là một nghiệm của hệ.

## Tính chất

Thường dùng Bellman–Ford hoặc Bellman–Ford tối ưu bằng hàng đợi (SPFA, chạy rất nhanh trên một số đồ thị ngẫu nhiên) để kiểm tra chu trình âm, độ phức tạp xấu nhất là $O(nm)$.

## Một số kỹ thuật biến đổi thường gặp

### Ví dụ [luogu P1993 Nông trại của K](https://www.luogu.com.cn/problem/P1993)

Tóm tắt đề: Giải hệ ràng buộc hiệu với $m$ điều kiện, mỗi điều kiện có dạng $x_a-x_b\geq c_k$, $x_a-x_b\leq c_k$ hoặc $x_a=x_b$, kiểm tra hệ có nghiệm hay không.

|      Ý nghĩa đề      |           Biến đổi           |         Cách nối cạnh        |
| :-----------------: | :-------------------------: | :-------------------------: |
| $x_a - x_b \geq c$  |     $x_b - x_a \leq -c$     |     `add(a, b, -c);`        |
| $x_a - x_b \leq c$  |     $x_a - x_b \leq c$      |     `add(b, a, c);`         |
|   $x_a = x_b$       | $x_a - x_b \leq 0, x_b - x_a \leq 0$ | `add(b, a, 0), add(a, b, 0);` |

Chạy kiểm tra chu trình âm, nếu không có thì in `Yes`, ngược lại in `No`.

??? note "Tham khảo mã nguồn"
    ```cpp
    --8<-- "docs/graph/code/diff-constraints/diff-constraints_1.cpp"
    ```

### Ví dụ [P4926\[1007\] Đo lường nhân bội](https://www.luogu.com.cn/problem/P4926)

Không xét nhị phân hay các kỹ thuật khác, chỉ bàn về cách giải hệ $\frac{x_i}{x_j}\leq c_k$ bằng ràng buộc hiệu.

Chỉ cần lấy $\log$ cho mỗi $x_i,x_j$ và $c_k$, phép chia chuyển thành phép cộng: $\log x_i-\log x_j \leq \log c_k$, khi đó có thể dùng hệ ràng buộc hiệu để giải.

## Mã kiểm tra chu trình âm bằng Bellman–Ford

Dưới đây là mã kiểm tra chu trình âm bằng thuật toán Bellman–Ford. Khi dùng, cần đảm bảo đồ thị liên thông.

???+ note "Cài đặt"
    === "C++"
        ```cpp
        bool Bellman_Ford() {
          for (int i = 0; i < n; i++) {
            bool jud = false;
            for (int j = 1; j <= n; j++)
              for (int k = h[j]; ~k; k = nxt[k])
                if (dist[j] > dist[p[k]] + w[k])
                  dist[j] = dist[p[k]] + w[k], jud = true;
            if (!jud) break;
          }
          for (int i = 1; i <= n; i++)
            for (int j = h[i]; ~j; j = nxt[j])
              if (dist[i] > dist[p[j]] + w[j]) return false;
          return true;
        }
        ```
    
    === "Python"
        ```python
        def Bellman_Ford():
            for i in range(0, n):
                jud = False
                for j in range(1, n + 1):
                    while ~k:
                        k = h[j]
                        if dist[j] > dist[p[k]] + w[k]:
                            dist[j] = dist[p[k]] + w[k]
                            jud = True
                        k = nxt[k]
                if jud == False:
                    break
            for i in range(1, n + 1):
                while ~j:
                    j = h[i]
                    if dist[i] > dist[p[j]] + w[j]:
                        return False
                    j = nxt[j]
            return True
        ```

## Bài tập

[Usaco2006 Dec Wormholes - Lỗ sâu](https://loj.ac/problem/10085)

[「SCOI2011」Kẹo](https://loj.ac/problem/2436)

[POJ 1364 King](http://poj.org/problem?id=1364)

[POJ 2983 Thông tin có đáng tin cậy?](http://poj.org/problem?id=2983)
