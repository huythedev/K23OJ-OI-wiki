## Khái quát

Khi một biến cố đã xảy ra, xác suất của các biến cố khác có thể thay đổi do thêm thông tin. Ví dụ khi rút thẻ trong game, ta có thể cho rằng “ra 6 sao” và “không ra 6 sao” là như nhau, nhưng nếu đã rút 50 lần không ra 6 sao thì việc tiếp tục tin “xác suất như nhau” không còn hợp lý.

Tóm lại, nghiên cứu xác suất trong điều kiện đã biết là cần thiết.

## Xác suất có điều kiện

### Định nghĩa

Biết biến cố $A$ xảy ra, xác suất biến cố $B$ xảy ra trong điều kiện đó gọi là **xác suất có điều kiện**, ký hiệu $P(B|A)$.

Trong không gian xác suất $(\Omega, \mathcal{F}, P)$, nếu $A \in \mathcal{F}$ và $P(A) > 0$, thì xác suất có điều kiện $P(\cdot|A)$ được định nghĩa bởi

$$
P(B|A) = \frac{P(AB)}{P(A)} \quad \forall B \in \mathcal{F}
$$

Có thể kiểm tra $P(\cdot|A)$ là hàm xác suất trên $(\Omega, \mathcal{F})$.

Từ định nghĩa suy ra hai công thức:

-   **Công thức nhân xác suất**: trong $(\Omega, \mathcal{F}, P)$, nếu $P(A) > 0$ thì với mọi $B$,

$$
P(AB) = P(A)P(B|A)
$$

-   **Công thức xác suất toàn phần**: nếu $A_1, \cdots, A_n$ đôi một rời nhau và hợp bằng $\Omega$, thì với mọi $B$,

$$
P(B) = \sum_{i=1}^{n} P(A_i)P(B|A_i)
$$

### Công thức Bayes

Giả sử các nguyên nhân có thể gây ra $B$ là $A_1, A_2, \cdots, A_n$. Khi biết $P(A_i)$ và $P(B|A_i)$, ta tính $P(B)$ bằng công thức toàn phần. Nhưng nhiều khi cần suy ngược: biết $B$ đã xảy ra, tìm xác suất các nguyên nhân. Khi đó

$$
P(A_i|B) = \frac{P(A_iB)}{P(B)} = \frac{P(A_i)P(B|A_i)}{\sum_{j=1}^{n} P(A_j)P(B|A_j)}
$$

Đây là công thức Bayes.

## Tính độc lập của biến cố

Khi xét xác suất có điều kiện, có thể gặp $P(B|A) = P(B)$, tức $B$ không cung cấp thông tin về $A$. Khi đó $A$ và $B$ “không liên quan”, dẫn đến định nghĩa sau.

### Định nghĩa

Với biến cố $A$,$B$ trong cùng không gian xác suất, nếu

$$
P(AB) = P(A)P(B)
$$

thì gọi $A$,$B$ **độc lập**. Với nhiều biến cố $A_1, A_2, \cdots, A_n$, gọi là độc lập khi và chỉ khi với mọi tập con $\{ A_{i_k} : 1 \leq i_1 < i_2 < \cdots < i_k \leq n \}$,

$$
P( A_{i_1}A_{i_2} \cdots A_{i_r} ) = \prod_{k=1}^{r} P(A_{i_k})
$$

### Độc lập của nhiều biến cố

Không thể suy từ độc lập từng đôi ra độc lập chung. Phản ví dụ:

Có một con xúc xắc tứ diện đều, ba mặt tô đỏ, xanh lá, xanh dương; mặt còn lại có đủ ba màu. Gieo một lần, gọi $A$,$B$,$C$ lần lượt là “mặt chạm bàn có đỏ, có xanh lá, có xanh dương”.

Ta có $P(A) = P(B) = P(C) = \frac{1}{2}$ và $P(AB) = P(BC) = P(CA) = P(ABC) = \frac{1}{4}$.

Rõ ràng $A, B, C$ đôi một độc lập, nhưng $P(ABC) \neq P(A)P(B)P(C)$, nên không độc lập.
