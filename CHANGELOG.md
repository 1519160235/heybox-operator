# Changelog

本文件记录 heybox-operator（二次开发自 [HannJia/xiaoheihe-operator](https://github.com/HannJia/xiaoheihe-operator)）的变更，按时间倒序。

## 2026-11-03（首批二创改动）

- 修复 `link_tag` bug：纯文字帖从 `21`（非法值，会被小黑盒判「错误的动态分区」拒掉）改为 `28`（默认动态）。正确映射：图文=27、动态=28、文章=11。
- 发帖社区/话题分离：「发帖目标」拆成「发帖社区」下拉（真实社区）+「发帖话题标签」下拉（热门 `#标签`）+ 自定义标签输入，不再是混在一起的游戏列表。
- 浏览去游戏锁：删除「游戏筛选关键词」的硬过滤，浏览/互动范围改由「社区白名单」决定；原字段改为「浏览内容关键词（可选）」，默认空。
- 自动发帖自由发挥：删除写死的 `topic_hint="游戏日常"`，改为人设 + 生活轨迹自由生成内容，不再限定游戏领域。
- 配图直连自建生图 API：新增 `image_generation.py`，直接调 OpenAI 兼容 `/images/generations`；可留空自动读 MaiBot 主配置的「画图」provider，不依赖「麦麦绘图」。
- 生活轨迹联动改造：因生活频率插件无 `get_current_context` API，改为直接读其 `life_state.json` 状态文件。
- 安装生活频率插件 `org.orge-8.life-frequency` v1.25.5 作为生活轨迹来源。
