Phân hoạch: viết số tự nhiên $n$ thành tổng các số nguyên dương giảm dần.

$$
n=r_1+r_2+\ldots+r_k \quad r_1 \ge r_2 \ge \ldots \ge r_k \ge 1
$$

Mỗi số trong tổng gọi là một phần.

Số phân hoạch: $p_n$. Số cách phân hoạch của $n$.

Số phân hoạch từ $0$:

| n     | 0 | 1 | 2 | 3 | 4 | 5 | 6  | 7  | 8  |
| ----- | - | - | - | - | - | - | -- | -- | -- |
| $p_n$ | 1 | 1 | 2 | 3 | 5 | 7 | 11 | 15 | 22 |

## Số phân hoạch k phần

Phân $n$ thành đúng $k$ phần gọi là số phân hoạch $k$ phần, ký hiệu $p(n,k)$.

Rõ ràng, $p(n,k)$ cũng là số nghiệm của:

$$
n-k=y_1+y_2+\ldots+y_k\quad y_1\ge y_2\ge\ldots\ge y_k\ge 0
$$

Nếu phương trình có đúng $j$ phần khác $0$ thì có $p(n-k,j)$ nghiệm. Do đó:

$$
p(n,k)=\sum_{j=0}^k p(n-k,j)
$$

Lấy hiệu hai tổng liên tiếp:

$$
p(n,k)=p(n-1,k-1)+p(n-k,k)
$$

Nếu lập bảng, mỗi ô bằng tổng ô trên trái và ô phía trên cùng cột (cách đó đúng số cột).

| k        | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| -------- | - | - | - | - | - | - | - | - | - |
| $p(0,k)$ | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| $p(1,k)$ | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| $p(2,k)$ | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| $p(3,k)$ | 0 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| $p(4,k)$ | 0 | 1 | 2 | 1 | 1 | 0 | 0 | 0 | 0 |
| $p(5,k)$ | 0 | 1 | 2 | 2 | 1 | 1 | 0 | 0 | 0 |
| $p(6,k)$ | 0 | 1 | 3 | 3 | 2 | 1 | 1 | 0 | 0 |
| $p(7,k)$ | 0 | 1 | 3 | 4 | 3 | 2 | 1 | 1 | 0 |
| $p(8,k)$ | 0 | 1 | 4 | 5 | 5 | 3 | 2 | 1 | 1 |

### Ví dụ

???+ note "Tính số phân hoạch k phần"
    Tính $p(n,k)$, nhiều test, $n\le 10000$, $k\le 1000$, modulo $1000007$.
    
    Quan sát bảng và truy hồi, cập nhật theo cột tiết kiệm bộ nhớ. Ta có:
    
    ```cpp
    #include <cstdio>
    #include <cstring>
    
    int p[10005][1005]; /*Số cách phân hoạch n thành k phần*/
    
    int main() {
      int n, k;
      while (~scanf("%d%d", &n, &k)) {
        memset(p, 0, sizeof(p));
        p[0][0] = 1;
        int i;
        for (i = 1; i <= n; ++i) {
          int j;
          for (j = 1; j <= k; ++j) {
            if (i - j >= 0) /*p[i-j][j] mọi phần > 1*/
            {
              p[i][j] = (p[i - j][j] + p[i - 1][j - 1]) %
                        1000007; /*p[i-1][j-1] có ít nhất một phần =1.*/
            }
          }
        }
        printf("%d\n", p[n][k]);
      }
    }
    ```

### Hàm sinh

Theo cấp số nhân:

$$
\frac{1}{1-x^k}=1+x^k+x^{2k}+x^{3k}+\ldots
$$

$$
1+p_1 x+p_2 x^2+p_3 x^3+\ldots=\frac{1}{1-x}  \frac{1}{1-x^2}  \frac{1}{1-x^3}\ldots
$$

Với phân hoạch k phần, hàm sinh:

$$
\sum_{n,k=0}^\infty {p(n,k) x^n y^k }=\frac{1}{1-xy}  \frac{1}{1-x^2 y}  \frac{1}{1-x^3 y}\ldots
$$

### Hình Ferrers

Hình Ferrers: biểu diễn mỗi phần của phân hoạch bằng một hàng điểm, số điểm trong hàng bằng kích thước phần.

Theo định nghĩa, các hàng sắp giảm dần, hàng dài nhất ở trên.

Ví dụ: phân hoạch $12=5+4+2+1$.

![](./images/ferrers.jpg)

Lật hình Ferrers theo đường chéo được hình liên hợp, phân hoạch mới là liên hợp của phân hoạch cũ.

Ví dụ: liên hợp của $12=5+4+2+1$ là $12=4+3+2+2+1$.

Số phân hoạch có phần lớn nhất bằng $k$.

Theo định nghĩa liên hợp:

Số phân hoạch có phần lớn nhất $k$ bằng số phân hoạch có đúng $k$ phần, đều là $p(n,k)$.

## Phân hoạch phân biệt

Phân hoạch phân biệt: $pd_n$. Các phần của phân hoạch đôi một khác nhau. (Different)

| n      | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| ------ | - | - | - | - | - | - | - | - | - |
| $pd_n$ | 1 | 1 | 1 | 2 | 2 | 3 | 4 | 5 | 6 |

Định nghĩa $pd(n,k)$ là số phân hoạch phân biệt có đúng $k$ phần, nghiệm của:

$$
n=r_1+r_2+\ldots+r_k\quad r_1>r_2>\ldots>r_k\ge 1
$$

Tương tự, nghiệm của:

$$
n-k=y_1+y_2+\ldots+y_k\quad y_1>y_2>\ldots>y_k\ge 0
$$

Khác với trên, do phân biệt nên tối đa một phần bằng 0. Kết luận: đúng $j$ phần khác 0 thì có $pd(n-k,j)$ nghiệm, $j$ chỉ là $k$ hoặc $k-1$. Do đó:

$$
pd(n,k)=pd(n-k,k-1)+pd(n-k,k)
$$

Lập bảng tương tự. Mỗi ô bằng tổng hai cột trước tương ứng như mô tả.

| k         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| --------- | - | - | - | - | - | - | - | - | - |
| $pd(0,k)$ | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| $pd(1,k)$ | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| $pd(2,k)$ | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| $pd(3,k)$ | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| $pd(4,k)$ | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| $pd(5,k)$ | 0 | 1 | 2 | 0 | 0 | 0 | 0 | 0 | 0 |
| $pd(6,k)$ | 0 | 1 | 2 | 1 | 0 | 0 | 0 | 0 | 0 |
| $pd(7,k)$ | 0 | 1 | 3 | 1 | 0 | 0 | 0 | 0 | 0 |
| $pd(8,k)$ | 0 | 1 | 3 | 2 | 0 | 0 | 0 | 0 | 0 |

### Ví dụ

???+ note "Tính số phân hoạch phân biệt"
    Tính $pd_n$ với nhiều test, $n\le 50000$, modulo $1000007$.
    
    Quan sát bảng và truy hồi, cập nhật theo cột tiết kiệm bộ nhớ. Code giữ hai cột.
    
    ```cpp
    #include <cstdio>
    #include <cstring>
    
    int pd[50005][2]; /*Số cách phân hoạch n thành k phần phân biệt*/
    
    int main() {
      int n;
      while (~scanf("%d", &n)) {
        memset(pd, 0, sizeof(pd));
        pd[0][0] = 1;
        int ans = 0;
        int j;
        for (j = 1; j < 350; ++j) {
          int i;
          for (i = 0; i < 350; ++i) {
            pd[i][j & 1] = 0; /*pd[i][j] chỉ phụ thuộc pd[][j] và pd[][j-1]*/
          }
          for (i = 0; i <= n; ++i) {
            if (i - j >= 0) /*pd[i-j][j] mọi phần > 1*/
            {
              pd[i][j & 1] = (pd[i - j][j & 1] + pd[i - j][(j - 1) & 1]) %
                             1000007; /*pd[i-j][j-1] có ít nhất một phần =1.*/
            }
          }
          ans = (ans + pd[n][j & 1]) % 1000007;
        }
        printf("%d\n", ans);
      }
    }
    ```

### Phân hoạch lẻ

Phân hoạch lẻ: $po_n$. Các phần đều là số lẻ. (Odd)

Có đẳng thức:

$$
\prod_{i=1}^\infty (1+x^i ) =\frac{\prod_{i=1}^\infty (1-x^{2i} ) }{\prod_{i=1}^\infty (1-x^i ) }=\prod_{i=1}^\infty \frac{1}{1-x^{2i-1} }
$$

Vế trái là hàm sinh phân hoạch phân biệt, vế phải là hàm sinh phân hoạch lẻ. Hệ số tương ứng bằng nhau, nên:

$$
po_n=pd_n
$$

Nhưng $k$ phần lẻ và $k$ phần phân biệt không phải một khái niệm, không liệt kê.

Thêm hai khái niệm:

Phân hoạch phân biệt có số phần chẵn: $pde_n$. (Even)  
Phân hoạch phân biệt có số phần lẻ: $pdo_n$. (Odd)

Do đó:

$$
pd_n=pde_n+pdo_n
$$

Có định nghĩa $k$ phần tương ứng, nhưng quá phức tạp nên bỏ.

## Định lý số ngũ giác

Xét mẫu của hàm sinh phân hoạch:

$$
\prod_{i=1}^\infty (1-x^i ) 
$$

Khai triển, liên hệ với phân hoạch phân biệt theo chẵn lẻ số phần.

