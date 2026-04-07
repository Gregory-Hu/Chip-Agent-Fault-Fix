# AheadBtb_code_seg_74 重构正确性分析

|准确性分数|98|问题简述|
|--|--|--|
|98|信号正确传递，8路tag逐一正确映射，但缺少overrideValid场景|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 146 行：
```scala
private val s2_entries  = RegEnable(Mux(overrideValid, s3_entries, s1_entries), s1_fire)
```
以及标签比较使用：
```scala
private val s2_hitMask = s2_entries.map(entry => entry.valid && entry.tag === s2_tag)
```
对应关系：
- Scala中 `s2_entries(i).tag` 是8路条目的tag字段
- RTL中 `pred_s2_entry_tag[i]` 逐路来自 `pred_s1_s2_entry_tag_d[i]`
- 8路tag逐一正确映射

功能：传递8路条目的Tag值到阶段2，用于与PC提取的Tag进行比较

### 1.2 Preview 阶段分析
在`aheadbtb_pipeline_function_preview.md`中，SEG-S2-01代码段：
```scala
private val s2_entries  = RegEnable(Mux(overrideValid, s3_entries, s1_entries), s1_fire)
```
条目数据（包含tag）从S1级锁存到S2级。

### 1.3 Induction 阶段转换
在`aheadbtb_design_induction.md`中：
- 节点N03（标签比较）：包含多路比较器，将提取的标签与8路条目中的标签逐一比较

### 1.4 Derivation → Architecture Document
架构规范中应描述8路Tag的并行比较结构。

### 1.5 Derivation → Microarchitecture Document
微架构规范中应描述Tag比较的位宽和路数。

### 1.6 Derivation → Implementation Document
实现规范中应体现8路Tag的独立存储和比较。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`s2_entries(i).tag` 结构体字段|8路条目tag|
|Preview|S2级锁存条目数据|包含tag字段|
|设计归纳|阶段2锁存的条目tag|用于比较|
|Architecture|8路tag并行|24位Tag|
|Microarch|结构体字段|拆分为数组|
|Implementation|`pred_s2_entry_tag[i] = pred_s1_s2_entry_tag_d[i]`|逐路映射|

⚠️ 潜在问题：
- RTL缺少 `overrideValid` 场景下从S3级选择数据的逻辑。
- 8路tag逐一映射正确，位宽（24位）与原始设计一致。
