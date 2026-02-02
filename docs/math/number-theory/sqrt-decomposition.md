Số học phân khối (number theory block) có thể tính nhanh các tổng dạng

$$
\sum_{i=1}^nf(i)g\left(\left\lfloor\dfrac ni\right\rfloor\right)
$$

Nếu có thể tính $\sum_{i=l}^{r}f(i)$ trong $O(1)$ hoặc đã tiền xử lý prefix sum của $f$, thì số học phân khối có thể tính tổng trên trong $O(\sqrt{n})$.

Số học phân khối thường được kết hợp với các kỹ thuật như [phản diễn Möbius](./mobius.md).

## Ý tưởng

Trước hết, bài này dùng một ví dụ đơn giản để minh họa ý tưởng số học phân khối. Giả sử cần đếm số điểm nguyên dưới đường hypebol như hình:

![Điểm nguyên dưới hypebol](./images/sqrt-decomposition.svg)

Điều này tương đương với việc tính tổng:

$$
\sum_{i=1}^{11}\left\lfloor\dfrac{11}{i}\right\rfloor.
$$

Đây là trường hợp đặc biệt của tổng ở trên với $f(k)=1,~g(k)=k$.

Cách đơn giản nhất là tính từng cột rồi cộng, nhưng như vậy phải tính số điểm nguyên ở từng cột $i=1,2,\cdots,11$. Quan sát hình vẽ cho thấy các cột điểm nguyên có thể chia thành $5$ khối, mỗi khối có chiều cao bằng nhau, tạo thành các hình chữ nhật điểm. Vì vậy, chỉ cần biết bề rộng của các khối này là có thể tính nhanh kích thước của từng khối để hoàn thành thống kê.

Đó chính là ý tưởng cơ bản của phân khối theo phép chia.

## Tính chất

Mục này thảo luận các kết luận về phân khối các điểm nguyên dưới hypebol $y = \dfrac{n}{x}$. Cụ thể, cần chia các số nguyên $1\sim n$ thành các khối theo giá trị $\left\lfloor\dfrac{n}{i}\right\rfloor$. Đặt

$$
D(n) = \left\{\left\lfloor\dfrac{n}{i}\right\rfloor : 1 \le i \le n,~i\in\mathbf N_+\right\}.
$$

Đây là tập các giá trị có thể của $\left\lfloor\dfrac{n}{i}\right\rfloor$.

Trước hết, số lượng giá trị khác nhau chỉ có $O(\sqrt{n})$. Vì vậy, số khối khi phân khối cũng chỉ là $O(\sqrt{n})$.

???+ note "Tính chất 1"
    $|D(n)|\le 2\sqrt{n}$.

??? note "Chứng minh"
    Xét hai trường hợp:
    
    -   Khi $i\le\sqrt{n}$, số giá trị của $i$ nhiều nhất là $\sqrt{n}$, nên $\left\lfloor\dfrac{n}{i}\right\rfloor$ cũng có nhiều nhất $\sqrt{n}$ giá trị.
    -   Khi $i>\sqrt{n}$, thì $\left\lfloor\dfrac{n}{i}\right\rfloor \le\dfrac{n}{i} < \sqrt{n}$ nên cũng có nhiều nhất $\sqrt{n}$ giá trị.
    
    Do đó, tổng số giá trị khả dĩ $|D(n)|\le 2\sqrt{n}$.

Phân tích kỹ hơn, có thể mô tả chính xác tập $D(n)$ và kích thước của nó.

???+ note "Tính chất 2"
    Đặt $s = \lfloor\sqrt{n}\rfloor$. Các phần tử của $D(n)$ theo thứ tự tăng là
    
    $$
    1 < 2 < \cdots < s-1 < s \le \left\lfloor\dfrac{n}{s}\right\rfloor < \left\lfloor\dfrac{n}{s-1}\right\rfloor < \cdots < \left\lfloor\dfrac{n}{2}\right\rfloor < \left\lfloor\dfrac{n}{1}\right\rfloor = n.
    $$
    
    Hơn nữa, $|D(n)| = \lfloor \sqrt{4n+1}\rfloor - 1$.

