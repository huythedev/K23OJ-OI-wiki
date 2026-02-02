Mảng là một контей (container) lưu các đối tượng cùng kiểu, các đối tượng trong mảng không có tên, mà được truy cập theo vị trí của chúng. Kích thước mảng là cố định, không thể tùy ý thay đổi độ dài mảng.

## Định nghĩa mảng

Khai báo mảng có dạng `a[d]`, trong đó `a` là tên mảng, `d` là số phần tử của mảng. Khi biên dịch, `d` phải là một hằng biểu thức nguyên đã biết.

```cpp
unsigned int d1 = 42;
const int d2 = 42;
int arr1[d1];  // Sai: d1 không phải là hằng biểu thức
int arr2[d2];  // Đúng: arr2 là mảng có độ dài 42
```

Không thể gán trực tiếp một mảng cho một mảng khác:

```cpp
int arr1[3];
int arr2 = arr1;  // Sai
arr2 = arr1;      // Sai
```

Nên khai báo các mảng lớn ở phạm vi toàn cục. Vì biến cục bộ được tạo trên vùng stack, mảng quá lớn (lớn hơn kích thước stack) sẽ gây tràn stack, dẫn đến RE. Nếu khai báo mảng ở phạm vi toàn cục, mảng sẽ được tạo ở vùng tĩnh.

## Truy cập phần tử mảng

Có thể dùng toán tử chỉ số `[]` để truy cập phần tử trong mảng. Chỉ số mảng bắt đầu từ 0. Ví dụ, một mảng có 10 phần tử sẽ có chỉ số từ 0 đến 9, chứ không phải 1 đến 10. Nhưng trong OI, để thuận tiện, ta thường khai báo mảng lớn hơn một chút và không dùng phần tử đầu tiên, bắt đầu truy cập từ chỉ số 1.

Ví dụ 1: Đọc một số nguyên $n$ từ đầu vào chuẩn, sau đó đọc $n$ số, lưu vào mảng. Với $n\leq 1000$.

```cpp
#include <iostream>
using namespace std;

int arr[1001];  // Chỉ số của mảng arr nằm trong [0, 1001)

int main() {
  int n;
  cin >> n;
  for (int i = 1; i <= n; ++i) {
    cin >> arr[i];
  }
}
```

Ví dụ 2: (tiếp ví dụ 1) Tính tổng các phần tử trong mảng `arr` và in ra tổng. Giả sử tổng tất cả phần tử không vượt quá $2^{31} - 1$

```cpp
#include <iostream>
using namespace std;

int arr[1001];

int main() {
  int n;
  cin >> n;
  for (int i = 1; i <= n; ++i) {
    cin >> arr[i];
  }

  int sum = 0;
  for (int i = 1; i <= n; ++i) {
    sum += arr[i];
  }

  printf("%d\n", sum);
  return 0;
}
```

### Truy cập vượt chỉ số

Chỉ số mảng $\mathit{idx}$ phải thỏa $0\leq \mathit{idx}< \mathit{size}$. Nếu chỉ số nằm ngoài phạm vi này thì là hành vi không xác định, có thể gây hậu quả khó lường như lỗi phân đoạn (Segmentation Fault), hoặc sửa đổi các biến ngoài dự kiến, v.v.

## Mảng nhiều chiều

Bản chất của mảng nhiều chiều là “mảng của mảng”, tức phần tử của mảng ngoài là các mảng. Một mảng hai chiều cần hai kích thước để định nghĩa: độ dài mảng và độ dài phần tử trong mảng. Truy cập mảng hai chiều cần viết ra hai chỉ số:

```cpp
int arr[3][4];  // Một mảng độ dài 3, mỗi phần tử là “mảng độ dài 4 kiểu int”
arr[2][1] = 1;  // Truy cập mảng hai chiều
```

Ta thường dùng vòng lặp for lồng nhau để xử lý mảng hai chiều.

Ví dụ: Đọc hai số $n$ và $m$ từ đầu vào chuẩn, lần lượt là chiều cao và chiều rộng của ảnh đen trắng, với $n,m\leq 1000$. Trong $n$ dòng tiếp theo, mỗi dòng có $m$ số cách nhau bởi dấu cách, biểu diễn độ sáng tại vị trí đó. Ta đọc ảnh này và lưu vào mảng hai chiều.

```cpp
const int MAXN = 1001;
int pic[MAXN][MAXN];
int n, m;

cin >> n >> m;
for (int i = 1; i <= n; ++i)
  for (int j = 1; j <= m; ++j) cin >> pic[i][j];
```

Tương tự, bạn có thể định nghĩa mảng ba chiều, bốn chiều và cao hơn nữa.
