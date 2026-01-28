tác giả: HeRaNO, Xeonacid, AzurIce

## Cấu trúc

Bắt đầu từ cấu trúc của một Heap nhị phân (Binary Heap): nó là một cây nhị phân, cụ thể là một cây nhị phân đầy đủ (complete binary tree), trong đó mỗi nút lưu trữ một phần tử (hoặc một trọng số).

Tính chất của Heap: trọng số của cha không nhỏ hơn trọng số của con (Max-heap). Tương tự, chúng ta có thể định nghĩa Min-heap. Bài viết này lấy Max-heap làm ví dụ.

Theo tính chất của Heap, gốc của cây lưu trữ giá trị lớn nhất (thao tác `getmax` đã được giải quyết).

## Quá trình

### Thao tác Chèn (Insert)

Thao tác chèn là thêm một phần tử vào Heap nhị phân, phải đảm bảo sau khi chèn nó vẫn là một cây nhị phân đầy đủ.

Phương pháp đơn giản nhất là chèn vào sau lá ngoài cùng bên phải của tầng cuối cùng.

Nếu tầng cuối cùng đã đầy, hãy thêm một tầng mới.

Sau khi chèn, có thể tính chất Heap không còn được thỏa mãn?

**Điều chỉnh lên (Up-shift/Up-heap)**: Nếu trọng số của nút này lớn hơn trọng số của cha nó, hãy hoán đổi chúng, lặp lại quá trình này cho đến khi không còn thỏa mãn hoặc đến gốc.

Có thể chứng minh rằng sau khi chèn và điều chỉnh lên, không có nút nào khác vi phạm tính chất Heap.

Độ phức tạp thời gian của điều chỉnh lên là $O(\log n)$.

![Thao tác chèn của Heap nhị phân](./images/binary_heap_insert.svg)

### Thao tác Xóa (Delete)

Thao tác xóa là xóa phần tử lớn nhất trong Heap, tức là xóa nút gốc.

Tuy nhiên, nếu xóa trực tiếp, nó sẽ biến thành hai Heap riêng biệt, khó xử lý.

Vì vậy, hãy cân nhắc quá trình ngược lại của thao tác chèn, tìm cách di chuyển nút gốc đến vị trí nút cuối cùng, sau đó xóa trực tiếp.

Thực tế, chúng ta thường làm là: hoán đổi trực tiếp nút gốc với nút cuối cùng.

Sau đó xóa nút gốc (lúc này đang ở vị trí nút cuối cùng), nhưng nút gốc mới có thể không thỏa mãn tính chất Heap...

**Điều chỉnh xuống (Down-shift/Down-heap)**: Trong các con của nút đó, tìm con lớn nhất, hoán đổi với nút đó, lặp lại quá trình này cho đến tầng đáy.

Có thể chứng minh rằng sau khi xóa và điều chỉnh xuống, không có nút nào khác vi phạm tính chất Heap.

Độ phức tạp thời gian là $O(\log n)$.

### Tăng trọng số của một nút

Rõ ràng, sau khi sửa đổi trực tiếp, chỉ cần thực hiện điều chỉnh lên một lần, độ phức tạp thời gian là $O(\log n)$.

## Cài đặt

Chúng ta nhận thấy rằng các thao tác giới thiệu ở trên chủ yếu dựa vào hai hạt nhân: điều chỉnh lên và điều chỉnh xuống.

Cân nhắc sử dụng một dãy $h$ để biểu diễn Heap. Hai con của $h_i$ lần lượt là $h_{2i}$ và $h_{2i+1}$, nút $1$ là nút gốc:

![Cấu trúc Heap của h](./images/binary-heap-array.svg)

Mã tham khảo:

```cpp
void up(int x) {
  while (x > 1 && h[x] > h[x / 2]) {
    std::swap(h[x], h[x / 2]);
    x /= 2;
  }
}

void down(int x) {
  while (x * 2 <= n) {
    int t = x * 2;
    if (t + 1 <= n && h[t + 1] > h[t]) t++;
    if (h[t] <= h[x]) break;
    std::swap(h[x], h[t]);
    x = t;
  }
}
```

### Xây dựng Heap (Build Heap)

Xét bài toán: từ một Heap rỗng, chèn $n$ phần tử, không quan tâm thứ tự.

Chèn từng cái một mất thời gian $O(n \log n)$, có cách nào tốt hơn không?

#### Cách 1: Sử dụng điều chỉnh lên

Bắt đầu từ gốc, thực hiện theo thứ tự BFS.

```cpp
void build_heap_1() {
  for (int i = 1; i <= n; i++) up(i);
}
```

