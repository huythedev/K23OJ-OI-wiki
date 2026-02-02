## Giới thiệu

Cho một số nguyên dương $N \in \mathbf{N}_{+}$, hãy nhanh chóng tìm một [ước không tầm thường](basic.md) của nó．

Xét thuật toán ngây thơ, các ước đi theo cặp, toàn bộ ước của $N$ có thể chia thành hai phần, tức $[2, \sqrt N]$ và $[\sqrt N+1,N)$．Chỉ cần duyệt tất cả các số trong $[2, \sqrt N]$, rồi dựa vào phép chia là có thể tìm ra ít nhất hai ước．Phương pháp này có độ phức tạp thời gian $O(\sqrt N)$．

Khi $N\ge10^{18}$, thời gian chạy của thuật toán này là không thể chấp nhận, cần một thuật toán tốt hơn．Một ý tưởng là dùng phương pháp ngẫu nhiên, đoán xem một số có phải là ước của $N$ hay không, nếu may mắn có thể giải trong thời gian $O(1)$, nhưng với $N\ge10^{18}$, xác suất đoán trúng là $\frac{1}{10^{18}}$, số lần đoán kỳ vọng là $10^{18}$．Nếu đoán trong $[2,\sqrt N]$ thì xác suất lớn hơn một chút．Ta cần cách tối ưu hóa việc đoán．

## Thuật toán ngây thơ

Thuật toán đơn giản nhất là duyệt $[2, \sqrt N]$．

=== "C++"
    ```cpp
    vector<int> breakdown(int N) {
      vector<int> result;
      for (int i = 2; i * i <= N; i++) {
        if (N % i == 0) {  // Nếu i chia hết N, thì i là một thừa số nguyên tố của N.
          while (N % i == 0) N /= i;
          result.push_back(i);
        }
      }
      if (N != 1) {  // Sau các thao tác nếu N còn lại là một số nguyên tố
        result.push_back(N);
      }
      return result;
    }
    ```

=== "Python"
    ```python
    def breakdown(N):
        result = []
        for i in range(2, int(sqrt(N)) + 1):
            if N % i == 0:  # Nếu i chia hết N, thì i là một thừa số nguyên tố của N.
                while N % i == 0:
                    N //= i
                result.append(i)
        if N != 1:  # Sau các thao tác nếu N còn lại là một số nguyên tố
            result.append(N)
        return result
    ```

Ta có thể chứng minh mọi phần tử trong `result` chính là toàn bộ các thừa số nguyên tố của `N`．

??? note "Chứng minh `result` là toàn bộ thừa số nguyên tố của $N$"
    Trước hết xét sự thay đổi của `N`．Khi vòng lặp đến hết `i`, do vừa thực hiện xong phần `while(N % i == 0) N /= i`, nên `i` không còn chia hết `N` nữa．Hơn nữa, mỗi lần chia bỏ một thừa số, đều đảm bảo `N` vẫn chia hết $N$ ban đầu．Hai điều này đảm bảo rằng khi vòng lặp bắt đầu tại `i`, `N` là một ước của $N$ ban đầu, và không bị chia hết bởi bất kỳ số nguyên nào nhỏ hơn `i`．
    
    Tiếp theo chứng minh các phần tử trong `result` đều là ước của $N$．Khi vòng lặp đến `i`, điều kiện để lưu `i` vào `result` là `N % i == 0`, tức `i` chia hết `N`, và đã biết `N` là ước của $N$ ban đầu, nên `i` là ước của $N$．Khi vòng lặp kết thúc, nếu `N` không bằng một, cũng sẽ được đưa vào `result`．Lúc này theo phần trước, nó cũng phải là ước của $N$．
    
    Tiếp theo chứng minh các phần tử trong `result` đều là số nguyên tố．Giả sử tồn tại một hợp số $K$ trong `result`, thì chắc chắn tồn tại `i` không vượt quá $\sqrt K$ sao cho `i` là ước của $K$．Một $K$ như vậy không thể được đưa vào `result` bởi vòng lặp, vì phần đầu đã chỉ ra rằng khi đến $K$ thì `N` không bị chia hết bởi bất kỳ `i` nhỏ hơn $K$．Nó cũng không thể được thêm sau khi vòng lặp kết thúc, vì điều kiện thoát là `i * i > N`, tức đã duyệt qua mọi `i` không vượt $\sqrt K$, và theo trên thì các `i` đó không thể chia hết `N` hiện tại, tức $K$．
    
    Cuối cùng chứng minh mọi thừa số nguyên tố của $N$ đều xuất hiện trong `result`．Giả sử $p$ là một thừa số nguyên tố của $N$ nhưng không nằm trong `result`．Theo thảo luận trên, $p$ không thể là một `i` trong vòng lặp．Gọi `i` là `i` cuối cùng trước khi thoát, khi đó `i` nhỏ hơn $p$, và `N` sau vòng lặp không bị chia hết bởi các `i` trước đó, nên $p$ chia hết `N`．Vì vậy cuối cùng `N` lớn hơn một, theo trước đó nó phải là số nguyên tố, nên `N` chính là $p$, sẽ được thêm vào `result`, mâu thuẫn．

