## Xây dựng mảng khối (Block Array)

Mảng khối, tức là chia một mảng thành nhiều khối, thông tin trong khối được lưu trữ tổng thể, nếu khi truy vấn gặp khối hai bên không hoàn chỉnh thì trực tiếp dùng phương pháp vét cạn (brute force) để truy vấn. Thông thường, độ dài của khối là $O(\sqrt{n})$. Phân tích chi tiết có thể tham khảo bài viết "Sơ bộ về thuật toán phân khối với kích thước phi tiêu chuẩn" của Từ Minh Khoan trong tuyển tập bài viết của Đội tuyển quốc gia năm 2017.

Dưới đây là đoạn mã trực tiếp cho việc xây dựng mảng khối.

???+ note "Thực hiện"
    ```cpp
    num = sqrt(n);
    for (int i = 1; i <= num; i++)
      st[i] = n / num * (i - 1) + 1, ed[i] = n / num * i;
    ed[num] = n;
    for (int i = 1; i <= num; i++) {
      for (int j = st[i]; j <= ed[i]; j++) {
        belong[j] = i;
      }
      size[i] = ed[i] - st[i] + 1;
    }
    ```

Trong đó `st[i]` và `ed[i]` là điểm bắt đầu và điểm kết thúc của khối, `size[i]` là kích thước của khối.

## Lưu trữ và sửa đổi thông tin trong khối

### Ví dụ 1: [Phép thuật của Giáo chủ (Jiaozhu de Mofa)](https://www.luogu.com.cn/problem/P2801)

Hai loại thao tác:

1.  Mỗi số trong đoạn $[x,y]$ đều được cộng thêm $z$;
2.  Truy vấn số lượng số trong đoạn $[x,y]$ lớn hơn hoặc bằng $z$.

Vì ta cần đếm số lượng số lớn hơn hoặc bằng một giá trị nào đó trong một khối, nên cần một mảng `t` để sắp xếp các phần tử trong khối, mảng `a` là mảng gốc (chưa sắp xếp). Đối với thao tác sửa đổi trên các khối hoàn chỉnh, ta sử dụng cách thức tương tự như đánh dấu duy trì (lazy propagation), dùng mảng `delta` để ghi lại giá trị cộng thêm tổng thể hiện tại trong khối. Giả sử tổng số lần truy vấn và sửa đổi là $q$, thì độ phức tạp thời gian là $O(q\sqrt{n}\log n)$.

Sử dụng mảng `delta` để ghi lại tình trạng gán giá trị tổng thể của từng khối.

???+ note "Thực hiện"
    ```cpp
    void Sort(int k) {
      for (int i = st[k]; i <= ed[k]; i++) t[i] = a[i];
      sort(t + st[k], t + ed[k] + 1);
    }
    
    void Modify(int l, int r, int c) {
      int x = belong[l], y = belong[r];
      if (x == y)  // Nếu đoạn nằm trong một khối thì sửa trực tiếp
      {
        for (int i = l; i <= r; i++) a[i] += c;
        Sort(x);
        return;
      }
      for (int i = l; i <= ed[x]; i++) a[i] += c;     // Sửa trực tiếp đoạn đầu
      for (int i = st[y]; i <= r; i++) a[i] += c;     // Sửa trực tiếp đoạn cuối
      for (int i = x + 1; i < y; i++) delta[i] += c;  // Đánh dấu cộng dồn cho các khối ở giữa
      Sort(x);
      Sort(y);
    }
    
    int Answer(int l, int r, int c) {
      int ans = 0, x = belong[l], y = belong[r];
      if (x == y) {
        for (int i = l; i <= r; i++)
          if (a[i] + delta[x] >= c) ans++;
        return ans;
      }
      for (int i = l; i <= ed[x]; i++)
        if (a[i] + delta[x] >= c) ans++;
      for (int i = st[y]; i <= r; i++)
        if (a[i] + delta[y] >= c) ans++;
      for (int i = x + 1; i <= y - 1; i++)
        ans +=
            ed[i] - (lower_bound(t + st[i], t + ed[i] + 1, c - delta[i]) - t) + 1;
      // Dùng lower_bound để tìm vị trí của phần tử đầu tiên lớn hơn hoặc bằng c trong mỗi khối ở giữa
      return ans;
    }
    ```

