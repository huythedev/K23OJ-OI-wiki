author: inclyc

Ngôn ngữ thường dùng trong OI là C++. Đã dùng C++ thì phải làm việc với compiler và chuẩn. C++ rất “hỗn loạn và nguy hiểm”; bài này đưa kiến thức compiler thực dụng, đủ cho thi.

## Giới thiệu tối ưu hóa compiler

### Tối ưu hóa là gì (Optimization)

Theo [as-if rule](https://en.cppreference.com/w/cpp/language/as_if), trong khi giữ nguyên ngữ nghĩa, tối ưu tốc độ chạy và/hoặc kích thước file thực thi.

<!-- ### Các cuộc thi bật tối ưu? -->

<!-- TODO: thi bật O2 -->

## Các tối ưu phổ biến

### Gập hằng (Constant Folding)

Gập hằng, hay truyền bá hằng (Constant Propagation): nếu một biểu thức có thể xác định là hằng, thì trước định nghĩa tiếp theo, có thể thay bằng hằng.

```cpp
int x = 1;
int y = x;  // x = 1, => y = 1
x = 3;
int z = 2 * y;   // z => 2 * y = 2 * 1 = 2
int y2 = x * 2;  // x = 3, => y2 = 6
```

Compiler có thể biến thành:

```cpp
int x = 1;
int y = 1;
x = 3;
int z = 2;
int y2 = 6;
```

Ví dụ: <https://godbolt.org/z/oEfY35TTd>

### Loại bỏ mã chết (Deadcode Elimination)

Mã không dùng sẽ bị bỏ.

```cpp
int test() {
  int a = 233;
  int b = a * 2;
  int c = 234;
  return c;
}
```

Có thể thành:

```cpp
int test() { return 234; }
```

Ở đây trước tiên gập hằng, rồi `a`, `b` là biến không hoạt động nên bị xóa.

### Xoay vòng lặp (Loop Rotate)

Chuyển vòng `for` thành `do-while`, thêm điều kiện trước. Mục đích là chuẩn bị cho tối ưu khác.

```cpp
for (int i = 0; i < n; ++i) {
  auto v = *p;
  use(v);
}
```

Thành:

```cpp
if (0 < n) {
  do {
    auto v = *p;
    use(v);
    ++i;
  } while (i < n);
}
```

### Đưa bất biến ra ngoài (Loop Invariant Code Motion)

Dựa trên phân tích alias, đưa đoạn bất biến ra ngoài vòng để giảm code trong vòng.

```cpp
for (int i = 0; i < n; ++i) {
  auto v = *p;
  use(v);
}
```

Trực quan có thể thành:

```cpp
auto v = *p;
for (int i = 0; i < n; ++i) {
  use(v);
}
```

Nhưng nếu `n <= 0`, vòng không chạy; ta lại thực hiện lệnh thừa (có thể có side effect). Vì vậy thường rotate sang do-while rồi thêm “loop guard”:

```cpp
if (0 < n) {  // loop guard
  auto v = *p;
  do {
    use(v);
    ++i;
  } while (i < n);
}
```

### Mở rộng vòng lặp (Loop Unroll)

Vòng lặp có thân và nhánh, CPU cần dự đoán nhánh. Unroll dùng kích thước code đổi lấy tốc độ.

```cpp
for (int i = 0; i < 3; i++) {
  a[i] = i;
}
```

Thành:

```cpp
a[0] = 0;
a[1] = 1;
a[2] = 2;
```

### Đưa điều kiện ra ngoài (Loop Unswitching)

Đưa điều kiện trong vòng ra ngoài, tạo 2 vòng riêng, tăng khả năng vector hóa/ song song.

```cpp
// clang-format off
void before(int x) {
  for(;/* i in some range */;) {
    /* A */;
    if (/* condition */ x % 2) {
      /* B */;
    }
    /* C */;
  }
}

void after(int x) {
  if (/* condition */ x % 2) {
    for(;/* i in some range */;) {
      /* A */;
      /* B */; // Thực hiện trực tiếp B
      /* C */;
    }
  } else {
     for(;/* i in some range */;) {
      /* A */; 
               // Không thực hiện B
      /* C */;
    }
  }
}
```

### Tối ưu bố cục code (Code Layout Optimizations)

Khi chạy, đường đi có “nóng/lạnh”. Nhảy thường chậm hơn chạy tuần tự (fallthrough). Code hay chạy là nóng; ít chạy là lạnh. Trong OI, kiểm tra biên hoặc xử lý ngoại lệ thường là lạnh.

Basic block là khối điều khiển cơ bản; thủ tục là đồ thị các block. Khi tạo executable, compiler sắp xếp layout; đây là trọng tâm tối ưu.

Nguyên tắc: ghép nóng gần nhau, lạnh tách ra, giúp cache tốt hơn.

```cpp
// clang-format off
int hotpath; // <-- Nóng!
if (/* điều kiện biên */ false) {
    // <-- Lạnh!
}
int hotpath_again;  // <-- Nóng!
```

#### Sắp xếp basic block

Dùng label mô tả “mã giả”. Ví dụ:

???+ note "Layout 1"
    ```cpp
    // clang-format off
    hotblock1:
        Stmts; // <-- Nóng!
        if (/* điều kiện biên sai */ true)
            goto hotblock2; // Thường xảy ra! ------+
    coldblock:                           /*   |   */
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |  Nhảy qua nhiều lệnh, tốn kém!
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
    hotblock2:                          /*    |   */
        Stmts; // <- Nóng!           <----------+
    ```

Layout tốt hơn:

???+ note "Layout 2"
    ```cpp
    // clang-format off
    hotblock1:
        Stmts; // <-- Nóng!
        if (/* điều kiện biên */ false)
            goto coldblock; // Hiếm xảy ra
    hotblock2:                         /*   |  Chi phí thấp!  */
        Stmts; // <- Nóng!  <-----------------+
    coldblock:
        Stmt; // <- Lạnh
        Stmt; // <- Lạnh
        Stmt; // <- Lạnh
        Stmt; // <- Lạnh
        Stmt; // <- Lạnh
    ```

Để báo nhánh dễ xảy ra, dùng C++20 `[[likely]]`, `[[unlikely]]`: <https://en.cppreference.com/w/cpp/language/attributes/likely>

Nếu không có C++20, dùng `__builtin_expect` (GNU extension):

```cpp
#define likely(x) __builtin_expect(!!(x), 1)
#define unlikely(x) __builtin_expect(!!(x), 0)

if (unlikely(/* kiểm tra biên */ false)) {
  // code lạnh
}
```

#### Tách code nóng/lạnh (Hot Cold Splitting)

Một thủ tục có cả nóng và lạnh; nếu lạnh dài, nên tách thành hàm gọi, thay vì chặn đường nóng. Điều này nhắc ta không nên ép mọi hàm `inline`: code lạnh nội tuyến làm chậm hơn gọi hàm.

???+ note "Bố cục không tốt"
    ```cpp
    // clang-format off
    void foo() {
          // clang-format off
    hotblock1:
        Stmts; // <-- Nóng!
        if (/* điều kiện biên sai */ true)
            goto hotblock2; // Thường xảy ra! ------+
    coldblock:                           /*   |   */
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |  Nhảy qua nhiều lệnh, tốn kém!
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
        Stmt; // <- Lạnh                      |
    hotblock2:                          /*    |   */
        Stmts; // <- Nóng!           <----------+
    }
    ```

???+ note "Bố cục tốt"
    ```cpp
    // clang-format off
    void foo() {
    hotblock1:
      Stmts;  // <-- Nóng!
      if (/* điều kiện biên */ false)
        coldBlock();  // Tách code lạnh
    hotblock2:
      Stmts;  // <- Nóng!
    }
    
    void coldBlock() {
      Stmt;  // <- Lạnh
      Stmt;  // <- Lạnh
      Stmt;  // <- Lạnh
      Stmt;  // <- Lạnh
      Stmt;  // <- Lạnh
      Stmt;  // <- Lạnh
      Stmt;  // <- Lạnh
    }
    ```

Tách nóng/lạnh là thao tác ngược của inlining. Inlining không luôn nhanh hơn; nếu inline code lạnh thì chậm hơn. Compiler có mô hình chi phí, tự quyết định inline; ta tự quyết không chắc tốt hơn.

Không có thông tin thêm, compiler giả định nhánh true/false có xác suất như nhau. PGO (Profile Guided Optimization) dùng profiling để lấy xác suất thực, giúp layout tốt hơn.

### Inline (Function Inlining)

Gọi hàm cần truyền tham số qua thanh ghi/stack; caller và callee phải lưu trạng thái, gọi là calling convention. Inline là chèn thân hàm vào nơi gọi, tránh overhead.

```cpp
int add(int x) { return x + 1; }

int foo() {
  int a = 1;
  a = add(a);
}
```

Có thể thành:

```cpp
int foo() {
  int a = 1;
  a = a + 1;  // thân add() được chèn
}
```

#### `always_inline`, `__force_inline`

<https://clang.llvm.org/docs/AttributeReference.html#always-inline-force-inline>

Một số compiler cho phép ép inline bằng `__attribute__((always_inline))`. Không chắc nhanh hơn; compiler tin rằng lập trình viên biết rõ hơn.

### Tối ưu gọi đuôi (Tail Call Optimization)

Nếu lời gọi hàm ở cuối hàm, gọi là tail call. Với tail call, có thể tối ưu vì không cần giữ stack frame của hàm ngoài.

#### Dùng jump thay vì call

Gọi hàm thường lưu `$pc`, lưu registers... Tail call không cần, có thể dịch thành jump. Vì tail recursion không quay lại vị trí cũ.

Ví dụ: <https://godbolt.org/z/e7b1safaW>

```cpp
int test(int a);

int tailCall(int x) { return test(x); }
```

```nasm
tailCall(int):                           ; @tailCall(int)
        jmp     test(int)@PLT                    ; TAILCALL
```

#### Tự động chuyển tail recursion

Nếu tail call gọi chính nó, là tail recursion. Có thể tối ưu thành vòng lặp, giảm stack. Khi bật tối ưu, compiler có thể làm thay.

```cpp
int fac(int n) {
  if (n < 2) return 1;
  return /* dùng */ n * fac(n - 1); /* dùng n, không tail */
}
```

Viết lại:

```cpp
int fac(int acc, int n) {
  if (n < 2) return acc;
  return fac(acc * n, n - 1);
}
```

Compiler có thể tự nhận dạng và biến đổi.

#### Loại bỏ tail recursion -Rpass=tailcallelim

Nếu tail recursion, có thể loại bỏ hoàn toàn. Ví dụ:

???+ note "[GCD](https://godbolt.org/z/8Wb6WEnzv)"
    ```cpp
    int gcd(int a, int b) { return b ? gcd(b, a % b) : a; }
    ```

???+ note "[Fibonacci](https://godbolt.org/z/4enof6Wcb)"
    ```cpp
    // Mở rộng fib(n-2)
    // fib(n-1) vẫn là đệ quy mũ, không thể tối ưu thành vòng lặp
    int fib(int n) {
      if (n < 2) return 1;
      return fib(n - 1) + fib(n - 2);
    }
    ```

???+ note "[Giai thừa](https://godbolt.org/z/n64e75xrf)"
    ```cpp
    // Mở rộng thành vòng lặp và vector hóa
    unsigned fac(unsigned n) {
      if (n < 2) return 1;
      return n * fac(n - 1);
    }
    ```

Các hàm này sau tối ưu có assembly tương tự phiên bản không đệ quy. Với O2, có thể viết đệ quy mà không thua hiệu năng, nếu compiler tối ưu được.

### Giảm độ mạnh (Strength Reduction)

Ví dụ `x * 2` thành `x << 1`. Compiler tự tối ưu; khi bật tối ưu, hai cách tương đương.

#### Biến đổi toán tử vô hướng

##### Dịch bit thay nhân

```cpp
int a;
a = x * 2;   // xấu!
a = x << 1;  // tốt!
```

Lưu ý khác biệt giữa số có dấu và không dấu; phép dịch có khác biệt. Chia số có dấu không thể luôn tối ưu thành shift.

```cpp
int l, r;
/* code */
int mid = (l + r) / 2; /* Nếu compiler không biết l,r không âm, code kém */
                       // Không thể tối ưu thành (l + r) >> 1
                       // Phản ví dụ:
                       // mid = -127
                       // mid / 2 = -63
                       // mid >> 1 = -64
```

```cpp
int mid = (l + r);
int sign = mid >> 31; /* Dịch phải logic, lấy dấu */
mid += sign;
mid >>= 1; /* Dịch phải số học */
```

Giải pháp:

-   dùng `unsigned l, r;` vì chỉ số nên không dấu
-   tự viết shift

##### Nhân thay chia

```cpp
int x = a / 3;
```

Có thể biến thành `x = a * 0x55555556 >> 32`, xem [Zhihu](https://zhuanlan.zhihu.com/p/151038723) hoặc [paper](https://dl.acm.org/doi/10.1145/773473.178249).

#### Strength reduction cho biến chỉ số (IndVars)

Compiler nhận dạng biến chỉ số và biến đổi phép toán nặng thành nhẹ.

```cpp
int a = 0;
for (int i = 1; i < 10; i++) {
  a = 3 * i;  // xấu!
  a = a + 3;  // tốt!
}
```

Compiler có thể biến `a = 3 * i` thành `a = a + 3`. Phân tích tiến hóa biến gọi là SCEV.

SCEV còn tối ưu:

```cpp
int test(int n) {
  int ans = 1;
  for (int i = 0; i < n; i++) {
    ans += i * (i + 1);
  }
  return ans;
}
```

Có thể tối ưu thành O(1); xem <https://godbolt.org/z/ET8d89vvK>. Hiện chỉ LLVM làm, GCC bảo thủ hơn.

```nasm
test(int):                               # @test(int)
        test    edi, edi
        jle     .LBB0_1
        lea     eax, [rdi - 1]
        lea     ecx, [rdi - 2]
        imul    rcx, rax
        lea     eax, [rdi - 3]
        imul    rax, rcx
        shr     rax
        imul    eax, eax, 1431655766
        and     ecx, -2
        lea     eax, [rax + 2*rcx]
        lea     eax, [rax + 2*rdi]
        dec     eax
        ret
.LBB0_1:
        mov     eax, 1
        ret
```

### Tự động vector hóa (Auto-Vectorization)

SIMD giúp xử lý nhiều dữ liệu trong một lệnh. OI không cần chi tiết; Clang thường vector hóa mạnh hơn GCC.

```cpp
// https://godbolt.org/z/h1hx5sWoE
void test(int *a, int *b, int n) {
  for (int i = 0; i < n; i++) {
    a[i] += b[i];
  }
}
```

#### `__restrict` (GNU, MSVC)

Hai con trỏ có thể trỏ vùng chồng lấn, khiến khó vector hóa. `__restrict` giả định không chồng lấn.

![](./images/overlap.png)

```cpp
void test(int* __restrict a, int* __restrict b, int n) {
  for (int i = 0; i < n; i++) {
    a[i] += b[i];
  }
}
```

`__restrict` không thuộc chuẩn C++ nhưng compiler hỗ trợ. Hữu ích khi cực kỳ tối ưu.

## Lỗi dùng ngôn ngữ liên quan tối ưu

### inline

Bật O2 thì compiler tự inline. `inline` trong struct thường thừa. Nếu không bật O2, `inline` cũng không chắc inline. Trong C++ hiện đại, `inline` là về liên kết, không phải tối ưu.

### register

Compiler hiện đại bỏ qua `register`; tự phân bổ tốt hơn. `register` bị bỏ từ C++11 và xóa ở C++17[^p0001r1].

<https://en.cppreference.com/w/cpp/keyword/register>

## UB (Undefined Behavior) và tối ưu

Compiler có thể giả định không có [UB](https://en.cppreference.com/w/cpp/language/ub), nên khi có UB, tối ưu có thể gây kết quả bất ngờ.

UB thường gặp:

1.  [Tràn số có dấu](https://users.cs.utah.edu/~regehr/papers/overflow12.pdf)
2.  Dùng biến chưa init
3.  Truy cập vượt giới hạn
4.  Dereference null
5.  Vòng lặp vô hạn không side-effect

### Tràn số có dấu

```cpp
int f(int x) { return x * 2 / 2; }
```

Compiler có thể tối ưu thành:

```cpp
int f(int x) { return x; }
```

Ví dụ: <https://godbolt.org/z/WKv3W5hvM>、<https://godbolt.org/z/qqE9nxP1j>.

Dùng [`-fwrapv`](https://gcc.gnu.org/onlinedocs/gcc-13.2.0/gcc/Code-Gen-Options.html#index-fwrapv) để tắt giả định này. Ví dụ: <https://godbolt.org/z/5x3K5KGnr>、<https://godbolt.org/z/4r4a4EzMW>.

### Dùng biến chưa init

```cpp
int f(int x) {
  int a;
  if (x)  // x != 0 hoặc UB
    a = 42;
  return a;
}
```

Compiler giả định không UB nên `a` luôn init; có thể tối ưu thành:

```cpp
int f(int) { return 42; }
```

Ví dụ: <https://godbolt.org/z/8WYMYYjdG>、<https://godbolt.org/z/qvGd1nvv9>.

### Truy cập vượt giới hạn

```cpp
int table[4] = {};

bool exists_in_table(int v) {
  // return true trong 4 lần đầu hoặc UB do out-of-bounds
  for (int i = 0; i <= 4; i++)
    if (table[i] == v) return true;
  return false;
}
```

Compiler giả định không UB nên hàm luôn trả true trước khi out-of-bounds:

```cpp
bool exists_in_table(int) { return true; }
```

Ví dụ: <https://godbolt.org/z/xfePeYsE3>.

### Dereference null

```cpp
int f(int* p) {
  int x = *p;
  if (!p)
    return x;  // UB ở trên hoặc nhánh này không bao giờ chạy
  else
    return 0;
}
```

Compiler giả định không UB, nên `!p` luôn false, tối ưu thành:

```cpp
int f(int*) { return 0; }
```

Ví dụ: <https://godbolt.org/z/GY1jvsrb5>、<https://godbolt.org/z/4ronPsnxf>.

### Vòng lặp vô hạn không side-effect

???+ note "Kiểm chứng định lý Fermat lớn"
    Theo [Fermat lớn](https://en.wikipedia.org/wiki/Fermat%27s_Last_Theorem), phương trình $a^3=b^3+c^3$ không có nghiệm nguyên dương. Dưới đây thử kiểm chứng trong [1,1000], nếu trả `true` thì tìm thấy nghiệm (mâu thuẫn).
    
    ```cpp
    #include <iostream>
    
    bool fermat() {
      const int max_value = 1000;
    
      // Vòng lặp vô hạn không side-effect là UB
      for (int a = 1, b = 1, c = 1; true;) {
        if (((a * a * a) == ((b * b * b) + (c * c * c))))
          return true;  // bác bỏ :()
        a++;
        if (a > max_value) {
          a = 1;
          b++;
        }
        if (b > max_value) {
          b = 1;
          c++;
        }
        if (c > max_value) c = 1;
      }
    
      return false;  // chưa bác bỏ
    }
    
    int main() {
      std::cout << "Fermat's Last Theorem ";
      fermat() ? std::cout << "has been disproved!\n"
               : std::cout << "has not been disproved.\n";
    }
    ```

Compiler có thể giả định vòng lặp sẽ kết thúc và trả true, nên chương trình có thể in:

```text
Fermat's Last Theorem has been disproved!
```

Ví dụ: <https://godbolt.org/z/d834MK7bz>、<https://godbolt.org/z/Eov9nsKqf>.

## Sanitizer

“Bảo đảm sự tỉnh táo”: kiểm tra UB, out-of-bounds, null... khi chạy. Debug local nên bật để giảm thời gian. Google phát triển; GCC/Clang đều hỗ trợ. LLVM hỗ trợ tốt hơn, nên dùng Clang khi debug.

### Address Sanitizer -fsanitize=address

<https://clang.llvm.org/docs/AddressSanitizer.html>

GCC/Clang đều hỗ trợ. Kiểm tra:

-   Out-of-bounds
-   Use-after-free
-   Use-after-return
-   Double-free
-   Memory-leaks
-   Use-after-scope

Chạy chậm ~2x.

### Undefined Behavior Sanitizer -fsanitize=undefined

<https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html>

UBSan kiểm tra UB, gồm:

-   Tràn dịch bit (ví dụ 32-bit dịch trái 72)
-   Tràn số có dấu
-   Tràn khi convert float->int

Xem trang doc để biết tác động.

## Misc

### Compiler Explorer

Xem hành vi compiler và assembly tại <https://godbolt.org>

## Đọc thêm

1.  [LLVM Blog: UB #1/3](https://blog.llvm.org/2011/05/what-every-c-programmer-should-know.html)
2.  [LLVM Blog: UB #2/3](https://blog.llvm.org/2011/05/what-every-c-programmer-should-know_14.html)
3.  [LLVM Blog: UB #3/3](https://blog.llvm.org/2011/05/what-every-c-programmer-should-know_21.html)

## Tài liệu và chú thích

[^p0001r1]: [Remove Deprecated Use of the register Keyword (open-std.org)](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0001r1.html)
