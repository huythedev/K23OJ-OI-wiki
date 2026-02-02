author: sbofgayschool

`std::pair` là một class template được định nghĩa trong thư viện chuẩn, dùng để liên kết hai biến lại với nhau thành một “cặp”, và hai biến này có thể thuộc các kiểu dữ liệu khác nhau.

??? note "类模板"
    Class template (class template) bản thân không phải là một lớp, mà là một “khuôn mẫu” có thể sinh ra **những lớp khác nhau** dựa trên **các kiểu dữ liệu khác nhau**.
    
    Khi sử dụng, trình biên dịch sẽ sinh ra lớp tương ứng theo kiểu dữ liệu được truyền vào, rồi tạo thể hiện tương ứng.
    
    Template là một đặc trưng ngôn ngữ nâng cao của C++, trong lập trình thi đấu hầu như không xuất hiện. Nếu quan tâm, bạn có thể đọc thêm 《C++ Primer》 để học sâu hơn về C++.

Thông qua việc sử dụng `pair` một cách linh hoạt, có thể dễ dàng xử lý **các tình huống cần gộp dữ liệu có liên hệ để lưu trữ và xử lý**.

??? note "Struct"
    So với `struct` tự định nghĩa, `pair` không cần định nghĩa thêm cấu trúc hay nạp chồng toán tử, nên sử dụng tiện hơn.
    
    Tuy nhiên, tên biến trong `struct` tự định nghĩa thường rõ nghĩa hơn (`pair` chỉ có thể truy cập hai biến bằng `first` và `second`). Đồng thời, nếu cần liên kết nhiều hơn hai biến, `struct` tự định nghĩa sẽ phù hợp hơn.

## Sử dụng

### Khởi tạo

Có thể khởi tạo `pair` trực tiếp khi định nghĩa.

```cpp
pair<int, double> p0(1, 2.0);
```

Cũng có thể khởi tạo `pair` bằng cách định nghĩa trước rồi gán sau.

```cpp
pair<int, double> p1;
p1.first = 1;
p1.second = 2.0;
```

Cũng có thể dùng hàm `std::make_pair`. Hàm này nhận hai biến và trả về `pair` được tạo từ hai biến đó.

```cpp
pair<int, double> p2 = make_pair(1, 2.0);
```

Một cách thường dùng là định nghĩa macro `#define mp make_pair`, rút gọn `make_pair` (hơi dài) thành `mp`.

Trong C++11 trở lên, `make_pair` có thể kết hợp với `auto` để tránh khai báo kiểu dữ liệu một cách tường minh.

```cpp
auto p3 = make_pair(1, 2.0);
```

Về việc sử dụng `auto` trong lập trình thi đấu, xem phần [迭代器](./iterator.md).

### Truy cập

Thông qua các thành viên `first` và `second`, có thể truy cập hai biến trong `pair`.

```cpp
int i = p0.first;
double d = p0.second;
```

Cũng có thể sửa chúng.

```cpp
p1.first++;
```

### So sánh

`pair` đã định nghĩa sẵn tất cả các toán tử so sánh, bao gồm `<`、`>`、`<=`、`>=`、`==`、`!=`. Tất nhiên, điều này yêu cầu kiểu dữ liệu của hai biến trong `pair` đã định nghĩa toán tử `==` và/hoặc `<`.

Trong đó, bốn toán tử `<`、`>`、`<=`、`>=` sẽ so sánh biến thứ nhất của hai `pair` trước; khi biến thứ nhất bằng nhau thì so sánh biến thứ hai.

```cpp
if (p2 >= p3) {
  cout << "do something here" << endl;
}
```

Vì `pair` định nghĩa sẵn các toán tử thường dùng trong STL như `<` và `==`, nên có thể phối hợp tốt với các hàm hay cấu trúc dữ liệu STL khác. Ví dụ, `pair` có thể làm kiểu dữ liệu cho `priority_queue`.

```cpp
priority_queue<pair<int, double>> q;
```

### Gán và hoán đổi

Có thể gán giá trị của một `pair` cho một `pair` khác có cùng kiểu.

```cpp
p0 = p1;
```

Cũng có thể dùng `swap` để hoán đổi giá trị của `pair`.

```cpp
swap(p0, p1);
p2.swap(p3);
```

## Ví dụ ứng dụng

### Rời rạc hóa

`pair` có thể dễ dàng hiện thực rời rạc hóa.

Ta có thể tạo một mảng `pair`, lấy giá trị dữ liệu gốc làm biến thứ nhất của mỗi `pair`, và vị trí của dữ liệu gốc làm biến thứ hai. Sau khi sắp xếp, gán thứ hạng (vị trí sau khi sắp xếp) của giá trị đó về lại vị trí ban đầu.

```cpp
// a là dữ liệu gốc
pair<int, int> a[MAXN];
// ai là dữ liệu sau khi rời rạc hóa
int ai[MAXN];
for (int i = 0; i < n; i++) {
  // first là giá trị dữ liệu gốc, second là vị trí của dữ liệu gốc
  scanf("%d", &a[i].first);
  a[i].second = i;
}
// Sắp xếp
sort(a, a + n);
for (int i = 0; i < n; i++) {
  // Gán thứ hạng của giá trị đó về đúng vị trí ban đầu
  ai[a[i].second] = i;
}
```

### Dijkstra

Như đã nói ở trên, `pair` có thể làm kiểu dữ liệu cho `priority_queue`.

Vậy trong tối ưu hóa bằng heap của thuật toán Dijkstra, có thể dùng `pair` cùng `priority_queue` để quản lý các đỉnh, trong đó khoảng cách từ đỉnh đến nguồn là biến thứ nhất, và chỉ số đỉnh là biến thứ hai.

```cpp
priority_queue<pair<int, int>, std::vector<pair<int, int>>,
               std::greater<pair<int, int>>>
    q;
... while (!q.empty()) {
  // dis là khoảng cách của đỉnh đến nguồn khi được đưa vào heap, i là chỉ số đỉnh
  int dis = q.top().first, i = q.top().second;
  q.pop();
  ...
}
```

### pair và map

`map` là cấu trúc dữ liệu lưu trữ các cặp khóa–giá trị trong C++. Trong nhiều trường hợp, các cặp khóa–giá trị trong `map` được lộ ra ngoài thông qua `pair`.

```cpp
map<int, double> m;
m.insert(make_pair(1, 2.0));
```

Về `map`, xem thêm phần liên quan trong [关联式容器](./associative-container.md) và [无序关联式容器](./unordered-container.md).
