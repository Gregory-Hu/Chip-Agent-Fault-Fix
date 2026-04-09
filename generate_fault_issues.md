# Fault Issue 自动生成工具

## 功能说明

本工具根据 `status.md` 和 `report_summary.md` 自动生成每个问题的独立 fault issue 文件。

## 使用方法

```bash
python3 generate_fault_issues.py
```

## 生成规则

### 1. 问题分类
- **严重问题**: Score < 70
- **中等问题**: 70 <= Score < 80
- **轻微问题**: 80 <= Score < 90

### 2. 负责人分配
根据问题首次出现的 pipeline 阶段确定负责人:

| Pipeline 阶段 | 负责人 |
|-------------|--------|
| Interface Signals | 接口设计组 |
| Frame1: 预测阶段0 | 预测阶段0开发组 |
| Frame2: 预测阶段1 | 预测阶段1开发组 |
| Frame3: 预测阶段2 | 预测阶段2开发组 |
| Frame5: 训练阶段0 | 训练阶段0开发组 |
| Frame6: 训练阶段1 | 训练阶段1开发组 |

### 3. Issue 文件内容
每个 `fault_issue_segXXX.md` 包含:
- 基本信息 (段号、分数、严重等级、负责人)
- 问题描述
- 相关代码片段
- 详细分析报告

## 输出目录

生成的文件保存在 `fault_issues/` 目录下:
```
fault_issues/
├── README.md                    # 问题索引
├── fault_issue_seg007.md
├── fault_issue_seg022.md
├── ...
└── fault_issue_seg143.md
```

## 示例输出

每个 issue 文件格式如下:

```markdown
# 故障问题报告: 代码段 130

## 基本信息

| 字段 | 值 |
|------|-----|
| **代码段** | 130 |
| **分数** | 45 |
| **严重程度** | 严重问题 (Score <70) |
| **负责人** | 训练阶段1开发组 |
| **所属阶段** | Frame6: 训练阶段1 - 计数器更新与条目写入 |

## 问题描述

train_entry_position 硬编码为 3'b000,丢失了原始设计中来自 t1_train.finalPrediction.cfiPosition 的动态位置信息,影响多命中检测和计数器更新

## 代码片段

```systemverilog
assign train_entry_position = 3'b000;
```

## 详细分析

[来自 report/code_seg_130.md 的完整内容]
```
