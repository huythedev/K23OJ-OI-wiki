Nếu bạn đã học qua về số phức, hãy bỏ qua trang này．

Học số phức cần một chút nền tảng vector, nếu chưa học hãy xem [trang vector](../math/linear-algebra/vector.md)．

## Số phức

### Dẫn nhập

???+ note "注"
    Phần dẫn nhập sau lấy từ SGK toán THPT A bản bắt buộc 2．

Từ góc nhìn phương trình, “số thực âm có khai căn được không” là hỏi phương trình $x^2+a=0 (a>0)$ có nghiệm hay không, quy về phương trình $x^2+1=0$ có nghiệm hay không．

Nhìn lại quá trình mở rộng tập số, ta thấy mỗi lần mở rộng đều gắn với nhu cầu thực tế．Ví dụ, để giải vấn đề độ dài đường chéo hình vuông, hoặc phương trình $x^2-2=0$ vô nghiệm trong hữu tỷ, người ta mở rộng lên số thực．Sau mở rộng, phép cộng/nhân trong số thực vẫn thống nhất với phép cộng/nhân trong hữu tỷ, và thỏa giao hoán, kết hợp, phân phối．

Theo tư tưởng đó, để giải $x^2+1=0$ vô nghiệm trong số thực, ta giả sử có số mới $\mathrm{i}$ sao cho $x=\mathrm{i}$ là nghiệm, tức $\mathrm{i}^2=-1$．

Suy nghĩ: thêm $\mathrm{i}$ vào số thực, ta muốn $\mathrm{i}$ và số thực vẫn cộng/nhân như số thực, và các luật giao hoán, kết hợp, phân phối vẫn đúng．Vậy hệ số mới gồm những số nào?

Theo giả thiết, với số thực $b$ và $\mathrm{i}$, ký hiệu $b\mathrm{i}$; với số thực $a$ và $b\mathrm{i}$, ký hiệu $a+b\mathrm{i}$．Nhận thấy mọi số thực và $\mathrm{i}$ đều có dạng $a+b\mathrm{i}(a,b\in \mathbf{R})$, nên các số này đều thuộc hệ số mới．

### Định nghĩa

Định nghĩa các số dạng $a+b\mathrm{i}$ với $a,b\in \mathbf{R}$ là **số phức**; $\mathrm{i}$ gọi là **đơn vị ảo**; tập tất cả số phức gọi là **tập số phức**, ký hiệu $\mathbf{C}$．

Số phức thường ký hiệu $z$, tức $z=a+b\mathrm{i}$．Dạng này gọi là **dạng đại số**．Trong đó $a$ là **phần thực** $\operatorname{Re}(z)$, $b$ là **phần ảo** $\operatorname{Im}(z)$．Nếu không nói rõ, luôn có $a,b\in \mathbf{R}$．

Với số phức $z$, khi và chỉ khi $b=0$ thì là số thực; khi $b\not = 0$ thì là số ảo; khi $a=0$ và $b\not = 0$ thì là số ảo thuần．

Quan hệ giữa số ảo thuần, số ảo, số thực, số phức như hình：

![](./images/complex-relation.svg)

## Tính chất và phép toán

### Ý nghĩa hình học

Ta biết số thực tương ứng một-một với điểm trên trục số．Ta xét số phức tương tự．

Trước hết định nghĩa **hai số phức bằng nhau**: $z_1=a+b\mathrm{i},z_2=c+d\mathrm{i}$ bằng nhau khi và chỉ khi $a=c$ và $b=d$．

Vì vậy, mỗi số phức tương ứng duy nhất với cặp $(a,b)$．Liên hệ với hệ tọa độ Descartes, ta thấy **tập số phức tương ứng một-một với tập điểm trong mặt phẳng**．Đó là ý nghĩa hình học của số phức．

Mặt phẳng này không còn là Descartes thường nữa, vì điểm mang ý nghĩa số phức; ta gọi đó là **mặt phẳng phức**，trục $x$ là **trục thực**, trục $y$ là **trục ảo**．Ta nói: **tập số phức tương ứng một-một với tập điểm trong mặt phẳng phức**．

