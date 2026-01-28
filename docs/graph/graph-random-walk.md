Trang này giới thiệu bài toán đi bộ ngẫu nhiên trên đồ thị. Chủ yếu khảo sát từ ba góc độ: đồ thị lưới, đồ thị thưa, đồ thị tổng quát; trình bày các phương pháp giải và so sánh ưu nhược điểm của chúng trong từng dạng bài.

## Định nghĩa

Cho một đồ thị có hướng đơn $G=(V, E)(V=\{v_1, v_2, \cdots, v_{|V|}\})$ với điểm xuất phát $s \in V$, đích $t \in V$. Mỗi cạnh $e=\left(x, y\right)$ có trọng số dương $w_e$, thỏa mãn $\forall x \in V \backslash\left\{t\right\}$, $\sum_{\left(x, y\right) \in E} w_{\left(x, y\right)}=1$, và với mọi điểm $x$ đều tồn tại đường đi từ $x$ tới $t$. Có một quân cờ xuất phát từ $s$, mỗi giây chọn cạnh ra $(x, y)$ với xác suất $w_{(x, y)}$ và đi tới $y$; tới đích thì dừng, yêu cầu kỳ vọng thời gian.

Thực tế, bài toán có thể viết dưới dạng ma trận. Định nghĩa ma trận $P$:

$$
P_{x, y}=
\begin{cases}
w_{(x, y)} & \text{if } (x, y) \in E \text{ and } x \neq t \\
0 & \text{if } (x, y) \notin E \text{ or } x=t \\
\end{cases}
$$

Đáp án cần tìm là:

$$
\sum_{k \geq 0} k \times\left(P^k\right)_{s, t}
$$

trong đó $\left(P^k\right)_{s, t}$ là xác suất lần đầu tới đích sau $k$ bước. Khi đồ thị hữu hạn và mọi điểm đều tới được đích, từ định nghĩa của $P$ có thể chứng minh mọi trị riêng của nó nhỏ hơn 1 nên đáp án hội tụ.

Để tiện trình bày, trong trang này nếu không nói riêng, dùng $n$ chỉ $|V|$, $m$ chỉ $|E|$.

Ngoài ra, ở đây, đồ thị thưa là đồ thị có số cạnh và số đỉnh cùng bậc lớn.

## Đồ thị lưới

???+ note "Ví dụ 1 [Circles of Waiting](https://codeforces.com/problemset/problem/963/E)"
    Có một quân cờ ban đầu đặt tại tọa độ $(0,0)$ trên mặt phẳng Oxy. Mỗi giây quân cờ di chuyển ngẫu nhiên. Giả sử nó đang ở $(x, y)$, giây tiếp theo có xác suất $p_1$ di chuyển tới $(x-1, y)$, xác suất $p_2$ tới $(x, y-1)$, xác suất $p_3$ tới $(x+1, y)$, xác suất $p_4$ tới $(x, y+1)$. Đảm bảo $p_1+p_2+p_3+p_4=1$.
    Hỏi kỳ vọng thời gian để nó di chuyển tới vị trí có khoảng cách Euclid tới gốc lớn hơn $R$. $0 \leq R \leq 50$, $p_1, p_2, p_3, p_4>0$, đáp án lấy mod $10^9+7$.

### Cách làm ngây thơ

Gọi $f(i, j)$ là kỳ vọng thời gian khi quân cờ ở $(i, j)$ để đi tới vị trí có khoảng cách Euclid tới gốc lớn hơn $R$, phương trình chuyển là:

$$
f(i, j)=
\begin{cases}
p_1 f(i-1, j) + p_2 f(i, j-1) + p_3 f(i+1, j) + p_4 f(i, j+1) + 1 & i^2 + j^2 \leq R^2 \\
0 & i^2 + j^2 > R^2
\end{cases}
$$

Do chuyển trạng thái không có thứ tự topo, cần dùng khử Gauss để giải. Độ phức tạp $O\left(R^6\right)$, không qua bài.

