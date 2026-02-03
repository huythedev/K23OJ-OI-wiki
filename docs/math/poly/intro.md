## Đa thức và hàm sinh

Việc thao tác các đa thức hữu hạn/vô hạn là nội dung quan trọng trong toán OI, đặc biệt là trong hàm sinh.

Các thuật toán đa thức dựa trên [biến đổi Fourier nhanh](./fft.md) trao cho thí sinh khả năng thao tác trực tiếp trên hàm sinh.

## Khái niệm cơ bản

Với tổng $\sum a_nx^n$, nếu là tổng hữu hạn thì gọi là đa thức, ký hiệu $f(x)=\sum_{n=0}^m a_nx^n$.

Tổng vô hạn đếm được gọi là chuỗi. Trong tổng $\sum_{n=0}^\infty a_nx^n$, mỗi hạng là hàm mũ nguyên không âm nhân hệ số hằng; dạng chuỗi như vậy gọi là chuỗi lũy thừa.

Khi nghiên cứu số học đa thức, thường xét đa thức đơn giản trước; khái niệm chuỗi lũy thừa chỉ dùng để tiện hiểu. Trong giải tích sẽ nghiên cứu thêm tính hội tụ/phân kỳ của chuỗi lũy thừa.

Định nghĩa tổng quát về vành, trường và các cấu trúc dẫn xuất xem tại [Khái niệm cơ bản của đại số trừu tượng](../algebra/basic.md).

Với vành $R$ bất kỳ, định nghĩa **vành đa thức** (polynomial ring) $R[x]$.

Mỗi phần tử $f$ gọi là **đa thức** (polynomial) trên $R$, có thể biểu diễn:

$$
f=\left<f_0,f_1,f_2,\cdots,f_n\right>\quad(f_0,f_1,f_2,\cdots,f_n\in R)
$$

Nói cách khác, ta định nghĩa đa thức trực tiếp là một dãy hệ số. Cũng có thể viết:

$$
f(x)=f_0+f_1x+f_2x^2+\cdots+f_nx^n
$$

Ở đây, ta coi $x$ chỉ là một **ký hiệu hình thức**, dùng để đánh dấu vị trí hệ số.

Nếu cho phép tồn tại vô hạn hạng, tức là

$$
f(x)=f_0+f_1x+f_2x^2+\cdots
$$

thì ta có **vành chuỗi lũy thừa hình thức** (formal power series ring) $R[[x]]$, mỗi phần tử $f$ gọi là **chuỗi lũy thừa hình thức** (formal power series), sau đây gọi tắt là chuỗi lũy thừa.

### Bậc của đa thức

Với đa thức $f(x)$, bậc của hạng cao nhất gọi là **bậc (degree)** của đa thức, ký hiệu $\operatorname{deg}{f}$.

### Phép nhân đa thức

Thao tác cốt lõi là nhân hai đa thức, tức cho đa thức $f(x)$ và $g(x)$:

$$
\begin{alignedat}{3}
f(x)&=a_0+a_1x+\dots+a_nx^n\quad \quad &(1)\\
g(x)&=b_0+b_1x+\dots+b_mx^m\quad \quad &(2)
\end{alignedat}
$$

Cần tính đa thức $Q(x)=f(x)\cdot g(x)$:

$$
\boxed {Q(x) = \sum \limits_ {i = 0} ^ n \sum \limits_ {j = 0 } ^ m a_i b_j x ^ {i + j}} = c_0 + c_1 x + \dots + c_ {n + m} x ^ {n + m}
$$

Phép nhân đa thức hoặc chuỗi lũy thừa thỏa tính kết hợp, và thỏa tính phân phối với phép cộng. Nếu $R$ là vành giao hoán hoặc vành có đơn vị, phép nhân tương ứng cũng có tính giao hoán và phần tử đơn vị.

Nếu trên $R$ tồn tại căn bậc $2^n$ của đơn vị, [FFT](./fft.md) cho phép tính tích của hai đa thức bậc $2^n$ trong $O(n2^n)$ thay vì $O(2^{2n})$.

### Hợp

Định nghĩa lũy thừa của phần tử $f$ trong $R[[x]]$ là

$$
f^1=f,f^k=f^{k-1}\times f
$$

Dựa trên đó, định nghĩa hợp của $f,g$ trong $R[[x]]$ là

$$
(f\circ g)(x)=f(g(x))=f_0+\sum_{k=1}^{+\infty}f_kg^k(x)
$$

