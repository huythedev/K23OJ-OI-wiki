## Giới thiệu

**Thuật toán Garsia–Wachs** (Garsia–Wachs Algorithm) là một thuật toán hiệu quả dùng để xây dựng **cây nhị phân tìm kiếm tối ưu** và **mã Huffman theo thứ tự chữ cái** trong **thời gian tuyến tính**. Thuật toán được đặt theo tên Adriano Garsia và Michelle L. Wachs, những người công bố bài báo liên quan năm 1977.

## Mô tả bài toán

Cho số nguyên $n$, với $n+1$ trọng số không âm $w_{0},w_{1},\dots ,w_{n}$, hãy xây dựng một cây nhị phân có gốc, trong đó $n$ nút trong đều có đúng hai con, tức cây có $n+1$ lá. Ánh xạ lần lượt $n+1$ trọng số đầu vào với các lá theo thứ tự. Mục tiêu là trong tất cả cây có $n$ nút trong, tìm cây có tổng trọng số độ dài đường đi từ gốc tới mỗi lá là nhỏ nhất.

### Cây nhị phân tìm kiếm tối ưu

Bài toán có thể hiểu là xây dựng cây tìm kiếm nhị phân cho $n$ khóa theo thứ tự, với giả thiết cây chỉ dùng để tìm các giá trị **không** có trong cây. Khi đó $n$ khóa chia không gian tìm kiếm thành $n+1$ khoảng, và trọng số của mỗi khoảng là xác suất truy vấn rơi vào khoảng đó. Tổng trọng số độ dài đường đi ngoài quyết định kỳ vọng thời gian tìm kiếm.

### Mã Huffman theo thứ tự chữ cái

Bài toán cũng có thể xem là xây dựng mã Huffman. Đây là phương pháp mã hóa $n+1$ giá trị bằng dãy nhị phân độ dài biến đổi. Trong diễn giải này, mã của một giá trị là dãy bước trái/phải từ gốc tới lá (ví dụ trái là $0$, phải là $1$). Khác với Huffman chuẩn, mã Huffman xây theo cách này có thứ tự chữ cái: thứ tự từ điển của các mã nhị phân trùng với thứ tự đầu vào. Nếu trọng số của một giá trị là tần suất xuất hiện trong thông điệp, đầu ra của Garsia–Wachs là mã Huffman theo thứ tự chữ cái cho độ dài thông điệp ngắn nhất.

## Quy trình

Thuật toán Garsia–Wachs thường gồm ba giai đoạn:

1.  Xây một cây nhị phân mà giá trị nằm ở lá, lưu ý thứ tự có thể sai.
2.  Tính khoảng cách từ gốc tới từng lá.
3.  Xây một cây nhị phân khác có cùng khoảng cách lá nhưng thứ tự đúng.

![](./images/garsia-wachs.png)

Như hình, ở giai đoạn 1 ta tạo cây bằng cách tìm và gộp các bộ ba không thứ tự trong dãy đầu vào (bên trái); còn cây kết quả ở bên phải có thứ tự lá đúng nhưng chiều cao lá giống cây kia.

Nếu thêm hai giá trị canh gác $\infty$ (hoặc bất kỳ giá trị hữu hạn đủ lớn) vào đầu và cuối dãy, giai đoạn 1 sẽ dễ mô tả hơn. Vì vậy trong lời giải thi đấu, với mảng $\mathit{num}$ dài $n$, thường đặt $\mathit{num}[0] = \mathit{num}[n+1] = \infty$.

Giai đoạn 1 duy trì một rừng gồm các cây một nút, mỗi cây tương ứng với một trọng số đầu vào không phải canh gác. Mỗi cây có một giá trị bằng tổng trọng số lá của nó. Để duy trì thứ tự các giá trị, thêm hai giá trị canh gác ở hai đầu. Dãy ban đầu chính là thứ tự trọng số lá. Sau đó lặp các bước sau, mỗi bước giảm độ dài dãy, cho đến khi chỉ còn một cây chứa tất cả lá:

-   Tìm ba giá trị liên tiếp $x$，$y$，$z$ sao cho $x \leq z$. Do giá trị canh gác ở cuối lớn hơn mọi giá trị hữu hạn nên luôn tồn tại bộ ba này.
-   Loại $x$ và $y$ khỏi dãy, tạo một nút cha mới của hai nút tương ứng với giá trị $x+y$.
-   Chèn lại nút mới vào bên phải giá trị gần $x$ nhất ở phía trái sao cho giá trị đó $\ge x+y$. Do có canh gác bên trái nên luôn tồn tại vị trí.

