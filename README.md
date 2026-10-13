# Bài 4 — Tự động triển khai Spring Boot lên VPS qua SSH

**Sinh viên:** Nguyễn Đăng Dương  
**Lớp:** CNTT2  
**Môn học:** IT209 — Session 12

Workflow `.github/workflows/deploy-vps.yml` build Docker image từ Dockerfile, publish `ghcr.io/<owner>/app:latest`, rồi chạy job deploy sau khi build thành công. Trên VPS, job đăng nhập GHCR bằng `GITHUB_TOKEN` tạm thời, pull image mới, thay container `app` và publish cổng `8080`.

## Repository Secrets cần khai báo

Vào **Settings → Secrets and variables → Actions → New repository secret** và tạo:

| Secret | Giá trị |
|---|---|
| `SSH_HOST` | IP hoặc hostname VPS |
| `SSH_USER` | Tài khoản SSH có quyền chạy Docker |
| `SSH_KEY` | Nội dung private key SSH tương ứng |

Không commit private key hay mật khẩu vào repository. Khi thiếu một trong ba secret SSH, bước deploy sẽ được bỏ qua và workflow ghi thông báo. Sau khi khai báo, push lên `main` để build image và deploy.

VPS cần cài Docker, cho phép SSH từ GitHub Actions và mở cổng `8080`. Ứng dụng cung cấp health endpoint tại `/actuator/health`.

## Chạy cục bộ

```bash
./gradlew test bootJar
```

Để kiểm tra image cục bộ: `docker build -t session12-app:local .` rồi chạy container trên cổng `8080`.

