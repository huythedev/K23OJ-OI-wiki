author: H-J-Granger, ranwen, abc1763613206, Ahacad, Allenyou1126, AndrewWayne, AngelKitty, AtomAlpaca, Backl1ght, billchenchina, c-forrest, CCXXXI, Cheuring, Chrogeek, ChungZH, countercurrent-time, DepletedPrism, Early0v0, EarthMessenger, Enter-tainer, F1shAndCat, GavinZhengOI, Gesrua, Great-designer, greyqz, Haohu Shen, henryrabbit, heroming, hly1204, Ir1d, isdanni, jiang1997, kenlig, Lewy Zeng, lucifer1004, Menci, muoshuosha, NachtgeistW, needtocalmdown, opsiff, ouuan, ouuan, partychicken, schtonn, Sshwy, sshwy, StudyingFather, SukkaW, Taoran-01, Tiphereth-A, TrisolarisHD, untitledunrevised, Xeonacid, YouXam, Yukimaikoriya

Kiến thức tiền đề: [Số phức](../complex.md).

Bài viết này giới thiệu một thuật toán có thể tính tích của hai đa thức bậc $n$ trong thời gian $O(n\log n)$, hiệu quả hơn thuật toán ngây thơ $O(n^2)$. Vì phép nhân hai số nguyên cũng có thể xem như nhân hai đa thức, nên thuật toán này cũng dùng để tăng tốc nhân số nguyên lớn.

## Mở đầu

Xét hai đa thức $A$ và $B$:

$$
\begin{aligned}
A ={}& 5x^2 + 3x + 7 \\
B ={}& 7x^2 + 2x + 1 \\
\end{aligned}
$$

Tích $C = A \times B$ có thể tính trong $O(n^2)$ (với $n$ là bậc của $A$ hoặc $B$):

$$
\begin{aligned}
C ={}& A \times B \\
  ={}& 35x^4 + 31x^3 + 60x^2 + 17x + 7
\end{aligned}
$$

Rõ ràng hệ số $c_i$ của $C$ thỏa $c_i = \sum_{j = 0}^i a_j b_{i - j}$. Với thuật toán ngây thơ, mỗi hệ số tốn $O(n)$, có $O(n)$ hệ số nên tổng là $O(n^2)$.

Có thể tăng tốc không? Nếu dùng FFT, ta giảm xuống $O(n\log n)$.

## Biến đổi Fourier

Biến đổi Fourier (Fourier Transform) là phương pháp phân tích tín hiệu: phân rã tín hiệu thành các thành phần hoặc tổng hợp tín hiệu từ các thành phần. Nhiều dạng sóng có thể làm thành phần; biến đổi Fourier dùng sóng sin làm thành phần.

Cho $f(t)$ theo thời gian $t$, biến đổi Fourier đo mức độ xuất hiện của tần số $\omega$ trong $f(t)$:

$$
F(\omega)=\mathbb{F}[f(t)]=\int_{-\infty}^{\infty}f(t)\mathrm{e}^{-\mathrm{i}{\omega}t}dt
$$

Biến đổi ngược:

$$
f(t)=\mathbb{F}^{-1}[F(\omega)]=\frac{1}{2\pi}\int_{-\infty}^{\infty}F(\omega)\mathrm{e}^{\mathrm{i}{\omega}t}d\omega
$$

Dạng của biến đổi ngược rất giống biến đổi thuận; mẫu $2\pi$ đúng bằng chu kỳ của hàm mũ phức.

Biến đổi Fourier tương đương lấy tích vô hướng liên tục giữa hàm miền thời gian và hàm mũ phức chu kỳ $2\pi$. Biến đổi ngược cũng là một tích vô hướng.

Biến đổi Fourier có định lý chập, chuyển chập miền thời gian thành tích miền tần số, và ngược lại.

## Biến đổi Fourier rời rạc

**Biến đổi Fourier rời rạc** (Discrete Fourier Transform, DFT) là dạng rời rạc theo cả thời gian và tần số, biến mẫu thời gian thành mẫu của DTFT (discrete-time Fourier transform) trong miền tần số.

Biến đổi Fourier là tích vô hướng dạng tích phân; DFT là tích vô hướng dạng tổng.

Cho dãy $\{x_n\}_{n=0}^{N-1}$ thỏa điều kiện hữu hạn, DFT là:

