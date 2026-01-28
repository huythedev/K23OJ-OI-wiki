author: abc1763613206, cesonic, Ir1d, MingqiHuang, xinchengo, xiaofu-15191, hsefz-ChenJunJie

## Giới thiệu

Thuật toán "heuristic" là gì?

Thuật toán heuristic là các kỹ thuật tối ưu hóa dựa trên kinh nghiệm và trực giác của con người.

Ví dụ điển hình nhất là kỹ thuật hợp nhất theo heuristic trong cấu trúc dữ liệu hợp nhất tập hợp (DSU), mã như sau:

```cpp
void merge(int x, int y) {
  int xx = find(x), yy = find(y);
  if (size[xx] < size[yy]) swap(xx, yy);
  fa[yy] = xx;
  size[xx] += size[yy];
}
```

Ở đây, với hai tập hợp có kích thước khác nhau, ta sẽ hợp tập nhỏ vào tập lớn, thay vì ngược lại.

Tại sao lại như vậy? Kích thước tập hợp có thể coi là chiều cao của cây (trong trường hợp thông thường), và việc hợp cây thấp vào cây cao sẽ giúp việc tìm cha nhanh hơn.

Việc cho cây thấp làm con của cây cao là một tối ưu gọi là hợp nhất theo heuristic.

## Nội dung thuật toán

Thuật toán "dsu on tree" (hợp nhất theo heuristic trên cây) là một kỹ thuật mạnh để giải các bài toán truy vấn offline trên cây, vừa dễ hiểu vừa dễ cài đặt, tốc độ ngang hoặc vượt nhiều thuật toán khác.