??? note "Chứng minh"
    Trước hết, với $1 \le i \le s$, có thể chứng minh $\left\lfloor\dfrac{n}{\lfloor n/i\rfloor}\right\rfloor = i$. Đặt $d = \left\lfloor\dfrac{n}{i}\right\rfloor$, cần chứng minh $\left\lfloor\dfrac{n}{d}\right\rfloor = i$. Viết dưới dạng bất đẳng thức, điều này tương đương với đã biết $d \le \dfrac{n}{i} < d + 1$ cần chứng minh $i \le \dfrac{n}{d} < i + 1$. Điều kiện đã biết có thể viết là $i\le\dfrac{n}{d} < i + \dfrac{i}{d}$, do đó chỉ cần chứng minh $\dfrac{i}{d} \le 1$, tức là $i \le d = \left\lfloor\dfrac{n}{i}\right\rfloor$. Điều này tương đương với $i \le \dfrac{n}{i}$, đúng với mọi $1 \le i \le s \le \sqrt{n}$.
    
    Kết quả này thực chất cho thấy ánh xạ $i \mapsto \left\lfloor\dfrac{n}{i}\right\rfloor$ tạo thành song ánh giữa tập $\{i:1\le i \le s\}$ và tập $\left\{\left\lfloor\dfrac{n}{i}\right\rfloor: 1\le i\le s\right\}$. Do $i$ khác nhau nên $\left\lfloor\dfrac{n}{i}\right\rfloor$ cũng khác nhau. Hai tập này chỉ có thể trùng nhau ở phần tử $s$ và $\left\lfloor\dfrac{n}{s}\right\rfloor$. Do đó, $|D(n)|=2s - \left[s = \lfloor n/s\rfloor\right]$.
    
    Để thu được biểu thức cuối cùng của $|D(n)|$, cần xét khi nào $s = \left\lfloor\dfrac{n}{s}\right\rfloor$.
    
    -   Khi $s = \left\lfloor\dfrac{n}{s}\right\rfloor$, luôn có $s \le \dfrac{n}{s} < s + 1$, tức là $s^2 \le n < s^2+s$. Lúc này, $4s^2 + 1 \le 4n + 1 < 4s^2+4s+1 = (2s+1)^2$. Vì $4n+1$ luôn lẻ, vế trái có thể nới lỏng tương đương thành $4s^2$, nên điều kiện này tương đương $2s \le \sqrt{4n+1} < 2s+1$, tức $\lfloor \sqrt{4n+1}\rfloor = 2s$.
    -   Khi $s < \left\lfloor\dfrac{n}{s}\right\rfloor$, luôn có $s + 1 \le \dfrac{n}{s}$, tức $s^2 + s\le n$. Đồng thời $n < (s+1)^2$, nên $s^2 + s\le n < (s+1)^2$. Điều này tương đương $(2s+1)^2\le 4n+1 < 4(s+1)^2+1$. Lại dùng $4n+1$ lẻ, vế phải có thể siết tương đương thành $4(s+1)^2$, nên điều kiện này tương đương $2s+1\le\sqrt{4n+1} < 2s+2$, tức $\lfloor \sqrt{4n+1}\rfloor = 2s+1$.
    
    Tổng hợp hai trường hợp, suy ra $|D(n)| = \lfloor \sqrt{4n+1}\rfloor - 1$.

Sau đó, hai đầu mút trái/phải của từng khối đều dễ xác định.

???+ note "Tính chất 3"
    Với $d\in D(n)$, mọi số nguyên $i$ thỏa $\left\lfloor\dfrac{n}{i}\right\rfloor=d$ có miền giá trị
    
    $$
    \left\lfloor\dfrac{n}{d+1}\right\rfloor + 1\le i \le \left\lfloor\dfrac{n}{d}\right\rfloor.
    $$

??? note "Chứng minh"
    Vì $\left\lfloor\dfrac{n}{i}\right\rfloor=d$ tương đương bất đẳng thức
    
    $$
    d\le \dfrac{n}{i} < d+1.
    $$
    
    Tương đương
    
    $$
    \dfrac{n}{d+1} < i\le \dfrac{n}{d}.
    $$
    
    Dùng $i\in\mathbf N_+$, lấy phần nguyên ta được
    
    $$
    \left\lfloor\dfrac{n}{d+1}\right\rfloor + 1 \le i\le \left\lfloor\dfrac{n}{d}\right\rfloor.
    $$

Tính chất này còn phản ánh tính đối xứng của đồ thị: tập các điểm đầu phải của mỗi khối (các điểm xanh trong hình) chính là $D(n)$. Điều này dễ hiểu vì đồ thị đối xứng qua đường $y=x$.

Ngoài ra, tập $D(n)$ còn có tính chất đệ quy tốt:

???+ note "Tính chất 4"
    Với $m\in D(n)$, có $D(m)\subseteq D(n)$.

