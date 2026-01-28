Trang này tổng hợp một số khái niệm trong lý thuyết đồ thị, không phải tất cả đều phổ biến trong OI. Đối với học sinh luyện thi OI, chỉ cần nắm chắc phần cơ bản trong trang này, nếu gặp khái niệm lạ khi học có thể tra cứu lại.

??? warning "Cảnh báo"
    Định nghĩa trong lý thuyết đồ thị có thể khác nhau giữa các giáo trình, cần chú ý ngữ cảnh khi gặp.

## Đồ thị

**Đồ thị (graph)** là một bộ hai $G=(V(G), E(G))$. $V(G)$ là tập không rỗng gọi là **tập đỉnh (vertex set)**, mỗi phần tử của $V$ gọi là **đỉnh (vertex)** hoặc **nút (node)**, viết tắt là **điểm**; $E(G)$ là tập các cạnh giữa các đỉnh trong $V(G)$, gọi là **tập cạnh (edge set)**.

Thường ký hiệu $G=(V,E)$.

Nếu $V,E$ đều là tập hữu hạn, gọi $G$ là **đồ thị hữu hạn**.

Nếu $V$ hoặc $E$ là tập vô hạn, gọi $G$ là **đồ thị vô hạn**.

Có nhiều loại đồ thị: **đồ thị vô hướng (undirected graph)**, **đồ thị có hướng (directed graph)**, **đồ thị hỗn hợp (mixed graph)**,...

Nếu $G$ là đồ thị vô hướng, mỗi phần tử của $E$ là một cặp không thứ tự $(u, v)$, gọi là **cạnh vô hướng (undirected edge)**, viết tắt là **cạnh (edge)**, với $u, v \in V$. Gọi $e = (u, v)$, thì $u$ và $v$ là **đầu mút (endpoint)** của $e$.

Nếu $G$ là đồ thị có hướng, mỗi phần tử của $E$ là một cặp có thứ tự $(u, v)$, đôi khi viết là $u \to v$, gọi là **cạnh có hướng (directed edge)** hoặc **cung (arc)**, cũng có thể gọi là **cạnh (edge)** nếu không gây nhầm lẫn. Với $e = u \to v$, $u$ là **đầu (tail)**, $v$ là **cuối (head)** của $e$, cả hai cũng gọi là **đầu mút (endpoint)**. Ngoài ra, $u$ là tiền nhiệm trực tiếp của $v$, $v$ là hậu duệ trực tiếp của $u$.

???+ note "Tại sao đầu là tail, cuối là head?"
    Cạnh thường được vẽ bằng mũi tên, mũi tên đi từ "đuôi" đến "đầu".

Nếu $G$ là đồ thị hỗn hợp, $E$ gồm cả **cạnh có hướng** và **cạnh vô hướng**.

Nếu mỗi cạnh $e_k=(u_k,v_k)$ của $G$ được gán một số gọi là **trọng số (weight)**, thì $G$ là **đồ thị có trọng số**. Nếu tất cả trọng số là số thực dương, gọi là **đồ thị trọng số dương**.

Số đỉnh của $G$, $\left| V(G) \right|$, gọi là **bậc (order)** của đồ thị $G$.

Hình dung, đồ thị là tập hợp các điểm và các cạnh nối giữa các điểm.

## Liên thuộc và kề

Trong đồ thị vô hướng $G = (V, E)$, nếu đỉnh $v$ là đầu mút của cạnh $e$, thì $v$ và $e$ gọi là **liên thuộc (incident)** hoặc **kề (adjacent)**. Hai đỉnh $u$ và $v$ có cạnh $(u, v)$ thì gọi là **kề nhau (adjacent)**.

**Láng giềng (neighborhood)** của đỉnh $v \in V$ là tập các đỉnh kề với $v$, ký hiệu $N(v)$.

Láng giềng của tập $S$ là tập các đỉnh kề với ít nhất một đỉnh trong $S$, ký hiệu $N(S)$:

$$
N(S) = \bigcup_{v \in S} N(v)
$$

## Đồ thị đơn giản

