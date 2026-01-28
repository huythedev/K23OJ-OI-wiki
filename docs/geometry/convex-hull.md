## Bao lồi hai chiều

### Định nghĩa

#### Đa giác lồi

Đa giác lồi là đa giác đơn giản mà tất cả các góc trong đều nằm trong khoảng $[0,\pi]$.

#### Bao lồi

Trên mặt phẳng, bao lồi của một tập hợp điểm là đa giác lồi nhỏ nhất chứa tất cả các điểm đó.

Định nghĩa chính xác: Với tập hợp $X$ cho trước, giao của tất cả các tập lồi chứa $X$ được gọi là **bao lồi** của $X$.

Có thể hình dung như dùng một sợi dây chun bao quanh tất cả các điểm đã cho.

Bao lồi là đa giác có chu vi nhỏ nhất bao quanh tất cả các điểm đã cho. Nếu một đa giác lõm bao quanh tất cả các điểm, chu vi của nó chắc chắn không nhỏ nhất, như hình dưới. Theo bất đẳng thức tam giác, đa giác lồi luôn tối ưu về chu vi.

![](./images/ch.png)

### Thuật toán Andrew tìm bao lồi

Có hai phương pháp phổ biến là Graham scan và Andrew, ở đây chủ yếu giới thiệu thuật toán Andrew.

#### Tính chất

Độ phức tạp thời gian của thuật toán này là $O(n\log n)$, với $n$ là số lượng điểm, chủ yếu do bước sắp xếp các điểm theo hai khóa.

#### Quy trình

Đầu tiên, sắp xếp tất cả các điểm theo hoành độ tăng dần, nếu hoành độ bằng nhau thì so sánh tung độ.

Rõ ràng, điểm nhỏ nhất và lớn nhất sau khi sắp xếp chắc chắn nằm trên bao lồi. Vì là đa giác lồi, nếu đi ngược chiều kim đồng hồ từ một điểm, đường đi luôn "rẽ trái", nếu xuất hiện "rẽ phải" thì đoạn đó không thuộc bao lồi. Do đó, ta có thể dùng một ngăn xếp đơn điệu để duyệt vỏ trên và vỏ dưới.

Vì hướng quay của vỏ trên và vỏ dưới khác nhau, để ngăn xếp đơn điệu hoạt động đúng, ta **duyệt tăng** để tìm vỏ dưới, sau đó **duyệt giảm** để tìm vỏ trên.

Khi tìm vỏ, nếu phát hiện điểm sắp vào ngăn xếp ($P$) và hai điểm trên đỉnh ngăn xếp ($S_1, S_2$, $S_1$ là đỉnh) tạo thành hướng quay phải, tức là tích có hướng nhỏ hơn $0$: $\overrightarrow{S_2S_1}\times \overrightarrow{S_1P}<0$, thì loại bỏ đỉnh ngăn xếp, lặp lại kiểm tra cho đến khi $\overrightarrow{S_2S_1}\times \overrightarrow{S_1P}\ge 0$ hoặc ngăn xếp chỉ còn một phần tử.

Thông thường không cần giữ các điểm nằm trên cạnh bao lồi, nên điều kiện "$<$" ở trên có thể thay bằng "$\le$", đồng thời điều kiện sau đổi thành "$>$".

![Andrew](./images/andrew.svg)

#### Cài đặt

