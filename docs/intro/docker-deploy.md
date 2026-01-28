Trang này sẽ hướng dẫn cách triển khai môi trường **OI Wiki** bằng Docker.

???+ warning "Cảnh báo"
    Các bước dưới đây cần thực hiện với quyền root hoặc tài khoản thuộc nhóm docker.

## Kéo image **OI Wiki**

```bash
# Chạy một trong các lệnh sau trên máy chủ
# Docker Hub (kho chính thức)
docker pull 24oi/oi-wiki
# DaoCloud Hub (kho trong nước)
docker pull daocloud.io/sirius/oi-wiki
# Tencent Hub (kho trong nước)
docker pull ccr.ccs.tencentyun.com/oi-wiki/oi-wiki
```

## Tự xây dựng image

```bash
# Chạy trên máy chủ
# Clone repo Git
git clone https://github.com/huythedev/K23OJ-OI-wiki.git
cd OI-wiki/
# Xây dựng image
docker build -t [name][:tag] . --build-arg [variable1]=[value1] [variable2]=[value2]...
```

-   (Bắt buộc) Đặt `[name]` làm tên image, (tùy chọn) đặt `[tag]` làm tag (nếu có tag thì tên image sẽ gồm hai phần).
-   Có thể dùng tham số `--build-arg` để truyền biến môi trường.

Các biến môi trường có thể sử dụng:

-   Có thể đặt `WIKI_REPO` để dùng mirror repo Wiki (nếu không đặt sẽ mặc định dùng GitHub).
-   Có thể đặt `PYPI_MIRROR` để dùng mirror PyPI (nếu không đặt sẽ mặc định dùng PyPI chính thức).
    -   Trong nước nên dùng mirror TUNA: `https://pypi.tuna.tsinghua.edu.cn/simple/`
-   Có thể đặt `LISTEN_IP` để đổi IP lắng nghe (mặc định là `0.0.0.0`, tức là lắng nghe mọi IP).
-   Có thể đặt `LISTEN_PORT` để đổi cổng lắng nghe (mặc định là `8000`).

Ví dụ:

```bash
docker build -t OI_Wiki . --build-arg WIKI_REPO=https://hub.fastgit.xyz/OI-wiki/OI-wiki.git PYPI_MIRROR=https://pypi.tuna.tsinghua.edu.cn/simple/
# Xây dựng image tên OI_Wiki (tag mặc định), dùng FastGit để clone, dùng mirror TUNA cho PyPI.
```

## Chạy container

```bash
# Chạy trên máy chủ
docker run -d -it [image]
```

-   (Bắt buộc) Đặt `[image]` là tên image. Ví dụ, image từ Docker Hub là `24oi/oi-wiki`; từ DaoCloud Hub là `daocloud.io/sirius/oi-wiki`.
-   (Bắt buộc) Thêm `-p [port]:8000` để ánh xạ cổng container ra cổng máy chủ (nếu không sẽ không mở cổng). Đặt `[port]` là cổng trên máy chủ. Sau đó có thể truy cập **OI Wiki** tại `http://127.0.0.1:[port]`.
-   Thêm `--name [name]` để đặt tên container (mặc định không có tên, thay `[name]` bằng tên tùy chọn. Xem id container bằng `docker ps`).

## Sử dụng container

???+ note "Lưu ý"
    Ví dụ dưới đây dựa trên Ubuntu latest.

Vào container:

```bash
# Chạy trên máy chủ
docker exec -it [name] /bin/bash
```

Nếu khi chạy container không thêm `-d`, sẽ vào bash trực tiếp, thoát ra thì container dừng. Nếu có `-d` thì chạy nền, muốn vào bash thì dùng lệnh trên.

Một số lệnh đặc biệt:

```bash
# Chạy trong container
# Cập nhật repo git
wiki-upd

# Dùng theme tùy chỉnh của chúng tôi
wiki-theme

# Build mkdocs, kết quả ở thư mục site
wiki-bld

# Build mkdocs và render MathJax, kết quả ở thư mục site
wiki-bld-math

# Chạy server, truy cập http://127.0.0.1:8000 trong container hoặc http://127.0.0.1:[port] trên máy chủ để xem
wiki-svr

# Sửa định dạng Markdown
wiki-o
```

Thoát khỏi container:

```bash
# Chạy trong container
# Thoát
exit
```

## Dừng container

```bash
# Chạy trên máy chủ
docker stop [name]
```

## Khởi động container

```bash
# Chạy trên máy chủ
docker start [name]
```

## Khởi động lại container

```bash
# Chạy trên máy chủ
docker restart [name]
```

## Xóa container

```bash
# Chạy trên máy chủ
# Xóa trước khi xóa image
docker rm [name]
```

## Cập nhật image

Chỉ cần `pull` lại, thông thường image sẽ không cập nhật thường xuyên.

## Xóa image

```bash
# Chạy trên máy chủ
# Trước khi xóa image cần xóa hết container dùng image đó
docker rmi [image]
```

## Thắc mắc

Nếu bạn có câu hỏi, hãy tạo [issue](https://github.com/huythedev/K23OJ-OI-wiki/issues/new/choose)!.
