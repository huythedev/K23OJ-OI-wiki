前置知识：[前缀和](./prefix-sum.md)

???+ warning "Nhắc nhở"
    Trang này **không** nói về [**Sắp xếp theo cơ số (Radix sort)**](./radix-sort.md).

Trang này sẽ giới thiệu ngắn gọn về **Sắp xếp đếm** (Counting sort).

## Định nghĩa

**Sắp xếp đếm** (tiếng Anh: Counting sort) là một thuật toán sắp xếp có độ phức tạp tuyến tính.

## Quy trình

Nguyên lý của sắp xếp đếm là sử dụng một mảng phụ $C$, trong đó phần tử thứ $i$ lưu số lần xuất hiện của giá trị $i$ trong mảng $A$ cần sắp xếp, sau đó dựa vào mảng $C$ để đưa các phần tử của $A$ về đúng vị trí.[^ref1]

Quy trình gồm ba bước:

1.  Đếm số lần xuất hiện của từng số;
2.  Tính [tổng tiền tố (prefix sum)](./prefix-sum.md) của số lần xuất hiện;
3.  Dựa vào tổng tiền tố, từ phải sang trái xác định thứ hạng của từng số.

### Vì sao cần tính tổng tiền tố?

Nếu chỉ đơn giản đưa các số có $C[i]>0$ vào mảng $A$ theo thứ tự, sẽ không xử lý được trường hợp có nhiều số trùng nhau.

Bằng cách tính tổng tiền tố cho từng phần tử của mảng phụ $C$, ta xác định được thứ hạng duy nhất cho từng phần tử trùng nhau:

Giá trị tại mỗi vị trí của $C$ là số lượng phần tử có giá trị đó, còn tổng tiền tố tại vị trí đó là thứ hạng của phần tử trùng cuối cùng.

Nếu ta duyệt $A$ từ phải sang trái, mảng kết quả sẽ giữ nguyên thứ tự ban đầu của các phần tử trùng nhau, tức là thuật toán **ổn định**.

![counting sort animate example](images/counting-sort-animate.svg)

## Tính chất

### Tính ổn định

Sắp xếp đếm là một thuật toán **ổn định**.

### Độ phức tạp thời gian

Độ phức tạp thời gian của sắp xếp đếm là $O(n+w)$, trong đó $w$ là miền giá trị của dữ liệu cần sắp xếp.

## Cài đặt mã nguồn

### Giả mã

$$
\begin{array}{ll}
1 & \textbf{Input. } \text{Một mảng } A \text{ gồm }n\text{ số nguyên dương không vượt quá } w. \\
2 & \textbf{Output. } \text{Mảng }A\text{ sau khi được sắp xếp tăng dần, ổn định.} \\
3 & \textbf{Method. }  \\
4 & \textbf{for }i\gets0\textbf{ to }w\\
5 & \qquad \textit{cnt}[i]\gets0\\
6 & \textbf{for }i\gets1\textbf{ to }n\\
7 & \qquad \textit{cnt}[A[i]]\gets\textit{cnt}[A[i]]+1\\
8 & \textbf{for }i\gets1\textbf{ to }w\\
9 & \qquad \textit{cnt}[i]\gets \textit{cnt}[i]+\textit{cnt}[i-1]\\
10 & \textbf{for }i\gets n\textbf{ downto }1\\
11 & \qquad B[\textit{cnt}[A[i]]]\gets A[i]\\
12 & \qquad \textit{cnt}[A[i]]\gets \textit{cnt}[A[i]]-1\\
13 & \textbf{return } B
\end{array}
$$

=== "C++"
    ```cpp
    --8<-- "docs/basic/code/counting-sort/counting-sort_1.cpp"
    ```

=== "Python"
    ```python
    --8<-- "docs/basic/code/counting-sort/counting-sort_1.py:core"
    ```

## Tài liệu tham khảo & chú thích

[^ref1]: [Counting sort - Wikipedia (tiếng Trung)](https://zh.wikipedia.org/wiki/%E8%AE%A1%E6%95%B0%E6%8E%92%E5%BA%8F)
