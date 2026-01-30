author: pw384, s0cks5, Watersail2005, Xeonacid

Định lý cây ma trận (Matrix-Tree Theorem) giải quyết bài toán đếm số lượng cây khung của một đồ thị.

## Ký hiệu sử dụng trong bài

Trong bài này, đồ thị (dù vô hướng hay có hướng) đều cho phép đa cạnh, nhưng mặc định không có khuyên (self-loop).

??? note "Trường hợp có khuyên"
    Khuyên không ảnh hưởng đến số lượng cây khung, cũng không ảnh hưởng đến việc tính ma trận Laplace ở phần sau, do đó định lý cây ma trận vẫn đúng cho trường hợp có khuyên. Khi tính toán không cần xóa khuyên. Nếu xóa khuyên, sẽ ảnh hưởng đến việc áp dụng định lý BEST để đếm số chu trình Euler trên đồ thị có hướng.

### Trường hợp đồ thị vô hướng

Gọi $G$ là đồ thị vô hướng có $n$ đỉnh. Định nghĩa ma trận bậc (degree matrix) $D(G)$:

$$
D_{ii}(G) = \mathrm{deg}(i),\ D_{ij} = 0,\ i\neq j.
$$

Gọi $\#e(i,j)$ là số cạnh nối giữa $i$ và $j$, định nghĩa ma trận kề $A$:

$$
A_{ij}(G)=A_{ji}(G)=\#e(i,j),\ i\neq j.
$$

Định nghĩa ma trận Laplace (còn gọi là ma trận Kirchhoff) $L$:

$$
L(G) = D(G) - A(G).
$$

Ký hiệu $t(G)$ là số lượng cây khung của đồ thị $G$.

### Trường hợp đồ thị có hướng

Gọi $G$ là đồ thị có hướng với $n$ đỉnh. Định nghĩa ma trận bậc ra $D^{out}(G)$:

$$
D^\mathrm{out}_{ii}(G) = \mathrm{deg}^\mathrm{out}(i),\ D^\mathrm{out}_{ij} = 0,\ i\neq j.
$$

Tương tự, định nghĩa ma trận bậc vào $D^\mathrm{in}(G)$.

Gọi $\#e(i,j)$ là số cạnh có hướng từ $i$ đến $j$, định nghĩa ma trận kề $A$:

$$
A_{ij}(G)=\#e(i,j),\ i\neq j.
$$

Định nghĩa ma trận Laplace bậc ra $L^\mathrm{out}$:

$$
L^\mathrm{out}(G) = D^\mathrm{out}(G) - A(G).
$$

Định nghĩa ma trận Laplace bậc vào $L^\mathrm{in}$:

$$
L^\mathrm{in}(G) = D^\mathrm{in}(G) - A(G).
$$

Ký hiệu $t^\mathrm{root}(G,k)$ là số lượng cây gốc (arborescence) hướng về gốc $k$ của $G$. Cây gốc là đồ thị con là cây, mọi cạnh đều hướng về cha.

Ký hiệu $t^\mathrm{leaf}(G,k)$ là số lượng cây hướng về lá $k$ (mọi cạnh đều hướng về con).

## Phát biểu định lý

Định lý cây ma trận có nhiều dạng.

Định nghĩa $[n]=\{1,2,\cdots,n\}$, với ma trận $A$, ký hiệu $A_{S,T}$ là ma trận con lấy các phần tử $A_{i,j}$ với $i\in S, j\in T$.

???+ note "Định lý 1 (Matrix-Tree Theorem, đồ thị vô hướng, dạng định thức)"
    Với đồ thị vô hướng $G$ và mọi $k$,
    
    $$
    t(G) = \det L(G)_{[n]\setminus\{k\},[n]\setminus\{k\}}.
    $$
    
    Tức là, mọi định thức con bậc $n-1$ của ma trận Laplace đều bằng số cây khung của đồ thị.

???+ note "Hệ quả 1 (Matrix-Tree Theorem, đồ thị vô hướng, dạng giá trị riêng)"
    Gọi $\lambda_1\ge\lambda_2\ge\cdots\ge\lambda_{n-1}\ge\lambda_n=0$ là các giá trị riêng của $L(G)$, khi đó
    
    $$
    t(G) = \frac{1}{n}\lambda_1\lambda_2\cdots\lambda_{n-1}.
    $$

???+ note "Định lý 2 (Matrix-Tree Theorem, đồ thị có hướng, cây gốc, dạng định thức)"
    Với đồ thị có hướng $G$ và mọi $k$,
    
    $$
    t^\mathrm{root}(G,k) = \det L^\mathrm{out}(G)_{[n]\setminus\{k\},[n]\setminus\{k\}}.
    $$
    
    Tức là, định thức con bậc $n-1$ của ma trận Laplace bậc ra (xóa dòng và cột $k$) bằng số cây gốc hướng về $k$.

