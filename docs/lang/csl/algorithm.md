STL cung cấp khoảng 100 hàm mẫu thuật toán, hầu hết nằm trong `<algorithm>`, một phần nằm trong `<numeric>` và `<functional>`. Danh sách đầy đủ [xem tại cppreference](https://zh.cppreference.com/w/cpp/algorithm), phần liên quan đến sắp xếp có thể tham khảo [trang sắp xếp](../../basic/stl-sort.md).

-   `find`: tìm tuyến tính. `find(v.begin(), v.end(), value)`, trong đó `value` là giá trị cần tìm.

-   `reverse`: đảo ngược mảng/chuỗi. `reverse(v.begin(), v.end())` hoặc `reverse(a + begin, a + end)`.

-   `unique`: loại bỏ các phần tử trùng nhau **liền kề** trong container. `unique(ForwardIterator first, ForwardIterator last)`, trả về iterator trỏ tới **cuối sau khi loại trùng**, kích thước container gốc không đổi. Kết hợp với `sort` có thể loại trùng toàn bộ.

-   `random_shuffle`: xáo trộn ngẫu nhiên mảng. `random_shuffle(v.begin(), v.end())` hoặc `random_shuffle(v + begin, v + end)`.

    ???+ warning "`random_shuffle` đã bị loại khỏi chuẩn C++ mới"
        `random_shuffle` bị deprecate từ C++14 và bị loại bỏ từ C++17.
        
        Trong C++11 trở đi, có thể dùng `shuffle` thay thế. Cú pháp: `shuffle(v.begin(), v.end(), rng)` (tham số cuối là bộ sinh số ngẫu nhiên, thường dùng `mt19937` với seed từ `random_device`).
        
        ```cpp
        // #include <random>
        std::mt19937 rng(std::random_device{}());
        std::shuffle(v.begin(), v.end(), rng);
        ```

-   `sort`: sắp xếp. `sort(v.begin(), v.end(), cmp)` hoặc `sort(a + begin, a + end, cmp)`, trong đó `end` là vị trí sau phần tử cuối cần sắp xếp, `cmp` là hàm so sánh tự định nghĩa.

-   `stable_sort`: sắp xếp ổn định, cách dùng như `sort()`.

-   `nth_element`: phân loại theo vị trí, tìm phần tử lớn thứ $n$, bên trái nhỏ hơn nó, bên phải lớn hơn nó. `nth_element(v.begin(), v.begin() + n, v.end(), cmp)` hoặc `nth_element(a + begin, a + begin + n, a + end, cmp)`.

-   `binary_search`: tìm kiếm nhị phân. `binary_search(v.begin(), v.end(), value)`, trong đó `value` là giá trị cần tìm.

-   `merge`: **gộp có thứ tự** hai dãy đã sắp xếp vào **inserter** của dãy thứ ba. `merge(v1.begin(), v1.end(), v2.begin(), v2.end() ,back_inserter(v3))`.

-   `inplace_merge`: gộp **tại chỗ** hai đoạn `[first,middle), [middle,last)` đã sắp theo toán tử `<` thành một dãy có thứ tự. `inplace_merge(v.begin(), v.begin() + middle, v.end())`.

-   `lower_bound`: tìm nhị phân trong dãy có thứ tự, trả về iterator trỏ tới phần tử đầu tiên **không nhỏ hơn** $x$. Nếu không tồn tại thì trả về end. `lower_bound(v.begin(),v.end(),x)`.

-   `upper_bound`: tìm nhị phân trong dãy có thứ tự, trả về iterator trỏ tới phần tử đầu tiên **lớn hơn** $x$. Nếu không tồn tại thì trả về end. `upper_bound(v.begin(),v.end(),x)`.

    ???+ warning "Độ phức tạp của `lower_bound` và `upper_bound`"
        Với mảng thông thường, thời gian là $O(\log n)$, nhưng trong container liên kết như `set`, gọi `lower_bound(s.begin(),s.end(),val)` sẽ là $O(n)$.
        
        `set` có sẵn `s.lower_bound(val)` với độ phức tạp $O(\log n)$.

-   `next_permutation`: chuyển hoán vị hiện tại sang **hoán vị kế tiếp**. Nếu đã là **hoán vị cuối** (giảm dần), trả `false` và đổi về **hoán vị đầu** (tăng dần); ngược lại trả `true`. `next_permutation(v.begin(), v.end())` hoặc `next_permutation(v + begin, v + end)`.

-   `prev_permutation`: chuyển hoán vị hiện tại sang **hoán vị trước đó**. Cách dùng như `next_permutation`.

-   `partial_sum`: tính tổng tiền tố. Với nguồn $x$ và đích $y$, `y[i]=x[0]+x[1]+\dots+x[i]`. `partial_sum(src.begin(), src.end(), back_inserter(dst))`.

### Ví dụ sử dụng

-   Dùng `next_permutation` sinh hoán vị của $1$ đến $9$. Bài: [Luogu P1706 全排列问题](https://www.luogu.com.cn/problem/P1706)

    ???+ note "Cài đặt"
        ```cpp
        int N = 9, a[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
        do {
          for (int i = 0; i < N; i++) cout << a[i] << " ";
          cout << endl;
        } while (next_permutation(a, a + N));
        ```
-   Dùng `lower_bound` và `upper_bound` tìm ranh giới các phần tử < $x$, = $x$, > $x$ trong mảng tăng $a$.

    ???+ note "Cài đặt"
        ```cpp
        int N = 10, a[] = {1, 1, 2, 4, 5, 5, 7, 7, 9, 9}, x = 5;
        int i = lower_bound(a, a + N, x) - a, j = upper_bound(a, a + N, x) - a;
        // a[0] ~ a[i - 1] là các phần tử < x, a[i] ~ a[j - 1] là = x,
        // a[j] ~ a[N - 1] là > x
        cout << i << " " << j << endl;
        ```
-   Dùng `partial_sum` tính tổng tiền tố của $src$ và lưu vào $dst$.

    ???+ note "Cài đặt"
        ```cpp
        vector<int> src = {1, 2, 3, 4, 5}, dst;
        // Tính tổng tiền tố của src, dst[i] = src[0] + ... + src[i]
        // back_inserter tác động lên dst, cung cấp một iterator
        partial_sum(src.begin(), src.end(), back_inserter(dst));
        for (unsigned int i = 0; i < dst.size(); i++) cout << dst[i] << " ";
        ```
-   Dùng `lower_bound` tìm phần tử gần $x$ nhất trong mảng tăng $a$. Bài: [UVa10487 Closest Sums](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=16&page=show_problem&problem=1428)

    ???+ note "Cài đặt"
        ```cpp
        int N = 10, a[] = {1, 1, 2, 4, 5, 5, 8, 8, 9, 9}, x = 6;
        // lower_bound trả về địa chỉ phần tử đầu tiên >= x, i là chỉ số
        int i = lower_bound(a, a + N, x) - a;
        // Trong hai trường hợp sau, a[i] là đáp án:
        // 1. phần tử nhỏ nhất trong a cũng >= x;
        // 2. tồn tại phần tử >= x và a[i] gần x hơn a[i - 1];
        // Ngược lại, a[i - 1] là đáp án
        if (i == 0 || (i < N && a[i] - x < x - a[i - 1]))
          cout << a[i];
        else
          cout << a[i - 1];
        ```
-   Dùng `sort` và `unique` tìm **giá trị nhỏ thứ $k$** trong mảng $a$ (lưu ý: giá trị trùng chỉ tính một lần). Bài: [Luogu P1138 第 k 小整数](https://www.luogu.com.cn/problem/P1138)

    ???+ note "Cài đặt"
        ```cpp
        int N = 10, a[] = {1, 3, 3, 7, 2, 5, 1, 2, 4, 6}, k = 3;
        sort(a, a + N);
        // unique trả về địa chỉ sau phần tử cuối sau khi loại trùng, cnt là độ dài mới
        int cnt = unique(a, a + N) - a;
        cout << a[k - 1];
        ```