???+ note "Cài đặt mã nguồn"
    === "C++"
        ```cpp
        // stk[] là mảng chỉ số nguyên, lưu chỉ số điểm
        // p[] lưu vector hoặc điểm
        tp = 0;                       // Khởi tạo ngăn xếp
        std::sort(p + 1, p + 1 + n);  // Sắp xếp các điểm
        stk[++tp] = 1;
        // Thêm phần tử đầu tiên vào ngăn xếp, không cập nhật used để 1 vẫn được xử lý khi đóng bao lồi
        for (int i = 2; i <= n; ++i) {
          while (tp >= 2  // Dòng dưới: * được nạp chồng thành tích có hướng
                 && (p[stk[tp]] - p[stk[tp - 1]]) * (p[i] - p[stk[tp]]) <= 0)
            used[stk[tp--]] = 0;
          used[i] = 1;  // used đánh dấu điểm nằm trên vỏ
          stk[++tp] = i;
        }
        int tmp = tp;  // tmp lưu kích thước vỏ dưới
        for (int i = n - 1; i > 0; --i)
          if (!used[i]) {
            // ↓ Khi tìm vỏ trên không ảnh hưởng vỏ dưới
            while (tp > tmp && (p[stk[tp]] - p[stk[tp - 1]]) * (p[i] - p[stk[tp]]) <= 0)
              used[stk[tp--]] = 0;
            used[i] = 1;
            stk[++tp] = i;
          }
        for (int i = 1; i <= tp; ++i)  // Sao chép sang mảng mới
          h[i] = p[stk[i]];
        int ans = tp - 1;
        ```
    
    === "Python"
        ```python
        stk = []  # Mảng chỉ số nguyên, lưu chỉ số điểm
        p = []  # Lưu vector hoặc điểm
        tp = 0  # Khởi tạo ngăn xếp
        p.sort()  # Sắp xếp các điểm
        tp = tp + 1
        stk[tp] = 1
        # Thêm phần tử đầu tiên vào ngăn xếp, không cập nhật used để 1 vẫn được xử lý khi đóng bao lồi
        for i in range(2, n + 1):
            while tp >= 2 and (p[stk[tp]] - p[stk[tp - 1]]) * (p[i] - p[stk[tp]]) <= 0:
                # Dòng dưới: * được nạp chồng thành tích có hướng
                used[stk[tp]] = 0
                tp = tp - 1
            used[i] = 1  # used đánh dấu điểm nằm trên vỏ
            tp = tp + 1
            stk[tp] = i
        tmp = tp  # tmp lưu kích thước vỏ dưới
        for i in range(n - 1, 0, -1):
            if used[i] == False:
                #      ↓ Khi tìm vỏ trên không ảnh hưởng vỏ dưới
                while tp > tmp and (p[stk[tp]] - p[stk[tp - 1]]) * (p[i] - p[stk[tp]]) <= 0:
                    used[stk[tp]] = 0
                    tp = tp - 1
                used[i] = 1
                tp = tp + 1
                stk[tp] = i
        for i in range(1, tp + 1):
            h[i] = p[stk[i]]
        ans = tp - 1
        ```

Theo đoạn mã trên, cuối cùng bao lồi có $\textit{ans}$ điểm (mảng $h$ có $\textit{ans}+1$ phần tử, do lưu thêm điểm đầu), các điểm được sắp xếp ngược chiều kim đồng hồ. Chu vi là:

$$
\sum_{i=1}^{\textit{ans}}\left|\overrightarrow{h_ih_{i+1}}\right|
$$

### Thuật toán Graham scan

#### Tính chất

Tương tự Andrew, Graham scan cũng có độ phức tạp $O(n\log n)$, chủ yếu do bước sắp xếp.

#### Quy trình

Đầu tiên, tìm điểm có tung độ nhỏ nhất (nếu bằng nhau thì lấy hoành độ nhỏ nhất), gọi là $P$. Theo định nghĩa bao lồi, điểm này chắc chắn nằm trên bao lồi. Sau đó, sắp xếp tất cả các điểm còn lại theo thứ tự tăng dần của góc cực so với $P$.

![](./images/ch1.svg)

Tương tự Andrew, nếu đi ngược chiều kim đồng hồ trên bao lồi, mỗi bước đều là "rẽ trái". Cụ thể, với ba điểm liên tiếp $P_1, P_2, P_3$ trên bao lồi, luôn có $\overrightarrow{P_1 P_2} \times \overrightarrow{P_2 P_3} \ge 0$.

Dùng một ngăn xếp để lưu các điểm trên bao lồi, đầu tiên đẩy $P$ vào ngăn xếp, sau đó lần lượt thử thêm từng điểm theo thứ tự góc cực. Nếu điểm mới vào ngăn xếp cùng hai điểm trên đỉnh tạo thành "rẽ phải", thì loại bỏ đỉnh ngăn xếp, lặp lại cho đến khi điều kiện thỏa mãn hoặc ngăn xếp chỉ còn một phần tử, rồi đẩy điểm mới vào.

![](./images/ch2.svg)

![](./images/ch3.svg)

