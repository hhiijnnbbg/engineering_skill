# 代码与架构审查流程 (review)

适用场景：

- "帮我看看这个代码"
- "这个设计合理吗"
- "为什么会这样"

## 执行步骤

1. 定位代码所属架构层（UI / Service / Hardware Interface / SDK）
2. 检查架构依赖方向
3. 检查代码风险项
4. 输出问题清单与改进建议

## 审查内容

### 1. 架构问题

检查依赖方向是否正确。

发现问题示例：

```text
UI
 |
Camera SDK
```

问题：UI 直接依赖硬件层

建议改为：

```text
UI
 |
Service
 |
Camera Interface
 |
SDK
```

原则：依赖只能自上而下单向传递，禁止跨层直连硬件。

### 2. 代码风险

逐项检查：

| 检查项 | 关注点 |
|-------|-------|
| 生命周期 | 对象创建与销毁是否配对 |
| 异步 | UI 线程是否被阻塞、async/await 链路是否完整 |
| 线程 | 是否存在跨线程访问 UI、共享数据竞争 |
| 资源释放 | 相机句柄、文件流、SDK 资源是否释放 |
| 异常处理 | 异常是否被吞掉、是否有恢复逻辑 |

风险发现示例：

```csharp
camera.Open();
```

没有对应的 `Dispose()`，输出：

```text
风险: 相机句柄泄漏
```

## 输出要求

按严重级别分类：

- **严重**：必须修复（资源泄漏、线程安全、硬件依赖）
- **建议**：应当改进（结构、可读性）
- **提示**：可选优化

审查依据引用 [knowledge/dotnet-rules.md](../knowledge/dotnet-rules.md) 与 [knowledge/vision-rules.md](../knowledge/vision-rules.md)。
