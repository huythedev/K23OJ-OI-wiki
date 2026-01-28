author: GoodCoder666, Ir1d, Marcythm, ouuan, hsfzLZH1, Xeonacid, greyqz, Chrogeek, ftxj, sshwy, LuoshuiTianyi, hyp1231, sun2snow

## Giới thiệu

Phân tích chuỗi nặng nhẹ (Heavy-Light Decomposition, viết tắt HLD) là kỹ thuật chia cây thành các chuỗi để dễ dàng quản lý thông tin trên đường đi trong cây.

Cụ thể, HLD chia cây thành nhiều chuỗi, kết hợp thành cấu trúc tuyến tính, sau đó dùng các cấu trúc dữ liệu khác để quản lý thông tin.

**Phân tích chuỗi nặng nhẹ** (còn gọi là "phân tích chuỗi", "phân tích đường") có nhiều biến thể như **phân tích chuỗi nặng** (heavy chain), **phân tích chuỗi dài** (long chain), và phân tích dùng cho Link/Cut Tree (đôi khi gọi là "phân tích chuỗi thực"). Nếu không nói rõ, "phân tích chuỗi" thường chỉ "phân tích chuỗi nặng".

Phân tích chuỗi nặng nhẹ đảm bảo mọi đường đi trên cây có thể chia thành không quá $O(\log n)$ chuỗi liên tiếp, các đỉnh trên mỗi chuỗi có độ sâu khác nhau (chuỗi đi từ dưới lên, LCA của chuỗi là một đầu mút).

HLD còn đảm bảo các đỉnh trên mỗi chuỗi có thứ tự DFS liên tiếp, nên có thể dùng các cấu trúc dữ liệu quản lý dãy số (như segment tree) để quản lý thông tin đường đi. Ví dụ:

1.  Cập nhật giá trị các đỉnh trên đường đi giữa hai đỉnh bất kỳ.
2.  Truy vấn tổng/giá trị lớn nhất/các thông tin khác trên đường đi giữa hai đỉnh bất kỳ (miễn là có thể quản lý trên dãy số).

Ngoài việc phối hợp với cấu trúc dữ liệu để quản lý đường đi, HLD còn có thể dùng để tìm LCA trong $O(\log n)$ (với hằng số nhỏ). Một số bài toán còn tận dụng các tính chất đặc biệt của HLD.

## Phân tích chuỗi nặng

Một số định nghĩa:

-   **Con nặng** của một đỉnh là con có cây con lớn nhất. Nếu có nhiều con cùng lớn nhất, chọn một. Nếu không có con, không có con nặng.
-   **Con nhẹ** là các con còn lại.
-   Cạnh nối tới con nặng gọi là **cạnh nặng**.
-   Cạnh nối tới con nhẹ gọi là **cạnh nhẹ**.
-   Nhiều cạnh nặng nối tiếp tạo thành **chuỗi nặng**.
-   Đỉnh lẻ cũng coi là chuỗi nặng, nên toàn bộ cây được chia thành nhiều chuỗi nặng.

Minh họa:

![HLD](./images/hld.png)

## Cài đặt

HLD gồm hai lần DFS. Pseudocode:

DFS đầu tiên ghi nhận cha ($\textit{father}$), độ sâu ($\textit{depth}$), kích thước cây con ($\textit{size}$), con nặng ($\textit{hson}$).

$$
\begin{array}{l}
\text{TREE-BUILD }(u,\textit{dep}) \\
\begin{array}{ll}
1 & u.\textit{hson}\gets 0 \\
2 & u.\textit{hson}.\textit{size}\gets 0 \\
3 & u.\textit{depth}\gets \textit{dep} \\
4 & u.\textit{size}\gets 1 \\
5 & \textbf{for }\text{each son }v\text{ of }u \\
6 & \qquad u.\textit{size}\gets u.\textit{size} + \text{TREE-BUILD }(v,\textit{dep}+1) \\
7 & \qquad v.\textit{father}\gets u \\
8 & \qquad \textbf{if }v.\textit{size}> u.\textit{hson}.\textit{size} \\
9 & \qquad \qquad u.\textit{hson}\gets v \\
10 & \textbf{return } u.\textit{size}
\end{array}
\end{array}
$$

DFS thứ hai ghi nhận đỉnh đầu chuỗi ($\textit{top}$, khởi tạo là chính nó), thứ tự DFS khi duyệt chuỗi nặng ($\textit{top}$), và ánh xạ thứ tự DFS sang đỉnh ($\textit{rank}$).

