Bài viết giới thiệu ngắn gọn các khái niệm về trường bậc hai. Hai ví dụ quan trọng là số nguyên Gauss và số nguyên Eisenstein, dùng để giải một số bài toán số học.

## Khái niệm cơ bản

Mục này giới thiệu các khái niệm cơ bản. Trường bậc hai và vành số nguyên bậc hai là các trường hợp đặc biệt của mở rộng đại số và vành số nguyên đại số, nên gần như mọi định nghĩa/kết luận ở đây có thể tổng quát hóa. Bài viết chỉ xét trường bậc hai.

### Trường bậc hai

Phần tử của trường bậc hai là các số đại số bậc hai.

**Số đại số bậc hai** (quadratic algebraic number) là nghiệm của phương trình bậc hai hệ số nguyên. Theo công thức nghiệm, mọi số đại số bậc hai có thể viết

$$
a+b\sqrt{d}
$$

với $a,b$ hữu tỉ, $d$ nguyên không có ước bình phương. Mọi số dạng này đều là số đại số bậc hai. Chúng gồm số hữu tỉ và **vô tỉ bậc hai** (quadratic irrational number). Cách viết là duy nhất.

Với $d\neq 0,1$ không có ước bình phương, tập $Q(\sqrt{d})=\{a+b\sqrt{d}:a,b\in\mathbf Q\}$ đóng với cộng, trừ, nhân, chia. Một tập đóng với bốn phép gọi là [trường](../algebra/basic.md#域), nên $Q(\sqrt{d})$ gọi là **trường bậc hai**. Mọi trường bậc hai chứa toàn bộ hữu tỉ, nên là [mở rộng bậc hai](../algebra/field-theory.md#域的扩张) của $\mathbf Q$. Khi $d>0$, mọi số trong $\mathbf Q(\sqrt{d})$ là thực, gọi là trường bậc hai thực; khi $d<0$, ngoài hữu tỉ thì là số phức, gọi là trường bậc hai ảo.

### Liên hợp và chuẩn

Liên hợp của $a+b\sqrt{d}$ là $a-b\sqrt{d}$. Hai số liên hợp là hai nghiệm khác nhau của cùng một phương trình bậc hai nguyên. Trong trường bậc hai thực, liên hợp khác với liên hợp phức; trong trường bậc hai ảo, chúng trùng nhau. Số hữu tỉ liên hợp với chính nó. Như vậy khái niệm liên hợp xác định cho mọi số đại số bậc hai.

Mọi đẳng thức tạo từ cộng trừ nhân chia trong trường bậc hai đều bất biến khi thay mỗi số bằng liên hợp của nó.

Dùng liên hợp có thể xây dựng các ánh xạ từ số đại số bậc hai về hữu tỉ. Đơn giản là **vết** (trace) $\operatorname{tr}(\alpha)$, là tổng một số với liên hợp của nó. Đây chỉ là $2$ lần phần hữu tỉ nên ít thông tin.

Hữu ích hơn là **chuẩn** (norm): tích của số với liên hợp:

$$
N(a+b\sqrt{d})=a^2-db^2
$$

Trong trường bậc hai ảo, chuẩn trùng với bình phương môđun phức; trong trường bậc hai thực thì không.

Chuẩn có tính chất tốt: vì $d$ không là bình phương nên chỉ $0$ có chuẩn $0$. Chuẩn bảo toàn nhân và chia:

$$
N(a_1+b_1\sqrt{d})N(a_2+b_2\sqrt{d})=N((a_1+b_1\sqrt{d})(a_2+b_2\sqrt{d})),
$$

$$
\frac{N(a_1+b_1\sqrt{d})}{N(a_2+b_2\sqrt{d})}=N\left(\frac{a_1+b_1\sqrt{d}}{a_2+b_2\sqrt{d}}\right).
$$

Ngoài ra, nghịch đảo của số là liên hợp chia cho chuẩn:

$$
\dfrac{1}{a+b\sqrt{d}}=\frac{a-b\sqrt{d}}{N(a+b\sqrt{d})}.
$$

Theo định lý Viète, $\alpha$ là nghiệm của

$$
x^2-\operatorname{tr}(\alpha)x+N(\alpha)=0
$$

Định thức biệt (discriminant) của phương trình gọi là **định thức biệt** của $\alpha$, ký hiệu $\operatorname{disc}(\alpha)$, bằng $4db^2$.

???+ note "Biểu diễn ma trận"
    Tương tự số phức, số đại số bậc hai có thể biểu diễn bằng ma trận. Với $d\neq 0,1$ không có ước bình phương và $a,b$ hữu tỉ, số $a+b\sqrt{d}$ biểu diễn bởi
    
    $$
    \begin{pmatrix}a & b \\ db & a\end{pmatrix}.
    $$
    
    Có thể kiểm tra cộng, trừ, nhân, chia của ma trận tương ứng với phép toán của số. Vết và định thức của ma trận tương ứng với vết và chuẩn. Định thức của đa thức đặc trưng là định thức biệt. Ma trận phụ hợp tương ứng với liên hợp.

### Vành số nguyên bậc hai

Trong các số đại số bậc hai, đặc biệt là **số nguyên bậc hai**: nghiệm của phương trình bậc hai hệ số nguyên với hệ số bậc hai bằng $1$. So với số đại số bậc hai, khác ở điều kiện hệ số bậc hai.

Với $x^2+px+q=0$, nghiệm là

$$
\frac{-p\pm\sqrt{p^2-4q}}{2}.
$$

Nếu $p$ chẵn ($p=2k$), nghiệm là $-k\pm\sqrt{k^2-q}$; nếu $p$ lẻ ($p=2k+1$), nghiệm là $-k-\dfrac{1\pm\sqrt{4(k^2+k-q)+1}}{2}$. Từ đó suy ra số nguyên bậc hai trong $\mathbf Q(\sqrt{d})$ đều viết được

$$
a+b\omega
$$

với $a,b$ nguyên và

$$
\omega=\begin{cases}
\dfrac{1+\sqrt{d}}{2}, & d\equiv 1\pmod 4,\\
\sqrt{d}, & d\equiv 2,3\pmod 4.
\end{cases}
$$

Ngược lại, mọi số dạng đó là số nguyên bậc hai. Với số nguyên bậc hai không hữu tỉ, biểu diễn là duy nhất.

Tập các số nguyên bậc hai trong $\mathbf Q(\sqrt{d})$ ký hiệu $\mathbf Z[\omega]$. Tập này đóng với cộng, trừ, nhân, nên gọi là **vành số nguyên bậc hai** (quadratic integer ring). Các số hữu tỉ trong $\mathbf Z[\omega]$ chính là các số nguyên. Tập tỉ số của các số nguyên bậc hai tạo lại trường $\mathbf Q(\sqrt{d})$.

Vết, chuẩn, định thức biệt của số nguyên bậc hai đều là số nguyên. Định thức biệt nhỏ nhất của số vô tỉ bậc hai trong $\mathbf Z[\omega]$ cũng gọi là định thức biệt của trường $\mathbf Q(\sqrt{d})$. Khi $d\equiv 1\pmod 4$, định thức biệt là $d$; khi $d\equiv 2,3\pmod 4$ là $4d$.

### Chia hết, liên hợp và đơn vị

Tương tự số nguyên, ta có lý thuyết chia hết cho số nguyên bậc hai (trong cùng vành).

Với $\alpha,\beta\in\mathbf Z[\omega]$, nếu tồn tại $\gamma$ trong vành sao cho $\beta=\alpha\gamma$, thì $\alpha$ chia $\beta$, ký hiệu $\alpha\mid\beta$. Quan hệ này là [thứ tự bán phần](../order-theory.md#二元关系). Nếu $\alpha\mid\beta$ và $\beta\mid\alpha$ thì $\alpha,\beta$ xem như cùng một số trong chia hết, gọi là **liên hợp** (associate). Đây là quan hệ tương đương.

Với số nguyên, liên hợp là đối dấu. Với số nguyên bậc hai, phức tạp hơn. Nếu $\alpha,\beta$ liên hợp, tồn tại $\gamma,\delta$ sao cho $\beta=\alpha\gamma$, $\alpha=\beta\delta$. Khi đó $\gamma$ là số đặc biệt có nghịch đảo trong vành (tức $\gamma\delta=1$), gọi là **đơn vị** (unit). Hai số liên hợp khi và chỉ khi tỉ số của chúng là đơn vị. Do đó cần hiểu cấu trúc đơn vị.

Vì chuẩn bảo toàn nhân và số nguyên bậc hai có chuẩn nguyên, nên nếu $\alpha\mid\beta$ thì $N(\alpha)\mid N(\beta)$. Một số $\alpha$ là đơn vị khi và chỉ khi $N(\alpha)=\pm 1$. Do đó tìm đơn vị trong $\mathbf Z[\omega]$ tương đương giải phương trình:

$$
N(a+b\sqrt{d})=1,
$$

với

$$
N(a+b\sqrt{d})=\begin{cases}
a^2+ab+\dfrac{1-d}{4}b^2, & d\equiv 1\pmod 4,\\
a^2-db^2, & d\equiv 2,3\pmod 4.
\end{cases}
$$

Với vành bậc hai ảo ($d<0$), chuẩn không âm, nên với mọi $d<0$ không có ước bình phương và $d\neq -1,-3$, nghiệm chỉ là $(a,b)=(\pm 1,0)$, tức đơn vị chỉ là $\pm 1$. Đặt $\mathrm{i}=\sqrt{-1}$, vành $\mathbf Z[\mathrm{i}]$ gọi là vành số nguyên Gauss, đơn vị là $\{\pm 1,\pm\mathrm{i}\}$. Đặt $\omega=\frac{1+\sqrt{-3}}{2}$, vành $\mathbf Z[\omega]$ gọi là vành số nguyên Eisenstein, đơn vị là $\{\pm 1,\pm\omega,\pm\omega^2\}$.

Với vành bậc hai thực ($d>0$) phức tạp hơn, quy về [phương trình Pell](./pell-equation.md). Theo đó, tập đơn vị có dạng $\{\pm u^k:k\in\mathbf Z\}$, trong đó $u$ là **đơn vị cơ bản**. Đơn vị cơ bản không duy nhất: nếu $u$ là cơ bản thì $\bar u$, $-u$, $-\bar u$ cũng là cơ bản.

Cấu trúc đơn vị có thể tổng quát hóa cho [vành số nguyên đại số](../algebra/field-theory.md#代数扩张). [Định lý đơn vị Dirichlet](https://en.wikipedia.org/wiki/Dirichlet%27s_unit_theorem) cho biết tập đơn vị là [nhóm Abel hữu hạn sinh](../algebra/group-theory.md#有限生成-abel-群), đồng thời cho biết hạng.

Lý thuyết ước chung, chia có dư, Bezout, phân tích duy nhất… có thể mở rộng một phần hoặc toàn bộ lên vành số nguyên bậc hai. Việc có thể mở rộng hay không phản ánh mức độ “giống” với $\mathbf Z$. Không phải mọi vành đều có phân tích duy nhất; trong các vành có phân tích duy nhất, chỉ một phần có chia có dư. Xem [vành số nguyên bậc hai](../algebra/ring-theory.md#例子二次整数环) hoặc tài liệu khác.

### Phân tích duy nhất

Nếu phân tích duy nhất của $\mathbf Z$ được mở rộng, thì trong $\mathbf Z[\omega]$ mọi số phân tích thành tích các phần tử bất khả quy, và duy nhất đến thứ tự và liên hợp. [Phần tử bất khả quy](../algebra/ring-theory.md#整除关系) là phần tử không thể phân tích thành tích của hai phần tử không là đơn vị, giống “nguyên tố” trong $\mathbf Z$. Như đã nói, không phải mọi $\mathbf Z[\omega]$ đều có phân tích duy nhất.

Ví dụ trong $\mathbf Z[\sqrt{-5}]$ có $9=3\times 3=(2+\sqrt{-5})\times(2-\sqrt{-5})$, nhưng $3$ và $2\pm\sqrt{-5}$ đều bất khả quy, nên phân tích không duy nhất. Để chứng minh chúng bất khả quy, xem chuẩn: đều có chuẩn $9$, nếu phân tích được thì chuẩn các thừa số phải là $3$, nhưng $\mathbf Z[\sqrt{-5}]$ không có số chuẩn $3$.

Lý do chính của việc không có phân tích duy nhất là phân tích bằng phần tử là chưa đủ. Ví dụ phân tích $abcd$ nhưng chỉ có thể chọn $\{ab,cd,ac,bd\}$ thì không thể duy nhất; phải xét $\{a,b,c,d\}$. Trong vành số nguyên bậc hai, cấu trúc tinh hơn là [ideal](../algebra/ring-theory.md#理想). Ánh xạ phần tử sang ideal chính của nó nhúng lớp liên hợp vào tập ideal, nên phân tích phần tử là trường hợp đặc biệt của phân tích ideal. Thật vậy, mọi ideal trong vành số nguyên bậc hai phân tích duy nhất thành tích các ideal nguyên tố. Điều này cho thấy các vành này là [Dedekind domain](https://en.wikipedia.org/wiki/Dedekind_domain), và mọi vành số nguyên đại số cũng vậy.

Nếu một vành số nguyên bậc hai có phân tích duy nhất, thì ideal nguyên tố tương ứng một-một với phần tử bất khả quy (theo liên hợp). Khi đó phân tích ideal thành ideal nguyên tố tương đương phân tích phần tử thành bất khả quy; và bất khả quy cũng gọi là [phần tử nguyên tố](../algebra/ring-theory.md#整除关系). Các phần sau dùng ideal nguyên tố; nếu chưa quen, có thể thay bằng “phần tử nguyên tố” trong trường hợp phân tích duy nhất.

Để hiểu phân tích duy nhất trong $\mathbf Z[\omega]$, cần biết các ideal nguyên tố. Mọi ideal nguyên tố chia ideal chính của chuẩn. Phân tích chuẩn trong $\mathbf Z$, theo phân tích duy nhất, ideal nguyên tố phải chia một trong các ideal nguyên tố ấy. Vì vậy, ideal nguyên tố trong $\mathbf Z[\omega]$ là kết quả phân tích tiếp của các nguyên tố trong $\mathbf Z$. Liệt kê chúng tương đương với việc phân tích $(p)$ trong $\mathbf Z[\omega]$ cho mỗi nguyên tố $p$. Vì chuẩn của $(p)$ là $p^2$, nên các ideal nguyên tố trong phân tích có chuẩn là ước của $p^2$, tức chỉ có $p$ hoặc $p^2$. Do đó có ba khả năng:

1.  $p$ **trơ** (inert): $(p)$ vẫn là ideal nguyên tố;
2.  $p$ **tách** (split): $(p)$ phân tích thành tích hai ideal nguyên tố liên hợp;
3.  $p$ **phân nhánh** (ramify): $(p)$ là bình phương của một ideal nguyên tố.

Có thể chứng minh: để xác định $p$ thuộc trường hợp nào, chỉ cần tính [ký hiệu Kronecker](https://en.wikipedia.org/wiki/Kronecker_symbol) $\left(\dfrac{D}{p}\right)$ với $D$ là định thức biệt của trường. Ba trường hợp tương ứng $\{-1,+1,0\}$. Với $p$ lẻ, Kronecker là [ký hiệu Legendre](./quad-residue.md#legendre-符号); $-1$ tương ứng $D$ là [bất thặng dư bậc hai](./quad-residue.md), $+1$ là thặng dư, $0$ là $p\mid D$. Với $p=2$, ba trường hợp lần lượt là $D\equiv \pm 3\pmod 8$, $D\equiv \pm 1\pmod 8$, và $2\mid D$.

## Số nguyên Gauss

Mục này đặt $\mathrm{i}=\sqrt{-1}$. Trường $\mathbf Q(\mathrm{i})$ còn gọi là trường Gauss, cũng là [trường phân chia bậc 4](../algebra/field-theory.md#分圆域). Vành $\mathbf Z[\mathrm{i}]$ gọi là vành số nguyên Gauss, phần tử gọi là **số nguyên Gauss** (Gaussian integer). Đơn vị là $\pm 1,\pm\mathrm{i}$, nên mỗi số Gauss khác $0$ có 4 liên hợp (kể cả chính nó). Trên mặt phẳng phức, số Gauss là các điểm lưới; chuẩn $N(a+b\mathrm{i})=a^2+b^2$ là bình phương khoảng cách tới gốc.

![](./images/gaussian-integer.svg)

Trong số Gauss có phép chia có dư: với $a,b\neq 0$, tồn tại $q,r$ sao cho $a=bq+r$ và $N(r)<N(b)$. Để tính, tính $\dfrac{a}{b}$ trong $\mathbf Q(\mathrm{i})$, chọn điểm nguyên gần nhất làm $q$, rồi $r=a-bq$; khi đó $N(r)\le\dfrac{1}{2}N(b)$. Nhờ đó có thể mở rộng Euclid, Bezout, và phân tích duy nhất cho số Gauss.

### Nguyên tố Gauss

Dùng kết luận trên để tìm phần tử nguyên tố (Gauss primes). Vì định thức biệt của $\mathbf Z[\mathrm{i}]$ là $-4$, ký hiệu Kronecker

$$
\left(\dfrac{-4}{n}\right) = \begin{cases}
+1,& n\equiv 1\pmod 4,\\
-1,& n\equiv 3\pmod 4,\\
0,& 2\mid n.
\end{cases}
$$

Vậy nguyên tố Gauss gồm:

1.  Nguyên tố $4k+3$ trong $\mathbf Z$;
2.  Hai thừa số Gauss liên hợp của nguyên tố $4k+1$;
3.  Thừa số $1+\mathrm{i}$ của $2$, liên hợp của nó là liên hợp và liên hợp nhau.

Ví dụ, trong $\mathbf Z[\mathrm{i}]$, $60=2^2\times 3\times 5=-(1+\mathrm{i})^4\times 3\times(2+\mathrm{i})\times(2-\mathrm{i})$.

Bàn về việc $p$ phân tích trong $\mathbf Z[\mathrm{i}]$ tương đương việc $p$ viết được dưới dạng $a^2+b^2$. Do đó, suy ra: nguyên tố $p$ biểu diễn được dưới dạng tổng hai bình phương khi và chỉ khi $p=2$ hoặc $p\equiv 1\pmod 4$ (định lý Fermat về tổng hai bình phương).

### Điểm nguyên trên đường tròn

Trên mặt phẳng phức, số Gauss là các điểm nguyên. Chuẩn là bình phương khoảng cách tới gốc. Do đó, các số Gauss cùng chuẩn tương ứng điểm nguyên trên đường tròn tâm gốc. Tức số điểm nguyên trên $x^2+y^2=n$ bằng số số Gauss có chuẩn $n$.

Giải $N(x+y\mathrm{i})=n$ có thể xét phân tích của $x+y\mathrm{i}$ trong số Gauss, khi đó tích chuẩn của các thừa số bằng $n$. Chỉ cần phân tích $n$ trong $\mathbf Z$ rồi suy ra các thừa số Gauss có thể.

Giả sử

$$
n=2^kp_1^{r_1}\cdots p_\ell^{r_\ell}q_1^{s_1}\cdots q_{m}^{s_m},
$$

trong đó $p_i$ là nguyên tố $4k+1$, $q_i$ là $4k+3$.

Phương trình có nghiệm khi và chỉ khi mọi $s_i$ chẵn; vì các $q_i$ là nguyên tố Gauss, chuẩn của chúng là bình phương của chính nó nên phải xuất hiện theo cặp.

Nếu có nghiệm, thì nghiệm có dạng

$$
u(1+\mathrm{i})^k(a_1+b_1\mathrm{i})^{r_{1}^+}(a_1-b_1\mathrm{i})^{r_{1}^-}\cdots(a_\ell+ b_\ell\mathrm{i})^{r_\ell^+}(a_\ell-b_\ell\mathrm{i})^{r_\ell^-}q_1^{s_1/2}\cdots q_{m}^{s_m/2},
$$

trong đó $u$ là đơn vị, $a_j\pm b_j\mathrm{i}$ là cặp thừa số của $p_j$, và $r_j^++r_j^-=r_j$. Số nghiệm là

$$
4(1+r_1)\cdots(1+r_\ell).
$$

Gọi $f(n)$ là số nghiệm nguyên của $x^2+y^2=n$. Nếu có nghiệm, $f(n)$ như trên; nếu không, $f(n)=0$. Dễ kiểm tra $\dfrac14f(n)$ là hàm tích tính. Giá trị tại lũy thừa nguyên tố:

1.  Nếu $p$ là $4k+3$, thì $\dfrac14f(1)=1,\dfrac14f(p)=0,\dfrac14f(p^2)=1,\dfrac14f(p^3)=0,\cdots$;
2.  Nếu $p$ là $4k+1$, $\dfrac14f(p^k)=k+1$;
3.  Nếu $p=2$, $\dfrac14f(2^k)=1$.

Ba trường hợp đều viết thành

$$
\dfrac14f(p^k) = \sum_{j=0}^k\left(\dfrac{-4}{p}\right)^k = \sum_{j=0}^k\left(\dfrac{-4}{p^k}\right) = \sum_{d\mid p^k}\left(\dfrac{-4}{d}\right).
$$

Vì Kronecker $\left(\dfrac{-4}{n}\right)$ là hàm hoàn toàn tích tính, suy ra

$$
f(n) = 4\sum_{d\mid n}\left(\dfrac{-4}{d}\right)=4\sum_{d\mid n}\chi_{4,3}(d).
$$

Trong đó $\chi_{4,3}(d)$ là ký hiệu Kronecker, cũng là ký tự Dirichlet thực mod $4$.

### Phương trình Pythagore

Dùng số Gauss có thể tìm nghiệm tổng quát của

$$
x^2+y^2=z^2.
$$

Điều này tương đương $N(x+y\mathrm{i})=z^2$. Giả sử

$$
z=2^kp_1^{r_1}\cdots p_\ell^{r_\ell}q_1^{s_1}\cdots q_{m}^{s_m},
$$

thì trong $\mathbf Z[\mathrm{i}]$,

$$
x+y\mathrm{i} = u(1+\mathrm{i})^{2k}(a_1+b_1\mathrm{i})^{r_{1}^+}(a_1-b_1\mathrm{i})^{r_{1}^-}\cdots(a_\ell+ b_\ell\mathrm{i})^{r_\ell^+}(a_\ell-b_\ell\mathrm{i})^{r_\ell^-}q_1^{s_1}\cdots q_{m}^{s_m}
$$

và $r_j^++r_j^-=2r_j$. Tính gcd giữa $x+y\mathrm{i}$ và $z$ trong $\mathbf Z$, ta được

$$
\kappa = 2^kp_1^{\min\{r_1^+,r_1^-\}}\cdots p_\ell^{\min\{r_\ell^+,r_\ell^-\}}q_1^{s_1}\cdots q_m^{s_m}.
$$

Chia cho $\kappa$, ta thấy $\kappa^{-1}(x+y\mathrm{i})$ chỉ còn thừa số dạng $a_j\pm b_j\mathrm{i}$, và các thừa số liên hợp không đi theo cặp, số mũ $|r_j^+-r_j^-|$ là chẵn. Do đó $\kappa^{-1}(x+y\mathrm{i})$ là bình phương của một số Gauss $u+v\mathrm{i}$. Suy ra

$$
x+y\mathrm{i} = \kappa(u+v\mathrm{i})^2,\ z=\kappa N(u+v\mathrm{i}).
$$

Trong $\mathbf Z$:

$$
x = \kappa(u^2-v^2),\ y = 2\kappa uv,\ z = \kappa(u^2+v^2).
$$

Ngược lại, mọi $u,v$ nguyên cho nghiệm. Đây là nghiệm tổng quát.

Từ đó, với bộ nguyên tố nguyên thủy $(x,y,z)$ (gcd = 1), $x,y$ khác parity, $z$ lẻ và chỉ có thừa số $4k+1$.

Cách tương tự cho $x^2+y^2=z^3$ hoặc chứng minh $x^4+y^4=z^4$ vô nghiệm, thậm chí mạnh hơn: $x^4+y^4=z^2$ vô nghiệm (vô hạn hạ).

## Số nguyên Eisenstein

Đặt $\omega=\dfrac{-1+\sqrt{3}\mathrm{i}}{2}=e^{2\pi\mathrm{i}/3}$.[^omega] Trường $\mathbf Q(\sqrt{3}\mathrm{i})$ là [trường phân chia](../algebra/field-theory.md#分圆域) bậc 3 và 6, số nguyên đại số tương ứng gọi là số nguyên Eisenstein. Tập của chúng tạo thành vành $\mathbf Z[\omega]$ gọi là vành số nguyên Eisenstein. Đơn vị có 6 phần tử: $\pm 1$, $\pm\omega$, $\pm\omega^2$. Trên mặt phẳng phức, chúng tạo lưới tam giác, không nhất thiết là điểm nguyên.

![](./images/eisenstein-integer.svg)

Chuẩn của số Eisenstein:

$$
N(a+b\omega) = a^2-ab+b^2,
$$

cũng là bình phương khoảng cách tới gốc trong lưới trên.

Số Eisenstein tương tự số Gauss: có thể định nghĩa phép chia có dư theo chuẩn, suy ra Euclid, Bezout, phân tích duy nhất. Có thể suy ra nguyên tố Eisenstein. Vì định thức biệt của $\mathbf Z[\omega]$ là $-3$, ký hiệu Kronecker

$$
\left(\dfrac{-3}{n}\right) = \begin{cases}
+1, & n\equiv 1\pmod 3,\\
-1, & n\equiv 2\pmod 3,\\
0,  & 3\mid n,
\end{cases}
$$

nên nguyên tố Eisenstein có 3 loại:

1.  Nguyên tố $3k+2$ trong $\mathbf Z$ (tức $2$ và các $6k+5$);
2.  Hai thừa số Eisenstein liên hợp của nguyên tố $3k+1$ (tức $6k+1$);
3.  Thừa số $(3+\sqrt{3}\mathrm{i})/2$ của $3$, liên hợp của nó liên hợp nhau.

Tương tự, số Eisenstein có chuẩn $n$ là

$$
f(n)=6\sum_{d\mid n}\left(\dfrac{-3}{d}\right)=6\sum_{d\mid n}\chi_{3,2}(d). 
$$

Trong đó $\chi_{3,2}(n)=\left(\dfrac{-3}{n}\right)$ là ký tự Dirichlet thực mod $3$. Điều này cho biết chỉ tồn tại khi mọi thừa số $3k+2$ của $n$ có số mũ chẵn.

Theo chuẩn, $f(n)$ là số nghiệm của $x^2-xy+y^2=n$ hoặc $x^2+xy+y^2=n$. Hình học: số điểm nguyên trên elip nghiêng $x^2\pm xy+y^2=n$.

Liên quan là phương trình $x^2+3y^2=n$ (elip chuẩn). Đặt $x=(u+v)/2$, $y=(u-v)/2$ thì đổi thành $u^2-uv+v^2=n$. Tuy nhiên nghiệm nguyên của $x^2+3y^2=n$ không phải lúc nào cũng tương ứng 1-1 với nghiệm của $u^2-uv+v^2=n$. Nếu $n$ chẵn thì $u,v$ đều chẵn, suy ra $x,y$ nguyên, nên số nghiệm vẫn là $f(n)$. Nếu $n$ lẻ, $u,v$ có thể khác parity, khi đó $x,y$ có thể là nửa nguyên. Cần phân tích kỹ.

Với một nghiệm $(u,v)$ của $u^2-uv+v^2=n$, các số Eisenstein liên hợp với $u+v\omega$ (kể cả chính nó) tương ứng với các nghiệm

$$
(u,v),(u-v,u),(-v,u-v),(-u,-v),(v-u,-u),(v,v-u).
$$

Ba số $u,v,u-v$ nếu không đồng thời chẵn thì có đúng hai số lẻ và một số chẵn, nên trong 6 nghiệm có 2 nghiệm toàn lẻ, 4 nghiệm khác parity. Do đó khi $n$ lẻ, chỉ $\dfrac13$ nghiệm của $u^2-uv+v^2=n$ là toàn lẻ và mới tương ứng với nghiệm nguyên của $x^2+3y^2=n$. Vì vậy, khi $n$ lẻ, số nghiệm của $x^2+3y^2=n$ là $\dfrac13f(n)$.

Cuối cùng, tương tự phương trình Pythagore, có thể dùng Eisenstein để giải:

$$
x^2-xy+y^2=z^2
$$

$$
x^2+xy+y^2=z^2
$$

$$
x^2+3y^2=z^2
$$

$$
x^2+3y^2=z^3
$$

Các nghiệm tổng quát không trình bày ở đây. Cách tương tự cũng chứng minh $x^3+y^3=z^3$ vô nghiệm.

## Tài liệu tham khảo và chú thích

-   [Quadratic field - Wikipedia](https://en.wikipedia.org/wiki/Quadratic_field)
-   [Quadratic integer - Wikipedia](https://en.wikipedia.org/wiki/Quadratic_integer)
-   [Gaussian integer - Wikipedia](https://en.wikipedia.org/wiki/Gaussian_integer)
-   [Eisenstein integer - Wikipedia](https://en.wikipedia.org/wiki/Eisenstein_integer)
-   [Kronecker symbol - Wolfram MathWorld](https://mathworld.wolfram.com/KroneckerSymbol.html)
-   [Dirichlet character - Wikipedia](https://en.wikipedia.org/wiki/Dirichlet_character)
-   [Theodorus J. Dekker's Notes on Primes in Quadratic Fields](https://staff.science.uva.nl/t.j.dekker/PrimesPaper/Primes.pdf)
-   [Franz Lemmermeyer's Notes on Ideals in Quadratic Number Fields](http://www.fen.bilkent.edu.tr/~franz/ant/ant02.pdf)
-   [J.S. Milne - Algebraic Number Theory](https://www.jmilne.org/math/CourseNotes/ANT301.pdf)

[^omega]: Lưu ý: ở đây $\omega$ khác với ký hiệu ở trên. Theo thông lệ, với vành số nguyên bậc hai thường đặt $\omega=(1+\sqrt{d})/2$ (khi $d\equiv 1\pmod4$), còn với Eisenstein lại đặt $\omega=(-1+\sqrt{-3})/2$. Khác biệt này không ảnh hưởng bản chất, nhưng có thể làm thay đổi hình thức biểu thức. Không nên viết vành Eisenstein là $\mathbf Z[\sqrt{-3}]$.
