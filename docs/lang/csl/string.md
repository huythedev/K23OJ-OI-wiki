author: johnvp22, Ir1d

## `string` là gì

`std::string` là một lớp trong thư viện chuẩn `<string>` (lưu ý không phải `<string.h>` của C) và về bản chất là bí danh của `std::basic_string<char>`.

## Vì sao nên dùng `string`

Trong C, thao tác chuỗi phải dùng mảng ký tự. `string` là một lớp đơn giản, dễ dùng, được dùng rộng rãi trong OI. So với các container STL khác, hằng số của `string` rất tốt, gần như tương đương mảng ký tự.

### `string` có thể cấp phát động

Giống nhiều container STL khác, `string` có thể cấp phát động, giúp ta dùng `std::cin` để nhập trực tiếp nhưng tốc độ cũng chậm hơn. Điều này cũng giúp ta không cần lo về bộ nhớ.

### `string` nạp chồng toán tử cộng và so sánh

Toán tử cộng của `string` có thể nối hai chuỗi hoặc một chuỗi với một ký tự. Tương tự `std::vector`, `string` nạp chồng các toán tử so sánh theo thứ tự từ điển, nên có thể dùng `std::sort` để sắp xếp chuỗi.

## Cách dùng

Giới thiệu các thao tác cơ bản, xem thêm [tài liệu C++](https://zh.cppreference.com/w/cpp/string/basic_string).

### Khai báo

```cpp
std::string s;
```

### Chuyển sang mảng char

Trong C có nhiều hàm xử lý chuỗi nhưng tham số là con trỏ char. Để tiện dùng, `string` có hai hàm thành viên chuyển thành con trỏ char — `data()`/`c_str()` (gần như giống nhau, nhưng nên dùng `c_str()` vì đảm bảo có ký tự kết thúc, còn `data()` thì không), ví dụ:

```cpp
printf("%s", s);          // Lỗi biên dịch
printf("%s", s.data());   // Biên dịch được nhưng là undefined behavior
printf("%s", s.c_str());  // Luôn in đúng
```

### Lấy độ dài

Nhiều hàm trả về độ dài của string:

```cpp
printf("Độ dài của s là %zu", s.size());
printf("Độ dài của s là %zu", s.length());
printf("Độ dài của s là %zu", strlen(s.c_str()));
```

???+ note "Độ phức tạp các hàm"
    `strlen()` có độ phức tạp tuyến tính theo độ dài chuỗi.
    
    `size()` và `length()` không được chỉ rõ trong C++98; từ C++11 là hằng số. Nhưng trên các compiler phổ biến, kể cả C++98, chúng cũng là hằng số.

???+ warning "Warning"
    Ba hàm này (và `find` bên dưới) trả về `size_t` (`unsigned long`). Vì vậy không thể so sánh/trừ trực tiếp với số âm; nên ép kiểu khi cần.

### Tìm vị trí xuất hiện đầu tiên của ký tự (chuỗi)

`find(str,pos)` dùng để tìm vị trí xuất hiện đầu tiên của ký tự/chuỗi trong chuỗi, bắt đầu từ `pos` (mặc định `0`). Nếu không có thì trả về `string::npos` (được định nghĩa là `-1` nhưng kiểu vẫn là `size_t`/`unsigned long`).

Ví dụ:

```cpp
string s = "OI Wiki", t = "OI", u = "i";
int pos = 5;
printf("Ký tự I xuất hiện lần đầu tại vị trí %lu\n", s.find('I'));
printf("Ký tự a xuất hiện lần đầu tại vị trí %lu\n", s.find('a'));
printf("Ký tự a xuất hiện lần đầu tại vị trí %d\n", s.find('a'));
printf("Chuỗi t xuất hiện lần đầu tại vị trí %lu\n", s.find(t));
printf("Trong s, từ vị trí pos, chuỗi u xuất hiện lần đầu tại %lu", s.find(u, pos));
```

Kết quả:

```text
Ký tự I xuất hiện lần đầu tại vị trí 1
Ký tự a xuất hiện lần đầu tại vị trí 18446744073709551615 // tức size_t(-1), phụ thuộc nền tảng.
Ký tự a xuất hiện lần đầu tại vị trí -1 // ép sang int thì in -1
Chuỗi t xuất hiện lần đầu tại vị trí 0
Trong s, từ vị trí pos, chuỗi u xuất hiện lần đầu tại 6
```

### Cắt chuỗi con

`substr(pos, len)` trả về chuỗi gồm tối đa `len` ký tự bắt đầu từ `pos` (nếu phần hậu tố từ `pos` ngắn hơn `len` thì lấy hết hậu tố).

Ví dụ:

```cpp
string s = "OI Wiki", t = "OI";
printf("Chuỗi con từ ký tự thứ 4 của s, tối đa 3 ký tự: %s\n",
       s.substr(3, 3).c_str());
printf("Chuỗi con từ ký tự thứ 2 của t, tối đa 3 ký tự: %s",
       t.substr(1, 3).c_str());
```

Kết quả:

```text
Chuỗi con từ ký tự thứ 4 của s, tối đa 3 ký tự: Wik
Chuỗi con từ ký tự thứ 2 của t, tối đa 3 ký tự: I
```

### Chèn/xóa ký tự (chuỗi)

`insert(index,count,ch)` và `insert(index,str)` là các hàm chèn thường dùng. Lần lượt là chèn `count` lần ký tự `ch` tại `index` và chèn chuỗi `str` tại `index`.

`erase(index,count)` xóa `count` ký tự bắt đầu từ `index` (nếu không truyền `count` thì xóa từ `index` đến hết).

Ví dụ:

```cpp
string s = "OI Wiki", t = " Wiki";
char u = '!';
s.erase(2);
printf("Xóa từ ký tự thứ 3 của s đến hết, được: %s\n", s.c_str());
s.insert(2, t);
printf("Chèn t vào vị trí thứ 3 của s, được: %s\n", s.c_str());
s.insert(7, 3, u);
printf("Chèn 3 lần ký tự u vào vị trí thứ 8 của s, được: %s",
       s.c_str());
```

Kết quả:

```text
Xóa từ ký tự thứ 3 của s đến hết, được: OI
Chèn t vào vị trí thứ 3 của s, được: OI Wiki
Chèn 3 lần ký tự u vào vị trí thứ 8 của s, được: OI Wiki!!!
```

### Thay thế ký tự (chuỗi)

`replace(pos,count,str)` và `replace(first,last,str)` là các hàm thay thế thường dùng. Lần lượt là thay thế `count` ký tự bắt đầu từ `pos` bằng `str`, hoặc thay thế đoạn từ `first` (gồm) đến `last` (không gồm) bằng `str`, trong đó `first` và `last` là iterator.

Ví dụ:

```cpp
string s = "OI Wiki";
s.replace(2, 5, "");
printf("Thay ký tự 3~7 của s bằng chuỗi rỗng, được: %s\n", s.c_str());
s.replace(s.begin(), s.begin() + 2, "NOI");
printf("Thay 2 ký tự đầu của s thành NOI, được: %s", s.c_str());
```

Kết quả:

```text
Thay ký tự 3~7 của s bằng chuỗi rỗng, được: OI
Thay 2 ký tự đầu của s thành NOI, được: NOI
```
