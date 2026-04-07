# AheadBtb_code_seg_30 重构正确性分析

|准确性分数|问题简述|
|--|--|
|80|replacer_hit_way_i 对应原始 replacers 的 readWayMask 输入（经预测命中更新），位宽8位正确。但原始 readWayMask 是在预测阶段2由 s2_hitMask 驱动，重构后作为外部输入，需确认数据时序与预测流水线一致。|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在 `/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala` 第 197-199 行：
```scala
replacers.zipWithIndex.foreach { case (r, i) =>
  r.io.readValid   := s2_valid && s2_hit && s2_bankMask(i)
  r.io.readSetIdx  := s2_setIdx
  r.io.readWayMask := s2_hitMask
}
```

对应关系：
- 原始 Scala 中 `r.io.readWayMask` 类型为 `Vec[NumWays](Bool)` = `Vec[8](Bool)`，即 8 位布尔向量
- 每个 replacer 接收相同的 `s2_hitMask`（8 路标签比较的命中掩码）
- `s2_hitMask` 定义在第 164 行：
  ```scala
  private val s2_hitMask = s2_entries.map(entry => entry.valid && entry.tag === s2_tag)
  ```
- 重构后 `replacer_hit_way_i` 是 input wire [7:0]，位宽一致

功能：将预测阶段2的标签比较命中掩码输入到替换策略模块，用于在预测命中时更新 PLRU 状态。

### 1.2 Preview 阶段分析
在 `/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/aheadbtb_interface_preview.md` 的 6.1 替换策略接口部分：
- `readWayMask`: Input, Vec[NumWays](Bool), 预测读 Way 命中掩码

在 `/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/aheadbtb_pipeline_function_preview.md` 中：
- SEG-REPL-01 描述预测读状态更新：
  ```scala
  touchWays.zip(io.readWayMask).zipWithIndex.foreach { case ((t, r), i) =>
    t.valid := r
    t.bits  := i.U
  }
  ```
  readWayMask 用于标记哪些 Way 被命中访问，以更新 PLRU 状态

### 1.3 Induction 阶段转换
在 `/home/gregory/Desktop/Chip-Agent/result_check/Intermediate_files/aheadbtb_design_induction.md` 中：
- 事件 E10（替换策略更新-读）：
  - 当预测命中时更新 PLRU 状态
  - 来源：submodule, control
- 机制 M03（8路组相联与PLRU替换机制）：
  - 预测命中时更新 PLRU 状态，记录访问的 Way

### 1.4 Derivation → Architecture Document
无独立的 Architecture 文档可供追溯。

### 1.5 Derivation → Microarchitecture Document
无独立的 Microarchitecture 文档可供追溯。

### 1.6 Derivation → Implementation Document
无独立的 Implementation 文档可供追溯。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`r.io.readWayMask := s2_hitMask` (Vec[8](Bool))|预测阶段2内部生成的命中掩码|
|Preview|readWayMask: Input Vec[NumWays](Bool)|接口 preview 中为8位布尔向量|
|设计归纳|预测命中时通过 hitMask 更新 PLRU 状态|归纳为替换策略的读路径输入|
|Architecture|N/A|N/A|
|Microarch|N/A|N/A|
|Implementation|`input wire [7:0] replacer_hit_way_i`|8位命中掩码输入|

⚠️ 潜在问题：
- **信号来源变更**：原始设计中 `readWayMask` 是 AheadBtb 内部产生的信号（`s2_hitMask`），直接驱动到 replacer 子模块。重构后 `replacer_hit_way_i` 变为从外部输入的信号。这意味着命中掩码的计算被移出 AheadBtb 模块，需要外部逻辑根据 SRAM 读数据和标签比较结果生成该信号。需要确认外部模块的标签比较逻辑与原始设计一致。
- **时序一致性**：原始 `s2_hitMask` 在预测阶段2生成并立即驱动到 replacer。重构后 `replacer_hit_way_i` 作为外部输入，其时序必须与预测阶段2的时序精确对齐，否则 PLRU 状态更新会出现时序错误。
- **readValid 信号关联**：原始设计中 `readWayMask` 的更新与 `readValid` 信号配合使用（`r.io.readValid := s2_valid && s2_hit && s2_bankMask(i)`）。重构后 `replacer_hit_way_i` 需要与相应的 readValid 信号配合使用，需确认外部是否也提供了该信号。
