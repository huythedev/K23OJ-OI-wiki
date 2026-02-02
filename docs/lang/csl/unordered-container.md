## Tổng quan

Từ C++11, bốn loại container liên kết không thứ tự dựa trên [băm](../../ds/hash.md) chính thức vào STL: `unordered_set`, `unordered_multiset`, `unordered_map`, `unordered_multimap`.

??? note "Cách dùng khi compiler không hỗ trợ C++11"
    Trước C++11, các container không thứ tự thuộc TR1. Vì vậy nếu compiler không hỗ trợ C++11, cần thêm tiền tố `tr1/` vào tên header và dùng namespace `std::tr1`. Ví dụ `#include <unordered_map>` đổi thành `#include <tr1/unordered_map>`; `std::unordered_map` đổi thành `std::tr1::unordered_map` (nếu `using namespace std;` thì là `tr1::unordered_map`).

Chúng có nhiều điểm chung với container liên kết tương ứng, nhưng khác biệt lớn nhất là: container liên kết thường dùng cây đỏ-đen, phần tử có thứ tự; còn container không thứ tự dùng băm, phần tử không theo bất kỳ thứ tự nào, nên thứ tự truy cập không đảm bảo.

Đặc tính băm giúp các thao tác (tìm, chèn, xóa) **trung bình** là thời gian hằng số, tốt hơn so với container liên kết có độ phức tạp log.

??? warning "Warning"
    Trong trường hợp xấu nhất, chèn/xóa/tìm có thể **tuyến tính theo kích thước container**! Điều này thường xảy ra khi có nhiều xung đột băm.
    
    Đồng thời, do hằng số ẩn lớn, hiệu năng đôi khi không vượt trội so với container liên kết.
    
    Vì vậy cần dùng thận trọng, tránh lạm dụng (ví dụ không muốn rời rạc hóa mà dùng `unordered_map<int, int>` như mảng vô hạn).

Do các thao tác tương tự container liên kết, phần này không lặp lại; xem [container liên kết](./associative-container.md).

## Tạo xung đột băm

Ở trường hợp xấu nhất, độ phức tạp có thể tuyến tính.

Với hàm băm cố định, có thể dựng dữ liệu tạo nhiều xung đột, đẩy độ phức tạp lên cao.

Trong hiện thực thư viện chuẩn, giá trị băm là lấy modulo một số nguyên tố, cụ thể là [danh sách này](https://github.com/gcc-mirror/gcc/blob/releases/gcc-8.1.0/libstdc%2B%2B-v3/src/shared/hashtable-aux.cc) (g++ 6 trở về trước thường là $126271$, g++ 7 trở đi thường là $107897$).

Vì vậy có thể chèn các bội của mô-đun để tạo nhiều xung đột băm.

## Tùy biến hàm băm

Dùng hàm băm tự định nghĩa có thể tránh các bộ dữ liệu gây xung đột lớn.

Cần định nghĩa một struct và nạp chồng toán tử `()`:

```cpp
struct my_hash {
  size_t operator()(int x) const { return x; }
};
```

Để tránh bị hack (ví dụ Codeforces), có thể thêm ngẫu nhiên hóa (như thời gian) để tăng độ khó phá.

Ví dụ từ [bài blog này](https://codeforces.com/blog/entry/62393):

```cpp
struct my_hash {
  static uint64_t splitmix64(uint64_t x) {
    x += 0x9e3779b97f4a7c15;
    x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
    x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
    return x ^ (x >> 31);
  }

  size_t operator()(uint64_t x) const {
    static const uint64_t FIXED_RANDOM =
        chrono::steady_clock::now().time_since_epoch().count();
    return splitmix64(x + FIXED_RANDOM);
  }

  // Hàm băm cho std::pair<int, int> làm key
  size_t operator()(pair<uint64_t, uint64_t> x) const {
    static const uint64_t FIXED_RANDOM =
        chrono::steady_clock::now().time_since_epoch().count();
    return splitmix64(x.first + FIXED_RANDOM) ^
           (splitmix64(x.second + FIXED_RANDOM) >> 1);
  }
};
```

Sau đó dùng `unordered_map<int, int, my_hash> my_map;` hoặc `unordered_map<pair<int, int>, int, my_hash> my_pair_map;` để truyền hàm băm tự định nghĩa vào container.
