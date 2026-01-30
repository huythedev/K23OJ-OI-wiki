tác giả: Ir1d, HeRaNO, Xeonacid

## Giới thiệu

Thực ra, chia khối (block decomposition) là một tư tưởng, chứ không phải là một cấu trúc dữ liệu.

Từ NOIP đến NOI đến IOI, tư tưởng chia khối xuất hiện ở các mức độ khó khác nhau.

Tư tưởng cơ bản của chia khối là thông qua việc phân chia hợp lý dữ liệu gốc, và tiền xử lý một phần thông tin trên mỗi khối đã chia, từ đó đạt được độ phức tạp thời gian tốt hơn so với các thuật toán vét cạn thông thường.

Độ phức tạp thời gian của chia khối chủ yếu phụ thuộc vào độ dài khối, thông thường có thể tìm ra độ dài khối tối ưu cho một bài toán nhất định và độ phức tạp thời gian tương ứng bằng cách sử dụng bất đẳng thức trung bình.

Chia khối là một tư tưởng rất linh hoạt, ưu điểm so với mảng Fenwick (BIT) và cây đoạn là tính phổ dụng tốt hơn, có thể duy trì nhiều thông tin mà mảng Fenwick và cây đoạn không thể duy trì.

Tất nhiên, nhược điểm của chia khối là độ phức tạp tiệm cận không tốt bằng cây đoạn và mảng Fenwick.

Tuy nhiên, trong hầu hết các bài toán, chia khối vẫn là một lựa chọn tốt để giải quyết các bài toán này.

Dưới đây là một vài ví dụ.

## Tổng đoạn

??? note "Bài toán [LibreOJ 6280 Nhập môn Chia khối Dãy số 4](https://loj.ac/problem/6280)"
    Cho một dãy $\{a_i\}$ có độ dài $n$, cần thực hiện $n$ thao tác. Thao tác chia làm hai loại:
    
    1.  Cộng $x$ vào tất cả các số từ $a_l$ đến $a_r$;
    2.  Tính $\sum_{i=l}^r a_i$.
    
        $1 \leq n \leq 5 \times 10^4$

Chúng ta chia dãy thành các khối, mỗi khối có $s$ phần tử, và ghi lại tổng đoạn của mỗi khối là $b_i$.

$$
\underbrace{a_1, a_2, \ldots, a_s}_{b_1}, \underbrace{a_{s+1}, \ldots, a_{2s}}_{b_2}, \dots, \underbrace{a_{(s-1) \times s+1}, \dots, a_n}_{b_{\frac{n}{s}}}
$$

Khối cuối cùng có thể không đầy đủ (vì $n$ rất có thể không phải là bội số của $s$), nhưng điều này không ảnh hưởng lớn đến thảo luận của chúng ta.

Trước tiên xem xét thao tác truy vấn:

-   Nếu $l$ và $r$ nằm trong cùng một khối, trực tiếp tính tổng vét cạn, vì độ dài khối là $s$, nên độ phức tạp xấu nhất là $O(s)$.
-   Nếu $l$ và $r$ không nằm trong cùng một khối, thì kết quả được tạo thành từ ba phần: khối không đầy đủ bắt đầu từ $l$, các khối đầy đủ ở giữa, và khối không đầy đủ kết thúc tại $r$. Đối với các khối không đầy đủ, vẫn sử dụng phương pháp tính vét cạn ở trên; đối với các khối đầy đủ, trực tiếp sử dụng $b_i$ đã tính để tính tổng. Trong trường hợp này, độ phức tạp xấu nhất là $O(\dfrac{n}{s}+s)$.

Tiếp theo là thao tác sửa đổi:

-   Nếu $l$ và $r$ nằm trong cùng một khối, trực tiếp sửa đổi vét cạn, vì độ dài khối là $s$, nên độ phức tạp xấu nhất là $O(s)$.
-   Nếu $l$ và $r$ không nằm trong cùng một khối, cần sửa đổi ba phần: khối không đầy đủ bắt đầu từ $l$, các khối đầy đủ ở giữa, và khối không đầy đủ kết thúc tại $r$. Đối với các khối không đầy đủ, vẫn là sửa đổi vét cạn giá trị của từng phần tử (đừng quên cập nhật tổng đoạn $b_i$), đối với các khối đầy đủ, trực tiếp sửa đổi $b_i$. Trong trường hợp này, độ phức tạp xấu nhất vẫn là $O(\dfrac{n}{s}+s)$.

