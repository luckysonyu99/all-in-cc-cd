# all-in.cc.cd 部署指南

## 架构概览

- **前端**: Cloudflare Pages (免费)
- **后端**: Railway (免费层 $5/月额度)
- **数据库**: SQLite (内置在 Railway 容器中)
- **域名**: all-in.cc.cd

## 前置准备

1. GitHub 账号
2. Cloudflare 账号
3. Railway 账号 (使用 GitHub 登录)
4. 域名 all-in.cc.cd 已添加到 Cloudflare

---

## 第一步：推送代码到 GitHub

```bash
cd ~/all-in-cc-cd

# 初始化 git（如果还没有）
git init
git add .
git commit -m "Initial commit: all-in.cc.cd setup"

# 在 GitHub 创建新仓库，然后推送
git remote add origin https://github.com/YOUR_USERNAME/all-in-cc-cd.git
git branch -M main
git push -u origin main
```

---

## 第二步：部署后端到 Railway

### 2.1 创建 Railway 项目

1. 访问 https://railway.app
2. 点击 "Login" 使用 GitHub 账号登录
3. 点击 "New Project"
4. 选择 "Deploy from GitHub repo"
5. 授权 Railway 访问你的 GitHub
6. 选择 `all-in-cc-cd` 仓库
7. Railway 会自动检测 Dockerfile 并开始构建

### 2.2 配置环境变量

在 Railway 项目中：

1. 点击你的服务
2. 进入 "Variables" 标签
3. 添加以下环境变量：

```bash
SESSION_SECRET=your-random-secret-string-change-this
FRONTEND_BASE_URL=https://all-in.cc.cd
SQLITE_PATH=/app/data/new-api.db
MEMORY_CACHE_ENABLED=true
GENERATE_DEFAULT_TOKEN=true
NODE_TYPE=master
SYNC_FREQUENCY=600
```

**重要**: 将 `SESSION_SECRET` 改为一个随机字符串！

### 2.3 获取 Railway 后端 URL

1. 在 Railway 项目中，点击 "Settings"
2. 找到 "Domains" 部分
3. 点击 "Generate Domain"
4. 复制生成的域名，类似：`https://all-in-cc-cd-production.up.railway.app`

**保存这个 URL，后面会用到！**

---

## 第三步：部署前端到 Cloudflare Pages

### 3.1 修改前端配置

在部署前，需要修改前端的 API 地址：

```bash
cd ~/all-in-cc-cd/web/default

# 编辑 .env.production 文件
# 将 VITE_REACT_APP_SERVER_URL 改为你的 Railway 后端 URL
```

编辑 `web/default/.env.production`:
```
VITE_REACT_APP_SERVER_URL=https://your-railway-app.railway.app
```

提交修改：
```bash
git add web/default/.env.production
git commit -m "Update API URL for production"
git push
```

### 3.2 创建 Cloudflare Pages 项目

1. 访问 https://dash.cloudflare.com
2. 选择你的账号
3. 进入 "Workers & Pages"
4. 点击 "Create application"
5. 选择 "Pages" 标签
6. 点击 "Connect to Git"

### 3.3 配置构建设置

1. 选择你的 GitHub 仓库 `all-in-cc-cd`
2. 配置构建设置：
   - **Project name**: `all-in-cc-cd`
   - **Production branch**: `main`
   - **Build command**: `cd web/default && npm install && npm run build`
   - **Build output directory**: `web/default/dist`
   - **Root directory**: `/`

3. 点击 "Save and Deploy"

### 3.4 添加环境变量（可选）

如果你想在 Cloudflare Pages 中覆盖环境变量：

1. 进入项目设置
2. 找到 "Environment variables"
3. 添加：
   - `VITE_REACT_APP_SERVER_URL`: 你的 Railway 后端 URL

---

## 第四步：配置自定义域名

### 4.1 在 Cloudflare Pages 添加域名

1. 在 Cloudflare Pages 项目中
2. 进入 "Custom domains"
3. 点击 "Set up a custom domain"
4. 输入 `all-in.cc.cd`
5. 点击 "Continue"

### 4.2 配置 DNS 记录

Cloudflare 会自动为你配置 DNS 记录。如果需要手动配置：

