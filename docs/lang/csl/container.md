## Phân loại

![](images/container1.png)

### Container tuần tự

-   **Vector**(`vector`) bảng tuần tự có thể thêm phần tử hiệu quả ở cuối.
-   **Array**(`array`)**C++11** bảng tuần tự độ dài cố định, bọc mảng C.
-   **Deque**(`deque`) bảng tuần tự có thể thêm phần tử hiệu quả ở cả hai đầu.
-   **List**(`list`) danh sách liên kết đôi, có thể duyệt hai chiều.
-   **Forward list**(`forward_list`) danh sách liên kết đơn, chỉ duyệt một chiều.

### Container liên kết

-   **Set**(`set`) lưu phần tử **khác nhau** có thứ tự. Hiện thực bằng cây đỏ-đen với các node chứa phần tử, sắp theo một “vị từ” so sánh.
-   **Multiset**(`multiset`) lưu phần tử có thứ tự, cho phép trùng.
-   **Map**(`map`) tập các cặp {khóa, giá trị}, sắp theo quan hệ so sánh khóa.
-   **Multimap**(`multimap`) đa tập các cặp {khóa, giá trị}, cho phép khóa trùng.

???+ note "Vị từ ([Predicate](https://en.wikipedia.org/wiki/Predicate_%28mathematical_logic%29)) là gì?"
    Vị từ là hàm trả về đúng hoặc sai. STL thường dùng vị từ làm tham số template.

### Container (liên kết) không thứ tự

-   **Unordered (multi)set**(`unordered_set`/`unordered_multiset`)**C++11**, khác `set`/`multiset` ở chỗ không có thứ tự, chỉ quan tâm “phần tử có tồn tại hay không”, dùng hash.
-   **Unordered (multi)map**(`unordered_map`/`unordered_multimap`)**C++11**, khác `map`/`multimap` ở chỗ khóa không có thứ tự, chỉ quan tâm quan hệ “khóa–giá trị”, dùng hash.

### Bộ thích nghi container

Bộ thích nghi không phải là container. Chúng không có một số đặc điểm của container (ví dụ: có iterator, có `clear()`...).

> “Adapter là cơ chế làm cho hành vi của một thứ giống với hành vi của thứ khác”, adapter bọc container để thể hiện hành vi khác.

-   **Stack**(`stack`) LIFO, mặc định bọc `deque`.
-   **Queue**(`queue`) FIFO, mặc định bọc `deque`.
-   **Priority queue**(`priority_queue`) thứ tự phần tử do một vị từ quyết định, mặc định bọc `vector`.

## Điểm chung

### Khai báo container

Đều có dạng `containerName<typeName,...> name`, nhưng số lượng/hình thức tham số template khác nhau tùy container.

Nguyên nhân: STL là “Standard Template Library”, nên container là lớp template.

### Iterator

Xem [iterator](./iterator.md).

### Hàm chung

`=`: có toán tử gán và copy constructor.

`begin()`：trả về iterator trỏ đầu.

`end()`：trả về iterator trỏ sau phần tử cuối. `end()` không trỏ phần tử, nhưng là hậu kế của phần tử cuối.

`size()`：số phần tử.

`max_size()`：số phần tử tối đa **theo lý thuyết**. Tùy loại container và kiểu dữ liệu.

`empty()`：rỗng hay không.

`swap()`：hoán đổi hai container.

`clear()`：xóa toàn bộ.

`==`/`!=`/`<`/`>`/`<=`/`>=`：so sánh theo **thứ tự từ điển** (với `map`, mỗi phần tử tương đương `set<pair<key, value>>`; container không thứ tự không hỗ trợ `<`/`>`/`<=`/`>=`).
