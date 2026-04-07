# AheadBtb_code_seg_50 重构正确性分析

|准确性分数|98|问题简述|
|--|--|
|98|`pred_s1_pc` directly comes from `pred_s0_s1_pc_d`, which is the pipeline register output. This correctly mirrors the original Scala `s1_startPc = io.startPc`. The refactoring is accurate - the PC is captured in S0 and registered for S1 use.|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 118 行：
```scala
private val s1_startPc = io.startPc
```

注意：原始设计中 `s1_startPc` 直接读取 `io.startPc`（当前周期值），而不是使用 `RegEnable`。这是因为 PC 在 S1 阶段需要最新值用于后续 S2 的 tag 比较。

在 Code Seg 45 中：
```systemverilog
pred_s0_s1_pc_d <= pred_s0_s1_pc_din;  // where pred_s0_s1_pc_din = pred_req_pc_i
```

对应关系：
- Original: `s1_startPc = io.startPc` (directly reads input each cycle)
- Refactored: `pred_s1_pc = pred_s0_s1_pc_d` where `pred_s0_s1_pc_d` is registered from `pred_req_pc_i` on `pred_s0_s1_valid_en`

功能：S1 阶段的 PC 地址，用于 S2 阶段的标签提取和比较

### 1.2 Preview 阶段分析
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/aheadbtb_pipeline_function_preview.md`中：
```scala
private val s1_startPc = io.startPc
```
S1 级直接使用最新的 PC 地址，而不是锁存 S0 级的 PC。

### 1.3 Induction 阶段转换
在`/home/gregory/Desktop/Chip-Agent/result_check/Intermediate_files/aheadbtb_design_induction.md`中：
- 节点 N02: "包含寄存器组，锁存地址信息和条目数据"

### 1.4 Derivation → Architecture Document
无对应的 Architecture 文档。

### 1.5 Derivation → Microarchitecture Document
无对应的 Microarchitecture 文档。

### 1.6 Derivation → Implementation Document
无对应的 Implementation 文档。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`s1_startPc = io.startPc`|直接读取输入 PC（非锁存）|
|Preview|S1 级获取最新 PC 地址|非锁存方式|
|设计归纳|锁存地址信息|寄存器组|
|Architecture|N/A|N/A|
|Microarch|N/A|N/A|
|Implementation|`pred_s1_pc = pred_s0_s1_pc_d`|通过流水线寄存器传递|

⚠️ 潜在问题：
- 原始设计中 `s1_startPc` 直接读取 `io.startPc`，而重构设计通过流水线寄存器 `pred_s0_s1_pc_d` 传递。这引入了一个周期的延迟差异。在原始设计中，S1 使用当前周期的 PC；在重构设计中，S1 使用 S0 周期锁存的 PC。如果 `io.startPc` 在相邻周期变化，两者的行为可能不同。需要确认这种差异是否影响预测正确性。
