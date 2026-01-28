tác giả: Hope666666

## Giới thiệu

Bài viết này sẽ giới thiệu tư tưởng DP lồng DP (DP lồng DP) và thông qua hai ví dụ để minh họa cách áp dụng nó vào các bài toán cụ thể.

## Tư tưởng

Cái gọi là "DP lồng DP" thực chất chỉ việc trong quá trình quy hoạch động, ta trừu tượng hóa quá trình giải một bài toán con (thường là một DP) thành một máy tự động (DFA), sau đó thiết kế một lớp DP mới dựa trên máy tự động này.

Kỹ thuật này chủ yếu được áp dụng cho lớp bài toán **đếm dãy**, **xác suất** hoặc **kỳ vọng**. Một bài toán điển hình có cấu trúc như sau:

-   Cho bảng chữ cái $\Sigma$ và một tập hợp các "dãy hợp lệ" độ dài $n$ trên đó là $A \subseteq \Sigma^n$. Tùy vào bảng chữ cái, dãy có thể là chuỗi nhị phân, chuỗi số, dãy trạng thái, v.v.
-   Đối với mỗi dãy cụ thể $s \in \Sigma^n$, có thể dùng quy hoạch động để kiểm tra xem nó có hợp lệ hay không (tức là $s \in A$), tính trọng số của nó hoặc tìm các giá trị liên quan.
-   Cuối cùng, chúng ta muốn thống kê số lượng tất cả các dãy trong tập $A$, tổng trọng số, giá trị kỳ vọng, v.v.