$$
X_k=\sum_{n=0}^{N-1}x_n\mathrm{e}^{-\mathrm{i}\frac{2\pi}{N}kn}
$$

trong đó $\mathrm{e}$ là cơ số logarit tự nhiên, $i$ là đơn vị ảo. Thường ký hiệu phép biến đổi là $\mathcal {F}$:

$$
\hat{x}=\mathcal{F}x
$$

Tương tự, **nghịch biến đổi Fourier rời rạc** (IDFT) là:

$$
x_n=\frac{1}{N}\sum_{k=0}^{N-1}X_k\mathrm{e}^{\mathrm{i}\frac{2\pi}{N}kn}
$$

ký hiệu:

$$
x=\mathcal{F}^{-1}\hat{x}
$$

Hệ số chuẩn hóa ở DFT/IDFT không quá quan trọng. Ở định nghĩa trên, DFT có hệ số $1$, IDFT có hệ số $\frac{1}{N}$. Đôi khi cả hai cùng lấy $\frac{1}{\sqrt{N}}$.

DFT vẫn là biến đổi từ miền thời gian sang miền tần số. Vì là dạng tổng nên có cách diễn giải khác.

Nếu xem dãy $x_n$ là hệ số của đa thức $f(x)$ tại $x^n$, thì $X_k$ chính là giá trị $f(\mathrm{e}^{\frac{-2\pi \mathrm{i}k}{N}})$ tại các căn đơn vị.

Vì vậy DFT là phép đánh giá đa thức tại các căn đơn vị.

Ví dụ, tính:

$$
\dbinom{n}{3}+\dbinom{n}{7}+\dbinom{n}{11}+\dbinom{n}{15}+\ldots
$$

Đặt

$$
f(x)={(1+x)}^n=\dbinom{n}{0}x^0+\dbinom{n}{1}x^1+\dbinom{n}{2}x^2+\dbinom{n}{3}x^3+\ldots
$$

Khi đó tại căn bậc 4 của đơn vị:

$$
f(\mathrm{i})={(1+\mathrm{i})}^n=\dbinom{n}{0}+\dbinom{n}{1}\mathrm{i}-\dbinom{n}{2}-\dbinom{n}{3}\mathrm{i}+\ldots
$$

Ta có:

$$
f(1)+\mathrm{i}f(\mathrm{i})-f(-1)-\mathrm{i}f(-\mathrm{i})=4\dbinom{n}{3}+4\dbinom{n}{7}+4\dbinom{n}{11}+4\dbinom{n}{15}+\ldots
$$

Suy ra:

$$
\dbinom{n}{3}+\dbinom{n}{7}+\dbinom{n}{11}+\dbinom{n}{15}+\ldots=\frac{2^n+\mathrm{i}(1+\mathrm{i})^n-\mathrm{i}(1-\mathrm{i})^n}{4}
$$

Bài toán này chính là đánh giá tại căn đơn vị, tương ứng DFT.

### Công thức ma trận

Vì DFT là **toán tử tuyến tính**, có thể biểu diễn bằng nhân ma trận:

$$
\begin{bmatrix}
    X_{0}  \\
    X_{1}  \\
    X_{2}  \\
    \vdots \\
    X_{N-1}
\end{bmatrix}
=
\begin{bmatrix}
    1      & 1            & 1               & \cdots & 1                   \\
    1      & \alpha       & \alpha^{2}      & \cdots & \alpha^{N-1}        \\
    1      & \alpha^{2}   & \alpha^{4}      & \cdots & \alpha^{2(N-1)}     \\
    \vdots & \vdots       & \vdots          & \ddots & \vdots              \\
    1      & \alpha^{N-1} & \alpha^{2(N-1)} & \cdots & \alpha^{(N-1)(N-1)}
\end{bmatrix}
\begin{bmatrix}
    x_{0}  \\
    x_{1}  \\
    x_{2}  \\
    \vdots \\
    x_{N-1}
\end{bmatrix}
$$

với $\alpha = \mathrm{e}^{-\mathrm{i}\frac{2\pi}{N}}$.

## Biến đổi Fourier nhanh

FFT (Fast Fourier Transform) là thuật toán tính DFT hiệu quả. Nó không phát hiện lý thuyết mới, nhưng là bước tiến lớn trong ứng dụng DFT trên máy tính. NTT (Number Theoretic Transform) là FFT trên nền số học.

