## Kiểu dữ liệu

Hệ thống kiểu của C++ gồm các phần sau:

1.  Kiểu cơ bản (trong ngoặc là từ khóa/kiểu đại diện)
    1.  Kiểu vô loại/`void` (`void`)
    2.  (Từ C++11) kiểu con trỏ rỗng (`std::nullptr_t`)
    3.  Kiểu số học
        1.  Kiểu số nguyên (`int`)
        2.  Kiểu bool (`bool`)
        3.  Kiểu ký tự (`char`)
        4.  Kiểu số thực (`float`,`double`)
2.  Kiểu phức hợp[^note11]

### Kiểu bool

Một biến kiểu `bool` chỉ có thể là `true` hoặc `false`.

Thông thường, một biến `bool` chiếm $1$ byte (thường $1$ byte = $8$ bit).

???+ tip "Mẹo"
    Có thể dùng macro `CHAR_BIT` trong `<climits>`(C++)/`<limits.h>`(C) để biết số bit mỗi byte.

???+ note "Kiểu bool trong C"
    Xem thêm [Khác biệt giữa C++ và các ngôn ngữ thường dùng - bool](./cpp-other-langs.md#bool).
    
    Ban đầu C không có kiểu bool; đến C99 mới thêm từ khóa `_Bool` làm kiểu bool, được coi là kiểu số nguyên không dấu.
    
    ???+ note "Ghi chú"
        Từ C23, kiểu `bool` không còn dùng quy ước 0/khác 0, mà là kiểu đủ để chứa hai hằng `true` và `false`.
    
    Để tiện dùng, `stdbool.h` cung cấp 3 macro `bool`,`true`,`false` như sau:
    
    ```c
    #define bool _Bool
    #define true 1
    #define false 0
    ```
    
    Các macro này bị loại bỏ trong C23, và từ C23 đưa `true`,`false`,`bool` thành từ khóa, đồng thời vẫn giữ `_Bool` như một cách viết thay thế[^note10].
    
    Ngoài ra, từ C23 còn có thể dùng macro `BOOL_WIDTH` trong `<limits.h>` để lấy độ rộng bit của kiểu bool.

### Kiểu số nguyên

Dùng để lưu số nguyên. Kiểu số nguyên cơ bản nhất là `int`.

???+ warning "Lưu ý"
    Do lịch sử, trong C++ kiểu bool và kiểu ký tự được xem là các kiểu số nguyên đặc biệt.
    
    Trong hầu hết mọi trường hợp, **không nên** dùng các kiểu ký tự (trừ `signed char` và `unsigned char`) như kiểu số nguyên.

Các kiểu số nguyên thường có 5 mức theo độ rộng: `char`,`short`,`int`,`long`,`long long`.

C++ đảm bảo `1 == sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`

Do lịch sử, độ rộng bit của số nguyên có nhiều mô hình phổ biến; để giải quyết, C99/C++11 đưa ra [kiểu số nguyên định rộng](#定宽整数类型).

???+ note "Kích thước kiểu `int`"
    Chuẩn C++ quy định `int` có **ít nhất** $16$ bit.
    
    Thực tế, trên đa số nền tảng hiện đại, `int` là $32$ bit.

Với từ khóa `int`, có thể dùng các từ khóa sửa đổi sau:

Dấu:

-   `signed`: số nguyên có dấu (mặc định);
-   `unsigned`: số nguyên không dấu.

Kích thước:

-   `short`: **ít nhất** $16$ bit;
-   `long`: **ít nhất** $32$ bit;
-   (Từ C++11) `long long`: **ít nhất** $64$ bit.

Bảng sau là độ rộng và phạm vi trong **trường hợp phổ biến** (một số nền tảng có thể khác):

| Tên kiểu                                                                   | Kiểu tương đương              | Độ rộng (chuẩn C++) | Phổ biến | Hiếm gặp                        |
| --------------------------------------------------------------------- | ------------------------ | ---------- | ------ | -------------------------- |
| `signed char`                                                         | `signed char`            | $8$        | -      | -                          |
| `unsigned char`                                                       | `unsigned char`          | $8$        | -      | -                          |
| `short`,`short int`,`signed short`,`signed short int`                 | `short int`              | $\geq 16$  | $16$   | -                          |
| `unsigned short`,`unsigned short int`                                 | `unsigned short int`     | $\geq 16$  | $16$   | -                          |
| `int`,`signed`,`signed int`                                           | `int`                    | $\geq 16$  | $32$   | $16$ (thường gặp ở Win16 API) |
| `unsigned`,`unsigned int`                                             | `unsigned int`           | $\geq 16$  | $32$   | $16$ (thường gặp ở Win16 API) |
| `long`,`long int`,`signed long`,`signed long int`                     | `long int`               | $\geq 32$  | $32$   | $64$ (thường gặp ở Linux/macOS 64-bit) |
| `unsigned long`,`unsigned long int`                                   | `unsigned long int`      | $\geq 32$  | $32$   | $64$ (thường gặp ở Linux/macOS 64-bit) |
| `long long`,`long long int`,`signed long long`,`signed long long int` | `long long int`          | $\geq 64$  | $64$   | -                          |
| `unsigned long long`,`unsigned long long int`                         | `unsigned long long int` | $\geq 64$  | $64$   | -                          |

Với độ rộng $x$, phạm vi của kiểu có dấu là $-2^{x-1}\sim 2^{x-1}-1$[^note16], và kiểu không dấu là $0 \sim 2^x-1$. Cụ thể:

| Độ rộng | Phạm vi                                              |
| ---- | ------------------------------------------------- |
| $8$  | Có dấu: $-2^{7}\sim 2^{7}-1$, Không dấu: $0 \sim 2^{8}-1$    |
| $16$ | Có dấu: $-2^{15}\sim 2^{15}-1$, Không dấu: $0 \sim 2^{16}-1$ |
| $32$ | Có dấu: $-2^{31}\sim 2^{31}-1$, Không dấu: $0 \sim 2^{32}-1$ |
| $64$ | Có dấu: $-2^{63}\sim 2^{63}-1$, Không dấu: $0 \sim 2^{64}-1$ |

???+ note "Cách viết kiểu tương đương"
    Khi không gây nhập nhằng, có thể bỏ bớt từ khóa sửa đổi hoặc đổi thứ tự.
    
    Ví dụ `int`,`signed`,`int signed`,`signed int` là cùng một kiểu; `unsigned long` và `unsigned long int` là cùng một kiểu.

Một số trình biên dịch có kiểu số nguyên mở rộng, ví dụ GCC có số nguyên 128-bit: `__int128_t` (có dấu) và `__uint128_t` (không dấu). Nếu muốn dùng trong thi đấu, **hãy đọc kỹ quy định** để biết có được phép hay không.

???+ warning "Lưu ý"
    STL không nhất thiết hỗ trợ tốt kiểu số nguyên mở rộng, nên cần đặc biệt cẩn thận.
    
    ???+ note "Mã ví dụ"
        ```cpp
        #include <cmath>
        #include <iostream>
        
        int f1(int n) {
          return abs(n);  // Tốt
        }
        
        int f2(int n) {
          return std::abs(n);  // Tốt
        }
        
        __int128_t f3(__int128_t n) {
          return abs(n);  // Sai
        }
        
        // Sai
        // __int128_t f4(__int128_t n) {
        //   return std::abs(n);
        // }
        
        int main() {
          std::cout << "f1: " << f1(-42) << std::endl;
          std::cout << "f2: " << f2(-42) << std::endl;
          // std::cout << "f3: " << f3(-42) << std::endl; // Sai
          // std::cout << "f4: " << f4(-42) << std::endl; // Sai
          return 0;
        }
        ```
    
    Các vấn đề trong ví dụ:
    
    1.  `__int128_t f3(__int128_t)` dùng hàm trị tuyệt đối kiểu C với chữ ký `int abs(int)`, nên `n` bị ép về `int` trước rồi mới gọi `abs`.
    2.  `__int128_t f4(__int128_t)` dùng `std::abs` kiểu C++, nhưng không có overload `__int128_t std::abs(__int128_t)`, nên không biên dịch được.
    3.  I/O dạng stream của C++ không hỗ trợ `__int128_t` và `__uint128_t`.
    
    Một cách khắc phục:
    
    ??? note "Mã sửa"
        ```cpp
        #include <cmath>
        #include <iostream>
        
        __int128_t abs(__int128_t n) { return n < 0 ? -n : n; }
        
        std::ostream &operator<<(std::ostream &os, __uint128_t n) {
          if (n > 9) os << n / 10;
          os << (int)(n % 10);
          return os;
        }
        
        std::ostream &operator<<(std::ostream &os, __int128_t n) {
          if (n < 0) {
            os << '-';
            n = -n;
          }
          return os << (__uint128_t)n;
        }
        
        int f1(int n) { return abs(n); }
        
        int f2(int n) { return std::abs(n); }
        
        __int128_t f3(__int128_t n) { return abs(n); }
        
        int main() {
          std::cout << "f1: " << f1(-42) << std::endl;
          std::cout << "f2: " << f2(-42) << std::endl;
          std::cout << "f3: " << f3(-42) << std::endl;
        }
        ```

### Kiểu ký tự

Chia thành “ký tự hẹp” và “ký tự rộng”; do thi đấu thuật toán gần như không dùng ký tự rộng, nên chỉ giới thiệu ký tự hẹp.

Ký tự hẹp thường có $8$ bit; thực chất lưu trữ như số nguyên, thường dùng [ASCII](http://www.asciitable.com/) để ánh xạ ký tự ↔ số nguyên:

-   `signed char`: ký tự có dấu, phạm vi $-128 \sim 127$.
-   `unsigned char`: ký tự không dấu, phạm vi $0 \sim 255$.
-   `char` có cùng biểu diễn và căn chỉnh với một trong hai kiểu trên, nhưng là một kiểu độc lập.

    Dấu của `char` phụ thuộc trình biên dịch và nền tảng: ARM/PowerPC thường mặc định không dấu, x86/x64 thường có dấu.

    GCC có thể thêm `-fsigned-char` hoặc `-funsigned-char` để buộc `char` thành `signed char` hoặc `unsigned char`; trình biên dịch khác xem tài liệu. Lưu ý thay đổi dấu khác mặc định có thể phá ABI và làm chương trình không chạy đúng.

???+ warning "Lưu ý"
    Khác với các kiểu số nguyên khác, `char`、`signed char`、`unsigned char` là **ba kiểu khác nhau**.
    
    Thông thường `signed char`,`unsigned char` không nên dùng để lưu ký tự, đa số trường hợp chúng được xem như kiểu số nguyên.

### Kiểu số thực

Dùng để lưu “số thực” (thực ra là xấp xỉ theo quy tắc nhất định), gồm:

-   `float`: đơn chính xác. Nếu hỗ trợ thì khớp IEEE-754 binary32.
-   `double`: kép chính xác. Nếu hỗ trợ thì khớp IEEE-754 binary64.
-   `long double`: chính xác mở rộng. Nếu hỗ trợ thì khớp IEEE-754 binary128; nếu không thì có thể khớp binary64 mở rộng; nếu không nữa thì là một định dạng mở rộng không theo IEEE-754 nhưng tốt hơn/ít nhất ngang binary64; nếu không thì khớp binary64.

| Định dạng                 | Độ rộng  | Số dương lớn nhất              | Số chữ số chính xác |
| ---------------------- | --------- | -------------------------- | ---------------- |
| IEEE-754 binary32      | $32$      | $3.4\times 10^{38}$        | $6\sim 9$        |
| IEEE-754 binary64      | $64$      | $1.8\times 10^{308}$       | $15\sim 17$      |
| IEEE-754 binary64 mở rộng | $\geq 80$ | $\geq 1.2\times 10^{4932}$ | $\geq 18\sim 21$ |
| IEEE-754 binary128     | $128$     | $1.2\times 10^{4932}$      | $33\sim 36$      |

> Số âm nhỏ nhất (theo nghĩa giá trị) của IEEE-754 là đối của số dương lớn nhất.

Vì `float` có phạm vi nhỏ và độ chính xác thấp, thực tế thường dùng `double`.

Ngoài ra, số thực hỗ trợ các giá trị đặc biệt:

-   Vô cùng (dương/âm): `INFINITY`.
-   Âm 0: `-0.0`, ví dụ `1.0 / 0.0 == INFINITY`, `1.0 / -0.0 == -INFINITY`.
-   NaN (Not a Number): `std::nan`,`NAN`, thường sinh từ `0.0 / 0.0`… Nó không bằng bất kỳ giá trị nào (kể cả chính nó); từ C++11 có thể dùng `std::isnan` để kiểm tra.

### Kiểu vô loại

`void` là kiểu vô loại, không thể khai báo biến kiểu `void`. Tuy nhiên, hàm có thể trả về `void` để chỉ ra không có giá trị trả về.

### Kiểu con trỏ rỗng

Xem phần [con trỏ rỗng](./pointer.md#空指针).

## Kiểu số nguyên định rộng

Từ C++11 hỗ trợ số nguyên định rộng:

-   `<cstdint>`: cung cấp kiểu định rộng và macro max/min…
-   `<cinttypes>`: macro định dạng cho `std::fprintf`/`std::fscanf`.

Có các nhóm:

-   `intN_t`: có độ rộng **chính xác** $N$ bit, ví dụ `int32_t`.
-   `int_fastN_t`: có độ rộng **ít nhất** $N$ bit và **nhanh nhất**.
-   `int_leastN_t`: có độ rộng **ít nhất** $N$ bit và **nhỏ nhất**.

Bản không dấu thêm tiền tố `u`, ví dụ `uint32_t`,`uint_least8_t`.

Chuẩn yêu cầu 16 kiểu sau:

`int_fast8_t`,`int_fast16_t`,`int_fast32_t`,`int_fast64_t`,

`int_least8_t`,`int_least16_t`,`int_least32_t`,`int_least64_t`,

`uint_fast8_t`,`uint_fast16_t`,`uint_fast32_t`,`uint_fast64_t`,

`uint_least8_t`,`uint_least16_t`,`uint_least32_t`,`uint_least64_t`.

Đa số trình biên dịch còn có 8 kiểu:

`int8_t`,`int16_t`,`int32_t`,`int64_t`,

`uint8_t`,`uint16_t`,`uint32_t`,`uint64_t`.

Nếu có kiểu tương ứng, chuẩn yêu cầu macro max/min/bit-width theo dạng bỏ `_t`, viết hoa và thêm hậu tố:

-   `_MAX` là giá trị lớn nhất, ví dụ `INT32_MAX`.
-   `_MIN` là giá trị nhỏ nhất, ví dụ `INT32_MIN`.

???+ warning "Lưu ý"
    Kiểu định rộng là alias của kiểu nguyên thường, nên trộn lẫn có thể ảnh hưởng tính đa nền tảng, ví dụ:
    
    ???+ note "Mã ví dụ"
        ```cpp
        #include <algorithm>
        #include <cstdint>
        #include <iostream>
        
        int main() {
          long long a;
          int64_t b;
          std::cin >> a >> b;
          std::cout << std::max(a, b) << std::endl;
          return 0;
        }
        ```
    
    `int64_t` trên Windows 64-bit thường là `long long int`, còn trên Linux 64-bit thường là `long int`, nên đoạn code này sẽ không biên dịch trên GCC/Linux 64-bit, nhưng lại biên dịch được trên MSVC/Windows 64-bit vì `std::max` yêu cầu hai tham số cùng kiểu.

Ngoài ra, từ C++17, `<limits>` cung cấp `std::numeric_limits` để truy vấn thuộc tính kiểu số học: max/min, có dấu hay không, v.v.

```cpp
#include <cstdint>
#include <limits>

std::numeric_limits<int32_t>::max();  // Giá trị lớn nhất của int32_t, 2'147'483'647
std::numeric_limits<int32_t>::min();  // Giá trị nhỏ nhất của int32_t, -2'147'483'648

std::numeric_limits<double>::min();  // Giá trị nhỏ nhất của double, ~2.22507e-308
std::numeric_limits<double>::epsilon();  // Khoảng cách giữa 1.0 và giá trị kế tiếp của double,
                                         // ~2.22045e-16
```

## Chuyển đổi kiểu

Đôi khi ta cần chuyển một kiểu sang kiểu khác (ví dụ hàm nhận `int` nhưng truyền `double`). Cơ chế chuyển đổi kiểu trong C++ khá phức tạp; ở đây chỉ giới thiệu hai loại cho kiểu cơ bản: nâng kiểu và chuyển kiểu số.

### Nâng kiểu

Nâng kiểu giữ nguyên giá trị.

???+ note "Ghi chú"
    Biến tham số kiểu C với dấu `...` sẽ tự động nâng kiểu khi truyền. Ví dụ:
    
    ???+ note "Mã ví dụ"
        ```c
        #include <stdarg.h>
        #include <stdio.h>
        
        void test(int tot, ...) {
          va_list valist;
          int i;
        
          // Khởi tạo danh sách tham số biến đổi
          va_start(valist, tot);
        
          for (i = 0; i < tot; ++i) {
            // Lấy giá trị tham số thứ i
            double xx = va_arg(valist, double);  // Đúng
            // float xx = va_arg(valist, float); // Sai
        
            // In nội dung lưu trữ bên dưới của biến thứ i
            printf("i = %d, value = 0x%016llx\n", i, *(long long *)(&xx));
          }
        
          // Dọn danh sách tham số biến đổi
          va_end(valist);
        }
        
        int main() {
          float f;
          double fd, d;
          f = 123.;   // 0x42f60000
          fd = 123.;  // 0x405ec00000000000
          d = 456.;   // 0x407c800000000000
          test(3, f, fd, d);
        }
        ```
    
    Khi gọi `test`, `f` được nâng lên `double`, nên nội dung lưu trữ giống `fd`, kết quả là:
    
    ```text
    i = 0, value = 0x405ec00000000000
    i = 1, value = 0x405ec00000000000
    i = 2, value = 0x407c800000000000
    ```
    
    Nếu đổi `double xx = va_arg(valist, double);` thành `float xx = va_arg(valist, float);`, GCC thường cảnh báo:
    
    ```text
    In file included from test.c:2:
    test.c: In function ‘test’:
    test.c:14:35: warning: ‘float’ is promoted to ‘double’ when passed through ‘...’
      14 |         float xx = va_arg(valist, float);
         |                                   ^
    test.c:14:35: note: (so you should pass ‘double’ not ‘float’ to ‘va_arg’)
    test.c:14:35: note: if this code is reached, the program will abort
    ```
    
    Khi đó chương trình sẽ dừng trước khi in kết quả.
    
    Điều này cũng giải thích vì sao `%f` của `printf` có thể khớp cả `float` và `double`.

#### Nâng kiểu số nguyên

Các prvalue số nguyên nhỏ (như `char`) có thể nâng lên kiểu lớn hơn (như `int`).

Cụ thể, toán tử số học không nhận kiểu nhỏ hơn `int` làm toán hạng; sau chuyển lvalue-to-rvalue, nếu áp dụng được thì sẽ tự động nâng kiểu số nguyên.

Quy tắc:

-   `signed char`, `signed short / short` có thể nâng lên `int`.
-   `unsigned char`, `unsigned short` nếu `int` bao trùm được phạm vi nguồn thì nâng lên `int`, ngược lại nâng lên `unsigned int` (từ C++20, `char8_t` cũng theo quy tắc này).
-   `char` phụ thuộc kiểu nền tảng là `signed char` hay `unsigned char`.
-   `bool` nâng lên `int`: `false` → `0`, `true` → `1`.
-   Nếu kiểu đích bao trùm kiểu nguồn, và phạm vi nguồn không được `int`/`unsigned int` bao trùm, thì có thể nâng lên kiểu đích.[^note12]

???+ warning "Lưu ý"
    `char`->`short` không phải nâng kiểu số, vì `char` ưu tiên nâng lên `int`/`unsigned int`, rồi mới từ `int`/`unsigned int` -> `short`, không thỏa điều kiện nâng kiểu.

Ví dụ (giả sử `int` 32 bit, `unsigned short` 16 bit, `signed char`/`unsigned char` 8 bit, `bool` 1 bit):

-   `(signed char)'\0' - (signed char)'\xff'` nâng thành `(int)0` và `(int)-1`, kết quả `(int)1`.
-   `(unsigned char)'\0' - (unsigned char)'\xff'` nâng thành `(int)0` và `(int)255`, kết quả `(int)-255`.
-   `false - (unsigned short)12` nâng thành `(int)0` và `(int)12`, kết quả `(int)-12`.

#### Nâng kiểu số thực

Số thực nhỏ hơn có thể nâng lên số thực lớn hơn (ví dụ khi `float` và `double` tính toán, `float` được nâng lên `double`), giá trị không đổi.

### Chuyển đổi số

Chuyển đổi số có thể làm thay đổi giá trị.

???+ warning "Lưu ý"
    Nâng kiểu có ưu tiên cao hơn chuyển đổi số. Ví dụ `bool`->`int` là nâng kiểu chứ không phải chuyển đổi số.

#### Chuyển đổi số nguyên

<!-- scripts.linter.preprocess.fix_details off -->

-   Nếu kiểu đích là số nguyên không dấu độ rộng $x$, kết quả là giá trị gốc $\bmod 2^x$.

    -   Nếu độ rộng đích lớn hơn nguồn:

        -   Nếu nguồn có dấu, thường cần mở rộng bit dấu rồi chuyển đổi.

            Ví dụ:

            -   `(short)-1` (`(short)0b1111'1111'1111'1111`) sang `unsigned int`: mở rộng dấu thành `0b1111'1111'1111'1111'1111'1111'1111'1111`, rồi chuyển, kết quả `(unsigned int)4'294'967'295` (`0b1111'1111'1111'1111'1111'1111'1111'1111`).
            -   `(short)32'767` (`0b0111'1111'1111'1111`) sang `unsigned int`: mở rộng dấu thành `0b0000'0000'0000'0000'0111'1111'1111'1111`, kết quả `(unsigned int)32'767`.

        -   Nếu nguồn không dấu, cần mở rộng 0 rồi chuyển đổi.

            Ví dụ `(unsigned short)65'535` (`0b1111'1111'1111'1111`) sang `unsigned int`: mở rộng 0 thành `0b0000'0000'0000'0000'1111'1111'1111'1111`, kết quả `(unsigned int)65'535`.

    -   Nếu độ rộng đích không lớn hơn nguồn, cần cắt bớt rồi chuyển.

        Ví dụ `(unsigned int)4'294'967'295` (`0b1111'1111'1111'1111'1111'1111'1111'1111`) sang `unsigned short`: cắt còn `0b1111'1111'1111'1111`, kết quả `(unsigned short)65'535`.

-   Nếu kiểu đích là số nguyên có dấu độ rộng $x$, **thông thường** có thể coi kết quả là $\bmod 2^x$.[^note13]

    Ví dụ `(unsigned int)4'294'967'295` sang `short` cho `(short)-1` (`0b1111'1111'1111'1111`).

-   Nếu kiểu đích là `bool` thì là [chuyển đổi bool](#布尔转换).

-   Nếu nguồn là `bool`, `false` → 0, `true` → 1 trong kiểu đích.

<!-- scripts.linter.preprocess.fix_details on -->

#### Chuyển đổi số thực

Số thực lớn sang số thực nhỏ sẽ làm tròn về giá trị gần nhất trong kiểu đích.

#### Chuyển đổi giữa số thực và số nguyên

-   Số thực sang số nguyên: bỏ phần thập phân.

    Nếu đích là `bool`, thì là [chuyển đổi bool](#布尔转换).

-   Số nguyên sang số thực: làm tròn đến giá trị gần nhất trong kiểu đích.

    Nếu không biểu diễn được trong kiểu đích, hành vi không xác định.

    Nếu nguồn là `bool`, `false` → 0, `true` → 1.

#### Chuyển đổi bool

Chuyển kiểu khác sang `bool`: giá trị 0 → `false`, khác 0 → `true`.

## Định nghĩa biến

Nói đơn giản[^note14], định nghĩa biến cần có kiểu (type specifier) và tên biến.

Ví dụ:

```cpp
int oi;
double wiki;
char org = 'c';
```

Trong các đoạn chương trình thường gặp, biến định nghĩa trong dấu `{}` là biến cục bộ, còn ngoài `{}` là biến toàn cục. Có ngoại lệ, nhưng hiện chưa cần quan tâm.

Biến toàn cục không khởi tạo sẽ tự về 0. Biến cục bộ thì không, cần gán giá trị ban đầu nếu không dễ sinh bug.

## Phạm vi biến

Phạm vi là khối mã mà biến có hiệu lực.

Biến toàn cục có phạm vi từ nơi định nghĩa[^note15] đến hết file.

Biến cục bộ có phạm vi từ nơi định nghĩa đến hết khối lệnh.

Một khối lệnh là các câu lệnh được bao bởi `{}`.

```cpp
int g = 20;  // Định nghĩa biến toàn cục

int main() {
  int g = 10;         // Định nghĩa biến cục bộ
  printf("%d\n", g);  // In g
  return 0;
}
```

Nếu trong khối lồng nhau có biến trùng tên, biến ở khối trong sẽ che biến ở khối ngoài.

Ví dụ trên sẽ in ra $g = 10$. Vì vậy nên tránh đặt tên biến cục bộ trùng tên biến toàn cục.

## Hằng số

Hằng số là giá trị cố định, không thay đổi trong quá trình chạy.

Hằng số không thể bị sửa; thêm `const` khi định nghĩa.

```cpp
const int a = 2;
a = 3;
```

Nếu sửa hằng, trình biên dịch báo lỗi: `error: assignment of read-only variable‘a’`.

## Tài liệu tham khảo và chú thích

1.  [Working Draft, Standard for Programming Language C++](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/n4917.pdf)
2.  [Type - cppreference.com](https://zh.cppreference.com/w/cpp/language/type)
3.  [Arithmetic types (C) - cppreference.com](https://zh.cppreference.com/w/c/language/arithmetic_types)
4.  [Fundamental types - cppreference.com](https://zh.cppreference.com/w/cpp/language/types)
5.  [Fixed-width integer types (since C++11) - cppreference.com](https://zh.cppreference.com/w/cpp/types/integer)
6.  William Kahan (1 October 1997).["Lecture Notes on the Status of IEEE Standard 754 for Binary Floating-Point Arithmetic"](https://people.eecs.berkeley.edu/~wkahan/ieee754status/IEEE754.PDF).
7.  [Implicit conversions - cppreference.com](https://zh.cppreference.com/w/cpp/language/implicit_conversion)
8.  [Declarations - cppreference](https://zh.cppreference.com/w/cpp/language/declarations)
9.  [Scope - cppreference.com](https://zh.cppreference.com/w/cpp/language/scope)

[^note10]: Xem <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3054.pdf>

[^note11]: Gồm kiểu mảng, tham chiếu, con trỏ, lớp, hàm... Bài này dành cho người mới nên không trình bày chi tiết; xem [Type - cppreference.com](https://zh.cppreference.com/w/cpp/language/type)

[^note12]: Không bao gồm kiểu ký tự rộng, bit-field và enum; xem [Integral conversion - cppreference](https://zh.cppreference.com/w/cpp/language/implicit_conversion#.E6.95.B4.E5.9E.8B.E8.BD.AC.E6.8D.A2).

[^note13]: Có hiệu lực từ C++20. Trước C++20 là phụ thuộc hiện thực. Xem [Integral conversion - cppreference](https://zh.cppreference.com/w/cpp/language/implicit_conversion#.E6.95.B4.E5.9E.8B.E8.BD.AC.E6.8D.A2).

[^note14]: Khi định nghĩa biến, ngoài type specifier còn có thể có các specifier khác. Xem [Declarations - cppreference](https://zh.cppreference.com/w/cpp/language/declarations).

[^note15]: Cách nói chính xác hơn là [điểm khai báo](https://zh.cppreference.com/w/cpp/language/scope#.E5.A3.B0%E6%98%8E%E7%82%B9).

[^note16]: Trước C++20, số nguyên có dấu phải bao phủ phạm vi của [bù một](../math/bit.md#整数与位序列) (tức $-2^{x-1}+1\sim 2^{x-1}-1$), nhưng thực tế đa số dùng [bù hai](../math/bit.md#整数与位序列); từ C++20 yêu cầu bắt buộc dùng bù hai. Xem [Range of values - cppreference](https://en.cppreference.com/w/cpp/language/types.html#Range_of_values).
````
