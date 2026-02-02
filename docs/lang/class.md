author: Ir1d, cjsoft, Lans1ot, JasonkayZK
Lớp (class) là mở rộng của struct, không chỉ có dữ liệu thành viên mà còn có hàm thành viên.

Trong lập trình hướng đối tượng (OOP), đối tượng là thể hiện của lớp, tức là biến.

Trong C++, `struct` cũng định nghĩa lớp; khái niệm **struct** ở phần trước là từ C. Vì lý do lịch sử, C++ giữ và mở rộng `struct`.

## Định nghĩa lớp

Lớp dùng `class` hoặc `struct`, dưới đây dùng `class`.

```cpp
class ClassName {
  ...
};

// Ví dụ:
class Object {
 public:
  int weight;
  int value;
} e[array_length];

const Object a;
Object b, B[array_length];
Object *c;
```

Tương tự `struct`. Ví dụ trên định nghĩa lớp `Object` với hai thành viên `weight,value`, và sau `}` khai báo mảng `e`.

Con trỏ tới lớp giống [struct](./struct.md).

### Bộ chỉ định truy cập

Khác với ví dụ `struct`, ở đây có `public`, đây là bộ chỉ định truy cập.

-   `public`: các thành viên sau đó đều truy cập được từ **trong lớp** và **ngoài lớp**.
-   `protected`: truy cập được từ **trong lớp**, lớp dẫn xuất hoặc friend, nhưng **không** từ ngoài lớp.
-   `private`: **chỉ** truy cập từ **trong lớp** hoặc friend; **không** từ ngoài lớp hoặc lớp dẫn xuất.

Với `struct`, mặc định là `public`; với `class`, mặc định là `private`.

??? note "Khái niệm cơ bản về friend và lớp dẫn xuất"
    Friend (`friend`): đánh dấu một hàm hoặc lớp để có thể truy cập `private`/`protected` của lớp đó, dù không là thành viên. Nói đơn giản, có `friend` thì truy cập được phần riêng tư.
    
    Lớp dẫn xuất (derived class): C++ cho phép dùng một lớp làm **lớp cơ sở** và tạo **lớp dẫn xuất**. Lớp dẫn xuất kế thừa thành viên và hàm theo quy tắc. Tăng tái sử dụng.
    
    Lớp dẫn xuất giống quan hệ "is". Ví dụ mèo (lớp dẫn xuất) "is" động vật có vú (lớp cơ sở).
    
    Khác biệt `private` và `protected`: lớp dẫn xuất truy cập được `protected` (cũng như `public`) nhưng không truy cập `private`.

## Truy cập và sửa giá trị thành viên

Giống [struct](./struct.md)

-   Với biến, dùng `.`.
-   Với con trỏ, dùng `->`.

## Hàm thành viên

Hàm thành viên là các hàm nằm trong lớp.

??? note "Ví dụ hàm thành viên phổ biến"
    ```cpp
    vector.push_back();
    set.insert();
    queue.empty();
    ```

```cpp
class Class_Name {
  ... type Function_Name(...) { ... }
};

// Ví dụ:
class Object {
 public:
  int weight;
  int value;

  void print() {
    cout << weight << endl;
    return;
  }

  void change_w(int);
};

void Object::change_w(int _weight) { weight = _weight; }

Object var;
```

Lớp có hàm in `Object` và hàm đổi `weight`.

Giống hàm thường, có thể khai báo trước rồi định nghĩa sau như dòng 14 và 17.

Gọi `var.print()` để gọi hàm thành viên.

### Nạp chồng toán tử

??? note "Nạp chồng là gì"
    C++ cho phép nhiều hàm hoặc toán tử cùng tên nhưng khác tham số gọi là **nạp chồng** (overload).
    
    Nếu loại tham số hoặc số lượng khác nhau thì xem là hàm khác nhau.
    
    Lưu ý: nếu chỉ khác kiểu trả về thì **không** thể nạp chồng, compiler sẽ báo lỗi.
    
    Khi gọi không gây mơ hồ (thường mơ hồ nếu có tham số mặc định), compiler chọn hàm dựa vào tham số.
    
    Quá trình đó gọi là phân giải nạp chồng.

