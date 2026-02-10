Phần này giới thiệu kiến thức cơ bản về lý thuyết tính toán. Nội dung này không quá quan trọng trong OI (nhưng vẫn có chút tác dụng: nếu gặp một bài NP-hard, bạn có thể coi là không có lời giải đa thức), có thể dùng để mở rộng hiểu biết hoặc chuẩn bị cho việc học sau này.

Nhiều kết luận trong bài không chứng minh; nếu quan tâm có thể tự tra cứu chứng minh liên quan.

Kiến thức tiền đề: [độ phức tạp thời gian](../basic/complexity.md).

## Bài toán

### Ngôn ngữ

Một **bảng chữ cái (alphabet)** là một tập hữu hạn không rỗng, các phần tử trong đó gọi là **ký hiệu/ký tự (symbol)**.

Ký hiệu $\Sigma^\ast$ là tập các xâu tạo bởi nối không âm số lượng ký tự từ $\Sigma$. Một **ngôn ngữ (language)** trên bảng chữ cái $\Sigma$ là một tập con của $\Sigma^\ast$.

Lưu ý, “ngôn ngữ” ở đây là khái niệm trừu tượng; xâu ký tự thông thường là một ngôn ngữ, mọi đồ thị có hướng không chu trình cũng có thể là một ngôn ngữ (có thể thiết lập song ánh giữa chuỗi 01 và đồ thị có hướng, không cần biết chi tiết).

Do mọi ngôn ngữ đều có thể chuyển thành chuỗi 01, nên nếu không nói rõ thì $\Sigma=\{0, 1\}$.

### Bài toán quyết định

Bài toán quyết định là bài chỉ trả lời YES/NO; bản chất là xác định một xâu có thuộc một ngôn ngữ hay không, tức: $f:\Sigma^\ast\rightarrow\{0, 1\}, f(x)=1\iff x\in L$ là một bài toán quyết định với bảng chữ cái $\Sigma$ và ngôn ngữ $L$. Ví dụ: “xác định một đồ thị có hướng có chu trình hay không” là một bài toán quyết định.

Bài toán quyết định do tính đơn giản nên thường là đối tượng nghiên cứu của lý thuyết tính toán. Trong bài này, nếu không nói rõ, “bài toán” đều là “bài toán quyết định”, dù một số mệnh đề có thể mở rộng sang các bài toán khác.

Một ngôn ngữ cũng có thể đại diện cho “bài toán quyết định xem một xâu có thuộc ngôn ngữ đó hay không”, nên “ngôn ngữ” và “bài toán” có thể coi là đồng nghĩa.

### Bài toán chức năng

Bài toán chức năng không chỉ trả YES/NO mà có thể là một số hoặc dạng kết quả khác. Ví dụ: “tính tổng hai số” là một bài toán chức năng.

Mọi bài toán chức năng đều có thể chuyển thành bài toán quyết định, ví dụ “tính tổng hai số” thành “kiểm tra tổng hai số có bằng số thứ ba hay không”.

Bài toán quyết định cũng có thể chuyển thành bài toán chức năng: tính hàm chỉ thị của bài toán quyết định, tức hàm $f$ trong định nghĩa trên.

## Máy Turing

### Máy Turing xác định

Nếu không nói rõ, “máy Turing” thường chỉ “máy Turing xác định”, và bài này cũng vậy.

Máy Turing có nhiều định nghĩa khác nhau; ở đây chọn một định nghĩa, các định nghĩa khác thường tương đương về năng lực tính toán.

Máy Turing là một máy thao tác trên một dải băng vô hạn hai chiều, chia thành các ô, có trạng thái nội bộ và một đầu đọc/ghi có thể sửa nội dung và di chuyển trên băng.

Chính thức, máy Turing là một bộ bảy $M=\langle Q,\Gamma,b,\Sigma,\delta,q_0,F\rangle$, trong đó:

