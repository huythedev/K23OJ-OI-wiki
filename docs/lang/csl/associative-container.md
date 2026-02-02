## `set`

`set` là container liên kết, chứa tập các đối tượng kiểu khóa đã được sắp thứ tự; tìm kiếm, xóa và chèn có độ phức tạp logarit. `set` thường được hiện thực bằng [cây đỏ-đen](../../ds/rbtree.md). Tính chất của [cây nhị phân cân bằng](../../ds/bst.md) khiến `set` rất phù hợp cho các tình huống vừa cần tìm kiếm, chèn và xóa.

Tương tự tập trong toán học, `set` không chứa phần tử trùng giá trị. Nếu cần tập có phần tử trùng, hãy dùng `multiset`. Cách dùng `multiset` gần như giống `set`.

### Thao tác chèn và xóa

-   `insert(x)` Khi trong container không có phần tử tương đương, chèn phần tử x vào `set`.
-   `erase(x)` Xóa **tất cả** phần tử có giá trị x, trả về số phần tử bị xóa.
-   `erase(pos)` Xóa phần tử tại iterator pos, iterator phải hợp lệ.
-   `erase(first,last)` Xóa tất cả phần tử trong phạm vi iterator $[first,last)$.
-   `clear()` Xóa toàn bộ `set`.

???+ note "Giá trị trả về của hàm insert"
    Kiểu trả về của insert là `pair<iterator, bool>`, trong đó iterator trỏ tới phần tử được chèn (hoặc phần tử đã có giá trị bằng với giá trị chèn), còn bool biểu thị chèn thành công hay không. Do `set` có tính duy nhất nên nếu đã có phần tử bằng giá trị đó thì chèn thất bại, trả về false; ngược lại trả về true. `map` cũng tương tự.

### Iterator

`set` cung cấp các iterator sau:

1.  `begin()/cbegin()`   
    Trả về iterator trỏ đến phần tử đầu, trong đó `*begin = front`.
2.  `end()/cend()`   
    Trả về iterator trỏ đến vị trí sau phần tử cuối (không có phần tử).
3.  `rbegin()/crbegin()`   
    Trả về reverse iterator trỏ đến phần tử đầu của dãy đảo, có thể hiểu là phần tử cuối của container thường.
4.  `rend()/crend()`   
    Trả về reverse iterator trỏ đến vị trí sau phần tử cuối của dãy đảo (trước phần tử đầu), không có phần tử.

Trong các iterator trên, iterator có ký tự `c` là iterator chỉ đọc; không thể dùng để sửa giá trị phần tử. Nếu `set` là hằng thì iterator thường và iterator chỉ đọc là tương đương. Iterator chỉ đọc được hỗ trợ từ C++11.

### Thao tác tìm kiếm

-   `count(x)` Trả về số phần tử có khóa x trong `set`.
-   `find(x)` Nếu có phần tử khóa x thì trả về iterator tới phần tử đó, nếu không trả về `end()`.
-   `lower_bound(x)` Trả về iterator trỏ tới phần tử đầu tiên không nhỏ hơn khóa cho trước. Nếu không có, trả về `end()`.
-   `upper_bound(x)` Trả về iterator trỏ tới phần tử đầu tiên lớn hơn khóa cho trước. Nếu không có, trả về `end()`.
-   `empty()` Trả về container có rỗng hay không.
-   `size()` Trả về số phần tử trong container.

???+ warning "Độ phức tạp của `lower_bound` và `upper_bound`"
    `lower_bound` và `upper_bound` của `set` có độ phức tạp $O(\log n)$.
    
    Nhưng nếu dùng `lower_bound` và `upper_bound` trong thư viện `algorithm` để truy vấn `set` thì độ phức tạp là $O(n)$.

???+ warning "Độ phức tạp của `nth_element`"
    `set` không có `nth_element`. Dùng `nth_element` trong `algorithm` để tìm phần tử lớn thứ $k$ có độ phức tạp $O(n)$.
    
    Nếu cần chức năng tìm phần tử lớn thứ $k$ với $O(\log n)$ như cây cân bằng, phải tự cài cây cân bằng hoặc cây đoạn theo giá trị, hoặc dùng cây cân bằng trong thư viện pb_ds.

### Ví dụ sử dụng

#### `set` trong greedy

Trong thuật toán greedy thường cần thao tác kiểu **tìm và xóa phần tử nhỏ nhất mà >= một giá trị nào đó**. Thao tác này dễ dàng thực hiện với `set`.

```cpp
// Các phần tử hiện có
set<int> available;
// Giá trị cần >=
int x;

// Tìm phần tử nhỏ nhất >= x
set<int>::iterator it = available.lower_bound(x);
if (it == available.end()) {
  // Không tồn tại phần tử như vậy, xử lý tương ứng...
} else {
  // Tìm thấy, xóa khỏi tập hiện có
  available.erase(it);
  // Xử lý tiếp...
}
```

## `map`

`map` là container cặp khóa-giá trị có thứ tự, khóa là duy nhất. Tìm kiếm, xóa, chèn có độ phức tạp logarit. `map` thường hiện thực bằng [cây đỏ-đen](../../ds/rbtree.md).

