Đồ thị chordal (đồ thị có dây cung, hay còn gọi là đồ thị có dây) là một loại đồ thị đặc biệt, nhiều bài toán NP-Hard trên đồ thị tổng quát lại có thuật toán tuyến tính trên đồ thị chordal.

## Một số định nghĩa và tính chất

**Đồ thị con**: Đồ thị có tập đỉnh và tập cạnh đều là tập con của đồ thị gốc.

**Đồ thị con cảm ứng (đồ thị con dẫn xuất)**: Đồ thị có tập đỉnh là tập con của đồ thị gốc, tập cạnh gồm tất cả các cạnh mà **cả hai đầu mút đều thuộc tập đỉnh đã chọn**.

**Clique (Bộ đầy đủ)**: Đồ thị con đầy đủ.

**Clique cực đại**: Clique không phải là đồ thị con của một clique lớn hơn.

**Clique lớn nhất**: Clique có số đỉnh lớn nhất.

**Số clique**: Số đỉnh của clique lớn nhất, ký hiệu $\omega(G)$.

**Tô màu tối thiểu**: Tô màu các đỉnh sao cho hai đỉnh kề nhau có màu khác nhau, dùng ít màu nhất.

**Số sắc (chromatic number)**: Số màu nhỏ nhất cần để tô màu, ký hiệu $\chi(G)$.

**Tập độc lập lớn nhất**: Tập đỉnh lớn nhất sao cho hai đỉnh bất kỳ trong tập không có cạnh nối trực tiếp. Kích thước tập này ký hiệu $\alpha(G)$.

**Phủ clique tối thiểu**: Dùng ít clique nhất để phủ toàn bộ các đỉnh. Số clique dùng ký hiệu $\kappa(G)$.

**Dây cung (chord)**: Cạnh nối hai đỉnh không kề nhau trên một chu trình.

**Đồ thị chordal (đồ thị có dây cung)**: Đồ thị mà mọi chu trình độ dài lớn hơn $3$ đều có ít nhất một dây cung.

**Bổ đề 1**: Số clique $\omega(G)\le \chi(G)$ (số sắc).

Chứng minh: Xét riêng đồ thị con cảm ứng của clique lớn nhất, cần ít nhất $\omega(G)$ màu để tô.

**Bổ đề 2**: Số tập độc lập lớn nhất $\alpha(G)\le \kappa(G)$ (số phủ clique tối thiểu).

Chứng minh: Mỗi clique chọn được nhiều nhất một đỉnh.

**Bổ đề 3**: Mọi đồ thị con cảm ứng của đồ thị chordal đều là đồ thị chordal.

Chứng minh: Nếu đồ thị chordal có đồ thị con cảm ứng không phải chordal, tức là tồn tại chu trình độ dài $>3$ không có dây cung, thì dù thêm cạnh thế nào cũng không thể làm đồ thị gốc là chordal, mâu thuẫn.

**Bổ đề 4**: Mọi đồ thị con cảm ứng của đồ thị chordal không thể là chu trình độ dài $>3$.

Chứng minh: Chu trình độ dài $>3$ không phải là chordal, áp dụng bổ đề trên.

## Nhận diện đồ thị chordal

### Mô tả bài toán

Cho một đồ thị vô hướng, kiểm tra xem nó có phải là đồ thị chordal không.

### Tập cắt đỉnh

Với hai đỉnh $u,v$ trên đồ thị $G$, tập các đỉnh mà khi xóa đi thì $u,v$ không còn liên thông gọi là **tập cắt đỉnh** giữa $u,v$. Nếu mọi tập con thực sự của tập này đều không phải là tập cắt đỉnh, thì gọi là **tập cắt đỉnh tối tiểu**.

**Bổ đề 5**: Tập cắt đỉnh tối tiểu giữa $u,v$ chia đồ thị thành nhiều thành phần liên thông, gọi $V_1$ là thành phần chứa $u$, $V_2$ là thành phần chứa $v$. Khi đó, với mọi đỉnh $a$ trong tập cắt đỉnh tối tiểu, $N(a)$ phải chứa đỉnh thuộc cả $V_1$ và $V_2$.

