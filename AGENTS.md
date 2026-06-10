# AGENTS.md

本仓库维护一个在 DeepSeek 网页端运行的金庸群侠传式文字游戏提示词和触发器资料库。

## 项目目标

- `game_engine_prompt.md` 是发送给 DeepSeek 的主提示词。
- `triggers.md` 是 DeepSeek 通过 Raw URL 读取的 DM 内部触发器圣典。
- `deepseek_operator_protocol.md` 是给操作者使用的运行手册。
- `martial_sect_research.md` 是门派和剧情资料库，不直接喂给玩家。
- `triggers_writing_plan.md` 是触发器编写手册。

## 编辑规则

- 修改 `triggers.md` 后，如果影响运行行为，更新 `TRIGGER_CANON_VERSION`。
- 不要在玩家可见文本里暴露 `FLAG_`、Trigger ID、Raw URL、版本号或隐藏触发条件。
- 新增剧情时优先按“地点包 / 势力包 / 路线包”模块化组织，不要写成固定时间表。
- 新增门派或路线前，先在 `martial_sect_research.md` 补资料摘要，再写 trigger。
- 角色创建、存档、恢复相关字段必须保持一致：姓名、出身、属性、天赋、状态、武学、道具、已知经历、内部旗帜、路线状态。
- 禁忌武学不得无代价练成；辟邪、葵花、七伤、化功等必须保留代价或反噬。

## DeepSeek 运行约束

- 正式游玩前先测试 Raw URL 是否能读取 `TRIGGER_CANON_VERSION`。
- DeepSeek 网页端没有真正隐藏的 system prompt，推荐由 DM 操作者控制窗口，玩家只看游戏正文。
- 校准和存档按需使用，不要让模型自行计数回合。
- `DM_SAVE` 是给操作者看的，不能展示给玩家。

## 发布流程

1. 本地修改并检查 Markdown。
2. 确认 `game_engine_prompt.md` 中 Raw URL 仍指向：
   `https://raw.githubusercontent.com/Healock/jy/main/triggers.md`
3. 提交并推送到 `main`。
4. 推送后用 Raw URL 验证线上 `TRIGGER_CANON_VERSION`。

## 代码风格

- 文档使用 UTF-8。
- 优先使用简洁中文，避免长篇解释污染 DeepSeek 上下文。
- 面向玩家的选项要自然、行为驱动，不写“触发任务”“开启路线”等游戏化暗示。