### Phương pháp khử trực tiếp

Nhận thấy hệ phương trình cần khử hầu hết hệ số là 0, khi khử chỉ tính ở vị trí khác 0 sẽ giảm độ phức tạp.

Xét quá trình khử, sắp xếp phương trình theo thứ tự từ trên xuống dưới trong hệ trục, cùng hàng từ trái sang phải. Tô vàng các phương trình đã khử, tô xanh các ô kề với ô vàng, các ô khác tô đen, như hình:

![graph-random-walk-1](images/graph-random-walk-1.svg)

Tiếp theo khử phương trình ô xanh kế tiếp. Trong phương trình này, chỉ hệ số biến của ô xanh và ô đen đầu tiên bên dưới có thể khác 0; và chỉ trong phương trình ứng với ô xanh và ô đen đầu tiên bên dưới là hệ số biến ứng với ô hiện tại khác 0.

Chú ý số ô xanh là $O(R)$, nên khử một phương trình tốn $O\left(R^2\right)$. Tổng cộng $O\left(R^2\right)$ phương trình, độ phức tạp hạ xuống $O\left(R^4\right)$, qua bài.

### Phương pháp chủ biến

Số phương trình và biến đều là $O\left(R^2\right)$, nếu rút xuống $O(R)$ thì khử Gauss ngây thơ sẽ qua.

Đặt biến ứng với ô đầu tiên mỗi hàng từ trái sang phải làm chủ biến, tổng $2 R+1$ biến, tìm cách biểu diễn biến ô khác thành hàm tuyến tính của các chủ biến. Xét từng cột trái qua phải, với mỗi ô $(i, j)$ trong cột, chú ý $f(i, j), f(i-1, j), f(i, j-1), f(i, j+1)$ đã biết dưới dạng hàm tuyến tính của chủ biến, chuyển vế ta có:

$$
f(i+1, j)=\frac{f(i, j)-p_1 f(i-1, j)-p_2 f(i, j-1)-p_4 f(i, j+1)-1}{p_3}
$$

Như vậy ta biểu diễn được $f(i+1, j)$ theo chủ biến. Nếu $(i+1, j)$ đã cách gốc quá $R$, ta nhận được phương trình: $f(i+1, j)=0$. Cuối cùng, thu được $2 R+1$ phương trình, khử Gauss các phương trình này là xong.

Giai đoạn truy hồi hàm tuyến tính theo chủ biến có $O\left(R^2\right)$ biến, mỗi biến tốn $O(R)$; sau đó kích thước bài toán giảm xuống $O(R)$. Cả hai phần đều $O\left(R^3\right)$, tổng $O\left(R^3\right)$, qua bài.

### So sánh hai cách

So sánh:

- Về độ phức tạp thời gian, phương pháp chủ biến trên đồ thị lưới tệ nhất là $O(n \sqrt{n})$ (khi chiều dài và rộng đều $O(\sqrt{n})$), còn khử trực tiếp là $O\left(n^2\right)$, chủ biến tốt hơn.
- Về độ chính xác, với bài cần tính thực thay vì mod, khử trực tiếp chính xác hơn chủ biến.
- Về phạm vi áp dụng:
    - Khi đồ thị lưới có chướng ngại hoặc một số cạnh xác suất 0, mỗi chướng ngại hoặc cạnh xác suất 0 phương pháp chủ biến cần thêm một chủ biến; nếu số này nhiều hơn $O(R)$, độ phức tạp tăng, trong khi khử trực tiếp không đổi.
    - Nhưng phương pháp chủ biến có thể làm khử cho phương trình chuyển tương tự đồ thị lưới, ví dụ $f(i, j)=p_1 f(i+1, j)+p_2 f(i, j+1)+p_3 f(\operatorname{pre}(i, j))+1$, với $\operatorname{pre}(i, j)=(x, y)(x \leq i, y \leq j)$ cho trước; phân tích độ phức tạp của khử trực tiếp không áp dụng cho mô hình này.
    - Ngoài ra, tính định thức ma trận kề đồ thị lưới không dùng được chủ biến, chỉ có thể dùng khử trực tiếp để tối ưu.

