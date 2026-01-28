本页面主要解答一些常见的问题．

## Tôi muốn hỏi về Wiki này

Q: Vì sao các bạn lại muốn xây dựng Wiki này?

A: Khi học **OI**, bạn có từng cảm thấy lạc lõng trước một hệ thống kiến thức khổng lồ? **OI Wiki** mong muốn giúp nhiều bạn học sinh ở những nơi thiếu tài nguyên có thể dễ dàng tiếp cận nguồn tài liệu luyện tập. Tất nhiên, động lực xây dựng Wiki cũng rất đơn giản: chỉ là muốn đóng góp một phần nhỏ cho sự phát triển của **OI** mà thôi. XD

***

Q: Tôi rất hứng thú, làm sao để tham gia?

A: **OI Wiki** hiện được lưu trữ trên GitHub, bạn có thể truy cập [repo này](https://github.com/huythedev/K23OJ-OI-wiki) để xem tiến độ mới nhất. Bạn có thể tham gia bằng cách mở [Issue](https://github.com/huythedev/K23OJ-OI-wiki/issues), [Pull Request](https://github.com/huythedev/K23OJ-OI-wiki/pulls) trên GitHub, hoặc chia sẻ ý tưởng trong nhóm chat, gửi bài trực tiếp cho quản trị viên. Hiện tại, dự án sử dụng framework [MkDocs](https://mkdocs.readthedocs.io) viết bằng Python, hỗ trợ Markdown (và cả công thức toán học).

***

Q: Nhưng tôi còn yếu... không biết mình giúp được gì.

A: Mọi thứ bắt đầu từ đam mê. Bạn có thể giúp kiểm tra, duyệt bài, quảng bá **OI Wiki**, hoặc góp phần xây dựng môi trường học tập tích cực cho cộng đồng!

***

Q: Ai là người chính đang làm dự án này? Đây là một dự án lớn, liệu có thể hoàn thiện không?

A: Ban đầu là một số bạn đã nghỉ thi đấu OI, sau đó có thêm nhiều bạn cùng chí hướng: có cả tuyển thủ đang thi, cựu tuyển thủ, và cả những người chưa từng thi **OI**. Hiện tại, dự án chủ yếu do nhóm **OI Wiki** duy trì (dưới đây là ảnh nhóm).

<a href="https://github.com/huythedev/K23OJ-OI-wiki/graphs/contributors"><img src="https://opencollective.com/oi-wiki/contributors.svg?width=890&button=false"/></a>

Tất nhiên, chỉ dựa vào chúng tôi thì khó có thể hoàn thiện, rất mong bạn cùng chung tay xây dựng **OI Wiki**.

***

Q: Làm sao để đảm bảo nội dung tôi đóng góp không bị mất?

A: Nội dung được lưu trữ trên [GitHub](https://github.com/huythedev/K23OJ-OI-wiki), nên kể cả khi server gặp sự cố, dữ liệu vẫn an toàn. Ngoài ra, chúng tôi cũng thường xuyên sao lưu, nên kể cả khi GitHub "biến mất" (?), nội dung vẫn được giữ lại.

***

Q: **OI Wiki** có nhiều trang còn trống?

A: Đúng vậy. Do giới hạn về thời gian và năng lực của thành viên dự án, chúng tôi chưa thể hoàn thiện hết các trang. Vì vậy, chúng tôi luôn chào đón bạn cùng tham gia đóng góp để hoàn thiện **OI Wiki**.

***

Q: Tại sao không viết trực tiếp trên [Wikipedia tiếng Trung](https://zh.wikipedia.org/)?

A: Vì chúng tôi muốn thực sự giúp đỡ nhiều bạn học sinh, tuyển thủ hoặc những người quan tâm đến lĩnh vực này. Ngoài ra, do một số lý do khách quan, nội dung trên Wikipedia tiếng Trung không phải lúc nào cũng dễ dàng truy cập.

## Tôi muốn tham gia!

Q: Làm sao để liên hệ với nhóm dự án?

A: Bạn có thể xem thông tin liên hệ tại [Giới thiệu - Kênh trao đổi](./about.md#交流方式).

***

Q: Làm sao để đóng góp code hoặc nội dung?

Vui lòng xem trang [Hướng dẫn tham gia](./htc.md).

***

Q: Mục lục ở đâu?

A: Mục lục nằm trong file [mkdocs.yml](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/mkdocs.yml#L17) ở thư mục gốc của dự án.

***

Q: Làm sao sửa nội dung một topic?

A: Ở góc trên bên phải mỗi trang có nút chỉnh sửa <i class="md-icon">edit</i>, nhấn vào và xác nhận đã đọc [Hướng dẫn đóng góp](./htc.md), bạn sẽ được chuyển tới file tương ứng trên GitHub.

Hoặc bạn cũng có thể tự đọc mục lục [(mkdocs.yml)](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/mkdocs.yml) để tìm vị trí file.

***

Q: Làm sao thêm một topic mới?

A: Có hai cách:

-   Mở một Issue, ghi rõ nội dung muốn thêm.
-   Mở một Pull Request, thêm topic mới vào [(mkdocs.yml)](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/mkdocs.yml), đồng thời tạo file `.md` tương ứng trong thư mục [docs](https://github.com/huythedev/K23OJ-OI-wiki/tree/master/docs). Định dạng tài liệu xem tại [Hướng dẫn định dạng](./format.md#贡献文档要求).

***

Q: Tôi gặp khó khăn khi truy cập GitHub.

A: Bạn có thể thêm các dòng sau vào file hosts[^ref1]:

```text
# GitHub Start
140.82.114.25                 alive.github.com
140.82.113.5                  api.github.com
185.199.110.153               assets-cdn.github.com
185.199.111.133               avatars.githubusercontent.com
185.199.111.133               avatars0.githubusercontent.com
185.199.111.133               avatars1.githubusercontent.com
185.199.111.133               avatars2.githubusercontent.com
185.199.111.133               avatars3.githubusercontent.com
185.199.111.133               avatars4.githubusercontent.com
185.199.111.133               avatars5.githubusercontent.com
185.199.111.133               camo.githubusercontent.com
140.82.112.22                 central.github.com
185.199.111.133               cloud.githubusercontent.com
140.82.114.9                  codeload.github.com
140.82.113.22                 collector.github.com
185.199.111.133               desktop.githubusercontent.com
185.199.111.133               favicons.githubusercontent.com
140.82.112.3                  gist.github.com
52.216.163.147                github-cloud.s3.amazonaws.com
52.217.124.1                  github-com.s3.amazonaws.com
52.216.144.83                 github-production-release-asset-2e65be.s3.amazonaws.com
52.217.121.249                github-production-repository-file-5c1aeb.s3.amazonaws.com
52.217.206.57                 github-production-user-asset-6210df.s3.amazonaws.com
192.0.66.2                    github.blog
140.82.114.4                  github.com
140.82.113.18                 github.community
185.199.110.154               github.githubassets.com
151.101.1.194                 github.global.ssl.fastly.net
185.199.110.153               github.io
185.199.111.133               github.map.fastly.net
185.199.110.153               githubstatus.com
140.82.112.25                 live.github.com
185.199.111.133               media.githubusercontent.com
185.199.111.133               objects.githubusercontent.com
13.107.42.16                  pipelines.actions.githubusercontent.com
185.199.111.133               raw.githubusercontent.com
185.199.111.133               user-images.githubusercontent.com
13.107.253.40                 vscode.dev
140.82.112.21                 education.github.com
# GitHub End
```

Bạn có thể xem thêm tại [GitHub520](https://gitee.com/klmahuaw/GitHub520).

Người dùng Linux/macOS có thể dùng script [gh-check của依云](https://gist.github.com/lilydjwg/93d33ed04547e1b9f7a86b64ef2ed058) để tìm IP GitHub nhanh nhất, dùng tham số `--hosts` để cập nhật file hosts, `--help` để xem hướng dẫn. Cần cài Python3 và aiohttp (`pip install aiohttp -i https://pypi.tuna.tsinghua.edu.cn/simple/`). Xem thêm tại blog: [Tìm IP GitHub nhanh nhất](https://blog.lilydjwg.me/2019/8/16/gh-check.214730.html).

Bạn cũng có thể dùng dịch vụ [Gitclone](https://www.gitclone.com/) để tăng tốc clone, hướng dẫn chi tiết trên trang chủ.

Nếu chỉ muốn clone repo **OI Wiki**:

```bash
git clone https://gitclone.com/github.com/huythedev/K23OJ-OI-wiki
```

Nếu muốn đóng góp cho **OI Wiki**, hãy fork repo, sau đó (thay `username` bằng tên của bạn), chú ý ví dụ dưới dùng SSH[^only-ssh-connect]:

```bash
git clone https://gitclone.com/github.com/username/K23OJ-OI-wiki
git remote set-url origin git@github.com:username/K23OJ-OI-wiki.git
```

***

Q: pip của tôi quá chậm!

A: Có thể đổi sang nguồn trong nước[^ref2], hoặc:

```bash
pip install -U -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

***

Q: Clone repo về máy quá chậm.

A: Nếu dùng `git bash`, có thể thêm tham số để giảm dung lượng tải về[^ref3]:

```bash
git clone https://github.com/huythedev/K23OJ-OI-wiki.git --depth=1 -b master
```

***

Q: Tôi chưa cài Python 3.

A: Truy cập [Trang chủ Python](https://www.python.org/downloads/) để tải và cài đặt.

***

Q: pip báo phiên bản quá thấp.

A: Mở cmd/shell, chạy:

```bash
python -m pip install --upgrade pip
```

***

Q: Tôi cài dependencies bị lỗi.

A: Kiểm tra lại: mạng? quyền truy cập? Xem kỹ thông báo lỗi.

***

Q: Đã clone về rồi mà không deploy được?

A: Kiểm tra đã cài đủ dependencies chưa?

***

Q: Tôi clone repo từ lâu, làm sao cập nhật lên bản mới?

A: Xem hướng dẫn chính thức của GitHub: [Syncing a fork - GitHub Docs](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/syncing-a-fork).

***

Q: Nếu đã cài dependencies cũ, làm sao cập nhật?

A: Chạy lệnh sau:

```bash
pip install -U -r requirements.txt
```

***

Q: Tại sao định dạng markdown của tôi bị lỗi?

A: Tham khảo [ghi chú của cyent](https://cyent.github.io/markdown-with-mkdocs-material/) hoặc [Hướng dẫn MkDocs](https://github.com/ctf-wiki/ctf-wiki/wiki/Mkdocs-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E).

Hiện tại, chúng tôi dùng [remark-lint](https://github.com/remarkjs/remark-lint) để tự động sửa định dạng, nếu có vấn đề về cấu hình, hãy góp ý tại [file cấu hình](https://github.com/huythedev/K23OJ-OI-wiki/blob/master/.remarkrc).

***

Q: GitHub không hiển thị công thức toán học?

A: Đúng vậy, GitHub không hỗ trợ preview công thức toán học. Nhưng MkDocs hỗ trợ MathJax, bạn có thể dùng thoải mái.

***

Q: Công thức toán học bị lỗi hiển thị?

A: Nếu là công thức dạng block (`$$`), cần để `$$` trên một dòng riêng, hai bên có dòng trống, không có dấu cách ở đầu dòng. Ví dụ:

```text
// Dòng trống
$$
a_i
$$
// Dòng trống
```

***

Q: Công thức trong mục lục bị lỗi hiển thị (bị lặp lại)?

A: Đây là bug của python-markdown, có thể sẽ được sửa trong tương lai.

Nếu muốn tránh lỗi này, tham khảo cách viết mục lục của phần SAM trong [string](https://github.com/huythedev/K23OJ-OI-wiki/blame/master/docs/string/sam.md#L73):

```text
Kết thúc <script type="math/tex">endpos</script>
```

Trong mục lục sẽ hiển thị:

```text
Kết thúc endpos
```

Lưu ý: Hạn chế dùng MathJax trong mục lục.

***

Q: Làm sao khai báo bản quyền riêng cho một trang?

A: Thêm dòng sau vào đầu file[^ref4]:

```text
copyright: SATA
```

Mặc định là CC BY-SA 4.0 và SATA.

***

Q: Tại sao tên tôi không xuất hiện trong danh sách tác giả?

A: Nếu bạn đã đóng góp cho một trang mà không thấy tên mình, hãy thêm GitHub ID vào trường [author](./htc.md#author-字段) ở đầu file.

***

Cảm ơn bạn đã đọc đến đây, chúng tôi rất cần sự giúp đỡ của bạn!

**Nhóm dự án OI Wiki**

2018.8

## Tài liệu tham khảo & chú thích

[^ref1]: [GitHub520](https://gitee.com/klmahuaw/GitHub520)

[^ref2]: [Đổi nguồn pip sang mirror trong nước - L瑜 - CSDN Blog](https://blog.csdn.net/lambert310/article/details/52412059)

[^ref3]: [GIT--- Hướng dẫn từng bước (Windows Git Bash)](https://blog.csdn.net/FreeApe/article/details/46845555)

[^ref4]: [Metadata - Material for MkDocs](https://squidfunk.github.io/mkdocs-material/extensions/metadata/#usage)

[^only-ssh-connect]: GitHub đã bỏ xác thực HTTPS bằng mật khẩu, cần dùng SSH hoặc Personal Access Token, xem [Nên dùng URL nào?](https://docs.github.com/cn/github/using-git/which-remote-url-should-i-use), [Tạo Personal Access Token](https://docs.github.com/cn/github/authenticating-to-github/creating-a-personal-access-token) và [Kết nối GitHub bằng SSH](https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh).
