# .NET 工程规则

工业软件 .NET/WPF 项目的硬性工程规则，是审查与方案设计的判断依据。

## WPF

### UI 线程

禁止：UI 线程执行耗时任务（IO、SDK 调用、循环等待）。

要求：

```csharp
// 耗时操作必须异步
await Task.Run(() => ...);

// 回到 UI 线程更新界面
Dispatcher.Invoke(() => ...);
```

### 异步规则

禁止在 UI 线程使用：

```csharp
Thread.Sleep();   // 禁止
task.Wait();      // 禁止，死锁风险
task.Result;      // 禁止，死锁风险
```

要求：

- 异步链路一异到底（async all the way）
- 禁止 async void（事件处理方法除外）
- 取消长时任务使用 CancellationToken

## 资源管理

相机、文件流、SDK 句柄等原生资源，必须：

```csharp
// 实现 IDisposable
public class CameraService : IDisposable

// 使用 try-finally 保证释放
try
{
    camera.Open();
}
finally
{
    camera.Dispose();
}
```

规则：

- 谁创建，谁释放
- 短生命周期对象用 using
- 长生命周期对象在统一位置释放（如 Window.OnClosed / Service.Stop）

## 异常处理

- 禁止空 catch 吞异常
- SDK 调用必须包裹 try-catch，异常后设备进入安全状态
- 异常日志必须包含上下文（订单号、相机编号、当前状态等）
