author: hsfzLZH1, sshwy, StudyingFather, Marcythm

Sàng Dujiao dùng để xử lý tiền tố của một lớp hàm số học. Với hàm số học $f$, sàng Dujiao có thể tính $S(n)=\sum_{i=1}^{n}f(i)$ với độ phức tạp thấp hơn tuyến tính.

## Ý tưởng thuật toán

Ta cố gắng xây dựng công thức truy hồi của $S(n)$ theo $S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)$.

Với mọi hàm số học $g$, ta có:

$$
\begin{aligned}
    \sum_{i=1}^{n}(f * g)(i) & =\sum_{i=1}^{n}\sum_{d \mid i}g(d)f\left(\frac{i}{d}\right)           \\
                             & =\sum_{i=1}^{n}g(i)S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)
\end{aligned}
$$

trong đó $f*g$ là [chập Dirichlet](./dirichlet.md#dirichlet-卷积) của $f$ và $g$.

???+ note "Chứng minh phác thảo"
    $g(d)f\left(\frac{i}{d}\right)$ đóng góp cho mọi $i\leq n$, nên đổi thứ tự duyệt, duyệt $d,\frac{i}{d}$ (tương ứng $i,j$)
    
    $$
    \begin{aligned}
        \sum_{i=1}^n\sum_{d \mid i}g(d)f\left(\frac{i}{d}\right) & =\sum_{i=1}^n\sum_{j=1}^{\left\lfloor n/i \right\rfloor}g(i)f(j) \\
                                                                 & =\sum_{i=1}^ng(i)\sum_{j=1}^{\left\lfloor n/i \right\rfloor}f(j) \\
                                                                 & =\sum_{i=1}^ng(i)S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)
    \end{aligned}
    $$

Suy ra:

$$
\begin{aligned}
    g(1)S(n) & = \sum_{i=1}^n g(i)S\left(\left\lfloor\frac{n}{i}\right\rfloor\right) - \sum_{i=2}^n g(i)S\left(\left\lfloor\frac{n}{i}\right\rfloor\right) \\
             & = \sum_{i=1}^n (f * g)(i) - \sum_{i=2}^n g(i)S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)
\end{aligned}
$$

Nếu ta có thể chọn $g$ sao cho:

1.  Tính nhanh $\sum_{i=1}^n(f * g)(i)$;
2.  Tính nhanh tiền tố của $g$ để dùng phân khối số học tính $\sum_{i=2}^ng(i)S\left(\left\lfloor\dfrac{n}{i}\right\rfloor\right)$.

thì ta có thể tính nhanh $g(1)S(n)$.

???+ warning "Lưu ý"
    Dù $f$ có là hàm nhân hay không, chỉ cần chọn được $g$ phù hợp thì đều có thể dùng sàng Dujiao để tính tiền tố của $f$.
    
    Ví dụ với $f(n)=\mathrm{i}\varphi(n)$, rõ ràng $f$ không phải hàm nhân, nhưng chọn $g(n)=1$ thì:
    
    $$
    \sum_{k=1}^n (f*g)(k)=\mathrm{i}\frac{n(n+1)}{2}
    $$
    
    Tính $\sum_{k\leq m} (f*g)(k)$ và $\sum_{k \leq m} g(k)$ đều là $O(1)$, nên có thể dùng sàng Dujiao.

## Độ phức tạp thời gian

