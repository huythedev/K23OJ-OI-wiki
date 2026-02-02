## `__gnu_pbds::tree`

Kèm theo: [Địa chỉ tài liệu chính thức](https://gcc.gnu.org/onlinedocs/libstdc++/ext/pb_ds/tree_based_containers.html)

```cpp
#include <ext/pb_ds/assoc_container.hpp>  // vì tree được định nghĩa ở đây nên cần include header này
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;
__gnu_pbds::tree<Key, Mapped, Cmp_Fn = std::less<Key>, Tag = rb_tree_tag,
                 Node_Update = null_tree_node_update,
                 Allocator = std::allocator<char>>
```

## Tham số mẫu

-   `Key`: kiểu phần tử lưu trữ; nếu muốn lưu nhiều phần tử `Key` trùng nhau thì cần dùng `std::pair` hoặc `struct` kết hợp với `lower_bound` và `upper_bound` để tra cứu
-   `Mapped`: kiểu chính sách ánh xạ (Mapped-Policy); nếu muốn chỉ ra container là **tập**, tương tự lưu trong `std::set`, điền `null_type` (ở g++ phiên bản thấp là `null_mapped_type`); nếu muốn chỉ ra container là **tập có giá trị**, tương tự `std::map`, điền kiểu `Value` như trong `std::map<Key, Value>`
-   `Cmp_Fn`: hàm so sánh khóa, ví dụ `std::less<Key>`
-   `Tag`: chọn cấu trúc dữ liệu nền, mặc định `rb_tree_tag`. `__gnu_pbds` cung cấp ba loại cây cân bằng:
    -   `rb_tree_tag`: cây đỏ-đen, thường dùng loại này; hai loại sau thường kém hơn
    -   `splay_tree_tag`: cây splay
    -   `ov_tree_tag`: cây vector có thứ tự, chỉ là cấu trúc có thứ tự bằng `vector`, tương tự một `vector` đã sắp xếp để làm cây cân bằng; hiệu năng phụ thuộc vào dữ liệu có “cố tình” hay không
-   `Node_Update`: chính sách cập nhật nút, mặc định `null_node_update`; nếu muốn dùng `order_of_key` và `find_by_order` thì cần `tree_order_statistics_node_update`
-   `Allocator`: kiểu cấp phát bộ nhớ

## Cách khởi tạo

```cpp
__gnu_pbds::tree<std::pair<int, int>, __gnu_pbds::null_type,
                 std::less<std::pair<int, int>>, __gnu_pbds::rb_tree_tag,
                 __gnu_pbds::tree_order_statistics_node_update>
    trr;
```

## Hàm thành viên

-   `insert(x)`: chèn phần tử `x`, trả về `std::pair<point_iterator, bool>`; phần tử đầu là iterator vị trí chèn, phần tử hai cho biết chèn thành công hay không.
-   `erase(x)`: xóa một phần tử/iterator `x`. Nếu `x` là iterator thì trả iterator tới phần tử sau `x` (nếu `x` là `end()` thì trả `end()`); nếu `x` là `Key` thì trả về xóa thành công hay không (không tồn tại thì xóa thất bại).
-   `order_of_key(x)`: trả số phần tử nhỏ hơn chặt `x` (theo `Cmp_Fn`), tức là thứ hạng bắt đầu từ $0$.
-   `find_by_order(x)`: trả iterator của phần tử ứng với thứ hạng theo `Cmp_Fn`.
-   `lower_bound(x)`: trả iterator của phần tử đầu tiên không nhỏ hơn `x` (theo `Cmp_Fn`).
-   `upper_bound(x)`: trả iterator của phần tử đầu tiên lớn hơn chặt `x` (theo `Cmp_Fn`).
-   `join(x)`: gộp cây `x` vào cây hiện tại, `x` bị làm rỗng (cần đảm bảo **hàm so sánh** và **kiểu phần tử** giống nhau).
-   `split(x,b)`: theo `Cmp_Fn`, các phần tử $\le x$ thuộc cây hiện tại, còn lại thuộc cây `b`.
-   `empty()`: trả về có rỗng hay không.
-   `size()`: trả về kích thước.

???+ warning "Lưu ý"
    Hàm `join(x)` yêu cầu miền giá trị của khóa trong cây được gộp và cây hiện tại **không giao nhau** (tức là mọi giá trị trong cây được gộp đều phải lớn hơn/nhỏ hơn toàn bộ giá trị của cây hiện tại), nếu không sẽ ném ngoại lệ `join_error`.
    
    Nếu muốn gộp hai cây có miền giá trị giao nhau, cần chèn từng phần tử của một cây vào cây còn lại.

## Ví dụ

```cpp
// Common Header Simple over C++11
#include <iostream>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using ld = long double;
using pii = pair<int, int>;
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
__gnu_pbds::tree<pair<int, int>, __gnu_pbds::null_type, less<pair<int, int>>,
                 __gnu_pbds::rb_tree_tag,
                 __gnu_pbds::tree_order_statistics_node_update>
    trr;

int main() {
  int cnt = 0;
  trr.insert(make_pair(1, cnt++));
  trr.insert(make_pair(5, cnt++));
  trr.insert(make_pair(4, cnt++));
  trr.insert(make_pair(3, cnt++));
  trr.insert(make_pair(2, cnt++));
  // Các phần tử trên cây {(1,0), (2,4), (3,3), (4,2), (5,1)}

  auto it = trr.lower_bound(make_pair(2, 0));
  trr.erase(it);
  // Các phần tử trên cây {(1,0), (3,3), (4,2), (5,1)}

  // In phần tử có thứ hạng 1 trong các thứ hạng 0 1 2 3 (lấy first)
  auto it2 = trr.find_by_order(1);
  cout << (*it2).first << endl;  // Output: 3

  // In thứ hạng của nó
  int pos = trr.order_of_key(*it2);
  cout << pos << endl;  // Output: 1

  // Tách trr theo it2
  decltype(trr) newtr;
  trr.split(*it2, newtr);
  for (auto i = newtr.begin(); i != newtr.end(); ++i) {
    cout << (*i).first << ' ';  // Output: 4 5
  }
  cout << endl;

  // Gộp cây newtr vào trr, newtr bị làm rỗng.
  trr.join(newtr);
  for (auto i = trr.begin(); i != trr.end(); ++i) {
    cout << (*i).first << ' ';  // Output: 1 3 4 5
  }
  cout << endl;
  cout << newtr.size() << endl;  // Output: 0

  return 0;
}
```

## Tài liệu tham khảo

-   [Tree-Based Containers](https://gcc.gnu.org/onlinedocs/libstdc++/ext/pb_ds/tree_based_containers.html)
-   [`join` trong GCC 14.1.0](https://gcc.gnu.org/onlinedocs/gcc-14.1.0/libstdc++/api/a18391_source.html#l00043)
-   [`erase` trong GCC 14.1.0](https://gcc.gnu.org/onlinedocs/gcc-14.1.0/libstdc++/api/a18211_source.html#l00043)
