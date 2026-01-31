author: Ir1d, ShadowsEpic, Fomalhauthmj, siger-young, MingqiHuang, Xeonacid, hsfzLZH1, orzAtalod, NachtgeistW

Trang này sẽ giới thiệu ngắn gọn về phương pháp **bội số hóa** (Binary Lifting).

## Định nghĩa

**Bội số hóa** (tiếng Anh: binary lifting) là một kỹ thuật "tăng theo bội số". Khi thực hiện truy hồi (hoặc truy vết trạng thái), nếu không gian trạng thái quá lớn, việc truy hồi tuyến tính thông thường sẽ không đáp ứng được yêu cầu về độ phức tạp thời gian và bộ nhớ. Khi đó, ta có thể chỉ truy hồi tại các vị trí là lũy thừa nguyên của $k$ (thường là $k=2$), tức là chỉ lưu trạng thái ở các mốc $k^0, k^1, k^2, \ldots$. Khi cần truy vấn giá trị ở vị trí bất kỳ, ta tận dụng tính chất "mọi số nguyên đều có thể biểu diễn thành tổng các lũy thừa của $k$" để ghép các trạng thái đã lưu lại. Do đó, bài toán cần có tính chất chia nhỏ được theo lũy thừa của $k$. Thông thường, $k=2$ là phổ biến nhất.[^ref1]

Phương pháp này được ứng dụng rộng rãi trong nhiều bài toán, phổ biến nhất là bài toán RMQ và tìm [LCA (tổ tiên chung gần nhất)](../graph/lca.md).

## Ứng dụng

### Bài toán RMQ

Xem thêm: [Chuyên đề RMQ](../topic/rmq.md)

RMQ là viết tắt của Range Maximum/Minimum Query, tức là truy vấn giá trị lớn nhất (nhỏ nhất) trên một đoạn. Phương pháp giải RMQ bằng ý tưởng bội số hóa chính là [Sparse Table (ST bảng)](../ds/sparse-table.md).

### Tìm LCA trên cây bằng bội số hóa

Xem thêm: [Tổ tiên chung gần nhất](../graph/lca.md)

## Bài tập ví dụ

### Bài 1

???+ note "Bài tập"
    Làm thế nào để dùng ít quả cân nhất để cân được tất cả các trọng lượng trong đoạn $[0,31]$? (Chỉ được đặt quả cân ở một bên cân)

??? note "Hướng dẫn giải"
    Đáp án là dùng các quả cân có trọng lượng 1, 2, 4, 8, 16, tổng cộng 5 quả cân, có thể cân được mọi trọng lượng từ $[0,31]$. Tương tự, nếu muốn cân các trọng lượng từ $[0,127]$, chỉ cần 7 quả cân: 1, 2, 4, 8, 16, 32, 64. Mỗi lần ta chọn trọng lượng là lũy thừa của 2, như vậy chỉ cần rất ít quả cân để cân được mọi trọng lượng cần thiết.

    Tại sao lại là "rất ít"? Vì nếu muốn cân các trọng lượng từ $[0,1023]$, chỉ cần 10 quả cân; từ $[0,1048575]$ chỉ cần 20 quả cân. Nếu phạm vi trọng lượng cần cân tăng gấp đôi, số quả cân chỉ tăng thêm 1. Đây gọi là tốc độ tăng "logarit", vì số quả cân tỉ lệ với logarit của phạm vi trọng lượng cần cân.

### Bài 2

???+ note "Bài tập"
    Cho một vòng tròn gồm $n$ điểm và một hằng số $k$. Mỗi lần nhảy từ điểm $i$ sang điểm $((i+k)\bmod n)+1$. Tổng cộng thực hiện $m$ lần nhảy. Mỗi điểm có trọng số $a_i$. Hãy tính tổng trọng số của các điểm xuất phát trong $m$ lần nhảy, lấy kết quả theo modulo $10^9+7$.

    Giới hạn: $1\leq n\leq 10^6$, $1\leq m\leq 10^{18}$, $1\leq k\leq n$, $0\le a_i\le 10^9$.

