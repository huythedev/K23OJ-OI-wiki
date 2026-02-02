author: cmpute, Henry-ZHR, ranwen, abc1763613206, billchenchina, chinggg, ChungZH, CoelacanthusHex, countercurrent-time, Dong Tsing-hsuen, Early0v0, Enter-tainer, F1shAndCat, Great-designer, hensier, HeRaNO, Hszzzx, imba-tjd, Ir1d, ksyx, lingxier, LovelyBuggies, Marcythm, mgt, Mooos-MoSheng, NachtgeistW, ouuan, Rottenwooood, shawlleyw, shuzhouliu, sshwy, SukkaW, Suyun514, Tiphereth-A, tLLWtG, wineee, wxh06, Xeonacid, yusancky, zyouxam, zzjjbb, jiangmuran, CuriosityQiu

## Về Python

Python là một ngôn ngữ thông dịch được sử dụng rộng rãi trên thế giới. Nó cung cấp các cấu trúc dữ liệu bậc cao hiệu quả, hỗ trợ lập trình hướng đối tượng đơn giản, và cũng có thể dùng trong thi đấu thuật toán.

### Ưu điểm của Python

-   Python là ngôn ngữ **thông dịch**: không cần biên dịch và liên kết, giảm thao tác.
-   Python là ngôn ngữ **tương tác**: trình thông dịch hỗ trợ tương tác, có thể nhập lệnh trực tiếp trong terminal.
-   Python **dễ học, dễ dùng**: cung cấp nhiều cấu trúc dữ liệu, hỗ trợ phát triển chương trình lớn.
-   Python **tương thích mạnh**: hỗ trợ Windows, macOS, Unix.
-   Python **tính thực tiễn cao**: từ nhập/xuất đơn giản đến tính toán khoa học hay WEB ứng dụng lớn đều có thể viết.
-   Python **mã ngắn, dễ đọc**: thường ngắn hơn các ngôn ngữ khác cho cùng chức năng.
-   Python **hỗ trợ mở rộng**: CPython viết bằng C cho phép liên kết ứng dụng C, dùng Python mở rộng/điều khiển ứng dụng đó.

### Lưu ý khi học Python

