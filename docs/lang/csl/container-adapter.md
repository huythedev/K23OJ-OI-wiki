author: Xeonacid, ksyx, Early0v0

## Stack

STL [stack](../../ds/stack.md) (`std::stack`) là bộ thích nghi LIFO, chỉ hỗ trợ truy cập/xóa phần tử được thêm cuối cùng (đỉnh), không hỗ trợ truy cập ngẫu nhiên, và để đảm bảo tính thứ tự nghiêm ngặt nên không hỗ trợ iterator.

### Header

```cpp
#include <stack>
```

### Định nghĩa

```cpp
std::stack<TypeName> s;  // Dùng deque mặc định, kiểu dữ liệu là TypeName
std::stack<TypeName, Container> s;  // Dùng Container làm container nền
std::stack<TypeName> s2(s1);        // Sao chép s1 để tạo s2
```

### Hàm thành viên

**Tất cả hàm dưới đây đều hằng số**

-   `top()` truy cập đỉnh (nếu rỗng sẽ lỗi)
-   `push(x)` chèn x
-   `pop()` xóa đỉnh
-   `size()` số phần tử
-   `empty()` rỗng hay không

### Ví dụ đơn giản

```cpp
std::stack<int> s1;
s1.push(2);
s1.push(1);
std::stack<int> s2(s1);
s1.pop();
std::cout << s1.size() << " " << s2.size() << std::endl;  // 1 2
std::cout << s1.top() << " " << s2.top() << std::endl;    // 2 1
s1.pop();
std::cout << s1.empty() << " " << s2.empty() << std::endl;  // 1 0
```

## Queue

STL [queue](../../ds/queue.md) (`std::queue`) là bộ thích nghi FIFO, chỉ hỗ trợ truy cập/xóa phần tử được thêm đầu tiên (đầu hàng), không hỗ trợ truy cập ngẫu nhiên, và để đảm bảo tính thứ tự nghiêm ngặt nên không hỗ trợ iterator.

### Header

```cpp
#include <queue>
```

### Định nghĩa

```cpp
std::queue<TypeName> q;  // Dùng deque mặc định, kiểu dữ liệu là TypeName
std::queue<TypeName, Container> q;  // Dùng Container làm container nền

std::queue<TypeName> q2(q1);  // Sao chép q1 để tạo q2
```

### Hàm thành viên

**Tất cả hàm dưới đây đều hằng số**

-   `front()` truy cập đầu hàng (nếu rỗng sẽ lỗi)
-   `push(x)` chèn x
-   `pop()` xóa đầu hàng
-   `size()` số phần tử
-   `empty()` rỗng hay không

### Ví dụ đơn giản

```cpp
std::queue<int> q1;
q1.push(2);
q1.push(1);
std::queue<int> q2(q1);
q1.pop();
std::cout << q1.size() << " " << q2.size() << std::endl;    // 1 2
std::cout << q1.front() << " " << q2.front() << std::endl;  // 1 2
q1.pop();
std::cout << q1.empty() << " " << q2.empty() << std::endl;  // 1 0
```

## Priority queue

`std::priority_queue` là một [heap](../../ds/heap.md), thường là [binary heap](../../ds/binary-heap.md).

### Header

```cpp
#include <queue>
```

### Định nghĩa

```cpp
std::priority_queue<TypeName> q;             // Kiểu dữ liệu TypeName
std::priority_queue<TypeName, Container> q;  // Dùng Container làm container nền
std::priority_queue<TypeName, Container, Compare> q;
// Dùng Container làm container nền, Compare làm kiểu so sánh

// Mặc định dùng vector làm container nền
// Kiểu so sánh less<TypeName> (khi đó top() là lớn nhất)
// Nếu muốn top() nhỏ nhất, dùng greater<TypeName>
// Lưu ý: không thể bỏ qua Container rồi truyền Compare

// Từ C++11, nếu dùng lambda làm Compare
// thì cần truyền vào constructor, ví dụ:
auto cmp = [](const std::pair<int, int> &l, const std::pair<int, int> &r) {
  return l.second < r.second;
};
std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>,
                    decltype(cmp)>
    pq(cmp);
```

### Hàm thành viên

**Các hàm sau hằng số**

-   `top()` truy cập phần tử đỉnh (priority queue không được rỗng)
-   `empty()` rỗng hay không
-   `size()` số phần tử

**Các hàm sau logarit**

-   `push(x)` chèn và sắp xếp lại container nền
-   `pop()` xóa đỉnh (priority queue không được rỗng)

### Ví dụ đơn giản

```cpp
std::priority_queue<int> q1;
std::priority_queue<int, std::vector<int>> q2;
// Sau C++11 có thể bỏ khoảng trắng
std::priority_queue<int, std::deque<int>, std::greater<int>> q3;
// q3 là min-heap
for (int i = 1; i <= 5; i++) q1.push(i);
// Phần tử trong q1:  [1, 2, 3, 4, 5]
std::cout << q1.top() << std::endl;
// Kết quả: 5
q1.pop();
// Phần tử trong heap: [1, 2, 3, 4]
std::cout << q1.size() << std::endl;
// Kết quả: 4
for (int i = 1; i <= 5; i++) q3.push(i);
// Phần tử trong q3:  [1, 2, 3, 4, 5]
std::cout << q3.top() << std::endl;
// Kết quả: 1
```
