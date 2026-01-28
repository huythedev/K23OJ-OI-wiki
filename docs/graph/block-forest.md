author: GitPinkRabbit, Early0v0, Backl1ght, mcendu, ksyx, iamtwz, Xeonacid, kenlig, Menci, Enter-tainer, CCXXXI, hcx2012Git

Trước khi đọc nội dung dưới đây, bạn cần nắm vững phần [Các khái niệm liên quan đến lý thuyết đồ thị](./concept.md).

Đọc thêm: [Điểm cắt và cầu](./cut.md).

## Giới thiệu

Như chúng ta đã biết, cây (hoặc rừng) có nhiều tính chất tốt và dễ dàng được quản lý bằng các cấu trúc dữ liệu phổ biến.

Tuy nhiên, đồ thị tổng quát lại không có những tính chất thuận lợi như vậy. May mắn thay, đôi khi chúng ta có thể chuyển đổi một số bài toán trên đồ thị tổng quát sang cây để xử lý.

Cây tròn vuông (Block forest hoặc Round-square tree) [^ref1] là một phương pháp chuyển đổi đồ thị thành cây. Bài viết này sẽ giới thiệu cách xây dựng, các tính chất và ứng dụng của cây tròn vuông.

Do giới hạn về độ dài, một số kết luận trong bài chưa được chứng minh chi tiết, bạn đọc có thể tự tìm hiểu hoặc chứng minh thêm.

## Định nghĩa

Cây tròn vuông ban đầu được sử dụng để xử lý "đồ thị xương rồng" (cactus graph - đồ thị vô hướng mà mỗi cạnh thuộc không quá một chu trình đơn). Tuy nhiên, khi khai thác thêm các tính chất, đôi khi chúng ta có thể áp dụng nó cho cả đồ thị vô hướng tổng quát.

Để giới thiệu cây tròn vuông, trước tiên cần nói về **thành phần hai điểm liên thông** (biconnected component).

Một **đồ thị hai điểm liên thông** được định nghĩa là: giữa bất kỳ hai đỉnh khác nhau đều tồn tại ít nhất hai đường đi đơn giản (không lặp lại đỉnh) mà không có đỉnh chung (ngoại trừ điểm đầu và điểm cuối).

Có thể thấy, với đồ thị chỉ có một đỉnh thì khó xác định nó có phải là hai điểm liên thông hay không, ở đây ta không xét trường hợp chỉ có một đỉnh.

Một định nghĩa gần tương đương là: đồ thị không có điểm cắt.  
Định nghĩa này chỉ không đúng với đồ thị có hai đỉnh và một cạnh nối giữa chúng. Đồ thị này không có điểm cắt, nhưng không thể tìm được hai đường đi không giao nhau giữa hai đỉnh (vì chỉ có một đường đi).  
(Cũng có thể hiểu là đường đi đó được tính hai lần, thực tế không có giao điểm vì không qua đỉnh khác.)

Mặc dù định nghĩa gốc là định nghĩa đầu tiên, nhưng để thuận tiện, ta quy ước định nghĩa đồ thị hai điểm liên thông là định nghĩa thứ hai.

**Thành phần hai điểm liên thông** của một đồ thị là **tiểu đồ thị hai điểm liên thông lớn nhất**.  
Khác với thành phần liên thông mạnh, một đỉnh có thể thuộc nhiều thành phần hai điểm liên thông, nhưng một cạnh chỉ thuộc đúng một thành phần hai điểm liên thông (nếu dùng định nghĩa đầu thì có thể không thuộc thành phần nào).

Trong cây tròn vuông, mỗi đỉnh gốc của đồ thị tương ứng với một **điểm tròn**, mỗi thành phần hai điểm liên thông tương ứng với một **điểm vuông**.  
Vậy tổng cộng có $n+c$ đỉnh, với $n$ là số đỉnh của đồ thị gốc, $c$ là số thành phần hai điểm liên thông.

