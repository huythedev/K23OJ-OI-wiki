author: sshwy, ComeIntoCalm

Hàm sinh mũ (exponential generating function, EGF) của dãy $a$ được định nghĩa là chuỗi lũy thừa hình thức:

$$
\hat{F}(x)=\sum_{n}a_n \frac{x^n}{n!}
$$

## Phép toán cơ bản

Phép cộng/trừ EGF giống với hàm sinh thường, tức là cộng hệ số theo từng hạng.

Xét phép nhân EGF. Với hai dãy $a,b$, giả sử EGF của chúng là $\hat{F}(x),\hat{G}(x)$, ta có

$$
\begin{aligned}
\hat{F}(x)\hat{G}(x)
&=\sum_{i\ge 0}a_i\frac{x^i}{i!}\sum_{j\ge 0}b_j\frac{x^j}{j!}\\
&=\sum_{n\ge 0}x^{n}\sum_{i=0}^na_ib_{n-i}\frac{1}{i!(n-i)!}\\
&=\sum_{n\ge 0}\frac{x^{n}}{n!}\sum_{i=0}^n\binom{n}{i}a_ib_{n-i}
\end{aligned}
$$

Vì vậy $\hat{F}(x)\hat{G}(x)$ là EGF của dãy

$$
\left\langle \sum_{i=0}^n \binom{n}{i}a_ib_{n-i} \right\rangle
$$

## Dạng đóng

Ta cũng xét dạng đóng của EGF.

Dãy $\langle 1,1,1,\cdots\rangle$ có EGF là:

$$
\hat{F}(x) = \sum_{n \ge 0}\frac{x^n}{n!} = \mathrm{e}^x
$$

vì khai triển Taylor của $\mathrm{e}^x$ tại $x=0$ chính là chuỗi vô hạn đó.

Tương tự, dãy hình học $\langle 1,p,p^2,\cdots\rangle$ có EGF là:

$$
\hat{F}(x) = \sum_{n\ge 0}\frac{p^nx^n}{n!}=\mathrm{e}^{px}
$$

## EGF và hàm sinh thường

Làm thế nào để hiểu EGF? Ta định nghĩa EGF của dãy $a$ là:

$$
F(x)=\sum_{n\ge 0}a_n\frac{x^n}{n!}
$$

Nhưng $F(x)$ thực ra cũng là hàm sinh thường của dãy $\left\langle \dfrac{a_n}{n!} \right\rangle$.

Hai cách hiểu này đều đúng. Nói cách khác, các loại hàm sinh chỉ là cách chuyển đổi góc nhìn của bài toán.

## Ý nghĩa tổ hợp của exp đa thức trong EGF

Nếu bạn chưa học đa thức exp thì hãy bỏ qua mục này. Đây là ý nghĩa rút ra từ exp, giúp hiểu sâu hơn về EGF.

Trong EGF, khi nói $f^n(x)$ thì $f$ mặc định là EGF. Trước tiên xét tích của hai EGF:

$$
\hat{H}(x) = \hat{F}(x)\hat{G}(x) = \sum_{n\geq 0} \left[ \sum_{i = 0}^n\binom {n}{i}f_ig_{n-i} \right] \frac{x^n}{n!}
$$

Với hai EGF nhân nhau, $[x^k]\hat{H}(x)$ thực chất là một phép chập. Nếu xét tích của nhiều EGF, thì $[x^k]\hat{H}(x)$ là tổng hệ số của mọi cách chọn một hạng $x^{a_i}$ từ mỗi EGF sao cho $\sum_i a_i=k$.
Từ góc nhìn tập hợp, đó là số cách chia $n$ phần tử có nhãn thành $k>0$ tập có nhãn.

> Nếu $k=0$ thì hệ số hiển nhiên là tích các hằng số của mỗi EGF, nhưng trong đa thức $\exp$ có một số yêu cầu khiến hạng tự do của $f(x)$ phải là $0$; nguyên nhân sẽ được giải thích ở phần sau.

Trong ý nghĩa tổ hợp của hệ số đa thức (xem mục tổ hợp), các tập thường có thứ tự; nhưng trong $\exp(f(x))$, các tích $f^k(x)$ với $k$ bản sao $f(x)$ cho cùng EGF, trong khi phép chia tập là không có thứ tự, nên hệ số cần nhân thêm $\dfrac{1}{k!}$.

