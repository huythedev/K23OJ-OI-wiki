author: chu-yuehan

SAT là viết tắt của bài toán **tính thỏa mãn** (Satisfiability). Dạng tổng quát là bài toán **k-thỏa mãn**, gọi tắt là **k-SAT**. Khi $k>2$ thì bài toán là **NP-đầy đủ**, vì vậy ta chỉ xét trường hợp $k=2$.

## Định nghĩa

**2-SAT**, nói đơn giản, là bài toán: cho $n$ mệnh đề logic, mỗi mệnh đề liên quan đến đúng hai biến, ví dụ $a \vee b$, nghĩa là trong hai điều kiện $a, b$ phải có **ít nhất một** điều đúng. Nhiệm vụ là kiểm tra xem có tồn tại một cách gán giá trị cho các biến để thỏa tất cả mệnh đề hay không. Rõ ràng có thể có nhiều cách gán; đa số bài chỉ yêu cầu tìm **một** nghiệm bất kỳ. Ngoài ra, $\neg a$ là phủ định của $a$.

## Ý tưởng giải

???+ example "[洛谷 P4782「模板」2-SAT](https://www.luogu.com.cn/problem/P4782)"
Có $n$ biến Boolean $x_1\sim x_n$, và $m$ điều kiện cần thỏa, mỗi điều kiện có dạng: “$x_i$ là `true`/`false` **hoặc** $x_j$ là `true`/`false`”.
Ví dụ: “$x_1$ đúng hoặc $x_3$ sai”, “$x_7$ sai hoặc $x_2$ sai”.

```
Mục tiêu của 2-SAT là gán giá trị cho từng biến sao cho mọi điều kiện đều được thỏa mãn.
```

Biểu diễn bài toán bằng các mệnh đề Boolean. Gọi $a$ là “$x_a$ đúng” (khi đó $\neg a$ là “$x_a$ sai”). Nếu có một điều kiện $(a \vee b)$ (tức $a, b$ có ít nhất một điều đúng). Ta xây dựng **đồ thị có hướng** thể hiện quan hệ kéo theo giữa các điều kiện: dùng các đỉnh để biểu diễn “$a$ đúng” và “$a$ sai”. Khi có mệnh đề $(a \vee b)$, ta thêm hai cạnh:

* $\neg a \to b$
* $\neg b \to a$

Ý nghĩa: nếu $a$ **không đúng** thì $b$ **bắt buộc đúng**; tương tự, nếu $b$ **không đúng** thì $a$ **bắt buộc đúng**. Sau khi xây đồ thị, ta có thể dùng thuật toán **co SCC** để giải 2-SAT.

|     Mệnh đề gốc    |             Xây cạnh             |
| :----------------: | :------------------------------: |
|   $\neg a \vee b$  | $a \to b$ và $\neg b \to \neg a$ |
|     $a \vee b$     | $\neg a \to b$ và $\neg b \to a$ |
| $\neg a\vee\neg b$ | $a \to \neg b$ và $b \to \neg a$ |

Rất nhiều bài 2-SAT đều quy về việc tìm các quan hệ dạng: nếu $a$ **không thỏa**, thì $b$ **phải thỏa**.

## Giải bài toán

Hãy xét ý nghĩa của việc hai đỉnh nằm trong cùng một **thành phần liên thông mạnh** (SCC). Từ ý nghĩa logic của các cạnh suy ra: nếu hai đỉnh nằm trong cùng một SCC, thì hai điều kiện mà chúng biểu diễn sẽ **hoặc cùng đúng, hoặc cùng sai**.

Sau khi xây đồ thị, ta dùng [thuật toán Tarjan để tìm SCC](./scc.md). Với mỗi biến Boolean $a$, kiểm tra xem đỉnh biểu diễn “$a$ đúng” và đỉnh biểu diễn “$a$ sai” có nằm trong cùng một SCC không. Nếu có, thì **vô nghiệm** (một biến không thể vừa đúng vừa sai). Nếu không, thì **có nghiệm**.

