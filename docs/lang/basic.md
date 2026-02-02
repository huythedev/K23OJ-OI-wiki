## Khung mã

Nếu bạn không muốn đi sâu nguyên lý, lúc mới học có thể thuộc lòng “khung” này:

```cpp
#include <cstdio>
#include <iostream>

int main() {
  // làm gì đó...
  return 0;
}
```

??? note "include là gì?"
    `#include` thực chất là một lệnh tiền xử lý, nghĩa là chèn nội dung của một tệp vào vị trí câu lệnh này. Tệp được “chèn” được gọi là header. Nói cách khác, khi biên dịch, trình biên dịch sẽ “sao chép” nội dung của header `iostream` và “dán” vào vị trí câu lệnh `#include <iostream>`. Nhờ vậy, bạn có thể dùng các đối tượng `std::cin`、`std::cout`、`std::endl` do `iostream` cung cấp.
    
    Nếu bạn học C, sẽ thấy header trong C++ thường không có hậu tố `.h`, còn các header C kiểu `xx.h` sẽ thành `cxx`, ví dụ `stdio.h` thành `cstdio`. Vì C++ tương thích với C nên vẫn dùng các header C, nhưng thêm tiền tố `c` để phân biệt.
    
    Nói chung, hãy `#include` đúng những header mà chương trình cần. Nếu `#include` thừa, chỉ tăng thời gian biên dịch, hầu như không ảnh hưởng thời gian chạy. Hiện ta chỉ dùng `iostream` và `cstdio`; nếu chỉ cần `scanf` và `printf` thì không cần `#include <iostream>`.
    
    Có thể `#include` header tự viết không? Có.
    
    Bạn có thể tự viết một header, ví dụ `myheader.h`, đặt cùng thư mục với code, rồi `#include "myheader.h"`. Lưu ý: header tự viết dùng dấu ngoặc kép, không dùng ngoặc nhọn. Hoặc bạn có thể dùng tùy chọn biên dịch `-I <header_file_path>` để báo cho trình biên dịch nơi tìm header, khi đó không cần đặt cùng thư mục.

??? note "main() là gì?"
    Có thể hiểu khi chương trình chạy, nó sẽ thực thi code trong `main()`.
    
    Thực tế, hàm `main` do hệ thống hoặc chương trình bên ngoài gọi. Ví dụ bạn gọi chương trình ở dòng lệnh thì tức là gọi `main` (trước đó sẽ hoàn tất khởi tạo [biến](./var.md) toàn cục).
    
    Dòng `return 0;` cuối biểu thị chương trình chạy thành công. Mặc định, kết thúc với 0 là bình thường, giá trị khác 0 là mã lỗi (trên Windows có thể tra mã lỗi ở [Windows Error Codes](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/)). Giá trị này trả về cho hệ thống hoặc chương trình gọi. Nếu không viết `return`, mặc định vẫn trả 0.
    
    Trong C/C++, chương trình trả về giá trị khác 0 sẽ gây lỗi chạy (RE).

## Chú thích

Trong C++, có hai kiểu chú thích:

1.  Chú thích dòng

    Bắt đầu bằng `//`, toàn bộ phần sau trên cùng dòng là chú thích.

2.  Khối chú thích

    Bắt đầu `/*`, kết thúc `*/`, phần giữa là chú thích, có thể nhiều dòng.

Chú thích không ảnh hưởng chạy, dùng để giải thích ý nghĩa chương trình, hoặc vô hiệu hóa một đoạn code (nhưng vẫn giữ trong file nguồn).

Trong phát triển dự án, chú thích giúp bảo trì và đọc hiểu.

Trong OI, ít người viết nhiều chú thích, nhưng chú thích giúp làm rõ ý tưởng khi viết, và giúp ôn tập. Khi viết bài giải, tutorial, chú thích vừa đủ sẽ giúp người đọc hiểu ý đồ code. Hy vọng mọi người có thói quen viết chú thích tốt.

## Nhập và xuất

### `cin` và `cout`

```cpp
#include <iostream>

int main() {
  int x, y;                          // khai báo biến
  std::cin >> x >> y;                // đọc x và y
  std::cout << y << std::endl << x;  // in y, xuống dòng, rồi in x
  return 0;                          // kết thúc hàm main
}
```

