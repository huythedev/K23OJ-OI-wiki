Trang này sẽ giới thiệu ngắn gọn về thuật toán **Sắp xếp theo thùng** (Bucket Sort).

## Định nghĩa

**Sắp xếp theo thùng** (tiếng Anh: Bucket sort) là một thuật toán sắp xếp, phù hợp với trường hợp dữ liệu cần sắp xếp có giá trị lớn nhưng phân bố khá đều.

## Quy trình

Sắp xếp theo thùng thực hiện theo các bước sau:

1.  Khởi tạo một mảng các thùng rỗng;
2.  Duyệt qua dãy số, đưa từng phần tử vào thùng tương ứng;
3.  Sắp xếp từng thùng (nếu thùng không rỗng);
4.  Gom các phần tử từ các thùng về lại dãy ban đầu.

## Tính chất

### Tính ổn định

Nếu sử dụng thuật toán sắp xếp ổn định bên trong mỗi thùng, và khi đưa phần tử vào thùng không làm thay đổi thứ tự tương đối giữa các phần tử, thì sắp xếp theo thùng là một thuật toán ổn định.

Vì mỗi thùng thường có ít phần tử, nên thường dùng sắp xếp chèn (insertion sort) cho mỗi thùng. Khi đó, thuật toán là ổn định.

### Độ phức tạp thời gian

Trung bình, sắp xếp theo thùng có độ phức tạp $O(n + n^2/k + k)$ (chia giá trị thành $n$ thùng + sắp xếp + gộp lại), khi $k\approx n$ thì là $O(n)$.[^ref1]

Trường hợp xấu nhất, độ phức tạp là $O(n^2)$.

## Cài đặt

=== "C++"
    ```cpp
    constexpr int N = 100010;
    
    int n, w, a[N];
    vector<int> bucket[N];
    
    void insertion_sort(vector<int>& A) {
      for (int i = 1; i < A.size(); ++i) {
        int key = A[i];
        int j = i - 1;
        while (j >= 0 && A[j] > key) {
          A[j + 1] = A[j];
          --j;
        }
        A[j + 1] = key;
      }
    }
    
    void bucket_sort() {
      int bucket_size = w / n + 1;
      for (int i = 0; i < n; ++i) {
        bucket[i].clear();
      }
      for (int i = 1; i <= n; ++i) {
        bucket[a[i] / bucket_size].push_back(a[i]);
      }
      int p = 0;
      for (int i = 0; i < n; ++i) {
        insertion_sort(bucket[i]);
        for (int j = 0; j < bucket[i].size(); ++j) {
          a[++p] = bucket[i][j];
        }
      }
    }
    ```

=== "Python"
    ```python
    N = 100010
    w = n = 0
    a = [0] * N
    bucket = [[] for i in range(N)]
    
    
    def insertion_sort(A):
        for i in range(1, len(A)):
            key = A[i]
            j = i - 1
            while j >= 0 and A[j] > key:
                A[j + 1] = A[j]
                j -= 1
            A[j + 1] = key
    
    
    def bucket_sort():
        bucket_size = int(w / n + 1)
        for i in range(0, n):
            bucket[i].clear()
        for i in range(1, n + 1):
            bucket[int(a[i] / bucket_size)].append(a[i])
        p = 0
        for i in range(0, n):
            insertion_sort(bucket[i])
            for j in range(0, len(bucket[i])):
                a[p] = bucket[i][j]
                p += 1
    ```

## Tài liệu tham khảo & chú thích

[^ref1]: [Bucket sort - Wikipedia (tiếng Anh)](https://en.wikipedia.org/wiki/Bucket_sort#Average-case_analysis)