Gọi $F_k(n)$ là số cách chia $n$ phần tử có nhãn thành $k$ tập con không rỗng, không có nhãn (vì là $\exp$ nên có điều kiện không rỗng). Gọi $f_i$ là số cấu trúc đặc biệt trên tập $i$ phần tử (chỉ phụ thuộc kích thước tập), tức hệ số của EGF gốc. Khi đó:

$$
F_k(n)=\frac{n!}{k!}\sum_{\sum_{i}^ka_i=n}\prod_{j=1}^{k}\frac{f_{a_j}}{a_j!}
$$

Gọi EGF của $f_n$ là $\hat{F}(x)$:

$$
\hat{F}(x) = \sum_{n \geq 0} f_n\frac{x^n}{n!}
$$

Gọi EGF của $F_k(n)$ là $G_k(x)$, thì:

$$
\begin{aligned}
G_k(x)&=\sum_{n \geq 0} F_k(n)\frac{x^n}{n!}\\
&=\sum_{n \geq 0} x^n\frac{1}{k!}\sum_{\sum_i^k a_i=n}\prod_{j=1}^{k}\frac{f_{a_j}}{a_j!}\\
&=\frac{1}{k!}\sum_{n \geq 0}\sum_{\sum_i^k a_i=n}\prod_{j=1}^{k}\frac{f_{a_j}x^{a_j}}{a_j!}\\
&=\frac{1}{k!}\hat{F}^k(x)
\end{aligned}
$$

Với mọi $k \geq 0$:

$$
\sum_{k \geq 0}G_k(x) = \sum_{k \geq 0}\frac{\hat{F}^k(x)}{k!} = \exp{\hat{F}(x)}
$$

Trên đây là hiểu trực tiếp bằng công thức tổ hợp; ta cũng có thể chứng minh quan hệ giữa $\exp(f(x))$ và $f(x)$ bằng truy hồi.

Gọi $F_k(n)$ là số cách chia $n$ phần tử có nhãn thành $k$ tập không rỗng (không nhãn), $g_i$ là số cách tổ chức bên trong một tập $i$ phần tử (ý nghĩa như $f_i$ ở trên). Đặt $G(x)$ là EGF của $\{g_i\}$, $H_k(x)$ là EGF của $\{F_k(n)\}$.

Chọn một tập con gồm $i$ phần tử làm một tập riêng có $g_i$ cách, phần còn lại $n-i$ phần tử tạo thành $k-1$ tập có $F_{k-1}(n-i)$ cách; nhưng trong phân hoạch cuối cùng, mỗi tập sẽ được chọn làm “tập riêng” một lần, nên bị đếm $k$ lần, cần chia cho $k$.

$$
\begin{aligned}
H_k(x) &= \sum_{n\ge 0}\cfrac{x^n}{n!}F_k(n)\\
&=\sum_{n\ge 0}\cfrac{x^n}{n!}\sum_{i=1}^{n-k+1}\binom n {i} F_{k-1}(n-i)\times g_i\times \cfrac{1}{k}\\
&=\cfrac{1}{k}\sum_{n\ge 0}\cfrac{x^n}{n!}\sum_{i=0}^{n}\binom n {i}F_{k-1}(n-i)\times g_i\\
&=\cfrac{1}{k}\cdot  H_{k-1}(x)G(x)
\end{aligned}
$$

Cận trên xuất phát từ điều kiện tập không rỗng $n-(k-1)\geq i$ (mỗi trong $k-1$ tập có ít nhất một phần tử), nhưng nếu vượt cận thì đặt $F_{k-1}(n-i)=0$ nên không ảnh hưởng.

Từ truy hồi, khai triển với biên $k=1$ và $H_1(x)=G(x)$:

$$
\begin{aligned}
H_k(x) &= \cfrac{1}{k}\cdot   H_{k-1}(x)G(x)\\
&= \cfrac{1}{k}\cdot\cfrac{1}{k-1}\cdot   H_{k-2}(x)G^2(x)\\
&=\cdots \\
&= \cfrac{1}{k}\cdot\cfrac{1}{k-1} \cdots\cfrac{1}{2}\cdot H_{1}(x)G^{k-1}(x)\\
&= \cfrac{1}{k!}G^{k}(x)\\
\end{aligned}
$$

Từ đó:

$$
\begin{aligned}
\sum_{k\ge 0}H_k(x)=\sum_{k\ge 0}\cfrac{G^k(x)}{k!}=\exp G(x)
\end{aligned}
$$

