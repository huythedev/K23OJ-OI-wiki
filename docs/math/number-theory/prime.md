author: Ir1d, Tiphereth-A, c-forrest, Xeonacid, Enter-tainer, StudyingFather, iamtwz, ksyx, Marcythm, MegaOwIer, 383494, Alpacabla, HeRaNO, abc1763613206, alphagocc, Backl1ght, CCXXXI, drkelo, Early0v0, Great-designer, greyqz, GuanghaoYe, H-J-Granger, HHH2309, isdanni, kenlig, lazyasn, Menci, ouuan, r-value, shawlleyw, shopee-jin, shuzhouliu, Siger Young, TrisolarisHD, untitledunrevised, void-mian, Voileexperiments, weilycoder, xtlsoft, yusancky, YuzhenQin1, sun2snow

Định nghĩa số nguyên tố và hợp số xem tại [cơ sở số học](./basic.md).

Hàm đếm số nguyên tố: số lượng nguyên tố không vượt quá $x$, ký hiệu $\pi(x)$. Khi $x$ lớn, có xấp xỉ: $\pi(x) \sim \dfrac{x}{\ln(x)}$.

## Kiểm tra nguyên tố

**Kiểm tra nguyên tố** (Primality test) dùng để xác định một số tự nhiên có phải nguyên tố hay không.

Có hai loại:

1.  Kiểm tra xác định: chắc chắn đúng tuyệt đối. Ví dụ: thử chia, kiểm tra Lucas–Lehmer, chứng minh nguyên tố bằng đường cong elliptic.
2.  Kiểm tra xác suất: thường nhanh hơn nhiều nhưng có xác suất nhỏ sai (nhận hợp số là nguyên tố). Vì vậy, số qua kiểm tra xác suất gọi là **có thể nguyên tố**, cho đến khi được chứng minh bằng phương pháp xác định. Số qua kiểm tra nhưng thực ra là hợp số gọi là **giả nguyên tố**. Có nhiều loại giả nguyên tố, phổ biến nhất là giả nguyên tố Fermat (thỏa định lý nhỏ Fermat). Ví dụ phổ biến: Miller–Rabin.

### Thử chia

Cách vét cạn là thử chia mọi số nhỏ hơn:

???+ example "Cài đặt tham khảo"
    === "C++"
        ```cpp
        bool isPrime(int a) {
          if (a < 2) return false;
          for (int i = 2; i < a; ++i)
            if (a % i == 0) return false;
          return true;
        }
        ```
    
    === "Python"
        ```python
        def isPrime(a):
            if a < 2:
                return False
            for i in range(2, a):
                if a % i == 0:
                    return False
            return True
        ```

Cách này chắc chắn nhưng có cần thử tất cả không?

Dễ thấy: nếu $x$ là ước của $a$ thì $\frac{a}{x}$ cũng là ước.

Do đó, với mỗi cặp $(x, \frac{a}{x})$ chỉ cần kiểm tra một số. Ta xét số nhỏ hơn trong cặp; các số này nằm trong $[1,\sqrt{a}]$.

Vì $1$ chắc chắn là ước, nên không cần kiểm tra.

???+ example "Cài đặt tham khảo"
    === "C++"
        ```cpp
        bool isPrime(int a) {
          if (a < 2) return 0;
          for (int i = 2; (long long)i * i <= a; ++i)  // tránh tràn
            if (a % i == 0) return 0;
          return 1;
        }
        ```
    
    === "Python"
        ```python
        def isPrime(a):
            if a < 2:
                return False
            for i in range(2, int(sqrt(a)) + 1):
                if a % i == 0:
                    return False
            return True
        ```

### Kiểm tra Fermat

**Kiểm tra Fermat** là kiểm tra xác suất đơn giản nhất.