Tại sao làm vậy: Đối với nút ở tầng thứ $k$, độ phức tạp điều chỉnh lên là $O(k)$ chứ không phải $O(\log n)$.

Tổng độ phức tạp: $\log 1 + \log 2 + \cdots + \log n = \Theta(n \log n)$.

#### Cách 2: Sử dụng điều chỉnh xuống

Lần này đổi hướng tư duy, bắt đầu từ các lá, thực hiện điều chỉnh xuống từng cái một.

```cpp
void build_heap_2() {
  for (int i = n; i >= 1; i--) down(i);
}
```

Một cách hiểu khác, mỗi lần "hợp nhất" hai Heap đã được điều chỉnh xong, điều này chứng minh tính đúng đắn.

Lưu ý độ phức tạp của điều chỉnh xuống là $O(\log n - k)$, ngoài ra các nút lá không cần điều chỉnh, do đó có thể bắt đầu điều chỉnh từ vị trí khoảng $n/2$ trong dãy để giảm bớt hằng số nhưng không ảnh hưởng độ phức tạp.

???+ note "Chứng minh"
    $$
    \begin{aligned}
    \text{Tổng độ phức tạp} & = n \log n - \log 1 - \log 2 - \cdots - \log n \\
    & \leq n \log n - 0 \times 2^0 - 1 \times 2^1 -\cdots - (\log n - 1) \times \frac{n}{2} \\\
    & = n \log n - (n-1) - (n-2) - (n-4) - \cdots - (n-\frac{n}{2}) \\
    & = n \log n - n \log n + 1 + 2 + 4 + \cdots + \frac{n}{2} \\
    & = n - 1 \\ &  = O(n)
    \end{aligned}
    $$

Sở dĩ có thể xây Heap trong $O(n)$ là vì tính chất Heap rất yếu, Heap nhị phân không phải là duy nhất.

## Ứng dụng

### Heap đối đỉnh (Running Median)

??? note "[SPOJ RMID2 - Running Median Again](https://www.spoj.com/problems/RMID2/)"
    Duy trì một dãy, hỗ trợ hai thao tác:
    
    1.  Chèn một phần tử vào dãy.
    2.  Xuất và xóa số trung vị hiện tại của dãy (nếu độ dài dãy là chẵn, xuất số trung vị nhỏ hơn).

Bài toán này có thể được trừu tượng hóa thành: duy trì động số lớn thứ $k$ trên một dãy, giá trị $k$ có thể thay đổi.

Đối với loại bài toán này, chúng ta có thể sử dụng kỹ thuật **Heap đối đỉnh** để giải quyết (tránh được sự rắc rối khi viết cây phân đoạn trọng số hoặc BST).

Heap đối đỉnh gồm một Max-heap và một Min-heap. Min-heap duy trì các giá trị lớn tức là $k$ số lớn nhất (bao gồm số thứ $k$), Max-heap duy trì các giá trị nhỏ tức là các số còn lại nhỏ hơn số thứ $k$.

Cấu trúc dữ liệu này hỗ trợ các thao tác sau:

-   Duy trì: Khi kích thước Min-heap nhỏ hơn $k$, liên tục lấy phần tử đỉnh Max-heap chèn vào Min-heap cho đến khi kích thước Min-heap bằng $k$; khi kích thước Min-heap lớn hơn $k$, liên tục lấy phần tử đỉnh Min-heap chèn vào Max-heap cho đến khi bằng $k$;
-   Chèn phần tử: Nếu phần tử chèn vào lớn hơn hoặc bằng đỉnh Min-heap, chèn vào Min-heap, ngược lại chèn vào Max-heap, sau đó duy trì Heap đối đỉnh;
-   Truy vấn số lớn thứ $k$: Đỉnh Min-heap chính là kết quả;
-   Xóa số lớn thứ $k$: Xóa đỉnh Min-heap, sau đó duy trì Heap đối đỉnh;
-   Thay đổi giá trị $k$ ($+1/-1$): Duy trì trực tiếp Heap đối đỉnh theo giá trị $k$ mới.

Hiển nhiên, độ phức tạp truy vấn số lớn thứ $k$ là $O(1)$. Các thao tác khác có độ phức tạp $O(\log n)$.

??? note "Mã tham khảo"
    ```cpp
    --8<-- "docs/ds/code/binary-heap/binary-heap_1.cpp"
    ```

### Bài tập tự luyện

-   [SPOJ RMID - Running Median](https://www.spoj.com/problems/RMID)
-   [Luogu P1801 Hộp đen (Black Box)](https://www.luogu.com.cn/problem/P1801)