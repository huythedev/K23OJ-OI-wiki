## Giới thiệu

![](images/hashtable.svg)

Bảng băm (Hash Table), hay còn gọi là Bảng phân tán (Hash List), là một cấu trúc dữ liệu lưu trữ dữ liệu dưới dạng cặp **"khóa-giá trị"** (key-value). Lưu trữ dữ liệu dưới dạng "khóa-giá trị" có nghĩa là bất kỳ khóa (key) nào cũng được ánh xạ duy nhất tới một vị trí cụ thể trong bộ nhớ. Chỉ cần nhập khóa cần tìm, ta có thể nhanh chóng tìm được giá trị tương ứng. Có thể hiểu bảng băm như một kiểu mảng nâng cao, trong đó chỉ số mảng có thể là số nguyên rất lớn, số thực, chuỗi, hoặc thậm chí là cấu trúc (struct).

## Hàm Băm

Để ánh xạ khóa tới một vị trí trong bộ nhớ, ta cần tính toán một chỉ số (index) từ khóa, hàm tính chỉ số này được gọi là **hàm băm** (hash function), hay hàm phân tán. Ví dụ, nếu khóa là số căn cước công dân của một người, hàm băm có thể là bốn chữ số cuối của số đó, hoặc bốn chữ số đầu. "Số đuôi điện thoại di động" thường dùng trong đời sống cũng là một dạng hàm băm. Trong các ứng dụng thực tế, khóa có thể phức tạp hơn, ví dụ như số thực, chuỗi, cấu trúc, lúc này cần thiết kế hàm băm phù hợp với từng trường hợp cụ thể. Hàm băm phải dễ tính toán và cố gắng phân bố các chỉ số tính được một cách đồng đều.

Sau khi có thể tính chỉ số cho khóa, ta biết được giá trị `value` tương ứng với cặp `(key, value)` nên được đặt ở đâu. Giả sử ta dùng mảng `a` để lưu trữ dữ liệu, và hàm băm là $f$, thì cặp `(key, value)` sẽ được đặt tại `a[f(key)]`. Bất kể khóa thuộc kiểu gì, phạm vi bao nhiêu, $f(key)$ luôn là một số nguyên trong phạm vi chấp nhận được, có thể dùng làm chỉ số mảng.

Trong lập trình thi đấu (OI), trường hợp phổ biến nhất là khóa là số nguyên. Khi phạm vi khóa nhỏ, ta có thể dùng trực tiếp khóa làm chỉ số mảng. Nhưng khi phạm vi khóa lớn, ví dụ số nguyên trong phạm vi $10^9$, ta cần dùng bảng băm. Thông thường, ta lấy khóa modulo một số nguyên tố lớn làm chỉ số, tức là lấy $f(x) = x \bmod M$ làm hàm băm.

Một trường hợp phổ biến khác là khóa là chuỗi (string). Do không thể dùng chuỗi làm chỉ số mảng, và việc chuyển đổi chuỗi thành số để lưu trữ cũng tránh được việc so sánh chuỗi nhiều lần. Trong OI, người ta thường không dùng chuỗi làm khóa trực tiếp, mà tính **giá trị băm** của chuỗi, sau đó dùng giá trị băm này làm khóa để chèn vào bảng băm. Về giá trị băm chuỗi, ta thường dùng ý tưởng cơ số, coi chuỗi như một số trong hệ cơ số $127$. Với chuỗi $s$ có độ dài $n$:

$$x = s_0 \cdot 127^0 + s_1 \cdot 127^1 + s_2 \cdot 127^2 + \dots + s_n \cdot 127^n$$

Ta có thể lấy $x$ modulo $2^{64}$ (giá trị lớn nhất của `unsigned long long`). Khi đó, sự tràn tự nhiên của `unsigned long long` tương đương với phép toán modulo. Điều này làm cho thao tác tiện lợi hơn.

Phương pháp này tuy đơn giản nhưng không hoàn hảo. Có thể xây dựng dữ liệu khiến phương pháp này xảy ra **xung đột** (collision) (tức là hai chuỗi khác nhau có kết quả $x$ modulo $2^{64}$ bằng nhau). Ta có thể sử dụng **băm kép** (double hashing): chọn hai số nguyên tố lớn $a, b$. Chỉ khi giá trị băm của hai chuỗi modulo $a$ và modulo $b$ đều bằng nhau, ta mới coi hai chuỗi đó là bằng nhau. Điều này làm giảm đáng kể xác suất xảy ra xung đột băm.

## Xung đột (Collision)

Nếu hàm băm tính ra chỉ số khác nhau cho mọi khóa, ta chỉ cần đặt cặp `(key, value)` vào vị trí tương ứng với chỉ số đó. Tuy nhiên, trong thực tế, thường xảy ra trường hợp hai khóa khác nhau lại có cùng chỉ số tính được từ hàm băm. Khi đó, cần có phương pháp để xử lý xung đột. Trong OI, phương pháp phổ biến nhất là **phương pháp mảng liên kết** (Chaining method).

### Phương pháp mảng liên kết (Chaining)

Phương pháp mảng liên kết còn gọi là **băm mở** (open hashing).

