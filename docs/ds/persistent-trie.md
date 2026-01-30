## Giới thiệu

Phương pháp Trie bền vững tương tự như Cây phân đoạn bền vững, tức là mỗi lần chỉ sửa đổi các nút được thêm hoặc giá trị bị thay đổi, trong khi giữ lại các nút không thay đổi, nối các cạnh dựa trên phiên bản trước đó, sao cho cuối cùng mỗi phiên bản của Trie gốc đều hoàn chỉnh và chứa đầy đủ thông tin.

Trong hầu hết các bài toán Trie bền vững, Trie xuất hiện dưới dạng [01-Trie](../string/trie.md#maintaining-xor-extremum).

??? note "Ví dụ [Maximum XOR Sum](https://www.luogu.com.cn/problem/P4735)"
    Cho một mảng $a$ có độ dài $n$, thực hiện các thao tác sau:
    
    1.  Thêm một số $x$ vào cuối mảng, độ dài mảng $n$ tăng thêm $1$.
    2.  Cho khoảng truy vấn $[l,r]$ và một giá trị $k$, tìm giá trị lớn nhất của $k \oplus \bigoplus^{n}_{i=p} a_i$ với $l\le p\le r$.

## Quá trình

Giá trị cần tìm có vẻ hơi phức tạp, sử dụng phương pháp xử lý XOR liên tiếp thường dùng, ký hiệu $s_x=\bigoplus_{i=1}^x a_i$, thì biểu thức ban đầu tương đương với $s_{p-1}\oplus s_n\oplus k$. Nhận thấy rằng $s_n \oplus k$ là cố định trong quá trình truy vấn, bài toán truy vấn chuyển thành tìm giá trị lớn nhất khi XOR với một giá trị cố định ($s_n\oplus k$) trong khoảng $[l-1,r-1]$.

Tiếp tục theo ý tưởng tương tự như Cây phân đoạn bền vững, hãy xem xét mỗi lần truy vấn là truy vấn trên toàn bộ khoảng. Chúng ta chỉ cần xây dựng một Trie cho khoảng này, thêm mỗi số trong khoảng này vào Trie, khi truy vấn thì cố gắng nhảy đến nhánh có bit khác với bit hiện tại.

Để truy vấn khoảng, chỉ cần sử dụng ý tưởng tổng tiền tố và hiệu, dùng hai cây Trie tiền tố (tức là hai phiên bản lịch sử thêm số theo thứ tự) trừ đi nhau để có được cây Trie của khoảng đó. Sử dụng ý tưởng cấp phát động, không thêm các điểm chưa tính toán để giảm dung lượng bộ nhớ.

```cpp
--8<-- "docs/ds/code/persistent-trie/persistent-trie_1.cpp"
```