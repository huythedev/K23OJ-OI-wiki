**DP Đếm (Counting DP)** là một phương pháp sử dụng kỹ thuật tìm kiếm có ghi nhớ tương tự như DP (có một số điểm khác biệt so với DP theo nghĩa hẹp, tức là các bài toán tối ưu hóa), được sử dụng để giải quyết các bài toán đếm (và tính tổng).

## Cơ sở

### Tư tưởng cơ bản

Bài toán đếm thường chỉ việc tìm kích thước của một tập hợp $S$. Trong OI, kích thước của $S$ đôi khi đạt đến mức $\Theta(n^n)$ hoặc thậm chí $\Theta(2^{n!})$ (tất nhiên, thông thường sẽ yêu cầu lấy modulo cho một số cố định nào đó), trong đó $n$ là quy mô của bài toán, vì vậy chúng ta không thể liệt kê từng phần tử của $S$.

Nếu chúng ta có thể chia $S$ thành các tập con rời nhau, thì số lượng phần tử của $S$ sẽ bằng tổng số lượng phần tử của các phần này. Nếu việc đếm các tập con này tương tự như bài toán gốc, chúng ta có thể giải quyết bằng phương pháp tương tự như quy hoạch động.

### Ví dụ

???+ note "Ví dụ"
    Cho một số nguyên dương $n$, hỏi có bao nhiêu cách phân hoạch $n$ thành tổng của $k$ số nguyên dương, các cách thay đổi thứ tự được coi là các cách phân hoạch khác nhau.

Cần tập hợp $S_{n,k}$ là tập hợp các bộ số nguyên dương có dạng $(a_1, \dots, a_k)$ sao cho $a_1 + \dots + a_k = n$. Nếu $a_k$ cố định, ta có suy luận sau: vì $a_1 + a_2 + \dots + a_{k-1} + a_k = n$, nên $a_1 + a_2 + \dots + a_{k-1} = n - a_k$. Theo định nghĩa của $S_{n,k}$, $(a_1, a_2, \dots, a_{k-1}) \in S_{n - a_k, k - 1}$.

Vì $a_1, a_2, \dots, a_k$ là các số nguyên dương, nên phạm vi giá trị của $a_k$ là $[1, n - k + 1] \cap \mathbb Z$. Do đó, $S_{n,k}$ có thể được phân hoạch theo $a_k$ thành $n - k + 1$ tập con, trong đó khi $a_k = i$, tập con này là:

$$
\{(L, i) \mid L \in S_{n-i,k-1}\}.
$$

Số lượng phần tử của tập con này hiển nhiên bằng $S_{n-i,k-1}$. Do các giá trị $i$ khác nhau, các tập con này đôi một rời nhau. Vì vậy:

$$
|S_{n,k}| = \sum_{i=1}^{n-k+1} |S_{n-i,k-1}|.
$$

Bằng cách này, chúng ta có thể sử dụng phương pháp tương tự DP để xử lý: gọi $f_{n,k}$ là $|S_{n,k}|$, ta có phương trình chuyển trạng thái:

$$
f_{n,k} = \sum_{i=1}^{n-k+1} f_{n-i,k-1}.
$$

Như vậy có thể sử dụng phương pháp DP để giải quyết.

### Sự giống và khác nhau với DP tối ưu hóa

Có thể thấy rằng, DP đếm và DP tối ưu hóa đều tìm một giá trị (giá trị cực đại/cực tiểu, giá trị tối ưu) trong một phạm vi $\Omega$. Giá trị này có được bằng cách xử lý tất cả các phần tử trong $\Omega$ một lần, sau đó tổng hợp các giá trị đã xử lý.

Ví dụ, đối với bài toán cái túi 0-1, các phần tử trong $\Omega$ là tập hợp tất cả các vật phẩm trong túi. Đối với một phương án $S$ trong $\Omega$, chúng ta xử lý $S$ một lần để nhận được kết quả $w(S)$ là tổng giá trị của các vật phẩm trong $S$. Đối với tất cả các giá trị nhận được, chúng ta lấy giá trị lớn nhất để có câu trả lời cho bài toán.

Đối với bài toán đếm, các phần tử trong $\Omega$ là tập hợp $S$ cần tính số lượng phần tử. Cách xử lý của nó là biến tất cả các phần tử trong $S$ thành $1$, sau đó tập hợp các số $1$ này lại bằng cách cộng chúng. Vì mỗi phần tử trong $S$ tương ứng với một số $1$, nên giá trị nhận được chính là số lượng phần tử trong $S$.

Khi thao tác tổng hợp là giá trị lớn nhất/nhỏ nhất, chúng ta có thể chia $\Omega$ thành các phần bất kỳ, chỉ cần hợp của các phần này là $\Omega$, không cần điều kiện rời nhau. Tuy nhiên, bài toán đếm không thỏa mãn điều kiện này, vì vậy chúng ta cần chia $\Omega$ thành các phần đôi một rời nhau, đây chính là điểm khác biệt so với DP tối ưu hóa.

## Ví dụ

???+ note "Ví dụ"
    Cho một số nguyên dương $n$, hỏi có bao nhiêu cách phân hoạch $n$ thành tổng của một số lượng bất kỳ các số nguyên dương, các cách thay đổi thứ tự được coi là các cách phân hoạch **giống nhau**.

### Cách giải 1

