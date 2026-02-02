Bài này giới thiệu sự khác biệt giữa C++ và các ngôn ngữ phổ biến khác, tập trung vào khác biệt giữa C và C++ (quan trọng hoặc dễ bỏ qua). Dù C++ gần như là siêu tập của C, trộn C/C++ thường không vấn đề, nhưng hiểu các khác biệt quan trọng giúp tránh bug kỳ lạ. Nếu bạn dùng C là chính, bài này giúp chuyển sang C++ dễ hơn. Các tính năng C++ riêng có thể xem ở [C++ nâng cao](./class.md). Bài cũng tóm lược khác biệt giữa Python, Java và C++.

## Khác biệt giữa C và C++

### Macro và template

Một mục tiêu ban đầu của template C++ là thay macro. Học template là bước quan trọng từ C sang C++. Template khác macro vì không chỉ thay văn bản; tại thời điểm biên dịch, template có kiểm tra kiểu đầy đủ hơn, giúp code an toàn hơn. Từ C++11, template hỗ trợ tham số biến độ dài, có thể thay thế hàm biến độ dài trong C và vẫn an toàn kiểu.

### Con trỏ và tham chiếu

C++ vẫn có thể dùng con trỏ kiểu C, nhưng truyền biến khuyến nghị dùng [tham chiếu](./reference.md) của C++. Tham chiếu không thể rỗng, tránh lỗi truy cập null. Con trỏ vẫn hữu ích nhờ tính linh hoạt. Đáng chú ý, `NULL` trong C có thay thế an toàn kiểu là `nullptr` từ C++11. Tham chiếu và con trỏ có thể chuyển qua lại bằng [toán tử `*` và `&`](./op.md).

### bool

Xem thêm [kiểu bool](var.md#布尔类型).

Khác C++, C ban đầu không có bool.

C99 thêm `_Bool` (và macro tương đương `bool`), cùng macro `true` và `false`. Muốn dùng `bool`/`true`/`false` cần `stdbool.h`. Dùng `_Bool` thì không cần.

```c
bool x = true;  // cần stdbool.h
_Bool x = 1;    // không cần stdbool.h
```

Từ C23, `true`,`false`,`bool` thành từ khóa, không cần `stdbool.h`, đồng thời giữ `_Bool` là cách viết thay thế `bool`[^boolean-keyword].

Bảng sau tóm tắt thay đổi hỗ trợ bool trong C (so sánh với C++):

| Chuẩn ngôn ngữ | `bool`                            | `true`/`false`                                        | `_Bool`                   |
| ------------ | --------------------------------- | ----------------------------------------------------- | ------------------------- |
| C89          | /                                 | /                                                     | giữ lại[^reserved-identifiers] |
| C99 đến trước C23 | Macro, tương đương `_Bool`, cần `stdbool.h` | Macro, `true` = `1`, `false` = `0`, cần `stdbool.h` | Từ khóa                   |
| Từ C23        | Từ khóa                            | Từ khóa                                               | Cách viết thay thế của `bool` |
| C++          | Từ khóa                            | Từ khóa                                               | giữ lại[^reserved-identifiers] |

### struct

C và C++ đều có struct, nhưng **không thể trộn lẫn**! Trong C, struct mô tả bố cục bộ nhớ cố định. Trong C++, struct là một lớp, **khác lớp chỉ ở chỗ mặc định là `public`** (lớp mặc định là `private`). Điều này rất quan trọng khi viết code C/C++ lẫn nhau.

Ngoài ra, khai báo struct trong C++ đơn giản hơn. C:

```c
typedef struct Node_t {
  struct Node_t *next;
  int key;
} Node;
```

C++:

```cpp
struct Node {
  Node *next;
  int key;
};
```

### const

Trong C, const chỉ giới hạn không cho sửa. Trong C++, const có nhiều ý nghĩa hơn. Người kế nhiệm const trong C ở C++ là constexpr; chi tiết const xem [hằng](./const.md).

### Cấp phát bộ nhớ

C++ thêm `new` và `delete` để cấp phát vùng “free store”, có thể là heap hoặc static, phục vụ lớp. `delete[]` còn giải phóng mảng động. `new`/`delete` gọi constructor/destructor, hỗ trợ kiểu tốt hơn `malloc()`/`realloc()`/`free()` nhưng chậm hơn.

Nói ngắn gọn: nếu cấp phát kiểu cơ bản hoặc mảng, dùng `malloc()` hiệu quả hơn; nếu cấp phát đối tượng không cơ bản, nên dùng `new` để an toàn. Lưu ý: `new` trả về con trỏ chỉ được thu hồi bằng `delete`, còn `malloc()` chỉ bằng `free()`, nếu không sẽ rò rỉ.

### Khai báo biến

Trước C99, C yêu cầu khai báo biến ở đầu khối; C++ và C99 trở đi không còn hạn chế này.

### Mảng độ dài biến

C99 hỗ trợ VLA (mảng độ dài biến), C++ không hỗ trợ.

### Khởi tạo struct

C99 hỗ trợ [khởi tạo chỉ định](https://en.cppreference.com/w/c/language/struct_initialization) (C11 là tùy chọn), C++ đến C++20 mới hỗ trợ dạng có thứ tự, và các đặc tính C như thứ tự lộn xộn, lồng nhau, trộn với khởi tạo thường, khởi tạo chỉ định cho mảng đều không được C++ hỗ trợ[^cpp-designated-init].

### Cú pháp chú thích

Chú thích dòng `//` kiểu C++ không được C hỗ trợ trước C99.

## Khác biệt giữa Python và C++

Python rất phổ biến trong ML. So với C++, Python dễ học và dễ thực hành. Cú pháp đơn giản, không cần khai báo kiểu. Nhưng cái giá là hiệu năng. C++ nhanh hơn và dùng được trên nhiều nền tảng (kể cả nhúng). C++ gần tầng thấp nên có thể dùng để viết hệ điều hành.

## Khác biệt giữa Java và C++

Java và C++ đều hướng đối tượng (đóng gói, kế thừa, đa hình), nên tái sử dụng tốt. So với Python, Java gần C++ hơn.

Khác biệt lớn nhất là cơ chế JVM. JVM (Java Virtual Machine) giúp Java độc lập nền tảng. Ngôn ngữ bậc cao khác muốn chạy đa nền tảng phải biên dịch riêng. Với JVM, Java chỉ cần sinh bytecode chạy trên JVM, không cần sửa khi đổi nền tảng.

Vì vậy Java thường dùng khi cần đa nền tảng. Nhưng do biên dịch sang bytecode và chạy trên JVM, Java kém hiệu năng hơn C++.

## Tài liệu tham khảo

[^cpp-designated-init]: <https://en.cppreference.com/w/cpp/language/aggregate_initialization>

[^boolean-keyword]: <https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3054.pdf>．

[^reserved-identifiers]: C và C++ quy định định danh bắt đầu bằng dấu gạch dưới và chữ hoa là tên dành riêng, xem <https://en.cppreference.com/w/c/language/identifier>．
