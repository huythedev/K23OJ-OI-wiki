Trang này sẽ giới thiệu tóm tắt về thuật toán sắp xếp nhanh (Quick Sort).

## Định nghĩa

**Sắp xếp nhanh** (tiếng Anh: Quicksort), còn được gọi là sắp xếp phân chia trao đổi (partition-exchange sort), thường được gọi tắt là "Quick Sort", là một thuật toán sắp xếp được sử dụng rộng rãi.

## Nguyên lý cơ bản và Cài đặt

### Quá trình

Nguyên lý hoạt động của sắp xếp nhanh là thông qua phương pháp [Chia để trị](./divide-and-conquer.md) để sắp xếp một mảng.

Sắp xếp nhanh chia thành ba giai đoạn:

1.  Chia dãy số thành hai phần (yêu cầu đảm bảo quan hệ độ lớn tương đối);
2.  Đệ quy vào hai dãy con để thực hiện sắp xếp nhanh riêng biệt;
3.  Không cần gộp lại, vì lúc này dãy số đã hoàn toàn có thứ tự.

Khác với sắp xếp trộn (Merge Sort), bước đầu tiên không phải là chia trực tiếp thành hai dãy trước và sau, mà trong quá trình chia phải đảm bảo quan hệ độ lớn tương đối. Cụ thể, bước đầu tiên là chia dãy số thành hai phần, sau đó đảm bảo các số trong dãy con thứ nhất đều nhỏ hơn các số trong dãy con thứ hai. Để đảm bảo độ phức tạp thời gian trung bình, người ta thường chọn ngẫu nhiên một số $m$ để làm ranh giới giữa hai dãy con.

Sau đó, duy trì hai con trỏ $p$ và $q$ ở trước và sau, lần lượt xem xét số hiện tại có nằm đúng vị trí (trước hay sau) hay không. Nếu số hiện tại không đúng vị trí, ví dụ nếu con trỏ phía sau $q$ gặp một số nhỏ hơn $m$, thì có thể hoán đổi các số tại vị trí $p$ và $q$, sau đó di chuyển $p$ về phía sau một vị trí. Sau khi vị trí của số hiện tại đã đúng, tiếp tục di chuyển con trỏ để xử lý, cho đến khi hai con trỏ gặp nhau.

Thực tế, sắp xếp nhanh không quy định cụ thể cách thực hiện bước đầu tiên, dù là quá trình chọn $m$ hay quá trình phân chia, đều có nhiều hơn một cách thực hiện.

Dãy số ở bước thứ ba đã lần lượt có thứ tự và các số trong dãy thứ nhất đều nhỏ hơn các số trong dãy thứ hai, vì vậy chỉ cần nối chúng lại là xong.

=== "C++"
    === "Cài đặt không đệ quy[^ref2]"
        ```cpp
        struct Range {
          int start, end;
        
          Range(int s = 0, int e = 0) { start = s, end = e; }
        };
        
        template <typename T>
        void quick_sort(T arr[], const int len) {
          if (len <= 0) return;
          Range r[len];
          int p = 0;
          r[p++] = Range(0, len - 1);
          while (p) {
            Range range = r[--p];
            if (range.start >= range.end) continue;
            T mid = arr[range.end];
            int left = range.start, right = range.end - 1;
            while (left < right) {
              while (arr[left] < mid && left < right) left++;
              while (arr[right] >= mid && left < right) right--;
              std::swap(arr[left], arr[right]);
            }
            if (arr[left] >= arr[range.end])
              std::swap(arr[left], arr[range.end]);
            else
              left++;
            r[p++] = Range(range.start, left - 1);
            r[p++] = Range(left + 1, range.end);
          }
        }
        ```
    
    === "Cài đặt đệ quy"
        ```cpp
        template <typename T>
        int Partition(T A[], int low, int high) {
          int pivot = A[low];
          while (low < high) {
            while (low < high && pivot <= A[high]) --high;
            A[low] = A[high];
            while (low < high && A[low] <= pivot) ++low;
            A[high] = A[low];
          }
          A[low] = pivot;
          return low;
        }
        
        template <typename T>
        void QuickSort(T A[], int low, int high) {
          if (low < high) {
            int pivot = Partition(A, low, high);
            QuickSort(A, low, pivot - 1);
            QuickSort(A, pivot + 1, high);
          }
        }
        
        template <typename T>
        void QuickSort(T A[], int len) {
          QuickSort(A, 0, len - 1);
        }
        ```

