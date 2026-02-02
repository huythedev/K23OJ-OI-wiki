## Định nghĩa

Powerful Number (gọi tắt PN) sieve tương tự Du Jiao sieve, hoặc có thể xem là một mở rộng của Du Jiao sieve, dùng để tính prefix sum của một số hàm tích tính.

**Yêu cầu**:

-   Tồn tại hàm $g$ thỏa:
    -   $g$ là hàm tích tính.
    -   $g$ dễ tính prefix sum.
    -   Với số nguyên tố $p$, $g(p) = f(p)$.

Giả sử cần tính prefix sum của hàm tích tính $f$: $F(n) = \sum_{i=1}^{n} f(i)$.

## Powerful Number

**Định nghĩa**: Với số nguyên dương $n$, phân tích $n = \prod_{i=1}^{m} p_{i}^{e_{i}}$. $n$ là PN khi và chỉ khi $\forall 1 \le i \le m, e_{i} > 1$.

**Tính chất 1**: Mọi PN đều có thể viết dưới dạng $a^{2}b^{3}$.

**Chứng minh**: Nếu $e_i$ chẵn thì gộp $p_{i}^{e_{i}}$ vào $a^{2}$; nếu $e_i$ lẻ thì gộp $p_{i}^{3}$ vào $b^{3}$, sau đó gộp $p_{i}^{e_{i}-3}$ vào $a^{2}$.

**Tính chất 2**: Số PN không vượt quá $n$ nhiều nhất là $O(\sqrt{n})$.

**Chứng minh**: Xét liệt kê $a$, rồi đếm số $b$ thỏa điều kiện, số PN xấp xỉ

$$
\int_{1}^{\sqrt{n}} \sqrt[3]{\frac{n}{x^2}} \mathrm{d}x = O(\sqrt{n})
$$

Vậy làm sao tìm tất cả PN $\le n$? Dùng sàng tuyến tính tìm các số nguyên tố $\le \sqrt{n}$, rồi DFS liệt kê các số mũ của các nguyên tố. Do số PN $\le n$ nhiều nhất $O(\sqrt{n})$, nên số lần tìm kiếm tối đa $O(\sqrt{n})$.

## PN sieve

Đầu tiên, xây dựng hàm tích tính $g$ dễ tính prefix sum và thỏa $g(p)=f(p)$ với mọi nguyên tố $p$. Đặt $G(n) = \sum_{i=1}^{n} g(i)$.

Sau đó, định nghĩa $h = f / g$, trong đó $/$ là phép chia theo chập Dirichlet. Theo tính chất chập Dirichlet, $h$ cũng là hàm tích tính, nên $h(1)=1$. Ta có $f = g * h$ (với $*$ là chập Dirichlet).

Với nguyên tố $p$, $f(p) = g(1)h(p) + g(p)h(1) = h(p) + g(p) \implies h(p) = 0$. Từ $h(p)=0$ và tính tích tính của $h$ suy ra với số không phải PN thì $h(n)=0$, tức $h$ chỉ khác $0$ tại PN.

Từ $f = g * h$:

$$
\begin{aligned}
F(n) &= \sum_{i = 1}^{n} f(i)\\
     &= \sum_{i = 1}^{n} \sum_{d|i} h(d) g\left(\frac{i}{d}\right)\\
     &= \sum_{d=1}^{n} \sum_{i=1}^{\lfloor \frac{n}{d}\rfloor} h(d) g(i)\\
     &= \sum_{d=1}^{n} h(d) \sum_{i=1}^{\lfloor \frac{n}{d}\rfloor}  g(i) \\
     &= \sum_{d=1}^{n} h(d) G\left(\left\lfloor \frac{n}{d}\right\rfloor\right)\\
     &= \sum_{\substack{d=1 \\ d \text{ là PN}}}^{n}h(d) G\left(\left\lfloor \frac{n}{d}\right\rfloor\right)
\end{aligned}
$$

Tìm tất cả PN trong $O(\sqrt{n})$, tính các giá trị hữu hiệu của $h$. Để tính $h$ chỉ cần tính $h(p^c)$, sau đó dùng tính tích tính để suy ra $h$ ở các PN khác. Với mỗi giá trị hữu hiệu $d$, tính $h(d)G\left(\left\lfloor \dfrac{n}{d} \right\rfloor\right)$ và cộng lại để thu được $F(n)$.

