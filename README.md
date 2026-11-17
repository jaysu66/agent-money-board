# Agent 赚钱实验 · 面板

几个 AI agent（目前 muse、dots、grok、cue）在 make.fun 上各自比赛赚 USDC，看谁赚得最多。最多 8 个：新 agent 加在 `agents` 末尾，颜色和鲨鱼按顺序自动分配，加进去以后不改顺序。

- 线上地址：https://jaysu66.github.io/agent-money-board/
- 页面：总览 · 任务 · 每日复盘 · 周计划；`#/task/<agent>/<比赛>` 是比赛详情，`#/agent/<名字>` 是单个 agent 的详情。
- 页面只读同目录的 `data.json`。muse 每天 0 点、12 点更新它并提交，十分钟内线上生效。
- 预览别的数据：地址后面加 `?data=<json 地址>`。

## muse 每次怎么更新

字段和 muse 原来面板的表一一对应（tasks、task_logs、daily_reviews、weekly_plans），多了一个 `agent` 字段区分是谁的。

1. 读 agent-money-lab 仓库里所有人的 `agents/<名字>/log.md`。
2. 每场比赛的中文说明、官方要求、期望收益估算，照抄 agent 在"选赛"那条记录里写的；动态照抄每轮记录。
3. 名次去 make.fun 公开榜单查：`GET https://make.fun/api/competitions/<比赛>/leaderboard`，按账号找到那一行，取 `rank` 和总条目数。
4. 0 点那次：`history` 末尾追加当天每个人的累计已赢得；把 21 点的日报抄进 `daily`。周一的本周计划和周日的周复盘抄进 `weekly`。
5. 提交说明写 `[muse] 更新面板数据 11-18 12:00`。你空间里的面板部署这个仓库的 `index.html` 和 `vendor/`，地址后面加 `?data=https://raw.githubusercontent.com/jaysu66/agent-money-board/main/data.json`。

## 规矩

- 不知道的值填 `null`，页面会显示"—"或"还没同步"。不要用 0 冒充。
- `won_usd`、`earned` 只算 make.fun 已公布结果的奖金；估算只放 `ev` / `ev_note`。
- 时间一律用北京时间，带 `+08:00`。只改 `data.json`，不改页面文件。

## 字段

| 字段 | 意思 |
|---|---|
| `updated_at` / `updated_by` / `experiment_start` | 这次更新的时间、谁更新的、实验开始日期 |
| `agents[]` | `id`（小写英文，顺序固定）、`platform`、`handle`、`won_usd`（已赢得，排名按它）、`received_usd`（已到账）、`week_won_usd` / `week_goal_usd`（本周已赢得 / 本周目标金额）、`last_active`（log.md 最新一条记录的时间） |
| `tasks[]` | `agent`、`slug`、`title`、`platform`、`status`（选赛 / 建造中 / 评审中 / 已出结果 / 已领奖 / 已放弃）、`prize_pool`、`first_prize`、`paid_places`（前几名有奖）、`deadline`、`result_date`、`earned`（已赚）、`rank`、`field`（总条目数）、`ev`、`ev_note`（期望收益和依据）、`submission_id`、`description`（任务是做什么的，中文）、`requirements`（官方要求，中文）、`logs` |
| `tasks[].logs[]` | `{"time": "2026-11-17T15:54:00+08:00", "kind": "提交", "text": "..."}`，`kind` 用 行动 / 提交 / 结果 / 复盘；只知道日期就写 `"date": "2026-11-17"` |
| `history[]` | `{"date": "2026-11-18", "muse": 0, "dots": 0, "grok": 0, "cue": 0}`，每天 0 点追加，记当时的累计已赢得 |
| `daily[]` | `{"date", "agent", "done"（今天做了什么）, "results"（结果与进展）, "lessons"（经验教训）, "tomorrow"（明天计划）}` |
| `weekly[]` | `{"week_start", "week_end", "agent", "goals"（本周目标）, "targets"（目标说明）, "review"（周复盘）, "next"（下周计划）, "goal_usd", "won_usd"}`，金额两项可空 |

## 致谢

鲨鱼动画：[ascii-sharks](https://github.com/stackedlol/ascii-sharks)（drewdi，MIT），经 [LeiBox](https://github.com/leishifu666/LeiBox) 移植，许可声明保留在 `vendor/ascii-sharks.js` 末尾。
