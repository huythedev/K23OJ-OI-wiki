author: Ir1d, cqnuljs, akakw1, MingqiHuang, Chrogeek, henrytbtrue, Planet6174, StudyingFather

## Khái niệm tệp

Tệp là tập hợp dữ liệu được thu thập theo mục đích nhất định. C/C++ coi mỗi tệp là một luồng byte có thứ tự, mỗi tệp kết thúc bằng **dấu kết thúc tệp** (EOF). Khi thao tác với tệp, chương trình cần mở tệp trước; mỗi khi tệp được mở (nhớ đóng sau khi dùng), tệp sẽ gắn với một luồng; luồng thực chất là một dãy byte.

C/C++ chia tệp thành tệp văn bản và tệp nhị phân. Tệp văn bản là văn bản thông thường (trọng tâm), còn tệp nhị phân là tệp định dạng đặc biệt hoặc tệp thực thi.

## Các bước thao tác tệp

1、Mở tệp, đưa con trỏ tệp trỏ vào tệp, quyết định chế độ mở;  
2、Đọc/ghi tệp (trong thi đấu chủ yếu dùng, thao tác khác tạm không đề cập);  
3、Dùng xong thì đóng tệp.

## Hàm `freopen`

### Giới thiệu

Hàm dùng để chuyển hướng luồng vào/ra tiêu chuẩn đến tệp theo chế độ chỉ định, nằm trong `stdio.h (cstdio)`. Hàm này cho phép đổi môi trường I/O mà không cần sửa code. Khi dùng phải đảm bảo luồng đáng tin cậy.

Có ba chế độ chính: đọc, ghi, thêm.

### Cú pháp

```cpp
FILE* freopen(const char* filename, const char* mode, FILE* stream);
```

### Tham số

-   `filename`: tên tệp cần mở
-   `mode`: chế độ mở, xác định quyền truy cập
-   `stream`: con trỏ tệp, thường là luồng chuẩn (`stdin/stdout`) hoặc lỗi (`stderr`)
-   Giá trị trả về: con trỏ tệp trỏ tới tệp đã mở

### Chế độ mở (tùy đọc)

-   `r`: mở chỉ đọc, tệp phải tồn tại, chỉ đọc **(phổ biến)**
-   `r+`: đọc/ghi, tệp phải tồn tại
-   `rb`: chỉ đọc nhị phân, tệp phải tồn tại
-   `rb+`: đọc/ghi nhị phân, tệp phải tồn tại
-   `rt+`: đọc/ghi văn bản
-   `w`: chỉ ghi, tệp không tồn tại sẽ tạo mới, nếu tồn tại sẽ xóa nội dung **(phổ biến)**
-   `w+`: đọc/ghi, tệp không tồn tại tạo mới, nếu tồn tại xóa nội dung
-   `wb`: chỉ ghi nhị phân, tệp không tồn tại tạo mới, nếu tồn tại xóa nội dung
-   `wb+`: đọc/ghi nhị phân, tệp không tồn tại tạo mới, nếu tồn tại xóa nội dung
-   `a`: chỉ ghi, tệp không tồn tại tạo mới, dữ liệu ghi sẽ thêm vào cuối (giữ EOF)
-   `a+`: đọc/ghi, tệp không tồn tại tạo mới, dữ liệu ghi thêm cuối (không giữ EOF)
-   `at+`: đọc/ghi văn bản, dữ liệu ghi thêm cuối
-   `ab+`: đọc/ghi nhị phân, dữ liệu ghi thêm cuối

### Cách dùng

Đọc từ tệp:

```cpp
freopen("data.in", "r", stdin);
// data.in là tên tệp đọc, đặt cùng thư mục với file thực thi
```

Ghi ra tệp:

```cpp
freopen("data.out", "w", stdout);
// data.out là tên tệp ghi, đặt cùng thư mục với file thực thi
```

Đóng luồng chuẩn:

```cpp
fclose(stdin);
fclose(stdout);
```

??? note "Ghi chú"
    `printf/scanf/cin/cout` mặc định dùng `stdin/stdout`; sau khi chuyển hướng, các hàm này sẽ đọc/ghi vào tệp đã chuyển hướng.

### Mẫu

```cpp
#include <cstdio>
#include <iostream>

int main(void) {
  freopen("data.in", "r", stdin);
  freopen("data.out", "w", stdout);
  /*
  Phần code giữa không cần đổi, chỉ cần dùng cin và cout
  */
  fclose(stdin);
  fclose(stdout);
  return 0;
}
```

## Hàm `fopen` (tùy đọc)

Hàm gần giống `freopen`, mở tệp và trả về con trỏ tệp

### Nguyên mẫu

```cpp
FILE* fopen(const char* path, const char* mode)
```

Ý nghĩa tham số như `freopen`.

### Hàm đọc/ghi cơ bản

-   `fread/fwrite`
-   `fgetc/fputc`
-   `fscanf/fprintf`
-   `fgets/fputs`

### Cách dùng

```cpp
FILE *in, *out;  // khai báo con trỏ tệp
in = fopen("data.in", "r");
out = fopen("data.out", "w");
/*
làm những gì cần làm
*/
fclose(in);
fclose(out);
```

## Luồng vào/ra tệp C++ `ifstream/ofstream`

### Cách dùng

Đọc tệp:

```cpp
ifstream fin("data.in");
// data.in là đường dẫn tương đối hoặc tuyệt đối
```

Ghi tệp:

```cpp
ofstream fout("data.out");
// data.out là đường dẫn tương đối hoặc tuyệt đối
```

Đóng luồng:

```cpp
fin.close();
fout.close();
```

### Mẫu

```cpp
#include <fstream>
using namespace std;  // cả hai kiểu đều trong std

ifstream fin("data.in");
ofstream fout("data.out");

int main(void) {
  /*
  Ở giữa chỉ cần đổi cin thành fin, cout thành fout
  */
  fin.close();
  fout.close();
  return 0;
}
```

## Tài liệu tham khảo

1.  信息学奥赛一本通
