author: StudyingFather, NachtgeistW, countercurrent-time, Ir1d, H-J-Granger, Chrogeek, sshwy, Suyun514, hsfzLZH1, CBW2007, Xeonacid, kawa-yoiko, Konano

Trong lập trình thi đấu, có nhiều loại bài khác nhau.

## Bài truyền thống

**Bài truyền thống** là loại phổ biến nhất.

Thí sinh nộp mã nguồn; hệ thống chấm dùng các bộ dữ liệu đầu vào và đầu ra chuẩn làm test[^note1], biên dịch mã nguồn[^note2], cho chương trình đọc input, rồi so sánh output của thí sinh với output chuẩn để判断 đúng sai. Cách chấm này gọi là **chấm hộp đen**[^note3].

Với mỗi test, thường có giới hạn thời gian và bộ nhớ.

Giới hạn thời gian là giới hạn thời gian chạy chương trình[^note4]; chương trình không được vượt quá thời gian này trên một test.

Giới hạn bộ nhớ là giới hạn lượng bộ nhớ tối đa chương trình sử dụng.

Sau khi chương trình kết thúc bình thường, output sẽ được so với output chuẩn. So sánh thường lọc xuống dòng cuối và khoảng trắng cuối dòng rồi so sánh toàn văn. Với một số đề đặc biệt, dùng [Special Judge](../tools/special-judge.md).

Kết thúc quá trình, hệ thống chấm đưa ra **kết quả**[^note5]:

-   Accepted (AC)：được chấp nhận.
-   Compile Error (CE)：lỗi biên dịch.
-   Wrong Answer (WA)：kết thúc bình thường nhưng output sai.
-   Presentation Error (PE)：kết thúc bình thường nhưng format sai[^note6].
-   Runtime Error (RE)：kết thúc bất thường (mã trả về khác 0).
-   Time Limit Exceeded (TLE)：vượt thời gian.
-   Memory Limit Exceeded (MLE)：vượt bộ nhớ.
-   Output Limit Exceeded (OLE)：vượt giới hạn output.

Trong ICPC, phải AC tất cả test của bài mới được tính qua. Trong OI, AC từng test sẽ nhận điểm của test đó[^note7].

## Bài nộp đáp án

**Bài nộp đáp án** là bài chỉ nộp đáp án. Thường cung cấp file input, yêu cầu nộp gói có `XXX1.out`、`XXX2.out`、`XXX3.out`…`XXXn.out`.

Sau khi nộp, hệ thống so sánh với đáp án chuẩn và cho điểm theo chất lượng đáp án.

Vì không chạy chương trình, bài nộp đáp án không có giới hạn thời gian/bộ nhớ.

Cách làm thường có hai:

-   Làm tay.
-   Viết chương trình sinh đáp án.

## Bài tương tác

**Bài tương tác** yêu cầu chương trình tương tác với chương trình chấm. Thường là chương trình thí sinh gửi truy vấn, nhận phản hồi; chương trình chấm có thể限制 truy vấn hoặc调整策略 để tăng số truy vấn, tạo thêm biến hóa.

Xem [交互题](./interaction.md) để biết thêm.

Có hai kiểu tương tác chính. Dù khác về kỹ thuật, bản chất thuật toán không khác.

### Tương tác STDIO

