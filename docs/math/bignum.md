> Bản tóm tắt TL;DR: cuối bài tự lấy template…

## Định nghĩa

Tính toán độ chính xác tùy ý (Arbitrary-Precision Arithmetic), còn gọi là tính toán số nguyên lớn (bignum), sử dụng một số cấu trúc thuật toán để hỗ trợ các phép toán giữa những số nguyên rất lớn (độ lớn vượt quá kiểu số nguyên dựng sẵn của ngôn ngữ).

## Giới thiệu

Bài toán số học độ chính xác cao có rất nhiều chi tiết nhỏ, cách hiện thực cũng có nhiều điểm cần chú ý.

Vậy hôm nay hãy cùng nhau viết một chiếc máy tính đơn giản nhé.

???+ note "Nhiệm vụ"
    Input: một biểu thức dạng `a <op> b`.
    
    -   `a`, `b` là các số nguyên không âm hệ thập phân, độ dài không quá $1000$;
    -   `<op>` là một ký tự (`+`, `-`, `*` hoặc `/`), biểu thị phép toán;
    -   Số nguyên và toán tử cách nhau bởi một dấu cách.
    
    Output: kết quả phép toán.
    
    -   Với `+`, `-`, `*` in ra một dòng là kết quả;
    -   Với `/` in ra hai dòng lần lượt là thương và dư.
    -   Đảm bảo kết quả đều là số nguyên không âm.

## Lưu trữ

Trong cách cài đặt thông thường, số lớn được biểu diễn bằng chuỗi, mỗi ký tự là một chữ số thập phân. Vì vậy có thể nói tính toán số lớn thực chất là một dạng xử lý chuỗi đặc biệt.

Khi đọc chuỗi, chữ số cao nhất nằm ở đầu chuỗi (chỉ số nhỏ). Nhưng theo thói quen, ta lưu chữ số **thấp nhất** ở vị trí chỉ số nhỏ nhất, tức là lưu chuỗi đảo ngược. Lý do là độ dài số có thể thay đổi, nhưng ta muốn các vị trí cùng trọng số luôn thẳng hàng (ví dụ: mọi hàng đơn vị ở chỉ số `[0]`, mọi hàng chục ở chỉ số `[1]`…); đồng thời, phép cộng, trừ, nhân thường bắt đầu từ hàng đơn vị (nhớ phép tính đặt dọc ở tiểu học), nên “lưu đảo” rất hợp lý.

Từ đây về sau sẽ luôn dùng quy ước này. Định nghĩa hằng `LEN = 1004` là độ dài tối đa chương trình chứa được.

Từ đó dễ dàng viết mã đọc số lớn:

```cpp
void clear(int a[]) {
  for (int i = 0; i < LEN; ++i) a[i] = 0;
}

void read(int a[]) {
  static char s[LEN + 1];
  scanf("%s", s);

  clear(a);

  int len = strlen(s);
  // Như đã nói, đảo ngược
  for (int i = 0; i < len; ++i) a[len - i - 1] = s[i] - '0';
  // s[i] - '0' chính là chữ số tương ứng
  // Một số bạn quen hiểu theo ord(s[i]) - ord('0')
}
```

In ra cũng theo thứ tự đảo ngược. Vì không muốn in số 0 ở đầu, ta tìm từ vị trí cao nhất xuống chữ số khác 0 đầu tiên rồi in từ đó; điều kiện dừng `i >= 1` thay vì `i >= 0` là để khi toàn bộ số bằng $0$ vẫn in ra ký tự `0`.

```cpp
void print(int a[]) {
  int i;
  for (i = LEN - 1; i >= 1; --i)
    if (a[i] != 0) break;
  for (; i >= 0; --i) putchar(a[i] + '0');
  putchar('\n');
}
```

Ghép lại là một chương trình “nhại lại” hoàn chỉnh.

??? note "`copycat.cpp`"
    ```cpp
    #include <cstdio>
    #include <cstring>
    
    constexpr int LEN = 1004;
    
    int a[LEN];
    
    void clear(int a[]) {
      for (int i = 0; i < LEN; ++i) a[i] = 0;
    }
    
    void read(int a[]) {
      static char s[LEN + 1];
      scanf("%s", s);
    
      clear(a);
    
      int len = strlen(s);
      for (int i = 0; i < len; ++i) a[len - i - 1] = s[i] - '0';
    }
    
    void print(int a[]) {
      int i;
      for (i = LEN - 1; i >= 1; --i)
        if (a[i] != 0) break;
      for (; i >= 0; --i) putchar(a[i] + '0');
      putchar('\n');
    }
    
    int main() {
      read(a);
      print(a);
    
      return 0;
    }
    ```

## Bốn phép toán

Trong bốn phép toán, độ khó không giống nhau. Dễ nhất là cộng/trừ số lớn, tiếp theo là nhân số lớn với số thường (`int`) và nhân số lớn với số lớn, cuối cùng là chia số lớn với số lớn.

