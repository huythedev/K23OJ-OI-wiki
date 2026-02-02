Bài viết này giới thiệu phần mở đầu của số học.

## Tính chia hết

???+ note "Định nghĩa"
    Cho $a,b\in\mathbf{Z}$, $a\ne 0$. Nếu $\exists q\in\mathbf{Z}$ sao cho $b=aq$, thì nói rằng $b$ **chia hết** cho $a$, ký hiệu $a\mid b$; nếu không chia hết thì ký hiệu $a\nmid b$.

Tính chất của chia hết:

-   $a\mid b\iff-a\mid b\iff a\mid-b\iff|a|\mid|b|$
-   $a\mid b\land b\mid c\implies a\mid c$
-   $a\mid b\land a\mid c\iff\forall x,y\in\mathbf{Z}, a\mid(xb+yc)$
-   $a\mid b\land b\mid a\implies b=\pm a$
-   Với $m\ne0$ thì $a\mid b\iff ma\mid mb$.
-   Với $b\ne0$ thì $a\mid b\implies|a|\le|b|$.
-   Với $a\ne0,b=qa+c$ thì $a\mid b\iff a\mid c$.

### Ước số

???+ note "Định nghĩa"
    Nếu $a\mid b$ thì $b$ là **bội số** của $a$, và $a$ là **ước số** của $b$.

$0$ là bội số của mọi số nguyên khác $0$. Với số nguyên $b\ne0$, $b$ chỉ có hữu hạn ước số.

Ước tầm thường (nhân tử tầm thường): với số nguyên $b\ne0$, $\pm1$ và $\pm b$ là các ước tầm thường của $b$. Khi $b=\pm1$ thì $b$ chỉ có hai ước tầm thường.

Với số nguyên $b\ne 0$, các ước còn lại gọi là ước thực (nhân tử thực, ước không tầm thường, nhân tử không tầm thường).

Tính chất của ước:

-   Với số nguyên $b\ne0$. Khi $d$ chạy qua toàn bộ ước của $b$, thì $\dfrac{b}{d}$ cũng chạy qua toàn bộ ước của $b$.
-   Với số nguyên $b\gt 0$. Khi $d$ chạy qua toàn bộ ước dương của $b$, thì $\dfrac{b}{d}$ cũng chạy qua toàn bộ ước dương của $b$.

Trong bài toán cụ thể, **nếu không có chỉ định đặc biệt thì “ước” luôn hiểu là ước dương.**

## Phép chia có dư

???+ note "Số dư"
    Cho $a,b$ là hai số nguyên, $a\ne0$. Cho $d$ là một số nguyên bất kỳ. Khi đó luôn tồn tại duy nhất một cặp số nguyên $q$ và $r$ sao cho $b=qa+r, d\le r<|a|+d$.

Bất kể $d$ nhận giá trị nào, $r$ đều được gọi là số dư. $a\mid b$ tương đương với $a\mid r$.

Thông thường, $d$ lấy $0$, khi đó đẳng thức $b=qa+r,0\le r<|a|$ gọi là phép chia có dư. Số dư $r$ khi đó gọi là số dư không âm nhỏ nhất.

Số dư còn có hai cách chọn thông dụng:

-   Số dư tuyệt đối nhỏ nhất: $d$ lấy đối của nửa giá trị tuyệt đối của $a$, tức $b=qa+r,-\dfrac{|a|}{2}\le r<|a|-\dfrac{|a|}{2}$.
-   Số dư dương nhỏ nhất: $d$ lấy $1$, tức $b=qa+r,1\le r<|a|+1$.

Trong phép chia có dư, số dư luôn là số dư không âm nhỏ nhất. **Nếu không nói rõ, “số dư” luôn hiểu là số dư không âm nhỏ nhất.**

Tính chất của số dư:

-   Một số nguyên khi chia cho số nguyên dương $a$ thì số dư chắc chắn là và chỉ là một trong $a$ số $0$ đến $(a-1)$.
-   $a$ số nguyên liên tiếp khi chia cho số nguyên dương $a$ thì đúng bằng sẽ nhận đủ $a$ số dư trên. Đặc biệt, có và chỉ có một số chia hết cho $a$.

## Ước chung lớn nhất và bội chung nhỏ nhất

Về định nghĩa ước chung, bội chung, ước chung lớn nhất và bội chung nhỏ nhất, xem [Ước chung lớn nhất](./gcd.md).

???+ warning "Warning"
    Một số tác giả cho rằng $\gcd(0,0)$ là không xác định, số khác thường lấy bằng $0$. Cài đặt trong C++ STL chọn cách sau, tức $\gcd(0,0)=0$[^gcdcpp].

Ước chung lớn nhất có các tính chất:

-   $(a_1,\dots,a_n)=(|a_1|,\dots,|a_n|)$;
-   $(a,b)=(b,a)$;
-   Nếu $a\ne 0$ thì $(a,0)=(a,a)=|a|$;
-   $(bq+r,b)=(r,b)$;
-   $(a_1,\dots,a_n)=((a_1,a_2),a_3,\dots,a_n)$. Suy ra $\forall 1<k<n-1,~(a_1,\dots,a_n)=((a_1,\dots,a_k),(a_{k+1},\dots,a_n))$;
-   Với các số nguyên không đồng thời bằng $0$ là $a_1,\dots,a_n$ và số nguyên khác $0$ $m$, $(ma_1,\dots,ma_n)=|m|(a_1,\dots,a_n)$;
-   Với các số nguyên không đồng thời bằng $0$ $a_1,\dots,a_n$, nếu $(a_1,\dots,a_n)=d$ thì $(a_1/d,\dots,a_n/d)=1$;
-   $(a^n,b^n)=(a,b)^n$.

