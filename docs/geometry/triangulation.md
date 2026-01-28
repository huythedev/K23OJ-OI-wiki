author: xehoth

Trong hình học, **tam giác hóa** (triangulation) là việc chia nhỏ một đối tượng hình học thành các tam giác, và mở rộng lên các chiều cao hơn là chia thành các simplex (đơn hình). Với một tập điểm cho trước, có rất nhiều cách tam giác hóa, ví dụ:

![Ba kiểu tam giác hóa](./images/triangulation-0.svg)

Trong lập trình thi đấu (OI), tam giác hóa thường chỉ tam giác hóa hoàn hảo trong hình học hai chiều, tức là **tam giác hóa Delaunay** (Delaunay Triangulation, viết tắt DT).

## Tam giác hóa Delaunay

### Định nghĩa

Trong toán học và hình học tính toán, với một tập điểm rời rạc $P$ trên mặt phẳng, tam giác hóa Delaunay DT($P$) thỏa mãn:

1.  **Tính không chứa điểm trong đường tròn ngoại tiếp**: DT($P$) là **duy nhất** (nếu không có bốn điểm nào đồng viên), và trong DT($P$), **bất kỳ** tam giác nào cũng có đường tròn ngoại tiếp không chứa điểm nào khác của $P$ bên trong.
2.  **Cực đại hóa góc nhỏ nhất**: Trong tất cả các tam giác hóa có thể của $P$, DT($P$) là tam giác hóa mà tam giác có góc nhỏ nhất là lớn nhất. Nói cách khác, DT($P$) là tam giác hóa "gần đều" nhất. Cụ thể, với hai tam giác kề nhau tạo thành một tứ giác lồi, nếu đổi đường chéo, góc nhỏ nhất trong sáu góc của hai tam giác sẽ không tăng lên.

![Một tam giác hóa Delaunay với các đường tròn ngoại tiếp](./images/triangulation-1.png)

### Tính chất

1.  **Gần nhất**: Ba điểm gần nhau nhất sẽ tạo thành một tam giác, các cạnh (cạnh tam giác) không cắt nhau.
2.  **Duy nhất**: Bất kể bắt đầu xây dựng từ đâu, kết quả cuối cùng là như nhau (nếu không có bốn điểm đồng viên).
3.  **Tối ưu**: Với hai tam giác kề nhau tạo thành tứ giác lồi, nếu có thể đổi đường chéo, thì sáu góc trong hai tam giác, góc nhỏ nhất không thay đổi.
4.  **Quy tắc nhất**: Nếu sắp xếp các góc nhỏ nhất của các tam giác theo thứ tự tăng dần, DT sẽ cho dãy số lớn nhất.
5.  **Cục bộ**: Thêm, xóa, di chuyển một đỉnh chỉ ảnh hưởng đến các tam giác lân cận.
6.  **Bao ngoài là đa giác lồi**: Biên ngoài của tam giác hóa tạo thành một đa giác lồi.

## Thuật toán chia để trị xây dựng DT

Có nhiều thuật toán xây dựng DT, trong đó thuật toán chia để trị có độ phức tạp $O(n \log n)$ là dễ hiểu và dễ cài đặt nhất.

Bước đầu tiên là sắp xếp tập điểm theo hoành độ $x$ **tăng dần**. Hình dưới là một tập 10 điểm đã được sắp xếp.

![Tập 10 điểm đã sắp xếp](./images/triangulation-2.svg)

Sau khi sắp xếp, ta liên tục chia tập điểm thành hai phần (chia để trị), cho đến khi mỗi tập con chỉ còn tối đa 3 điểm. Khi đó, mỗi tập con có thể tam giác hóa ngay thành một tam giác hoặc một đoạn thẳng.

![Chia thành các tập con 2 hoặc 3 điểm](./images/triangulation-3.svg)

Khi quay lại (hợp), các tập con đã tam giác hóa sẽ được hợp lại. Tam giác hóa sau khi hợp gồm các cạnh LL-edge (cạnh bên trái), RR-edge (cạnh bên phải), LR-edge (cạnh nối giữa hai phần), như hình: LL-edge (xám), RR-edge (đỏ), LR-edge (xanh). Khi hợp, để giữ tính chất DT, **có thể** phải xóa một số LL-edge và RR-edge, nhưng **không bao giờ** thêm mới LL-edge hoặc RR-edge.

![edge](./images/triangulation-4.svg)

