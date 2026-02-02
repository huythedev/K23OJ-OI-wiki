Trang này giới thiệu ngắn gọn về thuật toán sắp xếp kiểu "đấu loại" (Tournament sort).

## Định nghĩa

Sắp xếp theo hình thức đấu loại (Tournament sort), còn gọi là sắp xếp chọn theo cây, là phiên bản tối ưu của [sắp xếp chọn](./selection-sort.md) và là một biến thể của [heap sort](./heap-sort.md) (đều dùng cây nhị phân hoàn chỉnh). Ý tưởng là dùng cấu trúc giống hàng đợi ưu tiên để chọn phần tử tiếp theo cần lấy.

## Nguồn gốc tên

Tên "tournament" xuất phát từ hình thức thi đấu loại trực tiếp. Nhiều tuyển thủ so tài từng cặp, kẻ thắng tiến vòng kế tiếp. Cách loại trực tiếp xác định người thắng nhưng người bị loại cuối cùng không nhất thiết là nhì — vì có thể thua người sau đã bị loại trước đó.

## Quá trình

Lấy ví dụ "cây đấu loại chọn nhỏ nhất":

![tournament-sort1](./images/tournament-sort1.png)

Các phần tử cần sắp xếp được đặt ở các lá. Các cạnh màu đỏ biểu diễn đường thắng của phần tử nhỏ hơn trong mỗi so sánh. Rõ ràng một lượt "đấu" chọn ra phần tử nhỏ nhất của một tập.

Mỗi lượt so sánh trên n phần tử tạo ra n/2 "người thắng" để vào vòng sau; với mỗi cặp, phần tử nhỏ hơn tiến. Nếu có phần tử lẻ không có cặp, nó tự động tiến vòng sau.

![tournament-sort2](./images/tournament-sort2.png)

Sau khi chọn được phần tử nhỏ nhất, ta loại bỏ nó (thường gán giá trị đó là +∞ — ý tương tự như trong [heap sort](./heap-sort.md)) và tổ chức lại "đấu" để chọn phần tử nhỏ thứ hai. Lặp lại đến khi các phần tử đều được lấy ra theo thứ tự tăng dần.

## Tính chất

### Tính ổn định

Tournament sort không phải là thuật toán ổn định.

### Độ phức tạp thời gian

Độ phức tạp tốt nhất, trung bình và xấu nhất đều là $O(n\log n)$. Thuật toán khởi tạo "tournament" trong $O(n)$, sau đó mỗi lần chọn một phần tử tốn $O(\log n)$.

### Độ phức tạp không gian

Sử dụng $O(n)$ bộ nhớ phụ.

## Triển khai

=== "C++"
    ```cpp
    int n, a[MAXN], tmp[MAXN << 1];
    
    int winner(int pos1, int pos2) {
      int u = pos1 >= n ? pos1 : tmp[pos1];
      int v = pos2 >= n ? pos2 : tmp[pos2];
      if (tmp[u] <= tmp[v]) return u;
      return v;
    }
    
    void creat_tree(int &value) {
      for (int i = 0; i < n; i++) tmp[n + i] = a[i];
      for (int i = 2 * n - 1; i > 1; i -= 2) {
        int k = i / 2;
        int j = i - 1;
        tmp[k] = winner(i, j);
      }
      value = tmp[tmp[1]];
      tmp[tmp[1]] = INF;
    }
    
    void recreat(int &value) {
      int i = tmp[1];
      while (i > 1) {
        int j, k = i / 2;
        if (i % 2 == 0)
          j = i + 1;
        else
          j = i - 1;
        tmp[k] = winner(i, j);
        i = k;
      }
      value = tmp[tmp[1]];
      tmp[tmp[1]] = INF;
    }
    
    void tournament_sort() {
      int value;
      creat_tree(value);
      for (int i = 0; i < n; i++) {
        a[i] = value;
        recreat(value);
      }
    }
    ```

=== "Python"
    ```python
    n = 0
    a = [0] * MAXN
    tmp = [0] * MAXN * 2
    
    
    def winner(pos1, pos2):
        u = pos1 if pos1 >= n else tmp[pos1]
        v = pos2 if pos2 >= n else tmp[pos2]
        if tmp[u] <= tmp[v]:
            return u
        return v
    
    
    def creat_tree():
        for i in range(0, n):
            tmp[n + i] = a[i]
        for i in range(2 * n - 1, 1, -2):
            k = int(i / 2)
            j = i - 1
            tmp[k] = winner(i, j)
        value = tmp[tmp[1]]
        tmp[tmp[1]] = INF
        return value
    
    
    def recreat():
        i = tmp[1]
        while i > 1:
            j = k = int(i / 2)
            if i % 2 == 0:
                j = i + 1
            else:
                j = i - 1
            tmp[k] = winner(i, j)
            i = k
        value = tmp[tmp[1]]
        tmp[tmp[1]] = INF
        return value
    
    
    def tournament_sort():
        value = creat_tree()
        for i in range(0, n):
            a[i] = value
            value = recreat()
    ```

## Liên kết ngoài

-   [Tournament sort - Wikipedia](https://en.wikipedia.org/wiki/Tournament_sort)
