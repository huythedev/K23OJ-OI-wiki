author: hsfzLZH1, Ir1d, JosephusW

k-D Tree (KDT, k-Dimension Tree) là một cấu trúc dữ liệu có thể **xử lý hiệu quả thông tin không gian $k$ chiều**.

Khi số lượng nút $n$ lớn hơn nhiều so với $2^k$, hiệu quả thời gian của k-D Tree là rất tốt.

Trong các bài toán lập trình thi đấu, thường gặp trường hợp $k=2$. Khi phân tích độ phức tạp thời gian trong trang này, chúng ta sẽ coi $k$ là một hằng số.

## Xây dựng cây (Build)

k-D Tree có hình thái của một cây tìm kiếm nhị phân (Binary Search Tree - BST), trong đó mỗi nút trên cây tương ứng với một điểm trong không gian $k$ chiều. Các điểm trong mỗi cây con nằm trong một siêu hình hộp chữ nhật (hyper-rectangle) $k$ chiều, và tất cả các điểm nằm trong siêu hình hộp này cũng đều thuộc cây con đó.

Giả sử chúng ta đã biết tọa độ của $n$ điểm phân biệt trong không gian $k$ chiều, để xây dựng một k-D Tree, ta thực hiện các bước sau:

1.  Nếu siêu hình hộp hiện tại chỉ chứa một điểm, trả về điểm đó.

2.  Chọn một chiều (dimension), chia siêu hình hộp hiện tại thành hai phần theo chiều này.

3.  Chọn điểm chia cắt: Trên chiều đã chọn, chọn một điểm làm mốc. Các giá trị nhỏ hơn điểm này trên chiều đó sẽ thuộc về một siêu hình hộp (cây con trái), phần còn lại thuộc về siêu hình hộp kia (cây con phải).

4.  Lấy điểm đã chọn làm nút gốc của cây con này, đệ quy xây dựng cây con trái và phải cho hai siêu hình hộp vừa được chia tách, đồng thời cập nhật thông tin của cây con.

Để dễ hiểu, hãy xem ví dụ khi $k=2$.

![](./images/kdt1.jpg)

Hình thái k-D Tree được xây dựng có thể trông như sau:

![](./images/kdt2.jpg)

Trong đó, tọa độ trên mỗi nút của cây là tọa độ của điểm chia cắt được chọn, ký hiệu $x$ hoặc $y$ bên cạnh các nút không phải lá biểu thị chiều được chọn để cắt.

Độ phức tạp của cách làm trên chưa được đảm bảo. Đối với bước 2 và 3, chúng ta đề xuất hai tối ưu hóa:

1.  Luân phiên chọn $k$ chiều, để đảm bảo trong bất kỳ $k$ tầng liên tiếp nào, mỗi chiều đều được chọn để cắt một lần.
2.  Mỗi lần chọn điểm chia cắt trên một chiều, hãy chọn **trung vị (median)** của chiều đó. Điều này đảm bảo kích thước của cây con trái và phải sau khi chia luôn cân bằng nhất có thể.

Có thể thấy, khi sử dụng tối ưu hóa 2, chiều cao của k-D Tree được xây dựng sẽ tối đa là $\log n + O(1)$.

Hiện tại, điểm nghẽn (bottle neck) về độ phức tạp thời gian khi xây dựng k-D Tree nằm ở việc chọn nhanh trung vị trên một chiều, đồng thời đặt các phần tử nhỏ hơn trung vị sang bên trái và các phần tử còn lại sang bên phải. Nếu mỗi lần đều dùng hàm `sort` để sắp xếp chiều đó, độ phức tạp thời gian sẽ là $O(n\log^2 n)$. Trên thực tế, độ phức tạp để tìm trung vị của $n$ phần tử và đặt nó vào đúng vị trí sau khi sắp xếp có thể đạt được $O(n)$.

Hãy nhớ lại tư tưởng của thuật toán Sắp xếp nhanh (Quick Sort). Mỗi lần chúng ta chọn một số (pivot), đặt các số nhỏ hơn nó sang trái, lớn hơn sang phải, đảm bảo số đó nằm đúng vị trí sau khi sắp xếp, rồi đệ quy sắp xếp bên trái và bên phải. Độ phức tạp kỳ vọng là $O(n\log n)$. Tuy nhiên, vì k-D Tree chỉ yêu cầu trung vị nằm đúng vị trí sau khi sắp xếp, nên ta chỉ cần đệ quy sắp xếp **một phía** chứa trung vị. Có thể chứng minh độ phức tạp kỳ vọng của việc này là $O(n)$. Trong thư viện `algorithm` của C++, hàm `nth_element()` thực hiện chức năng này: tìm giá trị tại vị trí `s[mid]` sau khi đoạn từ `s[l]` đến `s[r]` được sắp xếp theo quy tắc `cmp`, đồng thời đảm bảo các giá trị bên trái `s[mid]` nhỏ hơn `s[mid]` và bên phải lớn hơn `s[mid]`. Chỉ cần gọi `nth_element(s+l, s+mid, s+r+1, cmp)`.