Phương pháp mảng liên kết là mở một danh sách liên kết tại mỗi vị trí lưu trữ dữ liệu. Nếu có nhiều khóa có cùng chỉ số, ta chỉ cần đưa tất cả chúng vào danh sách liên kết tại vị trí đó. Khi truy vấn, ta phải duyệt toàn bộ danh sách liên kết tại vị trí tương ứng, so sánh khóa của từng phần tử với khóa truy vấn. Nếu phạm vi chỉ số là $1\ldots M$, và kích thước bảng băm là $N$, thì một lần chèn/truy vấn cần trung bình $O(\frac{N}{M})$ lần so sánh.

#### Triển khai

=== "C++"
    ```cpp
    constexpr int SIZE = 1000000;
    constexpr int M = 999997;
    
    struct HashTable {
      struct Node {
        int next, value, key;
      } data[SIZE];
    
      int head[M], size;
    
      int f(int key) { return (key % M + M) % M; }
    
      int get(int key) {
        for (int p = head[f(key)]; p; p = data[p].next)
          if (data[p].key == key) return data[p].value;
        return -1;
      }
    
      int modify(int key, int value) {
        for (int p = head[f(key)]; p; p = data[p].next)
          if (data[p].key == key) return data[p].value = value;
      }
    
      int add(int key, int value) {
        if (get(key) != -1) return -1;
        data[++size] = Node{head[f(key)], value, key};
        head[f(key)] = size;
        return value;
      }
    };
    ```

=== "Python"
    ```python
    M = 999997
    SIZE = 1000000
    
    
    class Node:
        def __init__(self, next=None, value=None, key=None):
            self.next = next
            self.value = value
            self.key = key
    
    
    data = [Node() for _ in range(SIZE)]
    head = [0] * M
    size = 0
    
    
    def f(key):
        return key % M
    
    
    def get(key):
        p = head[f(key)]
        while p:
            if data[p].key == key:
                return data[p].value
            p = data[p].next
        return -1
    
    
    def modify(key, value):
        p = head[f(key)]
        while p:
            if data[p].key == key:
                data[p].value = value
                return data[p].value
            p = data[p].next
    
    
    def add(key, value):
        if get(key) != -1:
            return -1
        size = size + 1
        data[size] = Node(head[f(key)], value, key)
        head[f(key)] = size
        return value
    ```

Ở đây cung cấp thêm một mẫu được đóng gói, có thể sử dụng giống như `map`, và ngắn hơn:

```cpp
struct hash_map {  // Mẫu bảng băm

  struct data {
    long long u;
    int v, nex;
  };  // Cấu trúc chuỗi tiến (forward star)

  data e[SZ << 1];  // SZ là const int biểu thị kích thước
  int h[SZ], cnt;

  int hash(long long u) { return (u % SZ + SZ) % SZ; }

  // Ở đây sử dụng (u % SZ + SZ) % SZ thay vì u % SZ là vì
  // Phép toán % trong C++ không thể chuyển số âm thành số dương

  int& operator[](long long u) {
    int hu = hash(u);  // Lấy con trỏ đầu chuỗi
    for (int i = h[hu]; i; i = e[i].nex)
      if (e[i].u == u) return e[i].v;
    return e[++cnt] = data{u, -1, h[hu]}, h[hu] = cnt, e[cnt].v;
  }

  hash_map() {
    cnt = 0;
    memset(h, 0, sizeof(h));
  }
};
```

Ở đây, hàm băm được thiết kế cho kiểu khóa, và trả về một con trỏ đầu danh sách liên kết để truy vấn. Trong mẫu này, ta viết bảng băm với cặp khóa-giá trị kiểu `(long long, int)`, và trả về -1 khi khóa không tồn tại. Hàm `hash_map()` được dùng để khởi tạo khi khai báo.

### Phương pháp băm kín (Closed Hashing)

Phương pháp băm kín lưu trữ tất cả các bản ghi trực tiếp trong bảng băm, nếu xảy ra xung đột thì tiếp tục thăm dò theo một quy tắc nào đó.

Ví dụ: Phương pháp thăm dò tuyến tính (Linear Probing): Nếu xảy ra xung đột tại vị trí `d`, ta lần lượt kiểm tra `d + 1`, `d + 2`, ...

#### Triển khai

```cpp
constexpr int N = 360007;  // N là số lượng phần tử tối đa có thể lưu trữ

class Hash {
 private:
  int keys[N];
  int values[N];

 public:
  Hash() { memset(values, 0, sizeof(values)); }

  int& operator[](int n) {
    // Trả về một tham chiếu tới Hash[Key] tương ứng
    // Giá trị 0 được coi là rỗng (trừ khi giá trị thực sự cần lưu là 0)
    int idx = (n % N + N) % N, cnt = 1;
    while (keys[idx] != n && values[idx] != 0) {
      idx = (idx + cnt * cnt) % N;
      cnt += 1;
    }
    keys[idx] = n;
    return values[idx];
  }
};
```

## Bài toán ví dụ

[“JLOI2011” Số không lặp lại](https://www.luogu.com.cn/problem/P4305)