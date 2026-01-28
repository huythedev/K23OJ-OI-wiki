author: hyp1231, 383494

## Dẫn nhập

Phép nghịch đảo (phép phản hình tròn) rất hữu ích trong các bài toán có nhiều quan hệ tiếp xúc giữa các đường tròn/đường thẳng. Bằng cách sử dụng các tính chất của phép nghịch đảo, ta có thể chuyển bài toán sang không gian nghịch đảo để giải quyết, giúp đơn giản hóa đáng kể các phép tính.

## Định nghĩa

Cho tâm nghịch đảo $O$ và bán kính nghịch đảo $R$. Với điểm $P$ trên mặt phẳng, điểm $P'$ được gọi là ảnh nghịch đảo của $P$ nếu:

-   $P'$ nằm trên tia $\overrightarrow{OP}$
-   $|OP| \cdot |OP'| = R^2$

Khi đó, $P$ và $P'$ gọi là hai điểm nghịch đảo của nhau.

## Giải thích

Hình dưới minh họa ảnh nghịch đảo của một điểm $P$ trên mặt phẳng:

![Inv1](./images/inverse1.png)

## Tính chất

1.  Ảnh nghịch đảo của điểm nằm ngoài đường tròn $O$ sẽ nằm trong đường tròn $O$ và ngược lại; điểm nằm trên đường tròn $O$ là điểm bất biến (ảnh nghịch đảo là chính nó).

2.  Đường tròn $A$ không đi qua $O$, ảnh nghịch đảo của nó cũng là một đường tròn không đi qua $O$.

    ![Inv2](./images/inverse2.png)

    -   Gọi bán kính đường tròn $A$ là $r_1$, bán kính ảnh nghịch đảo $B$ là $r_2$, ta có:

        $$
        r_2 = \frac{1}{2}\left(\frac{1}{|OA| - r_1} - \frac{1}{|OA| + r_1}\right) R^2
        $$

    ???+ note "Chứng minh"
        ![Inv3](./images/inverse3.png)
        
        Theo định nghĩa phép nghịch đảo:
        
        $$
        \begin{aligned}
        |OC|\cdot|OC'| &= (|OA|+r_1)\cdot(|OB|-r_2) = R^2 \\
        |OD|\cdot|OD'| &= (|OA|-r_1)\cdot(|OB|+r_2) = R^2
        \end{aligned}
        $$
        
        Khử $|OB|$, giải hệ phương trình là ra.

    -   Gọi tọa độ $O$ là $(x_0, y_0)$, $A$ là $(x_1, y_1)$, $B$ là $(x_2, y_2)$, ta có:

        $$
        \begin{aligned}
        x_2 &= x_0 + \frac{|OB|}{|OA|} (x_1 - x_0) \\
        y_2 &= y_0 + \frac{|OB|}{|OA|} (y_1 - y_0)
        \end{aligned}
        $$

        Trong đó $|OB|$ được tính trong quá trình tìm $r_2$ ở trên.

3.  Đường tròn $A$ đi qua $O$, ảnh nghịch đảo của nó là một đường thẳng không đi qua $O$. Vì điểm trên $A$ càng gần $O$ thì ảnh nghịch đảo càng xa $O$ vô hạn.

    ![Inv4](./images/inverse4.png)

4.  Nếu hai hình tiếp xúc tại một điểm khác $O$, thì ảnh nghịch đảo của chúng cũng tiếp xúc.

## Bài tập ví dụ

### [「ICPC 2013 Hàng Châu」Problem of Apollonius](https://acm.hdu.edu.cn/showproblem.php?pid=4773)

#### Đề bài tóm tắt

Tìm tất cả các đường tròn đi qua một điểm ngoài hai đường tròn cho trước và tiếp xúc với cả hai đường tròn đó.

#### Hướng giải

Nếu giải bằng hình học giải tích thì khá phức tạp.

