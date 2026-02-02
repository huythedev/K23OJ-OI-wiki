author: tsagaanbar, Enter-tainer, Xeonacid

## Địa chỉ biến, con trỏ

Trong chương trình, dữ liệu đều có địa chỉ lưu trữ. Mỗi lần chạy, vị trí vật lý trong RAM có thể khác nhau. Nhưng ta vẫn có thể lấy địa chỉ bằng câu lệnh.

Địa chỉ cũng là dữ liệu. Kiểu biến lưu địa chỉ có tên đặc biệt là “biến con trỏ”, thường gọi tắt là “con trỏ”.

???+ note "Kích thước con trỏ"
    Kích thước con trỏ khác nhau theo môi trường. 32-bit thì địa chỉ là số 32-bit, con trỏ 4 byte. 64-bit thì con trỏ 8 byte.

Địa chỉ chỉ là một số, nhưng để chỉ dữ liệu khác kiểu, con trỏ cũng có kiểu tương ứng. Ví dụ con trỏ `int` chứa địa chỉ của vùng 32-bit; con trỏ `char` chứa địa chỉ vùng 8-bit.

Người dùng cũng có thể khai báo con trỏ trỏ tới con trỏ.

Giả sử có struct:

```cpp
struct ThreeInt {
  int a;
  int b;
  int c;
};
```

Con trỏ `ThreeInt` trỏ tới vùng 3 × 32 = 96 bit.

## Khai báo và sử dụng con trỏ

Trong C/C++, kiểu con trỏ là kiểu + `*`. Ví dụ `int*`.

Dùng `&` để lấy địa chỉ.

Muốn truy cập vùng mà con trỏ trỏ tới cần **giải tham chiếu** (dereference) bằng `*`.

```cpp
int main() {
  int a = 123;  // a: 123
  int* pa = &a;
  *pa = 321;  // a: 321
}
```

Tương tự với struct. Truy cập thành viên cần giải tham chiếu rồi dùng `.`, hoặc dùng `->` cho gọn.

```cpp
struct ThreeInt {
  int a;
  int b;
  int c;
};

int main() {
  ThreeInt x{1, 2, 3}, y{6, 7, 8};
  ThreeInt* px = &x;
  (*px) = y;    // x: {6,7,8}
  (*px).a = 4;  // x: {4,7,8}
  px->b = 5;    // x: {4,5,8}
}
```

## Dịch chuyển con trỏ

Con trỏ có thể cộng/trừ với số nguyên. Với con trỏ `int`, tăng 1 thì địa chỉ tăng 32 bit (4 byte); tăng 2 thì tăng 64 bit. Con trỏ `char` tăng 1 thì tăng 8 bit (1 byte).

### Dùng dịch chuyển để truy cập mảng

Mảng là vùng liên tục. Trong C/C++, dùng tên mảng là địa chỉ phần tử đầu.

```cpp
int main() {
  int a[3] = {1, 2, 3};
  int* p = a;  // p trỏ a[0]
  *p = 4;      // a: [4, 2, 3]
  p = p + 1;   // p trỏ a[1]
  *p = 5;      // a: [4, 5, 3]
  p++;         // p trỏ a[2]
  *p = 6;      // a: [4, 5, 6]
}
```

Truy cập mảng qua con trỏ thường dùng dịch chuyển: địa chỉ gốc + offset.

Toán tử `[]` chính là cú pháp cho phép đó, `p[4]` tương đương `*(p + 4)`.

## Con trỏ rỗng

Trước C++11, dùng `NULL`:

```cpp
// Trước C++11
#define NULL 0
```

???+ note "Định nghĩa `NULL` trong C"
    Trước C23, C có hai định nghĩa `NULL` khác kiểu: một là hằng số nguyên, một là hằng số kiểu `void*`, đều có giá trị 0, compiler chọn một.

Trộn `NULL` với số nguyên trong C++ gây nhiều vấn đề, ví dụ:

```cpp
int f(int x);
int f(int* p);
```

Gọi `f(NULL)` sẽ gọi `int(int)` chứ không phải `int(int*)`.

