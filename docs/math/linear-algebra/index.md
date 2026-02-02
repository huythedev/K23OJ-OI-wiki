author: codewasp942

??? tip "提示"
    Bài này không liên hệ nhiều với các bài khác trong mục “Đại số tuyến tính”. Tuy nhiên, tác giả cho rằng việc nói về bản chất, truy về nguồn gốc khái niệm và liên hệ sẽ giúp người đọc có một cái nhìn sơ bộ nhưng hệ thống về đại số tuyến tính.

Từ vài nghìn năm trước, người ta đã dùng hệ phương trình tuyến tính để giải bài toán, và đến nay đại số tuyến tính vẫn rất quan trọng.

Đại số tuyến tính xuất phát từ quan sát: nhiều đối tượng có tính chất tương tự, ví dụ:

-   Lực có thể phân tích, tổng hợp.

-   Với mọi $k,x_0$, $k \sin (x-x_0)$ có thể phân tích thành $k_1\sin x + k_2\cos x$.

Những tính chất này liên quan đến **co giãn**, **phân rã**, **chồng chập**,... Đại số tuyến tính trừu tượng hóa chúng thành một môn độc lập. Trong OI, kiến thức này giúp giải bài, tối ưu thuật toán, cấu trúc dữ liệu, ví dụ:

-   Dùng HLD duy trì cơ sở tuyến tính để lấy XOR lớn nhất trên đường đi
-   Dùng định lý cây ma trận để đếm cây khung bằng định thức
-   Dùng lũy thừa ma trận để tối ưu truy hồi