Tóm lại, hai phương pháp có ưu nhược, cần chọn theo bài cụ thể.

## Đồ thị thưa

???+ note "Ví dụ 2 Expected Value"
    Cho đồ thị vô hướng đơn liên thông thưa $G=(V, E)$, quân cờ bắt đầu ở $v_1$, mỗi giây chọn ngẫu nhiên đều một cạnh kề để đi, yêu cầu kỳ vọng thời gian tới $v_n$. $n \leq 2000$, đáp án mod $p$, $p$ là số nguyên tố ngẫu nhiên trong $\left[10^9, 1.01 \times 10^9\right]$.

### Kiến thức nền

**Định nghĩa 4.1.** Mọi đa thức $p(\lambda)$ thỏa mãn $p(A) = 0$ gọi là đa thức tiêu biến của ma trận $A$.

**Định nghĩa 4.2.** Ký hiệu $I_n$ là ma trận đơn vị bậc $n$, đặc trưng đa thức của ma trận $n \times n$ $A$ là $p(\lambda) = \det(\lambda I_n - A)$, với $\det$ là định thức. Không khó thấy bậc của đặc trưng đa thức của ma trận bậc $n$ không vượt quá $n$.

**Định lý 4.2.** (Cayley–Hamilton) Đặc trưng đa thức của bất kỳ ma trận nào cũng là đa thức tiêu biến của nó.

Vậy đa thức tiêu biến bậc nhỏ nhất của ma trận bậc $n$ không vượt quá $n$.

### Giải bài

Lưu ý kỳ vọng thời gian đi $E(t)=\sum_{i\geq0}\Pr[t>i]$, nếu tính được xác suất đi $i$ bước chưa kết thúc, cộng tất cả $i \ge 0$ là đáp án.

Gọi $f(i, j)$ là xác suất đi $i$ bước, đang ở $j$, và chưa tới $n$, khi đó:

$$
f(i,j)=\sum_{(k,j)\in E}\frac{f(i-1,k)}{\deg_k}(j\neq n)
$$

trong đó $\deg_k$ là bậc của $k$.

Chú ý chuyển của $f$ không phụ thuộc $i$, có thể coi mỗi bước là nhân ma trận $M$, tức $f_{i+1}=f_iM$. Vì đa thức tiêu biến bậc nhỏ nhất của $M$ không quá $n$, nên truy hồi ngắn nhất của $f$ cũng không quá $n$, suy ra truy hồi ngắn nhất của $\Pr[t>i]=\sum_{j=1}^{n-1}f(i,j)$ cũng không quá $n$. Ta có thể $O(nm)$ tính $\Pr[t>0],\Pr[t>1],\cdots,\Pr[t>3n]$, rồi dùng thuật toán Berlekamp–Massey $O(n^2)$ tìm truy hồi ngắn nhất của $\Pr[t > i]$.

Xét hàm sinh của dãy truy hồi tuyến tính bậc $k$ $a$. Giả sử $i ≥ i_0$ thì $a_i=\sum_{j=1}^kc_ja_{i-j}$, ký hiệu hàm sinh của $a$ và $c$ lần lượt là $A(x)$ và $C(x)$, khi đó $A(x)=A(x)C(x)+A_0(x)$, $A_0(x)$ quyết định bởi các hạng $i < i_0$.

Quay lại bài, vì tính được truy hồi ngắn nhất của $\Pr[t > i]$, ta có thể tính $C(x)$ và $A_0(x)$ (định nghĩa như trên), chuyển vế được $A(x)=\frac{A_0(x)}{1-C(x)}$. Ta cần $\sum_{i\geq0}[x^i]A(x)$, không khó thấy giá trị này bằng $A(1)$, thay $x = 1$ vào là đáp án. Vì mod là số nguyên tố ngẫu nhiên, có thể coi mẫu không bằng 0.

