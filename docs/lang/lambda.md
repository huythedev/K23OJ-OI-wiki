**Lưu ý**: Xét tới thực tế trong thi thuật toán, bài viết này sẽ không nghiên cứu toàn bộ cú pháp, chỉ nêu các phần có thể dùng trong thi đấu.

Cú pháp trong bài dựa trên chuẩn **C++11**, các chuẩn cao hơn sẽ được nhắc khi cần và có đánh dấu rõ ràng.

## Biểu thức Lambda

Biểu thức Lambda được đặt tên theo phép toán $\lambda$ trong toán học, tương ứng trực tiếp với lambda abstraction. Khi biên dịch, trình biên dịch sẽ sinh ra một [**đối tượng hàm**](./new.md#函数对象) ẩn danh, lấy các biến bị bắt làm thành viên, tham số và thân hàm dùng để cài đặt quá tải `operator()`.

??? note "Đối tượng hàm (Function Object)"
    Đối tượng hàm là một đối tượng lớp, thường quá tải `operator()` nên có thể gọi như hàm. So với hàm thông thường, đối tượng hàm có nhiều ưu điểm: lưu được trạng thái, truyền làm tham số cho hàm khác, v.v.

Một dạng cú pháp của lambda:

```text
[capture] (parameters) mutable -> return-type {statement}
```

Bản thân lambda là một lớp, mở rộng ra như sau:

<!-- scripts.linter.preprocess.fix_details off -->

```text
class Lambda_1 {
 private:
  Lambda_1() : capture-list(init-value) { }

 public:
  return-type operator()(parameters) const { statement }

 private:
  mutable capture-list
};
```

<!-- scripts.linter.preprocess.fix_details on -->

Capture rỗng có thể chuyển ngầm thành con trỏ hàm, ví dụ:

```cpp
void (*f)(int, int) = [](int, int) -> void {};
```

Dưới đây lần lượt giới thiệu từng phần của cú pháp.

### statement thân hàm

Thân hàm tương tự thân hàm thông thường, ngoài việc truy cập tham số và biến toàn cục, còn có thể truy cập các biến [bị bắt](#capture-捕获子句).

### capture mệnh đề bắt

Lambda bắt đầu bằng mệnh đề capture, chỉ định biến nào bị bắt. Danh sách capture có thể rỗng, hoặc chỉ định cách bắt: biến có tiền tố `&` được truy cập bằng [tham chiếu](./reference.md), biến không có tiền tố được truy cập bằng giá trị.

Có thể dùng chế độ bắt mặc định, bắt mọi biến được nhắc tới trong lambda: `&` nghĩa là tất cả biến bị bắt bằng tham chiếu, `=` nghĩa là tất cả biến bị bắt bằng giá trị.

Sau khi dùng bắt mặc định, vẫn có thể **chỉ định rõ** cách bắt cho biến cụ thể.

Nếu cần bắt biến ngoài `a` theo tham chiếu và `b` theo giá trị, các capture sau đều được:

-   `[&a, b]`
-   `[b, &a]`
-   `[&, b]`
-   `[b, &]`
-   `[=, &a]`

Danh sách capture cũng có thể dùng để khai báo biến, kiểu được suy luận từ biểu thức khởi tạo, tương tự `auto`.

Một số ví dụ thường gặp:

```cpp
int a = 0;
auto f0 = []() { return a * 9; };   // Lỗi, không truy cập được 'a'
auto f1 = [a]() { return a * 9; };  // OK, 'a' bị bắt theo giá trị
auto f2 = [&a]() { return a++; };   // OK, 'a' bị bắt theo tham chiếu
auto f3 = [v = a + 1]() {
  return v + 1;
};  // OK, dùng initializer khai báo v, kiểu giống a

// Lưu ý: khi bắt theo tham chiếu, hãy đảm bảo a chưa bị hủy
auto b = f2();  // f2 lấy giá trị a từ capture, không cần truyền a qua tham số
```

#### generalized capture Bắt có khởi tạo (C++14)

Từ C++14, capture không chỉ bắt biến ngoài mà còn khai báo biến mới và khởi tạo:

```cpp
auto f1 = [val = 520]() {
  return val;
};  // OK, val kiểu int, giá trị 520, trả về int

auto f2 = [val = 520LL]() {
  return val;
};  // OK, val kiểu long long, giá trị 520, trả về long long

auto f3 = [val = "520"]() {
  return val;
};  // OK, val kiểu const char*, giá trị "520", trả về const char*

auto f4 = [val = "520"s]() {
  return val;
};  // OK, từ C++14 cần using namespace std; hoặc using namespace std::literals;
    // val kiểu std::string, giá trị std::string("520"), trả về std::string

auto f5 = [val = std::string("520")]() {
  return val;
};  // OK, val kiểu std::string, giá trị std::string("520"), trả về std::string

auto f6 = [val = std::vector<int>(3, 6)]() {
  return val;
};  // OK, val kiểu std::vector<int>, size 3, phần tử 6, trả về std::vector<int)

auto f7 = [val = 520]() -> int {
  return val;
};  // OK, val kiểu int, giá trị 520, trả về int

auto f8 = [val = 520]() -> long long {
  return val;
};  // OK, val kiểu int, giá trị 520, trả về long long
```

Khai báo biến mới không được bỏ giá trị khởi tạo, kiểu suy ra từ giá trị khởi tạo, tương đương:

```text
auto val = init-value;
```

Ví dụ sai:

```cpp
auto f = [val]() { return val; };  // Lỗi: ‘val’ chưa được khai báo trong
                                   // scope này
```

Giá trị khởi tạo cũng có thể là biến ngoài:

```cpp
int value = 520;
auto f = [val = value]() { return val; };
std::cout << f();  // Output: 520
```

`val` cũng có thể là kiểu tham chiếu, tham chiếu tới biến ngoài; nhờ vậy ta có thể đặt bí danh cho biến bị bắt theo tham chiếu:

```cpp
int value = 520;

auto f = [&val = value]() {
  return val;
};  // OK, val kiểu int&, trả về int, tương đương int& val = value;

std::cout << f() << '\n';  // Output: 520

value = 1314;

std::cout << f() << '\n';  // Output: 1314
```

Có thể đồng thời bắt biến ngoài và khai báo biến mới.

Nếu muốn sửa biến mới trong capture, cần `mutable` (trừ khi là tham chiếu):

```cpp
int value = 520;

{
  auto f = [val = value]() mutable -> int {
    return val = 1314;
  };  // Cần mutable
  auto val_f = f();
  std::cout << value << ' ' << val_f << std::endl;  // Output: 520 1314
}

{
  auto f = [&val = value]() -> int { return val = 1314; };  // Không cần mutable
  auto val_f = f();
  std::cout << value << ' ' << val_f << std::endl;  // Output: 1314 1314
}
```

Xem thêm [mutable可变规范](#mutable-可变规范).

Biến trong capture có vòng đời theo đối tượng nhận lambda, ở các ví dụ trên là biến `f`. Lambda là một lớp, nội dung capture là thành viên `private` của lớp đó:

```cpp
int main() {
  auto f = [val = 0]() mutable -> int { return ++val; };  // val được tạo và khởi tạo

  std::cout << f() << '\n';  // Output: 1
  std::cout << f() << '\n';  // Output: 2
  std::cout << f() << '\n';  // Output: 3
}  // val bị hủy theo f
```

### parameters danh sách tham số

Thông thường giống danh sách tham số của hàm:

```cpp
int x[] = {5, 1, 7, 6, 1, 4, 2};
std::sort(x, x + 7, [](int a, int b) { return (a > b); });
for (auto i : x) std::cout << i << " ";
```

Sẽ in ra mảng `x` theo thứ tự giảm dần.

Vì **parameters** là tùy chọn, nếu không truyền tham số và khai báo không có [mutable](#mutable-可变规范) và không có kiểu trả về hậu tố, thì có thể bỏ cặp ngoặc tròn rỗng.

??? note "Tham số khai báo bằng `auto`"
    **C++14** trở đi, nếu tham số dùng `auto`, sẽ tạo [lambda tổng quát](#泛型-lambdac14).

#### Tham số đối tượng tường minh (C++23)

Từ **C++23**, [tham số đối tượng tường minh](https://zh.cppreference.com/w/cpp/language/function#.E5.BD.A2.E5.8F.82.E5.88.97.E8.A1.A8) có thể dùng trong tham số lambda.

```cpp
auto nth_fibonacci = [](this auto self, unsigned n) -> unsigned {
  return n < 2 ? n : self(n - 1) + self(n - 2);
};

cout << nth_fibonacci(10u);
```

### mutable quy định khả biến

Cho phép thân hàm sửa các biến bị bắt theo giá trị.

```cpp
int a = 0;
auto by_value = [a]() mutable { ++a; };
auto by_ref = [&a] { ++a; };

by_value();
by_ref();
```

Sau khi gọi `by_value()`, biến bắt `a` trong `by_value` là 1, nhưng biến ngoài `a` vẫn là 0.
Sau khi gọi `by_ref()`, biến ngoài `a` trở thành 1.

### return-type kiểu trả về

Dùng để chỉ định kiểu trả về của lambda. Nếu bỏ, kiểu sẽ được suy luận (như hàm trả về `auto`).

Nếu có nhiều `return` với kiểu suy luận khác nhau sẽ lỗi biên dịch.

```cpp
auto lam = [](int a, int b) -> int { return 0; };

auto x1 = [](int i) { return i; };

auto x2 = [](bool condition) {
  if (condition) return 1;
  return 1.0;
};  // Lỗi, suy luận kiểu không一致
```

### Lambda tổng quát (C++14)

Dùng `auto` làm kiểu tham số để tạo lambda tổng quát:

```cpp
auto add = [](auto a, auto b) { return a + b; };
```

Trên [cpp insights](https://cppinsights.io) có thể thấy lớp lambda được tạo:

```cpp
class add_lambda {
 public:
  template <class T, class U>
  auto operator()(T a, U b) const {
    return a + b;
  }
};

add_lambda add{};
```

Hai tham số của `add` đều dùng `auto`, tương ứng là hai tham số mẫu `T`, `U` của hàm `operator()`.

### Đệ quy trong Lambda

Xem ví dụ biên dịch lỗi:

```cpp
int n = 10;

auto dfs = [&](int i) -> void {
  if (i == n)
    return;
  else
    dfs(i + 1);  // Lỗi: biến khai báo với auto
                 // không thể xuất hiện trong khởi tạo của chính nó
};
```

Ta cố bắt `dfs` trong capture, nhưng `dfs` có kiểu `auto`, phải chờ suy luận kiểu từ vế phải. Lambda muốn bắt `dfs` thì cần biết kiểu của `dfs` trước, gây vòng lặp vô hạn.

Cách giải:

1.  Chỉ định rõ kiểu `dfs`, dùng `std::function`.

    ???+ example "Sửa như sau:"
        ```cpp
        int n = 10;
        
        std::function<void(int)> dfs = [&](int i) -> void {
          if (i == n)
            return;
          else
            dfs(i + 1);  // OK
        };
        
        dfs(1);
        ```

    ??? warning "Không khuyến nghị dùng [`std::function`](./new.md#stdfunction) để đệ quy"
        Việc type erasure thường cần cấp phát thêm bộ nhớ, và gọi gián tiếp làm giảm hiệu năng.
        
        Trong [Benchmark](https://quick-bench.com/q/U5qf_dHHKsSyVU83jmt0p_U541c) với Clang 17, libc++, `std::function` chậm hơn lambda đệ quy khoảng 2.5 lần.
        
        ??? note "Mã тест"
            ```cpp
            #include <algorithm>
            #include <functional>
            #include <numeric>
            #include <random>
            
            using namespace std;
            
            const auto& nums = [] {
              random_device rd;
              mt19937 gen{rd()};
              array<unsigned, 32> arr{};
            
              std::iota(arr.begin(), arr.end(), 0u);
              ranges::shuffle(arr, gen);
            
              return arr;
            }();
            
            static void std_function_fib(benchmark::State& state) {
              std::function<int(int)> fib;
            
              fib = [&](int n) { return n <= 2 ? 1 : fib(n - 1) + fib(n - 2); };
            
              unsigned i = 0;
            
              for (auto _ : state) {
                auto res = fib(nums[i]);
                benchmark::DoNotOptimize(res);
            
                ++i;
            
                if (i == nums.size()) i = 0;
              }
            }
            
            BENCHMARK(std_function_fib);
            
            static void template_lambda_fib(benchmark::State& state) {
              auto n_fibonacci = [](const auto& self, int n) -> int {
                return n <= 2 ? 1 : self(self, n - 1) + self(self, n - 2);
              };
            
              unsigned i = 0;
            
              for (auto _ : state) {
                auto res = n_fibonacci(n_fibonacci, nums[i]);
                benchmark::DoNotOptimize(res);
            
                ++i;
            
                if (i == nums.size()) i = 0;
              }
            }
            
            BENCHMARK(template_lambda_fib);
            ```
2.  Không bắt `dfs`, mà truyền bằng tham số hàm.

    ???+ example "Sửa như sau:"
        ```cpp
        int n = 10;
        
        // Nếu danh sách tham số có auto, operator() sẽ là hàm mẫu,
        // có thể được instantiate khi gọi sau
        auto dfs = [&](auto& self,
                       int i) -> void  // [&] chỉ bắt biến dùng đến, nên không bắt auto dfs
        {
          if (i == n)
            return;
          else
            self(self, i + 1);  // OK
        };
        
        dfs(dfs, 1);
        ```

    ???+ note "Khác nhau giữa `auto self`, `auto& self`, `auto&& self`:"
        `auto& self` và `auto&& self` về lý thuyết đều chỉ dùng 8 byte (kích thước con trỏ) để truyền tham số, không tạo bản sao; còn `auto self` sẽ sao chép đối tượng, kích thước phụ thuộc capture vì là thành viên riêng của lớp lambda.
3.  Tự mở rộng lớp lambda hoặc viết tương tự để khai báo rõ kiểu `dfs`.

    ???+ example "Sửa như sau:"
        ```cpp
        int n = 10;
        
        class Lambda_1 {
         public:
          auto operator()(int i) const -> void {
            if (i == n)
              return;
            else
              (*this)(i + 1);  // OK
          }
        
          explicit Lambda_1(int& __n) : n(__n) {}
        
         private:
          int& n;
        } dfs(n);
        
        dfs(1);
        ```
4.  Nếu lambda không bắt biến nào, có thể dùng con trỏ hàm.

    Nếu lambda không bắt biến nào, nó có thể chuyển ngầm thành con trỏ hàm. Khi đó lambda có thể khai báo `static`, con trỏ hàm cũng `static`, nhờ vậy lambda có thể gọi con trỏ hàm để đệ quy.

    ???+ example "Ví dụ"
        ```cpp
        static unsigned (*fptr)(unsigned);
        
        static const auto lambda = [](const unsigned a) {
          return a < 2 ? a : (*fptr)(a - 2) + (*fptr)(a - 1);
        };
        
        static auto init = [] {
          fptr = +lambda;
          // Hoặc
          // fptr = static_cast<unsigned (*)(unsigned)>(lambda);
          return 0;
        }();
        
        cout << lambda(10);
        ```

### Ứng dụng của Lambda

#### Làm predicate cho thuật toán STL

Sắp xếp giảm dần:

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};
std::sort(v.begin(), v.end(), [](int a, int b) { return a > b; });
```

Dùng [std::find_if](https://zh.cppreference.com/w/cpp/algorithm/find) để tìm phần tử đầu tiên lớn hơn 3:

```cpp
std::vector<int> v = {1, 2, 3, 4, 5};
auto it = std::find_if(v.begin(), v.end(), [](int a) { return a > 3; });
```

#### Kiểm soát vòng đời biến trung gian

Trong thi đấu, có tình huống một biến cần được khởi tạo dựa trên biến trước đó, quá trình khởi tạo tạo ra biến trung gian lớn.

Ta muốn hủy sớm các biến trung gian này để giảm dùng bộ nhớ. Khi đó có thể dùng lambda để kiểm soát vòng đời.

```cpp
void solution(const vector<int>& input) {
  int b = [&] {
    vector<int> large_objects(input.size());
    int c = 0;

    for (int i = 0; i < large_objects.size(); ++i)
      large_objects[i] = i + input[i];

    for (int i = 0; i < input.size(); ++i) c += large_objects[input[i]];

    return c;
  }();

  // ...
}
```

So với dùng khối phạm vi, lambda cho phép trả về giá trị, làm mã gọn hơn; so với hàm, ta không cần đặt tên và khai báo tham số bị bắt, nên mã ngắn gọn hơn.

## Tài liệu tham khảo

-   [cppreference-lambda](https://en.cppreference.com/w/cpp/language/lambda)
-   [Stackoverflow: Overhead with std::function](https://stackoverflow.com/a/33881130/11120338)
