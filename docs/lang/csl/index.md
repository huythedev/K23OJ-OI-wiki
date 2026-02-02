## Chuẩn C++

Trước tiên cần giới thiệu về phiên bản của C++. C++ là một ngôn ngữ, nhưng cách mỗi compiler hiện thực khác nhau, nên cần chuẩn hóa để đảm bảo mã C++ nhất quán giữa các compiler. Từ 1985 đến nay, ISO đã ban hành 5 chuẩn chính thức: C++98, C++03, C++11 (còn gọi C++0x), C++14 (C++1y), C++17 (C++1z), C++20 (C++2a). Bản thảo chuẩn có tại [open-std](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/); chuẩn mới nhất C++23 (C++2b) vẫn đang được hoàn thiện. Ngoài ra còn có chuẩn bổ sung như C++ TR1.

Mỗi phiên bản C++ không chỉ quy định cú pháp và đặc tính ngôn ngữ, mà còn quy định một bộ thư viện chuẩn. Thư viện chuẩn C++ chứa nhiều hiện thực thường dùng như I/O, cấu trúc dữ liệu cơ bản, quản lý bộ nhớ, đa luồng... Nắm vững thư viện chuẩn là bước cần thiết để viết C++ hiện đại. Tài liệu thư viện chuẩn ở [cppreference](https://zh.cppreference.com/), nơi có mô tả cách dùng, hiệu suất, lưu ý của các kiểu/hàm. Hãy tận dụng.

Cần lưu ý các OJ khác nhau hỗ trợ chuẩn C++ khác nhau. Ví dụ [quy định ICPC mới nhất](https://docs.icpc.global/worldfinals-programming-environment/) hỗ trợ C++20. Theo quyết định của NOI, từ 01/09/2021, [NOI Linux 2.0](https://www.noi.cn/gynoi/jsgz/2021-07-16/732450.shtml) là môi trường chuẩn cho các kỳ thi NOI và CSP-J/S. Trong NOI Linux 2.0, g++ 9.3.0 [mặc định](https://gcc.gnu.org/projects/cxx-status.html#cxx14) hỗ trợ C++14 và có thể bật C++17, đáp ứng hầu hết nhu cầu. Vì vậy khi học C++ cần chú ý chuẩn mà kỳ thi hỗ trợ để tránh lỗi biên dịch.

## Standard Template Library (STL)

STL là một phần của thư viện chuẩn C++, gồm các cấu trúc dữ liệu và thuật toán dùng template. Nhờ template, STL tương thích với kiểu dữ liệu tự định nghĩa, giúp tránh “tự làm bánh xe”. NOI và ICPC đều hỗ trợ STL, nên sử dụng hợp lý STL giúp tránh viết lại thuật toán và tận dụng tối ưu của compiler. Xem [container STL](./container.md) và [thuật toán STL](./algorithm.md).

??? note "Thế nào là “tự làm bánh xe”"
    “Tự làm bánh xe” ([Reinventing_the_wheel](https://en.wikipedia.org/wiki/Reinventing_the_wheel)) nghĩa là lặp lại phát minh thuật toán đã có hoặc viết lại mã tối ưu sẵn. Việc này tốn thời gian, hiệu quả kém hơn người khác. Nhưng để học hoặc luyện tập, “tự làm bánh xe” là cần thiết.

## Thư viện Boost

[Boost](https://www.boost.org/) là thư viện công cụ C++ mã nguồn mở nổi tiếng, có tính di động, chất lượng cao, hiệu năng cao, độ tin cậy cao. Boost có rất nhiều module, đầy đủ tính năng và hỗ trợ đa nền tảng, nên được xem là “chuẩn gần” của C++. Nhiều tính năng chuẩn C++ đến từ Boost như smart pointer, meta-programming, ngày/giờ. Dù OI không dùng Boost, nhưng Boost có nhiều “bánh xe” để kiểm chứng thuật toán hoặc đối sánh, như Boost.Geometry có R-tree, Boost.Graph có thuật toán đồ thị, Boost.Intrusive có container xâm nhập tương tự STL. Bạn có thể tự tìm thêm tài liệu.

## Tài liệu tham khảo

1.  [C++ reference](https://en.cppreference.com/)
2.  [C++ 参考手册](https://zh.cppreference.com/)
3.  [Wikipedia - C++](https://zh.wikipedia.org/wiki/C%2B%2B)
4.  [Boost official](https://www.boost.org/)
5.  [Boost tutorial](https://theboostcpplibraries.com/)