???+ note "Cài đặt mã nguồn"
    ```cpp
    struct Point {
      double x, y, ang;
    
      Point operator-(const Point& p) const { return {x - p.x, y - p.y, 0}; }
    } p[MAXN];
    
    double dis(Point p1, Point p2) {
      return sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
    }
    
    bool cmp(Point p1, Point p2) {
      if (p1.ang == p2.ang) {
        return dis(p1, p[1]) < dis(p2, p[1]);
      }
      return p1.ang < p2.ang;
    }
    
    double cross(Point p1, Point p2) { return p1.x * p2.y - p1.y * p2.x; }
    
    int main() {
      for (int i = 2; i <= n; ++i) {
        if (p[i].y < p[1].y || (p[i].y == p[1].y && p[i].x < p[1].x)) {
          std::swap(p[1], p[i]);
        }
      }
      for (int i = 2; i <= n; ++i) {
        p[i].ang = atan2(p[i].y - p[1].y, p[i].x - p[1].x);
      }
      std::sort(p + 2, p + n + 1, cmp);
      sta[++top] = 1;
      for (int i = 2; i <= n; ++i) {
        while (top >= 2 &&
               cross(p[sta[top]] - p[sta[top - 1]], p[i] - p[sta[top]]) < 0) {
          top--;
        }
        sta[++top] = i;
      }
      return 0;
    }
    ```

## Tổng Minkowski

### Định nghĩa

Tổng Minkowski của hai tập điểm $P$ và $Q$ được định nghĩa là $P+Q=\{a+b|a\in P,b\in Q\}$, tức là dịch chuyển mỗi điểm của $P$ theo từng vector của $Q$, tập hợp kết quả là $P+Q$. Ở đây chỉ xét tổng Minkowski của **bao lồi**.

Ví dụ: Với $P=\{(0,0),(-3,3),(2,1)\}$ và $Q=\{(0,0),(-1,3),(1,4),(2,2)\}$,

![](./images/convex-hull1.svg)

Dịch $P$ theo từng vector của $Q$:

![](./images/convex-hull2.svg)

Dễ thấy hình mới cũng là một **bao lồi**:

![](./images/convex-hull3.svg)

### Tính chất

1.  Nếu $P$, $Q$ là tập lồi, thì $P+Q$ cũng là tập lồi.

    ??? note "Chứng minh"
        Giả sử $e,f\in P+Q$, tồn tại $a,b \in P$, $c,d\in Q$ sao cho $e=a+c,f=b+d$, với mọi $t\in[0,1]$:
        
        $$
        \begin{aligned}
        te + (1-t)f &= t(a+c)+(1-t)(b+d)\\
        &=(ta+(1-t)b)+(tc+(1-t)d)\\
        &\in P+Q.
        \end{aligned}
        $$
        
        Q.E.D.
2.  Nếu $P$, $Q$ là tập lồi, thì các cạnh của $P+Q$ là kết quả nối các cạnh của $P$, $Q$ sau khi sắp xếp theo góc cực.

    ??? note "Chứng minh"
        Giả sử các cạnh của $P$ và $Q$ không có cạnh nào cùng độ dốc. Xoay hệ trục tọa độ sao cho một cạnh $XY$ của $P$ song song với trục $x$ và nằm thấp nhất.
        
        Khi đó, điểm thấp nhất của $Q$ là $U$, điểm thấp nhất và trái nhất của $P+Q$ là $A$.
        
        Ta có $\vec{A} = \vec{X} + \vec{U}$, nên $A$ nằm trên biên $P+Q$.
        
        Tương tự, điểm thấp nhất và phải nhất của $P+Q$ là $B$ với $\vec{B} = \vec{Y} + \vec{U}$, cũng nằm trên biên.
        
        Do đó, $\vec{AB} = \vec{XY} + \vec{U}$.
        
        Nếu tiếp tục xoay, ta sẽ nhận được liên tiếp các cạnh của $P+Q$.
        
        Q.E.D.

### Cài đặt

Dựa vào tính chất 2, ta sắp xếp các cạnh của $P,Q$ theo góc cực, lấy $P_1+Q_1$ làm điểm bắt đầu, sau đó dùng kỹ thuật **trộn** (merge) để lần lượt thêm các cạnh.

Độ phức tạp: $O(n+m)$

