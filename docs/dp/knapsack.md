Kiến thức chuẩn bị: [Giới thiệu về Quy hoạch động](./index.md).

## Giới thiệu

Trước khi đi vào chi tiết "DP cái túi" là gì, hãy cùng xem bài toán ví dụ sau:

???+ note "[「USACO07 DEC」Charm Bracelet](https://www.luogu.com.cn/problem/P2871)"
    Tóm tắt đề bài: Có $n$ đồ vật và một cái túi có dung lượng $W$. Mỗi đồ vật có trọng lượng $w_{i}$ và giá trị $v_{i}$. Yêu cầu chọn một số đồ vật cho vào túi sao cho tổng giá trị là lớn nhất và tổng trọng lượng không vượt quá dung lượng túi.

Trong ví dụ trên, vì mỗi vật thể chỉ có hai trạng thái khả thi (chọn hoặc không chọn), tương ứng với $0$ và $1$ trong hệ nhị phân, nên loại bài toán này được gọi là "Bài toán cái túi 0-1".

## Cái túi 0-1 (0-1 Knapsack)

### Giải thích

Các điều kiện đã biết bao gồm: trọng lượng $w_{i}$ của đồ vật thứ $i$, giá trị $v_{i}$, và tổng dung lượng túi $W$.

Gọi trạng thái DP $f_{i,j}$ là tổng giá trị tối đa có thể đạt được khi chỉ xét $i$ đồ vật đầu tiên với túi có dung lượng $j$.

Xét phép chuyển trạng thái: Giả sử hiện tại đã xử lý xong mọi trạng thái của $i-1$ đồ vật đầu tiên. Đối với đồ vật thứ $i$:
- Nếu **không chọn** nó vào túi: dung lượng còn lại không đổi, tổng giá trị không đổi. Giá trị tối đa là $f_{i-1,j}$.
- Nếu **chọn** nó vào túi: dung lượng còn lại giảm đi $w_{i}$, tổng giá trị tăng thêm $v_{i}$. Giá trị tối đa là $f_{i-1,j-w_{i}}+v_{i}$.

Từ đó ta có phương trình chuyển trạng thái:

$$
f_{i,j}=\max(f_{i-1,j},f_{i-1,j-w_{i}}+v_{i})
$$

Nếu trực tiếp dùng mảng hai chiều sẽ dễ dẫn đến quá giới hạn bộ nhớ (MLE). Ta có thể dùng mảng cuốn để tối ưu.

Vì $f_i$ chỉ phụ thuộc vào $f_{i-1}$, ta có thể bỏ chiều thứ nhất, dùng $f_j$ để biểu thị giá trị lớn nhất khi túi có dung lượng $j$ tại thời điểm xử lý đồ vật hiện tại:

$$
f_j=\max \left(f_j,f_{j-w_i}+v_i\right)
$$

**Hãy ghi nhớ và hiểu rõ phương trình này, vì hầu hết các bài toán cái túi đều được suy luận dựa trên cơ sở này.**

### Cài đặt

Một điểm cực kỳ quan trọng là tránh viết **đoạn code sai** sau đây:

=== "C++"
    ```cpp
    for (int i = 1; i <= n; i++)
      for (int l = 0; l <= W - w[i]; l++)
        f[l + w[i]] = max(f[l] + v[i], f[l + w[i]]);
    ```

Đoạn code trên sai ở đâu? Sai ở **thứ tự duyệt**.

Nếu duyệt xuôi, khi tính $f_{j}$, giá trị $f_{j-w_i}$ đã được cập nhật bởi đồ vật $i$ hiện tại rồi. Điều này tương đương với việc một đồ vật có thể được chọn nhiều lần, không đúng với bài toán 0-1 (đây thực tế là cách giải bài toán cái túi hoàn toàn).

Để tránh điều này, ta duyệt ngược từ $W$ về $w_i$. Như vậy khi tính $f_j$, trạng thái $f_{j-w_i}$ vẫn là giá trị của bước $i-1$.

Đoạn code chuẩn:

=== "C++"
    ```cpp
    for (int i = 1; i <= n; i++)
      for (int l = W; l >= w[i]; l--) f[l] = max(f[l], f[l - w[i]] + v[i]);
    ```

=== "Python"
    ```python
    for i in range(1, n + 1):
        for l in range(W, w[i] - 1, -1):
            f[l] = max(f[l], f[l - w[i]] + v[i])
    ```

??? note "Mã nguồn ví dụ"
    ```cpp
    --8<-- "docs/dp/code/knapsack/knapsack_1.cpp"
    ```

## Cái túi hoàn toàn (Complete Knapsack)

