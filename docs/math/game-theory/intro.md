author: cutekibry, woruo27, Backl1ght, c-forrest

**Lý thuyết trò chơi** (game theory) là một nhánh của kinh tế học, nghiên cứu hành vi của các cá nhân có tính cạnh tranh/đối kháng dưới các luật lệ nhất định, và nghiên cứu chiến lược tối ưu của họ．

Nói đơn giản, lý thuyết trò chơi nghiên cứu: trong một trò chơi, các người chơi chọn chiến lược như thế nào．

## Khái niệm cơ bản

Phần này giới thiệu ngắn gọn một số khái niệm thường gặp．

### Trò chơi hợp tác/phi hợp tác

**Trò chơi hợp tác** (cooperative game) là trò chơi mà người tham gia có thể lập liên minh, hợp tác với nhau．Trong loại này, hành vi không hợp tác thường bị một cơ chế bên ngoài trừng phạt．Ngược lại, **trò chơi phi hợp tác** (noncooperative game) không có cơ chế như vậy, nên người tham gia либо không thể liên minh, либо chỉ có thể dựa vào đe dọa đáng tin để duy trì hợp tác．

So với trò chơi hợp tác, trò chơi phi hợp tác được nghiên cứu hệ thống và mature hơn．Bài viết này chỉ thảo luận trò chơi phi hợp tác．

### Trò chơi đối xứng/phi đối xứng

Trong **trò chơi đối xứng** (symmetric game), các người chơi nhận được lợi ích như nhau khi thực hiện cùng một hành động, tức lợi ích chỉ phụ thuộc vào hành động, không phụ thuộc vào danh tính người chơi．Không thỏa điều kiện này gọi là **trò chơi phi đối xứng** (asymmetric game)．

### Trò chơi tổng bằng không/khác không

Trang chính: [Trò chơi tổng bằng không](./zero-sum-game.md)

**Trò chơi tổng bằng không** (zero-sum game) là trò chơi mà tổng lợi ích của mọi người tham gia luôn bằng 0．Thường xét hai người, khi đó lợi ích của một bên là tổn thất của bên kia．Ngược lại, **trò chơi tổng khác không** (non-zero-sum game) cho phép cả hai cùng thắng hoặc cùng thua, gồm **trò chơi tổng dương** (positive-sum game) và **trò chơi tổng âm** (negative-sum game), v.v．

### Trò chơi đồng thời/tuần tự

Trong **trò chơi đồng thời** (simulatenous game), tất cả người chơi ra quyết định đồng thời khi không biết lựa chọn của người khác．Ví dụ kéo-búa-bao là trò chơi đồng thời điển hình．Loại này thường dùng ma trận lợi ích và không nhấn mạnh thời gian．

Ngược lại là **trò chơi tuần tự** (sequential game), nơi người chơi hành động lần lượt．Cần lưu ý: người đi sau phải quan sát được ít nhất một phần hành vi của người đi trước,否则 thứ tự vô nghĩa．Trò chơi tuần tự thường mô tả bằng cây trò chơi．

### Trò chơi thông tin hoàn hảo/không hoàn hảo

**Thông tin hoàn hảo** (perfect information) nghĩa là tại mọi thời điểm ra quyết định, người chơi biết đầy đủ mọi sự kiện đã xảy ra, bao gồm trạng thái ban đầu．Cờ vua, cờ vây là thông tin hoàn hảo; mạt chược, poker là không hoàn hảo vì không biết bài đối thủ．Thông tin hoàn hảo thường dùng cho trò chơi tuần tự; vì trong trò chơi đồng thời người chơi không biết hành động sắp tới của đối thủ, nên thường xem trò chơi đồng thời là không hoàn hảo．

### Trò chơi thông tin đầy đủ/không đầy đủ

**Thông tin đầy đủ** (complete information) nghĩa là mọi người chơi biết đầy đủ cấu trúc trò chơi (tập hành động và lợi ích), và các thông tin này là kiến thức chung (common knowledge)．Ngược lại là thông tin không đầy đủ, khi một số yếu tố (như hành động khả dĩ hoặc hàm lợi ích của đối thủ) là unknown．

Đáng chú ý, “thông tin đầy đủ” và “thông tin hoàn hảo” là hai khái niệm độc lập．Ví dụ, mạt chược là trò chơi thông tin đầy đủ nhưng không hoàn hảo vì luật và lợi ích công khai nhưng bài không minh bạch; một số trò chơi mục tiêu ẩn nhưng hành động công khai là hoàn hảo nhưng không đầy đủ．

## Lý thuyết trò chơi tổ hợp

Trong thi đấu thuật toán, loại trò chơi thường gặp nhất là **trò chơi tổ hợp** (combinatorial game)．Thuật ngữ này thường chỉ những trò chơi có không gian trạng thái lớn nên khó giải．Vì trò chơi tổ hợp tổng quát rất phức tạp, lý thuyết trò chơi tổ hợp chủ yếu quan tâm đến các trò chơi: hai người luân phiên, thông tin hoàn hảo, không ngẫu nhiên．Cờ vua, cờ vây là ví dụ điển hình．

### Trò chơi tổ hợp công bằng

Trang chính: [Trò chơi tổ hợp công bằng](./impartial-game.md)

**Trò chơi công bằng** (impartial game) là trò chơi tổ hợp thỏa:

-   Ở mọi trạng thái, tập hành động của các người chơi hoàn toàn giống nhau, chỉ phụ thuộc vào trạng thái, không phụ thuộc danh tính；
-   Một trạng thái không thể lặp lại, trò chơi kết thúc khi người chơi không thể hành động, và luôn kết thúc sau hữu hạn bước với kết quả không hòa．

Trò chơi công bằng luôn là trò chơi đối xứng．

### Trò chơi tổ hợp không công bằng

Trang chính: [Trò chơi tổ hợp không công bằng](./partizan-game.md)

Khái niệm đối lập là **trò chơi không công bằng** (partizan game), nơi hành động hợp lệ phụ thuộc vào danh tính người chơi．Hầu hết trò chơi cờ (cờ vua, cờ tướng, cờ vây, gomoku, v.v.) đều là không công bằng vì người chơi chỉ điều khiển quân của mình．

### Trò chơi chuẩn/phản thường

Trong trò chơi tổ hợp, người thắng thường là người thực hiện bước đi cuối cùng．Đó là **trò chơi chuẩn** (normal game)．Ngược lại là **trò chơi phản thường** (misère game), nơi người thực hiện bước đi cuối cùng là người thua．

Cả trò chơi công bằng và không công bằng đều có thể là chuẩn hoặc phản thường．

## Tài liệu tham khảo

-   [Game theory - Wikipedia](https://en.wikipedia.org/wiki/Game_theory)
-   [Combinatorial game theory - Wikipedia](https://en.wikipedia.org/wiki/Combinatorial_game_theory)
-   [Impartial game - Wikipedia](https://en.wikipedia.org/wiki/Impartial_game)
-   [Misère - Wikipedia](https://en.wikipedia.org/wiki/Mis%C3%A8re)
