author: ChungZH, Yukimaikoriya, tigerruanyifan, isdanni, Saisyc, 383494, Tiphereth-A, XuYueming520

## Giới thiệu

**Biến đổi số học** (number-theoretic transform, NTT) là hiện thực của biến đổi Fourier rời rạc (DFT) trên cơ sở số học; **biến đổi số học nhanh** (fast number-theoretic transform, FNTT) là hiện thực của [FFT](./fft.md) trên cơ sở số học.

**Biến đổi số học** là một thuật toán nhanh để tính tích chập (convolution). Thuật toán thường dùng nhất gồm cả FFT. Tuy nhiên FFT có một số bất lợi trong cài đặt: vector dữ liệu phải nhân với ma trận hệ số phức, mỗi hệ số phức có phần thực và ảo là hàm sin/cos, nên hầu hết hệ số là số thực dấu phẩy động. Do đó phải thực hiện phép toán phức dấu phẩy động, tốn chi phí và sai số lớn.

NTT giải quyết phép nhân đa thức có mod, chịu ràng buộc bởi mô-đun và giá trị cũng lớn. Mô-đun thường gặp nhất là 998244353.

## Kiến thức trước

Học NTT cần kiến thức: DFT, nhóm con sinh, [căn nguyên thủy](../number-theory/primitive-root.md), log rời rạc. Xem trang tương ứng.

## Định nghĩa

### Biến đổi số học