Nhờ tư tưởng này, độ phức tạp thời gian để xây dựng k-D Tree là $O(n\log n)$.

## Các thao tác trên không gian nhiều chiều

Khi truy vấn thông tin của tất cả các điểm nằm trong một vùng hình hộp chữ nhật nhiều chiều (hyper-rectangle), ta ghi lại tọa độ lớn nhất và nhỏ nhất trên mỗi chiều của tất cả các điểm trong mỗi cây con.
- Nếu hình hộp tương ứng với cây con hiện tại không giao với hình hộp cần truy vấn, ta không tiếp tục tìm kiếm trong cây con đó.
- Nếu hình hộp tương ứng với cây con hiện tại nằm hoàn toàn trong hình hộp cần truy vấn, trả về tổng trọng số của tất cả các điểm trong cây con đó.
- Ngược lại, kiểm tra xem điểm hiện tại có nằm trong hình hộp cần truy vấn hay không để cập nhật kết quả, sau đó đệ quy tìm kiếm trong cây con trái và phải.

??? note "Cài đặt tham khảo"
    ```cpp
    int query(int p) {
      if (!p) return 0;
      bool flag{false};
      // Kiểm tra xem hình hộp của nút p có nằm ngoài phạm vi truy vấn (l, h) hay không
      for (int k : {0, 1}) flag |= (!(l.x[k] <= t[p].L[k] && t[p].R[k] <= h.x[k]));
      if (!flag) return t[p].sum; // Nằm hoàn toàn trong phạm vi truy vấn
      
      // Kiểm tra xem hình hộp của nút p có giao với phạm vi truy vấn hay không (cắt tỉa)
      for (int k : {0, 1})
        if (t[p].R[k] < l.x[k] || h.x[k] < t[p].L[k]) return 0;
        
      int ans{0};
      flag = false;
      // Kiểm tra điểm p có nằm trong phạm vi truy vấn không
      for (int k : {0, 1}) flag |= (!(l.x[k] <= t[p].x[k] && t[p].x[k] <= h.x[k]));
      if (!flag) ans = t[p].v;
      
      return ans += query(t[p].l) + query(t[p].r);
    }
    ```

### Phân tích độ phức tạp

Xét trường hợp 2 chiều (2D), khi truy vấn hình chữ nhật $R$, chúng ta chia các nút trên k-D Tree thành 3 loại:

1.  Không giao với $R$.
2.  Được bao chứa hoàn toàn bởi $R$.
3.  Giao một phần với $R$ (Bị biên của $R$ cắt qua).

Rõ ràng độ phức tạp của một lần truy vấn phụ thuộc vào số lượng nút loại 3. Chú ý rằng hình chữ nhật bao của nút loại 3 hoặc chứa hoàn toàn $R$, hoặc không chứa $R$ (nhưng giao nhau), loại đầu tiên rõ ràng chỉ có $O(h) = O(\log n)$ nút. Bây giờ ta phân tích số lượng nút loại sau (không bao chứa $R$ nhưng bị cắt).

Trước hết, giả sử ta dịch chuyển tất cả các cạnh của hình chữ nhật một đoạn nhỏ $\epsilon$ sao cho hình chữ nhật truy vấn không đi qua bất kỳ điểm nào đã có. Điều này rõ ràng không ảnh hưởng đến tập hợp điểm nằm trong hình chữ nhật truy vấn.

Nhận thấy rằng các hình chữ nhật tương ứng với các nút loại 3 (không bao chứa nhau) chắc chắn bị ít nhất một cạnh của $R$ cắt qua. Do đó, ta chỉ cần tính số lượng hình chữ nhật bị mỗi cạnh của $R$ cắt qua, tức là một đoạn thẳng bất kỳ có thể đi qua tối đa bao nhiêu hình chữ nhật tương ứng với các điểm.

Xét một nút $u$, nó có 4 cháu (nút con của con), và đường đi từ nó đến mỗi cháu đều trải qua 2 lần phân chia trên 2 chiều. Quan sát thấy rằng, với cách chia một hình chữ nhật thành 4 hình chữ nhật con như vậy, một đoạn thẳng song song với trục tọa độ đi qua tối đa 2 vùng. Tức là từ $u$, truy vấn đi xuống tối đa 2 người cháu vẫn còn là nút loại 3 (nếu đoạn thẳng trùng với biên phân chia thì không chắc chắn, nhưng thao tác dịch chuyển biên hình chữ nhật truy vấn ở trên đã loại bỏ trường hợp này).

