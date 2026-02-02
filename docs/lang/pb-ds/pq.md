author: Xeonacid, ouuan, Ir1d, WAAutoMaton, Chrogeek, abc1763613206, Planet6174, i-Yirannn, opsiff, GoodCoder666

## `__gnu_pbds::priority_queue`

Kèm theo: [Tài liệu chính thức — kiểm thử độ phức tạp và hằng số](https://gcc.gnu.org/onlinedocs/libstdc++/ext/pb_ds/pq_performance_tests.html#std_mod1)

```cpp
#include <ext/pb_ds/priority_queue.hpp>
using namespace __gnu_pbds;
__gnu_pbds::priority_queue<T, Compare, Tag, Allocator>
```

## Tham số mẫu

-   `T`: kiểu phần tử lưu trữ
-   `Compare`: kiểu so sánh weak order nghiêm ngặt
-   `Tag`: 5 loại heap khác nhau của `__gnu_pbds`, mặc định là `pairing_heap_tag`:
    -   `pairing_heap_tag`: pairing heap
        Tài liệu chính thức cho rằng với phần tử không nguyên thủy (như struct tự định nghĩa/`std::string`/`pair`), pairing heap tốt nhất
    -   `binary_heap_tag`: binary heap
        Tài liệu chính thức cho rằng với phần tử nguyên thủy, binary heap tốt nhất, nhưng người viết thử nghiệm thấy không hẳn
    -   `binomial_heap_tag`: binomial heap
        Binomial heap tốt hơn binary heap ở phép gộp, nhưng thao tác lấy đỉnh có độ phức tạp cao hơn
    -   `rc_binomial_heap_tag`: redundant-counting binomial heap
    -   `thin_heap_tag`: một tag mà mọi độ phức tạp ngoài phép gộp đều giống Fibonacci heap
-   `Allocator`: bộ cấp phát; ít gặp trong OI nên không bàn ở đây

Bài này chủ yếu phục vụ học sinh thi đấu, nên chỉ giới thiệu ngắn gọn độ phức tạp của bốn tag sau; tag đầu sẽ giới thiệu hàm và cách dùng.

Theo thử nghiệm trên máy tác giả (Core i5 @3.1 GHz, macOS), kết hợp kiểm thử độ phức tạp GNU và kiểm thử Dijkstra:
Ít nhất đối với OIer, bốn tag ngoài pairing heap đều kém hiệu quả: hoặc không hữu dụng, hoặc hằng số lớn hơn `std`, thậm chí có thể MLE. Vì vậy chỉ khuyến nghị dùng pairing heap mặc định. Đồng thời, pairing heap cũng tốt hơn `make_heap()` của `algorithm`.

## Cách khởi tạo

Cần chỉ rõ namespace vì trùng tên với `std`.

```cpp
// __gnu_pbds::priority_queue<int>;
// __gnu_pbds::priority_queue<int, greater<int>>;
// __gnu_pbds::priority_queue<int, greater<int>, pairing_heap_tag>;
__gnu_pbds::priority_queue<int>::point_iterator id;  // iterator dạng point
// push và modify đều trả về một point_iterator, chi tiết sẽ nói phía dưới
id = q.push(1);
```

## Hàm thành viên

-   `push()`: đẩy một phần tử vào heap, trả iterator vị trí phần tử đó.
-   `pop()`: loại bỏ phần tử đỉnh.
-   `top()`: trả phần tử đỉnh.
-   `size()` trả số lượng phần tử.
-   `empty()` trả về có rỗng hay không.
-   `modify(point_iterator, const key)`: sửa khóa tại iterator thành `key` mới và sắp xếp lại cấu trúc lưu trữ.
-   `erase(point_iterator)`: xóa phần tử tại iterator.
-   `join(__gnu_pbds::priority_queue &other)`: gộp `other` vào `*this` và làm rỗng `other`.

Độ phức tạp phụ thuộc vào tag:

|                        | push                                | pop                                 | modify                              | erase                               | Join              |
| ---------------------- | ----------------------------------- | :---------------------------------- | ----------------------------------- | ----------------------------------- | ----------------- |
| `pairing_heap_tag`     | $O(1)$                              | tối đa $\Theta(n)$, trung bình $\Theta(\log(n))$ | tối đa $\Theta(n)$, trung bình $\Theta(\log(n))$ | tối đa $\Theta(n)$, trung bình $\Theta(\log(n))$ | $O(1)$            |
| `binary_heap_tag`      | tối đa $\Theta(n)$, trung bình $\Theta(\log(n))$ | tối đa $\Theta(n)$, trung bình $\Theta(\log(n))$ | $\Theta(n)$                         | $\Theta(n)$                         | $\Theta(n)$       |
| `binomial_heap_tag`    | tối đa $\Theta(\log(n))$, trung bình $O(1)$      | $\Theta(\log(n))$                   | $\Theta(\log(n))$                   | $\Theta(\log(n))$                   | $\Theta(\log(n))$ |
| `rc_binomial_heap_tag` | $O(1)$                              | $\Theta(\log(n))$                   | $\Theta(\log(n))$                   | $\Theta(\log(n))$                   | $\Theta(\log(n))$ |
| `thin_heap_tag`        | $O(1)$                              | tối đa $\Theta(n)$, trung bình $\Theta(\log(n))$ | tối đa $\Theta(\log(n))$, trung bình $O(1)$      | tối đa $\Theta(n)$, trung bình $\Theta(\log(n))$ | $\Theta(n)$       |

## Ví dụ

```cpp
#include <algorithm>
#include <cstdio>
#include <ext/pb_ds/priority_queue.hpp>
#include <iostream>
using namespace __gnu_pbds;
// Vì hướng đến OIer, ở đây dùng heap phổ biến: pairing_heap_tag làm ví dụ
// Để dễ đọc hơn, định nghĩa alias như sau:
using pair_heap = __gnu_pbds::priority_queue<int>;
pair_heap q1;  // max-heap, pairing heap
pair_heap q2;
pair_heap::point_iterator id;  // một iterator

int main() {
  id = q1.push(1);
  // Các phần tử trong heap: [1];
  for (int i = 2; i <= 5; i++) q1.push(i);
  // Các phần tử trong heap: [1, 2, 3, 4, 5];
  std::cout << q1.top() << std::endl;
  // Kết quả: 5;
  q1.pop();
  // Các phần tử trong heap: [1, 2, 3, 4];
  id = q1.push(10);
  // Các phần tử trong heap: [1, 2, 3, 4, 10];
  q1.modify(id, 1);
  // Các phần tử trong heap: [1, 1, 2, 3, 4];
  std::cout << q1.top() << std::endl;
  // Kết quả: 4;
  q1.pop();
  // Các phần tử trong heap: [1, 1, 2, 3];
  id = q1.push(7);
  // Các phần tử trong heap: [1, 1, 2, 3, 7];
  q1.erase(id);
  // Các phần tử trong heap: [1, 1, 2, 3];
  q2.push(1), q2.push(3), q2.push(5);
  // Các phần tử trong q1: [1, 1, 2, 3], q2: [1, 3, 5];
  q2.join(q1);
  // q1 rỗng, q2: [1, 1, 1, 2, 3, 3, 5];
}
```

## Bảo đảm hiệu lực iterator trong __gnu_pbds (invalidation_guarantee)

Trong ví dụ ở trên và thực tế (ví dụ dùng heap pb-ds để viết Dijkstra), ta thường cần lưu và dùng iterator của heap (như `__gnu_pbds::priority_queue<int>::point_iterator`).

Tuy nhiên với các Tag khác nhau, hiện thực nền khác nhau nên điều kiện mất hiệu lực iterator cũng khác. Theo thiết kế của __gnu_pbds, có ba mức bảo đảm (từ cao xuống thấp):

1.  Bảo đảm cơ bản (basic_invalidation_guarantee): khi **không** sửa container, point_iterator, pointer và reference (key/value) **vẫn** hợp lệ.

2.  Bảo đảm điểm (point_invalidation_guarantee): khi **sửa** container, point_iterator, pointer và reference (key/value) tương ứng **vẫn** hợp lệ nếu phần tử đó chưa bị xóa.

3.  Bảo đảm phạm vi (range_invalidation_guarantee): khi **sửa** container, ngoài (2) thì mọi iterator phạm vi (kể cả `begin()` và `end()`) đều đúng; các Tag có bảo đảm này gồm `rb_tree_tag` và `splay_tree_tag` (dùng cho `__gnu_pbds::tree`) và `pat_trie_tag` (dùng cho `__gnu_pbds::trie`).

Chạy đoạn code dưới cho thấy, trừ `binary_heap_tag` có `basic_invalidation_guarantee` nên iterator sẽ mất hiệu lực sau khi sửa, các tag còn lại đều là `point_invalidation_guarantee`, đáp ứng yêu cầu point_iterator không mất hiệu lực sau khi sửa.

```cpp
#include <iostream>
using namespace std;
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/priority_queue.hpp>
using namespace __gnu_pbds;
#include <cxxabi.h>

template <typename T>
void print_invalidation_guarantee() {
  using gute = __gnu_pbds::container_traits<T>::invalidation_guarantee;
  cout << abi::__cxa_demangle(typeid(gute).name(), 0, 0, 0) << endl;
}

int main() {
  using pairing =
      __gnu_pbds::priority_queue<int, greater<int>, pairing_heap_tag>;
  using binary = __gnu_pbds::priority_queue<int, greater<int>, binary_heap_tag>;
  using binomial =
      __gnu_pbds::priority_queue<int, greater<int>, binomial_heap_tag>;
  using rc_binomial =
      __gnu_pbds::priority_queue<int, greater<int>, rc_binomial_heap_tag>;
  using thin = __gnu_pbds::priority_queue<int, greater<int>, thin_heap_tag>;
  print_invalidation_guarantee<pairing>();
  print_invalidation_guarantee<binary>();
  print_invalidation_guarantee<binomial>();
  print_invalidation_guarantee<rc_binomial>();
  print_invalidation_guarantee<thin>();
  return 0;
}
```