Chúng ta sẽ hiện thực theo thứ tự này.

### Cộng

Cộng số lớn thực chất là cộng đặt dọc.

![](./images/plus.svg)

Bắt đầu từ hàng thấp nhất, cộng từng cặp chữ số tương ứng rồi kiểm tra có đạt hoặc vượt $10$ không. Nếu có thì xử lý nhớ: tăng kết quả ở hàng cao hơn lên $1$, và giảm hàng hiện tại đi $10$.

```cpp
void add(int a[], int b[], int c[]) {
  clear(c);

  // Trong hiện thực số lớn, thường đặt LEN lớn hơn độ dài input
  // rồi bỏ qua vài vòng lặp cuối để giảm xử lý biên
  // Vì input tối đa 1000 chữ số, nên lặp đến LEN - 1 = 1003 là đủ
  for (int i = 0; i < LEN - 1; ++i) {
    // Cộng chữ số cùng vị trí
    c[i] += a[i] + b[i];
    if (c[i] >= 10) {
      // Nhớ
      c[i + 1] += 1;
      c[i] -= 10;
    }
  }
}
```

Kết hợp với phần trước, ta được máy tính cộng.

??? note "`adder.cpp`"
    ```cpp
    #include <cstdio>
    #include <cstring>
    
    constexpr int LEN = 1004;
    
    int a[LEN], b[LEN], c[LEN];
    
    void clear(int a[]) {
      for (int i = 0; i < LEN; ++i) a[i] = 0;
    }
    
    void read(int a[]) {
      static char s[LEN + 1];
      scanf("%s", s);
    
      clear(a);
    
      int len = strlen(s);
      for (int i = 0; i < len; ++i) a[len - i - 1] = s[i] - '0';
    }
    
    void print(int a[]) {
      int i;
      for (i = LEN - 1; i >= 1; --i)
        if (a[i] != 0) break;
      for (; i >= 0; --i) putchar(a[i] + '0');
      putchar('\n');
    }
    
    void add(int a[], int b[], int c[]) {
      clear(c);
    
      for (int i = 0; i < LEN - 1; ++i) {
        c[i] += a[i] + b[i];
        if (c[i] >= 10) {
          c[i + 1] += 1;
          c[i] -= 10;
        }
      }
    }
    
    int main() {
      read(a);
      read(b);
    
      add(a, b, c);
      print(c);
    
      return 0;
    }
    ```

### Trừ

Trừ số lớn chính là trừ đặt dọc.

![](./images/subtraction.svg)

Từ hàng đơn vị trừ dần lên; nếu âm thì mượn $1$ từ hàng cao hơn. Ý tưởng giống hệt cộng.

```cpp
void sub(int a[], int b[], int c[]) {
  clear(c);

  for (int i = 0; i < LEN - 1; ++i) {
    // Trừ từng vị trí
    c[i] += a[i] - b[i];
    if (c[i] < 0) {
      // Mượn
      c[i + 1] -= 1;
      c[i] += 10;
    }
  }
}
```

Thay `add()` bằng `sub()` là được máy tính trừ.

??? note "`subtractor.cpp`"
    ```cpp
    #include <cstdio>
    #include <cstring>
    
    constexpr int LEN = 1004;
    
    int a[LEN], b[LEN], c[LEN];
    
    void clear(int a[]) {
      for (int i = 0; i < LEN; ++i) a[i] = 0;
    }
    
    void read(int a[]) {
      static char s[LEN + 1];
      scanf("%s", s);
    
      clear(a);
    
      int len = strlen(s);
      for (int i = 0; i < len; ++i) a[len - i - 1] = s[i] - '0';
    }
    
    void print(int a[]) {
      int i;
      for (i = LEN - 1; i >= 1; --i)
        if (a[i] != 0) break;
      for (; i >= 0; --i) putchar(a[i] + '0');
      putchar('\n');
    }
    
    void sub(int a[], int b[], int c[]) {
      clear(c);
    
      for (int i = 0; i < LEN - 1; ++i) {
        c[i] += a[i] - b[i];
        if (c[i] < 0) {
          c[i + 1] -= 1;
          c[i] += 10;
        }
      }
    }
    
    int main() {
      read(a);
      read(b);
    
      sub(a, b, c);
      print(c);
    
      return 0;
    }
    ```

Thử nhập `1 2` —— kết quả `/9999999`, ủa **OI Wiki** sao lại cho code giả vậy…

Thực ra, đoạn code trên chỉ xử lý trường hợp số bị trừ $a$ lớn hơn hoặc bằng số trừ $b$. Khi $a<b$ thì rất đơn giản:

$a-b=-(b-a)$

Cần tính $b-a$ vì $b>a$ nên gọi `sub(b,a,c)`. Sau đó thêm dấu âm trước kết quả là được.

### Nhân

#### Số lớn — số thường

Nhân số lớn là… khoan đã!

