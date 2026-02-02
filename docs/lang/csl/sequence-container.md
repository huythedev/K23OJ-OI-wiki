author: MingqiHuang, Xeonacid, greyqz, i-Yirannn, ChenZ01

## `vector`

`std::vector` là cấu trúc dữ liệu mảng **liền kề trong bộ nhớ**, **độ dài biến đổi** (còn gọi là danh sách) do STL cung cấp. Cho phép chèn/xóa tuyến tính và truy cập ngẫu nhiên hằng số.

### Vì sao dùng `vector`

Là OIer, theo đuổi hiệu năng thường cao hơn ổn định ở mức工程. `vector` do xử lý bộ nhớ động nên trong một số trường hợp chậm hơn mảng tĩnh, và trên OJ không bật tối ưu có thể còn tệ hơn. Vì vậy khi lưu dữ liệu bình thường thường không chọn `vector`. Dưới đây là các đặc tính nổi bật giúp `vector` hữu ích khi cần.

#### `vector` có thể cấp phát động

Nhiều lúc không thể cấp sẵn đủ lớn (vd: tiền xử lý ước của 1~n). Dù biết tổng dữ liệu đủ nhỏ, nhưng từng phần có thể rất lớn, khi đó cần `vector` để kiểm soát bộ nhớ. `vector` cũng hỗ trợ mở rộng động, rất hữu ích khi bộ nhớ hạn chế.

#### `vector` nạp chồng toán tử so sánh và gán

`vector` nạp chồng 6 toán tử so sánh theo thứ tự từ điển, giúp so sánh hai container thuận tiện (độ phức tạp tuyến tính theo kích thước). Ví dụ có thể dùng `vector<char>` so sánh chuỗi (nhưng `std::string` vẫn nhanh và tiện hơn). Ngoài ra `vector` nạp chồng toán tử gán, giúp sao chép mảng dễ dàng.

#### `vector` khởi tạo tiện lợi

