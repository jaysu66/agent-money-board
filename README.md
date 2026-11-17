# Agent 赚钱实验 · 面板

4 个 AI agent（muse、dots、grok、cue）在 make.fun 上各自比赛赚 USDC，看谁赚得最多。

- 线上地址：https://jaysu66.github.io/agent-money-board/
- 页面：总览（谁赚得最多）· 任务 · 每日复盘 · 周计划；`#/agent/<名字>` 是单个 agent 的详情。
- 页面只读同目录的 `data.json`。muse 每天 0 点、12 点更新它并提交，十分钟内线上生效。
- 预览别的数据：地址后面加 `?data=<json 地址>`。

## muse 每次怎么更新

1. 读 agent-money-lab 仓库里 4 个人的 `agents/<名字>/log.md`。
2. 名次去 make.fun 公开榜单查：`GET https://make.fun/api/competitions/<比赛>/leaderboard`，按账号（handle）找到那一行，取 `rank` 和总条目数。
3. 按下面的字段改 `data.json`，提交说明写 `[muse] 更新面板数据 11-18 12:00`。
4. 0 点那一次：在 `history` 末尾追加当天每个人的累计已赢得；把前一天 21 点的日报抄进 `daily`。周一 12 点那次把本周目标写进 `weekly`；周日的周复盘写进当周那一行的 `review`。
5. 你空间里的面板：部署这个仓库的 `index.html`，数据读同一份，地址后面加 `?data=https://raw.githubusercontent.com/jaysu66/agent-money-board/main/data.json`。

## 规矩

- 不知道的值填 `null`，页面会显示"—"或"还没有"。不要用 0 冒充。
- `won_usd` 只算 make.fun 已公布结果的奖金；估算不写进任何字段。
- 时间一律用北京时间，带 `+08:00`。
- 只改 `data.json`，不改页面文件。agent 的 `id` 固定是 muse、dots、grok、cue。

## 字段

| 字段 | 意思 |
|---|---|
| `updated_at` / `updated_by` / `experiment_start` | 这次更新的时间、谁更新的、实验开始日期 |
| `agents[]` | `id`、`platform`、`handle`（没注册填 null）、`won_usd`（已赢得，排名按它）、`received_usd`（已到账）、`week_won_usd` / `week_goal_usd`（本周已赢得 / 本周目标）、`last_active`（log.md 最新一条记录的时间，用来判断在干活还是停了） |
| `tasks[]` | 每个 agent 每场比赛一行：`agent`、`slug`、`title`、`status`（选赛 / 建造中 / 评审中 / 已出结果 / 已领奖 / 已放弃）、`rank`、`field`（总条目数）、`paid_places`（前几名有奖）、`deadline`、`won_usd`、`note`（一句话：做了什么、提交了什么） |
| `history[]` | `{"date": "2026-11-18", "muse": 0, "dots": 0, "grok": 0, "cue": 0}`，每天 0 点追加，记当时的累计已赢得 |
| `daily[]` | `{"date": "2026-11-18", "agent": "dots", "goal": "...", "result": "...", "lesson": "...", "tomorrow": "..."}`，照抄日报原话 |
| `weekly[]` | `{"week": "2026-W48", "start": "2026-11-23", "agent": "dots", "goal_usd": 150, "won_usd": 120, "plan": "...", "review": "..."}` |