Trước hết xét trường hợp một thừa số là `int` thường. Có cách đơn giản không?

Trực giác là nhân từng chữ số của $a$ với $b$. Về giá trị thì đúng, nhưng không đúng dạng thập phân nên cần chỉnh lại.

Cách chỉnh cũng xử lý nhớ từ thấp lên cao. Nhưng nhớ ở đây có thể rất lớn (tới cỡ $9b$), nên không thể chỉ trừ $10$; phải dùng thương và dư của phép chia cho $10$. Xem chú thích và hình dưới minh họa $1337 \times 42$.

![](./images/multiplication-short.png)

Vì lý do này cần chú ý phạm vi của $b$. Nếu $b$ cùng cỡ $10^9$ (giới hạn `int`), cần cẩn thận với cách nhân số lớn—số thường.

```cpp
void mul_short(int a[], int b, int c[]) {
  clear(c);

  for (int i = 0; i < LEN - 1; ++i) {
    // Nhân trực tiếp chữ số thứ i với b, cộng vào kết quả
    c[i] += a[i] * b;

    if (c[i] >= 10) {
      // Xử lý nhớ
      // c[i] / 10 là phần thương dùng làm tăng nhớ
      c[i + 1] += c[i] / 10;
      // c[i] % 10 là phần dư giữ lại ở vị trí hiện tại
      c[i] %= 10;
    }
  }
}
```

#### Số lớn — số lớn

Nếu cả hai là số lớn, ta dùng nhân đặt dọc.

Nhớ rằng mỗi bước thực chất tính các tổng $a \times b_i \times 10^i$. Ví dụ $1337 \times 42$ là $1337 \times 2 \times 10^0 + 1337 \times 4 \times 10^1$.

Vì vậy ta tách $b$ thành từng chữ số, nhân từng chữ số (số thường) với $a$, dịch trái đến đúng vị trí rồi cộng. Cuối cùng xử lý nhớ như trên.

![](./images/multiplication-long.png)

Lưu ý quá trình này hơi khác nhân đặt dọc: trong mỗi bước không xử lý nhớ, mà dồn lại rồi cuối cùng mới xử lý. Kết quả vẫn đúng.

```cpp
void mul(int a[], int b[], int c[]) {
  clear(c);

  for (int i = 0; i < LEN - 1; ++i) {
    // Trực tiếp tính chữ số thứ i của kết quả (từ thấp lên cao), kèm xử lý nhớ
    // Vòng lặp i cộng vào c[i] tất cả tích a[p]*b[q] với p+q=i
    // Hiệu quả tương đương nhân đặt dọc rồi cộng, nhưng gọn hơn
    for (int j = 0; j <= i; ++j) c[i] += a[j] * b[i - j];

    if (c[i] >= 10) {
      c[i + 1] += c[i] / 10;
      c[i] %= 10;
    }
  }
}
```

### Chia

Một cách thực hiện chia số lớn là phép chia đặt dọc.

![](./images/division.svg)

Chia đặt dọc có thể xem như quá trình trừ lặp. Ví dụ hàng chục của thương trong hình: lấy $45$ trừ $12$ ba lần vẫn còn $\ge 12$, lần thứ tư thì < $12$, nên chữ số là $3$.

Để giảm tính toán, ta biết trước độ dài bị chia $l_a$ và chia $l_b$, bắt đầu từ chỉ số $l_a-l_b$ và đi từ cao xuống thấp. Điều này giống việc căn thẳng chữ số cao nhất khi chia tay.

Hàm `greater_eq()` kiểm tra phần bị chia (từ `last_dg` trở lên) có đủ lớn để trừ tiếp chia hay không. Với mỗi chữ số thương, ta lặp kiểm tra, nếu được thì trừ chia khỏi dư (tức mô phỏng chia đặt dọc).

```cpp
// Kiểm tra bị chia a, lấy last_dg là chữ số thấp nhất, có thể trừ tiếp b mà vẫn không âm không
// len là độ dài của b để tránh tính lại
bool greater_eq(int a[], int b[], int last_dg, int len) {
  // Có thể phần còn lại dài hơn chia 1 chữ số, vậy kiểm tra vậy là đủ
  if (a[last_dg + len] != 0) return true;
  // So sánh từ cao xuống thấp
  for (int i = len - 1; i >= 0; --i) {
    if (a[last_dg + i] > b[i]) return true;
    if (a[last_dg + i] < b[i]) return false;
  }
  // Bằng nhau cũng trừ được
  return true;
}

void div(int a[], int b[], int c[], int d[]) {
  clear(c);
  clear(d);

  int la, lb;
  for (la = LEN - 1; la > 0; --la)
    if (a[la - 1] != 0) break;
  for (lb = LEN - 1; lb > 0; --lb)
    if (b[lb - 1] != 0) break;
  if (lb == 0) {  // Chia cho 0 không được
    puts("> <");
    return;
  }

  // c là thương
  // d là phần còn lại của bị chia, sau khi kết thúc chính là dư
  for (int i = 0; i < la; ++i) d[i] = a[i];
  for (int i = la - lb; i >= 0; --i) {
    // Tính chữ số thứ i của thương
    while (greater_eq(d, b, i, lb)) {
      // Nếu trừ được thì trừ
      // Đoạn này là phép trừ số lớn
      for (int j = 0; j < lb; ++j) {
        d[i + j] -= b[j];
        if (d[i + j] < 0) {
          d[i + j + 1] -= 1;
          d[i + j] += 10;
        }
      }
      // Tăng chữ số thương lên 1
      c[i] += 1;
      // Quay lại kiểm tra
    }
  }
}
```