$$
\begin{array}{l}
\text{TREE-DECOMPOSITION }(u,\textit{top}) \\
\begin{array}{ll}
1 & u.\textit{top}\gets \textit{top} \\
2 & \textit{tot}\gets \textit{tot}+1\\
3 & u.\textit{dfn}\gets \textit{tot} \\
4 & \textit{rank}(\textit{tot})\gets u \\
5 & \textbf{if }u.\textit{hson}\text{ is not }0 \\
6 & \qquad \text{TREE-DECOMPOSITION }(u.\textit{hson},\textit{top}) \\
7 & \qquad \textbf{for }\text{each son }v\text{ of }u \\
8 & \qquad \qquad \textbf{if }v\text{ is not }u.\textit{hson} \\
9 & \qquad \qquad \qquad \text{TREE-DECOMPOSITION }(v,v) 
\end{array}
\end{array}
$$

Mã tham khảo:

-   $\operatorname{fa}(x)$: cha của $x$.
-   $\operatorname{dep}(x)$: độ sâu của $x$.
-   $\operatorname{siz}(x)$: kích thước cây con của $x$.
-   $\operatorname{son}(x)$: con nặng của $x$.
-   $\operatorname{top}(x)$: đầu chuỗi nặng chứa $x$.
-   $\operatorname{dfn}(x)$: thứ tự DFS của $x$, cũng là vị trí trên segment tree.
-   $\operatorname{rnk}(x)$: ánh xạ từ thứ tự DFS sang đỉnh, $\operatorname{rnk}(\operatorname{dfn}(x))=x$.

Hai lần DFS: lần 1 tính $\operatorname{fa}(x)$, $\operatorname{dep}(x)$, $\operatorname{siz}(x)$, $\operatorname{son}(x)$; lần 2 tính $\operatorname{top}(x)$, $\operatorname{dfn}(x)$, $\operatorname{rnk}(x)$.

```cpp
void dfs1(int u, int f) {
  fa[u] = f, dep[u] = dep[f] + 1, siz[u] = 1;
  for (auto v : G[u]) {
    if (v == f) continue;
    dfs1(v, u);
    siz[u] += siz[v];
    if (siz[v] > siz[son[u]]) son[u] = v;
  }
}

void dfs2(int u, int ftop) {
  top[u] = ftop, dfn[u] = ++idx, rnk[idx] = u;
  if (son[u]) dfs2(son[u], ftop);
  for (auto v : G[u])
    if (v != son[u] && v != fa[u]) dfs2(v, v);
}
```

## Tính chất của phân tích chuỗi nặng

**Mỗi đỉnh thuộc duy nhất một chuỗi nặng.**

Đầu chuỗi nặng không nhất thiết là con nặng (vì cạnh nặng xác định cho từng đỉnh).

Các chuỗi nặng chia cây thành các chuỗi không giao nhau.

Khi duyệt chuỗi nặng trước, thứ tự DFS trên chuỗi là liên tiếp. Sắp xếp theo DFN sẽ ra chuỗi.

Thứ tự DFS của cây con là liên tiếp.

Khi đi qua một cạnh nhẹ, kích thước cây con giảm ít nhất một nửa.

Với mọi đường đi trên cây, chia thành hai đoạn từ LCA xuống hai đầu, mỗi đoạn đi qua tối đa $O(\log n)$ chuỗi nặng.

??? info "Làm sao để kiểm tra HLD có thực sự $O(\log n)$?"
    Thường thì hằng số của HLD rất nhỏ, khó kiểm tra. Nếu muốn kiểm tra, có thể xây cây nhị phân có độ sâu thấp.

    Có thể dùng phương pháp trung gian.

    Xây cây nhị phân $\sqrt{n}$ đỉnh, mỗi cạnh nối tới con thay bằng chuỗi dài $\sqrt{n}$.

    Như vậy, số lần chuyển chuỗi khi truy vấn ngẫu nhiên là trung bình $\frac{\log n}{2}$, độ sâu $O(\sqrt{n} \log n)$.

    Thêm một số lá ngẫu nhiên có thể kiểm tra HLD. Tuy nhiên, hằng số nhỏ nên khó kiểm tra.

## Ứng dụng phổ biến

### Quản lý đường đi

Dùng HLD để tính tổng trọng số trên đường đi giữa hai đỉnh, pseudocode:

$$
\begin{array}{l}
\text{TREE-PATH-SUM }(u,v) \\
\begin{array}{ll}
1 & \textit{tot}\gets 0 \\
2 & \textbf{while }u.\textit{top}\text{ is not }v.\textit{top} \\
3 & \qquad \textbf{if }u.\textit{top}.\textit{depth}< v.\textit{top}.\textit{depth} \\
4 & \qquad \qquad \text{SWAP}(u, v) \\
5 & \qquad \textit{tot}\gets \textit{tot} + \text{sum of values between }u\text{ and }u.\textit{top} \\
6 & \qquad u\gets u.\textit{top}.\textit{father} \\
7 & \textit{tot}\gets \textit{tot} + \text{sum of values between }u\text{ and }v \\
8 & \textbf{return } \textit{tot} 
\end{array}
\end{array}
$$

Thứ tự DFS trên chuỗi là liên tiếp, có thể dùng segment tree, BIT để quản lý.

Mỗi lần nhảy lên chuỗi có độ sâu lớn hơn, đến khi hai đỉnh cùng chuỗi.

Cách nhảy chuỗi này cũng dùng cho các truy vấn khác trên đường đi.

### Quản lý thông tin cây con

Đôi khi cần quản lý thông tin trên cây con, ví dụ tăng trọng số mọi đỉnh trong cây con gốc $x$.

Khi DFS, thứ tự DFS của cây con là liên tiếp.

Mỗi đỉnh lưu bottom là đỉnh cuối cùng của cây con trên DFS.

Như vậy, thông tin cây con chuyển thành một đoạn liên tiếp.

### Tìm LCA

Nhảy lên chuỗi nặng, khi hai đỉnh cùng chuỗi, đỉnh có độ sâu nhỏ hơn là LCA.

Khi nhảy, luôn nhảy chuỗi có độ sâu lớn hơn.

Mã tham khảo:

```cpp
int lca(int u, int v) {
  while (top[u] != top[v]) {
    if (dep[top[u]] > dep[top[v]])
      u = fa[top[u]];
    else
      v = fa[top[v)];
  }
  return dep[u] > dep[v] ? v : u;
}
```

### Đổi gốc

Xét bài toán: ngoài các thao tác cơ bản, còn có thao tác đổi gốc.

Vì HLD quản lý thông tin tĩnh, không hỗ trợ cập nhật động. Không thể mỗi lần đổi gốc lại tiền xử lý, quá tốn thời gian. Cần tận dụng thông tin đã có.

Với truy vấn đường đi, vì đường đi giữa hai đỉnh là duy nhất, không bị ảnh hưởng, xử lý như bình thường.

