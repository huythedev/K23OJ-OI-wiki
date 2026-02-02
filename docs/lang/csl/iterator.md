Trong STL, iterator (Iterator) là đối tượng dùng để truy cập và kiểm tra phần tử trong container STL. Hành vi giống con trỏ, nhưng có thêm kiểm tra hợp lệ và cung cấp cách truy cập thống nhất. Khái niệm tương tự cũng có trong nhiều ngôn ngữ khác, như `__iter__` của Python, `IEnumerator` của C#.

## Cách dùng cơ bản

Iterator nghe có vẻ trừu tượng, nhưng có thể xem như con trỏ dữ liệu. Iterator chủ yếu hỗ trợ hai toán tử: tăng (`++`) và giải tham chiếu (toán tử một ngôi `*`). Tăng để di chuyển iterator, giải tham chiếu để lấy hoặc sửa phần tử mà nó trỏ tới.

Kiểu iterator trỏ tới phần tử trong [container STL](./container.md) `container` thường là `container::iterator`.

Iterator có thể dùng để duyệt container; ví dụ hai vòng for dưới đây tương đương:

```cpp
vector<int> data(10);

for (int i = 0; i < data.size(); i++)
  cout << data[i] << endl;  // Truy cập bằng chỉ số

for (vector<int>::iterator iter = data.begin(); iter != data.end(); iter++)
  cout << *iter << endl;  // Truy cập bằng iterator
// Sau C++11 có thể dùng auto iter = data.begin() để rút gọn
```

???+ tip "`auto` trong thi đấu"
    Phần lớn thí sinh thích dùng `auto` thay cho khai báo iterator dài dòng. Theo [Thông báo bổ sung về giới hạn ngôn ngữ trong các kỳ thi NOI](https://www.noi.cn/xw/2021-09-01/735729.shtml) (09/2021), các kỳ NOI (bao gồm CSP J/S) sẽ dùng **C++14**, hỗ trợ `auto`.

## Phân loại

Trong STL, iterator được phân loại theo các thao tác hỗ trợ:

-   InputIterator (iterator đầu vào): chỉ yêu cầu copy, tăng, và giải tham chiếu để đọc.
-   OutputIterator (iterator đầu ra): chỉ yêu cầu copy, tăng, và giải tham chiếu để gán.
-   ForwardIterator (iterator tiến): ngoài InputIterator, hỗ trợ duyệt nhiều lần và đảm bảo kết quả nhất quán.
-   BidirectionalIterator (iterator hai chiều): ngoài ForwardIterator, hỗ trợ giảm (duyệt ngược).
-   RandomAccessIterator (iterator truy cập ngẫu nhiên): ngoài BidirectionalIterator, hỗ trợ cộng/trừ và so sánh (truy cập ngẫu nhiên).
-   ContiguousIterator (iterator liên tiếp): ngoài RandomAccessIterator, yêu cầu `*(a + n)` tương đương `*(std::address_of(*a) + n)` (lưu trữ liên tiếp), với `a` là iterator, `n` là số nguyên.

    ContiguousIterator được đưa vào chính thức từ C++17.

???+ tip "Vì sao gọi là iterator đầu vào?"
    “Đầu vào” nghĩa là “có thể lấy dữ liệu từ iterator”, “đầu ra” nghĩa là “có thể ghi dữ liệu vào iterator”.
    
    “Đầu vào/đầu ra” là từ góc nhìn của phần còn lại của chương trình, không phải từ iterator.

Các loại iterator không loại trừ nhau. Trên thực tế, trừ OutputIterator, các loại trước bao hàm các loại sau. Ví dụ nơi cần ForwardIterator vẫn có thể dùng BidirectionalIterator. Nếu iterator còn hỗ trợ ghi (như OutputIterator) thì gọi là iterator có thể sửa. Vì vậy có thể có các khái niệm như “iterator truy cập ngẫu nhiên có thể sửa”.

Các [container STL](./container.md) khác nhau hỗ trợ loại iterator khác nhau, cần lưu ý khi dùng.

Con trỏ mảng thỏa các yêu cầu của contiguous iterator (hoặc random access trong C++14 trở về trước), nên có thể dùng như contiguous iterator.

## Các hàm liên quan

Nhiều [hàm STL](./algorithm.md) dùng iterator làm tham số.

Có thể dùng `std::advance(it, n)` để dịch iterator `it` đi `n` bước; nếu `n` âm thì dịch ngược, khi đó iterator phải là bidirectional trở lên, иначе hành vi không xác định.

Trong C++11 trở đi, có thể dùng `std::next(it)` để lấy hậu kế của iterator tiến `it` (it không đổi), `std::next(it, n)` lấy hậu kế thứ `n`.

Trong C++11 trở đi, có thể dùng `std::prev(it)` để lấy tiền nhiệm của iterator hai chiều `it` (it không đổi), `std::prev(it, n)` lấy tiền nhiệm thứ `n`.

[Container STL](./container.md) thường hỗ trợ truy cập từ một hoặc hai đầu, và hỗ trợ [const](../const.md). Ví dụ `begin()` trả về iterator trỏ tới phần tử đầu, `rbegin()` trả về reverse iterator trỏ tới phần tử cuối, `cbegin()` trả về const iterator trỏ tới phần tử đầu, `end()` trả về iterator trỏ tới vị trí sau phần tử cuối (không trỏ phần tử nào; tiền nhiệm của `end()` là phần tử cuối).

Xem thêm tại [Iterator library - cppreference.com](https://en.cppreference.com/w/cpp/iterator).