Xét cách tính $h(p^c)$, có hai cách: suy ra công thức chỉ phụ thuộc $p,c$; hoặc dùng $f = g * h$ có $f(p^c) = \sum_{i=0}^c g(p^i)h(p^{c-i})$, suy ra $h(p^c) = f(p^c) - \sum_{i=1}^{c}g(p^i)h(p^{c-i})$, rồi liệt kê $p,c$ để tính toàn bộ $h(p^c)$.

### Quy trình

1.  Xây dựng $g$
2.  Xây dựng cách tính nhanh $G$
3.  Tính $h(p^c)$
4.  Duyệt PN, trong quá trình cộng dồn đáp án
5.  Lấy kết quả

Bước 3 có thể dùng công thức trực tiếp, hoặc tiền xử lý bằng liệt kê, hoặc tính tạm khi cần.

### Phân tích

Lấy cách 2 để phân tích. Chia thành phần tính $h(p^c)$ và phần tìm kiếm.

Với phần 1, số nguyên tố $\le \sqrt{n}$ là $O\left(\dfrac{\sqrt{n}}{\log n}\right)$. Mỗi nguyên tố có số mũ tối đa $\log n$. Tính $h(p^c)$ cần lặp $(c-1)$ lần, nên độ phức tạp $O\left(\dfrac{\sqrt{n}}{\log n} \cdot \log n \cdot \log n\right) = O(\sqrt{n}\log{n})$. Đây là cận trên lỏng; tùy bài có thể tối ưu thêm.

Phần tìm kiếm: do số PN $\le n$ là $O(\sqrt{n})$, nên tối đa $O(\sqrt{n})$ lượt. Với mỗi PN, tùy cách tính $G$ mà độ phức tạp khác nhau. Ví dụ nếu $G\left(\left\lfloor \dfrac{n}{d}\right\rfloor\right)$ là $O(1)$ thì phần này là $O(\sqrt{n})$.

Đặc biệt, nếu dùng Du Jiao sieve để tính $G\left(\left\lfloor \dfrac{n}{d}\right\rfloor\right)$ thì độ phức tạp phần 2 là độ phức tạp Du Jiao sieve, tức $O(n^{\frac{2}{3}})$. Vì nếu đã tính $G(n)$ một lần và dùng sàng tuyến tính cộng với cấu trúc truy cập nhanh (như `std::map`/`std::unordered_map`) để lưu các giá trị lớn, thì các giá trị $G\left(\left\lfloor \dfrac{n}{d}\right\rfloor\right)$ đều đã có trong sàng hoặc trong map.

Về bộ nhớ, nút thắt là lưu $h(p^c)$. Nếu dùng mảng 2 chiều $a$, $a_{i,j}$ là $h(p_i^j)$, thì bộ nhớ là $O\left(\dfrac{\sqrt{n}}{\log n} \cdot \log n\right) = O(\sqrt{n})$.

## Ví dụ

### [Luogu P5325【模板】Min\_25 筛](https://www.luogu.com.cn/problem/P5325)

**Đề**: Cho hàm tích tính $f(p^k) = p^k(p^k-1)$, tính $\sum_{i=1}^{n} f(i)$.

Dễ thấy $f(p) = p(p-1) = \operatorname{id}(p)\varphi(p)$, xây dựng $g(n) = \operatorname{id}(n)\varphi(n)$.

Dùng Du Jiao sieve để tính $G(n)$, theo $(\operatorname{id}\cdot \varphi) * \operatorname{id} = \operatorname{id}_2$ suy ra $G(n)= \sum_{i=1}^{n} i^2 - \sum_{d=2}^{n} d \cdot G\left(\left\lfloor \dfrac{n}{d} \right\rfloor\right)$.

Sau đó $h(p^k)$ có thể tính bằng liệt kê, phần này không lặp lại.

Ngoài ra có thể trực tiếp suy ra công thức $h(p^k)$ chỉ phụ thuộc $p,k$:

$$
\begin{aligned}
& f(p^k) = \sum_{i=0}^{k} g(p^{k-i})h(p^i)\\
\iff & p^k(p^k-1) = \sum_{i=0}^{k} p^{k-i}\varphi(p^{k-i}) h(p^i)\\
\iff & p^k(p^k-1) = \sum_{i=0}^{k} p^{2k-2i-1}(p - 1) h(p^i)\\
\iff & p^k(p^k-1) = h(p^k) + \sum_{i=0}^{k-1} p^{2k-2i-1}(p - 1) h(p^i)\\
\iff & h(p^k) = p^k(p^k-1) - \sum_{i=0}^{k-1} p^{2k-2i-1}(p - 1) h(p^i)\\
\iff & h(p^k) - p^2h(p^{k-1}) = p^{k}(p^k-1)-p^{k+1}(p^{k-1}-1) - p(p-1)h(p^{k-1})\\
\iff & h(p^k) - ph(p^{k-1}) = p^{k+1} - p^k\\
\iff & \frac{h(p^k)}{p^k} - \frac{h(p^{k-1})}{p^{k-1}} = p - 1\\
\end{aligned}
$$

