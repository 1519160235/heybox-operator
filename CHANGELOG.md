# Changelog

## 二创版本变更记录（heybox-operator）

> 本节记录 heybox-operator（二创自 HannJia/xiaoheihe-operator）的本地定制变更，按时间倒序。上游原版的版本记录见下方。

### 2026-11-03（首批二创改动）

- 修复 `link_tag` bug：纯文字帖从 `21`（非法值，会被小黑盒判「错误的动态分区」拒掉）改为 `28`（默认动态）。正确映射：图文=27、动态=28、文章=11。
- 发帖社区/话题分离：「发帖目标」拆成「发帖社区」下拉（真实社区）+「发帖话题标签」下拉（热门 `#标签`）+ 自定义标签输入，不再是混在一起的游戏列表。
- 浏览去游戏锁：删除「游戏筛选关键词」的硬过滤，浏览/互动范围改由「社区白名单」决定；原字段改为「浏览内容关键词（可选）」，默认空。
- 自动发帖自由发挥：删除写死的 `topic_hint="游戏日常"`，改为人设 + 生活轨迹自由生成内容，不再限定游戏领域。
- 配图直连自建生图 API：新增 `image_generation.py`，直接调 OpenAI 兼容 `/images/generations`；可留空自动读 MaiBot 主配置的「画图」provider，不依赖「麦麦绘图」。
- 生活轨迹联动改造：因生活频率插件无 `get_current_context` API，改为直接读其 `life_state.json` 状态文件。
- 安装生活频率插件 `org.orge-8.life-frequency` v1.25.5 作为生活轨迹来源。

---

## 0.5.0 - 2026-11-01

- Persona now follows the host MaiBot by default: comments, replies and posts use `bot.nickname` and `personality.personality` (the same identity the host replyer uses), re-read at most once a minute so WebUI persona edits and profile switches take effect without a restart. The previous hard-coded studio persona has been removed.
- Added the optional `[content] persona_override` field to give Xiaoheihe a different identity or tone; when the host persona cannot be read and no override is set, a neutral gamer persona is used. Declared the `config.get` capability for this.
- Reworked recommendation-feed browsing: the interval is randomised between 0.5× and 1.5× `feed_interval_seconds`; each session opens 2–4 posts in feed order; comments and likes only go to posts that pass the game filter; the daily comment quota is spread over the remaining sessions; the first session after a restart is delayed by 2–15 minutes.
- Added post likes (`POST /bbs/app/profile/award/link`) with the new `likes_per_day` quota (default 3). Each post is liked at most once and likes do not consume the comment cooldown.
- Browsing and proactive posting pause while the linked life-frequency plugin reports the persona as asleep or napping.
- Rewrote the comment prompts: proactive comments and replies to others use separate instructions, avoid restating the post and avoid stock phrases; output limits raised to 600 tokens for comments and 900 for posts to stop truncation.
- Fixed optional image generation failures so a timed-out drawing task is recorded and the independent Xiaoheihe text-post path can continue.
- Added nested task-payload handling and audit markers for image fallback; no new drawing request is created during fallback.
- Raised the SDK upper bound to 2.99.99; tested with MaiBot 1.3.12 (SDK 2.12.0) and 1.3.19 (SDK 2.20.0).

## 0.4.1 - 2026-10-26

- Fixed the proactive-post send boundary so `dry_run` records the generated draft without image generation, image upload, or a Xiaoheihe `create_post` request.
- Added a regression test covering `dry_run` with image generation enabled.

## 0.4.0 - 2026-10-26

- Added independent server-local time windows for recommendation browsing/active comments and proactive posts.
- Made the three MaiBot integrations optional so text-only Xiaoheihe posting works without them.
- Changed new-install image generation to opt-in instead of requiring the drawing plugin.
- Added the user-provided Xiaoheihe-style local avatar to the manifest.
- Corrected the release documentation boundary to match the published `auto` default.

## 0.3.9 - 2026-10-26

- Changed the new-install recommendation flow interval default to 600 seconds (10 minutes).
- Clarified that 600 seconds is the plugin's recommended default, not an official Xiaoheihe polling interval.
- Set the SDK compatibility upper bound to the tested 2.12.x line.

## 0.3.8 - 2026-10-26

- Changed the run-mode field from free text to a `dry_run` / `auto` dropdown.
- Set the published default to low-quota automatic operation: enabled, `auto`, one post per day, Steam + PC 游戏.
- Kept direct-message automation disabled.

## 0.3.7 - 2026-10-26

- Displayed the installed plugin marketplace names「麦麦绘图」「生活频率」「自我身份档案」in linkage settings.
- Kept plugin IDs and API names beside the Chinese names for troubleshooting and replacement.

## 0.3.6 - 2026-10-26

- Documented the three linked plugins and how to derive their MaiBot API names.
- Clarified that duplicate post targets are accepted in the UI and deduplicated before sending.
- Added the linkage details to the Chinese WebUI hints and README.

## 0.2.9 - 2026-10-25

- Applied MaiBot marketplace review fixes for integration classification and host version bounds.
- Removed unused `api.list` and `config.get` capability declarations.
- Restricted the LLM-facing QR login tools to status and PNG Data URL output; raw login URLs remain internal.
- Added a regression test for QR login output safety.

## 0.2.8 - 2026-10-25

- Consolidated the safe publish boundary for the MaiBot marketplace.
- Added project status and future direct-message option documents.
- Kept direct-message automation out of scope and disabled by default.
- Documented the 60-minute feed interval and the low-quota verification requirement.
- Preserved the existing QR login, feed browsing, post, comment, threaded-reply, image, quota, cooldown, idempotency, and readback safeguards.
