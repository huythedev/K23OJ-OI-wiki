author: Early0v0, frank-xjh, Great-designer, ksyx, qiqistyle, Tiphereth-A , Saisyc, shuzhouliu, Xeonacid, xyf007

Trang này sẽ giới thiệu ngắn gọn về thuật toán **liệt kê** (enumerate).

## Giới thiệu

**Liệt kê** (tiếng Anh: Enumerate) là một chiến lược giải quyết bài toán dựa trên việc dự đoán đáp án từ kiến thức đã có.

Tư tưởng của liệt kê là liên tục thử các khả năng, lần lượt kiểm tra từng trường hợp trong tập hợp các khả năng, sau đó kiểm tra xem có thỏa mãn điều kiện đề bài không.

## Các điểm cần lưu ý

### Xác định không gian nghiệm

Xây dựng mô hình toán học ngắn gọn.

Khi liệt kê, cần xác định rõ: các trường hợp có thể là gì? Cần liệt kê những yếu tố nào?

### Giảm không gian liệt kê

Phạm vi liệt kê là gì? Có cần liệt kê toàn bộ không?

Khi giải bài bằng phương pháp liệt kê, nhất định phải làm rõ hai điều này, nếu không sẽ tốn thời gian không cần thiết.

### Chọn thứ tự liệt kê hợp lý

Tùy vào yêu cầu bài toán. Ví dụ, nếu đề yêu cầu tìm số nguyên tố lớn nhất thỏa mãn điều kiện, thì nên liệt kê từ lớn xuống nhỏ.

## Ví dụ

Dưới đây là một ví dụ về giải bài bằng liệt kê và tối ưu hóa phạm vi liệt kê.

??? note "Ví dụ"
    Cho một mảng các số nguyên khác nhau và đều khác $0$. Hỏi có bao nhiêu cặp số trong mảng có tổng bằng $0$?

??? note "Hướng dẫn giải"
    Viết code liệt kê hai số rất dễ.
    
    === "C++"
        ```cpp
        for (int i = 0; i < n; ++i)
          for (int j = 0; j < n; ++j)
            if (a[i] + a[j] == 0) ++ans;
        ```
    
    === "Python"
        ```python
        for i in range(n):
            for j in range(n):
                if a[i] + a[j] == 0:
                    ans += 1
        ```
    
    === "Java"
        ```java
        for (int i = 0; i < n; ++i)
          for (int j = 0; j < n; ++j)
            if (a[i] + a[j] == 0) ++ans;
        ```
    
    Xem xét cách tối ưu phạm vi liệt kê. Vì đề không yêu cầu cặp số có thứ tự, đáp án là gấp đôi trường hợp có thứ tự (nếu `(a, b)` là đáp án thì `(b, a)` cũng là đáp án). Do đó, chỉ cần đếm các cặp có thứ tự rồi nhân $2$.

    Ta có thể yêu cầu số đầu tiên phải đứng sau số thứ hai. Code như sau:
    
    === "C++"
        ```cpp
        for (int i = 0; i < n; ++i)
          for (int j = 0; j < i; ++j)
            if (a[i] + a[j] == 0) ++ans;
        ans *= 2;
        ```
    
    === "Python"
        ```python
        for i in range(n):
            for j in range(i):
                if a[i] + a[j] == 0:
                    ans += 1
        ans *= 2
        ```
    
    === "Java"
        ```java
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < i; ++j)
                if (a[i] + a[j] == 0) ++ans;
        ans *= 2;
        ```
    
    Như vậy đã giảm được phạm vi liệt kê của $j$, tiết kiệm thời gian.

    Ta có thể tối ưu hơn nữa.

    Có nhất thiết phải liệt kê cả hai số không? Sau khi liệt kê một số, điều kiện đề bài đã xác định số còn lại. Nếu có cách kiểm tra nhanh số còn lại có tồn tại không, có thể bỏ qua vòng lặp thứ hai. Ở mức nâng cao, nếu dữ liệu cho phép, có thể dùng **mảng đánh dấu (bucket)**[^1] để ghi lại các số đã duyệt.

    === "C++"
        ```cpp
        --8<-- "docs/basic/code/enumerate/enumerate_1.cpp"
        ```
    
    === "Python"
        ```python
        met = [False] * (MAXN * 2 + 1)
        for i in range(n):
            if met[MAXN - a[i]]:
                ans += 1
            met[a[i] + MAXN] = True
        ans *= 2
        ```
    
    === "Java"
        ```java
        boolean[] met = new boolean[MAXN * 2 + 1];
        for (int i = 0; i < n; ++i) {
            if (met[MAXN - a[i]]) ++ans;
            met[MAXN + a[i]] = true;
        }
        ans *= 2;
        ```

### Phân tích độ phức tạp

-   Phân tích thời gian: chỉ cần duyệt một lần mảng $a$, với $n$ lớn thì độ phức tạp là $O(n)$.
-   Phân tích bộ nhớ: $O(n+\max\{|x|:x\in a\})$.

## Bài tập

-   [2811: Bài toán tắt đèn - OpenJudge](http://bailian.openjudge.cn/practice/2811/)

## Chú thích

[^1]: [Sắp xếp theo thùng (Bucket sort)](../basic/bucket-sort.md), [Bài toán phần tử chủ yếu](../misc/main-element.md#离线算法), và [Giải thích về bucket trên Stack Overflow](https://stackoverflow.com/questions/42399355/what-is-a-bucket-or-double-bucket-data-structure) (tiếng Anh)