Từ kiến thức vector, tọa độ vector cũng là cặp $(a,b)$, nên $z=a+b\mathrm{i}$ tương ứng điểm $Z(a,b)$ và vector $\overrightarrow{OZ}=(a,b)$; vậy số phức còn có ý nghĩa: **tập số phức tương ứng một-một với tập vector trong mặt phẳng phức (số thực 0 tương ứng vector không)**．

Do đó ta định nghĩa **mô-đun** của số phức là độ dài vector tương ứng: $|z|=\sqrt{a^2+b^2}$．

Để tiện, ta thường gọi số phức $z=a+b\mathrm{i}$ là điểm $Z$ hoặc vector $\overrightarrow {OZ}$, và xem các vector bằng nhau là cùng một số phức．

Từ vector, ta thấy số ảo không thể so sánh lớn nhỏ (trong khi số thực thì có)．

### Cộng và trừ

Với $z_1=a+b\mathrm{i},z_2=c+d\mathrm{i}$, định nghĩa cộng:

$$
z_1+z_2=(a+c)+(b+d)\mathrm{i}
$$

Rõ ràng tổng vẫn là số phức．

Từ cộng vector, ta thấy cộng số phức phù hợp với cộng vector, củng cố ý nghĩa hình học．

Cộng số phức thỏa **giao hoán** và **kết hợp**：

$$
\begin{aligned}
z_1+z_2&=z_2+z_1\\
(z_1+z_2)+z_3&=z_1+(z_2+z_3)
\end{aligned}
$$

Trừ là phép ngược của cộng, suy ra:

$$
z_1-z_2=(a-c)+(b-d)\mathrm{i}
$$

Cũng phù hợp với trừ vector．

### Nhân, chia và liên hợp

Với $z_1=a+b\mathrm{i},z_2=c+d\mathrm{i}$, định nghĩa nhân:

$$
\begin{aligned}
z_1z_2&=(a+b\mathrm{i})(c+d\mathrm{i})\\
&=ac+bc\mathrm{i}+ad\mathrm{i}+bd\mathrm{i}^2\\
&=(ac-bd)+(bc+ad)\mathrm{i}
\end{aligned}
$$

Nhân số phức tương tự nhân đa thức, chỉ cần thay $\mathrm{i}^2$ bằng $-1$ rồi gom phần thực/ảo．

Nhân số phức tương tự tích vector theo tọa độ．

Dễ thấy nhân số phức thỏa **giao hoán**, **kết hợp**, **phân phối**：

-   $z_1z_2=z_2z_1$
-   $(z_1z_2)z_3=z_1(z_2z_3)$
-   $z_1(z_2+z_3)=z_1z_2+z_1z_3$

Do thỏa các luật, các **hằng đẳng thức nhân** trong số thực cũng đúng trong số phức．

Chia là phép ngược của nhân:

$$
\begin{aligned}
\frac{a+b\mathrm{i}}{c+d\mathrm{i}}&=\frac{(a+b\mathrm{i})(c-d\mathrm{i})}{(c+d\mathrm{i})(c-d\mathrm{i})}\\
&=\frac{ac+bd}{c^2+d^2}+\frac{bc-ad}{c^2+d^2}\mathrm{i} &(c+d\mathrm{i}\not =0)
\end{aligned}
$$

Vector không có phép chia nên không bàn ở đây．

Để mẫu là số thực, ta nhân với $c-d\mathrm{i}$, đây là biểu thức có ý nghĩa．

Với $z=a+b\mathrm{i}$, gọi $a-b\mathrm{i}$ là **liên hợp** của $z$, ký hiệu $\bar z$．Hai số liên hợp **đối xứng qua trục thực**．

Với $z,w$, liên hợp có các tính chất:

-   $z\cdot\bar{z}=|z|^2$
-   $\overline{\overline{z}}=z$
-   $\operatorname{Re}(z)=\dfrac{z+\bar{z}}{2}$，$\operatorname{Im}(z)=\dfrac{z-\bar{z}}{2}$
-   $\overline{z\pm w}=\bar{z}\pm\bar{w}$
-   $\overline{zw}=\bar{z}\bar{w}$
-   $\overline{z/w}=\bar{z}/\bar{w}$

### Góc (argument) và góc chính

Chọn đơn vị thực $1$ làm chiều dương ngang, đơn vị ảo $\mathrm{i}$ làm chiều dương dọc, ta có mặt phẳng phức theo tọa độ Descartes．

