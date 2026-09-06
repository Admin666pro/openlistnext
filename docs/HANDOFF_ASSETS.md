# OpenListNext 核心技术资产与特性交接清单 (Technical Assets & Feature Inventory)

本文档由 **OpenListNext** 原创团队梳理并向 **[OpenListTeam/OpenList-Worker](https://github.com/OpenListTeam/OpenList-Worker)** 官方团队完整交接。

OpenListNext 在从 Go 移植为 TypeScript 边缘全栈的过程中，攻克了大量边缘运行时兼容性难题，并自主研发了插件体系与 AI MCP 基础设施。以下为可直接复用、借鉴或 Cherry-pick 的核心技术资产。

---

## 🏛️ 资产一：全栈纯 Web 标准与 Serverless 多边缘适配层

OpenListNext 核心设计原则是 **Zero Node.js Built-in Modules（零 Node 内置模块依赖）**。后端所有 HTTP 请求、流式传输、加解密仅依赖于 W3C 标准 `fetch`、`ReadableStream`、`Web Crypto API`。

### 适配运行时矩阵

- **Cloudflare Workers**：`wrangler.toml` + KV 自动发现，`ASSETS` 静态资源托管 + Hono API 路由。
- **腾讯云 EdgeOne Pages / Makers**：原生内置 `edgeone.json`，适配 `@edgeone/pages-blob` 解决边缘 KV 协议崩溃问题，内置 Schedules 定时刷新 Token 机制。
- **阿里云 ESA 边缘安全加速**：适配 ESA KV 存储空间与边缘计算运行时。
- **Vercel / AWS Lambda**：`api/[...route].ts` 与 `handler.ts` 导出标准化无服务器句柄。
- **Docker / Node.js 裸机**：`dist-server/` 配套纯静态与文件系统持久化方案。

---

## 💾 资产二：34 款纯 Web 标准存储驱动矩阵 (Storage Drivers)

所有驱动均位于 `src/backend/drivers/`，继承统一的抽象基类，具备自动 Token 刷新、直链解析与分片流式上传能力：

| 驱动分类               | 涵盖网盘与协议                                                                                                                                                                                                                                                                                                                                                                        | 核心特点与资产路径                                                                                                     |
| :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------- |
| **主流商业网盘**       | • 夸克网盘 (`quark`)<br/>• 阿里云盘开放平台 (`aliyundrive_open`)<br/>• 阿里云盘分享 (`aliyundrive_share`)<br/>• 115网盘开放平台 (`115open`)<br/>• 115分享 (`115_share`)<br/>• 百度网盘 (`baidu_netdisk`)<br/>• 123云盘 (`123pan`)<br/>• 123分享 (`123_share`)<br/>• 移动天翼联通 (`139`, `189`, `wopan`)<br/>• 迅雷云盘 (`thunder`)<br/>• 腾讯微云 (`weiyun`)<br/>• 蓝奏云 (`lanzou`) | • 纯 Web API 实现，无需调用外部 Go/C 二进制<br/>• 优化了分享链接解析与批量提取码处理逻辑<br/>• `src/backend/drivers/*` |
| **海外主流网盘**       | • Google Drive (`google_drive`)<br/>• Microsoft OneDrive (`onedrive`, `onedrive_app`, `onedrive_sharelink`)<br/>• Dropbox (`dropbox`)<br/>• MEGA (`mega`)<br/>• Yandex Disk (`yandex`)<br/>• TeraBox (`terabox`)<br/>• PikPak / PikPak 分享 (`pikpak`, `pikpak_share`)                                                                                                                | • 官方 OAuth2 / REST API 直连<br/>• 多租户客户端 ID / 秘钥自动切换<br/>• `src/backend/drivers/*`                       |
| **标准协议与对象存储** | • Amazon S3 及兼容协议 (`s3`)<br/>• WebDAV 客户端 (`webdav`)<br/>• FTP / SFTP 协议 (`ftp`, `sftp`)<br/>• SMB 局域网共享 (`smb`)<br/>• Seafile 协同存储 (`seafile`)<br/>• GitHub 文件映射 (`github`)                                                                                                                                                                                   | • 边缘流式管道传输，支持大文件边读边发<br/>• `src/backend/drivers/*`                                                   |
| **虚拟与增强驱动**     | • 别名驱动 (`alias`)<br/>• 本地文件驱动 (`local.ts`)<br/>• 媒体音轨增强 (`mediatrack`)<br/>• WPS 云文档 (`wps`)                                                                                                                                                                                                                                                                       | • 路径映射与虚拟重定向层                                                                                               |

---

## 🧩 资产三：原创前端插件系统架构 (Plugin Architecture)

详见完整文档：[docs/plugin-development.md](./plugin-development.md) / [插件.md](../插件.md)。

### 架构创新亮点

1. **零本地编译门槛**：开发者仅需标准原生 JavaScript (ES6+)、CSS 与 SVG 图标。
2. **可视化表单联动**：插件只需在 `plugin.json` 声明 `config_schema`，管理后台即可自动渲染出配置项（支持下拉列表、布尔开关、多行文本输入等）。
3. **运行时安全沙箱与上下文**：
   - 暴露 `OpenListPlugin` 全局 SDK；
   - 规范了 UI 悬浮挂件 (`addFloatingWidget`)、文件列表右键操作项注入、事件总线 (`on/emit`) 与生命周期管理。
4. **一键分发机制**：支持将插件打包为 `.zip`，后台拖拽上传即刻热安装生效，无需重编前端产物。

---

## 🤖 资产四：Model Context Protocol (MCP) 基础设施

代码位置：`src/backend/server/mcp.ts` & `src/backend/internal/mcp/`

- **协议支持**：实现了基于 SSE (Server-Sent Events) 与 JSON-RPC 2.0 的 MCP 标准规范接口。
- **AI Agent 赋能**：将文件列表、存储挂载、文件检索等能力抽象为标准 AI 工具集，供 Claude Desktop、Cursor、Antigravity 等智能体无缝对接调用。
- **权限防护**：内置 `adminAuthMiddleware` 管理员鉴权通道，防止未授权探测与越权调用。

---

## 🛡️ 资产五：安全与用户体验细节优化

1. **分享链接与提取码安全**：
   - 提取密码不在 URL 中明文暴露（通过生成链接时主动同步专用 Cookie 实现安全下载）。
   - 空分享模板自动回退到标准链接，消除静默失败体验。
   - 分享失效/过期页面定制 404 互动动画反馈。
2. **Token 自动保活机制**：
   - 支持 EdgeOne Schedules 与 Cloudflare Cron Triggers，每天自动执行网盘 Token 保活刷新，解决挂载失效问题。
3. **国际化工程链路**：
   - 完整配置 Crowdin 自动化脚本与多语言热切换。

---

## 📦 代码移植与 Cherry-pick 推荐路径索引

官方团队在合并进 `OpenList-Worker` 时，推荐按以下优先级提取与合并：

- [ ] **存储驱动集合**：`src/backend/drivers/*`（可以直接搬迁或补充官方未覆盖的驱动）
- [ ] **插件系统核心**：`src/plugins/` 及 `插件.md` 规范
- [ ] **MCP 路由与工具集**：`src/backend/server/mcp.ts` 与 `src/backend/internal/mcp/`
- [ ] **边缘适配脚本**：`scripts/deploy.js`、`edgeone.json` 与各边缘运行时入口