### Giải thích

Mô hình này tương tự cái túi 0-1, điểm khác biệt duy nhất là mỗi đồ vật có thể chọn **vô hạn lần**.

### Quá trình

Phương trình chuyển trạng thái:
$$
f_{i,j}=\max(f_{i-1,j},f_{i,j-w_i}+v_i)
$$

Khác với túi 0-1, ở đây ta dùng $f_{i, j-w_i}$ để chuyển trạng thái. Điều này có nghĩa là sau khi chọn đồ vật $i$ một lần, ta vẫn có thể tiếp tục chọn nó thêm lần nữa.

Khi tối ưu không gian, vòng lặp dung lượng túi sẽ là **duyệt xuôi**:

=== "C++"
    ```cpp
    for (int i = 1; i <= n; i++)
      for (int l = w[i]; l <= W; l++) f[l] = max(f[l], f[l - w[i]] + v[i]);
    ```

??? note "[「Luogu P1616」Hái thuốc điên cuồng](https://www.luogu.com.cn/problem/P1616)"
    (Mã nguồn ví dụ tương tự túi 0-1 nhưng duyệt xuôi chiều dung lượng)

## Cái túi đa tạp (Bounded Knapsack)

Khác với hai loại trên, mỗi đồ vật $i$ có số lượng giới hạn là $k_i$ cái.

Một cách đơn giản là chuyển nó về túi 0-1: coi $k_i$ đồ vật giống nhau là $k_i$ vật phẩm riêng biệt. Độ phức tạp: $O(W \sum k_i)$.

### Tối ưu bằng phân tách nhị phân

Chúng ta có thể chia $k_i$ vật phẩm thành các nhóm có số lượng là $1, 2, 4, 8, \dots, 2^p$ và một nhóm dư cuối cùng sao cho tổng các nhóm bằng $k_i$.
Bất kỳ số lượng nào từ $0$ đến $k_i$ đều có thể được biểu diễn bằng tổng của các nhóm này. Sau đó ta giải bài toán như túi 0-1 với các "nhóm" này.

Độ phức tạp: $O(W \sum \log k_i)$.

### Tối ưu bằng hàng đợi đơn điệu

Xem chi tiết tại [Tối ưu hàng đợi/ngăn xếp đơn điệu](./opt/monotonous-queue-stack.md).

## Cái túi hỗn hợp (Mixed Knapsack)

Là sự kết hợp của 0-1, hoàn toàn và đa tạp. Với mỗi loại vật phẩm, ta dùng đoạn code tương ứng:

```plain
for (với mỗi loại vật phẩm) {
  if (là túi 0-1)
    áp dụng code 0-1;
  else if (là túi hoàn toàn)
    áp dụng code hoàn toàn;
  else if (là túi đa tạp)
    áp dụng code đa tạp (phân tách nhị phân);
}
```

## Cái túi hai chiều (Two-dimensional Cost Knapsack)

Mỗi vật phẩm tiêu tốn hai loại tài nguyên (ví dụ: tiền và thời gian). Ta chỉ cần thêm một chiều vào mảng DP:
`dp[i][j]` là giá trị tối đa với chi phí 1 là $i$ và chi phí 2 là $j$.

## Cái túi chia nhóm (Grouped Knapsack)

Vật phẩm được chia thành các nhóm. Trong mỗi nhóm, bạn chỉ được chọn **tối đa một** vật phẩm.

Thứ tự vòng lặp chuẩn:
1. Duyệt qua từng nhóm.
2. Duyệt ngược dung lượng túi.
3. Duyệt qua từng vật phẩm trong nhóm hiện tại.

## Cái túi có phụ thuộc (Dependency Knapsack)

Để mua vật phẩm phụ, bạn phải mua vật phẩm chính tương ứng. Thường được giải quyết bằng cách coi một vật chính cùng các vật phụ của nó là một nhóm vật phẩm trong túi chia nhóm (xét mọi tổ hợp khả thi).

## Các biến thể khác

- **Xuất phương án**: Dùng mảng phụ ghi lại quyết định (chọn hay không) tại mỗi trạng thái.
- **Tính số phương án**: Thay `max` bằng phép cộng `+`.
- **Tính số phương án tối ưu**: Vừa duy trì giá trị tối đa vừa duy trì số cách đạt được giá trị đó.
- **Tìm nghiệm tốt thứ k**: Thêm một chiều để lưu $k$ giá trị tốt nhất tại mỗi trạng thái và thực hiện hợp nhất danh sách.

## Tài liệu tham khảo

- [Chín chương về bài toán cái túi - Thôi Thiêm Dực](https://github.com/tianyicui/pack).