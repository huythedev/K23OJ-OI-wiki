author: Ir1d, tsagaanbar, yang-lile

## Khai báo hàm

Trong lập trình, hàm (function) là tập hợp các câu lệnh. Ta cũng gọi là **tiểu thủ tục** (subroutine). Khi có thao tác lặp lại, có thể trích ra thành hàm. Hàm nhận các giá trị đầu vào gọi là tham số, và có thể trả về giá trị gọi là giá trị trả về.

Để khai báo hàm, cần kiểu trả về, tên hàm và danh sách tham số.

```cpp
// kiểu trả về int
// tên hàm some_function
// danh sách tham số int, int
int some_function(int, int);
```

Như trên, ta khai báo hàm `some_function` nhận hai tham số kiểu `int`, trả về `int`. Có thể hiểu hàm sẽ xử lý hai số nguyên và trả về kết quả cùng kiểu.

## Cài đặt hàm: viết định nghĩa

Chỉ khai báo (declaration) là chưa đủ; nó chỉ cho ta biết **giao diện** (nhận gì, trả gì), nhưng thiếu phần **định nghĩa** (definition). Ta có thể **cài đặt** (implement) hàm ở vị trí khác sau khai báo (hoặc ở file khác và liên kết khi build).

Nếu hàm có trả về, dùng `return` để trả kết quả. Khi gặp `return`, hàm kết thúc, không chạy tiếp.

```cpp
int some_function(int, int);  // khai báo

/* một số code khác... */

int some_function(int x, int y) {  // định nghĩa
  int result = 2 * x + y;
  return result;
  result = 3;  // dòng này không được thực thi
}
```

Khi định nghĩa, ta đặt tên cho tham số để dùng trong thân hàm.

Nếu cùng file, có thể **gộp khai báo và định nghĩa**, tức là định nghĩa ngay khi khai báo.

```cpp
int some_function(int x, int y) { return 2 * x + y; }
```

Nếu hàm không trả về, dùng kiểu `void`. Nếu không cần tham số, để danh sách trống. Hàm `void` gặp `return;` cũng kết thúc.

```cpp
void say_hello() {
  cout << "hello!\n";
  cout << "hello!\n";
  cout << "hello!\n";
  return;
  cout << "hello!\n";  // dòng này không được thực thi
}
```

## Gọi hàm

Giống biến, hàm phải được khai báo trước khi dùng. Hành vi dùng hàm gọi là “gọi” (call). Có thể gọi hàm khác bên trong hàm, kể cả tự gọi chính nó, gọi là **đệ quy** (recursion).

Cú pháp gọi là **tên hàm + cặp ngoặc** `()` như `foo()`. Nếu có tham số, điền theo thứ tự, cách nhau dấu phẩy, như `foo(1, 2)`. Gọi hàm là một biểu thức, **giá trị trả về** là **giá trị của biểu thức**.

Các tham số khai báo có thể hiểu là biến dùng trong **lần gọi hiện tại**, giá trị khởi tạo từ vị trí gọi. Ví dụ:

```cpp
void foo(int, int);

/* ... */

void foo(int x, int y) {
  x = x * 2;
  y = y + 3;
}

/* ... */

a = 1;
b = 1;
// trước gọi: a = 1, b = 1
foo(a, b);  // gọi foo
            // sau gọi: a = 1, b = 1
```

Ở ví dụ trên, `foo(a, b)` gọi `foo`. Khi gọi, `x` và `y` được khởi tạo từ `a` và `b`. Vì vậy, sửa `x` và `y` **không ảnh hưởng** đến `a` và `b`.

Nếu muốn sửa biến ở ngoài, cần **truyền tham chiếu**.

```cpp
void foo(int& x, int& y) {
  x = x * 2;
  y = y + 3;
}

/* ... */

a = 1;
b = 1;
// trước gọi: a = 1, b = 1
foo(a, b);  // gọi foo
            // sau gọi: a = 2, b = 4
```

Trong ví dụ, sau `int` có dấu `&` biểu thị **tham chiếu**. Khi gọi, `x` và `y` là “bí danh” của `a` và `b`, nên sửa `x`/`y` là sửa trực tiếp `a`/`b`.

## Hàm `main`

Mỗi chương trình C/C++ phải có hàm `main`. Mọi chương trình bắt đầu chạy từ `main`.

> `main` cũng có thể có tham số để nhận lệnh từ bên ngoài (tham số dòng lệnh), nhằm phản ứng khác nhau.

Đoạn code gọi hàm (tiểu thủ tục) ví dụ:

```cpp
// hello_subroutine.cpp

#include <iostream>

void say_hello() {
  std::cout << "hello!\n";
  std::cout << "hello!\n";
  std::cout << "hello!\n";
}

int main() {
  say_hello();
  say_hello();
}
```