???+ note "Biến là gì?"
    Có thể tham khảo trang [biến](./var.md).

???+ note "std là gì?"
    std là **không gian tên** (namespace) của thư viện chuẩn C++.
    
    Về namespace xem thêm tại [namespace](./namespace.md).

### `scanf` và `printf`

`scanf` và `printf` là hàm của C. Thường chúng nhanh hơn `cin`/`cout`, và dễ kiểm soát định dạng.

???+ note "Tối ưu nhập xuất"
    Khác biệt `cin`/`cout` và `scanf`/`printf` cũng như tối ưu I/O, xem trang [tối ưu nhập xuất](../contest/io.md).

```cpp
#include <cstdio>

int main() {
  int x, y;
  scanf("%d%d", &x, &y);   // đọc x và y
  printf("%d\n%d", y, x);  // in y, xuống dòng, rồi in x
  return 0;
}
```

Trong đó `%d` biểu thị biến kiểu số nguyên có dấu (`int`).

Tương tự:

1.  `%s` là chuỗi.
2.  `%c` là ký tự.
3.  `%lf` là số thực double.
4.  `%lld` là số nguyên dài (`long long`), tùy hệ có thể là `%I64d`.
5.  `%u` là số nguyên không dấu (`unsigned int`).
6.  `%llu` là số nguyên dài không dấu (`unsigned long long`), có thể là `%I64u`.

Ngoài ký hiệu kiểu, còn các cách điều khiển định dạng. Hai cách phổ biến:

1.  `%1d` biểu thị độ dài 1. Khi đọc, dù không có khoảng trắng vẫn đọc từng chữ số. Khi in, nếu độ rộng lớn hơn số chữ số thì sẽ đệm khoảng trắng; nhỏ hơn thì không hiệu lực.
2.  `%.6lf` dùng khi in, giữ 6 chữ số thập phân.

Hai toán tử này có thể thay bằng số khác, ví dụ `%.3lf` là giữ 3 chữ số.

??? note "“Số thực double”, “số nguyên dài” là gì"
    Đây là kiểu dữ liệu; sẽ nói ở [biến](./var.md).

??? note "Vì sao `scanf` có toán tử `&`?"
    Ở đây `&` là toán tử lấy địa chỉ, trả về địa chỉ của biến. `scanf` nhận địa chỉ. Chi tiết xem [con trỏ](./pointer.md), hiện chỉ cần nhớ.

??? note "`\n` là gì?"
    `\n` là **ký tự escape**, nghĩa là xuống dòng.
    
    Ký tự escape dùng để biểu diễn ký tự không thể gõ trực tiếp, như xuống dòng trong chuỗi, dấu nháy có ý nghĩa đặc biệt, hoặc dấu gạch chéo ngược.
    
    Các escape phổ biến:
    
    1.  `\t` là tab.
    
    2.  `\\` là `\`.
    
    3.  `\"` là `"`.
    
    4.  `\0` là ký tự rỗng, kết thúc chuỗi kiểu C.
    
    5.  `\r` là carriage return. Trên Linux, xuống dòng là `\n`, Windows là `\r\n`. Trong OI, khi cần xuống dòng dùng `\n` là đủ. Nhưng khi đọc từng ký tự, cần chú ý vì ký tự xuống dòng có thể gây vấn đề. Ví dụ `gets` coi `\n` là kết thúc chuỗi; nếu xuống dòng là `\r\n` thì `\r` sẽ còn ở cuối chuỗi.
    
    6.  Đặc biệt, `%%` biểu thị ký tự `%`, chỉ dùng trong `printf` hoặc `scanf`. Ở chuỗi khác, chỉ cần `%`.
    
    ??? note "Literal là gì?"
        “Literal” là đoạn code trực tiếp biểu diễn một giá trị, ví dụ `3` là literal kiểu `int`, `'c'` là literal kiểu `char`. `"hello world"` cũng là literal chuỗi.
        
        Literal vô cớ, không giải thích, còn gọi là “magic number”, không được khuyến khích nếu code cần người đọc.

## Một số nội dung mở rộng

### Ký tự trắng trong C++

Trong C++, mọi ký tự trắng (dấu cách, tab, xuống dòng) đều được xem như nhau (trừ trong chuỗi).

Vì vậy bạn có thể dùng bất kỳ phong cách mã nào (trừ chú thích dòng, chuỗi literal và lệnh tiền xử lý phải nằm trên một dòng), ví dụ:

```cpp
--8<-- "docs/lang/code/basic/basic_1.cpp:main"
```

Tất nhiên không khuyến khích.

Một phong cách khác phổ biến nhưng không giống yêu cầu của **OI Wiki**:

```cpp
--8<-- "docs/lang/code/basic/basic_2.cpp:main"
```

### Lệnh `#define`

