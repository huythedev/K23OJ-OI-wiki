author: GavinZhengOI, PlanariaIce

## Giới thiệu

Rời rạc hóa là một kỹ thuật xử lý dữ liệu, về bản chất có thể xem như một dạng [băm](../string/hash.md#hash-的思想), đảm bảo sau khi băm vẫn giữ được quan hệ [toàn/phần thứ tự](../math/order-theory.md#偏序集) ban đầu.

Nói một cách dễ hiểu, khi một số dữ liệu quá lớn hoặc kiểu dữ liệu không hỗ trợ nên không thể dùng trực tiếp làm chỉ số mảng để xử lý, trong khi thứ tự tương đối giữa các phần tử mới là yếu tố ảnh hưởng kết quả, ta có thể xử lý dữ liệu theo thứ hạng của chúng, tức là rời rạc hóa.

Dữ liệu cần rời rạc có thể là số nguyên lớn, số thực, chuỗi, v.v.

## Cài đặt

Rời rạc một mảng và hỗ trợ truy vấn là tình huống khá phổ biến.

### Cách 1

Thông thường mảng gốc có phần tử trùng nhau, và ta rời rạc các phần tử trùng thành cùng một giá trị.

Các bước:

1.  Tạo bản sao của mảng gốc.

2.  Sắp xếp bản sao theo thứ tự tăng dần.

3.  Loại bỏ phần tử trùng lặp trong bản sao đã sắp xếp.

4.  Với mỗi phần tử của mảng gốc, tìm vị trí của nó trong bản sao, vị trí chính là thứ hạng và là giá trị sau rời rạc.

```cpp
// arr[i] là mảng ban đầu, chỉ số trong [1, n]

for (int i = 1; i <= n; ++i)  // bước 1
  tmp[i] = arr[i];
std::sort(tmp + 1, tmp + n + 1);                          // bước 2
int len = std::unique(tmp + 1, tmp + n + 1) - (tmp + 1);  // bước 3
for (int i = 1; i <= n; ++i)                              // bước 4
  arr[i] = std::lower_bound(tmp + 1, tmp + len + 1, arr[i]) - tmp;
```

Các thuật toán STL dùng trong đoạn này có thể xem tại [STL 算法](../lang/csl/algorithm.md).

Tương tự, ta cũng có thể rời rạc [std::vector](../lang/csl/sequence-container.md#vector):

```cpp
// std::vector<int> arr;
std::vector<int> tmp(arr);  // tmp là bản sao của arr
std::sort(tmp.begin(), tmp.end());
tmp.erase(std::unique(tmp.begin(), tmp.end()), tmp.end());
for (int i = 0; i < n; ++i)
  arr[i] = std::lower_bound(tmp.begin(), tmp.end(), arr[i]) - tmp.begin();
```

### Cách 2

Theo yêu cầu đề bài, đôi khi các phần tử bằng nhau cần được rời rạc thành các giá trị khác nhau theo thứ tự xuất hiện.

Khi đó dùng `std::lower_bound()` sẽ khó, cần đổi cách làm:

1.  Tạo bản sao của mảng gốc, đồng thời lưu vị trí xuất hiện của mỗi phần tử.

2.  Sắp xếp bản sao theo giá trị tăng dần; nếu giá trị bằng nhau thì theo thứ tự xuất hiện tăng dần.

3.  Gán giá trị rời rạc trở lại mảng gốc.

```cpp
struct Data {
  int idx, val;

  bool operator<(const Data& o) const {
    if (val == o.val)
      return idx < o.idx;  // khi giá trị bằng nhau, phần tử xuất hiện trước có giá trị rời rạc nhỏ hơn
    return val < o.val;
  }
} tmp[MAXN];  // cũng có thể dùng std::pair

for (int i = 1; i <= n; ++i) tmp[i] = Data{i, arr[i]};
std::sort(tmp + 1, tmp + n + 1);
for (int i = 1; i <= n; ++i) arr[tmp[i].idx] = i;
```

### Độ phức tạp

Với cách 1, bỏ trùng có độ phức tạp $O(n)$, sắp xếp là $O(n \log n)$, cuối cùng $n$ lần tìm kiếm là $O(n \log n)$.

Với cách 2, độ phức tạp sắp xếp là $O(n \log n)$.

Vì vậy tổng độ phức tạp thời gian của cả hai cách đều là $O(n \log n)$.

Độ phức tạp bộ nhớ là $O(n)$.

## Bài tập

-   [\[HAOI2014\] 贴海报](https://www.luogu.com.cn/problem/P3740)
-   [\[NOI2015\] 程序自动分析](https://www.luogu.com.cn/problem/P1955)
