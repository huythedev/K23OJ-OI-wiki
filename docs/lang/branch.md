Một chương trình mặc định chạy theo thứ tự code. Đôi khi cần chọn lọc chạy một số câu lệnh, lúc đó dùng nhánh. Chọn nhánh phù hợp có thể tăng hiệu quả.

## Câu lệnh if

### if cơ bản

Cấu trúc:

```cpp
if (điều_kiện) {
  thân;
}
```

if đánh giá điều kiện, nếu đúng (khác 0) thì chạy thân, ngược lại không chạy.

Nếu thân chỉ có một câu lệnh, có thể bỏ ngoặc nhọn.

### if...else

```cpp
if (điều_kiện) {
  thân1;
} else {
  thân2;
}
```

if...else tương tự if; else không cần điều kiện. Nếu điều kiện đúng thì chạy thân1, sai thì chạy thân2. Nếu thân chỉ một câu lệnh, có thể bỏ ngoặc.

### else if

```cpp
if (điều_kiện1) {
  thân1;
} else if (điều_kiện2) {
  thân2;
} else if (điều_kiện3) {
  thân3;
} else {
  thân4;
}
```

else if là tổ hợp if và else, kiểm tra nhiều điều kiện và chọn nhánh. else cuối không cần điều kiện. Ví dụ: nếu điều kiện 1 đúng thì chạy thân1; nếu điều kiện 3 đúng và 1,2 đều sai thì chạy thân3; chỉ khi tất cả sai mới chạy thân4.

Thực chất, đây là else của if đầu chỉ chứa một if, bỏ ngoặc và ghép lại. Khi các điều kiện là song song, cách viết này rõ ràng hơn.

Về logic, tương đương:

> Giải phương trình bậc hai, quan hệ giữa nghiệm và biệt thức:
>
> -   Nếu ($\Delta<0$)
>     vô nghiệm;
> -   Ngược lại, nếu ($\Delta=0$)
>     có hai nghiệm thực trùng nhau;
> -   Ngược lại
>     có hai nghiệm thực phân biệt;

## Câu lệnh switch

```cpp
switch (biểu_thức_chọn) {
  case nhãn1:
    thân1;
  case nhãn2:
    thân2;
  default:
    thân3;
}
```

switch tính giá trị biểu thức chọn, rồi chọn nhãn tương ứng để chạy từ đó. Biểu thức chọn phải là biểu thức kiểu số nguyên, các nhãn phải là hằng số nguyên. Ví dụ:

```cpp
int i = 1;  // i là kiểu int, thỏa điều kiện biểu thức số nguyên

switch (i) {
  case 1:
    cout << "OI WIKI" << endl;
}
```

```cpp
char i = 'A';

// i là kiểu char, cũng thuộc loại số nguyên, thỏa điều kiện
switch (i) {
  case 'A':
    cout << "OI WIKI" << endl;
}
```

Trong switch cần thêm `break` để ngắt, nếu không sau case được chọn sẽ chạy tiếp mọi case dưới và default. Ví dụ:

```cpp
char i = 'B';

switch (i) {
  case 'A':
    cout << "OI" << endl;
    break;

  case 'B':
    cout << "WIKI" << endl;

  default:
    cout << "Hello World" << endl;
}
```

Code trên in `WIKI` và `Hello World`. Nếu không muốn chạy nhánh sau thì cần `break`:

```cpp
char i = 'B';

switch (i) {
  case 'A':
    cout << "OI" << endl;
    break;

  case 'B':
    cout << "WIKI" << endl;
    break;

  default:
    cout << "Hello World" << endl;
}
```

Lúc này chỉ in `WIKI` vì có `break`. Nhánh cuối không cần `break` vì không còn nhánh sau.

Nhãn case không được trùng nhưng có thể đảo thứ tự. Thứ tự xuất hiện không quan trọng. Ví dụ:

```cpp
char i = 'B';

switch (i) {
  case 'B':
    cout << "WIKI" << endl;
    break;

  default:
    cout << "Hello World" << endl;
    break;

  case 'A':
    cout << "OI" << endl;
}
```

Trong switch, có thể thêm ngoặc nhọn cho từng case. Nhưng nếu cần khai báo biến trong switch, **bắt buộc** phải có ngoặc. Ví dụ:

```cpp
char i = 'B';

switch (i) {
  case 'A': {
    int i = 1, j = 2;
    cout << "OI" << endl;
    ans = i + j;
    break;
  }

  case 'B': {
    int qwq = 3;
    cout << "WIKI" << endl;
    ans = qwq * qwq;
    break;
  }

  default: {
    cout << "Hello World" << endl;
  }
}
```

??? note "Hiểu switch như thế nào"
    Ở trên dùng nhiều từ như “case phân nhánh”, “case con”, thực tế ở mức thấp, switch tương đương một nhóm câu lệnh nhảy. Vì vậy có kỹ thuật như Duff's Device, ai quan tâm có thể tự tìm hiểu.
