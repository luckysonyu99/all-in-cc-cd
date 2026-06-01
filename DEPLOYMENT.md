# Cloudflare Pages 构建配置

## 构建设置
- Build command: `cd web/default && bun install && bun run build`
- Build output directory: `web/default/dist`
- Root directory: `/`
- Node version: `20`

## 环境变量
需要在 Cloudflare Pages 设置中添加以下环境变量：

```
VITE_API_BASE_URL=https://your-railway-app.railway.app
```

## 部署步骤

### 1. 准备 Railway 后端部署

1. 访问 https://railway.app
2. 使用 GitHub 账号登录
3. 点击 "New Project"
4. 选择 "Deploy from GitHub repo"
5. 选择你 fork 的 all-in-cc-cd 仓库
6. Railway 会自动检测 Dockerfile 并开始构建

### 2. 配置 Railway 环境变量

在 Railway 项目设置中添加以下环境变量：
```
SESSION_SECRET=your-random-secret-string-here
FRONTEND_BASE_URL=https://all-in.cc.cd
SQLITE_PATH=/app/data/new-api.db
MEMORY_CACHE_ENABLED=true
GENERATE_DEFAULT_TOKEN=true
NODE_TYPE=master
```

### 3. 获取 Railway 后端 URL

部署完成后，Railway 会提供一个 URL，类似：
`https://your-app-name.railway.app`

### 4. 部署前端到 Cloudflare Pages

1. 访问 https://dash.cloudflare.com
2. 进入 "Workers & Pages"
3. 点击 "Create application" > "Pages" > "Connect to Git"
4. 选择你的 GitHub 仓库
5. 配置构建设置：
   - Build command: `cd web/default && bun install && bun run build`
   - Build output directory: `web/default/dist`
   - Root directory: `/`
6. 添加环境变量：
   - `VITE_API_BASE_URL`: 你的 Railway 后端 URL

### 5. 配置自定义域名

在 Cloudflare Pages 项目设置中：
1. 进入 "Custom domains"
2. 添加 `all-in.cc.cd`
3. 按照提示配置 DNS 记录

## 注意事项

- Railway 免费层每月有 $5 的额度限制
- 如果流量较大，可能需要升级到付费计划
- SQLite 数据库文件存储在容器中，重启会丢失数据
- 建议后续迁移到 PostgreSQL 或 MySQL
