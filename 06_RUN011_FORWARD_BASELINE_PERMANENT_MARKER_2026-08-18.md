# RUN011 正向基线永久标记（2026-08-18）

Codex 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`

本页冻结 RUN010 沉积结构第一次接入 ATLAS 正向低压求解后的真实状态。它不是正向拟合完成声明，也不是 BV/SEB 入口。

## 冻结结构与物理边界

- 材料身份始终为 BetaGa2O3；ATLAS 中的 `user.default=GaN` 只是定义 `UD1(BetaGa2O3)` 用户材料时沿用的数值模板语法，不能解释为 GaN 器件。
- Al2O3 为一次连续沉积，厚度 0.02 um。
- Gate / Gate FP 金属用 0.20 um Nickel 作二维等效；有效功函数为继承值 5.78 eV，论文未给该有效值。
- SiO2 为一次连续沉积，厚度 0.30 um；300 nm 只属于 SiO2，不是金属厚度。
- Source、Drain 与 Source FP 的 Ti/Au 总厚度均为 0.24 um 的 Gold 二维等效。
- `source_fp` 保持独立区域名并使用 `elec.id=1`，不加 `AUTOMATIC.JOIN`；导入 ATLAS 后电极 1 的显示名称为 `source_fp`，但物理上是 Source 与 Source FP 的同一端子。
- 网格没有在栅槽、场板末端或其他高场敏感边缘增加局部加密 box。

## DevEdit 到 ATLAS 的实际区域映射

DevEdit 写出 STR 后会重新编号区域。RUN011 导入 ATLAS 后共有 11 个区域：BetaGa2O3 半导体区是 `region=1..6`，Al2O3 是 `region=8`，SiO2 是 `region=10`。第一次尝试沿用了错误区号，因此在进入偏压前被主动停止；正式结果来自修正映射后的第二次运行。

## 正式运行结果

- 条件：`Wch=20 um`、`VDS=1 V`、`VGS=0..12 V`、步长 0.1 V。
- DevEdit：0 error、0 warning、2402 points、4579 triangles、0 obtuse。
- 实际最后偏压：`VDS=1 V`、`VGS=12 V`。
- 最终漏电流：`2.347091106e-5 A`，按 20 um 宽度归一化为约 `1.1735 mA/mm`。
- 最大绝对 KCL：约 `4.50e-17 A`。
- 论文阈值判据：`ID=1 mA/mm`，对 20 um 器件对应总电流 20 uA。
- RUN011 首次跨过 20 uA 的采样点为 `VGS=3.1 V`，线性插值得 `VTH≈3.06 V`；论文目标约为 9 V，因此正向尚未拟合。

## 正式远端证据

- 输入：`/root/YUE2026_EMODE_GRADED_DOPING/RUN011_DEPOSIT_IDVG_SMALL/YUE2026_RUN011_DEPOSIT_IDVG_SMALL.in`
- 最新结构：`/root/YUE2026_EMODE_GRADED_DOPING/RUN011_DEPOSIT_IDVG_SMALL/YUE2026_RUN011_DEPOSIT_IDVG_SMALL_vd1_vg12.str`
- 正式 LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN011_DEPOSIT_IDVG_SMALL/YUE2026_RUN011_DEPOSIT_IDVG_SMALL.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN011_DEPOSIT_IDVG_SMALL/YUE2026_RUN011_DEPOSIT_IDVG_SMALL.typescript`
- tmux 会话：`yue_run011_idvg_20260818`，已正常结束，无残留计算进程。

## 网页端复核所需图像

1. 已有的全器件 Mesh + Regions 图。
2. 栅槽局部 Mesh + Regions 放大图，建议视窗约为 `x=3..8 um`，纵向同时看见 Gate、20 nm Al2O3、UID 和相邻 access；必须打开网格线和材料图例。
3. `VDS=1 V, VGS=0 V` 的对数电子浓度等高线。
4. `VDS=1 V, VGS=12 V` 的对数电子浓度等高线；与第 3 张保持相同视窗和色标。
5. 可选：上述两个状态的 Conduction Band 或 Potential 等高线，也必须保持相同视窗和色标。

这些图只用于判断当前 `VTH≈3.06 V` 更可能来自网格/区域错误，还是 UID 补偿、有效功函数、固定界面电荷或界面态等未标定物理。当前阶段不得进入 BV 或 SEB，也不得仅为移动阈值而改变论文锁定几何。
