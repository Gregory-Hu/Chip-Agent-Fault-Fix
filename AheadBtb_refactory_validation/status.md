# Code Segmentation Status for AheadBtb.sv

## Code Segments

### Interface Signals

#### Code Seg 1
```systemverilog
input wire                                  pred_req_valid_i,
```
**Score:** 90

---

#### Code Seg 2
```systemverilog
input wire [`PC_WIDTH-1:0]                  pred_req_pc_i,
```
**Score:** 85

---

#### Code Seg 3
```systemverilog
input wire                                  pred_req_enable_i,
```
**Score:** 90

---

#### Code Seg 4
```systemverilog
output wire [7:0]                           pred_res_valid_o,
```
**Score:** 80

---

#### Code Seg 5
```systemverilog
output wire [7:0]                           pred_res_direction_o,
```
**Score:** 85

---

#### Code Seg 6
```systemverilog
output wire [7:0]                           pred_res_position_o,
```
**Score:** 85

---

#### Code Seg 7
```systemverilog
output wire [7:0]                           pred_res_is_branch_o,
```
**Score:** 75

---

#### Code Seg 8
```systemverilog
output wire [8*`PC_WIDTH-1:0]               pred_res_target_o,
```
**Score:** 80

---

#### Code Seg 9
```systemverilog
output wire                                 pred_meta_valid_o,
```
**Score:** 85

---

#### Code Seg 10
```systemverilog
output wire [3:0]                           pred_meta_bank_mask_o,
```
**Score:** 85

---

#### Code Seg 11
```systemverilog
output wire [4:0]                           pred_meta_set_idx_o,
```
**Score:** 90

---

#### Code Seg 12
```systemverilog
output wire [`PC_WIDTH-1:0]                 pred_meta_pc_o,
```
**Score:** 85

---

#### Code Seg 13
```systemverilog
input wire                                  train_req_valid_i,
```
**Score:** 95

---

#### Code Seg 14
```systemverilog
input wire [`PC_WIDTH-1:0]                  train_req_pc_i,
```
**Score:** 95

---

#### Code Seg 15
```systemverilog
input wire                                  train_req_final_direction_i,
```
**Score:** 88

---

#### Code Seg 16
```systemverilog
input wire                                  train_req_meta_valid_i,
```
**Score:** 92

---

#### Code Seg 17
```systemverilog
output wire                                 bank_read_req_o,
```
**Score:** 90

---

#### Code Seg 18
```systemverilog
output wire [4:0]                           bank_set_idx_o,
```
**Score:** 95

---

#### Code Seg 19
```systemverilog
input wire [8*64-1:0]                       bank_read_data_i,
```
**Score:** 85

---

#### Code Seg 20
```systemverilog
output wire                                 bank_write_req_o,
```
**Score:** 88

---

#### Code Seg 21
```systemverilog
output wire [4:0]                           bank_write_set_idx_o,
```
**Score:** 80

---

#### Code Seg 22
```systemverilog
output wire [7:0]                           bank_write_way_mask_o,
```
**Score:** 75

---

#### Code Seg 23
```systemverilog
output wire [64-1:0]                        bank_write_data_o,
```
**Score:** 80

---

#### Code Seg 24
```systemverilog
output wire [8:0]                           dir_counter_read_idx_o,
```
**Score:** 70

---

#### Code Seg 25
```systemverilog
input wire [8*2-1:0]                        dir_counter_read_data_i,
```
**Score:** 75

---

#### Code Seg 26
```systemverilog
output wire                                 dir_counter_write_en_o,
```
**Score:** 70

---

#### Code Seg 27
```systemverilog
output wire [8:0]                           dir_counter_write_idx_o,
```
**Score:** 70

---

#### Code Seg 28
```systemverilog
output wire [2-1:0]                         dir_counter_write_data_o,
```
**Score:** 65

---

#### Code Seg 29
```systemverilog
output wire [4:0]                           replacer_set_idx_o,
```
**Score:** 85

---

#### Code Seg 30
```systemverilog
input wire [7:0]                            replacer_hit_way_i,
```
**Score:** 80

---

#### Code Seg 31
```systemverilog
output wire [2:0]                           replacer_replace_way_o
```
**Score:** 85

---

### Frame1: 预测阶段0 - 地址计算与SRAM读请求

#### Code Seg 32
```systemverilog
assign pred_s0_bank_idx = pred_req_pc_i[6:5];
```
**Score:** 80

---

#### Code Seg 33
```systemverilog
assign pred_s0_set_idx  = pred_req_pc_i[10:7];
```
**Score:** 80

---

#### Code Seg 34
```systemverilog
assign pred_s0_tag      = pred_req_pc_i[45:22];
```
**Score:** 75

---

#### Code Seg 35
```systemverilog
assign pred_s0_bank_mask = (pred_s0_bank_idx == 2'b00) ? 4'b0001 :
                           (pred_s0_bank_idx == 2'b01) ? 4'b0010 :
                           (pred_s0_bank_idx == 2'b10) ? 4'b0100 :
                           (pred_s0_bank_idx == 2'b11) ? 4'b1000 : 4'b0000;
```
**Score:** 85

---

#### Code Seg 36
```systemverilog
assign pred_s0_valid     = pred_req_valid_i & pred_req_enable_i;
```
**Score:** 75

---

#### Code Seg 37
```systemverilog
assign bank_read_req_o   = pred_s0_valid;
```
**Score:** 70

---

#### Code Seg 38
```systemverilog
assign bank_set_idx_o    = pred_s0_set_idx;
```
**Score:** 85

---

#### Code Seg 39
```systemverilog
assign pred_s0_s1_valid_en            = pred_s0_valid;
```
**Score:** 75

---

#### Code Seg 40
```systemverilog
assign pred_s0_s1_bank_idx_din        = pred_s0_bank_idx;
```
**Score:** 85

---

#### Code Seg 41
```systemverilog
assign pred_s0_s1_bank_mask_din       = pred_s0_bank_mask;
```
**Score:** 90

---

#### Code Seg 42
```systemverilog
assign pred_s0_s1_set_idx_din         = pred_s0_set_idx;
```
**Score:** 80

---

#### Code Seg 43
```systemverilog
assign pred_s0_s1_pc_din              = pred_req_pc_i;
```
**Score:** 80

---

#### Code Seg 44
```systemverilog
assign pred_s0_s1_tag_din             = pred_s0_tag;
```
**Score:** 85

---

#### Code Seg 45
```systemverilog
always_ff @(posedge clk or negedge reset_n) begin
  if (!reset_n) begin
    pred_s0_s1_bank_idx_d  <= '0;
    pred_s0_s1_bank_mask_d <= '0;
    pred_s0_s1_set_idx_d   <= '0;
    pred_s0_s1_pc_d        <= '0;
    pred_s0_s1_tag_d       <= '0;
  end else if (pred_s0_s1_valid_en) begin
    pred_s0_s1_bank_idx_d  <= pred_s0_s1_bank_idx_din;
    pred_s0_s1_bank_mask_d <= pred_s0_s1_bank_mask_din;
    pred_s0_s1_set_idx_d   <= pred_s0_s1_set_idx_din;
    pred_s0_s1_pc_d        <= pred_s0_s1_pc_din;
    pred_s0_s1_tag_d       <= pred_s0_s1_tag_din;
  end
end
```
**Score:** 70

---

### Frame2: 预测阶段1 - SRAM读响应锁存

#### Code Seg 46
```systemverilog
assign pred_s1_valid      = pred_s0_s1_valid_en;
```
**Score:** 95

---

#### Code Seg 47
```systemverilog
assign pred_s1_bank_idx   = pred_s0_s1_bank_idx_d;
```
**Score:** 98

---

#### Code Seg 48
```systemverilog
assign pred_s1_bank_mask  = pred_s0_s1_bank_mask_d;
```
**Score:** 98

---

#### Code Seg 49
```systemverilog
assign pred_s1_set_idx    = pred_s0_s1_set_idx_d;
```
**Score:** 98

---

#### Code Seg 50
```systemverilog
assign pred_s1_pc         = pred_s0_s1_pc_d;
```
**Score:** 98

---

#### Code Seg 51
```systemverilog
assign pred_s1_tag        = pred_s0_s1_tag_d;
```
**Score:** 98

---

#### Code Seg 52
```systemverilog
assign s1_entry_valid[0]     = bank_read_data_i[0];
assign s1_entry_valid[1]     = bank_read_data_i[64];
assign s1_entry_valid[2]     = bank_read_data_i[128];
assign s1_entry_valid[3]     = bank_read_data_i[192];
assign s1_entry_valid[4]     = bank_read_data_i[256];
assign s1_entry_valid[5]     = bank_read_data_i[320];
assign s1_entry_valid[6]     = bank_read_data_i[384];
assign s1_entry_valid[7]     = bank_read_data_i[448];
```
**Score:** 95

---

#### Code Seg 53
```systemverilog
assign s1_entry_tag[0]  = bank_read_data_i[1 +: 24];
assign s1_entry_tag[1]  = bank_read_data_i[65 +: 24];
assign s1_entry_tag[2]  = bank_read_data_i[129 +: 24];
assign s1_entry_tag[3]  = bank_read_data_i[193 +: 24];
assign s1_entry_tag[4]  = bank_read_data_i[257 +: 24];
assign s1_entry_tag[5]  = bank_read_data_i[321 +: 24];
assign s1_entry_tag[6]  = bank_read_data_i[385 +: 24];
assign s1_entry_tag[7]  = bank_read_data_i[449 +: 24];
```
**Score:** 95

---

#### Code Seg 54
```systemverilog
assign s1_entry_position[0]  = bank_read_data_i[25];
assign s1_entry_position[1]  = bank_read_data_i[89];
assign s1_entry_position[2]  = bank_read_data_i[153];
assign s1_entry_position[3]  = bank_read_data_i[217];
assign s1_entry_position[4]  = bank_read_data_i[281];
assign s1_entry_position[5]  = bank_read_data_i[345];
assign s1_entry_position[6]  = bank_read_data_i[409];
assign s1_entry_position[7]  = bank_read_data_i[473];
```
**Score:** 95

---

#### Code Seg 55
```systemverilog
assign s1_entry_is_branch[0]  = bank_read_data_i[28];
assign s1_entry_is_branch[1]  = bank_read_data_i[92];
assign s1_entry_is_branch[2]  = bank_read_data_i[156];
assign s1_entry_is_branch[3]  = bank_read_data_i[220];
assign s1_entry_is_branch[4]  = bank_read_data_i[284];
assign s1_entry_is_branch[5]  = bank_read_data_i[348];
assign s1_entry_is_branch[6]  = bank_read_data_i[412];
assign s1_entry_is_branch[7]  = bank_read_data_i[476];
```
**Score:** 95

---

#### Code Seg 56
```systemverilog
assign s1_entry_target_low[0]  = bank_read_data_i[29 +: 22];
assign s1_entry_target_low[1]  = bank_read_data_i[93 +: 22];
assign s1_entry_target_low[2]  = bank_read_data_i[157 +: 22];
assign s1_entry_target_low[3]  = bank_read_data_i[221 +: 22];
assign s1_entry_target_low[4]  = bank_read_data_i[285 +: 22];
assign s1_entry_target_low[5]  = bank_read_data_i[349 +: 22];
assign s1_entry_target_low[6]  = bank_read_data_i[413 +: 22];
assign s1_entry_target_low[7]  = bank_read_data_i[477 +: 22];
```
**Score:** 95

---

#### Code Seg 57
```systemverilog
assign pred_s1_s2_valid_en               = pred_s1_valid;
```
**Score:** 95

---

#### Code Seg 58
```systemverilog
assign pred_s1_s2_bank_mask_din          = pred_s1_bank_mask;
```
**Score:** 98

---

#### Code Seg 59
```systemverilog
assign pred_s1_s2_set_idx_din            = pred_s1_set_idx;
```
**Score:** 98

---

#### Code Seg 60
```systemverilog
assign pred_s1_s2_pc_din                 = pred_s1_pc;
```
**Score:** 95

---

#### Code Seg 61
```systemverilog
assign pred_s1_s2_tag_din                = pred_s1_tag;
```
**Score:** 98

---

#### Code Seg 62
```systemverilog
assign pred_s1_s2_entry_valid_din        = s1_entry_valid;
```
**Score:** 95

---

#### Code Seg 63
```systemverilog
assign pred_s1_s2_entry_tag_din[0]       = s1_entry_tag[0];
assign pred_s1_s2_entry_tag_din[1]       = s1_entry_tag[1];
assign pred_s1_s2_entry_tag_din[2]       = s1_entry_tag[2];
assign pred_s1_s2_entry_tag_din[3]       = s1_entry_tag[3];
assign pred_s1_s2_entry_tag_din[4]       = s1_entry_tag[4];
assign pred_s1_s2_entry_tag_din[5]       = s1_entry_tag[5];
assign pred_s1_s2_entry_tag_din[6]       = s1_entry_tag[6];
assign pred_s1_s2_entry_tag_din[7]       = s1_entry_tag[7];
```
**Score:** 95

---

#### Code Seg 64
```systemverilog
assign pred_s1_s2_entry_position_din     = s1_entry_position;
```
**Score:** 95

---

#### Code Seg 65
```systemverilog
assign pred_s1_s2_entry_is_branch_din    = s1_entry_is_branch;
```
**Score:** 95

---

#### Code Seg 66
```systemverilog
assign pred_s1_s2_entry_target_low_din[0] = s1_entry_target_low[0];
assign pred_s1_s2_entry_target_low_din[1] = s1_entry_target_low[1];
assign pred_s1_s2_entry_target_low_din[2] = s1_entry_target_low[2];
assign pred_s1_s2_entry_target_low_din[3] = s1_entry_target_low[3];
assign pred_s1_s2_entry_target_low_din[4] = s1_entry_target_low[4];
assign pred_s1_s2_entry_target_low_din[5] = s1_entry_target_low[5];
assign pred_s1_s2_entry_target_low_din[6] = s1_entry_target_low[6];
assign pred_s1_s2_entry_target_low_din[7] = s1_entry_target_low[7];
```
**Score:** 95

---

#### Code Seg 67
```systemverilog
always_ff @(posedge clk or negedge reset_n) begin
  if (!reset_n) begin
    pred_s1_s2_bank_mask_d      <= '0;
    pred_s1_s2_set_idx_d        <= '0;
    pred_s1_s2_pc_d             <= '0;
    pred_s1_s2_tag_d            <= '0;
    pred_s1_s2_entry_valid_d    <= '0;
    pred_s1_s2_entry_tag_d[0]   <= '0;
    pred_s1_s2_entry_tag_d[1]   <= '0;
    pred_s1_s2_entry_tag_d[2]   <= '0;
    pred_s1_s2_entry_tag_d[3]   <= '0;
    pred_s1_s2_entry_tag_d[4]   <= '0;
    pred_s1_s2_entry_tag_d[5]   <= '0;
    pred_s1_s2_entry_tag_d[6]   <= '0;
    pred_s1_s2_entry_tag_d[7]   <= '0;
    pred_s1_s2_entry_position_d <= '0;
    pred_s1_s2_entry_is_branch_d<= '0;
    pred_s1_s2_entry_target_low_d[0] <= '0;
    pred_s1_s2_entry_target_low_d[1] <= '0;
    pred_s1_s2_entry_target_low_d[2] <= '0;
    pred_s1_s2_entry_target_low_d[3] <= '0;
    pred_s1_s2_entry_target_low_d[4] <= '0;
    pred_s1_s2_entry_target_low_d[5] <= '0;
    pred_s1_s2_entry_target_low_d[6] <= '0;
    pred_s1_s2_entry_target_low_d[7] <= '0;
  end else if (pred_s1_s2_valid_en) begin
    pred_s1_s2_bank_mask_d      <= pred_s1_s2_bank_mask_din;
    pred_s1_s2_set_idx_d        <= pred_s1_s2_set_idx_din;
    pred_s1_s2_pc_d             <= pred_s1_s2_pc_din;
    pred_s1_s2_tag_d            <= pred_s1_s2_tag_din;
    pred_s1_s2_entry_valid_d    <= pred_s1_s2_entry_valid_din;
    pred_s1_s2_entry_tag_d[0]   <= pred_s1_s2_entry_tag_din[0];
    pred_s1_s2_entry_tag_d[1]   <= pred_s1_s2_entry_tag_din[1];
    pred_s1_s2_entry_tag_d[2]   <= pred_s1_s2_entry_tag_din[2];
    pred_s1_s2_entry_tag_d[3]   <= pred_s1_s2_entry_tag_din[3];
    pred_s1_s2_entry_tag_d[4]   <= pred_s1_s2_entry_tag_din[4];
    pred_s1_s2_entry_tag_d[5]   <= pred_s1_s2_entry_tag_din[5];
    pred_s1_s2_entry_tag_d[6]   <= pred_s1_s2_entry_tag_din[6];
    pred_s1_s2_entry_tag_d[7]   <= pred_s1_s2_entry_tag_din[7];
    pred_s1_s2_entry_position_d <= pred_s1_s2_entry_position_din;
    pred_s1_s2_entry_is_branch_d<= pred_s1_s2_entry_is_branch_din;
    pred_s1_s2_entry_target_low_d[0] <= pred_s1_s2_entry_target_low_din[0];
    pred_s1_s2_entry_target_low_d[1] <= pred_s1_s2_entry_target_low_din[1];
    pred_s1_s2_entry_target_low_d[2] <= pred_s1_s2_entry_target_low_din[2];
    pred_s1_s2_entry_target_low_d[3] <= pred_s1_s2_entry_target_low_din[3];
    pred_s1_s2_entry_target_low_d[4] <= pred_s1_s2_entry_target_low_din[4];
    pred_s1_s2_entry_target_low_d[5] <= pred_s1_s2_entry_target_low_din[5];
    pred_s1_s2_entry_target_low_d[6] <= pred_s1_s2_entry_target_low_din[6];
    pred_s1_s2_entry_target_low_d[7] <= pred_s1_s2_entry_target_low_din[7];
  end
end
```
**Score:** 90

---

### Frame3: 预测阶段2 - 标签比较与预测输出

#### Code Seg 68
```systemverilog
assign pred_s2_valid            = pred_s1_s2_valid_en;
```
**Score:** 95

---

#### Code Seg 69
```systemverilog
assign pred_s2_bank_mask        = pred_s1_s2_bank_mask_d;
```
**Score:** 98

---

#### Code Seg 70
```systemverilog
assign pred_s2_set_idx          = pred_s1_s2_set_idx_d;
```
**Score:** 98

---

#### Code Seg 71
```systemverilog
assign pred_s2_pc               = pred_s1_s2_pc_d;
```
**Score:** 98

---

#### Code Seg 72
```systemverilog
assign pred_s2_tag              = pred_s1_s2_tag_d;
```
**Score:** 98

---

#### Code Seg 73
```systemverilog
assign pred_s2_entry_valid      = pred_s1_s2_entry_valid_d;
```
**Score:** 98

---

#### Code Seg 74
```systemverilog
assign pred_s2_entry_tag[0]     = pred_s1_s2_entry_tag_d[0];
assign pred_s2_entry_tag[1]     = pred_s1_s2_entry_tag_d[1];
assign pred_s2_entry_tag[2]     = pred_s1_s2_entry_tag_d[2];
assign pred_s2_entry_tag[3]     = pred_s1_s2_entry_tag_d[3];
assign pred_s2_entry_tag[4]     = pred_s1_s2_entry_tag_d[4];
assign pred_s2_entry_tag[5]     = pred_s1_s2_entry_tag_d[5];
assign pred_s2_entry_tag[6]     = pred_s1_s2_entry_tag_d[6];
assign pred_s2_entry_tag[7]     = pred_s1_s2_entry_tag_d[7];
```
**Score:** 98

---

#### Code Seg 75
```systemverilog
assign pred_s2_entry_position   = pred_s1_s2_entry_position_d;
```
**Score:** 98

---

#### Code Seg 76
```systemverilog
assign pred_s2_entry_is_branch  = pred_s1_s2_entry_is_branch_d;
```
**Score:** 98

---

#### Code Seg 77
```systemverilog
assign pred_s2_entry_target_low[0] = pred_s1_s2_entry_target_low_d[0];
assign pred_s2_entry_target_low[1] = pred_s1_s2_entry_target_low_d[1];
assign pred_s2_entry_target_low[2] = pred_s1_s2_entry_target_low_d[2];
assign pred_s2_entry_target_low[3] = pred_s1_s2_entry_target_low_d[3];
assign pred_s2_entry_target_low[4] = pred_s1_s2_entry_target_low_d[4];
assign pred_s2_entry_target_low[5] = pred_s1_s2_entry_target_low_d[5];
assign pred_s2_entry_target_low[6] = pred_s1_s2_entry_target_low_d[6];
assign pred_s2_entry_target_low[7] = pred_s1_s2_entry_target_low_d[7];
```
**Score:** 95

---

#### Code Seg 78
```systemverilog
assign pred_s2_hit_mask[0] = (pred_s2_entry_tag[0] == pred_s2_tag) & pred_s2_entry_valid[0];
assign pred_s2_hit_mask[1] = (pred_s2_entry_tag[1] == pred_s2_tag) & pred_s2_entry_valid[1];
assign pred_s2_hit_mask[2] = (pred_s2_entry_tag[2] == pred_s2_tag) & pred_s2_entry_valid[2];
assign pred_s2_hit_mask[3] = (pred_s2_entry_tag[3] == pred_s2_tag) & pred_s2_entry_valid[3];
assign pred_s2_hit_mask[4] = (pred_s2_entry_tag[4] == pred_s2_tag) & pred_s2_entry_valid[4];
assign pred_s2_hit_mask[5] = (pred_s2_entry_tag[5] == pred_s2_tag) & pred_s2_entry_valid[5];
assign pred_s2_hit_mask[6] = (pred_s2_entry_tag[6] == pred_s2_tag) & pred_s2_entry_valid[6];
assign pred_s2_hit_mask[7] = (pred_s2_entry_tag[7] == pred_s2_tag) & pred_s2_entry_valid[7];
```
**Score:** 90

---

#### Code Seg 79
```systemverilog
assign dir_counter_read_idx_o = {pred_s2_bank_mask, pred_s2_set_idx};
```
**Score:** 85

---

#### Code Seg 80
```systemverilog
assign pred_s2_direction[0] = dir_counter_read_data_i[1];
assign pred_s2_direction[1] = dir_counter_read_data_i[3];
assign pred_s2_direction[2] = dir_counter_read_data_i[5];
assign pred_s2_direction[3] = dir_counter_read_data_i[7];
assign pred_s2_direction[4] = dir_counter_read_data_i[9];
assign pred_s2_direction[5] = dir_counter_read_data_i[11];
assign pred_s2_direction[6] = dir_counter_read_data_i[13];
assign pred_s2_direction[7] = dir_counter_read_data_i[15];
```
**Score:** 80

---

#### Code Seg 81
```systemverilog
generate
  genvar p;
  for (p = 0; p < 8; p = p + 1) begin : gen_position_hit_mask
    assign s2_position_hit_mask[p][0] = pred_s2_hit_mask[0] & (pred_s2_entry_position[0] == pred_s2_entry_position[p]);
    assign s2_position_hit_mask[p][1] = pred_s2_hit_mask[1] & (pred_s2_entry_position[1] == pred_s2_entry_position[p]);
    assign s2_position_hit_mask[p][2] = pred_s2_hit_mask[2] & (pred_s2_entry_position[2] == pred_s2_entry_position[p]);
    assign s2_position_hit_mask[p][3] = pred_s2_hit_mask[3] & (pred_s2_entry_position[3] == pred_s2_entry_position[p]);
    assign s2_position_hit_mask[p][4] = pred_s2_hit_mask[4] & (pred_s2_entry_position[4] == pred_s2_entry_position[p]);
    assign s2_position_hit_mask[p][5] = pred_s2_hit_mask[5] & (pred_s2_entry_position[5] == pred_s2_entry_position[p]);
    assign s2_position_hit_mask[p][6] = pred_s2_hit_mask[6] & (pred_s2_entry_position[6] == pred_s2_entry_position[p]);
    assign s2_position_hit_mask[p][7] = pred_s2_hit_mask[7] & (pred_s2_entry_position[7] == pred_s2_entry_position[p]);
  end
endgenerate
```
**Score:** 75

---

#### Code Seg 82
```systemverilog
assign s2_position_0_count[0] = s2_position_hit_mask[0][1] ^ s2_position_hit_mask[0][2] ^ s2_position_hit_mask[0][4] ^ s2_position_hit_mask[0][6];
assign s2_position_0_count[1] = s2_position_hit_mask[0][2] ^ s2_position_hit_mask[0][3] ^ s2_position_hit_mask[0][6] ^ s2_position_hit_mask[0][7];
assign s2_position_0_count[2] = s2_position_hit_mask[0][4] | s2_position_hit_mask[0][5] | s2_position_hit_mask[0][6] | s2_position_hit_mask[0][7];
assign s2_multi_hit_per_position[0] = (s2_position_0_count > 3'd1);
```
**Score:** 70

---

#### Code Seg 83
```systemverilog
assign s2_position_1_count[0] = s2_position_hit_mask[1][1] ^ s2_position_hit_mask[1][2] ^ s2_position_hit_mask[1][4] ^ s2_position_hit_mask[1][6];
assign s2_position_1_count[1] = s2_position_hit_mask[1][2] ^ s2_position_hit_mask[1][3] ^ s2_position_hit_mask[1][6] ^ s2_position_hit_mask[1][7];
assign s2_position_1_count[2] = s2_position_hit_mask[1][4] | s2_position_hit_mask[1][5] | s2_position_hit_mask[1][6] | s2_position_hit_mask[1][7];
assign s2_multi_hit_per_position[1] = (s2_position_1_count > 3'd1);
```
**Score:** 70

---

#### Code Seg 84
```systemverilog
assign s2_position_2_count[0] = s2_position_hit_mask[2][1] ^ s2_position_hit_mask[2][2] ^ s2_position_hit_mask[2][4] ^ s2_position_hit_mask[2][6];
assign s2_position_2_count[1] = s2_position_hit_mask[2][2] ^ s2_position_hit_mask[2][3] ^ s2_position_hit_mask[2][6] ^ s2_position_hit_mask[2][7];
assign s2_position_2_count[2] = s2_position_hit_mask[2][4] | s2_position_hit_mask[2][5] | s2_position_hit_mask[2][6] | s2_position_hit_mask[2][7];
assign s2_multi_hit_per_position[2] = (s2_position_2_count > 3'd1);
```
**Score:** 70

---

#### Code Seg 85
```systemverilog
assign s2_position_3_count[0] = s2_position_hit_mask[3][1] ^ s2_position_hit_mask[3][2] ^ s2_position_hit_mask[3][4] ^ s2_position_hit_mask[3][6];
assign s2_position_3_count[1] = s2_position_hit_mask[3][2] ^ s2_position_hit_mask[3][3] ^ s2_position_hit_mask[3][6] ^ s2_position_hit_mask[3][7];
assign s2_position_3_count[2] = s2_position_hit_mask[3][4] | s2_position_hit_mask[3][5] | s2_position_hit_mask[3][6] | s2_position_hit_mask[3][7];
assign s2_multi_hit_per_position[3] = (s2_position_3_count > 3'd1);
```
**Score:** 70

---

#### Code Seg 86
```systemverilog
assign s2_position_4_count[0] = s2_position_hit_mask[4][1] ^ s2_position_hit_mask[4][2] ^ s2_position_hit_mask[4][4] ^ s2_position_hit_mask[4][6];
assign s2_position_4_count[1] = s2_position_hit_mask[4][2] ^ s2_position_hit_mask[4][3] ^ s2_position_hit_mask[4][6] ^ s2_position_hit_mask[4][7];
assign s2_position_4_count[2] = s2_position_hit_mask[4][4] | s2_position_hit_mask[4][5] | s2_position_hit_mask[4][6] | s2_position_hit_mask[4][7];
assign s2_multi_hit_per_position[4] = (s2_position_4_count > 3'd1);
```
**Score:** 75

---

#### Code Seg 87
```systemverilog
assign s2_position_5_count[0] = s2_position_hit_mask[5][1] ^ s2_position_hit_mask[5][2] ^ s2_position_hit_mask[5][4] ^ s2_position_hit_mask[5][6];
assign s2_position_5_count[1] = s2_position_hit_mask[5][2] ^ s2_position_hit_mask[5][3] ^ s2_position_hit_mask[5][6] ^ s2_position_hit_mask[5][7];
assign s2_position_5_count[2] = s2_position_hit_mask[5][4] | s2_position_hit_mask[5][5] | s2_position_hit_mask[5][6] | s2_position_hit_mask[5][7];
assign s2_multi_hit_per_position[5] = (s2_position_5_count > 3'd1);
```
**Score:** 75

---

#### Code Seg 88
```systemverilog
assign s2_position_6_count[0] = s2_position_hit_mask[6][1] ^ s2_position_hit_mask[6][2] ^ s2_position_hit_mask[6][4] ^ s2_position_hit_mask[6][6];
assign s2_position_6_count[1] = s2_position_hit_mask[6][2] ^ s2_position_hit_mask[6][3] ^ s2_position_hit_mask[6][6] ^ s2_position_hit_mask[6][7];
assign s2_position_6_count[2] = s2_position_hit_mask[6][4] | s2_position_hit_mask[6][5] | s2_position_hit_mask[6][6] | s2_position_hit_mask[6][7];
assign s2_multi_hit_per_position[6] = (s2_position_6_count > 3'd1);
```
**Score:** 75

---

#### Code Seg 89
```systemverilog
assign s2_position_7_count[0] = s2_position_hit_mask[7][1] ^ s2_position_hit_mask[7][2] ^ s2_position_hit_mask[7][4] ^ s2_position_hit_mask[7][6];
assign s2_position_7_count[1] = s2_position_hit_mask[7][2] ^ s2_position_hit_mask[7][3] ^ s2_position_hit_mask[7][6] ^ s2_position_hit_mask[7][7];
assign s2_position_7_count[2] = s2_position_hit_mask[7][4] | s2_position_hit_mask[7][5] | s2_position_hit_mask[7][6] | s2_position_hit_mask[7][7];
assign s2_multi_hit_per_position[7] = (s2_position_7_count > 3'd1);
```
**Score:** 75

---

#### Code Seg 90
```systemverilog
assign pred_s2_multi_hit = |s2_multi_hit_per_position;
```
**Score:** 85

---

#### Code Seg 91
```systemverilog
assign s2_multi_hit_priority_mask[0] = pred_s2_hit_mask[0];
assign s2_multi_hit_priority_mask[1] = pred_s2_hit_mask[1] & ~pred_s2_hit_mask[0];
assign s2_multi_hit_priority_mask[2] = pred_s2_hit_mask[2] & ~pred_s2_hit_mask[1] & ~pred_s2_hit_mask[0];
assign s2_multi_hit_priority_mask[3] = pred_s2_hit_mask[3] & ~pred_s2_hit_mask[2] & ~pred_s2_hit_mask[1] & ~pred_s2_hit_mask[0];
assign s2_multi_hit_priority_mask[4] = pred_s2_hit_mask[4] & ~pred_s2_hit_mask[3] & ~pred_s2_hit_mask[2] & ~pred_s2_hit_mask[1] & ~pred_s2_hit_mask[0];
assign s2_multi_hit_priority_mask[5] = pred_s2_hit_mask[5] & ~pred_s2_hit_mask[4] & ~pred_s2_hit_mask[3] & ~pred_s2_hit_mask[2] & ~pred_s2_hit_mask[1] & ~pred_s2_hit_mask[0];
assign s2_multi_hit_priority_mask[6] = pred_s2_hit_mask[6] & ~pred_s2_hit_mask[5] & ~pred_s2_hit_mask[4] & ~pred_s2_hit_mask[3] & ~pred_s2_hit_mask[2] & ~pred_s2_hit_mask[1] & ~pred_s2_hit_mask[0];
assign s2_multi_hit_priority_mask[7] = pred_s2_hit_mask[7] & ~pred_s2_hit_mask[6] & ~pred_s2_hit_mask[5] & ~pred_s2_hit_mask[4] & ~pred_s2_hit_mask[3] & ~pred_s2_hit_mask[2] & ~pred_s2_hit_mask[1] & ~pred_s2_hit_mask[0];
```
**Score:** 80

---

#### Code Seg 92
```systemverilog
assign pred_s2_multi_hit_way[0] = s2_multi_hit_priority_mask[1] | s2_multi_hit_priority_mask[3] | s2_multi_hit_priority_mask[5] | s2_multi_hit_priority_mask[7];
assign pred_s2_multi_hit_way[1] = s2_multi_hit_priority_mask[2] | s2_multi_hit_priority_mask[3] | s2_multi_hit_priority_mask[6] | s2_multi_hit_priority_mask[7];
assign pred_s2_multi_hit_way[2] = s2_multi_hit_priority_mask[4] | s2_multi_hit_priority_mask[5] | s2_multi_hit_priority_mask[6] | s2_multi_hit_priority_mask[7];
```
**Score:** 80

---

#### Code Seg 93
```systemverilog
assign s2_reconstructed_target[0*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[0]};
assign s2_reconstructed_target[1*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[1]};
assign s2_reconstructed_target[2*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[2]};
assign s2_reconstructed_target[3*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[3]};
assign s2_reconstructed_target[4*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[4]};
assign s2_reconstructed_target[5*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[5]};
assign s2_reconstructed_target[6*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[6]};
assign s2_reconstructed_target[7*`PC_WIDTH +: `PC_WIDTH] = {pred_s2_pc[45:22], pred_s2_entry_target_low[7]};
```
**Score:** 90

---

#### Code Seg 94
```systemverilog
assign pred_res_valid_o[0]      = pred_s2_hit_mask[0];
assign pred_res_valid_o[1]      = pred_s2_hit_mask[1];
assign pred_res_valid_o[2]      = pred_s2_hit_mask[2];
assign pred_res_valid_o[3]      = pred_s2_hit_mask[3];
assign pred_res_valid_o[4]      = pred_s2_hit_mask[4];
assign pred_res_valid_o[5]      = pred_s2_hit_mask[5];
assign pred_res_valid_o[6]      = pred_s2_hit_mask[6];
assign pred_res_valid_o[7]      = pred_s2_hit_mask[7];
```
**Score:** 95

---

#### Code Seg 95
```systemverilog
assign pred_res_direction_o[0]  = pred_s2_hit_mask[0] ? pred_s2_direction[0] : 1'b0;
assign pred_res_direction_o[1]  = pred_s2_hit_mask[1] ? pred_s2_direction[1] : 1'b0;
assign pred_res_direction_o[2]  = pred_s2_hit_mask[2] ? pred_s2_direction[2] : 1'b0;
assign pred_res_direction_o[3]  = pred_s2_hit_mask[3] ? pred_s2_direction[3] : 1'b0;
assign pred_res_direction_o[4]  = pred_s2_hit_mask[4] ? pred_s2_direction[4] : 1'b0;
assign pred_res_direction_o[5]  = pred_s2_hit_mask[5] ? pred_s2_direction[5] : 1'b0;
assign pred_res_direction_o[6]  = pred_s2_hit_mask[6] ? pred_s2_direction[6] : 1'b0;
assign pred_res_direction_o[7]  = pred_s2_hit_mask[7] ? pred_s2_direction[7] : 1'b0;
```
**Score:** 90

---

#### Code Seg 96
```systemverilog
assign pred_res_position_o[0]   = pred_s2_hit_mask[0] ? pred_s2_entry_position[0] : 3'b0;
assign pred_res_position_o[1]   = pred_s2_hit_mask[1] ? pred_s2_entry_position[1] : 3'b0;
assign pred_res_position_o[2]   = pred_s2_hit_mask[2] ? pred_s2_entry_position[2] : 3'b0;
assign pred_res_position_o[3]   = pred_s2_hit_mask[3] ? pred_s2_entry_position[3] : 3'b0;
assign pred_res_position_o[4]   = pred_s2_hit_mask[4] ? pred_s2_entry_position[4] : 3'b0;
assign pred_res_position_o[5]   = pred_s2_hit_mask[5] ? pred_s2_entry_position[5] : 3'b0;
assign pred_res_position_o[6]   = pred_s2_hit_mask[6] ? pred_s2_entry_position[6] : 3'b0;
assign pred_res_position_o[7]   = pred_s2_hit_mask[7] ? pred_s2_entry_position[7] : 3'b0;
```
**Score:** 90

---

#### Code Seg 97
```systemverilog
assign pred_res_is_branch_o[0]  = pred_s2_hit_mask[0] ? pred_s2_entry_is_branch[0] : 1'b0;
assign pred_res_is_branch_o[1]  = pred_s2_hit_mask[1] ? pred_s2_entry_is_branch[1] : 1'b0;
assign pred_res_is_branch_o[2]  = pred_s2_hit_mask[2] ? pred_s2_entry_is_branch[2] : 1'b0;
assign pred_res_is_branch_o[3]  = pred_s2_hit_mask[3] ? pred_s2_entry_is_branch[3] : 1'b0;
assign pred_res_is_branch_o[4]  = pred_s2_hit_mask[4] ? pred_s2_entry_is_branch[4] : 1'b0;
assign pred_res_is_branch_o[5]  = pred_s2_hit_mask[5] ? pred_s2_entry_is_branch[5] : 1'b0;
assign pred_res_is_branch_o[6]  = pred_s2_hit_mask[6] ? pred_s2_entry_is_branch[6] : 1'b0;
assign pred_res_is_branch_o[7]  = pred_s2_hit_mask[7] ? pred_s2_entry_is_branch[7] : 1'b0;
```
**Score:** 90

---

#### Code Seg 98
```systemverilog
assign pred_res_target_o[0*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[0] ? s2_reconstructed_target[0*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
assign pred_res_target_o[1*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[1] ? s2_reconstructed_target[1*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
assign pred_res_target_o[2*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[2] ? s2_reconstructed_target[2*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
assign pred_res_target_o[3*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[3] ? s2_reconstructed_target[3*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
assign pred_res_target_o[4*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[4] ? s2_reconstructed_target[4*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
assign pred_res_target_o[5*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[5] ? s2_reconstructed_target[5*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
assign pred_res_target_o[6*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[6] ? s2_reconstructed_target[6*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
assign pred_res_target_o[7*`PC_WIDTH +: `PC_WIDTH] = pred_s2_hit_mask[7] ? s2_reconstructed_target[7*`PC_WIDTH +: `PC_WIDTH] : {`PC_WIDTH{1'b0}};
```
**Score:** 90

---

#### Code Seg 99
```systemverilog
assign pred_meta_valid_o      = |pred_s2_hit_mask;
```
**Score:** 90

---

#### Code Seg 100
```systemverilog
assign pred_meta_bank_mask_o  = pred_s2_bank_mask;
```
**Score:** 95

---

#### Code Seg 101
```systemverilog
assign pred_meta_set_idx_o    = pred_s2_set_idx;
```
**Score:** 95

---

#### Code Seg 102
```systemverilog
assign pred_meta_pc_o         = pred_s2_pc;
```
**Score:** 95

---

### Frame5: 训练阶段0 - 训练请求判断

#### Code Seg 103
```systemverilog
assign train_s0_valid           = train_req_valid_i & train_req_meta_valid_i;
```
**Score:** 85

---

#### Code Seg 104
```systemverilog
assign train_s0_update_counter  = train_s0_valid;
```
**Score:** 90

---

#### Code Seg 105
```systemverilog
assign train_s0_write_entry     = train_s0_valid & train_req_final_direction_i;
```
**Score:** 80

---

#### Code Seg 106
```systemverilog
assign train_s0_invalidate      = train_s0_valid & pred_s2_multi_hit;
```
**Score:** 85

---

#### Code Seg 107
```systemverilog
assign train_s0_s1_valid_en              = train_s0_valid;
```
**Score:** 90

---

#### Code Seg 108
```systemverilog
assign train_s0_s1_pc_din                = train_req_pc_i;
```
**Score:** 90

---

#### Code Seg 109
```systemverilog
assign train_s0_s1_final_direction_din   = train_req_final_direction_i;
```
**Score:** 85

---

#### Code Seg 110
```systemverilog
assign train_s0_s1_bank_mask_din         = pred_meta_bank_mask_o;
```
**Score:** 85

---

#### Code Seg 111
```systemverilog
assign train_s0_s1_set_idx_din           = pred_meta_set_idx_o;
```
**Score:** 85

---

#### Code Seg 112
```systemverilog
assign train_s0_s1_update_counter_din    = train_s0_update_counter;
```
**Score:** 90

---

#### Code Seg 113
```systemverilog
assign train_s0_s1_write_entry_din       = train_s0_write_entry;
```
**Score:** 80

---

#### Code Seg 114
```systemverilog
assign train_s0_s1_invalidate_din        = train_s0_invalidate;
```
**Score:** 85

---

#### Code Seg 115
```systemverilog
always_ff @(posedge clk or negedge reset_n) begin
  if (!reset_n) begin
    train_s0_s1_pc_d             <= '0;
    train_s0_s1_final_direction_d<= '0;
    train_s0_s1_bank_mask_d      <= '0;
    train_s0_s1_set_idx_d        <= '0;
    train_s0_s1_update_counter_d <= '0;
    train_s0_s1_write_entry_d    <= '0;
    train_s0_s1_invalidate_d     <= '0;
  end else if (train_s0_s1_valid_en) begin
    train_s0_s1_pc_d             <= train_s0_s1_pc_din;
    train_s0_s1_final_direction_d<= train_s0_s1_final_direction_din;
    train_s0_s1_bank_mask_d      <= train_s0_s1_bank_mask_din;
    train_s0_s1_set_idx_d        <= train_s0_s1_set_idx_din;
    train_s0_s1_update_counter_d <= train_s0_s1_update_counter_din;
    train_s0_s1_write_entry_d    <= train_s0_s1_write_entry_din;
    train_s0_s1_invalidate_d     <= train_s0_s1_invalidate_din;
  end
end
```
**Score:** 85

---

### Frame6: 训练阶段1 - 计数器更新与条目写入

#### Code Seg 116
```systemverilog
assign train_s1_valid             = train_s0_s1_valid_en;
```
**Score:** 90

---

#### Code Seg 117
```systemverilog
assign train_s1_pc                = train_s0_s1_pc_d;
```
**Score:** 95

---

#### Code Seg 118
```systemverilog
assign train_s1_final_direction   = train_s0_s1_final_direction_d;
```
**Score:** 85

---

#### Code Seg 119
```systemverilog
assign train_s1_bank_mask         = train_s0_s1_bank_mask_d;
```
**Score:** 95

---

#### Code Seg 120
```systemverilog
assign train_s1_set_idx           = train_s0_s1_set_idx_d;
```
**Score:** 95

---

#### Code Seg 121
```systemverilog
assign train_s1_update_counter    = train_s0_s1_update_counter_d;
```
**Score:** 90

---

#### Code Seg 122
```systemverilog
assign train_s1_write_entry       = train_s0_s1_write_entry_d;
```
**Score:** 85

---

#### Code Seg 123
```systemverilog
assign train_s1_invalidate        = train_s0_s1_invalidate_d;
```
**Score:** 90

---

#### Code Seg 124
```systemverilog
assign train_s1_counter_write_en = train_s1_valid & (train_s1_update_counter | train_s1_write_entry);
```
**Score:** 75

---

#### Code Seg 125
```systemverilog
assign train_s1_counter_write_data = train_s1_write_entry ? 2'b10 :
                                     train_s1_final_direction ? 2'b11 : 2'b00;
```
**Score:** 70

---

#### Code Seg 126
```systemverilog
assign dir_counter_write_en_o   = train_s1_counter_write_en;
```
**Score:** 95

---

#### Code Seg 127
```systemverilog
assign dir_counter_write_idx_o  = {train_s1_bank_mask, train_s1_set_idx};
```
**Score:** 90

---

#### Code Seg 128
```systemverilog
assign dir_counter_write_data_o = train_s1_counter_write_data;
```
**Score:** 75

---

#### Code Seg 129
```systemverilog
assign train_entry_tag         = train_s1_pc[45:22];
```
**Score:** 70

---

#### Code Seg 130
```systemverilog
assign train_entry_position    = 3'b000;
```
**Score:** 45

---

#### Code Seg 131
```systemverilog
assign train_entry_is_branch   = 1'b1;
```
**Score:** 40

---

#### Code Seg 132
```systemverilog
assign train_entry_target_low  = train_s1_pc[21:0];
```
**Score:** 65

---

#### Code Seg 133
```systemverilog
assign train_entry_valid_bit   = train_s1_write_entry & ~train_s1_invalidate;
```
**Score:** 60

---

#### Code Seg 134
```systemverilog
assign train_s1_write_data = {9'b0, train_entry_target_low, train_entry_is_branch,
                               train_entry_position, train_entry_tag, train_entry_valid_bit};
```
**Score:** 55

---

#### Code Seg 135
```systemverilog
assign replacer_set_idx_o    = train_s1_set_idx;
```
**Score:** 85

---

#### Code Seg 136
```systemverilog
assign replacer_hit_way_i    = 8'b0;
```
**Score:** 60

---

#### Code Seg 137
```systemverilog
assign train_s1_write_way_mask[0] = train_s1_invalidate & ~pred_s2_multi_hit_way[0] & ~pred_s2_multi_hit_way[1] & ~pred_s2_multi_hit_way[2];
assign train_s1_write_way_mask[1] = train_s1_invalidate & pred_s2_multi_hit_way[0] & ~pred_s2_multi_hit_way[1] & ~pred_s2_multi_hit_way[2];
assign train_s1_write_way_mask[2] = train_s1_invalidate & ~pred_s2_multi_hit_way[0] & pred_s2_multi_hit_way[1] & ~pred_s2_multi_hit_way[2];
assign train_s1_write_way_mask[3] = train_s1_invalidate & pred_s2_multi_hit_way[0] & pred_s2_multi_hit_way[1] & ~pred_s2_multi_hit_way[2];
assign train_s1_write_way_mask[4] = train_s1_invalidate & ~pred_s2_multi_hit_way[0] & ~pred_s2_multi_hit_way[1] & pred_s2_multi_hit_way[2];
assign train_s1_write_way_mask[5] = train_s1_invalidate & pred_s2_multi_hit_way[0] & ~pred_s2_multi_hit_way[1] & pred_s2_multi_hit_way[2];
assign train_s1_write_way_mask[6] = train_s1_invalidate & ~pred_s2_multi_hit_way[0] & pred_s2_multi_hit_way[1] & pred_s2_multi_hit_way[2];
assign train_s1_write_way_mask[7] = train_s1_invalidate & pred_s2_multi_hit_way[0] & pred_s2_multi_hit_way[1] & pred_s2_multi_hit_way[2];
```
**Score:** 50

---

#### Code Seg 138
```systemverilog
assign write_buffer_push_en    = train_s1_valid & (train_s1_write_entry | train_s1_invalidate) & ~write_buffer_full;
```
**Score:** 55

---

#### Code Seg 139
```systemverilog
assign bank_write_req_o         = write_buffer_push_en;
```
**Score:** 70

---

#### Code Seg 140
```systemverilog
assign bank_write_set_idx_o     = train_s1_set_idx;
```
**Score:** 85

---

#### Code Seg 141
```systemverilog
assign bank_write_way_mask_o    = train_s1_write_way_mask;
```
**Score:** 55

---

#### Code Seg 142
```systemverilog
assign bank_write_data_o        = train_s1_write_data;
```
**Score:** 50

---

#### Code Seg 143
```systemverilog
assign write_buffer_full = 1'b0;
```
**Score:** 40

---

## Summary

**Total Code Segments:** 143

- **Interface Signals:** 31 (Code Seg 1-31)
- **Frame1 (预测阶段0):** 14 (Code Seg 32-45)
- **Frame2 (预测阶段1):** 22 (Code Seg 46-67)
- **Frame3 (预测阶段2):** 35 (Code Seg 68-102)
- **Frame5 (训练阶段0):** 13 (Code Seg 103-115)
- **Frame6 (训练阶段1):** 28 (Code Seg 116-143)
