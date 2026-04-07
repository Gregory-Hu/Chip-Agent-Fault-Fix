# AheadBtb 重构正确性分析总结报告

对143个code seg进行了重构正确性评估

|Range|Count|Percentage|
|--|--|--|
|90-100（没有问题）|64|44.8%|
|80-90（轻微问题）|42|29.4%|
|70-80（中等问题）|25|17.5%|
|<70（严重问题）|12|8.4%|


## 严重问题 (Score <70)
- **Code Seg 28 (65)**: dir_counter_write_data_o 将原始三种饱和计数器更新操作（resetWeakPositive/selfDecrease/selfIncrease）简化为直接写入2位数据值，丢失了饱和操作的语义信息
- **Code Seg 130 (45)**: train_entry_position 硬编码为 3'b000，丢失了原始设计中来自 t1_train.finalPrediction.cfiPosition 的动态位置信息，影响多命中检测和计数器更新
- **Code Seg 131 (40)**: train_entry_is_branch 硬编码为 1'b1，将原始 BranchAttribute 结构（包含 isConditional、isIndirect 等多位属性）简化为单比特，导致分支类型信息完全丢失
- **Code Seg 132 (65)**: train_entry_target_low 使用 train_s1_pc[21:0] 而非实际的目标地址低位，导致目标预测错误
- **Code Seg 133 (60)**: train_entry_valid_bit 逻辑与原始设计不一致，原始设计中新条目 valid 始终为 true，无效化通过写全零条目实现
- **Code Seg 134 (55)**: train_s1_write_data 将条目打包为扁平化64位向量，位布局与原始 AheadBtbEntry 结构不完全一致，且内部字段存在多项硬编码问题
- **Code Seg 136 (60)**: replacer_hit_way_i 硬编码为 8'b0，未正确连接 replacer 的命中 way 信息
- **Code Seg 137 (50)**: train_s1_write_way_mask 仅处理 invalidate 场景，缺少 write_new_entry（使用 PLRU victim）和 correct_target（使用命中 way）场景的 way 选择
- **Code Seg 138 (55)**: write_buffer_push_en 缺少 correct_target 场景的写请求驱动，且 write_buffer_full 硬编码为0移除了写缓冲满保护
- **Code Seg 141 (55)**: bank_write_way_mask_o 与 train_s1_write_way_mask 相同的场景覆盖不完整问题
- **Code Seg 142 (50)**: bank_write_data_o 传递的扁平化 write_data 内部字段存在多项功能语义丢失（position、attribute、target_low 等）
- **Code Seg 143 (40)**: write_buffer_full 硬编码为 1'b0 移除了写缓冲满保护机制，原始设计中写缓冲满时会丢弃新写请求以防止溢出

## 中等问题 (70<= Score <80)
- **Code Seg 7 (75)**: pred_res_is_branch_o 受限于 train_entry_is_branch 硬编码问题，分支属性输出不完整
- **Code Seg 22 (75)**: bank_write_way_mask_o 仅处理 invalidate 场景，缺少新条目和 target 修正场景的 way 选择
- **Code Seg 24 (70)**: dir_counter_read_idx_o 的索引组合方式可能与原始计数器阵列接口不完全匹配
- **Code Seg 25 (75)**: dir_counter_read_data_i 的读取数据解析存在方向位提取简化
- **Code Seg 26 (70)**: dir_counter_write_en_o 的写使能条件简化了原始三种更新操作的触发逻辑
- **Code Seg 27 (70)**: dir_counter_write_idx_o 的写地址生成与原始设计可能不完全一致
- **Code Seg 34 (75)**: pred_s0_tag 使用硬编码位宽 [45:22] 而非参数化，缺乏可配置性
- **Code Seg 36 (75)**: pred_s0_valid 的有效信号组合可能与原始时序控制不完全一致
- **Code Seg 37 (70)**: bank_read_req_o 直接由 pred_s0_valid 驱动，缺少原始设计中的缓冲控制
- **Code Seg 39 (75)**: pred_s0_s1_valid_en 的流水线使能信号可能与原始设计不完全一致
- **Code Seg 45 (70)**: pred_s0_s1 流水线寄存器的复位和使能逻辑可能与原始设计不完全匹配
- **Code Seg 81 (75)**: s2_position_hit_mask 的 generate 块生成逻辑位宽可能与原始设计不完全一致
- **Code Seg 82 (70)**: s2_position_0_count 的多命中计数逻辑简化了原始比较器
- **Code Seg 83 (70)**: s2_position_1_count 的多命中计数逻辑简化了原始比较器
- **Code Seg 84 (70)**: s2_position_2_count 的多命中计数逻辑简化了原始比较器
- **Code Seg 85 (70)**: s2_position_3_count 的多命中计数逻辑简化了原始比较器
- **Code Seg 86 (75)**: s2_position_4_count 的多命中计数逻辑基本正确
- **Code Seg 87 (75)**: s2_position_5_count 的多命中计数逻辑基本正确
- **Code Seg 88 (75)**: s2_position_6_count 的多命中计数逻辑基本正确
- **Code Seg 89 (75)**: s2_position_7_count 的多命中计数逻辑基本正确
- **Code Seg 124 (75)**: train_s1_counter_write_en 的写使能条件简化了原始更新逻辑
- **Code Seg 125 (70)**: train_s1_counter_write_data 的写数据生成逻辑简化了原始计数器更新规则
- **Code Seg 128 (75)**: dir_counter_write_data_o 传递的写数据存在操作语义丢失问题
- **Code Seg 129 (70)**: train_entry_tag 使用硬编码位宽 [45:22] 而非参数化
- **Code Seg 139 (70)**: bank_write_req_o 的写请求驱动受限于 write_buffer_push_en 的场景覆盖问题

