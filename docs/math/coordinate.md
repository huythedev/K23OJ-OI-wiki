author: Ir1d, HeRaNO, Chrogeek, abc1763613206, mxdyzmx

## Định nghĩa góc

Ở tiểu học hoặc trung học cơ sở đã học định nghĩa **tĩnh** của góc: hình tạo bởi hai tia có chung đỉnh gọi là góc．

Nhưng định nghĩa này giới hạn góc trong $[0, 360^\circ]$, gây khó khăn khi nghiên cứu sâu hơn và không giải thích được các trường hợp như: quay $720^\circ$ nghĩa là gì？

Ở trung học phổ thông, ta học định nghĩa **động** của góc: một tia trong mặt phẳng quay quanh đầu mút từ vị trí này đến vị trí khác tạo thành hình gọi là góc．

Vị trí bắt đầu gọi là **cạnh đầu**, vị trí kết thúc gọi là **cạnh cuối**．Quy ước:

-   Quay theo **ngược chiều kim đồng hồ** tạo góc **dương**, có số đo dương；
-   Quay theo **thuận chiều kim đồng hồ** tạo góc **âm**, có số đo âm；
-   Cạnh cuối trùng với cạnh đầu, không quay, gọi là **góc không**, số đo $0^\circ$．

Như vậy khái niệm góc được mở rộng thành **góc bất kỳ**．

???+ note "注意"
    Góc không có cạnh đầu và cạnh cuối trùng nhau, nhưng không phải mọi góc có cạnh đầu trùng cạnh cuối đều là góc không, ví dụ các góc là bội của $360^\circ$．

## Hệ radian

Trong ứng dụng, thường cần chuyển đổi giữa góc và các tham số khác, dùng radian giúp giảm hệ số．Tiếp theo giới thiệu **hệ radian**：

Cung có độ dài bằng bán kính chắn tại tâm một góc gọi là góc $1$ radian, ký hiệu $\text{rad}$, đọc là radian．

Theo quy ước trên, góc dương có radian dương, góc âm có radian âm, góc không là $0$，nếu bán kính $r$ của đường tròn có góc ở tâm $\alpha$ chắn cung dài $l$，thì：

$$
|\alpha|=\dfrac{l}{r}
$$

Từ đó có thể viết công thức độ dài cung và diện tích quạt (lược)．

Do $360^\circ$ tương ứng $2\pi$ rad, ta có chuyển đổi:

$$
k \operatorname{rad} = \frac{\pi}{180^\circ} n^\circ
$$

Xét một góc, nếu quay cạnh cuối thêm một vòng hoặc nhiều vòng, cạnh đầu cố định thì cạnh cuối vẫn trùng vị trí ban đầu, gọi là các góc có cạnh cuối trùng nhau．

Tập các góc cùng cạnh cuối với $\alpha$ là $\{\varphi \mid \varphi = \alpha + 2k\pi, k \in \mathbf{Z}\}$．

Có thể hiểu là: mỗi lần quay thêm một vòng, cạnh cuối không đổi．

???+ note "$\pi$ và $\tau$"
    Hiện có quan điểm cho rằng “hằng số tròn thực sự” là $2\pi$, ký hiệu $\tau$．Những người ủng hộ chọn 28/6 làm ngày $\tau$．
    
    Ví dụ, trong hệ radian, một vòng là $2\pi$, chia đều $2\pi$ cho phép chia đều vòng tròn．Trong giải tích phức cũng hay xuất hiện $2\pi$，v.v．
    
    Để phù hợp thói quen ở các vùng tại Trung Quốc, tại **OI Wiki** dùng $\pi$ cho hằng số tròn．

???+ note "Cách viết $\pi$ trong lập trình"
    Trong C/C++, thường lấy $\pi$ là `acos(-1)` vì đây là giá trị gần nhất của $\pi$ ở dạng floating．`acos(-1)` hoặc `4 * atan(1)` cho $3.14159265358979310000$．
    
    Dùng giá trị khác như `acos(-1.0/2.0)`，`acos(1.0/2.0)`，`asin(1.0/2.0)`，v.v. thì cho $3.14159265358979360000$，không gần bằng．
    
    Nếu nhớ được, có thể viết trực tiếp $3.1415926535897932$．

## Hệ tọa độ Descartes phẳng

Trong cùng một mặt phẳng, hai trục số vuông góc và có chung gốc tạo thành hệ tọa độ Descartes．

Thông thường, một trục đặt ngang, một trục đặt dọc, chọn chiều sang phải và lên trên là chiều dương．Trục ngang gọi là trục $x$ (x-axis), trục dọc gọi là trục $y$ (y-axis), gọi chung là trục tọa độ; gốc chung $O$ gọi là gốc tọa độ (origin), hệ tọa độ với gốc $O$ ký hiệu $xOy$．

Trục $x$ và $y$ chia mặt phẳng thành bốn góc phần tư (quadrant): phía phải trên là góc phần tư I, các góc còn lại theo ngược chiều kim đồng hồ là II, III, IV．Các điểm trên trục và gốc không thuộc góc phần tư nào．Thông thường hai trục dùng cùng đơn vị, nhưng có thể khác trong trường hợp đặc biệt．

