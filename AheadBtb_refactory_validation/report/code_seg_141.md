# AheadBtb_code_seg_141 重构正确性分析

|准确性分数|55|bank_write_way_mask_o 仅支持 invalidate 场景的 way 选择，缺少 write_new_entry 和 correct_target 场景|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在`/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala`第 273-282 行：
```scala
banks.zipWithIndex.foreach { case (b, i) =>
  when(t1_fire && t1_needWriteNewEntry && t1_bankMask(i)) {
    b.io.writeReq.bits.wayIdx := victimWayIdx(i)
  }.elsewhen(t1_fire && t1_needCorrectTarget && t1_bankMask(i)) {
    b.io.writeReq.bits.wayIdx := OHToUInt(t1_hitMaskOH)
  }.elsewhen(s2_valid && s2_multiHit && s2_bankMask(i)) {
    b.io.writeReq.bits.wayIdx := s2_multiHitWayIdx
  }
}
```
对应关系：
- 原始设计使用 `wayIdx`（3-bit 索引），重构后使用 `way_mask`（8-bit one-hot 掩码）
- 三种场景使用不同的 way 选择策略

功能：指定写入目标 Way 的 one-hot 掩码。

### 1.2 Preview 阶段分析
在`aheadbtb_storage_structure_preview.md`中：
```scala
private val writeWayMask = UIntToOH(writeBuffer.io.read.head.bits.wayIdx)
sram.io.w.apply(
  waymask = writeWayMask
)
```
Bank 内部将 wayIdx 转换为 one-hot 掩码用于 SRAM 写选择。

### 1.3 Induction 阶段转换
在`aheadbtb_design_induction.md`中，机制 M06 描述为：
- 检测到多命中时选择优先级最高的 Way 进行无效化

### 1.4 Derivation → Architecture Document
无对应的 Architecture 文档。

### 1.5 Derivation → Microarchitecture Document
无对应的 Microarchitecture 文档。

### 1.6 Derivation → Implementation Document
无对应的 Implementation 文档。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`writeReq.bits.wayIdx`（3-bit 索引）|每场景独立选择 wayIdx|
|Preview|Bank 内部将 wayIdx 转换为 one-hot|预览中描述了 way 编码转换|
|设计归纳|E17: 写请求包含 Way 索引|归纳中描述了写请求内容|
|Architecture|-|-|
|Microarch|-|-|
|Implementation|`assign bank_write_way_mask_o = train_s1_write_way_mask;`|8-bit one-hot 掩码|

⚠️ 潜在问题：
- **场景不完整**: `train_s1_write_way_mask` 仅处理了 invalidate 场景，缺少 write_new_entry 和 correct_target 场景（详见 code_seg_137）。
- **接口格式改变**: 原始设计使用 `wayIdx`（3-bit），重构后使用 `way_mask`（8-bit one-hot）。这种改变需要与外部 Bank 接口定义保持一致。
- **编码效率**: one-hot 掩码比索引占用更多位宽（8 bits vs 3 bits），但对于 SRAM 的 way mask 接口来说，one-hot 是更自然的格式。
- **信号传递正确性**: `bank_write_way_mask_o` 正确地传递了 `train_s1_write_way_mask`，但由于 write_way_mask 本身的场景覆盖不完整，导致整个写路径存在问题。