Khi cần xuất một phương án, có thể dựa vào **thứ tự topo** trên đồ thị sau khi co SCC để quyết định giá trị biến. Nếu biến $x$ có thứ tự topo **sau** $\neg x$ thì gán $x$ là đúng. Khi áp dụng với Tarjan: nếu SCC của $x$ có “thứ tự topo” **sau** SCC của $\neg x$ thì gán $x$ là đúng. Lưu ý rằng trong Tarjan, do cơ chế stack, các SCC được đánh số theo **phản topo** (SCC “gần lá hơn” thường có số nhỏ hơn).

Thuật toán duyệt toàn bộ đồ thị. Do đồ thị có kích thước cùng bậc theo $n$ và $m$, phần xuất nghiệm tính trong $O(n)$, nên tổng độ phức tạp là $O(n)$.

??? note "代码实现"
`cpp     --8<-- "docs/graph/code/2-sat/2-sat_3.cpp"
    `

## Bài tập ví dụ

### Ví dụ 1

???+ example "[HDU3062 Party](https://acm.hdu.edu.cn/showproblem.php?pid=3062)"
Có $n$ cặp vợ chồng được mời đến một buổi tiệc. Do hạn chế về địa điểm, mỗi cặp chỉ có thể có **một người** tham dự. Trong $2n$ người, một số cặp người có mâu thuẫn lớn (dĩ nhiên giữa vợ chồng thì không có mâu thuẫn); hai người mâu thuẫn sẽ không thể cùng xuất hiện tại buổi tiệc. Hỏi có thể có đúng $n$ người cùng tham dự hay không?

Theo phân tích ở trên, nếu “chồng của cặp $a_1$” mâu thuẫn với “vợ của cặp $a_2$”, ta nối cạnh giữa “chồng của $a_1$” và “chồng của $a_2$”, đồng thời nối cạnh giữa “vợ của $a_2$” và “vợ của $a_1$”. Sau đó co SCC và tô/kiểm tra như thường là được.

??? note "参考代码"
`cpp     --8<-- "docs/graph/code/2-sat/2-sat_1.cpp"
    `

### Ví dụ 2

???+ example "[2018-2019 ACM-ICPC Asia Seoul Regional K TV Show Game](https://codeforces.com/gym/101987/problem/K)"
Có $k$ bóng đèn, mỗi bóng là đỏ hoặc xanh, nhưng ban đầu không biết màu. Có $n$ người, mỗi người chọn 3 bóng và đoán màu của chúng. Nếu một người đoán đúng **ít nhất 2** bóng thì sẽ nhận được quà. Hãy xác định xem có tồn tại một cách tô màu cho các bóng để ai cũng nhận được quà không; nếu có thì in ra một cách tô màu.

Theo [伍昱 -《由对称性解 2-sat 问题》](https://github.com/OI-wiki/libs/blob/master/%E9%9B%86%E8%AE%AD%E9%98%9F%E5%8E%86%E5%B9%B4%E8%AE%BA%E6%96%87/%E5%9B%BD%E5%AE%B6%E9%9B%86%E8%AE%AD%E9%98%9F2003%E8%AE%BA%E6%96%87%E9%9B%86/%E4%BC%8D%E6%98%B1--%E7%94%B1%E5%AF%B9%E7%A7%B0%E6%80%A7%E8%A7%A32-SAT%E9%97%AE%E9%A2%98/%E4%BC%8D%E6%98%B1.ppt), ta có thể rút ra: để xuất một nghiệm của 2-SAT, chỉ cần sau khi Tarjan co SCC tạo ra DAG, ta thực hiện quá trình **chọn và loại bỏ từ dưới lên** trên DAG đó.

Khi hiện thực, có thể dựng **đồ thị ngược** của DAG rồi topo trên đồ thị ngược; hoặc dựa vào tính chất: sau khi Tarjan co SCC, **SCC có số nhỏ hơn thì gần các đỉnh lá hơn**, nên ưu tiên chọn các nút thuộc SCC có số nhỏ.

Dưới đây là mã cho cách hiện thực thứ hai.

??? note "参考代码"
`cpp     --8<-- "docs/graph/code/2-sat/2-sat_2.cpp"
    `

## Bài tập

* [洛谷 P5782 和平委员会](https://www.luogu.com.cn/problem/P5782)
* [POJ3683 Priest John's Busiest Day](http://poj.org/problem?id=3683)
