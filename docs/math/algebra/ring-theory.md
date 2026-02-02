Kiến thức trước: [Khái niệm cơ bản về đại số trừu tượng](./basic.md)、[Lý thuyết nhóm](./group-theory.md)

## Giới thiệu

**Lý thuyết vành** (ring theory) nghiên cứu các loại vành khác nhau.

Nội dung bài này liên quan chặt chẽ đến lý thuyết chia hết trong số học. Trước hết, tương tự nhóm con chuẩn, ta giới thiệu hạt nhân của đồng cấu vành, gọi là ideal (lý tưởng); thực ra đây là khái niệm số trong số học được mở rộng cho vành. Sau đó, ta mở rộng các khái niệm như số nguyên tố, thuật toán Euclid, phân tích thừa số nguyên tố từ vành số nguyên sang vành tổng quát, dẫn đến các loại miền nguyên khác nhau.

Nhiều kết luận trong số học vẫn đúng trên các vành quen thuộc. Có thể nói một phần của lý thuyết vành là nghiên cứu điều kiện để các kết luận số học vẫn đúng trên vành; nếu không đúng thì cần hạn chế gì lên vành.

???+ info "Ký hiệu"
    Khi không gây nhầm lẫn, bài viết có thể lược bỏ ký hiệu nhân trong vành, và viết vành $(R,+,\cdot)$ thành $R$. Trong vành $R$, phần tử đơn vị của phép cộng gọi là phần tử không, ký hiệu $0$; phần tử đơn vị của phép nhân gọi là phần tử đơn vị, ký hiệu $1$.

??? warning "Định nghĩa vành ở đây không yêu cầu có đơn vị"
    Lưu ý bài viết không yêu cầu vành phải có đơn vị. Một số tài liệu yêu cầu có đơn vị; khi đó một số kết luận cần điều chỉnh. Ví dụ, bài viết định nghĩa ideal dựa trên vành con, còn nơi khác có thể dựa trên nhóm con cộng.

## Ideal

Tương tự nhóm, ta có thể định nghĩa vành con và đồng cấu vành.

???+ abstract "Vành con"
    Với vành $(R,+,\cdot)$ và tập con $S$, nếu $(S,+,\cdot)$ cũng là một vành, thì $S$ là **vành con** (subring) của $R$.

???+ example "Ví dụ: vành số nguyên $\mathbf Z$"
    Với mọi số nguyên $n$, $n\mathbf Z=\{nk:k\in\mathbf Z\}$ là vành con của $\mathbf Z$.

???+ abstract "Đồng cấu vành"
    Với vành $(R,+,\cdot)$ và $(S,\oplus,\odot)$, nếu ánh xạ $\pi$ bảo toàn cộng và nhân, tức $\pi(r_1+r_2)=\pi(r_1)\oplus\pi(r_2)$ và $\pi(r_1\cdot r_2)=\pi(r_1)\odot\pi(r_2)$ với mọi $r_1,r_2\in R$, thì $\pi:R\rightarrow S$ gọi là **đồng cấu** (homomorphism) từ $R$ sang $S$.

??? info "Khi vành có đơn vị"
    Nếu định nghĩa vành yêu cầu đơn vị, thì đồng cấu thường yêu cầu gửi đơn vị sang đơn vị. Với đồng cấu giữa các vành có đơn vị khác 0, điều này chỉ đảm bảo đồng cấu không gửi cả vành về 0.

???+ example "Ví dụ: vành số nguyên $\mathbf Z$ (tiếp)"
    Với mọi số nguyên khác 0 $n$, ánh xạ lấy modulo $\pi:\mathbf Z\rightarrow\mathbf Z/n\mathbf Z$, $\pi(a)=\bar a$, là đồng cấu vành.

