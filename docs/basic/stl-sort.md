Trang này giới thiệu ngắn gọn về các hàm sắp xếp trong thư viện chuẩn C và C++.

Ngoài các hàm đã nêu, mặc định các hàm trong trang này được định nghĩa trong header `<algorithm>`.

## qsort

Tham khảo: [`qsort`](https://zh.cppreference.com/w/c/algorithm/qsort)，[`std::qsort`](https://zh.cppreference.com/w/cpp/algorithm/qsort)

Hàm này là phần của chuẩn C, thực hiện [quick sort](./quick-sort.md) (thuật toán sắp xếp), được định nghĩa trong `<stdlib.h>` (C) và `<cstdlib>` (C++) .

### Hàm so sánh cho qsort và bsearch

Hàm qsort có bốn tham số: tên mảng, số phần tử, kích thước phần tử và hàm so sánh. Hàm so sánh được biểu diễn bởi một hàm nhận hai con trỏ const void; giá trị trả về là số âm, 0 hoặc số dương để chỉ quan hệ thứ tự.

Ví dụ một hàm so sánh cho mảng int:

```c
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
int compare(const void *p1, const void *p2)  // hàm so sánh cho mảng int
{
  int *a = (int *)p1;
  int *b = (int *)p2;
  if (*a > *b)
    return 1;  // trả về số dương biểu thị a lớn hơn b
  else if (*a < *b)
    return -1;  // trả về số âm biểu thị a nhỏ hơn b
  else
    return 0;  // trả về 0 biểu thị a và b tương đương theo quy tắc so sánh
}
```

Lưu ý: thay vì trả về hiệu hai phần tử là một lỗi phổ biến vì có thể gây tràn số.

Ví dụ với struct:

```c
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
struct eg  // ví dụ struct
{
  int e;
  int g;
};

int compare(const void *p1,
            const void *p2)  // hàm so sánh cho mảng struct eg: theo thành viên e
{
  struct eg *a = (struct eg *)p1;
  struct eg *b = (struct eg *)p2;
  if (a->e > b->e)
    return 1;  // trả về số dương biểu thị a lớn hơn b
  else if (a->e < b->e)
    return -1;  // trả về số âm biểu thị a nhỏ hơn b
  else
    return 0;  // trả về 0 biểu thị a và b tương đương theo quy tắc so sánh
}
```

Ở đây lưu ý: tương đương (equivalent) không nhất thiết là bằng nhau tuyệt đối, mà chỉ là tương đương theo quy tắc so sánh.

## std::sort

Tham khảo: [`std::sort`](https://zh.cppreference.com/w/cpp/algorithm/sort)

Cách dùng:

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
// a[0] .. a[n - 1] là dãy cần sắp xếp
// Sắp xếp tại chỗ (in-place) dãy a theo thứ tự tăng dần
std::sort(a, a + n);

// cmp là hàm so sánh tùy chỉnh
std::sort(a, a + n, cmp);
```

Lưu ý: Hàm so sánh của std::sort trả về true/false để biểu diễn quan hệ thứ tự (precedence), khác với qsort trả về giá trị âm/0/dương. Nếu chuyển đổi về qsort thì phải đổi true -> -1, false -> 1 để giữ thứ tự tương đương (không xét các phần tử tương đương).

std::sort là hàm thường dùng trong C++. Tham số cuối cùng là hàm nhị phân so sánh; nếu không chỉ định sẽ sắp theo thứ tự tăng dần.

Các chuẩn C++ cũ chỉ yêu cầu độ phức tạp trung bình O(n log n); từ C++11 trở đi yêu cầu độ phức tạp tồi nhất cũng là O(n log n). Triển khai cụ thể phụ thuộc vào thư viện chuẩn của trình biên dịch; libstdc++ và libc++ đều dùng introsort (xem ./quick-sort.md#内省排序).

## std::nth_element

Tham khảo: [`std::nth_element`](https://zh.cppreference.com/w/cpp/algorithm/nth_element)

Cách dùng:

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
std::nth_element(first, nth, last);
std::nth_element(first, nth, last, cmp);
```

Hàm này phân bố lại [first, last) sao cho phần tử tại vị trí nth sau khi sắp xếp sẽ là phần tử đúng tại vị trí đó; các phần tử trước nó nhỏ hơn hoặc bằng các phần tử sau nó. Triển khai dùng biến thể của introsort chưa hoàn chỉnh. Chuẩn yêu cầu độ phức tạp trung bình O(n), với n = std::distance(first, last). Thường dùng để xây K-D Tree (../ds/kdt.md).

## std::stable_sort

Tham khảo: [`std::stable_sort`](https://zh.cppreference.com/w/cpp/algorithm/stable_sort)

Cách dùng:

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
std::stable_sort(first, last);
std::stable_sort(first, last, cmp);
```

Đây là sắp xếp ổn định — các phần tử bằng nhau sẽ giữ thứ tự tương đối ban đầu. Độ phức tạp O(n log^2 n); nếu có bộ nhớ phụ thì O(n log n).

## std::partial_sort

Tham khảo: [`std::partial_sort`](https://zh.cppreference.com/w/cpp/algorithm/partial_sort)

Cách dùng:

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
// mid = first + k
std::partial_sort(first, mid, last);
std::partial_sort(first, mid, last, cmp);
```

Sắp xếp tại chỗ k phần tử đầu tiên theo cmp; phần còn lại không đảm bảo thứ tự. Độ phức tạp xấp xỉ (last-first) * log(mid-first).

Nguyên lý: xây một max-heap trên [first, mid) bằng make_heap(), rồi duyệt [mid, last), so sánh với phần tử lớn nhất trong heap (first); nếu nhỏ hơn thì hoán đổi và điều chỉnh heap; cuối cùng sort_heap() trên [first, mid) để đưa về thứ tự tăng dần.

## Tùy biến hàm so sánh

Tham khảo: [Nạp chồng toán tử](https://zh.cppreference.com/w/cpp/language/operators)

Kiểu cơ bản (int...) và struct người dùng cho phép truyền hàm so sánh khi gọi các hàm sắp xếp STL. Có thể truyền một hàm nhị phân ở tham số cuối cùng. Với struct người dùng, nên định nghĩa operator< hoặc truyền hàm so sánh. Thông thường nên định nghĩa operator<.[^note1]

Ví dụ:

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
int a[1009], n = 10;
// ...
std::sort(a + 1, a + 1 + n);                  // sắp tăng dần
std::sort(a + 1, a + 1 + n, greater<int>());  // sắp giảm dần
```

```cpp
// filepath: /home/ubuntu/K23OJ-OI-wiki/docs/basic/stl-sort.md
struct data {
  int a, b;

  bool operator<(const data rhs) const {
    return (a == rhs.a) ? (b < rhs.b) : (a < rhs.a);
  }
} da[1009];

bool cmp(const data u1, const data u2) {
  return (u1.a == u2.a) ? (u1.b > u2.b) : (u1.a > u2.a);
}

// ...
std::sort(da + 1, da + 1 + 10);  // dùng operator< trong struct, tăng dần
std::sort(da + 1, da + 1 + 10, cmp);  // dùng cmp để sắp giảm dần
```

### Strict weak ordering (Thứ tự yếu nghiêm ngặt)

Xem thêm: [Ứng dụng trong C++ - Lý thuyết thứ tự](../math/order-theory.md#c-中的应用)

Hàm so sánh phải thỏa mãn tính "strict weak ordering", nếu không sẽ gây hành vi không xác định (lỗi thời gian chạy, sắp không đúng, ...).

Lỗi thường gặp:

- Dùng <= để định nghĩa operator<.
- Hàm so sánh phụ thuộc vào dữ liệu bên ngoài có thể thay đổi khi gọi (ví dụ trong một số thuật toán đường đi ngắn).
- Dùng kết quả so sánh của nhiều giá trị làm cơ sở (ví dụ một số bài toán sắp xếp đặc thù).

## Liên kết ngoài

-   [浅谈邻项交换排序的应用以及需要注意的问题](https://ouuan.github.io/浅谈邻项交换排序的应用以及需要注意的问题/)

## Tài liệu tham khảo và chú thích

[^note1]: Vì nhiều hàm chuẩn mặc định dùng `operator<`.
