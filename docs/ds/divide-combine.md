## Về các đoạn

Chúng ta bắt đầu bằng một vấn đề khá đơn giản:

> Cho một hoán vị từ $1-n$, một đoạn có miền giá trị liên tiếp được gọi là một **đoạn (segment)**. Hỏi số lượng đoạn trong một hoán vị. Ví dụ: $\{5 ,3 ,4, 1 ,2\}$ có các đoạn là: $[1,1],[2,2],[3,3],[4,4],[5,5],[2,3],[4,5],[1,3],[2,5],[1,5]$.

Nhìn vào điều này, ta có cảm giác cần duy trì tập hợp miền giá trị của một đoạn, độ phức tạp có vẻ không thân thiện. Cây đoạn có thể truy vấn xem một đoạn có phải là đoạn hay không, nhưng không dễ thống kê số lượng đoạn.

Ở đây chúng ta giới thiệu cấu trúc dữ liệu thần kỳ này - Cây Phân Tích Hợp (析合树 - Divisor-Combinator Tree/Segment Tree)!

## Đoạn Liên Tiếp (Continuous Segment)

Trước khi giới thiệu Cây Phân Tích Hợp, chúng ta đưa ra một số điều kiện tiên quyết. Vì định nghĩa trong tài liệu về LCA khó hiểu, ở đây đưa ra các định nghĩa không hoàn toàn chặt chẽ (nhưng dễ hiểu hơn) để độc giả dễ tiếp thu.

### Hoán vị và Đoạn Liên Tiếp

**Hoán vị**: Định nghĩa hoán vị cấp $n$, $P$, là một dãy có kích thước $n$, sao cho $P_i$ lần lượt nhận các giá trị $1, 2, \ldots, n$. Nói một cách hình thức hơn, hoán vị cấp $n$, $P$, là một tập hợp có thứ tự thỏa mãn:

1.  $|P|=n$.
2.  $\forall i, P_i \in [1,n]$.
3.  $\nexists i, j \in [1,n], P_i=P_j$.

**Đoạn Liên Tiếp**: Đối với hoán vị $P$, đoạn liên tiếp $(P,[l,r])$ biểu thị một đoạn $[l,r]$ sao cho tập giá trị của $P_{l\sim r}$ là liên tiếp. Nói một cách hình thức hơn, đối với hoán vị $P$, đoạn liên tiếp biểu thị một đoạn $[l,r]$ thỏa mãn:

$$
(\nexists\ x,z\in[l,r],y\notin[l,r],\ P_x<P_y<P_z)
$$

Đặc biệt, khi $l>r$, ta coi đây là một đoạn rỗng, ký hiệu là $(P,\varnothing)$.

Tập hợp tất cả các đoạn liên tiếp của hoán vị $P$ được gọi là $I_P$, và ta coi $(P,\varnothing)\in I_P$.

### Phép toán của Đoạn Liên Tiếp

Đoạn liên tiếp phụ thuộc vào đoạn chỉ số và định nghĩa miền giá trị, vì vậy ta có thể định nghĩa các phép toán giao, hợp, trừ của đoạn liên tiếp.

Định nghĩa $A=(P,[a,b]), B=(P,[x,y])$, và $A,B\in I_P$. Các quan hệ và phép toán của đoạn liên tiếp có thể biểu diễn như sau:

1.  $A\subseteq B\iff x\le a\wedge b\le y$.
2.  $A=B\iff a=x\wedge b=y$.
3.  $A\cap B=(P,[\max(a,x),\min(b,y)])$.
4.  $A\cup B=(P,[\min(a,x),\max(b,y)])$.
5.  $A\setminus B=(P,\{i|i\in[a,b]\wedge i\notin[x,y]\})$.

Thực ra các phép toán này chỉ là phép toán giao, hợp, trừ tập hợp thông thường trên các đoạn chỉ số.

### Tính chất của Đoạn Liên Tiếp

Một số tính chất hiển nhiên của đoạn liên tiếp. Ta định nghĩa $A,B\in I_P, A \cap B \neq \varnothing, A \notin B, B \notin A$, thì có $A\cup B,A\cap B,A\setminus B,B\setminus A\in I_P$.

Chứng minh? Bản chất của chứng minh là các phép toán giao, hợp, trừ tập hợp.

## Cây Phân Tích Hợp (Divisor-Combinator Tree)

