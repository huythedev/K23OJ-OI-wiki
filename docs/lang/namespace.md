## Tổng quan

Cơ chế **namespace** trong C++ có thể dùng để giải quyết xung đột tên trong dự án phức tạp.

Ví dụ: toàn bộ nội dung của thư viện chuẩn C++ đều nằm trong namespace `std`. Nếu bạn định nghĩa một biến tên `cin`, bạn có thể truy cập biến đó bằng `cin`, và truy cập đối tượng `cin` của thư viện chuẩn bằng `std::cin`, không lo xung đột.

## Khai báo

Đoạn mã sau khai báo một namespace tên `A`:

```cpp
namespace A {
int cnt;

void f(int x) { cnt = x; }
}  // namespace A
```

Sau khi khai báo, bên ngoài namespace này bạn có thể gọi `A::f(x)` để truy cập hàm `f`, hoặc `A::cnt` để truy cập biến `cnt` bên trong `A`.

Namespace có thể lồng nhau, nên đoạn mã sau là hợp lệ:

```cpp
namespace A {
namespace B {
void f() { ... }
}  // namespace B

void f() {
  B::f();  // Thực tế là A::B::f(), do hiện đang ở trong namespace A
           // nên có thể bỏ tiền tố A::
}
}  // namespace A

void f()  // Hàm f trong namespace toàn cục, không xung đột với A::f và A::B::f
{
  A::f();
  A::B::f();
}
```

## Chỉ thị `using`

Sau khi khai báo namespace, nếu muốn truy cập thành viên bên trong từ bên ngoài, cần thêm `namespace::` trước tên thành viên.

Có cách nào tiện hơn để dùng trực tiếp tên thành viên không? Có. Ta dùng chỉ thị `using`.

`using` có hai dạng:

1.  `using namespace::member;`: bỏ tiền tố namespace trước một thành viên cụ thể, tương đương nhập thành viên đó vào phạm vi hiện tại.
2.  `using namespace namespace;`: có thể truy cập **mọi** thành viên trong namespace, tương đương nhập toàn bộ thành viên của namespace đó vào phạm vi hiện tại.

Vì vậy, nếu dùng `using namespace std;` thì toàn bộ tên trong `std` sẽ được đưa vào phạm vi hiện tại. Khi đó có thể dùng `cin` thay cho `std::cin`, `cout` thay cho `std::cout`.

??? warning "Chỉ thị `using` có thể gây xung đột tên!"
    Do `using namespace std;` đưa vào **tất cả** tên trong `std`, nếu bạn khai báo biến hoặc hàm trùng tên với `std` thì có thể gây lỗi biên dịch.
    
    Vì vậy trong dự án thực tế, không khuyến nghị dùng `using namespace ...;`.

Có `using` thì các đoạn mã trong [C++ 语法基础](./basic.md#cin-与-cout) có thể viết theo hai cách tương đương:

```cpp
#include <iostream>

using std::cin;
using std::cout;
using std::endl;

int main() {
  int x, y;
  cin >> x >> y;
  cout << y << endl << x;
  return 0;
}
```

```cpp
#include <iostream>

using namespace std;

int main() {
  int x, y;
  cin >> x >> y;
  cout << y << endl << x;
  return 0;
}
```

## Namespace vô danh

Khi trong một phạm vi chỉ cần một namespace để tránh xung đột tên, ta có thể dùng namespace vô danh để viết gọn hơn.

Namespace dạng `namespace { /* something ... */ }` (không có tên) gọi là namespace vô danh. Một file có namespace vô danh sẽ được xem như có một tên duy nhất, khác với các namespace khác; nhưng trong cùng một phạm vi, nhiều namespace vô danh được xem là cùng một namespace. Sau khi định nghĩa, các tên bên trong có thể được tìm thấy từ phạm vi bên ngoài khi sử dụng, giống như đã thêm một `using namespace` ngầm.

## Ứng dụng

### Tránh xung đột tên giữa các subtasks

Trong bài có nhiều subtasks, ta có thể tạo một namespace riêng cho mỗi subtask, đặt biến và hàm vào đó. Nhờ vậy dù các subtask dùng cùng tên, chúng vẫn không xung đột, thuận tiện cho gỡ lỗi và tăng tính đọc hiểu.

### Tránh xung đột với thư viện chuẩn và tên môi trường

Dùng namespace cũng giúp tránh xung đột giữa các tên thường dùng trong thi đấu và thư viện chuẩn/ môi trường, ví dụ:

```cpp
#include <math.h>

#include <vector>

using namespace std;

namespace Sol {
int end;  // std::end được đưa vào bởi using namespace std;

int y1;  // y1 là hàm Bessel loại 2 do POSIX định nghĩa

// Vì vậy thường trên Linux sẽ xung đột, còn Windows thì không

void solve() {
  // Trong Sol::solve() có thể dùng end và y1 mà không cần ::,
  // sẽ không gây xung đột; nếu ở namespace toàn cục thì sẽ xung đột:
  // end chỉ xung đột lúc tra cứu tên, còn y1 xung đột ngay khi khai báo;
  // hơn nữa xung đột y1 phụ thuộc môi trường, Windows không thấy nhưng Linux
  // khi chấm sẽ báo lỗi biên dịch.
}
}  // namespace Sol

int main() { Sol::solve(); }
```

## Tài liệu tham khảo

-   [Namespaces - cppreference.com](https://en.cppreference.com/w/cpp/language/namespace)