Vì `vector` nạp chồng `=`, ta có thể gán cả `vector`. Từ C++11, `vector` hỗ trợ [list initialization](https://zh.cppreference.com/w/cpp/language/list_initialization), ví dụ `vector<int> data {1, 2, 3};`.

### Cách dùng `vector`

Giới thiệu thao tác thường dùng, chi tiết xem [tài liệu C++](https://zh.cppreference.com/w/cpp/container/vector).

#### Hàm dựng

Ví dụ (giả sử đã `using` các loại trong `std`):

```cpp
// 1. Tạo vector rỗng; độ phức tạp hằng số
vector<int> v0;
// 1+. Dòng này đảm bảo khi chèn 3 phần tử đầu là hằng số
v0.reserve(3);
// 2. Tạo vector độ dài 3, giá trị mặc định 0; tuyến tính
vector<int> v1(3);
// 3. Tạo vector độ dài 3, giá trị mặc định 2; tuyến tính
vector<int> v2(3, 2);
// 4. Tạo vector độ dài 3, giá trị 1,
// và dùng allocator của v2; tuyến tính
vector<int> v3(3, 1, v2.get_allocator());
// 5. Tạo vector v4 là bản sao v2; tuyến tính
vector<int> v4(v2);
// 6. Tạo vector v5 từ v4[1]..v4[2]; tuyến tính
vector<int> v5(v4.begin() + 1, v4.begin() + 3);
// 7. Di chuyển v2 sang v6, không sao chép; hằng số; cần C++11
vector<int> v6(std::move(v2));  // hoặc v6 = std::move(v2);
```

??? note "Mã test"
    ```cpp
    // Dưới đây là mã test, có thể tự biên dịch chạy thử.
    cout << "v1 = ";
    copy(v1.begin(), v1.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    cout << "v2 = ";
    copy(v2.begin(), v2.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    cout << "v3 = ";
    copy(v3.begin(), v3.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    cout << "v4 = ";
    copy(v4.begin(), v4.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    cout << "v5 = ";
    copy(v5.begin(), v5.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    cout << "v6 = ";
    copy(v6.begin(), v6.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    ```

Có thể dùng các cách trên để dựng `vector`, đủ cho hầu hết trường hợp.

#### Truy cập phần tử

`vector` cung cấp các cách truy cập sau:

1.  `at()`

    `v.at(pos)` trả về tham chiếu phần tử ở vị trí `pos`. Nếu vượt biên ném `std::out_of_range`.

2.  `operator[]`

    `v[pos]` trả về tham chiếu phần tử ở `pos`. Không kiểm tra biên.

3.  `front()`

    `v.front()` trả về tham chiếu phần tử đầu.

4.  `back()`

    `v.back()` trả về tham chiếu phần tử cuối.

5.  `data()`

    `v.data()` trả về con trỏ tới phần tử đầu trong vùng nhớ liên tiếp của `v`.

#### Iterator

vector cung cấp các [iterator](./iterator.md) sau:

1.  `begin()/cbegin()`

    Trả về iterator trỏ đến phần tử đầu, `*begin = front`.

2.  `end()/cend()`

    Trả về iterator trỏ đến vị trí sau phần tử cuối, không có phần tử.

3.  `rbegin()/crbegin()`

    Trả về reverse iterator trỏ đến phần tử đầu của dãy đảo, có thể hiểu là phần tử cuối của container thường.

4.  `rend()/crend()`

    Trả về reverse iterator trỏ đến vị trí sau phần tử cuối của dãy đảo (trước phần tử đầu).

Iterator có `c` là chỉ đọc; không thể sửa phần tử qua iterator đó. Nếu `vector` là hằng thì iterator thường và chỉ đọc tương đương. Iterator chỉ đọc hỗ trợ từ C++11.

#### Độ dài và dung lượng

`vector` có các hàm liên quan đến độ dài và dung lượng. Lưu ý: độ dài (size) là số phần tử hiệu lực, dung lượng (capacity) là lượng bộ nhớ đã cấp. Chi tiết xem phần triển khai.

**Liên quan đến độ dài**:

-   `empty()` trả về `bool`, tức `v.begin() == v.end()`; `true` rỗng, `false` không rỗng.

-   `size()` trả về độ dài, tức `std::distance(v.begin(), v.end())`.

-   `resize(n)` đổi độ dài thành `n`. Nếu `n` lớn hơn, sẽ bổ sung phần tử (dùng giá trị truyền vào nếu có, nếu không dùng mặc định); nếu `n` nhỏ hơn, giữ `n` phần tử đầu và xóa phần còn lại.

-   `max_size()` trả về độ dài tối đa có thể.

    **Liên quan đến dung lượng**:

-   `reserve()` cho `vector` dự trữ bộ nhớ, tránh cấp phát/copy không cần thiết.

-   `capacity()` trả về dung lượng hiện tại.

-   `shrink_to_fit()` đưa dung lượng về đúng độ dài, loại bỏ phần dư.

### Thêm/xóa/sửa phần tử

-   `clear()` xóa toàn bộ phần tử
-   `insert()` chèn tại vị trí iterator, có thể chèn nhiều. **Độ phức tạp tuyến tính theo khoảng cách tới cuối**
-   `erase()` xóa phần tử hoặc đoạn, trả về iterator sau phần tử bị xóa cuối cùng. Độ phức tạp như `insert`.
-   `push_back()` chèn cuối, độ phức tạp trung bình **hằng số**, tệ nhất tuyến tính.
-   `pop_back()` xóa cuối, hằng số.
-   `swap()` đổi với container khác, **hằng số**.

### Chi tiết triển khai `vector`

`vector` thực chất là mảng cố định; mở rộng động nhờ tránh tràn số lượng. `vector` lưu riêng số phần tử $n$ và dung lượng $N$. Khi thêm phần tử mà $n>N$, container sẽ cấp một mảng kích thước $2N$, copy dữ liệu sang mảng mới, rồi giải phóng mảng cũ. Dù thao tác này có độ phức tạp $O(n)$, nhưng có thể chứng minh độ phức tạp trung bình là $O(1)$. Xóa ở cuối và truy cập vẫn là $O(1)$.

Do đó, nếu ước lượng tốt kích thước và dùng `resize()`/`reserve()` hợp lý, hiệu năng `vector` sẽ gần như mảng tĩnh.

### `vector<bool>`

Thư viện chuẩn có đặc hóa `vector<bool>`, mỗi `bool` chỉ chiếm 1 bit và hỗ trợ tăng động. Nhưng `operator[]` không trả về `bool&` mà là `vector<bool>::reference`. Vì vậy dùng `vector<bool>` cần cẩn thận; có thể dùng `deque<bool>` hoặc `vector<char>` thay thế. Nếu cần tiết kiệm bộ nhớ, hãy dùng [`bitset`](./bitset.md).

## `array`(C++11)

`std::array` là cấu trúc dữ liệu mảng **liền kề**, **độ dài cố định** do STL cung cấp. Bản chất là bọc mảng C.

### Vì sao dùng `array`

`array` là bọc STL cho mảng. So với `vector`, nó hi sinh mở rộng động để đổi lấy hiệu năng gần như mảng C (khi bật tối ưu). Vì vậy khi có C++11, các nơi dùng mảng tĩnh có thể dùng `array`; mảng cấp phát động có thể thay bằng `vector`.

### Hàm thành viên

#### Hàm thành viên được định nghĩa ngầm

| Hàm          | Tác dụng                                  |
| ----------- | ----------------------------------- |
| `operator=` | Gán từng phần tử của một `array` khác vào `array` hiện tại |

#### Truy cập phần tử

| Hàm           | Tác dụng                   |
| ------------ | -------------------- |
| `at`         | Truy cập phần tử với kiểm tra biên     |
| `operator[]` | Truy cập phần tử, **không** kiểm tra biên |
| `front`      | Truy cập phần tử đầu              |
| `back`       | Truy cập phần tử cuối             |
| `data`       | Trả về con trỏ tới phần tử đầu trong bộ nhớ |

`at` ném `std::out_of_range` nếu `pos >= size()`.

#### Dung lượng

| Hàm         | Tác dụng          |
| ---------- | ----------- |
| `empty`    | Kiểm tra rỗng    |
| `size`     | Số phần tử    |
| `max_size` | Số phần tử tối đa |

Vì `array` là cố định, `size()` = `max_size()`.

### Thao tác

| Hàm     | Tác dụng       |
| ------ | -------- |
| `fill` | Điền giá trị |
| `swap` | Hoán đổi     |

**Lưu ý: hoán đổi hai `array` là $\Theta(\text{size})$, không phải $O(1)$.**

### Hàm không phải thành viên

| Hàm             | Tác dụng                  |
| -------------- | ------------------- |
| `operator==`... | So sánh theo từ điển |
| `std::get`     | Truy cập phần tử |
| `std::swap`    | Thuật toán `std::swap` đặc hóa |

Ví dụ `array`:

```cpp
// 1. Tạo array rỗng, độ dài 3; hằng số
std::array<int, 3> v0;
// 2. Tạo array với giá trị hằng; hằng số
std::array<int, 3> v1{1, 2, 3};

v0.fill(1);  // Điền giá trị

// Duyệt mảng
for (int i = 0; i != arr.size(); ++i) cout << arr[i] << " ";
```

## `deque`

`std::deque` là [deque](../../ds/queue.md#双端队列) trong STL. Cho phép chèn/xóa tuyến tính, truy cập ngẫu nhiên hằng số.

### Cách dùng `deque`

Giới thiệu cách dùng thường gặp, chi tiết xem [tài liệu C++](https://zh.cppreference.com/w/cpp/container/deque). Các iterator như `vector`, nên không trình bày lại.

#### Hàm dựng

Ví dụ (giả sử đã `using` các loại trong `std`):

```cpp
// 1. Định nghĩa deque rỗng kiểu int v0
deque<int> v0;
// 2. Định nghĩa deque v1 có kích thước 10; tuyến tính
deque<int> v1(10);
// 3. Định nghĩa deque v2 kích thước 10, giá trị 1; tuyến tính
deque<int> v2(10, 1);
// 4. Sao chép deque v1; tuyến tính
deque<int> v3(v1);
// 5. Tạo deque v4 là bản sao từ v2[0] đến v2[2]; tuyến tính
deque<int> v4(v2.begin(), v2.begin() + 3);
// 6. Di chuyển v2 sang v5, không copy; hằng số; cần C++11
deque<int> v5(std::move(v2));
```

#### Truy cập phần tử

Giống `vector`, nhưng không thể truy cập bộ nhớ nền. Tốc độ truy cập tham khảo phần triển khai.

-   `at()` trả về tham chiếu phần tử, có kiểm tra biên, **hằng số**.
-   `operator[]` trả về tham chiếu phần tử, không kiểm tra biên, **hằng số**.
-   `front()` trả về tham chiếu phần tử đầu.
-   `back()` trả về tham chiếu phần tử cuối.

#### Iterator

Giống `vector`.

#### Độ dài

Giống `vector`, nhưng không có `reserve()` và `capacity()` (vẫn có `shrink_to_fit()`).

#### Thêm/xóa/sửa phần tử

Giống `vector`, và thêm thao tác ở đầu.

-   `clear()` xóa toàn bộ phần tử
-   `insert()` chèn tại iterator, có thể chèn nhiều. **Độ phức tạp tuyến tính theo khoảng cách tới đầu/cuối gần hơn**.
-   `erase()` xóa phần tử hoặc đoạn, trả về iterator sau phần tử bị xóa cuối cùng. Độ phức tạp như `insert`.
-   `push_front()` chèn đầu, **hằng số**.
-   `pop_front()` xóa đầu, **hằng số**.
-   `push_back()` chèn cuối, **hằng số**.
-   `pop_back()` xóa cuối, **hằng số**.
-   `swap()` đổi với container khác, **hằng số**.

### Chi tiết triển khai `deque`

`deque` thường được hiện thực bởi nhiều buffer không liên tiếp, nhưng mỗi buffer là liên tiếp. Mỗi buffer lưu con trỏ đầu/cuối để đánh dấu vùng dữ liệu. Khi buffer đầy sẽ cấp thêm buffer trước hoặc sau để chứa thêm dữ liệu. Xem thêm [《STL 源码剖析》deque 实现原理](https://www.cnblogs.com/q1076452761/p/16903229.html).

## `list`

`std::list` là [danh sách liên kết đôi](../../ds/linked-list.md). Truy cập ngẫu nhiên tuyến tính, chèn/xóa hằng số.

### Cách dùng `list`

Cách dùng gần giống `deque` nhưng độ phức tạp khác. Xem [tài liệu C++](https://zh.cppreference.com/w/cpp/container/list). Các iterator/độ dài/thêm xóa giống `deque` nên không trình bày lại.

#### Truy cập phần tử

Vì là danh sách liên kết nên không hỗ trợ truy cập ngẫu nhiên; cần dùng iterator.

-   `front()` trả về tham chiếu phần tử đầu.
-   `back()` trả về tham chiếu phần tử cuối.

#### Thao tác

`list` có thêm các thuật toán STL tối ưu cho đặc tính danh sách: `splice()`、`remove()`、`sort()`、`unique()`、`merge()`.

## `forward_list`（C++11）

`std::forward_list` là [danh sách liên kết đơn](../../ds/linked-list.md), tiết kiệm bộ nhớ hơn `std::list`.

### Cách dùng `forward_list`

Gần như giống `list`, nhưng iterator chỉ một chiều nên không trình bày chi tiết. Xem [tài liệu C++](https://zh.cppreference.com/w/cpp/container/forward_list)
