author: Jerry3128

Kiến thức tiền đề: [Cây phân đoạn (Segment Tree)](./seg.md)

## Giới thiệu vấn đề

Cho một dãy các hàm tuyến tính một biến $F=\{f_1,\dots,f_n\}$: $f_i: \mathbf{R} \rightarrow \mathbf{R}$. Trong đó $f_i(x)=k_ix+b_i$ với $k_i,b_i \in \mathbf{R}$. Chúng ta cần bảo trì các thao tác sau:

-   $\operatorname{QueryMax}(l,r)$: Cho $l$ và $r$, trả về $\max_{i=l}^r{f_i(0)}$.
-   $\operatorname{TranslateLeft}(l,r,\delta)$: Cho $l$, $r$ và $\delta$, với mọi $i\in[l,r]$, thực hiện phép gán $f_i(x) \leftarrow f_i(x+\delta)$, thao tác này tương đương với thực hiện $b_i\leftarrow b_i+k_i\delta$. Trong đó $\delta > 0$.

Để thuận tiện, chúng ta giả sử tất cả các hàm đều khác nhau đôi một.

Bản chất của việc tịnh tiến (dịch chuyển) khoảng hàm bậc nhất sang trái là $b_i \leftarrow k_i\cdot \delta$, tức là đối với hệ số tự do $b_i$, ta cộng thêm hệ số góc $k_i$ nhân với lượng tịnh tiến của hoành độ $\delta$; thao tác này tương đương với bài toán "cộng giá trị trên đoạn theo trọng số vị trí" mà ta thường gặp trong nhiều cấu trúc dữ liệu: tức là với mỗi chỉ số $i$ trong đoạn $[l, r]$, cộng vào giá trị của nó một lượng cố định $\delta$ nhân với hệ số riêng biệt $k_i$ tại vị trí đó. Do đó, về bản chất, tịnh tiến khoảng hàm bậc nhất chính là phép cộng đoạn có trọng số theo vị trí.

Để trình bày cấu trúc chia để trị hình cây nhị phân độc đáo của KTT, chúng ta sẽ bắt đầu trực tiếp từ thao tác tịnh tiến khoảng.

## Cấu trúc dữ liệu động học (Kinetic Data Structures)

Kinetic Data Structures, viết tắt là KDS. KDS được sử dụng để duy trì các thuộc tính của một hệ thống đối tượng hình học trong quá trình chuyển động liên tục.

### Hàng đợi sự kiện (Event Queue)

Chúng ta giả sử mỗi điểm đều có một kế hoạch chuyển động đã biết, kế hoạch này cung cấp thông tin chuyển động toàn bộ hoặc một phần của nó. Ví dụ: đường cong hoặc đường thẳng được tạo bởi hàm $f_i(x)$ có thể mô tả rất tốt quỹ đạo chuyển động của điểm động $i$. Kế hoạch chuyển động có thể thay đổi bất cứ lúc nào, có thể do va chạm hoặc tương tác với môi trường; chúng ta gọi nguyên nhân gây ra sự thay đổi kế hoạch chuyển động là sự kiện (event). Hàng đợi sự kiện sẽ đưa ra các sự kiện theo trình tự thời gian.

Một khía cạnh quan trọng của KDS là cần phải có các sự kiện dễ bảo trì, nghĩa là các loại sự kiện trong hàng đợi sự kiện tương ứng với những thay đổi tổ hợp có thể xảy ra, những thay đổi này liên quan đến một số lượng đối tượng hằng số và thường là rất nhỏ. Ví dụ, trong bài toán này, một loại sự kiện chúng ta sử dụng là "quan hệ độ lớn giữa hàm $f_i(0)$ và hàm $f_{j}(0)$ thay đổi".

Hàng đợi sự kiện có thể được bảo trì ngầm định.

### Chứng nhận (Certificates)

Các sự kiện này phải tương đương với việc được đảm bảo thông qua sự giao nhau của một loạt các điều kiện đại số bậc thấp, mỗi điều kiện đại số liên quan đến một số lượng hữu hạn các đối tượng. Chúng ta gọi các điều kiện này là chứng nhận (certificates) của KDS. Ví dụ $[f_i(0) > f_j(0)]$.