Với mỗi thành phần hai điểm liên thông, điểm vuông tương ứng sẽ nối với tất cả các đỉnh thuộc thành phần đó.  
Mỗi thành phần hai điểm liên thông tạo thành một "đồ thị hoa cúc", nhiều "hoa cúc" nối với nhau qua các điểm cắt (vì điểm cắt là điểm phân tách các thành phần hai điểm liên thông).

Rõ ràng, mỗi cạnh trong cây tròn vuông nối một điểm tròn và một điểm vuông.

Hình dưới minh họa các thành phần hai điểm liên thông và hình dạng cây tròn vuông tương ứng [^ref2]:

![](./images/block-forest1.svg)![](./images/block-forest2.svg)![](./images/block-forest3.svg)

Số đỉnh của cây tròn vuông nhỏ hơn $2n$, vì số điểm cắt nhỏ hơn $n$, nên khi khai báo mảng cần chú ý cấp phát gấp đôi.

Thực ra, nếu đồ thị gốc liên thông thì "cây tròn vuông" là một cây, nếu đồ thị có $k$ thành phần liên thông thì cây tròn vuông sẽ là $k$ cây tạo thành một rừng.

Nếu một thành phần liên thông chỉ có một đỉnh, cần xét riêng, trong bài này không xét trường hợp đỉnh cô lập.

## Quy trình xây dựng

Với một đồ thị, làm sao để xây dựng cây tròn vuông? Nếu đồ thị không liên thông, ta tách ra xử lý từng thành phần liên thông, nên chỉ cần xét đồ thị liên thông.

Cây tròn vuông dựa trên thành phần hai điểm liên thông, mà thành phần này lại dựa trên điểm cắt, nên chỉ cần dùng thuật toán tìm điểm cắt là được.

Thuật toán tìm điểm cắt phổ biến là Tarjan. Nếu bạn đã biết thì phần dưới sẽ rất dễ hiểu, nếu chưa biết cũng không sao.

Ta bỏ qua phần tìm điểm cắt, trực tiếp giới thiệu thuật toán xây dựng cây tròn vuông (thực chất là biến thể của Tarjan):

Tiến hành DFS trên đồ thị, sử dụng hai mảng quan trọng là `dfn` và `low` (giống Tarjan).

`dfn[u]` lưu thứ tự DFS của đỉnh $u$, tức là thứ tự khi lần đầu tiên truy cập vào $u$.  
`low[u]` lưu giá trị nhỏ nhất của DFS thứ tự mà đỉnh $u$ hoặc các đỉnh trong cây con của $u$ có thể truy cập tới thông qua **tối đa một cạnh ngược hoặc cạnh về cha**.  
Nếu chưa biết thuật toán Tarjan, hãy xem ví dụ sau:

![](./images/block-forest4.svg)

(Thực ra đồ thị này giống với hình ở trên.)  
Cạnh cây vẽ bằng đường thẳng từ trên xuống, cạnh ngược vẽ bằng đường cong từ dưới lên. Số thứ tự của đỉnh chính là thứ tự DFS.

Mảng `low` như sau:

|        $i$        | $1$ | $2$ | $3$ | $4$ | $5$ | $6$ | $7$ | $8$ | $9$ |
| :---------------: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| $\mathrm{low}[i]$ | $1$ | $1$ | $1$ | $3$ | $3$ | $4$ | $3$ | $3$ | $7$ |

Không quá khó hiểu, chú ý ở đây `low[9] = 7`, khác với một số cách tìm điểm cắt, vì để thuận tiện, ta cho phép đi lên qua cạnh về cha, nhưng ý tưởng chính là giống nhau.

Ta có thể dễ dàng viết hàm DFS tính `dfn` và `low` (ban đầu mảng `dfn` gán bằng 0):

