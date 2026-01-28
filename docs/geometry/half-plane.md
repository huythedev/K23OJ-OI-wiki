author: wjy-yy, Ir1d, Xeonacid

## Định nghĩa

### Nửa mặt phẳng

Một nửa mặt phẳng được xác định bởi một đường thẳng và một phía của đường thẳng đó. Nửa mặt phẳng là một tập hợp điểm, tức là tập hợp các điểm nằm ở một phía của một đường thẳng. Nếu bao gồm cả đường thẳng, gọi là nửa mặt phẳng đóng; nếu không bao gồm, gọi là nửa mặt phẳng mở.

Phương trình tổng quát thường dùng là $Ax+By+C\ge 0$.

Trong hình học tính toán, thường biểu diễn bằng vectơ, toàn bộ bài toán sẽ thống nhất lấy phía bên trái hoặc bên phải của vectơ làm nửa mặt phẳng.

![Nửa mặt phẳng](./images/hpi1.svg)

### Giao nửa mặt phẳng

Giao nửa mặt phẳng là giao của nhiều nửa mặt phẳng. Vì nửa mặt phẳng là tập hợp điểm nên giao của chúng vẫn là tập hợp điểm, tạo thành một vùng trên mặt phẳng tọa độ.

Điều này rất giống với bài toán quy hoạch tuyến tính, giao nửa mặt phẳng chính là miền khả thi trong quy hoạch tuyến tính. Thông thường giao nửa mặt phẳng là một vùng hữu hạn, thường gặp trong các bài toán tính diện tích, v.v.

Có thể hiểu là giao của phía bên phải (hoặc bên trái) của mỗi vectơ trong tập vectơ, hoặc là tập nghiệm của hệ bất phương trình sau:

$$
\begin{cases}
A_1x+B_1y+C\ge 0\\
A_2x+B_2y+C\ge 0\\
\cdots
\end{cases}
$$

### Nhân của đa giác

Nếu một tập hợp điểm mà mọi điểm trong đó nối với bất kỳ điểm nào trên đa giác đều không cắt đa giác tại điểm nào khác, thì tập hợp đó gọi là nhân của đa giác.

Xem mỗi cạnh của đa giác là một vectơ nối tiếp nhau, thì giao nửa mặt phẳng phía trong của các vectơ này chính là nhân của đa giác.

## Giải thuật - Thuật toán S&I

### Sắp xếp theo góc cực

Trong C có hàm `atan2(double y,double x)`, trả về $\theta\in (-\pi,\pi]$, với $\theta =\arctan \frac{y}{x}$.

Trực tiếp lấy vectơ làm biến, gọi hàm này, dùng giá trị trả về làm khóa để sắp xếp, thu được tập cạnh (vectơ) mới đã sắp xếp.

Khi sắp xếp, nếu gặp các vectơ đồng phương (cùng hướng), thì giữ lại vectơ gần miền khả thi hơn. Ví dụ, hai vectơ có cùng góc cực, và ta lấy phía bên trái của vectơ làm nửa mặt phẳng, thì chỉ cần giữ lại vectơ phía bên trái. Cách kiểm tra là lấy một điểm đầu hoặc cuối của một vectơ so với vectơ còn lại, kiểm tra xem nằm bên trái hay bên phải.

### Duy trì hàng đợi đơn điệu

Vì giao nửa mặt phẳng là một đa giác lồi, nên cần duy trì một bao lồi. Khi thêm một cạnh mới, chỉ có thể ảnh hưởng đến cạnh đầu hoặc cuối của bao (vì bao lồi liên thông), nên chỉ cần loại bỏ phần tử ở đầu hoặc cuối hàng đợi, do đó dùng hàng đợi đơn điệu.

Ta duyệt qua các vectơ đã sắp xếp, đồng thời duy trì một mảng giao điểm. Khi hàng đợi có hơn 2 phần tử, giữa chúng sẽ sinh ra giao điểm.

Với vectơ hiện tại, nếu giao điểm trước đó nằm ở phía đối diện với nửa mặt phẳng của vectơ này, thì cạnh trước đó không còn ý nghĩa.

![Hàng đợi đơn điệu](./images/hpi2.svg)

Như hình trên, giả sử lấy phía bên trái của vectơ làm nửa mặt phẳng. Sau khi sắp xếp theo góc cực, thứ tự duyệt là $\vec a\to\vec b\to\vec c$. Khi $\vec a$ và $\vec b$ vào hàng đợi, trong mảng giao điểm sẽ sinh ra điểm $D$ (mảng giao điểm lưu giao điểm của hai vectơ liên tiếp trong hàng đợi).

Tiếp tục đến $\vec c$, phát hiện $D$ nằm bên phải $\vec c$. Vì **vectơ sinh ra $D$ có góc cực nhỏ hơn $\vec c$**, nên vectơ sinh ra $D$ (tức $\vec b$) không còn ảnh hưởng đến giao nửa mặt phẳng.

Cũng có trường hợp khi gần kết thúc, vectơ mới thêm vào lại ảnh hưởng từ đầu hàng đợi.

![Ảnh hưởng từ đầu hàng đợi](./images/hpi7.svg)

Vẫn giả sử lấy phía bên trái của vectơ làm nửa mặt phẳng. Khi thêm vectơ $\vec f$, giao điểm đầu tiên $G$ nằm bên phải $\vec f$, ta đảo ngược tiêu chí kiểm tra ở trên, lúc này cần loại bỏ vectơ $\vec a$, tức là **phần tử đầu hàng đợi**.

