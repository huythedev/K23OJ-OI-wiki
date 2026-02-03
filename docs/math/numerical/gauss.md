author: StudyingFather, CCXXXI, Chrogeek, ChungZH, countercurrent-time, Early0v0, Enter-tainer, GavinZhengOI, Great-designer, H-J-Granger, henrytbtrue, HeRaNO, huayucaiji, iamtwz, Ir1d, ksyx, MegaOwIer, NachtgeistW, P-Y-Y, qwqAutomaton, shuzhouliu, shuzhouliu-bot, Siger Young, sshwy, SukkaW, Tiphereth-A, tsentau, WhenMelancholy, Xeonacid, Yukimaikoriya, Zhoier, zyj-111, qute-firefly-26710-zjyjoe-lg-592080

## Giới thiệu

Khử Gauss (Gauss–Jordan elimination) là thuật toán kinh điển để giải hệ phương trình tuyến tính, có vai trò quan trọng trong toán học hiện đại và là nội dung cốt lõi của đại số tuyến tính.

Ngoài giải hệ, khử Gauss còn dùng để tính định thức, tìm nghịch đảo ma trận và nhiều ứng dụng kỹ thuật khác.

## Phương pháp khử và tư tưởng khử Gauss

### Định nghĩa

Phương pháp khử là biểu diễn ẩn của một phương trình bằng biểu thức chứa ẩn khác rồi thế vào phương trình khác để loại bỏ ẩn; hoặc nhân một phương trình với hằng số rồi cộng vào phương trình khác để loại bỏ ẩn. Phương pháp này chủ yếu dùng cho hệ hai ẩn.

### Giải thích

Ví dụ 1: dùng khử để giải hệ hai ẩn:

$$
\begin{cases}
4x+y&=100 \\
x-y&=100
\end{cases}
$$

Giải: cộng hai phương trình, khử $y$:

$$
5x = 200
$$

Suy ra:

$$
x = 40
$$

Thay vào phương trình thứ hai:

$$
y = -60
$$

### Cốt lõi lý thuyết của phương pháp khử

-   Hoán đổi hai phương trình, nghiệm không đổi;
-   Nhân một phương trình với số khác 0, nghiệm không đổi;
-   Cộng một phương trình với bội của phương trình khác, nghiệm không đổi.

### Khái niệm tư tưởng khử Gauss

Gauss phân tích và rút ra:

-   Trong khử, chỉ hệ số của biến tham gia tính toán và thay đổi;
-   Các biến không tham gia tính toán và không đổi;
-   Có thể dùng vị trí hệ số để biểu diễn biến, bỏ qua ký hiệu biến;
-   Việc rút gọn biến trong tính toán không đổi nghiệm.

Từ đó, Gauss đề xuất khử Gauss: biến đổi ma trận mở rộng về dạng bậc thang rút gọn bằng phép biến đổi sơ cấp, rồi gán giá trị cho ẩn tự do theo tiêu chí độc lập tuyến tính, cuối cùng viết nghiệm tổng quát.

## Phương pháp 5 bước khử Gauss

## Giải thích

Sau khi đưa ma trận mở rộng về dạng rút gọn, việc gán ẩn tự do cần kiến thức độc lập tuyến tính và có yếu tố kinh nghiệm, nên khó học. Vì vậy chia thành 5 bước:

1.  Biến đổi sơ cấp đưa ma trận mở rộng về dạng rút gọn;
2.  Khôi phục hệ phương trình;
3.  Giải ẩn đầu tiên của mỗi phương trình;
4.  Bổ sung ẩn tự do;
5.  Viết nghiệm tổng quát dạng vectơ.

Minh họa bằng ví dụ.

## Quá trình

Ví dụ 2: giải hệ:

$$
\begin{cases}
2x_1+5x_3+6x_4&=9 \\
x_3+x_4&=-4 \\
2x_3+2x_4&=-8
\end{cases}
$$

### Biến đổi ma trận mở rộng về dạng rút gọn

Ma trận mở rộng là ghép ma trận hệ số $A$ với cột hằng $b$ thành $(A | b)$. Dùng phép biến đổi sơ cấp đưa về dạng rút gọn.

$$
\left(\begin{matrix}
2 & 0 & 5 & 6 \\
0 & 0 & 1 & 1 \\
0 & 0 & 2 & 2
\end{matrix} \middle|
\begin{matrix}
9 \\
-4 \\
-8
\end{matrix} \right)
$$