Bước đầu tiên khi hợp hai tam giác hóa là thêm base LR-edge, tức là cạnh LR-edge **thấp nhất** không cắt bất kỳ LL-edge hay RR-edge nào.

![Hợp hai tam giác hóa](./images/triangulation-5.svg)

Tiếp theo, xác định LR-edge tiếp theo **ngay phía trên** base LR-edge. Ví dụ, với tập bên phải, các điểm có thể là đầu mút phải của LR-edge tiếp theo là các điểm nối với đầu mút phải của base LR-edge qua RR-edge ($6, 7, 9$), đầu mút trái là điểm $2$.

![LR-edge tiếp theo](./images/triangulation-6.svg)

Với mỗi điểm có thể, kiểm tra hai điều kiện:

1.  RR-edge tương ứng và base LR-edge tạo góc nhỏ hơn $180^\circ$.
2.  Đường tròn ngoại tiếp qua hai đầu mút của base LR-edge và điểm này không chứa điểm khả dĩ nào khác.

![Kiểm tra điểm khả dĩ](./images/triangulation-7.svg)

Như hình, điểm $6$ (vòng tròn xanh) chứa điểm $9$, còn điểm $7$ (vòng tròn tím) không chứa điểm nào khác, nên $7$ là đầu mút phải của LR-edge tiếp theo.

Với tập bên trái, làm tương tự (lấy đối xứng).

![Kiểm tra điểm bên trái](./images/triangulation-8.svg)

Khi cả hai bên không còn điểm khả dĩ, việc hợp kết thúc. Nếu có điểm khả dĩ, thêm LR-edge, đồng thời xóa các LL-edge hoặc RR-edge bị cắt bởi LR-edge mới.

Nếu cả hai bên đều có điểm khả dĩ, kiểm tra xem đường tròn ngoại tiếp qua ba điểm bên trái có chứa điểm bên phải không (và ngược lại). Thường chỉ có một điểm thỏa mãn (trừ trường hợp bốn điểm đồng viên).

![LR-edge tiếp theo](./images/triangulation-9.svg)

Sau khi thêm LR-edge này, lặp lại quá trình trên với LR-edge mới làm base, cho đến khi hợp xong.

![Hợp](./images/triangulation-10.svg)

## Mã nguồn

??? note "Cài đặt"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/geometry/triangulation.md
    ```

## Đồ thị Voronoi

Đồ thị Voronoi là tập hợp các đa giác liên tục được tạo bởi các đường trung trực của các đoạn nối hai điểm gần nhau nhất. Với $n$ điểm (hạt giống) trên mặt phẳng, đồ thị Voronoi chia mặt phẳng thành $n$ vùng, mỗi vùng gồm các điểm gần một hạt giống hơn bất kỳ hạt giống nào khác.

Đồ thị Voronoi là đồ thị đối ngẫu của tam giác hóa Delaunay, có thể xây dựng bằng cách chia để trị tam giác hóa Delaunay, sau đó dùng thuật toán "left-turn" để dựng đồ thị đối ngẫu, với độ phức tạp $O(n \log n)$.

## Bài tập

[SGU 383 Caravans](https://codeforces.com/problemsets/acmsguru/problem/99999/383) Tam giác hóa + Nhảy bậc (doubling)

[ContestHunter. Vòng lặp vô tận](http://noi-test.zzstep.com/contest/Beta%20Round%20%EF%BC%832%20%28%E6%96%B0%E7%96%86%E7%9C%81%E9%98%9F%E4%BA%92%E6%B5%8BWeek1-Day2%29/%E6%97%A0%E5%B0%BD%E7%9A%84%E6%AF%81%E7%81%AD) Tam giác hóa xây đồ thị đối ngẫu Voronoi

[Codeforces Gym 103485M. Constellation collection](https://codeforces.com/gym/103485/problem/M) Tam giác hóa rồi xây đồ thị để floodfill

## Tài liệu tham khảo & Đọc thêm

1.  [Wikipedia - Triangulation (geometry)](https://en.wikipedia.org/wiki/Triangulation_%28geometry%29)
2.  [Wikipedia - Delaunay triangulation](https://en.wikipedia.org/wiki/Delaunay_triangulation)
3.  Samuel Peterson -[Computing Constrained Delaunay Triangulations in 2-D (1997-98)](http://www.geom.uiuc.edu/~samuelp/del_project.html)
