author: HeRaNO, konnyakuxzy, littlefrog

![./images/kuaizhuanglianbiao.png](./images/kuaizhuanglianbiao.png "./images/kuaizhuanglianbiao.png")

Danh sách liên kết khối (Block List) trông đại khái như thế này...

Không khó để nhận ra danh sách liên kết khối là một danh sách liên kết, mỗi nút (node) trỏ tới một mảng.
Ta chia mảng có độ dài $n$ ban đầu thành $\sqrt{n}$ nút, mỗi nút tương ứng với một mảng có kích thước $\sqrt{n}$.
Vì vậy, ta định nghĩa cấu trúc như sau, xem mã bên dưới. Trong đó `sqn` biểu thị `sqrt(n)` tức là $\sqrt{n}$, `pb` biểu thị `push_back`, tức là thêm một phần tử vào `node` này.

???+ note "Thực hiện"
    ```cpp
    struct node {
      node* nxt;
      int size;
      char d[(sqn << 1) + 5];
    
      node() { size = 0, nxt = NULL, memset(d, 0, sizeof(d)); }
    
      void pb(char c) { d[size++] = c; }
    };
    ```

Danh sách liên kết khối cần hỗ trợ tối thiểu: tách (split), chèn (insert), tìm kiếm (find).
Tách là gì? Tách là chia một `node` thành hai `node` nhỏ hơn, để đảm bảo kích thước của mỗi `node` gần bằng $\sqrt{n}$ (nếu không có thể thoái hóa thành mảng thông thường). Khi kích thước của một `node` vượt quá $2\times \sqrt{n}$ thì thực hiện thao tác tách.

Thao tác tách được thực hiện thế nào? Đầu tiên tạo một nút mới, sau đó `copy` $\sqrt{n}$ giá trị cuối cùng của nút bị tách sang nút mới, sau đó xóa $\sqrt{n}$ giá trị cuối cùng của nút bị tách (`size--`), cuối cùng chèn nút mới vào sau nút bị tách là xong.

Độ phức tạp của tất cả các thao tác của danh sách liên kết khối đều là $\sqrt{n}$.

Còn một điều cần nói nữa.
Khi các phần tử được chèn (hoặc xóa), $n$ sẽ thay đổi, $\sqrt{n}$ cũng thay đổi. Như vậy kích thước khối sẽ thay đổi, chẳng lẽ ta phải duy trì kích thước khối mỗi lần sao?

Thực ra không cần, ta đặt $\sqrt{n}$ là một giá trị cố định. Ví dụ phạm vi đề bài cho là $10^6$, vậy $\sqrt{n}$ có thể đặt là một hằng số có kích thước $10^3$, không cần thay đổi nó.

```cpp
list<vector<char>> orz_list;
```

## `rope` trong libstdc++

### Nhập

`rope` trong libstdc++ cũng đóng vai trò như danh sách liên kết khối, nó sử dụng cây cân bằng có tính bền vững (persistent balanced tree) để thực hiện, có thể hoàn thành các thao tác truy cập ngẫu nhiên và chèn, xóa phần tử.

Vì `rope` không thực sự được triển khai bằng danh sách liên kết khối, nên độ phức tạp thời gian của nó không tương đương với danh sách liên kết khối, mà tương đương với độ phức tạp của cây cân bằng có tính bền vững (tức là $O(\log n)$).

Có thể sử dụng cách sau để nhập:

```cpp
#include <ext/rope>
using namespace __gnu_cxx;
```

???+ warning "Về hàm thư viện bắt đầu bằng hai dấu gạch dưới"
    Trong OI, việc có thể sử dụng hàm thư viện bắt đầu bằng hai dấu gạch dưới đã từng không chắc chắn, trong [Thông báo bổ sung về giới hạn sử dụng ngôn ngữ lập trình trong các hoạt động dòng NOI] do CCF phát hành năm 2021, có đề cập "Cho phép sử dụng các hàm hoặc macro thư viện bắt đầu bằng dấu gạch dưới, ngoại trừ các hàm và macro thư viện bị cấm rõ ràng". Do đó, hiện tại `rope` có thể được sử dụng bình thường trong OI.

### Thao tác cơ bản

| Thao tác | Tác dụng |
| :---: | :---: |
| `rope<int> a` | Khởi tạo `rope` (tương tự các container như `vector`) |
| `a.push_back(x)` | Thêm phần tử `x` vào cuối `a` |
| `a.insert(pos, x)` | Thêm phần tử `x` vào vị trí thứ `pos` của `a` |
| `a.erase(pos, x)` | Xóa `x` phần tử tại vị trí `pos` của `a` |
| `a.at(x)` hoặc `a[x]` | Truy cập phần tử thứ `x` của `a` |
| `a.length()` hoặc `a.size()` | Lấy kích thước của `a` |

## Ví dụ đề bài

[POJ2887 Big String](http://poj.org/problem?id=2887)

Giải thích:
Đây là một bài tập mẫu rất đơn giản. Mã nguồn như sau:

```cpp
--8<-- "docs/ds/code/block-list/block-list_1.cpp"
```