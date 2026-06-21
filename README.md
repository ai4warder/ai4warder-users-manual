# Ai4Warder用户使用指南

本指南面向**Ai4Warder用户**，帮助你完成从注册到接入 Claude Code 的全部流程：

- [Ai4Warder用户使用指南](#ai4warder用户使用指南)
  - [一、注册账号](#一注册账号)
  - [二、登录](#二登录)
  - [三、充值（兑换额度卡）](#三充值兑换额度卡)
  - [四、创建 API Key](#四创建-api-key)
  - [五、配置 Claude Code 工作环境](#五配置-claude-code-工作环境)
    - [macOS / Linux](#macos--linux)
    - [Windows（PowerShell）](#windowspowershell)
    - [安装与启动 Claude Code](#安装与启动-claude-code)
    - [验证配置](#验证配置)
  - [六、安装并使用 ai4warder-cc-statusline 状态栏插件](#六安装并使用-ai4warder-cc-statusline-状态栏插件)
    - [能看到哪些信息](#能看到哪些信息)
    - [前置条件](#前置条件)
    - [安装方式一：Claude Code 插件（推荐）](#安装方式一claude-code-插件推荐)
    - [安装方式二：手动安装](#安装方式二手动安装)
    - [小提示](#小提示)
  - [七、常见问题](#七常见问题)

---

## 一、注册账号

1. 打开浏览器访问：[https://ai4warder.com/wallet/register](https://ai4warder.com/wallet/register)
2. 填写以下信息：
   - **邮箱**：用作登录账号，请使用真实可收信的邮箱
   - **密码**：至少 8 位，并再次输入确认
3. 点击「注册」。系统会向你的邮箱发送一封**验证邮件**。
4. 打开邮件，点击其中的验证链接（或访问 `https://ai4warder.com/wallet/verify-email?token=...`）完成邮箱验证。

![TODO]()

> **注意**
> - 没收到验证邮件？先查看垃圾邮件箱；仍未收到可在登录页使用「重新发送验证邮件」。

---

## 二、登录

1. 访问登录页：[https://ai4warder.com/wallet/login](https://ai4warder.com/wallet/login)
   （直接访问站点首页通常也会自动跳转到登录页）
2. 输入**注册邮箱**和**密码**，点击「登录」。
3. 登录成功后进入**Ai4Warder 用户中心** `/wallet`，包含以下功能页签：
   - **仪表板**：余额、今日成本、累计用量等概览
   - **API Keys**：创建与管理你的密钥
   - **充值管理**：兑换额度卡
   - **账单 / 交易记录**：按日/月查看消费明细，支持导出 CSV

![TODO]()

**忘记密码**：在登录页点击「忘记密码」，输入邮箱后按邮件指引重置即可。

---

## 三、充值（兑换额度卡）

本服务采用**额度卡**（兑换码）方式充值，余额以**美元**（USD）计量，按实际调用成本扣费。

1. 从管理员或销售渠道获取一张**额度卡兑换码**，格式类似：
   `CC-XXXX-XXXX-XXXX-XXXX`
2. 登录后进入Ai4Warder 用户中心，打开「**充值管理**」页签。
3. 在输入框粘贴兑换码，点击「**兑换**」。
4. 兑换成功后页面会显示**本次充值金额**和**充值后新余额**，该金额立即计入你的钱包余额。

![TODO]()

可在「**交易记录**」中查看每一笔充值与扣费历史。

> **说明**：账户余额低于系统阈值时，API 请求会被拒绝。请保持余额充足以保证调用不中断。

---

## 四、创建 API Key

API Key 是你调用中转服务的认证凭证。

1. 进入Ai4Warder 用户中心，打开「**API Keys**」页签，点击「**创建 API Key**」。
2. 填写参数：
   | 参数 | 是否必填 | 说明 |
   |------|---------|------|
   | **名称**（name） | 必填 | 1–64 个字符，便于自己区分用途 |
   | **单日成本限额**（dailyCostLimit） | 可选 | 单位 USD，`0` 或留空表示不限制 |
   | **总成本限额**（totalCostLimit） | 可选 | 单位 USD，`0` 或留空表示不限制 |
3. 点击「创建」。系统会生成一个以 **`af_`** 开头的密钥，例如：
   ```
   af_xxxxxxxxxxxxxxxxxxxxxxxx
   ```

![TODO]()

> ⚠️ **重要：明文密钥只在创建时显示一次，刷新后无法再次查看。请立即复制并妥善保存。** 若遗失，只能删除后重新创建。

**密钥管理**：在「API Keys」列表中你可以
- 启用 / 停用密钥、修改名称与限额
- 删除（软删除，可在「已删除」中恢复）或永久删除
- 查看每个密钥的使用统计与逐请求调用记录

---

## 五、配置 Claude Code 工作环境

拿到 API Key 后，只需配置两个环境变量，把 Claude Code 指向本中转服务即可。

- **服务地址（Base URL）**：`https://ai4warder.com/api`
  （以Ai4Warder 用户中心「使用教程 / 接入说明」页面显示的地址为准）
- **认证令牌（Auth Token）**：你刚创建的 `af_` 开头的 API Key

### macOS / Linux

临时生效（仅当前终端会话）：

```bash
export ANTHROPIC_BASE_URL="https://ai4warder.com/api"
export ANTHROPIC_AUTH_TOKEN="af_xxxxxxxxxxxxxxxxxxxxxxxx"
```

永久生效，写入 `~/.bashrc` 或 `~/.zshrc`：

```bash
echo 'export ANTHROPIC_BASE_URL="https://ai4warder.com/api"' >> ~/.zshrc
echo 'export ANTHROPIC_AUTH_TOKEN="af_xxxxxxxxxxxxxxxxxxxxxxxx"' >> ~/.zshrc
source ~/.zshrc
```

### Windows（PowerShell）

临时生效（仅当前窗口）：

```powershell
$env:ANTHROPIC_BASE_URL = "https://你的服务地址/api"
$env:ANTHROPIC_AUTH_TOKEN = "af_xxxxxxxxxxxxxxxxxxxxxxxx"
```

永久生效（写入用户环境变量，需重启终端）：

```powershell
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", "https://你的服务地址/api", "User")
[System.Environment]::SetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", "af_xxxxxxxxxxxxxxxxxxxxxxxx", "User")
```

### 安装与启动 Claude Code

```bash
# 安装（需 Node.js 环境）
npm install -g @anthropic-ai/claude-code

# 在你的项目目录启动
claude
```

### 验证配置

确认环境变量已生效：

```bash
# macOS / Linux
echo $ANTHROPIC_BASE_URL
echo $ANTHROPIC_AUTH_TOKEN

# Windows PowerShell
echo $env:ANTHROPIC_BASE_URL
echo $env:ANTHROPIC_AUTH_TOKEN
```

只要能输出你配置的地址和密钥，并且 `claude` 能正常对话，即接入成功。后续的调用费用会按实际用量从你的钱包余额中扣除。

---

## 六、安装并使用 ai4warder-cc-statusline 状态栏插件

`ai4warder-cc-statusline` 是一个 Claude Code 状态栏（statusline）插件，在 Claude Code 界面底部实时显示你的用量与钱包余额，方便随时掌握消耗情况，无需登录Ai4Warder 用户中心查看。

### 能看到哪些信息

装好后，Claude Code 底部会显示两行：

- **第一行（本地会话）**：当前模型 · 项目目录 · 本次会话费用 · 已用时长
- **第二行（来自服务端）**：
  - **Upstream Usage**：上游 Claude 账号的配额窗口（5 小时 / 7 天 / sonnet）使用率与重置倒计时
  - **Key Daily Usage**：当前 API Key 今日已用费用 / 日限额
  - **Balance**：你的钱包余额

示例：

```
Sonnet · my-project · $0.35 · 12m12s
Upstream Usage: 5h 16% (3h29m), 7d 43% (3d), sonnet 21% (3d); Key Daily Usage: $14.85/$250; Balance: $99.05
```

![TODO]()

### 前置条件

- 已按「第五章」配置好 `ANTHROPIC_BASE_URL` 与 `ANTHROPIC_AUTH_TOKEN`，且 Claude Code 能正常对话。
- 本机已安装 Node.js（安装 Claude Code 时已要求）。

### 安装方式一：Claude Code 插件（推荐）

在 Claude Code 里依次执行：

```
/plugin marketplace add ai4warder/ai4warder-cc-statusline
/plugin install ai4warder-cc-statusline@ai4warder-marketplace
/reload-plugins
/ai4warder-cc-statusline:setup
```

`setup` 会自动下载脚本并写入状态栏配置。完成后重启 Claude Code 即可生效。

### 安装方式二：手动安装

下载脚本：

```bash
mkdir -p ~/.claude
curl -fsSL -o ~/.claude/ai4warder-cc-statusline.js \
  https://raw.githubusercontent.com/ai4warder/ai4warder-cc-statusline/main/ai4warder-cc-statusline.js
```

在 `~/.claude/settings.json` 中加入（或更新）`statusLine` 字段：

```json
{
  "statusLine": {
    "type": "command",
    "command": "node ~/.claude/ai4warder-cc-statusline.js"
  }
}
```

重启 Claude Code 生效。Windows 用户路径同为用户目录下的 `.claude`（如 `C:\Users\<你的用户名>\.claude`），用 PowerShell 下载脚本并编辑同目录的 `settings.json` 即可。

### 小提示

- 余额、当日用量等数据由服务端按你的 API Key 实时返回，本地约 10 秒刷新一次。
- 若状态栏底部只显示第一行或显示 `Claude —`，请确认 `ANTHROPIC_BASE_URL` / `ANTHROPIC_AUTH_TOKEN` 已生效，并重启 Claude Code。

---

## 七、常见问题

| 现象 | 排查方向 |
|------|----------|
| 收不到验证 / 重置邮件 | 检查垃圾邮件箱；在登录页重新发送验证邮件 |
| 充值兑换失败 | 确认兑换码完整无误、未被使用、未过期 |
| 创建 API Key 后找不到明文 | 明文仅显示一次，请删除后重新创建 |
| 调用报「余额不足 / 被拒绝」 | 钱包余额过低，请充值后重试 |
| 调用报认证失败（401） | 检查 `ANTHROPIC_AUTH_TOKEN` 是否正确、密钥是否被停用 |
| 调用无法连接 | 检查 `ANTHROPIC_BASE_URL` 是否为 `https://你的服务地址/api`，注意结尾路径 |
| 单日 / 总额超限 | 密钥触发了你设置的成本限额，可在「API Keys」中调整限额 |

---

如有疑问，请联系服务管理员。