$$
\xrightarrow{r_3-2r_2}
\left(\begin{matrix}
2 & 0 & 5 & 6 \\
0 & 0 & 1 & 1 \\
0 & 0 & 0 & 0
\end{matrix} \middle|
\begin{matrix}
9 \\
-4 \\
0
\end{matrix} \right)
$$

Đưa về dạng bậc thang

$$
\xrightarrow{\frac{r_1}{2}}
\left(\begin{matrix}
1 & 0 & 2.5 & 3 \\
0 & 0 & 1 & 1 \\
0 & 0 & 0 & 0
\end{matrix} \middle|
\begin{matrix}
4.5 \\
-4 \\
0
\end{matrix} \right)
$$

$$
\xrightarrow{r_1-r_2 \times 2.5}
\left(\begin{matrix}
1 & 0 & 0 & 0.5 \\
0 & 0 & 1 & 1 \\
0 & 0 & 0 & 0
\end{matrix} \middle|
\begin{matrix}
14.5 \\
-4 \\
0
\end{matrix} \right)
$$

Đưa về dạng rút gọn

### Khôi phục hệ phương trình

$$
\begin{cases}
x_1+0.5x_4 &= 14.5\\
x_3+x_4 &= -4 \\
\end{cases}
$$

???+ note "Giải thích"
    Khôi phục hệ là viết lại dạng phương trình từ ma trận rút gọn, gán lại vị trí hệ số cho biến, biến dấu gạch đứng thành dấu bằng.

### Giải ẩn đầu tiên

$$
\begin{cases}
x_1 = -0.5x_4+14.5\notag \\
x_3 = -x_4-4\notag
\end{cases}
$$

???+ note "Giải thích"
    Với mỗi phương trình, giải ẩn xuất hiện đầu tiên theo các ẩn còn lại, ở đây là $x_1$ và $x_3$.

### Bổ sung ẩn tự do

$$
\begin{cases}
x_1 = -0.5x_4+14.5 \\
x_2 = x_2 \\
x_3 = -x_4-4 \\
x_4 = x_4
\end{cases}
$$

???+ note "Giải thích"
    Sau bước 3, thấy $x_2$ và $x_4$ là ẩn tự do không bị ràng buộc, nên bổ sung $x_2=x_2, x_4=x_4$ cho dễ hiểu.

### Viết nghiệm tổng quát dạng vectơ

$$
\begin{aligned}
\begin{pmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{pmatrix} &=
\begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix} x_2+
\begin{pmatrix} -0.5 \\ 0 \\ -1 \\ 1 \end{pmatrix} x_4 +
\begin{pmatrix} 14.5 \\ 0 \\ -4 \\ 0 \end{pmatrix} \\
&= \begin{pmatrix} 0 \\ 1 \\ 0 \\ 0 \end{pmatrix} C_1+
\begin{pmatrix} -0.5 \\ 0 \\ -1 \\ 1 \end{pmatrix} C_2 +
\begin{pmatrix} 14.5 \\ 0 \\ -4 \\ 0 \end{pmatrix}
\end{aligned}
$$

Trong đó $C_1$ và $C_2$ là hằng số tùy ý.

???+ note "Giải thích"
    Từ bước 4, viết nghiệm dưới dạng tổ hợp tuyến tính; vì $x_2, x_4$ tự do nên đặt bằng hằng tùy ý $C_1, C_2$.

## Tính định thức

### Giải thích

Định thức của ma trận vuông $N \times N$ có thể hiểu là thể tích có hướng của khối tạo bởi các vectơ cột.

Ví dụ:

$$
\begin{vmatrix}
1 & 0 \\
0 & 1 \end{vmatrix} = 1
$$

$$
\begin{vmatrix}
1 & 2 \\
2 & 1 \end{vmatrix} = -3
$$

Công thức định thức:

$$
\operatorname{det}(A)=\sum_{\sigma \in S_{n}} \operatorname{sgn}(\sigma) \prod_{i=1}^{n} a_{i, \sigma(i)}
$$

Trong đó $S_n$ là tập hoán vị độ dài $n$, $\sigma$ là một hoán vị. Nếu $\sigma$ có số nghịch thế chẵn thì $\operatorname{sgn}(\sigma)=1$, lẻ thì $\operatorname{sgn}(\sigma)=−1$.

Từ góc nhìn thể tích:

-   Chuyển vị ma trận, định thức không đổi;
-   Hoán đổi hai hàng (cột), định thức đổi dấu;
-   Cộng/trừ một hàng (cột) với hàng (cột) khác, định thức không đổi;
-   Nhân một hàng (cột) với $k$, định thức nhân $k$.