-   $Q$ là một **tập trạng thái** hữu hạn không rỗng;
-   $\Gamma$ là một **bảng chữ cái băng** hữu hạn không rỗng;
-   $b\in\Gamma$ là **ký tự trống**, là ký tự duy nhất có thể xuất hiện vô hạn lần trên băng trong quá trình tính toán;
-   $\Sigma\subseteq(\Gamma\setminus\{b\})$ là **tập ký hiệu đầu vào**, có thể xuất hiện trên băng đầu vào ban đầu;
-   $q_0\in Q$ là **trạng thái khởi đầu**;
-   $F\subseteq Q$ là **tập trạng thái chấp nhận**; nếu máy dừng ở một trạng thái chấp nhận thì nội dung băng đầu vào được **chấp nhận**.
-   $\delta :(Q\setminus F)\times \Gamma \not \to Q\times \Gamma \times \{L,R\}$ là **hàm chuyển** dạng partial function (chỉ xác định trên một phần miền xác định). Nếu $\delta$ không xác định tại trạng thái hiện tại, máy dừng.

Máy bắt đầu từ trạng thái khởi đầu ở vị trí đầu băng; mỗi bước dựa vào trạng thái nội bộ $x$ và ký tự hiện tại $y$: nếu $\delta(x, y)$ không xác định thì dừng; nếu $\delta(x, y)=(a, b, c)$ thì chuyển trạng thái nội bộ sang $a$, ghi ký tự $b$ vào ô hiện tại, và nếu $c=L$ thì dịch trái một ô, $c=R$ thì dịch phải một ô.

Thực tế không cần biết chi tiết cơ chế, chỉ cần hiểu trực quan.

Đầu ra của máy $M$ trên input $x$ ký hiệu $M(x)$ ($M(x)=1$ khi và chỉ khi $M$ chấp nhận $x$, $M(x)=0$ khi và chỉ khi $M$ dừng hữu hạn bước và không chấp nhận $x$). Cũng có thể có nhiều tham số, ngăn cách bằng dấu phẩy, khi triển khai có thể thêm ký tự vào bảng chữ cái để làm dấu phẩy.

Chênh lệch độ phức tạp thời gian giữa máy Turing và máy tính kiểu Von Neumann chỉ ở đa thức, nên nghiên cứu lớp độ phức tạp có thể dùng máy Turing làm mô hình.

### Máy Turing không xác định

Máy Turing không xác định khác máy xác định ở chỗ: máy xác định mỗi bước chỉ chuyển đến một trạng thái, còn máy không xác định có thể “đồng thời” chuyển đến nhiều trạng thái, tạo nhiều nhánh tính toán song song; chỉ cần một nhánh dừng ở trạng thái chấp nhận thì máy chấp nhận input.

Thực tế, mọi máy xác định đều có thể mô phỏng máy không xác định bằng kiểu tìm kiếm tăng độ sâu, với thời gian mũ để mô phỏng hành vi thời gian đa thức.

Trong đời sống, máy xác định tương đương CPU một lõi (xử lý tuần tự), còn máy không xác định tương đương bộ xử lý đa lõi lý tưởng có song song vô hạn.

### Máy Turing đa băng

Máy Turing chuẩn chỉ có một băng, nhưng để tiện, ta nghiên cứu máy Turing đa băng. Với máy $k$ băng, có một băng chỉ đọc làm đầu vào, còn lại $k-1$ băng đọc/ghi, và trong đó có một băng dùng làm đầu ra.

Số băng phải hữu hạn.

Không gian máy dùng là số ô mà đầu đọc trên các băng (trừ băng đầu vào) đã truy cập.

### Mã hóa máy Turing

Máy Turing có thể được mã hóa thành số tự nhiên; tồn tại hàm toàn ánh $f:\mathbb{N}\to\mathbb{M}$ sao cho mỗi số tự nhiên ứng với một máy Turing và mỗi máy có vô số mã. Do đó, tập các máy Turing có thể là một ngôn ngữ.

Ký hiệu máy Turing được mã bởi số $\alpha$ là $M_{\alpha}$.

