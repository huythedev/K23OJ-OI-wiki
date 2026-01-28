author: Chrogeek, frank-xjh, ChungZH, hsfzLZH1, Marcythm, Planet6174, partychicken, i-Yirannn

## Khoảng cách Euclid (欧氏距离)

### Không gian hai chiều

#### Định nghĩa

Khoảng cách Euclid, còn gọi là khoảng cách Euclid (Euclidean distance). Trong hệ tọa độ vuông góc, giả sử hai điểm $A,B$ có tọa độ lần lượt là $A(x_1,y_1),B(x_2,y_2)$, thì khoảng cách Euclid giữa hai điểm là:

$$
\left | AB \right | = \sqrt{\left ( x_2 - x_1 \right )^2 + \left ( y_2 - y_1 \right )^2}
$$

#### Giải thích

Ví dụ, trong hệ tọa độ vuông góc, có hai điểm $A(6,5),B(2,2)$, áp dụng công thức trên, ta dễ dàng tính được khoảng cách Euclid giữa $A$ và $B$:

$$
\left | AB \right | = \sqrt{\left ( 2 - 6 \right )^2 + \left ( 2 - 5 \right )^2} = \sqrt{4^2+3^2} = 5
$$

Ngoài ra, khoảng cách Euclid từ điểm $P(x,y)$ đến gốc tọa độ có thể được biểu diễn như sau:

$$
|P| = \sqrt{x^2+y^2}
$$

### Không gian n chiều

#### Dẫn nhập

Vậy trong không gian ba chiều, công thức khoảng cách Euclid là gì? Hãy quan sát hình dưới.

![dis-3-dimensional](./images/distance-0.png)

Ta dễ dàng nhận thấy, trong tam giác $\triangle ADC$, $\angle ADC = 90^\circ$; trong tam giác $\triangle ACB$, $\angle ACB = 90^\circ$.

$$
\begin{aligned}
\therefore ~ |AB| &= \sqrt{|AC|^2+|BC|^2} \\
&= \sqrt{|AD|^2+|CD|^2+|BC|^2}
\end{aligned}
$$

#### Định nghĩa

Từ đó, công thức khoảng cách Euclid trong không gian ba chiều là:

$$
\begin{gathered}
\left | AB \right | = \sqrt{\left ( x_2 - x_1 \right )^2 + \left ( y_2 - y_1 \right )^2 + \left ( z_2 - z_1 \right )^2} \\
|P| = \sqrt{x^2+y^2+z^2}
\end{gathered}
$$

#### Giải thích