=== "Python[^ref2]"
    ```python
    def quick_sort(alist, first, last):
        if first >= last:
            return
        mid_value = alist[first]
        low = first
        high = last
        while low < high:
            while low < high and alist[high] >= mid_value:
                high -= 1
            alist[low] = alist[high]
            while low < high and alist[low] < mid_value:
                low += 1
            alist[high] = alist[low]
        alist[low] = mid_value
        quick_sort(alist, first, low - 1)
        quick_sort(alist, low + 1, last)
    ```

## Tính chất

### Tính ổn định

Sắp xếp nhanh là một thuật toán sắp xếp không ổn định.

### Độ phức tạp thời gian

Độ phức tạp thời gian tốt nhất và trung bình của sắp xếp nhanh là $O(n\log n)$, độ phức tạp thời gian xấu nhất là $O(n^2)$.

Đối với trường hợp tốt nhất, mỗi lần chọn giá trị phân chia (pivot) đều là trung vị của dãy, lúc này công thức truy hồi về độ phức tạp thời gian của thuật toán là $T(n) = 2T(\dfrac{n}{2}) + \Theta(n)$, theo Định lý thợ (Master Theorem), $T(n) = \Theta(n\log n)$.

Đối với trường hợp xấu nhất, mỗi lần chọn giá trị phân chia đều là giá trị cực trị (lớn nhất hoặc nhỏ nhất) của dãy, lúc này công thức truy hồi là $T(n) = T(n - 1) + \Theta(n)$, cộng dồn lại ta được $T(n) = \Theta(n^2)$.

Đối với trường hợp trung bình, mỗi lần chọn giá trị phân chia có thể xem như ngẫu nhiên với xác suất bằng nhau.