Các thảo luận về hạt nhân và ảnh của [đồng cấu nhóm](./group-theory.md#群同态) gần như giữ nguyên. Ảnh quyết định tính surjective, hạt nhân quyết định tính injective. Hạt nhân của đồng cấu vành:

???+ abstract "Hạt nhân đồng cấu"
    Với đồng cấu $\pi:R\rightarrow S$, **hạt nhân** (kernel) là $\ker\pi=\{r\in R:\pi(r)=0\}$, trong đó $0$ là phần tử đơn vị cộng của $S$.

Hạt nhân và ảnh đều là vành con. Không phải mọi vành con đều là hạt nhân của một đồng cấu. Những vành con có thể là hạt nhân gọi là ideal.

???+ abstract "Ideal"
    Với vành $R$ và vành con $I$, gọi $I$ là
    
    -   **ideal trái** (left ideal) nếu với mọi $r\in R$, $rI\subseteq I$, trong đó $rI=\{ra:a\in I\}$;
    -   **ideal phải** (right ideal) nếu với mọi $r\in R$, $Ir\subseteq I$, trong đó $Ir=\{ar:a\in I\}$;
    -   **ideal** nếu $I$ vừa là ideal trái vừa là ideal phải.

Điều kiện đóng dưới nhân trái/phải là tự nhiên vì phần tử trong ideal sẽ bị ánh xạ về 0 trong đồng cấu, và nhân với 0 luôn là 0. Điều kiện này cũng đủ do cấu trúc cộng là nhóm Abel (mọi nhóm con là chuẩn), và nhân không đặt thêm ràng buộc lên cấu trúc con.

???+ example "Ví dụ: vành số nguyên $\mathbf Z$ (tiếp)"
    Trong $\mathbf Z$, vành con $n\mathbf Z$ là ideal. Mọi bội của $n$ nhân với số nguyên bất kỳ vẫn là bội của $n$. Thực tế, mọi ideal của $\mathbf Z$ đều dạng này, gọi là [miền ideal chính](#主理想整环). Với vành tổng quát, có ideal không phải tập các bội của một phần tử; đó là động cơ nghiên cứu ideal[^ideal-history].

### Vành thương

Giống như nhóm, từ ideal có thể định nghĩa **vành thương** (quotient ring). Xét

$$
R/I=\{a+I:a\in R\},
$$

với $a+I=\{a+b:b\in I\}$. Khi và chỉ khi $I$ là ideal, phép toán

$$
\begin{aligned}
(a+I)+(b+I)&=(a+b)+I,\\
(a+I)(b+I)&=(ab)+I
\end{aligned}
$$

là xác định tốt. Khi đó $R/I$ là vành. Có **định lý đẳng cấu thứ nhất** và đồng cấu tự nhiên từ $R$ tới $R/I$, tương tự nhóm.

???+ note "Định lý đẳng cấu thứ nhất"
    Với đồng cấu $\pi:R\rightarrow S$, $\ker\pi$ là ideal của $R$, và $R/\ker\pi\cong\pi(R)$ là vành con của $S$.

???+ abstract "Đồng cấu tự nhiên"
    Với vành $R$ và ideal $I$, ánh xạ $\pi(r)=r+I$ là đồng cấu đầy đủ từ $R$ đến $R/I$, gọi là **đồng cấu tự nhiên** (natural homomorphism).

???+ example "Ví dụ: vành số nguyên $\mathbf Z$ (tiếp)"
    Vành $\mathbf Z/n\mathbf Z$ là vành thương của $\mathbf Z$ theo ideal $n\mathbf Z$. Điều này giải thích ký hiệu $\mathbf Z/n\mathbf Z$. Ánh xạ modulo $n$ là đồng cấu tự nhiên, hạt nhân là $n\mathbf Z$.

Các định lý đẳng cấu khác cũng đúng cho vành.

???+ note "Định lý đẳng cấu thứ hai"
    Với vành $R$ có vành con $A$ và ideal $B$, thì $A+B=\{a+b:a\in A,b\in B\}$ là vành con, $A\cap B$ là ideal của $A$, $B$ là ideal của $A+B$, và $(A+B)/B\cong A/(A\cap B)$.

???+ note "Định lý đẳng cấu thứ ba"
    Với vành $R$ có ideal $I,J$ và $I\subseteq J$, thì $J/I$ là ideal của $R/I$ và $(R/I)/(J/I)\cong R/J$.

???+ note "Định lý tương ứng"
    Với vành $R$ có ideal $I$, tập các vành con chứa $I$ là $\mathcal S=\{S:I\subseteq S\subseteq R\}$ và tập các nhóm con của $R/I$ là $\mathcal T=\{T:T\le R/I\}$. Tồn tại song ánh $\varphi:\mathcal S\rightarrow\mathcal T$ gửi $S$ thành $S/I$. Song ánh giữ quan hệ bao hàm, và ideal của $R$ luôn gửi thành ideal của $R/I$.

Các định lý này là nền tảng trong phần sau.

### Phép toán trên ideal

Ideal có các phép toán tương tự gcd/lcm.

???+ abstract "Phép toán trên ideal"
    Với ideal $I,J$ của $R$, định nghĩa:
    
    -   **Tổng**: $I+J=\{a+b:a\in I,b\in J\}$;
    -   **Tích**: $IJ=\{\sum_{i=1}^na_ib_i:a_i\in I,b_i\in J\}$;
    -   **Giao**: $I\cap J$.

Các phép toán cho ra ideal.

???+ example "Ví dụ: $\mathbf Z$ (tiếp)"
    Với $n\mathbf Z$ và $m\mathbf Z$:
    
    $$
    \begin{aligned}
    n\mathbf Z+m\mathbf Z&=\gcd(m,n)\mathbf Z,\\
    (n\mathbf Z)(m\mathbf Z)&=(mn)\mathbf Z,\\
    (n\mathbf Z)\cap(m\mathbf Z)&=\mathrm{lcm}(m,n)\mathbf Z.
    \end{aligned}
    $$

Tổng quát,

$$
IJ\subseteq I\cap J\subseteq I,J\subseteq I+J.
$$

Nhờ đó có thể mở rộng định lý CRT sang vành, nhưng trước hết cần mở rộng khái niệm số nguyên tố và nguyên tố cùng nhau.

### Ideal tối đại

Cấu trúc ideal phản ánh tính chất của vành.

Vành khác 0 $R$ luôn có hai ideal tầm thường: $\{0\}$ và $R$. Nếu $R$ còn giao hoán, chỉ những vành này là trường[^simple-ring].

???+ note "Định lý"
    Với vành giao hoán khác 0 có đơn vị $R$, $R$ là trường khi và chỉ khi $R$ chỉ có ideal tầm thường $\{0\}$ và $R$.

??? note "Chứng minh"
    Nếu $R$ là trường, mọi ideal khác 0 chứa phần tử $a\neq0$, nên với mọi $r\in R$, $r=(ra^{-1})a\in I$, suy ra $I=R$. Ngược lại, với mọi $a\neq0$, ideal $aR$ bằng $R$, nên tồn tại $b$ sao cho $ab=1$, tức $a$ khả nghịch. Vậy $R$ là trường.

Điều kiện giao hoán là cần thiết; nếu không, phải yêu cầu ideal trái và phải đều tầm thường thì mới là vành chia.

Trong trường hợp $R$ không phải trường, ta xét vành thương. Nếu $R/I$ là trường thì không có ideal nằm giữa $I$ và $R$. Đây chính là ideal tối đại.

???+ abstract "Ideal tối đại"
    Với vành $R$ và ideal $M$, nếu $M\neq R$ và mọi ideal chứa $M$ chỉ có $M$ và $R$, thì $M$ là **ideal tối đại** (maximal ideal).

???+ note "Định lý"
    Với vành giao hoán khác 0 có đơn vị $R$ và ideal $M$, $R/M$ là trường khi và chỉ khi $M$ là ideal tối đại.

???+ example "Ví dụ: $\mathbf Z$ (tiếp)"
    Ideal $n\mathbf Z$ là tối đại khi và chỉ khi $n$ là số nguyên tố. Khi đó $\mathbf Z/p\mathbf Z$ là trường, ký hiệu $\mathbf F_p$.

Không phải mọi vành đều có ideal tối đại, nhưng mọi vành có đơn vị khác 0 thì có.

???+ note "Định lý (Krull)"
    Với vành có đơn vị khác 0 $R$ và ideal $I\neq R$, luôn tồn tại ideal tối đại $M$ sao cho $I\subseteq M$.

??? note "Chứng minh"
    Dùng bổ đề Zorn. Xét tập các ideal thật $\mathcal S$ chứa $I$ (không bằng $R$). $\mathcal S$ không rỗng và được sắp theo bao hàm. Với mọi chuỗi $J_0\subseteq J_1\subseteq\cdots$, hợp $J$ của chúng là ideal, và $J\neq R$; nếu $1\in J$ thì $1\in J_n$ nào đó, mâu thuẫn $J_n$ là ideal thật. Do Zorn, tồn tại phần tử cực đại $M$, tức ideal tối đại.

Ideal tối đại tương tự phần tử bất khả quy trong số học: quan hệ bao hàm giữa ideal tương ứng với chia hết. Nhưng ideal tối đại rộng hơn bất khả quy vì không phải mọi ideal là principal.

### Ideal nguyên tố

Điều kiện trường mạnh hơn miền nguyên. Ideal làm cho vành thương là miền nguyên gọi là ideal nguyên tố, tương ứng số nguyên tố.

???+ abstract "Ideal nguyên tố"
    Với vành giao hoán $R$ và ideal $P$, nếu $P\neq R$ và với mọi $a,b\in R$, $ab\in P$ suy ra $a\in P$ hoặc $b\in P$, thì $P$ là **ideal nguyên tố** (prime ideal).

???+ note "Định lý"
    Với vành giao hoán khác 0 có đơn vị $R$ và ideal $P$, $R/P$ là miền nguyên khi và chỉ khi $P$ là ideal nguyên tố.

??? note "Chứng minh"
    $R/P$ là miền nguyên khi và chỉ khi không có ước của 0, tức $\bar a\bar b=\bar 0$ suy ra $\bar a=\bar 0$ hoặc $\bar b=\bar 0$. Theo tương ứng, điều này đúng khi và chỉ khi $ab\in P$ suy ra $a\in P$ hoặc $b\in P$.

Trong $\mathbf Z$, $n\mathbf Z$ là ideal tối đại và nguyên tố khi và chỉ khi $n$ là số nguyên tố. Trong vành giao hoán tổng quát, ideal tối đại luôn là nguyên tố, nhưng ngược lại không nhất thiết.

???+ note "Định lý"
    Với vành giao hoán khác 0 có đơn vị $R$, mọi ideal tối đại đều là ideal nguyên tố.

Sẽ thấy rằng trong các vành “tốt” giống $\mathbf Z$, điều ngược lại cũng đúng.

### Ideal chính

Cần xét ideal sinh bởi một tập con.

???+ abstract "Ideal sinh bởi tập con"
    Với vành có đơn vị khác 0 $R$ và tập con không rỗng $A\subseteq R$, nếu $I$ là ideal nhỏ nhất chứa $A$, thì $I$ gọi là **ideal sinh bởi $A$** (ideal generated by a subset), ký hiệu $(A)$. Khi đó $A$ là **tập sinh** của $(A)$.

???+ abstract "Ideal chính"
    Ideal sinh bởi một phần tử $a\in R$ gọi là **ideal chính** (principal ideal), ký hiệu $(a)$. Phần tử $a$ gọi là **phần tử sinh**.

Với tập $A$, ta có

$$
\begin{aligned}
RA&=\{r_1a_1+\cdots+r_na_n:r_i\in R,a_i\in A,n\in\mathbf Z\},\\
AR&=\{a_1r_1+\cdots+a_nr_n:r_i\in R,a_i\in A,n\in\mathbf Z\}.
\end{aligned}
$$

Đây là ideal trái và phải do $A$ sinh. Ideal do $A$ sinh là $RAR$. Với vành giao hoán, các khái niệm trùng nhau.

Trong $\mathbf Z$, các ideal $n\mathbf Z$ đều là principal, thường ký hiệu $(n)$.

## Miền nguyên

Miền nguyên là vành giao hoán có đơn vị, không có ước của 0, khác 0. Đây là khái niệm tổng quát hóa vành số nguyên. Nhưng chưa đủ để chuyển mọi kết luận số học sang. Để làm vậy, cần hạn chế thêm. Ba lớp phổ biến: miền Euclid, miền ideal chính, miền phân tích duy nhất; lớp trước chứa trong lớp sau.

### Quan hệ chia hết

Mở rộng khái niệm chia hết.

???+ abstract "Chia hết"
    Với vành giao hoán $R$, nếu tồn tại $x\in R$ sao cho $a=bx$, thì nói $b$ **chia hết** $a$, ký hiệu $b\mid a$. Khi đó $b$ là **ước** của $a$.

???+ abstract "Kết hợp"
    Với $R$, nếu $a=bu$ với $u$ khả nghịch, thì $a$ và $b$ **kết hợp** (associate).

Chia hết là quan hệ bán thứ tự, kết hợp là quan hệ tương đương. Ở mức ideal, $a\mid b$ tương đương $(b)\subseteq(a)$, và kết hợp tương đương $(a)=(b)$. Vì vậy thường xét các phần tử theo lớp kết hợp. GCD là cận dưới lớn nhất.

???+ abstract "Ước chung lớn nhất"
    Với $R$ và $a,b\in R$, nếu tồn tại $d\neq0$ sao cho $d\mid a$ và $d\mid b$, và mọi $d'$ chia cả $a$ và $b$ đều chia $d$, thì $d$ là **ước chung lớn nhất** (gcd), ký hiệu $\gcd(a,b)$.

Trong miền nguyên, gcd là duy nhất theo nghĩa kết hợp.

Trong miền nguyên, có hai khái niệm tương ứng với số nguyên tố:

???+ abstract "Phần tử nguyên tố"
    Với miền nguyên $R$, phần tử $p\neq0$ là **nguyên tố** (prime) nếu $(p)$ là ideal nguyên tố, tức $p$ không khả nghịch và $p\mid ab$ suy ra $p\mid a$ hoặc $p\mid b$.

???+ abstract "Phần tử bất khả quy"
    Với miền nguyên $R$, phần tử $r\neq0$ là **bất khả quy** (irreducible) nếu $r$ không khả nghịch và mọi phân tích $r=ab$ đều có $a$ hoặc $b$ khả nghịch. Nếu tồn tại $r=ab$ với $a,b$ đều không khả nghịch thì $r$ là khả quy.

Phần tử bất khả quy tương ứng ideal chính tối đại trong lớp ideal chính. Nhưng vì không phải mọi ideal là principal, nên bất khả quy không tương đương ideal tối đại.

Có kết luận:

???+ note "Định lý"
    Nếu $R$ là miền nguyên và $a\in R$ nguyên tố thì $a$ cũng bất khả quy.

??? note "Chứng minh"
    Giả sử $r$ nguyên tố và $r=ab$. Vì $r$ nguyên tố, $r\mid a$ hoặc $r\mid b$, giả sử $r\mid a$, thì $a=cr=cba$. Do miền nguyên có luật khử, suy ra $1=bc$ nên $b$ khả nghịch. Vậy $r$ bất khả quy.

Chiều ngược không đúng.

??? example "Phản ví dụ"
    Trong vành số nguyên bậc hai $\mathbf Z[\sqrt{-5}]$, $3$ bất khả quy nhưng $9=3\cdot3=(2+\sqrt{-5})(2-\sqrt{-5})$, nên $3$ không nguyên tố.
    
    Chứng minh: gọi $N(\cdot)$ là chuẩn. Với mọi phân tích $3=ab$, có $N(a)N(b)=N(3)=9$. Nếu $a,b$ đều không khả nghịch thì $N(a),N(b)>1$ nên $N(a)=N(b)=3$. Nhưng $x^2+5y^2=3$ không có nghiệm nguyên, nên không tồn tại $a,b$ như vậy. Suy ra $3$ bất khả quy. $3$ không nguyên tố vì không chia $2\pm\sqrt{-5}$.

### Miền Euclid

Đọc thêm: [Euclid mở rộng](../number-theory/gcd.md)、[Định lý Bézout](../number-theory/bezouts.md)

Miền Euclid là miền cho phép thuật toán chia có dư.

???+ abstract "Miền Euclid"
    Miền nguyên $R$ là **miền Euclid** (Euclidean domain, ED) nếu tồn tại ánh xạ $N:R\setminus\{0\}\rightarrow\mathbf N$ sao cho với mọi $a,b\in R$, $b\neq0$, tồn tại $q,r\in R$ với $a=qb+r$ và $r=0$ hoặc $N(r)<N(b)$. Hàm $N$ gọi là chuẩn (norm).

??? info "Các định nghĩa tương đương"
    Ở đây $N$ chỉ định nghĩa trên phần tử khác 0. Một số tài liệu thêm $N(0)=0$, nhưng không ảnh hưởng. Một số tài liệu yêu cầu thêm $N(a)\le N(ab)$. Nếu $N$ thỏa điều kiện ở đây, có thể định nghĩa $N'(a)=\min_{b\neq0} N(ab)$ để thỏa thêm $N'(a)\le N'(ab)$. Vì vậy các định nghĩa tương đương.

Đây là tổng quát hóa phép chia có dư. Chuẩn đo “độ lớn” của dư để thuật toán Euclid kết thúc.

Do đó trong miền Euclid có thể tính gcd hiệu quả. Tương tự số nguyên, thuật toán Euclid cho gcd, và định lý Bézout đúng, hệ số tìm bằng Euclid mở rộng.

???+ note "Định lý"
    Với miền Euclid $R$ và $a,b\in R$, thuật toán Euclid cho gcd $d$, và tồn tại $x,y\in R$ sao cho $d=ax+by$. Ngược lại, mọi $ax+by$ đều là bội của $d$.

Tức là ideal $(a,b)$ là principal $(d)$.

Miền Euclid có mọi ideal là principal.

???+ note "Định lý"
    Mọi ideal trong miền Euclid đều là ideal chính.

??? note "Chứng minh"
    Với ideal $I\neq\{0\}$, chọn $d\in I$ khác 0 có $N(d)$ nhỏ nhất. Với mọi $a\in I$, chia $a=qd+r$, $r=0$ hoặc $N(r)<N(d)$. Nhưng $r=a-qd\in I$, nên do tối tiểu phải có $r=0$, tức $a=qd\in(d)$. Vậy $I=(d)$.

### Miền ideal chính

Miền nguyên mà mọi ideal đều là principal gọi là miền ideal chính.

???+ abstract "Miền ideal chính"
    Miền nguyên $R$ là **miền ideal chính** (principal ideal domain, PID) nếu mọi ideal là principal.

Hệ quả từ phần trước:

???+ note "Định lý"
    Miền Euclid luôn là miền ideal chính.

Trong PID, ideal tối đại tương đương ideal sinh bởi phần tử bất khả quy. Tương tự số nguyên, trong PID, nguyên tố và bất khả quy là tương đương, nên ideal tối đại và ideal nguyên tố cũng tương đương.

???+ note "Định lý"
    Với PID $R$ và ideal khác 0 $I$, $I$ là ideal nguyên tố khi và chỉ khi $I$ là ideal tối đại.

??? note "Chứng minh"
    Chỉ cần chứng minh ideal nguyên tố là tối đại. Với ideal nguyên tố $(p)$ và ideal $(a)$ sao cho $(p)\subseteq(a)\subseteq R$, ta có $a\mid p$, nên $p=ab$. Vì $(p)$ nguyên tố, $ab\in(p)$ suy ra $a\in(p)$ hoặc $b\in(p)$. Nếu $a\in(p)$ thì $(a)=(p)$. Nếu $b\in(p)$ thì $b=cp$, suy ra $p=acp$ và vì $p\neq0$ nên $1=ac$, tức $a$ khả nghịch, nên $(a)=R$. Vậy $(p)$ tối đại.

???+ note "Hệ quả"
    Trong PID, phần tử $r\neq0$ là nguyên tố khi và chỉ khi là bất khả quy.

Định lý Bézout cũng đúng trong PID:

???+ note "Định lý"
    Với PID $R$ và $a,b\neq0$, gọi $d$ là phần tử sinh của ideal $(a,b)$. Khi đó $d$ là gcd của $a$ và $b$ (duy nhất theo kết hợp), và tồn tại $x,y$ sao cho $ax+by=d$.

Khác biệt giữa ED và PID là ED có thuật toán Euclid hiệu quả, còn PID không nhất thiết.

### Miền phân tích duy nhất

Khái niệm tổng quát hơn PID là **miền phân tích duy nhất** (UFD). Trong số nguyên là định lý cơ bản của số học. Một số vành không là PID nhưng vẫn UFD.

???+ abstract "Miền phân tích duy nhất"
    Miền nguyên $R$ là **UFD** nếu mọi phần tử khác 0 và không khả nghịch $r$ có thể viết $r=p_1\cdots p_n$ với $p_i$ bất khả quy (có thể lặp), và phân tích là duy nhất theo nghĩa kết hợp và hoán vị.

Trong UFD, nguyên tố và bất khả quy tương đương.

???+ note "Định lý"
    Với UFD $R$ và $a\neq0$, $a$ nguyên tố khi và chỉ khi $a$ bất khả quy.

??? note "Chứng minh"
    Chỉ cần chứng minh bất khả quy $\Rightarrow$ nguyên tố. Nếu $r$ bất khả quy và $r\mid ab$, thì $ab=rc$. Phân tích $a,b,c$ thành tích bất khả quy, so sánh hai vế theo tính duy nhất, suy ra $r$ kết hợp với một bất khả quy của $a$ hoặc $b$, nên $r$ chia $a$ hoặc $b$.

Mọi PID đều là UFD.

???+ note "Định lý"
    Miền ideal chính luôn là miền phân tích duy nhất.

??? note "Chứng minh"
    Chứng minh tồn tại phân tích và tính duy nhất. Tồn tại: nếu $r$ đã bất khả quy thì xong, nếu không thì $r=r_1r_2$ với $r_1,r_2$ không khả nghịch. Tiếp tục phân tích cho đến khi tất cả bất khả quy. Quá trình kết thúc vì nếu không sẽ tạo chuỗi ideal chính tăng vô hạn $(r)\subsetneq(r_1)\subsetneq\cdots$, nhưng hợp của chuỗi vẫn là ideal chính, mâu thuẫn. Tính duy nhất: quy nạp theo số lượng thừa số, dùng kết luận trong PID: bất khả quy là nguyên tố. Nếu $r=p_1\cdots p_n=q_1\cdots q_m$, $n\le m$, thì $p_1$ chia một $q_j$, suy ra $p_1$ kết hợp $q_j$. Khử chúng đi, áp dụng quy nạp.

GCD vẫn tồn tại trong UFD:

???+ note "Định lý"
    Với UFD $R$ và $a,b\neq0$, viết $a=up_1^{r_1}\cdots p_n^{r_n}$, $b=vp_1^{s_1}\cdots p_n^{s_n}$ (các $p_i$ khác nhau), thì một gcd là $d=p_1^{\min\{r_1,s_1\}}\cdots p_n^{\min\{r_n,s_n\}}$.

Điều này cho thấy “tồn tại gcd” yếu hơn UFD[^gcd-domain].

### Ví dụ: vành số nguyên bậc hai

Đọc thêm: [trường bậc hai](../number-theory/quadratic.md)

Đại số trừu tượng cần ví dụ. Nghiên cứu định lý Fermat đã thúc đẩy lý thuyết vành[^ring-theory-history]. Ta xét số nguyên đại số bậc hai.

**Số nguyên bậc hai** là nghiệm của phương trình bậc hai đơn nhất hệ số nguyên $\alpha^2+b\alpha+c=0$. Mọi số nguyên bậc hai có dạng

$$
\alpha=a+b\omega,~(a,b\in\mathbf Z)
$$

với

$$
\omega=\begin{cases}
\dfrac{1+\sqrt{D}}{2},& D\equiv 1\pmod 4,\\
\sqrt D,& D\equiv 2,3\pmod 4,
\end{cases}
$$

$D$ không có thừa số bình phương.

??? note "Phân tích"
    Từ công thức nghiệm:
    
    $$
    \alpha=\frac{-b\pm\sqrt{b^2-4c}}{2}.
    $$
    
    Nếu $b=2k+1$ lẻ:
    
    $$
    \alpha=-k-\frac{1\pm\sqrt{4(k^2+k-c)+1}}{2}.
    $$
    
    Nếu $b=2k$ chẵn:
    
    $$
    \alpha=-k\pm\sqrt{k^2-c}.
    $$
    
    Suy ra dạng trên.

Dễ thấy $\mathbf Z[\omega]=\{a+b\omega:a,b\in\mathbf Z\}$ là vành, gọi là **vành số nguyên bậc hai** (quadratic integer ring). Trường phân thức là $\mathbf Q(\sqrt D)$. Nếu $D>0$ thì các phần tử là thực, gọi là vành số nguyên bậc hai thực; nếu $D<0$ thì (ngoài số nguyên) là phức, gọi là vành số nguyên bậc hai ảo.

Mọi vành số nguyên bậc hai đều là miền nguyên. Với $D=-1$ thì $\mathbf Z[\sqrt{-1}]$ (hay $\mathbf Z[\mathrm{i}]$) là vành Gauss; với $D=-3$ thì $\mathbf Z\left[\dfrac{1+\sqrt{-3}}{2}\right]$ là vành Eisenstein.

Với $a+b\omega$, định nghĩa **liên hợp** là $a+b\bar\omega$ với

$$
\bar\omega=\begin{cases}
\dfrac{1-\sqrt{D}}{2},& D\equiv 1\pmod 4,\\
-\sqrt D,& D\equiv 2,3\pmod 4,
\end{cases}
$$

Lưu ý khi $D>0$, số bậc hai là thực nên liên hợp ở đây khác liên hợp phức, nhưng đều là trường hợp đặc biệt của liên hợp trong mở rộng trường. Hai phần tử liên hợp là nghiệm của cùng phương trình.

Trên vành số nguyên bậc hai, định nghĩa **chuẩn**

$$
\begin{aligned}
N(a+b\omega)&=(a+b\omega)(a+b\bar\omega)\\
&=\begin{cases}
a^2+ab+\dfrac{1-D}{4}b^2,& D\equiv 1\pmod 4,\\
a^2-Db^2,& D\equiv 2,3\pmod 4.
\end{cases}
\end{aligned}
$$

Chuẩn là số nguyên. Đặc biệt khi $D<0$, chuẩn là số tự nhiên. Chuẩn nhân: $N(ab)=N(a)N(b)$.

Đơn vị trong vành là các phần tử có chuẩn $\pm1$. Với $D>0$, tương ứng phương trình Pell $x^2-Dy^2=\pm1$ hoặc $\pm4$. Với $D<0$, ngoài trường hợp Gauss ($\{\pm1,\pm\mathrm{i}\}$) và Eisenstein ($\{\pm1,\pm\omega,\pm\omega^2\}$), các đơn vị chỉ là $\{\pm1\}$.

Chuẩn $N(\alpha)$ có thể dùng để chứng minh vành Euclid. Với $D>0$, dùng $|N(\alpha)|$ làm chuẩn. Có thể chứng minh với $D<0$ các giá trị

$$
D=-1,-2,-3,-7,-11
$$

hoặc với $D>0$:

$$
D=2, 3, 5, 6, 7, 11, 13, 17, 19, 21, 29, 33, 37, 41, 57, 73
$$

thì vành số nguyên bậc hai là Euclid theo $|N|$. Tuy nhiên chuẩn Euclid không nhất thiết là chuẩn trên. Ví dụ $D=14,69$ cũng Euclid nhưng cần chuẩn khác. Với $D<0$, các giá trị trên là tất cả trường hợp Euclid.

Dùng phương pháp phức tạp hơn, có thể xác định khi nào là PID. Với $D<0$, chỉ có

$$
D=-1,-2,-3,-7,-11,-19,-43,-67,-163
$$

là PID. So sánh cho thấy như $D=-19$ là PID nhưng không Euclid. Với $D>0$, chưa có kết quả đầy đủ.

Trong vành số nguyên bậc hai, UFD và PID là tương đương. Ví dụ $\mathbf Z[\sqrt{-5}]$ không là PID, nên không là UFD; ta đã thấy

$$
9=3\times3=(2+\sqrt{-5})\times(2-\sqrt{-5}).
$$

Cũng có thể dùng ví dụ này để thấy ideal $(3,2+\sqrt 5)$ không là principal. Một ví dụ đơn giản của UFD nhưng không PID là vành đa thức $\mathbf Z[x]$.

Dù nhiều vành số nguyên bậc hai không là UFD, chúng đều là [Dedekind domain](https://en.wikipedia.org/wiki/Dedekind_domain). Mọi ideal không tầm thường phân tích duy nhất thành tích các ideal nguyên tố. Nhưng nếu vành không PID, các ideal nguyên tố không nhất thiết tương ứng với phần tử nguyên tố, nên phân tích duy nhất của phần tử (như số nguyên) không còn; đây cũng là động cơ nghiên cứu ideal.

## Vành đa thức

Đọc thêm: [Giới thiệu kỹ thuật đa thức](../poly/intro.md)

Trong lập trình thi đấu thường gặp các phép toán đa thức. Có thể hiểu các phép toán đa thức như mở rộng phép toán số trên vành đa thức.

???+ abstract "Đa thức"
    Với vành giao hoán có đơn vị khác 0 $R$, một **đa thức** trên $R$ là biểu thức
    
    $$
    \sum_{k=0}^{n}a_kx^k = a_0+a_1x+\cdots+a_{n-1}x^{n-1}+a_nx^n,
    $$
    
    với $n\in\mathbf N$, $a_k\in R$. Các $a_k$ gọi là **hệ số**, $a_kx^k$ là **hạng** (term), $k$ là **bậc** của hạng. Đa thức có mọi hệ số bằng 0 gọi là **đa thức 0**, ký hiệu $0$. Với đa thức khác 0, lấy $a_n\neq0$, thì $n$ là **bậc** của đa thức, $a_nx^n$ là **hạng bậc cao nhất**, $a_n$ là **hệ số bậc cao nhất**. Nếu hệ số bậc cao nhất là 1 thì gọi là **đa thức monic**. Đa thức 0 có bậc không xác định hoặc quy ước $-\infty$.

Ký hiệu $x$ là **ẩn** (indeterminate), không có ý nghĩa hay phạm vi giá trị; nó chỉ đánh dấu vị trí hệ số. Đa thức có thể xem như dãy

$$
(a_0,a_1,...,a_{n-1},a_n,0,0,\cdots),
$$

với hữu hạn số hạng khác 0. Hai đa thức bằng nhau nếu dãy hệ số bằng nhau (có thể bổ sung hệ số 0).

Khi thay $a\in R$ vào đa thức $f(x)$, thu được $f(a)$ là giá trị phép tính trong $R$ sau khi thay $x\to a$.

??? info "Đa thức và hàm đa thức"
    Hai khái niệm khác nhau. Đa thức là dãy hệ số hữu hạn, không tự động là hàm. Ánh xạ “thay $x$ bằng $a$” có thể không đơn ánh. Ví dụ $f(x)=x^p-x$ trên $\mathbf F_p$ không phải đa thức 0, nhưng hàm $\mathbf F_p\rightarrow\mathbf F_p$ lại luôn bằng 0. Dù khác nhau, nhiều khái niệm của hàm đa thức vẫn áp dụng cho đa thức, như đạo hàm hình thức, tích phân hình thức, hợp, v.v. Các phép toán này không cần tô pô nhưng vẫn thỏa các quy tắc.

Với

$$
\begin{aligned}
f(x)&=a_0+a_1x+\cdots+a_{n-1}x^{n-1}+a_nx^n,\\
g(x)&=b_0+b_1x+\cdots+b_{n-1}x^{n-1}+b_nx^n,
\end{aligned}
$$

cộng:

$$
f(x)+g(x) = (a_0+b_0)+(a_1+b_1)x+\cdots+(a_{n-1}+b_{n-1})x^{n-1}+(a_n+b_n)x^n,
$$

nhân:

$$
f(x)g(x) = a_0b_0+(a_1b_0+a_0b_1)x+(a_2b_0+a_1b_1+a_0b_2)x^2+\cdots,
$$

hệ số $x^k$ là $\sum_{i=0}^ka_{k-i}b_i$. Với các phép này, tập đa thức là vành, ký hiệu $R[x]$.

Bậc đa thức $f(x)$ ký hiệu $\deg f(x)$. Đa thức hằng tương ứng với $R$ nhúng vào $R[x]$. Rõ ràng $R$ có ước của 0 khi và chỉ khi $R[x]$ có ước của 0.

???+ note "Định lý"
    $R[x]$ là miền nguyên khi và chỉ khi $R$ là miền nguyên.

Với miền nguyên $R$, có

$$
\begin{aligned}
\deg(f+g) &\le \max\{\deg f,\deg g\},\\
\deg(fg) &= \deg f + \deg g.
\end{aligned}
$$

Quy ước $\deg 0=-\infty$. Các đơn vị trong $R[x]$ là các đa thức hằng khả nghịch, và mọi đa thức bậc ≥1 không khả nghịch.

Phần sau chỉ xét đa thức trên miền nguyên.

???+ info "Quy ước"
    Dưới đây, “đa thức trên $R$” và “đa thức trong $R[x]$” được dùng như nhau. Nếu $R$ là vành con của $S$ thì đa thức trên $R$ tự động là đa thức trên $S$.

### Vành đa thức trên trường

Trường $F$ cho phép chia hệ số, nên $F[x]$ có phép chia có dư. Đặt chuẩn $N(f)=\deg f$. Với $f,g\in F[x]$, $g\neq0$:

$$
f(x)=g(x)q(x)+r(x),
$$

với $\deg r<\deg g$ hoặc $r=0$. Vậy $F[x]$ là miền Euclid.

???+ note "Định lý"
    $F[x]$ là miền Euclid, PID và UFD.

Trong thi lập trình, thường dùng $\mathbf F_p[x]$ với $p$ nguyên tố; khi đó phép chia có dư hợp lệ. Nếu modulo $n$ bất kỳ, $(\mathbf Z/n\mathbf Z)[x]$ thậm chí không là miền nguyên.

Có phép chia có dư nghĩa là nghiệm tương ứng nhân tử bậc một.

???+ abstract "Nghiệm"
    **Nghiệm** của đa thức $f(x)$ là $\xi\in F$ sao cho $f(\xi)=0$.

???+ note "Định lý"
    Với đa thức $f(x)\in F[x]$ và $\xi\in F$, $\xi$ là nghiệm khi và chỉ khi $(x-\xi)$ là nhân tử.

??? note "Chứng minh"
    Chia $f(x)$ cho $x-\xi$ được $f(x)=q(x)(x-\xi)+r(x)$ với $\deg r<1$, tức $r$ là hằng $c$. Thay $x=\xi$ được $0=f(\xi)=c$, nên $f(x)=q(x)(x-\xi)$.

Nghiệm bội:

???+ abstract "Nghiệm bội"
    Nếu $f(x)$ có nhân tử $(x-\xi)^k$ nhưng không chia hết bởi $(x-\xi)^{k+1}$, thì $\xi$ là **nghiệm bội $k$**. Nếu $k>1$ là **nghiệm bội**, nếu $k=1$ là **nghiệm đơn**.

???+ note "Định lý"
    Nếu $f(x)$ có nghiệm (kể cả lặp) $\xi_1,\cdots,\xi_k$, thì $(x-\xi_1)\cdots(x-\xi_k)$ là nhân tử. Do đó đa thức bậc $n$ có tối đa $n$ nghiệm (tính bội).

??? note "Chứng minh"
    Do $F[x]$ là UFD.

Dù $F[x]$ là UFD, không có cách tổng quát để quyết định khả quy. Bậc nhỏ dễ hơn. Tất cả đa thức bậc một là bất khả quy. Một số trường có mọi đa thức bất khả quy đều bậc một, gọi là [trường đóng đại số](./field-theory.md#代数闭域) như $\mathbf C$. Trên $\mathbf R$ có đa thức bất khả quy bậc 2; trên $\mathbf Q$ cấu trúc phức tạp hơn. Trang [lý thuyết trường](./field-theory.md) bàn thêm về đa thức trên $\mathbf Q$ và trường hữu hạn.

Các kết luận trên chỉ cho đa thức trên trường. Với miền nguyên tổng quát, ta có thể chuyển sang trường phân thức.

Xét UFD $R$ và trường phân thức $F$. Với $f(x)\in R[x]$, phân tích trong $F[x]$ rồi suy ngược về $R[x]$. Gauss đảm bảo điều này.

???+ note "Bổ đề Gauss"
    Với UFD $R$ và trường phân thức $F$, nếu $f(x)\in R[x]$ và $f(x)=A(x)B(x)$ trong $F[x]$, thì tồn tại $s,t\in F$ sao cho $a(x)=sA(x)\in R[x]$, $b(x)=tB(x)\in R[x]$ và $f(x)=a(x)b(x)$. Do đó nếu $f(x)$ bất khả quy trong $R[x]$ thì cũng bất khả quy trong $F[x]$.

??? note "Chứng minh"
    Nếu $f(x)=A(x)B(x)$ trong $F[x]$, lấy $r_a,r_b$ là bội chung nhỏ nhất của mẫu số trong $A,B$, thì $\tilde a=r_aA$, $\tilde b=r_bB$ thuộc $R[x]$ và $rf=\tilde a\tilde b$ với $r=r_ar_b$. Nếu $r$ là đơn vị thì xong. Nếu $r$ có thừa số bất khả quy $p$, thì $(p)$ là ideal nguyên tố. Lấy modulo $p$, được $\overline a\overline b=0$ trong $(R/(p))[x]$, suy ra một trong $\overline a,\overline b$ bằng 0, nên $\tilde a$ (hoặc $\tilde b$) có mọi hệ số chia hết cho $p$. Vậy có thể khử $p$. Lặp hữu hạn lần đến khi $r$ là đơn vị.

???+ note "Hệ quả"
    Với UFD $R$ và trường phân thức $F$, nếu $f(x)\in R[x]$ có các hệ số nguyên tố cùng nhau (gcd là đơn vị), thì $f(x)$ bất khả quy trong $R[x]$ khi và chỉ khi bất khả quy trong $F[x]$.

Vậy đa thức hệ số nguyên bất khả quy trong $\mathbf Z[x]$ cũng bất khả quy trong $\mathbf Q[x]$. Tiêu chuẩn Eisenstein là phương pháp hữu hiệu.

???+ note "Tiêu chuẩn Eisenstein"
    Với đa thức $f(x)=a_0+a_1x+\cdots+a_{n-1}x^{n-1}+a_nx^n$, nếu tồn tại số nguyên tố $p$ sao cho $p\mid a_i$ với $i=0,\cdots,n-1$, $p\nmid a_n$ và $p^2\nmid a_0$, thì $f$ bất khả quy trên $\mathbf Q$. Nếu $\gcd(a_0,\cdots,a_n)=1$, thì $f$ bất khả quy trong $\mathbf Z[x]$.

??? note "Chứng minh"
    Theo Gauss, nếu $f$ khả quy trong $\mathbf Q[x]$ thì cũng khả quy trong $\mathbf Z[x]$: $f=b(x)c(x)$. Lấy modulo $p$ được $\overline f=\overline b\overline c$ trong $\mathbf F_p[x]$. Theo giả thiết, $\overline f=x^n$, nên $\overline b=x^m$, $\overline c=x^{n-m}$ với $0<m<n$. Do đó $b_0,c_0$ đều chia hết cho $p$, nên $a_0=b_0c_0$ chia hết cho $p^2$, mâu thuẫn.

??? example "Ví dụ"
    1.  $x^3-2$ bất khả quy trong $\mathbf Q[x]$ theo Eisenstein với $p=2$.
    2.  $x^4+1$ bất khả quy trong $\mathbf Q[x]$. Nếu không, $(x+1)^4+1$ cũng khả quy, nhưng theo Eisenstein với $p=2$ thì không.

Với UFD $R$, $F[x]$ là UFD, và Gauss cho ta tương ứng phân tích, nên $R[x]$ là UFD.

???+ note "Định lý"
    $R[x]$ là UFD khi và chỉ khi $R$ là UFD.

Vì vậy $\mathbf Z[x]$ là UFD nhưng không PID, ví dụ ideal $(2,x)$ không principal.

Có nhiều cách mở rộng $R[x]$, như trường phân thức $R(x)$ gồm các phân thức $\dfrac{f(x)}{g(x)}$, gọi là **trường phân thức hữu tỉ** (field of rational fractions).

### Vành đa thức nhiều biến

Có thể mở rộng sang nhiều biến. Với vành giao hoán có đơn vị $R$, định nghĩa $R[x]$, rồi $R[x][y]$ (tức $R[x,y]$), và đệ quy tới $R[x_1,\cdots,x_k]$. Nếu $R$ là miền nguyên thì mọi vành đa thức nhiều biến cũng là miền nguyên; tương tự cho UFD.

### Vành chuỗi lũy thừa hình thức

Xét chuỗi có vô hạn số hạng:

$$
\sum_{k=0}^\infty a_kx^k=a_0+a_1x+a_2x^2+\cdots.
$$

Với phép cộng và nhân như đa thức, ta có vành chuỗi lũy thừa hình thức, ký hiệu $R[[x]]$. Không xét hội tụ; đây chỉ là dãy hệ số.

Trong $R[[x]]$, phần tử khả nghịch không chỉ là hằng: ví dụ

$$
(1-x)^{-1}=\sum_{k=0}^\infty x^k.
$$

Nói chung, nếu hệ số tự do $a_0$ là đơn vị, thì chuỗi khả nghịch. Ta có thể tìm nghịch đảo bằng cách giải phương trình hệ số của tích bằng 1, chỉ cần nghịch đảo của $a_0$.

Trong $R[[x]]$ có thể định nghĩa nhiều phép toán như nghịch đảo, chia, nghịch đảo hợp, đạo hàm hình thức, hàm sơ cấp, v.v. xem [giới thiệu đa thức](../poly/intro.md).

### Vành chuỗi Laurent hình thức

Cho phép bậc âm:

$$
\sum_{k=N}^\infty a_kx^k,
$$

với $N\in\mathbf Z$. Các chuỗi Laurent có hữu hạn số hạng bậc âm. Mở rộng phép cộng, nhân, ta có vành $R((x))$. Nếu $F$ là trường thì $F((x))$ cũng là trường.

Vành này dùng trong [phản diễn Lagrange](../poly/lagrange-inversion.md).

## Định lý phần dư Trung Quốc

Đọc thêm: [CRT](../number-theory/crt.md)

Trong số học, CRT giải hệ phương trình đồng dư. Trên vành giao hoán có đơn vị, cũng có CRT. Mỗi phương trình đồng dư xác định ảnh trong một vành thương; CRT nói khi nào các ảnh này xác định phần tử gốc.

Cụ thể, với $R$ và ideal $I_1,\cdots,I_n$, xét đồng cấu $\varphi:R\rightarrow R/I_1\times\cdots\times R/I_n$ gửi $r$ đến $(r+I_1,\cdots,r+I_n)$.

???+ abstract "Tích trực tiếp"
    Với vành $R_1,R_2$, trên $R_1\times R_2$ định nghĩa nhân theo từng thành phần, ta được vành, gọi là **tích trực tiếp** (direct product), ký hiệu $R_1\times R_2$.

Hạt nhân $\ker\varphi=I_1\cap\cdots\cap I_n$. CRT hỏi khi nào $\varphi$ là toàn ánh.

Trong số học, điều kiện là các modulus đôi một nguyên tố cùng nhau. Trên vành:

???+ abstract "Nguyên tố cùng nhau"
    Với ideal $I,J$, nếu $I+J=R$, thì $I,J$ **nguyên tố cùng nhau** (comaximal).

Với ideal chính $(a),(b)$, điều này tương đương tồn tại $x,y$ sao cho $ax+by=1$, tương tự Bézout. Khi đó:

???+ note "Định lý CRT"
    Với vành giao hoán có đơn vị khác 0 $R$ và các ideal $I_1,\cdots,I_n$ đôi một comaximal, đồng cấu $\varphi$ là toàn ánh, và
    
    $$
    \ker\varphi=I_1\cap\cdots\cap I_n=I_1\cdots I_n,
    $$
    
    do đó
    
    $$
    R/(I_1\cdots I_n)\cong R/I_1\times\cdots\times R/I_n.
    $$

??? note "Chứng minh"
    Cần chứng minh $\varphi$ toàn ánh và $I_1\cap\cdots\cap I_n=I_1\cdots I_n$. Xét $n=2$. Vì $I_1+I_2=R$, tồn tại $a_1\in I_1$, $a_2\in I_2$ sao cho $a_1+a_2=1$. Khi đó $\varphi(a_1)=(I_1,1+I_2)$ và $\varphi(a_2)=(1+I_1,I_2)$. Bất kỳ $(r_1+I_1,r_2+I_2)$ đều có nguyên ảnh $r_1a_2+r_2a_1$, nên $\varphi$ toàn ánh.
    
    Với giao, luôn có $I_1I_2\subseteq I_1\cap I_2$. Ngược lại, với $r\in I_1\cap I_2$, $r=r(a_1+a_2)=ra_1+ra_2\in I_1I_2$, nên $I_1\cap I_2\subseteq I_1I_2$.
    
    Với $n>2$, dùng quy nạp. Cần chứng minh $I_1$ comaximal với $I_2\cdots I_n$. Do $I_1$ comaximal với mỗi $I_i$, tồn tại $a_i\in I_1$, $b_i\in I_i$ sao cho $1=a_i+b_i$. Khi đó $1=(a_2+b_2)\cdots(a_n+b_n)\in I_1+(I_2\cdots I_n)$, nên $I_1$ comaximal với tích. QED.

### Ứng dụng: Nội suy Lagrange

Đọc thêm: [Nội suy Lagrange](../numerical/interp.md#lagrange-插值法)、[Nội suy nhanh](../poly/multipoint-eval-interpolation.md#多项式的快速插值)

Bài toán nội suy: cho các điểm $\{(x_i,y_i)\}_{i=1}^n$, tìm đa thức $f(x)$ trên trường $F$ sao cho $f(x_i)=y_i$. Giả sử $x_i$ khác nhau. Công thức Lagrange cho nghiệm tổng quát.

Điều kiện $f(x_i)=y_i$ tương đương $x_i$ là nghiệm của $f(x)-y_i$, tức $(x-x_i)\mid(f(x)-y_i)$, hay $f(x)\equiv y_i\pmod{x-x_i}$. Nên nội suy tương đương hệ đồng dư:

$$
\begin{cases}
f(x)\equiv y_1&\pmod{x-x_1},\\
f(x)\equiv y_2&\pmod{x-x_2},\\
\cdots\\
f(x)\equiv y_n&\pmod{x-x_n}.
\end{cases}
$$

Các đa thức $(x-x_i)$ đôi một nguyên tố cùng nhau. Theo CRT, nghiệm có dạng

$$
f(x)=\sum_{i=1}^ny_iM_i(x),
$$

với $M_i(x)=m_i(x)\prod_{j\neq i}(x-x_j)$ và $M_i(x)\equiv 1\pmod{x-x_i}$, tương đương $M_i(x_i)=1$, tức

$$
m_i(x_i)\prod_{j\neq i}(x_i-x_j) = 1.
$$

Chọn $m_i(x)$ là hằng:

$$
m_i(x) = \frac{1}{\prod_{j\neq i}(x_i-x_j)}.
$$

Suy ra công thức Lagrange:

$$
f(x)=\sum_{i=1}^ny_i\frac{\prod_{j\neq i}(x-x_j)}{\prod_{j\neq i}(x_i-x_j)}.
$$

Tương tự có thể suy ra [nội suy Hermite](https://en.wikipedia.org/wiki/Hermite_interpolation).

### Ứng dụng: Nhóm nhân của lớp đồng dư

Đọc thêm: [căn nguyên](../number-theory/primitive-root.md)、[định lý nhóm Abel hữu hạn](./group-theory.md#分类定理)

Xét nhóm nhân modulo $n$: $(\mathbf Z/n\mathbf Z)^\times$, tức nhóm đơn vị của $\mathbf Z/n\mathbf Z$. Bậc là $\varphi(n)$, vì phần tử khả nghịch iff nguyên tố cùng nhau với $n$. Đây là nhóm Abel.

Theo phân tích nguyên tố:

$$
n=p_1^{\alpha_1}\cdots p_s^{\alpha_s}.
$$

Với ideal trong $\mathbf Z$, comaximal tương đương gcd của các phần tử sinh là 1, nên CRT cho

$$
\mathbf Z/n\mathbf Z\cong\mathbf Z/p_1^{\alpha_1}\mathbf Z\times\cdots\times\mathbf Z/p_s^{\alpha_s}\mathbf Z.
$$

Suy ra

$$
(\mathbf Z/n\mathbf Z)^\times\cong(\mathbf Z/p_1^{\alpha_1}\mathbf Z)^\times\times\cdots\times(\mathbf Z/p_s^{\alpha_s}\mathbf Z)^\times.
$$

Điều này chứng minh $\varphi$ là hàm nhân.

Vì vậy chỉ cần xét $p^k$:

-   Với $p=2$: $(\mathbf Z/2\mathbf Z)^\times\cong C_1$, $(\mathbf Z/4\mathbf Z)^\times\cong C_2$. Với $k\ge3$, $(\mathbf Z/2^k\mathbf Z)^\times\cong C_2\times C_{2^{k-2}}$.
    
    ??? note "Chứng minh"
        Dùng nhị thức:
        
        $$
        \begin{aligned}
        5^{2^{k-2}}=(1+2^2)^{2^{k-2}}&\equiv 1\pmod {2^k},\\
        5^{2^{k-3}}=(1+2^2)^{2^{k-3}}&\equiv 1+2^{k-1}\pmod {2^k}.
        \end{aligned}
        $$
        
        Nên $5$ có bậc $2^{k-2}$. Đồng thời $-1$ và $5^{2^{k-3}}$ là hai phần tử bậc 2 khác nhau, nên $-1\notin\langle 5\rangle$. Do đó $\langle-1\rangle\cap\langle 5\rangle$ tầm thường, suy ra
        
        $$
        (\mathbf Z/2^k\mathbf Z)^\times\cong\langle-1\rangle\times\langle 5\rangle\cong C_2\times C_{2^{k-2}}.
        $$
-   Với $p$ lẻ: $(\mathbf Z/p^k\mathbf Z)^\times$ là nhóm cyclic $C_{\varphi(p^k)}$.
    
    ??? note "Chứng minh"
        Dùng phân tích nhóm Abel hữu hạn, đủ chứng minh mỗi Sylow $q$-nhóm là cyclic. Với Sylow $p$-nhóm:
        
        $$
        \begin{aligned}
        (1+p)^{p^{k-1}} &\equiv 1\pmod{p^k},\\
        (1+p)^{p^{k-2}} &\equiv 1+p^{k-1}\pmod{p^k}.
        \end{aligned}
        $$
        
        nên $(1+p)$ có bậc $p^{k-1}$. Với Sylow $q$ khác $p$, xét đồng cấu $\varphi:(\mathbf Z/p^k\mathbf Z)^\times\rightarrow(\mathbf Z/p\mathbf Z)^\times$, ảnh của Sylow $q$-nhóm đẳng cấu với một Sylow $q$-nhóm của $(\mathbf Z/p\mathbf Z)^\times$. Do đó chỉ cần chứng minh $(\mathbf Z/p\mathbf Z)^\times$ là cyclic. Viết
        
        $$
        (\mathbf Z/p\mathbf Z)^\times\cong C_{n_1}\times\cdots\times C_{n_r},\ n_1\mid\cdots\mid n_r.
        $$
        
        Nếu $r>1$ thì có hơn $n_1$ nghiệm của $x^{n_1}=1$, mâu thuẫn vì $\mathbf Z/p\mathbf Z$ là trường nên đa thức bậc $n_1$ có nhiều nhất $n_1$ nghiệm. Vậy $r=1$, tức $(\mathbf Z/p\mathbf Z)^\times\cong C_{p-1}$.

Do đó điều kiện nhóm nhân modulo $n$ là cyclic chỉ khi

$$
1,2,4,p^k,2p^k
$$

với $p$ lẻ; nếu không thì nhóm chứa $C_2\times C_2$, không cyclic. Khi nhóm cyclic, phần tử sinh gọi là **căn nguyên** (primitive root). Định lý trên chính là điều kiện tồn tại căn nguyên.

Phân tích cấu trúc nhóm còn cho biết bậc các phần tử. Phần tử thỏa $x^k=1$ gọi là **căn đơn vị bậc $k$ modulo $n$**, phần tử bậc đúng $k$ gọi là **căn đơn vị nguyên thủy bậc $k$ modulo $n$**. Từ cấu trúc nhóm có thể tính số lượng. LCM bậc của mọi phần tử là [hàm Carmichael](../number-theory/primitive-root.md#carmichael-函数).

## Tài liệu tham khảo và chú thích

-   Dummitt, D.S. and Foote, R.M. (2004) Abstract Algebra. 3rd Edition, John Wiley & Sons, Inc.
-   [Quadratic integer - Wikipedia](https://en.wikipedia.org/wiki/Quadratic_integer)
-   [Formal power series - Wikipedia](https://en.wikipedia.org/wiki/Formal_power_series)
-   [Multiplicative group of integers modulo $n$- Wikipedia](https://en.wikipedia.org/wiki/Multiplicative_group_of_integers_modulo_n)

[^ideal-history]: <https://en.wikipedia.org/wiki/Ideal_(ring_theory)#History>

[^simple-ring]: Giống trường hợp nhóm, vành như vậy gọi là **vành đơn** (simple ring). Vành giao hoán đơn chỉ có thể là trường; vành không giao hoán đơn phức tạp hơn.

[^gcd-domain]: Miền nguyên mà mọi cặp có gcd gọi là [GCD domain](https://en.wikipedia.org/wiki/GCD_domain).

[^ring-theory-history]: Lịch sử ngắn của lý thuyết vành có thể xem [tại đây](https://mathshistory.st-andrews.ac.uk/HistTopics/Ring_theory/).