Vì khi xây dựng cây, mỗi điểm là trung vị của cả cây con trên chiều được chọn, nên kích thước cây con chắc chắn giảm một nửa. Đặt $u$ có kích thước cây con là $n$, ta có công thức truy hồi:

$$
T(n)=2T(n/4)+O(1)
$$

Theo Định lý thợ (Master Theorem), ta có $T(n)=O(\sqrt{n})$.

Mở rộng công thức truy hồi sang $k$ chiều, tức là $T(n)=2^{k-1}T(n/2^k)+O(1)$, ta có $T(n)=O(n^{1-\frac1k})$ (coi $k$ là hằng số).

### Chèn/Xóa (Insert/Delete)

Nếu tập điểm $k$ chiều được duy trì là biến động, tức là có thể thêm hoặc xóa điểm, lúc này tính cân bằng của k-D Tree không được đảm bảo. Do cấu trúc của k-D Tree, nó không hỗ trợ xoay (rotation) như các cây cân bằng thông thường, và việc gán độ ưu tiên ngẫu nhiên kiểu FHQ Treap cũng không đảm bảo độ phức tạp. Có hai phương pháp bảo trì phổ biến:

???+ note "Lưu ý"
    Nhiều thí sinh sử dụng cấu trúc **Cây Scapegoat** (Cây thế mạng) để bảo trì. Tuy nhiên, lưu ý rằng trong phần phân tích độ phức tạp ở trên, ta yêu cầu kích thước cây con của các con phải giảm nghiêm ngặt một nửa, tức là chiều cao cây phải xấp xỉ $\log n + O(1)$. Trong khi đó, Cây Scapegoat chỉ đảm bảo chiều cao $O(\log n)$ (với hằng số lớn hơn), nên độ phức tạp truy vấn $O(n^{1-\frac1k})$ không được đảm bảo chặt chẽ về lý thuyết (dù thực tế vẫn chạy khá tốt).

#### Tái cấu trúc căn bậc hai (Square Root Reconstruction)

Khi chèn, ta lưu tạm các điểm cần chèn lại, cứ sau mỗi $B$ lần chèn thì tiến hành xây dựng lại (rebuild) toàn bộ cây.

Khi xóa, chỉ cần đánh dấu là đã xóa. Nếu yêu cầu nghiêm ngặt hơn, có thể duy trì đếm số lượng điểm đã bị xóa trong cây con, nếu đạt đến $B$ thì tái cấu trúc.

Độ phức tạp sửa đổi trung bình (amortized) là $O(n\log n/B)$, truy vấn là $O(B+n^{1-\frac1k})$. Nếu số lượng truy vấn và sửa đổi cùng bậc thì $B=O(\sqrt{n\log n})$ là tối ưu (Sửa đổi $O(\sqrt{n\log n})$, Truy vấn $O(\sqrt{n\log n}+n^{1-\frac1k})$).

#### Phân nhóm nhị phân (Binary Grouping / Logarithmic Method)

Duy trì một số cây k-D Tree có kích thước là lũy thừa của 2, sao cho tổng kích thước các cây này bằng $n$.

Khi chèn, tạo một k-D Tree mới kích thước 1, sau đó liên tục gộp các cây có cùng kích thước (bằng cách đập phẳng và xây lại). Khi cài đặt có thể chỉ cần xây lại một lần.

Dễ thấy kích thước các cây cần gộp luôn bắt đầu từ $2^0$ và số mũ liên tiếp nhau. Độ phức tạp tương tự như phép cộng nhị phân, trung bình là $O(n\log^2 n)$, vì bản thân việc xây dựng lại tốn $\log$.

Khi truy vấn, thực hiện truy vấn trên từng cây, độ phức tạp là $O\left(\sum_{i\geq0} (\frac n{2^i})^{1-\frac1k}\right)=O(n^{1-\frac1k})$.

### Bài tập ví dụ

???+ note "[Luogu P4148 Bài toán đơn giản](https://www.luogu.com.cn/problem/P4148)"
    Trên một ma trận 2 chiều $n\times n$ với giá trị ban đầu toàn là $0$, thực hiện $q$ thao tác, mỗi thao tác thuộc một trong hai loại sau:
    
    1.  `1 x y A`: Cộng thêm $A$ vào số tại tọa độ $(x,y)$.
    2.  `2 x1 y1 x2 y2`: Xuất ra tổng các số trong hình chữ nhật có góc dưới trái là $(x1,y1)$ và góc trên phải là $(x2,y2)$ (bao gồm cả biên).
    
    Bắt buộc online (Forced online). Giới hạn bộ nhớ `20M`. Đảm bảo đáp án và các biến trung gian nằm trong phạm vi `int`.
    
    $1\le n\le 500000 , 1\le q\le 200000$

