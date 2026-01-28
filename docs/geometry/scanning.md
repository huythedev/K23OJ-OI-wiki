## Dẫn nhập

Thuật toán quét (scanning line, hay còn gọi là "quét đường thẳng" hoặc "scanline") thường được sử dụng trong các bài toán hình học. Đúng như tên gọi, ý tưởng là dùng một đường thẳng "quét" qua toàn bộ hình, thường để giải các bài toán về diện tích, chu vi, hoặc đếm số điểm trong hình hai chiều.

## Bài toán hợp diện tích hình chữ nhật trong mặt phẳng

Trên hệ tọa độ hai chiều, cho nhiều hình chữ nhật với tọa độ góc dưới trái và góc trên phải, hãy tính tổng diện tích hợp của tất cả các hình chữ nhật này.

### Quy trình

Quan sát hình vẽ, tổng diện tích có thể tính vét cạn, nhưng nếu dữ liệu lớn thì cần dùng thuật toán **quét**.

Giả sử ta có một đường thẳng quét từ dưới lên trên:

![](./images/scanning.svg)

Như hình, mỗi lần quét, ta chia mặt phẳng thành các dải màu khác nhau, mỗi dải có chiều cao là khoảng cách quét, còn chiều rộng thay đổi liên tục.

Gán nhãn cho mỗi cạnh ngang của hình chữ nhật: cạnh dưới gán $+1$, cạnh trên gán $-1$. Mỗi khi gặp một cạnh ngang, ta cộng/trừ giá trị này vào đoạn tương ứng trên trục hoành.

???+ note "Lưu ý"
    Thao tác này giống như duyệt một chuỗi ngoặc: mở ngoặc cộng 1, đóng ngoặc trừ 1, "giá trị" tại mỗi vị trí chính là độ sâu hiện tại, kiểm tra giá trị lớn hơn 0 tương đương với việc đang nằm trong một cặp ngoặc, tức là đoạn này thuộc về hình chữ nhật.

Chiều rộng của các dải nhỏ (có thể có nhiều dải) chính là tổng độ dài các đoạn trên trục hoành có giá trị lớn hơn 0.

### Cài đặt

Dùng cây đoạn (segment tree) để duy trì tổng độ dài các đoạn trên trục hoành có số lần phủ lớn hơn 0.

Yêu cầu:

-   Cộng/trừ 1 cho một đoạn.
-   Tính tổng độ dài các đoạn có giá trị lớn hơn 0 trên toàn trục.

Nếu bạn thử dùng cây đoạn thông thường, có thể gặp khó khăn: khi cập nhật một đoạn, dù biết đoạn đó trùng với nút quản lý, ta vẫn không thể biết ngay số đoạn chuyển từ 1 về 0 (hoặc ngược lại).

Giải pháp là: với mỗi nút, duy trì số lần phủ hoàn toàn (`v[]`, giống như lazy tag không cần đẩy xuống) và tổng độ dài đã phủ (`w[]`).

Cần [rời rạc hóa](../misc/discrete.md).

??? note "[LuoGu P5490【Mẫu】Quét & Hợp diện tích hình chữ nhật](https://www.luogu.com.cn/problem/P5490) - Mã mẫu"
    ```cpp
    --8<-- "docs/geometry/code/scanning/scanning_1.cpp"
    ```

??? note "[POJ 1151 Atlantis] (http://poj.org/problem?id=1151) - Mã mẫu"
    ```cpp
    --8<-- "docs/geometry/code/scanning/scanning_2.cpp"
    ```

### Bài tập luyện tập