Chứng minh: Nếu $N(a)$ chỉ chứa đỉnh thuộc tối đa một thành phần, khi xóa $a$ khỏi tập cắt vẫn không nối lại được $u,v$, mâu thuẫn với tính tối tiểu.

**Bổ đề 6**: Đồ thị chordal, với mọi cặp đỉnh, tập cắt đỉnh tối tiểu cảm ứng luôn là một clique.

Chứng minh: Nếu tập cắt đỉnh tối tiểu có kích thước $\le 1$ thì hiển nhiên là clique.

Nếu lớn hơn, lấy hai đỉnh $x,y$ trong tập, theo **Bổ đề 5**, $N(x)$ có đỉnh ở $V_1,V_2$, gọi là $x_1,x_2$, tương tự với $y$. Có thể $x_1=y_1,x_2=y_2$.

Vì $V_1,V_2$ liên thông, tồn tại đường đi ngắn nhất trong $V_1$ từ $x_1$ đến $y_1$, và trong $V_2$ từ $x_2$ đến $y_2$. Khi đó, tồn tại chu trình $x-x_1\sim y_1-y-y_2\sim x_2-x$ có độ dài $\ge 4$, theo định nghĩa chordal, chu trình này phải có dây cung.

Nếu dây cung nối $V_1$ và $V_2$ thì tập không phải là tập cắt đỉnh. Nếu dây cung nối hai đỉnh trong cùng một thành phần hoặc nối một đỉnh trong thành phần với một đỉnh trong tập cắt, đều mâu thuẫn với đường đi ngắn nhất. Vậy dây cung chỉ có thể nối $x$ và $y$.

Suy ra, mọi cặp đỉnh trong tập cắt đỉnh tối tiểu đều có cạnh nối trực tiếp, tức là clique.

### Đỉnh đơn giản (simplicial vertex)

Gọi $N(x)$ là tập các đỉnh kề với $x$. Nếu đồ thị con cảm ứng trên $\{x\}+N(x)$ là một clique, gọi $x$ là đỉnh đơn giản.

**Bổ đề 7**: Mọi đồ thị chordal đều có ít nhất một đỉnh đơn giản, nếu không phải là đồ thị đầy đủ thì có ít nhất hai đỉnh đơn giản không kề nhau.

Chứng minh: Quy nạp theo số đỉnh, xét từng thành phần liên thông.

Cơ sở quy nạp: Nếu đồ thị là đầy đủ, mọi đỉnh đều là đỉnh đơn giản. Nếu số đỉnh $\le 3$, bổ đề đúng.

Nếu số đỉnh $\ge 4$ và không phải đồ thị đầy đủ, tồn tại $u,v$ không kề nhau. Gọi $I$ là tập cắt đỉnh tối tiểu giữa $u,v$. Gọi $A,B$ là thành phần liên thông chứa $u,v$ sau khi xóa $I$. Xét phía $A$, gọi $L=A+I$. Nếu $L$ là đồ thị đầy đủ thì $u$ là đỉnh đơn giản; nếu không, $L$ là đồ thị con cảm ứng của đồ thị chordal nên cũng là chordal, theo quy nạp có hai đỉnh đơn giản không kề nhau, vì $I$ là clique nên hai đỉnh này phải nằm trong $A$. Đỉnh đơn giản này mở rộng lên toàn bộ đồ thị vẫn là đỉnh đơn giản.

Mỗi lần chia nhỏ đồ thị, kích thước giảm, quy nạp thành công.

### Dãy loại bỏ hoàn hảo (Perfect Elimination Ordering - PEO)

Gọi $n=|V|$, dãy loại bỏ hoàn hảo $v_1,v_2,\ldots ,v_n$ là một hoán vị của $1,2,\ldots ,n$ sao cho $v_i$ là đỉnh đơn giản trong đồ thị con cảm ứng trên $\{v_i,v_{i+1},\ldots ,v_n\}$.

**Bổ đề 8**: Đồ thị vô hướng là chordal khi và chỉ khi tồn tại dãy loại bỏ hoàn hảo.

Đủ: Đồ thị chordal một đỉnh có dãy loại bỏ hoàn hảo. Theo **Bổ đề 3** và **Bổ đề 7**, đồ thị $n$ đỉnh chordal có dãy loại bỏ hoàn hảo bằng cách thêm một đỉnh đơn giản vào dãy loại bỏ hoàn hảo của đồ thị $n-1$ đỉnh.

