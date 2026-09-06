# 致 OpenListTeam 官方团队：OpenListNext 归档合并与共建提案函

# Proposal: Merging OpenListNext into OpenList-Worker & Unified Co-development

> 本文档为 OpenListNext 主创团队提交给 **[OpenListTeam/OpenList-Worker](https://github.com/OpenListTeam/OpenList-Worker)** 官方管理团队的正式交接与合并提案。可直接复制用于官方 Issue / Discussion / 邮件沟通。

---

## 🇨🇳 中文提案版本 (Chinese Version)

**主题：【合并与共建提案】OpenListNext 正式归档并入 OpenList-Worker：资产交接、历史提交保留与协作机制建议**

尊敬的 OpenListTeam 官方团队成员：

你们好！

我是 **OpenListNext** 的创建者与主维护者（GitHub: [@Polonium-salts](https://github.com/Polonium-salts)）。

长期以来，OpenList 系列以其卓越的设计与体验赢得了大量开发者与用户的喜爱。为了让整个 OpenList 边缘轻量化生态力量聚合、避免社区力量割裂，经过慎重考虑与团队讨论，**我们已正式将 OpenListNext 设为归档状态，并全量引导用户与开发者迁移至官方上游的 [OpenListTeam/OpenList-Worker](https://github.com/OpenListTeam/OpenList-Worker)**。

在过去的发展中，OpenListNext 沉淀了大量纯 Web 标准边缘架构成果、34 款网盘驱动以及原创的插件生态规范。为了让这笔技术财富平滑融入官方主线，同时保障原贡献团队的历史荣誉与持续贡献动力，我们诚挚提出以下合并交接提案：

### 一、交接的核心技术资产

我们已将完整的资产清单整理在 [HANDOFF_ASSETS.md](https://github.com/Polonium-salts/openlistnext/blob/main/docs/HANDOFF_ASSETS.md) 中，主要包括：

1. **34 款全栈纯 Web 标准网盘驱动**（覆盖 115、123、夸克、阿里、百度、Google Drive、OneDrive、S3、WebDAV、SMB 等，无任何 Node.js 原生依赖）。
2. **多边缘运行时适配方案**（Cloudflare Workers、腾讯云 EdgeOne、阿里云 ESA、Vercel Serverless）。
3. **首创的前端插件规范与运行时 SDK**（零构建门槛、后台配置 Schema 自动渲染、ZIP 拖拽热载入）。
4. **AI MCP (Model Context Protocol) 接口实现**（标准化 AI Agent 云盘工具协议）。

### 二、关于 Git 提交历史与贡献者荣誉的诉求（关键）

OpenListNext 历经 **300+ 次 Commits**，凝聚了以 @Polonium-salts、@BAJJDY、@wumingzhinu、@Dummysky06 等为代表的 **10 位核心贡献者** 的心血。
为了尊重开源社区的贡献者精神与 LICENSE (AGPL-3.0) 法律合规，**我们恳切希望：**

1. **保留 Git 提交树历史**：在将代码合并进 `OpenList-Worker` 时，**请避免使用 Squash & Merge（直接压缩成单一提交）或直接复制代码文件**，建议采用 `git subtree add` 或 `git merge --allow-unrelated-histories` 方式合入，确保贡献者图谱（Insights / Contributors）与每次提交的 Blame 记录完整保留。
2. **鸣谢与版权保留**：在 `OpenList-Worker` 的 `README.md` 致谢章节（Acknowledgements / Special Thanks）中加入原 OpenListNext 项目及其贡献者名单；在 LICENSE 或代码头部保留 `Copyright (c) 2024-2026 OpenListNext Contributors`。

### 三、加入 OpenList-Worker 核心共建与权限申请

为了保障合并后相关特性（尤其是多平台边缘适配、插件系统与网盘驱动）能够持续迭代与维护：

1. 申请将原项目核心维护者（如 [@Polonium-salts](https://github.com/Polonium-salts)）吸纳为 **OpenListTeam 组织成员**，并授予 `OpenList-Worker` 仓库的 **Maintainer（或至少 Write / Triage）权限**；
2. 担任边缘适配层与插件模块的 **Reviewer / Codeowner**，共同参与日常 PR Review 与 Issue 解答，共同将 OpenList-Worker 打造成最顶级的文件列表边缘解决方案！

期待官方团队的回复与进一步交流！如需视频会议或即时通讯沟通（如 Telegram / 微信），我随时可以配合。

再次感谢 OpenListTeam 为开源社区所做出的卓越贡献！

**OpenListNext 团队 / @Polonium-salts 敬上**

---

## 🌐 英文提案版本 (English Version)

**Subject: [Proposal] Archiving & Merging OpenListNext into OpenList-Worker: Asset Handover, Git History Preservation & Future Collaboration**

Dear OpenListTeam Core Members,

Greetings!

I am the creator and lead maintainer of **OpenListNext** ([@Polonium-salts](https://github.com/Polonium-salts)).

To consolidate community efforts and bring the best lightweight serverless experience into the official ecosystem, we have officially decided to **archive `openlistnext` and direct all our users and contributors to the upstream [OpenListTeam/OpenList-Worker](https://github.com/OpenListTeam/OpenList-Worker)**.

Throughout our journey, OpenListNext has accumulated rich technical assets, including 34 pure Web-standard storage drivers, multi-edge runtime adapters, an original plugin system architecture, and MCP (Model Context Protocol) capabilities. We are eager to contribute all of these to `OpenList-Worker`.

### 1. Key Technical Assets Handed Over

A comprehensive inventory is available in [docs/HANDOFF_ASSETS.md](https://github.com/Polonium-salts/openlistnext/blob/main/docs/HANDOFF_ASSETS.md):

- **34 Pure Web-standard Storage Drivers**: Built without Node.js built-in modules (`fetch`, `ReadableStream`, `Web Crypto`).
- **Multi-Edge Runtime Support**: Cloudflare Workers, Tencent EdgeOne, Alibaba ESA, Vercel, Docker.
- **Original Front-end Plugin Architecture**: Zero-build runtime SDK, dynamic JSON config schema form generation, ZIP hot-loading.
- **AI Model Context Protocol (MCP)**: Tool standardization for AI agents.

### 2. Safeguarding Git History & Contributor Recognition (Key Request)

OpenListNext contains **over 300 commits** and contributions from **10 core developers** (including @Polonium-salts, @BAJJDY, @wumingzhinu, @Dummysky06, etc.).
In alignment with open-source ethics and AGPL-3.0 compliance:

1. **Preserve Commit History**: Please **avoid squash-merging or direct copy-pasting** which eliminates past commit attribution. We suggest using `git subtree` or `git merge --allow-unrelated-histories` so all original commits and contributors appear in `OpenList-Worker`'s contributor insights.
2. **Credits & Attribution**: Please include OpenListNext and its contributors in the `README.md` Acknowledgements section, and retain `Copyright (c) 2024-2026 OpenListNext Contributors` in the license notices.

### 3. Maintainer Role & Ongoing Co-development

To ensure smooth integration and maintain the edge runtime & plugin modules:

- We request that [@Polonium-salts](https://github.com/Polonium-salts) be invited to the **OpenListTeam organization** with **Maintainer (or Write/Reviewer) permissions** on `OpenList-Worker`.
- We would be honored to serve as Codeowners/Reviewers for edge runtime adapters, storage drivers, and the plugin subsystem.

We look forward to collaborating closely with you and making `OpenList-Worker` the undisputed leader in edge-native file management!

Warm regards,  
**Polonium-salts & The OpenListNext Contributors**