-   [POJ1177 Picture](http://poj.org/problem?id=1177)
-   [POJ3832 Posters](http://poj.org/problem?id=3832)
-   [LuoGu P1856 [IOI1998][USACO5.5] Chu vi hình chữ nhật Picture](https://www.luogu.com.cn/problem/P1856)
    -   Độ dài cạnh ngang là biến thiên độ dài phủ.
    -   Tính hai chiều riêng biệt để tránh phải xét cạnh dọc.
    -   Khi sắp xếp các cạnh, chú ý trường hợp trùng cạnh.
    -   Nếu dữ liệu nhỏ, có thể mô phỏng vét cạn $O(n^2)$.

## Hình hộp trực giao B chiều

Hình hộp trực giao B chiều là tập các điểm trong không gian $B$ chiều, với mỗi chiều $i$ có tọa độ nằm trong đoạn $[l_i, r_i]$.

Thông thường, đoạn 1 chiều gọi là "đoạn", 2 chiều gọi là "hình chữ nhật", 3 chiều gọi là "hình hộp" (bài toán đếm điểm trong hình chữ nhật 2D là bài toán hình hộp trực giao 2 chiều).

Với bài toán tĩnh hai chiều, ta có thể dùng quét một chiều, cấu trúc dữ liệu duy trì chiều còn lại. Khi quét từ trái sang phải, các thao tác cập nhật và truy vấn sẽ diễn ra trên chiều còn lại.

Nếu truy vấn có thể hiệu (dạng prefix sum), dùng cây Fenwick (BIT) hoặc segment tree; nếu không, dùng phân chia và chinh phục (CDQ divide and conquer, nhưng ở đây không đề cập).

Một cách khác là nhìn bài toán dưới góc độ dãy số: quét theo điểm kết thúc $r=1\cdots n$, duy trì cấu trúc dữ liệu cho các điểm bắt đầu $l$, tức là quét một chiều, cấu trúc dữ liệu duy trì chiều còn lại.

Độ phức tạp thường là $O((n+m)\log n)$.

## Đếm điểm trong hình chữ nhật 2D

Cho một dãy số độ dài $n$, có $m$ truy vấn, mỗi truy vấn hỏi có bao nhiêu phần tử trong đoạn $[l,r]$ có giá trị thuộc $[x,y]$.

Bài toán này gọi là "đếm điểm trong hình chữ nhật 2D". Thực chất là đếm số điểm trong một hình chữ nhật trên mặt phẳng.

Cách đơn giản nhất là dùng quét + cây Fenwick (BIT).

Vì đây là bài toán tĩnh 2D, ta dùng quét để chuyển thành bài toán động 1D, rồi dùng cấu trúc dữ liệu cho dãy số.

Trước hết, rời rạc hóa các truy vấn, dùng BIT duy trì số lượng phần tử theo giá trị. Với mỗi truy vấn $l, r$, khi quét đến $l-1$ thì đếm số phần tử trong $[x,y]$ là $a$, khi quét đến $r$ thì đếm được $b$, đáp án là $b-a$.

### Bài tập ví dụ

???+ note "[LuoGu P2163 [SHOI2007] Nỗi phiền của người làm vườn](https://www.luogu.com.cn/problem/P2163)"
    Đầu tiên rời rạc hóa. Gọi $ans_{x, y}$ là số điểm trong hình chữ nhật có góc dưới trái $(0,0)$, góc trên phải $(x,y)$. Đáp án truy vấn là $ans_{c, d} - ans_{a - 1, d} - ans_{c, b - 1} + ans_{a - 1, b - 1}$.
    
    ??? note "Mã mẫu"
        ```cpp
        --8<-- "docs/geometry/code/scanning/scanning_3.cpp"
        ```

???+ note "[LuoGu P1908 Đếm số nghịch thế](https://www.luogu.com.cn/problem/P1908)"
    Đúng vậy, đếm số nghịch thế cũng có thể dùng tư tưởng quét. Chuyển bài toán thành: duyệt từ cuối về đầu, với mỗi vị trí $i$, đếm số phần tử trong $[i+1,n]$ nhỏ hơn $a_i$. Vì giá trị có thể lớn, cần rời rạc hóa. Duyệt từ cuối về đầu, mỗi lần cập nhật BIT, rồi đếm số phần tử nhỏ hơn $a_i$ (tức là số nghịch thế của $a_i$). Có thể dùng BIT hoặc segment tree để cập nhật và truy vấn.
    
    ??? note "Mã mẫu"
        ```cpp
        --8<-- "docs/geometry/code/scanning/scanning_4.cpp"
        ```

???+ note "[LuoGu P1972 [SDOI2009] Chuỗi hạt của HH](https://www.luogu.com.cn/problem/P1972)"
    Tóm tắt: Cho một dãy số, nhiều truy vấn hỏi trong đoạn $[l,r]$ có bao nhiêu số khác nhau.

    Có thể suy luận tính chất, rồi dùng quét theo điểm kết thúc, cấu trúc dữ liệu duy trì điểm bắt đầu, hoặc chuyển về bài toán đếm điểm trong hình chữ nhật 2D.

    Với mỗi $a_i$, gọi $pre_i$ là vị trí xuất hiện trước đó của $a_i$ (nếu chưa từng xuất hiện thì $pre_i=0$). Theo đề, mỗi số chỉ tính một lần trong đoạn, nên chỉ cần đếm số $pre_x \le l-1$ trong đoạn $[l,r]$.

    Bài toán trở thành: cho dãy $pre$, nhiều truy vấn hỏi trong đoạn $[l,r]$ có bao nhiêu $pre_i \le l-1$.

    Xem $pre_i$ là điểm trên mặt phẳng: hoành độ $i$, tung độ $pre_i$, mỗi truy vấn là đếm số điểm trong hình chữ nhật có góc dưới trái $(l,0)$, góc trên phải $(r,l-1)$.

    Vì truy vấn có thể hiệu, ta tách thành hai hình chữ nhật: $(0,0)-(r,l-1)$ trừ $(0,0)-(l-1,l-1)$. Như vậy dễ dùng quét.

    Mỗi thao tác $O(\log n)$, tổng $O((n+m)\log n)$.

    ??? note "Mã mẫu"
        ```cpp
        --8<-- "docs/geometry/code/scanning/scanning_5.cpp"
        ```

### Bài tập luyện tập

-   [LuoGu P8593「KDOI-02」Một phát bắn](https://www.luogu.com.cn/problem/P8593) - ứng dụng đếm nghịch thế.
-   [AcWing 4709. Bộ ba](https://www.acwing.com/problem/content/4712/) - phiên bản đơn giản hóa, cũng là ứng dụng đếm nghịch thế.
-   [LuoGu P8773 [Blue Bridge Cup 2022 Prov A] Chọn số XOR](https://www.luogu.com.cn/problem/P8773) - biến thể của chuỗi hạt HH.
-   [LuoGu P8844 [Trí Tuệ Cup #4 Sơ loại] Tiểu Kha và lá rụng](https://www.luogu.com.cn/problem/P8844) - bài toán trên cây chuyển về dãy số rồi đếm điểm 2D.

Tóm lại, tư tưởng chính của đếm điểm trong hình chữ nhật 2D là: dùng cấu trúc dữ liệu duy trì một chiều, quét qua chiều còn lại.

## Tài liệu tham khảo

-   [cnblogs/Yang1208: Giải thích scanline, segment tree mở động](https://www.cnblogs.com/yangsongyi/p/8378629.html)
-   [csdn/riba2534: POJ1151 Atlantis giải chi tiết](https://blog.csdn.net/riba2534/article/details/76851233)
-   [csdn/刀刀狗 0102: POJ1151 Atlantis giải chi tiết](https://blog.csdn.net/winddreams/article/details/38495093)
-   [Bàn về scanline](https://www.luogu.com.cn/article/f8q5bmnz)