Vị trí số phức $z$ có thể biểu diễn bằng tọa độ cực $(r, \theta)$; $r$ là mô-đun của $z$．

Từ trục thực dương đến **số phức khác 0** $z=x+\mathrm{i}y$, góc $\theta$ thỏa:

$$
\tan \theta=\frac{y}{x}
$$

Gọi là **góc (argument)** của $z$, ký hiệu:

$$
\theta= \arg z
$$

Mỗi số phức khác 0 có vô số góc, nên $\arg z$ là một tập．Ký hiệu $\operatorname{Arg} z$ là **một giá trị cụ thể**, thỏa:

$$
-\pi<\operatorname{Arg} z \le \pi
$$

Gọi $\operatorname{Arg} z$ là **góc chính**．Góc $\arg z$ là $\operatorname{Arg} z$ cộng thêm $2k\pi$ với $k\in \mathbf Z$．

Lưu ý tổng của hai góc chính không nhất thiết là góc chính, nhưng tổng của hai góc (argument) luôn là góc hợp lệ．

Tập các số phức có mô-đun < 1 tạo thành **đĩa đơn vị**．Số phức có mô-đun = 1 gọi là **số phức đơn vị**; toàn bộ tạo thành **đường tròn đơn vị** (đôi khi gọi tắt là đĩa đơn vị nếu không nhầm)．

Trong tọa độ cực, nhân/chia số phức rất đơn giản: nhân thì nhân mô-đun, cộng góc; chia thì chia mô-đun, trừ góc．

### Công thức Euler