Xét bài toán sau: [Đếm số màu trên cây](https://www.luogu.com.cn/problem/U41492).

???+ note "Ví dụ dẫn nhập"
    Cho một cây $n$ đỉnh gốc tại $1$, mỗi đỉnh $u$ có màu $c_u$. Với mỗi đỉnh $u$, hỏi trong cây con gốc $u$ có bao nhiêu màu khác nhau.

    $n\le 2\times 10^5$.

![dsu-on-tree-1.png](./images/dsu-on-tree-1.svg)

Với dạng bài này, thường phải dùng các cấu trúc dữ liệu phức tạp (như cây phân đoạn lồng nhau), nhưng nếu cho phép truy vấn offline, liệu có cách đơn giản hơn?

## Quy trình

Vì cho phép truy vấn offline, ta có thể tiền xử lý để trả lời truy vấn trong $O(1)$.

Nếu duyệt brute-force, độ phức tạp là $O(n^2)$: với mỗi đỉnh, duyệt toàn bộ cây con, mỗi lần $O(n)$, tổng $O(n^2)$.

Nhận thấy đáp án của mỗi đỉnh phụ thuộc vào cây con của nó và chính nó, ta có thể tận dụng tính chất này.

Trước tiên, tiền xử lý kích thước cây con và xác định "con nặng" (giống như trong phân tích chuỗi nặng nhẹ - HLD), con nặng là con có cây con lớn nhất, quá trình này $O(n)$.

Gọi $cnt_i$ là số lần màu $i$ xuất hiện, $ans_u$ là đáp án của đỉnh $u$.

Duyệt một đỉnh $u$ theo các bước sau:

1.  Duyệt các con nhẹ (không phải con nặng), tính đáp án nhưng **không giữ lại ảnh hưởng lên mảng $cnt$**;
2.  Duyệt con nặng, **giữ lại ảnh hưởng lên mảng $cnt$**;
3.  Duyệt lại cây con của các con nhẹ, cộng thêm ảnh hưởng để tính đáp án cho $u$.

![dsu-on-tree-2.png](./images/dsu-on-tree-2.svg)

Hình trên minh họa quy trình.

Như vậy, mỗi đỉnh chỉ duyệt cây con nặng một lần, cây con nhẹ hai lần, rất tối ưu.

Sau khi thực hiện, ta có đáp án cho mọi cây con.

Tại sao không gộp bước 1 và 3? Vì mảng $cnt$ không thể dùng lại, nếu không sẽ tốn quá nhiều bộ nhớ, cần đảm bảo dùng $O(n)$ không gian.

Rõ ràng, nếu một đỉnh $u$ bị duyệt $x$ lần, thì con nặng bị duyệt $x$ lần, con nhẹ (nếu có) bị duyệt $2x$ lần.

Lưu ý: sau khi duyệt xong con nhẹ, phải reset mảng $cnt$.

## Chứng minh

Giống như phân tích chuỗi nặng nhẹ, định nghĩa cạnh nặng và nhẹ (cạnh nối tới con nặng là cạnh nặng, còn lại là cạnh nhẹ). Xem hình dưới, với cây $n$ đỉnh:

Số cạnh nhẹ từ gốc tới bất kỳ đỉnh không quá $\log n$. Gọi số cạnh nhẹ là $x$, kích thước cây con là $y$, rõ ràng cây con nối qua cạnh nhẹ nhỏ hơn một nửa cha (nếu lớn hơn thì là con nặng), nên $y<n/2^x$, tức $n>2^x$, nên $x<\log n$.

Nếu một đỉnh là con nặng của cha, thì cây con của nó lớn nhất trong các anh em, nên trên đường từ gốc tới nó, các cha nối qua cạnh nặng sẽ không duyệt lại nó, nên số lần bị duyệt là số cạnh nhẹ trên đường + 1 (vì chính nó cũng bị duyệt), tổng số lần bị duyệt là $\log n+1$, tổng thời gian $O(n(\log n+1))=O(n\log n)$, trả lời truy vấn $O(m)$.

![dsu-on-tree-3.png](./images/dsu-on-tree-3.svg)

*Các cạnh tô đậm là cạnh nặng, con nối qua cạnh nặng là con nặng*

## Tối ưu

Như đã nói, dsu on tree tận dụng khái niệm con nặng/nhẹ để tăng tốc. Ta cũng có thể dùng luôn thứ tự dfs từ phân tích chuỗi nặng nhẹ để chuyển đệ quy thành lặp, tối ưu thêm hằng số.

Thứ tự dfs có tính chất: cây con của một đỉnh là một đoạn liên tiếp trên thứ tự dfs. Do đó, có thể duyệt ngược thứ tự dfs, đảm bảo khi duyệt một đỉnh, cây con của nó đã được xử lý.

Thứ tự dfs từ phân tích chuỗi nặng nhẹ còn có tính chất: một chuỗi nặng là một đoạn liên tiếp trên thứ tự dfs. Khi duyệt ngược, nếu đỉnh là đầu chuỗi nặng, thì đỉnh tiếp theo không phải cha nó, nên phải reset ảnh hưởng; nếu không phải đầu chuỗi nặng, thì đỉnh trước là con nặng hoặc đã reset, nên có thể kế thừa ảnh hưởng. Dựa vào đó, dùng thứ tự dfs để cộng nhanh ảnh hưởng của các con nhẹ, ghi lại đáp án.

Quy trình này gọi là dsu on tree không đệ quy/lặp (hay dsu on tree theo thứ tự dfs). So với bản đệ quy, nó giảm chi phí gọi hàm và bộ nhớ, đặc biệt hiệu quả với cây nhiều chuỗi, tiết kiệm stack.

## Cài đặt

??? example "Mã tham khảo"
    === "Cài đặt đệ quy"
        ```cpp
        --8<-- "docs/graph/code/dsu-on-tree/dsu-on-tree_1.cpp"
        ```
    
    === "Cài đặt không đệ quy"
        ```cpp
        --8<-- "docs/graph/code/dsu-on-tree/dsu-on-tree_2.cpp"
        ```

## Ứng dụng

1.  Một số bài chính xác yêu cầu dùng dsu on tree

    Ví dụ [CF741D](http://codeforces.com/problemset/problem/741/D): Cho một cây, mỗi đỉnh mang ký tự từ 'a' đến 'v', mỗi truy vấn hỏi trong cây con có đường đi nào mà các ký tự trên đường đi (sắp xếp lại) tạo thành chuỗi đối xứng.

    Vì chuỗi đối xứng nên mỗi ký tự xuất hiện hai lần coi như không xuất hiện, tức là đường đi thỏa mãn **tối đa một ký tự xuất hiện lẻ**.

    Cách thường là dfs từng đỉnh, mỗi lần duyệt liệt kê mọi ký tự, kiểm tra số ký tự lẻ lớn hơn 1 thì loại, lấy đường đi dài nhất, độ phức tạp $O(n^2\log n)$, dùng dsu on tree tối ưu xuống $O(n\log^2n)$. Chi tiết xem phần mở rộng bên dưới.

2.  Một số bài có thể dùng dsu để "ăn gian"

    Có thể dùng dsu để giải một số bài cây phân đoạn (không có truy vấn cập nhật), dsu có độ phức tạp tốt hơn Mo's trên cây $O(n\sqrt{m})$.

## Bài tập

[CF600E Lomsat gelral](http://codeforces.com/problemset/problem/600/E)

Dịch đề: Mỗi đỉnh trên cây có màu, một màu chiếm cây con khi không có màu nào xuất hiện nhiều hơn nó trong cây con. Tính tổng các màu chiếm mỗi cây con.

[UOJ284 Gà chơi game vui vẻ](https://uoj.ac/problem/284)

[CF1709E XOR Tree](https://codeforces.com/contest/1709/problem/E)

## Tài liệu tham khảo/mở rộng

[Blog tác giả CF741D về dsu on tree](http://codeforces.com/blog/entry/44351)

[Blog giải thích chi tiết](http://codeforces.com/blog/entry/48871)