### Mô tả vị trí trong hệ tọa độ Descartes phẳng

Trong hệ tọa độ Descartes phẳng, mỗi điểm có duy nhất một cặp số có thứ tự (tọa độ) tương ứng; ngược lại, mỗi cặp số có thứ tự xác định duy nhất một điểm．

Với điểm $C$, kẻ vuông góc xuống trục $x$ và $y$, chân vuông góc lần lượt là $a,b$ thì $a$ gọi là hoành độ, $b$ là tung độ; cặp $(a,b)$ gọi là tọa độ trực giao của $C$．Một điểm ở các góc phần tư khác nhau hoặc trên trục có tọa độ khác nhau．

## Hệ tọa độ cực phẳng

Xét tình huống thực tế, ví dụ hàng hải: nói “điểm $B$ nằm về phía đông bắc $30^\circ$ so với $A$, cách $100$ mét” hơn là “lập hệ tọa độ Descartes tại $A$, $B(50,50\sqrt 3)$”．

Thiết lập hệ tọa độ cực:

1.  Chọn một điểm $O$ gọi là **cực**；
2.  Từ cực vẽ tia $Ox$ gọi là **trục cực**；
3.  Chọn đơn vị độ dài (thường là $1$), đơn vị góc (thường là radian) và chiều dương (thường ngược chiều kim đồng hồ)；

Thế là có **hệ tọa độ cực**．

### Mô tả vị trí trong hệ tọa độ cực

Cho điểm $A$ trên mặt phẳng．

-   Khoảng cách $|OA|$ gọi là **bán kính cực**, ký hiệu $\rho$；
-   Góc $\angle xOA$ (lấy trục cực làm cạnh đầu, $OA$ làm cạnh cuối) gọi là **góc cực**, ký hiệu $\varphi$；

Cặp $(\rho,\varphi)$ là **tọa độ cực** của $A$．

Do góc cùng cạnh cuối, $(\rho,\varphi)$ và $(\rho,\varphi + 2k\pi)\ (k\in \mathbf{Z})$ biểu diễn cùng một điểm．Đặc biệt, cực có tọa độ $(0,\varphi)\ (\varphi \in \mathbf{R})$，vì vậy một điểm có thể có vô số biểu diễn cực．

Nếu quy ước $\rho \ge 0,0 \le \varphi < 2\pi$，thì mọi điểm (trừ cực) có biểu diễn cực duy nhất, và biểu diễn cực là duy nhất．

### Chuyển đổi giữa Descartes phẳng và cực

Đôi khi nghiên cứu hình trong cực không tiện, cần đổi sang Descartes．Với $A(\rho,\varphi)$, tọa độ Descartes $(x,y)$:

$$
\begin{aligned}
x &= \rho \cos \varphi \\
y &= \rho \sin \varphi
\end{aligned}
$$

Suy ra:

$$
\begin{aligned}
\rho^2 &= x^2 + y^2\\
\tan \varphi &= \frac{y}{x}\ \ \ \ (x\not =0)
\end{aligned}
$$

Vì $\tan\varphi$ có hai góc, cần xác định góc dựa trên $x,y$．Định nghĩa:

$$
\operatorname{atan2}(y, x) = \begin{cases}
\arctan(\frac{y}{x}) & \text{if } x > 0 \\
\arctan(\frac{y}{x}) + \pi & \text{if } y \ge 0, x < 0 \\
\arctan(\frac{y}{x}) - \pi & \text{if } y < 0, x < 0 \\
\pi/2 & \text{if } y > 0, x = 0 \\
-\pi/2 & \text{if } y < 0, x = 0 \\
\text{any} & \text{if } y = 0, x = 0
\end{cases}
$$

khi đó $\varphi = \operatorname{atan2}(y, x)$．Chú ý giá trị nằm trong $(-\pi, \pi]$．

