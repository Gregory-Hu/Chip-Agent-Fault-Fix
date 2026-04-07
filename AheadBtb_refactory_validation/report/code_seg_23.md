# AheadBtb_code_seg_23 重构正确性分析

|准确性分数|问题简述|
|--|--|
|80|bank_write_data_o 对应原始 writeReq.bits.entry 条目数据，位宽64位基本正确，但原始 entry 结构包含 valid/tag/position/attribute/targetLowerBits 等多字段（总约60位，对齐到64位），需确认字段打包顺序与外部 Bank 模块一致。|

## 1. 信号追踪分析

### 1.1 重构前 Scala 原始代码
在 `/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/frontend/bpu/abtb/AheadBtb.scala` 第 266 行：
```scala
b.io.writeReq.bits.entry        := t1_writeEntry
```
以及在第 270 行：
```scala
b.io.writeReq.bits.entry        := t1_writeEntry
```
以及在第 276 行：
```scala
b.io.writeReq.bits.entry        := 0.U.asTypeOf(new AheadBtbEntry)
```

对应关系：
- 原始 Scala 中 `b.io.writeReq.bits.entry` 类型为 `AheadBtbEntry`，其结构定义在 Bundles.scala 中：
  ```scala
  class AheadBtbEntry(implicit p: Parameters) extends AheadBtbBundle {
    val valid:           Bool            = Bool()                     // 1 bit
    val tag:             UInt            = UInt(TagWidth.W)           // 24 bits
    val position:        UInt            = UInt(CfiPositionWidth.W)   // 3 bits
    val attribute:       BranchAttribute = new BranchAttribute        // ~8 bits
    val targetLowerBits: UInt            = UInt(TargetLowerBitsWidth.W) // 22 bits
    val targetCarry: Option[TargetCarry] = if (EnableTargetFix) Option(new TargetCarry) else None
  }
  ```
- 总位宽约 1+24+3+8+22 = 58 bits（不含 targetCarry），在 SRAM 中对齐到 64 位
- 重构后 `bank_write_data_o` 是 output wire [64-1:0] = [63:0]，位宽一致

功能：向 Bank SRAM 写入的完整条目数据，包含 valid、tag、position、attribute、targetLowerBits 等字段。

### 1.2 Preview 阶段分析
在 `/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/aheadbtb_interface_preview.md` 的 3.3 AheadBtbEntry 数据结构中：
- valid: Bool (1 bit)
- tag: UInt[TagWidth-1:0] (24 bits)
- position: UInt[CfiPositionWidth-1:0] (3 bits)
- attribute: BranchAttribute (包含 branchType[3:0] + rasAction[3:0] = 8 bits)
- targetLowerBits: UInt[TargetLowerBitsWidth-1:0] (22 bits)
- targetCarry: Option[TargetCarry] (仅在 EnableTargetFix 时存在)

在 `/home/gregory/Desktop/Chip-Agent/result_check/Xiangshan/aheadbtb_pipeline_function_preview.md` 中：
- SEG-T1-05 确认 writeReq.bits.entry 为 AheadBtbEntry 类型

### 1.3 Induction 阶段转换
在 `/home/gregory/Desktop/Chip-Agent/result_check/Intermediate_files/aheadbtb_design_induction.md` 中：
- 存储结构表中 BTB Bank 位宽约 60 位/条目
- 机制 M08（目标地址重建机制）描述 targetLowerBits 为 22 位

### 1.4 Derivation → Architecture Document
无独立的 Architecture 文档可供追溯。

### 1.5 Derivation → Microarchitecture Document
无独立的 Microarchitecture 文档可供追溯。

### 1.6 Derivation → Implementation Document
无独立的 Implementation 文档可供追溯。

## 2. 演变总结

|阶段|信号形态|说明|
|--|--|--|
|原始代码|`b.io.writeReq.bits.entry` (AheadBtbEntry bundle)|多字段结构体，通过 Chisel bundle 传递|
|Preview|writeReq.bits.entry: AheadBtbEntry|接口 preview 中保留为结构体类型|
|设计归纳|Entry 位宽约60位（valid+tag+position+attribute+targetLowerBits）|SRAM 条目位宽|
|Architecture|N/A|N/A|
|Microarch|N/A|N/A|
|Implementation|`output wire [64-1:0] bank_write_data_o`|扁平化为64位纯数据总线|

⚠️ 潜在问题：
- **字段打包顺序**：原始设计中 entry 是 Chisel bundle，各字段有明确的名称和位域。重构后扁平化为 64 位向量 `bank_write_data_o`，外部 Bank 模块必须使用完全相同的字段排列顺序才能正确解析。需确认打包/解包一致性。
- **EnableTargetFix 配置**：原始设计中 targetCarry 是可选字段（EnableTargetFix = false 时不存在）。重构后固定为 64 位，需确认是否正确处理了该配置选项。
- **多路 Bank 合并**：4 个 Bank 的独立 writeReq.entry 接口被合并为单一输出，每周期只有一个 Bank 实际接收数据。
