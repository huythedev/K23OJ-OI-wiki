Khi cần kiểm tra hai cây có đồng cấu (isomorphic) hay không, ta thường chuyển cây thành giá trị băm (hash) để giảm độ phức tạp.

Tree Hash rất linh hoạt, có thể thiết kế nhiều kiểu băm khác nhau; tuy nhiên nếu thiết kế tùy tiện, dễ bị "hack". Dưới đây là một phương pháp dễ cài đặt và khó bị hack.

## Phương pháp

Phương pháp này cần một hàm băm cho đa tập. Giá trị hash của cây con gốc tại một đỉnh là giá trị hash của đa tập các giá trị hash của các cây con gốc tại các con của nó, tức là:

$$
h_x = f(\{ h_i \mid i \in son(x) \})
$$

Trong đó $h_x$ là hash của cây con gốc $x$, $f$ là hàm băm đa tập.

Ví dụ hàm hash dùng trong code:

$$
f(S) = \left( c + \sum_{x \in S} g(x) \right) \bmod m
$$

Với $c$ là hằng số (thường lấy $1$), $m$ là modulus (thường dùng $2^{32}$ hoặc $2^{64}$ để tràn tự nhiên, hoặc số nguyên tố lớn). $g$ là ánh xạ số nguyên sang số nguyên, ví dụ dùng xor shift, hoặc các hàm khác (không nên dùng đa thức). Để tránh bị hack, có thể xor thêm một số ngẫu nhiên trước/sau khi ánh xạ.

Cách hash này rất dễ viết. Nếu cần đổi gốc, chỉ cần DP lần hai, trừ đi hash cây con là được.

## Bài tập ví dụ

### [UOJ #763. Tree Hash](https://uoj.ac/problem/763)

Bài mẫu. Chỉ cần DFS từ gốc $1$ là xong.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-hash/tree-hash_1.cpp"
    ```

### [\[BJOI2015\] Đồng cấu cây](https://www.luogu.com.cn/problem/P5043)

Ở đây đồng cấu là vô gốc, còn phương pháp trên là cho cây có gốc. Do đó, chỉ khi chọn cùng gốc thì hai cây vô gốc mới có hash giống nhau. Với dữ liệu nhỏ, có thể brute-force hash với mọi gốc, rồi sort so sánh.

Nếu dữ liệu lớn, có thể dùng DP đổi gốc, duyệt hai lần để tính hash với mọi gốc. Hoặc, dùng hàm hash đa tập: lưu hash của mọi gốc vào một đa tập, rồi hash đa tập này để so sánh (cách 1).

Cũng có thể tối ưu bằng cách tìm trọng tâm (centroid) của cây. Một cây có tối đa hai trọng tâm, chỉ cần hash với các trọng tâm làm gốc. Sau đó, so sánh từng hash (cách 2), hoặc nếu có một trọng tâm thì lấy hash đó làm hash toàn cây, nếu có hai thì lấy min/max.

??? note "Cách 1"
    ```cpp
    --8<-- "docs/graph/code/tree-hash/tree-hash_2.cpp"
    ```

??? note "Cách 2"
    ```cpp
    --8<-- "docs/graph/code/tree-hash/tree-hash_3.cpp"
    ```

### [HDU 6647 Bracket Sequences on Tree](https://acm.hdu.edu.cn/showproblem.php?pid=6647)

Yêu cầu đếm số chuỗi ngoặc khác nhau sinh ra từ các cách duyệt cây vô gốc.

Nhận xét: hai cây có gốc không đồng cấu sẽ không sinh ra cùng chuỗi ngoặc. Đầu tiên, với cây có gốc, gọi $f(u)$ là số cách sinh chuỗi ngoặc khác nhau từ cây con gốc $u$. Khi duyệt các con của $u$ theo mọi thứ tự, có $|son(u)|!$ cách, mỗi con $v$ có $f(v)$ cách, nên $f(u)=|son(u)|! \cdot \prod_{v \in son(u)} f(v)$. Tuy nhiên, nếu có các cây con đồng cấu, sẽ bị trùng, nên cần chia cho tích giai thừa số lần xuất hiện của mỗi loại cây con (giống đếm hoán vị đa tập).

DP như trên sẽ tính được số cách cho gốc. Dùng DP đổi gốc để tính cho mọi đỉnh.

??? note "Code tham khảo"
    ```cpp
    --8<-- "docs/graph/code/tree-hash/tree-hash_4.cpp"
    ```

## Tài liệu tham khảo

Phương pháp hash trong bài tham khảo và mở rộng từ blog [一种好写且卡不掉的树哈希](https://peehs-moorhsum.blog.uoj.ac/blog/7891).
