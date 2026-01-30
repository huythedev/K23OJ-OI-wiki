Heap là một cây, trong đó mỗi nút có một giá trị khóa, và giá trị khóa của mỗi nút đều lớn hơn hoặc bằng/nhỏ hơn hoặc bằng giá trị khóa của nút cha nó.

Heap mà giá trị khóa của mỗi nút đều **lớn hơn hoặc bằng** giá trị khóa của nút cha được gọi là **min-heap** (hay **đống cực tiểu**). Ngược lại, heap mà giá trị khóa của mỗi nút đều **nhỏ hơn hoặc bằng** giá trị khóa của nút cha được gọi là **max-heap** (hay **đống cực đại**). [`priority_queue` trong STL](../lang/csl/container-adapter.md#优先队列) thực chất là một max-heap.

(Min-heap) Heap chủ yếu hỗ trợ các thao tác: chèn một số, truy vấn giá trị nhỏ nhất, xóa giá trị nhỏ nhất, hợp nhất hai heap, và giảm giá trị của một phần tử.

Một số heap mạnh mẽ (heap có thể hợp nhất - meldable heap) còn có thể (hiệu quả) hỗ trợ các thao tác như `merge` (hợp nhất).

Một số heap mạnh mẽ hơn nữa còn hỗ trợ tính **bền vững** (persistence), tức là cho phép truy vấn hoặc thao tác trên bất kỳ phiên bản lịch sử nào, tạo ra phiên bản mới.

## Phân loại Heap

| Thao tác $\setminus$ Cấu trúc dữ liệu[^ref4] | Heap Cặp đôi (Pairing Heap) | Heap Nhị phân (Binary Heap) | Leftist Heap (Heap Lệch Trái) | Heap Nhị phân Thức (Binomial Heap) | Heap Fibonacci (Fibonacci Heap) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Chèn (insert) | $O(1)$ | $O(\log n)$ | $O(\log n)$ | $O(\log n)$[^ref1] | $O(1)$ |
| Truy vấn Min (find-min) | $O(1)$ | $O(1)$ | $O(1)$ | $O(1)$[^ref2][^ref3] | $O(1)$ |
| Xóa Min (delete-min) | $O(\log n)$[^ref3] | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(\log n)$[^ref3] |
| Hợp nhất (merge) | $O(1)$ | $O(n)$ | $O(\log n)$ | $O(\log n)$ | $O(1)$ |
| Giảm giá trị phần tử (decrease-key) | $o(\log n)$ (Lower bound $\Omega(\log \log n)$, Upper bound $O(2^{2\sqrt{\log \log n}})$)[^ref3] | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(1)$[^ref3] |
| Có hỗ trợ tính bền vững không | $\times$ | $\checkmark$ | $\checkmark$ | $\checkmark$ | $\times$ |

[^ref1]: Độ phức tạp cho một lần chèn là $O(\log n)$, nhưng khi có $k$ lần chèn liên tiếp, có thể tạo một binomial heap chỉ chứa các phần tử cần chèn, sau đó hợp nhất nó với binomial heap ban đầu. Độ phức tạp trung bình là $O(1)$.

[^ref2]: Có thể lưu trữ một con trỏ tới phần tử nhỏ nhất, và sửa đổi con trỏ này khi thực hiện các thao tác khác, để có thể truy vấn trong độ phức tạp $O(1)$.

[^ref3]: Độ phức tạp là độ phức tạp trung bình (amortized complexity).

[^ref4]: Bảng được lấy từ [Wikipedia](https://en.wikipedia.org/wiki/Priority_queue#Summary_of_running_times)

Theo thói quen, khi không có giới hạn rõ ràng, thuật ngữ "heap" thường ám chỉ **Binary Heap** (Heap Nhị phân).