Số Bell $B_n$ được đặt theo tên Eric Temple Bell, là một dãy số nguyên trong tổ hợp, mở đầu như sau ([OEIS A000110](https://oeis.org/A000110)):

$$
B_0 = 1,B_1 = 1,B_2=2,B_3=5,B_4=15,B_5=52,B_6=203,\dots
$$

$B_n$ là số cách phân hoạch một tập có lực lượng $n$ phần tử. Một phân hoạch của tập $S$ được định nghĩa là một họ các tập con không rỗng, đôi một rời nhau, hợp lại bằng $S$. Ví dụ $B_3 = 5$ vì tập gồm 3 phần tử ${a, b, c}$ có 5 cách phân hoạch khác nhau:

$$
\begin{aligned}
&\{ \{a\},\{b\},\{c\}\} \\
&\{ \{a\},\{b,c\}\} \\
&\{ \{b\},\{a,c\}\} \\
&\{ \{c\},\{a,b\}\} \\
&\{ \{a,b,c\}\} \\
\end{aligned}
$$

$B_0$ bằng 1 vì tập rỗng có đúng 1 cách phân hoạch.

## Công thức truy hồi

Số Bell thỏa mãn truy hồi:

$$
B_{n+1}=\sum_{k=0}^n\binom{n}{k}B_{k}
$$

Chứng minh:

$B_{n+1}$ là số phân hoạch của tập có $n+1$ phần tử. Gọi tập của $B_n$ là $\{b_1,b_2,b_3,\dots,b_n\}$, tập của $B_{n+1}$ là $\{b_1,b_2,b_3,\dots,b_n,b_{n+1}\}$. Có thể xem $B_{n+1}$ được tạo ra bằng cách thêm $b_{n+1}$ vào các phân hoạch của $B_n$, xét phần tử $b_{n+1}$:

-   Nếu nó đứng riêng một lớp, còn lại $n$ phần tử, số cách là $\dbinom{n}{n}B_{n}$;

-   Nếu nó cùng 1 phần tử khác vào một lớp, còn lại $n-1$ phần tử, số cách là $\dbinom{n}{n-1}B_{n-1}$;

-   Nếu nó cùng 2 phần tử khác vào một lớp, còn lại $n-2$ phần tử, số cách là $\dbinom{n}{n-2}B_{n-2}$;

-   ……

Suy ra công thức trên.

Mỗi số Bell là tổng của các [Số Stirling loại hai](./stirling.md#第二类斯特林数stirling-number) tương ứng, vì số Stirling loại hai là số cách phân hoạch tập có $n$ phần tử thành đúng $k$ tập con không rỗng.

$$
B_{n} = \sum_{k=0}^n{n\brace k}
$$

## Tam giác Bell

Dựng một tam giác (tương tự tam giác Pascal) như sau:

-   $a_{0,0} = 1$;
-   Với $n \ge 1$, phần tử đầu dòng $n$ bằng phần tử cuối dòng trước: $a_{n,0}=a_{n-1,n-1}$;
-   Với $m,n \ge 1$, phần tử thứ $m$ của dòng $n$ bằng tổng phần tử bên trái và chéo trái trên: $a_{n,m}=a_{n,m-1}+a_{n-1,m-1}$.

Một phần kết quả:

$$
\begin{aligned}
& 1   \\
& 1\quad\qquad 2  \\
& 2\quad\qquad 3\quad\qquad 5  \\
& 5\quad\qquad 7\quad\qquad 10\,\,\,\qquad 15 \\
& 15\,\,\,\qquad 20\,\,\,\qquad  27\,\,\,\qquad 37\,\,\,\qquad 52  \\
& 52\,\,\,\qquad  67\,\,\,\qquad 87\,\,\,\qquad 114\qquad 151\qquad 203\\
& 203\qquad  255\qquad 322\qquad  409\qquad 523\qquad  674\qquad 877 \\  
\end{aligned}
$$

Phần tử đầu mỗi dòng là số Bell. Có thể dùng tam giác này để truy hồi số Bell.

??? note "Tham khảo cài đặt"
    === "C++"
        ```cpp
        constexpr int MAXN = 2000 + 5;
        int bell[MAXN][MAXN];
        
        void f(int n) {
          bell[0][0] = 1;
          for (int i = 1; i <= n; i++) {
            bell[i][0] = bell[i - 1][i - 1];
            for (int j = 1; j <= i; j++)
              bell[i][j] = bell[i - 1][j - 1] + bell[i][j - 1];
          }
        }
        ```
    
    === "Python"
        ```python
        MAXN = 2000 + 5
        bell = [[0 for i in range(MAXN + 1)] for j in range(MAXN + 1)]
        
        
        def f(n):
            bell[0][0] = 1
            for i in range(1, n + 1):
                bell[i][0] = bell[i - 1][i - 1]
                for j in range(1, i + 1):
                    bell[i][j] = bell[i - 1][j - 1] + bell[i][j - 1]
        ```

## Hàm sinh mũ

Xét hàm sinh mũ của số Bell và đạo hàm của nó:

$$
\begin{aligned}
\hat B(x) &= \sum_{n = 0}^{+\infty}\frac{B_n}{n!}x^n \\
&= 1 + \sum_{n = 0}^{+\infty}\frac{B_{n+1}}{(n + 1)!}x^{n + 1} \\
\hat B'(x) &= \sum_{n = 0}^{+\infty}\frac{B_{n+1}}{n!}x^{n}
\end{aligned}
$$

Từ công thức truy hồi của số Bell, ta có:

$$
\frac{B_{n+1}}{n!} = \sum_{k = 0}^{n}\frac{1}{(n-k)!}\frac{B_{k}}{k!}
$$

Đây là một tích chập, nên:

$$
\hat B'(x) = \mathrm{e}^x \hat B(x)
$$

Đây là phương trình vi phân, nghiệm:

$$
\hat B(x) = \exp\left(\mathrm{e}^x + C\right)
$$

Thay $x = 0$ thì $\hat B(x) = 1$, suy ra $C = -1$, được dạng đóng:

$$
\hat B(x) = \exp\left(\mathrm{e}^x - 1\right)
$$

Tiền xử lý $\mathrm{e}^x - 1$ đến $n$ hạng rồi thực hiện [exp đa thức](../poly/elementary-func.md#多项式对数函数--指数函数) sẽ thu được $n$ hạng đầu của số Bell, nút thắt ở exp đa thức, đạt $O(n \log n)$.

## Tài liệu tham khảo

<https://en.wikipedia.org/wiki/Bell_number>
