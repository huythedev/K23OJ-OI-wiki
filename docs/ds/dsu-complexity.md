tác giả: orzAtalod

Phần này được đăng lại và sửa đổi từ [Độ phức tạp thời gian - Thảo luận sơ lược về phân tích năng lượng thế (Potential Function Analysis)](https://www.luogu.com.cn/blog/Atalod/shi-jian-fu-za-du-shi-neng-fen-xi-qian-tan)，đã được tác giả gốc cho phép.

## Định nghĩa

### Hàm Ackermann

Ở đây, đưa ra định nghĩa của $\alpha(n)$ trước. Để đưa ra định nghĩa này, trước hết đưa ra định nghĩa của $A_k(j)$.

Định nghĩa $A_k(j)$ là:

$$
A_k(j)=\left\{
\begin{aligned}
&j+1& &k=0&\\
&A_{k-1}^{(j+1)}(j)& &k\geq1&
\end{aligned}
\right.
$$

Tức là hàm Ackermann.

Ở đây, $f^i(x)$ biểu thị việc áp dụng hàm $f$ liên tiếp lên $x$ $i$ lần, tức là $f^0(x)=x$，$f^i(x)=f(f^{i-1}(x))$.

Tiếp theo định nghĩa $\alpha(n)$ là giá trị nguyên nhỏ nhất sao cho $A_{\alpha(n)}(1)\geq n$. Chú ý, trước đây ta mô tả nó là $A_{\alpha(n)}(\alpha(n))\geq n$, dù sao tốc độ tăng trưởng của chúng đều rất chậm, giá trị không vượt quá 4.

### Định nghĩa cơ bản

Mỗi nút có một hạng (rank). Hạng ở đây không phải là số lượng nút, mà là độ sâu. Hạng ban đầu của nút là 0. Khi hợp nhất, nếu hạng của hai nút khác nhau, thì hợp nút có hạng nhỏ hơn vào nút có hạng lớn hơn, và không cập nhật giá trị hạng của nút lớn hơn. Ngược lại, ngẫu nhiên hợp nhất một nút vào nút kia, và tăng hạng của nút gốc lên 1. Hạng của nút gốc cho biết chiều cao của cây đó. Ký hiệu hạng của $x$ là $rnk(x)$, tương tự, ký hiệu nút cha của $x$ là $fa(x)$. Ta luôn có $rnk(x)+1\leq rnk(fa(x))$.

Để định nghĩa hàm thế năng, cần định nghĩa trước một hàm phụ trợ $level(x)$. Trong đó, $level(x)=\max(k:rnk(fa(x))\geq A_k(rnk(x)))$. Khi $rnk(x)\geq1$, định nghĩa thêm một hàm phụ trợ $iter(x)=\max(i:rnk(fa(x))\geq A_{level(x)}^i(rnk(x)))$. Các hàm này định nghĩa cho $x$ thỏa mãn $rnk(x)>0$ và $x$ không phải là nút gốc của một cây nào đó.

Các định nghĩa trên có thể làm bạn hơi choáng váng. Ta sắp xếp lại, đối với một $x$ và $fa(x)$, nếu $rnk(x)>0$, luôn có thể tìm được cặp $i,k$ để $rnk(fa(x))\geq A_k^i(rnk(x))$, trong đó $level(x)=\max(k)$, với điều kiện này, $iter(x)=\max(i)$. $level$ mô tả cấp lặp lại lớn nhất của $A$, còn $iter$ mô tả số lần lặp lại lớn nhất ở cấp lặp lớn nhất.

Đối với hai hàm này, $level(x)$ luôn tăng hoặc không đổi theo các thao tác, nếu $level(x)$ không tăng, $iter(x)$ cũng chỉ tăng hoặc không đổi. Hơn nữa, chúng luôn thỏa mãn hai bất đẳng thức sau:

$$
0\leq level(x)<\alpha(n)
$$

$$
1\leq iter(x)\leq rnk(x)
$$

Xem xét định nghĩa của các hàm này và $A_k^j$, rất dễ chứng minh, xin để lại cho độc giả để làm quen với định nghĩa.

Định nghĩa hàm thế năng $\Phi(S)=\sum\limits_{x\in S}\Phi(x)$, trong đó $S$ biểu thị toàn bộ tập hợp hợp nhất (disjoint set), và $x$ là một nút trong tập hợp hợp nhất. Định nghĩa $\Phi(x)$ là:

$$
\Phi(x)=
\begin{cases}
\alpha(n)\times \mathit{rnk}(x)& \mathit{rnk}(x)=0\ \text{hoặc}\ x\ \text{là nút gốc của một cây nào đó}\\
(\alpha(n)-\mathit{level}(x))\times \mathit{rnk}(x)-iter(x)& \text{ngược lại}
\end{cases}
$$

Sau đó là chứng minh độ phức tạp thời gian khấu hao là $\Theta(\alpha(n))$ dựa trên sự thay đổi thế năng do các thao tác gây ra. Chú ý, thao tác $union(x,y)$ mà ta thảo luận ở đây đảm bảo $x$ và $y$ đều là nút gốc của một cây, do đó không cần thực hiện thêm $find(x)$ và $find(y)$.

Có thể thấy, thế năng luôn là một số không âm. Ngoài ra, lúc bắt đầu, thế năng của tập hợp hợp nhất là $0$.

## Chứng minh

### Thao tác union(x,y)

Nó tiêu tốn thời gian $\Theta(1)$, vì vậy ta xét sự thay đổi thế năng mà nó gây ra.

Ở đây, ta giả sử $rnk(x)\leq rnk(y)$, tức là $x$ được nối vào $y$. Khi đó, các nút gây tăng thế năng chỉ có $x$ (từ nút gốc chuyển thành nút không phải gốc), $y$ (hạng có thể tăng) và các nút con $c$ của $y$ trước thao tác (hạng của nút cha có thể tăng). Ta chứng minh trước rằng thế năng của nút con $c$ của $y$ trước thao tác không thể tăng, và nếu giảm thì ít nhất giảm 1.

Đặt $\Phi(c)$ là thế năng trước thao tác, $\Phi(c')$ là sau thao tác, ở đây $c$ có thể là bất kỳ nút không phải gốc nào có $rnk(c)>0$, thao tác có thể là bất kỳ thao tác nào, bao gồm cả thao tác find bên dưới. Ta chia làm ba trường hợp để thảo luận.

1.  $iter(c)$ và $level(c)$ không tăng. Hiển nhiên có $\Phi(c)=\Phi(c')$.
2.  $iter(c)$ tăng, $level(c)$ không tăng. Ở đây $iter(c)$ ít nhất tăng 1, tức là $\Phi(c')\leq \Phi(c)-1$, hàm thế năng giảm đi, và ít nhất giảm 1.
3.  $level(c)$ tăng, $iter(c)$ có thể giảm. Nhưng do $0<iter(c)\leq rnk(c)$, $iter(c)$ nhiều nhất giảm $rnk(c)-1$, trong khi $level(c)$ ít nhất tăng $1$. Theo định nghĩa $\Phi(c)=(\alpha(n)-level(c))\times rnk(c)-iter(c)$, suy ra $\Phi(c')\leq\Phi(c)-1$.
4.  Các trường hợp khác. Do $rnk(c)$ không đổi, $rnk(fa(c))$ không giảm, nên không xảy ra.

Vì vậy, nút gây tăng thế năng chỉ có thể là $x$ hoặc $y$. Mà $x$ từ nút gốc chuyển thành nút không phải gốc, nếu $rnk(x)=0$, thì luôn có $\Phi(x)=\Phi(x')=0$. Ngược lại, chắc chắn có $\alpha(x)\times rnk(x)\geq(\alpha(n)-level(x))\times rnk(x)-iter(x)$. Tức là, $\Phi(x')\leq \Phi(x)$.

Do đó, nút duy nhất có thế năng có thể tăng là $y$. Mà thế năng của $y$ nhiều nhất tăng $\alpha(n)$. Vì vậy, độ phức tạp khấu hao của thao tác $union$ là $\Theta(\alpha(n))$.

### Thao tác find(a)

Nếu đường đi tìm kiếm chứa $\Theta(s)$ nút, hiển nhiên độ phức tạp thời gian tìm kiếm là $\Theta(s)$. Nếu do thao tác tìm kiếm, không có nút nào làm tăng thế năng, và ít nhất có $s-\alpha(n)$ nút có thế năng ít nhất giảm 1, có thể chứng minh độ phức tạp thời gian thao tác $find(a)$ là $\Theta(\alpha(n))$. Để tránh nhầm lẫn, ở đây dùng $a$ làm tham số, còn $x$ xuất hiện là chỉ một nút bất kỳ trong tập hợp hợp nhất.

Trước hết chứng minh không có nút nào làm tăng thế năng. Rất rõ ràng, ta đã chứng minh ở trên tất cả các nút không phải gốc đều có thế năng không tăng, mà hạng của nút gốc không thay đổi, nên không có nút nào làm tăng thế năng.

Tiếp theo chứng minh ít nhất có $s-\alpha(n)$ nút có thế năng ít nhất giảm 1. Ta đã chứng minh ở trên, nếu $level(x)$ hoặc $iter(x)$ thay đổi, thế năng của chúng ít nhất giảm 1. Vì vậy, chỉ cần chứng minh ít nhất có $s-\alpha(n)$ nút có $level(x)$ hoặc $iter(x)$ thay đổi là đủ.

Nhắc lại định nghĩa thế năng của nút không phải gốc, $\Phi(x)=(\alpha(n)-level(x))\times rnk(x)-iter(x)$, mà $level(x)$ và $iter(x)$ là giá trị lớn nhất sao cho $rnk(fa(x))\geq A_{level(x)}^{iter(x)}(rnk(x))$.

Vì vậy, chỉ cần chứng minh $rnk(root_x)\geq A_{level(x)}^{iter(x)+1}(rnk(x))$ là được. Theo định nghĩa của $A_k^i$, $A_{level(x)}^{iter(x)+1}(rnk(x))=A_{level(x)}(A_{level(x)}^{iter(x)}(rnk(x)))$.

Chú ý, ta có thể dùng $k(x)$ đại diện cho $level(x)$, $i(x)$ đại diện cho $iter(x)$ để tránh công thức quá dài. Ở đây, chính là $rnk(root_x)\geq A_{k(x)}(A_{k(x)}^{i(x)}(x))$.

Khi bạn nhìn thấy điều này, có thể sẽ có cảm giác "cái gì thế này". Điều này có nghĩa là bạn có thể cần xem lại nhiều lần, hoặc bỏ qua một số nội dung rồi xem lại sau.

Ở đây, ta cần một $A_{k(x)}$ bên ngoài, có nghĩa là ta có thể cần tìm thêm một nút $y$. Đặt $y$ là nút trên đường đi tìm kiếm sau $x$ thỏa mãn $k(y)=k(x)$, ở đây "sau đường đi tìm kiếm" tương đương với "là tổ tiên của $x$". Rõ ràng, không phải mọi $x$ đều có một $y$ như vậy. Dễ dàng chứng minh, số lượng $x$ không có $y$ như vậy không quá $\alpha(n)+2$ cái. Bởi vì chỉ có $x$ cuối cùng của mỗi $k$ và $a$ cùng với $root_a$ là không có $y$ như vậy.

Ta nhấn mạnh lại $fa(x)$ chỉ nút cha của $x$ **trước khi** nén đường đi, nút cha của $x$ **sau khi** nén đường đi đều được ký hiệu là $root_x$. Đối với mỗi $x$ tồn tại $y$, luôn có $rnk(y)\geq rnk(fa(x))$. Đồng thời, ta có $rnk(fa(x))\geq A_{k(x)}^{i(x)}(rnk(x))$. Do $k(x)=k(y)$, ta dùng $k$ để gọi chung, tức là, $rnk(fa(x))\geq A_k^{i(x)}(rnk(x))$. Ta cần tạo ra một $A_k$, nên ta có thể không quan tâm đến giá trị của $iter(y)$, mà trực tiếp dùng bất đẳng thức yếu hơn $rnk(fa(y))\geq A_k(rnk(y))$.

Nếu ta kết hợp các bất đẳng thức lại, điều kỳ diệu sẽ xảy ra. Ta phát hiện ra, $rnk(fa(y))\geq A_k^{i(x)+1}(rnk(x))$. Điều này có nghĩa là, để đi từ $rnk(x)$ lặp đến $rnk(fa(y))$, ít nhất có thể lặp $A_k$ nhiều hơn $i(x)+1$ lần mà không vượt quá $rnk(fa(y))$.

Rõ ràng, có $rnk(root_x)\geq rnk(fa(x))$, và $rnk(x)$ không thay đổi khi nén đường đi. Do đó, ta có thể suy ra $rnk(root_x)\geq A_k^{i(x)+1}(rnk(x))$, nghĩa là giá trị $iter(x)$ ít nhất tăng 1, nếu $rnk(x)$ không tăng, thì chắc chắn $level(x)$ tăng.

Vì vậy, $\Phi(x)$ ít nhất giảm 1. Do có ít nhất $s-\alpha(n)-2$ nút như vậy, nên cuối cùng $\Phi(S)$ ít nhất giảm $s-\alpha(n)-2$, độ phức tạp thời gian khấu hao chính là $\Theta(\alpha(n)+2)=\Theta(\alpha(n))$.

## Tại sao có thể bị kẹt với cấu trúc hợp nhất tập hợp (Disjoint Set Union - DSU)?

Bài toán này hỏi, nếu ta không hợp nhất theo hạng (rank), thì những tính chất nào bị phá vỡ, dẫn đến độ phức tạp thời gian của DSU không thể đảm bảo là $\Theta(m\alpha(n))$.

Nếu khi hợp nhất, nút có hạng lớn hơn được hợp vào nút có hạng nhỏ hơn, ta sẽ đặt hạng của nút có hạng nhỏ hơn bằng giá trị hạng của nút kia cộng một. Như vậy, ta có thể đảm bảo $rnk(fa(x))\geq rnk(x)+1$, từ đó không xảy ra tính chất không phù hợp như đầy lỗi compile.

Rõ ràng, nếu làm như vậy, ta phá vỡ câu "thế năng của $y$ nhiều nhất tăng $\alpha(n)$" trong hàm $union(x,y)$.

Tồn tại một cấu trúc có thể làm giảm độ phức tạp thời gian của DSU có nén đường đi xuống $\Omega(m\log_{1+\frac{m}{n}}n)$, định nghĩa như sau:

Cây nhị thức (thực ra khác với cây nhị thức thông thường), trong đó $j$ là hằng số, $T_k$ là một $T_{k-1}$ cộng thêm một $T_{k-j}$ làm nút con của gốc.

![Cây nhị thức](./images/dsu-complexity.svg)

Điều kiện biên, $T_1$ đến $T_j$ đều là một nút đơn lẻ.

Đặt $rnk(T_k)=r_k$, ở đây ta có $r_k=(k-1)/j$ (chứng minh bỏ qua). Mỗi vòng thao tác, ta nối nó vào một nút đơn lẻ, sau đó truy vấn $j$ nút dưới cùng. Nghĩa là, khi ta nối vào nút đơn lẻ, thế năng của nút đơn lẻ tăng $(k-1)/j+1$. Khi $j=\lfloor\frac{m}{n}\rfloor$, $i=\lfloor\log_{j+1}\frac{n}{2}\rfloor$, $k=ij$, lượng tăng thế năng là:

$$
\alpha(n)\times((ij-1)/j+1)=\alpha(n)\times((\lfloor\log_{\lfloor\frac{m}{n}\rfloor+1}\frac{n}{2}\rfloor\times \lfloor\frac{m}{n}\rfloor-1)/\lfloor\frac{m}{n}\rfloor+1)
$$

Biến đổi một chút, bỏ hết các dấu $\lfloor \cdot \rfloor$, có thể suy ra, lượng tăng thế năng $\geq \alpha(n)\times(\log_{1+\frac{m}{n}}n-\frac{n}{m})$, $m$ lần thao tác là $\Omega(m\log_{1+\frac{m}{n}}n-n)=\Omega(m\log_{1+\frac{m}{n}}n)$.

## Về hợp nhất heuristic (Heuristic Merging)

Do hợp nhất theo hạng khó viết hơn hợp nhất heuristic, nên nhiều người giỏi (dalao) sẽ chọn dùng hợp nhất heuristic để viết DSU. Cụ thể, đối với mỗi nút gốc đều duy trì một $size(x)$, mỗi lần hợp nhất nút có $size$ nhỏ hơn vào nút lớn hơn.

Vậy, hợp nhất heuristic có bị kẹt không?

Trước hết, có thể suy luận từ tính chất chứng minh bằng hạng. Nếu $size$ có thể thay thế vị trí của $rnk$, thì có thể dùng hợp nhất heuristic. Tóm tắt nhanh, các tính chất dùng trong chứng minh liên quan đến hạng có ba điều sau:

1.  Mỗi lần hợp nhất, nhiều nhất một nút có hạng tăng, và nhiều nhất tăng 1.
2.  Luôn có $rnk(fa(x))\geq rnk(x)+1$.
3.  Hạng của nút không giảm.

Về điều kiện thứ hai và thứ ba, $siz$ rõ ràng thỏa mãn, nhưng điều kiện thứ nhất không thỏa mãn, nếu hợp nhất $x$ vào $y$, thì $siz(y)$ sẽ tăng thêm $siz(x)$ nhiều như vậy.

Vì vậy, có thể cân nhắc sử dụng $\log_2 siz(x)$ thay cho $rnk(x)$.

Về tính chất thứ nhất, do $siz$ của nút nhiều nhất tăng gấp đôi, nên $\log_2 siz(x)$ nhiều nhất tăng 1. Về tính chất thứ hai và ba, kết luận khá rõ ràng, ở đây lược bỏ chứng minh.

Vì vậy, nếu không muốn viết hợp nhất theo hạng, cứ viết hợp nhất heuristic là được, độ phức tạp thời gian vẫn là $\Theta(m\alpha(n))$.