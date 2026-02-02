Đôi khi, ta cần làm một việc nhiều lần; để tránh viết lặp mã, ta dùng vòng lặp.

Đôi khi số lần lặp không phải là một hằng số, ta không thể sao chép mã nhiều lần, bắt buộc phải dùng vòng lặp.

## Câu lệnh for

Cấu trúc của câu lệnh for:

```cpp
for (khởi tạo; điều kiện; cập nhật) {
  thân vòng lặp;
}
```

Thứ tự thực thi:

![](images/for-loop.svg)

Ví dụ: đọc vào n số:

```cpp
for (int i = 1; i <= n; ++i) {
  cin >> a[i];
}
```

Ba phần của for đều có thể bỏ trống. Nếu bỏ điều kiện, điều kiện được hiểu là luôn đúng.

## Câu lệnh while

Cấu trúc của câu lệnh while:

```cpp
while (điều kiện) {
  thân vòng lặp;
}
```

Thứ tự thực thi:

![](images/while-loop.svg)

Ví dụ: kiểm chứng giả thuyết 3x+1:

```cpp
while (x > 1) {
  if (x % 2 == 1) {
    x = 3 * x + 1;
  } else {
    x = x / 2;
  }
}
```

## Câu lệnh do...while

Cấu trúc của câu lệnh do...while:

```cpp
do {
  thân vòng lặp;
} while (điều kiện);
```

Thứ tự thực thi:

![](images/do-while-loop.svg)

Khác với while ở chỗ do...while sẽ chạy thân vòng lặp trước rồi mới kiểm tra điều kiện.

Ví dụ: liệt kê hoán vị:

```cpp
do {
  // làm gì đó...
} while (next_permutation(a + 1, a + n + 1));
```

## Liên hệ giữa ba loại vòng lặp

```cpp
// Câu lệnh for

for (statement1; statement2; statement3) {
  statement4;
}

// Câu lệnh while

statement1;
while (statement2) {
  statement4;
  statement3;
}
```

Khi statement4 không có `continue` (xem dưới) thì hai cách trên tương đương, nhưng cách dưới ít dùng.

```cpp
// Câu lệnh while

statement1;
while (statement2) {
  statement1;
}

// Câu lệnh do...while

do {
  statement1;
} while (statement2);
```

Khi statement1 không có `continue` thì hai cách này cũng tương đương.

```cpp
while (1) {
  // làm gì đó...
}

for (;;) {
  // làm gì đó...
}
```

Hai cách đều là vòng lặp vô hạn (có thể dùng `break` (xem dưới) để thoát).

Có thể thấy ba loại có thể thay thế nhau, nhưng thường chọn theo nguyên tắc:

1.  Khi vòng lặp có bước tăng cố định (phổ biến nhất là liệt kê), dùng for;
2.  Khi chỉ biết điều kiện dừng, dùng while;
3.  Khi cần chạy thân vòng lặp trước rồi mới kiểm tra, dùng do...while. Thường ít gặp, hay dùng cho nhập liệu người dùng.

## Câu lệnh break và continue

break dùng để thoát khỏi vòng lặp.

continue dùng để bỏ qua phần còn lại của thân vòng lặp. Ví dụ với do...while:

```cpp
do {
  // làm gì đó...
  continue;  // tương đương goto END;
// làm gì đó...
END:;
} while (statement);

```

break và continue đều có thể dùng trong cả ba loại vòng lặp.

Thông thường break và continue giúp logic rõ ràng hơn, ví dụ:

```cpp
// Logic khó hiểu hơn, nhiều tầng ngoặc

for (int i = 1; i <= n; ++i) {
  if (i != x) {
    for (int j = 1; j <= n; ++j) {
      if (j != x) {
        // làm gì đó...
      }
    }
  }
}

// Logic rõ ràng hơn, ngoặc gọn gàng

for (int i = 1; i <= n; ++i) {
  if (i == x) continue;
  for (int j = 1; j <= n; ++j) {
    if (j == x) continue;
    // làm gì đó...
  }
}
```

```cpp
// Điều kiện for phức tạp, không thể hiện bản chất "liệt kê"

for (int i = l; i <= r && i % 10 != 0; ++i) {
  // làm gì đó...
}

// for để liệt kê, break để "dừng tại thời điểm"

for (int i = l; i <= r; ++i) {
  if (i % 10 == 0) break;
  // làm gì đó...
}
```

```cpp
// Lặp câu lệnh, thứ tự không tự nhiên

statement1;
while (statement3) {
  statement2;
  statement1;
}

// Không lặp, thứ tự tự nhiên

while (1) {
  statement1;
  if (!statement3) break;
  statement2;
}
```
