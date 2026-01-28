Tác giả: Dev-jqe, HeRaNO, huaruoji

## Công dụng thường gặp

Trong các kỳ thi thuật toán, đôi khi chúng ta cần duy trì thông tin trên nhiều chiều. Lúc này, chúng ta thường cần sử dụng kỹ thuật cây lồng cây (tree in tree) để lưu trữ thông tin. Khi cần duy trì các thao tác như tìm tiền bối (predecessor), hậu duệ (successor), số lớn thứ $k$, thứ hạng (rank) của một số, hoặc chèn và xóa, chúng ta thường sử dụng cây cân bằng (balanced BST) để đáp ứng nhu cầu, cụ thể là cây phân đoạn lồng cây cân bằng (Segment Tree lồng BST).

## Quá trình

Chúng ta sẽ lấy bài toán **Cây cân bằng bậc hai (二逼平衡树)** làm ví dụ để giải thích nguyên lý thực hiện.

Về việc xây dựng cây lồng cây: đối với cây phân đoạn (Segment Tree) lớp ngoài, chúng ta xây dựng cây một cách bình thường. Với mỗi nút trên cây phân đoạn, chúng ta xây dựng một cây cân bằng chứa các phần tử trong dãy mà nút đó bao phủ. Trong thao tác cụ thể, chúng ta có thể chèn từng phần tử của dãy vào; mỗi khi đi qua một nút của cây phân đoạn, ta thêm phần tử đó vào cây cân bằng của nút đó.

Thao tác 1, tìm thứ hạng của một giá trị trong một khoảng: Chúng ta thao tác bình thường trên cây phân đoạn lớp ngoài. Đối với các cây cân bằng tại các nút thuộc khoảng cần tìm, chúng ta trả về số lượng phần tử nhỏ hơn giá trị đó trong cây cân bằng. Khi hợp nhất kết quả từ các khoảng, chúng ta cộng tổng số lượng các phần tử nhỏ hơn này lại. Cuối cùng, lấy kết quả cộng thêm $1$ chính là thứ hạng của giá trị đó trong khoảng.

Thao tác 2, tìm giá trị có thứ hạng $k$ trong một khoảng: Chúng ta có thể sử dụng chiến lược chặt nhị phân. Vì một phần tử có thể xuất hiện nhiều lần (thứ hạng của nó là một khoảng) và một số giá trị có thể không tồn tại trong dãy gốc, chúng ta áp dụng tư duy tương tự thao tác 1: dùng số lượng phần tử nhỏ hơn giá trị đó làm tham số để chặt nhị phân tìm đáp án.

Thao tác 3, thay thế một số bằng một số khác: Chúng ta chỉ cần xóa số cũ trong tất cả các cây cân bằng chứa nó, sau đó chèn số mới vào. Cây phân đoạn lớp ngoài vẫn thao tác như bình thường.

Thao tác 4, tìm tiền bối của một giá trị trong một khoảng: Chúng ta thao tác bình thường trên cây phân đoạn lớp ngoài. Đối với các cây cân bằng tại các nút thuộc khoảng cần tìm, chúng ta trả về tiền bối của giá trị đó trong cây cân bằng đó. Khi hợp nhất kết quả từ cây phân đoạn, chúng ta lấy giá trị lớn nhất trong số các tiền bối tìm được.

## Tính chất

### Độ phức tạp không gian

Mỗi phần tử được thêm vào $O(\log n)$ cây cân bằng, vì vậy độ phức tạp không gian là $O((n + q)\log{n})$.

### Độ phức tạp thời gian

- Đối với các thao tác 1, 3, 4: Chúng ta thực hiện $O(\log{n})$ thao tác trên cây phân đoạn lớp ngoài, mỗi thao tác thực hiện $O(\log{n})$ trên một cây cân bằng lớp trong. Do đó, độ phức tạp thời gian là $O(\log^2{n})$.
- Đối với thao tác 2: Có thêm một quá trình chặt nhị phân, nên độ phức tạp là $O(\log^3{n})$.

## Bài tập ví dụ kinh điển

[二逼平衡树 (Cây cân bằng bậc hai)](https://loj.ac/problem/106): Lớp ngoài dùng cây phân đoạn, lớp trong dùng cây cân bằng.

## Cài đặt

Mã nguồn phần cây cân bằng vui lòng tham khảo [Splay](./splay.md) hoặc các mục tương đương khác.

Thao tác 1:

```cpp
int vec_rank(int k, int l, int r, int x, int y, int t) {
  if (x <= l && r <= y) {
    return spy[k].chk_rank(t);
  }
  int mid = (l + r) >> 1;
  int res = 0;
  if (x <= mid) res += vec_rank(k << 1, l, mid, x, y, t);
  if (y > mid) res += vec_rank(k << 1 | 1, mid + 1, r, x, y, t);
  if (x <= mid && y > mid) res--; // Điều chỉnh tùy theo cách định nghĩa rank
  return res;
}
```

Thao tác 2:

```cpp
int el = 0, er = 100000001, emid;
while (el != er) {
  emid = (el + er) >> 1;
  if (vec_rank(1, 1, n, tl, tr, emid) - 1 < tk)
    el = emid + 1;
  else
    er = emid;
}
printf("%d\n", el - 1);
```

Thao tác 3:

```cpp
void vec_chg(int k, int l, int r, int loc, int x) {
  int t = spy[k].find(dat[loc]);
  spy[k].dele(t);
  spy[k].insert(x);
  if (l == r) return;
  int mid = (l + r) >> 1;
  if (loc <= mid) vec_chg(k << 1, l, mid, loc, x);
  if (loc > mid) vec_chg(k << 1 | 1, mid + 1, r, loc, x);
}
```

Thao tác 4:

```cpp
int vec_front(int k, int l, int r, int x, int y, int t) {
  if (x <= l && r <= y) return spy[k].chk_front(t);
  int mid = (l + r) >> 1;
  int res = -2147483647; // Giá trị cực tiểu
  if (x <= mid) res = max(res, vec_front(k << 1, l, mid, x, y, t));
  if (y > mid) res = max(res, vec_front(k << 1 | 1, mid + 1, r, x, y, t));
  return res;
}
```

## Thuật toán liên quan

Khi đối mặt với các bài toán yêu cầu duy trì thông tin đa chiều, nếu đề bài không yêu cầu bắt buộc phải giải trực tuyến (online), chúng ta còn có thể cân nhắc sử dụng [Chia để trị CDQ](../misc/cdq-divide.md), hoặc [Chặt nhị phân toàn cục](../misc/parallel-binsearch.md) và các thuật toán chia để trị khác để tránh việc sử dụng các cấu trúc dữ liệu phức tạp, từ đó giảm độ khó khi cài đặt mã nguồn.