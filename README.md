# 羽签 AI 2.0 · 固定体验入口

校园运动预约 · 智能提醒助手 —— 第一次配置，以后通过截图或一句话管理预约提醒。

**正式入口：** https://longyanwang4-cell.github.io/yuqian-ai2-entry/

> 本仓库**只有静态入口页**：不含后端源码、数据库、密钥、邀请码、Cookie 或任何用户数据。
> 真正的 OCR 识别、对话与预约提醒服务运行在演示电脑上，通过 HTTPS 隧道对外提供。

## 当前状态（2026-10-22 03:45 实测）

| 项目 | 状态 |
|---|---|
| GitHub Pages | 已启用，Source = `Deploy from a branch` / `main` / `/ (root)` |
| 站点地址 | https://longyanwang4-cell.github.io/yuqian-ai2-entry/ （HTTP 301 → HTTPS，证书 `*.github.io`） |
| `index.html` | 200，根路径与 `/index.html` 均可访问 |
| `experienceUrl` | **空** —— 尚未有经公网验证的后端 HTTPS 地址，入口按钮置灰显示「体验服务暂不可用」 |

> 说明：`wly-legion.tailfde657.ts.net` 属于 Tailscale 组网域名，仅 tailnet 内的设备可达，
> 不能作为对外体验地址，因此未写入 `experienceUrl`。


## 页面结构

| 区块 | 说明 |
|---|---|
| 立即体验羽签 | 主按钮；服务不可用时置灰并显示「羽签体验服务暂时不可用，请稍后再试。」 |
| 了解羽签 | 产品能力介绍（OCR / 规则校验 / 一句话预约 / 长期记忆 / 多用户隔离） |
| 使用说明 | 首次配置与日常使用的四步流程 + 提醒能力真实边界说明 |
| 当前服务状态 | 加载时对后端 `/api/health` 做一次连通性预检并展示结果 |

## 文件

| 文件 | 作用 |
|---|---|
| `index.html` | 品牌页、入口按钮、服务状态（单文件，无外部依赖、无统计脚本） |
| `config.json` | 唯一需要维护的配置：`experienceUrl`（经验证的公网 HTTPS 地址） |
| `.nojekyll` | 关闭 Jekyll，保证文件按原样发布 |

## 更换后端体验地址（二维码不变）

1. 修改 `config.json` 的 `experienceUrl` 为新地址（必须 `https://`）。
2. 提交并推送 `main`。
3. 等待 Pages 重新发布（约 1 分钟），手机复测。

二维码指向本页固定地址，因此更换后端地址**无需重印二维码**。

如需完全停用体验入口：把 `experienceUrl` 清空即可，页面会显示「羽签体验服务暂时不可用，请稍后再试。」，不会跳转到任何假页面或本机地址。

## 安全边界

- 页面无 Cookie、无用户数据、不引用第三方脚本或统计代码。
- `experienceUrl` 强制 `https://`，并拒绝 `localhost`、`127.0.0.1`、`::1`、`10.`、`192.168.`、`172.16-31.`、`169.254.`、`.local`、`.internal`、`.lan`、`.ts.net`、`.tailscale.net`、`.ngrok-free.app` 等本机、私有网段与组网/内网穿透域名。
- 后端地址未配置或不合法时，按钮保持禁用状态，不跳转；连通性预检超时同样按不可用处理。
- 连通性预检使用 `mode: "no-cors"` + `credentials: "omit"`，只判断请求是否可达，不读取任何响应内容。
- 页面不伪造 OCR、聊天、预约创建或提醒结果；所有结果均来自真实后端。

## 回滚

```bash
git revert <commit>
git push
```

或把 `experienceUrl` 改回上一个已验证地址。
