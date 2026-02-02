## Về Java

Java là một ngôn ngữ lập trình được sử dụng rộng rãi, có các đặc tính **đa nền tảng**, **hướng đối tượng**, **lập trình generic**, được ứng dụng rộng rãi trong phát triển Web doanh nghiệp và ứng dụng di động.

## Cài đặt môi trường

Tham khảo [JDK](../tools/compiler.md#jdk).

## Cú pháp cơ bản

### Hàm main

Java tương tự C/C++, cần một hàm (trong lập trình hướng đối tượng gọi là phương thức) làm điểm vào thực thi của chương trình.

Định dạng hàm main của Java là cố định, dạng:

```java
class Test {
    public static void main(String[] args) {
        // Mã của chương trình
    }
}
```

Một chương trình Java đã đóng gói (thường có tên `*.jar`) có thể có nhiều hàm như vậy, nhưng khi chạy chương trình, chỉ một hàm được chạy — hàm này được khai báo trong tệp `Manifest` của `Jar`. Trong các kỳ thi OI, thường không cần kiến thức này.

### Chú thích

Giống C/C++, Java dùng `//` và `/* */` để chú thích một dòng và nhiều dòng.

### Kiểu dữ liệu cơ bản

|   Tên kiểu   |   Ý nghĩa  |
| :-----: | :---: |
| boolean |  Kiểu boolean |
|   byte  |  Kiểu byte |
|   char  |  Kiểu ký tự  |
|  double | Số thực dấu chấm động double |
|  float  | Số thực dấu chấm động float |
|   int   |   Số nguyên  |
|   long  |  Số nguyên dài |
|  short  |  Số nguyên ngắn |
|   null  |   Rỗng   |

### Khai báo biến

```java
int a = 12; // Đặt a là kiểu int, và gán giá trị 12
String str = "Hello, OI-wiki"; // Khai báo biến chuỗi str
char ch = 'W';
double PI = 3.1415926;
```

### Từ khóa final

`final` mang ý nghĩa là kết quả cuối cùng, không thể thay đổi; biến được `final` chỉ có thể gán một lần, sau đó không đổi.

```java
final double PI = 3.1415926;
```

### Mảng

```java
// Mảng số nguyên có mười phần tử
// Cú pháp: kiểu_dữ_liệu[] tên_biến = new kiểu_dữ_liệu[kích_thước_mảng]
int[] ary = new int[10];
```

### Chuỗi

-   Chuỗi là một lớp (class) có sẵn trong Java.

```java
// Cách đơn giản nhất để tạo biến chuỗi
String a = "Hello";

// Cũng có thể dùng mảng ký tự để tạo chuỗi
char[] stringArray = { 'H', 'e', 'l', 'l', 'o' };
String s = new String(stringArray);
```

### Gói và nhập gói

Các lớp (Class) trong Java đều nằm trong các gói (package). Trong một gói không cho phép có hai lớp trùng tên. Dòng đầu tiên của lớp thường khai báo lớp thuộc gói nào, ví dụ:

```java
package org.oi-wiki.tutorial;
```

Quy ước đặt tên gói thường là: `miền_cấp_1_của_chủ_dự_án.miền_cấp_2_của_chủ_dự_án.tên_dự_án`.

Dùng từ khóa `import` để nhập các lớp không cùng gói với lớp hiện tại, ví dụ `Scanner`:

```java
import java.util.Scanner;
```

Nếu muốn nhập tất cả các lớp trong một gói, chỉ cần thay tên lớp cuối bằng `*` trước dấu chấm phẩy.

### Nhập

Có thể dùng lớp `Scanner` để xử lý nhập từ dòng lệnh.

```java
package org.oiwiki.tutorial;

import java.util.Scanner;

class Test {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in); // System.in là luồng nhập
        int a = scan.nextInt();
        double b = scan.nextDouble();
        String c = scan.nextLine();
    }
}
```

### Xuất

Có thể định dạng khi in các biến.

|  Ký hiệu  |   Ý nghĩa  |
| :--: | :---: |
| `%f` |  Kiểu số thực |
| `%s` | Kiểu chuỗi |
| `%d` |  Kiểu số nguyên |
| `%c` |  Kiểu ký tự |

```java
class Test {
    public static void main(String[] args) {
        int a = 12;
        char b = 'A';
        double s = 3.14;
        String str = "Hello world";
        System.out.printf("%f\n", s);
        System.out.printf("%d\n", a);
        System.out.printf("%c\n", b);
        System.out.printf("%s\n", str);
    }
}
```

### Câu lệnh điều khiển

Cấu trúc điều khiển luồng trong Java gần như tương tự C++.

#### Rẽ nhánh

-   if

```java
class Test {
    public static void main(String[] args) {
        if ( /* Điều kiện */ ){
            // Thực thi khi điều kiện đúng
        }
    }
}
```

-   if...else

```java
class Test {
    public static void main(String[] args) {
        if ( /* Điều kiện */ ) {
            // Thực thi khi điều kiện đúng
        } else {
            // Thực thi khi điều kiện sai
        }
    }
}
```

-   if...else if...else

```java
class Test {
    public static void main(String[] args) {
        if ( /* Điều kiện */ ) {
            // Thực thi khi điều kiện 1 đúng
        } else if ( /* Điều kiện 2 */ ) {
            // Thực thi khi điều kiện 2 đúng
        } else {
          // Thực thi khi các điều kiện đều sai
        }
    }
}
```

-   switch...case

```java
class Test {
    public static void main(String[] args) {
        switch ( /* Biểu thức */ ){
          case /* Giá trị 1 */:
              // Khi biểu thức có giá trị phù hợp giá trị 1 thì chạy đoạn này
              break; // Nếu không có break, chương trình sẽ chạy tiếp các case phía dưới
          case /* Giá trị 2 */:
              // Khi biểu thức có giá trị phù hợp giá trị 2 thì chạy đoạn này
              break;
          default:
              // Khi biểu thức không khớp các giá trị đã liệt kê thì chạy đoạn này
        }
    }
}
```

#### Vòng lặp

-   for

Từ khóa `for` có hai cách dùng. Cách thứ nhất là vòng lặp `for` thông thường:

```java
class Test {
    public static void main(String[] args) {
        for ( /* Khởi tạo */; /* Điều kiện lặp */; /* Bước sau mỗi vòng */ ) {
            // Khi điều kiện đúng thì thực thi vòng lặp
        }
    }
}
```

Cách thứ hai là kiểu `foreach` như C++, dùng để lặp qua mảng hoặc tập hợp, tương đương việc ẩn biến lặp ở cách trên:

```java
class Test {
    public static void main(String[] args) {
        for ( /* Kiểu phần tử X */ /* Tên phần tử Y */ : /* Tập hợp Z */ ) {
            // Mỗi vòng lặp, Y là một phần tử trong Z.
        }
    }
}
```

-   while

```java
class Test {
    public static void main(String[] args) {
        while ( /* Điều kiện */ ) {
            // Khi điều kiện đúng thì thực thi
        }
    }
}
```

-   do...while

```java
class Test {
    public static void main(String[] args) {
        do {
          // Mã cần thực thi
        } while ( /* Điều kiện lặp */ );
    }
}
```

## Lưu ý

### Tên lớp trùng tên tệp

Khi tạo chương trình Java, cần tên lớp và tên tệp trùng nhau mới biên dịch được, nếu không trình biên dịch sẽ báo không tìm thấy lớp. Thông thường tên tệp được OJ chỉ định.

Ví dụ:

`Add.java`

```java
class Add {
    public static void main(String[] args) {
        // ...
    }
}
```

Trong tệp này phải dùng `Add` làm tên lớp mới biên dịch được.