Với truy vấn cây con, cần ánh xạ cây con sau khi đổi gốc về cây con ban đầu. Phải xét vị trí của gốc mới, gốc cũ và đỉnh truy vấn. Chi tiết xem [phần ví dụ](./hld.md#loj-139-树链剖分).

## Ví dụ

Dưới đây là một số ví dụ ứng dụng HLD. Đầu tiên là bài mẫu.

???+ example "[「ZJOI2008」Thống kê trên cây](https://loj.ac/problem/10138)"
    Cho cây $n$ đỉnh có trọng số, thực hiện $q$ thao tác:
    
    1.  Đổi trọng số một đỉnh;
    2.  Truy vấn giá trị lớn nhất trên đường đi giữa hai đỉnh;
    3.  Truy vấn tổng trọng số trên đường đi giữa hai đỉnh.
    
    $1\le n\le 30000$, $0\le q\le 200000$.

??? note "Giải thích"
    Theo đề và các tính chất trên, segment tree cần hỗ trợ:
    
    1.  Đổi trọng số một đỉnh;
    2.  Truy vấn giá trị lớn nhất trên đoạn;
    3.  Truy vấn tổng trên đoạn.
    
    Đổi trọng số một đỉnh rất dễ.
    
    Vì thứ tự DFS của cây con là liên tiếp (không cần HLD), đổi trọng số cây con chỉ cần cập nhật đoạn liên tiếp.
    
    Vấn đề là làm sao cập nhật/truy vấn đường đi giữa hai đỉnh.
    
    Xét cách tìm LCA bằng phương pháp nhảy bậc: đưa hai đỉnh lên cùng độ sâu, rồi nhảy lên cùng nhau. Với HLD cũng vậy.
    
    Khi nhảy, nếu đang ở chuỗi nặng, nhảy lên đầu chuỗi; nếu không, nhảy lên một đỉnh. Lặp lại đến khi hai đỉnh trùng nhau. Cập nhật/truy vấn trên đoạn tương ứng.
    
    Mỗi truy vấn đi qua tối đa $O(\log n)$ chuỗi nặng, mỗi chuỗi truy vấn segment tree $O(\log n)$, tổng $O(n\log n+q\log^2 n)$. Thực tế số chuỗi nặng khó đạt $O(\log n)$ (chỉ khi cây nhị phân đầy), nên HLD thường có hằng số nhỏ.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/graph/code/hld/hld_1.cpp"
    ```

Tiếp theo là bài mẫu có đổi gốc.

<a id="loj-139-树链剖分"></a>

???+ example "[LOJ 139. Phân tích chuỗi trên cây](https://loj.ac/p/139)"
    Cho cây $n$ đỉnh (gốc ban đầu là $1$), thực hiện $m$ thao tác:
    
    -   Đổi gốc, đặt $u$ làm gốc mới;
    -   Tăng trọng số các đỉnh trên đường đi giữa $u$ và $v$ lên $w$;
    -   Tăng trọng số các đỉnh trong cây con gốc $u$ lên $w$;
    -   Truy vấn tổng trọng số các đỉnh trên đường đi giữa $u$ và $v$;
    -   Truy vấn tổng trọng số các đỉnh trong cây con gốc $u$.
    
    $1 \le n,m \le 10^5$.

??? note "Giải thích"
    Tiền xử lý DFS với gốc $1$, lưu thông tin HLD. Gọi cây gốc $1$ là "cây gốc", cây sau khi đổi gốc là "cây hiện tại". Khi thao tác, cần ánh xạ truy vấn/cập nhật về cây gốc.

    Đổi gốc: gán $\textit{root}\gets u$. Truy vấn/cập nhật đường đi: không bị ảnh hưởng, xử lý như bình thường.

    Truy vấn/cập nhật cây con: cần xét vị trí của $u$ và $\textit{root}$ trên cây gốc:

    -   $u = \textit{root}$: thao tác trên toàn bộ cây, cập nhật/truy vấn segment tree gốc.
    -   $u$ là tổ tiên của $\textit{root}$ trên cây gốc (nằm trên đường từ $1$ tới $\textit{root}$):

        Trường hợp này cần chú ý. Gọi $v$ là đỉnh trên đường từ $u$ tới $\textit{root}$ (trừ $u$) có độ sâu nhỏ nhất. Khi đó, phần ngoài cây con gốc $v$ trên cây gốc chính là cây con gốc $u$ trên cây hiện tại.

        Làm sao tìm $v$? Đầu tiên gán $v\gets\textit{root}$, rồi nhảy lên chuỗi nặng cho tới khi $\operatorname{dep}(\operatorname{top}(v))\le\operatorname{dep}(u)+1$.

        -   Nếu $\operatorname{dep}(\operatorname{top}(v))=\operatorname{dep}(u)+1$, gán $v\gets\operatorname{top}(v)$, tức $v$ là con nhẹ của $u$.
        -   Nếu $\operatorname{dep}(\operatorname{top}(v))<\operatorname{dep}(u)+1$, tức $u,v$ cùng chuỗi nặng, $v$ có $\operatorname{dfn}(v)=\operatorname{dfn}(u)+1$, nên gán $v\gets\operatorname{rnk}(\operatorname{dfn}(u)+1)$.

        Hai trường hợp này có thể gộp: sau khi nhảy, gán

        $$
        v\gets\operatorname{rnk}(\operatorname{dfn}(\operatorname{top}(v))+\operatorname{dep}(u)+1-\operatorname{dep}(\operatorname{top}(v))).
        $$

        Có thể kiểm chứng $v$ tìm được như trên là đúng. Mã tham khảo cũng dùng cách này.

        Vì cây con gốc $v$ là đoạn $[\operatorname{dfn}(v),\operatorname{dfn}(v)+\operatorname{siz}(v))$, nên chỉ cần thao tác trên $[1,\operatorname{dfn}(v))\cup[\operatorname{dfn}(v)+\operatorname{siz}(v),n]$.

    -   Trường hợp khác: thao tác trên cây con gốc $u$ như bình thường.

    Độ phức tạp như không đổi gốc, $O(n\log^2 n)$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/graph/code/hld/hld_4.cpp"
    ```

Cuối cùng là một bài tương tác, ứng dụng đặc biệt của HLD.

???+ example "[Nauuo và cây nhị phân](https://loj.ac/problem/6669)"
    Cho cây nhị phân gốc $1$, bạn có thể hỏi khoảng cách giữa hai đỉnh bất kỳ, yêu cầu xác định cha của mỗi đỉnh.

    Số đỉnh không quá $3000$, tối đa $30000$ truy vấn.

??? note "Giải thích"
    Đầu tiên hỏi $n-1$ lần để xác định độ sâu từng đỉnh.

    Sau đó, duyệt theo thứ tự tăng dần độ sâu để xác định cha từng đỉnh, khi đó các tổ tiên đều đã biết.

    Trước khi xác định cha một đỉnh, xây HLD cho phần cây đã biết.

    Giả sử cần tìm vị trí của đỉnh $k$ trong cây con gốc $u$, hỏi khoảng cách giữa $k$ và đỉnh cuối chuỗi nặng gốc $u$, xác định vị trí $k$ như hình:

    ![](./images/hld2.png)

    Đường đỏ là chuỗi nặng, $d$ là kết quả truy vấn $\textit{dis}(k, \textit{bot}(u))$, độ sâu $v$ là $(\textit{dep}(k)+\textit{dep}(\textit{bot}(u))-d)/2$.

    Nếu $v$ có một con, cha của $k$ là $v$, nếu không thì đệ quy tìm trong cây con gốc $w$.

    Độ phức tạp $O(n^2)$, số truy vấn $O(n\log n)$.

    Gọi $T(n)$ là số truy vấn tối đa để tìm vị trí một đỉnh mới trong cây $n$ đỉnh:

    $$
    T(n)\le
    \begin{cases}
    0&n=1\\
    T\left(\left\lfloor\frac{n-1}2\right\rfloor\right)+1&n\ge2
    \end{cases}
    $$

    $2999+\sum_{i=1}^{2999}T(i)\le 29940$, thực tế có thể đạt được nếu dữ liệu xấu, nhưng nếu thêm một chút ngẫu nhiên (ví dụ sắp xếp độ sâu không ổn định), số truy vấn khó vượt quá $21000$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/graph/code/hld/hld_2.cpp"
    ```

## Phân tích chuỗi dài

Phân tích chuỗi dài là một biến thể khác của phân tích chuỗi.

-   **Con nặng** là con có cây con sâu nhất. Nếu có nhiều con sâu nhất, chọn một. Nếu không có con, không có con nặng.
-   **Con nhẹ** là các con còn lại.
-   Cạnh nối tới con nặng gọi là **cạnh nặng**.
-   Cạnh nối tới con nhẹ gọi là **cạnh nhẹ**.
-   Nhiều cạnh nặng nối tiếp tạo thành **chuỗi nặng**.
-   Đỉnh lẻ cũng coi là chuỗi nặng, nên toàn bộ cây được chia thành nhiều chuỗi nặng.

Minh họa (cách chia này cũng là phân tích chuỗi nặng hoặc chuỗi dài):

![HLD](./images/hld.png)

Cách cài đặt giống phân tích chuỗi nặng, không trình bày lại.

### Ứng dụng phổ biến

Phân tích chuỗi dài đảm bảo số lần chuyển chuỗi khi đi từ một đỉnh tới gốc là $O(\sqrt{n})$.

??? info "Làm sao kiểm tra số lần chuyển chuỗi đạt tối đa?"
    Có thể xây cây nhị phân như sau:

    Gọi tham số cây là $D$.

    Nếu $D \neq 0$, xây cây nhị phân trái với tham số $D-1$, cây phải là chuỗi dài $2D-1$.

    Nếu $D = 0$, xây một lá riêng và kết thúc.

    Như vậy, các lá sẽ có đường đi tới gốc toàn cạnh nhẹ, số đỉnh $D^2$.

    Chọn $D=\sqrt{n}$ là đạt tối đa.

#### Tối ưu DP bằng phân tích chuỗi dài

Các bài DP trên cây có một chiều là độ sâu có thể tối ưu bằng phân tích chuỗi dài.

Ý tưởng: mỗi đỉnh kế thừa trạng thái DP của con nặng, các con nhẹ thì hợp trạng thái vào.

???+ example "[Codeforces 1009 F. Dominant Indices](http://codeforces.com/contest/1009/problem/F)"
    Cho cây $n$ đỉnh gốc $1$.

    Định nghĩa mảng độ sâu của đỉnh $x$ là $[d_{x, 0}, d_{x, 1}, d_{x, 2}, \dots]$, với $d_{x, i}$ là số đỉnh $y$ thỏa:

    -   $x$ là tổ tiên của $y$;
    -   Đường đi từ $x$ tới $y$ có đúng $i$ cạnh.

    Chỉ số chủ đạo của $x$ là chỉ số $j$ thỏa:

    -   Với mọi $k < j$, $d_{x, k} < d_{x, j}$;
    -   Với mọi $k > j$, $d_{x, k} \le d_{x, j}$.

    Tính chỉ số chủ đạo cho mọi đỉnh.

??? note "Giải thích"
    Gọi $f_{i,j}$ là số đỉnh trong cây con $i$ cách $i$ đúng $j$ cạnh.

    Nếu chuyển trạng thái brute-force thì $O(n^2)$.

    Ý tưởng: mỗi lần chuyển trạng thái, kế thừa mảng DP của con nặng và đáp án, sau đó cập nhật.

    Đầu tiên, thêm 1 vào đầu mảng DP của con nặng (đỉnh hiện tại).

    Sau đó, hợp mảng DP của các con nhẹ vào mảng hiện tại.

    Vì mảng DP của con nhẹ có độ dài bằng chuỗi nặng của nó, tổng độ dài các chuỗi nặng là $n$.

    Do đó, hợp mảng DP của các con nhẹ tổng $O(n)$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/graph/code/hld/hld_3.cpp"
    ```

Lưu ý: thường mảng DP được cấp phát cho cả chuỗi nặng, mỗi đỉnh có con trỏ đầu riêng.

Độ dài mảng DP có thể xác định theo độ sâu lớn nhất của cây con.

Kỹ thuật tối ưu DP bằng phân tích chuỗi dài còn nhiều biến thể, như đánh dấu,... không trình bày thêm.

Tham khảo [blog của zhoushuyu](https://www.cnblogs.com/zhoushuyu/p/9468669.html).

#### Truy vấn tổ tiên cấp k

Tức là hỏi một đỉnh nhảy lên cha $k$ lần thì tới đâu.

Giả sử đã tiền xử lý tổ tiên $2^i$ cho mỗi đỉnh.

Giả sử tìm được tổ tiên $2^i$ của đỉnh truy vấn, với $2^i \le k < 2^{i+1}$.

Tìm vị trí trên chuỗi nặng theo độ sâu. Gọi độ dài chuỗi là $d$.

Tiền xử lý cho mỗi chuỗi nặng, lưu tổ tiên từ $1$ tới $d$ cho gốc chuỗi.

Theo tính chất phân tích chuỗi dài, $k-2^i \le 2^i \leq d$, nên có thể $O(1)$ truy vấn tổ tiên cấp $k$ trên chuỗi.

Tiền xử lý cần truy vấn tổ tiên $2^i$, và lưu bảng cho mỗi chuỗi nặng.

Tiền xử lý $O(n\log n)$, truy vấn $O(1)$.

## Bài tập

-   [「Luogu P3379」【Mẫu】LCA](https://www.luogu.com.cn/problem/P3379) (HLD tìm LCA không cần cấu trúc dữ liệu, có thể luyện tập)
-   [「JLOI2014」Nhà mới của sóc](https://loj.ac/problem/2236) (cũng có thể dùng phân tích hiệu trên cây)
-   [「HAOI2015」Thao tác trên cây](https://loj.ac/problem/2125)
-   [「Luogu P3384」【Mẫu】Phân tích chuỗi nặng nhẹ](https://www.luogu.com.cn/problem/P3384)
-   [「Luogu P1505」\[Đội tuyển quốc gia\] Du lịch](https://www.luogu.com.cn/problem/P1505)
-   [「NOI2015」Quản lý phần mềm](https://uoj.ac/problem/128)
-   [「SDOI2011」Tô màu](https://www.luogu.com.cn/problem/P2486)
-   [「SDOI2014」Du lịch](https://hydro.ac/p/bzoj-P3531)
-   [「Luogu P3979」Vương quốc xa xôi](https://www.luogu.com.cn/problem/P3979)
-   [「POI2014」Hotel bản nâng cao](https://hydro.ac/p/bzoj-P4543) (phân tích chuỗi dài tối ưu DP)
-   [Chiến thuật](https://hydro.ac/p/bzoj-P3252) (phân tích chuỗi dài tối ưu tham lam)
