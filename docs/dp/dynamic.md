Kiến thức chuẩn bị: [Ma trận](../math/linear-algebra/matrix.md), [Phân rã chuỗi trên cây (HLD)](../graph/hld.md).

Quy hoạch động động (Dynamic DP) là một kỹ thuật nâng cao được giới thiệu bởi Mao Côn (猫锟) tại WC2018. Nó thường được sử dụng để giải quyết các bài toán DP trên cây có kèm theo các thao tác sửa đổi trọng số của đỉnh (hoặc cạnh).

## Ví dụ

Lấy bài toán mẫu này làm ví dụ để giải thích quy trình của Dynamic DP.

???+ note "Ví dụ [Luogu P4719 【Mẫu】 Dynamic DP](https://www.luogu.com.cn/problem/P4719)"
    Cho một cái cây có $n$ đỉnh, mỗi đỉnh có một trọng số. Có $m$ thao tác, mỗi thao tác cho $x, y$ biểu thị việc sửa đổi trọng số của đỉnh $x$ thành $y$. Bạn cần tính kích thước trọng số của tập độc lập có trọng số lớn nhất trên cây sau mỗi thao tác.

### Phép nhân ma trận mở rộng

Định nghĩa phép nhân ma trận mở rộng $A \times B = C$ là:

$$
C_{i,j}=\max_{k=1}^{n}(A_{i,k}+B_{k,j})
$$

Điều này tương đương với việc thay phép nhân trong nhân ma trận thông thường bằng phép cộng, và phép cộng bằng phép $\max$.

Đồng thời, phép nhân ma trận mở rộng này cũng thỏa mãn tính kết hợp, vì vậy có thể sử dụng lũy thừa ma trận nhanh.

### Khi không có thao tác sửa đổi

Gọi $f_{i,0}$ là đáp án lớn nhất khi không chọn đỉnh $i$, $f_{i,1}$ là đáp án lớn nhất khi chọn đỉnh $i$.

Ta có phương trình DP:

$$
\begin{cases}f_{i,0}=\sum_{son}\max(f_{son,0},f_{son,1})\\f_{i,1}=w_i+\sum_{son}f_{son,0}\end{cases}
$$

Đáp án sẽ là $\max(f_{root,0}, f_{root,1})$.

### Khi có thao tác sửa đổi

Đầu tiên, thực hiện phân rã chuỗi trên cây (HLD). Giả sử có một chuỗi nặng (heavy chain) như sau:

![](./images/dynamic.png)

Gọi $g_{i,0}$ là đáp án lớn nhất khi không chọn đỉnh $i$ và chỉ cho phép chọn các đỉnh trong các cây con của các con nhẹ (light sons) của $i$. Gọi $g_{i,1}$ là đáp án lớn nhất khi chọn đỉnh $i$ mà không tính đến con nặng $son_i$. Ở đây $son_i$ là con nặng của $i$.

Giả sử chúng ta đã biết $g_{i,0/1}$, ta có phương trình DP:

$$
\begin{cases}f_{i,0}=g_{i,0}+\max(f_{son_i,0},f_{son_i,1})\\f_{i,1}=g_{i,1}+f_{son_i,0}\end{cases}
$$

Đáp án là $\max(f_{root,0}, f_{root,1})$.

Ta có thể xây dựng ma trận:

$$
\begin{bmatrix}
g_{i,0} & g_{i,0}\\
g_{i,1} & -\infty
\end{bmatrix}\times 
\begin{bmatrix}
f_{son_i,0}\\f_{son_i,1}
\end{bmatrix}=
\begin{bmatrix}
f_{i,0}\\f_{i,1}
\end{bmatrix}
$$

Lưu ý rằng ở đây chúng ta sử dụng quy tắc nhân ma trận mở rộng.

Có thể thấy, khi thực hiện sửa đổi, ta chỉ cần cập nhật $g_{i,1}$ và các chuỗi nặng hướng lên trên.

### Ý tưởng cụ thể

1.  Dùng DFS tiền xử lý để tính $f_{i,0/1}$ và $g_{i,0/1}$.

2.  Thực hiện phân rã chuỗi trên cây (lưu ý: vì khi truy vấn một điểm ta cần tính tích ma trận trên đoạn từ điểm đó đến cuối chuỗi nặng chứa nó, nên với mỗi đỉnh cần ghi lại $End_i$ là số hiệu của nút cuối cùng trong chuỗi nặng chứa $i$). Với mỗi chuỗi nặng, xây dựng một cây phân đoạn (Segment Tree) để duy trì ma trận $g$ và tích các ma trận $g$ trên đoạn.

3.  Khi sửa đổi: đầu tiên sửa đổi $g_{i,1}$ và ma trận của nút $i$ trong cây phân đoạn, tính toán sự thay đổi của ma trận tại $top_i$ (đỉnh chuỗi nặng), sau đó cập nhật sự thay đổi này lên ma trận của cha của $top_i$ ($fa_{top_i}$).

4.  Khi truy vấn: chỉ cần tính tích ma trận trên đoạn từ nút gốc (1) đến cuối chuỗi nặng chứa nó, cuối cùng lấy giá trị $\max$ là xong.

??? note "Mã nguồn tham khảo"
    ```cpp
    --8<-- "docs/dp/code/dynamic/dynamic_1.cpp"
    ```

## Bài tập tự luyện

-   [SPOJ GSS3 - Can you answer these queries III](https://www.spoj.com/problems/GSS3/)
-   [「NOIP2018」Bảo vệ vương quốc](https://loj.ac/p/2955)
-   [「SDOI2017」Trò chơi cắt cây](https://loj.ac/p/2269)