Nạp chồng toán tử giúp đơn giản code.

Ví dụ:

```cpp
class Vector {
 public:
  int x, y;

  Vector() : x(0), y(0) {}

  Vector(int _x, int _y) : x(_x), y(_y) {}

  int operator*(const Vector& other) const { return x * other.x + y * other.y; }

  Vector operator+(const Vector&) const;
  Vector operator-(const Vector&) const;
};

Vector Vector::operator+(const Vector& other) const {
  return Vector(x + other.x, y + other.y);
}

Vector Vector::operator-(const Vector& other) const {
  return Vector(x - other.x, y - other.y);
}

// Về dòng 4,5 gán x,y, xem phần sau.
```

Ví dụ định nghĩa lớp Vector và nạp chồng `* + -` lần lượt là tích vô hướng, cộng, trừ vector.

Mẫu nạp chồng:

```text
/*nạp chồng trong lớp*/ Kiểu trả về operatorKýHiệu(Tham số){...}

/*khai báo trong lớp, định nghĩa ngoài*/ Kiểu trả về TênLớp::operatorKýHiệu(Tham số){...}
```

Với lớp tự định nghĩa, nếu nạp chồng một số toán tử (thường chỉ cần `<`), có thể dùng STL container hoặc thuật toán tương ứng như [`sort`](../basic/stl-sort.md).

Xem thêm mục “Tài liệu tham khảo” số 4.

??? note "Toán tử có thể nạp chồng"
    ```text
    +       -       *       /       %       ^       &
    |       ~       !       =       <       >       +=
    -=      *=      /=      %=      ^=      &=      |=
    <<      >>      >>=     <<=     ==      !=      <=
    >=      &&      ||      ++      --      ,       ->*
    ->      ()      []      new     new []  delete  delete []
    ```

### Gán giá trị khởi tạo khi tạo biến

Cần định nghĩa **constructor mặc định** (Default constructor).

```cpp
class ClassName {
  ... ClassName(...)... { ... }
};

// Ví dụ:
class Object {
 public:
  int weight;
  int value;

  Object() {
    weight = 0;
    value = 0;
  }
};
```

Ví dụ này định nghĩa constructor mặc định để khởi tạo `weight` và `value` bằng 0.

Nếu không định nghĩa constructor, compiler tạo constructor mặc định ngầm định, và khởi tạo theo kiểu của thành viên (như biến kiểu cơ bản).

Trong trường hợp đó, thành viên chưa được khởi tạo; đọc giá trị chưa khởi tạo là undefined.

Nếu cần khởi tạo giá trị khác, có thể định nghĩa (nạp chồng) constructor khác.

??? note "Về định nghĩa (nạp chồng) constructor"
    Constructor mặc định thường không có tham số, khác với constructor khác có tham số. Nếu đã định nghĩa constructor có tham số, compiler **không** tự sinh constructor mặc định, nên gọi không tham số sẽ lỗi.

Dùng C++11 trở lên có thể dùng `{}` để khởi tạo.

??? note "Về `{}`"
    Dùng `{}` sẽ sử dụng `std::initializer_list` để khởi tạo.
    
    Quy trình đại khái:
    
    1.  Tìm constructor nhận `std::initializer_list`, nếu có thì gọi (xong thì dừng).
    2.  Thử điền phần tử `{}` vào tham số constructor khác theo thứ tự; nếu điền đủ (kể cả tham số mặc định) thì gọi.
    3.  Nếu không có thành viên `private`, thử gán ngoài lớp theo thứ tự khai báo hoặc chỉ số.
    
    *Quy trình trên là rút gọn, xem "Tài liệu tham khảo 9".*

