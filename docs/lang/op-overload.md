Nạp chồng toán tử là việc định nghĩa lại toán tử để hỗ trợ thao tác trên kiểu dữ liệu cụ thể. Nạp chồng toán tử là một trường hợp đặc biệt của nạp chồng hàm.

> Khi một toán tử xuất hiện trong một biểu thức và ít nhất một toán hạng có kiểu là lớp hoặc enum, trình biên dịch sẽ dùng phân giải nạp chồng (overload resolution) để quyết định gọi hàm do người dùng định nghĩa nào phù hợp với khai báo tương ứng.[^ref1]

Nói đơn giản, nếu coi việc dùng “toán tử” như gọi một hàm đặc biệt (ví dụ coi `1+2` là gọi `add(1, 2)`), và tham số (toán hạng) của hàm này có ít nhất một kiểu là `class`、`struct` hoặc `enum`, thì trình biên dịch cần dựa vào kiểu toán hạng để quyết định sẽ gọi hàm tự định nghĩa nào.

Trong C++, ta có thể nạp chồng gần như tất cả các toán tử có sẵn.

???+ note "Một số toán tử có thể nạp chồng"
    Toán tử một ngôi: `+` (dấu dương); `-` (dấu âm); `~` (NOT bit); `++`; `--`; `!` (NOT logic); `*` (lấy giá trị qua con trỏ); `&` (lấy địa chỉ); `->` (truy cập thành viên)...
    
    Toán tử hai ngôi: `+`; `-`; `&` (AND bit); `[]` (truy cập chỉ số); `==`; `=` (gán)...
    
    Khác: `()` (gọi hàm); `""` (hậu tố literal[^ref1], từ C++11); `new` (cấp phát bộ nhớ); `,` (toán tử dấu phẩy); `<=>` (so sánh ba chiều[^ref2], từ C++20)…

## Hạn chế

Nạp chồng toán tử có các hạn chế sau:

-   Chỉ có thể nạp chồng các toán tử có sẵn, không thể tự định nghĩa toán tử mới.
-   Các toán tử sau không thể nạp chồng: `::` (phân giải phạm vi), `.` (truy cập thành viên), `.*` (truy cập thành viên qua con trỏ thành viên), `?:` (toán tử ba ngôi).
-   Sau nạp chồng, độ ưu tiên, số toán hạng, hướng kết hợp của toán tử không được thay đổi.
-   Nạp chồng `&&` (AND logic) và `||` (OR logic) sẽ mất tính ngắn mạch.

## Cài đặt

Nạp chồng toán tử có hai cách: nạp chồng thành hàm thành viên hoặc hàm không thành viên.

Khi nạp chồng thành hàm thành viên, do có tham số ẩn `this` trỏ tới đối tượng hiện tại, nên số tham số của hàm sẽ ít hơn số toán hạng một đơn vị.

Còn khi nạp chồng thành hàm không thành viên, số tham số của hàm bằng số toán hạng.

Khuôn mẫu cơ bản (giả sử toán tử cần nạp chồng là `@`):

```cpp
class Example {
  // Ví dụ hàm thành viên
  Kiểu_trả_về operator@(các_tham_số_trừ_bản_thân) { /* ... */ }
};

// Ví dụ hàm không thành viên
Kiểu_trả_về operator@(tất_cả_tham_số_tham_gia) { /* ... */ }
```

Dưới đây là một vài ví dụ nạp chồng toán tử.

### Toán tử số học cơ bản

Định nghĩa một struct vector 2D `Vector2D` và nạp chồng phép cộng và tích vô hướng.

??? note "Ví dụ nạp chồng toán tử số học"
    ```cpp
    struct Vector2D {
      double x, y;
    
      Vector2D(double a = 0, double b = 0) : x(a), y(b) {}
    
      Vector2D operator+(Vector2D v) const { return Vector2D(x + v.x, y + v.y); }
    
      // Lưu ý kiểu trả về có thể không phải là lớp này
      double operator*(Vector2D v) const { return x * v.x + y * v.y; }
    };
    ```

### Toán tử tăng/giảm

Toán tử tăng/giảm có hai dạng: tiền tố (`++a`) và hậu tố (`a++`). Để phân biệt, khi nạp chồng hậu tố cần thêm một tham số giả kiểu `int`.

Có thể hiểu tiền tố tương ứng gọi `operator++(a)` hoặc `a.operator++()`, hậu tố tương ứng gọi `operator++(a, 0)` hoặc `a.operator++(0)`.

??? note "Ví dụ nạp chồng tiền tố và hậu tố"
    ```cpp
    struct MyInt {
      int x;
    
      // Tiền tố, tương ứng ++a
      MyInt &operator++() {
        x++;
        return *this;
      }
    
      // Hậu tố, tương ứng a++
      MyInt operator++(int) {
        MyInt tmp;
        tmp.x = x;
        x++;
        return tmp;
      }
    };
    ```

Một điểm nữa là với toán tử tăng/giảm built-in, dạng tiền tố trả về tham chiếu, còn dạng hậu tố trả về giá trị. Dù toán tử nạp chồng không bắt buộc theo ràng buộc này, về mặt ngữ nghĩa vẫn nên giữ nhất quán với toán tử built-in ở kiểu trả về.

