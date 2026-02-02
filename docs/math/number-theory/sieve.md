author: inkydragon, TravorLZH, YOYO-UIAT, wood3, shuzhouliu, Mr-Python-in-China, HeRaNO, weilycoder

## Sàng số nguyên tố

### Giới thiệu

Nếu chúng ta muốn biết có bao nhiêu số nguyên tố không vượt quá $n$ thì sao?

Một ý tưởng tự nhiên là kiểm tra tính nguyên tố cho từng số không vượt quá $n$. Cách vét cạn này hiển nhiên không đạt độ phức tạp tối ưu.

### Sàng Eratosthenes

#### Quy trình

Xét một điều như sau: với mọi số nguyên dương $n > 1$, mọi bội số của nó với $x > 1$ đều là hợp số. Dựa vào kết luận này, ta có thể tránh nhiều lần kiểm tra không cần thiết.

Nếu ta xét các số theo thứ tự tăng dần, đồng thời đánh dấu tất cả các bội (lớn hơn chính nó) của số hiện tại là hợp số, thì khi kết thúc, các số chưa bị đánh dấu chính là số nguyên tố.

#### Cài đặt

=== "C++"
    ```cpp
    vector<int> prime;
    bool is_prime[N];
    
    void Eratosthenes(int n) {
      is_prime[0] = is_prime[1] = false;
      for (int i = 2; i <= n; ++i) is_prime[i] = true;
      for (int i = 2; i <= n; ++i) {
        if (is_prime[i]) {
          prime.push_back(i);
          if ((long long)i * i > n) continue;
          for (int j = i * i; j <= n; j += i)
            // Vì các bội từ 2 đến i - 1 đã được sàng trước đó, ở đây bắt đầu
            // từ bội của i để tăng tốc
            is_prime[j] = false;  // Bội của i đều không phải là số nguyên tố
        }
      }
    }
    ```

=== "Python"
    ```python
    prime = []
    is_prime = [False] * N
    
    
    def Eratosthenes(n):
        is_prime[0] = is_prime[1] = False
        for i in range(2, n + 1):
            is_prime[i] = True
        for i in range(2, n + 1):
            if is_prime[i]:
                prime.append(i)
                if i * i > n:
                    continue
                for j in range(i * i, n + 1, i):
                    is_prime[j] = False
    ```

Đây là **sàng Eratosthenes** (Eratosthenes sieve, gọi tắt là sàng Eratosthenes), có độ phức tạp thời gian $O(n\log\log n)$.

???+ note "Chứng minh"
    Bây giờ ta xét quá trình suy luận:
    
    Nếu mỗi lần thao tác trên mảng tốn 1 đơn vị thời gian, thì độ phức tạp thời gian là:
    
    $$
    O\left(\sum_{k=1}^{\pi(n)}{\frac{n}{p_k}}\right)=O\left(n\sum_{k=1}^{\pi(n)}{\frac{1}{p_k}}\right)
    $$
    
    Trong đó $p_k$ là số nguyên tố nhỏ thứ $k$, $\pi(n)$ là số lượng số nguyên tố $\le n$. $\sum_{k=1}^{\pi(n)}$ biểu diễn vòng for ngoài cùng, trong đó cận trên $\pi(n)$ là số lần nhánh true của `if (prime[i])`; $\frac{n}{p_k}$ là số lần thực hiện của vòng for bên trong.
    
    Theo định lý thứ hai của Mertens, tồn tại hằng số $B_1$ sao cho:
    
    $$
    \sum_{k=1}^{\pi(n)}{\frac{1}{p_k}}=\log\log n+B_1+O\left(\frac{1}{\log n}\right)
    $$
    
    Vì vậy **sàng Eratosthenes** có độ phức tạp thời gian $O(n\log\log n)$. Tiếp theo ta chứng minh phiên bản yếu của định lý Mertens thứ hai $\sum_{k\le\pi(n)}1/p_k=O(\log\log n)$:
    
    Theo $\pi(n)=\Theta(n/\log n)$, suy ra số nguyên tố thứ $n$ có kích thước $\Theta(n\log n)$. Do đó:
    
    $$
    \begin{aligned}
    \sum_{k=1}^{\pi(n)}{\frac{1}{p_k}}
    &=O\left(\sum_{k=2}^{\pi(n)}{\frac{1}{k\log k}}\right) \\
    &=O\left(\int_2^{\pi(n)}{\frac{\mathrm dx}{x\log x}}\right) \\
    &=O(\log\log\pi(n))=O(\log\log n)
    \end{aligned}
    $$
    
    Dĩ nhiên, cách làm trên vẫn chưa đủ tối ưu; áp dụng các phương pháp dưới đây có thể cải thiện hiệu năng.