**Khuyên (loop)**: Nếu cạnh $e = (u, v)$ với $u = v$, thì $e$ là một khuyên.

**Cạnh đa (multiple edge)**: Nếu $E$ có hai cạnh giống hệt nhau $e_1, e_2$, chúng là **cạnh đa**.

**Đồ thị đơn giản (simple graph)**: Không có khuyên và cạnh đa. Trong đồ thị đơn giản vô hướng có ít nhất hai đỉnh, luôn tồn tại hai đỉnh cùng bậc. ([Nguyên lý Dirichlet](../math/combinatorics/drawer-principle.md))

Nếu có khuyên hoặc cạnh đa, gọi là **đa đồ thị (multigraph)**.

??? warning "Cảnh báo"
    Trong đồ thị vô hướng, $(u, v)$ và $(v, u)$ là một cặp cạnh đa; trong đồ thị có hướng, $u \to v$ và $v \to u$ không phải cạnh đa.

??? warning "Cảnh báo"
    Nếu đề không nói rõ, có thể tồn tại khuyên và cạnh đa, cần chú ý khi làm bài.

## Bậc

Số cạnh liên thuộc với đỉnh $v$ gọi là **bậc (degree)** của $v$, ký hiệu $d(v)$. Với cạnh $(v, v)$, mỗi cạnh như vậy cộng $2$ vào $d(v)$.

Với đồ thị đơn giản vô hướng, $d(v) = \left| N(v) \right|$.

**Định lý bắt tay**: Với mọi đồ thị vô hướng $G = (V, E)$, $\sum_{v \in V} d(v) = 2 \left| E \right|$.

Hệ quả: Số đỉnh bậc lẻ trong mọi đồ thị là chẵn.

Nếu $d(v) = 0$, $v$ là **đỉnh cô lập (isolated vertex)**.

