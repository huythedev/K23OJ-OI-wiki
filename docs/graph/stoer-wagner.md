author: DanJoshua, opsiff, yzy-1, yingqi-z20

## Định nghĩa

Do không còn khái niệm **đỉnh nguồn - đỉnh đích** (source/sink), ta cần định nghĩa lại khái niệm **cắt** (cut).

(Thực ra định nghĩa về cắt trong phần mạng lưới dòng chảy khác với Wikipedia, chỉ là thông thường ta hay gặp bài toán "cắt nhỏ nhất có nguồn đích", nên khái niệm này trở thành quy ước.)

### Cắt

Tập các cạnh mà khi loại bỏ khỏi đồ thị sẽ làm đồ thị không còn liên thông (tức là chia thành hai thành phần liên thông) được gọi là một cắt của đồ thị.

Cụ thể: với đồ thị vô hướng $G = (V, E)$, gọi $C$ là một tập các cạnh của $G$, nếu loại bỏ tất cả các cạnh trong $C$ khỏi $G$ làm $G$ không còn liên thông, thì $C$ là một cắt của $G$.

### Bài toán cắt nhỏ nhất có nguồn đích

Như trong [min-cut](./flow/min-cut.md).

### Bài toán cắt nhỏ nhất không nguồn đích

Tìm cắt có tổng trọng số các cạnh nhỏ nhất. Còn gọi là cắt nhỏ nhất toàn cục (global min-cut).

Rõ ràng, không thể dùng trực tiếp thuật toán mạng lưới dòng chảy do độ phức tạp quá lớn.

***

## Thuật toán Stoer–Wagner

### Giới thiệu

Thuật toán Stoer–Wagner được *Mechthild Stoer* và *Frank Wagner* đề xuất năm 1995, là một thuật toán giải bài toán cắt nhỏ nhất toàn cục trên đồ thị vô hướng có trọng số dương, dựa trên ý tưởng **đệ quy**.

### Tính chất

Độ phức tạp $O(|V||E| + |V|^{2}\log|V|)$, thường coi gần đúng là $O(|V|^3)$.

Thuật toán dựa trên thực tế sau: với mọi cặp đỉnh $S, T$ trong $G$, mọi cắt $C$ của $G$ hoặc là $S, T$ nằm cùng một thành phần liên thông, hoặc $C$ là một cắt $S-T$.

### Quy trình

1.  Trong đồ thị $G$, chọn hai đỉnh $s, t$ bất kỳ, coi như nguồn - đích, tìm cắt nhỏ nhất $S-T$ (gọi là *cut of phase*), cập nhật đáp án.
2.  "Gộp" hai đỉnh $s, t$ lại, nếu $|V| > 1$ thì lặp lại bước 1.
3.  Kết quả là giá trị nhỏ nhất trong tất cả các *cut of phase*.

Gộp hai đỉnh $s, t$: xóa cạnh $(s, t)$, với mọi đỉnh $k$ còn lại trong $G \setminus \{s, t\}$, xóa cạnh $(t, k)$ và cộng trọng số $d(t, k)$ vào $d(s, k)$.

Giải thích: nếu $s, t$ cùng thành phần liên thông, với mọi $k$ trong $G \setminus \{s, t\}$, nếu $(k, s) \in C_{\min}$ thì $(k, t) \in C_{\min}$ cũng đúng, ngược lại cũng vậy. Do đó, có thể coi $s, t$ là một đỉnh.

Bước 1 xét trường hợp $s, t$ khác thành phần liên thông, bước 2 xét trường hợp còn lại. Mỗi lần thực hiện bước 2, $|V|$ giảm đi 1, nên thuật toán kết thúc sau $|V| - 1$ lần.

### Cách tìm cắt nhỏ nhất S-T

(Rõ ràng không dùng mạng lưới dòng chảy.)

Sau một số lần gộp, xét đồ thị $G'=(V', E')$, thực hiện bước 1.

Tạo tập $A$, ban đầu $A = \varnothing$.

Mỗi lần, chọn đỉnh $i \notin A$ có giá trị $w(A, i)$ lớn nhất, thêm vào $A$, cho đến khi $|A| = |V'|$.

