author: aofall, greyqz, Ir1d, Link-cute, Marcythm, ouuan, Shen-Linwood, sshwy, StudyingFather

## Toán tử số học

| Toán tử       | Chức năng |
| --------- | --- |
|  `+` （đơn ngôi） | Dương   |
|  `-` （đơn ngôi） | Âm   |
|  `*` （nhị phân） | Nhân  |
|  `/`      | Chia  |
|  `%`      | Lấy dư  |
|  `+` （nhị phân） | Cộng  |
|  `-` （nhị phân） | Trừ  |

??? note "Toán tử đơn ngôi và nhị phân"
    Toán tử đơn ngôi (còn gọi là toán tử một ngôi) là toán tử chỉ có một toán hạng, còn toán tử nhị phân (còn gọi là toán tử hai ngôi) có hai toán hạng. Ví dụ trong `1 + 2` thì dấu cộng là toán tử nhị phân, có hai toán hạng `1` và `2`. Ngoài ra trong C++ còn có duy nhất một toán tử ba ngôi là `?:`.

Trong các toán tử số học có hai toán tử đơn ngôi (dương, âm) và năm toán tử nhị phân (nhân, chia, lấy dư, cộng, trừ), trong đó toán tử đơn ngôi có độ ưu tiên cao nhất.

Toán tử lấy dư `%` dùng để lấy phần dư khi chia hai số nguyên.

Còn `-` khi là toán tử nhị phân thì là phép trừ, như `2-1`; khi là toán tử đơn ngôi thì là lấy đối (âm), như `-1`.

Cách dùng như sau:

 `op=x-y*z` 

Giá trị của `op` tuân theo quy tắc ưu tiên của cộng trừ nhân chia trong toán học: thực hiện phép có độ ưu tiên cao trước, cùng độ ưu tiên thì theo tính kết hợp, ngoặc sẽ nâng độ ưu tiên.

### Chuyển đổi kiểu trong phép toán số học

