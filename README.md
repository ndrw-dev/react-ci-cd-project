# 🚀 Automated CI/CD Project: Static Web Deployment using CodeDeploy

This project outlines the construction of a complete CI/CD Pipeline, fully integrating AWS Developer Tools (CodePipeline, CodeDeploy) to deploy a static React application to Amazon S3.
---

## 🖼️ 1. Architecture Diagram

This diagram illustrates the project's workflow, focusing on integrating AWS CodeDeploy into the static application deployment process:

![Project's Structer](https://media.discordapp.net/attachments/1331318634294808711/1434439381556527184/Structure.png?ex=69085547&is=690703c7&hm=e1de377653428477a479f1db55e66a9af4c122408d90351abdd207f196a7b90c&=&format=webp&quality=lossless)

**Workflow Summary:**

1.  **Source (GitHub):**  The Developer commits code to the branch (`main`) on **GitHub**.
2.  **Orchestration (CodePipeline):** **AWS CodePipeline** automatically triggers the pipeline.
3.  **Build (CodeBuild):** The source code is passed to **AWS CodeBuild** to compile the React application and create an Artifact containing the static files. (`build` folder).
4.  **Deployment (CodeDeploy):** The Artifact is handed over to **AWS CodeDeploy**  to manage and execute the deployment based on defined steps.
5.  **Hosting (Amazon S3):** CodeDeploy (or a subsequent action after CodeDeploy) deploys the compiled files to **Amazon S3**, , configured for Static Website Hosting.

---

## 🛠️ 2. Công cụ và Công nghệ (Tech Stack)

| Lĩnh vực | Công cụ/Dịch vụ | Mục đích trong dự án |
| :--- | :--- | :--- |
| **Source Code** | React, Node.js |	Static Web Application. |
| **Source Control** | **GitHub** | Source code repository and pipeline trigger point. |
| **CI Orchestration** | **AWS CodePipeline** | Service orchestrating the entire automated process. |
| **Build & Test** | **AWS CodeBuild** | 	Compiles source code and runs build commands defined in the  `buildspec.yml` file. |
| **Deployment Management** | **AWS CodeDeploy** | 	Manages the Artifact deployment process, demonstrating proficiency with the DevTools suite. |
| **Hosting** | **Amazon S3** | Stores and serves the static web application. |

---

## ⚙️ 3. Deployment and Configuration

This section highlights key configuration files and important technical points.

### 3.1. Build Configuration (File `buildspec.yml`)

This file instructs CodeBuild on how to compile the React application.

```yaml
version: 0.2

phases:
  install:
    commands:
      # Use Node.js LTS
      - echo Installing dependencies...
      - npm install
  build:
    commands:
      # Create the 'build' folder containing static files
      - echo Build started on `date`
      - npm run build
artifacts:
  files:
    - '**/*'
  base-directory: build 
  discard-paths: yes
```

### 3.2. Deployment
The Deploy stage in CodePipeline uses the S3 Action Provider to copy the files.


## 🔗 4. Results
Let’s test the whole pipeline. I’ll make a small change to the homepage text and push it to GitHub.

As soon as the code is pushed, CodePipeline is triggered. You’ll see it run through the source, build, and deploy stages.


![Project's Structer](https://media.discordapp.net/attachments/1331318634294808711/1434448687198634146/image.png?ex=69085df1&is=69070c71&hm=972d2c08a2f0e3cbc60b3f4b56f7ec1cc5216e0c3be26e3f3a6201fc385fd13b&=&format=webp&quality=lossless&width=1557&height=777)