```cpp
class Object {
 public:
  int weight;
  int value;

  Object() {
    weight = 0;
    value = 0;
  }

  Object(int _weight = 0, int _value = 0) {
    weight = _weight;
    value = _value;
  }

  // tương đương
  // Object(int _weight,int _value):weight(_weight),value(_value) {}
};

// tương đương
// Object::Object(int _weight,int _value){
//   weight = _weight;
//   value = _value;
// }
//}

Object A;        // ok
Object B(1, 2);  // ok
Object C{1, 2};  // ok (C++11)
```

??? note "Về chuyển kiểu ngầm định"
    Có thể gặp code:
    
    ```cpp
    class Node {
     public:
      int var;
    
      Node(int _var) : var(_var) {}
    };
    
    Node a = 1;
    ```
    
    Nhìn có vẻ vô lý vì `int` không thể chuyển thành `node`, nhưng compiler không báo lỗi.
    
    Lý do: khi gán, `1` được dùng để gọi `node::node(int)`, sau đó gọi copy constructor để gán.
    
    Thường người viết muốn compiler báo lỗi; khi đó thêm `explicit` trước constructor.
    
    ```cpp
    class Node {
     public:
      int var;
    
      explicit Node(int _var) : var(_var) {}
    };
    ```
    
    Khi đó `node a=1` sẽ lỗi, còn `node a=node(1)` thì không vì gọi rõ ràng constructor.
    
    *Trong thi đấu, thường tránh bằng cách tuân thủ quy tắc code.*

### Hủy

Mọi biến sẽ bị hủy khi hết phạm vi.

Với con trỏ trỏ đến vùng nhớ cấp phát động, khi bị hủy sẽ **không** tự giải phóng bộ nhớ; cần giải phóng thủ công.

Nếu struct có thành viên là con trỏ, cũng gặp vấn đề này, nên dùng destructor để giải phóng.

**Destructor** sẽ được gọi khi biến bị hủy. Cú pháp giống constructor nhưng thêm `~` phía trước.

*Destructor mặc định thường đủ cho thi đấu; chỉ cần tự viết khi thành viên có con trỏ.*

```cpp
class Object {
 public:
  int weight;
  int value;
  int* ned;

  Object() {
    weight = 0;
    value = 0;
  }

  ~Object() { delete ned; }
};
```

### Gán cho biến lớp

Mặc định, gán sẽ theo quy tắc gán từng thành viên. Có thể dùng `TênLớp()` hoặc `TênLớp{}` làm biến tạm để gán.

Cách đầu chỉ gọi copy constructor, cách sau còn gọi constructor mặc định trước.

Mặc định là **shallow copy**. Nếu có con trỏ, sau khi gán, hai biến sẽ trỏ cùng địa chỉ.

```cpp
// A,tmp1,tmp2,tmp3 kiểu Object
tmp1 = A;
tmp2 = Object(...);
tmp3 = {...};
```

Muốn xử lý con trỏ hay thao tác khác cần nạp chồng constructor tương ứng.

*Xem thêm “Tài liệu tham khảo” số 6 về constructor.*

## Tài liệu tham khảo

1.  [cppreference class](https://zh.cppreference.com/w/cpp/language/class)
2.  [cppreference access](https://zh.cppreference.com/w/cpp/language/access)
3.  [cppreference default_constructor](https://zh.cppreference.com/w/cpp/language/default_constructor)
4.  [cppreference operator](https://zh.cppreference.com/w/cpp/language/operators)
5.  [cplusplus Data structures](http://www.cplusplus.com/doc/tutorial/structures/)
6.  [cplusplus Special members](http://www.cplusplus.com/doc/tutorial/classes2/)
7.  [C++11 FAQ](http://www.stroustrup.com/C++11FAQ.html)
8.  [cppreference Friendship and inheritance](http://www.cplusplus.com/doc/tutorial/inheritance/)
9.  [cppreference value initialization](https://zh.cppreference.com/w/cpp/language/value_initialization)
