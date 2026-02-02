author: sshwy, FFjet, qz-cqy

## Giới thiệu

Thuật toán Euclid kiểu mới do Hong Huadun đề xuất trong chia sẻ trại đông 2016. Nó thường dùng để giải các bài toán tổng dạng

$$
\left\lfloor\dfrac{ai+b}{c}\right\rfloor
$$

(theo chỉ số $i$). Ý tưởng chính là tận dụng cấu trúc đệ quy của phân số để quy về bài toán nhỏ hơn, giải bằng đệ quy. Vì cấu trúc đệ quy của phân số có liên hệ trực tiếp với [thuật toán Euclid](./gcd.md#欧几里得算法) (xem [liên phân số](./continued-fraction.md#连分数表示的求法)), nên phương pháp này được gọi là thuật toán Euclid kiểu mới.

Vì [liên phân số](./continued-fraction.md) và [cây Stern–Brocot](./stern-brocot.md) cũng mô tả cấu trúc đệ quy của phân số, nên các bài toán giải được bằng Euclid kiểu mới thường cũng giải được bằng các phương pháp đó. So với chúng, Euclid kiểu mới thường dễ hiểu và cài đặt gọn hơn.

## Thuật toán Euclid kiểu mới

Ví dụ đơn giản nhất là bài toán:

$$
f(a,b,c,n)=\sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor,
$$

trong đó $a,b,c,n$ đều là số nguyên dương.

### Cách giải đại số

Trước hết, lấy $a,b$ mod $c$ để đưa về $0\le a,b<c$:

$$
\begin{aligned}
f(a,b,c,n)&=\sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor\\
&=\sum_{i=0}^n\left\lfloor
\frac{\left(\left\lfloor\frac{a}{c}\right\rfloor c+(a\bmod c)\right)i+\left(\left\lfloor\frac{b}{c}\right\rfloor c+(b\bmod c)\right)}{c}\right\rfloor\\
&=\sum_{i=0}^n\left(\left\lfloor\frac{a}{c}\right\rfloor i+\left\lfloor\frac{b}{c}\right\rfloor+\left\lfloor\frac{\left(a\bmod c\right)i+\left(b\bmod c\right)}{c}
\right\rfloor\right)\\
&=\frac{n(n+1)}{2}\left\lfloor\frac{a}{c}\right\rfloor
+(n+1)\left\lfloor\frac{b}{c}\right\rfloor+f(a\bmod c,b\bmod c,c,n).
\end{aligned}
$$

Xét bài toán sau khi chuyển đổi. Đặt

$$
m = \left\lfloor \frac{an+b}{c} \right\rfloor.
$$

Khi đó có dạng tổng kép:

$$
\sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor
=\sum_{i=0}^n\sum_{j=0}^{m-1}\left[j<\left\lfloor \frac{ai+b}{c} \right\rfloor\right].
$$

Đổi thứ tự tổng, cần tính phạm vi $i$ ứng với mỗi $j$. Biến đổi điều kiện:

$$
\begin{aligned}
&j<\left\lfloor \frac{ai+b}{c} \right\rfloor = \left\lceil \frac{ai+b+1}{c} \right\rceil-1\\
&\iff j + 1 < \left\lceil \frac{ai+b+1}{c} \right\rceil
\iff j+1< \frac{ai+b+1}{c} \\
&\iff \dfrac{cj+c-b-1}{a} < i
\iff \left\lfloor\dfrac{cj+c-b-1}{a}\right\rfloor < i.
\end{aligned}
$$

Suy ra:

$$
\begin{aligned}
f(a,b,c,n)&=\sum_{j=0}^{m-1}
\sum_{i=0}^n\left[i>\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor \right]\\
&=\sum_{j=0}^{m-1}\left(n-\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor\right)\\
&=nm-f\left(c,c-b-1,a,m-1\right).
\end{aligned}
$$

Đặt $(a',b',c',n')=(c,c-b-1,a,m-1)$, ta quay lại trường hợp $a'>c'$.

Kết hợp hai bước, trong quá trình $(a,c)$ liên tục lấy modulo rồi hoán đổi cho đến $a=0$. Điều này tương tự phép chia Euclid, nên gọi là thuật toán Euclid kiểu mới. Độ phức tạp là $O(\log\min\{a,c\})$.

Trong tính toán có thể có trường hợp $m=0$, khi đó đệ quy bên trong sẽ có $n=-1$. Điều này không ảnh hưởng kết quả. Nhưng nếu yêu cầu dừng ngay khi $m=0$ thì độ phức tạp cải thiện thành $O(\log\min\{a,c,n\})$.

??? note "Giải thích độ phức tạp"
    Dùng tương tự với thuật toán Euclid, ta dễ thấy độ phức tạp là $O(\log\min\{a,c\})$. Cần giải thích thêm rằng nếu dừng khi $m=0$ thì độ phức tạp cũng là $O(\log n)$.
    
    Đặt $m=\lfloor(an+b)/c\rfloor$, ký hiệu $S=mn$, $k=m/n$. Chúng tương ứng với diện tích điểm lưới (xem mục sau) và độ dốc đường thẳng. Với $n$ đủ lớn, gần đúng $k\doteq a/c$.
    
    Xét biến thiên của $S$ và $k$ trong thuật toán. Bước lấy modulo giữ $n$ không đổi, $k$ gần đúng từ $a/c$ thành $(a\bmod c)/c$, tức $k\to k-\lfloor k\rfloor$, và $S$ cũng giảm theo $(k-\lfloor k\rfloor)$. Bước hoán đổi trục giữ $S$ xấp xỉ không đổi, còn $k$ thành nghịch đảo. Nếu sau hai bước $(k,S)\to(k',S')$ thì $k'=(k-\lfloor k\rfloor)^{-1}$ và $S'=(k-\lfloor k\rfloor)S$.
    
    Vì $1\le\lfloor k'\rfloor\le k'<\lfloor k'\rfloor+1$, nên sau hai vòng lặp, hệ số giảm ít nhất là
    
    $$
    (k'-\lfloor k'\rfloor)(k-\lfloor k\rfloor) = 1-\dfrac{\lfloor k'\rfloor}{k'} < 1-\dfrac{\lfloor k'\rfloor}{\lfloor k'\rfloor+1} = \dfrac{1}{\lfloor k'\rfloor+1}\le \dfrac{1}{2}.
    $$
    
    Do đó sau $O(\log S)$ vòng lặp thuật toán kết thúc. Từ vòng 2 trở đi, $S$ không vượt quá giá trị sau bước modulo ở vòng trước, mà giá trị đó xấp xỉ $kn^2$ và $k<1$, nên $O(\log S)\subseteq O(\log n)$. Suy ra kết luận.

Cài đặt mẫu:

??? example "Cài đặt mẫu（[Library Checker - Sum of Floor of Linear](https://judge.yosupo.jp/problem/sum_of_floor_of_linear)）"
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-0.cpp:full-text"
    ```

### Trực quan hình học

Thuật toán này cũng có thể hiểu theo hình học. Nó giải bài toán đếm điểm nguyên dưới đường thẳng.

Hình bên trái cho thấy tổng tương ứng số điểm nguyên dưới đường thẳng

$$
y = \dfrac{ax+b}{c}
$$

nằm trên trục $x$ (không gồm trục $x$) và có hoành độ trong $[0,n]$.

![](./images/euclidean-1.svg)

Đầu tiên, loại bỏ phần nguyên của hệ số góc và tung độ gốc. Đây tương đương tính riêng số điểm màu xanh ở hình giữa. Khi hệ số góc và tung độ gốc là số nguyên, các điểm xanh tạo thành hình thang với số điểm theo cấp số cộng, dễ tính. Sau khi loại, phần còn lại trùng với các điểm đỏ hình bên phải. Khi đó bài toán trở thành trường hợp hệ số góc và tung độ gốc đều nhỏ hơn 1. Vì chiều cao hình thang là $n+1$ và đáy có độ dài $\lfloor b/c\rfloor$ và $(\lfloor a/c\rfloor n+\lfloor b/c\rfloor)$, nên theo công thức diện tích, bước này gộp thành:

$$
f(a,b,c,n) = f(a\bmod c,b\bmod c,c,n) + \dfrac{1}{2}(n+1)\left(\left\lfloor\dfrac{b}{c}\right\rfloor+\left(\left\lfloor\dfrac{a}{c}\right\rfloor n+\left\lfloor\dfrac{b}{c}\right\rfloor\right)\right).
$$

Sau đó, lật trục hoành và tung. Như hình trái, các điểm đỏ và xanh tạo thành một lưới chữ nhật $n\times m$ với $m=\lfloor(an+b)/c\rfloor$. Số điểm đỏ bằng tổng điểm trong chữ nhật trừ số điểm xanh. Sau khi lật, lưới điểm xanh ở nửa trái trở thành lưới điểm đỏ dưới một đường thẳng, và hệ số góc lớn hơn 1, quay về trường hợp đã xử lý.

![](./images/euclidean-2.svg)

Mấu chốt là xác định phương trình đường thẳng mới. Lật trục của đường $y=(ax+b)/c$ cho đường $y=(cx-b)/a$, nhưng đường này đi qua các điểm nguyên, dẫn đến đếm trùng. Vì vậy cần tịnh tiến xuống một chút, thành $y=(cx-b-1)/a$, để các điểm dưới đường đúng là các điểm xanh trước khi lật.

Còn một chi tiết: đường mới có tung độ gốc âm, chưa về dạng ban đầu. Để đưa về tung độ không âm, tịnh tiến sang trái một đơn vị. Không bị thiếu điểm vì trước đó không có điểm có tung độ 0, nên sau lật cũng không có điểm có hoành độ 0. Cuối cùng được đường $y=(cx+c-b-1)/a$, và giới hạn hoành độ từ $m$ thành $m-1$. Bước này gộp thành:

$$
f(a,b,c,n) = mn - f(c,c-b-1,a,m-1).
$$

Đệ quy này đúng vì:

-   Hệ số góc liên tục lấy phần thập phân rồi lấy nghịch đảo, tương đương khai triển [liên phân số](./continued-fraction.md#连分数表示的求法) của $k=a/c$. Độ dài khai triển của phân số hữu tỉ là $O(\log\min\{a,c\})$, nên quá trình dừng trong $O(\log\min\{a,c\})$ bước.
-   Mỗi lần lật trục, hệ số góc < 1 nên trực giác cho thấy $m<n$, tức phạm vi hoành độ giảm. Phân tích trước đó cho thấy sau mỗi hai vòng, $n$ giảm ít nhất một nửa, nên kết thúc trong $O(\log n)$ bước.

Do đó độ phức tạp với hệ số góc hữu tỉ là $O(\log\min\{a,c,n\})$.

Bằng trực quan hình học tương tự, có thể mở rộng sang hệ số góc vô tỉ; xem ví dụ ở phần sau.

### Ví dụ

???+ example "[【模板】类欧几里得算法](https://www.luogu.com.cn/problem/P5170)"
    Nhiều truy vấn. Cho $a,b,c,n$ nguyên dương, tính
    
    $$
    \begin{aligned}
    f(a,b,c,n) &= \sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor,\\
    g(a,b,c,n) &= \sum_{i=0}^ni\left\lfloor \frac{ai+b}{c} \right\rfloor,\\
    h(a,b,c,n) &= \sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor^2.
    \end{aligned}
    $$

??? note "Lời giải 1"
    Tương tự $f$, ta suy ra biểu thức đệ quy của $g,h$.
    
    Trước hết dùng modulo đưa về $0\le a,b<c$:
    
    $$
    \begin{aligned}
    g(a,b,c,n)
    &=g(a\bmod c,b\bmod c,c,n)+\left\lfloor\frac{a}{c}\right\rfloor\frac{n(n+1)(2n+1)}{6}+\left\lfloor\frac{b}{c}\right\rfloor\frac{n(n+1)}{2}, \\
    h(a,b,c,n)&=h(a\bmod c,b\bmod c,c,n)\\
    &\quad+2\left\lfloor\frac{b}{c}\right\rfloor f(a\bmod c,b\bmod c,c,n)
    +2\left\lfloor\frac{a}{c}\right\rfloor g(a\bmod c,b\bmod c,c,n)\\
    &\quad+\left\lfloor\frac{a}{c}\right\rfloor^2\frac{n(n+1)(2n+1)}{6}+\left\lfloor\frac{b}{c}\right\rfloor^2(n+1)
    +\left\lfloor\frac{a}{c}\right\rfloor\left\lfloor\frac{b}{c}\right\rfloor n(n+1).
    \end{aligned}
    $$
    
    Sau đó, đổi thứ tự tổng. Đặt
    
    $$
    m = \left\lfloor \frac{an+b}{c} \right\rfloor.
    $$
    
    Với $g$:
    
    $$
    \begin{aligned}
    g(a,b,c,n)&=\sum_{i=0}^ni\left\lfloor \frac{ai+b}{c} \right\rfloor\\
    &=\sum_{i=0}^n \sum_{j=0}^{m-1}i
    \left[j<\left\lfloor\frac{ai+b}{c}\right\rfloor\right] \\
    &=\sum_{j=0}^{m-1}\sum_{i=0}^n i\left[i>\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor \right]\\
    &=\sum_{j=0}^{m-1}\dfrac{1}{2}\left(\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor+n+1\right)\left(n-\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor\right)\\
    &=\dfrac{1}{2}mn(n+1) - \dfrac{1}{2}\sum_{j=0}^{m-1}\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor - \dfrac{1}{2}\sum_{j=0}^{m-1}\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor^2\\
    &=\dfrac{1}{2}mn(n+1) - \dfrac{1}{2}f(c,c-b-1,a,m-1) - \dfrac{1}{2}h(c,c-b-1,a,m-1).
    \end{aligned}
    $$
    
    Với $h$:
    
    $$
    \begin{aligned}
    h(a,b,c,n)&=\sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor^2\\
    &=\sum_{i=0}^n\sum_{j=0}^{m-1}(2j+1)\left[j<\left\lfloor\frac{ai+b}{c}\right\rfloor\right]\\
    &=\sum_{j=0}^{m-1}\sum_{i=0}^n(2j+1)\left[i>\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor \right]\\
    &=\sum_{j=0}^{m-1}(2j+1)\left(n-\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor\right)\\
    &=nm^2 - \sum_{j=0}^{m-1}\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor - 2\sum_{j=0}^{m-1}j\left\lfloor\frac{cj+c-b-1}{a}\right\rfloor\\
    &=nm^2 - f(c,c-b-1,a,m-1) - 2g(c,c-b-1,a,m-1).
    \end{aligned}
    $$
    
    Theo trực quan hình học, các tổng phi tuyến này tương ứng gán trọng số $w(i,j)$ cho từng điểm $(i,j)$. Ngoài trọng số, quá trình tính vẫn giống nhau. Với trọng số, tổng quát có:
    
    $$
    \sum_{i=0}^ni^r\left\lfloor \frac{ai+b}{c} \right\rfloor^s = \sum_{i=0}^n\sum_{j=0}^{m-1} i^r\left((j+1)^s-j^s\right)\left[j<\left\lfloor\frac{ai+b}{c}\right\rfloor\right].
    $$
    
    Bài này còn có điểm là $g$ và $h$ đệ quy đan xen. Vì vậy cần đệ quy đồng thời bộ ba $(f,g,h)$.
    
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-1.cpp"
    ```

???+ example "[\[清华集训 2014\] Sum](https://www.luogu.com.cn/problem/P5172)"
    Nhiều truy vấn. Cho $n$ và $r$ nguyên dương, tính
    
    $$
    \sum_{d=1}^n(-1)^{\lfloor d\sqrt{r}\rfloor}.
    $$

??? note "Lời giải 1"
    Nếu $r$ là số chính phương, khi $\sqrt{r}$ chẵn thì tổng là $n$, ngược lại tổng dao động giữa $0$ và $-1$ theo chẵn lẻ của $n$. Xét $r$ không là chính phương.
    
    Để áp dụng Euclid kiểu mới, chuyển tổng về dạng quen thuộc:
    
    $$
    \begin{aligned}
    \sum_{d=1}^n(-1)^{\lfloor d\sqrt{r}\rfloor} &= \sum_{d=1}^n\left(1 - 2(\lfloor d\sqrt{r}\rfloor\bmod 2)\right)\\
    &= n - 2\sum_{d=1}^n\left(\lfloor d\sqrt{r}\rfloor - 2\left\lfloor\dfrac{\lfloor d\sqrt{r}\rfloor}{2}\right\rfloor\right) \\
    &= n - 2\sum_{d=1}^n\lfloor d\sqrt{r}\rfloor + 4 \sum_{d=1}^n\left\lfloor\dfrac{d\sqrt{r}}{2}\right\rfloor\\
    &= n - 2f(n,1,0,1) + 4f(n,1,0,2)
    \end{aligned}
    $$
    
    với
    
    $$
    f(a,b,c,n) = \sum_{i=1}^n\left\lfloor\dfrac{a\sqrt{r}+b}{c}i\right\rfloor.
    $$
    
    Ở đây hệ số góc không còn hữu tỉ. Đặt
    
    $$
    k = \dfrac{a\sqrt{r}+b}{c}.
    $$
    
    Xét hai trường hợp. Nếu $k\ge 1$:
    
    $$
    \begin{aligned}
    f(a,b,c,n) &= \sum_{i=1}^n \lfloor ki\rfloor = \sum_{i=1}^n \lfloor(k-\lfloor k\rfloor)i\rfloor + \lfloor k\rfloor \sum_{i=1}^ni\\
    &= \lfloor k\rfloor\dfrac{n(n+1)}{2} + f(a,b-c\lfloor k\rfloor,c,n).
    \end{aligned}
    $$
    
    Bài toán trở thành hệ số góc < 1. Nếu $k<1$, đặt $m=\lfloor nk\rfloor$:
    
    $$
    \begin{aligned}
    f(a,b,c,n) &= \sum_{i=1}^n \lfloor ki\rfloor = \sum_{i=1}^n\sum_{j=1}^m[j\le\lfloor ki\rfloor]\\
    &= \sum_{j=1}^m\sum_{i=1}^n[i>\lfloor k^{-1}j\rfloor] = nm - \sum_{j=1}^m\sum_{i=1}^n[i\le\lfloor k^{-1}j\rfloor].
    \end{aligned}
    $$
    
    Đổi $i$ và $j$ đơn giản hơn vì đường $y=kx$ không có điểm nguyên nào ngoài gốc. Cần viết lại dưới dạng $f(a,b,c,n)$, tức tìm $a',b',c'$ sao cho
    
    $$
    k^{-1} = \dfrac{a'\sqrt{r}+b'}{c'}.
    $$
    
    Hữu tỉ hóa mẫu:
    
    $$
    k^{-1} = \dfrac{c}{a\sqrt{r}+b} = \dfrac{ca\sqrt{r}-cb}{a^2r-b^2}.
    $$
    
    Suy ra
    
    $$
    a'=ca,~b'=-cb,~c'=a^2r-b^2.
    $$
    
    Vì vậy
    
    $$
    f(a,b,c,n) = nm - f(ca,-cb,a^2r-b^2,m).
    $$
    
    Để tránh tràn số, mỗi lần chia $a,b,c$ cho gcd. Quá trình này trùng với quá trình khai triển liên phân số của $k$, nên nếu $\gcd(a,b,c)=1$ thì các giá trị luôn trong phạm vi số nguyên. Dù $(a,b,c,n)$ không tràn, $f(a,b,c,n)$ có thể vượt 64 bit trong dữ liệu bài này; để tràn tự nhiên là được, kết quả cuối luôn nằm trong $[-n,n]$.
    
    Dù hệ số góc không về 0, độ phức tạp vẫn là $O(\log n)$ theo phân tích trước đó.
    
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-2.cpp"
    ```

???+ example "[Fraction](https://www.luogu.com.cn/problem/P5179)"
    Cho $a,b,c,d$ nguyên dương, tìm phân số tối giản $p/q$ thỏa $a/b<p/q<c/d$ sao cho $(q,p)$ là nhỏ nhất theo thứ tự từ điển.

??? note "Lời giải"
    Bài này là ứng dụng kinh điển của [cây Stern–Brocot](./stern-brocot.md), lời giải liên quan tại [đây](./continued-fraction.md#连分数的树). Vì chỉ dựa vào cấu trúc đệ quy của phân số, nên cũng có thể giải bằng Euclid kiểu mới.
    
    Nếu giữa $a/b$ và $c/d$ (không kể biên) tồn tại ít nhất một số nguyên, thì lấy $(q,p)=(1,\lfloor a/b\rfloor+1)$.
    Ngược lại, ta có
    
    $$
    \left\lfloor\dfrac{a}{b}\right\rfloor \le \dfrac{a}{b} <\dfrac{p}{q} <\dfrac{c}{d}\le\left\lfloor\dfrac{a}{b}\right\rfloor+1.
    $$
    
    Từ đây biết phần nguyên của $p/q$ là $\lfloor a/b\rfloor$. Loại phần nguyên, lấy nghịch đảo để xác định phần thập phân. Đây chính là [cách xác định liên phân số](./continued-fraction.md#连分数表示的求法). Nếu đáp án là $p/q$, độ phức tạp là $O(\log\min\{p,q\})$.
    
    Có một chi tiết: sau khi lấy nghịch đảo, phân số nhỏ nhất theo thứ tự từ điển có còn là nhỏ nhất trước đó không? Giả sử không, gọi $p/q$ là nhỏ nhất theo $(q,p)$, nhưng $r/s\neq p/q$ là nhỏ nhất theo $(p,q)$. Khi đó $r<p$ và $q<s$, suy ra
    
    $$
    \dfrac{a}{b} < \dfrac{r}{s} < \dfrac{r}{q} < \dfrac{p}{q} < \dfrac{c}{d}.
    $$
    
    Khi đó $r/q$ nhỏ hơn nghiệm hiện tại theo mọi thứ tự từ điển, mâu thuẫn. Do đó thuật toán đúng.
    
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-3.cpp"
    ```

## Thuật toán Euclid tổng quát

Phần trước suy luận khá dài, và các tổng chủ yếu là đếm điểm nguyên dưới đường thẳng có trọng số. Phần này đưa ra phương pháp tổng quát hơn, trừu tượng hóa quá trình trên để giải nhiều bài hơn, gọi là thuật toán Euclid tổng quát. Nó vẫn dùng cấu trúc đệ quy của phân số, nhưng cách quy bài khác một chút.

Xét bài toán cổ điển:

$$
f(a,b,c,n)=\sum_{i=1}^n\left\lfloor \frac{ai+b}{c} \right\rfloor,
$$

với $a,b,c,n$ nguyên dương.

### Chuyển đổi bài toán

Với đoạn thẳng tham số $(a,b,c,n)$:

$$
y = \frac{ax+b}{c},~0< x\le n.
$$

Định nghĩa một chuỗi ký tự $S$ gồm $U$ và $R$ (gọi là **chuỗi thao tác**):

-   Chuỗi có đúng $n$ ký tự $R$ và $m=\lfloor(an+b)/c\rfloor$ ký tự $U$;
-   Số $U$ trước ký tự $R$ thứ $i$ đúng bằng $\lfloor(ai+b)/c\rfloor$, với $i=1,\cdots,n$.

Theo trực quan hình học, từ gốc tọa độ, mỗi lần đi qua một đường lưới dọc thì ghi $R$, mỗi lần đi qua một đường lưới ngang thì ghi $U$:

![](./images/euclidean-universal.svg)

Cần xử lý vài trường hợp đặc biệt:

-   Nếu đi qua điểm nguyên (vừa lên vừa sang), phải ghi $U$ trước rồi $R$;
-   Ở đầu chuỗi, ngoài số lần đi lên trong đoạn $(0,1]$, còn phải thêm $\lfloor b/c\rfloor$ ký tự $U$;
-   Kết thúc chuỗi không được có $U$ dư.

Nếu mô tả hình học chưa rõ, có thể xem định nghĩa đại số ở trên.

Ý tưởng của thuật toán Euclid tổng quát là xem $U$ và $R$ như các phần tử của một [nửa nhóm đơn vị](../algebra/basic.md#群), chuỗi thao tác là tích của các phần tử, và kết quả liên quan đến tích đó.

Ví dụ, ta định nghĩa vector trạng thái $v=(1,y,\sum y)$, biểu thị sau khi đi qua vài lần lưới, tung độ hiện tại và tổng cần tính. Ban đầu $v=(1,0,0)$. Mỗi lần đi lên, $y$ tăng 1, tức nhân phải bởi ma trận

$$
U = \begin{pmatrix}1 & 1 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1\end{pmatrix}.
$$

Mỗi lần đi sang phải, tổng tăng thêm $y$, tức nhân phải bởi

$$
R = \begin{pmatrix}1 & 0 & 0 \\ 0 & 1 & 1 \\ 0 & 0 & 1\end{pmatrix}.
$$

Vậy trạng thái cuối là $(1,0,0)S$, với $S$ là tích các ma trận. Kết quả là thành phần thứ ba của trạng thái cuối.

Ngoài ma trận, có thể định nghĩa mỗi đoạn thao tác đóng góp $(x,y,\sum y)$, coi các thành phần như hàm của chuỗi $S$: $(x(S),y(S),(\sum y)(S))$. Trong đó $x(S)$ là số ký tự $R$, $y(S)$ là số ký tự $U$. Định nghĩa tổng trên chuỗi:

$$
\sum_S f := \sum\{f(S_{[1,r]}):S_r=R\}.
$$

với $S_r$ là ký tự thứ $r$ và $S_{[1,r]}$ là tiền tố độ dài $r$. Ví dụ:

$$
\sum_S 1 = x,~ \sum_S x = \dfrac{1}{2}x(x+1).
$$

$\sum y$ chính là tổng các $y$ tại mọi tiền tố kết thúc bằng $R$, tức tổng các $\lfloor(ai+b)/c\rfloor$ với $i=1,\cdots,n$.

Ban đầu $U=(0,1,0)$, $R=(1,0,0)$. Định nghĩa phép nhân:

$$
(x_1,y_1,s_1)\cdot (x_2,y_2,s_2) = (x_1+x_2,y_1+y_2,s_1+s_2+x_2y_1).
$$

Trong đó:

$$
\sum_{S_1+S_2}y = \sum_{S_1}y + \sum_{S_2}(y+y_1) = \sum_{S_1}y + \sum_{S_2}y + y_1\sum_{S_2}1 = s_1+s_2+x_2y_1.
$$

Phép nhân này kết hợp được, có đơn vị $(0,0,0)$, nên tạo thành nửa nhóm đơn vị. Kết quả là thành phần thứ ba.

Hai cách đều đúng, nhưng vì ma trận có nhiều thông tin dư, hằng số lớn, nên cách thứ hai thực tế hơn.

### Quy trình thuật toán

Khác với Euclid kiểu mới, Euclid tổng quát gộp thao tác theo lô. Gọi tích chuỗi là

$$
F(a,b,c,n,U,R).
$$

Các bước:

-   Nếu $b\ge c$, đầu chuỗi có $\lfloor b/c\rfloor$ ký tự $U$, tính và loại chúng. Khi đó số $U$ trước $R$ thứ $i$ là
    
    $$
    \left\lfloor\dfrac{ai+b}{c}\right\rfloor - \left\lfloor\dfrac{b}{c}\right\rfloor = \left\lfloor\dfrac{ai+(b\bmod c)}{c}\right\rfloor.
    $$
    
    Tương đương đổi tham số từ $(a,b,c,n)$ sang $(a,b\bmod c,c,n)$:
    
    $$
    F(a,b,c,n,U,R) = U^{\lfloor b/c\rfloor}F(a,b\bmod c,c,n,U,R).
    $$

-   Nếu $a\ge c$, mỗi $R$ có ít nhất $\lfloor a/c\rfloor$ ký tự $U$ trước nó, nên gộp thành $U^{\lfloor a/c\rfloor}R$. Khi đó
    
    $$
    \left\lfloor\dfrac{ai+b}{c}\right\rfloor - \left\lfloor\dfrac{a}{c}\right\rfloor i = \left\lfloor\dfrac{(a\bmod c)i+b}{c}\right\rfloor.
    $$
    
    Tương đương đổi tham số từ $(a,b,c,n)$ sang $(a\bmod c,b,c,n)$:
    
    $$
    F(a,b,c,n,U,R) = F(a\bmod c,b,c,n,U,U^{\lfloor a/c\rfloor}R).
    $$

-   Trường hợp còn lại cần lật trục, tức hoán đổi $U$ và $R$, nhưng tham số mới phải tính kỹ. Ta cần $(a',b',c',n')$ sao cho trong chuỗi trước, số $R$ trước $U$ thứ $j$ là $\lfloor(a'j+b')/c'\rfloor$, tổng có $n'$ ký tự $U$. Theo định nghĩa:
    
    $$
    n'=\left\lfloor\dfrac{an+b}{c}\right\rfloor = m,
    $$
    
    còn số $R$ trước $U$ thứ $j$ là số lớn nhất $i$ thỏa:
    
    $$
    \begin{aligned}
    \left\lfloor\dfrac{ai+b}{c}\right\rfloor < j 
    &\iff \dfrac{ai+b}{c} < j \iff i < \dfrac{cj-b}{a} \\
    &\iff i < \left\lceil\dfrac{cj-b}{a}\right\rceil = \left\lfloor\dfrac{cj-b - 1}{a}\right\rfloor + 1.
    \end{aligned}
    $$
    
    Do đó $i = \lfloor(cj-b-1)/a\rfloor$. Suy luận giống phần Euclid kiểu mới, dùng tính chất hàm lấy phần nguyên.
    
    Có hai chi tiết:
    
    -   Hệ số chặn $-(b+1)/a$ âm. Nếu tịnh tiến đường thẳng sang trái một đơn vị, hệ số chặn thành không âm vì $(c-b-1)/a\ge 0$. Do đó tách đoạn đầu $R^{\lfloor(c-b-1)/a\rfloor}U$, rồi chỉ hoán đổi phần còn lại.
    -   Sau khi hoán đổi, cuối chuỗi có $U$ dư. Vì thế trước khi hoán đổi cần tách đoạn cuối $R$, số lượng là $n-\lfloor(cm-b-1)/a\rfloor$.
    
    Sau khi bỏ đầu và cuối, số $R$ trước $U$ thứ $j$ trở thành:
    
    $$
    \left\lfloor\dfrac{c(j+1)-b-1}{a}\right\rfloor - \left\lfloor\dfrac{c-b-1}{a}\right\rfloor = \left\lfloor\dfrac{cj+(c-b-1)\bmod a}{a}\right\rfloor.
    $$
    
    Nhớ rằng số $U$ trước hoán đổi là $m=\lfloor(an+b)/c\rfloor$. Việc tịnh tiến trái một đơn vị cần $m>0$. Do đó có hai trường hợp:
    
    -   Với $m>0$, sau khi xử lý hai điểm trên, chuỗi sau hoán đổi tương ứng tham số $(c,(c-b-1)\bmod a,a,m-1)$:
    
        $$
        F(a,b,c,n,U,R) = R^{\lfloor(c-b-1)/a\rfloor}UF(c,(c-b-1)\bmod a,a,m-1,R,U)R^{n-\lfloor(cm-b-1)/a\rfloor}.
        $$
    
    -   Với $m=0$, chuỗi chỉ có $n$ ký tự $R$, không cần hoán đổi:
    
        $$
        F(a,b,c,n,U,R) = R^n.
        $$
    
        Khác Euclid kiểu mới, trường hợp này cần xử lý riêng, nếu không sẽ gặp lũy thừa âm.

Nhờ đó, bài toán được giải đệ quy.

Giả sử phép nhân trong nửa nhóm đơn vị $O(1)$. Nếu mọi lũy thừa dùng [nhân nhanh](../binary-exponentiation.md), độ phức tạp là $O(\log\max\{a,c\}+\log(b/c))$.[^complexity]

??? note "Giải thích độ phức tạp"
    So với Euclid (kiểu mới), Euclid tổng quát chỉ thêm bước lũy thừa nhanh. Các phần còn lại có độ phức tạp $O(\log\min\{a,c,n\})$. Ta cần ước lượng tổng chi phí lũy thừa.
    
    Trừ vòng đầu, luôn có $b<c$, nên mỗi vòng cần ba lần lũy thừa:
    
    $$
    O\left(\log\left\lfloor\dfrac{a}{c}\right\rfloor+\log\left\lfloor\dfrac{c-b_1-1}{a_1}\right\rfloor+\log\left(n-\left\lfloor\dfrac{cm-b_1-1}{a_1}\right\rfloor\right)\right),
    $$
    
    trong đó $a_1=a\bmod c$, $b_1=b\bmod c$, $m=\lfloor(a_1n+b_1)/c\rfloor$. Hai hạng sau có ước lượng:
    
    $$
    \begin{aligned}
    \dfrac{c-b_1-1}{a_1} &\le \dfrac{c}{a_1},\\
    n-\left\lfloor\dfrac{cm-b_1-1}{a_1}\right\rfloor &\le n - \dfrac{cm-b_1-1}{a_1} + 1 \\
    &\le n - \dfrac{c((a_1n+b_1)/c-1)-b_1-1}{a_1} +1 \\
    &= \dfrac{c+1}{a_1}+1.
    \end{aligned}
    $$
    
    Nên cả hai là $O(\log(c/a_1))$.
    
    Mỗi vòng, tham số đổi từ $(a,\cdot,c,\cdot)$ sang $(c,\cdot,a\bmod c,\cdot)$, và chi phí vòng đó là
    
    $$
    O\left(\log\dfrac{a}{c}+\log\dfrac{c}{a\bmod c}\right).
    $$
    
    Cộng dồn qua các vòng, các hạng triệt tiêu, tổng là $O(\log a+\log c)=O(\log\max\{a,c\})$.
    
    Cộng thêm vòng đầu với $U^{\lfloor b/c\rfloor}$ có $O(\log(b/c))$, được tổng $O(\log\max\{a,c\}+\log(b/c))$.

Thuật toán Euclid tổng quát có thể viết thành template, khi xử lý bài cụ thể chỉ cần thay kiểu `T`.

???+ example "Cài đặt tham khảo"
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-4.cpp:euclidean"
    ```

Dùng Euclid tổng quát cho bài mẫu:

??? example "Cài đặt mẫu（[Library Checker - Sum of Floor of Linear](https://judge.yosupo.jp/problem/sum_of_floor_of_linear)）"
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-4.cpp:full-text"
    ```

### Ví dụ

???+ example "[【模板】类欧几里得算法](https://www.luogu.com.cn/problem/P5170)"
    Nhiều truy vấn. Cho $a,b,c,n$ nguyên dương, tính
    
    $$
    \begin{aligned}
    f(a,b,c,n) &= \sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor,\\
    g(a,b,c,n) &= \sum_{i=0}^ni\left\lfloor \frac{ai+b}{c} \right\rfloor,\\
    h(a,b,c,n) &= \sum_{i=0}^n\left\lfloor \frac{ai+b}{c} \right\rfloor^2.
    \end{aligned}
    $$

??? note "Lời giải 2"
    Để dùng template Euclid tổng quát, trước hết tách hạng $i=0$. Phần còn lại coi như tính $\sum y,\sum xy,\sum y^2$ với đoạn thẳng tham số $(a,b,c,n)$. Như đã nói, có hai cách chuyển chuỗi thao tác sang phần tử nửa nhóm.
    
    **Ma trận**: vector trạng thái $(1,x,y,xy,y^2,\sum y,\sum xy,\sum y^2)$, khởi tạo $(1,0,0,0,0,0,0,0)$, hai thao tác:
    
    $$
    U =
    \begin{pmatrix}
    1 & 0 & 1 & 0 & 1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 1 & 0 & 0 & 0 & 0 \\
    0 & 0 & 1 & 0 & 2 & 0 & 0 & 0 \\
    0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 
    \end{pmatrix},~
    R = 
    \begin{pmatrix}
    1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
    0 & 0 & 1 & 1 & 0 & 1 & 1 & 0 \\
    0 & 0 & 0 & 1 & 0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 0 & 1 & 0 & 0 & 1 \\
    0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 
    \end{pmatrix}.
    $$
    
    Đáp án là ba thành phần cuối của vector thu được khi nhân các ma trận thao tác.
    
    Cách này có hằng số rất lớn nên không qua được bài, chỉ nêu để hiểu.
    
    **Gộp đóng góp**: đóng góp của một đoạn là $(x,y,\sum y,\sum xy,\sum y^2)$. Hai thao tác:
    
    $$
    U = (0,1,0,0,0),~ R = (1,0,0,0,0).
    $$
    
    Gộp đóng góp:
    
    $$
    \begin{aligned}
    \sum_{S_1+S_2} y 
    &= \sum_{S_1}y + \sum_{S_2}(y+y_1) = \sum_{S_1}y + \sum_{S_2}y + x_2y_1,\\
    \sum_{S_1+S_2} xy
    &= \sum_{S_1}xy + \sum_{S_2}(x+x_1)(y+y_1) \\
    &= \sum_{S_1}xy + \sum_{S_2}xy + x_1\sum_{S_2}y + y_1\sum_{S_2}x + x_1y_1\sum_{S_2}1\\
    &= \sum_{S_1}xy + \sum_{S_2}xy + x_1\sum_{S_2}y + \dfrac{1}{2}x_2(x_2+1)y_1 + x_1x_2y_1,\\
    \sum_{S_1+S_2}y^2
    &= \sum_{S_1}y^2 + \sum_{S_2}(y+y_1)^2 \\
    &= \sum_{S_1}y^2 + \sum_{S_2}y^2 + 2y_1\sum_{S_2}y + y_1^2\sum_{S_2}1  \\
    &= \sum_{S_1}y^2 + \sum_{S_2}y^2 + 2y_1\sum_{S_2}y + x_2y_1^2.
    \end{aligned}
    $$
    
    Do đó định nghĩa phép nhân:
    
    $$
    \begin{aligned}
    &(x_1,y_1,s_1,t_1,u_1)\cdot(x_2,y_2,s_2,t_2,u_2)\\
    &= (x_1+x_2,y_1+y_2,s_1+s_2+x_2y_1,\\
    &\qquad t_1+t_2+x_1s_2+(1/2)x_2(x_2+1)y_1+x_1x_2y_1,\\
    &\qquad u_1+u_2+2y_1s_2+x_2y_1^2).
    \end{aligned}
    $$
    
    Có thể chứng minh các vector này tạo thành nửa nhóm đơn vị với đơn vị $(0,0,0,0,0)$.
    
    Tổng quát:
    
    $$
    \begin{aligned}
    \sum_{S_1+S_2}x^ry^s &= \sum_{S_1}x^ry^s + \sum_{S_2}(x+x_1)^r(y+y_1)^s \\
    &= \sum_{S_1}x^ry^s + \sum_{i=0}^r\sum_{j=0}^s\binom{r}{i}\binom{s}{j}x_1^{r-i}y_1^{s-j}\sum_{S_2}x^iy^j.
    \end{aligned}
    $$
    
    Chỉ cần duy trì các bậc thấp hơn là tính được tổng quát.
    
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-5.cpp"
    ```

???+ example "[\[清华集训 2014\] Sum](https://www.luogu.com.cn/problem/P5172)"
    Nhiều truy vấn. Cho $n$ và $r$ nguyên dương, tính
    
    $$
    \sum_{d=1}^n(-1)^{\lfloor d\sqrt{r}\rfloor}.
    $$

??? note "Lời giải 2"
    Xử lý riêng trường hợp $r$ là chính phương như trước. Dưới đây chỉ xét $r$ không là chính phương.
    
    Có nhiều cách áp dụng Euclid tổng quát. Ví dụ định nghĩa biến đổi tuyến tính:
    
    $$
    U(x) = -x,~ R(x) = x + 1.
    $$
    
    Phép nhân là hợp thành biến đổi. Đáp án là giá trị của biến đổi cuối cùng tại $x=0$.
    
    Hoặc định nghĩa đóng góp $((-1)^y,\sum(-1)^y)$, hai thao tác:
    
    $$
    U = (0,-1),~ R = (1,1).
    $$
    
    Gộp đóng góp:
    
    $$
    (u_1,v_1)\cdot(u_2,v_2) = (u_1u_2,v_1+u_1v_2).
    $$
    
    Dễ kiểm tra tạo thành nửa nhóm đơn vị với đơn vị $(0,1)$. Đáp án là thành phần thứ hai của tích.
    
    Hai cách là tương đương: nếu viết biến đổi tuyến tính $f(x)=u+vx$, thì hợp thành tương ứng đúng với phép nhân trên. Tức hai nửa nhóm này đẳng cấu.
    
    Trong bài, tham số là $(k,n)$, với $k\in\mathbf R$ là hệ số góc. Gọi tích chuỗi thao tác là $F(k,n,U,R)$. Khi đó:
    
    -   Nếu $k\ge 1$, mỗi $R$ có ít nhất $\lfloor k\rfloor$ ký tự $U$ trước, nên
    
        $$
        F(k,n,U,R) = F(k-\lfloor k\rfloor,n,U,U^{\lfloor k\rfloor} R).
        $$
    -   Nếu $k<1$, hoán đổi $U$ và $R$, bỏ $U$ dư ở cuối (tức $R$ trước hoán đổi), nên
    
        $$
        F(k,n,U,R) = F(k^{-1},m,R,U)R^{n-\lfloor k^{-1}m\rfloor}.
        $$
    
    Quá trình lặp $k$ chính là khai triển liên phân số của $\sqrt{r}$. Có thể dùng [thuật toán PQa](./pell-equation.md#pqa-算法). Quá trình khai triển và Euclid tổng quát có thể thực hiện đồng thời.
    
    Độ phức tạp vẫn là $O(\log n)$.
    
    ```cpp
    --8<-- "docs/math/code/euclidean/euclidean-6.cpp"
    ```

## Bài tập

Bài mẫu:

-   [Library Checker - Sum of Floor of Linear](https://judge.yosupo.jp/problem/sum_of_floor_of_linear)
-   [Luogu P5170【模板】类欧几里得算法](https://www.luogu.com.cn/problem/P5170)
-   [Luogu P5171 Earthquake](https://www.luogu.com.cn/problem/P5171)
-   [Luogu P5172 \[清华集训 2014\] Sum](https://www.luogu.com.cn/problem/P5172)
-   [Luogu P4132 \[BJOI2012\] 算不出的等式](https://www.luogu.com.cn/problem/P4132)
-   [LOJ 138. 类欧几里得算法](https://loj.ac/p/138)
-   [LOJ 6440. 万能欧几里得](https://loj.ac/p/6440)
-   [Luogu P5179 Fraction](https://www.luogu.com.cn/problem/P5179)
-   [Codeforces 1182 F. Maximum Sine](https://codeforces.com/problemset/problem/1182/F)

Bài ứng dụng:

-   [Luogu P4433 \[COCI 2009/2010 #1\] ALADIN](https://www.luogu.com.cn/problem/P4433)
-   [AtCoder Beginner Contest 372 G - Ax + By < C](https://atcoder.jp/contests/abc372/tasks/abc372_g)
-   [AtCoder Beginner Contest 313 G - Redistribution of Piles](https://atcoder.jp/contests/abc313/tasks/abc313_g)
-   [AtCoder Beginner Contest 283 Ex - Popcount Sum](https://atcoder.jp/contests/abc283/tasks/abc283_h)
-   [Codeforces 1098 E. Fedya the Potter](https://codeforces.com/problemset/problem/1098/E)
-   [Codeforces 868 G. El Toll Caves](https://codeforces.com/problemset/problem/868/G)

## Tài liệu tham khảo và chú thích

[^complexity]: Thường trong các bài toán, $b$ cùng bậc với $a$ nên $O(\log(b/c))$ có thể bỏ qua. Hơn nữa, nếu trước khi gọi Euclid tổng quát, ta thực hiện một vòng Euclid kiểu mới để loại ảnh hưởng của $b$, thì chi phí lũy thừa nhanh có thể tránh được. Vì trong nhiều bài, dạng ban đầu của $U$ đặc biệt, lũy thừa của nó có công thức đơn giản, không cần lũy thừa nhanh. Ví dụ trong bài, $U^{\lfloor b/a\rfloor}$ chỉ cần thay phần tử ngoài đường chéo từ $1$ thành $\lfloor b/a\rfloor$.
