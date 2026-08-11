# Codex 任务生成流程 (codex-task)

将 change-plan 结果转换为 Codex 可执行的开发任务。

## 前置条件

- 已有 change-plan 输出，或用户直接提供明确的修改方案

## 执行步骤

1. 提取 change-plan 中的修改项
2. 按 [templates/codex-prompt.md](../templates/codex-prompt.md) 结构组织
3. 将工程约束翻译为明确的 DO NOT / MUST 指令
4. 附上验证命令

## 转换规则

| change-plan 内容 | Codex prompt 表达 |
|-----------------|------------------|
| 保持不变的部分 | `DO NOT modify ...` |
| 必须遵守的规则 | `MUST: ...` |
| 风险项 | 写入 Constraint，要求防护 |
| 验证方式 | `dotnet build` / `dotnet test` |

## 输出示例

```markdown
# Task

实现录像上下文切换

## Modify

RecordingManager.cs

## Add

RecordingContext.cs

## Constraint

DO NOT modify Camera SDK layer

MUST:

maintain current recording flow

## Verification

dotnet build

dotnet test
```

## 输出要求

- 任务描述必须自包含：Codex 不依赖本次对话上下文也能执行
- 约束必须显式写出，不要隐含
- 文件路径使用相对路径