Cần: Nếu tồn tại chu trình độ dài $>3$ mà vẫn có dãy loại bỏ hoàn hảo, xét đỉnh đầu tiên $v$ của chu trình xuất hiện trong dãy, $v$ kề với $v_1,v_2$ trên chu trình, theo định nghĩa đỉnh đơn giản thì $v_1,v_2$ phải kề nhau, mâu thuẫn.

### Thuật toán đơn giản

Mỗi lần tìm một **đỉnh đơn giản** $v$, thêm vào dãy loại bỏ hoàn hảo.

Xóa $v$ và các cạnh kề khỏi đồ thị.

Lặp lại đến khi hết đỉnh, nếu xóa hết thì đồ thị là chordal và đã tìm được dãy loại bỏ hoàn hảo; nếu không tìm được đỉnh đơn giản thì không phải chordal.

Độ phức tạp $O(n^4)$.

### Thuật toán MCS

**Thuật toán Maximum Cardinality Search (MCS)** là một phương pháp tìm dãy loại bỏ hoàn hảo cho đồ thị vô hướng trong thời gian $O(n+m)$.

Đánh số đỉnh ngược, từ $n$ đến $1$.

Gọi $label_x$ là số đỉnh đã được đánh số mà $x$ kề. Mỗi lần chọn đỉnh chưa đánh số có $label$ lớn nhất để đánh số.

Dùng danh sách liên kết để quản lý các đỉnh có cùng $label$.

Mỗi cạnh chỉ làm tăng tổng $label$ tối đa $2$ lần, nên độ phức tạp $O(n+m)$.

**Chứng minh đúng**:

Gọi $\alpha(x)$ là vị trí của $x$ trong dãy.

Cần chứng minh với mọi đồ thị chordal, dãy tìm được là dãy loại bỏ hoàn hảo, tức là với mọi đỉnh, các đỉnh kề phía sau nó trong dãy phải tạo thành clique.

**Bổ đề 9**: Xét ba đỉnh $u,v,w$ với $\alpha(u)<\alpha(v)<\alpha(w)$, nếu $uw$ kề, $vw$ không kề, thì $w$ chỉ tăng $label$ cho $u$, không cho $v$. Để $v$ được chọn trước $u$, phải tồn tại $x$ với $\alpha(v)<\alpha(x)$, $vx$ kề, $ux$ không kề, tức là $x$ chỉ tăng $label$ cho $v$.

**Bổ đề 10**: Không tồn tại dãy $v_0,v_1,\dots,v_k(k\ge 2)$ thỏa mãn:

1.  $v_iv_j$ kề khi và chỉ khi $|i-j|=1$.
2.  $\alpha(v_0)>\alpha(v_i)(i\in[1,k])$.
3.  Tồn tại $i\in[1,k-1]$ sao cho $\alpha(v_i)<\alpha(v_{i+1})<\dots<\alpha(v_k)$ và $\alpha(v_i)<\alpha(v_{i-1})<\dots<\alpha(v_1)<\alpha(v_k)<\alpha(v_0)$.

Chứng minh:

Vì $\alpha(v_1)<\alpha(v_k)<\alpha(v_0)$, $v_1v_0$ kề, $v_kv_0$ không kề, nên tồn tại $x$ với $\alpha(v_k)<\alpha(x)$, $v_kx$ kề, $v_1x$ không kề.

Xét $j$ nhỏ nhất trong $(1,k]$ sao cho $v_jx$ kề, suy ra $v_0x$ không kề, nếu không $v_0v_1\cdots v_jx$ tạo thành chu trình độ dài $\ge 4$ không dây cung.

Nếu $x<v_0$, thì $v_0,v_1,\dots,v_j,x$ cũng thỏa mãn; nếu $v_0<x$ thì $x,v_j,\dots,v_1,v_0$ cũng thỏa mãn.

Lặp lại sẽ dẫn đến mâu thuẫn.

**Định lý 1**: Với mọi đồ thị chordal, thuật toán MCS cho ra dãy loại bỏ hoàn hảo.

Chứng minh: Xét ba đỉnh $u,v,w$ với $\alpha(u)<\alpha(v)<\alpha(w)$, nếu $uv$ kề, $uw$ kề, thì $vw$ cũng phải kề.

