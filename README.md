# falix - 4h Check

每 4 小时自动检查一次 Falix 服务器是否在运行。

## 行为

- `schedule: 0 */4 * * *` 每 4 小时运行一次（UTC 00:00/04:00/08:00...）
- 登录后检查 `SERVER_URL` 状态
  - `online` / `starting` → TG 通知无需操作，直接结束
  - `offline` / `unknown` → 点击 Start 并看广告尝试启动，重试 3 轮，TG 通知结果
- 已移除：timer 续期、常驻自触发（`repository_dispatch` + `START/LIMIT` 接力）、每15分钟轮询、`push` 触发
- 手动触发：Actions → Falix Auto Check → Run workflow

## Secrets

```
FALIX_EMAIL
FALIX_PASSWORD
FALIX_SERVER_ID
TG_TOKEN        (可选)
TG_CHAT_ID      (可选)
```

## Keepalive

`.github/workflows/Keepalive.yml` 每 3 天提交一次 `keep-alive.txt`，防止仓库 60 天无活动被禁用 Actions。4h 定时已算活动，此文件可保留或删除。
