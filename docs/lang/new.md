**Lưu ý**: Xét theo thực tế thi đấu thuật toán, bài này sẽ không khảo sát toàn bộ cú pháp, chỉ trình bày những phần có thể dùng trong thi đấu.

Cú pháp trong bài tham chiếu chuẩn **C++11**. Những chỗ có khác biệt về ngữ nghĩa sẽ lấy **C++11** làm chuẩn; các cú pháp C++14, C++17... sẽ được nhắc đến khi phù hợp và có ghi chú rõ.

## Từ khóa `auto`

`auto` dùng để tự động suy luận kiểu của biến, ví dụ:

```cpp
auto a = 1;        // a là kiểu int
auto b = a + 0.1;  // b là kiểu double
```

Lưu ý `auto` sẽ bỏ tham chiếu; nếu không muốn phát sinh chi phí copy, cần chỉ định thủ công:

```cpp
int a = 1;
int& b = a;
auto c = b;   // c là kiểu int, có chi phí copy
auto& e = a;  // e là kiểu int&, không có chi phí copy
```

## Từ khóa decltype

`decltype` có thể suy luận kiểu dựa trên **thực thể** hoặc **biểu thức**, lưu ý hai cách suy luận khác nhau, dùng sai có thể gây tham chiếu treo. Trong thi đấu ít dùng, ở đây chỉ giới thiệu sơ lược.

```cpp
#include <iostream>
#include <vector>

int main() {
  int a = 1926;
  decltype(a) b;                 // Suy luận từ thực thể, b là kiểu int
  decltype(1 + 1) c;             // Suy luận từ biểu thức, c là kiểu int
  decltype((a)) d = a;           // Suy luận từ biểu thức, d là kiểu int&!
  std::vector<decltype(b)> vec;  // Suy luận từ thực thể, vec là std::vector <int>
  return 0;
}
```

## constexpr

