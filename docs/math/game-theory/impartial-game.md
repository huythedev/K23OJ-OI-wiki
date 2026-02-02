author: cutekibry, woruo27, tinjyu, 2008verser, Backl1ght, billchenchina, Enter-tainer, FFjet, Ir1d, Molmin, orzAtalod, ouuan, SaMiiKaaaa, SamZhangQingChuan, Tiphereth-A, chu-yuehan

Tiền đề: [Giới thiệu về lý thuyết trò chơi](./intro.md)

Bài viết này thảo luận [trò chơi tổ hợp công bằng](./intro.md#公平组合博弈)．

Trong trò chơi tổ hợp công bằng, cơ bản và quan trọng nhất là trò chơi Nim chuẩn．Định lý Sprague–Grundy chỉ ra rằng mọi trò chơi tổ hợp công bằng theo luật chuẩn đều tương đương với một trò chơi Nim một đống．Từ đó có thể phát triển khái niệm hàm Sprague–Grundy và Nim số, chúng mô tả hoàn toàn một trò chơi tổ hợp công bằng theo luật chuẩn．Vì vậy, bài viết này trước hết xây dựng các kết luận của Nim chuẩn và lý thuyết Sprague–Grundy．Sau đó, bài viết thảo luận một số trò chơi tổ hợp công bằng thường gặp trong thi đấu thuật toán．

Cuối cùng, bài viết đơn giản bàn về Nim phản thường．Trò chơi phản thường phức tạp hơn nhiều so với trò chơi chuẩn và rất hiếm gặp trong thi đấu thuật toán．Những trò chơi được nhắc đến trong bài, nếu không nói rõ, mặc định đều là trò chơi tổ hợp công bằng theo luật chuẩn．

???+ info "「Trạng thái」、「thế cờ」 và 「trò chơi」"
    Bài viết sẽ luân phiên sử dụng ba từ này．Trong lý thuyết trò chơi, trạng thái (state) của trò chơi thường bao gồm tất cả thông tin có liên quan đến trò chơi tại một thời điểm．Trong trường hợp tổng quát, trạng thái thường bao gồm hành động quá khứ của hai người chơi, giá trị của các biến ngẫu nhiên đã hiện thực, nội dung thông tin mà hai bên biết, v.v．Thế cờ (position) không phải là thuật ngữ chuẩn của lý thuyết trò chơi, thường chỉ tình thế mà hai người chơi đối mặt tại một thời điểm, ví dụ vị trí các quân cờ trong trò chơi cờ bàn．Chỉ đối với trò chơi tổ hợp công bằng (hoặc nói rộng hơn là trò chơi tổng bằng, xác định, thông tin hoàn hảo), do không có ngẫu nhiên, tập hành động tương lai và hàm lợi ích không phụ thuộc vào lịch sử dẫn đến thế cờ hiện tại, nên trạng thái và thế cờ không khác nhau, đều có thể xem là một đỉnh trên đồ thị trò chơi．Vì một trò chơi luôn có thể được mô tả bởi thế cờ ban đầu, nên đôi khi dùng từ 「thế cờ」 để chỉ trò chơi itself．

## Trò chơi Nim

Luật Nim rất đơn giản:

???+ abstract "Trò chơi Nim"
    Có $n$ đống đá, đống thứ $i$ có $a_i$ viên．Hai người chơi luân phiên lấy đi một số bất kỳ viên đá từ một đống bất kỳ, nhưng không được bỏ lượt．Người lấy viên cuối cùng thắng．

Dễ thấy, Nim là trò chơi tổ hợp công bằng theo luật chuẩn．

???+ example "Ví dụ"
    Lấy ví dụ．Hiện có $3$ đống đá, số lượng lần lượt là $2,5,4$．Có thể lấy hết $2$ viên ở đống $1$ để được $0,5,4$; cũng có thể lấy $4$ viên ở đống $2$ để được $2,1,4$．Nếu tại một thời điểm thế cờ trở thành $0,0,5$ và người chơi A lấy đi $5$ viên ở đống $3$ (tức là lấy viên cuối cùng), thì A thắng．

### Đồ thị trò chơi và trạng thái

Trong Nim, các biến đổi của thế cờ có thể mô tả bằng đồ thị trò chơi．

Xem mỗi trạng thái là một đỉnh, và nối cạnh có hướng từ trạng thái đến các trạng thái kế tiếp (tức đạt được sau một lần thao tác), ta được một đồ thị có hướng không chu trình, đó là đồ thị trò chơi．Đồ thị không có chu trình vì mỗi lần thao tác, tổng số viên đá giảm строго．

???+ example "Ví dụ"
    Ví dụ, với thế cờ ban đầu có $3$ đống đá, số lượng lần lượt là $1,1,2$, có thể vẽ đồ thị trò chơi như sau：
    
    ![Ví dụ đồ thị trò chơi](./images/nim.svg)
    
    Sắp tới sẽ thấy, các đỉnh màu đỏ là trạng thái thắng, đỉnh màu đen là trạng thái thua．

Do Nim là trò chơi tổ hợp công bằng, việc người chơi có chiến lược thắng chỉ phụ thuộc vào trạng thái hiện tại, không phụ thuộc vào danh tính．Vì vậy, mọi trạng thái có thể chia thành **trạng thái thắng (cho người đi trước)** và **trạng thái thua (cho người đi trước)**, ký hiệu là $\mathcal N$ và $\mathcal P$[^n-vs-p]．Định nghĩa này áp dụng cho mọi trò chơi tổ hợp công bằng．

Thông qua bổ đề sau, có thể quy nạp để gán nhãn tất cả trạng thái là thắng hay thua：<a id="np-lem"></a>

???+ note "Bổ đề"
    Trong trò chơi tổ hợp công bằng theo luật chuẩn，
    
    1.  Trạng thái không có trạng thái kế tiếp là trạng thái thua $\mathcal P$，
    2.  Một trạng thái là thắng $\mathcal N$ khi và chỉ khi tồn tại ít nhất một trạng thái kế tiếp là thua $\mathcal P$，
    3.  Một trạng thái là thua $\mathcal P$ khi và chỉ khi mọi trạng thái kế tiếp đều là thắng $\mathcal N$．

??? note "Chứng minh"
    Với mệnh đề 1, nếu người chơi hiện tại không còn hành động hợp lệ, người đó thua．
    
    Với mệnh đề 2, nếu tồn tại trạng thái kế tiếp là thua, người chơi chuyển đến đó; đối thủ đối mặt với trạng thái thua nên người chơi thắng．
    
    Với mệnh đề 3, nếu không có trạng thái kế tiếp thua, thì mọi lựa chọn đều chuyển sang trạng thái thắng; đối thủ thắng và người chơi thua．

Trong mọi trò chơi tổ hợp công bằng, đồ thị trò chơi là DAG．Vì vậy, nhờ ba tính chất trên, sau khi xây dựng đồ thị trò chơi, có thể tính mọi trạng thái thắng/thua trong thời gian $O(|V|+|E|)$, trong đó $|V|$ là số trạng thái, $|E|$ là số cạnh (tổng số hành động hợp lệ)．

Bổ đề này có thể mở rộng cho trò chơi phản thường và trường hợp đồ thị có hướng có thể có chu trình．Thảo luận chi tiết xem [trò chơi trên đồ thị có hướng](#有向图游戏)．

### Nim tổng

Tiếp tục xét Nim．

Bằng cách vẽ đồ thị trò chơi, có thể xác định một thế cờ thắng hay thua trong thời gian $\Omega(\prod_{i=1}^na_i)$, nhưng phức tạp quá cao．Thực tế, có thể thấy việc thắng/thua chỉ phụ thuộc vào Nim tổng của số đá hiện tại．

???+ abstract "Nim tổng"
    Nim tổng (Nim sum) của các số tự nhiên $a_1,a_2,\cdots,a_n$ được định nghĩa là $a_1\oplus a_2\oplus\cdots\oplus a_n$．

Nim tổng chính là [phép XOR](../bit.md#位运算)．

???+ note "Định lý"
    Trong Nim, trạng thái $(a_1,a_2,\cdots,a_n)$ là thua $\mathcal P$ khi và chỉ khi Nim tổng
    
    $$
    a_1\oplus a_2\oplus\cdots\oplus a_n = 0.
    $$

??? note "Chứng minh"
    Dùng quy nạp cho mọi trạng thái：
    
    1.  Nếu $a_i=0$ với mọi $i$, trạng thái không có trạng thái kế tiếp và Nim tổng bằng $0$，mệnh đề đúng．
    2.  Nếu $k = a_1\oplus a_2\oplus\cdots\oplus a_n\neq 0$，cần chứng minh trạng thái là thắng．Tức là cần một bước đi hợp lệ để trạng thái kế tiếp là thua；theo giả thuyết quy nạp, chỉ cần chứng minh trạng thái kế tiếp có Nim tổng bằng $0$．Dựa vào tính chất XOR, điều này tương đương với việc tồn tại một đống sao cho lấy bớt đá để được $a_i\oplus k$，tức $a_i>a_i\oplus k$．
    
        Thực tế, gọi $d$ là vị trí bit cao nhất có giá trị $1$ trong $k$．Khi đó tồn tại một $a_i$ có bit $d$ là $1$．Với đống này, chắc chắn $a_i>a_i\oplus k$ vì trong $a_i\oplus k$ bit $d$ bằng $0$ và các bit cao hơn giữ nguyên．
    3.  Nếu $a_1\oplus a_2\oplus\cdots\oplus a_n= 0$，cần chứng minh trạng thái là thua．Theo giả thuyết quy nạp, chỉ cần chứng minh mọi trạng thái kế tiếp có Nim tổng khác $0$．Điều này hiển nhiên vì mọi bước đi hợp lệ làm $a_i$ thành $a'_i\neq a_i$ sẽ làm Nim tổng đổi thành $a'_i\oplus a_i\neq 0$．

Do đó, có thể kiểm tra một trạng thái Nim thắng/thua trong $O(n)$．

## Lý thuyết Sprague–Grundy

Lý thuyết Sprague–Grundy cho biết mọi trò chơi tổ hợp công bằng đều tương đương với một Nim một đống．Kết luận này chủ yếu dùng khi trò chơi gồm nhiều tiểu trò chơi độc lập．Khi đó, có thể tính bằng Nim tổng của các giá trị SG．Nếu trò chơi không có cấu trúc này, chỉ cần dùng [bổ đề](#np-lem) ở phần đồ thị trò chơi để判定 thắng/thua．

### Ký hiệu trò chơi

Trước đó đã nói mọi trò chơi tổ hợp công bằng có thể mô tả bằng đồ thị trò chơi．Do mỗi trạng thái chỉ phụ thuộc các trạng thái kế tiếp, ta có thể biểu diễn một trạng thái $S$ bằng tập các trạng thái kế tiếp của nó．

???+ example "Ví dụ (tiếp)"
    Lấy đồ thị trò chơi trên，ta có：
    
    $$
    \begin{aligned}
    S_{0,0,0} &= \{\},\\
    S_{0,1,0} &= \{S_{0,0,0}\} = \{\{\}\},\\
    S_{0,0,1} &= \{S_{0,0,0}\} = \{\{\}\},\\
    S_{0,0,2} &= \{S_{0,0,0},S_{0,0,1}\} = \{\{\},\{\{\}\}\},\\
    S_{0,1,1} &= \{S_{0,0,0},S_{0,1,0},S_{0,0,1}\} = \{\{\},\{\{\}\}\},\\
    S_{0,1,2} &= \{S_{0,0,2},S_{0,1,0},S_{0,1,1}\} = \{\{\{\}\},\{\{\},\{\{\}\}\}\}.
    \end{aligned}
    $$
    
    Trong đó，$S_{0,1,0}=S_{0,0,1}$，$S_{0,0,2}=S_{0,1,1}$．

Một trò chơi có thể biểu diễn bởi trạng thái ban đầu của nó．

Dù biểu diễn trò chơi công bằng có thể phức tạp, Nim một đống thì đơn giản．Với một đống có $n$ viên, ta có

$$
*0 = \{\},~*n = \{*m : m<n,~m\in\mathbf N\} = \{*0,*1,\cdots,*(n-1)\}.
$$

Ký hiệu $*n$ là trò chơi Nim một đống có $n$ viên (trạng thái ban đầu)．

???+ example "Ví dụ (tiếp)"
    Dùng ký hiệu này, các trạng thái trên có thể viết：
    
    $$
    S_{0,0,0} = *0,~
    S_{0,1,0} = S_{0,0,1} = *1,~
    S_{0,0,2} = S_{0,1,1} = *2,~
    S_{0,1,2} = \{*1, *2\}.
    $$

Trong các thảo luận sau, ký hiệu $T\in S$ hiểu là trạng thái $T$ là trạng thái kế tiếp của $S$．

### Tổng và tương đương của trò chơi

Quan hệ tương đương của trò chơi dựa trên khái niệm tổng[^more-sums]．

???+ note "Tổng của trò chơi"
    Tổng (sum), hay trò chơi kết hợp (combined game) của $G$ và $H$, ký hiệu $G+H$, là trò chơi
    
    $$
    G + H = \{g + H : g \in G\} \cup \{G + h : h \in H\}.
    $$

Tổng có thể hiểu là hai tiểu trò chơi diễn ra đồng thời và độc lập; mỗi bước người chơi chỉ được chọn một tiểu trò chơi để đi một bước, và trò chơi kết thúc khi cả hai tiểu trò chơi đều không thể đi．Khái niệm tổng mở rộng cho nhiều trò chơi và thỏa giao hoán, kết hợp — thứ tự kết hợp không quan trọng．Nim chính là tổng của nhiều Nim một đống．

Một quan sát là, mặc dù Nim một đống (trừ trường hợp rỗng) đều là trạng thái thắng cho người đi trước, nhưng chúng không tương đương khi kết hợp với các Nim một đống khác．Ví dụ, $*n$ chỉ khi ghép với $*n$ mới thành trạng thái thua; ghép với $*n'\neq *n$ đều là thắng．

Quan sát này gợi ý: có thể nghiên cứu tính chất của một trò chơi bằng cách xét tổng với trò chơi khác．Từ đó có khái niệm tương đương．

???+ abstract "Quan hệ tương đương của trò chơi"
    Nếu với mọi trò chơi $H$，$G_1+H$ và $G_2+H$ đều đồng thời là thắng hoặc đồng thời là thua, thì nói $G_1$ và $G_2$ **tương đương** (equivalent), ký hiệu $G_1\approx G_2$．

Dễ thấy định nghĩa này khiến $\approx$ là một [quan hệ tương đương](../order-theory.md#二元关系) trên tập các trò chơi công bằng．

### Hàm Sprague–Grundy

Phân tích Nim cho thấy các Nim một đống là không tương đương lẫn nhau．Nhưng mọi trò chơi công bằng đều tương đương với một Nim một đống．Do đó có thể gán cho mỗi trò chơi công bằng một số, gọi là hàm Sprague–Grundy．

Để chứng minh, cần hai bổ đề về quan hệ tương đương．Thứ nhất: một trò chơi thua cộng với bất kỳ trò chơi nào đều tương đương với trò chơi đó．

???+ note "Bổ đề 1"
    Với trò chơi $G$ và mọi trò chơi thua $A\in\mathcal P$, có $G\approx G + A$．

??? note "Chứng minh"
    Theo định nghĩa, chỉ cần chứng minh với mọi $H$，$G+H\approx G+A+H$．
    
    Nếu $G+H$ có chiến lược thắng, thì $G+A+H$ cũng có．Nếu đối thủ đi trong tiểu trò chơi $A$ thì đáp trả để đưa về trạng thái thua; nếu không, đi theo chiến lược thắng của $G+H$．Khi đó luôn thắng．
    
    Nếu $G+H$ là thua, thì $G+A+H$ cũng thua．Vì bất kể bước đi ở $G+H$ hay $A$, đối thủ đều có thể đưa tiểu trò chơi tương ứng về trạng thái thua trong lượt sau, cuối cùng người đi trước không thể thắng．

Thứ hai: hai trò chơi tương đương khi và chỉ khi tổng của chúng là thua．Bổ đề này cung cấp cách chứng minh tương đương．

<a id="sg-lem-2"></a>

???+ note "Bổ đề 2"
    $G$ và $G'$ tương đương khi và chỉ khi $G+G'\in\mathcal P$ là thua．

??? note "Chứng minh"
    Nếu $G$ và $G'$ tương đương, thì $G+G'$ và $G+G$ đồng thời thắng hoặc thua, mà $G+G$ là thua, vì người đi sau có thể bắt chước ở tiểu trò chơi kia và cuối cùng người đi trước không còn nước．
    
    Ngược lại, nếu $G+G'$ là thua, theo bổ đề 1, $G\approx G+(G+G') = (G+G)+G' \approx G'$．

Từ đó có định lý:

???+ note "Định lý (Sprague–Grundy)"
    Với mọi trò chơi công bằng (hữu hạn) $G$, tồn tại $n\in\mathbf N$ sao cho $G\approx *n$．

??? note "Chứng minh"
    Dùng quy nạp toán học．Giả sử $G = \{G_1,G_2,\cdots,G_k\}$．Theo giả thuyết quy nạp, tồn tại $n_1,\cdots,n_k$ sao cho $G_i\approx *n_i$．Xét
    
    $$
    G' = \{*n_1,*n_2,\cdots,*n_k\}.
    $$
    
    Cần chứng minh $G'\approx *m$ với $m=\operatorname{mex}\{n_1,n_2,\cdots,n_k\}$ là số tự nhiên nhỏ nhất không thuộc tập．
    
    Bước 1: chứng minh $G\approx G'$．Theo [Bổ đề 2](#sg-lem-2), chỉ cần chứng minh $G+G'$ là thua．Giả sử $G\neq *0$．Nếu người đi trước chọn $G_i$, người đi sau chọn $*n_i$; ngược lại nếu chọn $*n_i$ thì người đi sau chọn $G_i$．Sau hai bước, trò chơi thành $G_i+*n_i$，theo bổ đề 2 và $G_i\approx *n_i$, đây là thua．Vậy $G\approx G'$．
    
    Bước 2: chứng minh $G'\approx*m$．Theo [Bổ đề 2](#sg-lem-2), chỉ cần chứng minh $G'+*m$ là thua．Giả sử $G'\neq *0$．Nếu người đi trước chọn $*n_i\in *m$，theo định nghĩa $m$, người đi sau chọn $*n_i\in G'$，đưa trò chơi về $*n_i+*n_i\in\mathcal P$，người đi trước thua．Nếu người đi trước chọn $*n_i\in G'$ với $n_i<m$，người đi sau chọn $*n_i\in *m$，tương tự．Cuối cùng nếu người đi trước chọn $*n_i\in G'$ với $n_i>m$，người đi sau chọn $*m\in *n_i$，đưa về $*m+*m\in\mathcal P$．Vậy $G'\approx *m$．
    
    Tính truyền递 của tương đương cho $G\approx *m$．Quy nạp hoàn tất, mọi $G$ đều tương đương với Nim một đống．

Kết luận này nói rằng mỗi trò chơi công bằng $G$ tương ứng duy nhất với một số tự nhiên $n$ sao cho $G\approx *n$．

???+ abstract "Nim số"
    Nim số (nimber) của trò chơi công bằng $G$ là số tự nhiên duy nhất $n$ sao cho $G\approx *n$．

Hàm ánh xạ trò chơi công bằng sang Nim số gọi là **hàm Sprague–Grundy** (SG), ký hiệu $\operatorname{SG}(\cdot)$．Vì mỗi trạng thái của trò chơi công bằng cũng là một trò chơi công bằng, nên với mỗi trạng thái ta cũng có thể tính Nim số (giá trị SG)．

Theo quá trình chứng minh định lý, hàm SG có thể tính đệ quy:

???+ note "Hệ quả"
    Với trạng thái $x$ trong trò chơi công bằng $G$, giá trị SG thỏa
    
    $$
    \operatorname{SG}(x) = \operatorname{mex}\{\operatorname{SG}(x'): x'\in x\}.
    $$
    
    Trong đó, $\operatorname{mex}(A):=\min\{n\in\mathbf N:n\notin A\}$ là số tự nhiên nhỏ nhất không thuộc $A$．

Tức là giá trị SG của một trạng thái bằng $\operatorname{mex}$ của các giá trị SG của các trạng thái kế tiếp．

Dùng SG (Nim số) để判定 trạng thái thắng:

???+ note "Hệ quả"
    Trạng thái $x$ là thắng khi và chỉ khi $\operatorname{SG}(x)\neq 0$．

Cuối cùng, SG của tổng trò chơi là Nim tổng của SG các tiểu trò chơi:

???+ note "Định lý (Sprague–Grundy)"
    Với các trò chơi công bằng $G_1,G_2,\cdots,G_n$，
    
    $$
    \operatorname{SG}(G_1+ G_2+\cdots + G_n) = \operatorname{SG}(G_1)\oplus \operatorname{SG}(G_2)\oplus\cdots\oplus\operatorname{SG}(G_n).
    $$

??? note "Chứng minh"
    Vì $*a_1+ *a_2 + \cdots + *a_n$ là Nim với số đá $(a_1,a_2,\cdots,a_n)$, theo kết luận Nim ta biết
    
    $$
    *a_1+ *a_2 + \cdots + *a_n + *(a_1\oplus a_2\oplus\cdots\oplus a_n)
    $$
    
    là thua cho người đi trước．Theo [Bổ đề 2](#sg-lem-2),
    
    $$
    *a_1+ *a_2 + \cdots + *a_n \approx *(a_1\oplus a_2\oplus\cdots\oplus a_n).
    $$
    
    Do đó,
    
    $$
    \operatorname{SG}(*a_1 + *a_2 + \cdots + *a_n) = a_1\oplus a_2\oplus\cdots\oplus a_n.
    $$
    
    Đặt $a_i=\operatorname{SG}(G_i)$ thì $G_i\approx *a_i$．Theo tính chất đại số của $\approx$，
    
    $$
    (G_1+ G_2+\cdots + G_n) + (*a_1 + *a_2 + \cdots + *a_n) = \sum_{i=1}^n(G_i+*a_i) \in\mathcal P.
    $$
    
    Vì vậy，
    
    $$
    \begin{aligned}
    \operatorname{SG}(G_1+ G_2+\cdots + G_n) &= \operatorname{SG}(*a_1 + *a_2 + \cdots + *a_n) \\
    &= a_1\oplus a_2\oplus \cdots \oplus a_n \\
    &= \operatorname{SG}(G_1)\oplus \operatorname{SG}(G_2)\oplus\cdots\oplus\operatorname{SG}(G_n).
    \end{aligned}
    $$

Nhờ định lý này, khi tính SG của tổng trò chơi có thể đơn giản hóa rất nhiều．

Tóm tắt cách tính SG：

-   Với nhiều trò chơi độc lập, tính SG từng trò chơi rồi XOR；
-   Với một trò chơi đơn, SG của mỗi trạng thái là $\operatorname{mex}$ của SG các trạng thái kế tiếp；
-   Đặc biệt, trạng thái kết thúc (không có trạng thái kế tiếp) có SG bằng $\operatorname{mex}\varnothing = 0$．

### Nim số

Mọi trò chơi công bằng đều tương ứng duy nhất với một Nim số．(Hữu hạn) tập Nim số là $\mathbf N$．Nhưng cấu trúc đại số khác với số tự nhiên．Cụ thể, trên Nim số định nghĩa phép Nim tổng $\oplus$ và Nim tích $\otimes$：

???+ abstract "Phép toán trên Nim số"
    Với Nim số $a,b$，định nghĩa：
    
    -   Nim tổng $a\oplus b=\operatorname{mex}(\{a'\oplus b:a'<a,~a'\in\mathbf N\}\cup\{a\oplus b':b'<b,~b'\in\mathbf N\})$，
    -   Nim tích $a\otimes b=\operatorname{mex}(\{(a'\otimes b)\oplus(a\otimes b')\oplus(a'\otimes b'):a'<a,~b'<b,~a',b'\in\mathbf N\})$．

Toàn bộ Nim số dưới $\oplus$ và $\otimes$ tạo thành một [trường](../algebra/basic.md#域) có đặc trưng $2$．Hơn nữa, các phép toán này và nghịch đảo của chúng đóng trên $2^{2^n}$ Nim số đầu；do đó thu được dãy [trường hữu hạn](../algebra/field-theory.md#有限域) $\mathbf F_{2^{2^n}}$．

## Các trò chơi công bằng thường gặp

Dù Sprague–Grundy giải quyết hoàn toàn trò chơi công bằng, nhưng với trò chơi cụ thể, tính SG trực tiếp thường không hiệu quả．Ví dụ, trong Nim, vét cạn SG là mũ．Vì vậy thường phải "bấm bảng" để đoán kết luận．

Phần này liệt kê một số trò chơi công bằng thường gặp và kết luận．Khi nêu kết luận chỉ cho điều kiện thắng/thua．Chiến lược thắng là thực hiện bước đi thích hợp để để lại thế thua cho đối thủ．Vì các biến thể xuất hiện nhiều trong thi đấu, nắm vững chứng minh cũng rất quan trọng．

???+ info "Phương pháp chứng minh kết luận trong phần này"
    Các chứng minh đều là kiểm chứng．Với một trò chơi, kết luận mô tả trạng thái thua và thắng cho người đi trước．Chứng minh chỉ cần kiểm tra: từ trạng thái thua chỉ có thể đi tới trạng thái thắng; từ trạng thái thắng luôn có ít nhất một bước đi tới trạng thái thua．Để viết chứng minh chặt chẽ, cần dựng đồ thị trò chơi và quy nạp toán học trên các trạng thái, và các bước kiểm chứng chính là phần quy nạp．

### Trò chơi Bachet

So với Nim một đống, Bachet giới hạn số đá có thể lấy mỗi lần．

???+ abstract "Trò chơi Bachet"
    Có một đống đá, tổng $n$ viên．Hai người luân phiên lấy ít nhất $1$ và nhiều nhất $k$ viên．Người lấy viên cuối cùng thắng．

Kết luận:

???+ note "Định lý"
    Trò chơi thua cho người đi trước khi và chỉ khi $n\equiv 0\pmod {k+1}$．

??? note "Chứng minh 1"
    Khi $n\not\equiv 0\pmod {k+1}$，chỉ cần lấy $n\bmod{(k+1)}\in[1,k]$ viên để đưa đối thủ vào thế thua．Vậy là thắng．
    
    Ngược lại, khi $n\equiv 0\pmod {k+1}$，hoặc không còn nước đi, hoặc nếu lấy $k'$ viên thì đối thủ sẽ lấy $k+1-k'$ viên, đưa lại về thế thua cho mình．

??? note "Chứng minh 2"
    Theo Sprague–Grundy, đặt $f(n)$ là giá trị SG khi còn $n$ viên．
    
    Với $n\le k$, có thể quy nạp chứng minh $f(n)=n$ giống Nim một đống vì giới hạn chưa có tác dụng．Với $n>k$，có thể chứng minh $f(n)=n\bmod{(k+1)}$, nên
    
    $$
    f(n) = \operatorname{mex}\{f(n-k),f(n-k+1),\cdots,f(n-1)\}.
    $$
    
    Tập này duyệt toàn bộ các dư modulo $k+1$ trừ $n\bmod{(k+1)}$，nên $f(n) = n\bmod{(k+1)}$．

### Trò chơi Moore's Nim-k

So với Nim, Moore's Nim-$k$ cho phép lấy đá từ tối đa $k$ đống trong một lượt．

???+ abstract "Trò chơi Moore's Nim-$k$"
    Có $n$ đống đá, đống $i$ có $a_i$ viên．Hai người luân phiên lấy bất kỳ số viên từ ít nhất $1$ và nhiều nhất $k$ đống, nhưng không được bỏ lượt．Người lấy viên cuối cùng thắng．

Kết luận:

???+ note "Định lý"
    Viết mỗi $a_i$ ở dạng nhị phân, với mỗi bit $d$, đếm số đống có bit $d$ bằng $1$ rồi lấy modulo $(k+1)$．Nếu với mọi bit, dư đều bằng $0$ thì người đi trước thua; ngược lại thắng．

??? note "Chứng minh"
    Mô phỏng chứng minh Nim．Gọi $d$ là bit cao nhất có dư khác $0$, và dư là $k'\le k$．Chiến lược thắng: trong các đống có bit $d$ bằng $1$, chọn $k$ đống và lấy số viên sao cho trong thế của đối thủ, mọi bit đều có dư $0$．Cần chứng minh luôn chọn được số viên hợp lệ．
    
    Thực tế, chỉ cần chọn $k'$ đống và mỗi đống lấy $2^d$ viên là dư bit $d$ thành $0$．Với các bit thấp hơn, phân phối các dư đó tùy ý cho một đống nào đó．

### Trò chơi Nim bậc thang

Trò chơi này cho phép chuyển đá giữa các đống kề nhau．

???+ abstract "Trò chơi Nim bậc thang"
    Có $n$ đống đá, đống $i$ có $a_i$ viên．Mỗi lượt, hoặc lấy bất kỳ số viên từ đống 1, hoặc chuyển bất kỳ số viên từ đống $i>1$ sang đống $i-1$, nhưng không được bỏ lượt．Người lấy viên cuối cùng thắng．

Kết luận:

???+ note "Định lý"
    Trò chơi thua cho người đi trước khi và chỉ khi Nim tổng của các đống lẻ $a_1\oplus a_3\oplus\cdots\oplus a_{n-1+(n\bmod 2)}=0$．

??? note "Chứng minh"
    Khi người chơi chuyển đá từ đống chẵn sang đống lẻ, đối thủ có thể tiếp tục chuyển chúng sang đống chẵn kế tiếp (hoặc lấy đi), nên việc này không ảnh hưởng đến cấu hình đống lẻ．Lúc này, mỗi đống lẻ chuyển xuống đống chẵn (hoặc lấy đi) có thể xem là một Nim một đống độc lập．Theo Sprague–Grundy về tổng trò chơi, SG của Nim bậc thang là Nim tổng các SG đó, suy ra kết luận．

### Trò chơi Fibonacci Nim

Giống Bachet nhưng giới hạn động．

???+ abstract "Trò chơi Fibonacci Nim"
    Có một đống đá $n$ viên．Hai người luân phiên lấy đá．Người đi đầu không bị giới hạn số viên nhưng không được lấy hết．Sau đó, số viên lấy mỗi lượt không vượt quá gấp đôi số viên đối thủ vừa lấy．Mỗi lượt phải lấy ít nhất một viên．Người lấy viên cuối cùng thắng．

Kết luận:

???+ note "Định lý"
    Ở trạng thái ban đầu, người đi trước thua khi và chỉ khi $n$ là [số Fibonacci](../combinatorics/fibonacci.md)．

??? note "Chứng minh"
    Gọi $q$ là giới hạn (quota) số viên có thể lấy．Lượt đầu $q=n-1$; các lượt sau $q$ bằng gấp đôi số viên đối thủ vừa lấy．Xét [mã hóa Fibonacci](../combinatorics/fibonacci.md#斐波那契编码) của $n$，tức biểu diễn $n$ thành tổng các số Fibonacci dương không kề nhau．Cần chứng minh: trạng thái hiện tại là thắng khi và chỉ khi $q$ lớn hơn hoặc bằng số Fibonacci nhỏ nhất trong phân rã của $n$．
    
    Chiến lược thắng: nếu có thể thì lấy hết; nếu không, lấy số Fibonacci nhỏ nhất trong phân rã．Do số Fibonacci nhỏ thứ hai luôn lớn hơn gấp đôi số nhỏ nhất, nên nếu không lấy hết, đối thủ không thể lấy được số Fibonacci nhỏ thứ hai (tức nhỏ nhất ở lượt sau), đối thủ ở thế thua．
    
    Ngược lại, nếu đang ở thế thua, giả sử lấy $k$ viên, chắc chắn $k$ nhỏ hơn số Fibonacci nhỏ nhất $F$．Gọi $F'$ là số nhỏ nhất trong phân rã của $F-k$ ở lượt sau．Viết $F'=F''+F'''$ với $F''>F'''$ (ba số liên tiếp trong dãy Fibonacci)．Nếu $k<F''$，thì khi cộng $k+(F-k)$ theo mã Fibonacci không cần carry, nên không thể có $F$．Vì vậy phải có $k\ge F''$．Suy ra giới hạn lượt sau $2k>F''+F'''=F'$，là thế thắng．

### Trò chơi Wythoff

Cho phép lấy đồng thời từ nhiều đống với số lượng bằng nhau．

???+ abstract "Trò chơi Wythoff"
    Có hai đống đá, lần lượt $a_1$ và $a_2$ viên．Hai người luân phiên lấy đá từ một đống hoặc cả hai đống, không được bỏ lượt, nhưng nếu lấy cả hai đống thì số viên lấy phải bằng nhau．Người lấy viên cuối cùng thắng．

Kết luận:

???+ note "Định lý"
    Giả sử $a_1\le a_2$，người đi trước thua khi và chỉ khi $a_1 = \lfloor(a_2-a_1)\phi\rfloor$，trong đó $\phi=(\sqrt{5}+1)/2$ là tỉ lệ vàng．

Để chứng minh, cần các bổ đề sau：

???+ abstract "Dãy Beatty"
    Với $r > 1$ vô tỷ，dãy Beatty là $\mathcal B_r = \{\lfloor kr\rfloor : k \in\mathbf N_+\}$．

???+ note "Định lý Rayleigh"
    Với $r,s > 1$ vô tỷ, và $\dfrac{1}{r}+\dfrac{1}{s}=1$，thì $\mathcal B_r$ và $\mathcal B_s$ phân hoạch $\mathbf N_+$．Khi đó gọi là hai dãy Beatty bổ sung．

??? note "Chứng minh"
    Đặt $\mathcal A_r=\{kr:k\in\mathbf N_+\}$．Xét dãy $\{a_i\}$ là kết quả sắp xếp các phần tử của $\mathcal A=\mathcal A_r\cup\mathcal A_\ell$．Cần chứng minh $i=\lfloor a_i\rfloor$ với mọi $i$ để suy ra $\mathcal B_r\cup\mathcal B_s$ là phân hoạch của $\mathbf N_+$．
    
    Trước hết, không có phần tử trùng nhau．Nếu có $kr=\ell s$ với $k,\ell\in\mathbf N_+$，thì
    
    $$
    \dfrac{\ell}{k} = \dfrac{r}{s} = r - 1.
    $$
    
    Vế trái hữu tỷ, vế phải vô tỷ, mâu thuẫn．
    
    Tiếp theo, chứng minh số phần tử trong $\mathcal A$ không vượt quá $a_i$ là $\lfloor a_i\rfloor$．Không mất tổng quát, $a_i=kr\in\mathcal A_r$，thì số nguyên dương không vượt quá $a_i$ là
    
    $$
    k + \left\lfloor\dfrac{kr}{s}\right\rfloor = k + \lfloor k(r-1)\rfloor = \lfloor kr\rfloor = \lfloor a_i\rfloor
    $$
    
    Do $\{a_i\}$ tăng nghiêm ngặt, số phần tử không vượt quá $a_i$ là $i$．Suy ra $i=\lfloor a_i\rfloor$．

Từ đó chứng minh kết luận trên．

??? note "Chứng minh kết luận Wythoff"
    Với mọi trạng thái thua $(a_1,a_2)$ có $a_1 < a_2$，đặt $k = a_2 - a_1 \in\mathbf N_+$，ta có $a_1=\lfloor k\phi\rfloor$ và $a_2=\lfloor k(\phi+1)\rfloor$．Do $\phi$ là tỉ lệ vàng nên $\dfrac{1}{\phi}+\dfrac{1}{\phi+1}=1$．Theo Rayleigh, $\{\lfloor k\phi\rfloor\}$ và $\lfloor k(\phi+1)\rfloor$ phân hoạch $\mathbf N_+$．Điều này nói rằng mọi trạng thái thua $(a_1,a_2)$ với $a_1<a_2$ bao phủ toàn bộ số nguyên dương đúng một lần ở mỗi thành phần, và hiệu $a_2-a_1$ cũng chạy qua toàn bộ số nguyên dương đúng một lần．
    
    Trong Wythoff, một bước hợp lệ hoặc giữ nguyên một thành phần hoặc giữ nguyên hiệu hai thành phần, nên từ trạng thái thua không thể đi đến trạng thái thua khác．Ngược lại, với mọi trạng thái thắng $(a_1,a_2)$, giả sử $a_1\le a_2$ và $k=a_2-a_1$．Nếu $a_1>\lfloor k\phi\rfloor$，người đi trước có thể lấy $(a_1 - \lfloor k\phi\rfloor)$ viên từ cả hai đống để đưa về trạng thái thua．Ngược lại, theo đoạn trước, với $a_1$ tồn tại duy nhất một trạng thái thua $(a_1,a_2')$．Nếu $a_1 > a_2'$ thì $a_2' < a_2$；nếu $a_1 < a_2'$，lấy $k'=a_2'-a_1$ sao cho $a_1=\lfloor k'\phi\rfloor$，suy ra $a_1 < \lfloor k\phi\rfloor$ nên $k' < k$ và $a_2'=a_1 + k' < a_1+k = a_2$．Vì vậy nếu $a_1 < \lfloor k\phi\rfloor$ thì $a_2' < a_2$，người đi trước chỉ cần lấy $(a_2-a'_2)$ viên từ đống hai để đưa về trạng thái thua．

### Trò chơi lật đồng xu

Trò chơi lật xu là một lớp trò chơi công bằng thường gặp．

???+ abstract "Trò chơi lật đồng xu"
    Cho $(S,\preceq)$ là một [tập thứ tự tốt](../order-theory.md)，ánh xạ $f:S\rightarrow\mathcal P\mathcal PS$ thỏa với mọi $s\in S$ thì $f(s)$ không rỗng, với mọi $T\in f(s)$ thì $s\in T$, và với mọi $t\in T$ thì $t\preceq s$．Tại mỗi phần tử của $S$ có một đồng xu, có thể ngửa hoặc sấp．Hai người luân phiên hành động, chọn một đồng xu ngửa $s$ và một tập $T\in f(s)$, rồi lật tất cả đồng xu trong $T$．Người lật tất cả đồng xu về sấp thắng．

Trò chơi này là một lớp lớn．Tùy vào $S$ và $f$ mà hình thức khác nhau．Điều kiện của $f$ nói rằng trong tập $T$ được lật luôn có một đồng xu ngửa $s$ sao cho mọi phần tử trong $T$ đứng trước $s$．Điều này đảm bảo trò chơi kết thúc sau hữu hạn bước．

???+ example "Ví dụ"
    1.  $S=\{1,2,\cdots,n\}$ và $f(s)=\{\{t,s\}:t \le s\}$．Tức có một hàng $n$ đồng xu, mỗi lần lật một đồng xu ngửa, và có thể lật thêm một đồng xu bên trái nó．
    2.  $S=\{1,2,\cdots,n\}$ và $f(s)=\{[t,s]:t \le s\}$．Tức có một hàng $n$ đồng xu, mỗi lần lật một đoạn liên tiếp, nhưng đồng xu phải ở vị trí phải nhất của đoạn phải đang ngửa．
    3.  $S=\{1,2,\cdots,n\}^2$ và $f(s)=\{\{s\}\}$．Tức có lưới $n\times n$, mỗi lần chỉ lật một đồng xu ngửa．
    4.  $S$ là tập đỉnh của một cây có gốc, và $f(s)$ là tất cả các tập con của đường đi từ $s$ đến gốc mà chứa $s$．Tức có một cây có gốc, mỗi đỉnh có một đồng xu, mỗi lần lật một đồng xu ngửa và có thể lật thêm một số tổ tiên của nó．

Mặc dù nhiều dạng, cách giải là thống nhất．Gọi $G_s$ là thế cờ chỉ có đồng xu tại $s$ là ngửa．Các thế cờ này là thế cơ sở．Mọi thế cờ $G$ có thể xem là tổng của các trò chơi tương ứng các thế cơ sở đó．Do đó:

???+ note "Định lý"
    Với trò chơi lật xu $(S,f)$ và thế cờ $G$, gọi $H(G)\subseteq S$ là tập vị trí có đồng xu ngửa．Khi đó,
    
    $$
    \operatorname{SG}(G) = \bigoplus_{s\in H(G)}\operatorname{SG}(G_s).
    $$

??? note "Chứng minh"
    Xét trò chơi liên quan: mỗi vị trí $s$ đặt một số viên đá; mỗi lượt được lấy một viên đá ở $s$, chọn $T\in f(s)$, rồi thêm một viên đá vào mỗi phần tử trong $T\setminus\{s\}$．Vẫn định nghĩa thế cơ sở $G'_s$ là chỉ có một viên đá ở $s$．Mọi thế cờ là tổng của các thế cơ sở của từng viên đá, vì có thể gắn mỗi viên đá mới với viên đá bị lấy đi, và quá trình của các viên đá độc lập nhau．Do các viên đá ở cùng vị trí có SG như nhau, nên SG của thế cờ chỉ phụ thuộc vào chẵn lẻ số viên đá tại mỗi vị trí, không phụ thuộc số lượng cụ thể．Vì vậy, nếu gọi $H(G')$ là tập vị trí có số viên đá lẻ, ta có：
    
    $$
    \operatorname{SG}(G') = \bigoplus_{s\in H(G')} \operatorname{SG}(G'_s).
    $$
    
    Cần chứng minh $G'$ tương đương $G$ khi $H(G')$ trùng với tập đồng xu ngửa của $G$．Theo [Bổ đề 2 của Sprague–Grundy](#sg-lem-2), điều này tương đương chứng minh $G+G'$ là thua．Chiến lược của người đi sau: nếu người đi trước lấy đá tại $s$ và còn nhiều hơn một viên đá, người đi sau bắt chước; nếu không, người đi sau chọn cùng $s$ và $T\in f(s)$ nhưng chọn tiểu trò chơi khác (người trước lấy đá thì người sau lật xu, ngược lại)．Như vậy luôn giữ được rằng tập vị trí có đá lẻ trùng với tập đồng xu ngửa．Trò chơi kết thúc khi người đi trước không còn nước, nên người đi trước thua．Định lý được chứng minh．

Dùng kết luận này, để判定 một thế cờ thắng, chỉ cần XOR các SG của các thế cơ sở tương ứng với các đồng xu ngửa．Các SG cơ sở có thể tính quy nạp:

$$
\operatorname{SG}(G_s) = \operatorname{mex}\limits_{T\in f(s)}\bigoplus_{t\in T\setminus\{s\}}\operatorname{SG}(G_t).
$$

Đây là công thức truy hồi cho SG của thế cơ sở．

### Trò chơi trên đồ thị hai phía

Tiền đề: [Ghép cực đại trên đồ thị hai phía](../../graph/graph-matching/bigraph-match.md)

Cuối phần này thảo luận trò chơi trên đồ thị hai phía．Dù tên vậy, mô tả và chứng minh không phụ thuộc vào cấu trúc hai phía, nên kết luận đúng với đồ thị vô hướng tổng quát．Tuy nhiên, ghép cực đại trên đồ thị tổng quát phức tạp, nên kết luận này hay xuất hiện trong bài hai phía．

???+ abstract "Trò chơi trên đồ thị hai phía"
    Hai người chơi luân phiên．Mỗi thế cờ bởi đồ thị vô hướng $G=(V,E)$ và một đỉnh $v\in V$．Trong lượt của một người, nếu thế cờ là $(G,v)$, người đó phải chọn một đỉnh kề $u$ của $v$．Sau đó, xóa đỉnh $v$ và các cạnh kề khỏi $G$得到图 $G'$．Thế cờ mới là $(G',u)$ giao cho người tiếp theo．Nếu ở lượt của ai đó, đỉnh $v$ không còn kề đỉnh nào, người đó không thể đi và thua．

Kết luận:

???+ note "Định lý"
    Người đi trước thắng khi và chỉ khi đỉnh $v$ là điểm then chốt của ghép cực đại của $G$, tức trong mọi ghép cực đại của $G$, $v$ đều là đỉnh được ghép．

??? note "Chứng minh"
    Trước hết, giả sử $v$ là điểm then chốt．Gọi $M$ là một ghép cực đại của $G$．Người đi trước có thể chuyển đến đỉnh $u$ được ghép với $v$ trong $M$．Vì $v$ thuộc mọi ghép cực đại, nên ghép cực đại của $G'$ có kích thước最多 $|M|-1$；và bỏ cạnh $(v,v)$ khỏi $M$ (ý là bỏ cạnh ghép với $v$) ta được một ghép $M'$ kích thước $|M|-1$ của $G'$，suy ra $M'$ là cực đại của $G'$．Khi đó trong thế cờ của người đi sau, $u$ không phải là đỉnh được ghép trong $M'$，nên người đi sau ở thế thua．
    
    Ngược lại, giả sử tồn tại một ghép cực đại $M$ mà $v$ không được ghép．Vì $M$ là cực đại, mọi đỉnh kề với $v$ đều là đỉnh được ghép; nếu không, thêm cạnh sẽ cho ghép lớn hơn．Do đó, bất kể người đi trước chọn gì, người đi sau đều ở thế thắng．

Thuật toán tìm điểm then chốt của ghép cực đại xem [trang ghép cực đại hai phía](../../graph/graph-matching/bigraph-match.md#最大匹配关键点)．

Ngoài ra còn biến thể:

???+ abstract "Biến thể của trò chơi đồ thị hai phía"
    Cho đồ thị vô hướng $G=(V,E)$, mỗi đỉnh có một viên đá．Hai người luân phiên lấy đá．Ban đầu người đi trước lấy bất kỳ viên nào；sau đó mỗi lượt, người chơi phải lấy viên ở đỉnh kề với đỉnh mà đối thủ vừa lấy．Người đầu tiên không thể lấy đá thua．

Biến thể này tương đương với việc người đi trước chọn thế cờ ban đầu, rồi người đi sau bắt đầu trò chơi đồ thị hai phía．Vì vậy, người đi trước thua khi và chỉ khi mọi đỉnh đều là điểm then chốt, tức $G$ có [ghép hoàn hảo](../../graph/graph-matching/graph-match.md#定义)．

## Nim phản thường

Phần này thảo luận Nim phản thường．

???+ abstract "Trò chơi Nim"
    Có $n$ đống đá, đống $i$ có $a_i$ viên．Hai người luân phiên lấy bất kỳ số viên từ một đống bất kỳ, không được bỏ lượt．Người lấy viên cuối cùng thua．

Kết luận:

???+ note "Định lý"
    Trong Nim phản thường, trạng thái $(a_1,a_2,\cdots,a_n)$ là thua $\mathcal P$ khi và chỉ khi
    
    1.  Tồn tại $i$ sao cho $a_i>1$ và Nim tổng $a_1\oplus a_2\oplus\cdots\oplus a_n=0$，hoặc
    2.  Với mọi $i$ đều có $a_i\le 1$ và số đống không rỗng còn lại là số lẻ．

??? note "Chứng minh"
    Do không thể đi là trạng thái thắng $\mathcal N$，nên có thể quy nạp rằng nếu mỗi đống chỉ có một viên, số đống lẻ thì là trạng thái thua $\mathcal N$, số đống chẵn thì là trạng thái thắng $\mathcal N$．
    
    Tiếp theo xét trường hợp có đống lớn hơn $1$．
    
    Trường hợp A: Nếu chỉ có một đống có số viên >1, khi đó Nim tổng chắc chắn không bằng $0$．Hơn nữa người đi trước có thể đưa về trạng thái mọi đống $\le 1$ và điều khiển chẵn lẻ số đống không rỗng．Do đó đây là trạng thái thắng $\mathcal N$．
    
    Trường hợp B: Nếu có ít nhất hai đống >1, thì sau mọi bước đi, trạng thái tiếp theo vẫn có ít nhất một đống >1．Theo giả thuyết quy nạp, ở trạng thái tiếp theo, Nim tổng bằng $0$ tương ứng thua, khác $0$ tương ứng thắng．Điều này giống hệt Nim chuẩn, nên lặp lại chứng minh Nim cho thấy trạng thái hiện tại thua khi và chỉ khi Nim tổng bằng $0$．

## Trò chơi trên đồ thị có hướng

Các trò chơi công bằng đã nói yêu cầu không có trạng thái lặp lại và không có hòa, nên đồ thị trò chơi là DAG．Phần này nới lỏng, xét cách phân loại trạng thái thắng, thua, hoặc hòa trên đồ thị có hướng tổng quát．

Luật chung: từ trạng thái ban đầu, hai người luân phiên đi theo cạnh có hướng; khi không còn cạnh đi được thì dừng．Theo luật chuẩn hoặc phản thường, người không đi được là thua hoặc thắng．Trong trò chơi này, mỗi trạng thái có ba khả năng: thắng, thua, hòa．Hòa là trò chơi không bao giờ kết thúc．Dù phức tạp hơn, bổ đề về thắng/thua vẫn đúng, còn lại là hòa：

-   Một trạng thái là thắng khi và chỉ khi tồn tại trạng thái kế tiếp là thua；
-   Một trạng thái là thua khi và chỉ khi mọi trạng thái kế tiếp đều là thắng；
-   Trạng thái không phân loại được thắng hay thua thì là hòa．

Phân loại tất cả trạng thái có thể dùng ý tưởng như [tôpô](../../graph/topo.md)：

1.  Khởi tạo: ghi nhận mọi bậc ra, đưa các trạng thái bậc ra bằng $0$ vào hàng đợi, và tùy luật chuẩn/phản thường mà gán là thua hoặc thắng．
2.  Pop một trạng thái ở đầu．Nếu là thua, đặt các trạng thái tiền nhiệm là thắng；nếu là thắng, giảm bậc ra của các tiền nhiệm và khi bậc ra về $0$ thì đặt là thua．Đưa các trạng thái vừa判定 được vào hàng đợi．
3.  Khi hàng đợi rỗng, dừng．Các trạng thái chưa判定 là hòa．

Thuật toán chạy $O(|V|+|E|)$．

## Ví dụ

Phần này bàn về một số bài mẫu．

???+ example "[Luogu P2148 \[SDOI2009\] E&D](https://www.luogu.com.cn/problem/P2148)"
    Có $2n$ đống đá．Với $k=1,2,\cdots,n$，đống $2k-1$ và $2k$ là một nhóm．Hai người luân phiên, mỗi lần chọn một nhóm, loại bỏ một đống trong nhóm và chia đống còn lại thành hai đống không rỗng đặt lại ở hai vị trí của nhóm．Nếu mọi đống đều có một viên, người chơi hiện tại không có nước đi và thua．Cho $\{a_i\}_{i=1}^{2n}$, hỏi người đi trước có thắng không．

??? note "Lời giải"
    Rõ ràng các nhóm độc lập, nên chỉ cần tính SG từng nhóm rồi XOR．Khó ở chỗ tính SG của mỗi nhóm．Cách thường dùng là bấm bảng．Đặt SG của nhóm có $(i,j)$ viên là $f(i,j)$，viết chương trình vét cạn得到：
    
    ```text
    0 1 0 2 0 1 0 3 0 1 0 2 0 1 0 4 
    1 1 2 2 1 1 3 3 1 1 2 2 1 1 4 4
    0 2 0 2 0 3 0 3 0 2 0 2 0 4 0 4
    2 2 2 2 3 3 3 3 2 2 2 2 4 4 4 4
    0 1 0 3 0 1 0 3 0 1 0 4 0 1 0 4
    1 1 3 3 1 1 3 3 1 1 4 4 1 1 4 4
    0 3 0 3 0 3 0 3 0 4 0 4 0 4 0 4
    3 3 3 3 3 3 3 3 4 4 4 4 4 4 4 4
    0 1 0 2 0 1 0 4 0 1 0 2 0 1 0 4
    1 1 2 2 1 1 4 4 1 1 2 2 1 1 4 4
    0 2 0 2 0 4 0 4 0 2 0 2 0 4 0 4
    2 2 2 2 4 4 4 4 2 2 2 2 4 4 4 4
    0 1 0 4 0 1 0 4 0 1 0 4 0 1 0 4
    1 1 4 4 1 1 4 4 1 1 4 4 1 1 4 4
    0 4 0 4 0 4 0 4 0 4 0 4 0 4 0 4
    4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4
    ```
    
    Bảng rất có quy luật．Quan sát: chia thành nhiều khối $2\times 2$，góc trái trên luôn là $0$，ba giá trị còn lại bằng nhau．Nén mỗi khối $2\times 2$ thành giá trị chung (trừ góc trái trên)：
    
    ```text
    1 2 1 3 1 2 1 4 
    2 2 3 3 2 2 4 4 
    1 3 1 3 1 4 1 4 
    3 3 3 3 4 4 4 4 
    1 2 1 4 1 2 1 4 
    2 2 4 4 2 2 4 4 
    1 4 1 4 1 4 1 4 
    4 4 4 4 4 4 4 4 
    ```
    
    Có thể thấy bảng nén bằng bảng đầy đủ cộng một．Đặt chỉ số từ $0$, giá trị $g(i,j)$ thỏa：
    
    $$
    g(i,j) =
    \begin{cases}
    0, & \text{if }2\mid i\text{ and }2\mid j,\\
    g(\lfloor i/2\rfloor,\lfloor j/2\rfloor)+1,& \text{otherwise}.
    \end{cases}
    $$
    
    SG cần tìm là $f(i,j)=g(i-1,j-1)$．Công thức này cho phép tính $f(i,j)$ trong $O(\log\min\{i,j\})$．
    
    Cũng có thể chứng minh $g(i,j)$ là số lần tối thiểu chia đôi đồng thời $i$ và $j$ để được hai số chẵn, tức số lượng bit $1$ ở phần đuôi của $(i|j)$．Do đó có thể dùng `__builtin_ctz(~(i | j))`．
    
    Với dạng bài này, chỉ cần bấm bảng để đoán công thức SG, rồi có thể chứng minh quy nạp．Ví dụ, đặt $S_k$ là tập SG của các trạng thái nhận được khi chia $k$ viên thành hai đống không rỗng, thì $f(i,j) = \operatorname{mex}(S_i \cup S_j)$, nên $S_k$ thỏa：
    
    $$
    S_k = \{\operatorname{mex}(S_i \cup S_j) : i + j = k,~i,j\in\mathbf N_+\}.
    $$
    
    Cần chứng minh $d\in S_k$ khi và chỉ khi bit $d$ (bit thấp nhất là $0$) trong biểu diễn nhị phân của $(k-1)$ là $1$．
    
    Dùng quy nạp．Cơ sở $S_1=\varnothing$ hiển nhiên．Giả sử đúng với mọi số nhỏ hơn $k$．Khi đó $d\in S_k$ khi và chỉ khi tồn tại $i,j\in\mathbf N_+$ với $i+j=k$ và trong $(i-1)$ hoặc $(j-1)$ có ít nhất một bit $d'<d$ bằng $1$，còn bit $d$ đều bằng $0$．Rõ ràng tồn tại phân tách như vậy khi và chỉ khi xét modulo $2^{d+1}$ thì $(k-1)=(i-1)+(j-1)+1$ nằm trong $[2^d,2^{d+1}-1)$, tương đương bit $d$ của $(k-1)$ là $1$．Suy ra quy nạp đúng．

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/impartial-game/impartial-game-1.cpp"
    ```

???+ example "[Luogu P5675 \[GZOI2017\] 取石子游戏](https://www.luogu.com.cn/problem/P5675)"
    Có $n$ đống đá, đống $i$ có $a_i$ viên．Chơi Nim．Bây giờ có thể chỉ định một số đống làm thế cờ ban đầu, và chỉ định một đống bắt buộc người đi trước phải lấy ở lượt đầu, nhưng không chỉ định số lượng lấy．Hỏi có bao nhiêu cách chỉ định để người đi trước không thể thắng．$n,a_i\le 200$．

??? note "Lời giải"
    Với dạng này, cần dùng kết luận về Nim và kết hợp kiến thức khác．Giả sử bắt buộc lấy ở đống $i$, và Nim tổng của các đống được chọn là $v$，thì người đi trước không thể thắng khi và chỉ khi $a_i \le a_i\oplus v$，tức $a_i$ không vượt quá Nim tổng của các đống còn lại $a_i\oplus v$．Vì giới hạn nhỏ, có thể duyệt đống bắt buộc $i$；khi cố định $i$, số cách chọn các đống còn lại để đạt Nim tổng khác nhau có thể tính bằng DP, rồi cộng các trường hợp Nim tổng $\ge a_i$．

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/impartial-game/impartial-game-2.cpp"
    ```

???+ example "[Luogu P2599 \[ZJOI2009\] 取石子游戏](https://www.luogu.com.cn/problem/P2599)"
    Có $n$ đống đá, đống $i$ có $a_i$ viên．Hai người luân phiên lấy đá, mỗi lần chỉ được chọn một trong hai đống ở biên trái hoặc biên phải và lấy bất kỳ số viên, không được bỏ lượt．Người lấy viên cuối cùng thắng．Hỏi người đi trước có thắng không．

??? note "Lời giải"
    Bài này không có các tiểu trò chơi độc lập, về nguyên tắc chỉ dùng [bổ đề判定 thắng/thua](#博弈图和状态)．Phân tích từ trường hợp đơn giản．Khi $n\le 2$ thì là Nim．Khi $n\ge 3$ thì phức tạp．Vì chỉ thao tác hai đống biên, gọi số viên ở hai biên là $x$ và $y$．Đặt $f(x,y)$ là hàm chỉ báo trạng thái thắng cho người đi trước: thắng thì $f(x,y)=1$, ngược lại $0$．Ta có quan hệ：$f(x,y)=0$ khi và chỉ khi với mọi $s < x$ và $t < y$ đều có $f(x,t)=f(s,y)=1$．Điểm khởi đầu ở $x=0$ hoặc $y=0$，lúc đó còn lại ít hơn $n$ đống nên cần xem xét các đống giữa．Vì vậy tạm giả sử đã biết $f(x,0)$ và $f(0,y)$, xét cách suy ra mọi $f(x,y)$．Điều này không khó．Xét ma trận vô hạn chỉ số $\mathbf N\times\mathbf N$，điền $0$ và $1$ sao cho mỗi hàng và mỗi cột最多 một $0$, và nếu trước đó trong cùng hàng/cột chưa có $0$ thì vị trí đó phải là $0$．Vị trí $0$ của mỗi hàng định nghĩa một hàm từ $x$ sang $y$．Bấm vài ví dụ sẽ thấy: nếu $x_0$ là giá trị duy nhất sao cho $f(x,0)=0$ và $y_0$ là giá trị duy nhất sao cho $f(0,y)=0$，thì với mọi $x$，$f(x,y)=0$ khi
    
    $$
    y = \begin{cases}
    0, & x = x_0,\\
    x - 1, & x_0 < x < y_0,\\
    x + 1, & y_0 < x < x_0,\\
    x, & \text{otherwise}.
    \end{cases}
    $$
    
    Nghĩa là chỉ cần biết $x_0$ và $y_0$ là tính được $f(x,y)$ trong $O(1)$．Còn $x_0$ và $y_0$ có thể tính đệ quy．Ví dụ, $x_0$ là nghiệm duy nhất của $f(x,0)=0$，mà $f(x,0)$ lại có thể tính bằng cách bỏ đống phải nhất và chỉ xét $n-1$ đống còn lại；tức xét $f_{1,n-1}(x,y)$ thì $f(x,0)=f_{1,n-1}(x,a_{n-1})$；tương tự, bỏ đống trái nhất得到 $f_{2,n}(x,y)$ rồi $f(0,y)=f_{2,n}(a_1,y)$．Hàm bên trong lại phụ thuộc vào lớp sâu hơn, là [DP đoạn](../../dp/interval.md)．Mỗi tầng chỉ cần duy trì $x_0$ và $y_0$ tương ứng．

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/impartial-game/impartial-game-3.cpp"
    ```

## Bài tập

Trước hết là các bài mẫu, áp dụng trực tiếp kết luận của trang：

-   [Luogu P2197【模板】Nim 游戏](https://www.luogu.com.cn/problem/P2197)
-   [Luogu P2252 \[SHOI2002\] 取石子游戏](https://www.luogu.com.cn/problem/P2252)
-   [Luogu P2594 \[ZJOI2009\] 染色游戏](https://www.luogu.com.cn/problem/P2594)
-   [Luogu P3185 \[HNOI2007\] 分裂游戏](https://www.luogu.com.cn/problem/P3185)
-   [Luogu P3480 \[POI 2009\] KAM-Pebbles](https://www.luogu.com.cn/problem/P3480)
-   [Luogu P4101 \[HEOI2014\] 人人尽说江南好](https://www.luogu.com.cn/problem/P4101)
-   [Luogu P4279 \[SHOI2008\] 小约翰的游戏](https://www.luogu.com.cn/problem/P4279)
-   [Luogu P6487 \[COCI 2010/2011 #4\] HRPA](https://www.luogu.com.cn/problem/P6487)
-   [Luogu P6560 \[SBCOI2020\] 时光的流逝](https://www.luogu.com.cn/problem/P6560)
-   [Luogu P7589 黑白棋（2021 CoE-II B）](https://www.luogu.com.cn/problem/P7589)
-   [AtCoder Regular Contest 168 B - Arbitrary Nim](https://atcoder.jp/contests/arc168/tasks/arc168_b)

Tiếp theo là các bài đòi hỏi tư duy hoặc tổng hợp mạnh hơn：

-   [Luogu P2490 \[SDOI2011\] 黑白棋](https://www.luogu.com.cn/problem/P2490)
-   [Luogu P3179 \[HAOI2015\] 数组游戏](https://www.luogu.com.cn/problem/P3179)
-   [Luogu P5363 \[SDOI2019\] 移动金币](https://www.luogu.com.cn/problem/P5363)
-   [Luogu P5970 \[POI 2016\] Nim z utrudnieniem](https://www.luogu.com.cn/problem/P5970)
-   [Luogu P6791 \[SNOI2020\] 取石子](https://www.luogu.com.cn/problem/P6791)
-   [Luogu P7864「EVOI-RD1」摘叶子](https://www.luogu.com.cn/problem/P7864)
-   [Luogu P8347「Wdoi-6」另一侧的月](https://www.luogu.com.cn/problem/P8347)
-   [AtCoder Grand Contest 002 E - Candy Piles](https://atcoder.jp/contests/agc002/tasks/agc002_e)
-   [AtCoder Grand Contest 010 F - Tree Game](https://atcoder.jp/contests/agc010/tasks/agc010_f)
-   [AtCoder Grand Contest 017 D - Game on Tree](https://atcoder.jp/contests/agc017/tasks/agc017_d)
-   [AtCoder Beginner Contest 278 G - Generalized Subtraction Game](https://atcoder.jp/contests/abc278/tasks/abc278_g)
-   [SPOJ COT3 - Combat on a tree](https://www.spoj.com/problems/COT3/)
-   [Codeforces 494 E. Sharti](https://codeforces.com/problemset/problem/494/E)
-   [Codeforces 1149 E. Election Promises](https://www.luogu.com.cn/problem/CF1149E)
-   [Codeforces 1451 F. Nullify The Matrix](https://codeforces.com/problemset/problem/1451/F)
-   [Codeforces 1704 F. Colouring Game](https://codeforces.com/problemset/problem/1704/F)

Cuối cùng là các bài về trò chơi đồ thị hai phía：

-   [Luogu P4136 谁能赢呢？](https://www.luogu.com.cn/problem/P4136)
-   [Luogu P4617 \[COCI 2017/2018 #5\] Planinarenje](https://www.luogu.com.cn/problem/P4617)
-   [Luogu P4055 \[JSOI2009\] 游戏](https://www.luogu.com.cn/problem/P4055)
-   [Luogu P1971 \[NOI2011\] 兔兔与蛋蛋游戏](https://www.luogu.com.cn/problem/P1971)
-   [Codeforces 1147 F. Zigzag Game](https://codeforces.com/problemset/problem/1147/F)

## Tài liệu tham khảo và chú thích

-   [（转载）Nim 游戏博弈（收集完全版）by exponent - 博客园](http://www.cnblogs.com/exponent/articles/2141477.html)
-   [\[组合游戏与博弈论\]【学习笔记】by Candy? - 博客园](https://www.cnblogs.com/candy99/p/6548836.html)
-   [Nim - Wikipedia](https://en.wikipedia.org/wiki/Nim)
-   [Sprague–Grundy theorem - Wikipedia](https://en.wikipedia.org/wiki/Sprague%E2%80%93Grundy_theorem)
-   [Nimber - Wikipedia](https://en.wikipedia.org/wiki/Nimber)
-   [Beatty Sequence - Wikipedia](https://en.wikipedia.org/wiki/Beatty_sequence)
-   [Games on arbitrary graphs - CP Algorithms](https://cp-algorithms.com/game_theory/games_on_graphs.html)
-   [算法学习笔记（74): 二分图博弈 by Pecco - 知乎](https://zhuanlan.zhihu.com/p/359334008)
-   Conway, John H. On numbers and games. AK Peters/CRC Press, 2000.
-   Berlekamp, Elwyn R., John H. Conway, and Richard K. Guy. Winning ways for your mathematical plays, volume 1-4. AK Peters/CRC Press, 2001-2004.

[^n-vs-p]: 「$\mathcal N$ trạng thái」 và 「$\mathcal P$ trạng thái」 lần lượt biểu thị 「người chơi tiếp theo thắng」（Next player wins） và 「người chơi trước đó thắng」（Previous player wins）．

[^more-sums]: 「Tổng」 trong bài là **luật dài**（long rule） của **tổng rời rạc**（disjunctive sum）．Đây là cách kết hợp phổ biến nhất．Ngoài ra còn có các cách kết hợp khác, xem Conway, John H. On numbers and games. AK Peters/CRC Press, 2000, chương 14．