???+ note "Cài đặt"
    === "C++"
        ```cpp
        void Tarjan(int u) {
          low[u] = dfn[u] = ++dfc;                // low khởi tạo bằng dfn của đỉnh hiện tại
          for (int v : G[u]) {                    // Duyệt các đỉnh kề với u
            if (!dfn[v]) {                        // Nếu chưa truy cập
              Tarjan(v);                          // Đệ quy
              low[u] = std::min(low[u], low[v]);  // Lấy min giữa low của u và low của v chưa truy cập
            } else
              low[u] = std::min(low[u], dfn[v]);  // Nếu đã truy cập thì lấy min với dfn
          }
        }
        ```
    
    === "Python"
        ```python
        def Tarjan(u):
            low[u] = dfn[u] = dfc  # low khởi tạo bằng dfn của đỉnh hiện tại
            dfc = dfc + 1
            for v in G[u]:  # Duyệt các đỉnh kề với u
                if dfn[v] == False:  # Nếu chưa truy cập
                    Tarjan(v)  # Đệ quy
                    low[u] = min(low[u], low[v])  # Lấy min giữa low của u và low của v chưa truy cập
                else:
                    low[u] = min(low[u], dfn[v])  # Nếu đã truy cập thì lấy min với dfn
        ```

Tiếp theo, ta xét mối liên hệ giữa thành phần hai điểm liên thông, cây DFS và hai mảng trên.

Có thể thấy, mỗi thành phần hai điểm liên thông trên cây DFS là một cây con liên thông, và chứa ít nhất hai đỉnh; đặc biệt, đỉnh ở trên cùng chỉ nối xuống một đỉnh.

Đồng thời, mỗi cạnh cây thuộc đúng một thành phần hai điểm liên thông.

Xét một thành phần hai điểm liên thông trên cây DFS, đỉnh trên cùng là $u$, tại $u$ xác định thành phần này vì cây con của $u$ chứa toàn bộ thông tin.

Vì có ít nhất hai đỉnh, xét đỉnh tiếp theo $v$, thì giữa $u$ và $v$ có một cạnh cây.

Khi đó, chắc chắn $\mathrm{low}[v]=\mathrm{dfn}[u]$.  
Chính xác hơn, với cạnh cây $u\to v$, $u,v$ cùng thuộc một thành phần hai điểm liên thông, và $u$ là đỉnh sâu nhất trong thành phần này **khi và chỉ khi** $\mathrm{low}[v]=\mathrm{dfn}[u]$.

Như vậy, ta có thể xác định vị trí xuất hiện thành phần hai điểm liên thông trong quá trình DFS, nhưng chưa xác định được tập đỉnh của thành phần đó.

Việc này không khó, ta duy trì một ngăn xếp trong quá trình DFS, lưu các đỉnh chưa xác định thuộc thành phần nào (có thể thuộc nhiều thành phần).

Khi tìm thấy thành phần hai điểm liên thông, các đỉnh ngoài $u$ đều nằm ở đầu ngăn xếp, chỉ cần pop liên tục đến khi gặp $v$.

Đồng thời, ta xử lý các đỉnh vừa pop ra, chỉ cần nối chúng với điểm vuông mới tạo trong cây tròn vuông. Cuối cùng, nhớ nối $u$ với điểm vuông (nhưng không pop $u$).

Như vậy, ta đã xây dựng cây tròn vuông một cách tự nhiên, có thể đánh số điểm vuông từ $n+1$ trở đi để phân biệt với điểm tròn.

Phần này có thể hơi khó hiểu, dưới đây là đoạn code có chú thích chi tiết và các câu lệnh xuất ra giúp kiểm tra, kèm một ví dụ mẫu. Bạn nên copy code và thực hành để hiểu rõ hơn (đừng quên bật `c++11`).

