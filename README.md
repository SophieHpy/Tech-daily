# Tech Daily · 每日科技推送

每天早上把科技热点推送到你的手机：聚合 Hacker News、GitHub Trending、Product Hunt、IT之家、少数派、掘金、V2EX、知乎、华尔街见闻等 **12 个热榜** + **10 个优质 RSS 源**，按关键词分组（AI/芯片/开源/机器人/自动驾驶/商业动态…），可选 AI 总结「今日值得关注的方向」。

零成本：GitHub Actions 每天定时运行，无需服务器。

> 基于 [sansan0/TrendRadar](https://github.com/sansan0/TrendRadar) v6.10 改造，保留 GPL-3.0 协议，感谢原作者。

---

## 快速开始（3 步）

### 1️⃣ 建仓库
在 GitHub 创建一个仓库（如 `tech-daily`），把本项目代码推上去。公开/私有均可：公开仓库 Actions 免费用量无限；私有仓库免费额度 2000 分钟/月（本项目每天约用 2 分钟，也够用）。

### 2️⃣ 配推送渠道（任选其一）
仓库页面 → **Settings → Secrets and variables → Actions → New repository secret**：

| 渠道 | Secret 名称 | 怎么获取 |
|---|---|---|
| **Bark**（iOS，最省事） | `BARK_URL` | App Store 装 Bark → 打开 App 复制 URL（形如 `https://api.day.app/xxxxx`） |
| **ntfy**（iOS/Android） | `NTFY_TOPIC` | 装 ntfy App → 点 + 订阅一个自己起的英文 topic 名（如 `my-tech-daily-x7k2`），secret 就填这个名字 |
| **Telegram** | `TELEGRAM_BOT_TOKEN` + `TELEGRAM_CHAT_ID` | 找 @BotFather 建 bot 拿 token；给自己发条消息后访问 `https://api.telegram.org/bot<token>/getUpdates` 拿 chat_id |
| **企业微信** | `WEWORK_WEBHOOK_URL` | 企业微信群 → 添加群机器人 → 复制 webhook |
| **Server酱**（微信里收） | `GENERIC_WEBHOOK_URL` + `GENERIC_WEBHOOK_TEMPLATE` | 见下方说明 |
| 邮件 / 飞书 / 钉钉 / Slack | 见 `config/config.yaml` 通知段 | 对应服务后台生成 webhook |

<details><summary>Server酱 / PushPlus 配置说明</summary>

- Server酱 Turbo：`GENERIC_WEBHOOK_URL = https://sctapi.ftqq.com/<你的SendKey>.send`，`GENERIC_WEBHOOK_TEMPLATE = {"title": "{title}", "desp": "{content}"}`
- PushPlus：`GENERIC_WEBHOOK_URL = http://www.pushplus.plus/send/<你的token>`，`GENERIC_WEBHOOK_TEMPLATE = {"title": "{title}", "content": "{content}"}`
</details>

### 3️⃣ 验证推送
仓库 → **Actions → Tech Daily · 每日科技推送 → Run workflow**，一分钟后手机应收到推送。之后每天北京时间 08:00 自动推送（改时间编辑 `.github/workflows/crawler.yml` 的 cron，UTC 时间）。

---

## 推送内容长这样

- **AI 大模型与 Agent**｜AI 基建与芯片｜开发者与开源｜新产品与消费科技｜机器人与硬件｜自动驾驶与汽车｜公司与商业动态（融资/上市/新业务方向）｜安全与隐私｜前沿科技｜政策与监管
- **独立展示区**：GitHub 今日热榜 + Show HN + 岗位源全量（不经过滤——新产品、新方向、新岗位一网打尽）
- **每日岗位**：V2EX 酷工作 / HN Who's Hiring / WeWorkRemotely 后端+DevOps，按「AI/LLM/Agent、低空/机器人、后端/Infra、远程/实习」画像筛选；新闻源里的招聘信号（校招/内推等）单独成组
- 每条带来源和链接，点进去看原文

## 想调整看什么？

| 想改 | 文件 |
|---|---|
| 关注的关键词/分组 | `config/frequency_words.txt` |
| 增减热榜平台 | `config/config.yaml` → `platforms.sources` |
| 增减 RSS 源 | `config/config.yaml` → `rss.feeds` |
| 推送时间 | `.github/workflows/crawler.yml` → `cron` |
| 每组最多显示几条 | `config.yaml` → `report.max_news_per_keyword` |

## 可选：AI 每日总结（精读/速览分层）+ 英文标题翻译

配好后推送开头会多出 AI 分析板块，包含：**今日总览**（3-5 句主线定性）、**值得细看**（精读清单 ≤8 条，每条附理由）、**简讯即可**（速览清单 ≤12 条）、**RSS 增量**、**方向信号**（新机会/新赛道标注，指向你的主线）、**岗位源摘要**。

需要三个 secret：

| Secret | 值 |
|---|---|
| `AI_API_KEY` | 你的模型 API key |
| `AI_ANALYSIS_ENABLED` | `true` |
| `AI_MODEL` | 模型名（可选，默认 `deepseek/deepseek-v4-flash`） |

**免费方案（推荐先试这个）**：注册 [智谱开放平台](https://open.bigmodel.cn/) 拿 GLM-4-Flash 免费 key，然后 `AI_MODEL = openai/glm-4-flash`，`AI_API_BASE = https://open.bigmodel.cn/api/paas/v4`。每天一次的用量免费额度完全够。
**付费方案**：DeepSeek（约几分钱一天）`AI_MODEL = deepseek/deepseek-v4-flash`，或任何 OpenAI 兼容服务。

另设 `AI_TRANSLATION_ENABLED = true` 可把英文标题翻译成中文。不配 AI 也完全能用。

## 本地调试（可选）

```bash
uv sync && NTFY_TOPIC=你的topic名 uv run python -m trendradar
```

---

*License: GPL-3.0 · 上游项目: https://github.com/sansan0/TrendRadar · 热榜数据源: https://github.com/ourongxing/newsnow*
