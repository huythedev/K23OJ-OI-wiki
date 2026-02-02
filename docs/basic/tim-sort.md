Trang này giới thiệu Timsort, một thuật toán sắp xếp lai, ổn định.

## Giới thiệu

Timsort do Tim Peters (lõi phát triển Python) thiết kế năm 2002 và được ứng dụng trong Python. Nó kết hợp lợi thế của insertion sort và merge sort, tận dụng các đoạn con đã có trật tự trong dữ liệu, nên đặc biệt hiệu quả với dữ liệu chứa nhiều đoạn đã có thứ tự. Kể từ Python 2.3, Timsort là thuật toán sắp xếp mặc định của thư viện chuẩn Python và cũng được dùng ở nhiều môi trường khác (ví dụ Java SE 7 cho mảng đối tượng).

## Các bước

Ý tưởng chính của Timsort là nhận dạng và tận dụng độ có trật tự trong dữ liệu; các bước chính:

1.  Nhận dạng Run: quét mảng để tìm các đoạn con đã có thứ tự (Run).
2.  Mở rộng Run: nếu độ dài Run < MIN_RUN thì dùng insertion sort mở rộng Run.
3.  Gộp Run: duy trì một ngăn xếp các Run và theo luật hợp nhất để ghép các Run thành đoạn lớn hơn.

### Nhận dạng Run

Timsort quét từ trái sang phải để nhận Run liên tiếp:
-  Run tăng: nếu phần tử sau >= phần tử trước thì tiếp tục mở rộng.
-  Run giảm: nếu phần tử sau < phần tử trước thì tiếp tục mở rộng, sau đó đảo chiều Run thành tăng.

### Mở rộng Run

Để tối ưu cho dữ liệu nhỏ, Timsort có một ngưỡng độ dài Run tối thiểu `MIN_RUN` (thường tính theo độ dài mảng, nằm trong khoảng 32–64).

-  Nếu Run đã >= MIN_RUN thì push vào ngăn xếp.
-  Nếu Run < MIN_RUN thì dùng binary insertion sort để mở rộng Run đến MIN_RUN rồi push.

### Gộp Run

Timsort quản lý việc gộp Run bằng một ngăn xếp và tuân thủ các quy tắc gộp đặc biệt để giữ cân bằng và tính ổn định.

#### Quy tắc gộp

Timsort là thuật toán ổn định; khi gộp chỉ gộp các Run kề nhau để không làm xáo trộn thứ tự tương đối của các phần tử bằng nhau.

Để đảm bảo cân bằng, trước mỗi lần gộp thuật toán kiểm tra ba Run trên đỉnh ngăn xếp X, Y, Z và yêu cầu:
-  Điều kiện 1: len(Z) > len(Y) + len(X)
-  Điều kiện 2: len(Y) > len(X)

Nếu ba Run trên đỉnh không thỏa, Timsort sẽ gộp Y với X hoặc Z (theo phần nhỏ hơn) rồi tiếp tục kiểm tra lại. Khi thỏa, tiếp tục push Run mới và lặp.

![Merge Rules](./images/tim-sort-1.png)

#### Tối ưu khi gộp

Để giảm công việc khi gộp hai Run có độ dài chênh lệch, Timsort dùng binary search để xác định chính xác phạm vi cần gộp, chỉ xử lý phần tử cần di chuyển.

Ngoài ra, Timsort sao chép Run ngắn hơn vào bộ đệm tạm rồi trộn dần trở lại mảng chính để giảm thao tác di chuyển.

Ví dụ: Run A = [1,2,3,6,10], Run B = [4,5,7,9,12,14,17]
-  Bằng binary search, xác định vị trí chèn của 4 trong A (vị trí thứ 4), vị trí chèn của 10 trong B (vị trí thứ 5).
-  Do đó phần A trước 3 phần tử và phần B sau 3 phần tử đã đúng vị trí; chỉ cần gộp A phần [6,10] với B phần [4,5,7,9] như hình:

![Timsort Merge](./images/tim-sort-2.apng)