## Kinetic Tournament Tree

### Giới thiệu

Kinetic Tournament Tree (gọi tắt là KTT), thuộc nhóm Cấu trúc dữ liệu động học (Kinetic Data Structures), xuất hiện lần đầu vào năm 1999 trong bài báo [Data Structures for Mobile Data](https://www.sciencedirect.com/science/article/pii/S0196677498909889), dùng để bảo trì dữ liệu biến đổi liên tục. Tổng quát hơn, bất kỳ cấu trúc nào áp dụng chiến lược động học hóa (kinetization strategy) sau đây đều có thể được gọi là Kinetic Tournament:

-   Tạo ra các chứng nhận tính đúng đắn cho các thao tác quan trọng trong thuật toán tĩnh (ví dụ: so sánh), và liên kết mỗi chứng nhận với một hàng đợi sự kiện toàn cục, ghi lại thời điểm mà chứng nhận đó có thể bị vô hiệu.
-   Khi một chứng nhận bị vô hiệu, chúng ta có thể cập nhật kết quả đầu ra của thuật toán và bảo trì tập hợp chứng nhận một cách hiệu quả.

Trong cộng đồng lập trình thi đấu, nó bắt đầu nổi lên từ bài luận văn của Đội tuyển quốc gia Trung Quốc năm 2020: "[Bàn về việc bảo trì động giá trị cực trị của hàm số](https://github.com/OI-wiki/libs/blob/master/%E9%9B%86%E8%AE%AD%E9%98%9F%E5%8E%86%E5%B9%B4%E8%AE%BA%E6%96%87/IOI2020%E4%B8%AD%E5%9B%BD%E5%9B%BD%E5%AE%B6%E5%80%99%E9%80%89%E9%98%9F%E8%AE%BA%E6%96%87%E9%9B%86%20%E9%9D%9E%E6%AD%A3%E5%BC%8F%E7%89%88.pdf)". KTT trong giới học thuật và KTT trong giới lập trình thi đấu có sự khác biệt về lĩnh vực ứng dụng và cách cài đặt, do đó chúng ta sẽ giới thiệu KTT đã được tối ưu hóa cho lập trình thi đấu.

### Cấu trúc cơ bản

Đầu tiên, chúng ta xem xét việc thiết kế một cấu trúc dữ liệu tương tự như Segment Tree để bảo trì giá trị lớn nhất tĩnh. Chúng ta xây dựng cấu trúc của Segment Tree, với mỗi nút không phải lá, giá trị của nó là giá trị lớn hơn trong hai nút con. Sau khi thực hiện $O(n)$ phép so sánh, chúng ta nhận được giá trị của nút gốc chính là giá trị lớn nhất toàn cục. Bây giờ, các giá trị bắt đầu thay đổi. Miễn là KTT có thể phát hiện được mỗi khi nguồn gốc của giá trị lớn nhất tại các nút trên cây thay đổi, chúng ta sẽ có thể bảo trì giá trị lớn nhất toàn cục.

Để KTT có thể phát hiện mỗi lần thay đổi nguồn gốc giá trị lớn nhất trên cây, đối với nút $x$ trên cây cùng các hàm $f_L$ và $f_R$ được cung cấp bởi con trái và con phải của nó, chúng ta định nghĩa chứng nhận là "quan hệ độ lớn giữa $f_L$ và $f_R$ không thay đổi". Khi chứng nhận bị vô hiệu, chúng ta cần đi theo đường đi trên cây đến nút có chứng nhận bị vô hiệu để cập nhật thông tin của nó. Để bảo trì thời gian hết hiệu lực của mỗi chứng nhận, chúng ta nhận thấy thời điểm chứng nhận vô hiệu chính là thời điểm hai hàm có cùng giá trị, bài toán trở thành tìm hoành độ giao điểm của hai hàm tuyến tính, có thể giải quyết trong thời gian $O(1)$.

Đối với mỗi nút trên cây, ta bảo trì hàm số đạt giá trị lớn nhất tại $0$, thời gian chứng nhận hiện tại bị vô hiệu, và thời gian vô hiệu sớm nhất của chứng nhận trong toàn bộ cây con; như vậy đối với thời điểm chứng nhận của mỗi nút bị vô hiệu, chúng ta có thể tìm thấy nó vào thời điểm đó và cập nhật thông tin. Những thông tin này dùng để ghi lại thông tin của chính hàm số. Tiếp theo, chúng ta xem xét việc bảo trì thao tác tịnh tiến khoảng. Vì nó có thể cộng dồn đơn giản, chúng ta có thể sử dụng lazy tag (nhãn lười) để xử lý thao tác tịnh tiến khoảng.

Chúng ta định nghĩa lazy tag $\Delta_v$ biểu thị rằng tất cả các hàm trên các nút khác trong cây con của nút $v$ đều cần được tịnh tiến sang trái $\Delta_v$ đơn vị. Lúc này, đối với nút $v$ trên cây, một thao tác mới cần tịnh tiến tất cả các hàm trong cây con của nó sang trái một lượng $\delta$, tức là $f(x)\leftarrow f(x+\delta)$. Chúng ta cần cập nhật lazy tag: $\Delta_v\leftarrow \Delta_v + \delta$, tức là cộng dồn lượng dịch chuyển cho tất cả các nút khác trong cây con. Đồng thời, việc tịnh tiến sang trái cũng có nghĩa là giá trị hàm tại điểm $0$ thay đổi. Chúng ta có thể phát hiện rằng nếu hoành độ vô hiệu của chứng nhận là $t$, thì hoành độ vô hiệu sau khi tịnh tiến sẽ là $t-\delta$; lúc này, nếu $t-\delta$ vượt qua điểm $0$ (chuyển từ dương sang âm hoặc ngược lại tùy ngữ cảnh, ở đây ý nói thời điểm sự kiện xảy ra đã trôi qua so với mốc hiện tại), điều đó có nghĩa là chứng nhận đã vô hiệu, chúng ta cần đệ quy xuống dưới để tìm nút chứa chứng nhận hiện tại, cập nhật nút đó và cập nhật thông tin mới lên gốc. Quá trình này có thể được thực hiện cùng với thao tác sửa đổi.

Do đó, chúng ta có thể có được cài đặt đơn giản sau.

???+ example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/ds/code/ktt/ktt_1.cpp:core"
    ```

### Phân tích độ phức tạp

Việc chứng minh độ phức tạp thời gian của KTT cần sử dụng phương pháp phân tích thế năng (Potential Analysis).

Gọi $d(x)$ là độ sâu của nút $x$ trên Segment Tree, độ sâu của nút gốc là $1$. Định nghĩa thế năng của nút $x$ trên Segment Tree là:

$$
\alpha(x) = \begin{cases}
d(x) & \text{nếu hàm có hệ số góc nhỏ hơn có giá trị lớn hơn}  \\
0    & \text{ngược lại}\\
\end{cases}
$$

Tức là tại $x$, khi so sánh hai hàm, nếu hàm có hệ số góc nhỏ hơn lại có giá trị tại điểm $0$ lớn hơn, thì thế năng của nút hiện tại là $d(x)$, ngược lại là $0$.

Định nghĩa thế năng của toàn bộ KTT là tổng thế năng của tất cả các nút:

$$
\Phi = \sum_x \alpha(x)
$$

Xét một lần cập nhật thực tế đối với nút $x$ và cha của nó là $p$ với chi phí $c=1$, thế năng trước và sau khi cập nhật lần lượt là $\Phi$ và $\Phi'$. Chúng ta đi tính chi phí cập nhật khấu hao (amortized update cost) của nút $x$. Do nút $x$ hiện tại được cập nhật (tức là có sự thay đổi về hàm lớn nhất, dẫn đến hàm có hệ số góc lớn hơn sẽ chiếm ưu thế hoặc ngược lại để thỏa mãn điều kiện giao nhau), thế năng của nó chắc chắn giảm từ $d(x)$ xuống $0$. Còn đối với $p$, thế năng của nó trong trường hợp xấu nhất có thể tăng từ $0$ lên $d(p)$:

$$
\begin{aligned}
\hat{c} &= 1 + \Phi' - \Phi\\
    &= 1 + (\alpha'(p) + \alpha'(x)) - (\alpha(p) + \alpha(x))\\
    &= 1 + (\alpha'(p) - \alpha(p)) + (\alpha'(x) - \alpha(x))\\
    &\leq 1 + d(p) - d(x)\\
    &= 0
\end{aligned}
$$

Tính tổng chi phí thực tế, định nghĩa thế năng ban đầu $\Phi_s$ và thế năng cuối cùng $\Phi_t$:

$$
\begin{aligned}
\sum c  &= \sum \hat{c} + \Phi_{s} - \Phi_{t}\\
    &\leq \Phi_{s} - \Phi_{t}\\
    &=O(n\log n)
\end{aligned}
$$

Phần này là số lần cập nhật chứng nhận vô hiệu của KTT trong trường hợp chỉ có sửa đổi toàn cục.

Ngoài ra, hãy xem xét ảnh hưởng của việc tịnh tiến khoảng đến thế năng. Đối với một lần tịnh tiến khoảng, các nút chúng ta cần xem xét là những nút mà trong cây con của nó có tồn tại nhưng không phải tất cả các nút đều thực hiện thao tác tịnh tiến. Những nút như vậy chính là các nút chúng ta đi qua trên cây khi thực hiện thao tác sửa đổi, số lượng của chúng không quá $O(\log n)$, trong trường hợp xấu nhất thế năng của mỗi nút tăng $d(x)\le \log n$, do đó mỗi thao tác làm tăng thế năng thêm $O(\log^2 n)$.

Để bảo trì tịnh tiến khoảng, thao tác cập nhật chứng nhận sẽ được thực hiện $O(n\log n + m\log^2 n)$ lần. Mỗi lần cập nhật chứng nhận, chúng ta cần đi dọc theo đường đi trên cây đến nút có chứng nhận bị vô hiệu, phần này mất $O(\log n)$. Do đó tổng độ phức tạp thời gian là $O(n\log^2 n+ m\log^3 n)$.

Điểm xuất sắc của phương pháp này là nó đã chạm đến cận dưới của độ phức tạp thời gian của vấn đề $O(\lambda_{s}(n)\log^2 n)$. $\lambda_{s}(n)$ biểu thị độ dài lớn nhất của dãy Davenport-Schinzel cấp $(n, s)$. Trong đó hàm tuyến tính tương ứng với $s=1$ và $\lambda_1(n)=n$. Phần này thuộc nội dung hình học tính toán, bài viết này sẽ không đi sâu vào chi tiết.

### Trường hợp bậc cao

Nếu chúng ta bảo trì không phải hàm tuyến tính mà là hàm đa thức hoặc các hàm phức tạp hơn thì xử lý như thế nào. Giữa hai hàm phức tạp có thể có nhiều giao điểm. Cho một dãy hàm đơn biến liên tục, được định nghĩa đầy đủ $F=\{f_1,\dots,f_n\}$: $f_i: \mathbf{R} \rightarrow \mathbf{R}$. Trong đó đồ thị của mỗi cặp hàm cắt nhau tại tối đa $s$ điểm. Điển hình, tập hợp các hàm đa thức bậc $s$ thỏa mãn yêu cầu này.

Đối với vấn đề tương tự, chúng ta sử dụng phân tích thế năng.

$d(x)$ là độ sâu của nút $x$ trên Segment Tree, độ sâu của nút gốc là $1$. Định nghĩa $I(x)$ biểu thị số giao điểm nằm sau điểm $0$ của hai hàm được so sánh tại nút $x$. Định nghĩa thế năng của nút $x$ trên Segment Tree là:

$$
\alpha(x)=d(x)^{\log_2(s+1)}I(x)
$$

Định nghĩa thế năng của toàn bộ KTT là tổng thế năng của tất cả các nút:

$$
\Phi = \sum_x \alpha(x)
$$

Xét một lần cập nhật thực tế đối với nút $x$ và cha của nó là $p$ với chi phí $c=1$, thế năng trước và sau khi cập nhật lần lượt là $\Phi$ và $\Phi'$. Chúng ta đi tính chi phí cập nhật khấu hao của nút $x$. Do nút $x$ hiện tại được cập nhật, thế năng của nó chắc chắn giảm từ $d(x)^{\log_2(s+1)}I(x)$ xuống $d(x)^{\log_2(s+1)}(I(x)-1)$ (do một giao điểm đã bị vượt qua). Còn đối với $p$, thế năng của nó trong trường hợp xấu nhất có thể tăng từ $0$ lên $d(p)^{\log_2(s+1)}$:

$$
\begin{aligned}
        \hat{c} &= 1 + \Phi' - \Phi\\
                &= 1 + (\alpha'(x) - \alpha(x)) + (\alpha'(p) - \alpha(p))\\
                &\leq 1 - d(x)^{\log_2{(s+1)}} + s(d(x)-1)^{\log_2{(s+1)}}\\
                &\leq 0
    \end{aligned}
$$

Từ dòng thứ ba xuống dòng thứ tư chúng ta đã sử dụng ràng buộc $d(x)$ là số nguyên dương.

Tính tổng chi phí thực tế, định nghĩa thế năng ban đầu $\Phi_s$ và thế năng cuối cùng $\Phi_t$:

$$
\begin{aligned}
    \sum c  &= \sum \hat{c} - \Phi_t + \Phi_s\\
            &\leq \Phi_s - \Phi_t\\
            &= O(ns (\log n)^{\log_2{(s+1)}})
\end{aligned}
$$

Chúng ta nhận được cận trên của độ phức tạp là $O(ns (\log n)^{1+\log_2{(s+1)}} + ms (\log n)^{2+\log_2{(s+1)}})$. [^ref1]

### Trường hợp xấp xỉ

Cho một dãy hàm đơn biến liên tục, được định nghĩa đầy đủ $F=\{f_1,\dots,f_n\}$, định nghĩa $\mathfrak U_F(x)$, $\mathfrak L_F(x)$ và $\mathfrak E_F(x)$ lần lượt là bao lồi trên (upper envelope), bao lồi dưới (lower envelope) và biên độ (extent).

$$
\begin{aligned}
    \mathfrak U_F(x) & = \max\{f_i(x) \mid f_i \in F\} \\
    \mathfrak L_F(x) & = \min\{f_i(x) \mid f_i \in F\} \\
    \mathfrak E_F(x) & = \mathfrak U_F(x) - \mathfrak L_F(x)
\end{aligned}
$$

Chúng ta chỉ yêu cầu chương trình trả về $\tilde{\mathfrak U}_F(x)$ thỏa mãn:

$$
\mathfrak U_F(x) \geq \tilde{\mathfrak U}_F(x) \geq \mathfrak U_F(x) - \epsilon \mathfrak E_F(x)
$$

Khi đó trong trường hợp phức tạp, chúng ta có thể đạt được độ phức tạp $O((1/\epsilon^2)n\log^3 n)$, không phụ thuộc vào bậc của đa thức, cũng như cho phép các hàm đồng thời tịnh tiến sang trái hoặc sang phải.

## Tài liệu tham khảo và chú thích

[^ref1]: Cần lưu ý rằng, đây chỉ là cận trên, cận dưới của độ phức tạp phải là $O(\lambda_{s}(n)\log n)$. Tác giả phỏng đoán rằng việc xây dựng phân tích thế năng ở đây nên tham khảo công thức tổng quát của $\lambda_{s}(n)$ tương ứng với dãy Davenport-Schinzel để có được cận trên chặt hơn.

-   P. K. Agarwal, S. Har-Peled, and K. R. Varadarajan. Approximating extent measures of points. J. ACM, 51(4):606–635, July 2004.
-   J. Basch, L. J. Guibas, and J. Hershberger. Data structures for mobile data. Journal of Algorithms, 31(1):1–28, 1999.
-   G. Alexandron, H. Kaplan, and M. Sharir. Kinetic and dynamic data structures for convex hulls and upper envelopes. Computational Geometry, 36(2):144–158, 2007.