### Máy Turing phổ dụng

Tồn tại máy Turing $\mathcal U$ sao cho:

1.  Nếu $M_{\alpha}$ dừng hữu hạn thời gian với input $x$ thì $\mathcal{U}(x, \alpha)=M_{\alpha}(x)$, ngược lại $\mathcal{U}(x, \alpha)$ không dừng hữu hạn;
2.  Nếu với mọi $x\in\{0, 1\}^\ast$, $M_\alpha$ dừng trong $T(|x|)$, thì với mọi $x\in\{0, 1\}^\ast$, $\mathcal{U}(x, \alpha)$ dừng trong $O(T(|x|)\log T(|x|))$.

Tức là: tồn tại máy Turing phổ dụng có thể mô phỏng mọi máy Turing, và thời gian chỉ chậm hơn bội số log của thời gian máy được mô phỏng.

## Tính được

### Bài toán không tính được

Với một bài toán quyết định, nếu tồn tại máy Turing luôn dừng hữu hạn và phân định đúng, thì bài toán là **tính được theo Turing**; nếu không thì là **không tính được theo Turing**.

Vì máy Turing có thể mã hóa bằng số tự nhiên nên số máy Turing là đếm được vô hạn, còn số ngôn ngữ (tập xâu nhị phân) là không đếm được; mỗi máy Turing tối đa chỉ quyết định một ngôn ngữ, nên chắc chắn tồn tại bài toán không tính được.

### Bài toán dừng

Bài toán dừng là bài toán kinh điển không tính được: cho $\alpha$ và $x$, quyết định $M_{\alpha}$ có dừng hữu hạn với input $x$ hay không.

??? note "Chứng minh bài toán dừng là không tính được"
    Định nghĩa hàm $\mathsf{UC}:\{0,1\}^\ast\to\{0,1\}$ bởi:
    
    $$
    \mathsf{UC}(\alpha)=\begin{cases}0&M_\alpha(\alpha)=1\\1&\text{otherwise}\end{cases}
    $$
    
    Ta chứng minh $\mathsf{UC}$ là không tính được:
    
    Giả sử tồn tại máy $M_{\beta}$ tính được $\mathsf{UC}$, thì theo định nghĩa $\mathsf{UC}(\beta)=1\iff M_\beta(\beta)\neq 1$, đồng thời do $M_{\beta}$ tính được $\mathsf{UC}$ nên $M_{\beta}(\beta)=\mathsf{UC}(\beta)$, mâu thuẫn. Suy ra không tồn tại máy tính $\mathsf{UC}$.
    
    Giả sử có máy $M_{\mathsf{HALT}}$ giải bài toán dừng, với $M_{\mathsf{HALT}}(x,\alpha)$ trả lời $M_\alpha$ có dừng hay không. Ta xây máy $M_{\mathsf{UC}}$ tính $\mathsf{UC}$:
    
    $M_\mathsf{UC}$ gọi $M_\mathsf{HALT}(α,α)$, nếu ra $0$ thì $M_\mathsf{UC}(α)=1$; nếu không, dùng máy phổ dụng để mô phỏng và tính kết quả.
    
    Vì $\mathsf{UC}$ không tính được, nên $M_\mathsf{HALT}$ không tồn tại, tức bài toán dừng là không tính được.

## Luận đề Church–Turing

Luận đề Church–Turing nói rằng nếu một lớp bài toán có phương pháp hiệu quả để giải, thì lớp bài toán đó có thể được giải bởi một máy Turing.

“Phương pháp hiệu quả” phải thỏa:

1.  Gồm hữu hạn chỉ dẫn rõ ràng;
2.  Khi dùng để giải một bài trong lớp này, phương pháp phải dừng hữu hạn và trả lời đúng.

Luận đề này chưa được chứng minh, nhưng là tiên đề cơ bản của lý thuyết tính toán.

## Các lớp độ phức tạp

