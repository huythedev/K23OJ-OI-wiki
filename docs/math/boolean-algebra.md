Trong logic toán học, đại số Boolean (boolean algebra) là một nhánh của đại số．Trong đại số sơ cấp, biến nhận giá trị là số, các phép toán chính là cộng, nhân, lũy thừa và các phép ngược của chúng．Trong đại số Boolean, biến chỉ nhận **đúng** và **sai** (thường ký hiệu $1$ và $0$), các phép toán chính là hội (và, $\land$), tuyển (hoặc, $\lor$), phủ định (không, $\lnot$)．Giống như đại số sơ cấp mô tả phép tính số, đại số Boolean mô tả phép tính logic．

## Hàm Boolean

???+ abstract "Định nghĩa"
    **Hàm Boolean** (boolean function) là hàm dạng $f:\mathbf{B}^k\to \mathbf{B}$, trong đó $\mathbf{B}=\{0,1\}$ là **trường Boolean** (boolean domain), số nguyên không âm $k$ là **bậc** (arity) của hàm．$k=1$ là hàm một ngôi, v.v. $k=0$ thì hàm là hằng trong $\mathbf{B}$．

Ta thường chỉ nghiên cứu hàm một ngôi và hai ngôi．Nếu không nói rõ, các hàm Boolean dưới đây chỉ xét một ngôi và hai ngôi．

