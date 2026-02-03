author: AntiLeaf

Thuật toán Berlekamp–Massey dùng để tìm truy hồi tuyến tính ngắn nhất của một dãy. Cho dãy dài $n$, nếu truy hồi ngắn nhất có bậc $m$, thì Berlekamp–Massey tìm truy hồi ngắn nhất cho mọi tiền tố trong $O(nm)$. Trường hợp xấu nhất $m = O(n)$ nên độ phức tạp xấu nhất là $O(n^2)$.

### Định nghĩa

Gọi truy hồi của dãy $\{a_0 \dots a_{n - 1} \}$ là dãy $\{r_0\dots r_m\}$ thỏa:

$\sum_{j = 0} ^ m r_j a_{i - j} = 0, \forall i \ge m$

với $r_0 = 1$. $m$ là **bậc** của truy hồi.

Truy hồi ngắn nhất của $\{a_i\}$ là truy hồi có bậc nhỏ nhất.

### Cách làm

Khác với định nghĩa trên, ta dùng hệ số $\{f_0 \dots f_{m - 1}\}$ sao cho:

$a_i = \sum_{j = 0} ^ {m - 1} f_j a_{i - j - 1}, \forall i \ge m$

Dễ thấy $f_i = -r_{i + 1}$ và bậc $m$ không đổi.

Ta tìm truy hồi gia tăng, lần lượt xét từng phần tử của $\{a_i\}$, và khi truy hồi sai thì điều chỉnh $\{f_i\}$. Gọi truy hồi ngắn nhất của $i$ phần tử đầu là $F_i = \{f_{i, j}\}$.

Ban đầu $F_0 = \{\}$. Giả sử $F_{i - 1}$ đúng với $i-1$ phần tử đầu, thì với phần tử thứ $i$ có hai trường hợp:

1.  Truy hồi đúng với $a_i$, không cần chỉnh, đặt $F_i = F_{i - 1}$.
2.  Truy hồi sai với $a_i$, cần điều chỉnh $F_{i - 1}$ thành $F_i$.

Đặt $\Delta_i = a_i - \sum_{j = 0} ^ m f_{i - 1, j} a_{i - j - 1}$, tức sai lệch giữa $a_i$ và giá trị suy ra từ $F_{i - 1}$.

Nếu đây là lần đầu chỉnh, thì $a_i$ là phần tử khác 0 đầu tiên. Khi đó đặt $F_i$ là $i$ số 0, rõ ràng là truy hồi ngắn nhất hợp lệ.