Được rồi, đến phần trọng tâm. Bạn có thể đoán được, Cây Phân Tích Hợp được tạo thành từ các đoạn liên tiếp. Nhưng ta biết rằng một hoán vị có thể có tới $O(n^2)$ đoạn liên tiếp, vì vậy chúng ta cần rút ra những đoạn liên tiếp cơ bản hơn để tạo thành Cây Phân Tích Hợp.

### Đoạn Nguyên Thủy (Primitive Segment)

Thực ra tên đầy đủ là **Đoạn Liên Tiếp Nguyên Thủy (Primitive Continuous Segment)**. Nhưng người viết cho rằng Đoạn Nguyên Thủy (Primitive Segment) ngắn gọn hơn.

Đối với hoán vị $P$, ta cho rằng một đoạn nguyên thủy $M$ biểu thị một đoạn liên tiếp trong tập hợp $I_P$ mà không tồn tại đoạn liên tiếp nào giao với nó mà không chứa nó. Định nghĩa hình thức, ta cho rằng $X\in I_P$ và thỏa mãn $\forall A\in I_P,\ X\cap A= (P,\varnothing)\vee X\subseteq A\vee A\subseteq X$.

Tập hợp tất cả các đoạn nguyên thủy là $M_P$. Hiển nhiên, $(P,\varnothing)\in M_P$.

Rõ ràng, giữa các đoạn nguyên thủy chỉ có quan hệ rời nhau hoặc bao hàm. Và bạn nhận ra **một đoạn liên tiếp có thể được cấu thành từ một vài đoạn nguyên thủy không giao nhau**. Đoạn nguyên thủy lớn nhất chính là toàn bộ hoán vị, nó bao hàm tất cả các đoạn nguyên thủy khác, vì vậy ta cho rằng các đoạn nguyên thủy có thể tạo thành một cấu trúc cây, cấu trúc này gọi là **Cây Phân Tích Hợp**. Nghiêm ngặt hơn, Cây Phân Tích Hợp của hoán vị $P$ được tạo thành từ **tất cả các đoạn nguyên thủy** của hoán vị $P$.

Nói nhiều định nghĩa khô khan như vậy mà không có hình vẽ thì sao được. Xét hoán vị $P=\{9,1,10,3,2,5,7,6,8,4\}$. Cây Phân Tích Hợp tạo bởi các đoạn nguyên thủy của nó như sau:

![p1](./images/div-com1.png)

Trong hình, chúng ta không đánh dấu các đoạn nguyên thủy. Mà **mỗi nút trong hình đại diện cho một đoạn nguyên thủy**. Chúng ta chỉ đánh dấu miền giá trị của mỗi đoạn nguyên thủy. Ví dụ, nút $[5,8]$ đại diện cho đoạn nguyên thủy $(P,[6,9])=\{5,7,6,8\}$. Vậy ở đây có một vấn đề: **Thế nào là nút Hợp (Combinator Node) và nút Phân Tích (Divisor Node)?**

### Nút Phân Tích và Nút Hợp

Ở đây chúng ta đưa ra định nghĩa trực tiếp, lát nữa sẽ thảo luận về tính đúng đắn của nó.

1.  **Miền giá trị (Value Range)**: Đối với một nút $u$, ký hiệu $[u_l,u_r]$ biểu thị miền giá trị của nút này.
2.  **Dãy con (Son Sequence)**: Đối với một nút $u$ trên Cây Phân Tích Hợp, giả sử các nút con của nó là một dãy **có thứ tự**, dãy này chứa các miền giá trị (một số đơn lẻ $x$ có thể được hiểu là đoạn $[x,x]$). Ta gọi dãy này là dãy con. Ký hiệu là $S_u$.
3.  **Hoán vị con (Son Permutation)**: Đối với một dãy con $S_u$, hoán vị được tạo thành sau khi rời rạc hóa các phần tử của nó thành các số nguyên dương được gọi là hoán vị con. Ví dụ, đối với nút $[5,8]$, dãy con của nó là $\{[5,5],[6,7],[8,8]\}$, nếu đánh số theo thứ tự sắp xếp các đoạn, thì hoán vị con của nó là $\{1,2,3\}$; tương tự, nút $[4,8]$ có hoán vị con là $\{2,1\}$. Hoán vị con của nút $u$ ký hiệu là $P_u$.
4.  **Nút Hợp (Combinator Node)**: Ta cho rằng các nút có hoán vị con là tuần tự hoặc nghịch đảo là nút hợp. Nói hình thức: các nút thỏa mãn $P_u=\{1,2,\cdots,|S_u|\}$ hoặc $P_u=\{|S_u|,|S_u-1|,\cdots,1\}$ được gọi là nút hợp. **Nút lá không có hoán vị con, ta cũng coi nó là nút hợp**.
5.  **Nút Phân Tích (Divisor Node)**: Là nút không phải nút hợp.

