# OpenListNext 迁移至 OpenList-Worker 操作指南 (Migration Guide)

随着 **OpenListNext** 正式与官方上游仓库 **[OpenListTeam/OpenList-Worker](https://github.com/OpenListTeam/OpenList-Worker)** 归档合并，所有未来的功能迭代、漏洞修复和网盘驱动更新均已迁往官方仓库。

本指南旨在协助现有 OpenListNext 用户与系统管理员平滑过渡，确保数据完整性与业务连续性。

---

## 一、迁移前核心认知

| 对比维度     | OpenListNext (当前仓库)            | OpenList-Worker (官方主线)                                                      |
| :----------- | :--------------------------------- | :------------------------------------------------------------------------------ |
| **项目定位** | 独立探索分支 / 纯 Web 标准全栈实现 | OpenList 官方边缘运行时标准版本                                                 |
| **代码状态** | 归档（Read-only），不再接受新代码  | 积极维护（Active Maintenance）                                                  |
| **官方仓库** | `Polonium-salts/openlistnext`      | [OpenListTeam/OpenList-Worker](https://github.com/OpenListTeam/OpenList-Worker) |
| **社区支持** | 社区历史存档                       | 官方 Issue、Discussions、Telegram/QQ 交流群                                     |

---

## 二、各部署形态迁移步骤

### 1. Cloudflare Workers 用户

如果您通过 `wrangler` 部署在 Cloudflare Workers 上：

1. **备份现有 KV 数据**：
   在迁移前，建议导出现有的 KV 数据，避免存储挂载与用户配置丢失：
   ```bash
   # 获取当前绑定名 OPENLISTNEXT_KV 的所有键值
   npx wrangler kv:key list --binding OPENLISTNEXT_KV
   ```
2. **切换仓库代码源**：
   ```bash
   git clone https://github.com/OpenListTeam/OpenList-Worker.git
   cd OpenList-Worker
   pnpm install
   ```
3. **继承现有 KV 存储（无损迁移）**：
   - 打开 `OpenList-Worker` 的 `wrangler.toml`。
   - 将原 `OPENLISTNEXT_KV` 绑定的命名空间 ID 填入或保持同名 namespace 绑定。
   - 检查环境变量：如 `JWT_SECRET`、`TOKEN_SECRET` 等保持一致，以防已有登录会话或 Token 失效。
4. **重新部署**：
   ```bash
   pnpm run deploy:worker
   # 或
   pnpm run deploy
   ```

---

### 2. 腾讯云 EdgeOne / 阿里云 ESA 用户

对于中国大陆边缘加速平台用户：

1. **EdgeOne Makers / Pages**：
   - 在腾讯云 EdgeOne 控制台中，将项目关联的 GitHub 仓库从 `openlistnext` 改为指向新仓库的 Fork 或官方仓库。
   - 检查 Pages Blob 存储配置：保持原有环境变量（如 `CRON_SECRET` 等），触发全新构建部署。
2. **阿里云 ESA**：
   - 在 ESA 控制台更新仓库源，保持绑定的 `KV_NAMESPACE` 和 `JWT_SECRET` 不变，直接触发重新拉取构建。

---

### 3. Docker / Node.js 容器化与裸机部署

如果您使用 Node.js 容器并挂载了本地目录：

1. **备份数据卷**：
   ```bash
   # 备份 db.json 与 public_data
   cp -r public_data/ /path/to/backup/public_data/
   ```
2. **更新镜像/代码**：
   - 拉取 `OpenList-Worker` 最新代码或官方构建镜像。
   - 将已备份的 `public_data/`（包含挂载点设置与用户凭据）挂载回新容器相同路径下。
3. **启动验证**：
   检查后台「存储管理」各网盘挂载点是否正常，测试文件上传与直链生成。

---

## 三、常见问题与注意事项 (FAQ)

### Q1: 迁移后原来的挂载网盘需要重新授权吗？

> **大部分无需重新授权**。只要你保留了原来的 KV 存储或 `db.json`，所有的 `refresh_token` 和配置均会被读取。但若驱动涉及授权回调域名变更，请在网盘开放平台将授权 Redirect URI 补全为新域名。

### Q2: 自定义的插件系统如何迁移？

> OpenListNext 原创的插件系统及接口定义已完整移交给官方团队（详见 [HANDOFF_ASSETS.md](./HANDOFF_ASSETS.md)）。随着官方版本吸收该特性，您可以直接在官方后台按相同规范载入插件 ZIP 包。

### Q3: 遇到 Bug 去哪里反馈？

> 请直接前往 [OpenList-Worker Issues](https://github.com/OpenListTeam/OpenList-Worker/issues) 提交反馈。OpenListNext 的维护者们也已作为官方团队的一员在新仓库协助处理。
