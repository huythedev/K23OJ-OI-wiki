author: linehk, persdre

Độ phức tạp thời gian và độ phức tạp bộ nhớ là hai tiêu chí quan trọng để đánh giá hiệu quả của một thuật toán.

## Số phép toán cơ bản

Cùng một thuật toán khi chạy trên các máy tính khác nhau có thể có tốc độ khác nhau, và thời gian thực tế cũng khó xác định chính xác về mặt lý thuyết, việc đo đạc thực tế lại khá phiền phức. Vì vậy, thông thường ta không xét thời gian thực tế mà xét số lượng phép toán cơ bản mà thuật toán thực hiện.

Trên máy tính thông thường, các phép cộng, trừ, nhân, chia, truy cập biến (biến kiểu dữ liệu cơ bản), gán giá trị cho biến... đều được coi là phép toán cơ bản.

Việc đếm hoặc ước lượng số phép toán cơ bản có thể dùng làm chỉ số đánh giá thời gian chạy của thuật toán.

## Độ phức tạp thời gian

### Định nghĩa

Để đánh giá tốc độ của một thuật toán, nhất định phải xét đến kích thước dữ liệu. Kích thước dữ liệu thường là số lượng phần tử đầu vào, số đỉnh/số cạnh của đồ thị, v.v. Nói chung, dữ liệu càng lớn thì thời gian chạy càng lâu. Trong các kỳ thi thuật toán, điều quan trọng nhất không phải là thời gian chạy với một kích thước cụ thể, mà là xu hướng tăng thời gian chạy khi kích thước dữ liệu tăng, tức là **độ phức tạp thời gian**.

### Dẫn nhập

Việc xét xu hướng thay đổi thời gian chạy theo kích thước dữ liệu có các lý do sau:

1.  Máy tính hiện đại có thể thực hiện hàng trăm triệu phép toán cơ bản mỗi giây, nên dữ liệu thường rất lớn. Nếu thuật toán A với dữ liệu kích thước $n$ chạy mất $100n$ đơn vị thời gian, còn thuật toán B chạy mất $n^2$, thì với $n<100$ thuật toán B nhanh hơn, nhưng với $n$ lớn, thuật toán A xử lý được hàng triệu dữ liệu mỗi giây, còn B chỉ xử lý được vài chục nghìn. Khi cho phép chạy lâu hơn, ảnh hưởng của độ phức tạp thời gian đến quy mô dữ liệu xử lý được sẽ lớn hơn nhiều so với ảnh hưởng của thời gian chạy ở một kích thước cụ thể.
2.  Ta dùng số phép toán cơ bản để biểu diễn thời gian chạy, nhưng thực tế mỗi phép toán lại có thời gian khác nhau (cộng/trừ nhanh hơn chia). Khi tính độ phức tạp thời gian, ta bỏ qua sự khác biệt này cũng như sự khác biệt giữa 1 phép toán và 10 phép toán, giúp loại bỏ ảnh hưởng của các phép toán khác nhau.

Tất nhiên, thời gian chạy không chỉ phụ thuộc vào kích thước đầu vào mà còn phụ thuộc vào nội dung đầu vào. Vì vậy, độ phức tạp thời gian lại chia thành các loại như:

1.  Độ phức tạp thời gian **tệ nhất**: là độ phức tạp với trường hợp đầu vào khiến thuật toán chạy lâu nhất. Trong các kỳ thi, vì dữ liệu có thể được cho tùy ý trong phạm vi cho phép, nên thường xét trường hợp tệ nhất để đảm bảo thuật toán chạy đúng với mọi dữ liệu.
2.  Độ phức tạp thời gian **trung bình** (kỳ vọng): là độ phức tạp trung bình trên tất cả các đầu vào cùng kích thước (hoặc kỳ vọng với đầu vào ngẫu nhiên).

Khái niệm "xu hướng tăng thời gian chạy theo kích thước dữ liệu" là một khái niệm mơ hồ, cần dùng **ký hiệu tiệm cận** để biểu diễn một cách hình thức.

## Định nghĩa ký hiệu tiệm cận

