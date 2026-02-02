author: jifbt, billchenchina, Enter-tainer, Great-designer, iamtwz, ImpleLee, isdanni, Menci, ouuan, Tiphereth-A, warzone-oier, Xeonacid, c-forrest

Chương này sẽ giới thiệu ngắn gọn về các kiến thức cơ bản của đại số trừu tượng. Hiện tại, nội dung thi thuật toán không trực tiếp kiểm tra kiến thức đại số trừu tượng, nhưng trong mô tả thuật toán hoặc lời giải bài toán thường sẽ nhắc đến một số khái niệm cơ bản của các hàm trừu tượng. Điều này giúp những bạn nắm được các khái niệm cơ bản của đại số trừu tượng có thể hiểu nhanh hơn một số thuật toán. Vì thế, phần này không phải là kiến thức bắt buộc với mọi thí sinh, mà chỉ mang tính tham khảo cho những người quan tâm hoặc có thể hưởng lợi từ nó. Đồng thời, chương này sẽ tránh việc trình bày quá rộng hoặc quá sâu về đại số trừu tượng[^oi-wiki-not-wikipedia], mà tập trung vào các khái niệm nền tảng và phần liên hệ chặt chẽ nhất với các kiến thức khác của OI. Nếu muốn học hệ thống về đại số trừu tượng, bạn nên tham khảo các giáo trình chuyên ngành.

Để giúp người đọc hiểu rõ hơn về lợi ích của phần này, dưới đây là một số ví dụ trong thi đấu thuật toán có thể liên quan tới đại số trừu tượng:

-   Nhiều định lý trong số học và đa thức là các trường hợp riêng của kết quả trong đại số trừu tượng;
-   Trong cấu trúc dữ liệu, các cấu trúc như [segment tree](../../ds/seg.md) có thể bảo trì thông tin của một nửa nhóm có đơn vị (monoid), và nhiều bài DP có quan hệ truy hồi có thể trừu tượng hóa thành cấu trúc monoid như vậy;
-   Trong tổ hợp, phát biểu và chứng minh chặt chẽ của [Nguyên lý đếm Pólya](../combinatorics/polya.md) cần dùng đến các khái niệm liên quan đến lý thuyết nhóm.

Dựa trên đó, chương này tập trung giới thiệu những kiến thức cơ bản không thể bỏ qua và phần liên quan trực tiếp đến các ứng dụng trên. Bắt đầu, bài viết sẽ giới thiệu các khái niệm cơ bản về nhóm, vành, trường.

## Nhóm

Định nghĩa nhóm như sau.

???+ abstract "Nhóm"
    Cho $G$ là một tập không rỗng, có phép toán hai ngôi $\cdot:G\times G\rightarrow G$. Nếu thỏa mãn các tính chất sau, thì $(G,\cdot)$ được gọi là **nhóm** (group):
    
    1.  Tính kết hợp (associative property): với mọi $a,b,c\in G$ có $a\cdot(b\cdot c)=(a\cdot b)\cdot c$;
    2.  Tồn tại phần tử đơn vị: tồn tại $e\in G$ sao cho với mọi $a\in G$ đều có $a\cdot e = e\cdot a = a$. Ở đây, $e$ được gọi là **phần tử đơn vị** (identity element), cũng gọi là phần tử trung hòa;
    3.  Tồn tại phần tử nghịch đảo: với mọi $a\in G$ tồn tại $b\in G$ sao cho $a\cdot b=b\cdot a=e$. Ở đây, $b$ được gọi là **nghịch đảo** (inverse element) của $a$.

??? info "Về điều kiện đóng trong định nghĩa"
    Phép toán hai ngôi đã ngầm bao hàm điều kiện đóng, tức là với mọi $a,b\in G$ thì $a\cdot b\in G$. Có một số tài liệu tách điều kiện này ra riêng.

