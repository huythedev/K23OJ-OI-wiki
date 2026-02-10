Trang này sẽ giới thiệu ngắn gọn khái niệm và phân loại thứ tự byte.

## Giới thiệu

Thứ tự byte là quy tắc lưu trữ của các đối tượng chương trình có nhiều byte, biểu thị cách sắp xếp các byte của một đối tượng.

## Phân loại

Thứ tự byte có hai loại: little-endian (tiểu đầu) và big-endian (đại đầu).

Để tiện giới thiệu, xét một biến nằm tại `0x100`, kiểu `int`, giá trị thập lục phân là `0x01234567`. Trong đó `0x01` là byte có trọng số lớn nhất, `0x67` là byte có trọng số nhỏ nhất.

### Little-endian

Little-endian là cách máy lưu trữ đối tượng trong bộ nhớ theo thứ tự từ byte **có trọng số nhỏ nhất** đến byte **có trọng số lớn nhất**.

Biến ở trên được biểu diễn như sau:

| .... | 0x100 | 0x101 | 0x102 | 0x103 | .... |
| ---- | ----- | ----- | ----- | ----- | ---- |
| .... | 67    | 45    | 23    | 01    | .... |

### Big-endian

Big-endian là cách máy lưu trữ đối tượng trong bộ nhớ theo thứ tự từ byte **có trọng số lớn nhất** đến byte **có trọng số nhỏ nhất**.

Biến ở trên được biểu diễn như sau:

| .... | 0x100 | 0x101 | 0x102 | 0x103 | .... |
| ---- | ----- | ----- | ----- | ----- | ---- |
| .... | 01    | 23    | 45    | 67    | .... |

### Sự khác nhau giữa hai thứ tự

Thực tế, hai thứ tự byte này không có ưu nhược điểm tuyệt đối. Tên gọi “little-endian” và “big-endian” xuất phát từ cuốn *Gulliver’s Travels*. Trong truyện, hai phe ở xứ người tí hon giao chiến không ngừng vì không thể thống nhất việc đập trứng từ đầu nhỏ hay đầu to. Tương tự như vậy, tranh luận về việc chọn thứ tự byte nào là vấn đề phi kỹ thuật.

Tuy nhiên, sự không nhất quán về thứ tự byte sẽ khiến dữ liệu nhị phân bị đảo thứ tự khi truyền giữa các loại máy khác nhau. Để tránh điều này, các ứng dụng mạng thiết lập một tiêu chuẩn, đảm bảo khi truyền sẽ dùng chuẩn mạng đã thỏa thuận thay vì biểu diễn nội bộ của từng máy.

## Quy ước lựa chọn thứ tự

-   Little-endian: x86, các bộ xử lý ARM chạy Android, iOS và Windows

-   Big-endian: Sun, PPC Mac, Internet
