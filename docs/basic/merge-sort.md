## Định nghĩa

**Sắp xếp trộn** ([merge sort](https://en.wikipedia.org/wiki/Merge_sort)) là một thuật toán sắp xếp ổn định, hiệu quả dựa trên so sánh.

## Tính chất

Sắp xếp trộn dựa trên tư tưởng chia để trị, chia mảng thành các đoạn nhỏ rồi hợp lại, có độ phức tạp thời gian tốt nhất, xấu nhất và trung bình đều là $\Theta (n \log n)$, độ phức tạp bộ nhớ là $\Theta (n)$.

Sắp xếp trộn có thể chỉ dùng $\Theta (1)$ bộ nhớ phụ, nhưng thường để tiện lợi sẽ dùng một mảng phụ có độ dài bằng mảng gốc.

## Quy trình

### Hợp mảng (merge)

Phần cốt lõi của merge sort là quá trình hợp hai mảng: hợp hai mảng đã sắp xếp `a[i]` và `b[j]` thành một mảng đã sắp xếp `c[k]`.

Duyệt từ trái sang phải các phần tử `a[i]` và `b[j]`, chọn giá trị nhỏ nhất đưa vào `c[k]`; lặp lại cho đến khi một trong hai mảng hết phần tử, sau đó đưa nốt phần còn lại của mảng kia vào `c[k]`.

Để đảm bảo tính ổn định, khi hai phần tử đầu bằng nhau (`a[i] <= b[j]`), phải ưu tiên lấy phần tử ở mảng trước.

#### Cài đặt

=== "C/C++"
    === "Cài đặt dùng mảng"
        ```cpp
        void merge(const int *a, size_t aLen, const int *b, size_t bLen, int *c) {
          size_t i = 0, j = 0, k = 0;
          while (i < aLen && j < bLen) {
            if (b[j] < a[i]) {  // <!> 先判断 b[j] < a[i]，保证稳定性
              c[k] = b[j];
              ++j;
            } else {
              c[k] = a[i];
              ++i;
            }
            ++k;
          }
          // 此时一个数组已空，另一个数组非空，将非空的数组并入 c 中
          for (; i < aLen; ++i, ++k) c[k] = a[i];
          for (; j < bLen; ++j, ++k) c[k] = b[j];
        }
        ```
    
    === "Cài đặt dùng con trỏ"
        ```cpp
        void merge(const int *aBegin, const int *aEnd, const int *bBegin,
                   const int *bEnd, int *c) {
          while (aBegin != aEnd && bBegin != bEnd) {
            if (*bBegin < *aBegin) {
              *c = *bBegin;
              ++bBegin;
            } else {
              *c = *aBegin;
              ++aBegin;
            }
            ++c;
          }
          for (; aBegin != aEnd; ++aBegin, ++c) *c = *aBegin;
          for (; bBegin != bEnd; ++bBegin, ++c) *c = *bBegin;
        }
        ```
    
    Có thể dùng hàm `merge` trong `<algorithm>`, cách dùng giống như kiểu con trỏ ở trên.

=== "Python"
    ```python
    def merge(a, b):
        i, j = 0, 0
        c = []
        while i < len(a) and j < len(b):
            # <!> 先判断 b[j] < a[i]，保证稳定性
            if b[j] < a[i]:
                c.append(b[j])
                j += 1
            else:
                c.append(a[i])
                i += 1
        # 此时一个数组已空，另一个数组非空，将非空的数组并入 c 中
        c.extend(a[i:])
        c.extend(b[j:])
        return c
    ```

### Cài đặt merge sort bằng chia để trị

1.  Khi mảng chỉ còn 1 phần tử, nó đã được sắp xếp, không cần chia nhỏ nữa.

2.  Khi mảng có nhiều hơn 1 phần tử, chia thành hai đoạn, kiểm tra từng đoạn đã sắp xếp chưa (bước 1). Nếu chưa, tiếp tục chia nhỏ (bước 2), sau đó hợp lại.

Có thể chứng minh bằng quy nạp rằng quá trình này sẽ sắp xếp được mảng.

Để đảm bảo độ phức tạp, thường chia mảng thành hai đoạn gần bằng nhau ($mid = \left\lfloor \dfrac{l + r}{2} \right\rfloor$).

#### Cài đặt

Lưu ý các đoạn code dưới đây dùng các đoạn $[l, r)$, $[l, mid)$, $[mid, r)$.

=== "C/C++"
    ```cpp
    void merge_sort(int *a, int l, int r) {
      if (r - l <= 1) return;
      // 分解
      int mid = l + ((r - l) >> 1);
      merge_sort(a, l, mid), merge_sort(a, mid, r);
      // 合并
      int tmp[1024] = {};  // 请结合实际情况设置 tmp 数组的长度（与 a 相同），或使用
                           // vector；先将合并的结果放在 tmp 里，再返回到数组 a
      merge(a + l, a + mid, a + mid, a + r, tmp + l);  // pointer-style merge
      for (int i = l; i < r; ++i) a[i] = tmp[i];
    }
    ```

=== "Python"
    ```python
    def merge_sort(a, ll, rr):
        if rr - ll <= 1:
            return
        # 分解
        mid = (rr + ll) // 2
        merge_sort(a, ll, mid)
        merge_sort(a, mid, rr)
        # 合并
        a[ll:rr] = merge(a[ll:mid], a[mid:rr])
    ```

### Cài đặt merge sort bằng phương pháp tăng dần đoạn (bottom-up)

Khi mảng có độ dài 1, mỗi đoạn đã được sắp xếp.

Từ trái sang phải, hợp từng cặp đoạn độ dài 1 thành các đoạn độ dài $\leq 2$ đã sắp xếp;

Tiếp tục hợp từng cặp đoạn độ dài $\leq 2$ thành các đoạn độ dài $\leq 4$ đã sắp xếp;

Tiếp tục hợp từng cặp đoạn độ dài $\leq 4$ thành các đoạn độ dài $\leq 8$ đã sắp xếp;

...

Lặp lại cho đến khi chỉ còn một đoạn, đó là mảng đã sắp xếp.

???+ note "Tại sao là $\le n$ mà không phải $= n$"
    Độ dài mảng có thể không phải là $2^x$，nên đoạn cuối cùng có thể không đủ，có thể có đoạn lẻ.

#### Cài đặt

=== "C/C++"
    ```cpp
    void merge_sort(int *a, size_t n) {
      int tmp[1024] = {};  // 请结合实际情况设置 tmp 数组的长度（与 a 相同），或使用
                           // vector；先将合并的结果放在 tmp 里，再返回到数组 a
      for (size_t seg = 1; seg < n; seg <<= 1) {
        for (size_t left1 = 0; left1 < n - seg;
             left1 += seg + seg) {  // n - seg: 如果最后只有一个段就不用合并
          size_t right1 = left1 + seg;
          size_t left2 = right1;
          size_t right2 = std::min(left2 + seg, n);  // <!> 注意最后一个段的边界
          merge(a + left1, a + right1, a + left2, a + right2,
                tmp + left1);  // pointer-style merge
          for (size_t i = left1; i < right2; ++i) a[i] = tmp[i];
        }
      }
    }
    ```

=== "Python"
    ```python
    def merge_sort(a):
        seg = 1
        while seg < len(a):
            for l1 in range(0, len(a) - seg, seg + seg):
                r1 = l1 + seg
                l2 = r1
                r2 = l2 + seg
                a[l1:r2] = merge(a[l1:r1], a[l2:r2])
        seg <<= 1
    ```

## Đếm số nghịch thế (inversion)

Xem thêm và code mẫu: [Nghịch thế](../math/permutation.md#逆序数)

Nghịch thế là các cặp $(i, j)$ với $i < j$ và $a_i > a_j$.

Sau khi sắp xếp, mảng không còn nghịch thế. Trong quá trình hợp mảng của merge sort, mỗi lần lấy phần tử đầu của đoạn sau làm giá trị nhỏ nhất, số phần tử còn lại ở đoạn trước chính là số nghịch thế bị loại bỏ; do đó merge sort đếm nghịch thế trong thời gian $\Theta (n \log n)$. Ngoài ra, có thể dùng Fenwick tree hoặc segment tree để đếm nghịch thế với $O(n \log n)$; xem chi tiết ở [Fenwick tree](../ds/fenwick.md#全局逆序对全局二维偏序). Code mẫu ở [Nghịch thế](../math/permutation.md#逆序数).

## Liên kết ngoài

-   [Merge Sort - GeeksforGeeks](https://www.geeksforgeeks.org/merge-sort/)
-   [归并排序 - Wikipedia tiếng Trung](https://zh.wikipedia.org/wiki/%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F)
-   [逆序对 - Wikipedia tiếng Trung](https://zh.wikipedia.org/wiki/%E9%80%86%E5%BA%8F%E5%AF%B9)