Đặt $R(n)=\left\{\left\lfloor \dfrac{n}{k} \right\rfloor: k=2,3,\dots,n\right\}$. Dùng [tính chất](./sqrt-decomposition.md#性质) của phân khối số học, với mọi $m\in R(n)$ thì $R(m)\subseteq R(n)$. Nghĩa là sau khi ghi nhớ, chỉ cần tính $S(k)$ cho mọi $k\in R(n)$ đúng một lần là đủ. Số lượng các điểm này $|R(n)|=O(\sqrt{n})$.

Giả sử tính $\sum_{i=1}^n(f * g)(i)$ và $\sum_{i=1}^n g(i)$ đều $O(1)$. Gọi $T(n)$ là độ phức tạp tính $S(n)$, thì:

$$
\begin{aligned}
    T(n) & = \sum_{k\in R(n)} T(k)\\
         & = \Theta(\sqrt n)+\sum_{k=1}^{\lfloor\sqrt n\rfloor} O(\sqrt k)+\sum_{k=2}^{\lfloor\sqrt n\rfloor} O\left(\sqrt{\dfrac{n}{k}}\right)\\
         & = O\left(\int_{0}^{\sqrt n} \left(\sqrt{x} + \sqrt{\dfrac{n}{x}}\right) \mathrm{d}x\right)\\
         & = O\left(n^{3/4}\right).
\end{aligned}
$$

Nếu có thể tiền xử lý một phần $S(k)$ với $k=1,2,\dots,m$ và $m\geq \lfloor\sqrt n\rfloor$. Gọi độ phức tạp tiền xử lý là $T_0(m)$, khi đó:

$$
\begin{aligned}
    T(n) & = T_0(m)+\sum_{k\in R(n);k>m} T(k)\\
         & = T_0(m)+\sum_{k=1}^{\lfloor n/m \rfloor} O\left(\sqrt{\dfrac{n}{k}}\right)\\
         & = O\left(T_0(m)+\int_{0}^{n/m} \sqrt{\dfrac{n}{x}} \mathrm{d}x\right)\\
         & = O\left(T_0(m)+\dfrac{n}{\sqrt m}\right).
\end{aligned}
$$

Nếu $T_0(m)=O(m)$ (ví dụ sàng tuyến tính), theo bất đẳng thức trung bình, khi $m=\Theta\left(n^{2/3}\right)$ thì $T(n)$ nhỏ nhất là $O\left(n^{2/3}\right)$.

??? failure "Một ví dụ chứng minh sai"
    Giả sử độ phức tạp tính $S(n)$ là $T(n)$, thì:
    
    $$
    T(n)=\Theta\left(\sqrt{n}\right)+O\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor} T\left(\left\lfloor\frac{n}{i}\right\rfloor\right)\right)
    $$
    
    $$
    \begin{aligned}
        T\left(\left\lfloor\frac{n}{i}\right\rfloor\right) & = \Theta\left(\sqrt{\frac{n}{i}}\right)+O\left(\sum_{j=2}^{\lfloor\sqrt{n/i}\rfloor} T\left(\left\lfloor\frac{n}{ij}\right\rfloor\right)\right) \\
                                                           & = O\left(\sqrt{\frac{n}{i}}\right)
    \end{aligned}
    $$
    
    Coi $O\left(\sum_{j=2}^{\lfloor\sqrt{n/i}\rfloor} T\left(\left\lfloor\dfrac{n}{ij}\right\rfloor\right)\right)$ là vô cùng nhỏ bậc cao và bỏ đi. Khi đó:
    
    $$
    \begin{aligned}
        T(n) & = \Theta\left(\sqrt{n}\right)+O\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor} \sqrt{\frac{n}{i}}\right) \\
             & = O\left(\sum_{i=1}^{\lfloor\sqrt{n}\rfloor} \sqrt{\frac{n}{i}}\right) \\
             & = O\left(\int_{0}^{\sqrt{n}}\sqrt{\frac{n}{x}}\mathrm{d}x\right) \\
             & = O\left(n^{3/4}\right)
    \end{aligned}
    $$
    
    ??? bug "Lỗi"
        Vấn đề nằm ở chỗ “coi là vô cùng nhỏ bậc cao rồi bỏ đi”. Khi thế $T\left(\left\lfloor\dfrac{n}{i}\right\rfloor\right)$ vào $T(n)$, ta có:
        
        $$
        \begin{aligned}
            T(n) & = \Theta\left(\sqrt{n}\right)+O\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor} \sqrt{\frac{n}{i}}\right)+O\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor}\sum_{j=2}^{\lfloor\sqrt{n/i}\rfloor} T\left(\left\lfloor\frac{n}{ij}\right\rfloor\right)\right)\\
                 & = O\left(\sqrt{n}+\int_{0}^{\sqrt{n}}\sqrt{\frac{n}{x}}\mathrm{d}x\right)+O\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor}\sum_{j=2}^{\lfloor\sqrt{n/i}\rfloor} T\left(\left\lfloor\frac{n}{ij}\right\rfloor\right)\right)\\
                 & = O\left(n^{3/4}\right)+O\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor}\sum_{j=2}^{\lfloor\sqrt{n/i}\rfloor} T\left(\left\lfloor\frac{n}{ij}\right\rfloor\right)\right)\\
        \end{aligned}
        $$
        
        Xét $\displaystyle\sum_{i=2}^{\lfloor\sqrt{n}\rfloor}\sum_{j=2}^{\lfloor\sqrt{n/i}\rfloor} T\left(\left\lfloor\frac{n}{ij}\right\rfloor\right)$, ta thấy:
        
        $$
        \begin{aligned}
            \sum_{i=2}^{\lfloor\sqrt{n}\rfloor}\sum_{j=2}^{\lfloor\sqrt{n/i}\rfloor} T\left(\left\lfloor\frac{n}{ij}\right\rfloor\right) & = \Omega\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor} T\left(\left\lfloor\frac{n}{i}\cdot\left\lfloor\sqrt\frac{n}{i}\right\rfloor^{-1}\right\rfloor\right)\right) \\
                                                                                                                                         & = \Omega\left(\sum_{i=2}^{\lfloor\sqrt{n}\rfloor} T\left(\left\lfloor\sqrt\frac{n}{i}\right\rfloor\right)\right)
        \end{aligned}
        $$
        
        Do không dùng ghi nhớ, nên $T\left(\left\lfloor\sqrt{\dfrac{n}{i}}\right\rfloor\right)$ vẫn là $\Omega\left(\left(\dfrac{n}{i}\right)^{1/4}\right)$, vì vậy phần “vô cùng nhỏ bậc cao” là không thể bỏ.
        
        Thực tế độ phức tạp dưới tuyến tính của sàng Dujiao là nhờ ghi nhớ. Chỉ khi có ghi nhớ mới loại bỏ được hạng tử tổng nhiều lớp đó.

