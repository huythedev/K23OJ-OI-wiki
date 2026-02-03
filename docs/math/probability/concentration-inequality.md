Trong lập trình thi đấu đôi khi dùng [thuật toán ngẫu nhiên](../../misc/rand-technique.md), tính đúng đắn và độ phức tạp thường dựa trên giả định “một số biến cố có xác suất rất nhỏ”. Ví dụ độ phức tạp của quicksort phụ thuộc vào việc “pivot gần nhỏ nhất hoặc lớn nhất” hiếm khi xảy ra.

Bài viết này giới thiệu một số công cụ phân tích thuật toán ngẫu nhiên và vài ví dụ đơn giản.

## Union Bound

Với các biến cố $A_1, \cdots, A_m$,

$$
P\left\{ \bigcup_{i=1}^m A_i \right\} \leq \sum_{i=1}^m P\{A_i\}
$$

Tức xác suất ít nhất một biến cố xảy ra không vượt quá tổng xác suất từng biến cố.

Thực ra có thể tăng cường:

-   Xác suất ít nhất một biến cố xảy ra **không nhỏ hơn** tổng xác suất từng biến cố trừ đi tổng xác suất của từng cặp cùng xảy ra.
-   Xác suất ít nhất một biến cố xảy ra **không vượt quá** tổng xác suất từng biến cố trừ tổng xác suất từng cặp cùng xảy ra cộng tổng xác suất từng bộ ba cùng xảy ra.
-   ……

Khi số bậc tăng, các cận trên/dưới xen kẽ ngày càng chặt, dạng thức giống nguyên lý bao hàm–loại trừ; chứng minh tương tự nên lược bỏ.

## Bất đẳng thức Markov

Nếu $X$ là biến ngẫu nhiên không âm thì với mọi $a>0$:

$$
P\{ X \geq a \} \leq \frac{EX}{a}
$$

Do Markov không dùng thông tin phân phối ngoài kỳ vọng nên cận thường khá lỏng.

### Chứng minh

Gọi $I$ là hàm chỉ thị của biến cố $X \geq a$ thì

$$
I \leq \frac{X}{a}
$$

Suy ra

$$
P\{ X \geq a \} = EI \leq E \left[ \frac{X}{a} \right] = \frac{EX}{a}
$$

## Bất đẳng thức Chebyshev

Với biến ngẫu nhiên $X$, mọi $a>0$:

$$
P \{ |X - EX| \geq a \} \leq \frac{DX}{a^2}
$$

Đặc biệt, nếu $a=k\sigma$ thì

$$
P \{ |X - EX| \geq k\sigma \} \leq \frac{1}{k^2}
$$

với $\sigma$ là độ lệch chuẩn.

### Chứng minh

Ta có

$$
P \{ |X - EX| \geq a \} = P \{ (X - EX)^2 \geq a^2 \}
$$

Vì $(X - EX)^2$ không âm, theo Markov:

$$
P \{ (X - EX)^2 \geq a^2 \} \leq \frac{E(X - EX)^2}{a^2} = \frac{DX}{a^2}
$$

## Bất đẳng thức Chernoff

Chernoff tổng quát có thể suy từ Markov áp dụng cho $\mathrm{e}^{tX}$:

Với $X$, mọi $t>0$:

$$
P\{ X \geq a \} = P\{ \mathrm{e}^{tX} > \mathrm{e}^{ta} \} \leq \frac{E \mathrm{e}^{tX}}{\mathrm{e}^{ta}}
$$

Tương tự, khi $t<0$:

$$
P\{ X \leq a \} = P\{ \mathrm{e}^{tX} > \mathrm{e}^{ta} \} \leq \frac{E \mathrm{e}^{tX}}{\mathrm{e}^{ta}}
$$

### Chernoff cho tổng các thử nghiệm Poisson

Trong lập trình thi đấu, biến ngẫu nhiên thường là tổng các thử nghiệm Poisson.

Thử nghiệm Poisson là thử nghiệm có hai kết quả.

Một lần thử có thể mô tả bởi biến $X \in \{0,1\}$ với phân phối

$$
P\{ X = i \} = \begin{cases}
    p_i, & i = 1 \\
    1 - p_1, & i = 0
