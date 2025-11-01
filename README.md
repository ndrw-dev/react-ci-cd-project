# 🚀 Dự án CI/CD Tự động Triển khai Web Tĩnh theo Mô hình CodeDeploy

Dự án này mô tả việc xây dựng một Pipeline CI/CD hoàn chỉnh, tích hợp đầy đủ các dịch vụ AWS Developer Tools (CodePipeline, CodeDeploy) để triển khai ứng dụng React tĩnh lên Amazon S3.

---

## 🖼️ 1. Kiến trúc Giải pháp (Architecture Diagram)

Đây là sơ đồ thể hiện luồng làm việc của dự án, tập trung vào việc tích hợp AWS CodeDeploy vào quy trình triển khai ứng dụng tĩnh:



**Tóm tắt Luồng làm việc (Workflow):**

1.  **Source (GitHub):** Developer commit code lên nhánh chính (`main`) trên **GitHub**.
2.  **Orchestration (CodePipeline):** **AWS CodePipeline** tự động kích hoạt Pipeline.
3.  **Build (CodeBuild):** Mã nguồn được chuyển qua **AWS CodeBuild** để biên dịch (compile) ứng dụng React và tạo ra Artifact chứa các file tĩnh (`build` folder).
4.  **Deployment (CodeDeploy):** Artifact được chuyển sang **AWS CodeDeploy** để quản lý và thực hiện triển khai theo các bước được định nghĩa.
5.  **Hosting (Amazon S3):** CodeDeploy (hoặc một bước hành động sau CodeDeploy) triển khai các file đã biên dịch lên **Amazon S3**, được cấu hình làm Static Website Hosting.

---

## 🛠️ 2. Công cụ và Công nghệ (Tech Stack)

| Lĩnh vực | Công cụ/Dịch vụ | Mục đích trong dự án |
| :--- | :--- | :--- |
| **Mã nguồn** | React, Node.js | Ứng dụng Web tĩnh. |
| **Source Control** | **GitHub** | Kho lưu trữ mã nguồn và điểm kích hoạt Pipeline. |
| **CI Orchestration** | **AWS CodePipeline** | Dịch vụ điều phối toàn bộ quy trình tự động. |
| **Build & Test** | **AWS CodeBuild** | Biên dịch mã nguồn và chạy lệnh Build theo file `buildspec.yml`. |
| **Deployment Management** | **AWS CodeDeploy** | Quản lý quy trình triển khai Artifact, thể hiện khả năng làm việc với bộ công cụ DevTools. |
| **Hosting** | **Amazon S3** | Lưu trữ và phục vụ ứng dụng web tĩnh. |
| **Bảo mật** | **AWS IAM** | Quản lý các Service Role với nguyên tắc **Least Privilege Principle**. |
| **(Tùy chọn)** | **[Terraform/CloudFormation]** | (Nếu có) Quản lý toàn bộ cơ sở hạ tầng AWS dưới dạng mã (IaC). |

---

## ⚙️ 3. Chi tiết Triển khai và Cấu hình

Phần này nhấn mạnh các file cấu hình và điểm kỹ thuật quan trọng.

### 3.1. Cấu hình Build (File `buildspec.yml`)

File này hướng dẫn CodeBuild cách biên dịch ứng dụng React.

```yaml
version: 0.2
# ... (Nội dung đầy đủ của install, pre_build, build)
artifacts:
  files:
    - '**/*'
  base-directory: build # Thư mục chứa các file tĩnh sau khi biên dịch
  # ...
```

### 3.2. Triển khai và Quản lý Cache
- **Triển khai**: Giai đoạn Deploy trong CodePipeline sử dụng Action Provider S3 để sao chép các file.

- **Cache Invalidation**: Một Action CodeBuild riêng biệt được thêm vào sau Deploy để chạy lệnh AWS CLI, xóa cache trên CloudFront. Điều này là bằng chứng cho việc bạn hiểu rõ về hiệu suất và phân phối nội dung.

## 🔗 4. Kết quả và Liên kết (Results & Links)
