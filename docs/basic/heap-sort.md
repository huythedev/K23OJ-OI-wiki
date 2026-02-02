Trang này sẽ giới thiệu ngắn gọn về thuật toán Heap Sort.

## Định nghĩa

Heap sort (Tiếng Anh: Heapsort) là thuật toán sắp xếp sử dụng cấu trúc dữ liệu [cây nhị phân dạng heap](../ds/binary-heap.md). Heap sort hoạt động trên mảng.

## Quá trình

Bản chất của heap sort là một dạng selection sort dựa trên heap.

### Sắp xếp

Trước hết xây dựng một max-heap (đỉnh là phần tử lớn nhất), sau đó lấy phần tử đỉnh của heap làm giá trị lớn nhất, hoán đổi với phần tử ở cuối mảng và duy trì tính chất heap của phần còn lại;

Tiếp tục lấy đỉnh heap làm giá trị lớn thứ hai, hoán đổi với phần tử ở vị trí kế cuối mảng và duy trì tính chất heap của phần còn lại;

Lặp như vậy, sau $n-1$ lần thao tác, toàn bộ mảng sẽ được sắp xếp.

### Xây dựng binary heap trên mảng

Sắp xếp các nút theo thứ tự các tầng từ gốc xuống, lưu trong mảng.

Vì vậy, với nút có chỉ số `i` trong mảng, chỉ số của nút cha, nút con trái và nút con phải như sau:

```cpp
iParent(i) = (i - 1) / 2;
iLeftChild(i) = 2 * i + 1;
iRightChild(i) = 2 * i + 2;
```

## Tính chất

### Tính ổn định

Cũng như selection sort, vì có các phép hoán vị nên đây không phải là thuật toán ổn định.

### Độ phức tạp thời gian

Độ phức tạp tốt nhất, trung bình và tồi nhất của heap sort đều là $O(n\log n)$.

### Độ phức tạp không gian

Vì có thể xây heap trực tiếp trên mảng đầu vào nên đây là thuật toán in-place.

## Cài đặt

=== "C++"
    ```cpp
    void sift_down(int arr[], int start, int end) {
      // Tính chỉ số của nút cha và nút con
      int parent = start;
      int child = parent * 2 + 1;
      while (child <= end) {  // Chỉ so sánh khi chỉ số nút con còn trong phạm vi
        // So sánh hai nút con, chọn nút lớn hơn
        if (child + 1 <= end && arr[child] < arr[child + 1]) child++;
        // Nếu nút cha lớn hơn hoặc bằng nút con thì đã cân chỉnh xong, thoát hàm
        if (arr[parent] >= arr[child])
          return;
        else {  // Nếu không thì hoán đổi cha và con, rồi tiếp tục so sánh từ vị trí con
          swap(arr[parent], arr[child]);
          parent = child;
          child = parent * 2 + 1;
        }
      }
    }
    
    void heap_sort(int arr[], int len) {
      // Bắt đầu từ cha của nút cuối cùng và sift down để hoàn thành heapify
      for (int i = (len - 1 - 1) / 2; i >= 0; i--) sift_down(arr, i, len - 1);
      // Hoán đổi phần tử đầu với phần tử cuối vùng chưa sắp, rồi sift_down vùng còn lại, lặp đến khi xong
      for (int i = len - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        sift_down(arr, 0, i - 1);
      }
    }
    ```

=== "Python"
    ```python
    def sift_down(arr, start, end):
        # Tính chỉ số của nút cha và nút con
        parent = int(start)
        child = int(parent * 2 + 1)
        while child <= end:  # Chỉ so sánh khi chỉ số nút con còn trong phạm vi
            # So sánh hai nút con, chọn nút lớn hơn
            if child + 1 <= end and arr[child] < arr[child + 1]:
                child += 1
            # Nếu nút cha lớn hơn hoặc bằng nút con thì đã cân chỉnh xong, thoát hàm
            if arr[parent] >= arr[child]:
                return
            else:  # Nếu không thì hoán đổi cha và con, rồi tiếp tục so sánh từ vị trí con
                arr[parent], arr[child] = arr[child], arr[parent]
                parent = child
                child = int(parent * 2 + 1)
    
    
    def heap_sort(arr, len):
        # Bắt đầu từ cha của nút cuối cùng và sift down để hoàn thành heapify
        i = (len - 1 - 1) / 2
        while i >= 0:
            sift_down(arr, i, len - 1)
            i -= 1
        # Hoán đổi phần tử đầu với phần tử cuối vùng chưa sắp, rồi sift_down vùng còn lại, lặp đến khi xong
        i = len - 1
        while i > 0:
            arr[0], arr[i] = arr[i], arr[0]
            sift_down(arr, 0, i - 1)
            i -= 1
    ```

## Liên kết ngoài

-   [堆排序 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E5%A0%86%E6%8E%92%E5%BA%8F)
