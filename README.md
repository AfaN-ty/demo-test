核心体验：
在有限棋盘内通过交换与合成，持续优化阵容结构。
通过防线对抗波次敌军，保护生命核心。
通过肉鸽牌在局内形成差异化成长路线。
2. 核心目标与胜负条件
玩家目标：守住生命核心，清理全部波次敌军并击败Boss波。
失败条件：核心生命降为0。
胜利条件：全部波次结束后，场上无敌军且无在途攻击体。
3. 单局主循环
玩家阶段：交换方块，触发三消与连锁。
生成阶段：三消后补位下落，形成新局面。
战斗阶段：防御单位自动攻击，敌军推进并碰撞结算。
经济阶段：生产格周期结算为修复收益或金币收益。
成长阶段：击杀累积肉鸽权重，达到节点后暂停选牌。
波次阶段：按“战斗时段-暂停时段”推进，直至Boss波与清场。
4. 系统规则
4.1 棋盘与地块规则
棋盘为等距网格，中心玩法区为可用土地。
土地具备耐久值；耐久归零后该格转为海洋格。
海洋格不可承载单位，且会导致对应地块位移与补位重排。
4.2 三消与升级规则
仅允许交换相邻两个单位。
若交换后未形成可消除组合，则立即回退。
同类型同等级单位，横向或纵向连续达到3个及以上可消除。
消除组内每3个可形成1个升级保留位。
一次消除中若同组数量达到5个或以上，升级阶数直接提升2阶；否则提升1阶。
每次消除结算后触发下落与补位，支持连锁继续结算。
4.3 下落与补位规则
被清除后，上方单位向下压实。
新生成单位从斜向上方入场，入场方向为：
左上到右下，或右上到左下（随机）。
若棋盘无可行交换步，会触发基础单位刷新，恢复可操作局面。
4.4 单位类型与基础职能
敌军格：用于生成地面敌军。
飞行敌军格：用于生成飞行敌军。
生产格：周期性提供修复或金币收益。
箭塔格：对敌输出，默认可对空。
炮塔格：范围输出，默认对地。
4.5 Buy 与 Roll 规则
Buy：消耗金币将一个可用地块随机转化为箭塔或炮塔。
Buy费用按阶梯+倍率递增。
当次Buy花费超过1000时，生成塔额外+1阶；超过3000时再+1阶。
Roll：消耗金币刷新“非保护地块”。
Roll基础消耗为5金币，之后按倍率递增。
保护地块为：等级1~3的箭塔、炮塔、生产格；保护地块不参与Roll刷新。
4.6 塔攻击规则
箭塔：单体追踪输出，可获得额外射程成长。
炮塔：范围伤害输出，可获得额外射程成长。
炮塔默认不可对空；获得指定肉鸽牌后可对空，但对空伤害为原伤害50%。
塔攻速、伤害、弹体数量、分裂与范围表现由数值表驱动。
4.7 敌军移动与碰撞规则
飞行敌军：以核心为目标推进，允许更大左右摆动，终点目标不变。
地面敌军：优先压向可用土地；地块耗尽时直接冲击核心。
地面敌军撞击地块后，按规则扣减地块耐久并完成自身结算。
Boss撞击地块后不会消失，每次撞击固定扣减地块5点耐久。
被撞击地块会显示当前耐久反馈，便于玩家观察破坏进度。
4.8 生产格规则
生产格按固定时间间隔结算。
优先修复受损地块；若无可修复目标，则按比例转化为金币收益。
当前版本中地块修复收益已多次下调，地块恢复速度明显弱于早期版本。
4.9 波次与Boss规则
波次按“战斗持续时间 + 间歇暂停时间”循环推进。
随波次推进，敌军血量与出怪数量按规则增长。
最终Boss波出现巨型地面敌军，具备高血量与持续破坏地块能力。
4.10 肉鸽成长规则
击杀敌军按类型与等阶提供不同权重值。
达到节点阈值后，战斗暂停并进入选牌流程。
每次提供3张牌供选择，可消耗金币刷新单张。
节点上限为15；前期节点固定，后续按倍率增长。
“仅出现一次”标签的牌，整局仅能出现一次。
4.11 交互与表现规则
顶部提供状态提示、金币、权重进度与核心生命信息。
右上角提供“重新开始”入口，并带确认弹窗。
选牌流程附带过场与强调动画。
可交换方块有明显高亮与闪烁提示，降低操作盲区。
5. 数值配置表（当前版本）
5.1 全局平衡配置（balance）
配置项	当前值	说明
land_hp_init	10.0	地块初始耐久
player_life_init	50.0	核心初始生命
enemy_hp_init	2.5	敌军基础生命
enemy_speed	34.0	地面敌军速度
flying_enemy_speed	38.0	飞行敌军速度
enemy_spawn_interval	2.0	刷怪间隔（秒）
production_heal_interval	1.0	生产结算间隔（秒）
production_heal_amount	0.25	生产基础收益
production_to_gold_ratio	0.25	生产转金币比例
enemy_collision_damage_base	1.0	飞行敌军触核基础伤害
ground_enemy_land_damage	1.0	地面敌军撞地块伤害
enemy_level_hp_damage_step	0.5	敌军每阶生命增幅步进
projectile_speed	340.0	弹体速度
projectile_hit_radius	12.0	弹体命中半径
tower_attack_interval	0.5	塔基础攻击间隔
tower_damage_per_hit	1.0	塔基础伤害项
tower_damage_flat_bonus	1.0	塔平坦增伤项
tower_hp_init	10.0	塔基础耐久项
tower_base_projectiles	1	塔基础弹体数
tower_effect_max_level	3	塔效果表最大等级
base_unit_refresh_max_attempts	64	无步可走时刷新尝试次数
swap_move_duration	0.18	交换动画时长
merge_remove_duration	0.16	消除动画时长
merge_upgrade_punch_duration	0.18	升级强调动画时长
collapse_move_duration	0.18	下落移动动画时长
spawn_pop_duration	0.20	新单位出现动画时长
cascade_settle_duration	0.20	连锁结算间隔
swap_hint_idle_seconds	10.0	提示触发空闲时长
swap_hint_fail_threshold	5	连续失败后触发提示阈值
ground_unit_name	海兵	地面敌军显示名
flying_unit_name	飞行兵	飞行敌军显示名
ground_weight_by_tier	[2,4,7,11]	地面敌军权重表
flying_weight_by_tier	[3,5,9,14]	飞行敌军权重表
gold_init	30	初始金币
gold_cost_steps	[10,20,30,40,50]	Buy/刷新费用阶梯
roll_growth_factor	1.5	Buy后续费用增长倍率
board_roll_base_cost	5	Roll基础费用
board_roll_cost_growth	1.5	Roll费用增长倍率
buy_level_bonus_cost_1	1000	Buy额外+1阶阈值1
buy_level_bonus_cost_2	3000	Buy额外+1阶阈值2
rogue_score_thresholds	[50,100,170,300,450,800]	肉鸽前置节点阈值
rogue_card_draw_count	3	每次抽取牌数
rogue_max_nodes	15	肉鸽节点上限
rogue_node_growth_factor	1.2	后续节点增长倍率
ground_near_flying_spawn_ratio	0.5	一半海兵改在飞行兵附近生成
ground_near_flying_left_lower_offsets	[[0,1],[-1,1],[0,2],[-1,2]]	海兵近飞行点偏移池
flying_lateral_range_cells	3	飞行兵横向摆动上限（格）
boss_land_collision_damage	5.0	Boss撞地块固定伤害
boss_hp_multiplier	33.0	Boss血量倍率
5.2 塔成长效果表（tower_effects）
箭塔效果表
等级档	生命倍率	伤害倍率	频率倍率	弹体数	分裂	范围参数
0	1.0	0.0	1.0	0	否	1
1	2.0	2.0	1.5	1	是	1
2	3.0	2.0	2.25	2	是	1
3	3.0	2.0	2.25	3	是	1
炮塔效果表
等级档	生命倍率	伤害倍率	频率倍率	弹体数	分裂	范围参数
0	1.0	1.0	1.0	1	否	1
1	2.0	2.0	1.5	1	否	2
2	3.0	2.0	2.25	2	否	2
3	3.0	2.0	2.25	3	否	4
5.3 关卡配置（level_001）
配置项	当前值	说明
id	level_001	关卡ID
name	core_defense_11_waves	关卡名称
life_target_y	96.0	核心UI纵向定位
board.grid_size	16	棋盘总尺寸
board.playable_start	4	玩法区起点
board.playable_end	11	玩法区终点
board.tile_width	128.0	格子宽度
board.tile_height	64.0	格子高度
spawns.ground_cell	[15,7]	地面敌军出生格
spawns.flying_cell	[7,15]	飞行敌军出生格
spawns.flying_use_ground_chance	0.5	飞行敌军复用地面出生概率
waves.active_duration	20.0	波内战斗时长（秒）
waves.pause_duration	5.0	波间暂停时长（秒）
waves.boost_step	0.5	每轮成长步进
waves.total_waves	11	总波次
waves.boss_wave_index	11	Boss波次编号
waves.first_wave_piece_ratio	0.5	首波数量比例
waves.spawn_growth_every_n_waves	2	出怪规模增长周期
5.4 肉鸽牌池配置（当前）
牌ID	名称	效果摘要	限制
placeholder_firepower	待定牌A	塔伤害+20%	可重复
placeholder_reload	待定牌B	塔攻速+20%	可重复
placeholder_projectile	待定牌C	弹道速度+20%	可重复
placeholder_life	待定牌D	核心生命+10	可重复
placeholder_mix	待定牌E	塔伤害+10%且攻速+10%	可重复
rogue_arrow_range_plus_1	长弓达眼	所有箭塔攻击范围+1	可重复
rogue_cannon_range_plus_1	火炮达眼	所有炮塔攻击范围+1	可重复
rogue_cannon_anti_air_half_damage	高射改造	炮塔可对空，且对空伤害减半	仅一次
rogue_base_production_to_cannon	分工转炮	随机1个基础生产格变炮塔	可重复
rogue_production_to_arrow	生产转弓	随机1个生产格变箭塔	可重复
rogue_enemy_to_cannon	敌势炼炮	随机敌军或飞行敌军格变炮塔	可重复
rogue_enemy_to_arrow	敌势炼弓	随机敌军或飞行敌军格变箭塔	可重复