Dựa vào [định lý nhỏ Fermat](./fermat.md#费马小定理):

Ý tưởng: chọn nhiều cơ số $a$ trong $[2,n-1]$, kiểm tra xem có luôn $a^{n-1} \equiv 1 \pmod n$ hay không.

???+ example "Cài đặt tham khảo"
    === "C++"
        ```cpp
        bool fermat(int n) {
          if (n < 3) return n == 2;
          // test_time là số lần kiểm tra, nên >= 8
          // để đảm bảo xác suất đúng, nhưng không quá lớn để tránh chậm
          for (int i = 1; i <= test_time; ++i) {
            int a = rand() % (n - 2) + 2;
            if (quickPow(a, n - 1, n) != 1) return false;
          }
          return true;
        }
        ```
    
    === "Python"
        ```python
        def fermat(n):
            if n < 3:
                return n == 2
            # test_time là số lần kiểm tra, nên >= 8
            # để đảm bảo xác suất đúng, nhưng không quá lớn để tránh chậm
            for i in range(1, test_time + 1):
                a = random.randint(0, 32767) % (n - 2) + 2
                if quickPow(a, n - 1, n) != 1:
                    return False
            return True
        ```

Nếu $a^{n−1} \equiv 1 \pmod n$ nhưng $n$ không nguyên tố, thì $n$ gọi là **giả nguyên tố Fermat** theo cơ số $a$. Thực tế, nếu $a^{n−1} \equiv 1 \pmod n$ thì $n$ thường là nguyên tố. Nhưng có phản ví dụ: $n=341$, $a=2$ thì $2^{340}\equiv 1 {\pmod {341}}$ nhưng $341=11\cdot31$ là hợp số. Thậm chí với mỗi cơ số cố định $a$ có vô hạn phản ví dụ[^inf-fermat-pp].

Do đó, kiểm tra Fermat đơn lẻ không đủ. Tăng số cơ số có thể giúp, nhưng kể cả thử tất cả $a$ nguyên tố cùng $n$ vẫn không đảm bảo $n$ nguyên tố. Tức là, đảo của định lý nhỏ Fermat không đúng. Các số này gọi là [số Carmichael](./primitive-root.md#carmichael-数), và cũng vô hạn. Vì vậy cần kiểm tra mạnh hơn.

### Kiểm tra Miller–Rabin

**Miller–Rabin** là kiểm tra tốt hơn. Được Miller và Rabin cải tiến từ Fermat. Cũng là xác suất: có thể phát hiện giả nguyên tố, nhưng để chắc chắn cần thuật toán xác định chậm hơn. Trên thực tế, chưa biết số nào qua Miller–Rabin mà vẫn hợp số, nên có thể dùng an toàn.

Bỏ qua độ phức tạp nhân, kiểm tra $k$ vòng cho $n$ có độ phức tạp $O(k \log n)$. Với số lớn, độ phức tạp là $O(k \log^3n)$, dùng FFT có thể tối ưu tới [$O(k \log^2n \log \log n \log \log \log n)$](https://en.wikipedia.org/wiki/Miller%E2%80%93Rabin_primality_test#Complexity).

Để xử lý Carmichael, Miller–Rabin dùng tính chất:

???+ note "Định lý thăm dò bậc hai"
    Nếu $p$ là nguyên tố lẻ, nghiệm của $x^2 \equiv 1 \pmod p$ là $x \equiv 1 \pmod p$ hoặc $x \equiv p - 1 \pmod p$.

??? note "Chứng minh"
    Dễ thấy $x\equiv 1\pmod p$ và $x\equiv p-1\pmod p$ đều thỏa. Theo [định lý Lagrange](./congruence-equation.md#定理-3lagrange-定理), đó là tất cả nghiệm.

Kết hợp định lý Fermat và định lý trên:

1.  Tách $n−1=u \times 2^t$;
2.  Mỗi vòng chọn $a$, tính $v = a^{u} \bmod n$, rồi bình phương tối đa $t$ lần;
3.  Nếu phát hiện căn bậc hai không tầm thường của $1$ (không phải $\pm1$), kết luận không nguyên tố;
4.  Ngược lại, kiểm tra Fermat.

Một số chi tiết:

-   Nếu trong quá trình có $a^{u \times 2^s} \equiv n-1 \pmod n$, các lần bình phương sau sẽ ra $1$, có thể chấp nhận vòng kiểm tra.
-   Nếu tìm được căn bậc hai không tầm thường $a^{u \times 2^s} \not\equiv n-1 \pmod n$, thì các lần bình phương sau sẽ ra $1$. Có thể trả `false` ngay hoặc sau khi xong $t$ lần.

Miller–Rabin chuẩn (từ fjzzq2002):

???+ example "Cài đặt tham khảo"
    === "C++"
        ```cpp
        bool millerRabin(int n) {
          if (n < 3 || n % 2 == 0) return n == 2;
          if (n % 3 == 0) return n == 3;
          int u = n - 1, t = 0;
          while (u % 2 == 0) u /= 2, ++t;
          // test_time là số lần kiểm tra, nên >= 8
          // để đảm bảo xác suất đúng, nhưng không quá lớn để tránh chậm
          for (int i = 0; i < test_time; ++i) {
            // 0, 1, n-1 có thể qua ngay, a trong [2, n-2]
            int a = rand() % (n - 3) + 2, v = quickPow(a, u, n);
            if (v == 1) continue;
            int s;
            for (s = 0; s < t; ++s) {
              if (v == n - 1) break;  // được căn tầm thường n-1, qua vòng
              v = (long long)v * v % n;
            }
            // Nếu có căn không tầm thường thì sẽ không break sớm, chạy tới s == t
            // Nếu Fermat không qua thì v cũng không bao giờ là -1 trước s == t
            if (s == t) return 0;
          }
          return 1;
        }
        ```
    
    === "Python"
        ```python
        def millerRabin(n):
            if n < 3 or n % 2 == 0:
                return n == 2
            if n % 3 == 0:
                return n == 3
            u, t = n - 1, 0
            while u % 2 == 0:
                u = u // 2
                t = t + 1
            # test_time là số lần kiểm tra, nên >= 8
            # để đảm bảo xác suất đúng, nhưng không quá lớn để tránh chậm
            for i in range(test_time):
                # 0, 1, n-1 có thể qua ngay, a trong [2, n-2]
                a = random.randint(2, n - 2)
                v = pow(a, u, n)
                if v == 1:
                    continue
                s = 0
                while s < t:
                    if v == n - 1:
                        break
                    v = v * v % n
                    s = s + 1
                # Nếu có căn không tầm thường thì sẽ không break sớm, chạy tới s == t
                # Nếu Fermat không qua thì v cũng không bao giờ là -1 trước s == t
                if s == t:
                    return False
            return True
        ```

Có thể chứng minh[^millerrabinproof] với hợp số lẻ $n>9$, xác suất qua một cơ số $a$ của Miller–Rabin không vượt quá $1/4$. Do đó, chọn $k$ cơ số ngẫu nhiên thì xác suất sai không vượt quá $1/4^k$.

??? note "Chứng minh"
    Đặt $n-1=u2^t$, với $u$ lẻ, $t$ dương. Khi đó $n$ qua Miller–Rabin với cơ số $a$ khi
    
    $$
    a^u\equiv 1{\textstyle\pmod n},\text{ hoặc }a^{u2^i}\equiv -1{\textstyle\pmod n}\text{ với }0\le i < t.
    $$
    
    Gọi $S$ là tập các $a$ thỏa. Cần chứng minh
    
    $$
    |S| \le \dfrac14\varphi(n).
    $$
    
    Trong đó $\varphi(n)$ là [hàm Euler](./euler-totient.md). Chứng minh gồm ba bước.
    
    **Bước 1**: Gọi $\ell$ là số nguyên dương lớn nhất sao cho $2^\ell \mid p-1$ với mọi ước nguyên tố $p$ của $n$. Khi đó
    
    $$
    S\subseteq S' = \{a\bmod n:a^{u2^{\ell-1}}\equiv\pm 1{\textstyle\pmod n}\}.
    $$
    
    Với $a\in S$, nếu $a^u\equiv 1\pmod n$ thì hiển nhiên $a^{u2^{\ell-1}}\equiv 1\pmod n$, nên $a\in S'$. Nếu tồn tại $0\le i < t$ sao cho $a^{u2^i}\equiv -1\pmod n$, thì với mọi $p\mid n$ cũng có $a^{u2^i}\equiv-1\pmod p$. Gọi $\delta_p(a)$ là [bậc](./primitive-root.md#阶) của $a$ mod $p$, ta có $\delta_p(a)\mid u2^{i+1}$ nhưng $\delta_p(a)\nmid u2^{i}$, nên trong phân tích của $\delta_p(a)$, số mũ của $2$ là $i+1$, do đó $2^{i+1}\mid\delta_p(a)$. Theo Fermat, $\delta_p(a)\mid p-1$, nên $2^{i+1}\mid p-1$ với mọi $p\mid n$, suy ra $i+1\le\ell$. Vậy $a^{u2^{\ell-1}} = (a^{u2^i})^{2^{\ell-1-i}} \equiv \pm 1 \pmod n$, nên $a\in S'$. Suy ra $S\subseteq S'$.
    
    **Bước 2**: Tính $|S'|$.
    
    Viết $n = p_1^{e_1}\cdots p_k^{e_k}$. Theo [CRT](./crt.md), điều kiện $a^{u2^{\ell - 1}}\equiv 1\pmod n$ tương đương $a^{u2^{\ell - 1}}\equiv 1\pmod{p_i^{e_i}}$ với mọi $i$. Do với môđun lũy thừa nguyên tố lẻ luôn có [nguyên căn](./primitive-root.md#原根), số nghiệm của $a^{u2^{\ell - 1}}\equiv 1\pmod{p_i^{e_i}}$ là
    
    $$
    \gcd(u2^{\ell-1},p_i^{e_i-1}(p_i-1)) = \gcd(u2^{\ell-1},p_i-1) = 2^{\ell-1}\gcd(u,p_i-1).
    $$
    
    Dấu bằng đầu do $u$ chia $n-1$ nên không chia $p_i$; dấu bằng thứ hai do cách chọn $\ell$. Suy ra số nghiệm của $a^{u2^{\ell-1}}\equiv 1\pmod n$ là
    
    $$
    \prod_{p\mid n}2^{\ell-1}\gcd(u,p-1).
    $$
    
    Tương tự, điều kiện $a^{u2^{\ell - 1}}\equiv -1\pmod n$ tương đương $a^{u2^{\ell - 1}}\equiv -1\pmod{p_i^{e_i}}$ với mọi $i$. Với mỗi $p_i^{e_i}$, điều kiện này tương đương $a^{u2^{\ell - 1}}\not\equiv 1\pmod{p_i^{e_i}}$ và $a^{u2^{\ell}}\equiv 1\pmod{p_i^{e_i}}$. Số nghiệm của $a^{u2^{\ell}}\equiv 1\pmod{p_i^{e_i}}$ là $2^{\ell}\gcd(u,p_i-1)$, nên số nghiệm của $a^{u2^{\ell - 1}}\equiv -1\pmod{p_i^{e_i}}$ cũng là
    
    $$
    2^{\ell}\gcd(u,p_i-1) - 2^{\ell-1}\gcd(u,p_i-1) = 2^{\ell-1}\gcd(u,p_i-1).
    $$
    
    Theo CRT, số nghiệm của $a^{u2^{\ell - 1}}\equiv -1\pmod n$ là
    
    $$
    \prod_{p\mid n}2^{\ell-1}\gcd(u,p-1).
    $$
    
    Do đó
    
    $$
    |S'| = 2\prod_{p\mid n}2^{\ell-1}\gcd(u,p-1).
    $$
    
    **Bước 3**: Chứng minh $|S'|\le\varphi(n)/4$.
    
    Với $\varphi(n)=\prod_ip_i^{e_i-1}(p_i-1)$, ta có
    
    $$
    \dfrac{\varphi(n)}{|S'|} = \dfrac{1}{2}\prod_ip_i^{e_i-1}\dfrac{p_i-1}{2^{\ell-1}\gcd(u,p_i-1)}.
    $$
    
    Mỗi thừa số $p_i^{e_i-1}\dfrac{p_i-1}{2^{\ell-1}\gcd(u,p_i-1)}$ là chẵn, nên $\varphi(n)/|S'|$ là số nguyên. Giả sử $|S'|\le\varphi(n)/4$ không đúng. Khi đó $\varphi(n)/|S'|=1,2,3$, tức
    
    $$
    \prod_ip_i^{e_i-1}\dfrac{p_i-1}{2^{\ell-1}\gcd(u,p_i-1)} = 2,4,6.
    $$
    
    Do mỗi thừa số đều chẵn, tích chỉ có thể có một thừa số bằng $2,4,6$ hoặc hai thừa số đều bằng $2$.
    
    Trường hợp có hai thừa số: khi đó không có thừa số nguyên tố lẻ, nên $p_i^{e_i-1}=1$, tức $n$ không có bình phương nguyên tố. Giả sử $n=p_1p_2$, $p_1<p_2$ nguyên tố. Hai thừa số bằng $2$ nên $p_i-1=2^{\ell}\gcd(u,p_i-1)$, tức $p_i=1+2^\ell m_i$ với $m_i$ lẻ và $m_i\mid u$. Lấy $p_1p_2=n=1+u2^t$ mod $m_1$ suy ra $p_2\equiv 1\pmod{m_1}$, nên $m_1\mid m_2$. Tương tự $m_2\mid m_1$, nên $m_1=m_2$, suy ra $p_1=p_2$, mâu thuẫn.
    
    Trường hợp chỉ một thừa số: hợp số $n=p^e$, $e>1$. Khi đó $p^{e-1}\mid 2,4,6$, chỉ có thể $p=3,e=2$, tức $n=9$, trái với giả thiết. Vậy mâu thuẫn, suy ra $|S'|\le\varphi(n)/4$.

Ngoài ra, nếu [giả thuyết Riemann tổng quát](https://en.wikipedia.org/wiki/Generalized_Riemann_hypothesis) đúng thì để xác định nguyên tố của $n$, chỉ cần thử tất cả $a$ trong $[2, \min\{n-2, \lfloor 2\ln^2 n \rfloor\}]$.[^deterministic-proof]

Trong OI, thường xét $[1, 2^{64})$. Với $[1, 2^{32})$, dùng cơ số $\{2, 7, 61\}$ là đủ xác định nguyên tố; với $[1, 2^{64})$, dùng $\{2, 325, 9375, 28178, 450775, 9780504, 1795265022\}$ là đủ.[^witnesses]

Cũng có thể dùng $\{2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37\}$ (12 nguyên tố đầu) để kiểm tra $[1, 2^{64})$.

Lưu ý khi dùng các cơ số trên:

-   Phải thử tất cả cơ số, không chỉ những số nhỏ hơn $n$;
-   Thay $a$ bằng $a \bmod n$;
-   Nếu $a \equiv 0 \pmod n$ hoặc $a \equiv \pm 1 \pmod n$ thì vòng đó qua ngay.

## Phản nguyên tố

Hiểu đơn giản: số nguyên tố có đúng 2 ước, còn phản nguyên tố là số có nhiều ước nhất (và nếu bằng nhau thì giá trị nhỏ nhất) trong một tập.

Một định nghĩa trực giác: trong tập các số nguyên dương, số có nhiều ước nhất và nhỏ nhất là phản nguyên tố.

???+ abstract "Phản nguyên tố"
    Với số nguyên dương $n$, nếu mọi số dương nhỏ hơn $n$ đều có số ước nhỏ hơn $n$, thì $n$ là **phản nguyên tố** (anti-prime, a.k.a., highly composite number).

???+ warning "Chú ý"
    Không nhầm với [emirp](https://en.wikipedia.org/wiki/Emirp): số nguyên tố khi đảo chữ số vẫn là nguyên tố khác (ví dụ 149 và 941 là emirp, 101 không phải).

### Quy trình

Cách giải phản nguyên tố:

Trước hết cần phân tích nhân tử: $n=p_{1}^{k_{1}}p_{2}^{k_{2}} \cdots p_{n}^{k_{n}}$, số ước là $(k_1+1)(k_2+1)\cdots(k_n+1)$.

Phân tích trực tiếp tốn kém và không tái sử dụng được kết quả, nên cần cách khác.

Quan sát đặc điểm:

1.  Phản nguyên tố là tích các lũy thừa của các số nguyên tố liên tiếp bắt đầu từ $2$.
2.  Số mũ của nguyên tố nhỏ không nhỏ hơn số mũ của nguyên tố lớn: $k_1 \geq k_2 \geq \cdots \geq k_n$.

Giải thích:

1.  Nếu không bắt đầu từ $2$ hoặc bỏ sót nguyên tố, có thể thay bằng nguyên tố nhỏ hơn, số ước không đổi nhưng giá trị nhỏ hơn.
2.  Nếu nguyên tố nhỏ có số mũ nhỏ hơn nguyên tố lớn, đổi vị trí sẽ giữ số ước nhưng giảm giá trị.

Hai câu hỏi:

1.  Với $n$, cần xét tới nguyên tố nào?

    Trường hợp cực đoan $n=p_{1}p_{2}\cdots p_{n}$, nên chỉ cần nhân các nguyên tố liên tiếp tới khi tích vừa $\le n$. Nếu xét nguyên tố lớn hơn thì buộc có nguyên tố trước đó mũ $0$, không thể là phản nguyên tố.

2.  Cần xét tới mũ bao nhiêu?

    Nếu mũ của nguyên tố nhỏ nhất đã vượt quá $n$ (giá trị lớn nhất), thì các mũ khác đều nhỏ hơn. Trường hợp cực đoan $n=2^k$, nên xét tới $\lfloor\log_2 n\rfloor$.

Đủ chi tiết, ta triển khai DFS:

-   Mỗi mức xét một nguyên tố;
-   Duyệt số mũ giảm dần;
-   Dừng khi:
    1.  Giá trị vượt $n$;
    2.  Số ước đã vượt ngưỡng cần;
    3.  Giá trị số ước vượt ngưỡng;
    4.  Đúng số ước cần (cập nhật đáp án nhỏ nhất).

### Bài mẫu

???+ example "[Codeforces 27E. A number with a given number of divisors](https://codeforces.com/problemset/problem/27/E)"
    Tìm số tự nhiên nhỏ nhất có đúng số ước cho trước. Đáp án $\le 10^{18}$.

??? note "Ý tưởng"
    Dùng DFS với điều kiện dừng theo số ước, cập nhật đáp án nhỏ nhất.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/prime/prime_1.cpp"
    ```

???+ example "[ZOJ 2562 More Divisors](https://pintia.cn/problem-sets/91827364500/exam/problems/type/7?problemSetProblemId=91827366061)"
    Tìm số $\le n$ có số ước nhiều nhất.

??? note "Ý tưởng"
    Tương tự, thay điều kiện dừng trong DFS. Lưu ý tràn 32-bit.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/math/code/prime/prime_2.cpp"
    ```

## Tài liệu tham khảo và chú thích

1.  Rui-Juan Jing, Marc Moreno-Maza, Delaram Talaashrafi, "[Complexity Estimates for Fourier-Motzkin Elimination](https://arxiv.org/abs/1811.01510)", Journal of Functional Programming 16:2 (2006) pp 197-217.
2.  [Số học phần 1: số nguyên tố và kiểm tra nguyên tố](http://www.matrix67.com/blog/archives/234)
3.  [Ghi chú Miller–Rabin và Pollard–Rho - Bill Yang's Blog](https://blog.bill.moe/miller-rabin-notes/)
4.  [Primality test - Wikipedia](https://en.wikipedia.org/wiki/Primality_test)
5.  [Fermat pseudoprime - Wikipedia](https://en.wikipedia.org/wiki/Fermat_pseudoprime)
6.  [Ghi chú thuật toán về phản nguyên tố (acm/OI)](https://zhuanlan.zhihu.com/p/41759808)
7.  [The Rabin-Miller Primality Test](http://home.sandiego.edu/~dhoffoss/teaching/cryptography/10-Rabin-Miller.pdf)
8.  [Highly composite number - Wikipedia](https://en.wikipedia.org/wiki/Highly_composite_number)

[^inf-fermat-pp]: Pomerance, Carl, John L. Selfridge, and Samuel S. Wagstaff. "The pseudoprimes to 25⋅ 10⁹." Mathematics of Computation 35, no. 151 (1980): 1003-1026. Định lý 1 chỉ ra rằng với cơ số cố định $a$, vẫn có vô hạn hợp số qua được Miller–Rabin mạnh hơn.

[^millerrabinproof]: Kết luận và chứng minh tham khảo Crandall, Richard, and Carl Pomerance. Prime numbers: a computational perspective. New York, NY: Springer New York, 2005. Mục 3.5.

[^deterministic-proof]: Bach, Eric , "[Explicit bounds for primality testing and related problems](https://doi.org/10.2307%2F2008811)", Mathematics of Computation, 55:191 (1990) pp 355–380.

[^witnesses]: Xem thêm [Deterministic variant of the Miller–Rabin primality test](https://miller-rabin.appspot.com/#).
