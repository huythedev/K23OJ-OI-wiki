Dãy Fibonacci (The Fibonacci sequence, [OEIS A000045](http://oeis.org/A000045)) được định nghĩa như sau:

$$
F_0 = 0, F_1 = 1, F_n = F_{n-1} + F_{n-2}
$$

Một vài số đầu tiên:

$$
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, \dots
$$

## Dãy Lucas

Dãy Lucas (The Lucas sequence, [OEIS A000032](http://oeis.org/A000032)) được định nghĩa:

$$
L_0 = 2, L_1 = 1, L_n = L_{n-1} + L_{n-2}
$$

Một vài số đầu:

$$
2, 1, 3, 4, 7, 11, 18, 29, 47, 76, 123, 199, \dots
$$

Nghiên cứu Fibonacci thường cần dùng dãy Lucas làm công cụ.

## Công thức tổng quát của Fibonacci

Số Fibonacci thứ $n$ có thể tính trong $\Theta(n)$ bằng công thức truy hồi, nhưng ta vẫn có cách nhanh hơn.

### Nghiệm giải tích

Nghiệm giải tích là công thức tường minh. Ta có công thức Binet:

$$
F_n = \frac{\left(\frac{1 + \sqrt{5}}{2}\right)^n - \left(\frac{1 - \sqrt{5}}{2}\right)^n}{\sqrt{5}}
$$

Công thức này dễ chứng minh bằng quy nạp, hoặc suy ra từ hàm sinh, hoặc giải phương trình.

Bạn sẽ thấy hạng tử thứ hai ở tử luôn nhỏ hơn $1$ và giảm theo cấp số nhân. Vì vậy có thể viết:

$$
F_n = \left[\frac{\left(\frac{1 + \sqrt{5}}{2}\right)^n}{\sqrt{5}}\right]
$$

Dấu ngoặc vuông là làm tròn tới số nguyên gần nhất.

Hai công thức này yêu cầu độ chính xác rất cao nên thực tế ít dùng. Tuy vậy, trong OI, kết hợp với khái niệm thặng dư bậc hai và nghịch đảo modulo, công thức này vẫn hữu ích.

### Công thức tổng quát của Lucas

Ta có:

$$
L_n = \left(\frac{1 + \sqrt{5}}{2}\right)^n + \left(\frac{1 - \sqrt{5}}{2}\right)^n
$$

Rất giống Fibonacci. Thật vậy:

$$
\frac{L_n + F_n\sqrt{5}}{2} = \left(\frac{1 + \sqrt{5}}{2}\right)^n
$$

Nói cách khác, $L_n$ và $F_n$ đúng là các hệ số sau khi khai triển nhị thức của $\left(\frac{1 + \sqrt{5}}{2}\right)^n$ rồi gom nhóm. Điều này cũng cho thấy nghiệm của phương trình Pell

$$
x^2-5y^2=-4
$$

thỏa:

$$
\frac{x_n + y_n\sqrt{5}}{2} = \frac{L_n + F_n\sqrt{5}}{2}
$$

chính là dãy Lucas và Fibonacci. Do đó:

$$
{L_n}^2-5{F_n}^2=-4
$$

### Dạng ma trận

Truy hồi Fibonacci có thể biểu diễn bằng nhân ma trận:

$$
\begin{bmatrix}F_{n-1} & F_{n} \cr\end{bmatrix} = \begin{bmatrix}F_{n-2} & F_{n-1} \cr\end{bmatrix} \begin{bmatrix}0 & 1 \cr 1 & 1 \cr\end{bmatrix}
$$

Đặt $P = \begin{bmatrix}0 & 1 \cr 1 & 1 \cr\end{bmatrix}$, ta được:

$$
\begin{bmatrix}F_n & F_{n+1} \cr\end{bmatrix} = \begin{bmatrix}F_0 & F_1 \cr\end{bmatrix} P^n
$$

Vì vậy có thể tính Fibonacci trong $\Theta(\log n)$ bằng nhân ma trận. Ngoài ra, công thức tường minh ở trên cũng có thể suy ra bằng chéo hóa ma trận.

### Phương pháp nhân đôi nhanh

Từ trên ta có:

$$
\begin{aligned}
F_{2k} &= F_k (2 F_{k+1} - F_{k}) \\
F_{2k+1} &= F_{k+1}^2 + F_{k}^2
\end{aligned}
$$

Từ đó có thể tính nhanh hai số Fibonacci liên tiếp (hằng số tốt hơn nhân ma trận). Code trả về cặp $(F_n,F_{n+1})$:

```cpp
pair<int, int> fib(int n) {
  if (n == 0) return {0, 1};
  auto p = fib(n >> 1);
  int c = p.first * (2 * p.second - p.first);
  int d = p.first * p.first + p.second * p.second;
  if (n & 1)
    return {d, c + d};
  else
    return {c, d};
}
```

## Tính chất

Dãy Fibonacci có nhiều tính chất thú vị, dưới đây là một số tính chất cơ bản:

1.  Đẳng thức Cassini: $F_{n-1} F_{n+1} - F_n^2 = (-1)^n$.
2.  Tính chất cộng chỉ số: $F_{n+k} = F_k F_{n+1} + F_{k-1} F_n$.
3.  Lấy $k = n$ ở trên: $F_{2n} = F_n (F_{n+1} + F_{n-1})$.
4.  Từ đó suy ra $\forall k\in \mathbb{N},F_n|F_{nk}$.
5.  Chiều ngược: $\forall F_a|F_b,a|b$.
6.  Tính chất GCD: $(F_m, F_n) = F_{(m, n)}$.
7.  Dùng hai số Fibonacci liên tiếp làm đầu vào khiến thuật toán Euclid đạt độ phức tạp tệ nhất (xem [Wikipedia - Lame](https://en.wikipedia.org/wiki/Gabriel_Lam%C3%A9)).

### Quan hệ giữa Fibonacci và Lucas

Không khó thấy các đẳng thức giữa Lucas và Fibonacci giống các công thức lượng giác. Ví dụ:

$$
\frac{L_n + F_n\sqrt{5}}{2} = \left(\frac{1 + \sqrt{5}}{2}\right)^n
$$

giống:

$$
\cos nx + i\sin nx = \left(\cos x + i\sin x\right)^n
$$

và

$$
{L_n}^2-5{F_n}^2=-4
$$

giống:

$$
\cos^2 x + \sin^2 x = 1
$$

Do đó Lucas giống cos, Fibonacci giống sin. Ví dụ từ:

$$
\left(\frac{1 + \sqrt{5}}{2}\right)^m\left(\frac{1 + \sqrt{5}}{2}\right)^n = \left(\frac{1 + \sqrt{5}}{2}\right)^{m+n}
$$

suy ra:

$$
2L_{m+n}=5F_mF_n+L_mL_n
$$

$$
2F_{m+n}=F_mL_n+L_mF_n
$$

Từ đó có công thức nhân đôi chỉ số:

$$
L_{2n}={L_n}^2-2{\left(-1\right)}^n
$$

$$
F_{2n}=F_nL_n
$$

Đây cũng là một cách nhân đôi nhanh. Tương tự có thể suy ra nhiều đẳng thức khác kiểu lượng giác.

## Mã hóa Fibonacci

Ta có thể dùng Fibonacci để mã hóa số nguyên dương. Theo [định lý Zeckendorf](https://zh.wikipedia.org/wiki/%E9%BD%8A%E8%82%AF%E5%A4%9A%E5%A4%AB%E5%AE%9A%E7%90%86), mọi số tự nhiên $n$ có biểu diễn duy nhất dưới dạng tổng các số Fibonacci:

$$
N = F_{k_1} + F_{k_2} + \ldots + F_{k_r}
$$

và $k_1 \ge k_2 + 2,\ k_2 \ge k_3 + 2,\  \ldots,\  k_r \ge 2$ (không dùng hai số Fibonacci liên tiếp).

Ta mã hóa một số nguyên dương bằng $d_0 d_1 d_2 \dots d_s 1$, trong đó $d_i=1$ nghĩa là $F_{i+2}$ được dùng. Thêm một bit 1 ở cuối để đánh dấu kết thúc (vì vậy sẽ có hai bit 1 liên tiếp). Ví dụ:

$$
\begin{aligned}
1 &=& 1 &=& F_2 &=& (11)_F \\
2 &=& 2 &=& F_3 &=& (011)_F \\
6 &=& 5 + 1 &=& F_5 + F_2 &=& (10011)_F \\
8 &=& 8 &=& F_6 &=& (000011)_F \\
9 &=& 8 + 1 &=& F_6 + F_2 &=& (100011)_F \\
19 &=& 13 + 5 + 1 &=& F_7 + F_5 + F_2 &=& (1001011)_F
\end{aligned}
$$

Quá trình mã hóa dùng tham lam:

1.  Duyệt Fibonacci từ lớn tới nhỏ cho đến khi $F_i\le n$.
2.  Trừ $n$ đi $F_i$, đặt 1 tại vị trí $i-2$ của mã (đánh số vị trí từ trái sang phải bắt đầu 0).
3.  Nếu $n>0$ quay lại bước 1.
4.  Thêm một bit 1 ở cuối để đánh dấu kết thúc.

Giải mã tương tự: bỏ bit cuối, với mỗi vị trí $i$ có bit 1 (đánh số từ trái sang phải bắt đầu 0), cộng $F_{i+2}$ vào đáp án. Kết quả là số ban đầu.

## Tính chu kỳ theo modulo

Xét Fibonacci modulo $p$, dùng nguyên lý Dirichlet có thể chứng minh dãy có chu kỳ. Xét $p^2+1$ cặp liên tiếp:

$$
(F_1,\ F_2),\ (F_2,\ F_3),\ \ldots,\ (F_{p^2 + 1},\ F_{p^2 + 2})
$$

Có $p$ lớp dư, nên trong $p^2+1$ cặp sẽ có hai cặp trùng nhau. Từ đó sinh ra cùng dãy phía sau, nên dãy có chu kỳ.

### Chu kỳ Pisano

Chu kỳ dương nhỏ nhất của Fibonacci modulo $m$ gọi là [chu kỳ Pisano](https://en.wikipedia.org/wiki/Pisano_period) (Pisano periods, [OEIS A001175](http://oeis.org/A001175)).

Chu kỳ Pisano không vượt quá $6m$, và chỉ đạt bằng khi $m=2\times 5^k$.

Khi cần tính $F_n \bmod m$ với $n$ rất lớn, cần tìm chu kỳ (không nhất thiết là chu kỳ nhỏ nhất).

Dễ kiểm tra: modulo 2 có chu kỳ 3, modulo 5 có chu kỳ 20.

Nếu $a$ và $b$ nguyên tố cùng nhau, chu kỳ Pisano của $ab$ là bội chung nhỏ nhất của chu kỳ của $a$ và $b$.

Tính chu kỳ còn cần các kết luận sau:

Kết luận 1: Với số nguyên tố lẻ $p\equiv 1,4 \pmod 5$, $p-1$ là chu kỳ của Fibonacci modulo $p$. Nghĩa là chu kỳ Pisano chia hết $p-1$.

Chứng minh:

Khi đó $5^\frac{p-1}{2} \equiv 1\pmod p$.

Khai triển nhị thức:

$$
F_p=\frac{2}{2^p\sqrt{5}}\left(\dbinom{p}{1}\sqrt{5}+\dbinom{p}{3}\sqrt{5}^3+\ldots+\dbinom{p}{p}\sqrt{5}^p\right)\equiv\sqrt{5}^{p-1}\equiv 1\pmod p
$$

$$
F_{p+1}=\frac{2}{2^{p+1}\sqrt{5}}\left(\dbinom{p+1}{1}\sqrt{5}+\dbinom{p+1}{3}\sqrt{5}^3+\ldots+\dbinom{p+1}{p}\sqrt{5}^p\right)\equiv\frac{1}{2}\left(1+\sqrt{5}^{p-1}\right)\equiv 1\pmod p
$$

Vì $F_p$ và $F_{p+1}$ đều đồng dư 1 như $F_1$ và $F_2$, nên $p-1$ là chu kỳ.

Kết luận 2: Với số nguyên tố lẻ $p\equiv 2,3 \pmod 5$, $2p+2$ là chu kỳ của Fibonacci modulo $p$, tức chu kỳ Pisano chia hết $2p+2$.

Chứng minh:

Khi đó $5^\frac{p-1}{2} \equiv -1\pmod p$.

Khai triển nhị thức:

$$
F_{2p}=\frac{2}{2^{2p}\sqrt{5}}\left(\dbinom{2p}{1}\sqrt{5}+\dbinom{2p}{3}\sqrt{5}^3+\ldots+\dbinom{2p}{2p-1}\sqrt{5}^{2p-1}\right)
$$

$$
F_{2p+1}=\frac{2}{2^{2p+1}\sqrt{5}}\left(\dbinom{2p+1}{1}\sqrt{5}+\dbinom{2p+1}{3}\sqrt{5}^3+\ldots+\dbinom{2p+1}{2p+1}\sqrt{5}^{2p+1}\right)
$$

Sau khi lấy modulo $p$, trong $F_{2p}$ chỉ còn hạng $\dbinom{2p}{p}\equiv 2 \pmod p$; trong $F_{2p+1}$ còn các hạng $\dbinom{2p+1}{1}\equiv 1 \pmod p$、$\dbinom{2p+1}{p}\equiv 2 \pmod p$、$\dbinom{2p+1}{2p+1}\equiv 1 \pmod p$.

$$
F_{2p}\equiv\frac{1}{2}\dbinom{2p}{p}\sqrt{5}^{p-1}\equiv -1 \pmod p
$$

$$
F_{2p+1}\equiv\frac{1}{4}\left(\dbinom{2p+1}{1}+\dbinom{2p+1}{p}\sqrt{5}^{p-1}+\dbinom{2p+1}{2p+1}\sqrt{5}^{2p}\right)\equiv\frac{1}{4}\left(1-2+5\right)\equiv 1 \pmod p
$$

Vậy $F_{2p}$ và $F_{2p+1}$ trùng với $F_{-2}$ và $F_{-1}$, nên $2p+2$ là chu kỳ.

Kết luận 3: Với nguyên tố $p$, $M$ là chu kỳ của Fibonacci modulo $p^{k-1}$ khi và chỉ khi $Mp$ là chu kỳ modulo $p^k$. Đặc biệt, chu kỳ Pisano cũng tương đương theo quy tắc này.

Chứng minh:

Ở đây coi $\frac{1+\sqrt{5}}{2}$ là một phần tử.

Do:

$$
F_M=\frac{1}{\sqrt{5}}\left(\left(\frac{1+\sqrt{5}}{2}\right)^M-\left(\frac{1-\sqrt{5}}{2}\right)^M\right)\equiv 0\pmod {p^{k-1}}
$$

$$
F_{M+1}=\frac{1}{\sqrt{5}}\left(\left(\frac{1+\sqrt{5}}{2}\right)^{M+1}-\left(\frac{1-\sqrt{5}}{2}\right)^{M+1}\right)\equiv 1\pmod {p^{k-1}}
$$

Suy ra:

$$
\left(\frac{1+\sqrt{5}}{2}\right)^M \equiv \left(\frac{1-\sqrt{5}}{2}\right)^M\pmod {p^{k-1}}
$$

$$
1\equiv\frac{1}{\sqrt{5}}\left(\frac{1+\sqrt{5}}{2}\right)^M\left(\left(\frac{1+\sqrt{5}}{2}\right)-\left(\frac{1-\sqrt{5}}{2}\right)\right)=\left(\frac{1+\sqrt{5}}{2}\right)^M\pmod {p^{k-1}}
$$

Do chiều ngược cũng suy ra, nên $M$ là chu kỳ modulo $p^{k-1}$ iff:

$$
\left(\frac{1+\sqrt{5}}{2}\right)^M \equiv \left(\frac{1-\sqrt{5}}{2}\right)^M\equiv 1\pmod {p^{k-1}}
$$

Với $p$ nguyên tố lẻ, dùng [bổ đề nâng lũy thừa](../number-theory/lift-the-exponent.md):

$$
\nu_p\left(a^t-1\right)=\nu_p\left(a-1\right)+\nu_p(t)
$$

Với $p=2$, dùng:

$$
\nu_2\left(a^t-1\right)=\nu_2\left(a-1\right)+\nu_2\left(a+1\right)+\nu_2(t)-1
$$

Thay $a$ là $\left(\frac{1+\sqrt{5}}{2}\right)$ và $\left(\frac{1-\sqrt{5}}{2}\right)$, $t$ là $M$ và $Mp$, ta được điều kiện tương đương:

$$
\left(\frac{1+\sqrt{5}}{2}\right)^{Mp} \equiv \left(\frac{1-\sqrt{5}}{2}\right)^{Mp}\equiv 1\pmod {p^k}
$$

Vậy $Mp$ là chu kỳ modulo $p^k$. Do chu kỳ tương đương nên chu kỳ nhỏ nhất cũng tương đương.

Ba kết luận xong. Mã:

```cpp
struct prime {
  unsigned long long p;
  int times;
};

struct prime pp[2048];
int pptop;

unsigned long long get_cycle_from_mod(
    unsigned long long mod)  // Ở đây chỉ tính chu kỳ, không nhất thiết nhỏ nhất
{
  pptop = 0;
  srand(time(nullptr));
  while (n != 1) {
    __int128_t factor = (__int128_t)10000000000 * 10000000000;
    min_factor(mod, &factor);  // Tính ước nguyên tố nhỏ nhất
    struct prime temp;
    temp.p = factor;
    for (temp.times = 0; mod % factor == 0; temp.times++) {
      mod /= factor;
    }
    pp[pptop] = temp;
    pptop++;
  }
  unsigned long long m = 1;
  for (int i = 0; i < pptop; ++i) {
    int g;
    if (pp[i].p == 2) {
      g = 3;
    } else if (pp[i].p == 5) {
      g = 20;
    } else if (pp[i].p % 5 == 1 || pp[i].p % 5 == 4) {
      g = pp[i].p - 1;
    } else {
      g = (pp[i].p + 1) << 1;
    }
    m = lcm(m, g * qpow(pp[i].p, pp[i].times - 1));
  }
  return m;
}
```

## Bài tập

-   [SPOJ - Euclid Algorithm Revisited](http://www.spoj.com/problems/MAIN74/)
-   [SPOJ - Fibonacci Sum](http://www.spoj.com/problems/FIBOSUM/)
-   [HackerRank - Is Fibo](https://www.hackerrank.com/challenges/is-fibo/problem)
-   [Project Euler - Even Fibonacci numbers](https://www.hackerrank.com/contests/projecteuler/challenges/euler002/problem)
-   [洛谷 P4000 斐波那契数列](https://www.luogu.com.cn/problem/P4000)

    **Trang này chủ yếu dịch từ bài [Числа Фибоначчи](http://e-maxx.ru/algo/fibonacci_numbers) và bản tiếng Anh [Fibonacci Numbers](https://cp-algorithms.com/algebra/fibonacci-numbers.html). Bản Nga: Public Domain + Leave a Link; bản Anh: CC-BY-SA 4.0.**
