author: H-J-Granger, Ir1d, ChungZH, Marcythm, StudyingFather, billchenchina, Suyun514, Psycho7, greyqz, Xeonacid, partychicken

Trang này liệt kê một số mẹo nhỏ trong thi.

## Tận dụng tính cục bộ

Tính cục bộ nghĩa là chương trình có xu hướng truy cập dữ liệu gần dữ liệu vừa truy cập, hoặc chính dữ liệu vừa truy cập. Có cục bộ thời gian và cục bộ không gian.

Xem [Loop Unroll](../lang/optimizations.md#循环展开-loop-unroll)、[Code Layout Optimizations](../lang/optimizations.md#代码布局优化-code-layout-optimizations).

## Macro vòng lặp

Mã sau có thể rút gọn bằng macro:

```cpp
for (int i = 0; i < N; i++) {
  // Nội dung vòng lặp
}

// Rút gọn bằng macro
#define f(x, y, z) for (int x = (y), __ = (z); x < __; ++x)

// Khi dùng: f(i, 0, N)
// a là container STL
f(i, 0, a.size()) { ... }
```

Macro hữu ích khác:

```cpp
#define _rep(i, a, b) for (int i = (a); i <= (b); ++i)
```

## Dùng namespace hợp lý

Dùng namespace giúp code dễ đọc, dễ debug.

??? note "Ví dụ: NOI 2018 屠龙勇士"
    ```cpp
    // NOI 2018 屠龙勇士, code 40 điểm phần分
    #include <algorithm>
    #include <cmath>
    #include <cstring>
    #include <iostream>
    using namespace std;
    long long n, m, a[100005], p[100005], aw[100005], atk[100005];
    
    namespace one_game {
    // Trong namespace cũng có thể khai báo biến
    void solve() {
      for (int y = 0;; y++)
        if ((a[1] + p[1] * y) % atk[1] == 0) {
          cout << (a[1] + p[1] * y) / atk[1] << endl;
          return;
        }
    }
    }  // namespace one_game
    
    namespace p_1 {
    void solve() {
      if (atk[1] == 1) {  // solve 1-2
        sort(a + 1, a + n + 1);
        cout << a[n] << endl;
        return;
      } else if (m == 1) {  // solve 3-4
        long long k = atk[1], kt = ceil(a[1] * 1.0 / k);
        for (int i = 2; i <= n; i++)
          k = aw[i - 1], kt = max(kt, (long long)ceil(a[i] * 1.0 / k));
        cout << k << endl;
      }
    }
    }  // namespace p_1
    
    int main() {
      int T;
      cin >> T;
      while (T--) {
        memset(a, 0, sizeof(a));
        memset(p, 0, sizeof(p));
        memset(aw, 0, sizeof(aw));
        memset(atk, 0, sizeof(atk));
        cin >> n >> m;
        for (int i = 1; i <= n; i++) cin >> a[i];
        for (int i = 1; i <= n; i++) cin >> p[i];
        for (int i = 1; i <= n; i++) cin >> aw[i];
        for (int i = 1; i <= m; i++) cin >> atk[i];
        if (n == 1 && m == 1)
          one_game::solve();  // solve 8-13
        else if (p[1] == 1)
          p_1::solve();  // solve 1-4 or 14-15
        else
          cout << -1 << endl;
      }
      return 0;
    }
    ```

## Dùng macro để debug

Khi test local thường thêm debug prints; nộp OJ phải xóa, tốn thời gian. Có thể dùng macro:

```cpp
#define DEBUG
#ifdef DEBUG
// chạy khi DEBUG được định nghĩa
#endif
// hoặc
#ifndef DEBUG
// chạy khi DEBUG chưa được định nghĩa
#endif
```

`#ifdef` kiểm tra có định nghĩa hay không; `#ifndef` thì ngược lại.

Chỉ cần để code debug trong `#ifdef DEBUG`, code nộp trong `#ifndef DEBUG`; khi nộp chỉ cần comment `#define DEBUG`. Hoặc dùng `-DDEBUG` khi compile. Nhiều OJ bật `-DONLINE_JUDGE`; tận dụng để tiết kiệm thời gian.

## Đối拍

Đối拍 là phương pháp kiểm tra bằng cách so sánh output của hai chương trình để判断 đúng sai.

Cần chạy nhiều lần, nên dùng batch để tự động.

Cụ thể cần [generator](../tools/testlib/generator.md) và hai chương trình cần so sánh.

Mỗi lần chạy generator ghi input, rồi redirect cho hai chương trình, ghi output vào file, cuối cùng dùng `fc` (Windows) hoặc `diff` (Linux) để so sánh. Nếu sai, dùng luôn input đó để debug.

Khung chương trình đối拍:

```cpp
#include <cstdio>
#include <cstdlib>

int main() {
  // Cho Windows
  // Đối拍 không mở file IO
  // Có thể viết thành batch
  while (true) {
    system("gen > test.in");  // Generator ghi dữ liệu
    system("test1.exe < test.in > a.out");  // Output chương trình 1
    system("test2.exe < test.in > b.out");  // Output chương trình 2
    if (system("fc a.out b.out")) {
      // Dòng này so sánh output
      // fc trả 0 là giống, khác là có khác biệt
      system("pause");  // Dễ xem khác biệt
      return 0;
      // Dữ liệu nằm trong test.in, có thể dùng debug
    }
  }
}
```

## Bộ nhớ池

Khi cấp phát động, dùng `new`/`malloc` thường xuyên sẽ tốn thời gian và tạo碎片, có thể làm TLE/MLE.

Giải pháp: “memory pool” — cấp phát sẵn một khối, khi cần thì lấy trong đó.

Trong OI, thường có thể ước lượng bộ nhớ tối đa và cấp phát một lần.

Ví dụ:

```cpp
// Cấp phát mảng int 32-bit động:
int* newarr(int sz) {
  static int pool[MAXN], *allocp = pool;
  return allocp += sz, allocp - sz;
}

// Cây đoạn mở节点 động:
Node* newnode() {
  static Node pool[MAXN << 1], *allocp = pool - 1;
  return ++allocp;
}
```

## Tài liệu tham khảo

[洛谷日报 #86](https://studyingfather.blog.luogu.org/some-coding-tips-for-oiers)

《算法竞赛入门经典 习题与解答》
