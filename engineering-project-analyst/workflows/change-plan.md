# 修改方案设计流程 (change-plan)

最核心流程。适用场景：需求已确认，需要设计"怎么改"。

## 前置条件

- 需求已经过 analyze 流程确认，或用户明确描述了修改目标
- 必须加载 [knowledge/engineering-safety.md](../knowledge/engineering-safety.md)

## 执行步骤

1. 明确修改目标（Objective）
2. 列出涉及的文件与模块
3. 划分：新增 / 修改 / 保持不变
4. 说明每处修改的原因
5. 执行风险分析（必须）
6. 给出验证方式

## 输出结构

```markdown
# Change Plan

## Objective
[修改目标]

## Files

### 新增
- RecordingContext.cs — [原因]

### 修改
- RecordingManager.cs — [原因]

### 保持不变
- CameraService.cs — 避免影响设备稳定性

## Data Change
[数据模型变化]

## Logic Change
[逻辑变化说明]

## Risk
[风险分析结果]

## Test
[验证方式]
```

## 风险分析（必须执行）

每个修改方案必须输出风险清单，至少覆盖：

- 数据风险：如视频损坏、数据不一致
- 状态风险：如状态错乱、非法跳转
- 资源风险：如 SDK 资源泄漏、句柄未释放
- 稳定性风险：如掉线、异常中断后的恢复

示例：

```text
风险:
- 视频损坏：切换订单时未正确关闭 Segment 文件
- 状态错乱：RECORDING 中直接跳转 SAVING
- SDK资源泄漏：新上下文重复打开相机未释放
```

## 关键约束

- 明确写出"保持不变"的部分及其原因，与修改部分同等重要
- 禁止修改层见 engineering-safety.md，除非用户明确要求
- 方案完成后，询问用户是否进入 codex-task 流程生成开发任务
