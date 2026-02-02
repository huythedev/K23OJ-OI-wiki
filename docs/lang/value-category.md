Giá trị phân loại (value category) là một khái niệm rất quan trọng trong C++, tuy trong thi đấu thuật toán có thể ít dùng, nhưng hiểu rõ sẽ giúp ta phát hiện và tránh những bản sao không cần thiết, từ đó cải thiện hiệu quả và hiệu năng.

Khái niệm này trong C, C++98, C++11 và C++17 đã trải qua nhiều lần phát triển, dần trở nên khá phức tạp.

## Sao chép không cần thiết

Xét quá trình đưa chuỗi vào vector:

```cpp
int main() {
  std::vector<std::string> vec;
  vec.reserve(3);
  for (int i = 0; i < 3; ++i) {
    std::string str;
    std::cin >> str;
    vec.push_back(str);
  }
  return 0;
}
```

Có thể thấy trong quá trình chuyển, chuỗi được lưu cả ở `str` và `vec`, khiến bộ nhớ tăng gấp đôi.

Nếu muốn tiết kiệm phần bộ nhớ này, ta có thể tự viết một thao tác “di chuyển” đơn giản: tự định nghĩa struct `MyString`, bên trong có con trỏ trỏ tới chuỗi; ta chỉ cần sao chép con trỏ và cẩn thận dọn con trỏ của đối tượng nguồn để tránh bị giải phóng sai.

```cpp
struct MyString {
  char *beg, *end;
  // ...
};

void move_to(MyString &src, MyString &dst) {
  dst.beg = src.beg;
  dst.end = src.end;
  src.beg = src.end = nullptr;
}
```

Do nhu cầu chuyển đối tượng hiệu quả rất thường gặp, và việc này tương tác với C++ (khởi tạo, hủy, v.v.) khá khó, C++11 đã đưa ngữ nghĩa di chuyển (move semantics) vào lõi ngôn ngữ.

## Giá trị phân loại trong C

Trong chuẩn C, “đối tượng” là khái niệm tổng quát hơn biến, chỉ một vùng nhớ có địa chỉ. Thuộc tính chính gồm: kích thước, kiểu hiệu lực, giá trị và định danh. Định danh là tên biến, giá trị là ý nghĩa của vùng nhớ khi diễn giải theo kiểu. Ví dụ `int` và `float` đều dùng 4 byte, nhưng cùng một vùng nhớ sẽ được diễn giải khác nhau.

Trong C, mỗi biểu thức đều có kiểu và giá trị phân loại. Giá trị phân loại có ba loại chính:

-   Lvalue: biểu thức ngầm chỉ một đối tượng, tức có thể lấy địa chỉ.
-   Rvalue: biểu thức không chỉ đối tượng, tức không có vị trí lưu trữ, không thể lấy địa chỉ.
-   Hàm chỉ định (function designator): biểu thức có kiểu hàm.

Vì vậy, chỉ lvalue có thể sửa (lvalue không có `const` và không phải mảng) mới có thể nằm bên trái phép gán.

Với toán tử yêu cầu rvalue làm toán hạng, mỗi khi lvalue được dùng, sẽ áp dụng chuyển đổi chuẩn lvalue-to-rvalue, array-to-pointer hoặc function-to-pointer để biến nó thành rvalue.

Các hiểu nhầm thường gặp:

-   Một rvalue khi tiếp tục toán có thể cho ra lvalue. Ví dụ `int *a`, biểu thức `a + 1` là rvalue, nhưng `*(a + 1)` là lvalue.
-   Chỉ biểu thức mới có giá trị phân loại, còn biến thì không. Ví dụ `int *a`, không thể nói “biến `a` là lvalue”, mà nói biểu thức `a` là lvalue.

## Giá trị phân loại trong C++98

C++98 gần như giống C, nhưng thêm một số quy tắc:

-   Hàm là lvalue vì có thể lấy địa chỉ.
-   Tham chiếu lvalue (T&) là lvalue vì có thể lấy địa chỉ.
-   Chỉ `const T&` mới có thể bind vào rvalue.

### Loại bỏ bản sao

C++ cho phép trình biên dịch thực hiện loại bỏ bản sao (Copy Elision) để giảm tạo/hủy đối tượng tạm.

Ví dụ sau sẽ kích hoạt RVO (Return Value Optimization), bạn chỉ thấy một lần khởi tạo và một lần copy constructor, kể cả khi khởi tạo/hủy có tác dụng phụ:

```cpp
struct X {
  X() { std::puts("X::X()"); }

  X(const X &) { std::puts("X::X(const X &)"); }

  ~X() { std::puts("X::~X()"); }
};

X get() {
  X x;
  return x;
}

int main() {
  X x = get();
  X y = X(X(X(X(x))));
  return 0;
}
```

## Giá trị phân loại trong C++11