Để hiện thực hiệu quả giai đoạn này, có thể dùng bất kỳ cây tìm kiếm cân bằng nào để duy trì dãy hiện tại. Cấu trúc này cho phép xóa $x,y$ và chèn lại nút cha trong thời gian log. Ở mỗi bước, các trọng số tại chỉ số chẵn tới $y$ tạo thành một dãy giảm, các trọng số tại chỉ số lẻ tạo thành một dãy giảm khác. Vì vậy vị trí chèn $x+y$ có thể tìm bằng hai lần tìm nhị phân trên hai dãy giảm trong $O(\log n)$. Việc tìm bộ ba đầu tiên thỏa $x \le z$ có thể làm bằng quét tuyến tính bắt đầu từ giá trị $z$ trước đó, cho tổng thời gian tuyến tính.

Chứng minh của giai đoạn 3 — tồn tại một cây khác có cùng khoảng cách lá và là nghiệm tối ưu — là phần quan trọng nhưng khá phức tạp nên lược bỏ. Với tính đúng đắn của giai đoạn 3, giai đoạn 2 và 3 đều có thể thực hiện trong thời gian tuyến tính. Do đó, với đầu vào dài $n$, tổng độ phức tạp là $O(n\log n)$.

## Ứng dụng

Trong Haskell, gói [garsia-wachs package](https://hackage.haskell.org/package/garsia-wachs) hiện thực thuật toán Garsia–Wachs theo phong cách hàm. Nó chủ yếu dùng để xây dựng bảng tìm kiếm tối ưu hoặc cân bằng [rope](https://hackage.haskell.org/package/rope) với độ phức tạp tối ưu.

???+ note "Chú thích"
    **rope** là công cụ trong Haskell dùng để thao tác bytestring có chú thích tùy chọn, dựa trên [cây ngón tay](../ds/finger-tree.md).

## Ví dụ

???+ note "[POJ 1738 An old Stone Game](http://poj.org/problem?id=1738)"
    Một trò chơi đá cổ: ban đầu có $n$ ($1 \leq n \leq 50000$) đống đá xếp thành hàng. Mỗi bước có thể gộp hai đống kề nhau thành một đống mới. Điểm là tổng số đá của đống mới. Hãy tính tổng điểm nhỏ nhất.

??? note "Ý tưởng"
    Bài gộp đá kinh điển thường giải bằng DP đoạn, nhưng khi $n$ lớn (như ở đây), dùng Garsia–Wachs hiệu quả hơn: Bước 1, khởi tạo mảng $\mathit{num}$ kích thước $n$ với $\mathit{num}[0] = \mathit{num}[n+1] = \infty$. Bước 2, mỗi lần tìm $i$ nhỏ nhất sao cho $\mathit{num}[i-1] \leq \mathit{num}[i+1]$, gộp $\mathit{num}[i-1], \mathit{num}[i]$ thành $\mathit{temp}$; tìm $j$ lớn nhất phía trước sao cho $\mathit{num}[j] > \mathit{temp}$, rồi chuyển $\mathit{temp}$ ra sau $j$. Lặp đến khi còn 1 đống.
    Với ràng buộc chỉ gộp đống kề nhau: vì $\mathit{num}[j]\geq \mathit{num}[i-1] + \mathit{num}[i]$, ta có thể coi $\mathit{num}[j+1]$ đến $\mathit{num}[i-2]$ là một khối $\mathit{num}[mid]$, nên nhất định sẽ gộp $\mathit{sum}$ trước. Do đó không vi phạm yêu cầu đề.

???+ note "[ATCODER N-Slimes](https://atcoder.jp/contests/dp/tasks/dp_n)"
    Có $N$ slime xếp thành hàng. Kích thước slime thứ $i$ là $a_{i}$. Taro muốn gộp tất cả thành một slime lớn hơn. Anh ta lặp thao tác: chọn hai slime kề nhau, gộp thành slime mới có kích thước $x+y$ (với $x,y$ là kích thước trước gộp). Chi phí của bước này là $x+y$. Hỏi tổng chi phí nhỏ nhất.

## Tài liệu tham khảo & đọc thêm

1.  [Garsia–Wachs algorithm - Wikipedia](https://en.wikipedia.org/wiki/Garsia%E2%80%93Wachs_algorithm)
2.  [Data.Algorithm.GarsiaWachs - Hackage Haskell](https://hackage.haskell.org/package/garsia-wachs-1.2/docs/Data-Algorithm-GarsiaWachs.html)
3.  [garsia-wachs: A Functional Implementation of the Garsia-Wachs Algorithm](https://hackage.haskell.org/package/garsia-wachs)
4.  [Sentinel value - Wikipedia](https://en.wikipedia.org/wiki/Sentinel_value)
5.  [A new proof of the Garsia-Wachs algorithm](https://www.sciencedirect.com/science/article/abs/pii/0196677488900090)
