Trang này sẽ giới thiệu ngắn gọn về khái niệm, cách cài đặt và ứng dụng của đồ thị Euler.

## Định nghĩa

Trong phạm vi bài này, chỉ xét đồ thị hữu hạn.

Trong lý thuyết đồ thị, **đường đi Euler (Eulerian path)** là một đường đi qua mỗi cạnh của đồ thị đúng một lần, **chu trình Euler (Eulerian circuit)** là một chu trình đi qua mỗi cạnh đúng một lần.
Nếu một đồ thị tồn tại chu trình Euler, đồ thị đó được gọi là **đồ thị Euler (Eulerian graph)**; nếu không tồn tại chu trình Euler nhưng tồn tại đường đi Euler, đồ thị đó được gọi là **bán Euler (semi-Eulerian graph)**.

??? warning "Cảnh báo"
    Trong định nghĩa này sử dụng từ "đường đi", nhưng thực chất khái niệm chính xác ở đây là "vết" (trail). Đường đi Euler và chu trình Euler chỉ yêu cầu mỗi cạnh đi qua đúng một lần, không giới hạn số lần đi qua đỉnh.

## Tính chất

Sau đây, giả sử đồ thị $G$ đang xét không có đỉnh cô lập. Giả thiết này không làm mất tính tổng quát, vì với đồ thị $G$ có đỉnh cô lập, các tính chất sau vẫn đúng với đồ thị $G'$ thu được sau khi xóa các đỉnh cô lập khỏi $G$.

Với đồ thị liên thông $G$, ba tính chất sau là tương đương:

1.  $G$ là đồ thị Euler;
2.  Mọi đỉnh của $G$ đều có bậc chẵn (với đồ thị có hướng, mỗi đỉnh có số bậc vào bằng số bậc ra);
3.  $G$ có thể phân tách thành hợp của một số chu trình không trùng cạnh.

Sau đây là chứng minh sự tương đương:

Nếu một đồ thị $G$ là đồ thị Euler, thì mọi đỉnh của $G$ đều có bậc chẵn: Xét bắt đầu từ một đỉnh bất kỳ, đi theo chu trình Euler một vòng, mỗi đỉnh $v$ sẽ có số lần rời khỏi bằng số lần đi vào. Do đường đi là chu trình, nên với mỗi đỉnh $v$, số lần rời khỏi bằng số lần đi vào, tức là bậc của $v$ là số chẵn $2k$.
Đặc biệt, với đồ thị có hướng, theo cách chứng minh tương tự, mỗi đỉnh có số bậc vào bằng số bậc ra.

Nếu mọi đỉnh của $G$ đều có bậc chẵn (hoặc bậc vào = bậc ra), thì $G$ có thể phân tách thành hợp không giao nhau của một số chu trình không trùng cạnh: Bắt đầu từ một đỉnh $u$ bất kỳ, chọn một cạnh xuất phát $(u, v)$, đi đến đỉnh $v$ và xóa cạnh $(u, v)$, tiếp tục như vậy cho đến khi quay lại $u$. Có thể chứng minh quá trình này chắc chắn sẽ quay lại $u$: mỗi khi đến một đỉnh mới $v \neq u$, do bậc còn lại của $v$ là lẻ, nên luôn tồn tại cạnh để đi tiếp, quá trình chỉ dừng lại khi quay về $u$. Vì số cạnh hữu hạn, chắc chắn sẽ dừng lại, thu được một chu trình. Lặp lại quá trình này cho đến khi không còn cạnh nào, ta phân tách được $G$ thành các chu trình không trùng cạnh.
Hơn nữa, mỗi chu trình có thể tiếp tục phân tách thành các vòng đơn giản tại các đỉnh đi qua nhiều lần, nên có thể thay "chu trình" bằng "vòng đơn giản" trong tính chất trên.

Nếu một đồ thị liên thông $G$ có thể phân tách thành hợp không giao nhau của một số chu trình không trùng cạnh, thì $G$ là đồ thị Euler: Với một tập các chu trình không trùng cạnh, mỗi lần chọn hai chu trình có chung đỉnh rồi gộp lại thành một chu trình lớn hơn, lặp lại cho đến khi không còn hai chu trình nào có chung đỉnh.
Có thể chứng minh quá trình này sẽ kết thúc với một chu trình duy nhất. Với hai chu trình $P_1, P_2$ không trùng cạnh, nếu chúng có chung đỉnh thì gộp trực tiếp; nếu không, chọn $v_1$ trên $P_1$ và $v_2$ trên $P_2$, do $G$ liên thông nên tồn tại đường đi nối $v_1$ và $v_2$ gồm các cạnh $e_1, e_2, \ldots, e_k$, mỗi cạnh $e_i$ thuộc một chu trình $C_i$, và $P_1$ nối với $C_1$, $C_i$ nối với $C_{i+1}$, $C_k$ nối với $P_2$ đều có chung đỉnh (hoặc $C_i = C_{i+1}$, không ảnh hưởng chứng minh). Như vậy, mọi cặp chu trình đều có thể gộp lại, cuối cùng thu được một chu trình duy nhất gồm toàn bộ các cạnh của $G$, chính là chu trình Euler.

