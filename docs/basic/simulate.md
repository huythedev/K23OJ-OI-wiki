Trang này giới thiệu ngắn gọn về các bài toán mô phỏng (simulation).

## Giới thiệu

Mô phỏng là dùng máy tính để mô phỏng quá trình được mô tả trong đề bài.

Bài mô phỏng thường có lượng mã lớn, nhiều thao tác và logic phức tạp. Do lượng mã nhiều, dễ phát sinh lỗi khó tìm — trong thi cử sẽ rất tốn thời gian nếu viết sai.

## Kỹ thuật

Khi viết mã mô phỏng, tuân theo các đề xuất sau có thể giúp tăng tốc độ làm bài:

- Trước khi viết code, hãy phác thảo thật kỹ luồng xử lý trên giấy nháp.
- Trong code, tách các phần thành module, viết hàm, struct hoặc class để dễ quản lý.
- Với các khái niệm lặp lại, chuẩn hóa (ví dụ: chuyển YY-MM-DD hoặc giờ:phút sang số giây) để tránh nhầm lẫn.
- Gỡ lỗi theo từng phần; việc module hóa giúp test từng phần độc lập.
- Khi viết code cần có lộ trình rõ ràng, không nghĩ đến đâu viết tới đó.

Những bước trên cũng có ích cho các loại bài khác.

## Ví dụ giải

???+ note "[Climbing Worm](https://open.kattis.com/problems/climbingworm)"
    Một con giun (kích thước bỏ qua) ở đáy giếng sâu n inch. Mỗi lần leo lên u inch, nhưng phải nghỉ một lần rồi mới leo tiếp; khi nghỉ, nó trượt xuống d inch. Quá trình lặp lại. Hỏi giun cần ít nhất bao nhiêu lần leo để thoát giếng? Nếu sau lần leo vừa vặn tới miệng giếng thì coi là đã thoát.

??? note "Ý tưởng giải"
    Cứ mô phỏng quá trình giun leo: dùng vòng lặp lặp lại quá trình leo, nếu chiều cao leo được >= n thì dừng.

??? note "Tham khảo code"
    === "C++"
        ```cpp
        --8<-- "docs/basic/code/simulate/simulate_1.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/basic/code/simulate/simulate_1.py"
        ```
    
    === "Java"
        ```java
        --8<-- "docs/basic/code/simulate/simulate_1.java"
        ```

## Bài tập

-   [「NOIP2014」生活大爆炸版石头剪刀布 - Universal Online Judge](https://uoj.ac/problem/15)
-   [「OpenJudge 3750」魔兽世界](http://bailian.openjudge.cn/practice/3750/)
-   [「SDOI2010」猪国杀 - LibreOJ](https://loj.ac/problem/2885)