Vậy nếu muốn đếm tổng số cây gốc hướng về mọi gốc, chỉ cần tính tổng $t^\mathrm{root}(G,k)$ với mọi $k$.

???+ note "Định lý 3 (Matrix-Tree Theorem, đồ thị có hướng, cây lá, dạng định thức)"
    Với đồ thị có hướng $G$ và mọi $k$,
    
    $$
    t^\mathrm{leaf}(G,k) = \det L^\mathrm{in}(G)_{[n]\setminus\{k\},[n]\setminus\{k\}}.
    $$
    
    Tức là, định thức con bậc $n-1$ của ma trận Laplace bậc vào (xóa dòng và cột $k$) bằng số cây hướng về lá $k$.

Vậy nếu muốn đếm tổng số cây lá hướng về mọi lá, chỉ cần tính tổng $t^\mathrm{leaf}(G,k)$ với mọi $k$.

???+ note "Chú thích"
    Cây gốc còn gọi là cây hướng vào (in-arborescence), nhưng vì tính bằng Laplace bậc ra nên dùng thuật ngữ "cây gốc" để tránh nhầm lẫn giữa $\mathrm{in}$ và $\mathrm{out}$.

## Chứng minh định lý

Các định lý trên có dạng rất tương tự, dưới đây là cách chứng minh thống nhất, đồng thời mở rộng cho đồ thị có trọng số.

Ý tưởng chính:

-   Mọi trường hợp đều quy về đếm số cây gốc trên đồ thị có hướng.
-   Dùng ngôn ngữ ma trận để mô tả điều kiện chọn cạnh tạo thành cây gốc.
-   Liên hệ thao tác chọn cạnh với định lý Cauchy–Binet và định thức ma trận Laplace.
-   Cuối cùng, chuyển từ dạng định thức sang dạng giá trị riêng.

### Bổ đề: Công thức Cauchy–Binet

???+ note "Bổ đề 1 (Cauchy–Binet)"
    Cho ma trận $A$ kích thước $n\times m$ và $B$ kích thước $m\times n$, ta có
    
    $$
    \det(AB)=\sum_{S\subset[m];~|S|=n}\det A_{[n],S}\det B_{S,[n]},
    $$
    
    Tổng trên mọi tập con $S$ của $[m]$ có kích thước $n$. Nếu $n>m$, thì $\det(AB)=0$.

??? note "Chứng minh (góc nhìn tổ hợp)"
    ...existing code...

??? note "Chứng minh (góc nhìn đại số)"
    ...existing code...

### Biểu diễn cấu trúc đồ thị bằng ma trận liên thuộc

Với đồ thị có hướng $G=(V,E)$, $n$ đỉnh, $m$ cạnh, mỗi cạnh $e$ có trọng số $w(e)$. Định nghĩa ma trận liên thuộc bậc ra $M^\mathrm{out}$ kích thước $m\times n$:

$$
M^\mathrm{out}_{ij}=\begin{cases}
\sqrt{w(e_i)},&\exists u(e_i=(v_j,u)),\\
0,&\textrm{otherwise},
\end{cases}
$$

và ma trận liên thuộc bậc vào $M^\mathrm{in}$ kích thước $m\times n$:

$$
M^\mathrm{in}_{ij}=\begin{cases}
\sqrt{w(e_i)},&\exists u(e_i=(u,v_j)),\\
0,&\textrm{otherwise}.
\end{cases}
$$

Mỗi dòng ứng với một cạnh: $M^\mathrm{out}$ lưu đỉnh đầu, $M^\mathrm{in}$ lưu đỉnh cuối.

Dễ thấy:

$$
D^\mathrm{out}(G) = (M^\mathrm{out})^T M^\mathrm{out},\ A(G) = (M^\mathrm{out})^T M^\mathrm{in},\ D^\mathrm{in}(G) = (M^\mathrm{in})^T M^\mathrm{in}.
$$

Suy ra:

$$
L^\mathrm{out}(G) = (M^\mathrm{out})^T (M^\mathrm{out}-M^\mathrm{in}),\ L^\mathrm{in}(G) = (M^\mathrm{in}-M^\mathrm{out})^T M^\mathrm{in}.
$$

Công thức Cauchy–Binet cho thấy: các định thức con của ma trận Laplace là tổng trên các cấu trúc con của đồ thị, mỗi cấu trúc phản ánh tính chất của đồ thị con tương ứng.