Từ hình vẽ có thể thấy, chỉ có $[1,10]$ không phải là nút hợp, vì hoán vị con của $[1,10]$ là $\{3,1,4,2\}$.

### Tính chất của Nút Phân Tích và Nút Hợp

Tên gọi nút Phân Tích và nút Hợp bắt nguồn từ tính chất của chúng. Trước hết ta có một tính chất rất hiển nhiên: đối với bất kỳ nút $u$ nào trên Cây Phân Tích Hợp, hợp của các đoạn chỉ số của các nút con chính là đoạn chỉ số của nút $u$. Tức là $\bigcup_{i=1}^{|S_u|}S_u[i]=[u_l,u_r]$.

Đối với một **nút hợp** $u$: Bất kỳ **đoạn con** nào của dãy con đều tạo thành một **đoạn liên tiếp**. Nói hình thức: $\forall S_u[l\sim r]$, thì $\bigcup_{i=l}^rS_u[i]\in I_P$.

Đối với một **nút phân tích** $u$: Bất kỳ đoạn con nào của dãy con có **độ dài lớn hơn 1** (độ dài ở đây là số phần tử trong dãy con, không phải độ dài đoạn chỉ số) đều **không** tạo thành một **đoạn liên tiếp**. Nói hình thức: $\forall S_u[l\sim r],l<r$, thì $\bigcup_{i=l}^rS_u[i]\notin I_P$.

Tính chất của nút hợp không khó chứng minh. Vì hoán vị con của nút hợp hoặc là thứ tự tăng dần, hoặc là thứ tự giảm dần, và các đoạn giá trị cũng liền kề nhau, nên bất kỳ đoạn con liên tiếp nào cũng là một đoạn liên tiếp.

Đối với tính chất của nút phân tích, có lẽ nhiều độc giả sẽ không hiểu: tại sao **bất kỳ** đoạn con nào có độ dài lớn hơn 1 đều không tạo thành đoạn liên tiếp?

Sử dụng phản chứng. Giả sử đối với một nút $u$, có một đoạn **dài nhất** $S_u[l\sim r]$ tạo thành một đoạn liên tiếp. Khi đó $A=\bigcup_{i=l}^rS_u[i]\in I_P$, điều này có nghĩa là $A$ là một đoạn nguyên thủy! (Vì $A$ là đoạn dài nhất trong các đoạn con, nên không thể tìm được một đoạn liên tiếp giao với nó mà không chứa nó). Khi đó bạn đã không sử dụng tất cả các đoạn nguyên thủy để tạo thành cây này. Mâu thuẫn.

### Xây dựng Cây Phân Tích Hợp

Đối với việc xây dựng Cây Phân Tích Hợp cụ thể, LCA cung cấp một thuật toán tuyến tính[^ref1], dưới đây là một thuật toán $O(n\log n)$ dễ hiểu hơn.

#### Phương pháp Tăng dần (Incremental Method)

Chúng ta xét phương pháp tăng dần. Sử dụng một stack để duy trì rừng Phân Tích Hợp được tạo thành từ $i-1$ phần tử đầu tiên. Ở đây cần **nhấn mạnh** rằng rừng Phân Tích Hợp có nghĩa là, tại bất kỳ thời điểm nào, các nút trong stack hoặc là nút phân tích hoặc là nút hợp. Bây giờ xét nút hiện tại $P_i$.