`#define` là lệnh tiền xử lý để định nghĩa macro, bản chất là thay thế văn bản. Ví dụ:

```cpp
#include <iostream>
#define n 233

// n không phải biến, mà trình biên dịch sẽ thay mọi văn bản n thành 233,
// nhưng nếu n là một phần của định danh thì không bị thay, ví dụ fn không thành f233,
// tương tự, trong chuỗi cũng không thay.

int main() {
  std::cout << n;  // in 233
  return 0;
}
```

??? note "Định danh là gì?"
    Định danh là chuỗi ký tự dùng làm tên biến. Ví dụ `abcd` và `abc1` hợp lệ, còn `1a` và `c+b` không hợp lệ.
    
    Định danh bắt đầu bằng chữ cái hoặc `_`, sau đó chỉ được dùng chữ cái, `_` và chữ số. Lưu ý: từ khóa (`int`,`for`,`if`) không thể làm định danh.

??? note "Lệnh tiền xử lý là gì?"
    Lệnh tiền xử lý là lệnh mà bộ tiền xử lý nhận để biến đổi văn bản sơ bộ, ví dụ `#include` và `#define`. Với GCC, mặc định không giữ output giai đoạn tiền xử lý `.i`; có thể dùng `-E`.

Macro có thể có tham số, dùng như hàm:

```cpp
#include <iostream>
#define sum(x, y) ((x) + (y))
#define square(x) ((x) * (x))

int main() {
  std::cout << sum(1, 2) << ' ' << 2 * sum(3, 5) << std::endl;  // in 3 16
}
```

Nhưng macro có tham số khác hàm, vì là thay thế văn bản nên gây lỗi. Ví dụ:

```cpp
#include <iostream>
#define sum(x, y) x + y
// nên viết: #define sum(x, y) ((x) + (y))
#define square(x) ((x) * (x))

int main() {
  std::cout << sum(1, 2) << ' ' << 2 * sum(3, 5) << std::endl;
  // in 3 11, vì #define chỉ thay văn bản, câu lệnh thành 2 * 3 + 5
  int i = 1;
  std::cout << square(++i) << ' ' << i;
  // kết quả không xác định vì ++i bị thực hiện hai lần
  // và trong cùng một biểu thức, sửa cùng biến nhiều lần là undefined (có ngoại lệ)
}
```

Dùng `#define` có rủi ro (phạm vi toàn chương trình, dễ thay nhầm, cần `#undef`), nên thận trọng. Khuyến nghị: dùng `const` để khai báo hằng, dùng hàm thay macro.

Tuy vậy, trong OI, `#define` vẫn có chỗ dùng (hai cách sau không khuyến khích vì giảm tính chuẩn):

1.  `#define int long long` + `signed main()` để tránh quên `long long` khi debug (có thể tăng hằng số thời gian, TLE, hoặc MLE).
2.  `#define For(i, l, r) for (int i = (l); i <= (r); ++i)`、`#define pb push_back`、`#define mid ((l + r) / 2)` để rút ngắn code.

Ngoài ra, `#define` kết hợp `#ifdef` rất hữu ích, ví dụ:

```cpp
#ifdef LINUX
// code cho Linux
#else
// code cho OS khác
#endif
```

Khi biên dịch có thể dùng `-DLINUX` để điều khiển code mà không cần sửa file. Ưu điểm: file thực thi không chứa code cho OS khác vì đã bị tiền xử lý loại bỏ.

`#define` còn hỗ trợ toán tử `#`、`##`, rất tiện cho debug.