???+ note "Bổ đề 2"
    Với đồ thị con $(W,S)$ của $G$, nếu $|W|=|S|\le n$, thì $T=(V,S)$ là rừng gốc với gốc là $V\setminus W$ khi và chỉ khi
    
    $$
    \det(M^\mathrm{out}_{S,W})\det(M^\mathrm{out}_{S,W}-M^\mathrm{in}_{S,W})
    $$
    
    khác $0$. Khi khác $0$, giá trị bằng $\prod_{e\in S}w(e)$, ký hiệu $w(T)$.

??? note "Chứng minh"
    ...existing code...

### Định lý cây ma trận cho đồ thị có hướng có trọng số

Bây giờ chứng minh kết quả chính. Các định lý cây ma trận trước là trường hợp riêng của định lý này.

???+ note "Định lý 4 (Matrix-Tree Theorem, đồ thị có hướng có trọng số, cây gốc, dạng định thức)"
    Với mọi $k$,
    
    $$
    \sum_{T\in\mathcal T^\mathrm{root}(G,k)}w(T)=\det L^\mathrm{out}(G)_{[n]\setminus\{k\},[n]\setminus\{k\}}.
    $$
    
    Ở đây, $\mathcal T^\mathrm{root}(G,k)$ là tập các cây gốc hướng về $k$ của $G$.

??? note "Chứng minh"
    ...existing code...

Khi $w(e)=1$, mỗi cây có trọng số $1$, vế trái là số cây, chính là $t^\mathrm{root}(G,k)$, ra định lý 2. Tương tự, có thể mở rộng cho cây lá (định lý 3). Để đếm cây khung vô hướng, dùng hệ quả sau.

???+ note "Hệ quả 4 (Matrix-Tree Theorem, đồ thị vô hướng có trọng số, dạng định thức)"
    Với đồ thị vô hướng $G$ và mọi $k$,
    
    $$
    \sum_{T\in\mathcal T(G)}w(T) = \det L(G)_{[n]\setminus\{k\},[n]\setminus\{k\}}.
    $$
    
    Ở đây, $\mathcal T(G)$ là tập các cây khung của $G$. Tức là mọi định thức con bậc $n-1$ của $L(G)$ đều bằng nhau.

??? note "Chứng minh"
    ...existing code...

### Dạng giá trị riêng

Xét trước cho đồ thị có hướng.

???+ note "Định lý 5"
    Với đồ thị có hướng $G$, định nghĩa đa thức nhiều biến
    
    $$
    \chi(x_1,\cdots,x_n)=\det(\mathrm{diag}(x_1,\cdots,x_n)-L^\mathrm{out}(G)).
    $$
    
    Ở đây, $\mathrm{diag}(x_1,\cdots,x_n)$ là ma trận chéo với $x_1,\cdots,x_n$ trên đường chéo. Khi đó,
    
    $$
    (-1)^{n-r}[x_{k_1},\cdots,x_{k_r}]\chi(x_1,\cdots,x_n)
    $$
    
    bằng tổng trọng số các rừng gốc với gốc là $\{k_1,\cdots,k_r\}$.

??? note "Chứng minh"
    ...existing code...

Thay $x$ cho mọi biến, ta có đa thức đặc trưng của Laplace:

$$
P(x) = \det(xI-L^\mathrm{out}(G)) = \chi(x,\cdots,x).
$$

???+ note "Bổ đề 3"
    Ma trận Laplace $L^\mathrm{out}(G)$ luôn có ít nhất một giá trị riêng bằng $0$.

??? note "Chứng minh"
    ...existing code...

???+ note "Hệ quả 5"
    Với đồ thị có hướng $G$, tổng trọng số các rừng gốc gồm $k$ cây bằng hệ số
    
    $$
    (-1)^{n-k}[x^k]P(x).
    $$

??? note "Chứng minh"
    ...existing code...

Định nghĩa $k$-rừng khung là đồ thị con có $k$ thành phần liên thông và không có chu trình.

???+ note "Hệ quả 6"
    Gọi $\mathcal T_k(G)$ là tập các $k$-rừng khung của đồ thị vô hướng $G$, khi đó
    
    $$
    \sum_{T\in\mathcal T_k(G)}w(T)Q(T) = (-1)^{n-k}[x^k]P(x).
    $$
    
    $Q(T)$ là tích số đỉnh của mỗi thành phần liên thông trong rừng $T$. Đặc biệt, khi $k=1$, $Q(T)=n$, nên
    
    $$
    n\sum_{T\in\mathcal T(G)}w(T) = \lambda_1\lambda_2\cdots\lambda_{n-1}.
    $$

