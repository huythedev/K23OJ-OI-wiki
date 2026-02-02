author: Ir1d, Planet6174, abc1763613206, StudyingFather, cjsoft, Marcythm, luoguyuntianming, ChungZH, Xeonacid, YZircon, i-Yirannn, H-J-Granger, NachtgeistW, YuzhenQin, Andycode3759, HHH2309, shigengxin123456, Re-Ori, hcx1204

## Giới thiệu赛事

**Olympiad Tin học** (tiếng Anh: Olympiad in Informatics, viết tắt: OI) là cuộc thi học thuật phổ biến trong học sinh, cùng性质 với thi vật lý, toán. OI đánh giá năng lực vận dụng thuật toán, cấu trúc dữ liệu và toán học để viết chương trình giải quyết vấn đề thực tế.

OI có nhiều loại; chỉ riêng Trung Quốc đã có:

-   全国青少年信息学奥林匹克联赛（NOIP）
-   全国青少年信息学奥林匹克竞赛（NOI）
-   全国青少年信息学奥林匹克竞赛冬令营（WC）
-   国际信息学奥林匹克竞赛中国队选拔赛（CTSC）

Các OI quốc tế:

-   国际信息学奥林匹克（IOI）
-   美国计算机奥林匹克竞赛（USACO）
-   日本信息学奥林匹克（JOI）
-   亚太地区信息学奥林匹克（APIO）

    ……

Với đa số thí sinh, mùa thi mới bắt đầu từ vòng 1 CSP-J/S tháng 9.

Ở Trung Quốc, ngôn ngữ được phép chỉ có C++ (trước đây có C và Pascal nhưng đã dừng). Mỗi kỳ thi có quy định phiên bản C++ khác nhau. Đề thi chủ yếu là thuật toán hoặc cấu trúc dữ liệu, hình thức gồm bài truyền thống (input/output file) và bài phi truyền thống (nộp đáp án, tương tác,补全代码, v.v.).

## Giới thiệu赛制

###赛制 OI

Thí sinh chỉ có một lần nộp. Trong thi không thấy kết quả chấm, điểm công bố sau. Mỗi bài có nhiều test; điểm theo số test AC; mỗi test có thể có部分分, dù chỉ qua một phần dữ liệu vẫn có điểm.

???+ note "Công cụ tự chấm selfEval"
    Hiện nay, một số kỳ thi thuộc系列 NOI cung cấp công cụ selfEval. selfEval tích hợp trong NOI Linux bản tùy biến. Sau khi NOI2023 công bố và dùng chính thức, selfEval được dùng cho NOI, APIO (Trung Quốc), WC, v.v. Thí sinh dùng selfEval để test trên một bộ dữ liệu (pretest) và nhận phản hồi. Mỗi kỳ thi có giới hạn số lần tự chấm (NOI2024: 50, NOI2025: 30), và dữ liệu pretest không được thấy. Vì pretest khác dữ liệu chính thức nên chỉ dùng để debug, không tính điểm chính thức. Thí sinh test nhiều lần cùng một bài thì dùng cùng một bộ pretest.

CSP-J/S vòng 2, NOIP, tỉnh选, NOI đều là OI 赛制.

###赛制 IOI

Thí sinh có nhiều lần nộp. Chấm实时 và trả kết quả; nộp sai không bị phạt. Mỗi bài có nhiều test, điểm theo số test AC.

APIO, IOI đều là IOI 赛制. Trong nước cũng dần chuyển về IOI 赛制.

###赛制 Codeforces (CF)