Quy ước $f\circ g$ tồn tại khi và chỉ khi $f$ hữu hạn hạng hoặc $g_0=0$, nhờ đó không cần xét giới hạn trên $R$.

$\circ$ thỏa tính kết hợp (khi $(f\circ g)\circ h$ và $f\circ (g\circ h)$ đều tồn tại), không thỏa tính giao hoán. Khi $R$ là vành có đơn vị, $\circ$ có phần tử đơn vị $1\times x$.

Khi FFT khả thi, hợp của đa thức hữu hạn có thuật toán $O((n\log n)^{1.5})$, nhưng hằng số lớn; thực tế thuật toán $O(n^2)$ kiểu “FFT theo khối” thường hiệu quả hơn.

### Đạo hàm

Dù vành nói chung có thể không có giới hạn,  
ta vẫn định nghĩa **đạo hàm hình thức** (formal derivative) của chuỗi lũy thừa:

$$
\left(\sum_{k=0}^{+\infty}f_kx^k\right)'=\sum_{k=1}^{+\infty}kf_kx^{k-1}
$$

trong đó

$$
kf_k=\underbrace{f_k+f_k+\cdots+f_k}_{k \text{ lần } f_k}
$$

Các quy tắc đạo hàm cơ bản—quy tắc cộng, quy tắc nhân, quy tắc dây chuyền (nếu hợp được phép)—vẫn đúng.

Nếu $R$ cho phép chia, có thể tương tự định nghĩa **tích phân bất định hình thức** (formal indefinite integral).

### Phần tử nghịch đảo nhân

Từ ví dụ

$$
\dfrac{1}{1-x}=1+x+x^2+\cdots
$$

có thể thấy nghịch đảo của đa thức có thể khai triển thành chuỗi vô hạn. Tồn tại nghịch đảo khi và chỉ khi hạng tự do khác $0$, và nghịch đảo cũng có hạng tự do khác $0$.

Do đó định nghĩa: với chuỗi lũy thừa hình thức $f$, nếu $f_0\not=0$, **nghịch đảo nhân** (multiplicative inversion) $f^{-1}$ là một chuỗi lũy thừa hình thức khác, thỏa

$$
f\times f^{-1}=f^{-1}\times f=1
$$

Khai triển theo định nghĩa nhân chuỗi lũy thừa, ta được công thức truy hồi cho hệ số $f^{-1}$:

$$
f^{-1}_0=\dfrac{1}{f_0},f^{-1}_n=\dfrac{-1}{f_0}\sum_{k=0}^{n-1}f^{-1}_kf_{n-k}
$$

