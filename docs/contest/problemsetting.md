author: ouuan, Henry-ZHR, StudyingFather, ChungZH, xyf007, Cryflmind, oierlinch

## Chuẩn bị trước khi ra đề

### Có trình độ nhất định

Một mặt, tự ra đề thì rất khó ra bài khó hơn trình độ của chính mình; trình độ OI nhất định giúp nghĩ ra idea tốt hơn và lời giải hay hơn. Mặt khác, trình độ OI phần nào phản ánh thâm niên OI; những người đã gặp nhiều đề cũng có quan niệm riêng về “đề hay”.

### Có thái độ nghiêm túc, có trách nhiệm

Ra đề là để người khác làm, không phải để phô diễn bản thân, mà là phục vụ người khác. Thi thuật toán là cuộc thi giữa thí sinh, không phải cuộc đấu giữa ra đề và người làm. Vì vậy, ra đề không nên lấy việc “đánh đố” làm mục tiêu (tất nhiên chống AK và có độ phân hóa tốt cũng rất quan trọng), mà nên giúp thí sinh thu được điều gì đó trong cuộc thi. Dành đủ thời gian công sức để học cách ra đề và ra đề một cách nghiêm túc là rất quan trọng.

### Chuẩn bị cho việc tốn nhiều thời gian

Muốn ra đề nghiêm túc thì bắt buộc phải tốn nhiều thời gian. Nếu không chuẩn bị tâm lý, có thể dẫn tới chuẩn bị vội vàng, chất lượng kém, hoặc hối hận vì không dành thời gian cho học tập. Nhưng ra đề cũng mang lại nhiều ký ức đẹp; nếu thật sự hứng thú và đã chuẩn bị tâm lý đầy đủ, thu hoạch từ việc ra đề có thể bù lại thời gian bỏ ra.

### Đọc kỹ nội dung bài viết này

Bài viết giới thiệu toàn bộ quy trình ra đề từ hai khía cạnh: cách ra đề và cách ra đề hay. Với người muốn ra đề, đọc kỹ bài này chắc chắn có ích.

## Nội dung đề

Ra một đề, idea — tức nội dung bản chất của bài — là linh hồn của đề, đồng thời là bước đầu tiên.

### Nguồn idea

1.  Được gợi ý từ đề có sẵn (nhưng không được sao chép hoặc tăng cường vô nghĩa, ví dụ: chuyển bài dãy sang xương rồng).
2.  Được gợi ý từ kiến thức đã học (nhưng không được chắp vá vô liên hệ).
3.  Được gợi ý từ đời sống/game (nhưng chú ý đừng biến thành mô phỏng lớn).
4.  Không rõ vì sao, просто nảy ra một bài.

### Idea nào là không tốt

#### Về “đề gốc”

Đề gốc có thể chia thành ba loại: hoàn toàn trùng, gần trùng, và trùng lời giải.

-   Hoàn toàn trùng: dùng AC code của một bài có thể AC bài kia.
-   Gần trùng: sửa AC code của một bài sang AC code của bài khác có thể do người không biết bài làm được.
-   Trùng lời giải: tư tưởng/lời giải lõi giống nhau, nhưng khác ở hiện thực hoặc chi tiết không quan trọng.

Ba loại này có quan hệ bao hàm từ dưới lên.

Các trường hợp không nên xuất hiện:

1.  Biết rõ có “gần trùng” mà vẫn ra đề gốc.
2.  Do không tra cứu nên không biết có đề gốc, từ đó ra “gần trùng”.
3.  “Trùng lời giải” đã quá nổi tiếng (ví dụ: NOIP/NOI gốc) mà vẫn ra.
4.  Trong bài có tính chọn lọc (không phải cho điểm), xuất hiện “trùng lời giải”.

Các trường hợp最好不要出现:

1.  Biết rõ至少为 “trùng lời giải” mà vẫn ra đề gốc.
2.  Do không tra cứu nên không biết có đề gốc, từ đó ra “trùng lời giải”.
3.  Bất kỳ情况下都 ra “gần trùng”.

Những ngoại lệ có thể nới:

1.  Thi thử trong trường.
2.  Thi thử theo专题训练.
3.  Cuộc thi难度较低，hoặc定位为送分题。

#### Về “đề độc”

“Đề độc” là khái niệm mơ hồ và chủ quan; ở đây chỉ trích dẫn một số thảo luận trước đó kèm hiểu biết của bản thân. Chủ đề này rất mở, hoan nghênh mọi người nêu quan điểm.