??? note "Chứng minh"
    ...existing code...

## Ứng dụng

### Công thức Cayley

???+ note "Hệ quả 7 (Cayley)"
    Có $n^{n-2}$ cây khung vô hướng có gán nhãn với $n$ đỉnh.

??? note "Chứng minh"
    ...existing code...

### Định lý BEST

Kiến thức nền: [Đồ thị Euler](./euler.md)

Định lý này liên hệ số chu trình Euler trên đồ thị có hướng với số cây gốc, từ đó giải quyết bài toán đếm chu trình Euler trên đồ thị có hướng. Lưu ý: đếm chu trình Euler trên đồ thị vô hướng là bài toán NP-complete.

Khi cài đặt, cần kiểm tra đồ thị có phải Euler không, loại bỏ các đỉnh bậc 0, sau đó xây dựng đồ thị và tính số cây gốc, rồi áp dụng định lý BEST để ra số chu trình Euler. Nếu yêu cầu chu trình bắt đầu từ một đỉnh cho trước, nhân thêm bậc ra của đỉnh đó (tức là chọn cạnh đầu tiên của chu trình).

Trước khi chứng minh định lý BEST, cần biết kết quả sau.

???+ note "Tính chất (Điều kiện có chu trình Euler trên đồ thị có hướng)"
    Đồ thị có hướng có chu trình Euler khi và chỉ khi các đỉnh bậc dương tạo thành thành phần liên thông mạnh, và mọi đỉnh có bậc ra bằng bậc vào.

Với đồ thị Euler, vì bậc ra = bậc vào, có thể bỏ chỉ số trên, ký hiệu $\mathrm{deg}(v)$. Định lý BEST phát biểu như sau.

???+ note "Định lý 6 (BEST)"
    Cho $G$ là đồ thị Euler có hướng, $k$ là đỉnh bất kỳ, số chu trình Euler khác nhau của $G$ là
    
    $$
    \mathrm{ec}(G) = t^\mathrm{root}(G,k)\prod_{v\in V}(\deg (v) - 1)!.
    $$
    
    Điều này cũng cho thấy với mọi $k, k'$, $t^\mathrm{root}(G,k)=t^\mathrm{root}(G,k')$.

??? note "Chứng minh"
    ...existing code...

## Cài đặt

Viết ma trận Laplace, xóa một dòng một cột, tính định thức. Có thể dùng Gauss–Jordan để tính định thức.

Ví dụ, số cây khung của đồ thị hình vuông:

$$
\begin{pmatrix}
2 & 0 & 0 & 0 \\
0 & 2 & 0 & 0 \\
0 & 0 & 2 & 0 \\
0 & 0 & 0 & 2 \end{pmatrix}-\begin{pmatrix}
0 & 1 & 0 & 1 \\
1 & 0 & 1 & 0 \\
0 & 1 & 0 & 1 \\
1 & 0 & 1 & 0 \end{pmatrix}=\begin{pmatrix}
2 & -1 & 0 & -1 \\
-1 & 2 & -1 & 0 \\
0 & -1 & 2 & -1 \\
-1 & 0 & -1 & 2 \end{pmatrix}
$$

$$
\begin{vmatrix}
2 & -1 & 0 \\
-1 & 2 & -1 \\
0 & -1 & 2 \end{vmatrix} = 4
$$

Có thể dùng Gauss–Jordan, độ phức tạp $O(n^3)$.

??? note "Cài đặt"
    ```cpp
    #include <algorithm>
    #include <cassert>
    #include <cmath>
    #include <cstdio>
    #include <cstring>
    #include <iostream>
    using namespace std;
    constexpr int MOD = 100000007;
    constexpr double eps = 1e-7;
    
    struct matrix {
      static constexpr int MAXN = 20;
      int n, m;
      double mat[MAXN][MAXN];
    
      matrix() { memset(mat, 0, sizeof(mat)); }
    
      void print() {
        cout << "MATRIX " << n << " " << m << endl;
        for (int i = 0; i < n; i++) {
          for (int j = 0; j < m; j++) {
            cout << mat[i][j] << "\t";
          }
          cout << endl;
        }
      }
    
      void random(int n) {
        this->n = n;
        this->m = n;
        for (int i = 0; i < n; i++)
          for (int j = 0; j < n; j++) mat[i][j] = rand() % 100;
      }
    
      void initSquare() {
        this->n = 4;
        this->m = 4;
        memset(mat, 0, sizeof(mat));
        mat[0][1] = mat[0][3] = 1;
        mat[1][0] = mat[1][2] = 1;
        mat[2][1] = mat[2][3] = 1;
        mat[3][0] = mat[3][2] = 1;
        mat[0][0] = mat[1][1] = mat[2][2] = mat[3][3] = -2;
        this->n--;  // 去一行
        this->m--;  // 去一列
      }
    
      double gauss() {
        double ans = 1;
        for (int i = 0; i < n; i++) {
          int sid = -1;
          for (int j = i; j < n; j++)
            if (abs(mat[j][i]) > eps) {
              sid = j;
              break;
            }
          if (sid == -1) continue;
          if (sid != i) {
            for (int j = 0; j < n; j++) {
              swap(mat[sid][j], mat[i][j]);
              ans = -ans;
            }
          }
          for (int j = i + 1; j < n; j++) {
            double ratio = mat[j][i] / mat[i][i];
            for (int k = 0; k < n; k++) {
              mat[j][k] -= mat[i][k] * ratio;
            }
          }
        }
        for (int i = 0; i < n; i++) ans *= mat[i][i];
        return abs(ans);
      }
    };
    
    int main() {
      srand(1);
      matrix T;
      // T.random(2);
      T.initSquare();
      T.print();
      double ans = T.gauss();
      T.print();
      cout << ans << endl;
    }
    ```

