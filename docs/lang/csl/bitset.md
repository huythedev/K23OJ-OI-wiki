author: i-Yirannn, Xeonacid, ouuan

## Giới thiệu

`std::bitset` là container kích thước cố định lưu `0/1`. Nói строго, nó không thuộc STL.

??? note "bitset và STL"
    > The C++ standard library provides some special container classes, the so-called container adapters (stack, queue, priority queue). In addition, a few classes provide a container-like interface (for example, strings, bitsets, and valarrays). All these classes are covered separately.1 Container adapters and bitsets are covered in Chapter 12.
    >
    > The C++ standard library provides not only the containers for the STL framework but also some containers that fit some special needs and provide simple, almost self-explanatory, interfaces. You can group these containers into either the so-called container adapters, which adapt standard STL containers to fit special needs, or a bitset, which is a containers for bits or Boolean values. There are three standard container adapters: stacks, queues, and priority queues. In priority queues, the elements are sorted automatically according to a sorting criterion. Thus, the "next" element of a priority queue is the element with the "highest" value. A bitset is a bitfield with an arbitrary but fixed number of bits. Note that the C++ standard library also provides a special container with a variable size for Boolean values: vector.
    
    ——Trích từ “The C++ Standard Library 2nd Edition”
    
    Từ đó thấy `bitset` không thuộc STL mà là “Special Container” trong thư viện chuẩn. Thực tế, nó cũng không thỏa yêu cầu của container STL. Nó không phải adapter và cũng không phụ thuộc container STL khác làm nền.

Do bộ nhớ được địa chỉ theo byte, không theo bit; một biến `bool` chỉ biểu diễn `0/1` nhưng chiếm 1 byte.

`bitset` tối ưu để 8 bit trong 1 byte lưu 8 giá trị `0/1`.

Với biến `int` 4 byte, nếu chỉ lưu `0/1`, `bitset` tốn $\frac{1}{32}$ bộ nhớ, và thời gian xử lý cũng khoảng $\frac 1{32}$.

Trong một số trường hợp, `bitset` tối ưu hiệu năng. Việc tối ưu là giảm độ phức tạp hay hằng số tùy góc nhìn. Độ phức tạp thường viết:

1.  $O(n)$, coi như không tối ưu.
2.  $O(\frac n{32})$, không nghiêm ngặt (không nên có hằng số), nhưng thể hiện tối ưu $\frac 1{32}$.
3.  $O(\frac n w)$, với $w=32$, cách viết phổ biến.
4.  $O(\frac n {\log w})$, với $w$ là số bit của một biến nguyên.

Ngoài ra, `vector<bool>` có cách lưu giống `bitset`, khác ở chỗ hỗ trợ cấp phát động. Nhưng `bitset` có nhiều hàm tiện, đôi khi hỗ trợ SIMD giảm hằng số. `vector<bool>` còn có hành vi khác `vector` (ví dụ `&vec[0] + i` không bằng `&vec[i]`). Vì vậy thường không dùng `vector<bool>`.

## Cách dùng

