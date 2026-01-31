本页面将简要介绍二分查找，由二分法衍生的三分法以及二分答案．

## 二分法

### 定义

**Tìm kiếm nhị phân** (tiếng Anh: binary search), còn gọi là tìm kiếm chia đôi (half-interval search) hoặc tìm kiếm theo logarit (logarithmic search), là một thuật toán dùng để tìm kiếm một phần tử trong một mảng đã được sắp xếp.

### Quy trình

Lấy ví dụ tìm kiếm một số trong mảng tăng dần.

Mỗi lần, thuật toán xét phần tử ở giữa đoạn hiện tại của mảng. Nếu phần tử giữa đúng là giá trị cần tìm, dừng lại. Nếu phần tử giữa nhỏ hơn giá trị cần tìm, thì các phần tử bên trái đều nhỏ hơn, chỉ cần tiếp tục tìm ở bên phải. Nếu phần tử giữa lớn hơn giá trị cần tìm thì chỉ cần tìm ở bên trái.

### Tính chất

#### Độ phức tạp thời gian

Trường hợp tốt nhất của tìm kiếm nhị phân là $O(1)$.

Trung bình và tệ nhất đều là $O(\log n)$, vì mỗi lần tìm kiếm đều chia đôi đoạn cần xét, nên với mảng có $n$ phần tử, tối đa cần $O(\log n)$ lần kiểm tra.

#### Độ phức tạp bộ nhớ

Phiên bản lặp của tìm kiếm nhị phân có độ phức tạp bộ nhớ $O(1)$.

Phiên bản đệ quy (không tối ưu đuôi) có độ phức tạp bộ nhớ $O(\log n)$.

### Cài đặt

```cpp
int binary_search(int start, int end, int key) {
  int ret = -1;  // Nếu không tìm thấy trả về -1
  int mid;
  while (start <= end) {
    mid = start + ((end - start) >> 1);  // Tránh tràn số khi tính trung bình
    if (arr[mid] < key)
      start = mid + 1;
    else if (arr[mid] > key)
      end = mid - 1;
    else {  // Kiểm tra bằng nhau cuối cùng vì đa số trường hợp là lớn hơn hoặc nhỏ hơn
      ret = mid;
      break;
    }
  }
  return ret;  // Chỉ có một điểm trả về
}
```