Các tính chất trên cũng là tiêu chuẩn nhận biết đồ thị Euler. Cụ thể, một đồ thị là Euler khi và chỉ khi các đỉnh có bậc khác 0 liên thông (hoặc mạnh liên thông với đồ thị có hướng), và mọi đỉnh đều có bậc chẵn (hoặc bậc vào = bậc ra).

Với bán Euler, tính chất tương tự: Một đồ thị bán Euler có đúng hai đỉnh bậc lẻ, và hai đỉnh này là hai đầu mút của đường đi Euler. Nối hai đỉnh này lại sẽ thu được đồ thị Euler. Ngược lại, xóa một cạnh bất kỳ khỏi đồ thị Euler sẽ thu được đồ thị bán Euler.
Từ đó, tiêu chuẩn nhận biết đồ thị bán Euler: Một đồ thị là bán Euler khi và chỉ khi các đỉnh có bậc khác 0 liên thông (hoặc mạnh liên thông với đồ thị có hướng), và có đúng hai đỉnh bậc lẻ. Với đồ thị có hướng, điều kiện thứ hai là tồn tại đúng hai đỉnh $u, v$ sao cho $\deg^+(u) - \deg^-(u) = 1, \deg^+(v) - \deg^-(v) = -1$, các đỉnh còn lại có bậc vào = bậc ra.

## Xây dựng chu trình/đường đi Euler

Ở đây giới thiệu thuật toán Hierholzer, ý tưởng cốt lõi dựa trên tính chất thứ ba ở trên: đồ thị Euler có thể phân tách thành hợp của các chu trình không trùng cạnh.
Thực tế, trong phần chứng minh trên đã mô tả đầy đủ cách gộp các chu trình không trùng cạnh thành chu trình Euler, và việc cài đặt không khó nếu sử dụng cấu trúc dữ liệu phù hợp (như danh sách liên kết để lưu các vòng).

Quy trình cụ thể của thuật toán là: đầu tiên tìm một chu trình bất kỳ làm chu trình hiện tại, mỗi lần chọn một đỉnh còn bậc dương trên chu trình hiện tại, từ đó tìm một chu trình mới và gộp vào chu trình hiện tại, lặp lại cho đến khi mọi đỉnh trên chu trình hiện tại đều không còn cạnh chưa đi qua, khi đó chu trình hiện tại chính là chu trình Euler.

Thuật toán này cũng áp dụng được cho đồ thị có hướng. Với đồ thị bán Euler, bắt đầu từ một đường đi nối hai đỉnh bậc lẻ làm đường đi hiện tại, mỗi lần chọn đỉnh còn bậc dương để tìm chu trình mới và gộp vào, cuối cùng thu được đường đi Euler.

### Cài đặt

Giả mã thuật toán Hierholzer như sau:

$$
\begin{array}{ll}
1 &  \textbf{Input. } \text{Các cạnh của đồ thị } e , \text{ mỗi phần tử của } e \text{ là } (u, v) \\
2 &  \textbf{Output. } \text{Dãy đỉnh của đường đi Euler của đồ thị đầu vào}.\\
3 &  \textbf{Method. } \\
4 &  \textbf{Function } \text{Hierholzer } (v) \\
5 &  \qquad circle \gets \text{Tìm một chu trình bắt đầu từ } v \text{ trong } e \\
6 &  \qquad \textbf{if } circle=\varnothing \\
7 &  \qquad\qquad \textbf{return } v \\
8 &  \qquad e \gets e-circle \\
9 &  \qquad \textbf{for} \text{ mỗi } v \in circle \\
10&  \qquad\qquad v \gets \text{Hierholzer}(v) \\
11&  \qquad \textbf{return } circle \\
12&  \textbf{Endfunction}\\
13&  \textbf{return } \text{Hierholzer}(\text{một đỉnh bất kỳ})
\end{array}
$$

### Phân tích độ phức tạp

Độ phức tạp của thuật toán Hierholzer là $O(|E| + |V|)$.

Lưu ý rằng, trong phân tích tính đúng đắn ở trên, quá trình tìm chu trình đơn (hoặc đường đi ban đầu với bán Euler) là **không cần quay lui**: chỉ cần đi liên tục theo các cạnh chưa đi qua là chắc chắn sẽ tìm được chu trình hoặc đường đi cần thiết, và **mỗi cạnh chỉ bị duyệt đúng một lần**.
Để tận dụng tính chất này, nên lưu đồ thị bằng các cấu trúc như danh sách kề hoặc forward star để mỗi cạnh khi đi qua có thể xóa ngay. Nếu dùng ma trận kề, mỗi lần tìm cạnh mất $O(|V|)$, tổng độ phức tạp là $O(|V||E|)$.

