## Nội dung

Lift the Exponent (LTE) là một định lý khá phổ biến trong lý thuyết số sơ cấp.

Định nghĩa $\nu_p(n)$ là số mũ của số nguyên tố $p$ trong phân tích chuẩn của số nguyên $n$, tức là $\nu_p(n)$ thỏa mãn $p^{\nu_p(n)}\mid n$ và $p^{\nu_p(n)+1}\nmid n$.

Do LTE có nội dung dài, chúng ta chia thành ba phần:

Phần này giả sử $p$ là số nguyên tố, $x,y$ là các số nguyên thỏa mãn $p\nmid x$ và $p\nmid y$, $n$ là số nguyên dương.

### Phần một

Với mọi số nguyên tố $p$ và mọi số nguyên $n$ thỏa mãn $(n,p)=1$,

1. Nếu $p\mid x-y$, thì:

    $$
    \nu_p\left(x^n-y^n\right)=\nu_p(x-y)
    $$

2. Nếu $p\mid x+y$, thì với $n$ lẻ:

    $$
    \nu_p\left(x^n+y^n\right)=\nu_p(x+y)
    $$

???+ note "Chứng minh"
    Nếu $p\mid x-y$, thì không khó để thấy $p\mid x-y\iff x\equiv y\pmod p$ thì rõ ràng có:

    $$
    \sum_{i=0}^{n-1}x^iy^{n-1-i}\equiv nx^{n-1}\not\equiv 0\pmod p
    $$
    
    Từ đó, do $x^n-y^n=(x-y)\sum_{i=0}^{n-1}x^iy^{n-1-i}$ nên mệnh đề được chứng minh.
    
    Với $p\mid x+y$ thì chứng minh tương tự.

### Phần hai

Nếu $p$ là số nguyên tố lẻ,

1. Nếu $p\mid x-y$, thì:

    $$
    \nu_p\left(x^n-y^n\right)=\nu_p(x-y)+\nu_p(n)
    $$

2. Nếu $p\mid x+y$, thì với $n$ lẻ:

    $$
    \nu_p\left(x^n+y^n\right)=\nu_p(x+y)+\nu_p(n)
    $$

???+ note "Chứng minh"
    Nếu $p\mid x-y$, đặt $y=x+kp$, ta chỉ cần chứng minh với $p\mid n$.

    -   Nếu $n=p$, thì theo định lý nhị thức:

        $$
        \begin{aligned}
            \sum_{i=0}^{p-1}x^{p-1-i}y^i&=\sum_{i=0}^{p-1}x^{p-1-i}\sum_{j=0}^i\binom{i}{j}x^j(kp)^{i-j}\\
            &\equiv px^{p-1} \pmod{p^2}\\
        \end{aligned}
        $$
    
        Từ đó:

        $$
        \nu_p\left(x^n-y^n\right)=\nu_p(x-y)+1
        $$
    -   Nếu $n=p^a$, thì theo quy nạp:

        $$
        \nu_p\left(x^n-y^n\right)=\nu_p(x-y)+a
        $$
    
    Do đó mệnh đề được chứng minh.
    
    Với $p\mid x+y$ thì chứng minh tương tự.

### Phần ba

Nếu $p=2$ và $p\mid x-y$,

1. Với $n$ lẻ thì (giống phần một):

    $$
    \nu_p\left(x^n-y^n\right)=\nu_p(x-y)
    $$

2. Với $n$ chẵn thì:

    $$
    \nu_p\left(x^n-y^n\right)=\nu_p(x-y)+\nu_p(x+y)+\nu_p(n)-1
    $$

Ngoài ra, với các $x,y,n$ trên, ta có:

Nếu $4\mid x-y$, thì:

-   $\nu_2(x+y)=1$
-   $\nu_2\left(x^n-y^n\right)=\nu_2(x-y)+\nu_2(n)$

???+ note "Chứng minh"
    Ta chỉ cần chứng minh với $n$ chẵn. Vì lúc này $p\nmid \dbinom{p}{2}$ nên không thể dùng phương pháp phần hai.

    Đặt $n=2^a b$ với $a=\nu_p(n)$, $2\nmid b$, thì

    $$
    \begin{aligned}
        \nu_p\left(x^n-y^n\right)&=\nu_p\left(x^{2^a}-y^{2^a}\right)\\
        &=\nu_p\left((x-y)(x+y)\prod_{i=1}^{a-1}\left(x^{2^i}+y^{2^i}\right)\right)
    \end{aligned}
    $$
    
    Nhận thấy $2\mid x-y\implies 4\mid x^2-y^2$ nên $(\forall i\geq 1),~~x^{2^i}+y^{2^i}\equiv 2\pmod 4$ nên biểu thức trên có thể viết thành:

    $$
    \nu_p\left(x^n-y^n\right)=\nu_p(x-y)+\nu_p(x+y)+\nu_p(n)-1
    $$
    
    Do đó mệnh đề được chứng minh.

## Tài liệu tham khảo

1.  [Lifting-the-exponent lemma - Wikipedia](https://en.wikipedia.org/wiki/Lifting-the-exponent_lemma)