Như vậy giải bài trong $O(nm+n^2)$ thời gian. Nếu $n$ và $m$ cùng bậc, độ phức tạp coi như $O(n^2)$.

## Đồ thị tổng quát

???+ note "Ví dụ 3 Frank"
    Cho đồ thị có hướng đơn mạnh liên thông $G = (V, E)$, với mọi $1 ≤ s ≤ n$, $1 ≤ t ≤ n$, $s ≠ t$ trả lời:
    Quân cờ bắt đầu ở $v_s$, mỗi giây chọn ngẫu nhiên đều một cạnh ra, yêu cầu kỳ vọng thời gian tới $v_t$. $3 ≤ n ≤ 400$.

### Phân tích và chuyển hóa

Gọi $p_{i, j}$ là xác suất quân cờ ở $i$ chọn cạnh $(i, j)$ đi tới $j$, nếu cạnh không tồn tại thì xác suất 0. Gọi $f_{i,j}$ là kỳ vọng thời gian đi ngẫu nhiên từ $i$ đến $j$, đặc biệt $f_{i,i} = 0$. Khi $i ≠ j$, phương trình chuyển:

$$
f_{i,j}=1+\sum_{1\leq k\leq n}p_{i,k}f_{k,j}
$$

Khi $i = j$, gọi $g_i$ là kỳ vọng thời gian bắt đầu từ $i$, lần đầu quay về $i$, khi đó:

$$
f_{i,i}=1-g_i+\sum_{1\le k\le n}p_{i,k}f_{k,i}
$$

Viết dạng ma trận: gọi $P$ là ma trận chuyển của đồ thị, $F$ là ma trận đáp án, $I$ là ma trận đơn vị bậc $n$, $J$ là ma trận toàn $1$ bậc $n$, $G$ là ma trận $n$ bậc với $G_{i,i} = g_i$, chỗ khác 0, thì:

$$
F=J-G+PF
$$

Nếu tính được $G$, ta cần giải:

$$
(I − P)F = J − G
$$

### Tính G

**Định nghĩa 5.1.** Phân phối trạng thái ổn định của ma trận chuyển $P$ bậc $n$ là vectơ $n$ chiều $π$ thỏa $\sum_{i=1}^{n}\pi_{i}=1$, $πP = π$, mỗi thành phần $π$ trong $[0,1]$.

Ý nghĩa: nếu tại một thời điểm quân cờ ở $v_i$ với xác suất $π_i$, thì mọi thời điểm sau vẫn đúng phân phối này. Có thể giải phương trình bằng khử Gauss $O(n^3)$ để tìm $π$, liên hệ với $G$?

**Định lý 5.1.** Với mọi $1 ≤ i ≤ n$, có $π_ig_i = 1$.

???+ note "Chứng minh"
    Từ $F = J - G + PF$, chuyển vế:
    
    $$
    G = PF + J − F
    $$
    
    Nhân trái cả hai vế với $π$:
    
    $$
    πG = πPF + πJ − πF
    $$
    
    Từ định nghĩa $π$ có $πP = π$, suy ra:
    
    $$
    πG = πJ
    $$
    
    Do đó:
    
    $$
    \pi_ig_i=\sum_{j=1}^n\pi_j=1  
    $$

Vậy, bằng cách đưa vào phân phối ổn định, ta có thể $O(n^3)$ tính $G$.

### Giải bài

Khi giải phương trình, ta phát hiện $(I - P)$ không đầy hạng, không thể nghịch đảo.

**Định nghĩa 5.2.** Cây khung có hướng gốc $r\in V$ của đồ thị có hướng $G = (V,E)$ là đồ thị con $T = (V,A)$ thỏa:

1.  Với mọi $i ≠ r$, bậc ra của $i$ là $1$.
2.  Bậc ra của $r$ là $0$.
3.  $T$ không có chu trình.