## Hoàn thành phần nhập môn!

Kết hợp bốn phép toán trên là xong chương trình máy tính như đầu bài.

??? note "`calculator.cpp`"
    ```cpp
    #include <cstdio>
    #include <cstring>
    
    constexpr int LEN = 1004;
    
    int a[LEN], b[LEN], c[LEN], d[LEN];
    
    void clear(int a[]) {
      for (int i = 0; i < LEN; ++i) a[i] = 0;
    }
    
    void read(int a[]) {
      static char s[LEN + 1];
      scanf("%s", s);
    
      clear(a);
    
      int len = strlen(s);
      for (int i = 0; i < len; ++i) a[len - i - 1] = s[i] - '0';
    }
    
    void print(int a[]) {
      int i;
      for (i = LEN - 1; i >= 1; --i)
        if (a[i] != 0) break;
      for (; i >= 0; --i) putchar(a[i] + '0');
      putchar('\n');
    }
    
    void add(int a[], int b[], int c[]) {
      clear(c);
    
      for (int i = 0; i < LEN - 1; ++i) {
        c[i] += a[i] + b[i];
        if (c[i] >= 10) {
          c[i + 1] += 1;
          c[i] -= 10;
        }
      }
    }
    
    void sub(int a[], int b[], int c[]) {
      clear(c);
    
      for (int i = 0; i < LEN - 1; ++i) {
        c[i] += a[i] - b[i];
        if (c[i] < 0) {
          c[i + 1] -= 1;
          c[i] += 10;
        }
      }
    }
    
    void mul(int a[], int b[], int c[]) {
      clear(c);
    
      for (int i = 0; i < LEN - 1; ++i) {
        for (int j = 0; j <= i; ++j) c[i] += a[j] * b[i - j];
    
        if (c[i] >= 10) {
          c[i + 1] += c[i] / 10;
          c[i] %= 10;
        }
      }
    }
    
    bool greater_eq(int a[], int b[], int last_dg, int len) {
      if (a[last_dg + len] != 0) return true;
      for (int i = len - 1; i >= 0; --i) {
        if (a[last_dg + i] > b[i]) return true;
        if (a[last_dg + i] < b[i]) return false;
      }
      return true;
    }
    
    void div(int a[], int b[], int c[], int d[]) {
      clear(c);
      clear(d);
    
      int la, lb;
      for (la = LEN - 1; la > 0; --la)
        if (a[la - 1] != 0) break;
      for (lb = LEN - 1; lb > 0; --lb)
        if (b[lb - 1] != 0) break;
      if (lb == 0) {
        puts("> <");
        return;
      }
    
      for (int i = 0; i < la; ++i) d[i] = a[i];
      for (int i = la - lb; i >= 0; --i) {
        while (greater_eq(d, b, i, lb)) {
          for (int j = 0; j < lb; ++j) {
            d[i + j] -= b[j];
            if (d[i + j] < 0) {
              d[i + j + 1] -= 1;
              d[i + j] += 10;
            }
          }
          c[i] += 1;
        }
      }
    }
    
    int main() {
      read(a);
    
      char op[4];
      scanf("%s", op);
    
      read(b);
    
      switch (op[0]) {
        case '+':
          add(a, b, c);
          print(c);
          break;
        case '-':
          sub(a, b, c);
          print(c);
          break;
        case '*':
          mul(a, b, c);
          print(c);
          break;
        case '/':
          div(a, b, c, d);
          print(c);
          print(d);
          break;
        default:
          puts("> <");
      }
    
      return 0;
    }
    ```

## Số lớn ép cơ số (压位高精度)

### Giới thiệu

Trong cộng, trừ, nhân số lớn, ta tách số thành từng chữ số rồi tính.

Ví dụ $8192\times 42$ thì thực chất là $(8000+100+90+2)\times(40+2)$.

Khi số có nhiều chữ số, số phần tử tách ra rất nhiều, hiệu năng giảm.

Có thể tối ưu không?

Nhận thấy cách tách không ảnh hưởng kết quả, nên có thể gộp nhiều chữ số lại thành một “chữ số” lớn.

### Quá trình

