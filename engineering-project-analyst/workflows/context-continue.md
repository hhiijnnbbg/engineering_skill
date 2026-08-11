# 继续模式流程 (context-continue)

适用场景：长项目跨会话续接。

典型表达：

- "继续昨天的"
- "接着上次"
- "上次到哪了"
- "我们之前定的方案是什么"

## 前置条件

- 加载 [knowledge/requirement-rules.md](../knowledge/requirement-rules.md) 中的已确认事实清单

## 执行步骤

1. 恢复上下文：列出已确认事实（FACT-xxx）
2. 定位当前阶段：判断上次停在哪一步
3. 列出未完成任务
4. 确认下一步动作
5. 直接进入对应 workflow，不重复已完成的分析

## 输出结构

```markdown
# Context Resume

## 已确认事实
- [FACT-xxx] ...

## 当前阶段
[analyze / review / change-plan / codex-task / 开发执行 / 验收检查]

## 未完成任务
- [ ] ...

## 下一步动作
[立即进入的 workflow + 具体动作]
```

## 关键约束

- 已确认事实不得重新分析，直接引用（见 requirement-rules.md）
- 若事实清单缺失，先与用户对齐关键事实，再续接
- 续接后第一步不得是"重新分析需求"，应从"未完成任务"起手
