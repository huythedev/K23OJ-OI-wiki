author: Estrella-Explore, H-J-Granger, orzAtalod, ksyx, Ir1d, Chrogeek, Enter-tainer, yiyangit, shuzhouliu, broken-paint, CarvingAn

Trang này liệt kê các lỗi nhiều người thường gặp trong thi.

## Lỗi do khác môi trường

-   Dùng `%I64d` với `scanf/printf` có thể gây lỗi định dạng trên Linux.

## Lỗi gây CE

Các lỗi từ vựng/cú pháp/ngữ nghĩa, dễ sửa.

Ví dụ:

-   Viết `int main()` thành `int mian()`.

-   Quên dấu `;` sau `struct` hoặc `class`.

-   Mảng quá lớn, dùng hàm bị cấm (ví dụ đa luồng), hoặc khai báo hàm mà không định nghĩa gây lỗi link.

-   Kiểu tham số không khớp.

    -   Ví dụ: dùng `max` trong `<algorithm>` với một `int` và một `long long`.

        ```cpp
        // query là hàm tự định nghĩa trả về long long
        printf("%lld\n", max(0, query(1, 1, n, l, r));

        // lỗi    không có overload "std::max" phù hợp danh sách tham số
        ```

-   Dùng `goto` và `switch-case` nhảy qua khởi tạo biến cục bộ.

## Lỗi không CE nhưng gây Warning

Biên dịch qua nhưng dễ sai kết quả. Các lỗi này bị cảnh báo khi dùng `-W{warningtype}`.

-   Nhầm `=` và `==`.

    -   Ví dụ:

        ```cpp
        std::srand(std::time(nullptr));
        int n = std::rand();
        if (n = 1)
          printf("Yes");
        else
          printf("No");

        // Dù n là gì cũng in Yes
        // Cảnh báo    toán tử không đúng: gán hằng trong ngữ cảnh Boolean. Nên dùng "=="
        ```

    -   Nếu cố tình dùng `=` ở nơi đáng ra là `==` (vd `while (foo = bar)`), tránh Warning bằng **ngoặc đôi**: `while ((foo = bar))`.

-   Lỗi do ưu tiên toán tử.

    -   Ví dụ:

        ```cpp
        // Sai
        // std::cout << (1 << 1 + 1);
        // Đúng
        std::cout << ((1 << 1) + 1);

        // Cảnh báo    "<<" : kiểm tra ưu tiên toán tử; dùng ngoặc để làm rõ
        ```

-   Dùng `static` sai.

-   Dùng `scanf` quên `&`.

-   `scanf/printf` không khớp kiểu và định dạng.

-   Trộn bitwise và so sánh `==` mà không ngoặc.
    -   Ví dụ: `(x >> j) & 3 == 2`

-   Tràn `int` literal.

    -   Ví dụ: `long long x = 0x7f7f7f7f7f7f7f7f`，`1<<62`.

-   Biến cục bộ chưa khởi tạo.

    ???+ note "Biến chưa khởi tạo sẽ thế nào"
        原文：<https://loj.ac/d/3679> by @hly1204
        
        Ví dụ khai báo `int a;` nhưng không khởi tạo, nhiều khi nghĩ `a` là “ngẫu nhiên”, hoặc nghĩ nó cố định; thực tế không phải.
        
        Thử code:
        
        <https://wandbox.org/permlink/T2uiVe4n9Hg4EyWT>
        
        Code:
        
        ```cpp
        #include <iostream>
        
        int main() {
          int a;
          std::cout << std::boolalpha << (a < 0 || a == 0 || a > 0);
          return 0;
        }
        ```
        
        Trên một số compiler và môi trường khi bật tối ưu, output là false.
        
        Có thể xem <https://www.ralfj.de/blog/2019/07/14/uninit.html>, dù dùng Rust nhưng bản chất一样.

-   Biến cục bộ trùng tên biến toàn cục làm ghi đè không mong muốn. (Bật `-Wshadow` để kiểm tra.)

