# AheadBtb_code_seg_98 重构正确性分析

|准确性分数|90|问题简述|
|--|--|
|90|pred_res_target_o正确对应s2_reconstructed_target通过hit_mask门控，与原始Scala的pred.bits.target等价。同样缺少s2_valid条件。|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 183 行：
```scala
io.prediction.zipWithIndex.foreach { case (pred, i) =>
  ...
  pred.bits.target := getFullTarget(s2_startPc, s2_entries(i).targetLowerBits, s2_entries(i).targetCarry)
}
```
对应关系：
- 原始Scala中`pred.bits.target = getFullTarget(...)`：重建完整目标地址
- 重构后的RTL：`pred_res_target_o[i*PC_WIDTH +: PC_WIDTH] = pred_s2_hit_mask[i] ? s2_reconstructed_target[i*PC_WIDTH +: PC_WIDTH] : {PC_WIDTH{1'b0}}`
  - `s2_reconstructed_target[i]`是code_seg_93中重建的目标地址
  - 通过`pred_s2_hit_mask[i]`门控

功能：输出每路预测的分支目标地址，仅对命中的way有效。

### 1.2 Preview 阶段分析
在`/home/gregory/Desktop/Chip-Agent/result_check/Intermediate_files/preview/aheadbtb_pipeline_function_preview.md`中，代码段 S2-5 描述：
```scala
pred.bits.target := getFullTarget(s2_startPc, s2_entries(i).targetLowerBits, s2_entries(i).targetCarry)
```

### 1.3 Induction 阶段转换
在`/home/gregory/Desktop/Chip-Agent/result_check/Intermediate_files/aheadbtb_design_induction.md`中：
- 事件 E08：目标地址重建
- 事件 E09：预测结果输出

### 1.4 Derivation → Architecture Document
无独立的 Architecture 文档。

### 1.5 Derivation → Microarchitecture Document
无独立的 Microarchitecture 文档。

### 1.6 Derivation → Implementation Document
无独立的 Implementation 文档。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`pred.bits.target := getFullTarget(...)`|函数调用重建目标|
|Preview|S2-5代码段|target输出|
|设计归纳|事件E08, E09|目标重建+输出|
|Architecture|||
|Microarch|||
|Implementation|`pred_res_target_o[i] = hit_mask[i] ? reconstructed_target[i] : 0`|门控输出|

⚠️ 潜在问题：
- **门控逻辑**：同code_seg_95。
- **缺少s2_valid条件**。
- **缺少targetCarry**：继承自code_seg_93的问题。