1. 进入 Cloudflare DNS 管理
2. 添加 CNAME 记录：
   - **Type**: CNAME
   - **Name**: `all-in` 或 `@` (根据你的需求)
   - **Target**: `all-in-cc-cd.pages.dev`
   - **Proxy status**: Proxied (橙色云朵)

---

## 第五步：配置 CORS（重要！）

由于前端和后端在不同域名，需要配置 CORS。

### 5.1 在 Railway 添加 CORS 环境变量

在 Railway 项目的环境变量中添加：

```bash
FRONTEND_BASE_URL=https://all-in.cc.cd
```

这个变量会告诉后端允许来自前端域名的请求。

### 5.2 重启 Railway 服务

修改环境变量后，Railway 会自动重启服务。

---

## 第六步：初始化系统

### 6.1 访问网站

访问 `https://all-in.cc.cd`，你应该能看到首页。

### 6.2 注册管理员账号

1. 点击 "注册" 或 "Register"
2. 创建第一个账号（这将是管理员账号）
3. 登录后台

### 6.3 配置系统设置

1. 进入 "系统设置"
2. 配置：
   - 站点名称：`all-in.cc.cd`
   - 站点副标题：自定义
   - Logo：上传你的 Logo
   - 首页内容：可以自定义 HTML/Markdown

### 6.4 添加 API 渠道

1. 进入 "渠道管理"
2. 添加你的 API 密钥（OpenAI、Claude 等）
3. 测试渠道是否正常工作

---

## 验证部署

### 检查清单

- [ ] 前端可以访问 `https://all-in.cc.cd`
- [ ] 可以注册和登录
- [ ] 后端 API 正常响应
- [ ] 可以创建 API 密钥
- [ ] 可以调用 AI 模型

### 测试 API

```bash
# 获取你的 API 密钥后测试
curl https://all-in.cc.cd/api/status
```

---

## 常见问题

### Q1: 前端无法连接后端

**解决方案**:
1. 检查 `.env.production` 中的 `VITE_REACT_APP_SERVER_URL` 是否正确
2. 检查 Railway 后端是否正常运行
3. 检查浏览器控制台的 CORS 错误

### Q2: Railway 免费额度用完了

**解决方案**:
1. Railway 每月提供 $5 免费额度
2. 如果用完，可以：
   - 升级到付费计划（$5/月起）
   - 迁移到其他平台（Render、Fly.io）
   - 使用便宜的 VPS

### Q3: 数据丢失

**原因**: SQLite 数据库存储在容器中，重启会丢失

**解决方案**:
1. 在 Railway 添加 Volume 持久化存储
2. 或者迁移到 PostgreSQL/MySQL

### Q4: 构建失败

**检查**:
1. Node.js 版本是否正确（需要 20+）
2. 构建命令是否正确
3. 查看 Cloudflare Pages 的构建日志

---

## 后续优化

### 1. 添加持久化存储

在 Railway 项目中：
1. 进入 "Settings" > "Volumes"
2. 添加 Volume，挂载到 `/app/data`
3. 这样数据库文件会持久化

### 2. 迁移到 PostgreSQL

如果需要更稳定的数据库：
1. 在 Railway 添加 PostgreSQL 服务
2. 修改环境变量：
   ```bash
   SQL_DSN=postgresql://user:pass@host:5432/dbname
   ```
3. 删除 `SQLITE_PATH` 变量

### 3. 配置监控

1. 在 Railway 查看日志和指标
2. 在 Cloudflare 查看流量统计
3. 考虑添加 Sentry 错误追踪

### 4. 性能优化

1. 启用 Cloudflare CDN 缓存
2. 配置 Redis 缓存（Railway 可添加 Redis 服务）
3. 优化前端资源加载

---

## 成本估算

- **Cloudflare Pages**: 免费
- **Railway**: 免费（$5/月额度）
- **域名**: 已有
- **总计**: $0/月（在免费额度内）

如果超出免费额度：
- Railway Pro: $5/月起
- 或者迁移到 VPS: $3-5/月

---

## 支持

如果遇到问题：
1. 查看 Railway 日志
2. 查看 Cloudflare Pages 构建日志
3. 检查浏览器控制台错误
4. 参考 New API 官方文档

---

**部署完成！享受你的 AI API 网关吧！** 🎉
