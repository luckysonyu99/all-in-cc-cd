# all-in.cc.cd - AI API Gateway

基于 New API 的 AI API 中转服务，支持 Claude、GPT 等主流模型。

## 🚀 快速开始

### 在线访问

访问 [https://all-in.cc.cd](https://all-in.cc.cd)

### 本地开发

```bash
# 克隆仓库
git clone https://github.com/YOUR_USERNAME/all-in-cc-cd.git
cd all-in-cc-cd

# 启动后端
go run main.go

# 启动前端
cd web/default
bun install
bun run dev
```

## 📦 部署

详细部署指南请查看 [DEPLOY_GUIDE.md](./DEPLOY_GUIDE.md)

### 架构

- **前端**: Cloudflare Pages (免费)
- **后端**: Railway (免费层)
- **数据库**: SQLite

### 快速部署步骤

1. Fork 本仓库到你的 GitHub
2. 在 Railway 部署后端（自动检测 Dockerfile）
3. 在 Cloudflare Pages 部署前端
4. 配置自定义域名 all-in.cc.cd

详细步骤请参考 [DEPLOY_GUIDE.md](./DEPLOY_GUIDE.md)

## ✨ 功能特性

- 🔐 用户管理和权限控制
- 🔑 API 密钥管理
- 📊 使用统计和监控
- 💰 透明计费系统
- 🌍 多模型支持（Claude、GPT、Gemini 等）
- ⚡ 高性能 API 转发
- 🛡️ 企业级安全保障
- 📱 响应式 Web 界面

## 🛠️ 技术栈

### 后端
- Go 1.22+
- Gin Web Framework
- GORM ORM
- SQLite/MySQL/PostgreSQL

### 前端
- React 19
- TypeScript
- Rsbuild
- Tailwind CSS
- Base UI

### 部署
- Railway (后端)
- Cloudflare Pages (前端)

## 📝 配置说明

### 后端环境变量

参考 `.env.railway` 文件：

```bash
SESSION_SECRET=your-random-secret
FRONTEND_BASE_URL=https://all-in.cc.cd
SQLITE_PATH=/app/data/new-api.db
MEMORY_CACHE_ENABLED=true
GENERATE_DEFAULT_TOKEN=true
```

### 前端环境变量

编辑 `web/default/.env.production`：

```bash
VITE_REACT_APP_SERVER_URL=https://your-railway-app.railway.app
```

## 🔧 开发指南

### 后端开发

```bash
# 安装依赖
go mod download

# 运行开发服务器
go run main.go

# 构建生产版本
go build -o new-api
```

### 前端开发

```bash
cd web/default

# 安装依赖（推荐使用 bun）
bun install

# 开发模式（热重载）
bun run dev

# 类型检查
bun run typecheck

# 构建生产版本
bun run build
```

## 📊 成本估算

- **Cloudflare Pages**: 免费
- **Railway 免费层**: $5/月额度（通常够用）
- **域名**: 已有
- **总计**: $0/月（在免费额度内）

如果超出免费额度：
- Railway Pro: $5/月起
- 或迁移到 VPS: $3-5/月

## 🐛 常见问题

### 前端无法连接后端

检查：
1. `.env.production` 中的后端 URL 是否正确
2. Railway 服务是否正常运行
3. CORS 配置是否正确

### Railway 免费额度用完

解决方案：
1. 升级到 Railway Pro（$5/月）
2. 迁移到其他平台（Render、Fly.io）
3. 使用便宜的 VPS

### 数据丢失问题

原因：SQLite 存储在容器中，重启会丢失

解决：
1. 在 Railway 添加 Volume 持久化
2. 或迁移到 PostgreSQL

## 📄 许可证

本项目基于 [New API](https://github.com/QuantumNous/new-api) 修改。

原项目采用 AGPL-3.0 许可证。

## 🙏 致谢

- [New API](https://github.com/QuantumNous/new-api) - 原始项目
- [Cloudflare](https://cloudflare.com) - 前端托管
- [Railway](https://railway.app) - 后端托管

## 📮 支持

如有问题或建议：
- 提交 GitHub Issue
- 查看 [部署指南](./DEPLOY_GUIDE.md)
- 参考 New API 官方文档

---

**立即部署你的 AI API 网关！** 🚀