???+ note "Cài đặt"
    ```cpp
    #include <algorithm>
    #include <cstdio>
    #include <vector>
    
    constexpr int MN = 100005;
    
    int N, M, cnt;
    std::vector<int> G[MN], T[MN * 2];
    
    int dfn[MN], low[MN], dfc;
    int stk[MN], tp;
    
    void Tarjan(int u) {
      printf("  Enter : #%d\n", u);
      low[u] = dfn[u] = ++dfc;                // low khởi tạo bằng dfn của đỉnh hiện tại
      stk[++tp] = u;                          // Đưa vào ngăn xếp
      for (int v : G[u]) {                    // Duyệt các đỉnh kề với u
        if (!dfn[v]) {                        // Nếu chưa truy cập
          Tarjan(v);                          // Đệ quy
          low[u] = std::min(low[u], low[v]);  // Lấy min giữa low của u và low của v chưa truy cập
          if (low[v] == dfn[u]) {  // Đánh dấu tìm thấy một thành phần hai điểm liên thông gốc tại u
            ++cnt;                 // Tăng số lượng điểm vuông
            printf("  Found a New BCC #%d.\n", cnt - N);
            // Pop các đỉnh ngoài u thuộc thành phần này và nối trong cây tròn vuông
            for (int x = 0; x != v; --tp) {
              x = stk[tp];
              T[cnt].push_back(x);
              T[x].push_back(cnt);
              printf("    BCC #%d has vertex #%d\n", cnt - N, x);
            }
            // Chú ý nối cả u với điểm vuông (nhưng không pop u)
            T[cnt].push_back(u);
            T[u].push_back(cnt);
            printf("    BCC #%d has vertex #%d\n", cnt - N, u);
          }
        } else
          low[u] = std::min(low[u], dfn[v]);  // Nếu đã truy cập thì lấy min với dfn
      }
      printf("  Exit : #%d : low = %d\n", u, low[u]);
      printf("  Stack:\n    ");
      for (int i = 1; i <= tp; ++i) printf("%d, ", stk[i]);
      puts("");
    }
    
    int main() {
      scanf("%d%d", &N, &M);
      cnt = N;  // Đánh số điểm vuông từ N trở đi
      for (int i = 1; i <= M; ++i) {
        int u, v;
        scanf("%d%d", &u, &v);
        G[u].push_back(v);  // Thêm cạnh hai chiều
        G[v].push_back(u);
      }
      // Xử lý đồ thị không liên thông
      for (int u = 1; u <= N; ++u)
        if (!dfn[u]) Tarjan(u), --tp;
      // Khi thoát Tarjan, ngăn xếp còn một phần tử là gốc, pop ra
      return 0;
    }
    ```

Ví dụ kiểm thử:

```text
13 15
1 2
2 3
1 3
3 4
3 5
4 5
5 6
4 6
3 7
3 8
7 8
7 9
10 11
11 10
11 12
```

Đồ thị tương ứng (có cả cạnh lặp và đỉnh cô lập):

![](./images/block-forest5.svg)

## Bài mẫu

Dưới đây là một số bài toán có thể giải bằng cây tròn vuông.

