author: sshwy

Mã Gray là một hệ nhị phân trong đó hai số kề nhau chỉ khác đúng một bit. Ví dụ, dãy mã Gray của số nhị phân $3$ bit là

$$
000,001,011,010,110,111,101,100
$$

Lưu ý chỉ số dãy bắt đầu từ $0$, tức là $G(0)=000,G(4)=110$.

Mã Gray được Frank Gray của Bell Labs đề xuất vào thập niên 1940, và được cấp bằng sáng chế năm 1953.

## Xây dựng mã Gray (biến đổi)

Có nhiều cách xây dựng mã Gray. Trước hết giới thiệu cách thủ công, sau đó đưa mã xây dựng và chứng minh đúng đắn.

### Xây dựng thủ công

Mã Gray $k$ bit có thể xây dựng như sau. Bắt đầu từ mã Gray toàn $0$, lặp theo chiến lược:

1.  Lật bit thấp nhất để được mã Gray kế tiếp (ví dụ $000\to 001$);
2.  Lật bit bên trái của bit $1$ ngoài cùng bên phải để được mã Gray kế tiếp (ví dụ $001\to 011$).

Luân phiên hai chiến lược trên tổng cộng $2^{k-1}$ lần, ta thu được dãy mã Gray $k$ bit.

### Xây dựng bằng phép đối xứng

Mã Gray $k$ bit có thể nhanh chóng tạo từ mã Gray $(k-1)$ bit bằng cách đối xứng trên–dưới rồi thêm một bit mới, như hình:

$$
\begin{matrix}
k=1\\
0\\ 1\\\\\\\\\\\\\\
\end{matrix}
\to \begin{matrix}\\
\color{Red}0\\\color{Red}1\\\color{Blue}1\\\color{Blue}0\\\\\\\\\\
\end{matrix}
\to \begin{matrix}
k=2\\
{\color{Red}0}0\\{\color{Red}0}1\\{\color{Blue}1}1\\{\color{Blue}1}0\\\\\\\\\\
\end{matrix}
\to \begin{matrix}\\
\color{Red}00\\\color{Red}01\\\color{Red}11\\\color{Red}10\\\color{Blue}10\\\color{Blue}11\\\color{Blue}01\\\color{Blue}00
\end{matrix}
\to \begin{matrix}
k=3\\
{\color{Red}0}00\\{\color{Red}0}01\\{\color{Red}0}11\\{\color{Red}0}10\\{\color{Blue}1}10\\{\color{Blue}1}11\\{\color{Blue}1}01\\{\color{Blue}1}00
\end{matrix}
$$

### Cách tính

Quan sát nhị phân của $n$ và $G(n)$. Ta thấy bit thứ $i$ của $G(n)$ là $1$ khi và chỉ khi bit thứ $i$ của $n$ là $1$, bit thứ $i+1$ là $0$, hoặc bit thứ $i$ là $0$, bit thứ $i+1$ là $1$. Do đó có thể coi là phép XOR:

$$
G(n)=n\oplus \left\lfloor\frac{n}{2}\right\rfloor
$$

```cpp
int g(int n) { return n ^ (n >> 1); }
```

### Chứng minh đúng đắn

Chứng minh rằng dãy mã Gray sinh bởi công thức trên có hai mã kề nhau chỉ khác đúng một bit.

Xét sự khác nhau giữa $n$ và $n+1$. Cộng $1$ vào $n$ tương đương với việc đảo tất cả các bit $1$ liên tiếp ở cuối, rồi đổi bit $0$ thấp nhất thành $1$. Ta biểu diễn:

$$
\begin{aligned}
(n)_2 &= \cdots0\underbrace{11\cdots11}_{k\text{个}}\\
(n+1)_2 &= \cdots1\underbrace{00\cdots00}_{k\text{个}}
\end{aligned}
$$