Trong C/C++ `<math.h>` hoặc `<cmath>` có [hàm này](https://zh.cppreference.com/w/cpp/numeric/math/atan2), gọi `atan2(y, x)`．

## Hệ tọa độ Descartes không gian

Thiết lập như sau:

1.  Chọn điểm $O$ trong không gian；
2.  Qua $O$ vẽ ba trục vuông góc $\overrightarrow{Ox}, \overrightarrow{Oy}, \overrightarrow{Oz}$, lần lượt gọi là trục $x$ (ngang), $y$ (dọc), $z$ (đứng), gọi chung là trục tọa độ; chiều dương theo quy tắc bàn tay phải: nắm tay phải theo trục $z$, bốn ngón từ chiều dương trục $x$ xoay đến chiều dương trục $y$ thì ngón cái chỉ chiều dương trục $z$；
3.  Chọn đơn vị độ dài, thường là $1$．

Từ đó có hệ tọa độ không gian $O-xyz$．Điểm $O$ là gốc．

Bất kỳ hai trục xác định một mặt phẳng, tạo ba mặt phẳng vuông góc gọi là các mặt tọa độ: $xOy$, $yOz$, $zOx$．Ba mặt phẳng chia không gian thành tám phần gọi là các “octant”．

### Mô tả vị trí trong hệ tọa độ Descartes không gian

Chọn hệ $O-xyz$, ta có quan hệ một-một giữa điểm và bộ ba số．

Cho điểm $M$, qua $M$ dựng các mặt phẳng vuông góc trục $x,y,z$ cắt các trục tại $P,Q,R$; $P,Q,R$ là hình chiếu của $M$ lên các trục．Gọi tọa độ của $P,Q,R$ lần lượt là $x,y,z$ thì điểm $M$ xác định bộ ba $(x,y,z)$．

Ngược lại, với $(x,y,z)$, lấy điểm $P,Q,R$ trên các trục tương ứng, dựng các mặt phẳng vuông góc, chúng giao nhau tại điểm $M$．

Như vậy thiết lập quan hệ một-một giữa điểm $M$ và bộ ba $(x,y,z)$, gọi là tọa độ của $M$, ký hiệu $M(x,y,z)$; $x$ là hoành độ, $y$ là tung độ, $z$ là cao độ．

## Hệ tọa độ trụ

Hệ trụ là mở rộng của cực lên 3D: từ hệ cực trong mặt phẳng, thêm trục $z$ vuông góc qua cực, hướng lên．

Để xác định điểm bởi $(\rho, \varphi, z)$, trước hết dùng $\rho,\varphi$ trong mặt phẳng cực, sau đó dịch theo $z$ “lên” hoặc “xuống”．

### Chuyển đổi giữa hệ trụ và Descartes không gian

Giá trị $z$ là như nhau．

Chuyển đổi $(x,y)$ và $(\rho, \varphi)$ xem ở [Chuyển đổi giữa Descartes phẳng và cực](#平面直角坐标系与极坐标系的相互转换)．

## Hệ tọa độ cầu

Tọa độ cầu xác định như sau:

1.  Đứng tại gốc, hướng về trục cực ngang; trục thẳng đứng hướng từ chân lên đầu；
2.  Giơ tay chỉ theo trục cực đứng；
3.  Quay ngược chiều kim đồng hồ góc $\varphi$；
4.  Hạ tay xuống góc $\vartheta$，tay chỉ theo hướng xác định bởi $\varphi$ và $\vartheta$；
5.  Tịnh tiến theo hướng đó một khoảng $r$．

Ta đến điểm có tọa độ cầu $(r,\vartheta,\varphi)$, trong đó $\vartheta$ là **góc thiên đỉnh**, $\varphi$ là **góc phương vị**．

???+ warning "Warning"
    Do nhiều lý do, đôi khi dùng $\phi$ cho góc thiên đỉnh và $\theta$ cho góc phương vị．Khi đọc tài liệu gặp tọa độ cầu cần chú ý．
    
    Khi viết bài có dùng tọa độ cầu, nên nêu rõ ký hiệu dùng cho thiên đỉnh và phương vị．

### Chuyển đổi giữa hệ trụ và hệ cầu

Giá trị $\varphi$ giống nhau ở hai hệ．

Từ trụ sang cầu:

$$
\begin{aligned}
r &= \sqrt{\rho^2 + z^2} \\
\vartheta &= \begin{cases}
\arctan\left(\frac{\rho}{z}\right) & \text{if }z > 0 \\
\pi/2 & \text{if }z = 0, \rho \not= 0 \\
\arctan\left(\frac{\rho}{z}\right) + \pi & \text{if }z < 0 \\
\end{cases}
\end{aligned}
$$

Lưu ý với điểm $(0,0,0)$ trong hệ trụ, $\vartheta$ trong hệ cầu không xác định．

Từ cầu sang trụ:

$$
\begin{aligned}
\rho &= r \sin \vartheta \\
z &= r \cos \vartheta
\end{aligned}
$$

### Chuyển đổi giữa Descartes không gian và hệ cầu

Có thể kết hợp [Chuyển đổi giữa Descartes phẳng và cực](#平面直角坐标系与极坐标系的相互转换) và [Chuyển đổi giữa hệ trụ và hệ cầu](#柱坐标系与球坐标系的相互转换), hoặc dùng trực tiếp:

Từ Descartes không gian sang cầu:

$$
\begin{aligned}
r &= \sqrt{x^2 + y^2 + z^2} \\
\vartheta &= \arccos\left(\frac{z}{\sqrt{x^2 + y^2 + z^2}}\right) \\
\varphi &= \operatorname{atan2}(y, x)
\end{aligned}
$$

Trong đó $\operatorname{atan2}$ xem ở [Chuyển đổi giữa Descartes phẳng và cực](#平面直角坐标系与极坐标系的相互转换)．

Lưu ý với điểm $(0,0,0)$, $\vartheta$ và $\varphi$ không xác định．

Từ cầu sang Descartes không gian:

$$
\begin{aligned}
x &= r \sin \vartheta \cos \varphi \\
y &= r \sin \vartheta \sin \varphi \\
z &= r \cos \vartheta
\end{aligned}
$$