Có nhiều lớp độ phức tạp; bài này chỉ giới thiệu một phần.

### R và RE

Với ngôn ngữ $L$ và máy $M$, nếu $M$ dừng hữu hạn với mọi input, và $M(x)=1\iff x\in L$, thì $M$ **quyết định** $L$.

Nếu với mọi input thuộc $L$ thì $M$ dừng hữu hạn và $M(x)=1\iff x\in L$, thì $M$ **nhận diện** $L$.

Lớp $\mathsf R$ gồm các ngôn ngữ có thể được một máy Turing quyết định, tức mọi ngôn ngữ tính được.

Lớp $\mathsf{RE}$ gồm các ngôn ngữ có thể được một máy Turing nhận diện, còn gọi là ngôn ngữ đệ quy liệt kê.

Từ định nghĩa có $\mathsf{R}\subseteq\mathsf{RE}$.

### DTIME

Nếu có máy Turing xác định quyết định được một ngôn ngữ, và với mọi input $x$ dừng trong $O(f(|x|))$, thì ngôn ngữ đó thuộc $\mathsf{DTIME}(f(n))$.

### P

Lớp $\mathsf P$ là các bài toán quyết định giải được bằng máy Turing xác định trong thời gian đa thức:

$$
\mathsf{P}=\bigcup\limits_{k\in\mathbb{N}}\mathsf{DTIME}(n^k)
$$

Quy hoạch tuyến tính, tính gcd, phiên bản quyết định của matching cực đại đều thuộc $\mathsf P$.

### EXPTIME

Lớp $\mathsf{EXPTIME}$ là các bài quyết định giải được trong thời gian mũ:

$$
\mathsf{EXPTIME}=\bigcup\limits_{k\in\mathbb{N}}\mathsf{DTIME}(2^{n^k})
$$

Phiên bản yếu của bài toán dừng: cho mã máy và một số nguyên dương $k$, hỏi máy dừng trong $k$ bước không, là bài $\mathsf{EXPTIME}$, vì cần $O(k)$ thời gian, trong khi $k$ mã hóa bằng chuỗi nhị phân dài $O(\log k)$.

### NTIME

Nếu có máy Turing không xác định quyết định được ngôn ngữ và dừng trong $O(f(|x|))$, thì ngôn ngữ thuộc $\mathsf{NTIME}(f(n))$.

### NP

Lớp $\mathsf{NP}$ là các bài quyết định giải được bằng máy Turing không xác định trong thời gian đa thức:

$$
\mathsf{NP}=\bigcup\limits_{k\in\mathbb{N}}\mathsf{NTIME}(n^k)
$$

Mọi bài trong $\mathsf P$ đều thuộc $\mathsf{NP}$.

#### NP-hard

Nếu mọi bài toán $\mathsf{NP}$ quy về đa thức tới bài $H$, thì $H$ là NP-hard.

Tức là nếu giải được NP-hard trong một đơn vị thời gian thì mọi $\mathsf{NP}$ đều giải được trong thời gian đa thức.

#### NP-complete

Nếu một bài vừa thuộc $\mathsf{NP}$ vừa NP-hard thì là NP-complete (NPC).

Ví dụ kinh điển: phiên bản quyết định của TSP, independent set, vertex cover, longest path, 0-1 integer programming, set cover, graph coloring, knapsack, 3D matching, max cut.

Phiên bản chức năng của NPC thường là NP-hard. Ví dụ “kiểm tra đồ thị có clique size $k$” là NPC; còn “tìm clique lớn nhất” không phải NPC nhưng vẫn NP-hard.

Tương tự, các lớp khác cũng có “XX-complete”, như EXPTIME-complete.

#### co-NP

Một bài thuộc $\mathsf{co-NP}$ khi và chỉ khi bài đối ngẫu thuộc $\mathsf{NP}$. Nếu hiểu “bài toán” là “ngôn ngữ” thì “đối ngẫu” là phần bù của tập.