Cuối cùng, dùng vectơ đầu hàng đợi để loại bỏ các vectơ dư thừa ở cuối. Vì vectơ đầu hàng đợi sẽ bị các ràng buộc phía sau giới hạn, còn vectơ cuối thì không. Lúc này các cạnh tạo thành một vòng, nên vectơ đầu hàng đợi có thể giới hạn vectơ cuối.

### Lấy giao nửa mặt phẳng

Nếu giao nửa mặt phẳng là một đa giác lồi $n$ cạnh, cuối cùng trong mảng giao điểm sẽ có $n$ điểm. Nối chúng lại theo thứ tự sẽ được một đa giác $n$ cạnh cùng chiều (xuôi hoặc ngược kim đồng hồ).

Lúc này có thể dùng chia tam giác để tính diện tích (tính diện tích là dạng bài cơ bản nhất).

Đôi khi giao nửa mặt phẳng không tồn tại hoặc diện tích bằng 0, cần chú ý các trường hợp biên.

### Lưu ý

Khi xuất hiện một vectơ có thể loại toàn bộ các điểm trong hàng đợi (tức là tất cả các điểm trong hàng đợi đều nằm bên phải vectơ đó), **bắt buộc** phải xử lý phần tử cuối trước, rồi mới xử lý phần tử đầu. Do đó trong vòng lặp, phải duyệt `--r;` trước, rồi mới `++l;`, như vậy mới đúng. Lý do như sau.

![](./images/hpi4.svg)

Thông thường, khi thêm một cạnh (vectơ $\vec w$) vào hàng đợi (hàng đợi gồm $\left\{\vec{u},\vec{v}\right\}$), sẽ sinh ra giao điểm $N$, thu hẹp phạm vi phía sau $\vec{v}$.

![](./images/hpi5.svg)

Tuy nhiên, mỗi thao tác đều là trường hợp tổng quát, nên có thể xảy ra trường hợp điểm $M$ bị "đẩy ra ngoài".

![](./images/hpi6.svg)

Nếu lúc này xuất hiện vectơ $\vec a$ khiến $M$ nằm bên phải $\vec a$, thì $M$ phải bị loại khỏi hàng đợi. Nếu lúc này duyệt từ đầu hàng đợi `++l`, rõ ràng là đang mở rộng phạm vi. Thực tế, điểm $M$ được tạo bởi cả $\vec u$ và $\vec v$, nên cần xét xem bị ảnh hưởng bởi $\vec u$ hay $\vec v$. Vì sau khi sắp xếp theo góc cực, các vectơ theo thứ tự ngược chiều kim đồng hồ, nên ảnh hưởng của $\vec v$ lớn hơn.

Như hình trên, nếu $M$ xác định nằm bên phải $\vec a$, thì ảnh hưởng của $\vec v$ chắc chắn không còn đóng góp gì cho giao nửa mặt phẳng.

Lý do loại bỏ phần tử đầu là **ràng buộc của vectơ hiện tại lớn hơn vectơ đầu hàng đợi**, điều kiện này chỉ đúng khi hàng đợi có nhiều hơn hai đoạn (vectơ), nếu không sẽ xảy ra trường hợp như trên.

Vì vậy, nhất định phải loại bỏ phần tử cuối trước, rồi mới loại bỏ phần tử đầu.

???+ note "Code - Phần so sánh"
    ```cpp
    friend bool operator<(seg x, seg y) {
      db t1 = atan2((x.b - x.a).y, (x.b - x.a).x);
      db t2 = atan2((y.b - y.a).y, (y.b - y.a).x);  // 求极角
      if (fabs(t1 - t2) > eps)                      // 如果极角不等
        return t1 < t2;
      return (y.a - x.a) * (y.b - x.a) >
             eps;  // 判断向量x在y的哪边，令最靠左的排在最左边
    }
    ```

???+ note "Code - Phần thêm vào"
    ```cpp
    // pnt its(seg a,seg b)表示求线段a,b的交点
    // s[]是极角排序后的向量
    // q[]是向量队列
    // t[i]是s[i-1]与s[i]的交点
    // 【码风】队列的范围是(l,r]
    // 求的是向量左侧的半平面
    int l = 0, r = 0;
    for (int i = 1; i <= n; ++i)
      if (s[i] != s[i - 1]) {
        // 注意要先检查队尾
        while (r - l > 1 && (s[i].b - t[r]) * (s[i].a - t[r]) >
                                eps)  // 如果上一个交点在向量右侧则弹出队尾
          --r;
        while (r - l > 1 && (s[i].b - t[l + 2]) * (s[i].a - t[l + 2]) >
                                eps)  // 如果第一个交点在向量右侧则弹出队首
          ++l;
        q[++r] = s[i];
        if (r - l > 1) t[r] = its(q[r], q[r - 1]);  // 求新交点
      }
    while (r - l > 1 &&
           (q[l + 1].b - t[r]) * (q[l + 1].a - t[r]) > eps)  // 注意删除多余元素
      --r;
    t[r + 1] = its(q[l + 1], q[r]);  // 再求出新的交点
    ++r;
    // 这里不能在t里面++r需要注意一下……
    ```

## Bài tập luyện tập

[POJ 2451 Uyuw's Concert](http://poj.org/problem?id=2451) (chú ý trường hợp biên)

[POJ 1279 Art Gallery](http://poj.org/problem?id=1279) (tìm nhân của đa giác)

[「CQOI2006」Đa giác lồi](https://www.luogu.com.cn/problem/P4196)
