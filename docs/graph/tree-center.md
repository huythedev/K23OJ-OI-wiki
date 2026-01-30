author: littleparrot12345

## Định nghĩa

Trong một cây, nếu chọn đỉnh $x$ làm gốc sao cho độ dài xích lớn nhất xuất phát từ $x$ là nhỏ nhất, thì $x$ được gọi là tâm của cây (tree center).

## Tính chất

-   Tâm của cây không nhất thiết duy nhất, nhưng nhiều nhất có $2$ đỉnh, và nếu có hai tâm thì chúng kề nhau.
-   Tâm của cây luôn nằm trên đường kính của cây.
-   Mọi đường đi từ một đỉnh đến đỉnh xa nhất của nó đều đi qua tâm của cây.
-   Khi lấy tâm làm gốc, hai xích dài nhất từ tâm đến hai đầu đường kính chính là hai xích dài nhất và nhì của cây.
-   Khi nối hai cây thành một cây mới bằng một cạnh, nối hai tâm sẽ làm đường kính cây mới nhỏ nhất.
-   Khoảng cách từ tâm đến mọi đỉnh khác không vượt quá một nửa đường kính của cây.

## Cách tìm

Tìm một đỉnh $x$ sao cho khi lấy $x$ làm gốc, xích dài nhất xuất phát từ $x$ là nhỏ nhất.

### Các bước cụ thể

1.  Duy trì $len1_x$: độ dài xích dài nhất trong cây con gốc $x$.
2.  Duy trì $len2_x$: độ dài xích dài nhì trong cây con gốc $x$, không trùng với $len1_x$.
3.  Duy trì $up_x$: độ dài xích dài nhất đi ra ngoài cây con gốc $x$ (bắt buộc đi qua cha của $x$).
4.  Tìm đỉnh $x$ sao cho $\max(len1_x, up_x)$ nhỏ nhất, đó là tâm của cây.

???+ note "Code tham khảo"
    ```cpp
    // filepath: /home/ubuntu/K23OJ-OI-wiki/docs/graph/tree-center.md
    // Mặc định các đỉnh đánh số từ 1, i ∈ [1, n], dùng vector để lưu đồ thị
    int d1[N], d2[N], up[N], x, y, mini = 1e9;  // d1,d2 tương ứng len1,len2

    struct node {
      int to, val;  // to: đỉnh kề, val: trọng số cạnh
    };

    vector<node> nbr[N];

    void dfsd(int cur, int fa) {  // Tính len1 và len2
      for (node nxtn : nbr[cur]) {
        int nxt = nxtn.to, w = nxtn.val;  // nxt: đỉnh kề, w: trọng số
        if (nxt == fa) {
          continue;
        }
        dfsd(nxt, cur);
        if (d1[nxt] + w > d1[cur]) {  // Cập nhật xích dài nhất
          d2[cur] = d1[cur];
          d1[cur] = d1[nxt] + w;
        } else if (d1[nxt] + w > d2[cur]) {  // Cập nhật xích dài nhì
          d2[cur] = d1[nxt] + w;
        }
      }
    }

    void dfsu(int cur, int fa) {
      for (node nxtn : nbr[cur]) {
        int nxt = nxtn.to, w = nxtn.val;
        if (nxt == fa) {
          continue;
        }
        up[nxt] = up[cur] + w;
        if (d1[nxt] + w != d1[cur]) {  // Nếu xích dài nhất không nằm trong cây con nxt
          up[nxt] = max(up[nxt], d1[cur] + w);
        } else {  // Nếu xích dài nhất nằm trong cây con nxt, dùng xích dài nhì
          up[nxt] = max(up[nxt], d2[cur] + w);
        }
        dfsu(nxt, cur);
      }
    }

    void GetTreeCenter() {  // Tìm tâm của cây, lưu vào x và y (nếu có hai tâm)
      dfsd(1, 0);
      dfsu(1, 0);
      for (int i = 1; i <= n; i++) {
        if (max(d1[i], up[i]) < mini) {  // Tìm đỉnh có max(len1[x], up[x]) nhỏ nhất
          mini = max(d1[i], up[i]);
          x = i;
          y = 0;
        } else if (max(d1[i], up[i]) == mini) {  // Nếu có tâm thứ hai
          y = i;
        }
      }
    }
    ```

### Ví dụ

Giả sử có cây sau:

```text
           A
          / \
         B   C
        / \   \
       D   E   F
```

-   Đường kính của cây là $D \rightarrow B \rightarrow A \rightarrow C \rightarrow F$, độ dài $4$.
-   Tâm của cây là $A$, vì từ $A$ đến $D$ hoặc $F$ đều dài $2$.
-   Nếu lấy $B$ hoặc $C$ làm gốc, xích dài nhất sẽ lớn hơn, nên không phải tâm.

### Độ phức tạp

Thuật toán trên có độ phức tạp $O(n)$ với $n$ là số đỉnh của cây.

## Tham khảo

-   [TutorialsPoint: Centers of a Tree](https://www.tutorialspoint.com/centers-of-a-tree)
-   [ProofWiki: Definition of Center of Tree](https://proofwiki.org/wiki/Definition:Center_of_Tree)
-   [Wikipedia: Tree (graph theory)](https://en.wikipedia.org/wiki/Tree_%28graph_theory%29#Properties)