Lúc này, việc liệt kê tất cả các dãy là không khả thi. Do đó, chúng ta cân nhắc trừu tượng hóa quá trình "kiểm tra một dãy có hợp lệ hay không" (tức là DP lớp trong) thành một [máy tự động hữu hạn đơn định](../misc/fsm.md#máy-tự-động-hữu-hạn-đơn-định) (DFA). Thông thường, đối với dãy $s \in \Sigma^n$ cố định, hàm trạng thái của DP lớp trong có thể biểu diễn là $g(i, x; s)$, tức là đã xử lý xong tiền tố độ dài $i$ của dãy $s$, và giá trị của một đại lượng nào đó khi các thành phần trạng thái khác là $x$. Tương ứng, phương trình chuyển trạng thái của DP lớp trong là:

$$
g(i,\cdot;s) = G(g(i-1,\cdot;s),s_i).
$$

Nói cách khác, hàm $g(i, \cdot; s)$ được xác định duy nhất bởi hàm $g(i-1, \cdot; s)$ trước đó và ký tự hiện tại $s_i$. Nếu coi hàm $g(i, \cdot; s)$ là một trạng thái của máy tự động, thì phương trình chuyển trạng thái của DP lớp trong sẽ đưa ra phép chuyển trạng thái của máy tự động. Do đó, máy tự động $(Q, \Sigma, \delta, q_0, F)$ tương ứng với DP lớp trong có cấu trúc như sau:

-   Tập hợp trạng thái $Q$ là tập hợp tất cả các hàm $g(i, \cdot; s)$ khả dĩ tương ứng với các $s \in \Sigma^n$ và $i = 0, 1, \dots, n$;
-   Hàm chuyển $\delta: Q \times \Sigma \to Q$ chính là hàm $G$ trong phương trình chuyển trạng thái của DP lớp trong;
-   Trạng thái bắt đầu $q_0$ thường là hiển nhiên, tức là trạng thái ban đầu của DP lớp trong;
-   Tập hợp trạng thái chấp nhận $F$ tương ứng với tất cả các dãy hợp lệ $s \in A$.

Bản thân hàm $g(i, \cdot; s)$ có thể khá phức tạp, vì vậy khi xử lý các bài toán cụ thể, thường cần thực hiện [nén trạng thái](./state.md) hoặc kết hợp với các kỹ thuật tối ưu hóa DFA để nén không gian trạng thái. Đây cũng là lý do chính khiến DP lồng DP có thể giảm đáng kể độ phức tạp về thời gian và không gian so với DP vét cạn.

Sau khi trừu tượng hóa DP lớp trong thành DFA, ta có thể thiết kế một DP mới trên DFA này để giải quyết bài toán gốc, gọi là DP lớp ngoài. Để thuận tiện, lấy bài toán đếm đơn giản làm ví dụ. Hàm trạng thái của DP lớp ngoài được định nghĩa là $f(i, q)$, tức là số lượng tiền tố độ dài $i$ dẫn đến trạng thái $q \in Q$ trong DFA. Phương trình chuyển trạng thái của nó là:

$$
f(i,q) = \sum_{c\in\Sigma}\sum_{q'\in Q:\delta(q',c)=q} f(i-1,q').
$$

Trạng thái bắt đầu đương nhiên là $f(0, q_0)$, và đáp án cuối cùng thường có thể tính toán đơn giản từ $\{f(n, q): q \in F\}$. DP lớp ngoài thực chất là một trường hợp đặc biệt của [DP trên DAG](./dag.md).

## Ví dụ

Hai ví dụ tiếp theo sẽ giải thích chi tiết cách làm chung của DP lồng DP.

### Ví dụ 1

???+ example "[Hero meet devil](https://www.luogu.com.cn/problem/P10614)"
    Cho một xâu $S$ có bảng chữ cái là `ACGT`, và $|S| \le 15$. Với mỗi $0 \le i \le |S|$, hãy tìm xem có bao nhiêu xâu $T$ độ dài $m$ trên bảng chữ cái `ACGT` sao cho độ dài dãy con chung dài nhất của nó với $S$ bằng $i$.

??? note "Lời giải"
    Đầu tiên ta nghĩ đến một DP: Gọi $f_{i,j}$ là số phương án cho xâu $T$ độ dài $i$ có độ dài dãy con chung dài nhất với $S$ là $j$. Nhưng cách này không chuyển trạng thái được, vấn đề chính là ta không biết dãy con chung dài nhất đó ứng với những ký tự nào.
    
    Xét quá trình tìm dãy con chung dài nhất (LCS) thông thường. Gọi $g_{i,j}$ là độ dài LCS của $i$ ký tự đầu của $T$ và $j$ ký tự đầu của $S$, ta có:
    
    $$
    g_{i,j} = \max\{g_{i-1,j},g_{i,j-1},g_{i-1,j-1}+[T_i=S_j]\}.
    $$
    
    Ta nhận thấy rằng, với một giá trị $i$, chỉ cần ghi lại giá trị của từng vị trí trong mảng một chiều $g_i$ là có thể duy trì chính xác trạng thái LCS của $S$ và $i$ ký tự đầu của $T$. Vì độ dài $S$ chỉ tối đa 15, tư tưởng này là khả thi.
    
    Vì vậy, thiết lập lại trạng thái $f_{i,x}$ là số phương án cho xâu $T$ độ dài $i$ có trạng thái mảng DP của $S$ (chính là $g_i$) là $x$. DP này có vẻ có rất nhiều trạng thái, tuy nhiên ta thấy $g_{i,j} - g_{i,j-1} \in \{0, 1\}$, nên có thể duy trì mảng hiệu phân (difference array) của $g_i$, số trạng thái sẽ là $2^{|S|}$.
    
    Bây giờ suy nghĩ cách chuyển trạng thái. Dễ thấy nếu biết mảng $g_i$ và biết $T_{i+1}$, ta có thể thông qua chuyển trạng thái LCS thông thường (phương trình DP nêu trên) để tìm ra $g_{i+1}$. Do đó, LCS thông thường trở thành DP lớp trong hỗ trợ chuyển trạng thái cho $f$.
    
    Vì vậy, ta duyệt qua $T_{i+1}$, tính toán trạng thái $x'$ sau khi chuyển từ $x$, rồi cộng $f_{i,x}$ vào $f_{i+1,x'}$. Cuối cùng, ta ghi lại $\textit{ans}_i$ là đáp án cho độ dài LCS bằng $i$, duyệt qua mỗi trạng thái $S$, cộng $f_{m,S}$ vào $\textit{ans}_{\operatorname{popcount}(S)}$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/dp-of-dp/dp-of-dp_1.cpp"
    ```

### Ví dụ 2

???+ example "[\[ZJOI2019\] 麻将 (Mạt chược)](https://loj.ac/p/3042)"
    Giả sử mạt chược có $n$ loại quân, mỗi loại có 4 quân. Định nghĩa một "bộ" (mianzi) là ba quân có giá trị liên tiếp $i, i+1, i+2$ (chi/shunzu) hoặc ba quân cùng giá trị $i, i, i$ (pơ/kezu), một "cặp" (duizi) là hai quân cùng giá trị $i, i$. Một dãy quân mạt chược được gọi là "ù" (hu) khi và chỉ khi nó (coi như một đa tập) có thể chia thành bốn bộ và một cặp, hoặc bảy cặp khác nhau. Cho trước 13 quân, hỏi kỳ vọng cần bốc thêm bao nhiêu quân nữa để thỏa mãn điều kiện tồn tại một dãy con "ù".

??? note "Lời giải"
    Trước hết, đối với một bộ bài, ta chỉ cần quan tâm đến số lượng mỗi loại quân mà không cần để ý đến thứ tự của chúng. Do đó, với bất kỳ tiền tố nào của bộ bài, ta đều có thể chuyển nó thành một dãy độ dài $n$, mỗi vị trí có giá trị từ $0 \sim 4$. Ban đầu, nếu số lượng quân thứ $i$ là $a_i$, tương đương với việc giới hạn giá trị $x_i$ tại vị trí thứ $i$ trong dãy nằm trong đoạn $[a_i, 4]$. Lưu ý, dãy sau khi chuyển đổi không xét đến thứ tự bốc quân, nhưng dãy mạt chược trong đề bài thì có xét thứ tự.
    
    Gọi $X$ là số lần bốc quân tối thiểu để có thể "ù". Việc tính trực tiếp kỳ vọng $\mathbf E[X]$ khá khó khăn, ta cân nhắc chuyển đổi như sau. Gọi $h_i$ là số lượng dãy gồm $i$ quân bốc thêm mà **chưa ù**. Vì trong dãy mạt chược tương ứng, $i$ quân này chắc chắn đứng trước $(4n-13-i)$ quân còn lại, nhưng thứ tự giữa $i$ quân này và thứ tự giữa các quân còn lại là bất kỳ, nên số lượng dãy mạt chược bốc $i$ quân mà chưa ù là:
    
    $$
    h_i \cdot i!(4n-13-i)!.
    $$
    
    Vì tổng số dãy mạt chược là $(4n-13)!$, nên xác suất bốc $i$ quân mà chưa ù là:
    
    $$
    \mathbf P[X>i] = \dfrac{h_i\cdot i!(4n-13-i)!}{(4n-13)!}.
    $$
    
    Sử dụng công thức tổng đuôi (tail sum formula), ta có kỳ vọng cần tìm:
    
    $$
    \mathbf E[X] = \sum_{i=0}^\infty\mathbf P[X>i] = 1 + \sum_{i=1}^{4n-13}\dfrac{h_i\cdot i!(4n-13-i)!}{(4n-13)!}.
    $$
    
    Đến đây, bài toán chuyển thành cách tính $h_i$. Ta sử dụng phương pháp DP lồng DP.
    
    Đầu tiên xét DP lớp trong, tức là dùng DP để kiểm tra một dãy (sau chuyển đổi) có tương ứng với một bộ bài "ù" hay không. Trường hợp bảy cặp khá dễ, trọng tâm là hình thức "ù" thứ nhất. Để làm việc này, gọi $g_{0/1,i,j,k}$ là số lượng bộ tối đa sau khi xử lý xong $i$ loại quân đầu tiên, còn thừa $j$ nhóm $(i-1, i)$ và $k$ quân $i$, và đã có/chưa có cặp (tương ứng $1/0$). Nếu thực hiện DP trên một dãy, cuối cùng $g_{1,n}$ chứa một số lớn hơn hoặc bằng 4 thì dãy đó là "ù".
    
    Chuyển trạng thái của DP này khá phức tạp. Ta chia làm hai bước. Bước 1, xét chuyển trạng thái cho $g_{0/1,i}$. Điều này tương đương với việc nếu thêm $x_i$ quân loại $i$ vào bộ bài hiện tại nhưng không tạo cặp mới, số lượng bộ sẽ thay đổi thế nào. Hiển nhiên, nếu muốn sau khi thêm $x_i$ quân loại $i$ sẽ có $\ell$ bộ dây (shunzu), $j$ nhóm $(i-1, i)$ và $k$ quân $i$ riêng lẻ, thì ta nên chuyển từ $(g_{0/1,i-1})_{\ell,j}$ sang (lựa chọn này tránh lãng phí nhất), và dùng số quân còn lại $(x_i-\ell-j-k)$ để tạo ra nhiều bộ ba (kezu) nhất có thể. Liệt kê mọi khả năng, ta có phương trình:
    
    $$
    \tilde G(g_{0/1,i-1}, x_i)_{j,k} = \max\left\{(g_{0/1,i-1})_{\ell,j} + \ell + \left\lfloor\dfrac{x_i-\ell-j-k}{3}\right\rfloor:\ell+j+k\le x_i\right\}.
    $$
    
    Bước 2, xét trường hợp cần tạo cặp. Khi thêm $x_i$ quân loại $i$, có ba loại chuyển trạng thái:
    
    -   Chuyển từ $g_{0,i-1}$ thêm $x_i$ quân sang $g_{0,i}$;
    -   Chuyển từ $g_{1,i-1}$ thêm $x_i$ quân sang $g_{1,i}$;
    -   Nếu $x_i \ge 2$, chuyển từ $g_{0,i-1}$ thêm $x_i-2$ quân sang $g_{1,i}$.
    
    Từ đó, ta có toàn bộ các phép chuyển từ $g_{i-1}$ thêm $x_i$ quân để được $g_i$.
    
    Giải quyết xong chuyển trạng thái DP lớp trong, ta có thể xây dựng **máy tự động ù mạt chược**. Các phép chuyển của máy tự động chính là các phép chuyển DP lớp trong nêu trên, đồng thời cần cân nhắc cách biểu diễn mỗi trạng thái. Mỗi trạng thái tương ứng với một bộ giá trị khả dĩ của $g_i$. Nó có ba chiều $(0/1, j, k)$. Vì các giá trị $j$ và $k$ dùng để tạo bộ dây trong tương lai, mà ba bộ dây giống nhau luôn có thể chuyển thành ba bộ ba, nên chỉ cần xét nhu cầu tạo không quá 2 bộ dây giống nhau, mỗi loại quân chỉ cần giữ lại không quá 2 quân, tức $j, k \in \{0, 1, 2\}$. Do đó, $g_i$ có thể biểu diễn bằng mảng $2 \times 3 \times 3$. Ngoài ra, để duy trì hình thức "ù" bảy cặp, cần thêm một biến đếm cho mỗi trạng thái để biểu thị số cặp tối đa hiện có.
    
    Mỗi phần tử trong mảng $g_i$ có thể nhận giá trị trong $\{-\infty\} \cup \mathbf N$, nhưng vì số bộ $\ge 4$ là đã "ù", nên có thể giới hạn mỗi phần tử không quá 4. Vì một dãy đã "ù" thêm bất kỳ quân nào vẫn là "ù", ta có thể dùng tư tưởng tối thiểu hóa DFA để nén tất cả các trạng thái "ù" thành một trạng thái duy nhất. Do đó, đối với trạng thái chưa "ù", thực tế mỗi vị trí chỉ cần xét giá trị trong $\{-\infty\} \cup \{0, 1, 2, 3\}$. Khi cài đặt, $-\infty$ dùng $-1$.
    
    Mặc dù vậy, số lượng trạng thái khả dĩ vẫn rất lớn, tổng cộng $1 + 7 \times 5^{18}$. Liệt kê chúng là không thực tế. Thực tế, đại đa số các khả năng này không thực sự xuất hiện trong một máy tự động "ù". Để tránh xét các trạng thái không tồn tại, ta dùng tư tưởng BFS, bắt đầu từ trạng thái ban đầu, mở rộng từng bước cho đến khi dừng ở trạng thái "ù". Máy tự động thu được có $N = 2092$ trạng thái.
    
    Cuối cùng, xét DP trên máy tự động (DP lớp ngoài). Gọi $f_{i,j,k}$ là số lượng dãy đã xử lý đến loại quân thứ $i$, bốc tổng cộng $j$ quân, và đang ở trạng thái thứ $k$ trên máy tự động. Khi chuyển trạng thái, duyệt số quân bốc thêm $0 \le t \le 4-a_i$ ($a_i$ là số quân loại $i$ đã có trong 13 quân ban đầu), nhân số dãy trước đó với số cách chọn $t$ quân từ $4-a_i$ quân là $\dbinom{4-a_i}{t}$, rồi cộng dồn lại. Một cách hình thức:
    
    $$
    f_{i+1,j+t,k'} = \sum_{t=0}^{4-a_i}\dbinom{4-a_i}{t}f_{i,j,k}.
    $$
    
    Trong đó $k' = \delta(k, a_i+t)$ là trạng thái sau khi thêm $a_i+t$ quân vào trạng thái $k$. Sau khi kết thúc DP lớp ngoài, ta có thể tính được số lượng dãy bốc $i$ quân mà vẫn chưa "ù":
    
    $$
    h_i = \sum_{j=1}^N f_{n,i,j}.
    $$
    
    Thay vào biểu thức kỳ vọng ở trên để được kết quả.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/dp/code/dp-of-dp/dp-of-dp_2.cpp"
    ```

## Bài tập

-   [CF979E Kuro and Topological Parity](https://codeforces.com/problemset/problem/979/E)
-   [\[TJOI2018\] 游园会 (Hội hoa viên)](https://loj.ac/p/2575)
-   [\[NOI2022\] 移除石子 (Loại bỏ sỏi)](https://loj.ac/p/3848)