## Định nghĩa

Nguyên lý hộp (pigeonhole principle).

Nó thường dùng để chứng minh tồn tại và tìm nghiệm trong trường hợp xấu nhất.

## Trường hợp đơn giản

Chia $n+1$ vật vào $n$ nhóm, thì ít nhất một nhóm có từ hai vật trở lên.

Định lý này khá hiển nhiên; chứng minh bằng phản chứng: nếu mỗi nhóm có nhiều nhất 1 vật thì tổng tối đa là $1\times n$, nhưng thực tế có $n+1$, mâu thuẫn.

## Mở rộng

Chia $n$ vật thành $k$ nhóm, thì tồn tại một nhóm có ít nhất $\left \lceil \dfrac{n}{k} \right \rceil$ vật.

Có thể chứng minh bằng phản chứng: nếu mỗi nhóm có ít hơn $\left \lceil \dfrac{n}{k} \right \rceil$ vật thì tổng $S\leq (\left \lceil \dfrac{n}{k} \right \rceil -1 ) \times k=k\left\lceil \dfrac{n}{k} \right\rceil-k < k(\dfrac{n}{k}+1)-k=n$ mâu thuẫn.

Ngoài ra, “phân chia” có thể nới lỏng thành “phủ” mà kết luận không đổi.  
Cho tập $S$, một họ con không rỗng $\{A_1,A_2\ldots A_k\}$

-   Nếu $\bigcup_{i=1}^k A_i$ thì gọi là một phủ (cover) của $S$
-   Nếu một phủ còn thỏa $i\neq j\to A_i\cap A_j=\varnothing$ thì gọi là một phân hoạch (partition) của $S$.

Nguyên lý hộp có thể phát biểu: với một phủ $\{A_1,A_2\ldots A_k\}$ của $S$ luôn có ít nhất một tập $A_i$ sao cho $\left\vert A_i \right\vert \geq \left\lceil \dfrac{\left\vert S \right\vert}{k} \right\rceil$.

## Tài liệu tham khảo

-   [Wikipedia: Pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle)
-   *Discrete Mathematics and Its Applications*: Chapter 6, Section 1
