## Định nghĩa

Một số bài toán [DP trạng thái nén (State Compression DP)](./state.md) yêu cầu chúng ta ghi lại thông tin về tính liên thông của các trạng thái. Loại bài toán này thường được gọi một cách hình ảnh là DP Cắm (Plug DP) hoặc DP nén trạng thái liên thông. Ví dụ: đếm số đường đi Hamiltonian trên đồ thị lưới, tìm số cách tô màu đen trắng cho bàn cờ sao cho các ô cùng màu tạo thành một khối liên thông, hoặc đếm số cây bao trùm của một đồ thị đặc biệt. Những bài toán này thường yêu cầu chúng ta mã hóa tính liên thông của trạng thái và thảo luận về sự thay đổi tính liên thông trong quá trình chuyển trạng thái.

## Giới thiệu

### Phủ quân Domino và DP đường bao (Contour Line DP)

Trước khi học Plug DP, hãy cùng ôn lại một bài toán kinh điển.

???+ note "Ví dụ [「HDU 1400」Mondriaan’s Dream](https://acm.hdu.edu.cn/showproblem.php?pid=1400)"
    Đề bài: Cho bàn cờ $N \times M$, tìm số cách phủ kín bàn cờ bằng các quân domino kích thước $1 \times 2$ hoặc $2 \times 1$.

Khi $N$ hoặc $M$ không quá lớn, bài toán này có thể giải bằng [DP trạng thái nén](./state.md). Chia giai đoạn theo từng hàng, gọi $dp(i, s)$ là số cách khi đã xét xong $i$ hàng đầu tiên và trạng thái của hàng thứ $i$ là $s$. Trạng thái $s$ ở mỗi vị trí biểu thị ô đó có bị hàng trên phủ xuống hay không.

![domino](./images/domino.svg)

Một cách chia giai đoạn khác là DP theo từng ô, hay còn gọi là DP đường bao. $dp(i, j, s)$ biểu thị đã xét đến hàng $i$ cột $j$, và trạng thái trên đường bao hiện tại là $s$.

Mặc dù trạng thái tăng thêm một chiều $(j)$, nhưng độ phức tạp chuyển trạng thái giảm xuống còn $O(1)$, vì vậy tổng độ phức tạp không đổi. Gọi $f_0$ là trạng thái giai đoạn hiện tại, $f_1$ là trạng thái giai đoạn tiếp theo, $u = f_0(s)$ là giá trị hiện tại, ta có phương trình chuyển trạng thái:

```cpp
if (s >> j & 1) {       // Nếu đã bị phủ từ trên xuống
  f1[s ^ 1 << j] += u;  // Không đặt thêm gì (ô hiện tại đã kín)
} else {                // Nếu chưa bị phủ
  if (j != m - 1 && (!(s >> j + 1 & 1))) f1[s ^ 1 << j + 1] += u;  // Đặt nằm ngang
  f1[s ^ 1 << j] += u;                                             // Đặt thẳng đứng
}
```

??? note "Cài đặt"
    ```cpp
    #include <algorithm>
    #include <iostream>
    using namespace std;
    constexpr int N = 11;
    long long f[2][1 << N], *f0, *f1;
    int n, m;
    
    int main() {
      while (cin >> n >> m && n) {
        f0 = f[0];
        f1 = f[1];
        fill(f1, f1 + (1 << m), 0);
        f1[0] = 1;
        for (int i = 0; i < n; ++i) {
          for (int j = 0; j < m; ++j) {
            swap(f0, f1);
            fill(f1, f1 + (1 << m), 0);
    #define u f0[s]
            for (int s = 0; s < 1 << m; ++s)
              if (u) {
                if (j != m - 1 && (!(s >> j & 3))) f1[s ^ 1 << j + 1] += u;
                f1[s ^ 1 << j] += u;
              }
          }
        }
        cout << f1[0] << endl;
      }
    }
    ```

### Thuật ngữ

