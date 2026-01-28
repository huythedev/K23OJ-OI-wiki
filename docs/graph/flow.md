Trang này chủ yếu giới thiệu các kiến thức cơ bản liên quan đến mạng lưới dòng chảy (network flow).

## Tổng quan

Mạng (network) là một trường hợp đặc biệt của đồ thị có hướng $G=(V,E)$, điểm khác biệt so với đồ thị có hướng thông thường là có thêm khái niệm dung lượng và các đỉnh nguồn - đỉnh đích.

-   Mỗi cạnh $(u, v)$ trong $E$ đều có một trọng số gọi là dung lượng (capacity), ký hiệu $c(u, v)$. Nếu $(u,v)\notin E$, có thể giả sử $c(u,v)=0$.

-   Trong $V$ có hai đỉnh đặc biệt: đỉnh nguồn (source) $s$ và đỉnh đích (sink) $t$ ($s \neq t$).

Với mạng $G=(V, E)$, dòng chảy (flow) là một hàm từ tập cạnh $E$ tới tập số nguyên hoặc số thực, thỏa mãn các tính chất sau:

1.  Ràng buộc dung lượng: Với mỗi cạnh, lượng dòng chảy qua cạnh đó không vượt quá dung lượng, tức là $0 \leq f(u,v) \leq c(u,v)$;
2.  Bảo toàn dòng chảy: Với mọi đỉnh $u$ (trừ đỉnh nguồn và đỉnh đích), tổng dòng chảy vào bằng tổng dòng chảy ra, tức là $f(u) = \sum_{x \in V} f(u, x) - \sum_{x \in V} f(x, u) = 0$.

Với mạng $G = (V, E)$ và dòng chảy $f$ trên đó, ta định nghĩa giá trị dòng chảy $|f|$ là dòng chảy ròng tại đỉnh nguồn $f(s)$. Theo tính chất bảo toàn dòng chảy, điều này cũng bằng đối số của dòng chảy ròng tại đỉnh đích $-f(t)$.

Với mạng $G = (V, E)$, nếu $\{S, T\}$ là một phân hoạch của $V$ (tức là $S \cup T = V$ và $S \cap T = \varnothing$), đồng thời $s \in S, t \in T$, thì $\{S, T\}$ được gọi là một nhát cắt $s$-$t$ (cut) của $G$. Ta định nghĩa dung lượng của nhát cắt $s$-$t$ là $||S, T|| = \sum_{u \in S} \sum_{v \in T} c(u, v)$.

## Các bài toán thường gặp

Các bài toán mạng lưới dòng chảy phổ biến bao gồm nhưng không giới hạn ở các dạng sau:

-   Bài toán dòng chảy cực đại (maximum flow): Với mạng $G = (V, E)$, gán dòng chảy cho các cạnh sao cho tổng dòng chảy lớn nhất có thể. Khi đó $f$ được gọi là dòng chảy cực đại của $G$.
-   Bài toán nhát cắt nhỏ nhất (minimum cut): Với mạng $G = (V, E)$, tìm một nhát cắt $s$-$t$ sao cho tổng dung lượng của nhát cắt là nhỏ nhất. Khi đó tổng dung lượng này gọi là nhát cắt nhỏ nhất của $G$.
-   Bài toán dòng chảy cực đại chi phí nhỏ nhất (minimum cost maximum flow): Trên mạng $G = (V, E)$, mỗi cạnh có thêm một trọng số $w(u, v)$ gọi là chi phí (cost), nghĩa là mỗi đơn vị dòng chảy qua $(u, v)$ phải trả chi phí $w(u, v)$. Trong tất cả các dòng chảy cực đại, chọn dòng chảy có tổng chi phí nhỏ nhất gọi là dòng chảy cực đại chi phí nhỏ nhất.

Chúng ta sẽ tìm hiểu chi tiết các bài toán này ở các phần sau.

## Bài mẫu: 24 bài toán mạng lưới dòng chảy

"24 bài toán mạng lưới dòng chảy" là một danh sách bài tập nổi tiếng trên Internet tiếng Trung ([LibreOJ](https://loj.ac/problems/tag/30)/[LuoGu](https://www.luogu.com.cn/problem/list?tag=332)), đã xuất hiện ít nhất từ khoảng năm 2010. Danh sách này giới thiệu nhiều kỹ thuật kinh điển để mô hình hóa các bài toán khác thành bài toán mạng lưới dòng chảy. Dù do hạn chế về thời đại nên chưa chắc đã bao quát hết các dạng bài tiêu biểu nhất, nhưng vẫn rất đáng tham khảo cho các bạn học sinh đam mê thuật toán.
