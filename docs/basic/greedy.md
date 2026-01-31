Trang này sẽ giới thiệu ngắn gọn về **thuật toán tham lam** (greedy algorithm).

## Dẫn nhập

**Thuật toán tham lam** (tiếng Anh: greedy algorithm) là cách mô phỏng quá trình ra quyết định của một "người tham lam" trên máy tính. Người này luôn chọn phương án tốt nhất theo một tiêu chí nào đó ở mỗi bước, chỉ nhìn vào lợi ích trước mắt mà không quan tâm đến ảnh hưởng về sau.

Dễ thấy, không phải lúc nào tham lam cũng cho đáp án tối ưu, nên khi dùng cần đảm bảo có thể chứng minh được tính đúng đắn của thuật toán.

## Giải thích

### Phạm vi áp dụng

Thuật toán tham lam đặc biệt hiệu quả với các bài toán có **tính chất tối ưu con** (optimal substructure). Nghĩa là bài toán có thể chia nhỏ thành các bài toán con, và đáp án tối ưu của bài toán con có thể xây dựng thành đáp án tối ưu của bài toán gốc.[^ref1]

### Chứng minh

Có hai cách chứng minh đúng đắn cho thuật toán tham lam: phản chứng và quy nạp. Thông thường, một bài chỉ cần một trong hai cách.

1.  **Phản chứng:** Nếu hoán đổi hai phần tử bất kỳ (hoặc hai phần tử kề nhau) mà đáp án không tốt hơn, thì có thể kết luận phương án hiện tại là tối ưu.
2.  **Quy nạp:** Tìm đáp án tối ưu cho trường hợp biên (ví dụ $n=1$), sau đó chứng minh với mọi $n$, đáp án tối ưu $F_{n+1}$ có thể xây dựng từ $F_n$.

## Các điểm cần lưu ý

### Các dạng bài thường gặp

Với các bài dưới mức khó nâng cao, hai dạng tham lam phổ biến nhất là:

-   "Sắp xếp XXX theo một thứ tự nào đó, rồi chọn theo thứ tự (ví dụ từ nhỏ đến lớn)."
-   "Mỗi lần lấy phần tử lớn nhất/nhỏ nhất trong XXX, rồi cập nhật XXX." (đôi khi có thể tối ưu bằng cách dùng hàng đợi ưu tiên)

Khác biệt là một dạng là **offline** (xử lý trước rồi chọn), một dạng là **online** (xử lý và chọn đồng thời).

### Dạng sắp xếp

Thường gặp khi đầu vào là mảng có một hoặc hai trọng số, giải bằng cách sắp xếp rồi duyệt mô phỏng để tìm giá trị tối ưu.

### Dạng "hối hận"

Ý tưởng là luôn nhận mọi lựa chọn, sau đó so sánh: nếu lựa chọn hiện tại không còn tối ưu thì "hối hận", loại bỏ nó; nếu vẫn tối ưu thì giữ lại. Lặp lại quá trình này.

## Phân biệt

### So với quy hoạch động

Khác biệt giữa tham lam và quy hoạch động là: tham lam chọn phương án tốt nhất cho từng bài toán con mà không quay lại; quy hoạch động lưu lại kết quả các bài toán con và có thể quay lại để chọn phương án tốt hơn.

## Phân tích ví dụ

### Ví dụ về phương pháp hoán đổi cặp kề

???+ note "[NOIP 2012 Trò chơi của nhà vua](https://www.luogu.com.cn/problem/P1080)"
    Nhân dịp quốc khánh nước H, nhà vua mời $n$ đại thần chơi một trò chơi. Mỗi đại thần ghi một số lên tay trái và tay phải, nhà vua cũng ghi hai số lên hai tay. Sau đó, $n$ đại thần xếp thành hàng, nhà vua đứng đầu hàng. Mỗi đại thần nhận được số vàng bằng tích các số trên tay trái của tất cả người đứng trước chia cho số trên tay phải của mình, làm tròn xuống.

    Nhà vua muốn tối thiểu hóa số vàng lớn nhất mà một đại thần nhận được, hãy giúp ông sắp xếp lại thứ tự các đại thần (nhà vua luôn đứng đầu).