> Xem thêm [biểu thức hằng constexpr (C++11)](const.md#常量表达式-constexprc11)

## Vòng lặp `for` theo phạm vi

Dùng for theo phạm vi để duyệt đối tượng có thể lặp, hiệu suất tương đương duyệt bằng iterator. Hai cách này thường tốt hơn duyệt theo chỉ số vì không cần định vị theo index.

Cú pháp đơn giản của vòng for theo phạm vi:

```cpp
for (item_declaration : range_initializer) statement
```

Ví dụ:

```cpp
std::array<int, 4> arr = {1, 2, 3, 4};
for (int x : arr) {
  std::cout << x << std::endl;
}
```

Hiệu quả tương đương đoạn sau:

```cpp
std::array<int, 4> arr = {1, 2, 3, 4};
for (auto px = arr.begin(), ed = arr.end(); px != ed; ++px) {
  std::cout << *px << std::endl;
}
```

### Khai báo item-declaration

Khai báo một biến để nhận phần tử trong container bên phải, kiểu biến phải phù hợp với kiểu phần tử. Có thể dùng `auto` để suy luận, kiểu phức tạp thường dùng `auto&` để tránh copy.

### Khởi tạo phạm vi range-initializer

Phạm vi có thể là bất kỳ đối tượng có thể lặp nào (mảng, hoặc lớp có hàm `begin` và `end`). Nếu truyền vào một biểu thức, biểu thức chỉ được tính một lần.

Ví dụ:

```cpp
int a[] = {1, 1, 4, 5, 1, 4};
std::vector<int> b{1, 1, 4, 5, 1, 4};
std::map<std::string, int> c{{"114", 114}, {"514", 514}};
for (int i : a) std::cout << i;
for (auto i : b) std::cout << i;
// Kiểu của i bên dưới là std::pair<const std::string, int>&
for (auto& i : c) std::cout << i.first << i.second;
for (auto i : {1, 1, 4, 5, 1, 4}) std::cout << i;
```

### Tự định nghĩa kiểu hỗ trợ for theo phạm vi

Chỉ cần cung cấp hàm thành viên `begin` và `end`, kiểu trả về phải hỗ trợ so sánh, tăng và giải tham chiếu (`*`).

Ví dụ:

```cpp
#include <iostream>

struct C {
  int a[4];

  int* begin() { return a; }

  int* end() { return a + 4; }
};

int main() {
  C c = {1, 9, 2, 6};
  for (auto i : c) std::cout << i << " ";
  std::cout << std::endl;
  // output: 1 9 2 6
  return 0;
}
```

### Câu lệnh khởi tạo (C++20)

Trong C++20 có thể dùng câu lệnh khởi tạo để làm một số việc như bộ đếm:

```cpp
#include <iostream>
#include <vector>

int main() {
  std::vector<int> v = {0, 1, 2, 3, 4, 5};

  for (int counter = 0; auto i : v)  // câu lệnh khởi tạo (C++20)
    std::cout << counter++ << ' ' << i << std::endl;
}
```

## Ràng buộc cấu trúc (C++17)

Ràng buộc cấu trúc (Structured binding) là cú pháp đường tắt của C++17 để trích xuất phần tử hoặc tham chiếu phần tử, ví dụ:

```cpp
struct C {
  int x{1}, y{2};
};

int arr[]{4, 5, 6};

auto [c1, c2] = C{};       // c1=1,c2=2; kiểu int
auto& [a1, a2, a3] = arr;  // a1=arr[0],a2=arr[1],a3=arr[2]; kiểu int&
```

Lưu ý:

-   Số biến bên trái phải bằng số phần tử con của đối tượng bên phải
-   Khai báo kiểu cần dùng `auto`
-   Có thể thêm `&` để lấy tham chiếu

Khi duyệt `map` có thể viết:

```cpp
std::map<std::string, int> m = {{"k1", 1}, {"k2", 2}};

// Dùng "auto&" để không có chi phí copy
for (auto& [k, v] : m) {
  // k có kiểu const std::string& do khóa có const
  // v có kiểu int&
  std::cout << k << ' ' << v << std::endl;
}
```

## std::tuple

[Tuple](https://zh.cppreference.com/w/cpp/utility/tuple) được định nghĩa trong `<tuple>`, là mở rộng của `std::pair`, có thể lưu nhiều giá trị kiểu khác nhau. Ví dụ:

```cpp
#include <iostream>
#include <tuple>
#include <vector>

constexpr auto expr = 4 - 1;  // expr = 3

int main() {
  std::vector<int> vec = {1, 9, 2, 6, 0};
  std::tuple<int, int, std::string, std::vector<int>> tup =
      std::make_tuple(817, 114, "514", vec);

  // Dùng get<> để lấy phần tử, bên trong <> phải là biểu thức hằng số nguyên
  for (auto i : std::get<expr>(tup)) std::cout << i << " ";
  // Chỉ số phần tử đầu là 0, nên std::get<3> cho ta một std::vector<int>
  return 0;
}
```

Từ C++17 có thể dùng structured binding để lấy giá trị:

```cpp
std::vector<int> vec = {1, 9, 2, 6, 0};
std::tuple<int, int, std::string, std::vector<int>> tup =
    std::make_tuple(817, 114, "514", vec);

auto& [a, b, c, d] = tup;  // Structured binding C++17
std::cout << a << ' ' << b << c << std::endl;
std::cout << d.size() << ' ' << d[2] << std::endl;
```

### Hàm thành viên

| Hàm          | Tác dụng                   |
| ----------- | -------------------- |
| `operator=` | Gán nội dung của một `tuple` cho một `tuple` khác |
| `swap`      | Hoán đổi nội dung của hai `tuple`     |

Ví dụ:

```cpp
constexpr std::tuple<int, int> tup = {1, 2};
std::tuple<int, int> tupA = {2, 3}, tupB;
tupB = tup;
tupB.swap(tupA);
```

### Hàm không thành viên

| Hàm             | Tác dụng                           |
| -------------- | ---------------------------- |
| `make_tuple`   | Tạo một `tuple`, kiểu được xác định theo kiểu các đối số |
| `std::get`     | Truy cập phần tử của tuple                  |
| `std::tie`     | Gán phần tử tuple cho các biến có sẵn                |
| `operator==` ... | So sánh `tuple` theo thứ tự từ điển          |
| `std::swap`    | Thuật toán `std::swap` được đặc hóa           |

Ví dụ:

```cpp
std::tuple<int, int> tupA = {2, 3}, tupB;
tupB = std::make_tuple(1, 2);
std::swap(tupA, tupB);
std::cout << std::get<1>(tupA) << std::endl;
int x;
std::tie(x, std::ignore) = tupB;
std::cout << x << std::endl;
```

`std::tie` gán phần tử tuple vào biến có sẵn, có thể dùng `std::ignore` để bỏ qua phần tử không cần. Structured binding tạo biến mới (hỗ trợ gán theo giá trị/tham chiếu), và phải nhận tất cả phần tử.

## Hàm đối tượng

Đối tượng có thể dùng toán tử gọi hàm `operator()` được gọi là hàm đối tượng (FunctionObject).

Đây không phải là một tính năng của ngôn ngữ, mà là một [khái niệm/yêu cầu](https://zh.cppreference.com/w/cpp/named_req/FunctionObject), được dùng rộng rãi trong thư viện chuẩn.

Hàm đối tượng có thể chia thành hai loại:

1.  Con trỏ hàm
2.  Đối tượng lớp có nạp chồng `operator()`

[lambda](./lambda.md) là ví dụ điển hình của loại thứ hai: nó lưu phần bắt vào thành biến thành viên, và nạp chồng toán tử gọi hàm.

## Biểu thức Lambda

> Xem [biểu thức Lambda](lambda.md).

## std::function

???+ warning "Lưu ý chi phí hiệu năng"
    `std::function` có thể gây chi phí hiệu năng, theo [Benchmark](./lambda.md#lambda-中的递归) thường làm chậm 2 đến 3 lần hoặc hơn.
    
    Vì nó dùng kỹ thuật type erasure, thường được thực hiện qua cơ chế hàm ảo; gọi hàm ảo gây [chi phí](https://stackoverflow.com/questions/5057382/what-is-the-performance-overhead-of-stdfunction).
    
    Hãy cân nhắc dùng [**biểu thức Lambda**](./lambda.md) hoặc [**hàm đối tượng**](#函数对象) thay thế.

`std::function` là một bộ bao hàm tổng quát, định nghĩa trong `<functional>`.

Một đối tượng `std::function` có thể lưu, sao chép và gọi bất kỳ đối tượng [**có thể gọi**](https://zh.cppreference.com/w/cpp/named_req/Callable) nào, bao gồm [**lambda**](./lambda.md), con trỏ hàm thành viên hoặc các [**hàm đối tượng**](#函数对象) khác.

Nếu `std::function` không chứa đối tượng có thể gọi (ví dụ được khởi tạo mặc định), khi gọi sẽ ném ngoại lệ [`std::bad_function_call`](https://zh.cppreference.com/w/cpp/utility/functional/bad_function_call).

```cpp
#include <functional>
#include <iostream>

struct Foo {
  Foo(int num) : num_(num) {}

  void print_add(int i) const { std::cout << num_ + i << '\n'; }

  int num_;
};

void print_num(int i) { std::cout << i << '\n'; }

struct PrintNum {
  void operator()(int i) const { std::cout << i << '\n'; }
};

int main() {
  // Lưu hàm tự do
  std::function<void(int)> f_display = print_num;
  f_display(-9);

  // Lưu lambda
  std::function<void()> f_display_42 = []() { print_num(42); };
  f_display_42();

  // Lưu lời gọi đến hàm thành viên
  std::function<void(const Foo&, int)> f_add_display = &Foo::print_add;
  const Foo foo(314159);
  f_add_display(foo, 1);
  f_add_display(314159, 1);

  // Lưu lời gọi đến thành viên dữ liệu
  std::function<int(Foo const&)> f_num = &Foo::num_;
  std::cout << "num_: " << f_num(foo) << '\n';

  // Lưu lời gọi đến hàm đối tượng
  std::function<void(int)> f_display_obj = PrintNum();
  f_display_obj(18);
}
```

## Hàm mẫu tham số biến đổi

Trước C++11, template lớp và hàm chỉ nhận số lượng tham số template cố định. C++11 cho phép **bất kỳ số lượng, bất kỳ kiểu** tham số template.

Ở đây chỉ giới thiệu ngắn gọn **hàm** mẫu tham số biến đổi.

Hàm mẫu `fun` sau có thể nhận số lượng bất kỳ và kiểu bất kỳ các tham số template:

```cpp
template <typename... Clazz>
void fun(Clazz... paras) {}
```

`paras` là một gói tham số hàm (function parameter pack), nhận 0 hoặc nhiều tham số hàm. `Clazz` là một gói tham số template (template parameter pack), nhận 0 hoặc nhiều đối số template (không kiểu, kiểu hoặc template), khi dùng `typename` thì chỉ nhận kiểu.

Có thể hiểu đơn giản:

-   Gói tham số template thường là các tên kiểu (nhưng cũng có thể là hằng số biên dịch hoặc tên template)
-   Gói tham số hàm thường là các tên biến

Khi đó có thể gọi `fun` như sau:

```cpp
fun();
fun(1);
fun(1, 2, 3);
fun(1, 0.0, "abc");
```

### Mở rộng gói tham số

#### Cú pháp mở rộng gói

Mở rộng gói tham số rất đơn giản, dùng `...` để mở rộng và tự động dùng `,` để phân tách. Ví dụ:

```cpp
template <class A, class... C>
void func(A arg1, C... arg2) {
  // C là gói tham số template
  tuple<A, C...>();  // Mở rộng thành tuple<int, int, double, bool>();

  // arg2 là gói tham số hàm
  func(arg2...);  // Mở rộng thành func( 2, 1.1, true );
}

func(1, 2, 1.1, true);
```

Mở rộng gói còn có thể kèm theo toán tử:

```cpp
template <class A, class... C>
void func(A arg1, C... arg2) {
  func((arg2 + 1)...);
  // Mở rộng thành func( (2+1) , (1.1+1), (2.1f+1) );
}

func(1, 2, 1.1, 2.1f);
```

#### Hàm kết thúc

Hàm phía trên không thể chạy vì số lượng tham số giảm dần, cuối cùng thành rỗng và báo lỗi.

Cần chỉ định điều kiện dừng, bằng cách viết một hàm thường:

```cpp
void func() {}

template <class A, class... C>
void func(A arg1, C... arg2) {
  std::cout << arg1 << std::endl;
  func((arg2 + 1)...);
}

func(1, 2, 1.1, 2.1f);
```

Như vậy, khi số tham số không bằng 0 sẽ gọi template, còn rỗng thì gọi hàm thường, chương trình chạy bình thường.

### Biểu thức gập (C++17)

C++17 cung cấp cú pháp đơn giản để xử lý **gói tham số hàm**, cú pháp như sau (bắt buộc có ngoặc):

1.  `( pack op ... )` sẽ thành `(E1 op (... op (EN-1 op EN)))`
2.  `( ... op pack )` sẽ thành `(((E1 op E2) op ...) op EN)`
3.  `( pack op ... op init )` sẽ thành `(E1 op (... op (EN−1 op (EN op I))))`
4.  `( init op ... op pack )` sẽ thành `((((I op E1) op E2) op ...) op EN)`

Ví dụ đơn giản:

```cpp
template <class... C>
void func(C... args) {
  (std::cout << ... << args) << std::endl;
  // Cú pháp 4, tương đương ↓
  // ( ( ( std::cout << 1 ) << 2.1 ) << true ) << std::endl;
  // Kết quả: 12.11  Lưu ý true in thành 1 vì không có boolalpha

  std::cout << (args && ...) << std::endl;
  // Cú pháp 1, tương đương ↓
  // std::cout << ( 1 && ( 2.1 && true ) ) ) << std::endl;
  // Kết quả: 1
}

func(1, 2.1, true);
```

### Hàm mẫu rút gọn (C++20)

Từ C++20 có thể dùng trực tiếp `auto ...` làm kiểu tham số để rút gọn hàm mẫu:

```cpp
void func(auto... args) { (std::cout << ... << args) << std::endl; }
```

Lưu ý về bản chất vẫn là hàm mẫu, tương đương:

```cpp
template <class... T>
void func(T... args) {
  (std::cout << ... << args) << std::endl;
}
```

## Thư viện ranges (C++20)

> Thư viện ranges mở rộng iterator và thuật toán generic, giúp tổ hợp iterator/thuật toán mạnh hơn và giảm lỗi.

Range là dãy có thể duyệt, gồm mảng, container, view...

Khi cần thao tác phức tạp trên container/range, [ranges](https://zh.cppreference.com/w/cpp/ranges) giúp code dễ viết và rõ ràng hơn.

### View (góc nhìn)

View là đối tượng nhẹ, dùng cơ chế riêng (như iterator tùy biến) để thực hiện thuật toán, cung cấp thêm cách duyệt cho range.

Trong ranges đã có nhiều view, chia thành hai loại:

1.  **Range factory**: tạo ra range đặc biệt, giúp khỏi tự dựng container, giảm chi phí.
2.  **Range adaptor**: cung cấp nhiều kiểu duyệt, vừa có thể gọi như hàm, vừa dùng toán tử ống `|` để nối, tạo chuỗi.

**Range adaptor** là [**range adaptor closure object**](https://zh.cppreference.com/w/cpp/named_req/RangeAdaptorClosureObject), cũng thuộc [**function object**](#函数对象), chúng nạp chồng `operator|` để ghép như ống.

??? note "Toán tử ống"
    `|` ở đây nên hiểu là toán tử ống, không phải OR bit; cách dùng này lấy từ [pipe](https://zh.wikipedia.org/wiki/%E7%AE%A1%E9%81%93_%28Unix%29) trong Linux.

Khi thao tác phức tạp vẫn giữ được tính dễ đọc, có các tính chất:

Nếu A, B, C là các range adaptor closure object, R là một range, các chữ khác là tham số hợp lệ, thì

    R | A(a) | B(b) | C(c, d)

tương đương

    C(B(A(R, a), b), c, d)

Ví dụ với `ranges::take_view` và `ranges::iota_view`:

```cpp
#include <iostream>
#include <ranges>

int main() {
  const auto even = [](int i) { return 0 == i % 2; };

  for (int i : std::views::iota(0, 6) | std::views::filter(even))
    std::cout << i << ' ';
}
```

1.  Range factory `std::views::iota(0, 6)` tạo dãy số từ 0 đến 5
2.  Range adaptor `std::views::filter(even)` lọc range trước, chỉ còn số chẵn
3.  Hai thao tác nối bằng toán tử ống

Đoạn code này không cần cấp phát heap để lưu mỗi bước, việc sinh và lọc diễn ra trong lúc duyệt (cụ thể là tạo iterator, tăng, giải tham chiếu), tức zero overhead.

Đồng thời, vòng đời của range đầu vào tương đương vòng đời các phần tử trong **range adaptor**. Nếu range bên ngoài (container, range factory) đã bị hủy, thì duyệt view sẽ như giải tham chiếu con trỏ treo, là hành vi không xác định.

Để tránh điều này, cần đảm bảo vòng đời của adaptor nằm trong vòng đời của bất kỳ range nào mà nó dùng.

???+ note "Range bị hủy thì phần tử trong view đều bị treo"
    ```cpp
    #include <iostream>
    #include <ranges>
    #include <vector>
    
    using namespace std;
    
    int main() {
      auto view = [] {
        vector<int> vec{1, 2, 3, 4, 5};
        return vec | std::views::filter([](int i) { return 0 == i % 2; });
      }();
    
      for (int i : view) cout << i << ' ';  // Hành vi không xác định lúc chạy
    
      return 0;
    }
    ```

### Thuật toán có ràng buộc (Constrained Algorithm)

> C++20 cung cấp phiên bản ràng buộc của đa số thuật toán trong không gian tên std::ranges, có thể nhận cặp iterator-sentinel hoặc một range làm tham số, hỗ trợ projection và callable trỏ tới thành viên. Ngoài ra còn thay đổi kiểu trả về để chứa các thông tin hữu ích trong quá trình chạy.

Các thuật toán này có thể hiểu là bản cải tiến của thuật toán thư viện cũ, đều là function object, cung cấp overload và kiểm tra kiểu tham số tốt hơn (dựa trên [`concept`](https://zh.cppreference.com/w/cpp/language/constraints)). Trước tiên so sánh `std::sort` và `ranges::sort`:

```cpp
#include <algorithm>
#include <iostream>
#include <vector>

using namespace std;

int main() {
  vector<int> vec{4, 2, 5, 3, 1};

  sort(vec.begin(), vec.end());  // {1, 2, 3, 4, 5}

  for (const int i : vec) cout << i << ", ";
  cout << '\n';

  ranges::sort(vec, ranges::greater{});  // {5, 4, 3, 2, 1}

  for (const int i : vec) cout << i << ", ";

  return 0;
}
```

`ranges::sort` và `sort` có cùng cài đặt, nhưng cung cấp overload dựa trên range nên truyền tham số gọn hơn. Các thuật toán khác trong `std` phần lớn cũng có phiên bản range tương ứng trong `ranges`.

Dùng các tham số range này cùng với view ở phần trước giúp thao tác phức tạp mà vẫn dễ đọc. Ví dụ:

```cpp
#include <algorithm>
#include <array>
#include <iostream>
#include <ranges>

using namespace std;

int main() {
  const auto& inputs = views::iota(0u, 9u);  // Tạo dãy 0 đến 8
  const auto& chunks = inputs | views::chunk(3);  // Chia dãy thành các khối, mỗi khối 3 phần tử
  const auto& cartesian_product =
      views::cartesian_product(chunks, chunks);  // Lấy tích Descartes của các khối với chính nó

  for (const auto [l_chunk, r_chunk] : cartesian_product)
    // Tính tổng các phần tử trong hai khối ở tích Descartes
    cout << ranges::fold_left(l_chunk, 0u, plus{}) +
                ranges::fold_left(r_chunk, 0u, plus{})
         << ' ';
}
```

???+ note "Kết quả:"
    6 15 24 15 24 33 24 33 42

## Tài liệu tham khảo

1.  [C++ Reference](https://zh.cppreference.com/)