Năm 1965, Cooley và Tukey công bố FFT. Thực ra FFT đã được biết trước đó, nhưng khi ấy máy tính hiện đại chưa ra đời nên tầm quan trọng không được nhận ra. Có ý kiến cho rằng Runge và König phát hiện năm 1924, nhưng Gauss đã có thuật toán từ năm 1805 mà chưa công bố.

### Cài đặt chia để trị

Ý tưởng cơ bản của FFT là chia để trị. Với DFT, ta chia để trị theo đánh giá tại $x=\omega_n^k$. Với FFT cơ số 2, chia đa thức theo hạng chẵn và hạng lẻ.

Ví dụ đa thức 8 hạng:

$$
f(x) = a_0 + a_1x + a_2x^2+a_3x^3+a_4x^4+a_5x^5+a_6x^6+a_7x^7
$$

Tách theo chẵn/lẻ:

$$
\begin{aligned}
f(x) &= (a_0+a_2x^2+a_4x^4+a_6x^6) + (a_1x+a_3x^3+a_5x^5+a_7x^7)\\
     &= (a_0+a_2x^2+a_4x^4+a_6x^6) + x(a_1+a_3x^2+a_5x^4+a_7x^6)
\end{aligned}
$$

Đặt:

$$
\begin{aligned}
G(x) &= a_0+a_2x+a_4x^2+a_6x^3\\
H(x) &= a_1+a_3x+a_5x^2+a_7x^3
\end{aligned}
$$

khi đó:

$$
f(x)=G\left(x^2\right) + x  \times  H\left(x^2\right)
$$

Dùng tính chất $\omega^i_n = -\omega^{i + n/2}_n$ và $G(x^2),H(x^2)$ là hàm chẵn, ta có:

$$
\begin{aligned}
f(\omega_n^k) &= G((\omega_n^k)^2) + \omega_n^k  \times H((\omega_n^k)^2) \\
              &= G(\omega_n^{2k}) + \omega_n^k  \times H(\omega_n^{2k}) \\
              &= G(\omega_{n/2}^k) + \omega_n^k  \times H(\omega_{n/2}^k)
\end{aligned}
$$

và:

$$
\begin{aligned}
f(\omega_n^{k+n/2}) &= G(\omega_n^{2k+n}) + \omega_n^{k+n/2}  \times H(\omega_n^{2k+n}) \\
                    &= G(\omega_n^{2k}) - \omega_n^k  \times H(\omega_n^{2k}) \\
                    &= G(\omega_{n/2}^k) - \omega_n^k  \times H(\omega_{n/2}^k)
\end{aligned}
$$

Vì vậy, khi có $G(\omega_{n/2}^k)$ và $H(\omega_{n/2}^k)$, ta đồng thời có $f(\omega_n^k)$ và $f(\omega_n^{k+n/2})$. Đệ quy DFT cho $G$ và $H$ là xong.

Do DFT chia để trị yêu cầu độ dài là $2^m (m\in\mathbf{N}^\ast)$, nên trước khi DFT phải đệm $0$ để độ dài thành lũy thừa của 2, bậc cao nhất là $2^m-1$.

Khi đánh giá, ta dùng $n$ điểm $\omega_n^0,\omega_n^1,\ldots,\omega_n^{n-1}$ với $n=2^m$.

Về cài đặt, STL có `complex` và `exp` để lấy $\omega_n$; hoặc dùng công thức Euler để tính số phức cũng tương đương.

Đây là phần DFT của FFT: chuyển đa thức từ biểu diễn hệ số sang biểu diễn giá trị.

Lưu ý: vì là căn đơn vị phức, cần đệm lên $n=2^k$.