??? note "Chứng minh"
    Đặt $m = \left\lfloor\dfrac{n}{k}\right\rfloor$. Khi đó, với mọi $i\in\mathbf N_+$,
    
    $$
    \left\lfloor\dfrac{m}{i}\right\rfloor = \left\lfloor\dfrac{\lfloor n/k\rfloor}{i}\right\rfloor = \left\lfloor\dfrac{n}{ki}\right\rfloor \in D(n),
    $$
    
    nên $D(m)\subseteq D(n)$. Ở đây, dấu bằng thứ hai dùng tính chất của [hàm lấy phần nguyên](./basic.md#取整函数) với phân số lồng nhau.

Như đã nói, $D(n)$ vừa là tập giá trị $\left\lfloor\dfrac{n}{i}\right\rfloor$ trong từng khối, vừa là tập các đầu phải của khối. Điều này có nghĩa là nếu muốn áp dụng đệ quy số học phân khối (tức giá trị tại $n$ phụ thuộc vào các giá trị tại $m\in D(n)\setminus\{n\}$), thì trong toàn bộ quá trình tính toán, tập các giá trị và tập các đầu phải đều chính là $D(n)$. Ví dụ điển hình là [Sieve của Du Jiao](./du.md).

## Quy trình

Dựa trên các kết luận ở mục trước, ta có quy trình cụ thể của số học phân khối.

Để tính tổng

$$
\sum_{i=1}^nf(i)g\left(\left\lfloor\dfrac ni\right\rfloor\right)
$$

có thể chia các chỉ số $i=1,2,\cdots,n$ thành các khối theo giá trị $\left\lfloor\dfrac ni\right\rfloor$. Vì các chỉ số có cùng $\left\lfloor\dfrac ni\right\rfloor$ tạo thành một đoạn liên tiếp $[l,r]$, nên giá trị của khối là

$$
\left(\sum_{i=l}^rf(i)\right)\cdot g\left(\left\lfloor\dfrac nl\right\rfloor\right).
$$

Để tính nhanh, thường cần tính nhanh tổng theo $f$ ở vế trái. Có lúc biểu thức tổng đã biết và tính được trong $O(1)$; có lúc có thể tiền xử lý prefix sum, vẫn truy vấn $O(1)$.

Khi duyệt các khối, đầu trái $l$ của khối hiện tại bằng đầu phải khối trước cộng $1$, và đầu phải bằng $\left\lfloor\dfrac n{\lfloor n/l\rfloor}\right\rfloor$. Do đó có mã giả:

$$
\begin{array}{l}
\textbf{Algorithm }\text{Sum}(f,g,n):\\
\textbf{Input. }n,~s(k)=\sum_{i=1}^kf(k),~g(k).\\
\textbf{Output. }S(n) = \sum_{i=1}^nf(i)g(\lfloor n/i\rfloor).\\
\textbf{Method.}\\
\begin{array}{ll}
1 & l \gets 1\\
2 & \textit{result} \gets 0 \\
3 & \textbf{while } l \leq n \textbf{ do}\\
4 & \qquad r \gets \left\lfloor \dfrac{n}{\lfloor n/l \rfloor} \right\rfloor\\
5 & \qquad \textit{result} \gets \textit{result} + (s(r)-s(l-1))\cdot g\left(\left\lfloor \dfrac{n}{l} \right\rfloor\right)\\
6 & \qquad l \gets r+1\\
7 & \textbf{end while}\\
8 & \textbf{return }\textit{result}
\end{array}
\end{array}
$$

Giả sử tính $s(\cdot)$ mỗi lần trong $O(1)$, thì tổng độ phức tạp là $O(\sqrt{n})$.

## Mở rộng

Phần trước bàn về dạng phổ biến và cơ bản nhất. Mục này thảo luận các dạng mở rộng.

### Phân khối với làm tròn lên

Số học phân khối cũng áp dụng cho tổng có làm tròn lên:

$$
\sum_{i=1}^nf(i)g\left(\left\lceil\dfrac ni\right\rceil\right).
$$

Vì $\left\lceil\dfrac ni\right\rceil = \left\lfloor\dfrac {n-1}i\right\rfloor + 1$, tổng này có thể đổi về dạng làm tròn xuống:

$$
f(n)g(1) + \sum_{i=1}^{n-1}f(i)g\left(\left\lfloor\dfrac {n-1}i\right\rfloor + 1\right).
$$

Lưu ý cận trên của tổng thay đổi, và có thêm hạng riêng khi $i=n$.

### Phân khối nhiều chiều

Số học phân khối còn dùng cho tổng có nhiều biểu thức lấy phần nguyên:

$$
\sum_{i=1}^{n}f(i)g\left(\left\lfloor\dfrac {n_1}i\right\rfloor,\left\lfloor\dfrac {n_2}i\right\rfloor,\cdots,\left\lfloor\dfrac {n_m}i\right\rfloor\right).
$$

Để áp dụng ý tưởng phân khối, phải đảm bảo trong mỗi khối, tất cả các biểu thức $\left\lfloor\dfrac {n_1}i\right\rfloor,\left\lfloor\dfrac {n_2}i\right\rfloor,\cdots,\left\lfloor\dfrac {n_m}i\right\rfloor$ đều không đổi, tức khối nhiều chiều là giao của các khối một chiều. Với đầu trái $l$ đã biết, đầu phải là

$$
\min\left\{\left\lfloor\dfrac {n_1}{\lfloor n_1/l\rfloor}\right\rfloor,\left\lfloor\dfrac {n_2}{\lfloor n_2/l\rfloor}\right\rfloor,\cdots,\left\lfloor\dfrac {n_m}{\lfloor n_m/l\rfloor}\right\rfloor\right\}.
$$

Tức lấy min của các đầu phải một chiều. Hình minh họa:

![Minh họa phân khối nhiều chiều](./images/n-dimension-sqrt-decomposition.svg)

Trường hợp phổ biến là hai chiều, khi đó thay dòng $r \gets \left\lfloor \dfrac{n}{\lfloor n/l \rfloor}\right\rfloor$ bằng

$$
r \gets \min\left\{\left\lfloor \dfrac{n_1}{\lfloor n_1/l \rfloor}\right\rfloor,\left\lfloor \dfrac{n_2}{\lfloor n_2/l \rfloor}\right\rfloor\right\}.
$$

### Phân khối với số mũ bất kỳ

Số học phân khối còn dùng cho tổng có số mũ bất kỳ:

$$
\sum_{i=1}^{\lfloor n^{\alpha/\beta}\rfloor}f(i)g\left(\left\lfloor\dfrac {n^\alpha}{i^\beta}\right\rfloor\right).
$$

Trong đó $\alpha,\beta$ là số thực dương. Dạng cơ bản ở trên có $\alpha=\beta=1$.

???+ note "Tính chất"
    Với số nguyên dương $n$ và số thực dương $\alpha, \beta$, đặt
    
    $$
    D(n,\alpha,\beta) = \left\{\left\lfloor\dfrac{n^\alpha}{i^\beta}\right\rfloor: i=1,2,\cdots,\lfloor n^{\alpha/\beta}\rfloor\right\}.
    $$
    
    Khi đó:
    
    1.  $|D(n,\alpha,\beta)|\le 2n^{\alpha/(1+\beta)}$.
    2.  Với $d\in D(n,\alpha,\beta)$, các $i$ thỏa $\left\lfloor\dfrac{n^\alpha}{i^\beta}\right\rfloor=d$ có miền
    
        $$
        \left\lfloor\dfrac{n^{\alpha/\beta}}{(d+1)^{1/\beta}}\right\rfloor + 1\le i \le \left\lfloor\dfrac{n^{\alpha/\beta}}{d^{1/\beta}}\right\rfloor.
        $$

??? note "Chứng minh"
    Với ý 1, xét hai trường hợp:
    
    -   Khi $i\le \dfrac{n^\alpha}{i^\beta}$, ta có $i\le n^{\alpha/(1+\beta)}$, nên $\left\lfloor\dfrac{n^\alpha}{i^\beta}\right\rfloor$ có nhiều nhất $n^{\alpha/(1+\beta)}$ giá trị.
    -   Khi $i > \dfrac{n^\alpha}{i^\beta}$, ta có $i> n^{\alpha/(1+\beta)}$, suy ra $\dfrac{n^\alpha}{i^\beta} < n^{\alpha/(1+\beta)}$, nên $\left\lfloor\dfrac{n^\alpha}{i^\beta}\right\rfloor$ cũng có nhiều nhất $n^{\alpha/(1+\beta)}$ giá trị.
    
    Gộp hai trường hợp, $|D(n,\alpha,\beta)|\le 2n^{\alpha/(1+\beta)}$.
    
    Với ý 2, $\left\lfloor\dfrac{n^\alpha}{i^\beta}\right\rfloor=d$ tương đương
    
    $$
    d \le \dfrac{n^\alpha}{i^\beta} < d+1 \iff \dfrac{n^{\alpha/\beta}}{(d+1)^{1/\beta}} < i \le \dfrac{n^{\alpha/\beta}}{d^{1/\beta}}.
    $$
    
    Lấy phần nguyên, ta được mệnh đề 2.

Dùng các tính chất này có thể tính phân khối với độ phức tạp $O(n^{\alpha/(1+\beta)})$.

???+ example "Ví dụ"
    Với $\alpha=\beta=1/2$, xét tổng
    
    $$
    \sum_{i=1}^nf(i)g\left(\left\lfloor\sqrt{\dfrac {n}{i}}\right\rfloor\right),
    $$
    
    có thể giải bằng phân khối trong $O(n^{1/3})$. Với đầu trái $l$ đã biết, đầu phải là $r=\left\lfloor\dfrac{n}{\lfloor\sqrt{n/l}\rfloor^2}\right\rfloor$.

## Bài tập mẫu

???+ example "[UVa11526 H(n)](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=27&page=show_problem&problem=2521)"
    $T$ bộ dữ liệu, mỗi bộ là một số nguyên $n$. Với mỗi bộ, in $\sum_{i=1}^n\left\lfloor\dfrac ni\right\rfloor$.

??? note "Lời giải"
    Theo phân tích ở trên, có thể gộp các chỉ số có cùng $\left\lfloor\dfrac ni\right\rfloor$ để tính. Độ phức tạp $O(T\sqrt n)$.

??? note "Cài đặt"
    ```cpp
    --8<-- "docs/math/code/sqrt-decomposition/sqrt-decomposition_1.cpp"
    ```

???+ example "[Codeforces 1954E Chain Reaction](https://codeforces.com/contest/1954/problem/E)"
    Có $n$ quái, quái thứ $i$ có máu $a_i$. Mỗi lần tấn công làm giảm $k$ máu của một đoạn liên tiếp các quái còn sống; máu $\le 0$ thì chết. Với mọi $k$, hãy tính số lần tấn công cần để giết hết. $n,a_i\leq 10^5$.

??? note "Lời giải"
    Đặt $a_0=0$. Giả sử giết hết $(i-1)$ con đầu cần $T(k,i-1)$ lần. Quái $i$ có máu $a_i$. Khi giết quái $(i-1)$, cần tấn công $\lceil a_{i-1}/k\rceil$ lần; các đòn này có thể kéo dài sang quái $i$. Do đó, để giết quái $i$, chỉ cần thêm $\max\{0,\lceil a_i/k\rceil-\lceil a_{i-1}/k\rceil\}$ lần. Tổng số lần tấn công là
    
    $$
    T(k,n)=\sum_{i=1}^n\max\left(0,\left\lceil\dfrac{a_i}{k}\right\rceil-\left\lceil\dfrac{a_{i-1}}{k}\right\rceil\right).
    $$
    
    Do $n,k$ lớn, không thể tính riêng cho từng $k$. Có thể với mỗi $i=1,2,\cdots,n$ duy trì dãy $\{T(k,i)\}_k$. Ban đầu $T(k,0)\equiv 0$. Giả sử $\{T(k,i-1)\}_k$ đã biết, cần cập nhật để có $\{T(k,i)\}_k$. Theo phân tích, chỉ cần tăng hạng $k$ thêm $\max\left(0,\left\lceil\dfrac{a_i}{k}\right\rceil-\left\lceil\dfrac{a_{i-1}}{k}\right\rceil\right)$. Dùng phân khối 2 chiều, thao tác này tách thành $O(\sqrt{a_{i-1}}+\sqrt{a_i})$ đoạn cập nhật, mỗi đoạn cộng một giá trị cố định. Dãy $\{T(k,n)\}_k$ cuối cùng là đáp án.
    
    Vì chỉ có cộng đoạn và truy vấn sau cùng, có thể dùng mảng hiệu để cộng đoạn, rồi lấy prefix sum để ra kết quả. Độ phức tạp $O(\sum\sqrt{a_i})$. Bài này còn có cách khác.

??? note "Cài đặt"
    ```cpp
    --8<-- "docs/math/code/sqrt-decomposition/sqrt-decomposition_2.cpp"
    ```

## Bài tập

-   [UVa11526 H(n)](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=27&page=show_problem&problem=2521)
-   [Luogu P2261 CQOI2007 余数求和](https://www.luogu.com.cn/problem/P2261)
-   [Luogu P3455 POI2007 ZAP-Queries](https://www.luogu.com.cn/problem/P3455)

## Tài liệu tham khảo và chú thích

-   [Phân tích độ phức tạp thời gian/bộ nhớ của Du Jiao Sieve by riteme](https://riteme.site/blog/2018-9-11/time-space-complexity-dyh-algo.html)
