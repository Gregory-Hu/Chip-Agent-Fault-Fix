# AheadBtb_code_seg_60 重构正确性分析

|准确性分数|95|问题简述|
|--|--|
|95|`pred_s1_s2_pc_din = pred_s1_pc` drives the PC into the S1->S2 pipeline register. Mirrors `s2_startPc = RegEnable(s1_startPc, s1_fire)` in the original.|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 139 行：
```scala
private val s2_startPc  = RegEnable(s1_startPc, s1_fire)
```

对应关系：
- Original: `s2_startPc = RegEnable(s1_startPc, s1_fire)`
- Refactored: `pred_s1_s2_pc_din = pred_s1_pc` drives D input

功能：S1->S2 流水线 PC 数据输入

### 1.2-1.6 Pipeline/Induction/Derivation
在 Preview 中，S2 级锁存 S1 级的 startPc 用于 S2 的 tag 提取（如果未在 S0 提前计算）。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`s2_startPc = RegEnable(s1_startPc, s1_fire)`|S1 fire 时锁存|
|Preview|S2 级锁存 S1 级 PC|用于 tag 提取|
|设计归纳|锁存地址信息|寄存器组|
|Architecture|N/A|N/A|
|Microarch|N/A|N/A|
|Implementation|`pred_s1_s2_pc_din = pred_s1_pc`|直接传递|

⚠️ 潜在问题：
- 重构设计中 tag 已在 S0 阶段提前计算（pred_s0_tag），因此 S2 阶段可能不需要完整的 PC。但 PC 仍然用于目标地址重建（`getFullTarget(s2_startPc, ...)`）。需要确认 PC 在 S2 阶段是否被正确使用。