[NOIP2017 Bảng nâng cao - Bài "Cheese"](https://uoj.ac/problem/332) đã sử dụng kiến thức này, có thể xem như một ví dụ điển hình về khoảng cách Euclid.

Tương tự, ta có công thức khoảng cách Euclid trong không gian $n$ chiều: Với $\vec A(x_{11}, x_{12}, \cdots,x_{1n}) ,~ \vec B(x_{21}, x_{22}, \cdots,x_{2n})$:

$$
\begin{aligned}
\lVert\overrightarrow{AB}\rVert &= \sqrt{\left ( x_{11} - x_{21} \right )^2 + \left ( x_{12} - x_{22} \right )^2 + \cdot \cdot \cdot +\left ( x_{1n} - x_{2n} \right )^2}\\
&= \sqrt{\sum_{i = 1}^{n}(x_{1i} - x_{2i})^2}
\end{aligned}
$$

Khoảng cách Euclid rất hữu dụng, nhưng cũng có nhược điểm rõ rệt: Khi tính khoảng cách giữa hai điểm nguyên, kết quả thường là số thực, có thể gây sai số do làm tròn.

## Khoảng cách Manhattan

### Định nghĩa

Trong không gian hai chiều, khoảng cách Manhattan (Manhattan distance) giữa hai điểm là tổng giá trị tuyệt đối hiệu hoành độ và tung độ của chúng. Giả sử $A(x_1,y_1),B(x_2,y_2)$, thì khoảng cách Manhattan giữa $A$ và $B$ là:

$$
d(A,B) = |x_1 - x_2| + |y_1 - y_2|
$$

### Giải thích

Quan sát hình dưới:

![manhattan-dis-diff](./images/distance-1.png)

Giữa $A$ và $B$, các đoạn màu vàng, cam đều biểu diễn khoảng cách Manhattan, còn đoạn đỏ, xanh lam cũng là các cách đi tương đương, đoạn xanh lá là khoảng cách Euclid.

Tương tự, trong hình dưới, $A,B$ có tọa độ lần lượt là $A(25,20),B(10,10)$.

![manhattan-dis](./images/distance-2.svg)

Áp dụng công thức, ta dễ dàng tính được khoảng cách Manhattan giữa $A$ và $B$:

$$
d(A,B) = |20 - 10| + |25 - 10| = 10 + 15 = 25
$$

Suy rộng ra, công thức khoảng cách Manhattan trong không gian $n$ chiều là:

$$
\begin{aligned}
d(A,B) &= |x_1 - y_1| + |x_2 - y_2| + \cdot \cdot \cdot + |x_n - y_n|\\
&= \sum_{i = 1}^{n}|x_i - y_i|
\end{aligned}
$$

### Tính chất

Ngoài công thức, khoảng cách Manhattan còn có các tính chất toán học sau:

-   Không âm: Khoảng cách Manhattan luôn không âm, tức là $d(i,j)\geq 0$.
-   Đơn vị: Khoảng cách từ một điểm đến chính nó là $0$, tức $d(i,i) = 0$.
-   Đối xứng: Khoảng cách từ $A$ đến $B$ bằng khoảng cách từ $B$ đến $A$, tức $d(i,j) = d(j,i)$.
-   Bất đẳng thức tam giác: Khoảng cách trực tiếp từ $i$ đến $j$ không lớn hơn tổng khoảng cách qua bất kỳ điểm $k$ nào, tức $d(i,j)\leq d(i,k)+d(k,j)$.

### Bài tập ví dụ

[P5098「USACO04OPEN」Cave Cows 3](https://www.luogu.com.cn/problem/P5098)

Theo đề bài, với công thức $|x_1-x_2|+|y_1-y_2|$, ta có thể giả sử $x_1 - x_2 \geq 0$, rồi chia thành hai trường hợp theo dấu của $y_1 - y_2$:

-   $(y_1 - y_2 \geq 0)\rightarrow |x_1-x_2|+|y_1-y_2|=x_1 + y_1 - (x_2 + y_2)$

-   $(y_1 - y_2 < 0)\rightarrow |x_1-x_2|+|y_1-y_2|=x_1 - y_1 - (x_2 - y_2)$

Chỉ cần tìm giá trị lớn nhất và nhỏ nhất của $x+y, x-y$ là ra đáp án.

??? note "Mã mẫu tham khảo"
    === "C++"
        ```cpp
        #include <algorithm>
        #include <cstdio>
        using namespace std;
        
        int main() {
          int n, x, y, minx = 0x7fffffff, maxx = 0, miny = 0x7fffffff, maxy = 0;
          scanf("%d", &n);
          for (int i = 1; i <= n; i++) {
            scanf("%d%d", &x, &y);
            minx = min(minx, x + y), maxx = max(maxx, x + y);
            miny = min(miny, x - y), maxy = max(maxy, x - y);
          }
          printf("%d\n", max(maxx - minx, maxy - miny));
          return 0;
        }
        ```
    
    === "Python"
        ```python
        minx = 0x7FFFFFFF
        maxx = 0
        miny = 0x7FFFFFFF
        maxy = 0
        n = int(input())
        for i in range(1, n + 1):
            x, y = map(lambda x: int(x), input().split())
            minx = min(minx, x + y)
            maxx = max(maxx, x + y)
            miny = min(miny, x - y)
            maxy = max(maxy, x - y)
        print(max(maxx - minx, maxy - miny))
        ```

Thực ra còn một cách khác, đó là chuyển đổi khoảng cách Manhattan thành khoảng cách Chebyshev, sẽ trình bày ở phần sau.

## Khoảng cách Chebyshev (切比雪夫距离)

### Định nghĩa

Khoảng cách Chebyshev (Chebyshev distance) là một loại độ đo trong không gian vectơ, khoảng cách giữa hai điểm được định nghĩa là giá trị lớn nhất trong các hiệu tuyệt đối từng tọa độ.[^ref1]

Trong không gian hai chiều, khoảng cách Chebyshev giữa hai điểm là giá trị lớn nhất giữa hiệu tuyệt đối hoành độ và tung độ. Giả sử $A(x_1,y_1),B(x_2,y_2)$, thì:

$$
d(A,B) = \max(|x_1 - x_2|, |y_1 - y_2|)
$$

Trong không gian $n$ chiều, công thức là:

$$
\begin{aligned}
d(x,y) &= \max\begin{Bmatrix} |x_1 - y_1|,|x_2 - y_2|,\cdot \cdot \cdot,|x_n - y_n|\end{Bmatrix} \\
&= \max\begin{Bmatrix} |x_i - y_i|\end{Bmatrix}(i \in [1, n])\end{aligned}
$$

### Giải thích

Vẫn với ví dụ trên, trong hình dưới $A,B$ có tọa độ lần lượt là $A(25,20),B(10,10)$.

![Chebyshev-dis](./images/distance-2.svg)

$$
d(A,B) = \max(|20 - 10|, |25 - 10|) = \max(10, 15) = 15
$$

## Chuyển đổi giữa khoảng cách Manhattan và Chebyshev

### Quá trình

Trước tiên, hãy vẽ tập hợp các điểm có khoảng cách Manhattan đến gốc tọa độ bằng $1$.

Theo công thức, ta có phương trình $|x| + |y| = 1$.

Khai triển giá trị tuyệt đối, ta được $4$ hàm bậc nhất:

$$
\begin{aligned}
&y = -x + 1 &(x \geq 0, y \geq 0) \\
&y = x + 1 &(x \leq 0, y \geq 0) \\
&y = x - 1  &(x \geq 0, y \leq 0)  \\
&y = -x - 1  &(x \leq 0, y \leq 0) \\
\end{aligned}
$$

Vẽ các hàm này trên hệ tọa độ, ta được một hình vuông cạnh $\sqrt{2}$ như hình dưới:

![dis-diff-square-1](./images/distance-3.svg)

Tất cả các điểm trên biên hình vuông này đều có khoảng cách Manhattan đến gốc tọa độ là $1$.

Tương tự, hãy vẽ tập hợp các điểm có khoảng cách Chebyshev đến gốc tọa độ bằng $1$.

Theo công thức, ta có $\max(|x|,|y|)=1$.

Khai triển, ta cũng được $4$ đoạn thẳng:

$$
\begin{aligned}
&y = 1&(-1\leq x \leq 1) \\
&y = -1&(-1\leq x \leq 1) \\
&x = 1,&(-1\leq y \leq 1) \\
&x = -1,&(-1\leq y \leq 1) \\
\end{aligned}
$$

Vẽ lên hệ tọa độ, ta được một hình vuông cạnh $2$ như hình dưới:

![dis-diff-square-2](./images/distance-4.svg)

Tất cả các điểm trên biên hình vuông này đều có khoảng cách Chebyshev đến gốc tọa độ là $1$.

So sánh hai hình, ta thấy:

Hai hình vuông này là hai hình đồng dạng.

### Chứng minh

Vậy, liệu có mối liên hệ nào giữa khoảng cách Manhattan và Chebyshev không?

Hãy chứng minh ngắn gọn:

Giả sử $A(x_1,y_1),B(x_2,y_2)$,

Khai triển giá trị tuyệt đối trong khoảng cách Manhattan, ta được bốn giá trị, giá trị lớn nhất là tổng hai số không âm, tức là khoảng cách Manhattan. Khi đó:

$$
\begin{aligned}
d(A,B)&=|x_1 - x_2| + |y_1 - y_2|\\
&=\max\begin{Bmatrix} x_1 - x_2 + y_1 - y_2, x_1 - x_2 + y_2 - y_1,x_2 - x_1 + y_1 - y_2, x_2 - x_1 + y_2 - y_1\end{Bmatrix}\\
&= \max(|(x_1 + y_1) - (x_2 + y_2)|, |(x_1 - y_1) - (x_2 - y_2)|)
\end{aligned}
$$

Ta thấy, đây chính là khoảng cách Chebyshev giữa hai điểm $(x_1 + y_1,x_1 - y_1)$ và $(x_2 + y_2,x_2 - y_2)$.

Vậy, nếu chuyển mỗi điểm $(x,y)$ thành $(x + y, x - y)$, thì khoảng cách Chebyshev trong hệ tọa độ mới chính là khoảng cách Manhattan trong hệ cũ.

Tương tự, khoảng cách Chebyshev giữa $A,B$ là:

$$
\begin{aligned}
d(A,B)&=\max\begin{Bmatrix} |x_1 - x_2|,|y_1 - y_2|\end{Bmatrix}\\
&=\max\begin{Bmatrix} \left|\dfrac{x_1 + y_1}{2}-\dfrac{x_2 + y_2}{2}\right|+\left|\dfrac{x_1 - y_1}{2}-\dfrac{x_2 - y_2}{2}\right|\end{Bmatrix}
\end{aligned}
$$

Đây chính là khoảng cách Manhattan giữa hai điểm $(\dfrac{x_1 + y_1}{2},\dfrac{x_1 - y_1}{2})$ và $(\dfrac{x_2 + y_2}{2},\dfrac{x_2 - y_2}{2})$.

Vậy, nếu chuyển mỗi điểm $(x,y)$ thành $(\dfrac{x + y}{2},\dfrac{x - y}{2})$, thì khoảng cách Manhattan trong hệ mới chính là khoảng cách Chebyshev trong hệ cũ.

### Kết luận

-   Hệ tọa độ Manhattan là hệ Chebyshev quay $45^\circ$ rồi thu nhỏ một nửa.
-   Nếu chuyển điểm $(x,y)$ thành $(x + y, x - y)$, thì khoảng cách Manhattan trong hệ cũ bằng khoảng cách Chebyshev trong hệ mới.
-   Nếu chuyển điểm $(x,y)$ thành $(\dfrac{x + y}{2},\dfrac{x - y}{2})$, thì khoảng cách Chebyshev trong hệ cũ bằng khoảng cách Manhattan trong hệ mới.

Khi gặp bài toán về khoảng cách Chebyshev hoặc Manhattan, ta có thể chuyển đổi qua lại để giải quyết. Hai loại khoảng cách này có ưu nhược điểm riêng, cần linh hoạt áp dụng.

### Bài tập ví dụ

[P4648「IOI2007」pairs Số cặp động vật](https://www.luogu.com.cn/problem/P4648) (Chuyển Manhattan sang Chebyshev)

[P3964「TJOI2013」Hội tụ sóc](https://www.luogu.com.cn/problem/P3964) (Chuyển Chebyshev sang Manhattan)

Cuối cùng là cách giải thứ hai cho [P5098「USACO04OPEN」Cave Cows 3](https://www.luogu.com.cn/problem/P5098):

Chuyển bài toán tìm khoảng cách Manhattan thành tìm khoảng cách Chebyshev, tức là chuyển mỗi điểm $(x,y)$ thành $(x + y, x - y)$.

Khi đó, đáp án là $\max\limits_{i,j\in n}\begin{Bmatrix} \max\begin{Bmatrix} |x_i - x_j|,|y_i - y_j|\end{Bmatrix}\end{Bmatrix}$.

Để hiệu hoành độ hoặc tung độ lớn nhất, chỉ cần tìm giá trị lớn nhất và nhỏ nhất của $x,y$.

??? note "Mã mẫu tham khảo"
    === "C++"
        ```cpp
        #include <algorithm>
        #include <cstdio>
        using namespace std;
        
        int main() {
          int n, x, y, a, b, minx = 0x7fffffff, maxx = 0, miny = 0x7fffffff, maxy = 0;
          scanf("%d", &n);
          for (int i = 1; i <= n; i++) {
            scanf("%d%d", &a, &b);
            x = a + b, y = a - b;
            minx = min(minx, x), maxx = max(maxx, x);
            miny = min(miny, y), maxy = max(maxy, y);
          }
          printf("%d\n", max(maxx - minx, maxy - miny));
          return 0;
        }
        ```
    
    === "Python"
        ```python
        minx = 0x7FFFFFFF
        maxx = 0
        miny = 0x7FFFFFFF
        maxy = 0
        n = int(input())
        for i in range(1, n + 1):
            a, b = map(lambda x: int(x), input().split())
            x = a + b
            y = a - b
            minx = min(minx, x)
            maxx = max(maxx, x)
            miny = min(miny, y)
            maxy = max(maxy, y)
        print(max(maxx - minx, maxy - miny))
        ```

So sánh hai đoạn mã, ta thấy hai cách làm khác nhau nhưng mã lại hoàn toàn tương đương, thật thú vị phải không? Tất nhiên, các kiến thức sâu hơn các bạn có thể tự tìm hiểu thêm.

## Khoảng cách Minkowski (闵可夫斯基距离)

Ta định nghĩa khoảng cách Minkowski trong không gian $n$ chiều giữa hai điểm $X(x_1, x_2, \dots, x_n)$, $Y(y_1, y_2, \dots, y_n)$ là:

$$
D(X, Y) = \left(\sum_{i=1}^n \left\vert x_i - y_i \right\vert ^p\right)^{\frac{1}{p}}.
$$

Đặc biệt:

1.  Khi $p=1$, $D(X, Y) = \sum_{i=1}^n \left\vert x_i - y_i \right\vert$ chính là khoảng cách Manhattan;
2.  Khi $p=2$, $D(X, Y) = \left(\sum_{i=1}^n (x_i - y_i)^2\right)^{1/2}$ chính là khoảng cách Euclid;
3.  Khi $p \to \infty$, $D(X, Y) = \lim_{p \to \infty}\left(\sum_{i=1}^n \left\vert x_i - y_i \right\vert ^p\right) ^{1/p} = \max\limits_{i=1}^n \left\vert x_i - y_i \right\vert$ chính là khoảng cách Chebyshev.

Lưu ý: Chỉ khi $p \ge 1$, khoảng cách Minkowski mới là một metric (độ đo), chứng minh chi tiết xem tại [Minkowski distance - Wikipedia](https://en.wikipedia.org/wiki/Minkowski_distance).

## Tài liệu tham khảo & liên kết

1.  [Bàn về ba loại thuật toán tính khoảng cách thường gặp](https://www.luogu.com.cn/blog/xuxing/Distance-Algorithm), cảm ơn tác giả xuxing đã cho phép sử dụng.

[^ref1]: [Khoảng cách Chebyshev - Wikipedia tiếng Việt](https://zh.wikipedia.org/wiki/%E5%88%87%E6%AF%94%E9%9B%AA%E5%A4%AB%E8%B7%9D%E7%A6%BB)
