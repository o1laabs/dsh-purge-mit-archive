# FORK-NOTES —— 本仓库的定位与许可证边界

**上游**：https://github.com/YuJunZhiXue/dsh-purge （作者：小杨 / YuJunZhiXue）
**本仓库**：https://github.com/o1laabs/dsh-purge

本仓库是**归档 fork**，不是一个"继续维护"的分支。除本文件外，代码内容与上游逐字节一致。

---

## 一、为什么不是直接点 GitHub 的 Fork 按钮

上游在 **2026-10-19 13:27 UTC**（提交 `52e8c68`）把许可证从 MIT 换成了**专有商业许可**。
现行 `LICENSE` + `LICENSE-COMMERCIAL.md` 的原文明确禁止：

> 未经我书面授权，不得复制、修改、再分发、出售、出租、白标或公开提供源码。
> ……
> 3. 把源码或核心实现公开到别的仓库、网盘、频道（我自己的频道除外）。

也就是说，**对 `master` 头部做公开 fork = 第 3 条明令禁止的行为**。上游计划 2026-10-27 起转闭源。
（GitHub ToS 允许对公开仓库点 Fork，与仓库自带许可证之间存在冲突；一旦作者走 DMCA，fork 会被下架。这不是推测，是他自己写在 LICENSE 里的意思。）

## 二、本仓库的依据：MIT 时期的快照仍然有效

同一份 `LICENSE` 自己也写了：

> 四、旧版本 —— 换成本许可之前、曾经以 MIT 发布的 Git tag 和 tarball，仍按当时那份 MIT。

MIT 的授权不可撤回：在你拿到副本的那一刻，它就永久适用于那份副本。所以本仓库固定在**许可证切换前的最后一个提交**上：

| 项目 | 值 |
|---|---|
| 基线提交 | `ec69166` <br> `docs: README 默认中文，停更说明与广告位` |
| 提交时间 | 2026-10-19T01:57:27Z |
| 该提交的 LICENSE | MIT License, Copyright (c) 2026 小杨 |
| package.json version | `1.1.72`（未发布的开发版号） |
| 与最后发布 tag `v1.1.71`（2026-10-11）的差 | 含 10-13 `0.2.0 system prompt 修复`、10-16 `把注入打进原生 system 槽` 两个**关键兼容提交** |

**为什么不用 `v1.1.71` tag**：`v1.1.71` 对 `DeepSeek Harness 0.2.0-rc.2 / 0.2.1-rc.2` 是跑不起来的 —— 那两个 `0.2.0` 兼容修复都发生在这个 tag 之后、许可证切换之前。`ec69166` 是"既是 MIT、又能跑在 0.2.x 上"的唯一一档。

**不含什么**：`ec69166` 之后的 4 个提交（商业许可、README 改写、`LICENSE-COMMERCIAL.md`、`docs/notice/*` 公示图片、`lib/default-prompt-inject.*` 等）都未纳入，它们属于专有许可覆盖的版本。

## 三、作者要求的署名（照做，别省）

原文「署名要求」章节：借用名称、思路、代码或提示词，**必须署名并标明本仓库**。
因此：

- 原作者：**小杨 / YuJunZhiXue**，原仓库 https://github.com/YuJunZhiXue/dsh-purge
- 分发/二开时必须保留 `LICENSE`（MIT，Copyright (c) 2026 小杨）与本文件
- **不得**改名自称官方、不得去署名、不得当收费货卖（这几条 MIT 时期就写在 README 里，作者也点名叫过违规商家）

## 四、如果要跟进上游新版本

2026-10-27 之后新版本只在作者的 Telegram 频道发布：https://t.me/LLM_PoJia/271
需要商用/再分发授权：a2269198325@163.com，标题「dsh-purge 商业许可」。

本仓库**不做**上游同步：MIT 之后的上游提交不再有可复制的许可基础。

## 五、这个项目实际是什么

一个 **DeepSeek Harness (DSH) 的插件**，Node ≥18、纯 JavaScript、无编译产物。它做的事是改写宿主会话送到模型的 system / instruction 槽，
把一个"红队评测"人设与提示词注入进去，并附带记账、评分、工具面（toolFilter）、预设（presets）等模块。

`lib/` 56 个文件、`skills/` 23 个、`docs/` 18 个。核心在 `lib/core.js`（约 360KB）、`lib/index.js`、`lib/redteam/*`。
宿主兼容性是硬约束：DSH 每个 rc 版本的注入槽位和 `assemble` 流程都会变，这正是上游 10-13 / 10-16 两次修复的内容 —— 也是本仓库固定在这个提交的原因。