Dùng truy hồi trực tiếp để tính $n$ hạng đầu là $O(n^2)$, [dùng FFT](./elementary-func.md#多项式求逆) có thể đạt $O(n\log n)$.

???+ note "Chú"
    Dễ thấy, nghịch đảo của $f(x)$ chính là khai triển Maclaurin vô hạn của $\frac{1}{f(x)}$ (khai triển Taylor vô hạn tại $x=0$).

### Các khai triển chuỗi lũy thừa thường gặp

Trong giải tích, một hàm một biến khả vi đến hữu hạn bậc tại một điểm hoặc trên một khoảng có thể khai triển đa thức; nói chung gọi là khai triển Taylor, nếu tại $0$ thì gọi là khai triển Maclaurin.

Nếu khả vi vô hạn, có thể khai triển thành chuỗi lũy thừa. Phổ biến nhất là khai triển tại $0$.

Trong hàm phức, một số hàm không thể khai triển Taylor tại điểm kỳ dị nhưng có thể khai triển Laurent.

Các đẳng thức sau chỉ đúng khi chuỗi lũy thừa hội tụ; khi không hội tụ thì không đúng. Ở đây chỉ liệt kê khai triển, không bàn miền hội tụ.

Hai khai triển cơ bản là hàm mũ và hàm lũy thừa:

$$
\mathrm{e}^x=1+x+\frac{1}{2!}x^2+\ldots+\frac{1}{n!}x^n+\ldots
$$

$$
(1+x)^a=1+ax+\frac{a(a-1)}{2!}x^2+\ldots+\frac{a(a-1)\ldots(a-n+1)}{n!}x^n+\ldots
$$

Nhiều khai triển khác được suy ra từ hai khai triển trên, ví dụ hàm sin/cos từ hàm mũ với số phức:

$$
\cos x=1-\frac{1}{2!}x^2+\frac{1}{4!}x^4+\ldots+\frac{(-1)^n}{(2n)!}x^{2n}+\ldots
$$

$$
\sin x=x-\frac{1}{3!}x^3+\frac{1}{5!}x^5+\ldots+\frac{(-1)^n}{(2n+1)!}x^{2n+1}+\ldots
$$

Logarithm, arctan, arcsin suy ra từ tích phân:

$$
\frac{1}{1+x}=1-x+x^2+\ldots+{(-1)}^n x^n+\ldots
$$

$$
\ln(1+x)=x-\frac{1}{2}x^2+\frac{1}{3}x^3+\ldots+\frac{{(-1)}^{n-1}}{n}x^n+\ldots
$$

$$
\frac{1}{1+x^2}=1-x^2+x^4+\ldots+{(-1)}^n x^{2n}+\ldots
$$

$$
\arctan x=x-\frac{1}{3!}x^3+\frac{1}{5!}x^5+\ldots+\frac{{(-1)}^{n}}{2n+1}x^{2n+1}+\ldots
$$

$$
\frac{1}{\sqrt{1+x}}=1-\frac{1}{2}x+\frac{3}{8}x^2+\ldots+{(-1)}^n\frac{(2n)!}{{(n!)}^2 4^n} x^{n}+\ldots
$$

$$
\frac{1}{\sqrt{1-x^2}}=1+\frac{1}{2}x^2+\frac{3}{8}x^4+\ldots+\frac{(2n)!}{{(n!)}^2 4^n} x^{2n}+\ldots
$$

$$
\arcsin x=x+\frac{1}{6}x^3+\frac{3}{40}x^5+\ldots+\frac{(2n)!}{{(n!)}^2(2n+1)4^n} x^{2n+1}+\ldots
$$

### Nghịch hợp

**Nghịch hợp** (compound inversion) là mở rộng khái niệm hàm ngược trong vành chuỗi lũy thừa hình thức.

Với chuỗi lũy thừa hình thức $f$ thỏa $f_0=0$ và $f_1\not=0$, nghịch hợp của nó là chuỗi $g$ sao cho $g(f(x))=f(g(x))=x$. Theo Lagrange inversion, với mọi số nguyên $n,k$ ta có

$$
n[x^n]f^k=k[x^{n-k}]\left(\dfrac{x}{g}\right)^n
$$

trong đó $[x^k]f(x)$ là hệ số của $x^k$ trong $f(x)$.

### Chia hết đa thức

Với đa thức $f(x)$ và $g(x)$, nếu tồn tại đa thức $h(x)$ sao cho:

$$
f(x)=g(x)h(x)
$$

thì $g(x)$ chia hết $f(x)$.

Hiển nhiên, $g(x)$ chia hết $f(x)$ khi và chỉ khi mọi nghiệm của $g(x)$ đều là nghiệm của $f(x)$, và bội số trong $g(x)$ không vượt quá bội số tương ứng trong $f(x)$.

### Thương và dư của đa thức

Với đa thức $f(x), g(x)$, tồn tại **duy nhất** $Q(x), R(x)$ thỏa:

$$
\begin{aligned}
    f(x) &= Q(x) g(x) + R(x) \\
    \operatorname{deg}{R} &< \operatorname{deg}{g}
\end{aligned}
$$

Khi $\operatorname{deg}{f} \ge \operatorname{deg}{g}$ thì $\operatorname{deg}{Q} = \operatorname{deg}{f} - \operatorname{deg}{g}$, còn nếu không thì $Q(x) = 0$. Ta gọi $Q(x)$ là **thương (quotient)** của $f(x)$ chia cho $g(x)$, và $R(x)$ là **dư (remainder)**.

## Đa thức mô-đun

Đa thức mô-đun là vành con của vành đa thức, thu được bằng cách chia theo quan hệ đồng dư.

Trong phép chia có dư ở trên, đa thức $f(x)$ và phần dư $R(x)$ đồng dư theo mô-đun đa thức $g(x)$.

$$
f(x) \equiv R(x) \pmod{g(x)}
$$

Đồng dư này cũng có nghĩa: với mọi nghiệm $x_0$ của $g(x)$, thay vào $f(x)$ và $R(x)$ cho cùng giá trị:

$$
f(x_0)=R(x_0)
$$

Hơn nữa, nếu nghiệm $x_0$ có bội số $k$ trong $g(x)$, tức $(x-x_0)^k$ chia hết $g(x)$, thì với mọi số nguyên $t$ thỏa $0\le t<k$, ta có:

$$
f^{t}(x_0)=R^{t}(x_0)
$$

Ký hiệu ở đây là đạo hàm bậc $t$.

Đồng dư mô-đun đa thức có thể áp dụng cho chuỗi lũy thừa. Một chuỗi vô hạn có thể đồng dư với một đa thức hữu hạn theo một mô-đun đa thức cụ thể. Ví dụ:

$$
1+x+x^2+x^3+\ldots \equiv 1+x+\ldots+x^{n-1} \pmod{x^n}
$$

Rõ ràng các hạng còn lại đều chia hết cho $x^n$, nên mô-đun $x^n$ tương đương với “cắt cụt”, tức chỉ giữ $n$ hạng đầu và bỏ thông tin bậc cao.

Trong một số trường hợp đặc biệt, cũng có thể mô-đun theo các đa thức khác; phần sau sẽ giải thích.

### Đánh giá đa điểm và nội suy đa thức

**Đánh giá đa điểm (multi-point evaluation)**: cho đa thức $f(x)$ và $n$ điểm $x_{1}, x_{2}, \dots, x_{n}$, tính

$$
f(x_{1}), f(x_{2}), \dots, f(x_{n})
$$

**Nội suy (interpolation)**: cho $n+1$ điểm

$$
(x_{0}, y_{0}), (x_{1}, y_{1}), \dots, (x_{n}, y_{n})
$$

tìm đa thức bậc $n$ $f(x)$ sao cho $n+1$ điểm đều thuộc đồ thị $f(x)$.

Bản chất của hai thao tác này là chuyển đổi giữa **biểu diễn theo hệ số** và **biểu diễn theo giá trị**. Đánh giá đa điểm chuyển từ hệ số sang giá trị; nội suy chuyển từ giá trị sang hệ số.

???+ note "Chú"
    Theo góc nhìn chuỗi lũy thừa, đánh giá đa điểm tương đương “nén” thông tin vô hạn vào hữu hạn giá trị, nên mất mát một phần thông tin; nội suy là khôi phục về biểu diễn hệ số tương ứng bậc.
    
    Các phép biến đổi thường gặp trong lập trình như DFT (và nghịch biến đổi) chọn $n+1$ điểm có bội số đều bằng $1$, tức đôi một khác nhau, tránh việc phải lấy đạo hàm.
    
    Việc “nén” này chỉ đảm bảo nhất quán tại $n+1$ điểm. Theo giải thích về đồng dư mô-đun đa thức ở trên, nếu chuỗi lũy thừa $f(x)$ sau khi đánh giá tại $x_0$ đến $x_n$ rồi nội suy được đa thức $R(x)$, thì đa thức:
    
    $$
    g(x)=(x-x_0)\ldots(x-x_n)
    $$
    
    thỏa:
    
    $$
    f(x) \equiv R(x) \pmod{g(x)}
    $$
    
    Do $\deg R(x) < \deg g(x)$, $R(x)$ chính là phần dư. Vì vậy trong trường hợp này, nếu chuỗi lũy thừa có thể đánh giá tại các nghiệm, thì có thể mô-đun theo đa thức. Một phản ví dụ:
    
    $$
    \frac{1}{1-x}=1+x+x^2+x^3+\ldots
    $$
    
    không thể đánh giá tại $x=1$, nên chuỗi $1+x+x^2+x^3+\ldots$ không thể mô-đun theo $x-1$.
    
    Do các đạo hàm mọi bậc của chuỗi lũy thừa luôn tồn tại tại $0$, nên mô-đun theo $x^n$ luôn tính được, tương ứng với ý nghĩa “cắt cụt” ở trên. DFT (và nghịch biến đổi) tương đương mô-đun theo $x^n-1$.

### Phân tích nhân tử và Euclid

Nhiều kết luận trong số học sơ cấp có thể mở rộng sang đa thức.

Trên trường số phức, theo định lý cơ bản của đại số, với đa thức bậc $n$ thì phương trình

$$
f(x)=0
$$

có đúng $n$ nghiệm (tính theo bội số).

Do đó $f(x)$ trên $\mathbb{C}$ phân tích duy nhất thành

$$
a(x-x_1)^{c_1}(x-x_2)^{c_2}\cdots(x-x_m)^{c_m}
$$

$$
c_1+c_2+\cdots+c_m=n,x_1,x_2,\cdots,x_m \text{ đôi một khác nhau}
$$

Từ đó suy ra khái niệm tương tự ước chung lớn nhất với số nguyên, tức [**ước chung lớn nhất**](../number-theory/gcd.md) (greatest common divisor, gcd) của đa thức, có thể tính bằng thuật toán Euclid:

$$
\gcd(f,0)=f,\gcd(f,g)=\gcd(g,f\bmod g)
$$

Tính chất này còn đúng trong trường hợp tổng quát hơn:

> Với mọi trường $P$, trên vành đa thức $P[x]$,  
> đa thức phân tích duy nhất và có thể dùng thuật toán Euclid để tính gcd.
> Cần lưu ý rằng với đa thức trên một vành tổng quát, kết luận này không nhất thiết đúng.

Khi thuật toán Euclid áp dụng được, ta có thể dùng Euclid mở rộng để tìm một nghiệm riêng $(P(x),Q(x))$ của phương trình

$$
f(x)P(x)+g(x)Q(x)=\gcd(f(x),g(x))
$$

và dùng [định lý Bézout](../number-theory/bezouts.md) để xét tính khả giải của phương trình

$$
f(x)P(x)+g(x)Q(x)=h(x)
$$

[HALF-GCD](https://loj.ac/p/172) cho phép tính Euclid đa thức trong $O(n\log^2 n)$.

### Nghịch đảo nhân trong mô-đun đa thức

Trong mô-đun đa thức $h(x)$, đôi khi chuỗi lũy thừa $f(x)$ có nghịch đảo. Nghịch đảo chính là phần dư khi lấy nghịch đảo của $f(x)$ theo mô-đun $h(x)$.

Định nghĩa này tương đương với: với đa thức $f(x)$, nếu tồn tại $g(x)$ sao cho:

$$
\begin{aligned}
    f(x) g(x) & \equiv 1 \pmod{h(x)}
\end{aligned}
$$

thì $g(x)$ là **nghịch đảo (inverse element)** của $f(x)$ theo mô-đun $h(x)$. Khi Euclid đa thức áp dụng được, nghịch đảo tồn tại khi và chỉ khi $\gcd(f,g)=1$.

Trong mô-đun $h(x)$, nghịch đảo luôn là duy nhất. Nếu $\deg f(x) < \deg h(x)$, thì $g(x)$ và $f(x)$ là nghịch đảo của nhau.

Xét khái niệm “cắt cụt”, thường ký hiệu nghịch đảo theo mô-đun $x^n$ là $f^{-1}(x)$, và đây cũng là khái niệm mặc định về nghịch đảo trong phần sau. Nếu không nói rõ, mô-đun của nghịch đảo được hiểu là $x^n$.

???+ note "Chú"
    Một câu hỏi: có thể dùng các phép biến đổi nội suy trực tiếp để tính “nghịch đảo” không, ví dụ:
    
    $$
    IDFT\left(\frac{DFT(1)}{DFT(f(x))}\right)
    $$
    
    Câu trả lời là không. Theo giải thích ở trên, nghịch đảo tính bằng DFT (và nghịch biến đổi) là nghịch đảo theo mô-đun $x^n-1$, không phải theo mô-đun $x^n$. Hơn nữa, vì đa thức gốc có thể bằng $0$ tại một số điểm giá trị, phép tính này có thể không thực hiện được.

## Hàm sinh

Hàm sinh (generating function), còn gọi là hàm mẹ, là một chuỗi lũy thừa hình thức mà mỗi hệ số cung cấp thông tin về một dãy số.

Hàm sinh có nhiều loại khác nhau, nhưng phần lớn có thể biểu diễn dưới một dạng:

$$
F(x)=\sum_n a_nk_n(x)
$$

trong đó $k_n(x)$ gọi là hàm nhân (kernel). Các hàm nhân khác nhau dẫn tới các loại hàm sinh khác nhau với các tính chất khác nhau. Ví dụ:

1.  Hàm sinh thường: $k_n(x)=x^n$.
2.  Hàm sinh mũ: $k_n(x)=\dfrac{x^n}{n!}$.
3.  [Hàm sinh Dirichlet](../number-theory/dirichlet.md#dirichlet-生成函数): $k_n(x)=\dfrac{1}{n^x}$.

## Tài liệu tham khảo và đọc thêm

-   [**Picks's Blog**](https://picks.logdown.com)
-   [**Miskcoo's Space**](https://blog.miskcoo.com)
-   [**Polynomial ring - Wikipedia**](https://en.wikipedia.org/wiki/Polynomial_ring)
-   [**Formal power series - Wikipedia**](https://en.wikipedia.org/wiki/Formal_power_series#The_ring_of_formal_power_series)
-   《Khung lý thuyết tính toán hàm sinh trong thi đấu tin học》
