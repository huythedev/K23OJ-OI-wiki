author: Xeonacid, nocriz, ZnPdCo

## Giới thiệu

Biến đổi Walsh (Walsh Transform)[^note1] là một phương pháp thay thế biến đổi Fourier rời rạc trong phân tích phổ, được dùng rộng rãi trong xử lý tín hiệu. FFT dùng kiểu double, còn Walsh phân rã tín hiệu theo các sóng vuông tần số khác nhau, nên mọi hệ số đều là các số nguyên có trị tuyệt đối bằng nhau. Nhờ vậy không cần nhân số thực dấu phẩy động, tăng tốc độ tính toán.

Vì vậy, ý tưởng cốt lõi của FWT và FFT là giống nhau, đều là phép biến đổi trên mảng. Ký hiệu kết quả sau khi biến đổi Walsh nhanh của mảng $A$ là $FWT[A]$.

Ý tưởng cốt lõi của FWT là:

Ta cần một dãy mới $C$, được tạo từ dãy $A$ và $B$ theo một quy tắc nào đó, tức $C = A \cdot B$;

Ta biến đổi thuận để có $FWT[A], FWT[B]$, rồi dùng $FWT[C]=FWT[A] \cdot FWT[B]$ để tính $FWT[C]$ trong $O(n)$, trong đó $\cdot$ là nhân từng vị trí;

Sau đó biến đổi ngược để thu được dãy $C$. Độ phức tạp $O(n \log{n})$.

Trong thi đấu thuật toán, FWT dùng để giải các bài tích chập theo phép toán bit trên chỉ số.

Công thức: $C_{i} = \sum_{i=j \oplus k}A_{j} B_{k}$

(trong đó $\oplus$ là một phép toán bit nhị phân)

Dưới đây lấy ví dụ $\cup$ (OR bit), $\cap$ (AND bit) và $\oplus$ (XOR bit).

## Phép toán FWT

### Phép OR

Nếu $k=i\cup j$ thì các vị trí bit 1 của $i$ và $j$ chắc chắn là tập con của các vị trí bit 1 của $k$.

Để có $FWT[C] = FWT[A] \cdot FWT[B]$, ta cần xây quy tắc FWT.

Theo định nghĩa, có thể đặt $FWT[A]_i = A'_i = \sum_{i=i\cup j}A_{j}$, biểu diễn rằng $j$ có các bit 1 là tập con của $i$.

Khi đó:

$$
\begin{aligned}
FWT[A]_i\cdot FWT[B]_i&=\left(\sum_{i\cup j=i} A_j\right)\left(\sum_{i\cup k=i} B_k\right) \\
&=\sum_{i\cup j=i}\sum_{i\cup k=i}A_jB_k \\
&=\sum_{i\cup(j\cup k)=i}A_jB_k \\
&= FWT[C]_i
\end{aligned}
$$

Tiếp theo ta xem cách tính $FWT[A]$.

Không thể duyệt toàn bộ vì $O(n^2)$. Ta dùng chia để trị.

Chia đôi mảng, $A_0$ là nửa đầu, $A_1$ là nửa sau. Khi đó $A_0$ có bit cao nhất bằng 0, các tập con hợp lệ chỉ là chính nó; $A_1$ có bit cao nhất bằng 1, các tập con hợp lệ gồm cả các phần tử ở nửa đầu. Suy ra:

$$
FWT[A] = merge(FWT[A_0], FWT[A_0] + FWT[A_1])
$$

trong đó merge là ghép hai mảng, $+$ là cộng theo vị trí.

Như vậy với chia đôi, mỗi lần ghép tốn $O(n)$, tổng $O(n\log{n})$ cho $FWT[A]$.

Phép biến đổi ngược cũng đơn giản: $A_0$ chính là $FWT[A_0]$, còn $A_1$ là $FWT[A_0] + FWT[A_1]$, nên:

$$
UFWT[A'] = merge(UFWT[A_0'], UFWT[A_1'] - UFWT[A_0'])
$$

Dưới đây là mã. Dễ thấy có thể gộp thuận/ngược trong một hàm, thuận $\text{type}=1$, ngược $\text{type}=-1$.

???+ note "Cài đặt"
    ```cpp
    void Or(ll *a, ll type) {  // Cài đặt lặp, hằng số nhỏ hơn
      for (ll x = 2; x <= n; x <<= 1) {
        ll k = x >> 1;
        for (ll i = 0; i < n; i += x) {
          for (ll j = 0; j < k; j++) {
            (a[i + j + k] += a[i + j] * type) %= P;
          }
        }
      }
    }
    ```