1.  Ta xét xem nó có thể trở thành con của nút trên đỉnh stack hay không. Nếu có thì nó trở thành con của nút trên đỉnh stack, sau đó lấy nút trên đỉnh stack ra, làm nút hiện tại. Lặp lại quá trình này cho đến khi stack rỗng hoặc không thể trở thành con của nút trên đỉnh stack.
2.  Nếu không thể trở thành con của nút trên đỉnh stack, thì xem xét xem có thể hợp nhất một số nút liên tiếp trên đỉnh stack thành một nút hay không (phương pháp phán đoán hợp nhất ở phần sau), nút được hợp nhất này làm nút hiện tại.
3.  Lặp lại quá trình này cho đến khi không thể tiếp tục. Sau đó kết thúc lần tăng dần này, đẩy nút hiện tại vào stack.

Tiếp theo chúng ta giải thích chi tiết hơn.

#### Chiến lược cụ thể

Ta cho rằng, nếu nút hiện tại có thể trở thành con của nút trên đỉnh stack, thì nút trên đỉnh stack là một **nút hợp**. Nếu là nút phân tích, thì sau khi hợp nhất, nút phân tích này sẽ có một đoạn con liên tiếp, không thỏa mãn tính chất của nút phân tích. Do đó, nó chắc chắn là nút hợp.

Nếu không thể trở thành con của nút trên đỉnh stack, thì ta xem xét xem một số nút liên tiếp trên đỉnh stack có thể hợp nhất với nút hiện tại hay không. Đặt $l$ là mút trái của đoạn mà nút hiện tại nằm trong. Ta tính $L_i$ là giá trị lớn nhất của mút trái $< l$ trong đoạn liên tiếp có mút phải là $i$. Nút hiện tại là $P_i$, nút trên đỉnh stack là $t$.

1.  Nếu $L_i$ không tồn tại, thì rõ ràng nút hiện tại không thể hợp nhất;
2.  Nếu $t_l=L_i$, thì đây là hai nút hợp nhất, nút sau khi hợp nhất là một **nút hợp**;
3.  Ngược lại, chắc chắn tồn tại một nút $t'$ trong stack có mút trái ${t'}_l=L_i$, thì chắc chắn có thể hợp nhất từ nút hiện tại đến $t'$ tạo thành một **nút phân tích**;

#### Phán đoán có thể hợp nhất hay không

Cuối cùng, ta xét cách xử lý $L_i$. Thực tế, một đoạn liên tiếp $(P,[l,r])$ tương đương với việc hiệu cực đại trừ cực tiểu của đoạn bằng độ dài đoạn trừ 1. Tức là

$$
\max_{l\le i\le r}P_i-\min_{l\le i\le r}P_i=r-l
$$

Hơn nữa, do $P$ là một hoán vị, nên đối với bất kỳ đoạn $[l,r]$ nào đều có

$$
\max_{l\le i\le r}P_i-\min_{l\le i\le r}P_i\ge r-l
$$

Vì vậy, chúng ta duy trì $\max_{l\le i\le r}P_i-\min_{l\le i\le r}P_i-(r-l)$, việc tìm một đoạn liên tiếp tương đương với việc truy vấn giá trị nhỏ nhất!

Với ý tưởng trên, không khó để nghĩ ra thuật toán như sau. Đối với quá trình tăng dần hiện tại là $i$, ta duy trì một mảng $Q$ biểu thị hiệu cực đại trừ cực tiểu trừ độ dài cho đoạn $[j,i]$. Tức là

$$
Q_j=\max_{j\le k\le i}P_k-\min_{j\le k\le i}P_k-(i-j),\ \ 0<j<i
$$

Bây giờ chúng ta muốn biết liệu có tồn tại $j$ nhỏ nhất trong $1\sim i-1$ sao cho $Q_j=0$ hay không. Điều này tương đương với việc tìm giá trị nhỏ nhất của $Q_{1\sim i-1}$. Giá trị $j$ nhỏ nhất tìm được chính là $L_i$. Nếu không có, thì $L_i=i$.

Tuy nhiên, khi kết thúc lần tăng dần thứ $i$, ta cần nhanh chóng cập nhật mảng $Q$ sang trường hợp $i+1$. Đoạn ban đầu là $[j,i]$ trở thành $[j,i+1]$, nếu $P_{i+1}>\max$ hoặc $P_{i+1}<\min$ sẽ làm $Q_j$ thay đổi. Thay đổi như thế nào? Nếu $P_{i+1}>\max$, coi như ta lấy $Q_j$ trừ đi $\max$ rồi cộng thêm $P_{i+1}$ là hoàn thành việc cập nhật $Q_j$; $P_{i+1}<\min$ tương tự, coi như $Q_j=Q_j+\min-P_{i+1}$.