???+ note "Note"
    Tham khảo [Tối ưu biên dịch #Dùng dịch chuyển thay phép chia](../lang/optimizations.md#移位代替乘法), với $n$ là số nguyên không âm, `n >> 1` nhanh hơn `n / 2`.

### Tối ưu hóa giá trị lớn nhất nhỏ nhất

Lưu ý, "có thứ tự" ở đây là theo nghĩa rộng: nếu một phía của mảng đều thỏa mãn một điều kiện nào đó, còn phía còn lại thì không, thì cũng coi là có thứ tự (nếu coi thỏa mãn là $1$, không thỏa mãn là $0$, thì theo chiều này mảng là đơn điệu). Nói cách khác, tìm kiếm nhị phân có thể dùng để tìm giá trị lớn nhất (hoặc nhỏ nhất) thỏa mãn một điều kiện nào đó.

Để tìm giá trị lớn nhất nhỏ nhất thỏa mãn điều kiện, ý tưởng là duyệt từ nhỏ đến lớn giá trị "đáp án", kiểm tra hợp lệ. Nếu đáp án có tính đơn điệu, có thể dùng tìm kiếm nhị phân để tăng tốc. Để áp dụng được, cần:

1.  Đáp án nằm trong một đoạn cố định;
2.  Có thể kiểm tra nhanh một giá trị có hợp lệ không (không nhất thiết phải tìm ra giá trị đó);
3.  Tập hợp các giá trị hợp lệ có tính đơn điệu: nếu $x$ hợp lệ thì $x+1$ hoặc $x-1$ cũng hợp lệ.

Tối ưu hóa giá trị nhỏ nhất lớn nhất cũng tương tự.

### Tìm kiếm nhị phân trong STL

Thư viện chuẩn C++ cung cấp các hàm [`std::lower_bound`](https://zh.cppreference.com/w/cpp/algorithm/lower_bound) (tìm phần tử đầu tiên không nhỏ hơn giá trị cho trước) và [`std::upper_bound`](https://zh.cppreference.com/w/cpp/algorithm/upper_bound) (tìm phần tử đầu tiên lớn hơn giá trị cho trước), đều trong `<algorithm>`.

Cả hai đều dùng tìm kiếm nhị phân, nên mảng phải được sắp xếp trước khi gọi.

### bsearch

Hàm bsearch là phiên bản tìm kiếm nhị phân trong thư viện chuẩn C, định nghĩa trong `<stdlib.h>`. Trong C++, hàm này nằm trong `<cstdlib>`. qsort và bsearch là hai hàm thuật toán duy nhất của C.

So với qsort ([STL sắp xếp](./stl-sort.md)), bsearch có thêm tham số đầu là "địa chỉ phần tử cần tìm". Truyền địa chỉ giúp dùng lại hàm so sánh của qsort. Vì vậy, không truyền trực tiếp giá trị mà phải lưu vào biến rồi truyền địa chỉ.

bsearch có 5 tham số: địa chỉ phần tử cần tìm, tên mảng, số phần tử, kích thước phần tử, hàm so sánh. Hàm so sánh giống như qsort, xem [STL sắp xếp](./stl-sort.md).

Kết quả trả về là địa chỉ phần tử tìm được, kiểu void.

Lưu ý: bsearch khác lower\_bound và upper\_bound ở hai điểm:

-   Nếu có nhiều phần tử trùng nhau, bsearch trả về phần tử đầu tiên tìm thấy, có thể nằm giữa dãy các phần tử trùng.
-   Nếu không tìm thấy, trả về NULL.

Dùng lower\_bound có thể thay thế hoàn toàn bsearch, nên các bài dùng bsearch có thể chuyển sang lower\_bound. Tuy nhiên, do điểm khác biệt thứ hai, ví dụ tìm 3 trong dãy 1,2,4,5,6, thì bsearch không dễ mô phỏng lower\_bound.

Dùng bsearch để mô phỏng lower\_bound là khó, nhưng không phải không làm được. Nhờ đặc điểm truyền tham số của hàm so sánh (tham số đầu là phần tử cần tìm, tham số hai là phần tử trong mảng), có thể dùng bsearch để mô phỏng lower\_bound và upper\_bound như ví dụ dưới đây. Tuy nhiên, mảng cần là biến toàn cục để truyền địa chỉ đầu mảng.

```cpp
int A[100005];  // Mảng toàn cục ví dụ

// Tìm địa chỉ phần tử đầu tiên không nhỏ hơn phần tử cần tìm
int lower(const void *p1, const void *p2) {
  int *a = (int *)p1;
  int *b = (int *)p2;
  if ((b == A || compare(a, b - 1) > 0) && compare(a, b) > 0)
    return 1;
  else if (b != A && compare(a, b - 1) <= 0)
    return -1;  // Dùng phép trừ địa chỉ nên phải xác định kiểu phần tử
  else
    return 0;
}

// Tìm địa chỉ phần tử đầu tiên lớn hơn phần tử cần tìm
int upper(const void *p1, const void *p2) {
  int *a = (int *)p1;
  int *b = (int *)p2;
  if ((b == A || compare(a, b - 1) >= 0) && compare(a, b) >= 0)
    return 1;
  else if (b != A && compare(a, b - 1) < 0)
    return -1;  // Dùng phép trừ địa chỉ nên phải xác định kiểu phần tử
  else
    return 0;
}
```

Hiện nay rất ít bạn OI viết C thuần, và cách này cũng ít dùng, nên không cần chú trọng. Người mới nên dùng lower\_bound và upper\_bound trong C++.

### Nhị phân hóa đáp án

Khi giải bài toán, đôi khi ta cần duyệt các giá trị đáp án rồi kiểm tra hợp lệ. Nếu đáp án có tính đơn điệu, có thể dùng tìm kiếm nhị phân để tăng tốc, gọi là "nhị phân hóa đáp án".

???+ note "[Luogu P1873 Chặt cây](https://www.luogu.com.cn/problem/P1873)"
    Một người thợ rừng cần chặt đủ $M$ mét gỗ. Anh ta có một máy cưa, mỗi lần có thể cắt tất cả các cây cao hơn $H$ mét (các cây thấp hơn giữ nguyên). Hỏi nên đặt lưỡi cưa ở độ cao lớn nhất là bao nhiêu để vẫn đủ $M$ mét gỗ (nếu nâng thêm 1 mét thì không đủ nữa).

    Ví dụ, dãy chiều cao là $20,~15,~10,~17$, đặt lưỡi cưa ở $15$ mét thì còn $15,~15,~10,~15$, lấy được $5$ mét từ cây 1 và $2$ mét từ cây 4, tổng $7$ mét.

??? note "Hướng dẫn giải"
    Có thể duyệt từ $1$ đến $10^9$ để tìm đáp án, nhưng quá chậm. Thay vào đó, nhị phân hóa đáp án trên đoạn $[1,~10^9]$, kiểm tra từng giá trị (thường dùng tham lam). **Đây chính là nhị phân hóa đáp án.**

??? note "Code tham khảo"
    ```cpp
    int a[1000005];
    int n, m;
    
    bool check(int k) {  // Kiểm tra đáp án, k là độ cao lưỡi cưa
      long long sum = 0;
      for (int i = 1; i <= n; i++)       // Kiểm tra từng cây
        if (a[i] > k)                    // Nếu cây cao hơn lưỡi cưa
          sum += (long long)(a[i] - k);  // Cộng thêm phần gỗ lấy được
      return sum >= m;                   // Đủ gỗ thì hợp lệ
    }
    
    int find() {
      int l = 1, r = 1e9 + 1;   // Đoạn [l, r), nên 10^9 phải +1
      while (l + 1 < r) {       // Khi hai đầu chưa kề nhau
        int mid = (l + r) / 2;  // Lấy giá trị giữa
        if (check(mid))         // Nếu hợp lệ
          l = mid;              // Tăng độ cao
        else
          r = mid;  // Ngược lại giảm độ cao
      }
      return l;  // Trả về giá trị bên trái
    }
    
    int main() {
      cin >> n >> m;
      for (int i = 1; i <= n; i++) cin >> a[i];
      cout << find();
      return 0;
    }
    ```
    
    Đọc xong bạn sẽ có hai thắc mắc:
    
    1.  Tại sao đoạn tìm kiếm là [l, r)?
    
        Vì khi tìm đến cuối cùng, sẽ như sau (lấy giá trị lớn nhất hợp lệ làm ví dụ):
    
        ![](./images/binary-final-1.svg)
    
        Sau đó sẽ
    
        ![](./images/binary-final-2.svg)
    
        Trường hợp nhỏ nhất hợp lệ thì ngược lại.
    2.  Tại sao trả về giá trị bên trái?
    
        Như trên.

## 三分法

### Giới thiệu

Tìm kiếm nhị phân có thể dùng để tìm gần đúng nghiệm của phương trình $f(x)=0$. Nếu muốn tìm cực trị của hàm đơn đỉnh (unimodal), thường dùng **tìm kiếm tam phân** (ternary search).

Với một hàm $f(x)$, nếu tồn tại $x^*$ sao cho $f(x)$ tăng đơn điệu với $x<x^*$ và giảm đơn điệu với $x>x^*$, thì $f(x)$ là hàm đơn đỉnh. Khi đó, $x^*$ là điểm cực đại, $f(x^*)$ là giá trị lớn nhất.

??? note "Tại sao không dùng đạo hàm để tìm cực trị?"
    Thực tế, nếu có thể tính đạo hàm, ta có thể dùng nhị phân để tìm nghiệm của đạo hàm (vì hàm đơn đỉnh thì đạo hàm chỉ có một nghiệm). Tuy nhiên, với một số hàm, việc tính đạo hàm rất phức tạp.

    Ngoài ra, có những bài toán mà hàm cần tìm cực trị là kết quả của nhiều hàm đơn điệu khác nhau, hoặc là hàm ghép/phức tạp, đạo hàm có thể là hàm từng đoạn hoặc không xác định tại một số điểm.

???+ warning "Lưu ý"
    Tam phân có thể dùng để tìm cực đại của hàm đơn đỉnh hoặc cực tiểu của hàm đơn "thung lũng". Nếu không nói rõ, dưới đây mặc định là tìm cực đại.

### Quy trình

Tìm kiếm tam phân giống nhị phân, nhưng mỗi lần chọn hai điểm $lmid < rmid$ trong đoạn $[l, r]$. Nếu $f(lmid)<f(rmid)$, thì đoạn $[l, lmid)$ chắc chắn không chứa cực đại, có thể loại bỏ. Ngược lại, loại bỏ đoạn $[rmid, r]$. Không nhất thiết phải lấy hai điểm chia ba đều, nhưng nên chọn sao cho loại bỏ được nhiều nhất.

![](images/ternary.svg)

Việc chọn $lmid, rmid$ không ảnh hưởng đến tính đúng đắn, nhưng ảnh hưởng đến hiệu quả. Thường lấy hai điểm chia ba đều, hoặc lấy $mid-\varepsilon$ và $mid+\varepsilon$ quanh trung điểm.

### Cài đặt

Giả mã như sau:

$$
\begin{array}{l}
\textbf{Algorithm}\operatorname{TernarySearch}(f,l,r):\\
\textbf{Input. } \text{A unimodal function } f(x) \text{ and its domain } [l,r].  \\
\textbf{Output. } \text{The maximizer }x^*\text{, up to an error of }\varepsilon\text{, and its value } f(x^*). \\
\textbf{Method. } \\
\begin{array}{ll}
1 & \textbf{while } r - l > \varepsilon\\
2 & \qquad mid\gets (l+r)/2\\
3 & \qquad lmid\gets mid - \varepsilon \\
4 & \qquad rmid\gets mid + \varepsilon \\
5 & \qquad \textbf{if } f(lmid) < f(rmid) \\
6 & \qquad \qquad l\gets lmid \\
7 & \qquad \textbf{else } \\
8 & \qquad \qquad r\gets rmid \\
9 & x^* \gets (l+r)/2 \\
10& \textbf{return } x^*,~ f(x^*)
\end{array}
\end{array}
$$

???+ info "Trường hợp nguyên"
    Nếu hàm $f(x)$ xác định trên tập số nguyên, khi $r-l$ nhỏ thì nên dừng tam phân và duyệt vét cạn để tìm cực trị.

### Tối ưu: Phép chia vàng (Golden-section search)

Nếu mỗi lần gọi $f(x)$ tốn nhiều thời gian, có thể dùng **phép chia vàng** (golden-section search) để giảm số lần gọi hàm. Đây cũng là một phần quan trọng trong "phương pháp tối ưu hóa" của Hoa Lạc Canh.

Tam phân mỗi vòng cần 2 lần gọi hàm, mỗi vòng giảm độ dài đoạn tối đa còn $1/2$. Để đạt sai số $\varepsilon$, cần ít nhất

$$
2\log_2\dfrac{r-l}{\varepsilon}
$$

lần gọi hàm. Nếu chọn điểm chia khác, số lần gọi hàm còn nhiều hơn.

Phép chia vàng tận dụng việc tái sử dụng giá trị đã tính. Ngoài vòng đầu tiên cần 2 lần gọi hàm, các vòng sau chỉ cần 1 lần. Đặt tỉ lệ vàng là

$$
\phi = \dfrac{\sqrt{5}-1}{2} \approx 0.618.
$$

Mỗi vòng, chọn hai điểm chia vàng:

$$
m^l = \phi l +(1-\phi)r,~m^r = (1-\phi)l+\phi r.
$$

Điểm chia vàng có tính tự đồng dạng: $m^l$ là điểm chia vàng trái của $[l,r]$, cũng là điểm chia vàng phải của $[l,m^r]$. Nhờ đó, mỗi vòng sau có thể tái sử dụng một giá trị đã tính.

![](./images/golden-section-search.svg)

Như vậy, để đạt sai số $\varepsilon$, chỉ cần

$$
1 + \log_{\phi^{-1}}\dfrac{r-l}{\varepsilon} \approx 1 + 1.44\log_2\dfrac{r-l}{\varepsilon}
$$

lần gọi hàm, ít hơn tam phân.

Giả mã như sau:

$$
\begin{array}{l}
\textbf{Algorithm}\operatorname{GoldenSectionSearch}(f,l,r):\\
\textbf{Input. } \text{A unimodal function } f(x) \text{ and its domain } [l,r].  \\
\textbf{Output. } \text{The maximizer }x^*\text{, up to an error of }\varepsilon\text{, and its value } f(x^*). \\
\textbf{Method. } \\
\begin{array}{ll}
1 & lmid \gets \phi l + (1-\phi)r \\
2 & rmid \gets (1-\phi)l + \phi r \\
3 & lval \gets f(lmid) \\
4 & rval \gets f(rmid) \\
5 & \textbf{while } r - l > \varepsilon \\
6 & \qquad \textbf{if } lval > rval \\
7 & \qquad \qquad r \gets rmid \\
8 & \qquad \qquad rmid \gets lmid \\
9 & \qquad \qquad rval \gets lval \\
10& \qquad \qquad lmid \gets \phi l + (1-\phi)r \\
11& \qquad \qquad lval \gets f(lmid) \\
12& \qquad \textbf{else} \\
13& \qquad \qquad l \gets lmid \\
14& \qquad \qquad lmid \gets rmid \\
15& \qquad \qquad lval \gets rval \\
16& \qquad \qquad rmid \gets (1-\phi)l + \phi r \\
17& \qquad \qquad rval \gets f(rmid) \\
18& x^* \gets (l+r)/2 \\
19& \textbf{return }x^*,~f(x^*)
\end{array}
\end{array}
$$

### Bài tập ví dụ

???+ note "[洛谷 P3382 - Tam phân](https://www.luogu.com.cn/problem/P3382)"
    Cho một đa thức bậc $N$ và đoạn $[l, r]$, hãy tìm giá trị $x$ duy nhất sao cho hàm số tăng đơn điệu trên $[l, x]$ và giảm đơn điệu trên $[x, r]$.

??? note "Hướng dẫn giải"
    Bài này yêu cầu tìm giá trị $x$ tại đó đa thức bậc $N$ đạt cực đại trên $[l, r]$, có thể dùng tam phân.

??? note "Code tham khảo"
    === "C++"
        ```cpp
        --8<-- "docs/basic/code/binary/binary_1.cpp"
        ```
    
    === "Python"
        ```python
        --8<-- "docs/basic/code/binary/binary_1.py"
        ```

### Bài luyện tập

-   [UVa 1476 - Error Curves](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=447&page=show_problem&problem=4222)
-   [UVa 10385 - Duathlon](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=15&page=show_problem&problem=1326)
-   [UOJ 162 -【Tsinghua Training 2015】Kiểm tra bóng đèn](https://uoj.ac/problem/162)
-   [洛谷 P7579 -「RdOI R2」Cân nặng (weigh)](https://www.luogu.com.cn/problem/P7579)

## Quy hoạch phân số

Xem thêm: [Quy hoạch phân số](../misc/frac-programming.md)

Quy hoạch phân số thường mô tả các bài toán: mỗi vật có hai thuộc tính $c_i$, $d_i$, yêu cầu chọn một số vật sao cho $\frac{\sum{c_i}}{\sum{d_i}}$ lớn nhất hoặc nhỏ nhất.

Các ví dụ kinh điển: chu trình tỷ số tối ưu, cây khung tỷ số tối ưu, v.v.

Quy hoạch phân số có thể giải bằng tìm kiếm nhị phân.

## Tài liệu tham khảo

-   [Ternary search - Wikipedia](https://en.wikipedia.org/wiki/Ternary_search)
-   [Golden-section search - Wikipedia](https://en.wikipedia.org/wiki/Golden-section_search)
-   [Ternary search - CP Algortihms](https://cp-algorithms.com/num_methods/ternary_search.html)