> Một đề hay không nên là hai đề ghép lại; đề hay có idea của riêng nó — và应该突出 idea đó mà không包装 quá nhiều。
>
> Một đề hayShould be novel. Truly good problems are those that can inspire new problems.
>
> ——[vfk《UOJ 精神之源流》][1]

Ví dụ：[「XR-1」柯南家族](https://www.luogu.com.cn/problem/P5346)，hai nửa lời giải tách rời hoàn toàn: nửa đầu là [“mẫu” sắp xếp hậu tố trên cây](https://www.luogu.com.cn/problem/P5353)，nửa sau là bài cây经典. Dù nhập trọng số cây tùy ý vẫn làm được phần hai; hai phần không liên hệ.

> Một loại đề OI lấy toán làm chủ, từ mô tả đến lời giải đều mang tính toán, và không chứa kiến thức thuật toán; loại này gọi chung là đề toán thuần.
>
> ——[王天懿《论偏题的危害》][2]

Ví dụ kinh điển：[NOIP2017 小凯的疑惑](https://uoj.ac/problem/329)

Khác biệt giữa toán trong OI và toán thuần là: trọng tâm không phải **đáp án là gì** mà là làm sao **tăng tốc** tính đáp án. Nếu trọng tâm là “tính thế nào” chứ không phải “tính nhanh thế nào”, thì thường không phù hợp ra trong OI.

> Một số đề偏题涉及 vật lý đại học, khiến thí sinh面对 kiến thức chưa từng học mà bối rối, tạo ra隔阂 về kiến thức.
>
> ——[王天懿《论偏题的危害》][2]

Ví dụ kinh điển：[「清华集训 2015」多边形下海](https://uoj.ac/problem/159)

Không chỉ vật lý, đề OI không nên涉及 quá多 kiến thức môn khác; nếu涉及 phải解释 chi tiết, không nên để kiến thức đó trở thành rào cản lớn.

> Một đề hay dù难度如何 đều nên có độ难 tư duy, cần thí sinh思考 và发现性质.
>
> Code của đề hay có thể dài, nhưng không phải do强行嵌套 hoặc tăng条件 mà dài; nó dài một cáchNatural, khiến người ta感觉 code “đúng là nên dài vậy”.
>
> ——[王天懿《论偏题的危害》][2]

Ví dụ kinh điển：[「SDOI2010」猪国杀](https://loj.ac/problem/2885)，[「集训队互测 2015」未来程序·改](https://uoj.ac/problem/98)

Trong các kì OI thông thường, độ khó tư duy nên chiếm chính. Dĩ nhiên, dạng工程题 như THUWC/THUSC Day 2+ cũng有意义 — vì体验营除了考算法 còn考工程代码与文档学习能力. Nhưng trong OI thông thường, chủ yếu vẫn là thuật toán và tư duy.

## Đề bài

### Dùng LaTeX viết công thức

Có nhiều教程 LaTeX, ví dụ:

-   [LaTeX 入门](../tools/latex.md#图表)
-   [LaTeX 数学公式大全](https://www.luogu.com.cn/blog/IowaBattleship/latex-gong-shi-tai-quan)
-   [LaTeX 各种命令，符号](https://blog.csdn.net/anxiaoxi45/article/details/39449445)

Khi dùng hãy注意 [格式要求](../intro/format.md).

### Bối cảnh đề

Bối cảnh nên尽量简短. Nếu dài, nên tách khỏi mô tả đề.

Tuyệt đối tránh bối cảnh làm ảnh hưởng nghiêm trọng đến hiểu题意.

必要时 có thể提供两版本: gắn bối cảnh và bản mô tả简洁.

### Mô tả đề

Nói ngắn gọn: mô tả đề phải **rõ ràng, dễ hiểu**.

Mọi định nghĩa có thể không hiểu phải được giải thích, không nên出现概念 chưa định nghĩa. Ví dụ: trong [CF1172D Nauuo and Portals](https://codeforces.com/problemset/problem/1172/D), phải解释 “portal”.

Mỗi khái niệm nên dùng một từ duy nhất. Ví dụ: không nên lúc gọi “chi phí”, lúc gọi “giá trị”.

Không nên dùng từ với nghĩa khác nghĩa thông dụng mà không giải thích. Ví dụ: không nên dùng “đường đi” để chỉ một cạnh.

Phải đảm bảo đề không tự mâu thuẫn. Ví dụ: trong [CF1173A Nauuo and Votes](https://codeforces.com/problemset/problem/1173/A), không把 “?”作为一种 “result”，因为 “?” nghĩa là “có nhiều kết quả khả dĩ”.

Phải đảm bảo đề không thể bị hiểu sai rồi tự hợp lý hóa, dù cách hiểu đó trái thường识. Ví dụ: trong [CF1172D Nauuo and Portals](https://codeforces.com/problemset/problem/1172/D), định nghĩa rườm rà “walk into” vs “teleport” để tránh hiểu rằng đi vào cổng sẽ bị teleport lặp đi lặp lại giữa các cổng.

Đọc mô tả phải hiểu từng câu và nhiệm vụ. Ít nhất trong đoạn kế tiếp phải giải thích疑惑, chứ không nên đợi nhiều đoạn, hoặc phải xem IO format mới hiểu,甚至 phải đoán từ sample. Ví dụ: trong [「GuOJ Round #1」琪露诺的冰雪宴会](https://github.com/OI-wiki/problemset/blob/master/contest/online/GuOJ/OI%20Archive%20-%20GuOJ1171.pdf), mục tiêu “雾之湖最终能接收到的最大水量” chỉ xuất hiện ở output, cộng thêm câu “灵梦当然能很快算出来清理完全部小溪的总费用是多少” dễ误导, khiến đọc sai题意; nên nêu mục tiêu trong mô tả. (Ví dụ này còn có vấn đề bối cảnh ảnh hưởng hiểu题意.) Lỗi tương tự cũng xuất hiện ở [CF1423(4)N Bubblesquare Tokens](https://codeforces.com/problemset/problem/1423/N), mục tiêu chỉ xuất hiện ở output.

### Định dạng vào/ra

Định dạng vào/ra chỉ cần **rõ ràng và đầy đủ**. Không có要求 cứng,建议参照 CF 的写法，参考 [CF 出题人须知][3].

Để tiện cho thí sinh,最好说明每个变量意义，除非意义太长无法一句话说清（这时可写 “xem mô tả đề”）.

Nếu output có số thực,尽量 dùng [SPJ](#special-judge) để限制 sai số，thay vì yêu cầu “giữ x chữ số thập phân”.

“Giữ x chữ số” có thể đòi độ chính xác无限。Ví dụ: yêu cầu 3 chữ số, đáp án thực là $0.0015$，chỉ cần sai số khiến kết quả nhỏ hơn $0.0015$，dù输出 $0.00149999\cdots$ 仍会判错。

Nếu không dùng SPJ, hãy đảm bảo yêu cầu độ chính xác là hữu hạn, ví dụ: làm tròn đến 3 chữ số thập phân. Đặt đáp án chuẩn $ans$，数据保证对于任意满足 $\frac{|x-ans|}{\max(1,ans)}<10^{-9}$ 的 $x$，làm tròn rồi kết quả trùng với làm tròn của $ans$.

Một số câu mẫu:

```latex
输入的第一行包含三个正整数 $n$, $m$, $k$ ($1\le n,m\le 2\cdot 10^5$, $1\le k\le 100$) — $n$ 表示数列的长度，$m$ 表示操作个数，$k$ 的意义见题目描述．
```

```latex
输入的第二行包含 $n$ 个非负整数 $a_1,a_2,\ldots,a_n$ ($1\le a_i\le 10^9$) — 题目给出的数列．
```

```latex
接下来的 $m$ 行中的第 $i$ 行包含两个正整数 $l_i$ 和 $r_i$ ($1\le l_i\le r_i\le n$)，表示第 $i$ 次操作在区间 $[l_i,r_i]$ 上进行．
```

```latex
接下来的 $n-1$ 行，每行包含两个正整数 $u$ 和 $v$ ($1\le u,v\le n$)，表示 $u$ 和 $v$ 之间由一条边相连．

数据保证给出的边能构成一棵树．
```

```latex
输入的唯一一行包含一个由小写英文字母构成的非空字符串，其长度不超过 $10^6$．
```

```latex
输入的第二行包含一个小数点后不超过三位的实数 $x$ ($-10^6\le x\le 10^6$)，意义见题目描述．
```

```latex
输出包含一个实数，当你的输出与标准答案之间的绝对误差或相对误差小于 $10^{-6}$ 时视作正确．
```

```latex
输出的第二行包含 $n$ 个正整数，表示你构造的一组方案 — 其中第 $i$ 个数表示你打出的第 $i$ 张牌的编号．

如果有多组合法的答案，可以任意输出其中一组．
```

???+ note "Dữ liệu đầu vào do RNG sinh trong code thí sinh"
    Một số bài vì dữ liệu quá lớn, để tránh đọc vào tốn thời gian, yêu cầu thí sinh tự sinh dữ liệu trong code theo bộ sinh đã cho, thay cho đọc từ input tiêu chuẩn/tệp.
    
    Cách này cần cân nhắc kỹ vì có nhiều nhược điểm:
    
    -   Có thể引入随机性 mà lời giải đúng không cần, hoặc khiến构造数据 khó
    -   Tăng độ难理解 input
    -   Nếu RNG đóng gói kém, việc hiểu cách dùng bộ sinh đã khó
    -   Nếu thí sinh không dùng ngôn ngữ được khuyến nghị, phải tự viết bộ sinh
    
    Cách này thường để tránh đọc vào tốn thời gian, nên một phương án thay thế là phát một [mẫu tối ưu I/O](./io.md) đủ nhanh để保证 thời gian đọc gần như一致; như vậy dù đọc lâu cũng không ảnh hưởng差异 giữa thí sinh. Một giải pháp khác là gói đề thành dạng gọi hàm (không phải IO) giao互题, dù không có tương tác, giao互题 vẫn giúp统一 thời gian đọc; IOI dùng方案 tất cả đề là交互. Tuy nhiên hai方案 đều限制 ngôn ngữ và cần ra đề hỗ trợ từng ngôn ngữ.
    
    Quay lại本质, cũng nên考虑: dữ liệu quá lớn是否必要, có thể dùng dữ liệu nhỏ hơn đạt mục đích không, và是否 cần卡掉 cách làm chỉ kém hơn chút về độ phức tạp.

### Giới hạn dữ liệu

Theo CF, giới hạn dữ liệu nên写在 input format, nhưng trong nước thường写 ở cuối đề.

Lỗi phổ biến nhất là thiếu完整. Mỗi số, mỗi chuỗi trong input đều cần界定 rõ. Các ví dụ input/output ở trên có一些写法 đúng.

Các thiếu sót thường见:

1.  “整数” nhưng thiếu “整” (ví dụ không nói có thể âm/dương).
2.  Chỉ nói “整数” mà không nói “正整数”, và范围 chỉ có上限没有下限.
3.  Chuỗi không nói bộ ký tự.
4.  Số thực không nói số chữ số sau dấu chấm.
5.  Một số biến không có范围.

Cần đảm bảo lời giải chuẩn chạy được trên **mọi dữ liệu** trong phạm vi đề bài.

???+ note "Về “dữ liệu đảm bảo ngẫu nhiên”"
    Một số đề “bảo đảm dữ liệu sinh ngẫu nhiên”. Nhiều khi限制 này không tối ưu, vì “ngẫu nhiên” không明确限制数据, gây khó判断范围 và提供 hack.
    
    Thường có thể thay “ngẫu nhiên” bằng性质 dữ liệu cần cho lời giải. Ví dụ, thay “sinh ngẫu nhiên cây” bằng限制 chiều cao cây.
    
    Nếu必须保证 ngẫu nhiên, nên指定 thao tác ngẫu nhiên cụ thể. Ví dụ, sinh cây làRandom chọn cha hayRandom sinh Prüfer.
    
    Lưu ý: thuật toán không định型 và thuật toán phụ thuộc ngẫu nhiên của dữ liệu là khác nhau. Loại đầu có xác suất cao đúng với mọi dữ liệu; loại sau chỉ đúng với phần lớn dữ liệu, nhưng với một số dữ liệu đặc biệt thì không thể đúng.

### Sample

Sample nên đủ mạnh để bắt lỗi đơn giản. Người đọc sai đề nên có thể发现 qua sample.

Bài có nhiều thao tác, mỗi thao tác nên xuất hiện trong sample.

Bài có nhiều kiểu output (ví dụ [CF1173A Nauuo and Votes](https://codeforces.com/problemset/problem/1173/A)), mỗi kiểu output nên xuất hiện. Ngoại lệ: thực tế không thể vô nghiệm nhưng yêu cầu判断 có nghiệm.

### Giải thích sample

Đề càng复杂, càng khó hiểu thì càng cần解释 sample chi tiết.

Đề càng dễ thì càng nên có giải thích chi tiết.

Giải thích chi tiết có thể kèm hình.

Sample lớn có thể không cần giải thích.

Để考虑 người mù màu,最好不要让颜色成为理解 sample 的必要条件。Có thể dùng hình màu để đẹp, nhưng nếu必须用 màu传递信息,最好 tránh đỏ-vàng hoặc đỏ-xanh同时出现。

## Giới hạn thời gian, bộ nhớ và điểm từng phần

Giới hạn thời gian/ bộ nhớ nhằm卡掉 các cách sai độ phức tạp (và tránh评测 quá lâu).

原则上, thời gian nên chọn càng lớn càng tốt nhưng không让 cách sai qua.

一般要求:

1.  Ít nhất gấp đôi thời gian std ở trường hợp xấu nhất.
2.  Nếu cho phép Java, phải đảm bảo Java qua.
3.  Không让 cách sai qua (trừ trường hợp cố tình放过).

Để卡 sai tốt hơn trong khi放过 cách đúng hằng lớn, có thể tăng同时 dữ liệu và thời gian. Nhưng注意, đôi khi lời giải đúng có hằng rất lớn khi dữ liệu tăng, khiến差距 không tăng.

Trong赛制 có điểm phần, có thể thiết kế dữ liệu梯度 hoặc范围稍小 để让错解 tốt và正解 hằng lớn đều không过, nhưng仍能拿较高部分分。

注意: 当数据范围小于 $5\cdot 10^5$，需考虑是否能用 [指令集优化](https://ouuan.github.io/post/n方过百万-暴力碾标算——指令集优化的基础使用).

一般空间限制应足够大, trừ khi có cách tối ưu space rất巧妙值得卡掉. Khi đó có thể设置空间限制较松的部分分. 值得注意: nếu không muốn卡掉空间大做法, bài cấu trúc dữ liệu通常需要设置较大空间限制.

> Một đề hay phải具有选拔性, có đủ分 hóa. Nên có至少 4 档部分分 để新人 có điểm,高手展示实力.
>
> ——vfk《UOJ 精神之源流》

Phần điểm thường chia thành两类: dữ liệu小 hơn và性质特殊.

Dữ liệu nhỏ thường nên có多档; dù không nghĩ ra cách某复杂度, vẫn có thể给它一档分. Để tránh卡常, có thể设置一档极限数据除以二的部分分.

“Dữ liệu có梯度”最好用多档部分分替代.

Phần性质特殊 cần tùy đề. Lý tưởng là能引导思考正解. Khác với phần dữ liệu nhỏ, nếu không针对某性质的做法,最好不要给它一档分. Ví dụ: [「CTS2019」随机立方体](https://loj.ac/problem/3119) 的 $k=1$ 这一档 bị nhiều人吐槽 vì妨碍思考正解.

Nếu cách chấm điểm khác mặc định (ví dụ OI 绑 subtask),必须在题面说明.

Không推荐 dùng “% dữ liệu thỏa …”尤其 khi có nhiều biến. Ví dụ, “$30\%$ 的数据满足 $n \le 1000$” và “$40\%$ 的数据满足 $m \le 100$” có thể描述 70% 或 40%. Thông thường, bảng subtask hoặc bảng范围 là tốt hơn.

## Sinh dữ liệu

Sinh dữ liệu là bước cần thiết, cũng cần cho对拍. Nắm技巧 sẽ khiến quá trình nhẹ hơn và dữ liệu mạnh hơn.

### Sinh dữ liệu ngẫu nhiên

#### Sinh số ngẫu nhiên

Tham khảo [随机函数](../misc/random.md).

Nhấn mạnh: khi cần sinh số có giá trị lớn hơn phạm vi RNG, **không** dùng `rand() * rand()`; phân bố rất không均匀.

Ngoài ra, khuyến nghị dùng [testlib](../tools/testlib/generator.md) để sinh dữ liệu,保证同一 seed sinh cùng số trên các平台, và seed tự动生成 theo tham số dòng lệnh.

#### Sinh hoán vị ngẫu nhiên

Dùng `std::shuffle(a, a + n, rng)`; `rng` là RNG, ví dụ `std::mt19937 rng(std::chrono::steady_clock::now().time_since_epoch().count())`.

**Không** dùng `std::random_shuffle` (C++14 deprecate, C++17 remove).

#### Sinh đoạn ngẫu nhiên

Sai常见: trong $[1,n]$ chọn trái $l$, rồi trong $[l,n]$ chọn phải $r$ => đoạn偏右.

Đúng hơn (khuyến nghị): chọn hai số trong $[1,n]$, lấy nhỏ làm trái, lớn làm phải.

均匀真正: chọn $x$ trong $[0,n]$. Nếu $x=0$，trong $[1,n]$ chọn $y$，đoạn $[y,y]$;否则用方法 “đúng hơn”.

#### Sinh cây ngẫu nhiên

Cách常用: với mỗi nodes $i$ từ $2$ đến $n$, chọn cha trong $[1,i-1]$. Cây không均匀, độ cao kỳ vọng $O(\log n)$.

Một cách khác: chọn cha của $i$ trong $[i\cdot low, i\cdot high]$; nếu $low,high$ hợp lý, có thể tạo cây mạnh.

均匀真正: dùng [Prüfer 序列](../graph/prufer.md), sinh một Prüfer random rồi dựng cây. Khi đó độ cao kỳ vọng $O(\sqrt n)$.

Ngoài ra, có thể random một hoán vị để đổi nhãn hoặc xáo trộn thứ tự cạnh.

### 构造 dữ liệu

#### Bài liên quan đoạn

Cấu tạo常用: độ dài rất nhỏ (đặc biệt toàn điểm), độ dài rất lớn (đặc biệt toàn bộ dãy).

#### Bài cần phân tích ước

Số lượng thừa số nguyên tố (tính cả lặp) càng nhiều: lũy thừa của 2.

Số lượng thừa số nguyên tố khác nhau càng nhiều: tích của vài số nguyên tố nhỏ nhất.

Số ước càng nhiều: tham khảo dãy [A002182](http://oeis.org/A002182) trên OEIS.

#### Bài trên cây

Cấu tạo常用:

-   Chuỗi
-   Hoa cúc
-   Cây nhị phân hoàn chỉnh
-   Thay mỗi node của cây nhị phân hoàn chỉnh bằng một chuỗi dài $\sqrt n$
-   Hoa cúc treo một chuỗi
-   Chuỗi treo vài điểm
-   Cây cao $d$ ($d>1$): gốc có hai con; trái là chuỗi dài $d-1$, phải là cây cùng dạng cao $d-1$.

Nếu không ở phòng thi, có thể dùng [Tree-Generator](https://github.com/ouuan/Tree-Generator) để sinh nhiều loại cây.

### Sinh dữ liệu hàng loạt

Khuyến nghị dùng tham số dòng lệnh + bat/sh.

Ví dụ:

`gen.cpp`:

```cpp
#include "testlib.h"

using namespace std;

int n, m, k;
vector<int> p;

int main(int argc, char* argv[]) {
  registerGen(argc, argv, 1);

  int i;

  n = atoi(argv[1]);
  m = atoi(argv[2]);
  k = rnd.next(1, n);

  for (i = 1; i <= n; ++i) p.push_back(i);

  shuffle(p.begin(), p.end());
  // Dùng rnd.next() để shuffle

  printf("%d %d %d\n", n, m, k);
  for (i = 0; i < n; ++i) {
    printf("%d%c", p[i], " \n"[i == n - 1]);
    // Dùng chuỗi như mảng: giữa là khoảng trắng, cuối là xuống dòng; mẹo hay khi sinh dữ liệu
  }

  return 0;
}
```

`gen_scripts.bat`:

```bat
gen 10 10 > 1.in
gen 1 1 > 2.in
gen 100 200 > 3.in
gen 2000 1000 > 4.in
gen 100000 100000 > 5.in
```

Lợi ích: với các test khác nhau chỉ cần một generator, và dễ sửa tham số từng test.

### Yêu cầu khi sinh dữ liệu

Dữ liệu nên包含 giá trị nhỏ nhất và lớn nhất của các tham số.

Dữ liệu nên覆盖 các trường hợp biên.

Dùng subtask thì dữ liệu (input/output)最好覆盖各范围, không chỉ giá trị max.

Để防止特判过掉, có thể kết hợp nhiều cấu造 trong một test, hoặc phần lớn là构造, kèm ít random.

Dữ liệu nên包含各种构造, kể cả khi không biết错解 nào sẽ挂 trên cấu造 đó. (Trong赛制按点给分需 cân nhắc.)

Nếu biết một错解 có问题 đúng sai (mà người bình thường nghĩ ra), hãy尽量卡掉.

Nhắc lại: nếu có nguy cơ tràn số,一定要卡掉 cách会溢出. Trong赛制 phần分, không nên让 người không dùng long long 得分 thấp hơn brute force.

Nếu có pretests, nên đủ mạnh (và尽量少). Tức trong pretests (với ít test) phải包含所有已知叉点.

Nếu muốn出现少量而非无 FST, vẫn phải保证 pretests mạnh, vì thi thực tế có thể出现意外 lỗi,导致 FST vượt dự kiến.

### 格式 dữ liệu

Dưới đây là一些要求 thường见 cho input, làm参考:

> 1.  Dùng format xuống dòng của môi trường test.
> 2.  Dòng cuối của file phải có newline, tức ký tự cuối là `\n`.
> 3.  Đầu và cuối mỗi dòng không có khoảng trắng.
> 4.  Không có quá 1 dấu cách liên tiếp.

Trong Windows, newline là `\r\n`, trong Linux là `\n`. Nếu đọc dữ liệu Windows trên Linux, có thể gây lỗi khi đọc chuỗi, dẫn đến kết quả khác nhau; nếu so sánh output sinh từ Windows và Linux, newline khác nhau cũng gây khác biệt. Để一致 hành vi, phải chuyển newline về môi trường chạy.

Cách sinh dữ liệu newline Linux:

1.  Sinh trực tiếp trên Linux.
2.  Dùng [`dos2unix`](https://dos2unix.sourceforge.io/) (trong Cygwin/MinGW).
3.  Mở file output ở chế độ nhị phân và dùng `\n`.
4.  Tự viết tool dựa trên `dos2unix.cpp` trong [trang này](https://help.luogu.com.cn/manual/luogu/problem/testcase-format#附录windows-环境下造数据注意事项).

## Special Judge

[Hướng dẫn viết SPJ](../tools/special-judge.md)

Bài yêu cầu output方案 hoặc số thực thường cần SPJ; các bài khác tùy情况 cũng có thể cần. Trên CF, mọi bài đều phải dùng checker dựa trên testlib; ví dụ output nhiều số nguyên dùng ncmp, thí sinh có thể输出任意空白.

Checker thường viết bằng testlib. Vì cần xử lý各种 output không hợp lệ, độ鲁棒 phải cực mạnh; không dùng testlib很难写好.

Khi viết checker cần chú ý:

1.  Cần xử lý mọi output không hợp lệ, nên kiểm tra每 biến读入 có在范围合法 (`readInt(minvalue, maxvalue)`). Ví dụ: biến dùng làm chỉ số mảng phải kiểm tra phạm vi,否则 có thể越界, đôi khi RE, đôi khi AC sai.
2.  Nguyên tắc: checker không kiểm tra khoảng trắng (không dùng `readSpace()`、`readEoln()`、`readEof()`; testlib tự检查是否有多余输出).

## 题解

Mục tiêu của题解 là让 người dự kiến参赛都能看懂. Vì vậy题解官方 cần详细 hơn bình thường.

### 关于部分分

Trong bài có部分分, có thể viết cách làm部分分.

### 关于知识点

Các kiến thức dùng trong lời giải cần指出 rõ. Với kiến thức难度 tương当 với đề,最好给资料学习 (ví dụ link blog).

### 关于定义

Không nên tự dưng xuất hiện概念. Ví dụ DP cần解释 rõ定义 trạng thái.

### 关于细节

Chi tiết hiện thực nếu tinh巧最好 viết ra; nếu “详见代码” cũng được, nhưng nên thêm注释 trong code.

### 标程

标程 nên bỏ phần冗余. Ví dụ: giữ cả template define dài (thường dùng CF) mà phần lớn không dùng là不好.

Nếu có chi tiết hiện thực không giải thích,最好加注释适量.

## 比赛

### Thông báo độ khó phải真实

> Remember that authors tend to underestimate the difficulty of their problems.
>
> ——Nhắc nhở trên trang Codeforces PROPOSE A PROBLEM

Ra đề thường估计 sai độ khó; nếu要写 độ难 trong thông báo, cần慎重,最好 nhờ người验题 trước và评估.

### Phân bổ độ khó đề

Trong比赛 kiểu OI nội địa, thường chỉ cần难度 tổng của 3 đề tương đương难度 cuộc thi.

Trong比赛 kiểu CF/ATC, cần保证难度 tăng dần (dù估计 khó), và tránh gap lớn. Có thể chia một đề thành easy/hard (subtask) để giảm gap, nhưng分 subtask cần慎重, nhiều người không thích subtask của CF（[Are subtasks evil?](https://codeforces.com/blog/entry/71700)），理由包括:

-   Do赛制, làm easy trước rồi hard có thể罚时 ít hơn và tổng分 cao hơn
-   Điểm subtask thường không tương xứng难度
-   Easy version nhiều khi không phải đề合格 (không thú vị)
-   Lời giải easy thường không giúp nghĩ ra lời giải hard

### Phân bổ kiến thức đề

Một contest nên覆盖 rộng kiến thức (trừ专题训练).

Phản例经典: CTS2019包含 DP, kỳ vọng, tổ hợp, bao hàm-loại trừ, đa thức, v.v.

> Tôi phải chọn sáu bài từ năm bài, tôi cũng bó tay.
>
> ——Lời giải thích của nhóm ra đề CTS2019 khi không nhận đủ投稿

## Nền tảng ra đề

### Polygon

Polygon là nền tảng合作 ra đề rất mạnh,适合作多人合作 hoặc单人 ra đề (đặc biệt đa thiết bị). Xem [Polygon 简介](../tools/polygon.md).

### Codeforces

Codeforces là trang CP nổi tiếng nhất, đề chất lượng cao,适合 người có经验 ra đề và muốn nâng trình,出一套 đề chất lượng. Nhược点 là审核 chậm (thường vài tháng), nhưng có thể chuẩn bị trong thời gian审核 (dù có风险 bị否).

#### Điều kiện ra đề

-   Màu xanh dương và参加至少 25 场 rated；
-   Màu tím và参加至少 15 场 rated；
-   Màu cam và参加至少 5 场 rated；
-   Màu đỏ hoặc đen-đỏ.

#### Nộp申请 contest

Có资格后, sidebar có nút [Propose a contest/problems](http://codeforces.com/proposals/new-contest).

Vào đó,先写 contest proposal (PROPOSE A CONTEST), rồi写 problem proposal và thêm vào contest.

Sau khi确定题目, mở contest proposal để审核.

#### Chuẩn bị đề trên Polygon

参见 [Polygon 简介](../tools/polygon.md).

#### Liên hệ quản trị

Liên hệ quản trị có 2 mục đích:

1.  Tăng tốc审核.
2.  Khi进入准备阶段, quản trị提供建议与帮助.

Cách正规 là提交 proposal trong hệ thống,审核后讨论在 proposal comment.

Thực tế, nếu lâu không duyệt, có thể私信 quản trị (dù CF写 “Don't send private messages or emails to coordinators”，nhưng 300iq在 [评论](http://codeforces.com/blog/entry/64077#comment-478933) 表示可私信).

### Comet OJ

[Liên kết Comet OJ](https://www.cometoj.com/)

已不活跃（截至 2021 年 11 月，最后一场比赛为 2020 年 1 月）.

申请出题：<https://info.cometoj.com/contests/Questionnaire_IssuerInfo/>

### CodeChef

Nền tảng CP của Ấn Độ, có 3赛制: Long Challenge 10 ngày có challenge, Cook-Off 2.5h kiểu ICPC, LunchTime 3h kiểu IOI.

FAQ ra đề：<https://www.codechef.com/wiki/faq-problem-setters>

指南 ra đề：<https://www.codechef.com/problemsetting>

### AtCoder

Nền tảng CP Nhật, liên hệ ra đề：<contest@atcoder.jp>.

### UOJ & LOJ

OJ trong nước ít tổ chức contest.

### 洛谷

Tham gia ra đề cần等级认证 nhất định; sau khi tạo contest,负责人 gửi申请 tại [hệ thống工单](https://www.luogu.com.cn/ticket).

Chuẩn公开赛：<https://help.luogu.com.cn/rules/academic/opencontest-standard>

## 参考资料

1.  [vfk《UOJ 精神之源流》][1]

2.  [王天懿《论偏题的危害》][2]

3.  [CF 出题人须知][3]（[bản ảnh có thể truy cập](https://github.com/OI-wiki/libs/blob/master/topic/rules.jpg)）

4.  [CF 出题人的自我修养][4]

Bài viết được chuyển từ [ouuan 的出题规范](https://ouuan.github.io/post/ouuan-的出题规范/) và có chỉnh sửa, bổ sung.

[1]: https://vfleaking.blog.uoj.ac/blog/909 "vfk《UOJ 精神之源流》"

[2]: https://github.com/OI-wiki/libs/blob/master/topic/7-%E7%8E%8B%E5%A4%A9%E6%87%BF-%E8%AE%BA%E5%81%8F%E9%A2%98%E7%9A%84%E5%8D%B1%E5%AE%B3.ppt "王天懿《论偏题的危害》"

[3]: https://docs.google.com/document/d/e/2PACX-1vRhazTXxSdj7JEIC7dp-nOWcUFiY8bXi9lLju-k6vVMKf4IiBmweJoOAMI-ZEZxatXF08I9wMOQpMqC/pub "CF 出题人须知"

[4]: https://github.com/OI-wiki/libs/blob/master/topic/CF%E5%87%BA%E9%A2%98%E4%BA%BA%E7%9A%84%E8%87%AA%E6%88%91%E4%BF%AE%E5%85%BB.md "CF 出题人的自我修养"
