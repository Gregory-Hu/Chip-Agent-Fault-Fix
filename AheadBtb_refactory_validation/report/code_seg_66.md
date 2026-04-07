# AheadBtb_code_seg_66 重构正确性分析

|准确性分数|95|问题简述|
|--|--|
|95|`pred_s1_s2_entry_target_low_din[i] = s1_entry_target_low[i]` drives each way's 22-bit target low address into the S1->S2 pipeline register. Mirrors targetLowerBits in s2_entries.|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 138 行：
```scala
private val s2_entries  = RegEnable(Mux(overrideValid, s3_entries, s1_entries), s1_fire)
```

在预测输出 (SEG-S2-05)：
```scala
pred.bits.target := getFullTarget(s2_startPc, s2_entries(i).targetLowerBits, s2_entries(i).targetCarry)
```

对应关系：
- Original: `s2_entries(i).targetLowerBits` - 22-bit target low per way
- Refactored: `pred_s1_s2_entry_target_low_din[i] = s1_entry_target_low[i]` - 22 bits per way

功能：S1->S2 流水线 target_low 数据输入（每路 22 位）

### 1.2-1.6 Pipeline/Induction/Derivation
在 Preview 中，S2 级用 targetLowerBits 和 PC 高位拼接完整目标地址。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`s2_entries(i).targetLowerBits`|22-bit × 8 路|
|Preview|S2 级用 targetLowerBits 重建目标|地址拼接|
|设计归纳|目标地址压缩存储|仅存储低位|
|Architecture|N/A|N/A|
|Microarch|N/A|N/A|
|Implementation|`pred_s1_s2_entry_target_low_din[i] = s1_entry_target_low[i]`|8 路 × 22-bit 传递|

⚠️ 潜在问题：
- 无明显问题。