Ngược lại, gọi lần trước chỉnh sửa là ở $k$. Nếu tồn tại $G = \{g_0 \dots g_{m' - 1}\}$ sao cho:

$\sum_{j = 0} ^ {m' - 1} g_j a_{i' - j - 1} = 0, \forall i' \in [m', i)$

và $\sum_{j = 0} ^ {m' - 1} g_j a_{i - j - 1} = \Delta_i$, thì cộng từng vị trí $F_k$ với $G$ sẽ cho truy hồi hợp lệ $F_i$.

Xây dựng $G$ bằng:

$G = \{0, 0, \dots, 0, \frac{\Delta_i}{\Delta_k}, -\frac{\Delta_i}{\Delta_k}F_{k-1}\}$

trong đó có $i - k - 1$ số 0, và phần $-\frac{\Delta_i}{\Delta_k} F_{k-1}$ là nhân từng phần tử của $F_{k-1}$ với $-\frac{\Delta_i}{\Delta_k}$ rồi nối vào.

Khi đó $\sum_{j = 0} ^ {m' - 1} g_j a_{i - j - 1} = \Delta_k \frac{\Delta_i}{\Delta_k} = \Delta_i$, nên $G$ hợp lệ. Đặt $F_i = F_k + G$ (cộng theo từng vị trí).

Nếu cần dạng truy hồi ban đầu $\{r_i\}$, thì đổi dấu toàn bộ $\{f_j\}$ rồi chèn $r_0 = 1$ ở đầu.

Từ quy trình trên, nếu bậc truy hồi ngắn nhất là $m$ thì độ phức tạp là $O(nm)$; xấu nhất là $O(n^2)$.

Trong cài đặt, mỗi lần điều chỉnh chỉ cần truy hồi ở lần điều chỉnh trước $F_k$, nên nếu chỉ cần truy hồi toàn dãy thì chỉ cần lưu truy hồi hiện tại và lần trước, bộ nhớ $O(n)$.

??? note "Cài đặt tham khảo"
    ```cpp
    vector<int> berlekamp_massey(const vector<int> &a) {
      vector<int> v, last;  // v là đáp án, 0-based, p là modulo
      int k = -1, delta = 0;
    
      for (int i = 0; i < (int)a.size(); i++) {
        int tmp = 0;
        for (int j = 0; j < (int)v.size(); j++)
          tmp = (tmp + (long long)a[i - j - 1] * v[j]) % p;
    
        if (a[i] == tmp) continue;
    
        if (k < 0) {
          k = i;
          delta = (a[i] - tmp + p) % p;
          v = vector<int>(i + 1);
    
          continue;
        }
    
        vector<int> u = v;
        int val = (long long)(a[i] - tmp + p) * power(delta, p - 2) % p;
    
        if (v.size() < last.size() + i - k) v.resize(last.size() + i - k);
    
        (v[i - k - 1] += val) %= p;
    
        for (int j = 0; j < (int)last.size(); j++) {
          v[i - k + j] = (v[i - k + j] - (long long)val * last[j]) % p;
          if (v[i - k + j] < 0) v[i - k + j] += p;
        }
    
        if ((int)u.size() - i < (int)last.size() - k) {
          last = u;
          k = i;
          delta = a[i] - tmp;
          if (delta < 0) delta += p;
        }
      }
    
      for (auto &x : v) x = (p - x) % p;
      v.insert(v.begin(), 1);
    
      return v;  // $\forall i, \sum_{j = 0} ^ m a_{i - j} v_j = 0$
    }
    ```

Thuật toán Berlekamp–Massey cơ bản tìm truy hồi ngắn nhất của dãy hữu hạn. Nếu dãy là vô hạn nhưng biết trước cận trên bậc truy hồi, chỉ cần lấy $2m$ phần tử đầu để tìm truy hồi của toàn dãy. (Chứng minh bỏ qua)

### Ứng dụng

Do tính ổn định số kém, thuật toán ít dùng với số thực. Để đơn giản, giả sử làm việc trong trường $p$ là số nguyên tố.

#### Tìm truy hồi ngắn nhất của dãy vector hoặc dãy ma trận

Với dãy vector $\boldsymbol{v}_i$, giả sử chiều là $n$, ta chọn ngẫu nhiên vector hàng $\mathbf u^T$, rồi tính dãy vô hướng $\{\boldsymbol{u}^T\boldsymbol{v}_i\}$. Theo Schwartz–Zippel, hai truy hồi trùng nhau với xác suất ít nhất $1 - \frac n p$.

Với dãy ma trận $\{A_i\}$ kích thước $n \times m$, chọn ngẫu nhiên $\mathbf u^T$ kích thước $1 \times n$ và $\boldsymbol{v}$ kích thước $m \times 1$, rồi xét $\{\boldsymbol{u}^T A_i \boldsymbol{v}\}$. Xác suất trùng nhau ít nhất $1 - \frac{n + m} p$.

#### Tối ưu lũy thừa nhanh ma trận

Cho $\boldsymbol{f}_i$ là vector cột $n$ chiều, thỏa $\boldsymbol{f}_i = A \boldsymbol{f}_{i - 1}$, thì $\{\boldsymbol{f}_i\}$ là dãy truy hồi tuyến tính bậc không quá $n$. (Chứng minh bỏ qua)

Ta có thể tính $\boldsymbol{f}_0 \dots \boldsymbol{f}_{2n - 1}$, tìm truy hồi ngắn nhất, rồi dùng [truy hồi tuyến tính hệ số hằng](./poly/linear-recurrence.md). Nếu cần $\boldsymbol{f}_m$, độ phức tạp $O(n^3 + n\log n \log m)$. Nếu $A$ là ma trận thưa có $k$ phần tử khác 0, độ phức tạp giảm còn $O(nk + n\log n \log m)$. Tuy nhiên do cần $O(nk)$ tiền xử lý, trong thực tế có thể dùng truy hồi tuyến tính $O(n^2 \log m)$, vẫn chấp nhận được.

#### Tìm đa thức tối thiểu của ma trận

Đa thức tối thiểu của ma trận vuông $A$ là đa thức bậc nhỏ nhất thỏa $f(A) = 0$.

Đa thức tối thiểu chính là truy hồi ngắn nhất của dãy $\{A^i\}$, nên có thể dùng Berlekamp–Massey. Với ma trận $n \times n$, bậc không quá $n$.

Nút thắt là tính $A^i$; nếu nhân ma trận trực tiếp thì $O(n^4)$. Nhưng khi tìm truy hồi, ta thực chất cần $\{\boldsymbol{u}^T A^i \boldsymbol{v}\}$, nên chỉ cần tính $A^i \boldsymbol{v}$.

Nếu $A$ có $k$ phần tử khác 0, độ phức tạp $O(kn + n^2)$.

#### Tính định thức ma trận thưa

Nếu có đa thức đặc trưng của $A$, hằng số tự do nhân $(-1)^n$ là định thức. Nhưng đa thức tối thiểu không nhất thiết là đa thức đặc trưng.

Nếu nhân $A$ với một ma trận đường chéo ngẫu nhiên $B$, thì $AB$ có xác suất ít nhất $1 - \frac {2n^2 - n} p$ để đa thức tối thiểu là đa thức đặc trưng. Sau đó chia cho $\det B$.

Với $A$ là ma trận $n \times n$ có $k$ phần tử khác 0, độ phức tạp $O(kn + n ^ 2)$.

#### Tìm hạng của ma trận thưa

Cho $A$ là ma trận $n\times m$. Chọn ngẫu nhiên ma trận đường chéo $P$ kích thước $n\times n$ và $Q$ kích thước $m\times m$, rồi tính đa thức tối thiểu của $Q A P A^T Q$.

Không cần nhân ma trận trực tiếp; khi cần nhân $Q A P A^T Q$ với vector, chỉ việc nhân lần lượt. Đáp án là bậc còn lại sau khi bỏ các nhân tử $x$ khỏi đa thức tối thiểu.

Nếu $A$ có $k$ phần tử khác 0 và $n \le m$, độ phức tạp $O(kn + n ^ 2)$.

#### Giải hệ phương trình thưa

**Bài toán**: $A \mathbf x = \mathbf b$, trong đó $A$ là ma trận thưa **hạng đầy đủ** $n \times n$, $\mathbf b$ và $\mathbf x$ là vector cột $1\times n$. Biết $A,\mathbf b$, cần tìm $\mathbf x$ với độ phức tạp nhỏ hơn $n^\omega$.

**Cách làm**: $\mathbf x = A^{-1} \mathbf b$. Nếu tìm được truy hồi ngắn nhất của $\{A^i \mathbf b\}$ là $\{r_0 \dots r_{m - 1}\}$ ($m \le n$) thì

$A^{-1} \mathbf b = -\frac 1 {r_{m - 1}} \sum_{i = 0} ^ {m - 2} A^i \mathbf b r_{m - 2 - i}$

(Chứng minh bỏ qua)

Vì $A$ thưa, chỉ cần tính trực tiếp $\mathbf b \dots A^{2n - 1} \mathbf b$.

Nếu $A$ có $k$ phần tử khác 0, độ phức tạp $O(kn + n^2)$.

??? note "Cài đặt tham khảo"
    ```cpp
    vector<int> solve_sparse_equations(const vector<tuple<int, int, int>> &A,
                                       const vector<int> &b) {
      int n = (int)b.size();  // 0-based
    
      vector<vector<int>> f({b});
    
      for (int i = 1; i < 2 * n; i++) {
        vector<int> v(n);
        auto &u = f.back();
    
        for (auto [x, y, z] : A)  // [x, y, value]
          v[x] = (v[x] + (long long)u[y] * z) % p;
    
        f.push_back(v);
      }
    
      vector<int> w(n);
      mt19937 gen;
      for (auto &x : w) x = uniform_int_distribution<int>(1, p - 1)(gen);
    
      vector<int> a(2 * n);
      for (int i = 0; i < 2 * n; i++)
        for (int j = 0; j < n; j++) a[i] = (a[i] + (long long)f[i][j] * w[j]) % p;
    
      auto c = berlekamp_massey(a);
      int m = (int)c.size();
    
      vector<int> ans(n);
    
      for (int i = 0; i < m - 1; i++)
        for (int j = 0; j < n; j++)
          ans[j] = (ans[j] + (long long)c[m - 2 - i] * f[i][j]) % p;
    
      int inv = power(p - c[m - 1], p - 2);
    
      for (int i = 0; i < n; i++) ans[i] = (long long)ans[i] * inv % p;
    
      return ans;
    }
    ```

### Bài tập

1.  [LibreOJ #163. 高斯消元 2](https://loj.ac/p/163)
2.  [ICPC2021 台北 Gym103443E. Composition with Large Red Plane, Yellow, Black, Gray, and Blue](https://codeforces.com/gym/103443/problem/E)