#### Sàng đến căn bậc hai

Rõ ràng, để tìm tất cả số nguyên tố không vượt quá $n$, chỉ cần sàng với các số nguyên tố không vượt quá $\sqrt n$ là đủ.

=== "C++"
    ```cpp
    vector<int> prime;
    bool is_prime[N];
    
    void Eratosthenes(int n) {
      is_prime[0] = is_prime[1] = false;
      for (int i = 2; i <= n; ++i) is_prime[i] = true;
      // i * i <= n nghĩa là i <= sqrt(n)
      for (int i = 2; i * i <= n; ++i) {
        if (is_prime[i])
          for (int j = i * i; j <= n; j += i) is_prime[j] = false;
      }
      for (int i = 2; i <= n; ++i)
        if (is_prime[i]) prime.push_back(i);
    }
    ```

=== "Python"
    ```python
    prime = []
    is_prime = [False] * N
    
    
    def Eratosthenes(n):
        is_prime[0] = is_prime[1] = False
        for i in range(2, n + 1):
            is_prime[i] = True
        # Cho i chạy đến <= sqrt(n)
        for i in range(2, isqrt(n) + 1):  # `isqrt` là hàm mới trong Python 3.8
            if is_prime[i]:
                for j in range(i * i, n + 1, i):
                    is_prime[j] = False
        for i in range(2, n + 1):
            if is_prime[i]:
                prime.append(i)
    ```

Tối ưu này không ảnh hưởng đến độ phức tạp tiệm cận; nếu lặp lại chứng minh ở trên, ta thu được $n \ln \ln \sqrt n + o(n)$, theo tính chất logarit thì chúng tiệm cận như nhau, nhưng số phép toán sẽ giảm đáng kể.

#### Chỉ sàng số lẻ

Vì mọi số chẵn khác 2 đều là hợp số, ta có thể bỏ qua chúng, chỉ cần quan tâm đến số lẻ.

Cách này giúp giảm một nửa nhu cầu bộ nhớ; đồng thời số thao tác cũng giảm khoảng một nửa.

#### Giảm mức sử dụng bộ nhớ

Ta nhận thấy khi sàng chỉ cần mảng kiểu `bool`. Một phần tử `bool` thường chiếm 1 byte (8 bit), nhưng lưu một giá trị boolean chỉ cần 1 bit.

Ta có thể dùng kiến thức [bit thao tác](../bit.md) để nén mỗi boolean vào một bit, khi đó chỉ cần $n$ bit (tức $\dfrac n 8$ byte) thay vì $n$ byte, giúp giảm bộ nhớ đáng kể. Cách này gọi là “nén mức bit”.

Đáng chú ý, tồn tại các cấu trúc dữ liệu tự động thực hiện nén mức bit, như `vector<bool>` và `bitset<>` trong C++.

Ngoài ra, `vector<bool>` và `bitset<>` còn có tối ưu hằng số; sàng Eratosthenes với độ phức tạp $O(n \log \log n)$ khi dùng `bitset<>` hoặc `vector<bool>` có thể nhanh hơn cả sàng Euler có độ phức tạp $O(n)$.

