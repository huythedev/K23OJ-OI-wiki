???+ warning "Nhắc nhở"
    Trang này **không** nói về [**Sắp xếp đếm (Counting sort)**](./counting-sort.md).

Trang này sẽ giới thiệu ngắn gọn về **sắp xếp theo cơ số** (Radix sort).

## Định nghĩa

**Sắp xếp theo cơ số** (tiếng Anh: Radix sort) là một thuật toán sắp xếp không dựa trên so sánh, ban đầu dùng để sắp xếp thẻ bài. Radix sort tách mỗi phần tử thành $k$ khóa (key), lần lượt sắp xếp theo từng khóa để hoàn thành việc sắp xếp toàn bộ.

Nếu sắp xếp từ khóa thứ 1 đến khóa thứ $k$, gọi là **MSD** (Most Significant Digit first);  
Nếu sắp xếp từ khóa thứ $k$ đến khóa thứ 1, gọi là **LSD** (Least Significant Digit first).

## So sánh phần tử có k khóa

Gọi $a_i$ là khóa thứ $i$ của phần tử $a$.

Giả sử mỗi phần tử có $k$ khóa, với hai phần tử $a$ và $b$, cách so sánh mặc định là:

-   So sánh khóa thứ 1: $a_1$ với $b_1$, nếu $a_1 < b_1$ thì $a < b$, nếu $a_1 > b_1$ thì $a > b$, nếu $a_1 = b_1$ thì tiếp tục;
-   So sánh khóa thứ 2: $a_2$ với $b_2$, ...;
-   ...
-   So sánh khóa thứ $k$: $a_k$ với $b_k$, nếu $a_k < b_k$ thì $a < b$, nếu $a_k > b_k$ thì $a > b$, nếu $a_k = b_k$ thì $a = b$.

Ví dụ:

-   So sánh số tự nhiên: căn chỉnh các chữ số, mỗi chữ số là một khóa;
-   So sánh xâu ký tự theo từ điển: mỗi ký tự là một khóa;
-   `std::pair` và `std::tuple` trong C++ cũng so sánh theo thứ tự từng trường như trên.

## Sắp xếp theo cơ số MSD

Dựa trên cách so sánh nhiều khóa, ta có thể: sắp xếp theo khóa 1, rồi với các phần tử có cùng khóa 1, tiếp tục sắp xếp theo khóa 2, ...  
Vì đi từ khóa 1 đến khóa $k$, thuật toán này gọi là **MSD** (Most Significant Digit first).

### Quy trình thuật toán

Tách mỗi phần tử thành $k$ khóa, trước tiên sắp xếp ổn định theo khóa 1, sau đó với mỗi nhóm phần tử có cùng khóa 1, tiếp tục sắp xếp ổn định theo khóa 2 (đệ quy), ... cuối cùng sắp xếp theo khóa $k$.

Thông thường, ta dùng thuật toán ổn định (thường là sắp xếp đếm) cho mỗi bước.

Tính đúng đắn dựa trên quy tắc so sánh nhiều khóa ở trên.

### Code mẫu

#### Sắp xếp số tự nhiên

Dưới đây là code C++ dùng MSD radix sort để sắp xếp `unsigned int`, có thể điều chỉnh $W$ và $\log_2 W$ (nên chọn $\log_2 W$ là $2^k$ để tối ưu bitwise).

??? example "Code mẫu"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/radix-sort.md
    ...existing code...
    ```

#### Sắp xếp xâu ký tự

Dưới đây là code C++ dùng MSD radix sort để sắp xếp [xâu ký tự kết thúc bằng ký tự rỗng](https://zh.cppreference.com/w/cpp/string/byte) theo từ điển:

??? example "Code mẫu"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/radix-sort.md
    ...existing code...
    ```

Vì so sánh hai xâu có thể tốn $O(n)$, nên với bài toán sắp xếp xâu, MSD radix sort thường nhanh hơn các thuật toán dựa trên so sánh.

### Quan hệ với sắp xếp theo thùng

Xem thêm: [Sắp xếp theo thùng (Bucket sort)](./bucket-sort.md)

Bucket sort cần một thuật toán sắp xếp cho từng thùng. Thực ra, có thể tiếp tục dùng bucket sort cho từng thùng, lặp lại cho đến khi mỗi thùng chỉ còn tối đa 1 phần tử.

Vì vậy, MSD radix sort có thể hiểu là "bucket sort lồng bucket sort".

Một tối ưu thường dùng: nếu một thùng có số phần tử $\leq B$ (với $B$ là hằng số tự chọn), thì dùng insertion sort cho nhanh, giảm số lần đệ quy.