Giai đoạn: Thứ tự thực thi quy hoạch động, kết quả giai đoạn sau chỉ phụ thuộc vào giai đoạn trước (tính không hệ quả).

Đường bao (Contour Line): Đường ranh giới giữa các ô đã quyết định và các ô chưa quyết định.

![contour line](./images/contour_line.svg)

Cắm (Plug): Một ô có "phích cắm" tại một hướng nghĩa là tại hướng đó nó có kết nối với ô kề cạnh.

![plug](./images/plug.svg)

## Mô hình đường đi

### Nhiều chu trình (Multiple Circuits)

#### Ví dụ

???+ note "Ví dụ [「HDU 1693」Eat the Trees](https://acm.hdu.edu.cn/showproblem.php?pid=1693)"
    Đề bài: Tìm số cách dùng một hoặc nhiều chu trình rời nhau để phủ kín các ô không có vật cản trên bàn cờ $N \times M$.

Nghiêm túc mà nói, bài toán nhiều chu trình chưa hẳn là Plug DP thực thụ, vì ta chỉ cần ghi nhận sự tồn tại của phích cắm (0 hoặc 1) và thực hiện ghép đôi hoặc tạo mới phích cắm theo cặp là đủ.

Lưu ý: Với bàn cờ độ rộng $m$, đường bao có độ rộng $m+1$ (gồm $m$ phích cắm trên và $1$ phích cắm trái). Khi kết thúc một hàng, phích cắm trái ngoài cùng bên phải là không hợp lệ, ta cần thực hiện thao tác cuộn `roll()` để chuẩn bị cho hàng tiếp theo.

??? note "Mã nguồn ví dụ"
    ```cpp
    --8<-- "docs/dp/code/plug/plug_1.cpp"
    ```

### Một chu trình duy nhất (Single Circuit)

#### Ví dụ

???+ note "Ví dụ [「ASC 16 - Problem F」Pipe Layout](https://codeforces.com/gym/100220)"
    Đề bài: Tìm số cách dùng duy nhất một chu trình để phủ kín bàn cờ $N \times M$.

Trong bài toán này, nếu chỉ ghi nhận phích cắm tồn tại hay không, ta có thể vô tình tạo ra nhiều chu trình rời nhau. Vì vậy, ta cần phân biệt tính liên thông giữa các phích cắm bằng cách mã hóa trạng thái.

#### Mã hóa trạng thái

Hai phương pháp phổ biến là biểu diễn bằng dấu ngoặc (bracket) và biểu diễn tối tiểu (minimal representation). Ở đây giới thiệu biểu diễn tối tiểu: dùng mảng độ dài $m+1$, phích cắm không tồn tại là $0$, các phích cắm liên thông với nhau được đánh cùng một số hiệu.

Ví dụ, hai trạng thái sau là tương đương:
- `0 3 1 0 1 3`
- `0 1 2 0 2 1` (Biểu diễn tối tiểu - thứ tự số hiệu xuất hiện từ trái qua phải là nhỏ nhất).

??? note "Cài đặt mã hóa"
    ```cpp
    int b[M + 1], bb[M + 1];
    
    int encode() {
      int s = 0;
      memset(bb, -1, sizeof(bb));
      int bn = 1;
      bb[0] = 0;
      for (int i = m; i >= 0; --i) {
        if (!~bb[b[i]]) bb[b[i]] = bn++;
        s <<= offset;
        s |= bb[b[i]];
      }
      return s;
    }
    ```

#### Bảng băm thủ công (Handwritten Hash Table)

Vì các trạng thái hợp lệ thường rất thưa thớt, ta nên dùng bảng băm để lưu trữ để tiết kiệm không gian và thời gian.

???+ note "Cài đặt bảng băm"
    ```cpp
    struct hashTable {
      int head[Prime], next[MaxSZ], sz;
      int state[MaxSZ];
      long long key[MaxSZ];
      void clear() { ... }
      void push(int s) { ... }
      void roll() { ... }
    } H[2], *H0, *H1;
    ```