???+ note "FFT đệ quy"
    ```cpp
    #include <cmath>
    #include <complex>
    
    using Comp = std::complex<double>;  // complex của STL
    
    constexpr Comp I(0, 1);  // i
    constexpr int MAX_N = 1 << 20;
    
    Comp tmp[MAX_N];
    
    // rev=1, DFT; rev=-1, IDFT
    // Sau khi gọi hàm cần lưu ý xử lý hệ số chuẩn hóa
    void DFT(Comp* f, int n, int rev) {
      if (n == 1) return;
      for (int i = 0; i < n; ++i) tmp[i] = f[i];
      // Chẵn sang trái, lẻ sang phải
      for (int i = 0; i < n; ++i) {
        if (i & 1)
          f[n / 2 + i / 2] = tmp[i];
        else
          f[i / 2] = tmp[i];
      }
      Comp *g = f, *h = f + n / 2;
      // Đệ quy DFT
      DFT(g, n / 2, rev), DFT(h, n / 2, rev);
      // cur là căn đơn vị hiện tại; với k=0 thì omega^0_n = 1
      // step là bước giữa hai căn đơn vị: omega^k_n = step*omega^{k-1}_n
      // Tương đương exp(I*(-2*M_PI/n*rev))
      Comp cur(1, 0), step(cos(2 * M_PI / n), sin(-2 * M_PI * rev / n));
      for (int k = 0; k < n / 2;
           ++k) {  // F(omega^k_n) = G(omega^k_{n/2}) + omega^k_n*H(omega^k_{n/2})
        tmp[k] = g[k] + cur * h[k];
        // F(omega^{k+n/2}_n) = G(omega^k_{n/2}) - omega^k_n*H(omega^k_{n/2})
        tmp[k + n / 2] = g[k] - cur * h[k];
        cur *= step;
      }
      for (int i = 0; i < n; ++i) f[i] = tmp[i];
    }
    ```

Độ phức tạp $O(n\log n)$.

### Cài đặt nhân đôi

Thuật toán có thể tối ưu theo hướng “giả lập đệ quy”. Với FFT cơ số 2, ta tách chẵn/lẻ đến khi còn một hệ số, nhưng đệ quy tốn bộ nhớ. Thay vào đó, có thể “tách” trong mảng bằng hoán vị đảo bit, rồi “nhân đôi” để trộn kết quả.

“Trộn” dùng phép biến đổi bướm để chỉ cần $O(1)$ bộ nhớ phụ.

#### Hoán vị đảo bit

Ví dụ đa thức 8 hạng:

-   Ban đầu: $\{x_0, x_1, x_2, x_3, x_4, x_5, x_6, x_7\}$
-   Sau 1 lần chia: $\{x_0, x_2, x_4, x_6\},\{x_1, x_3, x_5, x_7 \}$
-   Sau 2 lần: $\{x_0,x_4\} \{x_2, x_6\},\{x_1, x_5\},\{x_3, x_7 \}$
-   Sau 3 lần: $\{x_0\}\{x_4\}\{x_2\}\{x_6\}\{x_1\}\{x_5\}\{x_3\}\{x_7 \}$

Quy luật: viết chỉ số ở nhị phân rồi đảo bit, đó là vị trí mới. Ví dụ $x_1$ là 001, đảo thành 100 tức 4. Biến đổi này gọi là hoán vị đảo bit (bit-reversal permutation).

Có thể tính $O(n)$:

???+ note "Hoán vị đảo bit ($O(n)$)"
    ```cpp
    /*
     * Biến đổi đảo bit trước FFT và IFFT
     * Vị trí i và vị trí đảo bit của i hoán đổi
     * len phải là lũy thừa của 2
     */
    void change(Complex y[], int len) {
      // Ban đầu i là 0...01, j là 10...0, đối xứng nhau trong nhị phân
      // i tăng dần, j luôn đối xứng với i, đến khi i = 1...11
      for (int i = 1, j = len / 2, k; i < len - 1; i++) {
        // Hoán đổi cặp đảo bit; i < j đảm bảo chỉ đổi một lần
        if (i < j) swap(y[i], y[j]);
        // i tăng bình thường, j tăng theo quy tắc đảo bit để luôn đối xứng
        // k là bit cao nhất đang là 0. j trừ dần phần toàn 1 ở cao vị
        // đến khi gặp 0 thì cộng lại
        // Số lần lật bit: bit cao nhất lật n lần, tiếp theo lật n/2 lần, ...
        // T(n) = n + n/2 + n/4 + ... = O(n)
        k = len / 2;
        while (j >= k) {
          j = j - k;
          k = k / 2;
        }
        j += k;
      }
    }
    ```

Cũng có thể tính từ nhỏ đến lớn. Với $len=2^k$, ký hiệu $R(x)$ là số đảo bit của $x$ (đệm 0 ở đầu). Ta cần $R(0),\ldots,R(n-1)$.

