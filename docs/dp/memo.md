## Định nghĩa

Tìm kiếm ghi nhớ (Memoization Search) là một cách hiện thực tìm kiếm bằng cách ghi lại thông tin của các trạng thái đã duyệt qua, từ đó tránh việc duyệt lặp lại cùng một trạng thái.

Vì tìm kiếm ghi nhớ đảm bảo mỗi trạng thái chỉ được truy cập một lần, nó cũng là một cách hiện thực quy hoạch động rất phổ biến.

## Giới thiệu

???+ note "[\[NOIP2005\] Hái thuốc](https://www.luogu.com.cn/problem/P1048)"
    Trong một hang động có $M$ loại thảo dược khác nhau, việc hái mỗi loại cần một thời gian $t_i$, và mỗi loại có giá trị riêng $v_i$. Cho bạn một khoảng thời gian $T$, trong thời gian này bạn có thể hái một số loại thảo dược. Hãy làm cho tổng giá trị thảo dược hái được là lớn nhất.
    
    $1 \leq T \leq 10^3$, $1 \leq t_i, v_i, M \leq 100$

### Cách làm [DFS](../search/dfs.md) thuần túy

Rất dễ để hiện thực một cách tìm kiếm thuần túy: trong khi tìm kiếm, ghi lại ba tham số: đang chuẩn bị chọn vật phẩm thứ mấy, thời gian còn lại là bao nhiêu, và giá trị đã đạt được là bao nhiêu. Sau đó duyệt xem vật phẩm hiện tại có được chọn hay không để chuyển sang trạng thái tương ứng.

???+ note "Hiện thực"
    === "C++"
        ```cpp
        int n, t;
        int tcost[103], mget[103];
        int ans = 0;
        
        void dfs(int pos, int tleft, int tans) {
          if (tleft < 0) return;
          if (pos == n + 1) {
            ans = max(ans, tans);
            return;
          }
          dfs(pos + 1, tleft, tans);
          dfs(pos + 1, tleft - tcost[pos], tans + mget[pos]);
        }
        
        int main() {
          cin >> t >> n;
          for (int i = 1; i <= n; i++) cin >> tcost[i] >> mget[i];
          dfs(1, t, 0);
          cout << ans << endl;
          return 0;
        }
        ```
    
    === "Python"
        ```python
        tcost = [0] * 103
        mget = [0] * 103
        ans = 0
        
        def dfs(pos, tleft, tans):
            global ans
            if tleft < 0:
                return
            if pos == n + 1:
                ans = max(ans, tans)
                return
            dfs(pos + 1, tleft, tans)
            dfs(pos + 1, tleft - tcost[pos], tans + mget[pos])
        
        t, n = map(lambda x: int(x), input().split())
        for i in range(1, n + 1):
            tcost[i], mget[i] = map(lambda x: int(x), input().split())
        dfs(1, t, 0)
        print(ans)
        ```

Độ phức tạp thời gian của cách làm này là hàm mũ, không thể vượt qua bài này.

### Tối ưu hóa

Tại sao cách làm trên lại hiệu suất thấp? Bởi vì cùng một trạng thái bị truy cập nhiều lần.

Nếu sau mỗi lần truy vấn xong một trạng thái, chúng ta lưu trữ thông tin của trạng thái đó lại, lần sau cần truy cập trạng thái này có thể sử dụng trực tiếp thông tin đã tính toán trước đó, từ đó tránh tính toán lặp lại. Điều này tận dụng đặc điểm của quy hoạch động là có nhiều bài toán con chồng lấn, thuộc về tư tưởng "ghi nhớ" dùng không gian đổi thời gian.

Cụ thể trong bài này, trên cơ sở DFS thuần túy, chúng ta thêm một mảng `mem` để ghi lại giá trị trả về của mỗi `dfs(pos, tleft)`. Ban đầu đặt mọi giá trị trong `mem` thành `-1` (đại diện cho chưa giải). Mỗi khi cần truy cập một trạng thái, nếu giá trị tương ứng trong `mem` là `-1`, ta sẽ truy cập đệ quy trạng thái đó. Ngược lại, chúng ta sử dụng trực tiếp giá trị đã lưu trong `mem`.

Thông qua xử lý này, chúng ta đảm bảo mỗi trạng thái chỉ được truy cập tối đa một lần, do đó độ phức tạp thời gian của thuật toán là $O(TM)$.

???+ note "Hiện thực"
    === "C++"
        ```cpp
        int n, t;
        int tcost[103], mget[103];
        int mem[103][1003];
        
        int dfs(int pos, int tleft) {
          if (mem[pos][tleft] != -1)
            return mem[pos][tleft];  // Trạng thái đã truy cập, trả về giá trị đã ghi
          if (pos == n + 1) return mem[pos][tleft] = 0;
          int dfs1, dfs2 = -INF;
          dfs1 = dfs(pos + 1, tleft);
          if (tleft >= tcost[pos])
            dfs2 = dfs(pos + 1, tleft - tcost[pos]) + mget[pos];  // Chuyển trạng thái
          return mem[pos][tleft] = max(dfs1, dfs2);  // Lưu lại giá trị trạng thái hiện tại
        }
        
        int main() {
          memset(mem, -1, sizeof(mem));
          cin >> t >> n;
          for (int i = 1; i <= n; i++) cin >> tcost[i] >> mget[i];
          cout << dfs(1, t) << endl;
          return 0;
        }
        ```

## Liên hệ và khác biệt với cách tiếp cận khử đệ quy (truy hồi)

Khi giải quyết các bài toán quy hoạch động, mã nguồn của tìm kiếm ghi nhớ và cách tiếp cận khử đệ quy (vòng lặp) có hình thức rất giống nhau. Điều này là do chúng sử dụng cùng một cách biểu diễn trạng thái và các bước chuyển trạng thái tương tự. Cũng chính vì thế, thông thường độ phức tạp thời gian của hai cách hiện thực là như nhau.

Trong việc giải quyết các bài toán quy hoạch động, cả tìm kiếm ghi nhớ và khử đệ quy đều đảm bảo rằng cùng một trạng thái chỉ được giải tối đa một lần. Cách chúng đạt được điều này hơi khác nhau: khử đệ quy tránh truy cập lặp lại bằng cách thiết lập một thứ tự truy cập rõ ràng, trong khi tìm kiếm ghi nhớ mặc dù không quy định rõ thứ tự truy cập, nhưng thông qua việc đánh dấu các trạng thái đã truy cập cũng đạt được mục đích tương tự.

So với khử đệ quy, tìm kiếm ghi nhớ vì không cần quy định rõ thứ tự truy cập nên đôi khi có độ khó hiện thực thấp hơn, và có thể xử lý các trường hợp biên một cách tiện lợi, đây là một ưu điểm lớn. Tuy nhiên, tìm kiếm ghi nhớ khó sử dụng các tối ưu hóa như mảng cuốn (rolling array), và do có đệ quy nên hiệu suất vận hành sẽ thấp hơn khử đệ quy. Do đó nên căn cứ vào đề bài để chọn cách hiện thực phù hợp nhất.

## Cách viết tìm kiếm ghi nhớ

### Phương pháp 1

1. Viết trạng thái và phương trình DP của bài toán.
2. Viết hàm DFS dựa trên chúng.
3. Thêm mảng ghi nhớ.

### Phương pháp 2

1. Viết chương trình vét cạn cho bài toán (tốt nhất là [DFS](../search/dfs.md)).
2. Sửa DFS này thành một hàm DFS "không dùng biến bên ngoài".
3. Thêm mảng ghi nhớ.