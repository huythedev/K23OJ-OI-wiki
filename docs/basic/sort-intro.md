Trang này sẽ giới thiệu ngắn gọn về các thuật toán sắp xếp.

## Định nghĩa

**Thuật toán sắp xếp** (Tiếng Anh: Sorting algorithm) là thuật toán sắp xếp một tập dữ liệu theo một thứ tự nhất định. Có nhiều thuật toán với đặc điểm khác nhau.

## Tính chất

### Tính ổn định

Tính ổn định hỏi rằng các phần tử bằng nhau có giữ thứ tự tương đối ban đầu sau khi sắp xếp hay không.

Thuật toán ổn định giữ thứ tự tương đối của các bản ghi có khóa bằng nhau: nếu R xuất hiện trước S trong dãy gốc và khóa R = S, thì sau khi sắp xếp R vẫn đứng trước S.

Các thuật toán ổn định: Radix sort, Counting sort, Insertion sort, Bubble sort, Merge sort.

Các thuật toán không ổn định: Selection sort, Heap sort, Quick sort, Shell sort.

### Độ phức tạp thời gian

Trang chính: [Độ phức tạp](./complexity.md)

Độ phức tạp thời gian đo mối quan hệ giữa thời gian chạy và kích thước đầu vào, thường ký hiệu bằng O.

Phương pháp đơn giản là đếm số lần thực hiện các "thao tác cơ bản" hoặc đếm số lớp vòng lặp để ước lượng.

Phân loại độ phức tạp: tốt nhất, trung bình, tệ nhất. Trong OI thường quan tâm đến tệ nhất vì đó là ràng buộc đảm bảo kết quả trên bộ test.

Mốc dưới của các thuật toán so sánh là O(n log n).

Tuy nhiên vẫn có các thuật toán tốt hơn trong các điều kiện đặc biệt, ví dụ [Counting sort](./counting-sort.md) có độ phức tạp O(n + w) với w là phạm vi giá trị.

Dưới đây là đồ thị so sánh một số thuật toán sắp xếp.

![几种排序算法的比较](images/sort-intro-1.apng)

### Độ phức tạp không gian

Tương tự thời gian, độ phức tạp không gian mô tả lượng bộ nhớ cần thiết. Thường độ phức tạp không gian nhỏ hơn là tốt hơn.

## Liên kết ngoài

-   [排序算法 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95)