??? note "Hướng dẫn giải"
    Rõ ràng không thể mô phỏng từng bước nhảy vì $m$ có thể lên tới $10^{18}$, quá lớn để duyệt trâu.

    Do đó, cần tiền xử lý, tổng hợp trước một số thông tin để khi truy vấn có thể trả lời nhanh hơn. Nếu lưu kết quả cho mọi số bước nhảy có thể thì cả thời gian lẫn bộ nhớ đều không chịu nổi.

    Vậy nên tiền xử lý như thế nào? Hãy xem lại bài tập 1. Bạn đã có ý tưởng chưa?

    Quay lại bài này. Ta sẽ tiền xử lý một số thông tin, sau đó dùng các thông tin này để ghép thành đáp án. Thông tin tiền xử lý cũng không nên quá nhiều. Cách làm là tiền xử lý các kết quả nhảy $2^i$ bước từ mỗi điểm (vị trí đến và tổng trọng số), như vậy chỉ cần xử lý một lượng nhỏ thông tin, và khi ghép đáp án cũng rất nhanh.

    Cụ thể, với mỗi điểm, lưu `go[i][x]` là điểm đến sau khi nhảy $2^i$ bước từ điểm $x$, và `sum[i][x]` là tổng trọng số thu được sau khi nhảy $2^i$ bước từ $x$. Khi tiền xử lý, dùng hai vòng lặp: với $2^i$ bước, coi như nhảy hai lần $2^{i-1}$ bước, tức là `sum[i][x] = sum[i-1][x]+sum[i-1][go[i-1][x]]`, và `go[i][x] = go[i-1][go[i-1][x]]`.

    Cần chú ý một số chi tiết khi cài đặt. Để tránh tính trùng hoặc thiếu, thường sẽ tiền xử lý tổng trọng số theo kiểu "nửa mở" (tức là chỉ tính các điểm bắt đầu, không tính điểm kết thúc). Như vậy, khi cộng tổng, chỉ cần cộng trực tiếp hai đoạn mà không lo bị trùng.

    Bài này $m\leq 10^{18}$, nhìn thì lớn nhưng thực ra chỉ cần tiền xử lý tới $i=65$ là đủ, nhanh hơn rất nhiều so với duyệt trâu. Độ phức tạp thời gian là tiền xử lý $\Theta(n\log m)$, mỗi truy vấn $\Theta(\log m)$.

??? note "Code tham khảo"
    ```cpp
    #include <cstdio>
    using namespace std;
    
    constexpr int mod = 1000000007;
    
    int modadd(int a, int b) {
      if (a + b >= mod) return a + b - mod;  // Dùng phép trừ thay cho chia lấy dư để tăng tốc
      return a + b;
    }
    
    int vi[1000005];
    
    int go[75][1000005];  // Mở rộng mảng một chút để tránh truy cập ngoài, ưu tiên khai báo chiều nhỏ trước
    int sum[75][1000005];
    
    int main() {
      int n, k;
      scanf("%d%d", &n, &k);
      for (int i = 1; i <= n; ++i) {
        scanf("%d", vi + i);
      }
    
      for (int i = 1; i <= n; ++i) {
        go[0][i] = (i + k) % n + 1;
        sum[0][i] = vi[i];
      }
    
      int logn = 31 - __builtin_clz(n);  // Cách nhanh để lấy logarit cơ số 2 của n
      for (int i = 1; i <= logn; ++i) {
        for (int j = 1; j <= n; ++j) {
          go[i][j] = go[i - 1][go[i - 1][j]];
          sum[i][j] = modadd(sum[i - 1][j], sum[i - 1][go[i - 1][j]]);
        }
      }
    
      long long m;
      scanf("%lld", &m);
    
      int ans = 0;
      int curx = 1;
      for (int i = 0; m; ++i) {
        if (m & (1ll << i)) {  // Xem lại phần toán tử bit, kiểm tra bit thứ i của m có bằng 1 không
          ans = modadd(ans, sum[i][curx]);
          curx = go[i][curx];
          m ^= 1ll << i;  // Đặt bit thứ i về 0
        }
      }
    
      printf("%d\n", ans);
    }
    ```

[^ref1]: Trích từ "Hướng dẫn giải thuật thi đấu" của Lý Dục Đông, mục 0x06. Bội số hóa
