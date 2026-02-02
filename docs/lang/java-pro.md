???+ warning "Chú ý"
    Nội dung dưới đây được viết dựa trên Java JDK 8, không loại trừ khả năng có thay đổi ở các phiên bản cao hơn.

## Nhập/xuất nhanh hơn

`Scanner` và `System.out.print` hoạt động tốt lúc đầu, nhưng sẽ giảm hiệu suất khi xử lý input lớn, vì vậy cần dùng một số cách để tăng tốc IO.

### Dùng Kattio + StringTokenizer cho input

Một trong những cách phổ biến là dùng [Kattio.java](https://github.com/Kattis/kattio/blob/master/Kattio.java) từ Kattis để tăng tốc IO.[^ref1] Cách này bọc `StringTokenizer` và `PrintWriter` vào một lớp để tiện sử dụng. Khi giải bài (nếu BTC/đơn vị tổ chức cho phép) có thể dùng trực tiếp mẫu này.

Mẫu IO bên dưới là phiên bản đã chỉnh sửa từ Kattio gốc (Kattio gốc có một số chức năng ít dùng, và dùng giấy phép MIT):

```java
class Kattio extends PrintWriter {
    private BufferedReader r;
    private StringTokenizer st;
    // IO chuẩn
    public Kattio() { this(System.in, System.out); }
    public Kattio(InputStream i, OutputStream o) {
        super(o);
        r = new BufferedReader(new InputStreamReader(i));
    }
    // IO file
    public Kattio(String intput, String output) throws IOException {
        super(output);
        r = new BufferedReader(new FileReader(intput));
    }
    // Trả về null khi không còn input
    public String next() {
        try {
            while (st == null || !st.hasMoreTokens())
                st = new StringTokenizer(r.readLine());
            return st.nextToken();
        } catch (Exception e) {}
        return null;
    }
    public int nextInt() { return Integer.parseInt(next()); }
    public double nextDouble() { return Double.parseDouble(next()); }
    public long nextLong() { return Long.parseLong(next()); }
}
```

Ví dụ sử dụng Kattio:

```java
class Test {
    public static void main(String[] args) {
        Kattio io = new Kattio();
        // Nhập chuỗi
        String str = io.next();
        // Nhập int
        int num = io.nextInt();
        // Xuất
        io.println("Result");
        // Hãy đóng IO để đảm bảo output được ghi đúng
        io.close();
    }
}
```

### Dùng StreamTokenizer cho input

Trong một số trường hợp dùng `StringTokenizer` có thể gây MLE (Memory Limit Exceeded), khi đó cần dùng `StreamTokenizer` để nhập.

```java
import java.io.*;
public class Main {
    // Mã IO
    public static StreamTokenizer in = new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in), 32768));
    public static PrintWriter out = new PrintWriter(new OutputStreamWriter(System.out));
    public static double nextDouble() throws IOException { in.nextToken(); return in.nval; }
    public static float nextFloat() throws IOException { in.nextToken(); return (float)in.nval; }
    public static int nextInt() throws IOException { in.nextToken(); return (int)in.nval; }
    public static String next() throws IOException { in.nextToken(); return in.sval; }
    public static long nextLong() throws Exception { in.nextToken(); return (long)in.nval;}
    
    // Ví dụ sử dụng
    public static void main(String[] args) throws Exception {
        int n = nextInt();
        out.println(n);
        out.close();
    }
}
```

### Phân tích và so sánh Kattio + StringTokenizer với StreamTokenizer

1.  `StreamTokenizer` dùng ít bộ nhớ hơn `StringTokenizer`, khi Java MLE có thể thử `StreamTokenizer`, nhưng `StreamTokenizer` có thể mất độ chính xác khi đọc một số dữ liệu;
    -   Mã nguồn `StreamTokenizer` có `Type`, kiểu này quyết định theo nội dung nhập. Nếu input dạng `123oi` (bắt đầu bằng **số**), nó sẽ ép kiểu thành `double`, vì vậy khi đọc kiểu `String` sẽ ném exception;
    -   `StreamTokenizer` sẽ mất độ chính xác khi đọc số lớn hơn `1e14`;
2.  Khi dùng `PrintWriter`, chú ý cuối chương trình phải `close()` hoặc khi cần xuất thì `flush()` để xả buffer, nếu không nội dung sẽ không được ghi ra console/file.
3.  `Kattio` kế thừa `PrintWriter`, nên đối tượng có luôn các hàm của `PrintWriter` để xuất, đồng thời dùng `StringTokenizer` làm biến thành viên. Còn cách `Main` thứ hai dùng `StreamTokenizer` và `PrintWriter` như các biến thành viên riêng, nên cách dùng hơi khác.

Tóm lại, đa số trường hợp `StringTokenizer` ưu thế hơn `StreamTokenizer`. Khi MLE cực hạn có thể thử `StreamTokenizer`, nhưng dữ liệu vượt phạm vi `int` thì `StreamTokenizer` cũng không xử lý tốt.

## BigInteger và số học

`BigInteger` là lớp tính toán độ chính xác cao trong Java, rất tiện để xử lý bài toán số lớn.

### Khởi tạo

`BigInteger` có hai cách tạo thường dùng:

```java
import java.io.PrintWriter;
import java.math.BigInteger;

class Main {
    static PrintWriter out = new PrintWriter(System.out);
    public static void main(String[] args) {
        BigInteger a = new BigInteger("12345678910");  // Tạo BigInteger từ chuỗi thập phân
        out.println(a);  // a = 12345678910
        BigInteger b = new BigInteger("1E", 16);  // Tạo BigInteger từ chuỗi theo hệ cơ số chỉ định
        out.println(b);  // b = 30
        out.close();
    }
}

```

### Phép toán cơ bản

Dùng `this` để chỉ `BigInteger` hiện tại:

|             Tên hàm             |               Chức năng               |
| :-------------------------: | :----------------------------: |
|           `abs()`           |         Trả về giá trị tuyệt đối của `this`         |
|          `negate()`         |         Trả về số đối của `this`         |
|    `add(BigInteger val)`    |      Trả về tổng của `this` và `val`      |
|  `subtract(BigInteger val)` |      Trả về hiệu của `this` và `val`      |
|  `multiply(BigInteger val)` |      Trả về tích của `this` và `val`      |
|   `divide(BigInteger val)`  |      Trả về thương của `this` và `val`      |
| `remainder(BigInteger val)` |     Trả về số dư của `this` chia `val`     |
|    `mod(BigInteger val)`    |     Trả về `this` mod `val`     |
|        `pow(int val)`       |      Trả về `this` mũ `val`      |
|    `and(BigInteger val)`    |     Trả về phép AND bit của `this` và `val`     |
|     `or(BigInteger val)`    |     Trả về phép OR bit của `this` và `val`     |
|           `not()`           |         Trả về phép NOT bit của `this`        |
|    `xor(BigInteger val)`    |     Trả về phép XOR bit của `this` và `val`    |
|      `shiftLeft(int n)`     |       Trả về `this` dịch trái `n` bit       |
|     `shiftRight(int n)`     |     Trả về `this` dịch phải `n` bit     |
|    `max(BigInteger val)`    |     Trả về giá trị lớn hơn giữa `this` và `val`     |
|    `min(BigInteger val)`    |     Trả về giá trị nhỏ hơn giữa `this` và `val`     |
|         `bitCount()`        | Trả về số bit `1` (không tính bit dấu) trong `this` |
|        `bitLength()`        |    Trả về độ dài nhị phân (không tính bit dấu) của `this`    |
|     `getLowestSetBit()`     |      Trả về vị trí bit `1` thấp nhất của `this`     |
| `compareTo(BigInteger val)` |      So sánh giá trị `this` và `val`     |
|         `toString()`        |      Trả về biểu diễn thập phân của `this`     |
|    `toString(int radix)`    |  Trả về biểu diễn `this` theo cơ số `radix` |

Ví dụ sử dụng:

```java
import java.io.PrintWriter;
import java.math.BigInteger;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BigInteger a, b;
    
    static void abs() {
        out.println("abs:");
        a = new BigInteger("-123");
        out.println(a.abs());  // In 123
        a = new BigInteger("123");
        out.println(a.abs());  // In 123
    }
    
    static void negate() {
        out.println("negate:");
        a = new BigInteger("-123");
        out.println(a.negate());  // In 123
        a = new BigInteger("123");
        out.println(a.negate());  // In -123
    }
    
    static void add() {
        out.println("add:");
        a = new BigInteger("123");
        b = new BigInteger("123");
        out.println(a.add(b));  // In 246
    }
    
    static void subtract() {
        out.println("subtract:");
        a = new BigInteger("123");
        b = new BigInteger("123");
        out.println(a.subtract(b));  // In 0
    }
    
    static void multiply() {
        out.println("multiply:");
        a = new BigInteger("12");
        b = new BigInteger("12");
        out.println(a.multiply(b));  // In 144
    }
    
    static void divide() {
        out.println("divide:");
        a = new BigInteger("12");
        b = new BigInteger("11");
        out.println(a.divide(b));  // In 1
    }
    
    static void remainder() {
        out.println("remainder:");
        a = new BigInteger("12");
        b = new BigInteger("10");
        out.println(a.remainder(b));  // In 2
        a = new BigInteger("-12");
        b = new BigInteger("10");
        out.println(a.remainder(b));  // In -2
    }
    
    static void mod() {
        out.println("mod:");
        a = new BigInteger("12");
        b = new BigInteger("10");
        out.println(a.mod(b));  // In 2
        a = new BigInteger("-12");
        b = new BigInteger("10");
        out.println(a.mod(b));  // In 8
    }
    
    static void pow() {
        out.println("pow:");
        a = new BigInteger("2");
        out.println(a.pow(10));  // In 1024
    }
    
    static void and() {
        out.println("and:");
        a = new BigInteger("3");  // 11
        b = new BigInteger("5");  // 101
        out.println(a.and(b));  // In 1
    }
    
    static void or() {
        out.println("or:");
        a = new BigInteger("2");  // 10
        b = new BigInteger("5");  // 101
        out.println(a.or(b));  // In 7
    }
    
    static void not() {
        out.println("not:");
        a = new BigInteger("2147483647");  // 01111111 11111111 11111111 11111111
        out.println(a.not());  // In -2147483648, nhị phân: 10000000 00000000 00000000 00000000
    }
    
    static void xor() {
        out.println("xor:");
        a = new BigInteger("6");  // 110
        b = new BigInteger("5");  // 101
        out.println(a.xor(b));  // 011 => In 3
    }
    
    static void shiftLeft() {
        out.println("shiftLeft:");
        a = new BigInteger("1");
        out.println(a.shiftLeft(10));  // In 1024
    }
    
    static void shiftRight() {
        out.println("shiftRight:");
        a = new BigInteger("1024");
        out.println(a.shiftRight(8));  // In 4
    }
    
    static void max() {
        out.println("max:");
        a = new BigInteger("6");
        b = new BigInteger("5");
        out.println(a.max(b));  // In 6
    }
    
    static void min() {
        out.println("min:");
        a = new BigInteger("6");
        b = new BigInteger("5");
        out.println(a.min(b));  // In 5
    }
    
    static void bitCount() {
        out.println("bitCount:");
        a = new BigInteger("6");  // 110
        out.println(a.bitCount());  // In 2
    }
    
    static void bitLength() {
        out.println("bitLength:");
        a = new BigInteger("6");  // 110
        out.println(a.bitLength());  // In 3
    }
    
    static void getLowestSetBit() {
        out.println("getLowestSetBit:");
        a = new BigInteger("8");  // 1000
        out.println(a.getLowestSetBit());  // In 3
    }
    
    static void compareTo() {
        out.println("compareTo:");
        a = new BigInteger("8");
        b = new BigInteger("9");
        out.println(a.compareTo(b));  // In -1
        a = new BigInteger("8");
        b = new BigInteger("8");
        out.println(a.compareTo(b));  // In 0
        a = new BigInteger("8");
        b = new BigInteger("7");
        out.println(a.compareTo(b));  // In 1
    }
    
    static void toStringTest() {
        out.println("toString:");
        a = new BigInteger("15");
        out.println(a.toString());  // In 15
        out.println(a.toString(16));  // In f
    }
    
    public static void main(String[] args) {
        abs();
        negate();
        add();
        subtract();
        multiply();
        divide();
        remainder();
        mod();
        pow();
        and();
        or();
        not();
        xor();
        shiftLeft();
        shiftRight();
        max();
        min();
        bitCount();
        bitLength();
        getLowestSetBit();
        compareTo();
        toStringTest();
        out.close();
    }
}
```

### Toán học

Dùng `this` để chỉ `BigIntger` hiện tại:

|                  Tên hàm                 |                Chức năng                |
| :----------------------------------: | :------------------------------: |
|         `gcd(BigInteger val)`        | Trả về gcd của \|this\| và \|val\| |
|      `isProbablePrime(int val)`      |      Trả về `this` có phải số nguyên tố không     |
|         `nextProbablePrime()`        |        Trả về số nguyên tố đầu tiên lớn hơn `this`        |
| `modPow(BigInteger b, BigInteger p)` |    Trả về `this^b mod p`    |
|      `modInverse(BigInteger p)`      |     Trả về nghịch đảo nhân của `this` theo mod `p`    |

Ví dụ sử dụng:

```java
import java.io.PrintWriter;
import java.math.BigInteger;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BigInteger a, b, p;
    
    static void gcd() {  // Ước chung lớn nhất
        a = new BigInteger("120032414321432144212100");
        b = new BigInteger("240231431243123412432140");
        out.println(String.format("gcd(%s,%s)=%s", a.toString(), b.toString(), a.gcd(b).toString()));  // gcd(120032414321432144212100,240231431243123412432140)=20
    }
    
    static void isPrime() {  // Dựa trên Miller-Rabin, tham số càng lớn càng chính xác, phức tạp hơn; độ chính xác (1-1/(val*2))
        a = new BigInteger("1200324143214321442127");
        out.println("a:" + a.toString());
        out.println(a.isProbablePrime(10) ? "a is prime" : "a is not prime");  // a is not prime
    }
    
    static void nextPrime() {  // Tìm số nguyên tố kế tiếp
        a = new BigInteger("1200324143214321442127");
        out.println("a:" + a.toString());
        out.println(String.format("a nextPrime is %s", a.nextProbablePrime().toString()));  // a nextPrime is 1200324143214321442199
    }
    
    static void modPow() {  // Lũy thừa nhanh, có tối ưu toán học
        a = new BigInteger("2");
        b = new BigInteger("10");
        p = new BigInteger("1000");
        out.println(String.format("a:%s b:%s p:%s", a, b, p));
        out.println(String.format("a^b mod p:%s", a.modPow(b, p).toString()));//  24
    }
    
    static void modInverse() {  // Nghịch đảo
        a = new BigInteger("10");
        b = new BigInteger("3");
        out.println(a.modInverse(b));  // a ^ (p-2) mod p = 1
    }
    
    public static void main(String[] args) {
        gcd();
        isPrime();
        nextPrime();
        modPow();
        modInverse();
        out.close();
    }
}
```

Về Miller-Rabin, tham khảo [Miller–Rabin 素性测试](../math/number-theory/prime.md#millerrabin-素性测试).

## Kiểu dữ liệu cơ bản và kiểu bao (wrapper)

### Giới thiệu

Do kiểu cơ bản không mang tính hướng đối tượng, Java cung cấp 8 lớp wrapper cho 8 kiểu cơ bản: `Byte`、`Double`、`Float`、`Integer`、`Long`、`Short`、`Character`、`Boolean`. Tương ứng như sau:

|   Kiểu cơ bản  |    Kiểu wrapper   |
| :-------: | :---------: |
|   `byte`  |    `Byte`   |
|  `short`  |   `Short`   |
| `boolean` |  `Boolean`  |
|   `char`  | `Character` |
|   `int`   |  `Integer`  |
|   `long`  |    `Long`   |
|  `float`  |   `Float`   |
|  `double` |   `Double`  |

### Khác biệt

Lấy `int` và `Integer` làm ví dụ:

1.  `Integer` là wrapper của `int`, `int` là kiểu cơ bản.
2.  `Integer` phải khởi tạo đối tượng mới dùng được, `int` thì không.
3.  `Integer` thực chất là tham chiếu; `new Integer` tạo một đối tượng, còn `int` lưu trực tiếp giá trị.
4.  `Integer` mặc định là `null`, nhận `null` và `int`; `int` mặc định 0 và không nhận `null`.
5.  So sánh `Integer` bằng `==` có thể sai, chỉ nên dùng `equals()`, còn `int` có thể dùng `==`.

### Autoboxing và unboxing

Ví dụ với `int` và `Integer`:

`Integer` là đối tượng, `int` là kiểu cơ bản, không thể gán trực tiếp. Cần chuyển kiểu: từ cơ bản sang wrapper gọi là boxing, ngược lại là unboxing.

```java
// Kiểu cơ bản
int value1 = 1;
// Boxing sang wrapper
Integer integer = Integer.valueOf(value1);
// Unboxing về kiểu cơ bản
int value2 = integer.intValue();
```

Java 5 thêm cơ chế auto boxing/unboxing:

```java
Integer integer = 1;
int value = integer;
```

???+ warning "Chú ý"
    Dù có auto boxing/unboxing, vẫn nên chọn kiểu phù hợp khi khai báo biến vì wrapper `Integer` nhận `null`, còn `int` thì không. Unboxing từ `null` sẽ ném exception. Ví dụ:
    
    ```java
    Integer integer = Integer.valueOf(null);
    integer.intValue();  // Ném java.lang.NumberFormatException
    
    Integer integer = null;
    integer.intValue();  // Ném java.lang.NullPointerException
    ```

## Kế thừa

Tạo thiết kế mới dựa trên thiết kế có sẵn là kế thừa trong OOP. Lớp mới được định nghĩa dựa trên lớp đã có. Qua kế thừa, lớp mới nhận được tất cả thành viên của lớp cơ sở, gồm biến và phương thức, gồm cả `public` và `private`. Nhờ vậy, định nghĩa lớp mới nhanh và tiện hơn so với viết từ đầu. Kế thừa là một cách quan trọng để tái sử dụng code.

Trong Java, từ khóa kế thừa là `extends`, Java chỉ hỗ trợ đơn kế thừa nhưng có thể implements nhiều interface.

Mọi lớp đều là con của `Object`.

Lớp con kế thừa mọi thành viên của lớp cha, trừ constructor. Constructor là đặc trưng của lớp cha và không tồn tại trong lớp con. Ngoài ra, lớp con thừa hưởng tất cả thành viên.

Mỗi thành viên có mức truy cập khác nhau. Lớp con nhận tất cả thành viên, nhưng mức truy cập ảnh hưởng cách sử dụng: có thành viên trở thành giao diện công khai, có thành viên bị ẩn và ngay cả lớp con cũng không truy cập được.

Bảng mức truy cập:

|    Mức truy cập ở lớp cha   |      Ý nghĩa trong lớp cha      |                     Ý nghĩa trong lớp con                    |
| :-----------: | :---------------: | :--------------------------------------------: |
|    `public`   |       Mở cho mọi lớp      |                     Mở cho mọi lớp                     |
|  `protected`  | Chỉ lớp cùng package, chính nó và lớp con truy cập |                Chỉ lớp cùng package, chính nó và lớp con truy cập               |
| Mặc định (`default`) |    Chỉ lớp cùng package truy cập    | Nếu lớp con cùng package, chỉ lớp cùng package truy cập; nếu khác package thì như `private`, không truy cập |
|   `private`   |      Chỉ chính nó truy cập     |                      Không truy cập được                      |

## Đa hình

Trong Java, khi gán một đối tượng cho một biến, kiểu của đối tượng phải khớp với kiểu của biến. Nhưng do có kế thừa, **một biến có thể lưu kiểu được khai báo hoặc bất kỳ kiểu con nào**.

Nếu một kiểu implements interface, nó cũng là kiểu con của interface đó.

Biến lưu đối tượng trong Java là biến đa hình. “Đa hình” nghĩa là một biến có thể lưu các đối tượng khác kiểu (kiểu khai báo hoặc các kiểu con).

Biến đa hình:

1.  Biến đối tượng trong Java là đa hình, có thể lưu nhiều kiểu đối tượng.
2.  Có thể lưu đối tượng đúng kiểu khai báo hoặc kiểu con.
3.  Gán đối tượng con cho biến kiểu cha gọi là upcasting.

## Generic

Generic là khi định nghĩa lớp không cố định kiểu thuộc tính/tham số, mà chỉ xác định kiểu khi sử dụng (hoặc tạo đối tượng). Bản chất là tham số hóa kiểu dữ liệu.

Generic cung cấp cơ chế kiểm tra an toàn kiểu ở thời điểm biên dịch, cho phép phát hiện kiểu không hợp lệ.

## Interface

### Giới thiệu

Interface trong Java là một kiểu trừu tượng, tập hợp các phương thức trừu tượng, khai báo bằng `interface`. Một lớp implements interface để “kế thừa” các phương thức trừu tượng.

Interface không phải là class. Cách viết gần giống class, nhưng khái niệm khác nhau. Class mô tả thuộc tính và phương thức của đối tượng; interface chứa các phương thức cần được triển khai.

Trừ khi lớp implements là abstract, lớp phải định nghĩa tất cả phương thức trong interface.

Interface không thể khởi tạo, nhưng có thể được implement. Một lớp implement interface phải hiện thực mọi phương thức mô tả trong interface, nếu không phải khai báo abstract. Kiểu interface cũng có thể dùng để khai báo biến; biến này có thể là null hoặc trỏ tới đối tượng implement interface.

### Khác biệt với class

1.  Interface không thể tạo instance.
2.  Interface không có constructor.
3.  Mọi phương thức trong interface phải là abstract; từ Java 8 có thể có phương thức `default`.
4.  Interface không chứa biến thành viên, trừ `static` và `final`.
5.  Class không kế thừa interface mà là implement.
6.  Interface hỗ trợ đa kế thừa, class thì không.

### Khai báo

```java
[Độ truy cập] interface TênInterface [extends các interface khác] {
        // Khai báo biến
        // Phương thức trừu tượng
}
```

### Implement

```java
...implements TênInterface[, TênInterface khác, ...] ...
```

## Biểu thức Lambda

### Giới thiệu

Lambda là một tính năng quan trọng nhất của Java 8.

Lambda cho phép truyền hàm như tham số của phương thức (hàm là tham số).

Dùng lambda giúp code gọn và súc tích hơn.

### Cú pháp

-   Kiểu tham số có thể bỏ: compiler tự suy ra.
-   Dấu ngoặc tham số có thể bỏ nếu chỉ có một tham số.
-   Dấu ngoặc nhọn có thể bỏ nếu thân chỉ có một câu lệnh.
-   Từ khóa `return` có thể bỏ nếu chỉ có một biểu thức trả về; nếu dùng `{}` phải có `return`.

Ví dụ:

```java
// 1. Không có tham số, trả về 5
() -> 5

// 2. Nhận một tham số (số), trả về gấp đôi
x -> 2 * x

// 3. Nhận 2 tham số (số) trả về hiệu
(x, y) -> x – y

// 4. Nhận 2 int trả về tổng
(int x, int y) -> x + y

// 5. Nhận String và in ra console, không trả về
(String s) -> System.out.print(s)
```

Ví dụ sắp xếp mảng chuỗi theo độ dài:

```java
import java.util.Arrays;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    
    public static void main(String[] args) {
        String[] plants = {"Mercury", "venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"};
        Arrays.sort(plants, (String first, String second) -> (first.length() - second.length()));
        for (String word : plants) {
            out.print(word + " ");
        }
        out.close();
    }
}
```

Ví dụ dùng nhiều câu lệnh trong lambda:

```java
import java.io.PrintWriter;
import java.util.Arrays;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    
    public static void main(String[] args) {
        String[] plants = {"Mercury", "venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"};
        Arrays.sort(plants, (first, second) ->
        {
            // Không ghi kiểu tham số, có thể suy từ ngữ cảnh
            int result = first.length() - second.length();
            return result;
        });
        for (String word : plants) {
            out.print(word + " ");
        }
        out.close();
    }
}
```

`->` là ký hiệu suy diễn, biểu thị nhận tham số bên trái và trả về/thi hành phần bên phải (tức truyền phương thức).

### Functional Interface

1.  Là một interface theo định nghĩa của Java.
2.  Chỉ chứa một phương thức trừu tượng.
3.  Vì chỉ có một phương thức chưa hiện thực, lambda có thể tự động điền vào.

Ví dụ dùng functional interface:

???+ example "In ra các chuỗi có độ dài chia hết cho 2"
    ```java
    import java.io.PrintWriter;
    
    public class Main {
        static PrintWriter out = new PrintWriter(System.out);
        
        public static void main(String[] args) {
            String[] plants = {"Mercury", "venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"};
            Test test = s -> {  // Lambda là một instance của functional interface
                if (s.length() % 2 == 0) {
                    return true;
                }
                return false;
            };
            for (String word : plants) {
                if (test.check(word)) {
                    out.print(word + " ");
                }
            }
            out.close();
        }
    }
    
    interface Test {
        public boolean check(String s);
    }
    ```

???+ example "Thực hiện 4 phép tính cộng trừ nhân chia"
    ```java
    import java.io.PrintWriter;
    
    public class Main {
        static PrintWriter out = new PrintWriter(System.out);
        
        public static double calc(double a, double b, Calculator util) {
            return util.operation(a, b);
        }
        
        public static void main(String[] args) {
            Calculator util[] = new Calculator[4];  // Mảng functional interface
            util[0] = (a, b) -> a + b;
            util[1] = (a, b) -> a - b;
            util[2] = (a, b) -> a * b;
            util[3] = (a, b) -> a / b;
            double a = 20, b = 15;
            for (Calculator c : util) {
                System.out.println(calc(a, b, c));
            }
            out.close();
        }
    }
    
    interface Calculator {
        public double operation(double a, double b);
    }
    ```

## Collection

`Collection` là một interface trong Java, được nhiều interface container generic implement. Ở đây `Collection` chỉ cấu trúc dữ liệu lưu đối tượng.

Trong Java, phần tử của `Collection` phải là kiểu đối tượng, không thể là kiểu cơ bản.

Nội dung dưới đây dựa trên tính đa hình của Java, đều dùng dạng implement interface.

Các interface thường dùng: `List`、`Queue`、`Set`、`Map`.

### Định nghĩa container

Khi định nghĩa container generic, cần chỉ rõ kiểu dữ liệu. Nếu không chỉ rõ kiểu (dùng `Object`), Java 8 vẫn compile được nhưng có nhiều cảnh báo.

Ví dụ an toàn:

```java
List<Integer> list1 = new LinkedList<>();
```

Ví dụ gây cảnh báo:

```java
List list = new ArrayList<>();
list.add(1);
list.add(true);
list.add(1.01);
list.add(1L);
list.add("I am String");
```

Vì vậy nếu không có nhu cầu đặc biệt thì không khuyến nghị cách thứ 2. Compiler không thể kiểm tra an toàn kiểu; `list.get(index)` trả về `Object` nên phải tự ép kiểu, dễ sai.

Nếu xác định kiểu như `List<Integer>`, compiler sẽ kiểm tra kiểu phần tử, chỉ nhận số nguyên. Khi khai báo collection, phải dùng wrapper như `List<Integer>` hoặc class tự định nghĩa, không thể dùng `List<int>`.

### List

#### ArrayList

`ArrayList` là mảng có thể mở rộng, kích thước mặc định 10. Nếu vượt quá sẽ mở rộng theo tỷ lệ $\dfrac{3}{2}$.

##### Khởi tạo

```java
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    
    public static void main(String[] args) {
        List<Integer> list1 = new ArrayList<>();  // Tạo mảng tự tăng list1, kích thước mặc định (10)
        List<Integer> list2 = new ArrayList<>(30);  // Tạo list2, kích thước ban đầu 30
        List<Integer> list3 = new ArrayList<>(list2);  // Tạo list3, dùng phần tử và size của list2 làm giá trị ban đầu
    }
}
```

#### LinkedList

`LinkedList` là danh sách liên kết đôi.

##### Khởi tạo

```java
import java.io.PrintWriter;
import java.util.LinkedList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    
    public static void main(String[] args) {
        List<Integer> list1 = new LinkedList<>();  // Tạo danh sách liên kết đôi list1
        List<Integer> list2 = new LinkedList<>(list1);  // Tạo list2, thêm toàn bộ phần tử list1
    }
}
```

#### Phương thức thường dùng

Dùng `this` để chỉ `List<Integer>` hiện tại:

|            Tên hàm            |                Chức năng                |
| :-----------------------: | :------------------------------: |
|          `size()`         |           Trả về độ dài của `this`          |
|     `add(Integer val)`    |      Thêm `val` vào cuối `this`      |
| `add(int idx, Integer e)` |   Chèn `e` vào vị trí `idx` trong `this`   |
|       `get(int idx)`      | Lấy phần tử tại `idx`, nếu vượt sẽ ném exception |
| `set(int idx, Integer e)` |   Gán `this[idx] = e`   |

Ví dụ và so sánh:

```java
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static List<Integer> array = new ArrayList<>();
    static List<Integer> linked = new LinkedList<>();
    
    static void add() {
        array.add(1);  // Độ phức tạp O(1)
        linked.add(1);  // Độ phức tạp O(1)
    }
    
    static void get() {
        array.get(10);  // Độ phức tạp O(1)
        linked.get(10);  // Độ phức tạp O(11)
    }
    
    static void addIdx() {
        array.add(0, 2);  // Trường hợp xấu nhất O(n)
        linked.add(0, 2);  // Trường hợp xấu nhất O(n)
    }
    
    static void size() {
        array.size();  // Độ phức tạp O(1)
        linked.size();  // Độ phức tạp O(1)
    }
    
    static void set() {  // Hàm này trả về giá trị cũ tại vị trí đó
        array.set(0, 1);  // Độ phức tạp O(1)
        linked.set(0, 1);  // Trường hợp xấu nhất O(n)
    }

}
```

#### Duyệt

```java
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static List<Integer> array = new ArrayList<>();
    static List<Integer> linked = new LinkedList<>();
    
    static void function1() {  // Duyệt cơ bản
        for (int i = 0; i < array.size(); i++) {
            out.println(array.get(i));  // Duyệt ArrayList, O(n)
        }
        for (int i = 0; i < linked.size(); i++) {
            out.println(linked.get(i));  // Duyệt LinkedList, O(n^2) vì get(i) là O(i)
        }
    }
    
    static void function2() {  // Duyệt bằng for-each
        for (int e : array) {
            out.println(e);
        }
        for (int e : linked) {
            out.println(e);  // Đều O(n)
        }
    }
    
    static void function3() {  // Duyệt bằng iterator
        Iterator<Integer> iterator1 = array.iterator();
        Iterator<Integer> iterator2 = linked.iterator();
        while (iterator1.hasNext()) {
            out.println(iterator1.next());
        }
        while (iterator2.hasNext()) {
            out.println(iterator2.next());
        }  // Đều O(n)
    }

}
```

???+ warning "Chú ý"
    Không xóa phần tử trong `for` hoặc `foreach` khi đang duyệt `List`, nếu không sẽ ném exception.
    
    Lý do: `list.size()` thay đổi nhưng số lần lặp dự kiến không đổi. Phần tử ở `index` kế tiếp bị xóa sẽ dồn lên, vòng lặp tiếp theo sẽ xử lý phần tử sai, gây kết quả không mong muốn.

### Queue

#### LinkedList

Có thể dùng `LinkedList` làm queue thường, nền là danh sách liên kết.

##### Khởi tạo

```java
Queue<Integer> q = new LinkedList<>();
```

`LinkedList` implement `List` và `Deque`, mà `Deque` kế thừa `Queue`, nên `LinkedList` vừa là `List` vừa là `Queue`.

#### ArrayDeque

Có thể dùng `ArrayDeque` làm queue thường, nền là mảng.

##### Khởi tạo

```java
Queue<Integer> q = new ArrayDeque<>();
```

`ArrayDeque` implement `Deque`, mà `Deque` kế thừa `Queue`, nên `ArrayDeque` là `Queue`.

#### Khác biệt khi implement Queue giữa LinkedList và ArrayDeque

1.  Cấu trúc dữ liệu: `ArrayDeque` và `LinkedList` đều implement `Deque`. Nhưng `ArrayDeque` không implement `List`, nên không có thao tác theo chỉ số.
2.  Thread safety: cả hai không đồng bộ, không thread-safe.
3.  Cài đặt: `ArrayDeque` dựa trên mảng động; `LinkedList` dựa trên danh sách liên kết đôi.
4.  Tốc độ duyệt: `ArrayDeque` là vùng nhớ liên tục, tận dụng cache tốt; `LinkedList` rời rạc, kém cache.
5.  Tốc độ thao tác: stack/queue đều O(1), `ArrayDeque` đôi khi mở rộng nhưng xét amortized vẫn O(1).
6.  Bộ nhớ: `ArrayDeque` có chỗ trống đầu/cuối; `LinkedList` có con trỏ prev/next.

#### PriorityQueue

`PriorityQueue` là hàng đợi ưu tiên, mặc định là heap nhỏ.

##### Khởi tạo

```java
Queue<Integer> q1 = new PriorityQueue<>();  // Heap nhỏ
Queue<Integer> q2 = new PriorityQueue<>((x, y) -> {return y - x;});  // Heap lớn
```

#### Phương thức thường dùng

Định nghĩa queue là `Queue<Integer>`:

|          Tên hàm         |                     Chức năng                     |
| :------------------: | :----------------------------------------: |
|       `size()`       |                  Trả về độ dài queue                  |
|  `add(Integer val)`  |     Thêm `val` vào queue; nếu vượt giới hạn sẽ ném exception     |
| `offer(Integer val)` | Thêm `val` vào queue; nếu vượt giới hạn thì thất bại nhưng không ném exception |
|      `isEmpty()`     |            Kiểm tra queue rỗng, rỗng trả `true`           |
|       `peek()`       |            Trả về phần tử đầu; nếu rỗng trả `null`           |
|       `poll()`       |          Trả về và xóa phần tử đầu; nếu rỗng trả `null`          |

Ví dụ và so sánh:

```java
import java.io.PrintWriter;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Queue<Integer> q1 = new LinkedList<>();
    static Queue<Integer> q2 = new PriorityQueue<>();
    
    static void add() {  // add và offer khác nhau ở chỗ có ném exception hay không
        q1.add(1);  // O(1)
        q2.add(1);  // O(logn)
    }
    
    static void isEmpty() {
        q1.isEmpty();  // O(1)
        q2.isEmpty();  // O(1)
    }
    
    static void size() {
        q1.size();  // O(1)
        q2.size();  // Trả về độ dài q2
    }
    
    static void peek() {
        q1.peek();  // O(1)
        q2.peek();  // O(logn)
    }
    
    static void poll() {
        q1.poll();  // O(1)
        q2.poll();  // O(logn)
    }
}
```

#### Duyệt

```java
import java.io.PrintWriter;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Queue<Integer> q1 = new LinkedList<>();
    static Queue<Integer> q2 = new PriorityQueue<>();
    
    static void test() {
        while (!q1.isEmpty()) {  // O(n)
            out.println(q1.poll());
        }
        while (!q2.isEmpty()) {  // O(nlogn)
            out.println(q2.poll());
        }
    }

}
```

### Deque

`Deque` là hàng đợi hai đầu trong Java, thường dùng cho thao tác queue và stack.

#### Hàm chính

Định nghĩa là `Deque<Integer>`:

|            Tên hàm            |                     Chức năng                     |
| :-----------------------: | :----------------------------------------: |
|  `addFirst(Integer val)`  |     Thêm `val` vào đầu; nếu vượt giới hạn ném exception     |
| `offerFirst(Integer val)` | Thêm `val` vào đầu; nếu vượt giới hạn thất bại nhưng không ném exception |
|      `removeFirst()`      |           Trả về và xóa phần tử đầu; nếu rỗng ném exception           |
|       `pollFirst()`       |         Trả về và xóa phần tử đầu; nếu rỗng trả `null`        |
|       `peekFirst()`       |          Trả về phần tử đầu; nếu rỗng trả `null`          |
|    `push(Integer val)`    |         Thêm `val` vào đầu, tương đương `addFirst`        |
|          `pop()`          |         Trả về và xóa phần tử đầu, tương đương `removeFirst`        |
|         `remove()`        |          Xóa phần tử đầu, tương đương `removeFirst`          |
|          `poll()`         |           Xóa phần tử đầu, tương đương `pollFirst`           |
|   `addLast(Integer val)`  |     Thêm `val` vào cuối; nếu vượt giới hạn ném exception     |
|  `offerLast(Integer val)` | Thêm `val` vào cuối; nếu vượt giới hạn thất bại nhưng không ném exception |
|       `removeLast()`      |           Trả về và xóa phần tử cuối; nếu rỗng ném exception           |
|        `pollLast()`       |         Trả về và xóa phần tử cuối; nếu rỗng trả `null`        |
|        `peekLast()`       |          Trả về phần tử cuối; nếu rỗng trả `null`          |
|     `add(Integer val)`    |         Thêm `val` vào cuối, tương đương `addLast`         |
|    `offer(Integer val)`   |        Thêm `val` vào cuối, tương đương `offerLast`        |

#### Thao tác stack

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Main {
    static Deque<Integer> stack = new ArrayDeque<>();
    static int[] a = {1, 2, 3, 4, 5};
    
    public static void main(String[] args) {
        for (int v : a) {
            stack.push(v);
        }
        while (!stack.isEmpty()) { // In 5 4 3 2 1
            System.out.println(stack.pop()); 
        }
    }
}

```

#### Thao tác deque

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Main {
    static Deque<Integer> deque = new ArrayDeque<>();
    
    static void insert() {
        deque.addFirst(1);
        deque.addFirst(2);
        deque.addLast(3);
        deque.addLast(4);
    }
    
    public static void main(String[] args) {
        insert();
        while (!deque.isEmpty()) { // In 2 1 3 4
            System.out.println(deque.poll());
        }
        insert();
        while (!deque.isEmpty()) { // In 4 3 1 2
            System.out.println(deque.pollLast());
        }
    }
}
```

### Set

`Set` là cấu trúc dữ liệu đảm bảo phần tử không trùng.

#### HashSet

`Set` chèn ở vị trí ngẫu nhiên.

##### Khởi tạo

```java
Set<Integer> s1 = new HashSet<>();
```

#### LinkedHashSet

`Set` giữ thứ tự chèn.

##### Khởi tạo

```java
Set<Integer> s2 = new LinkedHashSet<>();
```

#### TreeSet

`Set` giữ phần tử có thứ tự, mặc định tăng dần.

##### Khởi tạo

```java
Set<Integer> s3 = new TreeSet<>();
Set<Integer> s4 = new TreeSet<>((x, y) -> {return y - x;});  // Giảm dần
```

##### Dùng TreeSet nâng cao

Các phương thức này do `TreeSet` cung cấp riêng, không thể gọi từ `Set`, nên cần khai báo:

```java
TreeSet<Integer> s3 = new TreeSet<>();
TreeSet<Integer> s4 = new TreeSet<>((x, y) -> {return y - x;});  // Giảm dần 
```

Dùng `this` để chỉ `TreeSet<Integer>` hiện tại:

|           Tên hàm          |                    Chức năng                    |
| :--------------------: | :--------------------------------------: |
|        `first()`       |       Trả về phần tử đầu; không có trả `null`       |
|        `last()`        |       Trả về phần tử cuối; không có trả `null`      |
|  `floor(Integer val)`  | Trả về phần tử lớn nhất ≤ `val`; không có trả `null` |
| `ceiling(Integer val)` | Trả về phần tử nhỏ nhất ≥ `val`; không có trả `null` |
|  `higher(Integer val)` |  Trả về phần tử nhỏ nhất > `val`; không có trả `null`  |
|  `lower(Integer val)`  |  Trả về phần tử lớn nhất < `val`; không có trả `null`  |
|      `pollFirst()`     |      Trả về và xóa phần tử đầu; không có trả `null`     |
|      `pollLast()`      |     Trả về và xóa phần tử cuối; không có trả `null`     |

Ví dụ:

```java
import java.util.TreeSet;

public class Main {
    static int[] a = {4,7,1,2,3,6};
    
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        for(int v:a) {
            set.add(v);
        }
        Integer a2 = set.first();
        System.out.println(a2); // Trả về 1
        Integer a3 = set.last();
        System.out.println(a3); // Trả về 7
        Integer a4 = set.floor(5);
        System.out.println(a4); // Trả về 4
        Integer a5 = set.ceiling(6);
        System.out.println(a5); // Trả về 6
        Integer a6 = set.higher(7);
        System.out.println(a6); // Trả về null
        Integer a7 = set.lower(2);
        System.out.println(a7); // Trả về 1
        Integer a8 = set.pollFirst();
        System.out.println(a8); // Trả về 1
        Integer a9 = set.pollLast();
        System.out.println(a9); // Trả về 7
    }
}
```

#### Phương thức thường dùng của Set

|            Tên hàm            |                   Chức năng                   |
| :-----------------------: | :------------------------------------: |
|          `size()`         |                Trả về kích thước hiện tại               |
|     `add(Integer val)`    |              Thêm `val` vào tập              |
|  `contains(Integer val)`  |            Kiểm tra tập có phần tử `val`            |
|   `addAll(Collection e)`  |          Thêm tất cả phần tử của `e` vào tập hiện tại         |
| `retainAll(Collection e)` | Xóa phần tử không nằm trong `e`, tức là lấy giao với `e` |
| `removeAll(Collection e)` |  Xóa phần tử nằm trong `e`, tức là lấy hiệu với `e` |

```java
import java.io.PrintWriter;
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Set<Integer> s1 = new HashSet<>();
    static Set<Integer> s2 = new LinkedHashSet<>();
    
    static void add() {
        s1.add(1);
    }
    
    static void contains() {  // Kiểm tra set có phần tử 2 không; có trả true, không trả false
        s1.contains(2);
    }
    
    static void test1() {  // Hợp của s1 và s2
        Set<Integer> res = new HashSet<>();
        res.addAll(s1);
        res.addAll(s2);
    }
    
    static void test2() {  // Giao của s1 và s2
        Set<Integer> res = new HashSet<>();
        res.addAll(s1);
        res.retainAll(s2);
    }
    
    static void test3() {  // Hiệu: s1 - s2
        Set<Integer> res = new HashSet<>();
        res.addAll(s1);
        res.removeAll(s2);
    }
}
```

#### Duyệt

```java
import java.io.PrintWriter;
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Set<Integer> s1 = new HashSet<>();
    static Set<Integer> s2 = new LinkedHashSet<>();
    
    static void test() {
        for (int key : s1) {
            out.println(key);
        }
        out.close();
    }
}
```

### Map

`Map` là cấu trúc dữ liệu lưu cặp khóa-giá trị `<Key, Value>` với `Key` là duy nhất.

#### HashMap

`Map` chèn ngẫu nhiên.

##### Khởi tạo

```java
Map<Integer, Integer> map1 = new HashMap<>();
```

#### LinkedHashMap

`Map` giữ thứ tự chèn.

##### Khởi tạo

```java
Map<Integer, Integer> map2 = new LinkedHashMap<>();
```

#### TreeMap

`Map` giữ `key` có thứ tự, mặc định tăng dần.

##### Khởi tạo

```java
Map<Integer, Integer> map3 = new TreeMap<>();
Map<Integer, Integer> map4 = new TreeMap<>((x, y) -> {return y - x;});  // Giảm dần
```

#### Phương thức thường dùng

Dùng `this` cho `Map<Integer, Integer>` hiện tại:

|                Tên hàm                |               Chức năng              |
| :-------------------------------: | :---------------------------: |
| `put(Integer key, Integer value)` |   Chèn `<key, value>` vào `this`  |
|              `size()`             |         Trả về kích thước `this`         |
|     `containsKey(Integer key)`    | Kiểm tra `this` có khóa `key` không |
|         `get(Integer key)`        |  Trả về giá trị ứng với khóa `key`  |
|             `keySet()`            |     Trả về tập các khóa của `this`    |

Ví dụ:

```java
import java.io.PrintWriter;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.TreeMap;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    
    static Map<Integer, Integer> map1 = new HashMap<>();
    static Map<Integer, Integer> map2 = new LinkedHashMap<>();
    static Map<Integer, Integer> map3 = new TreeMap<>();
    static Map<Integer, Integer> map4 = new TreeMap<>((x,y)->{return y-x;});
    
    static void put(){  // Chèn phần tử key=1, value=1
        map1.put(1, 1);
    }
    static void get(){  // Lấy value ứng với key=1
        map1.get(1);
    }
    static void containsKey(){  // Kiểm tra có cặp key=1 không
        map1.containsKey(1);
    }
    static void KeySet(){
        map1.keySet();
    }
}
```

#### Duyệt

```java
import java.io.PrintWriter;
import java.util.HashMap;
import java.util.Map;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    
    static Map<Integer, Integer> map1 = new HashMap<>();
    
    static void print() {
        for (int key : map1.keySet()) {
            out.println(key + " " + map1.get(key));
        }
    }
}
```

Kiểu của key và value cũng có thể đổi, ví dụ:

```java
Map<String, Set<Integer>> map = new HashMap<>();
```

## Arrays

`Arrays` là lớp công cụ trong `java.util` để thao tác mảng. Các phương thức là `static`.

### Arrays.sort()

`Arrays.sort()` sắp xếp mảng, các overload chính:

```java
import java.util.Arrays;
import java.util.Comparator;

public class Main {
    static int[] a = new int[10];
    static Integer[] b = new Integer[10];
    static int firstIdx, lastIdx;
    
    public static void main(String[] args) {
        Arrays.sort(a);  // 1
        Arrays.sort(a, firstIdx, lastIdx);  // 2
        Arrays.sort(b, new Comparator<Integer>() {  // 3
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        Arrays.sort(b, firstIdx, lastIdx, new Comparator<Integer>() {  // 4
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        // Từ Java 8 có Lambda, nên 3 và 4 có thể viết:
        Arrays.sort(b, (x, y) -> {  // 5
            return y - x;
        });
        Arrays.sort(b, (x, y) -> {  // 6
            return y - x;
        });
    }
}
```

Ý nghĩa các overload:

1.  Sắp xếp `a` tăng dần.
2.  Sắp xếp đoạn `[firstIdx, lastIdx)` của `a` tăng dần.
3.  Sắp xếp `a` theo comparator; với kiểu đối tượng mới dùng comparator.
4.  Sắp xếp đoạn `[firstIdx, lastIdx)` theo comparator.
5.  Tương tự 3, dùng lambda.
6.  Tương tự 4, dùng lambda.

???+ note "`Arrays.sort()` bên dưới dùng thuật toán"
    1.  Nếu phần tử là kiểu cơ bản (`byte`、`short`、`char`、`int`、`long`、`double`、`float`) thì dùng `DualPivotQuicksort`, trường hợp xấu nhất $O(n^2)$.
    2.  Nếu phần tử là kiểu đối tượng thì dùng `legacyMergeSort` và `TimSort`, độ phức tạp $O(n\log n)$.

Có thể kiểm chứng qua:

???+ example "[Codeforces 1646B - Quality vs Quantity](https://codeforces.com/problemset/problem/1646/B)"
    Có $n$ số nguyên, cần chia thành 2 nhóm sao cho tồn tại nhóm có số phần tử ít hơn nhóm kia nhưng tổng lớn hơn.

??? note "Code mẫu"
    ```java
    import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStreamReader;
    import java.io.PrintWriter;
    import java.util.Arrays;
    import java.util.StringTokenizer;
    
    public class Main {
        static class FastReader {
            StringTokenizer st;
            BufferedReader br;
            
            public FastReader() {
                br = new BufferedReader(new InputStreamReader(System.in));
            }
            
            String next() {
                while (st == null || !st.hasMoreElements()) {
                    try {
                        st = new StringTokenizer(br.readLine());
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                return st.nextToken();
            }
            
            int nextInt() {
                return Integer.parseInt(next());
            }
            
            long nextLong() {
                return Long.parseLong(next());
            }
            
            double nextDouble() {
                return Double.parseDouble(next());
            }
            
            String nextLine() {
                String str = "";
                try {
                    str = br.readLine();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return str;
            }
        }
        
        static PrintWriter out = new PrintWriter(System.out);
        static FastReader in = new FastReader();
        
        static void solve() {
            int n = in.nextInt();
            // Ở đây nếu đổi mảng Integer thành int sẽ TLE
            Integer[] a = new Integer[n + 10];
            for (int i = 1; i <= n; i++) {
                a[i] = in.nextInt();
            }
            Arrays.sort(a, 1, n + 1);
            long left = a[1];
            long right = 0;
            int x = n;
            for (int i = 2; i < x; i++, x--) {
                left = left + a[i];
                right = right + a[x];
                if (right > left) {
                    out.println("YES");
                    return;
                }
            }
            out.println("NO");
        }
        
        public static void main(String[] args) {
            int t = in.nextInt();
            while (t-- > 0) {
                solve();
            }
            out.close();
        }
    }
    ```

### Arrays.binarySearch()

`Arrays.binarySearch()` tìm nhị phân trên đoạn liên tiếp của mảng đã được sắp xếp, độ phức tạp $O(\log_n)$, overload chính:

```java
import java.util.Arrays;

public class Main {
    static int[] a = new int[10];
    static Integer[] b = new Integer[10];
    static int firstIdx, lastIdx;
    static int key;
    
    public static void main(String[] args) {
        Arrays.binarySearch(a, key);  // 1
        Arrays.binarySearch(a, firstIdx, lastIdx, key);  // 2
    }
}
```

Nguồn:

```java
private static int binarySearch0(int[] a, int fromIndex, int toIndex, int key) {
    int low = fromIndex;
    int high = toIndex - 1;
    
    while (low <= high) {
        int mid = (low + high) >>> 1;
        int midVal = a[mid];
        
        if (midVal < key)
            low = mid + 1;
        else if (midVal > key)
            high = mid - 1;
        else
            return mid; // key found
    }
    return -(low + 1);  // key not found.
}
```

Ý nghĩa:

1.  Tìm `key` trong `a`, nếu có trả về chỉ số, nếu không trả về số âm.
2.  Tìm `key` trong đoạn `[firstIdx,lastIdx)`, nếu có trả về chỉ số, nếu không trả về số âm.

### Arrays.fill()

`Arrays.fill()` gán một giá trị cho đoạn liên tiếp của mảng. Tham số là mảng, `fromIndex`, `toIndex` và giá trị cần điền. Sau khi chạy, các phần tử trong đoạn `[firstIdx,lastIdx)` đều nhận giá trị đó.

## Collections

`Collections` là lớp công cụ trong `java.util` thao tác trên collection, các phương thức là `static`.

### Collections.sort()

`Collections.sort()` chuyển tất cả phần tử thành mảng, gọi `Arrays.sort()`, rồi gán lại. Vì `Collection` chỉ chứa đối tượng nên luôn dùng merge sort.

Không thể sắp xếp theo đoạn.

Nguồn:

```java
default void sort(Comparator<? super E> c) {
    Object[] a = this.toArray();
    Arrays.sort(a, (Comparator) c);
    ListIterator<E> i = this.listIterator();
    for (Object e : a) {
        i.next();
        i.set((E) e);
    }
}
```

### Collections.binarySearch()

`Collections.binarySearch()` tìm nhị phân trên collection, tương tự `Arrays.binarySearch()`.

```java
Collections.binarySearch(list, key);
```

Không thể chỉ định đoạn.

### Collections.swap()

`Collections.swap()` hoán đổi phần tử ở hai vị trí.

```java
 Collections.swap(list, i, j);
```

## Khác

### So sánh số

Trong Java, nếu là kiểu số, `-0.0 = 0.0`. Nếu là kiểu đối tượng, `-0.0 != 0.0`. Khi dùng `Set` đếm số lượng hệ số góc, vấn đề này gây rắc rối. Cách giải là cộng `0.0` vào mọi hệ số góc trước khi đưa vào `Set`.

```java
import java.io.PrintWriter;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    
    static void A() {
        Double a = 0.0;
        Double b = -0.0;
        out.println(a.equals(b));  // false
    }
    
    static void B() {
        Double a = 0.0;
        Double b = -0.0 + 0.0;
        out.println(a.equals(b));  // true
    }
    
    static void C() {
        double a = 0.0;
        double b = -0.0;
        out.println(a == b);  // true
    }
    
    
    public static void main(String[] args) {
        A();
        B();
        C();
        out.close();
    }
}
```

## Tài liệu tham khảo

[^ref1]: [Input & Output - USACO Guide](https://usaco.guide/general/input-output?lang=java#method-3---io-template)
