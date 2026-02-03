Trong máy tính, ngoài nhị phân, thường dùng bát phân và thập lục phân.

## Nhị phân

Nhị phân là hệ cơ số dùng trong tính toán nội bộ của máy tính. Trong hệ này chỉ có $0,1$, và mọi phép toán nội bộ (bao gồm phép toán bit) đều dựa trên nhị phân.

Tuy nhiên, biểu diễn nhị phân thường dài, nên để thuận tiện, người ta thường chuyển sang bát phân hoặc thập lục phân.

## Bát phân

Trong bát phân, có các chữ số $0,1,2,3,4,5,6,7$.

Thông thường, số bát phân được viết dưới dạng `oxx` (trong đó `o` là tiền tố bát phân, `xx` là số bát phân).

## Thập lục phân

Trong thập lục phân có các chữ số $0,1,2,3,4,5,6,7,8,9,A(10),B(11),C(12),D(13),E(14),F(15)$.

Ưu điểm lớn nhất của thập lục phân so với nhị phân là độ dài biểu diễn ngắn hơn: một chữ số thập lục phân tương ứng 4 bit nhị phân.

Thông thường, số thập lục phân được viết dưới dạng `0xdbf` (trong đó `0x` là tiền tố).

## Chuyển đổi giữa các hệ

### Thập phân sang nhị phân/bát phân/thập lục phân

Lấy nhị phân làm ví dụ, các hệ khác tương tự.

Phần nguyên: liên tục chia số thập phân cho 2 đến khi thương bằng 0, rồi lấy các số dư từ dưới lên. Phần thập phân: nhân với 2, lấy phần nguyên, lặp lại với phần thập phân còn lại cho đến khi bằng 0, rồi lấy các phần nguyên từ trên xuống.

```text
Chuyển 35.25 sang nhị phân
Phần nguyên:
35/2=17  ......1
17/2=8   ......1
8/2=4    ......0
4/2=2    ......0
2/2=1    ......0
1/2=0    ......1
Phần thập phân:
0.25*2=0.5     0
0.5*2=1        1
```

Tức $35.25 = (100011.01)_2$

### Nhị phân/bát phân/thập lục phân sang thập phân

Vẫn lấy nhị phân làm ví dụ.

Chuyển nhị phân sang thập phân bằng cách nhân từng bit với $2^i$, trong đó $i$ là vị trí (bit thấp nhất có $i=0$).

```text
Chuyển 11010.01(2) sang thập phân
11010.01(2)=1*2^4+1*2^3+0*2^2+1*2^1+0*2^0+0*2^(-1)+1*2(-2)
        =26.25
```

Tức $(11010.01)_2 = (26.25)_{10}$

### Chuyển đổi giữa nhị phân/bát phân/thập lục phân

Một chữ số bát phân tương ứng 3 bit nhị phân (vì $2^3 =8$), một chữ số thập lục phân tương ứng 4 bit nhị phân ($2^4 = 16$), và ngược lại.
