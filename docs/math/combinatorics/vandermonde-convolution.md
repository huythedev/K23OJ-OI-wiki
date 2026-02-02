## Giới thiệu

Tích chập Vandermonde là một đẳng thức gộp các tổ hợp, chủ yếu dùng trong suy luận công thức tổ hợp.

## Công thức Vandermonde

$$
\sum_{i=0}^k\binom{n}{i}\binom{m}{k-i}=\binom{n+m}{k}
$$

### Chứng minh

Xét chứng minh bằng nhị thức Newton:

$$
\begin{aligned}
\sum_{k=0}^{n+m}\binom{n+m}{k}x^k&=(x+1)^{n+m}\\
&=(x+1)^n(x+1)^m\\
&=\sum_{r=0}^n\binom{n}{r}x^r\sum_{s=0}^m\binom{m}{s}x^s\\
&=\sum_{k=0}^{n+m}\sum_{r=0}^k\binom{n}{r}\binom{m}{k-r}x^k\\
\end{aligned}
$$

Suy ra:

$$
\binom{n+m}{k}=\sum_{r=0}^k\binom{n}{r}\binom{m}{k-r}
$$

Nếu xét ý nghĩa tổ hợp:

Trong một tập kích thước $n+m$ chọn $k$ phần tử tương đương với việc chia tập thành hai phần kích thước $n$ và $m$, rồi chọn $i$ phần tử từ phần $n$ và $k-i$ từ phần $m$. Do ta đã liệt kê $i$, chỉ cần xét một cách chia vì các cách chia là tương đương.

## Hệ quả

### Hệ quả 1 và chứng minh

$$
\sum_{i=-r}^{s}\binom{n}{r+i}\binom{m}{s-i}=\binom{n+m}{r+s}
$$

Chứng minh tương tự công thức gốc.

### Hệ quả 2 và chứng minh

$$
\sum_{i=1}^n\binom{n}{i}\binom{n}{i-1}=\binom{2n}{n-1}
$$

Từ kiến thức tổ hợp cơ bản:

$$
\sum_{i=1}^n\binom{n}{i}\binom{n}{i-1}=\sum_{i=0}^{n-1}\binom{n}{i+1}\binom{n}{i}=\sum_{i=0}^{n-1}\binom{n}{n-1-i}\binom{n}{i}=\binom{2n}{n-1}
$$

### Hệ quả 3 và chứng minh

$$
\sum_{i=0}^n\binom{n}{i}^2=\binom{2n}{n}
$$

Từ kiến thức tổ hợp cơ bản:

$$
\sum_{i=0}^n\binom{n}{i}^2=\sum_{i=0}^n\binom{n}{i}\binom{n}{n-i}=\binom{2n}{n}
$$

### Hệ quả 4 và chứng minh

$$
\sum_{i=0}^m\binom{n}{i}\binom{m}{i}=\binom{n+m}{m}
$$

Từ kiến thức tổ hợp cơ bản:

$$
\sum_{i=0}^m\binom{n}{i}\binom{m}{i}=\sum_{i=0}^m\binom{n}{i}\binom{m}{m-i}=\binom{n+m}{m}
$$

Trong đó $\binom{n+m}{m}$ là số đường đi trên lưới quen thuộc. Do đó có thể chứng minh bằng ý nghĩa tổ hợp.

Trên một lưới, từ $(0,0)$ đến $(n,m)$ đi $n+m$ bước. Quy ước $(0,0)$ ở góc trên trái, đi xuống $n$ bước, sang phải $m$ bước, số cách là $\binom{n+m}{m}$.

Góc nhìn khác: tách $n+m$ bước thành hai phần, đi $n$ bước rồi đi $m$ bước. Trong $n$ bước có $i$ bước sang phải thì $m$ bước có $m-i$ bước sang phải, chứng minh xong.

## Bài tập

-   [CF785D Anton and School - 2](https://codeforces.com/problemset/problem/785/D)

-   [洛谷 P2791 幼儿园篮球题](https://www.luogu.com.cn/problem/P2791)

## Tài liệu tham khảo và chú thích

1.  [Vandermonde's Convolution Formula](https://www.cut-the-knot.org/arithmetic/algebra/VandermondeConvolution.shtml)