Trong đó:

$w(A, i) = \sum_{j \in A} d(i, j)$

(nếu $(i, j) \notin E'$, thì $d(i, j) = 0$).

Thứ tự các đỉnh vào $A$ là cố định, gọi $\operatorname{ord}(i)$ là thứ tự vào $A$, $t = \operatorname{ord}(|V'|)$; $\operatorname{pos}(v)$ là thứ tự thêm $v$ vào $A$.

Với mọi $s$, một cắt $s-t$ có giá trị $w(t)$.

### Chứng minh

Định nghĩa một đỉnh $v$ được "kích hoạt" khi $v$ được thêm vào $A$, và tại thời điểm đó, đỉnh cuối cùng trước $v$ trong $A$ không cùng thành phần liên thông với $v$ trong $G'' = (V', E'/C)$.

![Stoer-Wagner1](./images/Stoer-Wagner1.png)

Hình: vùng xanh và vàng là hai thành phần liên thông, số trong ngoặc là thứ tự vào $A$. Đỉnh xám là đỉnh kích hoạt, đỉnh trắng không kích hoạt.

Gọi $A_v = \{u \mid \operatorname{pos}(u) < \operatorname{pos}(v)\}$, tức là các đỉnh vào $A$ trước $v$. Gọi $E_v$ là tập cạnh của đồ thị con cảm ứng trên $A_v \cup\{v\}$. (Chú ý có cả $v$.)

Gọi cắt cảm ứng $C_v = C \cap E_v$, $w(C_v) = \sum_{(i,j) \in C_v} d(i, j)$.

???+ note "Bổ đề 1"
    Với mọi đỉnh $v$ được kích hoạt, $w(A_v, v) \le w(C_v)$.

    Chứng minh: dùng quy nạp.

    Với đỉnh đầu tiên $v_0$ được kích hoạt, theo định nghĩa $w(A_{v_0}, v_0) = w(C_{v_0})$.

    Với hai đỉnh $u, v$ được kích hoạt tiếp theo, giả sử $\operatorname{pos}(v) < \operatorname{pos}(u)$, có:

    $w(A_u, u) = w(A_v, u) + w(A_u - A_v, u)$

    Đã biết:

    $w(A_v, u) \le w(A_v, v)$ và $w(A_v, v) \le w(C_v)$

    Suy ra:

    $w(A_u, u) \le w(C_v) + w(A_u - A_v, u)$

    Vì $w(A_u - A_v, u)$ chỉ cộng vào $w(C_u)$ mà không cộng vào $w(C_v)$, với mọi cạnh dương, ta có:

    $w(A_u,u) \le w(C_u)$

    Quy nạp được chứng minh.

Vì $\operatorname{pos}(s) < \operatorname{pos}(t)$, $s, t$ khác thành phần liên thông, nên $t$ sẽ được kích hoạt, do đó $w(A_t, t) \le w(C_t) = w(C)$.

??? note "[P5632【模板】Thuật toán Stoer–Wagner](https://www.luogu.com.cn/problem/P5632)"
    ```cpp
    --8<-- "docs/graph/code/stoer-wagner/stoer-wagner_1.cpp"
    ```

***

### Phân tích độ phức tạp và tối ưu

Mỗi thao tác *contract* có độ phức tạp $O(|E| + |V|\log|V|)$.

Có tổng cộng $O(|V|)$ lần *contract*, tổng độ phức tạp $O(|E||V| + |V|^2\log|V|)$.

Theo kinh nghiệm từ [đường đi ngắn nhất](./shortest-path.md), nút thắt là tìm đỉnh có trọng số lớn nhất.

Trong mỗi lần *contract*, cần tìm $|V|$ lần đỉnh lớn nhất và cập nhật trọng số $|E|$ lần.

Heap Fibonacci hỗ trợ $O(\log|V|)$ cho lấy đỉnh lớn nhất và $O(1)$ cho cập nhật trọng số, lý thuyết đạt $O(|E| + |V|\log|V|)$, nhưng do hằng số lớn, code phức tạp, thực tế ít dùng.

(Thực tế cần bật O2 và tối ưu kỹ mới qua được các test khó.)
