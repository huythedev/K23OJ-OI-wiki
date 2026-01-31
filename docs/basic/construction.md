author: leoleoasd, yzxoi, Estrella-Explore

Trang này sẽ giới thiệu ngắn gọn về dạng bài **bài toán xây dựng (bài toán cấu trúc/constructive)**.

## Dẫn nhập

Bài toán xây dựng là một dạng bài rất phổ biến trong các kỳ thi.

Về mặt hình thức, đáp án của bài toán thường có một quy luật nhất định, giúp cho dù kích thước bài toán tăng rất nhanh, ta vẫn có thể dễ dàng tìm ra đáp án.

Điều này đòi hỏi khi giải, bạn phải suy nghĩ về ảnh hưởng của việc tăng kích thước bài toán lên đáp án, và liệu ảnh hưởng đó có thể khái quát hóa được không. Ví dụ, khi thiết kế quy hoạch động, cần xem xét việc chuyển từ một trạng thái sang trạng thái kế tiếp sẽ ảnh hưởng gì đến đáp án.

## Đặc điểm

Một đặc điểm nổi bật của bài toán xây dựng là **tính tự do cao**: có thể có rất nhiều cách xây dựng đáp án, nhưng thường sẽ có một cách đơn giản nhất thỏa mãn yêu cầu đề bài. Nhìn qua thì có vẻ dễ hơn, nhưng chính sự tự do này lại khiến nhiều bạn không biết bắt đầu từ đâu.

Đặc điểm thứ hai là **hình thức linh hoạt, đa dạng**. Không có một công thức hay khuôn mẫu chung nào để giải mọi bài xây dựng, thậm chí rất khó tìm ra điểm chung về mặt ý tưởng.

## Các ví dụ

Dưới đây là một số ví dụ giúp bạn hiểu hơn về tư duy xây dựng, đồng thời gợi mở ý tưởng. Khuyến khích bạn tự suy nghĩ trước khi xem lời giải, và hãy chia sẻ thêm các bài xây dựng thú vị mà bạn biết.

### Ví dụ 1

???+ note "[Codeforces Round #384 (Div. 2) C.Vladik and fractions](http://codeforces.com/problemset/problem/743/C)"
    Hãy xây dựng một bộ ba $x,y,z$ sao cho với $n$ cho trước, thỏa mãn $\dfrac{1}{x}+\dfrac{1}{y}+\dfrac{1}{z}=\dfrac{2}{n}$

??? note "Hướng dẫn giải"
    Có thể nhận ra cách xây dựng từ ví dụ mẫu.

    Rõ ràng, bộ ba $n, n+1, n(n+1)$ là một đáp án hợp lệ. Đặc biệt, khi $n=1$ thì không có đáp án, vì $n+1$ và $n(n+1)$ trùng nhau.

    Ý tưởng xây dựng này chủ yếu dựa vào quan sát mẫu và một chút cảm nhận số học. Với những bạn có trực giác toán tốt thì bài này không khó.

### Ví dụ 2

???+ note "[Luogu P3599 Koishi Loves Construction](https://www.luogu.com.cn/problem/P3599)"
    Task1: Hãy xác định có thể xây dựng và xây dựng một hoán vị độ dài $n$ của $1\dots n$ sao cho $n$ tổng tiền tố của nó (lấy theo modulo $n$) đều khác nhau.

    Task2: Hãy xác định có thể xây dựng và xây dựng một hoán vị độ dài $n$ của $1\dots n$ sao cho $n$ tích tiền tố của nó (lấy theo modulo $n$) đều khác nhau.

