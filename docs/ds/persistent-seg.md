## Chủ Tịch Tree (Persistent Segment Tree)

Chủ Tịch Tree, tên đầy đủ là Persistent Segment Tree (Cây phân đoạn bền vững), bạn có thể tham khảo [thảo luận trên Zhihu](https://www.zhihu.com/question/59195374).

???+ warning "Về Functional Segment Tree"
    **Functional Segment Tree** (Cây phân đoạn hàm) là cây phân đoạn sử dụng tư tưởng lập trình hàm. Trong tư tưởng lập trình hàm, tính toán máy tính được coi là hàm toán học và tránh các trạng thái hoặc biến có thể thay đổi. Không khó để nhận thấy, Functional Segment Tree là [Fully Persistent](persistent.md#fully-persistent) (bền vững hoàn toàn).

## Giới thiệu

Trước tiên hãy xem xét một bài toán: Cho một dãy số nguyên $a$ gồm $n$ phần tử, với một khoảng đóng $[l, r]$ bất kỳ, hãy tìm giá trị nhỏ thứ $k$ trong khoảng đó.

Bạn sẽ giải quyết vấn đề này như thế nào?

Một giải pháp khả thi là: sử dụng Chủ Tịch Tree.
Ý tưởng chính của Chủ Tịch Tree là: lưu lại các phiên bản lịch sử mỗi khi thực hiện thao tác chèn, để thuận tiện cho việc truy vấn giá trị nhỏ thứ $k$ trong khoảng.

Làm thế nào để lưu lại? Một cách đơn giản và "trâu bò" là mỗi lần tạo một cây phân đoạn mới.
Nhưng làm vậy thì bộ nhớ sẽ bị tràn mất?

## Giải thích

Chúng ta hãy phân tích một chút, mỗi lần thao tác sửa đổi, số lượng điểm bị thay đổi là như nhau.
(Ví dụ như hình bên dưới, sửa đổi nút có giá trị bằng 1 trong khoảng $[1,8]$, các điểm màu đỏ là các điểm bị thay đổi)
![](./images/persistent-seg.png)

Chỉ có $O(\log{n})$ nút bị thay đổi, tạo thành một chuỗi, tức là số lượng nút bị thay đổi mỗi lần = chiều cao của cây.
Lưu ý rằng Chủ Tịch Tree không thể sử dụng phương pháp lưu trữ kiểu heap (nghĩa là không thể dùng $x\times 2$, $x\times 2+1$ để biểu diễn con trái và con phải), mà phải sử dụng cấp phát động (dynamic node creation) và lưu lại chỉ số con trái, con phải của mỗi nút.
Vì vậy, chúng ta chỉ cần lưu lại nút gốc tại thời điểm chèn mỗi số, dựa trên việc ghi nhận con trái và con phải, là có thể thực hiện được tính bền vững (persistence).

Chúng ta hãy đơn giản hóa bài toán: mỗi lần tìm giá trị nhỏ thứ $k$ trong khoảng $[1,r]$.
Làm thế nào? Chỉ cần tìm phiên bản nút gốc khi chèn đến $r$, sau đó sử dụng cây phân đoạn giá trị (Value Segment Tree) thông thường để giải quyết.

Điều này chắc mọi người đều hiểu, quay lại bài toán ban đầu - tìm giá trị nhỏ thứ $k$ trong khoảng $[l,r]$.
Ở đây chúng ta liên hệ đến một kiến thức khác: **Tiền tố (Prefix Sum)**.
Công cụ nhỏ bé này sử dụng khéo léo tính chất của phép trừ trên khoảng, thông qua việc tiền xử lý để đạt được việc trả lời mỗi truy vấn trong $O(1)$.

Chúng ta có thể nhận thấy, thông tin thống kê của Chủ Tịch Tree cũng thỏa mãn tính chất này.
Vì vậy... nếu cần lấy thông tin thống kê của khoảng $[l,r]$, chỉ cần lấy thông tin của $[1,r]$ trừ đi thông tin của $[1,l - 1]$.

Đến đây, vấn đề đã được giải quyết!

Về vấn đề không gian, chúng ta hãy phân tích: do chúng ta cấp phát động, nên một cây phân đoạn ban đầu chỉ có $2n-1$ nút.
Sau đó, có $n$ lần sửa đổi, mỗi lần tăng tối đa $\lceil\log_2{n}\rceil+1$ nút. Do đó, trong trường hợp xấu nhất, tổng số nút sau $n$ lần sửa đổi sẽ đạt $2n-1+n(\lceil\log_2{n}\rceil+1)$.
Với bài toán này $n \leq 10^5$, một lần sửa đổi tăng tối đa $\lceil\log_2{10^5}\rceil+1 = 18$ nút, vậy tổng số nút sau $n$ lần sửa đổi là $2\times 10^5-1+18\times 10^5$, bỏ qua $-1$, xấp xỉ $20\times 10^5$.

Cuối cùng là một lời khuyên: đừng keo kiệt không gian (hầu hết các bài toán giới hạn không gian đều khá thoải mái, nên thường không phải lo lắng về việc vượt quá giới hạn không gian)! Hãy mạnh dạn cấp phát $2^5\times 10^5$, gần gấp đôi không gian gốc (tức là `n << 5`).

## Cài đặt

```cpp
#include <algorithm>
#include <cstdio>
#include <cstring>
using namespace std;
constexpr int MAXN = 1e5;  // Phạm vi dữ liệu
int tot, n, m;
int sum[(MAXN << 5) + 10], rt[MAXN + 10], ls[(MAXN << 5) + 10],
    rs[(MAXN << 5) + 10];
int a[MAXN + 10], ind[MAXN + 10], len;

int getid(const int &val) {  // Rời rạc hóa
  return lower_bound(ind + 1, ind + len + 1, val) - ind;
}

int build(int l, int r) {  // Xây dựng cây
  int root = ++tot;
  if (l == r) return root;
  int mid = l + r >> 1;
  ls[root] = build(l, mid);
  rs[root] = build(mid + 1, r);
  return root;  // Trả về nút gốc của cây con này
}

int update(int k, int l, int r, int root) {  // Thao tác chèn
  int dir = ++tot;
  ls[dir] = ls[root], rs[dir] = rs[root], sum[dir] = sum[root] + 1;
  if (l == r) return dir;
  int mid = l + r >> 1;
  if (k <= mid)
    ls[dir] = update(k, l, mid, ls[dir]);
  else
    rs[dir] = update(k, mid + 1, r, rs[dir]);
  return dir;
}

int query(int u, int v, int l, int r, int k) {  // Thao tác truy vấn
  int mid = l + r >> 1,
      x = sum[ls[v]] - sum[ls[u]];  // Lấy số lượng phần tử được lưu trữ trong con trái thông qua phép trừ khoảng
  if (l == r) return l;
  if (k <= x)  // Nếu k nhỏ hơn hoặc bằng x, nghĩa là số nhỏ thứ k nằm trong con trái
    return query(ls[u], ls[v], l, mid, k);
  else  // Ngược lại nghĩa là nằm trong con phải
    return query(rs[u], rs[v], mid + 1, r, k - x);
}

void init() {
  scanf("%d%d", &n, &m);
  for (int i = 1; i <= n; ++i) scanf("%d", a + i);
  memcpy(ind, a, sizeof ind);
  sort(ind + 1, ind + n + 1);
  len = unique(ind + 1, ind + n + 1) - ind - 1;
  rt[0] = build(1, len);
  for (int i = 1; i <= n; ++i) rt[i] = update(getid(a[i]), 1, len, rt[i - 1]);
}

int l, r, k;

void work() {
  while (m--) {
    scanf("%d%d%d", &l, &r, &k);
    printf("%d\n", ind[query(rt[l - 1], rt[r], 1, len, k)]);  // Trả lời truy vấn
  }
}

int main() {
  init();
  work();
  return 0;
}
```

## Mở rộng: DSU Bền vững dựa trên Chủ Tịch Tree

Chủ Tịch Tree là một cách thuận tiện để thực hiện Disjoint Set Union (DSU) bền vững (Persistent DSU). Dưới đây là một ví dụ về việc thực hiện DSU bền vững dựa trên Chủ Tịch Tree.

```cpp
--8<-- "docs/ds/code/persistent-seg/persistent-seg_1.cpp"
```

## Tham khảo

<https://en.wikipedia.org/wiki/Persistent_data_structure>

<https://www.cnblogs.com/zinthos/p/3899565.html>