Với ví dụ trên, nếu mỗi “chữ số” gồm 2 chữ số thập phân thì ta tách thành $(8100+92)\times 42$.

Không ảnh hưởng kết quả, nhưng số phần tử giảm nên nhanh hơn.

Từ góc nhìn [hệ cơ số](./base.md), ta thực hiện phép toán trong cơ số lớn hơn (mỗi 2 chữ số tương đương cơ số $100$), giúp giảm số “chữ số” tham gia tính toán.

Đó là ý tưởng **ép cơ số**.

Dưới đây là cộng trong ép cơ số:

??? note "Cài đặt tham khảo cho cộng ép cơ số"
    ```cpp
    // Các mảng a,b,c đều ở cơ số p
    // Khi xuất kết quả cần đổi về thập phân
    void add(int a[], int b[], int c[]) {
      clear(c);
    
      for (int i = 0; i < LEN - 1; ++i) {
        c[i] += a[i] + b[i];
        if (c[i] >= p) {  // Với số lớn thường thì p=10
          c[i + 1] += 1;
          c[i] -= p;
        }
      }
    }
    ```

### Chia đặt dọc hiệu quả trong ép cơ số

Trong ép cơ số, nếu vẫn thử thương theo cách cũ thì số lần thử rất nhiều. Ví dụ cơ số 10000 thì trung bình mỗi vị trí thử ~5000 lần, không chấp nhận được. Vì vậy cần cách ước lượng thương hiệu quả hơn.

Có thể dùng double làm trung gian. Giả sử bị chia có 4 chữ số $a_4,a_3,a_2,a_1$, chia có 3 chữ số $b_3,b_2,b_1$, thì chỉ cần ước lượng một chữ số thương bằng:

$\dfrac{a_4 base + a_3}{b_3 + b_2 base^{-1} + (b_1+1)base^{-2}}$.

Với nhiều chữ số, lặp lại cách ước lượng một chữ số. Vì dùng 3 chữ số của chia để ước lượng, ta đảm bảo thương ước lượng $q'$ và thương thực $q$ thỏa $q-1 \le q' \le q$. Khi đó mỗi vị trí nhiều nhất thử 2 lần. Tuy nhiên yêu cầu $base^3 < 2^{53}$ (độ chính xác double), nên khuyến nghị không vượt 32768 cơ số, nếu không dễ sai do lỗi chính xác.

Ngoài ra, vì ước lượng luôn $\le$ thương thực, còn có thể tối ưu: đa số trường hợp chỉ ước lượng một lần. Khi chuyển sang chữ số tiếp theo, thương có thể vượt base do sai số trước đó, nhưng chỉ cần dồn xử lý nhớ cuối cùng. Ví dụ base=10, tính $395081/9876$:

1.  Ước lượng $3950/988=3$, nên $395081-(9876 \times 3 \times 10^1)=98801$. Có sai số nhưng bỏ qua.
2.  Với dư 98801 ước lượng $9880/988=10$, nên $98801-(9876 \times 10 \times 10^0)=41$ là dư cuối.
3.  Cộng kết quả và xử lý nhớ: $3 \times 10^1 + 10 \times 10^0 = 40$ là thương đúng.

Ý tưởng đơn giản nhưng dễ mắc bẫy. Dưới đây là một cài đặt đã được kiểm chứng:

??? note "Cài đặt tham khảo chia đặt dọc hiệu quả trong ép cơ số"
    ```cpp
    // Template đầy đủ và hiện thực https://baobaobear.github.io/post/20210228-bigint1/
    // Trừ b * mul rồi dịch trái offset, phục vụ chia
    BigIntSimple &sub_mul(const BigIntSimple &b, int mul, int offset) {
      if (mul == 0) return *this;
      int borrow = 0;
      // Khác với phép trừ, borrow có thể rất lớn, không thể dùng cách trừ thường
      for (size_t i = 0; i < b.v.size(); ++i) {
        borrow += v[i + offset] - b.v[i] * mul - BIGINT_BASE + 1;
        v[i + offset] = borrow % BIGINT_BASE + BIGINT_BASE - 1;
        borrow /= BIGINT_BASE;
      }
      // Nếu còn mượn thì tiếp tục xử lý
      for (size_t i = b.v.size(); borrow; ++i) {
        borrow += v[i + offset] - BIGINT_BASE + 1;
        v[i + offset] = borrow % BIGINT_BASE + BIGINT_BASE - 1;
        borrow /= BIGINT_BASE;
      }
      return *this;
    }
    
    BigIntSimple div_mod(const BigIntSimple &b, BigIntSimple &r) const {
      BigIntSimple d;
      r = *this;
      if (absless(b)) return d;
      d.v.resize(v.size() - b.v.size() + 1);
      // Tính sẵn nghịch đảo của 3 chữ số cao nhất +1 của b
      // Nếu 3 chữ số cao nhất là a3,a2,a1
      // db là 1 / (a3 + a2/base + (a1+1)/base^2), dùng để ước lượng thương
      // Cách này với BIGINT_BASE<=32768 thì an toàn trong int32
      // Dù dùng int64, cũng chỉ an toàn khi BIGINT_BASE<=131072 (giới hạn độ chính xác double)
      // Đảm bảo q'<=q<=q'+1
      // Nên mỗi chữ số trung bình chỉ thử 1 lần, sau đó dồn xử lý nhớ
      // Nếu dùng base lớn hơn thì cần cách ước lượng khác
      double t = (b.get((unsigned)b.v.size() - 2) +
                  (b.get((unsigned)b.v.size() - 3) + 1.0) / BIGINT_BASE);
      double db = 1.0 / (b.v.back() + t / BIGINT_BASE);
      for (size_t i = v.size() - 1, j = d.v.size() - 1; j <= v.size();) {
        int rm = r.get(i + 1) * BIGINT_BASE + r.get(i);
        int m = std::max((int)(db * rm), r.get(i + 1));
        r.sub_mul(b, m, j);
        d.v[j] += m;
        if (!r.get(i + 1))  // Kiểm tra chữ số cao nhất đã về 0 chưa, tránh trường hợp cực đoan
          --i, --j;
      }
      r.trim();
      // Sửa chữ số hàng đơn vị
      int carry = 0;
      while (!r.absless(b)) {
        r.subtract(b);
        ++carry;
      }
      // Sửa nhớ cho mỗi chữ số
      for (size_t i = 0; i < d.v.size(); ++i) {
        carry += d.v[i];
        d.v[i] = carry % BIGINT_BASE;
        carry /= BIGINT_BASE;
      }
      d.trim();
      d.sign = sign * b.sign;
      return d;
    }
    
    BigIntSimple operator/(const BigIntSimple &b) const {
      BigIntSimple r;
      return div_mod(b, r);
    }
    
    BigIntSimple operator%(const BigIntSimple &b) const {
      BigIntSimple r;
      div_mod(b, r);
      return r;
    }
    ```

## Nhân Karatsuba

Gọi số chữ số là $n$, nhân đặt dọc số lớn cần $O(n^2)$. Phần này giới thiệu thuật toán tốt hơn do Anatoly Karatsuba đề xuất, là một thuật toán chia để trị.

Xét hai số nguyên lớn $x,y$ hệ thập phân, đều có $n$ chữ số (có thể có số 0 đầu). Chọn $0 < m < n$, đặt

$$
\begin{aligned}
x &= x_1 \cdot 10^m + x_0, \\
y &= y_1 \cdot 10^m + y_0, \\
x \cdot y &= z_2 \cdot 10^{2m} + z_1 \cdot 10^m + z_0,
\end{aligned}
$$

với $x_0, y_0, z_0, z_1 < 10^m$. Khi đó

$$
\begin{aligned}
z_2 &= x_1 \cdot y_1, \\
z_1 &= x_1 \cdot y_0 + x_0 \cdot y_1, \\
z_0 &= x_0 \cdot y_0.
\end{aligned}
$$

Quan sát

$$
z_1 = (x_1 + x_0) \cdot (y_1 + y_0) - z_2 - z_0,
$$

nên để tính $z_1$ chỉ cần tính $(x_1 + x_0)(y_1 + y_0)$ rồi trừ $z_2$ và $z_0$.

Đây là hạt nhân của Karatsuba: bài toán nhân độ dài $n$ chuyển thành 3 bài toán nhỏ hơn. Lấy $m = \left\lceil \dfrac n 2 \right\rceil$, nếu $T(n)$ là thời gian nhân hai số $n$ chữ số, thì $T(n) = 3 \cdot T \left(\left\lceil \dfrac n 2 \right\rceil\right) + O(n)$, suy ra $T(n) = \Theta(n^{\log_2 3}) \approx \Theta(n^{1.585})$.

Có thể hiện thực đệ quy. Để rõ ràng, đoạn mã dưới dùng Karatsuba để nhân đa thức, rồi xử lý nhớ.