-   Lỗi output do overload toán tử.
    -   Ví dụ:

        ```cpp
        // Ý định: << đầu là toán tử xuất; << sau là dịch bit (1 << 1)
        // Nhưng quên ngoặc khiến compiler hiểu << sau cũng là xuất
        // Sai: std::cout << 1 << 1; Đúng:
        std::cout << (1 << 1);
        ```

## Lỗi không CE và không Warning

Compiler không phát hiện, phải tự查.

### Lỗi gây WA

-   Xử lý xong test trước mà không xóa mảng.

-   Fast input không xử lý số âm.

-   Kiểu dữ liệu quá nhỏ gây tràn.
    -   Như câu “三年 OI 一场空，不开 long long 见祖宗”: không dùng `long long` đúng chỗ.

-   Lưu đồ thị đánh số từ 0 nhưng input từ 1, quên -1.

-   Dấu >/< sai hoặc ngược.

-   Sau `ios::sync_with_stdio(false);` mà trộn `scanf/printf` với `cin/cout` làm IO rối.

    -   Ví dụ:

        ```cpp
        // Ví dụ này minh họa hậu quả khi tắt đồng bộ stdio rồi trộn IO
        // Khuyến nghị chạy từng bước để quan sát
        #include <cstdio>
        #include <iostream>

        int main() {
          // Tắt đồng bộ, cin/cout dùng buffer riêng, không đồng bộ với scanf/printf
          std::ios::sync_with_stdio(false);
          // cout dùng '\n' thì buffer, không in ngay
          std::cout << "a\n";
          // printf với '\n' sẽ flush buffer của printf, gây lệch thứ tự
          printf("b\n");
          std::cout << "c\n";
          // Kết thúc chương trình, cout mới flush
          return 0;
        }
        ```

-   Lỗi do macro展开 mà không ngoặc.

    -   Ví dụ: macro trả về $2+2\times 2+2 = 8$ thay vì $4^2 = 16$.

        ```cpp
        #define square(x) x* x
        printf("%d", square(2 + 2));
        ```

