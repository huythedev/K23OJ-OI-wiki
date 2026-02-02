## Giới thiệu

???+ note "Ví dụ nhập môn"
    Giả sử trong lớp có 10 học sinh thích Toán, 15 học sinh thích Văn, 21 học sinh thích Lập trình, hỏi có bao nhiêu học sinh thích ít nhất một môn?

Có phải $10+15+21=46$ không? Không, vì có thể có học sinh thích nhiều môn.

Đặt các tập học sinh thích Văn, Toán, Lập trình lần lượt là $A,B,C$, số học sinh là $|A\cup B\cup C|$. Nếu cộng trực tiếp $|A|,|B|,|C|$ thì bị đếm trùng nên phải trừ $|A\cap B|,|B\cap C|,|C\cap A|$, nhưng lại bị trừ quá, cần cộng lại $|A\cap B\cap C|$:

$$
|A\cup B\cup C|=|A|+|B|+|C|-|A\cap B|-|B\cap C|-|C\cap A|+|A\cap B\cap C|
$$

![容斥原理 - venn 图示例](./images/incexcp.png)

Khái quát thành nguyên lý bao hàm – loại trừ.

## Định nghĩa

Cho $U$ có $n$ thuộc tính khác nhau. Thuộc tính thứ $i$ là $P_i$, các phần tử có thuộc tính $P_i$ tạo tập $S_i$, khi đó:

$$
\begin{aligned}
\left|\bigcup_{i=1}^{n}S_i\right|=&\sum_{i}|S_i|-\sum_{i<j}|S_i\cap S_j|+\sum_{i<j<k}|S_i\cap S_j\cap S_k|-\cdots\\
&+(-1)^{m-1}\sum_{a_i<a_{i+1} }\left|\bigcap_{i=1}^{m}S_{a_i}\right|+\cdots+(-1)^{n-1}|S_1\cap\cdots\cap S_n|
\end{aligned}
$$

Tức:

$$
\left|\bigcup_{i=1}^{n}S_i\right|=\sum_{m=1}^n(-1)^{m-1}\sum_{a_i<a_{i+1} }\left|\bigcap_{i=1}^mS_{a_i}\right|
$$

### Chứng minh

Dùng nhị thức Newton đếm số lần xuất hiện của mỗi phần tử. Với phần tử $x$, giả sử nó thuộc $T_1,T_2,\cdots,T_m$, số lần nó được đếm:

$$
\begin{aligned}
Cnt=&|\{T_i\}|-|\{T_i\cap T_j|i<j\}|+\cdots+(-1)^{k-1}\left|\left\{\bigcap_{i=1}^{k}T_{a_i}|a_i<a_{i+1}\right\}\right|\\
&+\cdots+(-1)^{m-1}|\{T_1\cap\cdots\cap T_m\}|\\
=&\dbinom{m}{1}-\dbinom{m}{2}+\cdots+(-1)^{m-1}\dbinom{m}{m}\\
=&\dbinom{m}{0}-\sum_{i=0}^m(-1)^i\dbinom{m}{i}\\
=&1-(1-1)^m=1
\end{aligned}
$$

Mỗi phần tử được đếm đúng 1 lần, nên tổng là hợp. QED.

### Bổ sung

Với giao của các tập trong vũ trụ $U$, ta có:

$$
\left|\bigcap_{i=1}^{n}S_i\right|=|U|-\left|\bigcup_{i=1}^n\overline{S_i}\right|
$$

Vế phải dùng bao hàm – loại trừ.

Phần trên quen thuộc, tiếp theo là ứng dụng qua 3 ví dụ.

## Đếm nghiệm nguyên không âm của phương trình

???+ note "Đếm nghiệm nguyên không âm"
    Cho phương trình $\sum_{i=1}^nx_i=m$ và $n$ điều kiện $x_i\leq b_i$, với $m,b_i \in \mathbb{N}$. Hỏi số nghiệm nguyên không âm.

### Không có ràng buộc

Nếu bỏ $x_i\leq b_i$, số nghiệm là $\dbinom{m+n-1}{n-1}$.

Chứng minh: phương pháp cắm vạch.

Tương đương phân $m$ quả bóng vào $n$ hộp, cho phép hộp rỗng. Thêm $n-1$ quả, chọn $n-1$ vị trí cắm vạch trong $m+n-1$ vị trí, được $\dbinom{m+n-1}{n-1}$.