??? note "karatsuba_mulc.cpp"
    ```cpp
    int *karatsuba_polymul(int n, int *a, int *b) {
      if (n <= 32) {
        // Kích thước nhỏ thì tính trực tiếp, tránh đệ quy làm chậm
        int *r = new int[n * 2 + 1]();
        for (int i = 0; i <= n; ++i)
          for (int j = 0; j <= n; ++j) r[i + j] += a[i] * b[j];
        return r;
      }
    
      int m = n / 2 + 1;
      int *r = new int[m * 4 + 1]();
      int *z0, *z1, *z2;
    
      z0 = karatsuba_polymul(m - 1, a, b);
      z2 = karatsuba_polymul(n - m, a + m, b + m);
    
      // Tính z1
      // Tạm thời thay đổi, xong thì khôi phục
      for (int i = 0; i + m <= n; ++i) a[i] += a[i + m];
      for (int i = 0; i + m <= n; ++i) b[i] += b[i + m];
      z1 = karatsuba_polymul(m - 1, a, b);
      for (int i = 0; i + m <= n; ++i) a[i] -= a[i + m];
      for (int i = 0; i + m <= n; ++i) b[i] -= b[i + m];
      for (int i = 0; i <= (m - 1) * 2; ++i) z1[i] -= z0[i];
      for (int i = 0; i <= (n - m) * 2; ++i) z1[i] -= z2[i];
    
      // Ghép z0, z1, z2 thành kết quả
      for (int i = 0; i <= (m - 1) * 2; ++i) r[i] += z0[i];
      for (int i = 0; i <= (m - 1) * 2; ++i) r[i + m] += z1[i];
      for (int i = 0; i <= (n - m) * 2; ++i) r[i + m * 2] += z2[i];
    
      delete[] z0;
      delete[] z1;
      delete[] z2;
      return r;
    }
    
    void karatsuba_mul(int a[], int b[], int c[]) {
      int *r = karatsuba_polymul(LEN - 1, a, b);
      memcpy(c, r, sizeof(int) * LEN);
      for (int i = 0; i < LEN - 1; ++i)
        if (c[i] >= 10) {
          c[i + 1] += c[i] / 10;
          c[i] %= 10;
        }
      delete[] r;
    }
    ```

