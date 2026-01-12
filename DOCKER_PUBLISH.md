步骤：

1. 在 GitHub 创建新仓库并将本地代码推送到该仓库（或将此目录作为现有仓库的一个分支/提交）。

2. 在 GitHub 仓库的 `Settings -> Secrets and variables -> Actions -> New repository secret` 中添加两个 Secret：
   - `DOCKERHUB_USERNAME`：你的 Docker Hub 用户名
   - `DOCKERHUB_TOKEN`：你的 Docker Hub 登录令牌（或密码）

3. 在 GitHub 仓库页面的 `Actions` -> `docker-publish` 工作流，点击 `Run workflow` 并填写：
   - `image_name`：例如 `yourusername/nsfw_detector`
   - `tag`：例如 `latest`

4. 触发后，Actions 将自动构建镜像并推送到你指定的 Docker Hub 仓库（确保仓库名与 Docker Hub 上的命名一致）。

本地构建与推送（如果你在本机安装并登录了 Docker）：

```powershell
# 进入项目目录
pushd "d:\下载\Browser\nsfw_detector-main\nsfw_detector-main"

# 构建镜像（替换 yourusername/nsfw_detector:latest）
docker build -t yourusername/nsfw_detector:latest -f dockerfile .

# 登录 Docker Hub（如果尚未登录）
docker login

# 推送镜像
docker push yourusername/nsfw_detector:latest
```

注意：项目的 `dockerfile` 已存在，Actions 工作流会直接使用它。如果你希望我把默认 `image_name` 改为某个指定值或添加多平台构建，请告诉我。