STDIO (chuẩn I/O) là cách Codeforces, AtCoder, v.v. sử dụng; cũng là tiêu chuẩn của ICPC. Codeforces có [hướng dẫn ngắn (EN)](https://codeforces.com/blog/entry/45307).

???+ note "Ví dụ [LOJ #559.「LibreOJ Round #9」ZQC 的迷宫](https://loj.ac/problem/559)"
    Lưu ý phần bổ sung ở cuối.
    
    Bài này là tương tác.
    
    Bạn đang ở mê cung tối gồm $n \times m$ ô, cần đi đến đích để hoàn thành thử thách.
    
    Ban đầu ở $(1,1)$, mặt hướng sang phải, đích ở $(n,m)$. Mọi cặp ô liên thông duy nhất bằng một đường; hai ô kề (4 hướng) có độ dài 1. Giữa hai ô kề có thể có tường, tường rất mỏng, coi như không đáng kể. Biên ngoài đều có tường và mọi tường đều nối với biên. Mê cung hoàn toàn tối, bạn không biết gì ngoài vị trí $(n,m)$.
    
    Để tránh lạc, mỗi bước chỉ được đi bằng cách bám tường trái hoặc phải, tay bám tường đi đúng 1 đơn vị. Nếu bên trái/bên phải không có tường thì không thể đi theo hướng đó.
    
    Ở trong bóng tối lâu sẽ sợ, nên cần thoát càng sớm càng tốt. Nếu không ra trong số bước giới hạn, thử thách thất bại.

Với dạng này, thí sinh chỉ cần ghi truy vấn ra stdout, **flush** rồi đọc phản hồi từ stdin. Sau khi flush, chương trình chấm (interactor) mới nhận dữ liệu qua pipe. Trong C/C++ dùng `fflush(stdout)` hoặc `std::cout << std::flush` (dùng `std::cout << std::endl` cũng tự flush, nhưng `std::cout << '\n'` thì không); Pascal dùng `flush(output)`.

### Tương tác Grader

Grader thường gặp ở IOI, APIO, v.v. (đặc biệt CMS).

???+ note "Ví dụ [UOJ #206.【APIO2016】Gap](https://uoj.ac/problem/206)"
    Có $N$ số nguyên không âm tăng dần $a_1,a_2,\cdots,a_N (0\leq a_1<a2<\cdots<a_N\leq 10^{18})$. Hãy tìm giá trị lớn nhất trong $a_{i+1}−a_i (0\leq i\leq N−1)$.
    
    Bạn không thể đọc trực tiếp dãy này, nhưng có thể truy vấn bằng các hàm đã cho. Chi tiết theo ngôn ngữ xem phần hiện thực.
    
    Hãy hiện thực một hàm trả về giá trị lớn nhất của $a_{i+1}−a_i$.

Với dạng này, thí sinh chỉ cần viết một hàm cụ thể, dùng các hàm trợ giúp để tương tác. Để test local, đề thường phát header và `grader.cpp` (Pascal dùng `graderlib`), thí sinh biên dịch chương trình của mình cùng `grader.cpp`.

```sh
g++ grader.cpp my_solution.cpp -o my_solution -Wall -O2
./my_solution   # Chạy chương trình
```

Chương trình chạy giống bài truyền thống: mở file cố định, đọc dữ liệu theo format cố định, gọi hàm thí sinh viết, rồi in kết quả và thông tin (số truy vấn, độ đúng) ra stdout.

Khi chấm thật, chương trình thí sinh sẽ biên dịch với `grader.cpp` khác, gọi hàm thí sinh và chấm điểm. Thường `grader.cpp` này đặt mọi symbol global là `static`, nên không thể phá bằng trùng tên; mọi nỗ lực phá grader đều bị disqualification.

### Khác biệt

STDIO có ưu điểm hỗ trợ mọi ngôn ngữ, nhưng thời gian I/O có thể成为瓶颈, khó phân biệt hiệu quả thuật toán; Grader则相反, vì gọi hàm chi phí nhỏ, cho phép $10^6$ truy vấn, nhưng hạn chế ngôn ngữ.

Nếu tự thiết kế đề/contest, cần cân nhắc kỹ.

## Bài通信

**Bài通信** cần hai chương trình thí sinh giao tiếp để hoàn thành nhiệm vụ. Chương trình 1 nhận input và xuất output; input của chương trình 2 liên quan tới output của chương trình 1 (có thể nguyên dạng hoặc qua xử lý của judge), rồi cần输出 lời giải.

Ví dụ: [UOJ #178. 新年的贺电](https://uoj.ac/problem/178)，[#454.【UER #8】打雪仗](https://uoj.ac/problem/454).

Cách test local tùy đề, thường gồm:

-   Nhập tay
-   Viết chương trình phụ chuyển output 1 thành input 2
-   Dùng pipe hai chiều nối stdin/stdout của hai chương trình

Do nền tảng hỗ trợCommunication hạn chế, dạng này chủ yếu thấy ở IOI series và UOJ, v.v. Đây vẫn là lĩnh vực cần探索.

## Bài补全函数

**Bài补全函数** yêu cầu thí sinh hoàn thiện một phần mã. Có thể hiểu như bài tương tác: đề cho code, yêu cầu viết hàm hỗ trợ.

Thường có hai形式:

-   Cho một chương trình và chỉ rõ vị trí cần补全.
-   Không cho chương trình, mà truyền input vào như tham số của hàm cần nộp.

Dạng này phổ biến trên [LeetCode](https://leetcode.com/) và [PTA - 拼题 A](https://pintia.cn/problem-sets).

## Các loại khác

???+ note "Ví dụ [Quine](https://loj.ac/problem/4)"
    Viết chương trình in ra chính mã nguồn của nó.
    
    Mã phải có ít nhất 10 ký tự hiển thị.

Đề này rất经典, nhưng rất khó实现 trên đa số OJ.

??? note "参考代码"
    **Lưu ý**: mã nguồn không包含 dòng đầu tiên dưới đây (tức `// clang-format off`).
    
    ```cpp
    // clang-format off
    #include<cstdio>
    
    char *s={"#include<cstdio>%cchar *s={%c%s%c};%cint main(){printf(s,10,34,s,34,10);return 0;}"};
    
    int main(){printf(s,10,34,s,34,10);return 0;}
    ```

## Tài liệu tham khảo và chú thích

[^note1]: Do hạn chế kỹ thuật và tài nguyên, test của một bài thường không thể覆盖 toàn bộ dữ liệu trong phạm vi.

[^note2]: Với ngôn ngữ thông dịch như Python thì chạy bằng interpreter.

[^note3]: Thực tế hệ thống chấm phức tạp hơn nhiều, đây chỉ là giới thiệu khái quát.

[^note4]: Chính xác hơn là thời gian user mode.

[^note5]: Kết quả ở đây cũng áp dụng cho các loại bài khác.

[^note6]: Nhiều hệ thống chấm gộp PE vào WA.

[^note7]: Một số test có phần điểm; khi hoàn thành một phần nhiệm vụ hoặc output đúng nhưng không tối ưu, có thể nhận điểm比例.