[Codeforces](https://codeforces.com) là hệ thống chấm online, tổ chức contest định kỳ.

Đặc điểm: khi thi chỉ test một phần dữ liệu (Pretests), sau khi thi mới trả kết quả đầy đủ (System Tests). Trong thi có thể nộp nhiều lần,允许 Hack code người khác (nghĩa là nộp dữ liệu khiến code người khác sai). Nếu muốn hack, phải lock code (tức không thể nộp lại bài). Khi hack không được tải code về test, code được chuyển thành ảnh.

Codeforces còn có赛制 Extended ICPC (ICPC+). Ở赛制 này, trong thi test toàn bộ dữ liệu, nhưng sau khi thi có 12 giờ hack. Hack允许 tải code về test.

## Các giải chính

### CSP-J/S

**CSP-J/S** (Certified Software Professional Junior/Senior) là kỳ thi năng lực phần mềm do CCF mở sau khi NOIP bị hủy năm 2019, trước 2025 mở cho mọi lứa tuổi, [sau đó đổi thành 12 tuổi trở lên](https://www.noi.cn/xw/2025-02-13/837984.shtml).

CSP-J/S chia hai nhóm: Junior (CSP-J) và Senior (CSP-S). Lịch có vòng 1 (thường tháng 9) và vòng 2 (thường tháng 10). Vòng 1 là lý thuyết, kiểm tra kiến thức máy tính và thuật toán/toán cơ bản; vòng 2 là thi máy. Cả hai nhóm có 4 bài; thời gian 3.5h cho Junior, 4h cho Senior (CSP-S 2019 dùng赛制 NOIP cũ, thi 2 ngày, mỗi ngày 3 bài 3.5h). Vòng 1 mở cho học sinh 12+; qua筛选 mới vào vòng 2.

Đăng ký vòng 1/2 và khiếu nại题目 cần nộp phí cho CCF.

Cả hai vòng đều xếp hạng theo tỉnh và cấp chứng nhận hạng nhất/nhì/ba.

### NOIP

**NOIP** (National Olympiad in Informatics in Provinces) là cuộc thi tin học trung học ở Trung Quốc (gồm HK/Macau).

赛制 cũ (trước 2018): chia普及组/提高组, 2018 thử nghiệm入门组 ở Thượng Hải; chia sơ khảo và chung khảo. Sơ khảo kiểm tra kiến thức máy tính và thuật toán cơ bản; chung khảo là thi máy. Thời gian thường là cuối tuần thứ hai tháng 11: thứ bảy sáng提高组 8:30-12:00 (3.5h, 3 bài), chiều 14:30-18:00 普及组 (3.5h, 4 bài), chủ nhật sáng提高组 8:30-12:00 (3.5h, 3 bài). Toàn quốc dùng cùng đề, nhưng quy tắc giải thưởng do CCF thống nhất và công bố trên [NOI官网](http://www.noi.cn). Điểm chuẩn nhất tỉnh khác nhau.

NOIP bị CCF暂停 ngày 16/8/2019, khôi phục ngày 21/1/2020. Từ 2020, NOIP thay đổi:

-   Hủy sơ khảo, thay bằng vòng 1 CSP-J/S；
-   Hủy普及组, thay bằng CSP-J, NOIP chỉ còn một组 hướng tới提高；
-   Lịch từ 2 ngày 6 bài (mỗi ngày 3.5h) rút còn 1 ngày 4 bài 4.5h；
-   Thí sinh cần xếp hạng đủ tốt ở CSP-S vòng 2 mới có资格 NOIP, quota theo tỉnh; quota dựa vào số người dự thi và成绩 năm trước.

Đăng ký NOIP và khiếu nại题目 không cần thêm phí.

NOIP xếp hạng và trao giải theo tỉnh. Tính đến 2019, nhiều trường đại học cho自主招生 nếu đạt一等奖省.

> Tháng 1/2020, Bộ GD Trung Quốc ban hành [意见](http://www.moe.gov.cn/srcsite/A15/moe_776/s3258/202001/t20200115_415589.html),指出 từ 2020 không tổ chức自主招生, thay bằng强基计划.

### 省队选拔赛

**省队选拔赛** (省选) chọn đội tỉnh dự NOI, thường tháng 1~4. Thường 2 ngày, mỗi ngày 3 bài 4.5h.

Đề do各 tỉnh tự quyết; xu hướng nhiều tỉnh联合命题.

Chỉ tiêu đội tỉnh có công thức phức tạp, liên quan成绩 và人数; NOIP thường占一定比例. Quy định: học sinh cấp II chỉ được选为 E 类, không参加 A/B. A 类 5 người ([至少 1 女](https://www.noi.cn/xw/2024-08-26/829152.shtml)), còn lại按名额和分数入 B. Số suất một trường参加 NOI không vượt quá 1/3 tổng A+B (làm tròn); nữ sinh cao điểm nhất vào A không tính vào tỷ lệ (gọi là 1/3 限制, xem [说明](https://www.noi.cn/xw/2022-12-14/781364.shtml)).

Từ 2020,省队选拔由 CCF统一命题评测; tỉnh có năng lực có thể tự ra đề nhưng cần批准. Từ 2024,恢复各省自主命题; tỉnh có nhu cầu có thể联考 hoặc dùng đề tỉnh khác, nhưng需 CCF批准.

### NOI

**NOI** (National Olympiad in Informatics) là giải cao nhất trong nước với đội tỉnh (gồm HK/Macau).

NOI thường tháng 7. Thí sinh chia chính thức và夏令营. Thí sinh chính thức chia A/B/C; A/B là đội tỉnh chính thức (A có cộng 5 điểm), C là suất mời (đóng góp cho CCF). 夏令营 chia D/E, tương ứng高中/初中非正式. 夏令营 nếu vượt chuẩn chỉ nhận chứng nhận không có huy chương (giá trị thấp hơn). Top 50 thí sinh chính thức vào国家集训队, có资格保送.

Trên quốc tế, để tránh nhầm với các giải cũng叫 NOI, đôi khi gọi CNOI.

### CTT

**CTT** (China Team Training) là tập huấn/选拔 đội IOI vào mùa đông, gồm 3-4 cuộc test. Ngoài队 quốc gia, một số thí sinh NOI成绩 tốt cũng vào với tư cách “精英集训”.

CTT cùng作业等组成 giai đoạn 1 chọn đội. Từ 2021, top 30 giai đoạn 1 thành国家候选队,进入阶段 2 (WC).

### WC

**WC** (Winter Camp) là活动 mùa đông tại địa điểm NOI năm đó. Dù chủ yếu用于培训 và选拔 đội, thí sinh NOIP/CSP-S vòng 2成绩 tốt cũng có thể参加 như营员 không chính thức.

Nội dung gồm đào tạo và test; điểm test cộng với điểm trước để tính排名. Trước 2020 chỉ một bài test, đội tuyển và营员 dùng cùng đề; top 15 đội tuyển vào候选队 (CTS等). Từ 2021, CTS融入 WC,候选队 test 2 bài,营员 1 bài, đề có部分重合. Top 6候选队 vào phỏng vấn cuối, chọn 4 chính thức + 2 dự bị dự IOI.

### APIO

**APIO** (Asia-Pacific Informatics Olympiad) là cuộc thi cho học sinh khu vực AP. CCF tổ chức鏡像赛 Trung Quốc đầu tháng 5, kèm hoạt động培训.

Thí sinh APIO chia A/B. Top 6 A (kể cả đồng hạng) tham gia xét奖 quốc tế; B chỉ xét奖 trong Trung Quốc.

### CTS

**CTS** (tên cũ CTSC) dùng để选 đội quốc gia (6) từ候选队 (15) dự IOI, gồm 4 chính thức + 2 dự bị. Giống WC, thí sinh NOIP tốt năm trước也可参加 (không tham gia选拔).

APIO và CTS đăng ký theo tỉnh, thường按 NOIP成绩确定名单 (thời gian hai cuộc thi rất gần).

CTS 2020 ngừng do疫情, đội quốc gia chọn từ NOI; từ 2021, CTS被 WC 替代.

### IOI

**IOI** (International Olympiad in Informatics) là cuộc thi tin học quốc tế hàng năm cho học sinh trung học. Mỗi nước 4 người, thường có直播. IOI có Subtask, mỗi subtask một mức điểm.

### 学科营

#### 北京大学（PKU）

-   Trại đông tin học PKU (PKUWC)：tổ chức quanh WC.
-   Trại体验 tin học PKU (PKUSC)：thường tháng 6, thi tại phòng máy trường, Windows, hệ thống OpenJudge.
-   Lớp hè PKU cho học sinh (tin học)：tổ chức hè, hướng tới học sinh lớp 11 khối tự nhiên.

#### 清华大学（THU）

-   Hoạt động “đại trung衔接” mùa đông của khoa CS: tương đương WC, đôi khi viết tắt THUWC. Thường 2 ngày: sáng thi (ngày 1 chuẩn OI, ngày 2 “工程题” của THU), chiều học课程.

## OI ở các quốc gia/khu vực khác

### Mỹ: USACO

Website: <http://www.usaco.org/>

USACO có lẽ là OI nước ngoài quen thuộc nhất với thí sinh trong nước.

Mỗi năm từ冬 đến đầu春, USACO tổ chức contest online hàng tháng. Mỗi contest 3~5 giờ.

Theo官网, USACO có 4 mức难度 (trước 2015~2016 là 3):

-   Bronze: phù hợp người mới, đặc biệt chỉ học cơ bản (sắp xếp, nhị phân).
-   Silver: phù hợp người bắt đầu học kỹ thuật cơ bản (đệ quy, tìm kiếm, tham lam) và CTDL cơ bản.
-   Gold: gặp thuật toán phức tạp hơn (shortest path, DP) và CTDL nâng cao.
-   Platinum: cho người có năng lực thiết kế thuật toán vững; đề开放, thách thức bản thân.

Trong nước, OJ có USACO đầy đủ nhất là Luogu.

### Ba Lan: POI

Website: <https://oi.edu.pl/>

Nộp bài: <https://szkopul.edu.pl/p/default/problemset/>

POI là比赛 nước ngoài thí sinh省选 hay luyện.

Theo [官网 POI](https://oi.edu.pl/l/42/):

-   Vòng 1: 6 bài (trước kỳ 31 là 5), online；
-   Vòng 2: gồm một practice và hai round chính, practice 1 bài, mỗi round chính 2 bài；
-   Vòng 3: gồm một practice và hai round chính, practice 1 bài, mỗi round chính 3 bài.

Một số năm có ONTAK (trại POI), tương đương CTT.

Ngoài ra có比赛公开 PA (“thuật toán đại chiến”), website: <https://potyczki.mimuw.edu.pl/>.

Trong nước, POI đầy đủ nhất là BZOJ.

### Croatia: COCI

Website (EN): <http://www.hsin.hr/coci/>

Website (HR): <http://www.hsin.hr/honi/>

Độ khó跨度 lớn, từ phổ cập đến省选.

Trước đây COCI công khai题面,数据,题解,标程. Cuối 2017 ngừng更新题解/标程; mùa 2019-2020更新 trở lại.

Luogu, BZOJ, LibreOJ có một số đề COCI.

### Nhật: JOI

Website: <https://www.ioi-jp.org/>

JOI (日文: 日本情報オリンピック) công khai题面,数据,题解,标程. Hai năm gần đây JOI Final và Spring Camp có题面 tiếng Anh, không có题解 tiếng Anh; JOI Open có题面 và题解 tiếng Anh.

Quy trình JOI:

-   予選 (sơ khảo)
-   本選/JOI Final
-   春季合宿/JOI Spring Camp/JOISC
-   公开赛/JOI Open Contest

预赛难度较低; từ mùa 2019/2020,预赛分 nhiều轮. JOI Final khó từ提高- đến提高+; JOISC và JOI Open từ提高 đến NOI-.

Hầu hết đề JOI có thể nộp trên [AtCoder](https://atcoder.jp/). Trên JOI官网 hoặc AtCoder có更多 đề (题面 tiếng Nhật).

LibreOJ và BZOJ có nhiều đề JOI Final/JOISC/JOI Open gần đây.

### Nga: ROI

Website: <http://neerc.ifmo.ru/school/archive/index.html>

Nộp online: <https://contest.yandex.ru/roiarchive/> và Codeforces (một phần).

ROI (俄文: олимпиадная информатика) là OI của Nga.

Quy trình:

-   Municipal Stage
-   Regional Stage
-   Final Stage

LibreOJ có bản dịch đề ROI Final các năm gần đây.

Ngoài ra, một số比赛 lớn cho học sinh ở Nga:

-   信息学网络奥赛 (Интернет-олимпиады по информатике)
    -   Website: <http://neerc.ifmo.ru/school/io/index.html>
    -   Do nhóm ra đề ROI tổ chức.
-   全国中学生团队信息学竞赛 (Всероссийской командной олимпиады школьников)
    -   Website: <http://neerc.ifmo.ru/school/russia-team/index.html>
    -   Vòng loại Moscow Team Olympiad có thể nộp trên Codeforces.
-   Innopolis Open
    -   Website <https://olymp.innopolis.ru/en/ooui/information/>
-   中学生编程公开赛（Открытая олимпиада школьников по программированию）
    -   Website: <https://olympiads.ru/zaoch/>
    -   Theo官网,比赛对标 ROI.

### Canada: CCC & CCO

CCC (Canadian Computing Competition), CCO (Canadian Computing Olympiad), xem lịch sử và题 tại [官网](https://cemc.math.uwaterloo.ca/contests/past_contests.html#ccc).

Trên DMOJ có thể nộp [CCC](https://dmoj.ca/problems/?category=4) và [CCO](https://dmoj.ca/problems/?category=24), có题解.

CCC Junior/Senior gần mức NOIP 普及/提高; CCO lấy vàng thường cần mức NOI bạc.

### Singapore: NOI SG

Website: <https://noisg.comp.nus.edu.sg/noi/>

Tên đầy đủ Singapore National Olympiad in Informatics, trong语境 Singapore cũng gọi NOI.赛制 gồm Online Qualification Contest và Final Contest. Vòng资格 online đăng ký theo trường, thi tại trường, nộp qua mạng.成绩 chỉ xếp trong trường; top 5 không-zero được làm代表 dự chung kết.

Trong nước OJ thu thập đề NOI SG ít; có thể tìm đề/数据/标程 trên [GitHub chính thức](https://github.com/noisg).

### Đài Loan: 資訊奧林匹亞競賽

Đài Loan dịch informatics là “资讯” thay vì “信息”.

Thí sinh muốn dự IOI phải qua:

-   區域資訊學科能力競賽
-   全國資訊學科能力競賽
-   資訊研習營（TOI）

### Các quốc gia khác

-   Úc: AIO：<https://orac.amt.edu.au/hub/aio/>

    -   Độ khó tương tự NOI.

-   Anh: British Informatics Olympiad：<https://www.olympiad.org.uk/>

    -   Độ khó quá thấp.

-   Séc: Matematická olympiáda–kategorie P：<http://mo.mff.cuni.cz/p/archiv.html>

-   Romania: Olimpiada Nationala de Informatica：<http://olimpiada.info/>
    -   Đề/数据/题解 trong tab có chữ Subiecte.

## Các OI quốc tế khác

### BalticOI

**BalticOI** dành cho các nước vùng Baltic. BalticOI 2018 có 9 nước (Lithuania, Poland, Estonia, Finland, v.v.). Đề khó.

Trừ 2017, BalticOI công khai đề/数据/题解. Không có官网 cố định; mỗi năm host dựng site mới. Xem [bài viết](https://loj.ac/article/416) để biết地址.

LibreOJ có đề BalticOI gần 10 năm.

### BalkanOI

**BalkanOI** dành cho khu vực Balkan. BalkanOI 2018 có 12 nước (Romania, Greece, Bulgaria, Serbia, v.v.). Đề khó.

BalkanOI chỉ một số năm công khai đề/数据/题解; xem [bài viết](https://loj.ac/article/416).

### CEOI

CEOI 2018 có nhiều nước trùng với hai giải trên (Poland, Romania, Georgia, Croatia...). Đề khó.

CEOI công khai đề/数据/题解 hằng năm; xem [bài viết](https://loj.ac/article/416).

### eJOI

**eJOI** (European Junior Olympiad in Informatics). Quốc gia tham gia gồm Nga, Armenia, Bulgaria, Poland, v.v. Đề khá khó.

eJOI công khai đề/数据/题解 hằng năm; xem [bài viết](https://loj.ac/article/416).

### NOI

???+ warning "Warning"
    Đây không phải “全国信息学奥林匹克竞赛”.

**NOI** viết đầy đủ Nordic Olympiads in Informatics.

Website: <http://nordic.progolymp.se>

Giải mới tổ chức vài năm gần đây, dành cho các nước Bắc Âu.

## Tài liệu tham khảo

-   [ICPC/CCPC 赛事与赛制](./icpc.md)
-   [「翻译组」一些大洲级 OI 比赛的地址](https://loj.ac/article/416)
