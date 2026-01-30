tác giả: ChungZH, billchenchina, Chrogeek, Early0v0, ethan-enhe, HeRaNO, hsfzLZH1, iamtwz, Ir1d, konnyakuxzy, luoguojie, Marcythm, orzAtalod, StudyingFather, wy-luke, Xeonacid, CCXXXI, chenryang, chenzheAya, CJSoft, cjsoft, countercurrent-time, DawnMagnet, Enter-tainer, GavinZhengOI, Haohu Shen, Henry-ZHR, hjsjhn, hly1204, jaxvanyang, Jebearssica, kenlig, ksyx, megakite, Menci, moon-dim, NachtgeistW, onelittlechildawa, ouuan, shadowice1984, shawlleyw, shuzhouliu, SukkaW, Tiphereth-A, x2e6, Ycrpro, yifan0305, zeningc, hcx2012Git

## Giới thiệu

Như chúng ta đã biết, cây đoạn (segment tree) có thể hỗ trợ truy vấn nhanh thông tin tổng hợp trên một đoạn, ví dụ như tổng đoạn con lớn nhất, tổng đoạn, tích ma trận liên tiếp trên một đoạn, v.v.

Nhưng có một vấn đề là các truy vấn đoạn của cây đoạn thông thường có thể vẫn còn hơi chậm đối với một số người dùng khó tính.

Nói một cách đơn giản, việc xây dựng cây đoạn cần $O(n)$ phép toán hợp nhất, và mỗi lần truy vấn đoạn cần $O(\log{n})$ phép toán hợp nhất. Khi truy vấn các thứ như tổng đoạn, chúng ta có thể chấp nhận được, nhưng khi chúng ta cần truy vấn các thông tin có độ phức tạp hợp nhất cao như cơ sở tuyến tính đoạn, độ phức tạp là $O(\log^2{w})$, thì ngay cả $O(\log{n})$ lần hợp nhất đôi khi cũng không thể chấp nhận được về mặt thời gian.

Và cái gọi là "cây mèo" (cat tree) là một loại cây đoạn tĩnh, không hỗ trợ sửa đổi, chỉ hỗ trợ truy vấn đoạn nhanh.

Việc xây dựng một cây đoạn tĩnh như vậy cần $O(n\log{n})$ phép toán hợp nhất, nhưng độ phức tạp truy vấn lúc này được tăng tốc lên $O(1)$ phép toán hợp nhất.

Trong việc xử lý các thông tin đặc biệt như cơ sở tuyến tính, độ phức tạp thậm chí có thể giảm xuống $O(n\log^2{w})$.

## Nguyên lý

Khi truy vấn thông tin tổng hợp trên đoạn $[l,r]$, chúng ta tìm LCA (Lowest Common Ancestor - Tổ tiên chung gần nhất) trên cây đoạn của nút đại diện cho đoạn $[l,l]$ và nút đại diện cho đoạn $[r,r]$. Gọi nút này là $p$, đoạn mà nó đại diện là $[L,R]$. Chúng ta sẽ nhận thấy một số tính chất rất thú vị:

1.  Đoạn $[L,R]$ chắc chắn chứa đoạn $[l,r]$. Rõ ràng, vì nó là tổ tiên của cả $l$ và $r$.

2.  Đoạn $[l,r]$ chắc chắn cắt qua điểm giữa của $[L,R]$. Do $p$ là LCA của $l$ và $r$, điều này có nghĩa là con trái của $p$ là tổ tiên của $l$ nhưng không phải là tổ tiên của $r$, và con phải của $p$ là tổ tiên của $r$ nhưng không phải là tổ tiên của $l$. Do đó, $l$ chắc chắn nằm trong đoạn $[L,\mathit{mid}]$, và $r$ chắc chắn nằm trong đoạn $(\mathit{mid},R]$.

Với hai tính chất này, chúng ta có thể giảm độ phức tạp truy vấn xuống $O(1)$ phép toán hợp nhất.

## Triển khai

Cụ thể, khi xây dựng cây, đối với một nút trên cây đoạn, giả sử nó đại diện cho đoạn $(l,r]$.