???+ note "Vấn đề `NULL` trong C"
    C còn tệ hơn: trong variadic function, nếu người viết muốn nhận con trỏ mà người gọi truyền `NULL` kiểu nguyên, sẽ dẫn đến hành vi không xác định vì chuyển đổi nguyên sang con trỏ là UB. [^note1]

C++11 đưa `nullptr` để giải quyết.

`nullptr` có thể chuyển ngầm sang mọi kiểu con trỏ, tạo giá trị con trỏ rỗng.

Kiểu của `nullptr` là `std::nullptr_t`, ví dụ:

```cpp
namespace std {
typedef decltype(nullptr) nullptr_t;
}
```

Từ C++11, `NULL` thường được định nghĩa lại:

```cpp
// C++11 trở đi
#define NULL nullptr
```

???+ note "C cải tiến hằng con trỏ rỗng"
    C23 cũng đưa `nullptr` và `nullptr_t` vì lý do tương tự.[^note1]

## Con trỏ nâng cao

Con trỏ giúp thao tác dữ liệu ngoài phạm vi.

### Tham số kiểu con trỏ

Trong C/C++, tham số hàm truyền bằng copy (trừ reference). Muốn sửa dữ liệu bên ngoài hoặc tránh copy lớn, truyền địa chỉ bằng con trỏ.

Ví dụ `my_swap`:

```cpp
void my_swap(int *a, int *b) {
  int t;
  t = *a;
  *a = *b;
  *b = t;
}

int main() {
  int a = 6, b = 10;
  my_swap(&a, &b);
  // Sau gọi, a = 10, b = 6
}
```

