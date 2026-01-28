author: Ir1d, TianyiQ

## Dẫn nhập

Thuật toán tăng dần ngẫu nhiên (Randomized Incremental Algorithm) là một kỹ thuật quan trọng trong hình học tính toán, không đòi hỏi quá nhiều lý thuyết, có độ phức tạp thấp và ứng dụng rộng rãi.

Tư tưởng của phương pháp tăng dần (Incremental Algorithm) tương tự như nguyên lý quy nạp toán học: bản chất là chuyển bài toán về một bài toán nhỏ hơn một cấp. Sau khi giải quyết bài toán con, thêm đối tượng hiện tại vào. Viết dưới dạng truy hồi:

$$
T(n)=T(n-1)+g(n)
$$

Hình thức của phương pháp tăng dần rất đơn giản, có thể áp dụng cho nhiều bài toán hình học.

Phương pháp tăng dần thường kết hợp với ngẫu nhiên hóa để tránh trường hợp xấu nhất.

## Bài toán bao tròn nhỏ nhất

### Mô tả bài toán

Cho $n$ điểm trên mặt phẳng, hãy tìm một đường tròn bán kính nhỏ nhất sao cho bao phủ toàn bộ các điểm đó.

### Quy trình

Giả sử đường tròn $O$ là đường tròn bao nhỏ nhất của $i-1$ điểm đầu tiên, khi thêm điểm thứ $i$, nếu điểm này nằm trong hoặc trên đường tròn thì không cần làm gì. Nếu không, đường tròn bao nhỏ nhất mới chắc chắn phải đi qua điểm thứ $i$.

Sau đó, lấy điểm thứ $i$ làm cơ sở (bán kính $0$), lặp lại quá trình trên với từng điểm $j$ trước đó, nếu điểm $j$ nằm ngoài đường tròn hiện tại thì đường tròn bao nhỏ nhất mới phải đi qua điểm $j$.

Tiếp tục lặp lại như vậy (vì tối đa chỉ cần 3 điểm để xác định một đường tròn bao nhỏ nhất).

Sau khi duyệt hết các điểm, đường tròn thu được chính là đường tròn bao nhỏ nhất của toàn bộ tập điểm.

### Tính chất

**Độ phức tạp thời gian**  $O(n)$, chứng minh chi tiết xem tài liệu tham khảo.

**Độ phức tạp không gian**  $O(n)$

### Cài đặt

??? note "Mã nguồn tham khảo"
    ```cpp
    #include <cmath>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <iostream>
    
    using namespace std;
    
    int n;
    double r;
    
    struct point {
      double x, y;
    } p[100005], o;
    
    double sqr(double x) { return x * x; }
    
    double dis(point a, point b) { return sqrt(sqr(a.x - b.x) + sqr(a.y - b.y)); }
    
    bool cmp(double a, double b) { return fabs(a - b) < 1e-8; }
    
    point geto(point a, point b, point c) {
      double a1, a2, b1, b2, c1, c2;
      point ans;
      a1 = 2 * (b.x - a.x), b1 = 2 * (b.y - a.y),
      c1 = sqr(b.x) - sqr(a.x) + sqr(b.y) - sqr(a.y);
      a2 = 2 * (c.x - a.x), b2 = 2 * (c.y - a.y),
      c2 = sqr(c.x) - sqr(a.x) + sqr(c.y) - sqr(a.y);
      if (cmp(a1, 0)) {
        ans.y = c1 / b1;
        ans.x = (c2 - ans.y * b2) / a2;
      } else if (cmp(b1, 0)) {
        ans.x = c1 / a1;
        ans.y = (c2 - ans.x * a2) / b2;
      } else {
        ans.x = (c2 * b1 - c1 * b2) / (a2 * b1 - a1 * b2);
        ans.y = (c2 * a1 - c1 * a2) / (b2 * a1 - b1 * a2);
      }
      return ans;
    }
    
    int main() {
      scanf("%d", &n);
      for (int i = 1; i <= n; i++) scanf("%lf%lf", &p[i].x, &p[i].y);
      for (int i = 1; i <= n; i++) swap(p[rand() % n + 1], p[rand() % n + 1]);
      o = p[1];
      for (int i = 1; i <= n; i++) {
        if (dis(o, p[i]) < r || cmp(dis(o, p[i]), r)) continue;
        o.x = (p[i].x + p[1].x) / 2;
        o.y = (p[i].y + p[1].y) / 2;
        r = dis(p[i], p[1]) / 2;
        for (int j = 2; j < i; j++) {
          if (dis(o, p[j]) < r || cmp(dis(o, p[j]), r)) continue;
          o.x = (p[i].x + p[j].x) / 2;
          o.y = (p[i].y + p[j].y) / 2;
          r = dis(p[i], p[j]) / 2;
          for (int k = 1; k < j; k++) {
            if (dis(o, p[k]) < r || cmp(dis(o, p[k]), r)) continue;
            o = geto(p[i], p[j], p[k]);
            r = dis(o, p[i]);
          }
        }
      }
      printf("%.10lf\n%.10lf %.10lf", r, o.x, o.y);
      return 0;
    }
    ```

## Bài tập luyện tập

[Bao tròn nhỏ nhất](https://www.luogu.com.cn/problem/P1742)

[「HNOI2012」Bắn cung](https://www.luogu.com.cn/problem/P3222)

[CodeForces 442E](https://codeforces.com/problemset/problem/442/E)

## Tài liệu tham khảo & Đọc thêm

[Thuật toán tăng dần ngẫu nhiên - Giải Dịch Luân](https://github.com/hzwer/shareOI/blob/master/%E8%AE%A1%E7%AE%97%E5%87%A0%E4%BD%95/%E9%9A%8F%E6%9C%BA%E5%A2%9E%E9%87%8F%E7%AE%97%E6%B3%95_%E8%A7%A3%E8%BD%B6%E4%BC%A6.pdf)

<https://www.cnblogs.com/aininot260/p/9635757.html>

<https://www.cise.ufl.edu/~sitharam/COURSES/CG/kreveldnbhd.pdf>

<https://blog.csdn.net/u014609452/article/details/62039612>
