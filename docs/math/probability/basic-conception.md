## Khái quát

Khi nghiên cứu hiện tượng ngẫu nhiên, ta thường quan tâm:

-   Không gian mẫu $\Omega$, nêu tất cả kết quả có thể.
-   Trường sự kiện $\mathcal{F}$, tập các biến cố quan tâm.
-   Xác suất $P$, đo mức độ xảy ra của mỗi biến cố.

## Không gian mẫu, biến cố

### Định nghĩa

Một kết quả không thể phân nhỏ hơn trong một hiện tượng ngẫu nhiên gọi là **điểm mẫu**. Tập các điểm mẫu gọi là **không gian mẫu**, ký hiệu $\Omega$.

Một **biến cố** là tập con của $\Omega$, gồm một số điểm mẫu, ký hiệu $A, B, C, \cdots$.

Với kết quả $\omega$ và biến cố $A$, nói $A$ **xảy ra** khi và chỉ khi $\omega \in A$.

Ví dụ, gieo một con xúc xắc có $\Omega=\{1,2,3,4,5,6\}$. Gọi biến cố $A$ là “điểm lớn hơn $4$”, thì $A=\{5,6\}$. Nếu kết quả $\omega=3$ thì $\omega \notin A$, nên $A$ không xảy ra.

### Phép toán trên biến cố

Vì biến cố là tập con của $\Omega$, ta có thể dùng các phép toán tập hợp (giao, hợp, bù). Ký hiệu giống tập hợp.

Đặc biệt, hợp $A \cup B$ còn ký hiệu $A + B$, giao $A \cap B$ ký hiệu $AB$, gọi lần lượt là **biến cố tổng** và **biến cố tích**.

## Trường sự kiện

Khi nghiên cứu hiện tượng cụ thể, ta cần xác định những biến cố quan tâm. Theo định nghĩa, $\mathcal{F} \subset 2^{\Omega}$ (với $2^{\Omega}$ là tập lũy thừa), nhưng không nhất thiết $\mathcal{F} = 2^{\Omega}$. Với $\Omega$ hữu hạn thì khó thấy sự khác biệt; với $\Omega$ vô hạn, $2^{\Omega}$ lớn hơn nhiều và có thể chứa những biến cố “không tốt” hoặc không quan tâm, nên cần cân bằng giữa tính chất và phạm vi.

Không phải mọi tập con của $2^{\Omega}$ đều là trường sự kiện. Ta thường muốn trường $\mathcal{F}$ đóng với các phép toán trên biến cố, nên yêu cầu:

-   $\varnothing \in \mathcal{F}$;
-   Nếu $A \in \mathcal{F}$ thì biến cố đối $\bar{A} \in \mathcal{F}$;
-   Nếu $A_n \in \mathcal{F}, n = 1, 2, 3\dots$ thì $\bigcup A_n \in \mathcal{F}$.

Tức $\mathcal{F}$ đóng với phép bù và hợp đếm được, và chứa $\varnothing$.

Có thể chứng minh $\mathcal{F}$ cũng đóng với giao đếm được.

Ví dụ gieo xúc xắc $\Omega=\{1,2,3,4,5,6\}$, hai tập sau là trường sự kiện:

-   $\mathcal{F}_1 = \{ \varnothing, \Omega \}$
-   $\mathcal{F}_2 = \{ \varnothing, \{1, 3, 5\}, \{2, 4, 6\}, \Omega \}$

Nhưng hai tập sau không phải:

-   $\mathcal{F}_3 = \{ \varnothing, \{1\}, \Omega \}$ (không đóng với bù)
-   $\mathcal{F}_4 = \{ \{1, 3, 5\}, \{2, 4, 6\} \}$ (không chứa $\varnothing$ và không đóng với hợp)

## Xác suất

### Định nghĩa

#### Định nghĩa cổ điển

Trong giai đoạn đầu, các hiện tượng đơn giản, $\Omega$ hữu hạn và các điểm mẫu được coi là đồng khả năng, nên có:

Nếu một hiện tượng thỏa:

-   Có hữu hạn kết quả cơ bản;
-   Mỗi kết quả cơ bản là đồng khả năng;

thì với mỗi biến cố $A$, xác suất

$$
P(A)=\frac{\#(A)}{\#(\Omega)}
$$

trong đó $\#(\cdot)$ là độ đo kích thước của tập.

Định nghĩa này có thể mở rộng cho một số trường hợp $\Omega$ vô hạn, gọi là [mô hình hình học](https://baike.baidu.com/item/%E5%87%A0%E4%BD%95%E6%A6%82%E5%9E%8B/4035773).

#### Định nghĩa tiên đề

Định nghĩa trực quan trên có lỗ hổng: dùng “khả năng” để định nghĩa “xác suất”, tạo vòng lặp. “Đồng khả năng” cũng mơ hồ khi $\Omega$ vô hạn, dẫn đến [nghịch lý Bertrand](https://baike.baidu.com/item/%E8%B4%9D%E7%89%B9%E6%9C%97%E6%82%96%E8%AE%BA/9241081).

Sau nhiều nghiên cứu, Kolmogorov năm 1933 trong “Cơ sở lý thuyết xác suất” đưa ra định nghĩa tiên đề:

Hàm xác suất $P$ là ánh xạ từ $\mathcal{F}$ đến đoạn $[0, 1]$, thỏa:

-   **Chuẩn hóa**: $P(\Omega)=1$.
-   **Cộng được đếm**: nếu $A_1, A_2, \cdots$ đôi một rời nhau thì $P\left( \bigcup_{i \geq 1} A_i \right) = \sum_{i \geq 1} P(A_i)$.

### Tính chất của hàm xác suất

Với mọi $A, B \in \mathcal{F}$:

-   **Đơn điệu**: nếu $A \subset B$ thì $P(A) \leq P(B)$.
-   **Nguyên lý bao hàm–loại trừ**: $P(A+B) = P(A) + P(B) - P(AB)$.
-   $P(A - B) = P(A) - P(AB)$, trong đó $A - B$ là hiệu tập.

## Không gian xác suất

Như đã nêu, khi nghiên cứu hiện tượng ngẫu nhiên ta xét $\Omega$, $\mathcal{F}$ và $P$. Bộ ba $(\Omega, \mathcal{F}, P)$ gọi là không gian xác suất.

Xác suất chỉ có ý nghĩa khi xác định rõ không gian xác suất. Nghịch lý Bertrand thực chất do mô tả $\Omega$ không rõ ràng.

## Tài liệu tham khảo và chú thích

-   [概率论（数学分支）_百度百科](https://baike.baidu.com/item/概率论/829122)
-   [Probability - Wikipedia](https://en.wikipedia.org/wiki/Probability)
