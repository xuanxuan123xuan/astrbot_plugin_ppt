

# 课件自动总结插件

AstrBot 插件。接收 PPTX / PDF / DOCX 课件文件后，自动提取文本内容并由机器人已配置的 LLM 生成结构化总结。

## 功能

| 功能 | 说明 |
|------|------|
| 自动总结 | 向机器人发送课件文件，自动生成：核心主题、关键知识点、结构大纲、一句话总结 |
| `/kw` | 本地 jieba 关键词提取（Top 15），不消耗 LLM Token |
| `/summarize` | 手动触发最近一份课件的重新总结 |

## 安装

### 1. 放入插件目录

```bash
cd AstrBot/data/plugins
git clone https://github.com/xuanxuan123xuan/astrbot_plugin_ppt.git
```

### 2. 安装依赖

```bash
pip install -r AstrBot/data/plugins/astrbot_plugin_ppt/requirements.txt
```

或在 AstrBot WebUI → 插件管理 → 找到插件 → 点击安装依赖。

### 3. 启用插件

在 AstrBot WebUI → 插件管理中启用本插件。

## 使用

1. 在 QQ / 微信 / Telegram 等平台向机器人**发送课件文件**（.pptx / .pdf / .docx）
2. 机器人自动提取文本并生成总结回复

## 架构说明

```
用户发送文件
    │
    ▼
on_receive_msg (event_message_type=ALL)
    │ 检测 Comp.File → 按扩展名分派提取函数
    │ PPTX: python-pptx
    │ PDF:  PyMuPDF (fitz)
    │ DOCX: python-docx
    │
    ▼
提取文本 → 存入 _pending 队列
    │
    ▼
on_llm_request (钩子)
    │ 将课件文本 + 总结指令追加到 req.prompt
    │
    ▼
AstrBot LLM 生成总结 → 回复用户
```

## 平台支持

所有 AstrBot 已适配的消息平台（QQ / Telegram / 微信 / 飞书 / 钉钉 等）