???+ note "[「APIO2018」Hai môn phối hợp](https://loj.ac/p/2587)"
    ??? note "Tóm tắt đề bài"
        Cho một đồ thị vô hướng đơn giản, hỏi có bao nhiêu bộ ba $\langle s, c, f \rangle$ ($s, c, f$ khác nhau) sao cho tồn tại một đường đi đơn giản từ $s$ qua $c$ đến $f$.
    
    ??? note "Phân tích"
        Nhắc đến đường đi đơn giản, cần nói đến một tính chất rất hay của thành phần hai điểm liên thông: với hai đỉnh bất kỳ trong một thành phần hai điểm liên thông, hợp các đường đi đơn giản giữa chúng chính là toàn bộ thành phần đó.  
        Tức là, với ba đỉnh khác nhau $u,v,c$ cùng thuộc một thành phần hai điểm liên thông, luôn tồn tại một đường đi đơn giản từ $u$ đến $v$ qua $c$.

        Chứng minh tính chất này:

        -   Rõ ràng nếu đường đi ra khỏi thành phần hai điểm liên thông thì không thể quay lại, nếu không sẽ mâu thuẫn với định nghĩa.
        -   Vậy chỉ cần chứng minh với ba đỉnh khác nhau $u,v,c$ trong một thành phần hai điểm liên thông, luôn tồn tại đường đi đơn giản từ $u$ đến $v$ qua $c$.
        -   Loại trừ trường hợp chỉ có 2 đỉnh, vì không thể chọn 3 đỉnh khác nhau.
        -   Với các trường hợp còn lại, xây dựng mô hình mạng lưới: nguồn nối $c$ với cạnh dung lượng $2$, $u$ và $v$ nối đích với cạnh dung lượng $1$.
        -   Các cạnh trong đồ thị gốc chuyển thành cạnh hai chiều dung lượng $1$.
        -   Các đỉnh (trừ nguồn, đích, $c$) gán dung lượng $1$ (có thể tách đỉnh).
        -   Nếu mạng có max-flow bằng $2$, tức là luôn có đường đi qua $c$.
        -   Theo định lý max-flow min-cut, min-cut không quá $2$, chỉ cần chứng minh min-cut lớn hơn $1$.
        -   Tức là, loại bỏ bất kỳ cạnh dung lượng $1$ nào cũng không thể ngắt kết nối nguồn và đích.
        -   Nếu loại bỏ cạnh nối $u$ hoặc $v$ với đích, theo định nghĩa đầu, luôn có đường đi đơn giản từ $c$ đến đỉnh còn lại.
        -   Nếu loại bỏ cạnh tách đỉnh, tức là xóa một đỉnh, theo định nghĩa hai, đồ thị còn lại vẫn liên thông.
        -   Nếu loại bỏ cạnh từ đồ thị gốc, tức là xóa một cạnh, rõ ràng vẫn tồn tại đường đi.
        -   Vậy min-cut lớn hơn $1$, tức là max-flow bằng $2$. Đã chứng minh.

        Kết luận này cho ta điều gì? Nó cho biết: xét đường đi giữa hai điểm tròn trên cây tròn vuông, tập các điểm tròn kề với các điểm vuông trên đường đi đó chính là tập các đỉnh trên đường đi đơn giản giữa hai đỉnh gốc.

        Quay lại bài toán, cố định $s$ và $f$, số lượng $c$ hợp lệ chính là số đỉnh trên hợp các đường đi đơn giản giữa $s$ và $f$ trừ đi $2$ (loại $s,f$).

        Vậy, sau khi xây dựng cây tròn vuông, số đỉnh trên đường đi đơn giản giữa hai đỉnh chính là tổng các điểm tròn và điểm vuông trên đường đi giữa hai điểm tròn tương ứng.

        Tiếp theo là một kỹ thuật thường dùng với cây tròn vuông: khi thống kê đường đi, gán trọng số phù hợp cho các đỉnh.  
        Với bài này, mỗi điểm vuông có trọng số bằng kích thước thành phần hai điểm liên thông, mỗi điểm tròn trọng số $-1$.

        Khi đó, tổng trọng số trên đường đi giữa hai điểm tròn chính là số đỉnh trên hợp các đường đi đơn giản trừ đi $2$.

        Bài toán chuyển thành thống kê tổng trọng số trên đường đi giữa các cặp điểm tròn trên cây tròn vuông.

        Đổi góc nhìn, tính đóng góp của từng đỉnh cho đáp số, tức là trọng số nhân với số đường đi qua nó, có thể dùng quy hoạch động trên cây để tính.

        Cuối cùng, đừng quên xử lý trường hợp đồ thị không liên thông. Dưới đây là code tham khảo:

    ??? note "Code tham khảo"
        ```cpp
        --8<-- "docs/graph/code/block-forest/block-forest_1.cpp"
        ```
    
    Với ví dụ kiểm thử ở trên, đáp số bài này là $212$.

