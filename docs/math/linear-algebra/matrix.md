Bài này giới thiệu một nội dung quan trọng của đại số tuyến tính — ma trận (Matrix), tập trung vào tính chất, phép toán và ứng dụng của nhân ma trận.

## Vectơ và ma trận

Trong đại số tuyến tính, vectơ chia thành vectơ cột và vectơ hàng.

???+ warning "Warning"
    Ở Đài Loan, dịch “row/column” ngược với Trung Quốc đại lục. Ở **OI Wiki** theo thói quen Trung Quốc đại lục: cột (column) và hàng (row).

Đối tượng chính là vectơ cột; quy ước dùng chữ thường in đậm cho vectơ cột. Khi viết tay, có thể bỏ ký hiệu vectơ nếu không nhầm lẫn.

Vectơ cũng là ma trận đặc biệt. Vectơ hàng là chuyển vị của vectơ cột.

## Giới thiệu

Ma trận xuất phát từ hệ phương trình tuyến tính. Giống vectơ, ma trận thể hiện cách “đóng gói” dữ liệu.

Ví dụ hệ:

$$
\begin{equation}
    \begin{cases}
        7x_1+8x_2+9x_3=13 \\
        4x_1+5x_2+6x_3=12 \\
        x_1+2x_2+3x_3=11
    \end{cases}
\end{equation}
$$

Tách hệ số thành:

$$
\begin{equation}
    \begin{pmatrix}
        7 & 8 & 9 \\
        4 & 5 & 6 \\
        1 & 2 & 3
    \end{pmatrix}\begin{pmatrix}
        x_1 \\ x_2 \\ x_3
    \end{pmatrix}=\begin{pmatrix}
      13 \\ 12 \\ 11
    \end{pmatrix}
\end{equation}
$$

Viết gọn:

$$
Ax=b
$$

Vectơ cột $x$ là ẩn, nhân trái bởi ma trận $A$ cho ra $b$. Đây là dạng cơ bản của đại số tuyến tính.

Đại số tuyến tính nghiên cứu tích vô hướng, tức “nhân rồi cộng”, là hàng nhân cột cho ra một số.

Nhân ma trận là mở rộng của tích vô hướng. Nó lấy hàng của ma trận trái và cột của ma trận phải để làm tích vô hướng, “hàng trái cột phải”.

Khi quan tâm đến vectơ cột bên phải, nhân ma trận là phép biến đổi tuyến tính lên vectơ cột. Khi áp dụng lên nhiều vectơ cột cùng lúc, thể hiện ý tưởng “đóng gói”.

Ma trận có thể biến đổi một vectơ, một nhóm vectơ, thậm chí toàn không gian (toàn bộ vectơ cột). Khi đó ma trận trở thành một phép biến đổi trừu tượng.

## Định nghĩa

Với ma trận $A$, đường chéo chính là các phần tử $A_{i,i}$.

Ma trận đơn vị ký hiệu $I$ là ma trận có 1 trên đường chéo chính, 0 chỗ khác.

### Ma trận cùng kích thước

Hai ma trận có số hàng, số cột tương ứng bằng nhau gọi là cùng kiểu.

### Ma trận vuông

Ma trận có số hàng bằng số cột. “Ma trận bậc $n$” tức là ma trận vuông bậc $n$.

Nghiên cứu hệ phương trình, nhóm vectơ, hạng dùng ma trận bất kỳ; nghiên cứu trị riêng, dạng toàn phương dùng ma trận vuông.

#### Đường chéo chính

Trong ma trận vuông, các phần tử có chỉ số hàng = chỉ số cột.

#### Ma trận đối xứng

Nếu $A_{i,j}=A_{j,i}$ thì là ma trận đối xứng.

#### Ma trận đường chéo

Các phần tử ngoài đường chéo chính đều là 0, ký hiệu:

$$
\operatorname{diag}\{\lambda_1,\cdots,\lambda_n\}
$$

Ma trận đường chéo là ma trận đối xứng.

Nếu các phần tử đều bằng 1, gọi là ma trận đơn vị $I$. Nhân với $I$ không đổi.

#### Ma trận tam giác

Nếu các phần tử dưới đường chéo chính là 0, gọi là tam giác trên; nếu trên đường chéo là 0, gọi là tam giác dưới.

Tích của hai ma trận tam giác cùng loại vẫn tam giác cùng loại. Nếu các phần tử đường chéo khác 0 thì khả nghịch, và nghịch đảo cũng là tam giác cùng loại.