Hãy tưởng tượng cần lưu các cặp khóa-giá trị, ví dụ lưu tên học sinh và điểm: `Tom 0`, `Bob 100`, `Alan 100`. Vì chỉ số mảng chỉ là số nguyên không âm, không thể dùng tên làm chỉ số. Khi đó cách đơn giản nhất là dùng `map` trong STL.

`map` nạp chồng `operator[]`, có thể dùng bất kỳ kiểu nào định nghĩa `operator <` làm chỉ số (trong `map` gọi là `key`):

```cpp
map<Key, T> yourMap;
```

Trong đó, `Key` là kiểu khóa, `T` là kiểu giá trị; ví dụ:

```cpp
map<string, int> mp;
```

`map` không chứa phần tử có khóa trùng; `multimap` cho phép nhiều phần tử có cùng khóa. Cách dùng `multimap` gần như giống `map`.

??? warning "Warning"
    Do `multimap` cho phép nhiều phần tử cùng khóa, nên không cung cấp cách truy cập giá trị bằng khóa.

### Thao tác chèn và xóa

-   Có thể truy cập/chèn trực tiếp bằng chỉ số, ví dụ `mp["Alan"]=100`.
-   Chèn một cặp `pair<Key, T>` vào `map`, ví dụ `mp.insert(pair<string,int>("Alan",100));`.
-   `erase(key)` sẽ xóa **tất cả** phần tử có khóa `key`. Trả về số phần tử bị xóa.
-   `erase(pos)`: xóa phần tử tại iterator pos, iterator phải hợp lệ.
-   `erase(first,last)`: xóa các phần tử trong phạm vi iterator $[first,last)$.
-   `clear()` xóa toàn bộ container.

???+ note "Lưu ý khi truy cập bằng chỉ số"
    Khi truy cập `map` bằng chỉ số, nếu không tồn tại khóa tương ứng, `map` sẽ tự chèn một phần tử mới với giá trị mặc định (với số nguyên là 0; với kiểu có constructor mặc định thì gọi constructor).
    
    Nếu thao tác truy cập bằng chỉ số quá thường xuyên, container sẽ có nhiều phần tử vô nghĩa, làm giảm hiệu quả. Vì vậy thường khuyến nghị dùng `find()` để tìm khóa cụ thể.

### Thao tác truy vấn

-   `count(x)`: trả về số phần tử có khóa x. Độ phức tạp $O(\log(size)+ans)$ (log kích thước cộng số lượng khớp).
-   `find(x)`: nếu có khóa x thì trả về iterator; nếu không trả về `end()`.
-   `lower_bound(x)`: trả về iterator trỏ tới phần tử đầu tiên không nhỏ hơn khóa cho trước.
-   `upper_bound(x)`: trả về iterator trỏ tới phần tử đầu tiên lớn hơn khóa cho trước. Nếu tất cả phần tử <= khóa, trả về `end()`.
-   `empty()`: trả về container có rỗng hay không.
-   `size()`: trả về số phần tử.

### Ví dụ sử dụng

#### `map` dùng để lưu trạng thái phức tạp

Trong tìm kiếm, đôi khi cần lưu trạng thái phức tạp (tọa độ, số không rời rạc được, chuỗi...) và đáp án liên quan (ví dụ số bước nhỏ nhất). `map` phù hợp cho mục đích này: khóa là trạng thái, giá trị là đáp án. Ví dụ dưới lưu trạng thái bằng `string`.

```cpp
// Lưu trạng thái và đáp án tương ứng
map<string, int> record;

// Trạng thái mới và đáp án tương ứng
string status;
int ans;
// Tìm xem trạng thái đã xuất hiện chưa
map<string, int>::iterator it = record.find(status);
if (it == record.end()) {
  // Chưa từng xuất hiện, thêm vào
  record[status] = ans;
  // Xử lý tiếp...
} else {
  // Đã xuất hiện, xử lý tương ứng...
}
```

## Duyệt container

Có thể dùng iterator để duyệt tất cả phần tử của container liên kết.

```cpp
set<int> s;
using si = set<int>::iterator;
for (si it = s.begin(); it != s.end(); it++) cout << *it << endl;
```

Lưu ý: dereference iterator của `map` sẽ trả về `pair<Key, T>`.

Trong C++11, dùng range-for sẽ gọn hơn:

```cpp
set<int> s;
for (auto x : s) cout << x << endl;
```

Với mọi container liên kết, duyệt bằng iterator có độ phức tạp $O(n)$.

## Tùy biến phép so sánh

Mặc định `set` dùng phép so sánh `<` (nếu là kiểu tự định nghĩa thì cần [nạp chồng `<`](../op-overload.md#比较运算符)). Trong một số trường hợp cần tự định nghĩa cách so sánh nội bộ.

Có thể truyền vào comparator tự định nghĩa.

Cụ thể, định nghĩa một lớp và [nạp chồng `()`](../op-overload.md#函数调用运算符).

Ví dụ, muốn `set` lưu số nguyên nhưng giá trị lớn hơn đứng trước:

```cpp
struct cmp {
  bool operator()(int a, int b) const { return a > b; }
};

set<int, cmp> s;
```

Các container liên kết khác có thể làm tương tự, không trình bày thêm.