Giả sử không kề, thì $w,u,v$ tạo thành dãy thỏa mãn **Bổ đề 10**, mâu thuẫn, nên $vw$ phải kề.

Code tham khảo:

```cpp
while (cur) {
  p[cur] = h[nww];
  rnk[p[cur]] = cur;
  h[nww] = nxt[h[nww]];
  lst[h[nww]] = 0;
  lst[p[cur]] = nxt[p[cur]] = 0;
  tf[p[cur]] = true;
  for (vector<int>::iterator it = G[p[cur]].begin(); it != G[p[cur]].end();
       it++)
    if (!tf[*it]) {
      if (h[deg[*it]] == *it) h[deg[*it]] = nxt[*it];
      nxt[lst[*it]] = nxt[*it];
      lst[nxt[*it]] = lst[*it];
      lst[*it] = nxt[*it] = 0;
      deg[*it]++;
      nxt[*it] = h[deg[*it]];
      lst[h[deg[*it]]] = *it;
      h[deg[*it]] = *it;
    }
  cur--;
  if (h[nww + 1]) nww++;
  while (nww && !h[nww]) nww--;
}
```

Nếu đồ thị là chordal, dãy tìm được là dãy loại bỏ hoàn hảo; nếu không, dãy này chắc chắn không phải dãy loại bỏ hoàn hảo, nên bài toán chuyển thành **kiểm tra dãy tìm được có phải dãy loại bỏ hoàn hảo của đồ thị gốc không**.

### Kiểm tra một dãy có phải dãy loại bỏ hoàn hảo

#### Thuật toán đơn giản

Theo định nghĩa, lần lượt kiểm tra với mỗi $v_i$ trong dãy, các đỉnh kề với $v_i$ phía sau nó có tạo thành clique không. Độ phức tạp $O(nm)$.

#### Thuật toán tối ưu

Theo định nghĩa, với $v_i$, các đỉnh kề phía sau nó là $\{v_{c_1},v_{c_2},\ldots ,v_{c_k} \}$ (theo thứ tự tăng dần), chỉ cần kiểm tra $v_{c_1}$ có kề với tất cả các đỉnh còn lại không. Độ phức tạp $O(n+m)$.

```cpp
jud = true;
for (int i = 1; i <= n; i++) {
  cur = 0;
  for (vector<int>::iterator it = G[p[i]].begin(); it != G[p[i]].end(); it++)
    if (rnk[p[i]] < rnk[*it]) {
      s[++cur] = *it;
      if (rnk[s[cur]] < rnk[s[1]]) swap(s[1], s[cur]);
    }
  for (int j = 2; j <= cur; j++)
    if (!st[s[1]].count(s[j])) {
      jud = false;
      break;
    }
}
if (!jud)
  printf("Imperfect\n");
else
  printf("Perfect\n");
```

Như vậy, **bài toán nhận diện đồ thị chordal** có thể giải trong $O(n+m)$.

## Các clique cực đại của đồ thị chordal

Gọi $N(x)$ là tập các đỉnh kề với $x$ và đứng sau $x$ trong dãy loại bỏ hoàn hảo. Khi đó, mọi clique cực đại đều có dạng $\{x\}+N(x)$.

Chứng minh: Xét một clique cực đại $V$, đỉnh đầu tiên $x$ của $V$ xuất hiện trong dãy loại bỏ hoàn hảo, $V\subseteq \{x\}+N(x)$, do $V$ cực đại nên $V=\{x\}+N(x)$.

Đồ thị chordal có nhiều nhất $n$ clique cực đại. Để liệt kê, kiểm tra mỗi $\{x\}+N(x)$ có phải clique cực đại không.

Gọi $A=\{x\}+N(x),B=\{y\}+N(y)$, nếu $A\subsetneqq B$ thì $A$ không phải clique cực đại. Khi đó, $y$ đứng trước $x$ trong dãy loại bỏ hoàn hảo.

Gọi $nxt_x$ là đỉnh trong $N(x)$ đứng sớm nhất trong dãy, $y^*$ là đỉnh $y$ cuối cùng sao cho $A\subseteq B$. Khi đó $nxt_{y^*}=x$, nếu không thì $y^*$ chưa phải cuối cùng.

