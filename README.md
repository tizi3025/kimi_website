# EchoReader · 地道英语发音阅读

把任何英文网页或粘贴的文字读成**地道英语发音**：服务端提取正文、浏览器语音合成逐句朗读、单词级高亮跟随，支持收藏、阅读进度同步（登录后）与生词本。

## 功能

- **网页链接提取**：粘贴 http(s) 链接，服务端用 Readability 提取正文（自动去广告/导航），按段落拆分后进入朗读器；同一链接自动去重。
- **粘贴文字模式**：直接粘贴英文正文，空行分段，立即朗读。
- **朗读器**：Web Speech API 逐句合成；支持英音/美音等全部英语音色选择、语速（0.6–1.6×）与音调调节；句级与词级高亮自动跟随；点任意句从该句开始；上一句/下一句/暂停/停止；单句循环复读；波形动效与进度条。
- **书库**：内置 6 篇公共版权英文名著节选（傲慢与偏见、福尔摩斯、了不起的盖茨比等）+ 提取/粘贴的文章；按标题搜索。
- **登录后**（Kimi OAuth）：阅读进度跨设备同步（可从上次位置继续）、收藏文章、生词本（划词添加、自动带例句、备注、点击听发音）。
- **PWA**：可安装到桌面/手机主屏；Service Worker 网络优先加载 HTML、静态资源缓存。

## 技术栈

- 前端：React 19 + TypeScript + Vite + Tailwind CSS + shadcn/ui
- 后端：Hono + tRPC 11 + Drizzle ORM + MySQL（平台云数据库）
- 正文提取：`@mozilla/readability` + `linkedom`（REST 路由 `/api/extract`，4MB / 15s 上限）
- 语音：浏览器 `speechSynthesis`（句级队列 + `onboundary` 词级高亮，缩写保护如 Mr. / U.S.）

## 目录

```
api/                 Hono 服务：tRPC 路由、Kimi OAuth、/api/extract 提取路由
db/                  Drizzle schema（articles / bookmarks / readingProgress / vocabEntries）+ 名著种子
contracts/           前后端共享类型
src/pages/           Home / Reader / Library / Vocab / Login / NotFound
src/hooks/useSpeech.ts  语音合成控制器（分句、边界事件、队列、调音）
src/components/      AppLayout / PlayerBar（底部播放器）
public/              manifest、Service Worker、图标
```

## 运行

```bash
npm install
npm run db:push        # 同步数据库结构
npx tsx db/seed.ts     # 写入内置名著（可重复执行，幂等）
npm run dev            # 开发：http://localhost:3000（源码 + HMR + API）
npm run build && npm start   # 生产：dist/public 由 Hono 服务
npm run check          # TypeScript 检查
```

## 说明

- 网页提取受运行环境网络策略影响：部分站点可能拒绝抓取，此时会返回友好错误；粘贴文字模式与内置书库不受影响。
- 语音合成依赖浏览器内置音色（Chrome / Edge / Safari / 移动端均自带），未安装任何语音的浏览器会提示不支持。
"# kimi_website" 
