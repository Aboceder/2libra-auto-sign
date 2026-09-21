# 2libra 自动签到

这是一个面向 2libra 用户的 GitHub Actions 自动签到项目，支持：

- 每日自动签到
- 多账号配置
- 手动触发运行
- 自动徽章处理
  - 赌狗专属：如果监测到用户佩戴了“捣蛋鬼徽章”，签到之前会自动卸下徽章，防止签到触发捣蛋鬼效果，签到后，等待徽章冷却时间，自动佩戴，继续开启赌狗模式

## 快速开始

### 1. Fork 本仓库

将此仓库 Fork 到你的 GitHub 账号。

### 2. 配置账号信息

进入你 Fork 后的仓库：

`Settings` → `Secrets and variables` → `Actions` → `New repository secret`

新增 Secret：

| Secret 名称 | 值 |
|---|---|
| `LIBRA_ACCOUNTS_JSON` | 账号配置 JSON |

示例：

```json
[
  {
    "label": "主账号",
    "username": "your-email@example.com",
    "password": "your-password",
    "enabled": true
  },
  {
    "label": "Token账号",
    "accessToken": "your-access-token",
    "enabled": true
  }
]
```

字段说明：

- `label`：账号备注名称（必填）
- `accessToken`：已有的访问令牌（可选）。配置后会跳过用户名密码登录，直接使用该 token 调用接口
- `username`：登录邮箱；未配置 `accessToken` 时必填
- `password`：登录密码；未配置 `accessToken` 时必填
- `enabled`：是否启用该账号（可选，默认 `true`）

每个账号需要选择一种认证方式：配置 `accessToken`，或同时配置 `username` 和 `password`。如果两者都配置，将优先使用 `accessToken`。访问令牌属于敏感信息，请通过 GitHub Actions Secret 保存，不要提交到仓库。

支持配置多个账号，脚本会按配置逐个处理。

### 3. 自动运行时间

默认每天执行一次：**UTC 17:00（北京时间 01:00）**。

## 手动触发

进入仓库：

`Actions` → `2libra Auto Sign` → `Run workflow`

可选参数说明：

- `account`：按 `label` 或 `username` 指定单账号运行，留空表示运行全部账号
- `dry_run`：仅检查，不执行实际签到
- `badge_restore_wait_minutes`：徽章功能等待分钟数（默认 11）

## 常见问题

**Q: 如何临时禁用某个账号？**  
A: 把该账号的 `enabled` 设为 `false`，然后更新 `LIBRA_ACCOUNTS_JSON`。

**Q: 配置改了什么时候生效？**  
A: 下一次运行（自动或手动触发）就会生效。

**Q: 运行失败怎么办？**  
A: 在 `Actions` 页面查看该次运行日志，按报错提示检查账号配置。