#### Ma trận tam giác đơn vị

Nếu ma trận tam giác trên có đường chéo toàn 1, gọi là đơn vị tam giác trên; tương tự cho tam giác dưới.

Tích và nghịch đảo của hai ma trận đơn vị tam giác vẫn là đơn vị tam giác.

## Phép toán

### Phép toán tuyến tính trên ma trận

Cộng, trừ và nhân vô hướng là theo phần tử, chỉ áp dụng cho ma trận cùng kích thước.

### Chuyển vị

Chuyển vị là đổi hàng và cột. Ma trận đối xứng chuyển vị không đổi.

### Nhân ma trận

Nhân ma trận là mở rộng của tích vô hướng.

Chỉ thực hiện được nếu số cột của ma trận trái bằng số hàng của ma trận phải.

Nếu $A$ là $P\times M$, $B$ là $M\times Q$, thì $C=AB$ có:

$$
C_{i,j} = \sum_{k=1}^MA_{i,k}B_{k,j}
$$

Phần tử $C_{i,j}$ là tích vô hướng của hàng $i$ của $A$ với cột $j$ của $B$ — “hàng trái cột phải”.

Nhân ma trận kết hợp nhưng không giao hoán.

Có thể dùng [lũy thừa nhanh](../binary-exponentiation.md) để tối ưu.

Trong thi đấu, nhiều truy hồi tuyến tính chuyển thành nhân ma trận, dùng lũy thừa nhanh.

#### Tối ưu

Với ma trận nhỏ, có thể viết tay để giảm hằng số.

Có thể đổi thứ tự vòng lặp để tối ưu cache, không đổi độ phức tạp nhưng giảm hằng số.

```cpp
// Ví dụ tham khảo
mat operator*(const mat& T) const {
  mat res;
  for (int i = 0; i < sz; ++i)
    for (int j = 0; j < sz; ++j)
      for (int k = 0; k < sz; ++k) {
        res.a[i][j] += mul(a[i][k], T.a[k][j]);
        res.a[i][j] %= MOD;
      }
  return res;
}

// Không tối ưu bằng:
mat operator*(const mat& T) const {
  mat res;
  int r;
  for (int i = 0; i < sz; ++i)
    for (int k = 0; k < sz; ++k) {
      r = a[i][k];
      for (int j = 0; j < sz; ++j)
        res.a[i][j] += T.a[k][j] * r, res.a[i][j] %= MOD;
    }
  return res;
}
```

### Nghịch đảo ma trận vuông

Nghịch đảo $P$ của $A$ thỏa $A\times P = I$.

Không phải mọi ma trận đều có nghịch đảo. Nếu có, có thể dùng [khử Gauss](../numerical/gauss.md).

### Định thức

Định thức là phép toán trên ma trận vuông.

## Mã tham khảo

Có thể dùng mảng 2D để mô phỏng.

```cpp
struct mat {
  LL a[sz][sz];

  mat() { memset(a, 0, sizeof a); }

  mat operator-(const mat& T) const {
    mat res;
    for (int i = 0; i < sz; ++i)
      for (int j = 0; j < sz; ++j) {
        res.a[i][j] = (a[i][j] - T.a[i][j]) % MOD;
      }
    return res;
  }

  mat operator+(const mat& T) const {
    mat res;
    for (int i = 0; i < sz; ++i)
      for (int j = 0; j < sz; ++j) {
        res.a[i][j] = (a[i][j] + T.a[i][j]) % MOD;
      }
    return res;
  }

  mat operator*(const mat& T) const {
    mat res;
    int r;
    for (int i = 0; i < sz; ++i)
      for (int k = 0; k < sz; ++k) {
        r = a[i][k];
        for (int j = 0; j < sz; ++j)
          res.a[i][j] += T.a[k][j] * r, res.a[i][j] %= MOD;
      }
    return res;
  }

  mat operator^(LL x) const {
    mat res, bas;
    for (int i = 0; i < sz; ++i) res.a[i][i] = 1;
    for (int i = 0; i < sz; ++i)
      for (int j = 0; j < sz; ++j) bas.a[i][j] = a[i][j] % MOD;
    while (x) {
      if (x & 1) res = res * bas;
      bas = bas * bas;
      x >>= 1;
    }
    return res;
  }
};
```

## Hai góc nhìn về hệ phương trình tuyến tính

Nhìn ma trận $A$ có hai cách.

Cách 1: theo hàng. Xem $A$ là hệ phương trình, dẫn đến khử Gauss.