-   Hash không dùng `unsigned` gây lỗi.
    -   Dịch phải số âm sẽ điền 1 ở bit cao; xem [位操作符](../lang/op.md#位操作符).

-   Quên xóa/ghi chú output debug.

-   Thừa dấu `;`.

    -   Ví dụ:

        ```cpp
        /* clang-format off */
        while (1);
            printf("OI Wiki!\n");
        ```

-   Sai giá trị sentinel. Ví dụ node `0` của cây cân bằng.

-   Dùng danh sách khởi tạo `:` trong constructor nhưng thứ tự khai báo biến không phù hợp.

    -   Thứ tự khởi tạo theo thứ tự khai báo trong class, không theo thứ tự trong initializer list. Xem [构造函数与成员初始化器列表](https://zh.cppreference.com/w/cpp/language/constructor).
    -   Ví dụ:

        ```cpp
        #include <iostream>

        class Foo {
         public:
          int a, b;

          // a sẽ khởi tạo trước b, nên giá trị không xác định
          Foo(int x) : b(x), a(b + 1) {}
        };

        int main() {
          Foo bar(1, 2);
          std::cout << bar.a << ' ' << bar.b;
        }

        // Kết quả có thể: -858993459 1
        ```

-   DSU hợp nhất mà không hợp nhất tổ tiên.

    -   Ví dụ:

        ```cpp
        f[a] = b;              // Sai
        f[find(a)] = find(b);  // Đúng
        ```

-   `freopen` dùng mode `a` để append
    -   Môi trường CCF không xóa file output; dùng `a` khiến output của thí sinh trước bị đọc gây WA

#### Khác newline

???+ warning "Warning"
    Trong thi chính thức, môi trường thí sinh và môi trường chấm sẽ cố一致.
    
    Phần này chỉ适用于 thi mô phỏng; khuyến nghị ra đề đảm bảo dữ liệu符合 [格式 dữ liệu](problemsetting.md#数据的格式).

Các hệ điều hành dùng ký hiệu newline khác nhau:

-   LF (`\n`)：Unix/Unix-like
-   CR+LF (`\r\n`)：Windows
-   CR (`\r`)：Mac OS 9 trở về trước

C/C++ dùng `\n` để xuống dòng, nên có thể chỉ đọc một ký tự newline và导致 đọc thiếu input.

Cách xử lý:

-   Gọi `getchar()` nhiều lần đến khi gặp ký tự mong muốn.
-   Dùng `cin` (**có thể tăng hằng**).
-   Dùng `scanf("%s",str)` đọc chuỗi rồi lấy `str[0]`.
-   Dùng `scanf(" %c",&c)` để bỏ qua whitespace.

### Lỗi gây kết quả không xác định

Hành vi không xác định có thể导致 WA/RE; compiler giả định chương trình không có UB, nên bật/tắt O2 có thể khác nhau.

-   Chia 0 (tính nghịch đảo 0)

    ???+ warning "Ví dụ"
        ```cpp
        cout << x / 0 << endl;
        ```

-   Mảng (chỉ số) vượt giới hạn

    Ví dụ:

    -   Sai giá trị khởi tạo vòng lặp làm访问 -1.
    -   Đồ thị vô hướng không nhân đôi cạnh.
    -   Segment tree không mở 4 lần.
    -   Nhìn sai范围, thiếu một số 0.
    -   Ước lượng sai space complexity.
    -   Viết segment tree mà `pushup`/`pushdown` ở lá.

        Cách đúng: tránh越界, kiểm tra để chỉ số `x` nằm trong phạm vi hợp lệ.

-   Hàm có trả về (trừ main) chạy đến cuối không return

    Dù có nhánh return, nhánh khác không return cũng là UB.

    Có thể thêm `-Wall` để kiểm tra cảnh báo.

-   Sửa string literal

    ???+ warning "Ví dụ"
        ```cpp
        char *p = "OI-wiki";
        p[0] = 'o';
        p[1] = 'i';
        ```

    Sửa literal gây **UB**; nên dùng `std::string` hoặc `char[]`.

-   Giải phóng nhiều lần/giải tham chiếu非法

    Ví dụ:

    -   Deref con trỏ chưa khởi tạo.
    -   Con trỏ trỏ tới vùng đã被释放.

        Dùng `erase/delete/free` phải tránh nhiều lần trên cùng đối tượng.

-   Giải phóng một phần bộ nhớ do `new[]` cấp phát

    Ví dụ:

    ```cpp
    object *pool = new object[POOL_SIZE];

    object *pointer = pool + 10;

    // Lỗi!
    delete pointer;
    ```

    Thường gặp khi dùng memory pool.

-   Deref null/野指针

    Null: kiểm tra `p == nullptr` hoặc `!p`.  
    野指针: có thể set `nullptr` sau khi free.

-   Tràn số có dấu

    Ví dụ `x+1 > x` thường true, nhưng khi `x=INT_MAX` sẽ false, gọi là signed integer overflow.

    Dùng kiểu lớn hơn (`long long`, `__int128`) hoặc kiểm tra tràn. Nếu đảm bảo không âm, có thể dùng unsigned.

    Tràn có dấu ảnh hưởng tối ưu. Ví dụ:

    ```cpp
    int foo(int x) {
      if (x > x + 1) return 1;
      return 0;
    }
    ```

    Có thể被优化为:

    ```cpp
    int foo(int x) { return 0; }
    ```

    Vì compiler giả định không tràn.

-   Dùng biến chưa khởi tạo

    ???+ warning "Ví dụ"
        ```cpp
        int foo(int a) {
          int t; /* chưa khởi tạo */
          if (/* dùng */ t > 3) return a;
          return 0;
        }
        ```

### Lỗi gây RE

-   Quên xóa file thao tác (trên một số OJ).

-   Lỗi hàm so sánh `std::sort`: yêu cầu strict weak ordering.  
    Nếu không, sort có thể RE.

    Ví dụ sai (Mo’s):

    ```cpp
    bool operator<(const int a, const int b) {
      if (block[a.l] == block[b.l])
        return (block[a.l] & 1) ^ (a.r < b.r);
      else
        return block[a.l] < block[b.l];
    }
    ```

    Sửa đúng:

    ```cpp
    bool operator<(const int a, const int b) {
      if (block[a.l] == block[b.l])
        // Sai: không thỏa strict weak ordering
        // return (block[a.l] & 1) ^ (a.r < b.r);
        // Đúng
        return (block[a.l] & 1) ? (a.r < b.r) : (a.r > b.r);
      else
        return block[a.l] < block[b.l];
    }
    ```

-   Windows: stack nhỏ gây overflow, OS gửi SIGSEGV, chương trình trả 3221225725 (0xC00000FD).  
    Với gcc có thể thêm `-Wl,--stack=SIZE`, `SIZE` là byte.

    Linux: stack overflow làm乱写 `head_info`, thường crash ngay với `segmentation fault`.  
    Có thể dùng `ulimit -s SIZE` (KB).  
    **Lưu ý: set quá lớn có thể làm crash hệ thống khi đệ quy vô hạn.**

### Lỗi gây TLE

-   Divide & conquer không kiểm biên gây đệ quy vô hạn.

-   Vòng lặp vô hạn.

    -   Trùng tên biến vòng lặp.
    -   Hướng lặp ngược.

-   BFS không đánh dấu visited.

-   Dùng macro cho min/max

    Lỗi này tăng thời gian đáng kể; thường gặp khi mới viết segment tree.

    Sai thường见:

    ```cpp
    #define Min(x, y) ((x) < (y) ? (x) : (y))
    #define Max(x, y) ((x) > (y) ? (x) : (y))
    ```

    Nếu dùng `a = Max(func1(), func2())` với hàm tốn thời gian, macro sẽ gọi 3 lần, chậm hơn. Nếu `func1()` trả về khác nhau mỗi lần, còn sai. Ví dụ `func1()` là `return ++a;`.

    Ví dụ: đoạn code bị卡到 mỗi query $\Theta(n)$ gây TLE:

    ```cpp
    #define max(x, y) ((x) > (y) ? (x) : (y))

    int query(int t, int l, int r, int ql, int qr) {
      if (ql <= l && qr >= r) {
        ++ti[t];  // Ghi số lần访问 node để debug
        return vi[t];
      }

      int mid = (l + r) >> 1;
      if (mid >= qr) return query(lt(t), l, mid, ql, qr);
      if (mid < ql) return query(rt(t), mid + 1, r, ql, qr);
      return max(query(lt(t), l, mid, ql, qr), query(rt(t), mid + 1, r, ql, qr));
    }
    ```

-   Dùng `+` để nối `std::string`

    Sẽ tạo string tạm rồi gán lại, không tối ưu, dữ liệu lớn sẽ chậm.

    Cách sai:

    ```cpp
    std::string a;
    char b = 'c';
    a = a + b;
    ```

    Thực tế tạo string tạm, copy, rồi gán lại.

    Cách đúng:

    ```cpp
    std::string a;
    char b = 'c';
    a += b;
    ```

    [Benchmark](https://quick-bench.com/q/JNDGl7HgOszNG-bo7AgVc42owv4).

-   Quên xóa file thao tác (trên một số OJ).

-   Trong vòng `for/while` lặp lại hàm độ phức tạp > $O(1)$; có thể改变 độ phức tạp.

-   Lỗi công thức mid hoặc điều kiện dừng khi nhị phân.

### Lỗi gây MLE

-   Mảng quá lớn.

    ??? note "Chi tiết chỉ số dùng bộ nhớ trên Linux"
        > Bản TL;DR: nếu trong CCF bạn khai báo một mảng tĩnh toàn cục rất lớn, cần hết sức慎重. Vì bộ nhớ của mảng sẽ被全部计入 (khác với nhiều OJ chỉ tính phần dùng thực), trong một số trường hợp có thể khiến cả bài MLE.
        
        -   Về RSS và VSZ[^ref1][^ref2]
        
            1.  VSZ (Virtual Memory Size) — kích thước bộ nhớ ảo[^ref3]
        
                VSZ là **bộ nhớ ảo** mà tiến trình có thể访问, thường tính KB.
        
                Bộ nhớ ảo là概念逻辑, thường lớn hơn bộ nhớ thực.
        
                Trên Linux dùng `top`, cột `VIRT` là bộ nhớ ảo.
        
                VSZ gồm vùng đã phân配 dù chưa dùng; nói đơn giản là cấp bao nhiêu thì VSZ bấy nhiêu.
        
                Lưu ý: nhiều OJ chỉ tính bộ nhớ thực, nhưng **CCF tính bộ nhớ ảo**. Nghĩa là mảng tĩnh toàn cục lớn sẽ占 bộ nhớ lớn dù chỉ dùng một phần.
            2.  RSS (Resident Set Size) — bộ nhớ thực[^ref4]
        
                RSS là **bộ nhớ vật lý** thực sự占, là các trang trong RAM, thường KB.
        
                Cột `RES` trong `top` là RSS.
        
                RSS chỉ包含 phần thực sự加载 vào RAM.
        -   Phân tích hành vi占用
        
            Giả sử mảng:
        
            ```cpp
            const int SIZE = 1e8;
            int arr[SIZE];  // Dung lượng: 4 byte * 1e8 = 400 MB
            ```
        
            Đây là mảng tĩnh toàn cục, nằm ở BSS nếu không init (hoặc DATA nếu init).
        
            -   Không dùng mảng (giả định compiler không tối ưu bỏ)
        
                -   Bộ nhớ thực: demand paging chưa加载, RSS tăng rất ít hoặc không tăng.
                -   Bộ nhớ ảo: VSZ tăng 400MB vì đã分配地址空间.
            -   Dùng một phần
        
                Ví dụ:
        
                ```cpp
                arr[0] = 1;
                arr[999999] = 2;
                ```
        
                -   VSZ không đổi (400MB).
                -   RSS tăng theo số trang被访问. Nếu page 4KB, mỗi page ~1024 int. Hai truy cập có thể加载 2 page ≈ 8KB.
            -   Dùng phần lớn
        
                Ví dụ dùng 50,000,000 phần tử:
        
                ```cpp
                for (int i = 0; i < 50000000; ++i) {
                  arr[i] = i;
                }
                ```
        
                -   VSZ vẫn 400MB.
                -   RSS: số page $\left\lceil \dfrac{50,000,000}{1024} \right\rceil = 48,828$ trang.  
                    4KB/page ⇒ khoảng $48,828 \times 4 \text{KB} \approx 190 \text{MB}$.
        
                Tóm lại: phần dùng tăng thì RSS tiến gần VSZ (giả sử không回收).
-   STL chứa quá nhiều phần tử.

    -   Thường do vòng lặp thêm phần tử bị vô hạn.
    -   Hoặc bị卡.

### Lỗi làm hằng số quá lớn

-   Định nghĩa mod không phải hằng.

    -   Ví dụ:

        ```cpp
        // int mod = 998244353;      // Sai
        const int mod = 998244353;  // Đúng, giúp compiler tối ưu
        ```

-   Dùng đệ quy không cần thiết (không kể tail recursion).

-   Chuyển đệ quy sang lặp nhưng thêm quá nhiều thao tác.

### Lỗi chỉ ảnh hưởng khi chạy local

-   Lỗi thao tác file:

    -   Đối拍 mà không `fclose(fp)` rồi lại `fp = fopen()`, gây大量 file pointer rác.
    -   `freopen()` thiếu đuôi `.in`/`.out`.

-   Dùng heap rồi quên `delete/free`.

## Tài liệu tham khảo và chú thích

[^ref1]: [What is RSS and VSZ in Linux memory management - Stack Overflow](https://stackoverflow.com/questions/7880784/what-is-rss-and-vsz-in-linux-memory-management)

[^ref2]: [Need explanation on Resident Set Size/Virtual Size - Stack Overflow](https://unix.stackexchange.com/questions/35129/need-explanation-on-resident-set-size-virtual-size)

[^ref3]: [Bộ nhớ ảo](https://zh.wikipedia.org/wiki/%E8%99%9A%E6%8B%9F%E5%86%85%E5%AD%98)

[^ref4]: [Resident Set Size](https://zh.wikipedia.org/wiki/%E5%B8%B8%E9%A9%BB%E9%9B%86%E5%A4%A7%E5%B0%8F)