Rõ ràng, **định nghĩa theo phân hoạch thành tập không rỗng** ($g_0=0$) là phù hợp; nếu **cho phép tập rỗng** ($g_0=1$), thì trong $[x^n]G^k$ sẽ có đóng góp từ $[x^n]G^y,y>k$ (chọn hạng tự do ở ít nhất một $G$), gây đếm thừa, không phải đại lượng cần tìm.

Ở góc nhìn truy hồi, tích nhiều EGF cũng có thể xem là một dạng tổ hợp giống “ba lô” (ghép hai nhóm đối tượng đếm).

Tóm lại, ý nghĩa của đa thức $\exp$ là: **số cách tạo tập hợp có nhãn từ các lớp cấu trúc cho trước**, hay số cách phân hoạch thành bất kỳ số tập con không rỗng.

## Hoán vị và hoán vị vòng

Số hoán vị độ dài $n$ có EGF:

$$
\hat{P}(x)=\sum_{n\ge 0}\frac{n!x^n}{n!}=\sum_{n\ge 0}x^n=\frac{1}{1-x}
$$

Hoán vị vòng là sắp $1,2,\cdots,n$ thành một vòng; các cấu hình quay là tương đương (nhưng lật không tương đương).

Số hoán vị vòng của $n$ phần tử là $(n-1)!$. Do đó EGF là

$$
\hat{Q}(x)=\sum_{n\ge 1}\frac{(n-1)!x^n}{n!}=\sum_{n\ge 1}\frac{x^n}{n}=-\ln(1-x)=\ln\left( \frac{1}{1-x} \right)
$$

Suy ra $\exp \hat{Q}(x)=\hat{P}(x)$. Đây là suy luận toán học. Trực giác: EGF của hoán vị vòng khi lấy $\exp$ cho EGF của hoán vị?

Một hoán vị là hợp của nhiều chu trình. Ví dụ $p=[4,3,2,5,1]$ có hai chu trình:

![](./images/p1.png)

(tức từ $p_i$ nối cung hướng đến $i$)

Nếu đổi chu trình thứ hai thành

![](./images/p2.png)

thì hoán vị tương ứng là $[5,3,2,1,4]$.

Nói cách khác, số hoán vị độ dài $n$ bằng:

1.  Chia $1,2,\cdots,n$ thành nhiều tập
2.  Mỗi tập tạo thành một chu trình hoán vị

Số cách tạo chu trình trên một tập có kích thước cố định chính là số hoán vị vòng. Do đó số hoán vị độ dài $n$ là: chia $1,2,\cdots,n$ thành nhiều tập, rồi nhân số hoán vị vòng của từng tập.

Đó là trực giác của $\exp$ đa thức.

Mở rộng:

-   Nếu EGF của **cây khung** có nhãn với $n$ đỉnh là $\hat{F}(x)$, thì EGF của **rừng** có nhãn là $\exp \hat{F}(x)$ — trực giác: chia $n$ đỉnh thành nhiều tập, mỗi tập tạo thành một cây khung.
-   Nếu EGF của đồ thị vô hướng liên thông có nhãn là $\hat{F}(x)$, thì EGF của đồ thị vô hướng có nhãn là $\exp \hat{F}(x)$, và có thể tính:

    $$
    \exp \hat{F}(x)=\sum_{n\ge 0}2^{\binom{n}{2}}\frac{x^n}{n!}
    $$

    Vì vậy muốn tính EGF của đồ thị liên thông, chỉ cần một lần $\ln$ đa thức.

Tiếp theo là một số ứng dụng của EGF.

## Ứng dụng

### Số hoán vị không điểm bất động

???+ note "Số hoán vị không điểm bất động"
    Định nghĩa một hoán vị độ dài $n$ là hoán vị không điểm bất động nếu $p_i\ne i$.
    
    Hãy tìm EGF của số hoán vị không điểm bất động.

Xét theo chu trình, hoán vị không điểm bất động là hoán vị không có chu trình độ dài $1$. EGF của chu trình độ dài $\ge 2$ là:

$$
\sum_{n\ge 2}\frac{x^n}{n}=-\ln\left(1-x\right)-x
$$

Do đó EGF của số hoán vị không điểm bất động là $\exp(-\ln(1-x)-x)$.

### Điểm bất động