Ví dụ: “cho $n$ tập con, chọn $k$ tập có phủ hết tập gốc không” là NPC, còn bài đối ngẫu “chọn bất kỳ $k$ tập nào cũng không phủ hết” là $\mathsf{co-NP}$. Nếu bài thứ nhất trả lời “có” thì tương đương có phản ví dụ cho bài thứ hai, nên bài thứ hai trả lời “không”.

#### NP-intermediate

Nếu một bài thuộc $\mathsf{NP}$ nhưng không thuộc $\mathsf{P}$ cũng không NP-complete, thì gọi là NP-intermediate.

Hiện nay người ta cho rằng bài toán đẳng cấu đồ thị, log rời rạc và phân tích thừa số có thể là NP-intermediate.

Định lý Ladner nói rằng nếu $\mathsf{P}\ne\mathsf{NP}$ thì tồn tại bài NP-intermediate.

### NEXPTIME

Lớp $\mathsf{NEXPTIME}$ là các bài quyết định giải được bằng máy không xác định trong thời gian mũ:

$$
\mathsf{NEXPTIME}=\bigcup\limits_{k\in\mathbb{N}}\mathsf{NTIME}(2^{n^k})
$$

### #P

Lớp $\mathsf{\#P}$ không phải bài quyết định, mà là bài đếm số nghiệm của các bài $\mathsf{NP}$: đếm số nhánh chấp nhận của một máy Turing không xác định chạy đa thức. 

Đếm số matching hoặc perfect matching của đồ thị thường hay hai phía đều là #P-complete; phiên bản quyết định tương ứng là “đồ thị có (perfect) matching hay không”.

### DSPACE

Nếu có máy Turing xác định có thể quyết định một ngôn ngữ trong không gian $O(f(|x|))$ thì ngôn ngữ thuộc $\mathsf{DSPACE}(f(n))$.

-   $\mathsf{REG}=\mathsf{DSPACE}(O(1))$ là ngôn ngữ chính quy.
-   $\mathsf{L}=\mathsf{DSPACE}(O(\log n))$ (không tính không gian của đầu vào).
-   $\mathsf{PSPACE}=\bigcup\limits_{k\in\mathbb N}\mathsf{DSPACE}(n^k)$
-   $\mathsf{EXPSPACE}=\bigcup\limits_{k\in\mathbb N}\mathsf{DSPACE}(2^{n^k})$

### NSPACE

Nếu có máy Turing không xác định quyết định trong không gian $O(f(|x|))$ thì ngôn ngữ thuộc $\mathsf{NSPACE}(f(n))$.

-   $\mathsf{REG}=\mathsf{DSPACE}(O(1))=\mathsf{NSPACE}(O(1))$
-   $\mathsf{NL}=\mathsf{NSPACE}(O(\log n))$
-   $\mathsf{CSL}=\mathsf{NSPACE}(O(n))$ (ngôn ngữ nhạy ngữ cảnh)
-   $\mathsf{PSPACE}=\mathsf{NPSPACE}=\bigcup\limits_{k\in\mathbb N}\mathsf{NSPACE}(n^k)$
-   $\mathsf{EXPSPACE}=\mathsf{NEXPSPACE}=\bigcup\limits_{k\in\mathbb N}\mathsf{NSPACE}(2^{n^k})$

## Thời gian đa thức

Đơn giản: nếu tồn tại $k>0$ sao cho thời gian là $O(n^k)$ (không phải $\Theta(n^k)$), với $n$ là kích thước đầu vào, thì thuật toán là **đa thức**. Nếu bài có thuật toán đa thức (trên máy Turing xác định) thì thuộc $\mathsf{P}$.

Thời gian đa thức có thể là mạnh/ yếu, và còn có giả đa thức.

### Strongly polynomial time (đa thức mạnh)

Định nghĩa mô hình số học: các phép toán số học (cộng/trừ/nhân/chia/so sánh) hoàn thành trong $O(1)$, không phụ thuộc vào độ lớn số.

Nếu thuật toán trong mô hình này thực hiện số phép toán là đa thức theo số lượng số đầu vào, và không gian là đa thức theo kích thước đầu vào (không chỉ số lượng số), thì là **đa thức mạnh**. Vì phép toán số học có thể làm trong thời gian đa thức theo độ dài biểu diễn, thuật toán đa thức mạnh luôn là đa thức.

Nói chung, đa thức mạnh không phụ thuộc miền giá trị.

### Weakly polynomial time (đa thức yếu)

Nếu thuật toán là đa thức nhưng không phải đa thức mạnh thì gọi là **đa thức yếu**.

Ví dụ: thuật toán Euclid tính gcd có độ phức tạp $O(\log a + \log b)$ (với $a,b$ là giá trị), là đa thức yếu.

### Pseudo-polynomial time (giả đa thức)

Nếu thời gian là đa thức theo miền giá trị thì gọi là **giả đa thức**. Thuật toán giả đa thức có thể là đa thức hoặc không; thường không là đa thức vì số nguyên $n$ chỉ cần $O(\log n)$ bit, nên thời gian đa thức theo giá trị thường là mũ theo độ dài đầu vào. Thông thường nói giả đa thức là không đa thức.

Ví dụ: knapsack là NP-hard nhưng có lời giải DP giả đa thức.

Nếu một bài NPC/NP-hard có lời giải giả đa thức thì gọi là **NPC yếu**/**NP-hard yếu**. Nếu (giả sử $\mathsf{P} \ne \mathsf{NP}$) không có lời giải giả đa thức thì gọi là **NPC mạnh**/**NP-hard mạnh**.

## Hàm có thể xây dựng

### Hàm thời gian có thể xây dựng

Đôi khi ta muốn máy Turing biết mình dùng bao nhiêu thời gian, ví dụ buộc dừng sau $T(n)$ bước. Nhưng nếu tính $T(n)$ lâu hơn $T(n)$ thì bất khả thi. Vì vậy định nghĩa hàm thời gian có thể xây dựng.

Nếu tồn tại máy $M$ sao cho với input $1^n$ ($n$ số 1), $M$ dừng trong $O(f(n))$ và xuất ra biểu diễn nhị phân của $f(n)$ (đầu ra là xâu), thì $f(n)$ là **hàm thời gian có thể xây dựng**.

Vì đọc input cần $O(n)$, nên các hàm không hằng $o(n)$ không thể xây dựng.

### Hàm không gian có thể xây dựng

Tương tự.

Nếu tồn tại máy $M$ sao cho input $1^n$ thì $M$ dừng trong $O(f(n))$ không gian và xuất ra biểu diễn nhị phân của $f(n)$, thì $f(n)$ là **hàm không gian có thể xây dựng**.

## Quan hệ giữa các lớp độ phức tạp

### Định lý phân cấp thời gian

#### Định lý phân cấp thời gian xác định

Nếu $f(n)$ là hàm thời gian có thể xây dựng thì:

$$
\mathsf {DTIME}\left(o\left({\frac {f(n)}{\log f(n)}}\right)\right)\subsetneq \mathsf {DTIME}(f(n))
$$

Suy ra $\mathsf{P}\subsetneq\mathsf{EXPTIME}$.

??? note "Chứng minh định lý phân cấp thời gian xác định"
    Định nghĩa ngôn ngữ $L=\{(x, y)|\mathcal{U}((x, y), x)\text{ dừng trong }f(|x|+|y|)\text{ và từ chối}\}$, do $f(n)$ có thể xây dựng nên có thể quyết định $L$ trong $O(f(|x|+|y|))$, nên $L\in\mathsf{DTIME}(f(n))$.
    
    Giả sử $L\in\mathsf{DTIME}(o\left({\dfrac {f(n)}{\log f(n)}}\right))$, gọi $M_z$ là máy quyết định $L$ trong thời gian này.
    
    Gọi $g(|x|)$ là thời gian của $\mathcal{U}(x, z)$. Từ máy phổ dụng, có $g(n)=o(f(n))$, nên với $y$ đủ lớn, $g(|z|+|y|)<f(|z|+|y|)$.
    
    Lấy $y'$ đủ lớn, khi đó $\mathcal{U}((z, y'), z)$ chắc chắn dừng trong $f(|z|+|y'|)$, suy ra $M_z(z, y')\ne M_z(z, y')$, mâu thuẫn. Do đó giả thiết sai, định lý đúng.

#### Định lý phân cấp thời gian không xác định

Nếu $g(n)$ là hàm thời gian có thể xây dựng và $f(n+1)=o(g(n))$, thì $\mathsf{NTIME}(f(n))\subsetneq\mathsf{NTIME}(g(n))$.

Suy ra $\mathsf{NP}\subsetneq\mathsf{NEXPTIME}$.

### Định lý phân cấp không gian

Nếu $f(n)$ là hàm không gian có thể xây dựng và $f(n)=\Omega(\log n)$ thì $\mathsf{SPACE}(o(f(n)))\subsetneq\mathsf{SPACE}(f(n))$.

Ở đây $\mathsf{SPACE}$ có thể là $\mathsf{DSPACE}$ hoặc $\mathsf{NSPACE}$.

Suy ra $\mathsf{PSPACE}\subsetneq\mathsf{EXPSPACE}$.

### Định lý Savitch

Máy Turing xác định có thể mô phỏng máy không xác định trong không gian bình phương:

Nếu $f(n)=\Omega(\log n)$ thì:

$$
\mathsf{NSPACE}\left(f\left(n\right)\right)\subseteq \mathsf {DSPACE}\left(\left(f\left(n\right)\right)^2\right)
$$

Hệ quả: $\mathsf{PSPACE}=\mathsf{NPSPACE}$, $\mathsf{EXPSPACE}=\mathsf{NEXPSPACE}$.

### P?=NP

Câu hỏi $\mathsf{P}$ có bằng $\mathsf{NP}$ hay không là vấn đề nổi tiếng chưa giải.

Nếu $\mathsf{P}=\mathsf{NP}$, suy ra $\mathsf{NP}=\mathsf{co-NP}$, nhưng chiều ngược lại không suy ra (hiện chưa có cách chứng minh $\mathsf{P}=\mathsf{NP}$ từ $\mathsf{NP}=\mathsf{co-NP}$).

???+ note "Vì sao NP?=co-NP không hiển nhiên?"
    Vì $\mathsf{NP}$ và $\mathsf{co-NP}$ chỉ khác đáp án, dễ nghĩ rằng chỉ cần đảo đầu ra của máy không xác định là xong nên $\mathsf{NP}=\mathsf{co-NP}$.
    
    Thực ra cách đó giải được bài $\mathsf{co-NP}$ nhưng không tạo ra một máy không xác định: máy không xác định chấp nhận nếu **có** một nhánh chấp nhận, còn từ chối nếu **mọi** nhánh từ chối; đảo đầu ra khiến chấp nhận khi **mọi** nhánh chấp nhận, từ chối khi **có** một nhánh từ chối, không còn là định nghĩa máy không xác định, nên không suy ra $\mathsf{co-NP}$ thuộc $\mathsf{NP}$.

Nếu $\mathsf{P}=\mathsf{NP}$ còn suy ra $\mathsf{EXPTIME}=\mathsf{NEXPTIME}$.

Nếu $\mathsf{P}\ne\mathsf{NP}$ thì NP-intermediate không rỗng.

## Tài liệu tham khảo

1.  [计算复杂性（1）Warming Up: 自动机模型](https://lingeros-tot.github.io/2019/03/05/Warming-Up-自动机模型/)；

2.  [计算复杂性（2）图灵机计算模型](https://lingeros-tot.github.io/2019/03/05/图灵机模型与可计算性/)；

3.  [Wikipedia](https://en.wikipedia.org/) và các tài liệu tham khảo trong các mục liên quan.