### Ví dụ 2: Thuyền cứu sinh đêm đông (Hanye Fangzhou)

Hai loại thao tác:

1.  Mỗi số trong đoạn $[x,y]$ đều được gán bằng $z$;
2.  Truy vấn số lượng số trong đoạn $[x,y]$ nhỏ hơn hoặc bằng $z$.

Dùng mảng `delta` để ghi lại giá trị được gán tổng thể hiện tại trong khối. Khi khối chưa được gán tổng thể, dùng một giá trị đặc biệt (ví dụ: `0x3f3f3f3f3f3f3f3fll`) để biểu thị. Đối với các khối ở biên, trước khi truy vấn cần `pushdown`, đưa thông tin của khối xuống từng phần tử. Sau khi gán giá trị, nhớ sắp xếp lại (`sort`) một lần nữa. Các mặt khác tương tự như ví dụ trên.

???+ note "Thực hiện"
    ```cpp
    void Sort(int k) {
      for (int i = st[k]; i <= ed[k]; i++) t[i] = a[i];
      sort(t + st[k], t + ed[k] + 1);
    }
    
    void PushDown(int x) {
      if (delta[x] != 0x3f3f3f3f3f3f3f3fll)  // Dùng giá trị này để đánh dấu khối chưa bị gán tổng thể
        for (int i = st[x]; i <= ed[x]; i++) a[i] = t[i] = delta[x];
      delta[x] = 0x3f3f3f3f3f3f3f3fll;
    }
    
    void Modify(int l, int r, int c) {
      int x = belong[l], y = belong[r];
      PushDown(x);
      if (x == y) {
        for (int i = l; i <= r; i++) a[i] = c;
        Sort(x);
        return;
      }
      PushDown(y);
      for (int i = l; i <= ed[x]; i++) a[i] = c;
      for (int i = st[y]; i <= r; i++) a[i] = c;
      Sort(x);
      Sort(y);
      for (int i = x + 1; i < y; i++) delta[i] = c;
    }
    
    int Binary_Search(int l, int r, int c) {
      int ans = l - 1, mid;
      while (l <= r) {
        mid = (l + r) / 2;
        if (t[mid] <= c)
          ans = mid, l = mid + 1;
        else
          r = mid - 1;
      }
      return ans;
    }
    
    int Answer(int l, int r, int c) {
      int ans = 0, x = belong[l], y = belong[r];
      PushDown(x);
      if (x == y) {
        for (int i = l; i <= r; i++)
          if (a[i] <= c) ans++;
        return ans;
      }
      PushDown(y);
      for (int i = l; i <= ed[x]; i++)
        if (a[i] <= c) ans++;
      for (int i = st[y]; i <= r; i++)
        if (a[i] <= c) ans++;
      for (int i = x + 1; i <= y - 1; i++) {
        if (0x3f3f3f3f3f3f3f3fll == delta[i])
          ans += Binary_Search(st[i], ed[i], c) - st[i] + 1;
        else if (delta[i] <= c)
          ans += size[i];
      }
      return ans;
    }
    ```

## Luyện tập

1.  [Sửa đổi điểm đơn, truy vấn đoạn](https://loj.ac/problem/130)
2.  [Sửa đổi đoạn, truy vấn đoạn](https://loj.ac/problem/132)
3.  [【Template】Segment Tree 2 (Cây phân đoạn 2)](https://www.luogu.com.cn/problem/P3373)
4.  [「Ynoi2019 模拟赛」Yuno thích công nghệ căn bậc hai III (Yuno loves sqrt technology III)](https://www.luogu.com.cn/problem/P5048)
5.  [「Violet」Bồ công anh (Pugongying)](https://www.luogu.com.cn/problem/P4168)
6.  [Làm thơ (Zuoshi)](https://www.luogu.com.cn/problem/P4135)