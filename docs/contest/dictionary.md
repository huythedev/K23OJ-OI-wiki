Tiền đề: [phân khối](../ds/decompose.md).

“Đánh bảng” kiểu **nguồn sơ khai** là: trong thi, tính trước đáp án cho mọi input có thể, lưu lại, rồi dùng mảng trong code để tra và in ra.

Kỹ thuật này chỉ phù hợp với miền giá trị nhỏ (ví dụ input chỉ có một số và phạm vi rất nhỏ), nếu không thì có thể khiến code quá dài, MLE, hoặc thời gian tiền xử lý quá lâu.

???+ note "Ví dụ"
    Đặt $f(x)$ là số bit $1$ trong biểu diễn nhị phân của số nguyên $x$. Cho $n$ nguyên dương ($n\leq 10^9$), tính $\sum_{i=1}^n f^2(i)$.

Nếu với mỗi $n$ đều output $f(n)$, ngoài nguy cơ MLE còn có thể vượt giới hạn độ dài code khiến không biên dịch được.

Ta tối ưu bảng này. Dùng ý tưởng [phân khối](../ds/decompose.md), chọn bước $m$ (phụ thuộc độ dài code), với khối thứ $i$ tính:

$$
\sum_{k=\frac{n}{m}(i-1)+1}^{\frac{ni}{m}} f^2(k)
$$

Sau đó khi trả lời dùng phân khối: khối đầy đủ dùng giá trị tiền xử lý, phần còn lại vét cạn.

Nhìn chung, dạng bài này tính một giá trị $f(x)$ rất nhanh, nhưng cần cộng (hoặc nhân/ghép nhanh) nhiều giá trị nên enumerate sẽ TLE; khi không có cách standard, “đánh bảng phân đoạn” là lựa chọn tốt.

???+ note "Lưu ý"
    Nếu số mũ không cố định nhưng phạm vi nhỏ, cũng có thể đánh bảng.

### Ví dụ

[「BZOJ 3798」特殊的质数](https://hydro.ac/p/bzoj-P3798)：đếm số nguyên tố trong $[l,r]$ biểu diễn được dưới dạng tổng bình phương hai số nguyên.

[「Luogu P1822」魔法指纹](https://www.luogu.com.cn/problem/P1822)
