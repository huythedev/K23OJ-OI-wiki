Phần này chỉ xét ma trận vuông: biến đổi tuyến tính đưa $n$ vectơ thành $n$ vectơ.

Trong thực tế, biến đổi lặp nhiều lần. Nếu chỉ nói “ma trận $A$ đưa $I$ thành $A$” thì quá trừu tượng. Tốt nhất là tìm “điểm bất động”. Nhưng thường không có, nên tìm phần “đồng tuyến” hay “biến dạng đơn giản”.

## Trị riêng và vectơ riêng

Trong biến đổi của $A$, một số vectơ giữ hướng, chỉ bị co giãn.

Cho $V$ trên trường $F$, $T$ là biến đổi. Nếu tồn tại $\lambda\in F$ và **vectơ không-zero** $\xi$ sao cho:

$$
T\xi=\lambda\xi
$$

thì $\lambda$ là **trị riêng**, $\xi$ là **vectơ riêng**.

Vectơ riêng trên cùng đường thẳng, hướng không đổi (kể cả bị co về 0). Vectơ riêng không duy nhất; mọi vectơ cùng phương đều là vectơ riêng, nhưng quy ước vectơ 0 không là vectơ riêng.

Trong thực tế, với cùng trị riêng, chọn một cơ sở đại diện.

Giả sử $\alpha_1,\dots,\alpha_n$ là cơ sở, $T$ có ma trận $A$:

$$
T(\alpha_1,\cdots,\alpha_n)=(\alpha_1,\cdots,\alpha_n)A
$$

Cho trị riêng $\lambda_0$ và vectơ riêng $\xi$, với $\xi=(\alpha_1,\cdots,\alpha_n)X$:

$$
AX=\lambda_0X
$$

$$
(A-\lambda_0I)X=0
$$

Suy ra định thức bằng 0.

## Đa thức đặc trưng

Với ma trận $A$ bậc $n$, ma trận $\lambda I-A$ gọi là **ma trận đặc trưng**.

Định thức của nó là **đa thức đặc trưng**, ký hiệu $p_A(\lambda)$:

$$
p_A(\lambda)=\det(\lambda I_n-A)=\begin{vmatrix}
\lambda-a_{11} & -a_{12} &  \cdots & -a_{1n} \\
-a_{21} & \lambda-a_{22} &  \cdots & -a_{2n} \\
\vdots & \vdots &  & \vdots  \\
-a_{n1} & -a_{n2} &  \cdots & \lambda-a_{nn} \\
\end{vmatrix}
$$

Một số nơi định nghĩa $\det(A-\lambda I_n)$, khác nhau hệ số $(-1)^n$. Lưu ý định thức ma trận $0\times 0$ là 1.

Nghiệm của $p_A$ là trị riêng. Vectơ riêng là nghiệm không-zero của $(\lambda_0I-A)X=0$.

Trị riêng/vectơ riêng của $T$ tương ứng với của $A$ qua cơ sở.

Theo định lý cơ bản đại số:

$$
f(\lambda)=|\lambda I-A|={(\lambda-\lambda_1)}^{d_1}\cdots{(\lambda-\lambda_m)}^{d_m}
$$

$d_i$ là **bội đại số** của $\lambda_i$. Tổng các bội bằng $n$.

### Tìm trị riêng và vectơ riêng

-   Tính $|\lambda I-A|$.
-   Tìm nghiệm của $f(\lambda)$ trong trường $F$.
-   Với mỗi trị riêng $\lambda$, giải $(\lambda I-A)X=0$ để lấy một hệ nghiệm cơ bản $X_1,\dots,X_t$. Khi đó mọi vectơ riêng thuộc $\lambda$ là:

$$
k_1X_1+\cdots+k_tX_t
$$

với không phải tất cả $k_i$ bằng 0.

-   Vectơ riêng của $T$ là:

$$
\xi_i=(\alpha_1,\cdots,\alpha_n)X_i
$$

Nên mọi vectơ riêng là tổ hợp tuyến tính của $\xi_i$.

Trị riêng/vectơ riêng phụ thuộc vào trường.

## Biến đổi tương tự

### Giới thiệu

Nếu $A$ là tam giác trên:

$$
A=
\begin{bmatrix}
a_{1,1}&a_{1,2}&\cdots &a_{1,n}\\
&a_{2,2}&\cdots &a_{2,n}\\
&&\ddots &\vdots \\
&&&a_{n,n}
\end{bmatrix}
$$

thì

$$
\begin{aligned}
p_A(x)&=\det(xI_n-A)\\
&=\prod_{i=1}^n(x-a_{i,i})
\end{aligned}
$$