Vậy nếu đối với một đoạn $[x,y]$, thỏa mãn $\max$ của các đoạn $P_{x\sim i},P_{x+1\sim i},P_{x+2\sim i},\cdots,P_{y\sim i}$ đều giống nhau thì sao? Bạn đã nhận ra, điều này tương đương với việc ta đang thực hiện thao tác cộng trên đoạn; tương tự, khi $\min$ của các đoạn $P_{x\sim i},P_{x+1\sim i},\cdots,P_{y\sim i}$ cũng là một thao tác cộng trên đoạn. Đồng thời, việc cập nhật $\max$ và $\min$ là độc lập với nhau, do đó có thể cập nhật riêng rẽ.

Do đó, việc duy trì $Q$ có thể được mô tả như sau:

1.  Tìm $j$ lớn nhất sao cho $P_{j}>P_{i+1}$, thì rõ ràng, đoạn $P_{j+1\sim i}$ đều nhỏ hơn $P_{i+1}$, do đó cần cập nhật giá trị lớn nhất của $Q_{j+1\sim i}$. Do $\max(P_i), \max(P_i,P_{i-1}), \max(P_i,P_{i-1},P_{i-2}),\cdots,\max(P_i,P_{i-1},\cdots,P_{j+1})$ là (không nghiêm ngặt) đơn điệu tăng, nên có thể làm cập nhật giống nhau cho mỗi đoạn có $\max$ giống nhau, tức là thao tác cộng trên đoạn.
2.  Cập nhật $\min$ tương tự.
3.  Trừ mỗi $Q_j$ đi $1$. Vì độ dài đoạn tăng thêm $1$.
4.  Truy vấn $L_i$: tức là truy vấn **chỉ số** của giá trị nhỏ nhất trong $Q$.

Đúng vậy, chúng ta có thể sử dụng cây đoạn để duy trì $Q$! Vẫn còn một vấn đề: làm thế nào để tìm một đoạn có $\max/\min$ giống nhau? Sử dụng stack đơn điệu để duy trì! Duy trì hai stack đơn điệu lần lượt cho $\max/\min$. Rõ ràng, $\max/\min$ của các đoạn có đầu mút là hai phần tử liền kề trong stack là giống nhau, do đó trong quá trình duy trì stack đơn điệu, ta đồng thời cập nhật cây đoạn.

Việc duy trì cụ thể xem trong mã nguồn.

Nói nhiều như vậy có vẻ các bạn cũng thấy rối, vậy ta xem hình trước đã. Cảnh báo hình lớn!

![p2](./images/div-com2.jpg)

### Triển khai