## Sắp xếp theo cơ số LSD

MSD radix sort đi từ khóa 1 đến khóa $k$, cần đệ quy hoặc lặp lại, hằng số thời gian lớn, không tiện cho số tự nhiên.

Nếu đảo ngược: đi từ khóa $k$ về khóa 1, ta có **LSD** (Least Significant Digit first) radix sort, không cần đệ quy.

### Quy trình thuật toán

Tách mỗi phần tử thành $k$ khóa, sau đó lần lượt sắp xếp ổn định theo khóa $k$, rồi khóa $k-1$, ..., cuối cùng là khóa 1.

![Ví dụ LSD radix sort](images/radix-sort-1.png "Ví dụ LSD radix sort")

LSD radix sort cũng cần một thuật toán ổn định cho mỗi bước (thường là sắp xếp đếm).

Tính đúng đắn có thể tham khảo [CLRS 8.3-3](https://walkccc.github.io/CLRS/Chap08/8.3/#83-3) hoặc giải thích dưới đây:

### Giải thích tính đúng đắn

Nhắc lại quy tắc so sánh nhiều khóa:

-   Nếu muốn so sánh $a_1$ và $b_1$ quyết định thứ tự $a, b$, cần biết trước kết quả so sánh $a_2, b_2$ (trường hợp $a_1 = b_1$);
-   ...;
-   So sánh $a_{k-1}, b_{k-1}$ cần biết trước kết quả $a_k, b_k$;
-   So sánh $a_k, b_k$ có thể quyết định ngay.

Nếu làm ngược lại:

-   So sánh $a_k, b_k$ trước;
-   Biết kết quả $a_k, b_k$ rồi mới so $a_{k-1}, b_{k-1}$;
-   ...;
-   Cuối cùng so $a_1, b_1$.

Mỗi lần sắp xếp lại theo khóa, ta sẽ có thứ tự đúng.

### Giả mã

$$
\begin{array}{ll}
1 & \textbf{Input. } \text{Một mảng } A \text{ gồm }n\text{ phần tử, mỗi phần tử có }k\text{ khóa.}\\
2 & \textbf{Output. } \text{Mảng }A\text{ được sắp xếp tăng dần, ổn định.} \\
3 & \textbf{Method. }  \\
4 & \textbf{for }i\gets k\textbf{ down to }1\\
5 & \qquad\text{Sắp xếp }A\text{ theo khóa thứ }i\text{ (ổn định)}
\end{array}
$$

### Code mẫu

Dưới đây là code LSD radix sort cho phần tử có $k$ khóa.

??? example "Code mẫu"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/radix-sort.md
    ...existing code...
    ```

Thực tế không nhất thiết phải duyệt từ sau về trước, chỉ cần xử lý mảng `cnt` tương đương với `std::exclusive_scan` là được.

???+ note "Bài tập [洛谷 P1177【模板】快速排序](https://www.luogu.com.cn/problem/P1177)"
    Cho $n$ số nguyên dương, in ra theo thứ tự tăng dần.

    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/radix-sort.md
    ...existing code...
    ```

## Tính chất

### Tính ổn định

Nếu thuật toán ổn định được dùng cho mỗi bước, thì cả MSD và LSD radix sort đều là **ổn định**.

### Độ phức tạp thời gian

Thông thường, radix sort nhanh hơn các thuật toán dựa trên so sánh (như quicksort). Tuy nhiên, do cần bộ nhớ phụ, khi bộ nhớ hạn chế thì các thuật toán in-place như quicksort có thể phù hợp hơn.[^ref1]

Nếu mỗi khóa có giá trị nhỏ, có thể dùng [sắp xếp đếm](./counting-sort.md) cho mỗi bước, khi đó độ phức tạp là $O(kn+\sum\limits_{i=1}^k w_i)$, với $w_i$ là miền giá trị của khóa $i$. Nếu khóa có miền giá trị lớn, có thể dùng sắp xếp so sánh $O(nk\log n)$.

### Độ phức tạp bộ nhớ

Cả MSD và LSD radix sort đều có độ phức tạp bộ nhớ $O(k+n)$.

## Tài liệu tham khảo & chú thích

[^ref1]: Thomas H. Cormen, Charles E. Leiserson, Ronald L. Rivest, and Clifford Stein.*Introduction to Algorithms*(3rd ed.). MIT Press and McGraw-Hill, 2009. ISBN 978-0-262-03384-8. "8.3 Radix sort", pp. 199.