$R(0)=0$. Khi tính $R(x)$, ta đã biết $R(\lfloor x/2 \rfloor)$. Dịch phải 1 bit rồi đảo, rồi lại dịch phải 1 bit sẽ cho phần đảo của tất cả bit trừ bit thấp nhất.

Nếu bit thấp nhất là 0 thì bit cao nhất sau đảo là 0; nếu là 1 thì bit cao nhất là 1, nên cộng thêm $\frac{len}{2}=2^{k-1}$.

$$
R(x)=\left\lfloor \frac{R\left(\left\lfloor \frac{x}{2} \right\rfloor\right)}{2} \right\rfloor + (x\bmod 2)\times \frac{len}{2}
$$

Ví dụ $k=5$, $len=(100000)_2$. Đảo $(11001)_2$:

1.  Xét $(1100)_2$, ta có $R((1100)_2)=R((01100)_2)=(00110)_2$, dịch phải 1 được $(00011)_2$.
2.  Xét bit thấp nhất: nếu là 1 thì cộng $(10000)_2=2^{k-1}$; nếu là 0 thì không cộng.

???+ note "Hoán vị đảo bit ($O(n)$)"
    ```cpp
    // Vẫn cần len là lũy thừa của 2
    // rev[i] là giá trị đảo bit của i
    void change(Complex y[], int len) {
      for (int i = 0; i < len; ++i) {
        rev[i] = rev[i >> 1] >> 1;
        if (i & 1) {  // Nếu bit cuối là 1 thì đảo thành len/2
          rev[i] |= len >> 1;
        }
      }
      for (int i = 0; i < len; ++i) {
        if (i < rev[i]) {  // Mỗi cặp chỉ hoán đổi một lần
          swap(y[i], y[rev[i]]);
        }
      }
      return;
    }
    ```

#### Tối ưu phép biến đổi bướm

Biết $G(\omega_{n/2}^k)$ và $H(\omega_{n/2}^k)$, ta cần:

$$
\begin{aligned}
    f(\omega_n^k)       & = G(\omega_{n/2}^k) + \omega_n^k \times H(\omega_{n/2}^k) \\
    f(\omega_n^{k+n/2}) & = G(\omega_{n/2}^k) - \omega_n^k \times H(\omega_{n/2}^k)
\end{aligned}
$$

Sau hoán vị đảo bit, với $n,k$:

-   $G(\omega_{n/2}^k)$ ở vị trí $k$, $H(\omega_{n/2}^k)$ ở vị trí $k+\frac{n}{2}$.
-   $f(\omega_n^k)$ sẽ lưu ở vị trí $k$, $f(\omega_n^{k+n/2})$ ở vị trí $k+\frac{n}{2}$.

Vì vậy có thể ghi đè trực tiếp không cần mảng phụ. Đây là **phép biến đổi bướm** (cơ số 2).

Cách trộn các đoạn dài $\frac{n}{2}$:

1.  Đặt độ dài đoạn $s=\frac{n}{2}$;
2.  Duyệt đầu đoạn của $\{G(\omega_{n/2}^k)\}$ là $l_g=0,2s,4s,\ldots,N-2s$, và của $\{H(\omega_{n/2}^k)\}$ là $l_h=s,3s,5s,\ldots,N-s$;
3.  Khi trộn, duyệt $k=0,1,\ldots,s-1$, khi đó $G$ ở $l_g+k$, $H$ ở $l_h+k$;
4.  Dùng phép bướm để ra $f(\omega_n^k)$ và $f(\omega_n^{k+n/2})$, ghi đè.

## Biến đổi Fourier ngược nhanh

Biến đổi Fourier ngược có thể biểu diễn qua biến đổi Fourier. Có hai cách hiểu.

### Góc nhìn đại số tuyến tính

IDFT biến biểu diễn điểm thành biểu diễn hệ số. DFT là biến đổi tuyến tính, có thể xem như nhân ma trận:

$$
\begin{bmatrix}y_0 \\ y_1 \\ y_2 \\ y_3 \\ \vdots \\ y_{n-1} \end{bmatrix}
=
\begin{bmatrix}1 & 1 & 1 & 1 & \cdots & 1 \\
1 & \omega_n^1 & \omega_n^2 & \omega_n^3 & \cdots & \omega_n^{n-1} \\
1 & \omega_n^2 & \omega_n^4 & \omega_n^6 & \cdots & \omega_n^{2(n-1)} \\
1 & \omega_n^3 & \omega_n^6 & \omega_n^9 & \cdots & \omega_n^{3(n-1)} \\
\vdots & \vdots & \vdots & \vdots & \ddots & \vdots \\
1 & \omega_n^{n-1} & \omega_n^{2(n-1)} & \omega_n^{3(n-1)} & \cdots & \omega_n^{(n-1)^2} \end{bmatrix}
\begin{bmatrix} a_0 \\ a_1 \\ a_2 \\ a_3 \\ \vdots \\ a_{n-1} \end{bmatrix}
$$

Muốn khôi phục hệ số, nhân hai vế với nghịch đảo của ma trận giữa. Ma trận này có cấu trúc đặc biệt: chỉ cần **lấy nghịch đảo** từng phần tử, rồi **chia cho $n$**.

Lưu ý: độ dài FFT không phải độ dài đa thức; độ dài biến đổi phải đủ lớn cho tích, nên cần đệm $0$ ở bậc cao.

Theo Euler:

$$
\frac{1}{\omega_k}=\omega_k^{-1}=\mathrm{e}^{-\frac{2\pi \mathrm{i}}{k}}=\cos\left(\frac{2\pi}{k}\right)+\mathrm{i} \sin\left(-\frac{2\pi}{k}\right)
$$

Do đó ta có thể dùng căn đơn vị đảo: lấy $\omega_k=\mathrm{e}^{-\frac{2\pi \mathrm{i}}{k}}$, rồi chia cho $n$. Ta có thể truyền tham số $1$ hoặc $-1$ để nhân vào $\pi$: $1$ là DFT, $-1$ là IDFT.

### Tính chu kỳ của căn đơn vị

Cũng có thể hiểu IDFT qua tính chu kỳ của căn đơn vị.

Gọi $f(x)=\sum_{i=0}^{n-1}a_ix^i$. IDFT biến từ giá trị tại căn đơn vị về hệ số.

Dùng **phương pháp xây dựng**. Biết $y_i=f(\omega_n^i)$, cần tìm $\{a_0,\ldots,a_{n-1}\}$. Đặt

$$
A(x)=\sum_{i=0}^{n-1}y_ix^i
$$

Tức coi $\{y_i\}$ là hệ số của $A$.

Có hai cách suy ra:

#### Cách 1

Đặt $b_i=\omega_n^{-i}$. Khi đó $A(b_k)$:

$$
\begin{aligned}
A(b_k)&=\sum_{i=0}^{n-1}f(\omega_n^i)\omega_n^{-ik}=\sum_{i=0}^{n-1}\omega_n^{-ik}\sum_{j=0}^{n-1}a_j(\omega_n^i)^{j}\\
&=\sum_{i=0}^{n-1}\sum_{j=0}^{n-1}a_j\omega_n^{i(j-k)}=\sum_{j=0}^{n-1}a_j\sum_{i=0}^{n-1}\left(\omega_n^{j-k}\right)^i\\
\end{aligned}
$$

Gọi $S(\omega_n^a)=\sum_{i=0}^{n-1}(\omega_n^a)^i$.

Nếu $a=0 \pmod{n}$ thì $S(\omega_n^a)=n$. Nếu $a\neq 0 \pmod{n}$:

$$
\begin{aligned}
S\left(\omega_n^a\right)&=\sum_{i=0}^{n-1}\left(\omega_n^a\right)^i\\
\omega_n^a S\left(\omega_n^a\right)&=\sum_{i=1}^{n}\left(\omega_n^a\right)^i\\
S\left(\omega_n^a\right)&=\frac{\left(\omega_n^a\right)^n-\left(\omega_n^a\right)^0}{\omega_n^a-1}=0\\
\end{aligned}
$$

Tức:

$$
S\left(\omega_n^a\right)=
\begin{cases}
n,&a=0\\
0,&a\neq 0
\end{cases}
$$

Suy ra:

$$
A(b_k)=\sum_{j=0}^{n-1}a_jS\left(\omega_n^{j-k}\right)=a_k\cdot n
$$

Vậy nếu lấy các điểm $b_i=\omega_n^{-i}$, thì