## Bài tập ví dụ

???+ note "Bài 1: [HEOI2015] Phòng của Z nhỏ ([LOJ2122](https://loj.ac/problem/2122))"
    **Giải** Bài toán áp dụng trực tiếp định lý cây ma trận. Xem mỗi phòng trống là một đỉnh, xây dựng đồ thị theo đề, lập ma trận Laplace, xóa một dòng một cột rồi tính định thức. Tính định thức bằng Gauss–Jordan trên trường $\mathbb{Z}_k$ (modulo), dùng thuật toán Euclid mở rộng.

???+ note "Bài 2: [FJOI2007] Virus hình bánh xe ([Luogu P2144](https://www.luogu.com.cn/problem/P2144))"
    **Giải** Có nhiều cách giải, dùng định lý cây ma trận là trực tiếp nhất. Với $n$ đầu vào, lập ma trận Laplace bậc $n+1$:
    
    $$
    L_n = \begin{bmatrix}
    n&  -1&  -1&  -1&  \cdots&  -1&  -1\\
    -1&  3&  -1&  0&  \cdots&  0&  -1\\
    -1&  -1&  3&  -1&  \cdots&  0&  0\\
    -1&  0&  -1&  3&  \cdots&  0&  0\\
    \vdots&  \vdots&  \vdots&  \vdots&  \ddots&  \vdots&  \vdots\\
    -1&  0&  0&  0&  \cdots&  3&  -1\\
    -1&  -1&  0&  0&  \cdots&  -1&  3\\
    \end{bmatrix}_{n+1}
    $$
    
    Tính định thức con bậc $n$ là xong, còn lại là tính toán số lớn.

??? note "Bài 2+"
    Nếu tăng $n\leq 100000$, đáp án lấy modulo $1000007$. (Cần kiến thức đại số tuyến tính)

    **Giải** Suy ra công thức truy hồi rồi dùng lũy thừa ma trận.

    ...existing code...

???+ note "Bài 3: [BZOJ3659] WHICH DREAMED IT ([Hydro](https://hydro.ac/p/bzoj-P3659))"
    **Giải** Áp dụng trực tiếp định lý BEST, chú ý: mỗi chu trình Euler, phòng số 1 có thể bắt đầu từ bất kỳ cạnh ra nào, nên đáp án nhân thêm bậc ra của phòng 1.

???+ note "Bài 4: [Tỉnh liên hợp 2020 A] Bài tập ([LOJ3304](https://loj.ac/p/3304))"
    **Giải** Dùng Möbius để chuyển về đếm tổng trọng số cây khung, chi tiết bỏ qua. Viết các hạng tử định thức thành $w_ix+1$, đáp án là hệ số bậc nhất, vì thực chất là số cây khung có chứa cạnh $i$ nhân trọng số cạnh $i$. Bỏ qua các bậc cao hơn, độ phức tạp $O(n^3)$. [Bài tương tự: Đếm tổng lũy thừa $k$ trọng số cây khung](https://www.luogu.com.cn/problem/P5296).

???+ note "Bài 5: [AGC051D C4](https://atcoder.jp/contests/agc051/tasks/agc051_d)"
    **Giải** Đếm chu trình Euler trên đồ thị vô hướng là NP-complete, nhưng bài này đồ thị đơn giản, xác định số cạnh từ $S$ sang $T$ là xác định được hướng các cạnh còn lại, rồi áp dụng định lý BEST, độ phức tạp $O(a+b+c+d)$.