???+ note "[Codeforces #487 E. Tourists](https://codeforces.com/contest/487/problem/E)"
    ??? note "Tóm tắt đề bài"
        Cho một đồ thị vô hướng liên thông đơn giản, yêu cầu hỗ trợ hai thao tác:
        
        1.  Thay đổi trọng số của một đỉnh.
        
        2.  Truy vấn giá trị nhỏ nhất của trọng số các đỉnh trên tất cả các đường đi đơn giản giữa hai đỉnh.

    ??? note "Phân tích"
        Tương tự, xây dựng cây tròn vuông, gán trọng số cho điểm vuông bằng giá trị nhỏ nhất trong các điểm tròn kề, bài toán chuyển thành truy vấn giá trị nhỏ nhất trên đường đi.

        Truy vấn giá trị nhỏ nhất có thể dùng phân đoạn cây và cây phân đoạn để xử lý, nhưng còn thao tác thay đổi thì sao?

        Khi thay đổi trọng số một điểm tròn, cần cập nhật tất cả các điểm vuông kề, có thể lên tới $O(n)$ lần cập nhật.

        Lúc này, tận dụng tính chất cây của cây tròn vuông, gán trọng số điểm vuông bằng giá trị nhỏ nhất trong các con là điểm tròn, khi thay đổi chỉ cần cập nhật điểm vuông cha.

        Với điểm vuông, chỉ cần dùng multiset để quản lý tập trọng số các con.

        Lưu ý, khi truy vấn nếu LCA là điểm vuông, cần kiểm tra thêm trọng số điểm tròn cha của LCA.

        Chú ý: số đỉnh của cây tròn vuông cần cấp phát gấp đôi so với đồ thị gốc để tránh tràn mảng.

    ??? note "Code tham khảo"
        ```cpp
        --8<-- "docs/graph/code/block-forest/block-forest_2.cpp"
        ```

???+ note "[「SDOI2018」Trò chơi chiến lược](https://loj.ac/p/2562)"
    ??? note "Tóm tắt đề bài"
        Cho một đồ thị vô hướng liên thông đơn giản. Có $q$ truy vấn:
        
        Mỗi truy vấn cho một tập đỉnh $S$ ($2 \le |S| \le n$), hỏi có bao nhiêu đỉnh $u$ thỏa mãn $u \notin S$ và khi xóa $u$ thì các đỉnh trong $S$ không còn nằm trong cùng một thành phần liên thông.

        Mỗi test có nhiều truy vấn.

    ??? note "Phân tích"
        Đầu tiên xây dựng cây tròn vuông, bài toán chuyển thành truy vấn số lượng điểm tròn trong thành phần liên thông chứa $S$ trên cây tròn vuông trừ đi $|S|$.

        Làm sao tính số lượng điểm tròn trong thành phần liên thông chứa $S$? Có một cách:

        Gán trọng số cho điểm tròn vào cạnh nối nó với điểm vuông cha, bài toán chuyển thành tính tổng trọng số các cạnh, có thể tham khảo bài [「SDOI2015」Trò chơi tìm kho báu](https://loj.ac/p/2182).  
        Tức là, sắp xếp các đỉnh trong $S$ theo thứ tự DFS, tính tổng khoảng cách giữa các cặp liên tiếp (kể cả cặp đầu-cuối), đáp số là tổng khoảng cách chia đôi vì mỗi cạnh đi qua hai lần.

        Cuối cùng, nếu đỉnh sâu nhất trong thành phần là điểm tròn, đáp số cần cộng thêm $1$ vì chưa tính đến nó.

        Vì có nhiều truy vấn, cần chú ý khởi tạo lại mảng.

    ??? note "Code tham khảo"
        ```cpp
        --8<-- "docs/graph/code/block-forest/block-forest_3.cpp"
        ```

## Bài tập

-   [UVa 1464 Traffic Real Time Query](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=447&page=show_problem&problem=4210)
-   [LuoGu P4320 Gặp nhau trên đường](https://www.luogu.com.cn/problem/P4320)
-   [LuoGu P10517 Quy hoạch lãnh thổ](https://www.luogu.com.cn/problem/P10517)

## Liên kết ngoài

immortalCO, [Cây tròn vuông - công cụ xử lý đồ thị xương rồng](https://immortalco.blog.uoj.ac/blog/1955), Universal OJ.

## Tài liệu tham khảo & chú thích

[^ref1]: Năm 2017, bạn Chen Junkun đã định nghĩa và đặt tên cấu trúc cây tròn vuông trong luận văn đội tuyển quốc gia IOI Trung Quốc "Báo cáo đề xuất về đồ thị thần kỳ và mở rộng" (〈神奇的子图〉命题报告及其拓展).

[^ref2]: Chen Junkun, "Cây tròn vuông bình thường và quy hoạch động (~~động~~) thần kỳ", trại đông NOI2018, trang 4.