Với kiểu T, định nghĩa điển hình cho nạp chồng tăng như sau:

| Định nghĩa nạp chồng (lấy `++` làm ví dụ) | Hàm thành viên                    | Hàm không thành viên                      |
| --------------- | ----------------------- | -------------------------- |
| Tiền tố              | `T& T::operator++();`   | `T& operator++(T& a);`     |
| Hậu tố              | `T T::operator++(int);` | `T operator++(T& a, int);` |

### Toán tử gọi hàm

Toán tử gọi hàm `()` chỉ có thể nạp chồng thành hàm thành viên. Nạp chồng `()` giúp đối tượng của lớp có thể gọi như một hàm.

Một ứng dụng thường gặp là dùng struct nạp chồng `()` làm hàm so sánh tùy chỉnh trong priority_queue và các container STL khác.

Ví dụ: cho $n$ học sinh với tên và điểm, sắp xếp giảm dần theo điểm, nếu bằng nhau thì tăng dần theo tên, xuất ra người đứng đầu.

Ta định nghĩa một struct so sánh để tùy chỉnh thứ tự của priority_queue.

??? note "Ví dụ nạp chồng toán tử gọi hàm"
    ```cpp
    struct student {
      string name;
      int score;
    };
    
    struct cmp {
      bool operator()(const student& a, const student& b) const {
        return a.score < b.score || (a.score == b.score && a.name > b.name);
      }
    };
    
    // Lưu ý tham số mẫu là tên struct chứ không phải instance
    priority_queue<student, vector<student>, cmp> pq;
    ```

### Toán tử so sánh

Trong `std::sort` và một số container STL, cần dùng toán tử `<`. Khi dùng kiểu tự định nghĩa, ta cần nạp chồng thủ công.

Dưới đây là ví dụ tương đương phần trước.

??? note "Ví dụ nạp chồng toán tử so sánh"
    ```cpp
    struct student {
      string name;
      int score;
    
      // Nạp chồng toán tử <
      bool operator<(const student& a) const {
        return score < a.score || (score == a.score && name > a.name);
        // Trên đây đã lược this, biểu thức đầy đủ:
        // this->score<a.score||(this->score==a.score&&this->name>a.name);
      }
    };
    
    priority_queue<student> pq;
    ```

Đoạn code trên nạp chồng dấu `<` thành hàm thành viên, tất nhiên cũng có thể nạp chồng thành hàm không thành viên.

??? note "Nạp chồng thành hàm không thành viên"
    ```cpp
    struct student {
      string name;
      int score;
    };
    
    bool operator<(const student& a, const student& b) {
      return a.score < b.score || (a.score == b.score && a.name > b.name);
    }
    
    priority_queue<student> pq;
    ```

Thực tế chỉ cần có `<` thì có thể dễ dàng định nghĩa 5 toán tử so sánh còn lại.

```cpp
/* clang-format off */

// Các cách dưới đây đều nạp chồng dấu < thành hàm không thành viên

bool operator<(const T& lhs, const T& rhs) { /* Nạp chồng toán tử < tại đây */ }
bool operator>(const T& lhs, const T& rhs) { return rhs < lhs; }
bool operator<=(const T& lhs, const T& rhs) { return !(lhs > rhs); }
bool operator>=(const T& lhs, const T& rhs) { return !(lhs < rhs); }
bool operator==(const T& lhs, const T& rhs) { return !(lhs < rhs) && !(lhs > rhs); }
bool operator!=(const T& lhs, const T& rhs) { return !(lhs == rhs); }
```

??? note "Về toán tử so sánh ba chiều trong C++20"
    Nếu dùng C++20 hoặc mới hơn, có thể dùng toán tử so sánh ba chiều mặc định để đơn giản hóa.[^ref3]
    
    ```cpp
    auto operator<=>(const T &lhs, const T &rhs) = default;
    ```
    
    Thứ tự so sánh mặc định là theo thứ tự khai báo các thành viên.[^ref4]
    
    Cũng có thể tự định nghĩa so sánh ba chiều. Khi đó cần chọn loại quan hệ thứ tự chứa trong đó (`std::strong_ordering`、`std::weak_ordering` hoặc `std::partial_ordering`), hoặc trả về một đối tượng sao cho:
    
    -   Nếu `a < b` thì `(a <=> b) < 0`;
    -   Nếu `a > b` thì `(a <=> b) > 0`;
    -   Nếu `a` và `b` bằng nhau hoặc tương đương thì `(a <=> b) == 0`.
    
    Chi tiết xem [toán tử so sánh #ba chiều - cppreference](https://zh.cppreference.com/w/cpp/language/operator_comparison#Three-way_comparison).

Tài liệu tham khảo và chú thích:

[^ref1]: [Operator overloading - cppreference](https://zh.cppreference.com/w/cpp/language/operators)

[^ref2]: [User-defined literals - cppreference](https://zh.cppreference.com/w/cpp/language/user_literal)

[^ref3]: [Operator comparison #three-way comparison - cppreference](https://zh.cppreference.com/w/cpp/language/operator_comparison#.E4.B8.89.E8.B7.AF.E6.AF.94.E8.BE.83)

[^ref4]: [Default comparisons - cppreference](https://zh.cppreference.com/w/cpp/language/default_comparisons)
