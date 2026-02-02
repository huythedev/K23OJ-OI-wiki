Trang này sẽ mô tả ngắn gọn cách sử dụng phép sắp xếp.

## Hiểu đặc điểm của dữ liệu

Sử dụng sắp xếp để xử lý dữ liệu giúp ta nắm được đặc điểm của dữ liệu, thuận tiện cho phân tích và trực quan hóa sau này. Ví dụ trong đời sống như từ điển, thực đơn — nếu không được sắp xếp theo một thứ tự nhất định thì thời gian tìm kiếm sẽ tăng đáng kể.

Máy tính cần xử lý dữ liệu quy mô lớn; sau khi sắp xếp, ta có thể thiết kế các bước xử lý tiếp theo phù hợp theo đặc điểm và nhu cầu của dữ liệu.

## Giảm độ phức tạp thời gian

Dùng sắp xếp làm tiền xử lý có thể giảm độ phức tạp thời gian cần thiết để giải bài toán, thường là đánh đổi không gian để lấy thời gian. Nếu một danh sách đã sắp xếp cần được phân tích nhiều lần thì chỉ tốn chi phí sắp xếp một lần sẽ rất có lợi, vì các lần phân tích sau sẽ tiết kiệm nhiều thời gian.

???+ note "Ví dụ: kiểm tra trong dãy số có phần tử bằng nhau hay không"
    Xét một dãy số, bạn cần kiểm tra xem có tồn tại hai phần tử bằng nhau hay không.
    
    Cách đơn giản là kiểm tra mọi cặp phần tử và so sánh — độ phức tạp thời gian là $O(n^2)$.
    
    Ta có thể sắp xếp dãy trước; khi đã sắp xếp, nếu tồn tại hai phần tử bằng nhau thì chúng sẽ đứng cạnh nhau trong dãy mới. Khi đó chỉ cần quét dãy đã sắp xếp một lần theo thứ tự, chi phí $O(n)$.
    
    Tổng độ phức tạp là chi phí sắp xếp: $O(n\log n)$.

## Là tiền xử lý cho tìm kiếm

Sắp xếp là tiền xử lý cần thiết cho [tìm kiếm nhị phân](./binary.md). Sau khi sắp xếp, việc tìm một phần tử trong dãy bằng tìm kiếm nhị phân có thể thực hiện trong $O(\log n)$ thời gian.
