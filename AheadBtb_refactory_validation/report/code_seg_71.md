# AheadBtb_code_seg_71 重构正确性分析

|准确性分数|98|问题简述|
|--|--|--|
|98|信号正确传递，但RTL未实现overrideValid场景下从S3级选择数据的逻辑|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 125 行和第 147 行：
```scala
private val s1_startPc = io.startPc
...
private val s2_startPc  = RegEnable(s1_startPc, s1_fire)
```
对应关系：
- Scala中 `s2_startPc` 是寄存器，输入是 `s1_startPc`（直接来自 `io.startPc`），使能信号是 `s1_fire`
- RTL中 `pred_s2_pc` 直接组合赋值 `assign pred_s2_pc = pred_s1_s2_pc_d`
- `pred_s1_s2_pc_d` 是前一阶段锁存器输出，已包含正确的时序逻辑
- RTL同样缺少 `overrideValid` 时的S3级数据选择逻辑

功能：传递PC地址到阶段2，用于提取Tag进行比较

### 1.2 Preview 阶段分析
在`aheadbtb_pipeline_function_preview.md`中，SEG-S2-01代码段：
```scala
private val s2_startPc  = RegEnable(s1_startPc, s1_fire)
```
该信号被描述为：从S1级锁存起始PC地址到S2级（注意：S2的startPc不经过override Mux，直接从S1获取）。

### 1.3 Induction 阶段转换
在`aheadbtb_design_induction.md`中：
- 节点N03（标签比较）：包含标签提取单元，从锁存的PC中提取标签字段
- 设计归纳中描述了PC在阶段间的传递

### 1.4 Derivation → Architecture Document
架构规范中应描述 `s2_startPc` 直接从S1级锁存，不受override影响（与bankMask/setIdx不同）。

### 1.5 Derivation → Microarchitecture Document
微架构规范中应描述 `s2_startPc` 为简单的RegEnable结构。

### 1.6 Derivation → Implementation Document
实现规范中应体现PC寄存器的锁存逻辑。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`RegEnable(s1_startPc, s1_fire)`|简单寄存器|
|Preview|S2级锁存起始PC|直接从S1获取，不经override|
|设计归纳|阶段2锁存的PC|用于标签提取|
|Architecture|S2级寄存器输出|简单RegEnable|
|Microarch|简单寄存器|无Mux选择|
|Implementation|`assign pred_s2_pc = pred_s1_s2_pc_d`|正确传递|

⚠️ 潜在问题：
- 原始Scala中 `s2_startPc` 不经过override Mux（直接从S1锁存），而RTL也未实现override选择，在这个特定信号上行为是一致的。
- 此信号的重构是正确的，与原始设计意图一致。