??? note "Hướng dẫn giải"
    Gọi số trên tay trái/phải của đại thần thứ $i$ sau khi sắp xếp là $a_i, b_i$. Dùng phương pháp hoán đổi cặp kề để tìm chiến lược tham lam.

    Gọi $s$ là tích các $a_i$ của những người đứng trước đại thần $i$, khi đó đại thần $i$ nhận được $\dfrac{s}{b_i}$ vàng, đại thần $i+1$ nhận được $\dfrac{s \cdot a_i}{b_{i+1}}$.

    Nếu đổi chỗ $i$ và $i+1$, đại thần $i$ nhận $\dfrac{s}{b_{i+1}}$, đại thần $i+1$ nhận $\dfrac{s \cdot a_{i+1}}{b_i}$.

    Đổi chỗ là tốt hơn khi

    $$
    \max \left(\dfrac{s} {b_i}, \dfrac{s \cdot a_i} {b_{i+1}}\right)  < \max \left(\dfrac{s} {b_{i+1}}, \dfrac{s \cdot a_{i+1}} {b_i}\right)
    $$

    Rút gọn $s$:

    $$
    \max \left(\dfrac{1} {b_i}, \dfrac{a_i} {b_{i+1}}\right)  < \max \left(\dfrac{1} {b_{i+1}}, \dfrac{a_{i+1}} {b_i}\right)
    $$

    Đưa về dạng nguyên:

    $$
    \max (b_{i+1}, a_i\cdot b_i)  < \max (b_i, a_{i+1}\cdot b_{i+1})
    $$

    Khi cài đặt, lưu hai số vào struct và nạp chồng toán tử:

    ```cpp
    struct uv {
      int a, b;

      bool operator<(const uv &x) const {
        return max(x.b, a * b) < max(b, x.a * x.b);
      }
    };
    ```

### Ví dụ về phương pháp "hối hận"

???+ note "[「USACO09OPEN」Lập lịch công việc Work Scheduling](https://www.luogu.com.cn/problem/P2949)"
    John có $10^9$ đơn vị thời gian, mỗi thời điểm có thể chọn một trong $N$ công việc ($1 \leq N \leq 10^5$). Công việc $i$ có hạn chót $D_i(1 \leq D_i \leq 10^9)$, hoàn thành được $P_i( 1\leq P_i\leq 10^9 )$ tiền. Hỏi John có thể kiếm tối đa bao nhiêu tiền.

??? note "Hướng dẫn giải"
    1.  Giả sử làm tất cả công việc, sắp xếp các công việc theo hạn chót rồi đưa vào hàng đợi;
    2.  Khi xét công việc thứ `i`, nếu hạn chót hợp lệ thì so với phần tử nhỏ nhất trong hàng đợi, nếu công việc hiện tại có lợi hơn thì "hối hận", thay thế phần tử nhỏ nhất.
        Dùng hàng đợi ưu tiên (heap nhỏ) để duy trì phần tử nhỏ nhất.
    3.  Nếu `a[i].d<=q.size()`, hiểu là trong $[0, a[i].d]$ chỉ làm được $a[i].d$ việc, nếu đã có $q.size()>=a[i].d$ thì phải thay thế nếu có việc tốt hơn.

??? note "Code tham khảo"
    === "C++"
        ```cpp
        --8<-- "docs/basic/code/greedy/greedy_1.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/basic/code/greedy/greedy_1.py"
        ```

??? note "Phân tích độ phức tạp"
    -   Bộ nhớ: dùng mảng $a$ và heap tối đa $n$ phần tử, $O(n)$.
    -   Thời gian: `std::sort` $O(n\log n)$, duy trì heap $O(n\log n)$, tổng $O(n\log n)$.

## Bài tập

-   [P1209\[USACO1.3\] Sửa chuồng bò Barn Repair - 洛谷](https://www.luogu.com.cn/problem/P1209)
-   [P2123 Trò chơi của hoàng hậu - 洛谷](https://www.luogu.com.cn/problem/P2123)
-   [Các bài LeetCode gắn nhãn greedy](https://leetcode-cn.com/tag/greedy/)

## Tài liệu tham khảo & chú thích

[^ref1]: [Thuật toán tham lam - Wikipedia tiếng Trung](https://zh.wikipedia.org/wiki/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95)