### Mô hình bao hàm – loại trừ

Trừu tượng hóa:

1.  Vũ trụ $U$: nghiệm không âm của $\sum_{i=1}^nx_i=m$.
2.  Phần tử: biến $x_i$.
3.  Thuộc tính: điều kiện $x_i\leq b_i$.

Cần $|\bigcap_{i=1}^nS_i|=|U|-\left|\bigcup_{i=1}^n\overline{S_i}\right|$. $|U|$ tính bằng tổ hợp, còn phần sau khai triển bao hàm – loại trừ.

Xét giao của một số $\overline{S_{a_i}}$, nghĩa là $x_{a_i}\ge b_{a_i}+1$. Đây là phương trình có một số biến có **cận dưới**. Ta trừ cận dưới để đưa về không âm.

Với

$$
\left|\bigcap_{a_i<a_{i+1} }^{1\leq i\leq k}S_{a_i}\right|
$$

thì phương trình thành:

$$
\sum_{i=1}^nx_i=m-\sum_{i=1}^k(b_{a_i}+1)
$$

Vậy số nghiệm tính bằng tổ hợp. Mảng $a$ độ dài $k$ tương đương duyệt tập con.

## HAOI2008 Mua sắm bằng xu

???+ note "HAOI2008 硬币购物"
    Có 4 loại xu, loại $i$ có mệnh giá $C_i$. Có $n$ truy vấn, mỗi truy vấn cho số lượng $D_i$ của từng loại và giá $S$, hỏi số cách trả.
    
    $n\leq 10^3,S\leq 10^5$.

Nếu dùng balo thì $O(4nS)$, không chịu được. Điểm đặc biệt là chỉ có 4 loại. Mô hình: nghiệm không âm của $\sum_{i=1}^4C_ix_i=S$ với $x_i\leq D_i$.

Dùng bao hàm – loại trừ, thuộc tính $x_i\leq D_i$. Khi đó cần:

$$
\sum_{i=1}^4C_ix_i=S-\sum_{i=1}^kC_{a_i}(D_{a_i}+1)
$$

tức là bài toán balo vô hạn. Có thể tiền xử lý, tổng $O(4S+2^4n)$.

??? note "Cài đặt"
    ```cpp
    --8<-- "docs/math/code/inclusion-exclusion-principle/inclusion-exclusion-principle_1.cpp"
    ```

## Tô màu đồ thị con của đồ thị đầy đủ

Ba bài trước là áp dụng xuôi. Bài này cần phân tích ngược.

???+ note "Tô màu đồ thị con của đồ thị đầy đủ"
    A và B thích tô màu đồ thị (không nhất thiết liên thông), quy tắc: các đỉnh kề nhau phải cùng màu. Với đồ thị đầy đủ $n$ đỉnh $G=(V,E)$, định nghĩa $F(S)$ với $S\subseteq E$ là số cách tô màu đồ thị $G'=(V,S)$ bằng $m$ màu. Nếu $|S|$ lẻ thì điểm A tăng $F(S)$, nếu chẵn thì điểm B tăng $F(S)$. Hỏi chênh lệch điểm A và B.

### Dạng toán học

Điểm chênh lệch là tổng có hệ số $(-1)$ theo chẵn lẻ. Ta cần:

$$
Ans=\sum_{S\subseteq E}(-1)^{|S|-1}F(S)
$$

### Mô hình bao hàm – loại trừ

Điều kiện “đỉnh kề nhau cùng màu” là thuộc tính. Ta tạm bỏ luật này, tô màu tùy ý. Với đồ thị $G'=(V,S)$ là **phần tử**. Thuộc tính $x_i=x_j$ là đỉnh $i,j$ cùng màu (không yêu cầu có cạnh). Gọi tập $Q_{i,j}$ là các phương án tô màu thỏa $x_i=x_j$; kích thước là số cách tô thỏa.

Quay lại đề: $F(S)$ là số cách tô thỏa $x_i=x_j$ với mọi $(i,j)\in S$, nên

$$
F(S)=\left|\bigcap_{(i,j)\in S}Q_{i,j}\right|
$$

Do bao hàm – loại trừ thường dùng một chỉ số, ánh xạ các cạnh $(i,j)$ thành $k,1\le k\le T=\frac{n(n+1)}{2}$, và $Q_{i,j}\to Q_k$. Thuộc tính $x_i=x_j$ là $P_k$. Khi đó $S$ tương ứng $K=\{k_1,\ldots,k_m\}$, và $E$ tương ứng $M=\{1,\ldots,T\}$:

$$
F(S)\iff F(\{ {k_i}\})=\left|\bigcap_{k_i}Q_{k_i}\right|
$$

### Phân tích ngược

Khai triển:

$$
\begin{aligned}
Ans &= \sum_{K\subseteq M}(-1)^{|K|-1}\left|\bigcap_{k_i\in K}Q_{k_i}\right|\\
    &= \sum_{i}|Q_i|-\sum_{i<j}|Q_i\cap Q_j|+\sum_{i<j<k}|Q_i\cap Q_j\cap Q_k|-\cdots+(-1)^{T-1}\left|\bigcap_{i=1}^TQ_i\right|
\end{aligned}
$$

Đây là dạng bao hàm – loại trừ, suy ra:

$$
Ans=\left|\bigcup_{i=1}^TQ_i\right|
$$

Vế phải là số cách tô mà **tồn tại** hai đỉnh cùng màu. Tổng số cách là $|U|=m^n$. Lấy bù là số cách tất cả khác màu: $A_m^n=\frac{m!}{(m-n)!}$. Do đó:

$$
Ans=m^n-A_m^n
$$

Giải bài bằng cách trừu tượng hóa, đưa $F(S)$ về giao/hợp/bổ, rồi áp dụng bao hàm – loại trừ **ngược**.

## Bao hàm – loại trừ trong số học

Bao hàm – loại trừ giúp giải một số bài số học.

### Đếm cặp có gcd bằng k

???+ note "Đếm cặp có gcd bằng $k$"
    Với $1 \le x, y \le N$, $f(k)$ là số cặp có thứ tự $(x, y)$ có $\gcd(x,y)=k$. Tính $f(1)$ đến $f(N)$.

Có thể dùng Euler hoặc Möbius, nhưng bao hàm – loại trừ đơn giản.

Số cặp có **ước chung** là $k$ trừ số cặp có **ước chung** là bội của $k$ sẽ ra số cặp có gcd là $k$. Tức:

$$
f(k)= \lfloor (N/k) \rfloor ^2 - \sum_{i=2}^{i*k \le N} f(i*k)
$$

Khi $k>N/2$ thì $f(k)=\lfloor (N/k) \rfloor ^2$. Duyệt ngược từ $N$ xuống $1$ là được.

```cpp
for (long long k = N; k >= 1; k--) {
  f[k] = (N / k) * (N / k);
  for (long long i = k + k; i <= N; i += k) f[k] -= f[i];
}
```

Độ phức tạp $O(N \log N)$.

Bài luyện tập:

-   [Luogu P2398 GCD SUM](https://www.luogu.com.cn/problem/P2398)
-   [Luogu P2158\[SDOI2008\] 仪仗队](https://www.luogu.com.cn/problem/P2158)
-   [Luogu P1447\[NOI2010\] 能量采集](https://www.luogu.com.cn/problem/P1447)

### Suy ra công thức Euler

???+ note "Công thức Euler"
    Tính $\varphi(n)$, với $\varphi(n)=|\{1\leq x\leq n|\gcd(x,n)=1\}|$.

Trực tiếp $O(n\log n)$, sàng tuyến tính $O(n)$, Dujiao sieve $O(n^{2/3})$. Dùng bao hàm – loại trừ:

Phân tích:

$$
n=\prod_{i=1}^k{p_i}^{c_i}
$$

Cần $x$ không chia hết bởi mọi $p_i$. Mỗi thuộc tính là $p_i\nmid x$, tập $S_i$. Khi đó:

$$
\varphi(n)=\left|\bigcap_{i=1}^kS_i\right|=|U|-\left|\bigcup_{i=1}^k\overline{S_i}\right|
$$

$|U|=n$, $\overline{S_i}$ là tập $p_i\mid x$, có $|\overline{S_i}|=\frac{n}{p_i}$. Suy ra

$$
\left|\bigcap_{a_i<a_{i+1}}S_{a_i}\right|=\frac{n}{\prod p_{a_i}}
$$

Do đó:

$$
\begin{aligned}
\varphi(n)&=n-\sum_{i}\frac{n}{p_i}+\sum_{i<j}\frac{n}{p_ip_j}-\cdots+(-1)^k\frac{n}{p_1p_2 \cdots p_k}\\
&=n\left(1-\frac{1}{p_1}\right)\left(1-\frac{1}{p_2}\right)\cdots\left(1-\frac{1}{p_k}\right)\\
&=n\prod_{i=1}^k\left(1-\frac{1}{p_i}\right)
\end{aligned}
$$

Đây là biểu thức Euler.

## Tổng quát hóa bao hàm – loại trừ

Cho hai hàm $f(S),g(S)$, nếu

$$
f(S)=\sum_{T\subseteq S}g(T)
$$

thì

$$
g(S)=\sum_{T\subseteq S}(-1)^{|S|-|T|}f(T)
$$

### Chứng minh

Bắt đầu từ vế phải:

$$
\begin{aligned}
&\sum_{T\subseteq S}(-1)^{|S|-|T|}f(T)\\
=&\sum_{T\subseteq S}(-1)^{|S|-|T|}\sum_{Q\subseteq T}g(Q)\\
=&\sum_{Q}g(Q)\sum_{Q\subseteq T\subseteq S}(-1)^{|S|-|T|}\\
\end{aligned}
$$

Vế trong không phụ thuộc $Q$:

$$
=\sum_{Q}g(Q)\sum_{T\subseteq (S\setminus Q)}(-1)^{|S\setminus Q|-|T|}
$$

Đặt $F(P)=\sum_{T\subseteq P}(-1)^{|P|-|T|}$, ta có:

$$
\begin{aligned}
F(P)&=\sum_{T\subseteq P}(-1)^{|P|-|T|}\\
&=\sum_{i=0}^{|P|}\dbinom{|P|}{i}(-1)^{|P|-i}=\sum_{i=0}^{|P|}\dbinom{|P|}{i}1^i(-1)^{|P|-i}\\
&=(1-1)^{|P|}=0^{|P|}
\end{aligned}
$$

Do đó:

$$
\sum_{Q}g(Q)F(S\setminus Q)=\sum_{Q}g(Q)\cdot 0^{|S\setminus Q|}
$$

Chỉ khi $|S\setminus Q|=0$ thì $0^0=1$, tức $Q=S$, đóng góp $g(S)$. Còn lại bằng 0. Suy ra đúng.

### Hệ quả

Nếu trong $U$:

$$
f(S)=\sum_{S\subseteq T}g(T)
$$

thì

$$
g(S)=\sum_{S\subseteq T}(-1)^{|T|-|S|}f(T)
$$

Đây là dạng bổ, chứng minh tương tự.

## Đếm DAG

???+ note "Đếm DAG"
    Đếm số đồ thị có hướng không chu trình có $n$ đỉnh có nhãn, modulo $10^9+7$. $n\leq 5\times 10^3$.

### DP trực tiếp

Đặt $f[i,j]$ là số DAG với $i$ đỉnh, có $j$ đỉnh bậc vào $0$. Bỏ $j$ đỉnh đó, còn $k$ đỉnh bậc vào $0$. Trong $k$ đỉnh, phải có ít nhất một đỉnh nối từ tập $j$, nên có $(2^j-1)^k$ cách; các đỉnh còn lại có $2^{i-j-k}$ cách nối từ $j$ đỉnh. Khi đó:

$$
f[i,j]=\binom{i}{j}\sum_{k=1}^{i-j}(2^j-1)^k2^{(i-j-k)j}f[i-j,k]
$$

Độ phức tạp $O(n^3)$.

### Nới lỏng điều kiện

Đặt $f[i]$ là số DAG có $i$ đỉnh. Dùng bao hàm – loại trừ. Chọn $j$ đỉnh, cho phép các cạnh từ chúng tới phần còn lại, có $2^{(i-j)j}$ cách:

$$
f[i]=\sum_{j=1}^i(-1)^{j-1}\binom{i}{j}2^{(i-j)j}f[i-j]
$$

Độ phức tạp $O(n^2)$.

## Bao hàm – loại trừ min-max

Với dãy $\{x_i\}$ thỏa [thứ tự toàn phần](../order-theory.md#偏序集) và có phép cộng trừ, đặt $S=\{1,2,3,\cdots,n\}$:

$$
\max_{i\in S}{x_i}=\sum_{T\subseteq S}{(-1)^{|T|-1}\min_{j\in T}{x_j}}
$$

$$
\min_{i\in S}{x_i}=\sum_{T\subseteq S}{(-1)^{|T|-1}\max_{j\in T}{x_j}}
$$

**Chứng minh:** Ánh xạ về bao hàm – loại trừ. Với $x\in S$, giả sử $x$ là phần tử nhỏ thứ $k$. Đặt $f:x\mapsto \{1,2,\cdots,k\}$, là song ánh.

Với $x,y\in S$, $f(\min(x,y))=f(x)\cap f(y)$, $f(\max(x,y))=f(x)\cup f(y)$. Do đó:

$$
\begin{aligned}
\left|f\left(\max_{i\in S}{x_i}\right)\right|
&= \left| \bigcup_{i\in S} f(x_i) \right|\\
&= \sum_{T\subseteq S}(-1)^{|T|-1} \left|\bigcap_{j\in T}f(x_j)\right|\\
&= \sum_{T\subseteq S}(-1)^{|T|-1} \left|f\left(\min_{j\in T}{x_j}\right)\right|\\
\end{aligned}
$$

Suy ra đẳng thức cho $\max$, $\min$ tương tự.

Công thức này quan trọng vì vẫn đúng với kỳ vọng:

$$
E\left(\max_{i\in S}{x_i}\right)=\sum_{T\subseteq S}{(-1)^{|T|-1}E\left(\min_{j\in T}{x_j} \right)}
$$

$$
E\left(\min_{i\in S}{x_i}\right)=\sum_{T\subseteq S}{(-1)^{|T|-1}E\left(\max_{j\in T}{x_j} \right)}
$$

**Chứng minh:** Viết kỳ vọng theo tổng trên mọi dãy $y$ rồi đổi thứ tự tổng, tương tự chứng minh trên.

Còn mạnh hơn:

$$
\underset{i\in S}{\operatorname{kthmax}{x_i}}=\sum_{T\subseteq S}{(-1)^{|T|-k}\dbinom {|T|-1}{k-1}\min_{j\in T}{x_j}}
$$

$$
\underset{i\in S}{\operatorname{kthmin}{x_i}}=\sum_{T\subseteq S}{(-1)^{|T|-k}\dbinom {|T|-1}{k-1}\max_{j\in T}{x_j}}
$$

$$
E\left(\underset{i\in S}{\operatorname{kthmax}{x_i}}\right)=\sum_{T\subseteq S}{(-1)^{|T|-k}\dbinom {|T|-1}{k-1}E\left(\min_{j\in T}{x_j}\right)}
$$

$$
E\left(\underset{i\in S}{\operatorname{kthmin}{x_i}}\right)=\sum_{T\subseteq S}{(-1)^{|T|-k}\dbinom {|T|-1}{k-1}E\left(\max_{j\in T}{x_j}\right)}
$$

Quy ước nếu $n< m$ thì $\dbinom nm=0$.

**Chứng minh:** Không mất tính tổng quát, giả sử $x_1\le\cdots\le x_n$. Khi đó:

$$
\begin{aligned}
\sum_{T\subseteq S}{(-1)^{|T|-k}\dbinom {|T|-1}{k-1}\min_{j\in T}{x_j}}
&=\sum_{i\in S}{x_i\sum_{T\subseteq S}{(-1)^{|T|-k}\dbinom {|T|-1}{k-1}\left[x_i=\min_{j\in T}{x_j} \right]}}\\
&=\sum_{i\in S}{x_i\sum_{j=k}^n{\dbinom {n-i}{j-1}\dbinom {j-1}{k-1}(-1)^{j-k}}}
\end{aligned}
$$

Dùng $\dbinom ab\dbinom bc=\dbinom ac\dbinom {a-c}{b-c}$:

$$
\begin{aligned}
&=\sum_{i\in S}{x_i\sum_{j=k}^n{\dbinom {n-i}{k-1}\dbinom {n-i-k+1}{j-k}(-1)^{j-k}}}\\
&=\sum_{i\in S}{\dbinom {n-i}{k-1}x_i\sum_{j=0}^{n-i-k+1}{\dbinom {n-i-k+1}j(-1)^{j}}}
\end{aligned}
$$

Khi $i=n-k+1$, tổng trong bằng 1, còn lại bằng 0. Suy ra đúng. Các công thức còn lại tương tự.

Từ min-max còn có:

$$
\underset{i\in S}{\operatorname{lcm}}{x_i}=\prod_{T\subseteq S}{\left(\gcd_{j\in T}{x_j} \right)^{(-1)^{|T|-1}}}
$$

Vì $\operatorname{lcm},\gcd,a^{1},a^{-1}$ lần lượt như $\max,\min,+,-$ trong mũ.

## PKUWC2018 Random Walk

???+ note "[PKUWC2018 随机游走](https://loj.ac/problem/2542)"
    Cho một cây $n$ đỉnh, bắt đầu ở $x$, mỗi bước chọn ngẫu nhiên một cạnh kề để đi. Có $Q$ truy vấn, mỗi truy vấn cho tập $S$, hỏi kỳ vọng số bước để đã đi qua tất cả đỉnh trong $S$ ít nhất một lần. Đỉnh $x$ được coi đã đi qua ngay từ đầu. Mod $998244353$.
    
    $1\le n\le 18,1\le Q\le 5000,1\le |S|\le n$.

Đặt $x_i$ là thời gian lần đầu đến đỉnh $i$, cần:

$$
E\left(\max_{i\in S}x_i\right)
$$

Dùng min-max:

$$
E\left(\max_{i\in S}x_i\right)
=\sum_{T\subseteq S}(-1)^{|T|-1}E\left(\min_{i\in T}x_i\right)
$$

Với tập $T$, đặt $F(T)=E(\min_{i\in T}x_i)$. Đây là kỳ vọng thời gian lần đầu tới một đỉnh trong $T$.

Gọi $f(i)$ là kỳ vọng thời gian từ $i$ đến $T$:

-   Nếu $i\in T$, $f(i)=0$.
-   Nếu $i\notin T$, $f(i)=1+\frac{1}{\text{deg}(i)}\sum_{(i,j)\in E}f(j)$.

Giải trực tiếp bằng Gauss $O(n^3)$, tổng $O(2^nn^3)$ không được. Dùng khử trên cây.

Chọn gốc $1$, cha $p_u$. Với lá $i$, $f(i)$ chỉ phụ thuộc cha, viết $f(i)=A_i+B_if(p_i)$.

Với nút $i$ có con $j_1,\cdots,j_k$, vì $f(j_e)=A_{j_e}+B_{j_e}f(i)$, suy ra:

$$
f(i)=1+\frac{1}{\deg(i)}\sum_{e=1}^k\left(A_{j_e}+B_{j_e}f(i)\right)+\frac{f(p_i)}{\deg(i)}
$$

Suy ra:

$$
f(i)=\frac{\deg(i)+\sum_{e=1}^kA_{j_e}}{\deg(i)-\sum_{e=1}^kB_{j_e}}+
\frac{f(p_i)}{\deg(i)-\sum_{e=1}^kB_{j_e}}
$$

Tức vẫn là $A_i+B_if(p_i)$. Đẩy lên đến gốc:

$$
f(1)=\frac{\deg(1)+\sum_{e=1}^kA_{j_e}}{\deg(1)-\sum_{e=1}^kB_{j_e}}
$$

Tính $f(1)$, rồi đẩy xuống để có $f(i)$, nên $F(T)=f(x)$. Mỗi $T$ tính $O(n)$, tổng $O(2^nn)$.

Quay lại:

$$
E(\max_{i\in S}x_i)=\sum_{T\subseteq S}(-1)^{|T|-1}F(T)
$$

Đặt $F'(T)=(-1)^{|T|-1}F(T)$, thì $E(\max_{i\in S}x_i)=\sum_{T\subseteq S}F'(T)$. Dùng FMT/FWT trong $O(2^nn)$ để trả lời mọi $S$.

### Bài tập

-   [ABC331- G - Collect Them All](https://atcoder.jp/contests/abc331/tasks/abc331_g)
-   [洛谷 P4707 重返现世](https://www.luogu.com.cn/problem/P4707)

## Tài liệu tham khảo

[浅探容斥原理 - 王迪](https://github.com/OI-wiki/libs/blob/master/%E9%9B%86%E8%AE%AD%E9%98%9F%E5%8E%86%E5%B9%B4%E8%AE%BA%E6%96%87/%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F2013%E8%AE%BA%E6%96%87%E9%9B%86.pdf)，2013 年信息学奥林匹克中国国家队候选队员论文集

[有标号的 DAG 计数系列问题 - Cyhlnj](https://www.cnblogs.com/cjoieryl/p/10078167.html)

[全序关系 - Wikipedia](https://en.wikipedia.org/wiki/Total_order)