???+ note "[Điểm bất động](https://www.51nod.com/Html/Challenge/Problem.html#problemId=1728)"
    Đề bài: đếm số ánh xạ $f:\{1,2,\cdots,n\}\to \{1,2,\cdots,n\}$ sao cho
    
    $$
    \underbrace{f\circ f\circ\cdots\circ f}_{k}=\underbrace{f\circ f\circ\cdots\circ f}_{k-1}
    $$
    
    $nk\le 2\times 10^6,1\le k\le 3$.

Xét đồ thị có cạnh từ $i$ đến $f(i)$. Khi đó từ bất kỳ $i$ đi $k$ bước hay $k-1$ bước đều đến cùng một điểm. Tức là cây gốc-vòng có vòng là tự vòng và độ sâu không vượt quá $k$ (độ sâu gốc là $1$). Xem cây gốc-vòng như cây có gốc là tương đương. Vậy bài toán trở thành: đếm rừng cây có gốc, có nhãn, độ sâu không vượt quá $k$ với $n$ đỉnh.

Gọi EGF của cây có gốc độ sâu không vượt quá $k$ là:

$$
\hat{F_k}(x)=\sum_{n\ge 0}f_{n,k}\frac{x^n}{n!}
$$

Xét truy hồi $\hat{F_k}(x)$. Cây có gốc độ sâu không vượt quá $k$ là một đỉnh gốc nối với một số cây có gốc độ sâu không vượt quá $k-1$. Do đó

$$
\hat{F_k}(x)=x\exp \hat{F}_{k-1}(x)
$$

EGF của đáp án là $\exp \hat{F}_k(x)$. Lấy hệ số bậc $n$ là đáp án.

### Lust

???+ note "[Lust](https://codeforces.com/contest/891/problem/E)"
    Cho dãy $a_1,a_2,\cdots,a_n$ và biến $s$ ban đầu bằng $0$. Lặp $k$ lần:
    
    -   Chọn ngẫu nhiên đều một $x$ trong $1,2,\cdots,n$.
    -   Cộng vào $s$ giá trị $\prod_{i\ne x}a_i$.
    -   Giảm $a_x$ đi $1$.
    
    Hãy tính kỳ vọng của $s$ sau $k$ lần.
    
    $1\le n\le 5000,1\le k\le 10^9,0\le a_i\le 10^9$.

Giả sử sau $k$ lần, $a_i$ giảm $b_i$, khi đó

$$
s=\prod_{i=1}^n a_i-\prod_{i=1}^n(a_i- b_i)
$$

Bài toán quy về tính kỳ vọng của $\prod_{i=1}^n (a_i- b_i)$ sau $k$ lần.

Xét tổng $\prod_{i=1}^n (a_i- b_i)$ trên mọi phương án, rồi chia cho $n^k$.

Số cách để trong $k$ thao tác, phần tử $i$ xuất hiện $b_i$ lần là

$$
\frac{k!}{b_1!b_2!\cdots b_n!}
$$

Tương tự hệ số trong phép nhân EGF.

Đặt EGF của $a_j$ là

$$
F_j(x)=\sum_{i\ge 0}(a_j-i)\frac{x^i}{i!}
$$

Khi đó đáp án là

$$
[x^k]\prod_{j=1}^nF_j(x)
$$

Để tính nhanh, đổi $F_j(x)$ sang dạng đóng:

$$
\begin{aligned}
F_j(x)&=\sum_{i\ge 0}a_j\frac{x^i}{i!}-\sum_{i\ge 1}\frac{x^i}{(i-1)!}\\
&=a_j\mathrm{e}^x-x\mathrm{e}^x\\
&=(a_j-x)\mathrm{e}^x
\end{aligned}
$$

Suy ra

$$
\prod_{j=1}^nF_j(x)=\mathrm{e}^{nx}\prod_{j=1}^n(a_j-x)
$$

Trong đó $\prod_{j=1}^n(a_j-x)$ là đa thức bậc $n$, tính trực tiếp được. Gọi khai triển $\sum_{i=0}^nc_ix^i$, khi đó

$$
\begin{aligned}
\prod_{j=1}^nF_j(x)
&=\left(\sum_{i\ge 0} \frac{n^ix^i}{i!}\right)\left(\sum_{i=0}^nc_ix^i\right)\\
&=\sum_{i\ge 0}\sum_{j=0}^i c_jx^j\frac{n^{i-j}x^{i-j}}{(i-j)!}\\
&=\sum_{i\ge 0}\frac{x^{i}}{i!}\sum_{j=0}^i n^{i-j}i^{\underline{j}}c_j
\end{aligned}
$$

Tính hệ số $x^k$ là xong.