**Bổ đề 5.1.** (Định lý cây ma trận trên đồ thị có hướng) Với đồ thị có hướng $G$, gọi $D$ là ma trận bậc ra ($D_{i,i} = d_i$, $D_{i,j} = 0(i ≠ j)$, $d_i$ là bậc ra của $i$), $A$ là ma trận kề, số cây khung có hướng gốc $r$ bằng định thức của $D - A$ sau khi bỏ hàng $r$ cột $r$.

**Định lý 5.2.** Với đồ thị mạnh liên thông $G = (V,E)$ và ma trận chuyển $P$, hạng của $(I - P)$ là $n - 1$.

???+ note "Chứng minh"
    Nhân một hàng của ma trận với hằng số khác 0 không đổi hạng, ta nhân hàng $i$ của $(I - P)$ với bậc ra của $v_i$, được ma trận $L$, chỉ cần chứng minh hạng $L$ là $n - 1$.  
    Vì tổng từng hàng của $L$ bằng 0, tổng mọi vectơ cột của $L$ là vectơ không, các vectơ này phụ thuộc tuyến tính, nên hạng $L$ không phải $n$.  
    Không khó thấy $L$ bằng ma trận bậc ra trừ ma trận kề của $G$, theo Bổ đề 5.1, định thức của $L$ sau khi bỏ hàng $i$ cột $i$ biểu diễn số cây khung có hướng gốc $v_i$.  
    Vì $G$ mạnh liên thông, số cây khung gốc bất kỳ đều khác 0, tức $L$ sau khi bỏ hàng $i$ cột $i$ vẫn đầy hạng.  
    Thêm một cột không giảm hạng, nên $L$ sau khi bỏ hàng $i$ mọi vectơ hàng độc lập tuyến tính. Do đó hạng $L$ là $n - 1$.  
    Quay lại bài, xét phương trình $AX = B$ (với $A$, $B$ biết, cần $X$). Vì $A$ không đầy hạng, nghiệm vô số, ta tìm một nghiệm riêng.  
    Khử Gauss chung $A$ và $B$. Biến $A$ ở $n - 1$ hàng đầu thành chỉ có đường chéo chính và cột $n$ có giá trị, hàng cuối thành toàn 0, dạng:
    
    $$
    \begin{bmatrix}
    1 & 0 & 0 & \cdots & 0 & a_1 \\0&1&0&\cdots&0&a_2\\0&0&1&\cdots&0&a_3\\
    \vdots&\vdots&\vdots&\ddots&\vdots&\vdots
    \\0&0&0&\cdots&1&a_{n-1}\\0&0&0&\cdots&0&0
    \end{bmatrix}
    X=
    \begin{bmatrix}
    b_{1,1}&b_{1,2}&b_{1,3}&\cdots&b_{1,n-1}&b_{1,n}
    \\b_{2,1}&b_{2,2}&b_{2,3}&\cdots&b_{2,n-1}&b_{2,n}
    \\b_{3,1}&b_{3,2}&b_{3,3}&\cdots&b_{3,n-1}&b_{3,n}
    \\\vdots&\vdots&\vdots&\ddots&\vdots&\vdots
    \\b_{n-1,1}&b_{n-1,2}&b_{n-1,3}&\cdots&b_{n-1,n-1}&b_{n-1,n}
    \\0&0&0&\cdots&0&0
    \end{bmatrix}
    $$
    
    Đặt $X_{n,i} = 0$, giải được một nghiệm riêng $Y$. Tiếp tục điều chỉnh nghiệm riêng thành nghiệm thực.  
    Vì $X_{n,i} = 0$, xét ý nghĩa tổ hợp, có $Y_{i,j} = 1 + Y_{j,j} + P_{i,k}X_{k,j}$, suy ra $X_{i,j} = Y_{i,j} - Y_{j,j}$.  
    Cuối cùng giải trong $O(n^3)$ thời gian.

## Tham khảo

1.  Bài viết “浅谈图模型上的随机游走问题.” Trong tuyển tập bài báo ứng viên đội tuyển IOI 2019 Trung Quốc (tr. 17-26)