???+ note "Lưu ý"
    Thực ra, độ phức tạp chính xác của thuật toán này là $O(|E|)$ chứ không phải $O(|V| + |E|)$, vì có thể cài đặt chỉ phụ thuộc vào số cạnh, không phụ thuộc số đỉnh, bằng cách duy trì một danh sách liên kết các cạnh còn lại để tìm chu trình tiếp theo.

Nếu cần xuất ra đường đi/chỉnh trình Euler có thứ tự từ điển nhỏ nhất, cần sắp xếp các cạnh, độ phức tạp là $\Theta(|E|\log |E|)$ hoặc $\Theta(|E|)$ (nếu dùng sắp xếp đếm hoặc radix sort).

### Ứng dụng

Đồ thị Euler có hướng có thể dùng trong mã hóa máy tính.

Giả sử có $m$ ký tự, muốn tạo một đĩa tròn gồm $m^n$ ô, mỗi ô ghi một ký tự, sao cho trên đĩa, mỗi đoạn liên tiếp $n$ ký tự tạo thành một xâu độ dài $n$ khác nhau.
Quay một vòng ($m^n$ lần) sẽ thu được tất cả các xâu độ dài $n$ từ $m$ ký tự.

![](images/euler1.svg)

Cách xây dựng đồ thị Euler có hướng:

Gọi $S = \{a_1, a_2, \cdots, a_m\}$, xây dựng $D=\langle V, E\rangle$ như sau:

$V = \{a_{i_1}a_{i_2}\cdots a_{i_{n-1}} |a_i \in S, 1 \leq i \leq n - 1 \}$

$E = \{a_{j_1}a_{j_2}\cdots a_{j_{n-1}}|a_j \in S, 1 \leq j \leq n\}$

Quy ước quan hệ giữa đỉnh và cạnh như sau:

Mỗi đỉnh $a_{i_1}a_{i_2}\cdots a_{i_{n-1}}$ có $m$ cạnh xuất phát: $a_{i_1}a_{i_2}\cdots a_{i_{n-1}}a_r, r=1, 2, \cdots, m$.

Mỗi cạnh $a_{j_1}a_{j_2}\cdots a_{j_{n-1}}$ đi vào đỉnh $a_{j_2}a_{j_3}\cdots a_{j_{n}}$.

![](images/euler2.svg)

Đồ thị $D$ này liên thông, mỗi đỉnh có bậc vào = bậc ra (đều bằng $m$), nên $D$ là đồ thị Euler có hướng.

Tìm một chu trình Euler $C$ trên $D$, lấy ký tự cuối cùng của mỗi cạnh trong $C$ theo thứ tự, xếp thành vòng tròn trên đĩa là được.

## Bài mẫu

???+ note "[LuoGu P2731 Cưỡi ngựa sửa hàng rào](https://www.luogu.com.cn/problem/P2731)"
    Cho một đồ thị vô hướng có 500 đỉnh, hãy tìm một đường đi hoặc chu trình Euler trên đồ thị này. Nếu có nhiều đáp án, hãy xuất ra đáp án nhỏ nhất theo thứ tự từ điển.
    
    Trong bài này, đường đi/chỉnh trình Euler không cần đi qua tất cả các đỉnh.
    
    Số cạnh $m$ thỏa mãn $1\leq m \leq 1024$.

??? note "Hướng dẫn giải"
    Bài này là ứng dụng trực tiếp của thuật toán Hierholzer.
    
    Có thể dùng `std::stack<int>` để lưu đáp án, vì nếu không phải chu trình thì đoạn đó phải đặt ở cuối.
    
    Lưu ý, không nên dùng ma trận kề để lưu đồ thị, nếu không độ phức tạp sẽ thành $\Theta(nm)$. Vì cần sắp xếp cạnh, nên dùng forward star hoặc `std::vector` để lưu đồ thị. Code mẫu dùng `std::vector`.

??? note "Code mẫu"
    ```cpp
    --8<-- "docs/graph/code/euler/euler_1.cpp"
    ```

## Bài tập

-   [SGU 101 Domino](https://codeforces.com/problemsets/acmsguru/problem/99999/101)

-   [POJ 1780 Code](http://poj.org/problem?id=1780)

-   [LuoGu P1127 Chuỗi từ](https://www.luogu.com.cn/problem/P1127)

-   [LuoGu P1333 Gậy của RuiRui](https://www.luogu.com.cn/problem/P1333)

-   [LuoGu P1341 Cặp chữ cái không thứ tự](https://www.luogu.com.cn/problem/P1341)

-   [LuoGu P6066 \[USACO05JAN\]Watchcow S](https://www.luogu.com.cn/problem/P6066)

-   [LuoGu P6628 \[Kỳ thi chọn đội tuyển tỉnh 2020 B\] Con đường hoa đinh hương](https://www.luogu.com.cn/problem/P6628)

-   [LuoGu P3520 \[POI 2011\] SMI-Garbage](https://www.luogu.com.cn/problem/P3520)
