## Định nghĩa

Đường đi qua tất cả các đỉnh của đồ thị đúng một lần được gọi là **đường đi Hamilton**.

Chu trình qua tất cả các đỉnh của đồ thị đúng một lần được gọi là **chu trình Hamilton**.

Đồ thị có chu trình Hamilton được gọi là **đồ thị Hamilton**.

Đồ thị có đường đi Hamilton nhưng không có chu trình Hamilton được gọi là **đồ thị nửa Hamilton**.

## Tính chất

Cho $G=\langle V, E\rangle$ là đồ thị Hamilton, khi đó với mọi tập con thực sự khác rỗng $V_1$ của $V$, đều có $p(G-V_1) \leq |V_1|$, trong đó $p(x)$ là số thành phần liên thông của $x$.

Hệ quả: Cho $G=\langle V, E\rangle$ là đồ thị nửa Hamilton, khi đó với mọi tập con thực sự khác rỗng $V_1$ của $V$, đều có $p(G-V_1) \leq |V_1|+1$, trong đó $p(x)$ là số thành phần liên thông của $x$.

Trong đồ thị đầy đủ $K_{2k+1} (k \geq 1)$ chứa $k$ chu trình Hamilton cạnh-disjoint, và $k$ chu trình này chứa toàn bộ cạnh của $K_{2k+1}$.

Trong đồ thị đầy đủ $K_{2k} (k \geq 2)$ chứa $k-1$ chu trình Hamilton cạnh-disjoint, sau khi xóa $k-1$ chu trình này khỏi $K_{2k}$ thì đồ thị thu được chứa $k$ cạnh đôi một không kề nhau.

## Điều kiện đủ

Cho $G$ là đồ thị vô hướng đơn có $n(n \geq 2)$ đỉnh, nếu với mọi cặp đỉnh không kề $v_i, v_j$ trong $G$ đều có $d(v_i)+ d(v_j) \geq n - 1$ thì trong $G$ tồn tại đường đi Hamilton.

Hệ quả 1: Cho $G$ là đồ thị vô hướng đơn có $n(n \geq 3)$ đỉnh, nếu với mọi cặp đỉnh không kề $v_i, v_j$ trong $G$ đều có $d(v_i)+ d(v_j) \geq n$ thì trong $G$ tồn tại chu trình Hamilton, do đó $G$ là đồ thị Hamilton.

Hệ quả 2: Cho $G$ là đồ thị vô hướng đơn có $n(n \geq 3)$ đỉnh, nếu với mọi đỉnh $v_i$ trong $G$ đều có $d(v_i) \geq \frac{n}{2}$ thì trong $G$ tồn tại chu trình Hamilton, do đó $G$ là đồ thị Hamilton.

Cho $D$ là đồ thị thi đấu (tournament) bậc $n(n \geq 2)$, khi đó $D$ có đường đi Hamilton.

Nếu $D$ chứa đồ thị thi đấu bậc $n(n \geq 2)$ làm đồ thị con, khi đó $D$ có đường đi Hamilton.

Đồ thị thi đấu liên thông mạnh là đồ thị Hamilton.

Nếu $D$ chứa đồ thị thi đấu liên thông mạnh bậc $n(n \geq 2)$ làm đồ thị con, khi đó $D$ có chu trình Hamilton.
