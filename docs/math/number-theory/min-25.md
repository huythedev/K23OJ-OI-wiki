author: Marcythm, Xeonacid, CSPNOIP

## Định nghĩa

Từ tư tưởng phương pháp của loại sàng này, nó còn được gọi là "Extended Eratosthenes Sieve".

Do nó được [Min\_25](http://min-25.hatenablog.com/) phát minh và sử dụng đầu tiên, nên gọi là "Min\_25 Sàng".

## Tính chất

Nó có thể giải quyết một lớp bài toán về **hàm tích tính** trong thời gian $O\left(\frac{n^{\frac{3}{4}}}{\log{n}}\right)$ hoặc $\Theta\left(n^{1 - \epsilon}\right)$.

Yêu cầu: $f(p)$ là một hàm hoàn toàn tích tính có thể tính nhanh (ví dụ như đa thức); $f(p^{c})$ có thể tính nhanh.

## Ký hiệu

-   **Nếu không nói gì thêm, tất cả các biến được gọi là $p$ trong các phần này đều là các số nguyên tố.**
-   $x / y := \left\lfloor\frac{x}{y}\right\rfloor$
-   $\operatorname{isprime}(n) := [ |\{d : d \mid n\}| = 2 ]$ tức là $n$ là số nguyên tố thì giá trị là $1$, ngược lại là $0$.
-   $p_{k}$: số nguyên tố thứ $k$ nhỏ nhất (ví dụ: $p_{1} = 2, p_{2} = 3$). Đặc biệt, đặt $p_{0} = 1$.
-   $\operatorname{lpf}(n) := [1 < n] \min\{p : p \mid n\} + [1 = n]$ tức là $n$ có số nguyên tố nhỏ nhất. Đặc biệt, $n=1$ thì giá trị là $1$.
-   $F_{\mathrm{prime}}(n) := \sum_{2 \le p \le n} f(p)$
-   $F_{k}(n) := \sum_{i = 2}^{n} [p_{k} \le \operatorname{lpf}(i)] f(i)$

## Giải thích

Quan sát định nghĩa $F_{k}(n)$, ta thấy rằng đáp án là $F_{1}(n) + f(1) = F_{1}(n) + 1$.

Xem xét cách tính $F_{k}(n)$. Qua việc liệt kê từng $i$ và số mũ nhỏ nhất của nó, ta có thể được công thức đệ quy:

$$
\begin{aligned}
    F_{k}(n)
    &= \sum_{i = 2}^{n} [p_{k} \le \operatorname{lpf}(i)] f(i) \\
    &= \sum_{\substack{k \le i \\ p_{i}^{2} \le n}} \sum_{\substack{c \ge 1 \\ p_{i}^{c} \le n}} f\left(p_{i}^{c}\right) ([c > 1] + F_{i + 1}\left(n / p_{i}^{c}\right)) + \sum_{\substack{k \le i \\ p_{i} \le n}} f(p_{i}) \\
    &= \sum_{\substack{k \le i \\ p_{i}^{2} \le n}} \sum_{\substack{c \ge 1 \\ p_{i}^{c} \le n}} f\left(p_{i}^{c}\right) ([c > 1] + F_{i + 1}\left(n / p_{i}^{c}\right)) + F_{\mathrm{prime}}(n) - F_{\mathrm{prime}}(p_{k - 1}) \\
    &= \sum_{\substack{k \le i \\ p_{i}^{2} \le n}} \sum_{\substack{c \ge 1 \\ p_{i}^{c + 1} \le n}} \left(f\left(p_{i}^{c}\right) F_{i + 1}\left(n / p_{i}^{c}\right) + f\left(p_{i}^{c + 1}\right)\right) + F_{\mathrm{prime}}(n) - F_{\mathrm{prime}}(p_{k - 1})
\end{aligned}
$$

Bước cuối cùng dựa trên một sự thật: với $c$ sao cho $p_{i}^{c} \le n < p_{i}^{c + 1}$, thì $p_{i}^{c + 1} > n \iff n / p_{i}^{c} < p_{i} < p_{i + 1}$, nên $F_{i + 1}\left(n / p_{i}^{c}\right) = 0$.
Giá trị biên là $F_{k}(n) = 0 (p_{k} > n)$.

Giả sử đã có tất cả các $F_{\mathrm{prime}}(n)$, thì có hai cách để tính tất cả các $F_{k}(n)$:

1. Tính trực tiếp theo công thức đệ quy.
2. Từ lớn đến nhỏ duyệt $p$ chuyển tiếp, chỉ khi $p^{2} < n$ thì giá trị tăng không phải là $0$, nên có thể tối ưu hóa theo hậu tố.

Bây giờ xem xét cách tính $F_{\mathrm{prime}}{(n)}$.
Quan sát quá trình tính $F_{k}(n)$, dễ thấy rằng $F_{\mathrm{prime}}$ chỉ có $O(\sqrt{n})$ điểm có giá trị hữu ích: $1, 2, \dots, \left\lfloor\sqrt{n}\right\rfloor, n / \sqrt{n}, \dots, n / 2, n$.
Thường thì $f(p)$ là một đa thức bậc thấp, có thể biểu diễn thành $f(p) = \sum a_{i} p^{c_{i}}$.
Thì đối với mỗi $p^{c_{i}}$, đóng góp vào $F_{\mathrm{prime}}(n)$ là $a_{i} \sum_{2 \le p \le n} p^{c_{i}}$.
Phân tích từng $p^{c_{i}}$ đóng góp, bài toán chuyển thành: cho $n, s, g(p) = p^{s}$, đối với mọi $m = n / i$, tính $\sum_{p \le m} g(p)$.

???+ tip "Lưu ý"
    $g(p) = p^{s}$ là hàm hoàn toàn tích tính!

Vậy đặt $G_{k}(n) := \sum_{i = 2}^{n} \left[p_{k} < \operatorname{lpf}(i) \lor \operatorname{isprime}(i)\right] g(i)$, tức là tổng giá trị $g$ của các số còn lại sau khi sàng thứ $k$.
Với một hợp số $x \le n$, chắc chắn có $\operatorname{lpf}(x) \le \sqrt{x} \le \sqrt{n}$.
Đặt $p_{\ell(n)}$ là số nguyên tố lớn nhất không vượt quá $\sqrt{n}$, thì $\sum_{2\le p\le n}g(p) = G_{\ell(n)}(n)$, tức là sau khi sàng $\ell$ lần, còn lại toàn bộ là các số nguyên tố.
Xem xét giá trị biên, rõ ràng là $G_{0}(n) = \sum_{i = 2}^{n} g(i)$.
Đối với chuyển tiếp, xem xét quá trình sàng, phân tích từng phần:

1. Với $n < p_{k}^{2}$, $G$ không đổi, tức là $G_{k}(n) = G_{k - 1}(n)$.
2. Với $p_{k}^{2} \le n$, các số bị sàng đều có thừa số nguyên tố $p_{k}$, tức là $-g(p_{k}) G_{k - 1}(n / p_{k})$.
3. Với phần thứ hai, do $p_{k}^{2} \le n \iff p_{k} \le n / p_{k}$, các $i$ thỏa mãn $\operatorname{lpf}(i) < p_{k}$ sẽ bị trừ đi. Phần này cần cộng lại, tức là $g(p_{k}) G_{k - 1}(p_{k - 1})$.

Thì có:

$$
G_{k}(n) = G_{k - 1}(n) - \left[p_{k}^{2} \le n\right] g(p_{k}) (G_{k - 1}(n / p_{k}) - G_{k - 1}(p_{k - 1}))
$$

## Phân tích độ phức tạp

Với $F_{k}(n)$, phương pháp thứ nhất có độ phức tạp $O\left(n^{1 - \epsilon}\right)$ (xem zzt tập huấn 2.3);  
Phương pháp thứ hai, thực chất là phần thứ hai của sàng Châu Gé, trong bài luận của Châu Gé cũng có đề cập (6.5.4), độ phức tạp được chứng minh là $O\left(\frac{n^{\frac{3}{4}}}{\log{n}}\right)$.

Với $F_{\mathrm{prime}}(n)$, thực tế, nó giống với phần đầu tiên của sàng Châu Gé.
Xem xét với mỗi $m = n / i$, chỉ có khi liệt kê các $p_{k}$ thỏa mãn $p_{k}^{2} \le m$ thì mới có đóng góp vào độ phức tạp, nên có thể ước lượng:

$$
\begin{aligned}
    T(n)
    &= \sum_{i^{2} \le n} O\left(\pi\left(\sqrt{i}\right)\right) + \sum_{i^{2} \le n} O\left(\pi\left(\sqrt{\frac{n}{i}}\right)\right) \\
    &= \sum_{i^{2} \le n} O\left(\frac{\sqrt{i}}{\ln{\sqrt{i}}}\right) + \sum_{i^{2} \le n} O\left(\frac{\sqrt{\frac{n}{i}}}{\ln{\sqrt{\frac{n}{i}}}}\right) \\
    &= O\left(\int_{1}^{\sqrt{n}} \frac{\sqrt{\frac{n}{x}}}{\log{\sqrt{\frac{n}{x}}}} \mathrm{d} x\right) \\
    &= O\left(\frac{n^{\frac{3}{4}}}{\log{n}}\right)
\end{aligned}
$$

Về không gian, có thể thấy rằng cả $F_{k}$ và $F_{\mathrm{prime}}$ chỉ có giá trị hữu ích tại $n / i$, tổng cộng $O(\sqrt{n})$ điểm, chỉ cần ghi lại giá trị hữu ích là có thể tối ưu hóa không gian xuống $O(\sqrt{n})$.

Trước hết, qua một lần phân tích số học, ta có thể tìm được tất cả các giá trị hữu ích, dùng một mảng $\text{lis}$ kích thước $O(\sqrt{n})$ để ghi lại. Với giá trị hữu ích $v$, ghi $\text{id}(v)$ là chỉ số trong $\text{lis}$, dễ thấy rằng: với mọi giá trị hữu ích $v$, $\text{id}(v) \le \sqrt{n}$.

Sau đó phân tích riêng các giá trị hữu ích nhỏ hơn hoặc bằng $\sqrt{n}$ và lớn hơn $\sqrt{n}$: với giá trị hữu ích nhỏ hơn hoặc bằng $\sqrt{n}$, dùng một mảng $\text{le}$ để ghi lại $\text{id}(v)$, tức là $\text{le}_v = \text{id}(v)$; với giá trị hữu ích lớn hơn $\sqrt{n}$, dùng một mảng $\text{ge}$ để ghi lại $\text{id}(v)$, do $v$ quá lớn nên dùng $v' = n / v < \sqrt{n}$ để ghi lại $\text{id}(v)$, tức là $\text{ge}_{v'} = \text{id}(v)$.

