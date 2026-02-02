C++ định nghĩa một hệ thống hoàn chỉnh để khai báo hằng chỉ-đọc; biến được `const` là chỉ đọc, trình biên dịch sẽ kiểm tra xung đột tại thời điểm biên dịch để tránh sửa đổi, đồng thời có thể tối ưu.

Thông thường nên dùng `const` cho biến và tham số để tăng độ an toàn và độ rõ ràng của mã.

## Từ khóa loại `const`

### Hằng

Biến `const` không thể đổi giá trị sau khi khởi tạo.

```cpp
const int a = 0;  // a có kiểu const int

// a = 1; // không thể sửa hằng
```

### Tham chiếu hằng, con trỏ hằng

Tham chiếu hằng và con trỏ hằng đều hạn chế việc sửa giá trị được trỏ tới.

```cpp
int a = 0;
const int b = 0;

int *p1 = &a;
*p1 = 1;
const int *p2 = &a;
// *p2 = 2; // không thể sửa biến thông qua con trỏ hằng
// int *p3 = &b; // không thể dùng int* trỏ tới biến const int
const int *p4 = &b;

int &r1 = a;
r1 = 1;
const int &r2 = a;
// r2 = 2; // không thể sửa biến thông qua tham chiếu hằng
// int &p3 = b; // không thể dùng int& tham chiếu biến const int
const int &r4 = b;
```

Ngoài ra cần phân biệt con trỏ hằng (`const t*`) và hằng con trỏ (`t* const`), ví dụ:

```cpp
int* const p1;  // hằng con trỏ: sau khởi tạo không đổi địa chỉ, có thể đổi giá trị trỏ tới
const int* p2;  // con trỏ hằng: không đổi giá trị khi giải tham chiếu, có thể trỏ tới int khác
const int* const p3;  // con trỏ hằng kiêm hằng con trỏ: cả giá trị lẫn địa chỉ đều không đổi

// Dùng alias để tăng độ dễ đọc
using const_int = const int;
using ptr_to_const_int = const_int*;
using const_ptr_to_const_int = const ptr_to_const_int;
```

Dùng `const` trong tham số hàm giúp tránh sửa nhầm và tăng độ rõ ràng.

```cpp
void sum(const std::vector<int> &data, int &total) {
  for (auto iter = data.begin(); iter != data.end(); ++iter)
    total += *iter;  // iter là iterator, kiểu sau giải tham chiếu là const int
}
```

## Hàm thành viên `const`

Hàm thành viên có `const` dùng để cấm sửa thành viên.

```cpp
#include <iostream>

struct ConstMember {
  int s = 0;

  void func() { std::cout << "General Function" << std::endl; }

  void constFunc1() const { std::cout << "Const Function 1" << std::endl; }

  void constFunc2(int ss) const {
    // func(); // hàm const không thể gọi hàm không const
    constFunc1();

    // s = ss; // hàm const không thể sửa biến thành viên
  }
};

int main() {
  int b = 1;
  ConstMember c{};
  const ConstMember d = c;
  // d.func(); // đối tượng const không thể gọi hàm không const
  d.constFunc2(b);
  return 0;
}
```

## Biểu thức hằng `constexpr` (C++11)

Biểu thức hằng là biểu thức có thể tính tại thời điểm biên dịch; `constexpr` yêu cầu trình biên dịch tính được giá trị của hàm hoặc biến tại thời điểm biên dịch.

Tính tại compile-time cho phép tối ưu tốt hơn, ví dụ nhúng trực tiếp kết quả vào mã máy, bỏ chi phí tính runtime. Khác với tối ưu do `const`, khi `constexpr` thỏa điều kiện biểu thức hằng, trình biên dịch **bắt buộc** tính ở compile-time.

???+ note "Hiểu trực quan: `const` là “chỉ đọc”, `constexpr` là “bất biến”"
    ```cpp
    constexpr int a = 10;  // khai báo hằng trực tiếp
    
    constexpr int FivePlus(int x) { return 5 + x; }
    
    void test(const int x) {
      std::array<int, x> c1;            // lỗi, x không biết tại compile-time
      std::array<int, FivePlus(6)> c2;  // được, FivePlus biết tại compile-time
    }
    ```

Ví dụ sau minh họa rõ sự khác biệt giữa `const` và `constexpr`, dùng đệ quy tính Fibonacci và in ra:

???+ note "Cài đặt"
    ```cpp
    #include <iostream>
    
    using namespace std;
    
    constexpr unsigned fib0(unsigned n) {
      return n <= 1 ? 1 : (fib0(n - 1) + fib0(n - 2));
    }
    
    unsigned fib1(unsigned n) { return n <= 1 ? 1 : (fib1(n - 1) + fib1(n - 2)); }
    
    int main() {
      constexpr auto v0 = fib0(9);
      const auto v1 = fib1(9);
    
      cout << v0;
      cout << ' ';
      cout << v1;
    }
    ```