Dễ tính; tam giác dưới tương tự. Nếu $A$ không thuộc hai loại này, dùng biến đổi tương tự để đưa về dạng dễ tính.

### Định nghĩa

Với ma trận $A,B$ bậc $n$, nếu tồn tại $P$ khả nghịch sao cho:

$$
B=P^{-1}AP
$$

thì $A$ và $B$ **tương tự**, và $A\mapsto P^{-1}AP$ gọi là biến đổi tương tự. Chúng có cùng đa thức đặc trưng.

Chứng minh:

$$
\begin{aligned}
\det(xI_n-P^{-1}AP)&=\det(P^{-1})\cdot \det(P)\cdot \det(xI_n-A)\\
&=\det(xI_n-A)
\end{aligned}
$$

Tương tự cho $A\mapsto PAP^{-1}$. Hơn nữa $p_A(0)=(-1)^n\det(A)$ nên $\det(A)=\det(P^{-1}AP)$.

Định lý: ma trận tương tự có cùng đa thức đặc trưng và trị riêng; ngược lại không đúng.

Do đó đa thức đặc trưng không phụ thuộc vào cơ sở.

Với $p_A(\lambda)$ là đa thức đơn thức hệ số 1, theo Viète, hệ số bậc $n-1$ là:

$$
-(\lambda_1+\cdots+\lambda_n)=-(a_{11}+\cdots+a_{nn})=-tr A
$$

trong đó $tr A$ là vết.

Hệ số tự do:

$$
{(-1)}^n|A|={(-1)}^n(\lambda_1\cdots\lambda_n)
$$

Định lý: ma trận tương tự có cùng vết.

### Công thức hoán vị

Định lý: với mọi ma trận $A,B$ (không cần vuông), nếu tích xác định thì $tr(AB)=tr(BA)$.

Một chứng minh khác dùng công thức:

Định lý: với $A$ $m\times n$, $B$ $n\times m$:

$$
\lambda^n|\lambda I_m-AB|=\lambda^m|\lambda I_n-BA|
$$

Suy ra $AB$ và $BA$ có cùng trị riêng khác 0.

### Định lý Schur

Mọi ma trận bậc $n$ tương tự một tam giác trên; đường chéo chính là các trị riêng.

Hệ quả: với đa thức $\phi(x)$, trị riêng của $\phi(A)$ là $\phi(\lambda_i)$. Đặc biệt, trị riêng của $kA$ là $k\lambda_i$, của $A^m$ là $\lambda_i^m$.

### Dùng khử Gauss để biến đổi tương tự

Khử Gauss dùng biến đổi hàng. Nếu sau khi nhân trái bởi ma trận sơ cấp lại nhân phải bởi nghịch đảo của nó thì là biến đổi tương tự; nhân phải tương ứng biến đổi cột.

Nếu đưa được về tam giác trên/dưới thì dễ tính đa thức đặc trưng. Nhưng biến đổi $A\mapsto T_{ij}(k)AT_{ij}(-k)$ có thể làm các phần tử đã khử trở lại khác 0, nên khó đạt tam giác.

Dưới đây sẽ xét ma trận Hessenberg.

### Ma trận Hessenberg trên

Với $n>2$, ma trận:

$$
H=
\begin{bmatrix}
\alpha_{1}&h_{12}&\dots&\dots&h_{1n}\\
\beta_{2}&\alpha_{2}&h_{23}&\dots &\vdots \\
&\ddots &\ddots & \ddots &\vdots \\
& &\ddots &\ddots & h_{(n-1)n}\\
&&& \beta_{n}& \alpha_{n}
\end{bmatrix}
$$

gọi là Hessenberg trên, trong đó $\beta$ là đường chéo phụ.

Dùng biến đổi tương tự để khử các phần tử dưới đường chéo phụ, ta được Hessenberg. Đa thức đặc trưng của Hessenberg có thể tính trong $O(n^3)$.

Gọi $H_i$ là ma trận con $i\times i$ đầu, $p_i(x)=\det(xI_i-H_i)$. Khi đó:

$$
H_0=\begin{bmatrix}\end{bmatrix},\quad p_0(x)=1
$$

$$
H_1=\begin{bmatrix}\alpha_1\end{bmatrix},\quad p_1(x)=x-\alpha_1
$$

$$
H_2=
\begin{bmatrix}
\alpha_1&h_{12}\\
\beta_2&\alpha_2
\end{bmatrix},\quad
p_2(x)=(x-\alpha_2)p_1(x)-\beta_2h_{12}p_0(x)
$$

