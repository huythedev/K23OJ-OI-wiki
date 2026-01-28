Trang này sẽ giới thiệu về thuật toán **xoay calip** (rotating calipers).

## Dẫn nhập

Thuật toán xoay calip (Rotating Calipers, còn gọi là "xoay thước kẹp") là một kỹ thuật dựa trên thuật toán tìm bao lồi (convex hull), cho phép liệt kê các cạnh của bao lồi đồng thời duy trì các điểm đặc biệt cần thiết, từ đó giải quyết hiệu quả các bài toán liên quan đến tính chất của bao lồi như: tìm đường kính bao lồi, tìm hình chữ nhật nhỏ nhất bao phủ, v.v... với độ phức tạp tuyến tính.

???+ note "Tên gọi tiếng Trung của thuật toán"
    Thuật toán này thường được gọi là "旋转卡壳" trong tiếng Trung, có thể hiểu là: với mỗi cạnh đang xét, ta có thể vẽ các đường thẳng (song song hoặc vuông góc với cạnh đó) đi qua các điểm đặc biệt để "kẹp" bao lồi lại. Khi cạnh được duyệt theo thứ tự xoay, các đường thẳng này cũng "xoay" theo, tạo thành quá trình "xoay calip".
    
    Tên tiếng Anh "rotating calipers" dịch sát nghĩa là "xoay thước kẹp", trong đó "calipers" là dụng cụ đo khoảng cách. Bài báo đầu tiên đề xuất thuật ngữ này[^ref1] mô tả ý tưởng: dùng một bộ "thước kẹp" động để kẹp bao lồi, rồi xoay quanh bao lồi để tìm các giá trị cực trị.

## Tìm đường kính bao lồi

???+ note "Ví dụ 1 :[Luogu P1452 Beauty Contest G](https://www.luogu.com.cn/problem/P1452)"
    Cho $n$ điểm trên mặt phẳng, hãy tìm cặp điểm có khoảng cách lớn nhất. ($2\leq n \leq 50000,|x|,|y| \leq 10^4$)

### Quy trình

Trước tiên, sử dụng một thuật toán bao lồi bất kỳ để tìm bao lồi của tập điểm. Cặp điểm có khoảng cách lớn nhất chắc chắn nằm trên bao lồi. Do hình dạng của bao lồi, khi duyệt các cạnh theo chiều ngược kim đồng hồ, với mỗi cạnh, ta tìm điểm xa nhất so với cạnh đó. Khi cạnh xoay, điểm xa nhất cũng sẽ di chuyển ngược chiều kim đồng hồ, không có trường hợp "quay ngược". Điều này cho phép ta duyệt các cạnh theo thứ tự và duy trì một chỉ số điểm xa nhất, liên tục cập nhật đáp án.

Mảng lưu các đỉnh của bao lồi sẽ tự động có thứ tự ngược chiều kim đồng hồ. Lưu ý cần thêm lại đỉnh đầu tiên vào cuối mảng để khi duyệt các cạnh $(i,i+1)$ không bị thiếu cạnh cuối.

![](images/rotating-calipers1.png)

Khi duyệt, với mỗi cạnh, kiểm tra xem điểm $j+1$ có xa hơn cạnh $(i,i+1)$ so với $j$ không. Nếu có thì tăng $j$, ngược lại $j$ chính là điểm tối ưu cho cạnh này. Để so sánh khoảng cách từ điểm đến cạnh, có thể dùng giá trị tuyệt đối của tích có hướng (diện tích tam giác) để so sánh.

### Cài đặt

???+ note "Mã nguồn lõi"
    === "C++"
        ```cpp
        int sta[N], top;  // 将凸包上的节点编号存在栈里，第一个和最后一个节点编号相同
        
        ll pf(ll x) { return x * x; }
        
        ll dis(int p, int q) { return pf(a[p].x - a[q].x) + pf(a[p].y - a[q].y); }
        
        ll sqr(int p, int q, int y) { return abs((a[q] - a[p]) * (a[y] - a[q])); }
        
        ll mx;
        
        void get_longest() {  // 求凸包直径
          int j = 3;
          if (top < 4) {
            mx = dis(sta[1], sta[2]);
            return;
          }
          for (int i = 1; i < top; ++i) {
            while (sqr(sta[i], sta[i + 1], sta[j]) <=
                   sqr(sta[i], sta[i + 1], sta[j % top + 1]))
              j = j % top + 1;
            mx = max(mx, max(dis(sta[i + 1], sta[j]), dis(sta[i], sta[j])));
          }
        }
        ```
    
    === "Python"
        ```python
        sta = [0] * N
        top = 0  # 将凸包上的节点编号存在栈里，第一个和最后一个节点编号相同
        
        
        def pf(x):
            return x * x
        
        
        def dis(p, q):
            return pf(a[p].x - a[q].x) + pf(a[p].y - a[q].y)
        
        
        def sqr(p, q, y):
            return abs((a[q] - a[p]) * (a[y] - a[q]))
        
        
        def get_longest():  # 求凸包直径
            j = 3
            if top < 4:
                mx = dis(sta[1], sta[2])
                return
            for i in range(1, top):
                while sqr(sta[i], sta[i + 1], sta[j]) <= sqr(
                    sta[i], sta[i + 1], sta[j % top + 1]
                ):
                    j = j % top + 1
                mx = max(mx, max(dis(sta[i + 1], sta[j]), dis(sta[i], sta[j])))
        ```