???+ note "Tính chất cơ bản của nhóm"
    Với nhóm $(G,\cdot)$, các tính chất sau luôn đúng:
    
    1.  Với mọi dãy hữu hạn $\{g_i\}_{i=1}^k\subseteq G$, kết quả của tích $g_1\cdot g_2\cdot\cdots\cdot g_k$ không phụ thuộc vào cách đặt ngoặc;
    2.  Phần tử đơn vị $e$ là duy nhất;
    3.  Nghịch đảo $a^{-1}$ của mỗi phần tử $a\in G$ là duy nhất;
    4.  Luật triệt tiêu (cancellation law): với mọi $a,b,c\in G$, nếu $a\cdot c=b\cdot c$ hoặc $c\cdot a=c\cdot b$ thì $a=b$.

Nhóm xuất hiện rất phổ biến. Một cách trực quan, mọi phép biến đổi bảo toàn cấu trúc đều tự nhiên tạo thành một nhóm. Dưới đây là một vài loại nhóm thường gặp.

???+ example "Ví dụ về nhóm"
    -   **Nhóm đối xứng** (symmetric group): tập tất cả các [hoán vị](../permutation.md) của tập $M$, tức là các song ánh từ $M$ tới chính $M$, tạo thành nhóm $S_M$ dưới phép hợp thành. Phần tử đơn vị là phép đồng nhất, nghịch đảo là ánh xạ ngược (song ánh luôn có nghịch đảo). Nếu $M$ hữu hạn kích thước $n$ thì thường viết $S_n$, gọi là nhóm đối xứng bậc $n$.
    -   Nhóm đối xứng không gian (symmetry group): với một hình học, tập tất cả các phép biến đổi khiến hình trùng với chính nó cũng tạo thành nhóm dưới phép hợp thành. Điều này mô tả tính đối xứng không gian của hình. Ví dụ cụ thể xem [các nhóm đối xứng không gian thường gặp](../combinatorics/polya.md#常见空间对称群).
    -   Nhóm cộng các số nguyên: tập số nguyên $\mathbf Z$ với phép cộng $+$ tạo thành nhóm $(\mathbf Z,+)$. Phần tử đơn vị là $0$, nghịch đảo là số đối.
    -   Nhóm nhân các lớp đồng dư modulo $n$ (multiplicative group of integers modulo $n$): với mô-đun $n$, các [lớp đồng dư](../number-theory/basic.md#同余类与剩余系) ứng với các số nguyên nguyên tố cùng nhau với $n$ tạo thành nhóm $((\mathbf Z/n\mathbf Z)^\times,\times)$ dưới phép nhân. Phần tử đơn vị là $\bar 1$, nghịch đảo là [nghịch đảo nhân modulo $n$](../number-theory/inverse.md) (lớp đồng dư tương ứng), tính tồn tại do [định lý Bézout](../number-theory/bezouts.md) đảm bảo. Phân tích cấu trúc xem [nhóm nhân modulo $n$](./ring-theory.md#应用整数同余类的乘法群).
    -   Nhóm tuyến tính tổng quát (general linear group): tất cả các ma trận vuông khả nghịch kích thước $n$ trên trường $F$ tạo thành nhóm $GL_n(F)$ dưới phép nhân. Phần tử đơn vị là ma trận đơn vị, nghịch đảo là ma trận nghịch đảo.

Để hiểu rõ định nghĩa nhóm, có thể đối chiếu với một vài ví dụ không phải là nhóm.

???+ example "Ví dụ không phải nhóm"
    -   Tập tất cả các ánh xạ từ $M$ tới $M$ (không nhất thiết song ánh) không tạo thành nhóm, vì các ánh xạ không song ánh không có nghịch đảo.
    -   Các số nguyên dưới phép nhân không tạo thành nhóm, vì $2$ không có nghịch đảo nhân trong $\mathbf Z$.
    -   Các số nguyên dương dưới phép cộng cũng không tạo thành nhóm, vì không có phần tử đơn vị của phép cộng.
    -   Các lớp đồng dư khác $0$ modulo $n$ thường không tạo thành nhóm dưới phép nhân. Ví dụ trong $(\mathbf Z/6\mathbf Z)\setminus\{\overline 0\}$, ta có $\overline 2\times\overline 3=\overline 0$ không thuộc tập, nghĩa là phép nhân không đóng (hoặc nói là không xác định tốt) trên tập này.

Đôi khi cũng cần xét các cấu trúc “kém hoàn chỉnh” hơn. Vì vậy có các khái niệm dưới đây, rộng hơn nhóm.

???+ abstract "Nửa nhóm"
    Với tập không rỗng $G$ và phép toán hai ngôi $\cdot$ trên đó, nếu phép toán thỏa mãn tính kết hợp thì $(G,\cdot)$ được gọi là **nửa nhóm** (semigroup).

???+ abstract "Monoid"
    Với nửa nhóm $(G,\cdot)$, nếu còn có phần tử đơn vị thì $(G,\cdot)$ được gọi là **monoid**.

???+ example "Ví dụ về monoid và nửa nhóm"
    Trong các ví dụ ở trên, $(\mathbf N_+,+)$ là nửa nhóm, còn $(\mathbf Z,\times)$ là monoid.

Cuối cùng, nhiều nhóm quen thuộc ngoài tính kết hợp còn thỏa mãn tính giao hoán. Các nhóm này có cấu trúc đơn giản hơn, gọi là nhóm Abel.

???+ abstract "Nhóm Abel"
    Với nhóm $(G,\cdot)$, nếu phép toán $\cdot$ còn thỏa mãn tính giao hoán (commutative property), tức là với mọi $a,b\in G$ đều có $a\cdot b=b\cdot a$, thì $(G,\cdot)$ được gọi là **nhóm Abel** (Abelian group) hay **nhóm giao hoán** (commutative group).

???+ example "Ví dụ về nhóm Abel và không Abel"
    -   Nhóm cộng các số nguyên $(\mathbf Z,+)$ là nhóm Abel.
    -   Với $n\ge3$, nhóm đối xứng $S_n$ không phải nhóm Abel.

Đó là các định nghĩa cơ bản của lý thuyết nhóm. Nhiều nội dung hơn có thể xem tại [lý thuyết nhóm](./group-theory.md) hoặc các sách chuyên khảo.

## Vành

Định nghĩa vành như sau.

???+ abstract "Vành"
    Với tập không rỗng $R$ và hai phép toán hai ngôi $+:R\times R\rightarrow R$ và $\cdot:R\times R\rightarrow R$, nếu thỏa mãn:
    
    1.  $(R,+)$ là nhóm Abel, phần tử đơn vị ký hiệu $0$, nghịch đảo của $a\in R$ dưới phép $+$ ký hiệu $-a$;
    2.  $(R,\cdot)$ là nửa nhóm, tức là $\cdot$ kết hợp;
    3.  Tính phân phối (distributive property): với mọi $a,b,c\in R$,
        $a\cdot(b+c)=a\cdot b+a\cdot c$ và $(a+b)\cdot c=a\cdot c+b\cdot c$;
    
    thì $(R,+,\cdot)$ được gọi là **vành** (ring).

Để thuận tiện, hai phép toán $+$ và $\cdot$ thường gọi là phép cộng và phép nhân của vành; phần tử đơn vị của cộng gọi là **phần tử không** (zero), phần tử đơn vị của nhân (nếu có) gọi là **phần tử đơn vị** (identity). Cần tránh nhầm lẫn với cộng/nhân cụ thể trên tập số và số $0,1$.

??? info "Về việc có yêu cầu phần tử đơn vị nhân hay không"
    Có một số định nghĩa yêu cầu vành phải có phần tử đơn vị nhân; khi đó, các cấu trúc không có phần tử đơn vị nhân được gọi là **pseudo-ring** (rng). Cần tùy văn cảnh để hiểu. Wikipedia dùng định nghĩa này[^ring-wiki].

Cấu trúc cộng của vành khá đơn giản, nhưng cấu trúc nhân còn rất thô sơ. Vì thế, nếu tương tự như nhóm, ta có thể đặt thêm điều kiện lên phép nhân để có các khái niệm sau.

???+ abstract "Vành có đơn vị"
    Với vành $(R,+,\cdot)$, nếu nó có phần tử đơn vị nhân, ký hiệu $1$, thì gọi là **vành có đơn vị** (ring with identity).

???+ abstract "Vành chia"
    Với vành có đơn vị khác $0$ $(R,+,\cdot)$, nếu mọi phần tử khác $0$ đều có nghịch đảo nhân (ký hiệu $a^{-1}$), thì gọi là **vành chia** (division ring).

???+ abstract "Vành giao hoán"
    Với vành $(R,+,\cdot)$, nếu phép nhân giao hoán thì gọi là **vành giao hoán** (commutative ring).

Một điểm thú vị trong định nghĩa vành chia là $0$ được xem như phần tử đặc biệt của phép nhân, bởi $0=0\cdot a=a\cdot 0$[^zero-multiplication]. Do đó $0$ không thể có nghịch đảo nhân, trừ khi chính nó là đơn vị nhân; trường hợp này chỉ xảy ra trong vành không (xem ví dụ dưới đây).

Điều này gợi ý rằng khi hiểu cấu trúc nhân của một vành, cần loại bỏ ảnh hưởng của phần tử $0$, tức là xét $R\setminus\{0\}$. Từ đó có định nghĩa sau.

???+ abstract "Ước không"
    Với vành $(R,+,\cdot)$, nếu tồn tại $b\in R$ và $b\ne 0$ sao cho $a\cdot b=0$ hoặc $b\cdot a=0$, thì phần tử khác $0$ là $a$ gọi là **ước không** (zero divisor).

???+ abstract "Phần tử khả nghịch (đơn vị)"
    Với vành $(R,+,\cdot)$, nếu phần tử $a$ có nghịch đảo nhân, tức là tồn tại $b\in R$ sao cho $a\cdot b=b\cdot a=1$, thì $a$ gọi là **phần tử khả nghịch**, hay **đơn vị** (unit).

???+ warning "“Đơn vị” và “phần tử đơn vị”"
    Xin đừng nhầm hai khái niệm này. Để tránh nhầm, trong phần đại số trừu tượng sẽ dùng tên “phần tử khả nghịch” thay cho “đơn vị”.

Ước không không thể là phần tử khả nghịch, và phần tử khả nghịch không thể là ước không. Tuy nhiên, một phần tử khác $0$ có thể vừa không là ước không, vừa không là phần tử khả nghịch.

Nếu một vành không có ước không, nghĩa là tập các phần tử khác $0$ đóng dưới phép nhân, tức $(R\setminus\{0\},\cdot)$ là nửa nhóm. Nếu còn yêu cầu nó là monoid giao hoán thì thu được định nghĩa của miền nguyên.

???+ abstract "Miền nguyên"
    Với vành khác $0$ $(R,+,\cdot)$, nếu là vành giao hoán, có đơn vị nhân, và không có ước không, thì gọi là **miền nguyên** (integral domain).

Dù trong miền nguyên các phần tử không nhất thiết có nghịch đảo, nhưng tính không có ước không đủ để suy ra luật triệt tiêu.

???+ note "Luật triệt tiêu trong miền nguyên"
    Cho miền nguyên $R$ và các phần tử $a,b,c\in R$ với $a\neq 0$. Nếu $ab=ac$ thì $b=c$.

Với một vành có đơn vị, nếu chỉ xét các phần tử khả nghịch, ta được một nhóm. Đây gọi là nhóm nhân (hay nhóm đơn vị) của vành.

???+ abstract "Nhóm nhân (nhóm đơn vị)"
    Với vành có đơn vị $(R,+,\cdot)$, đặt $R^\times$ là tập các phần tử khả nghịch của $R$. Khi đó $(R^\times,\cdot)$ là nhóm, gọi là **nhóm nhân** (multiplicative group) hoặc **nhóm đơn vị** (unit group) của $R$.

Một vài ví dụ đơn giản về vành:

???+ example "Ví dụ về vành"
    -   Vành không (zero ring): tập $\{0\}$ với phép cộng và nhân thông thường tạo thành vành, gọi là vành không. Đây là vành duy nhất chỉ có một phần tử, cũng là vành duy nhất có phần tử đơn vị cộng và nhân trùng nhau.
    -   Vành số nguyên: tập $\mathbf Z$ với phép cộng và nhân thông thường tạo thành vành $(\mathbf Z,+,\times)$. Thực ra đây là miền nguyên, nhưng không phải vành chia.
    -   Vành đa thức: với một vành $R$, có thể định nghĩa [vành đa thức](./ring-theory.md#多项式环) $R[x]$. Nếu $R$ là miền nguyên thì $R[x]$ cũng là miền nguyên.
    -   Quaternion: tương tự số phức, xét tập $\mathbf H=\{a+b\mathrm{i}+c\mathrm{j}+d\mathrm{k}:a,b,c,d\in\mathbf R\}$ và định nghĩa phép cộng, nhân với quy tắc
    
        $$
        \mathrm{i}^2=\mathrm{j}^2=\mathrm{k}^2=-1,\ \mathrm{i}\mathrm{j}=-\mathrm{j}\mathrm{i}=\mathrm{k},\ \mathrm{j}\mathrm{k}=-\mathrm{k}\mathrm{j}=\mathrm{i},\ \mathrm{k}\mathrm{i}=-\mathrm{i}\mathrm{k}=\mathrm{j}.
        $$
    
        Khi đó $\mathbf H$ là một vành, và là vành chia không giao hoán.
    -   Tập con $2\mathbf Z$ của số nguyên, với phép cộng và nhân thông thường, tạo thành một vành. Nó là vành giao hoán, không có ước không, nhưng không có đơn vị.
    -   Các lớp đồng dư modulo $n$ $\mathbf Z/n\mathbf Z$ với phép cộng và nhân của lớp đồng dư tạo thành một vành, là vành giao hoán có đơn vị ($\bar 1$). Vành này có ước không khi và chỉ khi $n$ là hợp số. Do đó, nếu $n$ là số nguyên tố thì $(\mathbf Z/n\mathbf Z,+,\times)$ là miền nguyên; hơn nữa khi đó nó là vành chia, tức là một trường. Nhóm nhân $((\mathbf Z/n\mathbf Z)^\times,\times)$ chính là nhóm nhân modulo $n$.
    -   Vành ma trận: tất cả ma trận vuông $n$ chiều trên vành $R$ tạo thành vành $M_n(R)$ dưới phép cộng và nhân ma trận. Thường vành này có ước không và không giao hoán.
    -   Với một tập $A$, xét tập lũy thừa $\mathcal P(A)$. Nếu định nghĩa hiệu đối xứng $\triangle$ là phép cộng và giao $\cap$ là phép nhân thì $(\mathcal P(A),\triangle,\cap)$ là vành. Nói chung, vành này có đơn vị, có ước không, và là giao hoán.

Dĩ nhiên, nghiên cứu cấu trúc của vành còn nhiều hơn thế; xem thêm [lý thuyết vành](./ring-theory.md) hoặc các sách chuyên khảo.

## Trường

Trường là cấu trúc đại số mạnh hơn vành. Cụ thể, trường là vành chia giao hoán. Dưới đây là định nghĩa đầy đủ.

???+ abstract "Trường"
    Cho tập không rỗng $F$ và hai phép toán hai ngôi $+:F\times F\rightarrow F$ và $\cdot:F\times F\rightarrow F$. Nếu:
    
    1.  $(F,+)$ là nhóm Abel, phần tử đơn vị ký hiệu $0$, nghịch đảo của $a\in F$ dưới $+$ ký hiệu $-a$;
    2.  $(F\setminus\{0\},\cdot)$ là nhóm Abel, phần tử đơn vị ký hiệu $1$, nghịch đảo của $a\in F\setminus\{0\}$ dưới $\cdot$ ký hiệu $a^{-1}$;
    
    thì $(F,+,\cdot)$ gọi là **trường** (field).

Nói cách khác, trường là cấu trúc đại số đóng dưới bốn phép cộng, trừ, nhân, chia.

Một số ví dụ về trường:

???+ example "Ví dụ về trường"
    -   Các trường số: tập số hữu tỉ $\mathbf Q$, số thực $\mathbf R$ và số phức $\mathbf C$ với phép cộng và nhân thông thường đều là trường.
    -   Trường hữu hạn (finite field): các lớp đồng dư modulo số nguyên tố $p$, tức $\mathbf Z/p\mathbf Z$, tạo thành trường. Ngoài ra còn có các trường hữu hạn khác, cấu trúc được xác định duy nhất bởi số phần tử, và số phần tử luôn là lũy thừa của số nguyên tố.
    -   **Trường phân thức** (fraction field): với miền nguyên $(R,+,\cdot)$, xét các phần tử dạng $ab^{-1}$. Nghiêm ngặt hơn, định nghĩa quan hệ tương đương trên $R\times(R\setminus\{0\})$: $(a_1,b_1)\sim(a_2,b_2)$ khi và chỉ khi $a_1b_2=a_2b_1$. Khi đó, tập $Q$ là tập các lớp tương đương $R\times(R\setminus\{0\})/\sim$, trong đó lớp của $(a,b)$ ký hiệu $ab^{-1}$. Định nghĩa phép toán
    
        $$
        \begin{aligned}
        a_1b_1^{-1}+a_2b_2^{-1} &= (a_1\cdot b_2+a_2\cdot b_1)(b_1\cdot b_2)^{-1},\\
        (a_1b_1^{-1})\cdot(a_2b_2^{-1}) &= (a_1\cdot a_2)(b_1\cdot b_2)^{-1}
        \end{aligned}
        $$
    
        thì $(Q,+,\cdot)$ là trường, gọi là trường phân thức của $R$. Ví dụ, trường hữu tỉ $(\mathbf Q,+,\times)$ là trường phân thức của vành số nguyên $(\mathbf Z,+,\times)$.
    -   **Trường bậc hai** (quadratic field): là trường thu được khi thêm $\sqrt d$ vào $\mathbf Q$, với $d\neq 0,1$ và không có ước bình phương. Xem thêm [trường bậc hai](../number-theory/quadratic.md).

So với vành, trường có cấu trúc cộng và nhân rất đơn giản. Vì vậy, cấu trúc của bản thân trường thường khá đơn giản, và nghiên cứu trường thường chuyển sang nghiên cứu các mở rộng trường và lý thuyết Galois. Trong thi đấu thuật toán, đôi khi cần tính toán trong mở rộng của trường hữu tỉ hoặc trường hữu hạn. Xem thêm [lý thuyết trường](./field-theory.md).

## Ứng dụng

Cuối cùng, lấy một bài toán làm ví dụ để thấy đối tượng đại số trừu tượng giúp phân tích bài toán cụ thể như thế nào.

???+ note "[【模板】"Dynamic DP"& 动态树分治（加强版）](https://www.luogu.com.cn/problem/P4751)"
    Cho cây có $n$ đỉnh với trọng số trên đỉnh, thực hiện $m$ lần cập nhật trọng số đỉnh. Sau mỗi lần cập nhật, hãy in ra tổng trọng số của tập độc lập có trọng số lớn nhất trên cây. Bài toán yêu cầu online.

???+ note "Phân tích ý tưởng"
    Bài này là mẫu Dynamic DP. Một cài đặt đúng độ phức tạp cần dùng [cây cân bằng toàn cục](../../ds/global-bst.md), ví dụ mã ở trang tương ứng. Ở đây chỉ phân tích quá trình mô hình hóa, không đi sâu vào xử lý cấu trúc cây.
    
    Để làm rõ ý chính, ta tạm bỏ qua xử lý cây và xét bài DP trên một đường thẳng. Xét lần lượt các đỉnh $[1,n]$. Với đỉnh $i$ có thể chọn ($1$) hoặc không chọn ($0$). Gọi $f_{i,1}$ và $f_{i,0}$ là lời giải tốt nhất của bài toán con $[1,i]$ tương ứng. Khi đó có phương trình DP
    
    $$
    \begin{aligned}
    f_{i,1}&=w_{i}+f_{i-1,0},\\
    f_{i,0}&=\max\{f_{i-1,1},f_{i-1,0}\}.
    \end{aligned}
    $$
    
    Giá trị khởi tạo là $(f_{0,1},f_{0,0})=(0,0)$, và đáp án cuối cùng là $\max\{f_{n,1},f_{n,0}\}$. Để biểu diễn ảnh hưởng của đỉnh $i$ tới kết quả cuối, lưu ý quan hệ truy hồi có thể viết
    
    $$
    (f_{i,1},f_{i,0})=g(f_{i-1,1},f_{i-1,0};w_i).
    $$
    
    Đây là chuỗi các ánh xạ từ $\mathbf R^2$ tới $\mathbf R^2$; mỗi ánh xạ biến $(f_{i-1,1},f_{i-1,0})$ thành $(f_{i,1},f_{i,0})$. Theo ngôn ngữ nhóm, các biến đổi này dưới phép hợp thành tạo thành một monoid, và đây là điều mà segment tree có thể bảo trì.
    
    Tuy nhiên, các biến đổi có tham số $g(\cdot;w_i)$ nếu không có cấu trúc đặc biệt thì một ánh xạ $\mathbf R^2\to\mathbf R^2$ không thể mô tả bằng dữ liệu hữu hạn. Ở đây cần một quan sát khác: nếu trên $\mathbf R\cup\{-\infty\}$, lấy $\max$ làm phép cộng và $+$ làm phép nhân, thì $\mathbf R\cup\{-\infty\}$ tạo thành một cấu trúc giống vành, trong đó $-\infty$ là đơn vị cộng và $0$ là đơn vị nhân. Nhưng nó không phải vành vì không phải phần tử nào cũng có nghịch đảo cộng. Cấu trúc này gọi là nửa vành nhiệt đới (tropical semiring).
    
    Trên nửa vành nhiệt đới $(R,\oplus,\otimes)$, có thể định nghĩa phép nhân ma trận. Với ma trận $m\times n$ $A=(a_{ij})$ và $n\times p$ $B=(b_{jk})$, định nghĩa tích $AB=(c_{ik})$ với
    
    $$
    c_{ik} = \bigoplus_{j=1}^n(b_{ij}\otimes c_{jk}) = \max_{1\le j\le n}\;(b_{ij}+c_{jk}).
    $$
    
    Với ký hiệu này, quan hệ truy hồi trên có thể xem là biến đổi tuyến tính trên nửa vành nhiệt đới và viết dưới dạng ma trận
    
    $$
    \left(\begin{matrix}f_{i,1}\\f_{i,0}\end{matrix}\right)
    =\left(\begin{matrix}-\infty&w_i\\0&0\end{matrix}\right)\left(\begin{matrix}f_{i-1,1}\\f_{i-1,0}\end{matrix}\right).
    $$
    
    Do đó, chỉ cần segment tree bảo trì tích các ma trận trên nửa vành nhiệt đới là có thể trả lời nhiều lần cập nhật cho bài toán DP trên đường.
    
    Quay lại phiên bản trên cây. Với đỉnh $i$, tập con $S(i)$ là các con của nó, thì
    
    $$
    \begin{aligned}
    f_{i,1}&=w_i+\sum_{j\in S(i)}f_{j,0},\\
    f_{i,0}&=\sum_{j\in S(i)}\max\{f_{j,1},f_{j,0}\}.
    \end{aligned}
    $$
    
    Trước hết, dùng heavy-light decomposition để chuyển về phiên bản trên đường. Gọi $h$ là con nặng của $i$ thì
    
    $$
    \begin{aligned}
    f_{i,1}&=w_i+f_{h,0}+g_{i,1},\\
    f_{i,0}&=\max\{f_{j,0},f_{j,1}\}+g_{i,0},
    \end{aligned}
    $$
    
    với
    
    $$
    \begin{aligned}
    g_{i,1}&=\sum_{j\in S(i),\ j\neq h}f_{j,0},\\
    g_{i,0}&=\sum_{j\in S(i),\ j\neq h}\max\{f_{j,1},f_{j,0}\}.
    \end{aligned}
    $$
    
    $g_{i,1},g_{i,0}$ tổng hợp đóng góp của các con nhẹ. Theo mô tả trên, các biến đổi này đều có thể viết dưới dạng ma trận trên nửa vành nhiệt đới, nên toàn bộ bài toán có thể bảo trì trên segment tree của đường sau khi HLD. Nhưng HLD+segment tree cho mỗi lần cập nhật là $O(\log^2 n)$, nên cần dùng cây cân bằng toàn cục để tối ưu xuống $O(\log n)$, hoặc cũng có thể dùng LCT.
    
    Nửa vành nhiệt đới và phép nhân ma trận như trên không hiếm. Nếu thay $\max$ bằng $\min$ thì nửa vành nhiệt đới dùng cho bài toán đường đi ngắn. Nếu ma trận $A$ kích thước $n$ mô tả đồ thị $n$ đỉnh với trọng số cạnh (ngắn nhất) thì phần tử $(i,j)$ của $A^k$ là khoảng cách ngắn nhất từ $i$ tới $j$ đi qua tối đa $k$ cạnh; đặc biệt $A^n$ là ma trận khoảng cách. Trong thực tế không tính lũy thừa ma trận trực tiếp mà dùng Floyd $O(n^3)$.

## Tài liệu tham khảo và chú thích

-   Dummitt, D.S. and Foote, R.M. (2004) Abstract Algebra. 3rd Edition, John Wiley & Sons, Inc.
-   [Tropical semiring - Wikipedia](https://en.wikipedia.org/wiki/Tropical_semiring)

[^oi-wiki-not-wikipedia]: Vì [OI Wiki không phải bách khoa toàn thư](../../intro/what-oi-wiki-is-not.md#oi-wiki-不是百科全书)．

[^ring-wiki]: [Ring（mathematics）- Wikipedia](https://en.wikipedia.org/wiki/Ring_%28mathematics%29)

[^zero-multiplication]: Suy ra từ $0\cdot a+0 = 0\cdot a = (0+0)\cdot a = 0\cdot a + 0\cdot a$, trong đó, dấu bằng thứ nhất và thứ hai là định nghĩa phần tử đơn vị cộng, dấu bằng thứ ba là tính phân phối, và suy ra cuối cùng là luật triệt tiêu của phép cộng. Vế còn lại của phép nhân tương tự.

[^semiring]: Nửa vành (semiring) là cấu trúc đại số nới lỏng yêu cầu phép cộng phải có nghịch đảo trong định nghĩa của vành có đơn vị, tức phép cộng là monoid giao hoán, phép nhân là monoid. Xem thêm [Wikipedia](https://en.wikipedia.org/wiki/Semiring)．