C++ có reference, dễ dùng và an toàn hơn. Xem [C++: reference](./reference.md) và [C vs C++: pointer/reference](./cpp-other-langs.md#指针与引用).

### Cấp phát động

Lập trình thường cần cấp phát bộ nhớ động. Khi gọi OS để cấp phát, OS trả địa chỉ; ta lưu bằng con trỏ.

C++ dùng `new` để cấp phát và `delete` để giải phóng:

```cpp
int* p = new int(1234);
/* ... */
delete p;
```

Có thể `new` đối tượng:

```cpp
class A {
  int a;

 public:
  A(int a_) : a(a_) {}
};

int main() {
  A* p = new A(1234);
  /* ... */
  delete p;
}
```

`new` sẽ cấp bộ nhớ và gọi constructor, trả địa chỉ.

```cpp
struct ThreeInt {
  int a;
  int b;
  int c;
};

int main() {
  ThreeInt* p = new ThreeInt{1, 2, 3};
  /* ... */
  delete p;
}
```

???+ note "List initialization"
    `{}` dùng để init struct không có constructor; cũng làm cú pháp thống nhất. Xem [list initialization (since C++11)](https://en.cppreference.com/w/cpp/language/list_initialization).

Phải `delete` khi không dùng nữa. Không `delete` một vùng nhiều lần. `delete` trên `nullptr` là hợp lệ.

### Tạo mảng động

Dùng `new[]` tạo mảng, trả địa chỉ phần tử đầu, và `delete[]` để giải phóng.

```cpp
size_t element_cnt = 5;
int *p = new int[element_cnt];
delete[] p;
```

Mảng liên tục, `p + 1` là phần tử kế tiếp.

### Mảng 2 chiều

Mảng 2D là “mảng của mảng”. Bộ nhớ là tuyến tính nên có khái niệm “liên tục” hay không.

Liên tục nghĩa là cuối hàng này kề đầu hàng sau; toàn bộ xem như 1D. Không liên tục thì từng hàng rời nhau.

Với mảng liên tục, chỉ cần một vòng lặp với con trỏ tăng dần. Với mảng không liên tục, cần lấy địa chỉ đầu từng hàng.

???+ note "Cách lưu mảng 2D"
    Lưu theo hàng gọi là row-major; theo cột gọi là column-major. Truy cập liên tục nhanh hơn, nên chọn theo cách sử dụng dữ liệu.

### Tạo mảng 2D động

Có thể khai báo mảng 2D liên tục:

???+ note "Mô tả chiều"
    Nói tổng quát là chiều thứ n. Với row-major, chiều 1 là N, chiều 2 là M.

```cpp
int a[N][M];
```

N và M phải là hằng tại compile-time.

Chỉ số bắt đầu 0, nên `a[r][c]` là phần tử hàng r+1 cột c+1.

Nếu kích thước động, thường tạo mảng 1D rồi ánh xạ `r*M + c`:

```cpp
int* a = new int[N * M];
```

Cách này đảm bảo **liên tục**.

???+ note "Lưu trữ tuyến tính"
    Dữ liệu trong bộ nhớ đều tuyến tính; có thể dùng mảng 1D để biểu diễn mảng nD theo quy tắc.

Cũng có thể theo mô hình “mảng của mảng”: mảng các con trỏ.

```cpp
int** a = new int*[5];
```

Cấp phát từng hàng:

```cpp
for (int i = 0; i < 5; i++) {
  a[i] = new int[5];
}
```

Giải phóng:

```cpp
for (int i = 0; i < 5; i++) {
  delete[] a[i];
}
delete[] a;
```

Cách này không đảm bảo liên tục.

Cách khác dùng “con trỏ tới mảng”:

???+ note "Khác biệt giữa tên mảng và địa chỉ phần tử đầu"
    Trong C/C++, tên mảng có giá trị là địa chỉ phần tử đầu, nhưng kiểu của tên mảng là kiểu mảng, không phải phần tử.
    
    ```cpp
    int main() { int a[5] = {1, 2, 3, 4, 5}; }
    ```
    
    Về khái niệm, `a` có kiểu `int[5]`; thực tế `a + 1` dịch 5 phần tử `int`.

```cpp
int main() {
  int(*a)[5] = new int[5][5];
  int* p = a[2];
  a[2][1] = 1;
  delete[] a;
}
```

Cách này liên tục và dùng `a[n]` lấy địa chỉ hàng n+1; `a[r][c]` truy cập phần tử `(r,c)`.

Vì con trỏ tới mảng là kiểu xác định, nên các chiều trừ chiều đầu phải là hằng compile-time; nếu không compiler không hiểu `a[n]`.

## Con trỏ hàm

Xem [C++ 函数](./func.md).

Gọi hàm cần biết kiểu tham số, số lượng, kiểu trả về (gọi chung là interface).

Có thể gọi hàm qua con trỏ hàm; nếu nhiều hàm cùng interface, con trỏ hàm giúp chọn hàm **động** theo runtime.

Ví dụ hai hàm toán với 2 int:

```cpp
#include <iostream>

int (*binary_int_op)(int, int);

int foo1(int a, int b) { return a * b + b; }

int foo2(int a, int b) { return (a + b) * b; }

int main() {
  int choice;
  std::cin >> choice;
  if (choice == 1) {
    binary_int_op = foo1;
  } else {
    binary_int_op = foo2;
  }

  int m, n;
  std::cin >> m >> n;
  std::cout << binary_int_op(m, n);
}
```

???+ note "`&`, `*` và con trỏ hàm"
    Trong C, các dạng `void (*p)() = foo;`, `void (*p)() = &foo;`, `void (*p)() = *foo;`, `void (*p)() = ***foo` là như nhau.
    
    Hàm có thể chuyển ngầm thành con trỏ hàm, nên `void (*p)() = foo;` hợp lệ.
    
    `&` lấy địa chỉ, áp dụng cho hàm, nên `&foo` cũng hợp lệ.
    
    `*` trên con trỏ hàm cho ra hàm; với `**foo` thì `*foo` là hàm, rồi lại chuyển ngầm thành con trỏ hàm; lặp lại vẫn như vậy.
    
    Gọi `(*p)()` và `p()` là như nhau.
    
    Tham khảo: [Why do function pointer definitions work with any number of & or *?](https://stackoverflow.com/questions/6893285/why-do-function-pointer-definitions-work-with-any-number-of-ampersands-or-as)

Có thể dùng `typedef` cho kiểu con trỏ hàm:

```cpp
typedef int (*p_bi_int_op)(int, int);
```

Sau đó dùng `p_bi_int_op` như kiểu con trỏ hàm “nhận 2 int, trả int”.

Có thể dùng `std::function` để tiện hơn (chưa hoàn thiện).

Con trỏ hàm dùng được cho callback (chưa hoàn thiện).

## Tài liệu và chú thích

[^note1]: Xem [Introduce the nullptr constant](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3042.htm)