Ước chung lớn nhất còn có các tính chất liên quan đến nguyên tố cùng nhau:

-   Nếu $b|ac$ và $(a,b)=1$ thì $b\mid c$;
-   Nếu $b|c$ và $a|c$ và $(a,b)=1$ thì $ab\mid c$;
-   Nếu $(a,b)=1$ thì $(a,bc)=(a,c)$;
-   Nếu $(a_i,b_j)=1,~\forall 1\leq i\leq n,1\leq j\leq m$ thì $\left(\prod_i a_i,\prod_j b_j\right)=1$. Đặc biệt, nếu $(a,b)=1$ thì $(a^n,b^m)=1$;
-   Với các số nguyên $a_1,\dots,a_n$, nếu $\exists v\in \mathbf{Z},~\prod_i a_i=v^m$, và $(a_i,a_j)=1,~\forall i\ne j$ thì $\forall 1\leq i\leq n,~\sqrt[m]{a_i}\in\mathbf{Z}$.

Bội chung nhỏ nhất có các tính chất:

-   $[a_1,\dots,a_n]=[|a_1|,\dots,|a_n|]$;
-   $[a,b]=[b,a]$;
-   Nếu $a\ne 0$ thì $[a,1]=[a,a]=|a|$;
-   Nếu $a\mid b$ thì $[a,b]=|b|$;
-   $[a_1,\dots,a_n]=[[a_1,a_2],a_3,\dots,a_n]$. Suy ra $\forall 1<k<n-1,~[a_1,\dots,a_n]=[[a_1,\dots,a_k],[a_{k+1},\dots,a_n]]$;
-   Nếu $a_i\mid m,~\forall 1\leq i\leq n$ thì $[a_1,\dots,a_n]\mid m$;
-   $[ma_1,\dots,ma_n]=|m|[a_1,\dots,a_n]$;
-   $[a,b,c][ab,bc,ca]=[a,b][b,c][c,a]$;
-   $[a^n,b^n]=[a,b]^n$.

ƯCLN và BCNN có thể kết hợp cho ra nhiều đẳng thức thú vị, ví dụ:

-   $(a,b)[a,b]=|ab|$;
-   $(ab,bc,ca)[a,b,c]=|abc|$;
-   $\dfrac{(a,b,c)^2}{(a,b)(b,c)(a,c)}=\dfrac{[a,b,c]^2}{[a,b][b,c][a,c]}$.

