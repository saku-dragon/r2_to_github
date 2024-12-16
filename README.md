# Cloudflare R2 + Backblaze B2 ---> GitHub 文件迁移工具

这个 GitHub Action 用于将 Cloudflare R2、Backblaze B2 存储中的文件迁移到 GitHub 仓库，支持根据策略选择文件的迁移目标仓库。支持按照文件大小或文件数量的策略进行选择，支持迁移后删除 R2 和 B2 中的文件。

## 功能

- 从 Cloudflare R2、Backblaze B2 存储中获取文件列表。
- 从 Cloudflare Worker 中获取所有的账户信息。
- 根据选择的策略将文件迁移到指定的 GitHub 仓库。
- 支持三种策略：根据仓库文件数量最少、仓库容量最小和指定目标仓库。
- 支持迁移后，可以选择保留或者删除 R2、B2 中的文件。

## 配置

通过 Cloudflare API，获取对应 worker 里的账户配置

## 使用说明

### 在 Cloudflare 面板创建可以读取 Worker 项目的 API, https://dash.cloudflare.com/profile/api-tokens

![image](https://github.com/user-attachments/assets/9e49b29a-54ae-46f0-aeda-28d95f4a9041)
![image](https://github.com/user-attachments/assets/11dceb4b-ab2e-41a8-b8e4-7317bcf4b50f)
![image](https://github.com/user-attachments/assets/b1e6f1c3-3d8d-4ba3-8d98-35ab4f061b14)
![image](https://github.com/user-attachments/assets/81e66642-cd5c-43d3-bb72-7fecf24e16a3)
![image](https://github.com/user-attachments/assets/3c832e81-bfc6-480d-939c-1d0731a07c17)

### 在 Action 处设置 3 个 secret 变量

![image](https://github.com/user-attachments/assets/25b8d0fa-8302-4cb9-a6db-83e449e9664c)

### Action yml 内容
```
name: S3 --> GitHub
 
on:
  schedule:
    - cron: '0 * * * *'  # 每小时运行一次
  workflow_dispatch:  # 允许手动触发

jobs:
  S3:
    runs-on: ubuntu-latest

    env:
      ACCOUNT_ID: ${{ secrets.ACCOUNT_ID }}
      WORKER_NAME: ${{ secrets.WORKER_NAME }}
      API_TOKEN: ${{ secrets.API_TOKEN }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v4.2.2

      - name: Sync files to GitHub
        uses: fscarmen3/r2_to_github@v1.0.2
```