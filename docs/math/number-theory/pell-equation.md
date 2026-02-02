<!-- filepath: /home/ubuntu/K23OJ-OI-wiki/docs/math/number-theory/pell-equation.md -->
Kiến thức tiền đề: [Phân số liên tục](./continued-fraction.md)、[Trường bậc hai](./quadratic.md)

## Giới thiệu

Bài viết này thảo luận việc giải (tổng quát hóa) phương trình Pell. Phương trình Pell tổng quát là phương trình vô định theo $x$ và $y$

$$
x^2-Dy^2=N,
$$

trong đó $D$ là số nguyên dương và không phải là số chính phương[^not-square], $N$ là số nguyên khác 0. Phương trình Pell theo nghĩa hẹp chỉ các trường hợp đặc biệt $N=1$ hoặc $N=\pm 1$, đôi khi còn bao gồm $N=\pm 4$. Phương trình Pell tổng quát gắn chặt với việc tìm các số nguyên bậc hai trong vành số nguyên bậc hai thực có chuẩn bằng $N$, còn các trường hợp thường được gọi là phương trình Pell (nghĩa hẹp) có thể xem là việc tìm các đơn vị trong vành số nguyên bậc hai thực.

Khi bài viết nhắc đến phương trình Pell, mặc định là trường hợp $N=1$. Tương ứng, trường hợp $N=-1$ được gọi là phương trình Pell âm[^neg-pell] (negative Pell's equation).

## Cấu trúc nghiệm

Nghiệm nguyên $(x,y)$ của phương trình Pell tổng quát có liên hệ chặt chẽ với số nguyên bậc hai $x+y\sqrt{D}$, do đó trong tài liệu, nghiệm của phương trình Pell thường được viết dưới dạng $x+y\sqrt{D}$. Vì chuẩn của số nguyên bậc hai là

$$
N(x+y\sqrt{D}) = x^2-Dy^2,
$$

nên phương trình Pell tổng quát về cơ bản tương đương với việc tìm số nguyên bậc hai có chuẩn $N$. Tuy nhiên, giữa hai bài toán vẫn có khác biệt tinh tế. Khi $x$ và $y$ đều là số nguyên, $x+y\sqrt{D}$ chắc chắn là số nguyên bậc hai; ngược lại, số nguyên bậc hai không nhất thiết yêu cầu $x$ và $y$ đều là số nguyên — trong trường hợp $D\equiv 1\pmod 4$, $x$ và $y$ còn có thể đồng thời là bán nguyên[^half-int].

Sự khác biệt này đặc biệt quan trọng khi tìm đơn vị cơ bản. Vì đơn vị trong vành số nguyên bậc hai là số nguyên bậc hai có chuẩn $\pm 1$. Với $D\equiv 2,3\pmod 4$, để tìm các đơn vị chỉ cần giải phương trình Pell tổng quát trong các trường hợp $N=\pm 1$; nhưng với $D\equiv 1\pmod 4$, cần xét thêm cả trường hợp $N=\pm 4$. [Phần dưới](#范数为-4-的情形) sẽ thảo luận cách tìm các đơn vị.

Để hiểu cấu trúc nghiệm của phương trình Pell tổng quát, cần bắt đầu từ [đẳng thức Brahmagupta](https://en.wikipedia.org/wiki/Brahmagupta%27s_identity):

$$
(x_1^2-Dy_1^2)(x_2^2-Dy_2^2)=(x_1x_2+Dy_1y_2)^2-D(x_1y_2+x_2y_1)^2.
$$

Điều này tương đương với việc chuẩn của số nguyên bậc hai bảo toàn phép nhân, tức là

$$
\begin{aligned}
N\left(x_1+y_1\sqrt{D}\right)N\left(x_2+y_2\sqrt{D}\right) &= N\left((x_1+y_1\sqrt{D})(x_2+y_2\sqrt{D})\right) \\
&= N\left((x_1x_2+Dy_1y_2)+(x_1y_2+x_2y_1)\sqrt{D}\right).
\end{aligned}
$$

Nhờ đẳng thức này, có thể kết hợp nghiệm nguyên của phương trình $x^2-Dy^2=N_1$ và phương trình $x^2-Dy^2=N_2$ để tạo ra nghiệm của phương trình $x^2-Dy^2=N_1N_2$. Ở góc nhìn số nguyên bậc hai, việc kết hợp nghiệm chính là phép nhân của các số nguyên bậc hai, qua đó thể hiện sự tiện lợi khi viết nghiệm dưới dạng số nguyên bậc hai. Đặc biệt, lấy $N_1=N$ và $N_2=1$ thì thấy rằng nếu đã biết một nghiệm của $x^2-Dy^2=N$ và toàn bộ nghiệm của phương trình Pell $x^2-Dy^2=1$, thì có thể sinh ra thêm nhiều nghiệm của $x^2-Dy^2=N$. Dĩ nhiên cách này không nhất thiết sinh ra tất cả nghiệm, nhưng ít nhất cho thấy việc hiểu cấu trúc nghiệm của phương trình Pell có vai trò quan trọng với việc hiểu cấu trúc nghiệm của phương trình Pell tổng quát.

### Phương trình Pell

Phương trình $x^2-Dy^2=1$ có ý nghĩa hình học là hyperbol với trục thực là trục $x$ và trục ảo là trục $y$. Mỗi điểm trên hyperbol tương ứng duy nhất với một giá trị khác 0 của $x+y\sqrt{D}$: nhánh trái tương ứng với giá trị âm, nhánh phải tương ứng với giá trị dương. Hơn nữa, trên mỗi nhánh, khi đi từ dưới lên trên, giá trị $x+y\sqrt{D}$ tăng строго. Giá trị của số nguyên bậc hai gán cho nghiệm của phương trình Pell một thứ tự tự nhiên.

Hyperbol đối xứng qua cả trục $x$ và trục $y$, do đó khi xét nghiệm của phương trình Pell chỉ cần xét phần trong góc phần tư thứ nhất; các nghiệm còn lại có thể thu được nhờ đối xứng. Điều này tương đương với việc chỉ xét nghiệm thỏa $x+y\sqrt{D}>1$. Nếu phương trình, ngoài $(\pm 1,0)$, còn có nghiệm không tầm thường, thì nhất định trong góc phần tư thứ nhất tồn tại nghiệm $(x_1,y_1)$ có giá trị $x+y\sqrt{D}$ nhỏ nhất; đây cũng là điểm nguyên có hoành và tung nhỏ nhất trong góc phần tư thứ nhất (không nằm trên trục tọa độ), gọi là nghiệm cơ bản (fundamental solution) của phương trình Pell[^fundamental-solution]. Theo thảo luận trước, mọi cặp $(x_k,y_k)$ thỏa $x_k+y_k\sqrt{D}=(x_1+y_1\sqrt{D})^k$ đều là nghiệm của phương trình Pell và đều nằm trong góc phần tư thứ nhất. Ngược lại, đây đúng là tất cả nghiệm trong góc phần tư thứ nhất. Kết hợp với đối xứng, ta có kết luận sau:

???+ note "Định lý"
    Giả sử nghiệm cơ bản của phương trình Pell $x^2-Dy^2=1$ là $(x_1,y_1)$. Khi đó, toàn bộ nghiệm của nó là
    
    $$
    \{(x,y):x+y\sqrt{D}=\pm(x_1+y_1\sqrt{D})^k,k\in\mathbf Z\}.
    $$

??? note "Chứng minh"
    Trước hết chứng minh không có nghiệm nào khác trong góc phần tư thứ nhất. Giả sử tồn tại nghiệm khác $x+y\sqrt{D}$ và với một $k\ge 0$ nào đó có
    
    $$
    x_k+y_k\sqrt{D}< x+y\sqrt{D}< x_{k+1}+y_{k+1}\sqrt{D}.
    $$
    
    Về mặt hình học, điều này nói rằng điểm nguyên $(x,y)$ nằm trên hyperbol giữa $(x_k,y_k)$ và $(x_{k+1},y_{k+1})$ (không kể hai đầu mút). Nhân bất đẳng thức với $x_k-y_k\sqrt{D}=(x_k+y_k\sqrt{D})^{-1}$ ta được
    
    $$
    1< (x+y\sqrt{D})(x_k-y_k\sqrt{D})=(xx_k-Dyy_k)+(x_ky-xy_k)\sqrt{D} < x_1+y_1\sqrt{D}.
    $$
    
    Theo tính đơn điệu đã nêu, bất đẳng thức này cho thấy $(xx_k-Dyy_k,x_ky-xy_k)$ là điểm nguyên nằm giữa $(1,0)$ và $(x_1,y_1)$. Điều này mâu thuẫn với cách chọn $(x_1,y_1)$.
    
    Khi mở rộng nghiệm trong góc phần tư thứ nhất ra toàn mặt phẳng, việc đổi dấu số mũ $k$ (tức lấy nghịch đảo) tương ứng với đối xứng qua trục $x$, còn đổi dấu toàn bộ tương ứng với đối xứng qua gốc tọa độ. Cộng thêm nghiệm tầm thường khi $k=0$, ta thu được toàn bộ nghiệm của phương trình Pell.

Phần trước chỉ giả sử nghiệm cơ bản tồn tại. Bây giờ cần chứng minh rằng phương trình Pell luôn có nghiệm không tầm thường.

???+ note "Định lý"
    Phương trình Pell $x^2-Dy^2=1$ luôn có nghiệm nguyên khác $(\pm 1,0)$.

??? note "Chứng minh"
    Trước hết, [định lý Dirichlet](./continued-fraction.md#用渐近分数逼近实数) cho biết tồn tại vô số cặp số nguyên dương $(x,y)$ sao cho
    
    $$
    \left|\dfrac{x}{y}-\sqrt{D}\right| \le \dfrac{1}{y^2}
    $$
    
    và do đó thỏa
    
    $$
    |x^2-Dy^2|=y^2\left|\dfrac{x}{y}-\sqrt{D}\right|\left|\dfrac{x}{y}+\sqrt{D}\right| \le \dfrac{1}{y^2}+2\sqrt{D}<1+2\sqrt{D}.
    $$
    
    Vì vậy, tất yếu tồn tại số nguyên $m\in(-1-2\sqrt{D},1+2\sqrt{D})$ sao cho có vô số cặp $(x,y)$ thỏa $x^2-Dy^2 = m$. Phân loại các cặp $(x,y)$ theo phần dư modulo $m$, ta suy ra tồn tại một cặp $(x_0,y_0)$ sao cho có vô số cặp $(x,y)$ thỏa $x\equiv x_0\pmod m$ và $y\equiv y_0\pmod m$. Lấy hai cặp khác nhau $(x_1,y_1)$ và $(x_2,y_2)$ thỏa các điều kiện đó, ta có
    
    $$
    \dfrac{x_1+y_1\sqrt{D}}{x_2+y_2\sqrt{D}}=\dfrac{x_1x_2-Dy_1y_2}{m}+\dfrac{x_2y_1-x_1y_2}{m}\sqrt{D}.
    $$
    
    Do các quan hệ đồng dư, ta có
    
    $$
    \begin{aligned}
    x_1x_2-Dy_1y_2 &\equiv x_0^2-Dy_0^2 = m \equiv 0 \pmod{|m|},\\
    x_2y_1-x_1y_2 &\equiv x_0y_0-x_0y_0 = 0 \pmod{|m|},
    \end{aligned}
    $$
    
    cho thấy vế phải là nghiệm nguyên. Hơn nữa, vì $(x_1,y_1)\neq(x_2,y_2)$, nghiệm này không tầm thường. Điều này chứng minh phương trình Pell действительно có nghiệm không tầm thường.

Tất nhiên, mục này đưa ra chứng minh phi kiến thiết; trong phần sau khi bàn về cách giải phương trình Pell, ta sẽ trực tiếp dùng phân số liên tục để xây dựng nghiệm, từ đó đưa ra một chứng minh khác cho sự tồn tại nghiệm không tầm thường. Ngoài ra, dù cấu trúc nghiệm của phương trình Pell trong mục này trùng với cấu trúc các đơn vị trong vành số nguyên bậc hai thực, nhưng với trường hợp $D\equiv 1\pmod 4$ thì mục này chưa giải quyết trọn vẹn cấu trúc đơn vị tương ứng; phần sau sẽ bàn tiếp.

### Phương trình Pell tổng quát

Đồ thị của phương trình Pell tổng quát $x^2-Dy^2=N$ cũng là hyperbol trên mặt phẳng, đối xứng qua trục $x$ và $y$. Như đã chỉ ra, một số nghiệm của $x^2-Dy^2=N$ có thể chỉ khác nhau bởi một thừa số là nghiệm của phương trình Pell, điều này cho phép chia các nghiệm thành các lớp tương đương. Với hai nghiệm $(x_1,y_1)$ và $(x_2,y_2)$ của $x^2-Dy^2=N$, nếu tồn tại nghiệm Pell $(u,v)$ sao cho $x_2+y_2\sqrt{D}=(x_1+y_1\sqrt{D})(u+v\sqrt{D})$, thì gọi hai nghiệm đó tương đương. Điều kiện cần và đủ để hai nghiệm tương đương là

$$
N\mid (x_1x_2-Dy_1y_2),\ N\mid (x_2y_1-x_1y_2).
$$

Vì nghiệm của phương trình Pell tương đối dễ tìm, một ý tưởng tự nhiên là trong mỗi lớp tương đương tìm một nghiệm. Khi biết các nghiệm này, có thể dùng các nghiệm Pell tương ứng để sinh ra toàn bộ nghiệm của phương trình Pell tổng quát. Trong các lớp tương đương, do đối xứng, mỗi lớp đều có nghiệm với tung độ $y$ không âm và nhỏ nhất có thể: nếu nghiệm đó là duy nhất thì gọi là nghiệm cơ bản của lớp; nếu không, lớp đó có hai nghiệm có $y$ không âm và nhỏ nhất, đối xứng qua trục $y$, và chọn nghiệm có $x>0$ làm nghiệm cơ bản. Do đó, giải $x^2-Dy^2=N$ tương đương với tìm tập nghiệm cơ bản $U$. Gọi nghiệm cơ bản của phương trình Pell tương ứng là $(r,s)$, thì toàn bộ nghiệm của phương trình Pell tổng quát là

$$
\{(x,y):x+y\sqrt{D}=\pm(r+s\sqrt{D})^k(u+v\sqrt{D}),k\in\mathbf Z,u+v\sqrt{D}\in U\}.
$$

Tập nghiệm cơ bản của phương trình Pell tổng quát là hữu hạn. Vì từ công thức nghiệm tổng quát, trị tuyệt đối $|u+v\sqrt{D}|$ nhất định nằm giữa $r-s\sqrt{D}$ và $r+s\sqrt{D}$. Ở phần tài liệu tham khảo cuối bài có các ước lượng chặt hơn về phạm vi tọa độ của nghiệm cơ bản. Dĩ nhiên, khác với phương trình Pell, phương trình Pell tổng quát có thể không có nghiệm.

Dùng một nghiệm $(u,v)$ của phương trình Pell tổng quát và nghiệm cơ bản $(r,s)$ của phương trình Pell để sinh ra toàn bộ nghiệm cùng lớp tương đương, ngoài cách dùng phép nhân nghiệm còn có thể dùng truy hồi:

$$
x_{k} = 2rx_{k-1} - x_{k-2},\ y_{k} = 2ry_{k-1} - y_{k-2},
$$

trong đó $x_k+y_k\sqrt{D}=(r+s\sqrt{D})^k(u+v\sqrt{D})$. Vì $x_k$ và $y_k$ đều có thể viết dạng $A(r+s\sqrt{D})^k+B(r-s\sqrt{D})^k$ với một cặp thực $(A,B)$, và theo định lý Vieta, $r\pm s\sqrt{D}$ là nghiệm của $x^2-2rx+1=0$, nên $x_k,y_k$ đều thỏa truy hồi tuyến tính bậc hai trên. So với phép nhân nghiệm, công thức truy hồi này cần ít phép nhân hơn.

## Phương pháp giải

Việc giải phương trình Pell và Pell tổng quát có thể dựa trên phân số liên tục.

### Thuật toán PQa

Các thuật toán trong bài dựa trên thuật toán PQa, dùng để tìm khai triển phân số liên tục của một số vô tỉ bậc hai.

Cho các số nguyên $P_0,Q_0,D$ thỏa $Q_0\neq 0$，$D>0$ và không phải là số chính phương, và $P_0^2\equiv D\pmod{Q_0}$. Khi đó số vô tỉ bậc hai

$$
\omega=\dfrac{P_0+\sqrt{D}}{Q_0}
$$

có khai triển phân số liên tục $[a_0,a_1,\cdots]$ có thể được tính bởi [công thức truy hồi](./continued-fraction.md#二次无理数):

$$
a_k = \left\lfloor\dfrac{P_k+\sqrt{D}}{Q_k}\right\rfloor,\ P_{k+1} = a_kQ_k - P_k,\ Q_{k+1} = \dfrac{D-P_{k+1}^2}{Q_k}.
$$

Tiếp theo, tử và mẫu của phân số liên tục thứ $k$ là $A_k$ và $B_k$ được cho bởi [công thức truy hồi](./continued-fraction.md#递推关系):

$$
A_k = a_kA_{k-1} + A_{k-2},\ B_k = a_kB_{k-1} + B_{k-2}
$$

với $A_{-1} = 1$，$A_{-2}=0$，$B_{-1}=0$，$B_{-2}=1$.

Độ đúng của các công thức đã được chứng minh trong bài phân số liên tục. Ở đó cũng chỉ ra rằng vì số vô tỉ bậc hai là [phân số liên tục tuần hoàn](./continued-fraction.md#二次无理数), nên bộ ba $(P_k,Q_k,a_k)$ cuối cùng sẽ đi vào chu kỳ, do đó thuật toán luôn kết thúc sau hữu hạn bước. Gọi độ dài chu kỳ tối tiểu là $\ell$, và vị trí bắt đầu sớm nhất là $k_0$, khi đó khai triển có dạng

$$
\omega=[a_0,\cdots,a_{k_0-1},\overline{a_{k_0},\cdots,a_{k_0+\ell-1}}].
$$

Để giải phương trình Pell bằng PQa, cần thiết lập kết quả sau:

???+ note "Định lý"
    Tiếp tục ký hiệu như trên. Đặt $G_k=Q_0A_k-P_0B_k$, khi đó cặp số nguyên $(G_{k-1},B_{k-1})$ thỏa
    
    $$
    G_{k-1}^2-DB_{k-1}^2=(-1)^{k}Q_0Q_{k},
    $$
    
    và $\gcd(G_{k-1},B_{k-1})$ chia hết $Q_{k}$.

??? note "Chứng minh"
    Gọi $\omega_k$ là dư (hoàn thương) thứ $k$ trong khai triển phân số liên tục của $\omega$, tức
    
    $$
    \omega = [a_0,a_1,\cdots,a_{k-1},\omega_k] = \dfrac{\omega_k A_{k-1}+A_{k-2}}{\omega_k B_{k-1}+B_{k-2}}.
    $$
    
    Thay $\omega=(P_0+\sqrt{D})/Q_0$ và $\omega_k=(P_k+\sqrt{D})/Q_k$ vào, ta được
    
    $$
    \dfrac{P_0+\sqrt{D}}{Q_0} = \dfrac{(P_k+\sqrt{D})A_{k-1}+Q_kA_{k-2}}{(P_k+\sqrt{D})B_{k-1}+Q_kB_{k-2}}.
    $$
    
    Khử mẫu và so sánh phần hữu tỉ và vô tỉ, rồi thay biểu thức $G_k$, ta được
    
    $$
    \begin{aligned}
    G_{k-1} &= P_kB_{k-1} + Q_kB_{k-2},\\
    DB_{k-1} &= P_kG_{k-1} + Q_kG_{k-2}.
    \end{aligned}
    $$
    
    Do đó, nhân phương trình đầu với $G_{k-1}$ và trừ phương trình hai nhân với $B_{k-1}$, ta có
    
    $$
    \begin{aligned}
    G_{k-1}^2-DB_{k-1}^2 &= (B_{k-2}G_{k-1}-B_{k-1}G_{k-2})Q_k \\
    &= (A_{k-1}B_{k-2}-B_{k-1}A_{k-2})Q_0Q_k \\
    &= (-1)^kQ_0Q_k.
    \end{aligned}
    $$
    
    Bước cuối dùng [công thức sai phân](./continued-fraction.md#误差估计) của phân số liên tục. Điều này chứng minh kết luận đầu.
    
    Để chứng minh kết luận thứ hai, thay biểu thức $G_k$ vào kết luận đầu, ta có
    
    $$
    (Q_0A_{k-1}-P_0B_{k-1})^2 - DB_{k-1}^2 = (-1)^kQ_0Q_k.
    $$
    
    Do $Q_0\mid(P_0^2-D)$ nên
    
    $$
    Q_0A_{k-1}^2 +\left(\dfrac{P_0^2-D}{Q_0}B_{k-1}- 2P_0A_{k-1}\right)B_{k-1} = (-1)^kQ_k.
    $$
    
    Suy ra $\gcd(G_{k-1},B_{k-1}) = \gcd(Q_0A_{k-1},B_{k-1})$ chia hết $Q_k$.

Kết quả này cung cấp cách tìm nghiệm của $x^2-Dy^2=N$. Nếu chọn $Q_0>0$ hợp lý và chọn $P_0$ là nghiệm của đồng dư $P_0^2\equiv D\pmod{Q_0}$, thì chạy PQa cho $(P_0+\sqrt{D})/Q_0$ cho đến khi gặp $(-1)^kQ_k=1$, lúc đó $(G_{k-1},B_{k-1})$ là một nghiệm của phương trình. Hơn nữa, nếu $Q_k=\pm 1$, nghiệm thu được là nghiệm nguyên tố cùng nhau, tức $\gcd(G_{k-1},B_{k-1})=1$.

Ý tưởng này là lõi để giải phương trình Pell và Pell tổng quát. Hiểu điều này rồi, ta sẽ xử lý chi tiết và chứng minh mọi nghiệm đều có thể thu được bằng cách này.

### Phương trình Pell

Để giải $x^2-Dy^2=1$, chỉ cần chạy PQa với $(P_0,Q_0,D)=(0,1,D)$ đến khi xuất hiện $(-1)^kQ_k=1$, khi đó $(A_{k-1},B_{k-1})$ là một nghiệm (vì $G_{k-1}$ khi đó chính là $A_{k-1}$). Dĩ nhiên, có mô tả chính xác hơn.

Trước hết, nghiệm chắc chắn xuất hiện ở cuối chu kỳ. Quá trình trên tương đương với khai triển phân số liên tục của $\sqrt{D}$. Ta đã có [kết luận](./continued-fraction.md#纯循环连分数):

$$
\sqrt{D} = [\lfloor\sqrt{D}\rfloor,\overline{a_1,\cdots,a_{\ell-1},2\lfloor\sqrt{D}\rfloor}].
$$

Ở đây, độ dài chu kỳ là $\ell$, và bắt đầu từ vị trí $1$ (chỉ số từ $0$). Hơn nữa, dư thứ $\ell$ bằng $\lfloor\sqrt{D}\rfloor+\sqrt{D}$, nên $Q_{\ell}=1$. Do đó, nếu $\ell$ chẵn thì $(A_{\ell-1},B_{\ell-1})$ là một nghiệm không tầm thường; nếu $\ell$ lẻ thì $(A_{2\ell-1},B_{2\ell-1})$ là một nghiệm không tầm thường.

Tiếp theo cần chứng minh nghiệm vừa tìm là nghiệm cơ bản. Kết luận này dựa trên hai điểm: (1) mọi nghiệm nguyên dương $(x,y)$ của phương trình Pell đều cho phân số $x/y$ xuất hiện trong các phân số liên tục của $\sqrt{D}$, nên $(x,y)$ phải là một $(A_k,B_k)$ nào đó của PQa; (2) ngoài cuối chu kỳ không có vị trí nào khác có $Q_k=1$, và do $A_k,B_k$ tăng theo $k$, nên nghiệm nguyên dương nhỏ nhất (nghiệm cơ bản) phải xuất hiện ở vị trí vừa nêu. Hai lý do này lần lượt từ hai định lý sau:

???+ note "Định lý"
    Nếu phương trình $x^2-Dy^2=N$ có nghiệm nguyên dương $(x,y)$ và $|N|<\sqrt{D}$, thì $\dfrac{x}{y}$ là một phân số liên tục của $\sqrt{D}$.

??? note "Chứng minh"
    Với $N>0$, do $x^2-Dy^2>0$ nên $x>y\sqrt{D}$. Do đó,
    
    $$
    \left|\dfrac{x}{y}-\sqrt{D}\right| = \dfrac{N}{y(x+y\sqrt{D})}<\dfrac{N}{2y^2\sqrt{D}}<\dfrac{1}{2y^2}.
    $$
    
    Theo [tiêu chuẩn Legendre](./continued-fraction.md#渐近分数的判定), suy ra $\dfrac{x}{y}$ là một phân số liên tục của $\sqrt{D}$.
    
    Với $N<0$, $x>y\sqrt{D}$ không còn đúng. Khi đó xét phương trình $y^2-\dfrac{1}{D}x^2=-\dfrac{N}{D}$. Vì $\dfrac{|N|}{D}<\sqrt{\dfrac{1}{D}}$, lập luận trên vẫn đúng. Do đó $\dfrac{y}{x}$ là phân số liên tục của $\dfrac{1}{\sqrt{D}}$. Theo [định lý đảo](./continued-fraction.md#递推关系), suy ra $\dfrac{x}{y}$ cũng là phân số liên tục của $\sqrt{D}$.

???+ note "Định lý"
    Khi chạy PQa với $(P_0,Q_0,D)=(0,1,D)$, nếu $Q_k=1$ thì обязательно $\ell\mid k$.

??? note "Chứng minh"
    Trong khai triển của $\sqrt{D}$, ngoài dư thứ $0$, các dư khác đều là [phân số liên tục thuần chu kỳ](./continued-fraction.md#纯循环连分数). Giả sử $Q_k=1$. Theo kết quả của Galois, dư $\omega_k=P_k+\sqrt{D}>1$ và liên hợp của nó thỏa $-1<P_k-\sqrt{D}<0$, suy ra $P_k=\lfloor\sqrt{D}\rfloor$. Do đó $\omega_k=\omega_\ell$. Lặp lại dư nghĩa là bắt đầu chu kỳ; nếu $k$ không là bội của $\ell$ thì mâu thuẫn với việc $\ell$ là chu kỳ nhỏ nhất. Vậy $\ell\mid k$.

Tổng hợp lại: chỉ cần khai triển $\sqrt{D}$ bằng PQa với $(0,1,D)$, khi lần đầu có $Q_\ell=1$ thì đã tới cuối chu kỳ đầu. Nếu $\ell$ chẵn thì $(A_{\ell-1},B_{\ell-1})$ là nghiệm cơ bản; nếu $\ell$ lẻ thì $(A_{2\ell-1},B_{2\ell-1})$ là nghiệm cơ bản. Với $\ell$ lẻ, không cần chạy PQa đến hai chu kỳ; sẽ thấy $A_{2\ell-1}+B_{2\ell-1}\sqrt{D}=(A_{\ell-1}+B_{\ell-1}\sqrt{D})^2$, nên có thể từ $(A_{\ell-1},B_{\ell-1})$ trực tiếp tính nghiệm cơ bản. Các nghiệm còn lại được sinh từ nghiệm cơ bản.

??? example "Ví dụ"
    1.  Giải phương trình $x^2-14y^2=1$.
    
        Chạy PQa với $(P_0,Q_0,D)=(0,1,14)$ thu được: (phần tô đỏ là chu kỳ đầu)
    
        | $k$ | $P$ | $Q$ |        $a$       |  $A$  |  $B$ |  $G$  | $G^2-DB^2$ |
        | :-: | :-: | :-: | :--------------: | :---: | :--: | :---: | :--------: |
        | $0$ | $0$ | $1$ |        $3$       |  $3$  |  $1$ |  $3$  |    $-5$    |
        | $1$ | $3$ | $5$ | $\color{red}{1}$ |  $4$  |  $1$ |  $4$  |     $2$    |
        | $2$ | $2$ | $2$ | $\color{red}{2}$ |  $11$ |  $3$ |  $11$ |    $-5$    |
        | $3$ | $2$ | $5$ | $\color{red}{1}$ |  $15$ |  $4$ |  $15$ |     $1$    |
        | $4$ | $3$ | $1$ | $\color{red}{6}$ | $101$ | $27$ | $101$ |    $-5$    |
        | $5$ | $3$ | $5$ |        $1$       | $116$ | $31$ | $116$ |     $2$    |
    
        Độ dài chu kỳ $\ell=4$ chẵn. Nghiệm nguyên dương nhỏ nhất là $(G_3,B_3)=(15,4)$.
    2.  Giải phương trình $x^2-41y^2=1$.
    
        Chạy PQa với $(P_0,Q_0,D)=(0,1,41)$ thu được: (phần tô đỏ là chu kỳ đầu)
    
        | $k$ | $P$ | $Q$ |        $a$        |   $A$   |   $B$  |   $G$   | $G^2-DB^2$ |
        | :-: | :-: | :-: | :---------------: | :-----: | :----: | :-----: | :--------: |
        | $0$ | $0$ | $1$ |        $6$        |   $6$   |   $1$  |   $6$   |    $-5$    |
        | $1$ | $6$ | $5$ |  $\color{red}{2}$ |   $13$  |   $2$  |   $13$  |     $5$    |
        | $2$ | $4$ | $5$ |  $\color{red}{2}$ |   $32$  |   $5$  |   $32$  |    $-1$    |
        | $3$ | $6$ | $1$ | $\color{red}{12}$ |  $397$  |  $62$  |  $397$  |     $5$    |
        | $4$ | $6$ | $5$ |        $2$        |  $826$  |  $129$ |  $826$  |    $-5$    |
        | $5$ | $4$ | $5$ |        $2$        |  $2049$ |  $320$ |  $2049$ |     $1$    |
        | $6$ | $6$ | $1$ |        $12$       | $25414$ | $3969$ | $25414$ |    $-5$    |
        | $7$ | $6$ | $5$ |        $2$        | $52877$ | $8258$ | $52877$ |     $5$    |
    
        Độ dài chu kỳ $\ell=3$ lẻ. Nghiệm nguyên dương nhỏ nhất là $(G_5,B_5)=(2049,320)$. Ta cũng có thể tính từ $(G_2,B_2)=(32,5)$:
    
        $$
        (32+5\sqrt{41})^2=2049+320\sqrt{41}.
        $$

### Phương trình Pell âm

Theo phần trước, nghiệm của phương trình Pell âm cũng tương ứng với các phân số liên tục của $\sqrt{D}$, và chỉ có thể xuất hiện tại vị trí $(-1)^kQ_k=-1$, tức là ở cuối chu kỳ. Do đó, phương trình Pell âm có nghiệm khi và chỉ khi độ dài chu kỳ $\ell$ là lẻ. Khi có nghiệm, $(A_{\ell-1},B_{\ell-1})$ là nghiệm cơ bản. Cách giải giống phần trước.

Dùng lập luận tương tự phần cấu trúc nghiệm của Pell, ta có:

???+ note "Định lý"
    Giả sử phương trình $x^2-Dy^2=-1$ có nghiệm, nghiệm cơ bản là $(x_1,y_1)$. Khi đó, toàn bộ nghiệm nguyên của $x^2-Dy^2=\pm 1$ thuộc
    
    $$
    \{(x,y):x+y\sqrt{D}=\pm(x_1+y_1\sqrt{D})^k,k\in\mathbf Z\}.
    $$
    
    Đặc biệt, nghiệm $(x_2,y_2)$ thỏa $x_2+y_2\sqrt{D}=(x_1+y_1\sqrt{D})^2$ chính là nghiệm cơ bản của $x^2-Dy^2=1$.

??? note "Chứng minh"
    Do đối xứng, chỉ cần xét nghiệm nguyên dương, tức $x+y\sqrt{D}>1$. Nhưng vì $x^2-Dy^2=\pm 1$ gồm hai nhánh hyperbol, $x+y\sqrt{D}$ không còn tương ứng một-một với $(x,y)$. Để xử lý, trước hết chứng minh $(x_2,y_2)$ là nghiệm cơ bản của $x^2-Dy^2=1$.
    
    Rõ ràng $(x_2,y_2)$ là nghiệm của $x^2-Dy^2=1$. Nếu $(z,w)$ là nghiệm cơ bản của $x^2-Dy^2=1$ thì $1<z+w\sqrt{D}\le x_2+y_2\sqrt{D}$. Nếu bất đẳng thức bên phải là строг, chia cho $x_1+y_1\sqrt{D}$ ta được $-x_1+y_1\sqrt{D}<(z+w\sqrt{D})(-x_1+y_1\sqrt{D})<x_1+y_1\sqrt{D}$. Khai triển hạng giữa thành $x'+y'\sqrt{D}$, ta có chuẩn $-1$ và $(x',y')$ nguyên. Lấy nghịch đảo bất đẳng thức, ta thấy $-x'+y'\sqrt{D}$ cũng nằm giữa $-x_1+y_1\sqrt{D}$ và $x_1+y_1\sqrt{D}$. Hai số nguyên bậc hai $\pm x'+y'\sqrt{D}$ là nghịch đảo của nhau, nên một trong hai lớn hơn $1$. Nhưng giữa $1$ và $x_1+y_1\sqrt{D}$ không được có số nguyên bậc hai chuẩn $-1$ khác, mâu thuẫn với tính nhỏ nhất của $x_1+y_1\sqrt{D}$. Do đó phải có $x_2+y_2\sqrt{D}=z+w\sqrt{D}$, tức $(x_2,y_2)$ là nghiệm cơ bản của $x^2-Dy^2=1$.
    
    Dựa vào đó, nếu có nghiệm $(x,y)$ của $x^2-Dy^2=\pm 1$ không tương ứng với $(x_1+y_1\sqrt{D})^k$, thì tồn tại $k$ sao cho $(x_1+y_1\sqrt{D})^{2k}<x+y\sqrt{D}<(x_1+y_1\sqrt{D})^{2k+2}$. Khử nhân tử $(x_1+y_1)^{2k+1}$, ta thu được số nguyên bậc hai chuẩn $\pm 1$ nằm giữa $-x_1+y_1\sqrt{D}$ và $x_1+y_1\sqrt{D}$ khác $1$, mâu thuẫn như trên. Suy ra mệnh đề đúng.

Vì $(A_{\ell-1},B_{\ell-1})$ là nghiệm dương nhỏ nhất của phương trình Pell âm, và mọi nghiệm dương của $x^2-Dy^2=\pm 1$ đều thuộc tập

$$
\{(x,y):x+y\sqrt{D}=(A_{\ell-1}+B_{\ell-1}\sqrt{D})^k,k\in\mathbf N_+\}
$$

mà các nghiệm dương này tương ứng với các phân số liên tục ở cuối chu kỳ của $\sqrt{D}$, với tử mẫu tăng строго, nên với mọi $k\in\mathbf N_+$ ta có

$$
(A_{\ell-1}+B_{\ell-1}\sqrt{D})^k = A_{k\ell-1}+B_{k\ell-1}\sqrt{D}.
$$

Trong các nghiệm dương, $k$ lẻ thì là nghiệm Pell âm, $k$ chẵn thì là nghiệm Pell dương; hai loại xen kẽ.

Việc判断 phương trình Pell âm có nghiệm đòi hỏi biết độ dài chu kỳ của $\sqrt{D}$, không dễ tính, nên mong muốn có tiêu chuẩn đơn giản hơn. Tuy vậy, hiện chưa có điều kiện ngắn gọn và dễ tính[^solubility-neg-pell]. Ở đây chỉ nêu một kết luận đơn giản.

???+ note "Định lý"
    Nếu phương trình $x^2-Dy^2=-1$ có nghiệm thì $4$ không chia $D$ và $D$ không có thừa số nguyên tố dạng $4k+3$. Ngược lại, nếu $D=2$ hoặc $D$ là số nguyên tố dạng $4k+1$, thì phương trình chắc chắn có nghiệm.

??? note "Chứng minh"
    Nếu Pell âm có nghiệm thì $-1$ là thặng dư bậc hai của $D$, do đó $-1$ là thặng dư bậc hai của mọi ước $d$ của $D$, suy ra $d\neq 4$ và $d$ không là nguyên tố dạng $4k+3$. Ngược lại, phương trình $x^2-2y^2=-1$ có nghiệm không tầm thường $(1,1)$. Còn lại là trường hợp $D$ nguyên tố dạng $4k+1$.
    
    Giả sử $D$ nguyên tố dạng $4k+1$. Ta chứng minh $x^2-Dy^2=-1$ có nghiệm bằng cách xuất phát từ nghiệm cơ bản $(u,v)$ của $x^2-Dy^2=1$ để dựng nghiệm $(\alpha,\beta)$ của $x^2-Dy^2=-1$. Nếu $u$ chẵn, lấy modulo $4$ từ $u^2-Dv^2=1$ được $v^2\equiv -1\pmod 4$, nhưng $-1$ không là thặng dư bậc hai modulo $4$, mâu thuẫn. Vậy $u$ lẻ. Xét $Dv^2=u^2-1=(u+1)(u-1)$. Vì $u$ lẻ nên $\gcd(u+1,u-1)=\gcd(u+1,2)=2$. Từ đó, phân chia các thừa số của $Dv^2$ vào $u+1$ và $u-1$, ta được một bên là $2\alpha^2$, bên kia là $2D\beta^2$, với $\alpha,\beta$ nguyên dương nguyên tố cùng nhau và $v=2\alpha\beta$. Thay $u=\alpha^2+D\beta^2$, $v=2\alpha\beta$ vào $u^2-Dv^2=1$ ta được $\alpha^2-D\beta^2=\pm 1$. Vì $(u,v)$ là nghiệm cơ bản và $(\alpha,\beta)$ nhỏ hơn, nên vế phải không thể là $+1$, do đó phải là $-1$. Vậy $x^2-Dy^2=-1$ có nghiệm $(\alpha,\beta)$.

Nếu $D$ hợp số thì việc không có thừa số dạng $4k+3$ và không có thừa số bình phương vẫn không đảm bảo có nghiệm, ví dụ $x^2-34y^2=-1$ vô nghiệm.

??? example "Ví dụ"
    Dựa vào các tính toán trên, phương trình $x^2-14y^2=-1$ vô nghiệm, và phương trình $x^2-41y^2=-1$ có nghiệm dương nhỏ nhất $(G_2,B_2)=(32,5)$.

### Trường hợp chuẩn bằng ±4

Tiếp theo xét phương trình $x^2-Dy^2=\pm 4$. Lúc này tính chất nghiệm phụ thuộc vào $D\bmod 4$.

Một số trường hợp dễ. Nếu $D\equiv 0\pmod 4$, thì $x$ chẵn, và $(x/2,y)$ là nghiệm của $u^2-(D/4)v^2=\pm 1$. Các trường hợp khác, $x,y$ либо cùng lẻ hoặc cùng chẵn. Nếu $x,y$ đều lẻ, lấy modulo $4$ cho hai vế được $D\equiv 1\pmod 4$. Do đó, nếu $D\equiv 2,3\pmod 4$ thì $x,y$ phải cùng chẵn, và $(x/2,y/2)$ là nghiệm của $u^2-Dv^2=\pm 1$. Vì vậy, trừ trường hợp $D\equiv 1\pmod 4$, nghiệm của $x^2-Dy^2=\pm 4$ đều có thể suy ra từ nghiệm của (âm) Pell tương ứng.

Bây giờ xét $D\equiv 1\pmod 4$, không thể giản lược như trên. Để tìm nghiệm cơ bản, chạy PQa với $(P_0,Q_0,D)=(1,2,D)$, khi lần đầu có $Q_\ell=2$ thì tới cuối chu kỳ đầu. Nếu độ dài chu kỳ $\ell$ chẵn, thì $(G_{\ell-1},B_{\ell-1})$ là nghiệm cơ bản của $x^2-Dy^2=4$; nếu $\ell$ lẻ, thì $(G_{\ell-1},B_{\ell-1})$ là nghiệm cơ bản của $x^2-Dy^2=-4$. Từ $(G_{\ell-1},B_{\ell-1})$ có thể sinh toàn bộ nghiệm của $x^2-Dy^2=\pm 4$:

$$
\left\{(x,y):\dfrac{x+y\sqrt{D}}{2}=\pm\left(\dfrac{G_{\ell-1}+B_{\ell-1}\sqrt{D}}{2}\right)^k,k\in\mathbf Z\right\}.
$$

Nếu $\ell$ chẵn, tất cả đều là nghiệm của $x^2-Dy^2=4$; nếu $\ell$ lẻ, khi $k$ lẻ thì $(x,y)$ là nghiệm của $x^2-Dy^2=-4$, khi $k$ chẵn thì là nghiệm của $x^2-Dy^2=4$.

Tính đúng đắn dựa vào các факты sau:

???+ note "Định lý"
    Giả sử phương trình $x^2-Dy^2=\pm 4$ có nghiệm dương $(x,y)$. Nếu $D\equiv 1\pmod 4$, thì $\dfrac{(x+y)/2}{y}$ là một phân số liên tục của $\dfrac{1+\sqrt{D}}{2}$.

??? note "Chứng minh"
    Lưu ý $x,y$ đồng parity, nên $(x+y)/2$ là整数. Nếu $(x,y)$ là nghiệm của $x^2-Dy^2=4$ thì $x>y\sqrt{D}>2y$, nên
    
    $$
    \left|\dfrac{(x+y)/2}{y}-\dfrac{1+\sqrt{D}}{2}\right| = \dfrac{2}{y(x+y\sqrt{D})}<\dfrac{1}{2y^2}.
    $$
    
    Theo [tiêu chuẩn Legendre](./continued-fraction.md#渐近分数的判定), suy ra $\dfrac{(x+y)/2}{y}$ là phân số liên tục của $\dfrac{1+\sqrt{D}}{2}$.
    
    Nếu $(x,y)$ là nghiệm của $x^2-Dy^2=-4$, để có bất đẳng thức trên chỉ cần chứng minh $4y<x+y\sqrt{D}$. Điều này đúng với mọi trường hợp trừ $D=5,13$. Với $D=5,13$, thay $x=\sqrt{Dy^2-4}$ vào bất đẳng thức, thấy tương đương với $2(\sqrt{D}-2)y^2>1$. Trừ $(D,y)=(5,1)$, bất đẳng thức đúng cho mọi $D=5,13$ và $y$ dương. Còn lại kiểm tra $(D,y)=(5,1)$, khi đó nghiệm của $x^2-5y^2=-4$ là $(1,1)$, cần kiểm tra $\dfrac{1}{1}$ là phân số liên tục của $\dfrac{1+\sqrt{5}}{2}=[\overline{1}]$, điều này hiển nhiên.

???+ note "Định lý"
    Với $D$ dương không là chính phương, số vô tỉ bậc hai $\omega=\dfrac{1+\sqrt{D}}{2}$ có khai triển phân số liên tục
    
    $$
    \omega = [\lfloor\omega\rfloor,\overline{a_1,\cdots,a_{\ell-1},2\lfloor\omega\rfloor-1}],
    $$
    
    trong đó $\ell$ là độ dài chu kỳ và $a_k=a_{\ell-k}$ với mọi $1<k<\ell$.

??? note "Chứng minh"
    Do $\lfloor\omega\rfloor-1+\omega>1$ và liên hợp của nó bằng $\lfloor\omega\rfloor - \omega$ nằm giữa $-1$ và $0$, theo [kết luận của Galois](./continued-fraction.md#纯循环连分数), ta có
    
    $$
    \lfloor\omega\rfloor-1+\omega = [\overline{2\lfloor\omega\rfloor-1,a_1,\cdots,a_{\ell-1}}].
    $$
    
    Còn kết luận của Galois về nghịch đảo liên hợp âm cho
    
    $$
    \dfrac{1}{\omega-\lfloor\omega\rfloor} = [\overline{a_{\ell-1},\cdots,a_1,2\lfloor\omega\rfloor-1}].
    $$
    
    Do đó,
    
    $$
    \lfloor\omega\rfloor-1+\omega = 2\lfloor\omega\rfloor-1 + \dfrac{1}{\dfrac{1}{\omega-\lfloor\omega\rfloor}} = [2\lfloor\omega\rfloor-1,\overline{a_{\ell-1},\cdots,a_1,2\lfloor\omega\rfloor-1}].
    $$
    
    Tính duy nhất của khai triển phân số liên tục suy ra $a_k=a_{\ell-k}$ với mọi $1<k<\ell$, nên dạng khai triển cần chứng minh đúng.

???+ note "Định lý"
    Giả sử $D\equiv 1\pmod 4$. Khi chạy PQa với $(P_0,Q_0,D)=(1,2,D)$, nếu $Q_k=2$ thì nhất định $\ell\mid k$.

??? note "Chứng minh"
    Trong khai triển của $\dfrac{1+\sqrt{D}}{2}$, ngoài dư thứ $0$, các dư còn lại đều là [phân số liên tục thuần chu kỳ](./continued-fraction.md#纯循环连分数). Giả sử $Q_k=2$. Theo kết luận của Galois, liên hợp của $\omega_k=\dfrac{P_k+\sqrt{D}}{2}$ thỏa $-1<\dfrac{P_k-\sqrt{D}}{2}<0$, tức $\sqrt{D}-2<P_k<\sqrt{D}$. Do trong PQa luôn có $Q_k\mid P_k^2-D$ (xem [chứng minh đúng đắn](./continued-fraction.md#二次无理数)), nên $P_k$ phải là số lẻ, suy ra giá trị $P_k$ là duy nhất: $P_k=P_0+2(\lfloor\omega\rfloor-1)$, tức $\omega_k=\omega_\ell$. Dư lặp lại nghĩa là bắt đầu chu kỳ; nếu $k$ không là bội của $\ell$ thì mâu thuẫn với tính tối tiểu của chu kỳ. Do đó $\ell\mid k$.

???+ note "Định lý"
    Giả sử nghiệm nguyên dương nhỏ nhất của $x^2-Dy^2=\pm 4$ là $(x_1,y_1)$. Khi đó, toàn bộ nghiệm là
    
    $$
    \left\{(x,y):\dfrac{x+y\sqrt{D}}{2}=\pm\left(\dfrac{x_1+y_1\sqrt{D}}{2}\right)^k,k\in\mathbf Z\right\}.
    $$

??? note "Chứng minh"
    Do đối xứng, chỉ cần xét nghiệm dương $(x,y)$. Ở đây chỉ cần chứng minh các cặp thực trong tập trên thực sự là nghiệm nguyên của $x^2-Dy^2=\pm 4$. Phần còn lại lặp lại chứng minh cấu trúc nghiệm của $x^2-Dy^2=\pm 1$.
    
    Cụ thể, cần chứng minh rằng với hai nghiệm $(x_1,y_1)$ và $(x_2,y_2)$ của $x^2-Dy^2=\pm 4$, cặp thực dương $(x_3,y_3)$ sau vẫn là nghiệm nguyên:
    
    $$
    \dfrac{x_3+y_3\sqrt{D}}{2} = \dfrac{x_1+y_1\sqrt{D}}{2}\dfrac{x_2+y_2\sqrt{D}}{2}.
    $$
    
    Khai triển, so sánh hệ số được
    
    $$
    x_3=\dfrac{x_1x_2+Dy_1y_2}{2},\ y_3=\dfrac{x_1y_2+x_2y_1}{2}.
    $$
    
    Vì $x_i\equiv x_i^2\equiv Dy_i^2\equiv Dy_i\pmod 2$, nên
    
    $$
    \begin{aligned}
    2x_3 &= x_1x_2+Dy_1y_2 \equiv D^2y_1y_2+Dy_1y_2=D(D+1)y_1y_2 \equiv 0 \pmod 2,\\
    2y_3 &= x_1y_2+x_2y_1 \equiv Dy_1y_2+Dy_2y_1 = 2Dy_1y_2 \equiv 0 \pmod 2.
    \end{aligned}
    $$
    
    Do đó $x_3,y_3$ là số nguyên. Kết hợp tính nhân của chuẩn, $(x_3,y_3)$ là nghiệm của $x^2-Dy^2=\pm 4$.

Từ đó, lặp lại lập luận các mục trước, ta chứng minh được độ đúng của thuật toán giải $x^2-Dy^2=\pm 4$. Các kết quả này cho thấy $x^2-Dy^2=\pm 4$ có cấu trúc nghiệm đơn giản tương tự $x^2-Dy^2=\pm 1$: mọi nghiệm đều biểu diễn qua nghiệm dương nhỏ nhất, không cần giải thêm phương trình khác.

Thực ra, mọi nghiệm của $x^2-Dy^2=\pm 1$ đều có thể tìm thấy trong nghiệm của $x^2-Dy^2=\pm 4$. Rõ ràng, $(x,y)$ là nghiệm của $x^2-Dy^2=\pm 1$ khi và chỉ khi $(2x,2y)$ là nghiệm của $x^2-Dy^2=\pm 4$. Phân tích trước cho thấy khi $D\equiv 2,3\pmod 4$, mọi nghiệm của $x^2-Dy^2=\pm 4$ đều chẵn, tương ứng với nghiệm của $x^2-Dy^2=\pm 1$.

Khi $D\equiv 0\pmod 4$, nghiệm $(x,y)$ của $x^2-Dy^2=\pm 4$ có $x$ chẵn nhưng $y$ có thể lẻ. Nếu trong nghiệm dương nhỏ nhất $(x_1,y_1)$ mà $y_1$ chẵn, thì mọi nghiệm đều có $y$ chẵn, và các nghiệm tương ứng một-một với nghiệm của $x^2-Dy^2=\pm 1$. Nhưng nếu $y_1$ lẻ, thì tính chẵn lẻ của $y_k$ sẽ đổi theo $k$, và chỉ khi $k$ chẵn mới tương ứng với nghiệm của $x^2-Dy^2=\pm 1$. Nếu nghiệm dương nhỏ nhất của $x^2-Dy^2=\pm 4$ có $y_1$ lẻ và chuẩn của $x_1+y_1\sqrt{D}$ là $-4$, thì với $D$ đó, $x^2-Dy^2=-4$ có nghiệm nhưng $x^2-Dy^2=-1$ vô nghiệm.

Khi $D\equiv 1\pmod 4$, nghiệm $(x,y)$ của $x^2-Dy^2=\pm 4$ có thể đồng thời lẻ hoặc đồng thời chẵn. Nếu nghiệm dương nhỏ nhất $(x_1,y_1)$ đã đồng thời chẵn, thì mọi nghiệm cũng đồng thời chẵn, nên luôn tương ứng với nghiệm của $x^2-Dy^2=\pm 1$. Nếu nghiệm dương nhỏ nhất $(x_1,y_1)$ đồng thời lẻ, ta có kết luận sau:

???+ note "Định lý"
    Giả sử nghiệm dương nhỏ nhất của $x^2-Dy^2=\pm 4$ là $(x_1,y_1)$. Nếu $x_1$ và $y_1$ đều lẻ, thì $D\equiv 5\pmod 8$, và nghiệm $(x,y)$ là đồng thời chẵn khi và chỉ khi
    
    $$
    \dfrac{x+y\sqrt{D}}{2} = \pm\left(\dfrac{x_1+y_1\sqrt{D}}{2}\right)^{3k},k\in\mathbf Z.
    $$

??? note "Chứng minh"
    Lấy modulo $8$ hai vế của $x_1^2-Dy_1^2=\pm 4$ được $D\equiv 5\pmod 8$. Để chứng minh kết luận thứ hai, trước hết chứng minh $(x_3,y_3)$ đều chẵn, vì
    
    $$
    \dfrac{x_3+y_3\sqrt{D}}{2} = \left(\dfrac{x_1+y_1\sqrt{D}}{2}\right)^{3} = \dfrac{x_1^3+3Dxy_1^2}{8}+\dfrac{3x_1^2y_1+Dy_1^3}{8}\sqrt{D},
    $$
    
    nên chỉ cần chứng minh vế phải là số nguyên. Vì bình phương số lẻ modulo $8$ dư $1$, ta có
    
    $$
    \begin{aligned}
    &x_1^3+3Dxy_1 = x_1(x_1^2+3Dy_1) \equiv x_1(1+3\times 5\times 1) = 16x_1 = 0 \pmod 8,\\
    &3x_1^2y_1+Dy_1^3 = y_1(3x_1^2+Dy_1^2) \equiv y_1(3\times 1+5\times 1) = 8y_1 = 0 \pmod 8.
    \end{aligned}
    $$
    
    Suy ra $x_3,y_3$ đều chẵn. Do đó với mọi $k\in\mathbf Z$, ta có
    
    $$
    \dfrac{x+y\sqrt{D}}{2} = \pm\left(\dfrac{x_1+y_1\sqrt{D}}{2}\right)^{3k} = \pm\left(\dfrac{x_3+y_3\sqrt{D}}{2}\right)^k \in \mathbf Z,
    $$
    
    nên $(x,y)$ đều chẵn. Ngược lại, với $r=1,2$ luôn có
    
    $$
    \pm\left(\dfrac{x_1+y_1\sqrt{D}}{2}\right)^{3k+r} = \pm\left(\dfrac{x_3+y_3\sqrt{D}}{2}\right)^k\left(\dfrac{x_r+y_r\sqrt{D}}{2}\right).
    $$
    
    Để chứng minh $(x,y)$ không nguyên, chỉ cần chứng minh tích bên phải không nguyên, tức hạng thứ hai không nguyên. Với $r=1$ thì đúng theo giả thiết. Với $r=2$, vì
    
    $$
    \dfrac{x_2+y_2\sqrt{D}}{2} = \left(\dfrac{x_1+y_1\sqrt{D}}{2}\right)^2 = \dfrac{x_1^2+Dy_1^2}{4} + \dfrac{x_1y_1}{2}\sqrt{D},
    $$
    
    và $x_1^2+Dy_1^2\equiv 1+1\times 1=2\pmod 4$, $x_1y_1\equiv 1\pmod 2$, nên biểu thức này cũng không nguyên. Vậy chỉ khi số mũ là bội của $3$ thì nghiệm mới đồng thời chẵn.

Nói cách khác, cứ mỗi ba nghiệm của $x^2-Dy^2=\pm 4$ thì có một nghiệm đồng thời chẵn, tương ứng với nghiệm của $x^2-Dy^2=\pm 1$. Điều này cũng cho thấy với $D\equiv 1\pmod 4$, $x^2-Dy^2=-4$ có nghiệm khi và chỉ khi $x^2-Dy^2=-1$ có nghiệm.

Đến đây, đã đủ để tính đơn vị cơ bản của vành số nguyên bậc hai thực. Giả sử $D$ là số nguyên dương không chứa thừa số bình phương. Với $D\equiv 2,3\pmod 4$, chỉ cần tìm nghiệm dương nhỏ nhất của $x^2-Dy^2=\pm 1$; với $D\equiv 1\pmod 4$, chỉ cần tìm nghiệm dương nhỏ nhất của $x^2-Dy^2=\pm 4$. Khi tìm được nghiệm dương nhỏ nhất $(x,y)$, thì với $D\equiv 2,3\pmod 4$, đơn vị cơ bản là $\pm x\pm y\sqrt{D}$; với $D\equiv 1\pmod 4$, đơn vị cơ bản là $\dfrac{\pm x\pm y\sqrt{D}}{2}$.

??? example "Ví dụ"
    1.  Giải phương trình $x^2-14y^2=\pm 4$.
    
        Dựa vào các ví dụ trước, phương trình $x^2-14y^2=4$ có nghiệm dương nhỏ nhất $(30,8)$, còn $x^2-14y^2=-4$ vô nghiệm.
    2.  Giải phương trình $x^2-41y^2=\pm 4$.
    
        Chạy PQa với $(P_0,Q_0,D)=(1,2,41)$ được: (phần tô đỏ là chu kỳ đầu)
    
        |  $k$ | $P$ | $Q$ |        $a$       |   $A$   |   $B$  |   $G$   | $G^2-DB^2$ |
        | :--: | :-: | :-: | :--------------: | :-----: | :----: | :-----: | :--------: |
        |  $0$ | $1$ | $2$ |        $3$       |   $3$   |   $1$  |   $5$   |    $-16$   |
        |  $1$ | $5$ | $8$ | $\color{red}{1}$ |   $4$   |   $1$  |   $7$   |     $8$    |
        |  $2$ | $3$ | $4$ | $\color{red}{2}$ |   $11$  |   $3$  |   $19$  |    $-8$    |
        |  $3$ | $5$ | $4$ | $\color{red}{2}$ |   $26$  |   $7$  |   $45$  |    $16$    |
        |  $4$ | $3$ | $8$ | $\color{red}{1}$ |   $37$  |  $10$  |   $64$  |    $-4$    |
        |  $5$ | $5$ | $2$ | $\color{red}{5}$ |  $211$  |  $57$  |  $365$  |    $16$    |
        |  $6$ | $5$ | $8$ |        $1$       |  $248$  |  $67$  |  $429$  |    $-8$    |
        |  $7$ | $3$ | $4$ |        $2$       |  $707$  |  $191$ |  $1223$ |     $8$    |
        |  $8$ | $5$ | $4$ |        $2$       |  $1662$ |  $449$ |  $2875$ |    $-16$   |
        |  $9$ | $3$ | $8$ |        $1$       |  $2369$ |  $640$ |  $4098$ |     $4$    |
        | $10$ | $5$ | $2$ |        $5$       | $13507$ | $3649$ | $23365$ |    $-16$   |
        | $11$ | $5$ | $8$ |        $1$       | $15876$ | $4289$ | $27463$ |     $8$    |
    
        Chu kỳ $\ell=5$ lẻ. Nghiệm dương nhỏ nhất của $x^2-41y^2=-4$ là $(G_4,B_4)=(64,10)$, còn nghiệm dương nhỏ nhất của $x^2-41y^2=4$ là $(G_9,B_9)=(4098,640)$. Chúng có quan hệ:
    
        $$
        \dfrac{4098+640\sqrt{41}}{2} = \left(\dfrac{64+10\sqrt{41}}{2}\right)^2.
        $$
    
        Dĩ nhiên, vì $D\equiv 1\pmod 8$, theo kết luận trước thì nghiệm dương nhỏ nhất của $x^2-41y^2=\pm 4$ đều chẵn và luôn là 2 lần nghiệm dương nhỏ nhất của $x^2-41y^2=\pm 1$, nên cũng có thể suy ra trực tiếp từ ví dụ trước.
    3.  Giải phương trình $x^2-13y^2=\pm 4$.
    
        Chạy PQa với $(P_0,Q_0,D)=(1,2,13)$ được: (phần tô đỏ là chu kỳ đầu)
    
        | $k$ | $P$ | $Q$ |        $a$       |  $A$ |  $B$ |  $G$  | $G^2-DB^2$ |
        | :-: | :-: | :-: | :--------------: | :--: | :--: | :---: | :--------: |
        | $0$ | $1$ | $2$ |        $2$       |  $2$ |  $1$ |  $3$  |    $-4$    |
        | $1$ | $3$ | $2$ | $\color{red}{3}$ |  $7$ |  $3$ |  $11$ |     $4$    |
        | $2$ | $3$ | $2$ |        $3$       | $23$ | $10$ |  $36$ |    $-4$    |
        | $3$ | $3$ | $2$ |        $3$       | $74$ | $33$ | $119$ |     $4$    |
    
        Chu kỳ $\ell=1$ lẻ. Nghiệm dương nhỏ nhất của $x^2-13y^2=-4$ là $(G_0,B_0)=(3,1)$, còn nghiệm dương nhỏ nhất của $x^2-13y^2=4$ là $(G_1,B_1)=(11,3)$.
    
        Vì các nghiệm dương nhỏ nhất đều lẻ, có thể dùng cặp $(G_2,B_2)=(36,10)$ ở cuối chu kỳ thứ ba để lấy nghiệm dương nhỏ nhất của (âm) Pell $x^2-13y^2=\pm 1$ là $(18,5)$. Ta cũng có thể tính trực tiếp:
    
        $$
        \dfrac{36+10\sqrt{13}}{2}=\left(\dfrac{3+\sqrt{13}}{2}\right)^3.
        $$
    
        Hơn nữa, đây là nghiệm của phương trình Pell âm. Nghiệm dương nhỏ nhất của Pell dương là $(649,180)$.
    4.  Giải phương trình $x^2-52y^2=\pm 4$.
    
        Vì nghiệm dương nhỏ nhất của $x^2-13y^2=\pm 1$ lần lượt là $(18,5)$ và $(649,180)$, nên nghiệm dương nhỏ nhất của $x^2-52y^2=\pm 4$ lần lượt là $(36,10)$ và $(1298,360)$.

### Trường hợp tổng quát

Cuối cùng, bàn về cách giải phương trình Pell tổng quát.

Với $|N|<\sqrt{D}$ có một cách giải đơn giản. Kết luận trước cho thấy nghiệm $(x,y)$ của $x^2-Dy^2=N$ phải có $\dfrac{x}{y}$ là một phân số liên tục của $\sqrt{D}$. Hơn nữa, theo cấu trúc nghiệm đã bàn, mỗi nghiệm cơ bản $(x,y)$ đều thỏa $x+y\sqrt{D}$ không vượt quá nghiệm cơ bản $x_1+y_1\sqrt{D}$ của $x^2-Dy^2=1$. Dùng tính đơn điệu của dãy mẫu $B_k$ trong PQa, các nghiệm cơ bản này phải xuất hiện trước khi nghiệm cơ bản của Pell xuất hiện. Vì vậy, chỉ cần chạy PQa với $(P_0,Q_0,D)=(0,1,D)$ cho đến khi $Q_{\ell'}=1$ và $\ell'$ chẵn; trong quá trình, với mỗi $(A_k,B_k)$ xuất hiện, kiểm tra có tồn tại số nguyên $f$ sao cho

$$
A_k^2-DB_k^2 = (-1)^{k+1}Q_{k+1} = N/f^2
$$

hay không. Nếu có, ghi lại $(fA_{k},fB_{k})$ là một nghiệm dương nhỏ nhất. Tập các $(fA_k,fB_k)$ thu được là toàn bộ nghiệm dương nhỏ nhất của $x^2-Dy^2=N$. Dùng $(A_{\ell'-1},B_{\ell'-1})$ (tức nghiệm cơ bản của Pell tương ứng) có thể sinh ra toàn bộ nghiệm. Lưu ý tùy theo $\ell$ chẵn hay lẻ, $\ell'$ có thể là $\ell$ hoặc $2\ell$.

Với $N$ tổng quát hơn, cách trên không còn dùng được. Trước hết, liệt kê mọi ước bình phương $f^2$ của $N$, đặt $m=N/f^2$, và liệt kê mọi nghiệm $z$ của đồng dư $z^2\equiv D\pmod{|m|}$ thỏa $-|m|/2<z \le |m|/2$. Sau đó chạy PQa với $(P_0,Q_0,D)=(z,|m|,D)$ cho đến khi $Q_k=\pm 1$ hoặc đã kết thúc một chu kỳ. Nếu rơi vào trường hợp thứ hai, thì không có nghiệm liên quan đến cặp $(f,z)$ đó. Nếu rơi vào trường hợp thứ nhất, cần kiểm tra $(-1)^kQ_k=N/|N|$ hay không. Nếu trùng dấu, thì $(fG_{k-1},fB_{k-1})$ là nghiệm của $x^2-Dy^2=N$. Nếu không, thì đó là nghiệm của $x^2-Dy^2=-N$, và chỉ khi phương trình Pell âm tương ứng có nghiệm thì mới có thể dùng phép nhân với nghiệm cơ bản của Pell âm để tạo ra nghiệm của $x^2-Dy^2=N$. Hoàn tất duyệt mọi $(f,z)$, ta thu được đúng một nghiệm trong mỗi lớp tương đương, và nghiệm đó là nghiệm cơ bản hoặc nghiệm dương nhỏ nhất của lớp. Dùng các nghiệm đó cùng nghiệm cơ bản Pell, ta sinh ra toàn bộ nghiệm. Thuật toán này gọi là **thuật toán Lagrange–Matthews–Mollin**.

Độ đúng của thuật toán được đảm bảo bởi định lý sau:

???+ note "Định lý"
    Giả sử phương trình $x^2-Dy^2=N$ có nghiệm nguyên $(x,y)$ với $x\ge 0, y>0,\gcd(x,y)=1$. Đặt $Q_0=|N|$, khi đó $\gcd(Q_0,y)=1$. Gọi $P_0$ là nghiệm của đồng dư $x\equiv -P_0y\pmod{Q_0}$ thỏa $-Q_0/2<P_0\le Q_0/2$, và đặt $X$ sao cho $x=Q_0X-P_0y$. Khi đó $P_0^2\equiv D\pmod{Q_0}$, $\dfrac{X}{y}$ là một phân số liên tục của $\omega=\dfrac{P_0+\sqrt{D}}{Q_0}$, ký hiệu $\dfrac{A_{k-1}}{B_{k-1}}$, và $Q_k=(-1)^k\dfrac{N}{|N|}$.

??? note "Chứng minh"
    Từ $x\equiv -P_0y\pmod{Q_0}$ và $x^2-Dy^2=N\equiv 0\pmod{Q_0}$, suy ra $P_0^2\equiv D\pmod{Q_0}$. Do đó
    
    $$
    P_0x+Dy\equiv -P_0^2y+Dy = (D-P_0^2)y\equiv 0\pmod{Q_0}.
    $$
    
    Xét ma trận hệ số nguyên
    
    $$
    \begin{pmatrix}P & R \\ Q & S\end{pmatrix}
    =
    \begin{pmatrix}X & \dfrac{P_0x+Dy}{Q_0} \\ y & x\end{pmatrix}.
    $$
    
    Định thức
    
    $$
    PS-QR = \dfrac{x(x+P_0y)-y(P_0x+Dy)}{Q_0} = \dfrac{x^2-Dy^2}{Q_0} = \pm 1.
    $$
    
    Hơn nữa, đặt $\zeta =\sqrt{D} > 1$, ta có
    
    $$
    \dfrac{P\zeta+R}{Q\zeta+S} = \dfrac{(x+P_0y)\sqrt{D}+(P_0x+Dy)}{(x+y\sqrt{D})Q_0} = \dfrac{P_0+\sqrt{D}}{Q_0} = \omega.
    $$
    
    Tiếp theo chứng minh $\dfrac{P}{Q}$ là phân số liên tục của $\omega$. Giả sử
    
    $$
    \dfrac{P}{Q} = [a_0,a_1,\cdots,a_k]
    $$
    
    và $PS-QR = (-1)^{k-1}$. Nếu $\dfrac{p_k}{q_k}$ là phân số liên tục thứ $k$, thì $(p_k,q_k)=(P,Q)$ và theo [công thức sai phân](./continued-fraction.md#误差估计), $p_kq_{k-1}-q_kp_{k-1}=(-1)^{k-1}$. Suy ra
    
    $$
    p_k(S-q_{k-1}) = q_k(R-p_{k-1}).
    $$
    
    Xét các trường hợp:
    
    -   Nếu $S=0$, dễ thấy $Q=R=1$, do đó $\omega=P+\zeta^{-1}=[P,\zeta]$, nên $\dfrac{P}{Q}=P$ là phân số liên tục thứ $0$ của $\omega$;
    -   Nếu $Q=S>0$, thì $Q=S=1$ và $P-R=\pm 1$. Khi đó:
        -   Nếu $P=R+1$, thì $\omega=R+\dfrac{1}{1+\zeta^{-1}}=[R,1,\zeta]$, nên $\dfrac{P}{Q}=\dfrac{R+1}{1}=[R,1]$ là phân số liên tục thứ $1$;
        -   Nếu $P=R-1$, thì $\omega=R-1+\dfrac{1}{1+\zeta}=[R-1,\zeta-1]$, nên $\dfrac{P}{Q}=R-1$ là phân số liên tục thứ $0$;
    -   Nếu $Q\neq S>0$, vì $Q=q_k\mid(S-q_{k-1})$ nên tồn tại $\kappa$ sao cho $S=\kappa q_k+q_{k-1}$ và $R=\kappa p_k+p_{k-1}$. Do $q_k\ge q_{k-1}$ và $S>0$, suy ra $\kappa\ge 0$. Khi đó $\omega=\dfrac{(\kappa+\zeta)p_k+p_{k-1}}{(\kappa+\zeta)q_k+q_{k-1}}=[a_0,a_1,\cdots,a_k,\kappa+\zeta]$, nên $\dfrac{P}{Q}$ là phân số liên tục thứ $k$.
    
    Tóm lại, luôn có $\dfrac{X}{y}$ là phân số liên tục của $\omega=\dfrac{P_0+\sqrt{D}}{Q_0}$, ký hiệu $\dfrac{A_{k-1}}{B_{k-1}}$. Vì $A_{k-1}^2-DB_{k-1}^2=(-1)^kQ_0Q_k$, suy ra $Q_k=(-1)^k\dfrac{N}{|N|}$.

Định lý này đảm bảo mọi nghiệm dương của phương trình đều xuất hiện trong các phân số liên tục của các số vô tỉ bậc hai tương ứng. Khi dùng PQa để tính phân số liên tục, chỉ cần vào chu kỳ thì các phân số liên tục luôn dương. Vì vậy, chỉ cần liệt kê mọi số vô tỉ bậc hai thỏa điều kiện của định lý và tính phân số liên tục trong một chu kỳ, ta có thể tìm một nghiệm. Vì hai nghiệm xuất hiện trong cùng một phân số liên tục là tương đương, nên chỉ cần lấy nghiệm đầu tiên thỏa $(-1)^kQ_k=N/|N|$ rồi dừng. Khác với các thuật toán trước, ở đây $k$ thỏa điều kiện có thể xuất hiện trước khi vào chu kỳ.

??? example "Ví dụ"
    1.  Giải phương trình $x^2-157y^2=12$.
    
        Vì $12^2<157$, nên chạy PQa với $(P_0,Q_0,D)=(0,1,157)$ được: (phần tô đỏ là chu kỳ đầu)
    
        |  $k$ |  $P$ |  $Q$ |        $a$        |         $A$        |        $B$       |         $G$        | $G^2-DB^2$ |
        | :--: | :--: | :--: | :---------------: | :----------------: | :--------------: | :----------------: | :--------: |
        |  $0$ |  $0$ |  $1$ |        $12$       |        $12$        |        $1$       |        $12$        |    $-13$   |
        |  $1$ | $12$ | $13$ |   $\color{red}1$  |        $13$        |        $1$       |        $13$        |    $12$    |
        |  $2$ |  $1$ | $12$ |   $\color{red}1$  |        $25$        |        $2$       |        $25$        |    $-3$    |
        |  $3$ | $11$ |  $3$ |   $\color{red}7$  |        $188$       |       $15$       |        $188$       |    $19$    |
        |  $4$ | $10$ | $19$ |   $\color{red}1$  |        $213$       |       $17$       |        $213$       |    $-4$    |
        |  $5$ |  $9$ |  $4$ |   $\color{red}5$  |       $1253$       |       $100$      |       $1253$       |     $9$    |
        |  $6$ | $11$ |  $9$ |   $\color{red}2$  |       $2719$       |       $217$      |       $2719$       |    $-12$   |
        |  $7$ |  $7$ | $12$ |   $\color{red}1$  |       $3972$       |       $317$      |       $3972$       |    $11$    |
        |  $8$ |  $5$ | $11$ |   $\color{red}1$  |       $6691$       |       $534$      |       $6691$       |    $-11$   |
        |  $9$ |  $6$ | $11$ |   $\color{red}1$  |       $10663$      |       $851$      |       $10663$      |    $12$    |
        | $10$ |  $5$ | $12$ |   $\color{red}1$  |       $17354$      |      $1385$      |       $17354$      |    $-9$    |
        | $11$ |  $7$ |  $9$ |   $\color{red}2$  |       $45371$      |      $3621$      |       $45371$      |     $4$    |
        | $12$ | $11$ |  $4$ |   $\color{red}5$  |      $244209$      |      $19490$     |      $244209$      |    $-19$   |
        | $13$ |  $9$ | $19$ |   $\color{red}1$  |      $289580$      |      $23111$     |      $289580$      |     $3$    |
        | $14$ | $10$ |  $3$ |   $\color{red}7$  |      $2271269$     |     $181267$     |      $2271269$     |    $-12$   |
        | $15$ | $11$ | $12$ |   $\color{red}1$  |      $2560849$     |     $204378$     |      $2560849$     |    $13$    |
        | $16$ |  $1$ | $13$ |   $\color{red}1$  |      $4832118$     |     $385645$     |      $4832118$     |    $-1$    |
        | $17$ | $12$ |  $1$ | $\color{red}{24}$ |     $118531681$    |     $9459858$    |     $118531681$    |    $13$    |
        | $18$ | $12$ | $13$ |        $1$        |     $123363799$    |     $9845503$    |     $123363799$    |    $-12$   |
        | $19$ |  $1$ | $12$ |        $1$        |     $241895480$    |    $19305361$    |     $241895480$    |     $3$    |
        | $20$ | $11$ |  $3$ |        $7$        |    $1816632159$    |    $144983030$   |    $1816632159$    |    $-19$   |
        | $21$ | $10$ | $19$ |        $1$        |    $2058527639$    |    $164288391$   |    $2058527639$    |     $4$    |
        | $22$ |  $9$ |  $4$ |        $5$        |    $12109270354$   |    $966424985$   |    $12109270354$   |    $-9$    |
        | $23$ | $11$ |  $9$ |        $2$        |    $26277068347$   |   $2097138361$   |    $26277068347$   |    $12$    |
        | $24$ |  $7$ | $12$ |        $1$        |    $38386338701$   |   $3063563346$   |    $38386338701$   |    $-11$   |
        | $25$ |  $5$ | $11$ |        $1$        |    $64663407048$   |   $5160701707$   |    $64663407048$   |    $11$    |
        | $26$ |  $6$ | $11$ |        $1$        |   $103049745749$   |   $8224265053$   |   $103049745749$   |    $-12$   |
        | $27$ |  $5$ | $12$ |        $1$        |   $167713152797$   |   $13384966760$  |   $167713152797$   |     $9$    |
        | $28$ |  $7$ |  $9$ |        $2$        |   $438476051343$   |   $34994198573$  |   $438476051343$   |    $-4$    |
        | $29$ | $11$ |  $4$ |        $5$        |   $2360093409512$  |  $188355959625$  |   $2360093409512$  |    $19$    |
        | $30$ |  $9$ | $19$ |        $1$        |   $2798569460855$  |  $223350158198$  |   $2798569460855$  |    $-3$    |
        | $31$ | $10$ |  $3$ |        $7$        |  $21950079635497$  |  $1751807067011$ |  $21950079635497$  |    $12$    |
        | $32$ | $11$ | $12$ |        $1$        |  $24748649096352$  |  $1975157225209$ |  $24748649096352$  |    $-13$   |
        | $33$ |  $1$ | $13$ |        $1$        |  $46698728731849$  |  $3726964292220$ |  $46698728731849$  |     $1$    |
        | $34$ | $12$ |  $1$ |        $24$       | $1145518138660728$ | $91422300238489$ | $1145518138660728$ |    $-13$   |
        | $35$ | $12$ | $13$ |        $1$        | $1192216867392577$ | $95149264530709$ | $1192216867392577$ |    $12$    |
    
        Chu kỳ $\ell=17$ lẻ, nên cần xét hai chu kỳ đối với các vị trí $k=1,9,13,19,23,31$ nơi $G_{k-1}^2-157B_{k-1}^2$ khác $12$ bởi một thừa số bình phương. Các nghiệm tương ứng là $(fG,fB)$ trong bảng sau:
    
        |  $k$ | $f$ |    $fG_{k-1}$    |    $fB_{k-1}$   |    $x$    |   $y$   |
        | :--: | :-: | :--------------: | :-------------: | :-------: | :-----: |
        |  $1$ | $1$ |       $13$       |       $1$       |    $13$   |   $1$   |
        |  $9$ | $1$ |      $10663$     |      $851$      |  $10663$  |  $851$  |
        | $13$ | $2$ |     $579160$     |     $46222$     |  $579160$ | $46222$ |
        | $19$ | $2$ |    $483790960$   |    $38610722$   | $-579160$ | $46222$ |
        | $23$ | $1$ |   $26277068347$  |   $2097138361$  |  $-10663$ |  $851$  |
        | $31$ | $1$ | $21950079635497$ | $1751807067011$ |   $-13$   |   $1$   |
    
        Tập các $(fG,fB)$ là các nghiệm dương nhỏ nhất trong mỗi lớp tương đương của $x^2-157y^2=12$. Để sinh toàn bộ nghiệm, dùng nghiệm cơ bản của Pell là $(46698728731849,3726964292220)$. Chẳng hạn, có thể chuyển chúng thành nghiệm cơ bản của từng lớp; các nghiệm đó cũng liệt kê trong bảng.
    2.  Giải phương trình $x^2-157y^2=12$.
    
        Lần này dùng thuật toán Lagrange–Matthews–Mollin. Trước hết liệt kê các ước bình phương của $N=12$:
    
        -   $f^2=1^2$ thì $m=12$, đồng dư $P^2\equiv 157\pmod{12}$ có nghiệm $z=\pm 1,\pm 5$;
        -   $f^2=2^2$ thì $m=3$, đồng dư $P^2\equiv 157\pmod{3}$ có nghiệm $z=\pm 1$.
    
        Với mọi cặp $(f,z)$, chạy PQa với $(P_0,Q_0,D)=(z,|m|,D)$ và tìm vị trí đầu tiên có $(-1)^kQ_k=1$, khi đó $(fG_{k-1},fB_{k-1})$ là một nghiệm. Kết quả:
    
        | $f$ |  $z$ |  $m$ |  $k$ |    $fG_{k-1}$    |    $fB_{k-1}$   |
        | :-: | :--: | :--: | :--: | :--------------: | :-------------: |
        | $1$ |  $1$ | $12$ | $32$ | $21950079635497$ | $1751807067011$ |
        | $1$ | $-1$ | $12$ |  $2$ |       $13$       |       $1$       |
        | $1$ |  $5$ | $12$ | $24$ |   $26277068347$  |   $2097138361$  |
        | $1$ | $-5$ | $12$ | $10$ |      $10663$     |      $851$      |
        | $2$ |  $1$ |  $3$ | $20$ |    $483790960$   |    $38610722$   |
        | $2$ | $-1$ |  $3$ | $14$ |     $579160$     |     $46222$     |
    
        Đây chính là các nghiệm dương nhỏ nhất của mỗi lớp, có thể dùng nghiệm cơ bản Pell để chuyển sang nghiệm cơ bản.
    3.  Giải phương trình $x^2-79y^2=\pm 101$.
    
        Vẫn dùng thuật toán Lagrange–Matthews–Mollin. Vì $N=101$ là số nguyên tố, nên $f=1$. Khi đó $m=101$, đồng dư $P^2\equiv 79\pmod{101}$ có nghiệm $P=\pm 33$.
    
        Chạy PQa với $(P_0,Q_0,D)=(33,101,79)$ được: (phần tô đỏ là chu kỳ đầu)
    
        | $k$ |  $P$  |  $Q$  |       $a$      |  $A$  |   $B$  |   $G$   | $G^2-DB^2$ |
        | :-: | :---: | :---: | :------------: | :---: | :----: | :-----: | :--------: |
        | $0$ |  $33$ | $101$ |       $0$      |  $0$  |   $1$  |  $-33$  |   $1010$   |
        | $1$ | $-33$ | $-10$ |       $2$      |  $1$  |   $2$  |   $35$  |    $909$   |
        | $2$ |  $13$ |  $9$  |       $2$      |  $2$  |   $5$  |   $37$  |   $-606$   |
        | $3$ |  $5$  |  $6$  | $\color{red}2$ |  $5$  |  $12$  |  $109$  |    $505$   |
        | $4$ |  $7$  |  $5$  | $\color{red}3$ |  $17$ |  $41$  |  $364$  |   $-303$   |
        | $5$ |  $8$  |  $3$  | $\color{red}5$ |  $90$ |  $217$ |  $1929$ |   $1010$   |
        | $6$ |  $7$  |  $10$ | $\color{red}1$ | $107$ |  $258$ |  $2293$ |   $-707$   |
        | $7$ |  $3$  |  $7$  | $\color{red}1$ | $197$ |  $475$ |  $4222$ |    $909$   |
        | $8$ |  $4$  |  $9$  | $\color{red}1$ | $304$ |  $733$ |  $6515$ |   $-606$   |
        | $9$ |  $5$  |  $6$  |       $2$      | $805$ | $1941$ | $17252$ |    $505$   |
    
        Chu kỳ $\ell=6$ chẵn. Kết thúc một chu kỳ vẫn không có $Q_k=\pm 1$, nên trường hợp này vô nghiệm. Tương tự với $(P_0,Q_0,D)=(-33,101,79)$ cũng không có nghiệm. Vì vậy phương trình vô nghiệm.

## Bài tập

-   [LOJ 6687.「Project Euler 66」解方程](https://loj.ac/p/6687)
-   [SPOJ EQU2 - Yet Another Equation](https://www.spoj.com/problems/EQU2/)
-   [SPOJ PELL2 - Pell (Mid pelling)](https://www.spoj.com/problems/PELL2/)
-   [UVa 12909. Numeric Center](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=862&page=show_problem&problem=4774)
-   [UVa 10241. Semi-triangular and also Square](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=14&page=show_problem&problem=1182)

## Tài liệu tham khảo và chú thích

-   [Pell's equation - Wikipedia](https://en.wikipedia.org/wiki/Pell%27s_equation)
-   [John P. Robertson - Solving the generalized Pell equation $x^2-Dy^2=N$](https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=5ac34a344ee346855184ff949eeaed18685b155c)
-   [Keith Matthews - The Diophantine Equation $x^2-Dy^2=N$,$D>0$](http://www.numbertheory.org/PDFS/patz5.pdf)
-   [Existence of Solution to Pell’s Equation - Suryateja Gavva's Blog](https://surya-teja.com/2011/01/11/existence-of-solution-to-pells-equation/)
-   [Calculating the simple continued fraction of a quadratic irrational - Number Theory Web](http://www.numbertheory.org/php/surd.html)（thuật toán PQa）
-   [Solving the diophantine equation x2–Dy2 = N, D > 0 and not a perfect square, N ≠ 0 - Number Theory Web](http://www.numbertheory.org/php/patz.html)（thuật toán Lagrange–Matthews–Mollin）

[^not-square]: Khi $D$ là số chính phương, phân tích nhân tử cho thấy $(x+y\sqrt{D})(x-y\sqrt{D})=N$, do đó mọi nghiệm có thể tìm được bằng cách duyệt các ước của $N$. Đặc biệt, khi $N=1$ thì chỉ có nghiệm $(\pm 1,0)$; khi $N=-1$ và $D\neq 1$ thì không có nghiệm.

[^neg-pell]: Một số tài liệu tiếng Trung cũng gọi đây là phương trình Pell loại hai.

[^half-int]: Tức các số hữu tỉ dạng $n+\dfrac12$ với $n\in\mathbf Z$.

[^fundamental-solution]: Lưu ý, định nghĩa nghiệm cơ bản của phương trình Pell không trùng với định nghĩa đơn vị cơ bản trong vành số nguyên bậc hai thực. Thứ nhất, trong một số vành số nguyên bậc hai thực, đơn vị cơ bản $x+y\sqrt{D}$ có $x,y$ là bán nguyên, nên không phải nghiệm của Pell. Thứ hai, cùng một vành số nguyên bậc hai thực có bốn đơn vị cơ bản, nhưng nghiệm cơ bản chỉ có một, vì yêu cầu $x,y$ đều dương.

[^solubility-neg-pell]: Một phương pháp và công cụ thực dụng được nêu [tại đây](http://www.numbertheory.org/php/hardy_williams.html) cùng các tài liệu tham khảo. Danh sách các $D$ dương sao cho $x^2-Dy^2=-1$ có nghiệm là [OEIS A031396](https://oeis.org/A031396).