Giới hạn bộ nhớ 20M loại bỏ tất cả các phương pháp Cây lồng cây (Segment Tree 2D), bắt buộc online loại bỏ CDQ phân trị, nên chỉ có thể dùng k-D Tree.

Dưới đây là code tham khảo sử dụng phương pháp Phân nhóm nhị phân.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/ds/code/kdt/kdt_3.cpp"
    ```

## Truy vấn lân cận (Nearest Neighbor Search)

???+ warning "Cảnh báo"
    Sử dụng k-D Tree để truy vấn điểm gần nhất có độ phức tạp thời gian tồi nhất vẫn là $O(n)$, nhưng đây vẫn là một thuật toán "cắn điểm" (heuristic) xuất sắc, hãy chú ý khi sử dụng. Ở đây, phần giải thích về truy vấn lân cận chỉ nhằm mục đích củng cố hiểu biết về cấu trúc k-D Tree.

???+ note "Ví dụ [luogu P1429 Cặp điểm gần nhất mặt phẳng (Bản nâng cao)](https://www.luogu.com.cn/problem/P1429)"
    Cho $n$ điểm $(x_i,y_i)$ trên mặt phẳng, tìm [khoảng cách Euclid](../geometry/distance.md#khoảng-cách-euclid) giữa hai điểm gần nhất.
    
    $2\le n\le 200000 , 0\le x_i,y_i\le 10^9$

Đầu tiên, xây dựng 2-D Tree cho $n$ điểm này.

Duyệt qua từng nút, đối với mỗi nút, tìm điểm khác nó có khoảng cách nhỏ nhất, từ đó tìm ra đáp án. Nếu duyệt trâu từng nút trên 2-D Tree thì độ phức tạp là $O(n)$, cần phải cắt tỉa (pruning). Ta có thể duy trì giá trị nhỏ nhất và lớn nhất của tọa độ trên mỗi chiều trong một cây con. Giả sử khoảng cách gần nhất hiện tại tìm được là $ans$, nếu khoảng cách **ngắn nhất** từ điểm đang xét đến hình chữ nhật bao quanh cây con lớn hơn hoặc bằng $ans$, thì chắc chắn trong cây con này không có đáp án tốt hơn, ta không cần tìm kiếm trong cây con đó nữa.

Ngoài ra, có thể sử dụng phương pháp tìm kiếm heuristic: nếu cả hai cây con đều có khả năng chứa đáp án, ta ưu tiên tìm kiếm trong cây con có khoảng cách gần điểm đang xét hơn. Có thể coi **khoảng cách ngắn nhất từ điểm đang xét đến hình chữ nhật tương ứng với cây con là hàm ước lượng (hàm heuristic) của bài toán này**.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/ds/code/kdt/kdt_1.cpp"
    ```

???+ note "Ví dụ [「CQOI2016」Cặp điểm xa thứ K](https://loj.ac/problem/2043)"
    Cho $n$ điểm $(x_i,y_i)$ trên mặt phẳng, tìm khoảng cách giữa cặp điểm xa thứ $k$ (theo khoảng cách Euclid).
    
    $n\le 100000 , 1\le k\le 100 , 0\le x_i,y_i<2^{31}$

Tương tự bài trước, từ cặp điểm gần nhất chuyển thành cặp điểm xa thứ $k$, hàm ước lượng đổi thành khoảng cách xa nhất từ điểm đang xét đến vùng hình chữ nhật của cây con. Sử dụng một Min-Heap (Hàng đợi ưu tiên bé) để duy trì $k$ khoảng cách lớn nhất tìm được. Nếu khoảng cách tìm được hiện tại lớn hơn đỉnh Heap, ta đẩy đỉnh Heap ra và chèn khoảng cách này vào. Tương tự, sử dụng giá trị đỉnh Heap để cắt tỉa.

Vì đề bài yêu cầu cặp điểm không có thứ tự (vô hướng), tức là đổi chỗ hai điểm vẫn là một cặp, nên mỗi cặp điểm có thứ tự sẽ được tính 2 lần. Do đó, giá trị $k$ đọc vào cần nhân với 2.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/ds/code/kdt/kdt_2.cpp"
    ```

## Bài tập

[「SDOI2010」Trốn tìm](https://www.luogu.com.cn/problem/P2479)

[「Violet」Búp bê thiên sứ / SJY xếp quân cờ](https://www.luogu.com.cn/problem/P4169)

[「National Training Team」JZPFAR](https://www.luogu.com.cn/problem/P2093)

[「BOI2007」Mokia](https://www.luogu.com.cn/problem/P4390)

[luogu P4475 Vương quốc Chocolate](https://www.luogu.com.cn/problem/P4475)

[「CH Weak Province Huce R2」TATT](https://www.luogu.com.cn/problem/P3769)