Thao tác bit là các phép toán một ngôi và hai ngôi trên biểu diễn nhị phân của số nguyên, chia thành **phép toán bit** và **dịch bit**．Đây là nhóm phép toán cơ bản nhất của CPU, tốc độ thường rất nhanh．

## Số nguyên và dãy bit

Tham khảo thêm: [Kiểu số nguyên](../lang/var.md#整数类型)

Chúng ta gọi một dãy độ dài cố định chỉ gồm `0` hoặc `1` là dãy bit．Bit ngoài cùng bên trái gọi là bit cao nhất, bit ngoài cùng bên phải gọi là bit thấp nhất．

Máy tính dùng dãy bit để biểu diễn số nguyên trong một phạm vi nhất định．Với độ dài $N$, dãy bit chỉ có $2^N$ khả năng, nên chỉ có thể thiết lập quan hệ một-một với $2^N$ số nguyên．Quan hệ một-một này chia làm hai loại: **có dấu** và **không dấu**．Có dấu nghĩa là tập số tương ứng có số âm, không dấu nghĩa là tất cả đều không âm．

-   Với quan hệ **không dấu**, ta có thể trực tiếp dùng biểu diễn nhị phân của số nguyên làm dãy bit, thiếu độ dài thì thêm `0` ở bit cao．

    Trong quan hệ không dấu, dãy bit độ dài $N$ biểu diễn các số trong $[0,2^N-1]$．

-   Với quan hệ **có dấu**, có hai quy tắc biểu diễn: **mã bù 1** (ones' complement) và **mã bù 2** (two's complement)．

    Với số không âm, quy tắc biểu diễn giống không dấu; với số âm, lấy dãy bit ứng với trị tuyệt đối rồi **đảo bit** (đổi `0` thành `1`, `1` thành `0`) gọi là mã bù 1; lấy mã bù 1 theo quan hệ không dấu đổi sang số nguyên rồi cộng một, cuối cùng đổi về dãy bit theo quan hệ không dấu, bỏ phần vượt quá độ dài ban đầu, thu được mã bù 2．

    Trong quan hệ mã bù 1, dãy bit độ dài $N$ biểu diễn các số trong $[-2^{N-1}+1,2^{N-1}-1]$．

    Trong quan hệ mã bù 2, dãy bit độ dài $N$ biểu diễn các số trong $[-2^{N-1},2^{N-1}-1]$．

Lấy dãy bit $3$ bit làm ví dụ:

| Dãy bit | Số không dấu | Số có dấu (mã bù 1) | Số có dấu (mã bù 2) |
| ----- | ----- | --------- | --------- |
| `000` | $0$   | $0$       | $0$       |
| `001` | $1$   | $1$       | $1$       |
| `010` | $2$   | $2$       | $2$       |
| `011` | $3$   | $3$       | $3$       |
| `100` | $4$   | $-3$      | $-4$      |
| `101` | $5$   | $-2$      | $-3$      |
| `110` | $6$   | $-1$      | $-2$      |
| `111` | $7$   | $-0$      | $-1$      |

Có thể thấy vấn đề lớn nhất của mã bù 1 là xuất hiện $-0$ (không tồn tại trong thực tế), vì vậy thông thường ta chỉ dùng mã bù 2．Do khi biểu diễn số có dấu, dấu của số chỉ do bit cao nhất quyết định, nên bit này gọi là **bit dấu**．

Đổi dãy bit sang số nguyên cũng khá dễ: với số không âm thì không cần xử lý đặc biệt; với mã bù 1 thì đảo bit để lấy số đối; với mã bù 2 thì đảo bit rồi cộng một để lấy số đối．

## Phép toán bit

Phép toán bit là việc áp dụng từng bit một số [hàm Boolean](./boolean-algebra.md#布尔函数)．Về hình thức, với hàm Boolean $f:\mathbf{B}^k\to \mathbf{B}$, phép toán bit là hàm dạng

$$
\begin{aligned}
    F:\left(\mathbf{B}^m\right)^k&\to \mathbf{B}^m\\
    ((p_{1,1},\dots,p_{m,1}),\dots,(p_{1,k},\dots,p_{m,k}))&\mapsto (f(p_{1,1},\dots,p_{1,k}),\dots,f(p_{m,1},\dots,p_{m,k}))
\end{aligned}
$$

trong đó $m$ là độ dài dãy bit．Tương tự, ta thường chỉ nghiên cứu phép toán bit một ngôi và hai ngôi．Nếu không nói rõ, các phép toán bit dưới đây đều chỉ xét một ngôi và hai ngôi．

Thông thường, ta coi **đảo bit**, **AND**, **OR**, **XOR** là các phép toán bit cơ bản, còn các phép khác đều có thể tổ hợp từ chúng．

| Phép toán bit | Ký hiệu toán học | Hàm Boolean tương ứng | Toán tử C++ | Diễn giải |
| ---- | ----------------------------- | -------- | --------------- | ----------------------- |
| Đảo bit | $\operatorname{NOT}$          | $\lnot$  | `~`             | Đổi $0$ thành $1$, $1$ thành $0$ |
| AND | $\operatorname{AND}$          | $\land$  | `&`             | Chỉ khi cả hai bit đều $1$ thì là $1$ |
| OR | $\operatorname{OR}$           | $\lor$   | <code>\|</code> | Chỉ cần một trong hai bit là $1$ thì là $1$ |
| XOR | $\oplus$、$\operatorname{XOR}$ | $\oplus$ | `^`             | Chỉ khi hai bit khác nhau thì là $1$ |

???+ warning "Warning"
    Lưu ý phân biệt phép toán bit và hàm Boolean．

Ví dụ:

-   $\operatorname{NOT} 01010111=10101000$，
-   $01010011 \operatorname{AND} 00110010=00010010$，
-   $01010011 \operatorname{OR}  00110010=01110011$，
-   $01010011 \operatorname{XOR} 00110010=01100001$．

Do bốn phép toán trên thao tác độc lập trên từng bit, nên chúng trực tiếp kế thừa các tính chất của hàm Boolean tương ứng．

Để tiện, khi độ dài dãy bit đã biết, ta cũng có thể thao tác trực tiếp trên số nguyên, ví dụ:

$$
\begin{aligned}
    \operatorname{NOT} 5&=-6,\\
    \operatorname{NOT} (-5)&=4,\\
    5 \operatorname{AND} 6 &=4,\\
    5 \operatorname{OR} 6 &=7,\\
    5 \operatorname{XOR} 6 &=3.
\end{aligned}
$$

Giả sử $x,y\geq 0$, ta cũng có thể biểu diễn phép toán bit dưới dạng tổng:

$$
\begin{aligned}
    \operatorname{NOT} x&=\sum_{n=0}^{\lfloor\log_{2}x\rfloor}2^n\left(\left(\left\lfloor\frac{x}{2^n}\right\rfloor\bmod 2+1\right)\bmod 2\right)\\
    &=\sum_{n=0}^{\lfloor\log_{2}x\rfloor}\left(2^{\left\lfloor\log_{2}x\right\rfloor +1}-1-x\right)\\
    x\operatorname{AND} y&=\sum_{n=0}^{\lfloor\log_{2}\max\{x,y\}\rfloor}2^n\left(\left\lfloor\frac{x}{2^n}\right\rfloor\bmod 2\right)\left(\left\lfloor{\frac{y}{2^n}}\right\rfloor\bmod 2\right)\\
    x\operatorname{OR} y&=\sum_{n=0}^{\lfloor\log_{2}\max\{x,y\}\rfloor}2^n\left(\left(\left\lfloor\frac{x}{2^n}\right\rfloor\bmod 2\right)+\left(\left\lfloor{\frac{y}{2^n}}\right\rfloor\bmod 2\right)-\left(\left\lfloor\frac{x}{2^n}\right\rfloor\bmod 2\right)\left(\left\lfloor{\frac{y}{2^n}}\right\rfloor\bmod 2\right)\right)\\
    x\operatorname{XOR} y&=\sum_{n=0}^{\lfloor\log_{2}\max\{x,y\}\rfloor}2^n\left(\left(\left(\left\lfloor\frac{x}{2^n}\right\rfloor\bmod 2\right)+\left(\left\lfloor{\frac{y}{2^n}}\right\rfloor\bmod 2\right)\right)\bmod 2\right)\\
    &=\sum_{n=0}^{\lfloor\log_{2}\max\{x,y\}\rfloor}2^n\left(\left(\left\lfloor\frac{x}{2^n}\right\rfloor +\left\lfloor\frac{y}{2^n}\right\rfloor\right)\bmod 2\right)
\end{aligned}
$$

Khi không gây nhầm lẫn, phần sau sẽ lược bỏ chữ “theo bit”．

## Dịch bit

Tham khảo thêm: [Toán tử bit trong C++](../lang/op.md#位操作符)．

Dịch bit là phép toán hai ngôi dịch dãy bit sang trái hoặc phải, tham số thứ nhất là dãy bit, tham số thứ hai thường là số nguyên không âm．Dịch sang trái gọi là **dịch trái**, dịch sang phải gọi là **dịch phải**．Tùy cách điền các bit trống sau khi dịch, có thể chia thành **dịch số học**, **dịch логic**, **dịch vòng**．Trong đó

-   Dịch логic điền 0 vào chỗ trống，
-   Dịch phải số học điền bit dấu vào chỗ trống; dịch trái số học giống dịch trái логic，
-   Dịch vòng điền các bit tràn vào chỗ trống．

Ví dụ với dãy 8 bit `10 01 01 10`：

| Thao tác       | Kết quả         |
| ---------- | ------------- |
| Dịch trái số học 2 bit | `01 01 10 00` |
| Dịch phải số học 2 bit | `11 10 01 01` |
| Dịch trái логic 2 bit | `01 01 10 00` |
| Dịch phải логic 2 bit | `00 10 01 01` |
| Dịch vòng trái 2 bit | `01 01 10 10` |
| Dịch vòng phải 2 bit | `10 10 01 01` |

Trong C++, dùng `a << b` cho dịch trái, `a >> b` cho dịch phải; quy tắc cụ thể xem [Toán tử bit trong C++](../lang/op.md#位操作符)．

Ta có thể dùng đoạn code sau để реализовать dịch vòng:

???+ note "实现"
    ```cpp
    --8<-- "docs/math/code/bit/bit_1.cpp:core"
    ```

## Ứng dụng của thao tác bit

Thao tác bit thường có ba tác dụng:

1.  Thực hiện một số phép tính hiệu quả, thay cho cách làm kém hiệu quả．Tham khảo [Tối ưu biên dịch #Giảm cường độ](../lang/optimizations.md#强度削减-strength-reduction)．
2.  [Biểu diễn tập hợp](./binary-set.md)（thường dùng trong [Quy hoạch động trạng thái](../dp/state.md)）．
3.  Bài toán yêu cầu thao tác bit．

Cần lưu ý, thay phép toán khác bằng thao tác bit nhiều khi không mang lại tối ưu rõ rệt, ngược lại làm code phức tạp hơn; cần cân nhắc khi dùng．

### Ứng dụng liên quan lũy thừa của 2

Do thao tác bit dựa trên biểu diễn nhị phân, có nhiều ứng dụng liên quan đến lũy thừa nguyên của 2．

Nhân (chia) một số với lũy thừa nguyên không âm của 2：

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:mul"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:mul"
    ```

??? warning "Warning"
    Phép chia thường dùng là làm tròn về $0$, còn dịch phải là làm tròn xuống (lưu ý khác biệt này): khi số $\ge 0$ thì hai cách tương đương, khi số < 0 thì khác, ví dụ: `-1 / 2` có giá trị $0$ nhưng `-1 >> 1` có giá trị $-1$．

### Lấy trị tuyệt đối

Trên một số máy, nhanh hơn `n > 0 ? n : -n`．

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:abs"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:abs"
    ```

### Lấy max/min của hai số

Trên một số máy, nhanh hơn `a > b ? a : b`．

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:minmax"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:minmax"
    ```

### Kiểm tra hai số khác 0 có cùng dấu hay không

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:sgn"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:sgn"
    ```

### Hoán đổi hai số

???+ note "Phương pháp này có hạn chế"
    Cách này chỉ dùng để hoán đổi hai số nguyên, phạm vi sử dụng hạn chế．
    
    Với hoán đổi thông thường, khuyến nghị dùng `std::swap` trong `algorithm`．

```cpp
--8<-- "docs/math/code/bit/bit_2.cpp:swap"
```

### Thao tác trên bit nhị phân của một số

Lấy một bit của một số:

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:get_bit"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:get_bit"
    ```

Đặt một bit về $0$:

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:unset_bit"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:unset_bit"
    ```

Đặt một bit về $1$:

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:set_bit"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:set_bit"
    ```

Đảo một bit:

=== "C++"
    ```cpp
    --8<-- "docs/math/code/bit/bit_2.cpp:flap_bit"
    ```

=== "Python"
    ```python
    --8<-- "docs/math/code/bit/bit_2.py:flap_bit"
    ```

Các thao tác này tương đương coi biến int 32-bit như một mảng boolean độ dài 32．

## Trọng số Hamming

Trọng số Hamming là số lượng ký hiệu khác ký hiệu zero trong một chuỗi ký hiệu (trên bộ ký tự dùng)．Với số nhị phân, trọng số Hamming bằng số lượng bit `1` (tức `popcount`)．

Cách tính trọng số Hamming bằng vòng lặp: liên tục bỏ bit thấp nhất (dịch phải 1 bit), duy trì biến đáp án, mỗi lần cập nhật tùy theo bit thấp nhất có là `1` hay không．

Code như sau:

```cpp
--8<-- "docs/math/code/bit/bit_2.cpp:popcnt1"
```

Có thể dùng thao tác `lowbit`: liên tục trừ đi `lowbit`[^note1] cho tới khi số bằng 0．

Code như sau:

```cpp
--8<-- "docs/math/code/bit/bit_2.cpp:popcnt2"
```

### Xây dựng hoán vị tăng theo trọng số Hamming

Trong [Quy hoạch động trạng thái](../dp/state.md), đôi khi duyệt theo thứ tự popcount tăng để tránh duyệt trạng thái lặp．Đây là một ứng dụng quan trọng của việc xây dựng hoán vị tăng theo trọng số Hamming．

Dưới đây ta khảo sát cách xây dựng hoán vị tăng theo trọng số Hamming trong $O(n)$．

Ta biết, số nhỏ nhất có trọng số Hamming bằng $n$ là $2^n-1$．Nếu có thể trong $O(1)$ tìm “kế tiếp” có cùng trọng số Hamming, ta có thể duyệt theo trọng số từ $2^n-1$ để xây dựng hoán vị của $0\sim n$ trong $O(n)$．

Cách tìm phần tử kế tiếp có cùng trọng số Hamming với $x$ có thể hiểu như sau, lấy $(10110)_2$ làm ví dụ:

-   Dịch bit `1` ở phải nhất sang trái, nếu không dịch được thì thử bit `1` bên trái, tiếp tục như vậy, được $(11010)_2$．
-   Đưa tất cả bit `1` từ vị trí vừa dịch đến bit thấp nhất về phía phải nhất．Với ví dụ, bit `1` vừa dịch ở vị trí thứ 3, nên ba bit cuối `010` trở thành `001`, được $(11001)_2$．

Quá trình này có thể tối ưu bằng thao tác bit:

```cpp
--8<-- "docs/math/code/bit/bit_3.cpp:hamming1"
```

-   Bước đầu, ta cộng $x$ với `lowbit` của nó, trong nhị phân tương đương thay đoạn liên tiếp `1` bên phải bằng một `1` bên trái của nó．Ví dụ $(10110)_2$ cộng `lowbit` thành $(11000)_2$．Đây là nửa trước của đáp án．
-   Ta cần bổ sung các bit `1` ở cuối: `lowbit(t)` là vị trí sau khi bit `1` cao nhất trong đoạn liên tiếp dịch sang trái; `lowbit(x)` là vị trí thấp nhất của đoạn liên tiếp đó．Ví dụ $t=(11000)_2$，$\operatorname{lowbit}(t)=(01000)_2$，$\operatorname{lowbit}(x)=(00010)_2$．
-   Phép chia tiếp theo là phần khó nhất nhưng quan trọng: giả sử đoạn `1` liên tiếp của **số gốc** có bit cao nhất ở vị trí $r$ (đánh số từ 0), bit thấp nhất ở vị trí $l$；`lowbit(t)=1<<(r+1)`，`lowbit(x)=1<<l`，khi đó `(((t&-t)/(x&-x))>>1)` = `(1<<(r+1))/(1<<l)/2 = 1<<(r-l)`，trong nhị phân là `1` kèm $r-l$ số 0, đúng bằng số lượng `1` liên tiếp trừ 1．Ví dụ: $\frac{\operatorname{lowbit(t)/2}}{\operatorname{lowbit(x)}} = \frac{(00100)_2}{(00010)_2} = (00010)_2$．Trừ 1 sẽ ra phần cần bổ sung, OR với nửa trước sẽ ra đáp án．

Vì vậy, code đầy đủ để liệt kê $0\sim n$ theo trọng số Hamming tăng là:

```cpp
--8<-- "docs/math/code/bit/bit_3.cpp:hamming2_begin"
--8<-- "docs/math/code/bit/bit_3.cpp:hamming2_end"
```

Lưu ý cần xử lý riêng trường hợp $0$ vì $0$ không có phần tử kế tiếp cùng trọng số Hamming．

## Lớp và hàm liên quan trong C++

### Hàm dựng sẵn của GCC

GCC có một số hàm dựng sẵn cho thao tác bit:

-   `int __builtin_ffs(int x)`：trả về vị trí bit `1` thấp nhất (tính từ 1), nếu $x=0$ trả 0．
-   `int __builtin_clz(unsigned int x)`：số lượng `0` ở đầu (leading zeros), nếu $x=0$ thì không xác định．
-   `int __builtin_ctz(unsigned int x)`：số lượng `0` liên tiếp ở cuối, nếu $x=0$ thì không xác định．
-   `int __builtin_clrsb(int x)`：nếu bit dấu là 0, trả số lượng `0` đầu trừ 1; ngược lại trả số lượng `1` đầu trừ 1．
-   `int __builtin_popcount(unsigned int x)`：số lượng bit `1`．
-   `int __builtin_parity(unsigned int x)`：tính chẵn lẻ của số bit `1`．

Có thể thêm `l` hoặc `ll` để đổi kiểu tham số thành (`unsigned`)`long` hoặc (`unsigned`)`long long` (giá trị trả về vẫn là `int`)．
Ví dụ, muốn lấy $\log_2$ của một số (không xét $0$), tương đương số bit của biểu diễn nhị phân trừ 1．Với số nguyên `N` bit, số bit của `n` là `N - __builtin_clz(n)`，nên `N - 1 - __builtin_clz(n)` là $\log_2 n$．

Các hàm này là built-in nên được tối ưu rất tốt, tốc độ rất nhanh (thậm chí chỉ một lệnh)．

### Nhiều bit hơn

Nếu cần thao tác dãy bit rất dài, có thể dùng [`std::bitset`](../lang/csl/bitset.md)．

## Đề bài gợi ý

-   [Luogu P1225 黑白棋游戏](https://www.luogu.com.cn/problem/P1225)

## Tài liệu tham khảo và chú thích

1.  [Bit hacks](https://graphics.stanford.edu/~seander/bithacks.html)
2.  [Bit Operation Builtins (Using the GNU Compiler Collection (GCC))](https://gcc.gnu.org/onlinedocs/gcc/Bit-Operation-Builtins.html)
3.  [Bitwise operation - Wikipedia](https://en.wikipedia.org/wiki/Bitwise_operation)

[^note1]: `lowbit` của một số là bit `1` thấp nhất kèm theo các `0` phía sau, ví dụ $(1010)_2$ có `lowbit` là $(0010)_2$，xem chi tiết ở [Fenwick Tree](../ds/fenwick.md)．