Các tính chất này đều có thể chứng minh từ định nghĩa hoặc từ [định lý phân tích duy nhất](#算术基本定理); cách dùng định lý phân tích duy nhất thường dễ hiểu hơn.

### Nguyên tố cùng nhau

???+ note "Định nghĩa"
    Nếu $(a_1,a_2)=1$ thì gọi $a_1$ và $a_2$ **nguyên tố cùng nhau** (**tối giản**).
    
    Nếu $(a_1,\ldots,a_k)=1$ thì gọi $a_1,\ldots,a_k$ **nguyên tố cùng nhau** (**tối giản**).

Nhiều số nguyên cùng nguyên tố không nhất thiết từng đôi nguyên tố cùng nhau. Ví dụ $6$, $10$ và $15$ nguyên tố cùng nhau, nhưng mọi cặp đều không nguyên tố cùng nhau.

Tính chất của nguyên tố cùng nhau và lý thuyết ƯCLN: định lý Bézout. Xem [Định lý Bézout](./bezouts.md).

### Thuật toán Euclid

Thuật toán Euclid còn gọi là thuật toán chia liên tiếp. Xem [Ước chung lớn nhất](./gcd.md).

## Số nguyên tố và hợp số

Về thuật toán liên quan số nguyên tố xem [Số nguyên tố](./prime.md).

???+ note "Định nghĩa"
    Cho số nguyên $p\ne0,\pm1$. Nếu $p$ ngoài các ước tầm thường không có ước nào khác, thì $p$ là **số nguyên tố** (**số bất khả quy**).
    
    Nếu số nguyên $a\ne0,\pm 1$ và $a$ không phải số nguyên tố thì $a$ là **hợp số**.

$p$ và $-p$ luôn cùng là nguyên tố hoặc cùng là hợp số. **Nếu không nói rõ, “số nguyên tố” luôn hiểu là số nguyên tố dương.**

Một ước nguyên tố của số nguyên được gọi là ước nguyên tố (thừa số nguyên tố) của số đó.

Một số tính chất đơn giản:

-   Số nguyên $a>1$ là hợp số khi và chỉ khi $a$ viết được thành tích $de$ với $1<d,e<a$.
-   Nếu số nguyên tố $p$ có ước $d>1$ thì $d=p$.
-   Mọi số nguyên $a>1$ đều viết được thành tích các số nguyên tố.
-   Với hợp số $a$, tồn tại số nguyên tố $p\le\sqrt{a}$ sao cho $p\mid a$.
-   Có vô hạn số nguyên tố.
-   Mọi số nguyên tố lớn hơn $3$ đều có dạng $6n\pm 1$[^ref1].

## Định lý cơ bản của số học

???+ note "Bổ đề cơ bản"
    Cho $p$ là số nguyên tố, $p\mid a_1a_2$ thì $p\mid a_1$ hoặc $p\mid a_2$.

Mệnh đề đảo của bổ đề trên nếu sửa nhẹ cũng cho một định nghĩa khác của số nguyên tố.

???+ note "Định nghĩa khác của số nguyên tố"
    Với số nguyên $p\ne 0,\pm 1$, nếu với mọi $a_1,a_2$ thỏa $p\mid a_1a_2$ đều suy ra $p\mid a_1$ hoặc $p\mid a_2$, thì $p$ là số nguyên tố.

??? tip "Tip"
    Động cơ của định nghĩa này có thể xem ở [ideal nguyên tố](../algebra/ring-theory.md#素理想).

???+ note "Định lý cơ bản của số học (phân tích duy nhất)"
    Cho số nguyên dương $a$, luôn có biểu diễn:
    
    $$
    a=p_1p_2\cdots p_s
    $$
    
    trong đó $p_j(1\le j\le s)$ là các số nguyên tố. Hơn nữa, theo nghĩa không xét thứ tự, biểu diễn là duy nhất.

???+ note "Dạng phân tích thừa số nguyên tố chuẩn"
    Gộp các thừa số nguyên tố trùng nhau, được:
    
    $$
    a={p_1}^{\alpha_1}{p_2}^{\alpha_2}\cdots{p_s}^{\alpha_s},p_1<p_2<\cdots<p_s
    $$
    
    gọi là dạng phân tích thừa số nguyên tố chuẩn của số nguyên dương $a$.

Định lý cơ bản và bổ đề cơ bản là tương đương nhau.

## Đồng dư

???+ note "Định nghĩa"
    Cho số nguyên $m\ne0$. Nếu $m\mid(a-b)$ thì gọi $m$ là **mô-đun** (**mod**), $a$ đồng dư với $b$ theo mô-đun $m$, $b$ là **số dư** của $a$ theo mô-đun $m$. Ký hiệu $a\equiv b\pmod m$.
    
    Nếu không, $a$ không đồng dư với $b$ theo mô-đun $m$, $b$ không phải số dư của $a$ theo mô-đun $m$. Ký hiệu $a\not\equiv b\pmod m$.
    
    Các đẳng thức như vậy gọi là đồng dư theo mô-đun $m$, gọi tắt là **đồng dư**.

Theo tính chất chia hết, đồng dư trên cũng tương đương với $a\equiv b\pmod{(-m)}$.

Về sau nếu không nói rõ, mô-đun luôn là **số nguyên dương**.

Trong biểu thức, $b$ là số dư của $a$ theo mô-đun $m$, khái niệm này trùng với số dư trong phép chia. Bằng cách giới hạn phạm vi của $b$, ta có số dư không âm nhỏ nhất, số dư tuyệt đối nhỏ nhất, số dư dương nhỏ nhất.

Tính chất của đồng dư:

-   Đồng dư là [quan hệ tương đương](../order-theory.md#二元关系), tức có:
    -   Phản xạ: $a\equiv a\pmod m$.
    -   Đối xứng: nếu $a\equiv b\pmod m$ thì $b\equiv a\pmod m$.
    -   Bắc cầu: nếu $a\equiv b\pmod m,b\equiv c\pmod m$ thì $a\equiv c\pmod m$.
-   Phép toán tuyến tính: nếu $a,b,c,d\in\mathbf{Z},m\in\mathbf{N}^*,a\equiv b\pmod m,c\equiv d\pmod m$ thì:
    -   $a\pm c\equiv b\pm d\pmod m$.
    -   $a\times c\equiv b\times d\pmod m$.
-   Cho $f(x)=\sum_{i=0}^n a_ix^i$ và $g(x)=\sum_{i=0}^n b_ix^i$ là hai đa thức hệ số nguyên, $m\in\mathbf{N}^*$, và $a_i\equiv b_i\pmod m,~0\leq i\leq n$, thì với mọi số nguyên $x$ đều có $f(x)\equiv g(x)\pmod m$. Suy ra nếu $s\equiv t\pmod m$ thì $f(s)\equiv g(t)\pmod m$.
-   Nếu $a,b\in\mathbf{Z},k,m\in\mathbf{N}^*,a\equiv b\pmod m$ thì $ak\equiv bk\pmod{mk}$.
-   Nếu $a,b\in\mathbf{Z},d,m\in\mathbf{N}^*,d\mid a,d\mid b,d\mid m$ và $a\equiv b\pmod m$ thì $\dfrac{a}{d}\equiv\dfrac{b}{d}\left(\bmod\;{\dfrac{m}{d}}\right)$.
-   Nếu $a,b\in\mathbf{Z},d,m\in\mathbf{N}^*,d\mid m$ và $a\equiv b\pmod m$ thì $a\equiv b\pmod d$.
-   Nếu $a,b\in\mathbf{Z},d,m\in\mathbf{N}^*$ và $a\equiv b\pmod m$ thì $(a,m)=(b,m)$. Nếu $d$ chia $m$ và chia một trong $a,b$ thì $d$ cũng chia số còn lại.

Còn có tính chất về nghịch đảo nhân. Xem [nghịch đảo nhân](./inverse.md).

## Lớp đồng dư và hệ thặng dư

Để thuận tiện, với hai tập $A,B$ và phần tử $r$, dùng ký hiệu:

-   $r+A:=\{r+a:a\in A\}$;
-   $rA:=\{ra:a\in A\}$;
-   $A+B:=\{a+b:a\in A,b\in B\}$;
-   $AB:=\{ab:a\in A,b\in B\}$.

???+ note "Lớp đồng dư"
    Với số nguyên khác $0$ $m$, chia toàn bộ các số nguyên thành $|m|$ tập con đôi một không giao nhau, sao cho mọi hai số trong cùng một tập đều đồng dư theo mô-đun $m$. Mỗi tập như vậy gọi là **lớp đồng dư** hoặc **lớp thặng dư** theo mô-đun $m$. Dùng $r\bmod m$ để chỉ lớp đồng dư chứa số nguyên $r$.
    
    Không khó chứng minh với mọi $m\ne0$, cách chia trên tồn tại và là duy nhất.

Từ định nghĩa lớp đồng dư:

-   $r\bmod m=\{r+km:k\in\mathbf{Z}\}$;
-   $r\bmod m=s\bmod m\iff r\equiv s\pmod m$;
-   Với mọi $r,s\in\mathbf{Z}$, hoặc $r\bmod m=s\bmod m$, hoặc $(r\bmod m)\cap (s\bmod m)=\varnothing$;
-   Nếu $m_1\mid m$ thì với mọi số nguyên $r$ đều có $r+m\mathbf{Z}\subseteq r+m_1\mathbf{Z}$.

Vì đồng dư là quan hệ tương đương nên lớp đồng dư chính là lớp tương đương của quan hệ đồng dư.

Tập hợp các lớp đồng dư theo mô-đun $m$ ký hiệu $\mathbf{Z}_m$, tức

$$
\mathbf{Z}_m:=\{r\bmod m:0\leq r<m\}
$$

Dễ thấy:

-   Với mọi số nguyên $a$, $a+\mathbf{Z}_m=\mathbf{Z}_m$;
-   Với mọi số nguyên $b$ nguyên tố cùng $m$, $b\mathbf{Z}_m=\mathbf{Z}_m$.

Theo định nghĩa [nhóm thương](../algebra/group-theory.md#商群), $\mathbf{Z}_m=\mathbf{Z}/m\mathbf{Z}$, nên đôi khi cũng viết $\mathbf{Z}/m\mathbf{Z}$ để chỉ $\mathbf{Z}_m$.

Theo [nguyên lý Dirichlet (ngăn kéo)](../combinatorics/drawer-principle.md):

-   Chọn $m+1$ số nguyên bất kỳ, luôn có hai số đồng dư theo mô-đun $m$.
-   Tồn tại $m$ số nguyên đôi một không đồng dư theo mô-đun $m$.

Từ đó định nghĩa hệ thặng dư hoàn chỉnh:

???+ note "Hệ thặng dư (hoàn chỉnh)"
    Với $m$ số nguyên $a_1,a_2,\dots,a_m$, nếu với mọi số $x$, tồn tại duy nhất một số $a_i$ sao cho $x$ đồng dư với $a_i$ theo mô-đun $m$, thì $a_1,a_2,\dots,a_m$ là **hệ thặng dư hoàn chỉnh** theo mô-đun $m$ (gọi tắt là **hệ thặng dư**).

Ta cũng có thể định nghĩa các hệ thặng dư theo mô-đun $m$:

-   Hệ thặng dư không âm nhỏ nhất: $0,\dots,m-1$;
-   Hệ thặng dư dương nhỏ nhất: $1,\dots,m$;
-   Hệ thặng dư tuyệt đối nhỏ nhất: $-\lfloor m/2\rfloor,\dots,-\lfloor -m/2\rfloor-1$;
-   Hệ thặng dư không dương lớn nhất: $-m+1,\dots,0$;
-   Hệ thặng dư âm lớn nhất: $-m,\dots,-1$.

Nếu không nói rõ, ta chỉ dùng hệ thặng dư không âm nhỏ nhất.

Ta chú ý mệnh đề sau:

-   Trong cùng một lớp đồng dư theo mô-đun $m$, lấy bất kỳ hai số nguyên $a_1,a_2$ thì $(a_1,m)=(a_2,m)$.

Xét lớp đồng dư $r\bmod m$, nếu $(r,m)=1$ thì mọi phần tử của lớp đều nguyên tố cùng $m$, do đó ta có thể tìm cấu trúc của tập các số nguyên cùng nguyên tố với $m$ theo cách tương tự.

???+ note "Lớp đồng dư tối giản"
    Với lớp đồng dư $r\bmod m$, nếu $(r,m)=1$ thì gọi là **lớp đồng dư tối giản** hoặc **lớp thặng dư tối giản**.
    
    Số lớp đồng dư tối giản theo mô-đun $m$ ký hiệu $\varphi(m)$, gọi là [hàm Euler](./euler-totient.md).

Tập các lớp đồng dư tối giản theo mô-đun $m$ ký hiệu $\mathbf{Z}_m^*$:

$$
\mathbf{Z}_m^*:=\{r\bmod m:0\leq r<m,(r,m)=1\}
$$

???+ warning "Warning"
    Với mọi số nguyên $a$ và số nguyên $b$ nguyên tố cùng $m$, ta có $b\mathbf{Z}_m^*=\mathbf{Z}_m^*$, nhưng $a+\mathbf{Z}_m^*$ không nhất thiết bằng $\mathbf{Z}_m^*$. Điều này khác với $\mathbf{Z}_m$.

Theo [nguyên lý ngăn kéo](../combinatorics/drawer-principle.md):

-   Lấy $\varphi(m)+1$ số nguyên đều nguyên tố cùng $m$, luôn có hai số đồng dư theo mô-đun $m$.
-   Tồn tại $\varphi(m)$ số nguyên nguyên tố cùng $m$ và đôi một không đồng dư theo mô-đun $m$.

Từ đó định nghĩa hệ thặng dư tối giản:

???+ note "Hệ thặng dư tối giản"
    Với $t=\varphi(m)$ số nguyên $a_1,a_2,\dots,a_t$, nếu $(a_i,m)=1,~\forall 1\leq i\leq t$, và với mọi $x$ thỏa $(x,m)=1$, tồn tại duy nhất một $a_i$ sao cho $x$ đồng dư với $a_i$ theo mô-đun $m$, thì $a_1,a_2,\dots,a_t$ là **hệ thặng dư tối giản**, **hệ thặng dư rút gọn** hoặc **hệ thặng dư giản lược** theo mô-đun $m$.

Tương tự, ta có thể định nghĩa hệ thặng dư tối giản không âm nhỏ nhất, v.v.

Nếu không nói rõ, ta chỉ dùng hệ thặng dư tối giản không âm nhỏ nhất.

### Ghép hệ thặng dư

Với số nguyên dương $m$, ta có định lý:

-   Nếu $m=m_1m_2,~1\leq m_1,m_2$, đặt $Z_{m_1},Z_{m_2}$ lần lượt là hệ thặng dư **hoàn chỉnh** theo mô-đun $m_1,m_2$, thì với mọi $a$ nguyên tố cùng $m_1$ đều có:

    $$
    Z_m=aZ_{m_1}+m_1Z_{m_2}.
    $$

    là hệ thặng dư **hoàn chỉnh** theo mô-đun $m$. Suy ra, nếu $m=\prod_{i=1}^k m_i,~1\leq m_1,m_2,\dots,m_k$, đặt $Z_{m_1},\dots,Z_{m_k}$ là các hệ thặng dư **hoàn chỉnh** tương ứng thì:

    $$
    Z_m=\sum_{i=1}^k\left(\prod_{j=1}^{i-1}m_j\right)Z_{m_i}.
    $$

    là hệ thặng dư **hoàn chỉnh** theo mô-đun $m$.

???+ note "Chứng minh"
    Chỉ cần chứng minh với mọi $x,x'\in Z_{m_1}$, $y,y'\in Z_{m_2}$ thỏa $ax+m_1y\equiv ax'+m_1y'\pmod{m_1m_2}$ đều có:
    
    $$
    ax+m_1y=ax'+m_1y'.
    $$
    
    Thật vậy, do $m_1\mid m_1m_2$ nên $ax+m_1y\equiv ax'+m_1y'\pmod{m_1}$, suy ra $ax\equiv ax'\pmod{m_1}$. Từ $(a,m_1)=1$ suy ra $x\equiv x'\pmod{m_1}$, do đó $x=x'$.
    
    Hơn nữa, $m_1y\equiv m_1y'\pmod{m_1m_2}$ nên $y\equiv y'\pmod{m_2}$, tức $y=y'$.
    
    Vì vậy,
    
    $$
    ax+m_1y=ax'+m_1y'.
    $$

-   Nếu $m=m_1m_2,~1\leq m_1,m_2,(m_1,m_2)=1$, đặt $Z_{m_1}^*,Z_{m_2}^*$ lần lượt là hệ thặng dư **tối giản** theo mô-đun $m_1,m_2$, thì:

    $$
    Z_m^*=m_2Z_{m_1}^*+m_1Z_{m_2}^*.
    $$

    là hệ thặng dư **tối giản** theo mô-đun $m$.

???+ tip "Tip"
    Định lý này tương đương với việc chứng minh hàm Euler là [hàm tích](#积性函数).

???+ note "Chứng minh"
    Gọi $Z_{m_1},Z_{m_2}$ là các hệ thặng dư hoàn chỉnh theo mô-đun $m_1,m_2$. Ta đã chứng minh
    
    $$
    Z_m=m_2Z_{m_1}+m_1Z_{m_2}
    $$
    
    là hệ thặng dư hoàn chỉnh theo mô-đun $m$. Đặt $M=\{a\in Z_m:(a,m)=1\}\subseteq Z_m$, hiển nhiên $M$ là hệ thặng dư tối giản theo mô-đun $m$, nên chỉ cần chứng minh $M=Z_m^*$.
    
    Rõ ràng $Z_m^*\subseteq Z_m$.
    
    Lấy $m_2x+m_1y\in M$, với $x\in Z_{m_1}$ và $y\in Z_{m_2}$, ta có $(m_2x+m_1y,m_1m_2)=1$. Từ $(m_1,m_2)=1$ suy ra
    
    $$
    1=(m_2x+m_1y,m_1)=(m_2x,m_1)=(x,m_1),
    $$
    
    $$
    1=(m_2x+m_1y,m_2)=(m_1y,m_2)=(y,m_2).
    $$
    
    Do đó $x\in Z_{m_1}^*$ và $y\in Z_{m_2}^*$, tức $M\subseteq Z_m^*$.
    
    Lấy $m_2x+m_1y\in Z_m^*$, với $x\in Z_{m_1}^*$ và $y\in Z_{m_2}^*$, ta có $(x,m_1)=1$ và $(y,m_2)=1$. Từ $(m_1,m_2)=1$ suy ra
    
    $$
    (m_2x+m_1y,m_1)=(m_2x,m_1)=(x,m_1)=1,
    $$
    
    $$
    (m_2x+m_1y,m_2)=(m_1y,m_2)=(x,m_2)=1,
    $$
    
    Do đó $(m_2x+m_1y,m_1m_2)=1$, tức $Z_m^*\subseteq M$.
    
    Tóm lại,
    
    $$
    Z_m^*=m_2Z_{m_1}^*+m_1Z_{m_2}^*.
    $$
    
    là hệ thặng dư **tối giản** theo mô-đun $m$.

## Hàm số học

Hàm số học (còn gọi là hàm số học) là hàm có miền xác định là các số nguyên dương. Hàm số học cũng có thể xem như một dãy số.

### Hàm tích

???+ note "Định nghĩa"
    Trong số học, nếu hàm $f(n)$ thỏa $f(1)=1$ và $f(xy)=f(x)f(y)$ với mọi $x, y \in\mathbf{N}^*$ nguyên tố cùng nhau, thì $f(n)$ là **hàm tích**.
    
    Nếu $f(n)$ thỏa $f(1)=1$ và $f(xy)=f(x)f(y)$ với mọi $x, y \in\mathbf{N}^*$ thì $f(n)$ là **hàm tích hoàn toàn**.

#### Tính chất

Nếu $f(x)$ và $g(x)$ đều là hàm tích, thì các hàm sau cũng là hàm tích:

$$
\begin{aligned}
h(x)&=f(x^p)\\
h(x)&=f^p(x)\\
h(x)&=f(x)g(x)\\
h(x)&=\sum_{d\mid x}f(d)g\left(\dfrac{x}{d}\right)
\end{aligned}
$$

Với số nguyên dương $x$, phân tích thừa số nguyên tố duy nhất $x=\prod p_i^{k_i}$, trong đó $p_i$ là số nguyên tố.

Nếu $F(x)$ là hàm tích thì $F(x)=\prod F(p_i^{k_i})$.

Nếu $F(x)$ là hàm tích hoàn toàn thì $F(x)=\prod F(p_i^{k_i})=\prod F(p_i)^{k_i}$.

#### Ví dụ

-   Hàm đơn vị: $\varepsilon(n)=[n=1]$. (tích hoàn toàn)
-   Hàm đồng nhất: $\operatorname{id}_k(n)=n^k$, $\operatorname{id}_{1}(n)$ thường viết tắt $\operatorname{id}(n)$. (tích hoàn toàn)
-   Hàm hằng: $1(n)=1$. (tích hoàn toàn)
-   Hàm ước: $\sigma_{k}(n)=\sum_{d\mid n}d^{k}$. $\sigma_{0}(n)$ thường viết tắt $d(n)$ hoặc $\tau(n)$, $\sigma_{1}(n)$ thường viết tắt $\sigma(n)$.
-   Hàm Euler: $\varphi(n)=\sum_{i=1}^n[(i,n)=1]$.
-   Hàm Möbius: $\mu(n)=\begin{cases}1&n=1\\0&\exists d>1,d^{2}\mid n\\(-1)^{\omega(n)}&\text{otherwise}\end{cases}$, trong đó $\omega(n)$ là số lượng thừa số nguyên tố phân biệt của $n$.

### Hàm cộng tính

???+ note "Định nghĩa"
    Trong số học, nếu $f(n)$ thỏa $f(1)=0$ và $f(xy)=f(x)+f(y)$ với mọi $x, y \in\mathbf{N}^*$ nguyên tố cùng nhau, thì $f(n)$ là **hàm cộng tính**.
    
    Nếu $f(n)$ thỏa $f(1)=0$ và $f(xy)=f(x)+f(y)$ với mọi $x, y \in\mathbf{N}^*$ thì $f(n)$ là **hàm cộng tính hoàn toàn**.

???+ warning "Hàm cộng tính"
    “Hàm cộng tính” ở đây là Additive function trong số học, cần phân biệt với Additive map trong đại số.

#### Tính chất

Với số nguyên dương $x$, phân tích thừa số nguyên tố duy nhất $x=\prod p_i^{k_i}$, trong đó $p_i$ là số nguyên tố.

Nếu $F(x)$ là hàm cộng tính thì $F(x)=\sum F(p_i^{k_i})$.

Nếu $F(x)$ là hàm cộng tính hoàn toàn thì $F(x)=\sum F(p_i^{k_i})=\sum F(p_i)\cdot k_i$.

#### Ví dụ

Để tiện, ký hiệu tập các số nguyên tố là $\mathbf P$.

-   Số mũ của $p$ trong phân tích thừa số nguyên tố: $\nu_p(n) = \max\{k\in\mathbf N: p^k\mid n\}$, với $p\in\mathbf P$. (cộng tính hoàn toàn)
-   Tổng số thừa số nguyên tố (kể bội): $\Omega(n)=\sum_{p \in\mathbf P} \nu_p(n)$. (cộng tính hoàn toàn)
-   Số thừa số nguyên tố phân biệt: $\omega(n)=\sum_{p \in\mathbf P} [p \mid n]$.
-   Tổng các thừa số nguyên tố (kể bội): $a_0(n)=\sum_{p \in\mathbf P} \nu_p(n)\cdot p$. (cộng tính hoàn toàn)
-   Tổng các thừa số nguyên tố phân biệt: $a_1(n)=\sum_{p \in\mathbf P} [p \mid n] \cdot p$.

## Hàm lấy phần nguyên

Với số thực $x$, định nghĩa **hàm lấy phần nguyên dưới** (floor) và **hàm lấy phần nguyên trên** (ceiling):

$$
\lfloor x\rfloor = \max\{k\in\mathbf Z:k\le x\},~\lceil x\rceil = \min\{k\in\mathbf Z:k\ge x\}.
$$

Dùng hàm floor, ta có thể tách một số thực thành phần nguyên và phần thập phân: $x = \lfloor x\rfloor + \{x\}$. Trong đó $\{x\}$ là phần thập phân của $x$.

Hàm lấy phần nguyên có các tính chất cơ bản: ($x\in\mathbf R,~n\in\mathbf Z$)

-   $x\in\mathbf Z \iff x = \lfloor x\rfloor = \lceil x\rceil$.
-   $\lceil x\rceil - \lfloor x\rfloor = [x\notin\mathbf Z]$.
-   $x - 1 < \lfloor x\rfloor \le x \le \lceil x\rceil < x + 1$.
-   $\lfloor -x\rfloor = -\lceil x\rceil,~\lceil -x\rceil = -\lfloor x\rfloor$.
-   $\lfloor x + n\rfloor = \lfloor x\rfloor + n,~\lceil x + n\rceil = \lceil x \rceil + n$.
-   $\lfloor x\rfloor$ và $\lceil x\rceil$ đều là các hàm đơn điệu không giảm theo $x$.

Chứng minh các đẳng thức với floor/ceiling thường dùng các dạng tương đương sau: ($x\in\mathbf R,~n\in\mathbf Z$)

-   $\lfloor x\rfloor = n \iff n \le x < n + 1 \iff x - 1 < n \le x$.
-   $\lceil x\rceil = n \iff n - 1 < x \le n \iff x \le n < x + 1$.

Chứng minh bất đẳng thức với floor/ceiling thường dùng các dạng tương đương sau: ($x\in\mathbf R,~n\in\mathbf Z$)

-   $x < n \iff \lfloor x\rfloor < n$.
-   $n < x \iff n < \lceil x\rceil$.
-   $x \le n \iff \lceil x\rceil \le n$.
-   $n \le x \iff n \le \lfloor x\rfloor$.

Tính chất liên quan đến tổng và hiệu: ($x,y\in\mathbf R$)

-   $\lfloor x\rfloor + \lfloor y\rfloor \le \lfloor x + y\rfloor \le \lfloor x\rfloor + \lfloor y\rfloor + 1$, và chỉ có một dấu bằng đúng.
-   $\lceil x\rceil +\lceil y\rceil -1\leq \lceil x+y\rceil \leq \lceil x\rceil +\lceil y\rceil$, và chỉ có một dấu bằng đúng.
-   $\lfloor|x - y|\rfloor \le |\lfloor x\rfloor - \lfloor y\rfloor| \le \lceil|x - y|\rceil$.
-   $\lfloor|x - y|\rfloor \le |\lceil x\rceil - \lceil y\rceil| \le \lceil|x-y|\rceil$.

Tính chất liên quan đến thương: ($x\in\mathbf R,~n\in\mathbf Z,~m\in\mathbf Z_+$)

-   $\left\lceil\dfrac{n}{m}\right\rceil = \left\lfloor\dfrac{n+m-1}{m}\right\rfloor,~\left\lfloor\dfrac{n}{m}\right\rfloor = \left\lceil\dfrac{n-m+1}{m}\right\rceil$.
-   $\left\lfloor\dfrac{x + n}{m} \right\rfloor = \left\lfloor\dfrac{\lfloor x\rfloor + n}{m} \right\rfloor,~\left\lceil\dfrac{x + n}{m} \right\rceil = \left\lceil\dfrac{\lceil x\rceil + n}{m} \right\rceil$.
-   $\left\lfloor\dfrac{\lfloor x/n\rfloor}{m}\right\rfloor = \left\lfloor\dfrac{x}{nm}\right\rfloor,~\left\lceil\dfrac{\lceil x/n\rceil}{m}\right\rceil = \left\lceil\dfrac{x}{nm}\right\rceil$.
-   Với $x > 0$, $\displaystyle\left\lfloor\dfrac{x}{m}\right\rfloor = \sum_{k=1}^{\lfloor x\rfloor}[m\mid k]$.

Trong đó, tính chất thứ hai và thứ ba đều là hệ quả trực tiếp của kết luận sau:

-   Cho $f$ là hàm liên tục, tăng đơn điệu, và nếu $f(x)\in\mathbf Z$ thì $x\in\mathbf Z$, khi đó

    $$
    \lfloor f(x)\rfloor = \lfloor f(\lfloor x\rfloor)\rfloor,~ \lceil f(x)\rceil = \lceil f(\lceil x\rceil)\rceil.
    $$

    ??? note "Chứng minh"
        Do tính đối xứng, chỉ cần chứng minh đẳng thức đầu. Nếu $x$ là số nguyên thì hiển nhiên. Ngược lại, $\lfloor x\rfloor < x$. Từ tính đơn điệu của $f$ và floor, suy ra $\lfloor f(x)\rfloor \ge \lfloor f(\lfloor x\rfloor)\rfloor$. Nếu không bằng nhau, đặt $y = \lfloor f(x)\rfloor$, ta có $\lfloor f(\lfloor x\rfloor)\rfloor < y \le \lfloor f(x)\rfloor$, tương đương $f(\lfloor x\rfloor) < y \le f(x)$. Do $f$ liên tục, tồn tại $\lfloor x\rfloor < x_0 \le x$ sao cho $f(x_0)=y$. Vì $y\in\mathbf Z$ nên $x_0\in\mathbf Z$, mâu thuẫn với định nghĩa của $\lfloor x\rfloor$. Vậy dấu bằng đúng, tức $\lfloor f(x)\rfloor = \lfloor f(\lfloor x\rfloor)\rfloor$.

Cuối cùng là một nhóm công thức tổng có chứa floor/ceiling: ($x\in\mathbf R,~n\in\mathbf Z,~m\in\mathbf Z_+$)

-   $n = \left\lfloor\dfrac{n}{2}\right\rfloor + \left\lceil\dfrac{n}{2}\right\rceil$.
-   $n = \left\lfloor\dfrac{n}{m} \right\rfloor + \left\lfloor\dfrac{n+1}{m} \right\rfloor + \cdots + \left\lfloor\dfrac{n+m-1}{m} \right\rfloor$.
-   $n = \left\lceil\dfrac{n}{m} \right\rceil + \left\lceil\dfrac{n-1}{m} \right\rceil + \cdots + \left\lceil\dfrac{n-m+1}{m} \right\rceil$.
-   $\lfloor mx\rfloor = \lfloor x\rfloor + \left\lfloor x+\dfrac{1}{m}\right\rfloor + \cdots + \left\lfloor x+\dfrac{m-1}{m}\right\rfloor$.
-   $\lceil mx\rceil = \lceil x\rceil + \left\lceil x - \dfrac{1}{m}\right\rceil + \cdots + \left\lceil x - \dfrac{m-1}{m}\right\rceil$.
-   Khi $m\perp n$, $\displaystyle\sum_{k=1}^{m-1}\left\lfloor\dfrac{kn}{m}\right\rfloor=\dfrac{1}{2}(n-1)(m-1)$.
-   Khi $m\perp n$, $\displaystyle\sum_{k=1}^{m-1}\left\lceil\dfrac{kn}{m}\right\rceil=\dfrac{1}{2}(n+1)(m-1)$.

Các công thức tổng kiểu này và tổng quát hơn có thể tham khảo ở trang [Thuật toán kiểu Euclid](./euclidean.md).

Các tính chất và ứng dụng khác của hàm floor/ceiling:

-   Phép lấy mod: $n\bmod m = n - \left\lfloor\dfrac{n}{m}\right\rfloor m$, dùng để [tối ưu phép lấy mod số nguyên](./mod-arithmetic.md#相关算法).
-   Dùng bổ đề Gauss chứng minh [luật tương hỗ bậc hai](./quad-residue.md#二次互反律).
-   [Chia khối số học](./sqrt-decomposition.md), đặc biệt là phần chứng minh tính chất.
-   Tính bậc của thừa số nguyên tố trong giai thừa theo [công thức Legendre](./factorial.md#legendre-公式).
-   [Dãy Beatty](../game-theory/impartial-game.md#wythoff-游戏), định lý Rayleigh và trò chơi Wythoff.

## Tài liệu tham khảo và chú thích

-   潘承洞，潘承彪．初等数论．北京大学出版社．
-   [Floor and ceiling functions - Wikipedia](https://en.wikipedia.org/wiki/Floor_and_ceiling_functions)
-   Graham, Ronald L., Donald E. Knuth, and Oren Patashnik. "Concrete mathematics: a foundation for computer science." (1989).

[^ref1]: [Are all primes (past 2 and 3) of the forms 6n+1 and 6n-1?](https://primes.utm.edu/notes/faq/six.html)

[^gcdcpp]: [std::gcd - cppreference.com](https://en.cppreference.com/w/cpp/numeric/gcd)
