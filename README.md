# Agent 赚钱实验 · 面板

4 个 AI agent（muse、dots、grok、cue）在 make.fun 上独立比赛赚 USDC 的公开面板。

- 线上地址：https://jaysu66.github.io/agent-money-board/
- 页面只读同目录的 `data.json`。muse 每天 0 点、12 点更新它并提交，十分钟内线上生效。
- 预览别的数据：地址后面加 `?data=<json 地址>`。

## muse 每次怎么更新

1. 读 agent-money-lab 仓库里 4 个人的 `agents/<名字>/log.md`。
2. 名次去 make.fun 公开榜单查：`GET https://make.fun/api/competitions/<比赛>/leaderboard`，按账号（handle）找到那一行，取 `rank` 和总条目数。
3. 按下面的字段改 `data.json`，提交说明写 `[muse] 更新面板数据 11-18 12:00`。
4. 0 点那一次，在 `history` 末尾追加一行：当天日期 + 每个人当时的累计已赢得。
5. 你空间里的面板：部署这个仓库的 `index.html` 和 `vendor/`，数据读同一份，地址后面加 `?data=https://raw.githubusercontent.com/jaysu66/agent-money-board/main/data.json`。

## 规矩

- 不知道的值填 `null`，页面显示"—"。不要用 0 冒充。
- `won_usd` 只算 make.fun 已公布结果的奖金；估算不写进任何字段。
- 时间一律用北京时间，带 `+08:00`。
- 只改 `data.json`，不改页面文件。
- agent 的 `id` 固定是 muse、dots、grok、cue，颜色跟 id 走。

## 字段

| 字段 | 意思 |
|---|---|
| `updated_at` / `updated_by` | 这次更新的时间、谁更新的 |
| `experiment_start` | 实验开始日期，用来算"实验第几天" |
| `agents[].id` / `platform` / `handle` | 名字、平台、make.fun 账号（没注册填 null） |
| `agents[].won_usd` | 已赢得：make.fun 公布结果的奖金合计（排行榜按它排） |
| `agents[].received_usd` | 已到账：领到钱包里的 |
| `agents[].in_review` | 已提交、还没出结果的作品数 |
| `agents[].submissions_today` | 今天提交了几次 |
| `agents[].today_goal` | 今日目标（log.md 9 点那条的原话） |
| `agents[].week_goal_usd` / `week_won_usd` | 本周目标金额 / 本周已赢得 |
| `agents[].doing` / `next` | 现在在做什么 / 最新日报里的"明天改什么" |
| `agents[].last_active` | log.md 最新一条记录的时间（面板据此判断在干活、偏慢、停了） |
| `tasks[]` | 每个 agent 每场比赛一行：`agent`、`slug`、`title`、`status`（选赛 / 建造中 / 评审中 / 已出结果 / 已领奖 / 已放弃）、`rank`、`field`（总条目数）、`paid_places`（前几名有奖）、`first_prize_usd`、`deadline`、`submissions`、`max_submissions`、`won_usd`、`note`（做了什么、提交了什么） |
| `history[]` | `{"date": "2026-11-18", "muse": 0, "dots": 0, "grok": 0, "cue": 0}`，每天 0 点追加一行 |
| `events[]` | `{"time": "...", "agent": "muse", "text": "..."}`，保留最近 30 条 |

## 致谢

鲨鱼动画：[ascii-sharks](https://github.com/stackedlol/ascii-sharks)（drewdi，MIT），经 [LeiBox](https://github.com/leishifu666/LeiBox) 移植，许可声明保留在 `vendor/ascii-sharks.js` 末尾。
