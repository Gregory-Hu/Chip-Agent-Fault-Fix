# AheadBtb_code_seg_132 重构正确性分析

|准确性分数|65|train_entry_target_low 使用训练 PC 的低位而非训练目标地址的低位，与原始设计语义不符|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 261 行：
```scala
private val t1_writeEntry = Wire(new AheadBtbEntry)
t1_writeEntry.valid           := true.B
t1_writeEntry.tag             := getTag(t1_train.startPc)
t1_writeEntry.position        := t1_trainPosition
t1_writeEntry.attribute       := t1_trainAttribute
t1_writeEntry.targetLowerBits := t1_trainTargetLowerBits
```
以及第 244-245 行：
```scala
private val t1_trainTarget          = t1_train.finalPrediction.target
private val t1_trainTargetLowerBits = getTargetLowerBits(t1_trainTarget)
```
对应关系：
- `t1_writeEntry.targetLowerBits` 对应重构后的 `train_entry_target_low`
- 原始设计中从 `t1_trainTarget`（分支目标地址）提取低位，而非从 PC 提取

功能：将分支目标地址的低位（22 bits）存储到 BTB 条目中，用于后续预测时重建完整目标地址。

### 1.2 Preview 阶段分析
在`aheadbtb_pipeline_function_preview.md`训练阶段 T1-5 中，writeEntry 的 targetLowerBits 被描述为：
```scala
t1_writeEntry.targetLowerBits := t1_trainTargetLowerBits
```
其中 `t1_trainTargetLowerBits = getTargetLowerBits(t1_trainTarget)`，来自最终预测的目标地址。

在`aheadbtb_enc_dec_function_preview.md`中：
```scala
def getTargetLowerBits(target: PrunedAddr): UInt =
  addrFields.extract("targetLower", target)
```

### 1.3 Induction 阶段转换
在`aheadbtb_design_induction.md`中，事件 E15（写入条目构建）和 E08（目标地址重建）描述为：
- 存储时：完整目标地址 → 提取低位（22 位）→ 存入 BTB 条目
- 预测时：锁存的 PC → 提取高位 → 拼接 → 完整目标地址

### 1.4 Derivation → Architecture Document
无对应的 Architecture 文档。

### 1.5 Derivation → Microarchitecture Document
无对应的 Microarchitecture 文档。

### 1.6 Derivation → Implementation Document
无对应的 Implementation 文档。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`t1_writeEntry.targetLowerBits := t1_trainTargetLowerBits`|从分支目标地址提取低位（22 bits）|
|Preview|getTargetLowerBits(t1_trainTarget) 从目标地址提取|预览中明确了 targetLowerBits 来源|
|设计归纳|E15/E08: 仅存储目标地址低位 22 位，高位复用 PC|归纳中描述了压缩存储机制|
|Architecture|-|-|
|Microarch|-|-|
|Implementation|`assign train_entry_target_low = train_s1_pc[21:0];`|从训练 PC 提取低位|

⚠️ 潜在问题：
- **来源错误**: 原始设计中 `targetLowerBits` 来自 `t1_train.finalPrediction.target`（分支目标地址），通过 `getTargetLowerBits()` 提取。重构后使用了 `train_s1_pc[21:0]`（训练 PC 的低位），这是分支指令的地址而非分支目标地址。
- **功能影响**: 对于跳转分支，分支指令地址（PC）和分支目标地址通常是不同的。使用 PC 低位代替目标低位会导致预测时重建出错误的目标地址。
- **位宽一致但语义错误**: 虽然都是 22 bits，但存储的内容完全不同，这是一个严重的功能错误。