Vậy ta có thể dùng hai mảng kích thước $O(\sqrt{n})$ để ghi lại tất cả các giá trị hữu ích của $\text{id}$ và $O(1)$ để truy vấn. Trong quá trình tính $F_{k}$ hoặc $F_{\mathrm{prime}}$, dùng chỉ số $\text{id}$ thay cho giá trị hữu ích, có thể tối ưu hóa không gian xuống $O(\sqrt{n})$.

## Quá trình

Với $F_{k}(n)$, ta thường chọn phương pháp đầu tiên vì độ khó thực hiện thấp, thường biểu hiện tốt hơn phương pháp thứ hai khi dữ liệu nhỏ.

Với $F_{\mathrm{prime}}(n)$, thực hiện trực tiếp theo công thức đệ quy.

Với $p_{k}^{2} \le n$ thì có thể dùng sàng tuyến tính để tính $s_{k} := F_{\mathrm{prime}}(p_{k})$ thay cho $F_{k}$ đệ quy trong $F_{\mathrm{prime}}(p_{k - 1})$.
Tương ứng, $G$ đệ quy trong $G_{k - 1}(p_{k - 1}) = \sum_{i = 1}^{k - 1} g(p_{i})$ cũng có thể dùng phương pháp này để tiền xử lý.

Dùng Extended Eratosthenes Sieve để tính **hàm tích tính** $f$ trước, cần rõ ràng các điểm sau:

-   Cách sàng nhanh (thường là thời gian tuyến tính) các giá trị $f$ trước $\sqrt{n}$.
-   Biểu diễn đa thức của $f(p)$.
-   Cách tính nhanh $f(p^{c})$.

Rõ ràng các điểm trên, thực hiện theo thứ tự:

1.  Sàng $[1, \sqrt{n}]$ và các giá trị $f$ trước $\sqrt{n}$.
2.  Với mỗi hạng tử của đa thức $f(p)$, sàng ra các $G$, hợp lại thành $F_{\mathrm{prime}}$ có $O(\sqrt{n})$ điểm hữu ích.
3.  Thực hiện đệ quy theo $F_{k}$, tính $F_{1}(n)$.

## Ví dụ

???+ example "[Luogu P4213【模板】杜教筛](https://www.luogu.com.cn/problem/P4213)"
    Tính $\displaystyle \sum_{i = 1}^{n} \varphi(i)$ và $\displaystyle \sum_{i = 1}^{n} \mu(i)$.

??? note "Giải"
    Với $\varphi(i)$, dễ thấy $f(p) = p - 1$.
    Với $f(p)$ bậc nhất $(p)$, có $g(p) = p, G_{0}(n) = \sum_{i = 2}^{n} g(i) = \frac{(n + 2) (n - 1)}{2}$.
    Với $f(p)$ bậc hằng $(-1)$, có $g(p) = -1, G_{0}(n) = \sum_{i = 2}^{n} g(i) = -n + 1$.
    Sàng hai lần cộng lại là được $F_{\mathrm{prime}}$ cần thiết.

    Với $\mu(i)$, dễ thấy $f(p) = -1$.
    Có $g(p) = -1, G_{0}(n) = \sum_{i = 2}^{n} g(i) = -n + 1$.
    Sàng trực tiếp là được $F_{\mathrm{prime}}$ cần thiết.

???+ example "[LOJ 6053 简单的函数](https://loj.ac/p/6053)"
    Cho $f(n)$:

    $$
    f(n) = \begin{cases}
        1 & n = 1 \\
        p \operatorname{xor} c & n = p^{c} \\
        f(a)f(b) & n = ab \land a \perp b
    \end{cases}
    $$

    Tính $\displaystyle \sum_{i = 1}^{n} f(i)$.

??? note "Giải"
    Dễ thấy $f(p) = p - 1 + 2[p = 2]$.
    Vậy sàng theo cách sàng $\varphi$ là được, chỉ cần xét riêng $2$.

??? note "Tham khảo code"
    ```cpp
    --8<-- "docs/math/code/min-25/min-25_1.cpp"
    ```
