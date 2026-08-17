# RUN021 槽底 Qf=-2e12 近线性验收

- 日期：2026-08-18
- 任务 session ID：`01a00f0f-a888-78f3-b717-e81e4367a0c7`
- 材料身份：`BetaGa2O3`
- 阶段：正向 `VDS=1 V` 转移拟合；未运行 BV 或 SEB
- 基线：无固定电荷的 RUN012

## 独立单因素定义

```text
interface qf=-2.0e12 x.min=4.5001 x.max=6.4999 y.min=0.129 y.max=0.131
```

RUN021 输入中只有这一条 Qf，不是在 RUN020 的 `-1e12 cm^-2` 上继续叠加。作用范围仍仅是凹栅水平槽底 Al2O3/UID BetaGa2O3 界面；侧壁、access 顶面、SiO2 与其他界面均排除。Gate work function 保持 `5.78 eV`，其他全部冻结。

## 正式路径与状态

- 远端目录：`/root/YUE2026_EMODE_GRADED_DOPING/RUN021_RECESS_BOTTOM_QF_NEG2E12/`
- IN：`/root/YUE2026_EMODE_GRADED_DOPING/RUN021_RECESS_BOTTOM_QF_NEG2E12/YUE2026_RUN021_RECESS_BOTTOM_QF_NEG2E12.in`
- 最新 STR：`/root/YUE2026_EMODE_GRADED_DOPING/RUN021_RECESS_BOTTOM_QF_NEG2E12/YUE2026_RUN021_RECESS_BOTTOM_QF_NEG2E12_vd1_vg12.str`
- LOG：`/root/YUE2026_EMODE_GRADED_DOPING/RUN021_RECESS_BOTTOM_QF_NEG2E12/YUE2026_RUN021_RECESS_BOTTOM_QF_NEG2E12.log`
- TYPESCRIPT：`/root/YUE2026_EMODE_GRADED_DOPING/RUN021_RECESS_BOTTOM_QF_NEG2E12/YUE2026_RUN021_RECESS_BOTTOM_QF_NEG2E12.typescript`
- tmux：`yue_run021_bottomqf2e12_20260818`，已结束
- 进程：DeckBuild 与 `atlas.exe2` 均已退出
- 正式点数：121；最后实际偏压 `VGS=12.0 V`
- ATLAS 正常结束；计算段 `Error(s)=0`、`Warning(s)=0`
- 最大绝对 KCL：约 `2.00e-14 A`

## RUN012/RUN020/RUN021 对比

| 指标 | RUN012，Qf=0 | RUN020，-1e12 | RUN021，-2e12 |
|---|---:|---:|---:|
| 1 μA 交点 | 1.906984 V | 2.307502 V | 2.708206 V |
| 2 μA 交点 | 1.953404 V | 2.352991 V | 2.753117 V |
| 20 μA 阈值 | 3.064425 V | 3.348993 V | 3.660352 V |
| ID@12 V | 23.470911 μA | 23.460009 μA | 23.448378 μA |
| SS，1e-12..1e-8 A | 73.2966 | 73.2778 | 73.2615 mV/dec |
| SS，1e-10..1e-6 A | 81.4829 | 81.0656 | 80.6886 mV/dec |

RUN021 相对 RUN012 的 20 μA 阈值位移为 `+0.595927403 V`，是 RUN020 位移的 `2.094149` 倍。1 μA 与 2 μA 位移分别为 `+0.801222310/+0.799713389 V`，是 RUN020 对应位移的 `2.000464/2.001348` 倍。

ID12 相对 RUN012 只下降 `0.0960%`；两个 SS 相对变化约 `-0.0479%/-0.9748%`。1 pA 以上无非单调、肩峰、平台断裂或压扁。网页端要求的近倍增、多电流判据一致性、完整收敛、ID12、SS、KCL 和曲线形状均通过。

## 下一步

RUN021 未显示响应饱和或超线性失控，但下一浓度仍须独立从 RUN012 出发，只改变槽底 Qf。计划先让网页端审核 `Qf=-3e12 cm^-2`，不得把 Qf 扩展到侧壁或 access，也不同时叠加 RUN017 的功函数增量。
