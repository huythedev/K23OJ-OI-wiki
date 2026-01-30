Bài toán cây Steiner là một bài toán tối ưu tổ hợp, tương tự như cây khung nhỏ nhất, thuộc nhóm các bài toán mạng ngắn nhất. Cây khung nhỏ nhất yêu cầu nối tất cả các đỉnh đã cho bằng mạng có tổng độ dài nhỏ nhất. Còn cây Steiner cho phép thêm các đỉnh phụ ngoài các đỉnh đã cho, nhằm tối thiểu hóa tổng chi phí của mạng nối các đỉnh.

## Dẫn nhập bài toán

Đầu thế kỷ 19, nhà toán học hình học nổi tiếng Steiner tại Đại học Berlin đã nghiên cứu một bài toán đơn giản nhưng rất gợi mở: nối ba ngôi làng bằng hệ thống đường có tổng chiều dài nhỏ nhất. Về mặt toán học, bài toán là: cho ba điểm $A$, $B$, $C$ trên mặt phẳng, tìm điểm $P$ trên mặt phẳng sao cho tổng $a+b+c$ là nhỏ nhất, với $a$, $b$, $c$ lần lượt là khoảng cách từ $P$ đến $A$, $B$, $C$.

Đáp án là: nếu mọi góc trong tam giác $\textit{ABC}$ đều nhỏ hơn $120^{\circ}$, thì $P$ là điểm sao cho các cạnh $\textit{AB}$, $\textit{BC}$, $\textit{AC}$ tạo với $P$ các góc $120^{\circ}$. Nếu tam giác $\textit{ABC}$ có một góc (ví dụ góc $C$) lớn hơn hoặc bằng $120^{\circ}$, thì $P$ trùng với đỉnh $C$.

### Mở rộng bài toán

1.  Trong bài toán Steiner gốc, chỉ có ba điểm cố định $A,B,C$. Có thể mở rộng thành $n$ điểm $A_1,A_2,\dots,A_n$; yêu cầu tìm điểm $P$ trên mặt phẳng sao cho tổng $a_1+a_2+\dots+a_n$ là nhỏ nhất, với $a_i$ là khoảng cách từ $P$ đến $A_i$.

2.  Xét thêm yếu tố trọng số cho các điểm. Khi đó, mỗi điểm $A_i$ có trọng số $w_i$, yêu cầu tìm $P$ sao cho tổng $\sum a_i \cdot w_i$ nhỏ nhất.

3.  Courant và Robbins cho rằng mở rộng đầu tiên là chưa đủ sâu sắc. Để có một mở rộng thực sự giá trị, cần bỏ ý tưởng chỉ tìm một điểm $P$, mà thay vào đó là tìm một "mạng lưới đường thẳng" có tổng chiều dài nhỏ nhất nối $n$ điểm $A_1,A_2,\cdots,A_n$, sao cho mọi cặp điểm đều có thể nối với nhau qua mạng lưới này. Bài toán này được gọi là **bài toán cây Steiner**. Với $n$ điểm, tối đa có $n-2$ điểm phụ (gọi là điểm Steiner). Qua mỗi điểm Steiner, tối đa có ba cạnh đi qua. Nếu có ba cạnh, chúng tạo với nhau các góc $120^{\circ}$; nếu chỉ có hai cạnh, điểm Steiner đó trùng với một điểm đã cho, và góc giữa hai cạnh đó lớn hơn hoặc bằng $120^{\circ}$.

Kết nối nhiều hơn ba điểm để tạo mạng ngắn nhất:

![steiner-tree1](./images/steiner-tree-1.svg)

Ở hình đầu tiên, lời giải gồm năm đoạn thẳng, có hai điểm Steiner (màu đỏ $s_1,s_2$), tại đó ba đoạn thẳng giao nhau với góc $120^{\circ}$. Hình thứ hai có ba điểm Steiner. Hình thứ ba, một số điểm Steiner có thể bị "thoái hóa", tức là trùng với một điểm đã cho.

Chúng ta sẽ mô hình hóa bài toán cây Steiner dưới dạng đồ thị.

![steiner-tree2](./images/steiner-tree-2.svg)