### Phép AND

Tương tự OR, ta có:

$$
FWT[A] = merge(FWT[A_0] + FWT[A_1], FWT[A_1])
$$

$$
UFWT[A'] = merge(UFWT[A_0'] - UFWT[A_1'], UFWT[A_1'])
$$

Mã cài đặt (thuận $\text{type}=1$, ngược $\text{type}=-1$):

???+ note "Cài đặt"
    ```cpp
    void And(ll *a, ll type) {
      for (ll x = 2; x <= n; x <<= 1) {
        ll k = x >> 1;
        for (ll i = 0; i < n; i += x) {
          for (ll j = 0; j < k; j++) {
            (a[i + j] += a[i + j + k] * type) %= P;
          }
        }
      }
    }
    ```

### Phép XOR

Tích chập XOR dựa trên nguyên lý sau:

Đặt $x\circ y$ là tính chẵn lẻ của số bit 1 trong $x\cap y$, tức $x\circ y=\text{popcnt}(x\cap y)\bmod 2$, thì có $(x\circ y)\oplus (x\circ z)=x\circ(y\oplus z)$.

Đặt $FWT[A]_i=\sum_{i\circ j=0}A_j-\sum_{i\circ j=1}A_j$. Ta chứng minh $FWT[C] = FWT[A] \cdot FWT[B]$:

$$
\begin{aligned}
FWT[A]_iFWT[B]_i&=\left(\sum_{i\circ j=0}A_j-\sum_{i\circ j=1}A_j\right)\left(\sum_{i\circ k=0}B_k-\sum_{i\circ k=1}B_k\right) \\
&=\left(\sum_{i\circ j=0}A_j\sum_{i\circ k=0}B_k+\sum_{i\circ j=1}A_j\sum_{i\circ k=1}B_k\right)-\left(\sum_{i\circ j=0}A_j\sum_{i\circ k=1}B_k+\sum_{i\circ j=1}A_j\sum_{i\circ k=0}B_k\right) \\
&=\sum_{(j\oplus k)\circ i=0}A_jB_k-\sum_{(j\oplus k)\circ i=1}A_jB_k \\
&=FWT[C]_i
\end{aligned}
$$

Cách tính nhanh $A,B$ vẫn là chia để trị:

Với $i$ ở nửa có bit hiện tại là 0 (dãy con $FWT[A_0]$), khi tính $\circ$ thì kết quả với 0 hoặc 1 đều như nhau (vì $0\cap 0=0,0\cap1=0$), nên trong $FWT[A]=\sum_{i\circ j=0}A_j-\sum_{i\circ j=1}A_j$ ta có $\sum_{i\circ j=1}A_j=0$.

Với $i$ ở nửa có bit hiện tại là 1 ($A_1$), khi tính $\circ$ với 0 thì ra 0, với 1 thì ra 1 (vì $1\cap 0=0,1\cap1=1$).

Suy ra:

$$
FWT[A]=merge((FWT[A_0]+FWT[A_1])-0, FWT[A_0]-FWT[A_1])
$$

tức:

$$
FWT[A] = merge(FWT[A_0] + FWT[A_1], FWT[A_0] - FWT[A_1])
$$

Biến đổi ngược:

$$
UFWT[A'] = merge(\frac{UFWT[A_0'] + UFWT[A_1']}{2}, \frac{UFWT[A_0'] - UFWT[A_1']}{2})
$$

Mã cài đặt (thuận $\text{type}=1$, ngược $\text{type}=\frac{1}{2}$):

???+ note "Cài đặt"
    ```cpp
    void Xor(ll *a, ll type) {
      for (ll x = 2; x <= n; x <<= 1) {
        ll k = x >> 1;
        for (ll i = 0; i < n; i += x) {
          for (ll j = 0; j < k; j++) {
            (a[i + j] += a[i + j + k]) %= P;
            (a[i + j + k] = a[i + j] - a[i + j + k] * 2) %= P;
            (a[i + j] *= type) %= P;
            (a[i + j + k] *= type) %= P;
          }
        }
      }
    }
    ```

### Phép XNOR (đồng hoặc)

Tương tự XOR, ta có:

$FWT[A]_{i} = \sum_{C_1}A_{j} - \sum_{C_2}A_{j}$ ($C_1$ là $\text{popcnt}(x\cup y)\bmod 2$ bằng 0, $C_2$ là bằng 1)

$$
FWT[A] = merge(FWT[A_1] - FWT[A_0], FWT[A_1] + FWT[A_0])
$$

$$
UFWT[A'] = merge(\frac{UFWT[A_1'] - UFWT[A_0']}{2}, \frac{UFWT[A_1'] + UFWT[A_0']}{2})
$$

## Một góc nhìn khác về FWT

Gọi $c(i,j)$ là hệ số đóng góp của $A_j$ vào $FWT[A]_i$. Ta mô tả FWT:

$$
FWT[A]_i = \sum_{j=0}^{n-1} c(i,j) A_j
$$

Vì

$$
FWT[A]_i\cdot FWT[B]_i=FWT[C]_i
$$

nên có thể chứng minh: $c(i,j)c(i,k)=c(i,j\odot k)$, với $\odot$ là một phép bit bất kỳ.

Hàm $c$ còn có tính chất xử lý theo bit.

Ví dụ:

$$
FWT[A]_i = \sum_{j=0}^{n-1} c(i,j) A_j
$$

Cách này không hiệu quả, ta tách:

$$
FWT[A]_i = \sum_{j=0}^{n/2-1} c(i,j) A_j+\sum_{j=n/2}^{n-1} c(i,j) A_j
$$

Hai tổng khác nhau chỉ ở bit cao nhất.

Gọi $i',j'$ là $i,j$ bỏ bit cao nhất, $i_0$ là bit cao nhất của $i$:

$$
FWT[A]_i = c(i_0,0)\sum_{j=0}^{n/2-1} c(i',j') A_j+c(i_0,1)\sum_{j=n/2}^{n-1} c(i',j') A_j
$$

Nếu $i_0=0$:

$$
FWT[A]_i = c(0,0)\sum_{j=0}^{n/2-1} c(i',j') A_j+c(0,1)\sum_{j=n/2}^{n-1} c(i',j') A_j
$$

Nếu $i_0=1$:

$$
FWT[A]_i = c(1,0)\sum_{j=0}^{n/2-1} c(i',j') A_j+c(1,1)\sum_{j=n/2}^{n-1} c(i',j') A_j
$$

Vì vậy chỉ cần ma trận:

$$
\begin{bmatrix}
c(0,0) & c(0,1) \\
c(1,0) & c(1,1)
\end{bmatrix}
$$

bốn số là đủ. Gọi đó là ma trận bit.

Biến đổi ngược cần ma trận nghịch đảo.

Nếu nghịch đảo là $c^{-1}$, ta có:

$$
A_i = \sum_{j=0}^n c^{-1}(i,j) FWT[A]_j
$$

Nghịch đảo không phải lúc nào cũng tồn tại (ví dụ có một hàng hoặc cột toàn 0), nên cần cẩn thận khi xây.

### OR theo bit

Có thể chọn:

$$
\begin{bmatrix}
1 & 0 \\
1 & 1
\end{bmatrix}
$$

thỏa $c(i,j)c(i,k)=c(i,j\cup k)$. Đây đúng với công thức $FWT[A]=\text{merge}(FWT[A_0], FWT[A_0]+FWT[A_1])$. Dưới đây cũng thỏa nhưng thường dùng cái trên:

$$
\begin{bmatrix}
1 & 1 \\
1 & 0
\end{bmatrix}
$$

Ma trận sau không hợp lệ vì có hàng 0 nên không khả nghịch:

$$
\begin{bmatrix}
0 & 0 \\
1 & 1
\end{bmatrix}
$$

Lấy nghịch đảo của ma trận **đầu tiên**:

$$
\begin{bmatrix}
1 & 0 \\
-1 & 1
\end{bmatrix}
$$

Rồi thay vào cách biến đổi thuận để được biến đổi ngược.

### AND theo bit

Có thể chọn:

$$
\begin{bmatrix}
1 & 1 \\
0 & 1
\end{bmatrix}
$$

thỏa $c(i,j)c(i,k)=c(i,j\cap k)$.

Nghịch đảo:

$$
\begin{bmatrix}
1 & -1 \\
0 & 1
\end{bmatrix}
$$

### XOR theo bit

Có thể chọn:

$$
\begin{bmatrix}
1 & 1 \\
1 & -1
\end{bmatrix}
$$

thỏa $c(i,j)c(i,k)=c(i,j\oplus k)$.

Nghịch đảo:

$$
\begin{bmatrix}
0.5 & 0.5 \\
0.5 & -0.5
\end{bmatrix}
$$

## FWT là biến đổi tuyến tính

FWT là biến đổi tuyến tính, tức:

$$
FWT[A+B]=FWT[A]+FWT[B]
$$

và:

$$
FWT[c\cdot A]=c\cdot FWT[A]
$$

## FWT K chiều

Bản chất phép toán bit là thao tác trên vector $\{0,1\}$ $n$ chiều. OR là lấy $\max$ theo từng chiều. AND là lấy $\min$. XOR là cộng từng chiều rồi $\bmod 2$.

Đặc điểm: mỗi chiều độc lập.

Mở rộng $\{0,1\}$ thành $[0,K)\cap \mathbf{Z}$ (tức hệ cơ số $K$), ta được:

### Phép max

Mở rộng $\cup$ theo cơ số $K$, định nghĩa $i\cup j$ là lấy $\max$ theo từng chữ số:

$$
c(i,j)c(i,k)=c(i,j\cup k)
$$

Nếu $j=k$:

$$
c(i,j)c(i,j)=c(i,j)
$$

Nghĩa là mỗi hàng chỉ có 1 nằm trước 0, nếu 1 nằm sau 0 thì không hợp lệ. Có thể xây:

$$
\begin{bmatrix}
1 & 0 & 0 & 0 \\
1 & 1 & 0 & 0 \\
1 & 1 & 1 & 0 \\
1 & 1 & 1 & 1
\end{bmatrix}
$$

Nghịch đảo:

$$
\begin{bmatrix}
1 & 0 & 0 & 0 \\
-1 & 1 & 0 & 0 \\
0 & -1 & 1 & 0 \\
0 & 0 & -1 & 1
\end{bmatrix}
$$

### Phép min

Mở rộng $\cap$ theo cơ số $K$, định nghĩa $i\cap j$ là lấy $\min$ theo từng chữ số:

$$
c(i,j)c(i,k)=c(i,j\cap k)
$$

Nếu $j=k$:

$$
c(i,j)c(i,j)=c(i,j)
$$

Nghĩa là mỗi hàng chỉ có 1 nằm sau 0, nếu 1 nằm trước 0 thì không hợp lệ. Có thể xây:

$$
\begin{bmatrix}
1 & 1 & 1 & 1 \\
0 & 1 & 1 & 1 \\
0 & 0 & 1 & 1 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

Nghịch đảo:

$$
\begin{bmatrix}
1 & -1 & 0 & 0 \\
0 & 1 & -1 & 0 \\
0 & 0 & 1 & -1 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

Hai phép trên ít dùng, thường dùng nhất là:

### Cộng không nhớ

Mở rộng $\oplus$ theo cơ số $K$, định nghĩa $i\oplus j$ là cộng từng chữ số rồi $\bmod K$:

$$
c(i,j)c(i,k)=c(i,j\oplus k)
$$

Nếu đặt $c(i,j)=\omega_{K}^j$ thì thỏa:

$$
\omega_{K}^j\omega_{K}^k=\omega_{K}^{j\oplus k}
$$

Nhưng mỗi hàng giống nhau nên không có nghịch đảo. Có thể chọn $c(i,j)=\omega_{K}^{(i-1)j}$.

Ma trận khi đó:

$$
\begin{bmatrix}
1 & 1 & 1 & \cdots & 1 \\
1 & \omega_{K}^1 & \omega_{K}^2 & \cdots & \omega_{K}^{k-1} \\
1 & \omega_{K}^2 & \omega_{K}^4 & \cdots & \omega_{K}^{2(k-1)} \\
1 & \omega_{K}^3 & \omega_{K}^6 & \cdots & \omega_{K}^{3(k-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1 & \omega_{K}^{k-1} & \omega_{K}^{2(k-1)} & \cdots & \omega_{K}^{(k-1)(k-1)}
\end{bmatrix}
$$

Đây là [ma trận Vandermonde](https://en.wikipedia.org/wiki/Vandermonde_matrix), nghịch đảo:

$$
\frac{1}{K}\begin{bmatrix}
1 & 1 & 1 & \cdots & 1 \\
1 & \omega_{K}^{-1} & \omega_{K}^{-2} & \cdots & \omega_{K}^{-(k-1)} \\
1 & \omega_{K}^{-2} & \omega_{K}^{-4} & \cdots & \omega_{K}^{-2(k-1)} \\
1 & \omega_{K}^{-3} & \omega_{K}^{-6} & \cdots & \omega_{K}^{-3(k-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1 & \omega_{K}^{-(k-1)} & \omega_{K}^{-2(k-1)} & \cdots & \omega_{K}^{-(k-1)(k-1)}
\end{bmatrix}
$$

Nếu mô-đun có đơn vị căn bậc $K$, ta có thể hiện thực trực tiếp.

Nhưng **đơn vị căn trong modulo có thể không tồn tại**, nên ta xét mở rộng trường: tự định nghĩa một $x$ sao cho $x^K=1$, rồi thay $x$ vào, khi đó mỗi số là đa thức bậc $k-1$ theo $x$. Ta chỉ cần tính trong $\bmod {x^K-1}$, ma trận trở thành:

$$
\begin{bmatrix}
1 & 1 & 1 & \cdots & 1 \\
1 & x^1 & x^2 & \cdots & x^{k-1} \\
1 & x^2 & x^4 & \cdots & x^{2(k-1)} \\
1 & x^3 & x^6 & \cdots & x^{3(k-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
1 & x^{k-1} & x^{2(k-1)} & \cdots & x^{(k-1)(k-1)}
\end{bmatrix}
$$

Cách này có thể có ước số không (zero divisor), tức **một số có nhiều cách biểu diễn**, không xác định được giá trị thực.

Ta dùng $\bmod \Phi_{K}(x)$, trong đó $\Phi_{K}(x)$ là đa thức cyclotomic, bậc của $x$ là $k$ và không thể phân tích trên $\mathbb{Q}$. Vì vậy tính toán trong $\bmod {\Phi_{K}(x)}$.

Vấn đề là $\bmod \Phi_{K}(x)$ có hằng số lớn. Nhưng vì $\Phi_{K}(x)\mid x^k-1$, ta chỉ cần tính $\bmod x^k-1$ trong quá trình, cuối cùng mới $\bmod \Phi_{K}(x)$.

## Ví dụ

???+ note "[「CF 1103E」Radix sum](https://www.luogu.com.cn/problem/CF1103E)"
    Cho dãy $a_1,a_2,...,a_n$, với mỗi $p \in [0,n-1]$, đếm số dãy số nguyên $i_1,i_2,...,i_n$ thỏa:
    
    -   $\forall j \in [1,n] , i_j \in [1,n]$;
    -   $\sum\limits_{j=1}^n a_{i_j} = p$, trong đó phép cộng là cộng không nhớ theo hệ 10.
    
    $n\le10^5,a_i\le10^5$
    
    ??? note "Lời giải"
        Ta có thể dùng DP: đặt $f_{i,s}$ là số cách xét tới phần tử thứ $i$, trạng thái cộng hiện tại là $s$. Vì FWT là tuyến tính, ta có thể biến đổi FWT sang dạng điểm, nâng lũy thừa bậc $n$, rồi biến đổi ngược.
        
        Điều trên là hiển nhiên, nhưng mô-đun là $2^{58}$. Không có đơn vị căn, nên ta xét mở rộng trường.
        
        Đa thức cyclotomic ở đây là $\Phi_{10}(x)=x^4-x^3+x^2-x+1$.
        
        Tuy nhiên khi UFWT cần chia cho cơ số 10, mà 10 không có nghịch đảo modulo $2^{58}$. Ta thấy 5 có nghịch đảo modulo $2^{58}$: $57646075230342349$. Chỉ cần chia thêm một 2. Gọi đáp án sau khi đã chia cho 5 là $x$, đáp án thật là $y$, tức $2^5y\equiv x\pmod{2^{64}}$. Suy ra $y\equiv \frac{x}{2^5}\pmod{2^{64-5}}$, tức $y\equiv \frac{x}{2^5}\pmod{2^{59}}$, nên chỉ cần chia đáp án cuối cùng cho $2^5$. Dù đề không rõ vì sao mod $2^{58}$, ta chỉ cần lấy lại modulo.

???+ note "[【CF103329F】【XXII Opencup, Grand Prix of XiAn】The Struggle](https://codeforces.com/gym/103329/problem/F)"
    Cho elip $E$, mọi điểm nguyên có tọa độ trong $[1,4 \cdot 10^6]$. Tính $\sum_{(x,y) \in E} (x \oplus y)^{33}x^{-2}y^{-1} \mod 10^9+7$.
    
    ??? note "Lời giải"
        Bài này không quá “lộ” hướng. Tác giả cung cấp lời giải tiếng Anh, xem [liên kết này](https://codeforces.com/blog/entry/96518).

## Tài liệu tham khảo

-   [Ghi chép thuật toán của 桃酱](https://zhuanlan.zhihu.com/p/41867199)
-   [Blog của ZnPdCo](https://znpdco.github.io/%E7%AE%97%E6%B3%95/2024/05/07/FWT.html)

[^note1]: [Wikipedia](https://zh.wikipedia.org/zh-cn/%E6%B2%83%E7%88%BE%E4%BB%80%E8%BD%89%E6%8F%9B)
