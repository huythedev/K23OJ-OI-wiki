Trước khi bắt đầu bài viết, **Ban dự án OI Wiki** xin gửi lời chào mừng nồng nhiệt tới bạn vì đã đóng góp cho dự án này. Chính nhờ hàng trăm người như bạn mà **OI Wiki** mới có được như ngày hôm nay!

Bài viết này sẽ hướng dẫn chi tiết quy trình tham gia biên soạn nội dung cho **OI Wiki**. Trước khi viết mới hoặc chỉnh sửa bất kỳ trang nào, hãy đọc kỹ các hướng dẫn dưới đây để giúp bạn hoàn thiện nội dung chất lượng cao hơn.

## Hướng dẫn đóng góp

Trước khi chỉnh sửa, hãy xem qua [Hướng dẫn đóng góp cho OI Wiki](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/.github/CONTRIBUTING.md) và [Phương châm dự án](./about.md#项目方针) để hợp tác, trao đổi hiệu quả hơn với cộng đồng.

## Tham gia cộng tác

???+ warning "Cảnh báo"
    Trước khi bắt đầu viết nội dung mới, hãy kiểm tra [Issues](https://github.com/huythedev/K23OJ-OI-wiki/issues) để đảm bảo chưa có ai làm việc trùng lặp, sau đó tạo một [issue mới](https://github.com/huythedev/K23OJ-OI-wiki/issues/new) để ghi nhận nội dung bạn sẽ thực hiện.

???+ tip "Mẹo"
    Trong Issues cũng có rất nhiều vấn đề cần sửa/chưa giải quyết, đặc biệt là các kế hoạch lặp (Iteration Plan). Lấy nhiệm vụ từ đây là một khởi đầu rất tốt!

Để đảm bảo tính chuyên môn và chính xác của nội dung, bạn nên cân nhắc các điểm sau trước khi chỉnh sửa:

1.  **Chọn lĩnh vực bạn am hiểu:** Ưu tiên chỉnh sửa các chủ đề phù hợp với kiến thức, nền tảng học tập hoặc sở thích của bạn. Điều này giúp bạn tạo ra nội dung chất lượng cao.
2.  **Cẩn trọng với lĩnh vực mới:** Nếu bạn chưa quen hoặc chưa hiểu sâu về chủ đề nào đó, hãy đọc và tìm hiểu kỹ trước khi chỉnh sửa.
3.  **Tham khảo tài liệu liên quan:** Khi thêm hoặc sửa nội dung, nên tra cứu các tài liệu, nguồn uy tín để đảm bảo thông tin chính xác. Bạn cũng có thể đặt câu hỏi ở phần bình luận hoặc cộng đồng để trao đổi với các biên tập viên khác.

Chúng tôi trân trọng sự nhiệt huyết và đóng góp của mỗi thành viên, đồng thời hiểu rằng trình độ chuyên môn của mọi người là khác nhau. Hãy cùng nhau xây dựng một kho tri thức chính xác, chuyên nghiệp để giúp đỡ nhiều bạn đọc hơn. Xin mượn lời của Wikipedia:

> Đừng ngại chỉnh sửa, hãy mạnh dạn cập nhật trang![^ref1]

### Chỉnh sửa trên GitHub

Để tham gia biên soạn **OI Wiki**, bạn **cần** có tài khoản GitHub ([Đăng ký tại đây](https://github.com/signup)), nhưng **không cần** kỹ năng GitHub nâng cao. Dù bạn là người mới, chỉ cần làm theo các bước dưới đây là có thể **hoàn thành chỉnh sửa xuất sắc**.

???+ tip "Mẹo"
    Trước khi thay đổi của bạn được hợp nhất vào kho chính của **OI Wiki**, các chỉnh sửa sẽ **không xuất hiện** trên trang chủ, nên bạn không cần lo lắng việc làm hỏng nội dung đang hiển thị.
    
    Nếu vẫn chưa yên tâm, hãy xem [hướng dẫn chính thức của GitHub](https://skills.github.com/).

#### Chỉnh sửa nội dung một trang

1.  Tìm trang cần chỉnh sửa trên **OI Wiki**;
2.  Nhấn nút **"Chỉnh sửa trang này"** (biểu tượng <i class="md-icon">edit</i> ở góc trên bên phải, cạnh mục lục), xác nhận bạn đã đọc trang này và [Sổ tay định dạng](./format.md), sau đó làm theo hướng dẫn để chuyển sang GitHub chỉnh sửa;
3.  Soạn thảo nội dung bạn muốn thay đổi trong khung chỉnh sửa. **Lưu ý:** Trong quá trình chỉnh sửa và gửi thay đổi, hãy **tắt phần mềm dịch tự động** để tránh các lỗi không mong muốn (ví dụ: đổi tên file sai làm ảnh hưởng cấu trúc thư mục);
4.  Sau khi chỉnh sửa, kéo xuống cuối trang, điền thông tin commit theo [quy tắc định dạng commit](#commit-信息格式规范) bên dưới, rồi nhấn **Propose changes** để gửi thay đổi. GitHub sẽ tự động tạo một nhánh (branch) mới cho bạn và thêm commit vào đó;
5.  GitHub sẽ chuyển sang trang nhánh của bạn, ở trên cùng sẽ có nút xanh **Create pull request**, nhấn vào để chuyển sang trang tạo Pull Request. Kiểm tra lại thay đổi, điền thông tin Pull Request theo [quy tắc định dạng Pull Request](#pull-request-信息格式规范), rồi nhấn **Create pull request**;
6.  Nếu không có gì bất thường, Pull Request của bạn đã được gửi, chờ quản trị viên duyệt và hợp nhất vào kho chính.

Trong thời gian chờ duyệt, bạn có thể bình luận, thích hoặc không thích Pull Request của người khác. Nếu có thông báo mới, sẽ hiện ở góc trên bên phải và gửi email (tùy cài đặt thông báo cá nhân).

#### Chỉnh sửa nhiều trang không liên quan

Nếu bạn cần chỉnh sửa nhiều trang không liên quan, hãy làm theo hướng dẫn ở [Chỉnh sửa nội dung một trang](#编辑单个页面内的内容) để chỉnh sửa từng trang một.

1.  Mở kho [K23OJ-OI-wiki](https://github.com/huythedev/K23OJ-OI-wiki), nhấn phím <kbd>.</kbd> (hoặc đổi `github.com` thành `github.dev` trong URL)[^ref2] để vào trình soạn thảo VS Code trên web của GitHub;
2.  Chỉnh sửa các file nguồn trong trình soạn thảo, có thể dùng nút xem trước (hoặc <kbd>Ctrl+K</kbd><kbd>V</kbd>) để xem kết quả;
3.  Sau khi chỉnh sửa, dùng tab Source Control bên trái, điền thông tin commit theo [quy tắc định dạng commit](#commit-信息格式规范), rồi gửi commit. Khi được hỏi có tạo nhánh mới không, nhấn **Fork Repository** màu xanh;
4.  Sau khi gửi, sẽ có thông báo ở giữa màn hình, nhập tiêu đề ở hộp thoại đầu tiên, nhập tên nhánh ở hộp thứ hai, sau đó sẽ có thông báo ở góc dưới bên phải, ví dụ `Created Pull Request #1 for OI-Wiki/OI-Wiki.`, nhấn vào liên kết xanh để xem Pull Request.

#### Thêm thay đổi vào Pull Request

1.  Mở [danh sách Pull Request của K23OJ-OI-wiki](https://github.com/huythedev/K23OJ-OI-wiki/pulls), tìm Pull Request bạn đã gửi và nhấn vào;
2.  Dưới tiêu đề Pull Request sẽ có dòng như `<ID của bạn> wants to merge x commits into OI-wiki:master from <ID của bạn>:patch-1`, nhấn vào phần `<ID của bạn>:patch-1`;
3.  Bạn sẽ được chuyển đến kho nhánh của mình, tên nhánh ở góc trên bên trái là tên nhánh đã gửi Pull Request (ví dụ `patch-1`);
4.  Tiến hành chỉnh sửa tiếp:
    -   Nếu chỉ sửa một file hoặc các trang không liên quan, tìm file cần sửa, chỉnh sửa rồi kéo xuống dưới, điền thông tin commit theo [quy tắc định dạng commit](#commit-信息格式规范), nhấn **Commit changes** để gửi;
    -   Nếu sửa nhiều file, nhấn <kbd>.</kbd> (hoặc đổi `github.com` thành `github.dev`)[^ref2] để vào VS Code web, chỉnh sửa rồi dùng tab Source Control bên trái, điền thông tin commit và gửi;
5.  Thay đổi của bạn sẽ tự động được thêm vào Pull Request.

### Chỉnh sửa trên máy bằng Git

???+ warning "Cảnh báo"
    Đối với người dùng phổ thông, chúng tôi khuyến khích sử dụng trình soạn thảo web của GitHub như hướng dẫn trên.

Dù đa số trường hợp bạn có thể chỉnh sửa trực tiếp trên GitHub, nhưng với một số trường hợp đặc biệt (ví dụ cần ký GPG), bạn nên dùng Git trên máy.

Quy trình cơ bản:

1.  Fork kho chính về tài khoản của bạn;
2.  Clone nhánh đã fork về máy;
3.  Chỉnh sửa, commit các thay đổi trên máy;
4.  Push các thay đổi lên kho nhánh của bạn;
5.  Gửi Pull Request về kho chính.

Xem hướng dẫn chi tiết tại trang [Git](../tools/git.md).

#### Thêm thay đổi vào Pull Request

Tiếp tục chỉnh sửa trên nhánh đã clone về máy, commit và push các thay đổi. Pull Request sẽ tự động cập nhật.

### Xem trước thay đổi trên trang web

Ở cuối trang Pull Request sẽ có mục kiểm thử, nhấn vào liên kết Details của dòng netlify/oi-wiki/deploy-preview (như hình dưới) để xem bản dựng tự động của trang đã chỉnh sửa.

![deploy\_preview](./images/deploy_preview.png)

### Thay đổi về mục lục và liên kết

Thông thường, nếu bạn thêm trang mới hoặc sửa liên kết trong mục lục, bạn cần chỉnh sửa file [`mkdocs.yml`](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/mkdocs.yml).

Tham khảo các mục đã có để thêm trang mới. Trừ khi tái cấu trúc hoặc sửa thuật ngữ, **không nên thay đổi liên kết các trang hiện có**; các Pull Request thay đổi không cần thiết sẽ bị từ chối.

Nếu thực sự cần sửa liên kết, hãy cập nhật trường author và file chuyển hướng.

### Trường author

Do GitHub API không thể theo dõi lịch sử khi thay đổi thư mục, chúng tôi duy trì thủ công danh sách tác giả ở đầu file Markdown, ví dụ: `author: Ir1d, cjsoft`, các ID cách nhau bởi dấu phẩy và cách. ID là tên người dùng GitHub (ví dụ <https://github.com/Ir1d> thì là `Ir1d`).

Khi sửa liên kết, hãy điền đầy đủ contributors vào trường author.

### File chuyển hướng

Khi sửa liên kết để tránh lỗi liên kết ngoài, cần cập nhật file chuyển hướng.

File [`_redirects`](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/docs/_redirects) dùng để tạo [cấu hình netlify](https://docs.netlify.com/routing/redirects/#syntax-for-the-redirects-file) và [file chuyển hướng](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/scripts/gen_redirect.py).

Mỗi dòng là một quy tắc chuyển hướng, gồm đường dẫn nguồn và đích (không có tên miền):

```text
/path/to/src /path/to/desc
```

Lưu ý: Tất cả chuyển hướng đều là 301, chỉ cần sửa khi thay đổi url gây lỗi liên kết.

### Quy tắc định dạng thông tin Commit

Khi gửi commit, hãy tuân thủ các yêu cầu sau:

1.  Tiêu đề commit mô tả ngắn gọn thay đổi, không quá 50 ký tự, phần vượt quá sẽ chuyển xuống nội dung chi tiết.
2.  Nếu cần, hãy mô tả chi tiết hơn ở phần nội dung commit.

Khuyến nghị định dạng commit như sau:

```text
<loại sửa đổi>(<tên file>): <nội dung thay đổi>
```

Các loại sửa đổi:

-   `feat`: Thêm nội dung mới.
-   `fix`: Sửa lỗi nội dung hiện có.
-   `refactor`: Tái cấu trúc (thay đổi lớn).
-   `revert`: Hoàn tác thay đổi trước đó.

### Quy tắc định dạng thông tin Pull Request

Khi gửi Pull Request, hãy tuân thủ các yêu cầu sau:

1.  Tiêu đề nêu rõ mục đích PR (đã làm gì, sửa lỗi gì).
2.  Nội dung mô tả ngắn gọn thay đổi. Nếu sửa lỗi issue, thêm `fix #xxxx` với xxxx là số issue.
3.  Đọc kỹ [Hướng dẫn đóng góp](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/.github/CONTRIBUTING.md) và [Quy ước cộng đồng](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/CODE_OF_CONDUCT.md), đồng ý bằng cách tick vào ô xác nhận trong mẫu PR.

Khuyến nghị định dạng tiêu đề Pull Request:

```plain
<loại sửa đổi>(<tên file>): <nội dung thay đổi> (<số issue>)
```

Các loại sửa đổi:

-   `feat`: Thêm nội dung mới.
-   `fix`: Sửa lỗi nội dung hiện có.
-   `refactor`: Tái cấu trúc (thay đổi lớn).
-   `revert`: Hoàn tác thay đổi trước đó.

Ví dụ:

-   `fix(ds/persistent-seg): sửa chú thích code cho rõ ràng`
-   `fix: tools/judger/index không có trong mục lục (#3709)`
-   `feat(math/poly/fft): chứng minh tốt hơn`
-   `refactor(ds/stack): sắp xếp lại nội dung`

### Quy trình cộng tác

1.  Khi có Pull Request mới, reviewer sẽ nhận được email;
2.  Đồng thời, [GitHub Actions](https://github.com/huythedev/K23OJ-OI-wiki/actions) và [Netlify](https://app.netlify.com/sites/oi-wiki) sẽ chạy kiểm thử, tiến trình sẽ hiển thị ở cuối trang PR. GitHub Actions kiểm tra thay đổi không làm lỗi quá trình dựng trang; Netlify dựng bản xem trước để reviewer kiểm tra (nhấn Details để xem);
3.  Reviewer có thể phát hiện vấn đề và gửi `review` hoặc `suggested changes` (biểu tượng xám)/`requested changes` (biểu tượng đỏ, chỉ reviewer có quyền ghi mới có). Thường reviewer sẽ kèm nhận xét và hướng dẫn sửa, bạn cần tiếp tục bổ sung thay đổi vào Pull Request. Xem hướng dẫn ở phần `Chỉnh sửa trên GitHub` hoặc `Chỉnh sửa trên máy bằng Git` > `Thêm thay đổi vào Pull Request`;
4.  Khi đủ reviewer đồng ý, PR sẽ được hợp nhất vào nhánh master;
5.  Sau khi hợp nhất, GitHub Actions sẽ dựng lại nội dung và cập nhật lên nhánh gh-pages;
6.  Máy chủ sẽ lấy bản mới từ gh-pages và triển khai nội dung mới nhất.

## Tài liệu tham khảo & chú thích

[^ref1]: [Wikipedia: Hướng dẫn cho người mới/Chỉnh sửa](https://zh.wikipedia.org/wiki/Wikipedia:%E6%96%B0%E6%89%8B%E5%85%A5%E9%96%80/%E7%B7%A8%E8%BC%AF)

[^ref2]: [Web-based editor - GitHub Codespaces - GitHub Docs](https://docs.github.com/en/codespaces/developing-in-codespaces/web-based-editor)