#### Chuyển trạng thái

Quá trình chuyển trạng thái xét ô $(i, j)$ với phích cắm trái $lt$ và phích cắm trên $up$. Có các trường hợp chính:
1. Cả $lt$ và $up$ đều tồn tại: Hợp nhất hai thành phần liên thông. Nếu chúng đã thuộc cùng một thành phần, chỉ cho phép đóng chu trình nếu đây là ô cuối cùng.
2. Chỉ một trong $lt$ hoặc $up$ tồn tại: Kéo dài phích cắm xuống dưới hoặc sang phải.
3. Cả hai đều không tồn tại: Tạo ra một cặp phích cắm mới hướng xuống và hướng sang phải.

??? note "Mã nguồn ví dụ"
    ```cpp
    --8<-- "docs/dp/code/plug/plug_2.cpp"
    ```

### Một đường đi duy nhất (Single Path)

#### Ví dụ

???+ note "Ví dụ [「ZOJ 3213」Beautiful Meadow](https://pintia.cn/problem-sets/91827364500/exam/problems/type/7?page=22&problemSetProblemId=91827367895)"
    Đề bài: Cho lưới $N \times M$ với các ô có trọng số, tìm một đường đi (không nhất thiết là chu trình) sao cho tổng trọng số các ô đi qua là lớn nhất.

Trong bài toán đường đi, trạng thái có thể xuất hiện các phích cắm độc lập (đầu mút đường đi). Ta cần ghi lại số lượng đầu mút đã xuất hiện (không quá 2).

??? note "Mã nguồn ví dụ"
    ```cpp
    --8<-- "docs/dp/code/plug/plug_3.cpp"
    ```

## Mô hình tô màu (Coloring Model)

Bên cạnh mô hình đường đi, còn có mô hình tô màu bàn cờ sao cho các ô cùng màu kề nhau tạo thành các khối liên thông. Thay vì duyệt hướng đường đi, ta duyệt màu cho ô hiện tại.

### Ví dụ [「UVa 10572」Black & White](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1513)

???+ note "Đề bài"
    Tô màu đen/trắng cho các ô chưa tô sao cho mọi vùng đen và vùng trắng đều liên thông, đồng thời không có bất kỳ hình vuông $2 \times 2$ nào cùng một màu.

Mô hình này yêu cầu mã hóa cả màu sắc và tính liên thông trên đường bao.

??? note "Mã nguồn ví dụ"
    ```cpp
    --8<-- "docs/dp/code/plug/plug_4.cpp"
    ```

## Mô hình đồ thị và Thực chiến

Plug DP còn áp dụng cho các bài toán về cây bao trùm đặc biệt hoặc bài toán xây tường ngăn cách.

### Ví dụ [「HDU 4113」Construct the Great Wall](https://acm.hdu.edu.cn/showproblem.php?pid=4113)

???+ note "Đề bài"
    Xây dựng một chu trình kín để ngăn cách tất cả các ô ký hiệu `x` và `o`.

Bài toán này có thể giải bằng cách chạy Plug DP trên các **điểm giao** của lưới. Ta cần duy trì thêm thông tin ô hiện tại nằm trong hay ngoài chu trình (sử dụng quy tắc tia sáng - số lần cắt phích cắm đứng).

## Ghi chú

Plug DP là kỹ thuật có độ khó lập trình cao, thường xuất hiện trong các kỳ thi OI/ACM nâng cao. Tài liệu kinh điển nhất là luận văn năm 2008 của **Chen Danqi** (Trần Đan Kỳ) về "Bài toán quy hoạch động dựa trên nén trạng thái liên thông".

### Các biến thể của Phủ Domino
- Nếu $M=2$, bài toán tương đương dãy Fibonacci.
- Nếu $M \le 10, N \le 10^9$, dùng nhân ma trận để tăng tốc.
- Nếu $N, M \le 100$, dùng thuật toán FKT để tính số bộ ghép hoàn hảo của đồ thị phẳng.