Cuối cùng đưa ra một mã nguồn tham khảo. Mã nguồn chuyển từ [Blog của Đại Mễ Bính](https://www.cnblogs.com/Paul-Guderian/p/11020708.html), có thêm một số chú thích.

```cpp
#include <algorithm>
#include <cstdio>
using namespace std;
constexpr int N = 200010;

int n, m, a[N], st1[N], st2[N], tp1, tp2, rt;
int L[N], R[N], M[N], id[N], cnt, typ[N], bin[20], st[N], tp;

// Bài toán gốc của đoạn mã này có lẽ là CERC2017 Intrinsic Interval
// Mảng a chính là hoán vị tương ứng trong bài toán gốc
// st1 và st2 lần lượt là hai stack đơn điệu, tp1, tp2 là đỉnh stack tương ứng, rt là nút gốc của Cây Phân Tích Hợp
// Mảng L, R biểu thị mút trái, phải của miền giá trị nút tương ứng, M có tác dụng được đề cập trong quá trình xây dựng Cây Phân Tích Hợp
// id lưu trữ chỉ số nút tương ứng với vị trí trong hoán vị, typ dùng để đánh dấu nút phân tích hay nút hợp
// st là stack lưu trữ chỉ số nút Cây Phân Tích Hợp, tp là đỉnh của nó
struct RMQ {  // Tiền xử lý RMQ (Max & Min)
  int lg[N], mn[N][17], mx[N][17];

  void chkmn(int& x, int y) {
    if (x > y) x = y;
  }

  void chkmx(int& x, int y) {
    if (x < y) x = y;
  }

  void build() {
    for (int i = bin[0] = 1; i < 20; ++i) bin[i] = bin[i - 1] << 1;
    for (int i = 2; i <= n; ++i) lg[i] = lg[i >> 1] + 1;
    for (int i = 1; i <= n; ++i) mn[i][0] = mx[i][0] = a[i];
    for (int i = 1; i < 17; ++i)
      for (int j = 1; j + bin[i] - 1 <= n; ++j)
        mn[j][i] = min(mn[j][i - 1], mn[j + bin[i - 1]][i - 1]),
        mx[j][i] = max(mx[j][i - 1], mx[j + bin[i - 1]][i - 1]);
  }

  int ask_mn(int l, int r) {
    int t = lg[r - l + 1];
    return min(mn[l][t], mn[r - bin[t] + 1][t]);
  }

  int ask_mx(int l, int r) {
    int t = lg[r - l + 1];
    return max(mx[l][t], mx[r - bin[t] + 1][t]);
  }
} D;

// Duy trì L_i

struct SEG {  // Cây đoạn
#define ls (k << 1)
#define rs (k << 1 | 1)
  int mn[N << 1], ly[N << 1];  // Cộng trên đoạn; Giá trị nhỏ nhất trên đoạn

  void pushup(int k) { mn[k] = min(mn[ls], mn[rs]); }

  void mfy(int k, int v) { mn[k] += v, ly[k] += v; }

  void pushdown(int k) {
    if (ly[k]) mfy(ls, ly[k]), mfy(rs, ly[k]), ly[k] = 0;
  }

  void update(int k, int l, int r, int x, int y, int v) {
    if (l == x && r == y) {
      mfy(k, v);
      return;
    }
    pushdown(k);
    int mid = (l + r) >> 1;
    if (y <= mid)
      update(ls, l, mid, x, y, v);
    else if (x > mid)
      update(rs, mid + 1, r, x, y, v);
    else
      update(ls, l, mid, x, mid, v), update(rs, mid + 1, r, mid + 1, y, v);
    pushup(k);
  }

  int query(int k, int l, int r) {  // Truy vấn vị trí bằng 0
    if (l == r) return l;
    pushdown(k);
    int mid = (l + r) >> 1;
    if (!mn[ls])
      return query(ls, l, mid);
    else
      return query(rs, mid + 1, r);
    // Nếu không tồn tại vị trí bằng 0 sẽ tự động trả về vị trí bạn đang truy vấn
  }
} T;

int o = 1, hd[N], dep[N], fa[N][18];

struct Edge {
  int v, nt;
} E[N << 1];

void add(int u, int v) {  // Thêm cạnh vào cấu trúc cây
  E[o] = Edge{v, hd[u]};
  hd[u] = o++;
}

void dfs(int u) {
  for (int i = 1; bin[i] <= dep[u]; ++i) fa[u][i] = fa[fa[u][i - 1]][i - 1];
  for (int i = hd[u]; i; i = E[i].nt) {
    int v = E[i].v;
    dep[v] = dep[u] + 1;
    fa[v][0] = u;
    dfs(v);
  }
}

int go(int u, int d) {
  for (int i = 0; i < 18 && d; ++i)
    if (bin[i] & d) d ^= bin[i], u = fa[u][i];
  return u;
}

int lca(int u, int v) {
  if (dep[u] < dep[v]) swap(u, v);
  u = go(u, dep[u] - dep[v]);
  if (u == v) return u;
  for (int i = 17; ~i; --i)
    if (fa[u][i] != fa[v][i]) u = fa[u][i], v = fa[v][i];
  return fa[u][0];
}

// Phán đoán đoạn có phải là đoạn liên tiếp hay không
bool judge(int l, int r) { return D.ask_mx(l, r) - D.ask_mn(l, r) == r - l; }

// Xây dựng cây
void build() {
  for (int i = 1; i <= n; ++i) {
    // Stack đơn điệu
    // Trong đoạn [st1[tp1-1]+1,st1[tp1]] thì Min chính là a[st1[tp1]]
    // Bây giờ đẩy nó ra, nghĩa là cần cộng lại Min đã trừ đi.
    // Vị trí lá j của cây đoạn duy trì Max{j,i}-Min{j,i}-(i-j)
    // Cộng trên đoạn chỉ là một Tag.
    // Duy trì stack đơn điệu là để hỗ trợ cập nhật cây đoạn từ i-1 lên i.
    // Sau khi cập nhật lên i, chỉ cần truy vấn giá trị nhỏ nhất toàn cục là biết có giải pháp hay không

    while (tp1 && a[i] <= a[st1[tp1]])  // Stack đơn điệu tăng, duy trì Min
      T.update(1, 1, n, st1[tp1 - 1] + 1, st1[tp1], a[st1[tp1]]), tp1--;
    while (tp2 && a[i] >= a[st2[tp2]])
      T.update(1, 1, n, st2[tp2 - 1] + 1, st2[tp2], -a[st2[tp2]]), tp2--;

    T.update(1, 1, n, st1[tp1] + 1, i, -a[i]);
    st1[++tp1] = i;
    T.update(1, 1, n, st2[tp2] + 1, i, a[i]);
    st2[++tp2] = i;

    id[i] = ++cnt;
    L[cnt] = R[cnt] = i;  // L, R ở đây là mút trái, phải của đoạn chỉ số nút tương ứng
    int le = T.query(1, 1, n), now = cnt;
    while (tp && L[st[tp]] >= le) {
      if (typ[st[tp]] && judge(M[st[tp]], i)) {
        // Phán đoán có thể làm con hay không, nếu có thì làm
        R[st[tp]] = i, M[st[tp]] = L[now], add(st[tp], now), now = st[tp--];
      } else if (judge(L[st[tp]], i)) {
        typ[++cnt] = 1;  // Nút hợp chắc chắn được xây dựng theo cách này
        L[cnt] = L[st[tp]], R[cnt] = i, M[cnt] = L[now];
        // Ở đây mảng M ghi lại mút trái của nút con ngoài cùng bên phải, dùng để phán đoán có thể làm con ở trên hay không
        add(cnt, st[tp--]), add(cnt, now);
        now = cnt;
      } else {
        add(++cnt, now);  // Tạo một nút mới, thêm now làm con
        // Nếu không thể tạo thành đoạn liên tiếp bắt đầu từ nút hiện tại, thì hợp nhất.
        // Cho đến khi tìm được một nút có thể tạo thành đoạn liên tiếp. Và ta chắc chắn tìm được một nút như vậy.
        do add(cnt, st[tp--]);
        while (tp && !judge(L[st[tp]], i));
        L[cnt] = L[st[tp]], R[cnt] = i, add(cnt, st[tp--]);
        now = cnt;
      }
    }
    st[++tp] = now;  // Kết thúc tăng dần, đẩy nút hiện tại vào stack

    T.update(1, 1, n, 1, i, -1);  // Vì mút phải của đoạn dịch sang phải một ô, nên tổng thể -1
  }

  rt = st[1];  // Nút còn lại trong stack là nút gốc
}

// Phân biệt nút lca thành phân tích hay hợp, ở đây coi nút lá là nút phân tích (typ=0)
void query(int l, int r) {
  int x = id[l], y = id[r];
  int z = lca(x, y);
  if (typ[z] & 1)
    l = L[go(x, dep[x] - dep[z] - 1)], r = R[go(y, dep[y] - dep[z] - 1)];
  // Nút hợp ở đây đặc biệt vì nút hợp này không nhất thiết là đoạn liên tiếp nhỏ nhất chứa l, r.
  // Vì đoạn con của nút hợp cũng là đoạn liên tiếp, và ta chỉ cần một trong số chúng là đủ.
  else
    l = L[z], r = R[z];
  printf("%d %d\n", l, r);
}

int main() {
  scanf("%d", &n);
  for (int i = 1; i <= n; ++i) scanf("%d", &a[i]);
  D.build();
  build();
  dfs(rt);
  scanf("%d", &m);
  for (int i = 1; i <= m; ++i) {
    int x, y;
    scanf("%d%d", &x, &y);
    query(x, y);
  }
  return 0;
}

// 20190612
// Cây Phân Tích Hợp
```

## Tài liệu tham khảo và Liên kết

[Blog của Đại Mễ Bính -【Ghi chép học tập】Cây Phân Tích Hợp](https://www.cnblogs.com/Paul-Guderian/p/11020708.html)

[^ref1]: Lưu Thừa Áo. Cấu trúc dữ liệu đoạn liên tiếp đơn giản. Giao lưu trại viên WC2019.