??? note "Hướng dẫn giải"
    Với task1:

    Khi $n$ là số lẻ, không thể xây dựng đáp án hợp lệ.

    Khi $n$ là số chẵn, có thể xây dựng dãy dạng $n,1,n-2,3,\cdots$.

    Đầu tiên, $n$ phải đứng đầu dãy, nếu không thì tổng tiền tố trước và sau $n$ sẽ trùng nhau theo modulo $n$.

    Để xây dựng dãy, hãy nghĩ theo hướng xây dựng dãy tổng tiền tố sao cho hiệu giữa các tổng tiền tố (theo modulo $n$) đều khác nhau, vì hiệu này chính là các phần tử của hoán vị.

    Ta thử xây dựng dãy tổng tiền tố theo dạng:

    $$
    0,1,-1,2,-2,\cdots
    $$

    Dạng này thỏa mãn mọi điều kiện đề bài.

    Với task2:

    Khi $n$ là hợp số khác $4$, không thể xây dựng đáp án hợp lệ.

    Khi $n$ là số nguyên tố hoặc $n=4$, có thể xây dựng dãy dạng $1,\dfrac{2}{1},\dfrac{3}{2},\cdots,\dfrac{n-1}{n-2},n$.

    Xét khi nào có đáp án:

    Rõ ràng, nếu $n$ là hợp số, tồn tại $p,q<n$ sao cho $p\times q \equiv 0 \pmod n$, ví dụ $(3\times6)\%9=0$. Khi đó, sau khi xuất hiện $p,q$, tích tiền tố sẽ luôn bằng $0$, nên không hợp lệ. Trường hợp $n=4$ là ngoại lệ vì không có $p,q$ như vậy.

    Cách xây dựng:

    Tương tự task1, $1$ phải đứng đầu, $n$ phải đứng cuối, vì sau $n$ mọi tích tiền tố đều bằng $0$. Quan sát các mẫu, thấy rằng có thể xây dựng dãy sao cho tích tiền tố theo modulo $n$ là $1,2,3,\cdots,n$. Khi đó, các số này là các nghịch đảo của $1\dots n-2$ cộng $1$, nên đều khác nhau.

### Ví dụ 3

???+ note "[AtCoder Grand Contest 032 B](https://atcoder.jp/contests/agc032/tasks/agc032_b)"
    Cho số nguyên $N$, hãy xây dựng một đồ thị vô hướng có $N$ đỉnh, đánh số $1\ldots N$, thỏa mãn:

    -   Đồ thị liên thông, đơn giản.
    -   Tồn tại số nguyên $S$ sao cho với mọi đỉnh, tổng chỉ số các đỉnh kề với nó đều bằng $S$.

    Đảm bảo dữ liệu vào luôn có đáp án.

??? note "Hướng dẫn giải"
    Phân tích các trường hợp $n=3,4,5$ giúp ta tìm ra ý tưởng xây dựng.

    Xây dựng một đồ thị phân $k$ đầy đủ, đảm bảo tổng các phần bằng nhau. Khi đó, với mỗi đỉnh, tổng chỉ số các đỉnh kề đều bằng $\dfrac{(k-1)\sum_{i=1}^{n}i}{k}$.

    Nếu $n$ chẵn, ghép từng cặp đầu-cuối: $\{1,n\},\{2,n-1\},\cdots$

    Nếu $n$ lẻ, tách $n$ thành một nhóm riêng, còn lại ghép từng cặp: $\{n\},\{1,n-1\},\{2,n-2\},\cdots$

    Đồ thị này với $n\ge 3$ luôn liên thông, có thể chứng minh dễ dàng.

### Ví dụ 4

???+ note "[BZOJ 4971「Lydsy1708 月赛」记忆中的背包](https://vjudge.net/problem/BZOJ-4971)"
    Sau một ngày làm việc mệt mỏi, Q chìm vào giấc mơ. Trong mơ, cậu nhớ lại bài toán 01 knapsack thời sinh viên: Cho $n$ vật, mỗi vật có thể tích $v_1,v_2,…,v_n$, hãy đếm số cách chọn một số vật (có thể không chọn) sao cho tổng thể tích đúng bằng $w$. Vì đáp án có thể rất lớn, chỉ cần in ra đáp án modulo $P$.

    Do thức khuya luyện tập, Q chỉ nhìn thấy $w$ và $P$ trong input mẫu, và đáp án là $k$, không rõ có bao nhiêu vật, cũng không rõ thể tích từng vật. Đến khi tỉnh dậy, Q vẫn không nhớ được $n$ và $v$. Hãy viết chương trình giúp Q "hồi tưởng" lại input mẫu năm xưa.

??? note "Hướng dẫn giải"
    Đây là một trong những bài xây dựng có tự do cao nhất, dẫn đến cảm giác không biết bắt đầu từ đâu.

    Đầu tiên, dễ thấy rằng modulo là "giả". Vì ta được tự do xây dựng dữ liệu, luôn có thể đảm bảo số cách không vượt quá modulo.

    Một cách "dị" là xây dựng $n$ vật nhỏ có thể tích $1$ và một số vật lớn có thể tích lớn hơn $\dfrac{w}{2}$.

    Vì vật lớn chỉ chọn được một, nên mỗi vật lớn có thể tích $x$ sẽ đóng góp $\dbinom{n}{w-x}$ cách.

    Gọi $f_{i,j}$ là số vật lớn nhỏ nhất để có $i$ vật nhỏ và $j$ cách chọn.

    Dùng quy hoạch động để tiền xử lý $f$, chỉ cần xét $i\le 20$ là đủ.

    Bài toán được giải quyết.
