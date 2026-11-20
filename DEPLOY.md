# EchoReader 生产部署包

## 部署步骤（Node.js 20+）

```bash
npm install --omit=dev     # 安装生产依赖
cp .env.example .env       # 按平台要求填写环境变量（数据库 / OAuth 等）
npm start                  # 启动 Hono 服务，默认 http://localhost:3000
```

- `dist/public/` 为前端静态资源（由 Hono 直接托管）
- `dist/boot.js` 为后端入口（tRPC API、Kimi OAuth、`/api/extract` 正文提取）

## 功能亮点

地道英语发音朗读：网页提取 / 粘贴正文 / 内置名著，句级+词级高亮跟随，
英音/美音音色、语速(0.6–1.6×)、音调调节，单句循环复读，
登录后同步进度/收藏/生词本，PWA 可安装。