Tập hợp cần tính toán có các phần tử là đa tập các số nguyên dương có tổng bằng $n$. Tuy nhiên, cách này hiển nhiên không dễ để suy luận.

Nếu một đa tập $T$ chỉ chứa các số nguyên dương $\le M$, và tổng tất cả các phần tử trong $T$ bằng $n$, thì ta nói $T \in S_{n, M}$. Xét số lần xuất hiện của $M$, có thể là $k \in \left[0, \left\lfloor \dfrac nM \right\rfloor\right] \cap \mathbb Z$. Do đó, nó có thể được chuyển trạng thái sang $S_{n - kM, M - 1}$. Tính tổng lại là được. Độ phức tạp là $\Theta(n^2 \log n)$ ($\log$ đến từ cấp số điều hòa do phạm vi của $k$).

Nhưng như vậy vẫn chưa đủ tối ưu. Xét một ví dụ dưới đây:

$$
\begin{aligned}
f_{8, 3} &= {\color{red}f_{8, 2} + f_{5, 2} + f_{2, 2}} \\
f_{9, 3} &= {\color{blue}f_{9, 2} + f_{6, 2} + f_{3, 2} + f_{0, 2}} \\
f_{10, 3} &= {\color{green}f_{10, 2} + f_{7, 2} + f_{4, 2} + f_{1, 2}}\\
f_{11, 3} &= f_{11, 2} + {\color{red}f_{8, 2} + f_{5, 2} + f_{2, 2}}\\
f_{12, 3} &= f_{12, 2} + {\color{blue}f_{9, 2} + f_{6, 2} + f_{3, 2} + f_{0, 2}}\\
f_{13, 3} &= f_{13, 2} + {\color{green}f_{10, 2} + f_{7, 2} + f_{4, 2} + f_{1, 2}}\\
\end{aligned}
$$

Thay thế tương đương ta được $f_{11, 3} = f_{11, 2} + f_{8, 3}$, $f_{12, 3} = f_{12, 2} + f_{9, 3}$, $f_{13, 3} = f_{13, 2} + f_{10, 3}$. Tương tự, ta có thể thu được một phương trình chuyển trạng thái tổng quát:

$$
f_{n, M} = f_{n, M - 1} + \begin{cases} f_{n - M, M} & n \ge M, \\ 0 & \text{otherwise}. \end{cases}
$$

Lúc này, độ phức tạp thời gian là $\Theta(n^2)$.

### Cách giải 2

Xét thấy một đa tập $T$ gồm các số nguyên dương chắc chắn có thể nhận được thông qua hai thao tác: "tăng mỗi phần tử trong $T$ lên 1 đơn vị", "thêm một phần tử có giá trị là $1$ vào $T$". Các chuỗi thao tác khác nhau sẽ cho kết quả khác nhau.

Bằng cách này, việc chuyển trạng thái đối với $T$ có thể trở thành chuyển trạng thái đối với chuỗi thao tác. Xét chuỗi thao tác phân hoạch $n$ thành $m$ số (ký hiệu tập hợp các chuỗi thao tác này là $B_{n,m}$), xét thao tác cuối cùng: nếu là thao tác 1, số lượng các số không tăng, nhưng $\sum T$ tăng thêm $m$. Để $\sum T = n$ cuối cùng, tổng của $T$ ban đầu (ký hiệu là $T'$) cần là $n-m$. Vì vậy $B_{n,m} \to B_{n-m,m}$; nếu là thao tác 2, số lượng các số tăng thêm 1, và $\sum T$ tăng thêm $1$. Vì vậy $B_{n,m} \to B_{n-1,m-1}$.

Độ phức tạp thời gian của cách này vẫn là $\Theta(n^2)$.

### Cách giải 3

Cân nhắc việc chia $T$ thành phần $T_1$ gồm các số lớn hơn $\sqrt n$ và phần $T_2$ gồm các số nhỏ hơn hoặc bằng $\sqrt n$. $T_2$ có thể tính bằng Cách giải 1, còn số lượng $T_1$ có thể tính bằng cách sửa đổi nhẹ Cách giải 2: coi hai thao tác là "tăng mỗi phần tử trong $T_1$ lên 1 đơn vị", "thêm một phần tử có giá trị là $\lfloor \sqrt n \rfloor + 1$ vào $T_1$". Dễ dàng liệt kê phương trình chuyển trạng thái.

Chia $n$ thành hai phần $A$ và $B$. Duyệt qua một phần có thể suy ra phần còn lại. Tính số lượng $T_1$ thỏa mãn $\sum T_1 = A$ và số lượng $T_2$ thỏa mãn $\sum T_2 = B$, nhân chúng lại, tổng của tất cả các giá trị $A$ chính là kết quả cuối cùng.

Vì trong quá trình tính số lượng $T_1$, $M \le \sqrt n$, nên độ phức tạp thời gian khi sử dụng Cách giải 1 để tính $T_1$ là $\Theta(n^{3/2})$. Tương tự, vì trong quá trình tính số lượng $T_2$, $|T_2| \le \dfrac{\sum T_2}{\sqrt n} \le \dfrac{n}{\sqrt n} = \sqrt n$, nên độ phức tạp thời gian khi sử dụng Cách giải 2 để tính $T_2$ cũng là $\Theta(n^{3/2})$. Do đó tổng độ phức tạp thời gian là $\Theta(n^{3/2})$.