## Ví dụ

### Bài toán 1

???+ note "[P4213【模板】杜教筛（Sum）](https://www.luogu.com.cn/problem/P4213)"
    Tính $S_1(n)= \sum_{i=1}^{n} \mu(i)$ và $S_2(n)= \sum_{i=1}^{n} \varphi(i)$, với $1\leq n<2^{31}$.

=== "Tiền tố hàm Möbius"
    Ta biết:
    
    $$
    \epsilon = [n=1] = \mu * 1 = \sum_{d \mid n} \mu(d)
    $$
    
    $$
    \begin{aligned}
        S_1(n) & =\sum_{i=1}^n \epsilon (i)-\sum_{i=2}^n S_1 \left(\left\lfloor \frac n i \right\rfloor\right) \\
               & = 1-\sum_{i=2}^n S_1\left(\left\lfloor \frac n i \right\rfloor\right)
    \end{aligned}
    $$
    
    Suy luận độ phức tạp xem ở mục [Độ phức tạp thời gian](#时间复杂度).
    
    Với giá trị lớn, cần dùng `map`/`unordered_map` lưu kết quả để tái sử dụng.

=== "Tiền tố hàm Euler"
    Có thể dùng sàng Dujiao cho tiền tố $\varphi(x)$, nhưng cách tốt hơn là dùng nghịch đảo Möbius.
    
    === "Nghịch đảo Möbius"
        $$
        \begin{aligned}
            \sum_{i=1}^n \sum_{j=1}^n [\gcd(i,j)=1] & =\sum_{i=1}^n \sum_{j=1}^n \sum_{d \mid i,d \mid j} \mu(d)    \\
                                                    & =\sum_{d=1}^n \mu(d) {\left\lfloor \frac n d \right\rfloor}^2
        \end{aligned}
        $$
        
        Do bài yêu cầu $\sum_{i=1}^n \sum_{j=1}^i [\gcd(i,j)=1]$, nên loại trường hợp $i=1,j=1$ rồi chia $2$ là được.
        
        Chỉ cần tiền tố hàm Möbius là có thể tính tiền tố Euler. Độ phức tạp $O\left(n^{\frac 2 3}\right)$.
    
    === "Sàng Dujiao"
        Tính $S(n)=\sum_{i=1}^n\varphi(i)$.
        
        Tương tự, $\varphi * 1=\operatorname{id}$, do đó:
        
        $$
            \begin{aligned}
                S(n) & =\sum_{i=1}^n i - \sum_{i=2}^n S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)    \\
                     & =\frac{1}{2}n(n+1) - \sum_{i=2}^n S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)
            \end{aligned}
        $$

