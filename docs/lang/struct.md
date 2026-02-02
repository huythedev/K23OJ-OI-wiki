author: Ir1d, cjsoft, Lans1ot

**Cấu trúc** (struct) có thể hiểu là một tổ hợp các phần tử gọi là thành viên.

Có thể xem struct như một kiểu dữ liệu do người dùng định nghĩa.

???+ note "Note"
    `struct` trong bài này khác `struct` trong C; trong C++ `struct` đã được mở rộng, tương tự như một bộ mô tả lớp [`class`](./class.md).

## Định nghĩa struct

```cpp
struct Object {
  int weight;
  int value;
} e[array_length];

const Object a;
Object b, B[array_length], tmp;
Object *c;
```

Ví dụ trên định nghĩa struct `Object` với hai thành viên `value, weight` đều là `int`.

Sau dấu `}`, khai báo hằng `a` kiểu `Object`, biến `b`, biến `tmp`, mảng `B`, và con trỏ `c`. Với bất kỳ kiểu đã tồn tại nào, ta đều có thể khai báo hằng/biến/con trỏ/mảng theo cách này.

*Về con trỏ: không bắt buộc phải nắm vững.*

### Định nghĩa con trỏ

Nếu là con trỏ kiểu dựng sẵn thì như bình thường.

Nếu là con trỏ struct, dùng `StructName*` để khai báo.

```cpp
struct Edge {
  /*
  ...
  */
  Edge* nxt;
};
```

Ví dụ chỉ minh họa, không cần bận tâm ý nghĩa thực tế.

## Truy cập/ sửa thành viên

Dùng `tên_biến.tên_thành_viên` để truy cập, ví dụ `cout << var.v` để in thành viên `v`.

Cũng có thể dùng `tên_con_trỏ->tên_thành_viên` hoặc `(*tên_con_trỏ).tên_thành_viên`. Ví dụ `(*ptr).v = tmp` hoặc `ptr->v = tmp` gán thành viên `v` của struct mà `ptr` trỏ tới bằng `tmp`.

## Vì sao cần struct?

Đầu tiên, có thể không dùng struct vẫn đạt cùng hiệu quả. Nhưng struct giúp gộp các thành viên (trong thi đấu thường là biến) lại với nhau một cách rõ ràng. Ví dụ struct `Object` gộp `value, weight` (ý nghĩa là trọng lượng và giá trị vật phẩm). Lợi ích là hạn chế sai sót khi dùng biến.
Hãy tưởng tượng không dùng struct mà có hai mảng `value[]`, `Value[]` rất dễ nhầm; dùng struct sẽ giảm khả năng dùng nhầm biến.

Hơn nữa, các struct khác nhau (kiểu struct như `Object`) hoặc các biến struct khác nhau (thể hiện như mảng `e`) có thể có thành viên trùng tên (như `tmp.value, b.value`), và chúng độc lập (bộ nhớ riêng; sửa `tmp.value` không ảnh hưởng `b.value`).
Lợi ích là có thể dùng tên thành viên giống nhau để mô tả các đối tượng. Ví dụ `Object` có `value`; ta có thể định nghĩa `Car` cũng có `value`; nếu không dùng struct thì phải tách thành `valueOfObject[], valueOfCar[]`, v.v.

*Nếu muốn mô tả chi tiết hơn, có thể định nghĩa hàm thành viên. Tham khảo [lớp](./class.md).*

## Thao tác thêm?

Xem [lớp](./class.md).

## Lưu ý

Để tăng hiệu quả truy cập bộ nhớ, trình biên dịch có thể căn chỉnh thành viên theo byte, tạo khoảng trống. Vì vậy kích thước struct có thể lớn hơn tổng kích thước các thành viên.

## Tài liệu tham khảo

1.  [Class - zh.cppreference.com](https://zh.cppreference.com/w/cpp/language/class)
2.  [Data structures - cplusplus.com](http://www.cplusplus.com/doc/tutorial/structures/)
3.  [Định dạng căn chỉnh - Microsoft Docs](https://docs.microsoft.com/zh-cn/cpp/cpp/alignment-cpp-declarations)
