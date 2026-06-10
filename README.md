# 金庸群侠传式 DeepSeek 文字游戏

这是一个用于 DeepSeek 网页端的金庸群侠传式文字游戏提示词项目。核心思路是：DeepSeek 先读取公开 Raw URL 中的 `triggers.md`，把隐藏剧情触发器加入上下文；玩家正常游玩时看不到触发器内容。

## 文件说明

- `game_engine_prompt.md`：正式发给 DeepSeek 的主提示词。
- `triggers.md`：DM 内部触发器圣典，DeepSeek 通过 Raw URL 读取。
- `deepseek_operator_protocol.md`：DeepSeek 网页端操作手册。
- `martial_sect_research.md`：金庸门派与同人游戏资料库。
- `triggers_writing_plan.md`：触发器编写计划和格式说明。
- `AGENTS.md`：给后续 Codex/代理的项目维护规则。

## Raw URL

DeepSeek 读取的触发器地址：

```text
https://raw.githubusercontent.com/Healock/jy/main/triggers.md
```

当前版本应为：

```text
TRIGGER_CANON_VERSION: jy-2026-06-11-v3
```

## DeepSeek 开局流程

### 1. 加载测试

先在 DeepSeek 网页端发送：

```md
请启用联网搜索并访问：
https://raw.githubusercontent.com/Healock/jy/main/triggers.md

只回答：
LOAD_OK: 你看到的 TRIGGER_CANON_VERSION
或
LOAD_FAIL: 原因

不要复述文件内容。
```

成功时应看到：

```text
LOAD_OK: jy-2026-06-11-v3
```

### 2. 正式启动

把 `game_engine_prompt.md` 的全文发送给 DeepSeek。

DeepSeek 应先进入角色创建，而不是直接开始剧情。

### 3. 角色创建

玩家需要提供：

- 姓名：留空则默认“小虾米”
- 出身：乡野孤儿 / 破落镖户 / 客栈杂役 / 寒门书生
- 初始属性：体魄、根骨、机敏、悟性、定力、福缘，基础均为 5，另有 6 点自由分配，最终总和 36，单项 4～8；出身不额外改变面板属性
- 天赋：侠心未泯 / 市井耳目 / 根骨清奇 / 胆大心细 / 命里多舛

示例：

```md
姓名：沈青
出身：客栈杂役
属性分配：体魄5，根骨5，机敏8，悟性6，定力5，福缘7
天赋：市井耳目
```

角色创建完成后，DeepSeek 应从牛家村开局。

## 日常游玩

多数时候直接转发玩家行动：

```md
玩家行动：我去客栈看看有没有活干。
```

如果场景容易跑偏，可以补充地点约束：

```md
玩家行动：我去后山看看有没有人。

注意：玩家当前仍在牛家村，时间是傍晚，尚未离村。请只考虑牛家村/后山外围相关事件。
```

## 按需校准

不要固定按回合校准。只有 DeepSeek 跑偏、换地图、获得关键线索、进入门派或忘记状态时使用：

```md
DM内部校准：当前地点=牛家村后山外围，当前路线=无，玩家尚未离村。只使用当前地点、当前势力或最近行为相关 trigger。不要输出内部信息，继续游戏。
```

## 存档

只在关键节点、准备关网页或准备新开对话时保存：

```md
生成极简 DM_SAVE。不要复述触发器内容，不要暴露 URL，不要解释。
```

如果格式不稳定，使用完整模板：

```md
生成极简 DM_SAVE。不要复述触发器内容，不要暴露 URL，不要解释。

按以下格式输出：

【DM_SAVE_BEGIN】
时间：
地点：
状态：
属性：
天赋：
武学：
已知经历：
道具：
内部旗帜摘要：
当前路线：
挂起/关闭路线：
下一步注意：
【DM_SAVE_END】
```

## 恢复游戏

新开 DeepSeek 对话时：

1. 先做加载测试。
2. 发送 `game_engine_prompt.md` 全文。
3. 粘贴最近一次 `DM_SAVE`。
4. 发送恢复指令：

```md
根据下面的 DM_SAVE 恢复游戏。不要向玩家展示内部旗帜、Trigger ID、URL 或版本号。从存档中的当前地点和当前状态继续，按四段格式输出下一段游戏内容。

【DM_SAVE_BEGIN】
这里粘贴存档内容
【DM_SAVE_END】
```

## 维护与发布

修改 `triggers.md` 后：

1. 更新 `TRIGGER_CANON_VERSION`。
2. 检查 `deepseek_operator_protocol.md` 中的期望版本。
3. 提交并推送到 GitHub。
4. 用 Raw URL 重新测试加载。

## 注意事项

- 玩家可见正文不能出现 `FLAG_`、Trigger ID、Raw URL、版本号或隐藏触发条件。
- 角色创建完成前不得触发剧情、门派入口、隐藏事件或时间推进。
- 禁忌武学不能无代价练成，例如辟邪剑法、葵花宝典、七伤拳等。
- DeepSeek 网页端的思考/浏览过程只给操作者看，不要转发给玩家。
