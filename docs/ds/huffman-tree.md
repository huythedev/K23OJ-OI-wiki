## Độ dài Đường đi Có Trọng số của Cây (Weighted Path Length - WPL)

Giả sử cây nhị phân có $n$ nút lá có trọng số. Tổng của các tích giữa độ dài đường đi từ nút gốc đến mỗi nút lá với trọng số tương ứng của nút lá đó được gọi là **Độ dài Đường đi Có Trọng số của Cây (WPL)**.

Ký hiệu $w_i$ là trọng số của nút lá thứ $i$, và $l_i$ là độ dài đường đi từ nút gốc đến nút lá thứ $i$, thì công thức tính WPL là:

$$
WPL=\sum_{i=1}^nw_il_i
$$

![](./images/huffman-tree-1.svg)

Trong hình trên, quá trình tính WPL và kết quả như sau:

$$
WPL=2*2+3*2+4*2+7*2=4+6+8+14=32
$$

## Cấu trúc

Với một tập hợp các nút lá có trọng số xác định, ta có thể xây dựng nhiều cây nhị phân khác nhau. **Cây có WPL nhỏ nhất** được gọi là **Cây Huffman (Huffman Tree)**.

Đối với Cây Huffman, nút lá có trọng số càng nhỏ thì càng ở xa nút gốc, nút lá có trọng số càng lớn thì càng gần nút gốc. Ngoài ra, chỉ các nút lá có bậc (degree) bằng $0$, các nút khác có bậc đều bằng $2$.

## Thuật toán Huffman

Thuật toán Huffman dùng để xây dựng một Cây Huffman, các bước của thuật toán như sau:

1.  **Khởi tạo**: Từ $n$ trọng số đã cho, xây dựng $n$ cây nhị phân chỉ gồm một nút gốc, tạo thành một tập hợp các cây nhị phân $F$.
2.  **Lựa chọn và Hợp nhất**: Từ tập hợp cây $F$, chọn **hai cây** có trọng số nút gốc **nhỏ nhất**, sử dụng chúng làm cây con trái và cây con phải để xây dựng một cây nhị phân mới. Trọng số nút gốc của cây mới này bằng tổng trọng số nút gốc của hai cây con trái và phải.
3.  **Xóa và Thêm**: Xóa hai cây được sử dụng làm cây con trái và phải khỏi $F$, và thêm cây mới được tạo vào $F$.
4.  Lặp lại bước 2 và 3, cho đến khi trong tập hợp chỉ còn lại một cây duy nhất, cây đó chính là Cây Huffman.

![](./images/huffman-tree-2.svg)

### Chứng minh tính đúng đắn

???+ note "Bổ đề"
    Trong cây mã tiền tố tối ưu (Cây Huffman), hai nút lá có trọng số nhỏ nhất luôn là các nút lá sâu nhất, và việc điều chỉnh hai nút này thành nút anh em ít nhất không làm phá vỡ tính tối ưu của cây mã.

??? note "Chứng minh"
    Chúng ta sử dụng phương pháp phản chứng để chứng minh mệnh đề này. Giả sử trong một cây tiền tố tối ưu, tồn tại hai nút lá có trọng số nhỏ nhất mà không phải là các nút lá sâu nhất. Gọi hai nút đó là $a$ và $b$, và độ sâu của chúng nhỏ hơn độ sâu của một nút lá sâu nhất nào đó. Đối với nút lá sâu nhất $c$, ta có thể hoán đổi vị trí của $a$ và $c$, hoặc hoán đổi $b$ và $c$. Do thuật toán Huffman đảm bảo việc hợp nhất các nút lá có trọng số nhỏ nhất theo từng tầng, nên sau khi hoán đổi, Độ dài Đường đi Có Trọng số của Cây (WPL) sẽ giảm. Điều này mâu thuẫn với giả định ban đầu, suy ra hai nút lá có trọng số nhỏ nhất phải là các nút lá sâu nhất.

    Tiếp theo, giả sử hai nút lá có trọng số nhỏ nhất là $a$ và $b$, có cùng độ sâu. Nếu trong một cây tiền tố tối ưu, hai nút này không phải là anh em, giả sử tồn tại các nút khác $c$ và $d$ là anh em với $a$ và $b$ (giả sử $a$ và $c$ là anh em, $b$ và $d$ là anh em). Ta có thể hợp nhất $a$ và $b$ thành một cây con.

    -   Nếu tổng trọng số sau khi hợp nhất $a$ và $b$ nhỏ hơn trọng số của $c$ hoặc $d$, thì ta có thể hợp nhất cây con mới tạo này với nút có trọng số lớn hơn (ví dụ $c$ hoặc $d$), tạo thành một cây con mới, WPL sẽ giảm.
    -   Nếu tổng trọng số của $a$ và $b$ không nhỏ hơn trọng số của $c$ và $d$, ta có thể trực tiếp điều chỉnh để $a$ và $b$ trở thành anh em, $c$ và $d$ là anh em của một nút khác, WPL sẽ không tăng.

    Do đó, sau khi điều chỉnh như vậy, tính tối ưu sẽ không bị phá vỡ, chứng minh được mệnh đề.

