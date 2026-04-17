# 三消塔防（Match-3 + Tower Defense + Rogue-like）

> 一个将三消构筑、塔防对抗与肉鸽成长融合的单局策略项目。

## 目录
- [项目概览](#项目概览)
- [核心目标与胜负条件](#核心目标与胜负条件)
- [单局流程](#单局流程)
- [系统规则](#系统规则)
- [数值配置表（当前版本）](#数值配置表当前版本)
- [Changelog](#changelog)

## 项目概览
本项目围绕三个核心环节展开：
- 三消构筑：通过交换与合成持续优化棋盘结构。
- 塔防战斗：依托当前棋盘单位构成自动进行攻防结算。
- 肉鸽成长：击杀累积权重，节点触发选牌，形成局内成长分支。
- 演示视频 BV1UpdqBtEUK
- 【1.0 test】 https://www.bilibili.com/video/BV1UpdqBtEUK/

## 核心目标与胜负条件
- 玩家目标：守住生命核心，完成全部波次并清场。
- 失败条件：核心生命值降为 0。
- 胜利条件：全部波次结束后，场上无敌军且无在途攻击体。

## 单局流程
1. 玩家交换单位，触发三消。
2. 三消结算后执行下落与补位，可能进入连锁。
3. 防御单位自动攻击，敌军推进与碰撞结算。
4. 生产格按周期提供修复或金币收益。
5. 击杀累积肉鸽权重，达到节点后暂停并选牌。
6. 波次按战斗段与暂停段循环推进，直至 Boss 波与终局。

## 系统规则

### 1) 棋盘与地块
- 棋盘为等距网格，中央玩法区为可用土地。
- 土地有耐久值；耐久归零后该格转为海洋格。
- 海洋格不可承载单位，并会触发该列重排与补位。

### 2) 三消与升级
- 仅允许相邻单位交换。
- 若交换后不形成消除，立即交换回退。
- 同类型同等级单位横向或纵向连续达到 3 个及以上可消除。
- 每 3 个可形成 1 个升级保留位。
- 同组一次消除达到 5 个或以上时，直接升 2 阶；否则升 1 阶。

### 3) 下落与补位
- 消除后先压实下落，再进行空位补充。
- 新单位斜向入场，方向为“左上到右下”或“右上到左下”随机。
- 若无可行交换步，触发基础单位刷新以恢复可玩局面。

### 4) 单位职能
- 敌军格：生成地面敌军。
- 飞行敌军格：生成飞行敌军。
- 生产格：周期性修复地块或产出金币。
- 箭塔格：默认可对空与对地。
- 炮塔格：默认对地范围输出。

### 5) Buy / Roll
- Buy：消耗金币将一个可用地块随机转化为箭塔或炮塔。
- Buy 费用按阶梯与倍率递增。
- 当次 Buy 消耗大于 1000 时，生成塔额外 +1 阶；大于 3000 时再 +1 阶。
- Roll：刷新非保护地块。
- Roll 基础消耗 5 金币，后续每次按倍率增长。
- 保护地块：等级 1~3 的箭塔、炮塔、生产格，不参与 Roll。

### 6) 塔攻击
- 箭塔：单体追踪输出，可通过成长提高有效射程与频率表现。
- 炮塔：范围伤害输出。
- 炮塔默认不可对空；获得指定肉鸽牌后可对空，但对空伤害减半。
- 塔的伤害、攻速、弹体数、分裂与范围由配置表统一驱动。

### 7) 敌军移动与碰撞
- 飞行敌军始终以核心为终点，允许横向动态摆动。
- 地面敌军优先压向可用土地，无地可走时冲击核心。
- 地面敌军撞击地块后对地块造成伤害并完成自身结算。
- Boss 接触地块后不会消失，并按每秒 5 点持续扣减接触地块耐久。
- 地块耐久归零后仍按原规则消失并转为海洋格。
- 被撞击地块会显示实时耐久反馈。

### 8) 生产格
- 按固定间隔结算。
- 优先修复受损地块；若无修复目标则转化为金币收益。
- 当前版本中生产对地块恢复效率已显著下调。

### 9) 波次与 Boss
- 波次按“战斗时段 + 暂停时段”循环。
- 随波次推进，敌军生命与数量按规则增长。
- 最终 Boss 波生成高血量巨型地面敌军。

### 10) 肉鸽成长
- 击杀按敌军类型与等阶累积权重。
- 达到节点阈值后暂停战斗并进入 3 选 1。
- 可消耗金币刷新单张牌。
- 节点上限为 15，前期固定阈值，后续按倍率增长。
- 标记“仅出现一次”的牌整局仅出现一次。

### 11) 交互与表现
- 顶部持续显示状态、金币、权重进度与核心生命。
- 右上角提供重新开始按钮及确认弹窗。
- 选牌流程带有专属特效动画。
- 可交换单位提供更强高亮与闪烁提示。
- 同类型不同等阶采用同色系分档色板，并叠加明暗变化（例如浅蓝/标准蓝/深蓝），提升三消识别效率。

## 数值配置表（当前版本）

### 全局平衡配置（balance）
| 配置项 | 当前值 | 说明 |
|---|---:|---|
| `land_hp_init` | 10.0 | 地块初始耐久 |
| `player_life_init` | 50.0 | 核心初始生命 |
| `enemy_hp_init` | 2.5 | 敌军基础生命 |
| `enemy_speed` | 34.0 | 地面敌军速度 |
| `flying_enemy_speed` | 38.0 | 飞行敌军速度 |
| `enemy_spawn_interval` | 2.0 | 刷怪间隔（秒） |
| `production_heal_interval` | 1.0 | 生产结算间隔（秒） |
| `production_heal_amount` | 0.25 | 生产基础收益 |
| `production_to_gold_ratio` | 0.25 | 生产转金币比例 |
| `enemy_collision_damage_base` | 1.0 | 飞行敌军触核基础伤害 |
| `ground_enemy_land_damage` | 1.0 | 地面敌军撞地块伤害 |
| `enemy_level_hp_damage_step` | 0.5 | 敌军每阶生命增幅步进 |
| `projectile_speed` | 340.0 | 弹体速度 |
| `projectile_hit_radius` | 12.0 | 弹体命中半径 |
| `tower_attack_interval` | 0.5 | 塔基础攻击间隔 |
| `tower_damage_per_hit` | 1.0 | 塔基础伤害项 |
| `tower_damage_flat_bonus` | 1.0 | 塔平坦增伤项 |
| `tower_hp_init` | 10.0 | 塔基础耐久项 |
| `tower_base_projectiles` | 1 | 塔基础弹体数 |
| `tower_effect_max_level` | 3 | 塔效果表最大等级 |
| `base_unit_refresh_max_attempts` | 64 | 无步可走时刷新尝试次数 |
| `swap_move_duration` | 0.18 | 交换动画时长 |
| `merge_remove_duration` | 0.16 | 消除动画时长 |
| `merge_upgrade_punch_duration` | 0.18 | 升级强调动画时长 |
| `collapse_move_duration` | 0.18 | 下落移动动画时长 |
| `spawn_pop_duration` | 0.20 | 新单位出现动画时长 |
| `cascade_settle_duration` | 0.20 | 连锁结算间隔 |
| `swap_hint_idle_seconds` | 10.0 | 提示触发空闲时长 |
| `swap_hint_fail_threshold` | 5 | 连续失败后触发提示阈值 |
| `ground_unit_name` | 海兵 | 地面敌军显示名 |
| `flying_unit_name` | 飞行兵 | 飞行敌军显示名 |
| `ground_weight_by_tier` | [2,4,7,11] | 地面敌军权重表 |
| `flying_weight_by_tier` | [3,5,9,14] | 飞行敌军权重表 |
| `gold_init` | 30 | 初始金币 |
| `gold_cost_steps` | [10,20,30,40,50] | Buy/刷新费用阶梯 |
| `roll_growth_factor` | 1.5 | Buy后续费用增长倍率 |
| `board_roll_base_cost` | 5 | Roll基础费用 |
| `board_roll_cost_growth` | 1.5 | Roll费用增长倍率 |
| `buy_level_bonus_cost_1` | 1000 | Buy额外+1阶阈值1 |
| `buy_level_bonus_cost_2` | 3000 | Buy额外+1阶阈值2 |
| `rogue_score_thresholds` | [50,100,170,300,450,800] | 肉鸽前置节点阈值 |
| `rogue_card_draw_count` | 3 | 每次抽取牌数 |
| `rogue_max_nodes` | 15 | 肉鸽节点上限 |
| `rogue_node_growth_factor` | 1.2 | 后续节点增长倍率 |
| `ground_near_flying_spawn_ratio` | 0.5 | 一半海兵改在飞行兵附近生成 |
| `ground_near_flying_left_lower_offsets` | [[0,1],[-1,1],[0,2],[-1,2]] | 海兵近飞行点偏移池 |
| `flying_lateral_range_cells` | 3 | 飞行兵横向摆动上限（格） |
| `boss_land_collision_damage` | 5.0 | Boss接触地块时每秒伤害值 |
| `boss_hp_multiplier` | 33.0 | Boss血量倍率 |

### 塔成长效果表（tower_effects）

#### 箭塔效果表
| 等级档 | 生命倍率 | 伤害倍率 | 频率倍率 | 弹体数 | 分裂 | 范围参数 |
|---|---:|---:|---:|---:|---|---:|
| 0 | 1.0 | 0.0 | 1.0 | 0 | 否 | 1 |
| 1 | 2.0 | 2.0 | 1.5 | 1 | 是 | 1 |
| 2 | 3.0 | 2.0 | 2.25 | 2 | 是 | 1 |
| 3 | 3.0 | 2.0 | 2.25 | 3 | 是 | 1 |

#### 炮塔效果表
| 等级档 | 生命倍率 | 伤害倍率 | 频率倍率 | 弹体数 | 分裂 | 范围参数 |
|---|---:|---:|---:|---:|---|---:|
| 0 | 1.0 | 1.0 | 1.0 | 1 | 否 | 1 |
| 1 | 2.0 | 2.0 | 1.5 | 1 | 否 | 2 |
| 2 | 3.0 | 2.0 | 2.25 | 2 | 否 | 2 |
| 3 | 3.0 | 2.0 | 2.25 | 3 | 否 | 4 |

### 关卡配置（level_001）
| 配置项 | 当前值 | 说明 |
|---|---:|---|
| `id` | level_001 | 关卡ID |
| `name` | core_defense_11_waves | 关卡名称 |
| `life_target_y` | 96.0 | 核心UI纵向定位 |
| `board.grid_size` | 16 | 棋盘总尺寸 |
| `board.playable_start` | 4 | 玩法区起点 |
| `board.playable_end` | 11 | 玩法区终点 |
| `board.tile_width` | 128.0 | 格子宽度 |
| `board.tile_height` | 64.0 | 格子高度 |
| `spawns.ground_cell` | [15,7] | 地面敌军出生格 |
| `spawns.flying_cell` | [7,15] | 飞行敌军出生格 |
| `spawns.flying_use_ground_chance` | 0.5 | 飞行敌军复用地面出生概率 |
| `waves.active_duration` | 20.0 | 波内战斗时长（秒） |
| `waves.pause_duration` | 5.0 | 波间暂停时长（秒） |
| `waves.boost_step` | 0.5 | 每轮成长步进 |
| `waves.total_waves` | 11 | 总波次 |
| `waves.boss_wave_index` | 11 | Boss波次编号 |
| `waves.first_wave_piece_ratio` | 0.5 | 首波数量比例 |
| `waves.spawn_growth_every_n_waves` | 2 | 出怪规模增长周期 |

### 肉鸽牌池（当前）
| 牌ID | 名称 | 效果摘要 | 限制 |
|---|---|---|---|
| `placeholder_firepower` | 待定牌A | 塔伤害 +20% | 可重复 |
| `placeholder_reload` | 待定牌B | 塔攻速 +20% | 可重复 |
| `placeholder_projectile` | 待定牌C | 弹道速度 +20% | 可重复 |
| `placeholder_life` | 待定牌D | 核心生命 +10 | 可重复 |
| `placeholder_mix` | 待定牌E | 塔伤害 +10% 且攻速 +10% | 可重复 |
| `rogue_arrow_range_plus_1` | 长弓达眼 | 所有箭塔攻击范围 +1 | 可重复 |
| `rogue_cannon_range_plus_1` | 火炮达眼 | 所有炮塔攻击范围 +1 | 可重复 |
| `rogue_cannon_anti_air_half_damage` | 高射改造 | 炮塔可对空，且对空伤害减半 | 仅一次 |
| `rogue_base_production_to_cannon` | 分工转炮 | 随机1个基础生产格变炮塔 | 可重复 |
| `rogue_production_to_arrow` | 生产转弓 | 随机1个生产格变箭塔 | 可重复 |
| `rogue_enemy_to_cannon` | 敌势炼炮 | 随机敌军或飞行敌军格变炮塔 | 可重复 |
| `rogue_enemy_to_arrow` | 敌势炼弓 | 随机敌军或飞行敌军格变箭塔 | 可重复 |

## Changelog

### 2026-04-17
- 新增视觉识别规则：同类型不同等阶采用同色系分档色板（含明显色差与明暗分层，如浅蓝/标准蓝/深蓝）。
- 调整 Boss 地块交互：Boss 接触三消地块后按每秒 5 点持续扣血，地块归零后转海洋格机制保持不变。
