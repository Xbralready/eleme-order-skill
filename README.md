# eleme-order - 饿了么外卖点餐 OpenClaw Skill

通过 Playwright 浏览器自动化，让 OpenClaw AI 助手帮你在饿了么上点外卖。

## 功能

- 打开饿了么 H5 页面，浏览附近商家
- 进店查看菜单，按需选择菜品和规格
- 自动加购、凑单、提交订单
- 支付环节提醒用户手动完成

## 安装

### 1. 复制 skill 文件

```bash
cp -r eleme-order ~/.openclaw/skills/
```

### 2. 配置 Playwright MCP（在 openclaw.json 中添加）

```json
{
  "plugins": {
    "entries": {
      "acpx": {
        "config": {
          "mcpServers": {
            "playwright": {
              "command": "npx",
              "args": ["@playwright/mcp@latest"]
            }
          }
        }
      }
    }
  }
}
```

### 3. 启用 skill（在 openclaw.json 的 skills.entries 中添加）

```json
{
  "skills": {
    "entries": {
      "eleme-order": {
        "enabled": true
      }
    }
  }
}
```

### 4. 重启 OpenClaw gateway

```bash
openclaw gateway restart
```

## 使用

在 Telegram 或其他渠道对 bot 说：

- "帮我点杯霸王茶姬的伯牙绝弦"
- "我想点外卖，附近有什么好吃的"
- "帮我在饿了么上点一份黄焖鸡"

Bot 会自动打开浏览器操作饿了么页面，引导你完成点餐。

## 注意事项

- 首次使用需要手机验证码登录饿了么
- 支付环节需要你手动完成（安全考虑）
- 需要本机安装 Node.js（用于 npx 运行 Playwright MCP）
- 饿了么 H5 页面定位基于浏览器，可能不完全准确