???+ note "Định lý"
    Cây tiền tố được tạo ra bởi thuật toán Huffman là cây tiền tố tối ưu.

??? note "Chứng minh"
    Chúng ta sử dụng phương pháp quy nạp toán học để chứng minh định lý này.

    -   **Trường hợp cơ sở**: Khi số lượng ký tự $n = 2$, rõ ràng việc hợp nhất trực tiếp hai ký tự thành một cây là cây mã tối ưu.
    -   **Giả thiết quy nạp**: Giả sử khi số lượng ký tự là $n = k$ ($k \geq 2$), thuật toán Huffman có thể tạo ra cây mã tối ưu.
    -   **Bước quy nạp**: Đối với số lượng ký tự là $n = k + 1$, ta chọn hai ký tự có trọng số nhỏ nhất và hợp nhất chúng thành một cây con, nút gốc của cây con này trở thành một ký tự ảo (nút ảo). Theo bổ đề, thao tác này không làm mất đi tính tối ưu của cây tiền tố. Lúc này, ký tự ảo cùng với $k$ ký tự còn lại tạo thành $k$ phần tử. Theo giả thiết quy nạp, khi số lượng ký tự là $k$, thuật toán Huffman có thể tạo ra cây mã tối ưu.

    Do đó, thông qua quy nạp toán học, thuật toán Huffman có thể tạo ra cây mã tiền tố tối ưu cho mọi số lượng ký tự $n$, chứng minh xong.

## Mã hóa Huffman

Trong thiết kế chương trình, người ta thường đánh dấu mỗi ký tự bằng một mã riêng để biểu diễn một tập hợp ký tự, đó chính là **mã hóa** (encoding).

Khi thực hiện mã hóa nhị phân, nếu tất cả các mã đều có độ dài bằng nhau, thì cần $\left \lceil \log_2 n \right \rceil$ bit để biểu diễn $n$ ký tự khác nhau, gọi là **mã hóa độ dài bằng nhau** (equal-length encoding).

Nếu **tần suất xuất hiện** của các ký tự là khác nhau, ta có thể cho các ký tự có tần suất cao hơn sử dụng mã ngắn hơn, và ký tự có tần suất thấp hơn sử dụng mã dài hơn, để xây dựng một loại **mã hóa độ dài không bằng nhau** (unequal-length encoding), từ đó đạt được hiệu quả không gian tốt hơn.

Khi thiết kế mã hóa độ dài không bằng nhau, cần đảm bảo tính duy nhất khi giải mã. Nếu trong một tập hợp mã, không có mã nào là tiền tố của bất kỳ mã nào khác, thì tập hợp mã này được gọi là **mã tiền tố** (prefix code), đảm bảo tính duy nhất khi giải mã.

Cây Huffman có thể được sử dụng để xây dựng **mã tiền tố ngắn nhất**, tức là **Mã Huffman (Huffman Code)**. Các bước xây dựng như sau:

1.  Gọi tập hợp ký tự cần mã hóa là: $d_1, d_2, \dots, d_n$, với tần suất xuất hiện tương ứng là: $w_1, w_2, \dots, w_n$.
2.  Lấy $d_1, d_2, \dots, d_n$ làm các nút lá, và $w_1, w_2, \dots, w_n$ làm trọng số nút lá, xây dựng một Cây Huffman.
3.  Quy ước nhánh trái của cây Huffman đại diện cho $0$, nhánh phải đại diện cho $1$. Chuỗi $0, 1$ được tạo thành từ đường đi từ nút gốc đến mỗi nút lá chính là mã hóa của ký tự tương ứng với nút lá đó.

