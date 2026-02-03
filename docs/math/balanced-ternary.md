## Định nghĩa

Hệ tam phân cân bằng (balanced ternary), còn gọi là tam phân đối xứng. Đây là một **hệ đếm** không quá phổ biến.

Trong tam phân chuẩn, các chữ số được tạo bởi `0`,`1`,`2`, còn trong tam phân cân bằng, các chữ số là `-1`,`0`,`1`. Cơ số vẫn là `3` (vì có ba giá trị khả dĩ). Do việc viết `-1` bất tiện, ta dùng chữ `Z` để thay cho `-1`.

## Giải thích

Một vài ví dụ:

| Thập phân | Tam phân cân bằng | Thập phân | Tam phân cân bằng |
| --- | ----- | --- | ----- |
| `0` | `0`   | `5` | `1ZZ` |
| `1` | `1`   | `6` | `1Z0` |
| `2` | `1Z`  | `7` | `1Z1` |
| `3` | `10`  | `8` | `10Z` |
| `4` | `11`  | `9` | `100` |

Biểu diễn số âm trong **hệ đếm** này rất dễ: chỉ cần đảo các chữ số của số dương (đổi `Z` thành `1`, và `1` thành `Z`).

| Thập phân  | Tam phân cân bằng |
| ---- | ----- |
| `-1` | `Z`   |
| `-2` | `Z1`  |
| `-3` | `Z0`  |
| `-4` | `ZZ`  |
| `-5` | `Z11` |

Dễ thấy, chữ số cao nhất của số âm là `Z`, còn của số dương là `1`.

## Quy trình

Trong phép chuyển đổi sang tam phân cân bằng, cần viết số `x` dưới dạng tam phân chuẩn trước. Khi `x` ở dạng tam phân chuẩn, mỗi chữ số là `0`、`1` hoặc `2`. Bắt đầu từ chữ số thấp nhất, ta có thể bỏ qua mọi `0` và `1`, nhưng nếu gặp `2` thì đổi nó thành `Z` và cộng `1` vào chữ số bên trái. Nếu gặp chữ số `3` thì đổi thành `0` và cộng `1` vào chữ số bên trái.

### Ứng dụng 1

Chuyển `64` sang tam phân cân bằng.

Trước hết, viết số này ở dạng tam phân chuẩn:

$$
\text 64_{10} = 02101_3
$$

Bắt đầu xử lý từ chữ số ảnh hưởng nhỏ nhất đến toàn bộ số (chữ số thấp nhất):

-   `101` được bỏ qua (vì `0` và `1` hợp lệ trong tam phân cân bằng);
-   `2` đổi thành `Z`, chữ số bên trái cộng `1`, được `1Z101`;
-   `1` được bỏ qua, vẫn là `1Z101`.

Kết quả cuối cùng là `1Z101`.

Chuyển ngược lại về thập phân:

$$
\texttt {1Z101}=81 \times 1 +27 \times (-1) + 9 \times 1 + 3 \times 0 + 1 \times 1 = 64_{10}
$$

### Ứng dụng 2

Chuyển `237` sang tam phân cân bằng.

Trước hết, viết số này ở dạng tam phân chuẩn:

$$
\text 237_{10} = 22210_3
$$

-   `0` và `1` được bỏ qua (vì `0` và `1` hợp lệ trong tam phân cân bằng);
-   `2` đổi thành `Z`, chữ số bên trái cộng `1`, được `23Z10`;
-   `3` đổi thành `0`, chữ số bên trái cộng `1`, được `30Z10`;
-   `3` đổi thành `0`, chữ số bên trái (mặc định là `0`) cộng `1`, được `100Z10`;
-   `1` được bỏ qua, được `100Z10`.

Kết quả cuối cùng là `100Z10`.

Chuyển ngược lại về thập phân:

$$
\texttt{100Z10} = 243 \cdot 1 + 81 \cdot 0 + 27 \cdot 0 + 9 \cdot (-1) + 3 \cdot 1 + 1 \cdot 0 = 237_{10}
$$

## Tính chất

Với một số tam phân cân bằng $X_3$, ta có thể nhân từng chữ số $x_i$ với trọng số tương ứng $3^i$ để nhận được duy nhất một số thập phân $Y_{10}$.

Vậy với một số thập phân $Y_{10}$, liệu có **duy nhất một số tam phân cân bằng** tương ứng không?

Câu trả lời là có, tính chất này gọi là tính duy nhất của tam phân cân bằng.

???+ note "Chứng minh"
    Ta dùng **phản chứng**:
    
    Giả sử tồn tại một số thập phân $Y_{10}$, có hai **số tam phân cân bằng khác nhau** $A_3,B_3$ cùng chuyển về $Y_{10}$, tức cần chứng minh $A_3 = B_3$. Xét các trường hợp:
    
    1.  Khi $Y_{10}=0$, hiển nhiên $A_3 = B_3 = 0_3$, mâu thuẫn với giả thiết.
    2.  Khi $Y_{10}>0$:
    
        -   Đánh số các chữ số của $A_3$ và $B_3$ từ thấp lên cao, ký hiệu $a_i$ là chữ số thứ $i$ của $A_3$, $b_i$ là chữ số thứ $i$ của $B_3$. Chắc chắn tồn tại $i$ sao cho $a_i\neq b_i$. Khi đó các chữ số $i-1,i-2,\dots,0$ không ảnh hưởng đến chứng minh. Do đó, dịch phải $A_3,B_3$ đi $i$ chữ số, được $A_3',B_3'$, và bài toán tương đương với việc chứng minh $A_3'=B_3'$.
        -   Với chữ số thứ $0$ của $A_3',B_3'$, có $a_0 \neq b_0$. Giả sử $b_0 > a_0$ (trường hợp $a_0>b_0$ tương tự), khi đó $b_0 - a_0 \in \{1,2\}$. Phần đóng góp của các chữ số $i=1,2,3,\dots$ vào giá trị của $A_3'$ là $S_1 = a_1 \times 3^1 + a_2 \times 3^2+ \dots$, của $B_3'$ là $S_2 = b_1 \times 3^1 + b_2 \times 3^2 + \dots$. Do $A_3' = B_3'$, suy ra $S_1 - S_2 = b_0 - a_0$. Nhưng $S_1,S_2$ có ước chung $3$, trong khi $b_0 - a_0$ không chia hết cho $3$, mâu thuẫn. Do đó $A_3'\neq B_3'$.
    3.  Khi $Y_{10}<0$, chứng minh tương tự như trường hợp $Y_{10}>0$.
    
    Vậy với mọi $Y_{10}$, luôn có duy nhất một số tam phân cân bằng $X_3$ tương ứng.

## Bài tập

[Topcoder SRM 604 PowerOfThree](https://archive.topcoder.com/ProblemStatement/pm/12917)

**Một phần nội dung trang này được dịch từ bài [Троичная сбалансированная система счисления](http://e-maxx.ru/algo/balanced_ternary) và bản dịch tiếng Anh [Balanced Ternary](https://cp-algorithms.com/algebra/balanced-ternary.html). Bản tiếng Nga có giấy phép Public Domain + Leave a Link; bản tiếng Anh có giấy phép CC-BY-SA 4.0.**