??? note "Cài đặt mã"
    ```cpp
    --8<-- "docs/math/code/du/du_1.cpp"
    ```

### Bài toán 2

???+ note "[「LuoguP3768」Bài toán toán học đơn giản](https://www.luogu.com.cn/problem/P3768)"
    Nội dung: tính
    
    $$
    \sum_{i=1}^n\sum_{j=1}^ni\cdot j\cdot\gcd(i,j)\pmod p
    $$
    
    với $n\leq 10^{10},5\times 10^8\leq p\leq 1.1\times 10^9$ và $p$ là số nguyên tố.

Dùng $\varphi * 1=\operatorname{id}$ và nghịch đảo Möbius:

$$
\sum_{d=1}^nF^2\left(\left\lfloor\frac{n}{d}\right\rfloor\right)\cdot d^2\varphi(d)
$$

trong đó $F(n)=\dfrac{1}{2}n(n+1)$

Dùng phân khối số học cho $\sum_{d=1}^nF\left(\left\lfloor\dfrac{n}{d}\right\rfloor\right)^2$, còn tiền tố $d^2\varphi(d)$ dùng sàng Dujiao:

$$
f(n)=n^2\varphi(n)=(\operatorname{id}^2\varphi)(n)
$$

$$
S(n)=\sum_{i=1}^nf(i)=\sum_{i=1}^n(\operatorname{id}^2\varphi)(i)
$$

Cần xây dựng hàm nhân $g$ để $f\times g$ và $g$ đều có thể tính tổng nhanh.

Với tiền tố $\varphi$ có thể dùng sàng Dujiao với $\varphi * 1$, nhưng ở đây $f$ có thêm $\operatorname{id}^2$, nên ta chập thêm $\operatorname{id}^2$ để nó thành hằng:

$$
S(n)=\sum_{i=1}^n\left(\left(\operatorname{id}^2\varphi\right) * \operatorname{id}^2\right)(i)-\sum_{i=2}^n\operatorname{id}^2(i)S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)
$$

Biến đổi chập:

$$
\begin{aligned}
    ((\operatorname{id}^2\varphi)* \operatorname{id}^2)(i) & =\sum_{d \mid i}\left(\operatorname{id}^2\varphi\right)(d)\operatorname{id}^2\left(\frac{i}{d}\right) \\
                                                           & =\sum_{d \mid i}d^2\varphi(d)\left(\frac{i}{d}\right)^2                                               \\
                                                           & =\sum_{d \mid i}i^2\varphi(d)=i^2\sum_{d \mid i}\varphi(d)                                            \\
                                                           & =i^2(\varphi*1)(i)=i^3
\end{aligned}
$$

Suy ra:

$$
\begin{aligned}
    S(n) & =\sum_{i=1}^n\left((\operatorname{id}^2\varphi)* \operatorname{id}^2\right)(i)-\sum_{i=2}^n\operatorname{id}^2(i)S\left(\left\lfloor\frac{n}{i}\right\rfloor\right) \\
         & =\sum_{i=1}^ni^3-\sum_{i=2}^ni^2S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)                                                                                  \\
         & =\left(\frac{1}{2}n(n+1)\right)^2-\sum_{i=2}^ni^2S\left(\left\lfloor\frac{n}{i}\right\rfloor\right)                                                                 \\
\end{aligned}
$$

Dùng phân khối để tính.

??? note "Cài đặt mã"
    ```cpp
    --8<-- "docs/math/code/du/du_2.cpp"
    ```

### Tài liệu tham khảo

1.  任之洲，2016，《积性函数求和的几种方法》，2016 年信息学奥林匹克中国国家队候选队员论文
2.  [杜教筛的时空复杂度分析 - riteme.site](https://riteme.site/blog/2018-9-11/time-space-complexity-dyh-algo.html)
