> Khai báo một biến có tên là tham chiếu, tức là bí danh của một đối tượng hoặc hàm đã tồn tại.

Tham chiếu có thể coi là con trỏ không rỗng được đóng gói trong C++, dùng để truyền đối tượng mà nó trỏ tới, và khi khai báo phải trỏ tới một đối tượng.

Tham chiếu không phải là đối tượng, nên không có mảng tham chiếu, không thể lấy con trỏ tới tham chiếu, cũng không có tham chiếu của tham chiếu.

??? note "Kiểu tham chiếu không thuộc kiểu đối tượng"
    Nếu muốn tham chiếu hỗ trợ sao chép/gán như đối tượng thường (ví dụ làm phần tử container), cần [`reference_wrapper`](https://zh.cppreference.com/w/cpp/utility/functional/reference_wrapper), thường được cài bằng một con trỏ không rỗng.

Tham chiếu chủ yếu gồm hai loại: tham chiếu trái (lvalue) và tham chiếu phải (rvalue).

??? note "Lvalue và rvalue"
    Xem giải thích tại [phân loại giá trị](./value-category.md).

## Tham chiếu trái T&

Tham chiếu thường gặp là tham chiếu trái, ràng buộc vào lvalue; tham chiếu trái có `const` có thể ràng buộc rvalue. Dưới đây là ví dụ từ [tham khảo](https://zh.cppreference.com/w/cpp/language/reference):

```cpp
#include <iostream>
#include <string>

int main() {
  std::string s = "Ex";
  std::string& r1 = s;
  const std::string& r2 = s;

  r1 += "ample";  // sửa r1 tức là sửa s
  // r2 += "!"; // lỗi: không thể sửa qua tham chiếu const
  std::cout << r2 << '\n';  // in r2, truy cập s, ra "Example"
}
```

Tham chiếu trái dùng nhiều nhất ở tham số hàm để tránh copy không cần thiết.

```cpp
#include <iostream>
#include <string>

// s là tham chiếu, gọi hàm không tạo bản sao
char& char_number(std::string& s, std::size_t n) {
  s += s;  // 's' với 'str' trong main() là cùng một đối tượng
           // cũng cho thấy lvalue có thể nằm bên phải dấu '='
  return s.at(n);  // string::at() trả về tham chiếu char
}

int main() {
  std::string str = "Test";
  char_number(str, 1) = 'a';  // hàm trả về lvalue, có thể gán
  std::cout << str << '\n';   // in "TastTest"
}
```

## Tham chiếu phải T&&（C++ 11）

Tham chiếu phải ràng buộc vào rvalue, dùng để di chuyển đối tượng và **kéo dài vòng đời tạm**.

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
  string s1 = "Test";
  // string&& r1 = s1; // lỗi: không thể ràng buộc lvalue, cần std::move hoặc static_cast

  const string& r2 = s1 + s1;  // OK: tham chiếu trái const kéo dài vòng đời
  // r2 += "Test"; // lỗi: không thể sửa qua tham chiếu const
  cout << r2 << '\n';

  string&& r3 = s1 + s1;  // OK: tham chiếu phải kéo dài vòng đời
  r3 += "Test";
  cout << r3 << '\n';

  const string& r4 = r3;  // tham chiếu phải có thể chuyển thành tham chiếu trái const
  cout << r4 << '\n';

  string& r5 = r3;  // tham chiếu phải có thể chuyển thành tham chiếu trái
  cout << r5 << '\n';
}
```

## Tham chiếu treo (dangling)

Khi đối tượng được tham chiếu bị hủy, tham chiếu trở thành dangling; truy cập là hành vi không xác định, có thể crash.

Ví dụ thường gặp:

-   Tham chiếu biến cục bộ

    ```cpp
    #include <iostream>

    int& foo() {
      int a = 1;
      return a;
    }

    int main() {
      int& b = foo();
      std::cout << b << std::endl;  // hành vi không xác định
    }
    ```

-   Treo do giải phóng

    ```cpp
    #include <iostream>

    int main() {
      int* ptr = new int(10);
      int& ref = *ptr;
      delete ptr;

      std::cout << ref << std::endl;  // hành vi không xác định
    }
    ```

-   Treo do tái cấp phát

    ```cpp
    #include <iostream>

    int main() {
      std::string str = "hello";

      const char& ref = str.front();

      str.append("world");  // có thể tái cấp phát, ref trỏ tới bộ nhớ đã bị giải phóng

      std::cout << ref << std::endl;  // hành vi không xác định
    }
    ```

    Các thao tác chèn của `std::vector`, `std::unordered_map`... đều có thể gây tái cấp phát.

Khi dùng tham chiếu, luôn phải chú ý vòng đời đối tượng, tránh dangling.

Công cụ phân tích tĩnh và thói quen tốt giúp tránh lỗi này.

## Mẹo tối ưu liên quan đến tham chiếu

### Loại bỏ copy của đối tượng “nặng”

Các **đối tượng nặng** thường gặp:

-   Container `vector`, `array`, `map`...
-   `string`
-   Các kiểu có copy/move constructor đặc biệt

Với **đối tượng nhẹ**, truyền tham chiếu không giúp gì, thậm chí tham chiếu còn tốn bộ nhớ hơn.

Điều này có thể gây overhead và cản trở tối ưu.

**Đối tượng nhẹ**:

-   Kiểu cơ bản `int`, `float`...
-   Các [aggregate](https://zh.cppreference.com/w/cpp/language/aggregate_initialization) nhỏ
-   Iterator của container chuẩn

### Chuyển lvalue thành rvalue

Dùng `std::move` để [chuyển](./value-category.md#stdmove) quyền sở hữu. Thường gặp giữa biến cục bộ hoặc giữa tham số và biến cục bộ:

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

string world(string str) { return std::move(str) += " world!"; }

int main() {
  // 1
  cout << world("hello") << '\n';

  vector<string> vec0;

  // 2
  {
    string&& size = to_string(vec0.size());

    size += ", " + to_string(size.size());

    vec0.emplace_back(std::move(size));
  }

  cout << vec0.front();
}
```

Không phải lúc nào cũng cần làm vậy, ví dụ [tối ưu trả về](./value-category.md#常见误区).

### Kéo dài vòng đời tạm bằng rvalue

Về mặt ngữ nghĩa, tạm có thể gây copy/move thêm; dù compiler thường tối ưu bằng [copy elision](./value-category.md#复制消除), tham chiếu có thể ép compiler tránh thao tác dư thừa, giảm bất định.

## Tài liệu tham khảo

1.  [C++ Reference — reference declaration](https://zh.cppreference.com/w/cpp/language/reference)
2.  [C++ Reference — value categories](https://zh.cppreference.com/w/cpp/language/value_category)
3.  [Does const ref lvalue to non-const func return value specifically reduce copies?](https://stackoverflow.com/questions/38909228/does-const-ref-lvalue-to-non-const-func-return-value-specifically-reduce-copies)