Từ $h(p)=0$, cộng dồn suy ra $h(p^k) = (k-1)(p-1)p^k$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/powerful-number/powerful-number_1.cpp"
    ```

### [「LOJ #6053」简单的函数](https://loj.ac/problem/6053)

Cho $f(n)$:

$$
f(n) =
\begin{cases}
1 & n = 1 \\
p \oplus c & n=p^c \\
f(a)f(b) & n=ab \text{ và } a \perp b
\end{cases}
$$

Dễ thấy:

$$
f(p) =
\begin{cases}
p + 1 & p = 2 \\
p - 1 & \text{ngược lại} \\
\end{cases}
$$

Xây dựng $g$ là

$$
g(n) =
\begin{cases}
3 \varphi(n) & 2 \mid n \\
\varphi(n) & \text{ngược lại} \\
\end{cases}
$$

Dễ chứng minh $g(p) = f(p)$ và $g$ tích tính.

Xét tính $G(n)$.

$$
\begin{aligned}
G(n)
&= \sum_{i=1}^{n}[i \bmod 2 = 1] \varphi(i) + 3 \sum_{i=1}^{n}[i \bmod 2 = 0] \varphi(i)\\
&= \sum_{i=1}^{n} \varphi(i) + 2\sum_{i=1}^{n} [i \bmod 2 = 0]\varphi(i) \\
&= \sum_{i=1}^{n} \varphi(i) + 2\sum_{i=1}^{\lfloor \frac{n}{2} \rfloor} \varphi(2i)
\end{aligned}
$$

Đặt $S_1(n) = \sum_{i=1}^{n} \varphi(i)$, $S_2(n) = \sum_{i=1}^{n} \varphi(2i)$, ta có $G(n) = S_1(n) + 2S_2\left(\left\lfloor \dfrac{n}{2} \right\rfloor\right)$.

Khi $2 \mid n$:

$$
\begin{aligned}
S_2(n)
&= \sum_{i=1}^{n} \varphi(2i) \\
&= \sum_{i=1}^{\frac{n}{2}} (\varphi(2(2i-1)) + \varphi(2(2i))) \\
&= \sum_{i=1}^{\frac{n}{2}} (\varphi(2i-1) + 2\varphi(2i)) \\
&= \sum_{i=1}^{\frac{n}{2}} (\varphi(2i-1) + \varphi(2i)) + \sum_{i=1}^{\frac{n}{2}} \varphi(2i) \\
&= \sum_{i=1}^{n} \varphi(i) + S_2\left(\frac{n}{2}\right)\\
&= S_1(n) + S_2\left(\left\lfloor \frac{n}{2} \right\rfloor\right)\\
\end{aligned}
$$

Khi $2 \nmid n$:

$$
\begin{aligned}
S_2(n)
&= S_2(n-1) + \varphi(2n) \\
&= S_2(n-1) + \varphi(n) \\
&= \sum_{i=1}^{n-1} \varphi(i) + S_2\left(\frac{n-1}{2}\right) + \varphi(n)\\
&= S_1(n) + S_2\left(\left\lfloor \frac{n}{2} \right\rfloor\right)\\
\end{aligned}
$$

Suy ra $S_2(n) = S_1(n) + S_2\left(\left\lfloor \dfrac{n}{2} \right\rfloor\right)$.

$S_1$ có thể tính bằng Du Jiao sieve, $S_2$ tính trực tiếp theo công thức, từ đó có $G$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/powerful-number/powerful-number_2.cpp"
    ```

## Bài tập

-   [PE708 Twos are all you need](https://projecteuler.net/problem=708)
-   [PE639 Summing a multiplicative function](https://projecteuler.net/problem=639)
-   [PE484 Arithmetic Derivative](https://projecteuler.net/problem=484)

## Tài liệu tham khảo

-   [破壁人五号 - Powerful number 筛略解](https://www.cnblogs.com/wallbreaker5th/p/13901487.html)
-   [command\_block - 杜教筛（+ 贝尔级数 + powerful number）](https://www.luogu.com.cn/blog/command-block/du-jiao-shai)