Ký hiệu tiệm cận là cách mô tả bậc của hàm số một cách chuẩn hóa. Nói đơn giản, ký hiệu tiệm cận bỏ qua các phần tăng chậm và các hệ số (trong phân tích độ phức tạp, hệ số thường gọi là "hằng số"), chỉ giữ lại phần quan trọng thể hiện xu hướng tăng của hàm.

Một mẹo nhớ đơn giản: ký hiệu có dấu bằng (không nghiêm ngặt) dùng chữ hoa, không có dấu bằng (nghiêm ngặt) dùng chữ thường. Bằng nhau là $\Theta$, nhỏ hơn là $O$, lớn hơn là $\Omega$. Chữ $O$ và $o$ vốn là chữ Hy Lạp Omicron, nhưng do giống chữ Latin nên cũng có thể hiểu là chữ O hoa và o thường.

Trong tiếng Anh, tiền tố "-micro-" và "-mega-" thường dùng để chỉ $10^{-6}$ (một phần triệu) và $10^6$ (một triệu), cũng có nghĩa là "nhỏ" và "lớn". Chữ Omicron và Omega trong tiếng Hy Lạp cũng mang nghĩa nhỏ và lớn.

### Ký hiệu $\Theta$ (Theta lớn)

Với hai hàm $f(n)$ và $g(n)$, $f(n)=\Theta(g(n))$ khi và chỉ khi tồn tại $c_1,c_2,n_0>0$ sao cho $\forall n \ge n_0, 0\le c_1\cdot g(n)\le f(n) \le c_2\cdot g(n)$.

Nói cách khác, nếu $f(n)=\Theta(g(n))$, ta có thể tìm được hai số dương $c_1, c_2$ sao cho $f(n)$ bị kẹp giữa $c_1\cdot g(n)$ và $c_2\cdot g(n)$.

Ví dụ, $3n^2+5n-3=\Theta(n^2)$, ở đây $c_1, c_2, n_0$ có thể lần lượt là $2, 4, 100$. $n\sqrt {n} + n{\log^5 n} + m{\log m} +nm=\Theta(n\sqrt {n} + m{\log m} + nm)$, ở đây $c_1, c_2, n_0$ có thể lần lượt là $1, 2, 100$.

### Ký hiệu $O$ (O lớn)

Ký hiệu $\Theta$ cho cả cận trên và dưới, nếu chỉ biết cận trên mà không biết cận dưới, dùng ký hiệu $O$. $f(n)=O(g(n))$ khi và chỉ khi tồn tại $c,n_0$ sao cho $\forall n \ge n_0,0\le f(n)\le c\cdot g(n)$.

Khi phân tích độ phức tạp thời gian, thường dùng ký hiệu $O$ vì ta quan tâm đến cận trên, không quan tâm đến cận dưới.

Lưu ý, "cận trên" và "cận dưới" ở đây là về xu hướng của hàm, không phải về thuật toán. Cận trên thời gian chạy của thuật toán là "độ phức tạp thời gian tệ nhất", không phải ký hiệu $O$. Do đó, có thể dùng ký hiệu $\Theta$ để biểu diễn độ phức tạp thời gian tệ nhất, thậm chí $\Theta$ còn chính xác hơn. Lý do chủ yếu dùng $O$ là đôi khi chỉ chứng minh được cận trên mà không chứng minh được cận dưới (thường gặp ở các thuật toán/phân tích phức tạp), và $O$ dễ gõ hơn.

### Ký hiệu $\Omega$ (Omega lớn)

Tương tự, ký hiệu $\Omega$ dùng để mô tả cận dưới tiệm cận. $f(n)=\Omega(g(n))$ khi và chỉ khi tồn tại $c,n_0$ sao cho $\forall n \ge n_0,0\le c\cdot g(n)\le f(n)$.

### Ký hiệu $o$ (o nhỏ)

Nếu $O$ tương ứng với dấu nhỏ hơn hoặc bằng, thì $o$ tương ứng với dấu nhỏ hơn.

Ký hiệu $o$ được dùng nhiều trong giải tích, ví dụ trong khai triển Taylor có phần dư Peano, dùng $o$ để biểu diễn "nhỏ hơn hẳn", phục vụ cho phân tích tiệm cận.