$$
\begin{aligned}
&\left\{ (b_0,A(b_0)),(b_1,A(b_1)),\cdots,(b_{n-1},A(b_{n-1})) \right\}\\
=&\left\{ (b_0,a_0\cdot n),(b_1,a_1\cdot n),\cdots,(b_{n-1},a_{n-1}\cdot n) \right\}
\end{aligned}
$$

Tức lấy căn đơn vị đảo, chạy FFT cho $\{y_i\}$ rồi chia $n$ là ra hệ số.

#### Cách 2

Trực tiếp thay $\omega_n^i$ vào $A(x)$.

Suy luận tương tự, có $A(\omega_n^k)=\sum_{j=0}^{n-1}a_jS(\omega_n^{j+k})$. Chỉ khi $j+k=0\pmod{n}$ thì $S(\omega_n^{j+k})=n$, nên $A(\omega_n^k)=a_{n-k}\cdot n$.

Tức DFT trên $\{y_i\}$, chia $n$, rồi đảo $n-1$ phần tử là thu được hệ số.

### Cài đặt

Vì vậy FFT có thể dùng cho cả DFT và IDFT:

???+ note "FFT không đệ quy (cách 1)"
    ```cpp
    /*
     * Thực hiện FFT
     * len phải là dạng 2^k
     * on == 1 là DFT, on == -1 là IDFT
     */
    void fft(Complex y[], int len, int on) {
      // Hoán vị đảo bit
      change(y, len);
      // Mô phỏng hợp nhất: từ độ dài 1 lên 2, rồi lên 4, ... tới len
      for (int h = 2; h <= len; h <<= 1) {
        // wn: khoảng giữa các căn đơn vị hiện tại: w^1_h
        Complex wn(cos(2 * PI / h), sin(on * 2 * PI / h));
        // Hợp nhất, có len / h lần
        for (int j = 0; j < len; j += h) {
          // w bắt đầu từ 1 = w^0_n, rồi tăng theo wn: w^1_n, ...
          Complex w(1, 0);
          for (int k = j; k < j + h / 2; k++) {
            // Nửa trái và nửa phải là nghiệm của bài toán con
            Complex u = y[k];
            Complex t = w * y[k + h / 2];
            // Gộp hai nửa
            y[k] = u + t;
            y[k + h / 2] = u - t;
            // Nửa sau của “step” luôn đối xứng âm với nửa trước
            // Quay một vòng là trở lại, quay nửa vòng là đổi dấu
            // Bình phương của số và của đối nó là bằng nhau
            w = w * wn;
          }
        }
      }
      // Nếu là IDFT, còn phải chia mỗi phần tử cho len
      if (on == -1) {
        for (int i = 0; i < len; i++) {
          y[i].x /= len;
          y[i].y /= len;
        }
      }
    }
    ```

???+ note "FFT không đệ quy (cách 2)"
    ```cpp
    /*
     * Thực hiện FFT
     * len phải là dạng 2^k
     * on == 1 là DFT, on == -1 là IDFT
     */
    void fft(Complex y[], int len, int on) {
      change(y, len);
      for (int h = 2; h <= len; h <<= 1) {             // Mô phỏng hợp nhất
        Complex wn(cos(2 * PI / h), sin(2 * PI / h));  // Tính căn đơn vị hiện tại
        for (int j = 0; j < len; j += h) {
          Complex w(1, 0);  // Tính căn đơn vị hiện tại
          for (int k = j; k < j + h / 2; k++) {
            Complex u = y[k];
            Complex t = w * y[k + h / 2];
            y[k] = u + t;  // Gộp hai nửa
            y[k + h / 2] = u - t;
            // Nửa sau của “step” luôn đối xứng âm với nửa trước
            // Quay một vòng là trở lại, quay nửa vòng là đổi dấu
            // Bình phương của số và của đối nó là bằng nhau
            w = w * wn;
          }
        }
      }
      if (on == -1) {
        reverse(y + 1, y + len);
        for (int i = 0; i < len; i++) {
          y[i].x /= len;
          y[i].y /= len;
        }
      }
    }
    ```

??? note "Mẫu FFT ([HDU 1402 - A * B Problem Plus](http://acm.hdu.edu.cn/showproblem.php?pid=1402))"
    ```cpp
    --8<-- "docs/math/code/poly/fft/fft_3.cpp"
    ```

## Tài liệu tham khảo

1.  [桃酱的算法笔记](https://zhuanlan.zhihu.com/p/41867199).