Xem [std::bitset - cppreference.com](https://en.cppreference.com/w/cpp/utility/bitset).

### Header

```cpp
#include <bitset>
```

### Chỉ định kích thước

```cpp
std::bitset<1000> bs;  // bitset 1000 bit
```

### Hàm dựng

-   `bitset()`: mọi bit là `false`.
-   `bitset(unsigned long val)`: đặt theo nhị phân của `val`.
-   `bitset(const string& str)`: đặt theo chuỗi $01$ `str`.

### Toán tử

-   `operator []`: truy cập một bit.

-   `operator ==`/`operator !=`: so sánh hai `bitset`.

-   `operator &`/`operator &=`/`operator |`/`operator |=`/`operator ^`/`operator ^=`/`operator ~`: AND/OR/XOR/NOT.

    Lưu ý: **`bitset` chỉ có thể làm phép bit với `bitset`**; muốn làm với số nguyên phải chuyển sang `bitset`.

-   `operator <<`/`operator >>`/`operator <<=`/`operator >>=`: dịch trái/phải.

`bitset` cũng hỗ trợ I/O kiểu stream, nên có thể dùng `cin/cout`.

### Hàm thành viên

-   `count()`: số bit `true`.
-   `size()`: kích thước `bitset`.
-   `test(pos)`: giống `at()` của `vector`, khác `[]` ở chỗ kiểm tra biên.
-   `any()`: có bit `true` nào không.
-   `none()`: tất cả đều `false`.
-   `all()`: tất cả đều `true`.
-   1.  `set()`: đặt toàn bộ `true`.
    2.  `set(pos, val = true)`: đặt một bit `true`/`false`.
-   1.  `reset()`: đặt toàn bộ `false`.
    2.  `reset(pos)`: đặt một bit `false` (tương đương `set(pos, false)`).
-   1.  `flip()`: đảo tất cả bit ($0\leftrightarrow1$).
    2.  `flip(pos)`: đảo một bit.
-   `to_string()`: trả về chuỗi.
-   `to_ulong()`: trả về `unsigned long` (`long` trên NT/32-bit POSIX như `int`, trên 64-bit POSIX như `long long`).
-   `to_ullong()`: (từ **C++11**) trả về `unsigned long long`.

Ngoài ra, libstdc++ có một số hàm nội bộ[^bitset1]:

-   `_Find_first()`: trả về vị trí `true` đầu tiên, nếu không có trả về kích thước.
-   `_Find_next(pos)`: trả về vị trí `true` đầu tiên sau `pos` (chỉ số > `pos`), nếu không có trả về kích thước.

## Ứng dụng

### [“LibreOJ β Round #2”贪心只能过样例](https://loj.ac/problem/515)

Bài này có thể dùng dp, chuyển trạng thái:

$f(i,j)$ là với $i$ số đầu, tổng bình phương có thể bằng $j$ hay không, thì $f(i,j)=\bigvee\limits_{k=a}^bf(i-1,j-k^2)$.

Nếu làm trực tiếp là $O(n^5)$, (trông có vẻ) không qua được.

Dùng `bitset` để tối ưu, dịch trái và OR là được:

??? note "Bản nộp: [std::bitset](https://loj.ac/submission/395274)"
    ```cpp
    #include <bitset>
    #include <cstdio>
    #include <iostream>
    
    using namespace std;
    
    constexpr int N = 101;
    
    int n, a[N], b[N];
    bitset<N * N * N> f[N];
    
    int main() {
      int i, j;
    
      cin >> n;
    
      for (i = 1; i <= n; ++i) cin >> a[i] >> b[i];
    
      f[0][0] = 1;
    
      for (i = 1; i <= n; ++i) {
        for (j = a[i]; j <= b[i]; ++j) {
          f[i] |= (f[i - 1] << (j * j));
        }
      }
    
      cout << f[n].count();
    
      return 0;
    }
    ```

Vì libstdc++ nén theo `__CHAR_BIT__ * sizeof(unsigned long)`[^bitset2], trên vài nền tảng là $32$. Có thể tự viết `bitset` (chỉ cần hỗ trợ dịch trái rồi OR) nén $64$ bit (`__CHAR_BIT__ * sizeof(unsigned long long)`) để tối ưu thêm:

??? note "Bản nộp: [bitset tự viết](https://loj.ac/submission/395619)"
    ```cpp
    #include <cstdio>
    #include <iostream>
    
    using namespace std;
    
    constexpr int N = 101;
    constexpr int W = 64;
    
    struct Bitset {
      unsigned long long a[N * N * N >> 6];
    
      void shiftor(const Bitset &y, int p, int l, int r) {
        int t = p - p / W * W;
        int tt = (t == 0 ? 0 : W - t);
        int to = (r + p) / W;
        int qaq = (p + W - 1) / W;
    
        for (int i = (l + p) / W; i <= to; ++i) {
          if (i - qaq >= 0) a[i] |= y.a[i - qaq] >> tt;
    
          a[i] |= ((y.a[i - qaq + 1] & ((1ull << tt) - 1)) << t);
        }
      }
    } f[N];
    
    int main() {
      int n, a, b, l = 0, r = 0, ans = 0;
    
      scanf("%d", &n);
    
      f[0].a[0] = 1;
    
      for (int i = 1; i <= n; ++i) {
        scanf("%d%d", &a, &b);
    
        for (int j = a; j <= b; ++j) f[i].shiftor(f[i - 1], j * j, l, r);
    
        l += a * a;
        r += b * b;
      }
    
      for (int i = l / W; i <= r / W; ++i)
        ans += __builtin_popcount(f[n].a[i] & 0xffffffffu) +
               __builtin_popcount(f[n].a[i] >> 32);
    
      printf("%d", ans);
    
      return 0;
    }
    ```

Ngoài ra, vét cạn kèm vài cắt tỉa cũng qua được:

??? note "Bản nộp: [Vét cạn có cắt tỉa](https://loj.ac/submission/395673)"
    ```cpp
    #include <cstdio>
    #include <iostream>
    
    using namespace std;
    
    constexpr int N = 101;
    constexpr int W = 64;
    
    bool f[N * N * N];
    
    int main() {
      int n, i, j, k, a, b, l = 0, r = 0, ans = 0;
    
      scanf("%d", &n);
    
      f[0] = true;
    
      for (i = 1; i <= n; ++i) {
        scanf("%d%d", &a, &b);
        l += a * a;
        r += b * b;
    
        for (j = r; j >= l; --j) {
          f[j] = false;
    
          for (k = a; k <= b; ++k) {
            if (j - k * k < l - a * a) break;
    
            if (f[j - k * k]) {
              f[j] = true;
              break;
            }
          }
        }
      }
    
      for (i = l; i <= r; ++i) ans += f[i];
    
      printf("%d", ans);
    
      return 0;
    }
    ```

### [CF1097F Alex and a TV Show](https://codeforces.com/contest/1097/problem/F)

#### Đề bài

Cho $n$ đa tập, 4 thao tác:

1.  Gán một đa tập bằng một số.
2.  Gán một đa tập bằng tổng của hai đa tập khác.
3.  Gán một đa tập bằng $\gcd$ của mỗi cặp phần tử lấy từ hai đa tập khác: $A=\{\gcd(x,y)|x\in B,y\in C\}$.
4.  Hỏi số lần xuất hiện của một số trong một đa tập, **trong mô-đun 2**.

Số đa tập $10^5$, số thao tác $10^6$, giá trị tối đa $7000$.

#### Cách làm

Thấy “mod 2” có thể nghĩ đến `bitset` để lưu mỗi đa tập.

Khi đó, thao tác 1 là gán trực tiếp, thao tác 2 là XOR (vì mod 2), thao tác 4 là truy vấn. Nhưng thao tác 3 thì sao?

Có thể lưu đa tập các ước của mỗi đa tập. Khi đó thao tác 3 là AND.

Ta tiền xử lý `bitset` các ước của mỗi số trong phạm vi, thao tác 1 giải quyết được, thao tác 2 vẫn là XOR.

Vấn đề là từ đa tập ước $A'$ suy ra số lần xuất hiện của $x$ trong đa tập $A$.

Gọi đa tập gốc là $A$, đa tập ước là $A'$. Cần số lần $x$ xuất hiện trong $A$, dùng [Möbius inversion](../../math/number-theory/mobius.md):

$$
\begin{aligned}&\sum\limits_{i\in A}[\frac i x=1]\\=&\sum\limits_{i\in A}\sum\limits_{d|\frac i x}\mu(d)\\=&\sum\limits_{d\in A',x|d}\mu(\frac d x)\end{aligned}
$$

Vì là mod 2, $-1$ và $1$ như nhau, chỉ cần xem $\frac d x$ có thừa số bình phương không. Do đó, tiền xử lý cho mỗi $x$ một `bitset` gồm các bội của $x$ mà chia cho $x$ không có thừa số bình phương. Khi hỏi, AND rồi `count()`.

Độ phức tạp mỗi truy vấn $O(\frac v w)$ ($v=7000,\,w=32$).

Tiền xử lý có thể $O(v\sqrt v)$ hoặc $O(v^2)$ cho đơn giản; cách $\log$ như dưới có độ phức tạp cấp điều hòa nên là $O(v\log v)$.

??? note "Mã tham khảo"
    ```cpp
    #include <bitset>
    #include <cctype>
    #include <cmath>
    #include <cstdio>
    #include <iostream>
    
    using namespace std;
    
    int read() {
      int out = 0;
      char c;
      while (!isdigit(c = getchar()));
      for (; isdigit(c); c = getchar()) out = out * 10 + c - '0';
      return out;
    }
    
    constexpr int N = 100005;
    constexpr int M = 1000005;
    constexpr int V = 7005;
    
    bitset<V> pre[V], pre2[V], a[N], mu;
    int n, m, tot;
    char ans[M];
    
    int main() {
      int i, j, x, y, z;
    
      n = read();
      m = read();
    
      mu.set();
      for (i = 2; i * i < V; ++i) {
        for (j = 1; i * i * j < V; ++j) {
          mu[i * i * j] = 0;
        }
      }
      for (i = 1; i < V; ++i) {
        for (j = 1; i * j < V; ++j) {
          pre[i * j][i] = 1;
          pre2[i][i * j] = mu[j];
        }
      }
    
      while (m--) {
        switch (read()) {
          case 1:
            x = read();
            y = read();
            a[x] = pre[y];
            break;
          case 2:
            x = read();
            y = read();
            z = read();
            a[x] = a[y] ^ a[z];
            break;
          case 3:
            x = read();
            y = read();
            z = read();
            a[x] = a[y] & a[z];
            break;
          case 4:
            x = read();
            y = read();
            ans[tot++] = ((a[x] & pre2[y]).count() & 1) + '0';
            break;
        }
      }
    
      printf("%s", ans);
    
      return 0;
    }
    ```

### Kết hợp với sàng Eratosthenes

Do `bitset` có khả năng đọc/ghi liên tiếp rất nhanh, nó rất phù hợp để kết hợp với [sàng Eratosthenes](../../math/number-theory/sieve.md#埃拉托斯特尼筛法).

Cách dùng đơn giản: thay mảng bool trong sàng bằng `bitset`.

??? note "Kiểm thử tốc độ"
    Dùng [Quick C++ Benchmarks](https://quick-bench.com), compiler `GCC 13.2`, tham số `-std=c++20 -O2`.
    
    | Thuật toán                            | Tên hàm                      |
    | ----------------------------- | ------------------------ |
    | Sàng Eratosthenes + mảng bool C, không lưu prime      | `Eratosthenes_CArray`    |
    | Sàng Eratosthenes +`vector<bool>`, không lưu prime | `Eratosthenes_vector`    |
    | Sàng Eratosthenes +`bitset`, không lưu prime       | `Eratosthenes_bitset`    |
    | Sàng Eratosthenes + mảng bool C, lưu prime       | `Eratosthenes_CArray_sp` |
    | Sàng Eratosthenes +`vector<bool>`, lưu prime  | `Eratosthenes_vector_sp` |
    | Sàng Eratosthenes +`bitset`, lưu prime        | `Eratosthenes_bitset_sp` |
    | Sàng Euler + mảng bool C                | `Euler_CArray`           |
    | Sàng Euler +`vector<bool>`           | `Euler_vector`           |
    | Sàng Euler +`bitset`                 | `Euler_bitset`           |
    
    -   Khi sàng Eratosthenes **lưu** prime:
    
        -   $N=5 \times 10^7 + 1$ [kết quả](https://quick-bench.com/q/iQL9FhsZ6PVV81HKABsidRw8hB8):
    
            ![](./images/bitset-5e7sp.png)
        -   $N=10^8 + 1$ [kết quả](https://quick-bench.com/q/pwEamEFUW-6nXeXEALRsYPd8FWI):
    
            ![](./images/bitset-1e8sp.png)
    -   Khi sàng Eratosthenes **không lưu** prime:
    
        -   $N=5 \times 10^7 + 1$ [kết quả](https://quick-bench.com/q/rg2mCUxT02a44w9fWvHtZoNTJyU):
    
            ![](./images/bitset-5e7.png)
        -   $N=10^8 + 1$ [kết quả](https://quick-bench.com/q/lusNWxWsR0VXoRBof7uBtqfvJuY):
    
            ![](./images/bitset-1e8.png)
    
    Từ kết quả:
    
    1.  Sàng Eratosthenes $O(n \log \log n)$ khi tối ưu bằng `bitset` hoặc `vector<bool>` còn nhanh hơn sàng Euler $O(n)$;
    2.  Sàng Euler dùng `bitset` hoặc `vector<bool>` thường không cải thiện nhiều;
    3.  `bitset` nhỉnh hơn `vector<bool>`.

??? note "Mã tham khảo"
    Cần cài [google/benchmark](https://github.com/google/benchmark).
    
    ```cpp
    #include <benchmark/benchmark.h>
    #include <bits/stdc++.h>
    using namespace std;
    using u32 = uint32_t;
    using u64 = uint64_t;
    
    #define ERATOSTHENES_STORAGE_PRIME
    #define ENABLE_EULER
    constexpr u32 N = 5e7 + 1;
    
    #ifndef ERATOSTHENES_STORAGE_PRIME
    
    void Eratosthenes_CArray(benchmark::State &state) {
      static bool is_prime[N];
      for (auto _ : state) {
        fill(is_prime, is_prime + N, true);
        is_prime[0] = is_prime[1] = false;
        for (u32 i = 2; (u64)i * i < N; ++i)
          if (is_prime[i])
            for (u32 j = i * i; j < N; j += i) is_prime[j] = false;
        benchmark::DoNotOptimize(0);
      }
    }
    
    BENCHMARK(Eratosthenes_CArray);
    
    void Eratosthenes_vector(benchmark::State &state) {
      static vector<bool> is_prime(N);
      for (auto _ : state) {
        fill(is_prime.begin(), is_prime.end(), true);
        is_prime[0] = is_prime[1] = false;
        for (u32 i = 2; (u64)i * i < N; ++i)
          if (is_prime[i])
            for (u32 j = i * i; j < N; j += i) is_prime[j] = false;
        benchmark::DoNotOptimize(0);
      }
    }
    
    BENCHMARK(Eratosthenes_vector);
    
    void Eratosthenes_bitset(benchmark::State &state) {
      static bitset<N> is_prime;
      for (auto _ : state) {
        is_prime.set();
        is_prime.reset(0);
        is_prime.reset(1);
        for (u32 i = 2; (u64)i * i < N; ++i)
          if (is_prime[i])
            for (u32 j = i * i; j < N; j += i) is_prime.reset(j);
        benchmark::DoNotOptimize(0);
      }
    }
    
    BENCHMARK(Eratosthenes_bitset);
    
    #else
    
    void Eratosthenes_CArray_sp(benchmark::State &state) {
      static bool is_prime[N];
      for (auto _ : state) {
        vector<u32> prime;
        fill(is_prime, is_prime + N, true);
        is_prime[0] = is_prime[1] = false;
        for (u32 i = 2; (u64)i * i < N; ++i)
          if (is_prime[i])
            for (u32 j = i * i; j < N; j += i) is_prime[j] = false;
        for (u32 i = 2; i < N; ++i)
          if (is_prime[i]) prime.push_back(i);
        benchmark::DoNotOptimize(prime);
      }
    }
    
    BENCHMARK(Eratosthenes_CArray_sp);
    
    void Eratosthenes_vector_sp(benchmark::State &state) {
      static vector<bool> is_prime(N);
      for (auto _ : state) {
        vector<u32> prime;
        fill(is_prime.begin(), is_prime.end(), true);
        is_prime[0] = is_prime[1] = false;
        for (u32 i = 2; (u64)i * i < N; ++i)
          if (is_prime[i])
            for (u32 j = i * i; j < N; j += i) is_prime[j] = false;
        for (u32 i = 2; i < N; ++i)
          if (is_prime[i]) prime.push_back(i);
        benchmark::DoNotOptimize(prime);
      }
    }
    
    BENCHMARK(Eratosthenes_vector_sp);
    
    void Eratosthenes_bitset_sp(benchmark::State &state) {
      static bitset<N> is_prime;
      for (auto _ : state) {
        vector<u32> prime;
        is_prime.set();
        is_prime.reset(0);
        is_prime.reset(1);
        for (u32 i = 2; (u64)i * i < N; ++i)
          if (is_prime[i])
            for (u32 j = i * i; j < N; j += i) is_prime.reset(j);
        for (u32 i = 2; i < N; ++i)
          if (is_prime[i]) prime.push_back(i);
        benchmark::DoNotOptimize(prime);
      }
    }
    
    BENCHMARK(Eratosthenes_bitset_sp);
    
    #endif
    
    #ifdef ENABLE_EULER
    
    void Euler_CArray(benchmark::State &state) {
      static bool not_prime[N];
      for (auto _ : state) {
        vector<u32> prime;
        fill(not_prime, not_prime + N, false);
        not_prime[0] = not_prime[1] = true;
        for (u32 i = 2; i < N; ++i) {
          if (!not_prime[i]) prime.push_back(i);
          for (u32 pri_j : prime) {
            if (i * pri_j >= N) break;
            not_prime[i * pri_j] = true;
            if (i % pri_j == 0) break;
          }
        }
        benchmark::DoNotOptimize(prime);
      }
    }
    
    BENCHMARK(Euler_CArray);
    
    void Euler_vector(benchmark::State &state) {
      static vector<bool> not_prime(N);
      for (auto _ : state) {
        vector<u32> prime;
        fill(not_prime.begin(), not_prime.end(), false);
        not_prime[0] = not_prime[1] = true;
        for (u32 i = 2; i < N; ++i) {
          if (!not_prime[i]) prime.push_back(i);
          for (u32 pri_j : prime) {
            if (i * pri_j >= N) break;
            not_prime[i * pri_j] = true;
            if (i % pri_j == 0) break;
          }
        }
        benchmark::DoNotOptimize(prime);
      }
    }
    
    BENCHMARK(Euler_vector);
    
    void Euler_bitset(benchmark::State &state) {
      static bitset<N> not_prime;
      for (auto _ : state) {
        vector<u32> prime;
        not_prime.reset();
        not_prime.set(0);
        not_prime.set(1);
        for (u32 i = 2; i < N; ++i) {
          if (!not_prime[i]) prime.push_back(i);
          for (u32 pri_j : prime) {
            if (i * pri_j >= N) break;
            not_prime.set(i * pri_j);
            if (i % pri_j == 0) break;
          }
        }
        benchmark::DoNotOptimize(prime);
      }
    }
    
    BENCHMARK(Euler_bitset);
    
    #endif
    
    static void Noop(benchmark::State &state) {
      for (auto _ : state) benchmark::DoNotOptimize(0);
    }
    
    BENCHMARK(Noop);
    BENCHMARK_MAIN();
    ```

### Kết hợp với phân khối trên cây

`bitset` kết hợp phân khối trên cây giải được các bài toán tổng hợp thông tin trên nhiều đường đi; xem [Cấu trúc dữ liệu/Phân khối trên cây](../../ds/tree-decompose.md).

### Kết hợp với Mo's algorithm

Xem [Khác/莫队配合 bitset](../../misc/mo-algo-with-bitset.md).

### Tính thứ tự từng chiều cao

Xem [slide FHR](https://github.com/OI-wiki/libs/blob/master/lang/csl/FHR-分块bitset求高维偏序.pdf).

## Tài liệu tham khảo và chú thích

[^bitset1]: [libstdc++: SGI STL extensions](https://gcc.gnu.org/onlinedocs/libstdc++/libstdc++-html-USERS-4.4/a00994.html#g32541eb0d6581b915af48b5a51006dff)

[^bitset2]: [libstdc++: std::bitset<_Nb> Class Template Reference](https://gcc.gnu.org/onlinedocs/libstdc++/libstdc++-html-USERS-4.4/a00219.html)