## Tìm hình chữ nhật bao nhỏ nhất

[Luogu P3187 Hình chữ nhật bao nhỏ nhất](https://www.luogu.com.cn/problem/P3187)

Cho tập hợp các điểm, hãy tìm hình chữ nhật có diện tích nhỏ nhất bao phủ tất cả các điểm. ($3\leq n \leq 50000$)

### Quy trình

Tương tự bài trên, ta vẫn dùng phương pháp xoay calip, nhưng lần này cần tìm diện tích nhỏ nhất. Nếu chỉ duy trì một điểm tối ưu thì chỉ tìm được hai đường song song gần nhất, nhưng hình chữ nhật cần xác định cả bốn cạnh. Do đó, ta cần duy trì ba điểm: một điểm đối diện cạnh đang xét (tối ưu hóa bằng tích có hướng), hai điểm ở hai phía còn lại (tối ưu hóa bằng tích vô hướng, tức là so sánh độ dài chiếu lên phương vuông góc). Hai giá trị này độc lập nên khi xác định được ba điểm tối ưu, ta có thể tính diện tích hình chữ nhật ứng với cạnh đang xét.

![](images/rotating-calipers2.png)

Nếu chỉ cần diện tích, có thể dùng công thức kết hợp tích có hướng và tích vô hướng như sau. Gọi diện tích hai lần của phần màu tím là $S$, diện tích hình chữ nhật là

$$
S\times (|\overrightarrow{AD}\cdot \overrightarrow{AB}|+|\overrightarrow{BC}\cdot \overrightarrow{BA}|-|\overrightarrow{AB}\cdot \overrightarrow{BA}|)/|\overrightarrow{AB}\cdot \overrightarrow{BA}|
$$

### Cài đặt

Bỏ qua phần tìm bao lồi, dưới đây là đoạn mã lõi:

???+ note "Mã nguồn lõi"
    === "C++"
        ```cpp
        void get_biggest() {
          int j = 3, l = 2, r = 2;
          double t1, t2, t3, ans = 2e10;
          for (int i = 1; i < top; ++i) {
            while (sqr(sta[i], sta[i + 1], sta[j]) <=
                   sqr(sta[i], sta[i + 1], sta[j % top + 1]))
              j = j % top + 1;
            while (dot(sta[i + 1], sta[r % top + 1], sta[i]) >=
                   dot(sta[i + 1], sta[r], sta[i]))
              r = r % top + 1;
            if (i == 1) l = r;
            while (dot(sta[i + 1], sta[l % top + 1], sta[i]) <=
                   dot(sta[i + 1], sta[l], sta[i]))
              l = l % top + 1;
            t1 = sqr(sta[i], sta[i + 1], sta[j]);
            t2 = dot(sta[i + 1], sta[r], sta[i]) + dot(sta[i + 1], sta[l], sta[i]);
            t3 = dot(sta[i + 1], sta[i + 1], sta[i]);
            ans = min(ans, t1 * t2 / t3);
          }
        }
        ```
    
    === "Python"
        ```python
        def get_biggest():
            j = 3
            l = 2
            r = 2
            ans = 2e10
            for i in range(1, top):
                while sqr(sta[i], sta[i + 1], sta[j]) <= sqr(
                    sta[i], sta[i + 1], sta[j % top + 1]
                ):
                    j = j % top + 1
                while dot(sta[i + 1], sta[r % top + 1], sta[i]) >= dot(
                    sta[i + 1], sta[r], sta[i]
                ):
                    r = r % top + 1
                if i == 1:
                    l = r
                while dot(sta[i + 1], sta[l % top + 1], sta[i]) <= dot(
                    sta[i + 1], sta[l], sta[i]
                ):
                    l = l % top + 1
                t1 = sqr(sta[i], sta[i + 1], sta[j])
                t2 = dot(sta[i + 1], sta[r], sta[i]) + dot(sta[i + 1], sta[l], sta[i])
                t3 = dot(sta[i + 1], sta[i + 1], sta[i])
                ans = min(ans, t1 * t2 / t3)
        ```

## Bài tập luyện tập

-   [POJ 3608. Bridge Across Islands](http://poj.org/problem?id=3608)
-   [2011 ACM-ICPC World Finals, Problem K. Trash Removal](https://codeforces.com/gym/101175)
-   [ICPC WF Moscow Invitational Contest - Online Mirror, Problem F. Framing Pictures](https://codeforces.com/contest/1578/problem/F)

## Tài liệu tham khảo & chú thích

[^ref1]: Toussaint, Godfried T. (1983). "Solving geometric problems with the rotating calipers". Proc. MELECON '83, Athens. CiteSeerX 10.1.1.155.5671

-   <https://en.wikipedia.org/wiki/Rotating_calipers>

-   <http://www-cgrl.cs.mcgill.ca/~godfried/research/calipers.html>

-   Shamos, Michael (1978). "Computational Geometry" (PDF). Yale University. pp. 76–81.