\end{cases}
$$

Với $n$ thử nghiệm độc lập $X_1, \cdots, X_n$, đặt $X = \sum_{i=1}^{n} X_i$ và $\mu = EX$, thì với mọi $0 < \epsilon < 1$:

$$
P\left\{ |X - \mu| \geq \epsilon \mu \right\} \leq 2 \exp\left( - \frac{1}{3} \mu \epsilon^2 \right)
$$

## Bất đẳng thức Hoeffding

Nếu $X_1, \cdots, X_n$ độc lập và $X_i\in [a_i,b_i]$, đặt $X=\sum\limits_{i=1}^n X_i$, thì

$$
P\{ |X - EX| \geq \epsilon \} \leq 2\exp \left( \frac {-2\epsilon^2}{\sum\limits_{i=1}^n (b_i-a_i)^2} \right)
$$

Chernoff và Hoeffding đều giới hạn mức lệch khỏi kỳ vọng. Chứng minh khá dài, có thể xem Probability and Computing.

Kinh nghiệm: nếu $EX$ không quá gần $a_1+\cdots+a_n$ thì cận khá chặt; nếu quá gần (như [UOJ #72 全新做法](https://matthew99.blog.uoj.ac/blog/5511)) thì cận lỏng, nên dùng Chernoff.

## Ví dụ ứng dụng

### Ví dụ: rải điểm ngẫu nhiên ước lượng $\pi$

Xét thuật toán ước lượng $\pi$:

Trong hình vuông $[-1, 1]^2$, tạo ngẫu nhiên $n$ điểm. Gọi $m$ là số điểm rơi vào đĩa đơn vị $x^2 + y^2 \leq 1$, thì lấy $\dfrac{4m}{n}$ làm xấp xỉ $\pi$.

Câu hỏi: để đảm bảo thuật toán trả kết quả sai số tương đối không quá $\epsilon$ với xác suất ít nhất $(1 - \delta)$, cần chọn $n$ thế nào?

??? note "Giải đáp"
    Gọi $X_i$ là biến cố “điểm thứ $i$ nằm trong đĩa”, khi đó $X = \sum_{i=1}^{n} X_i$. Cần tìm $n$ sao cho
    
    $$
    P\left\{ \left| \frac{4X}{n} - \pi \right| \geq \epsilon \pi \right\} \leq \delta
    $$
    
    Tương đương
    
    $$
    P\left\{ \left| X - \frac{\pi}{4}n \right| \geq \epsilon \cdot \frac{\pi}{4}n  \right\} \leq \delta
    $$
    
    Theo Chernoff, chỉ cần
    
    $$
    2 \exp\left( - \frac{1}{3} \epsilon^2 \cdot \frac{\pi}{4}n \right) \leq \delta
    $$
    
    Suy ra
    
    $$
    n \geq \frac{12}{\pi} \epsilon^{-2} \ln \frac{2}{\delta}
    $$
    
    Tức khi $n = \Omega(\epsilon^{-2} \ln \frac{1}{\delta})$ thì đạt độ chính xác yêu cầu.

### Ví dụ: bài toán trúng thưởng

Một hộp có $n$ bóng, đúng $k$ bóng là giải thưởng. Mỗi lần bốc ngẫu nhiên đều, bốc xong bỏ lại. Hỏi cần bốc bao nhiêu lần để đảm bảo với xác suất ít nhất $(1 - \epsilon)$ rằng **mỗi** bóng thưởng đã được bốc ít nhất một lần?

??? note "Giải đáp"
    Nếu chỉ có một bóng thưởng, bốc $M=n\log\epsilon^{-1}$ lần là đủ vì xác suất trượt hết:
    
    $$
    \Big(1-\dfrac 1n\Big)^{n\log\epsilon^{-1}}\leq e^{\log\epsilon}=\epsilon
    $$
    
    Với $k>1$, theo Union Bound chỉ cần mỗi bóng bị bỏ sót với xác suất không quá $\dfrac \epsilon k$. Do đó đáp án là $n \log \dfrac{k}{\epsilon}$.

### Ví dụ: chọn ngẫu nhiên một nửa phần tử

Cho thuật toán chọn ngẫu nhiên đều một tập con có kích thước $\dfrac{n}{2}$ từ $n$ phần tử (giả sử $n$ chẵn). Nguồn ngẫu nhiên duy nhất là một đồng xu công bằng; hãy giảm số lần tung xu (không cần tối ưu tuyệt đối).

??? note "Giải pháp"
    Có thể nghĩ tới:
    
    -   Tung $n$ lần xu, ta chọn đều một tập con bất kỳ.
    -   Lặp lại đến khi kích thước đúng $\dfrac n2$.
        -   Tập con kích thước $\dfrac n2$ chiếm ít nhất $\dfrac 1n$ trong tất cả tập con, nên số lần lặp kỳ vọng $\leq n$.
    
    Thuật toán này kỳ vọng cần $n^2$ lần tung.
    
    Thuật toán khác:
    
    -   Có thể tung kỳ vọng $2\lceil\log_2 n\rceil$ lần để chọn ngẫu nhiên một phần tử trong $n$.
        -   Cách làm: sinh ngẫu nhiên $\lceil\log_2 n\rceil$ bit; nếu số tạo ra $\ge n$ thì làm lại, nếu không chọn phần tử tương ứng (đánh số từ 0) và kết thúc.
    -   Lặp: chọn một phần tử từ các phần tử còn lại, cho đến khi đủ $\dfrac n2$ phần tử.
    
    Thuật toán này kỳ vọng cần $n\lceil\log_2 n\rceil$ lần tung.
    
    Kết hợp hai thuật toán:
    
    -   Dùng thuật toán đầu chọn ngẫu nhiên một tập con.
    -   Nếu kích thước nhỏ hơn $\dfrac n2$, dùng thuật toán thứ hai để thêm phần tử đến đủ.
    -   Nếu kích thước lớn hơn $\dfrac n2$, dùng thuật toán thứ hai để bỏ phần tử đến còn $\dfrac n2$.
    
    Phân tích số thao tác ở bước 2,3 (số lần thêm/bớt):
    
    -   Gọi biến 01 $X_i$ là việc $i$ được chọn trong tập con ban đầu, đặt $X:=X_1+\cdots+X_n$ là kích thước tập. Khi đó số thao tác bằng $\big|X-\mathrm{E}[X]\big|$. Áp dụng Hoeffding với $t=c\cdot\sqrt n$ (c là hằng), ta có $\mathrm{Pr}\Big[\big|X-\mathrm{E}[X]\big|\geq t\Big]\leq 2\mathrm{e}^{-c^2}$. Nghĩa là cho phép lệch $\Theta(\sqrt n)$ sẽ có xác suất thất bại hằng nhỏ.
    
    Do đó, số lần tung xu được đảm bảo (xác suất cao) trong $n+\Theta(\sqrt n\log n)$.
    
    -   $n$ đến từ việc chọn tập ban đầu; $\Theta(\sqrt n\log n)$ là chi phí cho $\Theta(\sqrt n)$ lần thêm/bớt.
    
    ??? note "Tính kỳ vọng độ phức tạp"
        Phân tích kỳ vọng số lần tung:
        
        Dùng Hoeffding để chặn kỳ vọng số thao tác ở bước 2,3:
        
        $$
        E|X - EX| = \int_0^\infty P\{ |X - E[X]| \geq t \} \mathrm{d}t \leq 
        2 \int_0^\infty \exp \left(-\frac {t^2}{n}\right) \mathrm{d}t=\sqrt{\pi n}
        $$
        
        Vậy kỳ vọng số lần tung ở bước 2,3 là $\sqrt{\pi n}\cdot2\lceil\log_2 n\rceil$.
        
        Kết luận: kỳ vọng cần $n+2\sqrt{\pi n}\lceil\log_2 n\rceil$ lần tung.

### Bài tập: Balls and Bins

$n$ bóng được thả ngẫu nhiên độc lập vào $n$ hộp. Chứng minh rằng: số bóng trong hộp nhiều nhất với xác suất $1 - \dfrac{1}{n}$ không nhỏ hơn $\Omega \left( \dfrac{\log n}{\log \log n} \right)$.