## 轻微问题 (80<= Score <90)
- **Code Seg 2 (85)**: pred_req_pc_i 的位宽宏定义 `PC_WIDTH 可能与原始参数化设计不完全一致
- **Code Seg 4 (80)**: pred_res_valid_o 的输出位宽为 8，与原始设计匹配但缺少参数化
- **Code Seg 5 (85)**: pred_res_direction_o 受限于方向位解析的简化
- **Code Seg 6 (85)**: pred_res_position_o 受限于 position 字段的硬编码问题
- **Code Seg 8 (80)**: pred_res_target_o 的目标重建逻辑受限于 target_low 的来源问题
- **Code Seg 9 (85)**: pred_meta_valid_o 的有效信号生成逻辑基本正确
- **Code Seg 10 (85)**: pred_meta_bank_mask_o 的 bank mask 生成逻辑正确
- **Code Seg 12 (85)**: pred_meta_pc_o 的 PC 传递逻辑正确
- **Code Seg 15 (88)**: train_req_final_direction_i 的接口定义基本正确
- **Code Seg 19 (85)**: bank_read_data_i 的位宽为 8*64，与原始设计一致
- **Code Seg 20 (88)**: bank_write_req_o 的写请求信号基本正确
- **Code Seg 21 (80)**: bank_write_set_idx_o 的写地址传递逻辑正确
- **Code Seg 23 (80)**: bank_write_data_o 的写数据传递受限于内部字段问题
- **Code Seg 29 (85)**: replacer_set_idx_o 的替换器索引传递正确
- **Code Seg 30 (80)**: replacer_hit_way_i 的替换器命中信息基本正确
- **Code Seg 31 (85)**: replacer_replace_way_o 的替换 way 输出正确
- **Code Seg 32 (80)**: pred_s0_bank_idx 的 bank 索引计算正确
- **Code Seg 33 (80)**: pred_s0_set_idx 的 set 索引计算正确
- **Code Seg 35 (85)**: pred_s0_bank_mask 的 bank mask 生成逻辑正确
- **Code Seg 38 (85)**: bank_set_idx_o 的 set 索引传递正确
- **Code Seg 40 (85)**: pred_s0_s1_bank_idx_din 的流水线输入正确
- **Code Seg 42 (80)**: pred_s0_s1_set_idx_din 的流水线输入基本正确
- **Code Seg 43 (80)**: pred_s0_s1_pc_din 的 PC 流水线输入正确
- **Code Seg 44 (85)**: pred_s0_s1_tag_din 的 tag 流水线输入正确
- **Code Seg 79 (85)**: dir_counter_read_idx_o 的读索引组合逻辑基本正确
- **Code Seg 80 (80)**: pred_s2_direction 的方向位解析逻辑基本正确
- **Code Seg 90 (85)**: pred_s2_multi_hit 的多命中判断逻辑正确
- **Code Seg 91 (80)**: s2_multi_hit_priority_mask 的优先级编码逻辑正确
- **Code Seg 92 (80)**: pred_s2_multi_hit_way 的多命中 way 选择逻辑正确
- **Code Seg 103 (85)**: train_s0_valid 的训练有效信号生成正确
- **Code Seg 105 (80)**: train_s0_write_entry 的写条目条件生成基本正确
- **Code Seg 106 (85)**: train_s0_invalidate 的无效化条件生成正确
- **Code Seg 109 (85)**: train_s0_s1_final_direction_din 的方向输入正确
- **Code Seg 110 (85)**: train_s0_s1_bank_mask_din 的 bank mask 输入正确
- **Code Seg 111 (85)**: train_s0_s1_set_idx_din 的 set 索引输入正确
- **Code Seg 114 (85)**: train_s0_s1_invalidate_din 的无效化输入正确
- **Code Seg 115 (85)**: train_s0_s1 流水线寄存器逻辑正确
- **Code Seg 118 (85)**: train_s1_final_direction 的方向传递正确
- **Code Seg 122 (85)**: train_s1_write_entry 的写条目信号传递正确
- **Code Seg 127 (90)**: dir_counter_write_idx_o 的写索引生成正确
- **Code Seg 135 (85)**: replacer_set_idx_o 的替换器索引传递正确
- **Code Seg 140 (85)**: bank_write_set_idx_o 的写 set 索引传递正确