#### Chế độ tăng tốc (Galloping Mode)

Khi một phía liên tục “thắng” nhiều lần trong quá trình gộp, Timsort bật chế độ galloping để tăng tốc:
1.  Dùng exponential search (bước 1,2,4,8,…) trên phía đang thắng để tìm khoảng chứa vị trí chèn.
2.  Sau đó dùng binary search trong khoảng đó để xác định chính xác.

Timsort giữ tham số `Min_Gallop` (mặc định 7) và điều chỉnh động: nếu galloping hiệu quả giảm `Min_Gallop` đi 1, nếu không hiệu quả tăng `Min_Gallop` đi 1. Nhờ vậy, với dữ liệu gần như đã có thứ tự Timsort có thể đạt gần $O(n)$; với dữ liệu ngẫu nhiên, nó sẽ nghiêng về merge thông thường giữ $O(n\log n)$.

## Độ phức tạp

Độ phức tạp của Timsort phụ thuộc vào mức độ trật tự của dữ liệu:

-  Tốt nhất: $O(n)$ khi dữ liệu đã hoặc gần như đã sắp xếp.
-  Tệ nhất: $O(n\log n)$ khi dữ liệu hoàn toàn ngẫu nhiên (run độ dài ~1).

Chứng minh tóm tắt:

-  Nhận dạng và mở rộng Run: quét mảng một lần => $O(n)$.
-  Gộp Run: số Run ở worst-case là $O(n)$ (với MIN_RUN là hằng số), số lần gộp tương đương $O(\log n)$, mỗi lần gộp tốn $O(n)$ tổng => $O(n\log n)$.

Không gian: Timsort cần thêm $O(n)$ bộ đệm và ngăn xếp Run, nên không gian phụ là $O(n)$.

## Cài đặt

???+ note "Ví dụ: giả mã"
    $$
    \begin{array}{ll}
    1 & nRemaining \gets \text{độ dài mảng} \\
    2 & minRun \gets \text{chọn giá trị MinRun phù hợp}(nRemaining) \\
    3 & startIndex \gets 0 \\
    4 & \textbf{while } nRemaining > 0 \ \textbf{do} \\
    5 & \qquad runLength \gets \text{nhận dạng Run }(array, startIndex, nRemaining) \\
    6 & \qquad \textbf{if } runLength < minRun \ \textbf{then} \\
    7 & \qquad \qquad extendLength \gets \min(minRun, nRemaining) \\
    8 & \qquad \qquad \text{dùng insertion sort chèn phần tử để mở rộng } [startIndex, startIndex + extendLength - 1]\\
    9 & \qquad \qquad runLength \gets extendLength \\
    10 & \qquad \textbf{end if} \\
    11 & \qquad \text{đẩy Run } (startIndex, runLength) \text{ vào ngăn xếp} \\
    12 & \qquad \text{gọi } \text{mergeCollapse(ngăn xếp)} \ \text{để kiểm tra và gộp các Run} \\
    13 & \qquad startIndex \gets startIndex + runLength \\
    14 & \qquad nRemaining \gets nRemaining - runLength \\
    15 & \textbf{end while} \\
    16 & \textbf{gọi } \text{mergeForceCollapse(ngăn xếp)} \ \text{để gộp hết các Run còn lại} \\
    \end{array}
    $$

## Tài liệu tham khảo

1.  [Timsort](https://en.wikipedia.org/wiki/Timsort)
2.  [On the Worst-Case Complexity of TimSort](https://drops.dagstuhl.de/opus/volltexte/2018/9467/pdf/LIPIcs-ESA-2018-4.pdf)
3.  [Original Explanation by Tim Peters](https://github.com/python/cpython/blob/main/Objects/listsort.txt)
4.  [Java 实现](https://cs.android.com/android/platform/superproject/main/+/main:libcore/ojluni/src/main/java/java/util/TimSort.java)
5.  [C 语言实现](https://github.com/python/cpython/blob/main/Objects/listobject.c)
