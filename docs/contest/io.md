author: Marcythm, YZircon, Chaigidel, Tiger3018, voidge, H-J-Granger, ouuan, Enter-tainer, lcfsih, Xeonacid, Ir1d

Bài viết giới thiệu cách tối ưu I/O dựa trên stream và I/O kiểu C.

???+ note "Lưu ý"
    Tốc độ I/O stream và C thay đổi theo môi trường (compiler, OS, phần cứng). Nếu muốn phân tích sâu hơn, hãy dựa vào kết quả thực nghiệm và kiểm soát biến.

## I/O dựa trên stream

Với stream (như `std::cin`/`std::cout`), tối ưu phổ biến là tắt đồng bộ với C stream và tách liên kết input-output.

### Tắt đồng bộ

Dùng [`std::ios::sync_with_stdio(false)`](https://en.cppreference.com/w/cpp/io/ios_base/sync_with_stdio) để tắt đồng bộ với C stream. C++ đồng bộ với C để tránh乱 khi dùng `printf` và `std::cout`. C++ stream đồng bộ là thread-safe.

Đây là biện pháp bảo thủ. Khi bật đồng bộ, mỗi I/O sẽ đồng bộ với C buffer; nếu không dùng I/O kiểu C thì là thừa. Vì vậy có thể tắt trước khi I/O. Nhưng sau đó không được同时 dùng `std::cin` với `scanf`, cũng không同时 dùng `std::cout` với `printf`; tuy nhiên có thể dùng `std::cin` với `printf`, hoặc `scanf` với `std::cout`.

### Tách liên kết

Dùng [`tie()`](https://en.cppreference.com/w/cpp/io/basic_ios/tie) để tách liên kết giữa input và output.

Mặc định `std::cin` liên kết với `&std::cout`, nên mỗi lần nhập sẽ gọi `std::cout.flush()`, tăng负担. Có thể `std::cin.tie(nullptr)` để tách liên kết, tăng hiệu suất.

???+ warning "Lưu ý"
    Không được bỏ tham số thành `std::cin.tie()`, như vậy không tách mà chỉ trả về stream liên kết. Cũng không cần `std::cout.tie(nullptr)` vì mặc định không có output stream liên kết với `std::cout`.

### Mã mẫu

```cpp
std::ios::sync_with_stdio(false);
std::cin.tie(nullptr);
```

???+ note "Lưu ý"
    Sau khi làm hai thao tác này, cần tự `flush` để đảm bảo `std::cout` hiển thị trước khi `std::cin` đọc. Vì lúc này `std::cin` không tự flush `std::cout`. Ví dụ:
    
    ```cpp
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr);
    std::cout << "Please input your name: "
              << std::flush;  // Hoặc: std::endl;
                              // Mỗi lần dùng std::endl sẽ flush, còn \n thì không.
    // Nếu bỏ std::flush, sẽ không hiện提示 trước khi nhập
    std::cin >> name;
    ```

## I/O kiểu C

`scanf` và `printf` vẫn còn space tăng tốc, chủ yếu ở chuyển đổi số nguyên và chuỗi.

???+ note "Lưu ý"
    Tối ưu đọc/ghi ở đây针对 số nguyên. Tối ưu số thực phức tạp; đọc tham khảo [Bellerophon](https://dl.acm.org/doi/10.1145/93542.93557), ghi tham khảo [Ryū](https://dl.acm.org/doi/10.1145/3192366.3192369).

### Thiết kế

???+ note "Lưu ý"
    Cách tối ưu hiện tại tập trung vào tốc độ I/O, chuyển đổi dữ liệu dùng phương pháp朴素, chưa tận dụng phần cứng. Hầu hết CPU x86 hỗ trợ AVX2, có thể dùng SIMD tăng tốc chuyển đổi. STL không dùng SIMD; ví dụ libstdc++ [实现](https://github.com/gcc-mirror/gcc/blob/releases/gcc-14.3.0/libstdc%2B%2B-v3/include/bits/charconv.h#L81) chuyển 2 chữ số và tra bảng. Tối ưu chuyển đổi có thể有益, nhưng trong thi, các method ở đây已 đủ cho đa số场景.

#### Tối ưu đọc

Mỗi số nguyên gồm dấu và phần số; dấu luôn trước phần số. Với dấu, `+` thường bỏ qua và không ảnh hưởng giá trị; `-` không được bỏ, cần判定. Nếu input không chứa số âm, có thể bỏ判定.

Phần số chỉ gồm 0..9; khi đọc đến ký tự không phải số (thường là khoảng trắng), kết thúc số.

Vì đọc từ trái sang phải, có thể dùng秦九韶 để chuyển đổi trong lúc đọc.

Khi đọc, cần kiểm tra ký tự có phải số: `ch >= '0' && ch <= '9'`, hoặc dùng [`isdigit()`](https://en.cppreference.com/w/cpp/string/byte/isdigit).

#### Tối ưu ghi

Ghi số thường chuyển sang chuỗi bằng phương pháp朴素: lấy từng chữ số từ thấp lên cao rồi đảo.

### Chi tiết triển khai

#### Vấn đề tràn số

Cần注意 tràn số. Ví dụ lấy đối số âm của min int gây tràn; có thể dẫn đến output sai. Đọc min int cũng có thể tràn, nhưng có thể không sai do giá trị tràn trùng input.

Tràn số có dấu là UB; có thể利用 tính chất chia số âm của C (làm tròn về 0) để tránh. Nếu không cần âm, hoặc không可能 gặp min int, vấn đề không出现.

#### Tăng tính通用

Nếu dùng nhiều kiểu số nguyên, cần nhiều hàm I/O khác kiểu nhưng logic giống nhau. Có thể dùng C++ [`template`](https://en.cppreference.com/w/cpp/language/templates.html). Ví dụ C++11:

```cpp
template <typename T>
typename std::enable_if<std::is_integral<T>::value &&
                        std::is_signed<T>::value>::type
read(T &x);
```

Hoặc C++20:

```cpp
template <std::signed_integral T>
void read(T &x);
```

Để tiện đọc, phần sau giả định chỉ cần `int`; các implementation đủ cho đa số题.

### 实现

Các implementation khác nhau ở hàm đọc/ghi, logic chuyển đổi giống nhau.

#### Dùng `getchar` và `putchar`

Core:

```cpp
--8<-- "docs/contest/code/io/io_1.cpp:core"
```

#### Dùng `fread` và `fwrite`

`fread`/`fwrite` có thể nhanh hơn. Hàm signature:

```cpp
std::size_t fread(void* buffer, std::size_t size, std::size_t count,
                  std::FILE* stream);
std::size_t fwrite(const void* buffer, std::size_t size, std::size_t count,
                   std::FILE* stream);
```

Ví dụ `fread(Buf, 1, SIZE, stdin)` đọc `SIZE` byte vào `Buf`, trả số byte đọc được.

`fread`/`fwrite` đọc/ghi theo block, nhanh hơn `getchar`/`putchar`. Nếu buffer đủ lớn, có thể đọc cả file một lần; nếu không, cần đọc nhiều lần. Để实现, chỉ cần redefinition `getchar`.

```cpp
char buf[1 << 20], *p1, *p2;
#define gc()                                                               \
  (p1 == p2 && (p2 = (p1 = buf) + fread(buf, 1, 1 << 20, stdin), p1 == p2) \
       ? EOF                                                               \
       : *p1++)
```

Ghi tương tự:先把 output vào buffer, cuối cùng `fwrite` một lần.

Core:

```cpp
--8<-- "docs/contest/code/io/io_2.cpp:core"
```

Lưu ý:

-   Khi tắt debug dùng `fread()`/`fwrite()`; khi mở debug dùng `getchar()`/`putchar()` để dễ debug.
-   Nếu đọc/ghi file, cần `freopen()` trước mọi đọc/ghi.

#### Dùng `mmap`

`mmap` là system call Linux, ánh xạ file vào memory, có thể nhanh hơn. Hàm signature:

```c
void *mmap(void addr[.length], size_t length, int prot, int flags, int fd,
           off_t offset);
```

???+ warning "Lưu ý"
    `mmap` không dùng được trên Windows (Codeforces/HDU), và không khuyến nghị dùng trong thi. Thực tế `fread` đã đủ nhanh; nếu dùng `mmap` lặp đọc file nhỏ, chi phí映射 + page fault có thể lớn hơn `fread`.

Cần lấy fd, dùng `fstat` lấy size, rồi `mmap` lấy pointer `*pc`. Sau đó dùng `*pc++` thay `getchar()` để đọc.

Nếu đọc từ stdin, có thể dùng fd=0. **Nhưng mmap stdin rất nguy hiểm, và không thể nhập từ terminal; có thể redirect file vào stdin.**

???+ note "Ví dụ：[洛谷 P10815【模板】快速读入](https://www.luogu.com.cn/problem/P10815)"
    Đọc $n$ số trong $[-n, n]$, tính tổng. $n \leq 10^8$. Dữ liệu đảm bảo với mọi prefix, tổng nằm trong phạm vi 32-bit signed.

Code tham khảo:

```cpp
--8<-- "docs/contest/code/io/io_3.cpp"
```

## 参考

[cin.tie 与 sync\_with\_stdio 加速输入输出 - 码农场](https://www.hankcs.com/program/cpp/cin-tie-with-sync_with_stdio-acceleration-input-and-output.html)

[C++ 高速化 - Heavy Watal](https://heavywatal.github.io/cxx/speed.html)

['Re: mmap/mlock performance versus read' - MARC](https://marc.info/?l=linux-kernel&m=95496636207616&w=2)