Khai triển theo hàng cuối:

$$
\begin{aligned}
p_3(x)&=
\det(xI_3-H_3)\\
&=(x-\alpha_3)p_2(x)-\beta_3(h_{23}p_1(x)+\beta_2h_{13}p_0(x))
\end{aligned}
$$

Suy ra với $2\le i\le n$:

$$
p_i(x)=(x-\alpha_i)p_{i-1}(x)-
\sum_{m=1}^{i-1}h_{i-m,i}
\left(
\prod_{j=i-m+1}^{i}\beta_j
\right)
p_{i-m-1}(x)
$$

Đây là thuật toán Hessenberg.

## Định lý Cayley–Hamilton

Với ma trận $A$ bậc $n$ và đa thức đặc trưng $f(\lambda)$, ta có $f(A)=0$.

Tương tự với biến đổi tuyến tính $T$.

Do đó mọi ma trận đều có đa thức triệt tiêu.

## Đa thức tối tiểu

Trong không gian $n$ chiều, mọi biến đổi tuyến tính tạo thành không gian $n^2$ chiều. Với $T$, các biến đổi $T^0,\dots,T^{n^2}$ tuyến tính phụ thuộc, nên tồn tại đa thức không-zero $f$ sao cho $f(T)=0$. Chọn đa thức bậc nhỏ nhất gọi là **đa thức tối tiểu** $m_A(\lambda)$.

Theo thuật toán Euclid, đa thức tối tiểu là duy nhất và chia mọi đa thức triệt tiêu. Đặc biệt, $m_A$ chia $p_A$.

Định lý: bỏ bội, $p_A$ và $m_A$ có cùng nghiệm.

Định lý: vectơ riêng thuộc trị riêng khác nhau là độc lập tuyến tính.

## Ứng dụng

Trong tin học, thường xét ma trận trên $(\mathbb{Z}/m\mathbb{Z})^{n\times n}$, với $m$ thường là số nguyên tố. Khi $m$ hợp số, có thể dùng phép chia Euclid tương tự.

??? note "实现"
    ```cpp
    #include <cassert>
    #include <iostream>
    #include <random>
    #include <vector>
    
    using Matrix = std::vector<std::vector<int>>;
    using i64 = int64_t;
    
    Matrix to_upper_Hessenberg(const Matrix &M, int mod) {
      Matrix H(M);
      int n = H.size();
      for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
          if ((H[i][j] %= mod) < 0) H[i][j] += mod;
        }
      }
      for (int i = 0; i < n - 1; ++i) {
        int pivot = i + 1;
        for (; pivot < n; ++pivot) {
          if (H[pivot][i] != 0) break;
        }
        if (pivot == n) continue;
        if (pivot != i + 1) {
          for (int j = i; j < n; ++j) std::swap(H[i + 1][j], H[pivot][j]);
          for (int j = 0; j < n; ++j) std::swap(H[j][i + 1], H[j][pivot]);
        }
        for (int j = i + 2; j < n; ++j) {
          for (;;) {
            if (H[j][i] == 0) break;
            if (H[i + 1][i] == 0) {
              for (int k = i; k < n; ++k) std::swap(H[i + 1][k], H[j][k]);
              for (int k = 0; k < n; ++k) std::swap(H[k][i + 1], H[k][j]);
              break;
            }
            if (H[j][i] >= H[i + 1][i]) {
              int q = H[j][i] / H[i + 1][i], mq = mod - q;
              for (int k = i; k < n; ++k)
                H[j][k] = (H[j][k] + i64(mq) * H[i + 1][k]) % mod;
              for (int k = 0; k < n; ++k)
                H[k][i + 1] = (H[k][i + 1] + i64(q) * H[k][j]) % mod;
            } else {
              int q = H[i + 1][i] / H[j][i], mq = mod - q;
              for (int k = i; k < n; ++k)
                H[i + 1][k] = (H[i + 1][k] + i64(mq) * H[j][k]) % mod;
              for (int k = 0; k < n; ++k)
                H[k][j] = (H[k][j] + i64(q) * H[k][i + 1]) % mod;
            }
          }
        }
      }
      return H;
    }
    
    std::vector<int> get_charpoly(const Matrix &M, int mod) {
      Matrix H(to_upper_Hessenberg(M, mod));
      int n = H.size();
      std::vector<std::vector<int>> p(n + 1);
      p[0] = {1 % mod};
      for (int i = 1; i <= n; ++i) {
        const std::vector<int> &pi_1 = p[i - 1];
        std::vector<int> &pi = p[i];
        pi.resize(i + 1, 0);
        int v = mod - H[i - 1][i - 1];
        if (v == mod) v -= mod;
        for (int j = 0; j < i; ++j) {
          pi[j] = (pi[j] + i64(v) * pi_1[j]) % mod;
          if ((pi[j + 1] += pi_1[j]) >= mod) pi[j + 1] -= mod;
        }
        int t = 1;
        for (int j = 1; j < i; ++j) {
          t = i64(t) * H[i - j][i - j - 1] % mod;
          int prod = i64(t) * H[i - j - 1][i - 1] % mod;
          if (prod == 0) continue;
          prod = mod - prod;
          for (int k = 0; k <= i - j - 1; ++k)
            pi[k] = (pi[k] + i64(prod) * p[i - j - 1][k]) % mod;
        }
      }
      return p[n];
    }
    
    bool verify(const Matrix &M, const std::vector<int> &charpoly, int mod) {
      if (mod == 1) return true;
      int n = M.size();
      std::vector<int> randvec(n), sum(n, 0);
      std::mt19937 gen(std::random_device{}());
      std::uniform_int_distribution<int> dis(1, mod - 1);
      for (int i = 0; i < n; ++i) randvec[i] = dis(gen);
      for (int i = 0; i <= n; ++i) {
        int v = charpoly[i];
        for (int j = 0; j < n; ++j) sum[j] = (sum[j] + i64(v) * randvec[j]) % mod;
        std::vector<int> prod(n, 0);
        for (int j = 0; j < n; ++j) {
          for (int k = 0; k < n; ++k) {
            prod[j] = (prod[j] + i64(M[j][k]) * randvec[k]) % mod;
          }
        }
        randvec.swap(prod);
      }
      for (int i = 0; i < n; ++i)
        if (sum[i] != 0) return false;
      return true;
    }
    
    int main() {
      std::ios::sync_with_stdio(false);
      std::cin.tie(nullptr);
      int n, mod;
      std::cin >> n >> mod;
      Matrix M(n, std::vector<int>(n));
      for (int i = 0; i < n; ++i)
        for (int j = 0; j < n; ++j) std::cin >> M[i][j];
      std::vector<int> charpoly(get_charpoly(M, mod));
      for (int i = 0; i <= n; ++i) std::cout << charpoly[i] << ' ';
      assert(verify(M, charpoly, mod));
      return 0;
    }
    ```