??? note "Chứng minh"
    Dưới đây chúng ta chứng minh rằng độ phức tạp thời gian của thuật toán trong trường hợp này là $O(n\log n)$.
    
    **Bổ đề 1:** Khi thực hiện sắp xếp nhanh trên mảng có $n$ phần tử, giả sử tổng số lần so sánh trong quá trình phân chia phần tử là $X$, thì độ phức tạp thời gian của sắp xếp nhanh là $O(n + X)$.
    
    Do trong mỗi quá trình phân chia phần tử, đều chọn một phần tử làm ranh giới, nên quá trình phân chia phần tử xảy ra tối đa $n$ lần. Lại do số lần so sánh trong quá trình tương đương về độ lớn với số lần thực hiện các thao tác cơ bản khác, nên tổng độ phức tạp thời gian là $O(n + X)$.
    
    Gọi $a_i$ là số nhỏ thứ $i$ trong mảng ban đầu, định nghĩa $A_{i,j}$ là $\{ a_i, a_{i+1}, \dots, a_j \}$, $X_{i,j}$ là một biến ngẫu nhiên rời rạc nhận giá trị $0$ hoặc $1$ biểu thị việc $a_i$ có so sánh với $a_j$ trong quá trình sắp xếp hay không.
    
    Rõ ràng mỗi lần chọn giá trị phân chia là khác nhau, và phần tử chỉ so sánh với giá trị phân chia, nên tổng số lần so sánh
    
    $$
    \begin{aligned} X = \sum \limits _ {i = 1} ^ {n - 1} \sum \limits _ {j = i + 1} ^ n X_{i,j} \end{aligned}
    $$
    
    Theo tính chất tuyến tính của kỳ vọng,
    
    $$
    \begin{aligned} E[X] & = E \left[ \sum \limits _ {i = 1} ^ {n - 1} \sum \limits _ {j = i + 1} ^ n X_{i,j} \right] \\ & = \sum \limits _ {i = 1} ^ {n - 1} \sum \limits _ {j = i + 1} ^ n E[X_{i,j}] \\ & = \sum \limits _ {i = 1} ^ {n - 1} \sum \limits _ {j = i + 1} ^ n P(a_i\ \text{và}\ a_j\ \text{so sánh}) \end{aligned}
    $$
    
    **Bổ đề 2:** $a_i$ và $a_j$ so sánh khi và chỉ khi $a_i$ hoặc $a_j$ là giá trị phân chia đầu tiên được chọn từ tập hợp $A_{i,j}$.
    
    Trước tiên chứng minh điều kiện cần, tức là nếu cả $a_i$ và $a_j$ đều không phải là giá trị phân chia đầu tiên được chọn từ tập hợp $A_{i,j}$, thì $a_i$ không so sánh với $a_j$.
    
    Nếu cả $a_i$ và $a_j$ đều không phải là giá trị phân chia đầu tiên được chọn từ tập hợp $A_{i,j}$, thì chắc chắn tồn tại một $x$ thỏa mãn $i < x < j$, sao cho $a_x$ là giá trị phân chia đầu tiên được chọn trong $A_{i,j}$. Trong lần phân chia với $a_x$ làm ranh giới, $a_i$ và $a_j$ bị chia vào hai dãy con khác nhau của mảng, nên sau đó $a_i$ và $a_j$ chắc chắn sẽ không so sánh với nhau. Lại vì phần tử chỉ so sánh với giá trị phân chia, nên $a_i$ và $a_j$ không so sánh trước và trong lần phân chia này. Vậy $a_i$ không so sánh với $a_j$.
    
    Tiếp theo chứng minh điều kiện đủ, tức là nếu $a_i$ hoặc $a_j$ là giá trị phân chia đầu tiên được chọn từ tập hợp $A_{i,j}$, thì $a_i$ và $a_j$ có so sánh.
    
    Không mất tính tổng quát, giả sử $a_i$ là giá trị phân chia đầu tiên được chọn từ tập hợp $A_{i,j}$. Do trong $A_{i,j}$ chưa có số nào khác được chọn làm giá trị phân chia, nên các phần tử trong $A_{i,j}$ đều nằm trong cùng một dãy con của mảng. Trong lần phân chia với $a_i$ làm ranh giới, $a_i$ so sánh với tất cả các phần tử trong dãy con hiện tại, nên $a_i$ và $a_j$ đã thực hiện so sánh.
    
    Xem xét tính $P(a_i\ \text{và}\ a_j\ \text{so sánh})$. Trước khi một phần tử nào đó trong $A_{i,j}$ được chọn làm giá trị phân chia, các phần tử trong $A_{i,j}$ đều nằm trong cùng một dãy con của mảng. Vì vậy mỗi phần tử trong $A_{i,j}$ đều có khả năng như nhau để trở thành giá trị phân chia được chọn đầu tiên. Do $A_{i,j}$ có $j - i + 1$ phần tử, theo Bổ đề 2,
    
    $$
    P(a_i \text{và} a_j \text{so sánh}) = P(a_i \text{hoặc} a_j \text{là giá trị phân chia đầu tiên được chọn trong tập} A_{i,j}) = \dfrac{2}{j-i+1}
    $$
    
    Vậy
    
    $$
    \begin{aligned} E[X] & = \sum \limits _ {i = 1} ^ {n - 1} \sum \limits _ {j = i + 1} ^ n \dfrac{2}{j - i + 1} \\ & = \sum \limits _ {i = 1} ^ {n - 1} O(\log n) \\ & = O(n \log n) \end{aligned}
    $$
    
    Từ đó, độ phức tạp thời gian kỳ vọng của sắp xếp nhanh là $O(n \log n)$.

Trong thực tế, hầu như không thể đạt được trường hợp xấu nhất, và việc truy cập bộ nhớ của sắp xếp nhanh tuân theo nguyên lý cục bộ, nên trong đa số trường hợp hiệu suất của sắp xếp nhanh vượt trội hơn nhiều so với sắp xếp vun đống (Heap Sort) và các thuật toán sắp xếp độ phức tạp $O(n \log n)$ khác.[^ref1]

## Tối ưu hóa

### Ý tưởng tối ưu hóa chất phác

