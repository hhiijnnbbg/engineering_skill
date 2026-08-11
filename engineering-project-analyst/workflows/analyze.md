# 需求分析流程 (analyze)

适用场景：新需求、业务描述、功能想法。

## 输入

用户的业务描述或功能想法，例如："录像过程中扫码添加新的板材"。

## 执行步骤

1. 提取业务规则：将自然语言描述转换为明确的规则条目
2. 识别状态变化：画出状态流转路径
3. 识别数据变化：梳理数据模型层级
4. 输出 Requirement Model

## 输出：Requirement Model

使用 yaml 格式统一输出：

```yaml
Feature:
  name: AddBoardDuringRecording

State:
  RECORDING

Rules:
  - allow_add_board=true
  - keep_video=true
  - save_event=true
```

## 分析内容

### 1. 业务规则

格式：`触发条件 → 系统行为`

示例：

```text
订单号变化 → 创建新录像上下文
```

### 2. 状态变化

格式：状态流转图

示例：

```text
WAIT_SCAN → RECORDING → ORDER_SWITCH → RECORDING
```

注意：状态跳转必须合法，禁止跨级跳转（如 WAIT_SCAN 直接跳 SAVING）。

### 3. 数据变化

格式：数据层级树

示例：

```text
Task
 └── OrderContext
       └── Segment
             └── Plate
```

## 输出要求

- 分析结果按 [templates/analysis-report.md](../templates/analysis-report.md) 结构组织
- 需求歧义必须列出并向用户确认，不要自行假设
- 分析完成后，询问用户是否进入 change-plan 流程