$f(n)=o(g(n))$ khi và chỉ khi với mọi số dương $c$, tồn tại $n_0$ sao cho $\forall n \ge n_0,0\le f(n)< c\cdot g(n)$.

### Ký hiệu $\omega$ (omega nhỏ)

Nếu $\Omega$ tương ứng với dấu lớn hơn hoặc bằng, thì $\omega$ tương ứng với dấu lớn hơn.

$f(n)=\omega(g(n))$ khi và chỉ khi với mọi số dương $c$, tồn tại $n_0$ sao cho $\forall n \ge n_0,0\le c\cdot g(n)< f(n)$.

![](images/order.png)

### Một số tính chất thường gặp

-   $f(n) = \Theta(g(n))\iff f(n)=O(g(n))\land f(n)=\Omega(g(n))$
-   $f_1(n) + f_2(n) = O(\max(f_1(n), f_2(n)))$
-   $f_1(n) \times f_2(n) = O(f_1(n) \times f_2(n))$
-   $\forall a \neq 1, \log_a{n} = O(\log_2 n)$. Theo công thức đổi cơ số, mọi hàm logarit đều có tốc độ tăng tương tự, nên khi viết độ phức tạp thường bỏ qua cơ số log.

## Ví dụ tính độ phức tạp thời gian đơn giản

### Vòng lặp `for`

=== "C++"
    ```cpp
    int n, m;
    std::cin >> n >> m;
    for (int i = 0; i < n; ++i) {
      for (int j = 0; j < n; ++j) {
        for (int k = 0; k < m; ++k) {
          std::cout << "hello world\n";
        }
      }
    }
    ```

=== "Python"
    ```python
    n = int(input())
    m = int(input())
    for i in range(0, n):
        for j in range(0, n):
            for k in range(0, m):
                print("hello world")
    ```

=== "Java"
    ```java
    int n, m;
    n = input.nextInt();
    m = input.nextInt();
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            for (int k = 0; k < m; ++k) {
                System.out.println("hello world");
            }
        }
    }
    ```

Nếu coi $n$ và $m$ là kích thước dữ liệu, đoạn code trên có độ phức tạp thời gian là $\Theta(n^2m)$.

### DFS

Khi duyệt [DFS](../graph/dfs.md) trên đồ thị $n$ đỉnh $m$ cạnh, mỗi đỉnh và mỗi cạnh chỉ được thăm một số lần hằng số, nên độ phức tạp là $\Theta(n+m)$.

## Những đại lượng nào là hằng số?

Khi thực hiện một số thao tác nhất định, làm sao biết các thao tác đó có ảnh hưởng đến độ phức tạp thời gian không? Ví dụ:

=== "C++"
    ```cpp
    constexpr int N = 100000;
    for (int i = 0; i < N; ++i) {
      std::cout << "hello world\n";
    }
    ```

=== "Python"
    ```python
    N = 100000
    for i in range(0, N):
        print("hello world")
    ```

=== "Java"
    ```java
    final int N = 100000;
    for (int i = 0; i < N; ++i) {
        System.out.println("hello world");
    }
    ```

Nếu $N$ không được coi là kích thước đầu vào, thì đoạn code trên có độ phức tạp $O(1)$.

Khi tính độ phức tạp, việc xác định biến nào là kích thước đầu vào rất quan trọng. Mọi đại lượng không phụ thuộc vào kích thước đầu vào đều coi là hằng số, khi tính độ phức tạp có thể coi là $1$.

Lưu ý, trong các thảo luận lý thuyết về độ phức tạp, giả định "thuật toán giải được mọi kích thước dữ liệu" là cơ bản (dù thực tế bộ nhớ/thời gian có hạn). Do đó, việc tiền xử lý đáp án cho mọi trường hợp nhỏ không làm độ phức tạp thành $O(1)$.

## Định lý chính (Master Theorem)

Có thể dùng Master Theorem để nhanh chóng tính độ phức tạp của các thuật toán đệ quy.

Công thức truy hồi Master Theorem:

$$
T(n) = a T\left(\frac{n}{b}\right)+f(n)\qquad \forall n > b
$$

Khi đó,

$$
T(n) = \begin{cases}\Theta(n^{\log_b a}) & f(n) = O(n^{\log_b (a)-\epsilon}),\epsilon > 0 \\ \Theta(f(n)) & f(n) = \Omega(n^{\log_b (a)+\epsilon}),\epsilon\ge 0\\ \Theta(n^{\log_b a}\log^{k+1} n) & f(n)=\Theta(n^{\log_b a}\log^k n),k\ge 0 \end{cases}
$$

Lưu ý, trường hợp thứ hai cần thỏa mãn điều kiện regularity: $a f(n/b) \leq c f(n)$ với một hằng số $c < 1$ và $n$ đủ lớn.

Ý tưởng chứng minh là chia bài toán kích thước $n$ thành $a$ bài toán kích thước $n/b$, rồi hợp lại, mỗi lần hợp tốn $f(n)$ thời gian.

??? note "Chứng minh"
    Theo ý tưởng trên, chứng minh cụ thể như sau:
    
    Ở tầng $0$ (tầng cao nhất), hợp các bài toán con tốn $f(n)$ thời gian.
    
    Ở tầng $1$ (sau khi chia lần đầu), có $a$ bài toán con, mỗi bài hợp tốn $f\left(\frac{n}{b}\right)$, tổng cộng $a f\left(\frac{n}{b}\right)$.
    
    Lặp lại, ta có cây như sau: ![](./images/master-theorem-proof.svg)
    
    Cây này cao ${\log_b n}$, có $n^{\log_b a}$ lá, nên $T(n) = \Theta(n^{\log_b a}) + g(n)$, với $g(n) = \sum_{j = 0}^{\log_{b}{n - 1}} a^{j} f(n / b^{j})$.
    
    Trường hợp 1: $f(n) = O(n^{\log_b a-\epsilon})$ nên $g(n) = O(n^{\log_b a})$.
    
    Trường hợp 2: $g(n) = \Omega(f(n))$, lại có $a f(\dfrac{n}{b}) \leq c f(n)$ với $c$ đủ nhỏ và $n$ đủ lớn, nên $g(n) = O(f(n))$. Hai phía kẹp lại, $g(n) = \Theta(f(n))$.
    
    Trường hợp 3: $f(n) = \Theta(n^{\log_b a})$ nên $g(n) = O(n^{\log_b a} {\log n})$. Kết quả $T(n)$ hiển nhiên.

Ví dụ sử dụng Master Theorem:

1.  $T(n) = 2T\left(\frac{n}{2}\right) + 1$, $a=2, b=2, {\log_2 2} = 1$, $\epsilon$ lấy $(0, 1]$, thỏa mãn trường hợp 1, $T(n) = \Theta(n)$.

2.  $T(n) = T\left(\frac{n}{2}\right) + n$, $a=1, b=2, {\log_2 1} = 0$, $\epsilon$ lấy $(0, 1]$, thỏa mãn trường hợp 2, $T(n) = \Theta(n)$.

3.  $T(n) = T\left(\frac{n}{2}\right) + {\log n}$, $a=1, b=2, {\log_2 1}=0$, $k=1$, thỏa mãn trường hợp 3, $T(n) = \Theta(\log^2 n)$.

4.  $T(n) = T\left(\frac{n}{2}\right) + 1$, $a=1, b=2, {\log_2 1} = 0$, $k=0$, thỏa mãn trường hợp 3, $T(n) = \Theta(\log n)$.

## Độ phức tạp trung bình (amortized)

Xem chi tiết tại [Độ phức tạp trung bình](./amortized-analysis.md).

## Độ phức tạp bộ nhớ

Tương tự, lượng bộ nhớ mà thuật toán sử dụng khi kích thước đầu vào thay đổi được gọi là **độ phức tạp bộ nhớ**.

## Độ phức tạp tính toán

Bài này chủ yếu giới thiệu độ phức tạp từ góc độ phân tích thuật toán. Nếu muốn tìm hiểu sâu hơn, xem [Độ phức tạp tính toán](../misc/cc-basic.md).