Thuật toán Hessenberg không ổn định số, nên với $\mathbb{R}^{n\times n}$ cần thuật toán ổn định hơn.

Có thể liên hệ đa thức đặc trưng với truy hồi tuyến tính, hoặc dùng Cayley–Hamilton, lấy modulo đa thức để tăng tốc lũy thừa ma trận.

Theo Cayley–Hamilton:

$$
\begin{aligned}
p_A(A)&=A^n+c_1A^{n-1}+\cdots +c_{n-1}A+c_nI\\
&=O
\end{aligned}
$$

với $O$ là ma trận 0, và $p_A(x)=x^n+\sum_{i=1}^nc_ix^{n-i}$.

Nếu cần $A^K$ lớn, tính $f(x)=x^K\bmod{p_A(x)}$ rồi dùng $f(A)=A^K$.

Vì $\deg f < n$, viết $f(x)=\sum_{i=0}^{n-1}f_ix^i$ và $n=km$:

$$
\begin{aligned}
f_{km-1}x^{km-1}+\cdots +f_1x+f_0&=(\cdots (f_{km-1}x^{k-1}+\cdots +f_{k(m-1)})x^k\\
&+f_{k(m-1)-1}x^{k-1}+\cdots +f_{k(m-2)})x^k\\
&+\cdots\\
&+f_{k-1}x^{k-1}+\cdots +f_1x+f_0
\end{aligned}
$$

Chọn $k=\sqrt{n}$, tính $f(A)$ cần khoảng $O(\sqrt{n})$ phép nhân ma trận.

## Tài liệu tham khảo

-   Rizwana Rehman, Ilse C.F. Ipsen.[La Budde’s Method for Computing Characteristic Polynomials](https://ipsen.math.ncsu.edu/ps/charpoly3.pdf).
-   Marshall Law.[Computing Characteristic Polynomials of Matrices of Structured Polynomials](http://summit.sfu.ca/system/files/iritems1/17301/etd10125_.pdf).
-   Mike Paterson.[On the Number of Nonscalar Multiplications Necessary to Evaluate Polynomials](https://epubs.siam.org/doi/10.1137/0202007).
````