Theo bất đẳng thức trung bình, khi $\dfrac{n}{s}=s$, tức là $s=\sqrt n$, độ phức tạp thời gian cho mỗi thao tác là tối ưu, là $O(\sqrt n)$.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/ds/code/decompose/decompose_1.cpp"
    ```

## Tổng đoạn 2

Cách tiếp cận trước có độ phức tạp $\Omega(1), O(\sqrt{n})$.

Ở đây chúng ta giới thiệu một thuật toán $O(\sqrt{n}) - O(1)$.

Để truy vấn $O(1)$, chúng ta có thể duy trì các loại tổng tiền tố khác nhau.

Tuy nhiên, khi có sửa đổi, việc duy trì không thuận tiện, chỉ có thể duy trì tổng tiền tố trong phạm vi từng khối.

Và tổng tiền tố của cả khối như một đơn vị.

Mỗi lần sửa đổi là $O(T+\frac{n}{T})$.

Truy vấn: liên quan đến ba phần, mỗi phần đều có thể nhận được trực tiếp thông qua tổng tiền tố, độ phức tạp thời gian là $O(1)$.

## Chia khối cho truy vấn

Cùng một bài toán, bây giờ độ dài dãy là $n$, có $m$ thao tác.

Nếu số lượng thao tác tương đối ít, chúng ta có thể ghi lại các thao tác, và cộng tác động của các thao tác này vào lúc truy vấn.

Giả sử tối đa ghi nhớ $T$ thao tác, thì sửa đổi $O(1)$, truy vấn $O(T)$.

Sau $T$ thao tác, tính toán lại tổng tiền tố, $O(n)$.

Độ phức tạp tổng: $O(mT+n\frac{m}{T})$.

Khi $T=\sqrt{n}$, độ phức tạp tổng là $O(m \sqrt{n})$.

### Các bài toán khác

Tư tưởng chia khối cũng có thể được áp dụng cho các bài toán liên quan đến số nguyên khác: tìm số lượng phần tử bằng 0, tìm phần tử khác 0 đầu tiên, tính số lượng phần tử thỏa mãn một thuộc tính nào đó, v.v.

Còn có một số bài toán có thể được giải quyết bằng chia khối, ví dụ như duy trì một tập hợp cho phép thêm hoặc xóa các số, kiểm tra xem một số có thuộc tập hợp này hay không, và tìm số lớn thứ $k$. Để giải quyết bài toán này, cần lưu trữ các số theo thứ tự tăng dần và chia thành nhiều khối, mỗi khối chứa $\sqrt{n}$ số. Mỗi lần thêm hoặc xóa một số, cần phải phân chia lại khối bằng cách di chuyển các số ở ranh giới giữa các khối liền kề.

Một thuật toán offline nổi tiếng [Thuật toán Mo](../misc/mo-algo.md), cũng được thực hiện dựa trên tư tưởng chia khối.

## Bài tập thực hành

-   [UVa - 12003 - Array Transformer](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3154)
-   [UVa - 11990 Dynamic Inversion](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=3141)
-   [SPOJ - Give Away](http://www.spoj.com/problems/GIVEAWAY/)
-   [Codeforces - Till I Collapse](http://codeforces.com/contest/786/problem/C)
-   [Codeforces - Destiny](http://codeforces.com/contest/840/problem/D)
-   [Codeforces - Holes](http://codeforces.com/contest/13/problem/E)
-   [Codeforces - XOR and Favorite Number](https://codeforces.com/problemset/problem/617/E)
-   [Codeforces - Powerful array](http://codeforces.com/problemset/problem/86/D)
-   [SPOJ - DQUERY](https://www.spoj.com/problems/DQUERY)

    **Trang này chủ yếu dịch từ bài viết [Sqrt-декомпозиция](http://e-maxx.ru/algo/sqrt_decomposition) và bản dịch tiếng Anh của nó [Sqrt Decomposition](https://cp-algorithms.com/data_structures/sqrt_decomposition.html). Bản tiếng Nga có giấy phép Public Domain + Leave a Link; bản tiếng Anh có giấy phép CC-BY-SA 4.0.**