Trong toán học, NTT là DFT trên một [vành](../algebra/basic.md#环). Trong trường hữu hạn, thường gọi là NTT.

NTT là cách đưa DFT về $F={\mathbb {Z}/p}$, với $p$ là số nguyên tố. Đây là **trường hữu hạn**; nếu $n$ chia hết $p-1$ thì tồn tại căn nguyên thủy bậc $n$, nên $p=\xi n+1$ với $ξ$ là số nguyên dương. Cụ thể, với $p=qn+1,(n=2^m)$, căn nguyên thủy $g$ thỏa $g^{qn} \equiv 1 \pmod p$, đặt $g_n=g^q\pmod p$ làm tương đương $\omega_n$, thì có tính chất tương tự: $g_n^n \equiv 1 \pmod p, g_n^{n/2} \equiv -1 \pmod p$.

Vì liên quan số học nên $N$ (để phân biệt với $n$ trong FFT, ở đây gọi là $N$) có thể lớn hơn $n$ trong FFT, nhưng chỉ cần coi $\frac{qN}{n}$ là $q$ ở đây để tránh vấn đề kích thước.

Các mô-đun thường gặp:

$$
p = 167772161 = 5 \times 2^{25}+1, g=3
$$

$$
p = 469762049 = 7 \times 2^{26}+1, g=3
$$

$$
p = 754974721 = 3^2 \times 5 \times 2^{24}+1, g=11
$$

$$
p = 998244353 = 7 \times 17 \times 2^{23}+1, g=3
$$

$$
p = 1004535809 = 479 \times 2^{21}+1, g=3
$$

Đó chính là tương đương của $g^{qn}$ với $\mathrm{e}^{2\pi \mathrm{i} n}$.

Khi lặp ở độ dài $l$ thì $g_l = g^{\frac{p-1}{l}}$, hoặc $\omega_n = g_l = g_N^{\frac{N}{l}} = g_N^{\frac{p-1}{l}}$.

## Biến đổi số học nhanh

**Biến đổi số học nhanh** (FNTT) là NTT cộng thêm chia để trị.

FNTT dùng phương pháp chia để trị giống FFT. Điều này nghĩa là chỉ cần sửa nhẹ từ mã FFT là có mã FNTT.

Trong thi đấu, từ NTT thường thực chất chỉ FNTT; mặc định “NTT” là “FNTT”.

Cách rút gọn này tương tự FFT. Thực ra “FFT” là “FDFT”, nhưng do “nhanh” chỉ áp dụng cho rời rạc (đặc biệt với bậc là lũy thừa của 2), nên chữ “rời rạc” được lược bỏ.

NTT/FNTT làm việc trong modulo, không có trường hợp liên tục nên không cần chữ “rời rạc”.

Trong thuật toán, không tăng tốc thì vô nghĩa. Việc giới thiệu DFT trong bài FFT là do DFT có ứng dụng khác và là nguyên lý của FFT.

Khi không gây nhầm lẫn, dùng NTT chỉ FNTT. Để tránh nhầm lẫn, phần sau tách NTT và FNTT.

Quan hệ giữa DFT, FFT, NTT, FNTT:

-   Từ DFT và NTT, thêm chia để trị được FFT và FNTT. Xem bài FFT.
-   Từ DFT và FFT, thay phép cộng/nhân phức bằng cộng/nhân modulo $p$; thay căn đơn vị nguyên thủy bằng căn nguyên thủy modulo $p$ cùng bậc, bậc là lũy thừa của 2, thì được NTT và FNTT.

Vì chỉ thay phép cộng và nhân, DFT/FFT/NTT/FNTT có cùng nguyên lý, đều làm việc trên vành có cộng/nhân, không cần chia.

Thực tế chỉ cần có căn nguyên thủy (phần tử sinh của nhóm), thì NTT/FNTT dưới mô-đun đó thực hiện được. Với mô-đun $1,2,4$ quá nhỏ nên không ý nghĩa. Với số nguyên tố lẻ $p$ và số nguyên dương $\alpha$, chỉ cần có căn nguyên thủy dưới mô-đun $p^\alpha$ hoặc $2p^\alpha$ là có thể làm tương tự.

## Mẫu code

Dưới đây là mẫu nhân số lớn, [nguồn](https://blog.csdn.net/blackjack_/article/details/79346433).

??? note "Mã tham khảo"
    ```cpp
    #include <algorithm>
    #include <bitset>
    #include <cmath>
    #include <cstdio>
    #include <cstdlib>
    #include <cstring>
    #include <ctime>
    #include <iomanip>
    #include <iostream>
    #include <map>
    #include <queue>
    #include <set>
    #include <string>
    #include <vector>
    using namespace std;
    
    int read() {
      int x = 0, f = 1;
      char ch = getchar();
      while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
      }
      while (ch <= '9' && ch >= '0') {
        x = 10 * x + ch - '0';
        ch = getchar();
      }
      return x * f;
    }
    
    void print(int x) {
      if (x < 0) putchar('-'), x = -x;
      if (x >= 10) print(x / 10);
      putchar(x % 10 + '0');
    }
    
    constexpr int N = 300100, P = 998244353;
    
    int qpow(int x, int y) {
      int res(1);
      while (y) {
        if (y & 1) res = 1ll * res * x % P;
        x = 1ll * x * x % P;
        y >>= 1;
      }
      return res;
    }
    
    int r[N];
    
    void ntt(int *x, int lim, int opt) {
      int i, j, k, m, gn, g, tmp;
      for (i = 0; i < lim; ++i)
        if (r[i] < i) swap(x[i], x[r[i]]);
      for (m = 2; m <= lim; m <<= 1) {
        k = m >> 1;
        gn = qpow(3, (P - 1) / m);
        for (i = 0; i < lim; i += m) {
          g = 1;
          for (j = 0; j < k; ++j, g = 1ll * g * gn % P) {
            tmp = 1ll * x[i + j + k] * g % P;
            x[i + j + k] = (x[i + j] - tmp + P) % P;
            x[i + j] = (x[i + j] + tmp) % P;
          }
        }
      }
      if (opt == -1) {
        reverse(x + 1, x + lim);
        int inv = qpow(lim, P - 2);
        for (i = 0; i < lim; ++i) x[i] = 1ll * x[i] * inv % P;
      }
    }
    
    int A[N], B[N], C[N];
    
    char a[N], b[N];
    
    int main() {
      int i, lim(1), n;
      scanf("%s", a);
      n = strlen(a);
      for (i = 0; i < n; ++i) A[i] = a[n - i - 1] - '0';
      while (lim < (n << 1)) lim <<= 1;
      scanf("%s", b);
      n = strlen(b);
      for (i = 0; i < n; ++i) B[i] = b[n - i - 1] - '0';
      while (lim < (n << 1)) lim <<= 1;
      for (i = 0; i < lim; ++i) r[i] = (i & 1) * (lim >> 1) + (r[i >> 1] >> 1);
      ntt(A, lim, 1);
      ntt(B, lim, 1);
      for (i = 0; i < lim; ++i) C[i] = 1ll * A[i] * B[i] % P;
      ntt(C, lim, -1);
      int len(0);
      for (i = 0; i < lim; ++i) {
        if (C[i] >= 10) len = i + 1, C[i + 1] += C[i] / 10, C[i] %= 10;
        if (C[i]) len = max(len, i);
      }
      while (C[len] >= 10) C[len + 1] += C[len] / 10, C[len] %= 10, len++;
      for (i = len; ~i; --i) putchar(C[i] + '0');
      puts("");
      return 0;
    }
    ```

## Tài liệu tham khảo và đọc thêm

1.  [FWT (Fast Walsh Transform) cho người mới](https://zhuanlan.zhihu.com/p/41867199)
2.  [FFT/NTT cho người mới](https://zhuanlan.zhihu.com/p/40505277)
3.  [Number-theoretic transform (NTT) - Wikipedia](https://en.wikipedia.org/wiki/Discrete_Fourier_transform_%28general%29#Number-theoretic_transform)
4.  [Tutorial on FFT/NTT—The tough made simple. (Part 1)](https://codeforces.com/blog/entry/43499)