??? note "Về `new` và `delete`"
    Xem [memory pool](../contest/common-tricks.md#内存池).

Tuy nhiên cách này có vấn đề: trong cơ số $b$, mỗi hệ số đa thức có thể tới cỡ $n \cdot b^2$, trong ép cơ số có thể tràn số; còn nếu xử lý nhớ trong quá trình nhân đa thức thì $x_1+x_0$ và $y_1+y_0$ có thể đạt $2 \cdot b^m$, tăng thêm một chữ số (nếu dùng $x_1-x_0$ thì phải xử lý số âm). Do đó cần chọn hiện thực phù hợp với bối cảnh.

## Nhân số lớn hiệu quả dựa trên đa thức

Nếu dữ liệu tới cỡ $10^{10^5}$ hoặc lớn hơn, nhân số lớn thông thường có thể TLE. Phần này giới thiệu cách dùng đa thức tối ưu.

Với số $a$ có $n$ chữ số, có thể xem như đa thức $A=a_{0} 10^0+a_{1} 10^1+\cdots+a_{n-1} 10^{n-1}$ với hệ số nguyên không quá $10$. Như vậy nhân số nguyên chuyển thành nhân đa thức.

Nhân đa thức thường là $O(n^2)$, nhưng có thể dùng [FFT](poly/fft.md), [NTT](poly/ntt.md) để giảm xuống $O(n\log n)$.

## Đóng gói thành lớp

[Ở đây](https://paste.ubuntu.com/p/7VKYzpC7dn/) có một lớp số lớn đóng gói sẵn, và [ở đây](https://github.com/Baobaobear/MiniBigInteger/blob/main/bigint_tiny.h) là một lớp siêu nhỏ hỗ trợ độ dài động và bốn phép toán.

??? note "Đây là một template khác"
    ```cpp
    constexpr int MAXN = 9999;
    // MAXN là giá trị lớn nhất của một “chữ số”
    constexpr int MAXSIZE = 10024;
    // MAXSIZE là số chữ số
    constexpr int DLEN = 4;
    
    // DLEN ghi số chữ số gộp vào một “chữ số”
    struct Big {
      int a[MAXSIZE], len;
      bool flag;  // Đánh dấu dấu '-' cho số âm
    
      Big() {
        len = 1;
        memset(a, 0, sizeof a);
        flag = false;
      }
    
      Big(const int);
      Big(const char*);
      Big(const Big&);
      Big& operator=(const Big&);
      Big operator+(const Big&) const;
      Big operator-(const Big&) const;
      Big operator*(const Big&) const;
      Big operator/(const int&) const;
      // TODO: Big / Big;
      Big operator^(const int&) const;
      // TODO: Big ^ Big;
    
      // TODO: Big phép toán bit;
    
      int operator%(const int&) const;
      // TODO: Big ^ Big;
      bool operator<(const Big&) const;
      bool operator<(const int& t) const;
      void print() const;
    };
    
    Big::Big(const int b) {
      int c, d = b;
      len = 0;
      // memset(a,0,sizeof a);
      CLR(a);
      while (d > MAXN) {
        c = d - (d / (MAXN + 1) * (MAXN + 1));
        d = d / (MAXN + 1);
        a[len++] = c;
      }
      a[len++] = d;
    }
    
    Big::Big(const char* s) {
      int t, k, index, l;
      CLR(a);
      l = strlen(s);
      len = l / DLEN;
      if (l % DLEN) ++len;
      index = 0;
      for (int i = l - 1; i >= 0; i -= DLEN) {
        t = 0;
        k = i - DLEN + 1;
        if (k < 0) k = 0;
        g(j, k, i) t = t * 10 + s[j] - '0';
        a[index++] = t;
      }
    }
    
    Big::Big(const Big& T) : len(T.len) {
      CLR(a);
      f(i, 0, len) a[i] = T.a[i];
      // TODO: có cần overload chỗ này?
    }
    
    Big& Big::operator=(const Big& T) {
      CLR(a);
      len = T.len;
      f(i, 0, len) a[i] = T.a[i];
      return *this;
    }
    
    Big Big::operator+(const Big& T) const {
      Big t(*this);
      int big = len;
      if (T.len > len) big = T.len;
      f(i, 0, big) {
        t.a[i] += T.a[i];
        if (t.a[i] > MAXN) {
          ++t.a[i + 1];
          t.a[i] -= MAXN + 1;
        }
      }
      if (t.a[big])
        t.len = big + 1;
      else
        t.len = big;
      return t;
    }
    
    Big Big::operator-(const Big& T) const {
      int big;
      bool ctf;
      Big t1, t2;
      if (*this < T) {
        t1 = T;
        t2 = *this;
        ctf = true;
      } else {
        t1 = *this;
        t2 = T;
        ctf = false;
      }
      big = t1.len;
      int j = 0;
      f(i, 0, big) {
        if (t1.a[i] < t2.a[i]) {
          j = i + 1;
          while (t1.a[j] == 0) ++j;
          --t1.a[j--];
          // WTF?
          while (j > i) t1.a[j--] += MAXN;
          t1.a[i] += MAXN + 1 - t2.a[i];
        } else
          t1.a[i] -= t2.a[i];
      }
      t1.len = big;
      while (t1.len > 1 && t1.a[t1.len - 1] == 0) {
        --t1.len;
        --big;
      }
      if (ctf) t1.a[big - 1] = -t1.a[big - 1];
      return t1;
    }
    
    Big Big::operator*(const Big& T) const {
      Big res;
      int up;
      int te, tee;
      f(i, 0, len) {
        up = 0;
        f(j, 0, T.len) {
          te = a[i] * T.a[j] + res.a[i + j] + up;
          if (te > MAXN) {
            tee = te - te / (MAXN + 1) * (MAXN + 1);
            up = te / (MAXN + 1);
            res.a[i + j] = tee;
          } else {
            up = 0;
            res.a[i + j] = te;
          }
        }
        if (up) res.a[i + T.len] = up;
      }
      res.len = len + T.len;
      while (res.len > 1 && res.a[res.len - 1] == 0) --res.len;
      return res;
    }
    
    Big Big::operator/(const int& b) const {
      Big res;
      int down = 0;
      gd(i, len - 1, 0) {
        res.a[i] = (a[i] + down * (MAXN + 1)) / b;
        down = a[i] + down * (MAXN + 1) - res.a[i] * b;
      }
      res.len = len;
      while (res.len > 1 && res.a[res.len - 1] == 0) --res.len;
      return res;
    }
    
    int Big::operator%(const int& b) const {
      int d = 0;
      gd(i, len - 1, 0) d = (d * (MAXN + 1) % b + a[i]) % b;
      return d;
    }
    
    Big Big::operator^(const int& n) const {
      Big t(n), res(1);
      int y = n;
      while (y) {
        if (y & 1) res = res * t;
        t = t * t;
        y >>= 1;
      }
      return res;
    }
    
    bool Big::operator<(const Big& T) const {
      int ln;
      if (len < T.len) return true;
      if (len == T.len) {
        ln = len - 1;
        while (ln >= 0 && a[ln] == T.a[ln]) --ln;
        if (ln >= 0 && a[ln] < T.a[ln]) return true;
        return false;
      }
      return false;
    }
    
    bool Big::operator<(const int& t) const {
      Big tee(t);
      return *this < tee;
    }
    
    void Big::print() const {
      printf("%d", a[len - 1]);
      gd(i, len - 2, 0) { printf("%04d", a[i]); }
    }
    
    void print(const Big& s) {
      int len = s.len;
      printf("%d", s.a[len - 1]);
      gd(i, len - 2, 0) { printf("%04d", s.a[i]); }
    }
    
    char s[100024];
    ```

## Bài tập

-   [NOIP 2012 国王游戏](https://loj.ac/problem/2603)
-   [SPOJ - Fast Multiplication](http://www.spoj.com/problems/MUL/en/)
-   [SPOJ - GCD2](http://www.spoj.com/problems/GCD2/)
-   [UVa - Division](https://uva.onlinejudge.org/index.php?option=onlinejudge&page=show_problem&problem=1024)
-   [UVa - Fibonacci Freeze](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=436)
-   [Codeforces - Notepad](http://codeforces.com/contest/17/problem/D)

## Tài liệu tham khảo & liên kết

1.  [Karatsuba algorithm - Wikipedia](https://en.wikipedia.org/wiki/Karatsuba_algorithm)
