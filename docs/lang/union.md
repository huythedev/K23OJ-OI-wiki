**Union** (union) là một kiểu lớp đặc biệt, tại một thời điểm chỉ có thể giữ một thành viên dữ liệu không tĩnh.

Union đã được thêm chính thức vào NOI đại纲 nhập môn năm 2023.

## Định nghĩa union

Cách khai báo union tương tự khai báo lớp hoặc [struct](./struct.md):

```cpp
union MyUnion {
  int x;
  long long y;
} x;
```

Định nghĩa union tương tự struct. Theo định nghĩa trên, `MyUnion` cũng có thể dùng như một kiểu tự định nghĩa. Tên `MyUnion` có thể bỏ.

## Truy cập/chỉnh sửa thành viên

Tương tự struct, có thể dùng `tên_biến.tên_thành_viên` để truy cập.

Kích thước bộ nhớ của union **không nhỏ hơn** thành viên lớn nhất của nó, tất cả thành viên **dùng chung vùng nhớ và địa chỉ**. Khi một thành viên được gán giá trị, do bộ nhớ dùng chung, các thành viên khác trong union sẽ bị ghi đè. Tức tại một thời điểm union chỉ lưu được giá trị của một thành viên.

Bạn có thể xem thêm cách dùng union tại [cppreference: khai báo union](https://zh.cppreference.com/w/cpp/language/union).