C++11 đưa vào ngữ nghĩa di chuyển và tham chiếu rvalue (`T&&`), gồm move constructor và move assignment. Điều này cho phép tận dụng đối tượng tạm.

`move_to` ở trên có thể viết lại như sau:

```cpp
struct MyString {
  // ...
  MyString(MyString&& other) {
    beg = other.beg;
    end = other.end;
    other.beg = other.end = nullptr;
  }
};
```

Giờ đặc tính biểu thức ta quan tâm thêm một điểm:

-   Có “định danh” hay không: có chỉ một đối tượng (có địa chỉ).
-   Có thể di chuyển hay không: có move constructor/assignment để tận dụng đối tượng tạm.

Do đó có ba loại giá trị:

-   Có định danh, không thể di chuyển: lvalue.
-   Có định danh, có thể di chuyển: xvalue.
-   Không có định danh, có thể di chuyển: prvalue.
-   Không có định danh, không thể di chuyển: không sử dụng được.

Ngoài ra C++11 còn có hai loại tổng hợp:

-   Có định danh: glvalue (gồm lvalue và xvalue).
-   Có thể di chuyển: rvalue (gồm prvalue và xvalue).

### std::move

Để phối hợp với ngữ nghĩa di chuyển, C++11 có hàm `std::move`, dùng để ép lvalue thành rvalue nhằm kích hoạt move semantics.

```cpp
int main() {
  std::vector<int> a = {1, 2, 3};
  std::cout << "a: " << a.data() << std::endl;
  std::vector<int> b = a;
  std::cout << "b: " << b.data() << std::endl;
  std::vector<int> c = std::move(b);
  std::cout << "c: " << c.data() << std::endl;
}
```

Vì vậy chỉ cần đổi `push_back(str)` thành `push_back(std::move(str))` là tránh được sao chép.

```cpp
int main() {
  std::vector<std::string> vec;
  vec.reserve(3);
  for (int i = 0; i < 3; ++i) {
    std::string str;
    std::cin >> str;
    vec.push_back(std::move(str));
    // Một cách viết khéo khác, cần C++17
    // std::cin >> vec.emplace_back();
  }
  return 0;
}
```

> Do `std::string` có tối ưu chuỗi nhỏ (SSO), chuỗi ngắn được lưu trực tiếp trong struct, nên có thể phải nhập chuỗi đủ dài mới quan sát được tính bất biến của con trỏ `data`.

## Giá trị phân loại trong C++17

C++17 tiếp tục đơn giản hóa:

-   Lvalue: có định danh, không thể di chuyển.
-   Xvalue: có định danh, có thể di chuyển.
-   Prvalue: khởi tạo đối tượng.

C++11 đã mở rộng copy elision sang move. Trong đoạn code sau, nếu trình biên dịch bật RVO thì `urvo` sẽ không có move.

C++17 yêu cầu prvalue không nhất thiết phải “thực thể hóa” (materialize), mà được xây trực tiếp vào vùng nhớ đích; trước khi xây dựng thì đối tượng còn chưa tồn tại. Vì vậy trong C++17 ta không có “bước trả về” này và không phải phụ thuộc RVO. Có thể coi như bắt buộc URVO, nhưng NRVO vẫn không bắt buộc.

```cpp
std::string urvo() { return std::string("123"); }

std::string nrvo() {
  std::string s;
  s = "123";
  std::cout << s;
  return s;
}

int main() {
  std::string str = urvo();  // Xây trực tiếp
  std::string str = nrvo();  // Không chắc xây trực tiếp, phụ thuộc tối ưu
}
```

C++17 cũng đưa vào cơ chế “thực thể hóa tạm” (temporary materialization) khi ta cần truy cập thành viên, gọi hàm thành viên… là các tình huống cần glvalue; khi đó sẽ ngầm chuyển thành xvalue.

### Hiểu nhầm thường gặp

Trong ví dụ sau:

-   Ở `f1`, trả về `std::move(x)` là dư thừa, không giúp tăng hiệu năng mà còn có thể cản trở NRVO.
-   Ở `f2`, trả về `std::move(x)` là nguy hiểm vì trả về rvalue reference trỏ tới biến cục bộ đã bị hủy, tạo tham chiếu treo.

```cpp
std::string f1() {
  std::string s = "123";
  // Tương đương return std::string(std::move(s))
  return std::move(s);
}

std::string&& f2() {
  std::string s = "123";
  return std::move(s);
}
```

## Tài liệu tham khảo và đọc thêm

1.  [Value categories](https://en.cppreference.com/w/cpp/language/value_category)
2.  [Wording for guaranteed copy elision through simplified value categories](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2016/p0135r1.html)
3.  [C++ 中的值类别](https://paul.pub/cpp-value-category/)
4.  [C++ 的右值引用、移动和值类别系统，你所需要的一切](https://zclll.com/index.php/cpp/value_category.html)
5.  [Copy elision](https://en.cppreference.com/w/cpp/language/copy_elision)
