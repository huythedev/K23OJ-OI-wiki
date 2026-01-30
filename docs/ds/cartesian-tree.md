author: sshwy, zhouyuyang2002, StudyingFather, Ir1d, ouuan, Enter-tainer

## Giới thiệu

Cây Descartes (Cartesian Tree) là một cấu trúc dữ liệu dạng cây nhị phân, trong đó mỗi nút được tạo thành bởi một cặp giá trị khóa nhị phân $(k, w)$. Yêu cầu là $k$ thỏa mãn tính chất của Cây tìm kiếm nhị phân (BST), còn $w$ thỏa mãn tính chất của Heap. Nếu các cặp giá trị khóa $k, w$ của cây Descartes được xác định, và các $k$ đôi một khác nhau, các $w$ cũng đôi một khác nhau, thì cấu trúc của cây Descartes là duy nhất. Như hình dưới:

![eg](./images/cartesian-tree1.png)

(Nguồn ảnh từ Wikipedia)

Cây Descartes trên hình tương đương với việc lấy giá trị phần tử của mảng làm khóa giá trị $w$, và lấy chỉ số mảng làm khóa giá trị $k$. Có thể thấy, khóa $k$ của cây này thỏa mãn tính chất BST, còn khóa $w$ thỏa mãn tính chất min-heap (heap gốc nhỏ). Đồng thời, dựa vào tính chất của cây tìm kiếm nhị phân, có thể thấy cây Descartes đặc biệt này thỏa mãn tính chất rằng chỉ số của các nút trong một cây con là một đoạn liên tục.

Trong thi đấu, khi sử dụng cây Descartes, người ta thường dùng chỉ số mảng làm khóa $k$ của cặp nhị phân, chỉ số mảng $k$ thỏa mãn tính chất BST.

Trong phần dưới, khi sử dụng $k, w$, mặc định $k$ thỏa mãn tính chất BST, $w$ thỏa mãn tính chất heap.

## Xây dựng cây Descartes bằng Stack đơn điệu

### Quá trình

Ta xét việc chèn lần lượt các phần tử theo thứ tự $k$ tăng dần vào cây Descartes hiện tại.

Đối với một cây Descartes, định nghĩa "chuỗi bên phải" là chuỗi tạo thành bằng cách đi từ nút gốc, luôn đi theo con phải, cho đến khi gặp nút lá. Khi chèn một nút mới, nút này chắc chắn sẽ nằm trên chuỗi bên phải. Vì việc chèn được thực hiện theo thứ tự $k$ tăng dần thỏa mãn tính chất BST, nên nút mới chèn vào chắc chắn nằm ở **phía ngoài cùng bên phải** của cây. Nút này không thể là con trái, và cũng không có con phải.

Vì vậy, ta thực hiện quy trình sau: so sánh $w$ của các nút trên chuỗi bên phải với $w$ của nút hiện tại $u$ từ dưới lên. Nếu tìm thấy một nút $x$ trên chuỗi bên phải thỏa mãn $w_x < w_u$, ta sẽ gắn $u$ làm con phải của $x$, và cây con bên phải ban đầu của $x$ sẽ trở thành cây con bên trái của $u$.

Phần được đóng khung đỏ trong hình là chuỗi bên phải mà ta luôn duy trì:

![build](./images/cartesian-tree2.png)

Rõ ràng mỗi số chỉ ra khỏi chuỗi bên phải nhiều nhất một lần (hoặc nói cách khác, mỗi nút tồn tại trên chuỗi bên phải trong một khoảng thời gian liên tục). Quá trình này có thể được duy trì bằng stack đơn điệu, stack duy trì các nút trên chuỗi bên phải của cây Descartes hiện tại. Khi một nút không còn nằm trên chuỗi bên phải nữa thì ta bật nó ra khỏi stack. Như vậy mỗi nút vào và ra khỏi stack nhiều nhất một lần, độ phức tạp là $O(n)$.

???+ note "Cây Descartes và Treap"
    Thực tế, Treap là một loại cây Descartes, chỉ khác là giá trị $w$ trong Treap hoàn toàn ngẫu nhiên. Treap có thuật toán xây dựng tuyến tính, nếu sắp xếp trước các khóa $k$, có thể sử dụng thuật toán stack đơn điệu trên để hoàn thành quá trình xây dựng, mặc dù rất ít khi được sử dụng theo cách này.

### Triển khai bằng C++

```cpp
// stk duy trì chỉ số trong dãy tương ứng với các nút của cây Descartes
for (int i = 1; i <= n; i++) {
  int k = top;  // top biểu thị stack top trước khi thao tác, k biểu thị stack top hiện tại
  while (k > 0 && w[stk[k]] > w[i]) k--;  // Duy trì các nút trên chuỗi bên phải
  if (k) rs[stk[k]] = i;  // Con phải của phần tử ở stack top. := Phần tử hiện tại
  if (k < top) ls[i] = stk[k + 1];  // Con trái của phần tử hiện tại := Phần tử vừa bị bật ra trước đó
  stk[++k] = i;                     // Phần tử hiện tại vào stack
  top = k;
}
```

## Ví dụ đề bài

???+ note "[HDU 1506. Largest Rectangle in a Histogram (Hình chữ nhật lớn nhất trong biểu đồ cột)](https://acm.hdu.edu.cn/showproblem.php?pid=1506)"
    Có $n$ vị trí, mỗi vị trí có chiều cao là $h_i$, tìm hình chữ nhật có diện tích lớn nhất. Như hình dưới:
    
    ![eg](./images/cartesian-tree3.png)
    
    Phần tô bóng chính là hình chữ nhật có diện tích lớn nhất trong hình.

??? note "Tư duy giải quyết"
    Cụ thể, ta lấy chỉ số làm khóa $k$, chiều cao $h_i$ làm khóa $w$ thỏa mãn tính chất min-heap, xây dựng một cây Descartes của $(i, h_i)$.
    
    Như vậy, ta duyệt qua từng nút $u$, coi $w_u$ (tức là chiều cao $h$ của nút $u$) làm chiều cao của hình chữ nhật lớn nhất có thể có. Do cây Descartes ta xây dựng thỏa mãn tính chất min-heap, nên chiều cao của các nút trong cây con của $u$ đều lớn hơn hoặc bằng $u$. Mà ta biết rằng các chỉ số của các nút trong cây con của $u$ là một đoạn liên tục. Vì vậy, ta chỉ cần biết kích thước của cây con, sau đó có thể tính diện tích hình chữ nhật lớn nhất trong đoạn này. Cập nhật câu trả lời bằng giá trị tính toán được từ mỗi điểm. Rõ ràng việc này có thể hoàn thành trong một lần DFS, do đó độ phức tạp là $O(n)$.

??? note "Tham khảo triển khai"
    ```cpp
    --8<-- "docs/ds/code/cartesian-tree/cartesian-tree_1.cpp"
    ```

## Tài liệu tham khảo

[Cây Descartes - Wikipedia](https://zh.wikipedia.org/wiki/%E7%AC%9B%E5%8D%A1%E5%B0%94%E6%A0%91)