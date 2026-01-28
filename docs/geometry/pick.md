## Định lý Pick

Định lý Pick: Cho một đa giác đơn giản có tất cả các đỉnh là điểm nguyên (điểm có tọa độ nguyên), định lý Pick mô tả mối quan hệ giữa diện tích ${\displaystyle A}$, số điểm nguyên nằm trong đa giác ${\displaystyle i}$ và số điểm nguyên nằm trên biên đa giác ${\displaystyle b}$ như sau: ${\displaystyle A=i+{\frac {b}{2}}-1}$.

Chứng minh chi tiết: [Pick's theorem](https://en.wikipedia.org/wiki/Pick%27s_theorem)

Một số mở rộng của định lý Pick:

-   Nếu lấy diện tích của mỗi ô lưới là 1 đơn vị, trên lưới hình bình hành, định lý Pick vẫn đúng. Nếu áp dụng cho tam giác lưới bất kỳ, định lý Pick có dạng ${\displaystyle A=2 \times i+b-2}$.
-   Với đa giác không đơn giản ${\displaystyle P}$, định lý Pick có dạng ${\displaystyle A=i+{\frac {b}{2}}-\chi (P)}$, trong đó ${\displaystyle \chi (P)}$ là **đặc trưng Euler** của ${\displaystyle P}$.
-   Tổng quát lên không gian nhiều chiều: Đa thức Ehrhart.
-   Định lý Pick tương đương với **công thức Euler** (${\displaystyle V-E+F=2}$).

## Ví dụ ([POJ 1265](http://poj.org/problem?id=1265))

### Đề bài tóm tắt

Trên hệ tọa độ vuông góc, một robot xuất phát từ một điểm bất kỳ và thực hiện $\textit{n}$ bước di chuyển, mỗi lần di chuyển sang phải $\textit{dx}$, lên trên $\textit{dy}$, cuối cùng tạo thành một đa giác đơn giản khép kín trên mặt phẳng. Hỏi có bao nhiêu điểm trên biên, bao nhiêu điểm trong đa giác, và diện tích đa giác là bao nhiêu.

### Hướng giải

Bài này sử dụng ba kiến thức sau:

-   Với đoạn thẳng nối hai điểm nguyên, nếu cả $\textit{dx}$ và $\textit{dy}$ đều khác $0$, số điểm nguyên mà đoạn thẳng đi qua là $\gcd(\textit{dx}, \textit{dy}) + 1$. Nếu tính cho toàn bộ đa giác, các điểm trùng ở đầu cuối sẽ bị tính lặp, nên mỗi cạnh chỉ cần cộng $\gcd(\textit{dx},\textit{dy})$. Ở đây, $\textit{dx},\textit{dy}$ lần lượt là số bước theo trục hoành và trục tung. Nếu $\textit{dx}$ hoặc $\textit{dy}$ bằng $0$, số điểm là giá trị tuyệt đối của phần khác $0$.
-   Định lý Pick: Diện tích của đa giác đơn giản có đỉnh là điểm nguyên = số điểm trên biên / 2 + số điểm trong đa giác - 1.
-   Diện tích đa giác bất kỳ bằng nửa tổng giá trị tuyệt đối của tổng các tích chéo giữa các cặp điểm liên tiếp (có thể tính bằng công thức tích chéo hoặc tích phân theo chiều kim đồng hồ).

??? note "Mã mẫu tham khảo"
    ```cpp
    --8<-- "docs/geometry/code/pick/pick_1.cpp"
    ```
