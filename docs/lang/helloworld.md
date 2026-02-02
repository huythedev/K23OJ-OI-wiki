disqus:

## Cấu hình môi trường

Có công cụ tốt thì việc mới tốt.

### Môi trường phát triển tích hợp

IDE dễ dùng, người mới thường chọn IDE để viết code. Trong thi đấu, phổ biến nhất là [Dev-C++](../tools/editor/devcpp.md) (nếu môi trường thi là Windows, thường sẽ có IDE này).

### Trình biên dịch

#### Windows

Khuyến nghị dùng GNU compiler. Cần tải MinGW ở [MinGW Distro](https://nuwen.net/mingw.html) và cài. Ngoài ra Windows cũng có thể dùng [Microsoft Visual C++ Compiler](https://docs.microsoft.com/en-us/cpp/build/projects-and-build-systems-cpp), cần tải ở [trang Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019).

#### macOS

Chạy trong terminal:

```bash
xcode-select --install
```

#### Linux

Dùng `g++ -v` để kiểm tra đã cài `g++` chưa.

Cài bằng:

```bash
sudo apt update && sudo apt install g++
```

#### Biên dịch bằng dòng lệnh

Khi quen, một số người dùng dòng lệnh để linh hoạt hơn, không phụ thuộc IDE, dùng editor mình thích.

```bash
g++ test.cpp -o test -lm
```

`g++` là trình biên dịch C++ (C là `gcc`), `-o` chỉ định tên file thực thi, `-lm` liên kết thư viện toán `libm` để code dùng `math.h` chạy được.

Lưu ý: Chương trình C++ không cần `-lm` vẫn chạy. Nhưng nhiều năm NOI/NOIP dùng tùy chọn có `-lm`, nên ở đây cũng thêm.

## Đoạn code đầu tiên

Bắt đầu hành trình C++ bằng ví dụ này nhé~

Lưu ý: trước khi viết nhớ bật bàn phím tiếng Anh.

C++:

```cpp
#include <iostream>  // include header

int main() {                     // định nghĩa hàm main
  std::cout << "Hello, world!";  // dùng cout trong không gian tên chuẩn
  return 0;  // trả về 0, kết thúc main; compiler thường tự thêm nên có thể bỏ
}
```

C:

```c
#include <stdio.h>  // include header

int main() {                // định nghĩa hàm main
  printf("Hello, world!");  // in Hello, world!
  return 0;                 // trả về 0, kết thúc main
}
```

Lưu ý: C ở đây chỉ để tham khảo. C++ tương thích với C và có nhiều tính năng mới giúp thi đấu hiệu quả hơn. Xem [C++ và các ngôn ngữ khác](./cpp-other-langs.md).