???+ note "Cài đặt"
    ```cpp
    template <class T>
    struct Point {
      T x, y;
    
      Point(T x = 0, T y = 0) : x(x), y(y) {}
    
      friend Point operator+(const Point &a, const Point &b) {
        return {a.x + b.x, a.y + b.y};
      }
    
      friend Point operator-(const Point &a, const Point &b) {
        return {a.x - b.x, a.y - b.y};
      }
    
      // Tích vô hướng
      friend T operator*(const Point &a, const Point &b) {
        return a.x * b.x + a.y * b.y;
      }
    
      // Tích có hướng
      friend T operator^(const Point &a, const Point &b) {
        return a.x * b.y - a.y * b.x;
      }
    };
    
    template <class T>
    vector<Point<T>> minkowski_sum(vector<Point<T>> a, vector<Point<T>> b) {
      vector<Point<T>> c{a[0] + b[0]};
      for (usz i = 0; i + 1 < a.size(); ++i) a[i] = a[i + 1] - a[i];
      for (usz i = 0; i + 1 < b.size(); ++i) b[i] = b[i + 1] - b[i];
      a.pop_back(), b.pop_back();
      c.resize(a.size() + b.size() + 1);
      merge(a.begin(), a.end(), b.begin(), b.end(), c.begin() + 1,
            [](const Point<i64> &a, const Point<i64> &b) { return (a ^ b) < 0; });
      for (usz i = 1; i < c.size(); ++i) c[i] = c[i] + c[i - 1];
      return c;
    }
    ```

### Bài tập ví dụ

???+ note "[Bài tập ví dụ \[JSOI2018\] Chiến tranh](https://loj.ac/p/2549)"
    Cho hai bao lồi $P,Q$, dịch chuyển $Q$ $q$ lần, hỏi sau mỗi lần dịch chuyển có giao điểm với $P$ không. $1\le n,m\le 10^5,1\le q\le 10^5$.

??? note "Cài đặt"
    ```cpp
    --8<-- "docs/geometry/code/convex-hull/convex-hull_1.cpp"
    ```

## Bao lồi ba chiều

### Kiến thức cơ bản

> Phép nghịch đảo tròn: Với tâm $O$, bán kính $R$, nếu đường thẳng qua $O$ đi qua $P,P'$, và $OP\times OP'=R^{2}$, thì $P$ và $P'$ gọi là nghịch đảo nhau qua $O$.

### Quy trình

Các bước tìm bao lồi ba chiều:

-   Đầu tiên, thực hiện nhiễu nhỏ để tránh trường hợp bốn điểm đồng phẳng.
-   Với bao lồi đã biết, thêm một điểm $P$, coi $P$ là nguồn sáng, chiếu tia tới bao lồi, các mặt nhìn thấy và không nhìn thấy được phân tách bởi các cạnh.
-   Xóa các mặt nhìn thấy, thêm các mặt mới tạo bởi các cạnh phân tách và $P$.
    Lặp lại quá trình này. Theo [Định lý Pick](./pick.md), công thức Euler ($V−E+F=2$ với đa diện lồi), và phép nghịch đảo tròn, độ phức tạp $O(n^2)$.[^3d-v]

### Bài mẫu

[P4724【Mẫu】Bao lồi ba chiều](https://www.luogu.com.cn/problem/P4724)

Lặp lại quy trình trên để có đáp án.

???+ note "Cài đặt mã nguồn"
    ```cpp
    --8<-- "docs/geometry/code/3d/3d_1.cpp"
    ```

## Bài tập luyện tập

-   [UVa11626 Convex Hull](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=78&page=show_problem&problem=2673)

-   [「USACO5.1」圈奶牛 Fencing the Cows](https://www.luogu.com.cn/problem/P2742)

-   [POJ1873 The Fortified Forest](http://poj.org/problem?id=1873)

-   [POJ1113 Wall](http://poj.org/problem?id=1113)

-   [USACO22JAN Multiple Choice Test P](https://www.luogu.com.cn/problem/P8101)

-   [「SHOI2012」信用卡凸包](https://www.luogu.com.cn/problem/P3829)

## Tài liệu tham khảo & chú thích

[^3d-v]: [Ghi chú học tập về bao lồi 3D](https://www.cnblogs.com/xzyxzy/p/10225804.html)
