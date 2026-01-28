## Dẫn nhập

Cho $n$ điểm trên mặt phẳng hai chiều, hãy tìm một cặp điểm có khoảng cách Euclid nhỏ nhất.

Dưới đây, chúng ta sẽ giới thiệu một thuật toán chia để trị có độ phức tạp $O(n\log n)$ để giải quyết bài toán này. Thuật toán này được đề xuất năm 1975 bởi [Franco P. Preparata](https://en.wikipedia.org/wiki/Franco_P._Preparata), và Preparata cùng [Michael Ian Shamos](https://en.wikipedia.org/wiki/Michael_Ian_Shamos) đã chứng minh rằng đây là thuật toán tối ưu trong mô hình cây quyết định.

## Quy trình

Tương tự các thuật toán chia để trị thông thường, ta chia tập $n$ điểm thành hai tập con $S_1, S_2$ có kích thước xấp xỉ nhau, và đệ quy tiếp tục. Tuy nhiên, vấn đề là: Làm sao để hợp lời giải? Tức là làm sao tìm được một cặp điểm gần nhất mà mỗi điểm thuộc một tập khác nhau? Ở đây, giả sử thao tác hợp có độ phức tạp $O(n)$, thì tổng độ phức tạp là $T(n) = 2T(\frac{n}{2}) + O(n) = O(n\log n)$.

Đầu tiên, ta sắp xếp tất cả các điểm theo $x_i$ (ưu tiên 1), $y_i$ (ưu tiên 2), và chọn điểm $p_m$ ($m = \lfloor \frac{n}{2} \rfloor$) làm điểm chia, tách tập thành $A_1, A_2$:

$$
\begin{aligned}
A_1 &= \{p_i \ \big | \ i = 0 \ldots m \}\\
A_2 &= \{p_i \ \big | \ i = m + 1 \ldots n-1 \}
\end{aligned}
$$

Tiếp tục đệ quy, tìm cặp điểm gần nhất trong từng tập con, gọi khoảng cách là $h_1, h_2$, lấy $h = \min(h_1, h_2)$.

Bây giờ đến bước hợp! Ta cần tìm một cặp điểm, mỗi điểm thuộc một tập khác nhau, sao cho khoảng cách nhỏ hơn $h$. Do đó, ta gom tất cả các điểm có hoành độ cách $x_m$ nhỏ hơn $h$ vào tập $B$:

$$
B = \{ p_i \ \big | \ \lvert x_i - x_m \rvert < h \}
$$

Trên hình, đường thẳng $m$ chia các điểm thành hai phần. Bên trái là $A_1$, bên phải là $A_2$.

Theo quy tắc $B = \{ p_i \ \big | \ \lvert x_i - x_m \rvert < h \}$, ta được tập $B$ gồm các điểm màu xanh lá. ![nearest-points1](./images/nearest-points1.png)

Với mỗi điểm $p_i$ trong $B$, mục tiêu là tìm một điểm khác trong $B$ có khoảng cách nhỏ hơn $h$. Để tránh xét trùng, chỉ xét các điểm có tung độ nhỏ hơn $y_i$. Rõ ràng, với một điểm hợp lệ $p_j$, $y_i - y_j < h$. Ta định nghĩa tập $C(p_i)$:

$$
C(p_i) = \{ p_j\ \big |\ p_j \in B,\ y_i - h < y_j \le y_i \}
$$

Chọn một điểm $p_i$ trong $B$, theo quy tắc $C(p_i) = \{ p_j\ \big |\ p_j \in B,\ y_i - h < y_j \le y_i \}$, ta được tập $C$ gồm các điểm màu vàng trong khung đỏ.

![nearest-points2](./images/nearest-points2.png)

Nếu sắp xếp $B$ theo $y_i$, thì $C(p_i)$ chỉ là một đoạn liên tiếp các điểm gần $p_i$.

Tóm lại, các bước hợp là:

1.  Xây dựng tập $B$.
2.  Sắp xếp $B$ theo $y_i$. Thông thường là $O(n\log n)$, nhưng có thể tối ưu xuống $O(n)$ (giải thích bên dưới).
3.  Với mỗi $p_i \in B$, xét các $p_j \in C(p_i)$, tính khoảng cách và cập nhật đáp án.

Lưu ý, việc sắp xếp hai lần ở trên có thể tối ưu: vì tọa độ các điểm không đổi, lần sắp xếp đầu chỉ cần làm một lần trước khi chia để trị. Mỗi lần đệ quy trả về tập điểm đã sắp xếp theo $y_i$, khi hợp chỉ cần trộn hai tập con đã sắp xếp.

Có vẻ như thuật toán vẫn chưa tối ưu, vì $|C(p_i)|$ có thể là $O(n)$, làm tăng độ phức tạp. Thực ra, $|C(p_i)|$ tối đa chỉ là $7$, chứng minh như sau:

## Chứng minh độ phức tạp

Ta biết rằng, tất cả các điểm trong $C(p_i)$ có tung độ thuộc $(y_i-h, y_i]$; và cùng với $p_i$, hoành độ thuộc $(x_m-h, x_m+h)$. Như vậy, chúng nằm trong một hình chữ nhật $2h \times h$.

Chia hình chữ nhật này thành hai hình vuông $h \times h$, bỏ qua $p_i$, mỗi hình vuông chứa các điểm thuộc $C(p_i) \cap A_1$ và $C(p_i) \cap A_2$, và trong mỗi hình vuông, mọi cặp điểm đều cách nhau hơn $h$ (vì chúng thuộc cùng một nhánh đệ quy).

Chia tiếp mỗi hình vuông $h \times h$ thành bốn hình vuông nhỏ $\frac{h}{2} \times \frac{h}{2}$. Mỗi hình vuông nhỏ chỉ chứa tối đa $1$ điểm: vì đường chéo của nó là $\frac{h}{\sqrt 2}$, nhỏ hơn $h$.

![nearest-points3](./images/nearest-points3.png)

Vậy, mỗi hình vuông lớn chứa tối đa $4$ điểm, cả hình chữ nhật tối đa $8$ điểm, trừ $p_i$ ra thì $\max(C(p_i))=7$.

???+ example "Mã mẫu tham khảo"
    ```cpp
    --8<-- "docs/geometry/code/nearest-points/nearest-points_1.cpp"
    ```

## Mở rộng: Tam giác có chu vi nhỏ nhất trên mặt phẳng

Thuật toán trên có thể mở rộng cho bài toán: Chọn ba điểm trong tập, sao cho tổng khoảng cách giữa các cặp là nhỏ nhất.

Thuật toán về cơ bản không đổi, mỗi lần thử tìm một tam giác có chu vi nhỏ hơn $d$ hiện tại, đưa các điểm có hoành độ cách $x_m$ nhỏ hơn $\frac{d}{2}$ vào tập $B$, rồi thử cập nhật đáp án. (Vì cạnh lớn nhất của tam giác chu vi $d$ nhỏ hơn $\frac{d}{2}$)

## Thuật toán không chia để trị

Ngoài thuật toán chia để trị ở trên, còn có một thuật toán khác cũng có độ phức tạp $O(n \log n)$ mà không dùng chia để trị.

Ý tưởng là: với mỗi điểm, cộng dồn đóng góp của nó với tất cả các điểm bên trái.

Cụ thể, sắp xếp các điểm theo $x_i$ (ưu tiên 1), $y_i$ (ưu tiên 2), và duy trì một multiset theo $y_i$. Với mỗi vị trí $i$, thực hiện:

1.  Xóa khỏi multiset các điểm có $x_i - x_j \ge d$ (không còn đóng góp cho đáp án).
2.  Với các điểm còn lại có $|y_i - y_j| < d$, tính khoảng cách với $p_i$.
3.  Thêm $p_i$ vào multiset.

Mỗi điểm chỉ bị thêm/xóa một lần nên tổng thời gian là $O(n \log n)$. Phần thống kê đáp án cũng có thể chứng minh tương tự như thuật toán chia để trị.

??? example "Mã mẫu tham khảo"
    ```cpp
    --8<-- "docs/geometry/code/nearest-points/nearest-points_2.cpp"
    ```

## Thuật toán kỳ vọng tuyến tính

Ngoài các thuật toán $O(n \log n)$ ở trên, còn có một thuật toán **kỳ vọng** $O(n)$.

Trước tiên, trộn ngẫu nhiên các điểm ([shuffle](../misc/random.md#shuffle)), duy trì đáp án cho tập tiền tố. Khi thêm điểm thứ $i$, giả sử khoảng cách gần nhất hiện tại là $s$, chia mặt phẳng thành các ô lưới cạnh $s$, lưu các điểm trong từng ô (dùng [bảng băm](../ds/hash.md)), rồi kiểm tra các điểm trong 9 ô lân cận điểm thứ $i$ để cập nhật đáp án. Số điểm cần kiểm tra là $O(1)$, vì mỗi ô không chứa quá $4$ điểm (do khoảng cách gần nhất là $s$).

Nếu đáp án được cập nhật, ta xây lại lưới; nếu không thì không cần. Xác suất điểm thứ $i$ thuộc cặp gần nhất là $O\left(\frac{1}{i}\right)$, chi phí xây lại lưới là $O(i)$, nên chi phí kỳ vọng cho mỗi điểm là $O(1)$. Tổng cộng với $n$ điểm, thuật toán có kỳ vọng $O(n)$.

## Bài tập

-   [UVa 10245 "The Closest Pair Problem"\[Dễ\]](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1186)
-   [SPOJ #8725 CLOPPAIR "Closest Point Pair"\[Dễ\]](https://www.spoj.com/problems/CLOPPAIR/)
-   [CODEFORCES Team Olympiad Saratov - 2011 "Minimum amount"\[Trung bình\]](http://codeforces.com/contest/120/problem/J)
-   [SPOJ #7029 CLOSEST "Closest Triple"\[Trung bình\]](https://www.spoj.com/problems/CLOSEST/)
-   [Google Code Jam 2009 Final "Min Perimeter"\[Trung bình\]](https://github.com/google/coding-competitions-archive/blob/main/codejam/2009/world_finals/min_perimeter/statement.pdf)

## Tài liệu tham khảo & Đọc thêm

**Phần thuật toán chia để trị trong trang này chủ yếu dịch từ bài [Нахождение пары ближайших точек](http://e-maxx.ru/algo/nearest_points) và bản tiếng Anh [Finding the nearest pair of points](https://github.com/e-maxx-eng/e-maxx-eng/blob/master/src/geometry/nearest_points.md). Bản tiếng Nga: Public Domain + Leave a Link; bản tiếng Anh: CC-BY-SA 4.0.**

[Chuyên mục Zhihu: Tính toán hình học - Bài toán cặp điểm gần nhất](https://zhuanlan.zhihu.com/p/74905629)