Khác với cây đoạn truyền thống chỉ giữ tổng của $[l,r]$ trong nút này, chúng ta còn lưu thêm mảng tổng hậu tố của $(l,\mathit{mid}]$ và mảng tổng tiền tố của $(\mathit{mid},r]$.

Khi đó, độ phức tạp xây dựng cây là $T(n)=2T(n/2)+O(n)=O(n\log{n})$, tương tự, độ phức tạp không gian cũng từ $O(n)$ ban đầu trở thành $O(n\log{n})$.

Sau đây là phần truy vấn quan trọng nhất.

Nếu chúng ta truy vấn đoạn $[l,r]$, chúng ta tìm LCA của nút đại diện cho $[l,l]$ và nút đại diện cho $[r,r]$, gọi là $p$.

Theo hai tính chất vừa nêu, $l,r$ nằm trong đoạn mà $p$ bao gồm và chắc chắn cắt qua điểm giữa của $p$.

Điều này ngụ ý một sự thật rất quan trọng là chúng ta có thể sử dụng mảng tổng tiền tố và mảng tổng hậu tố bên trong $p$ để phân tách $[l,r]$ thành $[l,\mathit{mid}]+(\mathit{mid},r]$ từ đó ghép lại thành đoạn $[l,r]$.

Và quá trình này chỉ cần $O(1)$ phép toán hợp nhất!

Tuy nhiên, chúng ta có vẻ đã bỏ qua điều gì đó?

Dường như độ phức tạp tìm LCA vẫn chưa phải là $O(1)$. Tìm kiếm vét cạn là $O(\log{n})$, phương pháp tăng đôi (binary lifting) là $O(\log{\log{n}})$, và việc chuyển đổi sang bảng ST (Sparse Table) thì chi phí quá lớn...

## Xây dựng kiểu đống (Heap-style construction)

Cụ thể, chúng ta bổ sung chuỗi cho đến khi có độ dài là lũy thừa của $2$, sau đó xây dựng cây đoạn.

Lúc này, chúng ta nhận thấy LCA của hai nút trên cây đoạn chính là tiền tố chung dài nhất (LCP) của chỉ số nhị phân của hai nút.

Chỉ cần suy nghĩ một chút là có thể thấy rằng trong biểu diễn nhị phân của $x$ và $y$, $\text{lcp}(x,y)=x>>\text{digits}[x\oplus y]$ (trong đó $\text{digits}[x]$ biểu thị số bit trong biểu diễn nhị phân của $x$, tức là $\lfloor \log_2 x \rfloor+1$).

Vì vậy, chúng ta chỉ cần tiền xử lý một mảng $\text{digits}$ là có thể dễ dàng hoàn thành công việc tìm LCA.

Như vậy chúng ta đã xây dựng được một cây mèo.

Vì việc xây dựng cây liên quan đến việc tính tổng tiền tố và tổng hậu tố, nên đối với các thông tin như cơ sở tuyến tính mà việc hợp nhất là $O(\log^2{w})$ nhưng tính tổng tiền tố là $O(n\log{n})$, việc sử dụng cây mèo có thể tối ưu hóa cơ sở tuyến tính đoạn tĩnh từ $O(n\log^2{w}+m\log^2{w}\log{n})$ xuống $O(n\log{n}\log{w}+m\log^2{w})$.

### Tham khảo

-   [Blog của immortalCO](https://immortalco.blog.uoj.ac/blog/2102)
-   [\[Kle77\]](http://ieeexplore.ieee.org/document/1675628/) V. Klee, "Can the Measure of be Computed in Less than O (n log n) Steps?," Am. Math. Mon., vol. 84, no. 4, pp. 284–285, Apr. 1977.
-   [\[BeW80\]](https://www.tandfonline.com/doi/full/10.1080/00029890.1977.11994336) Bentley and Wood, "An Optimal Worst Case Algorithm for Reporting Intersections of Rectangles," IEEE Trans. Comput., vol. C–29, no. 7, pp. 571–577, Jul. 1980.