Nếu $d(v) = 1$, $v$ là **lá (leaf vertex)**/**đỉnh treo (pendant vertex)**.

Nếu $2 \mid d(v)$, $v$ là **đỉnh chẵn (even vertex)**.

Nếu $2 \nmid d(v)$, $v$ là **đỉnh lẻ (odd vertex)**. Số đỉnh lẻ luôn chẵn.

Nếu $d(v) = \left| V \right| - 1$, $v$ là **đỉnh chi phối (universal vertex)**.

Bậc nhỏ nhất của đồ thị $G$ là **bậc nhỏ nhất (minimum degree)**, ký hiệu $\delta (G)$; bậc lớn nhất là **bậc lớn nhất (maximum degree)**, ký hiệu $\Delta (G)$. Tức là: $\delta (G) = \min_{v \in G} d(v)$, $\Delta (G) = \max_{v \in G} d(v)$.

Trong đồ thị có hướng $G = (V, E)$, số cạnh đi ra từ $v$ là **bán bậc ra (out-degree)**, ký hiệu $d^+(v)$. Số cạnh đi vào $v$ là **bán bậc vào (in-degree)**, ký hiệu $d^-(v)$. Rõ ràng $d^+(v)+d^-(v)=d(v)$.

Với mọi đồ thị có hướng $G = (V, E)$:

$$
\sum_{v \in V} d^+(v) = \sum_{v \in V} d^-(v) = \left| E \right|
$$

Nếu mọi đỉnh đều có bậc $k$, gọi $G$ là **đồ thị $k$-chính quy ($k$-regular graph)**.

Nếu cho trước một dãy số, tồn tại đồ thị $G$ có dãy bậc như vậy, gọi là **dãy khả đồ**.

Nếu tồn tại đồ thị đơn giản $G$ có dãy bậc như vậy, gọi là **dãy khả đồ đơn giản**.

## Đường đi

**Chuỗi (walk)**: Chuỗi là dãy các cạnh nối liên tiếp các đỉnh, có thể hữu hạn hoặc vô hạn. Cụ thể, chuỗi hữu hạn $w$ là dãy cạnh $e_1, e_2, \ldots, e_k$ sao cho tồn tại dãy đỉnh $v_0, v_1, \ldots, v_k$ với $e_i = (v_{i-1}, v_i)$, $i \in [1, k]$. Viết tắt là $v_0 \to v_1 \to v_2 \to \cdots \to v_k$. Số cạnh $k$ gọi là **độ dài** (nếu cạnh có trọng số, độ dài có thể là tổng trọng số).

**Vết (trail)**: Chuỗi $w$ mà các cạnh $e_1, e_2, \ldots, e_k$ đôi một khác nhau.

**Đường đi (path)** (hay **đường đi đơn giản (simple path)**): Vết $w$ mà dãy đỉnh không lặp lại.

**Chu trình (circuit)**: Vết $w$ với $v_0 = v_k$.

**Vòng/chu trình đơn giản (cycle)**: Chu trình $w$ mà $v_0 = v_k$ là cặp đỉnh duy nhất trùng nhau.

??? warning "Cảnh báo"
    Định nghĩa về đường đi có thể khác nhau ở các tài liệu, ví dụ "đường đi" có thể chỉ chuỗi, "vòng" có thể chỉ chu trình. Nếu đề không nói rõ "đường đi đơn giản"/"không đơn giản", nên hỏi lại ý nghĩa.

## Đồ thị con

Với đồ thị $G = (V, E)$, nếu tồn tại $H = (V', E')$ với $V' \subseteq V$, $E' \subseteq E$, thì $H$ là **đồ thị con (subgraph)** của $G$, ký hiệu $H \subseteq G$.

Nếu $H \subseteq G$ và với mọi $u, v \in V'$, nếu $(u, v) \in E$ thì $(u, v) \in E'$, thì $H$ là **đồ thị con cảm ứng (induced subgraph)** của $G$.

Đồ thị con cảm ứng chỉ phụ thuộc vào tập đỉnh, nên đồ thị con cảm ứng trên $V'$ ký hiệu $G \left[ V' \right]$.

Nếu $H \subseteq G$ và $V' = V$, thì $H$ là **đồ thị con sinh (spanning subgraph)** của $G$.

Rõ ràng, $G$ là đồ thị con, đồ thị con sinh, đồ thị con cảm ứng của chính nó; [đồ thị không cạnh](#特殊的图) là đồ thị con sinh của $G$. $G$ và đồ thị không cạnh đều là **đồ thị con tầm thường** của $G$.

Nếu một đồ thị vô hướng có đồ thị con sinh là $k$-chính quy, gọi đó là **$k$-nhân tử ($k$-factor)**.

Nếu đồ thị có hướng $G = (V, E)$ có đồ thị con cảm ứng $H = G \left[ V^\ast \right]$ sao cho $\forall v \in V^\ast, (v, u) \in E$ thì $u \in V^\ast$, thì $H$ là **đồ thị con đóng (closed subgraph)** của $G$.

## Liên thông

### Đồ thị vô hướng

Với đồ thị vô hướng $G = (V, E)$, hai đỉnh $u, v \in V$ liên thông nếu tồn tại chuỗi từ $u$ đến $v$. Mọi đỉnh liên thông với chính nó, hai đầu mút của một cạnh liên thông.

Nếu mọi cặp đỉnh đều liên thông, $G$ là **đồ thị liên thông (connected graph)**, tính chất này gọi là **tính liên thông (connectivity)**.

Nếu $H$ là đồ thị con liên thông của $G$, không tồn tại $F$ với $H\subsetneq F \subseteq G$ và $F$ liên thông, thì $H$ là **thành phần liên thông (connected component)** (đồ thị con liên thông cực đại).

### Đồ thị có hướng

Với đồ thị có hướng $G = (V, E)$, $u, v \in V$, nếu tồn tại chuỗi từ $u$ đến $v$, gọi $u$ **có thể tới được** $v$. Mọi đỉnh tới được chính nó, đầu của một cạnh tới được cuối.

Nếu mọi cặp đỉnh đều tới được nhau, $G$ là **mạnh liên thông (strongly connected)**.

Nếu thay mọi cạnh thành vô hướng và đồ thị thu được liên thông, $G$ là **yếu liên thông (weakly connected)**.

Tương tự, có **thành phần yếu liên thông (weakly connected component)** và **thành phần mạnh liên thông (strongly connected component)**.

Xem thêm thuật toán tại [Thành phần mạnh liên thông](./scc.md).

### Tập cắt

Xem thêm tại [Điểm cắt và cầu](./cut.md) và [Thành phần hai liên thông](./bcc.md).

Ở phần này, "liên thông" với đồ thị có hướng thường chỉ "mạnh liên thông".

Với đồ thị liên thông $G = (V, E)$, $V'\subseteq V$ và $G\left[V\setminus V'\right]$ không liên thông, thì $V'$ là **tập cắt đỉnh (vertex cut/separating set)**. Tập cắt đỉnh kích thước 1 gọi là **đỉnh cắt (cut vertex)**.

Với đồ thị liên thông $G = (V, E)$ và số nguyên $k$, nếu $|V|\ge k+1$ và $G$ không có tập cắt đỉnh kích thước $k-1$, thì $G$ là **$k$-đỉnh liên thông ($k$-vertex-connected)**, số $k$ lớn nhất như vậy gọi là **độ liên thông đỉnh (vertex connectivity)**, ký hiệu $\kappa(G)$. (Với đồ thị không đầy đủ, độ liên thông đỉnh là kích thước tập cắt nhỏ nhất, với đồ thị đầy đủ $K_n$ là $n-1$.)

Với $G = (V, E)$, $u, v\in V$, $u\ne v$, $u$ và $v$ không kề, $u$ tới được $v$, $V'\subseteq V$, $u, v\notin V'$, $G\left[V\setminus V'\right]$ không liên thông giữa $u$ và $v$, thì $V'$ là **tập cắt đỉnh giữa $u$ và $v$**. Kích thước nhỏ nhất của tập này gọi là **độ liên thông đỉnh cục bộ (local connectivity)** giữa $u$ và $v$, ký hiệu $\kappa(u, v)$.

Tương tự với cạnh:

Với $G = (V, E)$ liên thông, $E'\subseteq E$, $G' = (V, E\setminus E')$ không liên thông, $E'$ là **tập cắt cạnh (edge cut)**. Tập cắt cạnh kích thước 1 gọi là **cầu (bridge)**.

Với $G = (V, E)$ liên thông và số nguyên $k$, nếu $G$ không có tập cắt cạnh kích thước $k-1$, thì $G$ là **$k$-cạnh liên thông ($k$-edge-connected)**, số $k$ lớn nhất như vậy gọi là **độ liên thông cạnh (edge connectivity)**, ký hiệu $\lambda(G)$. (Với mọi đồ thị, độ liên thông cạnh là kích thước tập cắt cạnh nhỏ nhất.)

Với $G = (V, E)$, $u, v\in V$, $u\ne v$, $u$ tới được $v$, $E'\subseteq E$, $G'=(V, E\setminus E')$ không liên thông giữa $u$ và $v$, $E'$ là **tập cắt cạnh giữa $u$ và $v$**. Kích thước nhỏ nhất của tập này gọi là **độ liên thông cạnh cục bộ (local edge-connectivity)** giữa $u$ và $v$, ký hiệu $\lambda(u, v)$.

**Hai liên thông đỉnh (biconnected)** gần như tương đương với $2$-đỉnh liên thông, trừ trường hợp đồ thị chỉ có hai đỉnh nối bởi một cạnh, khi đó là hai liên thông đỉnh nhưng không phải $2$-đỉnh liên thông. Nói cách khác, đồ thị liên thông không có đỉnh cắt là hai liên thông đỉnh.

**Hai liên thông cạnh ($2$-edge-connected)** hoàn toàn tương đương với $2$-cạnh liên thông. Đồ thị liên thông không có cầu là hai liên thông cạnh.

Tương tự, có **thành phần hai liên thông đỉnh (biconnected component)** và **thành phần hai liên thông cạnh ($2$-edge-connected component)**.

**Định lý Whitney**: Với mọi đồ thị $G$, $\kappa(G)\le \lambda(G)\le \delta(G)$. (Ba đại lượng lần lượt là độ liên thông đỉnh, cạnh, bậc nhỏ nhất.)

## Đồ thị thưa/đồ thị đặc

Nếu số cạnh của đồ thị nhỏ hơn nhiều so với bình phương số đỉnh, gọi là **đồ thị thưa (sparse graph)**.

Nếu số cạnh gần bằng bình phương số đỉnh, gọi là **đồ thị đặc (dense graph)**.

Hai khái niệm này không có định nghĩa chặt chẽ, thường dùng để so sánh hiệu quả thuật toán [độ phức tạp](../basic/complexity.md) $O(|V|^2)$ và $O(|E|)$ (trên đồ thị đặc thì tương đương, trên đồ thị thưa thì $O(|E|)$ nhanh hơn rõ rệt).

## Đồ thị bù

Với đồ thị đơn vô hướng $G = (V, E)$, **đồ thị bù (complement graph)** ký hiệu $\bar G$, có $V \left( \bar G \right) = V \left( G \right)$, với mọi cặp đỉnh $(u, v)$, $(u, v) \in E \left( \bar G \right)$ khi và chỉ khi $(u, v) \notin E \left( G \right)$.

## Đồ thị chuyển vị

Với đồ thị có hướng $G = (V, E)$, **đồ thị chuyển vị (transpose graph)** là đồ thị giữ nguyên tập đỉnh, đảo ngược hướng mọi cạnh. Nếu $G'$ là chuyển vị của $G$, thì $E'=\{(v, u)|(u, v)\in E\}$.

## Một số đồ thị đặc biệt

Nếu đồ thị đơn vô hướng $G$ có mọi cặp đỉnh khác nhau đều có cạnh nối, gọi là **đồ thị đầy đủ (complete graph)**, ký hiệu $K_n$ với $n$ đỉnh. Nếu đồ thị có hướng $G$ có mọi cặp đỉnh khác nhau đều có hai cạnh ngược chiều, gọi là **đồ thị đầy đủ có hướng (complete digraph)**.

Đồ thị không có cạnh gọi là **đồ thị không cạnh (edgeless graph)**, **đồ thị rỗng (empty graph)** hoặc **đồ thị không (null graph)**, ký hiệu $\overline{K}_n$ hoặc $N_n$. $N_n$ và $K_n$ là đồ thị bù của nhau.

??? warning "Cảnh báo"
    **Đồ thị không (null graph)** cũng có thể chỉ **đồ thị bậc 0 (order-zero graph)** $K_0$, tức là không có đỉnh và cạnh.

Nếu đồ thị đơn có hướng $G$ mà mọi cặp đỉnh khác nhau chỉ có đúng một cạnh (một chiều), gọi là **đồ thị giải đấu (tournament graph)**.

Nếu đồ thị đơn vô hướng $G = (V, E)$ mà các cạnh tạo thành một chu trình, gọi là **đồ thị chu trình (cycle graph)**, ký hiệu $C_n$ với $n\geq 3$. Đồ thị là chu trình khi và chỉ khi là đồ thị $2$-chính quy liên thông.

Nếu $G = (V, E)$ có một đỉnh $v$ là đỉnh chi phối, các đỉnh còn lại không nối với nhau, gọi là **đồ thị sao (star graph)**, ký hiệu $S_n$ với $n+1$ đỉnh ($n\geq 1$).

Nếu $G = (V, E)$ có một đỉnh $v$ là đỉnh chi phối, các đỉnh còn lại tạo thành chu trình, gọi là **đồ thị bánh xe (wheel graph)**, ký hiệu $W_n$ với $n+1$ đỉnh ($n\geq 3$).

Nếu $G = (V, E)$ mà các cạnh tạo thành một đường đi đơn giản, gọi là **đồ thị đường đi (chain/path graph)**, ký hiệu $P_n$ với $n$ đỉnh. Một đường đi là chu trình xóa đi một cạnh.

Nếu đồ thị vô hướng liên thông không chứa chu trình, gọi là **cây (tree)**. Xem thêm [Cơ bản về cây](./tree-basic.md).

Nếu đồ thị vô hướng liên thông có đúng một chu trình, gọi là **cây có chu trình (pseudotree)**.

Nếu đồ thị có hướng yếu liên thông, mọi đỉnh có bậc vào bằng $1$, gọi là **cây ngoài chu trình có hướng**.

Nếu đồ thị có hướng yếu liên thông, mọi đỉnh có bậc ra bằng $1$, gọi là **cây trong chu trình có hướng**.

Nhiều cây hợp lại thành **rừng (forest)**, nhiều cây có chu trình thành **rừng có chu trình (pseudoforest)**, nhiều cây ngoài chu trình có hướng thành **rừng ngoài chu trình có hướng**, nhiều cây trong chu trình có hướng thành **rừng trong chu trình có hướng (functional graph)**.

Nếu đồ thị vô hướng liên thông mà mỗi cạnh thuộc nhiều nhất một chu trình, gọi là **cactus (đồ thị xương rồng)**. Nhiều cactus hợp lại thành **sa mạc**.

Nếu tập đỉnh của đồ thị chia thành hai phần, mỗi phần không có cạnh nội bộ, gọi là **đồ thị hai phía (bipartite graph)**. Nếu mọi cặp đỉnh khác phần đều có cạnh nối, gọi là **đồ thị hai phía đầy đủ (complete bipartite graph/biclique)**, với hai phần $n$ và $m$ đỉnh ký hiệu $K_{n, m}$. Xem thêm [Đồ thị hai phía](./bi-graph.md).

Nếu đồ thị có thể vẽ trên mặt phẳng mà không có hai cạnh nào cắt nhau ngoài đầu mút, gọi là **đồ thị phẳng (planar graph)**. Đồ thị không có đồ thị con là $K_5$ hoặc $K_{3, 3}$ là điều kiện cần và đủ để là đồ thị phẳng. Với đồ thị phẳng đơn liên thông $G=(V, E)$, $V\ge 3$, $|E|\le 3|V|-6$.

## Đẳng cấu

Hai đồ thị $G$ và $H$ là **đẳng cấu (isomorphic)** nếu tồn tại song ánh $f : V(G) \to V(H)$ sao cho $(u,v)\in E(G)$ khi và chỉ khi $(f(u),f(v))\in E(H)$. Ký hiệu $G \cong H$.

Từ định nghĩa, nếu $G \cong H$ thì:

-   $|V(G)|=|V(H)|,|E(G)|=|E(H)|$
-   Dãy bậc giảm dần của $G$ và $H$ giống nhau
-   $G$ và $H$ có các đồ thị con cảm ứng đẳng cấu

## Phép toán hai ngôi trên đồ thị đơn vô hướng

Với đồ thị đơn vô hướng, có các phép toán sau:

**Giao (intersection)**: $G = \left( V_1, E_1 \right), H = \left( V_2, E_2 \right)$, $G \cap H = \left( V_1 \cap V_2, E_1 \cap E_2 \right)$.

Dễ thấy giao của hai đồ thị đơn vô hướng vẫn là đồ thị đơn vô hướng.

**Hợp (union)**: $G = \left( V_1, E_1 \right), H = \left( V_2, E_2 \right)$, $G \cup H = \left( V_1 \cup V_2, E_1 \cup E_2 \right)$.

**Tổng (sum)/tổng trực tiếp (direct sum)**: $G = \left( V_1, E_1 \right), H = \left( V_2, E_2 \right)$, xây dựng $H' \cong H$ sao cho $V \left( H' \right) \cap V_1 = \varnothing$ ($H'$ có thể là $H$). Khi đó, mọi đồ thị đẳng cấu với $G \cup H'$ gọi là tổng/tổng trực tiếp/hợp rời, ký hiệu $G + H$ hoặc $G \oplus H$.

Nếu $G$ và $H$ có tập đỉnh không giao nhau, $G \cup H = G + H$.

Ví dụ, rừng là tổng của nhiều cây.

???+ note "Phân biệt hợp và tổng"
    "Hợp" sẽ gộp các đỉnh/cạnh trùng tên, còn "tổng" thì không.

## Một số tập đỉnh/cạnh đặc biệt

### Tập chi phối

Với đồ thị vô hướng $G=(V, E)$, $V'\subseteq V$ và $\forall v\in(V\setminus V')$ tồn tại cạnh $(u, v)\in E$ với $u\in V'$, thì $V'$ là **tập chi phối (dominating set)**.

Kích thước tập chi phối nhỏ nhất ký hiệu $\gamma(G)$. Bài toán tìm tập chi phối nhỏ nhất là [NP-khó](../misc/cc-basic.md#np-hard).

Với đồ thị có hướng $G=(V, E)$, $V'\subseteq V$ và $\forall v\in(V\setminus V')$ tồn tại cạnh $(u, v)\in E$ với $u\in V'$, thì $V'$ là **tập chi phối ra (out-dominating set)**. Tương tự, có **tập chi phối vào (in-dominating set)**.

Kích thước tập chi phối ra nhỏ nhất ký hiệu $\gamma^+(G)$, chi phối vào là $\gamma^-(G)$.

### Tập chi phối cạnh

Với $G=(V, E)$, $E'\subseteq E$ và $\forall e\in(E\setminus E')$ tồn tại cạnh trong $E'$ kề với $e$, thì $E'$ là **tập chi phối cạnh (edge dominating set)**.

Bài toán tìm tập chi phối cạnh nhỏ nhất là [NP-khó](../misc/cc-basic.md#np-hard).

### Tập độc lập

Với $G=(V, E)$, $V'\subseteq V$ và mọi cặp đỉnh trong $V'$ không kề nhau, $V'$ là **tập độc lập (independent set)**.

Kích thước tập độc lập lớn nhất ký hiệu $\alpha(G)$. Bài toán tìm tập độc lập lớn nhất là [NP-khó](../misc/cc-basic.md#np-hard).

### Ghép (matching)

Với $G=(V, E)$, $E'\subseteq E$ và mọi cặp cạnh khác nhau trong $E'$ không chung đầu mút, không có khuyên, $E'$ là **ghép (matching)** hay **tập cạnh độc lập (independent edge set)**. Đỉnh là đầu mút của cạnh trong matching gọi là **được ghép (matched)/bão hòa (saturated)**, ngược lại là **chưa ghép (unmatched)**.

Matching có nhiều cạnh nhất gọi là **matching cực đại (maximum-cardinality matching)**, kích thước ký hiệu $\nu(G)$.

Nếu cạnh có trọng số, matching tổng trọng số lớn nhất gọi là **matching trọng số lớn nhất (maximum-weight matching)**.

Nếu không thể thêm cạnh nào vào matching mà vẫn là matching, gọi là **matching tối đại (maximal matching)**. Matching cực đại là matching tối đại lớn nhất, mọi matching cực đại đều là tối đại. Matching tối đại là tập chi phối cạnh, nhưng tập chi phối cạnh không nhất thiết là matching. Matching tối đại nhỏ nhất và tập chi phối cạnh nhỏ nhất có cùng kích thước, nhưng tập chi phối cạnh nhỏ nhất không nhất thiết là matching. Tìm matching tối đại nhỏ nhất là NP-khó.

Nếu mọi đỉnh đều được ghép, matching là **matching hoàn hảo (perfect matching)**. Nếu chỉ còn một đỉnh chưa ghép, gọi là **matching gần hoàn hảo (near-perfect matching)**.

Đếm số matching hoặc matching hoàn hảo trong đồ thị thường hoặc hai phía đều là bài toán [#P-đầy đủ](../misc/cc-basic.md#p_1).

Với matching $M$, đường đi bắt đầu từ đỉnh chưa ghép, xen kẽ cạnh trong và ngoài matching, gọi là **đường xen kẽ (alternating path)**; nếu kết thúc ở đỉnh chưa ghép, gọi là **đường tăng (augmenting path)**.

**Định lý Tutte**: Đồ thị vô hướng $n$ đỉnh có matching hoàn hảo khi và chỉ khi với mọi $V' \subset V(G)$, $p_{\text{odd}}(G-V')\leq |V'|$, với $p_{\text{odd}}$ là số thành phần liên thông bậc lẻ.

**Hệ quả Tutte**: Mọi đồ thị $3$-chính quy không cầu đều có matching hoàn hảo.

### Tập phủ đỉnh

Với $G=(V, E)$, $V'\subseteq V$ và $\forall e\in E$ có ít nhất một đầu mút trong $V'$, $V'$ là **tập phủ đỉnh (vertex cover)**.

Tập phủ đỉnh là tập chi phối, nhưng tập phủ đỉnh tối tiểu không nhất thiết là tập chi phối tối tiểu.

Tập phủ đỉnh khi và chỉ khi phần bù là tập độc lập, nên tập phủ đỉnh nhỏ nhất là phần bù của tập độc lập lớn nhất. Bài toán tìm tập phủ đỉnh nhỏ nhất là [NP-khó](../misc/cc-basic.md#np-hard).

Mọi matching có kích thước không vượt quá mọi tập phủ đỉnh. Đồ thị hai phía đầy đủ $K_{n, m}$ có matching cực đại và tập phủ đỉnh nhỏ nhất đều bằng $\min(n, m)$.

### Tập phủ cạnh

Với $G=(V, E)$, $E'\subseteq E$ và $\forall v\in V$ có ít nhất một cạnh trong $E'$ kề với $v$, $E'$ là **tập phủ cạnh (edge cover)**.

Kích thước tập phủ cạnh nhỏ nhất ký hiệu $\rho(G)$, có thể tìm bằng cách mở rộng matching cực đại: với mọi đỉnh chưa ghép, thêm một cạnh kề vào matching, được tập phủ cạnh nhỏ nhất.

Matching cực đại cũng có thể tìm từ tập phủ cạnh nhỏ nhất: với mỗi cặp cạnh chung đỉnh trong tập phủ cạnh, xóa một cạnh.

Kích thước matching cực đại cộng với tập phủ cạnh nhỏ nhất bằng số đỉnh: $\rho(G)+\nu(G)=|V(G)|$.

Kích thước matching cực đại không vượt quá tập phủ cạnh nhỏ nhất: $\nu(G)\le\rho(G)$. Đặc biệt, matching hoàn hảo là tập phủ cạnh nhỏ nhất, và đây là trường hợp duy nhất đạt dấu "=".

Mọi tập độc lập có kích thước không vượt quá mọi tập phủ cạnh. Đồ thị hai phía đầy đủ $K_{n, m}$ có tập độc lập lớn nhất và tập phủ cạnh nhỏ nhất đều bằng $\max(n, m)$.

### Clique (bộ đầy đủ)

Với $G=(V, E)$, $V'\subseteq V$ và mọi cặp đỉnh khác nhau trong $V'$ đều kề nhau, $V'$ là **clique (bộ đầy đủ)**. Đồ thị con cảm ứng của clique là đồ thị đầy đủ.

Nếu không thể thêm đỉnh nào vào clique mà vẫn là clique, gọi là **clique tối đại (maximal clique)**.

Kích thước clique lớn nhất ký hiệu $\omega(G)$, bằng kích thước tập độc lập lớn nhất của đồ thị bù: $\omega(G)=\alpha(\bar{G})$. Bài toán tìm clique lớn nhất là [NP-khó](../misc/cc-basic.md#np-hard).

## Tài liệu tham khảo

[OI Trung chuyển - Tổng hợp khái niệm đồ thị](https://yhx-12243.github.io/OI-transit/memos/14.html)

[Wikipedia](https://en.wikipedia.org/wiki/Glossary_of_graph_theory_terms) (và các mục liên quan)

Sách "Toán rời rạc" (bản sửa đổi), Điền Văn Thành, Chu Lộc Tân, NXB Thiên Tân, tr.184-187

Đới Nhất Kỳ, Hồ Quán Chương, Trần Vệ. Lý thuyết đồ thị và cấu trúc đại số [M]. Bắc Kinh: NXB Thanh Hoa, 1995.