![](./images/huffman-tree-3.svg)

## Mã nguồn Ví dụ

??? note "Xây dựng Cây Huffman"
    ```cpp
    struct HNode {
      int weight;
      HNode *lchild, *rchild;
    };
    
    using Htree = HNode *;
    
    Htree createHuffmanTree(int arr[], int n) {
      Htree forest[N];
      Htree root = NULL;
      for (int i = 0; i < n; i++) {  // Đưa tất cả các nút vào rừng
        Htree temp;
        temp = (Htree)malloc(sizeof(HNode));
        temp->weight = arr[i];
        temp->lchild = temp->rchild = NULL;
        forest[i] = temp;
      }
    
      for (int i = 1; i < n; i++) {  // n-1 lần lặp để xây dựng cây Huffman
        int minn = -1, minnSub;  // minn là chỉ số gốc cây có giá trị nhỏ nhất, minnSub là chỉ số gốc cây có giá trị nhỏ thứ hai
        for (int j = 0; j < n; j++) {
          if (forest[j] != NULL && minn == -1) {
            minn = j;
            continue;
          }
          if (forest[j] != NULL) {
            minnSub = j;
            break;
          }
        }
    
        for (int j = minnSub; j < n; j++) {  // Gán giá trị cho minn và minnSub dựa trên trọng số
          if (forest[j] != NULL) {
            if (forest[j]->weight < forest[minn]->weight) {
              minnSub = minn;
              minn = j;
            } else if (forest[j]->weight < forest[minnSub]->weight) {
              minnSub = j;
            }
          }
        }
    
        // Xây dựng cây mới
        root = (Htree)malloc(sizeof(HNode));
        root->weight = forest[minn]->weight + forest[minnSub]->weight;
        root->lchild = forest[minn];
        root->rchild = forest[minnSub];
    
        forest[minn] = root;     // Con trỏ tới cây mới được gán cho vị trí minn
        forest[minnSub] = NULL;  // Vị trí minnSub đặt là NULL
      }
      return root;
    }
    ```

??? note "Tính WPL của Cây Huffman đã được xây dựng"
    ```cpp
    struct HNode {
      int weight;
      HNode *lchild, *rchild;
    };
    
    using Htree = HNode *;
    
    int getWPL(Htree root, int len) {  // Đệ quy để tính WPL cho cây Huffman đã xây dựng
      if (root == NULL)
        return 0;
      else {
        if (root->lchild == NULL && root->rchild == NULL)  // Nút lá
          return root->weight * len;
        else {
          int left = getWPL(root->lchild, len + 1);
          int right = getWPL(root->rchild, len + 1);
          return left + right;
        }
      }
    }
    ```

??? note "Tính trực tiếp WPL của cây Huffman chưa xây dựng"
    ```cpp
    int getWPL(int arr[], int n) {  // Tính trực tiếp WPL cho tập trọng số
      priority_queue<int, vector<int>, greater<int>> huffman;  // Min-heap
      for (int i = 0; i < n; i++) huffman.push(arr[i]);
    
      int res = 0;
      for (int i = 0; i < n - 1; i++) {
        int x = huffman.top();
        huffman.pop();
        int y = huffman.top();
        huffman.pop();
        int temp = x + y;
        res += temp;
        huffman.push(temp);
      }
      return res;
    }
    ```

??? note "Tính Mã Huffman cho dãy đã cho"
    ```cpp
    struct HNode {
      int weight;
      HNode *lchild, *rchild;
    };
    
    using Htree = HNode *;
    
    void huffmanCoding(Htree root, int len, int arr[]) {  // Tính mã Huffman
      if (root != NULL) {
        if (root->lchild == NULL && root->rchild == NULL) {
          printf("Mã của ký tự với trọng số %d là: ", root->weight);
          for (int i = 0; i < len; i++) printf("%d", arr[i]);
          printf("\n");
        } else {
          arr[len] = 0;
          huffmanCoding(root->lchild, len + 1, arr);
          arr[len] = 1;
          huffmanCoding(root->rchild, len + 1, arr);
        }
      }
    }
    ```