$A\subsetneqq B$ khi và chỉ khi $|A|+1\le |B|$.

Bài toán chuyển thành kiểm tra có tồn tại $y$ sao cho $nxt_y=x$ và $|N(x)|+1\le |N(y)|$. Độ phức tạp $O(n+m)$.

```cpp
for (int i = 1; i <= n; i++) {
  cur = 0;
  for (vector<int>::iterator it = G[p[i]].begin(); it != G[p[i]].end(); it++)
    if (rnk[p[i]] < rnk[*it]) {
      s[++cur] = *it;
      if (rnk[s[cur]] < rnk[s[1]]) swap(s[1], s[cur]);
    }
  fst[p[i]] = s[1];
  N[p[i]] = cur;
}
for (int i = 1; i <= n; i++) {
  if (!vis[p[i]]) ans++;
  if (N[p[i]] >= N[fst[p[i]]] + 1) vis[fst[p[i]]] = true;
}
```

## Số sắc/số clique của đồ thị chordal

Một cách xây dựng: tô màu các đỉnh theo thứ tự ngược của dãy loại bỏ hoàn hảo, mỗi đỉnh nhận màu nhỏ nhất có thể. Độ phức tạp $O(n+m)$.

Chứng minh: Gọi số màu dùng là $t$, rõ ràng $t\ge \chi(G)$. Vì mỗi clique các đỉnh đều khác màu, nên $t=\omega(G)$, theo **Bổ đề 1**, $t=\chi(G)=\omega(G)$.

Nếu chỉ cần số sắc/số clique mà không cần phương án tô màu, chỉ cần lấy giá trị lớn nhất của $|\{x\}+N(x)|$.

```cpp
for (int i = 1; i <= n; i++) ans = max(ans, deg[i] + 1);
```

## Tập độc lập lớn nhất/phủ clique tối thiểu của đồ thị chordal

Tập độc lập lớn nhất: Duyệt dãy loại bỏ hoàn hảo từ đầu, chọn các đỉnh không kề với đỉnh nào đã chọn.

Phủ clique tối thiểu: Gọi tập độc lập lớn nhất là $\{v_1,v_2,\ldots ,v_t\}$, tập các clique $\{\{v_1+N(v_1)\},\{v_2+N(v_2)\},\ldots ,\{v_t+N(v_t)\} \}$ là phủ clique tối thiểu. Độ phức tạp đều $O(n+m)$.

Chứng minh: Gọi số tập độc lập và số phủ clique là $t$, theo định nghĩa $t\le \alpha(G),t\ge \kappa(G)$, theo **Bổ đề 2** thì $\alpha(G)\le \kappa(G)$, nên $t=\alpha(G)=\kappa(G)$.

```cpp
for (int i = 1; i <= n; i++)
  if (!vis[p[i]]) {
    ans++;
    for (vector<int>::iterator it = G[p[i]].begin(); it != G[p[i]].end(); it++)
      vis[*it] = true;
  }
```

## Bài tập

[SPOJ FISHNET - Fishing Net](https://www.spoj.com/problems/FISHNET)

[P3196\[HNOI2008\] Vương quốc thần kỳ](https://www.luogu.com.cn/problem/P3196)

[P3852\[TJOI2007\] Các bạn nhỏ](https://www.luogu.com.cn/problem/P3852)

## Tài liệu tham khảo

[Chuyên đề chordal](https://yhx-12243.github.io/OI-transit/memos/15.html)

[Slide WC 2009](https://github.com/hzwer/shareOI/blob/master/%E5%9B%BE%E8%AE%BA/%E5%BC%A6%E5%9B%BE%E4%B8%8E%E5%8C%BA%E9%97%B4%E5%9B%BE_%E9%99%88%E4%B8%B9%E7%90%A6.pptx)

[Tổng kết chordal - zhoushuyu](https://www.cnblogs.com/zhoushuyu/p/8716935.html)

[R. E. Tarjan and M. Yannakakis, Simple linear-time algorithms to test chordality of graphs,test acyclicity of hypergraphs,and selectively reduce acyclic hypergraphs, SIAM J. Comput., 13 (1984), pp. 566–579.](https://dl.acm.org/doi/abs/10.1137/0213035)