???+ note "Ví dụ mã máy sau biên dịch (Compiler Explorer, Clang 19)"
    ```nasm
    fib1(unsigned int):
            push    r14
            push    rbx
            push    rax
            mov     ebx, 1
            cmp     edi, 2
            jb      .LBB0_4
            mov     r14d, edi
            xor     ebx, ebx
    .LBB0_2:
            lea     edi, [r14 - 1]
            call    fib1(unsigned int)
            add     r14d, -2
            add     ebx, eax
            cmp     r14d, 1
            ja      .LBB0_2
            inc     ebx
    .LBB0_4:
            mov     eax, ebx
            add     rsp, 8
            pop     rbx
            pop     r14
            ret
    
    main:
            push    r14
            push    rbx
            push    rax
            mov     edi, 9
            call    fib1(unsigned int) # khởi tạo `v1` có gọi hàm
            mov     ebx, eax
            mov     r14, qword ptr [rip + std::__1::cout@GOTPCREL]
            mov     rdi, r14
            mov     esi, 55 # `v0` được thay bằng kết quả tính sẵn
            call    std::__1::basic_ostream<char, std::__1::char_traits<char>>::operator<<(unsigned int)@PLT
            mov     byte ptr [rsp + 7], 32
            lea     rsi, [rsp + 7]
            mov     edx, 1
            mov     rdi, r14
            call    std::__1::basic_ostream<char, std::__1::char_traits<char>>& std::__1::__put_character_sequence[abi:ne200000]<char, std::__1::char_traits<char>>(std::__1::basic_ostream<char, std::__1::char_traits<char>>&, char const*, unsigned long)
            mov     rdi, r14
            mov     esi, ebx # đọc giá trị biến
            call    std::__1::basic_ostream<char, std::__1::char_traits<char>>::operator<<(unsigned int)@PLT
            xor     eax, eax
            add     rsp, 8
            pop     rbx
            pop     r14
            ret
    ```

Hàm `fib0` được gọi với tham số hằng, nên toàn bộ chạy ở compile-time. Do không chạy ở runtime, trình biên dịch có thể bỏ phát sinh mã máy của hàm.

Đồng thời, `v0` không có mã khởi tạo; khi in ra, `v0` đã được thay bằng kết quả cuối cùng, cho thấy giá trị đã được tính tại compile-time.
Còn `v1` vẫn là lời gọi đệ quy thông thường.

Vì vậy `constexpr` có thể dùng thay cho macro hằng, tránh [rủi ro của macro](./basic.md#define-命令).

Trong bài toán thuật toán, có thể dùng `constexpr` để lưu biến có kích thước nhỏ, giúp loại bỏ chi phí tính toán runtime. Đặc biệt thường gặp trong kỹ thuật "[đánh bảng](../contest/dictionary.md)" khi dùng `constexpr` cho mảng/lưu đáp án.

???+ note "Tính toán compile-time quá lớn sẽ gây lỗi biên dịch"
    Trình biên dịch giới hạn chi phí tính compile-time; nếu quá lớn sẽ không biên dịch được, khi đó nên dùng `const`.
    
    ```cpp
    #include <iostream>
    
    using namespace std;
    
    constexpr unsigned long long fib(unsigned long long i) {
      return i <= 2 ? i : fib(i - 2) + fib(i - 1);
    }
    
    int main() {
      // constexpr auto v = fib(32); evaluation exceeded maximum depth
      const auto v = fib(32);
      cout << v;
      return 0;
    }
    ```

???+ note "Lỗi biên dịch của Clang khi dùng constexpr"
    ```text
    <source>:10:20: error: constexpr variable 'v' must be initialized by a constant expression
        10 |     constexpr auto v = fib(32);
        |                    ^   ~~~~~~~~~~~~
    <source>:6:25: note: constexpr evaluation exceeded maximum depth of 512 calls
        6 |     return i <= 2 ? i : fib(i - 2) + fib(i - 1);
        |                         ^
    <source>:6:25: note: in call to 'fib(32)'
        6 |     return i <= 2 ? i : fib(i - 2) + fib(i - 1);
        |                         ^~~~~~~~~~
    <source>:6:25: note: in call to ...
    ```

## Tài liệu tham khảo

-   [C++ 关键字——const](https://zh.cppreference.com/w/cpp/keyword/const)
-   [C++ 关键字——constexpr](https://zh.cppreference.com/w/cpp/keyword/constexpr)