Xem thêm [bitset: kết hợp với sàng Eratosthenes](../../lang/csl/bitset.md#与埃氏筛结合).

#### Sàng theo khối

Từ tối ưu “sàng đến căn bậc hai”, ta thấy không cần giữ toàn bộ mảng `is_prime[1...n]`. Để sàng, chỉ cần giữ các số nguyên tố đến $\sqrt n$, tức `prime[1...sqrt(n)]`. Đồng thời chia toàn bộ miền thành các khối, mỗi khối được sàng riêng. Nhờ đó, ta không cần giữ nhiều khối cùng lúc trong bộ nhớ, và CPU có thể tận dụng cache tốt hơn.

Gọi $s$ là hằng số quyết định kích thước khối, khi đó có $\lceil {\frac n s} \rceil$ khối, và khối $k$($k = 0 \dots \lfloor {\frac n s} \rfloor$) chứa các số trong đoạn $[ks, ks + s - 1]$. Ta xử lý từng khối theo thứ tự: với mỗi khối $k$, ta duyệt mọi số nguyên tố (từ $1$ đến $\sqrt n$) và dùng chúng để sàng.

Lưu ý khi xử lý khối đầu tiên, cần điều chỉnh một chút: thứ nhất, phải giữ tất cả số nguyên tố trong $[1, \sqrt n]$; thứ hai, số 0 và 1 phải được đánh dấu là không nguyên tố. Khi xử lý khối cuối, không nên quên rằng số cuối cùng $n$ không nhất thiết nằm ở cuối khối.

Cài đặt sau dùng sàng theo khối để đếm số lượng số nguyên tố không vượt quá $n$.

???+ note "Cài đặt"
    ```cpp
    int count_primes(int n) {
      constexpr static int S = 10000;
      vector<int> primes;
      int nsqrt = sqrt(n);
      vector<char> is_prime(nsqrt + 1, true);
      for (int i = 2; i <= nsqrt; i++) {
        if (is_prime[i]) {
          primes.push_back(i);
          for (int j = i * i; j <= nsqrt; j += i) is_prime[j] = false;
        }
      }
      int result = 0;
      vector<char> block(S);
      for (int k = 0; k * S <= n; k++) {
        fill(block.begin(), block.end(), true);
        int start = k * S;
        for (int p : primes) {
          int start_idx = (start + p - 1) / p;
          int j = max(start_idx, p) * p - start;
          for (; j < S; j += p) block[j] = false;
        }
        if (k == 0) block[0] = block[1] = false;
        for (int i = 0; i < S && start + i <= n; i++) {
          if (block[i]) result++;
        }
      }
      return result;
    }
    ```

Độ phức tạp tiệm cận của sàng theo khối giống sàng Eratosthenes (trừ khi khối quá nhỏ), nhưng bộ nhớ cần dùng sẽ giảm xuống $O(\sqrt{n} + S)$ và hiệu quả cache tốt hơn.
Mặt khác, với mỗi cặp khối và các số nguyên tố trong $[1, \sqrt{n}]$ đều phải thực hiện phép chia; với các khối nhỏ thì điều này sẽ tệ hơn.
Vì vậy cần cân bằng khi chọn hằng số $S$.

Kích thước khối $S$ lấy trong khoảng $10^4$ đến $10^5$ thường cho tốc độ tốt nhất.

### Sàng tuyến tính

Sàng Eratosthenes vẫn còn có thể tối ưu, vì nó đánh dấu một hợp số nhiều lần. Có cách nào loại bỏ các bước thừa không? Câu trả lời là có.

Nếu đảm bảo mỗi hợp số chỉ bị đánh dấu đúng một lần, thì độ phức tạp thời gian có thể giảm xuống $O(n)$.

???+ note "Cài đặt"
    === "C++"
        ```cpp
        vector<int> pri;
        bool not_prime[N];
        
        void pre(int n) {
          for (int i = 2; i <= n; ++i) {
            if (!not_prime[i]) {
              pri.push_back(i);
            }
            for (int pri_j : pri) {
              if (i * pri_j > n) break;
              not_prime[i * pri_j] = true;
              if (i % pri_j == 0) {
                // i % pri_j == 0
                // Nói cách khác, i đã được sàng bởi pri_j trước đó
                // Vì các số nguyên tố trong pri được sắp tăng dần, nên i nhân với
                // các số nguyên tố lớn hơn sẽ chắc chắn bị sàng bởi bội của pri_j,
                // nên không cần sàng lại ở đây, chỉ cần break
                // là đủ
                break;
              }
            }
          }
        }
        ```
    
    === "Python"
        ```python
        pri = []
        not_prime = [False] * N
        mu = [0] * N
        
        
        def pre(n):
            for i in range(2, n + 1):
                if not not_prime[i]:
                    pri.append(i)
                    mu[i] = -1
                for pri_j in pri:
                    if i * pri_j > n:
                        break
                    not_prime[i * pri_j] = True
                    if i % pri_j == 0:
                        """
                        i % pri_j == 0
                        Nói cách khác, i đã được sàng bởi pri_j trước đó
                        Vì các số nguyên tố trong pri được sắp tăng dần, nên i nhân với
                        các số nguyên tố lớn hơn sẽ chắc chắn bị sàng bởi bội của pri_j,
                        nên không cần sàng lại ở đây, chỉ cần break
                        là đủ
                        """
                        break
        ```

Cách làm trên là **sàng tuyến tính** (còn gọi là **sàng Euler**).

???+ note "Note"
    Lưu ý rằng trong quá trình sàng tìm số nguyên tố, ta cũng thu được ước nguyên tố nhỏ nhất của mỗi số.

## Sàng tính hàm Euler

Lưu ý rằng trong sàng tuyến tính, mỗi hợp số bị sàng bởi ước nguyên tố nhỏ nhất. Ví dụ, đặt $p_1$ là ước nguyên tố nhỏ nhất của $n$, $n' = \frac{n}{p_1}$, thì trong quá trình sàng tuyến tính, $n$ bị sàng bởi $n' \times p_1$.

Quan sát quá trình sàng tuyến tính, ta còn cần xử lý hai trường hợp theo $n' \bmod p_1$.

Nếu $n' \bmod p_1 = 0$, thì $n'$ chứa tất cả các ước nguyên tố của $n$.

$$
\begin{aligned}
\varphi(n) & = n \times \prod_{i = 1}^s{\frac{p_i - 1}{p_i}} \\\\
& = p_1 \times n' \times \prod_{i = 1}^s{\frac{p_i - 1}{p_i}} \\\\
& = p_1 \times \varphi(n')
\end{aligned}
$$

Nếu $n' \bmod p_1 \neq 0$ thì $n'$ và $p_1$ nguyên tố cùng nhau. Theo tính chất của hàm Euler:

$$
\begin{aligned}
\varphi(n) & = \varphi(p_1) \times \varphi(n') \\\\
& = (p_1 - 1) \times \varphi(n')
\end{aligned}
$$

### Cài đặt

=== "C++"
    ```cpp
    vector<int> pri;
    bool not_prime[N];
    int phi[N];
    
    void pre(int n) {
      phi[1] = 1;
      for (int i = 2; i <= n; i++) {
        if (!not_prime[i]) {
          pri.push_back(i);
          phi[i] = i - 1;
        }
        for (int pri_j : pri) {
          if (i * pri_j > n) break;
          not_prime[i * pri_j] = true;
          if (i % pri_j == 0) {
            phi[i * pri_j] = phi[i] * pri_j;
            break;
          }
          phi[i * pri_j] = phi[i] * phi[pri_j];
        }
      }
    }
    ```

=== "Python"
    ```python
    pri = []
    not_prime = [False] * N
    phi = [0] * N
    
    
    def pre(n):
        phi[1] = 1
        for i in range(2, n + 1):
            if not not_prime[i]:
                pri.append(i)
                phi[i] = i - 1
            for pri_j in pri:
                if i * pri_j > n:
                    break
                not_prime[i * pri_j] = True
                if i % pri_j == 0:
                    phi[i * pri_j] = phi[i] * pri_j
                    break
                phi[i * pri_j] = phi[i] * phi[pri_j]
    ```

## Sàng tính hàm Möbius

### Định nghĩa

Theo định nghĩa hàm Möbius, đặt $n$ là hợp số, $p_1$ là ước nguyên tố nhỏ nhất của $n$, $n'=\frac{n}{p_1}$, ta có:

$$
\mu(n)=
\begin{cases}
    0 & n' \bmod p_1 = 0\\\\
    -\mu(n') & \text{ngược lại}
\end{cases}
$$

Nếu $n$ là số nguyên tố thì $\mu(n)=-1$.

### Cài đặt

=== "C++"
    ```cpp
    vector<int> pri;
    bool not_prime[N];
    int mu[N];
    
    void pre(int n) {
      mu[1] = 1;
      for (int i = 2; i <= n; ++i) {
        if (!not_prime[i]) {
          mu[i] = -1;
          pri.push_back(i);
        }
        for (int pri_j : pri) {
          if (i * pri_j > n) break;
          not_prime[i * pri_j] = true;
          if (i % pri_j == 0) {
            mu[i * pri_j] = 0;
            break;
          }
          mu[i * pri_j] = -mu[i];
        }
      }
    }
    ```

=== "Python"
    ```python
    pri = []
    not_prime = [False] * N
    mu = [0] * N
    
    
    def pre(n):
        mu[1] = 1
        for i in range(2, n + 1):
            if not not_prime[i]:
                pri.append(i)
                mu[i] = -1
            for pri_j in pri:
                if i * pri_j > n:
                    break
                not_prime[i * pri_j] = True
                if i % pri_j == 0:
                    mu[i * pri_j] = 0
                    break
                mu[i * pri_j] = -mu[i]
    ```

## Sàng tính số lượng ước

Dùng $d_i$ biểu diễn số lượng ước của $i$, $num_i$ biểu diễn số lần xuất hiện của ước nguyên tố nhỏ nhất của $i$.

### Định lý số lượng ước

Định lý: Nếu $n=\prod_{i=1}^m p_i^{c_i}$ thì $d_i=\prod_{i=1}^m (c_i+1)$.

Chứng minh: Ta biết $p_i^{c_i}$ có các ước $p_i^0,p_i^1,\dots ,p_i^{c_i}$, tổng cộng $c_i+1$ ước. Theo nguyên lý nhân, số lượng ước của $n$ là $\prod_{i=1}^m (c_i+1)$.

### Cài đặt

Vì $d_i$ là hàm nhân tính, nên có thể dùng sàng tuyến tính.

Ở đây giới thiệu ngắn gọn nguyên lý cài đặt sàng tuyến tính:

1.  Khi $i$ là số nguyên tố: $\textit{num}_i \gets 1,\textit{d}_i \gets 2$, đồng thời đặt $q = \left\lfloor \dfrac {i}{p} \right\rfloor$, trong đó $p$ là ước nguyên tố nhỏ nhất của $i$.
2.  Khi $p$ là ước của $q$: $\textit{num}_i \gets \textit{num}_q + 1,\textit{d}_i \gets \dfrac{\textit{d}_q}{\textit{num}_i} \times (\textit{num}_i + 1)$.
3.  Khi $p, q$ nguyên tố cùng nhau: $\textit{num}_i \gets 1,\textit{d}_i \gets \textit{d}_q \times (\textit{num}_i+1)$.

=== "C++"
    ```cpp
    vector<int> pri;
    bool not_prime[N];
    int d[N], num[N];
    
    void pre(int n) {
      d[1] = 1;
      for (int i = 2; i <= n; ++i) {
        if (!not_prime[i]) {
          pri.push_back(i);
          d[i] = 2;
          num[i] = 1;
        }
        for (int pri_j : pri) {
          if (i * pri_j > n) break;
          not_prime[i * pri_j] = true;
          if (i % pri_j == 0) {
            num[i * pri_j] = num[i] + 1;
            d[i * pri_j] = d[i] / num[i * pri_j] * (num[i * pri_j] + 1);
            break;
          }
          num[i * pri_j] = 1;
          d[i * pri_j] = d[i] * 2;
        }
      }
    }
    ```

=== "Python"
    ```python
    pri = []
    not_prime = [False] * N
    d = [0] * N
    num = [0] * N
    
    
    def pre(n):
        d[1] = 1
        for i in range(2, n + 1):
            if not not_prime[i]:
                pri.append(i)
                d[i] = 2
                num[i] = 1
            for pri_j in pri:
                if i * pri_j > n:
                    break
                not_prime[i * pri_j] = True
                if i % pri_j == 0:
                    num[i * pri_j] = num[i] + 1
                    d[i * pri_j] = d[i] // num[i * pri_j] * (num[i * pri_j] + 1)
                    break
                num[i * pri_j] = 1
                d[i * pri_j] = d[i] * 2
    ```

## Sàng tính tổng ước

$f_i$ biểu diễn tổng các ước của $i$, $g_i$ biểu diễn $p^0+p^1+p^2+\dots p^k$ với $p$ là ước nguyên tố nhỏ nhất.

### Cài đặt

=== "C++"
    ```cpp
    vector<int> pri;
    bool not_prime[N];
    int g[N], f[N];
    
    void pre(int n) {
      g[1] = f[1] = 1;
      for (int i = 2; i <= n; ++i) {
        if (!not_prime[i]) {
          pri.push_back(i);
          g[i] = i + 1;
          f[i] = i + 1;
        }
        for (int pri_j : pri) {
          if (i * pri_j > n) break;
          not_prime[i * pri_j] = true;
          if (i % pri_j == 0) {
            g[i * pri_j] = g[i] * pri_j + 1;
            f[i * pri_j] = f[i] / g[i] * g[i * pri_j];
            break;
          }
          f[i * pri_j] = f[i] * f[pri_j];
          g[i * pri_j] = 1 + pri_j;
        }
      }
    }
    ```

=== "Python"
    ```python
    pri = []
    not_prime = [False] * N
    f = [0] * N
    g = [0] * N
    
    
    def pre(n):
        g[1] = f[1] = 1
        for i in range(2, n + 1):
            if not not_prime[i]:
                pri.append(i)
                g[i] = i + 1
                f[i] = i + 1
            for pri_j in pri:
                if i * pri_j > n:
                    break
                not_prime[i * pri_j] = True
                if i % pri_j == 0:
                    g[i * pri_j] = g[i] * pri_j + 1
                    f[i * pri_j] = f[i] // g[i] * g[i * pri_j]
                    break
                f[i * pri_j] = f[i] * f[pri_j]
                g[i * pri_j] = 1 + pri_j
    ```

## Hàm nhân tính tổng quát

Giả sử một [hàm nhân tính](./basic.md#积性函数) $f$ thỏa: với mọi số nguyên tố $p$ và số nguyên dương $k$, có thể tính $f(p^k)$ trong thời gian đa thức bậc thấp theo $k$, thì có thể sàng các giá trị $f(1),f(2),\dots,f(n)$ trong $O(n)$.

Giả sử phân tích thừa số nguyên tố của hợp số $n$ là $\prod_{i=1}^k p_i^{\alpha_i}$, trong đó $p_1<p_2<\dots<p_k$ là các số nguyên tố; ta lưu $g_n=p_1^{\alpha_1}$ trong sàng tuyến tính. Nếu $n$ bị sàng bởi $x\cdot p$ (với $p$ là số nguyên tố), thì $g$ thỏa mãn:

$$
g_n=
\begin{cases}
    g_x\cdot p & x\bmod p=0\\\\
    p & \text{ngược lại}
\end{cases}
$$

Nếu $n=g_n$ thì $n$ là lũy thừa của một số nguyên tố, có thể tính $f(n)$ trong $O(1)$; ngược lại, $f(n)=f(\frac{n}{g_n})\cdot f(g_n)$.

**Một phần nội dung của mục này được dịch từ bài viết [Решето Эратосфена](http://e-maxx.ru/algo/eratosthenes_sieve) và bản dịch tiếng Anh [Sieve of Eratosthenes](https://cp-algorithms.com/algebra/sieve-of-eratosthenes.html). Bản tiếng Nga có giấy phép Public Domain + Leave a Link; bản tiếng Anh có giấy phép CC-BY-SA 4.0.**