Hãy lấy điểm cần đi qua làm tâm nghịch đảo (bán kính tùy ý). Khi đó, ảnh nghịch đảo của đường tròn cần tìm là một đường thẳng (theo tính chất 3), và ảnh nghịch đảo của hai đường tròn cho trước (theo tính chất 2) cũng là hai đường tròn. Theo tính chất 4, các tiếp xúc được bảo toàn.

Vậy bài toán chuyển thành: Tìm tất cả các tiếp tuyến chung của hai đường tròn.

Sau khi tìm được các tiếp tuyến chung, thực hiện phép nghịch đảo ngược để thu được các đường tròn cần tìm trên mặt phẳng gốc.

??? note "Mã mẫu tham khảo"
    ```cpp
    #include <algorithm>
    #include <cmath>
    #include <cstdio>
    #include <cstring>
    #include <iostream>
    #include <vector>
    using namespace std;
    
    constexpr double EPS = 1e-8;   // 精度系数
    const double PI = acos(-1.0);  // π
    constexpr int N = 4;
    
    // 点的定义
    struct Point {
      double x, y;
    
      Point(double x = 0, double y = 0) : x(x), y(y) {}
    
      bool operator<(Point A) const { return x == A.x ? y < A.y : x < A.x; }
    };
    
    // 向量的定义
    using Vector = Point;
    
    // 向量加法
    Vector operator+(Vector A, Vector B) { return Vector(A.x + B.x, A.y + B.y); }
    
    // 向量减法
    Vector operator-(Vector A, Vector B) { return Vector(A.x - B.x, A.y - B.y); }
    
    // 向量数乘
    Vector operator*(Vector A, double p) { return Vector(A.x * p, A.y * p); }
    
    // 向量数除
    Vector operator/(Vector A, double p) { return Vector(A.x / p, A.y / p); }
    
    // 与0的关系
    int dcmp(double x) {
      if (fabs(x) < EPS) return 0;
      return x < 0 ? -1 : 1;
    }
    
    // 向量点乘
    double Dot(Vector A, Vector B) { return A.x * B.x + A.y * B.y; }
    
    // 向量长度
    double Length(Vector A) { return sqrt(Dot(A, A)); }
    
    // 向量叉乘
    double Cross(Vector A, Vector B) { return A.x * B.y - A.y * B.x; }
    
    // 点在直线上投影
    Point GetLineProjection(Point P, Point A, Point B) {
      Vector v = B - A;
      return A + v * (Dot(v, P - A) / Dot(v, v));
    }
    
    // 圆
    struct Circle {
      Point c;
      double r;
    
      Circle() : c(Point(0, 0)), r(0) {}
    
      Circle(Point c, double r = 0) : c(c), r(r) {}
    
      // 输入极角返回点坐标
      Point point(double a) { return Point(c.x + cos(a) * r, c.y + sin(a) * r); }
    };
    
    // 两圆公切线 返回切线的条数，-1表示无穷多条切线
    // a[i] 和 b[i] 分别是第i条切线在圆A和圆B上的切点
    int getTangents(Circle A, Circle B, Point* a, Point* b) {
      int cnt = 0;
      if (A.r < B.r) {
        swap(A, B);
        swap(a, b);
      }
      double d2 =
          (A.c.x - B.c.x) * (A.c.x - B.c.x) + (A.c.y - B.c.y) * (A.c.y - B.c.y);
      double rdiff = A.r - B.r;
      double rsum = A.r + B.r;
      if (dcmp(d2 - rdiff * rdiff) < 0) return 0;  // 内含
    
      double base = atan2(B.c.y - A.c.y, B.c.x - A.c.x);
      if (dcmp(d2) == 0 && dcmp(A.r - B.r) == 0) return -1;  // 无限多条切线
      if (dcmp(d2 - rdiff * rdiff) == 0) {  // 内切，一条切线
        a[cnt] = A.point(base);
        b[cnt] = B.point(base);
        ++cnt;
        return 1;
      }
      // 有外公切线
      double ang = acos(rdiff / sqrt(d2));
      a[cnt] = A.point(base + ang);
      b[cnt] = B.point(base + ang);
      ++cnt;
      a[cnt] = A.point(base - ang);
      b[cnt] = B.point(base - ang);
      ++cnt;
      if (dcmp(d2 - rsum * rsum) == 0) {  // 一条内公切线
        a[cnt] = A.point(base);
        b[cnt] = B.point(PI + base);
        ++cnt;
      } else if (dcmp(d2 - rsum * rsum) > 0) {  // 两条内公切线
        double ang = acos(rsum / sqrt(d2));
        a[cnt] = A.point(base + ang);
        b[cnt] = B.point(PI + base + ang);
        ++cnt;
        a[cnt] = A.point(base - ang);
        b[cnt] = B.point(PI + base - ang);
        ++cnt;
      }
      return cnt;
    }
    
    // 点 O 在圆 A 外，求圆 A 的反演圆 B，R 是反演半径
    Circle Inversion_C2C(Point O, double R, Circle A) {
      double OA = Length(A.c - O);
      double RB = 0.5 * ((1 / (OA - A.r)) - (1 / (OA + A.r))) * R * R;
      double OB = OA * RB / A.r;
      double Bx = O.x + (A.c.x - O.x) * OB / OA;
      double By = O.y + (A.c.y - O.y) * OB / OA;
      return Circle(Point(Bx, By), RB);
    }
    
    // 直线反演为过 O 点的圆 B，R 是反演半径
    Circle Inversion_L2C(Point O, double R, Point A, Vector v) {
      Point P = GetLineProjection(O, A, A + v);
      double d = Length(O - P);
      double RB = R * R / (2 * d);
      Vector VB = (P - O) / d * RB;
      return Circle(O + VB, RB);
    }
    
    // 返回 true 如果 A B 两点在直线同侧
    bool theSameSideOfLine(Point A, Point B, Point S, Vector v) {
      return dcmp(Cross(A - S, v)) * dcmp(Cross(B - S, v)) > 0;
    }
    
    int main() {
      int T;
      scanf("%d", &T);
      while (T--) {
        Circle A, B;
        Point P;
        scanf("%lf%lf%lf", &A.c.x, &A.c.y, &A.r);
        scanf("%lf%lf%lf", &B.c.x, &B.c.y, &B.r);
        scanf("%lf%lf", &P.x, &P.y);
        Circle NA = Inversion_C2C(P, 10, A);
        Circle NB = Inversion_C2C(P, 10, B);
        Point LA[N], LB[N];
        Circle ansC[N];
        int q = getTangents(NA, NB, LA, LB), ans = 0;
        for (int i = 0; i < q; ++i)
          if (theSameSideOfLine(NA.c, NB.c, LA[i], LB[i] - LA[i])) {
            if (!theSameSideOfLine(P, NA.c, LA[i], LB[i] - LA[i])) continue;
            ansC[ans++] = Inversion_L2C(P, 10, LA[i], LB[i] - LA[i]);
          }
        printf("%d\n", ans);
        for (int i = 0; i < ans; ++i) {
          printf("%.8f %.8f %.8f\n", ansC[i].c.x, ansC[i].c.y, ansC[i].r);
        }
      }
    
      return 0;
    }
    ```

## Bài tập luyện tập

[「ICPC 2017 Nam Ninh Online」Finding the Radius for an Inserted Circle](https://vjudge.net/problem/%E8%AE%A1%E8%92%9C%E5%AE%A2-A1283)

[「CCPC 2017 Online」The Designer](https://acm.hdu.edu.cn/showproblem.php?pid=6158)

## Tài liệu tham khảo & Đọc thêm

-   [Inversive geometry - Wikipedia](https://en.wikipedia.org/wiki/Inversive_geometry)

-   [Phép nghịch đảo đường tròn - Blog ACdreamers](https://blog.csdn.net/acdreamers/article/details/16966369)