Với toán tử số học nhị phân, khi hai biến tham gia có cùng kiểu thì không xảy ra [chuyển đổi kiểu](./var.md#类型转换), kết quả sẽ dùng kiểu đó để biểu diễn; nếu khác kiểu thì sẽ xảy ra chuyển đổi kiểu để hai biến có cùng kiểu. Quy tắc chuyển đổi xem tại [chuyển đổi kiểu](./var.md#类型转换).

Ví dụ, với một biến số nguyên (`int`) $x$ và một biến số thực kép (`double`) $y$:

-  `x/3` cho kết quả kiểu số nguyên;
-  `x/3.0` cho kết quả kiểu số thực kép;
-  `x/y` cho kết quả kiểu số thực kép;
-  `x*1/3` cho kết quả kiểu số nguyên;
-  `x*1.0/3` cho kết quả kiểu số thực kép;

## Toán tử bit

Xem thêm: [Phép toán bit](../math/bit.md#位运算).

| Toán tử       | Chức năng   |
| --------- | ---- |
|  `~`      | NOT theo bit  |
|  `&` （nhị phân） | AND theo bit  |
|  `|`      | OR theo bit  |
|  `^`      | XOR theo bit |
|  `<<`     | Dịch trái theo bit |
|  `>>`     | Dịch phải theo bit |

Ý nghĩa các phép toán bit xem tại [phép toán bit](../math/bit.md). Cần lưu ý độ ưu tiên của phép toán bit thấp hơn toán tử số học (trừ phép NOT), và AND/OR/XOR theo bit còn thấp hơn toán tử so sánh (xem [Bảng độ ưu tiên toán tử C++](#c-运算符优先级总表)), nên khi dùng cần chú ý, khi cần thì thêm ngoặc.

Trong phép dịch bit, nếu xuất hiện các tình huống sau thì hành vi là không xác định:

1.  Toán hạng bên phải (số bit dịch) là số âm;
2.  Toán hạng bên phải lớn hơn hoặc bằng số bit của toán hạng bên trái;

Ví dụ với biến `a` kiểu `int32_t`, `a<<-1` và `a<<32` đều là không xác định.

Với phép dịch trái trên số có dấu không âm, cần đảm bảo kết quả sau dịch có thể biểu diễn trong kiểu của số ban đầu, nếu không thì cũng là không xác định.[^note1] Dịch trái một số âm cũng là không xác định.[^note2]

Với phép dịch phải, các bit thừa bên phải sẽ bị bỏ, còn bên trái phức tạp hơn: với số không dấu, sẽ điền $0$[^note3]; còn với số có dấu thì điền bit cao nhất (tức bit dấu, số không âm là $0$, số âm là $1$)[^note4].

## Toán tử tăng/giảm

Đôi khi ta cần tăng biến lên 1 (tăng) hoặc giảm đi 1 (giảm), lúc này toán tử tăng `++` và giảm `--` sẽ hữu ích.

Toán tử tăng/giảm có thể đặt trước hoặc sau biến, trước là tiền tố, sau là hậu tố. Khi dùng riêng lẻ thì tiền/hậu tố không cần phân biệt; nếu dùng giá trị của biểu thức thì cần chú ý, xem ví dụ dưới đây. Chi tiết xem phần ví dụ trong [tham chiếu](./reference.md).

```cpp
i = 100;

op1 = i++;  // op1 = 100, trước tiên op1 = i, sau đó i = i + 1

i = 100;

op2 = ++i;  // op2 = 101, trước tiên i = i + 1, sau đó gán op2

i = 100;

op3 = i--;  // op3 = 100, trước tiên gán op3, sau đó i = i - 1

i = 100;

op4 = --i;  // op4 = 99, trước tiên i = i - 1, sau đó gán op4
```

## Toán tử gán kết hợp

Toán tử gán kết hợp thực chất là dạng rút gọn của biểu thức. Có thể chia thành toán tử số học kết hợp `+=`、`-=`、`*=`、`/=`、`%=` và toán tử bit kết hợp `&=`、`|=`、`^=`、`<<=`、`>>=`.

Ví dụ, `op = op + 2` có thể viết `op += 2`, `op = op - 2` viết `op -= 2`, `op= op * 2` viết `op *= 2`.

## Toán tử điều kiện

Toán tử điều kiện có thể xem là dạng rút gọn của câu lệnh `if`, trong `a ? b : c` nếu biểu thức `a` đúng thì kết quả là `b`, ngược lại là `c`.
## Toán tử so sánh

| Toán tử    | Chức năng   |
| ------ | ---- |
|  `>`   | Lớn hơn   |
|  `>=`  | Lớn hơn hoặc bằng |
|  `<`   | Nhỏ hơn   |
|  `<=`  | Nhỏ hơn hoặc bằng |
|  `==`  | Bằng  |
|  `!=`  | Không bằng  |

Đặc biệt cần phân biệt toán tử so sánh bằng `==` với toán tử gán `=`; điều này rất quan trọng trong các câu lệnh điều kiện.

 `if (op=1)` và `if (op==1)` nhìn có vẻ giống nhau, nhưng chức năng khác hẳn. Câu lệnh đầu là gán giá trị cho op, nếu gán khác 0 thì là true, điều kiện luôn đúng, không còn tác dụng kiểm tra; còn câu lệnh thứ hai mới là so sánh giá trị của `op`.

## Toán tử logic

| Toán tử    | Chức năng  |
| ------ | --- |
|  `&&`  | AND logic |
|  `||`  | OR logic |
|  `!`   | NOT logic |

```cpp
Result = op1 && op2;  // Khi op1 và op2 đều đúng thì Result đúng

Result = op1 || op2;  // Khi op1 hoặc op2 đúng thì Result đúng

Result = !op1;  // Khi op1 sai thì Result đúng
```

Các toán tử **built-in** `&&` và `||` có đánh giá ngắn mạch (nếu sau khi đánh giá toán hạng đầu tiên đã biết kết quả thì không đánh giá toán hạng thứ hai), còn toán tử đã bị nạp chồng thì không có đặc tính này và luôn đánh giá cả hai toán hạng.

## Toán tử dấu phẩy

Toán tử dấu phẩy dùng để phân tách nhiều biểu thức, các biểu thức được tính từ trái sang phải, giá trị của toàn biểu thức là giá trị của biểu thức cuối. Độ ưu tiên của toán tử dấu phẩy là **thấp nhất** trong tất cả toán tử.

```cpp
exp1, exp2, exp3;  // Giá trị cuối cùng là kết quả của exp3.

Result = 1 + 2, 3 + 4, 5 + 6;
// Giá trị Result là 3 chứ không phải 11, vì toán tử gán "="
// có độ ưu tiên cao hơn toán tử dấu phẩy, nên gán trước rồi mới xử lý dấu phẩy.

Result = (1 + 2, 3 + 4, 5 + 6);

// Nếu muốn Result nhận giá trị của phép toán dấu phẩy thì cần bao toàn biểu thức bằng ngoặc
// để tăng độ ưu tiên, lúc này Result mới là 11.
```

## Toán tử truy cập thành viên

| Toán tử       | Chức năng       |
| --------- | -------- |
|  `[]`     | Chỉ số mảng     |
|  `.`      | Thành viên của đối tượng     |
|  `&` （đơn ngôi） | Lấy địa chỉ/lấy tham chiếu |
|  `*` （đơn ngôi） | Truy cập gián tiếp/giải tham chiếu |
|  `->`     | Thành viên qua con trỏ     |

Các toán tử này dùng để truy cập thành viên của đối tượng hoặc bộ nhớ; ngoài toán tử cuối cùng, các toán tử trên đều có thể nạp chồng. Nội dung liên quan đến `&`、`*` và `->` xem [con trỏ](./pointer.md) và [tham chiếu](./reference.md). Ở đây bỏ qua hai toán tử ít dùng `.*` và `->*`, cách dùng xem [C++ language manual](https://zh.cppreference.com/w/cpp/language/operator_member_access).

```cpp
auto result1 = v[1];  // Lấy phần tử có chỉ số 2 trong v
auto result2 = p.q;   // Lấy thành viên q của đối tượng p
auto result3 = p -> q;  // Lấy thành viên q của đối tượng mà con trỏ p trỏ tới, tương đương (*p).q
auto result4 = &v;      // Lấy con trỏ trỏ tới v
auto result5 = *v;      // Lấy đối tượng mà con trỏ v trỏ tới
```

## Bảng tổng hợp độ ưu tiên toán tử C++

Trích từ [C++ operator precedence - cppreference](https://zh.cppreference.com/w/cpp/language/operator_precedence), có chỉnh sửa.

|          Toán tử         |    Mô tả    |                              Ví dụ                              | Khả năng nạp chồng |
| :------------------: | :------: | :----------------------------------------------------------: | :--: |
|       **Mức 1**       |          |                                                              |      |
|         `::`         |  Toán tử phân giải phạm vi  |                       `Class::age = 2;`                      | Không nạp chồng |
|       **Mức 2**       |          |                                                              |      |
|         `++`         |  Hậu tố tăng  |           `for (int i = 0; i < 10; i++) cout << i;`          |  Có thể nạp chồng |
|         `--`         |  Hậu tố giảm  |           `for (int i = 10; i > 0; i--) cout << i;`          |  Có thể nạp chồng |
|   `type()  type{}`   |  Ép kiểu  |           `unsigned int a = unsigned(3.14);`                | Có thể nạp chồng |
|         `()`         |   Gọi hàm   |                        `isdigit('1')`                        |  Có thể nạp chồng |
|         `[]`         |  Truy cập mảng  |                        `array[4] = 2;`                       |  Có thể nạp chồng |
|          `.`         |  Truy cập thành viên đối tượng |                        `obj.age = 34;`                       | Không nạp chồng |
|         `->`         |  Truy cập thành viên qua con trỏ |                       `ptr->age = 34;`                       |  Có thể nạp chồng |
|   **Mức 3** （kết hợp từ phải sang trái）  |          |                                                              |      |
|         `++`         |  Tiền tố tăng  |             `for (i = 0; i < 10; ++i) cout << i;`            |  Có thể nạp chồng |
|         `--`         |  Tiền tố giảm  |             `for (i = 10; i > 0; --i) cout << i;`            |  Có thể nạp chồng |
|          `+`         |    Dấu dương    |                         `int i = +1;`                        |  Có thể nạp chồng |
|          `-`         |    Dấu âm    |                         `int i = -1;`                        |  Có thể nạp chồng |
|          `!`         |   Phủ định logic   |                        `if (!done) …`                       |  Có thể nạp chồng |
|          `~`         |   Phủ định theo bit   |                       `flags = ~flags;`                      |  Có thể nạp chồng |
|       `(type)`       |  Ép kiểu C  |                 `int i = (int) floatNum;`             |  Có thể nạp chồng |
|          `*`         |   Giải tham chiếu   |                     `int data = *intPtr;`                    |  Có thể nạp chồng |
|          `&`         |   Lấy địa chỉ   |                    `int *intPtr = &data;`                    |  Có thể nạp chồng |
|       `sizeof`       |  Kích thước bộ nhớ  |    `int size = sizeof floatNum; int size = sizeof(float);`   | Không nạp chồng |
|         `new`        | Cấp phát bộ nhớ động |  `long *pVar = new long; MyClass *ptr = new MyClass(args);`  |  Có thể nạp chồng |
|       `new []`       | Cấp phát mảng động |                 `long *array = new long[n];`                 |  Có thể nạp chồng |
|       `delete`       | Giải phóng phần tử động |                        `delete pVar;`                        |  Có thể nạp chồng |
|      `delete []`     | Giải phóng mảng động |                      `delete [] array;`                      |  Có thể nạp chồng |
|       **Mức 4**    |          |                                                              |      |
|         `.*`         |  Truy cập thành viên qua con trỏ thành viên (đối tượng) |                       `obj.*var = 24;`                       | Không nạp chồng |
|         `->*`        |  Truy cập thành viên qua con trỏ thành viên (con trỏ) |                       `ptr->*var = 24;`                      |  Có thể nạp chồng |
|       **Mức 5**    |          |                                                              |      |
|          `*`         |    Nhân    |                       `int i = 2 * 4;`                       |  Có thể nạp chồng |
|          `/`         |    Chia    |                    `float f = 10.0 / 3.0;`                   |  Có thể nạp chồng |
|          `%`         | Lấy dư (mod) |                      `int rem = 4 % 3;`                      |  Có thể nạp chồng |
|       **Mức 6**    |          |                                                              |      |
|          `+`         |    Cộng    |                       `int i = 2 + 3;`                       |  Có thể nạp chồng |
|          `-`         |    Trừ    |                       `int i = 5 - 1;`                       |  Có thể nạp chồng |
|       **Mức 7**    |          |                                                              |      |
|         `<<`         |    Dịch trái theo bit   |                    `int flags = 33 << 1;`                    |  Có thể nạp chồng |
|         `>>`         |    Dịch phải theo bit   |                    `int flags = 33 >> 1;`                    |  Có thể nạp chồng |
|       **Mức 8**     |          |                                                              |      |
|         `<=>`         | Toán tử so sánh ba chiều  |                `if ((i <=> 42) < 0) ...`                      |  Có thể nạp chồng |
|       **Mức 9**     |          |                                                              |      |
|          `<`         |    Nhỏ hơn    |                      `if (i < 42) ...`                      |  Có thể nạp chồng |
|         `<=`         |   Nhỏ hơn hoặc bằng   |                      `if (i <= 42) ...`                     |  Có thể nạp chồng |
|          `>`         |    Lớn hơn    |                      `if (i > 42) ...`                      |  Có thể nạp chồng |
|         `>=`         |   Lớn hơn hoặc bằng   |                      `if (i >= 42) ...`                     |  Có thể nạp chồng |
|       **Mức 10**       |          |                                                              |      |
|         `==`         |    Bằng    |                      `if (i == 42) ...`                     |  Có thể nạp chồng |
|         `!=`         |    Không bằng   |                      `if (i != 42) ...`                     |  Có thể nạp chồng |
|       **Mức 11**      |          |                                                              |      |
|          `&`         |   AND theo bit   |                     `flags = flags & 42;`                    |  Có thể nạp chồng |
|       **Mức 12**      |          |                                                              |      |
|          `^`         |   XOR theo bit  |                     `flags = flags ^ 42;`                    |  Có thể nạp chồng |
|       **Mức 13**      |          |                                                              |      |
|          `|`         |   OR theo bit   |                     `flags = flags | 42;`                    |  Có thể nạp chồng |
|       **Mức 14**      |          |                                                              |      |
|         `&&`         |   AND logic  |              `if (conditionA && conditionB) ...`             |  Có thể nạp chồng |
|   **Mức 15**         |          |                                                              |      |
|         `||`         |   OR logic  |              `if (conditionA || conditionB) ...`             |  Có thể nạp chồng |
|   **Mức 16** （kết hợp từ phải sang trái） |          |                                                              |      |
|         `? :`        |   Toán tử điều kiện  |                   `int i = a > b ? a : b;`                   | Không nạp chồng |
|        `throw`       |   Ném ngoại lệ   |                  `throw EClass("Message");`                  | Không nạp chồng |
|          `=`         |    Gán    |                         `int a = b;`                         |  Có thể nạp chồng |
|         `+=`         |   Cộng gán  |                           `a += 3;`                          |  Có thể nạp chồng |
|         `-=`         |   Trừ gán  |                           `b -= 4;`                          |  Có thể nạp chồng |
|         `*=`         |   Nhân gán  |                           `a *= 5;`                          |  Có thể nạp chồng |
|         `/=`         |   Chia gán  |                           `a /= 2;`                          |  Có thể nạp chồng |
|         `%=`         |   Lấy dư gán  |                           `a %= 3;`                          |  Có thể nạp chồng |
|         `<<=`        |  Dịch trái gán |                        `flags <<= 2;`                        |  Có thể nạp chồng |
|         `>>=`        |  Dịch phải gán |                        `flags >>= 2;`                        |  Có thể nạp chồng |
|         `&=`         |  AND bit gán  |                     `flags &= new_flags;`                    |  Có thể nạp chồng |
|         `^=`         |  XOR bit gán |                     `flags ^= new_flags;`                    |  Có thể nạp chồng |
|         `|=`         |  OR bit gán  |                     `flags |= new_flags;`                    |  Có thể nạp chồng |
|       **Mức 17**      |          |                                                              |      |
|          `,`         |   Dấu phẩy  |          `for (i = 0, j = 0; i < 10; i++, j++) ...`          |  Có thể nạp chồng |

Lưu ý bảng không liệt kê các toán tử `const_cast`, `static_cast`, `dynamic_cast`, `reinterpret_cast`, `typeid`, `sizeof...`, `noexcept` và `alignof` vì dạng dùng của chúng giống lời gọi hàm nên không gây nhập nhằng.

## Tài liệu tham khảo và chú thích

[^note1]: Trước C++20, nếu giá trị ban đầu là kiểu có dấu và kết quả sau dịch có thể biểu diễn bằng phiên bản không dấu của kiểu đó, thì kết quả sẽ được [chuyển đổi](../lang/var.md#类型转换) sang kiểu có dấu tương ứng, nếu không thì hành vi không xác định; với số không dấu, dịch trái sẽ bỏ các bit tràn khỏi kiểu kết quả. Từ C++20, quy định `a << b` là $a\cdot 2^b$ theo modulo $2^N$ (với $N$ là độ rộng bit của kiểu kết quả), tức dù có dấu hay không dấu, dịch trái đều bỏ các bit tràn (tức [dịch trái số học/logic](../math/bit.md#移位)).

[^note2]: Trước C++20. Hành vi từ C++20 xem [^note1].

[^note3]: Tức [dịch phải logic](../math/bit.md#移位).

[^note4]: Tức [dịch phải số học](../math/bit.md#移位). Trước C++20, dịch phải số có dấu là phụ thuộc hiện thực, đa số hiện thực dùng dịch phải số học. Từ C++20, quy định `a >> b` là $\lfloor a/2^b\rfloor$, nên dịch phải số có dấu là dịch phải số học.
