# RUN029 真实 Al2O3/BetaGa2O3 界面渐变硬停止

任务 Session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`

RUN029 是网页端批准的 VictoryMesh 最后一次 mesh-only 机会。它以 RUN027 对应的 RUN026 whole-region 设置为母版，恢复 `gate_oxide` 的旧值 `MAX.SIZE="0.10,0.0050"`，只新增一条真实 Al2O3/BetaGa2O3 异质界面的双侧窄带渐变。没有保留 RUN028 的 10 nm whole-region 横向限制。

唯一新增命令为：

```text
refine regions="USER:gate_oxide,USER:uid_channel,USER:access_left,USER:access_right" \
       interface.regions="USER:gate_oxide" \
       other.interface.regions="USER:uid_channel,USER:access_left,USER:access_right" \
       max.interface.size=0.010 \
       max.interface.size.distance=0.020 \
       max.interface.distance=0.060 \
       max.interface.distance.size=0.025
```

该选择只命中完整真实 Al2O3/BetaGa2O3 异质界面，没有触碰 Al2O3/Nickel、SiO2/金属、Gold/Nickel material-wide 接口、任何 BetaGa2O3/BetaGa2O3 同材质语义边界或局部 box/shape/edge/corner。

## Mesh-only 结果

- 45163 points
- 89908 triangles
- 2958 obtuse triangles，比例 3.29003%
- VictoryMesh 进程观测内存约 581652 KiB
- ATLAS 读入峰值约 343340 KiB
- swap 为 0
- 11 个区域、3 个电极均保持
- drain、gate、source_fp 的范围和映射保持
- 未运行 Id–Vg、BV 或 SEB

网页端预锁的点数硬停止线为 35000。RUN029 的 45163 points 明确越线，虽然 obtuse 比例为 3.29003% 并通过 5% 上限，仍必须在 mesh-only 停止，不能运行器件求解。

VictoryMesh 还给出明确警告：

```text
warning: max.interface.distance parameter should be at least three times max.interface.distance.size parameter for smooth grading.
```

本轮使用 0.060 μm 总渐变距离与 0.025 μm 末端尺寸，0.060 小于 3×0.025=0.075 μm。虽然把总距离改成约 0.080 μm 可以消除该警告，但网页端已经把 RUN029 定义为最后一个 interface-grading 单点，并规定任何一项硬停止触发后必须结束 VictoryMesh 正式基线路线。因此不得再用 0.080 μm 重跑追结果。

## 远端证据

远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_MESHONLY/`

- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_MESHONLY/YUE2026_RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_MESHONLY.in`
- DevEdit 原始 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_MESHONLY/YUE2026_RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_devedit_raw.str`
- VictoryMesh STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_MESHONLY/YUE2026_RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_vmesh.str`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_MESHONLY/YUE2026_RUN029_VICTORYMESH_AL2O3_BGO_INTERFACE_MESHONLY.typescript`

tmux 会话 `yue_run029_al2o3_bgo_interface_20260818` 已结束，没有 DeckBuild、VictoryMesh、ATLAS 或 xinternal 计算进程。自动生成的空 `deckbuild_log.txt` 已删除，远端目录只保留 `.in`、`.str`、`.typescript`。由于本轮是 mesh-only，实际偏压不适用。

## 路线裁决

VictoryMesh 的 whole-region 各向近似一致方案在 RUN028 因 7.32453% obtuse 越线；真实异质界面渐变方案在 RUN029 因 45163 points 越线。按照网页端预先锁定的停止规则，VictoryMesh 不再作为本项目正式低压基线，不再追加第二条界面规则或继续调整渐变距离。正式基线恢复为 RUN023 的 DevEdit 网格，后续物理拟合必须从 RUN023 继续。