Đáng chú ý, nếu ban đầu có một bảng số nguyên tố, độ phức tạp thời gian sẽ giảm từ $O(\sqrt N)$ xuống $O(\frac {\sqrt{N}} {\ln N})$．Xem [sàng](./sieve.md) để biết thêm về bảng này．

Bài ví dụ：[CF 1445C](https://codeforces.com/problemset/problem/1445/C)

## Thuật toán Pollard Rho

### Giới thiệu

Dùng thuật toán vét cạn để tìm một ước không tầm thường có độ phức tạp $O(p)=O(\sqrt N)$, trong đó $p$ là thừa số nguyên tố nhỏ nhất của $N$．Thuật toán Pollard-Rho là một thuật toán ngẫu nhiên, có thể tìm một ước không tầm thường ( **chú ý**! không nhất thiết là thừa số nguyên tố) với độ phức tạp kỳ vọng $O(\sqrt p)=O(N^{1/4})$．

Ý tưởng cốt lõi: với một tự ánh xạ ngẫu nhiên $f: \mathbb Z_p \rightarrow \mathbb Z_p$, bắt đầu từ bất kỳ điểm $x_1$, lặp $x_n = f(x_{n-1})$, sẽ đi vào một chu trình trong thời gian kỳ vọng $O(\sqrt p)$．Nếu tìm được $x_i \equiv x_j \pmod p$, thì $p$ chia hết $\gcd(|x_i-x_j|, N)$, và ước lớn nhất này là một ước không tầm thường của $N$．

Để hiểu thời gian kỳ vọng đi vào chu trình là $O(\sqrt p)$, có thể lấy cảm hứng từ nghịch lý sinh nhật．

### Nghịch lý sinh nhật

Bỏ qua năm sinh (giả sử mỗi năm đều có 365 ngày), hỏi: cần ít nhất bao nhiêu người trong phòng để xác suất có hai người cùng ngày sinh đạt $50\%$?

Giải: giả sử năm có $n$ ngày, phòng có $k$ người, đánh số $1,2,\dots,k$．Giả định sinh nhật phân bố đều trên $n$ ngày và độc lập．

Gọi sự kiện $A$ là $k$ người sinh nhật đôi một khác nhau, thì

$$
P(A)=\prod_{i=0}^{k-1}\frac{n-i}{n}
$$

Xác suất có ít nhất hai người trùng sinh nhật là $P(\overline A)=1-P(A)$．Theo đề bài $P(\overline A)\ge\frac{1}{2}$, nên

$$
P(A)=\prod_{i=0}^{k-1}\frac{n-i}{n} \le \frac{1}{2}
$$

Dùng bất đẳng thức $1+x\le \mathrm{e}^x$:

$$
P(A) \le \prod_{i=1}^{k-1}\exp\left({-\frac{i}{n}}\right)=\exp \left({-\frac{k(k-1)}{2n}}\right)
$$

Vì vậy

$$
\exp\left({-\dfrac{k(k-1)}{2n}}\right) \le \frac{1}{2}\implies P(A) \le \frac{1}{2}
$$

Thế $n=365$, giải ra $k\geq 23$．Tức ít nhất $23$ người để xác suất trùng sinh nhật đạt $50\%$, điều này khá phản trực giác nên gọi là nghịch lý．

Khi $k>56$, $n=365$, xác suất trùng sinh nhật lớn hơn $99\%$[^ref1]．Do đó, với $n$ ngày, khi phòng có $\frac{1}{2}(\sqrt{8n\ln 2+1}+1)\approx \sqrt{2n\ln 2}$ người, xác suất trùng khoảng $50\%$．

Tương tự, chọn ngẫu nhiên một dãy sinh nhật, số người kỳ vọng để lần đầu trùng cũng là $O(\sqrt n)$．Gọi số này là $X$, thì

$$
E(X) = \sum_{x=1}^{n+1}P(X\ge x+1) = \sum_{x=0}^n\frac{n!}{(n-x)!n^x} = \sqrt{\frac{\pi n}{2}}-\frac13+o(1).
$$

Điều này gợi ý: nếu lấy ngẫu nhiên một dãy số, kích thước kỳ vọng để xuất hiện phần tử lặp cũng là $O(\sqrt n)$．

### Dùng GCD để tìm ước

Không thể trực tiếp tạo dãy ngẫu nhiên modulo $p$ vì $p$ cần tìm．Do đó ta dùng $f(x)=(x^2+c)\bmod N$ để sinh dãy giả ngẫu nhiên $\{x_i\}$: chọn ngẫu nhiên $x_1$, đặt $x_2=f(x_1),\ x_3=f(x_2),\ \dots$ với $c\in[1,N)$ ngẫu nhiên．

Hàm này dễ tính và thường cho dãy khá ngẫu nhiên, nhưng không hoàn toàn ngẫu nhiên．Ví dụ $n=50,\ c=6,\ x_1=1$, ta có

$$
1, 7, 5, 31, 17, 45, 31, 17, 45, 31,\dots
$$

Dữ liệu từ $x_4$ trở đi lặp trong $31,17,45$．Nếu sắp xếp như hình, đồ thị trông như chữ $\rho$, nên thuật toán có tên rho．

![pollard-rho](./images/pollard-rho.svg)

Quan trọng hơn, hàm này thực sự tạo một tự ánh xạ trên $\mathbb Z_p$．Tức nếu $x\equiv y\pmod p$ thì $f(x)\equiv f(y)\pmod p$．

???+ note "Chứng minh"
    Nếu $x\equiv y\pmod p$ thì $x^2+c\equiv y^2+c\pmod p$．Lưu ý $f(x)=x^2+c-k_xN$, với $k_x$ là số nguyên phụ thuộc $x$, và $p|N$, nên $f(x)=x^2+c\pmod p$, do đó $f(x)=f(y)\pmod p$．

Dãy $\{x_n\bmod p\}$ sẽ lặp trong thời gian kỳ vọng $O(\sqrt p)$．Khi quan sát được lặp $x_i\equiv x_j\pmod p$, ta có thể lấy $\gcd(|x_i-x_j|,N)$ để tìm một ước không tầm thường của $N$．Vì $p$ chưa biết, ta không thể trực tiếp nhận biết lặp, cách đơn giản là kiểm tra $\gcd(|x_i-x_j|,N)$ lớn hơn $1$．

Thuật toán không luôn thành công, vì $\gcd(|x_i-x_j|,N)$ có thể bằng $N$, tức $x_i\equiv x_j\pmod N$．Khi đó, chu trình của $\{x_n\bmod p\}$ trùng với chu trình của $\{x_n\}$, không tìm được ước không tầm thường, cần đổi $c$ và chạy lại．

Theo phân tích, bất kỳ hàm $f(x)$ thỏa $\forall x \equiv y \pmod p, f(x) \equiv f(y) \pmod p$ và đủ giả ngẫu nhiên (ví dụ đa thức) đều dùng được．Thực tế thường dùng $f(x)=x^2+c\ (c\neq 0,-2)$．[^pseudo]

### Cài đặt

Cần nhanh chóng phát hiện lặp của $\{x_n\bmod p\}$ trong quá trình lặp．Xem $f$ như cạnh trên đồ thị có hướng với đỉnh $\mathbb Z_p$, bài toán là phát hiện chu trình, chỉ khác là thay phép so sánh bằng kiểm tra $\gcd(|x_i-x_j|,N)>1$．

#### Floyd phát hiện chu trình

Giả sử hai người chạy vòng, A nhanh, B chậm, sau một thời gian A sẽ gặp B, và chênh lệch quãng đường là bội số chu vi．

Đặt $a=f(0),b=f(f(0))$, mỗi lần cập nhật $a=f(a),b=f(f(b))$, chỉ cần kiểm tra $a$ và $b$ có bằng nhau không; nếu có, có chu trình．

Mỗi lần đặt $d=\gcd(|x_i-x_j|,N)$, kiểm tra $1< d< N$, nếu thỏa trả $d$．Nếu $d=N$, tức $\{x_i\}$ đã vào chu trình, không thể tiếp tục, trả về $N$, và đổi $c$ để phân rã lại．

??? note "Pollard-Rho dựa trên Floyd"
    === "C++"
        ```cpp
        ll Pollard_Rho(ll N) {
          if (N == 4) return 2;  // Do ban đầu nhảy hai bước nên cần đặc biệt cho 4
          ll c = rand() % (N - 1) + 1;
          ll t = f(0, c, N);
          ll r = f(f(0, c, N), c, N);
          while (t != r) {
            ll d = gcd(abs(t - r), N);
            if (d > 1) return d;
            t = f(t, c, N);
            r = f(f(r, c, N), c, N);
          }
          return N;
        }
        ```
    
    === "Python"
        ```python
        import random
        
        
        def Pollard_Rho(N):
            if N == 4:
                return 2  # Do ban đầu nhảy hai bước nên cần đặc biệt cho 4
            c = random.randint(1, N - 1)
            t = f(0, c, N)
            r = f(f(0, c, N), c, N)
            while t != r:
                d = gcd(abs(t - r), N)
                if d > 1:
                    return d
                t = f(t, c, N)
                r = f(f(r, c, N), c, N)
            return N
        ```

#### Brent phát hiện chu trình

Thực tế, Floyd có thể cải thiện hằng số．Brent bắt đầu từ $k=1$ tăng dần $k$, ở vòng $k$ để A đứng yên, B tiến $2^k$ bước, nếu B gặp A thì có chu trình, nếu không, để A nhảy đến vị trí B rồi tiếp tục vòng sau．

Có thể chứng minh[^brent] số lần gọi $f$ trước khi gặp chu trình không lớn hơn Floyd．Bài gốc cho thấy Brent giảm thời gian trung bình khoảng $24\%$ so với Floyd．

#### Tối ưu nhân dồn (batch gcd)

Dù dùng Floyd hay Brent, số vòng lặp là $O(\sqrt p)$．Nhưng mỗi bước đều tính $\gcd$ sẽ chậm．Có thể dùng tích dồn để giảm số lần $\gcd$．

Cụ thể, nếu $\gcd(a,N)>1$ thì với mọi $b\in\mathbb N_+$, $\gcd(ab\bmod N,N)=\gcd(ab,N)>1$．Tức nếu tính $\gcd(\prod |x_i-x_j| \bmod N,N)>1$ thì chắc chắn có một cặp $(x_i,x_j)$ thỏa $\gcd(|x_i-x_j|,N)>1$．Nếu tích này bằng $0$ tại thời điểm nào đó thì phân rã thất bại, trả về $N$．

Nếu mỗi $k$ cặp mới tính một lần $\gcd$, độ phức tạp giảm còn $O(\sqrt p+k^{-1}\sqrt p\log N)$, trong đó $\log N$ là chi phí một lần $\gcd$．Khi $k$ cỡ $\log N$ thì kỳ vọng $O(\sqrt p)$．Thực tế thường chọn $k=128$．

Dưới đây là Brent + batch gcd．

??? note "Cài đặt"
    === "C++"
        ```cpp
        ll Pollard_Rho(ll x) {
          ll t = 0;
          ll c = rand() % (x - 1) + 1;
          ll s = t;
          int step = 0, goal = 1;
          ll val = 1;
          for (goal = 1;; goal <<= 1, s = t, val = 1) {
            for (step = 1; step <= goal; ++step) {
              t = f(t, c, x);
              val = val * abs(t - s) % x;
              // Nếu val bằng 0, thoát để phân rã lại
              if (!val) return x;
              if (step % 127 == 0) {
                ll d = gcd(val, x);
                if (d > 1) return d;
              }
            }
            ll d = gcd(val, x);
            if (d > 1) return d;
          }
        }
        ```
    
    === "Python"
        ```python
        from random import randint
        from math import gcd
        
        
        def Pollard_Rho(x):
            c = randint(1, x - 1)
            s = t = f(0, c, x)
            goal = val = 1
            while True:
                for step in range(1, goal + 1):
                    t = f(t, c, x)
                    val = val * abs(t - s) % x
                    if val == 0:
                        return x  # Nếu val bằng 0, thoát để phân rã lại
                    if step % 127 == 0:
                        d = gcd(val, x)
                        if d > 1:
                            return d
                d = gcd(val, x)
                if d > 1:
                    return d
                s = t
                goal <<= 1
                val = 1
        ```

#### Độ phức tạp

Số bước lặp kỳ vọng là $O(\sqrt p)$, với $p$ là thừa số nguyên tố nhỏ nhất của $N$．Dù dùng Floyd hay Brent, nếu không tối ưu nhân dồn, kỳ vọng $O(\sqrt p\log N)$; có tối ưu thì gần $O(\sqrt p)$．

Đáng chú ý, phân tích ở trên dựa trên tự ánh xạ ngẫu nhiên hoàn toàn, trong khi Pollard-Rho dùng hàm giả ngẫu nhiên, nên không có phân tích nghiêm ngặt, nhưng thực tế chạy khá nhanh．

#### Bài mẫu: tìm thừa số nguyên tố lớn nhất

Bài ví dụ：[P4718【模板】Pollard-Rho 算法](https://www.luogu.com.cn/problem/P4718)

Với số $n$, dùng [Miller Rabin](./prime.md#millerrabin-素性测试) để kiểm tra nguyên tố, nếu là nguyên tố thì trả về; nếu không, dùng Pollard-Rho tìm một ước $p$, chia $n$ cho $p$．Sau đó đệ quy phân rã $n$ và $p$, dùng Miller Rabin kiểm tra và cập nhật max\_factor để tìm thừa số nguyên tố lớn nhất．Dữ liệu đề này rất lớn, dùng Floyd là không đủ, nên dùng tối ưu nhân dồn．

??? note "Cài đặt"
    ```cpp
    --8<-- "docs/math/code/pollard-rho/pollard-rho_1.cpp"
    ```

## Tài liệu tham khảo

[^ref1]: <https://en.wikipedia.org/wiki/Birthday_problem#Reverse_problem>

[^pseudo]: Menezes, Alfred J.; van Oorschot, Paul C.; Vanstone, Scott A. (2001). Handbook of Applied Cryptography. Section 3.11 and 3.12.

[^brent]: Brent, R. P. (1980), An improved Monte Carlo factorization algorithm, BIT Numerical Mathematics, 20(2): 176–184, doi:10.1007/BF01933190