Ngoài biểu thức hàm, ta có thể dùng **bảng chân trị** (truth table), **cổng logic** (logic gate), [biểu đồ Venn](https://en.wikipedia.org/wiki/Venn_diagram) để biểu diễn．

???+ abstract "Bảng chân trị"
    Với một hàm Boolean, liệt kê mọi đầu vào và đầu ra tương ứng thành bảng gọi là bảng chân trị．

Hàm Boolean $n$ biến cũng có thể biểu diễn bằng **công thức mệnh đề** (propositional formula) với $n$ biến; hai công thức $p$ và $q$ **tương đương logic** (logically equivalent) khi và chỉ khi chúng mô tả cùng một hàm Boolean, ký hiệu $p\iff q$．

Dưới đây là một số hàm Boolean thường gặp, cũng gọi là **toán tử logic** (logical connective) hoặc **toán tử luận lý** (logical operator)：

| Tên (logic toán học)                                      | Tên khác                | Ký hiệu                          |
| -------------------------------------------------- | -------------------- | -------------------------------- |
| Hằng đúng (truth、tautology)                             |                      | $\top$                           |
| Hằng sai (falsity、contradiction)                       |                      | $\bot$                           |
| Mệnh đề                                                | Chính nó               | $A$                              |
| Phủ định (negation)                                     | NOT                    | $\lnot A$                        |
| Hội (conjunction)                                       | AND                    | $A \land B$                      |
| Tuyển (disjunction)                                     | OR                     | $A \lor B$                       |
| Phi hội (non-conjunction)                                | NAND, Sheffer stroke   | $A \bar{\land} B$、$A\uparrow B$  |
| Phi tuyển (non-disjunction)                              | NOR                    | $A \bar{\lor} B$、$A\downarrow B$ |
|                                                      | XOR (Exclusive-OR)      | $A \oplus B$                     |
|                                                      | XNOR (Exclusive-NOR)    | $A \odot B$                      |
| Hàm kéo theo thực (material implication）[^note1]         |                      | $A \to B$                        |
| Phi kéo theo thực (material nonimplication）[^note1]      |                      | $A \nrightarrow B$               |
| Hệ quả đảo (converse implication）[^note1]               |                      | $A \gets B$                      |
| Phi hệ quả đảo (converse nonimplication）[^note1]         |                      | $A \nleftarrow B$                |
| Tương đương (biconditional/equivalence）[^note1][^note2]  |                      | $A \leftrightarrow B$            |
| Phi tương đương (non-equivalence)[^note1][^note3]        |                      | $A \nleftrightarrow B$           |

Bảng chân trị (From [Wikipedia](https://commons.wikimedia.org/wiki/File:Logical_connectives_table.svg)）：

![](./images/logical-connectives-table.svg)

Biểu đồ Venn và [biểu đồ Hasse](./order-theory.md#偏序集的可视化表示hasse-图) (với thứ tự $\subseteq$, From [Wikipedia](https://en.wikipedia.org/wiki/File:Logical_connectives_Hasse_diagram.svg)）：

![](./images/logical-connectives-hasse-diagram.svg)

Vì hàm Boolean $n$ biến có $2^n$ đầu vào, nên có $2\uparrow (2\uparrow n)$ hàm, với $\uparrow$ là mũ Knuth．

Tổ hợp các toán tử logic gọi là **biểu thức logic** (logical expression)．

Nếu xem $\mathbf{B}$ là lớp dư modulo 2, thì XOR tương đương cộng modulo 2, AND tương đương nhân modulo 2, nên đôi khi dùng $\mathbf{Z}_2$ để chỉ trường Boolean．

### Độ ưu tiên

Toán tử một ngôi có ưu tiên cao hơn toán tử hai ngôi, tức $\lnot$ ưu tiên hơn $\land$、$\lor$、$\oplus$．

Giữa các toán tử hai ngôi có nhiều quy ước; có tài liệu cho rằng $\land$、$\lor$、$\oplus$ cao hơn $\to$、$\gets$、$\leftrightarrow$，có tài liệu ngược lại．Vì vậy nên thêm ngoặc để rõ thứ tự．

Quy ước trong C++ xem [Bảng ưu tiên toán tử C++](../lang/op.md#c-运算符优先级总表)．

### Toán tử tự đủ và tập toán tử đầy đủ

Thực ra chỉ cần NAND hoặc NOR là đủ để biểu diễn mọi toán tử khác, CPU cũng dựa trên điều này．Tuy nhiên, do **AND, OR, NOT, XOR** có tính chất tốt, nên khi nghiên cứu thường dùng bốn phép này．

??? example "Biểu diễn các toán tử khác bằng NAND/NOR"
    Ta có
    
    -   $\lnot p=p\bar{\land} p=p\bar{\lor} p$，
    -   $p\land q=(p\bar{\land}q)\bar{\land}(p\bar{\land}q)=(p\bar{\lor}p)\bar{\lor}(q\bar{\lor}q)$，
    -   $p\lor q=(p\bar{\land}p)\bar{\land}(q\bar{\land}q)=(p\bar{\lor}q)\bar{\lor}(p\bar{\lor}q)$，
    -   $p\to q=p\bar{\land} (q\bar{\land} q)=((p\bar{\lor}p)\bar{\lor}q)\bar{\lor}((p\bar{\lor}p)\bar{\lor}q)$．
    
    Ngoài ra
    
    -   $p=\lnot\lnot p$，
    -   $p\nleftrightarrow q=p\oplus q=(p\lor q)\land\lnot (p\land q)$，
    -   $p\leftrightarrow q=p\odot q=\lnot(p\oplus q)$，
    -   $p\nrightarrow q=\lnot(p\to q)$，
    -   $p\gets q=q\to p$，
    -   $p\nleftarrow q=\lnot(p\gets q)$．

Câu hỏi: có thể chỉ dùng một số toán tử để biểu diễn mọi toán tử không? Đây là khái niệm tập toán tử đầy đủ．

???+ abstract "Định nghĩa"
    Với một tập toán tử logic, nếu chỉ dùng các toán tử trong tập có thể biểu diễn mọi toán tử, thì gọi là **tập toán tử đầy đủ** (functionally complete operator set)．Đặc biệt, nếu chỉ dùng một toán tử là đủ, gọi là **toán tử tự đủ** (sole sufficient operator) hoặc **hàm Sheffer** (Sheffer function)．
    
    Nếu bỏ bất kỳ phần tử nào khỏi tập đầy đủ thì không còn đầy đủ, gọi là **tập đầy đủ tối thiểu** (minimal functionally complete operator set)．

Có thể chứng minh chỉ có $\bar{\land}$ và $\bar{\lor}$ là toán tử tự đủ．

Các tập đầy đủ tối thiểu thường gặp[^vaughan1942complete]：

-   $\{\bar{\land}\}$，$\{\bar{\lor}\}$，
-   $\{\land,\lnot\}$，$\{\lor,\lnot\}$，$\{\gets,\lnot\}$，$\{\to,\lnot\}$，$\{\nleftarrow,\lnot\}$，$\{\nrightarrow,\lnot\}$，
-   $\{\gets,\bot\}$，$\{\to,\bot\}$，$\{\nleftarrow,\top\}$，$\{\nrightarrow,\top\}$，
-   $\{\gets,\nleftarrow\}$，$\{\to,\nleftarrow\}$，$\{\gets,\nrightarrow\}$，$\{\to,\nrightarrow\}$，
-   $\{\gets,\nleftrightarrow\}$，$\{\to,\nleftrightarrow\}$，$\{\nleftarrow,\leftrightarrow\}$，$\{\nrightarrow,\leftrightarrow\}$，
-   $\{\lor,\leftrightarrow,\bot\}$，$\{\lor,\leftrightarrow,\nleftrightarrow\}$，$\{\lor,\nleftrightarrow,\top\}$，
-   $\{\land,\leftrightarrow,\bot\}$，$\{\land,\leftrightarrow,\nleftrightarrow\}$，$\{\land,\nleftrightarrow,\top\}$．

### Tính chất

Trước hết là tính chất cấu trúc đại số：

-   AND và OR tạo [bán nhóm đơn vị giao hoán](./algebra/basic.md#群) trên $\mathbf{B}$, tức thỏa giao hoán, kết hợp và có đơn vị ($x\land 1=x\lor 0=x$)．
-   XOR và XNOR tạo [nhóm](./algebra/basic.md#群) trên $\mathbf{B}$, tức thỏa giao hoán, kết hợp, có đơn vị ($x\oplus 0=x\odot 1=x$) và có nghịch đảo ($x\oplus x=0$，$x\odot x=1$)．
-   NAND và NOR không thỏa kết hợp, nên không là bán nhóm．

Với $\land$、$\lor$：

-   Luật phân phối:
    -   $a\land(b\diamond c)=(a\land b)\diamond (a\land c)$ với $\diamond$ là $\land$、$\lor$、$\oplus$，
    -   $a\lor(b\diamond c)=(a\lor b)\diamond (a\lor c)$ với $\diamond$ là $\land$、$\lor$、$\odot$．
-   **Idempotent**: $x\land x=x$、$x\lor x=x$．
-   Đơn điệu: $a\to b\iff(a\land c)\to(b\land c)$、$a\to b\iff(a\lor c)\to(b\lor c)$．
-   **Hấp thụ**: $x\land(x\lor y)=x\lor(x\land y)=x$．
-   Quan hệ với “$\to$”:
    -   $a \lor b \iff (\lnot a \to b) \land (\lnot b \to a)$，
    -   $a \land b \iff \lnot((a \to \lnot b) \lor (b \to \lnot a))$．

???+ abstract "Tính đơn điệu của hàm Boolean"
    Với hàm $f(x_1,\dots,x_n)$ và hai phần tử $(a_1,\dots,a_n),(b_1,\dots,b_n)\in \mathbf{B}^n$, nếu khi $a_i\leq b_i$ với mọi $i$ thì luôn có $f(a_1,\dots,a_n)\leq f(b_1,\dots,b_n)$, thì gọi hàm đó là đơn điệu．

Các tính chất khác：

-   **Luật loại trừ trung gian**: $p\lor\lnot p$ luôn đúng．
-   $\lnot p\iff p\to\bot$．
-   **Đối hợp** của $\lnot$: $\lnot\lnot x=x$．
-   Đối hợp của $\oplus$、$\odot$: $x\oplus y\oplus y=x$、$x\odot y\odot y=x$．
-   Luật De Morgan: $\lnot(p\land q)=\lnot p\lor \lnot q$、$\lnot(p\lor q)=\lnot p\land \lnot q$．

## Chuẩn hóa biểu thức logic

Dựa vào các tính chất trên, ta có thể biến đổi tương đương để đưa biểu thức về dạng chuẩn, dùng trong chứng minh tự động．Các dạng chuẩn thường gặp: **CNF** (conjunctive normal form), **DNF** (disjunctive normal form), **ANF** (algebraic normal form)．

???+ abstract "CNF và DNF"
    Định nghĩa đệ quy：
    
    1.  **Literal**: với biến $x$, $x$ và $\lnot x$ là literal．
    2.  Tiểu thức:
        -   Literal là tiểu thức，
        -   Nếu $A$ là literal, $B$ là tiểu thức, thì $A\lor B$ là tiểu thức．
    3.  CNF:
        -   Nếu $A$ là tiểu thức thì $(A)$ là CNF，
        -   Nếu $A$ là tiểu thức, $B$ là CNF thì $(A)\land B$ là CNF．
    
    Tương tự, hoán đổi $\land$ và $\lor$ sẽ ra định nghĩa DNF．

Ví dụ DNF:

-   $(A\land\lnot B)\lor(C\land D\land\lnot E)$，
-   $(A\land B)\lor (C)$，
-   $(A\land B)$，
-   $(A)$．

Ví dụ CNF:

-   $(\lnot A\lor\lnot B\lor C)\land(\lor D\lor\lnot E)$，
-   $(A\lor B)\land (C)$，
-   $(A\lor B)$，
-   $(A)$．

Không phải CNF/DNF:

-   $\lnot(A\land B)$，
-   $A\land (B\lor (C\land D))$．

Đưa biểu thức chỉ gồm $\lnot$、$\land$、$\lor$ về DNF theo:

$$
\begin{array}{rcccl}
    \lnot\lnot x &&\mapsto&& x,\\
    \lnot(x\lor y) &&\mapsto&& \lnot x\land \lnot y,\\
    \lnot(x\land y) &&\mapsto&& \lnot x\lor \lnot y,\\
    x\land(y\lor z) &&\mapsto&& (x\land y)\lor (x\land z),\\
    (x\lor y)\land z &&\mapsto&& (x\land z)\lor (y\land z).
\end{array}
$$

Muốn CNF của $X$, lấy DNF của $\lnot X$ rồi phủ định và dùng De Morgan．

???+ abstract "ANF"
    Định nghĩa tiểu thức:
    
    -   Biến $x$ là tiểu thức，
    -   Nếu $A$ là tiểu thức, $x$ là biến thì $x\land A$ là tiểu thức．
    
    Biểu thức thuộc ANF nếu là một trong:
    
    1.  $1$ hoặc $0$，
    2.  XOR của các tiểu thức khác nhau, ví dụ $a\oplus b\oplus(a\land b)\oplus(a\land b\land c)$，
    3.  XOR giữa $1$ và các tiểu thức khác nhau, ví dụ $1\oplus a\oplus b\oplus(a\land b)\oplus(a\land b\land c)$．

ANF tương ứng một-một với đa thức trên $\mathbf{Z}_2$, nên còn gọi **đa thức Zhegalkin**．

Đưa biểu thức chỉ gồm $\lnot$、$\land$、$\lor$、$\oplus$ về ANF:

1.  $\oplus$: khai triển trực tiếp, ví dụ $(1\oplus x)\oplus(1\oplus x\oplus y)=1\oplus x\oplus 1\oplus x\oplus y=y$，
2.  $\land$: dùng phân phối, ví dụ $x\land(1\oplus x\oplus y)=(x\land 1)\oplus (x\land x)\oplus (x\land y)=x\oplus (x\land y)$，
3.  $\lnot$: thay $\lnot x$ bằng $1\oplus x$, ví dụ $\lnot(1\oplus x\oplus y)=1\oplus 1\oplus x\oplus y=x\oplus y$，
4.  $\lor$: thay $x\lor y$ bằng $1\oplus((1\oplus x)\land(1\oplus y))$ hoặc $x\oplus y\oplus (x\land y)$, ví dụ $(1\oplus x)\lor(1\oplus x\oplus y)=1\oplus((1\oplus 1\oplus x)\land(1\oplus 1\oplus x\oplus y))=1\oplus x\oplus(x\land y)$．

## Tài liệu tham khảo và chú thích

1.  [Boolean algebra - Wikipedia](https://en.wikipedia.org/wiki/Boolean_algebra)
2.  [Boolean function - Wikipedia](https://en.wikipedia.org/wiki/Boolean_function)
3.  [Logical connective - Wikipedia](https://en.wikipedia.org/wiki/Logical_connective)
4.  [Disjunctive normal form - Wikipedia](https://en.wikipedia.org/wiki/Disjunctive_normal_form)
5.  [Zhegalkin polynomial - Wikipedia](https://en.wikipedia.org/wiki/Zhegalkin_polynomial)

[^note1]: Dùng trong suy luận mệnh đề nên dùng mũi tên đôi dài như $A\implies B$、$A\impliedby B$、$A\iff B$．

[^note2]: Tương đương với XNOR．

[^note3]: Tương đương với XOR．

[^vaughan1942complete]: Vaughan, H. E. (1942). Complete sets of logical functions.*Transactions of the American Mathematical Society 51*: 117–32.