Cụ thể, phân hoạch phân biệt có số phần chẵn được đếm dương, số phần lẻ được đếm âm. Do đó:

$$
\sum_{i=0}^\infty ({pde}_n-{pdo}_n ) x^n =\prod_{i=1}^\infty (1-x^i ) 
$$

Tiếp theo, đa số hệ số bằng $0$, chỉ vài vị trí có $\pm1$.

Dùng phép dựng. Vẽ Ferrers của phân hoạch phân biệt. Gọi hàng cuối là đáy, số điểm là $b$ (Bottom); đường chéo $45^\circ$ dài nhất từ điểm cuối hàng trên cùng đến một điểm của hình gọi là sườn, số điểm là $s$ (Slide).

![](./images/bottom_slide.jpg)

Để tạo song ánh giữa phân hoạch phân biệt chẵn/lẻ, định nghĩa biến đổi làm đổi số hàng $\pm1$:

Biến đổi A: nếu $b\le s$, dịch đáy sang phải thành một sườn mới.

Biến đổi B: nếu $b>s$, dịch sườn xuống thành đáy mới.

Với đa số $n$, mỗi phân hoạch chỉ có một biến đổi hợp lệ, tạo song ánh giữa chẵn và lẻ, nên hệ số bằng 0.

Nhưng với vài $n$, có đúng một phân hoạch không thể biến đổi:

-   Trường hợp 1: $b=s$ và đáy với sườn chung một điểm, A không thực hiện được. Khi đó

$$
n=s+(s+1)+\ldots+(s+s-1)=\frac{s(3s-1)}{2}
$$

Hệ số là $(-1)^s x^n$.

-   Trường hợp 2: $b=s+1$ và đáy với sườn chung một điểm, B không thực hiện được. Khi đó

$$
n=(s+1)+(s+2)+\ldots+(s+s)=\frac{s(3s+1)}{2}
$$

Hệ số là $(-1)^s x^n$.

Thay $s$ bằng $-s$, được $n=\frac{s(3s-1)}{2}$ với $s$ âm, hệ số vẫn $(-1)^s x^n$.

Hai trường hợp không trùng, nên điều kiện là

$$
\exists k\in\mathbb{Z},n=\frac{k(3k-1)}{2}
$$

Do đó:

$$
(1-x)(1-x^2 )(1-x^3 )\ldots=\sum_{k=-\infty}^{+\infty} (-1)^k x^{\frac{k(3k-1)}{2}} =\ldots+x^{26}-x^{15}+x^7-x^2+1-x+x^5-x^{12}+x^{22}-\ldots
$$

Nhớ rằng đây là nghịch đảo của hàm sinh phân hoạch, nên nhân với hàm sinh phân hoạch được 1. So sánh hệ số, được truy hồi:

$$
(1+p_1 x+p_2 x^2+p_3 x^3+\ldots)(1-x-x^2+x^5+x^7-x^{12}-x^{15}+x^{22}+x^{26}-\ldots)=1
$$

$$
p_n=p_{n-1}+p_{n-2}-p_{n-5}-p_{n-7}+\ldots
$$

Truy hồi vô hạn, nhưng nếu quy ước $p_n=0$ với $n<0$ (và $p_0=1$), sẽ hữu hạn.

### Ví dụ

???+ note "Tính số phân hoạch"
    Tính $p_n$, nhiều test, $n\le 50000$, modulo $1000007$.
    
    Dùng định lý ngũ giác:
    
    ```cpp
    #include <cstdio>
    
    long long a[100010];
    long long p[50005];
    
    int main() {
      p[0] = 1;
      p[1] = 1;
      p[2] = 2;
      int i;
      for (i = 1; i < 50005;
           i++) /*hệ số truy hồi 1,2,5,7,12,15,22,26... i*(3*i-1)/2, i*(3*i+1)/2*/
      {
        a[2 * i] = i * (i * 3 - 1) / 2; /*số ngũ giác: 1,5,12,22... i*(3*i-1)/2*/
        a[2 * i + 1] = i * (i * 3 + 1) / 2;
      }
      for (
          i = 3; i < 50005;
          i++) /*p[n]=p[n-1]+p[n-2]-p[n-5]-p[n-7]+p[12]+p[15]-...+p[n-i*[3i-1]/2]+p[n-i*[3i+1]/2]*/
      {
        p[i] = 0;
        int j;
        for (j = 2; a[j] <= i; j++) /*có thể âm, trong biểu thức cộng 1000007*/
        {
          if (j & 2) {
            p[i] = (p[i] + p[i - a[j]] + 1000007) % 1000007;
          } else {
            p[i] = (p[i] - p[i - a[j]] + 1000007) % 1000007;
          }
        }
      }
      int n;
      while (~scanf("%d", &n)) {
        printf("%lld\n", p[n]);
      }
    }
    ```