???+ note "欧拉公式（Euler's formula）[^ref1]"
    Với mọi $x\in\mathbf{R}$,
    
    $$
    \mathrm{e}^{\mathrm{i}x}=\cos x+\mathrm{i}\sin x
    $$
    
    Sau khi bổ sung định nghĩa [hàm mũ phức và hàm lượng giác phức](#指数函数与三角函数), công thức mở rộng cho mọi số phức．

### Hàm mũ và hàm lượng giác

Với số phức $z=x+\mathrm{i}y$, hàm $f(z)=\mathrm{e}^x(\cos y+\mathrm{i}\sin y)$ thỏa $f(z_1+z_2)=f(z_1)f(z_2)$．Do đó định nghĩa **hàm mũ phức**：

$$
\exp z=\mathrm{e}^x(\cos y+\mathrm{i}\sin y)
$$

Hàm mũ phức trên số thực trùng với hàm mũ thực．Tính chất:

-   Mô-đun dương: $|\exp z|=\exp x>0$．
-   Góc: $\arg(\exp z)=\{y + 2k\pi \mid k\in\mathbf Z\}$．
-   Định lý cộng: $\exp (z_1+z_2)=\exp (z_1)\exp (z_2)$．
-   Chu kỳ: $\exp z$ có chu kỳ cơ bản $2\pi \mathrm{i}$．Nếu chu kỳ của $f(z)$ là bội của một chu kỳ, gọi chu kỳ đó là **chu kỳ cơ bản**．

**Hàm lượng giác phức** (cũng gọi tắt là hàm lượng giác) định nghĩa:

$$
\cos z=\frac{\exp (\mathrm{i}z)+\exp (-\mathrm{i}z)}{2}
$$

$$
\sin z=\frac{\exp (\mathrm{i}z)-\exp (-\mathrm{i}z)}{2\mathrm{i}}
$$

Nếu $z\in\mathbf{R}$, từ [công thức Euler](#欧拉公式):

$$
\cos z=\operatorname{Re}\left(\mathrm{e}^{\mathrm{i}z}\right)
$$

$$
\sin z=\operatorname{Im}\left(\mathrm{e}^{\mathrm{i}z}\right)
$$

Trên số thực, định nghĩa trùng với hàm lượng giác thực．Tính chất trên mặt phẳng phức:

-   Tính chẵn lẻ: $\sin$ là hàm lẻ, $\cos$ là hàm chẵn．
-   Đồng nhất thức lượng giác: các hằng đẳng thức như tổng bình phương bằng $1$, công thức cộng-trừ, v.v. đều đúng．
-   Chu kỳ: $\sin$ và $\cos$ có chu kỳ cơ bản $2\pi$．
-   Nghiệm: nghiệm của $\sin$ và $\cos$ thực tạo thành toàn bộ nghiệm của $\sin$ và $\cos$ phức; không phát sinh nghiệm mới．
-   Mô-đun không bị chặn: mô-đun của $\sin$ và $\cos$ phức có thể lớn tùy ý, không bị giới hạn trong $[-1,1]$ như trường hợp thực．

## Ba dạng của số phức

Dựa trên tọa độ Descartes và cực, ta có ba dạng biểu diễn số phức．

**Dạng đại số** dùng cho mọi số phức:

$$
z=x+y\mathrm{i}
$$

Dạng đại số thuận tiện cho cộng, trừ, nhân, chia．

**Dạng lượng giác** và **dạng mũ** dùng cho số phức khác 0:

$$
z=r(\cos \theta +\mathrm{i}\sin \theta)=r \exp (\mathrm{i}\theta)
$$

Hai dạng này thuận tiện cho nhân, chia và các phép tiếp theo．Nếu chỉ dùng hàm quen thuộc THPT, dùng dạng lượng giác; nếu có hàm mũ phức, dùng dạng mũ tiện hơn．

## Căn bậc $n$ của đơn vị

Xét phương trình $x^n=1$ trong số phức．Rõ ràng có $n$ nghiệm, gọi là **$n$-th root of unity**．Theo mặt phẳng phức, các nghiệm chia đều đường tròn đơn vị thành $n$ phần．

Đặt $\omega_n=\exp\dfrac{2\pi \mathrm{i}}{n}$ (góc $2\pi/n$), tập nghiệm $x^n=1$ là $\{\omega_n^k\mid k=0,1\cdots,n-1\}$, trong đó：

$$
w_n^k = \exp\dfrac{2\pi k \mathrm{i}}{n} = \cos\dfrac{2\pi k}{n} + \mathrm{i}\sin\dfrac{2\pi k}{n}.
$$

Nếu không nói rõ, “$n$-th root of unity” thường chỉ nghiệm đầu tiên từ $1$ theo chiều ngược kim đồng hồ, tức $\omega_n$; các nghiệm khác là lũy thừa của $\omega_n$．

???+ tip "Vì sao thường mặc định là nghiệm đầu tiên?"
    Vì tiện ứng dụng: mọi nghiệm đều là lũy thừa của $\omega_n$; và với mọi $k < n$, $\omega_n$ không phải $k$-th root of unity．

### Căn nguyên thủy của đơn vị

Thực ra có nhiều nghiệm có tính chất tương tự $\omega_n$．Gọi tập

$$
\{\omega_n^k\mid 0\le k<n,~\gcd(n,k)=1\}
$$

là các **căn nguyên thủy bậc $n$** (primitive root of unity)．Theo biểu thức trên, có $\varphi(n)$ phần tử, với $\varphi(n)$ là [hàm Euler](./number-theory/euler-totient.md)．

Mọi căn nguyên thủy $\omega$ đều có tính chất: với mọi $0<k<n$, $\omega^k\ne 1$; tức $\omega$ không là $k$-th root of unity．Bởi vậy, dùng một căn nguyên thủy bất kỳ đều có thể sinh toàn bộ nghiệm．

Để hiểu cấu trúc, xét tính chất sau:

???+ note "性质"
    Với số nguyên $n,k$, đặt $d=\gcd(n,k)$, ta có $\omega_n^k = \omega_{n/d}^{k/d}$．

??? note "证明"
    Tính trực tiếp:
    
    $$
    w_n^k = \exp\dfrac{2\pi k\mathrm{i}}{n} = \exp\dfrac{2\pi (k/d)\mathrm{i}}{n/d} = \omega_{n/d}^{k/d}.
    $$

Điều này cho thấy nếu $\gcd(n,k)\neq 1$ thì $\omega_n^k$ là $n/\gcd(n,k)$-th root of unity (có thể nguyên thủy)．Vì vậy, căn nguyên thủy cần có $\gcd(n,k)=1$．

Một hệ quả đơn giản:

???+ note "定理"
    Khi $k$ chạy qua các ước của $n$, các căn nguyên thủy bậc $k$ tạo thành một phân hoạch của các nghiệm bậc $n$．Hơn nữa, với $\ell\perp n$, ánh xạ $x\mapsto x^\ell$ là song ánh trên các nghiệm bậc $n$ và giữ nguyên phân hoạch: nó đưa căn nguyên thủy bậc $k\mid n$ sang căn nguyên thủy bậc $k$．

Dù có nhiều lựa chọn căn nguyên thủy, trong lập trình thi đấu thường dùng $\omega_n$ vì đơn giản nhất．Một số tình huống, để tăng hiệu quả tính toán, có thể dùng [căn nguyên thủy theo modulo](./number-theory/residue.md#单位根) thay cho $\omega_n$ trong số phức．

## Số phức trong ngôn ngữ lập trình

### Số phức trong C

Chuẩn C99 có `<complex.h>`．

Trong `<complex.h>`, có `double complex`, `float complex`, `long double complex`．

Các toán tử `+`, `-`, `*`, `/` dùng cho hỗn hợp số thực và số phức; nếu một vế là số phức thì kết quả là số phức．

`<complex.h>` cung cấp đơn vị ảo `I`, khi include thì không dùng `I` làm tên biến．

Với một số phức, `<complex.h>` có `creal` (phần thực), `cimag` (phần ảo), `cabs` (mô-đun), `carg` (góc chính)．

Các hàm có ba phiên bản theo kiểu: `creal`, `crealf`, `creall` cho `double`, `float`, `long double`．Mặc định không hậu tố là `double`．Các hàm khác tương tự．

Các hàm này trả về số thực．Có thể gán số thực cho số phức, nhưng không thể gán trực tiếp số phức sang số thực, cần dùng các hàm trích phần．

`conj` trả về liên hợp．

`cexp` mũ, `clog` log chính, `csin` sin, `ccos` cos, `ctan` tan．

`cpow` lũy thừa, `csqrt` căn bậc hai, `casin` arcsin, `cacos` arccos, `catan` arctan; tất cả là giá trị chính của hàm nhiều nhánh．

### Số phức trong C++

Trong C, `<ctype.h>` sang C++ là `<cctype>`, đa số header tuân quy ước này．

Tuy nhiên, `<complex.h>` không theo, C++ không có `<ccomplex>`; C++ dùng `<complex>` và nội dung khác C．

Điều này do C++98 đã có `<complex>`, còn C99 mới thêm．

Trong C++, kiểu số phức là `complex<float>`, `complex<double>`, `complex<long double>`．Do đa hình, các hàm không cần hậu tố f/l．

Một đối tượng số phức có thành viên `real` và `imag` để truy cập phần thực/ảo．

Các hàm không thành viên `real`, `imag`, `abs`, `arg` trả phần thực, ảo, mô-đun, góc．

Các hàm không thành viên khác: `norm` (bình phương mô-đun), `conj` (liên hợp)．

Các hàm `exp`, `log` (log cơ số $\mathrm{e}$), `log10` (log cơ số 10, C không có), `pow`, `sqrt`, `sin`, `cos`, `tan` tương tự C．

Từ C++14, có [toán tử literal `std::literals::complex_literals::""if, ""i, ""il`](https://zh.cppreference.com/w/cpp/numeric/complex/operator%2522%2522i.html)．Ví dụ `100if`, `100i`, `100il` lần lượt tạo `std::complex<float>{0.0f, 100.0f}`, `std::complex<double>{0.0, 100.0}`, `std::complex<long double>{0.0l, 100.0l}`．Giúp viết nhanh như `auto z = 4.0 + 3i`．

## Tài liệu tham khảo

-   [Complex number - Wikipedia](https://en.wikipedia.org/wiki/Complex_number)
-   [Euler's formula - Wikipedia](https://en.wikipedia.org/wiki/Euler's_formula)
-   [Complex number arithmetic - cppreference.com](https://en.cppreference.com/w/c/numeric/complex)
-   [std::complex - cppreference.com](https://en.cppreference.com/w/cpp/numeric/complex)

[^ref1]: Về công thức Euler có thể xem hai video: [欧拉公式与初等群论](https://www.bilibili.com/video/BV1fx41187tZ)、[微分方程概论 - 第五章：在 3.14 分钟内理解 $\mathrm{e}^{\mathrm{i}\pi}$](https://www.bilibili.com/video/BV1G4411D7kZ)．
