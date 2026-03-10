---
name: eleme-order
description: Order food delivery from Ele.me (饿了么) using browser automation. The agent controls a real browser via Playwright MCP to browse restaurants, add items to cart, and submit orders. User handles payment manually. Use when user wants to order food, drinks, or groceries from Ele.me.
metadata:
  {
    "openclaw":
      {
        "emoji": "🍜",
        "requires": { "anyBins": ["npx"], "mcpServers": ["playwright"] },
      },
  }
---

# 饿了么外卖点餐（浏览器自动化）

通过 Playwright 浏览器自动化操控饿了么 H5 页面，帮用户点外卖。

## 前置条件

需要在 openclaw.json 中配置 Playwright MCP Server：

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

## 操作流程

### 步骤1：打开饿了么并登录

```
使用 Playwright MCP 的 browser_navigate 工具打开 https://h5.ele.me
```

- 首次使用会跳转到登录页，需要用户提供手机号
- 输入手机号 → 获取验证码 → 用户告知验证码 → 登录
- 登录成功后会显示首页商家列表

### 步骤2：浏览商家

- 登录后首页会显示"为你推荐附近的商家"列表
- 如果页面报错"出错了"，点击"重新加载"
- 向用户展示附近商家信息（店名、评分、月售、配送时间、优惠活动）
- 用户可以指定想吃什么，你来推荐或搜索

### 步骤3：进入店铺选菜

- 点击商家进入店铺详情页
- 页面会显示菜单分类（左侧）和商品列表（右侧）
- 向用户展示菜品名称、价格、月售、好评率
- 用户告知想点什么，点击"选规格"或"加购"按钮

### 步骤4：选择规格

- 弹出规格选择框（大小杯、冰度、甜度等）
- 告知用户当前默认规格，询问是否调整
- 确认后点击"加入购物车"

### 步骤5：凑单（如需）

- 如果未达起送价，页面会提示"还差 X 元起送"
- 点击"凑单"按钮查看推荐商品
- 向用户展示选项，加购凑够起送价

### 步骤6：结算

- 点击"去结算"进入确认订单页
- 展示订单详情：商品、打包费、配送费、优惠、合计金额、预计送达时间、收货地址
- 用户确认后点击"提交订单"

### 步骤7：支付（用户手动）

- 提交订单后会跳转支付页面
- **告知用户需要手动完成支付**（输入支付密码/指纹等）
- 不要代替用户输入支付密码

## 关键注意事项

1. **每一步都要用 browser_snapshot 查看页面状态**，不要盲目操作
2. **向用户展示关键信息**再操作，不要替用户做决定（选什么菜、什么规格）
3. **支付环节必须用户手动完成**，绝不代输密码
4. **定位问题**：H5 页面定位可能不准，如果商家列表加载失败，点"重新加载"
5. **页面结构可能变化**：始终依赖 snapshot 而非硬编码选择器
6. **耐心等待页面加载**：饿了么 H5 页面加载较慢，操作之间适当等待
