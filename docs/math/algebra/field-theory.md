Kiến thức trước: [Khái niệm cơ bản về đại số trừu tượng](./basic.md)、[Lý thuyết nhóm](./group-theory.md)、[Lý thuyết vành](./ring-theory.md)

## Giới thiệu

**Lý thuyết trường** (field theory) là lý thuyết về trường.

Lý thuyết trường trong bài này chủ yếu là lý thuyết mở rộng trường. Trường là cấu trúc đại số đóng đối với cộng, trừ, nhân, chia; trong lập trình thi đấu thường xuyên phải lấy modulo một số nguyên tố $p$, tương đương với tính toán trên trường hữu hạn $\mathbf F_p$. Tương tự như trường số thực $\mathbf R$, có những bài toán việc tính toán trên một trường lớn hơn (tức trường số phức $\mathbf C$) thuận tiện hơn; ví dụ điển hình là dùng [biến đổi Fourier nhanh](../poly/fft.md) để tăng tốc nhân đa thức hệ số thực. Với trường hữu hạn cũng có thể làm tương tự. Đa số độc giả chưa quen với mở rộng trường hữu hạn; vì thế hiểu lý thuyết mở rộng trường nói chung là có ích. Cuối bài có nêu một số ứng dụng thuật toán cần mở rộng trường hữu hạn, đồng thời cũng bàn sơ về việc trong một số ứng dụng có thể cần mở rộng trên vành số nguyên.

Lý thuyết Galois liên hệ rất chặt với lý thuyết trường. Nó gắn mở rộng trường với nhóm tự đẳng cấu, từ đó dùng công cụ nhóm để hiểu tính chất của mở rộng trường. Dù đây là nội dung cốt lõi của các môn đại số liên quan, nhưng khá xa với nội dung thi lập trình, nên bài này không giới thiệu chi tiết. Độc giả quan tâm nên đọc sách chuyên ngành.

???+ info "Ký hiệu"
    Khi không gây nhầm lẫn, bài viết có thể lược bỏ ký hiệu nhân trong vành và trường, và viết vành $(R,+,\cdot)$ thành vành $R$, trường $(F,+,\cdot)$ thành trường $F$. Phần tử đơn vị của phép cộng gọi là phần tử không, phần tử đơn vị của phép nhân gọi là phần tử đơn vị (unity). Trong bài, $p$ luôn là số nguyên tố, còn $q$ luôn là lũy thừa nguyên tố và có thể viết $p^n$, với $n$ là số nguyên dương.

## Mở rộng trường

Tương tự trường hợp nhóm, vành, ta có thể định nghĩa trường con và đồng cấu trường.

???+ abstract "Trường con"
    Với trường $F$, nếu một vành con $E$ của nó cũng là trường thì gọi $E$ là **trường con** (subfield) của $F$.

Dù định nghĩa vành/vành con có xử lý phần tử đơn vị thế nào thì trường con $E$ luôn chứa phần tử đơn vị của trường $F$[^subfield-one].

???+ abstract "Đồng cấu trường"
    Đồng cấu vành $\varphi:F\rightarrow E$ từ trường $F$ sang trường $E$ cũng gọi là **đồng cấu trường** (field homomorphism).

??? info "Đồng cấu trường và phần tử đơn vị"
    Nếu trong định nghĩa khác, đồng cấu vành yêu cầu ánh xạ phần tử đơn vị sang phần tử đơn vị, thì đồng cấu trường cũng tự nhiên yêu cầu như vậy. Nếu không, phần tử đơn vị cũng có thể bị ánh xạ thành phần tử không.

Vì trường chỉ có các ideal tầm thường nên đồng cấu trường hoặc là ánh xạ toàn bộ trường thành phần tử không, hoặc là bắt buộc là một ánh xạ nhúng. Điều này cho thấy việc bàn về đồng cấu trường có thể chuyển sang bàn về trường con.

Trong trường hợp trường, thường trường nhỏ hơn là trường quen thuộc hơn, nên ta thường lấy trường con làm gốc để khảo sát trường lớn hơn. Đó là khái niệm mở rộng trường.

???+ abstract "Mở rộng trường"
    Với trường $F$, nếu $F$ là trường con của $E$, thì gọi trường $E$ là **mở rộng** (extension) của $F$, ký hiệu $E/F$.

???+ info "Ký hiệu mở rộng trường"
    Dù hình thức giống nhau, khái niệm mở rộng trường không liên quan đến vành thương, không nên nhầm lẫn.

???+ example "Ví dụ"
    Trường số phức $\mathbf C$ là mở rộng của trường số thực $\mathbf R$, và trường số thực $\mathbf R$ lại là mở rộng của trường số hữu tỉ $\mathbf Q$.

### Bậc của mở rộng trường

Với mở rộng trường $E/F$, trường $E$ luôn là một [không gian tuyến tính](../linear-algebra/vector-space.md) trên $F$. Số chiều của không gian này là bậc của mở rộng.

???+ abstract "Bậc của mở rộng trường"
    **Bậc** (degree) của mở rộng $E/F$ là số chiều của $E$ như một không gian tuyến tính trên $F$, ký hiệu $\dim_F(E)$ hay $[E:F]$. Nếu bậc hữu hạn thì gọi là **mở rộng hữu hạn** (finite extension), ngược lại gọi là **mở rộng vô hạn** (infinite extension).

???+ example "Ví dụ"
    Mở rộng $\mathbf C/\mathbf R$ có bậc $[\mathbf C:\mathbf R]=2$ nên là hữu hạn. Mở rộng $\mathbf R/\mathbf Q$ là vô hạn.

Bậc mở rộng thỏa mãn quy tắc nhân.

???+ note "Định lý"
    Nếu $F\subseteq K\subseteq E$ đều là các trường, thì $[E:F]=[E:K][K:F]$.

??? note "Chứng minh"
    Với trường hợp bậc vô hạn thì hiển nhiên; nếu hữu hạn, giả sử $\{\alpha_i\}$ là một cơ sở của $E$ như không gian tuyến tính trên $K$, và $\{\beta_j\}$ là một cơ sở của $K$ trên $F$, thì có thể kiểm tra rằng $\{\alpha_i\beta_j\}$ là một cơ sở của $E$ trên $F$.

Bài này chủ yếu bàn về các mở rộng hữu hạn.

### Đặc trưng của trường

Điểm khởi đầu tự nhiên khi nghiên cứu mở rộng trường là trường con nhỏ nhất chứa phần tử đơn vị của $F$, gọi là **trường con nguyên tố** (prime subfield) của $F$.

Cấu trúc của trường con nguyên tố được xác định duy nhất bởi tính chất của phần tử đơn vị. Đặc trưng của trường tóm tắt những tính chất này.

???+ abstract "Đặc trưng của trường"
    **Đặc trưng** (characteristic) của trường $F$ là số nguyên dương nhỏ nhất $n$ sao cho $n\cdot 1=0$; nếu không tồn tại thì đặc trưng là $0$. Ở đây $n\cdot 1$ là tổng của $n$ lần phần tử đơn vị $1$. Nếu đặc trưng khác $0$ thì gọi là **đặc trưng hữu hạn** (finite characteristic).

Đặc trưng của trường có thể hiểu qua đồng cấu vành. Vành số nguyên $\mathbf Z$ là cấu trúc đóng tạo từ $0$ và $1$ qua các phép cộng, trừ, nhân. Nó có thể coi như một “nguyên mẫu”, mọi vành có đơn vị đều “kế thừa” một phần cấu trúc của $\mathbf Z$[^initial-object-ring]. Vì vậy với trường $F$, xét đồng cấu $\varphi:\mathbf Z\rightarrow F$ với $\varphi(1)=1$. Đồng cấu này là duy nhất, nó gửi $n\in\mathbf N_+$ đến $n\cdot 1$, tức tổng $n$ lần $1$. Ảnh $\varphi(\mathbf Z)$ nhúng vào $F$, chứa đơn vị, giao hoán, không có ước của $0$, nên là một miền nguyên. Vì thế hạt nhân $\ker\varphi$ là một ideal nguyên tố. Ideal nguyên tố trong $\mathbf Z$ chỉ có dạng $(n)$ với $n=0$ hoặc $n$ là số nguyên tố. Số $n$ đó chính là đặc trưng của trường.

Đặc trưng xác định cấu trúc trường con nguyên tố:

1.  Nếu đặc trưng là $0$ thì $\varphi$ là đơn ánh, $\mathbf Z$ nhúng vào $F$. Trường hữu tỉ $\mathbf Q$ là trường nhỏ nhất chứa $\mathbf Z$, do đó $\mathbf Q$ nhúng vào $F$ và là trường con nguyên tố của $F$;
2.  Nếu đặc trưng là số nguyên tố $p$, thì ảnh $\mathbf Z/p\mathbf Z$ nhúng vào $F$. Lúc này $\mathbf Z/p\mathbf Z$ đã là trường, ký hiệu $\mathbf F_p$, và là trường con nguyên tố của $F$.

Những điều trên cho kết luận sau:

???+ note "Định lý"
    Đặc trưng của trường $F$ chỉ có thể là $0$ hoặc số nguyên tố $p$. Đặc trưng $0$ có trường con nguyên tố là $\mathbf Q$, đặc trưng $p$ có trường con nguyên tố là $\mathbf F_p$.

$\mathbf Q$ và $\mathbf F_p$ trong định lý cũng gọi là **trường nguyên tố** (prime field), tức trường mà trường con chỉ có chính nó. Trường hữu hạn bắt buộc có đặc trưng hữu hạn, vì trường đặc trưng $0$ ít nhất chứa $\mathbf Q$.

Trường đặc trưng hữu hạn và trường đặc trưng $0$ thường có tính chất khác nhau. Ví dụ, trường đặc trưng hữu hạn có các tính chất:

???+ note "Định lý"
    Nếu $F$ có đặc trưng $p$ thì:
    
    1.  Trong nhóm cộng của $F$, mọi phần tử khác $0$ đều có bậc $p$, tức với mọi $x\in F$ đều có $px=0$;
    2.  “Giấc mơ của tân sinh” (freshman's dream): với mọi $x,y\in F$ có $(x+y)^p=x^p+y^p$. Do đó ánh xạ $x\mapsto x^p$ là một tự đồng cấu đơn, gọi là **tự đồng cấu Frobenius** (Frobenius endomorphism).

??? note "Chứng minh"
    Với (1), chỉ cần chú ý $px=(p1)x=0x=0$. Với (2), khai triển nhị thức $(x+y)^p$ cho thấy mọi hệ số ngoài $x^p,y^p$ đều là bội của $p$, nên theo (1) suy ra $(x+y)^p=x^p+y^p$. Để chứng minh $x\mapsto x^p$ là tự đồng cấu, chỉ cần kiểm tra $(xy)^p=x^py^p$, đúng vì phép nhân giao hoán. Cuối cùng, nếu đồng cấu vành giữa các trường gửi $1$ sang $1$ thì nó phải là đơn ánh.

Với trường hữu hạn, tự đồng cấu Frobenius còn là toàn ánh, nên là tự đẳng cấu.

### Mở rộng đơn

Tương tự việc mở rộng $\mathbf R$ sang $\mathbf C$, nhiều mở rộng có thể thực hiện bằng cách thêm vào trường một phần tử mới và quy định phép toán của nó. Trong tổng quát, để tránh rắc rối, ta xét mở rộng $E/F$ và thêm một phần tử từ $E\setminus F$ vào $F$, khi đó các quy tắc tính toán đã được xác định trong $E$.

???+ abstract "Mở rộng do tập con sinh ra"
    Cho $E/F$ là mở rộng trường, $S\subseteq E$. **Mở rộng do $S$ sinh ra trên $F$** (extension generated by $S$ over $F$) là trường con nhỏ nhất của $E$ chứa $F$ và $S$, ký hiệu $F(S)$.

Trường hợp đơn giản nhất là $S$ có ít phần tử.

???+ abstract "Mở rộng hữu hạn sinh"
    Cho $E/F$ là mở rộng trường, nếu tồn tại tập hữu hạn $S=\{\alpha_1,\cdots,\alpha_n\}\subseteq E$ sao cho $E=F(S)$, thì gọi $E$ là **mở rộng hữu hạn sinh** (finitely generated extension) của $F$, ký hiệu $F(\alpha_1,\cdots,\alpha_n)$.

???+ abstract "Mở rộng đơn"
    Cho $E/F$ là mở rộng trường, nếu tồn tại $\alpha\in E$ sao cho $E=F(\alpha)$, thì $E$ là **mở rộng đơn** (simple extension) của $F$. Phần tử $\alpha$ gọi là **phần tử nguyên thủy** (primitive element) của mở rộng này.

???+ example "Ví dụ"
    Các ví dụ sau đều là thêm phần tử từ $\mathbf C$ vào $\mathbf Q$.
    
    1.  Với số nguyên không có thừa số bình phương $D\neq 0,1$, trường bậc hai $\mathbf Q(\sqrt D)$ là mở rộng đơn khi thêm $\sqrt D\in\mathbf C\setminus\mathbf Q$. Bậc mở rộng là $2$, vì $\{1,\sqrt D\}$ là một cơ sở.
    2.  Trường $\mathbf Q(\sqrt 2,\sqrt 3)$ là mở rộng khi thêm $\sqrt 2$ và $\sqrt 3$. Ta có $\mathbf Q(\sqrt 2,\sqrt 3)=\mathbf Q(\sqrt 2)(\sqrt 3)=\mathbf Q(\sqrt 3)(\sqrt 2)$, nên thứ tự thêm không ảnh hưởng. Đây là mở rộng đơn vì $\mathbf Q(\sqrt 2,\sqrt 3)=\mathbf Q(\sqrt 2+\sqrt 3)$. Bậc mở rộng là $4$, với cơ sở $\{1,\sqrt 2,\sqrt 3,\sqrt 6\}$.
    3.  Trường $\mathbf Q(\pi)$ cũng là mở rộng đơn, với $\pi$ là số pi. Đây là mở rộng vô hạn vì $\mathbf Q[\pi]\subseteq \mathbf Q(\pi)$ đã có cơ sở $\{1,\pi,\pi^2,\cdots\}$.
    4.  Trường $\mathbf Q(\pi,\mathrm e)$ là mở rộng hữu hạn sinh nhưng không đơn. Ở đây $\pi$ là số pi, $\mathrm e$ là cơ số tự nhiên của logarit.

Các ví dụ này cho thấy tính chất của mở rộng đơn có thể rất khác nhau, phụ thuộc vào tính chất của phần tử được thêm.

### Mở rộng đại số

Để phân tích các tình huống khi thêm phần tử, ta xét đồng cấu vành từ $F[x]$ tới $E/F$. Ở đây $F[x]$ đóng vai trò giống $\mathbf Z$ trước đó: nó là “nguyên mẫu” khi thêm ẩn $x$ vào $F$ và đóng với các phép toán[^polynomial-universal].

Xét đồng cấu $\varphi:F[x]\rightarrow E$ sao cho $\varphi$ trên $F$ là đồng nhất và $\varphi(x)=\alpha$. Khi đó ảnh $\varphi(F[x])=F[\alpha]$ là miền nguyên, nên hạt nhân $\ker\varphi$ là ideal nguyên tố của $F[x]$. Vì $F[x]$ là PID, nên $\ker\varphi$ có dạng $(f(x))$ với $f(x)=0$ hoặc $f(x)$ là đa thức bất khả quy.

Có các trường hợp:

1.  Nếu $\ker\varphi=\{0\}$ thì $F[x]$ nhúng vào $E$, ảnh $F[\alpha]$ là miền nguyên. Khi đó trường nhỏ nhất chứa $F$ và $\alpha$ là trường phân thức của $F[\alpha]$, tức $F(\alpha)$. Ký hiệu này có thể hiểu là thay $x$ bằng $\alpha$ trong trường phân thức $F(x)$, hoặc là mở rộng đơn do $\alpha$ sinh ra, hai cách hiểu là như nhau trong ngữ cảnh này;

2.  Nếu $\ker\varphi=(f(x))$ với $f(x)$ bất khả quy, thì $\varphi(f(x))=f(\alpha)=0$, tức $\alpha$ là nghiệm của $f(x)$. Có thể giả sử $f(x)$ là đa thức monic. Khi đó ảnh của $\varphi$ là trường $F(\alpha)$, nên

    $$
    F[x]/(f(x))\cong F(\alpha).
    $$

    Lúc này có hai tình huống:

    1.  Nếu $f(x)$ là bậc một, tức $f(x)=x-\alpha$, thì $\alpha\in F$, nên mở rộng $F(\alpha)=F$ là tầm thường;
    2.  Các trường hợp còn lại, $f(x)$ là bất khả quy bậc lớn hơn một, $\alpha\in E\setminus F$, lúc này ảnh $F[\alpha]$ đã là một trường chứa $F$ và $\alpha$, nên chính là $F(\alpha)$, và mở rộng là không tầm thường.

Các thảo luận này dẫn đến các định nghĩa:

???+ abstract "Phần tử đại số và siêu việt"
    Với mở rộng $E/F$, nếu $\alpha\in E$ là nghiệm của một đa thức không không $f(x)\in F[x]$ thì gọi $\alpha$ là **phần tử đại số** (algebraic element) trên $F$; ngược lại gọi là **phần tử siêu việt** (transcendental element).

???+ abstract "Đa thức tối tiểu"
    Với phần tử đại số $\alpha$ trên $F$, đa thức monic bậc nhỏ nhất có $\alpha$ là nghiệm gọi là **đa thức tối tiểu** (minimal polynomial) của $\alpha$.

Đa thức tối tiểu chính là đa thức bất khả quy trong phân tích trước. Tính tối tiểu cho thấy mọi đa thức trên $F$ có $\alpha$ là nghiệm đều phải chia hết cho $f(x)$.

???+ example "Ví dụ"
    1.  $\sqrt 2$ là phần tử đại số trên $\mathbf Q$, đa thức tối tiểu là $x^2-2$.
    2.  $\sqrt 2$ là phần tử đại số trên $\mathbf R$, đa thức tối tiểu là $x-\sqrt 2$.
    3.  $\pi$ là phần tử siêu việt trên $\mathbf Q$.
    4.  Nói chung, phần tử đại số trên $\mathbf Q$ gọi là **số đại số** (algebraic number), phần tử siêu việt gọi là **số siêu việt** (transcendental number). Đặc biệt, nếu đa thức tối tiểu của số đại số là monic thì gọi là **số nguyên đại số** (algebraic integer). Tập các số nguyên đại số trong một mở rộng đại số tạo thành một vành. Ví dụ, số nguyên đại số trong trường bậc hai $\mathbf Q(\sqrt{D})$ tạo thành vành số nguyên bậc hai $\mathbf Z[\omega]$, ký hiệu xem tại [vành số nguyên bậc hai](./ring-theory.md#例子二次整数环).

???+ abstract "Mở rộng đại số và siêu việt"
    Với mở rộng $E/F$, nếu mọi phần tử của $E$ đều là đại số trên $F$ thì $E$ là **mở rộng đại số** (algebraic extension) của $F$; ngược lại gọi là **mở rộng siêu việt** (transcendental extension).

Mở rộng đơn tùy thuộc vào phần tử thêm vào sẽ có hai loại. Nếu phần tử là siêu việt, mở rộng đơn luôn đẳng cấu với trường phân thức hữu tỉ, không thể rút gọn thêm. Nếu phần tử là đại số, mở rộng đơn chính là $F[\alpha]$, tức thay trực tiếp $x$ bằng $\alpha$ trong $F[x]$. Từ góc nhìn sơ cấp, phần tử trong mở rộng này có thể không cần mẫu; điều này nghĩa là quá trình “khử mẫu” luôn thực hiện được trong mở rộng đơn đại số. Vì trong lập trình thi đấu, mở rộng chủ yếu là mở rộng đơn đại số, phần tiếp theo sẽ phân tích chi tiết việc tính toán.

Tầm quan trọng của mở rộng đơn đại số còn thể hiện trong định lý sau:

???+ note "Định lý"
    Mở rộng trường là hữu hạn khi và chỉ khi là mở rộng đại số hữu hạn sinh.

??? note "Chứng minh"
    Cho $F$ là trường, $E=F(\alpha_1,\cdots,\alpha_n)$ là mở rộng đại số hữu hạn sinh, tức các $\alpha_i$ đều đại số trên $F$. Đặt $E_i=F(\alpha_1,\cdots,\alpha_i)$, thì $E_0=F$ và $E_n=E$. Khi đó $\alpha_i$ đại số trên $E_{i-1}$ vì đa thức tối tiểu của $\alpha_i$ trên $F$ cũng là đa thức trên $E_{i-1}$, và bậc đa thức tối tiểu trên $E_{i-1}$ không vượt quá bậc trên $F$. Do đó $[E_i:E_{i-1}]$ hữu hạn, theo quy tắc nhân bậc mở rộng, $[E:F]=\prod_{i=1}^n[E_i:E_{i-1}]$ hữu hạn. Ngược lại, từ $E_0=F$, mỗi bước chọn $\alpha_{i+1}\in E\setminus E_i$ và đặt $E_{i+1}=E_i(\alpha_{i+1})$ đến khi $E_n=E$. Vì bậc mở rộng giảm dần, quá trình dừng hữu hạn. Vậy mở rộng hữu hạn là mở rộng đại số hữu hạn sinh.

Điều này có nghĩa muốn hiểu tính chất của mở rộng hữu hạn, chỉ cần hiểu mở rộng đơn đại số, vì mọi mở rộng hữu hạn có thể tạo bởi hữu hạn lần mở rộng đơn đại số.

### Cấu trúc và tính toán trong mở rộng đơn đại số

Trong mục này, giả sử $F$ là trường số, $E$ là mở rộng của nó, và $\alpha\in E\setminus F$ là phần tử đại số trên $F$. Giả sử đa thức tối tiểu của $\alpha$ là $f(x)$, một đa thức monic bậc $n$:

$$
f(x)=x^n+a_{n-1}x^{n-1}+\cdots+a_1x+a_0,
$$

với $a_0,a_1,\cdots,a_{n-1}\in F$ và $f(x)$ bất khả quy trên $F$.

Đẳng cấu $F(\alpha)\cong F[x]/(f(x))$ cho thấy phép toán trong $F(\alpha)$ tương đương tính toán đa thức modulo $f(x)$. Theo phép chia đa thức có dư, chỉ cần xét các lớp đồng dư của đa thức bậc nhỏ hơn $n=\deg f(x)$. Một cơ sở tự nhiên là $\{1,\alpha,\cdots,\alpha^{n-1}\}$. Do đó:

???+ note "Định lý"
    Với giả thiết trên,
    
    $$
    F(\alpha)=\{\lambda(\alpha)=\lambda_0+\lambda_1\alpha+\cdots+\lambda_{n-1}\alpha^{n-1}:\lambda_0,\lambda_1,\cdots,\lambda_{n-1}\in F\}.
    $$
    
    Ở đây $\lambda(x)$ chạy qua mọi đa thức bậc nhỏ hơn $n$. Vì thế $[F(\alpha):F]=n$, tức bậc của đa thức tối tiểu của $\alpha$. Trong mở rộng, phép cộng của $\lambda(\alpha)$ và $\mu(\alpha)$ là cộng đa thức hệ số theo vị trí; phép nhân cho kết quả $\rho(\alpha)$ với $\rho(x)$ là dư của $\lambda(x)\mu(x)$ khi chia cho $f(x)$.

Với tư cách là trường, còn cần phép chia. Theo mô tả phép nhân, điều này tương đương giải phương trình đồng dư tuyến tính trên đa thức. Tương tự số nguyên, để tính $\lambda(\alpha)/\mu(\alpha)$, ta cần nghịch đảo của $\mu(\alpha)$ rồi nhân. Nghịch đảo của $\mu(\alpha)$ thỏa $\mu(x)\xi(x)\equiv 1\pmod{f(x)}$, có thể tìm bằng Euclid mở rộng.

Sau đây là ví dụ cụ thể.

???+ example "Ví dụ"
    Xét mở rộng $\mathbf Q(\alpha)$, trong đó $\alpha$ là một nghiệm của $x^3-2x-2=0$. Tính
    
    $$
    \frac{1+\alpha}{1+\alpha+\alpha^2}
    $$
    
    Bước đầu là tìm nghịch đảo của $1+\alpha+\alpha^2$, tức giải
    
    $$
    (x^2+x+1)\xi(x)+(x^3-2x-2)\nu(x)=1.
    $$
    
    Dùng Euclid mở rộng. Thực hiện chia:
    
    $$
    \begin{aligned}
    x^3-2x-2 &= (x-1)(x^2+x+1)+(-2x-1),\\
    x^2+x+1 &= \left(-\frac12x-\frac14\right)(-2x-1)+\frac34,\\
    -2x-1 &= \left(-\frac{8}{3}x-\frac{4}{3}\right)\frac{3}{4}.
    \end{aligned}
    $$
    
    Suy ra
    
    $$
    \begin{aligned}
    \frac{3}{4}
    &=(x^2+x+1)+\left(\frac12x+\frac14\right)(-2x-1)\\
    &=(x^2+x+1)+\left(\frac12x+\frac14\right)\left((x^3-2x-2)-(x-1)(x^2+x+1)\right)\\
    &=\left(-\frac12x^2+\frac14x+\frac54\right)(x^2+x+1)+\left(\frac12x+\frac14\right)(x^3-2x-2).
    \end{aligned}
    $$
    
    Do đó nghiệm:
    
    $$
    \xi(x)=-\frac23x^2+\frac13x+\frac53,\ \nu(x)=\frac23x+\frac13.
    $$
    
    Vậy nghịch đảo của $1+\alpha+\alpha^2$ là
    
    $$
    -\frac23\alpha^2+\frac13\alpha+\frac53.
    $$
    
    Bước hai là nhân nghịch đảo với $1+\alpha$:
    
    $$
    \begin{aligned}
    (1+\alpha)\left(-\frac23\alpha^2+\frac13\alpha+\frac53\right)
    &=-\frac23\alpha^3-\frac13\alpha^2+2\alpha+\frac53\\
    &=-\frac23(2\alpha+2)-\frac13\alpha^2+2\alpha+\frac53\\
    &=-\frac13\alpha^2+\frac23\alpha+\frac13.
    \end{aligned}
    $$
    
    Đây là đáp án cuối.

Trong ví dụ chỉ dùng việc $\alpha$ là một nghiệm của phương trình, không cần chỉ rõ nghiệm nào. Đa thức $x^3-2x-2=0$ có một nghiệm thực và một cặp nghiệm phức liên hợp trong $\mathbf C$; bất kỳ nghiệm nào thêm vào $\mathbf Q$ cũng cho mở rộng đẳng cấu. Nghĩa là, ba nghiệm khác nhau không khác biệt về mặt đại số. Tổng quát, với đa thức bất khả quy $f(x)$ trên $F$, các nghiệm $\alpha\neq\beta$ trong một mở rộng tạo ra các mở rộng đơn có tính chất đại số giống nhau; các nghiệm này gọi là **liên hợp** (conjugate). Liên hợp phức thông thường là trường hợp đặc biệt trên $\mathbf C/\mathbf R$.

???+ example "Ví dụ"
    Xét mở rộng $\mathbf F_2(\alpha)$, trong đó $\alpha$ là nghiệm của $x^2+x+1=0$. Với $a+b\alpha$ và $c+d\alpha$, ta có:
    
    $$
    \begin{aligned}
    (a+b\alpha)+(c+d\alpha)&=(a+c)+(b+d)\alpha,\\
    (a+b\alpha)(c+d\alpha)&=ac+(ad+bc)\alpha+bd\alpha^2\\
    &=(ac+bd)+(ad+bc+bd)\alpha.
    \end{aligned}
    $$
    
    Điều này giống phép toán trên số phức. Dù phần tử $\alpha$ lạ, ta vẫn tính toán được. Thực tế $[\mathbf F_2(\alpha):\mathbf F_2]=2$ nên $|\mathbf F_2(\alpha)|=4$, tức thu được trường hữu hạn có 4 phần tử. Chút nữa sẽ thấy mọi trường hữu hạn đều được xây như vậy.

Khi bậc nhỏ, phép toán modulo $f(x)$ có thể thực hiện bằng cách thay

$$
x^n=-a_{n-1}x^{n-1}-\cdots-a_1x-a_0
$$

để hạ bậc. Với bậc thấp, có thể rút ra quy tắc tính hệ số như số phức mà không cần mỗi lần lấy modulo.

Ví dụ về mở rộng đơn đại số có thể xem ở phần [mã tham khảo](#参考实现) của trường hữu hạn.

Các thuật toán này thực tế chỉ xử lý bậc mở rộng thấp, đủ cho đa số bài thi. Nếu bậc cao trở thành nút cổ chai, cần dùng kỹ thuật đa thức ([FFT](../poly/fft.md)、[NTT](../poly/ntt.md)、[chia lấy dư đa thức nhanh](../poly/elementary-func.md#多项式除法--取模)、[Euclid đa thức](../poly/intro.md#因式分解和欧几里得)…) để tăng tốc.

### Trường phân rã

Trên đây đã mô tả chi tiết cấu trúc của mở rộng đơn đại số, nhưng thường vẫn chưa đủ:

???+ example "Ví dụ"
    Xét mở rộng $\mathbf Q(\sqrt[3]{2})/\mathbf Q$. Phần tử $\sqrt[3]{2}$ có đa thức tối tiểu $x^3-2$. Trong $\mathbf C$, $x^3-2$ có ba nghiệm $\sqrt[3]{2},\sqrt[3]{2}\omega,\sqrt[3]{2}\omega^2$, với $\omega=\mathrm{e}^{2\pi\mathrm{i}/3}$ là căn bậc ba nguyên thủy của $1$. Dù $\mathbf Q(\sqrt[3]{2})\cong\mathbf Q(\sqrt[3]{2}\omega)\cong\mathbf Q(\sqrt[3]{2}\omega^2)$, nhưng $\mathbf Q(\sqrt[3]{2})$ không chứa hai nghiệm còn lại, nên các phép toán như $\sqrt[3]{2}+\sqrt[3]{2}\omega$ không thực hiện được. Muốn xét đủ ba nghiệm, cần mở rộng thêm tới $\mathbf Q(\sqrt[3]{2},\sqrt[3]{2}\omega,\sqrt[3]{2}\omega^2)$.
    
    Như đã nói, mở rộng này có thể làm bằng các mở rộng đơn liên tiếp. Lưu ý: $\sqrt[3]{2}\omega$ có đa thức tối tiểu trên $\mathbf Q$ là $x^3-2$, nhưng trên $\mathbf Q(\sqrt[3]{2})$ lại là $x^2+\sqrt[3]{2}x+\sqrt[3]{4}$ vì
    
    $$
    x^3-2 = (x-\sqrt[3]{2})(x^2+\sqrt[3]{2}x+\sqrt[3]{4}).
    $$
    
    Đa thức tối tiểu ban đầu khi mở rộng trường đã tách một nhân tử bậc một, nên bậc tối tiểu của nghiệm còn lại giảm. Quá trình mở rộng chính là quá trình đa thức “phân rã”. Vì vậy mỗi lần mở rộng đơn, cần xác định lại đa thức tối tiểu.

Thêm tất cả các nghiệm của đa thức vào trường cho ta trường phân rã.

???+ abstract "Phân rã"
    Với trường $F$, nếu đa thức $f(x)$ phân tích thành tích các nhân tử bậc một trong $F[x]$, thì nói $f(x)$ **phân rã** (split) trong $F$.

???+ abstract "Trường phân rã"
    Với đa thức $f(x)$ trên $F$, nếu mở rộng $E/F$ thỏa $f(x)$ phân rã trong $E$ nhưng không phân rã trong bất kỳ trường con thực sự nào của $E$, thì $E$ gọi là **trường phân rã** (splitting field) của $f(x)$.

Có thể chứng minh, giống mở rộng đơn, trường phân rã là duy nhất đến đẳng cấu và không phụ thuộc cách xây dựng. Trường phân rã luôn là mở rộng hữu hạn.

???+ abstract "Mở rộng chuẩn"
    Với mở rộng đại số $E/F$, nếu với mọi $\alpha\in E$ thì đa thức tối tiểu của $\alpha$ phân rã trong $E$, thì $E$ gọi là **mở rộng chuẩn** (normal extension) của $F$.

Mở rộng chuẩn là nền tảng trong lý thuyết Galois.

### Trường đóng đại số

Đa số khái niệm mở rộng cần xét trong một trường lớn hơn trường đang nói tới. Dù mở rộng đơn có thể xây từ đa thức mà không cần trường lớn hơn, nhưng trường hợp tổng quát thì không. Với $\mathbf Q$ và $\mathbf R$, ta luôn có thể giả định mở rộng nằm trong $\mathbf C$. Với trường hữu hạn thì không có trường “chuẩn” như vậy. Thực tế, mọi trường đều có bao đóng đại số, khiến mọi mở rộng đại số có thể xem nằm trong đó.

???+ abstract "Bao đóng đại số"
    Với trường $F$, nếu $\overline F$ là mở rộng đại số của $F$ và mọi $f(x)\in F[x]$ đều phân rã trong $\overline F$, thì $\overline F$ gọi là **bao đóng đại số** (algebraic closure) của $F$.

Bao đóng đại số là một mở rộng chuẩn. Cách xây dựng cơ bản là thêm vào mọi nghiệm có thể có của mọi đa thức. Tương tự trường phân rã, bao đóng đại số là duy nhất đến đẳng cấu.

???+ note "Định lý"
    Mọi trường $F$ đều có bao đóng đại số.

??? note "Chứng minh"
    Khó khăn thuộc về lý thuyết tập hợp. Ở đây dùng chứng minh của Artin.
    
    Phần một: từ $F$ xây mở rộng $K_1/F$ sao cho mọi đa thức trên $F$ có ít nhất một nghiệm trong $K_1$. Xét vành đa thức nhiều biến[^multi-poly-ring] $R=F[\cdots,x_f,\cdots]$ với chỉ số $x_f$ chạy qua mọi đa thức monic trên $F$. Gọi $I$ là ideal sinh bởi mọi $f(x_f)$. Trước hết $I\neq R$, nên tồn tại ideal tối đại $M\supseteq I$. Nếu $1\in I$ thì tồn tại hữu hạn đa thức $f_i$ và phần tử $g_i$ sao cho $g_1f_1(x_{f_1})+\cdots+g_kf_k(x_{f_k})=1$. Lấy mở rộng $F(\alpha_1,\cdots,\alpha_k)$ trong đó $\alpha_i$ là nghiệm của $f_i$. Thay $x_{f_i}$ bởi $\alpha_i$ và các biến khác bằng $0$ trong đẳng thức trên, sẽ được $0=1$ trong $F(\alpha_1,\cdots,\alpha_k)$, mâu thuẫn. Vậy $I\neq R$, tồn tại $M$. Khi đó $R/M$ là trường, ký hiệu $K_1$, và mọi $f(x)$ monic trên $F$ có nghiệm $\overline{x_f}$ trong $K_1$.
    
    Phần hai: lặp lại để có chuỗi $K_i$ sao cho mọi đa thức trên $K_i$ có nghiệm trong $K_{i+1}$, với $K_i$ nhúng vào $K_{i+1}$. Lấy $K=\bigcup_{i=1}^\infty K_i$. Dễ thấy $K$ là trường, mọi đa thức trên $K$ có hệ số nằm trong một $K_i$, nên có nghiệm trong $K_{i+1}\subseteq K$. Vậy $K$ là trường đóng đại số.
    
    Cuối cùng, lấy $\overline F$ là tập các phần tử trong $K$ đại số trên $F$. Đây là trường vì với $\alpha,\beta\in\overline F$ thì $\alpha\pm\beta,\alpha\beta,\alpha/\beta\in F(\alpha,\beta)\subseteq\overline F$. Nó là mở rộng đại số của $F$. Với mọi $f(x)\in F[x]$, các nghiệm của $f$ đều đại số trên $F$ nên thuộc $\overline F$, do đó $f$ phân rã trong $\overline F$. Suy ra $\overline F$ là bao đóng đại số của $F$.

???+ example "Ví dụ"
    1.  Bao đóng đại số của $\mathbf R$ là $\mathbf C$.
    2.  Bao đóng đại số của $\mathbf Q$ là tập tất cả số đại số trong $\mathbf C$, ký hiệu $\overline{\mathbf Q}$.

Mọi mở rộng đại số của bao đóng đại số đều là tầm thường. Trường như vậy gọi là trường đóng đại số.

???+ abstract "Trường đóng đại số"
    Nếu mọi đa thức không hằng trên trường $F$ đều có ít nhất một nghiệm trong $F$, thì $F$ là **trường đóng đại số** (algebraically closed field).

Thực tế có các định nghĩa tương đương:

???+ note "Định lý"
    Với trường $F$, các tính chất sau tương đương:
    
    1.  $F$ là trường đóng đại số;
    2.  Mọi đa thức trên $F$ đều phân rã;
    3.  Mọi đa thức bất khả quy trên $F$ đều bậc một;
    4.  $F$ không có mở rộng đại số không tầm thường;
    5.  $F$ không có mở rộng hữu hạn không tầm thường;
    6.  $F$ là bao đóng đại số của một trường.

??? note "Chứng minh"
    Năm tính chất đầu hiển nhiên tương đương theo định nghĩa. Với (6), trường đóng đại số hiển nhiên là bao đóng của chính nó vì không có mở rộng đại số không tầm thường. Ngược lại, nếu $\overline F$ là bao đóng của $F$, với đa thức $f(x)$ trên $\overline F$, lấy một nghiệm $\alpha$ trong trường phân rã của $f$, và tập hệ số khác 0 là $S\subseteq\overline F$. Khi đó $F(S)(\alpha)=F(S\cup\{\alpha\})$ là mở rộng hữu hạn, nên $\alpha$ là đại số trên $F$. Theo định nghĩa bao đóng, đa thức tối tiểu của $\alpha$ trên $F$ phân rã trong $\overline F$, suy ra $\alpha\in\overline F$. Vậy mọi đa thức trên $\overline F$ đều có nghiệm, nên $\overline F$ là trường đóng đại số.

Cuối cùng, [định lý cơ bản của đại số](../poly/fundamental.md)[^fundamental-algebra] cho biết $\mathbf C$ là trường đóng đại số. Trên $\mathbf R$, đa thức bất khả quy bậc tối đa là 2, tương đương với việc mở rộng đại số tối đa là bậc 2, vì trường lớn nhất đạt được là $\mathbf C$.

### Mở rộng tách

Khái niệm trường phân rã đảm bảo mọi đa thức đều có mở rộng chứa đủ nghiệm. Tuy nhiên, trường phân rã không phản ánh bội số của nghiệm. Nếu muốn nghiên cứu tính chất đa thức qua trường phân rã, nên xét “dạng tối giản” theo nghĩa không có nghiệm bội trong bao đóng đại số. Điều này dẫn đến khái niệm đa thức tách (separable).

???+ abstract "Đa thức tách"
    Với đa thức $f(x)$ trên $F$, nếu $f(x)$ không có nghiệm bội trong bao đóng đại số $\overline F$, tức khi phân tích thành nhân tử bậc một không có nhân tử lặp, thì $f(x)$ gọi là **tách** (separable).

Vì luôn phải xét trong bao đóng đại số, tính tách không phụ thuộc vào trường $F$ chọn. Nhưng vì hệ số ở $F$, ta muốn có tiêu chí kiểm tra ngay trên $F$.

Trong giải tích, nghiệm bội có thể xác định bằng đạo hàm. Điều này chuyển sang đa thức bằng đạo hàm hình thức.

???+ abstract "Đạo hàm hình thức"
    Với đa thức
    
    $$
    f(x)=a_0+a_1x+a_2x^2+\cdots+a_{n-1}x^{n-1}+a_nx^n=\sum_{i=0}^na_ix^i
    $$
    
    trên $F$, **đạo hàm (hình thức)** (derivative) ký hiệu $Df(x)$ là
    
    $$
    Df(x)=a_1+2a_2x+\cdots+(n-1)a_{n-1}x^{n-1}+na_nx^{n-1}=\sum_{i=1}^nia_ix^{i-1}.
    $$

Định nghĩa này đúng với mọi trường, không phụ thuộc cấu trúc tô pô. Các quy tắc đạo hàm quen thuộc như $D(fg)=D(f)g+fD(g)$ vẫn đúng.

Để kiểm tra $f$ và $Df$ có nghiệm chung trong trường phân rã, có thể dùng GCD thay vì xây trường phân rã. Khi nghiệm bội tồn tại, đa thức tối tiểu tương ứng lặp lại. Do đó:

???+ note "Định lý"
    Với đa thức $f(x)$ trên $F$, nếu $f$ có nghiệm bội $\alpha$ thì $Df$ cũng có nghiệm $\alpha$. Hơn nữa, $f$ tách khi và chỉ khi $\gcd(f(x),Df(x))=1$.

??? note "Chứng minh"
    Vì phép chia có dư và Euclid vẫn đúng trong mở rộng, chỉ cần xét trong trường phân rã. Nếu $\alpha$ là nghiệm bội bậc $k>1$, thì $f(x)=(x-\alpha)^kg(x)$, nên $Df(x)=k(x-\alpha)^{k-1}g(x)+(x-\alpha)^kDg(x)$ có nghiệm $\alpha$. Ngược lại, nếu $f$ và $Df$ cùng có nghiệm $\alpha$, với $f=(x-\alpha)g$, thì $Df=(x-\alpha)Dg+g$, suy ra $\alpha$ là nghiệm của $g$, tức $\alpha$ là nghiệm bội của $f$. Vậy $f$ không tách khi và chỉ khi $\gcd(f,Df)$ bậc ≥1.

Đa thức luôn phân tích thành tích các đa thức bất khả quy. Vì các đa thức bất khả quy khác nhau có nghiệm khác nhau, nghiệm lặp tương ứng với nhân tử bất khả quy lặp. Vậy nếu phân tích không có nhân tử bất khả quy lặp, có phải tách không? Điều này đúng trong trường hoàn hảo, nhưng không phải luôn đúng. Vấn đề ở trường đặc trưng hữu hạn.

Với đa thức bất khả quy $f$, $\gcd(f,Df)$ chỉ có thể là $1$ hoặc $f$. Nếu là $f$, do $\deg Df<\deg f$ hoặc $Df=0$, suy ra $Df=0$. Điều này có thể xảy ra trong đặc trưng hữu hạn.

Với đặc trưng $p$, nếu $Df(x)=0$ thì mọi hệ số khác 0 chỉ nằm ở bậc chia hết cho $p$, tức

$$
f(x)=a_0+a_px^p+a_{2p}x^{2p}+\cdots+a_{(k-1)p}x^{(k-1)p}+a_{kp}x^{kp}.
$$

Nếu trường $F$ sao cho mọi phần tử có căn bậc $p$ trong $F$, tức với mọi $a_{jp}$ tồn tại $b_j$ sao cho $a_{jp}=b_j^p$, thì theo Frobenius,

$$
\begin{aligned}
f(x)&=a_0+a_px^p+a_{2p}x^{2p}+\cdots+a_{(k-1)p}x^{(k-1)p}+a_{kp}x^{kp}\\
&=b_0^p+b_1^px^p+b_2^px^{2p}+\cdots+b_{k-1}^px^{(k-1)p}+b_k^px^{kp}\\
&=\left(b_0+b_1x+b_2x+\cdots+b_{k-1}x^{k-1}+b_kx^k\right)^p.
\end{aligned}
$$

Khi đó không thể có đa thức bất khả quy dạng này. Trường như vậy gọi là trường hoàn hảo.

???+ abstract "Trường hoàn hảo"
    Nếu mọi đa thức bất khả quy trên $F$ đều tách, thì $F$ gọi là **trường hoàn hảo** (perfect field).

Với trường hoàn hảo, đa thức tách tương đương với việc phân tích không có nhân tử bất khả quy lặp.

???+ note "Định lý"
    Nếu $F$ là trường hoàn hảo, thì đa thức trên $F$ tách khi và chỉ khi nó là tích của các đa thức bất khả quy đôi một khác nhau (theo nghĩa đồng nhất tới phần tử kết hợp).

Các thảo luận trên cũng cho đặc trưng của trường hoàn hảo:

???+ note "Định lý"
    Trường $F$ hoàn hảo khi và chỉ khi đặc trưng là $0$, hoặc đặc trưng là $p$ và mọi $x\in F$ có căn bậc $p$ (tức tự đồng cấu Frobenius là tự đẳng cấu).

$\mathbf Q$ và trường hữu hạn $\mathbf F_q$ đều là trường hoàn hảo.

Nếu trường không hoàn hảo, tồn tại đa thức bất khả quy không tách.

??? example "Ví dụ"
    Xét đa thức $x^2-t$ trên trường phân thức $\mathbf F_2(t)$. Vì $\mathbf F_2(t)$ là trường phân thức của UFD $\mathbf F_2[t]$, và $t$ là phần tử nguyên tố trong $\mathbf F_2[t]$, nên theo tiêu chuẩn Eisenstein, $x^2-t$ bất khả quy trong $\mathbf F_2[t]$, do đó bất khả quy trong $\mathbf F_2(t)$. Nhưng đạo hàm bằng $0$, nên $x^2-t$ không tách. Thực tế, trong mở rộng $\mathbf F_2(t)(\sqrt t)$, nó có nghiệm bội $\sqrt t$.

Quay lại mở rộng trường.

???+ abstract "Mở rộng tách"
    Với mở rộng đại số $E/F$, nếu với mọi $\alpha\in E$ thì đa thức tối tiểu của $\alpha$ là đa thức tách, thì $E$ gọi là **mở rộng tách** (seperable extension).

Mọi mở rộng đại số trên trường hoàn hảo đều là mở rộng tách. Đây cũng là một định nghĩa tương đương của trường hoàn hảo.

Nếu một mở rộng đại số vừa chuẩn vừa tách thì gọi là mở rộng Galois. Trong mở rộng Galois, mọi đa thức bất khả quy đều không có nghiệm bội và số nghiệm đúng bằng bậc, nên hoán vị nghiệm phản ánh đầy đủ tính chất mở rộng. Đây là nền tảng của lý thuyết Galois.

## Trường phân chia (cyclotomic)

Là ví dụ đơn giản của mở rộng trường, phần này bàn về trường phân chia. Một ví dụ đơn giản khác là [trường bậc hai](../number-theory/quadratic.md).

### Nhóm căn đơn vị

Trong $\mathbf C$, nghiệm của $x^n=1$ gọi là **căn đơn vị bậc $n$** (n-th root of unity). Ký hiệu $\zeta_n=\mathrm{e}^{2\pi\mathrm{i}/n}$. Tập tất cả căn đơn vị bậc $n$ là $C_n=\{\zeta_n^k:k\in\mathbf Z\}$. Với phép nhân, $C_n$ là nhóm cyclic bậc $n$, ký hiệu $\langle\zeta_n\rangle$, gọi là nhóm căn đơn vị bậc $n$. Các phần tử có bậc đúng bằng $n$ gọi là **căn đơn vị nguyên thủy bậc $n$** (primitive n-th root of unity). Tập chúng là $P_n=\{\zeta_n^k:k\in\mathbf Z,k\perp n\}$, có đúng $\varphi(n)$ phần tử. Phân loại theo bậc:

$$
C_n=\bigcup_{d|n}P_d.
$$

Đếm phần tử cho ta $n=\sum_{d\mid n}\varphi(d)$.

### Trường phân chia

Trường phân chia là trường thu được bằng cách thêm căn đơn vị vào $\mathbf Q$.

???+ abstract "Trường phân chia"
    Trường $\mathbf Q(\zeta_n)$ thu được bằng cách thêm căn đơn vị $\zeta_n=\mathrm{e}^{2\pi\mathrm{i}/n}$ vào $\mathbf Q$ gọi là **trường phân chia bậc $n$** (n-th cyclotomic field).

Vì mọi căn đơn vị bậc $n$ tạo thành nhóm cyclic, $\mathbf Q(\zeta_n)$ cũng chứa toàn bộ căn đơn vị bậc $n$. Thực tế, $\mathbf Q(\zeta_n)$ chính là trường phân rã của $x^n-1$ trên $\mathbf Q$.

???+ note "Định lý"
    Trường phân chia $\mathbf Q(\zeta_n)$ là trường phân rã của $x^n-1$ trên $\mathbf Q$.

??? note "Chứng minh"
    Gọi $F$ là trường phân rã của $x^n-1$ trên $\mathbf Q$. Vì $\mathbf Q(\zeta_n)$ chứa mọi nghiệm của $x^n-1$, nên $F\subseteq\mathbf Q(\zeta_n)$. Ngược lại, $\zeta_n\in F$ nên $\mathbf Q(\zeta_n)=F$. Vậy $F=\mathbf Q(\zeta_n)$.

Đây là một định nghĩa tương đương. Thực tế, thêm bất kỳ căn đơn vị nguyên thủy bậc $n$ nào cũng cho $\mathbf Q(\zeta_n)$.

### Đa thức phân chia

Trường phân chia $\mathbf Q(\zeta_n)$ là mở rộng đơn đại số của $\mathbf Q$. Theo phân tích trên, nó đẳng cấu với một vành thương của $\mathbf Q[x]$. Để có đẳng cấu, cần biết đa thức tối tiểu của $\zeta_n$. Vì $\zeta_n$ là nghiệm của $x^n-1$, nên đa thức tối tiểu của nó là một ước của $x^n-1$. Theo Gauss, $x^n-1$ phân tích trong $\mathbf Z[x]$ thành tích các đa thức monic bất khả quy.

Ta có:

$$
x^n-1=\prod_{\zeta\in C_n}(x-\zeta)=\prod_{d\mid n}\prod_{\zeta\in P_d}(x-\zeta).
$$

Vì các căn bậc khác nhau có tính chất đại số khác nhau, chúng không thể là nghiệm của cùng một đa thức bất khả quy. Do đó, đa thức tối tiểu của $\zeta_n$ chính là

$$
\Phi_n(x)=\prod_{\zeta\in P_n}(x-\zeta).
$$

Đa thức $\Phi_n(x)$ có các tính chất:

???+ note "Định lý"
    $\Phi_n(x)$ là đa thức monic hệ số nguyên và bất khả quy trong $\mathbf Z[x]$.

??? note "Chứng minh"
    Theo định nghĩa, $\Phi_n(x)$ là monic. Cần chứng minh $\Phi_n(x)\in\mathbf Z[x]$ và bất khả quy. Theo Gauss, $x^n-1$ có cùng phân tích trong $\mathbf Z[x]$ và $\mathbf Q[x]$ với các nhân tử monic nguyên. Mỗi nhân tử $f(x)$ là bất khả quy trong $\mathbf Q[x]$ và phân rã trong $\mathbf Q(\zeta_n)$; mọi nghiệm của $f$ là căn bậc $n$ và có cùng bậc nên nằm trong một $P_d$. Vậy $f$ chia $\Phi_d(x)$, suy ra $\Phi_n(x)$ là tích các đa thức monic hệ số nguyên, nên cũng có hệ số nguyên.
    
    Để chứng minh bất khả quy, giả sử $\Phi_n(x)=f(x)g(x)$ với $f$ bất khả quy trong $\mathbf Z[x]$. Chỉ cần chứng minh nếu $\zeta$ là nghiệm của $f$ thì $\zeta^k$ cũng là nghiệm với mọi $k\perp n$. Chỉ cần kiểm tra với mọi số nguyên tố $p\perp n$. Nếu ngược lại $\zeta^p$ là nghiệm của $g$, thì $\zeta$ là nghiệm chung của $f(x)$ và $g(x^p)$. Vì $f$ là đa thức tối tiểu của $\zeta$ trên $\mathbf Q$, nên $f$ chia $g(x^p)$, tức $g(x^p)=f(x)h(x)$. Lấy modulo $p$ được $\overline g(x^p)=\overline f(x)\overline h(x)$ trong $\mathbf F_p[x]$. Do Frobenius, $\overline g(x)^p=\overline f(x)\overline h(x)$, nên $\overline g$ và $\overline f$ có ước chung không tầm thường, suy ra $x^n-\overline 1=\overline f\overline g$ không tách trong $\mathbf F_p$. Nhưng vì $p\perp n$, đạo hàm $nx^{n-1}$ nguyên tố cùng nhau nên không có nghiệm bội. Do đó $\zeta^p$ cũng là nghiệm của $f$, và $f$ chứa mọi căn đơn vị nguyên thủy bậc $n$, tức $f=\Phi_n$.

Vậy $\Phi_n$ là đa thức tối tiểu của $\zeta_n$, gọi là **đa thức phân chia bậc $n$** (n-th cyclotomic polynomial). Nó có $\varphi(n)$ nghiệm phức, chính là các căn đơn vị nguyên thủy bậc $n$. Do đó $[\mathbf Q(\zeta_n):\mathbf Q]=\varphi(n)$.

Vành số nguyên đại số của $\mathbf Q(\zeta_n)$ là $\mathbf Z[\zeta_n]$. Ngoài ra, khi $\varphi(n)=2$, trường phân chia là [mở rộng bậc hai](../number-theory/quadratic.md): $\mathbf Q(\zeta_4)=\mathbf Q(\sqrt{-1})$, $\mathbf Q(\zeta_3)=\mathbf Q(\zeta_6)=\mathbf Q(\sqrt{-3})$.

Sử dụng đa thức phân chia, ta có phân tích duy nhất:

$$
x^n-1=\prod_{d\mid n}\Phi_d(x).
$$

Vì vậy $(x^d-1)\mid(x^n-1)$ khi và chỉ khi $d\mid n$. Áp dụng [phản diễn Möbius](../number-theory/mobius.md) được

$$
\Phi_d(x)=\prod_{d\mid n}(x^d-1)^{\mu(n/d)}.
$$

Từ đó có thể tính đệ quy các đa thức phân chia. Dưới đây là một số ví dụ:

???+ example "Đa thức phân chia"
    10 đa thức đầu tiên:
    
    $$
    \begin{aligned}
    \Phi_1(x) &= x-1,\\
    \Phi_2(x) &= x+1,\\
    \Phi_3(x) &= x^2+x+1,\\
    \Phi_4(x) &= x^2+1,\\
    \Phi_5(x) &= x^4+x^3+x^2+x+1,\\
    \Phi_6(x) &= x^2-x+1,\\
    \Phi_7(x) &= x^6+x^5+x^4+x^3+x^2+x+1,\\
    \Phi_8(x) &= x^4+1,\\
    \Phi_9(x) &= x^6+x^3+1,\\
    \Phi_{10}(x) &= x^4-x^3+x^2-x+1.
    \end{aligned}
    $$
    
    Một факт thú vị: tuy các hệ số ở đây chỉ là $0$ và $\pm1$, nhưng với $n$ tổng quát điều này không đúng. Phản ví dụ đầu tiên là $\Phi_{105}(x)$, và có thể chứng minh khi $n$ tăng, hệ số có thể lớn tùy ý.

Từ công thức Möbius, có các tính chất giúp tính $\Phi_n$:

???+ note "Tính chất"
    Với đa thức phân chia $\Phi_n(x)$:
    
    1.  Nếu $p\mid n$ thì $\Phi_{pn}(x)=\Phi_n(x^p)$;
    2.  Nếu $p\perp n$ thì $\Phi_{pn}(x)=\dfrac{\Phi_n(x^p)}{\Phi_n(x)}$;
    3.  Nếu $n$ lẻ thì $\Phi_{2n}(x)=\Phi_n(-x)$;
    4.  Với số nguyên tố $p$, $\Phi_{p}(x)=1+x+\cdots+x^{p-1}$;
    5.  Đặc biệt, $\Phi_{2^k}(x)=x^{2^{k-1}}+1$.

Những tính chất này cho thấy việc tính $\Phi_n$ tập trung vào trường hợp $n$ lẻ không có bình phương. Khi đó có thể thêm từng thừa số nguyên tố bằng tính chất (2), mỗi bước chỉ cần một phép chia đa thức.

Đa thức phân chia còn nhiều tính chất khác:

???+ note "Định lý"
    Với đa thức phân chia $\Phi_n(x)$, $n>1$, bậc là $\varphi(n)$, ta có:
    
    1.  $\Phi_n(x)$ là đa thức đối xứng, hệ số bậc $j$ bằng hệ số bậc $\varphi(n)-j$, tức $\Phi_n(x)=x^{\varphi(n)}\Phi_n(1/x)$;
    2.  Hệ số bậc $\varphi(n)-1$ bằng $-\mu(n)$ (hàm Möbius);
    3.  Nếu $n$ là lũy thừa nguyên tố $p^k$, thì $\Phi_n(1)=p$; nếu không thì $\Phi_n(1)=1$;
    4.  Với $b>1$ và $p$ là ước nguyên tố của $\Phi_n(b)$, thì $p\mid n$ hoặc $n$ là bậc của $b$ trong $(\mathbf Z/p\mathbf Z)^\times$, và hai trường hợp này loại trừ nhau.

??? note "Chứng minh"
    Ba tính chất đầu dùng phản diễn Möbius. (1) xét trực tiếp $\Phi_n(x)=\prod_{d\mid n}(x^d-1)^{\mu(n/d)}$; (2) so sánh hệ số bậc $n-1$ trong $x^n-1=\prod_{d\mid n}\Phi_d(x)$ rồi phản diễn; (3) chia hai vế cho $\Phi_1(x)=x-1$, thay $x=1$ và phản diễn.
    
    Chứng minh (4): nếu $n$ là bậc của $b$ trong $(\mathbf Z/p\mathbf Z)^\times$, thì $p\mid b^n-1$ và $p\mid\Phi_n(b)$. Ngược lại, nếu $p\mid\Phi_n(b)$ thì $b^n\equiv1\pmod p$. Nếu $n$ không phải bậc của $b$, gọi $k$ là bậc, thì $k\mid n$ và $p\mid\Phi_k(b)$. Khi đó $\Phi_k$ và $\Phi_n$ có nghiệm chung $b$ trong $\mathbf F_p$, suy ra $x^n-1$ có nghiệm bội $b$. Điều này buộc $p\mid n$; nếu $p\nmid n$ thì $x^n-1$ và đạo hàm nguyên tố cùng nhau nên không có nghiệm bội. Vậy ước nguyên tố $p$ của $\Phi_n(b)$ chỉ có hai khả năng: $p\mid n$ hoặc $n$ là bậc của $b$ modulo $p$, và hai khả năng loại trừ nhau vì trường hợp sau suy ra $n\mid p-1$.

Đa thức phân chia còn dùng trong nhiều bài toán số học và đại số, ví dụ độ dài chu kỳ phần thập phân của phân số trong một cơ số nào đó. Độc giả quan tâm xem thêm tài liệu cuối bài.

## Trường hữu hạn

**Trường hữu hạn** (finite field), còn gọi là **trường Galois** (Galois field), là trường có hữu hạn phần tử. Cấu trúc của trường hữu hạn được xác định duy nhất bởi số phần tử, và số đó bắt buộc là lũy thừa nguyên tố.

???+ note "Định lý"
    Tồn tại trường có $q$ phần tử khi và chỉ khi $q$ có dạng $p^n$. Trường này là duy nhất đến đẳng cấu, ký hiệu $\mathbf F_q$. Số nguyên tố $p$ là đặc trưng của $\mathbf F_q$, số nguyên dương $n$ là bậc của mở rộng $\mathbf F_q/\mathbf F_p$. Cuối cùng, $\mathbf F_q$ là trường phân rã của $x^q-x$ trên $\mathbf F_p$, và gồm đúng $q$ nghiệm phân biệt của $x^q-x$.

??? note "Chứng minh"
    Gọi $F$ là một trường hữu hạn. Đặc trưng của $F$ là một số nguyên tố $p$, nên $F$ có trường con nguyên tố $\mathbf F_p$. $F$ là mở rộng hữu hạn của $\mathbf F_p$, bậc $n$, nên $|F|=q=p^n$. Nhóm nhân $F^\times$ có bậc $q-1$, nên $x^{q-1}=1$ với mọi $x\neq0$. Do đó mọi phần tử $x\in F$ đều thỏa $x^q=x$, tức là nghiệm của $x^q-x$. Vì đa thức $x^q-x$ có bậc $q$, nó phải bằng $\prod_{\alpha\in F}(x-\alpha)$ và phân rã trong $F$. Với mọi trường khiến $x^q-x$ phân rã, do có $q$ nghiệm phân biệt, trường phải có ít nhất $q$ phần tử. Vậy $F$ là trường phân rã nhỏ nhất của $x^q-x$ và là duy nhất đến đẳng cấu.
    
    Ngược lại, cho $p$ và $q=p^n$, xét trường phân rã của $x^q-x$ trên $\mathbf F_p$ và gọi $F$ là tập tất cả nghiệm. Cần chứng minh $F$ là trường. Lặp Frobenius $n$ lần cho thấy $x\mapsto x^q$ là tự đồng cấu, nên với $\alpha,\beta\in F$ có $(\alpha\pm\beta)^q=\alpha^q\pm\beta^q$, $(\alpha\beta)^q=\alpha^q\beta^q$, $(\alpha^{-1})^q=(\alpha^q)^{-1}$. Do đó $F$ đóng với cộng, trừ, nhân, chia, nên là trường. Vậy $F$ chính là trường phân rã, có $q$ phần tử.

???+ note "Hệ quả"
    Trong trường hữu hạn $\mathbf F_q$ ($q>2$), tổng mọi phần tử khác 0 là $0$, tích là $-1$.

??? note "Chứng minh"
    Mọi phần tử khác 0 là nghiệm của $x^{q-1}-1$, dùng hệ thức Viète.

Trong trường nguyên tố $\mathbf F_p$, kết luận về tích chính là (một phần của) [định lý Wilson](../number-theory/factorial.md#wilson-定理).

### Cấu trúc nhân

Nhóm nhân của trường hữu hạn $\mathbf F^\times=\mathbf F\setminus\{0\}$ luôn là nhóm cyclic.

???+ note "Định lý"
    Mọi nhóm con hữu hạn của nhóm nhân của một trường đều là nhóm cyclic.

??? note "Chứng minh"
    Gọi $G$ là nhóm con hữu hạn của nhóm nhân, $|G|=n$. Theo cấu trúc nhóm Abel hữu hạn, $G\cong C_{n_1}\times\cdots\times C_{n_s}$ với $n_1\mid\cdots\mid n_s$. Khi đó mọi phần tử $x\in G$ thỏa $x^{n_s}=1$, tức là nghiệm của $x^{n_s}-1$. Đa thức này có nhiều nhất $n_s$ nghiệm phân biệt, nên $n\le n_s$. Mà $n_s\le n$ nên $n_s=n$, suy ra $G$ là cyclic.

???+ note "Hệ quả"
    Nhóm nhân của $\mathbf F_q$ là $\mathbf F_q^\times\cong C_{q-1}$.

Nhóm cyclic $\mathbf F_q^\times$ có $\varphi(q-1)$ phần tử sinh, gọi là phần tử nguyên thủy của trường hữu hạn.

???+ abstract "Phần tử nguyên thủy"
    Phần tử sinh của nhóm nhân $\mathbf F_q^\times$ gọi là **phần tử nguyên thủy** (primitive element) của $\mathbf F_q$.

??? warning "Phần tử nguyên thủy trong mở rộng đơn và trong trường hữu hạn là khác nhau"
    Tên gọi giống nhau nhưng khác nghĩa. Phần tử nguyên thủy trong mở rộng đơn là phần tử sinh mở rộng; phần tử nguyên thủy của trường hữu hạn là phần tử sinh nhóm nhân. Phần tử nguyên thủy của mở rộng không nhất thiết là phần tử nguyên thủy của trường hữu hạn. Ví dụ, trong $\mathbf F_{25}\cong\mathbf F_5[x]/(x^2+x+1)$, $\overline x$ là phần tử nguyên thủy của mở rộng nhưng không phải phần tử nguyên thủy của $\mathbf F_{25}$ vì bậc của nó là $3$.

??? warning "Phần tử nguyên thủy của $\mathbf F_q$ không trùng với căn nguyên modulo $q$"
    Với trường hữu hạn $\mathbf F_q$ đặc trưng lẻ, luôn tồn tại [căn nguyên modulo $q$](./ring-theory.md#应用整数同余类的乘法群). Nhưng không nên nhầm với phần tử nguyên thủy của $\mathbf F_q$. Dù cả hai là phần tử sinh của nhóm nhân tương ứng, $(\mathbf Z/q\mathbf Z)^\times$ và $\mathbf F_q$ không giống nhau khi $q$ không nguyên tố. Ví dụ, bậc của nhóm đầu là $\varphi(q)$ còn nhóm sau là $q-1$, khác nhau.

Nếu $\alpha$ là phần tử nguyên thủy của $\mathbf F_q$, thì với mọi $x\in\mathbf F_q$ tồn tại duy nhất $k<q-1$ sao cho $x=\alpha^k$; $k$ gọi là **logarit rời rạc** (discrete logarithm) của $x$ theo cơ sở $\alpha$. Tương tự trường $\mathbf F_p$, các thuật toán [logarit rời rạc](../number-theory/discrete-logarithm.md) thường có độ phức tạp cao.

Nhờ phép nhân, phần tử nguyên thủy sinh ra toàn bộ phần tử khác 0. Điều này cho thấy trường hữu hạn như một mở rộng của trường con luôn là mở rộng đơn.

???+ note "Định lý"
    Với trường hữu hạn $\mathbf F_q$, gọi $F$ là một trường con của nó, thì $\mathbf F_q$ là mở rộng đơn đại số của $F$; nếu $\alpha$ là phần tử nguyên thủy của $\mathbf F_q$ thì $\mathbf F_q=F(\alpha)$.

Đa thức tối tiểu của phần tử nguyên thủy là đa thức bất khả quy trên trường con.

### Quan hệ bao hàm

Trường con của trường hữu hạn cũng là hữu hạn. Quan hệ bao hàm giữa các trường hữu hạn được quyết định hoàn toàn bởi số phần tử.

???+ note "Định lý"
    Với các trường hữu hạn $\mathbf F_q$ và $\mathbf F_r$, $\mathbf F_r$ là trường con của $\mathbf F_q$ khi và chỉ khi tồn tại $k$ sao cho $q=r^k$. Tương đương, $\mathbf F_{p^d}$ là trường con của $\mathbf F_{p^n}$ khi và chỉ khi $d\mid n$.

??? note "Chứng minh"
    Nếu $\mathbf F_r$ là trường con của $\mathbf F_q$ thì hai trường có cùng đặc trưng $p$. Gọi bậc của các mở rộng $\mathbf F_q/\mathbf F_r$, $\mathbf F_r/\mathbf F_p$, $\mathbf F_q/\mathbf F_p$ lần lượt là $k,d,n$, thì $n=kd$, và $r=p^d$, $q=p^n$, nên $q=(p^d)^k=r^k$.
    
    Ngược lại, nếu $d\mid n$, đặt $r=p^d$, $q=p^n$. Xét $F$ là tập nghiệm của $x^r-x=0$ trong $\mathbf F_q$. Dùng Frobenius có thể chứng minh $F$ là trường; chỉ cần chứng minh có đúng $r$ nghiệm. Do $d\mid n$, $(p^d-1)\mid(p^n-1)$, nên $(x^r-x)\mid(x^q-x)$, tức $x^r-x$ phân rã trong $\mathbf F_q$ và có $r$ nghiệm phân biệt. Vậy $F\cong\mathbf F_r$ là trường con của $\mathbf F_q$.

Định lý cho thấy quan hệ bao hàm giữa $\mathbf F_{p^n}$ ứng với quan hệ chia hết giữa các số mũ $n$. Tập tất cả các trường hữu hạn đặc trưng $p$ tạo thành một lưới isomorphic với lưới các số nguyên theo quan hệ chia hết. Để xét giao/hợp, cần nhúng tất cả vào bao đóng đại số của $\mathbf F_p$.

???+ note "Định lý"
    Gọi $F$ là bao đóng đại số của $\mathbf F_p$, tập nghiệm của $x^{p^n}-x$ trong $F$ tạo thành $\mathbf F_{p^n}$. Khi đó:
    
    1.  $F=\bigcup_{n=1}^\infty\mathbf F_{p^n}$, tức bao đóng đại số là hợp của mọi trường hữu hạn đặc trưng $p$;
    2.  Các trường $\mathbf F_{p^n}$ tạo thành lưới theo bao hàm, isomorphic với lưới các số $n$ theo chia hết. Đặc biệt, $\mathbf F_{p^n}\cap\mathbf F_{p^m}=\mathbf F_{p^{\gcd(n,m)}}$ và $\mathbf F_{p^n}\mathbf F_{p^m}=\mathbf F_{p^{\operatorname{lcm}(n,m)}}$.

??? note "Chứng minh"
    Mấu chốt là phần (1). Phần (2) suy ra từ định lý về trường con.
    
    Lấy $\alpha\in\bigcup_{n\ge1}\mathbf F_{p^n}$, tồn tại $n$ sao cho $\alpha\in\mathbf F_{p^n}$, nên $\alpha$ là đại số trên $\mathbf F_p$. Vậy hợp này là mở rộng đại số. Với mọi đa thức $f(x)$ bậc $m$ trên $\mathbf F_p$, các nghiệm $\{\alpha_i\}$ nằm trong một số $\mathbf F_{p^{n_i}}$, nên nằm trong hợp. Do đó mọi đa thức phân rã trong hợp, suy ra đây là bao đóng đại số.

### Nhóm tự đẳng cấu

Trường con của $\mathbf F_q$ là tập nghiệm của $x^r-x=0$, tức là tập điểm cố định của ánh xạ $x\mapsto x^r$. Điều này phản ánh tương ứng sâu sắc giữa trường con và nhóm con của nhóm tự đẳng cấu.

Mọi trường đặc trưng $p$ đều có tự đồng cấu Frobenius $\sigma_p:x\mapsto x^p$. Với trường hữu hạn, đây là tự đẳng cấu, nên trường hữu hạn là hoàn hảo. Nhóm tự đẳng cấu của $\mathbf F_q$ là nhóm cyclic bậc $n$ sinh bởi $\sigma_p$.

???+ note "Định lý"
    $\operatorname{Aut}(\mathbf F_q)=\langle\sigma_p\rangle$ là nhóm cyclic bậc $n$, với sinh bởi Frobenius $\sigma_p:x\mapsto x^p$.

??? note "Chứng minh"
    $\sigma_p$ là tự đẳng cấu vì đơn ánh trên tập hữu hạn là toàn ánh. Bậc của $\sigma_p$ là $n$ vì $\sigma_p^n(x)=x^{p^n}=x$ với mọi $x$, và với $k<n$ thì $\sigma_p^k$ không thể là đồng nhất. Mặt khác, $\operatorname{Aut}(\mathbf F_q)$ có nhiều nhất $n$ phần tử: một tự đẳng cấu được xác định bởi ảnh của phần tử nguyên thủy $\alpha$, và ảnh đó phải là một nghiệm liên hợp của đa thức tối tiểu của $\alpha$, có đúng $n$ nghiệm. Vậy nhóm tự đẳng cấu chính là $\langle\sigma_p\rangle$.

Nhóm con của $\operatorname{Aut}(\mathbf F_q)$ tương ứng một-một với trường con của $\mathbf F_q$.

???+ note "Định lý"
    Với trường hữu hạn $\mathbf F_q$, gọi $\mathcal F$ là tập các trường con và $\mathcal G$ là tập các nhóm con của $\operatorname{Aut}(\mathbf F_q)$, khi đó:
    
    1.  Với $F\in\mathcal F$, đặt $\operatorname{Aut}(\mathbf F_q/F)=\{\sigma\in\operatorname{Aut}(\mathbf F_q):\forall x\in F(\sigma(x)=x)\}$, thì $\operatorname{Aut}(\mathbf F_q/F)\le\operatorname{Aut}(\mathbf F_q)$;
    2.  Với $G\in\mathcal G$, đặt $F^G=\{x\in\mathbf F_q:\forall\sigma\in G(\sigma(x)=x)\}$, thì $F^G$ là trường con;
    3.  Hai ánh xạ $F\mapsto\operatorname{Aut}(\mathbf F_q/F)$ và $G\mapsto F^G$ là nghịch đảo nhau;
    4.  Tương ứng này đảo chiều bao hàm: nếu $F_1\subseteq F_2$ thì $\operatorname{Aut}(\mathbf F_q/F_2)\le\operatorname{Aut}(\mathbf F_q/F_1)$.

Đây là trường hợp đặc biệt của định lý cơ bản Galois.

### Đa thức bất khả quy

Đa thức bất khả quy trên $\mathbf F_q$ dễ xác định. Mỗi đa thức bất khả quy bậc $n$ tương ứng với một mở rộng đại số bậc $n$, duy nhất, nên mọi nghiệm đều nằm trong $\mathbf F_{q^n}$. Do đó, mọi đa thức bất khả quy bậc $n$ là ước của $x^{q^n}-x$. Cần xét phân tích $x^{q^n}-x$ trên $\mathbf F_q$.

Các phần tử đại số trên $\mathbf F_q$ phân loại theo bậc đa thức tối tiểu. Gọi $P_n$ là tập phần tử có bậc tối tiểu đúng $n$, thì

$$
\mathbf F_{q^n} = \bigcup_{d\mid n}P_d.
$$

Tương ứng:

$$
x^{q^n}-x = \prod_{d|n}\prod_{\zeta\in P_d}(x-\zeta).
$$

Đa thức bất khả quy bậc $n$ có $n$ nghiệm, đều thuộc $P_n$, nên là ước của

$$
\prod_{\zeta\in P_n}(x-\zeta) = \prod_{d\mid n}\left(x^{q^d}-x\right)^{\mu(n/d)}.
$$

Bậc của đa thức này là

$$
\sum_{d\mid n}\mu(d)q^{n/d},
$$

nên số đa thức monic bất khả quy bậc $n$ trên $\mathbf F_q$ là

$$
\frac1n\sum_{d\mid n}\mu(d)q^{n/d}.
$$

Số này trùng với số vòng cổ độ dài $n$ với $q$ màu (không tính quay), nên gọi là đa thức vòng cổ (necklace polynomial).

???+ note "Định lý"
    Trên $\mathbf F_q$ tồn tại đa thức bất khả quy ở mọi bậc.

Do cấu trúc đơn giản, phân tích đa thức trên $\mathbf F_q$ dễ. Ví dụ, để tìm mọi ước bất khả quy bậc $n$ của một đa thức, chỉ cần lấy $\gcd$ với $x^{q^n}-x$[^ddf]. Tương tự, nếu đa thức bậc $n$ nguyên tố cùng $x^{q^k}-1$ với mọi $k<n$, thì nó bất khả quy.

Như đã nói, nghiệm của đa thức bất khả quy chưa chắc là phần tử nguyên thủy. Đa thức tối tiểu của phần tử nguyên thủy của $\mathbf F_q$ trên $\mathbf F_p$ gọi là **đa thức nguyên thủy** (primitive polynomial)[^prim-poly]. Nếu dùng đa thức này để xây mở rộng thì $\overline x$ chắc chắn là phần tử nguyên thủy. Đa thức nguyên thủy bậc $n$ trên $\mathbf F_p$ có thể tìm bằng cách phân tích đa thức phân chia $\Phi_n(x)$ trên $\mathbf F_p$.

???+ note "Định lý"
    Cho $p$ là số nguyên tố, $n$ là số nguyên dương và $p\perp n$. Gọi $d$ là bậc của $p$ trong $(\mathbf Z/n\mathbf Z)^\times$. Khi đó $\Phi_n(x)$ phân tích trong $\mathbf F_p$ thành $\dfrac{\varphi(n)}{d}$ đa thức nguyên thủy bậc $d$. Đặc biệt, $\Phi_n(x)$ bất khả quy trên $\mathbf F_p$ khi và chỉ khi $p$ là căn nguyên modulo $n$.

??? note "Chứng minh"
    Nhận xét rằng nghiệm của $\Phi_n$ là các căn đơn vị nguyên thủy bậc $n$ trong $\mathbf F_p$, và bậc đa thức tối tiểu của một căn đơn vị nguyên thủy chính là độ dài quỹ đạo dưới nhóm tự đẳng cấu $\langle\sigma_p\rangle$, tức là bậc của $p$ trong $(\mathbf Z/n\mathbf Z)^\times$. Nếu không dùng Galois, có thể chứng minh $d$ là số nhỏ nhất sao cho $(x^n-1)\mid(x^{p^d-1}-1)$. Phần còn lại hiển nhiên.

Dù đa thức bất khả quy rất quan trọng, tìm một đa thức bất khả quy bậc $n$ trên $\mathbf F_q$ không có phương pháp tất định tốt. Thường dùng ngẫu nhiên: vì tỷ lệ đa thức monic bất khả quy là $\Theta(1/n)$, nên thử ngẫu nhiên sẽ tìm được sau kỳ vọng $\Theta(n)$ lần. Đa thức tìm được có thể không nguyên thủy và hệ số phức tạp. Trong thực tế, nếu $q$ cố định, thường dùng bảng tra đa thức nguyên thủy hệ số đơn giản[^list-prim-poly].

### Mã tham khảo

Phần này cung cấp một hiện thực trường hữu hạn đơn giản, chỉ để tham khảo. Code có phương pháp sinh ngẫu nhiên đa thức bất khả quy.

??? example "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/finite-field/finite-field_1.cpp"
    ```

Trong mật mã thường dùng trường hữu hạn đặc trưng $2$. Với loại này, có thể lưu phần tử bằng chuỗi bit và dùng phép toán bit để thực hiện.

## Ứng dụng

Phần này liệt kê một số ứng dụng của mở rộng trường trong lập trình thi đấu. Chủ yếu là khi tính biểu thức trong trường, cần thêm phần tử không có trong trường gốc để phép tính trở nên khả thi. Độc giả nên quen với việc dùng số phức giải bài toán thực; đó là mở rộng trên $\mathbf R$. Trường hợp ít quen hơn là mở rộng trên trường hữu hạn, nên phần này tập trung vào đó, đặc biệt là mở rộng trên $\mathbf F_p$.

Trong một số trường hợp, mở rộng làm giảm độ phức tạp, nên là cần thiết (ví dụ FFT trên $\mathbf R$). Trong các trường hợp khác, mở rộng chỉ là một trong nhiều cách giải, có thể có phương pháp khác tương đương hoặc tốt hơn; ví dụ sắp nói về Fibonacci. Độc giả nên so sánh ưu nhược để chọn phương pháp phù hợp.

### Dãy Fibonacci

Tính [dãy Fibonacci](../combinatorics/fibonacci.md) thường dùng DP $O(n)$ hoặc lũy thừa ma trận $O(\log n)$. Thực ra có thể dùng mở rộng trường cũng đạt $O(\log n)$. Công thức tổng quát:

$$
f(n) = \frac{1}{\sqrt{5}}\left(\left(\frac{1+\sqrt{5}}{2}\right)^n-\left(\frac{1-\sqrt{5}}{2}\right)^n\right).
$$

Cần tính $f(n)\bmod p$ với $p\neq 5$[^fib-p5]. Chuyển sang tính trên $\mathbf F_p$, vấn đề là $\sqrt 5$ trong $\mathbf F_p$. Nếu $5$ là thặng dư bậc hai modulo $p$, ta tính trực tiếp. Nếu không, cần làm trong mở rộng $\mathbf F_p(\sqrt 5)\cong\mathbf F_p[x]/(x^2-5)$.

Không nhất thiết phải thêm $\sqrt 5$. Ta có thể đặt $\phi$ là nghiệm của $x^2-x-1$, khi đó

$$
f(n)=\frac{\phi^n-(-\phi)^{-n}}{2\phi-1}=\frac{\phi^n-(1-\phi)^n}{2\phi-1}.
$$

Nếu $5$ không là thặng dư bậc hai, thì $x^2-x-1$ bất khả quy, có thể tính trong $\mathbf F_p(\theta)\cong\mathbf F_p[x]/(x^2-x-1)$.

Cách này có thể tổng quát cho bài toán khác. Nhưng lưu ý: tính khả quy của đa thức trên $\mathbf F_p$ không giống trên $\mathbf Q$. Ví dụ $x^4-10x^2+1$ bất khả quy trên $\mathbf Q$ với trường phân rã $\mathbf Q(\sqrt 2,\sqrt 3)$; nhưng trên $\mathbf F_p$, nếu $2$ và $3$ đều không là thặng dư bậc hai, thì nó phân tích thành tích hai đa thức bất khả quy, tức trong $\mathbf F_p(\sqrt 2)$ đã có $\sqrt 3$.

### Mở rộng trên vành

Như mục trước, mở rộng trường có hạn chế. Với Fibonacci, chỉ dùng mở rộng trường giải được khi modulo là nguyên tố và $5$ không là thặng dư bậc hai. Nhưng nếu không cần phép chia, ta có thể mở rộng trên vành[^ring-extension]. Phần này minh họa với Fibonacci modulo $m$ bất kỳ.

Cho $m$ bất kỳ, $f(n)$ là Fibonacci. Cần tính $f(n)\bmod m$. Về nguyên tắc, tính trong $\mathbf Z/m\mathbf Z$. Nhưng trên các modulo khác nhau, $x^2-x-1$ có thể khả quy hoặc có nghiệm bội, nên công thức tổng quát khác nhau. Ngoài ra, nếu $\mathbf Z/m\mathbf Z$ không là trường, phần tử trong mở rộng có thể không có nghịch đảo (ví dụ modulo $5$, mẫu $\sqrt 5$ là $0$). Dù có nhiều vấn đề, ta vẫn có thể tính hệ số tự do của

$$
(1-x)^n-x^n\mod{x^2-x-1}
$$

và lấy đó làm kết quả. So với công thức tổng quát ở trên, điều này hợp lý: tương đương tính

$$
f(n)=\frac{(1-\phi)^n-\phi^n}{1-2\phi}
$$

trong “mở rộng” $(\mathbf Z/m\mathbf Z)[x]/(x^2-x-1)$, với $\phi$ là nghiệm của $x^2-x-1$.

Dù không quá hiển nhiên, cách này là hợp lệ. Trong $\mathbf Q(\phi)\cong\mathbf Q[x]/(x^2-x-1)$, công thức tổng quát đúng, nên trong $\mathbf Q[x]$ có

$$
(1-x)^n-x^n \equiv f(n)(1-2x) \pmod{x^2-x-1}.
$$

Đây là đẳng thức hệ số nguyên nên đúng trong $\mathbf Z[x]$. Lấy hệ số modulo $m$ ta được đẳng thức trong $(\mathbf Z/m\mathbf Z)[x]$. Kết luận đúng với mọi $m$.

Nói chung, nếu một biểu thức tính được trong mở rộng của $\mathbf Q$, ta có thể khử mẫu để có kết luận trong $\mathbf Z[x]$, rồi lấy modulo $m$ để có kết luận trong $(\mathbf Z/m\mathbf Z)[x]$. Mấu chốt là bước khử mẫu không làm mất thông tin không thể phục hồi. Ví dụ Fibonacci, nếu dùng hệ số bậc một thay vì hệ số tự do, sẽ xuất hiện hệ số $2$; nếu $m$ chẵn thì $2$ không có nghịch đảo, không phục hồi được $f(n)$. Hoặc nếu dùng công thức có $(-\phi)^{-n}$, việc khử mẫu tạo ra nhân tử khó xử lý. Vì vậy, chọn cách biến đổi phù hợp là quan trọng.

### Thuật toán Cipolla

Đây là ví dụ điển hình dùng mở rộng trường hữu hạn. Với thặng dư bậc hai $a$ modulo $p\neq 2$, ta cần nghiệm $x^2\equiv a\pmod p$. Dù là bài toán trên $\mathbf F_p$, [thuật toán Cipolla](../number-theory/quad-residue.md#cipolla-算法) tính trên $\mathbf F_{p^2}$. Diễn giải theo ngôn ngữ trường:

Chọn $r$ sao cho $r^2-a$ là phi thặng dư bậc hai. Khi đó $x^2-(r^2-a)$ bất khả quy. Đặt $u=r^2-a$, xét mở rộng $\mathbf F_p(\sqrt u)$. Trong mở rộng bậc hai, Frobenius gửi $(r-\sqrt u)^p=r+\sqrt u$. Do đó $(r-\sqrt u)^{p+1}=(r+\sqrt u)(r-\sqrt u)=r^2-u=a$. Vậy căn bậc hai cần tìm là $(r-\sqrt u)^{(p+1)/2}$. Giá trị này nằm trong $\mathbf F_p$ vì trường phân rã của $x^2-a$ là $\mathbf F_p$.

## Bài tập

Cuối cùng, liệt kê một số bài áp dụng nội dung, lưu ý nhiều nội dung không phải trọng tâm thi.

-   Đa thức phân chia:
    -   [Luogu P1520 因式分解](https://www.luogu.com.cn/problem/P1520)
    -   [Gym102114C Call It What You Want](https://codeforces.com/gym/102114/problem/C)
-   Trường hữu hạn:
    -   [Luogu P3923 大学数学题](https://www.luogu.com.cn/problem/P3923)
    -   [[COTS 2021] 菜 Jelo](https://www.luogu.com.cn/problem/P11192)
    -   [CF1310F. Bad Cryptography](https://codeforces.com/problemset/problem/1310/F)
    -   [LOJ 178. 多项式求根](https://loj.ac/p/178)
-   Mở rộng trường:
    -   [[Oleksandr Kulkov Contest 2] Problem A. Square Root Partitioning](https://codeforces.com/gym/102354/problem/A)
    -   [CF1103E. Radix Sum](https://codeforces.com/problemset/problem/1103/E)

## Tài liệu tham khảo và chú thích

-   Dummitt, D.S. and Foote, R.M. (2004) Abstract Algebra. 3rd Edition, John Wiley & Sons, Inc.
-   [Milne, J.S. Fields and Galois Theory.](https://www.jmilne.org/math/CourseNotes/FT.pdf)
-   [Factorization of polynomials - Wikipedia](https://en.wikipedia.org/wiki/Factorization_of_polynomials)
-   [Factorization of polynomials over finite fields - Wikipedia](https://en.wikipedia.org/wiki/Factorization_of_polynomials_over_finite_fields)
-   [Cyclotomic Polynomial - Wikipedia](https://en.wikipedia.org/wiki/Cyclotomic_polynomial)
-   [Brett Porter's Notes on Cyclotomic Polynomials](https://www.whitman.edu/documents/academics/majors/mathematics/2015/Final%20Project%20-%20Porter%2C%20Brett.pdf)
-   [Jordan Bell's Notes on Cyclotomic Polynomials](https://jordanbell.info/LaTeX/mathematics/cyclotomic/cyclotomic.pdf)
-   [Michel Waldschmidt. An introduction to the theory of finite fields](https://webusers.imj-prg.fr/~michel.waldschmidt/articles/pdf/FiniteFields.pdf)
-   [Finite Field Arithmetic - Wikipedia](https://en.wikipedia.org/wiki/Finite_field_arithmetic)

[^subfield-one]: Vì phần tử đơn vị $1_E$ của $E$ thỏa $x^2-x=0$ trên $F$, mà phương trình này trong $F$ chỉ có nghiệm $0_F$ và $1_F$. Do định nghĩa trường yêu cầu $1_E\neq 0_E$, nên $1_E=1_F$ và $0_E=0_F$.

[^initial-object-ring]: Theo ngôn ngữ phạm trù, $\mathbf Z$ là [đối tượng khởi đầu](https://en.wikipedia.org/wiki/Initial_and_terminal_objects) trong phạm trù các vành có đơn vị.

[^polynomial-universal]: Nghiêm ngặt mà nói, đây là [tính chất vạn năng](https://en.wikipedia.org/wiki/Polynomial_ring#Polynomial_evaluation) của vành đa thức $R[x]$.

[^multi-poly-ring]: Đây là vành đa thức với vô hạn biến. Để định nghĩa, trước hết định nghĩa đơn thức: với tập biến $X$, đơn thức là các hàm $\alpha:X\rightarrow\mathbf N$ chỉ khác 0 trên hữu hạn biến, ký hiệu $x_{i_1}^{\alpha(i_i)}\cdots x_{i_k}^{\alpha(i_k)}$. Đa thức là tổ hợp tuyến tính hữu hạn các đơn thức. Các phép cộng và nhân cho cấu trúc vành. Với hữu hạn biến, định nghĩa này trùng với định nghĩa đệ quy trong [vành đa thức nhiều biến](./ring-theory.md#多元多项式环).

[^fundamental-algebra]: Dù tên là định lý cơ bản của đại số, kết luận này không hoàn toàn thuần đại số vì việc xây dựng $\mathbf R$ cần cấu trúc tô pô.

[^ddf]: Cách nói này chưa hoàn toàn chặt chẽ vì còn nhận được các ước bất khả quy bậc $d\mid n$. Tuy nhiên trong thuật toán, thường tách các bậc nhỏ trước, nên khi tách bậc $n$ thì các bậc nhỏ đã bị loại, vì vậy cách nói này có thể chấp nhận.

[^prim-poly]: Đừng nhầm với “đa thức nguyên thủy” trong lý thuyết đa thức (nghĩa là gcd của hệ số bằng 1).

[^list-prim-poly]: Ví dụ, [Hansen, T., & Mullen, G. L. (1992). Primitive polynomials over finite fields. Mathematics of computation, 59(200), 639-643](https://www.ams.org/journals/mcom/1992-59-200/S0025-5718-1992-1134730-7/S0025-5718-1992-1134730-7.pdf) có bảng tra.

[^fib-p5]: Khi $p=5$, phương trình đặc trưng $x^2-x-1=0$ có nghiệm kép $x=3$, nên trong $\mathbf F_5$ công thức là $f(n)=n3^{n-1}$.

[^ring-extension]: “Mở rộng trên vành” có hai nghĩa: [mở rộng nhóm](https://en.wikipedia.org/wiki/Algebra_extension) hoặc [mở rộng trường](https://en.wikipedia.org/wiki/Subring#Ring_extensions). Bài này dùng nghĩa thứ hai. Cụ thể, các mở rộng ở đây là [mở rộng nguyên](https://en.wikipedia.org/wiki/Integral_element#Integral_extensions) của vành giao hoán có đơn vị, là tổng quát hóa của mở rộng đại số trên trường.
