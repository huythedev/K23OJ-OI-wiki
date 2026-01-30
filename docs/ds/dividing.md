tác giả: Xarfa

## Giới thiệu

Cây phân chia (Dividing Tree/Partition Tree) là một cấu trúc dữ liệu để giải quyết bài toán phần tử lớn thứ $K$ trong đoạn, hằng số và độ khó hiểu thấp hơn nhiều so với Cây Chủ Tịch (主席树 - Persistent Segment Tree). Đồng thời, Cây Phân Chia bám sát "lớn thứ $K$", nên là một cấu trúc dữ liệu dựa trên việc sắp xếp.

Kiến thức tiên quyết: [Cây Chủ Tịch](persistent-seg.md#主席树)

## Quá trình

### Xây dựng cây

Việc xây dựng Cây Phân Chia khá đơn giản, nhưng so với các cây khác thì nó phức tạp hơn một chút.

![](./images/dividing-1.svg)

Như hình vẽ, mỗi tầng có một mảng trông có vẻ vô thứ tự. Thực ra, tất cả các số được đánh dấu màu đỏ **sẽ được phân phối cho con trái**. Quy tắc phân phối là gì? Là so sánh với **trung vị của tầng này**, nếu nhỏ hơn hoặc bằng trung vị, thì phân sang trái, nếu không thì phân sang phải. Nhưng ở đây cần chú ý: không phải là nghiêm ngặt **nhỏ hơn hoặc bằng thì phân sang trái, nếu không thì phân sang phải**. Vì trung vị có thể có giá trị trùng nhau, và có liên quan đến tính chẵn lẻ của $N$. Đoạn mã dưới đây thể hiện một cách sử dụng khéo léo, mọi người có thể tham khảo mã.

Ta không thể sắp xếp lại toàn bộ mảng ở mỗi tầng, như vậy không nói về hằng số, ngay cả độ phức tạp lý thuyết cũng không thể chấp nhận. Ta nghĩ, tìm trung vị, sắp xếp một lần là đủ. Tại sao? Ví dụ, ta tìm trung vị của $l,r$, thực ra chính là `num[mid]` sau khi đã sắp xếp.

Hai mảng quan trọng:

`tree[log(N),N]`: Tức là cây, cần lưu tất cả các giá trị, độ phức tạp không gian $O(n\log n)$.
`toleft[log(N),n]`: Tức là số lượng phần tử được phân vào con trái từ $1$ đến $i$ ở mỗi tầng. Ở đây cần hiểu, đây là một mảng tổng tiền tố.

???+ note "Triển khai"
    ```pascal
    procedure Build(left,right,deep:longint); // left,right biểu thị điểm đầu cuối đoạn, deep là tầng thứ mấy
    var
      i,mid,same,ls,rs,flag:longint; // trong đó flag dùng để cân bằng số lượng hai bên
    begin
      if left=right then exit; // Xuống đến tầng cuối cùng
      mid:=(left+right) >> 1;
      same:=mid-left+1;
      for i:=left to right do 
        if tree[deep,i]<num[mid] then
          dec(same);
      
      ls:=left; // Con trỏ đầu tiên cho việc phân phối sang con trái
      rs:=mid+1; // Con trỏ đầu tiên cho việc phân phối sang con phải
      for i:=left to right do
      begin
        flag:=0;
        if (tree[deep,i]<num[mid])or((tree[deep,i]=num[mid])and(same>0)) then // Điều kiện phân phối sang trái
        begin
          flag:=1; tree[deep+1,ls]:=tree[deep,i]; inc(ls);
          if tree[deep,i]=num[mid] then // Cân bằng số lượng hai bên
            dec(same);
        end
        else
        begin
          tree[deep+1,rs]:=tree[deep,i]; inc(rs);
        end;
        toleft[deep,i]:=toleft[deep,i-1]+flag;
      end;
      Build(left,mid,deep+1); // Tiếp tục
      Build(mid+1,right,deep+1);
    end;
    ```

### Truy vấn

Ta nói qua về nội dung của Cây Chủ Tịch. Khi dùng Cây Chủ Tịch để tìm phần tử nhỏ thứ $K$ trong đoạn, ta lấy $K$ làm cơ sở, sang trái thì sang trái, sang phải thì trừ đi số lượng sang trái, trong Cây Phân Chia cũng tương tự.

Phần khó hiểu của truy vấn nằm ở **thu hẹp đoạn**. Hình dưới, truy vấn từ $3$ đến $7$, thì tầng tiếp theo chỉ cần truy vấn từ $2$ đến $3$. Tất nhiên, ta định nghĩa $[\text{left},\text{right}]$ là đoạn sau khi thu hẹp (đoạn mục tiêu), còn $[l,r]$ vẫn là đoạn của nút đang xét. Vậy tại sao phải đánh dấu đoạn mục tiêu? Bởi vì đó là **cơ sở để phán đoán đáp án nằm ở bên trái hay bên phải**.

![](./images/dividing-2.svg)

???+ note "Triển khai"
    ```pascal
    function Query(left,right,k,l,r,deep:longint):longint;
    var
      mid,x,y,cnt,rx,ry:longint;
    begin
      if left=right then // Viết thành l=r cũng không sao, vì đoạn mục tiêu chắc chắn có đáp án
        exit(tree[deep,left]);
      mid:=(l+r) >> 1;
      x:=toleft[deep,left-1]-toleft[deep,l-1]; // Số lượng đi sang con trái từ l đến left
      y:=toleft[deep,right]-toleft[deep,l-1]; // Số lượng đi sang con trái từ l đến right
      ry:=right-l-y; rx:=left-l-x; // ry là số lượng đi sang con phải từ l đến right, rx thì là từ l đến left
      cnt:=y-x; // Số lượng đi sang con trái của đoạn left đến right
      if cnt>=k then // Kiến thức cơ bản của Cây Chủ Tịch
        Query:=Query(l+x,l+y-1,k,l,mid,deep+1) // l+x là thu hẹp biên trái, l+y-1 là thu hẹp biên phải. Đối với hình trên, tức là bỏ qua nút 1 và 2.
      else
        Query:=Query(mid+rx+1,mid+ry+1,k-cnt,mid+1,r,deep+1); // Tương tự thu hẹp đoạn, nhưng là ở bên phải. Chú ý phải trừ đi cnt.
    end;
    ```

## Tính chất

Độ phức tạp thời gian: Mỗi lần truy vấn chỉ mất $O(\log n)$, $m$ lần truy vấn, tổng là $O(m\log n)$.

Độ phức tạp không gian: Chỉ cần lưu trữ $O(n\log n)$ số.

Kết quả thực nghiệm: Cây Chủ Tịch: $1482 \text{ms}$, Cây Phân Chia: $889 \text{ms}$. (Không đệ quy, hằng số khá nhỏ).

## Ứng dụng của Cây Phân Chia

Bài toán ví dụ: [Luogu P3157 [CQOI2011] Đảo ngược cặp số động (Dynamic Inversion)](https://www.luogu.com.cn/problem/P3157)

> Tóm tắt đề bài: Cho một hoán vị $n$ phần tử ($n\leq 10^5$), có $m$ lần truy vấn ($m\leq 5\times 10^4$), mỗi lần xóa một số trong hoán vị, hỏi số lượng đảo ngược cặp sau khi xóa số này.

Bài toán này có thể giải bằng CDQ trong thời gian $\Theta(n\log^2n)$ và không gian $\Theta(n)$, và hằng số của CDQ cũng rất tốt.

Nếu bài toán này đổi thành bắt buộc trực tuyến, thì thường dùng giải pháp Cây lồng Cây (Tree nested Tree) bằng Mảng Fenwick + Cây Chủ Tịch, độ phức tạp thời gian là $\Theta(n\log^2n)$, độ phức tạp không gian là $\Theta(n\log^2n)$, hằng số lớn hơn một chút, cũng có thể qua bài này.

Trong khi đó, sử dụng Cây Phân Chia có thể giải bài toán này trực tuyến trong thời gian $\Theta(n\log^2n)$ và không gian $\Theta(n\log n)$, đồng thời hằng số nhỏ hơn nhiều so với giải pháp Cây lồng Cây. (Tương đương với CDQ).

???+ warning "Lưu ý"
    Để tiện cho việc lập trình, bài viết này dựa vào giá trị trung vị của vị trí để chia mảng lớn thành hai mảng nhỏ, tức là Cây Phân Chia dưới đây tương đương với quá trình sắp xếp trộn (Merge Sort), chứ không phải Sắp xếp Nhanh (Quick Sort). Mảng lớn nhất ở tầng trên cùng là mảng có thứ tự, tầng dưới cùng là mảng gốc.

Đối với mỗi nút trong Cây Phân Chia, ta gọi nó là nút phải (right node) nếu và chỉ nếu ở tầng tiếp theo nó sẽ được phân vào con phải, tức là các số có vị trí tương đối về sau trong mảng gốc. Tương tự có thể định nghĩa nút trái (left node). Nếu ở tầng trên cùng sắp xếp thành có thứ tự, giống như tìm số cặp nghịch đảo trong sắp xếp trộn, ta có thể thấy số cặp nghịch đảo của một mảng bằng tổng số lượng nút phải đứng trước mỗi nút trái.

Xem xét thao tác xóa. Xóa một nút trái sẽ làm giảm số cặp nghịch đảo của toàn bộ mảng bằng số lượng nút phải đứng trước nó, trong khi xóa một nút phải sẽ làm giảm số lượng nút trái đứng sau nó. Vậy có thể cân nhắc duy trì động "số lượng nút phải đứng trước mỗi nút trái" và "số lượng nút trái đứng sau mỗi nút phải". Điều này có thể dùng Mảng Fenwick để duy trì đơn giản.

Cần lưu ý rằng, khi sử dụng Mảng Fenwick để duy trì, chỉ có thể tính toán sự đóng góp trong cùng một khối ở Cây Phân Chia, không thể nhảy ra khỏi khối. Đối với Mảng Fenwick, có một cách xử lý khá khéo léo.

Xét phạm vi chỉ số của mỗi khối trong Cây Phân Chia luôn có dạng $[c\times 2^k+1,(c+1)\times 2^k]$. Liệt kê như sau (do mã nguồn sẽ không liên quan đến việc xử lý tầng thấp nhất của Cây Phân Chia, nên chỉ liệt kê đến tầng áp chót):

    [0001 0010] [0011 0100] [0101 0110] [0111 1000] [1001 1010] [1011 1100] [1101 1110] [1111 10000]  lev=1
    [0001 0010 0011 0100]   [0101 0110 0111 1000]   [1001 1010 1011 1100]   [1101 1110 1111 10000]    lev=2
    [0001 0010 0011 0100 0101 0110 0111 1000]       [1001 1010 1011 1100 1101 1110 1111 10000]        lev=3
    [0001 0010 0011 0100 0101 0110 0111 1000 1001 1010 1011 1100 1101 1110 1111 10000]                lev=4

Nhớ lại nguyên lý của Mảng Fenwick, khi nhảy lên, ta luôn có `x += lowbit(x)`. Nếu khi nhảy lên có thể đảm bảo không nhảy ra khỏi khối, thì có thể đảm bảo chỉ ảnh hưởng đến giá trị bên trong khối. Truy vấn đi lên cũng tương tự.

Còn nhảy xuống thì xử lý hoàn toàn khác. Nếu sử dụng chỉ số 0-index, phạm vi chỉ số của mỗi khối có dạng $[c\times 2^k,(c+1)\times 2^k)$. Vậy, chỉ cần dịch phải một lượng $k$ bit là có thể có được nó thuộc khối nào. Khi nhảy xuống, luôn kiểm tra xem có nhảy ra khỏi khối hay không.

Cần lưu ý, Mảng Fenwick được cài đặt theo cách này sẽ truy cập chỉ số lớn nhất là lũy thừa của 2 gần $n$ nhất, vì vậy kích thước mảng không thể chỉ là $n$.

Do cần sửa đổi ở $\log n$ tầng, thời gian sửa đổi ở tầng thứ $k$ là $\Theta(k)$, độ phức tạp thời gian cuối cùng là $\Theta(n\log n+m\log^2n)$.

Kèm theo mã nguồn:

```cpp
--8<-- "docs/ds/code/dividing/dividing_1.cpp"
```

## Hậu ký

Tham khảo bài viết: [Cổng dẫn](https://blog.csdn.net/littlewhite520/article/details/70250722).