Với hình thức thứ nhất, nếu chọn các điểm quan trọng là $\{1,2,3,4\}$, dễ thấy nếu chỉ nối trực tiếp bốn điểm này thì tổng trọng số nhỏ nhất là 12, nhưng đây không phải là phương án tối ưu. Nếu sử dụng thêm đỉnh số 5, tổng trọng số nhỏ nhất là 9, cho đáp án tốt hơn.

Với hình thức thứ hai, nếu chọn các điểm quan trọng là $\{1,2,3,4\}$, có thể thấy một số điểm trong số này thậm chí không có cạnh nối trực tiếp, nên bắt buộc phải dùng các điểm phụ (điểm Steiner). Khi xét thêm đỉnh 5, tổng trọng số nhỏ nhất là 9.

Ta cũng nhận thấy trong cả hai hình, điểm Steiner của đỉnh 1 và 4 bị "thoái hóa", tức là trùng với chính đỉnh 1 hoặc 4.

## Bài tập mẫu

Trước tiên, hãy làm quen với bài toán cây Steiner nhỏ nhất qua một bài mẫu: [【模板】最小斯坦纳树](https://www.luogu.com.cn/problem/P6192).

Đề bài đã rất rõ ràng: cho một đồ thị liên thông $G$ với $n$ đỉnh và $k$ đỉnh quan trọng, yêu cầu nối $k$ đỉnh này sao cho tổng trọng số các cạnh trong cây khung là nhỏ nhất.

Như đã phân tích, nếu chỉ nối trực tiếp $k$ đỉnh quan trọng thì chưa chắc đã tối ưu, hoặc các đỉnh này không nhất thiết phải nối trực tiếp với nhau. Do đó, cần sử dụng thêm $n-k$ đỉnh còn lại.

Ta sử dụng quy hoạch động trạng thái (state compression DP) để giải. Gọi $f(i,S)$ là tổng trọng số nhỏ nhất của cây gốc tại $i$, bao phủ tất cả các đỉnh trong tập $S$.

Xét chuyển trạng thái:

-   Trước tiên, chuyển giữa các tập con liên thông: $f(i,S)\leftarrow \min(f(i,S),f(i,T)+f(i,S-T))$.

-   Sau đó, với trạng thái liên thông hiện tại, thực hiện phép nới lỏng cạnh: $f(i,S)\leftarrow \min(f(i,S),f(j,S)+w(j,i))$. Trong code dưới, dùng `tree[tot]` để lưu thông tin hai đỉnh $i,j$ liên thông.

??? note "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/graph/code/steiner-tree/steiner-tree_1.cpp"
    ```

Một bài kinh điển khác: [\[WC2008\] Kế hoạch tham quan](https://www.luogu.com.cn/problem/P4294).

Bài này yêu cầu tìm cây Steiner có tổng trọng số các đỉnh nhỏ nhất. Gọi $f(i,S)$ là tổng trọng số nhỏ nhất của cây gốc tại $i$, bao phủ tất cả các đỉnh trong tập $S$, $a_i$ là trọng số đỉnh.

Chuyển trạng thái:

-   $f(i,S)\leftarrow \min(f(i,S),f(i,T)+f(i,S-T)-a_i)$. Vì khi hợp nhất hai tập, $a_i$ bị cộng hai lần nên phải trừ đi.

-   $f(i,S)\leftarrow \min(f(i,S),f(j,S)+w(j,i))$.

Có thể thấy chuyển trạng thái giống bài mẫu ở trên, điểm khó là xuất đáp án, cần lưu lại đường đi trong quá trình DP.

Dùng `pre[i][s]` để lưu thông tin chuyển trạng thái về $i$ và tập $s$. Sau khi DP xong, bắt đầu từ `pre[root][S]`, lần lượt truy vết các đỉnh và tập con, dùng mảng ans để đánh dấu các đỉnh đã sử dụng, khi phân rã hết tập $S$ thì kết thúc.

??? note "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/graph/code/steiner-tree/steiner-tree_2.cpp"
    ```

## Bài tập

-   [【模板】最小斯坦纳树](https://www.luogu.com.cn/problem/P6192)
-   [\[WC2008\] Kế hoạch tham quan](https://www.luogu.com.cn/problem/P4294)
-   [\[JLOI2015\] Kết nối đường ống](https://loj.ac/problem/2110)
-   [\[APIO2013\] Robot](https://www.luogu.com.cn/problem/P3638)