Nếu chỉ thực hiện sắp xếp nhanh theo ý tưởng cơ bản đã trình bày ở trên (hoặc là chép thẳng code mẫu), thì khả năng cao là sẽ không vượt qua bài [P1177【Template】Quick Sort](https://www.luogu.com.cn/problem/P1177). Bởi vì có những dữ liệu hiểm hóc có thể khiến sắp xếp nhanh thông thường bị suy biến thành $O(n^2)$.

Vì vậy, chúng ta cần tối ưu hóa ý tưởng sắp xếp nhanh thông thường. Có ba hướng tối ưu hóa phổ biến như sau[^ref3].

-   Sử dụng phương pháp **Lấy trung vị của ba số (chọn phần tử đầu, cuối và giữa rồi lấy trung vị)** để chọn phần tử phân chia hai dãy con (tức là mốc so sánh). Cách này có thể tránh được sự suy biến do dữ liệu cực đoan (như dãy tăng dần hoặc giảm dần);
-   Khi dãy số ngắn, sử dụng **Sắp xếp chèn (Insertion Sort)** sẽ hiệu quả hơn;
-   Sau mỗi lần sắp xếp, **tập hợp các phần tử bằng phần tử phân chia lại xung quanh nó**, điều này có thể tránh được sự suy biến do dữ liệu cực đoan (như dãy có phần lớn các phần tử bằng nhau).

Dưới đây liệt kê một vài phương pháp tối ưu hóa sắp xếp nhanh khá hoàn thiện.

### Sắp xếp nhanh 3 đường (3-way Radix Quicksort)

#### Định nghĩa

**Sắp xếp nhanh 3 đường** (tiếng Anh: 3-way Radix Quicksort) là sự kết hợp giữa sắp xếp nhanh và [Sắp xếp cơ số (Radix Sort)](./radix-sort.md). Ý tưởng thuật toán của nó dựa trên cách giải của [bài toán cờ Hà Lan](https://en.wikipedia.org/wiki/Dutch_national_flag_problem).

#### Quá trình

Khác với sắp xếp nhanh nguyên bản, sắp xếp nhanh 3 đường sau khi chọn ngẫu nhiên điểm phân chia $m$, sẽ chia dãy số cần sắp xếp thành ba phần: nhỏ hơn $m$, bằng $m$ và lớn hơn $m$. Việc làm này thực hiện được hiệu quả việc tập hợp các phần tử bằng phần tử phân chia xung quanh nó.

#### Tính chất

Sắp xếp nhanh 3 đường có hiệu suất cao hơn nhiều so với sắp xếp nhanh nguyên bản khi xử lý mảng chứa nhiều giá trị lặp lại. Độ phức tạp thời gian tốt nhất của nó là $O(n)$.

#### Cài đặt

Việc cài đặt sắp xếp nhanh 3 đường rất đơn giản, dưới đây là một cài đặt bằng C++.

=== "C++"
    ```cpp
    // Tham số T của template biểu thị kiểu của phần tử, kiểu này cần định nghĩa toán tử nhỏ hơn (<)
    template <typename T>
    // arr là mảng cần sắp xếp, len là độ dài mảng
    void quick_sort(T arr[], const int len) {
      if (len <= 1) return;
      // Chọn ngẫu nhiên mốc (pivot)
      const T pivot = arr[rand() % len];
      // i: chỉ số phần tử đang thao tác
      // arr[0, j): lưu các phần tử nhỏ hơn pivot
      // arr[k, len): lưu các phần tử lớn hơn pivot
      int i = 0, j = 0, k = len;
      // Hoàn thành một lượt quick sort 3 đường, chia dãy thành:
      // Phần tử nhỏ hơn pivot ｜ Phần tử bằng pivot ｜ Phần tử lớn hơn pivot
      while (i < k) {
        if (arr[i] < pivot)
          swap(arr[i++], arr[j++]);
        else if (pivot < arr[i])
          swap(arr[i], arr[--k]);
        else
          i++;
      }
      // Đệ quy hoàn thành sắp xếp nhanh cho hai dãy con
      quick_sort(arr, j);
      quick_sort(arr + k, len - k);
    }
    ```

=== "Python[^ref2]"
    ```python
    def quick_sort(arr, l, r):
        if l >= r:
            return
        random_index = random.randint(l, r)
        pivot = arr[random_index]
        arr[l], arr[random_index] = arr[random_index], arr[l]
        i = l + 1
        j = l
        k = r + 1
        while i < k:
            if arr[i] < pivot:
                arr[i], arr[j + 1] = arr[j + 1], arr[i]
                j += 1
                i += 1
            elif arr[i] > pivot:
                arr[i], arr[k - 1] = arr[k - 1], arr[i]
                k -= 1
            else:
                i += 1
        arr[l], arr[j] = arr[j], arr[l]
        quick_sort(arr, l, j - 1)
        quick_sort(arr, k, r)
    ```

### Sắp xếp nội suy (Introsort)

#### Định nghĩa

**Sắp xếp nội suy** (tiếng Anh: Introsort hoặc Introspective sort)[^ref4] là sự kết hợp giữa sắp xếp nhanh và [Sắp xếp vun đống (Heap Sort)](./heap-sort.md), do David Musser phát minh năm 1997. Introsort thực chất là một sự tối ưu hóa cho sắp xếp nhanh, đảm bảo độ phức tạp thời gian xấu nhất là $O(n\log n)$.

#### Tính chất

Introsort giới hạn độ sâu đệ quy tối đa của sắp xếp nhanh là $\lfloor \log_2n \rfloor$, khi vượt quá giới hạn sẽ chuyển sang sắp xếp vun đống. Như vậy vừa giữ được tính cục bộ trong truy cập bộ nhớ của sắp xếp nhanh, vừa có thể ngăn chặn hiệu suất của sắp xếp nhanh suy biến thành $O(n^2)$ trong một số trường hợp.

#### Cài đặt

Từ tháng 6 năm 2000, việc cài đặt hàm `sort()` trong `stl_algo.h` của SGI C++ STL đã sử dụng thuật toán Introsort.

## Tìm số lớn thứ k tuyến tính

Trong ví dụ mã dưới đây, số lớn thứ $k$ được định nghĩa là số ở vị trí thứ $k$ khi dãy được sắp xếp tăng dần (đánh số từ 0).

Để tìm số lớn thứ $k$ (K-th order statistic), cách đơn giản nhất là sắp xếp trước, sau đó tìm trực tiếp phần tử ở vị trí lớn thứ $k$. Độ phức tạp thời gian của cách làm này là $O(n\log n)$, đối với vấn đề này thì không hiệu quả lắm.

Chúng ta có thể dựa vào ý tưởng của sắp xếp nhanh để giải quyết vấn đề này. Xem xét quá trình phân chia của sắp xếp nhanh, sau khi kết thúc "phân chia", dãy số $A_{p} \cdots A_{r}$ được chia thành $A_{p} \cdots A_{q}$ và $A_{q+1} \cdots A_{r}$, lúc này có thể dựa vào mối quan hệ giữa số lượng phần tử bên trái ($q - p + 1$) và $k$ để quyết định chỉ đệ quy tìm kiếm ở bên trái hay chỉ ở bên phải.

Giống như sắp xếp nhanh, độ phức tạp thời gian của phương pháp này phụ thuộc vào giá trị phân chia được chọn mỗi lần. Nếu sử dụng cách chọn ngẫu nhiên giá trị phân chia, có thể chứng minh được rằng trong ý nghĩa kỳ vọng, độ phức tạp thời gian của chương trình là $O(n)$.

### Cài đặt (C++)

```cpp
// Tham số T của template biểu thị kiểu của phần tử, kiểu này cần định nghĩa toán tử nhỏ hơn (<)
template <typename T>
// arr là mảng phạm vi tìm kiếm, rk là thứ hạng cần tìm (bắt đầu từ 0), len là độ dài mảng
T find_kth_element(T arr[], int rk, const int len) {
  if (len <= 1) return arr[0];
  // Chọn ngẫu nhiên mốc (pivot)
  const T pivot = arr[rand() % len];
  // i: chỉ số phần tử đang thao tác
  // arr[0, j): lưu các phần tử nhỏ hơn pivot
  // arr[k, len): lưu các phần tử lớn hơn pivot
  int i = 0, j = 0, k = len;
  // Hoàn thành một lượt quick sort 3 đường, chia dãy thành:
  // Phần tử nhỏ hơn pivot ｜ Phần tử bằng pivot ｜ Phần tử lớn hơn pivot
  while (i < k) {
    if (arr[i] < pivot)
      swap(arr[i++], arr[j++]);
    else if (pivot < arr[i])
      swap(arr[i], arr[--k]);
    else
      i++;
  }
  // Dựa vào thứ hạng cần tìm và vị trí của hai đường phân chia, đi vào các khoảng khác nhau để tìm đệ quy số lớn thứ k
  // Nếu số lượng phần tử nhỏ hơn pivot nhiều hơn k, thì phần tử lớn thứ k chắc chắn là một phần tử nhỏ hơn pivot
  if (rk < j) return find_kth_element(arr, rk, j);
  // Ngược lại, nếu tổng số phần tử nhỏ hơn pivot và bằng pivot cũng không đủ k,
  // thì phần tử lớn thứ k chắc chắn là một phần tử lớn hơn pivot
  else if (rk >= k)
    return find_kth_element(arr + k, rk - k, len - k);
  // Ngược lại, pivot chính là phần tử lớn thứ k
  return pivot;
}
```

### Cải tiến: Trung vị của các trung vị

**Trung vị của các trung vị** (tiếng Anh: Median of medians), cung cấp một phương pháp xác định để chọn giá trị phân chia trong quá trình phân chia, từ đó giúp thuật toán tìm số lớn thứ $k$ đạt được độ phức tạp thời gian tuyến tính ngay cả trong trường hợp xấu nhất.

Quy trình của thuật toán như sau:

1.  Chia toàn bộ dãy thành $\left \lfloor \dfrac{n}{5} \right \rfloor$ nhóm, mỗi nhóm có không quá 5 phần tử;
2.  Tìm trung vị của mỗi nhóm (vì số lượng phần tử ít, có thể sử dụng trực tiếp [Sắp xếp chèn](./insertion-sort.md) hoặc các thuật toán tương tự).
3.  Tìm trung vị của $\left \lfloor \dfrac{n}{5} \right \rfloor$ các trung vị nhóm này. Lấy phần tử đó làm giá trị phân chia cho thuật toán nêu trên.

#### Chứng minh độ phức tạp thời gian

Dưới đây sẽ chứng minh rằng độ phức tạp thời gian của thuật toán này trong trường hợp xấu nhất là $O(n)$. Gọi $T(n)$ là khối lượng tính toán cần thiết để giải quyết vấn đề với quy mô $n$.

Trước tiên phân tích hai bước đầu - phân chia và tìm trung vị. Do sau khi phân chia, số lượng phần tử trong mỗi nhóm rất ít, có thể coi thời gian tìm trung vị của một nhóm phần tử là $O(1)$. Do đó thời gian để tìm trung vị của tất cả $\left \lfloor \dfrac{n}{5} \right \rfloor$ nhóm phần tử là $O(n)$.

Tiếp theo phân tích bước thứ ba - quá trình đệ quy. Bước này thực hiện hai lần gọi đệ quy: lần thứ nhất là tìm trung vị của các trung vị nhóm, chi phí rõ ràng là $T(\dfrac{n}{5})$; lần thứ hai là đi vào phần bên trái hoặc bên phải của giá trị phân chia. Dựa vào phần tử phân chia chúng ta đã chọn, có $\dfrac{1}{2} \times \left \lfloor \dfrac{n}{5} \right \rfloor = \left \lfloor \dfrac{n}{10} \right \rfloor$ nhóm có trung vị nhỏ hơn giá trị phân chia, trong các nhóm này, các phần tử nhỏ hơn trung vị chắc chắn cũng nhỏ hơn giá trị phân chia, do đó trong toàn bộ dãy, số lượng phần tử nhỏ hơn giá trị phân chia ít nhất là $3 \times \left \lfloor \dfrac{n}{10} \right \rfloor = \left \lfloor \dfrac{3n}{10} \right \rfloor$. Tương tự, số lượng phần tử lớn hơn giá trị phân chia trong toàn bộ dãy cũng ít nhất là $\left \lfloor \dfrac{3n}{10} \right \rfloor$. Do đó, bên trái hoặc bên phải của giá trị phân chia có tối đa $\dfrac{7n}{10}$ phần tử, cận trên chi phí thời gian cho lần đệ quy này là $T(\dfrac{7n}{10})$.

Tổng hợp lại, chúng ta có thể lập bất đẳng thức sau:

$$
T(n) \leq T(\dfrac{n}{5}) + T(\dfrac{7n}{10}) + O(n)
$$

Giả sử $T(n) = O(n)$ đúng khi quy mô bài toán đủ nhỏ. Theo định nghĩa, lúc này có $T(n) \leq cn$, trong đó $c$ là một hằng số dương. Thay thế tất cả $T(n)$ ở vế phải của bất đẳng thức:

$$
\begin{aligned}
T(n) & \leq T(\dfrac{n}{5}) + T(\dfrac{7n}{10}) + O(n)\\
     & \leq \dfrac{cn}{5} + \dfrac{7cn}{10} + O(n)\\
     & \leq \dfrac{9cn}{10} + O(n)\\
     & = O(n)
\end{aligned}
$$

Đến đây chúng ta đã chứng minh được rằng thuật toán này có độ phức tạp thời gian là $O(n)$ ngay cả trong trường hợp xấu nhất.

## Tài liệu tham khảo và chú thích

[^ref1]: [Nguyên lý cục bộ - Máy ép nước ép hiệu suất C++ - I'm Root lee !](http://irootlee.com/juicer_locality/)

[^ref2]: [Cài đặt thuật toán/Sắp xếp/Quick Sort - Wikibooks](https://zh.wikibooks.org/wiki/%E7%AE%97%E6%B3%95%E5%AE%9E%E7%8E%B0/%E6%8E%92%E5%BA%8F/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F)

[^ref3]: [Ba loại Quick Sort và tối ưu hóa Quick Sort](https://blog.csdn.net/insistGoGo/article/details/7785038)

[^ref4]: [introsort](https://en.wikipedia.org/wiki/Introsort)