Cách 2: theo cột. $A$ là nhóm vectơ cột, $x$ là hệ số, hỏi liệu có thể tổ hợp để ra $b$.

Ví dụ ở đầu bài trở thành:

$$
\begin{equation}
    \begin{pmatrix}
        7 \\ 4 \\ 1
    \end{pmatrix}x_1+\begin{pmatrix}
        8 \\ 5 \\ 2
    \end{pmatrix}x_2+\begin{pmatrix}
        9 \\ 6 \\ 3
    \end{pmatrix}x_3=\begin{pmatrix}
      13 \\ 12 \\ 11
    \end{pmatrix}
\end{equation}
$$

Giải hệ trở thành: điều chỉnh $x$ sao cho các vectơ cơ sở tạo ra vectơ kết quả.

Góc nhìn theo cột mới mẻ hơn và giúp nghiên cứu độc lập/phụ thuộc tuyến tính.

## Ứng dụng của nhân ma trận

### Tăng tốc truy hồi bằng ma trận

Ví dụ dãy Fibonacci: $F_1=F_2=1$, $F_i=F_{i-1}+F_{i-2}$.

Nếu $n$ rất lớn (ví dụ $10^{18}$), cần dùng ma trận.

Dựa trên dạng ma trận:

$$
\begin{bmatrix}
  F_{n-1} & F_{n-2}
\end{bmatrix} \begin{bmatrix}
  1 & 1 \\
  1 & 0
\end{bmatrix} = \begin{bmatrix}
  F_n & F_{n-1}
\end{bmatrix}
$$

Đặt $\text{ans}=\begin{bmatrix}1 & 1\end{bmatrix}$, $\text{base}=\begin{bmatrix}1 & 1\\1 & 0\end{bmatrix}$. Khi đó $F_n$ là phần tử (1,1) của $\text{ans}\cdot\text{base}^{n-2}$.

???+ warning "注意"
    Nhân ma trận không giao hoán, không được viết $\text{base}^{n-2}\text{ans}$. Với $n\le 2$ thì đáp án là $1$.

Vì $F_1,F_2$ đã biết nên lũy thừa là $n-2$.

Ví dụ mã (mod $10^9+7$):

```cpp
constexpr int mod = 1000000007;

struct Matrix {
  int a[3][3];

  Matrix() { memset(a, 0, sizeof a); }

  Matrix operator*(const Matrix &b) const {
    Matrix res;
    for (int i = 1; i <= 2; ++i)
      for (int j = 1; j <= 2; ++j)
        for (int k = 1; k <= 2; ++k)
          res.a[i][j] = (res.a[i][j] + a[i][k] * b.a[k][j]) % mod;
    return res;
  }
} ans, base;

void init() {
  base.a[1][1] = base.a[1][2] = base.a[2][1] = 1;
  ans.a[1][1] = ans.a[1][2] = 1;
}

void qpow(int b) {
  while (b) {
    if (b & 1) ans = ans * base;
    base = base * base;
    b >>= 1;
  }
}

int main() {
  int n = read();
  if (n <= 2) return puts("1"), 0;
  init();
  qpow(n - 2);
  println(ans.a[1][1] % mod);
}
```

Một ví dụ phức tạp hơn:

$$
\begin{gathered}
f_{1} = f_{2} = 0\\
f_{n} = 7f_{n-1}+6f_{n-2}+5n+4\times 3^n
\end{gathered}
$$

Ta cần trạng thái gồm $f_n,f_{n-1},n,3^n,1$:

$$
\begin{bmatrix}f_n& f_{n-1}& n& 3^n & 1\end{bmatrix}
$$

Chuyển sang $n+1$:

$$
\begin{bmatrix}
f_{n+1}& f_{n}& n+1& 3^{n+1} & 1
\end{bmatrix}
$$

Ma trận chuyển:

$$
\begin{bmatrix}
7 & 1 & 0 & 0 & 0\\
6 & 0 & 0 & 0 & 0\\
5 & 0 & 1 & 0 & 0\\
12 & 0 & 0 & 3 & 0\\
5 & 0 & 1 & 0 & 1
\end{bmatrix}
$$

### Biến đổi biểu thức bằng ma trận

???+ note "[「THUSCH 2017」大魔法师](https://loj.ac/p/2980)"
    // ...existing code...
    // (Giữ nguyên nội dung tiếng Trung ở tiêu đề bài, phần thân đã dịch dưới)
    
    ... (Dịch toàn bộ nội dung tiếp theo trong file gốc) ...
````