Vì vậy, sau khi khử Gauss, ta được ma trận đường chéo, định thức là tích các phần tử đường chéo; dấu phụ thuộc vào số lần đổi hàng. Do đó có thể tính định thức trong $O(n^3)$.

Nếu tại một cột không tìm thấy phần tử khác 0, thuật toán dừng và trả 0.

### Cài đặt

```cpp
constexpr double EPS = 1E-9;
int n;
vector<vector<double>> a(n, vector<double>(n));

double det = 1;
for (int i = 0; i < n; ++i) {
  int k = i;
  for (int j = i + 1; j < n; ++j)
    if (abs(a[j][i]) > abs(a[k][i])) k = j;
  if (abs(a[k][i]) < EPS) {
    det = 0;
    break;
  }
  swap(a[i], a[k]);
  if (i != k) det = -det;
  det *= a[i][i];
  for (int j = i + 1; j < n; ++j) a[i][j] /= a[i][i];
  for (int j = 0; j < n; ++j)
    if (j != i && abs(a[j][i]) > EPS)
      for (int k = i + 1; k < n; ++k) a[j][k] -= a[i][k] * a[j][i];
}

cout << det;
```

## Tìm nghịch đảo ma trận

Với ma trận vuông $A$, nếu tồn tại $A^{-1}$ sao cho $A \times A^{-1} = A^{-1} \times A = I$ thì $A$ khả nghịch, $A^{-1}$ là nghịch đảo của $A$.

Cho ma trận $A$ bậc $n$, tìm nghịch đảo như sau:

1.  Lập ma trận $n \times 2n$ là $(A, I_n)$;
2.  Dùng khử Gauss đưa về dạng rút gọn $(I_n, A^{-1})$, khi đó $A^{-1}$ là nghịch đảo. Nếu nửa trái không phải $I_n$ thì $A$ không khả nghịch.

Chứng minh đúng cần nhiều kiến thức đại số tuyến tính, không trình bày ở đây.

## Khử Gauss cho hệ phương trình XOR

Hệ XOR có dạng:

$$
\begin{cases}
a_{1,1}x_1 \oplus a_{1,2}x_2 \oplus \cdots \oplus a_{1,n}x_n &= b_1\\
a_{2,1}x_1 \oplus a_{2,2}x_2 \oplus \cdots \oplus a_{2,n}x_n &= b_2\\
\cdots &\cdots \\ a_{m,1}x_1 \oplus a_{m,2}x_2 \oplus \cdots \oplus a_{m,n}x_n &= b_m
\end{cases}
$$

trong đó $\oplus$ là XOR bit (tức `xor` hoặc `^` trong C++), và các hệ số/hằng đều là $0$ hoặc $1$.

Vì XOR thỏa giao hoán và kết hợp, có thể khử tương tự Gauss. Lưu ý dùng “khử XOR” thay vì cộng/trừ, và không cần nhân/chia vì hệ số chỉ là $0$ hoặc $1$.

Ma trận mở rộng là ma trận $01$, nên có thể dùng `std::bitset` để tối ưu, đưa độ phức tạp xuống $O(\dfrac{n^2m}{\omega})$, với $n$ là số ẩn, $m$ là số phương trình, $\omega$ thường là $32$ (phụ thuộc máy).

Mã tham khảo:

```cpp
std::bitset<1010> matrix[2010];  // matrix[1~n]: ma trận mở rộng, vị trí 0 là hằng số

std::vector<bool> GaussElimination(
    int n, int m)  // n là số ẩn, m là số phương trình, trả nghiệm
                   // (nhiều nghiệm / vô nghiệm trả vector rỗng)
{
  for (int i = 1; i <= n; i++) {
    int cur = i;
    while (cur <= m && !matrix[cur].test(i)) cur++;
    if (cur > m) return std::vector<bool>(0);
    if (cur != i) swap(matrix[cur], matrix[i]);
    for (int j = 1; j <= m; j++)
      if (i != j && matrix[j].test(i)) matrix[j] ^= matrix[i];
  }
  std::vector<bool> ans(n + 1);
  for (int i = 1; i <= n; i++) ans[i] = matrix[i].test(0);
  return ans;
}
```

## Bài tập

-   [Codeforces - 巫师和赌注](http://codeforces.com/contest/167/problem/E)
-   [luogu - SDOI2010 外星千足虫](https://www.luogu.com.cn/problem/P2447)