Khi tính $g(n)$ và $g(n+1)$, $k$ bit cuối đều trở thành dạng $\displaystyle\underbrace{100\cdots00}_{k\text{个}}$, còn bit thứ $k+1$ thì khác nhau vì ngoài $k+1$ bit cuối, $n$ và $n+1$ giống hệt. Do đó bit thứ $k+1$ hoặc cùng XOR với $1$, hoặc cùng XOR với $0$, và kết quả là khác nhau. Các bit còn lại đều XOR như nhau nên giống nhau.

Chứng minh xong.

## Khôi phục số ban đầu từ mã Gray (biến đổi ngược)

Xét biến đổi ngược: cho mã Gray $g$ tìm số ban đầu $n$. Duyệt từ bit cao xuống thấp (bit thấp nhất là $1$, bit cao nhất là $k$). Quan hệ giữa bit thứ $i$ của $n$ và $g_i$ như sau:

$$
\begin{aligned}
n_k &= g_k \\
n_{k-1} &= g_{k-1} \oplus n_k &&= g_k \oplus g_{k-1} \\
n_{k-2} &= g_{k-2} \oplus n_{k-1} &&= g_k \oplus g_{k-1} \oplus g_{k-2} \\
n_{k-3} &= g_{k-3} \oplus n_{k-2} &&= g_k \oplus g_{k-1} \oplus g_{k-2} \oplus g_{k-3} \\
&\vdots\\
n_{k-i} &=\displaystyle\bigoplus_{j=0}^ig_{k-j}
\end{aligned}
$$

```cpp
int rev_g(int g) {
  int n = 0;
  for (; g; g >>= 1) n ^= g;
  return n;
}
```

## Ứng dụng thực tế

Mã Gray có nhiều ứng dụng hữu ích, đôi khi rất bất ngờ:

-   Dãy mã Gray của các số nhị phân $k$ bit có thể xem là một chu trình Hamilton trên các đỉnh của một siêu lập phương $k$ chiều (hình vuông ở 2D, vectơ đơn vị ở 1D), trong đó mỗi bit là một tọa độ chiều.

-   Mã Gray được dùng để giảm lỗi trong truyền tín hiệu của bộ chuyển đổi số–tương tự (ví dụ cảm biến), vì mỗi lần chỉ đổi một bit.

-   Mã Gray có thể dùng để giải bài toán Tháp Hà Nội.

    Gọi số đĩa là $n$. Bắt đầu từ mã Gray $n$ bit toàn $0$ là $G(0)$, lần lượt chuyển sang mã kế tiếp ($G(i)$ sang $G(i+1)$). Bit thứ $i$ của mã Gray hiện tại biểu diễn đĩa thứ $i$ theo thứ tự từ nhỏ đến lớn.

    Vì mỗi lần chỉ có một bit đổi, khi bit thứ $i$ đổi thì ta di chuyển đĩa thứ $i$. Khi di chuyển đĩa, trừ đĩa nhỏ nhất, mỗi đĩa khác chỉ có một lựa chọn đặt. Riêng đĩa nhỏ nhất luôn có hai lựa chọn. Do đó chiến lược:

    Nếu $n$ lẻ, đường đi là $f\to t\to r\to f\to t\to r\to\cdots$, trong đó $f$ là cọc ban đầu, $t$ là cọc đích, $r$ là cọc trung gian.

    Nếu $n$ chẵn: $f \to r \to t \to f \to r \to t \to \cdots$.

-   Mã Gray cũng được ứng dụng trong lý thuyết thuật toán di truyền.

## Bài tập

-   [CSP S2 2019 D1T1](https://www.luogu.com.cn/problem/P5657) Difficulty: easy

-   [SGU #249 Matrix](http://codeforces.com/problemsets/acmsguru/problem/99999/249) Difficulty: medium

> Một phần nội dung trang này dịch từ bài [Код Грея](http://e-maxx.ru/algo/gray_code) và bản dịch tiếng Anh [Gray code](https://cp-algorithms.com/algebra/gray-code.html). Bản tiếng Nga theo giấy phép Public Domain + Leave a Link; bản tiếng Anh theo giấy phép CC-BY-SA 4.0.
