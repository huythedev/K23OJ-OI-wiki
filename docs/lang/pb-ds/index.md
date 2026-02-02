author: HeRaNO, Xeonacid, saffahyjp

Thư viện pb_ds là viết tắt của Policy-Based Data Structures.

pb_ds đóng gói nhiều cấu trúc dữ liệu như bảng băm (Hash), cây cân bằng, cây Trie, heap (priority queue), v.v.

Giống như `vector`, `set`, `map`, các thành phần của nó tuân theo giao diện STL. Một số (như priority queue) bao gồm toàn bộ chức năng của STL tương ứng nhưng mạnh hơn.

pb_ds chỉ dùng được với trình biên dịch sử dụng libstdc++ làm thư viện chuẩn.

Có thể dùng `begin()` và `end()` để lấy `iterator` và duyệt.

Có thể `increase_key`, `decrease_key` và xóa một phần tử đơn.

Vì phần chính của pb_ds nằm trong namespace `__gnu_pbds` bắt đầu bằng dấu gạch dưới, tính hợp lệ trong các kỳ thi NOI trước đây chưa rõ. Ngày 1/9/2021, theo [《关于 NOI 系列活动中编程语言使用限制的补充说明》](https://www.noi.cn/xw/2021-09-01/735729.shtml), cho phép dùng thư viện hoặc macro bắt đầu bằng gạch dưới (trừ trường hợp bị cấm rõ ràng), nên việc dùng pb_ds trong NOI đã có căn cứ.

**Tài liệu tham khảo: [《C++ 的 pb_ds 库在 OI 中的应用》](https://github.com/OI-Wiki/libs/blob/master/lang/pb-ds/C%2B%2B的pb_ds库在OI中的应用.pdf)**