-   Hiện chủ yếu dùng Python 3.7+; Python 2 và Python 3.6 trở xuống đã [không còn hỗ trợ](https://devguide.python.org/versions/#unsupported-versions), nhưng một số hệ thống cũ vẫn dùng. Bài này **giới thiệu Python bản mới**. Nếu gặp mã Python 2 có thể dùng [`2to3`](https://docs.python.org/zh-cn/3/library/2to3.html).
-   Triết lý thiết kế và cú pháp **khác nhiều** so với các ngôn ngữ khác, ẩn nhiều chi tiết thấp, nên phong cách thực dụng và thanh lịch.
-   Python là ngôn ngữ thông dịch động, nên **chạy chậm hơn**, đặc biệt với `for` loop. Nên dùng `filter`, `map` hoặc [list comprehension](https://www.pythonforbeginners.com/basics/list-comprehensions-in-python) để tăng hiệu năng.

## Thiết lập môi trường

Xem [Python 3](../tools/compiler.md#python-3). Hoặc:

-   Windows: có thể lấy Python miễn phí nhanh qua Microsoft Store.
-   macOS/Linux: nhiều bản phân phối Linux có sẵn Python; nếu chỉ học cú pháp, không cần cài thêm.

    ???+ warning "Lưu ý"
        Trên một số hệ thống cài Python mặc định (như Unix), hãy chạy `python3` để mở Python 3. [^ref1]

Ngoài ra có thể dùng venv, conda, Nix... để quản lý toolchain và package, tạo môi trường ảo cách ly.

Python là ngôn ngữ thông dịch nên cách chạy khác C++. Khi dùng IDE có thể không thấy rõ, nên cần nhấn mạnh cách chạy.

Khi gõ `python3` hoặc mở IDLE, bạn vào môi trường tương tác (REPL) — “Read-Eval-Print Loop”. Có thể nhập lệnh và thấy kết quả ngay, rất tiện để kiểm chứng cú pháp; phần sau sẽ dùng nhiều dạng này.

Nhưng nếu viết chương trình hoàn chỉnh, nên tạo file `.py` và chạy `python3 filename.py`.

### Một số phiên bản Python theo nền tảng

| Hệ thống/phiên bản         | Phiên bản python         |
| ------------------ | ----------------------- |
| Noi Linux 2.0      | 3.8.0, gồm requests      |
| Luogu              | 3.11.5, NumPy 1.25.2     |
| OJ dựa trên Hydro  | 3.8.0+ gồm NumPy         |
| Ubuntu 22.04 (sẵn) | 3.10.4                  |
| Microsoft Store    | Bản ổn định mới nhất      |

???+ warning "Lưu ý"
    Bảng có hiệu lực tại thời điểm viết (2025/01/15). Nên kiểm tra lại trên nền tảng.

Trong nước, mirror **source** phổ biến là [北京交通大学自由与开源软件镜像站](https://mirror.bjtu.edu.cn/python/) và [华为开源镜像站](https://repo.huaweicloud.com/python/), có thể tải ở đó.

## Cài thư viện qua `pip`

Sức sống của Python đến từ thư viện bên thứ ba. `pip` là công cụ mặc định từ Python 3.4 để cài thư viện.

Thư viện lưu trên [PyPI](https://pypi.org/), cũng có thể chỉ định kho khác. Xem hướng dẫn như [TUNA PyPI mirror](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/). Xem thêm mirror tại [MirrorZ](https://mirrorz.org/list/pypi).

???+ info "Cài gói qua mirror TUNA"
    ```sh
    pip install -i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple <some-package>
    ```

## Cú pháp cơ bản

Python cú pháp ngắn gọn; có nhiều tài liệu. Ở đây chỉ giới thiệu tính năng hữu ích cho OIer. Xem thêm [Python Docs](https://docs.python.org/zh-cn/3/) và [Python Wiki](https://wiki.python.org/moin/).

### Chú thích

Chú thích không ảnh hưởng chạy, nhưng giúp dễ hiểu.

```python
# Dòng bắt đầu bằng # là chú thích một dòng

"""
Chuỗi nhiều dòng dùng ba dấu nháy
(ba nháy đơn hoặc ba nháy kép)
thường cũng dùng làm chú thích
"""
```

Khuyến khích thêm chú thích để dễ đọc.

### Kiểu dữ liệu cơ bản

#### Mọi thứ đều là đối tượng

Trong Python không cần khai báo kiểu trước, gán trực tiếp:

```pycon
>>> x = -3  # Không cần dấu ; ở cuối
>>> x
-3
>>> f = 3.1415926535897932384626; f  # Muốn có dấu ; cũng được, đỡ một dòng
3.141592653589793
>>> s1 = "O"
>>> s1  # Nháy đơn và nháy kép như nhau
'O'
>>> b = 'A' == 65  # 'A' và 65 khác kiểu nên không bằng nhau
>>> b  # True/False viết hoa chữ cái đầu
False
>>> True + 1 == 2 and not False != 0  # Dùng từ khóa nhưng cũng hỗ trợ ký hiệu
True
```

Nhưng điều đó không có nghĩa Python không có kiểu: interpreter tự suy luận. Dùng `type()` để xem:

```pycon
>>> type(x)
<class 'int'>
>>> type(f)
<class 'float'>
>>> type(s1)  # Đừng đặt tên biến là str, sẽ che mất str
<class 'str'>
>>> type(b)
<class 'bool'>
```

???+ note "[**Hàm built-in**](https://docs.python.org/zh-cn/3/library/functions.html) là gì?"
    Trong C/C++ các hàm nằm rải ở nhiều header. Python tích hợp nhiều hàm tiện dụng; bạn dùng trực tiếp mà không cần biết chúng ở đâu. Nhược điểm là tên trùng từ thông dụng, nên tránh đặt biến trùng tên built-in.

Python có int, float, str, bool tương tự `int`, `float`, `string`, `bool` trong C++. Nhưng khác ở chỗ: không có `char`, không có `double` (float thực tế là double của C). Nếu cần chính xác hơn dùng [decimal](https://docs.python.org/zh-cn/3/library/decimal.html); nếu cần số phức có `complex` (đừng đặt tên biến là `complex`).
Các kiểu đều là `class`; khác biệt lớn với C++ là mọi dữ liệu đều là đối tượng, hàm là đối tượng, kiểu cũng là đối tượng:

```pycon
>>> type(int)
<class 'type'>
>>> type(pow)  # Hàm built-in, sẽ nói sau
<class 'builtin_function_or_method'>
>>> type(type)  # type() cũng là built-in nhưng đặc biệt
<class 'type'>
```

Khái niệm này có thể khó lúc đầu; về sau bạn sẽ thấy Python ưu tiên thao tác theo đối tượng, khiến mã ngắn gọn rõ ràng.

#### Tính toán số

Python có thể coi như máy tính đa năng.  
Trong REPL, nhập biểu thức sau `>>>` và dùng `+ - * / %` như C++ và `()` để nhóm. Dưới đây là khác biệt chính:

```pycon
>>> 5.0 * 6  # Kết quả phép nhân có float nếu có float
30.0
>>> 15 / 3  # Khác C/C++: chia luôn trả float
5.0
>>> 5 / 100000  # Số lớn hiển thị dạng khoa học
5e-05
>>> 5 // 3 # Chia nguyên (floor), trả int
1
>>> -5 // 3 # Làm tròn xuống, khác C/C++
-2
>>> 5 % 3 # Lấy dư
2
>>> -5 % 3 # Dư luôn không âm, khác C/C++, nhưng thỏa (a//b)*b+(a%b)==a 
1
>>> x = abs(-1e4)  # Hàm trị tuyệt đối
>>> x += 1  # Không có ++/--
>>> x
10001.0
```

`/` luôn trả float; muốn chia nguyên dùng `//`. `%` lấy dư. Dạng khoa học giống C++.

Python dùng `**` để lũy thừa, và có `pow(a, b, mod)` cho [lũy thừa nhanh](../math/binary-exponentiation.md).

```pycon
>>> 3 ** 4 # Lũy thừa
81
>>> 2 ** 512
13407807929942597099574024998205846127479365820592393377723561443721764030073546976801874298166903427690031858186486050853753882811946569946433649006084096
>>> pow(2, 512, int(1e4)) # 2**512 % 10000 nhanh, 1e4 là float nên cần int
4096
>>> 2048 ** 2048 # Thử số lớn trong IDLE?
>>> 0.1 + 0.1 + 0.1 - 0.3 == 0.  # Như C/C++, không so sánh float trực tiếp
False
```

#### Kiểm tra kiểu

Dùng `type(obj)` để lấy kiểu, như `type(8)` và `type('a')`.

#### [I/O cơ bản](https://docs.python.org/zh-cn/3/tutorial/inputoutput.html)

I/O chủ yếu qua `input()` và `print()`. `print()` rất trực quan:

```pycon
>>> a = [1,2,3]; print(a[-1])  # Mặc định xuống dòng
3
>>> print(ans[0], ans[1])  # In nhiều biến, ngăn cách bằng khoảng trắng
1 2
>>> print(a[0], a[1], end='')  # end='' để không xuống dòng
1 2>>>
>>> print(a[0], a[1], sep=', ')  # sep=', ' đổi cách ngăn
1, 2
>>> print(str(a[0]) + ', ' + str(a[1]))  # Nối chuỗi thủ công
```

`input()` giống `getline()` của C++: đọc cả dòng thành chuỗi, không có newline cuối.

```pycon
>>> s = input('请输入一串数字: '); s  # Tự debug có thể truyền prompt
请输入一串数字: 1 2 3 4 5 6
'1 2 3 4 5 6'
```

#### Chuỗi

Python 3 có chuỗi Unicode mạnh mẽ, giống `string` C++, hỗ trợ nối `+`, truy cập index, nhân `*`, và `in`.

```pycon
>>> s1 = "O"  # Nháy đơn/đôi đều được
>>> s1 += 'I-Wiki'  # Khuyến nghị dùng nháy đôi để đồng bộ C++
>>> 'OI' in s1  # Kiểm tra substring
True
>>> len(s1)  # Tương tự s.length() trong C++
7
>>> s2 = """ 感谢你的阅读
... 欢迎参与贡献!
"""   # Chuỗi ba nháy có thể nhiều dòng
>>> s1 + s2 
'OI-Wiki 感谢你的阅读\n欢迎参与贡献!'
>>> print(s1 + s2)  # In chuỗi
OI-Wiki 感谢你的阅读
欢迎参与贡献!
>>> s2[2] * 2 + s2[3] + s2[-1]  # Index âm từ phải, tương đương modulo
'谢谢你!'
>>> s1[0] = 'o'  # str là bất biến, không sửa trực tiếp
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

Python có nhiều kiểu phức hợp; phổ biến nhất là `list`. `[1,2,3]` và `['a','b','c']` đều là list.

Chuỗi hỗ trợ *slice* `s[l:r:step]`:

```pycon
>>> s = 'OI-Wiki 感谢你的阅读\n欢迎参与贡献!'
>>> s[:8]
'OI-Wiki '
>>> s[8:14]
'感谢你的阅读'
>>> s[-4:]
'与贡献!'
>>> s[8:14:2]
'感你阅'
>>> s[::-1]
'!献贡与参迎欢\n读阅的你谢感 ikiW-IO'
>>> s
'OI-Wiki 感谢你的阅读\n欢迎参与贡献!'
```

Chuỗi là Unicode, đa ngôn ngữ. Dùng `ord()` lấy mã Unicode, `chr()` ngược lại. C/C++ `char` tương tự ASCII.

Đổi số thành chuỗi dùng `str()`, ngược lại `int()`, `float()` (giống ép kiểu nhưng là hàm).

Chuỗi có nhiều method mạnh; nên xem [tài liệu](https://docs.python.org/zh-cn/3/library/stdtypes.html#text-sequence-type-str).

### Tạo “mảng”

Trong Python, “mảng” là các [sequence](https://docs.python.org/zh-cn/3/library/stdtypes.html#iterator-types), gần với `vector` hơn là mảng C.

#### Dùng `list`

`list` là kiểu sequence mạnh nhất, có thể chứa mọi kiểu, kể cả list lồng nhau; đừng nhầm với `list` trong STL. Vì vậy dùng chữ “danh sách”.

```pycon
>>> []  # Danh sách rỗng
[]
>>> nums = [0, 1, 2, 3, 5, 8, 13]; nums
[0, 1, 2, 3, 5, 8, 13]
>>> nums[0] = 1; nums
[1, 1, 2, 3, 5, 8, 13]
>>> nums.append(nums[-2]+nums[-1]); nums  # append() ~ push_back()
[1, 1, 2, 3, 5, 8, 13, 21]
>>> nums.pop()
21
>>> nums.insert(0, 1); nums
[1, 1, 1, 2, 3, 5, 8, 13]
>>> nums.remove(1); nums
[1, 1, 2, 3, 5, 8, 13]
>>> len(nums)
7
>>> nums.reverse(); nums
[13, 8, 5, 3, 2, 1, 1]
>>> sorted(nums)
[1, 1, 2, 3, 5, 8, 13]
>>> nums
[13, 8, 5, 3, 2, 1, 1]
>>> nums.sort(); nums
[1, 1, 2, 3, 5, 8, 13]
>>> nums.count(1)
2
>>> nums.index(1)
0
>>> nums.clear(); nums
```

List có nhiều thao tác tương tự `vector`, nhưng một số là built-in (`len`, `sorted`), còn một số là method. Xem [list docs](https://docs.python.org/zh-cn/3/tutorial/datastructures.html#more-on-lists).

Python có nhiều kiểu phức hợp; list là phổ biến nhất.

```pycon
>>> lst = [1, '1'] + ["2", 3.0]
>>> lst
[1, '1', '2', 3.0]
>>> 3 in lst
True
>>> [1, '1'] in lst
False
>>> lst[1:3] = [2, 3]; lst
[1, 2, 3, 3.0]
>>> lst[::-1]
[3.0, 3, 2, 1]
>>> lst *= 2; lst
[1, 2, 3, 3.0, 1, 2, 3, 3.0]
>>> del lst[4:]; lst
[1, 2, 3, 3.0]
```

List là mutable, khác string là immutable. Có thể dùng [list comprehension](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) để chuyển đổi:

```pycon
>>> # Tạo mảng số nguyên [65, 70), range là kiểu dãy, mặc định bước 1
>>> nums = list(range(65,70))
[65, 66, 67, 68, 69]
>>> lst = [chr(x) for x in nums]
>>> lst
['A', 'B', 'C', 'D', 'E']
>>> s = ''.join(lst); s
'ABCDE'
>> list(s)
['A', 'B', 'C', 'D', 'E']
>>> # Nếu không biết s.lower() có thể viết kiểu này
>>> ''.join([chr(ord(ch) - 65 + 97) for ch in s if ch >= 'A' and ch <= 'Z'])  
'abcde'
```

Ví dụ mảng 2D:

```pycon
>>> vis = [[0] * 3] * 3  # Tạo mảng 3x3 toàn 0
>>> vis 
[[0, 0, 0], [0, 0, 0], [0, 0, 0]]
>>> vis[0][0] = 1; vis  # Vì sao cả các hàng khác bị đổi?
[[1, 0, 0], [1, 0, 0], [1, 0, 0]]
>>> # Xem gán list 1D
>>> a1 = [0, 0, 0]; a2 = a1; a3 = a1[:]
>>> a1[0] = 1; a1
[1, 0, 0]
>>> a2
[1, 0, 0]
>>> a3
[0, 0, 0]
>>> id(a1) == id(a2) and id(a1) != id(a3)
True
>>> vis2 = vis[:]
>>> vis[0][1] = 2; vis
>>> [[1, 2, 0], [1, 2, 0], [1, 2, 0]]
>>> vis2
>>> [[1, 2, 0], [1, 2, 0], [1, 2, 0]]
>>> id(vis) != id(vis2)
True
>>> [id(vis[i]) == id(vis2[i]) for i in range(3)]
[True, True, True]
>>> [id(x) for x in vis]
[139760373248192, 139760373248192, 139760373248192]
```

Trong Python, gán chỉ truyền tham chiếu. Với kiểu immutable như số/chuỗi, gán tạo đối tượng mới; với list (mutable) thì nhiều biến có thể trỏ cùng đối tượng. Vì vậy `[[0]*3]*3` lặp lại cùng một list. Khi copy bằng slice cũng chỉ “shallow copy”, cần `deepcopy` nếu muốn sâu. Cách tạo 2D đúng là dùng list comprehension:

```pycon
>>> vis1 = [[0] * 3 for _ in range(3)]  # Dùng _ làm biến đếm
>>> # Trong REPL, _ mặc định là kết quả trước, có thể dùng __
>>> vis1
[[0, 0, 0], [0, 0, 0], [0, 0, 0]]
>>> [id(x) for x in vis1]
[139685508981248, 139685508981568, 139685508981184]
>>> vis1[0][0] = 1
[[1, 0, 0], [0, 0, 0], [0, 0, 0]]
>>> a2[0][0] = 10  # Truy cập/gán mảng 2D
```

Chúng ta chưa học vòng lặp nhưng đã dùng comprehension vì Python chạy chậm với vòng lặp **for**. Nếu cần hiệu năng, ưu tiên comprehension hoặc `filter`, `map`, nhưng tùy bài.

#### Dùng NumPy

??? note "NumPy là gì"
    [NumPy](https://numpy.org/) là thư viện tính toán khoa học, hỗ trợ mảng/matrix hiệu năng cao. Có thể dùng để thử nghiệm thuật toán. Kiểu dữ liệu lõi là `ndarray`, mảng n chiều, lưu liên tục, độ dài cố định. NumPy viết bằng C nên nhanh. Không phải chuẩn, cần `pip install numpy`, và OI không đảm bảo có (xem [phiên bản Python](#一些平台提供的-python-版本)).

Ví dụ tạo mảng:

```pycon
>>> import numpy as np  # Tự tìm hiểu import
>>> np.empty(3) # Mảng trống dung lượng 3, chưa init
array([0.00000000e+000, 0.00000000e+000, 2.01191014e+180])
>>> np.zeros((3, 3)) # Mảng 3x3 toàn 0
array([[0., 0., 0.],
       [0., 0., 0.],
       [0., 0., 0.]])
>>> a1 = np.zeros((3, 3), dtype=int) # Mảng int 3x3
>>> a1[0][0] = 1 # Truy cập/gán
>>> a1[0, 0] = 1 # Cú pháp gọn hơn
>>> a1.shape # Kích thước
(3, 3)

>>> a1[:2, :2] # Lấy 2 hàng/2 cột đầu, không copy
array([[1, 0],
       [0, 0]])

>>> a1[:, [0, 2]] # Lấy cột 1 và 3, không copy
array([[1, 0],
       [0, 0],
       [0, 0]])
>>> np.max(a1) # Max
1
>>> a1.flatten() # Trải phẳng
array([1, 0, 0, 0, 0, 0, 0, 0, 0])

>>> np.sort(a1, axis = 1) # Sắp xếp theo hàng, trả kết quả
array([[0, 0, 1],
       [0, 0, 0],
       [0, 0, 0]])
>>> a1.sort(axis = 1) # Sắp xếp tại chỗ theo hàng
```

#### Dùng `array`

[`array`](https://docs.python.org/zh-cn/3/library/array.html) là mảng số hiệu quả trong stdlib, nhưng không hỗ trợ lồng và ít dùng; chỉ nhắc qua.

Nếu không nói rõ, “mảng” nghĩa là “list”.

### [I/O](https://docs.python.org/zh-cn/3/tutorial/inputoutput.html)

Python dùng `input()` và `print()`. Dưới đây là nâng cao.

#### In định dạng

Thi đấu thường chỉ in số/chuỗi, `print()` là đủ, trừ khi cần định dạng float. Có 3 cách: `%` kiểu `printf`, `format`, và f-string (Python 3.6+). Xem [hướng dẫn](https://www.python-course.eu/python3_formatted_output.php). Ở đây chỉ minh họa kiểu `%`:

```pycon
>>> pi = 3.1415926; print('%.4f' % pi)   # %[flags][width][.precision]type
3.1416
>>> '%.4f - %8f = %d' % (pi, 0.1416, 3)  # Nhiều tham số dùng ()
'3.1416 - 0.141600 = 3'
```

#### Hàm `split()`

`input()` đọc cả dòng; trong OI thường một dòng nhiều số nên cần `split()` và list comprehension:

```pycon
>>> s = input('请输入一串数字: '); s
请输入一串数字: 1 2 3 4 5 6
'1 2 3 4 5 6'
>>> a = s.split(); a
['1', '2', '3', '4', '5', '6']
>>> a = [int(x) for x in a]; a
[1, 2, 3, 4, 5, 6]
>>> sum(a) / len(a)
3.5
```

Có khi mỗi dòng có nhiều số cố định; dùng “unpack”:

```pycon
>>> u, v, w = [int(x) for x in input().split()]
1 2 4
>>> print(u,v,w)
1 2 4
```

Nhập N dòng mà không dùng loop bằng thao tác sequence:

```pycon
>>> N = 4; mat = [[int(x) for x in input().split()] for i in range(N)]
1 3 3 
1 4 1 
2 3 4 
3 4 1 
>>> mat
[[1, 3, 3], [1, 4, 1], [2, 3, 4], [3, 4, 1]]
>>> u, v, w = map(list, zip(*mat))   
# * giải nén mat thành các list bên trong
# zip() gom các phần tử cùng vị trí thành tuple, trả iterator
# map(list, iterable) chuyển tuple thành list
>>> print(u, v, w)
[1, 1, 2, 3] [3, 4, 3, 4] [3, 1, 4, 1]
```

Thực chất là chuyển ma trận N×3 thành 3×N. `zip()` ghép phần tử theo vị trí; `map()` áp hàm lên phần tử. Trong Python 3, `zip()` và `map()` trả iterator.

#### [Đọc/ghi file](https://docs.python.org/3/reference/compound_stmts.html#the-with-statement)

Dùng `open()` và `with` để đảm bảo file đóng đúng:

```python
a = []
with open("in.txt") as f:
    N = int(f.readline())  # Đọc dòng đầu N
    a[len(a) :] = [[int(x) for x in f.readline().split()] for i in range(N)]

with open("out.txt", "w") as f:
    f.write("1\n")
```

Có nhiều hàm I/O khác; OI chưa hỗ trợ Python nên lược.

### [Điều khiển luồng](https://docs.python.org/zh-cn/3/tutorial/controlflow.html)

Python dùng thụt lề thay `{}`; sai thụt lề sẽ lỗi, tab và space trộn cũng lỗi; dòng bắt đầu khối cần `:`. Điều này tăng tính đọc, nhưng copy/paste mất thụt lề sẽ khó chịu.

#### Vòng lặp

List comprehension nhanh, nhưng nhiều trường hợp vẫn cần loop. Ví dụ đọc nhiều dòng:

```python
# Từ đây không dùng REPL, hãy tự chạy file
u, v, w = ([] for i in range(3))  # Gán nhiều biến
for i in range(4):  # Giả sử 4 dòng
    _u, _v, _w = [int(x) for x in input().split()]
    u.append(_u), v.append(_v), w.append(_w)
    # Không thể dùng cin >> u[i] >> v[i] >> w[i] vì list chưa đủ độ dài
    # Có thể pre-allocate MAXN nhưng phải nhớ độ dài thực và cắt phần thừa
print(u, v, w)
```

`for` trong Python giống range-based loop của C++11. Muốn duyệt chỉ số cần `range(len(lst))`.

Dùng while để đọc không biết số dòng:

```python
u, v, w = [], [], []
s = input()  # Python không cho gán trong điều kiện
while s:  # Không thể while(!scanf())
    # Dùng slice nối tránh append(), list comp lồng list
    u[len(u) :], v[len(v) :], w[len(w) :] = [[int(x)] for x in s.split()]
    s = input()
# Python 3.8 có walrus, nhưng OJ có thể không hỗ trợ
while s := input():
    u[len(u) :], v[len(v) :], w[len(w) :] = [[int(x)] for x in s.split()]
print(u, v, w)
```

#### Rẽ nhánh

Gần giống C/C++ nhưng không có gán trong điều kiện (trừ `:=`), và [không có switch](https://docs.python.org/zh-cn/3/faq/design.html#why-isn-t-there-a-switch-or-case-statement-in-python).

```python
# Điều kiện không cần ngoặc
if 4 >= 3 > 2 and 3 != 5 == 5 != 7:
    print("Quan hệ có thể viết liên tiếp")
    x = None or [] or -2
    print("&&  ||  !", "và  hoặc  không", "and or not", sep="\n")
    print("Dùng and/or giúp giảm dòng")
    if not x:
        print("Số âm cũng là True, không in dòng này")
    elif x & 1:
        print("Dùng elif, không phải else if\n" "Toán tử bit giống C, chẵn&1 = 0")
    else:
        print("Cũng có toán tử ba ngôi") if x else print("Chú ý cấu trúc")
```

#### Xử lý ngoại lệ

C++ có try/catch nhưng ít dùng trong thi; Python hay dùng [EAFP](https://docs.python.org/zh-cn/3/glossary.html#term-eafp). Ví dụ:

```python
s = "OI-wiki"
pat = "NOIP"
x = s.find(pat)  # find() không thấy trả -1
try:
    y = s.index(pat)  # index() không thấy thì ném lỗi
    print(y)  # Bị bỏ qua
except ValueError:
    print("Không tìm thấy")
    try:
        print(y)  # y chưa định nghĩa, sẽ lỗi
    except NameError as e:
        print("Không thể in y")
        print("Lý do:", e)
```

### Container built-in

Python có nhiều container mạnh, ngoài `list` còn có `tuple`, [`dict`](https://docs.python.org/zh-cn/3/library/stdtypes.html#mapping-types-dict), `set`.

`tuple` là list bất biến; nếu phần tử là mutable thì vẫn sửa được phần tử đó. `tuple` nhỏ nhẹ và [hashable](https://docs.python.org/zh-cn/3/glossary.html), hữu ích cho dict/set.

```python
tup = tuple([[1, 2], 4])  # Từ list tạo tuple
# Tương đương tup = ([1,2], 4)
tup[0].append(3)
print(tup)
a, b = 0, "I-Wiki"  # Gán nhiều biến là unpack tuple
print(id(a), id(b))
b, a = a, b
print(id(a), id(b))  # id đổi chỗ, cho thấy biến chỉ là “tên”
```

`dict` giống `map` trong C++ (khác `map()` built-in). Khóa là đối tượng hashable. Tính chất dict thay đổi qua các phiên bản, tự tìm hiểu.

```python
dic = {"key": "value"}
dic = {chr(i): i for i in range(65, 91)}
dic = dict(zip([chr(i) for i in range(65, 91)], range(65, 91)))
dic = {dic[k]: k for k in dic}
dic = {v: k for k, v in dic.items()}
dic = {
    k: v for k, v in sorted(dic.items(), key=lambda x: -x[1])
}

print(dic["A"])
dic["a"] = 97
if "b" in dic:  # LBYL
    print(dic["b"])
else:
    dic["b"] = 98

# Đếm tần suất
try:  # EAFP
    cnter[key] += 1
except KeyError:
    cnter[key] = 1
```

`set` giống `set` trong C++: không trùng phần tử, giống dict chỉ có key. `{}` tạo dict rỗng chứ không phải set.

### Viết hàm

Python không cần khai báo kiểu tham số/return, giúp code ngắn.

```python
def add(a, b):
    return a + b  # Lợi thế kiểu động, a/b có thể là chuỗi


def add_no_swap(a, b):
    print("in func #1:", id(a), id(b))
    a += b
    b, a = a, b
    print("in func #2:", id(a), id(b))  # a, b đã đổi
    return a, b  # Trả nhiều giá trị là tuple


lst1 = [1, 2]
lst2 = [3, 4]
print("outside func #1:", id(lst1), id(lst2))
add_no_swap(lst1, lst2)
# Bên ngoài không đổi chỗ
print("outside func #2:", id(lst1), id(lst2))
# Nhưng giá trị đã đổi
print(lst1, lst2)
```

#### Tham số mặc định

Tham số mặc định linh hoạt nhưng dễ bẫy:

```python
def append_to(element, to=[]):
    to.append(element)
    return to


lst1 = append_to(12)
lst2 = append_to(42)
print(lst1, lst2)

# Bạn nghĩ [12] [42]
# Nhưng thực tế [12, 42] [12, 42]
```

Do mặc định chỉ khởi tạo một lần, các lần gọi dùng chung list. Cách đúng: dùng `None`.

```python
def append_to(element, to=None):
    if to is None:
        to = []
    to.append(element)
    return to


lst1 = append_to(12)
lst2 = append_to(42)
print(lst1, lst2)

# Kết quả [12] [42]
```

#### Ghi chú kiểu

Python là dynamic, lỗi kiểu có thể lộ khi chạy:

```pycon
>>> if False:
...     1 + "two"  # Dòng này không chạy
... else:
...     1 + 2
...
3

>>> 1 + "two"
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

Python 3.5+ hỗ trợ type hints, chỉ là gợi ý, cần tool tĩnh (PyCharm, Mypy). Ví dụ:

```python
def headline(
    text,  # type: str
    width=80,  # type: int
    fill_char="-",  # type: str
):  # type: (...) -> str
    return f"{text.title()}".center(width, fill_char)


print(headline("type comments work", width=40))
```

Biến cũng có thể type hint; xem `__annotations__`:

```pycon
>>> nothing: str
>>> nothing
NameError: name 'nothing' is not defined

>>> __annotations__
{'nothing': <class 'str'>}
```

## Decorator

Decorator là hàm nhận một hàm/ phương thức và trả lại một hàm mới có thêm chức năng, giúp mở rộng mà không sửa code gốc. Xem [docs](https://docs.python.org/3/glossary.html#term-decorator).

Một decorator hữu ích: [`lru_cache`](https://docs.python.org/3/library/functools.html#functools.lru_cache), giúp memoization:

`@lru_cache(maxsize=128,typed=False)`

-   Tham số: `maxsize` và `typed`. Mặc định 128 và `False`.
-   `maxsize` là số kết quả tối đa được cache; `None` là vô hạn.
-   `typed=True` thì `f(3)` và `f(3.0)` là hai cache khác nhau.

Ví dụ Fibonacci:

```python
@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)
```

## Thư viện built-in thường dùng

Một số thư viện hữu ích cho thuật toán:

| Thư viện                                                             | Công dụng         |
| ------------------------------------------------------------------- | -------------- |
| [`array`](https://docs.python.org/3/library/array.html)             | Mảng độ dài cố định |
| [`argparse`](https://docs.python.org/3/library/argparse.html)       | Xử lý tham số CLI |
| [`bisect`](https://docs.python.org/3/library/bisect.html)           | Tìm kiếm nhị phân |
| [`collections`](https://docs.python.org/3/library/collections.html) | Ordered dict, deque... |
| [`fractions`](https://docs.python.org/3/library/fractions.html)     | Phân số |
| [`heapq`](https://docs.python.org/3/library/heapq.html)             | Hàng đợi ưu tiên |
| [`io`](https://docs.python.org/3/library/io.html)                   | I/O file/memory |
| [`itertools`](https://docs.python.org/3/library/itertools.html)     | Iterator |
| [`math`](https://docs.python.org/3/library/math.html)               | Hàm toán |
| [`os.path`](https://docs.python.org/3/library/os.html)              | Đường dẫn |
| [`random`](https://docs.python.org/3/library/random.html)           | Ngẫu nhiên |
| [`re`](https://docs.python.org/3/library/re.html)                   | Regex |
| [`struct`](https://docs.python.org/3/library/struct.html)           | Chuyển struct/binary |
| [`sys`](https://docs.python.org/3/library/sys.html)                 | Thông tin hệ thống |

## So sánh C++ và Python qua ví dụ

??? note "[Ví dụ Luogu P4779 — Dijkstra](https://www.luogu.com.cn/problem/P4779)"
    Cho $n(1 \leq n \leq 10^5)$ đỉnh, $m(1 \leq m \leq 2\times 10^5)$ cung có trọng số không âm, tính khoảng cách từ $s$ đến mọi đỉnh. Đảm bảo từ $s$ đến mọi đỉnh.

### Khai báo hằng

=== "C++"
    ```cpp
    #include <cstdio>
    #include <cstring>
    #include <queue>
    #include <vector>
    using namespace std;
    constexpr int N = 1e5 + 5, M = 2e5 + 5;
    ```

=== "Python"
    ```python
    try:  # Nhập module hàng đợi ưu tiên
        import Queue as pq  # python version < 3.0
    except ImportError:
        import queue as pq  # python3.*
    
    N = int(1e5 + 5)
    M = int(2e5 + 5)
    INF = 0x3F3F3F3F
    ```

### Khai báo struct forward-star và biến khác

=== "C++"
    ```cpp
    struct qxx {
      int nex, t, v;
    };
    
    qxx e[M];
    int h[N], cnt;
    
    void add_path(int f, int t, int v) { e[++cnt] = qxx{h[f], t, v}, h[f] = cnt; }
    
    using pii = pair<int, int>;
    priority_queue<pii, vector<pii>, greater<pii>> q;
    int dist[N];
    ```

=== "Python"
    ```python
    class qxx:  # Lớp forward-star (struct)
        def __init__(self):
            self.nex = 0
            self.t = 0
            self.v = 0
    
    
    e = [qxx() for i in range(M)]  # Danh sách
    h = [0 for i in range(N)]
    cnt = 0
    
    dist = [INF for i in range(N)]
    q = pq.PriorityQueue()  # Hàng đợi ưu tiên, min-heap theo phần tử đầu
    
    
    def add_path(f, t, v):  # Thêm cạnh vào forward-star
        # Nếu muốn sửa biến toàn cục, dùng global
        global cnt, e, h
        # Dòng debug, nhiều biến dùng tuple
        # print("add_path(%d,%d,%d)" % (f,t,v))
        cnt += 1
        e[cnt].nex = h[f]
        e[cnt].t = t
        e[cnt].v = v
        h[f] = cnt
    ```

### Thuật toán Dijkstra

=== "C++"
    ```cpp
    void dijkstra(int s) {
      memset(dist, 0x3f, sizeof(dist));
      dist[s] = 0, q.push(make_pair(0, s));
      while (q.size()) {
        pii u = q.top();
        q.pop();
        if (dist[u.second] < u.first) continue;
        for (int i = h[u.second]; i; i = e[i].nex) {
          const int &v = e[i].t, &w = e[i].v;
          if (dist[v] <= dist[u.second] + w) continue;
          dist[v] = dist[u.second] + w;
          q.push(make_pair(dist[v], v));
        }
      }
    }
    ```

=== "Python"
    ```python
    def nextedgeid(u):  # Generator, dùng trong for
        i = h[u]
        while i:
            yield i
            i = e[i].nex
    
    
    def dijkstra(s):
        dist[s] = 0
        q.put((0, s))
        while not q.empty():
            u = q.get()  # get() đồng thời xóa phần tử
            if dist[u[1]] < u[0]:
                continue
            for i in nextedgeid(u[1]):
                v = e[i].t
                w = e[i].v
                if dist[v] <= dist[u[1]] + w:
                    continue
                dist[v] = dist[u[1]] + w
                q.put((dist[v], v))
    ```

### Hàm main

=== "C++"
    ```cpp
    int n, m, s;
    
    int main() {
      scanf("%d%d%d", &n, &m, &s);
      for (int i = 1; i <= m; i++) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        add_path(u, v, w);
      }
      dijkstra(s);
      for (int i = 1; i <= n; i++) printf("%d ", dist[i]);
      return 0;
    }
    ```

=== "Python"
    ```python
    if __name__ == "__main__":
        # Đọc nhiều số trên một dòng
        n, m, s = map(int, input().split())
        for i in range(m):
            u, v, w = map(int, input().split())
            add_path(u, v, w)
    
        dijkstra(s)
    
        for i in range(1, n + 1):
            print(dist[i], end=" ")
    
        print()
    ```

### Mã đầy đủ

=== "C++"
    ```cpp
    #include <cstdio>
    #include <cstring>
    #include <queue>
    #include <vector>
    using namespace std;
    constexpr int N = 1e5 + 5, M = 2e5 + 5;
    
    struct qxx {
      int nex, t, v;
    };
    
    qxx e[M];
    int h[N], cnt;
    
    void add_path(int f, int t, int v) { e[++cnt] = qxx{h[f], t, v}, h[f] = cnt; }
    
    using pii = pair<int, int>;
    priority_queue<pii, vector<pii>, greater<pii>> q;
    int dist[N];
    
    void dijkstra(int s) {
      memset(dist, 0x3f, sizeof(dist));
      dist[s] = 0, q.push(make_pair(0, s));
      while (q.size()) {
        pii u = q.top();
        q.pop();
        if (dist[u.second] < u.first) continue;
        for (int i = h[u.second]; i; i = e[i].nex) {
          const int &v = e[i].t, &w = e[i].v;
          if (dist[v] <= dist[u.second] + w) continue;
          dist[v] = dist[u.second] + w;
          q.push(make_pair(dist[v], v));
        }
      }
    }
    
    int n, m, s;
    
    int main() {
      scanf("%d%d%d", &n, &m, &s);
      for (int i = 1; i <= m; i++) {
        int u, v, w;
        scanf("%d%d%d", &u, &v, &w);
        add_path(u, v, w);
      }
      dijkstra(s);
      for (int i = 1; i <= n; i++) printf("%d ", dist[i]);
      return 0;
    }
    ```

=== "Python"
    ```python
    try:  # Nhập module hàng đợi ưu tiên
        import Queue as pq  # python version < 3.0
    except ImportError:
        import queue as pq  # python3.*
    
    N = int(1e5 + 5)
    M = int(2e5 + 5)
    INF = 0x3F3F3F3F
    
    
    class qxx:  # Lớp forward-star (struct)
        def __init__(self):
            self.nex = 0
            self.t = 0
            self.v = 0
    
    
    e = [qxx() for i in range(M)]  # Danh sách
    h = [0 for i in range(N)]
    cnt = 0
    
    dist = [INF for i in range(N)]
    q = pq.PriorityQueue()  # Hàng đợi ưu tiên, min-heap theo phần tử đầu
    
    
    def add_path(f, t, v):  # Thêm cạnh vào forward-star
        # Nếu muốn sửa biến toàn cục, dùng global
        global cnt, e, h
        # Dòng debug, nhiều biến dùng tuple
        # print("add_path(%d,%d,%d)" % (f,t,v))
        cnt += 1
        e[cnt].nex = h[f]
        e[cnt].t = t
        e[cnt].v = v
        h[f] = cnt
    
    
    def nextedgeid(u):  # Generator, dùng trong for
        i = h[u]
        while i:
            yield i
            i = e[i].nex
    
    
    def dijkstra(s):
        dist[s] = 0
        q.put((0, s))
        while not q.empty():
            u = q.get()
            if dist[u[1]] < u[0]:
                continue
            for i in nextedgeid(u[1]):
                v = e[i].t
                w = e[i].v
                if dist[v] <= dist[u[1]] + w:
                    continue
                dist[v] = dist[u[1]] + w
                q.put((dist[v], v))
    
    
    # Nếu chạy trực tiếp thì vào đây
    if __name__ == "__main__":
        # Đọc nhiều số trên một dòng
        n, m, s = map(int, input().split())
        for i in range(m):
            u, v, w = map(int, input().split())
            add_path(u, v, w)
    
        dijkstra(s)
    
        for i in range(1, n + 1):
            # Hai cách in đều được
            print("{}".format(dist[i]), end=" ")
            # print("%d" % dist[i],end=' ')
    
        print()  # Xuống dòng
    ```

## Tài liệu tham khảo

1.  [Python Documentation](https://www.python.org/doc/)
2.  [Python 官方中文教程](https://docs.python.org/zh-cn/3/tutorial/)
3.  [Learn Python3 In Y Minutes](https://learnxinyminutes.com/docs/python3/)
4.  [Real Python Tutorials](https://realpython.com/)
5.  [廖雪峰的 Python 教程](https://www.liaoxuefeng.com/wiki/1016959663602400/)
6.  [GeeksforGeeks: Python Tutorials](https://www.geeksforgeeks.org/python-programming-language/)

## Tài liệu và chú thích

[^ref1]: [2. Python 解释器—Python 3 文档](https://docs.python.org/zh-cn/3/tutorial/interpreter.html#id1)

[^ref2]: [Unicode 指南—Python 3 文档](https://docs.python.org/zh-cn/3/howto/unicode.html#the-string-type)
